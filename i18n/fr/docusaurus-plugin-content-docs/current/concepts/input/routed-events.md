---
description: CONCEPTS - Input
---

import InputEventRoutingDiagram from '/img/concepts/input/pointer-pressed-routing.png';

# Événements Routés

La plupart des événements dans Avalonia sont implémentés en tant qu'Événements Routés. Les événements routés sont des événements qui se produisent sur l'ensemble de l'arbre plutôt que seulement sur le contrôle qui a déclenché l'événement.

## Qu'est-ce qu'un Événement Routé

Une application typique d'Avalonia contient de nombreux éléments. Qu'ils soient créés dans le code ou déclarés en XAML, ces éléments existent dans une relation d'arbre d'éléments les uns par rapport aux autres. Le parcours de l'événement peut se déplacer dans l'une des deux directions en fonction de la définition de l'événement, mais en général, le parcours va de l'élément source et "bulle" ensuite vers le haut à travers l'arbre d'éléments jusqu'à atteindre la racine de l'arbre d'éléments (typiquement une page ou une fenêtre). Ce concept de bulle peut vous sembler familier si vous avez déjà travaillé avec le DOM HTML.

### Scénarios de Haut Niveau pour les Événements Routés

Voici un bref résumé des scénarios qui ont motivé le concept d'événement routé, et pourquoi un événement CLR typique n'était pas adéquat pour ces scénarios :

**Composition et encapsulation des contrôles :** Divers contrôles dans Avalonia ont un modèle de contenu riche. Par exemple, vous pouvez placer une image à l'intérieur d'un `Button`, ce qui étend effectivement l'arbre visuel du bouton. Cependant, l'image ajoutée ne doit pas perturber le comportement de test de frappe qui fait qu'un bouton réagit à un `Click` de son contenu, même si l'utilisateur clique sur des pixels qui font techniquement partie de l'image.

**Points d'attachement de gestionnaire singulier :** Dans Windows Forms, vous deviez attacher le même gestionnaire plusieurs fois pour traiter des événements pouvant être déclenchés par plusieurs éléments. Les événements routés vous permettent d'attacher ce gestionnaire une seule fois, comme cela a été montré dans l'exemple précédent, et d'utiliser la logique du gestionnaire pour déterminer d'où l'événement provient si nécessaire. Par exemple, cela pourrait être le gestionnaire pour le XAML montré précédemment :

```csharp
private void CommonClickHandler(object sender, RoutedEventArgs e)
{
  var source = e.Source as Control;
  switch (source.Name)
  {
    case "YesButton":
      // faire quelque chose ici ...
      break;
    case "NoButton":
      // faire quelque chose ...
      break;
    case "CancelButton":
      // faire quelque chose ...
      break;
  }
  e.Handled=true;
}
```

**Gestion de classe :** Les événements routés permettent un gestionnaire statique défini par la classe. Ce gestionnaire de classe a l'opportunité de traiter un événement avant que les gestionnaires d'instance attachés ne puissent le faire.

**Référencer un événement sans réflexion :** Certaines techniques de code et de balisage nécessitent un moyen d'identifier un événement spécifique. Un événement routé crée un champ `RoutedEvent` en tant qu'identifiant, ce qui fournit une technique d'identification d'événements robuste ne nécessitant pas de réflexion statique ou d'exécution.

### Comment les événements routés sont implémentés

