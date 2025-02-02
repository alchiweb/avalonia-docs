---
id: troubleshooting
title: Comment réparer les Styles
---


# 👉 Comment réparer les Styles

Une grande partie du système de style _Avalonia UI_ correspond aux approches de style CSS. Donc, sans connaissance de cette technologie, vous trouverez peut-être les conseils ici utiles.

## Le Sélecteur n'a Pas de Cibles

Un sélecteur _Avalonia UI_, comme un sélecteur CSS, ne génère pas d'erreur ou d'avertissement lorsqu'il n'y a pas de contrôles pouvant être correspondus. Le style échouera silencieusement à s'afficher.

:::info
Vérifiez si vous avez utilisé un nom ou une classe qui n'existe pas.
:::

:::info
Vérifiez si vous avez utilisé un sélecteur enfant là où il n'y a pas d'enfants à correspondre.
:::

## Séquence de Fichiers Inclus

Les styles sont appliqués dans l'ordre de déclaration. S'il y a plusieurs fichiers de style inclus qui ciblent la même propriété de contrôle, le dernier style inclus remplacera les précédents. Par exemple :

```xml
<Style Selector="TextBlock.header">
    <Style Property="Foreground" Value="Green" />
</Style>
```

```xml
<Style Selector="TextBlock.header">
    <Style Property="Foreground" Value="Blue" />
    <Style Property="FontSize" Value="16" />
</Style>
```

```xml
<StyleInclude Source="Style1.axaml" />
<StyleInclude Source="Style2.axaml" />
```

Ici, les styles du fichier **Styles1.axaml** ont été appliqués en premier, donc les setters dans les styles du fichier **Styles2.axaml** ont la priorité. Le TextBlock résultant aura FontSize="16" et Foreground="Green". Le même ordre de priorité s'applique également au sein des fichiers de style.

## Les Propriétés Définies Localement Ont la Priorité

Une valeur locale définie directement sur un contrôle a souvent une priorité plus élevée que toute valeur de style. Donc, dans cet exemple, le bloc de texte aura un premier plan rouge :

```xml
<Style Selector="TextBlock.header">
    <Setter Property="Foreground" Value="Green" />
</Style>
...
<TextBlock Classes="header" Foreground="Red" />
```

Vous pouvez voir la liste complète des priorités de valeur dans l'énumération `BindingPriority`, où les valeurs d'énumération inférieures ont une priorité plus élevée.

<table><thead><tr><th width="218">BindingPriority </th><th width="147.33333333333331">Value</th><th>Commentaire</th></tr></thead><tbody><tr><td><code>Animation</code></td><td>-1</td><td>La plus haute priorité - même au détriment d'une valeur locale.</td></tr><tr><td><code>LocalValue</code></td><td>0</td><td>Une valeur locale est définie sur la propriété du contrôle.</td></tr><tr><td><code>StyleTrigger</code></td><td>1</td><td>Ceci est déclenché lorsqu'une pseudo-classe devient active.</td></tr><tr><td><code>TemplatedParent</code></td><td>2</td><td></td></tr><tr><td><code>Style</code></td><td>3</td><td></td></tr><tr><td><code>Unset</code></td><td>2147483647</td><td></td></tr></tbody></table>

:::warning
L'exception est que les valeurs `Animation` ont la priorité la plus élevée et peuvent même remplacer les valeurs locales.
:::

:::info
Certains styles par défaut _Avalonia UI_ utilisent des valeurs locales dans leurs modèles au lieu de liaisons de modèle ou de définitions de style. Cela rend impossible la mise à jour de la propriété de modèle sans remplacer l'ensemble du modèle.
:::

### Sélecteur de pseudo-classe de style manquant (déclencheur)

Imaginons une situation dans laquelle vous pourriez vous attendre à ce qu'un deuxième style remplace le précédent, mais ce n'est pas le cas : 

```xml
<Style Selector="Border:pointerover">
    <Setter Property="Background" Value="Blue" />
</Style>
<Style Selector="Border">
    <Setter Property="Background" Value="Red" />
</Style>
...
<Border Width="100" Height="100" Margin="100" />
```

Avec cet exemple de code, le `Border` a un fond rouge normalement et bleu lorsque le pointeur est dessus. Cela s'explique par le fait qu'en CSS, des sélecteurs plus spécifiques ont la priorité. C'est un problème lorsque vous souhaitez remplacer les styles par défaut de tout état (pointeur survolé, pressé ou autres) par un seul style. Pour y parvenir, vous devrez également avoir de nouveaux styles pour ces états. 

