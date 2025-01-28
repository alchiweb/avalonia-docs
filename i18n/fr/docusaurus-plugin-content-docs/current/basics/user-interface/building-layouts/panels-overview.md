import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import CanvasOverlapScreenshot from '/img/basics/user-interface/building-layouts/panels-overview/canvas-example.png';
import DockPanelSampleScreenshot from '/img/basics/user-interface/building-layouts/panels-overview/dockpanel-example.png';
import GridSampleScreenshot from '/img/basics/user-interface/building-layouts/panels-overview/grid-example.png';
import StackPanelSampleScreenshot from '/img/basics/user-interface/building-layouts/panels-overview/stackpanel-example.png';
import WrapPanelSampleScreenshot from '/img/basics/user-interface/building-layouts/panels-overview/wrappanel-example.png';
import UniformGridSampleScreenshot from '/img/basics/user-interface/building-layouts/panels-overview/uniformgrid-example.png';

# Aperçu des Panneaux

Les éléments `Panel` sont des composants qui contrôlent le rendu des éléments - leur taille et dimensions, leur position, et l'agencement de leur contenu enfant. _Avalonia UI_ fournit un certain nombre d'éléments `Panel` prédéfinis ainsi que la possibilité de construire des éléments `Panel` personnalisés.

## La Classe Panel

`Panel` est la classe de base pour tous les éléments qui fournissent un support de mise en page dans Avalonia. Les éléments `Panel` dérivés sont utilisés pour positionner et agencer des éléments en XAML et dans le code.

Avalonia inclut une suite complète d'implémentations de panneaux dérivés qui permettent de nombreuses mises en page complexes. Ces classes dérivées exposent des propriétés et des méthodes qui permettent la plupart des scénarios UI standard. Les développeurs qui ne parviennent pas à trouver un comportement d'agencement enfant qui répond à leurs besoins peuvent créer de nouvelles mises en page en remplaçant les méthodes `ArrangeOverride` et `MeasureOverride`. Pour plus d'informations sur les comportements de mise en page personnalisés, voir [Créer un Panneau Personnalisé](../../../guides/custom-controls/create-a-custom-panel.md).

### Membres Communs du Panel

