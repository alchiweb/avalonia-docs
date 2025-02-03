---
id: markupextensions
title: Extensions de Marquage
---

Une `MarkupExtension` permet une personnalisation basée sur le code de la logique de définition pour une propriété cible dans une syntaxe pratique et réutilisable au sein de XAML. Les accolades sont utilisées pour différencier l'utilisation du texte brut.

Avalonia fournit ce qui suit :

| Extension de Marquage                                                                                  | Assigne à la Propriété                                                |
|--------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| [StaticResource](/docs/guides/styles-and-resources/resources#ressource-statique)                    | Une ressource clé existante qui ne se met pas à jour lors des changements          |
| [DynamicResource](/docs/guides/styles-and-resources/resources#utilisation-des-ressources)                   | Chargement différé d'une ressource clé qui se mettra à jour lors des changements   |
| Binding                                                                                          | Basé sur la préférence de liaison par défaut : Compilé ou Réflexion    |
| [CompiledBinding](/docs/basics/data/data-binding/compiled-bindings#compiledbinding-markup)       | Basé sur une liaison compilée                                        |
| [ReflectionBinding](/docs/basics/data/data-binding/compiled-bindings#balisage-reflectionbinding)   | Basé sur une liaison par réflexion                                      |
| [TemplateBinding](/docs/guides/custom-controls/how-to-create-templated-controls#liaison-de-données)    | Basé sur un lien simplifié utilisé uniquement dans un `ControlTemplate` |
| [OnPlatform](/docs/guides/platforms/platform-specific-code/xaml#onplatform-markup-extension)     | Conditionnellement lorsque sur la plateforme spécifiée                       |
| [OnFormFactor](/docs/guides/platforms/platform-specific-code/xaml#onformfactor-markup-extension) | Conditionnellement lorsque sur le facteur spécifié                         |

## Intrinsèques du compilateur

Ceci tombe techniquement en dehors des `MarkupExtension`s en tant que partie du compilateur XAML, mais la syntaxe XAML est la même.

| Intrinsèque | Assigne à la Propriété   |
|-------------|---------------------------|
| x:True      | littéral `true`          |
| x:False     | littéral `false`         |
| x:Null      | littéral `null`          |
| x:Static    | Valeur de membre statique |
| x:Type      | littéral `System.Type`   |

Les littéraux `x:True` et `x:False` ont des cas d'utilisation où la propriété de liaison cible est `object` et vous devez fournir un booléen. Dans ces scénarios qui manquent d'informations de type, fournir "True" reste une `string`.

```xml
<Button Command="{Binding SetStateCommand}" CommandParameter="{x:True}" />
```

## Création de MarkupExtensions

Dérivez de `MarkupExtension` ou ajoutez l'une des signatures suivantes qui sont prises en charge via duck-typing :

```csharp
T ProvideValue();
T ProvideValue(IServiceProvider provider);
object ProvideValue();
object ProvideValue(IServiceProvider provider);
```

Lorsque des types forts sont utilisés au lieu de `object`, vous recevrez des erreurs de compilation en cas de désaccord dans l'utilisation des paramètres de constructeur, des propriétés ou de la valeur de retour dans `ProvideValue` dans le XAML. Lorsque vous retournez `object`, le type réel retourné doit correspondre au type de la propriété cible, sinon une `InvalidCastException` est levée à l'exécution.

### Réception de paramètres littéraux

Lorsque des paramètres sont requis, utilisez un constructeur pour recevoir chaque paramètre dans l'ordre.

Pour les paramètres optionnels ou non ordonnés, utilisez plutôt des propriétés. Il est permis de mélanger et d'associer plusieurs constructeurs, y compris ceux sans paramètres.

```csharp
public class MultiplyLiteral
{
    private readonly double _premier;
    private readonly double _deuxieme;

    public double? Troisieme { get; set; }

    public MultiplyLiteral(double premier, double deuxieme)
    {
        _premier = premier;
        _deuxieme = deuxieme;
    }

    public double ProvideValue(IServiceProvider provider)
    {
        return _premier * _deuxieme * Troisieme ?? 1;
    }
}
```
```xml
<TextBlock Text="Ceci a FontSize=40" FontSize="{namespace:MultiplyLiteral 10, 8, Troisieme=0.5}" />
```

### Réception de paramètres à partir de liaisons

Un scénario courant consiste à vouloir transformer des données provenant d'une liaison et à mettre à jour la propriété cible. Lorsque tous les paramètres proviennent de liaisons, cela est relativement simple en créant un `MultiBinding` avec un `IMultiValueConverter`. Dans l'exemple ci-dessous, `MultiplyBinding` nécessite deux paramètres liés. Si un mélange de paramètres littéraux et liés est nécessaire, la création d'un `IMultiValueConverter` permettrait de passer des littéraux en tant que paramètres de constructeur ou `init`. `BindingBase` permet d'utiliser à la fois `CompiledBinding` et `ReflectionBinding`, mais ne permet pas les littéraux.

```csharp
public class MultiplyBinding
{
    private readonly BindingBase _premier;
    private readonly BindingBase _second;

    public MultiplyBinding(BindingBase premier, BindingBase deuxieme)
    {
        _premier = premier;
        _deuxieme = deuxieme;
    }

    public object ProvideValue()
    {
        var mb = new MultiBinding()
        {
            Bindings = new[] { _premier, _deuxieme },
            Converter = new FuncMultiValueConverter<double, double>(doubles => doubles.Aggregate(1d, (x, y) => x * y))
        };

        return mb;
    }
}
```

```xml
<TextBlock FontSize="{local:MultiplyBinding {Binding Multiplier}, {Binding Multiplicand}}" 
           Text="MarkupExtension avec des liaisons !" />
```

:::info

Une approche alternative consiste à retourner un `IObservable<T>.ToBinding()` à la place.

:::

### Retourner des Paramètres

Pour rendre un `MarkupExtension` compatible avec plusieurs types de propriétés cibles, retournez un `object` et gérez chaque type pris en charge individuellement.

```csharp
public object ProvideValue(IServiceProvider provider)
{
    var target = (IProvideValueTarget)provider.GetService(typeof(IProvideValueTarget))!;
    var targetProperty = target.TargetProperty as AvaloniaProperty;
    var targetType = targetProperty?.PropertyType;

    double result = Premier * Deuxieme * (Troisieme ?? 1);

    if (targetType == typeof(double))
        return result;
    else if (targetType == typeof(float))
        return (float)result;
    else if (targetType == typeof(int))
        return (int)result;
    else
        throw new NotSupportedException();
}
```

Les constructeurs peuvent également recevoir des types de paramètres en utilisant l'approche `object`, mais les erreurs de compilation se transforment également en exceptions d'exécution.

### Attributs de Propriété MarkupExtension

* `[ConstructorArgument]` - La propriété associée peut être initialisée par un paramètre de constructeur et doit être ignorée pour la sérialisation XAML si le constructeur est utilisé.
* `[MarkupExtensionOption]`, `[MarkupExtensionDefaultOption]` - Utilisé avec `ShouldProvideOption`, vérifiez la source `OnPlatform` et `OnFormFactor` par exemple.
