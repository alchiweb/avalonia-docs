---
id: how-to-use-fonts
title: Comment Utiliser des Polices Personnalisées
---

Personnaliser votre application Avalonia avec des polices uniques peut lui donner un aspect et une sensation distincts. Ce guide vous expliquera le processus d'intégration de polices personnalisées dans votre application Avalonia.

<GitHubSampleLink title="Google Fonts" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/GoogleFonts"/>

## Ajoutez Votre Police Personnalisée au Projet

Avant de pouvoir utiliser une police personnalisée, vous devez l'inclure dans votre projet.

Dans ce guide, nous utiliserons une police appelée [Nunito](https://fonts.google.com/specimen/Nunito) qui est déjà stockée dans les ressources de notre application sous `avares://GoogleFonts/Assets/Fonts`.

Assurez-vous que les polices ont la propriété de construction définie sur `AvaloniaResource`.

## Déclarez Votre Police dans les Ressources de l'Application

Dans votre application Avalonia, ouvrez votre fichier `App.xaml` et incluez votre police personnalisée à l'intérieur de `<Application.Resources>`. Attribuez-lui une clé, que vous utiliserez pour y faire référence dans votre application. Dans ce cas, nous avons attribué la clé `NunitoFont`.

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="GoogleFonts.App"
             RequestedThemeVariant="Default">
    <Application.Styles>
        <FluentTheme />
    </Application.Styles>
    // highlight-start
    <Application.Resources>
        <FontFamily x:Key="NunitoFont">avares://GoogleFonts/Assets/Fonts#Nunito</FontFamily>
    </Application.Resources>
     // highlight-end
</Application>
```

## Utilisez Votre Police Personnalisée
Une fois votre police déclarée dans les ressources de votre application, vous pouvez l'utiliser dans votre application.

Pour faire référence à votre police personnalisée, utilisez l'attribut `FontFamily` avec l'extension de balisage `StaticResource`. Vous devez passer la clé de la police déclarée en tant que paramètre. Dans ce cas, `NunitoFont` est la clé de notre police personnalisée.

Voici un exemple de la façon d'appliquer notre police personnalisée `Nunito` à un `TextBlock` :

```xml
<TextBlock Text="{Binding Greeting}" 
           FontSize="70" 
           FontFamily="{StaticResource NunitoFont}" 
           HorizontalAlignment="Center" VerticalAlignment="Center"/>
```

Dans l'exemple ci-dessus, le contrôle `TextBlock` utilisera la police `Nunito` que nous avons déclarée dans les ressources de notre application. Le texte lié au `TextBlock` apparaîtra désormais dans la police `Nunito` à la taille de police spécifiée de 70.

N'oubliez pas que l'attribut `FontFamily` peut être appliqué à tout contrôle ayant la propriété `FontFamily`, ce qui signifie que vous pouvez utiliser votre police personnalisée dans toute votre application.

Et c'est tout ! Vous avez intégré avec succès une police personnalisée dans votre application Avalonia. Vous pouvez maintenant ajouter une touche unique à l'interface utilisateur de votre application avec les polices de votre choix.

## Ajouter une police à la collection de polices

La `EmbeddedFontCollection` offre un moyen facile d'ajouter des polices à la collection de polices de votre application sans nécessiter de référence à une ressource lors de leur utilisation. Par exemple, au lieu de définir le `FontFamily` sur `{StaticResource NunitoFont}`, vous pouvez simplement référencer le nom de la famille de polices directement.

```xml
<TextBlock
    Text="{Binding Greeting}"
    FontSize="70"
    FontFamily="Nunito"
    HorizontalAlignment="Center" VerticalAlignment="Center" />
```

Cela nécessite une configuration supplémentaire lors de la construction de l'application, mais cela élimine le besoin de se souvenir d'une clé de ressource unique lors de la création de vos contrôles.

### Définir une collection de polices

Tout d'abord, nous devons définir une collection de polices en spécifiant le nom de la famille de polices et le répertoire dans lequel se trouvent les fichiers de police.

```csharp title="InterFontCollection.cs"
public sealed class InterFontCollection : EmbeddedFontCollection
{
    public InterFontCollection() : base(
        new Uri("fonts:Inter", UriKind.Absolute),
        new Uri("avares://Avalonia.Fonts.Inter/Assets", UriKind.Absolute))
    {
    }
}
```

Ici, `Inter` est le nom de la famille de polices et `avares://Avalonia.Fonts.Inter/Assets` est le localisateur de ressource pour le répertoire contenant les fichiers de police. Les noms réels des fichiers de police ne sont pas significatifs puisque la `EmbeddedFontCollection` recherchera chaque fichier dans le répertoire donné et ne chargera que les polices correspondant au nom de famille de police donné.

Pour plus d'informations sur la façon de créer un localisateur de ressources, consultez [Assets](../../basics/user-interface/assets) pour un aperçu sur l'inclusion des ressources dans votre projet.

### Ajout de la collection de polices

Ensuite, nous devons ajouter cette collection de polices à l'application. Cela peut être fait en utilisant `AppBuilder.ConfigureFonts` pour configurer le `FontManager` afin d'inclure vos polices lors de la construction de l'application.

```csharp title="Program.cs"
public static class Program
{
    [STAThread]
    public static void Main(string[] args) =>
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);

    public static AppBuilder BuildAvaloniaApp() =>
        AppBuilder.Configure<App>()
            .UsePlatformDetect()
// highlight-start
            .ConfigureFonts(fontManager =>
            {
                fontManager.AddFontCollection(new InterFontCollection());
            })
// highlight-end
            .LogToTrace();
}
```

### Création de paquets de polices

Le paquet [`Avalonia.Fonts.Inter`](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Fonts.Inter) montre comment vous pouvez créer un projet séparé pour contenir une ou plusieurs polices que vous pourriez utiliser dans plusieurs projets. Une fois que vous avez créé et publié un projet similaire à celui-ci, l'utilisation de la police devient aussi simple que d'ajouter un appel de méthode à la construction de votre application.

```csharp title="Program.cs"
public static class Program
{
    [STAThread]
    public static void Main(string[] args) =>
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);

    public static AppBuilder BuildAvaloniaApp() =>
        AppBuilder.Configure<App>()
            .UsePlatformDetect()
// highlight-start
            .WithInterFont()
// highlight-end
            .LogToTrace();
}
```

## Quelles polices sont prises en charge ?

La plupart des polices TrueType (`.ttf`) et OpenType (`.otf`, `.ttf`) sont prises en charge. Cependant, certaines fonctionnalités de police, telles que les "polices variables", ne sont pas actuellement prises en charge (voir : [Problème #11092](https://github.com/AvaloniaUI/Avalonia/issues/11092)).
