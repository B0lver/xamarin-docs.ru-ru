---
title: Поддержка платформы и функций Xamarin.Essentials
description: Xamarin.Essentials обеспечивает единый кроссплатформенных API-интерфейс, который предоставляет доступ из общего кода для любого приложения iOS, Android и универсальной платформы Windows независимо от используемого метода создания пользовательского интерфейса.
ms.assetid: 63FA28A5-6F52-4CB7-AF39-8DF7B436B5A4
author: jamesmontemagno
ms.author: jamont
ms.date: 07/10/2019
ms.openlocfilehash: 8bdd5c0d40cfdac0dadbc6bab1c538ab1b27946e
ms.sourcegitcommit: 4b6e832d1db5616b657dc8540da67c509b28dc1d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2019
ms.locfileid: "68386168"
---
# <a name="platform-support"></a>Поддержка платформ

Xamarin.Essentials поддерживает указанные ниже платформы и операционные системы.

| Platform | Версия |
| --- | --- |
| Android | 4.4 (API 19) или более поздней версии |
| iOS |10.0 или выше |
| Tizen | 4.0 или более поздней версии |
| tvOS | 10.0 или выше |
| watchOS | 4.0 или более поздней версии |
| UWP | 10.0.16299.0 или более поздней версии |

**Примечание.**

* Tizen официально поддерживает группа разработчиков Samsung.
* Охват API в tvOS и watchOS ограничен. Дополнительные сведения см. в руководстве по функциям.
* Операционные системы Tizen, tvOS и watchOS сейчас находятся на этапе предварительной версии и доступны в Xamarin.Essentials 1.3-pre.

## <a name="feature-support"></a>Поддержка компонентов

Xamarin.Essentials всегда пытается обеспечить компоненты ко всем платформам. Но для некоторых устройств действуют ограничения. Ниже указано, какие функции поддерживает каждая платформа.

Условные обозначения:

* ✔ — полная поддержка;
* ⚠ — ограниченная поддержка;
* ❌ — не поддерживается.

| Функция | Android | iOS | UWP | watchOS | tvOS | Tizen |
| --- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| [Акселерометр](accelerometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Сведения о приложении](app-information.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Барометр](barometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Батарея](battery.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ⚠ | ⚠ |
| [Буфер обмена](clipboard.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ❌ |
| [Преобразователи цвета](color-converters.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Компас](compass.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Подключение](connectivity.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Обнаружение тряски](detect-shake.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Сведения о дисплее устройства](device-display.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ❌ |
| [Сведения об устройстве](device-information.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Электронная почта](email.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Вспомогательные методы для файловой системы](file-system-helpers.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Фонарик](flashlight.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Геокодирование](geocoding.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Геопозиционирование](geolocation.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Гироскоп](gyroscope.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Средство запуска](launcher.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Магнитометр](magnetometer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [MainThread](main-thread.md?content=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Карты](maps.md?content=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Открыть браузер](open-browser.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Датчик ориентации](orientation-sensor.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ❌ | ✔ |
| [Телефон](phone-dialer.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Расширения платформы](platform-extensions.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Параметры](preferences.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Защищенное хранилище](secure-storage.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Общий доступ](share.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [SMS](sms.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |
| [Преобразование текста в речь](text-to-speech.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Преобразователи единиц](unit-converters.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Отслеживание версий](version-tracking.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ✔ | ✔ | ✔ |
| [Вибрация](vibrate.md?context=xamarin/xamarin-forms) | ✔ | ✔ | ✔ | ❌ | ❌ | ✔ |

