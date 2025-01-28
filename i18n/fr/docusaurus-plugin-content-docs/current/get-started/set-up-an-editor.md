---
id: set-up-an-editor
title: Configurer un éditeur de code (IDE)
---

import AvaloniaVsExtensionMarketplaceScreenshot from '/img/get-started/avalonia-vs-extension-marketplace.png';
import AvaloniaVsExtensionNuGetScreenshot from '/img/get-started/avalonia-vs-extension-nuget.png';

# Configurer un éditeur de code (IDE)

Vous pouvez créer une application Avalonia en utilisant n'importe quel éditeur de code, mais l'utilisation d'un IDE vous donnera un support pour la création de fichiers XAML Avalonia avec un prévisualiseur et l'autocomplétion de code.

## IDE Recommandé : JetBrains Rider

L'IDE [JetBrains Rider](https://www.jetbrains.com/rider/) dispose d'un support intégré pour Avalonia XAML depuis 2020.3, y compris un support de premier ordre pour les fonctionnalités XAML spécifiques à Avalonia et les inspections de code personnalisées. Maintenant que Rider est gratuit pour un usage individuel, nous le recommandons vivement comme IDE principal pour le développement Avalonia, en particulier pour les développeurs sur macOS et Linux.

Rider offre l'expérience de développement la plus complète et la plus soignée pour Avalonia, avec des fonctionnalités intégrées telles que :

* Complétion et navigation XAML avancées
* Analyse de code riche et corrections rapides
* Outils de débogage complets
* Profilage de performance intégré

### Plugin AvaloniaRider
Bien que Rider inclue un support natif pour Avalonia XAML dès le départ, nous recommandons d'installer le plugin tiers [AvaloniaRider](https://plugins.jetbrains.com/plugin/14839-avaloniarider) pour activer la fonctionnalité de prévisualisation XAML en direct. Ce plugin fournit un aperçu en direct de vos modifications XAML pendant que vous tapez, similaire à la fonctionnalité de prévisualisation disponible dans Visual Studio et Visual Studio Code.

Notez que le plugin est optionnel - vous pouvez développer des applications Avalonia dans Rider sans lui, mais la capacité de prévisualisation en direct rend le développement XAML plus efficace.

## Visual Studio

Si vous développez avec Avalonia dans Visual Studio, vous devez installer l'extension [Avalonia pour Visual Studio](https://marketplace.visualstudio.com/items?itemName=AvaloniaTeam.AvaloniaVS).

<img className="center" src={AvaloniaVsExtensionMarketplaceScreenshot} alt="" />

L'extension fournit un support IntelliSense pour le XAML d'Avalonia ainsi qu'un visualiseur.

Pour installer l'extension Avalonia pour Visual Studio :

- Dans Visual Studio, cliquez sur **Gérer les extensions** dans le menu **Extensions**
- Dans la boîte de **Recherche**, tapez "Avalonia"
- Cliquez sur **Télécharger** et suivez les instructions (vous devrez fermer Visual Studio pour terminer l'installation)

<img className="center" src={AvaloniaVsExtensionNuGetScreenshot} alt="" />

:::info
Alternativement, vous pouvez télécharger l'extension [ici](https://marketplace.visualstudio.com/items?itemName=AvaloniaTeam.AvaloniaVS).
:::

:::info
Si vous utilisez VS2019 ou VS2017, vous devrez télécharger l'extension pour les versions antérieures [ici](https://marketplace.visualstudio.com/items?itemName=AvaloniaTeam.AvaloniaforVisualStudio).
:::

## Visual Studio Code 
[L'extension Avalonia pour Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=AvaloniaTeam.vscode-avalonia) contient un support de base pour l'autocomplétion XAML d'Avalonia et le prévisualiseur. Bien que fonctionnelle, l'expérience de développement n'est pas aussi riche que celle que vous trouverez dans Rider ou Visual Studio. Pour les développeurs sur macOS et Linux nécessitant une expérience IDE complète, nous recommandons d'utiliser JetBrains Rider à la place.

Si vous préférez toujours utiliser VS Code, vous pouvez installer l'extension depuis le [marché Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=AvaloniaTeam.vscode-avalonia).

## Comparaison des éditeurs

Pour la meilleure expérience de développement Avalonia :

* **Windows** : Utilisez soit JetBrains Rider soit Visual Studio
* **macOS/Linux** : Utilisez JetBrains Rider
* **Éditeur léger** : Visual Studio Code peut être utilisé mais offre un ensemble de fonctionnalités plus limité


