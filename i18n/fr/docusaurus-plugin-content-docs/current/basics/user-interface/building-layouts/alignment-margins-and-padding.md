import LayoutMarginsPaddingAlignmentBasicScreenshot from '/img/basics/user-interface/building-layouts/layout-margins-padding/layout-margins-padding-alignment-graphic1.png';
import LayoutMarginsPaddingAlignmentBasicAnnotatedScreenshot from '/img/basics/user-interface/building-layouts/layout-margins-padding/layout-margins-padding-alignment-graphic2.png';
import LayoutHorizontalAlignmentScreenshot from '/img/basics/user-interface/building-layouts/layout-margins-padding/layout-horizontal-alignment-graphic.png';
import LayoutVerticalAlignmentScreenshot from '/img/basics/user-interface/building-layouts/layout-margins-padding/layout-vertical-alignment-graphic.png';
import LayoutMarginsPaddingAlignmentComplexAnnotatedScreenshot from '/img/basics/user-interface/building-layouts/layout-margins-padding/layout-margins-padding-alignment-graphic3.png';

# Alignement, Marges et Remplissages

Un contrôle Avalonia expose plusieurs propriétés qui sont utilisées pour positionner précisément les éléments enfants. Ce sujet aborde quatre des propriétés les plus importantes : `HorizontalAlignment`, `Margin`, `Padding` et `VerticalAlignment`. Les effets de ces propriétés sont importants à comprendre, car ils fournissent la base pour contrôler la position des éléments dans les applications Avalonia.

### Introduction au Positionnement des Éléments

Il existe de nombreuses façons de positionner des éléments en utilisant Avalonia. Cependant, obtenir une mise en page idéale va au-delà du simple choix du bon élément `Panel`. Un contrôle fin du positionnement nécessite une compréhension des propriétés `HorizontalAlignment`, `Margin`, `Padding` et `VerticalAlignment`.

L'illustration suivante montre un scénario de mise en page qui utilise plusieurs propriétés de positionnement.

<img src={LayoutMarginsPaddingAlignmentBasicScreenshot} alt="Exemple de Positionnement"/>

À première vue, les éléments `Button` dans cette illustration peuvent sembler placés de manière aléatoire. Cependant, leurs positions sont en réalité contrôlées précisément en utilisant une combinaison de marges, d'alignements et de remplissages.

L'exemple suivant décrit comment créer la mise en page dans l'illustration précédente. Un élément `Border` encapsule un `StackPanel` parent, avec une valeur de `Padding` de 15 pixels indépendants du périphérique. Cela tient compte de la bande étroite `LightBlue` qui entoure le `StackPanel` enfant. Les éléments enfants du `StackPanel` sont utilisés pour illustrer chacune des diverses propriétés de positionnement qui sont détaillées dans ce sujet. Trois éléments `Button` sont utilisés pour démontrer à la fois les propriétés `Margin` et `HorizontalAlignment`.

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="AvaloniaApplication2.MainWindow"
        Title="AvaloniaApplication2">
  <Border Background="LightBlue"
          BorderBrush="Black"
          BorderThickness="2"
          Padding="15">
    <StackPanel Background="White"
                HorizontalAlignment="Center"
                VerticalAlignment="Top">
      <TextBlock Margin="5,0"
                 FontSize="18"
                 HorizontalAlignment="Center">
        Exemple d'Alignement, de Marge et de Remplissage
      </TextBlock>
      <Button HorizontalAlignment="Left" Margin="20">Bouton 1</Button>
      <Button HorizontalAlignment="Right" Margin="10">Bouton 2</Button>
      <Button HorizontalAlignment="Stretch">Bouton 3</Button>
    </StackPanel>
  </Border>
</Window>

