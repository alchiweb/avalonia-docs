---
id: binding-from-code
title: Comment Lier depuis le Code
---


# Comment Lier depuis le Code

La liaison depuis le code dans Avalonia fonctionne quelque peu différemment de WPF/UWP. À un niveau bas, le système de liaison d'Avalonia est basé sur `IObservable` des Extensions Réactives, qui est ensuite construit par des liaisons XAML (qui peuvent également être instanciées dans le code).

## S'abonner aux Changements d'une Propriété

Vous pouvez vous abonner aux changements d'une propriété en appelant la méthode `GetObservable`. Cela renvoie un `IObservable<T>` qui peut être utilisé pour écouter les changements de la propriété :

```csharp
var textBlock = new TextBlock();
var text = textBlock.GetObservable(TextBlock.TextProperty);
```

Chaque propriété à laquelle on peut s'abonner a un champ statique en lecture seule appelé `[PropertyName]Property` qui est passé à `GetObservable` afin de s'abonner aux changements de la propriété.

`IObservable` (partie des Extensions Réactives, ou rx pour faire court) est hors du champ d'application de ce guide, mais voici un exemple qui utilise l'observable retourné pour imprimer un message avec les valeurs de propriété changeantes dans la console :

```csharp
var textBlock = new TextBlock();
var text = textBlock.GetObservable(TextBlock.TextProperty);
text.Subscribe(value => Console.WriteLine(value + " a changé"));
```

Lorsque l'observable retourné est souscrit, il renverra immédiatement la valeur actuelle de la propriété et poussera ensuite une nouvelle valeur chaque fois que la propriété change. Si vous ne souhaitez pas la valeur actuelle, vous pouvez utiliser l'opérateur `Skip` de rx :

```csharp
var text = textBlock.GetObservable(TextBlock.TextProperty).Skip(1);
```

## Liaison à un observable

Vous pouvez lier une propriété à un observable en utilisant la méthode `AvaloniaObject.Bind` :

```csharp
// Nous utilisons un Sujet Rx ici afin de pouvoir pousser de nouvelles valeurs en utilisant OnNext
var source = new Subject<string>();
var textBlock = new TextBlock();

// Lier TextBlock.Text à source
var subscription = textBlock.Bind(TextBlock.TextProperty, source);

// Définir textBlock.Text sur "hello"
source.OnNext("hello");
// Définir textBlock.Text sur "world!"
source.OnNext("world!");

// Terminer la liaison
subscription.Dispose();
```

Remarquez que la méthode `Bind` retourne un `IDisposable` qui peut être utilisé pour terminer la liaison. Si vous n'appelez jamais cela, alors la liaison se terminera automatiquement lorsque l'observable se terminera via `OnCompleted` ou `OnError`.

## Définir une liaison dans un initialiseur d'objet

Il est souvent utile de configurer des liaisons dans les initialisateurs d'objet. Vous pouvez le faire en utilisant l'indexeur :

```csharp
var source = new Subject<string>();
var textBlock = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = source.ToBinding(),
};
```

En utilisant cette méthode, vous pouvez également facilement lier une propriété d'un contrôle à une propriété d'un autre :

```csharp
var textBlock1 = new TextBlock();
var textBlock2 = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = textBlock1[!TextBlock.TextProperty],
};
```

Bien sûr, l'indexeur peut également être utilisé en dehors des initialisateurs d'objet :

```csharp
textBlock2[!TextBlock.TextProperty] = textBlock1[!TextBlock.TextProperty];
```

Le seul inconvénient de cette syntaxe est qu'aucun `IDisposable` n'est retourné. Si vous devez terminer manuellement la liaison, vous devez utiliser la méthode `Bind`.

## Transformation des valeurs de liaison

Parce que nous travaillons avec des observables, nous pouvons facilement transformer les valeurs que nous lions !

```csharp
var source = new Subject<string>();
var textBlock = new TextBlock
{
    Foreground = Brushes.Red,
    MaxWidth = 200,
    [!TextBlock.TextProperty] = source.Select(x => "Hello " + x).ToBinding(),
};
```

## Utilisation des liaisons XAML depuis le code

Parfois, lorsque vous voulez les fonctionnalités supplémentaires que les liaisons XAML fournissent, il est plus facile d'utiliser les liaisons XAML depuis le code. Par exemple, en utilisant uniquement des observables, vous pourriez lier à une propriété sur `DataContext` comme ceci :

```csharp
var textBlock = new TextBlock();
var viewModelProperty = textBlock.GetObservable(TextBlock.DataContextProperty)
    .OfType<MyViewModel>()
    .Select(x => x?.Name);
textBlock.Bind(TextBlock.TextProperty, viewModelProperty);
```

Cependant, il pourrait être préférable d'utiliser une liaison XAML dans ce cas :

```csharp
var textBlock = new TextBlock
{
    [!TextBlock.TextProperty] = new Binding("Name")
};
```

Ou, si vous avez besoin d'un `IDisposable` pour terminer la liaison :

```csharp
var textBlock = new TextBlock();
var subscription = textBlock.Bind(TextBlock.TextProperty, new Binding("Name"));

subscription.Dispose();
```

## Souscrire à une propriété sur n'importe quel objet

La méthode `GetObservable` retourne un observable qui suit les changements d'une propriété sur une seule instance. Cependant, si vous écrivez un contrôle, vous pourriez vouloir implémenter une méthode `OnPropertyChanged` qui n'est pas liée à une instance d'un objet.

Pour ce faire, vous pouvez vous abonner à [`AvaloniaProperty.Changed`](http://reference.avaloniaui.net/api/Avalonia/AvaloniaProperty/65237C52) qui est un observable qui se déclenche _à chaque fois que la propriété est changée sur n'importe quelle instance_.

> Dans WPF, cela se fait en passant un `PropertyChangedCallback` statique à la méthode d'enregistrement de `DependencyProperty`, mais cela ne permet qu'à l'auteur du contrôle d'enregistrer un rappel de changement de propriété.

De plus, il existe une méthode d'extension `AddClassHandler` qui peut automatiquement router l'événement vers une méthode de votre contrôle.

Par exemple, si vous souhaitez écouter les changements de la propriété `Foo` de votre contrôle, vous le feriez comme ceci :

```csharp
static MyControl()
{
    FooProperty.Changed.AddClassHandler<MyControl>(FooChanged);
}

private static void FooChanged(MyControl sender, AvaloniaPropertyChangedEventArgs e)
{
    // Le paramètre 'e' décrit ce qui a changé.
}
```

## Liaison à des objets `INotifyPropertyChanged`

La liaison à des objets qui implémentent `INotifyPropertyChanged` est également disponible.

```csharp
var textBlock = new TextBlock();

var binding = new Binding 
{ 
    Source = someObjectImplementingINotifyPropertyChanged, 
    Path = nameof(someObjectImplementingINotifyPropertyChanged.MyProperty)
}; 

textBlock.Bind(TextBlock.TextProperty, binding);
```
