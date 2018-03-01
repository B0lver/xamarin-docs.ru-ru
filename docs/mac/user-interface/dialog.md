---
title: "Диалоговые окна"
description: "В этой статье рассматривается работа с диалоговые окна и модальные окна в приложении Xamarin.Mac. Он описывает создание модальных окон в Xcode и интерфейс построителя, работа с стандартных диалоговых окон и работать с этими элементами управления в коде C#."
ms.topic: article
ms.prod: xamarin
ms.assetid: F1DB93A1-7549-4540-AD5E-D7605CCD8435
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 9b65e870fae0074726d0bdd46d9eecbe99240e98
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="dialogs"></a>Диалоговые окна

При работе с C# и .NET в приложении Xamarin.Mac, имеется доступ к одним и тем же диалоговые окна и модальных окон, работающий в *Objective-C* и *Xcode* does. Поскольку Xamarin.Mac напрямую интегрируется с Xcode, можно использовать в Xcode _интерфейс построителя_ для создания и поддержания модальных окон (или их при необходимости создать непосредственно в код C#).

Диалоговое окно появляется в ответ на действия пользователя и обычно предоставляет пользователям способов выполнить действие. Диалоговое окно требует ответа от пользователя, прежде чем можно будет закрыть.

Windows можно в состоянии без режима (например, текстовый редактор, который может иметь одновременно открыть несколько документов) либо модальное (например, в диалоговом окне экспорта, необходимо отменить перед продолжением приложения).

[ ![](dialog-images/dialog03.png "Открытое окно")](dialog-images/dialog03.png)

