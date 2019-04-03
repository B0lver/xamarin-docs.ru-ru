---
title: Приложение Xamarin Live Player
description: В этом документе описывается в Xamarin Live Player live приложение, которое может использоваться для предварительного просмотра изменений кода на устройстве. В нем описывается настройка, примеры, журналы, параметры, управление устройствами и многое другое.
ms.prod: xamarin
ms.assetid: A7EB73C1-38D7-46C5-9AF6-4C571C168BE7
author: lobrien
ms.author: laobri
ms.date: 08/08/2017
ms.openlocfilehash: 89795e5df00b426c0f11c04a0844993071df1e25
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2019
ms.locfileid: "58855202"
---
# <a name="xamarin-live-player-app"></a>Приложение Xamarin Live Player

![Функция предварительного просмотра](~/media/shared/preview.png)

> [!NOTE]
> Динамический просмотр проигрыватель доступен только в Visual Studio 2017.

После установки приложения на телефоне, выполните [инструкции по установке](~/tools/live-player/install.md) для подключения к компьютеру. Попробуйте выполнить одно из [примеры приложений](~/tools/live-player/samples.md) чтобы заставить ее работать.

При запуске приложение Xamarin Live Player выглядит следующим образом:

![Live Player Android снимка экрана приложения](player-images/app-android-sml.png)

При нажатии клавиши **пары для Visual Studio**, сканирование штрихкода, показывающий на компьютере с помощью камеры:

![Снимок экрана сканер штрихкодов Android](player-images/scan-android-sml.png)

Если подключение установлено успешно, код должен выполняться на устройстве почти сразу же (такие как [образец калькулятора](https://developer.xamarin.com/samples/mobile/LivePlayer/BasicCalculator)):

![Пример, калькулятор, приложения, выполняющегося на устройстве](player-images/basic-calculator-sml.png)

## <a name="options"></a>Параметры

Нажмите кнопку "Сведения" **(i)** в нижней части приложения, чтобы отобразить **параметры** меню:

[![Screenshot меню "Параметры"](player-images/options-sml.png)](player-images/options.png#lightbox)

### <a name="logs"></a>Журналы

Просмотр журналов для диагностики проблем.

### <a name="settings"></a>Параметры

- Показать или скрыть ошибки компиляции и времени выполнения.
- Сведения о версии.
- Отправьте отзыв.

[![Screenshot параметры](player-images/settings-sml.png)](player-images/settings.png#lightbox)

## <a name="managing-devices"></a>Управление устройствами

Для подключения устройства в первый раз, следуйте инструкциям в [требования и настройка](~/tools/live-player/install.md). Можно связать несколько устройств и управлять ими с помощью интегрированной среды разработки.

# [<a name="visual-studio"></a>Visual Studio](#tab/windows)

В Visual Studio, выберите **средства > Xamarin Live Player > Управление устройствами...**

![Управление окном устройств](player-images/manage-tools-menu-vs.png)

Это окно позволяет выполнить следующие действия.

- Чтобы связать новое устройство, проверка кода
- Кроме того, связывая устройство, введя код, отображаемый в его на экране
- Удалить существующие устройства из списка

Это окно также можно открыть из списка устройств.

# [<a name="visual-studio-for-mac"></a>Visual Studio для Mac](#tab/macos)

В Visual Studio для Mac выберите **Инструменты > Управление устройствами (Xamarin Live Player)...**

![Управление окном устройств](player-images/manage-tools-menu.png)

Это окно позволяет выполнить следующие действия.

- Чтобы связать новое устройство, проверка кода
- Кроме того, связывая устройство, введя код, отображаемый в его на экране
- Удалить существующие устройства из списка

![Управление окном устройств](player-images/manage.png)

Это окно также можно открыть из списка устройств:

![Выберите из списка устройств Xamarin Live Player Devices](player-images/manage-device-menu.png)

-----

При возникновении любой проблемы см. в разделе [ограничения и устранение неполадок](~/tools/live-player/troubleshooting.md).

## <a name="related-links"></a>Связанные ссылки

- [Устранение неполадок](~/tools/live-player/troubleshooting.md)
- [Примеры для использования с Live Player](https://developer.xamarin.com/samples/xamarin-live-player/all/)
