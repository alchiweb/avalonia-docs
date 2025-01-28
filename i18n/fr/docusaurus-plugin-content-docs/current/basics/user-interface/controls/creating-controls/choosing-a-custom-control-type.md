# Choisir un type de contrôle personnalisé

Avalonia propose différentes approches pour créer des contrôles personnalisés afin de répondre aux besoins spécifiques de votre application. Comprendre les différents types de contrôles personnalisés vous aidera à choisir l'approche la plus appropriée pour vos exigences. Dans Avalonia, les trois types courants de contrôles personnalisés sont les `UserControls`, les contrôles sans apparence et les contrôles dessinés sur mesure.

## UserControl

Un `UserControl` est une approche de haut niveau pour créer des contrôles personnalisés dans Avalonia. Il vous permet de composer un contrôle en combinant des contrôles existants et en définissant la mise en page à l'aide de XAML. Un `UserControl` agit comme un conteneur qui encapsule plusieurs contrôles et fournit une interface utilisateur cohérente.

:::info
En général, les `UserControls` sont utilisés pour représenter des vues spécialisées au sein d'une application, telles qu'une "Vue des détails de l'utilisateur", plutôt que de servir d'éléments d'interface utilisateur polyvalents.
:::

Créer un `UserControl` implique les étapes suivantes :

1. **Définir le XAML** : Créez un nouveau fichier XAML `UserControl` qui définit la mise en page et l'apparence du contrôle en plaçant des contrôles, en définissant des propriétés et en appliquant des styles.

2. **Code-behind** : En option, vous pouvez définir une logique supplémentaire de code-behind pour gérer des événements, modifier le comportement ou fournir des fonctionnalités supplémentaires au `UserControl`.

3. **Réutilisation et Personnalisation** : Les `UserControl`s peuvent être facilement réutilisés et personnalisés au sein d'une application. Ils sont particulièrement utiles lorsque vous souhaitez encapsuler un ensemble spécifique de contrôles et de comportements dans un composant réutilisable ou une "vue".

<GitHubSampleLink title="Contrôle Personnalisé" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/CustomControl"/>

## Contrôles Temporaire (Sans Apparence)

Les contrôles temporaires (également connus sous le nom de "contrôles sans apparence") offrent une approche plus avancée et personnalisable pour créer des contrôles personnalisés dans Avalonia. Un contrôle temporaire sépare le comportement et la logique du contrôle de son apparence visuelle, permettant au contrôle d'être stylisé et modélisé par le développeur de l'application.

Avec les contrôles temporaires, vous définissez le comportement et les propriétés du contrôle dans une classe de code-behind, tandis que la représentation visuelle est spécifiée à travers des modèles de contrôle définis en XAML. Cette séparation permet au développeur de l'application de personnaliser l'apparence et la sensation du contrôle sans modifier son comportement sous-jacent.

:::info
Les contrôles temporaires sont généralement utilisés pour des éléments d'interface utilisateur à usage général qui ne sont pas spécifiques à la logique métier et peuvent nécessiter différents thèmes ou styles visuels. La plupart des [contrôles intégrés](../builtin-controls.md) fournis par Avalonia sont des contrôles temporaires.
:::

La création d'un contrôle temporaire implique les étapes suivantes :

1. **Définir la classe de contrôle** : Créez une nouvelle classe qui dérive de `TemplatedControl`. Cette classe définit le comportement, les propriétés et les événements du contrôle.

2. **Modèle de contrôle** : Créez un [`ControlTheme`](control-themes) en XAML qui spécifie l'apparence visuelle et la structure du contrôle. Le modèle de contrôle définit les parties du contrôle et comment elles doivent être stylisées.

3. **Stylisation et modélisation**: Le développeur d'application peut personnaliser l'apparence du contrôle en modifiant son modèle de contrôle ou en appliquant des styles. Cela permet d'avoir un design visuel cohérent et unifié à travers l'application.

Les contrôles à modèle offrent une plus grande flexibilité et réutilisabilité, ce qui les rend idéaux pour les scénarios où vous souhaitez fournir un contrôle qui peut être stylisé pour correspondre à différents thèmes visuels ou s'adapter à diverses préférences utilisateur.

## Contrôles dessinés sur mesure

Les contrôles dessinés sur mesure offrent le plus haut niveau de personnalisation dans Avalonia. Avec les contrôles dessinés sur mesure, vous avez un contrôle total sur le rendu des éléments visuels du contrôle, vous permettant de créer des représentations visuelles uniques et complexes.

:::info
Les contrôles dessinés sur mesure sont généralement utilisés lorsque le contrôle représente un élément graphique principalement non interactif qui n'aura pas besoin d'être thématisé.
:::

Pour créer un contrôle dessiné sur mesure, vous devez remplacer la méthode `Render` du contrôle et utiliser des API de dessin de bas niveau, telles que `DrawingContext`, pour définir l'apparence du contrôle. Cette approche offre un contrôle précis sur chaque pixel de la représentation visuelle du contrôle.

Créer un contrôle dessiné sur mesure implique les étapes suivantes :

1. **Définir la classe de contrôle** : Créez une nouvelle classe qui dérive de `Control`. Cette classe définira le comportement et la logique de rendu du contrôle.

2. **Remplacer la méthode Render** : Remplacez la méthode `Render` dans la classe de contrôle et utilisez le `DrawingContext` pour dessiner le contenu du contrôle.