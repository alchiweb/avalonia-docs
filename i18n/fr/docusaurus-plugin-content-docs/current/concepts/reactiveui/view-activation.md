---
description: CONCEPTS - ReactiveUI
---

# Activation de la Vue

Pour que la fonctionnalité [WhenActivated](https://reactiveui.net/docs/handbook/when-activated/) de ReactiveUI fonctionne, vous devez utiliser des classes de base personnalisées du package `Avalonia.ReactiveUI`, telles que `ReactiveWindow<TViewModel>` ou `ReactiveUserControl<TViewModel>`. Bien sûr, vous pouvez également implémenter l'interface `IViewFor<TViewModel>` manuellement dans votre classe, mais assurez-vous de stocker le `ViewModel` dans un `AvaloniaProperty`.

### Exemple d'Activation

**ViewModel.cs**

Ce modèle de vue implémente l'interface `IActivatableViewModel`. Lorsque la vue correspondante est attachée à l'arbre visuel, le code à l'intérieur du bloc WhenActivated sera appelé. Lorsque la vue correspondante est détachée de l'arbre visuel, le composite disposable sera éliminé. `ReactiveObject` est la classe de base pour les [classes de modèle de vue](https://reactiveui.net/docs/handbook/view-models/), et elle implémente `INotifyPropertyChanged`.

```csharp
public class ViewModel : ReactiveObject, IActivatableViewModel
{
    public ViewModelActivator Activator { get; }

    public ViewModel()
    {
        Activator = new ViewModelActivator();
        this.WhenActivated((CompositeDisposable disposables) =>
        {
            /* activation de l'handle */
            Disposable
                .Create(() => { /* désactivation de l'handle */ })
                .DisposeWith(disposables);
        });
    }
}
```

**View.xaml**

Voici l'interface utilisateur pour le modèle de vue que vous voyez ci-dessus.

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        Background="#f0f0f0" FontFamily="Ubuntu"
        MinHeight="590" MinWidth="750">
  <TextBlock Text="Hello, world!" />
</Window>
```

**View.xaml.cs**

Voici le code-behind pour le fichier `View.xaml` que vous voyez ci-dessus. N'oubliez pas de toujours inclure un appel à `WhenActivated` dans le constructeur de votre Vue, sinon ReactiveUI ne pourra pas déterminer quand le modèle de vue est activé.

```csharp
public class View : ReactiveWindow<ViewModel>
{
    public View()
    {
        // Le bloc WhenActivated du ViewModel sera également appelé.
        this.WhenActivated(disposables => { /* Gérer l'activation de la vue, etc. */ });
        AvaloniaXamlLoader.Load(this);
    }
}
```

### Code-Behind Bindings ReactiveUI

```csharp
public class View : ReactiveWindow<ViewModel>
{
    // Supposons que le contrôle Button ait l'attribut Name="ExampleButton" défini dans XAML.
    public View()
    {
        this.WhenActivated(disposables => 
        {
            // Lier la 'ExampleCommand' au 'ExampleButton' défini ci-dessus.
            this.BindCommand(ViewModel, x => x.ExampleCommand, x => x.ExampleButton)
                .DisposeWith(disposables);
        });
        AvaloniaXamlLoader.Load(this);
    }
}
```
