---
title: Подробный обзор примера приложения "Привет, iOS"
description: Этот документ более подробно описывает пример приложения "Привет, iOS", включая его архитектуру, пользовательский интерфейс, иерархию представлений содержимого, тестирование, развертывание и многое другое.
zone_pivot_groups: platform
ms.topic: quickstart
ms.prod: xamarin
ms.assetid: 61ba3a7e-fe11-4439-8bc8-9809512b8eff
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 10/05/2018
ms.openlocfilehash: 4b9043d70bb7460abf62c964da8041f345cd1be6
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91435608"
---
# <a name="hello-ios--deep-dive"></a>Подробный обзор примера приложения "Привет, iOS"

Краткое пошаговое руководство, описывающее создание и запуск простого приложения Xamarin.iOS. Пришло время подробнее изучить принципы работы приложений iOS, чтобы создавать более сложные программы. Это руководство описывает шаги, предпринятые в пошаговом руководстве "Привет, iOS", чтобы вы могли изучить основные принципы разработки приложений iOS.

Это руководство поможет вам выработать навыки и получить знания, необходимые для создания приложения iOS с одним экраном. После его прохождения вы будете понимать, из каких компонентов состоит приложение Xamarin.iOS и как они связаны друг с другом.

::: zone pivot="macos"

## <a name="introduction-to-visual-studio-for-mac"></a>Введение в Visual Studio для Mac

Visual Studio для Mac — это бесплатная интегрированная среда разработки (IDE) с открытым исходным кодом, объединяющая в себе функции Visual Studio и XCode. Она включает в себя полностью интегрированный визуальный конструктор, текстовый редактор с инструментами рефакторинга, обозреватель сборок, средства интеграции исходного кода и другие возможности. В этом руководстве описаны некоторые базовые функции Visual Studio для Mac, но если сталкиваетесь с Visual Studio для Mac впервые, обратитесь к документации о [Visual Studio для Mac](/visualstudio/mac/).

В Visual Studio для Mac, так же как в Visual Studio, код упорядочивается по *решениям* и *проектам*. Решение — это контейнер для одного или нескольких проектов. Проект может представлять собой приложение (например, для iOS или Android), вспомогательную библиотеку, тестовое приложение и т. д. В приложение Phoneword был добавлен новый проект iPhone с помощью шаблона **Приложение одного представления**. Исходное решение выглядело следующим образом:

![Снимок экрана исходного решения](hello-ios-deepdive-images/image30.png)

::: zone-end
::: zone pivot="windows"

## <a name="introduction-to-visual-studio"></a>Введение в Visual Studio

Visual Studio — это полнофункциональная интегрированная среда разработки (IDE) от корпорации Майкрософт. Она включает в себя полностью интегрированный визуальный конструктор, текстовый редактор с инструментами рефакторинга, обозреватель сборок, средства интеграции исходного кода и другие возможности. Это руководство описывает, как использовать некоторые основные возможности Visual Studio с инструментами Xamarin для Visual Studio.

Код в Visual Studio упорядочен по решениям и проектам. Решение — это контейнер для одного или нескольких проектов. Проект может представлять собой приложение (например, для iOS или Android), вспомогательную библиотеку, тестовое приложение и т. д. В приложение Phoneword был добавлен новый проект iPhone с помощью шаблона **Приложение одного представления**. Исходное решение выглядело следующим образом:

![Снимок экрана исходного решения](hello-ios-deepdive-images/vs-image30.png)

::: zone-end

## <a name="anatomy-of-a-xamarinios-application"></a>Структура приложения Xamarin.iOS

::: zone pivot="macos"

Слева находится **Панель решения**, которая содержит структуру каталогов и все файлы, связанные с решением:

![Панель решения, которая содержит структуру каталогов и все файлы, связанные с решением](hello-ios-deepdive-images/image31.png)

::: zone-end
::: zone pivot="windows"

Справа находится **область решений**, которая содержит структуру каталогов и все файлы, связанные с решением:

![Область решения, которая содержит структуру каталогов и все файлы, связанные с решением](hello-ios-deepdive-images/vs-image31.png)

