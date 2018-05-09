---
title: Сведения о приложении Xamarin.Essentials
description: Класс AppInfo предоставляет сведения о приложении.
ms.assetid: 15924FCB-19E0-45B2-944E-E94FD7AE12FA
ms.technology: xamarin-crossplatform
author: jamesmontemagno
ms.author: jamont
ms.date: 05/04/2018
ms.openlocfilehash: 5b235fdeb666c3f8f2c1b52a9691c931724ce2b8
ms.sourcegitcommit: 46d3c9daa45350bdd536d9e105517f3c1c753c5b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/07/2018
---
# <a name="xamarinessentials-app-information"></a>Сведения о приложении Xamarin.Essentials

![Предварительная версия NuGet](~/media/shared/pre-release.png)

**AppInfo** класс предоставляет сведения о приложении.

## <a name="using-appinfo"></a>С помощью AppInfo

Добавьте ссылку на Xamarin.Essentials в классе:

```csharp
using Xamarin.Essentials;
```

Через API-Интерфейс предоставляются следующие сведения:

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

## <a name="api"></a>API

- [AppInfo исходного кода](https://github.com/xamarin/Essentials/tree/master/Essentials/AppInfo)
- [Документация по AppInfo API](xref:Xamarin.Essentials.AppInfo)
