---
id: data-templates
title: Modèles de données
---

Les modèles de données dans Avalonia offrent un moyen puissant de définir la représentation visuelle de vos données. Ils vous permettent de spécifier comment vos données doivent être présentées et formatées, vous permettant ainsi de créer des interfaces utilisateur dynamiques et personnalisables. Ce document vous présentera le concept de modèles de données dans Avalonia et démontrera comment les utiliser efficacement dans vos applications.

## Qu'est-ce qu'un modèle de données ?

Au fond, un modèle de données est une définition réutilisable qui spécifie comment présenter des données d'un type particulier. Il définit la structure visuelle et l'apparence des données lorsqu'elles sont affichées dans l'interface utilisateur. Dans Avalonia, un modèle de données est souvent associé à un contrôle de liste, tel qu'un `ListBox` ou `ItemsControl`, et est responsable du rendu des éléments individuels de données au sein de ce contrôle.

## Application d'un modèle de données à un ListBox

Pour appliquer un modèle de données à un `ListBox`, vous utilisez généralement la propriété `ItemTemplate` du contrôle.

Par exemple, si vous avez un `ListBox` qui doit afficher une collection d'objets `Item` en utilisant le modèle de données défini, vous pouvez définir la propriété `ItemTemplate` comme suit :

```xml
<ListBox ItemsSource="{Binding Items}">
  <ListBox.ItemTemplate>
    <DataTemplate>
        <StackPanel Orientation="Horizontal">
            <TextBlock Text="{Binding Name}" />
            <Image Source="{Binding ImageSource}" />
        </StackPanel>
    </DataTemplate>
  </ListBox.ItemTemplate>
</ListBox>
```

Dans cet exemple, le modèle de données définit une mise en page visuelle à l'aide d'un conteneur `StackPanel`. À l'intérieur du `StackPanel`, nous avons un `TextBlock` lié à la propriété `Name` de l'élément et un contrôle `Image` lié à la propriété `ImageSource`.

## Personnalisation des Modèles de Données

Les modèles de données peuvent être personnalisés et adaptés à des scénarios spécifiques. Vous pouvez inclure des éléments visuels supplémentaires, appliquer des styles et même définir des modèles imbriqués au sein d'un modèle de données. En utilisant des expressions de liaison de données et des convertisseurs, vous pouvez remplir et formater dynamiquement les éléments visuels en fonction des propriétés de données.

## Lectures Complémentaires

Pour plus d'informations, consultez l'approfondissement sur les [modèles de données](../../concepts/templates).