---
title: "Xamarin.Forms Stepper"
description: "The Xamarin.Forms Stepper allows a user to select a numeric value from a range of values. It consists of two buttons labeled with minus and plus signs. Manipulating the two buttons changes the selected value incrementally."
ms.service: xamarin
ms.assetid: 62571B3E-D84B-4F52-9FC7-C105D6733B16
ms.subservice: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/17/2018
no-loc: [Xamarin.Forms, Xamarin.Essentials]
---

# Xamarin.Forms Stepper

_Use a Stepper for selecting a numeric value from a range of values._

The Xamarin.Forms [`Stepper`](xref:Xamarin.Forms.Stepper) consists of two buttons labeled with minus and plus signs. These buttons can be manipulated by the user to incrementally select a `double` value from a range of values.

The [`Stepper`](xref:Xamarin.Forms.Stepper) defines four properties of type `double`:

- [`Increment`](xref:Xamarin.Forms.Stepper.Increment) is the amount to change the selected value by, with a default value of 1.
- [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) is the minimum of the range, with a default value of 0.
- [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) is the maximum of the range, with a default value of 100.
- [`Value`](xref:Xamarin.Forms.Stepper.Value) is the stepper's value, which can range between `Minimum` and `Maximum` and has a default value of 0.

All of these properties are backed by [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) objects. The [`Value`](xref:Xamarin.Forms.Stepper.Value) property has a default binding mode of [`BindingMode.TwoWay`](xref:Xamarin.Forms.BindingMode.TwoWay), which means that it's suitable as a binding source in an application that uses the [Model-View-ViewModel (MVVM)](~/xamarin-forms/enterprise-application-patterns/mvvm.md) architecture.