```

Le diagramme suivant fournit une vue rapprochée des différentes propriétés de positionnement utilisées dans l'exemple précédent. Les sections suivantes de ce sujet décrivent plus en détail comment utiliser chaque propriété de positionnement.

<img src={LayoutMarginsPaddingAlignmentBasicAnnotatedScreenshot} alt="Propriétés de positionnement"/>

### Comprendre les propriétés d'alignement

Les propriétés `HorizontalAlignment` et `VerticalAlignment` décrivent comment un élément enfant doit être positionné dans l'espace de mise en page alloué par un élément parent. En utilisant ces propriétés ensemble, vous pouvez positionner les éléments enfants avec précision. Par exemple, les éléments enfants d'un `DockPanel` peuvent spécifier quatre alignements horizontaux différents : `Gauche`, `Droite`, `Centre` ou `Étirer` pour remplir l'espace disponible. Des valeurs similaires sont disponibles pour le positionnement vertical.

Les propriétés `Height` et `Width` définies explicitement sur un élément ont la priorité sur la valeur de la propriété `Stretch`. Tenter de définir `Height`, `Width` et une valeur de `HorizontalAlignment` de `Stretch` entraîne l'ignorance de la demande d'étirement.

#### Propriété HorizontalAlignment

La propriété `HorizontalAlignment` déclare les caractéristiques d'alignement horizontal à appliquer aux éléments enfants. Le tableau suivant montre chacune des valeurs possibles de la propriété `HorizontalAlignment`.

| Membre | Description |
| :--- | :--- |
| `Gauche` | Les éléments enfants sont alignés à gauche de l'espace de mise en page alloué de l'élément parent. |
| `Centre` | Les éléments enfants sont alignés au centre de l'espace de mise en page alloué de l'élément parent. |
| `Droite` | Les éléments enfants sont alignés à droite de l'espace de mise en page alloué de l'élément parent. |
| `Étirer` \(Par défaut\) | Les éléments enfants sont étirés pour remplir l'espace de mise en page alloué de l'élément parent. Les valeurs explicites de `Largeur` et `Hauteur` prévalent. |

L'exemple suivant montre comment appliquer la propriété `HorizontalAlignment` aux éléments `Button`. Chaque valeur d'attribut est affichée pour mieux illustrer les différents comportements de rendu.

```xml
<Button HorizontalAlignment="Gauche">Bouton 1 (Gauche)</Button>
<Button HorizontalAlignment="Droite">Bouton 2 (Droite)</Button>
<Button HorizontalAlignment="Centre">Bouton 3 (Centre)</Button>
<Button HorizontalAlignment="Étirer">Bouton 4 (Étirer)</Button>
```

Le code précédent produit une mise en page similaire à l'image suivante. Les effets de positionnement de chaque valeur de `HorizontalAlignment` sont visibles dans l'illustration.

<img src={LayoutHorizontalAlignmentScreenshot} alt='Exemple de HorizontalAlignment'/>

#### Propriété VerticalAlignment

La propriété `VerticalAlignment` décrit les caractéristiques d'alignement vertical à appliquer aux éléments enfants. Le tableau suivant montre chacune des valeurs possibles pour la propriété `VerticalAlignment`.
| Membre | Description |
| :--- | :--- |
| `Haut` | Les éléments enfants sont alignés en haut de l'espace de mise en page alloué de l'élément parent. |
| `Centre` | Les éléments enfants sont alignés au centre de l'espace de mise en page alloué de l'élément parent. |
| `Bas` | Les éléments enfants sont alignés en bas de l'espace de mise en page alloué de l'élément parent. |
| `Étirer` \(Par défaut\) | Les éléments enfants sont étirés pour remplir l'espace de mise en page alloué de l'élément parent. Les valeurs explicites de `Largeur` et `Hauteur` ont la priorité. |

L'exemple suivant montre comment appliquer la propriété `VerticalAlignment` aux éléments `Button`. Chaque valeur d'attribut est affichée, pour mieux illustrer les différents comportements de rendu. Pour les besoins de cet exemple, un élément `Grid` avec des lignes de grille visibles est utilisé comme parent, afin de mieux illustrer le comportement de mise en page de chaque valeur de propriété.

```xml
<Border Background="LightBlue" BorderBrush="Black" BorderThickness="2" Padding="15">
    <Grid Background="White" ShowGridLines="True">
      <Grid.RowDefinitions>
        <RowDefinition Height="25"/>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
        <RowDefinition Height="50"/>
      </Grid.RowDefinitions>
      <TextBlock Grid.Row="0" Grid.Column="0"
                 FontSize="18"
                 HorizontalAlignment="Center">
        Exemple d'Alignement Vertical
      </TextBlock>
      <Button Grid.Row="1" Grid.Column="0" VerticalAlignment="Top">Bouton 1 (Haut)</Button>
      <Button Grid.Row="2" Grid.Column="0" VerticalAlignment="Bottom">Bouton 2 (Bas)</Button>
      <Button Grid.Row="3" Grid.Column="0" VerticalAlignment="Center">Bouton 3 (Centre)</Button>
      <Button Grid.Row="4" Grid.Column="0" VerticalAlignment="Stretch">Bouton 4 (Étirer)</Button>
    </Grid>
