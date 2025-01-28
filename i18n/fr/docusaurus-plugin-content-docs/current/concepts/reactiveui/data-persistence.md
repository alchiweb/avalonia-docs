# Persistance des Données

Pour une meilleure expérience utilisateur, votre application doit être capable de sauvegarder l'état sur le disque lorsque l'application est suspendue et de restaurer l'état lorsque l'application reprend. [ReactiveUI](https://reactiveui.net/) fournit des outils permettant de persister l'état de l'application en sérialisant l'arbre du modèle de vue lorsque l'application se ferme ou se suspend. Dans ce tutoriel, nous allons examiner les capacités de ReactiveUI qui nous aident à gérer l'état qui survit au processus.

### Annotation du Modèle de Vue

La sérialisation du modèle de vue est une affaire délicate. Nous devons décider quelles propriétés du modèle de vue sauvegarder lors de la fermeture de l'application et lesquelles recréer. En prenant notre modèle de vue typique de l'écran de recherche, nous voulons définitivement sauvegarder et restaurer la requête de recherche, donc nous annotons la propriété publique avec l'attribut `[DataMember]`. Nous ne voulons pas sauvegarder l'état des commandes sur le disque, donc nous marquons la commande avec l'attribut `[IgnoreDataMember]`. Ces attributs sont disponibles dans la bibliothèque standard, mais on peut facilement utiliser d'autres annotations, par exemple, `[JsonProperty]` ou `[JsonIgnore]`.

```csharp
[DataContract]
public class MainViewModel : ReactiveObject
{
    private string _searchQuery;

    public MainViewModel() 
    {
        var canSearch = this
            .WhenAnyValue(x => x.SearchQuery)
            .Select(query => !string.IsNullOrWhiteSpace(query));
        
        Search = ReactiveCommand.CreateFromTask(
            () => Task.Delay(1000), // une opération de longue durée
            canSearch);
    }

    [IgnoreDataMember]
    public ReactiveCommand<Unit, Unit> Search { get; }
    
    [DataMember]
    public string SearchQuery 
    {
        get => _searchQuery;
        set => this.RaiseAndSetIfChanged(ref _searchQuery, value);
    }
}
```

Il n'est pas nécessaire de sauvegarder l'état d'une [commande réactive](https://reactiveui.net/docs/handbook/commands/) qui implémente l'interface `ICommand`. La classe `ReactiveCommand<TIn, TOut>` est généralement initialisée dans le constructeur, son indicateur `CanExecute` dépend généralement entièrement des propriétés du modèle de vue et est recalculé chaque fois que l'une de ces propriétés change.

### Création de la Vue

Ensuite, nous modifions la classe `MainWindow` pour suivre le modèle standard de ReactiveUI.

```csharp
public class MainWindow : ReactiveWindow<MainViewModel>
{
    public MainWindow()
    {
        AvaloniaXamlLoader.Load(this);
        this.WhenActivated(disposable => { });
    }
}
```

Le XAML de notre `ReactiveWindow` ressemblera à ce qui suit :

```xml
<Window xmlns="https://github.com/avaloniaui"
        xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
        x:Class="ReactiveUI.Samples.Suspension.MainWindow"
        Title="ReactiveUI.Samples.Suspension">
    <TextBox Text="{Binding SearchQuery, Mode=TwoWay}"
             Watermark="Entrez la requête de recherche ici"
             Margin="20" />
</Window>
```

### Création du Pilote de Suspension

Ici, nous fournissons l'implémentation qui utilise `Newtonsoft.Json` et sauvegarde l'état de l'application dans un fichier texte brut.

```csharp
public class NewtonsoftJsonSuspensionDriver : ISuspensionDriver
{
    private readonly string _file;
    private readonly JsonSerializerSettings _settings = new JsonSerializerSettings
    {
        TypeNameHandling = TypeNameHandling.All
    };

    public NewtonsoftJsonSuspensionDriver(string file) => _file = file;

    public IObservable<Unit> InvalidateState()
    {
        if (File.Exists(_file)) 
            File.Delete(_file);
        return Observable.Return(Unit.Default);
    }

    public IObservable<object> LoadState()
    {
        var lines = File.ReadAllText(_file);
        var state = JsonConvert.DeserializeObject<object>(lines, _settings);
        return Observable.Return(state);
    }

    public IObservable<Unit> SaveState(object state)
    {
        var lines = JsonConvert.SerializeObject(state, _settings);
        File.WriteAllText(_file, lines);
        return Observable.Return(Unit.Default);
    }
}
```

### Raccordement des Éléments Ensemble

À la dernière étape, nous initialisons le `AutoSuspendHelper`, suivant le [nouveau modèle de durée de vie d'Avalonia](https://docs.avaloniaui.net/docs/concepts/application-lifetimes). Dans le fichier `App.xaml.cs`, nous créons une surcharge pour la méthode `OnFrameworkInitializationCompleted` et initialisons les variables nécessaires au bon fonctionnement de la fonction de suspension.

```csharp
public class App : Application
{
    public override void Initialize() => AvaloniaXamlLoader.Load(this);

    public override void OnFrameworkInitializationCompleted()
    {
        // Créer l'AutoSuspendHelper.
        var suspension = new AutoSuspendHelper(ApplicationLifetime);
        RxApp.SuspensionHost.CreateNewAppState = () => new MainViewModel();
        RxApp.SuspensionHost.SetupDefaultSuspendResume(new NewtonsoftJsonSuspensionDriver("appstate.json"));
        suspension.OnFrameworkInitializationCompleted();

        // Charger l'état du modèle de vue sauvegardé.
        var state = RxApp.SuspensionHost.GetAppState<MainViewModel>();
        new MainWindow {DataContext = state}.Show();
        base.OnFrameworkInitializationCompleted();
    }
}
```

Dans le fichier `Program.cs`, nous ajoutons un appel à `.UseReactiveUI()` dans le constructeur de l'application.

```csharp
internal static class Program
{
    public static void Main(string[] args) => BuildAvaloniaApp()
        .StartWithClassicDesktopLifetime(args);

    public static AppBuilder BuildAvaloniaApp()
        => AppBuilder.Configure<App>()
            .UsePlatformDetect()
            .UseReactiveUI()
            .LogToDebug();
}
```

Maintenant, nous pouvons exécuter l'application. Si nous tapons du texte dans le `TextBox`, il sera présent même si nous fermons l'instance de l'application et en lançons une nouvelle. L'état est sauvegardé dans le fichier `appstate.json` qui se trouve à proximité de l'exécutable du programme.

```bash
# Si vous ciblez .NET Core 3, utilisez
# netcoreapp3.0 comme argument CLI.
dotnet run --framework netcoreapp2.1
```

Consultez le billet de blog "[Enregistrer l'état de routage sur le disque dans une application GUI .NET Core multiplateforme avec ReactiveUI et Avalonia](https://habr.com/ru/post/462307/)" pour explorer comment combiner la fonctionnalité de persistance des données avec le routage ReactiveUI et l'injection de dépendances. Consultez également [le code source de l'application d'exemple](https://github.com/reactiveui/ReactiveUI.Samples/tree/a7d06759e27fa17f9c6a77018362a2f8e0c30fa6/avalonia) dans le dépôt ReactiveUI.Samples.

