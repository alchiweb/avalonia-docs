---
description: CONCEPTS - Modèles de données
---

import DataTemplatesCollectionStudentScreenshot from '/img/concepts/templates/datatemplates-collection-student.png';

# Collection de modèles de données

Chaque contrôle dans _Avalonia UI_ a une collection `DataTemplates` où vous pouvez placer un nombre quelconque de définitions de modèles de données. Vous pouvez ensuite choisir le modèle à utiliser pour l'affichage par type de classe.

Lorsqu'un contrôle n'a pas de modèle de données défini directement dans sa propriété `ContentTemplate` (comme sur la page précédente) ; il choisira alors un modèle dans sa collection `DataTemplates` qui correspond à la classe de l'objet affiché. Cela s'applique à une fenêtre.

Les modèles de données sont assortis par type : une correspondance se produit lorsque la classe de l'objet affiché est la même que le nom de classe entièrement qualifié spécifié dans la propriété `DataType` d'un modèle.

Ainsi, vous pouvez modifier l'exemple précédent pour utiliser la collection `DataTemplates`, comme suit :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:local="using:MySample"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="MySample.MainWindow"
        Title="MySample">
  <Window.DataTemplates>
    <DataTemplate DataType="{x:Type local:Student}">
      <Grid ColumnDefinitions="Auto,Auto" RowDefinitions="Auto,Auto">
        <TextBlock Grid.Row="0" Grid.Column="0">First Name:</TextBlock>
        <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding FirstName}"/>
        <TextBlock Grid.Row="1" Grid.Column="0">Last Name:</TextBlock>
        <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding LastName}"/>
      </Grid>
    </DataTemplate>
  </Window.DataTemplates>
  
  <local:Student FirstName="Jane" LastName="Deer"/>
</Window>
```

Cela donne exactement le même affichage que sur la page précédente :

<img src={DataTemplatesCollectionStudentScreenshot} alt=""/>

