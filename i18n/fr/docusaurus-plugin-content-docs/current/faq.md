---
id: faq
title: FAQ
---

## Qu'est-ce qu'Avalonia ?
Avalonia est un framework UI open-source et multiplateforme. C'est le framework UI multiplateforme le plus populaire pour les développeurs .NET. Il est conçu pour créer des interfaces utilisateur flexibles et belles. Avalonia prend en charge un large éventail de plateformes de développement d'applications, y compris Windows, Linux, macOS, iOS, Android et WebAssembly.

Construit sur une pile .NET moderne, Avalonia permet aux développeurs d'écrire en C# ou dans tout autre langage .NET, et de définir des interfaces utilisateur en utilisant le langage de balisage XAML. Semblable à WPF, Avalonia utilise un système de style basé sur XAML, et son système de mise en page et son infrastructure de liaison offrent un environnement familier pour les développeurs expérimentés avec les frameworks basés sur XAML.

Avalonia se distingue de nombreux autres frameworks UI car il ne s'appuie pas sur des contrôles fournis par le système d'exploitation. Au lieu de cela, il dessine toute l'interface utilisateur lui-même, ce qui permet un haut niveau de personnalisation et une expérience cohérente sur toutes les plateformes.

---

## En quoi Avalonia est-il différent des autres frameworks UI comme WPF ou Xamarin.Forms ?
Avalonia se distingue des autres frameworks UI tels que WPF et Xamarin.Forms par plusieurs aspects clés :

* **Multiplateforme par conception** : Contrairement à WPF, qui est exclusif à Windows, Avalonia est conçu dès le départ pour être multiplateforme. Il prend en charge Windows, Linux, macOS, iOS, Android, WebAssembly et plus encore. Il est capable de fournir un look et une sensation cohérents sur toutes ces plateformes.

* **Rendu indépendant** : Alors que Xamarin.Forms s'appuie sur les contrôles natifs de la plateforme cible pour le rendu, Avalonia dispose de son propre moteur de rendu. Cela signifie qu'il n'utilise pas les contrôles UI natifs du système d'exploitation mais dessine plutôt l'ensemble de l'interface utilisateur lui-même. Cela offre un haut degré de flexibilité et de personnalisation.

* **Style flexible** : Avalonia utilise un puissant système de style similaire à WPF. Il utilise des styles pour définir l'apparence de vos contrôles, et contrairement à Xamarin.Forms, ces styles peuvent être ajustés dynamiquement en fonction de l'état du contrôle et hérités de manière hiérarchique.

* **XAML et code-behind** : Comme WPF et Xamarin.Forms, Avalonia vous permet de définir des interfaces utilisateur en utilisant XAML, un langage de balisage que de nombreux développeurs .NET connaissent bien. Vous pouvez également manipuler votre interface utilisateur directement dans le code, vous donnant la flexibilité de choisir l'approche appropriée pour votre application.

* **Open source et dirigé par la communauté** : Avalonia est un projet open-source avec une communauté active contribuant à son développement. Cela signifie qu'il évolue et s'améliore continuellement en fonction des retours et des besoins de la communauté.

---

## Quelles versions de .NET puis-je utiliser ?

* .NET Framework 4.6.2+
* .NET Core 2.0+
* .NET 5+ (y compris le dernier .NET 8)

---

## Puis-je utiliser mes connaissances existantes de WPF ou UWP pour travailler avec Avalonia ?
Oui, vous le pouvez certainement ! Avalonia est fortement influencé par WPF et UWP, et il s'appuie sur de nombreux concepts similaires, tels que XAML pour la définition de l'interface utilisateur, la liaison de données et le modèle de conception MVVM (Model-View-ViewModel). Donc, si vous êtes déjà familiarisé avec ces technologies, vous constaterez probablement que la courbe d'apprentissage d'Avalonia est assez douce.

Cependant, il est important de noter qu' bien qu'Avalonia partage de nombreuses similitudes avec WPF et UWP, ce n'est pas un clone direct. Avalonia a été conçu pour être multiplateforme dès le départ, et en tant que tel, il possède ses propres caractéristiques et capacités uniques qui le distinguent de WPF et UWP. Ces différences peuvent inclure les contrôles disponibles, le mécanisme de stylisation, les intégrations spécifiques à la plateforme, etc.

Néanmoins, vos connaissances existantes de WPF ou UWP vous donneront certainement un bon point de départ pour apprendre et travailler avec Avalonia.

---

## Avalonia est-elle adaptée à la création d'applications de bureau complexes ?
Oui, Avalonia est en effet adaptée à la création d'applications de bureau complexes. Elle est conçue pour permettre le développement d'interfaces utilisateur flexibles et complexes sur une large gamme de plateformes, y compris Windows, macOS, Linux, iOS, Android et WebAssembly.

Le puissant système de stylisation d'Avalonia, inspiré de WPF et CSS, vous permet de créer des interfaces utilisateur belles et uniques. De plus, l'utilisation de la liaison de données et de l'architecture MVVM soutient la création d'applications évolutives avec un code bien structuré, testable et maintenable.

