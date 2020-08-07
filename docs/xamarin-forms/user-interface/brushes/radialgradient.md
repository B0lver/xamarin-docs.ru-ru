---
title: 'Xamarin.FormsКисти: радиальные градиенты'
description: Xamarin.FormsКласс RadialGradientBrush закрашивает область с радиальным градиентом.
ms.prod: xamarin
ms.assetid: 099BA530-3B38-4005-9B19-A0EB4D4DEEFC
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/28/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: dd915384c630cc08a2bd79a52dd22ccb40835dfa
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87919156"
---
# <a name="no-locxamarinforms-brushes-radial-gradients"></a>Xamarin.FormsКисти: радиальные градиенты

![Предварительный просмотр API](~/media/shared/preview.png "Этот API-интерфейс сейчас доступен в предварительной версии.")

[![Скачать пример](~/media/shared/download.png) Скачайте пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)

`RadialGradientBrush`Класс является производным от `GradientBrush` класса и закрашивает область с радиальным градиентом, который смешивает два или более цвета в окружности. `GradientStop`объекты используются для указания цветов в градиенте и их позиций. Дополнительные сведения об `GradientStop` объектах см. в разделе [ Xamarin.Forms кисти: градиенты](gradient.md).

Класс `RadialGradientBrush` определяет следующие свойства:

- `Center`Тип [`Point`](xref:Xamarin.Forms.Point) , представляющий центральную точку окружности для радиального градиента. Значение этого свойства по умолчанию равно (0,5, 0,5).
- `Radius`Тип `double` , который представляет радиус окружности для радиального градиента. Значение этого свойства по умолчанию равно 0,5.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами, что означает, что они могут быть целевыми объектами привязки данных и стилями.

`RadialGradientBrush`Класс также содержит `IsEmpty` метод, который возвращает объект `bool` , который представляет, назначена ли кисть какие-либо `GradientStop` объекты.

> [!NOTE]
> Радиальные градиенты также можно создавать с помощью `radial-gradient()` функции CSS.

## <a name="create-a-radialgradientbrush"></a>Создание RadialGradientBrush

Позиции градиента кисти радиального градиента располагаются вдоль оси градиента, определенной кругом. Ось градиента расходятся от центра окружности до ее окружности. Расположение и размер окружности можно изменить с помощью `Center` свойств кисти и `Radius` . Окружность определяет конечную точку градиента. Поэтому ограничитель градиента в 1,0 определяет цвет в окружности. Позиция градиента в 0,0 определяет цвет в центре круга.

Чтобы создать радиальный градиент, создайте `RadialGradientBrush` объект и задайте его `Center` Свойства и `Radius` . Затем добавьте в коллекцию два или более `GradientStop` объектов, `RadialGradientBrush.GradientStops` которые определяют цвета градиента и их позиции.

В следующем примере XAML показано `RadialGradientBrush` , что задается как объект `Background` [`Frame`](xref:Xamarin.Forms.Frame) :

```xaml    
<Frame BorderColor="LightGray"
       HasShadow="True"
       CornerRadius="12"
       HeightRequest="120"
       WidthRequest="120">
    <Frame.Background>
        <!-- Center defaults to (0.5,0.5)
             Radius defaults to (0.5) -->
        <RadialGradientBrush>
            <GradientStop Color="Red"
                          Offset="0.1" />
            <GradientStop Color="DarkBlue"
                          Offset="1.0" />
        </RadialGradientBrush>
    </Frame.Background>
</Frame>
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) отрисовывается с помощью `RadialGradientBrush` интерполяции от красного к темному синему. Центр радиального градиента располагается в центре `Frame` :

![Кадр, окрашенный в центрированный RadialGradientBrush](radialgradient-images/center.png)

В следующем примере XAML центр радиального градиента перемещается в левый верхний угол [`Frame`](xref:Xamarin.Forms.Frame) :

```xaml
<!-- Radius defaults to (0.5) -->
<RadialGradientBrush Center="0.0,0.0">
    <GradientStop Color="Red"
                  Offset="0.1" />
    <GradientStop Color="DarkBlue"
                  Offset="1.0" />
</RadialGradientBrush>
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) отрисовывается с помощью `RadialGradientBrush` интерполяции от красного к темному синему. Центр радиального градиента располагается в левом верхнем углу `Frame` :

![Рамка, окрашенная в левый верхний RadialGradientBrush](radialgradient-images/top-left.png)

В следующем примере XAML центр радиального градиента перемещается в правый нижний угол [`Frame`](xref:Xamarin.Forms.Frame) :

```xaml
<!-- Radius defaults to (0.5) -->
<RadialGradientBrush Center="1.0,1.0">
    <GradientStop Color="Red"
                  Offset="0.1" />
    <GradientStop Color="DarkBlue"
                  Offset="1.0" />
</RadialGradientBrush>            
```

В этом примере фон [`Frame`](xref:Xamarin.Forms.Frame) отрисовывается с помощью `RadialGradientBrush` интерполяции от красного к темному синему. Центр радиального градиента располагается в правом нижнем углу `Frame` :

![Рамка, окрашенная в правый нижний RadialGradientBrush](radialgradient-images/bottom-right.png)

## <a name="related-links"></a>Связанные ссылки

- [Брушесдемос (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-brushdemos/)
- [Xamarin.FormsКисти: градиенты](gradient.md)
