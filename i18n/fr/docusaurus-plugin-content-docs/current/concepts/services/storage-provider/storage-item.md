---
id: storage-item
title: Éléments de stockage
---

# Éléments de stockage

## Membres communs pour StorageFile et StorageFolder

### Name

Obtient un nom court de l'élément incluant l'extension du nom de fichier s'il y en a une.

### Path

Obtient le chemin du système de fichiers de l'élément.

:::note
Le backend Android peut renvoyer le chemin du fichier avec le schéma "content:".
Les backends de navigateur et iOS peuvent renvoyer des URI relatifs.
:::

:::warning
NE PAS utiliser la propriété Chemin pour préserver l'accès au fichier ou au dossier. Voir plutôt la page [Signets](./bookmarks) sur comment garder l'accès aux éléments de stockage.

NE PAS utiliser la propriété Chemin pour lire directement le fichier par son chemin, car cela ne fonctionnera pas sur la plupart des plateformes mobiles et de navigateur. Utilisez plutôt [OpenReadAsync](#openreadasync) et [OpenWriteAsync](#openwriteasync).
:::

### CanBookmark

Renvoie vrai si l'élément peut être mis en signet et réutilisé plus tard.

### SaveBookmarkAsync

Enregistre les éléments dans un signet.
Renvoie l'identifiant d'un signet. Peut être nul si le système d'exploitation a refusé la demande.

### GetBasicPropertiesAsync

Obtient les propriétés de base de l'élément actuel.
Propriétés actuellement disponibles :
- Size
- DateCreated
- DateModified

### GetParentAsync

Obtient le dossier parent de l'élément de stockage actuel.

### DeleteAsync

Supprime l'élément de stockage actuel et son contenu.

### MoveAsync

Déplace l'élément de stockage actuel et son contenu vers un IStorageFolder.

## Membres de StorageFile

### OpenReadAsync

Ouvre un flux pour un accès en lecture.

### OpenWriteAsync

Ouvre un flux pour écrire dans le fichier.

## Membres de StorageFolder

### GetItemsAsync

Obtient les fichiers et sous-dossiers dans le dossier actuel.  
Lorsque cette méthode se termine avec succès, elle renvoie une liste des fichiers et dossiers dans le dossier actuel. Chaque élément de la liste est représenté par un objet d'implémentation IStorageItem.

:::note  
Cette méthode est évaluée paresseusement et est asynchrone.  
:::

### CreateFileAsync

Crée un fichier avec le nom spécifié en tant qu'enfant du dossier de stockage actuel.

### CreateFolderAsync

Crée un dossier avec le nom spécifié en tant qu'enfant du dossier de stockage actuel.

## Méthodes d'extension

### TryGetLocalPath

Obtient le chemin du système de fichiers local de l'élément sous forme de chaîne.  
La plateforme Android utilise généralement des chemins de fichiers virtuels "content:" et la plateforme navigateur a un accès isolé sans chemins complets, donc sur ces plateformes, cette méthode renverra null.

:::note  
Si vous souhaitez enregistrer le chemin du fichier pour le réutiliser plus tard (en combinaison avec TryGetFileFromPathAsync), veuillez envisager d'utiliser [Bookmarks](./bookmarks) à la place, car elles sont conçues pour fonctionner dans un environnement isolé, où l'application utilisateur pourrait ne pas avoir un accès direct au système de fichiers physique.  
:::