---
id: selectors
title: Sélecteurs de style
---

# Sélecteurs de style

_Avalonia UI_ utilise des sélecteurs de style pour faire correspondre les contrôles en utilisant une syntaxe XAML personnalisée.

:::info
Si vous êtes familier avec la technologie CSS (Cascading Style Sheets), vous reconnaîtrez cette syntaxe comme étant très similaire.
:::

Voici une liste de quelques exemples de sélecteurs de style :

<table><thead><tr><th width="310">Sélecteur de style</th><th>Description</th></tr></thead><tbody><tr><td><code>Button</code></td><td>Sélectionne tous les contrôles de classe <code>Button</code>.</td></tr><tr><td><code>Button.red</code></td><td>Sélectionne tous les contrôles <code>Button</code> avec une classe de style <code>red</code> définie.</td></tr><tr><td><code>Button.red.large</code></td><td>Sélectionne tous les contrôles <code>Button</code> avec les classes de style <code>red</code> et <code>large</code> définies.</td></tr><tr><td><code>Button:focus</code></td><td>Sélectionne tous les contrôles <code>Button</code> avec la pseudo-classe <code>:focus</code> active.</td></tr><tr><td><code>Button.red:focus</code></td><td>Sélectionne tous les contrôles <code>Button</code> avec la classe de style <code>red</code> et la pseudo-classe <code>:focus</code> active.</td></tr><tr><td><code>Button#myButton</code></td><td>Sélectionne un contrôle <code>Button</code> avec l'attribut <code>Name</code> défini comme <code>"myButton"</code>.</td></tr><tr><td><code>StackPanel Button.xl</code></td><td>Sélectionne tous les contrôles de type  <code>Button</code> avec la classe <code>xl</code>définie; qui sont également des descendants à n'importe quel niveau d'un contrôle de type <code>StackPanel</code>.</td></tr><tr><td><code>StackPanel > Button.xl</code></td><td>Sélectionne tous les contrôles de type <code>Button</code> avec une classee <code>xl</code> définie; qui sont également des descendants directe d'un contrôle de type  <code>StackPanel</code>.</td></tr><tr><td><code>Button /template/ ContentPresenter</code></td><td>Sélectionne tous les contrôles de type <code>ContentPresenter</code> à l'intérieur d'un modèle pour un contôle de type <code>Button</code>.</td></tr></tbody></table>

Pour une description complète de ces formats de sélecteurs de style, et plus encore, consultez la référence [ici](../../reference/styles/style-selector-syntax).