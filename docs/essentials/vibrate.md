---
title: 'Xamarin.Essentials: вибрация'
description: Этот документ описывает класс Вибрация в Xamarin.Essentials, который позволяет запускать и останавливать vibrate функциональные возможности для требуемый промежуток времени.
ms.assetid: 7E8B24C4-2625-4DAE-A129-383542D34F1E
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2a2902ca4eac8b889f6875580d7cb4ea352803a8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34782925"
---
# <a name="xamarinessentials-vibration"></a>Xamarin.Essentials: вибрация

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Вибрация** класс позволяет запускать и останавливать vibrate функциональные возможности для требуемый промежуток времени.

## <a name="getting-started"></a>Начало работы

Чтобы получить доступ к **вибрация** требуется функциональность следующие платформы определенные настройки.

# <a name="androidtabandroid"></a>[Android](#tab/android)

Разрешение Vibrate является обязательным и должен быть настроен в проекте Android. Это можно сделать одним из следующих способов:

Откройте **AssemblyInfo.cs** файл **свойства** папки и добавьте:

```csharp
[assembly: UsesPermission(Android.Manifest.Permission.Vibrate)]
```

ИЛИ обновление манифеста Android.

Откройте **AndroidManifest.xml** файл **свойства** папки и добавьте следующий код внутри блока **манифеста** узла.

```xml
<uses-permission android:name="android.permission.VIBRATE" />
```

Или щелкните правой кнопкой мыши проект Anroid и откройте свойства проекта. В разделе **Android манифеста** найти **требуемые разрешения:** области и проверка **VIBRATE** разрешение. Будет автоматически обновлена **AndroidManifest.xml** файла.

# <a name="iostabios"></a>[iOS](#tab/ios)

Дополнительная настройка не требуется.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Дополнительная настройка не требуется.

-----

## <a name="using-vibration"></a>С помощью вибрация

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Вибрация функциональные возможности могут быть запрошены для определенного времени или по умолчанию 500 миллисекунд.

```csharp
try
{
    // Use default vibration length
    Vibration.Vibrate();

    // Or use specified time
    var duration = TimeSpan.FromSeconds(1);
    Vibration.Vibrate(duration);
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

Можно запросить отмену вибрацией устройства с `Cancel` метод:

```csharp
try
{
    Vibration.Cancel();
}
catch (FeatureNotSupportedException ex)
{
    // Feature not supported on device
}
catch (Exception ex)
{
    // Other error has occurred.
}
```

## <a name="platform-differences"></a>Различия между платформами

| Platform | Различие |
| --- | --- |
| iOS | Всегда подачи звуковых для 500 миллисекунд. |
| iOS | Не удается отменить вибрация. |

## <a name="api"></a>API

- [Вибрация исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Vibration)
- [Вибрация API документации](xref:Xamarin.Essentials.Vibration)
