---
description: CONCEPTS
---

import DataBindingModeDiagram from '/img/basics/data-binding/data-binding-syntax/data-binding-mode.png';
import TargetNullValueScreenshot from '/img/basics/data-binding/data-binding-syntax/targetnullvalue.gif';
import UpdateSourceTriggerScreenshot from '/img/basics/data-binding/data-binding-syntax/updatesourcetrigger.gif';

# Syntaxe de Liaison de Données

Avalonia prend en charge la création de liaisons de données en XAML et dans le code. Les liaisons de données en XAML sont généralement créées avec l'extension de balisage `Binding` décrite dans ce document. Pour créer des liaisons de données dans le code, voir [ici](../../../guides/data-binding/binding-from-code.md).

## Extension de Balisage de Liaison de Données

L'extension de balisage `Binding` utilise le mot-clé `Binding` en combinaison avec des paramètres optionnels pour définir la source de données et d'autres options comme le montre l'exemple suivant :

```xml
<SomeControl SomeProperty="{Binding Path, Mode=ModeValue, StringFormat=Pattern}" />
```

| Paramètre             | Description                                                                                    |
|-----------------------|------------------------------------------------------------------------------------------------|
| `Path`                | Le nom de la propriété source à lier.                                                          |
| `Mode`                | La direction de synchronisation de la liaison.                                                 |
| `Priority`            | Priorité du setter de propriété.                                                               |
| `Source`              | L'objet qui contient la propriété spécifiée par `Path`.                                        |
| `ElementName`         | Utilise un `Control` nommé comme `Source`.                                                     |
| `RelativeSource`      | Utilise un `Control` relatif au sein de la hiérarchie de l'Arbre Visuel comme `Source`.        |
| `StringFormat`        | Un modèle pour formater la valeur de la propriété en tant que chaîne.                          |
| `Converter`           | Un `IValueConverter` qui convertit la valeur source en valeur cible et vice versa.             |
| `ConverterParameter`  | Un paramètre à fournir au `Converter`.                                                         |
| `FallbackValue`       | Définit une valeur lorsque le binding ne peut pas être créé ou ne peut pas produire de valeur. |
| `TargetNullValue`     | Définit une valeur lorsque la propriété source contient une valeur nulle.                      |
| `UpdateSourceTrigger` | Déclenche une mise à jour de la propriété source lorsqu'une condition prédéfinie se produit.   |

Ces paramètres doivent être connus et définis au moment de la création du binding. Ce sont des propriétés CLR qui ne peuvent 
pas être définies et mises à jour par des bindings supplémentaires.

## Chemin de Liaison de Données

Le premier paramètre spécifié est généralement le `Path`. C'est le nom d'une propriété dans le `Source` (`DataContext` par défaut) 
que Avalonia localise lors de la création du binding.

Vous pouvez omettre `Path=` lorsqu'il s'agit du premier paramètre. Les deux bindings suivants sont équivalents :

```xml
<TextBlock Text="{Binding Name}"/>
<TextBlock Text="{Binding Path=Name}"/>
```

Le chemin de binding peut être une seule propriété ou une chaîne de sous-propriétés. Par exemple, si la source de données a 
une propriété `Student` et que l'objet retourné par cette propriété a une propriété `Name`, alors vous pouvez lier le nom de l'étudiant en utilisant une syntaxe comme celle-ci :

```xml
<TextBlock Text="{Binding Student.Name}"/>
```

Si la source de données peut être indexée (comme un tableau ou une liste), vous pouvez ajouter l'index au chemin de liaison comme ceci :

```xml
<TextBlock Text="{Binding Students[0].Name}"/>
```

## Chemin de Liaison Vide

Vous pouvez spécifier des liaisons de données sans un `Path`. Cela se lie au `DataContext` du `Control` lui-même (où la liaison est définie). Ces deux syntaxes sont équivalentes :

```xml
<TextBlock Text="{Binding}" />
<TextBlock Text="{Binding .}" />
```

## Mode de Liaison de Données

Vous pouvez changer la direction(s) dans laquelle les données sont synchronisées en spécifiant le `Mode`.

<img src={DataBindingModeDiagram} alt=''/>

Par exemple :

```xml
<TextBlock Text="{Binding Name, Mode=OneTime}" />
```

Les modes de liaison disponibles sont :