В этой статье мы обсудим основы работы с диалоговые окна и модальные окна в приложении Xamarin.Mac. Настоятельно рекомендуется работать через [Hello, Mac](~/mac/get-started/hello-mac.md) статью, во-первых, в частности [введение в Xcode и интерфейс построителя](~/mac/get-started/hello-mac.md#Introduction_to_Xcode_and_Interface_Builder) и [выходов и действия](~/mac/get-started/hello-mac.md#Outlets_and_Actions) разделы, как он охватывает основные принципы и методы, которые будут использоваться в этой статье.

Может потребоваться рассмотреть [предоставление C# классы и методы для Objective-C](~/mac/internals/how-it-works.md) раздел [внутренние компоненты Xamarin.Mac](~/mac/internals/how-it-works.md) документов также объясняется, какие `Register` и `Export` команд используется для передачи до классов C# Objective-C-объекты и элементы пользовательского интерфейса.

<a name="Introduction_to_Dialogs" />

## <a name="introduction-to-dialogs"></a>Общие сведения о диалоговых окнах

Диалоговое окно появляется в ответ на действия пользователя (например, при сохранении файла) и позволяет пользователям для выполнения этого действия. Диалоговое окно требует ответа от пользователя, прежде чем можно будет закрыть.

В соответствии с Apple можно в диалоговом окне тремя способами:

- **Документирование Modal** -документа модальное диалоговое окно не позволяет пользователю выполнять ничего в рамках данного документа до его закрытия.
- **Modal приложения** — это приложение, модальное диалоговое окно не позволяет пользователю взаимодействовать с приложением, пока оно не будет закрыто.
- **Немодальный** диалоговое окно немодальный пользователи могут изменять параметры в диалоговом окне при по-прежнему взаимодействия с окном документа.

### <a name="modal-window"></a>Модальное окно

Любой стандарт `NSWindow` может использоваться как настраиваемого диалогового окна, отобразив его как модальная:

[ ![](dialog-images/modal01.png "Пример модальное окно")](dialog-images/modal01.png)

### <a name="document-modal-dialog-sheets"></a>Листы модальное диалоговое окно документа

Объект _лист_ является модальным диалоговым окном, прикрепленный к окну данного документа, предотвращая взаимодействия с окном, пока они закрыть диалоговое окно. Лист прикрепленный к окну, из которого он возникает и в любой момент времени только на одном листе может быть открыт для окна.

[ ![](dialog-images/sheet08.png "Пример модального листа")](dialog-images/sheet08.png)

### <a name="preferences-windows"></a>Параметры Windows

Окно Параметры — немодального диалогового окна, который содержит параметры приложения, которые пользователь редко изменяются. Параметры Windows часто включают панель инструментов, которая позволяет пользователю переключаться между различные группы параметров:

[ ![](dialog-images/dialog02.png "Пример настройки окна")](dialog-images/dialog02.png)

### <a name="open-dialog"></a>Откройте диалоговое окно

Откройте диалоговое окно предоставляет пользователям согласованный способ найти и открыть элемент в приложении:

[ ![](dialog-images/dialog03.png "Откройте диалоговое окно")](dialog-images/dialog03.png)


### <a name="print-and-page-setup-dialogs"></a>Печать и диалоговых окнах параметры страницы

macOS предоставляет стандартный печати и страницы установки диалоговые окна, отображающие приложения так, чтобы пользователи могли иметь согласованные печати возможности во всех приложениях, которые они используют.

Диалоговое окно печати могут отображаться как оба бесплатно с плавающей запятой диалоговое окно:

[ ![](dialog-images/print01.png "Диалоговое окно печати")](dialog-images/print01.png)

Или он может отображаться в виде листа:

[ ![](dialog-images/print02.png "Печать листа")](dialog-images/print02.png)

Диалоговое окно настройки страницы может отображаться как оба бесплатно с плавающей запятой диалоговое окно:

[ ![](dialog-images/print03.png "Диалоговое окно настройки страницы")](dialog-images/print03.png)

Или он может отображаться в виде листа:

[ ![](dialog-images/print04.png "Лист страница установки")](dialog-images/print04.png)

### <a name="save-dialogs"></a>Сохранить диалоговые окна

Диалоговое окно сохранения предоставляет пользователям согласованный способ сохранения элемента в приложении. Диалоговое окно сохранения имеет два состояния: **минимальные** (также известный как свернутой):

[ ![](dialog-images/save01.png "Сохранение диалогового окна")](dialog-images/save01.png)

И **разреженный** состояния:

[ ![](dialog-images/save02.png "Развернутое диалоговое окно сохранения")](dialog-images/save02.png)

**Минимальные** сохранить диалоговое окно может отображаться в качестве листа:

[ ![](dialog-images/save03.png "Минимальный сохраните лист")](dialog-images/save03.png)

Как можно **разреженный** сохранить диалоговое окно:

[ ![](dialog-images/save04.png "Расширенная сохранить лист")](dialog-images/save04.png)

Дополнительные сведения см. в разделе [диалоговые окна](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/WindowDialogs.html#//apple_ref/doc/uid/20000957-CH43-SW1) раздел Apple [рекомендациям по интерфейсам OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)

<a name="Adding_a_Modal_Window_to_a_Project" />

## <a name="adding-a-modal-window-to-a-project"></a>Добавление в проект модальное окно

Помимо основным окном документа приложении Xamarin.Mac может возникнуть необходимость отображения других типов windows для пользователя, такие как настройки или панели инспектора.

Чтобы добавить новое окно, выполните следующие действия.

1. В **обозревателе решений**откройте `Main.storyboard` файл для редактирования в построителе интерфейса в Xcode.
2. Перетащите новую **View Controller** в рабочей области конструктора:

    [ ![](dialog-images/new01.png "При выборе контроллера представления из библиотеки")](dialog-images/new01.png)
3. В **удостоверение инспектора**, введите `CustomDialogController` для **имя класса**: 

    [ ![](dialog-images/new02.png "Задание имени класса")](dialog-images/new02.png)
4. Переключитесь в Visual Studio для Mac, разрешить его для синхронизации с Xcode и создать `CustomDialogController.h` файла.
5. Вернитесь в Xcode и спроектировать свой интерфейс: 

    [ ![](dialog-images/new03.png "Проектирование пользовательского интерфейса в Xcode")](dialog-images/new03.png)
6. Создание **модальное Перейти** из главного окна приложения на новый контроллер представление, перетащив элемент управления из элемент пользовательского интерфейса, который можно будет открыть диалоговое окно на диалоговое окно. Назначьте **идентификатор** `ModalSegue`: 

    [ ![](dialog-images/new06.png "Модальные segue")](dialog-images/new06.png)
6. Проводной доступ к любой **действия** и **торговцам**: 

    [ ![](dialog-images/new04.png "Настройка действий")](dialog-images/new04.png)
6. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации с Xcode.

Сделать `CustomDialogController.cs` внешний файл следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class CustomDialogController : NSViewController
    {
        #region Private Variables
        private string _dialogTitle = "Title";
        private string _dialogDescription = "Description";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string DialogTitle {
            get { return _dialogTitle; }
            set { _dialogTitle = value; }
        }

        public string DialogDescription {
            get { return _dialogDescription; }
            set { _dialogDescription = value; }
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public CustomDialogController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial title and description
            Title.StringValue = DialogTitle;
            Description.StringValue = DialogDescription;
        }
        #endregion

        #region Private Methods
        private void CloseDialog() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptDialog (Foundation.NSObject sender) {
            RaiseDialogAccepted();
            CloseDialog();
        }

        partial void CancelDialog (Foundation.NSObject sender) {
            RaiseDialogCanceled();
            CloseDialog();
        }
        #endregion

        #region Events
        public EventHandler DialogAccepted;

        internal void RaiseDialogAccepted() {
            if (this.DialogAccepted != null)
                this.DialogAccepted (this, EventArgs.Empty);
        }

        public EventHandler DialogCanceled;

        internal void RaiseDialogCanceled() {
            if (this.DialogCanceled != null)
                this.DialogCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Этот код предоставляет несколько свойств, чтобы задать заголовок и описание диалогового окна и несколько событий для реагирования на диалоговое окно, отменено или принято.

Далее следует изменить `ViewController.cs` файла, переопределите `PrepareForSegue` метод и сделать его выглядеть следующим образом:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    }
}
```

Этот код инициализирует segue, которое было определено в Xcode интерфейс построителя нашей диалоговое окно и устанавливает заголовок и описание. Он также обрабатывает выбор, вносимые пользователем в диалоговом окне.

Можно запустить приложение и отобразить настраиваемое диалоговое окно:

[ ![](dialog-images/new05.png "Пример этого окна")](dialog-images/new05.png)

Дополнительные сведения об использовании windows в приложении Xamarin.Mac см. в разделе нашей [работа с окнами](~/mac/user-interface/window.md) документации.

<a name="Creating_a_Custom_Sheet" />

## <a name="creating-a-custom-sheet"></a>Создание пользовательской вкладки

Объект _лист_ является модальным диалоговым окном, прикрепленный к окну данного документа, предотвращая взаимодействия с окном, пока они закрыть диалоговое окно. Лист прикрепленный к окну, из которого он возникает и в любой момент времени только на одном листе может быть открыт для окна. 

Чтобы создать пользовательский лист в Xamarin.Mac, давайте выполните следующее:

1. В **обозревателе решений**откройте `Main.storyboard` файл для редактирования в построителе интерфейса в Xcode.
2. Перетащите новую **View Controller** в рабочей области конструктора:

    [ ![](dialog-images/new01.png "При выборе контроллера представления из библиотеки")](dialog-images/new01.png)
2. Разработка пользовательского интерфейса.

    [ ![](dialog-images/sheet01.png "Разработки пользовательского интерфейса")](dialog-images/sheet01.png)
3. Создание **перейти лист** из главного окна на новый контроллер представление: 

    [ ![](dialog-images/sheet02.png "Выбор типа segue лист")](dialog-images/sheet02.png)
4. В **удостоверение инспектора**, имя контроллера представления **класса** `SheetViewController`: 

    [ ![](dialog-images/sheet03.png "Задание имени класса")](dialog-images/sheet03.png)
5. Определять любые необходимые **торговцам** и **действия**: 

    [ ![](dialog-images/sheet04.png "Определение необходимых выходов и действия")](dialog-images/sheet04.png)
6. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации.

Далее следует изменить `SheetViewController.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace MacDialog
{
    public partial class SheetViewController : NSViewController
    {
        #region Private Variables
        private string _userName = "";
        private string _password = "";
        private NSViewController _presentor;
        #endregion

        #region Computed Properties
        public string UserName {
            get { return _userName; }
            set { _userName = value; }
        }

        public string Password {
            get { return _password;}
            set { _password = value;}
        }

        public NSViewController Presentor {
            get { return _presentor; }
            set { _presentor = value; }
        }
        #endregion

        #region Constructors
        public SheetViewController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void ViewWillAppear ()
        {
            base.ViewWillAppear ();

            // Set initial values
            NameField.StringValue = UserName;
            PasswordField.StringValue = Password;

            // Wireup events
            NameField.Changed += (sender, e) => {
                UserName = NameField.StringValue;
            };
            PasswordField.Changed += (sender, e) => {
                Password = PasswordField.StringValue;
            };
        }
        #endregion

        #region Private Methods
        private void CloseSheet() {
            Presentor.DismissViewController (this);
        }
        #endregion

        #region Custom Actions
        partial void AcceptSheet (Foundation.NSObject sender) {
            RaiseSheetAccepted();
            CloseSheet();
        }

        partial void CancelSheet (Foundation.NSObject sender) {
            RaiseSheetCanceled();
            CloseSheet();
        }
        #endregion

        #region Events
        public EventHandler SheetAccepted;

        internal void RaiseSheetAccepted() {
            if (this.SheetAccepted != null)
                this.SheetAccepted (this, EventArgs.Empty);
        }

        public EventHandler SheetCanceled;

        internal void RaiseSheetCanceled() {
            if (this.SheetCanceled != null)
                this.SheetCanceled (this, EventArgs.Empty);
        }
        #endregion
    }
}
```

Далее следует изменить `ViewController.cs` файл, изменить `PrepareForSegue` метод и сделать его выглядеть следующим образом:

```csharp
public override void PrepareForSegue (NSStoryboardSegue segue, NSObject sender)
{
    base.PrepareForSegue (segue, sender);

    // Take action based on the segue name
    switch (segue.Identifier) {
    case "ModalSegue":
        var dialog = segue.DestinationController as CustomDialogController;
        dialog.DialogTitle = "MacDialog";
        dialog.DialogDescription = "This is a sample dialog.";
        dialog.DialogAccepted += (s, e) => {
            Console.WriteLine ("Dialog accepted");
            DismissViewController (dialog);
        };
        dialog.Presentor = this;
        break;
    case "SheetSegue":
        var sheet = segue.DestinationController as SheetViewController;
        sheet.SheetAccepted += (s, e) => {
            Console.WriteLine ("User Name: {0} Password: {1}", sheet.UserName, sheet.Password);
        };
        sheet.Presentor = this;
        break;
    }
}
```

Если запустить приложение и откройте страницу, она будет присоединена к окна:

[ ![](dialog-images/sheet08.png "Пример листа")](dialog-images/sheet08.png)

<a name="Creating_a_Preferences_Dialog" />

## <a name="creating-a-preferences-dialog"></a>Создание диалогового окна настройки

Прежде чем мы макета представления предпочтений в построителе интерфейса, нам нужно будет добавить тип пользовательского segue для обработки исходящему переключению в параметрах. Добавьте новый класс в проект и назовите его `ReplaceViewSeque `. Измените класс и сделать его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using Foundation;

namespace MacWindows
{
    [Register("ReplaceViewSeque")]
    public class ReplaceViewSeque : NSStoryboardSegue
    {
        #region Constructors
        public ReplaceViewSeque() {

        }

        public ReplaceViewSeque (string identifier, NSObject sourceController, NSObject destinationController) : base(identifier,sourceController,destinationController) {

        }

        public ReplaceViewSeque (IntPtr handle) : base(handle) {
        }

        public ReplaceViewSeque (NSObjectFlag x) : base(x) {
        }
        #endregion

        #region Override Methods
        public override void Perform ()
        {
            // Cast the source and destination controllers
            var source = SourceController as NSViewController;
            var destination = DestinationController as NSViewController;

            // Is there a source?
            if (source == null) {
                // No, get the current key window
                var window = NSApplication.SharedApplication.KeyWindow;

                // Swap the controllers
                window.ContentViewController = destination;

                // Release memory
                window.ContentViewController?.RemoveFromParentViewController ();
            } else {
                // Swap the controllers
                source.View.Window.ContentViewController = destination;

                // Release memory
                source.RemoveFromParentViewController ();
            }
        
        }
        #endregion

    }

}
```

Пользовательские segue создан мы можем добавить новое окно в построителе интерфейса в Xcode для обработки нашей предпочтения.

Чтобы добавить новое окно, выполните следующие действия.

1. В **обозревателе решений**откройте `Main.storyboard` файл для редактирования в построителе интерфейса в Xcode.
2. Перетащите новую **окна контроллера** в рабочей области конструктора:

    [ ![](dialog-images/pref01.png "Выберите окно контроллер из библиотеки")](dialog-images/pref01.png)
3. Расположите рядом окна **меню** конструктор:

    [ ![](dialog-images/pref02.png "Добавление нового окна")](dialog-images/pref02.png)
4. Создайте копии вложенное представление-контроллер, как будет вкладок в представлении предпочтения:

    [ ![](dialog-images/pref03.png "Добавление необходимых контроллеров представления")](dialog-images/pref03.png)
5. Перетащите новую **инструментов контроллера** из **библиотеки**:

    [ ![](dialog-images/pref04.png "Выберите контроллер инструментов из библиотеки")](dialog-images/pref04.png)
6. И поместите его в окне в области конструктора.

    [ ![](dialog-images/pref05.png "Добавление нового контроллера панели инструментов")](dialog-images/pref05.png)
7. Макет конструктора на панели инструментов:

    [ ![](dialog-images/pref06.png "Макет панели инструментов")](dialog-images/pref06.png)
8. Удерживая нажатой, перетащите из каждого **кнопки панели инструментов** с представлениями, созданной ранее. Выберите **настраиваемый** перейти типа:

    [ ![](dialog-images/pref07.png "Segue тип параметра")](dialog-images/pref07.png)
9. Выберите новый Segue и задайте **класса** для `ReplaceViewSegue`:

    [ ![](dialog-images/pref08.png "Класс segue параметров")](dialog-images/pref08.png)
10. В **конструктор Menubar** в области конструктора в меню приложения выберите **установки...** управления щелкните и перетащите в окно "Параметры" для создания **Показать** перейти:

    [ ![](dialog-images/pref09.png "Segue тип параметра")](dialog-images/pref09.png)
11. Сохранить изменения и вернуться в Visual Studio для Mac для синхронизации.

Если выполнение кода и выберите **установки...**  из **меню приложения**, откроется окно:

[ ![](dialog-images/pref10.png "Пример настройки окна")](dialog-images/pref10.png)

Дополнительные сведения о работе с Windows и на панелях инструментов см. в разделе нашей [Windows](~/mac/user-interface/window.md) и [панели инструментов](~/mac/user-interface/toolbar.md) документации.

<a name="Saving-and-Loading-Preferences" />

### <a name="saving-and-loading-preferences"></a>Сохранение и загрузка настроек

В обычной macOS приложения когда пользователь вносит изменения в любые пользовательские настройки приложения, эти изменения сохраняются автоматически. Для обработки в данном в приложении Xamarin.Mac, проще всего создать один класс для управления всеми персональные настройки пользователя и предоставить к нему во всей системе.

Во-первых, добавьте новый `AppPreferences` класса в проект, а также наследуется от `NSObject`. Предпочтения будет разработан для использования [привязки данных и ключ-значение кодирования](~/mac/app-fundamentals/databinding.md) что сделает процесс создания и обслуживания предпочтение forms гораздо проще. С момента установки будет состоять из небольшое количество простых типов данных, используйте встроенный `NSUserDefaults` для хранения и извлечения значений.

Изменить `AppPreferences.cs` файла и сделать его выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    [Register("AppPreferences")]
    public class AppPreferences : NSObject
    {
        #region Computed Properties
        [Export("DefaultLangauge")]
        public int DefaultLanguage {
            get { 
                var value = LoadInt ("DefaultLangauge", 0);
                return value; 
            }
            set {
                WillChangeValue ("DefaultLangauge");
                SaveInt ("DefaultLangauge", value, true);
                DidChangeValue ("DefaultLangauge");
            }
        }

        [Export("SmartLinks")]
        public bool SmartLinks {
            get { return LoadBool ("SmartLinks", true); }
            set {
                WillChangeValue ("SmartLinks");
                SaveBool ("SmartLinks", value, true);
                DidChangeValue ("SmartLinks");
            }
        }

        // Define any other required user preferences in the same fashion
        ...

        [Export("EditorBackgroundColor")]
        public NSColor EditorBackgroundColor {
            get { return LoadColor("EditorBackgroundColor", NSColor.White); }
            set {
                WillChangeValue ("EditorBackgroundColor");
                SaveColor ("EditorBackgroundColor", value, true);
                DidChangeValue ("EditorBackgroundColor");
            }
        }
        #endregion

        #region Constructors
        public AppPreferences ()
        {
        }
        #endregion

        #region Public Methods
        public int LoadInt(string key, int defaultValue) {
            // Attempt to read int
            var number = NSUserDefaults.StandardUserDefaults.IntForKey(key);

            // Take action based on value
            if (number == null) {
                return defaultValue;
            } else {
                return (int)number;
            }
        }
            
        public void SaveInt(string key, int value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetInt(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public bool LoadBool(string key, bool defaultValue) {
            // Attempt to read int
            var value = NSUserDefaults.StandardUserDefaults.BoolForKey(key);

            // Take action based on value
            if (value == null) {
                return defaultValue;
            } else {
                return value;
            }
        }

        public void SaveBool(string key, bool value, bool sync) {
            NSUserDefaults.StandardUserDefaults.SetBool(value, key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }

        public string NSColorToHexString(NSColor color, bool withAlpha) {
            //Break color into pieces
            nfloat red=0, green=0, blue=0, alpha=0;
            color.GetRgba (out red, out green, out blue, out alpha);

            // Adjust to byte
            alpha *= 255;
            red *= 255;
            green *= 255;
            blue *= 255;

            //With the alpha value?
            if (withAlpha) {
                return String.Format ("#{0:X2}{1:X2}{2:X2}{3:X2}", (int)alpha, (int)red, (int)green, (int)blue);
            } else {
                return String.Format ("#{0:X2}{1:X2}{2:X2}", (int)red, (int)green, (int)blue);
            }
        }

        public NSColor NSColorFromHexString (string hexValue)
        {
            var colorString = hexValue.Replace ("#", "");
            float red, green, blue, alpha;

            // Convert color based on length
            switch (colorString.Length) {
            case 3 : // #RGB
                red = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(0, 1)), 16) / 255f;
                green = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(1, 1)), 16) / 255f;
                blue = Convert.ToInt32(string.Format("{0}{0}", colorString.Substring(2, 1)), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 6 : // #RRGGBB
                red = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, 1.0f);
            case 8 : // #AARRGGBB
                alpha = Convert.ToInt32(colorString.Substring(0, 2), 16) / 255f;
                red = Convert.ToInt32(colorString.Substring(2, 2), 16) / 255f;
                green = Convert.ToInt32(colorString.Substring(4, 2), 16) / 255f;
                blue = Convert.ToInt32(colorString.Substring(6, 2), 16) / 255f;
                return NSColor.FromRgba(red, green, blue, alpha);
            default :
                throw new ArgumentOutOfRangeException(string.Format("Invalid color value '{0}'. It should be a hex value of the form #RBG, #RRGGBB or #AARRGGBB", hexValue));
            }
        }

        public NSColor LoadColor(string key, NSColor defaultValue) {

            // Attempt to read color
            var hex = NSUserDefaults.StandardUserDefaults.StringForKey(key);

            // Take action based on value
            if (hex == null) {
                return defaultValue;
            } else {
                return NSColorFromHexString (hex);
            }
        }

        public void SaveColor(string key, NSColor color, bool sync) {
            // Save to default
            NSUserDefaults.StandardUserDefaults.SetString(NSColorToHexString(color,true), key);
            if (sync) NSUserDefaults.StandardUserDefaults.Synchronize ();
        }
        #endregion
    }
}
```

Этот класс содержит несколько вспомогательные процедуры, такие как `SaveInt`, `LoadInt`, `SaveColor`, `LoadColor`, т. д., чтобы сделать работу с `NSUserDefaults` проще. Кроме того, поскольку `NSUserDefaults` не имеет встроенных способ обработки `NSColors`, `NSColorToHexString` и `NSColorFromHexString` методы используются для преобразования цветов в веб-шестнадцатеричных строк (`#RRGGBBAA` где `AA` имеет альфа-прозрачности), может быть легко хранимых и извлечь.

В `AppDelegate.cs` файл, создать экземпляр **AppPreferences** объекта, который будет использоваться на уровне приложения:

```csharp
using AppKit;
using Foundation;
using System.IO;
using System;

namespace SourceWriter
{
    [Register ("AppDelegate")]
    public class AppDelegate : NSApplicationDelegate
    {
        #region Computed Properties
        public int NewWindowNumber { get; set;} = -1;

        public AppPreferences Preferences { get; set; } = new AppPreferences();
        #endregion

        #region Constructors
        public AppDelegate ()
        {
        }
        #endregion
        
        ...
```

<a name="Wiring-Preferences-to-Preference-Views" />

### <a name="wiring-preferences-to-preference-views"></a>Персональные настройки привязки для настройки представления

Затем подключитесь предпочтений класс для элементов пользовательского интерфейса для окна предпочтений и представления, созданные выше. В построителе интерфейса выбрать контроллер представление предпочтений и переключитесь в **инспектора удостоверения**, создайте пользовательский класс для контроллера: 

[ ![](dialog-images/prefs12.png "Инспектор удостоверений")](dialog-images/prefs12.png)

Переключитесь в Visual Studio для Mac для синхронизации изменений и откройте только что созданный класс для редактирования. Сделайте класс выглядеть следующим образом:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class EditorPrefsController : NSViewController
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        [Export("Preferences")]
        public AppPreferences Preferences {
            get { return App.Preferences; }
        }
        #endregion

        #region Constructors
        public EditorPrefsController (IntPtr handle) : base (handle)
        {
        }
        #endregion
    }
}
```

Обратите внимание, что этот класс выполнит здесь две вещи: во-первых, это вспомогательный объект `App` свойство доступ к **AppDelegate** проще. Во-вторых, `Preferences` свойство предоставляет глобальный **AppPreferences** класса для привязки данных с элементами интерфейса разместить на это представление.

Затем дважды щелкните файл раскадровки, чтобы снова открыть его в интерфейс построителя (и увидеть изменения, выполненные над). Перетащите все элементы управления пользовательского интерфейса, необходимые для построения настройки интерфейса в представлении. Для каждого элемента управления, переключитесь в **привязки инспектора** и привязать к отдельных свойств **AppPreference** класса:

[ ![](dialog-images/prefs13.png "Инспектор привязки")](dialog-images/prefs13.png)

Повторите описанные выше шаги для всех панелей (Просмотр контроллеров) и необходимые свойства предпочтения.

<a name="Applying-Preference-Changes-to-All-Open-Windows" />

### <a name="applying-preference-changes-to-all-open-windows"></a>Применение предпочтений изменения всех открытых окон

Как уже говорилось выше, в типичных macOS, приложения, когда пользователь вносит изменения в любые пользовательские настройки приложения, эти изменения сохраняются автоматически и применить к любой версии windows пользователь может быть открытых в приложении.

Этот процесс происходит плавный и прозрачно для конечного пользователя, с минимальным объемом кода рабочих позволит тщательного планирования и проектирования установки приложения и windows.

Для окна, в которое будут использовать параметры приложения, добавьте следующее свойство вспомогательный его содержимого View-Controller доступ к нашей **AppDelegate** проще:

```csharp
#region Application Access
public static AppDelegate App {
    get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
}
#endregion
```

Добавьте класс, чтобы настроить содержимое или поведения в зависимости от настроек пользователя.

```csharp
public void ConfigureEditor() {

    // General Preferences
    TextEditor.AutomaticLinkDetectionEnabled = App.Preferences.SmartLinks;
    TextEditor.AutomaticQuoteSubstitutionEnabled = App.Preferences.SmartQuotes;
    ...

}
``` 

Необходимо вызвать метод конфигурации при первом открытии окна для убедитесь, что его в соответствии с пользовательскими настройками:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // Configure editor from user preferences
    ConfigureEditor ();
    ...
}
```

