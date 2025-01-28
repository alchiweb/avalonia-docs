---
id: focus
title: Focus
---

import DirectionalNavigationScreenshot from '/img/concepts/input/directional-navigation.gif';

# Focus

Le focus fait référence à l'`InputElement` qui est censé recevoir des entrées au clavier et qui est généralement distingué par un indicateur visuel. L'exemple le plus familier est un `TextBox` avec un curseur clignotant à l'intérieur, mais des contrôles non textuels comme `Button` et `Slider` participent également au focus.

## IsFocused et Focusable

`IsFocused` est une propriété en lecture seule qui suit l'état de focus de l'`InputElement`.

La propriété `Focusable` active ou désactive la capacité de focaliser un `InputElement`. Les éléments qui ne peuvent pas être focalisés peuvent toujours être interagis via un pointeur, donc il convient de s'assurer qu'un équivalent fonctionnel au clavier (comme des raccourcis) est disponible lorsque c'est possible.

## Focalisation explicite

Pour attribuer explicitement le focus à un `InputElement`, appelez sa méthode `.Focus()` depuis le code. En option, vous pouvez spécifier la `NavigationMethod` et les `KeyModifiers` pour simuler comme si le focus était déclenché par un flux de navigation de focus spécifique. La focalisation explicite est souvent utilisée pour focaliser un `InputElement` spécifique dans un formulaire de saisie de données lors du chargement ou pour passer programmétiquement au `InputControl` suivant une fois que l'entrée actuelle a été satisfaite.

| NavigationMethod | Description du déclencheur   |
|:-----------------|:-----------------------------|
| Tab              | Appui sur la touche Tab      |
| Pointer          | Interaction par pointeur     |
| Directional      | Directionnel 2D (`XYFocus`)  |
| Unspecified      | Par défaut                   |

## Événements de Focus

Les `InputElement`s exposent les événements `GotFocus` et `LostFocus`. Les `GotFocusEventArgs` contiennent le `NavigationMethod` et les `KeyModifiers` utilisés pour déclencher la navigation de focus.

## Pseudoclasses de Focus

Ces pseudoclasses sont utiles lors du stylage des `Control`s qui sont `Focusable`.

| Pseudoclasse    | Description                                                      |
|:----------------|------------------------------------------------------------------|
| :focus          | Le Contrôle a le focus.                                          |
| :focus-within   | Le Contrôle a le focus ou contient un descendant qui a le focus. |
| :focus-visible  | Le Contrôle a le focus et doit montrer un indicateur visuel.     |

:::tip
La propriété `FocusAdorner` est utilisée pour montrer un visuel de focus par défaut, généralement une `Border`, autour d'un `Control` avec `:focus-visible`. Lors de l'utilisation de `:focus-visible` pour montrer un indicateur visuel personnalisé, définir `FocusAdorner` à `null` évitera d'afficher un indicateur en double.
:::

## Gestionnaire de Focus

Le `FocusManager` fournit un accès global à la fonctionnalité de focus, comme la récupération de l'élément actuellement focalisé ou le nettoyage du focus. Pour plus d'informations, consultez la [documentation du FocusManager](../services/focus-manager).

## Navigation de Focus par Tabulation

La navigation par focus au moyen de la touche tabulation se produit lorsque l'utilisateur appuie sur la touche tab de son clavier. Les `InputElement`s dont la propriété `IsTabStop` est définie sur `true` seront disponibles pour la navigation par focus avec la touche tab. Le `TabIndex` spécifie la priorité, les valeurs numériques plus basses étant naviguées en premier. Lorsque le `TabIndex` de plusieurs contrôles est égal, la priorité est basée sur un ordre de parcours de l'arbre visuel.

La propriété attachée `KeyboardNavigation.TabNavigation` peut définir un `KeyboardNavigationMode` sur tout `InputElement` agissant comme un conteneur et modifier ses caractéristiques de navigation par tabulation.

| KeyboardNavigationMode | Parcours des éléments du conteneur                                                |
|:-----------------------|:----------------------------------------------------------------------------------|
| Continue               | Continue au-delà des éléments et dans le conteneur suivant                        |
| Cycle                  | Parcourt et revient à ses propres éléments                                        |
| Contained              | S'arrête au début/à la fin de l'élément                                           |
| Once                   | Le conteneur et les enfants reçoivent le focus une seule fois en tant que groupe  |
| None                   | Les éléments ne seront pas mis au focus par la navigation par tabulation          |
| Local                  | Le `TabIndex` est considéré uniquement pour les éléments dans le sous-arbre local |

