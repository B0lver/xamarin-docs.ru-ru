---
Title: "Line and Stroke Cap" Description: "в этой статье объясняется, как использовать SkiaSharp для рисования линий с различными наконечниками штриха в Xamarin.Forms приложениях и демонстрируется в примере кода".
MS. произв. Xamarin MS. AssetID: 1F854DDD-5D1B-4DE4-BD2D-584439429FDB MS. Technology: Xamarin-skiasharp Автор: давидбритч MS. author: дабритч МС. Дата: 03/10/2017 No-Loc: [ Xamarin.Forms , Xamarin.Essentials ]
---

# <a name="lines-and-stroke-caps"></a>Линии и концы штрихов

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)

_Сведения об использовании SkiaSharp для рисования линий с различными наконечниками штриха_

В SkiaSharp отрисовка одной строки сильно отличается от визуализации ряда Соединенных прямых линий. Однако даже при рисовании отдельных линий часто приходится присваивать линиям определенную толщину штрихов. Так как эти строки становятся шире, внешний вид концов линий также становится очень важным. Внешний вид конца линии называется *наконечником обводки*.

![](lines-images/strokecapsexample.png "The three stroke caps options")

Для рисования отдельных строк `SKCanvas` определяет простой метод, [`DrawLine`](xref:SkiaSharp.SKCanvas.DrawLine(System.Single,System.Single,System.Single,System.Single,SkiaSharp.SKPaint)) аргументы которого указывают начальную и конечную координаты строки с `SKPaint` объектом:

```csharp
canvas.DrawLine (x0, y0, x1, y1, paint);
```

По умолчанию [`StrokeWidth`](xref:SkiaSharp.SKPaint.StrokeWidth) свойство только что созданного `SKPaint` объекта имеет значение 0, что аналогично значению 1 в отрисовке линии с одним пикселем в толщине. Это выглядит очень тонко на устройствах с высоким разрешением, например на телефонах, поэтому вам, вероятно, потребуется задать `StrokeWidth` большее значение. Но после начала рисования линий с изменяемой толщиной возникает другая ошибка: как должны начинаться и завершаться эти толстые линии?

Внешний вид линий начала и конца линии называется *концом строки* или, в СКИА, *наконечником обводки*. Слово «Cap» в этом контексте относится к типу Hat &mdash; , который находится в конце строки. [`StrokeCap`](xref:SkiaSharp.SKPaint.StrokeCap)Свойству объекта задается `SKPaint` один из следующих членов [`SKStrokeCap`](xref:SkiaSharp.SKStrokeCap) перечисления:

- `Butt`(по умолчанию)
- `Square`
- `Round`

Они лучше проиллюстрированы с помощью примера программы. Раздел **SkiaSharp Lines and paths** программы [**скиашарпформсдемос**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos) начинается со страницы, наименованной **прописные буквы** на основе [`StrokeCapsPage`](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/StrokeCapsPage.cs) класса. На этой странице определяется `PaintSurface` обработчик событий, который циклически проходит по трем элементам `SKStrokeCap` перечисления, отображая как имя члена перечисления, так и рисование линии с помощью этого конца штриха:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    SKPaint textPaint = new SKPaint
    {
        Color = SKColors.Black,
        TextSize = 75,
        TextAlign = SKTextAlign.Center
    };

    SKPaint thickLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Orange,
        StrokeWidth = 50
    };

    SKPaint thinLinePaint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.Black,
        StrokeWidth = 2
    };

    float xText = info.Width / 2;
    float xLine1 = 100;
    float xLine2 = info.Width - xLine1;
    float y = textPaint.FontSpacing;

    foreach (SKStrokeCap strokeCap in Enum.GetValues(typeof(SKStrokeCap)))
    {
        // Display text
        canvas.DrawText(strokeCap.ToString(), xText, y, textPaint);
        y += textPaint.FontSpacing;

        // Display thick line
        thickLinePaint.StrokeCap = strokeCap;
        canvas.DrawLine(xLine1, y, xLine2, y, thickLinePaint);

        // Display thin line
        canvas.DrawLine(xLine1, y, xLine2, y, thinLinePaint);
        y += 2 * textPaint.FontSpacing;
    }
}
```

Для каждого члена `SKStrokeCap` перечисления обработчик рисует две строки: одну с толщиной штриха 50 пикселей, а другую — с толщиной обводки, равной двум пикселям. Вторая строка предназначена для иллюстрации геометрического начала и конца строки независимо от толщины линии и конца штриха.

[![](lines-images/strokecaps-small.png "Triple screenshot of the Stroke Caps page")](lines-images/strokecaps-large.png#lightbox "Triple screenshot of the Stroke Caps page")

Как видите, знаки и линии `Square` `Round` обводки фактически расширяют длину строки на половину ширины штриха в начале строки и снова в конце. Это расширение имеет важное значение, когда необходимо определить размеры визуализированного объекта Graphics.

`SKCanvas`Класс также содержит еще один метод для рисования нескольких строк, которые несколько довольно необычная:

```csharp
DrawPoints (SKPointMode mode, points, paint)
```

`points`Параметр является массивом `SKPoint` значений и `mode` является членом [`SKPointMode`](xref:SkiaSharp.SKPointMode) перечисления, который имеет три члена:

- `Points`отрисовка отдельных точек
- `Lines`соединение каждой пары точек
- `Polygon`подключение всех последовательных точек

Этот метод показан на странице с **несколькими строками** . Файл [**мултиплелинеспаже. XAML**](https://github.com/xamarin/xamarin-forms-samples/blob/master/SkiaSharpForms/Demos/Demos/SkiaSharpFormsDemos/Paths/MultipleLinesPage.xaml) создает два `Picker` представления, которые позволяют выбрать член `SKPointMode` перечисления и член `SKStrokeCap` перечисления:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:skia="clr-namespace:SkiaSharp;assembly=SkiaSharp"
             xmlns:skiaforms="clr-namespace:SkiaSharp.Views.Forms;assembly=SkiaSharp.Views.Forms"
             x:Class="SkiaSharpFormsDemos.Paths.MultipleLinesPage"
             Title="Multiple Lines">
    <Grid>
        <Grid.ColumnDefinitions>
            <ColumnDefinition Width="*" />
            <ColumnDefinition Width="*" />
        </Grid.ColumnDefinitions>

        <Grid.RowDefinitions>
            <RowDefinition Height="Auto" />
            <RowDefinition Height="*" />
        </Grid.RowDefinitions>

        <Picker x:Name="pointModePicker"
                Title="Point Mode"
                Grid.Row="0"
                Grid.Column="0"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKPointMode}">
                    <x:Static Member="skia:SKPointMode.Points" />
                    <x:Static Member="skia:SKPointMode.Lines" />
                    <x:Static Member="skia:SKPointMode.Polygon" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <Picker x:Name="strokeCapPicker"
                Title="Stroke Cap"
                Grid.Row="0"
                Grid.Column="1"
                SelectedIndexChanged="OnPickerSelectedIndexChanged">
            <Picker.ItemsSource>
                <x:Array Type="{x:Type skia:SKStrokeCap}">
                    <x:Static Member="skia:SKStrokeCap.Butt" />
                    <x:Static Member="skia:SKStrokeCap.Round" />
                    <x:Static Member="skia:SKStrokeCap.Square" />
                </x:Array>
            </Picker.ItemsSource>
            <Picker.SelectedIndex>
                0
            </Picker.SelectedIndex>
        </Picker>

        <skiaforms:SKCanvasView x:Name="canvasView"
                                PaintSurface="OnCanvasViewPaintSurface"
                                Grid.Row="1"
                                Grid.Column="0"
                                Grid.ColumnSpan="2" />
    </Grid>
</ContentPage>
```

