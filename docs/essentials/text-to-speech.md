---
title: Преобразования текста в речь Xamarin.Essentials
description: Класс TextToSpeech позволяет приложению использовать встроенный обработчиков чтения текста вслух оперировать назад текст с устройства, а также доступные языки запросов, поддерживающие ядро.
ms.assetid: AEEF03AE-A047-4DF0-B0E8-CC8D9A7B8351
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 2fcb97eafda15a73e2b7e5441f1d6c94f75af0ce
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="xamarinessentials-text-to-speech"></a>Преобразования текста в речь Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**TextToSpeech** класс позволяет приложению использовать встроенные преобразования текста в речь обработчиков оперировать назад текст с устройства, а также доступные языки запросов, поддерживающие ядро.

## <a name="using-text-to-speech"></a>С помощью преобразования текста в речь

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Функции преобразования текста в речь, работают путем вызова `SpeakAsync` метод с текстом и необязательные параметры и возвращает после завершения utterance. 

```csharp
public async Task SpeakNowDefaultSettings()
{
    await TextToSpeech.SpeakAsync("Hello World");

    // This method will block until utterance finishes.
}

public void SpeakNowDefaultSettings2()
{
    TextToSpeech.SpeakAsync("Hello World").ContinueWith((t) => 
    {
        // Logic that will run after utterance finishes.

    }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

Этот метод принимает необязательный CancellationToken остановить utterance, один для его запуска. 
```csharp
CancellationTokenSource cts;
public async Task SpeakNowDefaultSettings()
{
    cts = new CancellationTokenSource();
    await TextToSpeech.SpeakAsync("Hello World", cancelToken: cts.Token);

    // This method will block until utterance finishes.
}

public void CancelSpeech()
{
    if (cts?.IsCancellationRequested ?? false)
        return;

    cts.Cancel();
}
```

Преобразования текста в речь будет автоматически речи ставит запросы в очередь из того же потока. 

```csharp
bool isBusy = false;
public void SpeakMultiple()
{
    isBusy = true;
    Task.Run(async () =>
    {
        await TextToSpeech.SpeakAsync("Hello World 1");
        await TextToSpeech.SpeakAsync("Hello World 2");
        await TextToSpeech.SpeakAsync("Hello World 3");
        isBusy = false;
    });

    // or you can query multiple without a Task:
    Task.WhenAll(
        TextToSpeech.SpeakAsync("Hello World 1"),
        TextToSpeech.SpeakAsync("Hello World 2"),
        TextToSpeech.SpeakAsync("Hello World 3"))
        .ContinueWith((t) => { isBusy = false; }, TaskScheduler.FromCurrentSynchronizationContext());
}
```

### <a name="speech-settings"></a>Параметры речи

Для усиления контроля над как произносятся аудио обратно с `SpeakSettings` , позволяющий задание тома, шаг и языковой стандарт.

```csharp
public async Task SpeakNow()
{
    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

Ниже приведены поддерживаемые значения для этих параметров.

| Параметр | Минимум | Максимум |
| --- | :---: | :---: |
| Шаг | 0 | 2.0 |
| Тома | 0 | 1.0 |

### <a name="speech-locales"></a>Язык и региональные стандарты речи

Каждая платформа предоставляет язык и региональные стандарты оперировать назад текст на нескольких языках и диакритических знаков. Каждая платформа имеет различные коды и способы указания, поэтому Essentials предоставляет кросс платформенных `Locale` класс и способ их с помощью запроса `GetLocalesAsync`.

```csharp
public async Task SpeakNow()
{
    var locales = await TextToSpeech.GetLocalesAsync();

    // Grab the first locale
    var locale = locales.FirstOrDefault();

    var settings = new SpeakSettings()
        {
            Volume = .75,
            Pitch = 1.0,
            Locale = locale
        };

    await TextToSpeech.SpeakAsync("Hello World", settings);
}
```

## <a name="limitations"></a>Ограничения

- При вызове в нескольких потоках utterance очереди не гарантируется.
- Воспроизведение звука в фоновом режиме официально не поддерживается.

## <a name="api"></a>API

- [TextToSpeech исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/TextToSpeech)
- [Документация по TextToSpeech API](xref:Xamarin.Essentials.TextToSpeech)
