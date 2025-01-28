---
id: index
title: StorageProvider
---
# StorageProvider

Le `StorageProvider` est central pour la gestion des fichiers et des dossiers. Il fournit des méthodes pour la sélection de fichiers et de dossiers, vérifie les capacités de la plateforme et interagit avec les favoris stockés.

Le `StorageProvider` peut être accédé via une instance de `TopLevel` ou `Window`, pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../../toplevel) :
```cs
var storage = window.StorageProvider;
```

## Propriétés 

### CanOpen
Indique s'il est possible d'ouvrir un `sélecteur de fichiers` sur la plateforme actuelle.

```cs
bool CanOpen { get; }
```

### CanSave
Indique s'il est possible d'ouvrir un `sélecteur d'enregistrement de fichiers` sur la plateforme actuelle.

```cs
bool CanSave { get; }
```

### CanPickFolder
Indique s'il est possible d'ouvrir un `sélecteur de dossiers` sur la plateforme actuelle.

```cs
bool CanPickFolder { get; }
```

## Méthodes

### OpenFilePickerAsync
Ouvre une boîte de dialogue de sélection de fichiers.

```cs
Task<IReadOnlyList<IStorageFile>> OpenFilePickerAsync(FilePickerOpenOptions options);
```
La méthode renvoie un tableau d'instances `IStorageFile` sélectionnées ou une collection vide si l'utilisateur annule la boîte de dialogue.

### SaveFilePickerAsync
Ouvre une boîte de dialogue de sélection d'enregistrement de fichiers.

```cs
Task<IStorageFile?> SaveFilePickerAsync(FilePickerSaveOptions options);
```
La méthode renvoie une instance `IStorageFile` enregistrée ou null si l'utilisateur annule la boîte de dialogue.

### OpenFolderPickerAsync
Ouvre une boîte de dialogue de sélection de dossiers.

```cs
Task<IReadOnlyList<IStorageFolder>> OpenFolderPickerAsync(FolderPickerOpenOptions options);
```
La méthode renvoie un tableau d'instances `IStorageFolder` sélectionnées ou une collection vide si l'utilisateur annule la boîte de dialogue.

### OpenFileBookmarkAsync
Ouvre un `IStorageBookmarkFile` à partir de l'ID du signet.

```cs
Task<IStorageBookmarkFile?> OpenFileBookmarkAsync(string bookmark);
```
La méthode renvoie un fichier marqué ou null si le système d'exploitation a refusé la demande.

### OpenFolderBookmarkAsync
Ouvre un `IStorageBookmarkFolder` à partir de l'ID du signet.

```cs
Task<IStorageBookmarkFolder?> OpenFolderBookmarkAsync(string bookmark);
```
La méthode renvoie un dossier marqué ou null si le système d'exploitation a refusé la demande.

### TryGetFileFromPathAsync
Tente de lire un fichier à partir du système de fichiers par son chemin.

```cs
Task<IStorageFile?> TryGetFileFromPathAsync(Uri filePath);
```
La méthode renvoie un fichier ou null s'il n'existe pas. Le paramètre filePath est censé être un chemin absolu avec un schéma "file", mais peut être une URI avec un schéma "content" sur Android.

### TryGetFolderFromPathAsync
Tente de lire un dossier à partir du système de fichiers par son chemin.

```cs
Task<IStorageFolder?> TryGetFolderFromPathAsync(Uri folderPath);
```
La méthode renvoie un dossier ou null s'il n'existe pas. Le paramètre folderPath est censé être un chemin absolu avec un schéma "file", mais peut être une URI avec un schéma "content" sur Android.

### TryGetWellKnownFolderAsync
Tente de lire un dossier à partir du système de fichiers par son identifiant de dossier bien connu.

```cs
Task<IStorageFolder?> TryGetWellKnownFolderAsync(WellKnownFolder wellKnownFolder);
```
La méthode renvoie un dossier ou null s'il n'existe pas.

## Méthodes d'extension

### TryGetFileFromPathAsync
Tente de lire un fichier à partir du système de fichiers par son chemin.

```cs
Task<IStorageFile?> TryGetFileFromPathAsync(this IStorageProvider provider, string filePath);
```
La méthode renvoie un fichier ou null s'il n'existe pas.
Cette méthode accepte une chaîne de chemin de fichier local comme paramètre sans aucun schéma.
Uniquement pris en charge sur le système d'exploitation, avec des chemins de fichiers physiques, principalement sur le bureau.

### TryGetFolderFromPathAsync
Tente de lire un dossier à partir du système de fichiers par son chemin.

```cs
Task<IStorageFolder?> TryGetFolderFromPathAsync(this IStorageProvider provider, string folderPath);
```
La méthode renvoie un dossier ou null s'il n'existe pas.
Cette méthode accepte une chaîne de chemin de dossier local comme paramètre sans aucun schéma.
Uniquement pris en charge sur le système d'exploitation, avec des chemins de fichiers physiques, principalement sur le bureau.

## Compatibilité avec les différentes plateformes :

| Fonctionnalité         | Managed |  Windows | macOS | Linux | Browser | Android |  iOS |
|---------------|-------|-------|-------|-------|-------|-------|-------|
| `OpenFileBookmarkAsync` | ✔* | ✔* | ✔* | ✔* | ✔ | ✔ | ✔ |
| `OpenFolderBookmarkAsync` | ✔* | ✔* | ✔* | ✔* | ✔ | ✔ | ✔ |
| `OpenFilePickerAsync` | ✔** | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `SaveFilePickerAsync` | ✔** | ✔ | ✔ | ✔ | ✔ | ✔*** | ✖ |
| `OpenFolderPickerAsync` | ✔** | ✔ | ✔ | ✔ | ✔ | ✔*** | ✔ |
| `TryGetFileFromPathAsync` | ✔ | ✔ | ✔ | ✔ | ✖ | ✖ | ✖ |
| `TryGetFolderFromPathAsync` | ✔ | ✔ | ✔ | ✔ | ✖ | ✖ | ✖ |
| `TryGetWellKnownFolderAsync` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |

\* Les signets ne sont pas correctement pris en charge sur les plateformes de bureau et renvoient à la place le chemin du fichier. Le support macOS est prévu afin de fonctionner avec les applications Sandboxed de l'Apple Store.

** Le sélecteur de fichiers géré ne fonctionne que sur les plateformes de bureau où il est possible d'ouvrir une fenêtre personnalisée.

*** Seuls les navigateurs basés sur Chromium ont un support approprié pour les sélecteurs de fichiers.