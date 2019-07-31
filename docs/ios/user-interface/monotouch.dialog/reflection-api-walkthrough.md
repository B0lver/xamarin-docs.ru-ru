---
title: Создание приложения Xamarin. iOS с помощью API отражения
description: В этом документе описан API-интерфейс отражения на основе атрибутов, который создает пользовательский интерфейс на основе классов, оформленных атрибутами.
ms.prod: xamarin
ms.assetid: C0F923D2-300E-DB9D-F390-9FA71B22DFD6
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: 9dc98480d57dc7d4883306e9f4fc530599af9bd7
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68652419"
---
# <a name="creating-a-xamarinios-application-using-the-reflection-api"></a>Создание приложения Xamarin. iOS с помощью API отражения

MT. API отражения D позволяет декорированию классов с помощью атрибутов, которые MT. D использует для автоматического создания экранов. API отражения обеспечивает привязку между этими классами и то, что отображается на экране. Хотя этот API не предоставляет детализированный элемент управления, который делает API элементов, он сокращает сложность за счет автоматического создания иерархии элементов на основе декорирования классов.

## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем добавьте `using MonoTouch.Dialog` в исходный код инструкции, если это необходимо.

## <a name="getting-started-with-the-reflection-api"></a>Приступая к работе с API отражения

Использовать API отражения очень просто:

1.  Создание класса, снабженного MT. Атрибуты D.
1.  `BindingContext` Создание экземпляра, передавая ему экземпляр указанного выше класса. 
1.  `DialogViewController` Создание, передавая `BindingContext’s` `RootElement` ему. 


Рассмотрим пример, иллюстрирующий использование API отражения. В этом примере мы создадим простой экран ввода данных, как показано ниже:

 [![](reflection-api-walkthrough-images/01-expense-entry.png "В этом примере мы создадим простой экран ввода данных, как показано ниже.")](reflection-api-walkthrough-images/01-expense-entry.png#lightbox)

## <a name="creating-a-class-with-mtd-attributes"></a>Создание класса с помощью MT. Атрибуты D

В первую очередь нам нужно использовать API отражения — это класс, дополненный атрибутами. Эти атрибуты будут использоваться MT. D — внутренне создание объектов из API элементов. Например, рассмотрим следующее определение класса:

```csharp
public class Expense
{
    [Section("Expense Entry")]

    [Entry("Enter expense name")]
    public string Name;
        
    [Section("Expense Details")]
  
    [Caption("Description")]
    [Entry]
    public string Details;
        
    [Checkbox]
    public bool IsApproved = true;
}
```

В `SectionAttribute` результате будут созданы разделы `UITableView` , содержащие строковый аргумент, используемый для заполнения заголовка раздела. После объявления раздела все поля, следующие за ним, будут включаться в этот раздел, пока не будет объявлен другой раздел.
Тип элемента пользовательского интерфейса, созданного для поля, будет зависеть от типа поля и MT. Атрибут D.

Например, `Name` поле имеет значение `string` , а `EntryAttribute`оно дополнено. Это приводит к добавлению строки в таблицу с полем ввода текста и указанным заголовком. Аналогичным образом `bool` `CheckboxAttribute`поле — это, в результате чего строка таблицы с флажком справа от ячейки таблицы. `IsApproved` Машин. D использует имя поля, автоматически добавляя пробел, чтобы создать заголовок в этом случае, так как он не указан в атрибуте.

## <a name="adding-the-bindingcontext"></a>Добавление BindingContext

Чтобы использовать `Expense` класс, необходимо `BindingContext`создать. `BindingContext` — Это класс, который будет привязывать данные из класса с атрибутами для создания иерархии элементов. Чтобы создать его, просто создайте экземпляр класса и передайте его в конструктор.

Например, чтобы добавить пользовательский интерфейс, объявленный с помощью атрибута в `Expense` классе, включите следующий код `FinishedLaunching` в метод `AppDelegate`класса:

```csharp
var expense = new Expense ();
var bctx = new BindingContext (null, expense, "Create a task");
```

Затем все, что нужно сделать для создания пользовательского интерфейса, — добавить `BindingContext` `DialogViewController` в и задать его в качестве значения `RootViewController` в окне, как показано ниже:

```csharp
UIWindow window;

public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{   
    window = new UIWindow (UIScreen.MainScreen.Bounds);
            
    var expense = new Expense ();
    var bctx = new BindingContext (null, expense, "Create a task");
    var dvc = new DialogViewController (bctx.Root);
            
    window.RootViewController = dvc;
    window.MakeKeyAndVisible ();
            
    return true;
}
```

Запуск приложения теперь приводит к отображению экрана, показанного выше.

### <a name="adding-a-uinavigationcontroller"></a>Добавление Уинавигатионконтроллер

Обратите внимание, что заголовок «создание задачи», переданный `BindingContext` в, не отображается. Это вызвано тем `DialogViewController` , что не является частью `UINavigatonController`. Измените код, чтобы добавить `UINavigationController` в качестве `RootViewController,` окна, и добавьте его `DialogViewController` в качестве корня, `UINavigationController` как показано ниже:

```csharp
nav = new UINavigationController(dvc);
window.RootViewController = nav;
```

Теперь при запуске приложения заголовок отображается на `UINavigationController’s` панели навигации, как показано ниже.

 [![](reflection-api-walkthrough-images/02-create-task.png "Теперь при запуске приложения заголовок отображается на панели навигации Уинавигатионконтроллерс.")](reflection-api-walkthrough-images/02-create-task.png#lightbox)

Благодаря включению `UINavigationController`можно использовать преимущества других функций MT. D, для которого требуется Навигация. Например, можно добавить перечисление в `Expense` класс, чтобы определить категорию для расходов и MT. D создаст экран выбора автоматически. Чтобы продемонстрировать, измените класс `Expense` так, чтобы он `ExpenseCategory` включал поле, как показано ниже.

```csharp
public enum Category
{
    Travel,
    Lodging,
    Books
}
        
public class Expense
{
    …

    [Caption("Category")]
    public Category ExpenseCategory;
}
```

Выполнение приложения теперь приводит к отображению новой строки в таблице для категории, как показано ниже.

 [![](reflection-api-walkthrough-images/03-set-details.png "Выполнение приложения теперь приводит к отображению новой строки в таблице для категории, как показано ниже.")](reflection-api-walkthrough-images/03-set-details.png#lightbox)

Выбор строки приводит к переходу приложения на новый экран со строками, соответствующими перечислениям, как показано ниже:

 [![](reflection-api-walkthrough-images/04-set-category.png "Выбор строки приводит к переходу приложения на новый экран со строками, соответствующими перечислению")](reflection-api-walkthrough-images/04-set-category.png#lightbox)

 <a name="Summary" />


## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство по API отражения. Мы показали, как добавить атрибуты в класс для управления отображаемыми сведениями. Мы также рассмотрели, `BindingContext` как использовать для привязки данных класса к создаваемой иерархии элементов, а также как использовать MT. D с `UINavigationController`.


## <a name="related-links"></a>Связанные ссылки

- [Мтдрефлектионвалксраугх (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdreflectionwalkthrough)
- [Общие сведения о диалоговом окне для бескасания](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство по API элементов](~/ios/user-interface/monotouch.dialog/elements-api-walkthrough.md)
- [Пошаговое руководство по элементу JSON](~/ios/user-interface/monotouch.dialog/monotouch.dialog-json-markup.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Приложение Твитстатион](https://github.com/migueldeicaza/TweetStation)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
