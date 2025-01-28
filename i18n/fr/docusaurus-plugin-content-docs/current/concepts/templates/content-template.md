---
description: CONCEPTS - Modèles de données
---

import ContentTemplateStudentScreenshot from '/img/concepts/templates/contenttemplate-student.png';

# Modèle de contenu

Le but d'un modèle de données est de définir comment _Avalonia UI_ doit afficher un objet créé à partir d'une classe que vous avez définie, et qui n'est ni un contrôle, ni une simple chaîne.

Utiliser un modèle de données est un processus en deux étapes :

1. Définir le modèle de données
2. Choisir le modèle de données pour le contenu

Une façon d'utiliser un modèle de données est de définir directement la propriété `ContentTemplate` d'un contrôle. Cela fonctionne sur une fenêtre (car, comme tout contrôle, elle hérite de `ContentControl`).

Vous pouvez définir un modèle de données (pour aucune classe particulière) en utilisant la balise `DataTemplate`, une composition de contrôles intégrés, et quelques liaisons. Par exemple :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:local="using:MySample"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="MySample.MainWindow"
        Title="MySample">
  <Window.ContentTemplate>
    <DataTemplate>
      <StackPanel>
        <Grid ColumnDefinitions="Auto,Auto" RowDefinitions="Auto,Auto">
          <TextBlock Grid.Row="0" Grid.Column="0">First Name:</TextBlock>
          <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding FirstName}"/>
          <TextBlock Grid.Row="1" Grid.Column="0">Last Name:</TextBlock>
          <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding LastName}"/>
        </Grid>
      </StackPanel>
    </DataTemplate>
  </Window.ContentTemplate>
  
  <local:Student FirstName="Jane" LastName="Deer"/>
</Window>
```

Dans ce qui précède, les liaisons se réfèrent aux propriétés de toute classe présente dans la zone de contenu de la fenêtre. Ici, le contenu de la fenêtre est le même objet étudiant que vous avez utilisé auparavant ; mais lorsque vous exécutez ce code, _Avalonia UI_ affiche maintenant :

<img src={ContentTemplateStudentScreenshot} alt=""/>

En utilisant un modèle de données de cette manière, vous avez à la fois défini et choisi le modèle de données pour le contenu au même endroit - en définissant directement la propriété `ContentTemplate` de la fenêtre.

Le code fonctionne correctement car l'objet dans la zone de contenu de la fenêtre a les propriétés spécifiées dans les liaisons. En guise d'exercice : introduisez une liaison pour une propriété qui n'existe pas sur la classe étudiant. (Votre application fonctionnera toujours, mais elle ignorera la propriété qu'elle ne peut pas trouver.)

À la page suivante, vous verrez comment définir plusieurs modèles de données et choisir le modèle correct en fonction de la classe de l'objet dans la zone de contenu de la fenêtre.

