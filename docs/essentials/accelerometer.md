---
title: 'Xamarin.Essentials: Акселерометр'
description: Класс Accelerometer в Xamarin.Essentials позволяет считывать данные с датчика акселерометра устройства, который показывает ускорение устройства в трехмерном пространстве.
ms.assetid: 97883573-F0D9-4854-AC7C-A654814401C5
author: jamesmontemagno
ms.author: jamont
ms.date: 04/02/2019
ms.custom: video
ms.openlocfilehash: dd99d09f227809bf8834eea9749c4d5379abebdb
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "70765044"
---
# <a name="xamarinessentials-accelerometer"></a>Xamarin.Essentials: Акселерометр

Класс **Accelerometer** позволяет считывать данные с датчика акселерометра устройства, который показывает ускорение устройства в трехмерном пространстве.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-accelerometer"></a>Использование класса Accelerometer

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

Функции Accelerometer вызываются методами `Start` и `Stop`, которые активируют отслеживание изменений ускорения с датчика. Все изменения возвращаются через событие `ReadingChanged`. Пример использования:

```csharp

public class AccelerometerTest
{
    // Set speed delay for monitoring changes.
    SensorSpeed speed = SensorSpeed.UI;

    public AccelerometerTest()
    {
        // Register for reading changes, be sure to unsubscribe when finished
        Accelerometer.ReadingChanged += Accelerometer_ReadingChanged;
    }

    void Accelerometer_ReadingChanged(object sender, AccelerometerChangedEventArgs e)
    {
        var data = e.Reading;
        Console.WriteLine($"Reading: X: {data.Acceleration.X}, Y: {data.Acceleration.Y}, Z: {data.Acceleration.Z}");
        // Process Acceleration X, Y, and Z
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

Показания акселерометра сообщаются в единицах G. Это единица измерения силы тяжести, равная силе гравитационного поля Земли (9,81 м/с^2).

Система координат задается относительно экрана телефона в ориентации по умолчанию. При изменении ориентации экрана устройства положение осей не меняется.

Ось X — горизонтальная и указывает вправо, ось Y — вертикальная и указывает вверх, а ось Z указывает в направлении от экрана вперед. Координаты позади экрана в этой системе имеют отрицательные значения по оси Z.

Примеры

- Когда устройство лежит на столе и толкается в левый бок, в результате двигаясь вправо, значение ускорения по оси X будет положительным.

- Когда устройство лежит на столе, значение ускорения равно +1,00 G или (+9,81 м/с^2), что соответствует ускорению устройства (0 м/с^2) минус сила тяжести (–9,81 м/с^2), приведенному к единицам G.

- Когда устройство лежит на столе, а затем его толкают или тянут вверх с ускорением A м/с^2, значение ускорения равно A + 9,81, что соответствует ускорению устройства (+A м/с^2) минус сила тяжести (–9,81 м/с^2). Это значение затем приводится к единицам G.

[!include[](~/essentials/includes/sensor-speed.md)]

## <a name="api"></a>API

- [Исходный код класса Accelerometer](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/Accelerometer)
- [Документация по API Accelerometer](xref:Xamarin.Essentials.Accelerometer)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Accelerometer-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
