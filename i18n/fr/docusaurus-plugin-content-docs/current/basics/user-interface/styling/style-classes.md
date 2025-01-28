---
id: style-classes
title: Classes de Style
---

Vous pouvez attribuer à un contrôle _Avalonia UI_ une ou plusieurs classes de style, et les utiliser pour guider la sélection de style. Les classes de style sont attribuées dans un élément de contrôle à l'aide de l'attribut `Classes`. Si vous souhaitez attribuer plus d'une classe, utilisez une liste séparée par des espaces.

Par exemple, ce bouton a à la fois les classes de style `h1` et `bleu` appliquées :

```xml
<Button Classes="h1 bleu"/>
```

## Classes Pseudo

Comme en CSS, les contrôles peuvent avoir des classes pseudo ; ce sont des classes définies dans le contrôle lui-même plutôt que par l'utilisateur. Les noms des classes pseudo dans un sélecteur commencent toujours par un deux-points.

Par exemple, la classe pseudo `:pointerover` indique que l'entrée du pointeur est actuellement au-dessus (à l'intérieur des limites) d'un contrôle. (Cette classe pseudo est similaire à `:hover` en CSS.)

Voici un exemple d'un sélecteur de classe pseudo `:pointerover` :

```xml
<StackPanel>
  <StackPanel.Styles>
    <Style Selector="Border:pointerover">
      <Setter Property="Background" Value="Red"/>
    </Style>
  </StackPanel.Styles>
  <Border>
    <TextBlock>J'aurai un fond rouge quand je serai survolé.</TextBlock>
  </Border>
</StackPanel>
```

Dans cet exemple, le sélecteur de classe pseudo change les propriétés à l'intérieur d'un modèle de contrôle :

```xml
<StackPanel>
  <StackPanel.Styles>
    <Style Selector="Button:pressed /template/ ContentPresenter">
        <Setter Property="TextBlock.Foreground" Value="Red"/>
    </Style>
  </StackPanel.Styles>
  <Button>J'aurai du texte rouge quand je suis pressé.</Button>
</StackPanel>
```

D'autres pseudo-classes incluent `:focus`, `:disabled`, `:pressed` pour les boutons, et `:checked` pour les cases à cocher.

:::info
Pour plus de détails sur les pseudo-classes, consultez la référence [ici](../../../reference/styles/pseudo-classes.md).
:::

## Classes conditionnelles

Si vous devez ajouter ou supprimer une classe en utilisant une condition liée, vous pouvez utiliser la syntaxe spéciale suivante :

```xml
<Button Classes.accent="{Binding IsSpecial}" />
```

## Classes dans le code

Vous pouvez manipuler les classes de style dans le code en utilisant la collection `Classes` :

```csharp
control.Classes.Add("bleu");
control.Classes.Remove("rouge");
```
