---
title: Геокодирование карт Xamarin. Forms
description: В этой статье объясняется, как выполнять геокод и реверсировать данные карты геокодирования с помощью класса геокодирования Xamarin. Forms. Maps.
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
ms.openlocfilehash: ce1f6751c0381ed41058784fbea3ebedefbdac6d
ms.sourcegitcommit: e4c23187874488ff55794d0e81a9bba30d2c2cd6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/22/2019
ms.locfileid: "72778796"
---
# <a name="xamarinforms-map-geocoding"></a>Геокодирование карт Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

Xamarin. Forms. Maps предоставляет класс [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) , который выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах.

## <a name="geocode-an-address"></a>Геокодирование адреса

Адрес улицы может быть геокодирован в координаты широты и долготы путем создания экземпляра [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) и вызова метода [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) в экземпляре `Geocoder`:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

Метод [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) принимает `string` аргумент, представляющий адрес, и асинхронно Возвращает коллекцию объектов [`Position`](xref:Xamarin.Forms.Maps.Position) , которые могут представлять адрес.

## <a name="reverse-geocode-an-address"></a>Обратный геокодировании адреса

Координаты широты и долготы могут быть обратными геокодированными в адрес улицы путем создания [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) экземпляра и вызова метода [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) в экземпляре `Geocoder`:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder;

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

Метод [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) принимает [`Position`](xref:Xamarin.Forms.Maps.Position) аргумент, состоящий из координат широты и долготы, и асинхронно Возвращает коллекцию строк, представляющих адреса, расположенные рядом с позицией.

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [API-интерфейс геокодирования](xref:Xamarin.Forms.Maps.Geocoder)
