---
id: input-controls
title: Contrôles de Saisie
---
import useBaseUrl from '@docusaurus/useBaseUrl';

Sur cette page, vous apprendrez à ajouter des contrôles d'entrée et à les organiser dans une mise en page bien alignée. L'objectif est d'ajouter des entrées numériques avec des étiquettes et un contrôle de sortie en dessous.

Pour réaliser cette mise en page, vous utiliserez le contrôle `Grid` intégré pour créer des cellules et assigner nos contrôles à ces cellules.

L'image suivante montre l'application terminée en cours d'exécution avec les lignes de grille visibles à des fins de visualisation de la mise en page. Normalement, celles-ci sont invisibles sur une interface utilisateur de production.

<ThemedImage
        alt="StackPanel de Température"
        className="center"
        sources={{
            light: useBaseUrl('/img/get-started/test-drive/input-controls-light.png'),
            dark: useBaseUrl('/img/get-started/test-drive/input-controls-dark.png'),
        }}
        />

Pour créer une mise en page en utilisant le contrôle `Grid` avec 2 colonnes et 3 lignes, suivez cette procédure :

- Arrêtez l'application si elle est en cours d'exécution.
- Localisez la ligne vide dans le XAML entre `<Border>` et `<Button>`
- Insérez une balise `<Grid>` comme indiqué :

```xml
<StackPanel>
    <Border Margin="5" CornerRadius="10" Background="LightBlue">
        <TextBlock Margin="5"
            HorizontalAlignment="Center"
            FontSize="24"
            Text="Convertisseur de Température" />
    </Border>
    // highlight-start
    <Grid ShowGridLines="True" Margin="5" 
          ColumnDefinitions="120, 100"
          RowDefinitions="Auto, Auto, Auto">
    </Grid>
    // highlight-end
    <Button HorizontalAlignment="Center">Calculer</Button>
</StackPanel>
```

Cela assigne le nombre de lignes et de colonnes, leurs tailles et rend les lignes de grille visibles. Actuellement, cela apparaîtra comme une ligne droite car les cellules de la grille sont vides. Les lignes de taille `Auto` s'ajustent à leur contenu et auront une hauteur nulle jusqu'à ce que du contenu soit ajouté.

- Ajoutez des contrôles `<Label>` et `<TextBox>` (entrée de texte) aux enfants du `Grid` comme indiqué :

```xml
<Grid ShowGridLines="True" Margin="5"
      ColumnDefinitions="120, 100" 
      RowDefinitions="Auto, Auto, Auto">
    // highlight-start
    <Label Grid.Row="0" Grid.Column="0">Celsius</Label>
    <TextBox Grid.Row="0" Grid.Column="1"/>
    <Label Grid.Row="1" Grid.Column="0">Fahrenheit</Label>
    <TextBox Grid.Row="1"  Grid.Column="1"/>
    // highlight-end
</Grid>
```

Pour compléter la mise en page, nettoyez l'alignement des contrôles dans le `Grid` en utilisant leur propriété `Margin`. Déplacez également le `Button` à l'intérieur du `Grid`.

```xml
<Grid ShowGridLines="True"  Margin="5" 
      ColumnDefinitions="120, 100" 
      RowDefinitions="Auto, Auto, Auto">
    // highlight-start
    <Label Grid.Row="0" Grid.Column="0" Margin="10">Celsius</Label>
    <TextBox Grid.Row="0" Grid.Column="1" Margin="0 5" Text="0"/>
    <Label Grid.Row="1" Grid.Column="0" Margin="10">Fahrenheit</Label>
    <TextBox Grid.Row="1"  Grid.Column="1" Margin="0 5" Text="0"/>
    <Button Grid.Row="2"  Grid.Column="1">Calculer</Button>
    // highlight-end
</Grid>
```

- Exécutez l'application pour voir le résultat

:::info
Pour des informations complètes sur l'ensemble des contrôles intégrés d'Avalonia et leurs attributs, consultez la section de référence [ici](../../reference/controls/).
:::

Sur la page suivante, vous verrez comment améliorer votre expérience de conception en ajustant la taille de la fenêtre lorsqu'elle est affichée dans le panneau d'aperçu.
