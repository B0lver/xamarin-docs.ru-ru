---
title: Общие сведения о упреждающих предложениях в Xamarin. iOS
description: В этой статье показано, как использовать упреждающие предложения в приложении Xamarin. iOS для управления участием, позволяя системе автоматически предоставлять пользователю полезную информацию.
ms.prod: xamarin
ms.assetid: 8DDD084A-0D1E-4DF7-B686-6309DCEFF5D3
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/16/2017
ms.openlocfilehash: 4a5081c9246a0a740d5ba0516d69d23f4d3019b4
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73031539"
---
# <a name="introduction-to-proactive-suggestions-in-xamarinios"></a>Общие сведения о упреждающих предложениях в Xamarin. iOS

_В этой статье показано, как использовать упреждающие предложения в приложении Xamarin. iOS для управления участием, позволяя системе автоматически предоставлять пользователю полезную информацию._

Новые возможности iOS 10 — заблаговременное предложение предоставляет пользователям возможность привлекаться к приложению Xamarin. iOS с помощью упреждающего предоставления пользователю полезной информации в нужное время.

в iOS 10 представлены новые способы привлечения к приложению, позволяющие системе автоматически предоставлять пользователю полезную информацию в нужное время. Так же как iOS 9 предоставляет возможность добавлять в приложение глубокий поиск с помощью советов Spotlight, передачи и Siri (см. раздел [новые API-интерфейсы поиска](~/ios/platform/search/index.md)), с iOS 10 приложение может предоставлять функции, которые могут быть предоставлены пользователю системой в следующих расположениях. :

- Переключатель приложений
- Экран блокировки
- карплай
- Карты
- Взаимодействия Siri
- Предложения Куикктипе

Приложение предоставляет эту функцию системе, используя набор технологий, таких как `NSUserActivity`, веб-разметка, ядро Spotlight, Мапкит, мультимедийный проигрыватель и UIKit. Кроме того, предоставляя поддержку упреждающего предложения для приложения, она становится более глубокой Siri интеграцией бесплатно.

## <a name="location-based-suggestions"></a>Предложения на основе расположения

В iOS 10 класс `NSUserActivity` включает свойство `MapItem`, позволяющее разработчику указать сведения о расположении, которые можно использовать в других контекстах. Например, если приложение отображает Отзывы о ресторане, разработчик может задать для свойства `MapItem` расположение ресторана, который пользователь просматривает в приложении. Если пользователь переключается на приложение Maps, расположение ресторана автоматически становится доступным.

Если приложение поддерживает поиск приложений, оно может использовать новые компоненты адреса класса `CSSearchableItemAttributesSet`, чтобы указать расположения, которые пользователь может захотеть посетить. Устанавливая свойство `MapItem`, другие свойства автоматически заполняются.

Помимо настройки `Latitude` и `Longitude` свойств компонента Address, рекомендуется, чтобы приложение предоставляя свойства `NamedLocation` и `PhoneNumbers`, поэтому Siri может инициировать вызов в расположение.

## <a name="web-markup-based-suggestions"></a>Предложения на основе веб-разметки

