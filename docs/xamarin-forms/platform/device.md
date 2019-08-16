---
title: Класс устройств Xamarin.Forms
description: В этой статье объясняется, как использовать класс устройств Xamarin.Forms, возможность точного управления функциональные возможности и макеты на каждой платформы.
ms.prod: xamarin
ms.assetid: 2F304AEC-8612-4833-81E5-B2F3F469B2DF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 06/12/2019
ms.openlocfilehash: 802f9ff60f74914a9369c7ef281cb2e70ca01d4b
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69529061"
---
# <a name="xamarinforms-device-class"></a>Класс устройств Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)

[ `Device` ](xref:Xamarin.Forms.Device) Класс содержит несколько свойств и методов, чтобы помочь разработчикам настраивать макет и функций на каждой платформы.

Помимо методов и свойств, предназначенных для целевого кода на конкретных типах и размерах оборудования `Device` , класс включает методы, которые можно использовать для взаимодействия с элементами управления пользовательского интерфейса из фоновых потоков. Дополнительные сведения см. в разделе [взаимодействие с интерфейсом пользователя из фоновых потоков](#interact-with-the-ui-from-background-threads).

## <a name="providing-platform-specific-values"></a>Предоставление значений для конкретной платформы

До Xamarin.Forms 2.3.4, платформы, приложение было запущено на удалось получить, изучив [ `Device.OS` ](xref:Xamarin.Forms.Device.OS) свойство и его для сравнения [ `TargetPlatform.iOS` ](xref:Xamarin.Forms.TargetPlatform.iOS), [ `TargetPlatform.Android` ](xref:Xamarin.Forms.TargetPlatform.Android), [ `TargetPlatform.WinPhone` ](xref:Xamarin.Forms.TargetPlatform.WinPhone), и [ `TargetPlatform.Windows` ](xref:Xamarin.Forms.TargetPlatform.Windows) значений перечисления. Аналогично, один из [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) перегрузки может использоваться для предоставления значений, зависящих от платформы к элементу управления.

Тем не менее поскольку Xamarin.Forms 2.3.4 эти API устарели и заменены. [ `Device` ](xref:Xamarin.Forms.Device) Класс теперь содержит общедоступный строковые константы, определяющие платформ — [ `Device.iOS` ](xref:Xamarin.Forms.Device.iOS), [ `Device.Android` ](xref:Xamarin.Forms.Device.Android), `Device.WinPhone`() Устаревшая версия), `Device.WinRT` (устаревшая версия), [ `Device.UWP` ](xref:Xamarin.Forms.Device.UWP), и [ `Device.macOS` ](xref:Xamarin.Forms.Device.macOS). Аналогичным образом [ `Device.OnPlatform` ](xref:Xamarin.Forms.Device.OnPlatform(System.Action,System.Action,System.Action,System.Action)) перегрузки были заменены [ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) и [ `On` ](xref:Xamarin.Forms.On) API-интерфейсы.

В C#, специфические для платформы значения можно задать путем создания `switch` оператором в [ `Device.RuntimePlatform` ](xref:Xamarin.Forms.Device.RuntimePlatform) свойства, а затем предоставить `case` инструкций для необходимых платформ:

```csharp
double top;
switch (Device.RuntimePlatform)
{
  case Device.iOS:
    top = 20;
    break;
  case Device.Android:
  case Device.UWP:
  default:
    top = 0;
    break;
}
layout.Margin = new Thickness(5, top, 5, 0);
```

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) И [ `On` ](xref:Xamarin.Forms.On) классы предоставляют те же функциональные возможности, в XAML:

```xaml
<StackLayout>
  <StackLayout.Margin>
    <OnPlatform x:TypeArguments="Thickness">
      <On Platform="iOS" Value="0,20,0,0" />
      <On Platform="Android, UWP" Value="0,0,0,0" />
    </OnPlatform>
  </StackLayout.Margin>
  ...
</StackLayout>
```

