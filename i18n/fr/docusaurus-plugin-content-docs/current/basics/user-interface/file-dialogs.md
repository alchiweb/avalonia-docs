---
id: file-dialogs
title: Dialogues de fichiers
---

La fonctionnalité de dialogue de fichiers est accessible via l'API de service [`StorageProvider`](../../concepts/services/storage-provider), qui est disponible à partir des classes `Window` ou `TopLevel`. Cette page montre uniquement une utilisation de base et pour plus d'informations sur cette API, veuillez visiter la page StorageProvider.

<GitHubSampleLink title="Dialogue de fichiers" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/FileOps"/>

## OpenFilePickerAsync

Cette méthode ouvre un dialogue de sélection de fichiers, permettant à l'utilisateur de sélectionner un fichier. `FilePickerOpenOptions` définit les options qui sont passées au dialogue du système d'exploitation.

```cs
public class MyView : UserControl
{
    private async void OpenFileButton_Clicked(object sender, RoutedEventArgs args)
    {
        // Obtenir le niveau supérieur à partir du contrôle actuel. Alternativement, vous pouvez utiliser une référence à Window à la place.
        var topLevel = TopLevel.GetTopLevel(this);

        // Démarrer l'opération asynchrone pour ouvrir le dialogue.
        var fichiers = await topLevel.StorageProvider.OpenFilePickerAsync(new FilePickerOpenOptions
        {
            Title = "Ouvrir un fichier texte",
            AllowMultiple = false
        });

        if (fichiers.Count >= 1)
        {
            // Ouvrir le flux de lecture à partir du premier fichier.
            await using var stream = await fichiers[0].OpenReadAsync();
            using var streamReader = new StreamReader(stream);
            // Lit tout le contenu du fichier en tant que texte.
            var fileContent = await streamReader.ReadToEndAsync();
        }
    }
}
```

---

## SaveFilePickerAsync

Cette méthode ouvre une boîte de dialogue de sauvegarde de fichier, permettant à l'utilisateur de sauvegarder un fichier. `FilePickerSaveOptions` définit les options qui sont transmises à la boîte de dialogue du système d'exploitation.

### Exemple

```cs
public class MyView : UserControl
{
    private async void SaveFileButton_Clicked(object sender, RoutedEventArgs args)
    {
        // Obtenir le niveau supérieur à partir du contrôle actuel. Alternativement, vous pouvez utiliser une référence de fenêtre à la place.
        var topLevel = TopLevel.GetTopLevel(this);

        // Démarrer l'opération asynchrone pour ouvrir la boîte de dialogue.
        var file = await topLevel.StorageProvider.SaveFilePickerAsync(new FilePickerSaveOptions
        {
            Title = "Sauvegarder le fichier texte"
        });

        if (file is not null)
        {
            // Ouvrir le flux d'écriture à partir du fichier.
            await using var stream = await file.OpenWriteAsync();
            using var streamWriter = new StreamWriter(stream);
            // Écrire du contenu dans le fichier.
            await streamWriter.WriteLineAsync("Bonjour le monde !");
        }
    }
}
```

Pour plus d'informations sur le service StorageProvider, y compris sur la façon de conserver l'accès aux fichiers sélectionnés et sur les options possibles prises en charge, veuillez visiter la page de documentation [`StorageProvider`](../../concepts/services/storage-provider) et ses sous-pages.

:::note
Les exemples fournis accèdent directement à l'API [`StorageProvider`](../../concepts/services/storage-provider) à l'intérieur du ViewModel à des fins d'apprentissage. Dans une application réelle, il est recommandé de respecter les principes MVVM en créant des classes de service et en les localisant avec l'injection de dépendances / l'inversion de contrôle (DI/IoC). Veuillez vous référer aux projets [IoCFileOps](https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/IoCFileOps) et DepInject pour des exemples de la manière d'y parvenir.
:::




















