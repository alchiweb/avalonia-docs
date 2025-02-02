---
id: solution-setup
title: Configuration d'une solution multiplateforme
---

Malgré la diversité des plateformes, les projets Avalonia utilisent tous le même format de fichier de solution (le format de fichier ".SLN" de Visual Studio). Les solutions peuvent être partagées entre les environnements de développement, offrant une approche unifiée pour le développement d'applications multiplateformes.

La première étape pour créer une nouvelle application multiplateforme est de créer une solution. Cette section expliquera ce qui se passe ensuite : le processus de configuration des projets pour construire des applications multiplateformes avec Avalonia.

## Peuplement de la Solution

Le modèle `Application multiplateforme Avalonia` crée une structure de solution qui inclut les projets suivants pour permettre un partage et une réutilisation sans faille du code sur plusieurs plateformes :

:::info
[Assurez-vous d'avoir installé les modèles Avalonia.](../../get-started/install#install-avalonia-ui-templates)
:::

### Projet Principal
Ceci constitue le cœur de votre application et est conçu pour être indépendant de la plateforme. Il contient tous les composants réutilisables de votre application, y compris la logique métier, les modèles de vue et les vues. Tous les autres projets font référence à ce projet principal. La majorité de vos efforts de développement devraient se concentrer ici.

### Projet de Bureau
Ce projet permet à l'application de fonctionner sur les plateformes Windows, macOS et Linux, avec un type de sortie 'WinExe'.

### Projet Android
C'est un projet basé sur `NET-Android` qui fait référence au Projet Principal. Il dispose d'une MainActivity qui hérite de `AvaloniaMainActivity`, agissant comme point d'entrée pour l'application Android.

### Projet iOS
C'est un projet `NET-iOS` adapté aux plateformes iOS et iPadOS. Le point d'entrée pour ce projet est l'`AppDelegate`, qui hérite de `AvaloniaAppDelegate`.

### Projet Navigateur
Ce projet WebAssembly (WASM) permet à votre application Avalonia de fonctionner dans un navigateur web. Son RuntimeIdentifier est `'browser-wasm'`.

## Projet Principal

Les projets de code partagé ne doivent faire référence qu'à des assemblies qui sont universellement disponibles sur toutes les plateformes. Cela inclut généralement des espaces de noms de framework communs comme `System`, `System.Core` et `System.Xml`.

Ces projets partagés visent à implémenter autant de fonctionnalités d'application que possible, y compris les composants d'interface utilisateur, maximisant ainsi la réutilisabilité du code.

En séparant les fonctionnalités en couches distinctes, le code devient plus facile à gérer, à tester et à réutiliser sur plusieurs plateformes. Cette approche d'architecture en couches dans les projets Avalonia UI favorise l'efficacité et l'évolutivité dans le développement d'applications.

## Projets d'application spécifiques à la plateforme

Les projets spécifiques à la plateforme doivent faire référence au projet de base. Les projets spécifiques à la plateforme existent pour permettre à l'application de fonctionner sur des plateformes uniques, y compris iOS, Android et WASM.

Alors que les plateformes de bureau peuvent partager un seul projet, il peut être bénéfique de créer un projet séparé pour macOS en utilisant le [Xamarin.Mac Target Framework](https://learn.microsoft.com/en-us/xamarin/mac/platform/target-framework). Cela facilitera la distribution et l'emballage de votre application.
