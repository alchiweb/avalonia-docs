---
description: CONCEPTS - ReactiveUI
---

import ReactiveObjectDiagram from '/img/concepts/reactiveui/reactiveobject.png';

# Modèle de Vue Réactif

Cette page décrit comment vous pouvez utiliser le `ReactiveObject` de _ReactiveUI_ comme base de votre modèle de vue pour implémenter le binding MVVM avec _Avalonia UI_.

_ReactiveUI_ fournit le `ReactiveObject` comme classe de base pour les modèles de vue. Il implémente une notification des changements de propriété et des observables pour surveiller les changements d'objet.

:::info
Pour la documentation détaillée de _ReactiveUI_ pour `ReactiveObject`, voir [ici](https://www.reactiveui.net/api/reactiveui/reactiveobject/).
:::

Une fois que vous avez installé et configuré _ReactiveUI_, vous pouvez baser vos modèles de vue sur la classe :

```csharp
public class ViewModelBase : ReactiveObject
{
}
```

<img src={ReactiveObjectDiagram} alt=""/>

:::info
Si vous avez utilisé le modèle de solution d'application MVVM Avalonia, vous trouverez déjà cette classe de base ajoutée au dossier /ViewModels du projet.
:::

Par exemple, vous pouvez implémenter un modèle de vue simple comme ceci :

```csharp
public class MyViewModel : ViewModelBase
{
   private string _description = string.Empty;
   public string Description
   {
      get => _description;
      set => this.RaiseAndSetIfChanged(ref _description, value);
   }
}
```

## Notifier la Vue des Changements

_Avalonia UI_ utilise le `ReactiveObject` sous-jacent pour **Notifier** les changements dans le modèle de vue à la vue en utilisant les bindings définis dans le XAML. Par exemple, si vous liez le contrôle de saisie de texte d'_Avalonia UI_ comme ceci :

```xml
<TextBox AcceptsReturn="True"
         Text="{Binding Description}"
         Watermark="Entrez une description"/>
```

Tout changement de la propriété description du modèle de vue est réalisé en utilisant l'accesseur `set` et un changement est déclenché, ce qui amène _Avalonia UI_ à afficher la nouvelle valeur dans l'interface utilisateur.

## Mettre à Jour le Modèle de Vue à Partir de l'Entrée

Lorsque _Avalonia UI_ utilise le binding pour **Mettre à Jour** le modèle de vue, l'accesseur `set` garantit que toutes les parties du modèle de vue qui dépendent de la propriété description peuvent également réagir au changement si nécessaire.

Sur la page suivante, vous apprendrez comment une commande réactive agit comme un cas particulier de mise à jour du modèle de vue.
