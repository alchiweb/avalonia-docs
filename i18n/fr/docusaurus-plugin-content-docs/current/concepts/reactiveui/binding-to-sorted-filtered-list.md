---
description: CONCEPTS - ReactiveUI
---

# Liaison à des données triées/filtrées

Une tâche d'interface utilisateur courante que les applications doivent accomplir est d'afficher des 'vues' de données triées et/ou filtrées. Dans Avalonia, cela peut être réalisé en connectant un `SourceCache<TObject, TKey>` ou une `SourceList<T>` à une `ReadOnlyObservableCollection<T>` et en liant cette collection.

## Création d'un cache source

`SourceCache<TObject, TKey>` ou `SourceList<T>` proviennent de [Dynamic Data in ReactiveUI](https://www.reactiveui.net/docs/handbook/collections/) Exemple :

```csharp
// (x => x.Id) propriété qui sert de clé unique pour le cache
private SourceCache<TestViewModel, Guid> _sourceCache = new (x => x.Id);
```

Ensuite, le `_sourceCache` peut être peuplé via la méthode `AddOrUpdate`.

## Création de vues triées ou filtrées

Ensuite, la `ReadOnlyObservableCollection<T>` peut être liée au `_sourceCache` filtré ou trié. Le tri/la filtration se fait de manière similaire à Linq.

```csharp
private readonly ReadOnlyObservableCollection<TestViewModel> _testViewModels;
public ReadOnlyObservableCollection<TestViewModel> TestViewModels => _testViewModels;
...
public MainWindowViewModel(){
    // Remplir le cache source via _sourceCache.AddOrUpdate
    ...
    _sourceCache.Connect()
        // Trier par ordre croissant sur la propriété OrderIndex
        .Sort(SortExpressionComparer<TestViewModel>.Ascending(t => t.OrderIndex))
        .Filter(x => x.Id.ToString().EndsWith('1'))
        // Lier à notre ReadOnlyObservableCollection<T>
        .Bind(out _testViewModels)
        // S'abonner aux changements
        .Subscribe();
}
```

## Liaison

Maintenant que le `_sourceCache` est créé et peuplé et que la `ReadOnlyObservableCollection<T>` est créée et liée, nous pouvons aller dans notre vue et lier exactement de la manière dont nous le ferions normalement avec une `ObservableCollection<T>`

```xml
<Design.DataContext>
    <vm:MainWindowViewModel/>
</Design.DataContext>

<TreeView ItemsSource="{Binding TestViewModels}">
    <TreeView.DataTemplates>
        <!-- Définitions de DataTemplate -->
    </TreeView.DataTemplates> 
</TreeView>
```