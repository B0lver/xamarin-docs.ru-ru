---
title: Службы, запущенные с помощью Xamarin. Android
ms.prod: xamarin
ms.assetid: 8CC3A850-4CD2-4F93-98EE-AF3470794000
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: 6075b125f36625a8dec12c041631e3794a71cc6a
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455370"
---
# <a name="started-services-with-xamarinandroid"></a>Службы, запущенные с помощью Xamarin. Android

## <a name="started-services-overview"></a>Обзор служб запущен

Запущенные службы обычно выполняют единицу работы, не предоставляя клиенту никаких прямых отзывов или результатов. Примером единицы работы является служба, которая передает файл на сервер. Клиент выполнит запрос к службе, чтобы передать файл с устройства на веб-сайт. Служба будет без загрузок загружать файл (даже если приложение не имеет действий на переднем плане) и завершать его после завершения передачи. Важно понимать, что запущенная служба будет запускаться в потоке пользовательского интерфейса приложения. Это означает, что если служба выполняет работу, которая блокирует поток пользовательского интерфейса, при необходимости она должна создавать и удалять потоки.

В отличие от привязанной службы, канал связи между "чистой" запущенной службой и ее клиентами не существует. Это означает, что запущенная служба будет реализовывать некоторые другие методы жизненного цикла, чем привязанная служба. В следующем списке перечислены распространенные методы жизненного цикла в запущенной службе.

- `OnCreate`&ndash;Вызывается один раз при первом запуске службы. Здесь следует реализовать код инициализации.
- `OnBind`&ndash;Этот метод должен быть реализован всеми классами служб, однако запущенная служба обычно не привязана к клиенту. Поэтому запущенная служба просто возвращает `null` . В отличие от этого, гибридная служба (которая является сочетанием привязанной службы и запущенной службы) должна реализовывать и возвращать `Binder` для клиента.
- `OnStartCommand`&ndash;Вызывается для каждого запроса на запуск службы либо в ответ на вызов `StartService` или перезагрузку системой. Именно в этом случае служба может запустить любую длительную задачу. Метод возвращает  `StartCommandResult` значение, указывающее, должна ли система перезапускать службу после завершения работы из-за нехватки памяти. Этот вызов выполняется в основном потоке. Этот метод более подробно описан ниже.
- `OnDestroy`&ndash;Этот метод вызывается при уничтожении службы. Он используется для выполнения любой окончательной очистки.

Важным методом для запущенной службы является `OnStartCommand` метод. Он будет вызываться каждый раз, когда служба получает запрос для выполнения некоторой работы. Ниже приведен пример фрагмента кода `OnStartCommand` . 

```csharp
public override StartCommandResult OnStartCommand (Android.Content.Intent intent, StartCommandFlags flags, int startId)
{
    // This method executes on the main thread of the application.
    Log.Debug ("DemoService", "DemoService started");
    ...
    return StartCommandResult.Sticky;
}
```

Первый параметр — это `Intent` объект, содержащий метаданные о выполняемой работе. Второй параметр содержит `StartCommandFlags` значение, которое предоставляет некоторую информацию о вызове метода. Этот параметр имеет одно из двух возможных значений:

- `StartCommandFlag.Redelivery`&ndash;Это означает, что `Intent` является повторной доставкой предыдущего `Intent` . Это значение указывается, когда служба была возвращена, `StartCommandResult.RedeliverIntent` но была остановлена, прежде чем она сможет правильно завершить работу.
-`StartCommandFlag.Retry`&dash;Это значение получается, когда `OnStartCommand` произошел сбой предыдущего вызова, и Android пытается запустить службу повторно с тем же намерением, что и предыдущая попытка неудачной попытки.

