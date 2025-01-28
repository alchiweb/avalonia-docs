---
description: CONCEPTS
---

import ItemsControlCanvasScreenshot from '/img/concepts/itemscontrol-with-canvas.png';

# Panneau d'éléments personnalisé

Tous les `ItemsControl` ont un panneau de conteneurs d'éléments qui est utilisé pour disposer leurs éléments. Il est possible de remplacer le type de panneau utilisé par le contrôle pour obtenir des dispositions d'éléments personnalisées ou alternatives dans un contrôle. Ce document fournit quelques exemples montrant comment et pourquoi vous feriez cela.

- [`ItemsControl`](./../reference/controls/itemscontrol)
- [`TreeView`](./../reference/controls/treeview-1)
- [`Carousel`](./../reference/controls/carousel)
- [`Menu`](./../reference/controls/menu)
- [`ComboBox`](./../reference/controls/combobox)
- [`ListBox`](./../reference/controls/listbox)

## Exemple
Cet exemple lie une collection observable de `Rectangle`s (basée sur les données du modèle de vue Tile) à un `ItemsControl`. ItemsControl.ItemPanel est défini sur un `Canvas` et nous utilisons un style pour positionner le `Rectangle` à l'intérieur du `Canvas`.

```xml
<ItemsControl ItemsSource="{Binding TileList}">
  <ItemsControl.ItemsPanel>
    <ItemsPanelTemplate>
      <Canvas Width="50" Height="50" Background="Yellow" Margin="3"/>
    </ItemsPanelTemplate>
  </ItemsControl.ItemsPanel>
  <ItemsControl.ItemTemplate>
    <DataTemplate>
      <Rectangle Fill="Green" Height="{Binding Size}" Width="{Binding Size}"/>
    </DataTemplate>
  </ItemsControl.ItemTemplate>
  <ItemsControl.Styles>
    <Style Selector="ContentPresenter"  x:DataType="vm:Tile">
      <Setter Property="Canvas.Left" Value="{Binding TopX}"/>
      <Setter Property="Canvas.Top" Value="{Binding TopY}"/>
    </Style>
  </ItemsControl.Styles>
</ItemsControl>
```

```csharp title='Modèle de vue C#'
using AvaloniaControls.Models;
using System.Collections.Generic;
using System.Collections.ObjectModel;

namespace AvaloniaControls.ViewModels
{
    public class MainWindowViewModel
    {
        public ObservableCollection<Tile> TileList { get; set; }
        
        public MainWindowViewModel()
        {
            TileList = new ObservableCollection<Tile>(new List<Tile>
            {
                new Tile(10, 10, 10),
                new Tile(10, 20, 20),
                new Tile(10, 30, 30),
            });    
        }
    }
}
```

```csharp title='Classe d'élément C#'
public record Tile(int Size, int TopX, int TopY);
```

<img src={ItemsControlCanvasScreenshot} alt="" />