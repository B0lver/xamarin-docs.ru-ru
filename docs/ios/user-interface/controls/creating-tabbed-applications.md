---
title: Панели вкладки панели вкладок и контроллеры в Xamarin.iOS
description: В этом документе описываются iOS вкладку панели контроллеров и способы их использования с Xamarin.iOS. Показывается, как настроить UITabBarController, работать с изображениями, задайте значения эмблемы, работа с событиями и многое другое.
ms.prod: xamarin
ms.assetid: 7C772899-2900-F139-D642-F3C4F3F14DDC
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/21/2017
ms.openlocfilehash: ad4682e9a3d4de2565bee54ffa159fd739572e24
ms.sourcegitcommit: d8af612b6b3218fea396d2f180e92071c4d4bf92
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/04/2020
ms.locfileid: "75663356"
---
# <a name="tab-bars-and-tab-bar-controllers-in-xamarinios"></a>Панели вкладок и контроллеры панели вкладок в Xamarin. iOS

В iOS используются с вкладками приложения для поддержки пользовательских интерфейсов, где может быть доступно несколько экранов в произвольном порядке. Через `UITabBarController` класс, приложения могут легко включать поддержку таких сценариев с несколькими экранами. `UITabBarController` берет на себя управление несколькими экранами, позволяя разработчику сосредоточиться на деталях каждого экрана приложения.

Как правило, построение приложений с вкладками `UITabBarController` , `RootViewController` главного окна. Тем не менее применив немного дополнительного кода, с вкладками приложения также можно последовательно, чтобы некоторые другие начальный экран, подобная ситуация, где приложение сначала появится экран входа, следуют интерфейс с вкладками.

На этой странице обсуждаются оба сценария: Если вкладки находятся в корне иерархии представления приложения, а также в сценарии, не`RootViewController`.

## <a name="introducing-uitabbarcontroller"></a>Знакомство с UITabBarController

`UITabBarController` Поддерживает с вкладками разработки приложений, следующие:

- Разрешение нескольких контроллеров для добавления к нему.
- Предоставление с вкладками пользовательского интерфейса, с помощью `UITabBar` класса, чтобы разрешить пользователю переключаться между контроллерами и их представления. 

Контроллеры добавляются к `UITabBarController` через его `ViewControllers` свойство, являющееся `UIViewController` массива. `UITabBarController` Сам обрабатывает загрузке правильной контроллера и представляющим его представление, в зависимости от выбранной вкладки.

Вкладки являются экземплярами `UITabBarItem` класс, который содержатся в `UITabBar` экземпляра. Каждый `UITabBar` экземпляра можно получить с помощью `TabBarItem` свойства контроллера на каждой вкладке.

Чтобы понять, как работать с `UITabBarController`, давайте подробнее рассмотрим действия по созданию простого приложения, которое использует один.

## <a name="tabbed-application-walkthrough"></a>Пошаговое руководство по приложению с вкладками

В этом пошаговом руководстве мы создадим следующие приложения:

