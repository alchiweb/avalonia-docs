---
id: platform-settings
title: Paramétrage de la plateforme
---

La classe `PlatformSettings` représente un contrat pour accéder aux paramètres et informations spécifiques à la plateforme. Certains de ces paramètres peuvent être modifiés par l'utilisateur globalement dans le système d'exploitation à l'exécution.

Les `PlatformSettings` peuvent être accédés via une instance de `TopLevel` ou `Window`, pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../toplevel) :

```cs
var platformSettings = window.PlatformSettings;
```

## Méthodes

### GetTapSize(PointerType type)
Retourne la taille du rectangle autour de l'emplacement d'un pointeur enfoncé dans lequel un pointeur relâché doit se produire pour enregistrer un geste de tap, en pixels indépendants du dispositif.

```cs 
Size GetTapSize(PointerType type);
```

### GetDoubleTapSize(PointerType type)
Retourne la taille du rectangle autour de l'emplacement d'un pointeur enfoncé dans lequel un pointeur relâché doit se produire pour enregistrer un geste de double-tap, en pixels indépendants du dispositif.

```cs
Size GetDoubleTapSize(PointerType type);
```

### GetDoubleTapTime(PointerType type)
Retourne le temps maximum qui peut s'écouler entre le premier et le second clic d'un geste de double-tap.

```cs
TimeSpan GetDoubleTapTime(PointerType type);
```

### GetColorValues()
Retourne les valeurs de couleur système actuelles, y compris le mode sombre et les couleurs d'accent.

```cs
PlatformColorValues GetColorValues();
```

:::tip
Bien que le thème Fluent intégré prenne en charge le changement automatique entre les couleurs d'accent, cette méthode est utile pour appliquer une logique personnalisée avec les paramètres de couleur du système d'exploitation.
:::

## Propriétés

### HoldWaitDuration
La durée entre l'appui sur le pointeur et le moment où l'événement `Holding` est déclenché.

```cs
TimeSpan HoldWaitDuration { get; }
```

### HotkeyConfiguration
La configuration des raccourcis spécifiques à la plateforme dans une application Avalonia.

```cs
PlatformHotkeyConfiguration HotkeyConfiguration { get; }
```

:::tip
HotkeyConfiguration est particulièrement utile lorsque l'application doit gérer des gestes bien connus tels que Copier, Coller ou Couper.
:::

```cs
protected override void OnKeyDown(KeyEventArgs e)
{
    var hotkeys = TopLevel.GetTopLevel(this).PlatformSettings.HotkeyConfiguration;
    if (hotkeys.Copy.Any(g => g.Matches(e)))
    {
        // Gérer le raccourci Copier.
    }
}
```


## Événements

### ColorValuesChanged
Déclenché lorsque les valeurs de couleur du système actuel sont modifiées. Cela inclut les changements de mode sombre et de couleurs d'accent.

```cs
event EventHandler<PlatformColorValues>? ColorValuesChanged;
```

Utilisez l'interface `IPlatformSettings` pour adapter le comportement de votre application aux paramètres spécifiques à la plateforme de l'utilisateur.
