---
id: how-to-create-a-custom-controls-library
title: Comment créer une bibliothèque de contrôles personnalisés
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CustomControlSolutionScreenshot from '/img/guides/custom-controls/custom-control-solution.png';
import CustomControlNuGetScreenshot from '/img/guides/custom-controls/custom-control-nuget.png';

# Comment créer une bibliothèque de contrôles personnalisés

Ce guide vous montre comment créer une bibliothèque de contrôles personnalisés et la référencer pour une utilisation dans une application _Avalonia UI_.

<img src={CustomControlSolutionScreenshot} alt=""/>

Dans cet exemple, un fichier de contrôle personnalisé est ajouté à une bibliothèque de classes .NET. La bibliothèque a le package _NuGet_ _Avalonia UI_ installé :

<img src={CustomControlNuGetScreenshot} alt=""/>

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:cc="clr-namespace:CCLibrary;assembly=CCLibrary"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaCCLib.MainWindow"
        Title="AvaloniaCCLib">
  <Window.Styles>
    <Style Selector="cc|MyCustomControl">
      <Setter Property="Background" Value="Yellow"/>
    </Style>
  </Window.Styles>

  <cc:MyCustomControl Height="200" Width="300"/>

</Window>
```

</TabItem>
<TabItem value="cs">

```cs
using Avalonia;
using Avalonia.Controls;
using Avalonia.Media;

namespace CCLibrary
{

    public class MyCustomControl : Control
    {
        public static readonly StyledProperty<IBrush?> BackgroundProperty =
            Border.BackgroundProperty.AddOwner<MyCustomControl>();

        public IBrush? Background
        {
            get { return GetValue(BackgroundProperty); }
            set { SetValue(BackgroundProperty, value); }
        }

        public sealed override void Render(DrawingContext context)
        {
            if (Background != null)
            {
                var renderSize = Bounds.Size;
                context.FillRectangle(Background, new Rect(renderSize));
            }
            base.Render(context);
        }
    }
}
```
</TabItem>  

</Tabs>

:::info
Notez que la référence de l'espace de noms pour la bibliothèque de contrôles inclut le nom de l'assemblage.
:::

## Définitions des espaces de noms XML

Lorsque vous ajoutez une référence à une bibliothèque de contrôles dans un fichier XAML _Avalonia UI_, vous pouvez vouloir utiliser le format d'identification par URL. Par exemple :

```xml
xmlns:cc="https://my.controls.url"
```

Ceci est possible grâce à la présence de définitions d'espaces de noms XML dans une bibliothèque de contrôles. Celles-ci mappent les URL aux espaces de noms de code et se trouvent dans le fichier `Properties/AssemblyInfo.cs` du projet. Par exemple :

```csharp
[assembly: XmlnsDefinition("https://github.com/avaloniaui", "Avalonia")]
```

:::info
Vous pouvez voir cela dans le code source des contrôles intégrés _Avalonia UI_ [ici](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Controls/Properties/AssemblyInfo.cs).
:::

### Définitions d'espaces de noms courants

Vous pouvez également faire en sorte qu'une URL map plusieurs espaces de noms dans votre bibliothèque de contrôles. Pour ce faire, ajoutez simplement plusieurs définitions d'espaces de noms XML qui utilisent la même URL, mais qui mappent à différents espaces de noms de code, comme ceci :

```cs
using Avalonia.Metadata;

[assembly: XmlnsDefinition("https://my.controls.url", "My.NameSpace")]
[assembly: XmlnsDefinition("https://my.controls.url", "My.NameSpace.Other")]
```
