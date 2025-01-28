---
id: control-themes
title: Thèmes de contrôle
---

import StylingEllipseButtonScreenshot from '/img/basics/user-interface/styling/ellipse-button.png';

Les thèmes de contrôle s'appuient sur les [Styles](styles) pour créer des thèmes interchangeables pour les contrôles. Les thèmes de contrôle sont analogues aux Styles dans WPF/UWP, bien que leur mécanisme soit légèrement différent.

:::tip
Étant donné que les thèmes de contrôle sont basés sur les styles, il est important de comprendre d'abord le [système de style](styles) d'Avalonia.
:::

## Introduction

Avant Avalonia 11, les thèmes de contrôle étaient créés à l'aide de styles standard. Cependant, cette approche présentait un problème fondamental : une fois qu'un style était appliqué à un contrôle, il n'y avait aucun moyen de le supprimer. Par conséquent, si vous vouliez changer le thème pour une instance spécifique d'un contrôle ou une section de l'interface utilisateur (UI), la seule option était d'appliquer un deuxième thème au contrôle et d'espérer qu'il remplacerait toutes les propriétés définies dans le thème d'origine.

La solution à ce problème a été introduite dans Avalonia 11 sous la forme des _Thèmes de contrôle_.

Les thèmes de contrôle sont eux-mêmes des styles, mais avec quelques différences importantes :

- Les thèmes de contrôle n'ont pas de sélecteur : à la place, ils ont une propriété `TargetType` qui décrit le contrôle qu'ils ciblent
- Les thèmes de contrôle sont stockés dans un `ResourceDictionary` au lieu d'une collection `Styles`
- Les thèmes de contrôle sont attribués à un contrôle en définissant la propriété `Theme`, généralement à l'aide de l'extension de balisage `{StaticResource}`.

:::info
Les thèmes de contrôle sont généralement appliqués aux contrôles [modélisés (sans apparence)](../controls/creating-controls/choosing-a-custom-control-type.md), mais ils peuvent en fait être appliqués à n'importe quel contrôle. Cependant, pour les contrôles non modélisés, il est souvent plus pratique d'utiliser des styles standard à la place.
:::

## Exemple : Bouton arrondi

L'exemple suivant montre un simple thème `Button` qui affiche un bouton avec un arrière-plan elliptique avec une esthétique Geocities des années 90 :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App">
  <Application.Styles>
    <FluentTheme />
  </Application.Styles>

  <Application.Resources>
    // highlight-start
    <ControlTheme x:Key="EllipseButton" TargetType="Button">
      <Setter Property="Background" Value="Blue"/>
      <Setter Property="Foreground" Value="Yellow"/>
      <Setter Property="Padding" Value="8"/>
      <Setter Property="Template">
        <ControlTemplate>
          <Panel>
            <Ellipse Fill="{TemplateBinding Background}"
                     HorizontalAlignment="Stretch"
                     VerticalAlignment="Stretch"/>
            <ContentPresenter x:Name="PART_ContentPresenter"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}"/>
          </Panel>
        </ControlTemplate>
      </Setter>
    </ControlTheme>
    // highlight-end
  </Application.Resources>
</Application>
```

```xml title='MainWindow.xaml'
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x='http://schemas.microsoft.com/winfx/2006/xaml'
        x:Class="Sandbox.MainWindow">
  // highlight-start
  <Button Theme="{StaticResource EllipseButton}"
          HorizontalAlignment="Center"
          VerticalAlignment="Center">
    Hello World!
  </Button>
  // highlight-end
