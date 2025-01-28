# Contrôles intégrés

Voici quelques-uns des contrôles Avalonia les plus couramment utilisés, organisés par catégorie :

## Contrôles de mise en page

| Contrôle                                                                       | Description |
|:-------------------------------------------------------------------------------|:----|
| [Border](../../../reference/controls/border.md)                                | Décore un seul enfant avec une bordure et un arrière-plan. |
| [Canvas](../../../reference/controls/canvas.md)                                | Affiche les contrôles enfants à des positions spécifiées. |
| [DockPanel](../../../reference/controls/dockpanel.md)                         | Arrange les contrôles enfants le long des bords spécifiés (haut, bas, gauche, droite) avec un remplissant tout l'espace restant. |
| [Expander](../../../reference/controls/expander.md)                            | A une zone d'en-tête (toujours visible) et une section de contenu réductible (un seul enfant). |
| [Grid](../../../reference/controls/grid/README.md)                             | Arrange les contrôles enfants dans les cellules d'une grille, positionnés par ligne et colonne. Les cellules peuvent s'étendre sur des lignes et des colonnes. |
| [GridSplitter](../../../reference/controls/gridsplitter.md)                   | Peut être ajouté à une grille pour permettre à l'utilisateur de redimensionner les lignes ou les colonnes à l'exécution. |
| [Panel](../../../reference/controls/panel.md)                                  | Empile les contrôles enfants les uns sur les autres. |
| [RelativePanel](../../../reference/controls/relativepanel.md)                 | Permet plusieurs contrôles enfants. La position et l'alignement des contrôles enfants peuvent être spécifiés par rapport au panneau lui-même, ou par rapport à d'autres contrôles enfants. La taille des contrôles enfants peut être spécifiée ou calculée à partir des relations et des alignements.|
| [ScrollViewer](../../../reference/controls/scrollviewer.md)                   | Ajoute des barres de défilement et un comportement de défilement si l'enfant (unique) est plus grand que l'espace disponible.|
| [SplitView](../../../reference/controls/splitview.md)                         | Ajoute un panneau réductible au bord de sa zone de contenu (enfant unique).|
| [StackPanel](../../../reference/controls/stackpanel.md)                       | Permet plusieurs contrôles enfants, disposés en séquence, horizontalement ou verticalement.|
| [TabControl](../../../reference/controls/tabcontrol.md)    | Le contrôle d'onglets vous permet de subdiviser une vue en éléments d'onglet.|
| [UniformGrid](../../../reference/controls/uniform-grid.md) | Permet plusieurs contrôles enfants, disposés dans une grille avec des cellules de taille de colonne et de ligne uniforme. |
| [WrapPannel](../../../reference/controls/wrappanel.md) | Dispose les contrôles enfants en séquence de gauche à droite, tant qu'ils tiennent dans la largeur. Commence une nouvelle ligne lorsqu'il n'y a plus d'espace. |

## Boutons

| Contrôle | Description |
|:----|:----|
| [Button](../../../reference/controls/buttons/button.md) | Le contrôle de bouton de base - peut afficher du texte, une icône ou les deux. A un comportement standard de 'clic'. |
| [RepeatButton](../../../reference/controls/buttons/repeatbutton.md)|Un bouton qui déclenche son événement de clic de manière répétée lorsqu'il est pressé et maintenu.|
| [RadioButton](../../../reference/controls/buttons/radiobutton.md)|Un bouton qui a un état sélectionné. Il peut être placé dans un groupe afin que la sélection d'un bouton désélectionne tous les autres dans le groupe.|
| [ToggleButton](../../../reference/controls/buttons/togglebutton.md)|Un bouton qui a un état sélectionné et un état non sélectionné. Les clics successifs 'alterner' cet état. Une pseudo-classe 'checked' permet d'attribuer différents styles aux états sélectionné et non sélectionné.|
| [ButtonSpinner](../../../reference/controls/buttons/buttonspinner.md)|Un contrôle avec deux boutons de rotation et une zone de contenu.|
| [SplitButton](../../../reference/controls/buttons/splitbutton.md)|Cela fonctionne comme un bouton avec des parties primaire et secondaire qui peuvent être pressées indépendamment. La partie primaire se comporte comme un bouton standard, et la partie secondaire ouvre un menu déroulant avec des actions supplémentaires.|
| [ToggleSplitButton](../../../reference/controls/buttons/togglesplitbutton.md)|Cela fonctionne comme un bouton avec des parties primaire et secondaire qui peuvent être pressées indépendamment. La partie primaire se comporte comme un bouton à bascule, et la partie secondaire ouvre un menu déroulant avec des actions supplémentaires.|

