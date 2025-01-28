# Grid

Les définitions de colonnes et de lignes peuvent être spécifiées en Avalonia à l'aide de chaînes, évitant la syntaxe encombrante de WPF :

```xml
<Grid ColumnDefinitions="Auto,*,32" RowDefinitions="*,Auto">
```

Une utilisation courante de `Grid` dans WPF est d'empiler deux contrôles les uns sur les autres. À cette fin, en Avalonia, vous pouvez simplement utiliser un `Panel` qui est plus léger que `Grid`.

<XpfAd/>