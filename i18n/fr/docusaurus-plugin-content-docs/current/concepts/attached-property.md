---
description: CONCEPTS
---

import AttachedControlDiagram from '/img/concepts/attached-control.png';
import AttachedLayoutPropertyDiagram from '/img/concepts/attached-layout-property.png';

# Propriétés attachées

Les contrôles _Avalonia UI_ prennent en charge le concept de **propriété attachée**. Il s'agit d'une propriété appliquée à un contrôle enfant qui fait référence à son contrôle conteneur.

En XAML, les propriétés attachées sont définies comme des attributs de l'élément de contrôle enfant en utilisant le format : `NomDeClasseConteneur.PropriétéAttachée="valeur"`

Voici quelques scénarios où une propriété attachée est utilisée :

## Contrôle attaché

Un contrôle supplémentaire est attaché à un 'contrôle hôte' pour un certain but. Cela peut être utilisé lorsque le contrôle ne permet généralement qu'un seul enfant dans sa zone de contenu. Dans ce scénario, le contrôle attaché n'est pas compté comme faisant partie du contenu, mais il sera utilisé d'une autre manière par le conteneur. Les exemples incluent : les menus contextuels, les info-bulles et les fenêtres contextuelles.

<img src={AttachedControlDiagram} alt=""/>

## Contrôle de mise en page

Les propriétés de mise en page attachées sont utilisées dans des scénarios où le contrôle conteneur doit savoir quelque chose sur les contrôles enfants qu'il va agencer. Les exemples incluent : les grilles, les panneaux d'amarrage et les panneaux relatifs.

<img src={AttachedLayoutPropertyDiagram} alt=""/>

:::info
Pour une liste complète des contrôles intégrés _Avalonia UI_, consultez la référence [ici](../reference/controls/).
:::