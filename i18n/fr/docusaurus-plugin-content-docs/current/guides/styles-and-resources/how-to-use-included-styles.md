---
id: how-to-use-included-styles
title: Comment utiliser les styles inclus
---

import VsStylesTemplateScreenshot from '/img/guides/styles-and-resources/vs-styles-template.png';

# Comment utiliser les styles inclus

Ce guide vous montre comment partager des styles à partir d'un fichier de styles séparé (qui est inclus dans votre application). Cette approche vous permet de partager des styles entre plusieurs applications.

Pour ce faire, vous devez définir les styles dans un nouveau fichier XAML. Ici, l'élément racine doit être soit un élément `Style`, soit un élément `Styles`. Par exemple :

```xml
<Styles xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml">
    <Style Selector="TextBlock.h1">
        <Setter Property="FontSize" Value="24"/>
        <Setter Property="FontWeight" Value="Bold"/>
    </Style>
</Styles>
```

Les modèles de solution _Avalonia UI_ offrent un moyen rapide d'ajouter un fichier de styles à votre projet. Suivez cette procédure :

-  Dans l'**Explorateur de solutions**, faites un clic droit sur votre projet.
-  Cliquez sur **Ajouter** et **Nouvel élément**
-  Dans les éléments Avalonia, cliquez sur **Styles (Avalonia)**
-  Tapez un nom pour votre fichier de styles

<img src={VsStylesTemplateScreenshot} alt=""/>

Pour utiliser les styles définis dans un fichier séparé, vous devez le référencer en utilisant un élément `StyleInclude`. L'attribut source définit l'emplacement du fichier de styles. Vous pouvez choisir le niveau auquel ajouter cet élément.

Par exemple, pour utiliser des styles définis dans un fichier `AppStyles.axaml` (enregistré dans le dossier `/Styles`), vous pourriez écrire un élément `StyleInclude` dans la fenêtre comme ceci :

```xml
<Window ... >
    <Window.Styles>
        <StyleInclude Source="/Styles/AppStyles.axaml" />
    </Window.Styles>

    <StackPanel>
       <TextBlock Classes="h1">Titre 1</TextBlock>
       <TextBlock>Ce n'est pas un titre et ne sera pas modifié.</TextBlock>
    </StackPanel>
</Window>
```

Cependant, il est plus courant de référencer un fichier de styles dans le fichier `App.axaml` comme ceci :

```xml
<Application... > 
    <Application.Styles>
        <FluentTheme Mode="Light"/>
        <StyleInclude Source="/AppStyles.axaml"/>
    </Application.Styles>
</Application>
```

Cela vous permettra d'utiliser les styles du fichier séparé dans toute votre application.

Vous pouvez également inclure des styles d'un autre assembly en utilisant le préfixe `avares://` :

```xml
<Application... > 
    <Application.Styles>
        <FluentTheme Mode="Light"/>
        <StyleInclude Source="avares://MyApp.Shared/Styles/CommonAppStyles.axaml"/>
    </Application.Styles>
</Application>
```

fera référence au fichier `/Styles/CommonAppStyles.axaml` du projet `MyApp.Shared`.
