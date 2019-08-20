---
title: Создание объектов пользовательского интерфейса в Xamarin. iOS
description: В этом документе содержатся общие сведения о создании пользовательского интерфейса в Xamarin. iOS. В нем обсуждается конструктор iOS, Xcode Interface Builder, C#и раскадровки.
ms.prod: xamarin
ms.assetid: 4D6B136C-744A-4936-8655-A77E62BA7A60
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: cfbd722726357b0adc9fa14ce2c9a13f8de1bb87
ms.sourcegitcommit: 0df727caf941f1fa0aca680ec871bfe7a9089e7c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/19/2019
ms.locfileid: "69620925"
---
# <a name="creating-user-interface-objects-in-xamarinios"></a>Создание объектов пользовательского интерфейса в Xamarin. iOS

Apple группирует связанные элементы функциональности в "frameworks", которые соответствуют пространствам имен Xamarin. iOS. `UIKit`пространство имен, содержащее все элементы управления пользовательского интерфейса для iOS.

Каждый раз, когда код должен ссылаться на элемент управления пользовательского интерфейса, например метку или кнопку, не забудьте включить следующую инструкцию using:

```csharp
using UIKit;
```

Все элементы управления, обсуждаемые в этой главе, находятся в пространстве имен UIKit, а каждое имя класса пользовательского `UI` элемента управления имеет префикс.

Элементы управления и макеты пользовательского интерфейса можно редактировать тремя способами:

- **[Xamarin iOS Designer](~/ios/user-interface/designer/index.md)** — использование встроенного конструктора макетов Xamarin для разработки экранов. Дважды щелкните файл Storyboard или XIB, чтобы изменить его с помощью встроенного конструктора.
- **Xcode Interface Builder** — перетащите элементы управления на макеты экрана с помощью Interface Builder. Откройте файл Storyboard или XIB в Xcode, щелкнув правой кнопкой мыши файл в **панель решения** и выбрав пункт **открыть с помощью > Xcode Interface Builder**.
- **Using C#**  — элементы управления можно также создавать программно с помощью кода и добавлять в иерархию представлений.

Новые файлы Storyboard и XIB можно добавить, щелкнув правой кнопкой мыши проект iOS и выбрав **добавить > новый файл...** .

Независимо от используемого метода, Управление свойствами и событиями может осуществляться C# в логике приложения.

## <a name="using-xamarin-ios-designer"></a>Использование Xamarin iOS Designer

