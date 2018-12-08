---
title: Простая анимация в SkiaSharp
description: В этой статье объясняется, как анимация графики SkiaSharp в Xamarin.Forms приложений и демонстрирует это с помощью примера кода.
ms.prod: xamarin
ms.technology: xamarin-skiasharp
ms.assetid: 31C96FD6-07E4-4473-A551-24753A5118C3
author: davidbritch
ms.author: dabritch
ms.date: 03/10/2017
ms.openlocfilehash: 8a533dd48acf698667044d600338555b6c00a0ae
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2018
ms.locfileid: "53057637"
---
# <a name="basic-animation-in-skiasharp"></a>Простая анимация в SkiaSharp

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)

_Узнайте, как анимация графики SkiaSharp_

Можно анимировать графики SkiaSharp в Xamarin.Forms, вызывая `PaintSurface` метод периодически, вызываемый каждый раз, когда графики немного по-разному. Ниже приведен анимацию, описанных далее в этой статье с концентрических окружностей, казалось бы, разверните в центре.

![](animation-images/animationexample.png "Несколько концентрических окружностей, казалось бы, развернув в центре")

**Pulsating эллипс** странице в [ **SkiaSharpFormsDemos** ](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/) программы анимирует осей эллипса, таким образом, он должен быть pulsating и даже управления частота этой pulsation. [ **PulsatingEllipsePage.xaml** ](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/PulsatingEllipsePage.xaml) файл экземпляр Xamarin.Forms `Slider` и `Label` для отображения текущее значение ползунка. Это распространенный способ интегрируйте `SKCanvasView` с другими представлениями Xamarin.Forms:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.PulsatingEllipsePage"
             Title="Pulsating Ellipse">
    <Grid>
        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Slider x:Name="slider"
                Grid.Row="0"
                Maximum="10"
                Minimum="0.1"
                Value="5"
                Margin="20, 0" />

        <Label Grid.Row="1"
               Text="{Binding Source={x:Reference slider},
                              Path=Value,
                              StringFormat='Cycle time = {0:F1} seconds'}"
               HorizontalTextAlignment="Center" />

        <skia:SKCanvasView x:Name="canvasView"
                           Grid.Row="2"
                           PaintSurface="OnCanvasViewPaintSurface" />
    </Grid>
</ContentPage>
```

Создает файл с выделенным кодом `Stopwatch` объект для использования в качестве высокоточных часы. `OnAppearing` Переопределить наборы `pageIsActive` поле `true` и вызывает метод с именем `AnimationLoop`. `OnDisappearing` Задает переопределения, которое `pageIsActive` поле `false`:

```csharp
Stopwatch stopwatch = new Stopwatch();
bool pageIsActive;
float scale;            // ranges from 0 to 1 to 0

public PulsatingEllipsePage()
{
    InitializeComponent();
}

protected override void OnAppearing()
{
    base.OnAppearing();
    pageIsActive = true;
    AnimationLoop();
}

protected override void OnDisappearing()
{
    base.OnDisappearing();
    pageIsActive = false;
}
```

`AnimationLoop` Запуска метода `Stopwatch` и затем циклы при `pageIsActive` является `true`. Это по сути «бесконечный цикл» страницы активен, но оно не вызывает программа зависает, так как цикл завершается вызов `Task.Delay` с `await` оператор, который позволяет другие части функции программы. Аргумент `Task.Delay` для выполнения через секунду 1/30. Этот параметр определяет частоту кадров анимации.

```csharp
async Task AnimationLoop()
{
    stopwatch.Start();

    while (pageIsActive)
    {
        double cycleTime = slider.Value;
        double t = stopwatch.Elapsed.TotalSeconds % cycleTime / cycleTime;
        scale = (1 + (float)Math.Sin(2 * Math.PI * t)) / 2;
        canvasView.InvalidateSurface();
        await Task.Delay(TimeSpan.FromSeconds(1.0 / 30));
    }

    stopwatch.Stop();
}

```

`while` Цикл начинается с получения времени цикла от `Slider`. Это время в секундах, например, 5. Вторая инструкция вычисляет значение `t` для *время*. Для `cycleTime` 5, `t` увеличивается от 0 до 1 каждые 5 секунд. Аргумент `Math.Sin` функции в второй оператор в диапазоне от 0 до 2π каждые 5 секунд. `Math.Sin` Функция возвращает значение в диапазоне от 0 до 1 обратно к 0, а затем к &ndash;1 и 0 каждые 5 секунд, но со значениями, которые изменяют более медленно, если значение равно 1 или -1. Значение 1 добавляется, поэтому значения всегда имеют положительное значение, а затем он делится на 2, поэтому диапазон значений: от ½ значение 1, чтобы ½ равным 0, чтобы ½, но медленнее, если значение равно приблизительно 1 и 0. Он сохраняется в `scale` поля и `SKCanvasView` становится недействительным.

`PaintSurface` Метод использует это `scale` значение для расчета осей эллипса:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    float maxRadius = 0.75f * Math.Min(info.Width, info.Height) / 2;
    float minRadius = 0.25f * maxRadius;

    float xRadius = minRadius * scale + maxRadius * (1 - scale);
    float yRadius = maxRadius * scale + minRadius * (1 - scale);

    using (SKPaint paint = new SKPaint())
    {
        paint.Style = SKPaintStyle.Stroke;
        paint.Color = SKColors.Blue;
        paint.StrokeWidth = 50;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);

        paint.Style = SKPaintStyle.Fill;
        paint.Color = SKColors.SkyBlue;
        canvas.DrawOval(info.Width / 2, info.Height / 2, xRadius, yRadius, paint);
    }
}
```

