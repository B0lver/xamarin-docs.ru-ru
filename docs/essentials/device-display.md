---
title: 'Xamarin.Essentials: Отображение информации устройства'
description: Этот документ описывает класс DeviceDisplay в Xamarin.Essentials, предоставляют такие метрики экрана устройства, на котором выполняется приложение.
ms.assetid: 2821C908-C613-490D-8E8C-1BD3269FCEEA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: cb42da4c8c2d0e381a5b00f7e60da6f427d19c66
ms.sourcegitcommit: 51c274f37369d8965b68ff587e1c2d9865f85da7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39353832"
---
# <a name="xamarinessentials-device-display-information"></a>Xamarin.Essentials: Отображение информации устройства

![Предварительные версии NuGet](~/media/shared/pre-release.png)

**DeviceDisplay** класс предоставляет сведения о метриках экрана устройства, приложения.

## <a name="using-devicedisplay"></a>С помощью DeviceDisplay

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

## <a name="screen-metrics"></a>Метрики экрана

В дополнение к основные сведения об устройстве **DeviceDisplay** класс содержит сведения о пальцем экрана и ориентацию.

```csharp
// Get Metrics
var metrics = DeviceDisplay.ScreenMetrics;

// Orientation (Landscape, Portrait, Square, Unknown)
var orientation = metrics.Orientation;

// Rotation (0, 90, 180, 270)
var rotation = metrics.Rotation;

// Width (in pixels)
var width = metrics.Width;

// Height (in pixels)
var height = metrics.Height;

// Screen density
var density = metrics.Density;
```

**DeviceDisplay** класс также предоставляет событие, которое можно подписаться, запускаемое каждый раз, когда любой экрана изменениях метрик:

```csharp
public class ScreenMetricsTest
{
    public ScreenMetricsTest()
    {
        // Subscribe to changes of screen metrics
        DeviceDisplay.ScreenMetricsChanged += OnScreenMetricsChanged;
    }

    void OnScreenMetricsChanged(ScreenMetricsChangedEventArgs  e)
    {
        // Process changes
        var metrics = e.Metrics;
    }
}
```

## <a name="api"></a>API

- [DeviceDisplay исходного кода](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/DeviceDisplay)
- [Документация по DeviceDisplay API](xref:Xamarin.Essentials.DeviceDisplay)
