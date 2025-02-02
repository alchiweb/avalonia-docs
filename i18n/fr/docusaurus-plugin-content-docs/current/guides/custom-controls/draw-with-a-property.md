---
id: draw-with-a-property
title: Dessiner avec une Propriété
---

import DrawWithPropertyScreenshot from '/img/guides/custom-controls/draw-property.png';

# Dessiner avec une Propriété

Sur cette page, vous verrez comment dessiner un contrôle personnalisé, en utilisant la valeur d'une propriété simple qui définit la couleur de fond. Le code ressemble maintenant à ceci :

```xml title='MainWindow.xaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:cc="using:AvaloniaCCExample.CustomControls"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaCCExample.MainWindow"
        Title="Avalonia Custom Control">
  <cc:MyCustomControl Height="200" Width="300" Background="Red"/>
</Window>

```

```csharp title='MyCustomControl.cs'
using Avalonia.Controls;

namespace AvaloniaCCExample.CustomControls
{
    public class MyCustomControl : Control
    {
        public IBrush? Background { get; set; }

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

Cet exemple définit une propriété de pinceau simple sur le contrôle personnalisé pour la couleur de fond. Il remplace ensuite la méthode `Render` pour dessiner le contrôle.

Le code de dessin utilise le contexte graphique _Avalonia UI_ (qui est passé à la méthode de rendu), pour dessiner un rectangle rempli de la couleur de fond, et de la même taille que le contrôle (comme fourni par l'objet `Bounds.Size`).

<img src={DrawWithPropertyScreenshot} alt=""/>

Remarquez comment le contrôle s'affiche maintenant à la fois à l'exécution (ci-dessus) et dans le panneau de prévisualisation.

Sur la page suivante, vous verrez comment implémenter la propriété de fond afin qu'elle puisse être modifiée par le système de style _Avalonia UI_.

:::tip
Vous pouvez trouver un tutoriel plus avancé dans [Avalonia.Samples](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main/src/Avalonia.Samples/CustomControls/SnowflakesControlSample)
:::
