---
title: Расширения iOS в Xamarin. iOS
description: В этом документе описываются расширения, которые представляют собой мини-приложения iOS в стандартном контексте, например в центре уведомлений. В нем обсуждается создание расширения и взаимодействие с ним из родительского приложения.
ms.prod: xamarin
ms.assetid: 3DEB3D43-3E4A-4099-8331-93C1E7A77095
ms.technology: xamarin-ios
ms.custom: xamu-video
author: conceptdev
ms.author: crdun
ms.date: 03/22/2017
ms.openlocfilehash: 5995ba06873b2fb5f75c593fbc7136806e50d982
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "70290606"
---
# <a name="ios-extensions-in-xamarinios"></a>расширения iOS в Xamarin. iOS

> [!VIDEO https://youtube.com/embed/Sd0-ch9Udmk]

**Создание расширений в видео iOS**

Расширения, представленные в iOS 8, являются специализированными `UIViewControllers` , которые представлены в iOS внутри стандартных контекстов, например в **центре уведомлений**, как пользовательские типы клавиатуры, запрошенные пользователем, для выполнения специализированного ввода или других контекстов. как и при редактировании фотографии, в которой расширение может предоставлять специальные фильтры эффектов.

Все расширения устанавливаются вместе с приложением-контейнером (с элементами, написанными с помощью унифицированных API-интерфейсов 64) и активируются из определенной точки расширения в ведущем приложении. И поскольку они будут использоваться как дополнения к существующим системным функциям, они должны быть высокопроизводительными, экономичными и надежными. 

## <a name="extension-points"></a>Точки расширения

|Тип|Описание|Точка расширения|Ведущее приложение|
|--- |--- |--- |--- |
|Действие|Специализированный редактор или средство просмотра для определенного типа мультимедиа|`com.apple.ui-services`|Любой|
|Поставщик документов|Разрешает приложению использовать удаленное хранилище документов|`com.apple.fileprovider-ui`|Приложения, использующие [уидокументпиккервиевконтроллер](xref:UIKit.UIDocumentPickerViewController)|
|Клавиатура|Альтернативные клавиатуры|`com.apple.keyboard-service`|Любой|
|Редактирование фотографий|Обработка и редактирование фотографий|`com.apple.photo-editing`|Photos. редактор приложений|
|Общий доступ|Совместное применение данных с социальными сетями, службами обмена сообщениями и т. д.|`com.apple.share-services`|Любой|
|Сегодня|"Мини-приложения", отображаемые на странице "сегодня" или в центре уведомлений|`com.apple.widget-extensions`|Сегодня и центр уведомлений|

[Дополнительные точки расширения](~/ios/platform/introduction-to-ios10/index.md#app-extensions) были добавлены в iOS 10.

## <a name="limitations"></a>Ограничения

Расширения имеют ряд ограничений, некоторые из которых являются универсальными для всех типов (например, ни один тип расширения не может получить доступ к камерам или микротелефонам), а другие типы расширения могут иметь определенные ограничения на их использование (например, пользовательские клавиатуры не может использоваться для полей ввода защищенных данных, таких как пароли). 

Существуют следующие универсальные ограничения.

- Недоступные платформы [пакета работоспособности](~/ios/platform/healthkit.md) и [набора событий](~/ios/platform/eventkit.md)
- Расширения не могут использовать [Расширенные фоновые режимы](~/ios/app-fundamentals/backgrounding/ios-backgrounding-techniques/registering-applications-to-run-in-background.md)
- Расширения не могут получить доступ к камерам или телефонам устройства (хотя они могут обращаться к существующим файлам мультимедиа)
- Расширения не могут получить данные из воздушных раскрывающихся данных (хотя они могут передавать данные через перетаскивание воздуха)
- [Уиактионшит](xref:UIKit.UIActionSheet) и [уиалертвиев](xref:UIKit.UIAlertView) недоступны; расширения должны использовать [уиалертконтроллер](xref:UIKit.UIAlertController)
- Несколько членов [UIApplication](xref:UIKit.UIApplication) недоступны: [UIApplication. шаредаппликатион](xref:UIKit.UIApplication.SharedApplication), [UIApplication. OpenURL](xref:UIKit.UIApplication.OpenUrl(Foundation.NSUrl)), [UIApplication. бегинигнорингинтерактионевентс](xref:UIKit.UIApplication.BeginIgnoringInteractionEvents) и [UIApplication. ендигнорингинтерактионевентс](xref:UIKit.UIApplication.EndIgnoringInteractionEvents)
- iOS обеспечивает ограничение использования памяти размером 16 МБ для современных расширений.
- По умолчанию расширения клавиатуры не имеют доступа к сети. Это влияет на отладку на устройстве (это ограничение не применяется в симуляторе), так как Xamarin. iOS требует доступа к сети для работы отладки. Можно запросить сетевой доступ, задав `Requests Open Access` значение в файле info. `Yes`plist проекта. Дополнительные сведения об ограничениях на расширение клавиатуры см. в разделе о [пользовательской клавиатуре](https://developer.apple.com/library/content/documentation/General/Conceptual/ExtensibilityPG/CustomKeyboard.html) Apple.

Сведения об индивидуальных ограничениях см. в статье программное обеспечение для [программирования расширения приложения](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/)Apple.

## <a name="distributing-installing-and-running-extensions"></a>Распространение, установка и запуск расширений

Расширения распространяются в приложении-контейнере, которое, в свою очередь, отправляется и распространяется через App Store. Расширения, распространяемые вместе с приложением, устанавливаются в этот момент, но пользователь должен явно включить каждое расширение. Различные типы расширений включены разными способами. для некоторых требуется, чтобы пользователь перешел к приложению " **Параметры** " и включил их. Другие доступны в точке использования, например при включении расширения общего доступа при отправке фотографий. 

Приложение, в котором используется расширение (где пользователь встречает точку расширения), называется **ведущим приложением**, так как это приложение, в котором размещается расширение при его выполнении. Приложение, устанавливающее расширение, является **приложением-контейнером**, так как это приложение, содержащее расширение при его установке.  

Как правило, приложение-контейнер описывает расширение и просматривает пользователя путем его включения.

## <a name="extension-lifecycle"></a>Жизненный цикл расширения

Расширение может быть простым, например, одним [UIViewController](xref:UIKit.UIViewController) или более сложными расширениями, которые представляют несколько экранов пользовательского интерфейса. Когда пользователь встречает _точки расширения_ (например, при совместном использовании образа), у него будет возможность выбрать из расширений, зарегистрированных для этой точки расширения. 

Если выбрать одно из расширений приложения, будет создан его `UIViewController` экземпляр и начнется жизненный цикл стандартного контроллера представления. Однако, в отличие от обычного приложения, которое приостановлено, но обычно не завершается, когда пользователь заканчивает взаимодействие с ними, расширения загружаются, выполняются, а затем завершаются повторно.

Расширения могут взаимодействовать со своими ведущими приложениями через объект [нсекстенсионконтекст](xref:Foundation.NSExtensionContext) . Некоторые расширения имеют операции, получающие асинхронные обратные вызовы с результатами. Эти обратные вызовы будут выполняться в фоновых потоках, и расширение должно учитывать это. Например, с помощью [нсобжект. инвокеонмаинсреад](xref:Foundation.NSObject.InvokeOnMainThread*) , если требуется обновить пользовательский интерфейс. Дополнительные сведения см. в разделе [взаимодействие с ведущим приложением](#communicating-with-the-host-app) .

По умолчанию расширения и их приложения контейнеров не могут взаимодействовать, несмотря на то, что они устанавливаются вместе. В некоторых случаях приложение-контейнер по сути является пустым контейнером "Доставка", цель которого обслуживается после установки расширения. Однако если обстоятельства определяют, приложение-контейнер и расширение могут совместно использовать ресурсы из общей области. Кроме того, **расширение Today** может запрашивать у своего приложения контейнера URL-адрес. Это поведение отображается в мини-приложении [обратного отсчета событий](https://github.com/xamarin/ios-samples/tree/master/intro-to-extensions).

## <a name="creating-an-extension"></a>Создание расширения

Расширения (и их приложения-контейнеры) должны быть 64-разрядными двоичными файлами и созданы с помощью [унифицированных API-интерфейсов](~/cross-platform/macios/unified/index.md)Xamarin. iOS. При разработке расширения ваши решения будут содержать по крайней мере два проекта: приложение контейнера и один проект для каждого расширения, предоставляемого контейнером.

### <a name="container-app-project-requirements"></a>Требования к проекту приложения контейнера

Приложение контейнера, используемое для установки расширения, имеет следующие требования:

- Он должен поддерживать ссылку на проект расширения.   
- Это должно быть полноценное приложение (оно должно иметь возможность запускаться и выполняться успешно) даже в том случае, если оно не делает ничего больше, чем предоставлять способ установки расширения. 
- Он должен иметь идентификатор пакета, являющийся основанием для идентификатора пакета проекта расширения (Дополнительные сведения см. в разделе ниже).

### <a name="extension-project-requirements"></a>Требования к проекту расширения

Кроме того, проект расширения имеет следующие требования.

- Он должен иметь идентификатор пакета, который начинается с идентификатора пакета приложения контейнера. Например, если у приложения-контейнера есть идентификатор `com.myCompany.ContainerApp`пакета, идентификатором расширения может быть: `com.myCompany.ContainerApp.MyExtension` 

  ![](extensions-images/bundleidentifiers.png) 
- Он должен определить ключ `NSExtensionPointIdentifier`с соответствующим значением ( `Info.plist` `com.apple.widget-extension` например, для мини-приложения центра уведомлений) в файле.
- Он также `NSExtensionPrincipalClass` *должен определить* `NSExtensionMainStoryboard` ключ или ключ в `Info.plist` файле с соответствующим значением:
  - Используйте ключ, чтобы указать имя раскадровки, которая представляет основной пользовательский интерфейс для расширения (минус `.storyboard`). `NSExtensionMainStoryboard` Например, `Main` `Main.storyboard` для файла.
  - `NSExtensionPrincipalClass` Используйте ключ, чтобы указать класс, который будет инициализирован при запуске расширения. Значение должно соответствовать `UIViewController`значению **регистра** : 

  ![](extensions-images/registerandprincipalclass.png)

Определенные типы расширений могут иметь дополнительные требования. Например, в **сегодняшнем** или основном классе **центра уведомлений** должен быть реализован [инквиджетпровидинг](xref:NotificationCenter.INCWidgetProviding).

> [!IMPORTANT]
> При запуске проекта с использованием одного из шаблонов расширений, предоставленных Visual Studio для Mac, большинство (если не все) эти требования будут предоставлены и выполнены автоматически шаблоном.

## <a name="walkthrough"></a>Пошаговое руководство 

В следующем пошаговом руководстве вы создадите мини-приложение с примером **сегодня** , которое вычисляет день и количество дней, оставшихся в году:

[![](extensions-images/carpediemscreenshot-sm.png "Пример мини-приложения сегодня, которое вычисляет день и количество дней, оставшихся в году")](extensions-images/carpediemscreenshot.png#lightbox)

### <a name="creating-the-solution"></a>Создание решения

Чтобы создать требуемое решение, выполните следующие действия.

1. Сначала создайте проект iOS, **единый Просмотр приложения** и нажмите кнопку **Далее** : 

    [![](extensions-images/today01.png "Сначала создайте проект iOS, единый Просмотр приложения и нажмите кнопку Далее.")](extensions-images/today01.png#lightbox)
2. Вызовите проект `TodayContainer` и нажмите кнопку **Далее** : 

    [![](extensions-images/today02.png "Вызов проекта Тодайконтаинер и нажатие кнопки \"Далее\"")](extensions-images/today02.png#lightbox)
3. Проверьте **имя проекта** и **имя_решения** и нажмите кнопку " **создать** ", чтобы создать решение: 

    [![](extensions-images/today03.png "Проверьте имя проекта и имя_решения и нажмите кнопку \"создать\", чтобы создать решение.")](extensions-images/today03.png#lightbox)
4. Затем в **Обозреватель решений**щелкните решение правой кнопкой мыши и добавьте новый проект **расширения iOS** из шаблона " **Today** ". 

    [![](extensions-images/today04.png "Затем в обозреватель решений щелкните решение правой кнопкой мыши и добавьте новый проект расширения iOS из шаблона \"Today\".")](extensions-images/today04.png#lightbox)
5. Вызовите проект `DaysRemaining` и нажмите кнопку **Далее** : 

    [![](extensions-images/today05.png "Вызов проекта Дайсремаининг и нажатие кнопки \"Далее\"")](extensions-images/today05.png#lightbox)
6. Проверьте проект и нажмите кнопку **создать** , чтобы создать его: 

    [![](extensions-images/today06.png "Проверьте проект и нажмите кнопку \"создать\", чтобы создать его.")](extensions-images/today06.png#lightbox)

В полученном решении теперь должно быть два проекта, как показано ниже:

[![](extensions-images/today07.png "В полученном решении теперь должно быть два проекта, как показано ниже.")](extensions-images/today07.png#lightbox)

### <a name="creating-the-extension-user-interface"></a>Создание пользовательского интерфейса расширения

Далее вам потребуется разработать интерфейс для своего мини-приложения **сегодня** . Это можно сделать либо с помощью раскадровки, либо путем создания пользовательского интерфейса в коде. Оба метода будут рассмотрены ниже подробно.

#### <a name="using-storyboards"></a>Использование раскадровки

Чтобы создать пользовательский интерфейс с раскадровкой, выполните следующие действия.

1. В **Обозреватель решений**дважды щелкните `Main.storyboard` файл проекта расширения, чтобы открыть его для редактирования: 

    [![](extensions-images/today08.png "Дважды щелкните файл проекты расширения Main. Storyboard, чтобы открыть его для редактирования.")](extensions-images/today08.png#lightbox)
2. Выберите метку, которая была автоматически добавлена в пользовательский интерфейс с помощью шаблона, и присвойте ей **имя** `TodayMessage` на вкладке **мини** -приложение **обозревателя свойств**: 

    [![](extensions-images/today09.png "Выберите метку, которая была автоматически добавлена в пользовательский интерфейс с помощью шаблона, и присвойте ей имя Тодаймессаже на вкладке мини-приложение обозревателя свойств.")](extensions-images/today09.png#lightbox)
3. Сохраните изменения в раскадровке.

#### <a name="using-code"></a>Использование кода

Чтобы создать пользовательский интерфейс в коде, выполните следующие действия. 

1. В **Обозреватель решений**выберите проект **дайсремаининг** , добавьте новый класс и вызовите его `CodeBasedViewController`: 

    [![](extensions-images/code01.png "Аелект проект Дайсремаининг, добавьте новый класс и назовите его Кодебаседвиевконтроллер")](extensions-images/code01.png#lightbox)
2. Опять же, в **Обозреватель решений**дважды щелкните `Info.plist` файл расширения, чтобы открыть его для редактирования: 

    [![](extensions-images/code02.png "Дважды щелкните файл Extensions info. plist, чтобы открыть его для редактирования.")](extensions-images/code02.png#lightbox)
3. Выберите **представление исходного кода** (в нижней части экрана) и откройте `NSExtension` узел: 

    [![](extensions-images/code03.png "Выберите представление исходного кода в нижней части экрана и откройте узел Нсекстенсион.")](extensions-images/code03.png#lightbox)
4. Удалите ключ и `NSExtensionPrincipalClass` добавьте со значением `CodeBasedViewController`: `NSExtensionMainStoryboard` 

    [![](extensions-images/code04.png "Удалите ключ Нсекстенсионмаинсторибоард и добавьте НсекстенсионпринЦипалкласс со значением Кодебаседвиевконтроллер")](extensions-images/code04.png#lightbox)
5. Сохраните изменения.

Затем измените `CodeBasedViewController.cs` файл и сделайте его следующим:

```csharp
using System;
using Foundation;
using UIKit;
using NotificationCenter;
using CoreGraphics;

namespace DaysRemaining
{
  [Register("CodeBasedViewController")]
  public class CodeBasedViewController : UIViewController, INCWidgetProviding
  {
    public CodeBasedViewController ()
    {
    }

    public override void ViewDidLoad ()
    {
      base.ViewDidLoad ();

      // Add label to view
      var TodayMessage = new UILabel (new CGRect (0, 0, View.Frame.Width, View.Frame.Height)) {
        TextAlignment = UITextAlignment.Center
      };

      View.AddSubview (TodayMessage);

      // Insert code to power extension here...

    }
  }
}
```

Обратите внимание `[Register("CodeBasedViewController")]` , что соответствует значению, указанному `NSExtensionPrincipalClass` для приведенного выше.

### <a name="coding-the-extension"></a>Кодирование расширения

После создания пользовательского интерфейса откройте `TodayViewController.cs` `CodeBasedViewController.cs` файл или (на основе метода, используемого для создания пользовательского интерфейса выше), измените метод **ViewDidLoad** и сделайте его следующим:

```csharp
public override void ViewDidLoad ()
{
  base.ViewDidLoad ();

  // Calculate the values
  var dayOfYear = DateTime.Now.DayOfYear;
  var leapYearExtra = DateTime.IsLeapYear (DateTime.Now.Year) ? 1 : 0;
  var daysRemaining = 365 + leapYearExtra - dayOfYear;

  // Display the message
  if (daysRemaining == 1) {
    TodayMessage.Text = String.Format ("Today is day {0}. There is one day remaining in the year.", dayOfYear);
  } else {
    TodayMessage.Text = String.Format ("Today is day {0}. There are {1} days remaining in the year.", dayOfYear, daysRemaining);
  }
}
```

При использовании метода пользовательского интерфейса на основе кода замените `// Insert code to power extension here...` комментарий новым кодом, приведенным выше. После вызова базовой реализации (и вставки метки для версии на основе кода) Этот код выполняет простое вычисление для получения дня года и количества оставшихся дней. Затем оно отобразит сообщение в метке (`TodayMessage`), созданной в конструкторе пользовательского интерфейса.

Обратите внимание на то, как подобный процесс является обычным процессом написания приложения. Расширение `UIViewController` имеет тот же жизненный цикл, что и контроллер представления в приложении, за исключением того, что расширения не имеют фоновых режимов и не приостанавливаются после того, как пользователь завершит их использование. Вместо этого расширения многократно инициализируются и освобождаются при необходимости.

### <a name="creating-the-container-app-user-interface"></a>Создание пользовательского интерфейса приложения контейнера

В этом пошаговом руководстве приложение-контейнер просто используется как метод для доставки и установки расширения и не предоставляет собственной функциональности. Измените `Main.storyboard` файл тодайконтаинер и добавьте некоторый текст, определяющий функцию расширения, и способ ее установки:

[![](extensions-images/today10.png "Измените файл main. Storyboard Тодайконтаинерс и добавьте некоторый текст, определяющий функцию Extensions, и способ ее установки.")](extensions-images/today10.png#lightbox)

Сохраните изменения в раскадровке.

### <a name="testing-the-extension"></a>Тестирование расширения

Чтобы проверить расширение в симуляторе iOS, запустите приложение **тодайконтаинер** . Отобразится главное представление контейнера:

[![](extensions-images/run01.png "Будет отображено главное представление \"контейнеры\"")](extensions-images/run01.png#lightbox)

Затем нажмите кнопку " **домой** " в симуляторе, проведите вниз в верхней части экрана, чтобы открыть **Центр уведомлений**, перейдите на вкладку " **сегодня** " и нажмите кнопку " **изменить** ":

[![](extensions-images/run02.png "Нажмите кнопку \"домой\" в симуляторе, проведите вниз в верхней части экрана, чтобы открыть центр уведомлений, перейдите на вкладку \"сегодня\" и нажмите кнопку \"Изменить\".")](extensions-images/run02.png#lightbox)

Добавьте расширение **дайсремаининг** в представление " **сегодня** " и нажмите кнопку **done (Готово** ).

[![](extensions-images/run03.png "Добавление расширения Дайсремаининг в представление \"сегодня\" и нажатие кнопки \"Готово\"")](extensions-images/run03.png#lightbox)

Новое мини-приложение будет добавлено в представление " **сегодня** ", и результаты будут отображены:

[![](extensions-images/run04.png "Новое мини-приложение будет добавлено в представление \"сегодня\", и результаты будут отображены")](extensions-images/run04.png#lightbox)

## <a name="communicating-with-the-host-app"></a>Взаимодействие с ведущим приложением

Представленное выше расширение, созданное ранее, не взаимодействует с ведущим приложением ( **текущий** экран). Если это так, будет использоваться свойство [ExtensionContext](xref:Foundation.NSExtensionContext) классов `TodayViewController` или `CodeBasedViewController`. 

Для расширений, которые будут получать данные из их ведущих приложений, данные представляются в виде массива объектов [нсекстенсионитем](xref:Foundation.NSExtensionItem) , хранящихся в свойстве [инпутитемс](xref:Foundation.NSExtensionContext.InputItems) [екстенсионконтекст](xref:Foundation.NSExtensionContext) расширения `UIViewController`.

Другое расширение, например модули редактирования фотографий, может отличать пользователя от завершения или отмены использования. Она будет возвращена в ведущее приложение с помощью методов [комплетерекуест](xref:Foundation.NSExtensionContext.CompleteRequest*) и [CancelRequest](xref:Foundation.NSExtensionContext.CancelRequest*) свойства [екстенсионконтекст](xref:Foundation.NSExtensionContext) .

Дополнительные сведения см. в разделе [руководств по программированию для расширения приложения](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/index.html#//apple_ref/doc/uid/TP40014214-CH20-SW1)Apple.

## <a name="communicating-with-the-parent-app"></a>Взаимодействие с родительским приложением

Группы приложений предоставляют различным приложениям (или одному приложению и его расширениям) общее место хранения файлов. Группы приложений можно использовать для следующих данных:

- [Параметры Apple Watch](~/ios/watchos/app-fundamentals/settings.md).
- [Общий нсусердефаултс](~/ios/app-fundamentals/user-defaults.md).
- [Общие файлы](~/ios/watchos/app-fundamentals/parent-app.md#files).

Дополнительные сведения см. в разделе " [группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) " документации по **работе с возможностями** .

## <a name="mobilecoreservices"></a>мобилекоресервицес

При работе с расширениями используйте универсальный идентификатор типа (UTI) для создания и управления данными, которыми обмениваются приложения, другие приложения и (или) службы.

Статический класс определяет следующие вспомогательные свойства, связанные с `kUTType...` определениями Apple: `MobileCoreServices.UTType`

- `kUTTypeAlembic` - `Alembic`
- `kUTTypeAliasFile` - `AliasFile`
- `kUTTypeAliasRecord` - `AliasRecord`
- `kUTTypeAppleICNS` - `AppleICNS`
- `kUTTypeAppleProtectedMPEG4Audio` - `AppleProtectedMPEG4Audio`
- `kUTTypeAppleProtectedMPEG4Video` - `AppleProtectedMPEG4Video`
- `kUTTypeAppleScript` - `AppleScript`
- `kUTTypeApplication` - `Application`
- `kUTTypeApplicationBundle` - `ApplicationBundle`
- `kUTTypeApplicationFile` - `ApplicationFile`
- `kUTTypeArchive` - `Archive`
- `kUTTypeAssemblyLanguageSource` - `AssemblyLanguageSource`
- `kUTTypeAudio` - `Audio`
- `kUTTypeAudioInterchangeFileFormat` - `AudioInterchangeFileFormat`
- `kUTTypeAudiovisualContent` - `AudiovisualContent`
- `kUTTypeAVIMovie` - `AVIMovie`
- `kUTTypeBinaryPropertyList` - `BinaryPropertyList`
- `kUTTypeBMP` - `BMP`
- `kUTTypeBookmark` - `Bookmark`
- `kUTTypeBundle` - `Bundle`
- `kUTTypeBzip2Archive` - `Bzip2Archive`
- `kUTTypeCalendarEvent` - `CalendarEvent`
- `kUTTypeCHeader` - `CHeader`
- `kUTTypeCommaSeparatedText` - `CommaSeparatedText`
- `kUTTypeCompositeContent` - `CompositeContent`
- `kUTTypeConformsToKey` - `ConformsToKey`
- `kUTTypeContact` - `Contact`
- `kUTTypeContent` - `Content`
- `kUTTypeCPlusPlusHeader` - `CPlusPlusHeader`
- `kUTTypeCPlusPlusSource` - `CPlusPlusSource`
- `kUTTypeCSource` - `CSource`
- `kUTTypeData` - `Database`
- `kUTTypeDelimitedText` - `DelimitedText`
- `kUTTypeDescriptionKey` - `DescriptionKey`
- `kUTTypeDirectory` - `Directory`
- `kUTTypeDiskImage` - `DiskImage`
- `kUTTypeElectronicPublication` - `ElectronicPublication`
- `kUTTypeEmailMessage` - `EmailMessage`
- `kUTTypeExecutable` - `Executable`
- `kUTExportedTypeDeclarationsKey` - `ExportedTypeDeclarationsKey`
- `kUTTypeFileURL` - `FileURL`
- `kUTTypeFlatRTFD` - `FlatRTFD`
- `kUTTypeFolder` - `Folder`
- `kUTTypeFont` - `Font`
- `kUTTypeFramework` - `Framework`
- `kUTTypeGIF` - `GIF`
- `kUTTypeGNUZipArchive` - `GNUZipArchive` 
- `kUTTypeHTML` - `HTML`
- `kUTTypeICO` - `ICO`
- `kUTTypeIconFileKey` - `IconFileKey`
- `kUTTypeIdentifierKey` - `IdentifierKey`
- `kUTTypeImage` - `Image`
- `kUTImportedTypeDeclarationsKey` - `ImportedTypeDeclarationsKey`
- `kUTTypeInkText` - `InkText`
- `kUTTypeInternetLocation` - `InternetLocation`
- `kUTTypeItem` - `Item`
- `kUTTypeJavaArchive` - `JavaArchive`
- `kUTTypeJavaClass` - `JavaClass`
- `kUTTypeJavaScript` - `JavaScript`
- `kUTTypeJavaSource` - `JavaSource`
- `kUTTypeJPEG` - `JPEG`
- `kUTTypeJPEG2000` - `JPEG2000`
- `kUTTypeJSON` - `JSON`
- `kUTType3dObject` - `k3dObject`
- `kUTTypeLivePhoto` - `LivePhoto`
- `kUTTypeLog` - `Log` 
- `kUTTypeM3UPlaylist` - `M3UPlaylist`
- `kUTTypeMessage` - `Message`
- `kUTTypeMIDIAudio` - `MIDIAudio`
- `kUTTypeMountPoint` - `MountPoint`
- `kUTTypeMovie` - `Movie`
- `kUTTypeMP3` - `MP3`
- `kUTTypeMPEG` - `MPEG`
- `kUTTypeMPEG2TransportStream` - `MPEG2TransportStream`
- `kUTTypeMPEG2Video` - `MPEG2Video`
- `kUTTypeMPEG4` - `MPEG4`
- `kUTTypeMPEG4Audio` - `MPEG4Audio`
- `kUTTypeObjectiveCPlusPlusSource` - `ObjectiveCPlusPlusSource`
- `kUTTypeObjectiveCSource` - `ObjectiveCSource`
- `kUTTypeOSAScript` - `OSAScript`
- `kUTTypeOSAScriptBundle` - `OSAScriptBundle`
- `kUTTypePackage` - `Package`
- `kUTTypePDF` - `PDF`
- `kUTTypePerlScript` - `PerlScript`
- `kUTTypePHPScript` - `PHPScript`
- `kUTTypePICT` - `PICT`
- `kUTTypePKCS12` - `PKCS12`
- `kUTTypePlainText` - `PlainText`
- `kUTTypePlaylist` - `Playlist`
- `kUTTypePluginBundle` - `PluginBundle`
- `kUTTypePNG` - `PNG`
- `kUTTypePolygon` - `Polygon`
- `kUTTypePresentation` - `Presentation`
- `kUTTypePropertyList` - `PropertyList`
- `kUTTypePythonScript` - `PythonScript`
- `kUTTypeQuickLookGenerator` - `QuickLookGenerator`
- `kUTTypeQuickTimeImage` - `QuickTimeImage`
- `kUTTypeQuickTimeMovie` - `QuickTimeMovie` 
- `kUTTypeRawImage` - `RawImage`
- `kUTTypeReferenceURLKey` - `ReferenceURLKey`
- `kUTTypeResolvable` - `Resolvable`
- `kUTTypeRTF` - `RTF`
- `kUTTypeRTFD` - `RTFD`
- `kUTTypeRubyScript` - `RubyScript`
- `kUTTypeScalableVectorGraphics` - `ScalableVectorGraphics`
- `kUTTypeScript` - `Script`
- `kUTTypeShellScript` - `ShellScript`
- `kUTTypeSourceCode` - `SourceCode`
- `kUTTypeSpotlightImporter` - `SpotlightImporter`
- `kUTTypeSpreadsheet` - `Spreadsheet`
- `kUTTypeStereolithography` - `Stereolithography`
- `kUTTypeSwiftSource` - `SwiftSource`
- `kUTTypeSymLink` - `SymLink`
- `kUTTypeSystemPreferencesPane` - `SystemPreferencesPane`
- `kUTTypeTabSeparatedText` - `TabSeparatedText`
- `kUTTagClassFilenameExtension` - `TagClassFilenameExtension`
- `kUTTagClassMIMEType` - `TagClassMIMEType`
- `kUTTypeTagSpecificationKey` - `TagSpecificationKey`
- `kUTTypeText` - `Text`
- `kUTType3DContent` - `ThreeDContent`
- `kUTTypeTIFF` - `TIFF`
- `kUTTypeToDoItem` - `ToDoItem`
- `kUTTypeTXNTextAndMultimediaData` - `TXNTextAndMultimediaData`
- `kUTTypeUniversalSceneDescription` - `UniversalSceneDescription`
- `kUTTypeUnixExecutable` - `UnixExecutable`
- `kUTTypeURL` - `URL` 
- `kUTTypeURLBookmarkData` - `URLBookmarkData`
- `kUTTypeUTF16ExternalPlainText` - `UTF16ExternalPlainText`
- `kUTTypeUTF16PlainText` - `UTF16PlainText`
- `kUTTypeUTF8PlainText` - `UTF8PlainText`
- `kUTTypeUTF8TabSeparatedText` - `UTF8TabSeparatedText`
- `kUTTypeVCard` - `VCard`
- `kUTTypeVersionKey` - `VersionKey` 
- `kUTTypeVideo` - `Video` 
- `kUTTypeVolume` - `Volume` 
- `kUTTypeWaveformAudio` - `WaveformAudio`
- `kUTTypeWebArchive` - `WebArchive`
- `kUTTypeWindowsExecutable` - `WindowsExecutable`
- `kUTTypeX509Certificate` - `X509Certificate`
- `kUTTypeXML` - `XML`
- `kUTTypeXMLPropertyList` - `XMLPropertyList`
- `kUTTypeXPCService` - `XPCService`
- `kUTTypeZipArchive` - `ZipArchive`

См. Следующий пример:

```csharp
using MobileCoreServices;
...

NSItemProvider itemProvider = new NSItemProvider ();
itemProvider.LoadItem(UTType.PropertyList ,null, (item, err) => {
    if (err == null) {
        NSDictionary results = (NSDictionary )item;
        NSString baseURI =
results.ObjectForKey("NSExtensionJavaScriptPreprocessingResultsKey");
    }
});
```

Дополнительные сведения см. в разделе " [группы приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) " документации по **работе с возможностями** .

## <a name="precautions-and-considerations"></a>Меры предосторожности и рекомендации

Расширения имеют значительно меньший объем доступной памяти, чем приложения. Они должны выполняться быстро и с минимальным вторжением пользователя и приложения, в котором они размещены. Однако расширение также должно предоставлять отличительную полезную функцию для приложения с фирменным ИНТЕРФЕЙСом пользователя, что позволяет пользователю указать разработчика или контейнерного приложения, к которому они относятся.

Учитывая эти строгие требования, следует развертывать только те расширения, которые были тщательно протестированы и оптимизированы для производительности и потребления памяти. 

## <a name="summary"></a>Сводка

Этот документ содержит охваченные расширения, их типы, тип точек расширения и известные ограничения, накладываемые на расширение iOS. В нем обсуждаются создание, распространение, установка и запуск расширений, а также жизненный цикл расширения. В нем представлено пошаговое руководство по **созданию простого мини** -приложения, которое показывает два способа создания пользовательского интерфейса мини-приложения с помощью раскадровки или кода. Было показано, как тестировать расширение в симуляторе iOS. Наконец, в нем вкратце обсуждается взаимодействие с ведущим приложением и некоторые меры предосторожности и рекомендации, которые следует предпринять при разработке расширения. 

## <a name="related-links"></a>Связанные ссылки

- [Контаинерапп (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/intro-to-extensions)
- [Создание расширений в Xamarin. iOS (видео)](https://university.xamarin.com/lightninglectures/creating-extensions-in-ios)
