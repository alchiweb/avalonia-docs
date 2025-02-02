---
id: accessing-the-ui-thread
title: Comment accéder au thread UI
---

# Comment accéder au thread UI

Ce guide vous montrera comment accéder au thread UI dans votre application _Avalonia UI_.

Les applications _Avalonia UI_ ont un thread principal, qui gère l'interface utilisateur. Lorsque vous avez un processus intensif ou de longue durée, vous choisirez généralement de l'exécuter sur un thread différent. Vous pouvez alors avoir des scénarios où vous souhaitez mettre à jour le thread principal de l'interface utilisateur (par exemple avec des mises à jour de progression).

Un dispatcher fournit des services pour gérer les éléments de travail sur un thread spécifique. Dans _Avalonia UI_, vous disposez déjà du dispatcher qui gère le thread UI. Lorsque vous devez mettre à jour l'UI depuis un thread différent, vous y accédez via ce dispatcher, comme suit :

```csharp
Dispatcher.UIThread
```

Vous pouvez utiliser soit la méthode `Post`, soit la méthode `InvokeAsync` pour exécuter un processus sur le thread UI.

Utilisez `Post` lorsque vous souhaitez simplement démarrer un travail, mais que vous n'avez pas besoin d'attendre que le travail soit terminé, et que vous n'avez pas besoin du résultat : c'est la méthode de dispatcher 'fire-and-forget'.

Utilisez `InvokeAsync` lorsque vous devez attendre le résultat, et que vous souhaitez potentiellement recevoir le résultat.

## Priorité du Dispatcher

Les deux méthodes ci-dessus ont un paramètre de priorité de dispatcher. Vous pouvez l'utiliser avec l'énumération `DispatcherPriority` pour spécifier la priorité de la file d'attente que le travail donné doit recevoir.

:::info
Pour les valeurs possibles de l'énumération `DispatcherPriority`, voir [ici](http://reference.avaloniaui.net/api/Avalonia.Threading/DispatcherPriority/).
:::

## Exemple

Cet exemple montre comment accéder au thread UI depuis un thread de travail pour mettre à jour ou obtenir le texte d'un TextBlock. Créez un nouveau projet Avalonia et remplacez le contenu des deux fichiers suivants :

MainView.axaml:
```xml title='XAML'
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             xmlns:vm="clr-namespace:AvaloniaApplication1.ViewModels"
             mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
             x:Class="AvaloniaApplication1.Views.MainView"
             x:DataType="vm:MainViewModel">
  <Design.DataContext>
    <!-- Cela définit uniquement le DataContext pour le visualiseur dans un IDE,
         pour définir le DataContext réel pour l'exécution, définissez la propriété DataContext dans le code (regardez App.axaml.cs) -->
    <vm:MainViewModel />
  </Design.DataContext>

	<StackPanel Margin="20">
		<TextBlock Name="TextBlock1" />
	</StackPanel>
</UserControl>
```

MainView.axaml.cs:
```csharp title='MainView C#'
using Avalonia.Controls;
using Avalonia.Threading;
using System;
using System.Threading.Tasks;

namespace AvaloniaApplication1.Views;

public partial class MainView : UserControl
{
    public MainView()
    {
        InitializeComponent();

        // Exécutez OnTextFromAnotherThread dans le pool de thread
        // pour démontrer comment accéder au thread UI depuis là.
        _ = Task.Run(() => OnTextFromAnotherThread("test"));
    }

    private void SetText(string text) => TextBlock1.Text = text;
    private string GetText() => TextBlock1.Text ?? "";

    private async void OnTextFromAnotherThread(string text)
    {
        try
        {
            // Démarrer le travail sur le thread UI et retourner immédiatement.
            Dispatcher.UIThread.Post(() => SetText(text));

            // Démarrer le travail sur le thread UI et attendre le résultat.
            var result = await Dispatcher.UIThread.InvokeAsync(GetText);

            // Cette invocation provoquerait une exception car nous sommes
            // en cours d'exécution sur un thread de travail :
            // System.InvalidOperationException: 'Appel depuis un thread invalide'
            //SetText(text);
        }
        catch (Exception)
        {
            throw; // Todo: Gérer l'exception.
        }
    }
}

```

## Plus d'informations

:::info
Pour la documentation API complète sur le dispatcher, voir [ici](http://reference.avaloniaui.net/api/Avalonia.Threading/Dispatcher/).
:::

:::info
Voir le code source sur _GitHub_ [`Dispatcher.cs`](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Base/Threading/Dispatcher.cs)
:::
