---
id: how-to-bind-tabs
title: Comment lier des onglets
---

## Exemple de support de liaison

Vous pouvez créer dynamiquement des éléments d'onglet avec **liaison de données**. Pour ce faire, liez la propriété `ItemsSource` d'un contrôle d'onglet à un tableau d'objets représentant l'en-tête et le contenu de l'onglet.

Vous pouvez ensuite utiliser un **modèle de données** pour afficher les objets.

Cet exemple utilise un tableau d'objets créé à partir de cette classe `TabItemViewModel` :

```csharp
namespace MyApp.ViewModel;

public class TabItemViewModel
{
    public string Header { get; }
    public string Content { get; }
    public TabItemViewModel(string header, string content)
    {
        Header = header;
        Content = content;
    }
}
```

Créez un tableau de deux instances de `TabItemViewModel` et liez-le au DataContext.

```csharp
DataContext = new TabItemViewModel[] { 
    new TabItemViewModel("One", "Some content on first tab"),
    new TabItemViewModel("Two", "Some content on second tab"),
};
```

Le contenu de l'en-tête `TabStrip` est défini par la propriété ItemTemplate, tandis que le contenu de `TabItem` est défini par la propriété ContentTemplate.

Enfin, créez un `TabControl` et liez sa propriété `ItemsSource` au DataContext.

```xml
<TabControl ItemsSource="{Binding}">
    <TabControl.ItemTemplate>
      <DataTemplate>
        <TextBlock Text="{Binding Header}" />
      </DataTemplate>
    </TabControl.ItemTemplate>
    <TabControl.ContentTemplate>
        <!-- Le DataTemplate de ContentTemplate doit spécifier le modèle de vue dans DataType. L'alias 'vm' fait référence à la spécification de l'espace de noms du modèle de vue dans un attribut de l'élément racine XAML, qui ressemblera à :
            xmlns:vm="using:MyApp.ViewModel"
        ou à :
            xmlns:vm="clr-namespace:MyApp.ViewModel;assembly=MyApp.ViewModel" -->
      <DataTemplate DataType="vm:TabItemViewModel">
        <DockPanel LastChildFill="True">
          <TextBlock Text="This is content of selected tab" DockPanel.Dock="Top" FontWeight="Bold" />
          <TextBlock Text="{Binding Content}" />
        </DockPanel>
      </DataTemplate>
    </TabControl.ContentTemplate>
  </TabControl>
```
