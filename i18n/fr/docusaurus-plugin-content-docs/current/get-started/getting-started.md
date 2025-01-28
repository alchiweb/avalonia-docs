---
id: getting-started
title: Commencez avec la ligne de commande (CLI)
---

Si vous construisez vos projets avec le .NET CLI, suivez les procédures ici pour installer les modèles _Avalonia UI_ et créer votre première application.

## Installer les modèles Avalonia UI

Pour installer les modèles _Avalonia UI_, exécutez la commande suivante :

```bash
dotnet new install Avalonia.Templates
```

:::info
Remarque : Pour .NET 6.0 et les versions antérieures, vous devez utiliser `--install` à la place.
:::

## Créer une nouvelle application

Une fois les modèles installés, vous pouvez créer une nouvelle application _Avalonia UI_ en exécutant la commande suivante :

```bash
dotnet new avalonia.app -o MyApp
```

Cela créera un nouveau dossier appelé `MyApp` contenant vos fichiers d'application. Pour exécuter l'application, naviguez jusqu'au dossier `MyApp` et exécutez :

```bash
cd MyApp
dotnet run
```

C'est tout ! Votre application _Avalonia UI_ est maintenant opérationnelle. Ensuite, vous pouvez ouvrir le dossier `MyApp` pour commencer à améliorer et développer davantage votre application.