Далее следует изменить `AppDelegate.cs` и добавьте следующий метод для применения любого предпочтений изменения всех открытых окон:

```csharp
public void UpdateWindowPreferences() {

    // Process all open windows
    for(int n=0; n<NSApplication.SharedApplication.Windows.Length; ++n) {
        var content = NSApplication.SharedApplication.Windows[n].ContentViewController as ViewController;
        if (content != null ) {
            // Reformat all text
            content.ConfigureEditor ();
        }
    }

}
```

Добавьте `PreferenceWindowDelegate` класса в проект и сделайте его выглядеть следующим образом:

```csharp
using System;
using AppKit;
using System.IO;
using Foundation;

namespace SourceWriter
{
    public class PreferenceWidowDelegate : NSWindowDelegate
    {
        #region Application Access
        public static AppDelegate App {
            get { return (AppDelegate)NSApplication.SharedApplication.Delegate; }
        }
        #endregion

        #region Computed Properties
        public NSWindow Window { get; set;}
        #endregion

        #region constructors
        public PreferenceWidowDelegate (NSWindow window)
        {
            // Initialize
            this.Window = window;

        }
        #endregion

        #region Override Methods
        public override bool WindowShouldClose (Foundation.NSObject sender)
        {
            
            // Apply any changes to open windows
            App.UpdateWindowPreferences();

            return true;
        }
        #endregion
    }
}
```