Tous les éléments `Panel` prennent en charge les propriétés de taille et de positionnement de base définies par `Control`, y compris `Height`, `Width`, `HorizontalAlignment`, `VerticalAlignment` et `Margin`. Pour des informations supplémentaires sur les propriétés de positionnement définies par `Control`, voir [Aperçu de l'Alignement, des Marges et du Remplissage](alignment-margins-and-padding.md).

`Panel` expose des propriétés supplémentaires qui sont d'une importance critique pour comprendre et utiliser la mise en page. La propriété `Background` est utilisée pour remplir la zone entre les limites d'un élément de panneau dérivé avec un `Brush`. `Children` représente la collection d'éléments enfants dont le `Panel` est composé.

**Propriétés attachées**

Les éléments de panneau dérivés utilisent largement les propriétés attachées. Une propriété attachée est une forme spécialisée de propriété de dépendance qui n'a pas l'enveloppe conventionnelle de propriété du runtime commun (CLR). Les propriétés attachées ont une syntaxe spécialisée en XAML, que l'on peut voir dans plusieurs des exemples qui suivent.

Un des objectifs d'une propriété attachée est de permettre aux éléments enfants de stocker des valeurs uniques d'une propriété qui est en réalité définie par un élément parent. Une application de cette fonctionnalité est de permettre aux éléments enfants d'informer le parent de la manière dont ils souhaitent être présentés dans l'interface utilisateur, ce qui est extrêmement utile pour la mise en page des applications.

### Panneaux d'interface utilisateur

Il existe plusieurs classes de panneaux disponibles dans Avalonia qui sont optimisées pour prendre en charge des scénarios d'interface utilisateur : `Panel`, `Canvas`, `DockPanel`, `Grid`, `StackPanel`, `WrapPanel` et `RelativePanel`. Ces éléments de panneau sont faciles à utiliser, polyvalents et suffisamment extensibles pour la plupart des applications.

## Canvas

L'élément `Canvas` permet de positionner le contenu selon des coordonnées _x-_ et _y-_ absolues. Les éléments peuvent être dessinés à un endroit unique ; ou, si les éléments occupent les mêmes coordonnées, l'ordre dans lequel ils apparaissent dans le balisage détermine l'ordre dans lequel les éléments sont dessinés.

`Canvas` offre le support de mise en page le plus flexible de tous les `Panel`. Les propriétés de Hauteur et de Largeur sont utilisées pour définir la zone du canevas, et les éléments à l'intérieur se voient assigner des coordonnées absolues par rapport à la zone du `Canvas` parent. Quatre propriétés attachées, `Canvas.Left`, `Canvas.Top`, `Canvas.Right` et `Canvas.Bottom`, permettent un contrôle précis du placement des objets au sein d'un `Canvas`, permettant au développeur de positionner et d'agencer les éléments avec précision à l'écran.

### ClipToBounds dans un Canvas

Le `Canvas` peut positionner des éléments enfants à n'importe quelle position sur l'écran, même à des coordonnées qui se trouvent en dehors de sa propre Hauteur et Largeur définies. De plus, le `Canvas` n'est pas affecté par la taille de ses enfants. En conséquence, il est possible qu'un élément enfant dessine d'autres éléments en dehors du rectangle englobant du `Canvas` parent. Le comportement par défaut d'un `Canvas` est de permettre aux enfants d'être dessinés en dehors des limites du `Canvas` parent. Si ce comportement est indésirable, la propriété `ClipToBounds` peut être définie sur `true`. Cela fait que le `Canvas` se limite à sa propre taille. Le `Canvas` est le seul élément de mise en page qui permet aux enfants d'être dessinés en dehors de ses limites.

### Définir et utiliser un Canvas

Un `Canvas` peut être instancié simplement en utilisant XAML ou du code. L'exemple suivant démontre comment utiliser `Canvas` pour positionner absolument le contenu. Ce code produit trois carrés de 100 pixels. Le premier carré est rouge, et sa position en haut à gauche (_x, y_) est spécifiée comme (0, 0). Le deuxième carré est vert, et sa position en haut à gauche est (100, 100), juste en dessous et à droite du premier carré. Le troisième carré est bleu, et sa position en haut à gauche est (50, 50), englobant ainsi le quadrant inférieur droit du premier carré et le quadrant supérieur gauche du deuxième. Comme le troisième carré est disposé en dernier, il semble être au-dessus des deux autres carrés, c'est-à-dire que les portions qui se chevauchent prennent la couleur de la troisième boîte.

<img className="center" src={CanvasOverlapScreenshot} alt="Exemple de StackPanel" />

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<Canvas Height="400" Width="400">
  <Canvas Height="100" Width="100" Top="0" Left="0" Background="Red"/>
  <Canvas Height="100" Width="100" Top="100" Left="100" Background="Green"/>
  <Canvas Height="100" Width="100" Top="50" Left="50" Background="Blue"/>
</Canvas>
```

</TabItem>
<TabItem value="cs">

```cs
// Créer le Canvas
monCanvasParent = new Canvas();
monCanvasParent.Width = 400;
monCanvasParent.Height = 400;

// Définir les éléments enfants du Canvas
monCanvas1 = new Canvas();
monCanvas1.Background = Brushes.Red;
monCanvas1.Height = 100;
monCanvas1.Width = 100;
Canvas.SetTop(monCanvas1, 0);
Canvas.SetLeft(monCanvas1, 0);

monCanvas2 = new Canvas();
monCanvas2.Background = Brushes.Green;
monCanvas2.Height = 100;
monCanvas2.Width = 100;
Canvas.SetTop(monCanvas2, 100);
Canvas.SetLeft(monCanvas2, 100);

monCanvas3 = new Canvas();
monCanvas3.Background = Brushes.Blue;
monCanvas3.Height = 100;
monCanvas3.Width = 100;
Canvas.SetTop(monCanvas3, 50);
Canvas.SetLeft(monCanvas3, 50);

// Ajouter des éléments enfants à la collection Children du Canvas
monCanvasParent.Children.Add(monCanvas1);
monCanvasParent.Children.Add(monCanvas2);
monCanvasParent.Children.Add(monCanvas3);
```
</TabItem>  

</Tabs>


## DockPanel

L'élément `DockPanel` utilise la propriété attachée `DockPanel.Dock` définie dans les éléments de contenu enfants pour positionner le contenu le long des bords d'un conteneur. Lorsque `DockPanel.Dock` est défini sur `Top` ou `Bottom`, il positionne les éléments enfants les uns au-dessus des autres. Lorsque `DockPanel.Dock` est défini sur `Left` ou `Right`, il positionne les éléments enfants à gauche ou à droite les uns des autres. La propriété `LastChildFill` détermine la position du dernier élément ajouté en tant qu'enfant d'un `DockPanel`.

Vous pouvez utiliser `DockPanel` pour positionner un groupe de contrôles liés, comme un ensemble de boutons. Alternativement, vous pouvez l'utiliser pour créer une interface utilisateur "panée".

### Dimensionnement au Contenu

Si ses propriétés `Height` et `Width` ne sont pas spécifiées, `DockPanel` s'adapte à son contenu. La taille peut augmenter ou diminuer pour s'adapter à la taille de ses éléments enfants. Cependant, lorsque ces propriétés sont spécifiées et qu'il n'y a plus de place pour le prochain élément enfant spécifié, `DockPanel` ne montre pas cet élément enfant ni les éléments enfants suivants et ne mesure pas les éléments enfants suivants.

### LastChildFill

Par défaut, le dernier enfant d'un élément `DockPanel` "remplira" l'espace restant non alloué. Si ce comportement n'est pas souhaité, définissez la propriété `LastChildFill` sur `false`.

### Définir et Utiliser un DockPanel

L'exemple suivant démontre comment partitionner l'espace en utilisant un `DockPanel`. Cinq éléments `Border` sont ajoutés en tant qu'enfants d'un `DockPanel` parent. Chacun utilise une propriété de positionnement différente d'un `DockPanel` pour partitionner l'espace. L'élément final "remplit" l'espace restant, non alloué.

<img className="center" src={DockPanelSampleScreenshot} alt="Exemple de StackPanel" />

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<DockPanel LastChildFill="True">
  <Border Height="25" Background="SkyBlue" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Top">
    <TextBlock Foreground="Black">Dock = "Haut"</TextBlock>
  </Border>
  <Border Height="25" Background="SkyBlue" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Top">
    <TextBlock Foreground="Black">Dock = "Haut"</TextBlock>
  </Border>
  <Border Height="25" Background="LemonChiffon" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Bottom">
    <TextBlock Foreground="Black">Dock = "Bas"</TextBlock>
  </Border>
  <Border Width="200" Background="PaleGreen" BorderBrush="Black" BorderThickness="1" DockPanel.Dock="Left">
    <TextBlock Foreground="Black">Dock = "Gauche"</TextBlock>
  </Border>
  <Border Background="White" BorderBrush="Black" BorderThickness="1">
    <TextBlock Foreground="Black">Ce contenu va "Remplir" l'espace restant</TextBlock>
  </Border>
</DockPanel>
```

</TabItem>
<TabItem value="cs">

```cs
// Créer le DockPanel
DockPanel monDockPanel = new DockPanel();
monDockPanel.LastChildFill = true;

// Définir le contenu enfant
Border maBordure1 = new Border();
maBordure1.Height = 25;
maBordure1.Background = Brushes.SkyBlue;
maBordure1.BorderBrush = Brushes.Black;
maBordure1.BorderThickness = new Thickness(1);
DockPanel.SetDock(maBordure1, Dock.Top);
TextBlock monTextBlock1 = new TextBlock();
monTextBlock1.Foreground = Brushes.Black;
monTextBlock1.Text = "Dock = Haut";
maBordure1.Child = monTextBlock1;

Border maBordure2 = new Border();
maBordure2.Height = 25;
maBordure2.Background = Brushes.SkyBlue;
maBordure2.BorderBrush = Brushes.Black;
maBordure2.BorderThickness = new Thickness(1);
DockPanel.SetDock(maBordure2, Dock.Top);
TextBlock monTextBlock2 = new TextBlock();
monTextBlock2.Foreground = Brushes.Black;
monTextBlock2.Text = "Dock = Haut";
maBordure2.Child = monTextBlock2;

Border maBordure3 = new Border();
maBordure3.Height = 25;
maBordure3.Background = Brushes.LemonChiffon;
maBordure3.BorderBrush = Brushes.Black;
maBordure3.BorderThickness = new Thickness(1);
DockPanel.SetDock(maBordure3, Dock.Bottom);
TextBlock monTextBlock3 = new TextBlock();
monTextBlock3.Foreground = Brushes.Black;
monTextBlock3.Text = "Dock = Bas";
maBordure3.Child = monTextBlock3;

Border maBordure4 = new Border();
maBordure4.Width = 200;
maBordure4.Background = Brushes.PaleGreen;
maBordure4.BorderBrush = Brushes.Black;
maBordure4.BorderThickness = new Thickness(1);
DockPanel.SetDock(maBordure4, Dock.Left);
TextBlock monBlocTexte4 = new TextBlock();
monBlocTexte4.Foreground = Brushes.Black;
monBlocTexte4.Text = "Dock = Left";
maBordure4.Child = monBlocTexte4;

Border maBordure5 = new Border();
maBordure5.Background = Brushes.White;
maBordure5.BorderBrush = Brushes.Black;
maBordure5.BorderThickness = new Thickness(1);
TextBlock monBlocTexte5 = new TextBlock();
monBlocTexte5.Foreground = Brushes.Black;
monBlocTexte5.Text = "Ce contenu remplira l'espace restant";
maBordure5.Child = monBlocTexte5;

// Ajouter des éléments enfants à la collection Children du DockPanel
monDockPanel.Children.Add(maBordure1);
monDockPanel.Children.Add(maBordure2);
monDockPanel.Children.Add(maBordure3);
monDockPanel.Children.Add(maBordure4);
monDockPanel.Children.Add(maBordure5);
```
</TabItem>  

</Tabs>

## Grille

L'élément `Grille` fusionne la fonctionnalité d'un positionnement absolu et d'un contrôle de données tabulaires. Une `Grille` vous permet de positionner et de styliser facilement des éléments. La `Grille` vous permet de définir des groupements de lignes et de colonnes flexibles, et fournit même un mécanisme pour partager des informations de taille entre plusieurs éléments `Grille`.

### Comportement de Dimensionnement des Colonnes et Lignes

Les colonnes et lignes définies dans une `Grille` peuvent tirer parti du dimensionnement `Étoile` afin de distribuer l'espace restant de manière proportionnelle. Lorsque `Étoile` est sélectionné comme Hauteur ou Largeur d'une ligne ou d'une colonne, cette colonne ou ligne reçoit une proportion pondérée de l'espace disponible restant. Cela contraste avec `Automatique`, qui distribuera l'espace uniformément en fonction de la taille du contenu à l'intérieur d'une colonne ou d'une ligne. Cette valeur est exprimée sous la forme `*` ou `2*` lors de l'utilisation de XAML. Dans le premier cas, la ligne ou la colonne recevrait une fois l'espace disponible, dans le second cas, deux fois, et ainsi de suite. En combinant cette technique pour distribuer proportionnellement l'espace avec une valeur `AlignementHorizontal` et `AlignementVertical` de `Étirer`, il est possible de partitionner l'espace de mise en page par pourcentage de l'espace d'écran. La `Grille` est le seul panneau de mise en page qui peut distribuer l'espace de cette manière.

### Définir et Utiliser une Grille

L'exemple suivant démontre comment construire une interface utilisateur similaire à celle trouvée dans la boîte de dialogue Exécuter disponible dans le menu Démarrer de Windows.

<img className="center" src={GridSampleScreenshot} alt="Exemple d'Application Grille" />

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<Grid Background="Gainsboro" 
      HorizontalAlignment="Left" 
      VerticalAlignment="Top" 
      Width="425" 
      Height="165"
      ColumnDefinitions="Auto,*,*,*,*"
      RowDefinitions="Auto,Auto,*,Auto">
    
    <Image Grid.Row="0" Grid.Column="0" Source="{Binding runicon}" />
    
    <TextBlock Grid.Row="0" Grid.Column="1" Grid.ColumnSpan="4" 
               Text="Tapez le nom d'un programme, d'un dossier, d'un document ou d'une ressource Internet, et Windows l'ouvrira pour vous." 
               TextWrapping="Wrap" />
               
    <TextBlock Grid.Row="1" Grid.Column="0" Text="Ouvrir :" />
    
    <TextBox Grid.Row="1" Grid.Column="1" Grid.ColumnSpan="5" />
    
    <Button Grid.Row="3" Grid.Column="2" Content="OK" Margin="10,0,10,15" />
    
    <Button Grid.Row="3" Grid.Column="3" Content="Annuler" Margin="10,0,10,15" />
    
    <Button Grid.Row="3" Grid.Column="4" Content="Parcourir..." Margin="10,0,10,15" />
</Grid>

```

</TabItem>
<TabItem value="cs">

```cs
// Créer la grille.
grid1 = new Grid ();
grid1.Background = Brushes.Gainsboro;
grid1.HorizontalAlignment = HorizontalAlignment.Left;
grid1.VerticalAlignment = VerticalAlignment.Top;
grid1.ShowGridLines = true;
grid1.Width = 425;
grid1.Height = 165;

// Définir les colonnes.
colDef1 = new ColumnDefinition();
colDef1.Width = new GridLength(1, GridUnitType.Auto);
colDef2 = new ColumnDefinition();
colDef2.Width = new GridLength(1, GridUnitType.Star);
colDef3 = new ColumnDefinition();
colDef3.Width = new GridLength(1, GridUnitType.Star);
colDef4 = new ColumnDefinition();
colDef4.Width = new GridLength(1, GridUnitType.Star);
colDef5 = new ColumnDefinition();
colDef5.Width = new GridLength(1, GridUnitType.Star);
grid1.ColumnDefinitions.Add(colDef1);
grid1.ColumnDefinitions.Add(colDef2);
grid1.ColumnDefinitions.Add(colDef3);
grid1.ColumnDefinitions.Add(colDef4);
grid1.ColumnDefinitions.Add(colDef5);

// Définir les lignes.
rowDef1 = new RowDefinition();
rowDef1.Height = new GridLength(1, GridUnitType.Auto);
rowDef2 = new RowDefinition();
rowDef2.Height = new GridLength(1, GridUnitType.Auto);
rowDef3 = new RowDefinition();
rowDef3.Height = new GridLength(1, GridUnitType.Star);
rowDef4 = new RowDefinition();
rowDef4.Height = new GridLength(1, GridUnitType.Auto);
grid1.RowDefinitions.Add(rowDef1);
grid1.RowDefinitions.Add(rowDef2);
grid1.RowDefinitions.Add(rowDef3);
grid1.RowDefinitions.Add(rowDef4);

// Ajouter l'image.
img1 = new Image();
img1.Source = runicon;
Grid.SetRow(img1, 0);
Grid.SetColumn(img1, 0);

// Ajouter la boîte de dialogue principale de l'application.
txt1 = new TextBlock();
txt1.Text = "Tapez le nom d'un programme, d'un dossier, d'un document ou d'une ressource Internet, et Windows l'ouvrira pour vous.";
txt1.TextWrapping = TextWrapping.Wrap;
Grid.SetColumnSpan(txt1, 4);
Grid.SetRow(txt1, 0);
Grid.SetColumn(txt1, 1);

// Ajouter la deuxième cellule de texte au Grid.
txt2 = new TextBlock();
txt2.Text = "Ouvrir :";
Grid.SetRow(txt2, 1);
Grid.SetColumn(txt2, 0);

// Ajouter le contrôle TextBox.
tb1 = new TextBox();
Grid.SetRow(tb1, 1);
Grid.SetColumn(tb1, 1);
Grid.SetColumnSpan(tb1, 5);

// Ajouter les boutons.
button1 = new Button();
button2 = new Button();
button3 = new Button();
button1.Content = "OK";
button2.Content = "Annuler";
button3.Content = "Parcourir...";
Grid.SetRow(button1, 3);
Grid.SetColumn(button1, 2);
button1.Margin = new Thickness(10, 0, 10, 15);
button2.Margin = new Thickness(10, 0, 10, 15);
button3.Margin = new Thickness(10, 0, 10, 15);
Grid.SetRow(button2, 3);
Grid.SetColumn(button2, 3);
Grid.SetRow(button3, 3);
Grid.SetColumn(button3, 4);

grid1.Children.Add(img1);
grid1.Children.Add(txt1);
grid1.Children.Add(txt2);
grid1.Children.Add(tb1);
grid1.Children.Add(button1);
grid1.Children.Add(button2);
grid1.Children.Add(button3);
```
</TabItem>  

</Tabs>


## StackPanel

Un `StackPanel` vous permet de "superposer" des éléments dans une direction assignée. La direction de superposition par défaut est verticale. La propriété `Orientation` peut être utilisée pour contrôler le flux de contenu.

### StackPanel vs. DockPanel

Bien que le `DockPanel` puisse également "superposer" des éléments enfants, le `DockPanel` et le `StackPanel` ne produisent pas des résultats analogues dans certains scénarios d'utilisation. Par exemple, l'ordre des éléments enfants peut affecter leur taille dans un `DockPanel`, mais pas dans un `StackPanel`. Cela est dû au fait que le `StackPanel` mesure dans la direction de la superposition à `PositiveInfinity`, tandis que le `DockPanel` ne mesure que la taille disponible.

### Définir et utiliser un StackPanel

L'exemple suivant démontre comment utiliser un `StackPanel` pour créer un ensemble de boutons positionnés verticalement. Pour un positionnement horizontal, définissez la propriété `Orientation` sur `Horizontal`.

<img className="center" src={StackPanelSampleScreenshot} alt="Exemple de StackPanel" />

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
 <StackPanel HorizontalAlignment="Center" 
                VerticalAlignment="Top"
                Spacing="25">
        <Button Content="Bouton 1" />
        <Button Content="Bouton 2" />
        <Button Content="Bouton 3" />
    </StackPanel>
```

</TabItem>
<TabItem value="cs">

```cs
// Définir le StackPanel
myStackPanel = new StackPanel();
myStackPanel.HorizontalAlignment = HorizontalAlignment.Center;
myStackPanel.VerticalAlignment = VerticalAlignment.Top;
myStackPanel.Spacing = 25;

// Définir le contenu enfant
Button monBouton1 = new Button();
monBouton1.Content = "Bouton 1";
Button monBouton2 = new Button();
monBouton2.Content = "Bouton 2";
Button monBouton3 = new Button();
monBouton3.Content = "Bouton 3";

// Ajouter des éléments enfants au StackPanel parent
myStackPanel.Children.Add(monBouton1);
myStackPanel.Children.Add(monBouton2);
myStackPanel.Children.Add(monBouton3);
```
</TabItem>  

</Tabs>


## WrapPanel

`WrapPanel` est utilisé pour positionner les éléments enfants de manière séquentielle de gauche à droite, en déplaçant le contenu à la ligne suivante lorsqu'il atteint le bord de son conteneur parent. Le contenu peut être orienté horizontalement ou verticalement. `WrapPanel` est utile pour des scénarios d'interface utilisateur simples et fluides. Il peut également être utilisé pour appliquer une taille uniforme à tous ses éléments enfants.

L'exemple suivant démontre comment créer un `WrapPanel` pour afficher des contrôles `Button` qui se replient lorsqu'ils atteignent le bord de leur conteneur.

<img className="center" src={WrapPanelSampleScreenshot} alt="Exemple de StackPanel" />

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<Border HorizontalAlignment="Left" VerticalAlignment="Top" BorderBrush="Black" BorderThickness="2">
  <WrapPanel Background="LightBlue" Width="200" Height="100">
    <Button Width="200">Bouton 1</Button>
    <Button>Bouton 2</Button>
    <Button>Bouton 3</Button>
    <Button>Bouton 4</Button>
  </WrapPanel>
</Border>
```

</TabItem>
<TabItem value="cs">

```cs
// Instancier un nouveau WrapPanel et définir les propriétés
monWrapPanel = new WrapPanel();
monWrapPanel.Background = System.Windows.Media.Brushes.Azure;
monWrapPanel.Orientation = Orientation.Horizontal;
monWrapPanel.Width = 200;
monWrapPanel.HorizontalAlignment = HorizontalAlignment.Left;
monWrapPanel.VerticalAlignment = VerticalAlignment.Top;

// Définir 3 éléments de bouton. Les trois derniers boutons ont une largeur 
// de 75, donc le quatrième bouton se déplace à la ligne suivante.
btn1 = new Button();
btn1.Content = "Bouton 1";
btn1.Width = 200;
btn2 = new Button();
btn2.Content = "Bouton 2";
btn2.Width = 75;
btn3 = new Button();
btn3.Content = "Bouton 3";
btn3.Width = 75;
btn4 = new Button();
btn4.Content = "Bouton 4";
btn4.Width = 75;

// Ajouter les boutons au WrapPanel parent en utilisant la méthode Children.Add.
monWrapPanel.Children.Add(btn1);
monWrapPanel.Children.Add(btn2);
monWrapPanel.Children.Add(btn3);
monWrapPanel.Children.Add(btn4);
```
</TabItem>  

</Tabs>


### Éléments de Panneau Imbriqués

Les éléments `Panel` peuvent être imbriqués les uns dans les autres afin de produire des mises en page complexes. Cela peut s'avérer très utile dans des situations où un `Panel` est idéal pour une partie d'une interface utilisateur, mais peut ne pas répondre aux besoins d'une autre partie de l'interface utilisateur.

Il n'y a pas de limite pratique à la quantité d'imbrication que votre application peut supporter, cependant, il est généralement préférable de limiter votre application à n'utiliser que les panneaux qui sont réellement nécessaires pour la mise en page souhaitée. Dans de nombreux cas, un élément `Grid` peut être utilisé à la place de panneaux imbriqués en raison de sa flexibilité en tant que conteneur de mise en page. Cela peut augmenter les performances de votre application en évitant d'inclure des éléments inutiles dans l'arborescence.

## UniformGrid

Le `UniformGrid` est un type de Panneau qui fournit une mise en page en grille uniforme. Cela signifie qu'il dispose ses enfants dans une grille où toutes les cellules de la grille ont la même taille. Contrairement au `Grid` standard, le `UniformGrid` ne prend pas en charge les lignes et colonnes explicites, ni ne fournit les propriétés attachées `Grid.Row` ou `Grid.Column`.

Le cas d'utilisation principal d'un `UniformGrid` est lorsque vous devez afficher une collection d'éléments dans un format de grille où chaque élément occupe une quantité d'espace égale.

### Propriétés de UniformGrid

* **Lignes et Colonnes** : Le `UniformGrid` utilise les propriétés `Rows` et `Columns` pour déterminer la disposition de ses éléments enfants. Si vous définissez seulement l'une de ces propriétés, le `UniformGrid` calculera automatiquement l'autre pour créer une grille qui s'adapte au nombre total d'éléments enfants. Si vous ne définissez aucune des propriétés, le `UniformGrid` par défaut est une grille 1x1.

Par exemple, si vous avez 12 éléments et que vous définissez `Rows` à 3, le `UniformGrid` créera automatiquement 4 colonnes. Si vous définissez `Columns` à 4, il créera automatiquement 3 lignes.

* **FirstColumn** : La propriété `FirstColumn` vous permet de laisser un certain nombre de cellules vides dans la première ligne de la grille.


### Définir et Utiliser un UniformGrid

L'exemple suivant démontre comment définir et utiliser un `UniformGrid`. L'exemple crée un `UniformGrid` avec 3 lignes et 4 colonnes et ajoute 12 rectangles comme éléments enfants.

<img className="center" src={UniformGridSampleScreenshot} alt="Exemple de StackPanel" />

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<UniformGrid Rows="3" Columns="4">
  <Rectangle Width="50" Height="50" Fill="#330000"/>
  <Rectangle Width="50" Height="50" Fill="#660000"/>
  <Rectangle Width="50" Height="50" Fill="#990000"/>
  <Rectangle Width="50" Height="50" Fill="#CC0000"/>
  <Rectangle Width="50" Height="50" Fill="#FF0000"/>
  <Rectangle Width="50" Height="50" Fill="#FF3300"/>
  <Rectangle Width="50" Height="50" Fill="#FF6600"/>
  <Rectangle Width="50" Height="50" Fill="#FF9900"/>
  <Rectangle Width="50" Height="50" Fill="#FFCC00"/>
  <Rectangle Width="50" Height="50" Fill="#FFFF00"/>
  <Rectangle Width="50" Height="50" Fill="#FFFF33"/>
  <Rectangle Width="50" Height="50" Fill="#FFFF66"/>
</UniformGrid>

```

</TabItem>
<TabItem value="cs">

```cs
// Créer le UniformGrid
UniformGrid monUniformGrid = new UniformGrid();
monUniformGrid.Rows = 3;
monUniformGrid.Columns = 4;

// Définir le contenu enfant
for (int i = 0; i < 12; i++)
{
    Rectangle monRectangle = new Rectangle();
    monRectangle.Fill = new SolidColorBrush(Color.FromRgb((byte)(i * 20), 0, 0));
    monRectangle.Width = 50;
    monRectangle.Height = 50;
    monUniformGrid.Children.Add(monRectangle);
}
```
</TabItem>  

</Tabs>

Dans l'exemple ci-dessus, chaque `Rectangle` est automatiquement assigné à une cellule de la grille dans l'ordre où ils ont été ajoutés.


