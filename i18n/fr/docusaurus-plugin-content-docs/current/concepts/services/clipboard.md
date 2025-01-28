---
id: clipboard
title: Clipboard
---

La classe `Clipboard` permet d'interagir avec le presse-papiers du système, offrant des fonctionnalités pour définir et récupérer du texte, vider le presse-papiers, gérer des objets de données et travailler avec différents formats de données.

<GitHubSampleLink title="Clipboard" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/ClipboardOps"/>

Le `Clipboard` peut être accédé via une instance de `TopLevel` ou `Window`. Pour plus de détails sur l'accès à `TopLevel`, veuillez visiter la page [TopLevel](../toplevel) :
```cs
var clipboard = window.Clipboard;
```

## Méthodes

### GetTextAsync()

Récupère le texte du presse-papiers de manière asynchrone. La valeur résultante de la tâche est le texte du presse-papiers. Si le presse-papiers ne contient pas de texte ou est vide, la méthode retourne `null`.

```cs
Task<string?> GetTextAsync()
```

:::note
Le presse-papiers d'Avalonia fonctionne toujours avec du texte Unicode.
:::

### SetTextAsync(string? text)
Définit le texte du presse-papiers de manière asynchrone et le vide immédiatement. Cette méthode accepte un paramètre `string?` pour un texte qui doit être copié. Si le texte fourni est `null`, le presse-papiers sera vidé.

```cs
Task SetTextAsync(string? text)
```

:::note
Contrairement aux différentes API de presse-papiers Win32, le presse-papiers d'Avalonia vide toujours les données et n'est jamais retardé.
:::

### ClearAsync()
Vide le presse-papiers de manière asynchrone et le vide immédiatement.

```cs
Task ClearAsync()
```

### SetDataObjectAsync(IDataObject data)
Définit le contenu du presse-papiers sur l'objet de données spécifié de manière asynchrone. Cette méthode accepte un paramètre `IDataObject`. L'objet de données peut contenir plusieurs formats de données.

```cs
Task SetDataObjectAsync(IDataObject data)
```

:::note
Contrairement aux différentes API de presse-papiers Win32, le presse-papiers Avalonia vide toujours les données et n'est jamais retardé.
:::

### GetFormatsAsync()
Récupère la liste des formats actuellement stockés dans le presse-papiers de manière asynchrone. La valeur résultante de la tâche est un tableau de noms de formats de chaîne.

```cs
Task<string[]> GetFormatsAsync()
```

### GetDataAsync(string format)
Récupère des données dans le format spécifié du presse-papiers de manière asynchrone. Cette méthode renvoie un `Task<object?>` qui représente l'opération. La valeur résultante de la tâche est les données du presse-papiers dans le format spécifié. S'il n'y a pas de données dans le presse-papiers dans le format spécifié, la méthode renvoie `null`.

```cs
Task<object?> GetDataAsync(string format)
```

## Création d'un DataObject à envoyer au presse-papiers

Vous pouvez stocker des objets dans le presse-papiers sur certaines plateformes avec différents formats.

```csharp title='C#'
private async void CopyButton_OnClick(object? sender, RoutedEventArgs args)
{
    var clipboard = TopLevel.GetTopLevel(this)?.Clipboard;
    var dataObject = new DataObject();
    dataObject.Set(DataFormats.Text, "Hello World");
    await clipboard.SetDataObjectAsync(dataObject);
}
```

## Compatibilité avec les différentes plateformes :

| Fonctionnalité        |  Windows | macOS | Linux x11 | Browser | Android |  iOS |
|---------------|-------|-------|-------|-------|-------|-------|
| `GetTextAsync` | ✔ | ✔ | ✔ | ✔** | ✔ | ✔ |
| `SetTextAsync` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `ClearAsync` | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| `GetFormatsAsync` | ✔ | ✔ | ✔ | ✖* | ✖* | ✖* |
| `SetDataObjectAsync` | ✔ | ✔ | ✔ | ✖* | ✖* | ✖* |
| `GetDataAsync` | ✔ | ✔ | ✔ | ✖* | ✖* | ✖* |

\* Techniquement possible, mais pas encore implémenté. Les contributions sont les bienvenues !

** Dans le navigateur Mozilla, la méthode GetTextAsync ne fonctionne que après que le geste "Coller" a été déclenché, généralement en utilisant Ctrl+V.