[![пример приложения с вкладками](creating-tabbed-applications-images/00-app.png)](creating-tabbed-applications-images/00-app.png#lightbox)

Хотя в Visual Studio для Mac уже есть шаблон приложения с вкладками, в этом примере эти инструкции работают из пустого проекта, чтобы лучше понять, как создается приложение.

### <a name="creating-the-application"></a>Создание приложения

Начните с создания нового приложения.

Выберите **файл > Создать > решение** пункта меню в Visual Studio для Mac и выберите **iOS > приложение > пустой проект** шаблон, имя проекта `TabbedApplication`, как показано ниже:

[![](creating-tabbed-applications-images/newsolution1.png "Select the Empty Project template")](creating-tabbed-applications-images/newsolution1.png#lightbox)

[![](creating-tabbed-applications-images/newsolution2.png "Name the project TabbedApplication")](creating-tabbed-applications-images/newsolution2.png#lightbox)

### <a name="adding-the-uitabbarcontroller"></a>Добавление UITabBarController

Добавьте пустой класс, выбрав **файл > новый файл** и выбрав **общие: пустой класс** шаблона. Назовите файл `TabController` как показано ниже:

[![](creating-tabbed-applications-images/02-newclass.png "Add the TabController class")](creating-tabbed-applications-images/02-newclass.png#lightbox)

`TabController` Класс будет содержать реализацию `UITabBarController` , будут управлять массив `UIViewControllers`. Когда пользователь выбирает вкладку, `UITabBarController` позаботится о представление, соответствующее представление контроллера представления.

Для реализации `UITabBarController` нам нужно сделать следующее:

1. Задать базовый класс `TabController` для `UITabBarController` . 
1. Создание `UIViewController` экземпляры, чтобы добавить `TabController` . 
1. Добавить `UIViewController` экземпляров в массив, назначенный `ViewControllers` свойство `TabController` . 

Добавьте следующий код, чтобы `TabController` класс для достижения следующие действия:

```csharp
using System;
using UIKit;

namespace TabbedApplication {
    public class TabController : UITabBarController {

        UIViewController tab1, tab2, tab3;

        public TabController ()
        {
            tab1 = new UIViewController();
            tab1.Title = "Green";
            tab1.View.BackgroundColor = UIColor.Green;

            tab2 = new UIViewController();
            tab2.Title = "Orange";
            tab2.View.BackgroundColor = UIColor.Orange;

            tab3 = new UIViewController();
            tab3.Title = "Red";
            tab3.View.BackgroundColor = UIColor.Red;

            var tabs = new UIViewController[] {
                tab1, tab2, tab3
            };

            ViewControllers = tabs;
        }
    }
}
```

Обратите внимание, что для каждого `UIViewController` набор экземпляров, мы `Title` свойство `UIViewController`. При добавлении контроллера `UITabBarController`, `UITabBarController` будет считывать `Title` для каждого контроллера и отобразить ее на метку соответствующей вкладки, как показано ниже:

[![](creating-tabbed-applications-images/00-app.png "The sample app run")](creating-tabbed-applications-images/00-app.png#lightbox)

#### <a name="setting-the-tabcontroller-as-the-rootviewcontroller"></a>Установка TabController RootViewController

Порядок, контроллеров находятся на вкладках соответствует порядку, они добавляются `ViewControllers` массива.

Чтобы получить `UITabController` для загрузки в качестве первого экрана, нам нужно сделать его окна `RootViewController`, как показано в следующем коде для `AppDelegate`:

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate : UIApplicationDelegate
{
    UIWindow window;
    TabController tabController;
    
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        window = new UIWindow (UIScreen.MainScreen.Bounds);
        
        tabController = new TabController ();
        window.RootViewController = tabController;
        
        window.MakeKeyAndVisible ();
        
        return true;
    }
}
```

Если запустить приложение сейчас, `UITabBarController` будет загружаться с выбранной вкладкой «первый» по умолчанию. При выборе любой из других вкладок в связанного контроллера представление результатов представленных `UITabBarController,` как показано ниже, где конечный пользователь выбрал второй вкладке:

![Вторая показанная вкладка](creating-tabbed-applications-images/03-secondtab-sml.png)

### <a name="modifying-tabbaritems"></a>Изменение TabBarItems

Теперь, когда у нас есть промежуточный вкладке приложения, давайте изменим `TabBarItem` изменение изображения и текст, отображаемый, а также чтобы добавить эмблему на одной из вкладок.

#### <a name="setting-a-system-item"></a>Задание системного элемента

Во-первых давайте установим первую вкладку, чтобы использовать элемент "система". В конструкторе класса `TabController`, удалите строку, которая задает контроллера `Title` для `tab1` экземпляра и замените его следующим кодом, чтобы задать контроллер `TabBarItem` свойство:

```csharp
tab1.TabBarItem = new UITabBarItem (UITabBarSystemItem.Favorites, 0);
```

При создании `UITabBarItem` с помощью `UITabBarSystemItem`, заголовок и изображения предоставляются автоматически, iOS, как показано на снимке экрана ниже отображение **"Избранное"** значок и заголовок на первой вкладке:

 ![Первая вкладка со значком звездочки](creating-tabbed-applications-images/04a-tabimage-sml.png)

#### <a name="setting-the-image"></a>Настройка образа

Помимо использования элемент "система", заголовок и изображение `UITabBarItem` можно задать пользовательские значения. Например, измените код, который задает `TabBarItem` свойство с именем контроллера `tab2` следующим образом:

```csharp
tab2 = new UIViewController ();
tab2.TabBarItem = new UITabBarItem ();
tab2.TabBarItem.Image = UIImage.FromFile ("second.png");
tab2.TabBarItem.Title = "Second";
tab2.View.BackgroundColor = UIColor.Orange;
```

В приведенном выше коде предполагается, что в корневую папку проекта (или в каталоге **ресурсов** ) добавлен образ с именем `second.png`. Для поддержки всех плотий экрана требуются три изображения, как показано ниже:

![Изображения, добавленные в проект](creating-tabbed-applications-images/tabbedimages7new.png)

Инструкции по правильным измерениям см. в разделе **Размер значка панели вкладок** на [странице пользовательских значков Apple](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/) . Рекомендуемый размер зависит от стиля изображения (круглая, квадратная, широкая или высота).

Свойству `Image` необходимо задать только **второе имя PNG** , IOS автоматически загрузит файлы с более высоким разрешением при необходимости. Дополнительные сведения см. в [работа с образами](~/ios/app-fundamentals/images-icons/index.md) руководства. По умолчанию элементов этой панели, серый значок с синей оттенок, при выборе.

#### <a name="overriding-the-title"></a>Переопределение заголовка

Если свойство `Title` задано непосредственно в `TabBarItem`, оно переопределит любое значение, установленное для `Title` на самом контроллере.

На второй вкладке (средняя) на этом снимке экрана показан пользовательский заголовок и изображение:

![Вторая вкладка с квадратным значком](creating-tabbed-applications-images/05-customtab-sml.png)

#### <a name="setting-the-badge-value"></a>Задание значения эмблемы

Вкладки также можно отобразить индикатор событий. Например добавьте следующую строку кода, чтобы задать эмблему на третью вкладку:

```csharp
tab3.TabBarItem.BadgeValue = "Hi";
```

Под управлением, это приведет к красная метка со строки «Hi» в верхнем левом углу вкладки, как показано ниже:

![Вторая вкладка с эмблемой Hi](creating-tabbed-applications-images/06-badge-sml.png)

Эмблема часто используется для отображения чисел значение, указывающее непрочитанные, новые элементы. Чтобы удалить значок, задайте `BadgeValue` как null, как показано ниже:

```csharp
tab3.TabBarItem.BadgeValue = null;
```

## <a name="tabs-in-non-rootviewcontroller-scenarios"></a>Вкладки в сценариях, не Рутвиевконтроллер

В приведенном выше примере мы показали, как работать с `UITabBarController` где `RootViewController` окна. В этом примере мы рассмотрим, как использовать `UITabBarController` не `RootViewController` и показать, как это создается при помощи объектов Storyboard.

### <a name="initial-screen-example"></a>Пример начального экрана

В этом сценарии загружает начального экрана из контроллера, который не `UITabBarController`. Когда пользователь взаимодействует с экрана, коснувшись кнопки, один и тот же контроллер представления будут загружены в `UITabBarController`, который затем представляются пользователю. На следующем рисунке показан блок-схеме приложения:

[![](creating-tabbed-applications-images/inital-screen-application.png "This screenshot shows the application flow")](creating-tabbed-applications-images/inital-screen-application.png#lightbox)

Начнем с нового приложения в этом примере. Опять же, мы будем использовать **iPhone > приложение > пустой проект (C#)** шаблона, назвав проекта `InitialScreenDemo`.

В этом примере раскадровка используется для размещения контроллеров представления. Чтобы добавить раскадровку, сделайте следующее:

- Щелкните правой кнопкой мыши имя проекта и выберите **Добавить > новый файл**.

- В открывшемся диалоговом окне создания файла, перейдите к **iOS > Пустая раскадровка iPhone**.

Давайте назовем этот новая раскадровка **MainStoryboard** , как показано ниже: 

[![](creating-tabbed-applications-images/new-file-dialog.png "Add a MainStoryboard file to the project")](creating-tabbed-applications-images/new-file-dialog.png#lightbox)

Существует несколько важных шагов, чтобы Обратите внимание, при добавлении раскадровки в файл ранее раскадровки, как описано в [введение в раскадровки](~/ios/user-interface/storyboards/index.md) руководства. Эти особые значения приведены ниже.

1. Добавьте свое имя раскадровки, чтобы **главный интерфейс** раздел `Info.plist`:

    [![](creating-tabbed-applications-images/project-options.png "Set the Main Interface to MainStoryboard")](creating-tabbed-applications-images/project-options.png#lightbox)
1. В вашей `App Delegate`, переопределить метод окне следующим кодом:

    ```csharp
    public override UIWindow Window {
        get;
        set;
    }
    ```

Мы собираемся требуется три контроллера представления для этого примера. Один из них, именуемый `ViewController1`, будет использоваться как первый контроллер представления и на первой вкладке. Два других, именуемые `ViewController2` и `ViewController3`, которые будут использоваться на второй и третьей вкладках соответственно.

Откройте конструктор, дважды щелкнув файл MainStoryboard.storyboard и перетащите три контроллера представления в область конструктора. Мы хотим, каждый из этих контроллеров представлений для своего класса, соответствующий имени выше, в этом случае в разделе **удостоверений > класс**, введите его имя, как показано на следующем снимке экрана:

[![](creating-tabbed-applications-images/class-name.png "Set the Class to ViewController1")](creating-tabbed-applications-images/class-name.png#lightbox)

Visual Studio для Mac автоматически создает классы и необходимые файлы конструктора, это можно увидеть на панели решения, как показано ниже:

[![](creating-tabbed-applications-images/solution-pad2.png "Auto-generated files in the project")](creating-tabbed-applications-images/solution-pad2.png#lightbox)

#### <a name="creating-the-ui"></a>Создание пользовательского интерфейса

Далее мы создадим простой пользовательский интерфейс для каждого из представлений ViewController использовании конструктора iOS Xamarin.

Мы хотим перетащите `Label` и `Button` на ViewController1 из **элементов** правой стороны. Далее при помощи панели свойств для редактирования, именем и текстом элементов управления следующего:

- **Метка** : `Text`  =  **один**
- **Кнопка** : `Title`  =  **пользователь выполняет некоторое Начальное действие**

Мы Управление видимостью нашей кнопки в `TouchUpInside` событий и мы должны ссылаться на него в коде программной части. Давайте идентифицировать его с **имя** `aButton` в панели свойств, как показано на следующем снимке экрана:

[![](creating-tabbed-applications-images/abutton-properties.png "Set the Name to aButton in the Properties Pad")](creating-tabbed-applications-images/abutton-properties.png#lightbox)

Поверхность конструктора теперь должен выглядеть как на снимке экрана ниже:

[![](creating-tabbed-applications-images/design-surface1.png "Your Design Surface should now look similar to this screenshot")](creating-tabbed-applications-images/design-surface1.png#lightbox)

Давайте добавим немного более подробно в `ViewController2` и `ViewController3`, путем добавления метки к каждому и замену текста «Два» и «Три» соответственно. Это подчеркивает пользователь какие вкладки или представления, мы рассматриваем.

#### <a name="wiring-up-the-button"></a>Накладка кнопки

Мы собираемся загрузить `ViewController1` при первом запуске приложения. Когда пользователь нажимает кнопку, мы скрытия кнопки и загрузить `UITabBarController` с `ViewController1` экземпляра на первой вкладке.

Если пользователь отпускает `aButton`, мы хотим TouchUpInside события. Давайте выберем кнопки и в **вкладке "события"** панели свойств Объявите обработчик событий — `InitialActionCompleted` — поэтому его можно ссылаться в коде. Это показано на следующем снимке экрана:

[![](creating-tabbed-applications-images/event-handler.png "When the user releases the aButton, trigger a TouchUpInside event")](creating-tabbed-applications-images/event-handler.png#lightbox)

Теперь нам нужно сообщить контроллер представления, чтобы скрыть кнопки при возникновении события `InitialActionCompleted`. В `ViewController1`, добавьте следующий разделяемый метод:

```csharp
partial void InitialActionCompleted (UIButton sender)
{
    aButton.Hidden = true;  
}
```

Сохраните файл и запустить приложение. Мы должны увидеть отображается один экран и кнопку решаются при Touch вверх.

#### <a name="adding-the-tab-bar-controller"></a>Добавление контроллера панели вкладок

Теперь у нас есть нашего начального представления, работает должным образом. Далее, необходимо добавить его в `UITabBarController`, а также представления 2 и 3. Давайте откройте раскадровку, в конструкторе.

В **элементов**, поиск **контроллер панели вкладок** под контроллеры & объекты и перетащите в область конструктора. Как показано на следующем снимке экрана, контроллер панели вкладок является без интерфейса пользователя и таким образом, дает двумя контроллерами представлений с ним по умолчанию:

[![](creating-tabbed-applications-images/tabbarcontroller.png "Adding a Tab Bar Controller to the layout")](creating-tabbed-applications-images/tabbarcontroller.png#lightbox)

Удалите эти новые контроллеры представления, выбрав черную полосу в нижней и нажав клавишу delete.

В нашем раскадровке мы используем Segues обрабатывать переходы между TabBarController и наши контроллеров представлений. После взаимодействия с исходное представление, мы хотим загрузить их в TabBarController, представленные пользователю. Давайте настроим это в конструкторе.

**CTRL + щелчок** и **перетащите** с помощью кнопки для TabBarController. На доступ к мыши появится контекстное меню. Мы хотим использовать модального перехода. 

Для настройки каждой из наших вкладок **Ctrl + щелчок** из TabBarController для каждого из наших контроллеров представлений в порядке от одного до трех, а затем выберите связь **вкладке** в контекстном меню, как показано ниже:

[![](creating-tabbed-applications-images/context-menu.png "Select the Tab Relationship")](creating-tabbed-applications-images/context-menu.png#lightbox)

Раскадровка должна выглядеть так на снимке экрана ниже:

[![](creating-tabbed-applications-images/segue-layout.png "The Storyboard should resemble this screenshot")](creating-tabbed-applications-images/segue-layout.png#lightbox)

Если мы щелкните один из элементов этой панели и изучите панели «свойства», вы увидите несколько различных вариантов, как показано ниже:

[![](creating-tabbed-applications-images/properties-panel.png "Setting the tab options in the Properties Explorer")](creating-tabbed-applications-images/properties-panel.png#lightbox)

Мы можем использовать его для изменения определенных атрибутов, такие как эмблема, заголовок и iOS [идентификатор](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/UIKitUICatalog/TabBarItem.html), среди прочего

Если сохранить и запустить приложение сейчас, очень скоро найдется что кнопки появится снова при загрузке экземпляра ViewController1 в TabBarController. Исправим это путем проверки на наличие родительского контроллера представления для текущего представления. Если Да, мы знаем, мы внутри TabBarController, и поэтому должны быть скрыты кнопки. Давайте добавим приведенный ниже код к классу ViewController1:

```csharp
public override void ViewDidLoad ()
{
    if (ParentViewController != null){
        aButton.Hidden = true;
    }
}
```

Загружается после запуска приложения и пользователь нажимает кнопку на первом экране UITabBarController, с помощью представления на первом экране помещены на первой вкладке, как показано ниже:

[![выходных данных примера приложения](creating-tabbed-applications-images/first-view-sml.png)](creating-tabbed-applications-images/first-view.png#lightbox)

## <a name="summary"></a>Сводка

В этой статье описаны способы использования `UITabBarController` в приложении. Мы рассмотрели способ загрузки контроллеров в каждой вкладке, а также настройке свойств на вкладках такие названия, изображения и эмблемы. Мы затем проверяются, с помощью раскадровки, как загрузить `UITabBarController` во время выполнения, если она не `RootViewController` окна.

## <a name="related-links"></a>Связанные ссылки

- [Создание приложения с вкладками (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/creatingtabbedapplications)
- [Images.ZIP](https://github.com/xamarin/ios-samples/blob/master/CreatingTabbedApplications/Resources/images.zip?raw=true)
- [Ссылки на класс UITabBarController](https://developer.apple.com/library/ios/#documentation/uikit/reference/UITabBarController_Class/Reference/Reference.html)
