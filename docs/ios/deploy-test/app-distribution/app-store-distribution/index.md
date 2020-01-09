---
title: Распространение через App Store
description: Этот документ описывает, как распространять приложение Xamarin.iOS через App Store. Он описывает, как создать сертификат распространения, создать профиль подготовки для распространения, а также настроить iTunes Connect и отправить приложение.
ms.prod: xamarin
ms.assetid: B07E2C1F-A6DF-43CB-BFB0-0252A5558467
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/23/2017
ms.openlocfilehash: 96300cdddcc21511ef85ebbfe33f6ce32cb56cd7
ms.sourcegitcommit: 6f09bc2b760e76a61a854f55d6a87c4f421ac6c8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/02/2020
ms.locfileid: "75607936"
---
# <a name="app-store-distribution"></a>Распространение через App Store

После разработки приложения Xamarin.iOS наступает следующий этап жизненного цикла разработки ПО — распространение приложения среди пользователей с помощью Tunes App Store. Это наиболее популярный способ распространения приложений. Опубликовав приложение в Apple App Store, его можно сделать доступным для потребителей по всему миру.

> [!IMPORTANT]
> Корпорация Apple [объявила](https://developer.apple.com/ios/submit/), что начиная с марта 2019 г. все публикуемые в App Store приложения и обновления должны быть собраны с использованием пакета SDK для iOS 12.1 или более поздних версий, входящего в Xcode версии 10.1 и выше.
> Кроме того, приложения должны поддерживать размеры экранов iPhone XS и iPad Pro с диагональю 12,9 дюйма.

Распространение приложения, как и его разработка, требует подготовки приложений с помощью соответствующего *профиля*. Профили подготовки — это файлы, содержащие сведения о подписывании кода, а также идентификатор приложения и подходящий механизм распространения. Для распространения не через App Store они также содержат сведения о том, на каких устройствах можно развертывать приложения.

> [!IMPORTANT]
> Для использования iTunes Connect и, соответственно, публикации приложения в App Store вам **нужно** участвовать в программе для разработчиков Apple в индивидуальном порядке или в составе организации. Вы не сможете выполнить следующие шаги на этой странице, если являетесь участником программы для разработчиков Apple Developer **Enterprise** Program.

<a name="provisioning" />

## <a name="provisioning-an-app-for-app-store-distribution"></a>Подготовка приложения для распространения через App Store

Как бы вы ни планировали развернуть приложение Xamarin.iOS, для него нужно создать *профиль подготовки для распространения*. Этот профиль позволяет подписать приложение цифровой подписью для выпуска, чтобы его можно было установить на устройстве iOS. По аналогии с профилем подготовки к разработке профиль распространения будет содержать следующее:

- Идентификатор приложения
- Сертификат распространения

Вы можете выбрать те же значения для параметров **Идентификатор приложения** и **Устройства**, которые использовали для профиля подготовки к разработке, но если у вас его еще нет, нужно создать сертификат распространения для идентификации вашей организации при отправке приложения в магазин приложений. В следующем разделе описаны шаги для создания сертификата распространения.

> [!NOTE]
> Создавать сертификаты распространения и профили подготовки могут только агенты команды и администраторы.

<a name="creatingcertificate" />

## <a name="creating-a-distribution-certificate"></a>Создание сертификата распространения

1. Перейдите к разделу *Certificates, Identifiers & Profiles* (Сертификаты, профили и идентификаторы) в Центре разработчиков Apple.
2. В разделе *Certificates* (Сертификаты) выберите **Production** (Производство).
3. Нажмите кнопку **+** , чтобы создать новый сертификат.
4. В разделе *Production* (Производство) установите флажок **App Store and Ad Hoc** (Магазин приложений и специальный):

    [![](images/createcertmanually01.png "Select App Store and Ad Hoc")](images/createcertmanually01.png#lightbox)
5. Нажмите кнопку **Continue** (Продолжить) и следуйте инструкциям, чтобы создать запрос подписи сертификата через доступ к цепочке ключей:

    [![](images/createcertmanually02.png "Create a Certificate Signing Request via Keychain Access")](images/createcertmanually02.png#lightbox)
6. После создания запроса согласно инструкции нажмите кнопку **Continue** (Продолжить) и отправьте его в центр участников:

    [![](images/createcertmanually03.png "Upload the CSR to the Member Center")](images/createcertmanually03.png#lightbox)

7. Нажмите **Generate** (Создать), чтобы создать сертификат.
8. Наконец, **скачайте** готовый сертификат и дважды щелкните файл, чтобы установить его.
9. На этом этапе сертификат должен быть установлен на компьютере, однако может потребоваться [обновить профили](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download), чтобы убедиться, что они видны в Xcode.

Можно также запросить сертификат в Xcode через диалоговое окно "Preferences" (Параметры). Выполните указанные ниже действия:

1. Выберите свою команду и щелкните **Manage Certificates…** (Управление сертификатами): [![](images/selectteam.png "Выбор команды и просмотр сведений")](images/selectteam.png#lightbox)

2. Затем нажмите кнопку **Create** (Создать) рядом с полем **iOS Distribution Certificate** (Сертификат распространения iOS): [![](images/selectcert.png "Создание сертификата распространения iOS")](images/selectcert.png#lightbox)

3. В зависимости от привилегий вашей команды будет создано удостоверение подписывания, как показано ниже, или вам придется ждать, пока администратор или агент команды утвердит его: [![](images/generated.png "Создание удостоверения подписывания и отображение диалогового окна")](images/generated.png#lightbox)

<a name="creatingprofile" />

## <a name="creating-a-distribution-profile"></a>Создание профиля распространения

<a name="creatingappid" />

### <a name="creating-an-app-id"></a>Создание идентификатора приложения

Как и в случае с любым другим создаваемым профилем подготовки, идентификатор приложения требуется для идентификации приложения, которое распространяется на устройство пользователя. Если вы еще не создали идентификатор, выполните следующие действия, чтобы создать его:

1. В [центре разработчиков Apple](https://developer.apple.com/account/overview.action) перейдите в раздел *Certificates, Identifiers and Profiles* (Сертификаты, идентификаторы и профили). В разделе **Identifiers** (Идентификаторы) выберите элемент **App IDs** (ИД приложений).
2. Нажмите кнопку **+** и укажите **Name** (Имя) для идентификации приложения на портале.
3. Префикс приложения уже должен быть задан в качестве идентификатора вашей команды. Изменить его невозможно. Выберите "Explicit" (Явный) или "Wildcard App ID" (Шаблон ИД приложения), а затем введите идентификатор пакета (имя DNS в обратном порядке):
    - **Explicit**: com.[доменное_имя].[имя_приложения]
    - **Wildcard**:com.[доменное_имя].*
4. Выберите любые [службы приложений](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#provisioning-for-application-services), которые требуются приложению.
5. Нажмите кнопку **Continue** (Продолжить) и следуйте инструкциям на экране, чтобы создать идентификатор приложения.

### <a name="creating-a-provisioning-profile"></a>Создание профиля подготовки

Получив компоненты, необходимые для создания профиля распределения, выполните следующие действия, чтобы создать его:

1. Вернитесь на портал подготовки Apple и выберите **Provisioning** > **Distribution** (Подготовка > Распространение):

    [![](images/distribute01.png "RSelect Provisioning > Distribution")](images/distribute01.png#lightbox)

2. Нажмите кнопку **+** и выберите тип профиля распространения, который нужно создать для **App Store**:

    [![](images/distribute02.png "Create an App Store distribution profile")](images/distribute02.png#lightbox)

3. Нажмите кнопку **Continue** (Продолжить) и выберите из раскрывающегося списка ИД приложения, для которого необходимо создать профиль распространения:

    [![](images/distribute03.png "Select App ID from the dropdown list")](images/distribute03.png#lightbox)

4. Нажмите кнопку **Continue** (Продолжить) и выберите сертификат, необходимый для подписи приложения:

    [![](images/distribute04.png "Select the certificate required to sign the application")](images/distribute04.png#lightbox)

5. Нажмите кнопку **Continue** (Продолжить) и выберите устройства iOS, на которых можно будет запускать приложение Xamarin.iOS:

    [![](images/distribute05.png "Select the iOS devices that app will be allowed to run on")](images/distribute05.png#lightbox)

6. Нажмите кнопку **Continue** (Продолжить) и введите **имя** нового профиля распространения:

    [![](images/distribute06.png "Enter a Name for the new Distribution Profile")](images/distribute06.png#lightbox)

7. Нажмите кнопку **Generate** (Создать), чтобы создать профиль и завершить процесс.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

 Может потребоваться выйти из программы Visual Studio для Mac, а также обновить список доступных удостоверений подписи и профилей подготовки в Xcode (в соответствии с инструкциями из раздела [Запрос удостоверений подписывания](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), прежде чем новый профиль распространения станет доступен в Visual Studio для Mac.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 Может потребоваться выйти из программы Visual Studio, а также обновить список доступных удостоверений подписи и профилей подготовки в Xcode на Mac узла сборки (в соответствии с инструкциями из раздела [Запрос удостоверений подписывания](~/ios/get-started/installation/device-provisioning/manual-provisioning.md#download)), прежде чем новый профиль распространения станет доступен в Visual Studio.

-----

<a name="selectprofile" />

## <a name="selecting-a-distribution-profile-in-a-xamarinios-project"></a>Выбор профиля распространения в проекте Xamarin.iOS

Когда все готово к финальной сборке приложения Xamarin.iOS для продажи в iTunes App Store, выберите профиль распространения, который был создан выше.

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

 В Visual Studio для Mac выполните следующие действия:

1. Дважды щелкните проект в **обозревателе решений**, чтобы открыть его для редактирования.
2. Выберите **Подписывание пакета iOS** и **Выпуск | iPhone** в раскрывающемся списке **Конфигурация**:

    ![](images/releasexs01.png "Select Release | iPhone from the Configuration dropdown")
3. В большинстве случаев в полях **Удостоверение подписывания** и **Профиль подготовки** можно оставить значения по умолчанию (**Автоматически**), чтобы программа Visual Studio для Mac выбрала подходящий профиль на основе идентификатора пакета в файле Info.plist:

    ![](images/releasexs02.png "The Signing Identity and the Provisioning Profile set to the default values of Automatic")
4. При необходимости выберите требуемое удостоверение подписывания и профиль распространения, созданный выше:

    ![](images/releasexs03.png "Select the Signing Identity and Distribution Profiles")
5. Нажмите кнопку **ОК**, чтобы сохранить изменения.

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

 В Visual Studio выполните следующие действия:

1. Щелкните проект правой кнопкой мыши в **обозревателе решений** и выберите пункт **Свойства**, чтобы открыть его для изменения.
2. Выберите **Подписывание пакета iOS** и **Выпуск | iPhone** в раскрывающемся списке **Конфигурация**:

    ![](images/releasevs01.png "Select Release | iPhone from the Configuration dropdown")
3. В большинстве случаев в полях **Удостоверение подписывания** и **Профиль подготовки** можно оставить значения по умолчанию (**Автоматически**), чтобы программа Visual Studio выбрала подходящий профиль на основе идентификатора пакета в файле Info.plist.

    ![](images/releasevs02.png "The Signing Identity and the Provisioning Profile set to the default values of Automatic")
4. При необходимости выберите требуемое удостоверение подписывания и профиль распространения, созданный выше:

    ![](images/releasevs03.png "Select the Signing Identity and Distribution Profile")
5. Сохраните изменения в свойствах проекта.

-----

<a name="itunesconnect" />

## <a name="configuring-your-application-in-itunes-connect"></a>Настройка приложения в iTunes Connect

Следующим шагом после успешной подготовки приложения является настройка приложений в [iTunes Connect](https://itunesconnect.apple.com/WebObjects/iTunesConnect.woa), который представляет собой набор веб-инструментов, обеспечивающий, среди прочего, управление приложениями iOS в App Store.

Приложение Xamarin.iOS должно быть правильно настроено в iTunes Connect, прежде чем вы отправите его на проверку в Apple или в конечном итоге начнете его коммерческое или бесплатное распространение через App Store.

Дополнительные сведения см. в нашей документации по [настройке приложений в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

<a name="submitting" />

## <a name="submitting-an-app-to-itunes-connect"></a>Отправка приложения в iTunes Connect

После подписания приложения с помощью профиля подготовки к распространению и создания приложения в iTunes Connect его двоичный файл отправляется на рассмотрение в Apple. После успешного рассмотрения компанией Apple оно становится доступно в App Store.

Дополнительные сведения о публикации приложений в App Store см. в разделе [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md).

<a name="windows" />

## <a name="automatically-copy-app-bundles-back-to-windows"></a>Автоматическое обратное копирование пакетов приложений в Windows

[!include[](~/ios/includes/copy-app-bundle-to-windows.md)]

## <a name="summary"></a>Сводка

Эта статья описывает основные этапы подготовки приложения Xamarin.iOS к распространению через App Store.

## <a name="related-links"></a>Связанные ссылки

- [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Внутреннее распространение](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Прямое распространение](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Файл iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Поддержка IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
