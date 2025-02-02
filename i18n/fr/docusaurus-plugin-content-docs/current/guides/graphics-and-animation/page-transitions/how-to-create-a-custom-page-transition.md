---
id: how-to-create-a-custom-page-transition
title: Comment Créer une Transition de Page Personnalisée
---

import CustomPageTransitionScreenshot from '/img/guides/graphics-and-animations/custom-page-transition.webp';

# Comment Créer une Transition de Page Personnalisée

Ce guide vous montrera comment créer votre propre transition de page personnalisée en implémentant l'interface `IPageTransition`.

L'interface a une seule méthode que vous devez implémenter :

```csharp
public Task Start(Visual? from, Visual? to, bool forward, 
                                CancellationToken cancellationToken)
{
    // Configurer la transition ici.
}
```

## Exemple

Cet exemple réduira la vue ancienne puis ouvrira la nouvelle vue verticalement.

```csharp
using Avalonia.VisualTree;

public class CustomTransition : IPageTransition
{
    /// <summary>
    /// Initialise une nouvelle instance de la classe <see cref="CustomTransition"/>.
    /// </summary>
    public CustomTransition()
    {
    }

    /// <summary>
    /// Initialise une nouvelle instance de la classe <see cref="CustomTransition"/>.
    /// </summary>
    /// <param name="duration">La durée de l'animation.</param>
    public CustomTransition(TimeSpan duration)
    {
        Duration = duration;
    }

    /// <summary>
    /// Obtient la durée de l'animation.
    /// </summary>
    public TimeSpan Duration { get; set; }

    public async Task Start(Visual from, Visual to, bool forward, 
                                            CancellationToken cancellationToken)
    {
        if (cancellationToken.IsCancellationRequested)
        {
            return;
        }

        var tasks = new List<Task>();
        var parent = GetVisualParent(from, to);
        var scaleYProperty = ScaleTransform.ScaleYProperty;

        if (from != null)
        {
            var animation = new Animation
            {
                FillMode = FillMode.Forward,
                Children =
                {
                    new KeyFrame
                    {
                        Setters = { new Setter 
                        { Property = scaleYProperty, Value = 1d } },
                        Cue = new Cue(0d)
                    },
                    new KeyFrame
                    {
                        Setters =
                        {
                            new Setter
                            {
                                Property = scaleYProperty,
                                Value = 0d
                            }
                        },
                        Cue = new Cue(1d)
                    }
                },
                Duration = Duration
            };
            tasks.Add(animation.RunAsync(from, cancellationToken));
        }

        if (to != null)
        {
            to.IsVisible = true;
            var animation = new Animation
            {
                FillMode = FillMode.Forward,
                Children =
                {
                    new KeyFrame
                    {
                        Setters =
                        {
                            new Setter
                            {
                                Property = scaleYProperty,
                                Value = 0d
                            }
                        },
                        Cue = new Cue(0d)
                    },
                    new KeyFrame
                    {
                        Setters = { new Setter 
                        { 
                            Property = scaleYProperty, Value = 1d 
                        }},
                        Cue = new Cue(1d)
                    }
                },
                Duration = Duration
            };
            tasks.Add(animation.RunAsync(to, cancellationToken));
        }

        await Task.WhenAll(tasks);

        if (from != null && !cancellationToken.IsCancellationRequested)
        {
            from.IsVisible = false;
        }
    }

    /// <summary>
    /// Obtient le parent visuel commun des deux contrôles.
    /// </summary>
    /// <param name="from">Le contrôle d'origine.</param>
    /// <param name="to">Le contrôle de destination.</param>
    /// <returns>Le parent commun.</returns>
    /// <exception cref="ArgumentException">
    /// Les deux contrôles ne partagent pas un parent commun.
    /// </exception>
    /// <remarks>
    /// L'un des paramètres peut être nul, mais pas les deux.
    /// </remarks>
    private static Visual GetVisualParent(Visual? from, Visual? to)
    {
        var p1 = (from ?? to)!.GetVisualParent();
        var p2 = (to ?? from)!.GetVisualParent();

        if (p1 != null && p2 != null && p1 != p2)
        {
            throw new ArgumentException(
                                "Les contrôles pour PageSlide doivent avoir le même parent.");
        }

        return p1 ?? throw new InvalidOperationException(
                                                "Impossible de déterminer le parent visuel.");
    }
}
```

<img src={CustomPageTransitionScreenshot} alt=''/>

## Plus d'informations

:::info
Pour la documentation API complète concernant cette interface, voir [ici](http://reference.avaloniaui.net/api/Avalonia.Animation/IPageTransition/).
:::

:::info
Voir le code source sur _GitHub_ [`IPageTransition.cs`](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Base/Animation/IPageTransition.cs)
:::
