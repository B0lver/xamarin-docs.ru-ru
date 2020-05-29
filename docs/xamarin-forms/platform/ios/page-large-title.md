---
title: ''
description: ''
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 0db20620870340386ccd0cedf7f98cb2975527ba
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128053"
---
# <a name="large-page-titles-on-ios"></a>Крупные заголовки страниц в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS используется для вывода заголовка страницы в виде большого заголовка панели навигации [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , для устройств, использующих iOS 11 или более поздней версии. Крупное название выводится по левому краю и использует более крупный шрифт, а переход к стандартному названию происходит по мере того, как пользователь начинает прокручивать содержимое, чтобы эффективное использование экрана было эффективно. Однако в альбомной ориентации заголовок вернется в центр панели навигации, чтобы оптимизировать макет содержимого. Он используется в XAML путем присвоения `NavigationPage.PrefersLargeTitles` свойству присоединенного свойства `boolean` значения:

```xaml
<NavigationPage xmlns="http://xamarin.com/schemas/2014/forms"
                xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
                xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
                ...
                ios:NavigationPage.PrefersLargeTitles="true">
  ...
</NavigationPage>
```

Кроме того, его можно использовать в C# с помощью API Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

var navigationPage = new Xamarin.Forms.NavigationPage(new iOSLargeTitlePageCS());
navigationPage.On<iOS>().SetPrefersLargeTitles(true);
```

`NavigationPage.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `NavigationPage.SetPrefersLargeTitle`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен определяет, включены ли большие заголовки.

При условии, что крупные заголовки включены в [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) , все страницы в стеке навигации будут отображать крупные заголовки. Это поведение можно переопределить на страницах, задав `Page.LargeTitleDisplay` для присоединенного свойства значение `LargeTitleDisplayMode` перечисления:

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             Title="Large Title"
             ios:Page.LargeTitleDisplay="Never">
  ...
</ContentPage>
```

Кроме того, поведение страницы можно переопределить из C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

public class iOSLargeTitlePageCS : ContentPage
{
    public iOSLargeTitlePageCS(ICommand restore)
    {
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        ...
    }
    ...
}
```

`Page.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. `Page.SetLargeTitleDisplay`Метод в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен управляет большим поведением заголовка в [`Page`](xref:Xamarin.Forms.Page) , а `LargeTitleDisplayMode` перечисление предоставляет три возможных значения:

- `Always`— принудительно использовать большой формат панели навигации и размера шрифта.
- `Automatic`— Используйте тот же стиль (крупный или маленький), что и предыдущий элемент в стеке навигации.
- `Never`— принудительное использование панели навигации с обычным и небольшим форматом.

Кроме того, `SetLargeTitleDisplay` метод можно использовать для переключения значений перечисления путем вызова `LargeTitleDisplay` метода, который возвращает текущий объект `LargeTitleDisplayMode` :

```csharp
switch (On<iOS>().LargeTitleDisplay())
{
    case LargeTitleDisplayMode.Always:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Automatic);
        break;
    case LargeTitleDisplayMode.Automatic:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Never);
        break;
    case LargeTitleDisplayMode.Never:
        On<iOS>().SetLargeTitleDisplay(LargeTitleDisplayMode.Always);
        break;
}
```

В результате заданный объект `LargeTitleDisplayMode` применяется к [`Page`](xref:Xamarin.Forms.Page) , который управляет поведением крупного заголовка:

![](page-large-title-images/large-title.png "Blur Effect Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
