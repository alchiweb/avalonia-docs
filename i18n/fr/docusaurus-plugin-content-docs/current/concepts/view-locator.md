---
description: CONCEPTS
---

# Le Localisateur de Vue

Bien que l'utilisation du Localisateur de Vue fasse partie des modèles par défaut, il est important de noter que ce n'est pas une exigence obligatoire. C'est un outil optionnel fourni pour vous aider à structurer votre application Avalonia en utilisant le modèle de conception Model-View-ViewModel (MVVM).

Le Localisateur de Vue est un mécanisme dans Avalonia qui est utilisé pour résoudre la vue (interface utilisateur) qui correspond à un ViewModel spécifique. C'est une partie essentielle du modèle MVVM (Model-View-ViewModel), qui est un modèle de conception qui sépare le développement de l'interface utilisateur graphique du développement de la logique métier ou de la logique de back-end.

## Comment cela fonctionne

Le Localisateur de Vue utilise des conventions de nommage pour mapper les types de ViewModel aux types de vue. Par défaut, il remplace chaque occurrence de la chaîne "ViewModel" dans le nom de type de ViewModel entièrement qualifié par "View".

Par exemple, étant donné un ViewModel nommé `MyApplication.ViewModels.ExampleViewModel`, le Localisateur de Vue recherchera une Vue nommée `MyApplication.Views.ExampleView`.

Le Localisateur de Vue est généralement utilisé en conjonction avec la propriété `DataContext`, qui est utilisée pour lier une vue à son ViewModel.

Voici un exemple simple d'utilisation :

```cs
public class ViewLocator : IDataTemplate
{
    public bool SupportsRecycling => false;

    public Control Build(object data)
    {
        var name = data.GetType().FullName.Replace("ViewModel", "View");
        var type = Type.GetType(name);

        if (type != null)
        {
            return (Control)Activator.CreateInstance(type);
        }
        else
        {
            return new TextBlock { Text = "Non trouvé : " + name };
        }
    }

    public bool Match(object data)
    {
        return data is ViewModelBase;
    }
}
```

Dans cet exemple, le Localisateur de Vue est implémenté en tant qu'`IDataTemplate`. La méthode `Build` crée la vue pour le ViewModel, et la méthode `Match` vérifie si l'objet de données est un ViewModel que ce localisateur sait gérer. Si vous n'avez pas de classe `ViewModelBase`, votre ViewModel doit au minimum implémenter `INotifyPropertyChanged`, et la comparaison dans `Match` doit être modifiée en conséquence.

## Personnalisation du Localisateur de Vue

Vous pouvez personnaliser le Localisateur de Vue pour utiliser différentes conventions. Par exemple, vous pourriez vouloir rechercher des vues dans un autre assembly, ou utiliser une convention de nommage différente. Pour ce faire, vous pouvez implémenter votre propre Localisateur de Vue en créant une classe qui implémente l'interface `IDataTemplate`, et remplacer le Localisateur de Vue par défaut par le vôtre.

## Utilisation du Localisateur de Vue

Par défaut, le Localisateur de Vue est référencé dans App.axaml en tant que DataTemplate, dans le contenu de la balise XAML `Application.DataTemplates`. Assurez-vous que sa déclaration 'using' appropriée se trouve dans la propriété `xmlns:local` de la balise racine de l'Application.

```xaml
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="LearningAvalonia.App"
             xmlns:local="using:LearningAvalonia"
             RequestedThemeVariant="Default">
             <!-- Le thème "Default" suit la variante de thème du système. "Dark" ou "Light" sont d'autres options disponibles. -->
    <Application.DataTemplates>
        <local:ViewLocator />
    </Application.DataTemplates>

    <Application.Styles>
        <FluentTheme />
    </Application.Styles>
</Application>
```