[ `OnPlatform` ](xref:Xamarin.Forms.OnPlatform`1) Класс — это универсальный класс, который должен быть создан при помощи `x:TypeArguments` атрибут, который соответствует целевому типу. В [ `On` ](xref:Xamarin.Forms.On) класс, [ `Platform` ](xref:Xamarin.Forms.On.Platform) атрибут может принимать один `string` значение или несколько разделенных запятыми `string` значения.

> [!IMPORTANT]
> Предоставляя некорректное `Platform` значение в атрибута `On` не приведут к ошибке. Вместо этого код будет выполняться без значения платформы, которые применяются.

Кроме того `OnPlatform` расширение разметки, которая может использоваться в XAML для настройки внешнего вида пользовательского интерфейса на каждой платформы. Дополнительные сведения см. в разделе [расширение разметки OnPlatform](~/xamarin-forms/xaml/markup-extensions/consuming.md#onplatform).

## <a name="deviceidiom"></a>Device.Idiom

`Device.Idiom` Свойство может использоваться для изменения макеты или функциональные возможности в зависимости от устройства приложение выполняется на. [ `TargetIdiom` ](xref:Xamarin.Forms.TargetIdiom) Перечисление содержит следующие значения:

- **Телефон** — iPhone, iPod touch и уже, чем 600 частные интерфейсы Android ^
- **Tablet** — iPad, Windows и Android устройств шире, чем 600 спады ^
- **Рабочий стол** — возвращаются только в [приложений универсальной платформы Windows](~/xamarin-forms/platform/windows/installation/index.md) на настольных компьютерах Windows 10 (возвращает `Phone` на мобильных устройствах Windows, в том числе в сценариях непрерывное множество решений)
- **TV** — Tizen TV устройств
- **Контрольные значения** — Tizen watch устройств
- **Неподдерживаемый** – не используется

*^ частные интерфейсы не обязательно является количество физических пикселей*

`Idiom` Свойство особенно полезно для создания макетов, которые используют преимущества экраны больше, следующим образом:

```csharp
if (Device.Idiom == TargetIdiom.Phone) {
    // layout views vertically
} else {
    // layout views horizontally for a larger display (tablet or desktop)
}
```

[ `OnIdiom` ](xref:Xamarin.Forms.OnIdiom`1) Класс предоставляет те же функциональные возможности, в XAML:

```xaml
<StackLayout>
    <StackLayout.Margin>
        <OnIdiom x:TypeArguments="Thickness">
            <OnIdiom.Phone>0,20,0,0</OnIdiom.Phone>
            <OnIdiom.Tablet>0,40,0,0</OnIdiom.Tablet>
            <OnIdiom.Desktop>0,60,0,0</OnIdiom.Desktop>
        </OnIdiom>
    </StackLayout.Margin>
    ...
</StackLayout>
```

