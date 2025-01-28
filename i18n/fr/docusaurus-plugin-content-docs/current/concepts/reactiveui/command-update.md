---
description: CONCEPTS - ReactiveUI
---

# Mise à jour de la commande

Cette page présente comment vous pouvez utiliser le binding d'Avalonia UI pour initier des changements sur un modèle de vue à partir de contrôles comme des boutons qui ont un attribut `Command`.

Par exemple, vous pouvez utiliser ce modèle de vue avec une action définie dans la méthode `ButtonAction` :

```csharp
public class MainWindowViewModel : ViewModelBase
{
    private string _greeting = "Bienvenue dans Avalonia !";

    public string Greeting
    {
        get => _greeting;
        set => this.RaiseAndSetIfChanged(ref _greeting,  value);
    }

    public void ButtonAction()
    {
        Greeting = "Un autre message de bienvenue d'Avalonia";
    }
}
```

Puis, dans le XAML correspondant, définissez deux contrôles :

```xml
<TextBlock Text="{Binding Greeting}" />
<Button Command="{Binding ButtonAction}" >Changez-le</Button>
```

Cela signifie que lorsque l'utilisateur clique sur le bouton, _Avalonia UI_ met à jour le modèle de vue en appelant la méthode `ButtonAction`. Cela change la propriété `Greeting` en utilisant le setter, de sorte que le nouveau texte de bienvenue soit notifié de retour au contrôle de texte sur l'interface utilisateur.