---
id: how-to-bind-image-files
title: Comment lier des fichiers image
---


# Comment lier des fichiers image


<GitHubSampleLink title="Chargement des images" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/LoadingImages"/>


Dans Avalonia UI, lier un fichier image ouvre des opportunités pour afficher du contenu d'image dynamique au sein de votre application. Ce guide fournit un aperçu sur la manière de lier des fichiers image provenant de diverses sources.

## Lier des fichiers image provenant de diverses sources

En supposant que vous ayez des images provenant de diverses sources (c'est-à-dire une ressource locale ou une URL web) que vous souhaitez afficher dans votre vue, voici comment vous pouvez y parvenir :

Tout d'abord, dans votre `ViewModel`, vous devez définir des propriétés qui représentent ces sources d'image. Les propriétés peuvent être de type `Bitmap` ou `Task<Bitmap>` (si le chargement de l'image implique une opération asynchrone). La classe `ImageHelper` est utilisée pour charger ces images.

```csharp
public class MainWindowViewModel : ViewModelBase
{
    public Bitmap? ImageFromBinding { get; } = ImageHelper.LoadFromResource(new Uri("avares://LoadingImages/Assets/abstract.jpg"));
    public Task<Bitmap?> ImageFromWebsite { get; } = ImageHelper.LoadFromWeb(new Uri("https://upload.wikimedia.org/wikipedia/commons/4/41/NewtonsPrincipia.jpg"));
}
```

Vous aurez besoin d'une classe d'aide `ImageHelper` qui fournit des méthodes pour charger des images à partir de ressources et d'une URL web. Voici comment vous pouvez implémenter cette classe :

```csharp
using System;
using System.IO;
using System.Net.Http;
using System.Threading.Tasks;
using Avalonia;
using Avalonia.Media.Imaging;
using Avalonia.Platform;

namespace ImageExample.Helpers
{
    public static class ImageHelper
    {
        public static Bitmap LoadFromResource(Uri resourceUri)
        {
            return new Bitmap(AssetLoader.Open(resourceUri));
        }

        public static async Task<Bitmap?> LoadFromWeb(Uri url)
        {
            using var httpClient = new HttpClient();
            try
            {
                var response = await httpClient.GetAsync(url);
                response.EnsureSuccessStatusCode();
                var data = await response.Content.ReadAsByteArrayAsync();
                return new Bitmap(new MemoryStream(data));
            }
            catch (HttpRequestException ex)
            {
                Console.WriteLine($"Une erreur est survenue lors du téléchargement de l'image '{url}' : {ex.Message}");
                return null;
            }
        }
    }
}
```

La méthode `LoadFromResource` prend une URI de ressource et charge l'image en utilisant la classe `AssetLoader` fournie par Avalonia. La méthode `LoadFromWeb` charge une image à partir d'une URL web en utilisant la classe `HttpClient`.

Ensuite, dans votre vue, vous pouvez lier ces sources d'image aux contrôles `Image` :

```xml
<Grid ColumnDefinitions="*,*,*" RenderOptions.BitmapInterpolationMode="HighQuality">
    <Image Grid.Column="0" Source="avares://LoadingImages/Assets/abstract.jpg" MaxWidth="300" />
    <Image Grid.Column="1" Source="{Binding ImageFromBinding}" MaxWidth="300" />
    <Image Grid.Column="2" Source="{Binding ImageFromWebsite^}" MaxWidth="300" />
</Grid>
```

La propriété `Source` du contrôle `Image` peut accepter divers types de sources d'image, y compris un chemin de fichier, une URL ou une ressource. Veuillez noter que pour les sources d'image asynchrones, vous devez utiliser le caractère `^` après l'expression de liaison pour indiquer à Avalonia qu'il s'agit d'une liaison asynchrone.

Assurez-vous que les chemins de fichiers d'image locaux sont exacts, que le fichier image est accessible, et s'il fait partie de vos ressources d'application, qu'il a été correctement inclus dans votre projet. Si vous liez une image web, assurez-vous que l'URL est accessible.
