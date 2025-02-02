---
id: how-to-bind-can-execute
title: Comment lier Can Execute
---

import BindCanExecuteScreenshot from '/img/guides/data-binding/bind-canexecute.gif';

# Comment lier Can Execute

Que le contrôle, qui peut initier une action en réponse à l'interaction de l'utilisateur, soit dans son état activé, est un principe important de la partie 'fonctionnalité révélée' de la conception de l'expérience utilisateur (UX). La confiance de l'utilisateur est renforcée en désactivant les commandes qui ne peuvent pas s'exécuter. Par exemple, lorsqu'un bouton ou un élément de menu ne peut pas s'exécuter en raison de l'état actuel d'une application, il doit être présenté comme inactif.

Cet exemple suppose que vous utilisez le modèle d'implémentation MVVM avec le framework _ReactiveUI_. Cette approche (recommandée) offre une séparation très claire entre la vue et le modèle de vue.

Dans cet exemple, le bouton ne peut être cliqué que lorsque le message n'est pas vide. Dès que la sortie est affichée ; le message est réinitialisé à la chaîne vide - ce qui désactivera à nouveau le bouton.

```xml title='XAML'
<StackPanel Margin="20">
  <TextBox Margin="0 5" Text="{Binding Message}"
           Watermark="Ajouter un message pour activer le bouton"/>
  <Button Command="{Binding ExampleCommand}">    
    Run the example
  </Button>
  <TextBlock Margin="0 5" Text="{Binding Output}" />
</StackPanel>
```

```csharp title='MainWindowViewModel.cs'
namespace AvaloniaGuides.ViewModels
{
    public class MainWindowViewModel : ViewModelBase
    {
        private string _message = string.Empty;
        private string _output = "En attente...";

        public string Message 
        { 
            get => _message; 
            set => this.RaiseAndSetIfChanged(ref _message, value); 
        }

        public string Output
        {
            get => _output;
            set => this.RaiseAndSetIfChanged(ref _output, value);
        }

        public ReactiveCommand<Unit, Unit> ExampleCommand { get; }

        public MainWindowViewModel()
        {
            var isValidObservable = this.WhenAnyValue(
                x => x.Message,
                x => !string.IsNullOrWhiteSpace(x));
            ExampleCommand = ReactiveCommand.Create(PerformAction, 
                                                    isValidObservable);
        }

        private void PerformAction()
        {
             Output = $"L'action a été appelée. {_message}";
             Message = String.Empty;
        }
    }
}
```

```csharp title='ViewModelBase.cs'
using ReactiveUI;

namespace AvaloniaGuides.ViewModels
{
    public class ViewModelBase : ReactiveObject
    {
    }
}
```

Dans le constructeur du modèle de vue, la commande réactive est créée avec deux paramètres. Le premier est la méthode privée qui effectue l'action. Le second est un observable qui est créé par la méthode `WhenAnyValue` de l'objet `ReactiveObject` qui sous-tend le modèle de vue (de la classe `ViewModelBase`).

:::info
La classe `ViewModelBase` est ajoutée à votre projet lorsque vous utilisez le modèle de solution 'Application MVVM Avalonia'.
:::

Ici, la méthode `WhenAnyValue` prend deux arguments, le premier collecte une valeur pour le paramètre de la fonction de validation, et le second est la fonction de validation qui retourne un résultat booléen.

:::info
La méthode `WhenAnyValue` a en fait des surcharges qui peuvent prendre jusqu'à 10 récupérateurs de valeur différents (pour les paramètres de la fonction de validation), plus la fonction de validation elle-même.
:::

<img src={BindCanExecuteScreenshot} alt=""/>
