---
title: Проверка подлинности по отпечаткам
description: В этом руководство описано, как добавить проверку подлинности отпечатков пальцев, представленную в Android 6,0, в приложение Xamarin. Android.
ms.prod: xamarin
ms.assetid: 6742D874-4988-4516-A946-D5C714B20A10
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 4a4b6ee7a123683a9d5a140c46c0b3542767ffa3
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027515"
---
# <a name="fingerprint-authentication"></a>Проверка подлинности по отпечаткам

_В этом руководство описано, как добавить проверку подлинности отпечатков пальцев, представленную в Android 6,0, в приложение Xamarin. Android._

## <a name="fingerprint-authentication-overview"></a>Обзор проверки подлинности отпечатков пальцев

Появление сканеров отпечатков пальцев на устройствах Android предоставляет приложениям альтернативу традиционному методу проверки подлинности пользователя и пароля. Использование отпечатков пальцев для проверки подлинности пользователя позволяет приложению обеспечить безопасность менее агрессивной, чем имя пользователя и пароль.

API-интерфейсы Финжерпринтманажер предназначены для устройств с сканером отпечатков пальцев и работают на уровне API 23 (Android 6,0) или более поздней версии. API-интерфейсы находятся в пространстве имен `Android.Hardware.Fingerprints`. В библиотеке поддержки Android версии v4 предусмотрена версия API-интерфейсов отпечатков пальцев, предназначенных для старых версий Android. Интерфейсы API совместимости находятся в пространстве имен `Android.Support.v4.Hardware.Fingerprint` и распространяются с помощью [пакета NuGet Xamarin. Android. support. v4](https://www.nuget.org/packages/Xamarin.Android.Support.v4/).

[Финжерпринтманажер](https://developer.android.com/reference/android/hardware/fingerprint/FingerprintManager.html) (и его аналог библиотеки поддержки, [финжерпринтманажеркомпат](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/FingerprintManagerCompat.html)) является основным классом для использования оборудования сканирования отпечатков пальцев. Этот класс является пакет SDK для Android оболочкой для службы уровня системы, которая управляет взаимодействием с самим оборудованием. Он отвечает за запуск сканера отпечатков пальцев и реагирование на отзыв от сканера. Этот класс имеет довольно простой интерфейс с тремя членами:

- **`Authenticate`** &ndash; этот метод инициализирует средство проверки оборудования и запускает службу в фоновом режиме, ожидая, как пользователь проверит свой отпечаток.
- **`EnrolledFingerprints`** &ndash; это свойство будет возвращать `true`, если пользователь зарегистрировал один или несколько отпечатков пальцев с устройством.
- **`HardwareDetected`** &ndash; это свойство используется для определения того, поддерживает ли устройство сканирование отпечатков пальцев.

Метод `FingerprintManager.Authenticate` используется приложением Android для запуска сканера отпечатков пальцев. В следующем фрагменте кода приведен пример того, как вызвать его с помощью API-интерфейсов совместимости библиотеки поддержки:

```csharp
// context is any Android.Content.Context instance, typically the Activity 
FingerprintManagerCompat fingerprintManager = FingerprintManagerCompat.From(context);
fingerprintManager.Authenticate(FingerprintManager.CryptoObject crypto,
                                int flags,
                                CancellationSignal cancel,
                                FingerprintManagerCompat.AuthenticationCallback callback,
                                Handler handler
                               );
```

В этом руководство рассказывается, как использовать API `FingerprintManager` для улучшения приложения Android с проверкой подлинности отпечатков пальцев. В нем рассматривается создание и настройка `CryptoObject` для защиты результатов от сканера отпечатков пальцев. Мы рассмотрим, как приложение должно подделать подкласс `FingerprintManager.AuthenticationCallback` и реагировать на отзывы от сканера отпечатков пальцев. Наконец, мы посмотрим, как зарегистрировать отпечаток на устройстве или эмуляторе Android и как использовать **ADB** для имитации сканирования отпечатков пальцев.

## <a name="requirements"></a>Требования

Для проверки подлинности отпечатков пальцев требуется Android 6,0 (API уровня 23) или более поздней версии и устройство со сканером отпечатков пальцев. 

Отпечаток пальца должен быть уже зарегистрирован на устройстве для каждого пользователя, который должен пройти проверку подлинности. Это включает в себя настройку блокировки экрана, которая использует пароль, ПИН-код, схему прокрутки или распознавание лиц. Некоторые функции проверки подлинности отпечатков пальцев можно имитировать в Android Emulator.  Дополнительные сведения об этих двух разделах см. в разделе [Регистрация отпечатка пальца](enrolling-fingerprint.md) . 

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения для программы для отпечатка пальца](https://docs.microsoft.com/samples/xamarin/monodroid-samples/fingerprintguide)
- [Пример диалогового окна отпечатка](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Запрос разрешений во время выполнения](https://developer.android.com/training/permissions/requesting.html)
- [Android. Hardware. отпечаток](https://developer.android.com/reference/android/hardware/fingerprint/package-summary.html)
- [Android. support. v4. Hardware. отпечатков](https://developer.android.com/reference/android/support/v4/hardware/fingerprint/package-summary.html)
- [Android. Content. Context](xref:Android.Content.Context)
- [API отпечатков и платежей (видео)](https://youtu.be/VOn7VrTRlA4)
