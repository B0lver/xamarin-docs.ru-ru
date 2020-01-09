---
title: Шрифты в Xamarin.Forms
description: В этой статье объясняется, как указать информацию о шрифте для элементов управления, отображающие текст в приложениях Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 11/04/2019
ms.openlocfilehash: 62bc5d2012df4880e9257ac70d0fc2e0fca4af64
ms.sourcegitcommit: 619b32f4f96a255140963afcf87629ca690af93d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/27/2019
ms.locfileid: "75500442"
---
# <a name="fonts-in-xamarinforms"></a>Шрифты в Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)

В этой статье описывается, как Xamarin.Forms позволяет указать атрибуты шрифта (включая вес и размер) в элементах управления, отображающие текст. Сведения о шрифтах может быть [указано в коде](#Setting_Font_in_Code) или [указан в XAML](#Setting_Font_in_Xaml). Также можно использовать [пользовательский шрифт](#Using_a_Custom_Font)и [Отображать значки шрифтов](#display-font-icons).

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Установка шрифта в коде

Используйте три связанные со шрифтами свойства любых элементов управления, отображающие текст:

- **FontFamily** &ndash; `string` имя шрифта.
- **FontSize** &ndash; размер шрифта в виде `double`.
- **FontAttributes** &ndash; строку, указывающую сведения о стиле, например *курсив* и **Полужирный** (с помощью `FontAttributes` перечисления в C#).

Этот код показано, как создать метку и укажите размер и вес для отображения:

```csharp
var about = new Label {
    FontSize = Device.GetNamedSize (NamedSize.Medium, typeof(Label)),
    FontAttributes = FontAttributes.Bold,
    Text = "Medium Bold Font"
};
```

<a name="FontSize" />

### <a name="font-size"></a>Font size

`FontSize` Свойство может быть присвоено значение типа double, например:

```csharp
label.FontSize = 24;
```

Значение размера измеряется в независимых от устройства единицах. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Xamarin. Forms также определяет поля в перечислении [`NamedSize`](xref:Xamarin.Forms.NamedSize) , представляющие конкретные размеры шрифтов. Дополнительные сведения об именованных размерах шрифтов см. в разделе [размеры именованных](#named-font-sizes)шрифтов.

<a name="FontAttributes" />

### <a name="font-attributes"></a>Атрибуты шрифта

Задает стиль шрифта, такие как **полужирным** и *курсивом* может устанавливаться на `FontAttributes` свойство. В настоящее время поддерживаются следующие значения:

- **None**
- **Полужирный**
- **Курсив**

`FontAttribute` Перечисления можно использовать следующим образом (можно указать один атрибут или `OR` их):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Задание сведений о шрифте для платформы

Кроме того `Device.RuntimePlatform` свойство может использоваться для установки имен шрифтов на каждой платформе, как показано в этом коде:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Хорошим источником информации о шрифтах для iOS будет [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Установка шрифта в XAML

Xamarin.Forms элементы управления, отображаемый текст, все имеют `FontSize` свойство, которое можно задать в XAML. Самый простой способ задать шрифт в XAML является использование значений перечисления из указанных размеров, как показано в следующем примере:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Имеется встроенный преобразователь для `FontSize` свойство, которое обеспечивает все параметры шрифта были выражены в виде строкового значения в XAML. Кроме того, для указания атрибутов шрифта можно использовать свойство `FontAttributes`.

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-specific-values) может также использоваться в XAML для отрисовки шрифта на каждой платформе. В приведенном ниже примере используется пользовательский шрифт на iOS (Маркерфелт-тонкий), и на других платформах указывается только размер и атрибуты:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="MarkerFelt-Thin" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

При указании пользовательского шрифта, настоятельно рекомендуется использовать `OnPlatform`, как трудно найти шрифт, который доступен на всех платформах.

## <a name="named-font-sizes"></a>Размеры именованных шрифтов

Xamarin. Forms определяет поля в перечислении [`NamedSize`](xref:Xamarin.Forms.NamedSize) , представляющие конкретные размеры шрифтов. В следующей таблице показаны элементы `NamedSize` и их размеры по умолчанию для iOS, Android и универсальная платформа Windows (UWP).

| Член | iOS | Android | UWP |
| --- | --- | --- | --- |
| `Default` | 16 | 14 | 14 |
| `Micro` | 11 | 10 | 15,667 |
| `Small` | 13 | 14 | 18,667 |
| `Medium` | 16 | 17 | 22,667 |
| `Large` | 20 | 22 | 32 |
| `Body` | 17 | 16 | 14 |
| `Header` | 17 | 96 | 46 |
| `Title` | 28 | 24 | 24 |
| `Subtitle` | 22 | 16 | 20 |
| `Caption` | 12 | 12 | 12 |

Значения размера измеряются в единицах, независимых от устройства. Дополнительные сведения см. в разделе [единицы измерения](~/xamarin-forms/user-interface/controls/common-properties.md#units-of-measurement).

Именованные размеры шрифтов можно задать с помощью XAML и кода. Кроме того, можно вызвать метод `Device.GetNamedSize`, чтобы вернуть `double`, представляющий именованный размер шрифта:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

> [!NOTE]
> В iOS и Android именованные размеры шрифтов будут автоматически масштабироваться на основе специальных возможностей операционной системы. Это поведение можно отключить в iOS с конкретной платформой. Дополнительные сведения см. [в разделе масштабирование специальных возможностей для именованных размеров шрифтов в iOS](~/xamarin-forms/platform/ios/named-font-size-scaling.md).

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>Использовать пользовательский шрифт

Использование шрифтов, отличных от встроенных гарнитур требует кодирования некоторые специфические для платформы. На этом снимке экрана показан пользовательский шрифт **Lobster** из [шрифты открытым исходным кодом от Google](https://www.google.com/fonts) отображаются с помощью Xamarin.Forms.

 [![Пользовательский шрифт в iOS и Android](fonts-images/custom-sml.png "Пример пользовательских шрифтов")](fonts-images/custom.png#lightbox "Пример пользовательских шрифтов")

Ниже описаны шаги, необходимые для каждой платформы. При включении файлы пользовательских шрифтов с приложением, обязательно убедитесь, что лицензии шрифта разрешены для распространения.

### <a name="ios"></a>iOS

Можно отобразить пользовательский шрифт, во-первых, гарантируя, что он загружается, а затем обращения к нему по имени с помощью Xamarin.Forms `Font` методы.
Следуйте инструкциям в [этой записи блога](https://devblogs.microsoft.com/xamarin/custom-fonts-in-ios/):

1. Добавить файл шрифта с **действие при построении: BundleResource**, и
2. Обновление **Info.plist** файл (**шрифты, предоставленные приложением**, или `UIAppFonts`, key), затем
3. Ссылаться на него по имени везде, где вы определяете шрифта в Xamarin.Forms!

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" : null // set only for iOS
}
```

### <a name="android"></a>Android

Xamarin.Forms для Android может ссылаться на пользовательский шрифт, который был добавлен в проект, выполнив определенные стандарт именования. Сначала добавьте файл шрифта в **активы** папки в проект приложения и установите *действие при построении: AndroidAsset*. Затем используйте полный путь и *имя шрифта* разделенных решетки (#), как имя шрифта в Xamarin.Forms, как показано в фрагменте кода:

```csharp
new Label
{
  Text = "Hello, Forms!",
  FontFamily = Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : null // set only for Android
}
```

### <a name="windows"></a>Портал

Xamarin.Forms для платформ Windows можно ссылаться на пользовательский шрифт, который был добавлен в проект, выполнив определенные стандарт именования. Сначала добавьте файл шрифта в **/Assets/шрифты/** папки в проект приложения и установите **сборки: содержимое действия**. Затем используйте полный путь и шрифт имени файла, следуют решетки (#) и **имя шрифта**, как показано в следующем фрагменте кода:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Обратите внимание на то, что имя файла шрифта и имя шрифта могут различаться. Чтобы получить имя шрифта в Windows, щелкните правой кнопкой мыши файл TTF и выберите **предварительной версии**. Имя шрифта можно определить, затем из окна предварительного просмотра.

### <a name="xaml"></a>XAML

Можно также использовать [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#interact-with-the-ui-from-background-threads) в XAML для отображения пользовательского шрифта:

```xaml
<Label Text="Hello Forms with XAML">
    <Label.FontFamily>
        <OnPlatform x:TypeArguments="x:String">
                <On Platform="iOS" Value="Lobster-Regular" />
                <On Platform="Android" Value="Lobster-Regular.ttf#Lobster-Regular" />
                <On Platform="UWP" Value="Assets/Fonts/Lobster-Regular.ttf#Lobster" />
        </OnPlatform>
    </Label.FontFamily>
</Label>
```

## <a name="display-font-icons"></a>Отображение значков шрифтов

Значки шрифтов могут отображаться приложениями Xamarin. Forms путем указания данных значка шрифта в объекте `FontImageSource`. Этот класс, производный от класса [`ImageSource`](xref:Xamarin.Forms.ImageSource) , имеет следующие свойства:

- `Glyph` — значение символа Юникода значка шрифта, указанного как `string`.
- `Size` — `double` значение, указывающее размер (в единицах, независимых от устройства) значка отображаемого шрифта. Значение по умолчанию — 30. Кроме того, для этого свойства можно задать именованный размер шрифта.
- `FontFamily` — `string`, представляющий семейство шрифтов, которому принадлежит значок шрифта.
- `Color` — необязательное [`Color`](xref:Xamarin.Forms.Color) значение, используемое при отображении значка шрифта.

Эти данные используются для создания PNG-изображения, которое может отображаться в любом представлении, которое может отображать `ImageSource`. Такой подход позволяет отображать значки шрифтов, такие как эмодзи, в нескольких представлениях, в отличие от отображения значков шрифта в одном представлении представления текста, например [`Label`](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Значки шрифтов могут быть заданы только в символьном представлении в Юникоде.

В следующем примере XAML отображается один значок шрифта, отображаемый [`Image`](xref:Xamarin.Forms.Image) представлением:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Этот код отображает значок XBox из семейства шрифтов Иониконс в представлении [`Image`](xref:Xamarin.Forms.Image) . Обратите внимание, что хотя символ Юникода для этого значка является `\uf30c`, он должен быть экранирован в XAML, и поэтому становится `&#xf30c;`. Эквивалентный код на C# выглядит так:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

На следующих снимках экрана из примера [связываемые макеты](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts) показаны несколько значков шрифтов, отображаемых с помощью связываемого макета:

![Снимок экрана с отображаемыми значками шрифтов в iOS и Android](fonts-images/font-image-source.png "Значки шрифтов, отображаемые в представлении изображений")

## <a name="related-links"></a>Связанные ссылки

- [FontsSample](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithfonts)
- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Макеты с возможностью привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-bindablelayouts)
- [Привязываемые макеты](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
