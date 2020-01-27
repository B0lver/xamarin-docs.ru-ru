---
title: Пошаговое руководство. Использование сенсорного ввода в Xamarin. iOS
description: В этом документе описывается, как управлять касанием в приложениях Xamarin. iOS, обсуждая примеры взаимодействия касания, распознаватели жестов и пользовательские распознаватели жестов.
ms.prod: xamarin
ms.assetid: 13F8289B-7A80-4959-AF3F-57874D866DCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: 16e35d67f8fce33c8a0b21ddcd07df14fedf179b
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725207"
---
# <a name="walkthrough-using-touch-in-xamarinios"></a>Пошаговое руководство. Использование сенсорного ввода в Xamarin. iOS

В этом пошаговом руководстве показано, как написать код, реагирующий на различные виды событий касания. Каждый пример содержится на отдельном экране:

- [Примеры касаний](#Touch_Samples) — реагирование на события касания.
- [Примеры распознавателя жестов](#Gesture_Recognizer_Samples) — использование встроенных распознавателей жестов.
- [Пример пользовательского распознавателя жестов](#Custom_Gesture_Recognizer) — Создание распознавателя пользовательских жестов.

В каждом разделе содержатся инструкции по написанию кода с нуля.

Следуйте приведенным ниже инструкциям, чтобы добавить код в раскадровку и узнать о различных типах событий касания, доступных в iOS. Также можно открыть [готовый пример](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final) , чтобы увидеть все работу.

<a name="Touch_Samples"/>

## <a name="touch-samples"></a>Примеры сенсорного ввода

В этом примере мы продемонстрируем некоторые API сенсорного ввода. Выполните следующие действия, чтобы добавить код, необходимый для реализации событий касания:

1. Откройте проект **Touch_Start**. Сначала запустите проект, чтобы убедиться, что все готово, и коснитесь кнопки **сенсорные примеры** . Вы увидите экран, аналогичный приведенному ниже (хотя ни одна из кнопок не будет работать):

    [![](ios-touch-walkthrough-images/image4.png "Sample app run with non-working buttons")](ios-touch-walkthrough-images/image4.png#lightbox)

1. Измените файл **TouchViewController.CS** и добавьте следующие две переменные экземпляра в класс `TouchViewController`:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private bool touchStartedInside;
    #endregion
    ```

1. Реализуйте метод `TouchesBegan`, как показано в следующем коде:

    ```csharp
    public override void TouchesBegan(NSSet touches, UIEvent evt)
    {
        base.TouchesBegan(touches, evt);

        // If Multitouch is enabled, report the number of fingers down
        TouchStatus.Text = string.Format ("Number of fingers {0}", touches.Count);

        // Get the current touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            // Check to see if any of the images have been touched
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Fist image touched
                TouchImage.Image = UIImage.FromBundle("TouchMe_Touched.png");
                TouchStatus.Text = "Touches Began";
            } else if (touch.TapCount == 2 && DoubleTouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                // Second image double-tapped, toggle bitmap
                if (imageHighlighted)
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
                    TouchStatus.Text = "Double-Tapped Off";
                }
                else
                {
                    DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
                    TouchStatus.Text = "Double-Tapped On";
                }
                imageHighlighted = !imageHighlighted;
            } else if (DragImage.Frame.Contains(touch.LocationInView(View)))
            {
                // Third image touched, prepare to drag
                touchStartedInside = true;
            }
        }
    }
    ```

    Этот метод работает путем проверки объекта `UITouch` и, если он существует, выполняет некоторые действия в зависимости от того, где произошло касание:

    - _Внутри таучимаже_ — отображение текста `Touches Began` в метке и изменение изображения.
    - _Внутри даублетаучимаже_ — измените изображение, которое отображается, если жест был двойным касанием.
    - _Внутри драгимаже_ — установите флаг, указывающий, что касание началось. Метод `TouchesMoved` будет использовать этот флаг, чтобы определить, следует ли перемещать `DragImage` по экрану или нет, так как мы видим на следующем шаге.

    Приведенный выше код работает только с отдельными касаниями, но по-прежнему не имеет поведения, если пользователь перемещает палец на экран. Чтобы ответить на перемещение, реализуйте `TouchesMoved`, как показано в следующем коде:

    ```csharp
    public override void TouchesMoved(NSSet touches, UIEvent evt)
    {
        base.TouchesMoved(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchStatus.Text = "Touches Moved";
            }

            //==== IMAGE DRAG
            // check to see if the touch started in the drag me image
            if (touchStartedInside)
            {
                // move the shape
                float offsetX = touch.PreviousLocationInView(View).X - touch.LocationInView(View).X;
                float offsetY = touch.PreviousLocationInView(View).Y - touch.LocationInView(View).Y;
                DragImage.Frame = new RectangleF(new PointF(DragImage.Frame.X - offsetX, DragImage.Frame.Y - offsetY), DragImage.Frame.Size);
            }
        }
    }
    ```

    Этот метод получает объект `UITouch`, а затем проверяет, где произошло касание. Если в `TouchImage`возникло касание, то на экране отобразится текст «касания».

    Если `touchStartedInside` имеет значение true, то мы понимаем, что пользователь имеет палец на `DragImage` и перемещает его. Код будет перемещаться `DragImage` по мере того, как пользователь перемещает палец вокруг экрана.

1. Нам нужно решить, когда пользователь отрывает свой палец с экрана или iOS отменяет касание. Для этого мы реализуем `TouchesEnded` и `TouchesCancelled`, как показано ниже:

    ```csharp
    public override void TouchesCancelled(NSSet touches, UIEvent evt)
    {
        base.TouchesCancelled(touches, evt);

        // reset our tracking flags
        touchStartedInside = false;
        TouchImage.Image = UIImage.FromBundle("TouchMe.png");
        TouchStatus.Text = "";
    }

    public override void TouchesEnded(NSSet touches, UIEvent evt)
    {
        base.TouchesEnded(touches, evt);
        // get the touch
        UITouch touch = touches.AnyObject as UITouch;
        if (touch != null)
        {
            //==== IMAGE TOUCH
            if (TouchImage.Frame.Contains(touch.LocationInView(TouchView)))
            {
                TouchImage.Image = UIImage.FromBundle("TouchMe.png");
                TouchStatus.Text = "Touches Ended";
            }
        }
        // reset our tracking flags
        touchStartedInside = false;
    }
    ```

    Оба этих метода сбрасывают флаг `touchStartedInside` в значение false. `TouchesEnded` также будет отображать `TouchesEnded` на экране.

1. На этом этапе экран сенсорных примеров завершен. Обратите внимание, что экран изменяется при взаимодействии с каждым из изображений, как показано на следующем снимке экрана:

    [![](ios-touch-walkthrough-images/image4.png "The starting app screen")](ios-touch-walkthrough-images/image4.png#lightbox)

    [![](ios-touch-walkthrough-images/image5.png "The screen after the user drags a button")](ios-touch-walkthrough-images/image5.png#lightbox)

<a name="Gesture_Recognizer_Samples" />

## <a name="gesture-recognizer-samples"></a>Примеры распознавателя жестов

В [предыдущем разделе](#Touch_Samples) показано, как перетащить объект на экран с помощью событий касания.
В этом разделе мы удалим события касания и покажем, как использовать следующие распознаватели жестов:

- `UIPanGestureRecognizer` для перетаскивания изображения вокруг экрана.
- `UITapGestureRecognizer` для реагирования на двойные касания на экране.

Чтобы реализовать распознаватели жестов, выполните следующие действия.

1. Измените файл **GestureViewController.CS** и добавьте следующую переменную экземпляра:

    ```csharp
    #region Private Variables
    private bool imageHighlighted = false;
    private RectangleF originalImageFrame = RectangleF.Empty;
    #endregion
    ```

    Эта переменная экземпляра необходима для того, чтобы контролировать предыдущее расположение изображения.
Средство распознавания жестов панорамирования будет использовать значение `originalImageFrame`, чтобы вычислить смещение, необходимое для перерисовки изображения на экране.

1. Добавьте следующий метод в контроллер:

    ```csharp
    private void WireUpDragGestureRecognizer()
    {
        // Create a new tap gesture
        UIPanGestureRecognizer gesture = new UIPanGestureRecognizer();

        // Wire up the event handler (have to use a selector)
        gesture.AddTarget(() => HandleDrag(gesture));  // to be defined

        // Add the gesture recognizer to the view
        DragImage.AddGestureRecognizer(gesture);
    }
    ```

    Этот код создает экземпляр `UIPanGestureRecognizer` и добавляет его в представление.
Обратите внимание, что целевой объект присваивается жесту в форме метода `HandleDrag` — этот метод предоставляется на следующем шаге.

1. Чтобы реализовать Хандледраг, добавьте в контроллер следующий код:

    ```csharp
    private void HandleDrag(UIPanGestureRecognizer recognizer)
    {
        // If it's just began, cache the location of the image
        if (recognizer.State == UIGestureRecognizerState.Began)
        {
            originalImageFrame = DragImage.Frame;
        }

        // Move the image if the gesture is valid
        if (recognizer.State != (UIGestureRecognizerState.Cancelled | UIGestureRecognizerState.Failed
            | UIGestureRecognizerState.Possible))
        {
            // Move the image by adding the offset to the object's frame
            PointF offset = recognizer.TranslationInView(DragImage);
            RectangleF newFrame = originalImageFrame;
            newFrame.Offset(offset.X, offset.Y);
            DragImage.Frame = newFrame;
        }
    }
    ```

    Приведенный выше код сначала проверит состояние распознавателя жестов, а затем переместит изображение на экране. С помощью этого кода контроллер теперь может поддерживать перетаскивание одного изображения на экране.

1. Добавьте `UITapGestureRecognizer`, который изменит образ, отображаемый в Даублетаучимаже. Добавьте следующий метод в контроллер `GestureViewController`:

    ```csharp
    private void WireUpTapGestureRecognizer()
    {
        // Create a new tap gesture
        UITapGestureRecognizer tapGesture = null;

        // Report touch
        Action action = () => {
            TouchStatus.Text = string.Format("Image touched at: {0}",tapGesture.LocationOfTouch(0, DoubleTouchImage));

            // Toggle the image
            if (imageHighlighted)
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe.png");
            }
            else
            {
                DoubleTouchImage.Image = UIImage.FromBundle("DoubleTapMe_Highlighted.png");
            }
            imageHighlighted = !imageHighlighted;
        };

        tapGesture = new UITapGestureRecognizer(action);

        // Configure it
        tapGesture.NumberOfTapsRequired = 2;

        // Add the gesture recognizer to the view
        DoubleTouchImage.AddGestureRecognizer(tapGesture);
    }
    ```

    Этот код очень похож на код `UIPanGestureRecognizer` но вместо использования делегата для целевого объекта мы используем `Action`.

1. Наконец, нам нужно изменить `ViewDidLoad` так, чтобы он вызывал только что добавленные методы. Измените ViewDidLoad, чтобы он соответствовал следующему коду:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        Title = "Gesture Recognizers";

        // Save initial state
        originalImageFrame = DragImage.Frame;

        WireUpTapGestureRecognizer();
        WireUpDragGestureRecognizer();
    }
    ```

    Также обратите внимание, что мы инициализируем значение `originalImageFrame`.

