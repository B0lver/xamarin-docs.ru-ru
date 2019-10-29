---
title: Установка и использование watchOS в Xamarin
description: В этом документе описывается установка и использование watchOS с Xamarin. В нем обсуждается установка, структура проекта watchOS, использование конструктора iOS, интеграция Xcode и рекомендации по устранению неполадок.
ms.prod: xamarin
ms.assetid: 69F21F15-198D-4B42-A703-21D35CAB0CCA
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 12/05/2017
ms.openlocfilehash: f986099011dbccb0eb43c62d253ee497d46ca08e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73001686"
---
# <a name="installing-and-using-watchos-in-xamarin"></a>Установка и использование watchOS в Xamarin

для watchOS 4 требуется macOS Sierra (10,12) с Xcode 9.

watchOS 1 первоначально требовал OS X Yosemite (10,10) с Xcode 7.

> [!WARNING]
> [обновления watchOS 1 не будут приниматься после 1 апреля 2018](https://developer.apple.com/news/?id=11162017a). В будущих обновлениях должен использоваться пакет SDK для watchOS 2 или более поздней версии. рекомендуется сборка с помощью пакета SDK для watchOS 4.

## <a name="project-structure"></a>Структура проекта

Приложение-наблюдатель состоит из трех проектов:

- **Проект приложения Xamarin. iOS iPhone** — это стандартный проект iPhone, который может быть любым из шаблонов Xamarin. iOS. Приложение Watch и его расширение будут объединены в этот главный проект.

- **Просмотр проекта расширения** — содержит код (например, классы контроллеров) для приложения Watch.

- **Просмотр проекта приложения** — содержит файл раскадровки пользовательского интерфейса со всеми ресурсами пользовательского интерфейса для приложения Watch.

Пример решения " [Контрольный образец каталога](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) " в Xamarin. Studio выглядит следующим образом:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](installation-images/catalog-solution.png "The solution in Visual Studio")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/catalog-solution-vs.png "The solution in Visual Studio")

-----

Скачайте и запустите пример [ватчкиткаталог](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog) , чтобы приступить к работе.
Экраны из примера можно найти на странице [элементы управления](~/ios/watchos/user-interface/index.md) .

## <a name="creating-a-new-project"></a>Создание нового проекта

Невозможно создать новое "Контрольное решение"... Вместо этого можно добавить приложение Watch в существующее приложение iOS. Чтобы создать приложение наблюдения, выполните следующие действия.