в iOS 9 добавлена возможность включения разметки структурированных данных на веб-сайте, которая дополняет содержимое, которое пользователи видят в Spotlight и результатах поиска Safari (см. раздел [Поиск в веб-разметке](~/ios/platform/search/web-markup.md)). в iOS 10 добавлена возможность включения разметки на основе расположения (например, [посталаддресс](https://schema.org/PostalAddress) , как определено в [Schema.org](https://schema.org/)) для дальнейшего улучшения интерфейса пользователя. Например, если пользователи просматривает страницу, помеченную как расположение на веб-сайте, система может предложить то же расположение при открытии карт.

## <a name="text-based-suggestions"></a>Предложения на основе текста

UIKit был расширен в iOS 10, чтобы включить свойство [текстконтенттипе](https://developer.apple.com/reference/uikit/uitextinputtraits/1649656-textcontenttype) класса [уитекстинпуттраитс](https://developer.apple.com/reference/uikit/uitextinputtraits) , чтобы указать семантическое значение содержимого в текстовой области. Используя эти сведения, система может автоматически выбирать подходящий тип клавиатуры, улучшать предложения автоисправления и заранее интегрировать информацию из других приложений и веб-сайтов.

Например, если пользователь вводит текст в текстовое поле, помеченное как `UITextContentType.FullStreetAddress`, система может предложить Автозаполнение поля в том месте, где пользователь был в последний взгляд.

## <a name="media-based-suggestions"></a>Предложения на основе мультимедиа

Если приложение воспроизводит мультимедиа с помощью API [мпплайаблеконтентманажер](xref:MediaPlayer.MPPlayableContentManager) , iOS 10 позволяет пользователям просматривать обложку альбома и воспроизводить мультимедиа с помощью приложения на экране блокировки.

## <a name="contextual-siri-reminders"></a>Контекстные напоминания Siri

Позволяет пользователю использовать Siri, чтобы быстро создать напоминание для просмотра содержимого, просматриваемого в данный момент в приложении позже. Например, если они просматривают отзыв о ресторане в приложении, они могут вызвать Siri и сказать *«напомнить об этом, когда я получаю домой».* Siri создаст напоминание со ссылкой на проверку в приложении.

## <a name="contact-based-suggestions"></a>Предложения на основе контактов

Позволяет контактам приложения (и контактной информации) отображаться в **контактном** приложении на устройстве iOS вместе со всеми контактами пользователей, хранящимися в системе.

## <a name="ride-sharing-based-suggestions"></a>Заменять предложения на основе общего доступа

Если приложение для совместного использования использует API [мкдиректионсрекуест](xref:MapKit.MKDirectionsRequest) , iOS 10 будет предоставлять его в качестве параметра переключателя приложений в то время, когда пользователю, скорее всего, потребуется сделать это. Приложение также должно быть зарегистрировано в качестве приложения для предоставления общего доступа путем указания `MKDirectionsModeRideShare` для ключа [мкдиректионсаппликатионсуппортедмодес](https://developer.apple.com/library/content/documentation/General/Reference/InfoPlistKeyReference/Articles/iPhoneOSKeys.html) в `Info.plist`ном файле.

Если приложение поддерживает только заменяющий общий доступ, предложение системы будет начинаться с *"получить ответ на..."* , если поддерживаются другие типы направления маршрутизации (например, "прохождение" или "велосипед"), система будет использовать *"получить указания...".*

> [!IMPORTANT]
> Объект [мкмапитем](xref:MapKit.MKMapItem) , который получает приложение, может не содержать сведения о долготе и широте, а также потребовать геокодирования.

## <a name="implementing-proactive-suggestions"></a>Реализация упреждающих предложений

Добавление поддержки упреждающего предложения в приложение Xamarin. iOS, как правило, упрощает реализацию нескольких интерфейсов API или расширение нескольких интерфейсов API, которые приложение может уже реализовать.

Упреждающие предложения работают с приложениями тремя основными способами:

- **`NSUserActivity` и Schema.org** - `NSUserActivity` помогает системе понять, с какой информацией в данный момент работает пользователь на экране. Schema.org добавляет аналогичные возможности для веб-страниц.
- **Варианты расположения** . Если приложение предлагает или использует сведения на основе расположения, эти расширения API предлагают новые способы совместного использования этих сведений в приложениях.
- **Предложения для приложений мультимедиа** . система может повысить уровень приложения и его содержимого на основе контекста взаимодействия пользователя с устройством iOS.

И поддерживаются в приложении путем реализации следующего:

- ** - `NSUserActivity`** был добавлен в iOS 8 для поддержки передачи, что позволяет разработчику запускать действие на одном устройстве, а затем продолжать его на другом (см. статью [Общие сведения](~/ios/platform/handoff.md)о передаче).
- **Поиск в Spotlight** — в iOS 9 добавлена возможность повышения уровня содержимого приложения в результатах поиска Spotlight с помощью `NSUserActivity` (см. раздел [Поиск с ключевыми](~/ios/platform/search/corespotlight.md)сведениями о Core).
- **Контекстные напоминания Siri** . в iOS 10 `NSUserActivity` была расширена, чтобы позволить Siri быстро создать напоминание для просмотра содержимого, которое пользователь в настоящее время просматривает в приложении позже.
- **Предложения по расположению** . iOS 10 расширяет `NSUserActivity` для захвата мест, просматриваемых в приложении, и продвижения их во многих местах по всей системе.
- **Контекстные запросы Siri** - `NSUserActivity` предоставляет контекст для информации, представленной в приложении для Siri, чтобы пользователь мог получать направления или вызывать вызов Siri из приложения.
- **Взаимодействие с контактами** . Новое в iOS 10 `NSUserActivity` позволяет повысить уровень взаимодействия с контактной картой (в приложении "Контакты") в качестве альтернативного метода связи.

Все эти функции имеют одно общее значение, поэтому все они используют `NSUserActivity` в одной форме или другой для предоставления их функциональных возможностей. 

## <a name="nsuseractivity"></a>NSUserActivity

Как упоминалось выше, `NSUserActivity` помогает системе понять, с какой информацией в данный момент работает пользователь на экране. `NSUserActivity` — это облегченный механизм кэширования состояния, позволяющий захватывать действия пользователя по мере их перемещения по приложению. Например, рассмотрим приложение ресторана:

[![](proactive-suggestions-images/activity02.png "The NSUserActivity light-weight state caching mechanism")](proactive-suggestions-images/activity02.png#lightbox)

Со следующими взаимодействиями:

1. Когда пользователь работает с приложением, создается `NSUserActivity` для повторного создания состояния приложения позже.
2. Если пользователь выполняет поиск ресторана, то выполняется та же схема создания действий.
3. И снова, когда пользователь просматривает результат. В последнем случае, если пользователь просматривает расположение и в iOS 10, система более осведомлена о определенных понятиях (например, о расположении или взаимодействии связи).

Внимательно взгляните на последний экран:

[![](proactive-suggestions-images/activity03.png "The NSUserActivity details")](proactive-suggestions-images/activity03.png#lightbox)

Здесь приложение создает `NSUserActivity` и заполняет его сведениями для повторного создания состояния позже. Приложение также включало некоторые метаданные, такие как имя и адрес расположения. После создания этого действия приложение позволяет iOS понять, что оно представляет текущее состояние пользователя.

Затем приложение принимает решение о том, что действие будет объявлено при передаче, которое сохраняется как временное значение для предложений о расположении или добавляется в индекс Spotlight на устройстве для отображения в результатах поиска.

Дополнительные сведения о ведении и поиске Spotlight см. в статьях [Введение в руководства по](~/ios/platform/handoff.md) использованию [API-интерфейсов поиска в iOS 9](~/ios/platform/search/index.md) .

### <a name="creating-an-activity"></a>Создание действия

Перед созданием действия необходимо создать идентификатор типа действия для его идентификации. Идентификатор типа действия — это короткая строка, добавляемая в массив `NSUserActivityTypes` `Info.plist` файла приложения, используемого для уникальной идентификации данного типа действия пользователя. В массиве будет одна запись для каждого действия, которое поддерживает приложение и предоставляет поиск приложения. Дополнительные сведения см. в [справочнике по созданию идентификаторов типов действий](~/ios/platform/search/nsuseractivity.md) .

Рассмотрим пример действия:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Inform system of Activity
activity.BecomeCurrent();
```

Новое действие создается с помощью идентификатора типа действия. Затем создаются некоторые метаданные, определяющие действие, чтобы это состояние можно было восстановить позже. Затем действие получает значимое название и прикрепляется к сведениям о пользователе. Наконец, некоторые возможности включены и действие отправляется в систему.

Приведенный выше код можно дополнительно улучшить, включив в него метаданные, которые предоставляют контекст для действия, внося следующие изменения:

```csharp
...

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Inform system of Activity
activity.BecomeCurrent();
```

Если у разработчика есть веб-сайт, который может отображать те же сведения, что и приложение, приложение может включать URL-адрес, а содержимое может отображаться на других устройствах, на которых не установлено приложение (через "переносить"):

```csharp
// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");
```

### <a name="restoring-an-activity"></a>Восстановление действия

Чтобы ответить пользователю, коснувшись результата поиска (`NSUserActivity`) для приложения, измените файл **AppDelegate.CS** и переопределите метод `ContinueUserActivity`. Пример:

```csharp
public override bool ContinueUserActivity (UIApplication application, NSUserActivity userActivity, UIApplicationRestorationHandler completionHandler)
{

    // Take action based on the activity type
    switch (userActivity.ActivityType) {
    case "com.xamarin.platform":
        // Restore the state of the app here...
        break;
    }

    return true;
}
```

Разработчику необходимо убедиться, что это тот же идентификатор типа действия (`com.xamarin.platform`), что и созданное ранее действие. Приложение использует сведения, хранящиеся в `NSUserActivity`, для восстановления состояния, в котором пользователь вышел из системы.

### <a name="benefits-of-creating-an-activity"></a>Преимущества создания действия

С минимальным объемом приведенного выше кода приложение теперь может воспользоваться преимуществами трех новых функций iOS 10:

- **Handoff**
- **Поиск Spotlight**
- **Контекстные напоминания Siri**

В следующем разделе мы рассмотрим включение двух других новых функций iOS 10:

- **Варианты расположения**
- **Контекстные запросы Siri**

### <a name="location-based-suggestions"></a>Предложения на основе расположения 

Возьмем пример приложения для поиска ресторанов выше. Если он реализовал `NSUserActivity` и правильно заполнил все метаданные и атрибуты, пользователь сможет сделать следующее:

1. Найдите Ресторан в приложении, которое хотели бы встретиться с друзьями по адресу.
2. По мере того как пользователь перемещается из приложения с помощью многозадачного переключателя приложений, система автоматически отображает предложение (в нижней части экрана) для получения направлений о ресторане с помощью избранного приложения для навигации.
3. Если пользователь переключается в приложение для работы с сообщениями и начинает вводить *слово «давайте соблюдаем*», куикктипе клавиатура автоматически предлагает вставлять адрес ресторана.
4. Если пользователь переключается на приложение Maps, адрес ресторана автоматически предлагается в качестве назначения.
5. Это даже работает для сторонних приложений (которые поддерживают `NSUserActivity`), поэтому пользователь может переключиться на приложение с общим доступом, и адрес ресторана автоматически предлагается в качестве назначения.
6. Он также предоставляет контекст для Siri, поэтому пользователь может вызвать Siri в приложении ресторана и спросить *«Get Directions...»* , и Siri предоставит направления для ресторана, который просматривает пользователь.

Все функции, описанные выше, имеют один общий характер, и все они указывают, откуда поступило предложение. В примере выше это вымышленное приложение для проверки ресторана.

в iOS 10 добавлена возможность использования этой функции для приложения с помощью нескольких небольших изменений и дополнений к существующим платформам:

- `NSUserActivity` содержит дополнительные поля для записи сведений о местоположении, которые просматриваются в приложении.
- В Мапкит и Кореспотлигхт было внесено несколько дополнений для захвата расположения.
- Функции, учитывающие расположение, добавлены в Siri, карты, клавиатуры, многозадачность и другие приложения в системе.

Чтобы реализовать предложения на основе расположения, начните с того же кода действия, приведенного выше:

```csharp
// Create App Activity
var activity = new NSUserActivity ("com.xamarin.platform");

// Define details
var info = new NSMutableDictionary ();
info.Add(new NSString("link"),new NSString("http://xamarin.com/platform"));

// Populate Activity
activity.Title = "The Xamarin Platform";
activity.UserInfo = info;

// Enable capabilities
activity.EligibleForSearch = true;
activity.EligibleForHandoff = true;
activity.EligibleForPublicIndexing = true;

// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
attributes.ThumbnailUrl = myThumbnailURL;
attributes.Keywords = new string [] { "software", "mobile", "language" }; 
activity.ContentAttributeSet = attributes;

// Restore on the web
activity.WebPageUrl = new NSUrl("http://xamarin.com/platform");

// Inform system of Activity
activity.BecomeCurrent();
```

Если приложение использует Мапкит, то достаточно просто добавить текущую `MKMapItem` Map к действию:

```csharp
// Save MKMapItem location
activity.MapItem = myMapItem;
```

Если приложение не использует Мапкит, оно может внедрить Поиск приложения и указать следующие новые атрибуты для расположения:

```csharp
// Provide context
var attributes = new CSSearchableItemAttributeSet ("com.xamarin.location");
...

attributes.NamedLocation = "Apple Inc.";
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

Подробнее рассмотрим приведенный выше код. Во-первых, необходимо указать имя расположения в каждом экземпляре:

```csharp
attributes.NamedLocation = "Apple Inc.";
```

Затем текстовое описание расположения требуется для текстовых экземпляров (например, клавиатуры Куикктипе):

```csharp
attributes.SubThoroughfare = "1";
attributes.Thoroughfare = "Infinite Loop";
attributes.City = "Cupertino";
attributes.StateOrProvince = "CA";
attributes.Country = "United States";
```

Широту и долготу необязательны, но убедитесь, что пользователь направляется в точное расположение, куда приложение хочет отправить их, поэтому оно должно быть включено:

```csharp
attributes.Latitude = 37.33072;
attributes.Longitude = 122.029674;
```

Задавая номера телефонов, приложение может получить доступ к Siri, чтобы пользователь мог вызвать Siri из приложения, выполнив *примерно такую команду:*

```csharp
attributes.PhoneNumbers = new string[]{"(800) 275-2273"};
```

Наконец, приложение может указать, подходит ли экземпляр для навигации и телефонных звонков:

```csharp
attributes.SupportsPhoneCalls = true;
attributes.SupportsNavigation = true;
```

### <a name="implementing-contact-interactions"></a>Реализация взаимодействия с контактами

Новые приложения в iOS 10 тесно интегрируются в приложение Contacts с карточки контакта. Для приложений, которые реализовали взаимодействие с контактами, пользователь может добавить сведения о конкретном приложении для конкретных пользователей в своих контактах. Если, например, они нажимают кнопку быстрого действия в верхней части карточки для отправки сообщения, присоединенное приложение будет указано в качестве варианта отправки сообщения.

Если выбрано стороннее приложение, оно может быть сохранено и представлено по умолчанию для сообщения данному пользователю в следующий раз, когда ему нужно связаться.

Взаимодействие с контактами реализуется в приложении с помощью `NSUserActivity` и новой платформы для целей, представленной в iOS 10. Дополнительные сведения о работе с ними см. в статье основные [понятия SiriKit](~/ios/platform/sirikit/understanding-sirikit.md) и [Реализация SiriKit](~/ios/platform/sirikit/implementing-sirikit.md) руководств.

#### <a name="donating-interactions"></a>Пожертвование взаимодействий

Взгляните на то, как приложение может задействовать взаимодействие:

[![](proactive-suggestions-images/activity04.png "Donating Interactions overview")](proactive-suggestions-images/activity04.png#lightbox)

Приложение создает объект `INInteraction`, который содержит **намерение** (`INIntent`), **Участники** и **метаданные**. **Цель** представляет собой действие пользователя, например выполнение видеовызова или отправка текстового сообщения. **Участники** включают пользователей, получивших связь. **Метаданные** определяют дополнительные сведения, такие как успешная отправка сообщения и т. д.

Разработчик никогда не создает экземпляр `INIntent` или `INIntentResponse`, он будет использовать один из конкретных дочерних классов (на основе задачи, которую приложение выполняет от имени пользователя), которое наследует от этих родительских классов. Например, `INSendMessageIntent` и `INSendMessageIntentResponse` для отправки текстового сообщения. 

После полного заполнения взаимодействия вызовите метод `DonateInteraction`, чтобы сообщить системе о том, что взаимодействие доступно для использования.

Когда пользователь взаимодействует с приложением из карточки контакта, взаимодействие объединяется с `NSUserActivity`, который затем используется для запуска приложения:

[![](proactive-suggestions-images/activity05.png "The interaction gets bundled with a NSUserActivity that is used to launch the app")](proactive-suggestions-images/activity05.png#lightbox)

Рассмотрим следующий пример намерения отправить сообщение:

```csharp
using System;
using Foundation;
using UIKit;
using Intents;

namespace MonkeyNotification
{
  public class DonateInteraction
  {
    #region Constructors
    public DonateInteraction ()
    {
    }
    #endregion

    #region Public Methods
    public void SendMessageIntent (string text, INPerson from, INPerson[] to)
    {

      // Create App Activity
      var activity = new NSUserActivity ("com.xamarin.message");

      // Define details
      var info = new NSMutableDictionary ();
      info.Add (new NSString ("message"), new NSString (text));

      // Populate Activity
      activity.Title = "Sent MonkeyChat Message";
      activity.UserInfo = info;

      // Enable capabilities
      activity.EligibleForSearch = true;
      activity.EligibleForHandoff = true;
      activity.EligibleForPublicIndexing = true;

      // Inform system of Activity
      activity.BecomeCurrent ();

      // Create message Intent
      var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);

      // Create Intent Reaction
      var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);

      // Create interaction
      var interaction = new INInteraction (intent, response);

      // Donate interaction to the system
      interaction.DonateInteraction ((err) => {
        // Handle donation error
        ...
      });
    }
    #endregion
  }
}
```

Изучив этот код, он создает и заполняет экземпляр `NSUserActivity` (как показано в разделе [Создание действия](#creating-an-activity) выше). Затем создается экземпляр `INSendMessageIntent` (который наследуется от `INIntent`) и заполняется сведениями о отправляемом сообщении:

```csharp
var intent = new INSendMessageIntent (to, text, "", "MonkeyChat", from);
```

Создается `INSendMessageIntentResponse`, и ему передается созданная ранее `NSUserActivity`:

```csharp
var response = new INSendMessageIntentResponse (INSendMessageIntentResponseCode.Success, activity);
```

`INInteraction` строится на основе только что созданного действия Отправить сообщение (`INSendMessageIntent`) и ответ (`INSendMessageIntentResponse`):

```csharp
var interaction = new INInteraction (intent, response);
```

Наконец, система уведомляет о взаимодействии:

```csharp
// Donate interaction to the system
interaction.DonateInteraction ((err) => {
  // Handle donation error
  ...
});
```

Обработчик завершения передается в том месте, где приложение может реагировать на успешное или неудачное выполнение в пожертвовании.

### <a name="activities-best-practices"></a>Рекомендации по действиям

При работе с действиями компания Apple предлагает следующие рекомендации:

- Используйте `NeedsSave` для обновлений отложенных полезных данных.
- Обязательно обеспечьте строгую ссылку на текущее действие.
- Переносите небольшие полезные данные, которые содержат достаточно информации для восстановления состояния.
- Убедитесь, что идентификаторы типов действий являются уникальными и описательными, используя нотацию в обратную DNS, чтобы указать их. 

## <a name="schemaorg"></a>Schema.org

Как показано выше, `NSUserActivity` помогает системе понять, с какой информацией в данный момент работает пользователь на экране. Schema.org добавляет аналогичные возможности для веб-страниц.

Schema.org может предоставлять те же типы взаимодействий на веб-сайте, которые основаны на расположении. Компания Apple намерена работать с новыми предложениями о расположении, как и в Safari, так же как и в собственном приложении.

Schema.org фон:

- Он предоставляет открытый стандарт словаря веб-разметки.
- Он работает, включая структурированные метаданные на веб-страницах.
- Существует более 500 схем, представляющих различные доступные концепции.
- Реализовав его на веб-сайте, разработчик может получить некоторые преимущества использования `NSUserActivity` в собственном приложении.

Схемы упорядочиваются в виде дерева, такого как структура, где конкретные типы, такие как *Ресторан*, наследуются от более универсальных типов, таких как *локальный бизнес*. Дополнительные сведения см. в разделе [Schema.org](https://schema.org).

Например, если веб-страница включала следующие данные:

```html
<script type="application/ld+json">
{
  "@context":"http://schema.org",
  "@type":"Restaurant",
  "telephone":"(415) 781-1111",
  "url":"https://www.yanksing.com",
  "address":{
    "@type":"PostalAddress",
    "streetAddress":"101 Spear St",
    "addressLocality":"San Francisco",
    "postalCode":"94105",
    "addressRegion":"CA"
  },
  "aggregateRating":{
    "@type":"AggregateRating",
    "ratingValue":"3.5",
    "reviewCount":"2022"
  }
}
</script>
```

Если пользователь посетит эту страницу в Safari, а затем переключился на другое приложение, сведения о расположении со страницы будут сохранены и предложены в качестве предложения расположения в других частях системы (как показано в действиях выше).

Safari извлечет все данные на веб-странице, которые соответствуют любому из следующих свойств схемы:

- **посталаддресс**
- **Геокоординаты**
- Свойство телефона.

Дополнительные сведения см. [в нашем поиске с помощью веб-разметки](~/ios/platform/search/web-markup.md) .

## <a name="consuming-location-suggestions"></a>Использование предложений по расположению

В следующем разделе рассматривается использование предложения Location, полученного из других частей системы (например, приложения Maps) или других сторонних приложений.

Существует два основных способа, с помощью которых приложение может использовать варианты расположения:

- Через клавиатуру Куикктипе.
- Непосредственно, используя сведения о расположении в приложениях маршрутизации.

### <a name="location-suggestions-and-the-quicktype-keyboard"></a>Варианты расположения и клавиатура Куикктипе

Если приложение работает с адресами в текстовых форматах, приложение может воспользоваться предложениями по расположению через пользовательский интерфейс Куикктипе. iOS 10 расширяет пользовательский интерфейс Куикктипе, используя следующие возможности:

- Приложение может добавлять указания по семантическому намерению для текстовых полей в пользовательском интерфейсе.
- Приложение может получать упреждающие предложения в приложении.
- Приложение может воспользоваться преимуществами улучшенной автозамены.

Новое свойство `TextContentType` элементов управления "текстовое поле" в iOS 10 позволяет разработчику определить семантическую цель для значения, которое пользователь будет вводить в заданное поле. Пример:

```csharp
var textField = new UITextField();
textField.TextContentType = UITextContentType.FullStreetAddress;
```

Сообщает системе, что приложение предполагает, что пользователь введет полный почтовый адрес в заданном поле. Это позволит Куикктипе клавиатуре автоматически предоставлять варианты расположения на клавиатуре при вводе пользователем значения в этом поле.

Ниже приведены некоторые из наиболее распространенных типов, доступных разработчику в `UITextContentType` статическом классе:

- `Name`
- `GivenName`
- `FamilyName`
- `Location`
- `FullStreetAddress`
- `AddressCityAndState`
- `TelephoneNumber`
- `EmailAddress`

### <a name="routing-apps-and-locations-suggestions"></a>Предложения по маршрутизации приложений и расположений

В этом разделе рассматривается использование предложений о расположении непосредственно из приложения маршрутизации. Чтобы приложение маршрутизации добавили эту функцию, разработчик будет использовать имеющуюся `MKDirectionsRequest` платформу следующим образом:

- Для продвижения приложения в многозадачной области.
- Для регистрации приложения в качестве приложения маршрутизации.
- Для управления запуском приложения с помощью объекта `MKDirectionsRequest` Мапкит.
- Предоставить возможности iOS, чтобы предлагать приложению пользователю в нужное время в зависимости от участия пользователя.

Когда приложение запускается с помощью объекта Мапкит `MKDirectionsRequest`, оно должно автоматически начать перенаправление пользователя в запрошенное расположение или предоставить пользовательский интерфейс, который позволяет пользователю легко начать работу с инструкциями. Пример:

```csharp
using System;
using Foundation;
using UIKit;
using MapKit;
using CoreLocation;

namespace MonkeyChat
{
  [Register ("AppDelegate")]
  public class AppDelegate : UIApplicationDelegate, IUISplitViewControllerDelegate
  {
    ...
    
    public override bool OpenUrl (UIApplication app, NSUrl url, NSDictionary options)
    {
      if (MKDirectionsRequest.IsDirectionsRequestUrl (url)) {
        var request = new MKDirectionsRequest (url);
        var coordinate = request.Destination?.Placemark.Location?.Coordinate;
        var address = request.Destination.Placemark.AddressDictionary;
        if (coordinate.IsValid()) {
          var geocoder = new CLGeocoder ();
          geocoder.GeocodeAddress (address, (place, err) => {
            // Handle the display of the address

          });
        }
      }

      return true;
    }
  }    
}
```

Подробно изучите этот код. Он проверяет, является ли он допустимым запросом на назначение:

```csharp
if (MKDirectionsRequest.IsDirectionsRequestUrl(url)) {
```

Если это так, он создает `MKDirectionsRequest` из URL-адреса:

```csharp
var request = new MKDirectionsRequest(url);
```

Новое в iOS 10. приложение может отправлять адрес, у которого нет географических координат. в этом случае разработчику необходимо закодировать адрес:

```csharp
var geocoder = new CLGeocoder();
geocoder.GeocodeAddress(address, (place, err)=> {
  // Handle the display of the address
  
});

```

## <a name="media-app-suggestions"></a>Предложения для мультимедийных приложений

Если приложение обрабатывает любые типы носителей, такие как приложение подкастов, или мультимедийное содержимое, например аудио или видео, с iOS 10, оно может воспользоваться преимуществами мультимедийных предложений в системе.

Для приложений, обрабатывающих носители, iOS поддерживает следующие варианты поведения:

- iOS способствует приложениям, которые пользователь может использовать в зависимости от их предыдущего поведения.
- Предложения, связанные с приложением, будут представлены в центре внимания и в современном представлении.
- Если приложение соответствует одному из следующих триггеров, оно может быть расширено до предложения на экране блокировки:
  - После подключения наушников или устройства Bluetooth устанавливает подключение.
  - После получения автомобиля.
  - После поступающих дома или на работу. 

Включив простой вызов API в iOS 10, разработчик может создать более привлекательный интерфейс блокировки для пользователей приложения мультимедиа. Используя класс `MPPlayableContentManager` для управления воспроизведением мультимедиа, все элементы управления носителя (например, представленные музыкальным приложением) будут представлены на экране блокировки приложения.

```csharp
using System;
using MediaPlayer;
using UIKit;

namespace MonkeyPlayer
{
  public class PlayableContentDelegate : MPPlayableContentDelegate
  {
    #region Constructors
    public PlayableContentDelegate ()
    {
    }
    #endregion

    #region Override methods
    public override void InitiatePlaybackOfContentItem (MPPlayableContentManager contentManager, Foundation.NSIndexPath indexPath, Action<Foundation.NSError> completionHandler)
    {
      // Access the media item to play
      var item = LoadMediaItem (indexPath);

      // Populate the lock screen
      PopulateNowPlayingItem (item);

      // Prep item to be played
      var status = PreparePlayback (item);

      // Call completion handler
      completionHandler (null);
    }
    #endregion

    #region Public Methods
    public MPMediaItem LoadMediaItem (Foundation.NSIndexPath indexPath)
    {
      var item = new MPMediaItem ();

      // Load item from media store
      ...

      return item;
    }

    public void PopulateNowPlayingItem (MPMediaItem item)
    {
      // Get Info Center and album art
      var infoCenter = MPNowPlayingInfoCenter.DefaultCenter;
      var albumArt = (item.Artwork == null) ? new MPMediaItemArtwork (UIImage.FromFile ("MissingAlbumArt.png")) : item.Artwork;

      // Populate Info Center
      infoCenter.NowPlaying.Title = item.Title;
      infoCenter.NowPlaying.Artist = item.Artist;
      infoCenter.NowPlaying.AlbumTitle = item.AlbumTitle;
      infoCenter.NowPlaying.PlaybackDuration = item.PlaybackDuration;
      infoCenter.NowPlaying.Artwork = albumArt;
    }

    public bool PreparePlayback (MPMediaItem item)
    {
      // Prepare media item for playback
      ...

      // Return results
      return true;
    }
    #endregion
  }
}
```

## <a name="summary"></a>Сводка

В этой статье были рассмотрены упреждающие предложения и показано, как разработчик может использовать их для передачи трафика в приложение Xamarin. iOS. В нем рассмотрены действия по реализации упреждающего предложения и предоставлены рекомендации по использованию.

## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [SiriKit по программированию](https://developer.apple.com/library/prerelease/content/documentation/Intents/Conceptual/SiriIntegrationGuide/index.html)