В результате все изменения предпочтений отправку всех открытых окон при закрытии окна предпочтения.

Наконец измените контроллера окна предпочтений и добавьте созданный ранее делегат:

```csharp
using System;
using Foundation;
using AppKit;

namespace SourceWriter
{
    public partial class PreferenceWindowController : NSWindowController
    {
        #region Constructors
        public PreferenceWindowController (IntPtr handle) : base (handle)
        {
        }
        #endregion

        #region Override Methods
        public override void WindowDidLoad ()
        {
            base.WindowDidLoad ();

            // Initialize
            Window.Delegate = new PreferenceWidowDelegate(Window);
            Toolbar.SelectedItemIdentifier = "General";
        }
        #endregion
    }
}
```

Все эти изменения в месте Если пользователь изменяет параметры приложения и закрывает окно предпочтения изменения повлияют на все открытые окна:

[ ![](dialog-images/prefs14.png "Пример настройки окна")](dialog-images/prefs14.png)

<a name="The_Open_Dialog" />

## <a name="the-open-dialog"></a>Откройте диалоговое окно

Откройте диалоговое окно предоставляет пользователям согласованный способ найти и открыть объект в приложение. Чтобы отобразить диалоговое окно открытия в приложении Xamarin.Mac, используйте следующий код:

```csharp
var dlg = NSOpenPanel.OpenPanel;
dlg.CanChooseFiles = true;
dlg.CanChooseDirectories = false;
dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

if (dlg.RunModal () == 1) {
    // Nab the first file
    var url = dlg.Urls [0];

    if (url != null) {
        var path = url.Path;

        // Create a new window to hold the text
        var newWindowController = new MainWindowController ();
        newWindowController.Window.MakeKeyAndOrderFront (this);

        // Load the text into the window
        var window = newWindowController.Window as MainWindow;
        window.Text = File.ReadAllText(path);
        window.SetTitleWithRepresentedFilename (Path.GetFileName(path));
        window.RepresentedUrl = url;

    }
}
```

