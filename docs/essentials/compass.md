---
title: Компас Xamarin.Essentials
description: Класс компас позволяет наблюдать за заголовок Северная магнитные устройства.
ms.assetid: BF85B0C3-C686-43D9-811A-07DCAF8CDD86
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 39f141424ddd247458b9c8b35ae02ab29e2c2206
ms.sourcegitcommit: 3e05b135b6ff0d607bc2378c1b6e66d2eebbcc3e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/12/2018
---
# <a name="xamarinessentials-compass"></a>Компас Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**Компас** позволяет отслеживать устройства Северная магнитные заголовок.

## <a name="using-compass"></a>С помощью компас

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Эта функциональность компас работает путем вызова `Start` и `Stop` методы для прослушивания изменений компас. Все изменения отправляются обратно через `ReadingChanged` событий. Пример:

```csharp
public class CompassTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.Ui;

    public CompassTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Compass.ReadingChanged += Compass_ReadingChanged;
    }

    void Compass_ReadingChanged(CompassChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: {data.HeadingMagneticNorth} degrees");
        // Process Heading Magnetic North
    }

    public void ToggleCompass()
    {
        try
        {
            if (Compass.IsMonitoring)
              Compass.Stop();
            else
              Compass.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Some other exception has occured
        }
    }
}
```

## <a name="sensor-speedxrefxamarinessentialssensorspeed"></a>[Скорость датчика](xref:Xamarin.Essentials.SensorSpeed)

- **Самый быстрый** — получить данные датчика максимально быстро (не гарантирует возврат в потоке пользовательского интерфейса).
- **Игра** — интенсивность подходит для игры (не гарантирует возврат в потоке пользовательского интерфейса).
- **Обычный** — курс по умолчанию, подходит для изменения ориентации экрана.
- **Пользовательский интерфейс** — интенсивность подходит для общего пользовательского интерфейса.

## <a name="platform-implementation-specifics"></a>Особенности реализации платформы

# <a name="androidtabandroid"></a>[Android](#tab/android)

Android не предоставляют API для извлечения компаса заголовок. Мы используем, акселерометр и магнитометр для вычисления заголовком Северная магнитные рекомендуется Google. 

В редких случаях может быть появиться противоречивые результаты так, как нужно датчики калибровку, которой включает в себя перемещение устройства на ходу рис. 8. Лучший способ сделать это — открывать карты Google, коснуться точкой в данном месте, а затем выберите **компас откалибровать**.

Имейте в виду, на котором несколько датчиков из приложения, в то же время может регулировать скорость датчика.

--------------

## <a name="api"></a>API

- [Компас исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Compass)
- [Компаса документации по API](xref:Xamarin.Essentials.Compass)
