---
title: 'Xamarin.FormsФигуры: многоугольник'
description: Xamarin.FormsКласс Polygon можно использовать для рисования многоугольников, Соединенных рядом линий, образующих замкнутые фигуры.
ms.prod: xamarin
ms.assetid: D6539F60-A5AC-46EF-86EB-E9F508EB1FA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/16/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 455221b71c612b7987f876e8e9b82e33e5d91129
ms.sourcegitcommit: 34fa3086c55b1e01838419c930f839c20662c362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84990872"
---
# <a name="xamarinforms-shapes-polygon"></a>Xamarin.FormsФигуры: многоугольник

![](~/media/shared/preview.png "This API is currently pre-release")

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)

`Polygon`Класс является производным от `Shape` класса и может использоваться для рисования многоугольников, Соединенных рядом линий, образующих замкнутые фигуры. Сведения о свойствах, которые `Polygon` класс наследует от `Shape` класса, см. в разделе [ Xamarin.Forms Shapes](index.md).

`Polygon` определяет следующие свойства:

- `Points`Тип `PointCollection` , представляющий собой коллекцию `Point` структур, описывающих точки вершин многоугольника.
- `FillRule`Тип `FillRule` , который указывает, как определяется внутреннее заполнение фигуры. Значение по умолчанию этого свойства равно `FillRule.EvenOdd`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

## <a name="create-a-polygon"></a>Создание многоугольника

В следующем примере XAML показано, как нарисовать многоугольник:

```xaml
<Polygon Points="40,10 70,80 10,50"
         Fill="AliceBlue"
         Stroke="Green"
         StrokeThickness="5"
         HeightRequest="100"
         WidthRequest="200" />
```

## <a name="related-links"></a>Связанные ссылки

- [Шапедемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-shapesdemos/)
- [Xamarin.FormsМногоугольник](index.md)
