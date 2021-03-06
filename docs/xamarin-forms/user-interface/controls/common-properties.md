---
title: Xamarin.FormsОбщие свойства, методы и события элемента управления
description: В этой статье описываются общие свойства, методы и события, определенные в классе Висуалелемент, которые обычно используются в производных классах.
ms.prod: xamarin
ms.assetId: 85A0CCF5-C1D8-40BB-927F-A4D944E5534D
ms.technology: xamarin-forms
author: profexorgeek
ms.author: jusjohns
ms.date: 07/22/2020
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: b4c0ef44f528e3cbc56a27e98a1c38246736ff8c
ms.sourcegitcommit: 08290d004d1a7e7ac579bf1f96abf8437921dc70
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/07/2020
ms.locfileid: "87918346"
---
# <a name="no-locxamarinforms-common-control-properties-methods-and-events"></a>Xamarin.FormsОбщие свойства, методы и события элемента управления

Xamarin.Forms `VisualElement` Класс является базовым классом для большинства элементов управления, используемых в Xamarin.Forms приложении. `VisualElement`Класс определяет множество [свойств](#properties), [методов](#methods)и [событий](#events) , используемых в производных классах.

## <a name="properties"></a>Свойства

Для объектов доступны следующие свойства [`VisualElement`](xref:Xamarin.Forms.VisualElement) .

### [`AnchorX`](xref:Xamarin.Forms.VisualElement.AnchorX)

`AnchorX`Свойство — это `double` значение, определяющее центральную точку на оси X для преобразований, таких как масштабирование и вращение. Значение по умолчанию — 0,5.

### [`AnchorY`](xref:Xamarin.Forms.VisualElement.AnchorY)

`AnchorY`Свойство — это `double` значение, определяющее центральную точку на оси X для преобразований, таких как масштабирование и вращение. Значение по умолчанию — 0,5.

### `Background`

`Background`Свойство является `Brush` значением, позволяющим использовать кисти в качестве фона в любом элементе управления. Значение по умолчанию — `Brush.Default`.

### [`BackgroundColor`](xref:Xamarin.Forms.VisualElement.BackgroundColor)

`BackgroundColor`Свойство имеет значение `Color` , определяющее цвет фона элемента управления. Если не задано, то фон будет объектом по умолчанию `Color` , который визуализируется как прозрачный.

### [`Behaviors`](xref:Xamarin.Forms.VisualElement.Behaviors)

`Behaviors`Свойство имеет значение объекта `List` `Behavior` . Поведения позволяют присоединять многократно используемые функции к элементам, добавляя их в `Behaviors` список. Дополнительные сведения о `Behavior` классе см. в разделе [ Xamarin.Forms варианты поведения](~/xamarin-forms/app-fundamentals/behaviors/index.md).

### [`Bounds`](xref:Xamarin.Forms.VisualElement.Bounds)

`Bounds`Свойство является объектом только для чтения `Rectangle` , представляющим пространство, занимаемое элементом управления. `Bounds`Значение свойства назначается во время цикла макета. `Rectangle` `struct` Содержит полезные свойства и методы для тестирования пересечения и включения прямоугольников. Дополнительные сведения см. в разделе [ Xamarin.Forms API Rectangle](xref:Xamarin.Forms.Rectangle).

### `Clip`

`Clip`Свойство — это `Geometry` объект, определяющий контур содержимого элемента. Чтобы определить клип, используйте объект, `Geometry` например, `EllipseGeometry` для установки `Clip` свойства элемента. Будут видны только области, находящиеся в пределах области геометрии. Дополнительные сведения см. в разделе [Clip с геометрическим](~/xamarin-forms/user-interface/shapes/geometries.md#clip-with-a-geometry)объектом.

### [`Effects`](xref:Xamarin.Forms.Element.Effects)

`Effects`Свойство имеет объект `List` `Effect` , унаследованный от класса `Element` (xref: Xamarin.Forms . Класс element). Эффекты позволяют настраивать собственные элементы управления и обычно используются для небольших изменений стилей. Дополнительные сведения о `Effect` классе см. в разделе [ Xamarin.Forms эффекты](~/xamarin-forms/app-fundamentals/effects/index.md).

### [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection)

`FlowDirection`Свойство является `FlowDirection` значением перечисления. Направлением потока можно присвоить значение `MatchParent` , `LeftToRight` или `RightToLeft` и определить порядок разметки и направление. `FlowDirection`Свойство обычно используется для поддержки языков, которые читаются справа налево.

### [`Height`](xref:Xamarin.Forms.VisualElement.Height)

`Height`Свойство является значением только для чтения `double` , которое описывает отображаемую высоту элемента управления. `Height`Свойство вычисляется во время цикла макета и не может быть задано напрямую. Высоту элемента управления можно запросить с помощью [Свойства хеигхтрекуест](#heightrequest).

### [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest)

`HeightRequest`Свойство является `double` значением, определяющим нужную высоту элемента управления. Абсолютная высота элемента управления может не соответствовать запрошенному значению. Дополнительные сведения см. в разделе [Свойства запроса](#request-properties).

### [`InputTransparent`](xref:Xamarin.Forms.VisualElement.InputTransparent)

`InputTransparent`Свойство — это значение `bool` , которое определяет, получает ли элемент управления ввод данных пользователем. Значение по умолчанию — `false` , гарантируя, что элемент получает входные данные. Это свойство передает в дочерние элементы, если оно задано. Присвоение `InputTransparent` свойству значения `true` класса макета приведет к тому, что все элементы в макете не будут получать входные данные.

### [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled)

`IsEnabled`Свойство — это `bool` значение, которое определяет, реагирует ли элемент управления на вводимые пользователем данные. Значение по умолчанию — `true`. Если задать для этого свойства значение false, элемент управления не сможет принимать введенные пользователем данные.

### [`IsFocused`](xref:Xamarin.Forms.VisualElement.IsFocused)

`IsFocused`Свойство — это `bool` значение, которое описывает, является ли элемент управления в данный момент объектом, на который он нацелен. Вызов [`Focus`](#focus) метода в элементе управления приведет к тому, что `IsFocused` значение будет равно true. При вызове [`Unfocus`](#unfocus) метода этому свойству будет присвоено значение false.

### [`IsTabStop`](xref:Xamarin.Forms.VisualElement.IsTabStop)

`IsTabStop`Свойство — это `bool` значение, которое определяет, получает ли элемент управления фокус при перемещении пользователя через элементы управления с помощью клавиши TAB. Если это свойство имеет значение false, `TabIndex` свойство не будет действовать.

### [`IsVisible`](xref:Xamarin.Forms.VisualElement.IsVisible)

`IsVisible`Свойство является `bool` значением, определяющим, отображается ли элемент управления. Элементы управления со `IsVisible` свойством, имеющим значение false, не будут отображаться, не будут рассматриваться для вычисления пробелов во время цикла макета и не могут принимать данные, вводимые пользователем.

### [`MinimumHeightRequest`](xref:Xamarin.Forms.VisualElement.MinimumHeightRequest)

`MinimumHeightRequest`Свойство — это `double` значение, которое определяет, как переполнение обрабатывается, когда два элемента конкурируют за ограниченное пространство. Задание `MinimumHeightRequest` Свойства позволяет процессу макета масштабировать элемент до требуемого минимального размера. Если не `MinimumHeightRequest` указано, значение по умолчанию равно-1, и процесс макета будет считаться `HeightRequest` минимальным значением. Это означает, что у элементов без `MinimumHeightRequest` значений не будет масштабируемой высоты.

Дополнительные сведения см. в разделе [минимальные свойства запроса](#minimum-request-properties).

### [`MinimumWidthRequest`](xref:Xamarin.Forms.VisualElement.MinimumWidthRequest)

`MinimumWidthRequest`Свойство — это `double` значение, которое определяет, как переполнение обрабатывается, когда два элемента конкурируют за ограниченное пространство. Задание `MinimumWidthRequest` Свойства позволяет процессу макета масштабировать элемент до требуемого минимального размера. Если не `MinimumWidthRequest` указано, значение по умолчанию равно-1, и процесс макета будет считаться `WidthRequest` минимальным значением. Это означает, что у элементов без `MinimumWidthRequest` значений не будет масштабируемой ширины.

Дополнительные сведения см. в разделе [минимальные свойства запроса](#minimum-request-properties).

### [`Opacity`](xref:Xamarin.Forms.VisualElement.Opacity)

`Opacity`Свойство — это `double` значение от нуля до единицы, определяющее непрозрачность элемента управления во время отрисовки. Значение этого свойства по умолчанию равно 1,0. Значения за пределами диапазона от 0 до 1 будут заноситься в срез. `Opacity`Свойство применяется только в том случае, если [`IsVisible`](#isvisible) свойство имеет значение `true` . Непрозрачность применяется итеративно. Поэтому, если родительский элемент управления имеет 0,5 Opacity, а его дочерний объект имеет уровень непрозрачности 0,5, дочерний объект будет отображен с действующим значением непрозрачности 0,25. Присвоение `Opacity` свойству элемента управления вводом значения 0 имеет неопределенное поведение.

### [`Parent`](xref:Xamarin.Forms.Element.Parent)

Свойство `Parent` наследуется от класса `Element`. Это свойство является `Element` объектом, который является родительским для элемента управления. `Parent`Свойство обычно задается автоматически для элемента, когда он добавляется как дочерний элемент другого элемента.

### [`Resources`](xref:Xamarin.Forms.VisualElement.Resources)

`Resources`Свойство — это `ResourceDictionary` экземпляр, который заполняется парами "ключ-значение", которые обычно заполняются во время выполнения из XAML. Этот словарь позволяет разработчикам приложений использовать объекты, определенные в XAML, как во время компиляции, так и во время выполнения. Ключи в словаре заполняются из `x:Key` атрибута ТЕГА XAML. Объект, созданный из XAML, вставляется в `ResourceDictionary` для указанного ключа. После инициализации.

Дополнительные сведения см. в разделе [словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md).

### [`Rotation`](xref:Xamarin.Forms.VisualElement.Rotation)

`Rotation`Свойство имеет `double` значение от 0 до 360, определяющее поворот оси Z в градусах. Значение по умолчанию для этого свойства равно 0. Поворот применяется относительно [`AnchorX`](#anchorx) [`AnchorY`](#anchory) значений и.

### [`RotationX`](xref:Xamarin.Forms.VisualElement.RotationX)

`RotationX`Свойство имеет `double` значение от 0 до 360, определяющее поворот оси X в градусах. Значение по умолчанию для этого свойства равно 0. Поворот применяется относительно [`AnchorX`](#anchorx) [`AnchorY`](#anchory) значений и.

### [`RotationY`](xref:Xamarin.Forms.VisualElement.RotationY)

`RotationY`Свойство имеет `double` значение от 0 до 360, определяющее поворот оси Y в градусах. Значение по умолчанию для этого свойства равно 0. Поворот применяется относительно [`AnchorX`](#anchorx) [`AnchorY`](#anchory) значений и.

### [`Scale`](xref:Xamarin.Forms.VisualElement.Scale)

`Scale`Свойство — это `double` значение, определяющее масштаб элемента управления. Значение этого свойства по умолчанию равно 1,0. Масштаб применяется относительно [`AnchorX`](#anchorx) [`AnchorY`](#anchory) значений и.

### [`ScaleX`](xref:Xamarin.Forms.VisualElement.ScaleX)

`ScaleX`Свойство — это `double` значение, определяющее масштаб элемента управления вдоль оси X. Значение этого свойства по умолчанию равно 1,0. `ScaleX`Свойство применяется относительно [`AnchorX`](#anchorx) значения.

### [`ScaleY`](xref:Xamarin.Forms.VisualElement.ScaleY)

`ScaleY`Свойство — это `double` значение, определяющее масштаб элемента управления вдоль оси Y. Значение этого свойства по умолчанию равно 1,0. `ScaleY`Свойство применяется относительно [`AnchorY`](#anchory) значения.

### [`Style`](xref:Xamarin.Forms.NavigableElement.Style)

Свойство `Style` наследуется от класса `NavigableElement`. Это свойство является экземпляром `Style` класса. `Style`Класс содержит триггеры, методы задания и поведение, определяющие внешний вид и поведение визуальных элементов. Дополнительные сведения см. в разделе [ Xamarin.Forms стили XAML](~/xamarin-forms/user-interface/styles/xaml/index.md).

### [`StyleClass`](xref:Xamarin.Forms.NavigableElement.StyleClass)

`StyleClass`Свойство представляет собой список `string` объектов, представляющих имена `Style` классов. Это свойство наследуется от класса `NavigableElement`. `StyleClass`Свойство позволяет применять к экземпляру несколько атрибутов стиля `VisualElement` . Дополнительные сведения см. в разделе [ Xamarin.Forms классы стилей](~/xamarin-forms/user-interface/styles/xaml/style-class.md).

### [`TabIndex`](xref:Xamarin.Forms.VisualElement.TabIndex)

`TabIndex`Свойство является `int` значением, определяющим порядок элементов управления при переходе между элементами управления с помощью клавиши TAB. `TabIndex`Свойство является реализацией для свойства, определенного в `ITabStopElement` интерфейсе, который `VisualElement` реализуется классом.

### [`TranslationX`](xref:Xamarin.Forms.VisualElement.TranslationX)

`TranslationX`Свойство является `double` значением, определяющим Дельта-преобразование, которое должно применяться к оси X. Перевод применяется после макета и обычно используется для применения анимации. Преобразование элемента за пределами родительского контейнера My предотвращает вход в него.

Дополнительные сведения см. [в разделе Анимация Xamarin.Forms в ](~/xamarin-forms/user-interface/animation/index.md).

### [`TranslationY`](xref:Xamarin.Forms.VisualElement.TranslationY)

`TranslationY`Свойство является `double` значением, определяющим Дельта-преобразование, которое должно быть применено к оси Y. Перевод применяется после макета и обычно используется для применения анимации. Преобразование элемента за пределами родительского контейнера My предотвращает вход в него.

Дополнительные сведения см. [в разделе Анимация Xamarin.Forms в ](~/xamarin-forms/user-interface/animation/index.md).

### [`Triggers`](xref:Xamarin.Forms.VisualElement.Triggers)

`Triggers`Свойство доступно только `List` для чтения `TriggerBase` объектов. Триггеры позволяют разработчикам приложений выражать действия в XAML, которые изменяют внешний вид элементов управления в ответ на изменения события или свойства. Дополнительные сведения см. в разделе [ Xamarin.Forms Triggers](~/xamarin-forms/app-fundamentals/triggers.md).

### [`Visual`](xref:Xamarin.Forms.VisualElement.Visual)

`Visual`Свойство является `IVisual` экземпляром, позволяющим создавать модули подготовки отчетов и выборочно применять их к `VisualElement` экземплярам. `Visual`Свойство установлено в соответствие с родительским, поэтому определение модуля подготовки отчетов в компоненте также будет применяться к любым дочерним элементам этого компонента. Если для элемента управления или его предков не задан пользовательский модуль подготовки отчетов, будет использоваться модуль подготовки отчетов по умолчанию Xamarin.Forms . Дополнительные сведения см. в разделе [ Xamarin.Forms Visual](~/xamarin-forms/user-interface/visual/index.md).

### [`Width`](xref:Xamarin.Forms.VisualElement.Width)

`Width`Свойство является значением только для чтения `double` , которое описывает отображаемую ширину элемента управления. `Width`Свойство вычисляется во время цикла макета и не может быть задано напрямую. Ширину элемента управления можно запросить с помощью [Свойства видсрекуест](#widthrequest).

### [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest)

`WidthRequest`Свойство — это `double` значение, определяющее требуемую ширину элемента управления. Абсолютная ширина элемента управления может не соответствовать запрошенному значению. Дополнительные сведения см. в разделе [Свойства запроса](#request-properties).

### [`X`](xref:Xamarin.Forms.VisualElement.X)

`X`Свойство является значением только для чтения `double` , которое описывает текущую X-координату элемента управления.

### [`Y`](xref:Xamarin.Forms.VisualElement.Y)

`Y`Свойство является значением только для чтения `double` , которое описывает текущую координату Y элемента управления.

## <a name="methods"></a>Методы

В классе доступны следующие методы `VisualElement` . Полный список см. в разделе [методы API висуалелемент](xref:Xamarin.Forms.VisualElement#methods).

### [`FindByName`](xref:Xamarin.Forms.Element.FindByName*)

`FindByName`Метод наследуется от `Element` класса и имеет следующую сигнатуру:

```csharp
public object FindByName (string name)
```

Этот метод выполняет поиск всех дочерних элементов для указанного `name` аргумента и возвращает элемент с указанным именем. Если совпадения не найдены, то возвращается значение `null`.

### [`Focus`](xref:Xamarin.Forms.VisualElement.Focus)

`Focus`Метод пытается установить фокус на элемент. Этот метод имеет следующую сигнатуру:

```csharp
public bool Focus ()
```

`Focus`Метод возвращает `true` значение, если фокус клавиатуры был успешно установлен, и `false` Если вызов метода не привел к изменению фокуса. Для работы этого метода элемент должен иметь возможность получения фокуса. Вызов `Focus` метода для элементов, настоящих или нереальных, имеет неопределенное поведение.

### [`Unfocus`](xref:Xamarin.Forms.VisualElement.Unfocus)

`Unfocus`Метод пытается удалить фокус на элементе. Этот метод имеет следующую сигнатуру:

```csharp
public void Unfocus ()
```

Элемент уже должен иметь фокус, чтобы этот метод работал.

## <a name="events"></a>События

В классе доступны следующие события `VisualElement` . Полный список см. в разделе [ Xamarin.Forms висуалелемент Events](xref:Xamarin.Forms.VisualElement#events).

### [`Focused`](xref:Xamarin.Forms.VisualElement.Focused)

`Focused`Событие возникает всякий раз, когда `VisualElement` экземпляр получает фокус. Это событие не перемещается через Xamarin.Forms стек, оно получается непосредственно из собственного элемента управления. Это событие создается методом [`IsFocused`](#isfocused) задания свойства.

### [`SizeChanged`](xref:Xamarin.Forms.VisualElement.SizeChanged)

`SizeChanged`Событие возникает при `VisualElement` `Height` изменении экземпляра или `Width` свойств. Если разработчики хотят реагировать непосредственно на изменение размера, вместо того чтобы реагировать на событие после изменения, они должны реализовать [`OnSizeAllocated`](xref:Xamarin.Forms.VisualElement.OnSizeAllocated*) виртуальный метод.

### [`Unfocused`](xref:Xamarin.Forms.VisualElement.Unfocused)

`Unfocused`Событие возникает всякий раз, когда `VisualElement` экземпляр теряет фокус. Это событие не перемещается через Xamarin.Forms стек, оно получается непосредственно из собственного элемента управления. Это событие создается методом [`IsFocused`](#isfocused) задания свойства.

## <a name="units-of-measurement"></a>Единицы измерения

Платформы Android, iOS и UWP имеют разные единицы измерения, которые могут различаться на разных устройствах. Xamarin.Formsиспользует независимую от платформы единицу измерения, которая нормализует единицы между устройствами и платформами. 160 единиц на дюйм, или 64 единиц на сантиметр в Xamarin.Forms .

## <a name="request-properties"></a>Свойства запроса

Свойства, имена которых содержат "Request", определяют требуемое значение, которое может не совпадать с фактическим отображаемым значением. Например, `HeightRequest` можно задать значение 150, но если макет допускает наличие пространства только для единиц 100, то отображаемый `Height` элемент управления будет содержать только 100. Размер отображаемого содержимого зависит от доступного пространства и содержащихся в нем компонентов.

## <a name="minimum-request-properties"></a>Свойства минимального запроса

Минимальные свойства запроса включают `MinimumHeightRequest` и `MinimumWidthRequest` , и предназначены для более точного контроля над тем, как элементы вызывают переполнение при относительных значениях. Однако поведение макета, связанное с этими свойствами, имеет некоторые важные соображения.

### <a name="unspecified-minimum-property-values"></a>Неуказанные минимальные значения свойств

Если минимальное значение не задано, по умолчанию для свойства Minimum используется значение-1. Процесс макета пропускает это значение и считает абсолютное значение минимальным. Практический результат такого поведения заключается в том, что элемент без минимально заданного значения **не будет** сжиматься. Элемент с минимальным заданным значением **будет** сжиматься.

В следующем коде XAML показаны два `BoxView` элемента по горизонтали `StackLayout` :

```xaml
<StackLayout Orientation="Horizontal">
    <BoxView HeightRequest="100" BackgroundColor="Purple" WidthRequest="500"></BoxView>
    <BoxView HeightRequest="100" BackgroundColor="Green" WidthRequest="500" MinimumWidthRequest="250"></BoxView>
</StackLayout>
```

Первый `BoxView` экземпляр запрашивает ширину 500 и не задает минимальную ширину. Второй `BoxView` экземпляр запрашивает ширину 500 и минимальную ширину 250. Если родительский `StackLayout` элемент недостаточно широк для того, чтобы вместить оба компонента с запрошенной шириной, первый `BoxView` экземпляр будет считаться, что процесс макета имеет минимальную ширину 500, так как не указано другое допустимое минимальное значение. Второй `BoxView` экземпляр может масштабироваться до 250, и он будет сжиматься до тех пор, пока его ширина не достигнет 250 единиц.

Если требуется, чтобы первый `BoxView` экземпляр был уменьшен без минимальной ширины, `MinimumWidthRequest` необходимо задать допустимое значение, например 0.

### <a name="minimum-and-absolute-property-values"></a>Минимальные и абсолютные значения свойств

Поведение не определено, если минимальное значение больше, чем абсолютное значение. Например, если `WidthRequest` задано значение 100, `MinimumWidthRequest` свойство не должно превышать 100. При указании минимального значения свойства всегда следует указывать абсолютное значение, чтобы гарантировать, что абсолютное значение больше минимального.

### <a name="minimum-properties-within-a-grid"></a>Минимальные свойства в сетке

`Grid`макеты имеют собственную систему для относительных размеров строк и столбцов. Использование `MinimumWidthRequest` или `MinimumHeightRequest` в `Grid` макете не приведет к применению. Дополнительные сведения см. в разделе [ Xamarin.Forms Grid](~/xamarin-forms/user-interface/layouts/grid.md).

## <a name="related-links"></a>Связанные ссылки

- [API Висуалелемент](xref:Xamarin.Forms.VisualElement)