</Window>
```

<p><img className="medium-image-zoom" src={StylingEllipseButtonScreenshot} alt="Bouton elliptique" /></p>

## Interaction dans les thèmes de contrôle

Comme les styles standard, les thèmes de contrôle prennent en charge les [styles imbriqués](../styling/styles.md#nesting-styles) qui peuvent être utilisés pour ajouter des interactions telles que les états de survol et d'appui.

## Exemple : État de survol du bouton arrondi

En utilisant des styles imbriqués, nous pouvons faire changer de couleur notre bouton lorsque le pointeur est survolé :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App">
  <Application.Styles>
    <FluentTheme />
  </Application.Styles>

  <Application.Resources>
    <ControlTheme x:Key="EllipseButton" TargetType="Button">
      <Setter Property="Background" Value="Blue"/>
      <Setter Property="Foreground" Value="Yellow"/>
      <Setter Property="Padding" Value="8"/>
      <Setter Property="Template">
        <ControlTemplate>
          <Panel>
            <Ellipse Fill="{TemplateBinding Background}"
                     HorizontalAlignment="Stretch"
                     VerticalAlignment="Stretch"/>
            <ContentPresenter x:Name="PART_ContentPresenter"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}"/>
          </Panel>
        </ControlTemplate>
      </Setter>
      
      // highlight-start
      <Style Selector="^:pointerover">
        <Setter Property="Background" Value="Red"/>
        <Setter Property="Foreground" Value="White"/>
      </Style>
      // highlight-end
    </ControlTheme>
  </Application.Resources>
</Application>
```

## Recherche de thème de contrôle

Il y a deux façons dont un thème de contrôle peut être trouvé :

- Si la propriété `Theme` du contrôle est définie, alors ce thème de contrôle sera utilisé ; sinon
- Avalonia recherchera vers le haut dans l'arborescence logique une ressource `ControlTheme` avec un `x:Key` qui correspond à la [clé de style](styles#style-key) du contrôle

:::tip
Si vous avez du mal à faire trouver votre thème par Avalonia, assurez-vous qu'il renvoie une [clé de style](styles#style-key) qui correspond au `x:Key` et au `TargetType` de votre thème de contrôle
:::

En effet, cela signifie que vous avez deux choix pour définir votre thème de contrôle :

- **Si vous voulez que le thème de contrôle s'applique à toutes les instances du contrôle**, utilisez un `{x:Type}` comme clé de ressource. Par exemple :
`<ControlTheme x:Key="{x:Type Button}" TargetType="Button">`
- **Si vous voulez que le thème de contrôle soit appliqué à des instances sélectionnées du contrôle**, utilisez n'importe quoi d'autre comme clé de ressource et recherchez cette ressource à l'aide de `{StaticResource}`. Généralement, cette clé sera une `string`.

:::info
Notez que cela signifie qu'un seul thème de contrôle peut être appliqué à un contrôle à un moment donné.
:::

## Exemple : Rendre tous les boutons arrondis

Nous pouvons appliquer notre nouveau thème de contrôle à tous les boutons de l'application en modifiant simplement la `x:Key` du thème de contrôle pour qu'elle corresponde au type `Button`.

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App">
  <Application.Styles>
    <FluentTheme />
  </Application.Styles>

  <Application.Resources>
      // highlight-next-line
    <ControlTheme x:Key="{x:Type Button}" TargetType="Button">
      <Setter Property="Background" Value="Blue"/>
      <Setter Property="Foreground" Value="Yellow"/>
      <Setter Property="Padding" Value="8"/>
      <Setter Property="Template">
        <ControlTemplate>
          <Panel>
            <Ellipse Fill="{TemplateBinding Background}"
                     HorizontalAlignment="Stretch"
                     VerticalAlignment="Stretch"/>
            <ContentPresenter x:Name="PART_ContentPresenter"
                              Content="{TemplateBinding Content}"
                              Margin="{TemplateBinding Padding}"/>
          </Panel>
        </ControlTemplate>
      </Setter>
      
      <Style Selector="^:pointerover">
        <Setter Property="Background" Value="Red"/>
        <Setter Property="Foreground" Value="White"/>
      </Style>
    </ControlTheme>
  </Application.Resources>
</Application>
```

### TargetType

La propriété `ControlTheme.TargetType` spécifie le type auquel s'appliquent les propriétés de réglage. Si vous ne spécifiez pas de `TargetType`, vous devez qualifier les propriétés de vos objets Setter avec un nom de classe en utilisant la syntaxe `Property="ClassName.Property"`. Par exemple, au lieu de définir Property="FontSize", vous devez définir Property sur `TextBlock.FontSize` ou `Control.FontSize`.

## Ressources supplémentaires

- L'exemple [ButtonCustomize](https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/ButtonCustomize) a un `WinClassicButtonTheme`
- Vous pouvez voir les thèmes de contrôle pour les contrôles Avalonia intégrés ici :
  - [Simple Theme](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Themes.Simple/Controls)
  - [Fluent Theme](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Themes.Fluent/Controls)
