---
title: Введение в трехмерное касание в Xamarin. iOS
description: В этой статье описывается, как использовать жесты трехмерного касания, появившиеся в iPhone 6S и iPhone 6S Plus. Эти жесты обеспечивают чувствительность к нажиму, просмотр и открытие и быстрые действия.
ms.prod: xamarin
ms.assetid: 806D051E-3791-40F7-9776-4E4D3E56F7F3
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/19/2017
ms.openlocfilehash: 209d70c2e158261ca671e6a889e979de39362d53
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647987"
---
# <a name="introduction-to-3d-touch-in-xamarinios"></a>Введение в трехмерное касание в Xamarin. iOS

_В этой статье описывается, как использовать новые жесты касания iPhone 6S и iPhone 6S Plus в приложении._

[![](3d-touch-images/info01.png "Примеры приложений с поддержкой трехмерного касания")](3d-touch-images/info01.png#lightbox)

В этой статье приводятся общие сведения об использовании новых API трехмерного касания для добавления жестов с учетом нажима в приложения Xamarin. iOS, которые выполняются на новых устройствах iPhone 6S и iPhone 6S Plus.

С помощью трехмерного касания приложение iPhone теперь может не только сообщать о том, что пользователь касается экрана устройства, но может понять, насколько сильно наработает пользователь и отвечает на различные уровни давления.

Трехмерное касание предоставляет следующие возможности для приложения:

- [Чувствительность](#Pressure-Sensitivity) к нажиму. приложения теперь могут измерять, насколько жестко или светлое пользователь касается экрана, и пользоваться этими сведениями.
  Например, приложение для рисования может сделать линию более толстой или тонкой в зависимости от того, насколько тяжело пользователь касается экрана.
- [Просмотр и всплывающее окно](#Peek-and-Pop) . Теперь приложение может взаимодействовать с данными, не переходя из текущего контекста. При нажатии на экран сложностей на экране, они могут просматривать интересующий вас элемент (например, предварительный просмотр сообщения). Нажимая сложную, можно появление элемента.
- [Быстрые действия](#Quick-Actions) — Подумайте о быстрых действиях, таких как контекстные меню, которые могут быть извлечены, когда пользователь щелкает правой кнопкой мыши элемент в классическом приложении.
  С помощью быстрых действий можно добавлять сочетания клавиш для функций в приложении непосредственно из значка приложения на начальном экране.
- [Тестирование трехмерного касания в симуляторе](#Testing-3D-Touch-in-the-Simulator) . с помощью правильного оборудования Mac можно тестировать приложения с поддержкой трехмерного касания в симуляторе iOS.

<a name="Pressure-Sensitivity" />

## <a name="pressure-sensitivity"></a>Чувствительность к нажиму

Как упоминалось выше, с помощью новых свойств класса [уитауч](xref:UIKit.UITouch) можно измерять объем нагрузки, применяемый пользователем на экране устройства iOS, и использовать эти сведения в пользовательском интерфейсе. Например, сделать мазок кистью более полупрозрачным или непрозрачным в зависимости от объема нагрузки.

[![](3d-touch-images/pressure01.png "Штрих-кисть, отображаемая как более полупрозрачная или непрозрачная в зависимости от объема нажима")](3d-touch-images/pressure01.png#lightbox)

В результате трехмерного касания, если приложение выполняется в iOS 9 (или больше) и устройство iOS может поддерживать трехмерное касание, изменения в нажиме приведут `TouchesMoved` к возникновению события.

Например, при наблюдении за `TouchesMoved` событием [UIView](xref:UIKit.UIView)можно использовать следующий код, чтобы получить текущую нагрузку, которую пользователь применяет к экрану:

```csharp
public override void TouchesMoved (NSSet touches, UIEvent evt)
{
    base.TouchesMoved (touches, evt);
    UITouch touch = touches.AnyObject as UITouch;
    if (touch != null)
    {
        // Get the pressure
        var force = touch.Force;
        var maxForce = touch.MaximumPossibleForce;

        // Do something with the touch and the pressure
        ...
    }
}
```

Свойство `MaximumPossibleForce` возвращает наибольшее возможное значение свойства `Force` [UITouch](xref:UIKit.UITouch) на основе устройства iOS, на котором работает приложение.

> [!IMPORTANT]
> Изменения в нажиме приведут `TouchesMoved` к возникновению события, даже если координаты X/Y не изменились. Из-за этого изменения в поведении приложения iOS должны быть готовы `TouchesMoved` к вызову события чаще, а координаты X/Y должны совпадать с последним `TouchesMoved` вызовом.




Дополнительные сведения см. в разделе Apple [таучканвас: Эффективное использование уитауч и эффективный](https://developer.apple.com/library/prerelease/ios/samplecode/TouchCanvas/) пример справочника по классу App и [уитауч](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UITouch_Class/).

<a name="Peek-and-Pop" />

## <a name="peek-and-pop"></a>Просмотр и POP

Трехмерное касание дает пользователям новые способы взаимодействия с информацией в приложении быстрее, чем когда-либо, без необходимости переходить из их текущего расположения.

Например, если приложение отображает таблицу сообщений, пользователь может нажать на элемент тяжело, чтобы просмотреть его содержимое в наложенном представлении (которое Apple означает как *Обзор*).

[![](3d-touch-images/peekandpop01.png "Пример просмотра содержимого")](3d-touch-images/peekandpop01.png#lightbox)

Если пользователь нажмет кнопку сложнее, он введет обычное представление сообщений (которое называется *POP*-Ping в представление).

### <a name="checking-for-3d-touch-availability"></a>Проверка доступности трехмерных сенсорных объектов

При работе с `UIViewController` можно использовать следующий код, чтобы узнать, поддерживает ли устройство iOS, на котором выполняется приложение, поддержку трехмерного касания:

```csharp
public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
{
    //Important: call the base function
    base.TraitCollectionDidChange(previousTraitCollection);

    //See if the new TraitCollection value includes force touch
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        //Do something with 3D touch, for instance...
        RegisterForPreviewingWithDelegate (this, View);
        ...
```

Этот метод может быть вызван до *или после* `ViewDidLoad()`.

### <a name="handling-peek-and-pop"></a>Обработка просмотра и POP

На устройстве iOS, которое может работать с трехмерным касанием, можно использовать экземпляр `UIViewControllerPreviewingDelegate` класса для управления отображением сведений об элементах **просмотра** и элемента **POP** . Например, если у нас есть контроллер `MasterViewController` представления таблиц, можно использовать следующий код для поддержки **просмотра** и **POP**:

```csharp
using System;
using System.Collections.Generic;
using UIKit;
using Foundation;
using CoreGraphics;

namespace DTouch
{
    public class PreviewingDelegate : UIViewControllerPreviewingDelegate
    {
        #region Computed Properties
        public MasterViewController MasterController { get; set; }
        #endregion

        #region Constructors
        public PreviewingDelegate (MasterViewController masterController)
        {
            // Initialize
            this.MasterController = masterController;
        }

        public PreviewingDelegate (NSObjectFlag t) : base(t)
        {
        }

        public PreviewingDelegate (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        /// Present the view controller for the "Pop" action.
        public override void CommitViewController (IUIViewControllerPreviewing previewingContext, UIViewController viewControllerToCommit)
        {
            // Reuse Peek view controller for details presentation
            MasterController.ShowViewController(viewControllerToCommit,this);
        }

        /// Create a previewing view controller to be shown at "Peek".
        public override UIViewController GetViewControllerForPreview (IUIViewControllerPreviewing previewingContext, CGPoint location)
        {
            // Grab the item to preview
            var indexPath = MasterController.TableView.IndexPathForRowAtPoint (location);
            var cell = MasterController.TableView.CellAt (indexPath);
            var item = MasterController.dataSource.Objects [indexPath.Row];

            // Grab a controller and set it to the default sizes
            var detailViewController = MasterController.Storyboard.InstantiateViewController ("DetailViewController") as DetailViewController;
            detailViewController.PreferredContentSize = new CGSize (0, 0);

            // Set the data for the display
            detailViewController.SetDetailItem (item);
            detailViewController.NavigationItem.LeftBarButtonItem = MasterController.SplitViewController.DisplayModeButtonItem;
            detailViewController.NavigationItem.LeftItemsSupplementBackButton = true;

            // Set the source rect to the cell frame, so everything else is blurred.
            previewingContext.SourceRect = cell.Frame;

            return detailViewController;
        }
        #endregion
    }
}
```

Метод используется для выполнения операции просмотра. `GetViewControllerForPreview` Он получает доступ к ячейке таблицы и резервные данные, а затем загружает `DetailViewController` из текущей раскадровки. Если задать `PreferredContentSize` для параметра значение (0, 0), то запрашивается **Размер представления просмотра** по умолчанию. Наконец, мы будем размыт все, кроме ячейки, с `previewingContext.SourceRect = cell.Frame` которой мы видим, и возвращаем новое представление для отображения.

Компонент повторно использует представление, созданное при просмотре представления **POP** , когда пользователь нажимает сложнее. `CommitViewController`

### <a name="registering-for-peek-and-pop"></a>Регистрация для просмотра и POP

На контроллере представления, который хочет разрешить пользователю **просматривать** и **открывать** элементы, необходимо зарегистрироваться для получения этой службы. В примере, приведенном выше для контроллера представления таблицы (`MasterViewController`), мы будем использовать следующий код:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Check to see if 3D Touch is available
    if (TraitCollection.ForceTouchCapability == UIForceTouchCapability.Available) {
        // Register for Peek and Pop
        RegisterForPreviewingWithDelegate(new PreviewingDelegate(this), View);
    }
    ...

}
```

Здесь мы вызываем `RegisterForPreviewingWithDelegate` метод с помощью созданного ранее экземпляра `PreviewingDelegate` . На устройствах iOS, поддерживающих трехмерное касание, пользователь может нажать на элемент тяжело, чтобы посмотреть на него. Если они нажимают еще сложнее, элемент будет открываться в виде стандартного представления отображения.

Дополнительные сведения см. в нашем [примере для iOS 9 аппликатионшорткутс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) и Apple [виевконтроллерпревиевс: Использование примера приложения](https://developer.apple.com/library/prerelease/ios/samplecode/ViewControllerPreviews/Introduction/Intro.html) API предварительного просмотра UIViewController, справочника по классам [уипревиевактион](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewAction_Class/), справочника по классу [уипревиевактионграуп](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionGroup_Class/) и Справочника по протоколу [уипревиевактионитем](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIPreviewActionItem_Protocol/).

<a name="Quick-Actions" />

## <a name="quick-actions"></a>Быстрые действия

С помощью трехмерных и быстрых действий можно добавлять общие, быстрые и простые в доступе сочетания клавиш для функций в приложении со значка начального экрана на устройстве iOS.

Как упоминалось выше, можно представить быстрые действия, такие как контекстные меню, которые могут быть извлечены, когда пользователь щелкает правой кнопкой мыши элемент в классическом приложении. Быстрые действия следует использовать для предоставления сочетаний клавиш для наиболее распространенных функций и функций приложения.

[![](3d-touch-images/quickactions01.png "Пример меню быстрых действий")](3d-touch-images/quickactions01.png#lightbox)


### <a name="defining-static-quick-actions"></a>Определение статических быстрых действий

Если одно или несколько быстрых действий, необходимых для приложения, являются статическими и их не нужно изменять, их можно определить в `Info.plist` файле приложения. Измените этот файл во внешнем редакторе и добавьте следующие разделы:

```xml
<key>UIApplicationShortcutItems</key>
<array>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeSearch</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will search for an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Search</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.000</string>
    </dict>
    <dict>
        <key>UIApplicationShortcutItemIconType</key>
        <string>UIApplicationShortcutIconTypeShare</string>
        <key>UIApplicationShortcutItemSubtitle</key>
        <string>Will share an item</string>
        <key>UIApplicationShortcutItemTitle</key>
        <string>Share</string>
        <key>UIApplicationShortcutItemType</key>
        <string>com.company.appname.001</string>
    </dict>
</array>
```

Здесь мы определяем два статических элемента быстрого действия со следующими ключами:

- `UIApplicationShortcutItemIconType`— Определяет значок, который будет отображаться элементом быстрого действия как одно из следующих значений:
  - `UIApplicationShortcutIconTypeAdd`
  - `UIApplicationShortcutIconTypeAlarm`
  - `UIApplicationShortcutIconTypeAudio`
  - `UIApplicationShortcutIconTypeBookmark`
  - `UIApplicationShortcutIconTypeCapturePhoto`
  - `UIApplicationShortcutIconTypeCaptureVideo`
  - `UIApplicationShortcutIconTypeCloud`
  - `UIApplicationShortcutIconTypeCompose`
  - `UIApplicationShortcutIconTypeConfirmation`
  - `UIApplicationShortcutIconTypeContact`
  - `UIApplicationShortcutIconTypeDate`
  - `UIApplicationShortcutIconTypeFavorite`
  - `UIApplicationShortcutIconTypeHome`
  - `UIApplicationShortcutIconTypeInvitation`
  - `UIApplicationShortcutIconTypeLocation`
  - `UIApplicationShortcutIconTypeLove`
  - `UIApplicationShortcutIconTypeMail`
  - `UIApplicationShortcutIconTypeMarkLocation`
  - `UIApplicationShortcutIconTypeMessage`
  - `UIApplicationShortcutIconTypePause`
  - `UIApplicationShortcutIconTypePlay`
  - `UIApplicationShortcutIconTypeProhibit`
  - `UIApplicationShortcutIconTypeSearch`
  - `UIApplicationShortcutIconTypeShare`
  - `UIApplicationShortcutIconTypeShuffle`
  - `UIApplicationShortcutIconTypeTask`
  - `UIApplicationShortcutIconTypeTaskCompleted`
  - `UIApplicationShortcutIconTypeTime`
  - `UIApplicationShortcutIconTypeUpdate`

        ![](3d-touch-images/uiapplicationshortcuticontype.png "UIApplicationShortcutIconType imagery")

* `UIApplicationShortcutItemSubtitle`— Определяет подзаголовок для элемента.
* `UIApplicationShortcutItemTitle`— Определяет заголовок для элемента.
* `UIApplicationShortcutItemType`— Строковое значение, которое будет использоваться для распознавания элемента в нашем приложении. Дополнительные сведения см. в следующем разделе.

> [!IMPORTANT]
> Элементы ярлыков быстрого действия, заданные в `Info.plist` файле, недоступны `Application.ShortcutItems` со свойством. Они передаются `HandleShortcutItem` только обработчику событий.





### <a name="identifying-quick-action-items"></a>Определение элементов быстрого действия

Как было показано выше, при определении элементов быстрого действия в приложении `Info.plist`для их определения присваивается строковое значение. `UIApplicationShortcutItemType`

Чтобы упростить работу с идентификаторами в коде, добавьте класс с именем `ShortcutIdentifier` в проект приложения и сделайте его следующим:

```csharp
using System;

namespace AppSearch
{
    public static class ShortcutIdentifier
    {
        public const string First = "com.company.appname.000";
        public const string Second = "com.company.appname.001";
        public const string Third = "com.company.appname.002";
        public const string Fourth = "com.company.appname.003";
    }
}
```

<a name="Handling-a-Quick-Action" />

### <a name="handling-a-quick-action"></a>Обработка быстрого действия

Затем необходимо изменить `AppDelegate.cs` файл приложения, чтобы управлять пользовательским элементом быстрого действия на начальном экране.

Внесите следующие изменения:

```csharp
using System;
...

public UIApplicationShortcutItem LaunchedShortcutItem { get; set; }

public bool HandleShortcutItem(UIApplicationShortcutItem shortcutItem) {
    var handled = false;

    // Anything to process?
    if (shortcutItem == null) return false;

    // Take action based on the shortcut type
    switch (shortcutItem.Type) {
    case ShortcutIdentifier.First:
        Console.WriteLine ("First shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Second:
        Console.WriteLine ("Second shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Third:
        Console.WriteLine ("Third shortcut selected");
        handled = true;
        break;
    case ShortcutIdentifier.Fourth:
        Console.WriteLine ("Forth shortcut selected");
        handled = true;
        break;
    }

    // Return results
    return handled;
}

public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    return shouldPerformAdditionalDelegateHandling;
}

public override void OnActivated (UIApplication application)
{
    // Handle any shortcut item being selected
    HandleShortcutItem(LaunchedShortcutItem);

    // Clear shortcut after it's been handled
    LaunchedShortcutItem = null;
}

public override void PerformActionForShortcutItem (UIApplication application, UIApplicationShortcutItem shortcutItem, UIOperationHandler completionHandler)
{
    // Perform action
    completionHandler(HandleShortcutItem(shortcutItem));
}
```

Сначала мы определим общедоступное `LaunchedShortcutItem` свойство для отслеживания последнего выбранного элемента быстрого действия пользователем. Затем мы переопределяем `FinishedLaunching` метод и видим, если `launchOptions` он был передан и содержит элемент быстрого действия. В этом случае мы сохраняем быстрое действие в `LaunchedShortcutItem` свойстве.

Далее мы переопределяем `OnActivated` метод и передаем любой выбранный элемент быстрого запуска `HandleShortcutItem` в метод, к которому будет применена операция. Сейчас мы записываете сообщение только в **консоль**. В реальном приложении вам нужно было бы решить, какие действия требовалось выполнить. После выполнения действия `LaunchedShortcutItem` свойство удаляется.

Наконец, если приложение уже запущено, `PerformActionForShortcutItem` метод будет вызван для выполнения элемента быстрого действия, поэтому необходимо переопределить его и вызвать наш `HandleShortcutItem` метод.


### <a name="creating-dynamic-quick-action-items"></a>Создание динамических элементов быстрого действия

Помимо определения статических элементов быстрого действия в `Info.plist` файле приложения, вы можете создавать динамические быстрые действия в режиме реального времени. Чтобы определить два новых динамических действия, измените `AppDelegate.cs` файл еще раз и `FinishedLaunching` измените метод так, чтобы он выглядел следующим образом:

```csharp
public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
{
    var shouldPerformAdditionalDelegateHandling = true;

    // Get possible shortcut item
    if (launchOptions != null) {
        LaunchedShortcutItem = launchOptions [UIApplication.LaunchOptionsShortcutItemKey] as UIApplicationShortcutItem;
        shouldPerformAdditionalDelegateHandling = (LaunchedShortcutItem == null);
    }

    // Add dynamic shortcut items
    if (application.ShortcutItems.Length == 0) {
        var shortcut3 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Third, "Play") {
            LocalizedSubtitle = "Will play an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Play)
        };

        var shortcut4 = new UIMutableApplicationShortcutItem (ShortcutIdentifier.Fourth, "Pause") {
            LocalizedSubtitle = "Will pause an item",
            Icon = UIApplicationShortcutIcon.FromType(UIApplicationShortcutIconType.Pause)
        };

        // Update the application providing the initial 'dynamic' shortcut items.
        application.ShortcutItems = new UIApplicationShortcutItem[]{shortcut3, shortcut4};
    }

    return shouldPerformAdditionalDelegateHandling;
}
```

Теперь мы `application` продемонстрируем, содержит ли уже набор динамически создаваемых `ShortcutItems`, если это не так, мы создадим два новых `UIMutableApplicationShortcutItem` объекта для определения новых элементов и их добавления в `ShortcutItems` массив.

Код, который мы уже добавили в раздел [Обработка краткого действия](#Handling-a-Quick-Action) выше, будет обрабатывать эти динамические быстрые действия так же, как статические.

Следует отметить, что вы можете создать сочетание статических и динамических элементов быстрого действия (как мы делаем здесь), но вы не ограничены одним или другим.

Дополнительные сведения см. в нашем [примере виевконтроллерпревиев для iOS 9](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview) и в статье [Apple аппликатионшорткутс: Использование примера](https://developer.apple.com/library/prerelease/ios/samplecode/ApplicationShortcuts/) приложения уиаппликатионшорткутитем, [ссылки на класс уиаппликатионшорткутитем](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutItem_class/), ссылку [на класс уимутаблеаппликатионшорткутитем](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIMutableApplicationShortcutItem_class/) и [ссылку на класс UIApplicationShortcutIcon](https://developer.apple.com/library/prerelease/ios/documentation/UIKit/Reference/UIApplicationShortcutIcon_Class/).

<a name="Testing-3D-Touch-in-the-Simulator" />

## <a name="testing-3d-touch-in-the-simulator"></a>Тестирование трехмерного касания в симуляторе

При использовании последней версии Xcode и симулятора iOS на совместимом компьютере Mac с Force Touch включения трекпада можно протестировать функции трехмерного касания в симуляторе.

Чтобы включить эту функцию, запустите любое приложение на имитируемом оборудовании iPhone, поддерживающем трехмерное касание (iPhone 6S и более поздние версии). Затем выберите меню **оборудование** в симуляторе iOS и включите пункт меню **использовать трекпада Force для трехмерного касания** :

[![](3d-touch-images/simulator01.png "Выберите меню оборудование в симуляторе iOS и включите пункт меню использовать трекпада Force для трехмерного касания.")](3d-touch-images/simulator01.png#lightbox)

Если эта функция активна, вы можете нажимать на трекпада Mac, чтобы включить трехмерное касание точно так же, как на реальном устройстве iPhone.

## <a name="summary"></a>Сводка

В этой статье появились новые API-интерфейсы трехмерного касания, доступные в iOS 9 для iPhone 6S и iPhone 6S Plus. В нем рассматривается добавление чувствительности к нажиму для приложения. Использование функции Peek и POP для быстрого отображения сведений в приложении из текущего контекста без навигации; и использование быстрых действий для предоставления сочетаний клавиш для наиболее часто используемых функций приложения.



## <a name="related-links"></a>Связанные ссылки

- [Пример Виевконтроллерпревиев iOS 9](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-viewcontrollerpreview)
- [Пример Аппликатионшорткутс iOS 9](https://docs.microsoft.com/samples/xamarin/ios-samples/ios9-applicationshortcuts)
- [iOS 9 для разработчиков](https://developer.apple.com/ios/pre-release/)
- [iOS 9,0](https://developer.apple.com/library/prerelease/ios/releasenotes/General/WhatsNewIniOS/Articles/iOS9.html)
- [Подготовка приложений iPhone для трехмерного касания](https://developer.apple.com/ios/3d-touch/)
