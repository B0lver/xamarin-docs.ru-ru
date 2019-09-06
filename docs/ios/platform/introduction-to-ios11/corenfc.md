---
title: Ядро NFC в Xamarin. iOS
description: В этом документе описывается чтение тегов связи ближнего действия в Xamarin. iOS с помощью API, появившихся в iOS 11.
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 846B59D3-F66A-48F3-A78C-84217697194E
author: conceptdev
ms.author: crdun
ms.date: 09/25/2017
ms.openlocfilehash: 6da32e22fbdb3b5b7d96d7ee93c2f25bba84cd78
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70286629"
---
# <a name="core-nfc-in-xamarinios"></a>Ядро NFC в Xamarin. iOS

_Чтение тегов NFC с помощью iOS 11_

Коренфк — это новая платформа в iOS 11, которая предоставляет доступ к радиоприемнику _Ближнего_ действия (NFC) для чтения тегов в приложениях. Он работает на устройствах iPhone 7, 7 плюс, 8, 8 Plus и X.

Средство чтения тегов NFC на устройствах iOS поддерживает все типы NFC-тегов от 1 до 5, содержащие сведения о _формате обмена данными NFC_ (ндеф).

Существуют некоторые ограничения, которые следует учитывать:

- Коренфк поддерживает только чтение тегов (не запись и форматирование).
- Сканирование тегов должно быть инициировано пользователем, а время ожидания — через 60 секунд.
- Приложения должны отображаться на переднем плане для сканирования.
- Коренфк можно тестировать только на реальных устройствах (не в симуляторе).

На этой странице описывается конфигурация, необходимая для использования Коренфк, и демонстрируется использование API с помощью [примера кода "нфктагреадер"](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader).

## <a name="configuration"></a>Конфигурация

Чтобы включить Коренфк, необходимо настроить три элемента в проекте:

- Ключ конфиденциальности **info. plist** .
- Запись прав **. plist** .
- Профиль подготовки с возможностью **чтения тегов NFC** .

### <a name="infoplist"></a>Info.plist

Добавьте ключ и текст о конфиденциальности **нфкреадерусажедескриптион** , который отображается для пользователя во время сканирования. Используйте сообщение, подходящее для вашего приложения (например, Объясните цель проверки):

```xml
<key>NFCReaderUsageDescription</key>
<string>NFC tag to read NDEF messages into the application</string>
```

### <a name="entitlementsplist"></a>Entitlements.plist

Приложение должно запросить возможность **считывания тегов связи ближнего поля** , используя следующую пару "ключ-значение" в своих правах **. plist**:

```xml
<key>com.apple.developer.nfc.readersession.formats</key>
<array>
  <string>NDEF</string>
</array>
```

### <a name="provisioning-profile"></a>Профиль подготовки

Создайте новый **идентификатор приложения** и убедитесь, что служба **чтения тегов NFC** имеет импульсные показания:

[![Страница "новый идентификатор приложения" портала разработчика с выбранным чтением NFC-тегов](corenfc-images/app-services-nfc-sml.png)](corenfc-images/app-services-nfc.png#lightbox)

Затем необходимо создать новый профиль подготовки для этого идентификатора приложения, а затем скачать и установить его на компьютере Mac для разработки.

## <a name="reading-a-tag"></a>Чтение тега

После настройки проекта добавьте `using CoreNFC;` в начало файла и выполните следующие три шага, чтобы реализовать функцию чтения тегов NFC:

### <a name="1-implement-infcndefreadersessiondelegate"></a>1. Применить`INFCNdefReaderSessionDelegate`

Интерфейс имеет два метода для реализации:

- `DidDetect`— Вызывается при успешном считывании тега.
- `DidInvalidate`— Вызывается при возникновении ошибки или при достижении времени ожидания 60 секунд.

#### <a name="diddetect"></a>диддетект

В примере кода каждое сканированное сообщение добавляется в табличное представление:

```csharp
public void DidDetect(NFCNdefReaderSession session, NFCNdefMessage[] messages)
{
    foreach (NFCNdefMessage msg in messages)
    {  // adds the messages to a list view
        DetectedMessages.Add(msg);
    }
    DispatchQueue.MainQueue.DispatchAsync(() =>
    {
        this.TableView.ReloadData();
    });
}
```

Этот метод может вызываться несколько раз (и может передаваться массив сообщений), если в сеансе разрешено несколько операций чтения тегов. Этот параметр задается с помощью третьего параметра `Start` метода (см. [Шаг 2](#step2)).

#### <a name="didinvalidate"></a>дидинвалидате

Недействительность может возникать по ряду причин.

- Произошла ошибка при сканировании.
- Приложение перестало быть на переднем плане.
- Пользователь выбрал отмену сканирования.
- Проверка была отменена приложением.

В приведенном ниже коде показано, как обрабатывалась ошибка:

```csharp
public void DidInvalidate(NFCNdefReaderSession session, NSError error)
{
    var readerError = (NFCReaderError)(long)error.Code;
    if (readerError != NFCReaderError.ReaderSessionInvalidationErrorFirstNDEFTagRead &&
        readerError != NFCReaderError.ReaderSessionInvalidationErrorUserCanceled)
    {
      // some error handling
    }
}
```

После того как сеанс станет недействительным, необходимо создать новый объект сеанса для повторной проверки.

<a name="step2" />

### <a name="2-start-an-nfcndefreadersession"></a>2. Запуск`NFCNdefReaderSession`

Сканирование должно начаться с запроса пользователя, например нажатия кнопки.
Следующий код создает и запускает сеанс сканирования:

```csharp
Session = new NFCNdefReaderSession(this, null, true);
Session?.BeginSession();
```

Ниже приведены параметры для `NFCNdefReaderSession` конструктора.

- `delegate`— Реализация `INFCNdefReaderSessionDelegate`. В примере кода делегат реализуется в контроллере представления таблицы, поэтому `this` используется в качестве параметра делегата.
- `queue`— Очередь, в которой обрабатываются обратные вызовы. Это может быть `null`, в этом случае следует `DispatchQueue.MainQueue` использовать при обновлении элементов управления пользовательского интерфейса (как показано в примере).
- `invalidateAfterFirstRead`— Когда `true`, сканирование останавливается после первого успешного сканирования; при `false` продолжении сканирования будет выполнено несколько результатов, пока сканирование не будет отменено или не будет достигнуто время ожидания 60 секунд.


### <a name="3-cancel-the-scanning-session"></a>3. Отмена сеанса сканирования

Пользователь может отменить сеанс сканирования с помощью предоставляемой системой кнопки в пользовательском интерфейсе:

![Кнопка "Отмена" при сканировании](corenfc-images/scan-cancel-sml.png)

Приложение может программно отменить сканирование, вызвав `InvalidateSession` метод:

```csharp
Session.InvalidateSession();
```

В обоих случаях будет вызван `DidInvalidate` метод делегата.

## <a name="summary"></a>Сводка

Коренфк позволяет приложению читать данные из NFC. Он поддерживает чтение различных форматов тегов (НДЕФ типы от 1 до 5), но не поддерживает запись и форматирование.


## <a name="related-links"></a>Связанные ссылки

- [Нфктагреадер (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-nfctagreader)
- [Знакомство с ядром NFC (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/718/)