::: zone-end

В пошаговом руководстве [Привет, iOS](~/ios/get-started/hello-ios/hello-ios-quickstart.md) вы создали решение **Phoneword** и поместили внутрь него проект iOS — **Phoneword_iOS**. Ниже перечислены элементы, входящие в проект:

- **References** — содержит сборки, необходимые для создания и запуска приложения. Разверните этот каталог, чтобы отобразить ссылки на сборки .NET, такие как [System](/dotnet/api/system), System.Core и [System.Xml](/dotnet/api/system.xml), а также ссылку на сборку Xamarin.iOS.
- **Packages** — каталог Packages содержит готовые пакеты NuGet.
- **Resources** — в папке Resources хранятся другие файлы мультимедиа.
- **Main.cs** — это файл содержит главную точку входа для приложения. Для запуска приложения имя главного класса приложения передается в `AppDelegate`.
- **AppDelegate.cs** — этот файл содержит главный класс приложения и отвечает за создание окна, формирование пользовательского интерфейса и прослушивание событий из операционной системы.
- **Main.Storyboard** — раскадровка содержит визуальную структуру для пользовательского интерфейса приложения. Файлы раскадровки открываются в графическом редакторе, называемом конструктором iOS.
- **ViewController.cs** — контроллер представления обслуживает экран (представление), который пользователь просматривает и с которым взаимодействует. Контроллер представления отвечает за обработку взаимодействия между пользователем и представлением.
- **ViewController.designer.cs** — `designer.cs` представляет собой автоматически созданный файл, который соединяет элементы управления в представлении и их представлениях кода в контроллере представления. Так как это файл для внутреннего подключения, интегрированная среда разработки перезаписывает любые внесенные вручную изменения, и в большинстве случаев этот файл можно игнорировать. Дополнительные сведения о связи между визуальным конструктором и кодом резервирования см. в руководстве [Введение в конструктор iOS](~/ios/user-interface/designer/introduction.md).
- **Info.plist** — в **Info.plist** задаются свойства приложения, такие как имя приложения, значки, изображения запуска и другие. Этот файл предоставляет обширные возможности и подробно описан в руководстве [Работа со списками свойств](~/ios/app-fundamentals/property-lists.md).
- **Entitlements.plist** — список свойств назначений позволяет указать *возможности* приложения (так называемые технологии App Store), например iCloud, PassKit и многие другие. Дополнительные сведения о файле **Entitlements.plist** см. в руководстве [Работа со списками свойств](~/ios/app-fundamentals/property-lists.md). Общие сведения о назначениях см. в руководстве [Подготовка устройств](~/ios/get-started/installation/device-provisioning/index.md).

## <a name="architecture-and-app-fundamentals"></a>Архитектура и принципы работы приложения

Прежде чем приложение iOS сможет загрузить пользовательский интерфейс, оно должно удовлетворять двум требованиям. Во-первых, приложение должно определить *точку входа* — это первый код, который выполняется при загрузке приложения в память. Во-вторых, оно должно определить класс для обработки событий на уровне приложения и взаимодействия с операционной системой.

В этом разделе рассматриваются связи, показанные на следующей схеме:

