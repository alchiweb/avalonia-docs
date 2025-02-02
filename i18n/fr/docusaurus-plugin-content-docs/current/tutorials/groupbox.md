---
id: groupbox
title: Create a GroupBox Using HeaderedContentControl
sidebar_label: Créer un GroupBox
---

import groupboxscreenshot from '/img/tutorials/groupbox/groupbox.png';


Bien qu'Avalonia n'inclue pas de contrôle `GroupBox` intégré, vous pouvez obtenir la même fonctionnalité et apparence en utilisant un `HeaderedContentControl` avec un style personnalisé. Le `HeaderedContentControl` fournit une zone d'en-tête et une région de contenu, ce qui le rend parfait pour regrouper des éléments d'interface connexes.

<GitHubSampleLink title="Custom GroupBox" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/GroupBox"/>

## Mise en œuvre

Ajoutez le style suivant à vos ressources d'application (généralement dans App.axaml) ou à la `Window` ou `UserControl` spécifique où vous avez besoin de la fonctionnalité `GroupBox` :

```xml title='XAML'
<Style Selector="HeaderedContentControl">
    <Setter Property="Template">
        <ControlTemplate>
            <Grid>
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto"/>
                    <RowDefinition Height="*"/>
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto"/>
                    <ColumnDefinition Width="*"/>
                </Grid.ColumnDefinitions>
        
                <!-- En-tête -->
                <Border 
                    ZIndex="1" 
                    Background="{DynamicResource SystemControlBackgroundAltHighBrush}" 
                    Padding="5,0,5,0" 
                    Margin="5,0,0,0">
                    <TextBlock 
                        Text="{TemplateBinding Header}" 
                        FontWeight="Bold"/>
                </Border>
        
                <!-- Zone de contenu -->
                <Border 
                    Grid.RowSpan="2" 
                    Padding="0,5,0,0"
                    Grid.ColumnSpan="2"
                    CornerRadius="4"
                    Margin="0,10,0,0"
                    BorderBrush="{DynamicResource SystemControlForegroundBaseMediumBrush}"
                    BorderThickness="1">
                    <ContentPresenter 
                        Name="PART_ContentPresenter"
                        Padding="8"
                        Content="{TemplateBinding Content}"/>
                </Border>
            </Grid>
        </ControlTemplate>
    </Setter>
</Style>
```

Une fois le style en place, vous pouvez utiliser le `HeaderedContentControl` dans votre XAML :

```xml title='XAML'
<HeaderedContentControl Header="Settings">
    <StackPanel Spacing="8">
        <TextBox Text="Sample content"/>
        <Button Content="Click me"/>
    </StackPanel>
</HeaderedContentControl>
```

<img className="center" src={groupboxscreenshot} width="200"/>

Le style utilise les ressources de thème d'Avalonia pour garantir que le contrôle a un aspect approprié dans les thèmes clairs et sombres. Le texte de l'en-tête semble "casser" la ligne de bordure en utilisant une couleur de fond correspondant à celle de la fenêtre, créant ainsi l'apparence classique d'un `GroupBox`. La zone de contenu présente des coins arrondis et un rembourrage approprié pour un look moderne.

Cette mise en œuvre offre tous les avantages visuels et fonctionnels d'un `GroupBox` traditionnel tout en maintenant la cohérence avec les modèles de conception et le système de thèmes d'Avalonia.