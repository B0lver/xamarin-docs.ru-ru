---
title: Рисование простого кружка в SkiaSharp
description: В этой статье рассматриваются основы SkiaSharp документа, в том числе полотна и paint, в приложениях Xamarin.Forms и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: E3A4E373-F65D-45C8-8E77-577A804AC3F8
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: b4cd84e9134db2b2106af3205f189fbc2a92bdcc
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61018329"
---
# <a name="drawing-a-simple-circle-in-skiasharp"></a>Рисование простого кружка в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Познакомьтесь с основами SkiaSharp документе, включая полотна и рисования объектов_

В этой статье понятия рисованию графических элементов в Xamarin.Forms с помощью SkiaSharp, включая создание `SKCanvasView` объекта для размещения графики, обработки `PaintSurface` событий и с помощью `SKPaint` объект для указания цвета и другие графические атрибуты.

[ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программа содержит все примеры кода для этой серии статей SkiaSharp. На первой странице размещен право **простого кружка** и вызывает класс страницы [ `SimpleCirclePage` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs). Этот код показывает, как нарисовать круг в центре страницы с радиусом 100 пикселей. Имеет красный цвет контура круга и внутренней окружности — синий.

![](circle-images/circleexample.png "Синий круг, выделены красным")

[ `SimpleCirle` ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/SimpleCirclePage.cs) Страницы класс является производным от `ContentPage` и содержит два `using` директивы для SkiaSharp пространств имен:

```csharp
using SkiaSharp;
using SkiaSharp.Views.Forms;
```

Создает следующий конструктор класса [ `SKCanvasView` ](xref:SkiaSharp.Views.Forms.SKCanvasView) объекта, подключает обработчик для [ `PaintSurface` ](xref:SkiaSharp.Views.Forms.SKCanvasView.PaintSurface) событий и наборы `SKCanvasView` объект как содержимое страницы:

```csharp
public SimpleCirclePage()
{
    Title = "Simple Circle";

    SKCanvasView canvasView = new SKCanvasView();
    canvasView.PaintSurface += OnCanvasViewPaintSurface;
    Content = canvasView;
}
```

`SKCanvasView` Занимает всю область содержимого страницы. Кроме того, можно объединить `SKCanvasView` с помощью других Xamarin.Forms `View` производные, как можно будет увидеть в других примерах.

`PaintSurface` Обработчик событий, где сделать все прорисовки. Этот метод может вызываться несколько раз во время работы программы, поэтому он должен поддерживать все сведения необходимые для воссоздания отображения графики:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
}

```

[ `SKPaintSurfaceEventArgs` ](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs) Объект события имеет два свойства:

- [`Info`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Info) типа [`SKImageInfo`](xref:SkiaSharp.SKImageInfo)
- [`Surface`](xref:SkiaSharp.Views.Forms.SKPaintSurfaceEventArgs.Surface) типа [`SKSurface`](xref:SkiaSharp.SKSurface)

`SKImageInfo` Структура содержит сведения об области рисования, самое главное, ее ширину и высоту в пикселях. `SKSurface` Представляет самой поверхности рисования. В этой программе поверхности рисования является дисплея, но в других программах `SKSurface` объект также может представлять точечный рисунок, который используется для рисования в SkiaSharp.

Наиболее важным свойством `SKSurface` — [ `Canvas` ](xref:SkiaSharp.SKSurface.Canvas) типа [ `SKCanvas` ](xref:SkiaSharp.SKCanvas). Этот класс представляет собой графики, рисование контекст, который используется для выполнения фактического рисования. `SKCanvas` Инкапсулирует состояние графики, включающий преобразований графики, так и обрезки.

Ниже приведен типичный начале `PaintSurface` обработчик событий:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();
    ...
}

```

[ `Clear` ](xref:SkiaSharp.SKCanvas.Clear) Метод очищает canvas с прозрачным цветом. Перегрузка позволяет задать цвет фона для области.

