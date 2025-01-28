---
id: adding-interactivity
title: Ajouter de l'interactivité
---

L'une des choses fondamentales qu'une interface utilisateur doit faire est d'interagir avec l'utilisateur. Dans Avalonia, vous pouvez ajouter de l'interactivité à vos applications en tirant parti des événements et des commandes. Ce guide introduira les événements et les commandes avec des exemples simples.

## Gestion des événements

Les événements dans Avalonia fournissent un moyen de répondre aux interactions des utilisateurs et de contrôler des actions spécifiques. Vous pouvez gérer les événements en suivant ces étapes :

1. **Implémentez le gestionnaire d'événements** : Écrivez un gestionnaire d'événements dans le [code-behind](../user-interface/code-behind.md) qui sera exécuté lorsque l'événement est déclenché. Le gestionnaire d'événements doit contenir la logique que vous souhaitez exécuter en réponse à l'événement.

```csharp title='MainWindow.axaml.cs'
public partial class MainWindow : Window
{
    public MainWindow()
    {
        InitializeComponent();
    }

    // highlight-start
    private void HandleButtonClick(object sender, RoutedEventArgs e)
    {
        // La logique de gestion des événements va ici
    }
    // highlight-end
}
```

2. **Abonnez-vous à l'événement** : Identifiez l'événement que vous souhaitez gérer dans votre contrôle. La plupart des contrôles dans Avalonia exposent divers événements, tels que `Click` ou `SelectionChanged`. Abonnez-vous à l'événement dans le XAML en localisant le contrôle et en ajoutant un attribut avec le nom de l'événement et une valeur indiquant le nom de la méthode du gestionnaire d'événements.

```xml title='MainWindow.axaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="AvaloniaApplication1.Views.MainWindow">
  // highlight-next-line
  <Button Name="myButton" Content="Cliquez Moi" Click="HandleButtonClick" />
</Window>
```

L'exemple ci-dessus ajoute un gestionnaire appelé `HandleButtonClick` à l'événement `Click` d'un `Button`.

## Utilisation des Commandes

Les commandes dans Avalonia offrent une approche de niveau supérieur pour gérer les interactions utilisateur, découplant l'action utilisateur de la logique d'implémentation. Alors que les événements sont définis dans le code-behind d'un contrôle, les commandes sont généralement liées à une propriété ou à une méthode sur le [contexte de données](../data/data-binding/data-context.md).

:::info
Les commandes sont disponibles dans tous les contrôles qui fournissent une propriété `Command`. La commande est généralement déclenchée lorsque le principal moyen d'interaction du contrôle se produit, par exemple un clic sur un bouton.
:::

La façon la plus simple d'utiliser des commandes est de les lier à une méthode dans le contexte de données de l'objet.

1. **Ajouter une méthode au modèle de vue** : Définir une méthode dans un modèle de vue qui gérera la commande.

```csharp title='C#'
    public class MainWindowViewModel
    {
        // highlight-start
        public bool HandleButtonClick()
        {
            // Logique de gestion des événements ici
        }
        // highlight-end
    }
    ```

2. **Lier la Méthode** : Associer la méthode au contrôle qui la déclenche.

    ```xml title='XAML'
    <Button Content="Cliquez Moi" Command="{Binding HandleButtonClick}" />
    ```
