---
id: how-to-bind-to-a-command-without-reactiveui
title: Comment se lier à une Commande sans ReactiveUI
---

import BindCommandMethodScreenshot from '/img/guides/data-binding/bind-method.gif';
import BindCanExecuteMethodScreenshot from '/img/guides/data-binding/bind-method-canexecute.gif';

# Comment se lier à une Commande sans ReactiveUI

Parfois, vous souhaitez simplement appeler une méthode lorsqu'un bouton est cliqué sans toute la cérémonie de créer une commande réactive, en utilisant le framework _ReactiveUI_.

:::info
Pour voir comment lier à une commande **avec** _ReactiveUI_, voir [ici](how-to-bind-to-a-command-with-reactiveui.md).
:::

La liaison de données _Avalonia UI_ vous permet de mettre en œuvre directement à la fois une méthode de modèle de vue qui effectue une action, et une propriété qui peut contrôler si la méthode peut s'exécuter.

Par exemple, en utilisant le XAML comme suit :

```xml
<Window xmlns="https://github.com/avaloniaui">
   ...
   <StackPanel Margin="20">
      <Button Command="{Binding PerformAction}"
              CommandParameter="Depuis le bouton, sans ReactiveUI">
              Exécuter l'exemple</Button>
   </StackPanel>
</Window>
```

Vous pouvez écrire un modèle de vue capable d'exécuter l'action, comme ceci :

```csharp
namespace AvaloniaGuides.ViewModels
{
    public class MainWindowViewModel 
    {
        public void PerformAction(object msg)
        {
            Debug.WriteLine($"L'action a été appelée. {msg}");
        }
    }
}
```

<img src={BindCommandMethodScreenshot} alt=""/>

## Can Execute?

La liaison de données _Avalonia UI_ fournit un moyen simple de mettre en œuvre une fonctionnalité 'peut exécuter ?' en utilisant une convention de nommage.

Si vous devez avoir l'exécution dépendante de la valeur d'un paramètre de commande ou d'une propriété de modèle de vue, vous pouvez alors écrire une seconde méthode booléenne pour vérifier si la méthode d'action peut s'exécuter.

Pour que cela fonctionne, _Avalonia UI_ utilise la convention de nommage selon laquelle la méthode booléenne a le même nom de racine que la méthode d'action, mais avec le préfixe ajouté 'Can'.

Par exemple :

```csharp
namespace AvaloniaGuides.ViewModels
{
    public class MainWindowViewModel 
    {
        public void PerformAction(object msg)
        {
            Debug.WriteLine($"L'action a été appelée. {msg}");
        }

        public bool CanPerformAction(object msg)
        {
            if (msg!=null) return !string.IsNullOrWhiteSpace( msg.ToString() );
            return false;
        }
    }
}
```

Donc, en étendant l'exemple XAML pour fournir le paramètre (chaîne) d'une zone de texte :

```xml
<StackPanel Margin="20">
  <TextBox Margin="0 5" x:Name="message" 
           Watermark="Ajouter un message pour activer le bouton"/>
  <Button Command="{Binding PerformAction}"
          CommandParameter="{Binding #message.Text}">
    Exécuter l'exemple
  </Button>
</StackPanel>
```

:::info
Cet exemple utilise la technique de liaison directement à un autre contrôle. Vous pouvez voir comment faire cela, [ici](binding-to-controls.md).
:::

Vous verrez que le bouton devient activé uniquement lorsque la zone de texte contient une chaîne.

<img src={BindCanExecuteMethodScreenshot} alt=""/>

## **Trigger Can Execute**

Si vous souhaitez déclencher la méthode 'peut exécuter ?' à partir d'une autre propriété de votre modèle de vue, vous devrez alors décorer la propriété avec un ou plusieurs attributs `DependsOn`, et écrire le code pour invoquer les événements de changement de propriété vous-même.

:::info
Cette technique s'applique à un modèle de vue qui n'utilise pas le framework _ReactiveUI_.
:::
