---
title: Создание приложения Xamarin. iOS с помощью API элементов
description: Эта статья посвящена информации, приведенной в статье Общие сведения о диалоговом окне с ограниченным касанием. В нем представлено пошаговое руководство, в котором показано, как использовать «некасание» (MT. D). API элементов, чтобы быстро приступить к созданию приложения с помощью MT. Четырехмерного.
ms.prod: xamarin
ms.assetid: F1124734-DF44-F1F3-0832-46F52A788CDC
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: lobrien
ms.author: laobri
ms.openlocfilehash: ab7761071ef0795d054febbfb302702e09d80c53
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69528460"
---
# <a name="creating-a-xamarinios-application-using-the-elements-api"></a>Создание приложения Xamarin. iOS с помощью API элементов

_Эта статья посвящена информации, приведенной в статье Общие сведения о диалоговом окне с ограниченным касанием. В нем представлено пошаговое руководство, в котором показано, как использовать «некасание» (MT. D). API элементов, чтобы быстро приступить к созданию приложения с помощью MT. Четырехмерного._

В этом пошаговом руководстве мы будем использовать MT. D элементов API для создания стиля приложения "основной/подробности", отображающего список задач. Когда пользователь нажимает **+** кнопку на панели навигации, в таблицу добавляется новая строка для задачи. Выбор строки приведет к переходу на экран сведений, который позволяет обновить описание задачи и дату выполнения, как показано ниже:

 [![](elements-api-walkthrough-images/01-task-list-app.png "Выбор строки приведет к переходу на экран сведений, который позволяет обновить описание задачи и дату выполнения.")](elements-api-walkthrough-images/01-task-list-app.png#lightbox)

 ## <a name="setting-up-mtd"></a>Настройка MT. Четырехмерного

Машин. D распространяется с помощью Xamarin. iOS. Чтобы использовать его, щелкните правой кнопкой мыши узел **ссылки** проекта Xamarin. iOS в Visual Studio 2017 или Visual Studio для Mac и добавьте ссылку на сборку " **котушь. Dialog-1** ". Затем добавьте `using MonoTouch.Dialog` в исходный код инструкции, если это необходимо.

## <a name="elements-api-walkthrough"></a>Пошаговое руководство по API элементов

В статье [Общие сведения о диалоговом окне с](~/ios/user-interface/monotouch.dialog/index.md) ограниченным касанием мы получили полное представление о различных частях MT. Четырехмерного. Давайте будем использовать API Elements, чтобы объединить их в приложение.

## <a name="setting-up-the-multi-screen-application"></a>Настройка приложения для нескольких экранов

Чтобы начать процесс создания экрана, `DialogViewController`можно создать, а затем `RootElement`добавить.

Чтобы создать Многоэкранное приложение с помощью одноуровневого диалогового окна, необходимо выполнить следующие действия.

1. Создайте`UINavigationController.`
1. Создайте`DialogViewController.`
1. Добавьте в `DialogViewController` качестве корня`UINavigationController.` 
1. `RootElement` Добавьте в`DialogViewController.`
1. Добавьте `Sections` и `Elements` в`RootElement.` 

### <a name="using-a-uinavigationcontroller"></a>Использование Уинавигатионконтроллер

Чтобы создать приложение в стиле навигации, `UINavigationController`необходимо создать объект, а затем добавить его `RootViewController` как `AppDelegate`в `FinishedLaunching` метод класса. Чтобы сделать `UINavigationController` работу с котушь. Dialog, `DialogViewController` добавьте в, `UINavigationController` как показано ниже:

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
    _window = new UIWindow (UIScreen.MainScreen.Bounds);
            
    _rootElement = new RootElement ("To Do List"){new Section ()};

    // code to create screens with MT.D will go here …

    _rootVC = new DialogViewController (_rootElement);
    _nav = new UINavigationController (_rootVC);
    _window.RootViewController = _nav;
    _window.MakeKeyAndVisible ();
            
    return true;
}
```

Приведенный выше код создает экземпляр класса `RootElement` и передает его `DialogViewController`в. `DialogViewController` Всегда`RootElement` имеет в верхней части иерархии. В этом примере `RootElement` создается со строкой «Do List», которая служит заголовком на панели навигации контроллера навигации. На этом этапе при запуске приложения появится экран, показанный ниже:

 [![](elements-api-walkthrough-images/02-to-do-list-screen-.png "При запуске приложения появится показанный здесь экран")](elements-api-walkthrough-images/02-to-do-list-screen-.png#lightbox)

Давайте посмотрим, как использовать иерархическую структуру `Sections` диалогового окна и `Elements` добавить дополнительные экраны.

### <a name="creating-the-dialog-screens"></a>Создание экранов диалогового окна

`DialogViewController` Представляетсобойподкласс,которыйможноиспользовать`UITableViewController` для добавления экранов. Очень касание. диалоговое окно создает экраны, `DialogViewController`добавляя `RootElement` в, как мы видели выше. `RootElement` Может иметь`Section` экземпляры, представляющие разделы таблицы.
Разделы состоят из элементов, других разделов или даже других `RootElements`. Вложенное `RootElements`диалоговое окно автоматически создает приложение в стиле навигации, как будет показано далее.

### <a name="using-dialogviewcontroller"></a>Использование Диалогвиевконтроллер

Объект, являющийся подклассом, имеет `UITableView` в качестве своего представления. `UITableViewController` `DialogViewController` В этом примере мы хотим добавлять элементы в таблицу при каждом **+** нажатии кнопки. Так как `DialogViewController` объект был добавлен `UINavigationController`в `NavigationItem`, `RightBarButton` мы можем использовать свойство, чтобы добавить **+** кнопку, как показано ниже:

```csharp
_addButton = new UIBarButtonItem (UIBarButtonSystemItem.Add);
_rootVC.NavigationItem.RightBarButtonItem = _addButton;
```

Когда мы создали `RootElement` ранее, мы передали ему единственный `Section` экземпляр, чтобы мы **+** могли добавлять элементы при нажатии кнопки пользователем. Чтобы сделать это в обработчике событий для кнопки, можно использовать следующий код:

```csharp
_addButton.Clicked += (sender, e) => {                
    ++n;
                
    var task = new Task{Name = "task " + n, DueDate = DateTime.Now};
                
    var taskElement = new RootElement (task.Name) {
        new Section () {
            new EntryElement (task.Name, "Enter task description", task.Description)
        },
        new Section () {
            new DateElement ("Due Date", task.DueDate)
        }
    };
    _rootElement [0].Add (taskElement);
};
```

Этот код создает новый `Task` объект каждый раз при нажатии кнопки. Ниже показана простая реализация `Task` класса.

```csharp
public class Task
{   
    public Task ()
    {
    }
      
