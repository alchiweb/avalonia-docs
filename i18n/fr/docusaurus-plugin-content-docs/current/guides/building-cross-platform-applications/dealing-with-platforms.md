---
id: dealing-with-platforms
title: Gérer les différences entre les plateformes
---

## Gestion des différences et des capacités des plateformes

Les différences entre les plateformes ne sont pas seulement un problème dans le développement multiplateforme ; même les appareils au sein de la même plateforme peuvent posséder des capacités diverses.

Cela inclut notamment des différences de taille d'écran, mais de nombreuses autres caractéristiques des appareils peuvent également varier, nécessitant que l'application vérifie certaines capacités et adapte son comportement en fonction de leur présence (ou absence). Cela est particulièrement important lors de la conception pour des situations de paradigme croisé, les systèmes d'exploitation de bureau et mobiles offrant des modèles d'interaction très différents.

Par conséquent, toutes les applications doivent être équipées pour gérer une réduction fonctionnelle en douceur, ou risquer de présenter un ensemble de fonctionnalités minimal qui ne tire pas parti du plein potentiel de la plateforme sous-jacente.

### Exemples de divergence entre les plateformes

Il existe certaines caractéristiques fondamentales inhérentes aux applications qui sont universellement applicables. Ce sont des concepts de haut niveau qui s'appliquent à tous les appareils et plateformes et peuvent donc former le noyau de la conception de votre application :

* Un écran, qui peut afficher l'interface utilisateur de votre application.
* Une forme de dispositifs d'entrée, généralement tactile pour mobile et souris et clavier pour bureau.
* Affichage des vues de données.
* Édition des données.
* Capacités de navigation.

### Fonctionnalités spécifiques à la plateforme

Au-delà des caractéristiques universelles des applications, vous devrez également prendre en compte les différences clés entre les plateformes dans votre conception. Vous devrez peut-être envisager, et éventuellement écrire ou ajuster du code spécifiquement pour gérer ces différences :

* **Tailles d'écran** : Alors que certaines plateformes (comme iOS) ont des tailles d'écran standardisées qui sont relativement faciles à cibler, d'autres, comme le bureau et WebAssembly, permettent une variété illimitée de dimensions d'écran, ce qui nécessiterait plus d'efforts pour être pris en charge dans votre application.

* **Métaphores de navigation** : Celles-ci peuvent varier selon les plateformes (par exemple, bouton 'retour' matériel) et même au sein des plateformes (par exemple, différences entre Android 2 et 4, iPhone vs iPad).

* **Claviers** : Certains appareils peuvent être équipés de claviers physiques, tandis que d'autres ne disposent que d'un clavier logiciel. Le code qui détecte quand un clavier virtuel obscurcit une partie de l'écran doit être sensible à ces différences.

Ces différences spécifiques aux plateformes doivent être soigneusement prises en compte lors de la conception de votre application Avalonia afin d'assurer une expérience utilisateur fluide sur toutes les plateformes. Bien que vous deviez vous efforcer de maximiser la réutilisation de votre code, vous devez également éviter d'essayer de réutiliser 100 % de votre code sur toutes les plateformes prises en charge. Au lieu de cela, adaptez chaque interface utilisateur de la plateforme pour qu'elle soit en harmonie avec le dispositif.

### Gestion de la divergence des plateformes

Le support de plusieurs plateformes à partir de la même base de code peut être réalisé en abstraisant les fonctionnalités de la plateforme ou en utilisant du [code conditionnel](../../guides/platforms/platform-specific-code/dotnet.md).

* **Abstraction de la plateforme** : Cette approche s'appuie sur le modèle de façade métier pour fournir un accès uniforme à travers les plateformes. Elle abstrait les implémentations uniques de chaque plateforme en une API unique et cohérente. L'avantage principal est la possibilité d'écrire du code indépendant de la plateforme, améliorant ainsi la réutilisabilité et la maintenabilité du code. Cependant, cette approche peut ne pas exploiter pleinement les caractéristiques et capacités uniques de chaque plateforme.

## Abstraction de la plateforme

Dans Avalonia, vous pouvez utiliser des abstractions de classe pour simplifier votre processus de développement sur différentes plateformes. Cela peut être réalisé en utilisant des interfaces ou des classes de base définies dans le code partagé, puis mises en œuvre ou étendues dans des projets spécifiques à la plateforme.

### Interfaces

L'utilisation d'interfaces vous permet de créer des classes spécifiques à la plateforme qui peuvent être intégrées dans vos bibliothèques partagées pour la réutilisation du code.

#### Comment ça fonctionne
L'interface est définie dans le code partagé et passée dans la bibliothèque partagée en tant que paramètre ou propriété. Les applications spécifiques à la plateforme peuvent alors implémenter l'interface, permettant au code partagé de la traiter efficacement.

#### Avantages
L'avantage principal de cette approche est que l'implémentation peut contenir du code spécifique à la plateforme et même référencer des bibliothèques externes spécifiques à la plateforme, offrant ainsi une grande flexibilité.

#### Inconvénients
Un inconvénient potentiel est la nécessité de créer et de passer des implémentations dans le code partagé. Si l'interface est utilisée profondément dans le code partagé, elle peut devoir être transmise à travers plusieurs paramètres de méthode, ce qui pourrait conduire à une chaîne d'appels plus complexe. Si le code partagé utilise de nombreuses interfaces différentes, elles doivent toutes être créées et définies dans le code partagé.

### Héritage
Votre code partagé peut implémenter des classes abstraites ou virtuelles qui pourraient être étendues dans un ou plusieurs projets spécifiques à la plateforme. Cette technique ressemble à l'utilisation d'interfaces mais fournit déjà certains comportements implémentés.

#### Comment ça fonctionne
En utilisant l'héritage, vous pouvez créer des classes de base dans votre code partagé qui peuvent être étendues de manière optionnelle dans vos projets spécifiques à la plateforme. Cependant, comme C# n'autorise que l'héritage simple, cette approche peut influencer la conception future de votre API. Par conséquent, utilisez l'héritage avec prudence.

#### Avantages et Inconvénients
Les avantages et les inconvénients de l'utilisation des interfaces s'appliquent également à l'héritage. Cependant, un avantage supplémentaire de l'héritage est que la classe de base peut contenir un certain code d'implémentation. Cela pourrait potentiellement fournir une implémentation entièrement indépendante de la plateforme qui peut être étendue au besoin.

## Utilisation de Maui.Essentials

Une autre approche consisterait à utiliser une bibliothèque qui abstrait certaines fonctionnalités sous une API commune de niveau supérieur. [Maui.Essentials](https://learn.microsoft.com/en-us/dotnet/maui/platform-integration/?view=net-maui-8.0) est l'une de ces bibliothèques, qui peut être utilisée avec Avalonia sur .NET 8 ou supérieur via le package nuget [Microsoft.Maui.Essentials](https://www.nuget.org/packages/Microsoft.Maui.Essentials). Alternativement, vous pouvez utiliser l'ensemble complet des packages MAUI avec le package hybride [Avalonia.Maui](https://github.com/AvaloniaUI/AvaloniaMauiHybrid). Ce package offre une intégration plus approfondie avec les packages MAUI.

:::note
Bien que `Maui.Essentials` soit une excellente bibliothèque qui abstrait les API de plateforme, MAUI lui-même a un ensemble limité de plateformes prises en charge. Il ne fournit pas d'API pour les plateformes Linux, Navigateur et macOS (non macCatalyst).
:::