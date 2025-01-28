# UIElement, FrameworkElement and Control

Le `UIElement` et le `FrameworkElement` de WPF sont des classes de base de contrôles non tempérés, qui s'équivalent à peu près à la classe `Control` d'Avalonia. La classe `Control` de WPF, en revanche, est un contrôle tempéré - l'équivalent d'Avalonia est `TemplatedControl`.

- Dans WPF/UWP, vous hériteriez de la classe `Control` pour créer un nouveau contrôle tempéré, mais dans Avalonia, vous devriez hériter de `TemplatedControl`.
- Dans WPF/UWP, vous hériteriez de la classe `FrameworkElement` pour créer un nouveau contrôle dessiné sur mesure, mais dans Avalonia, vous devriez hériter de `Control`.

Pour résumer :

* `UIElement` 🠞 `Control`
* `FrameworkElement`🠞 `Control`
* `Control` 🠞 `TemplatedControl`

<XpfAd/>