---
id: types-of-control
title: Types de Contrôle
---

Si vous souhaitez créer vos propres contrôles, il existe trois principales catégories de contrôles dans Avalonia. La première chose à faire est de choisir la catégorie de contrôle qui convient le mieux à votre cas d'utilisation.

### Contrôles Utilisateurs

Les UserControls sont le moyen le plus simple de créer des contrôles. Ce type de contrôle est le mieux adapté aux "vues" ou "pages" spécifiques à une application. Les UserControls sont créés de la même manière que vous créeriez une fenêtre : en créant un nouveau UserControl à partir d'un modèle et en y ajoutant des contrôles.

### Contrôles Templatés

Les contrôles basés sur des modèles sont mieux utilisés pour des contrôles génériques qui peuvent être partagés entre diverses applications. Ce sont des contrôles sans apparence, ce qui signifie qu'ils peuvent être re-stylés pour différents thèmes et applications. La majorité des contrôles standard définis par Avalonia rentrent dans cette catégorie.

:::info
Dans WPF/UWP, vous hériteriez de la classe Control pour créer un nouveau contrôle basé sur un modèle, mais dans Avalonia, vous devriez hériter de TemplatedControl. 
:::

:::info
Si vous souhaitez fournir un style pour votre TemplatedControl dans un fichier séparé, n'oubliez pas d'inclure ce fichier dans votre application via StyleInclude. 
:::

### Contrôles de base

Les contrôles de base sont le fondement des interfaces utilisateur - ils se dessinent eux-mêmes en utilisant la géométrie en remplaçant la méthode Visual.Render. Des contrôles tels que TextBlock et Image entrent dans cette catégorie.

:::info
Dans WPF/UWP, vous hériteriez de la classe FrameworkElement pour créer un nouveau contrôle de base, mais dans Avalonia, vous devriez hériter de Control. 
:::