[ `OnIdiom` ](xref:Xamarin.Forms.OnPlatform`1) Класс — это универсальный класс, который должен быть создан при помощи `x:TypeArguments` атрибут, который соответствует целевому типу.

Кроме того `OnIdiom` расширение разметки, которая может использоваться в XAML для настройки внешнего вида пользовательского интерфейса на идиому устройства, приложения на основе. Дополнительные сведения см. в разделе [расширение разметки OnIdiom](~/xamarin-forms/xaml/markup-extensions/consuming.md#onidiom).

## <a name="deviceflowdirection"></a>Device.FlowDirection

[ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) Значение извлекает [ `FlowDirection` ](xref:Xamarin.Forms.FlowDirection) значение перечисления, представляющее текущее направление потока, который используется устройством. Направление потока — это направление, в котором глаз человека перемещается по элементам пользовательского интерфейса на странице. Перечисление имеет следующие значения.

- [`LeftToRight`](xref:Xamarin.Forms.FlowDirection.LeftToRight)
- [`RightToRight`](xref:Xamarin.Forms.FlowDirection.RightToLeft)
- [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent)

В XAML [ `Device.FlowDirection` ](xref:Xamarin.Forms.VisualElement.FlowDirection) значение можно получить с помощью `x:Static` расширение разметки:

```xaml
<ContentPage ... FlowDirection="{x:Static Device.FlowDirection}"> />
```

Эквивалентный код на C# является:

```csharp
this.FlowDirection = Device.FlowDirection;
```

Дополнительные сведения о направлении, см. в разделе [справа налево локализации](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="devicestyles"></a>Device.Styles

[ `Styles` Свойство](~/xamarin-forms/user-interface/styles/index.md) содержит определения встроенных стилей, которые могут применяться для некоторых элементов управления (такие как `Label`) `Style` свойство. Ниже приведены доступные стили.

* BodyStyle
* CaptionStyle
* ListItemDetailTextStyle
* ListItemTextStyle
* SubtitleStyle
* TitleStyle

## <a name="devicegetnamedsize"></a>Device.GetNamedSize

`GetNamedSize` можно использовать при задании [ `FontSize` ](~/xamarin-forms/user-interface/text/fonts.md) в C# кода:

```csharp
myLabel.FontSize = Device.GetNamedSize (NamedSize.Small, myLabel);
someLabel.FontSize = Device.OnPlatform (
      24,         // hardcoded size
      Device.GetNamedSize (NamedSize.Medium, someLabel),
      Device.GetNamedSize (NamedSize.Large, someLabel)
);
```

## <a name="deviceopenuri"></a>Device.OpenUri

`OpenUri` Метод может использоваться для запуска операций на базовой платформы, такие как открытие URL-адрес собственного веб-браузера (**Safari** в iOS или **Internet** в Android).

```csharp
Device.OpenUri(new Uri("https://evolve.xamarin.com/"));
```

[Пример WebView](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithWebview/WorkingWithWebview/WebAppPage.cs) включает пример использования `OpenUri` открывать URL-адреса и сработать телефонные звонки.

[Пример Maps](https://github.com/xamarin/xamarin-forms-samples/blob/master/WorkingWithMaps/WorkingWithMaps/MapAppPage.cs) также использует `Device.OpenUri` для отображения карт и направлений, используя собственные **сопоставляет** приложения на iOS и Android.

## <a name="devicestarttimer"></a>Device.StartTimer

`Device` Класс также имеет `StartTimer` метод, который предоставляет простой способ запустить задачи, зависящие от времени, работающая в общем коде Xamarin.Forms, включая библиотеку .NET Standard. Передайте `TimeSpan` для установки интервала и возврата `true` следует таймер или `false` для ее остановки после текущего вызова.

```csharp
Device.StartTimer (new TimeSpan (0, 0, 60), () => {
    // do something every 60 seconds
    return true; // runs again, or false to stop
});
```

Если код внутри таймер взаимодействует с помощью пользовательского интерфейса (такие как установка текст `Label` или отображая оповещение) должно выполняться внутри `BeginInvokeOnMainThread` выражение (см. ниже).

## <a name="interact-with-the-ui-from-background-threads"></a>Взаимодействие с пользовательским интерфейсом из фоновых потоков

Большинство операционных систем, в том числе iOS, Android и универсальная платформа Windows, используют однопотоковую модель для кода, включающего пользовательский интерфейс. Этот поток часто называют *основным потоком* или *потоком пользовательского интерфейса*. Следствием этой модели является то, что весь код, обращающийся к элементам пользовательского интерфейса, должен выполняться в основном потоке приложения.

Приложения иногда используют фоновые потоки для выполнения потенциально длительных операций, таких как извлечение данных из веб-службы. Если код, выполняемый в фоновом потоке, должен иметь доступ к элементам пользовательского интерфейса, он должен запустить этот код в основном потоке.

Класс включает следующие `static` методы, которые можно использовать для взаимодействия с элементами пользовательского интерфейса из фоновых потоков: `Device`

| Метод | Аргументы | Returns | Цель |
|---|---|---|---|
| `BeginInvokeOnMainThread` | `Action` | `void` | Вызывает объект `Action` в основном потоке и не ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>` | `Func<T>` | `Task<T>` | Вызывает объект `Func<T>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Action` | `Task` | Вызывает объект `Action` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync<T>`| `Func<Task<T>>` | `Task<T>` | Вызывает объект `Func<Task<T>>` в основном потоке и ожидает его завершения. |
| `InvokeOnMainThreadAsync` | `Func<Task>` | `Task` | Вызывает объект `Func<Task>` в основном потоке и ожидает его завершения. |
| `GetMainThreadSynchronizationContextAsync` | | `Task<SynchronizationContext>` | `SynchronizationContext` Возвращает для основного потока. |

В следующем коде показан пример использования `BeginInvokeOnMainThread` метода:

```csharp
Device.BeginInvokeOnMainThread (() =>
{
    // interact with UI elements
});
```

## <a name="related-links"></a>Связанные ссылки

- [Пример устройства](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithdevice)
- [Образец стилей](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithstyles)
- [Устройство](xref:Xamarin.Forms.Device)
