---
id: how-to-implement-multi-page-apps
title: Comment implémenter des applications multi-pages
---


# Comment implémenter des applications multi-pages

Contenu en préparation.

Ce guide vous montrera comment utiliser des contrôles utilisateur comme vues de page et la classe de localisation de vue pour implémenter une application multi-pages.



qui est ajoutée par le modèle de solution Avalonia MVVM.

```csharp
public class ViewLocator : IDataTemplate
{
    public Control? Build(object? data)
    {
        if (data==null) return null;
        var name = data.GetType().FullName!.Replace("ViewModel", "View");
        var type = Type.GetType(name);

        if (type != null)
        {
            return (Control)Activator.CreateInstance(type)!;
        }

        return new TextBlock { Text = "Non trouvé : " + name };
    }

    public bool Match(object? data)
    {
        return data is ViewModelBase;
    }
}
```
