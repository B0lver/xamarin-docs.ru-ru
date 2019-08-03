---
title: Переключатель Xamarin. Forms
description: Параметр Xamarin. Forms — это тип кнопки, которая может управляться пользователем для переключения между состояниями. В этой статье объясняется, как использовать класс Switch для отображения переключаемого элемента пользовательского интерфейса.
ms.prod: xamarin
ms.assetId: B2F9CC65-481B-4323-8E77-C6BE29C90DE9
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/03/2019
ms.openlocfilehash: 58755c54ce2afe80a8bf43adc25a0cf2d90a0bb5
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739456"
---
# <a name="xamarinforms-switch"></a>Переключатель Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)

Xamarin. Forms [`Switch`](xref:Xamarin.Forms.Switch) — это горизонтальная кнопка, которая может управляться пользователем для переключения между и выключенными состояниями, которые представлены `boolean` значением. Класс наследует от [`View`.](xref:Xamarin.Forms.View) `Switch`

На следующем снимке экрана `Switch` показан элемент управления в состояниях переключателя **On** и **Off** в iOS и Android:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-default.png "Параметры в iOS и Android")

`Switch` Элемент управления определяет два свойства:

* [`OnColor`](xref:Xamarin.Forms.Switch.OnColor)параметр, который влияет на то `Switch` , как объект отображается в переключенном или **включенном**состоянии. `Color`
* [`IsToggled`](xref:Xamarin.Forms.Switch.IsToggled)значение, указывающее, включено ли в. `Switch` `boolean`

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектом. Это означает, `Switch` что можно использовать стиль и цель привязок данных.

Элемент управления определяет событие, которое `IsToggled` возникает при изменении свойстваспомощьюманипуляциипользователяиликогдаприложениезадаетсвойство.`IsToggled` `Toggled` `Switch` Объект, `Value`сопровождающий событие, имеет одно свойство с именем типа `bool`. `Toggled` `ToggledEventArgs` При срабатывании события значение `Value` свойства отражает новое значение `IsToggled` свойства.

## <a name="create-a-switch"></a>Создание переключателя

Экземпляр `Switch` можно создать в XAML. Его `IsToggled` свойство можно задать для `Switch`переключения. По умолчанию `IsToggled` свойство имеет `false`значение. В следующем примере показано, как создать экземпляр `Switch` в XAML с помощью необязательного `IsToggled` набора свойств:

```xaml
<Switch IsToggled="true"/>
```

Также `Switch` можно создать в коде:

```csharp
Switch switch = new Switch { IsToggled = true };
```

### <a name="switch-style-properties"></a>Переключить свойства стиля

Свойство может быть задано для `Switch` определения цвета, когда он переключается в состояние **On.** `OnColor` В следующем примере показано, как создать экземпляр `Switch` в XAML `OnColor` с помощью набора свойств:

```xaml
<Switch OnColor="Orange" />
```

Это свойство также может быть задано при `Switch` создании в коде: `OnColor`

```csharp
Switch switch = new Switch { OnColor = Color.Orange };
```

На следующем снимке экрана `Switch` показано включение и **выключение** состояния переключателя со `OnColor` свойством `Color.Orange` , установленным в iOS и Android:

![Снимок экрана параметров включения и отключения состояний, в iOS и Android](switch-images/switch-states-oncolor.png "Параметры в iOS и Android")

## <a name="respond-to-a-switch-state-change"></a>Ответ на изменение состояния переключения

Когда свойство изменяется либо с помощью пользовательской манипуляции, либо когда приложение `IsToggled` задает свойство, `Toggled` возникает событие. `IsToggled` Для реагирования на изменение можно зарегистрировать обработчик событий для этого события:

```xaml
<Switch Toggled="OnToggled" />
```

Файл кода содержит обработчик `Toggled` событий:

```csharp
void OnToggled(object sender, ToggledEventArgs e)
{
    // Perform an action after examining e.Value
}
```

Аргумент в обработчике событий `Switch` является ответственным за срабатывание этого события. `sender` `sender` Свойство можно использовать для `Switch` доступа к объекту или для различения нескольких `Switch` объектов, совместно использующих один `Toggled` и тот же обработчик событий.

Обработчик `Toggled` событий можно также назначить в коде:

```csharp
Switch switch = new Switch {...};
switch.Toggled += (sender, e) =>
{
    // Perform an action after examining e.Value
}
```

## <a name="data-bind-a-switch"></a>Привязка данных коммутатора

Обработчик событий можно исключить с помощью привязки данных и триггеров, чтобы реагировать `Switch` на изменяющиеся состояния переключения. `Toggled`

```xaml
<Switch x:Name="styleSwitch" />
<Label Text="Lorem ipsum dolor sit amet, elit rutrum, enim hendrerit augue vitae praesent sed non, lorem aenean quis praesent pede.">
    <Label.Triggers>
        <DataTrigger TargetType="Label"
                     Binding="{Binding Source={x:Reference styleSwitch}, Path=IsToggled}"
                     Value="true">
            <Setter Property="FontAttributes"
                    Value="Italic, Bold" />
            <Setter Property="FontSize"
                    Value="Large" />
        </DataTrigger>
    </Label.Triggers>
</Label>
```

В [`Label`](xref:Xamarin.Forms.Label) этом примере компонент использует выражение привязки `IsToggled` `DataTrigger` в для `Switch` отслеживания свойства с именем `styleSwitch`. Когда это свойство примет `true`значение `FontAttributes` , изменяются свойства `Label` и `FontSize` объекта. `FontAttributes` `FontSize` Когда свойство возвращает значение `false`, свойства и объекта`Label` сбрасываются в исходное состояние. `IsToggled`

Дополнительные сведения о триггерах см. в разделе [триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md).

## <a name="disable-a-switch"></a>Отключение переключателя

Приложение может перейти в состояние, в котором `Switch` переключаемый объект не является допустимой операцией. В таких случаях `Switch` можно отключить, `IsEnabled` задав свойству `false`значение. Это предотвратит возможность пользователей управлять `Switch`.

## <a name="related-links"></a>Связанные ссылки

* [Переключить демонстрации](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-switchdemos/)
* [Триггеры Xamarin. Forms](~/xamarin-forms/app-fundamentals/triggers.md)
