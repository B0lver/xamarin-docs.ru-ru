---
title: Скроллвиевное содержимое касается iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая определяет, обрабатывает ли Скроллвиев жест касания или передает его в его содержимое.
ms.prod: xamarin
ms.assetid: 99F823DB-B379-40F0-A343-A9783C341120
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/24/2018
ms.openlocfilehash: 154666cce4ad6c53949952fa93f5ad7dc89824ab
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68651761"
---
# <a name="scrollview-content-touches-on-ios"></a>Скроллвиевное содержимое касается iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Таймеру неявное активируется в том случае, когда начинается жест касания в [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) в iOS и `ScrollView` решает, в зависимости от действий пользователя в течение интервала таймера, он должен обрабатывать жест или передайте его на его содержимое. По умолчанию iOS `ScrollView` содержимого штрихи задержки, но это может вызвать проблемы в некоторых случаях при работе с `ScrollView` содержимое, не выиграть жест, когда это требуется. Таким образом, эта определяет специфические для платформы ли `ScrollView` жест касания обрабатывает или передает его на его содержимое. Он используется в XAML, задав `ScrollView.ShouldDelayContentTouches` вложенное свойство, чтобы `boolean` значение:

```xaml
<MasterDetailPage ...
                  xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <MasterDetailPage.Master>
        <ContentPage Title="Menu" BackgroundColor="Blue" />
    </MasterDetailPage.Master>
    <MasterDetailPage.Detail>
        <ContentPage>
            <ScrollView x:Name="scrollView" ios:ScrollView.ShouldDelayContentTouches="false">
                <StackLayout Margin="0,20">
                    <Slider />
                    <Button Text="Toggle ScrollView DelayContentTouches" Clicked="OnButtonClicked" />
                </StackLayout>
            </ScrollView>
        </ContentPage>
    </MasterDetailPage.Detail>
</MasterDetailPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

scrollView.On<iOS>().SetShouldDelayContentTouches(false);
```

`ScrollView.On<iOS>` Метод указывает, что этой платформы будет выполняться только на устройствах iOS. `ScrollView.SetShouldDelayContentTouches` Метод в [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен, используемый для управления ли [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) жест касания обрабатывает или передает его на его содержимое. Кроме того `SetShouldDelayContentTouches` метод можно использовать для переключения задержки содержимого штрихи, вызвав `ShouldDelayContentTouches` метод для возврата, отложена ли штрихи содержимого:

```csharp
scrollView.On<iOS>().SetShouldDelayContentTouches(!scrollView.On<iOS>().ShouldDelayContentTouches());
```

В результате [ `ScrollView` ](xref:Xamarin.Forms.ScrollView) можно отключить, задерживая получение содержимого штрихи, таким образом, в этом сценарии [ `Slider` ](xref:Xamarin.Forms.Slider) получает жест, а не [ `Detail` ](xref:Xamarin.Forms.MasterDetailPage.Detail) странице [ `MasterDetailPage` ](xref:Xamarin.Forms.MasterDetailPage):

[![](scrollview-content-touches-images/scrollview-delay-content-touches.png "Задержка ScrollView содержимое касается платформы")](scrollview-content-touches-images/scrollview-delay-content-touches-large.png#lightbox "ScrollView задержки содержимого затрагивает специфические для платформы")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
