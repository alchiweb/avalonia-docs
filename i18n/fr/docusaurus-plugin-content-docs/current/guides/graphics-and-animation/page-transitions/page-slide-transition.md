---
id: page-slide-transition
title: Transition de Glissement de Page
---

# Transition de Glissement de Page

La transition de glissement de page déplace l'ancienne page hors de la vue et la nouvelle page dans la vue, pour la durée donnée. Vous pouvez spécifier la direction du glissement en utilisant la propriété d'orientation (par défaut horizontale).

```xml title='XAML'
<PageSlide Duration="0:00:00.500" Orientation="Vertical" />
```

```csharp title='C#'
var transition = new PageSlide(TimeSpan.FromMilliseconds(500), 
                                PageSlide.SlideAxis.Vertical);
```

## Plus d'informations

:::info
Pour la documentation API complète concernant cette transition, voir [ici](http://reference.avaloniaui.net/api/Avalonia.Animation/PageSlide/).
:::

:::info
Voir le code source sur _GitHub_ [`PageSlide.cs`](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Base/Animation/PageSlide.cs)
:::
