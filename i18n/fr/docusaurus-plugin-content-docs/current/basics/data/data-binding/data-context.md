---
description: CONCEPTS
---

import DataContextOverviewDiagram from '/img/basics/data-binding/data-context/data-context-overview.png';
import DataContextTreeSearchDiagram from '/img/basics/data-binding/data-context/data-context-tree-search.png';
import DataContextGreetingBindingScreenshot from '/img/basics/data-binding/data-context/data-context-greeting.png';
import DataContextPreviewerScreenshot from '/img/basics/data-binding/data-context/data-context-previewer.png';

# Contexte des données

Lorsque Avalonia effectue un lien de données, il doit localiser un objet d'application auquel se lier. Cet emplacement est représenté par un **Contexte de Données**.

<img src={DataContextOverviewDiagram} alt=''/>

Chaque contrôle dans Avalonia possède une propriété appelée `DataContext`, y compris les contrôles intégrés, les contrôles utilisateur et les fenêtres.

Lors du lien, Avalonia effectue une recherche hiérarchique dans l'arbre logique des contrôles, en commençant par le contrôle où le lien est défini, jusqu'à ce qu'il trouve un contexte de données à utiliser.

<img src={DataContextTreeSearchDiagram} alt=''/>

Cela signifie qu'un contrôle défini dans une fenêtre peut utiliser le contexte de données de la fenêtre ; ou (comme ci-dessus) un contrôle dans un contrôle dans une fenêtre peut utiliser le contexte de données de la fenêtre.

:::info
Pour des informations sur les arbres de contrôles dans Avalonia, et comment les voir à l'exécution, voir [ici](../../../concepts/control-trees).
:::

## Exemple

Vous pouvez voir le contexte de données de la fenêtre être défini si vous créez un nouveau projet en utilisant le modèle _Application MVVM Avalonia_. Localisez et ouvrez le fichier **App.axaml.cs** pour voir le code :

```csharp
public override void OnFrameworkInitializationCompleted()
{
    if (ApplicationLifetime is IClassicDesktopStyleApplicationLifetime desktop)
    {
        desktop.MainWindow = new MainWindow
        {
            DataContext = new MainWindowViewModel(),
        };
    }

    base.OnFrameworkInitializationCompleted();
}
```

Vous pouvez tracer l'objet défini dans le contexte de données de la fenêtre dans le fichier **MainWindowViewModel.cs**. Le code ressemble à ceci :

```csharp
public class MainWindowViewModel : ViewModelBase
{
    public string Greeting => "Bienvenue dans Avalonia !";
}
```

Dans le fichier de la fenêtre principale **MainWindow.axaml**, vous pouvez voir que la zone de contenu de la fenêtre est composée d'un bloc de texte dont la propriété de texte est liée à la propriété `Greeting`.

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:vm="using:AvaloniaMVVMApplication2.ViewModels"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="AvaloniaMVVMApplication2.Views.MainWindow"
        Icon="/Assets/avalonia-logo.ico"
        Title="AvaloniaMVVMApplication2">

    <Design.DataContext>
        <vm:MainWindowViewModel/>
    </Design.DataContext>

    <TextBlock Text="{Binding Greeting}" HorizontalAlignment="Center" VerticalAlignment="Center"/>

</Window>
```

Lorsque le projet s'exécute, le lien de données recherche dans l'arbre de contrôle logique à partir du bloc de texte et trouve un contexte de données défini au niveau de la fenêtre principale. Ainsi, le texte lié apparaît comme :

<img src={DataContextGreetingBindingScreenshot} alt=""/>

## Contexte de données de conception

Vous avez peut-être remarqué, après avoir d'abord compilé ce projet, que le panneau de prévisualisation affiche également le message de bienvenue.

<img src={DataContextPreviewerScreenshot} alt=""/>

C'est parce qu'Avalonia peut également définir un contexte de données pour un contrôle à utiliser au moment de la conception. Vous trouverez cela utile car cela signifie que le panneau de prévisualisation peut afficher des données réalistes pendant que vous ajustez la mise en page et les styles.

Vous pouvez voir le contexte de données de conception défini dans le XAML :

```xml
<Design.DataContext>
    <vm:MainWindowViewModel/>
</Design.DataContext>
```

:::tip
Pour un guide plus détaillé sur l'utilisation du contexte de données de conception, voir [ici](../../../guides/implementation-guides/how-to-use-design-time-data.md).
:::

:::info
Une discussion plus approfondie sur le binding de données nécessite que vous ayez une connaissance de base du modèle de programmation MVVM. Pour une introduction aux concepts du modèle MVVM, voir [ici](../../../concepts/the-mvvm-pattern).
:::

Informations supplémentaires

Lier aux commandes