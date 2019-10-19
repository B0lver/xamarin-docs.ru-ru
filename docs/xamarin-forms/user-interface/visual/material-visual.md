---
title: Визуальный элемент материалов Xamarin. Forms
description: Визуальный элемент "материалы Xamarin. Forms" можно использовать для создания приложений Xamarin. Forms, которые выглядят одинаково или в значительной степени идентичны в iOS и Android.
ms.prod: xamarin
ms.assetid: B774F68C-EF9E-49E1-B738-CDC64879ADA2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 03/12/2019
ms.openlocfilehash: b735541d51321231775b025745e68c54552697d3
ms.sourcegitcommit: dad4dfcd194b63ec9e903363351b6d9e543d4888
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/18/2019
ms.locfileid: "71198494"
---
# <a name="xamarinforms-material-visual"></a>Визуальный элемент материалов Xamarin. Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)

[Проектирование материалов](https://material.io) — это упрямого система разработки, созданная Google, которая предписывает размер, цвет, расстояния и другие аспекты того, как должны выглядеть и работать представления и макеты.

Визуальный элемент "материалы по Xamarin. Forms" можно использовать для применения правил проектирования материалов к приложениям Xamarin. Forms, создавая приложения, которые выглядят одинаково или в значительной степени идентичны в iOS и Android. Если визуальный элемент "материалы" включен, Поддерживаемые представления применяют один и тот же проект кросс-платформенный режим, создавая единообразный внешний вид. Это достигается с помощью модулей подготовки материалов, которые применяют правила проектирования материалов.

Процесс включения визуального элемента "материалы" в Xamarin. Forms в приложении:

1. Добавьте пакет NuGet [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) в проекты iOS и Android Platform. Этот пакет NuGet предоставляет оптимизированные для разработки материалов модули подготовки отчетов в iOS и Android. В iOS пакет предоставляет транзитивное зависимость для [Xamarin. iOS. материалкомпонентс](https://www.nuget.org/packages/Xamarin.iOS.MaterialComponents), которая является C# привязкой к [компонентам материалов Google для iOS](https://material.io/develop/ios/). В Android пакет предоставляет целевые объекты сборки, чтобы обеспечить правильную настройку TargetFramework.
1. Инициализируйте модули подготовки материалов в каждом проекте платформы. Дополнительные сведения см. в разделе [Инициализация модулей подготовки](#initialize-material-renderers)отчетов.
1. Чтобы использовать модули подготовки материалов, установите для свойства [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) значение `Material` на всех страницах, которые должны применять правила проектирования материалов. Дополнительные сведения см. в разделе Использование модулей подготовки отчетов к [просмотру](#consume-material-renderers).
1. используемых Настройте модули подготовки материалов. Дополнительные сведения см. в разделе Настройка модулей подготовки отчетов к [просмотру](#customize-material-renderers).

> [!IMPORTANT]
> В Android для модулей подготовки материалов требуется минимальная версия 5,0 (API 21) или более поздней версии, а также TargetFramework версии 9,0 (API 28). Кроме того, проект платформы требует наличия библиотек поддержки Android 28.0.0 или более поздней версии, и его тема должна наследовать от темы "компоненты материала" или продолжить наследовать от темы AppCompat. Дополнительные сведения см. в статье [Начало работы с материальными компонентами для Android](https://github.com/material-components/material-components-android/blob/master/docs/getting-started.md).

Модули подготовки материалов в настоящее время включены в пакет [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) NuGet для следующих представлений:

- [`Button`](xref:Xamarin.Forms.Button)
- `CheckBox`
- [`Entry`](xref:Xamarin.Forms.Entry)
- [`Frame`](xref:Xamarin.Forms.Frame)
- [`ProgressBar`](xref:Xamarin.Forms.ProgressBar)
- [`DatePicker`](xref:Xamarin.Forms.DatePicker)
- [`TimePicker`](xref:Xamarin.Forms.TimePicker)
- [`Picker`](xref:Xamarin.Forms.Picker)
- [`ActivityIndicator`](xref:Xamarin.Forms.ActivityIndicator)
- [`Editor`](xref:Xamarin.Forms.Editor)
- [`Slider`](xref:Xamarin.Forms.Slider)
- [`Stepper`](xref:Xamarin.Forms.Stepper)

Функционально, модули подготовки материалов по умолчанию не отличаются от модулей подготовки отчетов.

## <a name="initialize-material-renderers"></a>Инициализация модулей подготовки материалов

После установки пакета NuGet для [Xamarin. Forms. Visual. Material](https://www.nuget.org/packages/Xamarin.Forms.Visual.Material/) модули подготовки материалов должны быть инициализированы в каждом проекте платформы.

В iOS это должно произойти в **AppDelegate.CS** путем вызова метода `Xamarin.Forms.FormsMaterial.Init` *после* метода `Xamarin.Forms.Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init();
global::Xamarin.Forms.FormsMaterial.Init();
```

В Android это должно произойти в **MainActivity.CS** путем вызова метода `Xamarin.Forms.FormsMaterial.Init` *после* метода `Xamarin.Forms.Forms.Init`:

```csharp
global::Xamarin.Forms.Forms.Init(this, savedInstanceState);
global::Xamarin.Forms.FormsMaterial.Init(this, savedInstanceState);
```

## <a name="consume-material-renderers"></a>Использование модулей подготовки материалов

Приложения могут отказаться от использования модулей подготовки материалов, установив свойство [`VisualElement.Visual`](xref:Xamarin.Forms.VisualElement.Visual) на странице, макете или представлении, чтобы `Material`:

```xaml
<ContentPage Visual="Material"
             ...>
    ...
</ContentPage>
```

Эквивалентный код на C# выглядит так:

```csharp
ContentPage contentPage = new ContentPage();
contentPage.Visual = VisualMarker.Material;
```

Свойству [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) можно присвоить любой тип, реализующий `IVisual`, с классом [`VisualMarker`](xref:Xamarin.Forms.VisualMarker) , который предоставляет следующие свойства `IVisual`:

- `Default` — указывает, что представление должно отображаться с помощью модуля подготовки отчетов по умолчанию.
- `MatchParent` — указывает, что представление должно использовать тот же модуль подготовки отчетов, что и его непосредственного родителя.
- `Material` — указывает, что представление должно отображаться с помощью модуля подготовки материалов.

> [!IMPORTANT]
> Свойство [`Visual`](xref:Xamarin.Forms.VisualElement.Visual) определено в классе [`VisualElement`](xref:Xamarin.Forms.VisualElement) , в котором представления наследуют значение свойства `Visual` от своих родителей. Поэтому установка свойства `Visual` в [`ContentPage`](xref:Xamarin.Forms.ContentPage) гарантирует, что все поддерживаемые представления на странице будут использовать этот визуальный элемент. Кроме того, свойство `Visual` может быть переопределено в представлении.

На следующих снимках экрана показан пользовательский интерфейс, отображаемый с помощью модулей подготовки отчетов по умолчанию:

[![Снимок экрана модулей подготовки отчетов по умолчанию в iOS и Android](material-visual-images/default-renderers.png "Представления, использующие модули подготовки отчетов по умолчанию")](material-visual-images/default-renderers-large.png#lightbox)

На следующих снимках экрана показан один и тот же пользовательский интерфейс, отображаемый с помощью модулей подготовки материалов:

[![Снимок экрана модулей подготовки материалов в iOS и Android](material-visual-images/material-renderers.png "Представления, использующие модули подготовки материалов")](material-visual-images/material-renderers-large.png#lightbox)

Основные видимые различия между модулями подготовки отчетов по умолчанию и модулями подготовки материалов, показанными здесь, означают, что модули подготовки материалов записывают [`Button`](xref:Xamarin.Forms.Button) текста и округляют углы [`Frame`](xref:Xamarin.Forms.Frame) границ. Однако модули подготовки материалов используют собственные элементы управления, поэтому для таких областей, как шрифты, тени, цвета и повышения прав, могут существовать различия в пользовательском интерфейсе.

> [!NOTE]
> Компоненты проектирования материалов тесно соответствуют рекомендациям Google. В результате визуализаторы дизайна материалов перемещаются в сторону изменения размера и поведения. Если требуется больший контроль над стилями или поведением, можно по-прежнему создать собственный [результат](~/xamarin-forms/app-fundamentals/effects/index.md), [поведение](~/xamarin-forms/app-fundamentals/behaviors/index.md)или [пользовательский модуль подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) отчетов для получения необходимых сведений.

## <a name="customize-material-renderers"></a>Настройка модулей подготовки материалов

Модули подготовки материалов, как и модули подготовки отчетов по умолчанию, могут быть настроены с помощью следующих базовых классов:

- `MaterialButtonRenderer`
- `MaterialCheckBoxRenderer`
- `MaterialEntryRenderer`
- `MaterialFrameRenderer`
- `MaterialProgressBarRenderer`
- `MaterialDatePickerRenderer`
- `MaterialTimePickerRenderer`
- `MaterialPickerRenderer`
- `MaterialActivityIndicatorRenderer`
- `MaterialEditorRenderer`
- `MaterialSliderRenderer`
- `MaterialStepperRenderer`

В следующем примере кода показан пример настройки класса `MaterialProgressBarRenderer`.

```csharp
using Xamarin.Forms.Material.Android;

[assembly: ExportRenderer(typeof(ProgressBar), typeof(CustomMaterialProgressBarRenderer), new[] { typeof(VisualMarker.MaterialVisual) })]
namespace MyApp.Android
{
    public class CustomMaterialProgressBarRenderer : MaterialProgressBarRenderer
    {
        ...
    }
}
```

В этом примере `ExportRendererAttribute` указывает, что класс `CustomMaterialProgressBarRenderer` будет использоваться для отрисовки представления [`ProgressBar`](xref:Xamarin.Forms.ProgressBar) с типом `IVisual`, зарегистрированным в качестве третьего аргумента.

> [!NOTE]
> Модуль подготовки отчетов, указывающий тип `IVisual`, в составе его `ExportRendererAttribute`, будет использоваться для отрисовки включенных в представления, а не для модуля подготовки по умолчанию. Во время выбора модуля визуализации свойство `Visual` представления проверяется и включается в процесс выбора модуля подготовки отчетов.

Дополнительные сведения о пользовательских модулях подготовки отчетов см. в разделе [пользовательские модули подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

## <a name="related-links"></a>Связанные ссылки

- [Визуальный элемент "материал" (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-visualdemos)
- [Создание визуального модуля подготовки Xamarin. Forms](create.md)
- [Пользовательские отрисовщики](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)
