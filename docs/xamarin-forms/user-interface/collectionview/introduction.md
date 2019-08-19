---
title: Введение в Xamarin. Forms CollectionView
description: CollectionView — это гибкое и производительное представление для представления списков данных с использованием различных спецификаций макета.
ms.prod: xamarin
ms.assetid: 5C08F687-B9E6-4CE4-8726-F287F6D0B6A7
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 25e2d9bad11614cf594980480db14ddc18125a96
ms.sourcegitcommit: c6e56545eafd8ff9e540d56aba32aa6232c5315f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68738908"
---
# <a name="xamarinforms-collectionview-introduction"></a>Введение в Xamarin. Forms CollectionView

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView)представляет собой представление для представления списков данных с использованием различных спецификаций макета. Она нацелена на предоставление более гибкой и производительной альтернативы [`ListView`](xref:Xamarin.Forms.ListView). Например, на следующих снимках экрана показан `CollectionView` объект, использующий вертикальную сетку двух столбцов и допускающий множественный выбор:

[ ![Снимок экрана: вертикальный макет сетки CollectionView, в iOS и Android](introduction-images/verticalgrid-multipleselection.png "CollectionView вертикальная сетка с множественным выделением") ] (introduction-images/verticalgrid-multipleselection-large.png#lightbox "Вертикальный макет сетки CollectionView с множественным выделением")

[`CollectionView`](xref:Xamarin.Forms.CollectionView)доступна в Xamarin. Forms 4,0. Однако в настоящее время он эксперименталь и может использоваться только путем добавления следующей строки кода в `AppDelegate` класс в iOS или `MainActivity` в класс в Android перед вызовом `Forms.Init`:

```csharp
Forms.SetFlags("CollectionView_Experimental");
```

> [!IMPORTANT]
> [`CollectionView`](xref:Xamarin.Forms.CollectionView)доступен в iOS и Android, но частично доступен на универсальная платформа Windows.

## <a name="collectionview-and-listview-differences"></a>Различия в CollectionView и ListView

Хотя API- [`ListView`](xref:Xamarin.Forms.ListView) интерфейсы [ипохожи,существуютнекоторыеважныеотличия.`CollectionView`](xref:Xamarin.Forms.CollectionView)

- [`CollectionView`](xref:Xamarin.Forms.CollectionView)имеет гибкую модель макета, которая позволяет представлять данные по вертикали или по горизонтали, в списке или в сетке.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает выбор одного и нескольких элементов.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)не имеет концепции ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)автоматически использует виртуализацию, предоставляемую базовыми элементами управления.
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)сокращает поверхность [`ListView`](xref:Xamarin.Forms.ListView)API. Многие свойства и события из [`ListView`](xref:Xamarin.Forms.ListView) не представлены в. `CollectionView`
- [`CollectionView`](xref:Xamarin.Forms.CollectionView)не включает встроенные разделители.

## <a name="move-from-listview-to-collectionview"></a>Переход из ListView в CollectionView

[`ListView`](xref:Xamarin.Forms.ListView)реализации в существующих реализациях Xamarin. Forms можно перенести в [`CollectionView`](xref:Xamarin.Forms.CollectionView) реализации с помощью следующей таблицы:

| Понятие | API ListView | CollectionView |
|---|---|---|
| Данные | `ItemsSource` | Заполняется данными путем установки его `ItemsSource` свойства. [`CollectionView`](xref:Xamarin.Forms.CollectionView) Дополнительные сведения см. [в разделе Заполнение CollectionView данными](populate-data.md#populate-a-collectionview-with-data). |
| Внешний вид элемента | `ItemTemplate` | Внешний вид каждого элемента в [`CollectionView`](xref:Xamarin.Forms.CollectionView) может быть определен путем `ItemTemplate` присвоения свойству [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)значения. Дополнительные сведения см. в разделе [Определение внешнего вида элемента](populate-data.md#define-item-appearance). |
| Ячейки | `TextCell`, `ImageCell`, `ViewCell` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)не имеет концепции ячеек. Вместо этого шаблон данных используется для определения внешнего вида каждого элемента данных в списке. |
| Разделители строк | `SeparatorColor`, `SeparatorVisibility` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)не включает встроенные разделители. При необходимости они могут быть предоставлены в шаблоне элемента. |
| Выбранное | `SelectionMode`, `SelectedItem` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)поддерживает выбор одного и нескольких элементов. Дополнительные сведения см. в разделе [Xamarin. Forms CollectionView Selection](selection.md). |
| Высота строки | `HasUnevenRows`, `RowHeight` | В высота строки каждого элемента определяется `ItemSizingStrategy` свойством. `CollectionView` Дополнительные сведения см. в разделе [изменение размера элемента](layout.md#item-sizing).|
| Кэширование | `CachingStrategy` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)автоматически использует виртуализацию, предоставляемую базовыми элементами управления. |
| Верхние и нижние колонтитулы | `Header`, `HeaderElement`, `HeaderTemplate`, `Footer`, `FooterElement`, `FooterTemplate` | Верхние и нижние колонтитулы в настоящее время `CollectionView`не поддерживаются в, но будут добавлены в следующем выпуске.|
| Группирование | `GroupDisplayBinding`, `GroupHeaderTemplate`, `GroupShortNameBinding`, `IsGroupingEnabled` | Группирование в настоящее время не поддерживается `CollectionView`в, но будет добавлено в будущем выпуске. |
| Извлечь для обновления | `IsPullToRefreshEnabled`, `IsRefreshing`, `RefreshAllowed`, `RefreshCommand`, `RefreshControlColor`, `BeginRefresh()`, `EndRefresh()` | Извлечение для обновления в настоящее время не поддерживается в `CollectionView`, но будет добавлено в будущем выпуске. |
| Контекстные действия | `ContextActions` | Контекстные действия в настоящее время не поддерживаются в `CollectionView`, но будут добавлены в будущем выпуске. |
| Прокрутка | `ScrollTo()` | [`CollectionView`](xref:Xamarin.Forms.CollectionView)Определяет `ScrollTo` методы, которые просматривают элементы в представлении. Дополнительные сведения см. в разделе [прокрутка](scrolling.md). |

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