В приведенном выше коде открывается новое окно документа для отображения содержимого файла. Вам потребуется заменить этот код с функциональные возможности, необходимые для приложения.

Следующие свойства доступны при работе с `NSOpenPanel`:

- **CanChooseFiles** — Если `true` пользователь может выбрать файлы.
- **CanChooseDirectories** — Если `true` пользователь может выбрать каталоги.
- **AllowsMultipleSelection** — Если `true` пользователь может выбрать более одного файла за раз.
- **ResolveAliases** — Если `true` выбора и псевдоним, сопоставляется с путь исходного файла.
- **AllowedFileTypes** -является массивом строк типов файлов, которые пользователь может выбрать в качестве либо расширения или _UTI_. Значение по умолчанию — `null`, что позволяет открыть любой файл.

`RunModal ()` Метод отображает диалоговое окно открытия и разрешить пользователю выбирать файлы и каталоги (как указано в свойствах) и возвращает `1` при нажатии кнопки **откройте** кнопки.

Откройте диалоговое окно возвращает выбранные файлы или каталоги пользователя как массив URL-адреса в `URL` свойство.

Если запустить программу и выберите **открыть...**  элемента из **файл** отображается меню, следующие: 

[ ![](dialog-images/dialog03.png "Открытое окно")](dialog-images/dialog03.png)

