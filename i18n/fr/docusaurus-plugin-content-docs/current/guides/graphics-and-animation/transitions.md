---
id: transitions
title: Comment utiliser les transitions
---


# Comment utiliser les transitions

Les transitions dans Avalonia sont également fortement inspirées par les animations CSS. Elles écoutent les changements de valeur de la propriété cible et animent ensuite le changement selon ses paramètres. Elles peuvent être définies sur n'importe quel `Control` via la propriété `Transitions` :

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Styles>
        <Style Selector="Rectangle.red">
            <Setter Property="Height" Value="100"/>
            <Setter Property="Width" Value="100"/>
            <Setter Property="Fill" Value="Red"/>
            <Setter Property="Opacity" Value="0.5"/>
        </Style>
        <Style Selector="Rectangle.red:pointerover">
            <Setter Property="Opacity" Value="1"/>
        </Style>
    </Window.Styles>

    <Rectangle Classes="red">
        <Rectangle.Transitions>
            <Transitions>
                <DoubleTransition Property="Opacity" Duration="0:0:0.2"/>
            </Transitions>
        </Rectangle.Transitions>
    </Rectangle>

</Window>
```

L'exemple ci-dessus écoutera les changements dans la propriété `Opacity` du `Rectangle`, et lorsque la valeur change, appliquera une transition fluide de l'ancienne valeur à la nouvelle valeur sur une durée de 2 secondes.

Les transitions peuvent également être définies dans n'importe quel style en utilisant un `Setter` avec `Transitions` comme propriété cible et en les encapsulant dans un objet `Transitions`, comme ceci :

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Styles>
        <Style Selector="Rectangle.red">
            <Setter Property="Height" Value="100"/>
            <Setter Property="Width" Value="100"/>
            <Setter Property="Fill" Value="Red"/>
            <Setter Property="Opacity" Value="0.5"/>
            <Setter Property="Transitions">
                <Transitions>
                    <DoubleTransition Property="Opacity" Duration="0:0:0.2"/>
                </Transitions>
            </Setter>
        </Style>
        <Style Selector="Rectangle.red:pointerover">
            <Setter Property="Opacity" Value="1"/>
        </Style>
    </Window.Styles>

    <Rectangle Classes="red"/>

</Window>
```

Chaque transition a une `Property`, un `Delay`, une `Duration` et une propriété `Easing` optionnelle.

`Property` fait référence à la cible d'une transition pour écouter et animer les valeurs.

`Delay` fait référence à la durée avant que la transition ne soit appliquée à la cible.

`Duration` fait référence à la durée pendant laquelle la transition se joue.

Les fonctions d'assouplissement sont les mêmes que celles décrites dans [Les animations par images clés](./keyframe-animations#easings).

Les types de transitions suivants sont disponibles. Le type correct doit être utilisé en fonction du type de propriété animée.

* `BoxShadowsTransition` : Pour les propriétés cibles `BoxShadows`
* `BrushTransition` : Pour les propriétés cibles `IBrush`
* `ColorTransition` : Pour les propriétés cibles `Color`
* `CornerRadiusTransition` : Pour les propriétés cibles `CornerRadius`
* `DoubleTransitions` : Pour les propriétés cibles `double`
* `FloatTransitions` : Pour les propriétés cibles `float`
* `IntegerTransitions` : Pour les propriétés cibles `int`
* `PointTransition` : Pour les propriétés cibles `Point`
* `SizeTransition` : Pour les propriétés cibles `Size`
* `ThicknessTransition` : Pour les propriétés cibles `Thickness`
* `TransformOperationsTransition` : Pour les propriétés cibles `ITransform`
* `VectorTransition` : Pour les propriétés cibles `Vector`

## Transformations de rendu en transition

Les transformations de rendu appliquées aux contrôles en utilisant une syntaxe similaire à CSS peuvent être mises en transition. L'exemple suivant montre une bordure qui tourne de 45 degrés lorsque le pointeur passe dessus :

```xml title='XAML'
<Border Width="100" Height="100" Background="Red">
    <Border.Styles>
        <Style Selector="Border">
            <Setter Property="RenderTransform" Value="rotate(0)"/>
        </Style>
        <Style Selector="Border:pointerover">
            <Setter Property="RenderTransform" Value="rotate(45deg)"/>
        </Style>
    </Border.Styles>
    <Border.Transitions>
        <Transitions>
            <TransformOperationsTransition Property="RenderTransform" Duration="0:0:1"/>
        </Transitions>
    </Border.Transitions>
</Border>
```

```csharp title='C#'
new Border
{
    Width = 100,
    Height = 100,
    Background = Brushes.Red,
    Styles =
    {
        new Style(x => x.OfType<Border>())
        {
            Setters =
            {
                new Setter(
                    Border.RenderTransformProperty,
                    TransformOperations.Parse("rotate(0)"))
            },
        },
        new Style(x => x.OfType<Border>().Class(":pointerover"))
        {
            Setters =
            {
                new Setter(
                    Border.RenderTransformProperty,
                    TransformOperations.Parse("rotate(45deg)"))
            },
        },
    },
    Transitions = new Transitions
    {
        new TransformOperationsTransition
        {
            Property = Border.RenderTransformProperty,
            Duration = TimeSpan.FromSeconds(1),
        }
    }
};
```

Les transitions disponibles sont :

| Transition   | Exemple                                   | Unités acceptées             |
| ------------ | ----------------------------------------- | ---------------------------- |
| `translate`  | `translate(10px)`, `translate(0px, 10px)` | `px`                         |
| `translateX` | `translateX(10px)`                        | `px`                         |
| `translateY` | `translateY(10px)`                        | `px`                         |
| `scale`      | `scale(10)`, `scale(0, 10)`               |                              |
| `scaleX`     | `scaleX(10)`                              |                              |
| `scaleY`     | `scaleY(10)`                              |                              |
| `skew`       | `skew(90deg)`, `skew(0, 90deg)`           | `deg`, `grad`, `rad`, `turn` |
| `skewX`      | `skewX(90deg)`                            | `deg`, `grad`, `rad`, `turn` |
| `skewY`      | `skewY(90deg)`                            | `deg`, `grad`, `rad`, `turn` |
| `rotate`     | `rotate(90deg)`                           | `deg`, `grad`, `rad`, `turn` |
| `matrix`     | `matrix(1,2,3,4,5,6)`                     |                              |

:::info
Avalonia prend également en charge les transformations de rendu de style WPF telles que `RotateTransform`, `ScaleTransform`, etc. Ces transformations ne peuvent pas être mises en transition : utilisez toujours le format similaire à CSS si vous souhaitez appliquer une transition à une transformation de rendu.
:::
