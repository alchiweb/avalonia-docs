---
id: debian-ubuntu
title: Package pour Debian / Ubuntu
---

Les programmes Avalonia Linux peuvent être exécutés dans la plupart des distributions Linux en double-cliquant sur l'exécutable ou en le lançant depuis le terminal. Néanmoins, pour une meilleure expérience utilisateur, il est recommandé d'avoir le programme installé, afin que l'utilisateur puisse le démarrer via un raccourci sur le bureau, qui existe dans des environnements de bureau tels que GNOME et KDE, ou via la ligne de commande, en ajoutant le programme au PATH.

Les distributions liées à Debian et Ubuntu ont leurs applications emballées dans des fichiers `.deb`, qui peuvent être installés via `sudo apt install ./your_package.deb`.

## Tutoriel

Dans ce tutoriel, nous utiliserons l'outil `dpkg-deb` pour compiler votre package `.deb`.

### 1) Organiser les fichiers du programme dans un dossier de préparation

Les paquets Debian suivent cette structure de base :

```sh
./staging_folder/
├── DEBIAN
│   └── control # fichier de contrôle du paquet
└── usr
    ├── bin
    │   └── myprogram # script de démarrage
    ├── lib
    │   └── myprogram
    │       ├── libHarfBuzzSharp.so # bibliothèque native Avalonia
    │       ├── libSkiaSharp.so # bibliothèque native Avalonia
    │       ├── other_native_library_1.so
    │       ├── myprogram_executable # exécutable principal
    │       ├── myprogram.dll 
    │       ├── my_other_dll.dll 
    │       ├── ... # tous les fichiers de dotnet publish
    └── share
        ├── applications
        │   └── MyProgram.desktop # fichier de raccourci sur le bureau
        ├── icons
        │   └── hicolor
        │       ├── ... # autres icônes de résolution (optionnel)
        └── pixmaps
            └── myprogram.png # icône de l'application principale
```

Signification de chaque dossier :

* `DEBIAN` : contient le fichier `control`.
* `/usr/bin/` : contient le script de démarrage (recommandé pour démarrer votre programme via la ligne de commande).
* `/usr/lib/myprogram/` : où tous les fichiers générés par `dotnet publish` vont.
* `/usr/share/applications/` : dossier pour le raccourci de bureau.
* `/usr/share/pixmaps/` et `/usr/share/icons/hicolor/**` : dossiers pour les icônes d'application.

:::info

Les `/usr/share/icons/hicolor/**` sont optionnels, car l'icône de votre application apparaîtra probablement sur le bureau même sans ces images, cependant, il est recommandé de les avoir pour une meilleure résolution.

:::

### 2) Créer le fichier `control`

Le fichier `control` va à l'intérieur du dossier `DEBIAN`.

