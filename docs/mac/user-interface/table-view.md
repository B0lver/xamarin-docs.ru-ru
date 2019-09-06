---
title: Табличные представления в Xamarin. Mac
description: В этой статье рассматривается работа с табличными представлениями в приложении Xamarin. Mac. Он описывает создание табличных представлений в Xcode и Interface Builder и взаимодействие с ними в коде.
ms.prod: xamarin
ms.assetid: 3B55B858-4769-4331-966A-7F53B3B7C720
ms.technology: xamarin-mac
author: conceptdev
ms.author: crdun
ms.date: 03/14/2017
ms.openlocfilehash: 758134b0c5171e46c47ff6fd8071b13a44d5789b
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70291610"
---
# <a name="table-views-in-xamarinmac"></a>Табличные представления в Xamarin. Mac

_В этой статье рассматривается работа с табличными представлениями в приложении Xamarin. Mac. Он описывает создание табличных представлений в Xcode и Interface Builder и взаимодействие с ними в коде._

При работе с C# и .NET в приложении Xamarin. Mac у вас есть доступ к тем же представлениям таблиц, что и разработчик, работающий на *уровне цели-C* и *Xcode* . Так как Xamarin. Mac интегрируется напрямую с Xcode, можно использовать _Interface Builder_ Xcode для создания и обслуживания табличных представлений (или при необходимости создавать их непосредственно в C# коде).

Табличное представление отображает данные в табличном формате, содержащем один или несколько столбцов данных в нескольких строках. В зависимости от типа создаваемого представления таблицы пользователь может сортировать по столбцам, реорганизовывать столбцы, добавлять столбцы, удалять столбцы или изменять данные, содержащиеся в таблице.

