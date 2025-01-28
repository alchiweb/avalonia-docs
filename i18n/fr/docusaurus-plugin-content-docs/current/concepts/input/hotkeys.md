---
description: CONCEPTS - Input
---

# Clavier et Raccourcis

Divers contrôles qui implémentent `ICommandSource` ont une propriété `HotKey` que vous pouvez définir ou lier. Appuyer sur le raccourci exécutera la commande [liée](../../basics/user-interface/adding-interactivity#commands) au Contrôle.

```xml
<Menu>
    <MenuItem Header="_Fichier">
        <MenuItem x:Name="SaveMenuItem" Header="_Enregistrer" Command="{Binding SaveCommand}" HotKey="Ctrl+S"/>
    </MenuItem>
</Menu>
```

Vous pouvez également utiliser les méthodes statiques de la classe `HotKeyManager` pour définir et obtenir des raccourcis depuis le code :
```csharp
InitializeComponent();
HotKeyManager.SetHotKey(saveMenuItem, new KeyGesture(Key.S, KeyModifiers.Control));
```

## Touches et Modificateurs

Une touche de raccourci doit avoir une [Touche](http://reference.avaloniaui.net/api/Avalonia.Input/Key/) et zéro ou plusieurs [Modificateurs de Touche](http://reference.avaloniaui.net/api/Avalonia.Input/KeyModifiers/). Lors de la définition d'une touche de raccourci en XAML en utilisant la propriété `HotKey`, la chaîne sera analysée comme un [KeyGesture](http://reference.avaloniaui.net/api/Avalonia.Input/KeyGesture/). [Enum.Parse](https://docs.microsoft.com/en-us/dotnet/api/system.enum.parse) est utilisé pour analyser la touche et les modificateurs, mais des synonymes comme `Ctrl` au lieu de `Control` ou `Win` au lieu de `Meta` peuvent être utilisés.

## Assigner des touches numériques aux touches de raccourci

- Une touche de raccourci doit utiliser D1..D0 ou NumPad1..NumPad0.
  voir : [Touche](http://reference.avaloniaui.net/api/Avalonia.Input/Key/)
- En liant la même commande à deux boutons et en cachant un bouton, vous pouvez différencier entre un seul chiffre sur le pavé numérique et une simple touche Ctrl+chiffre.
- Si vous souhaitez limiter les dysfonctionnements de commande causés par la pression sur les chiffres du pavé numérique, vous pouvez également utiliser [Modificateurs de Touche](http://reference.avaloniaui.net/api/Avalonia.Input/KeyModifiers/).

```xml
<!--  Ça a bien fonctionné  -->
<!--  par exemple Ctrl+1  -->
<Button
    Command="{Binding CommandX}"
    Content="[1]"
    HotKey="Ctrl+D1" />
<!--  par exemple, vous pouvez également utiliser Alt+NumPad1  -->
<Button
    Command="{Binding CommandX}"
    HotKey="NumPad1"
    IsVisible="False" />

<!--  Ceux-ci n'ont pas fonctionné  -->
<!--  Alt+Numéro  -->
<Button Command="{Binding CommandX}" Content="_1" />
```

### Référence

* [HotKeyManager](http://reference.avaloniaui.net/api/Avalonia.Controls/HotKeyManager/)
* [KeyGesture](http://reference.avaloniaui.net/api/Avalonia.Input/KeyGesture/)
* [KeyModifiers](http://reference.avaloniaui.net/api/Avalonia.Input/KeyModifiers/)
* [Key](http://reference.avaloniaui.net/api/Avalonia.Input/Key/)

### Code source

* [HotkeyManager.cs](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Controls/HotkeyManager.cs)
* [KeyGesture.cs](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Input/KeyGesture.cs)