[![На этой схеме показаны связи архитектуры и принципов работы приложения](hello-ios-deepdive-images/image32.png)](hello-ios-deepdive-images/image32.png#lightbox)

### <a name="main-method"></a>Main - метод

Главной точкой входа для приложения iOS является класс `Application`. Класс `Application` определен в файле **Main.cs**, который содержит статический метод `Main`. Он создает экземпляр приложения Xamarin.iOS и передает имя класса *делегата приложения*, который будет обрабатывать события операционной системы. Код шаблона для статического метода `Main` представлен ниже:

```csharp
using System;
using UIKit;

namespace Phoneword_iOS
{
    public class Application
    {
        static void Main (string[] args)
        {
            UIApplication.Main (args, null, "AppDelegate");
        }
    }
}
```

### <a name="application-delegate"></a>Делегат приложения

В iOS класс *делегата приложения* обрабатывает системные события и располагается внутри **AppDelegate.cs**. Класс `AppDelegate` управляет *окном* приложения. Окно является отдельным экземпляром класса `UIWindow`, который служит контейнером для пользовательского интерфейса. По умолчанию приложение получает только одно окно для загрузки своего содержимого, и это окно подключено к *экрану* (отдельный экземпляр `UIScreen`), который предоставляет ограничивающий прямоугольник, соответствующий размерам экрана физического устройства.

*AppDelegate* также отвечает за подписку на обновления системы о важных событиях приложений, таких как завершение запуска приложения и нехватка памяти.

Код шаблона для AppDelegate приведен ниже:

```csharp
using System;
using Foundation;
using UIKit;

namespace Phoneword_iOS
{

    [Register ("AppDelegate")]
    public partial class AppDelegate : UIApplicationDelegate
    {
        public override UIWindow Window {
            get;
            set;
        }

        ...
    }
}
```

Когда приложение определило свое окно, оно может начать загрузку пользовательского интерфейса. В следующем разделе рассматривается создание пользовательского интерфейса.

## <a name="user-interface"></a>Пользовательский интерфейс

Пользовательский интерфейс приложения iOS похож на онлайн-магазин — приложение обычно получает одно окно, но может заполнять его любым необходимым количеством объектов, а объекты и схемы упорядочения могут изменяться в зависимости от того, что приложение хочет отобразить. В этом сценарии объекты — то, что видит пользователь — называются представлениями. Чтобы создать один экран в приложении, представления располагаются друг над другом в виде *иерархии представлений содержимого*, а управляет такой иерархией отдельный контроллер представления. Приложения с несколькими экранами используют несколько иерархий представлений содержимого, каждая из которых имеет свой контроллер представления. Приложение размещает представления в окне, чтобы создать другую иерархию представлений содержимого в зависимости от экрана, на котором находится пользователь.

В этом разделе подробно рассмотрен пользовательский интерфейс, а также описаны представления, иерархии представлений содержимого и конструктор iOS.

### <a name="ios-designer-and-storyboards"></a>Конструктор iOS и раскадровки

Конструктор IOS — это визуальный инструмент для создания пользовательских интерфейсов в Xamarin. Конструктор можно запустить, дважды щелкнув любой файл раскадровки (STORYBOARD). При этом будет открываться представление, похожее на следующий снимок экрана:

::: zone pivot="macos"

![Интерфейс конструктора iOS](hello-ios-deepdive-images/image33.png)

*Раскадровка* — это файл, содержащий визуальные структуры для экранов нашего приложения, а также переходы и связи между экранами. Представление экрана приложения в раскадровке называется _сценой_. Каждая сцена представляет контроллер представления и управляемый им стек представлений (иерархия представлений содержимого). При создании проекта **Приложение одного представления** из шаблона Visual Studio для Mac автоматически создает файл раскадровки с именем `Main.storyboard` и заполняет его одной сценой, как показано на снимке экрана ниже:

![Visual Studio для Mac автоматически создает файл раскадровки с именем Main.storyboard и заполняет его одной сценой](hello-ios-deepdive-images/image34.png)

С помощью черной полосы в нижней части экрана раскадровки можно выбрать контроллер представления для сцены. Контроллер представления является экземпляром класса `UIViewController`, который содержит код резервирования для иерархии представлений содержимого. Свойства в этом контроллере представления можно просмотреть и задать на **Панели свойств**, как показано на снимке экрана ниже:

![Область свойств](hello-ios-deepdive-images/image35.png)

::: zone-end
::: zone pivot="windows"

![Интерфейс конструктора iOS](hello-ios-deepdive-images/vs-image33.png)

*Раскадровка* — это файл, содержащий визуальные структуры для экранов нашего приложения, а также переходы и связи между экранами. Представление экрана приложения в раскадровке называется _сценой_. Каждая сцена представляет контроллер представления и управляемый им стек представлений (иерархия представлений содержимого). При создании проекта **Приложение одного представления** из шаблона Visual Studio автоматически создает файл раскадровки с именем `Main.storyboard` и заполняет его одной сценой, как показано на снимке экрана ниже:

![Visual Studio автоматически создает файл раскадровки с именем Main.storyboard и заполняет его одной сценой](hello-ios-deepdive-images/vs-image34.png)

С помощью полосы в нижней части экрана раскадровки можно выбрать контроллер представления для сцены. Контроллер представления является экземпляром класса `UIViewController`, который содержит код резервирования для иерархии представлений содержимого. Свойства в этом контроллере представления можно просмотреть и задать в **области свойств**, как показано на снимке экрана ниже:

![Область свойств](hello-ios-deepdive-images/vs-image35.png)

::: zone-end

_Представление_ можно выбрать, щелкнув внутри белой части сцены. Представление является экземпляром класса `UIView`, который определяет область экрана и предоставляет интерфейсы для работы с содержимым в этой области. Представлением по умолчанию является отдельное *корневое представление*, заполняющее весь экран устройства.

Слева от сцены находится серая стрелка со значком флага, как показано на снимке экрана ниже:

 [![Серая стрелка со значком флага](hello-ios-deepdive-images/image37.png)](hello-ios-deepdive-images/image37.png#lightbox)

Эта стрелка представляет переход раскадровки, который также называют *Segue* (произносится "сег-вей"). Так как этот переход не имеет источника, он называется *переходом без источника* (Sourceless Segue). Переход без источника указывает на первую сцену, представления которой загружаются в окно приложения при запуске последнего. Первым, что пользователь видит при загрузке приложения, будет сцена и представления внутри нее.

При создании пользовательского интерфейса можно перетащить дополнительные представления с **панели элементов** на основное представление в области конструктора, как показано на снимке экрана ниже:

::: zone pivot="macos"

![Можно перетащить дополнительные представления с панели элементов на основное представление в области конструктора](hello-ios-deepdive-images/image38.png)

::: zone-end
::: zone pivot="windows"

![Можно перетащить дополнительные представления с панели элементов на основное представление в области конструктора](hello-ios-deepdive-images/vs-image38.png)

::: zone-end

Эти дополнительные представления называются *вложенными*. Вместе корневое представление и вложенные представления входят в состав *иерархии представлений содержимого*, управляемой `ViewController`. Расположение всех этих элементов на сцене можно просмотреть на панели **Структура документа**:

::: zone pivot="macos"

![Панель "Структура документа"](hello-ios-deepdive-images/image39.png)

::: zone-end
::: zone pivot="windows"

![Панель "Структура документа"](hello-ios-deepdive-images/vs-image39.png)

::: zone-end

Вложенные представления выделены на схеме ниже:

::: zone pivot="macos"

![Вложенные представления на этой схеме выделены](hello-ios-deepdive-images/image40.png)

::: zone-end
::: zone pivot="windows"

![Вложенные представления на этой схеме выделены](hello-ios-deepdive-images/vs-image40.png)

::: zone-end

Следующий раздел описывает иерархию представлений содержимого, представленную этой сценой.

## <a name="content-view-hierarchy"></a>Иерархия представлений содержимого

_Иерархия представлений содержимого_ представляет собой стек представлений и вложенных представлений, управляемых одним отдельным контроллером представления, как показано на следующей схеме:

 [![Иерархия представлений содержимого](hello-ios-deepdive-images/image41.png)](hello-ios-deepdive-images/image41.png#lightbox)

Мы можем упростить просмотр иерархии представлений содержимого своего `ViewController`, временно изменив цвет фона корневого представления на желтый в разделе представления на **Панели свойств**, как показано на снимке экрана ниже:

::: zone pivot="macos"

![Изменение цвета фона корневого представления на желтый в разделе представлений Панели свойств](hello-ios-deepdive-images/image42.png)

::: zone-end
::: zone pivot="windows"

![Изменение цвета фона корневого представления на желтый в разделе представлений Панели свойств](hello-ios-deepdive-images/vs-image42.png)

::: zone-end

На следующей схеме показаны связи между окном, представлениями, вложенными представлениями и контроллером представления, которые позволяют вывести пользовательский интерфейс на экран устройства:

[![Связи между окном, представлениями, вложенными представлениями и контроллером представления](hello-ios-deepdive-images/image43.png)](hello-ios-deepdive-images/image43.png#lightbox)

Следующий раздел описывает работу с представлениями в коде, а также программирование взаимодействия с пользователем с помощью контроллеров представлений и жизненного цикла представлений.

## <a name="view-controllers-and-the-view-lifecycle"></a>Контроллеры представлений и жизненный цикл представления

Каждая иерархия представлений содержимого имеет соответствующий контроллер представления, обрабатывающий взаимодействие с пользователем. Роль контроллера представления заключается в управлении представлениями в иерархии представлений содержимого. Контроллер представления не является частью иерархии представлений содержимого, а также элементом в интерфейсе. Вместо этого он предоставляет код, обрабатывающий взаимодействие пользователя с объектами на экране.

### <a name="view-controllers-and-storyboards"></a>Контроллеры представлений и раскадровки

::: zone pivot="macos"

На раскадровке контроллер представлен полосой в нижней части сцены. При выборе контроллера представления его свойства отображаются на **Панели свойств**:

![При выборе контроллера представления его свойства отображаются на Панели свойств](hello-ios-deepdive-images/image44.png)

Пользовательский класс контроллера представления для иерархии представлений содержимого, представленный этой сценой, можно задать, изменяя свойство **Класс** в разделе **Удостоверение** на **панели свойств**. Например, наше приложение **Phoneword** задает `ViewController` в качестве контроллера представления для первого экрана, как показано на снимке экрана ниже:

![Приложение Phoneword задает ViewController в качестве контроллера представления](hello-ios-deepdive-images/image45new.png)

::: zone-end
::: zone pivot="windows"

На раскадровке контроллер представлен полосой в нижней части сцены. При выборе контроллера представления его свойства отображаются на **панели свойств**:

![При выборе контроллера представления его свойства отображаются на Панели свойств](hello-ios-deepdive-images/vs-image44.png)

Пользовательский класс контроллера представления для иерархии представлений содержимого, представленный этой сценой, можно задать, изменяя свойство **Класс** в разделе **Удостоверение** на **панели свойств**. Например, наше приложение **Phoneword** задает `ViewController` в качестве контроллера представления для первого экрана, как показано на снимке экрана ниже:

![Приложение Phoneword задает ViewController в качестве контроллера представления](hello-ios-deepdive-images/vs-image45.png)

::: zone-end

Это связывает представление раскадровки контроллера представления с классом `ViewController` C#. Откройте файл `ViewController.cs` и обратите внимание, что контроллер представления является *подклассом* класса `UIViewController`, как показано в следующем коде:

```csharp
public partial class ViewController : UIViewController
{
    public ViewController (IntPtr handle) : base (handle)
    {

    }
}
```

Теперь `ViewController` управляет взаимодействием иерархии представлений содержимого, связанной с этим контроллером представления в раскадровке. Далее вы узнаете о роли контроллера представления в управлении представлениями в рамках процесса, называемого жизненным циклом представления.

> [!NOTE]
> Для экранов, служащих только для вывода визуальной информации и не требующих вмешательства пользователя, свойство **Class** на **панели свойств** можно не указывать. При этом класс резервирования контроллера представления задается как реализация `UIViewController` по умолчанию, что удобно, если вы не планируете добавлять пользовательский код.

### <a name="view-lifecycle"></a>Жизненный цикл представления

Контроллер представления отвечает за загрузку иерархий представлений содержимого в окно и выгрузку их оттуда. Когда с представлением в иерархии представлений содержимого происходит что-то важное, операционная система уведомляет контроллер представления посредством событий в жизненном цикле представления. Переопределяя методы в жизненном цикле представления, вы можете взаимодействовать с объектами на экране и создавать динамический и быстродействующий пользовательский интерфейс.

Ниже представлены основные методы жизненного цикла и их функции:

- **ViewDidLoad** — вызывается, *когда* контроллер представления впервые загружает свою иерархию представлений содержимого в память. Он хорошо подходит для первоначальной настройки, так как именно здесь вложенные представления впервые становятся доступными в коде.
- **ViewWillAppear** — вызывается каждый раз, когда представление контроллера представления готовится к добавлению в иерархию представлений содержимого и отображается на экране.
- **ViewWillDisappear** — вызывается каждый раз, когда представление контроллера представления готовится к удалению из иерархии представлений содержимого и пропадает с экрана. Это событие жизненного цикла используется для очистки и сохранения состояния.
- **ViewDidAppear** и **ViewDidDisappear** — вызываются при добавлении представления в иерархию представлений содержимого и удалении его оттуда, соответственно.

При добавлении пользовательского кода на любом этапе жизненного цикла нужно *переопределить* *базовую реализацию* данного метода жизненного цикла. Для этого можно использовать существующий метод жизненного цикла, который уже имеет определенный код, расширив его с помощью дополнительного кода. Базовая реализация вызывается из метода для того, чтобы исходный код выполнялся перед добавленным вами новым кодом. Пример приведен в следующем разделе.

Дополнительные сведения о работе с контроллерами представлений см. в [руководстве по программированию контроллеров представлений для iOS](https://developer.apple.com/library/archive/featuredarticles/ViewControllerPGforiPhoneOS/index.html#//apple_ref/doc/uid/TP40007457-CH2-SW1) и [справочнике по UIViewController](https://developer.apple.com/documentation/uikit/uiviewcontroller?language=objc) компании Apple.

### <a name="responding-to-user-interaction"></a>Реакция на действия пользователя

Наиболее важной ролью контроллера представления является реагирование на взаимодействие с пользователем, например нажатия кнопок, переходы между элементами и многое другое. Взаимодействие с пользователем проще всего обрабатывать, предоставив элемент управления для прослушивания вводимых пользователем данных и подключив обработчик событий для реагирования на эти данные. Например, кнопку можно настроить для реагирования на событие сенсорного ввода, как показано в приложении Phoneword.

Давайте рассмотрим, как это работает.
В проекте `Phoneword_iOS` в иерархию представлений содержимого была добавлена кнопка `TranslateButton`:

[![В иерархию представлений содержимого была добавлена кнопка TranslateButton](hello-ios-deepdive-images/image1.png)](hello-ios-deepdive-images/image1.png#lightbox)

При назначении **имени** элементу управления **Button** на **Панели свойств** конструктор iOS автоматически сопоставил его с элементом управления в **ViewController.designer.cs**, сделав `TranslateButton` доступным в классе `ViewController`. Впервые элементы управления становятся доступными на этапе `ViewDidLoad` жизненного цикла представления, поэтому этот метод жизненного цикла используется для реагирования на сенсорный ввод пользователя:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();

    // wire up TranslateButton here
}
```

Приложение Phoneword использует событие сенсорного ввода `TouchUpInside` для прослушивания сенсорного ввода пользователя. `TouchUpInside` прослушивает событие отрыва пальца от экрана, а затем события прикосновения к экрану в границах элемента управления. Противоположностью `TouchUpInside` является событие `TouchDown`, которое возникает, когда пользователь нажимает элемент управления. Событие `TouchDown` перехватывает много посторонних данных и не позволяет пользователю отменить касание, отведя палец за границу элемента управления. `TouchUpInside` является наиболее распространенным способом для реагирования на касание **кнопки** и обеспечивает процедуру взаимодействия, ожидаемую пользователем при нажатии кнопки. Дополнительные сведения об этом доступны в [рекомендациях по работе с человеческим интерфейсом iOS](https://developer.apple.com/library/ios/documentation/userexperience/conceptual/MobileHIG/index.html) компании Apple.

Приложение обрабатывало событие `TouchUpInside` с помощью лямбда-выражения, но вместо этого можно было использовать делегат или именованный обработчик событий. В конечной форме код элемента "Button" выглядит так:

```csharp
public override void ViewDidLoad ()
{
    base.ViewDidLoad ();
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
      translatedNumber = Core.PhonewordTranslator.ToNumber(PhoneNumberText.Text);
      PhoneNumberText.ResignFirstResponder ();

      if (translatedNumber == "") {
        CallButton.SetTitle ("Call", UIControlState.Normal);
        CallButton.Enabled = false;
      } else {
        CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
        CallButton.Enabled = true;
      }
  };
}
```

## <a name="additional-concepts-introduced-in-phoneword"></a>Дополнительные понятия, представленные в Phoneword

В приложении Phoneword представлено несколько понятий, не охваченных этим руководством. В их число входят следующие:

- **Изменение текста кнопки** — приложение Phoneword продемонстрировало, как изменить текст элемента **Button**, вызвав `SetTitle` для **Button** и передав новый текст и _состояние элемента управления_**Button**. Например, следующий код позволяет изменить текст CallButton на "Call" (Вызов):

    ```csharp
    CallButton.SetTitle ("Call", UIControlState.Normal);
    ```

- **Включение и отключение кнопок** — **кнопки** могут находиться в состоянии `Enabled` или `Disabled`. Отключенный элемент **Button** не реагирует на действия пользователя. Например, в следующем коде отключается объект `CallButton`:

    ```csharp
    CallButton.Enabled = false;
    ```

    Дополнительные сведения о кнопках см. в руководстве [Кнопки](~/ios/user-interface/controls/buttons.md).

- **Убирание клавиатуры** — когда пользователь касается текстового поля, iOS отображает клавиатуру, чтобы он мог ввести данные. К сожалению, встроенная функция для убирания клавиатуры отсутствует. Следующий код добавляется в `TranslateButton`, чтобы закрывать клавиатуру, когда пользователь нажимает `TranslateButton`:

    ```csharp
    PhoneNumberText.ResignFirstResponder ();
    ```

    Еще один пример отмены клавиатуры см. в разделе [Отмена клавиатуры](https://github.com/xamarin/recipes/tree/master/Recipes/ios/input/keyboards/dismiss_the_keyboard).

- **Выполнение телефонного вызова с URL-адресом** — в приложении Phoneword используется схема URL-адресов Apple для запуска приложения телефонной системы. Пользовательская схема URL-адресов состоит из префикса "tel:" и преобразованного телефонного номера, как показано в следующем коде:

    ```csharp
    var url = new NSUrl ("tel:" + translatedNumber);
    if (!UIApplication.SharedApplication.OpenUrl (url))
    {
        // show alert Controller
    }
    ```

- **Отображение оповещения** — когда пользователь пытается выполнить телефонный вызов на устройстве, которое не поддерживает такие вызовы, например, в симуляторе или на iPod Touch, отображается диалоговое окно, оповещающее пользователя о невозможности телефонного вызова. Приведенный ниже код создает и заполняет контроллер оповещения:

    ```csharp
    if (!UIApplication.SharedApplication.OpenUrl (url)) {
                    var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                    alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                    PresentViewController (alert, true, null);
                }
    ```

    Дополнительные сведения о представлениях оповещений iOS см. в инструкции по [контроллеру оповещений](https://github.com/xamarin/recipes/tree/master/Recipes/ios/standard_controls/alertcontroller).

## <a name="testing-deployment-and-finishing-touches"></a>Тестирование, развертывание и последние штрихи

Как Visual Studio для Mac, так и Visual Studio предоставляют множество возможностей для тестирования и развертывания приложения. В этом разделе рассматриваются возможности отладки, демонстрируется тестирование приложения на устройстве и представлены инструменты для создания пользовательских значков приложения и изображений при запуске.

### <a name="debugging-tools"></a>Средства отладки

Иногда диагностика проблем в коде приложения может представлять трудность. Для выявления проблем в сложном коде можно [устанавливать точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint), [выполнять код пошагово](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code) или [выводить сведения в окне журнала](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window).

### <a name="deploy-to-a-device"></a>Развертывание на устройство

Симулятор iOS позволяет быстро протестировать приложение. Он имеет ряд полезных оптимизаций для тестирования, включая расположение макетов, [имитацию перемещения](https://github.com/xamarin/recipes/tree/master/Recipes/ios/multitasking/test_location_changes_in_simulator) и многое другое. Однако пользователи будут работать с итоговым приложением не в симуляторе. Все приложения следует с самого раннего этапа и регулярно протестировать на реальных устройствах.

Устройству требуется время для подготовки, а также учетная запись разработчика Apple. В руководстве [Подготовка устройств](~/ios/get-started/installation/device-provisioning/index.md) приведены подробные инструкции по подготовке устройства к разработке.

> [!NOTE]
> Сейчас, в связи с требованиями Apple, при компиляции кода для физического устройства или симулятора требуется сертификат разработки или _удостоверение подписывания_. Чтобы настроить их, следуйте указаниям в руководстве по [подготовке устройств](~/ios/get-started/installation/device-provisioning/manual-provisioning.md).

После подготовки устройство можно развернуть, подключив его, изменив целевой объект в панели инструментов сборки на устройство iOS и нажав кнопку **Запустить** (**Воспроизвести**), как показано на следующем снимке экрана:

::: zone pivot="macos"

![Нажатие кнопки "Запустить"/"Воспроизвести"](hello-ios-deepdive-images/image46new.png)

::: zone-end
::: zone pivot="windows"

![Нажатие кнопки "Запустить"/"Воспроизвести"](hello-ios-deepdive-images/vs-image46.png)

::: zone-end

Приложение развертывается на устройстве iOS:

[![Приложение развертывается на устройстве iOS и запускается](hello-ios-deepdive-images/image1.png)](hello-ios-deepdive-images/image1.png#lightbox)

### <a name="generate-custom-icons-and-launch-images"></a>Создание пользовательских значков и изображений, которые появляются при запуске

Не у всех есть конструктор для создания пользовательских значков и изображений при запуске, которые помогают приложению выделиться среди других. Ниже представлен ряд альтернативных средств для создания собственных изображений:

::: zone pivot="macos"

- [**Pixelmator**](https://www.pixelmator.com/) — универсальный редактор изображений для Mac, который стоит приблизительно 30 USD.
- [**Fiverr**](https://www.fiverr.com/) — воспользуйтесь услугами одного из дизайнеров по созданию набора значков по цене от 5 USD. Результат может быть разным, однако это хороший ресурс, если вам нужно создать значки максимально быстро.

::: zone-end
::: zone pivot="windows"

- Visual Studio — создать простой набор значков для приложения можно непосредственно в интегрированной среде разработки.
- [**Fiverr**](https://www.fiverr.com/) — воспользуйтесь услугами одного из дизайнеров по созданию набора значков по цене от 5 USD. Результат может быть разным, однако это хороший ресурс, если вам нужно создать значки максимально быстро.

::: zone-end

Дополнительные сведения о размерах значков изображений при запуске и требованиях к ним см. в руководстве по [работе со значками](~/ios/app-fundamentals/images-icons/index.md).

## <a name="summary"></a>Итоги

Поздравляем! Теперь у вас должно быть ясное понимание того, из каких компонентов состоит приложение Xamarin.iOS и какие инструменты нужны для его создания.
В [следующем руководстве из серии "Приступая к работе"](~/ios/get-started/hello-ios-multiscreen/index.md) вы расширите наше приложение, реализовав обработку нескольких экранов. При добавлении в приложение поддержки нескольких экранов вы реализуете контроллер навигации, узнаете о переходах раскадровки, а также познакомитесь с шаблоном "Модель — представление — контроллер" (MVC).

## <a name="related-links"></a>Связанные ссылки

- [Привет, iOS (пример)](/samples/xamarin/ios-samples/hello-ios)
- [Рекомендации по работе с человеческим интерфейсом iOS](https://developer.apple.com/design/human-interface-guidelines/ios/overview/themes/)
- [Портал подготовки iOS](https://developer.apple.com/account/#/overview)