Наконец, третий параметр — это целочисленное значение, уникальное для приложения, идентифицирующего запрос. Возможно, несколько вызывающих объектов могут вызывать один и тот же объект службы. Это значение используется для связывания запроса на прерывание службы с заданным запросом для запуска службы. Более подробно он будет обсуждаться в разделе [Остановка службы](#Stopping_the_Service). 

Значение `StartCommandResult` возвращается службой в качестве предложения для Android на действиях, выполняемых в случае, если служба прервана из-за ограничений ресурсов. Существует три возможных значения для `StartCommandResult` :

- **[Старткоммандресулт. нотстикки.](xref:Android.App.StartCommandResult.NotSticky)** &ndash; это значение указывает Android, что не требуется перезапускать службу, которая была уничтожена. В качестве примера рассмотрим службу, которая создает эскизы для коллекции в приложении. Если служба будет уничтожена, то не очень важно повторно создавать эскиз немедленно, так как &ndash; эскиз можно создать заново при следующем запуске приложения.
- **[Старткоммандресулт. закрепление](xref:Android.App.StartCommandResult.Sticky)** &ndash; указывает, что Android перезапускает службу, но не для предоставления последней цели, которая запустила службу. Если нет ожидающих целей для обработки, для `null` параметра намерения будет предоставлено значение. Примером может быть приложение музыкального проигрывателя; Служба будет перезапущена для воспроизведения музыки, но она будет играть последнюю песню.
- **[Старткоммандресулт. ределиверинтент.](xref:Android.App.StartCommandResult.RedeliverIntent)** &ndash; это значение указывает Android перезапустить службу и повторно доставить последнюю `Intent` . Примером этого является служба, которая скачивает файл данных для приложения. Если служба уничтожена, файл данных по-прежнему необходимо скачать. После возврата `StartCommandResult.RedeliverIntent` , когда Android перезапускает службу, она также предоставит намерение (содержащее URL-адрес загружаемого файла) в службу. Это позволит загрузить или перезапустить или возобновить (в зависимости от точной реализации кода).

Имеется четвертое значение для `StartCommandResult` &ndash; `StartCommandResult.ContinuationMask` . Это значение возвращается в `OnStartCommand` и описывает, как Android продолжит работу службы, которая была остановлена. Это значение обычно не используется для запуска службы.

Основные события жизненного цикла запущенной службы показаны на этой схеме: 

![Схема, показывающая порядок вызова методов жизненного цикла](started-services-images/started-service-01.png "Схема, показывающая порядок вызова методов жизненного цикла.")

<a name="Stopping_the_Service"></a>

## <a name="stopping-the-service"></a>Остановка службы

Запущенная служба будет оставаться в состоянии неограниченно долго. Android продолжит работу службы, пока достаточно системных ресурсов. Клиент должен завершить работу службы, или служба может завершить свою работу после ее завершения. Существует два способа отключения службы. 

- **[Android. Content. Context. не ()](xref:Android.Content.Context.StopService*)** &ndash; Клиент (например, действие) может запросить завершение службы, вызвав `StopService` метод:

    ```csharp
    StopService(new Intent(this, typeof(DemoService));
    ```

- **[Android. app. Service. стопселф ()](xref:Android.App.Service.StopSelf*)** &ndash; Служба может завершить свою работу, вызвав `StopSelf` :

    ```csharp
    StopSelf();
    ```

### <a name="using-startid-to-stop-a-service"></a>Использование Стартид для завершения службы

Несколько вызывающих объектов могут запросить запуск службы. Если имеется необработанный запрос на запуск, служба может использовать объект `startId` , переданный в, `OnStartCommand` чтобы предотвратить преждевременное прекращение работы службы. `startId`Будет соответствовать последнему вызову функции `StartService` и будет увеличиваться каждый раз при вызове. Таким образом, если последующий запрос `StartService` еще не привел к вызову `OnStartCommand` , служба может вызвать `StopSelfResult` , передав ей Последнее `startId` полученное значение (а не просто вызов `StopSelf` ). Если вызов `StartService` еще не привел к вызову `OnStartCommand` , система не будет прекращать работу службы, так как `startId` используемая в `StopSelf` вызове не будет соответствовать последнему `StartService` вызову.

## <a name="related-links"></a>Связанные ссылки

- [Стартедсервицесдемо (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-servicesamples-startedservicesdemo)
- [Android. app. Service](xref:Android.App.Service)
- [Android. app. Старткоммандфлагс](xref:Android.App.StartCommandFlags)
- [Android. app. Старткоммандресулт](xref:Android.App.StartCommandResult)
- [Android. Content. BroadcastReceiver](xref:Android.Content.BroadcastReceiver)
- [Android. Content. намерения](xref:Android.Content.Intent)
- [Android. OS. Handler](xref:Android.OS.Handler)
- [Android. widget. всплывающее](xref:Android.Widget.Toast)
- [Значки строки состояния](https://developer.android.com/guide/practices/ui_guidelines/icon_design_status_bar.html)