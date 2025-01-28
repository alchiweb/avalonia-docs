# RenderTransforms and RenderTransformOrigin

import RenderTransformOriginWpfScreenshot from '/img/get-started/wpf/rendertransformorigin-wpf.png';
import RenderTransformOriginAvaloniaScreenshot from '/img/get-started/wpf/rendertransformorigin-avalonia.png';

Les RenderTransformOrigins sont différents dans WPF et Avalonia : Si vous appliquez un `RenderTransform`, gardez à l'esprit que la valeur par défaut pour le RenderTransformOrigin dans Avalonia est `RelativePoint.Center`. Dans WPF, la valeur par défaut est `RelativePoint.TopLeft` \(0, 0\). Dans des contrôles comme Viewbox, le même code entraînera un comportement de rendu différent :

**In WPF:**
<img src={RenderTransformOriginWpfScreenshot} alt="WPF" />

**In Avalonia:**
<img src={RenderTransformOriginAvaloniaScreenshot} alt="Avalonia" />

Dans AvaloniaUI, pour obtenir la même transformation d'échelle, nous devrions indiquer que le RenderTransformOrigin est la partie TopLeft du Visuel.

<XpfAd/>