---
title: Как перейти на использование более ранней версии пакета NuGet
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 2375F833-A630-471E-B8E9-5AD2CB81F264
author: conceptdev
ms.author: crdun
ms.date: 05/08/2018
ms.openlocfilehash: fc504d1d7a9e990b3bcd90b3404e7db3d9fe7527
ms.sourcegitcommit: 13e43f510da37ad55f1c2f5de1913fb0aede6362
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021132"
---
# <a name="how-do-i-downgrade-a-nuget-package"></a>Как перейти на использование более ранней версии пакета NuGet

Visual Studio для Mac & Visual Studio имеют функции для выбора старых версий пакетов и их автоматической установки. Аналогично тому, как работает обновление пакетов. Эти действия описаны ниже.

## <a name="visual-studio"></a>Visual Studio

1. Последовательно выберите **инструменты > диспетчер пакетов NuGet > консоль диспетчера пакетов** .
2. Задание проекта в качестве **проекта по умолчанию**
3. Используйте следующий синтаксис:

    > Install-Package [PackageName] — версия [вкладка для меню "версия"]

Можно также скопировать и вставить точную команду из страницы NuGet пакета. Пример для Xamarin. Forms:[https://www.nuget.org/packages/Xamarin.Forms/](https://www.nuget.org/packages/Xamarin.Forms/)

## <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. В проекте щелкните правой кнопкой мыши папку пакеты & выберите команду **Добавить пакеты** .
2. В сеарчбар для поиска необходимых пакетов можно использовать следующий синтаксис:

    `[PackageName] version:*`

### <a name="examples"></a>Примеры 
- Список всех пакетов Xamarin. Forms: 

    `Xamarin.Forms version:`

- Список всех пакетов Xamarin. Forms 1.4. x: 

    `Xamarin.Forms version:1.4`

> [!NOTE]
> Если добавить пробел между `version:` & номера версии, поиск будет вести себя так, как будто версия не была указана.