Un événement routé est un événement CLR qui est soutenu par une instance de la classe `RoutedEvent` et enregistré auprès du système d'événements Avalonia. L'instance `RoutedEvent` obtenue lors de l'enregistrement est généralement conservée en tant que membre de champ `public` `static` `readonly` de la classe qui s'enregistre et "possède" ainsi l'événement routé. La connexion à l'événement CLR portant le même nom (qui est parfois appelé l'événement "wrapper") est réalisée en remplaçant les implémentations `add` et `remove` pour l'événement CLR. Ordinairement, les `add` et `remove` sont laissés comme une valeur par défaut implicite qui utilise la syntaxe d'événement spécifique à la langue appropriée pour ajouter et supprimer des gestionnaires de cet événement. Le mécanisme de soutien et de connexion de l'événement routé est conceptuellement similaire à la façon dont une propriété Avalonia est une propriété CLR qui est soutenue par la classe `AvaloniaProperty` et enregistrée auprès du système de propriétés Avalonia.

L'exemple suivant montre la déclaration d'un événement routé `Tap` personnalisé, y compris l'enregistrement et l'exposition du champ d'identifiant `RoutedEvent` et les implémentations `add` et `remove` pour l'événement CLR `Tap`.

```csharp
public class SampleControl: Control
{
  public static readonly RoutedEvent<RoutedEventArgs> TapEvent =
    RoutedEvent.Register<SampleControl, RoutedEventArgs>(nameof(Tap), RoutingStrategies.Bubble);

  // Provide CLR accessors for the event
  public event EventHandler<RoutedEventArgs> Tap
  { 
    add => AddHandler(TapEvent, value);
    remove => RemoveHandler(TapEvent, value);
  }
}
```

### Gestionnaires d'événements routés et XAML

Pour ajouter un gestionnaire pour un événement en utilisant XAML, vous déclarez le nom de l'événement en tant qu'attribut sur l'élément qui est un auditeur d'événements. La valeur de l'attribut est le nom de votre méthode de gestionnaire implémentée, qui doit exister dans la classe du fichier de code-behind.

```xml
<Button Click="b1SetColor">bouton</Button>
```

La syntaxe XAML pour ajouter des gestionnaires d'événements CLR standard est la même que pour ajouter des gestionnaires d'événements routés, car vous ajoutez réellement des gestionnaires à l'événement wrapper CLR, qui a une implémentation d'événement routé en dessous.

## Stratégies de routage

Les événements routés utilisent l'une des trois stratégies de routage :

* **Bubbling :** Les gestionnaires d'événements sur la source de l'événement sont invoqués. L'événement routé se propage ensuite aux éléments parents successifs jusqu'à atteindre la racine de l'arbre des éléments. La plupart des événements routés utilisent la stratégie de routage par bulle. Les événements routés par bulle sont généralement utilisés pour signaler des changements d'entrée ou d'état provenant de contrôles distincts ou d'autres éléments d'interface utilisateur.
* **Direct :** Seul l'élément source lui-même a l'opportunité d'invoquer des gestionnaires en réponse. Cela est analogue au "routage" que Windows Forms utilise pour les événements. Cependant, contrairement à un événement CLR standard, les événements routés directs prennent en charge la gestion de classe (la gestion de classe est expliquée dans une section à venir).
* **Tunneling :** Au départ, les gestionnaires d'événements à la racine de l'arbre des éléments sont invoqués. L'événement routé parcourt ensuite un chemin à travers les éléments enfants successifs le long de la route, vers l'élément nœud qui est la source de l'événement routé (l'élément qui a déclenché l'événement routé). Les événements routés par tunnel sont souvent utilisés ou gérés dans le cadre du composite d'un contrôle, de sorte que les événements des parties composites peuvent être délibérément supprimés ou remplacés par des événements spécifiques au contrôle complet. Les événements d'entrée fournis dans Avalonia déclenchent souvent à la fois des événements de tunnel et des événements de bulle.

## Pourquoi utiliser des événements routés ?

En tant que développeur d'applications, vous n'avez pas toujours besoin de savoir ou de vous soucier que l'événement que vous traitez est implémenté comme un événement routé. Les événements routés ont un comportement spécial, mais ce comportement est largement invisible si vous traitez un événement sur l'élément où il est déclenché.

Là où les événements routés deviennent puissants, c'est si vous utilisez l'un des scénarios suggérés : définir des gestionnaires communs à une racine commune, composer votre propre contrôle ou définir votre propre classe de contrôle personnalisé.

Les écouteurs d'événements routés et les sources d'événements routés n'ont pas besoin de partager un événement commun dans leur hiérarchie. Tout contrôle peut être un écouteur d'événements pour n'importe quel événement routé. Par conséquent, vous pouvez utiliser l'ensemble complet des événements routés disponibles dans l'ensemble API de travail comme une "interface" conceptuelle par laquelle des éléments disparates de l'application peuvent échanger des informations d'événements. Ce concept d'"interface" pour les événements routés est particulièrement applicable aux événements d'entrée.

Les événements routés peuvent également être utilisés pour communiquer à travers l'arbre des éléments, car les données de l'événement sont perpétuées à chaque élément sur le chemin. Un élément pourrait modifier quelque chose dans les données de l'événement, et ce changement serait disponible pour le prochain élément sur le chemin.

En dehors de l'aspect routage, il y a deux autres raisons pour lesquelles un événement Avalonia donné pourrait être implémenté comme un événement routé au lieu d'un événement CLR standard. Si vous implémentez vos propres événements, vous pourriez également considérer ces principes :

* Certaines fonctionnalités de style et de modèle nécessitent que l'événement référencé soit un événement routé. C'est le scénario d'identification d'événement mentionné précédemment.
* Les événements routés prennent en charge un mécanisme de gestion de classe permettant à la classe de spécifier des méthodes statiques qui ont la possibilité de gérer les événements routés avant que les gestionnaires d'instance enregistrés ne puissent y accéder. Cela est très utile dans la conception de contrôles, car votre classe peut imposer des comportements de classe pilotés par des événements qui ne peuvent pas être accidentellement supprimés en gérant un événement sur une instance.

Chacune des considérations ci-dessus est discutée dans une section distincte de ce sujet.

## Ajouter et implémenter un gestionnaire d'événements pour un événement routé

Pour ajouter un gestionnaire d'événements en XAML, vous ajoutez simplement le nom de l'événement à un élément en tant qu'attribut et définissez la valeur de l'attribut comme le nom du gestionnaire d'événements qui implémente un délégué approprié, comme dans l'exemple suivant.

```xml
<Button Click="b1SetColor">bouton</Button>
```

`b1SetColor` est le nom du gestionnaire implémenté qui contient le code qui gère l'événement `Click`. `b1SetColor` doit avoir la même signature que le délégué `RoutedEventHandler<RoutedEventArgs>`, qui est le délégué de gestionnaire d'événements pour l'événement `Click`. Le premier paramètre de tous les délégués de gestionnaires d'événements routés spécifie l'élément auquel le gestionnaire d'événements est ajouté, et le deuxième paramètre spécifie les données de l'événement.

```csharp
void b1SetColor(object sender, RoutedEventArgs args)
{
  // logique pour gérer l'événement Click
}
```

`RoutedEventHandler<RoutedEventArgs>` est le délégué de gestionnaire d'événements routés de base. Pour les événements routés qui sont spécialisés pour certains contrôles ou scénarios, les délégués à utiliser pour les gestionnaires d'événements routés peuvent également devenir plus spécialisés, afin qu'ils puissent transmettre des données d'événements spécialisées. Par exemple, dans un scénario d'entrée commun, vous pourriez gérer un événement routé `PointerPressed`. Votre gestionnaire devrait implémenter le délégué `RoutedEventHandler<PointerPressedEventArgs>`. En utilisant le délégué le plus spécifique, vous pouvez traiter les `PointerPressedEventArgs` dans le gestionnaire et lire la propriété `PointerEventArgs.Pointer`, qui contient des informations sur le pointeur qui a causé la pression.

Ajouter un gestionnaire pour un événement routé dans une application créée par code est simple. Les gestionnaires d'événements routés peuvent toujours être ajoutés par une méthode d'aide `AddHandler` (qui est la même méthode que les appels de support existants pour `add`). Cependant, les événements routés Avalonia existants ont généralement des implémentations de support de logique `add` et `remove` qui permettent aux gestionnaires d'événements routés d'être ajoutés par une syntaxe d'événements spécifique au langage, qui est une syntaxe plus intuitive que la méthode d'aide. L'exemple suivant montre l'utilisation de la méthode d'aide :

```csharp
void MakeButton()
{
  Button b2 = new Button();
  b2.AddHandler(Button.ClickEvent, Onb2Click);
}

void Onb2Click(object sender, RoutedEventArgs e)
{
  // logique pour gérer l'événement Click
}
```

L'exemple suivant montre la syntaxe de l'opérateur C# :

```csharp
void MakeButton2()
{
  Button b2 = new Button();
  b2.Click += Onb2Click2;
}

void Onb2Click2(object sender, RoutedEventArgs e)
{
  // logique pour gérer l'événement Click
}
```

**Le Concept de Handled**

Tous les événements routés partagent une classe de base de données d'événements commune, `RoutedEventArgs`. `RoutedEventArgs` définit la propriété `Handled`, qui prend une valeur booléenne. Le but de la propriété `Handled` est de permettre à tout gestionnaire d'événements le long de la route de marquer l'événement routé comme _traité_, en définissant la valeur de `Handled` sur `true`. Après avoir été traité par le gestionnaire à un élément le long de la route, les données d'événements partagées sont à nouveau rapportées à chaque auditeur le long de la route.

La valeur de `Handled` affecte la manière dont un événement routé est signalé ou traité alors qu'il progresse le long de la route. Si `Handled` est `true` dans les données d'événements pour un événement routé, alors les gestionnaires qui écoutent cet événement routé sur d'autres éléments ne sont généralement plus invoqués pour cette instance d'événement particulière. Cela est vrai tant pour les gestionnaires attachés en XAML que pour les gestionnaires ajoutés par des syntaxes d'attachement de gestionnaires d'événements spécifiques à un langage comme `+=`. Pour la plupart des scénarios de gestionnaires courants, marquer un événement comme traité en définissant `Handled` sur `true` "arrêtera" le routage pour soit une route de tunneling soit une route de bubbling, et également pour tout événement qui est traité à un point de la route par un gestionnaire de classe.

Cependant, il existe un mécanisme "handledEventsToo" par lequel les auditeurs peuvent toujours exécuter des gestionnaires en réponse à des événements routés où `Handled` est `true` dans les données d'événements. En d'autres termes, la route de l'événement n'est pas réellement arrêtée en marquant les données de l'événement comme traitées. Vous ne pouvez utiliser le mécanisme handledEventsToo que dans le code :

* Dans le code, au lieu d'utiliser une syntaxe d'événement spécifique à un langage qui fonctionne pour les événements CLR généraux, appelez la méthode Avalonia `AddHandler<TEventArgs>(RoutedEvent<TEventArgs>, EventHandler<TEventArgs> handler, RoutingStrategies, bool)` pour ajouter votre gestionnaire. Spécifiez la valeur de `handledEventsToo` comme `true`.

En plus du comportement que l'état `Handled` produit dans les événements routés, le concept de `Handled` a des implications sur la manière dont vous devriez concevoir votre application et écrire le code du gestionnaire d'événements. Vous pouvez conceptualiser `Handled` comme étant un protocole simple exposé par les événements routés. Exactement comment vous utilisez ce protocole dépend de vous, mais le design conceptuel de la manière dont la valeur de `Handled` est censée être utilisée est le suivant :

* Si un événement routé est marqué comme géré, alors il n'a pas besoin d'être géré à nouveau par d'autres éléments le long de cette route.
* Si un événement routé n'est pas marqué comme géré, alors d'autres auditeurs qui étaient plus tôt le long de la route ont choisi soit de ne pas enregistrer de gestionnaire, soit les gestionnaires qui ont été enregistrés ont choisi de ne pas manipuler les données de l'événement et de définir `Handled` sur `true`. (Ou, il est bien sûr possible que l'auditeur actuel soit le premier point de la route.) Les gestionnaires sur l'auditeur actuel ont maintenant trois possibilités d'action :
  * Ne rien faire du tout ; l'événement reste non géré, et l'événement se dirige vers l'auditeur suivant.  * Execute code in response to the event, but make the determination that the action taken was not substantial enough to warrant marking the event as handled. The event routes to the next listener.
  * Exécutez le code en réponse à l'événement. Marquez l'événement comme traité dans les données de l'événement passées au gestionnaire, car l'action entreprise a été jugée suffisamment substantielle pour justifier cette marque. L'événement est toujours routé vers le prochain écouteur, mais avec `Handled=true` dans ses données d'événement, de sorte que seuls les écouteurs `handledEventsToo` ont la possibilité d'invoquer d'autres gestionnaires.

