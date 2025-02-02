---
id: how-to-create-templated-controls
title: Comment créer des contrôles avec des modèles
---


# Comment créer des contrôles avec des modèles

## Liaison de données

Lorsque vous créez un modèle de contrôle et que vous souhaitez lier au parent modélisé, vous pouvez utiliser :

```xml
<TextBlock Name="tb" Text="{TemplateBinding Caption}"/>

<!-- Qui est la même chose que : -->
<TextBlock Name="tb" Text="{Binding Caption, RelativeSource={RelativeSource TemplatedParent}}"/>
```

Bien que les deux syntaxes montrées ici soient équivalentes dans la plupart des cas, il existe quelques différences :

1.  `TemplateBinding` n'accepte qu'une seule propriété plutôt qu'un chemin de propriété, donc si vous souhaitez lier en utilisant un chemin de propriété, vous devez utiliser la deuxième syntaxe :

    ```xml
    <!-- Cela ne fonctionnera PAS car TemplateBinding n'accepte que des propriétés uniques -->
    <TextBlock Name="tb" Text="{TemplateBinding Caption.Length}"/>

    <!-- Au lieu de cela, cette syntaxe doit être utilisée dans ce cas -->
    <TextBlock Name="tb" Text="{Binding Caption.Length, RelativeSource={RelativeSource TemplatedParent}}"/>
    ```
2.  Un `TemplateBinding` ne prend en charge que le mode `OneWay` pour des raisons de performance (c'est le [même que WPF](https://docs.microsoft.com/en-us/dotnet/desktop/wpf/advanced/templatebinding-markup-extension#remarks)). Cela signifie qu'un `TemplateBinding` est en réalité équivalent à `{Binding RelativeSource={RelativeSource TemplatedParent}, Mode=OneWay}`. Si un binding `TwoWay` est nécessaire dans un modèle de contrôle, la syntaxe complète est requise comme indiqué ci-dessous. Notez que `Binding` utilisera également le mode de liaison par défaut contrairement à `TemplateBinding`.

    ```markup
    {Binding RelativeSource={RelativeSource TemplatedParent}, Mode=TwoWay}
    ```
3.  `TemplateBinding` ne peut être utilisé que sur `IStyledElement`.

```xml
<!-- Cela ne fonctionnera PAS car GeometryDrawing n'est pas un IStyledElement. -->
<GeometryDrawing Brush="{TemplateBinding Foreground}"/>

<!-- Au lieu de cela, cette syntaxe doit être utilisée dans ce cas. -->
<GeometryDrawing Brush="{Binding Foreground, RelativeSource={RelativeSource TemplatedParent}}"/>
```

