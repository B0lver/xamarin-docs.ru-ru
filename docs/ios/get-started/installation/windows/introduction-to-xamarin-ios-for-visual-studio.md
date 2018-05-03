---
title: Введение в Xamarin.iOS для Visual Studio
description: В этой статье описывается сборка и тестирование приложений Xamarin iOS с помощью Visual Studio. В ней объясняется, как использовать Visual Studio для создания проектов iOS, сборки приложений iOS и их последующей компиляции, тестирования и отладки с помощью подключенного к сети компьютера Mac, на котором размещаются компилятор и симулятор Apple, и цепочки инструментов сборки Xamarin.
ms.prod: xamarin
ms.assetid: bf3c779f-959f-428d-babb-428f363f7e4e
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 859f84fbe329d79e9e1e6a8b7445fe2a7f03af23
ms.sourcegitcommit: dc6ccf87223942088ca926c0dadd5b5478c683cb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2018
---
# <a name="introduction-to-xamarinios-for-visual-studio"></a>Введение в Xamarin.iOS для Visual Studio

_В этой статье описано как выполнять сборку и тестирование приложений Xamarin iOS с помощью Visual Studio. Здесь также показано, как использовать Visual Studio для создания проектов iOS, сборки приложений iOS и их последующей компиляции, тестирования и отладки с помощью подключенного к сети компьютера Mac, на котором размещаются компилятор и симулятор Apple, и цепочки инструментов сборки Xamarin._

Xamarin для Windows позволяет разрабатывать и тестировать приложения iOS в Visual Studio с использованием подключенного к сети компьютера Mac, который предоставляет службу сборки и развертывания.

В этой статье рассматриваются действия по установке и настройке средств Xamarin.iOS на каждом компьютере, предназначенном для создания приложений iOS с помощью Visual Studio.

Разработка приложений для iOS в Visual Studio дает ряд преимуществ:

-  создание кроссплатформенных решений для приложений iOS, Android и Windows;
-  использование любимых средств Visual Studio (таких как **Resharper** и **Team Foundation Server**) для всех кроссплатформенных проектов, включая исходный код iOS;
-  работа в знакомой интегрированной среде (IDE) с использованием привязок Xamarin.iOS ко всем интерфейсам API Apple.


<a name="Requirements_and_Installation" />

## <a name="requirements-and-installation"></a>Требования и установка

При разработке приложений для iOS в Visual Studio должен выполняться ряд требований. Как было кратко упомянуто в обзоре, для компиляции файлов IPA требуется компьютер Mac, а приложения невозможно развертывать на устройствах без сертификатов и средств подписывания кода Apple.

Доступно несколько конфигураций, из которых вы можете выбрать наиболее подходящую под ваши потребности. Эти конфигурации перечислены ниже:

