# Définition des Propriétés

Les propriétés de contrôle dans Avalonia UI vous permettent d'exposer des aspects configurables de vos contrôles personnalisés, permettant aux utilisateurs de vos contrôles de personnaliser leur comportement et leur apparence. Ce document présentera le processus de définition des propriétés pour vos contrôles personnalisés.

## Propriétés Stylées

Les propriétés stylées dans Avalonia offrent un moyen puissant et flexible de définir des propriétés pour les contrôles. Ces propriétés sont spécifiquement conçues pour prendre en charge le système de stylisation d'Avalonia et la liaison de données. Les propriétés stylées dans Avalonia sont enregistrées à l'aide de la classe `AvaloniaProperty`.

Les propriétés stylées d'Avalonia ont les caractéristiques clés suivantes :

- **Support de Stylisation** : Elles peuvent être facilement ciblées et modifiées à l'aide de styles, et de setters définis en XAML ou par programme.
Héritage : elles prennent en charge l'héritage, ce qui signifie qu'une valeur de propriété définie sur un contrôle parent peut être automatiquement héritée par ses contrôles enfants, sauf si elle est explicitement remplacée.

- **Valeurs par Défaut** : Elles peuvent avoir des valeurs par défaut spécifiées au niveau du contrôle ou au sein des modèles de contrôle, garantissant un comportement cohérent à travers plusieurs instances du contrôle.

- **Priorité de Valeur de Propriété** : Elles suivent un ordre de priorité bien défini, permettant aux valeurs d'être résolues en fonction de facteurs tels que les valeurs locales, les paramètres de style, les déclencheurs et les valeurs par défaut. Les propriétés stylées d'Avalonia sont couramment utilisées pour les propriétés de contrôle qui sont destinées à être facilement personnalisables via le style, permettant des changements dynamiques d'apparence et de comportement en fonction de diverses conditions.

- **Validation et Coercition** : Les propriétés stylées permettent à un contrôle de valider et de contraindre les valeurs qui lui sont passées, garantissant que le contrôle n'est jamais dans un état invalide.

## Exemple

Voici un exemple de la façon de définir une propriété stylée personnalisée pour un contrôle de bouton personnalisé hypothétique :

```csharp
public class MyCustomButton : Button
{
    public static readonly StyledProperty<int> RepeatCountProperty =
        AvaloniaProperty.Register<MyCustomButton, int>(nameof(RepeatCount), defaultValue: 1);

    public int RepeatCount
    {
        get => GetValue(RepeatCountProperty);
        set => SetValue(RepeatCountProperty, value);
    }
}
```

Dans cet exemple, une propriété personnalisée appelée `RepeatCount` est définie comme une propriété entière pour le contrôle `MyCustomButton`. La propriété est enregistrée en utilisant le système `AvaloniaProperty`, permettant son accès, sa modification, son stylage et sa liaison de données par les utilisateurs du contrôle. Une propriété CLR est également définie pour plus de commodité, permettant à la propriété d'être consommée de manière cohérente avec les API .NET standard.

## Lecture Supplémentaire

Pour plus d'informations, consultez le [Guide sur la Définition des Propriétés](../../../../guides/custom-controls/defining-properties.md)