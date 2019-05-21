---
title: Fast ListView, прокрутка в Android
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать Android конкретных платформ, позволяет пользователям быстро прокручивать данные в ListView.
ms.prod: xamarin
ms.assetid: 37D95A2D-74AC-488A-B903-2BDD799EAA5C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 07/10/2018
ms.openlocfilehash: 6ae266f9309e32c79ec6028d737cfdcaaa85b0d4
ms.sourcegitcommit: b23a107b0fe3d2f814ae35b52a5855b6ce2a3513
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2019
ms.locfileid: "65926568"
---
# <a name="listview-fast-scrolling-on-android"></a>Fast ListView, прокрутка в Android

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)

Это Android платформы используется для включения быстрой прокрутке данные в [ `ListView` ](xref:Xamarin.Forms.ListView). Он используется в XAML, задав `ListView.IsFastScrollEnabled` вложенное свойство, чтобы `boolean` значение:

```xaml
<ContentPage ...
             xmlns:android="clr-namespace:Xamarin.Forms.PlatformConfiguration.AndroidSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout Margin="20">
        ...
        <ListView ItemsSource="{Binding GroupedEmployees}"
                  GroupDisplayBinding="{Binding Key}"
                  IsGroupingEnabled="true"
                  android:ListView.IsFastScrollEnabled="true">
            ...
        </ListView>
    </StackLayout>
</ContentPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.AndroidSpecific;
...

var listView = new Xamarin.Forms.ListView { IsGroupingEnabled = true, ... };
listView.SetBinding(ItemsView<Cell>.ItemsSourceProperty, "GroupedEmployees");
listView.GroupDisplayBinding = new Binding("Key");
listView.On<Android>().SetIsFastScrollEnabled(true);
```

`ListView.On<Android>` Метод указывает, что этой платформы будет выполняться только в Android. `ListView.SetIsFastScrollEnabled` Метод в [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен, используемый для обеспечения быстрой прокрутке данные в [ `ListView` ](xref:Xamarin.Forms.ListView). Кроме того `SetIsFastScrollEnabled` метод можно использовать для переключения быстрой прокрутке путем вызова `IsFastScrollEnabled` метод для возврата, включен ли быстрый прокрутка:

```csharp
listView.On<Android>().SetIsFastScrollEnabled(!listView.On<Android>().IsFastScrollEnabled());
```

Результатом является то, что быстрой прокрутке данные в [ `ListView` ](xref:Xamarin.Forms.ListView) можно включить, который изменяет размер бегунка прокрутки:

[![](listview-fast-scrolling-images/fastscroll.png "ListView FastScroll платформы")](listview-fast-scrolling-images/fastscroll-large.png#lightbox "ListView FastScroll платформы")

## <a name="related-links"></a>Связанные ссылки

- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/PlatformSpecifics/)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [AndroidSpecific API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific)
- [AndroidSpecific.AppCompat API](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat)