Метод вычисляет максимальное radius, в зависимости от размера области отображения и минимальный радиус, в зависимости от максимального радиуса. `scale` Значение анимируется от 0 до 1 и значение 0, поэтому данный метод использует, чтобы вычислить `xRadius` и `yRadius` , варьируется в диапазоне от `minRadius` и `maxRadius`. Эти значения используются для рисования и заливка эллипса:

[![](animation-images/pulsatingellipse-small.png "Тройной снимок экрана страницы пульсирующего эллипс")](animation-images/pulsatingellipse-large.png#lightbox "тройной снимок экрана страницы пульсирующего эллипс")

Обратите внимание, что `SKPaint` объект создается в `using` блока. Как и многие классы SkiaSharp `SKPaint` является производным от `SKObject`, который является производным от `SKNativeObject`, который реализует [ `IDisposable` ](xref:System.IDisposable) интерфейс. `SKPaint` переопределяет `Dispose` метод для освобождения неуправляемых ресурсов.

 Поместив `SKPaint` в `using` блок гарантирует, что `Dispose` вызывается в конце блока, чтобы освободить эти неуправляемые ресурсы. Это происходит в любом случае, когда память, используемая `SKPaint` объект освобождается сборщиком мусора .NET, но в коде анимации, лучше упреждающей освобождение памяти более правильным образом.

 В данном случае лучшим решением является создание двух `SKPaint` объектов один раз и сохраните их как поля.

Вот что **круги, развернув** анимации. [ `ExpandingCirclesPage` ](https://github.com/xamarin/xamarin-forms-samples/blob/skia-sharp-forms/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Basics/ExpandingCirclesPage.cs) Класс начинается, определив несколько полей, включая `SKPaint` объекта:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    const double cycleTime = 1000;       // in milliseconds

    SKCanvasView canvasView;
    Stopwatch stopwatch = new Stopwatch();
    bool pageIsActive;
    float t;
    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke
    };

    public ExpandingCirclesPage()
    {
        Title = "Expanding Circles";

        canvasView = new SKCanvasView();
        canvasView.PaintSurface += OnCanvasViewPaintSurface;
        Content = canvasView;
    }
    ...
}
```

Эта программа использует другой подход к анимации на основе Xamarin.Forms `Device.StartTimer` метод. `t` Поле анимируется от 0 до 1 каждые `cycleTime` миллисекунд:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    protected override void OnAppearing()
    {
        base.OnAppearing();
        pageIsActive = true;
        stopwatch.Start();

        Device.StartTimer(TimeSpan.FromMilliseconds(33), () =>
        {
            t = (float)(stopwatch.Elapsed.TotalMilliseconds % cycleTime / cycleTime);
            canvasView.InvalidateSurface();

            if (!pageIsActive)
            {
                stopwatch.Stop();
            }
            return pageIsActive;
        });
    }

    protected override void OnDisappearing()
    {
        base.OnDisappearing();
        pageIsActive = false;
    }
    ...
}
```

`PaintSurface` Обработчик рисует пять концентрических окружностей с анимированных радиусы. Если `baseRadius` переменная вычисляется как 100, затем как `t` анимируется от 0 до 1, радиусы пять круги увеличивать нагрузку между 0 100, 100 – 200, 200 – 300, 300 до 400 и 400 до 500. Для большинства кругов `strokeWidth` — 50, но для первого круг, `strokeWidth` выполняет анимацию от 0 до 50. Для большинства кругов синий цвет, но для последнего окружности, цвет анимируется от синего к прозрачности. Обратите внимание, что четвертый аргумент `SKColor` конструктор, который определяет непрозрачность:

```csharp
public class ExpandingCirclesPage : ContentPage
{
    ...
    void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
    {
        SKImageInfo info = args.Info;
        SKSurface surface = args.Surface;
        SKCanvas canvas = surface.Canvas;

        canvas.Clear();

        SKPoint center = new SKPoint(info.Width / 2, info.Height / 2);
        float baseRadius = Math.Min(info.Width, info.Height) / 12;

        for (int circle = 0; circle < 5; circle++)
        {
            float radius = baseRadius * (circle + t);

            paint.StrokeWidth = baseRadius / 2 * (circle == 0 ? t : 1);
            paint.Color = new SKColor(0, 0, 255,
                (byte)(255 * (circle == 4 ? (1 - t) : 1)));

            canvas.DrawCircle(center.X, center.Y, radius, paint);
        }
    }
}
```

Результатом является то, что образ, выглядит так же when `t` равен 0, как и при `t` равно 1, и круги, по-видимому, поочередно раскройте остальные навсегда:

[![](animation-images/expandingcircles-small.png "Тройной снимок экрана страницы круги, развернув")](animation-images/expandingcircles-large.png#lightbox "тройной снимок экрана страницы кругов развертывания")


## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [SkiaSharpFormsDemos (пример)](https://developer.xamarin.com/samples/xamarin-forms/SkiaSharpForms/Demos/)
