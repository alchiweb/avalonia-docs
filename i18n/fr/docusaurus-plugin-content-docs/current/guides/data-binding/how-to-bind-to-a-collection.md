---
id: how-to-bind-to-a-collection
title: Comment se lier à une collection
---


# Comment se lier à une collection

Lier à une collection dans Avalonia UI est un moyen efficace d'afficher des données dynamiques. Ce guide démontrera comment lier un `ObservableCollection` à un contrôle, comme un `ListBox` ou `ItemsControl`, pour afficher une liste d'éléments.

## Liaison à une ObservableCollection simple

Pour commencer, imaginez que vous avez un `ObservableCollection<string>` et que vous souhaitez le lier à un `ListBox` pour afficher une liste d'éléments de chaîne.

Voici un exemple de `ViewModel` avec un `ObservableCollection<string>` :

```csharp 
public class ViewModel : ObservableObject
{
    private ObservableCollection<string> _items;

    public ObservableCollection<string> Items
    {
        get { return _items; }
        set { SetProperty(ref _items, value); }
    }

    public ViewModel()
    {
        Items = new ObservableCollection<string> { "Item 1", "Item 2", "Item 3" };
    }
}
```

Dans votre vue, vous pouvez lier cette `ObservableCollection` à un `ListBox` comme ceci :

```xml
<ListBox ItemsSource="{Binding Items}"/>
```

## Liaison à une ObservableCollection d'Objets Complexes

Mais que se passe-t-il si votre `ObservableCollection` contient des objets complexes qui doivent eux-mêmes propager des changements ? Modifions notre `ViewModel` pour accommoder ce scénario.

Considérons une classe `Person` :

```csharp
public class Person : ObservableObject
{
    private string _name;
    private int _age;

    public string Name
    {
        get { return _name; }
        set { SetProperty(ref _name, value); }
    }

    public int Age
    {
        get { return _age; }
        set { SetProperty(ref _age, value); }
    }
}
```

Et une `ObservableCollection<Person>` dans notre ViewModel :

```csharp
public class ViewModel : ObservableObject
{
    private ObservableCollection<Person> _people;

    public ObservableCollection<Person> People
    {
        get { return _people; }
        set { SetProperty(ref _people, value); }
    }

    public ViewModel()
    {
        People = new ObservableCollection<Person> 
        {
            new Person { Name = "John Doe", Age = 30 },
            new Person { Name = "Jane Doe", Age = 28 }
        };
    }
}
```

Vous pouvez lier cette `ObservableCollection` à un `ListBox` dans votre vue, et utiliser un `DataTemplate` pour spécifier comment chaque `Person` doit être présentée :

```xml
<ListBox ItemsSource="{Binding People}">
    <ListBox.ItemTemplate>
        <DataTemplate>
            <StackPanel Orientation="Horizontal">
                <TextBlock Text="{Binding Name}" Margin="0,0,10,0"/>
                <TextBlock Text="{Binding Age}"/>
            </StackPanel>
        </DataTemplate>
    </ListBox.ItemTemplate>
</ListBox>
```

Dans ce scénario, chaque `Person` de la liste sera affichée avec son `Nom` et son `Âge` séparés par une petite marge. Si l'une des propriétés des éléments change, l'élément du `ListBox` se mettra automatiquement à jour.
