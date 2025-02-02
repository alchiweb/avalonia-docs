---
id: architecture
title: Architecture
---

Un aspect crucial de la création d'applications multiplateformes avec Avalonia est de créer une architecture qui permet un maximum de partage de code entre différentes plateformes. En adhérant aux principes fondamentaux de la programmation orientée objet, vous pouvez établir une application bien structurée :

1. **Encapsulation** – Cela implique de s'assurer que les classes et les couches architecturales n'exposent qu'une API minimale qui exécute leurs fonctions nécessaires tout en dissimulant les détails d'implémentation internes. En termes pratiques, cela signifie que les objets fonctionnent comme des « boîtes noires », et le code qui les utilise n'a pas besoin de comprendre leur fonctionnement interne. Architecturale, cela implique de mettre en œuvre des modèles comme la Façade qui promeut une API simplifiée orchestrant des interactions plus complexes au nom du code dans des couches abstraites supérieures. Ainsi, le code UI devrait se concentrer uniquement sur l'affichage des écrans et l'acceptation des entrées utilisateur, sans jamais interagir directement avec des bases de données ou d'autres opérations de bas niveau.
2. **Séparation des Responsabilités** – Chaque composant, qu'il soit au niveau architectural ou de classe, devrait avoir un but clair et défini. Chaque composant devrait exécuter ses tâches spécifiées et exposer cette fonctionnalité via une API accessible aux autres classes qui en ont besoin.
3. **Polymorphisme** – Programmer à une interface (ou une classe abstraite) supportant plusieurs implémentations permet d'écrire et de partager un code central à travers les plateformes tout en interagissant avec les fonctionnalités spécifiques à la plateforme offertes par Avalonia.

Le résultat de ces principes est une application modélisée sur des entités réelles ou abstraites avec des couches logiques distinctes.

Séparer le code en couches rend l'application plus facile à comprendre, tester et maintenir. Il est conseillé de garder le code de chaque couche physiquement séparé (soit dans des répertoires différents ou même des projets séparés pour des applications plus grandes) ainsi que logiquement séparé (en utilisant des espaces de noms). Avec Avalonia, vous pouvez partager non seulement la logique métier, mais aussi le code UI à travers les plateformes, réduisant ainsi le besoin de plusieurs projets UI et améliorant encore la réutilisation du code.

## Couches d'application typiques

Dans ce document et les études de cas pertinentes, nous faisons référence aux cinq couches d'application suivantes :

1. **Couche de données** – C'est ici que la persistance des données non volatiles se produit, probablement via une base de données comme SQLite ou LiteDB, mais cela pourrait être mis en œuvre avec des fichiers XML ou d'autres mécanismes appropriés.
2. **Couche d'accès aux données** – Cette couche est un wrapper autour de la couche de données fournissant des opérations de Création, Lecture, Mise à jour, Suppression (CRUD) sur les données sans révéler les détails d'implémentation à l'appelant. Par exemple, la DAL pourrait contenir des requêtes SQL pour interagir avec les données, mais le code qui s'y réfère n'a pas besoin d'en être conscient.
3. **Couche Métier** – Parfois appelée Couche de Logique Métier ou BLL, cette couche abrite les définitions des entités métier (le Modèle) et la logique métier. C'est un candidat privilégié pour le modèle de façade métier.
4. **Couche d'Accès aux Services** – Cette couche est utilisée pour accéder aux services dans le cloud, allant des services web complexes (REST, JSON) à la simple récupération de données et d'images depuis des serveurs distants. Elle encapsule le comportement réseau et fournit une API simplifiée pour la consommation par les couches Application et UI.
5. **Couche Application** – Cette couche contient du code qui est généralement spécifique à la plateforme ou du code qui est spécifique à l'application (pas typiquement réutilisable). Dans le cadre d'Avalonia, c'est dans cette couche que vous décidez quelles fonctionnalités spécifiques à la plateforme exploiter, le cas échéant. La distinction entre cette couche et la couche UI devient plus claire avec Avalonia, car le code UI peut être partagé entre les plateformes.
6. **Couche Interface Utilisateur (UI)** – Cette couche orientée utilisateur contient des vues et les modèles de vue qui les gèrent. Avalonia permet à cette couche d'être partagée sur toutes les plateformes prises en charge, contrairement aux architectures traditionnelles où la couche UI serait spécifique à la plateforme.

