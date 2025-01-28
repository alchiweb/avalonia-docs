---
id: animations
title: Animations
---

import KeyframeDiagram from '/img/basics/user-interface/animation-keyframe.png';

# Animations

Il existe deux types d'animations dans _Avalonia UI_ :

* Animation par images clés - peut changer une ou plusieurs valeurs de propriété en utilisant une chronologie. Les images clés sont définies le long de la chronologie à des points de repère. Les propriétés modifiées sont ajustées entre les images clés à l'aide d'une fonction d'assouplissement (qui est une interpolation en ligne droite par défaut). Les animations par images clés sont un type d'animation très polyvalent.
* Transitions - peuvent changer une seule propriété.

## Animation par images clés

La plus simple des animations par images clés changera une valeur de propriété sur une durée spécifiée en définissant deux images clés avec des points de repère au début (0 % de la durée) et à la fin (100 % de la durée).

<img src={KeyframeDiagram} alt=''/>

La valeur de la propriété est ensuite modifiée au fil du temps entre les images clés en utilisant le profil défini par une fonction d'assouplissement. La fonction d'assouplissement par défaut est également la plus simple - une interpolation en ligne droite entre deux images clés.

:::info
Vous pouvez voir l'ensemble des fonctions d'assouplissement dans la référence, [ici](../../reference/animation-settings.md).
:::

## Déclenchement des animations

Les animations _Avalonia UI_ définies en XAML s'appuient sur des sélecteurs pour leur comportement de déclenchement. Les sélecteurs peuvent toujours s'appliquer à un contrôle, ou ils peuvent s'appliquer conditionnellement (par exemple si le contrôle a une classe de style appliquée).

Si le sélecteur n'est pas conditionnel, alors l'animation sera déclenchée lorsqu'un `Control` correspondant sera créé dans l'arbre visuel. Sinon, les animations s'exécuteront chaque fois que son sélecteur sera activé. Lorsque le sélecteur ne correspond plus, l'animation en cours sera annulée.

## Autres paramètres d'animation

* Délai
* Répéter
* Direction de lecture
* Mode de remplissage de valeur
* Fonction d'assouplissement