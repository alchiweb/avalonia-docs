# Contrôles

Les contrôles dans l'interface utilisateur Avalonia sont des éléments de construction fondamentaux utilisés pour créer des interfaces utilisateur. Ils représentent divers éléments interactifs tels que des boutons, des zones de texte, des curseurs et plus encore. La compréhension des contrôles est essentielle pour le développement d'applications à l'aide de l'interface utilisateur Avalonia.

## Les contrôles, c'est quoi exactement ?

Les contrôles sont des éléments d'interface utilisateur qui permettent aux utilisateurs d'interagir avec une application. Ils fournissent des fonctionnalités pour l'entrée, l'affichage et la manipulation des données. Les contrôles peuvent être classés en plusieurs types en fonction de leur but et de leur comportement.

- **Boutons** : Les boutons sont couramment utilisés pour déclencher des actions lorsqu'ils sont cliqués ou touchés. Ils peuvent avoir du texte, des icônes ou les deux, et sont souvent utilisés pour des tâches comme la soumission de formulaires, l'ouverture de boîtes de dialogue ou l'exécution de commandes.
- **Zones de texte** : Les zones de texte permettent aux utilisateurs de saisir et de modifier du texte. Elles sont utilisées pour capturer les entrées des utilisateurs, telles que les noms d'utilisateur, les mots de passe ou toute forme d'informations textuelles. Les zones de texte peuvent également être personnalisées pour des modèles d'entrée et une validation spécifiques.
- **Étiquettes** : Les étiquettes sont utilisées pour afficher du texte statique ou des légendes pour d'autres contrôles. Elles fournissent des informations supplémentaires ou un contexte à l'utilisateur et sont généralement non interactives.
- **Cases à cocher et boutons radio** : Les cases à cocher et les boutons radio sont utilisés pour la sélection et les options à choix multiples. Les cases à cocher permettent aux utilisateurs de sélectionner une ou plusieurs options, tandis que les boutons radio permettent aux utilisateurs de choisir une seule option parmi un groupe.
- **Curseurs** : Les curseurs sont utilisés pour sélectionner une valeur dans une plage. Ils fournissent une représentation visuelle d'une valeur qui peut être ajustée en faisant glisser une poignée le long d'une piste. Les curseurs sont couramment utilisés pour des réglages tels que les contrôles de volume ou les ajustements d'image.
- **ListBox et ComboBox** : Les ListBox et les combo boxes permettent aux utilisateurs de sélectionner un élément dans une liste ou un menu déroulant. Les ListBox affichent plusieurs éléments à la fois, tandis que les ComboBox montrent un seul élément au départ et s'étendent pour afficher une liste lorsqu'on clique dessus.

Ce ne sont là que quelques exemples des nombreux contrôles disponibles dans Avalonia UI. Chaque contrôle a son propre ensemble de propriétés, de méthodes et d'événements, permettant aux développeurs de personnaliser son apparence et son comportement pour répondre aux besoins de leur application.

## Prise en main des contrôles intégrés

Pour commencer à utiliser les contrôles dans Avalonia UI, vous pouvez vous référer à la documentation pour chaque type de contrôle. La documentation fournit des explications détaillées, des exemples et des extraits de code pour vous aider à comprendre et à utiliser les contrôles efficacement.

