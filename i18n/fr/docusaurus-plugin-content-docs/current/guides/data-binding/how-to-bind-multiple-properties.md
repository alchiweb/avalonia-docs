---
id: how-to-bind-multiple-properties
title: Comment lier plusieurs propriétés
---

import MultiBindingRgbScreenshot from '/img/guides/data-binding/multibinding-rgb.gif';

## MultiBinding

Dans les scénarios où une propriété cible doit être assignée à un résultat calculé à partir de plusieurs autres propriétés liées, un `MultiBinding` peut être la solution appropriée. `MultiBinding` agrège plusieurs objets `Binding` et produit un résultat à l'aide d'un `IMultiValueConverter`. La méthode `Convert` est appelée chaque fois que l'une des propriétés liées notifie un changement. Comme pour `Binding`, `MultiBinding` peut être utilisé pour lier des propriétés sur des ViewModels, des `Control`s ou d'autres sources.

:::warning
`MultiBinding` ne prend en charge que `BindingMode.OneTime` et `BindingMode.OneWay`.
:::

## IMultiValueConverter

Semblable à `IValueConverter` en ce sens qu'il définit des conversions vers une propriété cible. Il n'y a pas de méthode `ConvertBack` car les opérations agrégées sont irréversibles.

```csharp
public interface IMultiValueConverter
{
    object? Convert(IList<object?> values, Type targetType, object? parameter, CultureInfo culture);
}
```

## Exemple de MultiBinding

Considérez le scénario suivant où vous avez des entrées pour les canaux de couleur rouge, vert et bleu. L'objectif est de lier les 3 entrées et de fournir un `IBrush` pour qu'un autre contrôle puisse dessiner. Ci-dessous, les valeurs des canaux de couleur sont contraintes à la plage appropriée ([0, 255]) par le `NumericUpDown`. La création d'objets `<Binding>` est nécessaire car l'`Extension de Marquage` `Binding` ne peut pas être utilisée car il n'y a pas de propriétés à cibler.

```xml
<StackPanel HorizontalAlignment="Center" VerticalAlignment="Center" Spacing="8">
    <NumericUpDown x:Name="red" Minimum="0" Maximum="255" Increment="20" Value="0" Foreground="Red" />
    <NumericUpDown x:Name="green" Minimum="0" Maximum="255" Increment="20" Value="0" Foreground="Green" />
    <NumericUpDown x:Name="blue" Minimum="0" Maximum="255" Increment="20" Value="0" Foreground="Blue" />

    <TextBlock Text="MultiBinding Text Color!" FontSize="24">
        <TextBlock.Foreground>
            <MultiBinding Converter="{StaticResource RgbToBrushMultiConverter}">
                <Binding Path="Value" ElementName="red" />
                <Binding Path="Value" ElementName="green" />
                <Binding Path="Value" ElementName="blue" />
            </MultiBinding>
        </TextBlock.Foreground>
    </TextBlock>
</StackPanel>
```

Ensuite, nous créons l'`IMultiValueConverter`. La vérification des types des paramètres est importante. Dans ce scénario, `NumericUpDown.Value` est un `decimal?` donc à la fois `decimal` et `null` doivent être vérifiés. La valeur peut également être `UnsetValueType` lorsque les liaisons sont en cours d'initialisation. Une conversion numérique supplémentaire pourrait être effectuée pour rendre le convertisseur largement compatible avec les types numériques.

```csharp title='Implémentation du Convertisseur'
public sealed class RgbToBrushMultiConverter : IMultiValueConverter
{
    public object? Convert(IList<object?> values, Type targetType, object? parameter, CultureInfo culture)
    {
        // Assurez-vous que toutes les liaisons sont fournies et attachées au bon type cible
        if (values?.Count != 3 || !targetType.IsAssignableFrom(typeof(ImmutableSolidColorBrush)))
            throw new NotSupportedException();

        // Assurez-vous que toutes les liaisons sont du bon type
        if (!values.All(x => x is decimal or UnsetValueType or null))
            throw new NotSupportedException();

        // Récupérez les valeurs, ne rien faire si l'une d'elles est non définie.
        // Convert est appelé plusieurs fois lors de l'initialisation des liaisons,
        // donc certaines propriétés seront initialement non définies.
        if (values[0] is not decimal r ||
            values[1] is not decimal g ||
            values[2] is not decimal b)
            return BindingOperations.DoNothing;

        byte a = 255;
        var color = new Color(a, (byte)r, (byte)g, (byte)b);
        return new ImmutableSolidColorBrush(color);
    }
}
```

<img src={MultiBindingRgbScreenshot} alt=''/>

:::tip
* Envisagez de créer une `MarkupExtension` pour simplifier la syntaxe XAML lorsque `MultiBinding` est fréquemment réutilisé.
* Envisagez d'utiliser `FuncMultiValueConverter` pour réduire la quantité de code nécessaire pour des convertisseurs plus simples.
:::
