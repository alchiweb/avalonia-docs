# Style

La différence la plus évidente par rapport à d'autres frameworks XAML réside dans son système de style. Il existe deux façons de styliser des contrôles dans Avalonia :

- Un [`Style`](../../basics/user-interface/styling) est un style similaire à CSS. Les styles ne sont pas stockés dans une collection `Resources` comme dans WPF, ils sont stockés dans une collection `Styles` séparée.
- Un [`ControlTheme`](../../basics/user-interface/styling/control-themes) est similaire à un `Style` WPF et est généralement utilisé pour créer des thèmes pour des contrôles sans apparence.

## Exemple

Le code suivant montre un `UserControl` qui définit son propre style similaire à CSS.

```xml
<UserControl>
    <UserControl.Styles>
        <!-- Faire en sorte que les TextBlocks avec la classe de style h1 aient une taille de police de 24 points -->
        <Style Selector="TextBlock.h1">
            <Setter Property="FontSize" Value="24"/>
        </Style>
    </UserControl.Styles>
    <TextBlock Classes="h1">Header</TextBlock>
</UserControl>
```

<XpfAd/>