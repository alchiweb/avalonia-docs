---
id: how-to-create-a-custom-data-binding-converter
title: Comment créer un convertisseur de liaison de données personnalisé
---


# Comment créer un convertisseur de liaison de données personnalisé

Lorsque l'un des convertisseurs de liaison de données intégrés ne répond pas à vos exigences de conversion, vous pouvez écrire un convertisseur personnalisé basé sur l'interface `IValueConverter`. Ce guide vous montrera comment faire.

:::info
Pour consulter la documentation _Microsoft_ sur l'interface `IValueConverter`, voir [ici](https://docs.microsoft.com/en-gb/dotnet/api/system.windows.data.ivalueconverter?view=netframework-4.7.1).
:::

:::info
Comme l'interface `IValueConverter` n'était pas disponible dans .NET standard 2.0, Avalonia UI contient une copie dans l'espace de noms `Avalonia.Data.Converters`. Vous pouvez voir la documentation API concernant cette interface, [ici](https://reference.avaloniaui.net/api/Avalonia.Data.Converters/IValueConverter/).
:::

Vous devez référencer un convertisseur personnalisé dans certaines ressources avant qu'il puisse être utilisé. Cela peut se faire à n'importe quel niveau de votre application. Dans cet exemple, le convertisseur personnalisé `myConverter` est référencé dans les ressources de la fenêtre :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="clr-namespace:ExampleApp;assembly=ExampleApp">

  <Window.Resources>
    <local:MyConverter x:Key="myConverter"/>
  </Window.Resources>

  <TextBlock Text="{Binding Value, Converter={StaticResource myConverter}}"/>
</Window>
```

## Exemple

Ce convertisseur de liaison de données peut convertir du texte en une casse spécifique à partir d'un paramètre :

```xml
<TextBlock Text="{Binding TheContent, 
    Converter={StaticResource textCaseConverter},
    ConverterParameter=lower}" />
```

Le XAML ci-dessus suppose que le `textCaseConverter` a été référencé dans une ressource.

```csharp
public class TextCaseConverter : IValueConverter
{
    public static readonly TextCaseConverter Instance = new();

    public object? Convert(object? value, Type targetType, object? parameter, 
                                                            CultureInfo culture)
    {
        if (value is string sourceText && parameter is string targetCase
            && targetType.IsAssignableTo(typeof(string)))
        {
            switch (targetCase)
            {
                case "upper":
                case "SQL":
                    return sourceText.ToUpper();
                case "lower":
                    return sourceText.ToLower();
                case "title": // Chaque première lettre en majuscule.
                    var txtinfo = new System.Globalization.CultureInfo("en-US",false)
                                    .TextInfo;
                    return txtinfo.ToTitleCase(sourceText);
                default:
                    // option invalide, retourner l'exception ci-dessous.
                    break;
            }
        }
        // convertisseur utilisé pour le mauvais type
        return new BindingNotification(new InvalidCastException(), 
                                                BindingErrorType.Error);
    }

    public object ConvertBack(object? value, Type targetType, 
                                object? parameter, CultureInfo culture)
    {
      throw new NotSupportedException();
    }
}
```

## Type de propriété cible

Vous souhaiterez peut-être écrire un convertisseur personnalisé qui peut changer le type de sortie en fonction de ce que la propriété cible exige. Vous pouvez y parvenir car la méthode `Convert` reçoit un argument `targetType` que vous pouvez tester avec la fonction `IsAssignableTo`.

Dans cet exemple, le `animalConverter` peut trouver une image ou un nom textuel pour un objet de classe `Animal` lié : 

```xml title='XAML'
<Image Width="42" 
       Source="{Binding Animal, Converter={StaticResource animalConverter}}"/>
<TextBlock 
       Text="{Binding Animal, Converter={StaticResource animalConverter}}" />
```

```csharp title='AnimalConverter.cs'
public class AnimalConverter : IValueConverter
{
    public static readonly AnimalConverter Instance = new();

    public object? Convert( object? value, Type targetType, 
                                    object? parameter, CultureInfo culture )
    {
        if (value is Animal animal)
        {
            if (targetType.IsAssignableTo(typeof(IImage)))
            {
                img = @"icons/generic-animal-placeholder.png"
                switch (animal)
                {
                    case Dog d:
                      img = d.IsGoodBoy ? @"icons/dog-happy.png" 
                                                      : @"icons/dog.png";
                      break;
                    case Cat:
                      img = @"icons/cat.png";
                      break;
                    // etc. etc.
                }
                // voir https://docs.avaloniaui.net/docs/guides/data-binding/how-to-create-a-custom-data-binding-converter
                return BitmapAssetValueConverter.Instance
                    .Convert(img, typeof(Bitmap), parameter, culture);
            }
            else if (targetType.IsAssignableTo(typeof(string)))
            {
                return !string.IsNullOrEmpty(animal.NickName) ? 
                    $"{animal.Name} \"{animal.NickName}\"" : animal.Name;
            }
        }
        // convertisseur utilisé pour le mauvais type
        return new BindingNotification(new InvalidCastException(), 
                                                    BindingErrorType.Error);
        
    }

    public object ConvertBack( object? value, Type targetType, 
                                    object? parameter, CultureInfo culture )
    {
      throw new NotSupportedException();
    }
}
```

## FuncValueConverter et FuncMultiConverter

Vous pouvez également implémenter un `FuncValueConverter` si vous n'avez pas besoin de convertir en retour et également pas le `ConverterParameter`. Le FuncValueConverter a deux paramètres génériques :

* **TIn** : Ce paramètre définit le type d'entrée attendu. Cela peut également être un tableau si vous souhaitez utiliser ce convertisseur dans un MultiBinding.

* **TOut** : Ce paramètre définit le type de sortie attendu.

### Exemple :

```cs
public static class MyConverters 
{
    /// <summary>
    /// Obtient un convertisseur qui prend un nombre en entrée et le convertit en une représentation textuelle
    /// </summary>
    public static FuncValueConverter<decimal?, string> MyConverter { get; } = 
        new FuncValueConverter<decimal?, string> (num => $"Your number is: '{num}'");
    
    /// <summary>
    /// Obtient un convertisseur qui prend plusieurs nombres en entrée et les convertit en une représentation textuelle
    /// </summary>
    public static FuncMultiValueConverter<decimal?, string> MyMultiConverter { get; } = 
        new FuncMultiValueConverter<decimal?, string> (num => $"Vos nombres sont : '{string.Join(", ", num)}'");
}
```

```xml
<StackPanel>
    <!-- Input -->
    <NumericUpDown x:Name="Num1" Value="3" />
    <NumericUpDown x:Name="Num2" Value="3" />
    <!-- Output -->
    <TextBlock Text="{Binding #Num1.Value, Converter={x:Static my:MyConverters.MyConverter}}" />
    <TextBlock>
        <TextBlock.Text>
            <MultiBinding Converter="{x:Static my:MyConverters.MyMultiConverter}">
                <Binding Path="#Num1.Value" />
                <Binding Path="#Num2.Value" />
            </MultiBinding>
        </TextBlock.Text>
    </TextBlock>
</StackPanel>
```

## Plus d'informations

:::info
Pour des conseils supplémentaires sur la façon de lier des images, voir [ici](how-to-bind-image-files.md).
:::
