---
description: CONCEPTS
---

import MvvmPatternDiagram from '/img/guides/implementation-guides/mvvm-architecture.png';

# Le modèle MVVM

<img src={MvvmPatternDiagram} alt=""/>

Le modèle Model-View-View Model (MVVM) est une manière courante de structurer une application UI. Il utilise un système de liaison de données qui aide à déplacer les données entre ses parties vue et modèle de vue. Cela signifie qu'il réalise une séparation de la logique de l'application (modèle de vue) de l'affichage de l'UI (vue).

La séparation entre la logique de l'application et les services métiers (modèle) est généralement réalisée par un système d'injection de dépendances (DI).

MVVM peut être excessif pour une application simple ; mais à mesure que les applications évoluent, elles atteindront souvent un point où le maintien de la définition de l'affichage et de la logique de l'application dans les mêmes modules de composants UI devient problématique :

* Les interactions entre les composants UI deviennent compliquées et sujettes aux erreurs.
* Il devient difficile de tester unitairement les composants UI en raison des dépendances sur la plateforme UI cible.

MVVM résout ce problème en abstrait la logique de l'application dans des classes uniquement codées qui ne dépendent pas de la plateforme UI cible, et peuvent donc être testées de manière indépendante.

:::info
Pour en savoir plus sur le contexte du modèle MVVM, consultez l'article _Microsoft Patterns and Practices_ [ici](https://learn.microsoft.com/en-us/previous-versions/msp-n-p/hh848246\(v=pandp.10\)).
:::

## Quand utiliser MVVM ?

MVVM est un modèle de programmation plus complexe par rapport au modèle de code-behind basé sur les événements. Vous avez un surcoût d'apprentissage supplémentaire pour maîtriser les techniques du framework _ReactiveUI_ que vous utiliserez pour implémenter MVVM avec _Avalonia UI_.

En fait, le modèle de code-behind peut être plus facile à comprendre et à maintenir pour une petite application simple.

:::info
Pour des détails sur la façon de programmer _Avalonia UI_ avec le modèle de code-behind, voir [ici](../../basics/user-interface/code-behind).
:::

Les avantages de l'utilisation du modèle MVVM ne deviennent apparents que lorsqu'une application grandit et devient plus complexe. Vous avez donc deux stratégies de développement à considérer :

1. Commencez par utiliser le modèle de code-behind plus simple. Visez à convertir en MVVM si l'application devient difficile à maintenir.
2. Utilisez MVVM dès le départ car vous vous attendez à ce que l'application se développe.

Vous pouvez utiliser les pages suivantes pour en savoir plus sur l'utilisation de MVVM avec _Avalonia UI_, quelle que soit la stratégie ci-dessus que vous adoptez.