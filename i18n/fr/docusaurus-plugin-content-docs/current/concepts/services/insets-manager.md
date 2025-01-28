---
id: insets-manager
title: Gestionnaire d'insets
---

Le `InsetsManager` vous permet d'interagir avec les barres système de la plateforme et de gérer les changements de la zone sécurisée de la fenêtre mobile.

Le `InsetsManager` peut être accédé via une instance de `TopLevel` ou `Window`. Pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../toplevel) :

```cs
var insetsManager = TopLevel.GetTopLevel(control).InsetsManager;
```

:::note
Pour l'instant, ce service n'a d'implémentation que sur les backends mobiles et de navigateur. Si vous devez ajuster les décorations de fenêtre de bureau, veuillez utiliser les propriétés `Window.ExtendClientAreaToDecorationsHint`, `Window.ExtendClientAreaChromeHints` et `Window.ExtendClientAreaTitleBarHeightHint`.
:::

:::note
À partir d'Avalonia 11.1, toute application Avalonia ajustera automatiquement sa vue racine en fonction des valeurs d'insets. Ce comportement peut être désactivé en définissant la valeur de propriété attachée `TopLevel.AutoSafeAreaPadding="False"` sur la vue racine.
:::

## Propriétés

### IsSystemBarVisible
Obtient ou définit une valeur indiquant si les barres du système sont visibles. Renvoie null si la plateforme ne prend pas en charge l'affichage ou la dissimulation des barres du système.

```cs
bool? IsSystemBarVisible { get; set; }
```

### DisplayEdgeToEdge
Obtient ou définit une valeur indiquant si la fenêtre doit être dessinée bord à bord derrière les barres du système visibles.

```cs
bool DisplayEdgeToEdge { get; set; }
```

### SafeAreaPadding
Obtient le décalage de la zone de sécurité actuel. La zone de sécurité représente la partie de la fenêtre qui n'est pas obscurcie par les barres du système.

```cs
Thickness SafeAreaPadding { get; }
```

### SystemBarColor
Obtient ou définit la couleur des barres du système de la plateforme. Renvoie null si la plateforme ne prend pas en charge la définition de la couleur de la barre du système.

```cs
Color? SystemBarColor { get; set; }
```

## Événements

### SafeAreaChanged
Se produit lorsque la zone de sécurité pour la fenêtre actuelle change. Cela peut se produire lorsque les barres du système sont affichées ou masquées, ou lorsque la taille ou l'orientation de la fenêtre change.

```cs
event EventHandler<SafeAreaChangedArgs>? SafeAreaChanged;
```

---

# SafeAreaChangedArgs
SafeAreaChangedArgs est une classe qui fournit des données pour l'événement SafeAreaChanged.

## Properties 

### SafeAreaPadding
Obtient le nouveau décalage de la zone de sécurité.

```
public Thickness SafeAreaPadding { get; }
```

---

# SystemBarTheme
ThèmeBarreSystème est une énumération avec des valeurs qui représentent des thèmes clairs et foncés pour la barre du système.

## Values

### Light
La barre du système a un fond clair et un premier plan foncé.

### Dark
La barre du système a un fond foncé et un premier plan clair.

## Compatibilité avec les différentes plateformes :

| Fonctionnalité        | Windows | macOS | Linux | Browser | Android |  iOS | Tizen |
|---------------|-------|-------|-------|-------|-------|-------|-------|
| `IsSystemBarVisible` | ✖ | ✖ | ✖ | ✔* | ✔ | ✔ | ✖ |
| `DisplayEdgeToEdge` | ✖ | ✖ | ✖ | ✖  | ✔ | ✔ | ✖ |
| `SafeAreaPadding` | ✖ | ✖ | ✖ | ✔* | ✔ | ✔ | ✖ |
| `SystemBarColor` | ✖ | ✖ | ✖ | ✖ | ✔ | ✖ | ✖ |
| `SafeAreaChanged` | ✖ | ✖ | ✖ | ✔* | ✔ | ✔ | ✖ |

\* - seuls les navigateurs mobiles Chromium prennent en charge l'API IInsetsManager.