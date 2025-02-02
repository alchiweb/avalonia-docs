---
id: resources
title: Comment utiliser les ressources
---


# 👉 Comment utiliser les ressources

Vous aurez souvent besoin de standardiser des éléments graphiques fondamentaux tels que (mais sans s'y limiter) les pinceaux et les couleurs dans vos applications. Vous pouvez les définir comme des ressources à divers niveaux dans votre application _Avalonia UI_, ainsi que dans des fichiers qui peuvent être inclus si nécessaire.

Les ressources sont toujours définies à l'intérieur d'un dictionnaire de ressources. Cela signifie que chaque ressource a un attribut clé.

Le niveau d'un dictionnaire de ressources définit la portée des ressources qu'il contient : les ressources sont disponibles dans le fichier où elles sont définies, et ci-dessous. Vous pouvez donc adapter la portée des ressources en choisissant où localiser un dictionnaire de ressources.

## Déclaration des ressources

Par exemple, vous pouvez souhaiter que les couleurs des pinceaux soient standardisées dans toute l'application. Dans ce cas, vous pouvez déclarer un dictionnaire de ressources dans le fichier XAML de l'application **App.axaml**, comme ceci :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.App">
    // highlight-start
  <Application.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </Application.Resources>
    // highlight-end
</Application>
```

Alternativement, vous pouvez vouloir qu'un ensemble de ressources s'applique uniquement à une fenêtre ou un contrôle utilisateur spécifique. Dans ce cas, vous définirez un dictionnaire de ressources dans le fichier de la fenêtre ou du contrôle utilisateur. Par exemple :

```xml title="MyUserControl.axaml"
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.MyUserControl">
    // highlight-start
  <UserControl.Resources>
    <SolidColorBrush x:Key="Warning">LightYellow</SolidColorBrush>
  </UserControl.Resources>
    // highlight-end
</UserControl>
```

En fait, vous pouvez définir des ressources au niveau du contrôle si nécessaire :

```xml title="MainWindow.axaml"
<Window xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="MyApp.MainWindow">
  <StackPanel>
    // highlight-start
    <StackPanel.Resources>
      <SolidColorBrush x:Key="Warning">PaleGoldenRod</SolidColorBrush>
    </StackPanel.Resources>
    // highlight-end
  </StackPanel>
</Window>
```

Vous pouvez également déclarer des ressources spécifiques à un style.

```xml title="MyStyle.axaml"
<Style Selector="TextBlock.warning">
  <Style.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </Style.Resources>
  <Setter ... />
</Style>
```

:::note
Gardez à l'esprit que cette ressource n'est pas visible en dehors de ce bloc de style spécifique, ce qui signifie qu'elle ne rendra pas chaque TextBlock avec une classe "warning" consciente de cette ressource en dehors du bloc de style.
:::

Il est également possible de définir des ressources pour des variantes de thème spécifiques : Sombre, Clair ou personnalisé. D'après l'exemple ci-dessous, `BackgroundBrush` et `ForegroundBrush` auront des valeurs différentes en fonction de la variante de thème actuelle définie par le système ou l'application. Pour plus d'informations sur les variantes de thème, veuillez lire la page [Variantes de thème](how-to-use-theme-variants).

```xml
<ResourceDictionary>
    <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key='Light'>
            <SolidColorBrush x:Key='BackgroundBrush'>White</SolidColorBrush>
            <SolidColorBrush x:Key='ForegroundBrush'>Black</SolidColorBrush>
        </ResourceDictionary>
        <ResourceDictionary x:Key='Dark'>
            <SolidColorBrush x:Key='BackgroundBrush'>Black</SolidColorBrush>
            <SolidColorBrush x:Key='ForegroundBrush'>White</SolidColorBrush>
        </ResourceDictionary>
    </ResourceDictionary.ThemeDictionaries>
</ResourceDictionary>
```

## Fichiers de dictionnaire de ressources

Vous pouvez améliorer l'organisation de votre projet d'application _Avalonia UI_ en définissant des dictionnaires de ressources dans leurs propres fichiers. Cela rend les définitions de ressources faciles à localiser et à maintenir.

Les ressources situées dans un fichier de dictionnaire de ressources sont accessibles à l'ensemble de l'application.

Pour ajouter un fichier de dictionnaire de ressources, suivez cette procédure :

-  Cliquez avec le bouton droit sur votre projet à l'emplacement où vous souhaitez créer le nouveau fichier.
-  Cliquez sur **Ajouter**, puis sur **Nouvel élément**.
-  Cliquez sur **Avalonia** dans la liste à gauche :

<img src="/img/gitbook-import/assets/image (8) (1) (2).png" alt=""/>

-  Cliquez sur **Dictionnaire de ressources (Avalonia)**.
-  Tapez le nom de fichier que vous souhaitez utiliser.
-  Cliquez sur **Ajouter**.

:::note
Après la création du fichier de ressources, vous devez l'inclure correctement dans votre application. Voir la section [Inclure et fusionner des ressources](#include-and-merge-resources).
:::

Vous pouvez maintenant ajouter les ressources que vous souhaitez définir à l'emplacement indiqué. Cela ressemble à ceci :

```xml
<ResourceDictionary xmlns="https://github.com/avaloniaui"
                    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <!-- Ajoutez les ressources ici -->
</ResourceDictionary>
```

## Utilisation des ressources

Vous pouvez utiliser une ressource d'un dictionnaire de ressources qui est dans le champ d'application en utilisant l'extension de balisage `{DynamicResource}`.

Par exemple, pour utiliser une ressource directement sur l'attribut de fond d'un élément de bordure, utilisez le XAML suivant :

```xml
<Border Background="{DynamicResource Warning}">
  Attention !
</Border>
```

### Ressource statique

Alternativement, vous pouvez choisir d'utiliser l'extension de balisage `StaticResource`. Par exemple :

```xml
<Border Background="{StaticResource Warning}">
  Attention !
</Border>
```

Une ressource statique est différente en ce sens qu'elle ne répondra pas aux modifications apportées à la ressource dans le code (à l'exécution). Une fois chargée, une ressource statique ne peut pas être modifiée.

L'avantage d'utiliser une ressource statique est qu'elle nécessite moins de travail à effectuer, donc elle sera légèrement plus rapide à charger et utilise légèrement moins de mémoire.

## Priorité des ressources

_Avalonia UI_ résout quelle ressource utiliser en recherchant vers le haut dans l'**arbre de contrôle logique** à partir du niveau d'un balisage `DynamicResource` ou `StaticResource`, à la recherche de la clé de ressource.

Cela signifie que les ressources ayant la même clé ont la priorité en fonction de leur proximité par rapport au balisage de ressource en cours de résolution. Les définitions de ressources plus haut dans l'arbre de contrôle logique sont donc effectivement 'écrasées' par celles qui sont plus proches. Par exemple, considérons ce XAML :

```xml
<UserControl ... >
  <UserControl.Resources>
    <SolidColorBrush x:Key="Warning">Yellow</SolidColorBrush>
  </UserControl.Resources>

  <StackPanel>
    <StackPanel.Resources>
      <SolidColorBrush x:Key="Warning">Orange</SolidColorBrush>
    </StackPanel.Resources>

    <Border Background="{DynamicResource Warning}">
      Attention !
    </Border>
  </StackPanel>
</UserControl>
```

Ici, le contrôle de bordure utilise la ressource avec la clé 'Warning'. Cela est défini deux fois - une fois au niveau du panneau empilé englobant, et à nouveau au niveau du contrôle utilisateur. _Avalonia UI_ déterminera que l'arrière-plan de la bordure doit être orange car son panneau empilé parent est le premier dans une recherche vers le haut dans l'arbre de contrôle logique à partir de la bordure elle-même.

## Inclure et Fusionner des Ressources

Les ressources peuvent être incluses à partir d'un fichier de dictionnaire de ressources et fusionnées avec les ressources définies dans un autre fichier (même s'il n'y en a pas).

<img src="/img/gitbook-import/assets/image (1) (4).png" alt=""/>

Dans le cas où vous souhaitez fusionner un dictionnaire de ressources au niveau de l'application entière, vous devez déclarer un dictionnaire de ressources dans la section **Application.Resources** du fichier XAML de l'application **App.axaml**, comme ceci :

```xml
<Application.Resources>
  <ResourceDictionary>
    <ResourceDictionary.MergedDictionaries>
      <MergeResourceInclude Source="/Assets/AppResources.axaml" />
    </ResourceDictionary.MergedDictionaries>
  </ResourceDictionary>
</Application.Resources>
```

Vous pouvez également fusionner un dictionnaire de ressources pour déclarer des ressources fusionnées spécifiques à un style.

<img src="/img/gitbook-import/assets/image (1) (3).png" alt=""/>

Cela signifie que vous pouvez implémenter des styles dans un fichier et utiliser des ressources définies dans un autre. Cela maintient la cohérence de votre style et rend la solution de votre application bien organisée et facile à maintenir.

Pour inclure le dictionnaire de ressources d'un fichier dans un fichier de styles, ajoutez le XAML suivant :

```xml
<Styles.Resources>
    <ResourceDictionary>
      <ResourceDictionary.MergedDictionaries>
        <ResourceInclude Source="/Assets/AppResources.axaml"/>
      </ResourceDictionary.MergedDictionaries>
    </ResourceDictionary>
  </Styles.Resources>
```

Dans les exemples ci-dessus, le fichier de ressources `AppResources.axaml` est situé dans le dossier de projet `/Assets`. Vous pouvez ensuite définir les styles en utilisant les ressources, par exemple :

```xml
<Style Selector="Button.btn-info">
    <Setter Property="Background" Value="{StaticResource InfoColor}"/>
</Style>
```

Où la ressource `InfoColor` est définie comme un `SolidColorBrush` dans le fichier importé.

:::info
Notez que la ressource a été référencée en utilisant `StaticResource` car elle ne doit pas changer - l'exigence ici est de maintenir la cohérence du style.
:::

## Priorité des ressources fusionnées

Comme vous l'avez vu précédemment, les ressources sont résolues en recherchant dans l'arbre logique de contrôle à partir du point de balisage jusqu'à ce qu'une ressource avec la clé demandée soit trouvée.

Cependant, la présence de styles et de dictionnaires fusionnés définis à divers niveaux d'une application introduit des règles de priorité supplémentaires comme suit :

* Ressources de contrôle -> Dictionnaires fusionnés
* Ressources de style -> Dictionnaires fusionnés
* Ressources d'application -> Dictionnaires fusionnés

Par exemple, dans l'application théorique ci-dessous, la recherche d'une ressource utilisée sur le contrôle de bordure (en bas) suivra l'ordre indiqué dans les crochets `[]` :

```
Application
 |- Resources [11]
     |- Merged dictionary [12]
     |- Merged dictionary [13]
 |- Styles
     |- Resources [14]
         |- Merged dictionary [15]
         |- Merged dictionary [16]

Window
 |- Resources [6]
     |- Merged dictionary [7]
 |- Styles
     |- Resources [8]
         |- Merged dictionary [9]
         |- Merged dictionary [10]
 |- StackPanel
     |- Resources [1]
         |- Merged dictionary [2]
         |- Merged dictionary [3]
     |- Styles
         |- Resources [4]
             |- Merged dictionary [5]
     |- Border
```

À partir de la frontière, les premières ressources recherchées sont celles définies dans le contrôle parent (panneau empilé). Après cela, tous les dictionnaires fusionnés au même niveau sont considérés - dans l'ordre dans lequel ils apparaissent dans le XAML.

La recherche passe ensuite à la recherche de styles définis dans le contrôle parent (panneau empilé), suivie de tous les dictionnaires fusionnés à ce niveau.

La recherche remonte dans l'arbre logique des contrôles, se comportant à chaque niveau de manière similaire. Elle atteint finalement les ressources et styles au niveau de l'application.

## Consommation des ressources depuis le code

Avalonia propose différentes options pour accéder aux ressources depuis le code.

:::note

`ResourceNode` dans les exemples ci-dessous peut être n'importe quel nœud qui prend en charge `Resource`, comme `Application.Current`, `Window`, `UserControl`, ...

:::

- **ResourceNode.Resources["TheKey"]** : <br/>
  Cela accédera directement au `Dictionary` sous-jacent. Attention : les dictionnaires fusionnés et les parents ne seront pas scannés. 
- **ResourceNode.TryGetResource** : <br/>
  Cette fonction essaiera d'obtenir une ressource spécifique et renverra `true` si elle réussit, sinon `false`. Les dictionnaires fusionnés seront scannés, mais il ne suivra pas l'arbre logique. 
- **ResourceNode.TryFindResource** : <br/>
  Cette méthode d'extension essaiera d'obtenir une ressource spécifique et renverra `true` si elle réussit, sinon `false`. Les dictionnaires fusionnés et l'arbre logique seront également scannés.
- **ResourceNode.GetResourceObservable**: <br/>
  Cela renverra un [`IObservable`](https://learn.microsoft.com/en-us/dotnet/api/System.IObservable-1) qui peut être utilisé pour observer les changements sur la ressource. Par exemple, vous pourriez vous y lier.

```cs
// Dans cet exemple, nous avons défini la ressource dans App.axaml et nous voulons récupérer la valeur dans le constructeur de MainWindow.
//
//    </Application.Resources>
//         <x:String x:Key="TheKey">HelloWorld</x:String>
//    </Application.Resources>

public MainWindow()
{
    InitializeComponent();

    // found1 = false | result1 = null
    var found1 = this.TryGetResource("TheKey", this.ActualThemeVariant, out var result1);

    // found2 = true | result2 = "Hello World" 
    var found2 = this.TryFindResource("TheKey", this.ActualThemeVariant, out var result2);

    // Liez la ressource à un TextBlock depuis le code-behind
    myTextBlock.Bind(TextBlock.TextProperty, Resources.GetResourceObservable("TheKey"));

    // cela mettra à jour myTextBlock.Text via l'observable lié
    this.Resources["TheKey"] = "Hello from code behind"; 
}
```
