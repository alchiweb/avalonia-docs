# PropertyChangedCallback

Écouter les changements sur les DependencyProperties dans WPF peut être complexe. Lorsque vous enregistrez un `DependencyProperty`, vous pouvez fournir un `PropertyChangedCallback` statique, mais si vous souhaitez écouter les changements d'ailleurs, [les choses peuvent devenir compliquées et sujettes aux erreurs](https://stackoverflow.com/questions/23682232).

Dans Avalonia, il n'y a pas de `PropertyChangedCallback` au moment de l'enregistrement, à la place, un écouteur de classe est [ajouté au constructeur statique du contrôle de la même manière que les écouteurs d'événements de classe sont ajoutés](../../guides/data-binding/binding-from-code#subscribing-to-a-property-on-any-object).

<XpfAd/>