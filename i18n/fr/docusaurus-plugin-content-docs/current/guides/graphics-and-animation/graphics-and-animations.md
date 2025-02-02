---
id: graphics-and-animations
title: Comment dessiner des graphiques
---

import ShapeAndGeometrySampleScreenshot from '/img/guides/graphics-and-animations/shapes-and-geometry.png';

# Comment dessiner des graphiques

Contenu en préparation.

## Graphiques

Avalonia introduit un ensemble de fonctionnalités graphiques étendu, évolutif et flexible qui présente les avantages suivants :

* Graphiques indépendants de la résolution et de l'appareil. L'unité de mesure de base dans le système graphique Avalonia est le pixel indépendant de l'appareil, qui équivaut à 1/96 de pouce, quelle que soit la résolution d'écran réelle, et constitue la base pour un rendu indépendant de la résolution et de l'appareil. Chaque pixel indépendant de l'appareil s'adapte automatiquement pour correspondre au paramètre de points par pouce (dpi) du système sur lequel il est rendu.
* Précision améliorée. Le système de coordonnées Avalonia est mesuré avec des nombres à virgule flottante en double précision plutôt qu'en simple précision. Les transformations et les valeurs d'opacité sont également exprimées en double précision.
* Support avancé des graphiques et de l'animation. Avalonia simplifie la programmation graphique en gérant les scènes d'animation pour vous ; il n'est pas nécessaire de se soucier du traitement des scènes, des boucles de rendu et de l'interpolation bilinéaire. De plus, Avalonia fournit un support de test de collision et un support complet de composition alpha.
* Skia. Par défaut, Avalonia utilise le [moteur de rendu Skia](https://skia.org/), le même moteur de rendu qui alimente Google Chrome et Chrome OS, Android, Mozilla Firefox et Firefox OS, ainsi que de nombreux autres produits.

## Formes et géométries 2D

Avalonia fournit une bibliothèque de formes 2D courantes dessinées en vecteurs telles que `Ellipse`, `Line`, `Path`, `Polygon` et `Rectangle`.

```xml
<Canvas Background="Yellow" Width="300" Height="400">
    <Rectangle Fill="Blue" Width="63" Height="41" Canvas.Left="40" Canvas.Top="31">
        <Rectangle.OpacityMask>
            <LinearGradientBrush StartPoint="0%,0%" EndPoint="100%,100%">
                <LinearGradientBrush.GradientStops>
                    <GradientStop Offset="0" Color="Black"/>
                    <GradientStop Offset="1" Color="Transparent"/>
                </LinearGradientBrush.GradientStops>
            </LinearGradientBrush>
        </Rectangle.OpacityMask>     
    </Rectangle>
    <Ellipse Fill="Green" Width="58" Height="58" Canvas.Left="88" Canvas.Top="100"/>
    <Path Fill="Orange" Data="M 0,0 c 0,0 50,0 50,-50 c 0,0 50,0 50,50 h -50 v 50 l -50,-50 Z" Canvas.Left="30" Canvas.Top="250"/>
    <Path Fill="OrangeRed" Canvas.Left="180" Canvas.Top="250">
        <Path.Data>
            <PathGeometry>
                <PathFigure StartPoint="0,0" IsClosed="True">
                    <QuadraticBezierSegment Point1="50,0" Point2="50,-50" />
                    <QuadraticBezierSegment Point1="100,-50" Point2="100,0" />
                    <LineSegment Point="50,0" />
                    <LineSegment Point="50,50" />
                </PathFigure>
            </PathGeometry>
        </Path.Data>
    </Path>
    <Line StartPoint="120,185" EndPoint="30,115" Stroke="Red" StrokeThickness="2"/>
    <Polygon Points="75,0 120,120 0,45 150,45 30,120" Stroke="DarkBlue" StrokeThickness="1" Fill="Violet" Canvas.Left="150" Canvas.Top="31"/>
    <Polyline Points="0,0 65,0 78,-26 91,39 104,-39 117,13 130,0 195,0" Stroke="Brown" Canvas.Left="30" Canvas.Top="350"/>
</Canvas>
```

<img src={ShapeAndGeometrySampleScreenshot} alt=''/>

## Ajouter des Animations

Avalonia UI prend en charge les animations, vous permettant de faire grandir, secouer, tourner et estomper des contrôles, pour créer des transitions de page intéressantes, et plus encore. Avalonia utilise un système d'animation similaire à CSS qui prend en charge les [transitions de propriété](transitions.md) et les [animations par images clés](keyframe-animations.md).
