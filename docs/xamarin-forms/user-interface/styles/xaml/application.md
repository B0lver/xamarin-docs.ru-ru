---
title: Глобальные стили в Xamarin.Forms
description: Стили могут быть сделаны доступными глобально, добавив их в словарь ресурсов приложения. Это помогает избежать дублирования стилей всех страниц или элементов управления.
ms.prod: xamarin
ms.assetid: BDC65F82-65E0-4C8E-BB91-8E340EB2D15A
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/17/2016
ms.openlocfilehash: f8fb026cd9a8dfbfd5abf13c9b11463bf84f7e0b
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647238"
---
# <a name="global-styles-in-xamarinforms"></a>Глобальные стили в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)

_Стили могут быть сделаны доступными глобально, добавив их в словарь ресурсов приложения. Это помогает избежать дублирования стилей всех страниц или элементов управления._

## <a name="create-a-global-style-in-xaml"></a>Создание глобального стиля в XAML

По умолчанию все приложения Xamarin.Forms, создаваемые на основе шаблона, используют класс **App** для реализации подкласса [`Application`](xref:Xamarin.Forms.Application). Чтобы объявить [ `Style` ](xref:Xamarin.Forms.Style) на уровне приложения, в приложении [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) с помощью XAML, значение по умолчанию **приложения** класса должны быть заменены XAML **Приложения** класс и связанные с выделенным кодом. Дополнительные сведения см. в разделе [работа с класс App](~/xamarin-forms/app-fundamentals/application-class.md).

В следующем коде показано в примере [ `Style` ](xref:Xamarin.Forms.Style) объявлен на уровне приложения:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.App">
    <Application.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                <Setter Property="HorizontalOptions" Value="Center" />
                <Setter Property="VerticalOptions" Value="CenterAndExpand" />
                <Setter Property="BorderColor" Value="Lime" />
                <Setter Property="BorderRadius" Value="5" />
                <Setter Property="BorderWidth" Value="5" />
                <Setter Property="WidthRequest" Value="200" />
                <Setter Property="TextColor" Value="Teal" />
            </Style>
        </ResourceDictionary>
    </Application.Resources>