Ce design conceptuel est renforcé par le comportement de routage mentionné précédemment : il est plus difficile (bien que toujours possible dans le code ou les styles) d'attacher des gestionnaires pour des événements routés qui sont invoqués même si un gestionnaire précédent le long de la route a déjà défini `Handled` sur `true`.

Dans les applications, il est assez courant de simplement gérer un événement routé en bulles sur l'objet qui l'a déclenché, sans se soucier des caractéristiques de routage de l'événement. Cependant, il est toujours bon de marquer l'événement routé comme traité dans les données de l'événement, pour éviter des effets secondaires inattendus au cas où un élément plus haut dans l'arbre des éléments aurait également un gestionnaire attaché pour ce même événement routé.

## Gestionnaires de classe

Si vous définissez une classe qui dérive d'une certaine manière de `AvaloniaObject`, vous pouvez également définir et attacher un gestionnaire de classe pour un événement routé qui est un membre d'événement déclaré ou hérité de votre classe. Les gestionnaires de classe sont invoqués avant tous les gestionnaires d'écouteurs d'instance qui sont attachés à une instance de cette classe, chaque fois qu'un événement routé atteint une instance d'élément dans sa route.

Certains contrôles Avalonia ont une gestion de classe inhérente pour certains événements routés. Cela peut donner l'apparence extérieure que l'événement routé n'est jamais déclenché, mais en réalité, il est géré par la classe, et l'événement routé peut potentiellement encore être géré par vos gestionnaires d'instance si vous utilisez certaines techniques. De plus, de nombreuses classes de base et contrôles exposent des méthodes virtuelles qui peuvent être utilisées pour remplacer le comportement de gestion de classe.

