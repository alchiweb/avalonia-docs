---
description: GUIDES - Conversion WPF
---

# Modèles de données

Dans Avalonia UI, les modèles de données ne sont pas stockés dans les ressources de l'application. (Il en va de même pour les styles - voir [ici](styling).)

Au lieu de cela, les modèles de données sont placés soit à l'intérieur d'une collection `DataTemplates` dans un contrôle, soit à l'intérieur de (et sur `Application`) :

Par exemple, ce code ajoute un modèle de données pour afficher la classe de modèle de vue `FooViewModel` :

```xml
<UserControl xmlns:viewmodels="using:MyApp.ViewModels"
             x:DataType="viewmodels:ControlViewModel">
    <UserControl.DataTemplates>
        <DataTemplate DataType="viewmodels:FooViewModel">
            <Border Background="Red" CornerRadius="8">
                <TextBox Text="{Binding Name}"/>
            </Border>
        </DataTemplate>
    </UserControl.DataTemplates>
    <!-- En supposant que ControlViewModel.Foo est un objet de type MyApp.ViewModels.FooViewModel,
         alors une bordure rouge avec un rayon de coin de 8 contenant un TextBox sera affichée ici.
         DataType n'est requis que si vous utilisez des liaisons compilées,
         afin qu'il puisse être vérifié par type. -->
    <ContentControl Content="{Binding Foo}"/>
</UserControl>
```

Les modèles de données dans Avalonia peuvent également cibler des interfaces et des classes dérivées (ce qui ne peut pas être fait dans WPF) et donc l'ordre des `DataTemplate` peut être important : les `DataTemplate` au sein de la même collection sont évalués dans l'ordre de déclaration, vous devez donc les placer du plus spécifique au moins spécifique comme vous le feriez dans le code.

## Sélecteur de modèle de données

Dans WPF, vous pouvez créer un `DataTemplateSelector` pour sélectionner ou créer un `DataTemplate` en fonction des données fournies. Dans Avalonia, vous ne pouvez pas faire cela ; mais vous pouvez implémenter `IDataTemplate` qui peut être considéré comme un bon remplacement pour le `DataTemplateSelector`. Veuillez trouver un exemple [ici](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main/src/Avalonia.Samples/DataTemplates/IDataTemplateSample).

<XpfAd/>
