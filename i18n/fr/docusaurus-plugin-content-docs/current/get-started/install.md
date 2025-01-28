---
id: install
title: Installez
---

## Préinstallation

Veuillez commencer par installer l'IDE de votre choix pris en charge. Avalonia prend en charge Visual Studio, Rider et Visual Studio Code.

## Installer les modèles Avalonia UI

La meilleure façon de commencer avec Avalonia est de créer une application en utilisant un modèle de projet.

Pour installer les [modèles Avalonia](https://github.com/AvaloniaUI/avalonia-dotnet-templates), exécutez la commande suivante :

```bash title='Bash'
dotnet new install Avalonia.Templates
```

:::note
Pour .NET 6.0 et les versions antérieures, remplacez `install` par `--install`
:::

Pour lister les modèles installés, exécutez :

```bash title='Bash'
 dotnet new list
```

Vous devriez voir les modèles Avalonia installés :

```
Nom du modèle                                 Nom court                   Langage     Tags (étiquettes)
--------------------------------------------  --------------------------  ----------  ---------------------------------------------------------
Avalonia App                                  avalonia.app                [C#],F#     Desktop/Xaml/Avalonia/Windows/Linux/macOS
Avalonia MVVM App                             avalonia.mvvm               [C#],F#     Desktop/Xaml/Avalonia/Windows/Linux/macOS
Avalonia Cross Platform Application           avalonia.xplat              [C#],F#     Desktop/Xaml/Avalonia/Web/Mobile
Avalonia Resource Dictionary                  avalonia.resource                       Desktop/Xaml/Avalonia/Windows/Linux/macOS
Avalonia Styles                               avalonia.styles                         Desktop/Xaml/Avalonia/Windows/Linux/macOS
Avalonia TemplatedControl                     avalonia.templatedcontrol   [C#],F#     Desktop/Xaml/Avalonia/Windows/Linux/macOS
Avalonia UserControl                          avalonia.usercontrol        [C#],F#     Desktop/Xaml/Avalonia/Windows/Linux/macOS
Avalonia Window                               avalonia.window             [C#],F#     Desktop/Xaml/Avalonia/Windows/Linux/macOS
```

## Créer une nouvelle application

Une fois que les modèles de projet sont installés, vous pouvez créer une nouvelle application _Avalonia UI_ depuis la CLI en exécutant la commande suivante :

```bash title='Bash'
dotnet new avalonia.app -o MyApp
```

Cela créera un nouveau dossier appelé `MyApp` contenant vos fichiers d'application. Pour exécuter l'application, naviguez vers le dossier `MyApp` et exécutez :

```bash title='Bash'
cd MyApp
dotnet run
```

Les modèles de projet permettront également la création de projets depuis votre IDE.

## Dépannage de l'installation

### Assurez-vous que le SDK .NET est installé

```
dotnet --list-sdks

8.0.202 [C:\Program Files\dotnet\sdk] <-- Votre version peut varier
```

Si `dotnet` n'est pas un programme reconnu, assurez-vous d'abord d'avoir installé votre IDE. Ensuite, assurez-vous que `dotnet` est associé au terminal. Sur Windows, cela implique de vérifier les variables d'environnement : `echo %PATH%` dans l'invite de commandes ou `echo $Env:PATH` dans PowerShell.

### Assurez-vous que la source NuGet est correcte

Si lors de l'installation des modèles de projet, vous recevez une erreur indiquant que le package `Avalonia.Templates` ne peut pas être trouvé, assurez-vous que NuGet est correctement configuré avec la source de package globale standard de .NET.

```
dotnet nuget list source

Sources enregistrées :
  1.  nuget.org [Activé]
      https://api.nuget.org/v3/index.json
```

Si cette source n'est pas répertoriée, ajoutez-la :

```
dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org
```

Si l'installation du package échoue toujours malgré la présence de NuGet, soupçonnez un problème de connectivité réseau ou de pare-feu.
