---
title: Xamarin.FormsГеокодирование карт
description: В этой статье объясняется, как выполнять геокод и реверсировать данные карт геокодирования с помощью Xamarin.Forms . Сопоставляет класс геокодирования.
ms.prod: xamarin
ms.assetid: DE7DB31A-8921-4614-8B49-DAEF1E7B03B3
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/22/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 7e92385f3a82c2d12881be4ff3e54bc47533696d
ms.sourcegitcommit: ea2abdc789d0e292c3e1700a2b53b92097e0e542
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/03/2020
ms.locfileid: "87517497"
---
# <a name="no-locxamarinforms-map-geocoding"></a>Xamarin.FormsГеокодирование карт

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)

[`Xamarin.Forms.Maps`](xref:Xamarin.Forms.Maps)Пространство имен предоставляет [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) класс, который выполняет преобразование между строковыми адресами и координатами широты и долготы, хранящимися в [`Position`](xref:Xamarin.Forms.Maps.Position) объектах. Дополнительные сведения о [`Position`](xref:Xamarin.Forms.Maps.Position) структуре см. в разделе [Map Disposition and Distance](position-distance.md).

> [!NOTE]
> Альтернативный API-интерфейс геокодирования доступные в Xamarin.Essentials . Xamarin.Essentials `Geocoding` API предлагает структурированные данные адреса при геокодировании адресов, в отличие от строк, возвращаемых этим API. Дополнительные сведения см. в разделе [ Xamarin.Essentials геокодирование](~/essentials/geocoding.md).

## <a name="geocode-an-address"></a>Геокодирование адреса

Адрес улицы может быть геокодирован в координаты широты и долготы путем создания [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) экземпляра и вызова [`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*) метода в `Geocoder` экземпляре:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

IEnumerable<Position> approximateLocations = await geoCoder.GetPositionsForAddressAsync("Pacific Ave, San Francisco, California");
Position position = approximateLocations.FirstOrDefault();
string coordinates = $"{position.Latitude}, {position.Longitude}";
```

[`GetPositionsForAddressAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetPositionsForAddressAsync*)Метод принимает `string` аргумент, представляющий адрес, и асинхронно Возвращает коллекцию [`Position`](xref:Xamarin.Forms.Maps.Position) объектов, которые могут представлять адрес.

## <a name="reverse-geocode-an-address"></a>Обратный геокодировании адреса

Координаты широты и долготы могут быть обратными геокодированы в адрес улицы путем создания [`Geocoder`](xref:Xamarin.Forms.Maps.Geocoder) экземпляра и вызова [`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*) метода в `Geocoder` экземпляре:

```csharp
using Xamarin.Forms.Maps;
// ...
Geocoder geoCoder = new Geocoder();

Position position = new Position(37.8044866, -122.4324132);
IEnumerable<string> possibleAddresses = await geoCoder.GetAddressesForPositionAsync(position);
string address = possibleAddresses.FirstOrDefault();
```

[`GetAddressesForPositionAsync`](xref:Xamarin.Forms.Maps.Geocoder.GetAddressesForPositionAsync*)Метод принимает [`Position`](xref:Xamarin.Forms.Maps.Position) аргумент, состоящий из координат широты и долготы, и асинхронно Возвращает коллекцию строк, представляющих адреса, расположенные рядом с позицией.

## <a name="related-links"></a>Связанные ссылки

- [Пример Maps](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithmaps)
- [Xamarin.FormsРасположение и расстояние на карте](position-distance.md)
- [API-интерфейс геокодирования](xref:Xamarin.Forms.Maps.Geocoder)
