---
description: CONCEPTS
---

# Input

Avalonia fonctionne sur l'abstraction appelée dispositifs de pointage.

Divers contrôles qui implémentent `ICommandSource` ont une propriété `HotKey`.

Les contrôles détectent et répondent le plus souvent aux entrées de l'utilisateur. Le système d'entrée d'Avalonia utilise à la fois des [événements directs et routés](../input/routed-events) pour prendre en charge la saisie de texte, la gestion du focus et le positionnement de la souris.

Les applications ont souvent des exigences d'entrée complexes. Avalonia fournit un [système de commandes](../../basics/user-interface/adding-interactivity) qui sépare les actions d'entrée de l'utilisateur du code qui répond à ces actions.