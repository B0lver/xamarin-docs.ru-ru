---
title: Таббедпаже полупрозрачная панель вкладок в iOS
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS для установки режима полупрозрачность панели вкладок в Таббедпаже.
ms.prod: xamarin
ms.assetid: 9581AE81-9557-47AD-8B07-25A1AC5F055B
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/16/2020
ms.openlocfilehash: c321884039674d3abb1a4b510ddfe2c062c28211
ms.sourcegitcommit: 10b4d7952d78f20f753372c53af6feb16918555c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/26/2020
ms.locfileid: "77646734"
---
# <a name="tabbedpage-translucent-tab-bar-on-ios"></a>Таббедпаже полупрозрачная панель вкладок в iOS

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Эта платформа iOS предназначена для установки режима полупрозрачность панели вкладок на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage). Он используется в XAML путем установки для свойства `TabbedPage.TranslucencyMode` BIND значения `TranslucencyMode` перечисления:

```xaml
<TabbedPage ...
            xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core"
            ios:TabbedPage.TranslucencyMode="Opaque">
    ...
</TabbedPage>
```

Кроме того его можно будет использовать с помощью C# с помощью текучего API:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

On<iOS>().SetTranslucencyMode(TranslucencyMode.Opaque);
```

Метод `TabbedPage.On<iOS>` указывает, что эта платформа будет запускаться только в iOS. Метод `TabbedPage.SetTranslucencyMode` в пространстве имен [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) используется для установки режима полупрозрачность панели вкладок на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) , указывая одно из следующих `TranslucencyMode` перечисляемых значений:

- `Default`, который задает для панели вкладок режим полупрозрачность по умолчанию. Это значение по умолчанию для свойства `TabbedPage.TranslucencyMode`.
- `Transparent`, который задает прозрачность панели вкладок.
- `Opaque`, который задает непрозрачную панель вкладок.

Кроме того, можно использовать метод `GetTranslucencyMode` для получения текущего значения перечисления `TranslucencyMode`, применяемого к [`TabbedPage`](xref:Xamarin.Forms.TabbedPage).

В результате можно установить режим полупрозрачность панели вкладок на [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) :

![Снимок экрана с полупрозрачными и непрозрачными панелями табуляции в iOS](tabbedpage-translucent-tabbar-images/translucencymodes.png "Полупрозрачные и непрозрачные панели вкладок")

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
