---
id: add-a-control
title: Ajoutez un contrôle
---

import Highlight from '@site/src/components/Highlight';
import CalculateButton from '/img/get-started/test-drive/calculate-button.png';
import ButtonIntellisenseScreenshot from '/img/get-started/test-drive/button-intellisense.png';

Jusqu'à présent, la fenêtre principale de votre application n'affiche qu'une chaîne de texte. Sur cette page, vous apprendrez à ajouter certains des contrôles intégrés qui font partie d'Avalonia.

## Bouton

Avalonia contient un contrôle intégré qui crée un bouton. Suivez cette procédure pour remplacer la chaîne de texte actuellement dans la zone de contenu de la `Window` par un contrôle de bouton.

- Arrêtez l'application si elle est en cours d'exécution.
- Localisez la ligne de XAML surlignée dans le fichier `MainWindow.xaml`.

```xml title='XAML' 
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:vm="using:GetStartedApp.ViewModels"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="400" d:DesignHeight="450"
        x:Class="GetStartedApp.Views.MainWindow"
        x:DataType="vm:MainWindowViewModel"
        Icon="/Assets/avalonia-logo.ico"
        Title="GetStartedApp">

    <Design.DataContext>
        <!-- Cela ne définit que le DataContext pour le visualiseur dans un IDE,
             pour définir le DataContext réel à l'exécution, définissez la propriété DataContext dans le code (regardez App.axaml.cs) -->
        <vm:MainWindowViewModel/>
    </Design.DataContext>

    // highlight-next-line
    <TextBlock Text="{Binding Greeting}" HorizontalAlignment="Center" VerticalAlignment="Center"/>
    
</Window>
```

- Remplacez la ligne entière par ce qui suit :
```xml title='XAML'
<Button>Calculer</Button>
```
- Votre XAML devrait maintenant ressembler à ceci :

```xml title='XAML'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:vm="using:GetStartedApp.ViewModels"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="400" d:DesignHeight="450"
        x:Class="GetStartedApp.Views.MainWindow"
        x:DataType="vm:MainWindowViewModel"
        Icon="/Assets/avalonia-logo.ico"
        Title="GetStartedApp">

    <Design.DataContext>
        <!-- Cela ne définit que le DataContext pour le visualiseur dans un IDE,
             pour définir le DataContext réel à l'exécution, définissez la propriété DataContext dans le code (regardez App.axaml.cs) -->
        <vm:MainWindowViewModel/>
    </Design.DataContext>
    
    // highlight-next-line
    <Button>Calculer</Button>    
</Window>
```

:::tip
Si vous utilisez le visualiseur, vous verrez le bouton apparaître dans le panneau de prévisualisation dès que le XAML est valide. Vous pouvez également essayer de survoler et de cliquer sur le `Button` pour voir son apparence changer dans différents états.
:::

- Exécutez l'application pour confirmer que la présentation et le comportement du bouton sont les mêmes à l'exécution.

## Attributs de Contrôle

XAML utilise des attributs XML pour spécifier la présentation et le comportement des contrôles. Ces attributs peuvent définir des propriétés, appeler des méthodes et appeler des gestionnaires d'événements dans les contrôles créés par le XAML.

Par exemple, le `Button` est actuellement positionné à gauche, collé contre le bord gauche de la `Window`. Cela résulte de la valeur par défaut (gauche) de sa propriété `HorizontalAlignment`. Suivez cette procédure pour définir l'`HorizontalAlignment` sur centré à la place.

- Ajoutez un nouvel attribut à la balise Button comme suit :

```xml title='XAML'
<Button HorizontalAlignment="Center">Calculer</Button>
```

:::tip
Si vous utilisez un IDE, remarquez comment la complétion de code Avalonia vous guide lorsque vous ajoutez des attributs au XAML.

<img className="center" src={ButtonIntellisenseScreenshot} alt="" />
:::

Le `Button` devrait maintenant se déplacer vers le centre de la zone de contenu de la fenêtre. Horizontalement à cause du changement et verticalement à cause de la valeur par défaut du Button.

:::info
Pour des informations complètes sur l'ensemble des contrôles intégrés d'Avalonia UI et leurs attributs, consultez la section de référence [ici](../../reference/controls).
:::

À la page suivante, vous apprendrez à créer une mise en page plus complexe.
