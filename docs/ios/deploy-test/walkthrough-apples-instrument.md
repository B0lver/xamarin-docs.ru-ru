---
title: Пошаговое руководство. Использование средства Apple Instruments
description: В этой статье описывается, как использовать средство Apple Instruments для диагностики проблем с памятью в приложении iOS, созданном с помощью Xamarin. Она описывает запуск Instruments, создание моментальных снимков кучи, анализ роста памяти и многое другое.
ms.prod: xamarin
ms.assetid: 8f21db1d-7107-4158-8058-d47e417689a0
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/19/2017
ms.openlocfilehash: 41254fb6aac176cd796fba851478b31f774553d2
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73023446"
---
# <a name="walkthrough---using-apples-instruments-tool"></a>Пошаговое руководство. Использование средства Apple Instruments

_В этой статье последовательно описываются этапы использования средства Apple Instruments для диагностики проблем с памятью в приложении iOS, созданном с помощью Xamarin. В ней описывается запуск Instruments, создание мгновенного снимка кучи и анализ роста памяти. В статье также показано, как использовать Instruments для отображения и определения именно тех строк кода, которые приводят к возникновению проблем с памятью._

На этой странице рассказывается, как использовать **средство Xcode Instruments** для диагностики проблем с памятью в приложении iOS.
Для начала загрузите [Пример MemoryDemo](https://docs.microsoft.com/samples/xamarin/ios-samples/profiling-memorydemo) и откройте **предыдущую версию** решения в Visual Studio для Mac.

## <a name="diagnosing-the-memory-issues"></a>Диагностика проблем с памятью

1. В среде Visual Studio для Mac запустите средство **Instruments**, открыв меню **"Сервис" > "Запустить Instruments"** .
2. Отправьте приложение на устройство, выбрав в меню **"Запуск" > "Отправить на устройство"** .
3. Выберите шаблон **Allocations** (Распределения). Это оранжевый значок с белым полем

    ![](walkthrough-apples-instrument-images/00-allocations-tempate.png "Choose the Allocations template")

4. Выберите приложение **Memory Demo** в списке **Choose a profiling template for** (Выберите шаблон профилирования для) в верхней части окна. Сначала щелкните устройство iOS, чтобы развернуть меню со списком установленных приложений.

    ![](walkthrough-apples-instrument-images/01-mem-demo.png "Select the Memory Demo application")

5. Нажмите кнопку **Choose** (Выбрать) в нижнем правом углу окна, чтобы запустить **Instruments**. В верхней панели шаблона отобразятся два элемента: Allocations (Выделения) и VM Tracker (Отслеживание виртуальных машин).

6. Нажмите кнопку **Record** (Запись), т. е. красный кружок в верхнем левом углу экрана Instruments, чтобы запустить приложение.

7. Выберите в верхней панели строку **VM Tracker**. Теперь, когда приложение запущено, в нем будет два раздела: Dirty (Изменение) и Resident Size (Размер в памяти). В области **Inspector** (Инспектор) выберите **Show Display Settings** (Показать параметры отображения), т. е. значок шестеренки, а затем установите флажок **Automatic Snapshotting** (Автоматическое создание снимков), показанный на следующем снимке экрана в правом нижнем углу:

    ![](walkthrough-apples-instrument-images/02-auto-snapshot.png "Choose the Show Display Settings option the gear icon then tick the Automatic Snapshotting checkbox")

8. Выберите строку **Allocations** в верхней области. Теперь, когда приложение запущено, в ней будет написано *All Heap and Anonymous VM* (Вся куча и анонимные ВМ)
9. В области **Inspector** выберите параметр **Show Display Settings** (значок шестеренки), а затем нажмите кнопку **Mark Generation** (Отметить поколение), чтобы задать набор базовых показателей. На временной шкале в верхней части окна появится небольшой красный флажок
10. Прокрутите приложение, а затем нажмите **Mark Generation**. Повторите это действие несколько раз
11. Нажмите кнопку **Stop** (Остановить).
12. Разверните узел **Generation** (Поколение) с наибольшим значением **Growth** (Рост) и задайте сортировку по показателю **Growth** по убыванию.
13. В области **Inspector** выберите **Show Extended Detail** (Показать расширенные сведения), т. е. элемент "E", чтобы отобразить **трассировку стека**.

14. Обратите внимание, что узел **&lt;non-object>** (без объектов) демонстрирует чрезмерное потребление памяти. Щелкните стрелку рядом с этим узлом, чтобы увидеть дополнительные сведения. Щелкните правой кнопкой мыши по трассировке стека, чтобы добавить в область **Source Location** (Исходное расположение):

    ![](walkthrough-apples-instrument-images/03-mem-growth.png "Add Source Location to the pane")

15. Выполните сортировку по показателю **Size** (Размер) и откройте представление **Expanded Detail** (Расширенные сведения):

    ![](walkthrough-apples-instrument-images/04-extended-detail.png "Sort by Size and display the  Extended Detail view")

16. Щелкните нужную запись в стеке вызовов, чтобы просмотреть соответствующий код:

    ![](walkthrough-apples-instrument-images/05-related-code.png "Viewing the related code")

В нашем случае для каждой ячейки создается новый образ, который помещается в коллекцию. Существующие ячейки коллекции повторно не используются.

## <a name="resolving-the-memory-issues"></a>Устранение проблем с памятью

Средство Instruments позволяет устранить эти проблемы и перезапустить приложение.

Объявив только один экземпляр на уровне класса, вы сможете повторно использовать образ и объект ячейки из существующего пула, а не создавать их каждый раз заново:

```csharp
public override UICollectionViewCell GetCell (UICollectionView collectionView, NSIndexPath indexPath)
{
    // Dequeue a cell from the reuse pool
    var imageCell = (ImageCell)collectionView.DequeueReusableCell (cellId, indexPath);

    // Reuse the image declared at the class level
    imageCell.ImageView.Image = image;

    return imageCell;
}
```

Теперь при запуске приложения использование памяти значительно сокращается — значение показателя **Growth** между поколениями теперь измеряется в KiB (килобайтах), а не в MiB (мегабайтах), как было до исправления кода:

![](walkthrough-apples-instrument-images/06-reduced-memory.png "Showing the app memory usage")

Улучшенный код представлен в [примере MemoryDemo](https://docs.microsoft.com/samples/xamarin/ios-samples/profiling-memorydemo) из **измененной версии** решения в Visual Studio для Mac.

Эта статья блога сообщества о [сборке мусора в Xamarin.iOS](https://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/) — полезный справочный ресурс по устранению проблем с памятью с помощью Xamarin.iOS.

## <a name="summary"></a>Сводка

Эта статья показывает, как использовать Instruments для диагностики проблем с памятью.
Она описывает, как запускать Instruments в среде Visual Studio для Mac, загружать шаблон распределения памяти и пошагово использовать моментальные снимки для выявления проблем с памятью.
Наконец, статья рассматривает повторную проверку приложения для подтверждения, что проблемы устранены.

## <a name="related-links"></a>Связанные ссылки

- [Пример MemoryDemo](https://docs.microsoft.com/samples/xamarin/ios-samples/profiling-memorydemo)
- [Сборка мусора Xamarin.iOS (запись блога)](https://c-sharx.net/2015-04-27-xamarin-ios-the-garbage-collector-and-me/)