Une application peut ne pas contenir toutes les couches – par exemple, la Couche d'Accès aux Services ne serait pas présente dans une application qui n'accède pas aux ressources réseau. Une application plus simple pourrait fusionner la Couche de Données et la Couche d'Accès aux Données car les opérations sont extrêmement basiques.
Avec Avalonia, vous avez la flexibilité de façonner l'architecture de votre application pour répondre à vos besoins spécifiques, tout en bénéficiant d'un haut degré de réutilisabilité du code sur différentes plateformes.

## Modèles Architecturaux Communs

Les modèles sont une approche bien établie pour capturer des solutions récurrentes à des problèmes communs. Il existe plusieurs modèles clés qui sont précieux à comprendre lors de la construction d'applications maintenables et compréhensibles avec Avalonia.

### Modèle, Vue, ViewModel (MVVM)
Un modèle populaire et souvent mal compris, le MVVM est principalement utilisé lors de la construction d'interfaces utilisateur et favorise une séparation entre la définition réelle d'un écran UI (Vue), la logique qui le sous-tend (ViewModel) et les données qui le peuplent (Modèle). Le ViewModel agit comme un intermédiaire entre la Vue et le Modèle. Le Modèle, bien que crucial, est une pièce distincte et optionnelle, et ainsi, l'essence de la compréhension de ce modèle réside dans la relation entre la Vue et le ViewModel.

:::info
[En savoir plus sur le MVVM](../../concepts/the-mvvm-pattern/).
:::

### Façade Métier
Également connu sous le nom de modèle Manager, cela fournit un point d'entrée simplifié pour des opérations complexes. Par exemple, dans une application de suivi des tâches, vous pourriez avoir une classe TaskManager avec des méthodes telles que GetAllTasks(), GetTask(taskID), SaveTask(task), etc. La classe TaskManager fournit une façade aux mécanismes internes de sauvegarde/récupération des objets de tâches.

### Singleton
Le modèle Singleton garantit qu'une seule instance d'un objet particulier peut exister. Par exemple, lors de l'utilisation de SQLite dans des applications, vous souhaitez généralement qu'il n'y ait qu'une seule instance de la base de données. Le modèle Singleton est une méthode efficace pour imposer cela.

### Fournisseur
Un modèle à l'origine créé par Microsoft pour promouvoir la réutilisation du code dans les applications Silverlight, WPF et WinForms. Le code partagé peut être écrit contre une interface ou une classe abstraite, et des implémentations concrètes spécifiques à la plateforme sont écrites et passées lorsque le code est utilisé. Dans Avalonia, puisque nous pouvons partager à la fois l'interface utilisateur et la logique de l'application, ce modèle peut aider à gérer les exceptions spécifiques à la plateforme ou à tirer parti des fonctionnalités spécifiques à la plateforme.

### Async
À ne pas confondre avec le mot-clé `Async`, le modèle Async est utilisé lorsque des tâches de longue durée doivent être exécutées sans bloquer l'interface utilisateur ou le traitement en cours. Dans sa forme la plus simple, le modèle Async décrit que les tâches de longue durée doivent être lancées dans un autre thread (ou une abstraction de thread similaire comme une tâche) pendant que le thread actuel continue de traiter et d'écouter une réponse du processus en arrière-plan, mettant à jour l'interface utilisateur lorsque des données et/ou un état sont renvoyés. Cela est essentiel pour maintenir une interface utilisateur réactive dans les applications Avalonia.

---
Chaque modèle mentionné ci-dessus sera exploré en profondeur alors que leur application pratique est démontrée dans nos études de cas. Pour une compréhension plus complète des modèles [Facade](https://fr.wikipedia.org/wiki/Facade_pattern), [Singleton](https://fr.wikipedia.org/wiki/Singleton_pattern) et [Provider](https://fr.wikipedia.org/wiki/Provider_model), ainsi que des [Design Patterns](https://fr.wikipedia.org/wiki/Design_Patterns) en général, vous voudrez peut-être approfondir les ressources disponibles sur des plateformes comme Wikipedia.
