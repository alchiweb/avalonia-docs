---
title: Interpolation d'Image
description: CONCEPTS
---

Lors de l'affichage d'images dans Avalonia, en particulier lors de leur mise à l'échelle à des tailles différentes de leur résolution native, la qualité du rendu dépend du mode d'interpolation utilisé. Ce guide explique comment contrôler l'interpolation d'image dans vos applications Avalonia.

## Comportement par Défaut

Depuis Avalonia 11, le mode d'interpolation par défaut est réglé sur `LowQuality`. Ce paramètre privilégie les performances mais peut entraîner un rendu d'image moins fluide lors de la mise à l'échelle des images, en particulier lors de leur affichage à des tailles nettement plus petites que leurs dimensions d'origine.

## Modes d'Interpolation

Avalonia prend en charge plusieurs modes d'interpolation de bitmap qui peuvent être appliqués au rendu d'images :

- `None` : Pas d'interpolation
- `LowQuality` : Interpolation basique (par défaut)
- `MediumQuality` : Interpolation équilibrée
- `HighQuality` : Interpolation lisse, idéale pour réduire la taille des images

## Définir le Mode d'Interpolation

### Paramètre par Contrôle

Vous pouvez définir le mode d'interpolation sur des contrôles individuels en utilisant la propriété attachée `RenderOptions.BitmapInterpolationMode` :

```xml
<Image Source="assets/myimage.png" 
       RenderOptions.BitmapInterpolationMode="HighQuality" />
```

Ceci peut également être appliqué aux conteneurs :

```xml
<Border RenderOptions.BitmapInterpolationMode="HighQuality">
    <Image Source="assets/myimage.png" />
</Border>
```

### Cas d'Utilisation Courants

1. **Icon Display**: When displaying icons that are being scaled down, using `HighQuality` interpolation can prevent jagged edges:
```xml
<Button>
    <Image Source="assets/icon.png" 
           Width="16" 
           Height="16"
           RenderOptions.BitmapInterpolationMode="HighQuality" />
</Button>
```

2. **Image Galleries**: For image galleries where quality is important:
```xml
<ItemsControl RenderOptions.BitmapInterpolationMode="HighQuality">
    <ItemsControl.ItemTemplate>
        <DataTemplate>
            <Image Source="{Binding ImagePath}" />
        </DataTemplate>
    </ItemsControl.ItemTemplate>
</ItemsControl>
```

## Performance Considerations

The interpolation mode is set per-control by design for performance reasons. Higher quality interpolation requires more computational resources, so consider these guidelines:

- Use `HighQuality` for:
  - Important UI elements like logos
  - Scaled-down images where quality is crucial
  - Photo galleries or image-focused interfaces
  
- Use default `LowQuality` for:
  - Background images
  - Decorative elements where quality is less critical
  - Performance-sensitive applications

## Creating a Global Setting

While Avalonia doesn't provide a built-in way to set a global interpolation mode, you can create a custom attached property or behavior to manage this across your application. Here's an example approach:

```csharp
public static class GlobalImageOptions
{
    public static readonly AttachedProperty<BitmapInterpolationMode> InterpolationModeProperty =
        AvaloniaProperty.RegisterAttached<Image, BitmapInterpolationMode>(
            "InterpolationMode",
            typeof(GlobalImageOptions),
            defaultValue: BitmapInterpolationMode.HighQuality);

    public static void SetInterpolationMode(Image image, BitmapInterpolationMode value)
    {
        image.SetValue(RenderOptions.BitmapInterpolationModeProperty, value);
    }
}
```

Ensuite, dans votre XAML :

```xml
<Style Selector="Image">
<Setter Property="(local:GlobalImageOptions.InterpolationMode)"
Value="HighQuality" />
</Style>
```

## Conseils pour de meilleurs résultats

1. **Préparation des actifs** :
- Fournissez des images à des résolutions appropriées pour leur taille d'affichage prévue
- Envisagez d'inclure plusieurs résolutions pour les actifs importants
- Utilisez des formats vectoriels (SVG) lorsque cela est possible pour des graphiques indépendants de la résolution

2. **Considérations de mise en page** :
- Soyez conscient des dimensions d'image d'origine par rapport à la taille d'affichage
- Utilisez des conteneurs appropriés et des panneaux de mise en page pour gérer le redimensionnement des images
- Envisagez d'utiliser les modes d'étirement `UniformToFill` ou `Uniform` avec une interpolation de haute qualité

3. **Tests** :
- Testez le rendu des images sur différentes densités d'écran
- Vérifiez l'impact sur les performances lors de l'utilisation de l'interpolation de haute qualité sur de nombreuses images
- Vérifiez l'utilisation de la mémoire avec différents paramètres d'interpolation
