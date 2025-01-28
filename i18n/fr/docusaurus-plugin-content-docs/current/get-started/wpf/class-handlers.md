# Gestionnaires de classe

Dans WPF, des gestionnaires de classe pour des événements peuvent être ajoutés en appelant [EventManager.RegisterClassHandler](https://msdn.microsoft.com/en-us/library/ms597875.aspx). Un exemple d'enregistrement d'un gestionnaire de classe dans WPF pourrait être :


```csharp title='WPF'
static MyControl()
{
    EventManager.RegisterClassHandler(typeof(MyControl), MyEvent, HandleMyEvent));
}

private static void HandleMyEvent(object sender, RoutedEventArgs e)
{
}
```


```csharp title='Avalonia'
static MyControl()
{
    MyEvent.AddClassHandler<MyControl>((x, e) => x.HandleMyEvent(e));
}

private void HandleMyEvent(RoutedEventArgs e)
{
}
```

Remarquez qu'en WPF, vous devez ajouter le gestionnaire de classe en tant que méthode statique, tandis qu'en Avalonia, le gestionnaire de classe n'est pas statique : la notification est automatiquement dirigée vers l'instance correcte. Le paramètre `sender`, typique des gestionnaires d'événements, n'est pas nécessaire dans ce cas et tout reste fortement typé.

<XpfAd/>