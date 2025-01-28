---
description: CONCEPTS - Modèles de données
---

import ControlContentButtonScreenshot from '/img/concepts/templates/content-button.png';
import ControlContentStringScreenshot from '/img/concepts/templates/content-string.png';
import ControlContentTypeScreenshot from '/img/concepts/templates/content-type.png';

# Contrôle de contenu

Vous avez probablement vu ce qui se passe si vous placez un contrôle de bouton dans la zone de contenu d'une fenêtre _Avalonia UI_.

:::info
Le concept des zones d'un contrôle _Avalonia UI_ est discuté [ici](../layout/layout-zones).
:::

Par exemple :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="MySample.MainWindow"
        Title="MySample">
  <Button HorizontalAlignment="Center" >Hello World!</Button>
</Window>
```

La fenêtre affiche le bouton - dans ce cas centré à la fois horizontalement (spécifié) et verticalement (par défaut). Cela ressemble à ceci :

<img src={ControlContentButtonScreenshot} alt=""/>

Et si vous placez une chaîne dans la zone de contenu de la fenêtre, par exemple :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="MySample.MainWindow"
        Title="MySample">
  Hello World!
</Window>
```

La fenêtre affichera la chaîne :

<img src={ControlContentStringScreenshot} alt=""/>

Mais que se passe-t-il si vous essayez d'afficher un objet d'une classe que vous avez définie dans la fenêtre ?

Par exemple, en utilisant la définition de classe `Student`

```csharp
namespace MySample
{
    public class Student
    {
        public string FirstName { get; set;} = String.Empty;
        public string LastName { get; set;} = String.Empty;
    }
}
```

Et l'espace de noms XML `local` défini comme l'espace de noms `MySample` (ci-dessus), vous pouvez définir un objet étudiant dans la zone de contenu de la fenêtre ; comme suit :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:local="using:MySample"
        x:Class="MySample.Views.MainWindow">
  <local:Student FirstName="Jane" LastName="Deer"/>
</Window>
```

Mais vous ne verrez que le nom de classe entièrement qualifié pour l'objet étudiant :

<img src={ControlContentTypeScreenshot} alt=""/>

Ce n'est pas très utile ! Cela se produit parce que _Avalonia UI_ n'a pas de définition de la façon d'afficher un objet de la classe `Student` - et ce n'est pas un contrôle - donc il revient à la méthode `.ToString()`, et tout ce que vous voyez est le nom de classe entièrement qualifié.

À la page suivante, vous verrez l'une des manières dont vous pouvez spécifier comment afficher un objet créé à partir d'une classe que vous avez définie (pas un contrôle ou une simple chaîne).
##
