---
id: keyframe-animations
title: Comment Utiliser les Animations par Images Clés (Keyframe Animations)
---

import AnimationKeyframeDiagram from '/img/basics/user-interface/animation-keyframe.png';
import KeyframeFadeScreenshot from '/img/guides/graphics-and-animations/keyframe-fade.gif';
import KeyframeCompositeAnimationScreenshot from '/img/guides/graphics-and-animations/keyframe-composite-animation.gif';
import LinearEasingScreenshot from '/img/guides/graphics-and-animations/linear-easing.gif';
import BounceEaseInScreenshot from '/img/guides/graphics-and-animations/bounce-ease-in.gif';

# Comment Utiliser les Animations par Images Clés (Keyframe Animations)

Vous pouvez utiliser une animation par images clés pour changer une ou plusieurs propriétés de contrôle suivant une chronologie. Les images clés sont définies dans les styles de _Avalonia UI_ avec des points de **repère** le long de la **durée** de l'animation, et définissent les valeurs intermédiaires des propriétés à un moment donné.

<img src={AnimationKeyframeDiagram} alt=""/>

Les valeurs de propriété entre les images clés sont définies suivant le profil d'une **fonction d'assouplissement**. La fonction d'assouplissement par défaut est une interpolation linéaire.

L'animation est déclenchée pour commencer, puis peut s'exécuter un nombre quelconque de fois, dans les deux sens. Il existe également des options pour retarder le début de l'animation et pour la répéter.

:::info
Si vous êtes familier avec les animations par images clés en CSS, vous reconnaîtrez la similarité avec la façon dont elles sont réalisées dans _Avalonia UI_.
:::

## Exemple

Vous définissez une animation par images clés en utilisant des styles.

:::info
Pour revoir comment _Avalonia UI_ utilise les styles, consultez le concept [ici](../../basics/user-interface/styling).
:::

Suivez cette procédure pour définir une simple animation de fondu de couleur en utilisant XAML :

-  Créez une collection de styles à votre niveau choisi.
-  Ajoutez un style à la collection avec un sélecteur qui peut cibler le contrôle que vous souhaitez animer.
-  Ajoutez un élément `Setter` pour définir la propriété que vous voulez que l'animation change. Dans cet exemple `<Setter Property="Fill" Value="Red"/>`
-  Ajoutez un élément `Style.Animations` pour contenir votre animation.
-  Ajoutez un élément `Animation` et définissez son attribut `Duration`. Cela est au format `"Heures:Minutes:Secondes"`.
-  Maintenant, définissez les images clés pour l'animation. Cet exemple utilise des repères à 0 % et 100 %.
-  Ajoutez des éléments `Setter` à chaque image clé pour la valeur de l'opacité de remplissage. Cet exemple anime entre les valeurs d'opacité de 0,0 et 1,0.

Le code fini ressemblera à ceci :

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Styles>
        <Style Selector="Rectangle.red">
            <Setter Property="Fill" Value="Red"/>
            <Style.Animations>
                <Animation Duration="0:0:3"> 
                    <KeyFrame Cue="0%">
                        <Setter Property="Opacity" Value="0.0"/>
                    </KeyFrame>
                    <KeyFrame Cue="100%">
                        <Setter Property="Opacity" Value="1.0"/>
                    </KeyFrame>
                </Animation>
            </Style.Animations>
        </Style>
    </Window.Styles>

    <Rectangle Classes="red" Width="100" Height="100"/>
</Window>
```

L'animation résultante ressemble à ceci :

<img src={KeyframeFadeScreenshot} alt=""/>

L'animation se déclenche dès que le contrôle rectangle est chargé et peut être sélectionnée par le style. En fait, elle fonctionne également dans le panneau de prévisualisation !

## Animer Deux Propriétés

Cet exemple vous montre comment animer deux propriétés sur la même chronologie.

```xml
<Window.Styles>
    <Style Selector="Rectangle.red">
      <Setter Property="Fill" Value="Red"/>
      <Style.Animations>
        <Animation Duration="0:0:3" IterationCount="4">
          <KeyFrame Cue="0%">
            <Setter Property="Opacity" Value="0.0"/>
            <Setter Property="RotateTransform.Angle" Value="0.0"/>
          </KeyFrame>
          <KeyFrame Cue="100%"> 
            <Setter Property="Opacity" Value="1.0"/>
            <Setter Property="RotateTransform.Angle" Value="90.0"/>
          </KeyFrame>
        </Animation> 
    </Style.Animations>
    </Style>
  </Window.Styles>
```

Le rectangle rouge est estompé et tourné en même temps.

<img src={KeyframeCompositeAnimationScreenshot} alt=""/>

## Configurer l'animation

### Délai

Vous pouvez ajouter un délai au début d'une animation en définissant l'attribut de délai de l'élément d'animation. Par exemple :

```xml
<Animation Duration="0:0:1"
           Delay="0:0:1"> 
    ...
</Animation>
```

### Répéter

Vous pouvez faire en sorte qu'une animation se répète un nombre défini de fois, ou indéfiniment. Pour répéter un nombre fini d'itérations, définissez l'attribut `IterationCount` sur l'élément d'animation comme ceci :

```xml
<Animation IterationCount="5">
    ...