Pour attacher un gestionnaire de classe dans l'un de vos propres contrôles, utilisez la méthode `AddClassHandler` à partir d'un constructeur statique :

```csharp
static MyControl()
{
    MyEvent.AddClassHandler<MyControl>((x, e) => x.OnMyEvent(e));
}

protected virtual void OnMyEvent(MyEventArgs e)
{
    // Le Handle de l'événement est ici.
}
```

## Événements attachés dans Avalonia

Le langage XAML définit également un type spécial d'événement appelé un _événement attaché_. Un événement attaché vous permet d'ajouter un gestionnaire pour un événement particulier à un élément arbitraire. L'élément gérant l'événement n'a pas besoin de définir ou d'hériter de l'événement attaché, et ni l'objet potentiellement déclenchant l'événement ni l'instance de destination gérant l'événement ne doivent définir ou "posséder" cet événement en tant que membre de classe.

Le système d'entrée Avalonia utilise largement les événements attachés. Cependant, presque tous ces événements attachés sont transmis par les éléments de base. Les événements d'entrée apparaissent alors comme des événements routés non attachés équivalents qui sont membres de la classe d'élément de base. Par exemple, l'événement attaché sous-jacent `Gestures.Tapped` peut être plus facilement géré sur un `Control` donné en utilisant `Tapped` sur ce contrôle plutôt qu'en traitant la syntaxe d'événements attachés soit dans XAML soit dans le code.