<a name="The_Print_and_Page_Setup_Dialogs" />

## <a name="the-print-and-page-setup-dialogs"></a>Печать и диалоговых окнах параметры страницы

macOS предоставляет стандартный печати и страницы установки диалоговые окна, отображающие приложения так, чтобы пользователи могли иметь согласованные печати возможности во всех приложениях, которые они используют.

Следующий код отображается стандартное диалоговое окно печати:

```csharp
public bool ShowPrintAsSheet { get; set;} = true;
...

[Export ("showPrinter:")]
void ShowDocument (NSObject sender) {
    var dlg = new NSPrintPanel();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet(new NSPrintInfo(),this,this,null,new IntPtr());
    } else {
        if (dlg.RunModalWithPrintInfo(new NSPrintInfo()) == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}

```

Если задается `ShowPrintAsSheet` свойства `false`, запустите приложение и отобразить диалоговое окно печати, отображается следующее:

[ ![](dialog-images/print01.png "Диалоговое окно печати")](dialog-images/print01.png)

Если задать `ShowPrintAsSheet` свойства `true`, запустите приложение и отобразить диалоговое окно печати, отображается следующее:

[ ![](dialog-images/print02.png "Печать листа")](dialog-images/print02.png)

Следующий код отображает диалоговое окно Макет страницы:

