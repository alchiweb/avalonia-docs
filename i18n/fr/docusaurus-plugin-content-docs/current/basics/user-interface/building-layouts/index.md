---
description: CONCEPTS
---

# Mise en page

## Panneaux

Avalonia comprend un groupe d'éléments qui dérivent de `Panel`. Ces éléments `Panel` permettent de réaliser de nombreuses mises en page complexes. Par exemple, l'empilement d'éléments peut être facilement réalisé en utilisant l'élément `StackPanel`, tandis que des mises en page plus complexes et fluides sont possibles en utilisant un `Canvas`.

Le tableau suivant résume les contrôles `Panel` disponibles :

| Nom             | Description                                                                                                                                                                                                                                                                                 |
| --------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `Panel`         | Dispose tous les enfants pour remplir les limites du `Panel`                                                                                                                                                                                                                                |
| `Canvas`        | Définit une zone dans laquelle vous pouvez positionner explicitement les éléments enfants par des coordonnées relatives à la zone du Canvas.                                                                                                                                                |
| `DockPanel`     | Définit une zone dans laquelle vous pouvez organiser les éléments enfants soit horizontalement, soit verticalement, les uns par rapport aux autres.                                                                                                                                         |
| `Grid`          | Définit une zone de grille flexible qui se compose de colonnes et de lignes.                                                                                                                                                                                                                |
| `RelativePanel` | Arrange les éléments enfants par rapport à d'autres éléments ou au panneau lui-même.                                                                                                                                                                                                        |
| `StackPanel`    | Organise les éléments enfants en une seule ligne qui peut être orientée horizontalement ou verticalement.                                                                                                                                                                                   |
| `WrapPanel`     | Positionne les éléments enfants dans un ordre séquentiel de gauche à droite, déplaçant le contenu à la ligne suivante à la limite de la boîte contenant. L'ordre suivant se fait de manière séquentielle de haut en bas ou de droite à gauche, selon la valeur de la propriété Orientation. |

Dans WPF, `Panel` est une classe abstraite et la disposition de plusieurs contrôles pour remplir l'espace disponible se fait généralement avec un `Grid` sans lignes/colonnes. Dans Avalonia, `Panel` est un contrôle utilisable qui a le même comportement de disposition qu'un `Grid` sans lignes/colonnes, mais avec une empreinte d'exécution plus légère.

## Boîtes de délimitation des éléments

Lorsqu'on pense à la mise en page dans Avalonia, il est important de comprendre la boîte englobante qui entoure tous les éléments. Chaque `Control` consommé par le système de mise en page peut être considéré comme un rectangle intégré dans la mise en page. La propriété `Bounds` renvoie les limites de l'allocation de mise en page d'un élément. La taille du rectangle est déterminée en calculant l'espace d'écran disponible, la taille des contraintes, les propriétés spécifiques à la mise en page (telles que la marge et le rembourrage), et le comportement individuel de l'élément parent `Panel`. En traitant ces données, le système de mise en page est capable de calculer la position de tous les enfants d'un `Panel` particulier. Il est important de se rappeler que les caractéristiques de taille définies sur l'élément parent, tel qu'un `Border`, affectent ses enfants.

## Le Système de Mise en Page

Dans sa forme la plus simple, la mise en page est un système récursif qui conduit à ce qu'un élément soit dimensionné, positionné et dessiné. Plus précisément, la mise en page décrit le processus de mesure et d'agencement des membres de la collection `Children` d'un élément `Panel`. La mise en page est un processus intensif. Plus la collection `Children` est grande, plus le nombre de calculs à effectuer est important. La complexité peut également être introduite en fonction du comportement de mise en page défini par l'élément `Panel` qui possède la collection. Un `Panel` relativement simple, tel que `Canvas`, peut avoir des performances nettement meilleures qu'un `Panel` plus complexe, tel que `Grid`.

