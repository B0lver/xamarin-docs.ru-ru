---
title: 'Xamarin.Essentials: параметры'
description: Этот документ описывает класс предпочтения в Xamarin.Essentials, который сохраняет параметры приложения в хранилище ключей и значений. В этом примере рассматривается использование класса и типы данных, которые могут быть сохранены.
ms.assetid: AA81BCBD-79BA-448F-942B-BA4415CA50FF
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 4a45587c79cfbbcd1198f100915e698289f74950
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353754"
---
# <a name="xamarinessentials-preferences"></a>Xamarin.Essentials: параметры

![Предварительные версии NuGet](~/media/shared/pre-release.png)

**Предпочтения** класс помогает для хранения параметров приложения в хранилище ключ значение.

## <a name="using-preferences"></a>Использование предпочтений

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Для сохранения значения для заданного _ключ_ в установках:

```csharp
Preferences.Set("my_key", "my_value");
```

Чтобы извлечь значение из настроек, или значение по умолчанию, если не задать:

```csharp
var myValue = Preferences.Get("my_key", "default_value");
```

Чтобы удалить _ключ_ от настроек:

```csharp
Preferences.Remove("my_key");
```

Чтобы удалить все настройки:

```csharp
Preferences.Clear();
```

Помимо этих методов принимать в необязательный `sharedName` , можно использовать для создания дополнительных контейнеров для предпочтения. Чтение ниже особенности реализации платформы.

## <a name="supported-data-types"></a>Поддерживаемые типы данных

Следующие типы данных поддерживаются в **предпочтения**:

- **bool**
- **double**
- **int**
- **float**
- **long**
- **string**
- **DateTime**

## <a name="implementation-details"></a>Сведения о реализации

Значения типа `DateTime` хранятся в виде 64-разрядный двоичный файл (длинное целое), с помощью двух методов, определенных `DateTime` класса: [ `ToBinary` ](xref:System.DateTime.ToBinary) метод, используемый для кодирования `DateTime` значение и [ `FromBinary` ](xref:System.DateTime.FromBinary(System.Int64)) метод декодирует значение. См. документацию из этих методов для корректировки, которые могут быть предприняты в декодированный значений, когда `DateTime` — то есть хранящихся не является значением времени (UTC).

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

# <a name="androidtabandroid"></a>[Android](#tab/android)

Все данные хранятся в [общих настроек](https://developer.android.com/training/data-storage/shared-preferences.html). Если не `sharedName` указан, используются параметры по умолчанию совместно, в противном случае имя используется для получения **частного** общих параметров с указанным именем.

# <a name="iostabios"></a>[iOS](#tab/ios)

[Параметры NSUserDefaults](https://docs.microsoft.com/en-us/xamarin/ios/app-fundamentals/user-defaults) используется для хранения значения на устройствах iOS. Если не `sharedName` указан `StandardUserDefaults` , используется, в противном случае имя используется для создания нового `NSUserDefaults` с указанным именем, используемый для `NSUserDefaultsType.SuiteName`.

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

[ApplicationDataContainer](https://docs.microsoft.com/en-us/uwp/api/windows.storage.applicationdatacontainer) используется для хранения значений на устройстве. Если не `sharedName` указан `LocalSettings` , используется, в противном случае имя используется для создания нового контейнера внутри `LocalSettings`.

--------------

## <a name="persistence"></a>Сохраняемость

Удалить приложение вызовет все _предпочтения_ для удаления. Есть одно исключение для этого, которого для приложений, предназначенных для и запускаемых на базе Android 6.0 (уровень API 23) или более поздней версии, которые используют [ __Автоматическая архивация__](https://developer.android.com/guide/topics/data/autobackup). Эта функция включена по умолчанию и сохраняет данные приложения, включая __общих настроек__, которых **предпочтения** использует API. Вы можете отключить, следующие Google [документации](https://developer.android.com/guide/topics/data/autobackup).

## <a name="limitations"></a>Ограничения

При сохранении строки, этот API предназначен для хранения небольших объемов текста.  Производительность может быть недостаточно высокие, если попытаться использовать для хранения больших объемов текста.

## <a name="api"></a>API

- [Параметры исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Preferences)
- [Документации по настройкам API](xref:Xamarin.Essentials.Preferences)
