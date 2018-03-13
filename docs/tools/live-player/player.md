---
title: "Приложение динамической проигрыватель Xamarin"
description: "Изменение и тестирование приложений в режиме реального времени на устройства iOS или Android"
ms.topic: article
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 05/10/2017
ms.openlocfilehash: 6b0d62a9026c1248a66166e75ed41bb0148547a6
ms.sourcegitcommit: 0fdb243b46cf21be47584900805cadcd077121bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2018
---
# <a name="xamarin-live-player-app"></a>Приложение динамической проигрыватель Xamarin

![Функция предварительного просмотра](~/media/shared/preview.png)

## <a name="get-the-app"></a>Получение приложения

### <a name="xamarin-live-player-for-android"></a>Xamarin динамической Player для Android
Xamarin Live Player доступен для Android в Google Play:

[ ![Доступно на Google Play](images/google-play-badge.png)](https://play.google.com/store/apps/details?id=com.xamarin.live)

Для устройств Android без Google Play Xamarin Live Player доступна через [HockeyApp](https://aka.ms/xlp-hockeyapp) распространения. Кроме того, предварительной версии сборки для Android можно установить непосредственно из Google Play с включением защиты для [открыть бета-версии программы](https://play.google.com/apps/testing/com.xamarin.live)

### <a name="xamarin-live-player-for-ios"></a>Live Xamarin Player для iOS
Мы рекомендуем пользователям присоединяться к [приложения Xamarin Live Player _iOS предварительного просмотра_ ](https://aka.ms/liveplayeralpha) более быстрый доступ к последним усовершенствованиям через TestFlight.



## <a name="using-the-app"></a>С помощью приложения

После установки приложения на телефоне, выполните [инструкции по установке](~/tools/live-player/install.md) для подключения к компьютеру. Попробуйте выполнить одно из [образец приложения](~/tools/live-player/samples.md) для его запуска.

Во время запуска приложения Xamarin Live Player выглядит следующим образом (в iOS и Android соответственно):

![Live проигрывателя снимка экрана приложения iOS](player-images/app-iphone-sml.png) ![Динамическая Player Android снимка экрана приложения](player-images/app-android-sml.png)

При нажатии клавиши **пару, чтобы Visual Studio**, использовать камеру для сканирования штрих-кода, показывающий на вашем компьютере:

![Снимок экрана сканер штрих-кодов iOS](player-images/scan-iphone-sml.png) ![Снимок экрана сканер штрихкодов Android](player-images/scan-android-sml.png)

Если соединение установлено успешно, код должен выполняться на устройстве практически сразу (например, образец калькулятора):

![Пример приложения калькулятора, запущенного на устройстве](player-images/basic-calculator-iphone-sml.png)

## <a name="options"></a>Параметры

Нажмите кнопку "Сведения" **(i)** в нижней части приложения, чтобы раскрыть **параметры** меню:

![Снимок экрана: меню «Параметры»](player-images/options.png)

### <a name="logs"></a>Журналы

Просмотр журналов для диагностики проблем.

### <a name="settings"></a>Параметры

* Показать или скрыть ошибки компиляции и времени выполнения.
* Сведения о версии.
* Отправьте отзыв.

![Снимок экрана: параметры](player-images/settings.png)

## <a name="managing-devices"></a>Управление устройствами

Для подключения к устройству в первый раз, следуйте инструкциям в [требования и настройка](~/tools/live-player/install.md). Можно связать несколько устройств (например, iOS и Android) и управлять ими в среде IDE.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В Visual Studio выберите **Сервис > Xamarin Live Player > Управление устройствами...**

![Управление для закрытия этого окна](player-images/manage-tools-menu-vs.png)

Это окно позволяет выполнить следующие действия.

- Чтобы связать новое устройство, проверки кода
- Можно также свяжите устройство, введя код, отображаемый на экране его
- Удалите существующие устройства из списка

Это окно можно также открыть из списка устройств.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

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
