---
title: Тексткит в Xamarin. iOS
description: В этом документе описывается, как использовать Тексткит в Xamarin. iOS. Тексткит предоставляет мощные функции верстки и отрисовки текста.
ms.prod: xamarin
ms.assetid: 1D0477E8-CD1E-48A9-B7C8-7CA892069EFF
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 4d4785d6e556c856b0f7b4db2accd87f5297e277
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68655371"
---
# <a name="textkit-in-xamarinios"></a>Тексткит в Xamarin. iOS

Тексткит — это новый интерфейс API, предлагающий мощные функции верстки и отрисовки текста. Она построена на основе основной платформы текста нижнего уровня, но гораздо проще в использовании, чем основной текст.

Чтобы сделать функции Тексткит доступными для стандартных элементов управления, несколько текстовых элементов управления iOS были повторно реализованы для использования Тексткит, включая:

-  UITextView
-  UITextField
-  UILabel

## <a name="architecture"></a>Архитектура

Тексткит предоставляет многоуровневую архитектуру, которая отделяет хранилище текста от макета и экрана, включая следующие классы:

-  `NSTextContainer`— Предоставляет систему координат и геометрию, используемую для разметки текста.
-  `NSLayoutManager`— Размещает текст, преобразуя текст в глифы. 
-  `NSTextStorage`— Содержит текстовые данные, а также обрабатывает обновления свойств текста пакета. Все обновления пакетов передаются диспетчеру макета для фактической обработки изменений, таких как пересчет макета и перерисовка текста.


Эти три класса применяются к представлению, которое визуализирует текст. Встроенные представления обработки текста, такие `UITextView`как, `UITextField`, и `UILabel` уже установили их, но их можно также создавать и применять к любому `UIView` экземпляру.

Эта архитектура показана на следующем рисунке:

 ![](textkit-images/textkitarch.png "На этом рисунке показана архитектура Тексткит")

## <a name="text-storage-and-attributes"></a>Хранение текста и атрибуты

`NSTextStorage` Класс содержит текст, отображаемый представлением. Он также передает любые изменения текста, такие как изменения символов или их атрибуты, в Диспетчер макетов для вывода. `NSTextStorage`наследует от `MSMutableAttributed` строки, позволяя изменять атрибуты текста в пакетах между `BeginEditing` вызовами и `EndEditing` .

Например, в следующем фрагменте кода задается изменение цвета переднего плана и фона соответственно, а также задаются определенные диапазоны:

```csharp
textView.TextStorage.BeginEditing ();
textView.TextStorage.AddAttribute(UIStringAttributeKey.ForegroundColor, UIColor.Green, new NSRange(200, 400));
textView.TextStorage.AddAttribute(UIStringAttributeKey.BackgroundColor, UIColor.Black, new NSRange(210, 300));
textView.TextStorage.EndEditing ();
```

После `EndEditing` вызова изменения отправляются диспетчеру макетов, который, в свою очередь, выполняет все необходимые вычисления макета и отрисовки для текста, отображаемого в представлении.

## <a name="layout-with-exclusion-path"></a>Макет с путем исключения

Тексткит также поддерживает макет и позволяет выполнять сложные сценарии, такие как Многостолбцовые тексты, и перетекание текста по указанным путям, называемым *путями исключения*. К контейнеру текста применяются пути исключения, которые изменяют геометрию макета текста, в результате чего текст обтекает указанные пути.

Для добавления пути исключения необходимо задать `ExclusionPaths` свойство в диспетчере макетов. Установка этого свойства приводит к тому, что диспетчер разметки сделает макет текста недействительным и пополняет текст вокруг пути исключения.

### <a name="exclusion-based-on-a-cgpath"></a>Исключение, основанное на Кгпас

Рассмотрим следующую `UITextView` реализацию подкласса:

