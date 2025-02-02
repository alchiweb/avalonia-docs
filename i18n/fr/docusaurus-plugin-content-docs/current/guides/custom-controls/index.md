---
id: index
title: Comment créer un contrôle personnalisé
---

# Comment créer un contrôle personnalisé

Ce guide vous montrera comment créer un contrôle personnalisé simple avec _Avalonia UI_.

Avant de commencer à créer votre propre contrôle, vous devez décider quel type de contrôle personnalisé vous souhaitez implémenter, les choix sont :

* Contrôle personnalisé
* Contrôle personnalisé avec modèle

### Contrôle personnalisé

Un contrôle personnalisé se dessine lui-même en utilisant le système graphique _Avalonia UI_, en utilisant des méthodes de base pour les formes, les lignes, les remplissages, le texte, et bien d'autres. Vous pouvez définir vos propres propriétés, événements et pseudo-classes.

Certains des contrôles intégrés d'_Avalonia UI_ sont comme ça. Par exemple, le contrôle de bloc de texte (classe `TextBlock`) et le contrôle d'image (classe `Image`).

### Contrôles Personnalisés Templatés

Un contrôle personnalisé templaté crée un contrôle 'sans apparence' qui peut être stylisé par des thèmes ou des dictionnaires de styles inclus dans votre projet. Le contrôle a du code pour les propriétés et les événements, ainsi que pour le traitement, mais pas de propriétés ou d'instructions sur la façon de dessiner. Un contrôle templaté se réfère à un thème ou à des styles pour sélectionner des propriétés comme les couleurs de pinceau, l'épaisseur des lignes, le rayon des coins, etc. Les instructions de dessin se trouvent dans le thème.

La majorité des contrôles intégrés d'_Avalonia UI_ sont templatés.

:::info
Pour des conseils sur la façon de créer des contrôles templatés, voir [ici](../../basics/user-interface/controls/creating-controls).
:::

Les pages suivantes vous montrent comment créer un simple contrôle personnalisé (hérité de `Control`).
