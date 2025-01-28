---
id: input-pane
title: Input Pane
---

# InputPane (panneau de saisie) <MinVersion version="11.1" />

Le `InputPane` permet aux développeurs d'écouter l'état actuel et les limites du panneau de saisie de la plateforme (par exemple, clavier logiciel ou clavier à l'écran).

Le `InputPane` peut être accessible via une instance de `TopLevel` ou `Window`. Pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../toplevel) :
```cs
var inputPane = TopLevel.GetTopLevel(control).InputPane;
```

:::note
Actuellement, Avalonia n'ajuste pas automatiquement la vue racine et la position de défilement en fonction de l'état du panneau d'entrée. Il est donc recommandé aux développeurs d'utiliser l'API IInputPane et d'ajuster leurs applications en conséquence.

Un ajustement automatique est prévu pour les futures versions 11.*.
:::

## Propriétés

### State
L'état actuel du panneau d'entrée.
Valeurs possibles :
- `InputPaneState.Closed`
- `InputPaneState.Opened`

```cs
InputPaneState State { get; }
```

### OccludedRect
Les limites actuelles du panneau d'entrée.

```cs
Rect OccludedRect { get; }
```

:::note
La valeur de retour est en coordonnées client par rapport au niveau supérieur actuel.
Un rectangle vide sera retourné en cas de panneau d'entrée flottant/détaché, qui est positionné au-dessus de la vue.
:::

## Événements

### StateChanged
Se produit lorsque l'état du panneau d'entrée a changé.

```cs
event EventHandler<InputPaneStateEventArgs>? StateChanged;
```

Notamment, les arguments d'événement incluent plusieurs paramètres utiles :
- `InputPaneStateEventArgs.NewState` - nouvel état du panneau de saisie.
- `InputPaneStateEventArgs.StartRect` - limites initiales du panneau de saisie.
- `InputPaneStateEventArgs.EndRect` - limites finales du panneau de saisie.
- `InputPaneStateEventArgs.AnimationDuration` - durée de l'animation de changement d'état du panneau de saisie.
- `InputPaneStateEventArgs.Easing` - adoucissement de l'animation de changement d'état du panneau de saisie.

Avoir `AnimationDuration` et `Easing` permet aux développeurs de créer une transition entre deux états.

## Compatibilité avec les différentes plateformes :

| Fonctionnalité        | Windows | macOS | Linux | Browser | Android |  iOS | Tizen |
|---------------|-------|-------|-------|-------|-------|-------|-------|
| `State` | ✔ | ✖ | ✖ | ✔* | ✔ | ✔ | ✖ |
| `OccludedRect` | ✔ | ✖ | ✖ | ✔*  | ✔ | ✔ | ✖ |
| `StateChanged` | ✔ | ✖ | ✖ | ✔* | ✔ | ✔ | ✖ |
| `StateChanged.StartRect` | ✖ | ✖ | ✖ | ✔* | ✔ | ✔ | ✖ |
| `StateChanged.AnimationDuration` | ✖ | ✖ | ✖ | ✖ | ✔ | ✔ | ✖ |
| `StateChanged.Easing` | ✖ | ✖ | ✖ | ✖ | ✔ | ✔ | ✖ |

\* - seuls les navigateurs mobiles Chromium prennent en charge l'API IInputPane.