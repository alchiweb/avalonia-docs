---
description: CONCEPTS
---

import ReactiveUINuGetScreenshot from '/img/concepts/reactiveui/reactiveui-nuget.png';

# ReactiveUI

:::tip
ReactiveUI est utilisé dans nos exemples, mais ce n'est pas obligatoire. Avalonia prend en charge tout framework MVVM ou vos propres solutions personnalisées.
:::

Ces pages expliquent comment _Avalonia UI_ utilise une version du framework open-source _ReactiveUI_ pour faciliter la mise en œuvre du modèle MVVM dans votre application.

_ReactiveUI_ est un framework avancé, composable et fonctionnel de modèle-vue-vue-modèle (MVVM) réactif pour toutes les plateformes .NET. Il a été inspiré par le paradigme de la programmation réactive fonctionnelle.

:::info
Pour un contexte technique complet sur la programmation réactive fonctionnelle, consultez l'article de Wikipédia [ici](https://en.wikipedia.org/wiki/Functional\_reactive\_programming).
:::

_Avalonia UI_ est livré avec son propre fork de _ReactiveUI_ dans le package _NuGet_ `Avalonia.ReactiveUI`.

<img src={ReactiveUINuGetScreenshot} alt=""/>

Pour utiliser _ReactiveUI_ et le modèle MVVM dans votre application _Avalonia UI_, ajoutez le package à votre projet en utilisant le gestionnaire de packages _NuGet_ (comme ci-dessus), ou exécutez la commande CLI suivante :

```bash
dotnet add package Avalonia.ReactiveUI
```

:::info
Pour des informations détaillées sur _ReactiveUI_ lui-même, consultez le site web [https://reactiveui.net/](https://reactiveui.net/)
:::

:::info
Pour en savoir plus sur le modèle MVVM, consultez l'article _Microsoft_ [ici](https://msdn.microsoft.com/en-us/library/hh848246.aspx).
:::

Le package comprend des helpers spécifiquement pour _Avalonia UI_ pour gérer les tâches _ReactiveUI_ de routage basé sur le modèle de vue, d'activation de vue et de planification. (voir la référence ci-dessus pour des détails complets sur ces tâches).

:::info
Si vous démarrez votre application à partir du modèle de solution d'application Avalonia MVVM ; vous aurez déjà le package _ReactiveUI_ installé et configuré.
:::

## Configurer pour utiliser ReactiveUI

Après avoir installé le package _NuGet_, vous devez configurer la classe `Program` de l'application pour l'utiliser. Vérifiez que vous appelez la méthode `UseReactiveUI()` dans le code `AppBuilder`.

Par exemple, si vous utilisez le modèle de solution d'application Avalonia MVVM, il ajoutera automatiquement le package _NuGet_, puis ajoutera le code :

```csharp
classe interne Program
{
    // Code d'initialisation. N'utilisez aucune API Avalonia, de tiers ou tout
    // code dépendant de SynchronizationContext avant que AppMain ne soit appelé : les choses ne sont pas initialisées
    // encore et certaines choses pourraient ne pas fonctionner.
    [STAThread]
    public static void Main(string[] args) => BuildAvaloniaApp()
        .StartWithClassicDesktopLifetime(args);

    // Configuration d'Avalonia, ne pas supprimer ; également utilisé par le concepteur visuel.
    public static AppBuilder BuildAvaloniaApp()
        => AppBuilder.Configure<App>()
            .UsePlatformDetect()
            .LogToTrace()
            .UseReactiveUI();
}
```

Dans les pages suivantes, vous apprendrez comment _ReactiveUI_ fonctionne avec _Avalonia UI_ pour vous permettre de mettre en œuvre les scénarios d'application suivants :

* Liaison de données à un modèle de vue réactif
* Activation de la vue
* Routage
* Persistance des données
* Liaison aux données triées/filtrées