# Routing

import ReactiveUIRoutingScreenshot from '/img/concepts/reactiveui/reactiveui-routing.gif';

Le [routage ReactiveUI](https://reactiveui.net/docs/handbook/routing/) consiste en un [IScreen](https://reactiveui.net/api/reactiveui/iscreen/) qui contient l'état de routage actuel [RoutingState](https://reactiveui.net/api/reactiveui/routingstate/), plusieurs [IRoutableViewModel](https://reactiveui.net/api/reactiveui/iroutableviewmodel/) et un contrôle XAML spécifique à la plateforme appelé [RoutedViewHost](https://github.com/AvaloniaUI/Avalonia/blob/55458cf7af24d6c987268ab5ff8a1ead1173310b/src/Avalonia.ReactiveUI/RoutedViewHost.cs). `RoutingState` gère la pile de navigation du modèle de vue et permet aux modèles de vue de naviguer vers d'autres modèles de vue. `IScreen` est la racine d'une pile de navigation ; malgré son nom, ses vues n'ont pas besoin d'occuper tout l'écran. `RoutedViewHost` surveille une instance de `RoutingState`, répondant à tout changement dans la pile de navigation en créant et en intégrant la vue appropriée.

## Exemple de routage

Créez un nouveau projet vide à partir des modèles Avalonia. Pour utiliser ceux-ci, clonez le dépôt [avalonia-dotnet-templates](https://github.com/AvaloniaUI/avalonia-dotnet-templates), installez les modèles et créez un nouveau projet nommé `RoutingExample` basé sur le modèle `avalonia.app`. Installez le package `Avalonia.ReactiveUI` dans le projet.

```bash
git clone https://github.com/AvaloniaUI/avalonia-dotnet-templates
dotnet new --install ./avalonia-dotnet-templates
dotnet new avalonia.app -o RoutingExample
cd ./RoutingExample
dotnet add package Avalonia.ReactiveUI
```

**FirstViewModel.cs**

Tout d'abord, créez des modèles de vue routables et les vues correspondantes. Nous dérivons des modèles de vue routables de l'interface `IRoutableViewModel` de l'espace de noms `ReactiveUI`, ainsi que de `ReactiveObject`. `ReactiveObject` est la classe de base pour les [classes de modèle de vue](https://reactiveui.net/docs/handbook/view-models/), et elle implémente `INotifyPropertyChanged`.

```csharp
namespace RoutingExample
{
    public class FirstViewModel : ReactiveObject, IRoutableViewModel
    {
        // Référence à IScreen qui possède le modèle de vue routable.
        public IScreen HostScreen { get; }

        // Identifiant unique pour le modèle de vue routable.
        public string UrlPathSegment { get; } = Guid.NewGuid().ToString().Substring(0, 5);

        public FirstViewModel(IScreen screen) => HostScreen = screen;
    }
}
```

**FirstView.xaml**

```xml
<UserControl xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             x:Class="RoutingExample.FirstView">
    <StackPanel HorizontalAlignment="Center"
                VerticalAlignment="Center">
        <TextBlock Text="Salut, je suis la première vue !" />
        <TextBlock Text="{Binding UrlPathSegment}" />
    </StackPanel>
</UserControl>
```

**FirstView.xaml.cs**

Si nous devons gérer l'activation et la désactivation du modèle de vue, nous ajoutons alors un appel à WhenActivated à la vue. En général, une règle de base est d'ajouter toujours WhenActivated à vos vues, consultez la documentation sur [l'activation](../../concepts/reactiveui/view-activation) pour plus d'informations.

```csharp
namespace RoutingExample
{
    public class FirstView : ReactiveUserControl<FirstViewModel>
    {
        public FirstView()
        {
            this.WhenActivated(disposables => { });
            AvaloniaXamlLoader.Load(this);
        }
    }
}
```

**MainWindowViewModel.cs**

Ensuite, créez un modèle de vue implémentant l'interface `IScreen`. Il contient l'état de routage actuel `RoutingState` qui gère la pile de navigation. `RoutingState` contient également des commandes d'aide vous permettant de naviguer en arrière et en avant.

En fait, vous pouvez utiliser autant d'`IScreen` que vous le souhaitez dans votre application. Malgré son nom, il n'est pas nécessaire qu'il occupe tout l'écran. Vous pouvez utiliser le routage imbriqué, placer des `IScreen` côte à côte, etc.

```csharp
namespace RoutingExample
{
    public class MainWindowViewModel : ReactiveObject, IScreen
    {
        // Le routeur associé à cet écran.
        // Requis par l'interface IScreen.
        public RoutingState Routeur { get; } = new RoutingState();

        // La commande qui navigue un utilisateur vers le premier modèle de vue.
        public ReactiveCommand<Unit, IRoutableViewModel> AllerSuivant { get; }

        // La commande qui navigue un utilisateur en arrière.
        public ReactiveCommand<Unit, IRoutableViewModel> Retourner => Routeur.NavigateBack;

        public MainWindowViewModel()
        {
            // Gérer l'état de routage. Utilisez la commande Router.Navigate.Execute
            // pour naviguer vers différents modèles de vue. 
            //
            // Notez que la méthode Navigate.Execute accepte une instance 
            // d'un modèle de vue, cela vous permet de passer des paramètres à 
            // vos modèles de vue, ou de réutiliser des modèles de vue existants.
            //
            AllerSuivant = ReactiveCommand.CreateFromObservable(
                () => Routeur.Navigate.Execute(new FirstViewModel(this))
            );
        }
    }
}
```

**MainWindow.xaml**

Maintenant, nous devons placer le contrôle XAML `RoutedViewHost` dans notre vue principale. Il résoudra et intégrera les vues appropriées pour les modèles de vue en fonction de l'implémentation `IViewLocator` fournie et de l'instance `Router` passée de type `RoutingState`. Notez que vous devez importer l'espace de noms `rxui` pour que `RoutedViewHost` fonctionne. De plus, vous pouvez remplacer les animations qui sont jouées lorsque `RoutedViewHost` change de vue — il suffit de remplacer la propriété `RoutedViewHost.PageTransition` dans XAML.

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:rxui="http://reactiveui.net"
        xmlns:app="clr-namespace:RoutingExample"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
        xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
        mc:Ignorable="d" d:DesignWidth="800" d:DesignHeight="450"
        x:Class="RoutingExample.MainWindow"
        Title="RoutingExample">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="*" />
            <RowDefinition Height="Auto" />
        </Grid.RowDefinitions>
        <rxui:RoutedViewHost Grid.Row="0" Router="{Binding Router}">
            <rxui:RoutedViewHost.DefaultContent>
                <TextBlock Text="Contenu par défaut"
                           HorizontalAlignment="Center"
                           VerticalAlignment="Center" />
            </rxui:RoutedViewHost.DefaultContent>
            <rxui:RoutedViewHost.ViewLocator>
                <!-- Voir la section AppViewLocator.cs ci-dessous -->
                <app:AppViewLocator />
            </rxui:RoutedViewHost.ViewLocator>
        </rxui:RoutedViewHost>
        <StackPanel Grid.Row="1" Orientation="Horizontal" Margin="15">
            <StackPanel.Styles>
                <Style Selector="StackPanel > :is(Control)">
                    <Setter Property="Margin" Value="2"/>
                </Style>
                <Style Selector="StackPanel > TextBlock">
                    <Setter Property="VerticalAlignment" Value="Center"/>
                </Style>
            </StackPanel.Styles>
            <Button Content="Aller au suivant" Command="{Binding GoNext}" />
            <Button Content="Retourner en arrière" Command="{Binding GoBack}" />
            <TextBlock Text="{Binding Router.NavigationStack.Count}" />
        </StackPanel>
    </Grid>
</Window>
```

Pour désactiver les animations, il suffit de définir la propriété `RoutedViewHost.PageTransition` sur `{x:Null}`, comme ceci :

```xml
<rxui:RoutedViewHost Grid.Row="0" Router="{Binding Router}" PageTransition="{x:Null}">
    <rxui:RoutedViewHost.DefaultContent>
        <TextBlock Text="Contenu par défaut"
                   HorizontalAlignment="Center"
                   VerticalAlignment="Center" />
    </rxui:RoutedViewHost.DefaultContent>
</rxui:RoutedViewHost>
```

**AppViewLocator.cs**

L'`AppViewLocator` que nous passons au contrôle `RoutedViewHost` déclaré dans le balisage `MainWindow.xaml` ci-dessus est responsable de la résolution d'une vue en fonction du type du ViewModel. L'instance `IScreen.Router` de type `RoutingState` détermine quel ViewModel doit être affiché actuellement. Voir [Localisation de la vue](https://reactiveui.net/docs/handbook/view-location/) pour plus de détails. La plus simple des implémentations `IViewLocator` basée sur le matching de motifs pourrait ressembler à ceci :

```csharp
namespace RoutingExample
{
    public class AppViewLocator : ReactiveUI.IViewLocator
    {
        public IViewFor ResolveView<T>(T viewModel, string contract = null) => viewModel switch
        {
            FirstViewModel context => new FirstView { DataContext = context },
            _ => throw new ArgumentOutOfRangeException(nameof(viewModel))
        };
    }
}
```

**MainWindow.xaml.cs**

Voici le code-behind pour `MainWindow.xaml` déclaré ci-dessus.

```csharp
namespace RoutingExample
{
    public class MainWindow : ReactiveWindow<MainWindowViewModel>
    {
        public MainWindow()
        {
            this.WhenActivated(disposables => { });
            AvaloniaXamlLoader.Load(this);
        }
    }
}
```

**App.axaml.cs**

Assurez-vous d'initialiser le `DataContext` de votre vue racine dans `App.axaml.cs`.

```csharp
public override void OnFrameworkInitializationCompleted()
{
    if (ApplicationLifetime is IClassicDesktopStyleApplicationLifetime desktop)
    {
        desktop.MainWindow = new MainWindow
        {
            DataContext = new MainWindowViewModel(),
        };
    }

    base.OnFrameworkInitializationCompleted();
}
```

Enfin, ajoutez `.UseReactiveUI()` à votre `AppBuilder` :

```csharp
namespace RoutingExample
{
    public static class Program
    {
        public static void Main(string[] args)
        {
            BuildAvaloniaApp().StartWithClassicDesktopLifetime(args);
        }

        public static AppBuilder BuildAvaloniaApp() =>
            AppBuilder.Configure<App>()
                .UseReactiveUI()
                .UsePlatformDetect()
                .LogToDebug();
    }
}
```

Maintenant, vous pouvez exécuter l'application et voir le routage en action !

```bash
dotnet run --framework netcoreapp2.1
```

<img src={ReactiveUIRoutingScreenshot} alt="" />

