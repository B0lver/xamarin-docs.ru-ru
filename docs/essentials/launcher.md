---
title: 'Xamarin.Essentials: средство запуска'
description: Класс Launcher в Xamarin.Essentials позволяет приложению открыть URI средствами системы.
ms.assetid: BABF40CC-8BEE-43FD-BE12-6301DF27DD33
author: jamesmontemagno
ms.author: jamont
ms.date: 08/20/2019
ms.openlocfilehash: 3e49e12faac1fc93816d9a51646f639046c200c5
ms.sourcegitcommit: ab51d32f4ea0e0d4701f0bf2f1465c9323cd070b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/11/2019
ms.locfileid: "70887373"
---
# <a name="xamarinessentials-launcher"></a>Xamarin.Essentials: Средство запуска

Класс **Launcher** позволяет приложению открыть URI средствами системы. Это часто используется при глубокой привязке к пользовательским схемам URI другого приложения. Если нужно открыть в браузере веб-сайт, следует обратиться к API **[Browser](open-browser.md)** .

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-launcher"></a>Использование класса Launcher

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Чтобы использовать функции класса Launcher, вызовите метод `OpenAsync` и передайте параметр `string` или `Uri` для открытия. При необходимости с помощью метода `CanOpenAsync` можно проверить, может ли приложение на устройстве обработать схему URI.

```csharp
public class LauncherTest
{
    public async Task OpenRideShareAsync()
    {
        var supportsUri = await Launcher.CanOpenAsync("lyft://");
        if (supportsUri)
            await Launcher.OpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

Это можно объединить в один вызов с помощью `TryOpenAsync`, который проверяет, можно ли открыть параметр, и, если да, открывает его.

```csharp
public class LauncherTest
{
    public async Task<bool> OpenRideShareAsync()
    {
        return await Launcher.TryOpenAsync("lyft://ridetype?id=lyft_line");
    }
}
```

## <a name="files"></a>Файлы

Эта функция позволяет приложению запрашивать другие приложения для открытия и просмотра файла. Xamarin.Essentials автоматически обнаруживает тип файла (MIME) и запрашивает его открытие.

Ниже приведен пример записи текста на диск и запрос на открытие.

```csharp
var fn = "File.txt";
var file = Path.Combine(FileSystem.CacheDirectory, fn);
File.WriteAllText(file, "Hello World");

await Launcher.OpenAsync(new OpenFileRequest
{
    File = new ReadOnlyFile(file)
});
```

## <a name="platform-differences"></a>Различия платформ

# <a name="androidtabandroid"></a>[Android](#tab/android)

Задача, возвращенная методом `CanOpenAsync`, выполняется немедленно.

# <a name="iostabios"></a>[iOS](#tab/ios)

Если конечное приложение на этом устройстве никогда ранее не открывалось с помощью метода `OpenAsync` из вашего приложения, iOS однократно предложит пользователю разрешить открывать его с помощью вашего приложения.

Задача, возвращенная методом `CanOpenAsync`, выполняется немедленно.

Дополнительные сведения о реализации iOS доступны [здесь](xref:UIKit.UIApplication.CanOpenUrl*).

# <a name="uwptabuwp"></a>[UWP](#tab/uwp)

Различия платформ отсутствуют.

-----

## <a name="api"></a>API

- [Исходный код класса Launcher](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Launcher)
- [Документация по API Launcher](xref:Xamarin.Essentials.Launcher)
