---
id: styles
title: Styles
---

import StyleH1SampleScreenshot from '/img/basics/user-interface/styling/style-h1.png';

# Styles

Le système de style _Avalonia UI_ est un mécanisme qui peut partager les paramètres de propriété entre les contrôles.

:::tip
Un `Style` dans Avalonia est plus similaire à un style CSS qu'à un style WPF/UWP. L'équivalent d'un Style WPF/UWP dans Avalonia est un [`ControlTheme`](control-themes).
:::

## Comment ça fonctionne

En essence, le mécanisme de style se compose de deux étapes : sélection et substitution. Le XAML pour le style peut définir comment ces deux étapes doivent être effectuées, mais souvent vous aiderez l'étape de sélection en définissant des étiquettes de 'classe' sur les éléments de contrôle.

:::info
L'utilisation d'étiquettes de 'classe' sur les éléments de contrôle par le système de style _Avalonia UI_ est analogue à la façon dont CSS (feuilles de style en cascade) fonctionne avec les éléments HTML.
:::

Le système de style implémente des styles en cascade en recherchant l'[arbre logique](../../../concepts/control-trees.md) vers le haut à partir d'un contrôle, pendant l'étape de sélection. Cela signifie que les styles définis au niveau le plus élevé de l'application (le fichier `App.axaml`) peuvent être utilisés n'importe où dans une application, mais peuvent encore être remplacés plus près d'un contrôle (par exemple dans une fenêtre ou un contrôle utilisateur).

Lorsqu'une correspondance est trouvée par l'étape de sélection, les propriétés du contrôle correspondant sont modifiées selon les paramètres dans le style.

## Comment c'est écrit

Le XAML pour un style a deux parties : un attribut de sélecteur et un ou plusieurs éléments de définition. La valeur du sélecteur contient une chaîne qui utilise la **syntaxe de sélecteur de style** _Avalonia UI_. Chaque élément de définition identifie la propriété qui sera modifiée par son nom, et la nouvelle valeur qui sera substituée. Le modèle est comme ceci :

```
<Style Selector="selector syntax">
     <Setter Property="property name" Value="new value"/>
     ...
</Style>
```

:::info
La **syntaxe de sélecteur de style** _Avalonia UI_ est analogue à celle utilisée par le CSS (feuilles de style en cascade). Pour des informations de référence détaillées, voir [ici](../../../reference/styles/style-selector-syntax.md).
:::

## Exemple

Voici un exemple de la façon dont un style est écrit et appliqué à un élément de contrôle, avec une [classe de style](style-classes) pour aider à la sélection :

```xml
<Window ... >
    <Window.Styles>
        <Style Selector="TextBlock.h1">
            <Setter Property="FontSize" Value="24"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>
    </Window.Styles>
    <StackPanel Margin="20">
       <TextBlock Classes="h1">Titre 1</TextBlock>
    </StackPanel>
</Window>
```

Dans cet exemple, tous les éléments `TextBlock` avec la classe de style `h1` seront affichés avec la taille de police et le poids définis par le style. Cela fonctionne dans le panneau de prévisualisation :

<img src={StyleH1SampleScreenshot} alt=""/>

## Où placer les styles

Les styles sont placés à l'intérieur d'un élément de collection `Styles` sur un `Control` ou sur l'`Application`. Par exemple, une collection de styles de fenêtre ressemble à ceci :

```xml
<Window.Styles>
   <Style> ... </Style>
</Window.Styles>
```

L'emplacement d'une collection de styles définit la portée des styles qu'elle contient. Dans l'exemple ci-dessus, les styles s'appliqueront à la fenêtre et à tout son contenu. Si un style est ajouté à l'`Application`, il s'appliquera globalement.

## Le sélecteur

Le sélecteur de style définit sur quels contrôles le style agira. Le sélecteur utilise une variété de formats, l'un des plus simples est celui-ci :

```xml
<Style Selector="TargetControlClass.styleClassName">
```

