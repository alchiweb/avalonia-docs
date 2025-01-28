---
id: focus-manager
title: Gestionnaire de Focus
---

Le service `FocusManager` est responsable de la gestion du focus clavier pour l'application. Il garde une trace de l'élément actuellement focalisé et de la portée de focus actuelle.

Le `FocusManager` peut être accédé via une instance de `TopLevel` ou `Window`, pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../toplevel) :
```cs
var focusManager = window.FocusManager;
```

## Méthodes

### GetFocusedElement()
Renvoie l'élément actuellement focalisé.

```cs
IInputElement? GetFocusedElement()
```

### ClearFocus()
Efface l'élément actuellement focalisé.

```cs
void ClearFocus()
```

## Conseils

### Focaliser un contrôle

Les développeurs n'ont généralement pas besoin d'un service `FocusManager` pour focaliser un contrôle. 
Cela peut être réalisé par un appel de méthode directement sur le contrôle :
```cs
var hasFocused = button.Focus();
```

La méthode `Focus` peut renvoyer `false` si le contrôle n'est pas visible et si la propriété `Focusable` est définie sur false.

### Écouter les changements de focus globaux

Bien que la méthode `FocusManager.GetFocusedElement` permette d'obtenir le contrôle actuellement focalisé, elle n'est pas adaptée en tant qu'événement.
À la place, veuillez utiliser la méthode `InputElement.GotFocusEvent.Raised.Subscribe(handler)`. Notez qu'elle écoute les événements globalement à travers tous les niveaux supérieurs.