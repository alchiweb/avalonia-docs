---
id: troubleshooting
title: Dépannage
---

## Mon thème de contrôle n'est pas trouvé

Si vous avez des difficultés à faire en sorte qu'Avalonia trouve votre thème, assurez-vous qu'il renvoie une [clé de style](styles#style-key) qui correspond à `x:Key` et `TargetType` de votre thème de contrôle.

## Mon thème de contrôle casse d'autres contrôles

De nombreux contrôles Avalonia se composent d'une combinaison d'autres contrôles Avalonia. Si vous créez un style ou un thème de contrôle qui s'applique à tous les contrôles d'un type, vous pourriez obtenir des résultats inattendus. Par exemple, si vous créez un style qui cible le type `TextBlock` dans une `Window`, le style est appliqué à tous les contrôles `TextBlock` dans la fenêtre, même si le `TextBlock` fait partie d'un autre contrôle, comme un `ListBox`.

## La fenêtre de l'application est transparente ou aucun contenu n'est rendu

Assurez-vous que vous avez installé et inclus le thème Avalonia dans votre application. 
Si vous utilisez les thèmes intégrés [Fluent](themes/fluent.md) ou [Simple](themes/simple.md), veuillez consulter leurs pages respectives sur la façon de les installer.

Si vous utilisez des thèmes tiers, veuillez contacter leurs mainteneurs.