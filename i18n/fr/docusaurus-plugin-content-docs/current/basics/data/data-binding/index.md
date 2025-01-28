---
description: CONCEPTS
---

import DataBindingOverviewDiagram from '/img/basics/data-binding/data-binding-overview.png';

# Liaison de données

Avalonia utilise la liaison de données pour transférer des données des objets d'application vers les contrôles de l'interface utilisateur, modifier les données dans les objets d'application en réponse aux entrées de l'utilisateur et initier des actions sur les objets d'application en réponse aux commandes de l'utilisateur.

<img src={DataBindingOverviewDiagram} alt=''/>

Dans cet agencement, le contrôle est la **cible de liaison**, et l'objet est la **source de données**.

Avalonia exécute un système de liaison de données pour compléter une grande partie de l'activité ci-dessus à partir de simples mappages déclarés dans le XAML ; c'est-à-dire sans nécessiter d'ajouter beaucoup de code supplémentaire.

Les mappages de liaison de données sont définis à l'aide de XML entre les attributs d'un contrôle Avalonia et les propriétés d'un objet d'application. En termes généraux, la syntaxe est la suivante :

```xml
<SomeControl Attribute="{Binding PropertyName}" />
```

Les mappages peuvent être bidirectionnels : où les modifications des propriétés d'un objet d'application lié sont reflétées dans le contrôle, et les modifications dans le contrôle (quelles qu'en soient les causes) sont appliquées à l'objet sous-jacent. Un exemple de liaison bidirectionnelle est une entrée de texte liée à une propriété de chaîne d'un objet. Le XML pourrait ressembler à ceci :

```xml
<TextBox Text="{Binding FirstName}" />
```

Si l'utilisateur modifie le texte dans la zone de texte, alors la propriété `FirstName` de l'objet sous-jacent est automatiquement mise à jour. Dans l'autre sens, si la propriété `FirstName` de l'objet sous-jacent change, alors le texte visible dans la zone de texte est mis à jour.

Les liaisons peuvent être unidirectionnelles : où les changements dans les propriétés d'un objet d'application lié sont reflétés dans le contrôle, mais l'utilisateur ne peut pas modifier le contrôle. Un exemple de cela serait le contrôle de bloc de texte, qui est en lecture seule.

```
<TextBlock Text="{Binding StatusMessage}" />
```

La liaison est utilisée avec le modèle architectural MVVM, et c'est l'un des principaux moyens de programmer avec Avalonia UI.

:::info
Pour plus d'informations sur la façon d'utiliser le modèle MVVM avec Avalonia, consultez la page conceptuelle [ici](../../../concepts/the-mvvm-pattern).
:::

:::info
Pour des informations de base sur les origines et le développement du modèle MVVM chez _Microsoft_, consultez l'article _Microsoft Patterns and Practices_ [ici](https://msdn.microsoft.com/en-us/library/hh848246.aspx).
:::

À la page suivante, vous apprendrez d'où le lieur de données obtient l'objet de données.