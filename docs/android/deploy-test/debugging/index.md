---
title: Отладка Xamarin.Android на устройствах и эмуляторах
description: Тестирование и отладка приложения Xamarin.Android
ms.prod: xamarin
ms.assetid: A355A471-8195-4391-93FE-0000BCB17923
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 04/25/2018
ms.openlocfilehash: a4ce36e28bd5b6dcf78d248b1f2ba951cad9b286
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/10/2018
---
# <a name="debugging"></a>Отладка

В этом разделе описываются принципы отладки приложения Xamarin.Android на устройствах или эмуляторах.
## <a name="debugging-overview"></a>Общие сведения об отладке

При разработке приложений Android приложения требуется запускать либо на физическом оборудовании, либо с помощью эмулятора. Использование оборудования является лучшим, но не всегда самым целесообразным вариантом. Во многих случаях может быть проще и рентабельнее смоделировать или сэмулировать оборудование Android с помощью одного из описанных ниже эмуляторов.


### <a name="google-android-emulatorandroiddeploy-testdebuggingandroid-sdk-emulatorindexmd"></a>[Эмулятор Google Android](~/android/deploy-test/debugging/android-sdk-emulator/index.md)

В этой статье рассматривается использование эмулятора по умолчанию, который предоставляется с пакетом SDK для Android. Этот эмулятор доступен для Visual Studio для Windows и Visual Studio для Mac.

### <a name="visual-studio-android-emulatorandroiddeploy-testdebuggingvisual-studio-android-emulatormd"></a>[Эмулятор Visual Studio для Android](~/android/deploy-test/debugging/visual-studio-android-emulator.md)

В этой статье описывается отладка и тестирование приложения Xamarin.Android с помощью эмулятора Android, встроенного в Visual Studio 2015. Он хорошо подходит при использовании Visual Studio 2015 и отсутствии необходимости в пользовательских профилях устройств.

### <a name="debugging-on-a-deviceandroiddeploy-testdebuggingdebug-on-devicemd"></a>[Отладка на устройстве](~/android/deploy-test/debugging/debug-on-device.md)

В этой статье содержатся сведения о настройке физического устройства Android для развертывания на нем приложения Xamarin.Android непосредственно из Visual Studio или Visual Studio для Mac.

### <a name="android-debug-logandroiddeploy-testdebuggingandroid-debug-logmd"></a>[Журнал отладки Android](~/android/deploy-test/debugging/android-debug-log.md)

Очень часто разработчики для отладки своих приложений используют `Console.WriteLine`. Однако на мобильной платформе, такой как Android, консоль отсутствует. На устройствах Android доступен журнал, который, скорее всего, потребуется вам при создании приложения. Иногда его называют **logcat** из-за команды, вводимой для его получения. Из этой статьи вы узнаете, как использовать **logcat**.

> [!WARNING]
> Обратите внимание, что **Xamarin Android Player** применять не рекомендуется. Дополнительные сведения см. в [объявлении в этой записи блога](https://blog.xamarin.com/live-from-dotnetconf-cycle-7-xamarin-studio-6-and-more/).