- [Documentation du contrôle de bouton](../../../reference/controls/buttons/button)
- [Documentation du contrôle de zone de texte](../../../reference/controls/textbox)
- [Documentation du contrôle d'étiquette](../../../reference/controls/label)
- [Documentation sur le Contrôle de Case à Cocher](../../../reference/controls/checkbox)
- [Documentation sur le Contrôle de Curseur](../../../reference/controls/slider)
- [Documentation sur le Contrôle de Liste](../../../reference/controls/listbox)

En explorant ces ressources, vous acquerrez une base solide pour utiliser les contrôles dans Avalonia UI et serez en mesure de créer des interfaces utilisateur riches et interactives pour vos applications.

## Types de Contrôles Intégrés

Les contrôles intégrés d'_Avalonia UI_ peuvent être classés de manière lâche dans les types suivants :

* Contrôles Dessinés
* Contrôles de Mise en Page
* Contrôles Utilisateur*
* Contrôles avec Modèle
* Entièrement Personnalisables
* Partiellement Personnalisables

*Les contrôles utilisateur ne sont disponibles que pour les applications.

:::note
Ces classifications sont en quelque sorte liées à la discussion sur [Choisir un Type de Contrôle Personnalisé](creating-controls/choosing-a-custom-control-type).
:::

### Contrôles Dessinés

Les contrôles dessinés sont ceux qui sont responsables de la génération de leur propre géométrie ou bitmap et de leur rendu. Des exemples de ces contrôles incluent `Border`, `TextBlock` et `Image`. Les contrôles dessinés sont les contrôles fondamentaux utilisés pour construire tout le reste.

La plupart des contrôles dessinés ont des propriétés standard qui peuvent être utilisées pour ajuster leur apparence et leur taille, mais ils ne permettent pas de re-modélisation. Cela signifie qu'en tant que développeur d'application, vous ne pouvez pas changer la fonctionnalité ou le style de ces contrôles sans plonger dans C#, dériver une nouvelle version du contrôle et intercepter les méthodes de rendu.

### Contrôles de Mise en Page

Les contrôles de mise en page sont spéciaux en ce sens qu'ils n'ont pas d'apparence par eux-mêmes. Les contrôles de mise en page comme `Grid`, `StackPanel` et d'autres sont responsables de la définition de la mise en page de leurs enfants et se comportent comme un conteneur parent. Les contrôles enfants sont responsables du rendu de l'interface utilisateur tandis que le contrôle parent de mise en page se contente de définir la taille et la position (qui n'ont pas d'apparence par elles-mêmes).

Il n'est pas très courant que les développeurs d'applications modifient les contrôles de mise en page fournis par le framework.

:::note
Certains contrôles de mise en page comme `Grid` ont des propriétés telles que `Background` pour simplifier les cas d'utilisation courants. L'utilisation de ces propriétés donne une certaine apparence à ces contrôles.
:::

## Contrôles Utilisateur

_Avalonia UI_ ne fournit jamais de `UserControl` par lui-même car ceux-ci ne sont pas considérés comme polyvalents. Pour plus d'informations sur la création et l'utilisation de `UserControl` dans votre application, consultez [Choisir un type de contrôle personnalisé](creating-controls/choosing-a-custom-control-type).

### Contrôles Templatés

La plupart des contrôles standard dans _Avalonia UI_ sont des contrôles templatés, ce qui signifie que leur apparence visuelle est définie dans un modèle de contrôle XAML séparé de la fonctionnalité. C'est la base du concept de contrôles sans apparence qui a vu le jour dans WPF.

Les développeurs d'applications peuvent modifier le modèle XAML d'un contrôle templaté et le faire apparaître complètement différemment. Cette fonctionnalité n'est pas disponible dans tous les frameworks UI et est l'une des fonctionnalités les plus puissantes des frameworks UI basés sur XAML.

      :::note
Le re-modélisation des contrôles est un recours de dernier ressort pour les développeurs d'applications. Cela signifie également que vous serez responsable de la mise à jour du modèle avec les modifications en amont. Au lieu de cela, il est préférable de :

 1. Essayer d'utiliser les propriétés existantes pour personnaliser le contrôle
 2. Créer un nouveau style avec les sélecteurs de style extrêmement puissants de _Avalonia UI_ pour modifier ce dont vous avez besoin dans le modèle existant
 3. En dernier recours, re-modéliser
:::

#### Entièrement Personnalisable

La majorité des contrôles modélisés dans _Avalonia UI_ sont entièrement personnalisables. Cela signifie qu'il est possible de remplacer complètement le modèle du contrôle et de changer son apparence. Le contrôle `Button` est un bon exemple, mais tous les contrôles modélisés dans _Avalonia UI_ essaient d'être entièrement personnalisables par défaut. Avec un contrôle modélisé entièrement personnalisable, l'application a presque une capacité totale à styliser ou à changer tout ce que vous voyez dessiné dans l'interface utilisateur.

#### Partiellement Personnalisable

En pratique, avoir des modèles de contrôles entièrement remplaçables n'est pas toujours possible. Il existe un spectre dans la conception des contrôles entre le soutien aux cas d'utilisation courants et la possibilité de rendre le contrôle entièrement re-modélisable. Pour des contrôles de haute complexité comme le `DataGrid`, le spectre se déplace vers le soutien des cas d'utilisation prévus et le contrôle ne peut pas, et ne devrait pas, être entièrement re-modélisé. Ces contrôles ont également généralement un très grand nombre de parties de modèle (éléments de contrôle requis qui sont utilisés directement par l'implémentation C# du contrôle).

Dans le cas d'un `DataGrid`, il est toujours possible de re-modéliser des composants individuels ou des parties du contrôle. Il est juste extrêmement difficile de changer complètement son apparence et son fonctionnement.

Les contrôles modélisés partiellement personnalisables de l'ordre du `DataGrid` sont rares en tant que contrôles de première partie fournis par le framework lui-même.

## Création de Contrôles

Dans Avalonia, vous avez la flexibilité de créer des contrôles personnalisés de tous types adaptés aux exigences spécifiques de votre application. Consultez la section [Création de Contrôles](creating-controls) pour plus d'informations.