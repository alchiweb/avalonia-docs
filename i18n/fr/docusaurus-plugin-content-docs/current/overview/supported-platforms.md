---
id: supported-platforms
title: Plateformes prises en charge
---

Les applications Avalonia peuvent être écrites pour les plateformes suivantes :

| Plateforme    | Prise en charge |
|----------------|-----------------|
| `Windows`      | ✔️              |
| `macOS`        | ✔️              |
| `Linux`        | ✔️              |
| `iOS`          | ✔️              |
| `Android`      | ✔️              |
| `WebAssembly`  | ✔️              |

## Windows

* Windows 8.1
* Windows 10
* Windows 11

Bien que les applications Avalonia fonctionnent avec succès sur Windows 7, cette plateforme héritée reçoit un support limité. Nous ne fournissons plus de corrections de bogues pour les problèmes spécifiques à Windows 7.

## macOS

* macOS 10.14 (Mojave)
* macOS 10.15 (Catalina)
* macOS 11 (Big Sur)
* macOS 12 (Monterey)
* macOS 13 (Ventura)
* macOS 14 (Sonoma)
* macOS 15 (Sequoia)

Avalonia fonctionne également sur macOS 10.13 (High Sierra), mais nous sommes en train de migrer vers l'API GPU Metal, qui est actuellement désactivée par défaut. Il est prévu de l'activer lors de l'une des mises à jour mineures.

:::important
Il est possible de développer pour macOS sur Windows, macOS et Linux en utilisant Avalonia. Si vous prévoyez de signer et de notariser votre application macOS pour distribution, vous aurez besoin d'un Mac avec XCode installé.  
:::

## Linux

* Debian 9+
* Ubuntu 16.04+
* Fedora 30+

Avalonia fonctionne de manière fiable sur la plupart des distributions Linux tant qu'elles prennent en charge le SDK .NET et disposent de capacités X11 ou framebuffer. Bien que nous supportions officiellement Debian 9+, Ubuntu 16.04+ et Fedora 30+, de nombreuses autres distributions exécutent des applications Avalonia sans problème, et nous travaillons activement pour garantir une large compatibilité avec Linux.

Pour les clients ayant des [accords de support](https://avaloniaui.net/support), nous offrons une couverture étendue des distributions Linux et pouvons aider avec des exigences spécifiques à certaines distributions. Le support Wayland est actuellement en aperçu privé et sera disponible dans une prochaine version.

Les distributions WSL 2 sont également prises en charge, mais les dépendances `libice6`, `libsm6` et `libfontconfig1` doivent être installées individuellement.

:::info
Skia est construit avec glibc 2.17. Si votre distribution utilise autre chose à la place, vous devez construire votre propre libSkiaSharp.so avec [SkiaSharp](https://github.com/mono/SkiaSharp). Vous pouvez également visiter la page d'accueil de SkiaSharp pour plus d'informations sur les versions prises en charge.
:::

## iOS 

* iOS 13
* iOS 14
* iOS 15
* iOS 16
* iOS 17
* iOS 18

:::note
.NET 7 est requis pour le support iOS.
:::

## Android 

| Nom                 | Numéro de version | Niveau API |
|---------------------|-------------------|------------|
| Android Lollipop    | 5.0               | 21         |
| Android Lollipop    | 5.1               | 22         |
| Android Marshmallow | 6.0               | 23         |
| Android Nougat      | 7.0               | 24         |
| Android Nougat      | 7.1               | 25         |
| Android Oreo        | 8.0               | 26         |
| Android Oreo        | 8.1               | 27         |
| Android Pie         | 9                 | 28         |
| Android 10          | 10                | 29         |
| Android 11          | 11                | 30         |
| Android 12          | 12                | 31         |
| Android 12L         | 12.1              | 32         |
| Android 13          | 13                | 33         |
| Android 14          | 14                | 34         |
| Android 15          | 15                | 35         |
| Android 16          | 16                | 36         |

:::note
.NET 7 est requis pour le support Android.
:::

## WebAssembly (Navigateur)
Tout navigateur avec un support complet de WebAssembly devrait techniquement fonctionner - https://caniuse.com/wasm.

Pour les meilleures performances et le meilleur support, nous recommandons les dernières versions de Chrome ou Safari.

:::note
.NET 7 est requis pour le support du navigateur. À partir de la version 11.0.6, nous recommandons .NET 8.
:::

## Support supplémentaire des plateformes
Avalonia prend également en charge Tizen et tvOS, bien que cela soit fourni par la communauté.