Ce sélecteur correspondra à tous les contrôles ayant une clé de style de `TargetControlClass`, avec une classe de style de `styleClassName`.

:::info
Une liste complète des sélecteurs peut être trouvée [ici](../../../reference/styles/style-selector-syntax.md).
:::

## Setters

Les setters décrivent ce qui se passera lorsque le sélecteur correspond à un contrôle. Ce sont des paires propriété/valeur simples écrites dans le format :

```xml
<Setter Property="FontSize" Value="24"/>
<Setter Property="Padding" Value="4 2 0 4"/>
```

Chaque fois qu'un style correspond à un contrôle, tous les setters dans le style seront appliqués au contrôle.

:::info
Pour plus d'informations sur les setters, voir [ici](../../../guides/styles-and-resources/property-setters.md).
:::

## Styles imbriqués

Les styles peuvent être imbriqués dans d'autres styles. Pour imbriquer un style, il suffit d'inclure le style enfant comme enfant de l'élément `<Style>` parent, et de commencer le sélecteur avec le [`Nesting Selector (^)`](../../../reference/styles/style-selector-syntax.md#nesting):

```xml
<Style Selector="TextBlock.h1">
    <Setter Property="FontSize" Value="24"/>
    <Setter Property="FontWeight" Value="Bold"/>
    
    // highlight-start
    <Style Selector="^:pointerover">
        <Setter Property="Foreground" Value="Red"/>
    </Style>
    // highlight-end
</Style>
```

Lorsque cela se produit, le sélecteur du style parent s'appliquera automatiquement au style enfant. Dans l'exemple ci-dessus, le style imbriqué aura effectivement un sélecteur de `TextBlock.h1:pointerover`, ce qui signifie qu'il s'affichera avec un premier plan rouge lorsque le pointeur est sur le contrôle.

:::info
Le sélecteur imbriqué doit être présent et doit apparaître au début du sélecteur enfant.
:::

## Clé de style

Le type d'un objet correspondant à un sélecteur de style n'est pas déterminé par le type concret du contrôle, mais plutôt en examinant sa propriété `StyleKey`.

Par défaut, la propriété `StyleKey` renvoie le type de l'instance actuelle. Cependant, si vous souhaitez que votre contrôle, qui hérite de Button, soit stylé comme un Button, vous pouvez remplacer la propriété `StyleKeyOverride` dans votre classe et la faire renvoyer `typeof(Button)`.

```csharp
public class MyButton : Button
{
    // `MyButton` sera stylé comme un contrôle `Button` standard.
    protected override Type StyleKeyOverride => typeof(Button);
}
```

:::info
Notez que cette logique est inversée par rapport à WPF/UWP : dans ces frameworks, lorsque vous dérivez un nouveau contrôle, il sera stylé comme son contrôle de base à moins que vous ne remplaciez la propriété `DefaultStyleKey`. Dans Avalonia, le contrôle sera stylé en utilisant son type concret à moins qu'une clé de style différente ne soit fournie.
:::

:::info
Avant Avalonia 11, la clé de style était remplacée en implémentant `IStyleable` et en fournissant une nouvelle implémentation de la propriété `IStyleable.StyleKey`. Ce mécanisme est toujours pris en charge dans Avalonia 11 pour des raisons de compatibilité, mais pourrait être supprimé dans une version future.
:::

## Styles et Ressources

Les ressources sont souvent utilisées avec des styles pour aider à maintenir une présentation cohérente. Les ressources peuvent aider à définir des couleurs et des icônes standard dans une application ; ou à travers plusieurs applications lorsqu'elles sont incluses à partir de fichiers séparés.

:::info
Pour des conseils sur la façon d'utiliser des ressources dans votre application, voir [ici](../../../guides/styles-and-resources/resources.md).
:::

## Informations Complémentaires

:::info
Pour des conseils sur la façon de partager des styles en incluant un fichier de styles, voir [ici](../../../guides/styles-and-resources/how-to-use-included-styles.md).
:::