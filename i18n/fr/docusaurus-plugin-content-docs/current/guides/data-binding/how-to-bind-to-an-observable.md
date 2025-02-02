---
id: how-to-bind-to-an-observable
title: Comment se lier à un Observable
---


# Comment se lier à un Observable

Contenu en préparation.

Vous pouvez vous abonner au résultat d'une tâche ou d'un observable en utilisant l'opérateur de liaison de flux `^`.

## Exemple 1 : Liaison à un observable

Par exemple, si `DataContext.Name` est un `IObservable<string>`, alors l'exemple suivant se liera à la longueur de chaque chaîne produite par l'observable à mesure que chaque valeur est produite.

```xml
<TextBlock Text="{Binding Name^.Length}"/>
```
