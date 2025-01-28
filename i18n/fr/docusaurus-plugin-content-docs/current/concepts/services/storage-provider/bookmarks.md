---
id: bookmarks
title: Signets
---

# Signets

Les signets sont particulièrement importants pour maintenir l'accès aux fichiers et dossiers dans les systèmes d'exploitation modernes qui ont des contrôles de sécurité et de confidentialité stricts. Par exemple, sur des plateformes comme iOS et les versions plus récentes de macOS, l'accès direct au système de fichiers est fortement restreint. Au lieu de cela, les applications demandent à l'utilisateur de sélectionner un fichier ou un dossier via un sélecteur de fichiers fourni par le système, et le système d'exploitation donne ensuite à l'application un signet sécurisé qu'elle peut utiliser pour accéder à ce fichier ou dossier à l'avenir.

Dans les `StorageProvider` d'Avalonia, ces signets sont représentés par es interfaces `IStorageBookmarkFile` et `IStorageBookmarkFolder`.

Pour obtenir l'ID de signet d'un dossier ou fichier spécifique, veuillez utiliser la méthode asynchrone `SaveBookmarkAsync` sur l'élément de stockage.
Après avoir récupéré l'ID de signet, il peut être enregistré dans une base de données locale pour une utilisation ultérieure au lieu de demander à l'utilisateur de choisir un dossier à chaque fois.
ous pouvez utiliser les méthodes `OpenFileBookmarkAsync` et `OpenFolderBookmarkAsync` pour ouvrir un fichier ou un dossier marqué avec son ID de signet. Cela renverra le fichier ou le dossier marqué, ou null si le système d'exploitation refuse la demande.

:::note
Le comportement et les capacités exacts peuvent dépendre du système d'exploitation spécifique et de ses politiques de sécurité. Par exemple, sur certaines plateformes, un signet peut devenir invalide si l'utilisateur déplace ou renomme le fichier ou le dossier auquel il pointe.
:::

:::note
Il n'est pas recommandé de stocker les ID de signet dans une base de données distante, car les signets peuvent ne pas être persistants et peuvent contenir des informations sensibles sur le chemin des fichiers.
:::