    public string Name { get; set; }
        
    public string Description { get; set; }

    public DateTime DueDate { get; set; }
}
```

`Name` Свойство задачи используется для `RootElement`создания заголовка и переменной счетчика с именем `n` , которая увеличивается для каждой новой задачи. Однокасание. диалоговое окно преобразует элементы в строки, добавляемые в `TableView` при добавлении `taskElement` каждого из них.

## <a name="presenting-and-managing-dialog-screens"></a>Представление экранов диалоговых окон и управление ими

Мы использовали, `RootElement` чтобы в диалоговом окне было автоматически создано новое окно для сведений о каждой задаче и переходить к ней при выборе строки.

Экран сведений о задаче состоит из двух разделов. Каждый из этих разделов содержит один элемент. Первый элемент создается из объекта `EntryElement` для предоставления редактируемой строки для `Description` свойства задачи. При выборе элемента клавиатура для редактирования текста представлена, как показано ниже:

 [![](elements-api-walkthrough-images/03-create-task.png "Если элемент выбран, для редактирования текста отображается клавиатура, как показано")](elements-api-walkthrough-images/03-create-task.png#lightbox)

Второй раздел содержит объект `DateElement` , позволяющий управлять `DueDate` свойством задачи. При выборе даты автоматически загружается средство выбора даты, как показано ниже.

 [![](elements-api-walkthrough-images/04-date-picker.png "При выборе даты автоматически загружается средство выбора даты")](elements-api-walkthrough-images/04-date-picker.png#lightbox)

В обоих `EntryElement` `DateElement` случаях (или для любого элемента ввода данных в виде однокасания. Dialog) любые изменения значений сохраняются автоматически. Это можно продемонстрировать, изменив дату, а затем перейдя между корневым экраном и различными сведениями о задачах, где значения на экранах сведений сохраняются.

## <a name="summary"></a>Сводка

В этой статье представлено пошаговое руководство, в котором показано, как использовать API элементов с элементами управления "только для касания". В нем описаны основные шаги по созданию многоэкранного приложения с помощью MT. D, включая использование `DialogViewController` и добавление элементов и разделов для создания экранов. Кроме того, в нем было показано, как использовать MT. D в сочетании с `UINavigationController`.

## <a name="related-links"></a>Связанные ссылки

- [Мтдвалксраугх (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/mtdwalkthrough)
- [Введение в бескасание. Dialog](~/ios/user-interface/monotouch.dialog/index.md)
- [Пошаговое руководство по API отражения](~/ios/user-interface/monotouch.dialog/reflection-api-walkthrough.md)
- [Пошаговое руководство по элементу JSON](~/ios/user-interface/monotouch.dialog/json-element-walkthrough.md)
- [Диалоговое окно с несенсорным касанием на GitHub](https://github.com/migueldeicaza/MonoTouch.Dialog)
- [Приложение Твитстатион](https://github.com/migueldeicaza/TweetStation)
- [Справочник по классам Уитаблевиевконтроллер](https://developer.apple.com/library/ios/#DOCUMENTATION/UIKit/Reference/UITableViewController_Class/Reference/Reference.html)
- [Справочник по классам Уинавигатионконтроллер](https://developer.apple.com/library/ios/#documentation/UIKit/Reference/UINavigationController_Class/Reference/Reference.html)
