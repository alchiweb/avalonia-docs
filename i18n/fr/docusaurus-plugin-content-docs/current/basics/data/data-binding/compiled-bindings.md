---
description: CONCEPTS
---

# Liaisons compilées

Les liaisons définies dans le XAML utilisent la réflexion pour trouver et accéder à la propriété demandée dans votre `ViewModel`. Dans Avalonia, vous pouvez également utiliser des liaisons compilées, qui présentent certains avantages :

* Si vous utilisez des liaisons compilées et que la propriété à laquelle vous vous liez n'est pas trouvée, vous obtiendrez une erreur à la compilation. Vous bénéficiez donc d'une bien meilleure expérience de débogage.
* La réflexion est connue pour être lente ([voir cet article sur codeproject.com](https://www.codeproject.com/Articles/1161127/Why-is-reflection-slow)). L'utilisation de liaisons compilées peut donc améliorer les performances de votre application.

## Activer et désactiver les liaisons compilées

:::info

Selon le modèle utilisé pour créer le projet Avalonia, les liaisons compilées peuvent être activées ou non par défaut. Vous pouvez vérifier cela dans le fichier du projet.

::: 

### Activer et désactiver globalement

Si vous souhaitez que votre application utilise des liaisons compilées par défaut, vous pouvez ajouter

```xml
<AvaloniaUseCompiledBindingsByDefault>true</AvaloniaUseCompiledBindingsByDefault>
```

à votre fichier de projet. Vous devrez toujours fournir `x:DataType` pour les objets que vous souhaitez lier, mais vous n'avez pas besoin de définir `x:CompileBindings="[True|False]"` pour chaque `UserControl` ou `Window`.

### Activer et désactiver par UserControl ou Window

Pour activer les liaisons compilées, vous devez d'abord définir le `DataType` de l'objet auquel vous souhaitez vous lier. Dans les [`DataTemplates`](../data-templates), il existe une propriété `DataType`, pour tous les autres éléments, vous pouvez le définir via `x:DataType`. Il est probable que vous définissiez `x:DataType` dans votre nœud racine, par exemple dans une `Window` ou un `UserControl`. Vous pouvez également spécifier le `DataType` directement dans la `Binding`.

Vous pouvez maintenant activer ou désactiver les liaisons compilées en définissant `x:CompileBindings="[True|False]"`. Tous les nœuds enfants hériteront de cette propriété, vous pouvez donc l'activer dans votre nœud racine et la désactiver pour un enfant spécifique, si nécessaire.

```xml
<!-- Définir DataType et activer les liaisons compilées -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             x:DataType="vm:MyViewModel"
             x:CompileBindings="True">
    <StackPanel>
        <TextBlock Text="Nom de famille :" />
        <TextBox Text="{Binding LastName}" />
        <TextBlock Text="Prénom :" />
        <TextBox Text="{Binding GivenName}" />
        <TextBlock Text="E-Mail :" />
        <!-- Définir DataType à l'intérieur du Binding-markup -->
        <TextBox Text="{Binding MailAddress, DataType={x:Type vm:MyViewModel}}" />

        <Button Content="Envoyer un E-Mail"
                Command="{Binding SendEmailCommand}" />
    </StackPanel>
</UserControl>
```

## CompiledBinding-Markup

Si vous ne souhaitez pas activer les liaisons compilées pour tous les nœuds enfants, vous pouvez également utiliser le balisage `CompiledBinding`. Vous devez toujours définir le `DataType`, mais vous pouvez omettre `x:CompileBindings="True"`.

```xml
<!-- Définir DataType -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             x:DataType="vm:MyViewModel">
    <StackPanel>
        <TextBlock Text="Nom de famille :" />
        <!-- utiliser le balisage CompiledBinding pour votre liaison -->
        <TextBox Text="{CompiledBinding LastName}" />
        <TextBlock Text="Prénom :" />
        <TextBox Text="{CompiledBinding GivenName}" />
        <TextBlock Text="E-Mail :" />
        <TextBox Text="{CompiledBinding MailAddress}" />

        <!-- Cette commande utilisera ReflectionBinding, car c'est par défaut -->
        <Button Content="Envoyer un E-Mail"
                Command="{Binding SendEmailCommand}" />
    </StackPanel>
</UserControl>
```

## Balisage ReflectionBinding

Si vous avez activé les liaisons compilées dans le nœud racine (via `x:CompileBindings="True"`) et que vous ne souhaitez pas utiliser la liaison compilée à un certain endroit, vous pouvez utiliser le balisage `ReflectionBinding`.
```xml
<!-- Définir le type de données -->
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:vm="using:MyApp.ViewModels"
             x:DataType="vm:MyViewModel"
             x:CompileBindings="True">
    <StackPanel>
        <TextBlock Text="Nom de famille :" />
        <TextBox Text="{Binding LastName}" />
        <TextBlock Text="Prénom :" />
        <TextBox Text="{Binding GivenName}" />
        <TextBlock Text="E-Mail :" />
        <TextBox Text="{Binding MailAddress}" />

        <!-- Nous utilisons ReflectionBinding à la place -->
        <Button Content="Envoyer un E-Mail"
                Command="{ReflectionBinding SendEmailCommand}" />
    </StackPanel>
</UserControl>
```

## Conversion de type

Dans certains cas, le type cible de l'expression de liaison ne peut pas être évalué automatiquement. Dans de tels cas, vous devez fournir une conversion de type explicite dans l'expression de liaison.

```xml
<ItemsRepeater ItemsSource="{Binding MyItems}">
<ItemsRepeater.ItemTemplate>
    <DataTemplate>
    <StackPanel Orientation="Horizontal">
        <TextBlock Text="{Binding DisplayName}"/>
        <Grid>
        <Button Command="{Binding $parent[ItemsRepeater].((vm:MyUserControlViewModel)DataContext).DoItCommand}"
                CommandParameter="{Binding ItemId}"/>
        </Grid>
    </StackPanel>
    </DataTemplate>
</ItemsRepeater.ItemTemplate>
</ItemsRepeater>
```

Dans ce cas, la commande du bouton ne doit pas être liée au `DataContext` de l'élément, mais à une commande définie dans le `DataContext` de l'`ItemsRepeater`. L'élément unique sera identifié à l'aide d'un `CommandParameter` lié au `DataContext` de l'élément. Par conséquent, vous devez spécifier le type du `DataContext` "parent" via une expression de conversion `((vm:MyUserControlViewModel)DataContext)`.