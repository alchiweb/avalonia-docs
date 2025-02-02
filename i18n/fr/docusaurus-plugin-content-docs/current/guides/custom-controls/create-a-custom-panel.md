---
id: create-a-custom-panel
title: Comment créer un panneau personnalisé
---

# Comment créer un panneau personnalisé

Cet exemple montre comment remplacer le comportement de mise en page par défaut de l'élément `Panel` et créer des éléments de mise en page personnalisés dérivés de `Panel`.

L'exemple définit un élément `Panel` personnalisé simple appelé `PlotPanel`, qui positionne les éléments enfants selon deux coordonnées x et y codées en dur. Dans cet exemple, `x` et `y` sont tous deux définis sur `50` ; par conséquent, tous les éléments enfants sont positionnés à cet emplacement sur les axes x et y.

Pour mettre en œuvre des comportements personnalisés de `Panel`, l'exemple utilise les méthodes `MeasureOverride` et `ArrangeOverride`. Chaque méthode renvoie les données `Size` nécessaires pour positionner et rendre les éléments enfants.

```csharp
public class PlotPanel : Panel
{
    // Remplace la méthode Measure par défaut de Panel
    protected override Size MeasureOverride(Size availableSize)
    {
        var panelDesiredSize = new Size();

        // Dans notre exemple, nous avons juste un enfant.
        // Indique que notre panneau nécessite juste la taille de son seul enfant.
        foreach (var child in Children)
        {
            child.Measure(availableSize);
            panelDesiredSize = child.DesiredSize;
        }

        return panelDesiredSize;
    }

    protected override Size ArrangeOverride(Size finalSize)
    {
        foreach (var child in Children)
        {
            double x = 50;
            double y = 50;

            child.Arrange(new Rect(new Point(x, y), child.DesiredSize));
        }
        
        return finalSize; // Renvoie la taille finale arrangée.
    }
}
```