```csharp
[Export ("showLayout:")]
void ShowLayout (NSObject sender) {
    var dlg = new NSPageLayout();

    // Display the print dialog as dialog box
    if (ShowPrintAsSheet) {
        dlg.BeginSheet (new NSPrintInfo (), this);
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to print the document here...",
                MessageText = "Print Document",
            };
            alert.RunModal ();
        }
    }
}
```

Если задается `ShowPrintAsSheet` свойства `false`, запустите приложение и отобразить диалоговое окно печати макета, отображается следующее:

[ ![](dialog-images/print03.png "Диалоговое окно настройки страницы")](dialog-images/print03.png)

Если задать `ShowPrintAsSheet` свойства `true`, запустите приложение и отобразить диалоговое окно печати макета, отображается следующее:

[ ![](dialog-images/print04.png "Лист страница установки")](dialog-images/print04.png)

Дополнительные сведения о работе с печати и страницы диалоговые окна программы установки см. в разделе Apple [NSPrintPanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPrintPanel_Class/index.html#//apple_ref/doc/uid/TP40004092), [NSPageLayout](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSPageLayout_Class/index.html#//apple_ref/doc/uid/TP40004080) и [Общие сведения о печати](http://sdg.mesonet.org/people/brad/XCode3/Documentation/DocSets/com.apple.adc.documentation.AppleSnowLeopard.CoreReference.docset/Contents/Resources/Documents/#documentation/Cocoa/Conceptual/Printing/Printing.html#//apple_ref/doc/uid/10000083-SW1) документация.

<a name="The_Save_Dialog" />

## <a name="the-save-dialog"></a>Сохранить диалоговое окно

Диалоговое окно сохранения предоставляет пользователям согласованный способ сохранения элемента в приложении.

Отображается стандартное диалоговое окно Сохранить следующий код:

```csharp
public bool ShowSaveAsSheet { get; set;} = true;
...

[Export("saveDocumentAs:")]
void ShowSaveAs (NSObject sender)
{
    var dlg = new NSSavePanel ();
    dlg.Title = "Save Text File";
    dlg.AllowedFileTypes = new string[] { "txt", "html", "md", "css" };

    if (ShowSaveAsSheet) {
        dlg.BeginSheet(mainWindowController.Window,(result) => {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        });
    } else {
        if (dlg.RunModal () == 1) {
            var alert = new NSAlert () {
                AlertStyle = NSAlertStyle.Critical,
                InformativeText = "We need to save the document here...",
                MessageText = "Save Document",
            };
            alert.RunModal ();
        }
    }

}
```

`AllowedFileTypes` Свойство имеет строковый массив типов файлов, которые пользователь может выбрать, чтобы сохранить файл как. Тип файла либо задан как расширение или _UTI_. Значение по умолчанию — `null`, что позволяет любой тип файла, который будет использоваться.

Если задается `ShowSaveAsSheet` свойства `false`, запустите приложение и выберите **Сохранить как...**  из **файл** меню будет отображаться следующее:

[ ![](dialog-images/save01.png "Сохранение диалоговое окно")](dialog-images/save01.png)

Пользователь может развернуть диалоговое окно:

[ ![](dialog-images/save02.png "Развернутое диалоговое окно сохранения")](dialog-images/save02.png)

Если задается `ShowSaveAsSheet` свойства `true`, запустите приложение и выберите **Сохранить как...**  из **файл** меню будет отображаться следующее:

[ ![](dialog-images/save03.png "Сохранение таблицы")](dialog-images/save03.png)

Пользователь может развернуть диалоговое окно:

[ ![](dialog-images/save04.png "Расширенная сохранить лист")](dialog-images/save04.png)

Для получения дополнительных сведения о работе с диалоговым окном сохранить см. в разделе Apple [NSSavePanel](https://developer.apple.com/library/mac/documentation/Cocoa/Reference/ApplicationKit/Classes/NSSavePanel_Class/index.html#//apple_ref/doc/uid/TP40004098) документации.

<a name="Summary" />

## <a name="summary"></a>Сводка

В этой статье были предприняты никакие подробное описание работы с модальных окон, листов и диалоговые окна в приложении Xamarin.Mac стандартной системы. Мы узнали различных типов и использования модальных окон, листов и диалоговых окнах, листы, как создавать и поддерживать модальные окна и вкладки в Xcode в интерфейс построителя и способы работы с модальных окон, диалоговых окон в коде C#.

## <a name="related-links"></a>Связанные ссылки

- [MacWindows (пример)](https://developer.xamarin.com/samples/mac/MacWindows/)
- [Привет, Mac](~/mac/get-started/hello-mac.md)
- [Меню](~/mac/user-interface/menu.md)
- [Windows](~/mac/user-interface/window.md)
- [Панели инструментов](~/mac/user-interface/toolbar.md)
- [Рекомендации по интерфейсу отдела OS X](https://developer.apple.com/library/mac/documentation/UserExperience/Conceptual/OSXHIGuidelines/)
- [Введение в Windows](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/WinPanel/Introduction.html#//apple_ref/doc/uid/10000031-SW1)
- [Введение в листы](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Sheets/Sheets.html#//apple_ref/doc/uid/10000002i)
- [Общие сведения о печати](https://developer.apple.com/library/mac/documentation/Cocoa/Conceptual/Printing/osxp_aboutprinting/osxp_aboutprt.html)
