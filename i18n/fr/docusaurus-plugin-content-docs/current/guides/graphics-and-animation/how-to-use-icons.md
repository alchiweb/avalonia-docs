---
id: how-to-use-icons
title: Comment utiliser des icônes
---


# Comment utiliser des icônes

Dans Avalonia, l'utilisation d'icônes dans votre interface utilisateur peut aider à améliorer l'apparence de votre application et la rendre plus conviviale. Les icônes peuvent fournir une représentation visuelle des actions ou du contenu, facilitant ainsi la compréhension de la fonctionnalité de votre application par les utilisateurs. Ce guide vous montrera comment ajouter et utiliser des icônes dans votre application Avalonia.

## Utilisation des icônes dans Avalonia
Les icônes peuvent être ajoutées à votre application Avalonia de différentes manières. Ce guide couvrira deux méthodes courantes : l'utilisation de fichiers image et l'utilisation de polices d'icônes.

### Utilisation de fichiers image
Une façon d'utiliser des icônes dans Avalonia est d'utiliser des fichiers image. Vous pouvez utiliser divers formats tels que PNG, JPG ou BMP. Voici un exemple d'utilisation d'un fichier image comme icône :

```xml
<Image Width="16" Height="16" Source="avares://MyApp/Assets/icon.png" />
```

Dans cet exemple, un contrôle `Image` est utilisé pour afficher une image provenant des ressources de l'application en tant qu'icône. La propriété `Source` du contrôle `Image` est définie sur un URI de ressource qui pointe vers le fichier image.

### Utilisation de polices d'icônes

Une autre façon d'utiliser des icônes dans Avalonia est d'utiliser des polices d'icônes. Les polices d'icônes vous permettent d'utiliser des icônes vectorielles évolutives qui peuvent être personnalisées avec CSS en termes de taille, de couleur, d'ombre portée, etc. Voici un exemple d'utilisation d'une police d'icônes dans Avalonia :

```xml
<TextBlock FontFamily="avares://MyApp/Assets/#FontAwesome" Text="&#xf030;" />
```

Dans cet exemple, un contrôle `TextBlock` est utilisé pour afficher une icône de la police d'icônes `FontAwesome`. La propriété `FontFamily` du contrôle `TextBlock` est définie sur un URI de ressource qui pointe vers le fichier de police, et la propriété Text est définie sur la valeur Unicode de l'icône souhaitée.

## Bonnes Pratiques

Bien que l'utilisation d'icônes puisse améliorer l'utilisabilité de votre application, il est important de les utiliser judicieusement. Gardez à l'esprit les conseils suivants lors de l'utilisation d'icônes :

* Assurez-vous que les icônes sont de taille appropriée et clairement visibles sur le fond.
* Utilisez des icônes universellement reconnues pour les actions courantes afin de rendre votre application plus intuitive.
