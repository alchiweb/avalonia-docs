---
description: CONCEPTS
---

# Le TopLevel

Le TopLevel agit comme la racine visuelle et est la classe de base pour tous les contrôles de niveau supérieur, par exemple, `Window`. Il gère la planification de la mise en page, le style et le rendu, ainsi que le suivi de la taille du client. La plupart des services sont accessibles via le TopLevel.

## Obtenir le TopLevel

Voici deux façons courantes d'accéder à une instance de TopLevel.

### Utiliser TopLevel.GetTopLevel

Vous pouvez utiliser la méthode statique `GetTopLevel` de la classe TopLevel pour obtenir le contrôle de niveau supérieur qui contient le contrôle actuel.

```cs
var topLevel = TopLevel.GetTopLevel(control);
// Ici, vous pouvez référencer divers services comme le Presse-papiers ou le StorageProvider à partir de l'instance topLevel.
```

Cette méthode peut être utile si vous travaillez dans un contrôle utilisateur ou un composant de niveau inférieur et que vous avez besoin d'accéder aux services TopLevel.

:::note
Si `TopLevel.GetTopLevel` renvoie null, il est probable que le contrôle ne soit pas encore attaché à la racine. Pour vous assurer que le contrôle est attaché, vous devez gérer les événements `Control.Loaded` et `Control.Unloaded` et suivre le niveau supérieur actuel à partir de ces événements.
:::

### Utiliser la classe Window

Puisque la classe `Window` hérite de `TopLevel`, vous pouvez accéder directement aux services à partir d'une instance de `Window` :

```cs
var topLevel = window;
```

Cette méthode est généralement utilisée lorsque vous travaillez déjà dans le contexte d'une fenêtre, comme dans un ViewModel ou un gestionnaire d'événements au sein de la classe `Window`.

## Propriétés courantes

### ActualTransparencyLevel

Obtient le `WindowTransparencyLevel` atteint que la plateforme a pu fournir.

```cs
WindowTransparencyLevel ActualTransparencyLevel { get; }
```

### ClientSize

Obtient la taille du client de la fenêtre.

```cs
Size ClientSize { get; }
```

### Clipboard

Obtient l'implémentation du [Clipboard](./services/clipboard) de la plateforme.

```cs
IClipboard? Clipboard { get; }
```

### FocusManager

Obtient le [gestionnaire de focus](./services/focus-manager) de la racine.

```cs
IFocusManager? FocusManager { get; }
```

### FrameSize

Obtient la taille totale du niveau supérieur y compris le cadre système s'il est présenté.

```cs
Size? FrameSize { get; }
```

### InsetsManager

Obtient l'implémentation de l'[InsetsManager](./services/insets-manager) de la plateforme.

```cs
IInsetsManager? InsetsManager { get; }
```

### PlatformSettings

Représente un contrat pour accéder aux [paramètres spécifiques à la plateforme](./services/platform-settings) de niveau supérieur.

```cs
IPlatformSettings? PlatformSettings { get; }
```

### RendererDiagnostics

Obtient une valeur indiquant si le rendu doit dessiner des diagnostics spécifiques.

```cs
RendererDiagnostics RendererDiagnostics { get; }
```

### RenderScaling

Obtient le facteur d'échelle à utiliser dans le rendu.

```cs
double RenderScaling { get; }
```

### RequestedThemeVariant

Obtient ou définit la variante de thème UI utilisée par le contrôle (et ses éléments enfants) pour la détermination des ressources. Le thème UI que vous spécifiez avec ThemeVariant peut remplacer le ThemeVariant au niveau de l'application.

```cs
ThemeVariant? RequestedThemeVariant { get; set; }
```

### StorageProvider

Service de [stockage de système de fichiers](./services/storage-provider/) utilisé pour les sélecteurs de fichiers et les signets.

```cs
IStorageProvider StorageProvider { get; }
```

### TransparencyBackgroundFallback

Obtient ou définit le `IBrush` avec lequel la transparence se mélangera lorsque la transparence n'est pas prise en charge. Par défaut, il s'agit d'un pinceau blanc solide.

```cs
IBrush TransparencyBackgroundFallback { get; set; }
```

### TransparencyLevelHint

Obtient ou définit le `WindowTransparencyLevel` que le TopLevel doit utiliser lorsque cela est possible. Accepte plusieurs valeurs qui sont appliquées dans un ordre de secours. Par exemple, avec "Mica, Blur", Mica sera appliqué uniquement sur les plateformes où cela est possible, et Blur sera utilisé sur le reste d'entre elles. La valeur par défaut est un tableau vide ou "None".

```cs
IReadOnlyList<WindowTransparencyLevel> TransparencyLevelHint { get; set; }
```

## Événements Communs

### BackRequested

Se produit lorsque le bouton de retour physique est pressé ou qu'une navigation vers l'arrière a été demandée.

```cs
event EventHandler<RoutedEventArgs> BackRequested { add; remove; }
```

### Closed

Se déclenche lorsque la fenêtre est fermée.

```cs
event EventHandler Closed;
```

### Opened

Se déclenche lorsque la fenêtre est ouverte.

```cs
event EventHandler Opened;
```

### ScalingChanged

Se produit lorsque l'échelle du TopLevel change.

```cs
event EventHandler ScalingChanged;
```

## Méthodes Communes

### GetTopLevel

Obtient le `TopLevel` dans lequel le `Visual` donné est hébergé.

#### Paramètres

`contrôle`
Le visuel pour interroger son TopLevel

```cs
static TopLevel? GetTopLevel(Visual? visual)
```

### RequestAnimationFrame

Met en file d'attente un rappel à appeler lors de la prochaine animation

```cs
void RequestAnimationFrame(Action<TimeSpan> action)
```

### RequestPlatformInhibition

Demande qu'un `PlatformInhibitionType` soit inhibé. Le comportement reste inhibé jusqu'à ce que la valeur de retour soit disposée. L'ensemble disponible de `PlatformInhibitionType` dépend de la plateforme. Si un comportement est inhibé sur une plateforme où ce type n'est pas pris en charge, la demande n'aura aucun effet.

```cs
async Task<IDisposable> RequestPlatformInhibition(PlatformInhibitionType type, string reason)
```

### TryGetPlatformHandle

Essaye d'obtenir le handle de la plateforme pour le contrôle dérivé de TopLevel.

```cs
IPlatformHandle? TryGetPlatformHandle()
```

## Plus d'informations

Voir le code source sur _GitHub_ [`TopLevel.cs`](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Controls/TopLevel.cs)