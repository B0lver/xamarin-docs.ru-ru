---
title: ListView Xamarin.Forms
description: Это руководство описывает ListView Xamarin.Forms, который может использоваться для представления данных в списках визуально привлекательные и интерактивные.
ms.prod: xamarin
ms.assetid: FEFDF7E0-720F-4BD1-863F-4477226AA695
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/14/2015
ms.openlocfilehash: f05703babd3f6e67713dfccdb1a1fc6a4ea6966e
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70228021"
---
# <a name="xamarinforms-listview"></a>ListView Xamarin.Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)

[`ListView`](xref:Xamarin.Forms.ListView)представляет собой представление для представления списков данных, особенно длинных списков, требующих прокрутки.

> [!IMPORTANT]
> Представление [`CollectionView`](xref:Xamarin.Forms.CollectionView) служит для вывода списков данных с различными спецификациями макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView). Дополнительные сведения см. в статье [Представление CollectionView в Xamarin.Forms](~/xamarin-forms/user-interface/collectionview/index.md).

## <a name="use-cases"></a>Варианты использования

Убедитесь, что ListView является элементом управления справа для ваших потребностей. ListView может использоваться в любой ситуации, где при отображении прокручиваемые списков данных. ListViews поддерживают контекстные действия и привязки данных.

ListView не следует путать с [TableView](~/xamarin-forms/user-interface/tableview.md). Элемент управления TableView является лучшим вариантом, каждый раз, когда у вас есть список несвязанные параметры или данные. Например параметры приложения iOS, которые главным образом предустановленный набор параметров, лучше подходит для использования TableView чем ListView.

Также Обратите внимание, что ListView лучше всего подходит для однородные данные &ndash; то есть все данные должны быть одного типа. Это так, как можно использовать только один тип ячейки для каждой строки в списке. TableViews может поддерживать несколько типов ячеек, так, чтобы они лучше, когда требуется сочетать представления.

## <a name="components"></a>Компоненты
ListView имеет ряд компонентов, доступных для использования собственные функции для каждой платформы. Ниже описан каждый из этих компонентов:

- **[Верхние и нижние колонтитулы](customizing-list-appearance.md#Headers_and_Footers)**  &ndash; текста или представления для отображения в начале и конце списка, отделить от данных в списке. Верхние и нижние колонтитулы можно привязать к источнику данных независимо от источника данных ListView.
- **[Группы](customizing-list-appearance.md#Grouping)**  &ndash; данные в ListView можно группировать для упрощения навигации. Группы обычно являются с привязкой к данным:

![](images/grouping-depth.png "ListView с сгруппированных данных")

- **[Ячейки](customizing-cell-appearance.md)**  &ndash; данные в ListView представлены в ячейках. Каждая ячейка соответствует строке данных. Встроенные ячейки для выбора или можно определить собственные настраиваемые ячейки. Встроенные и настраиваемые ячейки могут быть использовать/определенного в XAML или кода.
  - **[Встроенные](customizing-cell-appearance.md#Built_in_Cells)**  &ndash; построен в ячейках, особенно TextCell и ImageCell, может быть отлично подходят для производительности, так как они соответствуют собственные элементы управления на каждой платформе.
    - **[TextCell](customizing-cell-appearance.md#TextCell)**  &ndash; отображает строку текста, при необходимости с текстом детализации. Текст сведений отображается как вторая строка меньший размер шрифта и Цвет диакритических знаков.
    - **[ImageCell](customizing-cell-appearance.md#ImageCell)**  &ndash; отображает изображение с текстом. Отображается в виде TextCell с изображением в левой части.
  - **[Пользовательские ячейки](customizing-cell-appearance.md#customcells)**  &ndash; ячеек Custom удобны, когда требуется для представления сложных данных. Например пользовательское представление может использоваться для представления списка песен, включая альбомах и исполнителях:

![](images/image-cell-default.png "ListView с ImageCells")

Дополнительные сведения о настройке ячеек в ListView, см. в разделе [Настройка внешний вид ячейки ListView](customizing-cell-appearance.md).

## <a name="functionality"></a>Функция
ListView поддерживает ряд стилей взаимодействия, включая:

- **[Чтобы обновить по запросу](interactivity.md#Pull_to_Refresh)**  &ndash; ListView поддерживает по запросу для обновления на каждой платформе.
- **[Контекстные действия](interactivity.md#Context_Actions)**  &ndash; ListView поддерживает принятия мер для отдельных элементов в списке. Например можно реализовать действие прокрутки на iOS, или удерживайте действий на устройстве Android.
- **[Выбор](interactivity.md#selectiontaps)**  &ndash; может ожидать передачи данных для выбора и deselections выполнить действие, при выборе элемента строки.

![](images/context-default.png "ListView с помощью контекстных действий")

Дополнительные сведения о функциях интерактивность, ListView, см. в разделе [действия и взаимодействия с ListView](interactivity.md).

## <a name="related-links"></a>Связанные ссылки

- [Работа с ListView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistview)
- [Двусторонней привязки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-switchentrytwobinding)
- [Встроенные в ячейках (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-builtincells)
- [Пользовательские ячейки (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-customcells)
- [Группирование (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-grouping)
- [Пользовательское представление модуля подготовки отчетов (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/workingwithlistviewnative/)
- [ListView интерактивности (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-listview-interactivity)
