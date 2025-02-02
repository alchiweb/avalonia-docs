---
id: how-to-bind-to-a-task-result
title: Comment se lier au résultat d'une tâche
---


# Comment se lier au résultat d'une tâche

## Exemple 1 : Liaison à une tâche

Si vous devez effectuer un travail lourd pour charger le contenu d'une propriété, vous pouvez vous lier au résultat d'un `async Task<TResult>`.

Considérez que vous avez le modèle de vue suivant qui génère un texte dans un processus de longue durée :

```csharp
public Task<string> MyAsyncText => GetTextAsync();

private async Task<string> GetTextAsync()
{
  await Task.Delay(1000); // Le délai est juste à des fins de démonstration
  return "Hello de l'opération asynchrone";
}
```

Vous pouvez vous lier au résultat de la manière suivante :

```xml
<TextBlock Text="{Binding MyAsyncText^, FallbackValue='Attendez une seconde'}" />
```

:::info
Remarque : Vous pouvez utiliser `FallbackValue` pour afficher un indicateur de chargement.
:::