## Contrôles de données répétitives

Ces contrôles affichent des données répétitives, soit sous forme tabulaire, soit sous forme de liste :

|Contrôle|Description|
|:----|:----|
|[DataGrid](../../../reference/controls/datagrid)|Affiche des données dans une grille personnalisable.|
|[ItemsControl](../../../reference/controls/itemscontrol.md)|Affiche une collection d'éléments à partir d'une source de données liée.|
|[ItemsRepeater](../../../reference/controls/itemsrepeater.md)|Affiche des données répétées à partir d'une source de données liée. Il a à la fois un modèle de mise en page et un modèle de données.|
|[ListBox](../../../reference/controls/listbox.md)|Un contrôle avec des éléments qui peuvent être sélectionnés.|
|[ComboBox](../../../reference/controls/combobox.md)|Un contrôle avec une liste déroulante contenant des éléments qui peuvent être sélectionnés.|

## Affichage et édition de texte

|Contrôle|Description|
|:----|:----|
|[AutoCompleteBox](../../../reference/controls/autocompletebox.md)|Un contrôle qui affiche une zone de texte pour la saisie de l'utilisateur et une liste déroulante contenant des correspondances possibles en fonction de ce qui a été tapé.|
|[TextBlock](../../../reference/controls/textblock.md)|Un contrôle qui affiche un bloc de texte. En lecture seule.|
|[TextBox](../../../reference/controls/textbox.md)|Utilisé pour afficher ou éditer du texte sans restrictions de formatage.|
|[SelectableTextBlock](../../../reference/controls/selectable-textblock.md)|Utilisé pour afficher ou éditer du texte sans restrictions de formatage. Permet de sélectionner et de copier.|
|[MaskedTextBox](../../../reference/controls/maskedtextbox.md)|Utilisé pour afficher du texte dans le format contenu dans un masque ; ou utilisé pour éditer du texte en utilisant le masque de format pour empêcher une saisie utilisateur invalide.|

## Sélection de valeur

|Contrôle|Type|Description|
|:----|:----|:----|
|[CheckBox](../../../reference/controls/checkbox.md)|Booléen|Valeur vraie présentée sous forme de coche. L'interaction par clic bascule la valeur. Possède une option pour afficher une valeur 'inconnue'.|
|[Slider](../../../reference/controls/slider.md)|Double|Valeur relative par rapport à une valeur maximale et minimale présentée comme la position le long de la longueur de la piste du bouton du curseur. L'interaction de glisser sur le bouton du curseur peut modifier la valeur entre les valeurs maximales et minimales. Les interactions au clavier et par clic peuvent également ajuster la valeur.|
|[Calendar](../../../reference/controls/calendar)|DateTime|Le calendrier est un contrôle permettant aux utilisateurs de sélectionner des dates ou des plages de dates.|
|[CalendarDatePicker](../../../reference/controls/calendar/calendar-date-picker.md)|DateTime|Une extension du contrôle de calendrier qui inclut une zone de texte et un bouton.|
|[ColorPicker](../../../reference/controls/colorpicker)|Couleur / HsvColor|Le sélecteur de couleur prend en charge la sélection et l'édition des couleurs par l'utilisateur à l'aide d'un spectre, d'une palette et de curseurs de composants. Il prend également en charge un composant alpha optionnel, des modèles de couleur RVB ou HSV et des valeurs de couleur hexadécimales.|
|[DatePicker](../../../reference/controls/datepicker.md)|DateTime|Le sélecteur de date dispose de trois contrôles 'spinner' permettant à l'utilisateur de choisir une valeur de date.|
|[TimePicker](../../../reference/controls/timepicker.md)|TimeSpan|Le sélecteur de temps dispose de trois contrôles 'spinner' permettant à l'utilisateur de choisir une heure parmi les heures, les minutes et les secondes.|

## Affichage des images

|Contrôle|Description|
|:----|:----|
|[Image](../../../reference/controls/image.md)|Affiche une image bitmap ou vectorielle.|
|[PathIcon](../../../reference/controls/path-icon.md)|Dessine une image vectorielle en utilisant le `Foreground` actuel.|

## Menus et Popups

|Contrôle|Description|
|:----|:----|
|[Menu](../../../reference/controls/menu.md)|Affiche un menu d'application.|
|[Flyouts](../../../reference/controls/flyouts.md)|Attache un popup ou un menu contextuel à un contrôle.|
|[ToolTip](../../../reference/controls/tooltip.md)|Affiche une info-bulle lorsqu'un contrôle est survolé.|