Обратите внимание, что объявления пространства имен SkiaSharp немного отличаются, так как `SkiaSharp` пространство имен требуется для ссылки на члены `SKPointMode` `SKStrokeCap` перечислений и. `SelectedIndexChanged`Обработчик для обоих `Picker` представлений просто делает недействительным `SKCanvasView` объект:

```csharp
void OnPickerSelectedIndexChanged(object sender, EventArgs args)
{
    if (canvasView != null)
    {
        canvasView.InvalidateSurface();
    }
}
```

Этот обработчик должен проверить наличие `SKCanvasView` объекта, так как обработчик события сначала вызывается `SelectedIndex` , когда свойство объекта `Picker` имеет значение 0 в файле XAML и происходит до `SKCanvasView` создания экземпляра.

`PaintSurface`Обработчик получает два значения перечисления из `Picker` представлений:

```csharp
void OnCanvasViewPaintSurface(object sender, SKPaintSurfaceEventArgs args)
{
    SKImageInfo info = args.Info;
    SKSurface surface = args.Surface;
    SKCanvas canvas = surface.Canvas;

    canvas.Clear();

    // Create an array of points scattered through the page
    SKPoint[] points = new SKPoint[10];

    for (int i = 0; i < 2; i++)
    {
        float x = (0.1f + 0.8f * i) * info.Width;

        for (int j = 0; j < 5; j++)
        {
            float y = (0.1f + 0.2f * j) * info.Height;
            points[2 * j + i] = new SKPoint(x, y);
        }
    }

    SKPaint paint = new SKPaint
    {
        Style = SKPaintStyle.Stroke,
        Color = SKColors.DarkOrchid,
        StrokeWidth = 50,
        StrokeCap = (SKStrokeCap)strokeCapPicker.SelectedItem
    };

    // Render the points by calling DrawPoints
    SKPointMode pointMode = (SKPointMode)pointModePicker.SelectedItem;
    canvas.DrawPoints(pointMode, points, paint);
}
```

Снимки экрана показывают разнообразные `Picker` варианты выбора:

[![](lines-images/multiplelines-small.png "Triple screenshot of the Multiple Lines page")](lines-images/multiplelines-large.png#lightbox "Triple screenshot of the Multiple Lines page")

На iPhone слева показано, как `SKPointMode.Points` элемент перечисления вызывает `DrawPoints` визуализацию каждой точки в `SKPoint` массиве в виде квадрата, если конец строки имеет значение `Butt` или `Square` . Круги подготавливаются к просмотру, если конец строки имеет значение `Round` .

На снимке экрана Android показан результат `SKPointMode.Lines` . `DrawPoints`Метод рисует линию между каждой парой `SKPoint` значений, используя заданное окончание строки в данном случае `Round` .

Если вместо этого используется `SKPointMode.Polygon` , линия рисуется между последовательными точками в массиве, но если смотреть очень близко, вы увидите, что эти строки не подключены. Каждая из этих отдельных строк начинается и заканчивается заданным концом строки. Если выбрать `Round` ограничения, строки могут быть подключены, но они действительно не подключены.

Наличие подключенных или неподключенных линий является важнейшим аспектом работы с графическими путями.

## <a name="related-links"></a>Связанные ссылки

- [API-интерфейсы SkiaSharp](https://docs.microsoft.com/dotnet/api/skiasharp)
- [Скиашарпформсдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/skiasharpforms-demos)