1. Если у вас нет проекта, сначала выберите **файл > создать решение** и создайте приложение iOS (например, **одно приложение для просмотра**):

    [![](installation-images/cycle8-2-sml.png "Choose File > New Solution and create an iOS app")](installation-images/cycle8-2.png#lightbox)

2. Когда приложение iOS будет создано (или вы планируете использовать существующее приложение iOS), щелкните решение правой кнопкой мыши и выберите **добавить > добавить новый проект..** . В окне **Новый проект** выберите **watchOS > приложение > приложение WatchKit**:

    [![](installation-images/cycle8-6-sml.png "Select watchOS > App > WatchKit App")](installation-images/cycle8-6.png#lightbox)

3. На следующем экране вы можете выбрать проект приложения iOS, который должен содержать приложение Watch:

    [![](installation-images/cycle8-7-sml.png "Choose which iOS app project should include the watch app")](installation-images/cycle8-7.png#lightbox)

4. Наконец, выберите расположение для сохранения проекта (и при необходимости включения системы управления версиями):

    [![](installation-images/cycle8-8-sml.png "Choose the location to save the project")](installation-images/cycle8-8.png#lightbox)

5. Visual Studio для Mac автоматически настраивает [ссылки на проект и параметры **info. plist** ](~/ios/watchos/get-started/project-references.md) .

## <a name="creating-the-watch-user-interface"></a>Создание пользовательского интерфейса Watch

<a name="designer" />

### <a name="using-the-xamarin-ios-designer"></a>Использование Xamarin iOS Designer

Дважды щелкните интерфейс Watch приложения **. раскадровка** для редактирования с помощью конструктора iOS. Вы можете перетащить контроллеры интерфейса и элементы управления пользовательского интерфейса на раскадровку из **панели элементов** и настроить их с помощью панели **свойств** :

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

[![](installation-images/iosdesigner-sml.png "The storyboard in the Designer")](installation-images/iosdesigner.png#lightbox)

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](installation-images/iosdesigner-sml-vs.png "The storyboard in the Designer")](installation-images/iosdesigner-vs.png#lightbox)

-----

Каждый новый контроллер интерфейса следует назначить **классу** , выбрав его, а затем введя имя на панели **свойств** (это автоматически создаст необходимые C# файлы фонового кода):

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

![](installation-images/iosdesigner-classname.png "Give each new interface controller a Class")

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

![](installation-images/iosdesigner-classname-vs.png "Give each new interface controller a Class")

-----

Создайте переходов, нажав **CTRL + перетаскивание** из кнопки, таблицы или интерфейса на другой контроллер интерфейса.

### <a name="using-xcode-on-the-mac"></a>Использование Xcode на компьютере Mac

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

Вы можете продолжить использовать Xcode для создания пользовательского интерфейса, щелкнув файл Interface. Storyboard правой кнопкой мыши и выбрав пункт **Открыть с помощью > Xcode Interface Builder**:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

Пользователи Visual Studio также могут использовать Xcode для создания пользовательского интерфейса путем переключения на узел сборки Mac напрямую.
Откройте решение в Visual Studio для Mac, щелкните правой кнопкой мыши файл Interface. Storyboard и выберите **Открыть с помощью > Xcode Interface Builder**:

-----

![](installation-images/openwith-xcode.png "Open the Interface.storyboard in Xcode Interface Builder")

При использовании Xcode необходимо выполнить те же действия для просмотра приложений, что и для обычных [раскадровок приложений iOS](~/ios/user-interface/storyboards/index.md) (например, создание розеток и действий путем **перетаскивания мышью** в **h** -файл заголовка).

При сохранении раскадровки в Xcode Interface Builder она автоматически добавляет значения и действия, которые вы создаете C# , в **Designer.CS** файлы в проекте расширения Watch.

### <a name="adding-additional-screens-in-xcode"></a>Добавление дополнительных экранов в Xcode

При добавлении дополнительных экранов (помимо включенного в шаблон по умолчанию) в раскадровку с помощью Xcode Interface Builder **необходимо вручную добавить файлы C# кода** для каждого нового контроллера интерфейса.

[Дополнительные инструкции по добавлению новых контроллеров интерфейса в раскадровку](~/ios/watchos/troubleshooting.md#add)см. в этой статье.

*В конструкторе Xamarin iOS это происходит автоматически, никаких действий вручную не требуется.*

## <a name="building"></a>Сборка

Проект, включающий в себя приложение для просмотра контрольных значений, как и другие проекты iOS. Процесс сборки приведет к созданию приложения iPhone (. app), которое содержит расширение Watch (. аппекс), которое, в свою очередь, будет содержать приложение с контрольными числами без кода (. app).

## <a name="launching"></a>Пустив

Вы можете запустить Просмотр приложений в симуляторе с помощью Visual Studio для Mac или Visual Studio (начинается на узле сборки Mac).

Существует два режима запуска приложения WatchKit:

- нормальный режим приложения (по умолчанию) и
- [Уведомления](~/ios/watchos/platform/notifications.md) (для которых требуются полезные данные тестовых уведомлений в формате JSON).

### <a name="xcode-8-support"></a>Поддержка Xcode 8

После установки Xcode 8 (или более поздней версии) Apple Watch симуляторы отделены от симуляторов iOS (в отличие от [Xcode 6](#xcode6), где они отображались как *Внешние*).
Выбрав проект Watch App и сделав его запускаемым, в списке симуляторов отобразятся *симуляторы iOS* для выбора (как показано ниже).

[![](installation-images/xs-xcode8-watchos3-sml.png "Selecting the Simulator type")](installation-images/xs-xcode8-watchos3.png#lightbox)

При запуске отладки необходимо запустить *два* симулятора — симулятор iOS *и* симулятор Apple Watch. Используйте **команду + Shift + H** для перехода к меню Watch и циферблату часов; и используйте меню **оборудование** для установки **Force Touch давления**. Прокрутка трекпада или мыши имитируется с помощью Digital Crown.

#### <a name="troubleshooting"></a>Устранение неполадок

Следующая ошибка появится в **выходных данных приложения** при попытке запуска в симуляторе, для которого нет парных часов:

```csharp
error MT0000: Unexpected error - Please file a bug report at https://github.com/xamarin/xamarin-macios/issues/new
error HE0020: Could not find a paired Watch device for the iOS device 'iPhone 6'.
```

Инструкции по настройке симуляторов, если значения по умолчанию не работают, см. на [форумах Apple](https://forums.developer.apple.com/thread/7783) .

<a name="xcode6" />

### <a name="xcode-6-and-watchos-1"></a>Xcode 6 и watchOS 1

Перед запуском или отладкой приложения необходимо сделать *проект расширения контрольных значений* проектом **запуска** . Вы не можете «запустить» само приложение, а если выбрать приложение iOS, оно будет запускаться в симуляторе iOS обычным образом.

По умолчанию приложение наблюдения запускается в нормальном режиме **приложения** (не в режиме "без кратких сообщений" или "уведомления") от команд **запуска** или **отладки** Visual Studio для Mac.

При использовании Xcode 6 только устройства iPhone 5, iPhone 5S, iPhone 6 и iPhone 6 Plus могут активировать внешний экран либо для **Apple Watch-38** , либо для **Apple Watch-часы** , где будут отображаться приложения для просмотра.

> [!NOTE]
> Помните, что экран контрольных значений не отображается автоматически в симуляторе iOS при использовании Xcode 6.
> Используйте меню **оборудование > внешний экран** , чтобы отобразить экран контрольных значений.

<a name="custommodes" />

## <a name="launching-notification-mode"></a>Запуск режима уведомления

Сведения об обработке уведомлений в коде см. на [странице уведомлений](~/ios/watchos/platform/notifications.md) .

Visual Studio для Mac можете запустить приложение Watch с _режимами запуска_ уведомлений для уведомлений:

Щелкните проект Watch App правой кнопкой мыши и выберите пункт **выполнить с > пользовательской конфигурации...** :

[![](installation-images/runwith-customparams-sml.png "Running a Custom Configuration")](installation-images/runwith-customparams.png#lightbox)

Откроется окно **Настраиваемые параметры** , где можно выбрать **уведомление** (и предоставить полезные данные JSON), а затем нажмите кнопку **запустить** , чтобы запустить приложение Watch в симуляторе:

[![](installation-images/runwith-execargs-sml.png "Setting the Notification and Payload")](installation-images/runwith-execargs.png#lightbox)

## <a name="debugging"></a>Отладка

Отладка поддерживается как в Visual Studio для Mac, так и в Visual Studio.
При отладке в режиме уведомлений не забудьте указать JSON-файл уведомления. На этом снимке экрана показана точка останова отладки в приложении Watch:

![](installation-images/debug-sml.png "This screenshot shows a debug breakpoint being hit in a watch app")

После выполнения инструкций по запуску вы получите контрольное приложение, работающее в **симуляторе iOS (контрольные значения)** .
Для режима уведомления можно выбрать **отладка > открыть системный журнал** (**cmd +/** ) и использовать в коде `Console.WriteLine`.

### <a name="debugging-lifecycle-event-handlers"></a>Отладка обработчиков событий жизненного цикла

<!--
To test the functionality in your  and 
  methods, use the **Hardware > Lock** command in the iOS Simulator.
  Locking will trigger the `DidDeactivate` method and the watch simulator
  will indicate that it has been locked. Swipe the iOS Simulator to unlock,
  which triggers the `WillActivate` method of the watch app.
-->

Файлы шаблонов watchOS (такие как `InterfaceController`, `ExtensionDelegate`, `NotificationController`и `ComplicationController`) поставляются с уже реализованными необходимыми методами жизненного цикла. Добавьте `Console.WriteLine` вызовы и прочтите **выходные данные приложения** , чтобы лучше понять жизненный цикл событий.

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Первое видео о приложении](https://blog.xamarin.com/your-first-watch-kit-app/)
- [Советы по WatchKit Apple](https://developer.apple.com/watchkit/tips/)
