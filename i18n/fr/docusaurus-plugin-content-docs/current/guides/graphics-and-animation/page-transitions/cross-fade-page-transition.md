---
id: cross-fade-page-transition
title: Transition de Page en Fondu
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

# Transition de Page en Fondu

La transition de page en fondu fait disparaître la page actuelle et fait apparaître la nouvelle page en animant l'opacité.

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'C#', value: 'cs', },
  ]}
>
<TabItem value="xaml">

```xml
<CrossFade Duration="0:00:00.500" />
```

</TabItem>
<TabItem value="cs">

```cs
var transition = new CrossFade(TimeSpan.FromMilliseconds(500));
```
</TabItem>  

</Tabs>

## Plus d'Informations

:::info
Pour la documentation API complète concernant cette transition, voir [ici](http://reference.avaloniaui.net/api/Avalonia.Animation/PageSlide/).
:::

:::info
Consultez le code source sur _GitHub_ [`CrossFade.cs`](https://github.com/AvaloniaUI/Avalonia/blob/master/src/Avalonia.Base/Animation/CrossFade.cs)
:::