## Noms d'événements qualifiés en XAML

Une autre utilisation de la syntaxe qui ressemble à la syntaxe d'événements attachés _typename_._eventname_ mais qui n'est pas strictement parlant une utilisation d'événements attachés est lorsque vous attachez des gestionnaires pour des événements routés qui sont déclenchés par des éléments enfants. Vous attachez les gestionnaires à un parent commun, pour tirer parti du routage des événements, même si le parent commun n'a pas l'événement routé pertinent en tant que membre. Considérez cet exemple à nouveau :

```xml
<Border Height="50" Width="300">
  <StackPanel Orientation="Horizontal" Button.Click="CommonClickHandler">
    <Button Name="YesButton">Oui</Button>
    <Button Name="NoButton">Non</Button>
    <Button Name="CancelButton">Annuler</Button>
  </StackPanel>
</Border>
```

Ici, l'élément parent écouteur où le gestionnaire est ajouté est un `StackPanel`. Cependant, il ajoute un gestionnaire pour un événement routé qui a été déclaré et sera déclenché par la classe `Button`. `Button` "possède" l'événement, mais le système d'événements routés permet d'attacher des gestionnaires pour tout événement routé à tout écouteur d'instance de contrôle qui pourrait autrement attacher des écouteurs pour un événement de runtime de langage commun (CLR). L'espace de noms xmlns par défaut pour ces noms d'attribut d'événements qualifiés est généralement l'espace de noms xmlns par défaut d'Avalonia, mais vous pouvez également spécifier des espaces de noms préfixés pour des événements routés personnalisés.

## Événements d'entrée

Une application fréquente des événements routés dans la plateforme Avalonia est pour les événements d'entrée. Les événements d'entrée viennent souvent par paires, l'un étant l'événement de propagation et l'autre l'événement de tunnel. Parfois, les événements d'entrée n'ont qu'une version de propagation, ou peut-être seulement une version routée directe.

