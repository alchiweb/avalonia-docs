---
description: CONCEPTS
---

# Durées de vie des applications

Toutes les plateformes ne sont pas créées égales ! Par exemple, la gestion de la durée de vie que vous pouvez utiliser pour le développement avec Windows Forms ou WPF ne peut fonctionner que sur des plateformes de type bureau. _Avalonia UI_ est un framework multiplateforme ; donc pour rendre votre application portable, il fournit plusieurs modèles de durée de vie différents pour votre application, et vous permet également de tout contrôler manuellement si la plateforme cible le permet.

## Comment fonctionnent les durées de vie ?

Pour une application de bureau, vous l'initialisez comme ceci :

```csharp
class Program
{
  // Cette méthode est nécessaire pour l'infrastructure du prévisualiseur IDE
  public static AppBuilder BuildAvaloniaApp() 
    => AppBuilder.Configure<App>().UsePlatformDetect();

  // Le point d'entrée. Les choses ne sont pas encore prêtes, donc à ce stade
  // vous ne devriez pas utiliser de types Avalonia ou quoi que ce soit qui s'attend
  // à ce qu'un SynchronizationContext soit prêt
  public static int Main(string[] args) 
    => BuildAvaloniaApp().StartWithClassicDesktopLifetime(args);
}
```

Ensuite, la fenêtre principale est créée dans la classe `Application` :

```csharp
public override void OnFrameworkInitializationCompleted()
{
  if (ApplicationLifetime 
                  is IClassicDesktopStyleApplicationLifetime desktop)
    desktop.MainWindow = new MainWindow();
  else if (ApplicationLifetime 
                  is ISingleViewApplicationLifetime singleView)
    singleView.MainView = new MainView();
  base.OnFrameworkInitializationCompleted();
}
```

Cette méthode est appelée lorsque le framework a été initialisé et que la propriété `ApplicationLifetime` contient la durée de vie choisie, le cas échéant.

:::info
Si vous exécutez l'application en mode conception (cela utilise le processus de prévisualisation de l'IDE), alors `ApplicationLifetime` est nul.
:::

## Interfaces de Durée de Vie

_Avalonia UI_ fournit une gamme d'interfaces pour vous permettre de choisir un niveau de contrôle adapté à votre application. Celles-ci sont fournies par la famille de méthodes `BuildAvaloniaApp().Start[Something]`.

### IControlledApplicationLifetime

Fournie par :

* `StartWithClassicDesktopLifetime`
* `StartLinuxFramebuffer`

Vous permet de vous abonner aux événements `Startup` et `Exit` et permet d'arrêter explicitement l'application en appelant la méthode `Shutdown`. Cette interface vous donne le contrôle des procédures de sortie de l'application.

### IClassicDesktopStyleApplicationLifetime

Hérite de : `IControlledApplicationLifetime`

Fournie par :

* `StartWithClassicDesktopLifetime`

Vous permet de contrôler la durée de vie de votre application de la manière d'une application Windows Forms ou WPF. Cette interface fournit un moyen d'accéder à la liste des fenêtres actuellement ouvertes, de définir une fenêtre principale, et possède trois modes d'arrêt :

* `OnLastWindowClose` - arrête l'application lorsque la dernière fenêtre est fermée
* `OnMainWindowClose` - arrête l'application lorsque la fenêtre principale est fermée (si elle a été définie).
* `OnExplicitShutdown` - désactive l'arrêt automatique de l'application, vous devez appeler la méthode `Shutdown` dans votre code.

### ISingleViewApplicationLifetime

Fournie par :

* `StartLinuxFramebuffer`
* plateformes mobiles
* plateforme web (WebAssembly/WASM)

Certaines plateformes n'ont pas de concept de fenêtre principale de bureau et n'autorisent qu'une seule vue à l'écran de l'appareil à la fois. Pour ces plateformes, le cycle de vie vous permet de définir et de changer la classe de la vue principale (`MainView`) à la place.

:::info
Pour implémenter la pile de navigation sur des plateformes comme celle-ci (avec une seule vue principale), vous pouvez utiliser le routage [_ReactiveUI_](https://www.reactiveui.net/docs/handbook/routing/) ou un autre contrôle de routage.
:::

## Gestion manuelle du cycle de vie

Si vous en avez besoin, vous pouvez prendre le contrôle total de la gestion du cycle de vie de votre application. Par exemple, sur une plateforme de bureau, vous pouvez passer un délégué à `AppMain` à la méthode `BuildAvaloniaApp.Start`, puis gérer les choses manuellement à partir de là :

```csharp
class Program
{
  // Cette méthode est nécessaire pour l'infrastructure de prévisualisation de l'IDE
  public static AppBuilder BuildAvaloniaApp() 
    => AppBuilder.Configure<App>().UsePlatformDetect();

  // Le point d'entrée. Les choses ne sont pas encore prêtes, donc à ce stade
  // vous ne devriez pas utiliser de types Avalonia ou quoi que ce soit qui attend
  // qu'un SynchronizationContext soit prêt
  public static int Main(string[] args) 
    => BuildAvaloniaApp().Start(AppMain, args);

  // Point d'entrée de l'application. Avalonia est complètement initialisé.
  static void AppMain(Application app, string[] args)
  {
     // Une source de jeton d'annulation qui sera 
     // utilisée pour arrêter la boucle principale
     var cts = new CancellationTokenSource();

     // Mettez votre code de démarrage ici
     new Window().Show();

     // Démarrez la boucle principale
     app.Run(cts.Token);
  }
}
```