:::info
Visitez le code source d'Avalonia pour trouver les [modèles originaux](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Themes.Fluent/Controls) lorsque cela se produit et copiez-collez les styles avec des pseudoclasses dans votre code.
:::

### Un sélecteur avec une pseudoclasse ne remplace pas le style par défaut

L'exemple de code suivant de styles qui peuvent être attendus pour fonctionner par-dessus les styles par défaut :

```xml
<Style Selector="Button">
    <Setter Property="Background" Value="Red" />
</Style>
<Style Selector="Button:pointerover">
    <Setter Property="Background" Value="Blue" />
</Style>
```

Vous pourriez vous attendre à ce que le `Button` soit rouge par défaut et bleu lorsque le pointeur est dessus. En fait, seul le setter du premier style sera appliqué, et le second sera ignoré.

La raison est cachée dans le modèle du bouton. Vous pouvez trouver les modèles par défaut dans le code source d'Avalonia (ancien thème [par défaut](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Themes.Default/Button.xaml) et nouveau thème [Fluent](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Themes.Fluent/Controls/Button.xaml)), mais pour plus de commodité, nous avons simplifié un modèle du thème Fluent :

```xml
<Style Selector="Button">
    <Setter Property="Background" Value="{DynamicResource ButtonBackground}"/>
    <Setter Property="Template">
        <ControlTemplate>
            <ContentPresenter Name="PART_ContentPresenter"
                              Background="{TemplateBinding Background}"
                              Content="{TemplateBinding Content}"/>
        </ControlTemplate>
    </Setter>
</Style>
<Style Selector="Button:pointerover /template/ ContentPresenter#PART_ContentPresenter">
    <Setter Property="Background" Value="{DynamicResource ButtonBackgroundPointerOver}" />
</Style>
```

L'arrière-plan réel est rendu par un `ContentPresenter`, qui, par défaut, est lié à la propriété `Background` des boutons. Cependant, dans l'état de survol du pointeur, le sélecteur applique directement l'arrière-plan au `ContentPresenter (Button:pointerover /template/ ContentPresenter#PART_ContentPresenter)`. C'est pourquoi notre setter a été ignoré dans l'exemple de code précédent. Le code corrigé doit également cibler directement le présentateur de contenu :

```xml
<!-- Ici, le sélecteur #PART_ContentPresenter n'est pas nécessaire, mais a été ajouté pour avoir un style plus spécifique -->
<Style Selector="Button:pointerover /template/ ContentPresenter#PART_ContentPresenter">
    <Setter Property="Background" Value="Blue" />
</Style>
```

:::info
Vous pouvez observer ce comportement pour tous les contrôles dans les thèmes par défaut (à la fois l'ancien Default et le nouveau Fluent), pas seulement pour le bouton. Et pas seulement pour l'arrière-plan, mais aussi pour d'autres propriétés dépendantes de l'état.
:::

:::info
Pourquoi les styles par défaut changent-ils directement la propriété `Background` du ContentPresenter au lieu de changer la propriété `Button.Background` ?

C'est parce que si l'utilisateur devait définir une valeur locale sur le bouton, cela écraserait tous les styles et rendrait le bouton toujours de la même couleur. Pour plus de détails, consultez ce [PR annulé](https://github.com/AvaloniaUI/Avalonia/pull/2662#issuecomment-515764732).
:::

### La valeur précédente de propriétés spécifiques n'est pas restaurée lorsque le style n'est plus appliqué

Dans Avalonia, nous avons plusieurs types de propriétés, et l'une d'elles, la propriété directe, ne prend pas en charge le style du tout. Ces propriétés fonctionnent de manière simplifiée pour atteindre un coût réduit et une meilleure performance, et ne stockent pas plusieurs valeurs en fonction de la priorité. Au lieu de cela, seule la dernière valeur est enregistrée et ne peut pas être restaurée. Vous pouvez trouver plus de détails sur les propriétés [ici](../custom-controls/defining-properties).

Un exemple typique est [CommandProperty](http://reference.avaloniaui.net/api/Avalonia.Controls/Button/B9689B29). Elle est définie comme une propriété directe, et elle ne fonctionnera jamais correctement. À l'avenir, toute tentative de styliser une propriété directe entraînera une erreur de compilation, voir [#6837](https://github.com/AvaloniaUI/Avalonia/issues/6837).
