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
ms.openlocfilehash: e76684ffb293380c283153c35c907acc50e40aab
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84128080"
---
# <a name="home-indicator-visibility-on-ios"></a>Видимость индикатора дома в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS задает видимость индикатора Home на [`Page`](xref:Xamarin.Forms.Page) . Он используется в XAML путем установки [`Page.PrefersHomeIndicatorAutoHidden`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.Page.PrefersHomeIndicatorAutoHiddenProperty) Свойства BIND в значение `boolean` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
             ios:Page.PrefersHomeIndicatorAutoHidden="true">
    ...
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetPrefersHomeIndicatorAutoHidden(true);
```

`Page.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `Page.SetPrefersHomeIndicatorAutoHidden` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Page. Сетпрефершомеиндикатораутохидден ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Page}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен управляет видимостью индикатора "начало". Кроме того, [ `Page.PrefersHomeIndicatorAutoHidden` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Page. Префершомеиндикатораутохидден ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Page})). можно использовать метод, чтобы получить видимость индикатора "начало".

В результате можно контролировать видимость индикатора Home на элементе [`Page`](xref:Xamarin.Forms.Page) управления:

![Снимок экрана с отображением индикатора "домой" на странице iOS](page-home-indicator-images/home-indicator-visibility.png "Видимость индикатора домашней страницы")

> [!NOTE]
> Эта конкретная платформа может применяться к [`ContentPage`](xref:Xamarin.Forms.ContentPage) объектам, [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) , [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) и [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) .

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