</Application>
```

Это [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) определяет одно *явные* стиль, `buttonStyle`, который будет использоваться для задания внешнего вида [ `Button` ](xref:Xamarin.Forms.Button) экземпляров. Тем не менее, могут быть глобальные стили *явные* или *неявное*.

В следующем примере кода показано применение страницы XAML `buttonStyle` на страницу [ `Button` ](xref:Xamarin.Forms.Button) экземпляров:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Это приводит к появлению, показано на следующем снимке экрана:

[![](application-images/application-styles-1.png "Глобальные стили пример")](application-images/application-styles-1-large.png#lightbox "пример глобальные стили")

Сведения о создании стилей на странице [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary), см. в разделе [явные стили](~/xamarin-forms/user-interface/styles/explicit.md) и [неявные стили](~/xamarin-forms/user-interface/styles/implicit.md).

### <a name="override-styles"></a>Переопределить стили

Стили более низкого уровня в иерархии представлений имеют приоритет над определенных выше вверх. Например, установка [ `Style` ](xref:Xamarin.Forms.Style) , задает [ `Button.TextColor` ](xref:Xamarin.Forms.Button.TextColor) для `Red` приложения будут переопределены уровень стиль уровня страницы, который задает `Button.TextColor` для `Green`. Аналогичным образом стиль уровня будут переопределены уровня стиль элемента управления. Кроме того Если `Button.TextColor` задано напрямую на свойства элемента управления, это будет иметь приоритет над любые стили. В следующем примере кода демонстрируется такой приоритет:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms" xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml" x:Class="Styles.ApplicationStylesPage" Title="Application" IconImageSource="xaml.png">
    <ContentPage.Resources>
        <ResourceDictionary>
            <Style x:Key="buttonStyle" TargetType="Button">
                ...
                <Setter Property="TextColor" Value="Red" />
            </Style>
        </ResourceDictionary>
    </ContentPage.Resources>
    <ContentPage.Content>
        <StackLayout Padding="0,20,0,0">
            <StackLayout.Resources>
                <ResourceDictionary>
                    <Style x:Key="buttonStyle" TargetType="Button">
                        ...
                        <Setter Property="TextColor" Value="Blue" />
                    </Style>
                </ResourceDictionary>
            </StackLayout.Resources>
            <Button Text="These buttons" Style="{StaticResource buttonStyle}" />
            <Button Text="are demonstrating" Style="{StaticResource buttonStyle}" />
            <Button Text="application style overrides" Style="{StaticResource buttonStyle}" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

Исходный `buttonStyle`, определенные на уровне приложения, переопределенный методом `buttonStyle` экземпляр, определенный на уровне страниц. Кроме того, переопределяется стиля уровня страницы на уровне управления `buttonStyle`. Таким образом [ `Button` ](xref:Xamarin.Forms.Button) экземпляры отображаются голубым цветом, как показано на следующем снимке экрана:

[![](application-images/application-styles-2.png "Переопределение пример стилей")](application-images/application-styles-2-large.png#lightbox "переопределение пример стилей")

## <a name="create-a-global-style-in-c35"></a>Создание глобального стиля в C&#35;

[`Style`](xref:Xamarin.Forms.Style) экземпляры можно добавить в приложение [ `Resources` ](xref:Xamarin.Forms.VisualElement.Resources) коллекции в C# путем создания нового [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary)и затем добавив `Style` экземпляры `ResourceDictionary`, как в следующем примере кода показан:

```csharp
public class App : Application
{
    public App ()
    {
        var buttonStyle = new Style (typeof(Button)) {
            Setters = {
                ...
                new Setter { Property = Button.TextColorProperty,    Value = Color.Teal }
            }
        };

        Resources = new ResourceDictionary ();
        Resources.Add ("buttonStyle", buttonStyle);
        ...
    }
    ...
}
```

Конструктор определяет одно *явные* стиль для применения к [ `Button` ](xref:Xamarin.Forms.Button) экземпляров всего приложения. *Явные* [ `Style` ](xref:Xamarin.Forms.Style) экземпляры добавляются [ `ResourceDictionary` ](xref:Xamarin.Forms.ResourceDictionary) с помощью [ `Add` ](xref:Xamarin.Forms.ResourceDictionary.Add(System.String,System.Object)) метод, указывая `key`строка для ссылки на `Style` экземпляра. `Style` Экземпляр затем можно применить ко всем элементам управления в приложении правильного типа. Тем не менее, могут быть глобальные стили *явные* или *неявное*.

В следующем примере кода показано, C# страницы применение `buttonStyle` на страницу [ `Button` ](xref:Xamarin.Forms.Button) экземпляров:

```csharp
public class ApplicationStylesPageCS : ContentPage
{
    public ApplicationStylesPageCS ()
    {
        ...
        Content = new StackLayout {
            Children = {
                new Button { Text = "These buttons", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "are demonstrating", Style = (Style)Application.Current.Resources ["buttonStyle"] },
                new Button { Text = "application styles", Style = (Style)Application.Current.Resources ["buttonStyle"]
                }
            }
        };
    }
}
```

`buttonStyle` Применяется к [ `Button` ](xref:Xamarin.Forms.Button) экземпляров, задав их [ `Style` ](xref:Xamarin.Forms.NavigableElement.Style) свойства и определяет отображение `Button` экземпляров.

## <a name="related-links"></a>Связанные ссылки

- [Расширения разметки XAML](~/xamarin-forms/xaml/xaml-basics/xaml-markup-extensions.md)
- [Основные стили (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-styles-basicstyles)
- [Работа с использованием стилей (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [ResourceDictionary](xref:Xamarin.Forms.ResourceDictionary)
- [Стиль](xref:Xamarin.Forms.Style)
- [Метод задания](xref:Xamarin.Forms.Setter)
