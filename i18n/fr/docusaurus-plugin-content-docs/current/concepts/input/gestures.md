---
description: CONCEPTS - Input
---

# Gestes

Les contrôles peuvent détecter des gestes en utilisant des reconnaisseurs de gestes. Ces reconnaisseurs sont intégrés dans les contrôles et écoutent et suivent les événements de pointeur que le contrôle reçoit, envoyant des événements de geste lorsqu'ils ont détecté qu'un geste a commencé.

Tous les reconnaisseurs de gestes dérivent de la classe de base `GestureRecognizer` et peuvent être attachés à un contrôle en utilisant la propriété `GestureRecognizers` du contrôle. Ce qui suit montre une Image hébergeant un `ScrollGestureRecognizer` :

```xml
<Image Stretch="UniformToFill"
        Margin="5"
        Name="image"
        Source="/image.jpg">
  <Image.GestureRecognizers>
    <ScrollGestureRecognizer CanHorizontallyScroll="True"
                              CanVerticallyScroll="True"/>
  </Image.GestureRecognizers>
</Image>
```


```csharp title='C#'
image.GestureRecognizers.Add(new ScrollGestureRecognizer()
            {
                CanVerticallyScroll = true,
                CanHorizontallyScroll = true,
            });
```

## Plus d'informations

:::info
Vous pouvez consulter plus d'informations sur les reconnaisseurs de gestes disponibles [ici](../../reference/gestures)

Vous pouvez consulter le code source des classes liées

[GestureRecognizer](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Base/Input/GestureRecognizers/GestureRecognizer.cs)

[GestureRecognizerCollection](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Base/Input/GestureRecognizers/GestureRecognizerCollection.cs)
:::