---
id: how-to-create-advanced-custom-controls
title: Comment créer des contrôles personnalisés avancés
---

# Comment créer des contrôles personnalisés avancés

Extraits du guide des contrôles personnalisés.

Voici comment le contrôle `Border` définit sa propriété `Background` :

La méthode `AvaloniaProperty.Register` accepte également un certain nombre d'autres paramètres :

* `defaultValue` : Cela donne à la propriété une valeur par défaut. Assurez-vous de ne passer que des types de valeur et des types immuables ici, car le passage d'un type de référence entraînera l'utilisation du même objet sur toutes les instances sur lesquelles la propriété est enregistrée.
* `inherits` : Spécifie que la valeur par défaut de la propriété doit provenir du contrôle parent.
* `defaultBindingMode` : Le mode de liaison par défaut pour la propriété. Peut être défini sur `OneWay`, `TwoWay`, `OneTime` ou `OneWayToSource`.
* `validate` : Une fonction de validation/coercition de type `Func<TOwner, TValue, TValue>`. La fonction accepte l'instance de la classe sur laquelle la propriété est définie et la valeur, et retourne la valeur coercée ou lance une exception pour une valeur invalide.

> Une propriété stylée est analogue à un `DependencyProperty` dans d'autres frameworks XAML.

> La convention de nommage de la propriété et de son champ AvaloniaProperty associé est importante. Le nom du champ est toujours le nom de la propriété, avec le suffixe Property ajouté.

### Utiliser une `StyledProperty` sur une autre classe

Parfois, la propriété que vous souhaitez ajouter à votre contrôle existe déjà sur un autre contrôle, `Background` étant un bon exemple. Pour enregistrer une propriété définie sur un autre contrôle, vous appelez `StyledProperty.AddOwner` :

```csharp
public static readonly StyledProperty<IBrush> BackgroundProperty =
    Border.BackgroundProperty.AddOwner<Panel>();

public Brush Background
{
    get { return GetValue(BackgroundProperty); }
    set { SetValue(BackgroundProperty, value); }
}
```

> Remarque : Contrairement à WPF/UWP, une propriété doit être enregistrée sur une classe sinon elle ne peut pas être définie sur un objet de cette classe. Cela peut changer à l'avenir, cependant.

### Propriétés en lecture seule

Pour créer une propriété en lecture seule, vous utilisez la méthode `AvaloniaProperty.RegisterDirect`. Voici comment `Visual` enregistre la propriété en lecture seule `Bounds` :

```csharp
public static readonly DirectProperty<Visual, Rect> BoundsProperty =
    AvaloniaProperty.RegisterDirect<Visual, Rect>(
        nameof(Bounds),
        o => o.Bounds);

private Rect _bounds;

public Rect Bounds
{
    get { return _bounds; }
    private set { SetAndRaise(BoundsProperty, ref _bounds, value); }
}
```

Comme on peut le voir, les propriétés en lecture seule sont stockées sous forme de champ sur l'objet. Lors de l'enregistrement de la propriété, un getter est passé, qui est utilisé pour accéder à la valeur de la propriété via `GetValue`, puis `SetAndRaise` est utilisé pour notifier les écouteurs des changements apportés à la propriété.

### Propriétés attachées

[Les propriétés attachées](../../concepts/attached-property) sont définies presque de manière identique aux propriétés stylées, sauf qu'elles sont enregistrées en utilisant la méthode `RegisterAttached` et que leurs accesseurs sont définis comme des méthodes statiques.

Voici comment `Grid` définit sa propriété attachée `Grid.Column` :

```csharp
public static readonly AttachedProperty<int> ColumnProperty =
    AvaloniaProperty.RegisterAttached<Grid, Control, int>("Column");

public static int GetColumn(Control element)
{
    return element.GetValue(ColumnProperty);
}

public static void SetColumn(Control element, int value)
{
    element.SetValue(ColumnProperty, value);
}
```

### Propriétés directed d'Avalonia

Comme son nom l'indique, `RegisterDirect` n'est pas seulement utilisé pour enregistrer des propriétés en lecture seule. Vous pouvez également passer un _setter_ à `RegisterDirect` pour exposer une propriété C# standard en tant que propriété Avalonia.

