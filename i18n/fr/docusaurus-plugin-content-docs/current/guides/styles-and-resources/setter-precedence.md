---
id: setter-precedence
title: Préférence des Setters
---

import SetterPrecedenceAnimationWrongScreenshot from '/img/guides/styles-and-resources/setter-precedence-animation-wrong.gif';
import SetterPrecedenceAnimationCorrectScreenshot from '/img/guides/styles-and-resources/setter-precedence-animation-correct.gif';

Les `Setters` d'Avalonia sont appliqués dans l'ordre de la `BindingPriority`, puis selon la localité de l'arbre visuel, et enfin l'ordre de la collection `Styles`. La préférence s'applique individuellement à chaque `StyledProperty` afin que le style puisse bénéficier de la composition. Les propriétés `DirectProperty` et CLR ne peuvent pas être stylisées et ne participent donc pas à cette préférence.

## Valeurs de BindingPriority

```cs
Animation = -1, // Priorité la plus élevée
LocalValue = 0,
StyleTrigger,
Template,
Style,
Inherited,
Unset = int.MaxValue, // Priorité la plus basse
```

## Comment la BindingPriority est-elle assignée dans XAML ?

La `BindingPriority` ne peut pas être définie explicitement dans XAML. Les exemples suivants démontrent comment la `BindingPriority` est implicitement assignée dans chaque scénario. Cela est crucial pour concevoir et dépanner des styles qui fonctionnent comme prévu.

### Animation

L'`Animation` a la `BindingPriority` la plus élevée et est appliquée aux `Setter`s dans un `Keyframe` et généralement à travers le système des Transitions.

```xml
<Button Background="Green" Content="Bounces from Red to Blue">
    <Button.Styles>
        <Style Selector="Button">
            <Style.Animations>
                <Animation IterationCount="Infinite" Duration="0:0:2">
                    <KeyFrame Cue="0%">
                        <Setter Property="Background" Value="Red" />
                    </KeyFrame>
                    <KeyFrame Cue="100%">
                        <Setter Property="Background" Value="Blue" />
                    </KeyFrame>
                </Animation>
            </Style.Animations>
        </Style>
    </Button.Styles>
</Button>
```

### LocalValue

Assignée lorsque une propriété XAML est directement définie en dehors d'un `ControlTemplate`. Les deux `Setter`s `Background` ci-dessous auront une priorité de `LocalValue`.

```xml
<Button Background="Orange" />
<Button Background="{DynamicResource ButtonBrush}" />
```

:::tip

Les extensions de balisage de ressources n'ont aucun effet sur la priorité.

:::

### StyleTrigger

Lorsqu'un `Selector` a une activation conditionnelle, la `BindingPriority` du `Setter` est promue de `Style` à `StyleTrigger`. Deux sélecteurs avec une activation conditionnelle auront une priorité égale, indépendamment du nombre d'activateurs présents et de la position de l'activateur dans la syntaxe du sélecteur. Avalonia n'a pas le concept de Spécificité de CSS.

```xml
<Style Selector="Button:pointerover /template/ ContentPresenter#PART_ContentPresenter">
    <Setter Property="Background" Value="Orange" />
</Style>
```

:::tip

Les sélecteurs de classe de style, de pseudo-classe, de position d'enfant et de correspondance de propriété sont conditionnels. Les sélecteurs de nom de contrôle ne sont pas conditionnels.

:::

### Modèle

Lorsqu'une propriété est directement définie dans un `ControlTemplate`. `BorderThickness`, `Background` et `Padding` ci-dessous ont la priorité `Template`.

```xml
<ControlTemplate>
    <Border BorderThickness="2">
        <Button Background="{DynamicResource ButtonBrush}" Padding="{TemplateBinding Padding}" />
    </Border>
</ControlTemplate>
```
### Style

Lorsqu'un `Setter` est défini dans un `Style` sans activation conditionnelle.

```xml
<Style Selector="Button /template/ ContentPresenter#PART_ContentPresenter">
    <Setter Property="Background" Value="Orange" />
</Style>
```

:::tip

Il est particulièrement à noter que la priorité est inférieure à celle de `Template`. Par conséquent, ces sélecteurs ne peuvent pas être utilisés pour remplacer les propriétés mentionnées dans l'exemple `Template` ci-dessus.

:::

### Hérité

Lorsqu'une propriété n'est pas définie, elle peut hériter de la valeur de propriété de son parent. Cela doit être spécifié lors de l'enregistrement de la propriété ou avec `OverrideMetadata`.

```cs
public static readonly StyledProperty<bool> UseLayoutRoundingProperty =
    AvaloniaProperty.Register<Layoutable, bool>(
        nameof(UseLayoutRounding),
        defaultValue: true,
        inherits: true);
```


## Localité de l'Arbre Visuel

Les `Setter`s avec une `BindingPriority` égale sont ensuite sélectionnés par leur emplacement dans l'Arbre Visuel par rapport au `Control`. Le `Setter` avec le moins de nœuds requis pour remonter afin de localiser prendra la priorité. Les `Setter`s de style en ligne ont la plus haute priorité pour cette étape.