| Mode             | Description                                                                                                               |
|------------------|---------------------------------------------------------------------------------------------------------------------------|
| `OneWay`         | Les changements dans la source de données se propagent vers la cible de liaison.                                          |
| `TwoWay`         | Les changements dans la source de données se propagent vers la cible de liaison et vice-versa.                            |
| `OneTime`        | La valeur de la source de données est propagée lors de l'initialisation vers la cible de liaison, mais les changements suivants sont ignorés. |
| `OneWayToSource` | Les changements dans la cible de liaison se propagent à la source de données, mais pas dans l'autre sens.                 |
| `Default`        | Le mode de liaison est basé sur un mode par défaut défini dans le code pour la propriété. Voir ci-dessous.                |

Lorsque aucun `Mode` n'est spécifié, le `Default` est utilisé. Pour une propriété de contrôle qui ne change pas de valeur en raison de l'interaction de l'utilisateur, 
le mode par défaut est généralement `OneWay`. Pour une propriété de contrôle qui change de valeur en raison de l'entrée de l'utilisateur, le mode par défaut 
est généralement `TwoWay`.

Par exemple, le mode par défaut pour une propriété `TextBlock.Text` est `OneWay`, et le mode par défaut pour une propriété `TextBox.Text` est `TwoWay`.

## Sources de Liaison de Données

La `Source` spécifie l'instance d'objet racine par rapport à laquelle le `Path` est relatif. Par défaut, il s'agit du `DataContext` du 
`Control` contenant. Le scénario le plus courant implique la liaison à un autre contrôle en utilisant les paramètres `ElementName` ou `RelativeSource` 
ou avec leur syntaxe abrégée dans le cadre du `Path` (`#nomDuContrôle` et `$parent[TypeDeContrôle]` respectivement).

```xml
<TextBox Name="input" />
<TextBlock Text="{Binding Text, ElementName=input}" />
<TextBlock Text="{Binding #input.Text}" />

<TextBlock Text="{Binding Title, 
    RelativeSource={RelativeSource FindAncestor, AncestorType=Window}}" />
<TextBlock Text="{Binding $parent[Window].Title}" />
```

:::info
Pour plus de détails sur la façon de lier aux contrôles, voir [ici](../../../guides/data-binding/binding-to-controls)
:::

## Conversion des valeurs liées

Les liaisons offrent plusieurs approches pour convertir ou substituer la valeur fournie par une liaison de données en un type ou une valeur plus appropriée pour la propriété cible.

### Formatage de chaîne

Vous pouvez appliquer un modèle à une liaison `OneWay` pour formater la propriété source liée en texte via le paramètre `StringFormat` qui utilise `string.Format` en interne.

L'index du modèle est basé sur zéro et doit être à l'intérieur d'accolades. Lorsque les accolades sont au début du modèle, même lorsqu'elles sont aussi à l'intérieur de guillemets simples, elles doivent être échappées. L'échappement se fait en ajoutant une paire vide d'accolades au début du modèle ou un antislash devant chaque accolade.

```xml
<TextBlock Text="{Binding FloatProperty, StringFormat={}{0:0.0}}" />
```

Alternativement, vous pouvez utiliser des antislashs pour échapper les accolades nécessaires pour le modèle. Par exemple :

```xml
<TextBlock Text="{Binding FloatProperty, StringFormat=\{0:0.0\}}" />
```

Cependant, si votre modèle ne commence pas par un zéro, vous n'avez pas besoin de l'échappement. De plus, si vous avez des espaces dans votre modèle, vous devez les entourer de guillemets simples. Par exemple :

```xml
<TextBlock Text="{Binding Animals.Count, StringFormat='J'ai {0} animaux.'}" />
```

Notez que cela signifie que si votre modèle commence par la valeur que vous liez, alors vous avez besoin de l'échappement. Par exemple :

```xml
<TextBlock Text="{Binding Animals.Count,
    StringFormat='{}{0} animaux vivent à la ferme.'}" />
```

### Formatage de Chaîne avec Plusieurs Paramètres

`MultiBinding` peut être utilisé pour formater une chaîne qui nécessite plusieurs paramètres liés. L'exemple ci-dessous formate plusieurs entrées numériques en une seule chaîne à afficher.

```xml
<StackPanel Spacing="8">
  <NumericUpDown x:Name="rouge" Minimum="0" Maximum="255" Value="0" FormatString="{}{0:0.}" Foreground="Red" />
  <NumericUpDown x:Name="vert" Minimum="0" Maximum="255" Value="0" FormatString="{}{0:0.}" Foreground="Green" />
  <NumericUpDown x:Name="bleu" Minimum="0" Maximum="255" Value="0" FormatString="{}{0:0.}" Foreground="Blue" />

  <TextBlock>
    <TextBlock.Text>
      <MultiBinding StringFormat="(r: {0:0.}, g: {1:0.}, b: {2:0.})">
        <Binding Path="Value" ElementName="rouge" />
        <Binding Path="Value" ElementName="vert" />
        <Binding Path="Value" ElementName="bleu" />
      </MultiBinding>
    </TextBlock.Text>
  </TextBlock>
</StackPanel>
```

