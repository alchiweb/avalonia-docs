---
id: assets
title: Actifs et Images
---

import AssetFileDiagram from '/img/basics/user-interface/asset-file.png';
import AssetLibraryDiagram from '/img/basics/user-interface/asset-library.png';

# Actifs

De nombreuses applications doivent inclure des actifs tels que des images bitmap, des styles et des dictionnaires de ressources. Les dictionnaires de ressources contiennent des éléments graphiques fondamentaux qui peuvent être déclarés en XAML. Les styles peuvent également être écrits en XAML, mais les actifs bitmap sont des fichiers binaires, par exemple aux formats PNG et JPEG.

## Inclusion des actifs

<img src={AssetFileDiagram} alt=''/>

Vous incluez des actifs dans une application en utilisant l'élément `<AvaloniaResource>` dans votre fichier de projet.

Par exemple, le modèle de solution Avalonia .NET Core MVVM crée un dossier appelé `Assets` (contenant le fichier `avalonia-logo.ico`) et ajoute un élément au fichier de projet pour inclure tous les fichiers qui s'y trouvent. Comme suit :

```xml
<ItemGroup>
  <AvaloniaResource Include="Assets\**"/>
</ItemGroup>
```

Vous pouvez inclure tous les fichiers que vous souhaitez en ajoutant des éléments `<AvaloniaResource>` supplémentaires dans ce groupe d'éléments.

:::tip
Le nom de l'élément `AvaloniaResource` ici indique seulement que les actifs seront stockés en interne comme des ressources .NET par la construction. Cependant, en termes de _Avalonia UI_, ces fichiers sont appelés 'Actifs' pour les distinguer des 'ressources XAML'.
:::


### Référencer les Actifs Inclus

Une fois que les fichiers d'actifs sont inclus, ils peuvent être référencés au besoin dans le XAML qui définit votre interface utilisateur. Par exemple, ces actifs sont référencés en spécifiant leur chemin relatif :

```xml
<Image Source="icon.png"/>
<Image Source="images/icon.png"/>
<Image Source="../icon.png"/>
```

En alternative, vous pouvez utiliser le chemin rooté :

```xml
<Image Source="/Assets/icon.png"/>
```

## Actifs de la bibliothèque

<img src={AssetLibraryDiagram} alt=''/>

Si l'actif est inclus dans une assembly différente de celle du fichier XAML, vous utilisez le schéma URI `avares:`. Par exemple, si l'actif est contenu dans une assembly appelée `MyAssembly.dll` dans un dossier `Assets`, alors vous utilisez :

```xml
<Image Source="avares://MyAssembly/Assets/icon.png"/>
```

### Conversion de type d'actif

Avalonia UI dispose de convertisseurs intégrés qui peuvent charger des actifs pour des images bitmap, des icônes et des polices dès le départ. Ainsi, un URI d'actif peut être automatiquement converti en l'un des types suivants :

* Image - type `Image`
* Bitmap - type `Bitmap`
* Icône de fenêtre - type `WindowIcon`
* Police - type `FontFamily`

### Chargement des actifs dans le code

Vous pouvez écrire du code pour charger des actifs en utilisant la classe statique `AssetLoader`. Par exemple :

```csharp title='C#'
var bitmap = new Bitmap(AssetLoader.Open(new Uri(uri)));
```

La variable `uri` dans le code ci-dessus peut contenir n'importe quel URI valide avec le schéma `avares:` (comme décrit ci-dessus).

_Avalonia UI_ does not provide support for `file://`, `http://`, or `https://` schemes. If you want to load files from disk or the Web, you must implement that functionality yourself or use community implementations.

:::info
Avalonia UI has a community implementation for an image loader at [https://github.com/AvaloniaUtils/AsyncImageLoader.Avalonia](https://github.com/AvaloniaUtils/AsyncImageLoader.Avalonia)
:::
