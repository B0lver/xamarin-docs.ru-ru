---
title: "fastlane для iOS — sigh"
ms.topic: article
ms.prod: xamarin
ms.assetid: 92B35AB1-7AB7-3D3B-DB31-CC971E0B43AE
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 3d80a0ab5583231f95241fb8d4f6e339e44a84ca
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="fastlane-for-ios--sigh"></a>fastlane для iOS — sigh

> [!IMPORTANT]
> fastlane рекомендует использовать [`match`](~/ios/deploy-test/provisioning/fastlane/match.md) для создания и обслуживания профилей подготовки. sigh следует использовать только в том случае, если требуются полный контроль и достаточный объем сведений о подписывании кода.

## <a name="overview"></a>Обзор

Как правило, подготовка устройств осуществляется каждым членом команды разработчиков с помощью Xcode или на портале Apple Developer. Этот процесс состоит из нескольких шагов:

- Запрос сертификата разработки
- Добавление устройства на портал
- Создание идентификатора приложения
- Создание профиля подготовки
- Скачивание профилей и сертификатов

На каждом из этих этапов требуется учитывать определенные переменные в зависимости от типа разрабатываемого приложения. Дополнительные сведения об этапах настройки устройства для разработки вручную или с помощью Xcode см. в [руководстве по подготовке устройств](~/ios/get-started/installation/device-provisioning/index.md).

В этом руководстве описываются средства fastlane, которые можно использовать вместо Xcode, и приводится информация по следующим вопросам:

- [Что такое sigh?](#whatissigh)
- [Создание идентификатора приложения](#appid)
- [Добавление новых устройств](#newdevices)
- [Использование sigh](#using)
- [Дополнительные параметры](#options)

> [!NOTE]
> Если у вас есть существующий идентификатор приложения, соответствующий идентификатору пакета приложения, а также если ваше устройство существует на портале разработчиков, вы можете пропустить действия, приведенные для этапов [Создание идентификатора приложения](#appid) и [Добавление устройства](#newdevices). В таких случаях работу следует начинать сразу с этапа [Использование sigh](#using).

## <a name="installation"></a>Установка

Дополнительные сведения об установке fastlane см. в ознакомительном руководстве по [fastlane](~/ios/deploy-test/provisioning/fastlane/index.md#Installation).

<a name="whatissigh" />

## <a name="what-is-sigh"></a>Что такое sigh?

sigh реализует интерфейс терминала, позволяющий создавать и обновлять профили подготовки для всех конфигураций: разработка, распространение через App Store, прямое распространение и корпоративное распространение. Кроме того, это средство реализует простой способ для скачивания и восстановления профилей подготовки.

<a name="appid" />

## <a name="creating-an-app-id"></a>Создание идентификатора приложения

Идентификатор приложения можно создать с помощью следующей команды:

    fastlane produce -u your@appleid.com -a com.company.appname --skip_itc

Здесь `com.company.appname` — это идентификатор пакета приложения, который находится в файле Info.plist вашего приложения Xamarin.iOS, как показано ниже:

[ ![](sigh-images/fastlane-image5.png "Файл Info.plist приложения Xamarin.iOS")](sigh-images/fastlane-image5.png)

Уникальный идентификатор приложения должен представлять собой строку в стиле обратного запроса DNS. Сохраните созданный идентификатор в удобной форме, чтобы использовать его в дальнейшем при работе с sigh в рамках этого руководства.

Если требуется создать приложение для [iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md), удалите флаг `--skip_itc` из приведенной выше команды.

<a name="newdevices" />

## <a name="adding-new-devices"></a>Добавление новых устройств

Чтобы добавить одно устройство на портал разработчиков из командной строки, введите следующую команду:

```bash
fastlane run register_device name:"Adam iPhone" udid:"abcdeg1234567"
```

Для добавления нескольких устройств используйте команду `register_devices`:

```bash
    register_devices(
        devices: {
            "iPhone 6" => "1234567890123456789012345678901234567890",
            "iPad Air 2" => "abcdefghijklmnopqrstvuwxyzabcdefghijklmn"
         }
    )
```

<a name="using" />

## <a name="using-sigh"></a>Использование sigh

Чтобы начать работу со служебной программой sigh, введите следующую команду в терминале:

```bash
fastlane sigh
```

По умолчанию при этом будет создан профиль подготовки [Распространение через App Store](~/ios/deploy-test/app-distribution/app-store-distribution/index.md). Чтобы настроить устройство для разработки, передайте флаг `--development`:

```bash
fastlane sigh --development
```

При появлении соответствующего запроса fastlane введите имя пользователя Apple ID. Если вы используете fastlane в первый раз, также может появиться запрос на ввод пароля. В остальных случаях пароль извлекается из переменной среды в цепочке ключей.

Если ваш Apple ID подключен к нескольким группам, они будут отображаться здесь. Выберите номер команды, которую вы хотите использовать:

[ ![](sigh-images/fastlane-image2.png "Выбор используемой команды")](sigh-images/fastlane-image2.png)

Идентификатор команды также можно передать в интерфейс командной строки следующим образом:

```bash
fastlane sigh -l 2TU993NY9J
```

Введите [идентификатор приложения](#appid). Обратите внимание, что он должен соответствовать идентификатору пакета, который задан в файле Info.plist вашего приложения.

В профиль подготовки будут добавлены все устройства, подключенные к вашей учетной записи.

После этого fastlane создаст, скачает и установит профиль подготовки.

Новый профиль подготовки будет виден при просмотре центра разработчиков, как показано на рисунке ниже:

[ ![](sigh-images/fastlane-image10.png "Просмотр нового профиля подготовки")](sigh-images/fastlane-image10.png)

По умолчанию sigh сохраняет профили подготовки в текущей папке. Чтобы изменить каталог вывода, задайте новое значение `output_path` или выполните следующие действия:

```bash
fastlane sigh -o "~/Library/MobileDevice/Provisioning Profiles"
```

<a name="options" />

## <a name="sigh-additional-options"></a>Дополнительные параметры sigh

Для получения дополнительной поддержки при работе с sigh можно использовать следующие параметры:

- Скачивание всех профилей подготовки:

    ````bash
    fastlane sigh download_all
    ```

- To use a specific signing identity for your provisioning profile use:

    ```bash
    fastlane sigh -c "Amy cert"
    ```
    
    Where `Amy cert` is the Code Signing Identity name.


## Related Links

- [fastlane - sigh](https://github.com/fastlane/fastlane/tree/master/sigh#readme)
