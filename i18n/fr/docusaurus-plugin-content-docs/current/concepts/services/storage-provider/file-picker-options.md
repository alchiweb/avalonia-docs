---
id: file-picker-options
title: Options du sélecteur de fichiers
---

# Options du sélecteur de fichiers

## PickerOptions principales

### Title

Obtient ou définit le texte qui apparaît dans la barre de titre d'un sélecteur.

### SuggestedStartLocation

Obtient ou définit l'emplacement initial où le sélecteur de fichiers ouverts cherche des fichiers à présenter à l'utilisateur.  
Peut être obtenu à partir d'un dossier précédemment sélectionné ou en utilisant `StorageProvider.TryGetFolderFromPathAsync` ou `StorageProvider.TryGetWellKnownFolderAsync`.

:::note  
C'est une suggestion pour le système, qui peut ignorer ce paramètre, si l'application n'a pas accès au dossier ou s'il n'existe pas.  
:::  
:::note  
Sur Linux, certains sélecteurs de fichiers DBus ne prennent pas en charge l'emplacement de départ. Pour utiliser GTK Free Desktop, désactivez `UseDBusFilePicker` dans `X11PlatformOptions`  
:::

## FilePickerOpenOptions

### AllowMultiple

Obtient ou définit une option indiquant si le sélecteur ouvert permet aux utilisateurs de sélectionner plusieurs fichiers.

### FileTypeFilter

Obtient ou définit la collection de types de fichiers que le sélecteur de fichiers ouverts affiche.

Pour créer une liste de types de fichiers pour le sélecteur de fichiers :

```cs
//Cela peut également s'appliquer au SaveFilePicker.
var files = await _target.StorageProvider.OpenFilePickerAsync(new FilePickerOpenOptions()
{
 Titre = titre,
//Vous pouvez ajouter soit des types personnalisés, soit des types de fichiers intégrés. Voir "Définir des types de fichiers personnalisés" sur la façon d'en créer un personnalisé.
 FileTypeFilter = new[] { ImageAll, FilePickerFileTypes.TextPlain }
});
```

## FilePickerSaveOptions

### SuggestedFileName

Obtient ou définit le nom de fichier que le sélecteur de fichiers à enregistrer suggère à l'utilisateur.

### DefaultExtension

Obtient ou définit l'extension par défaut à utiliser pour enregistrer le fichier.

### FileTypeChoices

Obtient ou définit la collection de types de fichiers valides que l'utilisateur peut choisir d'assigner à un fichier.

### ShowOverwritePrompt

Obtient ou définit une valeur indiquant si le sélecteur de fichiers affiche un avertissement si l'utilisateur spécifie le nom d'un fichier qui existe déjà.

## FolderPickerOpenOptions

### AllowMultiple

Obtient ou définit une option indiquant si le sélecteur ouvert permet aux utilisateurs de sélectionner plusieurs dossiers.

## Compatibilité avec les différentes plateformes :

| Fonctionnalité        | Managed |  Windows | macOS | Linux | Browser | Android |  iOS |
|---------------|-------|-------|-------|-------|-------|-------|-------|
| `Title` | ✔ | ✔ | ✔ | ✔ | ✖ | ✔ | ✔ |
| `SuggestedStartLocation` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `AllowMultiple` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `FileTypeFilter` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `SuggestedFileName` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✖ |
| `DefaultExtension` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✖ |
| `FileTypeChoices` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✖ |
| `ShowOverwritePrompt` | ✔ | ✔ | ✖ | ✔ | ✖ | ✖ | ✖ |

# Définir des types de fichiers personnalisés

Avalonia dispose d'un ensemble de types de fichiers intégrés :

- FilePickerFileTypes.All - tous les fichiers
- FilePickerFileTypes.TextPlain - fichiers txt
- FilePickerFileTypes.ImageAll - toutes les images
- FilePickerFileTypes.ImageJpg - images jpg
- FilePickerFileTypes.ImagePng - images png
- FilePickerFileTypes.ImageWebP - images webp
- FilePickerFileTypes.Pdf - documents pdf

Cependant, il est possible de définir des types de fichiers personnalisés qui peuvent être utilisés par le sélecteur.

Par exemple, le type intégré ImageAll est défini comme suit :

Où chaque type de fichier a les indices suivants qui sont utilisés par les différentes plateformes :

- `Patterns` sont utilisés par la plupart des plateformes Windows, Linux et Browser, et est un modèle GLOB de base qui peut être associé à des types.
- `AppleUniformTypeIdentifiers` est un identifiant standard défini par Apple et est utilisé sur les plateformes macOS et iOS. Vous pouvez trouver la valeur correcte pour un fichier donné dans le terminal macOS avec `mdls -name kMDItemContentType votrefichier.ext`.
- `MimeTypes` est un identifiant web pour les fichiers utilisés sur la plupart des plateformes, mais pas sur Windows et iOS.

Il est recommandé de définir tous les indices si l'information est connue.

:::note
Si un indice spécifique n'est pas connu, ne définissez pas de valeurs aléatoires ou le caractère générique "*.*", mais laissez cette collection nulle. Cela indiquera à la plateforme d'ignorer cette collection et d'essayer d'en utiliser une autre.
:::

## Inclusion de WebP dans les Options <MinVersion version="11.1" />

Gardez à l'esprit que `FilePickerFileTypes.ImageWebP` et l'ajout de "*.webp" aux modèles "Toutes les images" ont été introduits dans la version 11.1. Vous pouvez toujours créer des types de sélection de fichiers personnalisés dans les versions antérieures pour incorporer des images WebP. Par exemple, pour autoriser uniquement le choix d'une image WebP, vous pouvez utiliser ceci :

```cs
var customWebPFileType = new FilePickerFileType("Seulement des images WebP")
{
    Patterns = new[] { "*.webp" },
    AppleUniformTypeIdentifiers = new[] { "org.webmproject.webp" },
    MimeTypes = new[] { "image/webp" }
};
```

Et si vous souhaitez inclure WebP comme l'un des types de fichiers que vous considérez comme une image, vous pouvez utiliser l'exemple "ImageAll" montré ci-dessus.