---
id: how-to-create-a-custom-flyout
title: Comment créer un Flyout personnalisé
---

# Comment créer un Flyout personnalisé

## Création de Flyouts personnalisés

Pour créer un type de flyout personnalisé, dérivez de FlyoutBase. Vous devrez remplacer la méthode abstraite `CreatePresenter()` pour spécifier le présentateur que le `Flyout` doit utiliser pour afficher son contenu. Cela peut être n'importe quel type de contrôle, mais notez qu'il s'agit du contenu racine pour le popup interne et qu'il doit être stylisé avec un arrière-plan, une bordure, un rayon d'angle, etc. pour correspondre à d'autres popups. Vous pouvez toujours utiliser un `FlyoutPresenter` normal si vous le souhaitez.

L'exemple suivant crée un simple `Flyout` qui héberge une image :

```csharp
public class MyImageFlyout : FlyoutBase
{
    public static readonly StyledProperty<IImage> ImageProperty = AvaloniaProperty.Register<MyImageFlyout, IImage>(nameof(Image));

    [Content]
    public IImage Image { get; set; }

    protected override Control CreatePresenter()
    {
        // In this example, we'll use the default FlyoutPresenter as the root content, and add an Image control to show our content
        return new FlyoutPresenter
        {
            Content = new Image
            {
                // Use binding here so the image automatically updates when the property updates
                [!Image.SourceProperty] = this[!ImageProperty]
            }
        };
    }
}
```

##
