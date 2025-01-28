---
id: index
title: Plateforme Headless
---

## Introduction

La plateforme headless dans AvaloniaUI offre la possibilité d'exécuter des applications Avalonia sans interface graphique visible (GUI). Cela permet des scénarios de test et d'automatisation sur des systèmes dépourvus d'environnement graphique, tels que les serveurs ou les environnements d'intégration continue/déploiement continu (CI/CD).

En utilisant la plateforme headless, vous pouvez effectuer des tests d'interface utilisateur, exécuter des scénarios d'application et valider des fonctionnalités dans un environnement headless, ce qui permet de gagner du temps et des ressources par rapport aux tests manuels.

## Prise en main

Bien que la plateforme headless puisse être initialisée sans dépendances, pour plus de commodité, nous avons créé des packages d'intégration pour les plateformes de tests unitaires courantes :

- [Tests headless avec XUnit](./headless-xunit)
- [Tests headless avec NUnit](./headless-nunit)
- Si vous utilisez une autre plateforme ou avez besoin de plus de contrôle : [Configuration manuelle de la plateforme headless](./headless-custom)

## Simulation de l'entrée utilisateur

Comme la plateforme headless n'a pas d'entrée réelle, chaque événement doit être déclenché depuis le test unitaire. Le package Avalonia.Headless est livré avec un certain nombre de méthodes d'aide qui peuvent être utilisées :

#### `Window.KeyPress(Key key, RawInputModifiers modifiers, PhysicalKey physicalKey, string? keySymbol)`
#### `Window.KeyPressQwerty(PhysicalKey physicalKey, RawInputModifiers modifiers)`

Simule une pression de touche sur la fenêtre headless/niveau supérieur.

#### `Window.KeyRelease(Key key, RawInputModifiers modifiers, PhysicalKey physicalKey, string? keySymbol)`
#### `Window.KeyReleaseQwerty(PhysicalKey physicalKey, RawInputModifiers modifiers)`

Simule un relâchement de clavier sur la fenêtre headless/niveau supérieur.

#### `Window.KeyTextInput(string text)`

Simule un événement de saisie de texte sur la fenêtre headless/niveau supérieur.

:::note
Cet événement est indépendant de KeyPress et KeyRelease. Si vous devez simuler une saisie de texte dans une TextBox ou un contrôle similaire, veuillez utiliser KeyTextInput.
:::

#### `Window.MouseDown(Point point, MouseButton button, RawInputModifiers modifiers)`

Simule un événement de clic de souris sur la fenêtre headless/niveau supérieur.

:::note
Dans la plateforme headless, il n'y a qu'un seul pointeur de souris. Il n'y a pas de méthodes d'aide pour l'entrée tactile ou au stylo.
:::

#### `Window.MouseMove(Point point, MouseButton button, RawInputModifiers modifiers)`

Simule un événement de déplacement de souris sur la fenêtre headless/niveau supérieur.

#### `Window.MouseUp(Point point, MouseButton button, RawInputModifiers modifiers)`

Simule un événement de relâchement de souris sur la fenêtre headless/niveau supérieur.

#### `Window.MouseWheel(Point point, Vector delta, RawInputModifiers modifiers)`

Simule un événement de molette de souris sur la fenêtre headless/niveau supérieur.

#### `Window.DragDrop(Point point, RawDragEventType type, DataObject data, DragDropEffects effects, RawInputModifiers modifiers)`

Simule un événement de glisser-déposer sur la fenêtre headless/niveau supérieur. Cet événement simule un utilisateur déplaçant des fichiers d'une autre application vers l'application actuelle.

Un simple test de clic de bouton est un exemple typique où ces méthodes peuvent être utilisées :