Chaque fois qu'un contrôle enfant change de position, il a le potentiel de déclencher un nouveau passage par le système de mise en page. Il est donc important de comprendre les événements qui peuvent invoquer le système de mise en page, car une invocation inutile peut entraîner une mauvaise performance de l'application. Ce qui suit décrit le processus qui se produit lorsque le système de mise en page est invoqué.

1. Un élément UI enfant commence le processus de mise en page en ayant d'abord ses propriétés essentielles mesurées.
2. Les propriétés de dimensionnement définies sur `Control` sont évaluées, telles que `Width`, `Height` et `Margin`.
3. La logique spécifique au `Panel` est appliquée, telle que la direction de `Dock` ou l'orientation de `Stacking`.
4. Le contenu est disposé après que tous les enfants ont été mesurés.
5. La collection `Children` est dessinée à l'écran.
6. Le processus est invoqué à nouveau si des `Children` supplémentaires sont ajoutés à la collection.

Ce processus et la manière dont il est invoqué sont définis plus en détail dans les sections suivantes.

## Mesurer et disposer les enfants

Le système de mise en page effectue deux passages pour chaque membre de la collection `Children`, un passage de mesure et un passage de disposition. Chaque `Panel` enfant fournit ses propres méthodes `MeasureOverride` et `ArrangeOverride` pour obtenir son propre comportement de mise en page spécifique.

Lors du passage de mesure, chaque membre de la collection `Children` est évalué. Le processus commence par un appel à la méthode `Measure`. Cette méthode est appelée dans l'implémentation de l'élément `Panel` parent et n'a pas besoin d'être appelée explicitement pour que la mise en page se produise.

Tout d'abord, les propriétés de taille natives du `Visual` telles que `Clip` et `IsVisible` sont évaluées. Cela génère une contrainte qui est transmise à `MeasureCore`.

Ensuite, les propriétés du framework qui affectent la valeur de la contrainte sont traitées. Ces propriétés décrivent généralement les caractéristiques de dimensionnement du `Control` sous-jacent, telles que sa `Hauteur`, sa `Largeur` et sa `Marge`. Chacune de ces propriétés peut modifier l'espace nécessaire pour afficher l'élément. `MeasureOverride` est ensuite appelé avec la contrainte comme paramètre.

Comme `Bounds` est une valeur calculée, vous devez être conscient qu'il pourrait y avoir plusieurs changements rapportés ou des changements incrémentiels à ce sujet en raison de diverses opérations effectuées par le système de mise en page. Le système de mise en page peut être en train de calculer l'espace de mesure requis pour les éléments enfants, les contraintes imposées par l'élément parent, etc.

L'objectif ultime du passage de mesure est que l'enfant détermine sa `DesiredSize`, ce qui se produit lors de l'appel à `MeasureCore`. La valeur de `DesiredSize` est stockée par `Measure` pour une utilisation lors du passage de disposition du contenu.

Le passage de disposition commence par un appel à la méthode `Arrange`. Pendant le passage de disposition, l'élément parent `Panel` génère un rectangle qui représente les limites de l'enfant. Cette valeur est transmise à la méthode `ArrangeCore` pour traitement.

La méthode `ArrangeCore` évalue la `DesiredSize` de l'enfant et évalue les marges supplémentaires qui peuvent affecter la taille rendue de l'élément. `ArrangeCore` génère une taille d'arrangement, qui est transmise à la méthode `ArrangeOverride` du `Panel` en tant que paramètre. `ArrangeOverride` génère la finalSize de l'enfant. Enfin, la méthode `ArrangeCore` effectue une évaluation finale des propriétés de décalage, telles que la marge et l'alignement, et place l'enfant dans son emplacement de mise en page. L'enfant n'a pas besoin de (et ne remplit souvent pas) tout l'espace alloué. Le contrôle est ensuite renvoyé au `Panel` parent et le processus de mise en page est complet.

## Dans cette section

* [Aperçu des panneaux](panels-overview.md)
* [Alignement, marges et remplissage](alignment-margins-and-padding.md)
* [Créer un panneau personnalisé](../../../guides/custom-controls/create-a-custom-panel.md)