Ce fichier décrit des aspects généraux de votre programme, tels que son nom, sa version, sa catégorie, ses dépendances, son mainteneur, son architecture de processeur et ses licences. [Les docs Debian](https://www.debian.org/doc/debian-policy/ch-controlfields.html) ont une description plus approfondie de tous les champs possibles dans le fichier.

:::tip[Commentaire de l'auteur]

Ne vous inquiétez pas trop de remplir tous les champs possibles, la plupart ne sont pas requis. Ce tutoriel vise à créer un paquet Debian "suffisamment bon".

:::

Les dépendances .NET peuvent être listées en exécutant `apt show dotnet-runtime-deps-8.0` (le suffixe change pour d'autres versions de .NET) ; elles apparaîtront sur la ligne commençant par *Depends: ...*. Vous pouvez également les vérifier dans le [dépôt .NET Core](https://github.com/dotnet/core/blob/main/release-notes/8.0/linux-packages.md).

Les dépendances requises pour Avalonia sont : `libx11-6, libice6, libsm6, libfontconfig1`.

Dans l'ensemble, toutes les dépendances .NET et Avalonia sont requises, ainsi que toutes les autres spécifiques à votre application.

Voici un exemple simple d'un fichier `control`.

```
Package: myprogram
Version: 3.1.0
Section: devel
Priority: optional
Architecture: amd64
Installed-Size: 68279
Depends: libx11-6, libice6, libsm6, libfontconfig1, ca-certificates, tzdata, libc6, libgcc1 | libgcc-s1, libgssapi-krb5-2, libstdc++6, zlib1g, libssl1.0.0 | libssl1.0.2 | libssl1.1 | libssl3, libicu | libicu74 | libicu72 | libicu71 | libicu70 | libicu69 | libicu68 | libicu67 | libicu66 | libicu65 | libicu63 | libicu60 | libicu57 | libicu55 | libicu52
Maintainer: Ken Lee <kenlee@outlook.com>
Homepage: https://github.com/kenlee/myprogram
Description: This is MyProgram, great for doing X.
Copyright: 2022-2024 Ken Lee <kenlee@outlook.com>
```

### 3) Créer le script de démarrage

Cette étape est recommandée pour deux raisons : d'abord, pour réduire la complexité du raccourci de bureau, et ensuite, pour rendre votre application exécutable depuis le Terminal.

Le nom du fichier de script de démarrage devrait de préférence être `myprogram` (sans l'extension `.sh`), de sorte que chaque fois que votre utilisateur tape "myprogram" dans le Terminal, il / elle lancera votre programme.

Le fichier **myprogram_executable** a généralement le même nom que son projet .NET, par exemple, si votre projet Avalonia .csproj est nommé *MyProgram.Desktop*, alors l'exécutable principal généré par dotnet publish sera `MyProgram.Desktop`.

Exemple de script de démarrage :

```sh
#!/bin/bash
# utilisez exec pour éviter que le script wrapper reste en tant que processus séparé
# "$@" pour passer les arguments de ligne de commande à l'application
exec /usr/lib/myprogram/myprogram_executable "$@"
```

### 4) Créer le raccourci sur le bureau

Le fichier de raccourci sur le bureau suit la [spécification freedesktop](https://specifications.freedesktop.org/desktop-entry-spec/desktop-entry-spec-latest.html#recognized-keys). Le Wiki Arch Linux a également [de bonnes informations connexes](https://wiki.archlinux.org/title/Desktop_entries).

Voici un exemple de fichier de raccourci sur le bureau.

```
[Desktop Entry]
Name=MyProgram
Comment=MyProgram, great for doing X
Icon=myprogram
Exec=myprogram
StartupWMClass=myprogram
Terminal=false
Type=Application
Categories=Development
GenericName=MyProgram
Keywords=keyword1; keyword2; keyword3
```

:::tip

Si votre application est censée ouvrir des fichiers, ajoutez **%F** à la fin de la ligne Exec, après `myprogram` ; si elle est censée ouvrir des URL, ajoutez **%U**.

:::

### 5) Ajouter des icônes hicolor (optionnel)

Les icônes hicolor suivent une structure de dossier comme ci-dessous.

[Ce billet de blog](https://martin.hoppenheit.info/blog/2016/where-to-put-application-icons-on-linux/) nous conseille de mettre des icônes à la fois dans les répertoires `hicolor` et `pixmaps`, selon [la documentation du système de menu Debian](https://www.debian.org/doc/packaging-manuals/menu.html/ch3.html#s3.7) et [la documentation FreeDesktop](https://specifications.freedesktop.org/icon-theme-spec/icon-theme-spec-0.11.html#install_icons).

```
├── icons
│   └── hicolor
│       ├── 128x128
│       │   └── apps
│       │       └── myprogram.png
│       ├── 16x16
│       │   └── apps
│       │       └── myprogram.png
│       ├── 256x256
│       │   └── apps
│       │       └── myprogram.png
│       ├── 32x32
│       │   └── apps
│       │       └── myprogram.png
│       ├── 48x48
│       │   └── apps
│       │       └── myprogram.png
│       ├── 512x512
│       │   └── apps
│       │       └── myprogram.png
│       ├── 64x64
│       │   └── apps
│       │       └── myprogram.png
│       └── scalable
│           └── apps
│               └── myprogram.svg
```

### 6) Compiler le paquet `.deb`

```sh
# pour les architectures x64, le suffixe suggéré est amd64.
dpkg-deb --root-owner-group --build ./staging_folder/ "./myprogram_${versionName}_amd64.deb"
```

## Exemple d'un script shell Linux pour l'ensemble du processus

```bash
#!/bin/bash

# Nettoyage
rm -rf ./out/
rm -rf ./staging_folder/

# Publication .NET
# self-contained est recommandé, afin que les utilisateurs finaux n'aient pas besoin d'installer .NET
dotnet publish "./src/MyProgram.Desktop/MyProgram.Desktop.csproj" \
  --verbosity quiet \
  --nologo \
  --configuration Release \
  --self-contained true \
  --runtime linux-x64 \
  --output "./out/linux-x64"

# Répertoire de staging
mkdir staging_folder

# Fichier de contrôle Debian
mkdir ./staging_folder/DEBIAN
cp ./src/MyProgram.Desktop.Debian/control ./staging_folder/DEBIAN

# Script de démarrage
mkdir ./staging_folder/usr
mkdir ./staging_folder/usr/bin
cp ./src/MyProgram.Desktop.Debian/myprogram.sh ./staging_folder/usr/bin/myprogram
chmod +x ./staging_folder/usr/bin/myprogram # définir les permissions d'exécution pour le script de démarrage

# Autres fichiers
mkdir ./staging_folder/usr/lib
mkdir ./staging_folder/usr/lib/myprogram
cp -f -a ./out/linux-x64/. ./staging_folder/usr/lib/myprogram/ # copie tous les fichiers du répertoire de publication
chmod -R a+rX ./staging_folder/usr/lib/myprogram/ # définir les permissions de lecture pour tous les fichiers
chmod +x ./staging_folder/usr/lib/myprogram/myprogram_executable # définir les permissions d'exécution pour l'exécutable principal

# Raccourci de bureau
mkdir ./staging_folder/usr/share
mkdir ./staging_folder/usr/share/applications
cp ./src/MyProgram.Desktop.Debian/MyProgram.desktop ./staging_folder/usr/share/applications/MyProgram.desktop

# Icône de bureau
# Un PNG de 1024px x 1024px, comme VS Code utilise pour son icône
mkdir ./staging_folder/usr/share/pixmaps
cp ./src/MyProgram.Desktop.Debian/myprogram_icon_1024px.png ./staging_folder/usr/share/pixmaps/myprogram.png

# Icônes hicolor
mkdir ./staging_folder/usr/share/icons
mkdir ./staging_folder/usr/share/icons/hicolor
mkdir ./staging_folder/usr/share/icons/hicolor/scalable
mkdir ./staging_folder/usr/share/icons/hicolor/scalable/apps
cp ./misc/myprogram_logo.svg ./staging_folder/usr/share/icons/hicolor/scalable/apps/myprogram.svg

# Créer un fichier .deb
dpkg-deb --root-owner-group --build ./staging_folder/ ./myprogram_3.1.0_amd64.deb
```

## Pour installer

```sh
sudo apt install ./myprogram_3.1.0_amd64.deb
```

## Pour désinstaller / supprimer

```sh
sudo apt remove myprogram
```

## Outils de packaging tiers pour Debian / Ubuntu

* https://github.com/quamotion/dotnet-packaging
* https://github.com/SuperJMN/DotnetPackaging
* https://github.com/kuiperzone/PupNet-Deploy