</Border>
```

Le code précédent produit une mise en page similaire à l'image suivante. Les effets de positionnement de chaque valeur de `VerticalAlignment` sont visibles dans l'illustration.

<img src={LayoutVerticalAlignmentScreenshot} alt='Exemple de propriété VerticalAlignment'/> 

### Comprendre les Propriétés de Marge

La propriété `Margin` décrit la distance entre un élément et ses enfants ou pairs. Les valeurs de `Margin` peuvent être uniformes, en utilisant une syntaxe comme `Margin="20"`. Avec cette syntaxe, un `Margin` uniforme de 20 pixels indépendants du dispositif serait appliqué à l'élément. Les valeurs de `Margin` peuvent également prendre la forme de quatre valeurs distinctes, chaque valeur décrivant un marge distincte à appliquer à gauche, en haut, à droite et en bas (dans cet ordre), comme `Margin="0,10,5,25"`. Une utilisation appropriée de la propriété `Margin` permet un contrôle très précis de la position de rendu d'un élément et de la position de rendu de ses éléments voisins et enfants.

Une marge non nulle applique de l'espace à l'extérieur des `Bounds` de l'élément.

L'exemple suivant montre comment appliquer des marges uniformes autour d'un groupe d'éléments `Button`. Les éléments `Button` sont espacés uniformément avec un tampon de marge de dix pixels dans chaque direction.

```xml
<Button Margin="10">Button 7</Button>
<Button Margin="10">Button 8</Button>
<Button Margin="10">Button 9</Button>
```

Dans de nombreux cas, une marge uniforme n'est pas appropriée. Dans ces cas, un espacement non uniforme peut être appliqué. L'exemple suivant montre comment appliquer un espacement de marge non uniforme aux éléments enfants. Les marges sont décrites dans cet ordre : gauche, haut, droite, bas.

```xml
<Button Margin="0,10,0,10">Button 1</Button>
<Button Margin="0,10,0,10">Button 2</Button>
<Button Margin="0,10,0,10">Button 3</Button>
```

#### Comprendre la Propriété Padding

Le remplissage est similaire à la `marge` à bien des égards. La propriété de remplissage est exposée uniquement sur quelques classes, principalement par commodité : `Border`, `TemplatedControl` et `TextBlock` sont des exemples de classes qui exposent une propriété de remplissage. La propriété `Padding` augmente la taille effective d'un élément enfant par la valeur de `Thickness` spécifiée.

L'exemple suivant montre comment appliquer le `Padding` à un élément parent `Border`.

```xml
<Border Background="LightBlue"
        BorderBrush="Black"
        BorderThickness="2"
        CornerRadius="45"
        Padding="25">
