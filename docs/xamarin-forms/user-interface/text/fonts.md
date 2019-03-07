---
title: Шрифты в Xamarin.Forms
description: В этой статье объясняется, как указать информацию о шрифте для элементов управления, отображающие текст в приложениях Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 49DD2249-C575-41AE-AE06-08F890FD6031
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/04/2019
ms.openlocfilehash: fc989ef73c9248bd359c9b1d35aaa9bdde846690
ms.sourcegitcommit: 00744f754527e5b55154365f89691caaf1c9d929
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2019
ms.locfileid: "57557064"
---
# <a name="fonts-in-xamarinforms"></a>Шрифты в Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)

В этой статье описывается, как Xamarin.Forms позволяет указать атрибуты шрифта (включая вес и размер) в элементах управления, отображающие текст. Сведения о шрифтах может быть [указано в коде](#Setting_Font_in_Code) или [указан в XAML](#Setting_Font_in_Xaml). Он имеет "также можно использовать [пользовательский шрифт](#Using_a_Custom_Font), и [отображать значки шрифта](#display-font-icons).

<a name="Setting_Font_in_Code" />

## <a name="set-the-font-in-code"></a>Установите шрифт в коде

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

Можно также использовать `NamedSize` перечисления, который имеет четыре встроенных параметров; Xamarin.Forms выбирает оптимальный размер для каждой платформы.

-  **Micro**
-  **Небольшой**
-  **Средний**
-  **Большой**

`NamedSize` Перечисления можно использовать везде, где `FontSize` можно задать с помощью `Device.GetNamedSize` метод для преобразования значения `double`:

```csharp
label.FontSize = Device.GetNamedSize(NamedSize.Small, typeof(Label));
```

<a name="FontAttributes" />

### <a name="font-attributes"></a>Атрибуты шрифта

Задает стиль шрифта, такие как **полужирным** и *курсивом* может устанавливаться на `FontAttributes` свойство. В настоящее время поддерживаются следующие значения:

-  **None**
-  **Полужирный**
-  **Курсив**

`FontAttribute` Перечисления можно использовать следующим образом (можно указать один атрибут или `OR` их):

```csharp
label.FontAttributes = FontAttributes.Bold | FontAttributes.Italic;
```

### <a name="set-font-info-per-platform"></a>Данные о шрифте набора каждой платформы

Кроме того `Device.RuntimePlatform` свойство может использоваться для установки имен шрифтов на каждой платформе, как показано в этом коде:

```csharp
label.FontFamily = Device.RuntimePlatform == Device.iOS ? "Lobster-Regular" :
   Device.RuntimePlatform == Device.Android ? "Lobster-Regular.ttf#Lobster-Regular" : "Assets/Fonts/Lobster-Regular.ttf#Lobster",
label.FontSize = Device.RuntimePlatform == Device.iOS ? 24 :
   Device.RuntimePlatform == Device.Android ? Device.GetNamedSize(NamedSize.Medium, label) : Device.GetNamedSize(NamedSize.Large, label);
```

Хорошим источником информации о шрифтах для iOS будет [iosfonts.com](http://iosfonts.com).

<a name="Setting_Font_in_Xaml" />

## <a name="set-the-font-in-xaml"></a>Установите шрифт в XAML

Xamarin.Forms элементы управления, отображаемый текст, все имеют `FontSize` свойство, которое можно задать в XAML. Самый простой способ задать шрифт в XAML является использование значений перечисления из указанных размеров, как показано в следующем примере:

```xaml
<Label Text="Login" FontSize="Large"/>
<Label Text="Instructions" FontSize="Small"/>
```

Имеется встроенный преобразователь для `FontSize` свойство, которое обеспечивает все параметры шрифта были выражены в виде строкового значения в XAML. Кроме того `FontAttributes` свойство может использоваться для указания атрибутов шрифта:

```xaml
<Label Text="Italics are supported" FontAttributes="Italic" />
<Label Text="Biggest NamedSize" FontSize="Large" />
<Label Text="Use size 72" FontSize="72" />
```

[`Device.RuntimePlatform`](~/xamarin-forms/platform/device.md#providing-platform-values) может также использоваться в XAML для отрисовки шрифта на каждой платформе. В следующем примере используется пользовательский шрифт в iOS (<span style="font-family:MarkerFelt-Thin">тонкой MarkerFelt</span>) и определяет только размер и атрибуты на других платформах:

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

<a name="Using_a_Custom_Font" />

## <a name="use-a-custom-font"></a>Применение пользовательского шрифта

Использование шрифтов, отличных от встроенных гарнитур требует кодирования некоторые специфические для платформы. На этом снимке экрана показан пользовательский шрифт **Lobster** из [шрифты открытым исходным кодом от Google](https://www.google.com/fonts) отображаются с помощью Xamarin.Forms.

 [![Пользовательский шрифт в iOS и Android](fonts-images/custom-sml.png "пользовательские шрифты пример")](fonts-images/custom.png#lightbox "пример пользовательских шрифтов")

Ниже описаны шаги, необходимые для каждой платформы. При включении файлы пользовательских шрифтов с приложением, обязательно убедитесь, что лицензии шрифта разрешены для распространения.

### <a name="ios"></a>iOS

Можно отобразить пользовательский шрифт, во-первых, гарантируя, что он загружается, а затем обращения к нему по имени с помощью Xamarin.Forms `Font` методы.
Следуйте инструкциям в [этой записи блога](http://blog.xamarin.com/custom-fonts-in-ios/):

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

### <a name="windows"></a>Windows

Xamarin.Forms для платформ Windows можно ссылаться на пользовательский шрифт, который был добавлен в проект, выполнив определенные стандарт именования. Сначала добавьте файл шрифта в **/Assets/шрифты/** папки в проект приложения и установите <span class="UIItem">сборки: содержимое действия</span>. Затем используйте полный путь и шрифт имени файла, следуют решетки (#) и <span class="UIItem">имя шрифта</span>, как показано в следующем фрагменте кода:

```csharp
new Label
{
    Text = "Hello, Forms!",
    FontFamily = Device.RuntimePlatform == Device.UWP ? "Assets/Fonts/Lobster-Regular.ttf#Lobster" : null // set only for UWP apps
}
```

> [!NOTE]
> Обратите внимание на то, что имя файла шрифта и имя шрифта могут различаться. Чтобы получить имя шрифта в Windows, щелкните правой кнопкой мыши файл TTF и выберите **предварительной версии**. Имя шрифта можно определить, затем из окна предварительного просмотра.

Итак, общий код для приложения полностью готов. Теперь мы реализуем код набора номера для конкретной платформы в качестве [DependencyService](~/xamarin-forms/app-fundamentals/dependency-service/index.md).

### <a name="xaml"></a>XAML

Можно также использовать [ `Device.RuntimePlatform` ](~/xamarin-forms/platform/device.md#providing-platform-values) в XAML для отображения пользовательского шрифта:

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

## <a name="display-font-icons"></a>Отображение значков шрифта

Шрифт могут отображаться значки приложений Xamarin.Forms, указав данные значка шрифта в `FontImageSource` объекта. Этот класс, который является производным от [ `ImageSource` ](xref:Xamarin.Forms.ImageSource) класса, имеет следующие свойства:

- `Glyph` — значение символа Юникода значка шрифта, указанный как `string`.
- `Size` — `double` значение, указывающее размер в аппаратно независимые единицы шрифта отображаемого значка. Значение по умолчанию — 30.
- `FontFamily` — `string` представляющий семейство шрифтов, к которой принадлежит значок шрифта.
- `Color` — Необязательный [ `Color` ](xref:Xamarin.Forms.Color) значение, используемое при отображении значок шрифта.

Эти данные используются для создания PNG, отображаемые по представлению, в котором можно отобразить `ImageSource`. Такой подход позволяет значки шрифта, такие как эмодзи, отображаемых по несколько представлений, в отличие от ограничения отображение значка шрифта в один текст, представляющим представление, таких как [ `Label` ](xref:Xamarin.Forms.Label).

> [!IMPORTANT]
> Значки шрифта можно указать только в настоящее время с их кодировки Юникод.

В следующем примере XAML имеет один шрифт значок, отображаемый элементом [ `Image` ](xref:Xamarin.Forms.Image) представления:

```xaml
<Image BackgroundColor="#D1D1D1">
    <Image.Source>
        <FontImageSource Glyph="&#xf30c;"
                         FontFamily="{OnPlatform iOS=Ionicons, Android=ionicons.ttf#}"
                         Size="44" />
    </Image.Source>
</Image>
```

Этот код отображает значок XBox, из Ionicons семейство шрифтов, в [ `Image` ](xref:Xamarin.Forms.Image) представления. Обратите внимание, что при Юникода символов для этот значок — `\uf30c`, он должен экранироваться в XAML и поэтому становится `&#xf30c;`. Ниже приведен аналогичный код C#:

```csharp
Image image = new Image { BackgroundColor = Color.FromHex("#D1D1D1") };
image.Source = new FontImageSource
{
    Glyph = "\uf30c",
    FontFamily = Device.RuntimePlatform == Device.iOS ? "Ionicons" : "ionicons.ttf#",
    Size = 44
};
```

Далее на снимках экрана, из [привязываемых макеты](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/) пример, отобразить несколько значков шрифтов, отображаемых привязываемых макета:

![Снимок экрана из отображаемых значков шрифта, в iOS и Android](fonts-images/font-image-source.png "значки шрифта, отображаемого в представлении изображения")

## <a name="related-links"></a>Связанные ссылки

- [FontsSample](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithFonts/)
- [Текст (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text/)
- [Привязываемые макеты (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BindableLayouts/)
- [Привязываемые макеты](~/xamarin-forms/user-interface/layouts/bindable-layouts.md)
