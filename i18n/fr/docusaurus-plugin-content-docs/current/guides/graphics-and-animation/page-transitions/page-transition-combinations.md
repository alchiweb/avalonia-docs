---
id: page-transition-combinations
title: Combinaisons de Transitions de Page
---

# Combinaisons de Transitions de Page

Vous pouvez combiner deux transitions de page intégrées ou plus pour créer un nouvel effet.

Ajoutez un élément `CompositePageTransition` pour combiner les effets de deux transitions intégrées différentes ou plus.

Par exemple, le code ici crée une transition qui glisse les vues en diagonale (le résultat de la combinaison d'un glissement horizontal et vertical), et qui estompe également les anciennes vues et fait apparaître les nouvelles.

```xml title='XAML'
<CompositePageTransition>
    <CrossFade Duration="0:00:00.500" />
    <PageSlide Duration="0:00:00.500" Orientation="Horizontal" />
    <PageSlide Duration="0:00:00.500" Orientation="Vertical" />
</CompositePageTransition>
```

```csharp title='C#'
var compositeTransition = new CompositePageTransition();
compositeTransition.PageTransitions.Add(new PageSlide(TimeSpan.FromMilliseconds(500), PageSlide.SlideAxis.Vertical));
compositeTransition.PageTransitions.Add(new PageSlide(TimeSpan.FromMilliseconds(500), PageSlide.SlideAxis.Horizontal));
compositeTransition.PageTransitions.Add(new CrossFade(TimeSpan.FromMilliseconds(500)));
```

#### Code source

[CompositePageTransition.cs](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Visuals/Animation/CompositePageTransition.cs)

#### Référence

[CompositePageTransition](http://reference.avaloniaui.net/api/Avalonia.Animation/CompositePageTransition/)
