---
id: how-to-create-attached-properties
title: Comment créer des propriétés attachées
---

# Comment créer des propriétés attachées

Lorsque vous avez besoin de propriétés supplémentaires ou, disons, étrangères sur les éléments Avalonia, les propriétés attachées sont la solution idéale à utiliser. Elles peuvent également être utilisées pour créer ce que l'on appelle des comportements afin de modifier généralement les composants GUI hébergés. Cela peut être utilisé pour lier une commande à un événement, par exemple.

Nous présentons ici un exemple de la façon d'utiliser une commande de manière compatible avec MVVM et de la lier à un événement.

Ce n'est peut-être pas la solution idéale car il existe des projets tels que [Avalonia Behaviors](https://github.com/wieslawsoltes/AvaloniaBehaviors) où cela est correctement fait. Mais cela illustre les deux apprentissages suivants :

* Comment créer des propriétés attachées dans _Avalonia UI_
* Comment les utiliser de manière MVVM.

Tout d'abord, nous devons créer notre propriété attachée. La méthode `AvaloniaProperty.RegisterAttached` est utilisée à cette fin. Notez qu'en convention, la propriété CLR **publique statique** pour la propriété attachée est nommée _XxxxProperty_. Notez également qu'en convention, le nom (le paramètre) de la propriété attachée est _Xxxx_ sans le _Property_. Et enfin, notez qu'en convention, il faut fournir deux méthodes **publiques statiques** appelées _SetXxxx(element,value)_ et _GetXxxx(element)_.

Cet appel garantit que la propriété a un type, un type propriétaire et un endroit où elle peut être utilisée.

La méthode de vérification peut être utilisée pour nettoyer une valeur qui est en cours de définition. Soit en retournant la valeur corrigée, soit en abandonnant le processus en retournant `AvaloniaProperty.UnsetValue`. Ou l'on peut effectuer des tâches spéciales avec l'élément auquel la propriété est associée. Les méthodes getter et setter ne devraient toujours que définir la valeur et ne jamais faire quoi que ce soit de plus. En fait, elles ne seront généralement jamais appelées car le système de liaison reconnaîtra la convention et définira les propriétés directement là où elles sont stockées.

Dans ce fichier d'exemple, nous créons deux propriétés attachées qui interagissent entre elles : une propriété _Command_ et un _CommandParameter_ qui est utilisé lors de l'invocation de la commande.