```xml
<Window>
    <Window.Styles>
        <Style Selector="Button">
            <Setter Property="FontSize" Value="16" />
            <Setter Property="Foreground" Value="Red" />
        </Style>
    </Window.Styles>
    <StackPanel>
        <StackPanel.Styles>
            <Style Selector="Button">
                <Setter Property="FontSize" Value="24" />
            </Style>
        </StackPanel.Styles>

        <Button Content="This Has FontSize=24 with Foreground=Red" />
    </StackPanel>
</Window>
```

## Ordre de Collection des Styles

Lorsque la `BindingPriority` et la localité de l'arbre visuel sont toutes deux égales, le dernier décideur est l'ordre dans la collection `Styles`. Le dernier `Setter` applicable prendra la priorité.

```xml
<StackPanel>
    <StackPanel.Styles>
        <Style Selector="Button.small">
            <Setter Property="FontSize" Value="12" />
        </Style>
        <Style Selector="Button.big">
            <Setter Property="FontSize" Value="24" />
        </Style>
    </StackPanel.Styles>

    <Button Classes="small big" Content="This Has FontSize=24" />
    <Button Classes="big small" Content="This Also Has FontSize=24" />
</StackPanel>
```

:::info

Ces Boutons spécifient leurs Classes dans un ordre différent, mais cela n'a aucun effet sur la Précédence des Setter.

:::

## La BindingPriority ne se propage pas

Rappelez-vous l'exemple `Animation` ci-dessus. Si vous survolez, l'arrière-plan animé est remplacé par un arrière-plan statique malgré le fait que `BindingPriority.Animation` ait la priorité la plus élevée. Cela est dû au fait que le `Selector` cible le mauvais `Control`. Examiner le `ControlTheme` est nécessaire pour diagnostiquer la cause.

<img src={SetterPrecedenceAnimationWrongScreenshot} alt="" />

```xml title='ControlTheme for Button, Trimmed'
<ControlTheme x:Key="{x:Type Button}" TargetType="Button">
    <Setter Property="Background" Value="{DynamicResource ButtonBackground}"/>
    <Setter Property="Template">
        <ControlTemplate>
            <ContentPresenter x:Name="PART_ContentPresenter"
                              Background="{TemplateBinding Background}"/>
        </ControlTemplate>
    </Setter>

    <Style Selector="^:pointerover /template/ ContentPresenter#PART_ContentPresenter">
        <Setter Property="Background" Value="{DynamicResource ButtonBackgroundPointerOver}"/>
    </Style>
</ControlTheme>
```

Le `Setter` supérieur applique le `ButtonBackground` au `Button` avec une priorité de `Style`. Le rendu de l'`Arrière-plan` est géré par le `ContentPresenter` qui a une priorité de `Template`. Il récupère le `ButtonBackground` qui a été appliqué au `Button`.

Cependant, lorsque le `Button` est survolé, le `Selector` `:pointerover` est activé avec sa priorité de `StyleTrigger`, remplace le `TemplateBinding` et récupère `ButtonBackgroundPointerOver` à la place. Cela contourne la récupération de l'`Arrière-plan` du `Button` que notre `Selector` d'`Animation` original ciblait. Cela est résumé dans le tableau suivant :

| Setters pour les arrière-plans et les styles lors du survol              | Priorité                         | Location        |
|---------------------------------------------------------------------|-----------------------------------|-----------------|
| ~~Background="Green"~~                                              | LocalValue                        | Button          |
| Background="Red"                                                    | Animation (Overrides LocalValue)  | Keyframe        |
| ~~`<ContentPresenter Background="{TemplateBinding Background}"/>`~~ | Template                          | ControlTemplate |
| `^:pointerover /template/ ContentPresenter#PART_ContentPresenter`   | StyleTrigger (Overrides Template) | ControlTheme    |

Au lieu de cela, nous devrions cibler le `ContentPresenter` avec un `Setter` qui a une priorité d'au moins `StyleTrigger`. `BindingPriority.Animation` correspond à cela. C'est une observation qui ne peut être faite sans examiner le `ControlTemplate` original et souligne que s'appuyer uniquement sur la priorité est insuffisant pour styliser efficacement une application.

```xml title='Corrected to override :pointerover priority'
<Button Background="Green" Content="Bounces from Red to Blue">
    <Button.Styles>
        <Style Selector="Button /template/ ContentPresenter#PART_ContentPresenter">
            <Style.Animations>
                <Animation IterationCount="Infinite" Duration="0:0:2">
                    <KeyFrame Cue="0%">
                        <Setter Property="Background" Value="Red" />
                    </KeyFrame>
                    <KeyFrame Cue="100%">
                        <Setter Property="Background" Value="Blue" />
                    </KeyFrame>
                </Animation>
            </Style.Animations>
        </Style>
    </Button.Styles>
</Button>
```

<img src={SetterPrecedenceAnimationCorrectScreenshot} alt="" />
