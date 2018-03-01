---
title: "Установка Xamarin для Visual Studio в Windows"
ms.topic: article
ms.prod: xamarin
ms.assetid: E20D4463-368E-4B60-A059-F50DB8C5552D
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 09/29/2017
ms.openlocfilehash: b68e03251b83192bdc5836af6ea54446ddaad24a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="installing-xamarin-in-visual-studio-on-windows"></a>Установка Xamarin для Visual Studio в Windows

Поскольку теперь Xamarin является бесплатным компонентом всех выпусков Visual Studio и не требует отдельной лицензии, установщик Visual Studio можно использовать для скачивания и установки средств Xamarin.

-   [Requirements](#requirements)
-   [Установка](#installation)
-   [Добавление Xamarin в Visual Studio 2017](#vs2017)
-   [Добавление Xamarin в Visual Studio 2015](#vs2015)
-   [Проверка установки](#verifying)
-   [Следующие шаги](#nextsteps)


<a name="requirements" />

# <a name="requirements"></a>Требования

Ниже приведены компоненты, необходимые для установки инструментов Visual Studio для Xamarin:

1. Windows 7 или более поздней версии.

2. Visual Studio 2015 или 2017 (Community, Professional или Enterprise).

3. Xamarin для Visual Studio.

Обратите внимание, что Xamarin не может использоваться с экспресс-выпусками Visual Studio из-за отсутствия поддержки подключаемого модуля.

Дополнительные сведения о необходимых компонентах для установки и использования Xamarin см. в статье [Требования к системе](~/cross-platform/get-started/requirements.md).


<a name="installation" />

# <a name="installation"></a>Установка

Xamarin можно установить в составе новой установки Visual Studio.
Для этого выполните следующие действия:

1. Скачайте Visual Studio Community, Visual Studio Professional или Visual Studio Enterprise со страницы [Visual Studio](https://www.visualstudio.com/vs/) (ссылки для скачивания приведены в нижней части).

2. Дважды щелкните скачанный пакет, чтобы начать установку.

3. На экране установки выберите рабочую нагрузку **Разработка мобильных приложений на платформе .NET**: 

    [![Выбор рабочей нагрузки "Разработка мобильных приложений на платформе .NET"](windows-images/01-mobile-dev-workload-sml.png)](windows-images/01-mobile-dev-workload.png)

4. Установив флажок **Разработка мобильных приложений на платформе .NET**, взгляните на панель **Сводка** справа. Здесь можно отменить выбор параметров разработки мобильных приложений, которые вам не нужны. По умолчанию устанавливаются все компоненты, показанные на следующем снимке экрана (**Xamarin Workbooks**, **Xamarin Profiler**, **Xamarin Remoted Simulator**, **пакет NDK для Android**, **пакета SDK для Android**, **пакет Java SE Development Kit**, **эмулятор Android Google**, **поддержка F #** и **Intel HAXM**):

    ![Панель "Сводка" со списком устанавливаемых компонентов Xamarin](windows-images/02-summary.png)

5. Если вы готовы начать установку Visual Studio, нажмите кнопку **Установить** в правом нижнем углу:

    ![Расположение кнопки установки](windows-images/03-click-install.png)

   В зависимости от устанавливаемого выпуска Visual Studio процесс установки может занять длительное время. Ход выполнения установки можно отслеживать с помощью индикаторов:

    ![Пример снимка экрана с индикаторами выполнения во время установки](windows-images/04-progress-bars.png)

6. После установки Visual Studio нажмите кнопку **Запустить**, чтобы запустить Visual Studio:

    ![Расположение кнопки запуска](windows-images/05-launch.png)


<a name="vs2017" />

## <a name="adding-xamarin-to-visual-studio-2017"></a>Добавление Xamarin в Visual Studio 2017

Если среда Visual Studio 2017 уже установлена, Xamarin можно добавить путем повторного запуска установщика Visual Studio для изменения рабочих нагрузок (дополнительные сведения см. в статье [Изменение Visual Studio](https://docs.microsoft.com/visualstudio/install/modify-visual-studio)). Затем следует выполнить приведенные выше действия по установке Xamarin.

Дополнительные сведения о скачивании и установке Visual Studio 2017 см. в статье [Установка Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio).


<a name="vs2015" />

## <a name="adding-xamarin-to-visual-studio-2015"></a>Добавление Xamarin в Visual Studio 2015

Чтобы добавить Xamarin.Android в существующую установку Visual Studio 2015, выполните следующие действия:

1. Щелкните правой кнопкой мыши кнопку Windows **Пуск** и выберите **Программы и компоненты**.

2. Щелкните правой кнопкой мыши **Microsoft Visual Studio** и выберите пункт **Изменить**.

3. В появившемся диалоговом окне установщика Visual Studio нажмите кнопку **Изменить**.

4. На вкладке **Компоненты** прокрутите до раздела **Разработка кроссплатформенных мобильных приложений**. Установите флажок **C#/.NET (Xamarin)**:

    ![Добавление C#/.NET Xamarin в Visual Studio 2015](windows-images/06-add-xamarin.png)

5. Нажмите кнопку **Обновить**, чтобы добавить Xamarin в Visual Studio.


<a name="verifying" />

## <a name="verifying-installation"></a>Проверка установки

Чтобы проверить установку Xamarin в Visual Studio 2017, щелкните меню **Справка**. Если Xamarin установлен, вы увидите пункт меню **Xamarin**, как показано на снимке экрана:

![Пункт меню "Xamarin" в меню "Справка"](windows-images/12-xamarin-menu-item.png)

Если вы используете более ранние версии Visual Studio, выберите **Справка > О Microsoft Visual Studio** и в списке установленных продуктов проверьте наличие или отсутствие Xamarin:

![Экран установленных продуктов Visual Studio](windows-images/13-xamarin-is-installed.png)

Дополнительные сведения о поиске данных о версии см. в статье [Где можно найти сведения о версии и журналы?](~/cross-platform/troubleshooting/questions/version-logs.md)

<a name="nextsteps" />

# <a name="next-steps"></a>Следующие шаги

Установка инструментов Visual Studio для Xamarin позволяет приступить к написанию кода для приложений, но требует дополнительной настройки для создания и развертывания приложений в симуляторе, эмуляторе и на устройствах. Сведения в приведенных далее руководствах помогут завершить установку и начать создавать кроссплатформенные приложения.

## <a name="ios"></a>iOS

Дополнительные сведения см. в руководстве [Установка Xamarin.iOS в Windows](~/ios/get-started/installation/windows/index.md). 

1. [Установка инструментов Xamarin.iOS на компьютере Mac](~/ios/get-started/installation/windows/index.md#installation)
2. [Настройка компьютера Mac](~/ios/get-started/installation/windows/index.md#configuration)
3. [Настройка для разработки на базе iOS](~/ios/get-started/installation/windows/index.md#developersetup) (для запуска приложения на устройстве).
4. [Подключение Visual Studio к узлу сборки Mac](~/ios/get-started/installation/windows/index.md#connectingtomac)
5. [Удаленный симулятор iOS](~/tools/ios-simulator.md)
6. [Введение в Xamarin.iOS для Visual Studio](~/ios/get-started/installation/windows/introduction-to-xamarin-ios-for-visual-studio.md)

## <a name="android"></a>Android

Дополнительные сведения см. в руководстве [Установка Xamarin.Android в Windows](~/android/get-started/installation/windows.md).

1. [Конфигурация Xamarin.Android](~/android/get-started/installation/windows.md#configuration)
2. [Использование диспетчера пакетов SDK Android для Xamarin](~/android/get-started/installation/android-sdk.md?ide=vs)
3. [Эмулятор SDK для Android](~/android/get-started/installation/android-emulator/index.md)
4. [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md)