```

#### Utilisation de l'alignement, des marges et du remplissage dans une application

`HorizontalAlignment`, `Margin`, `Padding` et `VerticalAlignment` fournissent le contrôle de positionnement nécessaire pour créer une interface utilisateur complexe. Vous pouvez utiliser les effets de chaque propriété pour changer le positionnement des éléments enfants, permettant ainsi une flexibilité dans la création d'applications dynamiques et d'expériences utilisateur.

L'exemple suivant démontre chacun des concepts détaillés dans ce sujet. En s'appuyant sur l'infrastructure trouvée dans le premier échantillon de ce sujet, cet exemple ajoute un élément `Grid` en tant qu'enfant du `Border` dans le premier échantillon. Un `Padding` est appliqué à l'élément parent `Border`. Le `Grid` est utilisé pour partitionner l'espace entre trois éléments enfants `StackPanel`. Des éléments `Button` sont à nouveau utilisés pour montrer les divers effets de `Margin` et `HorizontalAlignment`. Des éléments `TextBlock` sont ajoutés à chaque `ColumnDefinition` pour mieux définir les différentes propriétés appliquées aux éléments `Button` dans chaque colonne.

```xml
<Border Background="LightBlue"
        BorderBrush="Black"
        BorderThickness="2"
        CornerRadius="45"
        Padding="25">
    <Grid Background="White" ShowGridLines="True">
      <Grid.ColumnDefinitions>
        <ColumnDefinition Width="Auto"/>
        <ColumnDefinition Width="*"/>
        <ColumnDefinition Width="Auto"/>
      </Grid.ColumnDefinitions>

    <StackPanel Grid.Column="0" Grid.Row="0"
                HorizontalAlignment="Left"
                Name="StackPanel1"
                VerticalAlignment="Top">
        <TextBlock FontSize="18" HorizontalAlignment="Center" Margin="0,0,0,15">StackPanel1</TextBlock>
        <Button Margin="0,10,0,10">Bouton 1</Button>
        <Button Margin="0,10,0,10">Bouton 2</Button>
        <Button Margin="0,10,0,10">Bouton 3</Button>
        <TextBlock>ColumnDefinition.Width="Auto"</TextBlock>
        <TextBlock>StackPanel.HorizontalAlignment="Left"</TextBlock>
        <TextBlock>StackPanel.VerticalAlignment="Top"</TextBlock>
        <TextBlock>StackPanel.Orientation="Vertical"</TextBlock>
        <TextBlock>Button.Margin="0,10,0,10"</TextBlock>
    </StackPanel>

    <StackPanel Grid.Column="1" Grid.Row="0"
                HorizontalAlignment="Stretch"
                Name="StackPanel2"
                VerticalAlignment="Top"
                Orientation="Vertical">
        <TextBlock FontSize="18" HorizontalAlignment="Center" Margin="0,0,0,15">StackPanel2</TextBlock>
        <Button Margin="10,0,10,0">Bouton 4</Button>
        <Button Margin="10,0,10,0">Bouton 5</Button>
        <Button Margin="10,0,10,0">Bouton 6</Button>
        <TextBlock HorizontalAlignment="Center">ColumnDefinition.Width="*"</TextBlock>
        <TextBlock HorizontalAlignment="Center">StackPanel.HorizontalAlignment="Stretch"</TextBlock>
        <TextBlock HorizontalAlignment="Center">StackPanel.VerticalAlignment="Top"</TextBlock>
        <TextBlock HorizontalAlignment="Center">StackPanel.Orientation="Horizontal"</TextBlock>
        <TextBlock HorizontalAlignment="Center">Button.Margin="10,0,10,0"</TextBlock>
    </StackPanel>

    <StackPanel Grid.Column="2" Grid.Row="0"
                HorizontalAlignment="Left"
                Name="StackPanel3"
                VerticalAlignment="Top">
        <TextBlock FontSize="18" HorizontalAlignment="Center" Margin="0,0,0,15">StackPanel3</TextBlock>
        <Button Margin="10">Bouton 7</Button>
        <Button Margin="10">Bouton 8</Button>
        <Button Margin="10">Bouton 9</Button>
        <TextBlock>ColumnDefinition.Width="Auto"</TextBlock>
        <TextBlock>StackPanel.HorizontalAlignment="Left"</TextBlock>
        <TextBlock>StackPanel.VerticalAlignment="Top"</TextBlock>
        <TextBlock>StackPanel.Orientation="Vertical"</TextBlock>
        <TextBlock>Button.Margin="10"</TextBlock>
    </StackPanel>
  </Grid>
</Border>
```

Lorsqu'il est compilé, l'application précédente produit une interface utilisateur qui ressemble à l'illustration suivante. Les effets des différentes valeurs de propriété sont évidents dans l'espacement entre les éléments, et les valeurs de propriété significatives pour les éléments de chaque colonne sont affichées dans des éléments `TextBlock`.

<img src={LayoutMarginsPaddingAlignmentComplexAnnotatedScreenshot} alt="Plusieurs propriétés de positionnement dans une application"/>

