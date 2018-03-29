---
title: Приложение динамической проигрыватель Xamarin
description: Изменение и тестирование приложений в режиме реального времени на устройства iOS или Android
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: d666bb9a7749845265d50b86e46ef78e2036cb69
ms.sourcegitcommit: 17a9cf246a4d33cfa232016992b308df540c8e4f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/29/2018
---
# <a name="xamarin-live-player-app"></a>Приложение динамической проигрыватель Xamarin

![Функция предварительного просмотра](~/media/shared/preview.png)

## <a name="get-the-app"></a>Получение приложения

# <a name="androidtabandroid"></a>[Android](#tab/android)

Xamarin Live Player доступен для Android в Google Play:

[ ![Доступно на Google Play](images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Для устройств Android без Google Play Xamarin Live Player доступна через [HockeyApp](https://aka.ms/xlp-hockeyapp) распространения. Кроме того, предварительной версии сборки для Android можно установить непосредственно из Google Play с включением защиты для [открыть бета-версии программы](https://play.google.com/apps/testing/com.xamarin.live)

# <a name="iostabios"></a>[iOS](#tab/ios)

Мы рекомендуем пользователям присоединяться к [приложения Xamarin Live Player _iOS предварительного просмотра_ ](https://aka.ms/liveplayeralpha) более быстрый доступ к последним усовершенствованиям через TestFlight.

-----

## <a name="using-the-app"></a>С помощью приложения

После установки приложения на телефоне, выполните [инструкции по установке](~/tools/live-player/install.md) для подключения к компьютеру. Попробуйте выполнить одно из [образец приложения](~/tools/live-player/samples.md) для его запуска.

Во время запуска приложения Xamarin Live Player выглядит следующим образом (в iOS и Android соответственно):

![Live проигрывателя снимка экрана приложения iOS](player-images/app-iphone-sml.png) ![Динамическая Player Android снимка экрана приложения](player-images/app-android-sml.png)

При нажатии клавиши **пару, чтобы Visual Studio**, использовать камеру для сканирования штрих-кода, показывающий на вашем компьютере:

![Снимок экрана сканер штрих-кодов iOS](player-images/scan-iphone-sml.png) ![Снимок экрана сканер штрихкодов Android](player-images/scan-android-sml.png)

Если соединение установлено успешно, код должен выполняться на устройстве практически сразу (такие как [образец калькулятора](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Пример приложения калькулятора, запущенного на устройстве](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Параметры

Нажмите кнопку "Сведения" **(i)** в нижней части приложения, чтобы раскрыть **параметры** меню:

[ ![Снимок экрана: меню «Параметры»](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Журналы

Просмотр журналов для диагностики проблем.

### <a name="settings"></a>Параметры

* Показать или скрыть ошибки компиляции и времени выполнения.
* Сведения о версии.
* Отправьте отзыв.

[ ![Снимок экрана: параметры](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Управление устройствами

Для подключения к устройству в первый раз, следуйте инструкциям в [требования и настройка](~/tools/live-player/install.md). Можно связать несколько устройств (например, iOS и Android) и управлять ими в среде IDE.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

В Visual Studio выберите **Сервис > Xamarin Live Player > Управление устройствами...**

![Управление для закрытия этого окна](player-images/manage-tools-menu-vs.png)

Это окно позволяет выполнить следующие действия.

- Чтобы связать новое устройство, проверки кода
- Можно также свяжите устройство, введя код, отображаемый на экране его
- Удалите существующие устройства из списка

Это окно можно также открыть из списка устройств.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac, выберите **Инструменты > Управление устройствами (Live проигрыватель Xamarin)...**

![Управление для закрытия этого окна](player-images/manage-tools-menu.png)

Это окно позволяет выполнить следующие действия.

- Чтобы связать новое устройство, проверки кода
- Можно также свяжите устройство, введя код, отображаемый на экране его
- Удалите существующие устройства из списка

![Управление для закрытия этого окна](player-images/manage.png)

Можно также открыть это окно из списка устройств:

![Выберите из списка устройств Xamarin Live проигрывателей](player-images/manage-device-menu.png)

-----

При возникновении любой проблемы см. в разделе [ограничения и устранение неполадок](~/tools/live-player/troubleshooting.md).


## <a name="related-links"></a>Связанные ссылки

- [Ограничения](~/tools/live-player/limitations.md)
- [Устранение неполадок](~/tools/live-player/troubleshooting.md)
- [Образцы динамической проигрыватель Xamarin](~/tools/livehttps://developer.xamarin.com/samples.md)
