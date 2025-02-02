---
id: how-to-bind-to-a-command-with-reactiveui
title: Comment se lier à une Commande avec ReactiveUI
---

import BindReactiveCommandScreenshot from '/img/guides/data-binding/bind-reactivecommand.gif';

# Comment se lier à une Commande avec ReactiveUI

Ce guide vous montre comment lier une méthode de modèle de vue (qui effectue une action) à un contrôle qui peut initier une action en réponse à l'interaction de l'utilisateur (par exemple, un bouton). Cette liaison est définie en XAML en utilisant l'attribut `Command`, par exemple :

```xml
<Window xmlns="https://github.com/avaloniaui">
    ...
  <StackPanel Margin="20">
      <Button Command="{Binding ExampleCommand}">Exécuter l'exemple</Button>
  </StackPanel>
```

Ce guide suppose que vous utilisez le modèle de mise en œuvre MVVM, et que vous avez basé votre modèle de vue sur le framework _ReactiveUI_.

:::info
Pour revoir le concept derrière le modèle de mise en œuvre MVVM, voir [ici](../../concepts/the-mvvm-pattern/).
:::

Si vous avez créé votre application en utilisant le modèle de solution **Avalonia MVVM Application**, alors votre solution contiendra déjà le package du framework _ReactiveUI_, et vous pouvez y faire référence comme ceci :

```csharp
using ReactiveUI;
```

Un modèle de vue qui peut effectuer des actions les implémente via l'interface `ICommand`. Le cadre _ReactiveUI_ fournit la classe `ReactiveCommand` qui implémente `ICommand`.

:::info
Pour des détails sur la définition de l'interface `ICommand`, voir [ici](https://docs.microsoft.com/en-gb/dotnet/api/system.windows.input.icommand?view=netstandard-2.0).
:::

La liaison de données de l'attribut `Command` appellera la méthode du modèle de vue liée via son interface `ICommand.Execute`, lorsque le contrôle lié est activé. Dans cet exemple : lorsque le bouton est cliqué.

Pour créer un modèle de vue avec un `ReactiveCommand`, suivez cet exemple :

- Dans votre modèle de vue, déclarez une commande, comme ceci :

```csharp
public ReactiveCommand<Unit, Unit> ExampleCommand { get; } 
```


-  Créez une méthode privée dans le modèle de vue pour effectuer l'action.
-  Initialisez la commande réactive, en passant le nom de la méthode qui exécute l'action.

Votre code de modèle de vue ressemblera maintenant à ceci :

```csharp
namespace AvaloniaGuides.ViewModels
{
    public class MainWindowViewModel 
    {
        public ReactiveCommand<Unit, Unit> ExampleCommand { get; }

        public MainWindowViewModel()
        {
            ExampleCommand = ReactiveCommand.Create(PerformAction);
        }
        private void PerformAction()
        {
            Debug.WriteLine("L'action a été appelée.");
        }
    }
}
```

-  Exécutez l'application et surveillez la sortie de débogage.

Lorsque le contrôle lié à la commande réactive est activé (dans cet exemple : lorsque le bouton est cliqué) ; alors la méthode privée pour effectuer l'action est appelée par le biais de la commande réactive.

<img src={BindReactiveCommandScreenshot} alt=""/>

## Paramètre de Commande

Vous aurez souvent besoin de passer un argument à la commande réactive qui est liée à un contrôle. Vous pouvez y parvenir en utilisant l'attribut `CommandParameter` dans le XAML. Par exemple :

```xml
<Window xmlns="https://github.com/avaloniaui">
   ...
   <StackPanel Margin="20">
      <Button Command="{Binding ExampleCommand}"
              CommandParameter="Depuis le bouton">Exécuter l'exemple</Button>
   </StackPanel>
</Window>
```

Vous devez maintenant modifier le modèle de vue afin que la commande réactive s'attende à un paramètre de type chaîne, l'initialisation s'attende à un paramètre de type chaîne, et la méthode privée pour effectuer l'action s'attende à un paramètre de type chaîne. Comme suit :

```csharp
namespace AvaloniaGuides.ViewModels
{
    public class MainWindowViewModel 
    {
        public ReactiveCommand<string, Unit> ExampleCommand { get; }

        public MainWindowViewModel()
        {
            ExampleCommand = ReactiveCommand.Create<string>(PerformAction);
        }
        private void PerformAction(string msg)
        {
            Debug.WriteLine($"L'action a été appelée. {msg}");
        }
    }
}
```

Notez qu'aucune conversion de type n'est effectuée sur l'attribut `CommandParameter`, donc si vous devez utiliser un paramètre de type qui n'est pas une chaîne, vous devez définir le type dans le XAML. Vous devrez également utiliser la syntaxe XAML étendue pour le paramètre.

Par exemple, pour passer un paramètre entier :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:sys="clr-namespace:System;assembly=mscorlib">
 ...   
    <Button Command="{Binding ExampleIntegerCommand}">
        <Button.CommandParameter>
            <sys:Int32>42</sys:Int32>
        </Button.CommandParameter>
        Quelle est la réponse ?
    </Button>
</Window>
```

:::danger
Vous obtiendrez une erreur si vos définitions de paramètres sont manquantes ou ne sont pas du type correct.
:::

:::info
Comme toute autre propriété, le paramètre de commande peut être lié.
:::
