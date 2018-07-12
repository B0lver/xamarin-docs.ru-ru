---
title: 'Xamarin.Essentials: Сведения о приложении'
description: Этот документ описывает класс AppInfo в Xamarin.Essentials, который предоставляет сведения о приложении. Например он предоставляет, имя приложения и версии.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 7e79b3003f41b8de22950624e44e8c9e0e7e7e31
ms.sourcegitcommit: 632955f8cdb80712abd8dcc30e046cb9c435b922
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2018
ms.locfileid: "38831511"
---
# <a name="xamarinessentials-app-information"></a>Xamarin.Essentials: Сведения о приложении

![Предварительные версии NuGet](~/media/shared/pre-release.png)

**AppInfo** класс предоставляет сведения о приложении.

## <a name="using-appinfo"></a>С помощью AppInfo

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-application-information"></a>Получение сведений о приложении:

Через API предоставляются следующие сведения:

```csharp
// Application Name
var appName = AppInfo.Name;

// Package Name/Application Identifier (com.microsoft.testapp)
var packageName = AppInfo.PackageName;

// Application Version (1.0.0)
var version = AppInfo.VersionString;

// Application Build Number (1)
var build = AppInfo.BuildString;
```

## <a name="displaying-application-settings"></a>Отображение параметров приложения

**AppInfo** класс также можно отобразить страницу параметров, которые поддерживаются операционной системой для приложения:

```csharp
// Display settings page
AppInfo.OpenSettings();
```

Эта страница параметров позволяет пользователю изменить разрешения приложения и выполнять другие задачи с платформой.

## <a name="api"></a>API

- [Исходный код AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Документация по AppInfo API](xref:Xamarin.Essentials.AppInfo)
