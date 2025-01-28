---
description: CONCEPTS
---

import CompositionBasicLayoutDiagram from '/img/concepts/composition-basic-layout.png';
import CompositionTreesDiagram from '/img/concepts/composition-trees.png';
import CompositionUserControlsDiagram from '/img/concepts/composition-usercontrol.png';
import CompositionCollectionControlsDiagram from '/img/concepts/composition-collection-controls.png';

# Composition UI

La composition UI est le processus que vous utilisez pour créer les mises en page requises par vos applications. Elle vous permet de construire une vue complexe à partir d'un agencement de composants. Les avantages sont :

* _Encapsulation_ - réduire la complexité de chaque composant en limitant son XAML et son code uniquement à ce dont il a besoin, rendant votre code plus compréhensible et maintenable.
* _Réutilisation_ - maintenir une présentation et un comportement cohérents des parties répétées de votre application.

_Avalonia UI_ facilite l'utilisation de la composition UI pour créer les mises en page et les fonctions dont vos applications ont besoin.

Lorsque vous construisez une application en utilisant _Avalonia UI_, il existe plusieurs types de composants parmi lesquels choisir :

* Fenêtres
* Contrôles intégrés
* Contrôles utilisateur
* Contrôles personnalisés
* Contrôles de modèle

## Fenêtres et Contrôles intégrés

Une fenêtre dans _Avalonia UI_ est une unité de mise en page de base (pour une plateforme de fenêtres).

_Avalonia UI_ contient un grand nombre de contrôles intégrés qui couvriront la plupart de vos besoins en matière d'UI.

<img src={CompositionBasicLayoutDiagram} alt=""/>

Lorsque vous rencontrez _Avalonia UI_ pour la première fois, vous pourriez placer un seul contrôle intégré dans la zone de contenu d'une fenêtre (ci-dessus, à gauche). C'est la forme la plus simple de composition UI : la fenêtre a le titre de l'application et généralement quelques contrôles d'état de fenêtre (selon la plateforme cible). Le contrôle intégré permet à votre application de recevoir des entrées utilisateur ou de présenter des sorties avec mise en page et stylisation.

Une application légèrement plus complexe peut nécessiter l'un des contrôles de mise en page intégrés pour organiser plus d'un autre contrôle intégré dans la zone de contenu d'une fenêtre (ci-dessus, à droite).

:::info
Pour voir l'ensemble des contrôles intégrés d'Avalonia UI, consultez la section de référence [ici](../reference/controls/).
:::

## Arbres Logiques et Visuels

Quelle que soit l'arrangement des contrôles que vous utilisez, _Avalonia UI_ représente leurs relations sous forme de structure d'arbre, avec le contrôle 'le plus externe' comme racine. Par exemple, la composition UI précédente peut être représentée comme l'arbre montré ici :

<img src={CompositionTreesDiagram} alt=""/>

C'est l'**arbre de contrôle logique**, et il représente les contrôles de l'application (y compris la fenêtre principale) dans la hiérarchie dans laquelle ils sont définis dans le XAML. Il existe de nombreux systèmes dans _Avalonia UI_ qui traitent l'arbre de contrôle logique et son compagnon, l'**arbre de contrôle visuel**.

:::info
Pour plus d'informations sur le concept des arbres de contrôle, consultez [ici](control-trees.md).
:::

## Contrôles Utilisateurs

Les contrôles utilisateurs sont la pierre angulaire de la composition UI dans _Avalonia UI_.

<img src={CompositionUserControlsDiagram} alt=""/>

Vous pouvez ajouter un contrôle utilisateur à la zone de contenu d'une fenêtre principale, pour représenter une 'vue de page' (ci-dessus, à gauche). Cela vous permet de mettre en œuvre une application plus complexe avec plusieurs pages ; où la mise en page et la fonction de chaque page se trouvent dans leurs propres fichiers de contrôle utilisateur (XAML et code).

:::info
Pour plus d'informations sur la façon de mettre en œuvre une application multi-pages utilisant des vues, consultez le guide [ici](../guides/development-guides/how-to-implement-multi-page-apps.md).
:::

Une autre utilisation d'un contrôle utilisateur est en tant que contrôle de composant (ci-dessus, à droite). Vous pourriez initialement faire cela pour réduire la complexité d'une fenêtre ou d'une vue de page ; mais ensuite, vous pourriez également (peut-être plus tard) réutiliser le composant résultant sur une autre page.

## Tutoriel

:::info
Pour des tutoriels sur les `DataTemplates`, consultez [Avalonia.Samples](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main?tab=readme-ov-file#%EF%B8%8F-datatemplate-samples).  
:::

## Contrôles de Collection

Une autre variation de la composition UI est lorsque vous devez présenter une collection d'éléments.

<img src={CompositionCollectionControlsDiagram} alt=""/>

Ce scénario utilisera l'un des contrôles répétitifs intégrés, lié à une collection ; avec un modèle de données pour représenter les éléments de la collection.

:::info
Pour des informations sur la façon de FAIRE
:::

## Contrôles Personnalisés

Dans le scénario peu probable où vous ne pouvez pas trouver un contrôle intégré _Avalonia UI_ pour couvrir les exigences UI de votre application, vous pouvez créer votre propre contrôle personnalisé à partir de zéro. Cela vous permet de définir vos propres propriétés, événements et méthodes personnalisés ; mais cela nécessitera également que vous implémentiez le dessin de la présentation du contrôle à partir de zéro.

:::info
Pour apprendre à implémenter un contrôle personnalisé, consultez le guide [ici](../basics/user-interface/controls/creating-controls).
:::

## Contrôles Modélisés

Un contrôle modélisé utilise le système de **stylisation** _Avalonia UI_ pour substituer une balise dans la mise en page de l'interface utilisateur par un

:::info
Pour plus d'informations sur les concepts derrière le système de **stylisation** _Avalonia UI_, voir [ici](../basics/user-interface/styling).
:::