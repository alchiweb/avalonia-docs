---
id: index
title: Services
---

Cette section vous présentera les services inclus dans une application.

import {DocsCardList} from '../../../../../../src/components/DocsCard';
import {useCurrentSidebarCategory} from '@docusaurus/theme-common';

<DocsCardList list={useCurrentSidebarCategory().items} />