---
title: Создание службы
ms.prod: xamarin
ms.assetid: A78A55E7-FB5C-4C42-8E3E-939B5E98F9EB
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 05/03/2018
ms.openlocfilehash: d5b3f084be7adc664dcb52342af617788f4dde48
ms.sourcegitcommit: 6264fb540ca1f131328707e295e7259cb10f95fb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2019
ms.locfileid: "69526233"
---
# <a name="creating-a-service"></a>Создание службы

Службы Xamarin. Android должны соблюдать два правила непреодолимой служб Android:

* Они должны расширять [`Android.App.Service`](xref:Android.App.Service).
* Они должны быть дополнены [`Android.App.ServiceAttribute`](xref:Android.App.ServiceAttribute).

Еще одно требование к службам Android заключается в том, что они должны быть зарегистрированы в **файле AndroidManifest. XML** и иметь уникальное имя. Xamarin. Android будет автоматически регистрировать службу в манифесте во время сборки с помощью необходимого атрибута XML.

Этот фрагмент кода является простым примером создания службы в Xamarin. Android, которая соответствует следующим двум требованиям:  

```csharp
[Service]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Во время компиляции Xamarin. Android регистрирует службу, внедряя следующий XML-элемент в **AndroidManifest. XML** (Обратите внимание, что Xamarin. Android создал случайное имя для службы):

```xml
<service android:name="md5a0cbbf8da641ae5a4c781aaf35e00a86.DemoService" />
```

Можно предоставить общий доступ к службе другим приложениям Android, _экспортировав_ ее. Это достигается путем задания `Exported` свойства `ServiceAttribute`для. При экспорте службы `ServiceAttribute.Name` свойство также должно быть установлено, чтобы предоставить понятное общедоступное имя для службы. В этом фрагменте кода показано, как экспортировать и присвоить имя службе:

```csharp
[Service(Exported=true, Name="com.xamarin.example.DemoService")]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things.
}
```

Затем элемент **AndroidManifest. XML** для этой службы будет выглядеть примерно так:

```xml
<service android:exported="true" android:name="com.xamarin.example.DemoService" />
```

Службы имеют собственный жизненный цикл с методами обратного вызова, которые вызываются при создании службы. Точно, какие методы вызываются, зависит от типа службы. Запущенная служба должна реализовать разные методы жизненного цикла, чем привязанная служба, в то время как гибридная служба должна реализовать методы обратного вызова как для запущенной, так и для привязанной службы. Эти методы являются членами `Service` класса; способ запуска службы определяет, какие методы жизненного цикла будут вызываться. Эти методы жизненного цикла будут обсуждаться более подробно.

По умолчанию служба запускается в том же процессе, что и приложение Android. Можно запустить службу в собственном процессе, задав `ServiceAttribute.IsolatedProcess` свойству значение true:

```csharp
[Service(IsolatedProcess=true)]
public class DemoService : Service
{
    // Magical code that makes the service do wonderful things, in it's own process!
}
```

Следующим шагом является проверка того, как запустить службу, а затем перейти к рассмотрению реализации трех различных типов служб.

> [!NOTE]
> Служба выполняется в потоке пользовательского интерфейса, поэтому при выполнении любой работы, блокирующей пользовательский интерфейс, служба должна использовать потоки для выполнения работы.

## <a name="starting-a-service"></a>Запуск службы

Самый простой способ запуска службы в Android — отправка `Intent` , которая содержит метаданные, чтобы определить, какая служба должна быть запущена. Существует два разных стиля целей, которые можно использовать для запуска службы:

- **Явное намерение** Явная цель определяет, какая именно служба должна использоваться для выполнения данного действия. &ndash; Явная цель может рассматриваться как буква с указанным адресом. Android перенаправит цель в явно определенную службу. Этот фрагмент кода является одним из примеров использования явной намерений для запуска службы `DownloadService`с именем:

    ```csharp
    // Example of creating an explicit Intent in an Android Activity
    Intent downloadIntent = new Intent(this, typeof(DownloadService));
    downloadIntent.data = Uri.Parse(fileToDownload);
    ```

- **Неявная блокировка** &ndash; Этот тип намерения слабо определяет действие, которое пользователь хочет выполнить, но точная служба для выполнения этого действия неизвестна. Неявная цель может рассматриваться как буква, с которой оно может быть связано...».
    Android проверит содержимое намерения и определит, существует ли уже существующая служба, которая соответствует намерениям.

    _Фильтр_ с намерением используется для обеспечения соответствия неявной цели зарегистрированной службе. Фильтр с намерением — это XML-элемент, который добавляется в **AndroidManifest. XML** , который содержит необходимые метаданные, чтобы помочь сопоставить службу с неявным намерением.

    ```csharp
    Intent sendIntent = new Intent("common.xamarin.DemoService");
    sendIntent.Data = Uri.Parse(fileToDownload);
    ```

Если в Android имеется несколько возможных соответствий для неявной цели, может потребоваться, чтобы пользователь выбрал компонент для выполнения действия:

![Снимок экрана с диалоговым окном] устранения неоднозначности (images/creating-a-service-01.png "Снимок экрана с диалоговым окном") устранения неоднозначности

> [!IMPORTANT]
> Начиная с Android 5,0 (AP Level 21) неявная цель не может быть использована для запуска службы.

Там, где это возможно, приложения должны использовать явные способы для запуска службы. Неявная цель не требует от Вас запрашивать запуск &ndash; определенной службы — это запрос на обработку некоторых служб на устройстве для выполнения запроса. Такой неоднозначный запрос может привести к неправильной обработке запроса или запуску другого приложения (что увеличивает нагрузку на устройство).

Способ отправки намерения зависит от типа службы и будет обсуждаться более подробно далее в руководствах, относящихся к каждому типу службы.


### <a name="creating-an-intent-filter-for-implicit-intents"></a>Создание фильтра намерения для неявных целей

Чтобы связать службу с неявным намерением, приложение Android должно предоставить некоторые метаданные для выявления возможностей службы. Эти метаданные предоставляются _фильтрами намерения_. Фильтры намерения содержат некоторую информацию, например действие или тип данных, которые должны присутствовать в намерениях запустить службу. В Xamarin. Android фильтр намерения регистрируется в **AndroidManifest. XML** путем оформления службы с [`IntentFilterAttribute`](xref:Android.App.IntentFilterAttribute)помощью. Например, следующий код добавляет фильтр намерения со связанным действием `com.xamarin.DemoService`:

```csharp
[Service]
[IntentFilter(new String[]{"com.xamarin.DemoService"})]
public class DemoService : Service
{
}
```

В результате в файл &ndash; **AndroidManifest. XML** добавляется запись, которая упаковывается в приложение способом, аналогичным следующему примеру:

```xml
<service android:name="demoservice.DemoService">
    <intent-filter>
        <action android:name="com.xamarin.DemoService" />
    </intent-filter>
</service>
```

Изменяя основы службы Xamarin. Android, давайте более подробно рассмотрим различные подтипы служб.


## <a name="related-links"></a>Связанные ссылки

- [Android. app. Service](xref:Android.App.Service)
- [Android.App.ServiceAttribute](xref:Android.App.ServiceAttribute)
- [Android. app. намерение](xref:Android.Content.Intent)
- [Android.App.IntentFilterAttribute](xref:Android.App.IntentFilterAttribute)
