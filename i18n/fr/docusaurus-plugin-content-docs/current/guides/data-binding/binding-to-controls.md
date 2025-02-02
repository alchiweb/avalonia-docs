---
id: binding-to-controls
title: Comment lier à un contrôle
---


# Comment lier à un contrôle

Avec _Avalonia UI_, ainsi que la liaison à un contexte de données, vous pouvez également lier un contrôle directement à un autre.

:::info
Notez que cette technique n'utilise pas du tout de contexte de données. Lorsque vous faites cela, vous liez directement à un autre contrôle lui-même.
:::

## Liaison à un contrôle nommé

Si vous souhaitez lier à une propriété d'un autre contrôle nommé, vous pouvez utiliser le nom du contrôle précédé d'un caractère `#`.

```xml
<TextBox Name="other">

<!-- Lie à la propriété Text du contrôle "other" -->
<TextBlock Text="{Binding #other.Text}"/>
```

Ceci est l'équivalent de la liaison en forme longue qui sera familière aux utilisateurs de WPF et UWP :

```xml
<TextBox Name="other">
<TextBlock Text="{Binding Text, ElementName=other}"/>
```

_Avalonia UI_ prend en charge les deux syntaxes.

## Liaison à un ancêtre

Vous pouvez lier au parent (arbre logique de contrôle) de la cible en utilisant la syntaxe `$parent` :

```xml
<Border Tag="Hello World!">
  <TextBlock Text="{Binding $parent.Tag}"/>
</Border>
```

Ou à tout niveau d'ancêtre en utilisant un index avec la syntaxe `$parent` :

```xml
<Border Tag="Hello World!">
  <Border>
    <TextBlock Text="{Binding $parent[1].Tag}"/>
  </Border>
</Border>
```

L'index est basé sur zéro, donc `$parent[0]` équivaut à `$parent`.

Vous pouvez également vous lier à l'ancêtre le plus proche d'un type donné, comme ceci :

```xml
<Border Tag="Hello World!">
  <Decorator>
    <TextBlock Text="{Binding $parent[Border].Tag}"/>
  </Decorator>
</Border>
```

Enfin, vous pouvez combiner l'index et le type :

```xml
<Border Tag="Hello World!">
  <Border>
    <Decorator>
    <TextBlock Text="{Binding $parent[Border;1].Tag}"/>
    </Decorator>
  </Border>
</Border>
```

Si vous devez inclure un espace de noms XAML dans le type d'ancêtre, vous séparez l'espace de noms et la classe en utilisant un deux-points, comme ceci :

```xml
<local:MyControl Tag="Hello World!">
  <Decorator>
    <TextBlock Text="{Binding $parent[local:MyControl].Tag}"/>
  </Decorator>
</local:MyControl>
```

Pour accéder à une propriété du `DataContext` d'un parent, il sera nécessaire de le caster avec une expression de casting `(vm:MyUserControlViewModel)DataContext` à son type réel. Sinon, `DataContext` serait considéré comme de type `object` et accéder à une propriété personnalisée entraînerait une erreur de compilation.

```xml
<local:MyControl Tag="Hello World!">
  <Decorator>
    <TextBlock Text="{Binding $parent[local:MyControl].((vm:MyUserControlViewModel)DataContext).CustomProperty}"/>
  </Decorator>
</local:MyControl>
```

:::warning
_Avalonia UI_ prend également en charge la syntaxe `RelativeSource` de WPF/UWP qui fait quelque chose de similaire, mais n'est _pas_ la même chose. `RelativeSource` fonctionne sur l'arbre _visuel_ tandis que la syntaxe donnée ici fonctionne sur l'arbre _logique_.
:::
