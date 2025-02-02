---
id: inotifypropertychanged
title: Comment utiliser INotifyPropertyChanged
---

## Introduction
L'interface `INotifyPropertyChanged` est un composant essentiel dans le modèle de conception Model-View-ViewModel (MVVM) qui aide à créer des applications évolutives et maintenables. En notifiant qu'une propriété a été modifiée, elle permet à la Vue de se mettre à jour automatiquement, améliorant ainsi la communication entre les composants de votre application.

## Qu'est-ce que INotifyPropertyChanged ?

`INotifyPropertyChanged` est une interface fournie par .NET qu'une classe peut implémenter pour signaler qu'une propriété a changé de valeur. Cela est particulièrement utile dans les scénarios de liaison de données, où une mise à jour automatique de l'interface utilisateur peut être déclenchée une fois que les données auxquelles elle est liée changent.

L'interface `INotifyPropertyChanged` a un membre d'événement, `PropertyChanged`. Lorsqu'une valeur de propriété est modifiée, l'objet déclenche un événement `PropertyChanged` pour notifier tout élément lié que la propriété a changé.

## Pourquoi INotifyPropertyChanged est-il important dans MVVM ?
Dans le modèle MVVM, le ViewModel encapsule la logique d'interaction pour la Vue et encapsule les données du Modèle. La Vue se lie aux propriétés du ViewModel, qui expose à son tour les données contenues dans les objets Modèle.

Pour que le modèle MVVM fonctionne comme prévu, la Vue doit être mise à jour chaque fois que les données sous-jacentes changent. C'est là qu'intervient `INotifyPropertyChanged`. En implémentant cette interface dans votre ViewModel, vous pouvez notifier la Vue des changements dans le Modèle, ce qui met automatiquement à jour l'interface utilisateur.

## Implémentation de INotifyPropertyChanged
Voici un exemple de la façon d'implémenter `INotifyPropertyChanged` :

```csharp
public class MyViewModel : INotifyPropertyChanged
{
    private string _name;

    public string Name
    {
        get { return _name; }
        set
        {
            _name = value;
            OnPropertyChanged(nameof(Name));
        }
    }

    public event PropertyChangedEventHandler PropertyChanged;

    protected virtual void OnPropertyChanged(string propertyName)
    {
        PropertyChanged?.Invoke(this, new PropertyChangedEventArgs(propertyName));
    }
}
```

Dans ce code, chaque fois que la propriété `Name` est définie sur une nouvelle valeur, la méthode `OnPropertyChanged` est appelée, ce qui déclenche l'événement `PropertyChanged`. Tous les éléments de l'interface utilisateur liés à cette propriété se mettront alors à jour pour refléter la nouvelle valeur.

## Utilisation du MVVM Toolkit pour simplifier INotifyPropertyChanged
Bien que l'implémentation de `INotifyPropertyChanged` ne soit pas particulièrement complexe, elle peut devenir fastidieuse si vous avez de nombreuses propriétés dans votre ViewModel. Heureusement, la bibliothèque MVVM du .NET Community Toolkit propose une méthode encore plus efficace pour implémenter `INotifyPropertyChanged` en utilisant sa classe `ObservableObject` et l'attribut `[ObservableProperty]` avec l'aide des générateurs de code.

Voici comment vous pouvez obtenir le même résultat qu'auparavant, mais en utilisant `ObservableObject` :

```csharp
using CommunityToolkit.Mvvm.ComponentModel;

public partial class MyViewModel : ObservableObject
{
    [ObservableProperty]
    private string _name;
}
```

Dans ce code, la classe `ObservableObject` implémente `INotifyPropertyChanged`, et l'attribut `[ObservableProperty]` est utilisé pour indiquer que `_name` est une propriété observable. Le générateur de code générera alors le code d'accompagnement nécessaire en arrière-plan, y compris le getter et le setter de la propriété, et appellera automatiquement la méthode `OnPropertyChanged` lorsque la propriété change. Cela rend l'implémentation plus propre et moins sujette aux erreurs.

Le MVVM Toolkit fournit une gamme d'outils pour aider à simplifier l'implémentation du modèle MVVM dans vos applications .NET, y compris la simplification de l'utilisation de `INotifyPropertyChanged`. L'utilisation des générateurs de source rend votre code plus efficace et lisible, tout en maintenant la même fonctionnalité.
