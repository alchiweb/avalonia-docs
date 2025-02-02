---
id: index
title: Construire des applications multiplateformes
---

Ce guide présente Avalonia et décrit comment architecturer une application multiplateforme pour maximiser la réutilisation du code et offrir une expérience utilisateur de haute qualité sur toutes les principales plateformes : Windows, Linux, macOS, iOS, Android et WebAssembly.

Contrairement à l'approche de Xamarin.Forms et MAUI, qui tend à produire des applications avec un ensemble de fonctionnalités de plus bas commun multiple et une interface utilisateur générique, Avalonia UI encourage l'exploitation de ses capacités d'interface utilisateur dessinée. Il permet aux développeurs d'écrire leur code de stockage de données et de logique métier une seule fois, tout en offrant une interface utilisateur réactive et performante sur toutes les plateformes. Ce document discute d'une approche architecturale générale pour atteindre cet objectif.

Voici un résumé des points clés pour créer des applications multiplateformes avec Avalonia :

1. **Utilisez .NET** - Développez vos applications en C#, F# ou VB.NET. Le code existant écrit avec .NET peut être facilement porté vers Windows, Linux, macOS, iOS, Android et WebAssembly en utilisant Avalonia.
2. **Utilisez le modèle de conception MVVM** - Développez l'interface utilisateur de votre application en utilisant le modèle `Modèle/Vue/VueModèle`. Cette approche favorise une séparation claire entre le "Modèle" et la "Vue", le "VueModèle" agissant comme intermédiaire. Cela garantit que votre logique UI reste indépendante de la plateforme sous-jacente, favorisant ainsi la réutilisation du code et la maintenabilité.
3. **Exploitez les capacités de dessin d'Avalonia** - Avalonia ne s'appuie pas sur des contrôles UI natifs, mais fonctionne plutôt comme Flutter, en dessinant l'ensemble de l'interface utilisateur. Cela garantit non seulement une apparence et une convivialité cohérentes sur toutes les plateformes, mais offre également un niveau de personnalisation inégalé, vous permettant d'adapter l'interface utilisateur à vos besoins exacts.
4. **Équilibre entre le code de base et le code spécifique à la plateforme** - La clé pour atteindre une grande réutilisation du code est de trouver le bon équilibre entre le code de base indépendant de la plateforme et le code spécifique à la plateforme. Le code de base comprend tout ce qui n'interagit pas directement avec le système d'exploitation sous-jacent.
