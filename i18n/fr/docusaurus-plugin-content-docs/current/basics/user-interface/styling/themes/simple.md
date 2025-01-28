---
id: simple
title: Thème Simple
---

## Introduction

Le thème Simple d'Avalonia est spécifiquement conçu pour être minimal et léger, avec un style intégré limité. Il fournit une base simple et propre pour créer des styles personnalisés. Sa faible complexité visuelle et structurelle en fait un choix parfait pour les applications fonctionnant sur des dispositifs embarqués.

![Thème Simple](/img/basics/user-interface/styling/simple-theme.png)

## Comment utiliser

Comme première étape, le package nuget [Avalonia.Themes.Simple](https://www.nuget.org/packages/Avalonia.Themes.Simple/) doit être installé.

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
    <SimpleTheme />
    // highlight-end
  </Application.Styles>
</Application>

```

:::note
Si vous devez spécifier la variante sombre ou claire du thème, veuillez consulter la documentation sur [Les Variantes de Thème](../../../../guides/styles-and-resources/how-to-use-theme-variants.md).
:::