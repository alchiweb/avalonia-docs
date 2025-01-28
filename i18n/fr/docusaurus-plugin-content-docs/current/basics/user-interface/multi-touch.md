---
id: multi-touch
title: Multi-Touch
---

Avalonia UI, contrairement à certains autres frameworks, ne segmente pas les événements tactiles. Au lieu de cela, il utilise un système d'événements de pointeur qui unifie les événements de souris, de stylet et tactiles. Cela signifie que plutôt que d'avoir des événements séparés `TouchEnter` ou `TouchLeave`, vous utiliseriez les événements `PointerPressed`, `PointerMoved` et `PointerReleased` pour suivre les entrées tactiles.

## Utilisation des Reconnaisseurs de Gestes

En plus des événements tactiles de base, Avalonia fournit des reconnaisseurs de gestes intégrés qui facilitent la gestion des gestes tactiles courants comme le pincement et le défilement. Voici comment vous pouvez les utiliser :

```xml
<Image Stretch="UniformToFill"
       Margin="5"
       Name="PinchImage"
       Source="/Assets/delicate-arch-896885_640.jpg">
    <Image.GestureRecognizers>
        <PinchGestureRecognizer/>
        <ScrollGestureRecognizer CanHorizontallyScroll="True" CanVerticallyScroll="True"/>
    </Image.GestureRecognizers>
</Image>
```

Dans cet exemple, un contrôle `Image` est configuré pour répondre aux gestes de pincement et de défilement. Le `PinchGestureRecognizer` permet la fonctionnalité de zoom par pincement, et le `ScrollGestureRecognizer` permet à l'image d'être défilée à la fois horizontalement et verticalement.