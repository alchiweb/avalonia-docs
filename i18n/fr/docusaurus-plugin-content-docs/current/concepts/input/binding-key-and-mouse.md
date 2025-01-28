---
description: CONCEPTS - Input
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

import KeyMouseScreenshot from '/img/reference/controls/input/binding-key-mouse-test.gif';

# Liaison de touches et liaison de souris

- Cette section explique comment placer des touches de raccourci qui apparaissent souvent dans les outils professionnels dans divers contrôles.
- Par exemple, la liaison avec un double-clic ou la touche Entrée sur une simple boîte de liste.
- Cela fonctionne également pour DataGrid.

<Tabs
  defaultValue="xaml"
  values={[
      { label: 'XAML', value: 'xaml', },
      { label: 'CodeBehind', value: 'code-behind', },
      { label: 'ViewModel', value: 'ViewModel', },
  ]}
>
<TabItem value="xaml">

```xml
<UserControl ..>
    <StackPanel>
        <ListBox
            DoubleTapped="ListBox_DoubleTapped"
            ItemsSource="{Binding OperatingSystems}"
            SelectedItem="{Binding OS}">
            <ListBox.KeyBindings>
                <!--  Entrée  -->
                <KeyBinding Command="{Binding PrintItem}" Gesture="Enter" />
                <!--
                    Les liaisons de souris ne sont pas prises en charge.
                    Au lieu de cela, gérez-le dans le code-behind de la vue. (Événement DoubleTapped)
                -->
            </ListBox.KeyBindings>
        </ListBox>
        <TextBlock Text="{Binding Result}">
            <TextBlock.ContextMenu>
                <ContextMenu>
                    <!--  Clic droit  -->
                    <MenuItem Command="{Binding Clear}" Header="Clear" />
                </ContextMenu>
            </TextBlock.ContextMenu>
        </TextBlock>
    </StackPanel>
</UserControl>
```

</TabItem>
<TabItem value="code-behind">

```cs
public partial class MainView : UserControl
{
    public MainView()
    {
        InitializeComponent();
    }

    private void ListBox_DoubleTapped(object? sender, Avalonia.Input.TappedEventArgs e)
    {
        if (DataContext is MainViewModel vm)
        {
            vm.PrintItem.Execute(null);
        }
    }
}
```
</TabItem>  

<TabItem value="ViewModel">

```cs
public class MainViewModel : ViewModelBase
{
    public List<string> OperatingSystems =>
    [
        "Windows",
        "Linux",
        "Mac",
    ];
    public string OS { get; set; } = string.Empty;

    [Reactive]
    public string Result { get; set; } = string.Empty;

    public ICommand PrintItem { get; }
    public ICommand Clear { get; }

    public MainViewModel()
    {
        PrintItem = ReactiveCommand.Create(() => Result = OS);
        Clear = ReactiveCommand.Create(() => Result = string.Empty);
    }
}
```
</TabItem>  
</Tabs>

<img src={KeyMouseScreenshot} alt="" />
