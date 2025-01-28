---
description: CONCEPTS - Modèles de données
---

import DataTemplatesScopeScreenshot from '/img/concepts/templates/datatemplates-scope.png';

# Réutilisation des Modèles de Données

Si vous définissez un modèle de données dans la collection `Window.DataTemplates` (comme sur la page précédente), vous pouvez le réutiliser n'importe où dans la fenêtre. Cependant, vous pouvez également étendre la réutilisation d'un modèle de données à n'importe quelle fenêtre de votre application.

Cela fonctionne parce que _Avalonia UI_ effectue une recherche hiérarchique de son arbre logique pour choisir un modèle de données. Dans son extension maximale, la recherche commence dans un contrôle, s'étend à tous les contrôles parents (récursivement), puis regarde dans la fenêtre (comme sur la page précédente), et enfin examine l'application elle-même pour une collection de modèles de données.

:::info
Pour plus d'informations sur le concept d'arbre logique dans _Avalonia UI_, voir [ici](../ui-composition.md).
:::

Par conséquent, si vous souhaitez réutiliser un modèle dans n'importe quelle fenêtre de votre application : définissez des modèles dans la collection `Application.DataTemplates`, située dans le fichier app.axaml.

Pour voir comment cela fonctionne, ajoutez d'abord un autre modèle de vue comme suit :

```csharp
namespace MySample
{
    public class Teacher
    {
        public string Name { get; set; } = String.Empty;
        public string Subject { get; set; } = String.Empty;
    }
}
```

Et dans le fichier app.axaml, ajoutez un modèle de données pour le type `Teacher` :

```xml
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MySample"
             x:Class="MySample.App"
             RequestedThemeVariant="Light">
    <Application.Styles>
        <FluentTheme />
    </Application.Styles>

  <Application.DataTemplates>
    <DataTemplate DataType="{x:Type vm:Teacher}">
      <Grid ColumnDefinitions="Auto,Auto" RowDefinitions="Auto,Auto">
        <TextBlock Grid.Row="0" Grid.Column="0">Name:</TextBlock>
        <TextBlock Grid.Row="0" Grid.Column="1" Text="{Binding Name}"/>
        <TextBlock Grid.Row="1" Grid.Column="0">Subject:</TextBlock>
        <TextBlock Grid.Row="1" Grid.Column="1" Text="{Binding Subject}"/>
      </Grid>
    </DataTemplate>
  </Application.DataTemplates>
</Application>
```

Utilisez une définition locale d'un enseignant dans la zone de contenu de la fenêtre :

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
  
  <local:Teacher Name="Dr Jones" Subject="Maths"/>
</Window>
```

Bien qu'il n'y ait pas de modèle de données pour un enseignant dans la fenêtre ; _Avalonia UI_ trouvera le modèle que vous avez défini dans l'application, et l'affichage fonctionnera comme prévu :

<img src={DataTemplatesScopeScreenshot} alt=""/>

:::warning
N'oubliez pas de spécifier un `DataType` dans chaque modèle de données, où qu'il soit défini, car si _Avalonia UI_ ne parvient pas à trouver un modèle de données correspondant à vos données ; alors rien ne sera affiché !
:::
