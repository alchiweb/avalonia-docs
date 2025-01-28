---
id: code-behind
title: Code-behind
---

import VsSolutionExplorerScreenshot from '/img/basics/user-interface/code-behind/vs-solution-explorer.png';

# Code-behind

En plus d'un fichier XAML, la plupart des contrôles Avalonia ont également un fichier _code-behind_ qui est généralement écrit en C#. Par convention, le fichier code-behind a l'extension de fichier `.axaml.cs` et est souvent affiché imbriqué sous le fichier XAML dans votre IDE.

Par exemple, dans l'explorateur de solutions de Visual Studio, vous pouvez voir un fichier `MainWindow.axaml` ainsi que son fichier code-behind `MainWindow.axaml.cs` :

<p><img src={VsSolutionExplorerScreenshot} className="medium-zoom-image" /></p>

Le fichier code-behind contient une classe qui porte le même nom que le fichier XAML. Par exemple :

```csharp title='MainWindow.axaml.cs'
using Avalonia.Controls;

namespace AvaloniaApplication1.Views
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
        }
    }
}
```

Remarquez que le nom de la classe correspond au nom du fichier XAML et est également référencé dans l'attribut `x:Class` de l'élément fenêtre.

```xml title='MainWindow.axaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        // highlight-next-line
        x:Class="AvaloniaApplication1.Views.MainWindow">
  ...
</Window>
```

:::tip
Si vous apportez des modifications au nom de la classe dans le code, ou à son espace de noms, assurez-vous que l'attribut `x:Class` correspond toujours, sinon vous obtiendrez une erreur.
:::

Lorsque le fichier code-behind est ajouté pour la première fois, il ne contient qu'un constructeur, qui appelle la méthode `InitializeComponent()`. Cet appel de méthode est nécessaire pour charger le XAML à l'exécution.

## Localiser les Contrôles

Lorsque vous travaillez avec le code-behind, vous devez souvent accéder aux contrôles définis dans le XAML.

Pour ce faire, vous devez d'abord obtenir une référence au contrôle désiré. Donnez un nom au contrôle en utilisant l'attribut `Name` (ou `x:Name`) dans le XAML.

Voici un exemple d'un fichier XAML avec un bouton nommé :

```xml title='MainWindow.axaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="AvaloniaApplication5.MainWindow">
  // surligner-la-ligne-suivante
  <Button Name="greetingButton">Hello World</Button>
</Window>
```

Vous pouvez maintenant accéder au bouton via un champ `greetingButton` généré automatiquement depuis le code-behind :

```csharp title='MainWindow.axaml.cs'
using Avalonia.Controls;

namespace AvaloniaApplication1.Views
{
    public partial class MainWindow : Window
    {
        public MainWindow()
        {
            InitializeComponent();
            // surligner-la-ligne-suivante
            greetingButton.Content = "Goodbye Cruel World!";
        }
    }
}
```

## Définir des Propriétés

Avec la référence de contrôle disponible dans le code-behind, vous pouvez définir des propriétés. Par exemple, vous pouvez changer la propriété de fond comme ceci :

```csharp title='C#'
greetingButton.Background = Brushes.Blue;
```

## Gestion des Événements

Toute application utile nécessitera que vous mettiez en œuvre une action ! Lors de l'utilisation du modèle code-behind, vous écrivez des gestionnaires d'événements dans le fichier code-behind.

Les gestionnaires d'événements sont écrits sous forme de méthodes dans le fichier code-behind, puis référencés dans le XAML à l'aide d'un attribut d'événement. Par exemple, pour ajouter un gestionnaire pour un événement de clic de bouton :

```xml title='MainWindow.axaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="AvaloniaApplication4.MainWindow">
  <Button Click="GreetingButtonClickHandler">Bonjour le monde</Button>
</Window>
```

```csharp title='MainWindow.axaml.cs'
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    public void GreetingButtonClickHandler(object sender, RoutedEventArgs e)
    {
        // code ici.
    }
}
```

Notez que de nombreux gestionnaires d'événements Avalonia passent un argument spécial de la classe `RoutedEventArgs`. Cela inclut des informations sur la manière dont l'événement a été généré et propagé.

:::info
Pour plus d'informations sur les concepts de routage des événements, voir [ici](../../concepts/input/routed-events.md).
:::