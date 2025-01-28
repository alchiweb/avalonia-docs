---
description: CONCEPTS
---

import ControlTreesLogicalScreenshot from '/img/concepts/control-trees-logical.png';
import ControlTreesVisualScreenshot from '/img/concepts/control-trees-visual.png';
import ControlTreesEventScreenshot from '/img/concepts/control-trees-events.png';

# Arbres de Contrôle

_Avalonia UI_ crée des arbres de contrôle à partir des fichiers XAML d'une application afin de pouvoir rendre la présentation de l'interface utilisateur et gérer la fonctionnalité de l'application.

## Arbre Logique

L'arbre de contrôle logique représente les contrôles de l'application (y compris la fenêtre principale) dans la hiérarchie dans laquelle ils sont définis dans le XAML. Par exemple, un contrôle (bouton) à l'intérieur d'un autre contrôle (panneau empilé) dans une fenêtre aura l'arbre logique à 3 couches montré ici :

<img src={ControlTreesLogicalScreenshot} alt=""/>

Pendant que votre application est en cours d'exécution, vous pouvez afficher la fenêtre _Outils de Développement Avalonia_ (touche F12). Cela affiche l'arbre logique dans son onglet **Arbre Logique**.

## Arbre Visuel

L'arbre de contrôle visuel contient tout ce qui est effectivement exécuté par _Avalonia UI_. Il montre toutes les propriétés définies sur les contrôles, et toutes les parties supplémentaires qui ont été ajoutées par _Avalonia UI_ afin de présenter l'interface utilisateur et gérer la fonctionnalité de l'application.

<img src={ControlTreesVisualScreenshot} alt=""/>

Vous pouvez voir l'arbre de contrôle visuel dans l'onglet **Arbre Visuel** de la fenêtre _Outils de Développement Avalonia_.

## Événements

Une partie essentielle de la gestion de la fonctionnalité de l'application effectuée par _Avalonia UI_ est la génération et la propagation des événements. L'onglet **Événements** enregistre la source et la propagation des événements au fur et à mesure que vous vous déplacez et interagissez avec l'application en cours d'exécution.

<img src={ControlTreesEventScreenshot} alt=""/>