## Navigation par focus directionnel <MinVersion version="11.1" />

La navigation par focus à travers `XYFocus` est un schéma directionnel 2D permettant une navigation spatiale depuis le contrôle focalisé vers un autre contrôle dans une direction cardinale : gauche, droite, haut ou bas. Par défaut, `XYFocus.NavigationModes` est réglé pour permettre la navigation `Gamepad` et `Remote`.

| KeyDeviceType | Dispositif                                              |
|:--------------|:--------------------------------------------------------|
| Disabled      | La navigation XY par tout dispositif clé est désactivée.|
| Keyboard      | Les touches fléchées du clavier peuvent être utilisées. |
| Gamepad       | La manette de jeu DPad peut être utilisée.              |
| Remote        | La télécommande peut être utilisée.                     |
| Enabled       | Tous les dispositifs peuvent être utilisés.             |

Les entrées de manette sont prises en charge sur les dispositifs qui peuvent nativement envoyer ces entrées, comme Android et Tizen. Cependant, Avalonia manque actuellement des API de manette multiplateformes nécessaires pour un large soutien prêt à l'emploi.

### Stratégie de Navigation

Lorsque la navigation directionnelle 2D est activée, une stratégie de désambiguïsation est utilisée pour sélectionner la cible de navigation.

| XYFocusNavigationStrategy   | Cible de Navigation                                                                          |
|:----------------------------|:---------------------------------------------------------------------------------------------|
| Auto                        | Hérite de la stratégie de l'ancêtre. Projection si aucun ancêtre ne spécifie.                |
| Projection                  | Premier élément rencontré lors de la projection d'une ligne dans la direction de navigation. |
| NavigationDirectionDistance | Élément le plus proche de l'axe de la ligne de navigation.                                   |
| RectilinearDistance         | Élément le plus proche basé sur la plus courte distance de Manhattan.                        |

### Navigation Explicite

`XYFocus` permet à chaque contrôle de spécifier une cible de navigation explicite lorsqu'une direction est pressée via `XYFocus.Up`, 
`XYFocus.Down`, `XYFocus.Left`, et `XYFocus.Right`. Cela a la priorité sur toute stratégie de navigation.

:::warning
L'engagement du focus n'est pas encore implémenté, donc la combinaison de la navigation par focus directionnel avec des contrôles qui gèrent également 
l'entrée directionnelle eux-mêmes peut avoir certaines limitations, en particulier avec les visuels.
:::

### Exemple

Ce qui suit démontre comment utiliser la navigation par focus directionnel dans un `WrapPanel`. Cela permet explicitement la navigation pour 
se déplacer du premier au dernier élément et vice-versa.

Le `Slider` fournit un exemple de mélange de navigation avec l'interaction du contrôle. Sur Desktop, appuyer sur la touche Entrée pendant que 
le `Slider` est en focus commencera une interaction où l'utilisateur modifiera la `Slider.Value` au lieu de provoquer 
une navigation. Appuyer sur Entrée une seconde fois mettra fin à l'interaction et reprendra la navigation par focus directionnel.

```xml
<Window
    XYFocus.NavigationModes="Enabled"
    XYFocus.UpNavigationStrategy="Projection"
    XYFocus.DownNavigationStrategy="Projection"
    XYFocus.LeftNavigationStrategy="Projection"
    XYFocus.RightNavigationStrategy="Projection">
    <Grid>
        <WrapPanel>
            <Button x:Name="premier"
                Content="Premier"
                XYFocus.Left="{Binding #dernier}" />
            <Button Content="Deuxième" />
            <Button Content="Troisième" />
    
            <Slider Width="100" Maximum="100" />
    
            <Button Content="Quatrième" />
            <Button x:Name="dernier"
                Content="Dernier"
                XYFocus.Right="{Binding #premier}" />
        </WrapPanel>
    </Grid>
</Window>
```

<img src={DirectionalNavigationScreenshot} alt="Directional Navigation Example"/>
