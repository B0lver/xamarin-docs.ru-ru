---
title: Классы стилей Xamarin.Forms
description: Классы стилей Xamarin.Forms включить несколько стилей, применяемых к элементу управления, не прибегая к наследования стиля.
ms.prod: xamarin
ms.assetid: 4762401E-2B48-48F1-B6E4-61F7AF8AA46F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
ms.openlocfilehash: a3ef0f96bcc955dcac4231f9eb9cf1ab16ee61aa
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65925288"
---
# <a name="xamarinforms-style-classes"></a>Классы стилей Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)

_Классы стилей Xamarin.Forms включить несколько стилей, применяемых к элементу управления, не прибегая к наследования стиля._

## <a name="create-style-classes"></a>Создание классов

Класс стиля можно создать, задав [ `Class` ](xref:Xamarin.Forms.Style.Class) свойство [ `Style` ](xref:Xamarin.Forms.Style) для `string` , представляющая имя класса. Это обеспечивает через определение явный стиль с помощью преимущество `x:Key` атрибута, что несколько классов стиля могут применяться к [ `VisualElement` ](xref:Xamarin.Forms.VisualElement).

> [!IMPORTANT]
> Несколько стилей могут совместно использовать то же имя класса, если они предназначены для разных типов. Это позволяет нескольким классам стиль, с одинаковыми именами, к разным типам целевого.

В следующем примере показаны три [ `BoxView` ](xref:Xamarin.Forms.BoxView) стиля классы и [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) класс стиля:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        <Style TargetType="BoxView"
               Class="Separator">
            <Setter Property="BackgroundColor"
                    Value="#CCCCCC" />
            <Setter Property="HeightRequest"
                    Value="1" />
        </Style>

        <Style TargetType="BoxView"
               Class="Rounded">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="10" />
        </Style>    

        <Style TargetType="BoxView"
               Class="Circle">
            <Setter Property="BackgroundColor"
                    Value="#1FAECE" />
            <Setter Property="WidthRequest"
                    Value="100" />
            <Setter Property="HeightRequest"
                    Value="100" />
            <Setter Property="HorizontalOptions"
                    Value="Start" />
            <Setter Property="CornerRadius"
                    Value="50" />
        </Style>

        <Style TargetType="VisualElement"
               Class="Rotated"
               ApplyToDerivedTypes="true">
            <Setter Property="Rotation"
                    Value="45" />
        </Style>        
    </ContentPage.Resources>
</ContentPage>
```

`Separator`, `Rounded`, И `Circle` классы каждый набор стилей [ `BoxView` ](xref:Xamarin.Forms.BoxView) свойства с указанными значениями.

`Rotated` Класс стиля имеет [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) из [ `VisualElement` ](xref:Xamarin.Forms.VisualElement), то есть может применяться только к `VisualElement` экземпляров. Тем не менее его [ `ApplyToDerivedTypes` ](xref:Xamarin.Forms.Style.ApplyToDerivedTypes) свойству `true`, которое гарантирует, что он может применяться ко всем элементам управления, которые являются производными от `VisualElement`, такие как [ `BoxView` ](xref:Xamarin.Forms.BoxView). Дополнительные сведения о применении стиля к производному типу, см. в разделе [применение стиля к производным типам](implicit.md#apply-a-style-to-derived-types).

Эквивалентный код на C# выглядит так:

```csharp
var separatorBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Separator",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#CCCCCC")
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 1
        }
    }
};

var roundedBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Rounded",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 10
        }
    }
};

var circleBoxViewStyle = new Style(typeof(BoxView))
{
    Class = "Circle",
    Setters =
    {
        new Setter
        {
            Property = VisualElement.BackgroundColorProperty,
            Value = Color.FromHex("#1FAECE")
        },
        new Setter
        {
            Property = VisualElement.WidthRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = VisualElement.HeightRequestProperty,
            Value = 100
        },
        new Setter
        {
            Property = View.HorizontalOptionsProperty,
            Value = LayoutOptions.Start
        },
        new Setter
        {
            Property = BoxView.CornerRadiusProperty,
            Value = 50
        }
    }
};

var rotatedVisualElementStyle = new Style(typeof(VisualElement))
{
    Class = "Rotated",
    ApplyToDerivedTypes = true,
    Setters =
    {
        new Setter
        {
            Property = VisualElement.RotationProperty,
            Value = 45
        }
    }
};

Resources = new ResourceDictionary
{
    separatorBoxViewStyle,
    roundedBoxViewStyle,
    circleBoxViewStyle,
    rotatedVisualElementStyle
};
```

## <a name="consume-style-classes"></a>Использовать классы стилей

Можно использовать классы стилей, задав [ `StyleClass` ](xref:Xamarin.Forms.NavigableElement.StyleClass) свойства элемента управления, который имеет тип `IList<string>`, в список имена классов стилей. Классы стиль будет применяться, при условии, что соответствующий тип элемента управления [ `TargetType` ](xref:Xamarin.Forms.Style.TargetType) стилей классов.

В следующем примере показаны три [ `BoxView` ](xref:Xamarin.Forms.BoxView) экземпляров, каждый набор другой стиль классам:

```xaml
<ContentPage ...>
    <ContentPage.Resources>
        ...
    </ContentPage.Resources>
    <StackLayout Margin="20">
        <BoxView StyleClass="Separator" />       
        <BoxView WidthRequest="100"
                 HeightRequest="100"
                 HorizontalOptions="Center"
                 StyleClass="Rounded, Rotated" />
        <BoxView HorizontalOptions="Center"
                 StyleClass="Circle" />
    </StackLayout>
</ContentPage>    
```

В этом примере первая [ `BoxView` ](xref:Xamarin.Forms.BoxView) реализуется разделитель строк, а третье `BoxView` является циклической. Второй `BoxView` два класса стиля применил к нему, которой присвойте ИТ округленное углы и вращать на 45 градусов:

![](style-class-images/boxviews.png "BoxViews с классы стилей")

> [!IMPORTANT]
> Несколько классов стиля можно применить к элементу управления, поскольку [ `StyleClass` ](xref:Xamarin.Forms.NavigableElement.StyleClass) свойство имеет тип `IList<string>`. В этом случае стиль классы применяются в возрастающем порядке списка. Таким образом когда несколько классов стиля такими же свойствами, свойство в классе стиля, который находится в высшей позиции списка будет иметь приоритет.

Эквивалентный код на C# выглядит так:

```csharp
...
Content = new StackLayout
{
    Children =
    {
        new BoxView { StyleClass = new [] { "Separator" } },
        new BoxView { WidthRequest = 100, HeightRequest = 100, HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Rounded", "Rotated" } },
        new BoxView { HorizontalOptions = LayoutOptions.Center, StyleClass = new [] { "Circle" } }
    }
};
```

## <a name="related-links"></a>Связанные ссылки

- [Основные стили (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Styles/BasicStyles/)
