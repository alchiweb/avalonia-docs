---
id: how-to-add-menu-icons
title: Comment Ajouter des Icônes de Menu
---


# Comment Ajouter des Icônes de Menu

Dans Avalonia, vous pouvez améliorer l'apparence et l'expérience utilisateur de votre application en ajoutant des icônes aux éléments de menu. Les icônes peuvent fournir un indice visuel pour l'action effectuée par l'élément de menu, facilitant ainsi la navigation des utilisateurs dans votre application. Ce guide vous expliquera comment ajouter des icônes aux éléments de menu dans Avalonia.

## Ajouter des Icônes aux Éléments de Menu

La propriété `MenuItem.Icon` est utilisée pour définir une icône pour un élément de menu. Vous pouvez utiliser divers types de sources d'image pour l'icône, y compris des URI de ressources, des chemins de fichiers ou des URL web. Voici un exemple de la façon d'ajouter une icône à un élément de menu :

```xml
<Menu>
  <MenuItem Header="File">
    <MenuItem Header="Open" Command="{Binding OpenCommand}">
      <MenuItem.Icon>
        <Image Width="16" Height="16" Source="avares://MyApp/Assets/open_icon.png" />
      </MenuItem.Icon>
    </MenuItem>
  </MenuItem>
</Menu>
```

Dans cet exemple, la propriété `MenuItem.Icon` est définie sur un contrôle `Image` qui affiche une image provenant des ressources de l'application. La propriété `Source` du contrôle `Image` est définie sur un URI de ressource qui représente la source de l'image. Les propriétés `Width` et `Height` sont définies pour contrôler la taille de l'image.
