---
id: macOS
title: Déploiement sur macOS
---

Les applications macOS sont généralement distribuées dans un `.app` [package d'application](https://en.wikipedia.org/wiki/Bundle_%28macOS%29#macOS_application_bundles). Pour faire fonctionner les projets .NET Core et Avalonia dans un package `.app`, un travail supplémentaire doit être effectué après que votre application a traversé le processus de publication.

Avec Avalonia, vous aurez une structure de dossier `.app` qui ressemble à ceci :

```csharp
MyProgram.app
|
----Contents\
    |
    ------_CodeSignature\ (stocke les informations de signature de code)
    |     |
    |     ------CodeResources
    |
    ------MacOS\ (tous vos fichiers DLL, etc. -- la sortie de `dotnet publish`)
    |     |
    |     ---MyProgram
    |     |
    |     ---MyProgram.dll
    |     |
    |     ---Avalonia.dll
    |
    ------Resources\
    |     |
    |     -----MyProgramIcon.icns (fichier d'icône)
    |
    ------Info.plist (stocke des informations sur votre identifiant de package, version, etc.)
    ------embedded.provisionprofile (fichier contenant des informations de signature)
```

Pour plus d'informations sur `Info.plist`, consultez [la documentation d'Apple ici](https://developer.apple.com/documentation/bundleresources/information_property_list).

## Création du package d'application

Il existe plusieurs options pour créer la structure de fichiers/dossiers `.app`. Vous pouvez le faire sur n'importe quel système d'exploitation, car un fichier `.app` est simplement un ensemble de dossiers disposés dans un format spécifique et les outils ne sont pas spécifiques à un système d'exploitation. Cependant, si vous construisez sur Windows en dehors de WSL, l'exécutable peut ne pas avoir les bons attributs pour l'exécution sur macOS -- vous devrez peut-être exécuter `chmod +x` sur la sortie binaire publiée (la sortie générée par `dotnet publish`) depuis une machine Unix. C'est la sortie binaire qui se retrouve dans le dossier `MyApp.app/Contents/MacOS/`, et le nom doit correspondre à `CFBundleExecutable`.

La structure `.app` repose sur le fichier `Info.plist` étant correctement formaté et contenant les bonnes informations. Utilisez Xcode pour éditer `Info.plist`, il dispose de l'auto-complétion pour toutes les propriétés. Assurez-vous que :

* La valeur de [`CFBundleExecutable`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleexecutable) correspond au nom binaire généré par `dotnet publish` -- typiquement, c'est le même que le nom de votre assemblage `.dll` **sans** `.dll`.
* [`CFBundleName`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundlename) est défini sur le nom d'affichage de votre application. Si cela dépasse 15 caractères, définissez également [`CFBundleDisplayName`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundledisplayname).
* [`CFBundleIconFile`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleiconfile) est défini sur le nom de votre fichier d'icône `icns` \(y compris l'extension\).
* [`CFBundleIdentifier`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleidentifier) est défini sur un identifiant unique, généralement au format reverse-DNS -- par exemple `com.myapp.macos`.
* [`NSHighResolutionCapable`](https://developer.apple.com/documentation/bundleresources/information_property_list/nshighresolutioncapable) est défini sur vrai \(`<true/>` dans le `Info.plist`\).
* [`CFBundleVersion`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleversion) est défini sur la version de votre bundle, par exemple 1.4.2.
* [`CFBundleShortVersionString`](https://developer.apple.com/documentation/bundleresources/information_property_list/cfbundleshortversionstring) est défini sur la chaîne visible par l'utilisateur pour la version de votre application, par exemple `Major.Minor.Patch`.

Si vous avez besoin d'un enregistrement de protocole ou d'associations de fichiers, ouvrez les fichiers plist d'autres applications dans le dossier Applications et vérifiez leurs champs.

Exemple de protocole :

```xml
  <key>CFBundleURLTypes</key>
  <array>
    <dict>
      <key>CFBundleURLName</key>
      <string>AppName</string>
      <key>CFBundleTypeRole</key>
      <string>Viewer</string>
      <key>CFBundleURLSchemes</key>
      <array>
        <string>i8-AppName</string>
      </array>
    </dict>
  </array>
```

Exemple d'association de fichiers :

```xml
  <key>CFBundleDocumentTypes</key>
  <array>
    <dict>
      <key>CFBundleTypeName</key>
      <string>Sketch</string>
      <key>CFBundleTypeExtensions</key>
      <array>
        <string>sketch</string>
      </array>
      <key>CFBundleTypeIconFile</key>
      <string>icon.icns</string>
      <key>CFBundleTypeRole</key>
      <string>Viewer</string>
      <key>LSHandlerRank</key>
      <string>Default</string>
    </dict>
  </array>
```

Plus de documentation sur les clés possibles de `Info.plist` est disponible [ici](https://developer.apple.com/documentation/bundleresources/information_property_list/bundle_configuration).

Si à un moment donné, l'outil vous donne une erreur indiquant que votre fichier d'assets n'a pas de cible pour `osx-64`, ajoutez les [identifiants d'exécution](https://docs.microsoft.com/en-us/dotnet/core/rid-catalog) suivants en haut du `<PropertyGroup>` dans votre `.csproj` :

```xml
<RuntimeIdentifiers>osx-x64</RuntimeIdentifiers>
```

Ajoutez d'autres identifiants d'exécution si nécessaire. Chacun d'eux doit être séparé par un point-virgule \(;\).

### Remarques sur la création de fichiers d'icônes

Ce type de fichier d'icône ne peut pas seulement être créé sur des appareils Apple, mais il est également possible de le faire sur des appareils Linux.  
Vous pouvez trouver plus d'informations sur la façon d'y parvenir dans cet article de blog :  
[Création d'icônes macOS (icns) sur Linux](https://dentrassi.de/2014/02/25/creating-mac-os-x-icons-icns-on-linux/)

### Remarques sur le fichier exécutable `.app`

Le fichier qui est réellement exécuté par macOS lors du démarrage de votre bundle `.app` **n'aura pas** l'extension standard `.dll`. Si le contenu de votre dossier de publication, qui va à l'intérieur du bundle `.app`, ne contient pas à la fois un `MyApp` \(exécutable\) et un `MyApp.dll`, les choses ne se génèrent probablement pas correctement, et macOS ne pourra probablement pas démarrer votre `.app` correctement.

[Quelques changements récents dans la façon dont .NET Core est distribué et notarié sur macOS](https://docs.microsoft.com/en-us/dotnet/core/install/macos-notarization-issues) ont empêché la génération de l'exécutable `MyApp` (également appelé "hôte d'application" dans la documentation liée). **Vous avez besoin que ce fichier soit généré pour que votre `.app` fonctionne correctement.** Pour vous assurer que cela soit généré, faites l'une des choses suivantes :

* Ajoutez ce qui suit à votre fichier `.csproj` :

```xml
<PropertyGroup>
  <UseAppHost>true</UseAppHost>
</PropertyGroup>
```

* Ajoutez `-p:UseAppHost=true` à votre commande `dotnet publish`.

### dotnet-bundle

:::warning
[dotnet-bundle n'est plus maintenu](https://github.com/egramtel/dotnet-bundle/issues/16#issuecomment-1365767804) mais devrait encore fonctionner.

Il est recommandé de cibler `net6-macos`, qui gérera la génération de paquets.
:::

[dotnet-bundle](https://github.com/egramtel/dotnet-bundle) est un [paquet NuGet](https://www.nuget.org/packages/Dotnet.Bundle/) qui publie votre projet et crée ensuite le fichier `.app` pour vous.

Vous devrez d'abord ajouter le projet en tant que `PackageReference` dans votre projet. Ajoutez-le à votre projet via le gestionnaire de paquets NuGet ou en ajoutant la ligne suivante à votre fichier `.csproj` :

```xml
<PackageReference Include="Dotnet.Bundle" Version="*" />
```

Après cela, vous pouvez créer votre `.app` en exécutant ce qui suit dans la ligne de commande :

```bash
dotnet restore -r osx-x64
dotnet msbuild -t:BundleApp -p:RuntimeIdentifier=osx-x64 -p:UseAppHost=true
```

Vous pouvez spécifier d'autres paramètres pour la commande `dotnet msbuild`. Par exemple, si vous souhaitez publier en mode release :

```bash
dotnet msbuild -t:BundleApp -p:RuntimeIdentifier=osx-x64 -property:Configuration=Release -p:UseAppHost=true
```

ou si vous souhaitez spécifier un nom d'application différent :

```bash
dotnet msbuild -t:BundleApp -p:RuntimeIdentifier=osx-x64 -p:CFBundleDisplayName=MyBestThingEver -p:UseAppHost=true
```

Au lieu de spécifier `CFBundleDisplayName`, etc., sur la ligne de commande, vous pouvez également les spécifier dans votre fichier de projet :

```xml
<PropertyGroup>
    <CFBundleName>AppName</CFBundleName> <!-- Définit également le nom du fichier .app -->
    <CFBundleDisplayName>MyBestThingEver</CFBundleDisplayName>
    <CFBundleIdentifier>com.example</CFBundleIdentifier>
    <CFBundleVersion>1.0.0</CFBundleVersion>
    <CFBundlePackageType>APPL</CFBundlePackageType>
    <CFBundleSignature>????</CFBundleSignature>
    <CFBundleExecutable>AppName</CFBundleExecutable>
    <CFBundleIconFile>AppName.icns</CFBundleIconFile> <!-- Sera copié depuis le répertoire de sortie -->
    <NSPrincipalClass>NSApplication</NSPrincipalClass>
    <NSHighResolutionCapable>true</NSHighResolutionCapable>
</PropertyGroup>
```

Par défaut, `dotnet-bundle` mettra le fichier `.app` au même endroit que la sortie de `publish` : `[répertoire du projet]/bin/{Configuration}/netcoreapp3.1/osx-x64/publish/MyBestThingEver.app`.

Pour plus d'informations sur les paramètres que vous pouvez envoyer, consultez la [documentation dotnet-bundle](https://github.com/egramtel/dotnet-bundle).

Si vous avez créé le `.app` sur Windows, assurez-vous d'exécuter `chmod +x MyApp.app/Contents/MacOS/AppName` depuis une machine Unix. Sinon, l'application ne démarrera pas sur macOS.

### Manuel

Tout d'abord, publiez votre application \([documentation dotnet publish](https://docs.microsoft.com/en-us/dotnet/core/tools/dotnet-publish)\) :

```bash
dotnet publish -r osx-x64 --configuration Release -p:UseAppHost=true
```

Créez votre fichier `Info.plist`, en ajoutant ou en modifiant les clés si nécessaire :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>CFBundleIconFile</key>
    <string>myicon-logo.icns</string>
    <key>CFBundleIdentifier</key>
    <string>com.identifier</string>
    <key>CFBundleName</key>
    <string>MyApp</string>
    <key>CFBundleVersion</key>
    <string>1.0.0</string>
    <key>LSMinimumSystemVersion</key>
    <string>10.12</string>
    <key>CFBundleExecutable</key>
    <string>MyApp.Avalonia</string>
    <key>CFBundleInfoDictionaryVersion</key>
    <string>6.0</string>
    <key>CFBundlePackageType</key>
    <string>APPL</string>
    <key>CFBundleShortVersionString</key>
    <string>1.0</string>
    <key>NSHighResolutionCapable</key>
    <true/>
</dict>
</plist>
```

Vous pouvez ensuite créer la structure de dossier de votre `.app` comme décrit en haut de cette page. Si vous souhaitez un script pour le faire pour vous, vous pouvez utiliser quelque chose comme ceci \(macOS/Unix\) :

```bash
#!/bin/bash
APP_NAME="/path/to/your/output/MyApp.app"
PUBLISH_OUTPUT_DIRECTORY="/path/to/your/publish/output/netcoreapp3.1/osx-64/publish/."
# PUBLISH_OUTPUT_DIRECTORY doit pointer vers le répertoire de sortie de votre commande dotnet publish.
# Un exemple est /path/to/your/csproj/bin/Release/netcoreapp3.1/osx-x64/publish/.
# Si vous souhaitez changer les répertoires de sortie, ajoutez `--output /my/directory/path` à votre commande `dotnet publish`.
INFO_PLIST="/path/to/your/Info.plist"
ICON_FILE="/path/to/your/myapp-logo.icns"

if [ -d "$APP_NAME" ]
then
    rm -rf "$APP_NAME"
fi

mkdir "$APP_NAME"

mkdir "$APP_NAME/Contents"
mkdir "$APP_NAME/Contents/MacOS"
mkdir "$APP_NAME/Contents/Resources"

cp "$INFO_PLIST" "$APP_NAME/Contents/Info.plist"
cp "$ICON_FILE" "$APP_NAME/Contents/Resources/$ICON_FILE"
cp -a "$PUBLISH_OUTPUT_DIRECTORY" "$APP_NAME/Contents/MacOS"
```

Si vous avez créé le `.app` sur Windows, assurez-vous d'exécuter `chmod +x MyApp.app/Contents/MacOS/AppName` depuis une machine Unix. Sinon, l'application ne démarrera pas sur macOS.

## Signature de votre application

Une fois que vous avez créé votre fichier `.app`, vous voudrez probablement signer votre application afin qu'elle puisse être notariée et distribuée à vos utilisateurs sans que Gatekeeper ne vous pose de problème. La notarisation est requise pour les applications distribuées en dehors de l'App Store à partir de macOS 10.15 (Catalina), et vous devrez activer le [runtime renforcé](https://developer.apple.com/documentation/security/hardened_runtime?language=objc) et exécuter `codesign` sur votre `.app` afin de le notariser avec succès.

Vous aurez besoin d'un ordinateur Mac pour cette étape, malheureusement, car nous devons exécuter l'outil en ligne de commande `codesign` qui est fourni avec Xcode.

### Exécution de codesign et activation du runtime renforcé

L'activation du runtime renforcé se fait dans la même étape que la signature du code. Vous devez signer tout dans le bundle `.app` sous le dossier `Contents/MacOS`, ce qui est plus facile à faire avec un script étant donné qu'il y a beaucoup de fichiers. Pour signer vos fichiers, vous avez besoin d'un compte développeur Apple. Pour notariser votre application, vous devrez suivre les étapes suivantes avec un [certificat d'identité de développeur](https://developer.apple.com/developer-id/), ce qui nécessite un abonnement payant au programme développeur Apple.

Vous devrez également avoir les outils de ligne de commande Xcode installés. Vous pouvez les obtenir en installant Xcode et en l'exécutant ou en exécutant `xcode-select --install` dans la ligne de commande et en suivant les instructions pour installer les outils.

Tout d'abord, activez le runtime renforcé avec des [exceptions](https://developer.apple.com/documentation/security/hardened_runtime?language=objc) en créant un fichier `MyAppEntitlements.entitlements` :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.cs.allow-jit</key>
    <true/>
    <key>com.apple.security.automation.apple-events</key>
    <true/>
</dict>
</plist>
```

Ensuite, exécutez ce script pour effectuer toute la signature de code pour vous :

```bash
#!/bin/bash
APP_NAME="/chemin/vers/votre/sortie/MyApp.app"
ENTITLEMENTS="/chemin/vers/votre/MyAppEntitlements.entitlements"
SIGNING_IDENTITY="Identité de développeur : MonNomDeSociété" # correspond au nom du certificat dans le Trousseau d'accès

find "$APP_NAME/Contents/MacOS/"|while read fname; do
    if [[ -f $fname ]]; then
        echo "[INFO] Signature de $fname"
        codesign --force --timestamp --options=runtime --entitlements "$ENTITLEMENTS" --sign "$SIGNING_IDENTITY" "$fname"
    fi
done

echo "[INFO] Signature du fichier app"

codesign --force --timestamp --options=runtime --entitlements "$ENTITLEMENTS" --sign "$SIGNING_IDENTITY" "$APP_NAME"
```

La partie `--options=runtime` de la ligne `codesign` est ce qui active le runtime renforcé avec votre application. Parce que [.NET Core peut ne pas être entièrement compatible avec le runtime renforcé](https://github.com/dotnet/runtime/issues/10562#issuecomment-503013071), nous ajoutons quelques exceptions pour utiliser du code compilé JIT et permettre l'envoi d'événements Apple. L'exception pour le code compilé JIT est nécessaire pour exécuter des applications Avalonia sous un runtime renforcé. Nous ajoutons la deuxième exception pour les événements Apple afin de corriger une erreur qui apparaît dans Console.app.

Remarque : Microsoft liste [d'autres exceptions de runtime durcies](https://docs.microsoft.com/en-us/dotnet/core/install/macos-notarization-issues#default-entitlements) comme étant requises pour .NET Core. La seule dont vous avez réellement besoin pour exécuter une application Avalonia est `com.apple.security.cs.allow-jit`. Les autres peuvent imposer des risques de sécurité à votre application. Utilisez avec prudence.

Une fois que votre application est signée, vous pouvez vérifier qu'elle est correctement signée en vous assurant que la commande suivante ne renvoie aucune erreur :

```bash
codesign --verify --verbose /path/to/MyApp.app
```

### Notarisation de votre logiciel

La notarisation permet à votre application d'être distribuée en dehors du Mac App Store. Vous pouvez en lire plus [ici](https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution). Si vous rencontrez des problèmes pendant le processus, Apple a un document utile sur les solutions potentielles [ici](https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/resolving_common_notarization_issues?language=objc).

Pour plus d'informations sur la personnalisation de votre flux de travail de notarisation et d'autres options que vous pourriez avoir besoin d'envoyer lors de l'exécution de `xcrun altool`, [consultez la documentation d'Apple](https://developer.apple.com/documentation/xcode/notarizing_macos_software_before_distribution/customizing_the_notarization_workflow?language=objc).

Les étapes suivantes ont été modifiées à partir de [ce post StackOverflow](https://stackoverflow.com/a/53121755/3938401) :

1. Assurez-vous que votre `.app` est correctement signé.
2. Mettez votre `.app` dans un fichier `.zip`, par exemple `MyApp.zip`. Notez que l'utilisation de `zip` fera échouer la notarisation, utilisez plutôt `ditto` comme ceci : `ditto -c -k --sequesterRsrc --keepParent MyApp.app MyApp.zip`.
3. Exécutez `xcrun altool --notarize-app -f MyApp.zip --primary-bundle-id com.unique-identifier-for-this-upload -u nom_utilisateur -p mot_de_passe`. Vous pouvez utiliser un mot de passe dans votre trousseau en passant `-p "@keychain:AC_PASSWORD"`, où AC_PASSWORD est la clé. Le compte doit être enregistré en tant que développeur Apple.
4. Si le téléchargement réussit, vous recevrez un UUID pour votre jeton de demande comme ceci : `28fad4c5-68b3-4dbf-a0d4-fbde8e6a078f`.
5. Vous pouvez vérifier l'état de la notarisation en utilisant ce jeton comme ceci : `xcrun altool --notarization-info 28fad4c5-68b3-4dbf-a0d4-fbde8e6a078f -u nom_utilisateur -p mot_de_passe`. Cela peut prendre du temps -- finalement, cela réussira ou échouera.
6. Si cela réussit, vous devez coller la notarisation à l'application : `xcrun stapler staple MyApp.app`. Vous pouvez valider cela en exécutant `xcrun stapler validate MyApp.app`.

Une fois la notarisation terminée, vous devriez pouvoir distribuer votre application !

:::info
Si vous distribuez votre application dans un `.dmg`, vous voudrez modifier légèrement les étapes :
:::

1. Notarisez votre `.app` comme d'habitude (dans un fichier `.zip`).
2. Ajoutez votre application notarisée et collée (`xcrun stapler`) dans le DMG (le DMG contient maintenant le fichier `.app` notarisé/collé à l'intérieur).
3. Notarisez votre fichier `.dmg` (même commande de base `xcrun altool`, juste avec le fichier `.dmg` pour le drapeau `-f` au lieu du `.zip`)
4. Collez la notarisation au fichier `.dmg` : `xcrun stapler staple MyApp.dmg`

## Emballage pour l'App Store

Vous avez besoin de beaucoup de choses :

* Votre application respecte les [Directives de révision de l'App Store](https://developer.apple.com/app-store/review/guidelines/).
* Votre application respecte les [Directives d'interface humaine de macOS](https://developer.apple.com/design/human-interface-guidelines/macos/overview/themes/).
* Compte développeur Apple, avec votre identifiant Apple connecté.
* Votre application est enregistrée dans [App Store Connect](https://appstoreconnect.apple.com/apps/).
* Application Transporter installée depuis l'App Store.
* Dernière version de Xcode installée avec votre identifiant Apple autorisé.
* Deux certificats : `Installateur de développeur Mac tiers` pour signer le fichier `.pkg` et `Application de développeur Mac tiers` pour signer un bundle.
* Profil de provisionnement de l'App Store - obtenez-le pour votre application [ici](https://developer.apple.com/account/resources/profiles/list).
* Deux droits : un pour signer le `.app` et l'autre pour signer les helpers d'application.
* Le contenu de votre application est [emballé correctement](https://developer.apple.com/documentation/bundleresources/placing_content_in_a_bundle).
* Votre bundle est signé correctement.
* Vos fichiers `.dylib` ne contiennent aucune architecture non-ARM/x64. Vous pouvez les supprimer en utilisant l'outil en ligne de commande `lipo`.
* Votre application est prête à être lancée depuis l'intérieur d'un [sandbox](https://developer.apple.com/library/archive/documentation/Security/Conceptual/AppSandboxDesignGuide/AboutAppSandbox/AboutAppSandbox.html).

### Obtention de certificats

* Allez dans Xcode &gt; Préférences &gt; Compte &gt; Gérer les certificats... 
* Ajoutez-les s'ils n'existent pas.
* Exportez-les avec un mot de passe. 
* Ouvrez-les et importez-les dans le Trousseau d'accès.
* Dans le Trousseau, vous devriez voir ces certificats `Installateur de développeur Mac tiers` et `Distribution Apple`. Si les noms des certificats commencent par d'autres chaînes, vous avez créé un certificat incorrect. Réessayez.
* Développez les clés importées dans le Trousseau et double-cliquez sur une clé privée à l'intérieur.
* Allez dans l'onglet Contrôle d'accès.
* Sélectionnez `Autoriser toutes les applications à accéder à cet élément` si vous ne souhaitez pas entrer un mot de passe de profil Mac pour chaque signature de fichier.

### Sandbox et bundle

L'App Store exige que l'application soit lancée dans un sandbox. Cela signifie que l'application n'aura pas accès à quoi que ce soit et ne pourra pas nuire au PC de l'utilisateur.

Votre application doit être prête pour cela et ne pas se bloquer si un dossier est protégé en lecture/écriture.

Les applications .NET 6 ne se bloqueront pas dans un sandbox uniquement si elles sont publiées avec l'option de fichier unique activée. Exemple :

`dotnet publish src/MyApp.csproj -c Release -f net6.0 -r osx-x64 --self-contained true -p:PublishSingleFile=true`

Le contenu de votre application doit être correctement emballé. [Voici un article d'Apple avec beaucoup d'infos utiles](https://developer.apple.com/documentation/bundleresources/placing_content_in_a_bundle).

Les règles les plus importantes de l'article : 
* Les fichiers `.dll` ne sont pas considérés comme du code par Apple. Ils doivent donc être placés dans le dossier `/Resources` et peuvent ne pas être signés.
* Les fichiers `/MacOS` ne doivent contenir que des exécutables mach-o - l'exécutable de votre application et tout autre exécutable auxiliaire.
* Tous les autres fichiers mach-o `.dylib` doivent être dans le dossier `Frameworks/`.

Pour satisfaire cette exigence sans trop de douleur, vous pouvez utiliser des liens symboliques relatifs depuis le dossier `MacOS/` vers les dossiers `Resources/` et `Frameworks/`. Par exemple :

`ln -s fromFile toFile`

Il est également préférable de réécrire le schéma d'accès aux ressources de votre application pour accéder directement au dossier `Resources/` sans utiliser de liens symboliques, car vous pourriez rencontrer des problèmes d'accès I/O en sandbox.

### Droits et signature de sandbox

Vous devez lire toute la [documentation sur les droits](https://developer.apple.com/documentation/bundleresources/entitlements) et choisir ceux dont votre application a besoin.

D'abord, pour le fichier des droits, il faut signer tous les exécutables auxiliaires dans le dossier `.app/Content/MacOS/`. Cela devrait ressembler à ceci :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.inherit</key>
    <true/>
</dict>
</plist>
```

Ensuite, il faut signer l'exécutable de l'application et l'ensemble du bundle de l'application. Cela devrait contenir toutes les permissions de l'application. Voici un exemple :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>com.apple.security.cs.allow-jit</key>
    <true/>
    <key>com.apple.security.cs.allow-unsigned-executable-memory</key>
    <true/>
    <key>com.apple.security.cs.disable-library-validation</key>
    <true/>
    <key>com.apple.security.cs.allow-dyld-environment-variables</key>
    <true/>
    <key>com.apple.security.app-sandbox</key>
    <true/>
    <key>com.apple.security.temporary-exception.mach-lookup.global-name</key>
    <array>
        <string>com.apple.coreservices.launchservicesd</string>
    </array>
</dict>
</plist>
```

Voici quelques permissions optionnelles dont votre application pourrait avoir besoin :

```xml
    <key>com.apple.security.network.client</key>
    <true/>
    <key>com.apple.security.network.server</key>
    <true/>
    <key>com.apple.security.automation.apple-events</key>
    <true/>
    <key>com.apple.security.files.user-selected.read-write</key>
    <true/>
    <key>com.apple.security.files.bookmarks.document-scope</key>
    <true/>
    <key>com.apple.security.application-groups</key>
    <array>
      <string>[Your Team ID].[Your App ID]</string>
    </array>
```

### Script de packaging

Voici un exemple de script de packaging avec des commentaires

```bash
#Nettoyer les dossiers
rm -rf "App/AppName.app/Contents/MacOS/" 
rm -rf "App/AppName.app/Contents/CodeResources" 
rm -rf "App/AppName.app/Contents/_CodeSignature" 
rm -rf "App/AppName.app/Contents/embedded.provisionprofile" 
mkdir -p "App/AppName.app/Contents/Frameworks/"
mkdir -p "App/AppName.app/Contents/MacOS/"

#Construire l'application
dotnet publish ../../ProjectFolder/AppName.csproj -c release -f net5.0 -r osx-x64 --self-contained true -p:PublishSingleFile=true

#Déplacer l'application
cd ..
cd ..
cp -R -f ProjectFolder/bin/release/net5.0/osx-x64/publish/* "build/osx/App/AppName.app/Contents/MacOS/"
cd "build/osx/"

APP_ENTITLEMENTS="AppEntitlements.entitlements"
APP_SIGNING_IDENTITY="Application de développeur Mac tiers : [***]"
INSTALLER_SIGNING_IDENTITY="Installateur de développeur Mac tiers : [***]"
APP_NAME="App/AppName.app"

#<ici, déplacer les ressources de votre application vers le dossier Resources à l'aide de liens symboliques relatifs>

#<ici, déplacer vos fichiers .dylib vers le dossier Frameworks à l'aide de liens symboliques relatifs>

echo "[INFO] Changer le profil de provisionnement pour AppStore"
\cp -R -f AppNameAppStore.provisionprofile "App/AppName.app/Contents/embedded.provisionprofile"

echo "[INFO] Corriger les architectures de libuv.dylib"
lipo -remove i386 "App/AppName.app/Contents/Frameworks/libuv.dylib" "App/AppName.app/Contents/Frameworks/libuv.dylib"

find "$APP_NAME/Contents/Frameworks/"|while read fname; do
    if [[ -f $fname ]]; then
        echo "[INFO] Signature de $fname"
        codesign --force --sign "$APP_SIGNING_IDENTITY" "$fname"
    fi
done

echo "[INFO] Signature de l'exécutable de l'application"
codesign --force --entitlements "$FILE_ENTITLEMENTS" --sign "$APP_SIGNING_IDENTITY" "App/AppName.app/Contents/MacOS/AppName"

echo "[INFO] Signature du bundle de l'application"
codesign --force --entitlements "$APP_ENTITLEMENTS" --sign "$APP_SIGNING_IDENTITY" "$APP_NAME"

echo "[INFO] Création de AppName.pkg"
productbuild --component App/AppName.app /Applications --sign "$INSTALLER_SIGNING_IDENTITY" AppName.pkg
```

### Tester un paquet

Copiez votre `.app` dans le dossier Applications et lancez-le. S'il se lance correctement - vous avez tout fait correctement. S'il plante - ouvrez l'application Console et vérifiez le rapport de plantage.

### Téléchargement d'un paquet sur l'App Store

Ouvrez l'application Transporter, connectez-vous, sélectionnez votre paquet \*.pkg et attendez la validation et le téléchargement sur l'App Store.

Si vous recevez des erreurs - corrigez-les, regroupez à nouveau l'application, supprimez le fichier dans Transporter et sélectionnez-le à nouveau.

Lorsque le téléchargement réussit - vous verrez votre paquet dans App Store Connect.

## Dépannage

### L'élément de menu de l'application affiche _À propos d'Avalonia_

Cela signifie que votre application ne spécifie probablement pas de menu. Au démarrage, Avalonia crée les éléments de menu par défaut pour une application et ajoute automatiquement l'élément _À propos d'Avalonia_ lorsque aucun menu n'a été configuré. Cela peut être résolu en en ajoutant un à votre `App.xaml` :

```xml
<Application xmlns="https://github.com/avaloniaui"
             xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
             xmlns:local="using:RoadCaptain.App.RouteBuilder"
             x:Class="RoadCaptain.App.RouteBuilder.App">
	<NativeMenu.Menu>
		<NativeMenu>
			<NativeMenuItem Header="About MyApp" Click="AboutMenuItem_OnClick" />
		</NativeMenu>
	</NativeMenu.Menu>
</Application>
```

Le reste des éléments de menu par défaut de macOS sera toujours généré par Avalonia.

### Le nom de l'application dans la barre de menu ne correspond pas

Lorsque vous exécutez une application à partir d'un bundle, le nom de l'application affiché dans la barre de menu est tiré du `Info.plist` dans le bundle au lieu de la propriété `Name` dans `App.xaml`.

Si les noms ne correspondent pas, vérifiez que les valeurs pour `CFBundleName`, `CFBundleDisplayName` et la propriété `Name` sont identiques.

Notez que `CFBundleName` est limité à 15 caractères, si le nom de votre application est plus long, vous devez définir `CFBundleDisplayName`.

## Packaging dans le workflow GitHub Actions

Construire l'application dans un pipeline CI/CD est simple en utilisant la commande `dotnet`. Pour que la signature du code et la notarisation fonctionnent, un peu de travail supplémentaire est nécessaire.

`codesign` et `notarytool` lisent le certificat et les identifiants pour communiquer avec le service de notarisation à partir d'un trousseau sur la machine de construction :

```bash
# Créer un nouveau trousseau
security create-keychain -p "${{ secrets.KEYCHAIN_PASSWORD}}" build.keychain
# Le définir comme le trousseau par défaut
security default-keychain -s build.keychain
# Déverrouiller le trousseau afin qu'il puisse être utilisé sans demande d'autorisation
security unlock-keychain -p "${{ secrets.KEYCHAIN_PASSWORD}}" build.keychain
```

`KEYCHAIN_PASSWORD` est un mot de passe que vous générez spécifiquement pour ce trousseau. Il peut être généré à chaque construction ou un mot de passe que vous utilisez pour chaque construction.

Ensuite, le certificat à signer doit être importé dans le trousseau. Comme les secrets GitHub ne prennent en charge que les chaînes, le fichier de certificat `.p12` doit être stocké sous forme encodée en base64. Dans le pipeline, la chaîne est décodée en un fichier et ajoutée au trousseau :

```bash
# Décoder le certificat en fichier
echo "${{ secrets.MACOS_CERTIFICATE }}" | base64 --decode > certificate.p12
# Importer dans le trousseau
security import certificate.p12 -k build.keychain -P "${{ secrets.MACOS_CERTIFICATE_PWD}}" -T /usr/bin/codesign
```

`MACOS_CERTIFICATE` est le fichier `.p12` encodé en base64, `MACOS_CERTIFICATE_PWD` est le mot de passe du fichier `.p12`.

Pour éviter les fenêtres de demande d'autorisation lors de la signature de code, demandez au trousseau de permettre l'accès à `codesign` :

```bash
# Autoriser codesign à accéder au trousseau
security set-key-partition-list -S apple-tool:,apple:,codesign: -s -k "${{ secrets.KEYCHAIN_PASSWORD}}" build.keychain
```

Comme Apple exige une authentification multi-facteurs (MFA) sur les comptes développeurs, `notarytool` utilise un mot de passe d'application dédié que vous pouvez générer sur le site des développeurs Apple. Nous allons ajouter le mot de passe d'application pour `notarytool` afin qu'il puisse être utilisé plus tard :

```bash
xcrun notarytool store-credentials "AC_PASSWORD" --apple-id "${{ secrets.APPLE_ID }}" --team-id ${{ env.TEAM_ID }} --password "${{ secrets.NOTARY_TOOL_PASSWORD }}"
```

`TEAM_ID` est l'identifiant de l'équipe dans App Store Connect, `APPLE_ID` est l'adresse e-mail de votre compte Apple, `NOTARY_TOOL_PASSWORD` est le mot de passe d'application que vous avez généré.

Pour utiliser ces étapes dans votre flux de travail GitHub Actions, ajoutez-les en tant qu'étape au travail qui construit votre application :

```yaml
jobs:
  build_osx:
    runs_on: macos-11
    env:
      TEAM_ID: MY_TEAM_ID
    steps:
    - name: Setup Keychain
      run: |
        security create-keychain -p "${{ secrets.KEYCHAIN_PASSWORD}}" build.keychain
        security default-keychain -s build.keychain
        security unlock-keychain -p "${{ secrets.KEYCHAIN_PASSWORD}}" build.keychain
        echo "${{ secrets.MACOS_CERTIFICATE }}" | base64 --decode > certificate.p12
        security import certificate.p12 -k build.keychain -P "${{ secrets.MACOS_CERTIFICATE_PWD}}" -T /usr/bin/codesign
        security set-key-partition-list -S apple-tool:,apple:,codesign: -s -k "${{ secrets.KEYCHAIN_PASSWORD}}" build.keychain
        xcrun notarytool store-credentials "AC_PASSWORD" --apple-id "${{ secrets.APPLE_ID }}" --team-id ${{ env.TEAM_ID }} --password "${{ secrets.NOTARY_TOOL_PASSWORD }}"
```


Lorsqu'il est configuré de cette manière, vous n'aurez pas à spécifier un fichier de trousseau spécifique pour que `codesign` ou `notarytool` l'utilisent.

Les étapes suivantes consistent à publier l'application et à la signer.
Commencez par ajouter cette variable d'environnement au travail :

```yaml
    env:
      SIGNING_IDENTITY: thumbprint_of_certificate_added_to_keychain
```

Et ensuite, ajoutez ces étapes :

```yaml
    - name: Publish app
      run: dotnet publish -c Release -r osx-x64 -o $RUNNER_TEMP/MyApp.app/Contents/MacOS MyApp.csproj
    - name: Codesign app
      run: |
        find "$RUNNER_TEMP/MyApp.app/Contents/MacOS/"|while read fname; do
          if [ -f "$fname" ]
          then
              echo "[INFO] Signing $fname"
              codesign --force --timestamp --options=runtime --entitlements MyApp.entitlements --sign "${{ env.$SIGNING_IDENTITY }}" "$fname"
          fi
        done
        codesign --force --timestamp --options=runtime --entitlements MyApp.entitlements --sign "${{ env.SIGNING_IDENTITY }}" "$RUNNER_TEMP/MyApp.app"
```

> **Note:** `RUNNER_TEMP` est une variable d'environnement fournie par GitHub Actions.

Après la signature du code, le bundle de l'application peut maintenant être notarié, en ajoutant cette étape au travail :

```yaml
    - name: Notarise app
      run: |
        ditto -c -k --sequesterRsrc --keepParent "$RUNNER_TEMP/MyApp.app" "$RUNNER_TEMP/MyApp.zip"
        xcrun notarytool submit "$RUNNER_TEMP/MyApp.zip" --wait --keychain-profile "AC_PASSWORD"
        xcrun stapler staple "$RUNNER_TEMP/MyApp.app"
```

Lorsque vous exécutez ce flux de travail, vous aurez un bundle d'application qui est signé et notarié, prêt à être empaqueté dans une image disque ou un installateur.

Pour vérifier que la signature du code a fonctionné, vous devrez d'abord le télécharger pour déclencher la fonctionnalité de quarantaine de macOS. Vous pouvez le faire en vous l'envoyant par e-mail ou en utilisant un service comme WeTransfer ou similaire.

Une fois que vous avez téléchargé le bundle de l'application et que vous souhaitez le démarrer, vous devriez voir la fenêtre contextuelle de macOS indiquant que l'application a été scannée et qu'aucun logiciel malveillant n'a été trouvé.
