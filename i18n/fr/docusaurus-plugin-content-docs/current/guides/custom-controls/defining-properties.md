---
id: defining-properties
title: Définir des Propriétés
---

import DefiningPropertyPreviewScreenshot from '/img/guides/custom-controls/defining-property-preview.png';

# Propriété Stylisée

Si vous créez un contrôle personnalisé, vous voudrez généralement qu'il ait des propriétés pouvant être définies par le système de style _Avalonia UI_.

:::info
Pour plus d'informations sur l'utilisation des styles dans _Avalonia UI_, consultez le guide [ici](../../basics/user-interface/styling).
:::

Sur cette page, vous verrez comment implémenter une propriété afin qu'elle puisse être modifiée par le système de style _Avalonia UI_. C'est un processus en deux étapes :

* Enregistrer une propriété stylisée.
* Fournir le getter/setter pour la propriété.

### Enregistrer une Propriété Stylisée

Vous enregistrez une propriété stylisée en définissant un champ statique en lecture seule et en utilisant la méthode `AvaloniaProperty.Register`.

Il existe une convention pour le nom d'une propriété. Il doit suivre le modèle :

```
[AttributeName]Property
```

Cela signifie que _Avalonia UI_ recherchera un attribut dans le XAML, comme ceci :

```
<MyCustomControl AttributeName="value" ... >
```

Par exemple, avec une propriété stylisée en place, vous pouvez contrôler la couleur de fond du contrôle personnalisé à partir de la collection de styles de la fenêtre :

```xml title='MainWindow.axaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:cc="using:AvaloniaCCExample.CustomControls"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaCCExample.MainWindow"
        Title="Avalonia Custom Control">

  <Window.Styles>
    <Style Selector="cc|MyCustomControl">
      <Setter Property="Background" Value="Yellow"/>
    </Style>
  </Window.Styles>

  <cc:MyCustomControl Height="200" Width="300"/>

</Window>
```

```csharp title='MyCustomControl.cs'
using Avalonia;
using Avalonia.Controls;
using Avalonia.Media;

namespace AvaloniaCCExample.CustomControls
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

:::info
Notez que le getter/setter de la propriété utilise les méthodes spéciales `GetValue` et `SetValue` d'Avalonia UI.
:::

La propriété stylisée fonctionnera à la fois à l'exécution et dans le panneau de prévisualisation.

<img src={DefiningPropertyPreviewScreenshot} alt=''/>

:::info
Pour des informations plus avancées sur la création d'un contrôle personnalisé, voir [ici](../custom-controls/how-to-create-advanced-custom-controls.md).
:::