```csharp
public class ExclusionPathView : UITextView
{
    CGPath exclusionPath;
    CGPoint initialPoint;
    CGPoint latestPoint;
    UIBezierPath bezierPath;

    public ExclusionPathView (string text)
    {
        Text = text;
        ContentInset = new UIEdgeInsets (20, 0, 0, 0);
        BackgroundColor = UIColor.White;
        exclusionPath = new CGPath ();
        bezierPath = UIBezierPath.Create ();

        LayoutManager.AllowsNonContiguousLayout = false;
    }

    public override void TouchesBegan (NSSet touches, UIEvent evt)
    {
        base.TouchesBegan (touches, evt);

        var touch = touches.AnyObject as UITouch;

        if (touch != null) {
            initialPoint = touch.LocationInView (this);
        }
    }

    public override void TouchesMoved (NSSet touches, UIEvent evt)
    {
        base.TouchesMoved (touches, evt);

        UITouch touch = touches.AnyObject as UITouch;

        if (touch != null) {
            latestPoint = touch.LocationInView (this);
            SetNeedsDisplay ();
        }
    }

    public override void TouchesEnded (NSSet touches, UIEvent evt)
    {
        base.TouchesEnded (touches, evt);

        bezierPath.CGPath = exclusionPath;
        TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
    }

    public override void Draw (CGRect rect)
    {
        base.Draw (rect);

        if (!initialPoint.IsEmpty) {

            using (var g = UIGraphics.GetCurrentContext ()) {

                g.SetLineWidth (4);
                UIColor.Blue.SetStroke ();

                if (exclusionPath.IsEmpty) {
                    exclusionPath.AddLines (new CGPoint[] { initialPoint, latestPoint });
                } else {
                    exclusionPath.AddLineToPoint (latestPoint);
                }

                g.AddPath (exclusionPath);
                g.DrawPath (CGPathDrawingMode.Stroke);
            }
        }
    }
}
```

Этот код добавляет поддержку рисования в текстовом представлении с помощью основных графических объектов. Поскольку теперь `UITextView` класс создан для использования тексткит для отрисовки и компоновки текста, он может использовать все функции тексткит, такие как Настройка путей исключения.

> [!IMPORTANT]
> `UITextView` В этом примере подклассы добавляют поддержку рисования касаний. Для получения функций `UITextView` тексткит не нужно подклассировать.



После того, как пользователь выводит текст в текстовом представлении `CGPath` , нарисованный `UIBezierPath` объект применяется к экземпляру путем задания `UIBezierPath.CGPath` свойства:

```csharp
bezierPath.CGPath = exclusionPath;
```

Обновление следующей строки кода приводит к обновлению макета текста по пути:

```csharp
TextContainer.ExclusionPaths = new UIBezierPath[] { bezierPath };
```

На следующем снимке экрана показано, как изменяется макет текста, чтобы обтекать рисуемый контур.

<!-- ![](textkit-images/exclusionpath1.png "This screenshot illustrates how the text layout changes to flow around the drawn path")--> 
![](textkit-images/exclusionpath2.png "На этом снимке экрана показано, как изменяется макет текста, чтобы обтекать рисуемый контур.")

Обратите внимание, что в `AllowsNonContiguousLayout` этом случае свойство диспетчера макета имеет значение false. Это приводит к повторному вычислению макета для всех случаев, когда текст изменяется. Установка значения true может повысить производительность, избегая полного обновления макета, особенно в случае больших документов. Однако при установке `AllowsNonContiguousLayout` значения true путь исключения не будет обновлять макет в некоторых обстоятельствах, например, если текст записывается во время выполнения без завершающего возврата каретки до установки пути.


## <a name="related-links"></a>Связанные ссылки

- [Введение в iOS 7 (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/introtoios7)
- [Обзор пользовательского интерфейса iOS 7](~/ios/platform/introduction-to-ios7/ios7-ui.md)
- [Фоновая обработка](~/ios/app-fundamentals/backgrounding/index.md)
