---
id: activatable-lifetime
title: Activatable Lifetime
---

# Durée de vie activable <MinVersion version="11.1" />

Le service `IActivatableLifetime` définit un ensemble de méthodes et d'événements liés au cycle de vie d'activation et de désactivation d'une application. `IActivatableLifetime` est un service au niveau de l'application qui peut être accessible depuis l'instance de l'application en utilisant la méthode `TryGetService` : `Application.Current.TryGetService<IActivatableLifetime>()`.

## Événements

### Activated

Un événement qui est déclenché lorsque l'application est activée pour diverses raisons décrites par l'énumération ActivationKind.

### Deactivated

Un événement qui est déclenché lorsque l'application est désactivée pour diverses raisons décrites par l'énumération ActivationKind.

## Méthodes

### TryLeaveBackground

Indique à l'application qu'elle doit tenter de quitter son état d'arrière-plan.
Retourne vrai si cela a été possible et si la plateforme le prend en charge. Faux sinon.

:::note
Par exemple, sur macOS, cela serait [NSApp unhide].
:::

### TryEnterBackground

Indique à l'application qu'elle doit tenter d'entrer dans son état d'arrière-plan.

Retourne vrai si cela a été possible et si la plateforme le prend en charge. Faux sinon.

:::note
Par exemple, sur macOS, cela serait [NSApp hide].
:::

## Exemples

### Gestion de l'entrée et de la sortie de l'état d'arrière-plan de l'application

Dans certaines applications, vous pourriez vouloir mettre en pause ou arrêter certains traitements de code pendant que l'application est en arrière-plan.
Cela pourrait être la mise en pause de la lecture multimédia ou la désactivation des requêtes HTTP récurrentes.

```csharp
if (Application.Current.TryGetFeature<IActivatableLifetime>() is { } activatableLifetime)
{
    activatableLifetime.Activated += (sender, args) =>
    {
        if (args.Kind == ActivationKind.Background)
        {
            Console.WriteLine($"L'application est sortie de l'arrière-plan");
        }
    };
    activatableLifetime.Deactivated += (sender, args) =>
    {
        if (args.Kind == ActivationKind.Background)
        {
            Console.WriteLine($"L'application est entrée en arrière-plan");
        }
    };
}
```

### Gestion de l'activation URI

Certaines applications peuvent avoir besoin de prendre en charge l'activation par protocole, ou ce qu'on appelle souvent - le deep linking. Les schémas de lien (protocoles) qui sont enregistrés dans le système et associés à l'application. Une fois enregistrés, le système d'exploitation redirigera toujours ces liens vers l'application.

L'application peut gérer ces liens de différentes manières. Mais les cas d'utilisation typiques seraient soit d'activer la navigation vers une page spécifique, soit de les utiliser comme une [URL de redirection dans les opérations OAuth](https://www.oauth.com/oauth2-servers/oauth-native-apps/redirect-urls-for-native-apps/).


```csharp
if (Application.Current.TryGetFeature<IActivatableLifetime>() is { } activatableLifetime)
{
    activatableLifetime.Activated += (s, a) =>
   {
        if (a is ProtocolActivatedEventArgs protocolArgs && protocolArgs.Kind == ActivationKind.OpenUri)
        {
            Console.WriteLine($"Application activée via l'Uri : {protocolArgs.Uri}");
        }
   };
}
```

:::note
Pour activer la gestion des protocoles pour votre application, vous devez suivre les instructions spécifiques à la plateforme pour mettre à jour le manifeste.
Sur macOS et iOS, vous devez ajouter CFBundleURLTypes avec le segment CFBundleURLSchemes à votre `Info.plist`. Voir https://rderik.com/blog/creating-app-custom-url-scheme/ (ignorez la partie Swift, car elle est gérée par `IActivatableLifetime`).
Sur Android, vous devez ajouter `intent-filter` avec un `android:scheme` spécifique à votre `AndroidManifest.xml`. Voir https://developer.android.com/training/app-links/deep-linking pour plus de détails (ignorez les parties Kotlin/Java, car elles sont gérées par `IActivatableLifetime`).
:::

## Compatibilité avec les différentes plateformes :

| Feature        |  Windows | macOS | Linux | Browser | Android |  iOS | Tizen |
|---------------|-------|-------|-------|-------|-------|-------|-------|
| `ActivationKind.Background` | ✖ | ✔ | ✖ | ✔ | ✔ | ✔ | ✖ |
| `ActivationKind.File` | ✖ | ✔ | ✖ | ✖ | ✔ | ✔ | ✖ |
| `ActivationKind.OpenUri` | ✖ | ✔ | ✖ | ✖ | ✔ | ✔ | ✖ |
| `ActivationKind.Reopen` | ✖ | ✔ | ✖ | ✖ | ✖ | ✖ | ✖ |
| `TryLeaveBackground`  | ✖ | ✔ | ✖ | ✖ | ✖ | ✖ | ✖ |
| `TryEnterBackground` | ✖ | ✔ | ✖ | ✖ | ✔ | ✖ | ✖ |

Voir https://github.com/AvaloniaUI/Avalonia/issues/15316 pour plus d'informations sur les plateformes actuellement prises en charge et non prises en charge.