---
title: "Удаленное Siri и контроллеров Bluetooth"
description: "В этой статье рассматриваются поддержки нового удаленного Siri и Bluetooth игровые устройства в приложениях Xamarin.tvOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: A2DA4347-0563-4C72-A8D7-5B9DE9E28712
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: 5d74479e995c5c6ba6f6fd9fd23fbca78718ee31
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="siri-remote-and-bluetooth-controllers"></a>Удаленное Siri и контроллеров Bluetooth

_В этой статье рассматриваются поддержки нового удаленного Siri и Bluetooth игровые устройства в приложениях Xamarin.tvOS._


Пользователи приложения Xamarin.tvOS не взаимодействия с его интерфейс непосредственно как с iOS они tap изображения на экране устройства, но косвенно из в комнате с помощью [Siri удаленного](#The-Siri-Remote).

Если приложение будет игру, вы можете при необходимости сборку в поддержку третьих сторон, сделанные для операций ввода-вывода (MFI) [игровые устройства Bluetooth](#Bluetooth-Game-Controllers) в приложении также.

[ ![](remote-bluetooth-images/intro01.png "Bluetooth Remote и игрового устройства")](remote-bluetooth-images/intro01.png)

В этой статье описывается [удаленного Siri](#The-Siri-Remote), [жестов касания поверхности](#Touch-Surface-Gestures) и [удаленного кнопки Siri](#Siri-Remote-Buttons) и показано, как работать с ними с помощью [жесты и Раскадровки](#Gestures-and-Storyboards), [жестов и код](#Gestures-and-Code) и [обработка низкоуровневых событий](#Low-Level-Event-Handling). Наконец, в нем описывается [работа с игровые устройства](#Working-with-Game-Controllers) в приложении Xamarin.tvOS.

<a name="The-Siri-Remote" />

## <a name="the-siri-remote"></a>Удаленный Siri

Основной способ, которым пользователи будут взаимодействия с Apple TV и приложение Xamarin.tvOS — через включено удаленное Siri. Apple предназначены удаленного моста расстояние между пользователем на диване и большом Apple пользовательский интерфейс, отображаемый пространство на экране ТВ.

Ваш запрос, как разработчик приложения tvOS является создание быстрый, простой в использовании и визуально привлекательные пользовательский интерфейс, который использует область удаленного Siri сенсорный ввод, акселерометр, гироскопа и кнопки.

[ ![](remote-bluetooth-images/remote01.png "Удаленный Siri")](remote-bluetooth-images/remote01.png)

Удаленное Siri имеет следующие возможности и ожидаемый их использование в приложении tvOS:

<table width="100%" border="1px">
<tr>
    <td><b>Функция</b></td>
    <td><b>Использование приложения Общие</b></td>
    <td><b>Игры использования приложения</b></td>
</tr>
<tr>
    <td valign="top"><b>Поверхность сенсорного ввода</b><br/>Перейдите к экрану перейдите, нажмите клавишу, чтобы выбрать и удерживайте контекстных меню.</td>
    <td valign="top"><b>Проведите/TAP:</b><br/>Навигация по пользовательскому Интерфейсу между элементами может иметь фокус.<br/><br/><b>Щелкните:</b><br/>Активация выбранного элемента (фокус).</td>
    <td valign="top"><b>Проведите/TAP:</b><br/>Зависит от структуры игры и может использоваться как D-панель, коснувшись краев.<br/><br/><b>Щелкните:</b><br/>Выполните основную кнопку функцию.</td>
</tr>
<tr>
    <td valign="top"><b>Menu</b><br/>Нажмите, чтобы вернуться к предыдущему экрану или меню.</td>
    <td valign="top">Возвращает к предыдущему экрану и завершает работу для Apple TV начального экрана на экране основным приложением.</td>
    <td valign="top">Приостановка и возобновление игрового процесса, возврат к предыдущему экрану и завершает работу для Apple TV начального экрана на экране основным приложением.</td>
</tr>
<tr>
    <td valign="top"><b>Siri/Search</b><br/>В странах с Siri нажмите и удерживайте для управления речи, в любой другой стране, отображает экран поиска.</td>
    <td valign="top">Н/Д</td>
    <td valign="top">Н/Д</td>
</tr>
<tr>
    <td valign="top"><b>Play/Pause</b><br/>Воспроизведения и приостановки мультимедиа или предоставляет дополнительной функции в приложениях.</td>
    <td valign="top">Начинает воспроизведение мультимедиа и приостановки или возобновления воспроизведения.</td>
    <td valign="top">Выполняет функцию вторую кнопку или пропускает начальный видео (если существует).</td>
</tr>
<tr>
    <td valign="top"><b>Корневая папка</b><br/>Нажмите, чтобы вернуться на экран домашней, щелкните дважды, чтобы отобразить работающие приложения нажмите и удерживайте в спящий режим устройства.</td>
    <td valign="top">Н/Д</td>
    <td valign="top">Н/Д</td>
</tr>
<tr>
    <td valign="top"><b>Тома</b><br/>Элементы управления присоединенного тома аудио/видео оборудования.</td>
    <td valign="top">Н/Д</td>
    <td valign="top">Н/Д</td>
</tr>
</table>

<a name="Touch-Surface-Gestures" />

## <a name="touch-surface-gestures"></a>Сенсорный ввод поверхности

Поверхность Touch удаленного Siri может определять различные жестов касания одним пальцем, которые могут реагировать на в приложении Xamarin.tvOS:

<table width="100%">
<tr>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture01.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture02.png"></td>
    <td valign="top" width="30%"><img src="remote-bluetooth-images/Gesture03.png"></td>
</tr>
<tr>
    <td valign="top"><b>Проведите:</b><br/>Перемещает выделение (фокус) между элементами пользовательского интерфейса на экране (вверх, вниз влево, вправо). Проведение пальцем по экрану используется для прокрутки большие списки содержимого быстро с помощью инерции.</td>
    <td valign="top"><b>Щелкните:</b><br/>Активирует выбранного элемента (фокус) или ведет себя как основную кнопку в игру. Нажав и удерживая можно активировать контекстных меню или получателей функции.</td>
    <td valign="top"><b>Коснитесь:</b><br/>В незначительной степени касания поверхности Touch на краях ведет себя как направления кнопок на перемещение фокуса вверх, вниз, влево или вправо в зависимости от области при касании D Pad. В зависимости от приложения можно использовать для отображения скрытых элементов управления.</td>
</tr>
</table>

Apple предоставляет следующие рекомендации по работе с помощью жестов касания поверхности.

* **Различия между щелчками мышью и касания** -щелкнув является намеренное действие, пользователь и хорошо подходит для выбора, активации и основную кнопку игры. Нажимая кнопку менее заметно и следует использовать осмотрительно, поскольку пользователь часто удерживает удаленного Siri в их наличии и случайно можно активировать события касания легко.
* **Не переопределить стандартные жесты** -у пользователя есть предположения, что определенные жесты будет выполнять определенные действия в приложении не следует переопределить значение функции и функции этих жестов. Единственным исключением является игры приложения во время активного игрового процесса.
* **Определите новый жесты только в случае необходимости** -снова, у пользователя есть предположения, что определенные жесты будет выполнять определенные действия. Не следует определять пользовательских жестов для выполнения стандартных действий. И опять же, игры наиболее обычные исключение где пользовательских жестов можно добавить интересна, впечатляющие play игры.
* **В случае необходимости отвечать на касается планшета D** — в незначительной степени коснувшись угла края области Touch будет реагировать как D-прокладку на перемещение фокуса игровое или направление вверх, вниз, влево или вправо. При необходимости, вам следует реагировать на эти жесты в приложении или игры.

<a name="Siri-Remote-Buttons" />

## <a name="siri-remote-buttons"></a>Удаленное Siri кнопки

Помимо жесты на поверхности Touch приложение может отвечать на пользователя, щелкнув область Touch или нажав кнопку "Воспроизведение/Пауза". При доступе к Siri удаленного доступа с помощью Framework контроллера игры, можно также обнаружить нажатие кнопки меню. 

Кроме того, при нажатии кнопки меню могут быть обнаружены с помощью распознавателя со стандартными `UIKit` элементов. Если вы перехватывать нажатие кнопки меню, вам отвечает за закрытие текущего представления и контроллера представления и вернуться к предыдущему.

> [!IMPORTANT]
> **Примечание:** следует **всегда** назначить функцию на кнопку "Воспроизведение/Пауза" на удаленном. Нефункциональные кнопку можно сделать приложения поиска неработающий для конечного пользователя. Если у вас нет допустимой функцией для этой кнопки, назначьте та же функция, основную кнопку (Touch щелкните поверхность).




<a name="Gestures-and-Storyboards" />

## <a name="gestures-and-storyboards"></a>Ввод и раскадровки

Самый простой способ работы с удаленными Siri в приложении Xamarin.tvOS является добавление представления в конструкторе интерфейса распознавателей жестов.

Чтобы добавить распознаватель жестов, выполните следующее:

1. В **обозревателе решений**, дважды щелкните файл `Main.storyboard` и откройте его для редактирования конструктора интерфейса.
2. Перетащите **коснитесь распознаватель жестов** из **библиотеки** и поместите его в представлении: 

    [ ![](remote-bluetooth-images/storyboard01.png "Распознаватель жестов касания")](remote-bluetooth-images/storyboard01.png)
3. Проверьте **выберите** в **кнопку** раздел **инспектора атрибут**: 

    [ ![](remote-bluetooth-images/storyboard02.png "Проверьте Select")](remote-bluetooth-images/storyboard02.png)
4. **Выберите** означает жест будет реагировать на нажатие пользователем **Touch поверхность** на удаленном Siri. Вы также можете отвечает на **меню**, **воспроизведение/пауза**, **копирование**, **работу**, **влево** и **Право** кнопки.
5. Затем подключите **действия** из **коснитесь распознаватель жестов** и назовите его `TouchSurfaceClicked`: 

    [ ![](remote-bluetooth-images/storyboard03.png "Действие из распознаватель жестов касания")](remote-bluetooth-images/storyboard03.png)
6. Сохранить изменения и вернуться в Visual Studio для Mac.

Изменение контроллер представление (пример `FirstViewController.cs`) и добавьте следующий код для обработки жест запустилась файла:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class FirstViewController : UIViewController
    {
        ...

        #region Custom Actions
        partial void TouchSurfaceClicked (Foundation.NSObject sender) {
            // Handle click here
            ...
        }
        #endregion
    }
}
```

Дополнительные сведения о работе с помощью раскадровки см. в разделе нашей [Hello, tvOS краткое руководство по](~/ios/tvos/get-started/hello-tvos.md). В частности [Создание пользовательского интерфейса](~/ios/tvos/get-started/hello-tvos.md#Creating-the-User-Interface) и [написания кода с выходов и действия](~/ios/tvos/get-started/hello-tvos.md#Writing-the-Code) разделы.

<a name="Gestures-and-Code" />

## <a name="gestures-and-code"></a>Ввод и кода

При необходимости можно создать жесты непосредственно в код C# и добавить их к представлениям в пользовательском интерфейсе. Например чтобы добавить ряд проведите распознавателей жестов, изменить контроллер представление и добавьте следующий код:

```csharp
using System;
using UIKit;

namespace tvRemote
{
    public partial class SecondViewController : UIViewController
    {
        #region Constructors
        public SecondViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewDidLoad ()
        {
            base.ViewDidLoad ();    

            // Wire-up gestures
            var upGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Up";
                ButtonLabel.Text = "Swiped Up";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Up
            };
            this.View.AddGestureRecognizer (upGesture);

            var downGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Down";
                ButtonLabel.Text = "Swiped Down";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Down
            };
            this.View.AddGestureRecognizer (downGesture);

            var leftGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Left";
                ButtonLabel.Text = "Swiped Left";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Left
            };
            this.View.AddGestureRecognizer (leftGesture);

            var rightGesture = new UISwipeGestureRecognizer (() => {
                RemoteView.ArrowPressed = "Right";
                ButtonLabel.Text = "Swiped Right";
            }) {
                Direction = UISwipeGestureRecognizerDirection.Right
            };
            this.View.AddGestureRecognizer (rightGesture);
        }
        #endregion
    }
}
```

<a name="Low-Level-Event-Handling" />

## <a name="low-level-event-handling"></a>Обработка событий низкого уровня

При создании пользовательского типа на основе `UIKit` Xamarin.tvOS приложения (например `UIView`), также имеется возможность предоставления низкого уровня обработки при нажатии кнопки через `UIPress` события. 

Объект `UIPress` событие является tvOS что `UITouch` событий — iOS, за исключением `UIPress` возвращает сведения о кнопке нажимает на удаленном Siri или другие подключенные устройства Bluetooth (например, контроллер игры). `UIPress` события описаны на нажатие кнопки и его состояние (Began, отменено, изменено или завершено). 

Аналоговый кнопок на устройствах, такие как Bluetooth игровые устройства `UIPress` также возвращает объем принудительно применяется к кнопке. `Type` Свойство `UIPress` событий определяет, какие физические кнопка имеет измененное состояние, а остальные свойства описания произошедшего изменения.

Ниже приведен пример обработки низкоуровневого `UIPress` событий для `UIView`:

```csharp
using System;
using Foundation;
using UIKit;

