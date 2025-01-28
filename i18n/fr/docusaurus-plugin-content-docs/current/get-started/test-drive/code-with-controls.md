---
id: code-with-controls
title: Codez avec les contrôles
---

Dans cette section, vous allez implémenter la logique principale pour mettre à jour la température en Fahrenheit en fonction de l'entrée en Celsius.

## Noms des Contrôles

Avalonia crée des objets pour chaque contrôle défini dans la hiérarchie XAML. Votre code peut accéder à ces contrôles à l'exécution, mais ils doivent être nommés pour un accès facile.

Pour ajouter des noms de contrôle, suivez cette procédure :

- Arrêtez l'application si elle est en cours d'exécution.
- Localisez la `TextBox` pour Celsius.
- Ajoutez l'attribut `Name` comme ceci :

```xml
<TextBox ... Name="Celsius"/>
```

- Répétez ce qui précède pour l'entrée en Fahrenheit :

```xml
<TextBox ... Name="Fahrenheit"/>
```

## Obtenir les Valeurs des Contrôles dans le Code-Behind

Pour accéder à la valeur `Text` de l'entrée `celsius`, suivez cette procédure :

- Passez au fichier code-behind **MainWindow.axaml.cs**.
- Localisez le gestionnaire d'événements `Button_OnClick`.
- Modifiez l'instruction `Debug` pour afficher la propriété texte de l'entrée `Celsius`, comme ceci :

```csharp
Debug.WriteLine($"Click! Celsius={Celsius.Text}");
```

- Exécutez à nouveau l'application (en mode débogage) pour confirmer que vous pouvez voir la valeur en Celsius apparaître dans la fenêtre de débogage.

## Définir les Valeurs des Contrôles dans le Code-Behind

Pour utiliser la formule simple qui convertit la température en Celsius en Fahrenheit, vous devez d'abord vous assurer que le texte d'entrée en Celsius se convertit en un nombre. La formule est alors :

```
Tf = Tc * (9/5) + 32
```

Pour ajouter la formule de conversion, suivez cette procédure :

- Localisez le gestionnaire d'événements `Button_OnClick`.
- Validez le texte d'entrée en Celsius en tant que nombre.
- Utilisez la formule de conversion.
- Mettez à jour le `Text` dans l'entrée en Fahrenheit.
- Exécutez l'application pour vérifier votre travail.

Une mise en œuvre de ce qui précède est la suivante :

```csharp
private void Button_OnClick(object? sender, RoutedEventArgs e)
{
    if (double.TryParse(Celsius.Text, out double C))
    {
        var F = C * (9d / 5d) + 32;
        Fahrenheit.Text = F.ToString("0.0");
    }
    else
    {
        Celsius.Text = "0";
        Fahrenheit.Text = "0";
    }
}
```

Vous pouvez vérifier votre travail en utilisant le tableau de conversion suivant :

| Celsius | Fahrenheit |
|---------|------------|
| -10     | 14.0       |
| 0       | 32.0       |
| 10      | 50.0       |
| 21      | 69.8       |
| 25      | 77.0       |
| 32      | 89.6       |

### Exercices

Vous avez maintenant utilisé un gestionnaire d'événements pour obtenir et définir les propriétés de contrôle à l'exécution. Essayez certains de ces exercices :

- Arrêtez d'afficher les lignes de grille (facile).
- Empêchez l'utilisateur de modifier le texte dans le champ de saisie Fahrenheit en définissant l'attribut `IsReadOnly` (facile).
- Calculez la conversion pendant que l'utilisateur tape dans le champ de saisie Celsius en utilisant l'événement `TextChanged` (modéré).

:::info
Pour des informations complètes sur l'ensemble des contrôles intégrés, événements et attributs d'Avalonia, consultez la section de référence des contrôles [ici](../../reference/controls/).
:::
