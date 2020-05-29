---
title: ''
description: Описание всех элементов пользовательского интерфейса, используемых для создания Xamarin.Forms приложения. В этой статье перечислены группы элементов управления, которые составляют пользовательский интерфейс Xamarin.Forms приложения.
ms.prod: ''
ms.assetid: ''
ms.technology: ''
author: ''
ms.author: ''
ms.date: ''
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: e843f0e42f4f66a6ce4e60c2f5d8a233d19f1df6
ms.sourcegitcommit: 57bc714633364aeb34aba9803e88802bebf321ba
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/28/2020
ms.locfileid: "84136400"
---
# <a name="controls-reference"></a>Справочник по элементам управления

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery/)

Пользовательский интерфейс Xamarin.Forms приложения состоит из объектов, которые сопоставляются с собственными элементами управления каждой целевой платформы. Это позволяет приложениям для конкретных платформ для iOS, Android и универсальная платформа Windows использовать Xamarin.Forms код, содержащийся в [библиотеке .NET Standard](~/cross-platform/app-fundamentals/net-standard.md).

Ниже приведены четыре основные группы управления, используемые для создания пользовательского интерфейса Xamarin.Forms приложения.

- [**Страницы**](pages.md)
- [**Макеты**](layouts.md)
- [**Представления**](views.md)
- [**Ячейки**](cells.md)

Xamarin.FormsСтраница обычно занимает весь экран. Страница обычно содержит макет, который содержит представления и, возможно, другие макеты. Ячейки — это специализированные компоненты, используемые в соединении с [`TableView`](views.md#tableview) и [`ListView`](views.md#listview) . Схема классов, в которой показана иерархия типов, которые обычно используются для построения пользовательского интерфейса в, Xamarin.Forms можно найти в [ Xamarin.Forms иерархии классов элементов управления](~/xamarin-forms/internals/class-hierarchy.md).

В четырех статьях о [**страницах**](pages.md), [**макетах**](layouts.md), [**представлениях**](views.md)и [**ячейках**](cells.md)каждый тип элемента управления описывается ссылками на документацию по API, статья, описывающая ее использование (если она существует), и один или несколько примеров программ (если они существуют). Каждый тип элемента управления также сопровождается снимком экрана, показывающим страницу из примера [**формсгаллери**](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery) , работающего на устройствах iOS и Android. Под каждым снимком экрана расположены ссылки на исходный код страницы C#, эквивалентную страницу XAML и (при необходимости) файл кода программной части C# для страницы XAML.

> [!NOTE]
> Страницы, макеты и представления являются производными от `VisualElement` класса. `VisualElement`Класс предоставляет разнообразные свойства, методы и события, которые полезны при создании производных классов. Дополнительные сведения см. в разделе [висуалелемент Properties, Methods and Events](common-properties.md).

Помимо элементов управления, предоставляемых с Xamarin.Forms , доступны сторонние элементы управления. Дополнительные сведения см. в разделе [сторонние элементы управления](thirdparty.md).

## <a name="related-links"></a>Связанные ссылки

- [Xamarin.FormsПример Формсгаллери](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/formsgallery)
- [Xamarin.FormsУправляет иерархией классов](~/xamarin-forms/internals/class-hierarchy.md)
- [Документация по API-интерфейсам](https://docs.microsoft.com/dotnet/api/xamarin.forms?view=xamarin-forms)
