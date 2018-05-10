---
title: Откройте Xamarin.Essentials браузера
description: Класс обозревателя позволяет приложения, чтобы открыть веб-ссылку в браузере предпочтительный оптимизированного системы или внешнем браузере.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: fe5b1de83d30a6de15afa35e5884f8be5462b07b
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-browser"></a>Xamarin.Essentials браузера

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Браузера** класс позволяет приложения, чтобы открыть веб-ссылку в браузере предпочтительный оптимизированного системы или внешнем браузере.

## <a name="using-browser"></a>С помощью браузера

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Эта функциональность обозревателя работает путем вызова `OpenAsync` метод с `Uri` и `BrowserLaunchType`.

```csharp

public class BrowserTest
{
    public async Task OpenBrowser(Uri uri)
    {
        await Browser.RequestAsync(uri, BrowserLaunchType.SystemPreferred);
    }
}
```

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

# <a name="androidtabandroid"></a>[Android](#tab/android)

Тип запуска определяет способ запуска браузера:

## <a name="system-preferred"></a>Основной системы

[Хром пользовательских вкладок](https://developer.chrome.com/multidevice/android/customtabs) будет предпринята попытка использовать Uri нагрузки и обеспечения осведомленности навигации.

## <a name="external"></a>Внешняя

`Intent` Будет использоваться для запроса Uri открыть с помощью обычных браузера системы.

# <a name="iostabios"></a>[iOS](#tab/ios)

## <a name="system-preferred"></a>Основной системы

[SFSafariViewController](https://developer.xamarin.com/api/type/SafariServices.SFSafariViewController/) используется для загрузки Uri и обеспечения осведомленности навигации в.

## <a name="external"></a>Внешняя

Стандартные `OpenUrl` на основное приложение используется для запуска обозревателя по умолчанию вне приложения.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Браузер пользователя по умолчанию всегда будет запущен независимо от `BrowserLaunchType`.

--------------

## <a name="api"></a>API

- [Обозреватель исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/Browser)
- [Документация по API браузера](xref:Xamarin.Essentials.Browser)
