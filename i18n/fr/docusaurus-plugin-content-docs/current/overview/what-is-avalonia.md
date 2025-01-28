---
id: what-is-avalonia
title: Avalonia, c'est quoi ?
---

import AvaloniaArchitecture from '/img/overview/Architecture.png';
import MauiComparision from '/img/overview/MAUI-Comparision.png';

Avalonia est un framework UI open-source et multiplateforme qui permet aux développeurs de créer des applications en utilisant .NET pour Windows, macOS, Linux, iOS, Android et WebAssembly.

Il utilise son propre moteur de rendu pour dessiner des contrôles UI, garantissant une apparence et un comportement cohérents sur toutes les plateformes prises en charge. Cela signifie que les développeurs peuvent partager leur code UI et maintenir une apparence uniforme, quel que soit la plateforme cible.

<p><img className="image-zoom-medium" src={AvaloniaArchitecture} alt="" /></p>

## Pour qui est Avalonia ?

Avalonia est destiné aux développeurs qui souhaitent :

* Écrire des applications multiplateformes en XAML et C#, à partir d'une base de code partagée unique.
* Partager l'UI, la mise en page et le design sur plusieurs plateformes.
* Partager le code, les tests et la logique métier entre les plateformes.

## Comment fonctionne Avalonia ?
Avalonia unifie les plateformes de bureau, mobiles et web grâce à une approche unique qui diffère des frameworks multiplateformes traditionnels. Au lieu d'encapsuler des contrôles UI natifs, Avalonia implémente son propre moteur de rendu multiplateforme qui garantit une cohérence pixel-perfect sur toutes les plateformes prises en charge.

### Vue d'ensemble de l'architecture
Avalonia est construit sur .NET Standard 2.0, ce qui lui permet de fonctionner sur n'importe quelle plateforme qui prend en charge .NET. Le framework se compose de plusieurs couches clés :

#### Couche de base indépendante de la plateforme
La majorité des fonctionnalités d'Avalonia résident dans une couche de base indépendante de la plateforme qui gère :

* Contrôles UI et mise en page
* Gestion de l'arbre visuel
* Système de style
* Liaison de données
* Gestion des entrées
* Cadre d'animation

Cette couche de base est complètement indépendante de la plateforme, ce qui signifie qu'elle se comporte de manière identique, quel que soit le système d'exploitation ou l'appareil.

#### Moteur de rendu
Contrairement aux frameworks qui s'appuient sur des contrôles UI natifs, Avalonia utilise son propre moteur de rendu alimenté par Skia ou Direct2D. Cette approche signifie que :

* Les applications se ressemblent et se comportent de manière identique sur toutes les plateformes
* Les contrôles personnalisés et les effets visuels peuvent être implémentés une fois et fonctionner partout
* Le framework n'est pas limité par les capacités UI spécifiques à la plateforme

#### Couche d'intégration de la plateforme
Avalonia nécessite un code spécifique à la plateforme minimal pour s'intégrer à chaque plateforme prise en charge. Cette couche gère :

* Gestion des fenêtres
* Événements d'entrée
* Opérations de presse-papiers
* Dialogues natifs
* Accélération matérielle
* Fonctionnalités spécifiques à la plateforme

#### Environnement d'exécution
Les applications Avalonia s'exécutent sur le runtime .NET, que ce soit .NET Core ou Mono.

#### Comparaison avec les approches natives
Alors que des frameworks comme .NET MAUI abstraient les contrôles UI natifs, Avalonia adopte une approche différente :

<p><img className="image-zoom-medium" src={MauiComparision} alt="" /></p>

Cette différence architecturale offre plusieurs avantages :

* Comportement cohérent sur toutes les plateformes
* Rendu pixel-perfect
* Contrôle total sur la pile UI
* Support de plateforme simplifié
* Réduction des frais de maintenance
* Meilleure performance sur les appareils à ressources limitées

### Intégration avec les plateformes natives

Bien qu'Avalonia utilise son propre moteur de rendu, il s'intègre néanmoins parfaitement aux capacités des plateformes natives :

* **Windows** : Prend en charge les API Win32 et les fonctionnalités modernes de Windows
* **Linux** : Fonctionne avec X11, Wayland et le rendu framebuffer
* **macOS** : S'intègre avec Cocoa et les services de la plateforme
* **Mobile** : Fournit une gestion native du cycle de vie et une intégration de la plateforme
* **Web** : Fonctionne via WebAssembly avec une intégration complète dans le navigateur

### Exigences de support des plateformes
Au cœur d'Avalonia, il ne faut que deux capacités fondamentales pour prendre en charge une nouvelle plateforme :

1. La capacité de dessiner des pixels sur un écran
2. La capacité de recevoir des événements d'entrée

Cet ensemble d'exigences minimales est ce qui permet à Avalonia de prendre en charge une si large gamme de plateformes, des systèmes d'exploitation de bureau aux dispositifs embarqués, et même des plateformes inhabituelles comme les serveurs VNC.

Cette architecture permet à Avalonia de tenir sa promesse de "Une base de code, des possibilités infinies" tout en maintenant une haute performance et une intégration native des plateformes là où cela compte le plus.