```csharp
// Créer une fenêtre et un bouton :
var button = new Button
{
    HorizontalAlignment = HorizontalAlignment.Stretch,
    VerticalAlignment = VerticalAlignment.Stretch
};
var window = new Window
{
    Width = 100,
    Height = 100,
    Content = button
};

// S'abonner à l'événement de clic du bouton :
var buttonClicked = false;
button.Click += (_, _) => buttonClicked = true;

// Afficher la fenêtre :
window.Show();

// Simuler des événements de souris avec un clic (souris enfoncée + relâchée) :
window.MouseDown(new Point(50, 50), MouseButton.Left);
window.MouseUp(new Point(50, 50), MouseButton.Left);

// Vérifier que le bouton a été cliqué :
Assert.True(buttonClicked);
```

:::tip
Tout comme dans toute autre application Avalonia, il est également possible de déclencher des événements directement. Par exemple, avec le clic de bouton `button.RaiseEvent(new RoutedEventArgs(Button.ClickEvent))`. Cela peut être plus pratique pour la plupart des cas d'utilisation mais manque de flexibilité avec les paramètres d'entrée. RaiseEvent déclenchera l'événement de clic sur le bouton mais une commande liée ne sera pas exécutée. Si vous devez tester une commande de bouton, vous pouvez utiliser `button.Focus()` avec `window.KeyReleaseQwerty(PhysicalKey.Space, RawInputModifiers.None)`.
:::

## Capturer le Dernier Cadre Rendu

Par défaut, la plateforme Headless ne rend rien et a plutôt un backend de dessin fictif/headless activé. Cependant, il est possible d'activer le moteur de rendu Skia et de l'utiliser pour capturer le dernier cadre rendu, qui peut être comparé à une image attendue ou utilisé d'une autre manière.

Pour activer le moteur de rendu Skia, ajustez le code AppBuilder comme suit :

```csharp title=App.axaml.cs
public static AppBuilder BuildAvaloniaApp() => AppBuilder.Configure<TestApplication>()
    .UseSkia() // activer le moteur de rendu Skia
    .UseHeadless(new AvaloniaHeadlessPlatformOptions
    {
        UseHeadlessDrawing = false // désactiver le dessin headless
    });
```

Avec le moteur de rendu réel activé, vous pouvez utiliser la méthode d'assistance `Window.CaptureRenderedFrame` :

```csharp
var window = new Window
{
    Content = new TextBlock
    {
        Text = "Hello World"
    }
};
window.Show();

var frame = window.CaptureRenderedFrame();
frame.Save("file.png");
```

:::tip
La méthode `CaptureRenderedFrame` renvoie un WriteableBitmap, vous permettant de le verrouiller et de lire les données des pixels qui peuvent être comparées en mémoire.
:::

## Simuler un Délai Utilisateur

Dans les applications UI réelles, il y a toujours un certain délai entre les interactions de l'utilisateur, ce qui donne au système d'exploitation le temps d'exécuter diverses tâches dans l'application. Ces tâches sont souvent retardées et peuvent inclure des opérations telles que le redimensionnement de la fenêtre, qui sur la plupart des plateformes (y compris Headless), est asynchrone.

Ce délai peut entraîner un comportement inattendu lorsque certaines opérations n'ont pas encore été exécutées.

La solution évidente serait d'ajouter `Task.Delay` entre ces opérations, mais une option plus efficace dans Avalonia serait d'utiliser l'API `Dispatcher`, qui permet de vider la file des tâches directement :

```csharp
var window = new Window();
window.Show();

window.Width = 100;
window.Height = 100;

// highlight-start
Dispatcher.UIThread.RunJobs();
// highlight-end

Assert.AreEqual(new Size(100, 100), window.ClientSize);
```

De plus, la plateforme headless fournit une méthode pour forcer le minuteur de rendu à s'exécuter, ce qui peut être utile dans certaines situations :

```csharp
AvaloniaHeadlessPlatform.ForceRenderTimerTick();
```

:::tip
Toutes les méthodes d'aide à l'entrée et `CaptureRenderedFrame` utilisent en interne ces méthodes de délai utilisateur, donc il n'est pas nécessaire de les exécuter deux fois !
:::