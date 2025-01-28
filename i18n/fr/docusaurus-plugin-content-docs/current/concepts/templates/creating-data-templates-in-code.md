---
description: CONCEPTS - Modèles de données
---

# Création dans le Code

_Avalonia UI_ prend en charge la création d'un modèle de données dans le code. Vous pouvez le faire en utilisant la classe `FuncDataTemplate<T>` qui prend en charge l'interface `IDataTemplate`.

Dans sa forme la plus simple, vous pouvez créer un modèle de données en passant une fonction lambda qui crée un contrôle au constructeur `FuncDataTemplate<T>`, comme ceci :

```csharp
var template = new FuncDataTemplate<Student>((value, namescope) =>
    new TextBlock
    {
        [!TextBlock.TextProperty] = new Binding("FirstName"),
    });
```

Ce qui est équivalent au XAML :

```xml
<DataTemplate DataType="{x:Type local:Student}">
    <TextBlock Text="{Binding FirstName}"/>
</DataTemplate>
```

## Plus d'Exemples

:::info
Jetez un œil à quelques utilisations plus avancées de la classe `FuncDataTemplate<T>` dans le projet d'exemple _Avalonia UI_ [ici](https://github.com/AvaloniaUI/Avalonia.Samples/blob/main/src/Avalonia.Samples/DataTemplates/FuncDataTemplateSample).
:::
