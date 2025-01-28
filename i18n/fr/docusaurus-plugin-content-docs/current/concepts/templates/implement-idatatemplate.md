---
description: CONCEPTS - Modèles de données
---

# Prendre Plus de Contrôle dans le Code

Si vous avez besoin de prendre plus de contrôle sur un modèle de données dans le code, vous pouvez écrire une classe qui implémente vous-même l'interface `IDataTemplate`. Cela vous permettra de présenter les propriétés de votre type de données lié de la manière que vous souhaitez.

Pour utiliser l'interface `IDataTemplate`, vous devez implémenter les deux membres suivants dans votre classe de modèle de données :

* `public bool Match(object data) { ... }` - implémentez ce membre pour vérifier si les données liées fournies correspondent à votre `IDataTemplate` ou non. Retournez vrai si le type de données lié correspond, sinon faux.
* `public Control Build(object param) { ... }` - implémentez ce membre pour construire et retourner le contrôle qui présentera vos données.

## Exemple

Ceci est une simple implémentation de l'interface `IDataTemplate` pour afficher des données de chaîne dans un bloc de texte :

```csharp
using Avalonia.Controls.Templates;
...
public class MyDataTemplate : IDataTemplate
{
    public Control Build(object param)
    {
        return new TextBlock() { Text = (string)param };
    }

    public bool Match(object data)
    {
        return data is string;
    }
}
```

Vous pouvez maintenant utiliser la classe `MyDataTemplate` dans votre vue, comme ceci :

```xml
<!-- xmlns:dataTemplates="using:MyApp.DataTemplates" -->

<ContentControl Content="{Binding MyContent}">
	<ContentControl.ContentTemplate>
		<dataTemplates:MyDataTemplate />
	</ContentControl.ContentTemplate>
</ContentControl>
```

## Plus d'Exemples

:::info
Jetez un œil à quelques implémentations plus avancées de l'interface `IDataTemplate` dans le projet d'exemple _Avalonia UI_ [ici](https://github.com/AvaloniaUI/Avalonia.Samples/tree/main/src/Avalonia.Samples/DataTemplates/IDataTemplateSample).
:::
