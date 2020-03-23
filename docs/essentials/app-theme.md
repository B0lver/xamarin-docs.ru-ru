---
title: 'Xamarin.Essentials: Тема приложения'
description: В этой статье описывается API запрошенной темы приложения в Xamarin.Essentials, где предоставляются сведения о том, какой стиль темы запрашивается для работающего приложения.
ms.assetid: F6F6D496-A8A9-4B9A-AF1A-370D937E5073
author: jamesmontemagno
ms.author: jamont
ms.date: 01/06/2020
ms.openlocfilehash: e31cae6ff639dbe261599a7cf78ae31fc09318b3
ms.sourcegitcommit: c83b55f60ece20e9163b3e587130250fdf113a16
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/12/2020
ms.locfileid: "79190318"
---
# <a name="xamarinessentials-app-theme"></a>Xamarin.Essentials: Тема приложения

API **RequestedTheme** входит в состав класса [`AppInfo`](app-information.md) и предоставляет сведения о том, какая тема запрашивается системой для работающего приложения.

## <a name="get-started"></a>Начало работы

[!include[](~/essentials/includes/get-started.md)]

## <a name="using-requestedtheme"></a>Использование RequestedTheme

Добавьте в свой класс ссылку на Xamarin.Essentials:

```csharp
using Xamarin.Essentials;
```

## <a name="obtaining-theme-information"></a>Получение сведений о теме

Запрошенную тему приложения можно обнаружить с помощью API следующим образом:

```csharp
AppTheme appTheme = AppInfo.RequestedTheme;

```

Это позволит системе получить текущую запрошенную тему для вашего приложения. Будет возвращаться одно из следующих значений:

* Не указан
* Светлый
* Темный

Если операционная система не имеет определенного стиля пользовательского интерфейса для запроса, то будет возвращено значение "Не указан". Например, на устройствах под управлением iOS более поздних версий, чем 13.0.


## <a name="platform-implementation-specifics"></a>Особенности реализации для платформ

# <a name="android"></a>[Android](#tab/android)

В Android используются режимы конфигурации для указания типа темы, запрашиваемой у пользователя. В зависимости от версии Android пользователь может изменить его, а также он изменяется при включенном режиме экономии заряда.

Дополнительные сведения о темной теме для Android см. [здесь](https://developer.android.com/guide/topics/ui/look-and-feel/darktheme).


# <a name="ios"></a>[iOS](#tab/ios)

Значение "Не указан" всегда будет возвращаться для iOS более поздних версий, чем 13.0. 


# <a name="uwp"></a>[UWP](#tab/uwp)

По умолчанию приложение работает, используя тему, заданную пользователем в параметрах Windows (**Параметры > Персонализация > Цвета > Выбрать режим приложения по умолчанию**). Можно задать свойство RequestedTheme приложения, чтобы переопределить значение по умолчанию для пользователя и указать, какую тему использовать.

Дополнительные сведения по запрошенной теме UWP см. [здесь](https://docs.microsoft.com/uwp/api/windows.ui.xaml.application.requestedtheme).

--------------

## <a name="api"></a>API

- [Исходный код AppInfo](https://github.com/xamarin/Essentials/tree/master/Xamarin.Essentials/AppInfo)
- [Документация по API AppInfo](xref:Xamarin.Essentials.AppInfo)
