---
description: CONCEPTS - ReactiveUI
---

import ReactiveCommandViewScreenshot from '/img/concepts/reactiveui/reactivecommand-view.png';
import ReactiveCommandOutputScreenshot from '/img/concepts/reactiveui/reactivecommand-output.png';
import ReactiveCommandCanExecuteScreenshot from '/img/concepts/reactiveui/reactivecommand-canexecute.png';

# Commande Réactive

Sur cette page, vous apprendrez à utiliser le `ReactiveCommand` de _ReactiveUI_ et un `ObservableObject` créé dans le code, pour mettre en œuvre le principe de fonctionnalité révélée dans l'interface utilisateur.

## Fonctionnalité Révélée

C'est un principe très important qui garantit qu'un utilisateur est correctement guidé à travers votre interface utilisateur, car les fonctionnalités et les fonctions ne deviennent disponibles (ou même visibles) qu'une fois qu'elles sont valides.

Comme exemple simple : une entrée nécessite au moins 8 caractères avant qu'un bouton puisse être cliqué, il est donc de bonne pratique de garder le bouton désactivé jusqu'à ce qu'une entrée valide ait été faite.

## Commande Réactive

Comme point de départ, vous pouvez créer une vue simple comme ceci :

```xml
<StackPanel Margin="20">
    <TextBlock Margin="0 5">Nom d'utilisateur</TextBlock>
    <TextBox Text="{Binding UserName}"/>
    <Button Margin="0 20" Command="{Binding SubmitCommand}">Soumettre</Button>
</StackPanel>
```

<img src={ReactiveCommandViewScreenshot} alt=""/>

Vous pouvez ajouter un modèle de vue correspondant comme ceci :

```csharp
public class MainWindowViewModel : ViewModelBase
{
    private string _userName = string.Empty;

    public string UserName
    {
        get { return _userName; }
        set { this.RaiseAndSetIfChanged(ref _userName, value); }
    }

    public ReactiveCommand<Unit, Unit> SubmitCommand { get; }

    public MainWindowViewModel()
    {
        SubmitCommand = ReactiveCommand.Create(() => 
        {
            Debug.WriteLine("La commande de soumission a été exécutée.");
        }); 
    }
}
```

Ce modèle de vue ne réalise pas encore la fonctionnalité révélée. La `SubmitCommand` est déclarée sans paramètre et sans résultat (void). Le paramètre d'action synchrone de la méthode `Create` est l'endroit où vous implémentez ce qui se passe lorsque la commande est exécutée (lorsque l'utilisateur clique sur le bouton). L'exemple ci-dessus se contente de signaler l'action dans la fenêtre de débogage.

<img src={ReactiveCommandOutputScreenshot} alt=""/>

## Peut Exécuter ?

Lorsque vous utilisez _ReactiveUI_ pour implémenter une fonctionnalité révélée, vous créez un objet observable qui indique si votre commande peut s'exécuter ou non.

Par exemple, vous pouvez ajouter ce code au constructeur du modèle de vue ci-dessus (`public MainWindowViewModel()`) pour créer un objet observable afin de valider le modèle de vue :

```
IObservable<bool> isInputValid = this.WhenAnyValue(
                x => x.UserName,
                x => !string.IsNullOrWhiteSpace(x) && x.Length > 7
                );
```

L'objet observable surveille la valeur de la propriété `UserName` et exécute la fonction de validation chaque fois qu'elle change. L'objet observable est créé par la fonction `WhenAnyValue` de l'`ReactiveObject` qui sous-tend le modèle de vue (voir la page précédente [ici](reactive-view-model.md)).

Ensuite, ajoutez l'objet observable à la méthode `Create`. Ce deuxième paramètre est l'argument `canExecute` pour la méthode.

```csharp
SubmitCommand = ReactiveCommand.Create(() =>
{
    Debug.WriteLine("La commande de soumission a été exécutée.");
}, isInputValid);
```

Maintenant, vous verrez que le bouton ne devient activé que lorsque vous avez saisi 8 caractères.

<img src={ReactiveCommandCanExecuteScreenshot} alt=""/>
