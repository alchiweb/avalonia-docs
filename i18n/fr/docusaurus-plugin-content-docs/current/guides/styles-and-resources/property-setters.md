---
id: property-setters
title: Les Setters pour les Propriétés
---

# Les Setters pour les Propriétés

Les setters dans un style définissent quelles propriétés seront modifiées après que _Avalonia UI_ ait trouvé le contrôle dans l'arbre de contrôle logique en utilisant le sélecteur, et déterminé quel style doit être utilisé. Les setters sont des paires simples d'attributs de propriété et de valeur dans le XAML, écrites dans le format :

```xml
<Setter Property="propertyName" Value="newValueString"/>
```

Par exemple :

```xml
<Setter Property="FontSize" Value="24"/>
<Setter Property="Padding" Value="4 2 0 4"/>
```

Vous pouvez également utiliser une syntaxe longue pour définir une propriété de contrôle à un objet avec plusieurs propriétés définies, comme ceci :

```xml
<Setter Property="MyProperty">
   <MyObject Property1="My Value" Property2="999"/>
</Setter>
```

Un style peut également définir des propriétés en utilisant des liaisons. Après le processus de sélection habituel, cela amène _Avalonia UI_ à utiliser une valeur du contexte de données du contrôle cible. Par exemple, le setter peut être défini comme ceci :

```xml
<Setter Property="FontSize" Value="{Binding SelectedFontSize}"/>
```

## Priorité de Style

Il existe deux règles qui régissent quelle propriété de style a la priorité lorsqu'un sélecteur correspond à plusieurs styles :

* Position de la collection de styles englobante dans l'application - 'le plus proche' a la priorité.
* Position du style dans la collection de styles localisée - 'le plus récent' a la priorité.

Par exemple, cela signifie tout d'abord que les styles définis au niveau de la fenêtre remplaceront ceux définis au niveau de l'application. Deuxièmement, cela signifie que lorsque les collections de styles sélectionnées sont au même niveau, alors la définition la plus récente (telle qu'écrite dans le fichier) a la priorité.

:::warning
Si vous compariez les classes de style au CSS, vous devez noter que : **contrairement au CSS**, la séquence de liste des noms de classes dans l'attribut `Classes` n'a aucun effet sur la priorité du setter dans _Avalonia UI_. C'est-à-dire que si ces deux classes de style définissent la couleur, alors quelle que soit la manière de lister les classes, le résultat est le même :

```xml
<Button Classes="h1 blue"/>
<Button Classes="blue h1"/>
```
:::

## Réversion de Valeur

Chaque fois qu'un style est associé à un contrôle, tous les setters seront appliqués au contrôle. Si un sélecteur de style fait en sorte que le style ne corresponde plus à un contrôle, la valeur de la propriété reviendra à sa valeur de priorité suivante la plus élevée.

## Valeurs Mutables

Notez que le `Setter` crée une seule instance de `Value` qui sera appliquée à tous les contrôles que le style correspond : si l'objet est mutable, alors les changements seront reflétés sur tous les contrôles.

Notez également que les liaisons sur un objet défini dans une valeur de setter n'auront pas accès au contexte de données du contrôle cible. C'est parce qu'il peut y avoir plusieurs contrôles cibles. Ce scénario peut se produire avec un style défini comme ceci :

```xml
<Style Selector="local|MyControl">
  <Setter Property="MyProperty">
     <MyObject Property1="{Binding MyViewModelProperty}"/>
  </Setter>
</Style>
```

Cela signifie que dans l'exemple ci-dessus, la source de liaison pour le setter sera `MyObject.DataContext`, et non `MyControl.DataContext`. De plus, si `MyObject` n'a pas de contexte de données, alors la liaison ne pourra pas produire de valeur.

Remarque : si vous utilisez des liaisons compilées, vous devez définir explicitement le type de données de la source de liaison dans l'élément `<Style>` :

```xml
<Style Selector="MyControl" x:DataType="MyViewModelClass">
  <Setter Property="ControlProperty" Value="{Binding MyViewModelProperty}" />
</Style>
```

:::info
Pour plus d'informations sur les liaisons compilées, voir ici. --> À FAIRE
:::

## Modèles de données de setter

Comme décrit précédemment ici, lorsque vous utilisez un setter sans un **modèle de données**, une seule instance de la valeur du setter est créée et partagée entre tous les contrôles correspondants. Pour changer la valeur en fonction d'un modèle de données, vous placez le contrôle cible à l'intérieur d'un élément de modèle, comme ceci :

```xml
<Style Selector="Border.empty">
  <Setter Property="Child">
    <Template>
      <TextBlock>Aucun contenu disponible.</TextBlock>
    </Template>
  </Setter>
</Style>
```

:::info
Pour des informations sur les concepts derrière un **modèle de données**, voir [ici](../../concepts/templates).
:::
