---
title: Цвета в Xamarin.Forms
description: Xamarin.Forms предоставляет гибкий класс кросс платформенных цвет. В этой статье содержатся сведения о функциях, предоставляемых класса цвета и способы ее использования.
ms.prod: xamarin
ms.assetid: 22288ABF-57BE-47A9-ACC3-AC604D787C46
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/15/2017
ms.openlocfilehash: 1f29f283207ed8c1382424b3680177886ad2c806
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68653159"
---
# <a name="colors-in-xamarinforms"></a>Цвета в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)

_Xamarin.Forms предоставляет гибкий класс кросс платформенных цвет._

В этой статье рассматриваются различные способы `Color` класс может использоваться в Xamarin.Forms.

`Color` Класс предоставляет ряд методов для создания экземпляра цвет

-  **Именованные цвета** -коллекция общих именованные цвета, включая `Red`, `Green`, и `Blue`.
-  **Фромхекс** — строковое значение, аналогичное синтаксису, используемому в HTML, например "00FF00". При необходимости в качестве первой пары символов ("CC00FF00") можно указать альфа-канал.
-  **FromHsla** -тона, насыщенности и яркости `double` значения, с необязательным значением альфа-канала (0,0-1.0).
-  **FromRgb** -красного, зеленого и синего `int` значения (0-255).
-  **FromRgba** -красный, зеленый, синий и альфа-канал `int` значения (0-255).
-  **FromUint** -задайте один `double` значение, представляющее **argb**.

Вот некоторые пример цвета, назначенные `BackgroundColor` некоторых меток, с помощью различных вариантов разрешенных синтаксиса:

```csharp
var red    = new Label { Text = "Red",   BackgroundColor = Color.Red };
var orange = new Label { Text = "Orange",BackgroundColor = Color.FromHex("FF6A00") };
var yellow = new Label { Text = "Yellow",BackgroundColor = Color.FromHsla(0.167, 1.0, 0.5, 1.0) };
var green  = new Label { Text = "Green", BackgroundColor = Color.FromRgb (38, 127, 0) };
var blue   = new Label { Text = "Blue",  BackgroundColor = Color.FromRgba(0, 38, 255, 255) };
var indigo = new Label { Text = "Indigo",BackgroundColor = Color.FromRgb (0, 72, 255) };
var violet = new Label { Text = "Violet",BackgroundColor = Color.FromHsla(0.82, 1, 0.25, 1) };

var transparent = new Label { Text = "Transparent",BackgroundColor = Color.Transparent };
var @default = new Label    { Text = "Default",    BackgroundColor = Color.Default };
var accent = new Label      { Text = "Accent",     BackgroundColor = Color.Accent };
```

Эти цвета отображаются на каждой из платформ. Обратите внимание, что конечный цвет - `Accent` -blue-ish цвета для iOS и Android; это значение определяется Xamarin.Forms.

 [![Демонстрация цвет](colors-images/colors-sml.png "Демонстрация цвет")](colors-images/colors.png#lightbox "Демонстрация цвет")

## <a name="colordefault"></a>Color.Default

Используйте `Default` для установки (или повторной установки) значение цвета по умолчанию платформы (понимание, что это относится к другой базовый цвет на каждой платформе, для каждого свойства).

Разработчики могут использовать это значение, чтобы задать `Color` свойство, но должен **не** запрос данного экземпляра для его значения RGB компонента (они все готово к -1).

## <a name="colortransparent"></a>Color.Transparent

Задайте цвет для очистки.

## <a name="coloraccent"></a>Color.Accent

В iOS и Android контрастный цвет, который является видимым в фоновом режиме по умолчанию, но не так же, как цвета текста по умолчанию имеет значение данного экземпляра.

## <a name="additional-methods"></a>Дополнительные методы

`Color` в состав операций включены дополнительные методы, которые могут использоваться для создания новых цветов:

-  **AddLuminosity** -возвращает новый цвет, изменив яркость функцией предоставленного изменений.
-  **WithHue** -возвращает новый цвет, заменив указанное значение цветового тона.
-  **WithLuminosity** -возвращает новый цвет, заменив яркость указанное значение.
-  **WithSaturation** -возвращает новый цвет, заменив насыщенность указанное значение.
-  **MultiplyAlpha** -возвращает новый цвет, изменив альфа-канал, его умножения предоставленное значение альфа-канала.

## <a name="implicit-conversions"></a>Неявные преобразования

Неявное преобразование между `Xamarin.Forms.Color` и `System.Drawing.Color` типов может быть выполнена:

```csharp
Xamarin.Forms.Color xfColor = Xamarin.Forms.Color.FromRgb(0, 72, 255);
System.Drawing.Color sdColor = System.Drawing.Color.FromArgb(38, 127, 0);

// Implicity convert from a Xamarin.Forms.Color to a System.Drawing.Color
System.Drawing.Color sdColor2 = xfColor;

// Implicitly convert from a System.Drawing.Color to a Xamarin.Forms.Color
Xamarin.Forms.Color xfColor2 = sdColor;
```

## <a name="deviceruntimeplatform"></a>Device.RuntimePlatform

Этот фрагмент кода использует `Device.RuntimePlatform` свойство, чтобы выборочно задать цвет `ActivityIndicator`:

```csharp
ActivityIndicator activityIndicator = new ActivityIndicator
{
    Color = Device.RuntimePlatform == Device.iOS ? Color.Black : Color.Default,
    IsRunning = true
};
```

## <a name="using-from-xaml"></a>С помощью из XAML

Цвета можно также легко ссылаться в XAML, используя имена определенного цвета или шестнадцатеричного представления, показано ниже:

```xaml
<Label Text="Sea color" BackgroundColor="Aqua" />
<Label Text="RGB" BackgroundColor="#00FF00" />
<Label Text="Alpha plus RGB" BackgroundColor="#CC00FF00" />
<Label Text="Tiny RGB" BackgroundColor="#0F0" />
<Label Text="Tiny Alpha plus RGB" BackgroundColor="#C0F0" />
```

> [!NOTE]
> При использовании компиляции XAML, названия цветов нечувствительны к регистру и таким образом, могут быть записаны в нижнем регистре. Дополнительные сведения о компиляции XAML, см. в разделе [компиляции XAML](~/xamarin-forms/xaml/xamlc.md).

## <a name="summary"></a>Сводка

Xamarin.Forms `Color` класс используется для создания ссылки на платформы с поддержкой цвета. Он может использоваться в общий код и XAML.


## <a name="related-links"></a>Связанные ссылки

- [ColorsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithcolors)
- [Выбор привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablepicker)
