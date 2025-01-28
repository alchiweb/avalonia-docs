---
description: CONCEPTS
---

# La Fenêtre Principale

La fenêtre principale est la fenêtre passée à `ApplicationLifetime.MainWindow` dans la méthode `OnFrameworkInitializationCompleted` de votre fichier `App.axaml.cs` :

```csharp
public override void OnFrameworkInitializationCompleted()
{
    if (ApplicationLifetime is IClassicDesktopStyleApplicationLifetime desktopLifetime)
    {
        desktopLifetime.MainWindow = new MainWindow();
    }
}
```

Elle peut être récupérée à tout moment en castant `Application.Current.ApplicationLifetime` en `IClassicDesktopStyleApplicationLifetime`.

Il convient de mentionner que les développeurs doivent garder à l'esprit que l'utilisation de variables globales statiques et l'accès à MainWindow depuis n'importe quel endroit de l'application peuvent être dangereux et parfois causer une mauvaise expérience utilisateur. Toutes les API liées aux fenêtres de niveau supérieur doivent être utilisées depuis le niveau supérieur le plus spécifique, généralement, c'est le dernier actif. De cette manière, les dialogues utilisateur ne seront pas ouverts depuis la mauvaise fenêtre, par exemple.

:::warning
Les plateformes mobiles et de navigateur n'ont pas de concept de fenêtre dans Avalonia. Au lieu de cela, vous devez définir le contrôle MainView dans Application.ApplicationLifetime lorsqu'il implémente l'interface ISingleViewApplicationLifetime.
:::

### <a href="#show-hide-and-close-a-window" id="show-hide-and-close-a-window"></a>