namespace tvRemote
{
    public partial class EventView : UIView
    {
        #region Computed Properties
        public override bool CanBecomeFocused {
            get {
                return true;
            }
        }
        #endregion

        #region 
        public EventView (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void PressesBegan (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesBegan (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Red;
                }
            }
        }

        public override void PressesCancelled (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesCancelled (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }

        public override void PressesChanged (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesChanged (presses, evt);
        }

        public override void PressesEnded (NSSet<UIPress> presses, UIPressesEvent evt)
        {
            base.PressesEnded (presses, evt);

            foreach (UIPress press in presses) {
                // Was the Touch Surface clicked?
                if (press.Type == UIPressType.Select) {
                    BackgroundColor = UIColor.Clear;
                }
            }
        }
        #endregion
    }
}
```

Как и в `UITouch` события, если необходимо реализовать любой `UIPress` переопределения событий, следует реализовать все четыре.

<a name="Bluetooth-Game-Controllers" />

## <a name="bluetooth-game-controllers"></a>Bluetooth игровые устройства

Помимо стандартных удаленным Siri, который поставляется с Apple TV, третьих сторон, сделанные для iOS можно пару с Apple TV игровые устройства Bluetooth (периферийные устройства) и используется для управления Xamarin.tvOS приложения.

[ ![](remote-bluetooth-images/game01.png "Bluetooth игровые устройства")](remote-bluetooth-images/game01.png)

Игровые устройства можно использовать для улучшения игрового процесса и обеспечивают смысле погружения в игру. Они могут также использоваться для управления стандартный интерфейс Apple TV, так что использование не нужно переключаться между удаленной и контроллер.

> [!IMPORTANT]
> **Примечание:** игровые устройства Bluetooth — это необязательный покупки, конечные пользователи могут выполнить, приложение не может заставить пользователя приобрести. Если приложение поддерживает игровые устройства оно должно также поддерживать удаленного Siri таким образом, готовый к применению всеми пользователями Apple TV игры.


Контроллер игры имеет следующие функции и в приложении tvOS ожидаемый использования:
<table width="100%" border="1px">
<tr>
    <td><b>Функция</b></td>
    <td><b>Использование приложения Общие</b></td>
    <td><b>Игры использования приложения</b></td>
</tr>
<tr>
    <td valign="top"><b>D-Pad</b></td>
    <td valign="top">Переходит к элементы пользовательского интерфейса (изменения фокуса).</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>A</b></td>
    <td valign="top">Активация выбранного элемента (фокус).</td>
    <td valign="top">Выполняет функцию основную кнопку и подтверждает действий диалогового окна.</td>
</tr>
<tr>
    <td valign="top"><b>B</b></td>
    <td valign="top">Возвращает к предыдущему экрану или завершает работу на экран домашней, если на главном экране приложения.</td>
    <td valign="top">Выполняет функцию вторую кнопку или возвращает к предыдущему экрану.</td>
</tr>
<tr>
    <td valign="top"><b>X</b></td>
    <td valign="top">Начинает воспроизведение мультимедиа или приостановка и возобновление воспроизведения.</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Y</b></td>
    <td valign="top">Н/Д</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Menu</b></td>
    <td valign="top">Возвращает к предыдущему экрану или завершает работу на экран домашней, если на главном экране приложения.</td>
    <td valign="top">Приостановки или возобновления игрового процесса, возврат к предыдущему экрану или завершает работу на экран домашней на главном экране приложения.</td>
</tr>
<tr>
    <td valign="top"><b>Кнопка слева советом</b></td>
    <td valign="top">Переход влево.</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Левый триггера</b></td>
    <td valign="top">Переход влево.</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Кнопка плечо, сможет вправо</b></td>
    <td valign="top">Переход вправо.</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Правый триггера</b></td>
    <td valign="top">Переход вправо</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Левый джойстик</b></td>
    <td valign="top">Переходит к элементы пользовательского интерфейса (изменения фокуса).</td>
    <td valign="top">Зависит от игры.</td>
</tr>
<tr>
    <td valign="top"><b>Джойстик вправо</b></td>
    <td valign="top">Н/Д</td>
    <td valign="top">Зависит от игры.</td>
</tr>
</table>

Компания Apple предоставляет следующие рекомендации по работе с игровые устройства:

- **Подтверждение соединения контроллера игры** -tvOS приложения могут запускаться и останавливаться в любое время конечным пользователем. Всегда следует проверить наличие контроллера игры на запуск приложения или времени, переходит в спящий режим и принять меры.
- **Обеспечить Your App работает на удаленное Siri и игровые устройства** -не требуется для переключения между удаленного Siri и игры контроллер для использования приложения. Часто тестируете приложение с обоими типами контроллеров, обеспечивая все легко настраивается и работает должным образом.
- **Укажите способ обратно** -нажатие кнопки меню всегда возвращается к предыдущему экрану. Если пользователь входит в основное приложение экран, "меню" должен вернуть их для Apple TV начального экрана. Во время игрового процесса "меню" Отображать оповещение, предоставляя пользователю возможность приостановки или возобновления игрового процесса или возврата в главном меню.

<a name="Working-with-Game-Controllers" />

## <a name="working-with-game-controllers"></a>Работа с игровые устройства

Как уже говорилось выше, в дополнение к удаленным Siri, который поставляется с Apple TV, пользователь может при необходимости присоединить Стандартная сторонний, игровые устройства Bluetooth внесены для операций ввода-вывода (периферийные устройства) и использовать его для управления Xamarin.tvOS приложения.

Apple приложения необходимые входные данные низкого уровня контроллера, можно использовать [Framework контроллера игры](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013276) содержит следующие изменения для tvOS:

- Профиль контроллера Micro игры (`GCMicroGamepad`) был добавлен целевой удаленного Siri.
- Новый `GCEventViewController` класс может использоваться для перенаправления событий игрового устройства с помощью приложения. В разделе [определение ввод игру контроллера](#Determining-Game-Controller-Input) ниже, в разделе для получения дополнительных сведений.

<a name="Game-Controller-Support-Requirements" />

### <a name="game-controller-support-requirements"></a>Требования к поддержке игрового устройства

Apple имеет несколько особые требования, которые должны быть выполнены, если приложение Xamarin.tvOS поддерживает игровые устройства:

- **Должен поддерживать удаленное Siri** -всегда должен поддерживать удаленное Siri. Игры не требуется третьих сторон воспроизводиться игровому устройству.
- **Расширенные макет элемента управления необходимо поддерживать** -все tvOS игровые устройства не formfitting, расширенных контроллеров.
- **Игры должно быть Playable с контроллерами автономным** -Если приложение поддерживает контроллер расширенных игры, он должен быть пригодный для воспроизведения только с этого контроллера игры.
- **Должен поддерживать кнопку воспроизведения и приостановки** -во время игрового процесса, пользователь нажимает кнопку "Воспроизведение/Пауза", следует ли отображать оповещение, предоставляя пользователю возможность приостановки или возобновления игрового процесса или возврата в главном меню.

<a name="Enabling-Game-Controller-Support" />

### <a name="enabling-game-controller-support"></a>Включение поддержки игрового устройства

Чтобы включить поддержку контроллера игры в приложении Xamarin.tvOS, дважды щелкните `Info.plist` файла в **обозревателе решений** чтобы открыть его для редактирования:

[ ![](remote-bluetooth-images/game02.png "Редактор Info.plist")](remote-bluetooth-images/game02.png)

В разделе **игры контроллера** статьи, установите флажок, **включить игровые устройства**, затем проверьте все поддерживаемые приложением типы контроллера игры.

<a name="Using-the-Siri-Remote-as-a-Game-Controller" />

### <a name="using-the-siri-remote-as-a-game-controller"></a>С помощью удаленного Siri как игровые контроллеры

Удаленный Siri, входящие в состав Apple TV может использоваться как контроллер ограниченный игры. Как и другие игровые устройства оно отображается в инфраструктуре контроллера игры как `GCController` объект и поддерживает `GCMotion` и `GCMicroGamepad` профилей.

При использовании как контроллер игры, удаленного Siri имеет следующие характеристики:

- Область Touch может использоваться как D-панель, предоставляет аналогового входных данных.
- Удаленный может использоваться в книжной или альбомной ориентации и приложение решает, если объект профиля должны отразить входных данных автоматически.
- Щелкните область Touch действует как нажатие кнопки **A** на контроллере игры.
- Кнопка Воспроизведение/Пауза действует как кнопка **X** на контроллере игры.
- Отображать оповещение, предоставляя пользователю возможность приостановки или возобновления игрового процесса или вернуться в главное меню кнопки меню.

<a name="Summary" />

### <a name="determining-game-controller-input"></a>Определение входных данных игровое

В отличие от iOS, где можно получить событий контроллера игры параллельно с событиями сенсорного ввода, tvOS обрабатывает все низкоуровневые события для доставки высокого уровня `UIKit` события. В результате, если требуется доступ к событиям контроллера игры низкого уровня, необходимо отключить `UIKit`поведение по умолчанию.

На tvOS, когда необходимо обработать входные данные игровое напрямую, необходимо использовать `GCEventViewController` (или подкласс) для отображения игры в пользовательском интерфейсе. Каждый раз, когда `GCEventViewController` — *первый респондент*, ввод игру контроллера будут записываться и доставляется в приложение на платформе контроллера игры.

Можно использовать `UserInteractionEnabled` свойство `GCEventViewController` класса для переключения, как обрабатываются и обработки событий.

Сведения о реализации поддержки контроллера игры, см. в разделе Apple [работа с игровые устройства](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/WorkingwithGameControllers.html) статьи [приложения руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/index.html) и [игру контроллера Руководство по программированию](https://developer.apple.com/library/prerelease/tvos/documentation/ServicesDiscovery/Conceptual/GameControllerPG/Introduction/Introduction.html).

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье был проиллюстрирован новый удаленным Siri, который поставляется с Apple TV, жесты касания поверхности и удаленного Siri кнопки. Затем он рассматривается работа с жесты раскадровки, жестов и кода и низкоуровневые события. Наконец Если рассматриваются работа с игровые устройства.



## <a name="related-links"></a>Связанные ссылки

- [Примеры tvOS](https://developer.xamarin.com/samples/tvos/all/)
- [tvOS](https://developer.apple.com/tvos/)
- [tvOS человека направляющие интерфейса](https://developer.apple.com/tvos/human-interface-guidelines/)
- [Приложение руководство по программированию для tvOS](https://developer.apple.com/library/prerelease/tvos/documentation/General/Conceptual/AppleTV_PG/)
