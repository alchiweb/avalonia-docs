# Tunnelling Events

Avalonia a des événements de tunneling, mais ils ne sont pas exposés via des gestionnaires d'événements CLR `Preview` séparés. Pour s'abonner à un événement de tunneling, vous devez appeler `AddHandler` avec `RoutingStrategies.Tunnel` :

```csharp
target.AddHandler(InputElement.KeyDownEvent, OnPreviewKeyDown, RoutingStrategies.Tunnel);

void OnPreviewKeyDown(object sender, KeyEventArgs e)
{
    // Code du gestionnaire
}
```

<XpfAd/>