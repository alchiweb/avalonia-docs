---
description: CONCEPTS
---

# Avalonia XAML

_Avalonia UI_ utilise XAML pour définir une interface utilisateur. XAML est un langage de balisage basé sur XML qui est utilisé par de nombreux frameworks UI.

:::info
Ces pages vous introduiront à la manière dont XAML est utilisé spécifiquement dans _Avalonia UI_. Pour des informations de base sur l'utilisation de XAML ailleurs dans les technologies Microsoft, vous pouvez consulter ces références :

* Documentation Microsoft XAML pour WPF, voir [ici](https://docs.microsoft.com/en-us/dotnet/framework/wpf/advanced/xaml-overview-wpf). 
* Documentation Microsoft XAML pour UWP, voir [ici](https://docs.microsoft.com/en-us/windows/uwp/xaml-platform/xaml-overview).
:::

## Extension de Fichier AXAML

L'extension de fichier pour les fichiers XAML utilisés ailleurs est `.xaml`, mais en raison de problèmes techniques d'intégration avec Visual Studio, _Avalonia UI_ utilise sa propre extension `.axaml` - 'Avalonia XAML'.

## Format de Fichier

Un fichier XAML typique d'Avalonia ressemble à ceci :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="AvaloniaApplication1.MainWindow">
</Window>
```

Comme pour tous les fichiers XML, il y a un élément racine. La balise de l'élément racine `<Window></Window>` définit le type de la racine. Cela correspondra à un type de contrôle d'Avalonia UI, dans l'exemple ci-dessus, une fenêtre.

L'exemple ci-dessus utilise trois attributs intéressants :

* `xmlns="https://github.com/avaloniaui"` - il s'agit de la déclaration de l'espace de noms XAML pour _Avalonia UI_ lui-même. Cela est requis, sans cela, le fichier ne sera pas reconnu comme un document XAML Avalonia.
* `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"` - il s'agit de la déclaration pour l'espace de noms du langage XAML.
* `x:Class="AvaloniaApplication1.MainWindow"` - il s'agit d'une extension de la déclaration ci-dessus (pour 'x') qui indique au compilateur XAML où trouver la classe associée à ce fichier. La classe est définie dans un fichier de code-behind, généralement écrit en C#.

:::info
Pour des informations sur le concept de code-behind, voir [ici](code-behind).
:::

## Éléments de Contrôle

Vous pouvez composer une interface utilisateur pour votre application en ajoutant des éléments XML qui représentent l'un des contrôles _Avalonia UI_. La balise de l'élément utilise le même nom que le nom de la classe de contrôle.

:::info
Une interface utilisateur peut être composée de plusieurs types de contrôles différents. Pour en savoir plus sur le concept de composition UI, voir [ici](../../concepts/ui-composition.md).
:::

Par exemple, ce XAML ajoute un bouton au contenu d'une fenêtre :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Button>Hello World!</Button>
</Window>
```

:::info
Pour une liste complète des contrôles intégrés _Avalonia UI_, voir la référence [ici](../../reference/controls).
:::

## Attributs de Contrôle

Les éléments XML qui représentent des contrôles ont des attributs correspondant aux propriétés des contrôles qui peuvent être définies. Vous pouvez définir une propriété de contrôle en ajoutant un attribut à un élément.

Par exemple, pour spécifier un fond bleu pour un contrôle de bouton, vous ajoutez l'attribut `Background` et définissez la valeur sur `"Blue"`. Comme ceci :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Button Background="Blue">Hello World!</Button>
</Window>
```

## Contenu du Contrôle

Vous avez peut-être remarqué que le bouton dans l'exemple ci-dessus a son contenu (la chaîne 'Hello World') placé entre ses balises d'ouverture et de fermeture. En alternative, vous pouvez définir l'attribut de contenu, comme suit :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Button Content="Hello World!"/>
</Window>
```

Ce comportement est spécifique au contenu d'un contrôle _Avalonia UI_.

## Liaison de Données

Vous utiliserez souvent le système de liaison _Avalonia UI_ pour lier une propriété de contrôle à un objet sous-jacent. Le lien est déclaré en utilisant l'extension de balisage `{Binding}`. Par exemple :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Button Content="{Binding Greeting}"/>
</Window>
```

:::info
Pour plus d'informations sur le concept derrière la liaison de données, voir [ici](../data/data-binding).
:::

## Fichiers Code-behind

De nombreux fichiers XAML Avalonia ont également un fichier code-behind associé qui est généralement écrit en C#, et a l'extension de fichier `.axaml.cs`.

:::info
Pour des conseils sur la programmation utilisant des fichiers code-behind, voir [ici](code-behind).
:::

## Espaces de noms XML

Comme pour tout format XML, dans les fichiers XAML Avalonia, vous pouvez déclarer des espaces de noms. Cela permet au processeur XML de trouver les définitions des éléments dans le fichier.

:::info
Pour des informations de base, voir la documentation Microsoft sur les espaces de noms XML [ici](https://docs.microsoft.com/en-us/dotnet/standard/data/xml/managing-namespaces-in-an-xml-document).
:::

Vous pouvez ajouter un espace de noms en utilisant l'attribut `xmlns`. Le format d'une déclaration d'espace de noms est le suivant :

```xml
xmlns:alias="définition"
```

Il est d'usage de définir tous les espaces de noms que vous allez utiliser dans l'élément racine.

Un seul espace de noms dans un fichier peut être défini sans utiliser la partie alias du nom de l'attribut. L'alias doit toujours être unique dans un fichier.

La partie définition de la déclaration d'espace de noms peut être soit une URL, soit une définition de code. Ces deux éléments sont utilisés pour localiser la définition des éléments dans le fichier.

:::info
Pour des conseils détaillés sur le fonctionnement des déclarations d'espace de noms, voir [ici](../../guides/custom-controls/how-to-create-a-custom-controls-library.md).
:::

Il existe deux options de syntaxe valides pour la partie définition d'un attribut d'espace de noms XAML qui fait référence au code :

### **Utilisation d'un Préfixe**

Le préfixe `using:` peut être utilisé lors de la fourniture d'un alias pour un espace de noms dans l'assembly actuel ou un assembly référencé. La syntaxe est la même dans les deux cas. Par exemple :

```xml
xmlns:myAlias1="using:AppNameSpace.MyNamespace"
```
### **Préfixe d'Espace de Noms CLR**

Le préfixe `clr-namespace:`, le même que dans WPF, est également pris en charge. Cependant, la syntaxe dépend de savoir si l'espace de noms pour lequel un alias est requis se trouve dans l'assembly actuel ou un assembly référencé.

Par exemple, lorsque l'espace de noms se trouve dans le même assembly que le XAML, vous pouvez utiliser cette syntaxe :

```xml
<Window ...
    xmlns:myAlias1="clr-namespace:AppNameSpace.MyNamespace" 
... >
```

Si l'espace de noms se trouve dans un autre assembly référencé (par exemple dans une bibliothèque), vous devez étendre la description pour inclure le nom de l'assembly référencé :

```xml
<Window ...
    xmlns:myAlias2="clr-namespace:OtherAssembly.MyNameSpace;assembly=OtherAssembly"
 ... >
```
