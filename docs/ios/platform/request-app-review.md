---
title: Запрос проверки приложения в Xamarin. iOS
description: В этой статье описывается метод Рекуестревиев, добавленный компанией Apple в iOS 10, и обсуждаются способы его реализации в Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 6408e707-b7dc-4557-b931-16a4d79b8930
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 0522e6b1bcf3e0f927a403c12a9c27bb6b3bb3ff
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031603"
---
# <a name="request-app-review-in-xamarinios"></a>Запрос проверки приложения в Xamarin. iOS

_В этой статье рассматривается метод Рекуестревиев, который Apple добавил в iOS 10 и как его реализовать в Xamarin. iOS._

В iOS 10,3 метод `RequestReview()` позволяет приложению iOS запрашивать у пользователя возможность оценить или проверить его. При вызове этого метода в приложении-отгрузке, которое пользователь установил из магазина приложений, iOS 10 будет обрабатывать всю оценку и процесс проверки для разработчика. Так как этот процесс регулируется политикой магазина приложений, предупреждение может быть или не отображаться.

![](request-app-review-images/review01.png "A sample Review Request alert")

## <a name="requesting-a-rating-or-review"></a>Запрос оценки или пересмотра

Хотя статический метод `RequestReview()` класса `SKStoreReviewController` может быть вызван в любой точке, где он имеет смысл в работе пользователя, процесс проверки управляется и обрабатывается политикой магазина приложений. В результате этот метод может или не отображать предупреждение и никогда не должен вызываться в ответ на действие пользователя, например коснуться кнопки.

Например, приложение может запросить проверку после того, как оно было запущено заданное число раз, или игра может запросить проверку после того, как игрок завершит свой уровень.

Чтобы запросить проверку сразу после завершения запуска приложения Xamarin. iOS, внесите следующие изменения в файл `AppDelegate.cs`:

```csharp
using Foundation;
using StoreKit;
using UIKit;

namespace iOSTenThree
{
    [Register ("AppDelegate")]
    public class AppDelegate : UIApplicationDelegate
    {
        ...

        public override bool FinishedLaunching (UIApplication application, NSDictionary launchOptions)
        {
            // Request a review from the user
            SKStoreReviewController.RequestReview ();

            return true;
        }

        ...

    }
}
```

> [!NOTE]
> Вызов `RequestReview()` в приложении, предназначенном для разработки, всегда будет отображать диалоговое окно оценки и проверки, чтобы его можно было протестировать. Это не относится к приложениям, которые были распространены через TestFlight, где вызов метода будет проигнорирован.

При вызове метода `RequestReview()` в приложении-отгрузке, которое пользователь установил из магазина приложений, iOS 10 будет обрабатывать всю оценку и процесс проверки для разработчика. Опять же, поскольку этот процесс регулируется политикой магазина приложений, предупреждение может быть или не отображаться.

## <a name="linking-to-an-app-store-product-page"></a>Связывание со страницей продукта App Store 

В дополнение к новому методу `RequestReview`, разработчик по-прежнему может предоставить глубокую ссылку на страницу продукта приложения в магазине приложений в приложении. Добавив `action=write-review` в конец URL-адреса страницы продукта, откроется страница, где пользователь может автоматически написать проверку приложения. 

## <a name="summary"></a>Сводка

В этой статье рассматривается метод Рекуестревиев, который Apple добавил в iOS 10 и как его реализовать в Xamarin. iOS.

## <a name="related-links"></a>Связанные ссылки

- [Пример Иостенсри](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree/)
