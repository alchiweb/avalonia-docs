# Définir des Événements

Les événements dans Avalonia permettent à vos contrôles personnalisés de communiquer et de notifier les utilisateurs d'actions ou d'occurrences spécifiques. En définissant des événements, vous offrez un moyen aux utilisateurs de vos contrôles de répondre et de réagir à ces événements dans leurs applications. Ce document vous guidera à travers le processus de définition d'événements pour vos contrôles personnalisés.

## Événement Routé

Les événements routés dans Avalonia offrent un mécanisme pour gérer des événements qui peuvent voyager (ou "routage") à travers l'arbre de contrôle, permettant à plusieurs contrôles de répondre au même événement. Les événements routés offrent les caractéristiques clés suivantes :

- **Routage d'Événements** : Les événements routés peuvent se propager vers le haut de l'arbre (bubbling) ou vers le bas de l'arbre (tunneling), permettant à des contrôles à différents niveaux de gérer le même événement. Cela permet une gestion des événements plus flexible et centralisée.

- **Gestionnaires d'Événements** : Les événements routés utilisent des gestionnaires d'événements pour répondre aux événements. Les gestionnaires d'événements sont associés à des contrôles spécifiques ou peuvent être attachés à des niveaux supérieurs dans l'arbre visuel pour gérer les événements provenant de plusieurs contrôles.

- **État Géré** : Les événements routés ont une propriété `Handled` qui peut être utilisée pour marquer un événement comme géré, empêchant ainsi une propagation supplémentaire. Cela permet un contrôle précis sur la gestion des événements.

- **Stratégies de routage des événements** : Avalonia prend en charge différentes stratégies de routage pour les événements routés, telles que le bubbling, le tunneling et le routage direct. Ces stratégies déterminent l'ordre dans lequel les contrôles reçoivent et gèrent les événements. Les événements routés d'Avalonia sont particulièrement utiles lorsque vous devez gérer des événements qui peuvent se produire dans des contrôles imbriqués ou lorsque vous souhaitez centraliser la logique de gestion des événements plus haut dans l'arbre visuel.

## Exemple

Voici un exemple de la façon de définir un événement routé pour un contrôle de curseur personnalisé hypothétique :

```csharp
public class MyCustomSlider : Control
{
    public static readonly RoutedEvent<RoutedEventArgs> ValueChangedEvent =
        RoutedEvent.Register<MyCustomSlider, RoutedEventArgs>(nameof(ValueChanged), RoutingStrategies.Direct);

    public event EventHandler<RoutedEventArgs> ValueChanged
    {
        add => AddHandler(ValueChangedEvent, value);
        remove => RemoveHandler(ValueChangedEvent, value);
    }

    protected virtual void OnValueChanged()
    {
        RoutedEventArgs args = new RoutedEventArgs(ValueChangedEvent);
        RaiseEvent(args);
    }
}
```

Dans cet exemple, un événement routé personnalisé appelé `ValueChangedEvent` est défini pour le contrôle `MyCustomSlider`. L'événement est enregistré à l'aide du système `RoutedEvent`, permettant aux utilisateurs du contrôle de s'y abonner. Un événement CLR est également défini pour la commodité, permettant à l'événement d'être consommé de manière cohérente avec les API standard .NET.

## Lecture complémentaire

Pour plus d'informations, consultez le [Plongée approfondie sur les événements routés](../../../../concepts/input/routed-events.md)
