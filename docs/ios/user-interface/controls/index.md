---
title: Элементы управления пользовательского интерфейса в Xamarin. iOS
description: В этом документе содержатся ссылки на руководства, в которых описываются различные элементы управления пользовательского интерфейса iOS, доступные для разработчиков Xamarin. iOS. Связанное содержимое рассказывает о предупреждениях, кнопках, представлениях коллекций, изображениях, ручных элементах управления камеры, картах, метках, подборах, средствах выбора даты и т. д.
ms.prod: xamarin
ms.assetid: C00EA232-ADCC-42AD-BF86-B526414A21C6
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: 336e2468a3532b300697ed7fe596864e1bcf9622
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91433964"
---
# <a name="user-interface-controls-in-xamarinios"></a>Элементы управления пользовательского интерфейса в Xamarin. iOS

В этом документе представлены некоторые из наиболее распространенных элементов управления пользовательского интерфейса iOS и способы их использования.

## <a name="alerts"></a>[Оповещения](alerts.md)

Начиная с iOS 8, Уиалертконтроллер заменил Уиактионшит, а Уиалертвиев оба, которые теперь являются устаревшими.

## <a name="buttons"></a>[Кнопки](buttons.md)

Класс Уибуттон используется для представления различных стилей кнопок на экранах iOS. В этом разделе описываются различные варианты работы с кнопками в iOS.

## <a name="collection-views"></a>[Представления коллекций](uicollectionview.md)

Представления коллекций, доступные в `UICollectionView` классе, являются новой концепцией в iOS 6, что приводит к показу нескольких элементов на экране с помощью макетов. Шаблоны для предоставления данных для `UICollectionView` создания элементов и взаимодействия с этими элементами следуют тем же моделям делегирования и источников данных, которые обычно используются в разработке iOS.

## <a name="images"></a>[Изображения](image.md)

Добавление изображений в приложение требует выполнения двух шагов: сначала добавьте изображения в проект. Затем добавьте элементы управления и код для их отображения на экране. Дополнительные сведения об обработке изображений в Xamarin. iOS см. в статье [Работа с изображениями](~/ios/app-fundamentals/images-icons/index.md) .

## <a name="manual-camera-controls"></a>[Ручные элементы управления камерой](intro-to-manual-camera-controls.md)

Ручная камера, предоставляемая `AVFoundation Framework` в iOS 8, позволяет мобильному приложению получить полный контроль над камерой устройства iOS. Такой детализированный уровень контроля можно использовать для создания высококачественных приложений камеры и предоставления самокомпозиций путем настройки параметров камеры с сохранением изображения или видео.

## <a name="maps"></a>[Карты](ios-maps/index.md)

Карты являются распространенной функцией во всех современных мобильных операционных системах. iOS предлагает встроенную поддержку сопоставления с помощью платформы Map Kit Framework. С помощью Map Kit приложения могут легко добавлять насыщенные Интерактивные карты. Эти карты могут быть настроены различными способами, такими как добавление заметок для маркировки расположений на карте и переопределение изображений произвольных фигур. Map Kit даже имеет встроенную поддержку отображения текущего расположения устройства.

## <a name="labels"></a>[Метки](labels.md)

`UILabel`Элемент управления используется для отображения одного и многострочного текста, предназначенного только для чтения.

## <a name="pickers-and-date-pickers"></a>[Выборки и выбор даты](picker.md)

Элемент управления "выбор" отображает элемент управления "колесико", который содержит прокручиваемый список значений с выделенным выбранным значением. Пользователи поворачивают колесо, чтобы выбрать нужный вариант.

Один из вариантов, заданных пользователем для выбора даты и времени. Для предоставления этому Apple создал настраиваемый подкласс класса Уипиккервиев с именем Уидатепиккер.

## <a name="progress-and-activity-indicators"></a>[Индикаторы активности и хода выполнения](progress-activity-indicator.md)

iOS предоставляет два основных способа указания хода выполнения в приложении: Индикаторы действий (включая конкретный индикатор активности _сети_ ) и индикаторы выполнения.

## <a name="search-bars"></a>[Панели поиска](searchbar.md)

Уисеарчбар используется для поиска по списку значений. 

## <a name="sliders-switches-and-segmented-controls"></a>[Ползунки, переключатели и сегментированные элементы управления](slider-switch-segmented-controls.md)

Элемент управления "ползунок" позволяет просто выбрать числовое значение в диапазоне. iOS использует `UISwitch` как логическое входное значение, которое может быть представлено переключателем на других платформах. Сегментированный элемент управления — это организованный способ, позволяющий пользователям взаимодействовать с небольшим количеством параметров.

## <a name="stack-view"></a>[Представление стека](uistackview.md)

Элемент управления "представление стека" ( `UIStackView` ) использует возможности автоматического макета и классов размеров для управления стеком подпросмотров, по горизонтали или по вертикали, которые динамически отвечают на ориентацию и размер экрана устройства iOS.

## <a name="tables-and-cells"></a>[Таблицы и ячейки](tables/index.md)

в его разделе представлены классы, используемые для создания и отображения таблиц, а также примеры их использования в Xamarin. iOS. Он охватывает использование внешнего вида по умолчанию для таблиц, настройку макета, реализацию редактирования и использование конструктора Xamarin iOS для визуального проектирования таблицы. Иногда отображение представляет собой список строк (например, музыкальное приложение) и в других случаях трудно распознать элемент управления таблицы (например, редактирование в приложении "Контакты" или диалог в приложении "сообщения").

## <a name="text-input"></a>[Текстовый ввод](text-input.md)

Прием ввода текста пользователем осуществляется с помощью `UITextField` для однострочных входов и уитекствиев для многострочного редактируемого текста. Можно перетащить любой из этих элементов управления на экран и дважды щелкнуть, чтобы задать исходный текст.

## <a name="tab-bars-and-tab-bar-controllers"></a>[Панели вкладок и контроллеры панелей вкладок](creating-tabbed-applications.md)

приложения iOS, использующие пользовательский интерфейс навигации по вкладкам, создаются с помощью класса Уитаббарконтроллер. В этой статье мы рассмотрим, как настроить приложение с вкладками, которое содержит несколько контроллеров и представлений. Затем мы изучим, как загрузить Уитаббарконтроллер, если он не является корневым контроллером, например после экрана входа в систему.

## <a name="web-views"></a>[Веб-представления](webview.md)

В этой статье мы рассмотрим веб-представления, предоставляемые Apple — `WKWebview` и `SFSafariViewController` — их сходства и различия, а также способы их использования.

## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](/samples/xamarin/ios-samples/controls)