---
title: 'Xamarin.Essentials: Обнаружение тряски'
description: Класс Accelerometer в Xamarin.Essentials позволяет обнаруживать движение встряхивания устройства.
ms.assetid: 07513D32-120F-4F12-8757-A47802A8027B
author: jamesmontemagno
ms.author: jamont
ms.date: 11/04/2018
ms.openlocfilehash: f1482de3fd1c3e550ac9739d0f815092f7fe753d
ms.sourcegitcommit: 64d6da88bb6ba222ab2decd2fdc8e95d377438a6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/18/2019
ms.locfileid: "58176073"
---
# <a name="xamarinessentials-detect-shake"></a>Xamarin.Essentials: Обнаружение тряски

Класс **[Accelerometer](accelerometer.md)** позволяет считывать данные с датчика акселерометра устройства, который показывает ускорение устройства в трехмерном пространстве. Кроме того, он позволяет регистрировать события, когда пользователь трясет устройство.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-detect-shake"></a>Использование обнаружения тряски

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Для обнаружения тряски устройства необходимо использовать функциональные возможности акселерометра, вызвав методы `Start` и `Stop` для прослушивания изменений ускорения и обнаружения тряски. При каждом обнаружении встряхивания срабатывает событие `ShakeDetected `. Пример использования:

```csharp

public class DetectShakeTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public DetectShakeTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ShakeDetected  += Accelerometer_ShakeDetected ;
    }

    void Accelerometer_ShakeDetected (object sender, EventArgs e)
    {
        // Process shake event
    }

    public void ToggleAccelerometer()
    {
        try
        {
            if (Accelerometer.IsMonitoring)
              Accelerometer.Stop();
            else
              Accelerometer.Start(speed);
        }
        catch (FeatureNotSupportedException fnsEx)
        {
            // Feature not supported on device
        }
        catch (Exception ex)
        {
            // Other error has occurred.
        }
    }
}
```

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="implementation-details"></a>Сведения о реализации

Для обнаружения тряски API использует необработанные показания акселерометра для вычисления ускорения. Он использует простой механизм очереди для определения того, произошло ли 3/4 последних событий акселерометра за последнюю половину секунды. Ускорение рассчитывается путем добавления квадрата показаний X, Y и Z от акселерометра и его сравнения с определенным порогом.

## <a name="api"></a>API

- [Исходный код класса Accelerometer](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Документация по API Accelerometer](xref:Xamarin.Essentials.Accelerometer)