`FormatString` est utilisé en interne par `NumericUpDown` pour changer la façon dont sa valeur est affichée. Ici, parce que les couleurs RGB sont des entiers, nous ne devrions pas afficher la partie décimale, donc `0.` est fourni comme spécificateur de format numérique personnalisé que .NET comprend.

Si les valeurs des entrées sont `rouge = 100`, `vert = 80`, et `bleu = 255`, alors le texte affiché sera 
`(r: 100, g: 80, b: 255)`.

:::tip
Une alternative consiste à utiliser une `InlineCollection` d'éléments `Run`, chacun avec son propre paramètre de liaison unique. Cela permet une personnalisation visuelle de chaque segment. Voir l'exemple [ici](/docs/reference/controls/textblock#run)
:::

### Conversions intégrées

Avalonia dispose d'une gamme de convertisseurs de liaison de données intégrés. Ceux-ci incluent :

* Convertisseurs de test de null
* Convertisseurs d'opérations booléennes

:::info
Pour une liste des convertisseurs de liaison de données intégrés d'Avalonia, consultez la référence [ici](../../../reference/built-in-data-binding-converters.md).
:::

### Conversions personnalisées

Si les convertisseurs intégrés ne répondent pas à vos besoins, vous pouvez créer un convertisseur personnalisé en implémentant `IValueConverter`.

:::info
Pour des conseils sur la façon de créer un convertisseur personnalisé, voir [ici](../../../guides/data-binding/how-to-create-a-custom-data-binding-converter).
:::

### FallbackValue

`FallbackValue` est utilisé lorsque la liaison de propriété ne peut pas être effectuée ou lorsqu'un convertisseur renvoie `AvaloniaProperty.UnsetValue`.

Un cas d'utilisation courant est lorsque une propriété parente dans une liaison de sous-propriété est `null`. Si `Student` est `null` ci-dessous, 
le `FallbackValue` sera utilisé :

```xml
<TextBlock Text="{Binding Student.Name, FallbackValue=Impossible de trouver le nom}"/>
```

:::tip
`ReflectionBinding` peut se lier à des types arbitraires sans tenir compte de la sécurité à la compilation. Lorsque la liaison ne peut pas être effectuée, 
`FallbackValue` peut être utile pour substituer une valeur.
:::

### TargetNullValue

Lorsque l'association à une propriété est créée avec succès et que la valeur de la propriété est `null`, `TargetNullValue` peut être utilisé pour fournir une valeur spécifique.

```xml
<StackPanel>
    <NumericUpDown x:Name="number" Value="200" />
    <TextBlock Text="{Binding #number.Value, TargetNullValue=Value is null}" />
</StackPanel>
```

<img src={TargetNullValueScreenshot} alt=''/>

## UpdateSourceTrigger <MinVersion version="11.1" />

Des contrôles comme `TextBox` synchroniseront leur liaison `Text` à la propriété source à chaque frappe par défaut. Dans certains cas d'utilisation, cela peut déclencher une tâche de longue durée ou une validation indésirable. `UpdateSourceTrigger` permet aux liaisons de spécifier quand la synchronisation doit avoir lieu.

| UpdateSourceTrigger | Description                                                                                         |
|---------------------|-----------------------------------------------------------------------------------------------------|
| `Default`           | Cela par défaut à `PropertyChanged`.                                                                |
| `PropertyChanged`   | Met à jour la source de liaison immédiatement chaque fois que la propriété cible de liaison change. |
| `LostFocus`         | Met à jour la source de liaison chaque fois que l'élément cible de liaison perd le focus.           |
| `Explicit`          | Met à jour la source de liaison uniquement lorsque vous appelez la méthode `BindingExpressionBase.UpdateSource()`. |

```xml
<StackPanel>
    <TextBox Text="{Binding #propertyChanged.Text}" />
    <TextBlock Name="propertyChanged" />

    <TextBox Text="{Binding #lostFocus.Text, UpdateSourceTrigger=LostFocus}" />
    <TextBlock Name="lostFocus" />
</StackPanel>
```

<img src={UpdateSourceTriggerScreenshot} alt=''/>
