---
id: headless-nunit
title: Tester le Headless avec NUnit
---

## Préparation

Cette page suppose qu'un projet NUnit a déjà été créé.
Si ce n'est pas le cas, veuillez suivre les sections "Premiers pas" et "Installation" de NUnit ici https://docs.nunit.org/articles/nunit/getting-started/installation.html.

## Installer des packages

En plus des packages NUnit, nous devons installer deux autres packages :
- [Avalonia.Headless.NUnit](https://www.nuget.org/packages/Avalonia.Headless.NUnit) qui inclut également Avalonia.
- [Avalonia.Themes.Fluent](https://www.nuget.org/packages/Avalonia.Themes.Fluent) car même les contrôles headless ont besoin d'un thème.

:::tip
La plateforme headless ne nécessite pas de thème spécifique, et il est possible de remplacer FluentTheme par n'importe quel autre.
:::

## Configurer l'application

Comme dans toute autre application Avalonia, une instance `Application` doit être créée, et les thèmes doivent être appliqués. Lors de l'utilisation de la plateforme headless, la configuration n'est pas très différente de celle d'une application Avalonia classique et peut généralement être réutilisée.

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

:::note
En général, la méthode `BuildAvaloniaApp` est définie dans le fichier Program.cs, mais les tests NUnit/XUnit ne l'ont pas, donc elle est définie dans le fichier `App` à la place.
:::

## Initialiser les tests NUnit

L'attribut `[AvaloniaTestApplication]` relie les tests dans le projet actuel à l'application spécifique. Il doit être défini une fois par projet dans n'importe quel fichier.

```csharp
[assembly: AvaloniaTestApplication(typeof(TestAppBuilder))]

public class TestAppBuilder
{
    public static AppBuilder BuildAvaloniaApp() => AppBuilder.Configure<App>()
        .UseHeadless(new AvaloniaHeadlessPlatformOptions());
}
```

## Exemple de test

```csharp
[AvaloniaTest]
public void Should_Type_Text_Into_TextBox()
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
    Assert.AreEqual("Hello World", textBox.Text);
}
```

Au lieu de l'attribut typique `[Test]`, nous devons utiliser `[AvaloniaTest]` car il configure le thread UI. De même, au lieu de `[Theory]`, il existe un attribut `[AvaloniaTheory]`.
