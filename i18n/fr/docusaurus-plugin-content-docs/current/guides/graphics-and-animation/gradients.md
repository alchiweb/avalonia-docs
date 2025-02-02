---
id: gradients
title: Comment utiliser les dégradés
---


# Comment utiliser les dégradés

Ce guide explique comment utiliser efficacement LinearGradientBrush dans Avalonia pour créer de beaux effets de dégradé.

## Syntaxe de base
Un LinearGradientBrush est défini en utilisant la structure de base suivante :

```xml
<LinearGradientBrush StartPoint="0%,0%" EndPoint="100%,0%">
    <GradientStop Color="#COLOR1" Offset="0.0"/>
    <GradientStop Color="#COLOR2" Offset="1.0"/>
</LinearGradientBrush>
```

## Propriétés clés

### StartPoint et EndPoint

* Définit la direction du dégradé
* Utilise des valeurs en pourcentage (par exemple, "0%,0%") ou des valeurs décimales (0,0)
* Modèles courants :
  * Horizontal : StartPoint="0%,50%" EndPoint="100%,50%"
  * Vertical : StartPoint="50%,0%" EndPoint="50%,100%"
  * Diagonal : StartPoint="0%,0%" EndPoint="100%,100%"

### Éléments GradientStop

* Définit les couleurs et leurs positions dans le dégradé
* Propriétés :
  * `Color` : La valeur de couleur (code Hex ou couleur nommée)
  * `Offset` : Position dans le dégradé (0.0 à 1.0)

## Modèles de dégradé courants

### 1. Dégradé horizontal simple

```xml
<LinearGradientBrush StartPoint="0%,50%" EndPoint="100%,50%">
    <GradientStop Color="#FF6B6B" Offset="0.0"/>
    <GradientStop Color="#4ECDC4" Offset="1.0"/>
</LinearGradientBrush>
```

### 2. Dégradé multicolore

```xml
<LinearGradientBrush StartPoint="0%,50%" EndPoint="100%,50%">
    <GradientStop Color="#FF6B6B" Offset="0.0"/>
    <GradientStop Color="#FF8E53" Offset="0.3"/>
    <GradientStop Color="#FF5E3A" Offset="0.6"/>
    <GradientStop Color="#4ECDC4" Offset="1.0"/>
</LinearGradientBrush>
```

### 3. Dégradé vertical

```xml
<LinearGradientBrush StartPoint="50%,0%" EndPoint="50%,100%">
    <GradientStop Color="#A8E6CF" Offset="0.0"/>
    <GradientStop Color="#3D84A8" Offset="1.0"/>
</LinearGradientBrush>
```

## Cas d'utilisation courants

### Arrière-plans de boutons

```xml
<Button>
    <Button.Background>
        <LinearGradientBrush StartPoint="0%,0%" EndPoint="0%,100%">
            <GradientStop Color="#4CAF50" Offset="0.0"/>
            <GradientStop Color="#45A049" Offset="1.0"/>
        </LinearGradientBrush>
    </Button.Background>
</Button>
```

### Arrière-plans de panneaux

```xml
<Border CornerRadius="8">
    <Border.Background>
        <LinearGradientBrush StartPoint="0%,0%" EndPoint="100%,100%">
            <GradientStop Color="#FF9A9E" Offset="0.0"/>
            <GradientStop Color="#FAD0C4" Offset="0.5"/>
            <GradientStop Color="#FFD1FF" Offset="1.0"/>
        </LinearGradientBrush>
    </Border.Background>
</Border>
```

## Exemple 

Voici le code pour reproduire l'échantillon suivant.

![Exemple de dégradé](../../../../../../static/img/guides/gradients/gradients.png)

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="SampleApp.MainWindow"
        Title="Gradient Example">
        
    <StackPanel Spacing="20" Margin="20">
        <!-- Dégradé horizontal avec plusieurs arrêts de couleur -->
        <Border Height="100" CornerRadius="8">
            <Border.Background>
                <LinearGradientBrush StartPoint="0%,50%" EndPoint="100%,50%">
                    <GradientStop Color="#FF6B6B" Offset="0.0"/>
                    <GradientStop Color="#FF8E53" Offset="0.3"/>
                    <GradientStop Color="#FF5E3A" Offset="0.6"/>
                    <GradientStop Color="#4ECDC4" Offset="1.0"/>
                </LinearGradientBrush>
            </Border.Background>
            <TextBlock Text="Dégradé horizontal"
                     HorizontalAlignment="Center"
                     VerticalAlignment="Center"
                     Foreground="White"/>
        </Border>

        <!-- Dégradé vertical avec des transitions douces -->
        <Border Height="100" CornerRadius="8">
            <Border.Background>
                <LinearGradientBrush StartPoint="50%,0%" EndPoint="50%,100%">
                    <GradientStop Color="#A8E6CF" Offset="0.0"/>
                    <GradientStop Color="#3D84A8" Offset="0.5"/>
                    <GradientStop Color="#46CDCF" Offset="1.0"/>
                </LinearGradientBrush>
            </Border.Background>
            <TextBlock Text="Dégradé vertical"
                     HorizontalAlignment="Center"
                     VerticalAlignment="Center"
                     Foreground="White"/>
        </Border>

        <!-- Dégradé diagonal avec plusieurs arrêt -->
        <Border Height="100" CornerRadius="8">
            <Border.Background>
                <LinearGradientBrush StartPoint="0%,0%" EndPoint="100%,100%">
                    <GradientStop Color="#FF9A9E" Offset="0.0"/>
                    <GradientStop Color="#FAD0C4" Offset="0.25"/>
                    <GradientStop Color="#FFB6C1" Offset="0.5"/>
                    <GradientStop Color="#FFD1FF" Offset="1.0"/>
                </LinearGradientBrush>
            </Border.Background>
            <TextBlock Text="Dégradé diagonal"
                     HorizontalAlignment="Center"
                     VerticalAlignment="Center"
                     Foreground="Black"/>
        </Border>

        <!-- Dégradé avec angle personnalisé et effet de cycle -->
        <Border Height="100" CornerRadius="8">
            <Border.Background>
                <LinearGradientBrush StartPoint="0%,0%" EndPoint="100%,50%">
                    <GradientStop Color="#08AEEA" Offset="0.0"/>
                    <GradientStop Color="#2AF598" Offset="0.3"/>
                    <GradientStop Color="#08AEEA" Offset="0.6"/>
                    <GradientStop Color="#2AF598" Offset="1.0"/>
                </LinearGradientBrush>
            </Border.Background>
            <TextBlock Text="Dégradé avec angle personnalisé"
                     HorizontalAlignment="Center"
                     VerticalAlignment="Center"
                     Foreground="White"/>
        </Border>
    </StackPanel>
</Window>
```