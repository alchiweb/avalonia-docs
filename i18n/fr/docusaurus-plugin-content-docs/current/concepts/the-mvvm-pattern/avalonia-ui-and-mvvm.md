---
description: CONCEPTS - The MVVM Pattern
---

import MvvmDataBindingDiagram from '/img/concepts/mvvm/mvvm.png';

# Avalonia UI et MVVM

Sur cette page, vous apprendrez comment le modèle MVVM est réalisé lorsqu'il est utilisé avec _Avalonia UI_.

## Vues et Modèles de Vue

Lorsque vous utilisez le modèle MVVM avec _Avalonia UI_, vous implémentez une vue avec un fichier AXAML, attaché à un fichier de code-behind correspondant, et un modèle de vue avec un fichier de classe de code classique.

Dans _Avalonia UI_, une vue est implémentée comme une composition d'éléments d'interface utilisateur dans une fenêtre ou un contrôle utilisateur (deux fichiers AXAML avec code-behind). Les éléments d'interface utilisateur dans une composition peuvent être un mélange de contrôles intégrés à _Avalonia UI_, de contrôles utilisateur et de contrôles (plus avancés) de votre propre conception et mise en œuvre.

:::info
Pour une liste complète des contrôles intégrés à _Avalonia UI_, consultez la section de référence [ici](../../reference/controls/).
:::

:::info
Pour en savoir plus sur le concept de composition d'interface utilisateur, consultez [ici](../ui-composition.md).
:::

:::info
Pour apprendre à concevoir et à mettre en œuvre vos propres contrôles, consultez [ici](../../guides/custom-controls/how-to-create-a-custom-controls-library.md).
:::

## Liaison de Données

La liaison de données est la technologie clé qui permet à une application _Avalonia UI_ MVVM de séparer les vues des modèles de vue. Vous pouvez visualiser la relation entre la vue et le modèle de vue comme deux couches connectées par les liaisons de données :

<img src={MvvmDataBindingDiagram} alt=""/>

Remarquez comment certaines des liaisons de données sont représentées par une flèche bidirectionnelle et d'autres par une flèche unidirectionnelle. Par exemple, les champs de saisie du nom et de l'adresse sont bidirectionnels - vous voulez que les deux modifications dans le modèle de vue soient notifiées à la vue, et que les entrées de la vue soient mises à jour dans le modèle de vue.

Les boutons, en revanche, ont des commandes unidirectionnelles, émises par la vue et exécutées par le modèle de vue.

Remarquez comment la classe du modèle de vue n'est pas dépendante de la couche de vue, ni de la manière dont elle sera rendue sur la plateforme cible par _Avalonia UI_. Comme la classe du modèle de vue est indépendante, elle peut être testée unitairement comme n'importe quel autre code.

Lorsque vous utilisez le modèle MVVM en pratique, vous utiliserez un modèle de vue correspondant pour chaque vue, et la classe du modèle de vue contient toute la logique d'application pour la vue.

## Le modèle MVVM

Le modèle est l'autre partie du modèle MVVM. Les modèles sont beaucoup moins précisément définis dans le modèle car ils représentent 'le reste de l'architecture'. Il s'agit souvent de stockage de données ou d'autres services.

Le principe important que vous devez maintenir est la séparation. Vous devez mettre en œuvre la relation entre le modèle de vue et le modèle en utilisant une forme de modèle d'injection de dépendance (DI).

## ReactiveUI

Il existe plusieurs frameworks conçus pour aider à écrire des applications en utilisant le modèle MVVM.

Dans les pages suivantes, vous apprendrez à connaître le framework _ReactiveUI_ qui est l'un des plus populaires et est pris en charge par l'un des paquets _Avalonia UI_.