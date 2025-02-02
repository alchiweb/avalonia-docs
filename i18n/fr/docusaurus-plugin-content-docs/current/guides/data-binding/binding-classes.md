---
id: binding-classes
title: Comment Lier des Classes de Style
---

import BindStyleClassSampleScreenshot from '/img/guides/data-binding/bind-style-class.png';

# Comment Lier des Classes de Style

Ce guide vous montrera comment appliquer des classes de style à un contrôle en fonction de la valeur booléenne d'une liaison de données.

Pour ce faire, vous aurez besoin de certaines classes définies dans une collection `<Styles>` qui ciblent la classe de contrôle que vous utilisez.

Vous pouvez ensuite appliquer conditionnellement les classes à un contrôle en utilisant une syntaxe de classes spéciale et une liaison de données. La syntaxe est la suivante :

```
<SomeControl Classes.class1="{Binding IsClass1Active}">
```

## Exemple

Dans cet exemple, deux styles avec des sélecteurs de classe ont été définis. Ceux-ci donnent à un bloc de texte soit un fond rouge, soit un fond vert. La liaison de classe de style attribue `class1` lorsque la propriété `IsClass1` d'un élément est vraie. En utilisant l'opérateur de négation, `class2` est attribué lorsque la propriété `IsClass1` est fausse.

```xml title='XAML'
<StackPanel Margin="20">
  <ListBox ItemsSource="{Binding ItemList}">
    <ListBox.Styles>
      <Style Selector="TextBlock.class1">
        <Setter Property="Background" Value="OrangeRed" />
      </Style>
      <Style Selector="TextBlock.class2">
        <Setter Property="Background" Value="PaleGreen" />
      </Style>
    </ListBox.Styles>
    <ListBox.ItemTemplate>
      <DataTemplate>
        <StackPanel>
          <TextBlock
              Classes.class1="{Binding IsClass1 }"
              Classes.class2="{Binding !IsClass1 }"
              Text="{Binding Title}"/>
        </StackPanel>
      </DataTemplate>
    </ListBox.ItemTemplate>
  </ListBox>
</StackPanel>
```

```csharp title='C#'
public class MainWindowViewModel : ViewModelBase
{
    public ObservableCollection<ItemClass> ItemList { get; set; }

    public MainWindowViewModel()
    {
        ItemList = new ObservableCollection<ItemClass>(new List<ItemClass>
        {
            new ItemClass("Item 1", false),
            new ItemClass("Item Deux", false),
            new ItemClass("Troisième Item", true),
            new ItemClass("Item n°4", false),
               
        });
    }
}
```

```csharp title='ItemClass.cs'
public class ItemClass
{
    public string Title { get; set; }
    public bool IsClass1 { get; set; }

    public ItemClass(string title, bool isClass1 )
    {
        Title = title;
        IsClass1 = isClass1;
    }
}
```

<img src={BindStyleClassSampleScreenshot} alt=""/>
