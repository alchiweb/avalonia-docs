---
id: fluent
title: Thème Fluent
---

import FluentThemeNormalScreenshot from '/img/basics/user-interface/styling/fluent-theme-normal.png';
import FluentThemeForestScreenshot from '/img/basics/user-interface/styling/fluent-theme-forest.png';

## Introduction

Le thème Avalonia Fluent s'inspire du système de design Fluent de Microsoft, qui est un ensemble de directives et de composants de design pour créer des interfaces utilisateur visuellement attrayantes et interactives. Le système de design Fluent met l'accent sur une esthétique moderne et épurée, des animations fluides et des interactions intuitives. Il offre un aspect et une sensation cohérents et soignés sur différentes plateformes, tout en donnant aux développeurs la flexibilité avec notre système de style.

<p><img className="medium-image-zoom" src={FluentThemeNormalScreenshot} alt="Thème Fluent" /></p>

## Comment l'utiliser

Comme première étape, le package nuget [Avalonia.Themes.Fluent](https://www.nuget.org/packages/Avalonia.Themes.Fluent/) doit être installé.

:::info
Pour savoir comment ajouter un package nuget, vous pouvez suivre les étapes de la page NuGet ou la documentation de [Visual Studio](https://learn.microsoft.com/en-us/nuget/quickstart/install-and-use-a-package-in-visual-studio), [Rider](https://www.jetbrains.com/help/rider/Using_NuGet.html).
:::

Après cela, le thème doit être inclus dans la classe Application :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App">
  <Application.Styles>
    // highlight-start
    <FluentTheme />
    // highlight-end
  </Application.Styles>
</Application>
```

:::note
Si vous devez spécifier une variante de thème sombre ou clair, veuillez consulter la documentation [Variants de thème](../../../../guides/styles-and-resources/how-to-use-theme-variants.md).
:::

## Changer la densité du thème

Le thème Fluent a deux ensembles de variantes de densité prédéfinies.
Pour passer à un aspect plus compact, vous pouvez le définir avec la propriété DensityStyle :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App">
  <Application.Styles>
    // highlight-start
    <FluentTheme DensityStyle="Compact" />
    // highlight-end
  </Application.Styles>
</Application>
```

## Creating custom color palettes

## Créer des palettes de couleurs personnalisées

Bien que FluentTheme dispose de ressources intégrées pour les variantes sombres et claires, il est toujours possible de remplacer la palette de base pour ces variantes.
C'est utile lorsque les développeurs souhaitent utiliser le même thème de base, mais avec des couleurs différentes.

Pour ce faire, vous devez définir :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App">
  <Application.Styles>
    <FluentTheme>
    // highlight-start
      <FluentTheme.Palettes>
        <!-- Palette de couleurs pour la variante claire (Light) du thème -->
        <ColorPaletteResources x:Key="Light" Accent="Green" RegionColor="White" ErrorText="Red" />
        <!-- Palette de couleurs pour la variante sombre (Dark) du thème -->
        <ColorPaletteResources x:Key="Dark" Accent="DarkGreen" RegionColor="Black" ErrorText="Yellow" />
      </FluentTheme.Palettes>
    // highlight-end
    </FluentTheme>
  </Application.Styles>
</Application>
```

Bien que `ColorPaletteResources` ait de nombreuses propriétés de couleur qui peuvent être remplacées indépendamment pour chaque variante, il est possible de redéfinir uniquement l'ensemble minimal de ce qui est nécessaire, et de garder tout le reste selon les valeurs par défaut. Comme dans les exemples ci-dessus, seules quelques couleurs sont remplacées.

Si Accent n'est pas remplacé, Avalonia utilise la couleur d'accent du système d'exploitation si elle est disponible.
De plus, Accent prend en charge les liaisons et peut être changé à l'exécution. Mais pas d'autres propriétés, car elles sont lues une fois après le démarrage de l'application et sont utilisées statiquement pour des raisons de performance.

Il est possible de créer des palettes depuis le code derrière, mais les mêmes règles s'appliquent - seul Accent peut être mis à jour dynamiquement, et les palettes doivent être

:::note
FluentTheme ne prend en charge que les variantes de thème sombre et clair, et il n'est pas possible de définir des palettes pour des variantes personnalisées.
:::

## Créer des palettes de couleurs personnalisées avec un éditeur en ligne


L'éditeur de thème Fluent de Microsoft a été porté sur Avalonia et est maintenant disponible pour être utilisé avec notre FluentTheme également. Il est disponible sur la page https://theme.xaml.live/ et prend en charge les fonctionnalités suivantes :

1. Édition des couleurs de la palette pour les variantes claires et sombres.
2. Prévisualisation de la palette actuelle.
3. Exportation des palettes actuelles en tant que code XAML pouvant être copié et collé dans le fichier `App.axaml`.
4. Sauvegarde des couleurs actuelles dans un fichier json et chargement depuis le système de fichiers.
5. Indications automatiques lorsque la palette a un faible contraste entre les couleurs.
6. Quelques préréglages de démarrage rapide.

Exemple de FluentTheme avec un préréglage de palette Forest disponible sur l'application web :
<p><img className="medium-image-zoom" src={FluentThemeForestScreenshot} alt="Palette de thème Fluent Forest" /></p>