---
id: data-validation
title: Validation des données
---

# Validation des données

Avalonia offre différentes options de validation des données. Dans cette section, nous vous montrerons comment vous pouvez valider les `Propriétés` de votre `ViewModel` et comment vous pouvez styliser le message d'erreur affiché.

## Validation d'une propriété

Avalonia utilise [`DataValidationPlugins`](http://reference.avaloniaui.net/api/Avalonia.Data.Core.Plugins/IDataValidationPlugin/) pour valider les `Propriétés` auxquelles vous êtes lié. Par défaut, Avalonia fournit ces trois plugins de validation :

* [DataAnnotations - ValidationPlugin](data-validation.md#dataannotations---validationplugin)
* [INotifyDataErrorInfo - ValidationPlugin](data-validation.md#inotifydataerrorinfo---validationplugin)
* [Exception - ValidationPlugin](data-validation.md#exception---validationplugin)

### DataAnnotations - ValidationPlugin

Vous pouvez décorer les `Propriétés` de votre `ViewModel` avec différents [`Attributs de Validation`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.validationattribute). Vous pouvez utiliser ceux intégrés, utiliser le [`CustomValidationAttribute`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.dataannotations.customvalidationattribute) ou créer le vôtre en dérivant de `ValidationAttribute`.

**Exemple : La propriété EMail est requise et doit être une adresse e-mail valide**

```cs
[Required]
[EmailAddress]
public string? EMail
{
    get { return _EMail; }
    set { this.RaiseAndSetIfChanged(ref _EMail, value); }
}
```

### INotifyDataErrorInfo - ValidationPlugin

Avalonia prend également en charge la validation des classes qui implémentent [`INotifyDataErrorInfo`](https://learn.microsoft.com/en-us/dotnet/api/system.componentmodel.inotifydataerrorinfo). Plusieurs bibliothèques `MVVM` utilisent cette interface pour leur validation de données, par exemple le package [CommunityToolkit.Mvvm](https://learn.microsoft.com/en-us/windows/communitytoolkit/mvvm/observablevalidator) et le package [`ReactiveUI.Validation`](https://github.com/reactiveui/ReactiveUI.Validation#inotifydataerrorinfo-support). Pour des instructions d'utilisation, veuillez consulter la documentation du package `MVVM` de votre choix.

:::info
Certaines bibliothèques comme `CommunityToolkit.Mvvm` utilisent `DataAnnotations` pour leur validation. Cela peut entraîner des conflits avec le [DataAnnotations - ValidationPlugin](data-validation.md#dataannotations---validationplugin). Veuillez consulter [Gérer les ValidationPlugins](data-validation.md#manage-validationplugins) pour résoudre ce problème.
:::

### Exception - ValidationPlugin

Une autre option pour valider une propriété est de lever une [`Exception`](https://learn.microsoft.com/en-us/dotnet/csharp/fundamentals/exceptions/creating-and-throwing-exceptions) à l'intérieur du setter de votre propriété.

**Exemple : Valider la propriété EMail en utilisant des `Exceptions`**

```cs
public string? EMail
{
    get { return _EMail; }
    set 
    {
        if (string.IsNullOrEmpty(value))
        {
            throw new ArgumentNullException(nameof(EMail), "Ce champ est requis");
        }
        else if (!value.Contains('@'))
        {
            throw new ArgumentException(nameof(EMail), "Pas une adresse e-mail valide");
        }
        else
        { 
            this.RaiseAndSetIfChanged(ref _EMail, value); 
        } 
    }
}
```

:::danger
Les exceptions à l'intérieur du getter de votre propriété ne sont pas autorisées et entraîneront un plantage de votre application.
:::

## Personnaliser l'apparence du message de validation

Pour afficher les messages de validation, Avalonia dispose d'un contrôle appelé [`DataValidationErrors`](http://reference.avaloniaui.net/api/Avalonia.Controls/DataValidationErrors/). Ce contrôle est généralement placé à l'intérieur du `ControlTemplate` de tous les `Controls` qui prennent en charge la validation des données, comme `TextBox`, `Slider` et autres. Vous pouvez créer votre propre `Style` pour le contrôle `DataValidationErrors` afin de personnaliser la représentation des messages d'erreur.

**Exemple de style pour DataValidationErrors**

```xml
<Style Selector="DataValidationErrors">
  <Setter Property="Template">
    <ControlTemplate>
      <DockPanel LastChildFill="True">
        <ContentControl DockPanel.Dock="Right"
                        ContentTemplate="{TemplateBinding ErrorTemplate}"
                        DataContext="{TemplateBinding Owner}"
                        Content="{Binding (DataValidationErrors.Errors)}"
                        IsVisible="{Binding (DataValidationErrors.HasErrors)}"/>
        <ContentPresenter Name="PART_ContentPresenter"
                          Background="{TemplateBinding Background}"
                          BorderBrush="{TemplateBinding BorderBrush}"
                          BorderThickness="{TemplateBinding BorderThickness}"
                          CornerRadius="{TemplateBinding CornerRadius}"
                          ContentTemplate="{TemplateBinding ContentTemplate}"
                          Content="{TemplateBinding Content}"
                          Padding="{TemplateBinding Padding}"/>
      </DockPanel>
    </ControlTemplate>
  </Setter>
  <Setter Property="ErrorTemplate">
    <DataTemplate x:DataType="{x:Type x:Object}">
      <Canvas Width="14" Height="14" Margin="4 0 1 0" 
              Background="Transparent">
        <Canvas.Styles>
          <Style Selector="ToolTip">
            <Setter Property="Background" Value="LightRed"/>
            <Setter Property="BorderBrush" Value="Red"/>
          </Style>
        </Canvas.Styles>
        <ToolTip.Tip>
          <ItemsControl ItemsSource="{Binding}"/>
        </ToolTip.Tip>
        <Path Data="M14,7 A7,7 0 0,0 0,7 M0,7 A7,7 0 1,0 14,7 M7,3l0,5 M7,9l0,2" 
              Stroke="Red" 
              StrokeThickness="2"/>
      </Canvas>
    </DataTemplate>
  </Setter>
</Style>
```

<!-- ![custom validation style](broken-reference) -->

**Style de validation personnalisé**

## Gérer les ValidationPlugins

Si nécessaire, vous pouvez activer ou désactiver un `ValidationPlugin` spécifique dans votre application. Cela peut être utile si, par exemple, votre framework MVVM utilise `DataAnnotations` pour valider la propriété via `INotifyDataErrorInfo`. Dans ce cas, vous verriez le message deux fois. Utilisez la collection `BindingPlugins.DataValidators` pour ajouter ou supprimer un `ValidationPlugin` spécifique.

**Exemple : Supprimer le validateur DataAnnotations**

```cs
public override void OnFrameworkInitializationCompleted()
{
    // Obtenir un tableau de plugins à supprimer
    var dataValidationPluginsToRemove =
        BindingPlugins.DataValidators.OfType<DataAnnotationsValidationPlugin>().ToArray();

    // supprimer chaque entrée trouvée
    foreach (var plugin in dataValidationPluginsToRemove)
    {
        BindingPlugins.DataValidators.Remove(plugin);
    }

    // Continuer avec le démarrage normal
    if (ApplicationLifetime is IClassicDesktopStyleApplicationLifetime desktop)
    {
        desktop.MainWindow = new MainWindow()
        {
            DataContext = MainWindowViewModel.Instance
        };
    }

    base.OnFrameworkInitializationCompleted();
}
```
