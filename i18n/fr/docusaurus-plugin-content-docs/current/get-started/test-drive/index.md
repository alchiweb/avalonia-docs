# Exemple, pas à pas

import {DocsCardList} from '../../../../../../src/components/DocsCard';
import {useCurrentSidebarCategory} from '@docusaurus/theme-common';

<DocsCardList list={useCurrentSidebarCategory().items} />