</Animation>
```

Pour répéter une animation indéfiniment, utilisez la valeur spéciale `"INFINITE"`. Par exemple :

```xml
<Animation IterationCount="INFINITE">
    ...
</Animation>
```

### Direction de Lecture

Par défaut, une animation se joue en avant. C'est-à-dire qu'elle suit le profil de la fonction d'assouplissement de gauche à droite. Vous pouvez modifier ce comportement en définissant l'attribut `PlaybackDirection` sur l'élément d'animation. Par exemple :

```xml
<Animation IterationCount="9" PlaybackDirection="AlternateReverse">
    ...
</Animation>
```

Le tableau suivant décrit les options :

<table><thead><tr><th width="245">Valeur</th><th>Description</th></tr></thead><tbody><tr><td><code>Normal</code></td><td>(Par défaut) L'animation est jouée en avant.</td></tr><tr><td><code>Reverse</code></td><td>L'animation est jouée en direction inverse.</td></tr><tr><td><code>Alternate</code></td><td>L'animation est jouée d'abord en avant, puis en arrière.</td></tr><tr><td><code>AlternateReverse</code></td><td>L'animation est jouée d'abord en arrière, puis en avant.</td></tr></tbody></table>

### Mode de Remplissage

L'attribut de mode de remplissage d'une animation définit comment les propriétés définies persisteront après son exécution, ou pendant les intervalles entre les exécutions. Par exemple :

```xml
<Animation IterationCount="9" FillMode="Backward">
    ...
</Animation>
```

Le tableau suivant décrit les options :

<table><thead><tr><th width="240">Valeur</th><th>Description</th></tr></thead><tbody><tr><td><code>None</code></td><td>La valeur ne persistera pas après l'animation et la première valeur ne sera pas appliquée lorsque l'animation est retardée.</td></tr><tr><td><code>Forward</code></td><td>La dernière valeur interpolée sera persistée dans la propriété cible.</td></tr><tr><td><code>Backward</code></td><td>La première valeur interpolée sera affichée lors du retard d'animation.</td></tr><tr><td><code>Both</code></td><td>Les comportements <code>Forward</code> et <code>Backward</code> seront appliqués.</td></tr></tbody></table>

### Fonction d'Assouplissement

Une fonction d'assouplissement définit comment une propriété varie dans le temps pendant une animation.

<div>

<img src={LinearEasingScreenshot} alt=""/>

<img src={BounceEaseInScreenshot} alt=""/>

</div>

La fonction d'assouplissement par défaut est linéaire (ci-dessus à gauche), mais vous pouvez utiliser un autre modèle en définissant le nom de la fonction souhaitée dans l'attribut d'assouplissement. Par exemple, pour utiliser la fonction 'bounce ease in' (ci-dessus à droite) :

```xml
<Animation Duration="0:0:1"
           Delay="0:0:1"
           Easing="BounceEaseIn"> 
    ...
</Animation>
```

:::info
Pour une liste complète des fonctions d'assouplissement _Avalonia UI_, consultez la référence [ici](../../reference/animation-settings.md).
:::

Vous pouvez également ajouter votre propre classe de fonction d'assouplissement personnalisée comme ceci :

```xml
<Animation Duration="0:0:1"
           Delay="0:0:1">
    <Animation.Easing>
        <local:YourCustomEasingClassHere/>
    </Animation.Easing> 
    ...
</Animation>
```

## Exécution de l'animation depuis le code derrière

Dans certaines situations, les développeurs ont besoin de plus de flexibilité avec la durée de vie de l'animation, par rapport aux sélecteurs de style XAML. Le plus simple serait de définir l'animation dans le dictionnaire `Resources`.

Lors de la définition de l'`Animation` de cette manière, il est important de spécifier à la fois `x:Key` et `x:SetterTargetType`. Le premier sera utilisé pour accéder à l'animation par la clé, et le second aide le compilateur à créer des setters fortement typés.

```xml
<Window xmlns="https://github.com/avaloniaui">
    <Window.Resources>
        <Animation x:Key="ResourceAnimation"
                   x:SetterTargetType="Rectangle"
                   Duration="0:0:3"> 
            <KeyFrame Cue="0%">
                <Setter Property="Opacity" Value="0.0"/>
            </KeyFrame>
            <KeyFrame Cue="100%">
                <Setter Property="Opacity" Value="1.0"/>
            </KeyFrame>
        </Animation>
    </Window.Resources>

    <Rectangle x:Name="Rect" />
</Window>
```

Maintenant, cette animation peut être accessible et exécutée dans un gestionnaire de code derrière personnalisé.

```csharp
var animation = (Animation)this.Resources["ResourceAnimation"];
// Exécution de l'animation XAML sur le contrôle Rect.
await animation.RunAsync(Rect);
```

`RunAsync` renvoie une tâche qui est complétée avec l'animation. Si l'animation est infinie/répétitive, la tâche ne se terminera jamais, sauf si elle est annulée de manière externe en passant un `CancellationToken` à la méthode RunAsync.

:::info
Bien qu'il soit plus facile de définir des animations en XAML, il est également possible de le faire complètement en code C#. Il est possible de créer une instance de type `Animation` et de remplir la collection de cadres clés.
:::