Чтобы приступить к созданию пользовательского интерфейса в конструкторе iOS, дважды щелкните файл раскадровки. Элементы управления можно перетащить в область конструктора из **панели элементов** , как показано ниже:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

 [![](creating-ui-objects-images/image2b.png "Панель элементов")](creating-ui-objects-images/image2b.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 [![](creating-ui-objects-images/image2b-vs.png "Панель элементов — визуальный Стуио")](creating-ui-objects-images/image2b.png#lightbox)
 
-----

При выборе элемента управления в области конструктора **панель свойств** покажет атрибуты для этого элемента управления. Поле **имени > удостоверений мини** -приложения >, которое заполняется на снимке экрана ниже, используется в качестве имени розетки. Вот как можно ссылаться на элемент управления в C#:

 [![](creating-ui-objects-images/image3b.png "Панель мини-приложений свойств")](creating-ui-objects-images/image3b.png#lightbox)

Более подробные сведения об использовании конструктора iOS см. в руководстве введение в [конструктор iOS](~/ios/user-interface/designer/introduction.md) .

## <a name="using-xcode-interface-builder"></a>Использование Xcode Interface Builder

Если вы не знакомы с использованием Interface Builder, см. документацию по [Interface Builder](https://developer.apple.com/xcode/interface-builder/) Apple.

Чтобы открыть раскадровку в Xcode, щелкните правой кнопкой мыши для доступа к контекстному меню файла раскадровки и выберите команду Открыть с помощью **Xcode Interface Builder**:

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

 [![](creating-ui-objects-images/imagexcode.png "Контекстное меню раскадровки — Xcode")](creating-ui-objects-images/imagexcode.png#lightbox)
 
# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

[![](creating-ui-objects-images/imagexcode-vs.png "Контекстное меню раскадровки — Xcode")](creating-ui-objects-images/imagexcode-vs.png#lightbox)

-----

Элементы управления можно перетаскивать на область конструктора из **библиотеки объектов** , как показано ниже:

 [![](creating-ui-objects-images/image5a.png "Библиотека объектов Xcode")](creating-ui-objects-images/image5a.png#lightbox)

При проектировании пользовательского интерфейса с Interface Builder необходимо создать **выход** для каждого элемента управления, на который вы хотите сослаться C#. Это можно сделать, включив **Редактор помощника** с помощью кнопки Center **Editor** (Центральная панель) на панели инструментов Xcode:

 [![](creating-ui-objects-images/image6a.png "Кнопка редактора помощника")](creating-ui-objects-images/image6a.png#lightbox)

Щелкните объект пользовательского интерфейса; затем **перетащите элемент управления** в h файл. Чтобы **управлять перетаскиванием**, удерживая нажатой клавишу CTRL, щелкните и удерживайте за объектом пользовательского интерфейса, для которого создается выход (или действие). Удерживайте нажатой клавишу Control при перетаскивании в файл заголовка. Завершите перетаскивание `@interface` под определением. На рисунке ниже показана синяя линия с подписью вставить розетку или выход из коллекции.

Когда вы отпустите кнопку, появится запрос на ввод имени розетки, которая будет использоваться для создания C# свойства, на которое можно ссылаться в коде:

 [![](creating-ui-objects-images/image8a.png "Создание розетки")](creating-ui-objects-images/image8a.png#lightbox)

Дополнительные сведения о том, как Interface Builder Xcode интегрируется с Visual Studio для Mac, см. в документе о [создании кода XIB](~/ios/internals/xib-code-generation.md#generated) .

## <a name="using-c"></a>Использование C\#

Если вы решили создать объект пользовательского интерфейса с помощью C# (например, в представлении или контроллере представления), выполните следующие действия.

- Объявите поле уровня класса для объекта пользовательского интерфейса. Создайте элемент управления один раз, в `ViewDidLoad` , например. Затем на объект можно ссылаться во всех методах жизненного цикла контроллера представления (например,
`ViewWillAppear`).
- Создайте объект `CGRect` , определяющий фрейм элемента управления (его координаты X и Y на экране, а также ширину и высоту). Необходимо убедиться, что у вас есть `using CoreGraphics` директива для этого.
- Вызовите конструктор, чтобы создать и назначить элемент управления.
- Задайте любые свойства или обработчики событий.
- Вызовите `Add()` , чтобы добавить элемент управления в иерархию представления.

Ниже приведен простой пример создания `UILabel` в контроллере представления с помощью: C#

```csharp
UILabel label1;
public override void ViewDidLoad () {
    base.ViewDidLoad ();
    var frame = new CGRect(10, 10, 300, 30);
    label1 = new UILabel(frame);
    label1.Text = "New Label";
    View.Add (label1);
}
```

<a name="partial_classes" />

## <a name="using-c-and-storyboards"></a>Использование C# и раскадровки

При добавлении контроллеров представления в область конструктора в проекте создаются два C# соответствующих файла. В этом примере `ControlsViewController.cs` и `ControlsViewController.designer.cs` были созданы автоматически:

 [![](creating-ui-objects-images/image9b.png "Разделяемый класс ViewController")](creating-ui-objects-images/image9b.png#lightbox)

Файл предназначен для *вашего кода.* `MainViewController.cs` Именно здесь реализуются методы `ViewDidLoad` `ViewWillAppear` жизненного цикла, такие как и, и где можно добавлять собственные свойства, поля `View` и методы.

`ControlsViewController.designer.cs` Создается код, содержащий разделяемый класс. Когда вы назначите имя элемента управления в области конструктора в Visual Studio для Mac или создаете розетку или действие в Xcode, соответствующее свойство или разделяемый метод добавляется в файл конструктора (designer.cs). В приведенном ниже коде показан пример кода, созданного для двух кнопок и текстового представления, где одна из кнопок также имеет `TouchUpInside` событие.

Эти элементы разделяемого класса позволяют коду ссылаться на элементы управления и реагировать на действия, объявленные в области конструктора:

```csharp
[Register ("ControlsViewController")]
    partial class ControlsViewController
    {
        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button1 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UIButton Button2 { get; set; }

        [Outlet]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        UIKit.UITextField textfield1 { get; set; }

        [Action ("button2_TouchUpInside:")]
        [GeneratedCodeAttribute ("iOS Designer", "1.0")]
        partial void button2_TouchUpInside (UIButton sender);

        void ReleaseDesignerOutlets ()
        {
            if (Button1 != null) {
                Button1.Dispose ();
                Button1 = null;
            }
            if (Button2 != null) {
                Button2.Dispose ();
                Button2 = null;
            }
            if (textfield1 != null) {
                textfield1.Dispose ();
                textfield1 = null;
            }
        }
    }
}
```

`designer.cs` Файл не следует изменять вручную — интегрированная среда разработки (Visual Studio для Mac или Visual Studio) отвечает за синхронизацию с раскадровкой.

Когда объекты пользовательского интерфейса добавляются программно в `View` или `ViewController`, вы самостоятельно создаете ссылки на объекты и управляете ими, поэтому файл конструктора не требуется.



## <a name="related-links"></a>Связанные ссылки

- [Элементы управления (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/controls)