Цель — для рисования синюю заливку красный кружок. Так как этого конкретного изображения содержит два различных цветов, задание необходимо реализовать в два этапа. Первым делом для рисования контура круга. Чтобы указать цвет и другие характеристики строки, вы создали и инициализировали [ `SKPaint` ](xref:SkiaSharp.SKPaint) объекта:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = Color.Red.ToSKColor(),
        StrokeWidth = 25
    };
    ...
}
```

[ `Style` ](xref:SkiaSharp.SKPaint.Style) Свойство указывает, что вы хотите *stroke* строку (в данном случае контура круга) вместо *заливки* внутренней. Три члена [ `SKPaintStyle` ](xref:SkiaSharp.SKPaintStyle) перечисления являются следующим образом:

- [`Fill`](xref:SkiaSharp.SKPaintStyle.Fill)
- [`Stroke`](xref:SkiaSharp.SKPaintStyle.Stroke)
- [`StrokeAndFill`](xref:SkiaSharp.SKPaintStyle.StrokeAndFill)

Значение по умолчанию — `Fill`. Третий параметр для обводки линии и цвет заливки.

Задайте [ `Color` ](xref:SkiaSharp.SKPaint.Color) свойства со значением типа [ `SKColor` ](xref:SkiaSharp.SKColor). Один из способов получения `SKColor` значение — преобразование Xamarin.Forms `Color` значение `SKColor` значение с помощью метода расширения [ `ToSKColor` ](xref:SkiaSharp.Views.Forms.Extensions.ToSKColor*). [ `Extensions` ](xref:SkiaSharp.Views.Forms.Extensions) В класс `SkiaSharp.Views.Forms` пространство имен включает в себя другие методы, которые преобразуют между Xamarin.Forms и SkiaSharp значениями.

[ `StrokeWidth` ](xref:SkiaSharp.SKPaint.StrokeWidth) Свойство указывающее толщину линии. Здесь он становится равным 25 пикселей.

Использовать `SKPaint` объекта, чтобы рисовать окружность:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    canvas.DrawCircle(info.Width / 2, info.Height / 2, 100, paint);
    ...
}
```

Координаты указываются относительно левого верхнего угла поверхность отображения. X координирует вправо и увеличение координаты Y направлена вниз. В рассказе о графике часто математической нотации (x, y) используется для обозначения точки. Точка (0, 0) находится в левом верхнем углу поверхности отображения и часто называется *origin*.

Первые два аргумента из `DrawCircle` указывают координаты X и Y центра круга. Они назначаются половины ширины и высоты поверхность отображения для размещения центра круга в центре поверхности отображения. Третий аргумент указывает радиус круга и последним аргументом является `SKPaint` объекта.

Для заливки внутренней окружности, его можно изменить два свойства класса `SKPaint` и вызовите `DrawCircle` еще раз. Этот код также показывает альтернативный способ получения `SKColor` значение из одного из многих поля [ `SKColors` ](xref:SkiaSharp.SKColors) структуры:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    ...
    paint.Style = SKPaintStyle.Fill;
    paint.Color = SKColors.Blue;
    canvas.DrawCircle(args.Info.Width / 2, args.Info.Height / 2, 100, paint);
}
```
На этот раз `DrawCircle` вызов заполняет элемент управления circle, с помощью нового свойства `SKPaint` объекта.

Ниже приведен программу на iOS, Android и универсальной платформы Windows.

[![](circle-images/simplecircle-small.png "Тройной снимок экрана страницы простого кружка")](circle-images/simplecircle-large.png#lightbox "тройной снимок экрана страницы простого кружка")

При запуске программы самостоятельно, вы можете включить телефон или симулятор сторону, чтобы увидеть, как перерисовке рисунок. Каждый раз, когда должна быть перерисована, рисунок `PaintSurface` обработчик событий вызывается снова.

Можно также цвета графических объектов с градиентов или плиток растрового изображения. Эти параметры будут подробно описаны в разделе на [ **шейдеры SkiaSharp**](../effects/shaders/index.md).

`SKPaint` Нечто большее, чем коллекция рисования свойства объекта. Эти объекты являются упрощенных. Вы можете повторно использовать `SKPaint` объектов, так как эта программа не, или можно создать несколько `SKPaint` объекты для отображения свойств различных комбинаций. Можно создать и инициализировать эти объекты за пределами `PaintSurface` обработчик событий и сохраните их как поля в класс страницы.

> [!NOTE]
> `SKPaint` Класс определяет [ `IsAntialias` ](xref:SkiaSharp.SKPaint.IsAntialias) Чтобы включить сглаживание при подготовке к просмотру рисунков. Сглаживание обычно приводит к визуально более гладкие грани, поэтому возможно, нужно присвоить этому свойству `true` в большинстве вашей `SKPaint` объектов. Для простоты данное свойство содержит _не_ в большинство образцов страниц.

Несмотря на то, что ширина контура круга указывается как 25 пикселей &mdash; или части радиус круга &mdash; кажется более тонкие и есть хорошее объяснение: Половинная ширина линии замещается синий круг. Аргументы для `DrawCircle` метод определяет абстрактный геометрические координаты круга. Синий внутренней подбирается к такому измерению к ближайшей точки, но 25 пикселей всей структуры служит связующим звеном геометрические круг &mdash; половина на внутри, а другая половина — снаружи.

В приведенном ниже примере [интеграция с Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md) статья демонстрирует это визуально.


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