[![](table-view-images/intro01.png "Пример таблицы")](table-view-images/intro01.png#lightbox)

В этой статье рассматриваются основные принципы работы с табличными представлениями в приложении Xamarin. Mac. Мы настоятельно рекомендуем сначала ознакомиться со статьей [Hello, Mac](~/mac/get-started/hello-mac.md) , в частности [Знакомство с Xcode и Interface Builder](~/mac/get-started/hello-mac.md#introduction-to-xcode-and-interface-builder) , а также с разделом "возможности [и действия](~/mac/get-started/hello-mac.md#outlets-and-actions) ", так как в нем рассматриваются основные понятия и методы, которые мы будем использовать в Эта статья.

Возможно, вы захотите ознакомиться с [ C# представлением классов и методов для цели-C](~/mac/internals/how-it-works.md) в документе о внутренних компонентах [Xamarin. Mac](~/mac/internals/how-it-works.md) , а `Register` также объясняются команды и `Export` , используемые для подключения C# классов к Объекты цели-C и элементы пользовательского интерфейса.

<a name="Introduction_to_Table_Views" />

## <a name="introduction-to-table-views"></a>Общие сведения о табличных представлениях

Табличное представление отображает данные в табличном формате, содержащем один или несколько столбцов данных в нескольких строках. Табличные представления отображаются в представлениях прокрутки (`NSScrollView`) и начиная с macOS 10,7. для отображения строк и столбцов можно использовать любую `NSView` ячейку вместо ячеек (`NSCell`). Тем не менее, вы по- `NSCell` прежнему можете использовать, но обычно `NSTableCellView` подклассировать и создавать пользовательские строки и столбцы.

Табличное представление не хранит собственные данные, вместо этого оно использует источник данных (`NSTableViewDataSource`) для предоставления требуемых строк и столбцов по мере необходимости.

Поведение табличного представления можно настроить, предоставив подкласс делегата представления таблицы (`NSTableViewDelegate`) для поддержки управления столбцами таблицы, тип для выбора функциональности, выбора и редактирования строк, настраиваемого отслеживания и пользовательских представлений для отдельных столбцов. сквоз.

При создании табличных представлений Apple предлагает следующее:

- Разрешает пользователю сортировать таблицу, щелкая заголовки столбцов.
- Создайте заголовки столбцов, которые являются существительными или короткими фразами существительное, описывающими данные, отображаемые в этом столбце.

Дополнительные сведения см. в разделе " [представления содержимого](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/ControlsView.html#//apple_ref/doc/uid/20000957-CH52-SW1) " [руководства по использованию интерфейса пользователя ОС Apple X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/).

<a name="Creating-and-Maintaining-Table-Views-in-Xcode" />

## <a name="creating-and-maintaining-table-views-in-xcode"></a>Создание и обслуживание табличных представлений в Xcode

При создании нового приложения Xamarin. Mac Cocoa по умолчанию получается стандартное пустое окно. Эти окна определяются в `.storyboard` файле, который автоматически включается в проект. Чтобы изменить структуру Windows, в **Обозреватель решений**дважды щелкните `Main.storyboard` файл:

[![](table-view-images/edit01.png "Выбор основной раскадровки")](table-view-images/edit01.png#lightbox)

Это приведет к открытию структуры окна в Interface Builder Xcode:

[![](table-view-images/edit02.png "Изменение пользовательского интерфейса в Xcode")](table-view-images/edit02.png#lightbox)

Введите `table` в поле поиска **инспектора библиотек** , чтобы упростить поиск элементов управления представления таблицы.

[![](table-view-images/edit03.png "Выбор табличного представления из библиотеки")](table-view-images/edit03.png#lightbox)

Перетащите табличное представление на контроллер представления в **редакторе интерфейса**, сделайте его частью области содержимого контроллера представления и установите его в то место, где оно сжимается и растет с окном в **редакторе ограничений**:

[![](table-view-images/edit04.png "Изменение ограничений")](table-view-images/edit04.png#lightbox)

Выберите табличное представление в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit05.png "Инспектор атрибутов")](table-view-images/edit05.png#lightbox)

- **Режим содержимого** . позволяет использовать представления (`NSView`) или ячейки (`NSCell`) для отображения данных в строках и столбцах. Начиная с macOS 10,7, следует использовать представления.
- **Строки группы с плавающей запятой** — если `true`в табличном представлении будут отображаться сгруппированные ячейки, как если бы они были плавающими.
- **Columns (столбцы** ) — определяет количество отображаемых столбцов.
- **Headers** — `true`если столбцы будут иметь заголовки.
- **Изменение порядка** — если `true`пользователь сможет переупорядочить столбцы в таблице.
- **Изменение размера** — если `true`пользователь будет иметь возможность перетаскивать заголовки столбцов для изменения размера столбцов.
- **Изменение размера столбца** — управляет тем, как таблица будет иметь автоматический размер столбцов.
- **Выделение** — управляет типом выделения, используемым таблицей при выборе ячейки.
- **Альтернативные строки** — если `true`, когда-нибудь другая строка, будет иметь другой цвет фона.
- **Горизонтальная сетка** — выбор типа границы, нарисованной между ячейками по горизонтали.
- **Вертикальная сетка** — выбор типа границы, нарисованной между ячейками по вертикали.
- **Цвет сетки** — задает цвет границы ячейки.
- **Фон** — задает цвет фона ячейки.
- **Выбор** — позволяет контролировать, как пользователь может выбирать ячейки в таблице следующим образом:
  - **Несколько** — если `true`пользователь может выбрать несколько строк и столбцов.
  - **Столбец** — если `true`пользователь может выбрать столбцы.
  - **Введите SELECT** — если `true`пользователь может ввести символ для выбора строки.
  - **Пусто** — `true`если пользователю не нужно выбирать строку или столбец, в таблице не допускается выбор.
- **Автосохранение** — имя, в котором автоматически сохраняется формат таблиц.
- **Сведения о столбце** — `true`если значение равно, порядок и ширина столбцов будут сохраняться автоматически.
- **Разрывы строк** — выберите, как ячейка обрабатывает разрывы строк.
- **Усекает последнюю видимую строку** — если `true`ячейка будет обрезана, данные не будут помещаться в границы.

> [!IMPORTANT]
> Если вы не обслуживаете устаревшее приложение Xamarin. Mac `NSView` , табличные представления следует `NSCell` использовать при просмотре таблиц на основе. `NSCell`считается устаревшим и может не поддерживаться в дальнейшем.

Выберите столбец таблицы в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit06.png "Инспектор атрибутов")](table-view-images/edit06.png#lightbox)

- **Заголовок** — задает заголовок столбца.
- **Выравнивание** — задает выравнивание текста внутри ячеек.
- **Шрифт заголовка** — выбирает шрифт для текста заголовка ячейки.
- **Ключ сортировки** — ключ, используемый для сортировки данных в столбце. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Selector** — **действие** , используемое для выполнения сортировки. Оставьте пустым, если пользователь не может сортировать этот столбец.
- **Order** — порядок сортировки данных столбцов.
- **Изменение размера** — выбор типа изменения размера столбца.
- **Редактируемый** — `true`если пользователь может изменять ячейки в таблице на основе ячейки.
- **Hidden** — если `true`столбец скрыт.

Можно также изменить размер столбца, перетащив его маркер (вертикально на правой стороне столбца) влево или вправо.

Давайте выберем каждый столбец в представлении таблицы и присвоить первому столбцу **заголовок** `Product` `Details`, а второй —.

Выберите табличное представление (`NSTableViewCell`) в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit07.png "Инспектор атрибутов")](table-view-images/edit07.png#lightbox)

Это все свойства стандартного представления. Кроме того, здесь можно изменить размер строк для этого столбца.

Выберите ячейку представления таблицы (по умолчанию это `NSTextField`) в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit08.png "Инспектор атрибутов")](table-view-images/edit08.png#lightbox)

У вас будут все свойства стандартного текстового поля, которые будут заданы здесь. По умолчанию для вывода данных для ячейки в столбце используется стандартное текстовое поле.

Выберите табличное представление (`NSTableFieldCell`) в **иерархии интерфейсов** , и в **инспекторе атрибутов**доступны следующие свойства:

[![](table-view-images/edit09.png "Инспектор атрибутов")](table-view-images/edit09.png#lightbox)

Ниже приведены наиболее важные параметры.

- **Макет** — выберите порядок расположения ячеек в этом столбце.
- **Использует однострочный режим** — `true`если ячейка ограничена одной строкой.
- **Ширина первой структуры среды выполнения** . `true`если значение равно, то для ячейки будет предпочтительнее задать ширину (вручную или автоматически), если она отображается при первом запуске приложения.
- **Действие** — управляет тем, когда **действие** редактирования отправляется для ячейки.
- **Поведение** — определяет, является ли ячейка выбираемой или изменяемой.
- **Форматированный текст** — `true`если в ячейке может отображаться форматированный текст и стиль текста.
- **Отменить** — если `true`значение равно, ячейка принимает ответственность за выполнение отмены.

Выберите табличное представление ячейки (`NSTableFieldCell`) в нижней части столбца таблицы в **иерархии интерфейсов**:

[![](table-view-images/edit10.png "Выбор представления ячеек таблицы")](table-view-images/edit10.png#lightbox)

Это позволяет изменить представление ячейки таблицы, используемое в качестве базового _шаблона_ для всех ячеек, созданных для данного столбца.

<a name="Adding_Actions_and_Outlets" />

### <a name="adding-actions-and-outlets"></a>Добавление действий и розеток

Как и любой другой элемент управления пользовательского интерфейса Cocoa, нам нужно предоставить представление таблицы, а столбцы и ячейки — C# коду с помощью **действий** и **розеток** (в зависимости от требуемых функций).

Этот процесс одинаков для любого элемента представления таблицы, который мы хотим предоставить:

1. Перейдите в **Редактор помощника** и убедитесь, что `ViewController.h` файл выбран: 

    [![](table-view-images/edit11.png "Редактор помощника")](table-view-images/edit11.png#lightbox)
2. Выберите табличное представление в **иерархии интерфейс**, щелкните элемент управления и перетащите его в `ViewController.h` файл.
3. Создайте **выход** для табличного представления с именем `ProductTable`: 

    [![](table-view-images/edit13.png "Настройка розетки")](table-view-images/edit13.png#lightbox)
4. Создание **розеток** для столбцов таблиц также вызывается `ProductColumn` и: `DetailsColumn` 

    [![](table-view-images/edit14.png "Настройка розетки")](table-view-images/edit14.png#lightbox)
5. Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Далее мы напишем код, отображающий некоторые данные для таблицы при запуске приложения.

<a name="Populating_the_Table_View" />

## <a name="populating-the-table-view"></a>Заполнение табличного представления

С представлением таблицы, разработанным в Interface Builder и предоставляемых через **розетку**, далее необходимо создать C# код для его заполнения.

Сначала создадим новый `Product` класс для хранения информации по отдельным строкам. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **Общий** > **пустой класс**, введите `Product` для **имени** и нажмите кнопку **создать** :

[![](table-view-images/populate01.png "Создание пустого класса")](table-view-images/populate01.png#lightbox)

Сделайте так, чтобы файлвыгляделследующимобразом:`Product.cs`

```csharp
using System;

namespace MacTables
{
  public class Product
  {
    #region Computed Properties
    public string Title { get; set;} = "";
    public string Description { get; set;} = "";
    #endregion

    #region Constructors
    public Product ()
    {
    }

    public Product (string title, string description)
    {
      this.Title = title;
      this.Description = description;
    }
    #endregion
  }
}

```

Далее необходимо создать подкласс `NSTableDataSource` класса для предоставления данных для таблицы по мере их запроса. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **Общий** > **пустой класс**, введите `ProductTableDataSource` в качестве **имени** и нажмите кнопку **создать** .

`ProductTableDataSource.cs` Измените файл и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
  public class ProductTableDataSource : NSTableViewDataSource
  {
    #region Public Variables
    public List<Product> Products = new List<Product>();
    #endregion

    #region Constructors
    public ProductTableDataSource ()
    {
    }
    #endregion

    #region Override Methods
    public override nint GetRowCount (NSTableView tableView)
    {
      return Products.Count;
    }
    #endregion
  }
}

```

Этот класс имеет хранилище для элементов табличного представления и переопределяет метод `GetRowCount` , возвращающий количество строк в таблице.

Наконец, необходимо создать подкласс `NSTableDelegate` класса для обеспечения поведения для нашей таблицы. В **Обозреватель решений**щелкните правой кнопкой мыши проект и выберите **Добавить** > **новый файл...** Выберите **Общий** > **пустой класс**, введите `ProductTableDelegate` в качестве **имени** и нажмите кнопку **создать** .

`ProductTableDelegate.cs` Измените файл и сделайте его следующим:

```csharp
using System;
using AppKit;
using CoreGraphics;
using Foundation;
using System.Collections;
using System.Collections.Generic;

namespace MacTables
{
  public class ProductTableDelegate: NSTableViewDelegate
  {
    #region Constants 
    private const string CellIdentifier = "ProdCell";
    #endregion

    #region Private Variables
    private ProductTableDataSource DataSource;
    #endregion

    #region Constructors
    public ProductTableDelegate (ProductTableDataSource datasource)
    {
      this.DataSource = datasource;
    }
    #endregion

    #region Override Methods
    public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
    {
      // This pattern allows you reuse existing views when they are no-longer in use.
      // If the returned view is null, you instance up a new view
      // If a non-null view is returned, you modify it enough to reflect the new data
      NSTextField view = (NSTextField)tableView.MakeView (CellIdentifier, this);
      if (view == null) {
        view = new NSTextField ();
        view.Identifier = CellIdentifier;
        view.BackgroundColor = NSColor.Clear;
        view.Bordered = false;
        view.Selectable = false;
        view.Editable = false;
      }

      // Setup view based on the column selected
      switch (tableColumn.Title) {
      case "Product":
        view.StringValue = DataSource.Products [(int)row].Title;
        break;
      case "Details":
        view.StringValue = DataSource.Products [(int)row].Description;
        break;
      }

      return view;
    }
    #endregion
  }
}
```

При создании экземпляра класса `ProductTableDelegate`мы также передаем экземпляр `ProductTableDataSource` объекта, который предоставляет данные для таблицы. `GetViewForItem` Метод отвечает за возврат представления (данных) для отображения ячейки для столбца и строки предоставления. Если это возможно, для отображения ячейки будет использоваться существующее представление, если не нужно создавать новое представление.

Чтобы заполнить таблицу, измените `ViewController.cs` файл и `AwakeFromNib` сделайте метод следующим:

```csharp
public override void AwakeFromNib ()
{
  base.AwakeFromNib ();

  // Create the Product Table Data Source and populate it
  var DataSource = new ProductTableDataSource ();
  DataSource.Products.Add (new Product ("Xamarin.iOS", "Allows you to develop native iOS Applications in C#"));
  DataSource.Products.Add (new Product ("Xamarin.Android", "Allows you to develop native Android Applications in C#"));
  DataSource.Products.Add (new Product ("Xamarin.Mac", "Allows you to develop Mac native Applications in C#"));

  // Populate the Product Table
  ProductTable.DataSource = DataSource;
  ProductTable.Delegate = new ProductTableDelegate (DataSource);
}
```

При запуске приложения отображается следующее:

[![](table-view-images/populate02.png "Запуск примера приложения")](table-view-images/populate02.png#lightbox)

<a name="Sorting_by_Column" />

## <a name="sorting-by-column"></a>Сортировка по столбцу

Давайте разрешите пользователю сортировать данные в таблице, щелкнув заголовок столбца. Сначала дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. `Title` `Ascending` Выберитестолбец,`compare:` введите для ключа сортировки для **селектора** и выберите для заказа: `Product`

[![](table-view-images/sort01.png "Задание ключа сортировки")](table-view-images/sort01.png#lightbox)

`Description` `Ascending` Выберитестолбец,`compare:` введите для ключа сортировки для **селектора** и выберите для заказа: `Details`

[![](table-view-images/sort02.png "Задание ключа сортировки")](table-view-images/sort02.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductTableDataSource.cs` файл и добавим следующие методы:

```csharp
public void Sort(string key, bool ascending) {

  // Take action based on key
  switch (key) {
  case "Title":
    if (ascending) {
      Products.Sort ((x, y) => x.Title.CompareTo (y.Title));
    } else {
      Products.Sort ((x, y) => -1 * x.Title.CompareTo (y.Title));
    }
    break;
  case "Description":
    if (ascending) {
      Products.Sort ((x, y) => x.Description.CompareTo (y.Description));
    } else {
      Products.Sort ((x, y) => -1 * x.Description.CompareTo (y.Description));
    }
    break;
  }

}

public override void SortDescriptorsChanged (NSTableView tableView, NSSortDescriptor[] oldDescriptors)
{
  // Sort the data
  if (oldDescriptors.Length > 0) {
    // Update sort
    Sort (oldDescriptors [0].Key, oldDescriptors [0].Ascending);
  } else {
    // Grab current descriptors and update sort
    NSSortDescriptor[] tbSort = tableView.SortDescriptors; 
    Sort (tbSort[0].Key, tbSort[0].Ascending); 
  }
      
  // Refresh table
  tableView.ReloadData ();
}
```

Метод позволяет нам сортировать данные в источнике данных на основе заданного `Product` поля класса в порядке возрастания или убывания. `Sort` Переопределенный `SortDescriptorsChanged` метод будет вызываться при каждом щелчке по заголовку столбца. Ему будет передано значение **ключа** , установленное в Interface Builder, и порядок сортировки для этого столбца.

Если запустить приложение и щелкнуть в заголовках столбцов, строки будут отсортированы по этому столбцу:

[![](table-view-images/sort03.png "Пример выполнения приложения")](table-view-images/sort03.png#lightbox)

<a name="Row_Selection" />

## <a name="row-selection"></a>Выбор строк

Если вы хотите разрешить пользователю выбирать одну строку, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и снимите флажок **несколько** в **инспекторе атрибутов**:

[![](table-view-images/select01.png "Инспектор атрибутов")](table-view-images/select01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.


Затем измените `ProductTableDelegate.cs` файл и добавьте следующий метод:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

Это позволит пользователю выбрать одну строку в представлении таблицы. Вернитесь `false`к строке `false` для любой строки, которую пользователь не должен иметь возможность выбирать или для каждой строки, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было строки. `ShouldSelectRow`

Табличное представление (`NSTableView`) содержит следующие методы для работы с выбором строк:

- `DeselectRow(nint)`— Отменяет выделение данной строки в таблице.
- `SelectRow(nint,bool)`— Выбирает заданную строку. Передайте `false` второй параметр, чтобы выбрать только одну строку за раз.
- `SelectedRow`— Возвращает текущую строку, выбранную в таблице.
- `IsRowSelected(nint)`— Возвращает `true` значение, если выбрана указанная строка.

<a name="Multiple_Row_Selection" />

## <a name="multiple-row-selection"></a>Выбор нескольких строк

Если вы хотите разрешить пользователю выбирать несколько строк, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и установите флажок **несколько** в **инспекторе атрибутов**:

[![](table-view-images/select02.png "Инспектор атрибутов")](table-view-images/select02.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.


Затем измените `ProductTableDelegate.cs` файл и добавьте следующий метод:

```csharp
public override bool ShouldSelectRow (NSTableView tableView, nint row)
{
  return true;
}
```

Это позволит пользователю выбрать одну строку в представлении таблицы. Вернитесь `false`к строке `false` для любой строки, которую пользователь не должен иметь возможность выбирать или для каждой строки, если вы не хотите, чтобы пользователь мог выбрать какие бы то ни было строки. `ShouldSelectRow`

Табличное представление (`NSTableView`) содержит следующие методы для работы с выбором строк:

- `DeselectAll(NSObject)`— Отменит выделение всех строк в таблице. Используйте `this` для первого параметра, чтобы отправить объект, выполняющий выборку. 
- `DeselectRow(nint)`— Отменяет выделение данной строки в таблице.
- `SelectAll(NSobject)`— Выбирает все строки в таблице. Используйте `this` для первого параметра, чтобы отправить объект, выполняющий выборку.
- `SelectRow(nint,bool)`— Выбирает заданную строку. Перед `false` вторым параметром отмените выбор и выберите только одну строку, передайте `true` , чтобы расширить выбор и включить эту строку.
- `SelectRows(NSIndexSet,bool)`— Выбирает заданный набор строк. Перед `false` вторым параметром отмените выбор и выберите только эти строки, передайте `true` , чтобы расширить выбор и включить эти строки.
- `SelectedRow`— Возвращает текущую строку, выбранную в таблице.
- `SelectedRows`— Возвращает значение `NSIndexSet` типа, содержащее индексы выбранных строк.
- `SelectedRowCount`— Возвращает число выбранных строк.
- `IsRowSelected(nint)`— Возвращает `true` значение, если выбрана указанная строка.

<a name="Type_to_Select_Row" />

## <a name="type-to-select-row"></a>Введите, чтобы выбрать строку

Если вы хотите разрешить пользователю вводить символ с выбранным табличным представлением и выбрать первую строку с этим символом, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и установите флажок **тип SELECT** в **инспекторе атрибутов**:

[![](table-view-images/type01.png "Установка типа выделения")](table-view-images/type01.png#lightbox)

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductTableDelegate.cs` файл и добавим следующий метод:

```csharp
public override nint GetNextTypeSelectMatch (NSTableView tableView, nint startRow, nint endRow, string searchString)
{
  nint row = 0;
  foreach(Product product in DataSource.Products) {
    if (product.Title.Contains(searchString)) return row;

    // Increment row counter
    ++row;
  }

  // If not found select the first row
  return 0;
}
```

Метод принимает заданный `searchString` объект и возвращает строку первой `Product` строки, в `Title`которой находится эта строка. `GetNextTypeSelectMatch`

Если запустить приложение и ввести символ, выбирается строка:

[![](table-view-images/type02.png "Запуск примера приложения")](table-view-images/type02.png#lightbox)

<a name="Reordering_Columns" />

## <a name="reordering-columns"></a>Изменение порядка столбцов

Если вы хотите разрешить пользователю перетаскивать столбцы переупорядочения в табличном представлении, дважды щелкните `Main.storyboard` файл, чтобы открыть его для редактирования в Interface Builder. Выберите табличное представление в **иерархии интерфейсов** и установите флажок **изменить порядок** в **инспекторе атрибутов**:

[![](table-view-images/reorder01.png "Инспектор атрибутов")](table-view-images/reorder01.png#lightbox)

Если присвоить значение свойству **автосохранения** и проверить поле **сведения о столбце** , все изменения, внесенные в макет таблицы, будут автоматически сохранены для нас и восстановлены при следующем запуске приложения.

Сохраните изменения и вернитесь в Visual Studio для Mac для синхронизации с Xcode.

Теперь изменим `ProductTableDelegate.cs` файл и добавим следующий метод:

```csharp
public override bool ShouldReorder (NSTableView tableView, nint columnIndex, nint newColumnIndex)
{
  return true;
}
```

Метод должен возвращать `true` значение для любого столбца, который требуется перетаскивать `newColumnIndex`в, иначе возвращает `false`. `ShouldReorder`

При запуске приложения можно перетаскивать заголовки столбцов для изменения порядка наших столбцов:

[![](table-view-images/reorder02.png "Пример переупорядоченных столбцов")](table-view-images/reorder02.png#lightbox)

<a name="Editing_Cells" />

## <a name="editing-cells"></a>Редактирование ячеек

Если вы хотите разрешить пользователю изменять значения для данной ячейки, измените `ProductTableDelegate.cs` файл и `GetViewForItem` измените метод следующим образом:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{
  // This pattern allows you reuse existing views when they are no-longer in use.
  // If the returned view is null, you instance up a new view
  // If a non-null view is returned, you modify it enough to reflect the new data
  NSTextField view = (NSTextField)tableView.MakeView (tableColumn.Title, this);
  if (view == null) {
    view = new NSTextField ();
    view.Identifier = tableColumn.Title;
    view.BackgroundColor = NSColor.Clear;
    view.Bordered = false;
    view.Selectable = false;
    view.Editable = true;

    view.EditingEnded += (sender, e) => {
          
      // Take action based on type
      switch(view.Identifier) {
      case "Product":
        DataSource.Products [(int)view.Tag].Title = view.StringValue;
        break;
      case "Details":
        DataSource.Products [(int)view.Tag].Description = view.StringValue;
        break; 
      }
    };
  }

  // Tag view
  view.Tag = row;

  // Setup view based on the column selected
  switch (tableColumn.Title) {
  case "Product":
    view.StringValue = DataSource.Products [(int)row].Title;
    break;
  case "Details":
    view.StringValue = DataSource.Products [(int)row].Description;
    break;
  }

  return view;
}
```

Теперь, если приложение запускается, пользователь может изменять ячейки в представлении таблицы:

[![](table-view-images/editing01.png "Пример редактирования ячейки")](table-view-images/editing01.png#lightbox)

<a name="Using_Images_in_Table_Views" />

## <a name="using-images-in-table-views"></a>Использование изображений в представлениях таблиц

Чтобы включить изображение как часть ячейки в `NSTableView`, необходимо изменить способ возврата данных `GetViewForItem` методом табличного `NSTableViewDelegate's` представления для использования `NSTableCellView` вместо типичного `NSTextField`. Например:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

  // This pattern allows you reuse existing views when they are no-longer in use.
  // If the returned view is null, you instance up a new view
  // If a non-null view is returned, you modify it enough to reflect the new data
  NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
  if (view == null) {
    view = new NSTableCellView ();
    if (tableColumn.Title == "Product") {
      view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
      view.AddSubview (view.ImageView);
      view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
    } else {
      view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
    }
    view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
    view.AddSubview (view.TextField);
    view.Identifier = tableColumn.Title;
    view.TextField.BackgroundColor = NSColor.Clear;
    view.TextField.Bordered = false;
    view.TextField.Selectable = false;
    view.TextField.Editable = true;

    view.TextField.EditingEnded += (sender, e) => {

      // Take action based on type
      switch(view.Identifier) {
      case "Product":
        DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
        break;
      case "Details":
        DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
        break; 
      }
    };
  }

  // Tag view
  view.TextField.Tag = row;

  // Setup view based on the column selected
  switch (tableColumn.Title) {
  case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tags.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    break;
  case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    break;
  }

  return view;
}
```

Дополнительные сведения см. в разделе [Использование изображений с табличными представлениями](~/mac/app-fundamentals/image.md) документации по [работе с изображением](~/mac/app-fundamentals/image.md) .

<a name="Adding-a-Delete-Button-to-a-Row" />

## <a name="adding-a-delete-button-to-a-row"></a>Добавление кнопки «Удалить» в строку

В зависимости от требований приложения в некоторых случаях необходимо указать кнопку действия для каждой строки в таблице. В качестве примера можно развернуть пример представления таблицы, созданный выше, чтобы включить кнопку **Delete** в каждую строку.

`Main.storyboard` Во-первых, измените Interface Builder в Xcode, выберите табличное представление и увеличьте число столбцов до трех (3). Затем измените **заголовок** нового столбца на `Action`:

[![](table-view-images/delete01.png "Изменение имени столбца")](table-view-images/delete01.png#lightbox)

Сохраните изменения в раскадровке и вернитесь в Visual Studio для Mac, чтобы синхронизировать изменения.

Затем измените `ViewController.cs` файл и добавьте следующий открытый метод:

```csharp
public void ReloadTable ()
{
  ProductTable.ReloadData ();
}
```

В том же файле измените создание нового делегата `ViewDidLoad` представления таблицы в методе следующим образом:

```csharp
// Populate the Product Table
ProductTable.DataSource = DataSource;
ProductTable.Delegate = new ProductTableDelegate (this, DataSource);
```

Теперь измените `ProductTableDelegate.cs` файл, включив в него частное подключение к контроллеру представления, и возьмите контроллер в качестве параметра при создании нового экземпляра делегата:

```csharp
#region Private Variables
private ProductTableDataSource DataSource;
private ViewController Controller;
#endregion

#region Constructors
public ProductTableDelegate (ViewController controller, ProductTableDataSource datasource)
{
  this.Controller = controller;
  this.DataSource = datasource;
}
#endregion
```

Затем добавьте в класс следующий новый частный метод:

```csharp
private void ConfigureTextField (NSTableCellView view, nint row)
{
  // Add to view
  view.TextField.AutoresizingMask = NSViewResizingMask.WidthSizable;
  view.AddSubview (view.TextField);

  // Configure
  view.TextField.BackgroundColor = NSColor.Clear;
  view.TextField.Bordered = false;
  view.TextField.Selectable = false;
  view.TextField.Editable = true;

  // Wireup events
  view.TextField.EditingEnded += (sender, e) => {

    // Take action based on type
    switch (view.Identifier) {
    case "Product":
      DataSource.Products [(int)view.TextField.Tag].Title = view.TextField.StringValue;
      break;
    case "Details":
      DataSource.Products [(int)view.TextField.Tag].Description = view.TextField.StringValue;
      break;
    }
  };

  // Tag view
  view.TextField.Tag = row;
}
```

Это принимает все конфигурации текстового представления, которые ранее выполнялись в `GetViewForItem` методе, и помещает их в одно, вызываемое расположение (поскольку последний столбец таблицы не включает текстовое представление, а кнопку).

Наконец, измените `GetViewForItem` метод и сделайте его следующим:

```csharp
public override NSView GetViewForItem (NSTableView tableView, NSTableColumn tableColumn, nint row)
{

  // This pattern allows you reuse existing views when they are no-longer in use.
  // If the returned view is null, you instance up a new view
  // If a non-null view is returned, you modify it enough to reflect the new data
  NSTableCellView view = (NSTableCellView)tableView.MakeView (tableColumn.Title, this);
  if (view == null) {
    view = new NSTableCellView ();

    // Configure the view
    view.Identifier = tableColumn.Title;

    // Take action based on title
    switch (tableColumn.Title) {
    case "Product":
      view.ImageView = new NSImageView (new CGRect (0, 0, 16, 16));
      view.AddSubview (view.ImageView);
      view.TextField = new NSTextField (new CGRect (20, 0, 400, 16));
      ConfigureTextField (view, row);
      break;
    case "Details":
      view.TextField = new NSTextField (new CGRect (0, 0, 400, 16));
      ConfigureTextField (view, row);
      break;
    case "Action":
      // Create new button
      var button = new NSButton (new CGRect (0, 0, 81, 16));
      button.SetButtonType (NSButtonType.MomentaryPushIn);
      button.Title = "Delete";
      button.Tag = row;

      // Wireup events
      button.Activated += (sender, e) => {
        // Get button and product
        var btn = sender as NSButton;
        var product = DataSource.Products [(int)btn.Tag];

        // Configure alert
        var alert = new NSAlert () {
          AlertStyle = NSAlertStyle.Informational,
          InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
          MessageText = $"Delete {product.Title}?",
        };
        alert.AddButton ("Cancel");
        alert.AddButton ("Delete");
        alert.BeginSheetForResponse (Controller.View.Window, (result) => {
          // Should we delete the requested row?
          if (result == 1001) {
            // Remove the given row from the dataset
            DataSource.Products.RemoveAt((int)btn.Tag);
            Controller.ReloadTable ();
          }
        });
      };

      // Add to view
      view.AddSubview (button);
      break;
    }

  }

  // Setup view based on the column selected
  switch (tableColumn.Title) {
  case "Product":
    view.ImageView.Image = NSImage.ImageNamed ("tag.png");
    view.TextField.StringValue = DataSource.Products [(int)row].Title;
    view.TextField.Tag = row;
    break;
  case "Details":
    view.TextField.StringValue = DataSource.Products [(int)row].Description;
    view.TextField.Tag = row;
    break;
  case "Action":
    foreach (NSView subview in view.Subviews) {
      var btn = subview as NSButton;
      if (btn != null) {
        btn.Tag = row;
      }
    }
    break;
  }

  return view;
}
```

Рассмотрим несколько разделов этого кода более подробно. Во-первых, если `NSTableViewCell` создается новое действие, основанное на имени столбца. Для первых двух столбцов (**Product** и **Details**) вызывается новый `ConfigureTextField` метод.

Для `NSButton` столбца **Action** создается и добавляется в ячейку в виде подпредставления:

```csharp
// Create new button
var button = new NSButton (new CGRect (0, 0, 81, 16));
button.SetButtonType (NSButtonType.MomentaryPushIn);
button.Title = "Delete";
button.Tag = row;
...

// Add to view
view.AddSubview (button);
```

`Tag` Свойство кнопки используется для хранения номера строки, которая обрабатывается в данный момент. Это число будет использоваться позже, когда пользователь запросит строку для удаления в `Activated` событии кнопки:

```csharp
// Wireup events
button.Activated += (sender, e) => {
  // Get button and product
  var btn = sender as NSButton;
  var product = DataSource.Products [(int)btn.Tag];

  // Configure alert
  var alert = new NSAlert () {
    AlertStyle = NSAlertStyle.Informational,
    InformativeText = $"Are you sure you want to delete {product.Title}? This operation cannot be undone.",
    MessageText = $"Delete {product.Title}?",
  };
  alert.AddButton ("Cancel");
  alert.AddButton ("Delete");
  alert.BeginSheetForResponse (Controller.View.Window, (result) => {
    // Should we delete the requested row?
    if (result == 1001) {
      // Remove the given row from the dataset
      DataSource.Products.RemoveAt((int)btn.Tag);
      Controller.ReloadTable ();
    }
  });
};
```

В начале обработчика событий мы получаем кнопку и продукт, который находится в указанной строке таблицы. Затем пользователю предлагается предупреждение об удалении строки. Если пользователь выбирает удаление строки, заданная строка удаляется из источника данных, а таблица перегружается:

```csharp
// Remove the given row from the dataset
DataSource.Products.RemoveAt((int)btn.Tag);
Controller.ReloadTable ();
```

Наконец, если вместо создания новой ячейки табличного представления используется повторно, следующий код настраивает его на основе обрабатываемого столбца:

```csharp
// Setup view based on the column selected
switch (tableColumn.Title) {
case "Product":
  view.ImageView.Image = NSImage.ImageNamed ("tag.png");
  view.TextField.StringValue = DataSource.Products [(int)row].Title;
  view.TextField.Tag = row;
  break;
case "Details":
  view.TextField.StringValue = DataSource.Products [(int)row].Description;
  view.TextField.Tag = row;
  break;
case "Action":
  foreach (NSView subview in view.Subviews) {
    var btn = subview as NSButton;
    if (btn != null) {
      btn.Tag = row;
    }
  }
  break;
}

```

Для столбца **Action** все вложенные представления проверяются до тех `NSButton` пор `Tag` , пока не будет найден, а затем свойство обновляется так, чтобы указывать на текущую строку.

После внесения этих изменений при запуске приложения каждая строка будет иметь кнопку **Удалить** :

[![](table-view-images/delete02.png "Представление таблицы с кнопками удаления")](table-view-images/delete02.png#lightbox)

Когда пользователь нажимает кнопку " **Удалить** ", отобразится предупреждение с запросом на удаление заданной строки:

[![](table-view-images/delete03.png "Предупреждение об удалении строки")](table-view-images/delete03.png#lightbox)

Если пользователь выберет DELETE, строка будет удалена, а таблица будет перерисована:

[![](table-view-images/delete04.png "Таблица после удаления строки")](table-view-images/delete04.png#lightbox)

<a name="Data_Binding_Table_Views" />

## <a name="data-binding-table-views"></a>Представления таблиц привязки данных

Благодаря использованию кодирования и методов привязки данных в приложении Xamarin. Mac можно значительно сократить объем кода, который необходимо написать и поддерживать, чтобы заполнить элементы пользовательского интерфейса и работать с ними. Кроме того, у вас есть преимущество в дальнейшем разделять резервные данные (_модель данных_) от интерфейса пользователя переднего плана (_модель-представление-контроллер_), что упрощает обслуживание, более гибкое проектирование приложений.

Кодирование "ключ-значение" (КВК) — это механизм непрямого доступа к свойствам объекта с помощью клавиш (специально отформатированных строк) для обнаружения свойств вместо доступа к ним через переменные`get/set`экземпляра или методы доступа (). Реализуя методы доступа, совместимые с кодированием значений, в приложении Xamarin. Mac, вы получаете доступ к другим macOS функциям, таким как контроль значений ключа (кво), привязка данных, основные данные, привязки Cocoa и сценарии.

Дополнительные сведения см. в разделе [Привязка данных табличного представления](~/mac/app-fundamentals/databinding.md#Table_View_Data_Binding) в нашей [привязке данных и документации по кодированию "ключ-значение](~/mac/app-fundamentals/databinding.md) ".

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье подробно рассматривается работа с табличными представлениями в приложении Xamarin. Mac. Мы видели различные типы и использование табличных представлений, как создавать и поддерживать табличные представления в Interface Builder Xcode и как работать с табличными представлениями в C# коде.

## <a name="related-links"></a>Связанные ссылки

- [Мактаблес (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/mactables)
- [MacImages (пример)](https://docs.microsoft.com/samples/xamarin/mac-samples/macimages)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Представления структур](~/mac/user-interface/outline-view.md)
- [Списки источников](~/mac/user-interface/source-list.md)
- [Привязка данных и кодирование типа "ключ — значение"](~/mac/app-fundamentals/databinding.md)
- [Рекомендации по работе с человеческим интерфейсом OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [нстаблевиев](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSTableView_Class/index.html#//apple_ref/doc/uid/TP40004125)
- [NSTableViewDelegate](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/NSTableViewDelegate_Protocol/index.html#//apple_ref/doc/uid/TP40008622)
- [нстаблевиевдатасаурце](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Protocols/NSTableDataSource_Protocol/index.html#//apple_ref/doc/uid/TP40004178)