Les événements d'entrée d'Avalonia qui viennent par paires sont implémentés de sorte qu'une seule action utilisateur provenant de l'entrée, comme une pression sur un bouton de souris, déclenche les deux événements routés de la paire en séquence. D'abord, l'événement de tunnel est déclenché et parcourt son chemin. Ensuite, l'événement de propagation est déclenché et parcourt son chemin.
Les deux événements partagent littéralement la même instance de données d'événement, car l'appel de méthode `RaiseEvent` dans la classe d'implémentation qui déclenche l'événement de propagation écoute les données de l'événement de tunnel et les réutilise dans le nouvel événement déclenché. Les auditeurs avec des gestionnaires pour l'événement de tunnel ont la première opportunité de marquer l'événement routé comme traité (d'abord les gestionnaires de classe, puis les gestionnaires d'instance). Si un élément le long de la route de tunnel marque l'événement routé comme traité, les données d'événement déjà traitées sont envoyées pour l'événement de propagation, et les gestionnaires typiques attachés aux événements d'entrée de propagation équivalents ne seront pas invoqués. À première vue, il semblera que l'événement de propagation traité n'a même pas été déclenché. Ce comportement de gestion est utile pour la composition de contrôles, où vous pourriez vouloir que tous les événements d'entrée basés sur le test de frappe ou les événements d'entrée basés sur le focus soient rapportés par votre contrôle final, plutôt que par ses parties composites. L'élément de contrôle final est plus proche de la racine dans la composition, et a donc l'opportunité de gérer la classe de l'événement de tunnel en premier et peut-être de "remplacer" cet événement routé par un événement plus spécifique au contrôle, dans le cadre du code qui soutient la classe de contrôle.

Pour illustrer comment fonctionne le traitement des événements d'entrée, considérons l'exemple suivant d'événement d'entrée. Dans l'illustration d'arbre suivante, `élément feuille #2` est la source d'un événement `PointerPressed` : 

<img src={InputEventRoutingDiagram} alt="Diagramme de routage d'événements"/>

L'ordre de traitement des événements est le suivant :

1. `PointerPressed` (tunnel) sur l'élément racine.
2. `PointerPressed` (tunnel) sur l'élément intermédiaire #1.
3. `PointerPressed` (tunnel) sur l'élément source #2.
4. `PointerPressed` (bubbling) sur l'élément source #2.
5. `PointerPressed` (bubbling) sur l'élément intermédiaire #1.
6. `PointerPressed` (bubbling) sur l'élément racine.

Un délégué de gestionnaire d'événements routés fournit des références à deux objets : l'objet qui a déclenché l'événement et l'objet où le gestionnaire a été invoqué. L'objet où le gestionnaire a été invoqué est l'objet rapporté par le paramètre `sender`. L'objet où l'événement a été d'abord déclenché est rapporté par la propriété `Source` dans les données de l'événement. Un événement routé peut toujours être déclenché et géré par le même objet, auquel cas `sender` et `Source` sont identiques (c'est le cas des Étapes 3 et 4 dans la liste d'exemples de traitement des événements).

En raison du tunneling et du bubbling, les éléments parents reçoivent des événements d'entrée dont la `Source` est l'un de leurs éléments enfants. Lorsqu'il est important de savoir quel est l'élément source, vous pouvez identifier l'élément source en accédant à la propriété `Source`.

En général, une fois que l'événement d'entrée est marqué `Handled`, d'autres gestionnaires ne sont pas invoqués. En règle générale, vous devez marquer les événements d'entrée comme traités dès qu'un gestionnaire est invoqué qui traite la gestion logique spécifique à votre application du sens de l'événement d'entrée.

L'exception à cette déclaration générale concernant l'état `Handled` est que les gestionnaires d'événements d'entrée qui sont enregistrés pour ignorer délibérément l'état `Handled` des données d'événements seraient toujours invoqués par l'un ou l'autre chemin.

Certaines classes choisissent de gérer certaines événements d'entrée, généralement dans l'intention de redéfinir ce qu'un événement d'entrée déclenché par l'utilisateur signifie dans ce contrôle et de lever un nouvel événement.
