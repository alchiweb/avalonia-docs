---
description: CONCEPTS
---

# Exceptions Non Gérées

_Avalonia UI_ n'offre aucun mécanisme pour gérer les exceptions globalement et les marquer comme gérées. La raison en est qu'on ne peut pas savoir si l'exception a été gérée correctement et donc l'application peut être dans un état invalide. Il est donc fortement recommandé de gérer les exceptions localement si celles-ci peuvent être gérées par votre application. Cela dit, il est toujours bon de consigner toute exception non gérée pour un support et un débogage ultérieurs.

## Journalisation

Nous recommandons de consigner les exceptions dans la console, un fichier ou ailleurs. Il existe plusieurs bibliothèques de journalisation disponibles, par exemple [Serilog](https://serilog.net) et [NLog](https://nlog-project.org).

## Le bloc try-catch global

Vous pouvez attraper n'importe quelle exception du thread principal, qui est également le thread de l'interface utilisateur dans _Avalonia UI_, dans votre fichier `Program.cs`. Pour ce faire, il suffit d'encapsuler l'ensemble de `void Main` dans un bloc `try` et `catch`. Dans le bloc `catch`, vous pouvez enregistrer l'erreur, informer l'utilisateur, envoyer le fichier journal ou redémarrer l'application.

```csharp
// Fichier : Program.cs

public static void Main(string[] args)
{
    try
    {
        // préparez et exécutez votre application ici
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);
    }
    catch (Exception e)
    {
        // ici nous pouvons travailler avec l'exception, par exemple l'ajouter à notre fichier journal
        Log.Fatal(e, "Quelque chose de très mauvais s'est produit");
    }
    finally
    {
        // Ce bloc est optionnel. 
        // Utilisez le bloc finally si vous devez nettoyer ou faire quelque chose de similaire
        Log.CloseAndFlush();
    }
}
```

## Exceptions d'un autre Thread

Si vous utilisez des `Task`s pour exécuter du travail de manière asynchrone, vous pouvez configurer `TaskScheduler.UnobservedTaskException`. Pour plus d'informations, consultez la [documentation Microsoft .NET](https://docs.microsoft.com/dotnet/api/system.threading.tasks.taskscheduler.unobservedtaskexception).

## Exceptions de Reactive UI

Si vous utilisez Avalonia avec [ReactiveUI](../concepts/the-mvvm-pattern/avalonia-ui-and-mvvm#reactiveui), vous pouvez vous abonner à leur `RxApp.DefaultExceptionHandler`. Pour plus d'informations, veuillez vous référer à [Gestionnaire d'exception par défaut de ReactiveUI](https://www.reactiveui.net/docs/handbook/default-exception-handler/).

Notez que `RxApp.DefaultExceptionHandler` doit être défini avant la création de toute ReactiveCommand. Sinon, le gestionnaire personnalisé ne sera pas utilisé. Vous pouvez le définir dans le point d'entrée de votre application ou avant la création de toute vue ou fenêtre Avalonia.
