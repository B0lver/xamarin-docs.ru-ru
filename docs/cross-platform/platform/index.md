---
title: Поддержка языков программирования в Xamarin
description: В этом документе описываются различные языки программирования, поддерживаемые Xamarin. В нем C#обсуждаются F#,, переносимые шаблоны Visual Basic.NET и Razor.
ms.prod: xamarin
ms.assetid: CEE8C464-67D7-45F4-9614-EAEF5217CACC
author: conceptdev
ms.author: crdun
ms.date: 02/18/2018
ms.openlocfilehash: 95c63d446f961738ad71242671d632620316b879
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70290903"
---
# <a name="programming-language-support-in-xamarin"></a>Поддержка языков программирования в Xamarin

## <a name="c"></a>C\#

### <a name="async-support-overviewcross-platformplatformasyncmd"></a>[Общие сведения о поддержке асинхронного выполнения](~/cross-platform/platform/async.md)

В версии 5 C# появились два новых ключевых слова для выражения асинхронных операций: async и await. Эти ключевые слова позволяют написать простой код, использующий библиотеку параллельных задач для выполнения длительных операций (таких как сетевой доступ) в другом потоке и простого доступа к результатам по завершении. Последние версии Xamarin. iOS и Xamarin. Android поддерживают Async и await — этот документ содержит объяснения и пример использования нового синтаксиса с Xamarin.

### <a name="c-6-language-featurescross-platformplatformcsharp-sixmd"></a>[6 возможностей языка C#](~/cross-platform/platform/csharp-six.md)

Последняя версия C# языка — версия 6 — продолжит развивать язык, чтобы иметь меньшее количество шаблонов, улучшенную ясность и более согласованность. Синтаксис инициализации очистителя, возможность использования `await` в `catch/finally` блоках, а также условный `?` оператор null, особенно полезны.

## <a name="ffsharpindexmd"></a>[F#](fsharp/index.md)

Создание мобильных приложений с F# помощью и Xamarin.

## <a name="portable-visual-basicnetcross-platformplatformvisual-basicindexmd"></a>[Переносимая визуальная Basic.NET](~/cross-platform/platform/visual-basic/index.md)

Visual Studio поддерживает создание переносимых библиотек классов с помощью Visual Basic.NET, которые затем могут быть включены в приложения Xamarin. В этой статье показано, как создать новый Visual Basic PCL, а затем использовать его в примере приложения Xamarin. iOS, Xamarin. Android и Windows Phone.

## <a name="building-html-views-using-razor-templatescross-platformplatformrazor-html-templatesindexmd"></a>[Создание представлений HTML с помощью шаблонов Razor](~/cross-platform/platform/razor-html-templates/index.md)

Xamarin позволяет разработчикам использовать подсистему создания шаблонов Razor, впервые появившуюся с помощью ASP.NET C# MVC, а также легко сочетать данные с HTML, JavaScript и CSS без трудностей при создании HTML-строк в коде вручную.
В этой статье показано, как использовать шаблоны Razor с Xamarin для Android и iOS.