1. Запустите приложение и взаимодействуйте с двумя образами.
На следующем снимке экрана приведен один из примеров этих взаимодействий:

    [![](ios-touch-walkthrough-images/image7.png "This screenshot shows a drag interaction")](ios-touch-walkthrough-images/image7.png#lightbox)

<a name="Custom_Gesture_Recognizer"/>

## <a name="custom-gesture-recognizer"></a>Пользовательский распознаватель жестов

В этом разделе мы будем применять концепции из предыдущих разделов для создания пользовательского распознавателя жестов. Распознаватель пользовательских жестов будет подклассами `UIGestureRecognizer`и будет распознать, когда пользователь рисует "V" на экране, а затем переключается на точечный рисунок. Пример этого экрана приведен на следующем снимке экрана:

 [![](ios-touch-walkthrough-images/image8.png "The app will recognize when the user draws a `V` on the screen")](ios-touch-walkthrough-images/image8.png#lightbox)

Чтобы создать пользовательский распознаватель жестов, выполните следующие действия.

1. Добавьте новый класс в проект с именем `CheckmarkGestureRecognizer`и сделайте его похожим на следующий:

    ```csharp
    using System;
    using CoreGraphics;
    using Foundation;
    using UIKit;

    namespace Touch
    {
        public class CheckmarkGestureRecognizer : UIGestureRecognizer
        {
            #region Private Variables
            private CGPoint midpoint = CGPoint.Empty;
            private bool strokeUp = false;
            #endregion

            #region Override Methods
            /// <summary>
            ///   Called when the touches end or the recognizer state fails
            /// </summary>
            public override void Reset()
            {
                base.Reset();

                strokeUp = false;
                midpoint = CGPoint.Empty;
            }

            /// <summary>
            ///   Is called when the fingers touch the screen.
            /// </summary>
            public override void TouchesBegan(NSSet touches, UIEvent evt)
            {
                base.TouchesBegan(touches, evt);

                // we want one and only one finger
                if (touches.Count != 1)
                {
                    base.State = UIGestureRecognizerState.Failed;
                }

                Console.WriteLine(base.State.ToString());
            }

            /// <summary>
            ///   Called when the touches are cancelled due to a phone call, etc.
            /// </summary>
            public override void TouchesCancelled(NSSet touches, UIEvent evt)
            {
                base.TouchesCancelled(touches, evt);
                // we fail the recognizer so that there isn't unexpected behavior
                // if the application comes back into view
                base.State = UIGestureRecognizerState.Failed;
            }

            /// <summary>
            ///   Called when the fingers lift off the screen
            /// </summary>
            public override void TouchesEnded(NSSet touches, UIEvent evt)
            {
                base.TouchesEnded(touches, evt);
                //
                if (base.State == UIGestureRecognizerState.Possible && strokeUp)
                {
                    base.State = UIGestureRecognizerState.Recognized;
                }

                Console.WriteLine(base.State.ToString());
            }

            /// <summary>
            ///   Called when the fingers move
            /// </summary>
            public override void TouchesMoved(NSSet touches, UIEvent evt)
            {
                base.TouchesMoved(touches, evt);

                // if we haven't already failed
                if (base.State != UIGestureRecognizerState.Failed)
                {
                    // get the current and previous touch point
                    CGPoint newPoint = (touches.AnyObject as UITouch).LocationInView(View);
                    CGPoint previousPoint = (touches.AnyObject as UITouch).PreviousLocationInView(View);

                    // if we're not already on the upstroke
                    if (!strokeUp)
                    {
                        // if we're moving down, just continue to set the midpoint at
                        // whatever point we're at. when we start to stroke up, it'll stick
                        // as the last point before we upticked
                        if (newPoint.X >= previousPoint.X && newPoint.Y >= previousPoint.Y)
                        {
                            midpoint = newPoint;
                        }
                        // if we're stroking up (moving right x and up y [y axis is flipped])
                        else if (newPoint.X >= previousPoint.X && newPoint.Y <= previousPoint.Y)
                        {
                            strokeUp = true;
                        }
                        // otherwise, we fail the recognizer
                        else
                        {
                            base.State = UIGestureRecognizerState.Failed;
                        }
                    }
                }

                Console.WriteLine(base.State.ToString());
            }
            #endregion
        }
    }
    ```

    Метод Reset вызывается при изменении свойства `State` на `Recognized` или `Ended`. Это время для сброса любого внутреннего состояния, установленного в распознавателе пользовательских жестов.
Теперь класс может начать новый раз при следующем взаимодействии пользователя с приложением и быть готовым к повторной попытке распознавания жеста.

1. Теперь, когда мы определили пользовательский распознаватель жестов (`CheckmarkGestureRecognizer`), измените файл **CustomGestureViewController.CS** и добавьте следующие две переменные экземпляра:

    ```csharp
    #region Private Variables
    private bool isChecked = false;
    private CheckmarkGestureRecognizer checkmarkGesture;
    #endregion
    ```

1. Чтобы создать экземпляр и настроить распознаватель жестов, добавьте в контроллер следующий метод:

    ```csharp
    private void WireUpCheckmarkGestureRecognizer()
    {
        // Create the recognizer
        checkmarkGesture = new CheckmarkGestureRecognizer();

        // Wire up the event handler
        checkmarkGesture.AddTarget(() => {
            if (checkmarkGesture.State == (UIGestureRecognizerState.Recognized | UIGestureRecognizerState.Ended))
            {
                if (isChecked)
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Unchecked.png");
                }
                else
                {
                    CheckboxImage.Image = UIImage.FromBundle("CheckBox_Checked.png");
                }
                isChecked = !isChecked;
            }
        });

        // Add the gesture recognizer to the view
        View.AddGestureRecognizer(checkmarkGesture);
    }
    ```

1. Измените `ViewDidLoad` так, чтобы он вызывал `WireUpCheckmarkGestureRecognizer`, как показано в следующем фрагменте кода:

    ```csharp
    public override void ViewDidLoad()
    {
        base.ViewDidLoad();

        // Wire up the gesture recognizer
        WireUpCheckmarkGestureRecognizer();
    }
    ```

1. Запустите приложение и попробуйте вырисовать "V" на экране. Вы увидите, что отображаемое изображение изменилось, как показано на следующих снимках экрана:

    [![](ios-touch-walkthrough-images/image9.png "The button checked")](ios-touch-walkthrough-images/image9.png#lightbox)

    [![](ios-touch-walkthrough-images/image10.png "The button unchecked")](ios-touch-walkthrough-images/image10.png#lightbox)

В приведенных выше трех разделах демонстрируются различные способы реагирования на события касания в iOS: использование событий касания, встроенные средства распознавания жестов или пользовательский распознаватель жестов.

## <a name="related-links"></a>Связанные ссылки

- [Последняя версия iOS Touch (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/applicationfundamentals-touch-final)
