---
title: Xamarin.Formsиндикаторвиев
description: ''
ms.prod: ''
ms.assetId: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: a6bf11fd80dd5226ae26dd392e80f784a9b296bf
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84131993"
---
# <a name="xamarinforms-indicatorview"></a>Xamarin.Formsиндикаторвиев

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)

`IndicatorView`— Это элемент управления, отображающий индикаторы, представляющие количество элементов и текущую позиции в `CarouselView` :

[![Снимок экрана Карауселвиев и Индикаторвиев на iOS и Android](indicatorview-images/circles.png "Индикаторвиев круги")](indicatorview-images/circles-large.png#lightbox "Индикаторвиев круги")

`IndicatorView`доступна в Xamarin.Forms 4,4 на платформах iOS и Android, а также в 4,5 на универсальная платформа Windows. Однако в настоящее время он экспериментальен и может использоваться только путем добавления следующей строки кода в `AppDelegate` класс в iOS или в ваш `MainActivity` класс в Android перед вызовом `Forms.Init` :

```csharp
Forms.SetFlags("IndicatorView_Experimental");
```

`IndicatorView` определяет следующие свойства:

- `Count`, типа `int` , количество индикаторов.
- `HideSingle`Тип `bool` — указывает, должен ли индикатор быть скрытым, если существует только один. Значение по умолчанию — `true`.
- `IndicatorColor`Тип `Color` — Цвет индикаторов.
- `IndicatorSize`Тип — `double` Размер индикаторов. Значение по умолчанию — 6,0.
- `IndicatorLayout`Тип `Layout<View>` определяет класс макета, используемый для визуализации `IndicatorView` . Это свойство задается параметром Xamarin.Forms и обычно не требуется задавать разработчиками.
- `IndicatorTemplate`Тип `DataTemplate` — шаблон, определяющий внешний вид каждого индикатора.
- `IndicatorsShape`Тип `IndicatorShape` — Форма каждого индикатора.
- `ItemsSource`Тип `IEnumerable` — коллекция, для которой будут отображаться индикаторы. Это свойство будет автоматически установлено при `CarouselView.IndicatorView` установке свойства.
- `MaximumVisible`, типа `int` , максимальное количество видимых индикаторов. Значение по умолчанию — `int.MaxValue`.
- `Position`Тип `int` — текущий выбранный индекс индикатора. Это свойство использует `TwoWay` привязку. Это свойство будет автоматически установлено при `CarouselView.IndicatorView` установке свойства.
- `SelectedIndicatorColor`Тип `Color` — Цвет индикатора, представляющий текущий элемент в `CarouselView` .

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

## <a name="create-an-indicatorview"></a>Создание Индикаторвиев

В следующем примере показано, как создать экземпляр `IndicatorView` в XAML:

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="DarkGray"
                   HorizontalOptions="Center" />
</StackLayout>
```

В этом примере объект отображается `IndicatorView` под элементом `CarouselView` и с индикатором для каждого элемента в `CarouselView` . `IndicatorView`Заполняется данными путем присвоения `CarouselView.IndicatorView` свойству `IndicatorView` объекта значения. Каждый индикатор является светло-серым кругом, а индикатор, представляющий текущий элемент в, `CarouselView` темно-серый.

> [!IMPORTANT]
> Установка `CarouselView.IndicatorView` свойства приводит `IndicatorView.Position` к привязке свойства к `CarouselView.Position` свойству и `IndicatorView.ItemsSource` привязке свойства к `CarouselView.ItemsSource` свойству.

## <a name="change-indicator-shape"></a>Изменить форму индикатора

`IndicatorView`Класс имеет `IndicatorsShape` свойство, которое определяет форму индикаторов. Этому свойству может быть присвоено одно из `IndicatorShape` членов перечисления:

- `Circle`Указывает, что фигуры индикаторов будут циклическими. Это значение по умолчанию для свойства `IndicatorView.IndicatorsShape`.
- `Square`Указывает, что фигуры индикаторов будут квадратными.

В следующем примере показана `IndicatorView` Настройка для использования квадратных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorsShape="Square"
               IndicatorColor="LightGray"
               SelectedIndicatorColor="DarkGray" />
```

## <a name="change-indicator-size"></a>Изменить размер индикатора

`IndicatorView`Класс имеет `IndicatorSize` свойство типа `double` , которое определяет размер индикаторов в аппаратно-независимых единицах. Значение этого свойства по умолчанию равно 6,0.

В следующем примере показана `IndicatorView` Настройка для отображения более крупных индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               IndicatorSize="18" />
```

## <a name="limit-the-number-of-indicators-displayed"></a>Ограничение числа отображаемых индикаторов

`IndicatorView`Класс имеет `MaximumVisible` свойство типа `int` , которое определяет максимальное количество видимых индикаторов.

В следующем примере показана `IndicatorView` Настройка для отображения не более шести индикаторов:

```xaml
<IndicatorView x:Name="indicatorView"
               MaximumVisible="6" />
```

## <a name="define-indicator-appearance"></a>Определение внешнего вида индикатора

Внешний вид каждого индикатора можно определить, задав `IndicatorView.IndicatorTemplate` для свойства значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) .

```xaml
<StackLayout>
    <CarouselView ItemsSource="{Binding Monkeys}"
                  IndicatorView="indicatorView">
        <CarouselView.ItemTemplate>
            <!-- DataTemplate that defines item appearance -->
        </CarouselView.ItemTemplate>
    </CarouselView>
    <IndicatorView x:Name="indicatorView"
                   IndicatorColor="LightGray"
                   SelectedIndicatorColor="Black"
                   HorizontalOptions="Center">
        <IndicatorView.IndicatorTemplate>
            <DataTemplate>
                <Image Source="{FontImage &#xf30c;, FontFamily={OnPlatform iOS=Ionicons, Android=ionicons.ttf#}, Size=12}" />
            </DataTemplate>
        </IndicatorView.IndicatorTemplate>
    </IndicatorView>
</StackLayout>
```

Элементы, указанные в параметре, [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) определяют внешний вид каждого индикатора. В этом примере каждый индикатор имеет значение [`Image`](xref:Xamarin.Forms.Image) , которое отображает значок шрифта с помощью `FontImage` расширения разметки.

На следующих снимках экрана показаны индикаторы, отображаемые с помощью значка шрифта:

[![Снимок экрана с шаблоном Индикаторвиев в iOS и Android](indicatorview-images/templated.png "Шаблонные Индикаторвиев")](indicatorview-images/templated-large.png#lightbox "Шаблонные Индикаторвиев")

Дополнительные сведения о `FontImage` расширении разметки см. в разделе [расширение разметки фонтимаже](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension).

## <a name="related-links"></a>Связанные ссылки

- [Индикаторвиев (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-indicatorviewdemos/)
- [Расширение разметки FontImage](~/xamarin-forms/xaml/markup-extensions/consuming.md#fontimage-markup-extension)