-  Компьютер Mac используется в качестве главного компьютера разработки, а среда Visual Studio установлена на виртуальной машине Windows. Мы рекомендуем использовать для создания виртуальной машины такое программное обеспечение, как [Parallels](http://www.parallels.com/products/desktop/) или [VMWare](http://www.vmware.com/products/fusion/).
-  Компьютер Mac используется только в качестве узла сборки. В этом сценарии он подключается к той же сети, в которой находится компьютер Windows с установленными [необходимыми](~/cross-platform/get-started/installation/windows.md#installation) средствами.


В любом случае необходимо выполнить указанные ниже действия:

- [Установка Visual Studio для Mac](https://docs.microsoft.com/visualstudio/mac/installation)
- [Установите средства Xamarin в Windows](~/cross-platform/get-started/installation/windows.md)

## <a name="connecting-to-the-mac"></a>Подключение к компьютеру Mac

Для подключения Visual Studio к узлу сборки Mac выполните инструкции в руководстве [Связывание с компьютером Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).



## <a name="visual-studio-toolbar-overview"></a>Обзор панели инструментов Visual Studio

При установке Xamarin iOS для Visual Studio добавляются элементы на стандартную панель инструментов и на новую панель инструментов iOS.
Назначение этих панелей инструментов описывается ниже.

### <a name="standard-toolbar"></a>Стандартная панель инструментов

Элементы управления, имеющие отношение к разработке приложений iOS с помощью Xamarin, обведены красным:

 [![](introduction-to-xamarin-ios-for-visual-studio-images/03.png "Элементы управления, имеющие отношение к разработке приложений с помощью Xamarin iOS, обведены красным")](introduction-to-xamarin-ios-for-visual-studio-images/03.png#lightbox "Элементы управления, имеющие отношение к разработке приложений с помощью Xamarin iOS, обведены красным")

-  **Запуск** — запускает отладку или выполнение приложения на выбранной платформе. Должен быть подключен компьютер Mac (см. индикатор состояния на панели инструментов iOS).
-  **Конфигурации решения** — позволяет выбрать нужную конфигурацию (например, отладка или выпуск).
-  **Платформы решения** — позволяет выбрать iPhone или iPhoneSimulator в качестве платформы развертывания.


### <a name="ios-toolbar"></a>Панель инструментов iOS

Панель инструментов iOS выглядит антологичным образом во всех версиях Visual Studio. Она показана ниже:

[![](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png "Панель инструментов iOS")](introduction-to-xamarin-ios-for-visual-studio-images/iostoolbar.png#lightbox)

Далее описывается каждый ее элемент:

-  **Mac Agent или диспетчер подключений** — открывает диалоговое окно "Xamarin Mac Agent". Во время подключения этот значок будет *оранжевым*, а после установления подключения — *зеленым*.
-  **Показать симулятор iOS** — окно симулятора iOS на компьютере Mac открывается на переднем плане.
-  **Показать IPA-файл на сервере сборки** — на компьютере Mac открывается программа Finder в месте, где находится выходной файл IPA приложения.



## <a name="ios-output-options"></a>Параметры вывода iOS


### <a name="output-window"></a>Окно выходных данных

Это параметры в области *Вывод*, с помощью которых можно просматривать сообщения и ошибки, связанные со сборкой, развертыванием и подключением.

На снимке экрана ниже показаны доступные окна вывода, которые могут отличаться в зависимости от типа проекта:

[![](introduction-to-xamarin-ios-for-visual-studio-images/output-sml.png "Доступные окна вывода")](introduction-to-xamarin-ios-for-visual-studio-images/output-large.png#lightbox)

- **Xamarin** — содержит сведения, относящиеся исключительно к Xamarin, например о подключении к компьютеру Mac и состоянии активации.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output3-sml.png "Сведения, относящиеся исключительно к Xamarin, например о подключении к компьютеру Mac и состоянии активации")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

- **Диагностика Xamarin** — содержит более подробные сведения о проекте Xamarin, например о взаимодействии с Android.

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output4-sml.png "Подробные сведения о проекте Xamarin")](introduction-to-xamarin-ios-for-visual-studio-images/output3-large.png#lightbox)

Другие области вывода Visual Studio, такие как "Отладка" и "Сборка", также доступны в представлении "Вывод" и служат для отображения выходных данных отладки и MSBuild:

-  **Отладка**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output2-sml.png "Выходные данные отладки")](introduction-to-xamarin-ios-for-visual-studio-images/output2-large.png#lightbox)

- **Сборка** & **Порядок сборки**

    [![](introduction-to-xamarin-ios-for-visual-studio-images/output1-sml.png "Выходные данные MSBuild")](introduction-to-xamarin-ios-for-visual-studio-images/output1-large.png#lightbox)


## <a name="ios-project-properties"></a>Свойства проекта iOS

Чтобы получить доступ к свойствам проекта Visual Studio, можно щелкнуть его имя правой кнопкой мыши и выбрать в контекстном меню пункт *Свойства*. Это позволит настроить приложение iOS, как показано на снимке экрана ниже:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosproperties.png "Настройка приложения iOS")

-  *Подписывание пакета iOS* — подключение к компьютеру Mac для заполнения удостоверений подписывания кода и профилей подготовки:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/bundlesigning.png "Заполнение удостоверений подписывания кода и профилей подготовки")

-  *Параметры IPA iOS* — файл IPA сохраняется в файловой системе Mac:


 ![](introduction-to-xamarin-ios-for-visual-studio-images/ipaoptions.png "Параметры IPA iOS")

-  *Параметры запуска iOS* — настройка дополнительных параметров:

 ![](introduction-to-xamarin-ios-for-visual-studio-images/iosrunoptions.png "Параметры запуска iOS")



## <a name="creating-a-new-project-for-ios-applications"></a>Создание проекта для приложений iOS

Проект iOS создается в Visual Studio так же, как проект любого другого типа. Выберите пункт меню **Файл > Новый проект**, чтобы открыть показанное ниже диалоговое окно, на котором представлены некоторые доступные шаблоны для создания проекта iOS:


![](introduction-to-xamarin-ios-for-visual-studio-images/newproject.png "Создание проекта")

Раскадровку и файлы XIB можно редактировать в Visual Studio с помощью iOS Designer. Чтобы создать раскадровку, выберите один из шаблонов раскадровки. В **обозревателе решений** будет создан файл **Main.storyboard**, как показано на снимке экрана ниже.

![](introduction-to-xamarin-ios-for-visual-studio-images/solution-explorer-new.png "Файл Main.storyboard в обозревателе решений")

Чтобы приступить к созданию или редактированию раскадровки, дважды щелкните файл `Main.storyboard`. Он откроется в iOS Designer:

![](introduction-to-xamarin-ios-for-visual-studio-images/iosdesigner.png "Файл Main.storyboard в конструкторе iOS")

Чтобы добавить объекты в представление, перетащите их из области **Панель элементов** в область конструктора. Если панель элементов еще не добавлена, это можно сделать, выбрав пункт меню **Вид > Панель элементов**. С помощью области **Свойства** можно изменять свойства объектов, настраивать их макеты и создавать события, как показано ниже:

![](introduction-to-xamarin-ios-for-visual-studio-images/properties.png "Область свойств")

 Дополнительные сведения об использовании конструктора iOS см. в посвященных [конструктору](~/ios/user-interface/designer/index.md) руководствах.


## <a name="running--debugging-ios-applications"></a>Запуск и отладка приложений iOS

### <a name="device-logging"></a>Ведение журнала устройства

В Visual Studio 2017 используются единые панели журналов Android и iOS.

Журналы для устройств Android и iOS можно просматривать в новом окне средства журнала устройств для Visual Studio. Чтобы открыть это окно, нужно выполнить одну из указанных ниже последовательностей команд:

- **Вид > Другие окна > Журнал устройств**
- **Сервис > iOS > Журнал устройств**
- **Панель инструментов iOS > Журнал устройств**

Когда окно средства откроется, пользователь может выбрать физическое устройство в раскрывающемся списке устройств. После выбора устройства журналы автоматически добавляются в таблицу. При переключении между устройствами ведение журнала устройств останавливается и запускается.

Чтобы устройства отображались в поле со списком, проект iOS должен быть загружен. Кроме того, для обнаружения устройств iOS, подключенных к компьютеру Mac, среда Visual Studio должна быть [подключена к серверу Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

В этом окне находятся следующие элементы: таблица записей журнала, раскрывающийся список для выбора устройств, функция очистки записей журнала, поле поиска и кнопки воспроизведения, остановки, паузы.


### <a name="set-debugging-stops"></a>Остановка выполнения для отладки

В любых местах приложения можно установить точки останова, которые предписывают отладчику временно остановить выполнение программы. Чтобы задать точку останова в Visual Studio, щелкните область поля редактора рядом с номером строки кода, где требуется прервать выполнение:

![](introduction-to-xamarin-ios-for-visual-studio-images/image18.png "Задание точки отладки")

Начните отладку и перейдите к точке останова в приложении в симуляторе или на устройстве. При достижении точки останова строка выделяется и становятся доступны стандартные возможности отладки Visual Studio: вы можете выполнять код пошагово с заходом, обходом или выходом, просматривать локальные переменные или пользоваться окном "Интерпретация".

На этом снимке экрана показан симулятор iOS, выполняющийся вместе со средой Visual Studio с использованием ПО Parallels в OS X:


![](introduction-to-xamarin-ios-for-visual-studio-images/image19.png "На этом снимке экрана показан симулятор iOS, выполняющийся вместе со средой Visual Studio с использованием ПО Parallels в OS X")

### <a name="examine-local-variables"></a>Просмотр локальных переменных

![](introduction-to-xamarin-ios-for-visual-studio-images/image20.png "Просмотр локальных переменных во время отладки")

## <a name="summary"></a>Сводка

В этой статье было описано, как использовать Xamarin.iOS для Visual Studio. В ней были перечислены различные функции, доступные для создания, сборки и тестирования приложения iOS в Visual Studio, и приведены пошаговые инструкции по созданию и отладке простейшего приложения iOS.

## <a name="related-links"></a>Связанные ссылки

- [Установка Xamarin.iOS](~/ios/get-started/installation/windows/index.md)
- [Подготовка устройств](~/ios/get-started/installation/device-provisioning/index.md)
- [Создание пользовательского интерфейса iOS в коде](~/ios/app-fundamentals/ios-code-only.md)
- [Подключение компьютера Mac к среде Visual Studio с помощью XMA (видео)](https://university.xamarin.com/lightninglectures/xamarin-mac-agent)
