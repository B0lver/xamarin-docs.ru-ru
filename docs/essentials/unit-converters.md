---
title: Преобразователи единиц Xamarin.Essentials
description: Класс UnitConverters в Xamarin.Essentials предоставляет несколько преобразователей единиц, чтобы помочь разработчикам при использовании Xamarin.Essentials.
ms.assetid: 35DE2704-E730-4337-9476-66CD53376943
author: jamesmontemagno
ms.custom: video
ms.author: jamont
ms.date: 01/06/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 332762a83796fef24278c6685a29f7d31a10dadc
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84801859"
---
# <a name="xamarinessentials-unit-converters"></a>Xamarin.Essentials. Преобразователи единиц

Класс **UnitConverters** предоставляет несколько преобразователей единиц, чтобы помочь разработчикам при использовании Xamarin.Essentials.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-unit-converters"></a>Использование преобразователей единиц

Добавьте ссылку на Xamarin.Essentials в своем классе:

```csharp
using Xamarin.Essentials;
```

Все преобразователи единиц доступны с помощью статического класса `UnitConverters` в Xamarin.Essentials. Например, можно легко преобразовать градусы Фаренгейта в градусы Цельсия.

```csharp
var celsius = UnitConverters.FahrenheitToCelsius(32.0);
```

Ниже приведен список доступных преобразований:

- FahrenheitToCelsius
- CelsiusToFahrenheit
- CelsiusToKelvin
- KelvinToCelsius
- MilesToMeters
- MilesToMeters
- KilometersToMiles
- MetersToInternationalFeet
- InternationalFeetToMeters
- DegreesToRadians
- RadiansToDegrees
- DegreesPerSecondToRadiansPerSecond
- RadiansPerSecondToDegreesPerSecond
- DegreesPerSecondToHertz
- DegreesPerSecondToHertz
- HertzToDegreesPerSecond
- HertzToDegreesPerSecond
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- KilopascalsToHectopascals
- HectopascalsToKilopascals
- AtmospheresToPascals
- PascalsToAtmospheres
- CoordinatesToMiles
- CoordinatesToMiles
- KilogramsToPounds
- PoundsToKilograms
- StonesToPounds
- PoundsToStones

## <a name="api"></a>API

- [Исходный код преобразователей единиц](https://github.com/xamarin/Essentials/tree/main/Xamarin.Essentials/Types/UnitConverters.shared.cs)
- [Документация по API преобразователей единиц](xref:Xamarin.Essentials.UnitConverters)

## <a name="related-video"></a>Связанные видео

> [!Video https://channel9.msdn.com/Shows/XamarinShow/Unit-Conversion-XamarinEssentials-API-of-the-Week/player]

[!include[](~/essentials/includes/xamarin-show-essentials.md)]