> [!WARNING]
> Internally, the [`Stepper`](xref:Xamarin.Forms.Stepper) ensures that [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) is less than [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum). If `Minimum` or `Maximum` are ever set so that `Minimum` is not less than `Maximum`, an exception is raised. For more information on setting the `Minimum` and `Maximum` properties, see [Precautions](#precautions) section.

The [`Stepper`](xref:Xamarin.Forms.Stepper) coerces the [`Value`](xref:Xamarin.Forms.Stepper.Value) property so that it is between [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) and [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum), inclusive. If the `Minimum` property is set to a value greater than the `Value` property, the `Stepper` sets the `Value` property to `Minimum`. Similarly, if `Maximum` is set to a value less than `Value`, then `Stepper` sets the `Value` property to `Maximum`.

[`Stepper`](xref:Xamarin.Forms.Stepper) defines a [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event that is fired when the [`Value`](xref:Xamarin.Forms.Stepper.Value) changes, either through user manipulation of the `Stepper` or when the application sets the `Value` property directly. A `ValueChanged` event is also fired when the `Value` property is coerced as described in the previous paragraph.

The [`ValueChangedEventArgs`](xref:Xamarin.Forms.ValueChangedEventArgs) object that accompanies the [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event has two properties, both of type `double`: [`OldValue`](xref:Xamarin.Forms.ValueChangedEventArgs.OldValue) and [`NewValue`](xref:Xamarin.Forms.ValueChangedEventArgs.NewValue). At the time the event is fired, the value of `NewValue` is the same as the [`Value`](xref:Xamarin.Forms.Stepper.Value) property of the [`Stepper`](xref:Xamarin.Forms.Stepper) object.

## Basic Stepper code and markup

The sample contains three pages that are functionally identical, but are implemented in different ways. The first page uses only C# code, the second uses XAML with an event handler in code, and third is able to avoid the event handler by using data binding in the XAML file.

### Creating a Stepper in code

The **Basic Stepper Code** page in the sample shows how to create a [`Stepper`](xref:Xamarin.Forms.Stepper) and two [`Label`](xref:Xamarin.Forms.Label) objects in code:

```csharp
public class BasicStepperCodePage : ContentPage
{
    public BasicStepperCodePage()
    {
        Label rotationLabel = new Label
        {
            Text = "ROTATING TEXT",
            FontSize = Device.GetNamedSize(NamedSize.Large, typeof(Label)),
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Label displayLabel = new Label
        {
            Text = "(uninitialized)",
            HorizontalOptions = LayoutOptions.Center,
            VerticalOptions = LayoutOptions.CenterAndExpand
        };

        Stepper stepper = new Stepper
        {
            Maximum = 360,
            Increment = 30,
            HorizontalOptions = LayoutOptions.Center
        };
        stepper.ValueChanged += (sender, e) =>
        {
            rotationLabel.Rotation = stepper.Value;
            displayLabel.Text = string.Format("The Stepper value is {0}", e.NewValue);
        };

        Title = "Basic Stepper Code";
        Content = new StackLayout
        {
            Margin = new Thickness(20),
            Children = { rotationLabel, stepper, displayLabel }
        };
    }
}
```

The [`Stepper`](xref:Xamarin.Forms.Stepper) is initialized to have a [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) property of 360, and an [`Increment`](xref:Xamarin.Forms.Stepper.Increment) property of 30. Manipulating the `Stepper` changes the selected value incrementally between [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) to `Maximum` based on the value of the `Increment` property. The [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) handler of the `Stepper` uses the [`Value`](xref:Xamarin.Forms.Stepper.Value) property of the `stepper` object to set the [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) property of the first [`Label`](xref:Xamarin.Forms.Label)  and uses the `string.Format` method with the `NewValue` property of the event arguments to set the [`Text`](xref:Xamarin.Forms.Label.Text) property of the second `Label`. These two approaches to obtain the current value of the `Stepper` are interchangeable.

The following screenshots show the **Basic Stepper Code** page:

[![Basic Stepper Code](stepper-images/basic-stepper-code.png "Basic Stepper Code")](stepper-images/basic-stepper-code-large.png#lightbox)

The second [`Label`](xref:Xamarin.Forms.Label)  displays the text "(uninitialized)" until the [`Stepper`](xref:Xamarin.Forms.Stepper) is manipulated, which causes the first [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event to be fired.

### Creating a Stepper in XAML

The **Basic Stepper XAML** page is functionally the same as **Basic Stepper Code** but implemented mostly in XAML:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperXAMLPage"
             Title="Basic Stepper XAML">
    <StackLayout Margin="20">
        <Label x:Name="_rotatingLabel"
               Text="ROTATING TEXT"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center"
                 ValueChanged="OnStepperValueChanged" />
        <Label x:Name="_displayLabel"
               Text="(uninitialized)"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />        
    </StackLayout>
</ContentPage>
```

The code-behind file contains the handler for the [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event:

```csharp
public partial class BasicStepperXAMLPage : ContentPage
{
    public BasicStepperXAMLPage()
    {
        InitializeComponent();
    }

    void OnStepperValueChanged(object sender, ValueChangedEventArgs e)
    {
        double value = e.NewValue;
        _rotatingLabel.Rotation = value;
        _displayLabel.Text = string.Format("The Stepper value is {0}", value);
    }
}
```

It's also possible for the event handler to obtain the [`Stepper`](xref:Xamarin.Forms.Stepper) that is firing the event through the `sender` argument. The [`Value`](xref:Xamarin.Forms.Stepper.Value) property contains the current value:

```csharp
double value = ((Stepper)sender).Value;
```

If the [`Stepper`](xref:Xamarin.Forms.Stepper) object were given a name in the XAML file with an `x:Name` attribute (for example, "stepper"), then the event handler could reference that object directly:

```csharp
double value = stepper.Value;
```

### Data binding the Stepper

The **Basic Stepper Bindings** page shows how to write a nearly equivalent application that eliminates the [`Value`](xref:Xamarin.Forms.Stepper.Value) event handler by using [Data Binding](~/xamarin-forms/app-fundamentals/data-binding/index.md):

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="StepperDemo.BasicStepperBindingsPage"
             Title="Basic Stepper Bindings">
    <StackLayout Margin="20">
        <Label Text="ROTATING TEXT"
               Rotation="{Binding Source={x:Reference _stepper}, Path=Value}"
               FontSize="Large"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
        <Stepper x:Name="_stepper"
                 Maximum="360"
                 Increment="30"
                 HorizontalOptions="Center" />
        <Label Text="{Binding Source={x:Reference _stepper}, Path=Value, StringFormat='The Stepper value is {0:F0}'}"
               HorizontalOptions="Center"
               VerticalOptions="CenterAndExpand" />
    </StackLayout>
</ContentPage>
```

The [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation) property of the first [`Label`](xref:Xamarin.Forms.Label) is bound to the [`Value`](xref:Xamarin.Forms.Stepper.Value) property of the [`Stepper`](xref:Xamarin.Forms.Stepper), as is the [`Text`](xref:Xamarin.Forms.Label.Text) property of the second `Label` with a `StringFormat` specification. The **Basic Stepper Bindings** page functions a little differently from the two previous pages: When the page first appears, the second `Label` displays the text string with the value. This is a benefit of using data binding. To display text without data binding, you'd need to specifically initialize the `Text` property of the `Label` or simulate a firing of the [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event by calling the event handler from the class constructor.

## Precautions

The value of the [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) property must always be less than the value of the [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) property. The following code snippet causes the [`Stepper`](xref:Xamarin.Forms.Stepper) to raise an exception:

```csharp
// Throws an exception!
Stepper stepper = new Stepper
{
    Minimum = 180,
    Maximum = 360
};
```

The C# compiler generates code that sets these two properties in sequence, and when the [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) property is set to 180, it is greater than the default [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) value of 100. You can avoid the exception in this case by setting the `Maximum` property first:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

Setting [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) to 360 is not a problem because it is greater than the default [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) value of 0. When `Minimum` is set, the value is less than the `Maximum` value of 360.

The same problem exists in XAML. Set the properties in an order that ensures that [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) is always greater than `Minimum`:

```xaml
<Stepper Maximum="360"
         Minimum="180" ... />
```

You can set the [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) and [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) values to negative numbers, but only in an order where `Minimum` is always less than `Maximum`:

```xaml
<Stepper Minimum="-360"
         Maximum="-180" ... />
```

The [`Value`](xref:Xamarin.Forms.Stepper.Value) property is always greater than or equal to the [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) value and less than or equal to [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum). If `Value` is set to a value outside that range, the value will be coerced to lie within the range, but no exception is raised. For example, this code will *not* raise an exception:

```csharp
Stepper stepper = new Stepper
{
    Value = 180
};
```

Instead, the [`Value`](xref:Xamarin.Forms.Stepper.Value) property is coerced to the [`Maximum`](xref:Xamarin.Forms.Stepper.Maximum) value of 100.

Here's a code snippet shown above:

```csharp
Stepper stepper = new Stepper
{
    Maximum = 360,
    Minimum = 180
};
```

When [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) is set to 180, then [`Value`](xref:Xamarin.Forms.Stepper.Value) is also set to 180.

If a [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event handler has been attached at the time that the [`Value`](xref:Xamarin.Forms.Stepper.Value) property is coerced to something other than its default value of 0, then a `ValueChanged` event is fired. Here's a snippet of XAML:

```xaml
<Stepper ValueChanged="OnStepperValueChanged"
         Maximum="360"
         Minimum="180" />
```

When [`Minimum`](xref:Xamarin.Forms.Stepper.Minimum) is set to 180, [`Value`](xref:Xamarin.Forms.Stepper.Value) is also set to 180, and the [`ValueChanged`](xref:Xamarin.Forms.Stepper.ValueChanged) event is fired. This might occur before the rest of the page has been constructed, and the handler might attempt to reference other elements on the page that have not yet been created. You might want to add some code to the `ValueChanged` handler that checks for `null` values of other elements on the page. Or, you can set the `ValueChanged` event handler after the [`Stepper`](xref:Xamarin.Forms.Stepper) values have been initialized.

## Related Links

- [Stepper API](xref:Xamarin.Forms.Stepper)