En plus de cela, Avalonia prend en charge une multitude d'autres fonctionnalités importantes pour les applications de bureau complexes, telles que le support multi-fenêtres, les couches contextuelles, les modèles de contrôle, les contrôles utilisateur, et plus encore.

Que vous développiez un utilitaire simple ou une application d'entreprise à grande échelle, Avalonia offre les outils et la flexibilité nécessaires pour créer des applications robustes, performantes et époustouflantes.

---

## Puis-je coder mon interface utilisateur au lieu d'utiliser XAML ?

Oui. Vous pouvez coder toute votre interface utilisateur avec le langage .NET de votre choix.

---

## Existe-t-il un concepteur visuel avec glisser-déposer ?

Nous travaillons à offrir une expérience complète de concepteur avec glisser-déposer à Avalonia en 2025. Ce concepteur fera partie de [Avalonia Accelerate](https://github.com/AvaloniaUI/Avalonia/discussions/16997), une offre payante conçue pour améliorer la productivité des développeurs dans l'écosystème Avalonia.

Avalonia Accelerate aidera les développeurs à créer des applications plus efficacement grâce à des outils de conception visuelle, complétant l'approche XAML-first existante pour laquelle Avalonia est connue.

---

## Avalonia prend-elle en charge le Hot Reload ?

Vous pouvez utiliser un [projet communautaire](https://github.com/AvaloniaCommunity/Live.Avalonia) pour activer le hot reload avec Avalonia.

---

## Avalonia peut-elle interagir avec des API natives ?

Oui. Consultez notre [guide sur l'utilisation des fonctionnalités spécifiques à la plateforme](guides/building-cross-platform-applications/dealing-with-platforms#platform-abstraction).

---

## Puis-je compiler pour différentes plateformes ?

Oui. Vous pouvez compiler pour macOS, Linux, Android et WebAssembly depuis Windows. Vous devrez probablement empaqueter votre application sur ces plateformes pour créer des packages de publication de votre application.

Vous aurez besoin d'un Mac pour construire des applications iOS.

---

## Puis-je créer une application mobile avec Avalonia ?

Oui. Vous pouvez développer pour Android et iOS aujourd'hui. Vous devez cependant prêter une attention particulière à chaque plateforme et vous assurer que votre application fonctionne bien sur des appareils plus petits, axés sur le tactile.

---

## Comment puis-je m'impliquer ?

Consultez le [guide de la communauté](community.md) pour voir comment vous pouvez vous impliquer dans le projet.

---

## Quelles plateformes sont prises en charge ?

:::warning
Vous devez également savoir quelles plateformes sont prises en charge par votre version de .NET. Souvent, .NET abandonne le support des anciennes versions de systèmes d'exploitation, tandis qu'Avalonia peut encore fonctionner avec elles. Vous devrez peut-être attendre avant de mettre à jour le SDK. Par exemple, [voici la liste](https://github.com/dotnet/core/blob/main/release-notes/8.0/supported-os.md) des versions de systèmes d'exploitation prises en charge par .NET 8.
:::

### Distros Linux

* Debian 9+
* Ubuntu 16.04+
* Fedora 30+

D'autres distributions peuvent également fonctionner. La principale limitation est le support du SDK .NET et la disponibilité du système X11. En alternative, le backend framebuffer linux est également pris en charge. La version avec support Wayland est en prévisualisation et n'est pas encore publiée.

Les distributions WSL 2 sont également prises en charge, mais les dépendances `libice6`, `libsm6` et `libfontconfig1` doivent être installées individuellement.

:::info
Skia est construit contre glibc 2.17. Si votre distribution utilise autre chose, vous devez construire votre propre libSkiaSharp.so sur [SkiaSharp](https://github.com/mono/SkiaSharp). Vous pouvez également visiter la page d'accueil de SkiaSharp pour plus d'informations sur les versions prises en charge.
:::

### Quelles versions de Windows sont prises en charge ?

* Windows 8.1+

Avalonia fonctionne également sur Windows 7, mais les nouvelles fonctionnalités spécifiques aux plateformes ne seront pas disponibles et nous ne fournissons plus de correctifs pour cette version.

### Quelles versions de macOS sont prises en charge ?

* macOS 10.14+

Avalonia fonctionne également sur macOS 10.13, mais nous sommes en train de migrer vers l'API GPU Metal, qui est actuellement désactivée par défaut. Il est prévu de l'activer lors de l'une des mises à jour mineures.

### Quelles versions d'Android sont prises en charge ?

* Android 5.0+ (API 21)

:::note
.NET 7 est requis pour le support Android.
:::

### Quelles versions d'iOS sont prises en charge ?

* iOS 13.0+

:::note
.NET 7 est requis pour le support iOS.
:::

### Quelles versions de navigateur sont prises en charge ?

Tout navigateur avec un support complet de WebAssembly devrait techniquement fonctionner - https://caniuse.com/wasm.

Pour les meilleures performances et un meilleur support, nous recommandons les dernières versions de Chrome ou Safari.

:::note
.NET 7 est requis pour le support des navigateurs.
À partir de la version 11.0.6, nous recommandons .NET 8.
:::

## Crédits

* Des portions de cette documentation ont été adaptées de la [documentation Dotnet](https://github.com/dotnet/docs/) sous licence [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)