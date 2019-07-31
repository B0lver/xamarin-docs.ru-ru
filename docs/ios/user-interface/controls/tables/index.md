---
title: Работа с таблицами и ячейками в Xamarin. iOS
description: В этом документе содержатся ссылки на различные руководства, описывающие отображение данных с помощью элемента управления Уитаблевиев в приложении Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 04DF47DD-4E17-75D7-AC7C-8CF4A574CD21
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 01/06/2016
ms.openlocfilehash: d01814bc241dcfb7b62f40bef226ee720a96ff23
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68657009"
---
# <a name="working-with-tables-and-cells-in-xamarinios"></a>Работа с таблицами и ячейками в Xamarin. iOS

В этом разделе описываются классы, используемые для создания и отображения таблиц, а также приводятся примеры их использования в Xamarin. iOS. Он охватывает использование внешнего вида по умолчанию для таблиц, настройку макета, реализацию редактирования и использование конструктора Xamarin iOS для визуального проектирования таблицы. Иногда отображение представляет собой список строк (например, музыкальное приложение) и в других случаях трудно распознать элемент управления таблицы (например, редактирование в приложении "Контакты" или диалог в приложении "сообщения").

Для тех, кто работает с приложениями на разных платформах с помощью Xamarin. Android, элемент управления Уитаблевиев аналогичен классу ListView в Android (и класс Уитаблевиевсаурце похож на классы адаптеров Android).

В этих статьях подробно рассматривается работа с таблицами, в том числе:

-   **Части таблицы** — знакомство с визуальными элементами `UITableView` элемента управления и их объяснение. 
-   **Отображение данных в таблицах** — демонстрация создания и заполнения таблицы, использование различных стилей таблиц и ячеек и избежание проблем с памятью путем повторного запуска объектов ячеек. 
-   **Расширенное использование** — создание пользовательских ячеек и использование функций редактирования класса уитаблевиев. 
-   **Визуальное создание таблицы** с помощью Xamarin Designer для iOS для создания интерфейса с раскадровкой, основанного на таблицах. 

## <a name="contents"></a>Описание

 [Функциональные возможности &amp; частей таблиц](~/ios/user-interface/controls/tables/table-parts-and-functionality.md)

 [Заполнение таблицы данными](~/ios/user-interface/controls/tables/populating-a-table-with-data.md)

 [Настройка вида таблицы](~/ios/user-interface/controls/tables/customizing-table-appearance.md)

 [Редактирование](~/ios/user-interface/controls/tables/editing.md)
 
 [Действия со строками](~/ios/user-interface/controls/tables/row-action.md)

 [Создание таблиц в раскадровке](~/ios/user-interface/controls/tables/creating-tables-in-a-storyboard.md)
 
 [Автоматическое изменение высоты строки](~/ios/user-interface/controls/tables/autosizing-row-height.md)

## <a name="related-links"></a>Связанные ссылки

- [Воркингвистаблес (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/workingwithtables)
- [Таблицы в раскадровках (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/storyboardtable)
- [Введение в раскадровку](~/ios/user-interface/storyboards/index.md)
- [Раскадровка для рецепта Таблевиев](https://github.com/xamarin/recipes/tree/master/Recipes/ios/general/storyboard/storyboard_a_tableview)
- [Введение в бескасание. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пример Таблидитинг на GitHub](https://github.com/xamarin/monotouch-samples/tree/master/TableEditing)
- [Пример Таблепартс на GitHub](https://github.com/xamarin/monotouch-samples/tree/master/TableParts)
- [Пример Таблеандцеллстилес на GitHub](https://github.com/xamarin/mobile-samples/tree/master/TablesLists)
- [Справочник по классам Уитаблевиев](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableView_Class/)
- [Справочник по классам Уитаблевиевцелл](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewCell_Class/)
- [UITableViewDelegate](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDelegate_Protocol/)
- [уитаблевиевдатасаурце](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UITableViewDataSource_Protocol/)
