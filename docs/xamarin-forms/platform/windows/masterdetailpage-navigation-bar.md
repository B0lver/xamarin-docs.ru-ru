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
ms.openlocfilehash: d3ae71685c7aaebdceacb5f8b7cd5f3dd308407c
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84137256"
---
# <a name="masterdetailpage-navigation-bar-on-windows"></a>Панель навигации Мастердетаилпаже в Windows

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Этот универсальная платформа Windows для конкретной платформы используется для свертывания панели навигации в [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) и используется в XAML путем установки [`MasterDetailPage.CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapseStyleProperty) [`MasterDetailPage.CollapsedPaneWidth`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.MasterDetailPage.CollapsedPaneWidthProperty) свойств и присоединенные свойства:

```xaml
<MasterDetailPage ...
                  xmlns:windows="clr-namespace:Xamarin.Forms.PlatformConfiguration.WindowsSpecific;assembly=Xamarin.Forms.Core"
                  windows:MasterDetailPage.CollapseStyle="Partial"
                  windows:MasterDetailPage.CollapsedPaneWidth="48">
  ...
</MasterDetailPage>

```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.WindowsSpecific;
...

page.On<Windows>().SetCollapseStyle(CollapseStyle.Partial).CollapsedPaneWidth(148);
```

`MasterDetailPage.On<Windows>`Метод указывает, что эта платформа будет запускаться только в Windows. [ `Page.SetCollapseStyle` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Мастердетаилпаже. Сетколлапсестиле ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Мастердетаилпаже}, Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Коллапсестиле)). в [`Xamarin.Forms.PlatformConfiguration.WindowsSpecific`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространстве имен используется для указания стиля свертывания с [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) перечислением, предоставляющим два значения: [`Full`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Full) и [`Partial`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle.Partial) . [ `MasterDetailPage.CollapsedPaneWidth` ] (Xref: Xamarin.Forms . Платформконфигуратион. ВиндовсспеЦифик. Мастердетаилпаже. Коллапседпаневидс ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. Windows, Xamarin.Forms . Мастердетаилпаже}, System. Double)) используется для указания ширины частично свернутой панели навигации.

Результат заключается в том, что к [`CollapseStyle`](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific.CollapseStyle) [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) экземпляру применяется указанный атрибут, ширина которого также определяется:

[![](masterdetailpage-navigation-bar-images/collapsed-navigation-bar.png "Collapsed Navigation Bar Platform-Specific")](masterdetailpage-navigation-bar-images/collapsed-navigation-bar-large.png#lightbox "Collapsed Navigation Bar Platform-Specific")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ВиндовсспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific)
