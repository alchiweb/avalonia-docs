---
id: how-to-use-theme-variants
title: Comment utiliser les variantes de thème
---

:::tip
Parce que les variantes de thème sont profondément intégrées dans le système de ressources, il est important de comprendre d'abord les [ressources](resources) d'Avalonia.
:::

## Introduction

Dans Avalonia, une `variante de thème` fait référence à une apparence visuelle spécifique d'un contrôle basée sur un thème choisi.

En utilisant des variantes de thème, les développeurs peuvent créer des interfaces utilisateur visuellement attrayantes et cohérentes qui s'adaptent aux différentes préférences des utilisateurs ou aux paramètres du système. Par exemple, une application peut proposer une variante de thème clair avec un fond blanc et un texte noir, ainsi qu'une variante de thème sombre avec un fond noir et un texte blanc. L'utilisateur peut choisir son thème préféré, et l'application ajustera son apparence en conséquence.

Les thèmes intégrés d'Avalonia, `SimpleTheme` et `FluentTheme`, prennent en charge sans effort les variantes `Sombre` et `Clair` sans code supplémentaire. Cela permet aux applications de s'adapter dynamiquement en fonction des préférences du système tout en utilisant des contrôles intégrés. Pour une personnalisation avancée, cette documentation explique la définition de ressources dépendantes de variantes personnalisées et leur référence.

## Changer la variante de thème actuelle
https://avalonia.choixdevie.fr/fr/docs/guides/styles-and-resources/how-to-use-theme-variants#propri%C3%A9t%C3%A9-actualthemevariant
Par défaut, Avalonia hérite de la variante de thème définie par les préférences de l'utilisateur à l'échelle du système. L'application a le contrôle sur les variantes de thème à travers deux propriétés importantes : [ActualThemeVariant](#propriété-actualthemevariant) et [RequestedThemeVariant](#propriété-requestedthemevariant). Ces propriétés permettent de gérer et de changer les variantes de thème à différents niveaux au sein de votre application.

### Propriété `ActualThemeVariant`

La propriété en lecture seule ActualThemeVariant est utilisée pour récupérer le thème UI actuellement utilisé par un contrôle, une fenêtre ou une application. Elle représente la variante de thème qui est activement appliquée à l'élément. Cette propriété est disponible sur chaque contrôle et est héritée dans l'arbre. Sa valeur est également utilisée par le système de style lors de l'accès aux `dictionnaires de thèmes`.

### Propriété `RequestedThemeVariant`

La propriété RequestedThemeVariant permet de remplacer la variante de thème et de spécifier une variante souhaitée pour une `Application`, une `Window` (`TopLevel`) ou un `ThemeVariantScope`.

Pour remplacer la variante globale de l'application au lieu d'utiliser la valeur par défaut du système :

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="AvaloniaApplication.App"
    // highlight-start
             RequestedThemeVariant="Dark">
    // highlight-end
  <Application.Styles>
    <FluentTheme />
  </Application.Styles>
</Application>
```

Ou il est possible de redéfinir la variante de thème par sous-arbre spécifique en utilisant le contrôle `ThemeVariantScope`. Dans l'exemple ci-dessous, la fenêtre utilise la variante Sombre, tandis que le ThemeVariantScope à l'intérieur la redéfinit avec la variante Claire :

```xml title="MainWindow.axaml"
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x='http://schemas.microsoft.com/winfx/2006/xaml'
        x:Class="AvaloniaApplication.MainWindow"
    // highlight-start
        RequestedThemeVariant="Dark"
    // highlight-end
        Background="Gray">
  <StackPanel Spacing="5" Margin="5">
    <Button Content="Dark button" />
    // highlight-start
    <ThemeVariantScope RequestedThemeVariant="Light">
    // highlight-end
      <Button Content="Light button" />
    </ThemeVariantScope>
  </StackPanel>
</Window>
```

![Variante de thème remplacée](/img/basics/user-interface/styling/overriden-theme-variant.png)

S'il est nécessaire de réinitialiser la valeur de RequestedThemeVariant, la valeur `RequestedThemeVariant="Default"` peut être définie dessus.

:::tip
Changer la variante de thème demandée de la fenêtre affecte également la variante des décorations de fenêtre sur la plateforme où cela est pris en charge.
:::

## Defining and referencing custom variant specific resources

## Définir et référencer des ressources spécifiques à une variante personnalisée

Dans Avalonia, les ressources spécifiques à une variante de thème peuvent être définies dans le `ResourceDictionary` en utilisant la propriété `ThemeDictionaries`.

En général, les développeurs utilisent `Light` ou `Dark` comme clé pour les variantes de thème. L'utilisation de `Default` comme clé marque ce dictionnaire de thème spécifique comme une solution de repli au cas où la variante de thème ou la clé de ressource ne serait pas trouvée dans d'autres dictionnaires de thème.

En poursuivant l'exemple précédent, ajoutons `BackgroundBrush` et `ForegroundBrush` avec des valeurs différentes par variante de thème :

```xml title="MainWindow.axaml"
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x='http://schemas.microsoft.com/winfx/2006/xaml'
        x:Class="Sandbox.MainWindow"
        RequestedThemeVariant="Dark"
        Background="Gray">
  <Window.Resources>
    // highlight-start
    <ResourceDictionary>
      <ResourceDictionary.ThemeDictionaries>
        <ResourceDictionary x:Key='Light'>
          <SolidColorBrush x:Key='BackgroundBrush'>SpringGreen</SolidColorBrush>
          <SolidColorBrush x:Key='ForegroundBrush'>Black</SolidColorBrush>
        </ResourceDictionary>
        <ResourceDictionary x:Key='Dark'>
          <SolidColorBrush x:Key='BackgroundBrush'>DodgerBlue</SolidColorBrush>
          <SolidColorBrush x:Key='ForegroundBrush'>White</SolidColorBrush>
        </ResourceDictionary>
      </ResourceDictionary.ThemeDictionaries>
    </ResourceDictionary>
    // highlight-end
  </Window.Resources>
  
  <Window.Styles>
    // highlight-start
    <Style Selector="Button">
      <Setter Property="Background" Value="{DynamicResource BackgroundBrush}" />
      <Setter Property="Foreground" Value="{DynamicResource ForegroundBrush}" />
    </Style>
    // highlight-end
  </Window.Styles>

  <StackPanel Spacing="5" Margin="5">
    <Button Content="Dark button"
            Background="{DynamicResource BackgroundBrush}"
            Foreground="{DynamicResource ForegroundBrush}" />
    <ThemeVariantScope RequestedThemeVariant="Light">
      <Button Content="Light button"
              Background="{DynamicResource BackgroundBrush}"
              Foreground="{DynamicResource ForegroundBrush}" />
    </ThemeVariantScope>
  </StackPanel>
</Window>

```

![Dictionnaires de Thème Personnalisés](/img/basics/user-interface/styling/custom-theme-dictionaries.png)

Pour plus de détails sur l'utilisation des ressources, veuillez consulter la page [Comment Utiliser les Ressources](resources).
