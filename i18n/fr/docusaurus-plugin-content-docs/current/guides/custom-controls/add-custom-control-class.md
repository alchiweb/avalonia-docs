---
id: add-custom-control-class
title: Ajouter une classe de contrôle personnalisée
---

# Ajouter une classe de contrôle personnalisée

Vous créez un contrôle personnalisé en utilisant une classe qui hérite de la classe `Control` de _Avalonia UI_. Vous pouvez placer vos classes de contrôle personnalisées n'importe où dans votre projet d'application, ou les inclure dans un autre projet de bibliothèque de contrôles.

:::info
Pour plus d'informations sur la création d'une bibliothèque de contrôles personnalisés, voir [ici](how-to-create-a-custom-controls-library).
:::

Où que vous choisissiez de placer votre classe de contrôle personnalisée, vous devez être en mesure de la référencer dans le XAML. Par exemple, ce code montre la classe de contrôle personnalisée `MyControl` placée dans la fenêtre principale ; et la classe de contrôle personnalisée définie dans l'espace de noms et le dossier de projet `/CustomControls`:

```xml title='XAML'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:cc="using:AvaloniaCCExample.CustomControls"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaCCExample.MainWindow"
        Title="Avalonia Custom Control">
  <cc:MyCustomControl Height="200" Width="300"/>
</Window>

```

```csharp title='C#'
using Avalonia.Controls;

namespace AvaloniaCCExample.CustomControls
{
    public class MyCustomControl : Control
    {
    }
}
```

Remarquez que vous pouvez déjà ajouter des propriétés pour la hauteur et la largeur du contrôle personnalisé. Celles-ci proviennent de la classe de base : `Control`.

Cependant, pour l'instant, rien ne s'affiche. À la page suivante, vous verrez comment définir une propriété et apprendre au contrôle personnalisé comment dessiner en l'utilisant.
