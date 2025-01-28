---
id: headless-custom
title: Configuration Manuelle de la Plateforme Headless
---

:::warning
Cette page explique un scénario d'utilisation avancé avec la plateforme Headless.
Nous recommandons d'utiliser les frameworks de test [XUnit](headless-xunit.md) ou [NUnit](headless-nunit.md) à la place.
:::

## Installer les Paquets

Pour configurer la plateforme Headless, vous devez installer deux paquets :
- [Avalonia.Headless](https://www.nuget.org/packages/Avalonia.Headless), qui inclut également Avalonia.
- [Avalonia.Themes.Fluent](https://www.nuget.org/packages/Avalonia.Themes.Fluent), car même les contrôles headless ont besoin d'un thème.

:::tip
La plateforme Headless ne nécessite aucun thème spécifique, et il est possible de remplacer FluentTheme par n'importe quel autre.
:::

## Configurer l'Application

Comme dans toute autre application Avalonia, une instance `Application` doit être créée, et les thèmes doivent être appliqués. Lors de l'utilisation de la plateforme Headless, la configuration n'est pas très différente de celle d'une application Avalonia classique et peut généralement être réutilisée.

```xml title=App.axaml
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="Tests.App">
  <Application.Styles>
    <FluentTheme />
  </Application.Styles>
</Application>
```

Et le code :

```csharp title=App.axaml.cs
using Avalonia;
using Avalonia.Headless;

public class App : Application
{
    public override void Initialize()
    {
        AvaloniaXamlLoader.Load(this);
    }
}
```

## Exécuter une Session Headless

```csharp title=Program.cs
using Avalonia.Controls;
using Avalonia.Headless;

// Démarrer la session Headless en passant le type d'Application.
using var session = HeadlessUnitTestSession.StartNew(typeof(App));

// Puisque la session Headless a son propre thread en interne, nous devons y dispatcher les actions :
await session.Dispatch(() =>
{
    // Configurer les contrôles :
    var textBox = new TextBox();
    var window = new Window { Content = textBox };

    // Ouvrir la fenêtre :
    window.Show();

    // Mettre le focus sur la zone de texte :
    textBox.Focus();

    // Simuler une saisie de texte :
    window.KeyTextInput("Hello World");

    // Vérifier :
    if (textBox.Text != "Hello World")
    {
        throw new Exception("Assert");
    }
}, CancellationToken.None);
```