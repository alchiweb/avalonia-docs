---
id: launcher
title: Launcher
---

# Launcher <MinVersion version="11.1" />

Le `Launcher` vous permet d'ouvrir un fichier ou un lien URI dans l'application par défaut associée à l'argument spécifié.

Le `Launcher` peut être accédé via une instance de `TopLevel` ou `Window`. Pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../toplevel) :

```cs
var launcher = TopLevel.GetTopLevel(control).Launcher;
```

## Méthodes 

### LaunchUriAsync
Démarre l'application par défaut associée au nom du schéma URI pour l'URI spécifié.

```cs
Task<bool> LaunchUriAsync(Uri uri)
```

:::note
L'URI d'entrée peut avoir n'importe quel schéma, y compris des schémas personnalisés. Mais c'est à l'Operating System d'accepter ou de refuser cette demande de lanceur.
:::

### LaunchFileAsync
Démarre l'application par défaut associée au fichier ou au dossier de stockage spécifié.

```cs
Task<bool> LaunchFileAsync(IStorageItem storageItem);
```

:::note
IStorageItem est un fichier ou un dossier récupéré à partir d'APIs sandboxées telles que IStorageProvider ou IClipboard.
Si vous ciblez uniquement des plateformes de bureau non sandboxées, envisagez d'utiliser des méthodes d'extension acceptant FileInfo ou DirectoryInfo.
:::

## Méthodes d'Extension

### LaunchFileInfoAsync
Démarre l'application par défaut associée au fichier de stockage spécifié.

```cs
Task<bool> LaunchFileInfoAsync(FileInfo fileInfo)
```

### LaunchFileAsync
Démarre l'application par défaut associée au répertoire de stockage spécifié (dossier).

```cs
Task<bool> LaunchDirectoryInfoAsync(DirectoryInfo directoryInfo);
```

:::note
Chacune de ces méthodes renvoie un résultat booléen indiquant si le système d'exploitation peut traiter la demande ou non.
Cela ne garantit pas qu'il existe une application capable de gérer la demande de lanceur.
:::

## Compatibilité avec les différentes plateformes :

| Fonctionnalité        | Windows | macOS | Linux | Browser | Android |  iOS | Tizen |
|---------------|-------|-------|-------|-------|-------|-------|-------|
| `LaunchUriAsync` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `LaunchFileAsync` | ✔ | ✔ | ✔ | ✖ | ✔ | ✔ | ✔ |
| `LaunchFileInfoAsync` | ✔ | ✔ | ✔ | ✖ | ✖ | ✖ | ✖ |
| `LaunchDirectoryInfoAsync` | ✔ | ✔ | ✔ | ✖ | ✖ | ✖ | ✖ |
