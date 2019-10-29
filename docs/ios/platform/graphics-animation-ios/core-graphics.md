---
title: Основные графические объекты в Xamarin. iOS
description: В этой статье обсуждаются основные платформы графики iOS. В нем показано, как использовать основные графические объекты для рисования геометрии, изображений и PDF-файлов.
ms.prod: xamarin
ms.assetid: 4A30F480-0723-4B8A-9049-7CEB6211304A
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 76901a5c48caef666d18f5cc7e2bfd8b28096184
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032453"
---
# <a name="core-graphics-in-xamarinios"></a>Основные графические объекты в Xamarin. iOS

_В этой статье обсуждаются основные платформы графики iOS. В нем показано, как использовать основные графические объекты для рисования геометрии, изображений и PDF-файлов._

iOS включает в себя [*основную графическую*](https://developer.apple.com/library/prerelease/ios/documentation/CoreGraphics/Reference/CoreGraphics_Framework/index.html) платформу для обеспечения поддержки отрисовки низкого уровня. Эти платформы обеспечивают широкие возможности работы с графическими возможностями в UIKit.

Основная графика — Это высокоуровневая платформа двухмерной графики, которая позволяет рисовать независимым от устройства графику. Весь 2D-рисунок в UIKit использует основные графические изображения.

Основная графика поддерживает рисование в ряде сценариев, в том числе:

- [Рисование на экране с помощью `UIView`](#Drawing_in_a_UIView_Subclass) .
- [Рисование изображений в памяти или на экране](#Drawing_Images_and_Text).
- Создание и рисование в PDF.
- Чтение и Рисование существующего PDF-документа.

## <a name="geometric-space"></a>Геометрическое пространство

Независимо от сценария Вся прорисовка, выполненная с помощью основной графики, выполняется в геометрических пространствах, то есть она работает в абстрактных точках, а не в пикселях. Вы описываете, что нужно рисовать с точки зрения геометрии и состояния рисования, такие как цвета, стили линий и т. д., а также основные графические дескрипторы, посвященные преобразованию всех точек в пиксели. Такое состояние добавляется в графический контекст, который можно представить себе как холст по образцу.

Существует несколько преимуществ этого подхода:

- Код прорисовки превращается в динамический и может впоследствии изменить графические объекты во время выполнения.
- Уменьшение потребности в статических образах в пакете приложений может привести к уменьшению размера приложения.
- Графика становится более устойчивой к изменениям разрешения на разных устройствах.

<a name="Drawing_in_a_UIView_Subclass"/>

## <a name="drawing-in-a-uiview-subclass"></a>Рисование в подклассе UIView

Каждый `UIView` имеет метод `Draw`, который вызывается системой при необходимости его прорисовки. Чтобы добавить код рисования в представление, подкласс `UIView` и переопределите `Draw`:

```csharp
public class TriangleView : UIView
{
    public override void Draw (CGRect rect)
    {
        base.Draw (rect);
    }
}
```

Прорисовку никогда не следует вызывать напрямую. Он вызывается системой во время обработки цикла выполнения. В первый раз через цикл выполнения после добавления представления в иерархию представлений вызывается метод `Draw`. Последующие вызовы `Draw` происходят, когда представление помечено как требующее прорисовку путем вызова метода `SetNeedsDisplay` или `SetNeedsDisplayInRect` представления.

### <a name="pattern-for-graphics-code"></a>Шаблон для графического кода

Код в реализации `Draw` должен описывать, что он хочет рисовать. Код рисования соответствует шаблону, в котором задается определенное состояние рисования, и вызывается метод для запроса на его прорисовку. Этот шаблон можно обобщить следующим образом:

1. Получение графического контекста.

2. Настройка атрибутов рисования.

3. Создайте геометрию из примитивов рисования.

4. Вызовите метод Draw или Stroke.

### <a name="basic-drawing-example"></a>Пример простого рисования

Например, рассмотрим следующий фрагмент кода:

```csharp
//get graphics context
using (CGContext g = UIGraphics.GetCurrentContext ()) {

    //set up drawing attributes
    g.SetLineWidth (10);
    UIColor.Blue.SetFill ();
    UIColor.Red.SetStroke ();

    //create geometry
    var path = new CGPath ();

    path.AddLines (new CGPoint[]{
    new CGPoint (100, 200),
    new CGPoint (160, 100),
    new CGPoint (220, 200)});

    path.CloseSubpath ();

    //add geometry to graphics context and draw it
    g.AddPath (path);
    g.DrawPath (CGPathDrawingMode.FillStroke);
}
```

Давайте развернемся к этому коду:

```csharp
using (CGContext g = UIGraphics.GetCurrentContext ()) {
...
}
```

В этой строке сначала получает текущий графический контекст, используемый для рисования. Графический контекст можно представить себе как холст, на котором происходит рисование, который содержит все сведения о изображении, например цвета обводки и заливки, а также геометрию для рисования.

```csharp
g.SetLineWidth (10);
UIColor.Blue.SetFill ();
UIColor.Red.SetStroke ();
```

После получения графического контекста код настраивает некоторые атрибуты для использования при прорисовке, показанном выше. В этом случае задаются цвета толщина линии, штриха и заливки. Затем все последующие рисунки будут использовать эти атрибуты, так как они сохраняются в состоянии графического контекста.

Для создания геометрии код использует `CGPath`, который позволяет описывать графические пути из линий и кривых. В этом случае путь добавляет линии, соединяющие массив точек для создания треугольника. Как показано ниже, основные графические объекты используют систему координат для рисования представления, где начало координат находится в левом верхнем углу с положительным крестиком справа и положительным направлением по оси x:

```csharp
var path = new CGPath ();

path.AddLines (new CGPoint[]{
new CGPoint (100, 200),
new CGPoint (160, 100),
new CGPoint (220, 200)});

path.CloseSubpath ();
```

После создания пути он добавляется в графический контекст, чтобы вызывающий `AddPath` и `DrawPath`, соответственно, могли их рисовать.

Полученное представление показано ниже:

 ![](core-graphics-images/00-bluetriangle.png "The sample output triangle")

## <a name="creating-gradient-fills"></a>Создание градиентных заливок

Также доступны более широкие формы рисования. Например, основная графика позволяет создавать градиентные заливки и применять обтравочные контуры. Чтобы нарисовать градиентную заливку внутри пути из предыдущего примера, сначала необходимо задать путь для обрезки:

```csharp
// add the path back to the graphics context so that it is the current path
g.AddPath (path);
// set the current path to be the clipping path
g.Clip ();
```

Установка текущего пути в качестве обтравочного пути ограничивает весь последующий рисунок в пределах геометрической траектории, например следующий код, который рисует линейный градиент:

```csharp
// the color space determines how Core Graphics interprets color information
    using (CGColorSpace rgb = CGColorSpace.CreateDeviceRGB()) {
        CGGradient gradient = new CGGradient (rgb, new CGColor[] {
        UIColor.Blue.CGColor,
        UIColor.Yellow.CGColor
    });

// draw a linear gradient
    g.DrawLinearGradient (
        gradient,
        new CGPoint (path.BoundingBox.Left, path.BoundingBox.Top),
        new CGPoint (path.BoundingBox.Right, path.BoundingBox.Bottom),
        CGGradientDrawingOptions.DrawsBeforeStartLocation);
    }
```

Эти изменения приводят к градиентной заливки, как показано ниже:

 ![](core-graphics-images/01-gradient-fill.png "The example with a gradient fill")

## <a name="modifying-line-patterns"></a>Изменение шаблонов линий

Атрибуты рисования линий можно также изменять с помощью основных графических объектов. Это включает изменение толщины и цвета линии, а также сам шаблон линии, как показано в следующем коде:

```csharp
//use a dashed line
g.SetLineDash (0, new nfloat[] { 10, 4 * (nfloat)Math.PI });
```

Добавление этого кода перед любыми операциями рисования приводит к отображению штриховых штрихов в 10 единиц, с 4 единицами расстояния между тире, как показано ниже:

 ![](core-graphics-images/02-dashed-stroke.png "Adding this code before any drawing operations results in dashed strokes")

Обратите внимание, что при использовании Unified API в Xamarin. iOS тип массива должен быть `nfloat`, а также должен быть явно приведен к Math. PI.

<a name="Drawing_Images_and_Text"/>

## <a name="drawing-images-and-text"></a>Рисование изображений и текста

Помимо контуров рисования в графическом контексте представления, основные графические объекты также поддерживают рисование изображений и текста. Чтобы нарисовать изображение, просто создайте `CGImage` и передайте его в вызов `DrawImage`:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using(CGContext g = UIGraphics.GetCurrentContext ()){
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
    }
}
```

Однако при этом создается изображение, отображаемое сверху вниз, как показано ниже:

 ![](core-graphics-images/03-upside-down-monkey.png "An image drawn upside down")

Причина этого — основной источник графики для рисования изображений находится в левом нижнем углу, а представление — в левом верхнем углу. Таким образом, чтобы правильно отобразить изображение, необходимо изменить источник, что можно сделать, изменив *текущую матрицу преобразования* *(CTM для)* . CTM для определяет, где находятся точки в реальном времени, также называемые *пространством пользователя*. Инвертирование CTM для в направлении y и сдвиг его на высоту границ в отрицательном направлении y может поразить изображение.

Графический контекст содержит вспомогательные методы для преобразования CTM для. В этом случае `ScaleCTM` "переворачивает" рисунок и `TranslateCTM` смещает его в левый верхний угол, как показано ниже:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    using (CGContext g = UIGraphics.GetCurrentContext ()) {

        // scale and translate the CTM so the image appears upright
        g.ScaleCTM (1, -1);
        g.TranslateCTM (0, -Bounds.Height);
        g.DrawImage (rect, UIImage.FromFile ("MyImage.png").CGImage);
}
```

Полученное изображение будет отображаться вертикально:

 ![](core-graphics-images/04-upright-monkey.png "The sample image displayed upright")

> [!IMPORTANT]
> Изменения в графическом контексте применяются ко всем последующим операциям рисования. Таким образом, при преобразовании CTM для оно повлияет на любую дополнительную прорисовку. Например, если вы нанарисовали треугольник после преобразования «CTM для», он будет выглядеть так, как показано выше.

### <a name="adding-text-to-the-image"></a>Добавление текста к изображению

Как и в случае с путями и изображениями, при рисовании текста с помощью основной графики используется тот же базовый шаблон, что и для настройки определенного состояния графики и вызова метода для рисования. В случае текста метод для вывода текста `ShowText`. При добавлении в пример рисования изображения текст с помощью основных графических объектов рисуется следующим кодом:

```csharp
public override void Draw (RectangleF rect)
{
    base.Draw (rect);

    // image drawing code omitted for brevity ...

    // translate the CTM by the font size so it displays on screen
    float fontSize = 35f;
    g.TranslateCTM (0, fontSize);

    // set general-purpose graphics state
    g.SetLineWidth (1.0f);
    g.SetStrokeColor (UIColor.Yellow.CGColor);
    g.SetFillColor (UIColor.Red.CGColor);
    g.SetShadow (new CGSize (5, 5), 0, UIColor.Blue.CGColor);

    // set text specific graphics state
    g.SetTextDrawingMode (CGTextDrawingMode.FillStroke);
    g.SelectFont ("Helvetica", fontSize, CGTextEncoding.MacRoman);

    // show the text
    g.ShowText ("Hello Core Graphics");
}
```

Как видите, задание состояния графики для рисования текста похоже на рисование геометрии. Однако для рисования текста также применяются режим рисования текста и шрифт. В этом случае также применяется тень, хотя применение теней для рисования контура выполняется одинаково.

Полученный текст отображается вместе с изображением, как показано ниже:

 ![](core-graphics-images/05-text-on-image.png "The resulting text is displayed with the image")

## <a name="memory-backed-images"></a>Образы, поддерживающие память

В дополнение к рисованию в графическом контексте представления, основная графика поддерживает изображения, поддерживающие память, также называемые изображением с экрана. Для этого требуется:

- Создание графического контекста, который поддерживается точечным рисунком в памяти
- Установка и выдача команд для рисования состояния рисования
- Получение образа из контекста
- Удаление контекста

В отличие от метода `Draw`, в котором контекст предоставляется представлением, в данном случае контекст создается одним из двух способов:

1. Путем вызова `UIGraphics.BeginImageContext` (или `BeginImageContextWithOptions`)

2. Путем создания нового `CGBitmapContextInstance`

 `CGBitmapContextInstance` удобно использовать при работе с битами изображения напрямую, например в случаях, когда используется пользовательский алгоритм обработки изображений. Во всех остальных случаях следует использовать `BeginImageContext` или `BeginImageContextWithOptions`.

После создания контекста образа Добавление кода рисования выглядит так же, как в подклассе `UIView`. Например, пример кода, который использовался ранее для рисования треугольника, можно использовать для рисования изображения в памяти, а не в `UIView`, как показано ниже:

```csharp
UIImage DrawTriangle ()
{
    UIImage triangleImage;

    //push a memory backed bitmap context on the context stack
    UIGraphics.BeginImageContext (new CGSize (200.0f, 200.0f));

    //get graphics context
    using(CGContext g = UIGraphics.GetCurrentContext ()){

        //set up drawing attributes
        g.SetLineWidth(4);
        UIColor.Purple.SetFill ();
        UIColor.Black.SetStroke ();

        //create geometry
        path = new CGPath ();

        path.AddLines(new CGPoint[]{
            new CGPoint(100,200),
            new CGPoint(160,100),
            new CGPoint(220,200)});

        path.CloseSubpath();

        //add geometry to graphics context and draw it
        g.AddPath(path);
        g.DrawPath(CGPathDrawingMode.FillStroke);

        //get a UIImage from the context
        triangleImage = UIGraphics.GetImageFromCurrentImageContext ();
    }

    return triangleImage;
}
```

Обычно рисование в точечный рисунок с памятью используется для записи изображения из любого `UIView`. Например, следующий код подготавливает слой представления к контексту битовой карты и создает из него `UIImage`:

```csharp
UIGraphics.BeginImageContext (cellView.Frame.Size);

//render the view's layer in the current context
anyView.Layer.RenderInContext (UIGraphics.GetCurrentContext ());

//get a UIImage from the context
UIImage anyViewImage = UIGraphics.GetImageFromCurrentImageContext ();
UIGraphics.EndImageContext ();
```

## <a name="drawing-pdfs"></a>Рисование документов PDF

Помимо образов, основная графика поддерживает рисование в формате PDF. Как и изображения, можно визуализировать PDF в памяти, а также читать PDF для подготовки к просмотру в `UIView`.

### <a name="pdf-in-a-uiview"></a>PDF в UIView

Основная графика также поддерживает чтение документа PDF из файла и отображение его в представлении с помощью класса `CGPDFDocument`. Класс `CGPDFDocument` представляет PDF в коде и может использоваться для чтения и рисования страниц.

Например, следующий код в подклассе `UIView` считывает PDF-файл из файла в `CGPDFDocument`:

```csharp
public class PDFView : UIView
{
    CGPDFDocument pdfDoc;

    public PDFView ()
    {
        //create a CGPDFDocument from file.pdf included in the main bundle
        pdfDoc = CGPDFDocument.FromFile ("file.pdf");
    }

     public override void Draw (Rectangle rect)
    {
        ...
    }
}
```

Затем метод `Draw` может использовать `CGPDFDocument` для чтения страницы в `CGPDFPage` и ее визуализации путем вызова `DrawPDFPage`, как показано ниже:

```csharp
public override void Draw (CGRect rect)
{
    base.Draw (rect);

    //flip the CTM so the PDF will be drawn upright
    using (CGContext g = UIGraphics.GetCurrentContext ()) {
        g.TranslateCTM (0, Bounds.Height);
        g.ScaleCTM (1, -1);

        // render the first page of the PDF
        using (CGPDFPage pdfPage = pdfDoc.GetPage (1)) {

        //get the affine transform that defines where the PDF is drawn
        CGAffineTransform t = pdfPage.GetDrawingTransform (CGPDFBox.Crop, rect, 0, true);

        //concatenate the pdf transform with the CTM for display in the view
        g.ConcatCTM (t);

        //draw the pdf page
        g.DrawPDFPage (pdfPage);
        }
    }
}
```

### <a name="memory-backed-pdf"></a>PDF с поддержкой памяти

Для PDF-файла в памяти необходимо создать контекст PDF, вызвав `BeginPDFContext`. Рисование в PDF детализировано по страницам. Каждая страница запускается путем вызова `BeginPDFPage` и завершается вызовом `EndPDFContent`с графическим кодом. Кроме того, как и при рисовании изображений, для рисования в памяти в формате PDF используется источник в левом нижнем углу, который может быть основан на изменении CTM для, как и на изображениях.

В следующем коде показано, как нарисовать текст в PDF:

```csharp
//data buffer to hold the PDF
NSMutableData data = new NSMutableData ();

//create a PDF with empty rectangle, which will configure it for 8.5x11 inches
UIGraphics.BeginPDFContext (data, CGRect.Empty, null);

//start a PDF page
UIGraphics.BeginPDFPage ();

using (CGContext g = UIGraphics.GetCurrentContext ()) {
    g.ScaleCTM (1, -1);
    g.TranslateCTM (0, -25);
    g.SelectFont ("Helvetica", 25, CGTextEncoding.MacRoman);
    g.ShowText ("Hello Core Graphics");
    }

//complete a PDF page
UIGraphics.EndPDFContent ();
```

Полученный текст отображается в формате PDF, который затем содержится в `NSData`, который можно сохранить, отправить, отправить по электронной почте и т. д.

## <a name="summary"></a>Сводка

В этой статье мы рассмотрели возможности графики, предоставляемые с помощью *основной графической* платформы. Мы увидели, как использовать основные графические объекты для рисования геометрии, изображений и PDF в контексте `UIView,`, а также для контекстов графики, поддерживающих память.

## <a name="related-links"></a>Связанные ссылки

- [Образец графики Core](https://docs.microsoft.com/samples/xamarin/ios-samples/graphicsandanimation)
- [Пошаговое руководство по графике и анимации](~/ios/platform/graphics-animation-ios/graphics-animation-walkthrough.md)
- [Core Animation](~/ios/platform/graphics-animation-ios/core-animation.md)
- [Основные рецепты анимации](https://github.com/xamarin/recipes/tree/master/Recipes/ios/animation/coreanimation)