Une `StyledProperty` qui est enregistrée en utilisant `AvaloniaProperty.Register` maintient une liste priorisée de valeurs et de liaisons qui permettent aux styles de fonctionner. Cependant, cela est excessif pour de nombreuses propriétés, telles que `ItemsControl.Items` - cela ne sera jamais stylé et le surcoût impliqué avec les propriétés stylées est inutile.

Voici comment `ItemsControl.Items` est enregistré :

```csharp
public static readonly DirectProperty<ItemsControl, IEnumerable> ItemsProperty =
    AvaloniaProperty.RegisterDirect<ItemsControl, IEnumerable>(
        nameof(Items),
        o => o.Items,
        (o, v) => o.Items = v);

private IEnumerable _items = new AvaloniaList<object>();

public IEnumerable Items
{
    get { return _items; }
    set { SetAndRaise(ItemsProperty, ref _items, value); }
}
```

Les propriétés directes sont une version allégée des propriétés stylées qui prennent en charge les éléments suivants :

* AvaloniaObject.GetValue
* AvaloniaObject.SetValue pour les propriétés non en lecture seule
* PropertyChanged
* Liaison (uniquement avec la priorité LocalValue)
* GetObservable
* AddOwner
* Métadonnées

Elles ne prennent pas en charge les éléments suivants :

* Validation/Coercition (bien que cela puisse être fait dans le setter de la propriété)
* Remplacement des valeurs par défaut.
* Valeurs héritées

### Utilisation d'une DirectProperty sur une autre classe

De la même manière que vous pouvez appeler `AddOwner` sur une propriété stylée, vous pouvez également ajouter un propriétaire à une propriété directe. Parce que les propriétés directes font référence aux champs sur le contrôle, vous devez également ajouter un champ pour la propriété :

```csharp
public static readonly DirectProperty<MyControl, IEnumerable> ItemsProperty =
    ItemsControl.ItemsProperty.AddOwner<MyControl>(
        o => o.Items,
        (o, v) => o.Items = v);

private IEnumerable _items = new AvaloniaList<object>();

public IEnumerable Items
{
    get { return _items; }
    set { SetAndRaise(ItemsProperty, ref _items, value); }
}
```

### Quand utiliser une propriété directe ou une propriété stylée ?

En général, vous devriez déclarer vos propriétés comme des propriétés stylées. Cependant, les propriétés directes ont des avantages et des inconvénients :

Avantages :

* Aucun objet supplémentaire n'est alloué par instance pour la propriété
* Le getter de la propriété est un getter de propriété standard en C#
* Le setter de la propriété est un setter de propriété standard en C# qui déclenche un événement.
* Vous pouvez ajouter un support de [validation des données](../../guides/development-guides/data-validation.md)

Inconvénients :

* Ne peut pas hériter de la valeur du contrôle parent
* Ne peut pas tirer parti du système de stylisation d'Avalonia
* La valeur de la propriété est un champ et, en tant que tel, est allouée que la propriété soit définie sur l'objet ou non

Utilisez donc des propriétés directes lorsque vous avez les exigences suivantes :

* La propriété n'aura pas besoin d'être stylée
* La propriété aura généralement ou toujours une valeur

### Support de la validation des données

Si vous souhaitez permettre à une propriété de valider les données et d'afficher des messages d'erreur de validation, la propriété doit être implémentée comme une `DirectProperty` et le support de validation doit être activé (`enableDataValidation: true`).

**Exemple d'une propriété avec validation des données activée**

```cs
public static readonly DirectProperty<MyControl, int> ValueProperty =
    AvaloniaProperty.RegisterDirect<MyControl, int>(
        nameof(Value),
        o => o.Value,
        (o, v) => o.Value = v, 
        enableDataValidation: true);
```

Si vous souhaitez [réutiliser une propriété directe d'une autre classe](how-to-create-advanced-custom-controls.md#using-a-directproperty-on-another-class), vous pouvez également activer la validation des données. Dans ce cas, utilisez `AddOwnerWithDataValidation`.

**Exemple : la propriété TextBox.TextProperty property ré-utilise la propriété TextBlock.TextProperty en y ajouter le support de la validation**

```cs
public static readonly DirectProperty<TextBox, string?> TextProperty =
    TextBlock.TextProperty.AddOwnerWithDataValidation<TextBox>(
        o => o.Text,
        (o, v) => o.Text = v,
        defaultBindingMode: BindingMode.TwoWay,
        enableDataValidation: true);
```
