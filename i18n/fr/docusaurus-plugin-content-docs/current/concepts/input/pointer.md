---
description: CONCEPTS - Input
---

import PointerPressedSampleScreenshot from '/img/concepts/input/pointer-pressed.gif';

# Souris et dispositifs de pointage

Dans _Avalonia UI_, vous implémentez l'interaction des dispositifs de pointage avec votre application en utilisant une abstraction de 'pointeur'. Cela peut représenter des dispositifs y compris, mais sans s'y limiter, une souris, un pavé tactile et un stylo. Les contrôles _Avalonia UI_ ont des événements qui vous permettent de vous abonner aux mouvements de pointeur, aux clics et aux mouvements de la molette. Ceux-ci sont les suivants :

* PointerEntered
* PointerExited
* PointerMoved
* PointerPressed
* PointerReleased
* PointerWheelChanged
* Tapped
* DoubleTapped
* Holding

Par exemple, vous pouvez vous abonner à l'événement pour l'un des boutons de pointeur étant pressé sur un contrôle, comme ceci :

```csharp title='C#'
private void PointerPressedHandler (object sender, PointerPressedEventArgs args)
{
    var point = args.GetCurrentPoint(sender as Control);
    var x = point.Position.X;
    var y = point.Position.Y;
    var msg = $"Pression du pointeur à {x}, {y} relatif à l'expéditeur.";
    if (point.Properties.IsLeftButtonPressed)
    {
        msg += " Bouton gauche pressé.";
    }
    if (point.Properties.IsRightButtonPressed)
    {
        msg += " Bouton droit pressé.";
    }
    results.Text = msg ;
}
```

```xml title='XAML'
<StackPanel Margin="20" Background="AliceBlue" 
                PointerPressed="PointerPressedHandler" >
  <TextBlock x:Name="results" Margin="5">Ready...</TextBlock>
</StackPanel>
```

<img src={PointerPressedSampleScreenshot} alt=""/>

## Position du pointeur

Dans l'exemple ci-dessus, les coordonnées du pointeur (`x` et `y`) ont été calculées par rapport à l'origine du contrôle de l'expéditeur (en haut, à gauche), dans ce cas, le panneau empilé. Si vous souhaitez les coordonnées par rapport à la fenêtre contenant, vous pouvez utiliser la méthode `GetCurrentPoint` comme suit :

```csharp
var point = args.GetCurrentPoint(this);
```

## Événements de tapotement

Les contrôles ont également des événements de geste spéciaux, qui sont : `Tapped`, `DoubleTapped` et `Holding`. L'événement tapoté est déclenché après que le pointeur a été pressé sur le contrôle puis relâché. Le double tapotement est déclenché après que le pointeur a été pressé deux fois au même endroit.

Le maintien est déclenché après que le pointeur a été pressé pendant une durée déterminée. La durée de maintien est définie dans la propriété `HoldWaitDuration` dans `TopLevel` PlatformSettings. Le maintien peut être activé sur un contrôle en définissant la propriété attachée `Gestures.IsHoldingEnabled`. Lorsque la durée de maintien est écoulée, l'événement `HoldingEvent` du contrôle est déclenché avec l'état `HoldingState` des arguments défini sur `HoldingState.Started`. Lors du relâchement du pointeur, l'événement est déclenché à nouveau avec l'état `HoldingState.Completed`. Si un nouveau geste est initié ou qu'un second pointeur est pressé pendant que le maintien a commencé, le geste de maintien est annulé et un `HoldingEvent` est déclenché avec l'état `HoldingState.Cancelled`. Le maintien peut également être initié en utilisant le pointeur de la souris, en définissant la propriété attachée `Gestures.IsHoldWithMouseEnabled` sur le contrôle.

:::info
Notez que la distance maximale entre un premier et un second tap, ainsi que le délai entre eux, dépendra de la plateforme cible et est généralement plus grande pour les appareils tactiles.
:::

## Plus d'informations

Pour la documentation API complète concernant les événements de pointeur et de tap, voir [ici](http://reference.avaloniaui.net/api/Avalonia.Input/PointerEventArgs/).