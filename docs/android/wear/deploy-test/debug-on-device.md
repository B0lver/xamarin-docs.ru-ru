---
title: Отладка в устройстве Android Wear
description: В этой статье объясняется, как выполнить отладку приложения Xamarin.Android Wear на устройстве Android Wear.
ms.prod: xamarin
ms.assetid: 01668E4B-BB83-4C26-B23A-F788173FB823
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/16/2018
ms.openlocfilehash: 816ec5c861b5889e1735eab6293ed10318c53644
ms.sourcegitcommit: 654df48758cea602946644d2175fbdfba59a64f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67831878"
---
# <a name="debug-on-a-wear-device"></a>Отладка в устройстве Android Wear

_В этой статье объясняется, как выполнить отладку приложения Xamarin.Android Wear на устройстве Android Wear._


## <a name="overview"></a>Обзор

При наличии на устройстве Android Wear, таких как Android Wear Smartwatch запуском приложения на устройстве, а не с помощью эмулятора. (Если вы еще не знакомы с процессом развертывания и запуска приложения на Android Wear, см. в разделе [Wear Hello,](~/android/wear/get-started/hello-wear.md).)

## <a name="prepare-the-wear-device"></a>Подготовка устройства износа:

Следуйте инструкциям ниже, чтобы включить отладку на устройстве Android Wear:

1.  Откройте **параметры** меню на устройстве Android Wear.

2.  В нижней части меню и коснитесь **о**.

3.  Выберите номер сборки 7 секунд.

4.  На **параметры** меню, выберите **параметры разработчика**.

5.  Убедитесь, что **отладки ADB** включен.


## <a name="debugging-over-usb"></a>Отладка по USB

Если на устройстве износа USB-порту, можно подключить износа устройство к компьютеру, развертывание в нем и запуска и отладки приложения так же, как телефон Android (Дополнительные сведения см. в разделе [отладка на устройстве](~/android/deploy-test/debugging/debug-on-device.md)).


## <a name="debugging-over-bluetooth"></a>Отладка через Bluetooth

Если устройство износа нет USB-порт, можно развернуть приложение на устройстве Android Wear через Bluetooth системой маршрутизации вывод отладки приложения на телефоне с Android, который подключен к компьютеру. 

### <a name="prepare-your-phone"></a>Подготовка телефона

Следуйте инструкциям ниже, чтобы подготовиться к установке подключений Bluetooth на устройстве Android Wear ваш телефон: 

1.  Если вы еще не сделано, настройте телефон для разработки приложений Xamarin.Android как описано в [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md).

2.  Скачайте и установите бесплатную [Android Wear](https://play.google.com/store/apps/details?id=com.google.android.wearable.app) приложение из Google Play Store.

### <a name="connect-the-device"></a>Подключите устройство

Следуйте инструкциям ниже, чтобы подключить устройство Wear на ваш телефон:

1.  На телефоне, который будет выступать в качестве промежуточных Bluetooth (настроенных выше), запустите приложение Android Wear. 

2.  Коснитесь **параметры** значок.

3.  Включить **отладка через Bluetooth**. Вы должны увидеть следующие состояние, отображаемое на экране приложения Android Wear:

        Host: disconnected
        Target: connected

4.  Подключите телефон к компьютеру через USB. На компьютере введите следующие команды:

    ```shell
    adb forward tcp:4444 localabstract:/adb-hub
    adb connect 127.0.0.1:4444
    ```

    Если порт 4444 недоступен, можно использовать любой другой доступный порт, к которому у вас есть доступ. 

    > [!NOTE]
    > При перезапуске Visual Studio или Visual Studio для Mac, необходимо выполнить эти команды еще раз, чтобы установить подключение к устройству одежды.

5.  При устройстве Android Wear появится запрос, подтвердите, что позволяет пользователям **отладки ADB**. В приложении Android Wear вы увидите, как изменить состояние:

        Host: connected
        Target: connected

6.  Выполнив описанные выше шаги, выполнение `adb devices` показано состояние телефона и устройства Android Wear:

        List of devices attached
        127.0.0.1:4444    device
        019ad61df0a69399  device

На этом этапе можно развернуть приложение на устройстве Android Wear.

<a name="screenshots" />

### <a name="taking-screenshots"></a>Делать снимки экрана

Можно сделать снимок экрана устройства одежды, введя следующую команду: 

```shell
adb -s 127.0.0.1:4444 shell screencap -p /sdcard/DCIM/screencap.png
```

Копирование снимка экрана на компьютер, введя следующую команду:

```shell
adb -s 127.0.0.1:4444 pull /sdcard/DCIM/screencap.png
```

Удалите снимок экрана на устройстве, введя следующую команду:

```shell
adb -s 127.0.0.1:4444 shell rm /sdcard/DCIM/screencap.png
```


### <a name="uninstalling-an-app"></a>Удаление приложения

Приложение можно удалить с устройства одежды, введя следующую команду:

```shell
adb -s 127.0.0.1:4444 uninstall <package name>
```

Например, чтобы удалить приложение с именем пакета `com.xamarin.weartest`, введите следующую команду:

```shell
adb -s 127.0.0.1:4444 uninstall com.xamarin.weartest
```

Дополнительные сведения об отладке устройств Android Wear через Bluetooth, см. в разделе [отладка через Bluetooth](https://developer.android.com/training/wearables/apps/bt-debugging.html).


## <a name="debugging-a-wear-app-with-a-companion-phone-app"></a>Отладка в приложение Wear с вспомогательное приложение для телефона

Приложения Android Wear упаковываются с помощью дополнительного приложения для телефона Android для распространения в Google Play (Дополнительные сведения см. в разделе [работа с упаковкой](~/android/wear/deploy-test/packaging.md)). Тем не менее вы по-прежнему разрабатывать приложение Wear и его сопутствующее приложение отдельно. После выпуска приложения через Google Play Store, приложение Wear будет упакован в состав сопутствующее приложение и автоматически устанавливается, если это возможно.

Отладка приложения одежды с дополнительного приложения: 

1.  Создавайте и развертывайте сопутствующее приложение на телефоне.

2.  Щелкните правой кнопкой мыши проект износа и задать его в качестве проекта запуска по умолчанию.

3.  Развертывание проекта износа носимого устройства.

4.  Выполнять и отлаживать приложение Wear на устройстве.

 
## <a name="summary"></a>Сводка

В этой статье описано, как настроить устройство Android Wear для износа отладки из Visual Studio через Bluetooth и как выполнить отладку приложения одежды с вспомогательное приложение для телефона. Он также предоставляются общие советы по отладке для отладки в приложение Wear через Bluetooth.
