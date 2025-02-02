---
id: improving-performance
title: Amélioration des performances
---

Les performances des applications Avalonia peuvent être considérablement améliorées en tenant compte de plusieurs considérations clés durant le processus de développement. Ce document discute des étapes que vous pouvez suivre pour optimiser les performances de vos applications Avalonia.

## Utiliser les CompiledBindings

L'un des moyens les plus efficaces d'améliorer les performances dans Avalonia est d'utiliser les [`CompiledBindings`](../../basics/data/data-binding/compiled-bindings) dans votre application. Les liaisons compilées permettent un data binding plus rapide en compilant le chemin de liaison au moment de la compilation, réduisant ainsi le surcoût de la réflexion à l'exécution.

## Choisir le bon contrôle pour l'affichage des données

Lorsque vous devez afficher une grande quantité de données dans un `DataGrid` ou un `TreeView` avec de nombreux nœuds, il est recommandé d'utiliser le contrôle `TreeDataGrid`. Le `TreeDataGrid` est construit de zéro et offre de meilleures performances que le `DataGrid` normal. Il prend en charge la virtualisation et est particulièrement utile si vous avez besoin d'un arbre virtualisé, car il dispose de modèles de données hiérarchiques.

Évitez d'utiliser le contrôle `DataGrid` si vous n'avez pas besoin de fonctionnalités d'édition. Il est généralement considéré comme un contrôle moins optimal pour les performances.

## Virtualisation

Lorsqu'il s'agit de grandes quantités de données, activer la virtualisation peut améliorer les performances de votre application Avalonia. La virtualisation signifie que seuls les éléments visibles dans le contrôle sont rendus, ce qui améliore considérablement les performances lorsqu'il y a un grand nombre d'éléments à afficher.

### TreeDataGrid

`TreeDataGrid` prend en charge la virtualisation et peut gérer efficacement des milliers de lignes avec des cellules complexes.

## Optimisez votre structure d'arbre visuel

La performance peut souvent être entravée par une mise en page profondément imbriquée et compliquée. Efforcez-vous de maintenir votre balisage XAML aussi simple et plat que possible. Le rendu des éléments d'interface utilisateur à l'écran déclenche un "passage de mise en page" deux fois pour chaque élément unique (un passage de mesure suivi d'un passage d'agencement).

Ce processus de passage de mise en page est gourmand en calculs : plus un élément a d'enfants, plus il faut de calculs. Par conséquent, minimiser la complexité de votre arbre visuel dans Avalonia UI peut considérablement améliorer les performances de l'application.

## Minimisez l'utilisation de Run pour définir les propriétés de texte

Il est conseillé de minimiser l'utilisation de Run au sein d'un TextBlock, car cela peut entraîner des opérations plus gourmandes en ressources. Si vous utilisez Run pour définir des propriétés de texte, envisagez de définir ces propriétés directement sur le TextBlock à la place. Cette pratique peut aider à améliorer les performances de votre application.

## Utilisez StreamGeometries plutôt que PathGeometries

Lorsque vous travaillez avec des géométries dans Avalonia UI, `StreamGeometry` est une alternative plus efficace à `PathGeometry`. `StreamGeometry` est spécifiquement optimisé pour gérer de nombreux objets `PathGeometry`, consommant moins de mémoire et offrant de meilleures performances. Par conséquent, lorsqu'un choix est disponible, il est recommandé d'utiliser `StreamGeometry` plutôt que `PathGeometry` pour améliorer les performances de l'application.

## Utilisez des tailles d'image réduites

Lorsque votre application nécessite l'affichage d'images plus petites ou de vignettes, il est bénéfique de générer et d'utiliser des versions réduites de vos images. Par défaut, Avalonia chargera et décodera votre image à sa taille originale, ce qui peut potentiellement entraîner des goulets d'étranglement en termes de performance si vous chargez de grandes images et les redimensionnez à des tailles de vignettes dans des contrôles comme un `ItemsControl`.

## Résoudre vos erreurs de liaison

Les erreurs de liaison sont une source courante de problèmes de performance dans les applications Avalonia UI. Chaque occurrence d'une erreur de liaison entraîne une baisse de performance alors que l'application tente de résoudre la liaison et enregistre l'erreur dans le journal de trace. Naturellement, plus il y a d'erreurs de liaison, plus l'impact sur la performance est important.

Un contributeur significatif aux erreurs de liaison est l'utilisation de liaisons `RelativeSource` dans les `DataTemplates`, car la liaison n'est généralement pas résolue correctement tant que le `DataTemplate` n'a pas terminé son initialisation. Il est recommandé d'éviter complètement `RelativeSource.FindAncestor`. Une approche plus efficace consiste à définir une propriété attachée et à utiliser l'héritage de propriété pour transmettre des valeurs dans l'arbre visuel, plutôt que de faire une recherche dans l'arbre visuel.

## Charger des données de manière asynchrone

Les problèmes de performance, les gels de l'interface utilisateur et les applications non réactives proviennent souvent de la manière dont les données sont chargées. Pour éviter de surcharger le fil d'interface utilisateur, assurez-vous que vos données sont chargées de manière asynchrone.