```csharp
/// <summary>
/// Classe conteneur pour les propriété attachées. Doit hériter de <see cref="AvaloniaObject"/>.
/// </summary>
public class DoubleTappedBehav : AvaloniaObject
{
    static DoubleTappedBehav()
    {
        CommandProperty.Changed.AddClassHandler<Interactive>(HandleCommandChanged);
    }

    /// <summary>
    /// Identifie la propriété attachée <seealso cref="CommandProperty"/> d'Avalonia.
    /// </summary>
    /// <value>Fournit un objet ou une liaison dérivé de <see cref="ICommand"/>.</value>
    public static readonly AttachedProperty<ICommand> CommandProperty = AvaloniaProperty.RegisterAttached<DoubleTappedBehav, Interactive, ICommand>(
        "Command", default(ICommand), false, BindingMode.OneTime);

    /// <summary>
    /// Identifie la propriété attachée <seealso cref="CommandParameterProperty"/> d'Avalonia.
    /// Utilise ceci comme paramètre pour la <see cref="CommandProperty"/>.
    /// </summary>
    /// <value>Toute valeur de type <see cref="object"/>.</value>
    public static readonly AttachedProperty<object> CommandParameterProperty = AvaloniaProperty.RegisterAttached<DoubleTappedBehav, Interactive, object>(
        "CommandParameter", default(object), false, BindingMode.OneWay, null);


    /// <summary>
    /// <see cref="CommandProperty"/> changed event handler.
    /// </summary>
    private static void HandleCommandChanged(Interactive interactElem, AvaloniaPropertyChangedEventArgs args)
    {
        if (args.NewValue is ICommand commandValue)
        {
             // Ajouter une valeur non nulle
             interactElem.AddHandler(InputElement.DoubleTappedEvent, Handler);
        }
        else
        {
             // Supprimer la valeur précédente
             interactElem.RemoveHandler(InputElement.DoubleTappedEvent, Handler);
        }
        // fonction de gestionnaire local
        static void Handler(object s, RoutedEventArgs e)
        {
            if (s is Interactive interactElem)
            {
                // C'est ainsi que nous obtenons le paramètre de l'élément GUI.
                object commandParameter = interactElem.GetValue(CommandParameterProperty);
                ICommand commandValue = interactElem.GetValue(CommandProperty);
                if (commandValue?.CanExecute(commandParameter) == true)
                {
                    commandValue.Execute(commandParameter);
                }
            }
        }
    }


    /// <summary>
    /// Accesseur pour la propriété attachée <see cref="CommandProperty"/>.
    /// </summary>
    public static void SetCommand(AvaloniaObject element, ICommand commandValue)
    {
        element.SetValue(CommandProperty, commandValue);
    }

    /// <summary>
    /// Accesseur pour la propriété attachée <see cref="CommandProperty"/>.
    /// </summary>
    public static ICommand GetCommand(AvaloniaObject element)
    {
        return element.GetValue(CommandProperty);
    }

    /// <summary>
    /// Accesseur pour la propriété attachée <see cref="CommandParameterProperty"/>.
    /// </summary>
    public static void SetCommandParameter(AvaloniaObject element, object parameter)
    {
        element.SetValue(CommandParameterProperty, parameter);
    }

    /// <summary>
    /// Accesseur pour la propriété attachée <see cref="CommandParameterProperty"/>.
    /// </summary>
    public static object GetCommandParameter(AvaloniaObject element)
    {
        return element.GetValue(CommandParameterProperty);
    }
}

```

Dans la méthode de vérification, nous utilisons le système d'événements routés pour attacher un nouveau gestionnaire. Notez que le gestionnaire doit être détaché, à nouveau. La valeur de la propriété est demandée par les mécanismes normaux du programme en utilisant la méthode `GetValue()`.

Cet exemple d'interface utilisateur montre comment utiliser la propriété attachée. Après avoir fait connaître l'espace de noms au compilateur XAML, il peut être utilisé en le qualifiant avec un point. Ensuite, des liaisons peuvent être utilisées.

```xml
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:loc="clr-namespace:MyApp.Behaviors"
             x:Class="MyApp.Views.TestView">
    <ListBox ItemsSource="{Binding Accounts}"
             SelectedIndex="{Binding SelectedAccountIdx, Mode=TwoWay}"
             loc:DoubleTappedBehav.Command="{Binding EditCommand}"
             loc:DoubleTappedBehav.CommandParameter="test77"
             >
      <ListBox.ItemTemplate>
        <DataTemplate>
          <TextBlock Text="{Binding }" />          
        </DataTemplate>
      </ListBox.ItemTemplate>
    </ListBox>
</UserControl>
```

Bien que le `CommandParameter` n'utilise qu'une valeur statique, il peut également être utilisé avec une liaison. Lorsqu'il est utilisé avec ce modèle de vue, le `EditCommandExecuted` sera exécuté lorsqu'un double-clic se produit.

```csharp
public class TestViewModel : ReactiveObject
{
    public ObservableCollection<Profile> Accounts { get; } = new ObservableCollection<Profile>();

    public ReactiveCommand<object, Unit> EditCommand { get; set; }

    public TestViewModel()
    {
        EditCommand = ReactiveCommand.CreateFromTask<object, Unit>(EditCommandExecuted);
    }

    private async Task<Unit> EditCommandExecuted(object p)
    {
        // p contient "test77"

        return Unit.Default;
    }
}
```
