---
title: "watchOS Устранение неполадок"
description: "Известные проблемы и способы решения проблем разработки watchOS."
ms.topic: article
ms.prod: xamarin
ms.assetid: 27C31DB8-451E-4888-BBC1-CE0DFC2F9DEC
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9c9032b3206fa35d264bd69c94b7882e877b334f
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="watchos-troubleshooting"></a>watchOS Устранение неполадок

Эта страница содержит дополнительные сведения и решения для функции находится в разработке. Некоторые из этих способов применяются только к выпускам предварительного просмотра.

- [Известные проблемы](#knownissues)

- [Удаление альфа-канала из изображений значков](#noalpha)

- [Ручное добавление файлов контроллера интерфейса](#add) для Xcode Interface Builder.

- [Запуск WatchApp из командной строки](#command_line)

<a name="knownissues" />

## <a name="known-issues"></a>Известные проблемы

### <a name="general"></a>Общие

<a name="deploy" />
<!--
* You cannot deploy to the App Store *from within Visual Studio for Mac or Visual Studio*
    in the current release. You should create an **Archive** in Visual Studio for Mac
    and then switch to Xcode to upload the archive to iTunes Connect. Visual Studio
    is not currently supported (but will be a future release). Refer to the
    [deployment guide](~/ios/watchos/deploy-test/appstore.md) for more information.
-->

- Более ранних версиях Visual Studio для Mac неправильно отображается одно из **AppleCompanionSettings** значки, как если бы 88 x 88 точек; что приводит к **отсутствует значок ошибки** при попытке отправить в приложение Хранилище.
    Этот значок должны иметь размер 87 x 87 (29 единицы измерения для  **@3x**  Сетчатка экраны). Не удается устранить эту проблему в Visual Studio для Mac - либо изменение ресурса изображения в Xcode или вручную изменить **Contents.json** файла (для соответствия [в этом примере](https://github.com/xamarin/monotouch-samples/blob/master/WatchKit/WatchKitCatalog/WatchApp/Resources/Images.xcassets/AppIcons.appiconset/Contents.json#L126-L132)).

- Если проект расширения Контрольные значения **Info.plist > идентификатор пакета WKApp** не [правильно установить](~/ios/watchos/get-started/project-references.md) для сопоставления приложения Контрольные значения **идентификатор пакета**, отладчик не удастся подключиться и Visual Будет ожидать Studio для Mac с сообщением *«Ожидание подключения отладчика»*.

- Отладка поддерживается в **уведомления** режиме, но может быть ненадежной. Повторная попытка иногда будет работать. Убедитесь, что приложение Watch **Info.plist** `WKCompanionAppBundleIdentifier` соответствует идентификатор пакета, родительского или контейнер приложения iOS (т. е., выполняется на iPhone).

<!--
- **Can't launch application on Watch simulator.** This seems to
    be an issue with the iOS Simulator hanging when trying to
    install an app that has changed. Xcode release notes (beta 4)
    includes a similar known issue:
    If the issue persists, reset the Simulator (**iOS Simulator > Reset Content and Settings...**).
-->

- Конструктор iOS не содержит entrypoint стрелок для обзора или уведомления интерфейса контроллеров.

- Не удается добавить два `WKNotificationControllers` раскадровки.
    Обходной путь: `notificationCategory` всегда вставляется элемент в раскадровке XML с тем же `id`. Чтобы обойти эту проблему, можно добавить контроллеры уведомления два (или более), откройте файл раскадровки в текстовом редакторе и вручную изменить `id` элемент быть уникальным.

    [ ![](troubleshooting-images/duplicate-id-sml.png "")](troubleshooting-images/duplicate-id.png)

- Может появится сообщение об ошибке «приложение не была построена» при попытке запустить приложение. Это происходит после **Очистить** Если имеет значение запускаемого проекта в проект расширения контрольных значений.
    Исправление заключается в выборе **сборки > перестроить все** и повторном запуске приложения.

### <a name="visual-studio"></a>Visual Studio

Поддержка iOS конструктор для комплекта средств для наблюдения за *требует* решения настроены правильно. Если ссылки проекта не заданы (в разделе [как задать ссылки](~/ios/watchos/get-started/project-references.md)) рабочей области конструирования не будет работать правильно.

<!--
* New Watch Kit apps created in Visual Studio might not allow
    starting in Notifications mode.

* You cannot deploy to the App Store from Visual Studio (see [notes above](#deploy)
    and the [deployment guide](~/ios/watchos/deploy-test/appstore.md)). Use
    Visual Studio for Mac and Xcode on your Mac Build Host.
    -->

<a name="noalpha" />

## <a name="removing-the-alpha-channel-from-icon-images"></a>Удаление альфа-канала из изображения значков

Значки не должны содержать альфа-канала (альфа-канал определяет прозрачные области изображения), в противном случае будут отклонены приложения во время отправки в магазин приложений с сообщением об ошибке следующего вида:

```csharp
Invalid Icon - The watch application '...watchkitextension.appex/WatchApp.app'
contains an icon file '...watchkitextension.appex/WatchApp.app/Icon-27.5@2x.png'
with an alpha channel. Icons should not have an alpha channel.
```

Можно легко удалить альфа-канал в Mac OS X с помощью **предварительного просмотра** приложения:

1. Открыть изображение значка в **предварительного просмотра** и выберите **файл > Экспорт**.

2. Появление диалогового окна будет включать **альфа-канал** флажок, если присутствует альфа-канала.

    ![](troubleshooting-images/remove-alpha-sml.png "")

3. *

4. Изображение значка теперь следует передавать проверки компании Apple.


<a name="add" />

## <a name="manually-adding-interface-controller-files"></a>Вручную добавлять файлы контроллер интерфейса

> [!IMPORTANT]
> Поддержка комплекта средств для наблюдения за Xamarin включает в себя проектирование раскадровки Контрольные значения в конструкторе операций ввода-вывода (в Visual Studio для Mac и Visual Studio), не требующий следующие действия. Просто присвойте контроллеру интерфейса имя класса в Visual Studio для свойства Mac планшета и файлы кода C# создается автоматически.


*


1. Откройте приложение watch **Interface.storyboard** в **Xcode интерфейс построителя**.
    
    ![](troubleshooting-images/add-6.png "")

2. Перетащите новую `InterfaceController` на раскадровку:

    ![](troubleshooting-images/add-1.png "A InterfaceController")

3. Теперь можно перетаскивать элементы управления на контроллер интерфейса (например) подписи и кнопки), но нельзя создать розетки или действия еще, так как не **.h** файл заголовка. Следующие шаги вызовет необходимая **.h** создания файла заголовка.

    ![](troubleshooting-images/add-2.png "")

4. Закройте раскадровку и вернуться в Visual Studio для Mac. Создайте новый файл C# **MyInterfaceController.cs** (или любое имя по своему усмотрению) в **смотреть расширения приложения** проекта (не Контрольное значение само приложение где раскадровки). Добавьте следующий код (обновление пространства имен, classname и имени конструктора):

        using System;
        using WatchKit;
        using Foundation;
        
        namespace WatchAppExtension  // remember to update this
        {
            public partial class MyInterfaceController // remember to update this
            : WKInterfaceController
            {
                public MyInterfaceController // remember to update this
                (IntPtr handle) : base (handle)
                {
                }
                public override void Awake (NSObject context)
                {
                    base.Awake (context);
                    // Configure interface objects here.
                    Console.WriteLine ("{0} awake with context", this);
                }
                public override void WillActivate ()
                {
                    // This method is called when the watch view controller is about to be visible to the user.
                    Console.WriteLine ("{0} will activate", this);
                }
                public override void DidDeactivate ()
                {
                    // This method is called when the watch view controller is no longer visible to the user.
                    Console.WriteLine ("{0} did deactivate", this);
                }
            }
        }

5. Создайте еще один новый файл C# **MyInterfaceController.designer.cs** в **смотреть расширения приложения** проекта и добавьте приведенный ниже код. Не забудьте обновить пространство имен, classname и `Register` атрибута:

    ```csharp
    using Foundation;
    using System.CodeDom.Compiler;
    
    namespace HelloWatchExtension  // remember to update this
    {
        [Register ("MyInterfaceController")] // remember to update this
        partial class MyInterfaceController  // remember to update this
        {
            void ReleaseDesignerOutlets ()
            {
            }
        }
    }
    ```
    
    Совет: Можно (необязательно) сделать этот файл является дочерним узлом первый файл, перетащив его в другой файл C# в Visual Studio для Mac планшета решения. Затем он будет отображаться следующим образом:
    
    ![](troubleshooting-images/add-5.png "")

6. Выберите **сборки > собрать все** , чтобы Xcode синхронизации распознает новый класс (через `Register` атрибут), мы использовали.

7. Повторно открыть раскадровку, щелкнув файл раскадровки приложения контрольных значений и выбрав **открыть с помощью > Xcode интерфейс построителя**:

    ![](troubleshooting-images/add-6.png "")

8. Выберите новый контроллер интерфейса и присвойте ему classname, определенного выше, например. `MyInterfaceController`.
Если все работал правильно, она должна отображаться автоматически в **класса:** раскрывающийся список и можно выбрать его оттуда.

    ![](troubleshooting-images/add-4.png "")

9. Выберите **помощника редактор** открыть в Xcode (значок с двумя круга), можно увидеть раскадровки и код side-by-side:

    ![](troubleshooting-images/add-7.png "")

    Если фокус находится в области кода, обеспечения взглянуть на **.h** файл заголовка и если не щелкните правой кнопкой мыши иерархическую панель и выберите нужный файл (**MyInterfaceController.h**)

    ![](troubleshooting-images/add-8.png "")

10. Теперь можно создать выходов и действия по **Ctrl + перетащите** из раскадровки в **.h** файл заголовка.

    ![](troubleshooting-images/add-9.png "")

    При освобождении перетаскивание, будет предложено выбрать, следует ли создать розетки или действия и выберите его имя.

    ![](troubleshooting-images/add-a.png "")

11. После сохранения изменений раскадровки Xcode закрыт, вернитесь в Visual Studio для Mac. Он обнаружит изменения файла заголовка и автоматически добавлять код **. designer.cs** файла:


        [Register ("MyInterfaceController")]
        partial class MyInterfaceController
        {
            [Outlet]
            WatchKit.WKInterfaceButton myButton { get; set; }
        
            void ReleaseDesignerOutlets ()
            {
                if (myButton != null) {
                    myButton.Dispose ();
                    myButton = null;
                }
            }
        }


Теперь можно ссылаться на элемент управления (или выполнения действий) в C#!


<a name="command_line" />

## <a name="launching-the-watch-app-from-the-command-line"></a>Запустив приложение Контрольные значения из командной строки

> [!IMPORTANT]
> Можно запустить приложение Watch в режиме обычного приложения по умолчанию, а также в **Обзор** или **уведомления** режимы с использованием [выполнения пользовательских параметров](~/ios/watchos/get-started/installation.md#custommodes) в Visual Studio для Mac и Visual Studio.


Также можно использовать из командной строки для управления симулятор iOS. — Средство командной строки, используемые для запуска наблюдения за приложениями **mtouch**.

Ниже приведен полный пример (выполняется в виде одной строки в окне терминала).

```bash
/Library/Frameworks/Xamarin.iOS.framework/Versions/Current/bin/mtouch --sdkroot=/Applications/Xcode.app/Contents/Developer/ --device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```

Параметр, необходимо выполнить обновление приложения `launchsimwatch`:

### <a name="--launchsimwatch"></a>--launchsimwatch

Полный путь к пакету основным приложением *для приложения iOS, которое содержит приложение watch и расширение*.

> [!NOTE]
> *

Пример

```bash
--launchsimwatch=/path/to/watchkitproject/watchsample/bin/iPhoneSimulator/Debug/watchsample.app
```


## <a name="notification-mode"></a>Режим уведомлений

Тестирование приложения [ **уведомления** режим](~/ios/watchos/platform/notifications.md), задайте `watchlaunchmode` параметр `Notification` и укажите путь к JSON-файл, содержащий полезные данные уведомления теста.

Параметр полезных данных *необходимые* для режим уведомления.

Например добавьте следующие аргументы для команды mtouch.

```bash
--watchlaunchmode=Notification --watchnotificationpayload=/path/to/file.json
```


## <a name="other-arguments"></a>Остальные аргументы

Далее описаны остальные аргументы.

### <a name="--sdkroot"></a>--sdkroot

Обязательно. Указывает путь к Xcode (6.2 или более поздней версии).

Пример

```bash
 --sdkroot /Applications/Xcode.app/Contents/Developer/
```

### <a name="--device"></a>--устройства

Имитатор устройство для выполнения. Здесь можно указать двумя способами: с помощью udid конкретного устройства, или используя сочетание среды выполнения и тип устройства.

Точные значения и зависит от между машинами, можно запросить, используя Apple **simctl** средство:

```bash
/Applications/Xcode.app/Contents/Developer/usr/bin/simctl list
```

**UDID**

Пример

```bash
--device=:v2:udid=AAAAAAA-BBBB-CCCC-DDDD-EEEEEEEEEEEE
```

**Время выполнения и тип устройства**

Пример

```bash
--device=:v2:runtime=com.apple.CoreSimulator.SimRuntime.iOS-8-2,devicetype=com.apple.CoreSimulator.SimDeviceType.iPhone-6
```



## <a name="related-links"></a>Связанные ссылки

- [WatchKitCatalog (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchKitCatalog/)
- [WatchTables (пример)](https://developer.xamarin.com/samples/monotouch/watchOS/WatchTables/)
