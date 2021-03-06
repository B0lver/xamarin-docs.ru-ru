---
title: Руководство по проверке подлинности по отпечаткам
ms.prod: xamarin
ms.assetid: B40332CC-8123-4150-B47E-996214388842
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: aaa67972afeffd10c038a145a5703e917647b0fb
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91453874"
---
# <a name="fingerprint-authentication-guidance"></a>Руководство по проверке подлинности по отпечаткам

## <a name="fingerprint-authentication-guidance"></a>Руководство по проверке подлинности по отпечаткам

Теперь, когда мы рассмотрели основные понятия и интерфейсы API, связанные с проверкой подлинности по отпечаткам в Android 6.0, давайте рассмотрим некоторые общие рекомендации по использованию API отпечатков.

1. **Используйте API совместимости с библиотекой поддержки Android версии 4** &ndash; это упростит код приложения путем удаления проверки API из кода и разрешения приложению ориентироваться на максимальное количество устройств.
2. **Укажите альтернативы для проверки подлинности отпечатков пальцев** &ndash; проверка подлинности отпечатков пальцев — это отличный и быстрый способ проверки подлинности пользователя в приложении, однако он может не всегда работать или быть доступным. Сканер отпечатков пальцев может не работать, линза может быть грязной, пользователь может не настроить устройство на использование проверки подлинности по отпечаткам, или отпечатки пальцев могут со временем исчезнуть. Также возможно, что пользователь не захочет использовать в приложении проверку подлинности по отпечаткам. По этим причинам приложение Android должно предоставлять альтернативный процесс проверки подлинности, например с помощью имени пользователя и пароля.
3. **Используйте значок отпечатка Google** &ndash; все приложения должны использовать один и тот же значок отпечатка, предоставленный Google. Использование стандартного значка позволяет пользователям Android легко определить, для каких приложений используется проверка подлинности по отпечаткам: 
    
    ![Значок отпечатка Android](summary-images/ic-fp-40px.png)
    
4. **Уведомлять пользователя** &ndash; приложение должно отображать какое-либо уведомление пользователю о том, что сканер отпечатков активен и ожидает касания или смахивания. 

## <a name="summary"></a>Сводка

Проверка подлинности по отпечаткам — это отличный способ, позволяющий приложению Xamarin.Android быстро проверять пользователей, что упрощает взаимодействие пользователей с конфиденциальными функциями, такими как покупки из приложений. В этом разделе обсуждаются основные понятия и код, необходимый для внедрения API отпечатков Android 6.0 в приложение Xamarin.Android.

Сначала мы обсуждали сами API отпечатков, `FingerprintManager` (и `FingerprintManagerCompat`). Мы рассмотрели, как абстрактный класс `FingerprintManager.AuthenticationCallbacks` должен быть расширен приложением и использоваться в качестве посредника между оборудованием для считывания отпечатков и самим приложением. Затем мы рассмотрели, как проверить целостность результатов сканера отпечатков с помощью объекта Java `Cipher`. Наконец, мы немного коснулись тестирования, описав, как зарегистрировать отпечаток на устройстве и использовать **adb** для симуляции считывания отпечатка на эмуляторе. 

Если вы еще этого не сделали, вам следует взглянуть на [пример приложения](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide), сопровождающее это руководство. [Пример диалогового окна отпечатков пальцев](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog) перенесен из Java в Xamarin.Android и предоставляет еще один пример добавления проверки подлинности по отпечаткам в приложение Android.

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.Android. Пример отпечатка пальца](https://github.com/xamarin/monodroid-samples/tree/master/FingerprintGuide)
- [Xamarin.Android. Пример диалогового окна отпечатка](/samples/xamarin/monodroid-samples/android-m-fingerprintdialog)
- [Значок отпечатка пальца](https://raw.githubusercontent.com/xamarin/monodroid-samples/master/FingerprintGuide/FingerprintSampleApp/Resources/drawable-hdpi/ic_fp_40px.png)