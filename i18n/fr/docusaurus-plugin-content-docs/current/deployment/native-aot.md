---
id: native-aot
title: Déploiement AOT natif
---

La compilation AOT (Ahead-of-Time) native vous permet de publier vos applications Avalonia sous forme d'exécutables autonomes avec des caractéristiques de performance natives. Ce guide couvre les considérations et la configuration spécifiques à Avalonia pour le déploiement AOT natif.

## Avantages pour les applications Avalonia

La compilation AOT native offre plusieurs avantages spécifiquement pertinents pour les applications Avalonia :

- Temps de démarrage de l'application plus rapide, particulièrement bénéfique pour les applications de bureau
- Empreinte mémoire réduite pour les environnements à ressources limitées
- Déploiement autonome sans nécessiter l'installation du runtime .NET
- Sécurité améliorée grâce à une surface d'attaque réduite (pas de compilation JIT)
- Taille de distribution plus petite lorsqu'elle est combinée avec le trimming

## Configuration de l'AOT natif pour Avalonia

### 1. Configuration du projet

Ajoutez ce qui suit à votre fichier csproj :

```xml
<PropertyGroup>
    <PublishAot>true</PublishAot>
    <!-- Paramètres de trimming recommandés pour Avalonia AOT natif -->
    <BuiltInComInteropSupport>false</BuiltInComInteropSupport>
    <TrimMode>link</TrimMode>
</PropertyGroup>
```

### 2. Configuration du trimming

L'AOT natif nécessite un trimming. Ajoutez ces paramètres de trimming spécifiques à Avalonia :

```xml
<ItemGroup>
    <!-- Préserver les types Avalonia pour la réflexion -->
    <TrimmerRootAssembly Include="Avalonia.Themes.Fluent" />
    <TrimmerRootAssembly Include="Avalonia.Themes.Default" />
</ItemGroup>
```

## Considérations Spécifiques à Avalonia

### Chargement XAML
Lors de l'utilisation de Native AOT, le XAML est compilé dans l'application au moment de la construction. Assurez-vous de :
- Utiliser `x:CompileBindings="True"` dans vos fichiers XAML
- Éviter le chargement dynamique de XAML à l'exécution
- Utiliser des références de ressources statiques au lieu de ressources dynamiques lorsque cela est possible

### Actifs et Ressources
- Regroupez tous les actifs en tant que ressources intégrées
- Utilisez l'action de construction `AvaloniaResource` pour vos actifs
- Évitez le chargement dynamique d'actifs à partir de sources externes

### ViewModels et Injection de Dépendances
- Enregistrez vos ViewModels au démarrage
- Utilisez la configuration DI à la compilation
- Évitez la localisation de services basée sur la réflexion

## Publication d'Applications Avalonia Native AOT

### Windows
```bash
dotnet publish -r win-x64 -c Release
```

### Linux
```bash
dotnet publish -r linux-x64 -c Release
```

### macOS
macOS basé sur Intel 
```bash
dotnet publish -r osx-x64 -c Release
```
macOS basé sur Apple Silicon 
```bash
dotnet publish -r osx-arm64 -c Release
```

:::tip
Vous pouvez ensuite utiliser l'[outil lipo d'Apple](https://developer.apple.com/documentation/apple-silicon/building-a-universal-macos-binary) pour combiner les binaires Intel et Apple Silicon, vous permettant de livrer des binaires universels.
:::

## Dépannage des Problèmes Courants

### 1. Contrôles XAML Manquants
Si des contrôles sont manquants à l'exécution :
```xml
<ItemGroup>
    <!-- Ajoutez les contrôles Avalonia spécifiques que vous utilisez -->
    <TrimmerRootAssembly Include="Avalonia.Controls" />
</ItemGroup>
```

### 2. Erreurs Liées à la Réflexion
Pour les ViewModels ou les services utilisant la réflexion :
```xml
<ItemGroup>
    <TrimmerRootDescriptor Include="TrimmerRoots.xml" />
</ItemGroup>
```

Créez un `TrimmerRoots.xml` :
```xml
<linker>
    <assembly fullname="YourApplication">
        <type fullname="YourApplication.ViewModels*" preserve="all"/>
    </assembly>
</linker>
```

## Limitations Connues

Lors de l'utilisation de Native AOT avec Avalonia, soyez conscient de ces limitations :
- La création dynamique de contrôles doit être configurée dans les paramètres du trimmer
- Certains contrôles Avalonia tiers peuvent ne pas être compatibles avec AOT
- Les fonctionnalités spécifiques à la plateforme nécessitent une configuration explicite
- L'aperçu en direct dans les outils de conception peut être limité

## Support des Plateformes

| Plateforme | Statut |
|------------|--------|
| Windows x64 | ✅ Pris en charge |
| Windows Arm64 | ✅ Pris en charge |
| Linux x64 | ✅ Pris en charge | |
| Linux Arm64 | ✅ Pris en charge |
| macOS x64 | ✅ Pris en charge | |
| macOS Arm64 | ✅ Pris en charge |
| Navigateur | ❌ Non pris en charge |

## Meilleures Pratiques

1. **Structure de l'Application**
- Utilisez le modèle MVVM de manière cohérente
- Minimisez l'utilisation de la réflexion
- Préférez la configuration à la compilation

2. **Gestion des Ressources**
- Utilisez des ressources statiques lorsque cela est possible
- Regroupez tous les actifs nécessaires
- Implémentez un nettoyage approprié dans IDisposable

3. **Optimisation des Performances**
- Activez la compilation de liaison
- Utilisez des liaisons compilées
- Implémentez une virtualisation appropriée pour les grandes collections

## Ressources Supplémentaires
- [Applications d'exemple Avalonia avec Native AOT](https://github.com/AvaloniaUI/Avalonia.Samples)