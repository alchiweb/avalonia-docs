---
id: how-to-use-fonts
title: How To Use Custom Fonts
---

Customizing your Avalonia application with unique fonts can add a distinctive look and feel. This guide will walk you through the process of integrating custom fonts into your Avalonia application.

<GitHubSampleLink title="Google Fonts" link="https://github.com/AvaloniaUI/AvaloniaUI.QuickGuides/tree/main/GoogleFonts"/>

## Add Your Custom Font to the Project

Before you can use a custom font, you need to include it in your project.

In this guide, we will be using a font called [Nunito](https://fonts.google.com/specimen/Nunito) which is already stored in our application resources under `avares://GoogleFonts/Assets/Fonts`.

Ensure that the fonts have the build property set to `AvaloniaResource`.

## Declare Your Font in Application Resources

In your Avalonia application, open your `App.xaml` file and include your custom font inside `<Application.Resources>`. Assign it a key, which you will use to reference it in your application. In this case, we have assigned the key `NunitoFont`.

```xml title="App.axaml"
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="GoogleFonts.App"
             RequestedThemeVariant="Default">
    <Application.Styles>
        <FluentTheme />
    </Application.Styles>
    // highlight-start
    <Application.Resources>
        <FontFamily x:Key="NunitoFont">avares://GoogleFonts/Assets/Fonts#Nunito</FontFamily>
    </Application.Resources>
     // highlight-end
</Application>
```

## Use Your Custom Font
Once your font is declared in your application resources, you can use it in your application.

To reference your custom font, use the `FontFamily` attribute with the `StaticResource` markup extension. You need to pass the key of the declared font as the parameter. In this case, `NunitoFont` is the key for our custom font.

Here's an example of how to apply our custom `Nunito` font to a `TextBlock`:

```xml
<TextBlock Text="{Binding Greeting}" 
           FontSize="70" 
           FontFamily="{StaticResource NunitoFont}" 
           HorizontalAlignment="Center" VerticalAlignment="Center"/>
```

In the above example, the `TextBlock` control will use the `Nunito` font that we declared in our application resources. The text bound to the `TextBlock` will now appear in the `Nunito` font at the specified font size of 70.

Remember that the `FontFamily` attribute can be applied to any control that has the `FontFamily` property, meaning you can use your custom font throughout your application.

And that's it! You've successfully integrated a custom font into your Avalonia application. Now you can add a unique touch to your application's UI with the fonts of your choice.

## Adding a Font to the Font Collection

The `EmbeddedFontCollection` provides an easy way to add fonts to your application's collection of fonts without requiring a reference to a resource when using them. For example, instead of setting the `FontFamily` to `{StaticResource NunitoFont}`, you can simply reference the name of the font family directly.

```xml
<TextBlock
    Text="{Binding Greeting}"
    FontSize="70"
    FontFamily="Nunito"
    HorizontalAlignment="Center" VerticalAlignment="Center" />
```

This requires additional setup on application construction, but it removes the need to remember a unique resource key when authoring your controls.

### Defining a Font Collection

First, we need to define a collection of fonts by specifying the font family name and the directory in which the font files reside.

```csharp title="InterFontCollection.cs"
public sealed class InterFontCollection : EmbeddedFontCollection
{
    public InterFontCollection() : base(
        new Uri("fonts:Inter", UriKind.Absolute),
        new Uri("avares://Avalonia.Fonts.Inter/Assets", UriKind.Absolute))
    {
    }
}
```

Here, `Inter` is the name of the font family and `avares://Avalonia.Fonts.Inter/Assets` is the resource locator for the directory containing the font files. The actual names of the font files are not significant since the `EmbeddedFontCollection` will search every file in the given directory and only load those fonts with the given font family name.

For more information on how to create a resource locator, see [Assets](../../basics/user-interface/assets) for a primer on including assets in your project.

### Adding the Font Collection

Next, we need to add this font collection to the application. This can be done by using `AppBuilder.ConfigureFonts` to configure the `FontManager` to include your fonts on application construction.

```csharp title="Program.cs"
public static class Program
{
    [STAThread]
    public static void Main(string[] args) =>
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);

    public static AppBuilder BuildAvaloniaApp() =>
        AppBuilder.Configure<App>()
            .UsePlatformDetect()
// highlight-start
            .ConfigureFonts(fontManager =>
            {
                fontManager.AddFontCollection(new InterFontCollection());
            })
// highlight-end
            .LogToTrace();
}
```

### Creating Font Packages

The [`Avalonia.Fonts.Inter`](https://github.com/AvaloniaUI/Avalonia/tree/master/src/Avalonia.Fonts.Inter) package shows how you can create a separate project to contain one or many fonts that you might use in multiple projects. Once you have created and published a project similar to this, using the font becomes as simple as appending a method call to your application construction.

```csharp title="Program.cs"
public static class Program
{
    [STAThread]
    public static void Main(string[] args) =>
        BuildAvaloniaApp()
            .StartWithClassicDesktopLifetime(args);

    public static AppBuilder BuildAvaloniaApp() =>
        AppBuilder.Configure<App>()
            .UsePlatformDetect()
// highlight-start
            .WithInterFont()
// highlight-end
            .LogToTrace();
}
```

## Which Fonts Are Supported?

Most TrueType (`.ttf`) and OpenType (`.otf`, `.ttf`) fonts are supported. However, some font features, such as "Variable fonts" are not currently supported (see: [Issue #11092](https://github.com/AvaloniaUI/Avalonia/issues/11092)).