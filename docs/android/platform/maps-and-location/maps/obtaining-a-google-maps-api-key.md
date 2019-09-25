---
title: Получение ключа API Google Maps
description: Как получить ключ API Google Maps для добавления функций карт в приложение.
ms.prod: xamarin
ms.assetid: D5969C57-3444-465E-D6FF-249AEE62E127
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 06/25/2018
ms.openlocfilehash: 3868b2a35894cdcd7a11c626268307338744ecb4
ms.sourcegitcommit: 699de58432b7da300ddc2c85842e5d9e129b0dc5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250054"
---
# <a name="obtaining-a-google-maps-api-key"></a>Получение ключа API Google Maps

Чтобы использовать функции Google Maps в Android, необходимо зарегистрироваться для получения ключа API Maps в Google. Пока вы не сделаете это, в приложениях будет просто отображаться пустая сетка, а не схема. Вы должны получить ключи ключей API Android версии 2 для Google Maps из старого интерфейса Google Maps Android API V1 не будет работать.

Получение ключа API карт версии 2 включает следующие шаги.

1. Получите отпечаток SHA-1 хранилища ключей, который используется для подписания приложения.
2. Создайте проект в консоли Google API.
3. Получение ключа API.

## <a name="obtaining-your-signing-key-fingerprint"></a>Получение отпечатка ключа подписывания

Чтобы запросить ключ API карт из Google, необходимо знать отпечаток SHA-1 хранилища ключей, который используется для подписания приложения.
Как правило, это означает, что необходимо определить отпечаток SHA-1 для хранилища ключей отладки, а затем отпечаток SHA-1 для хранилища ключей, используемого для подписи приложения для выпуска.

<!-- markdownlint-disable MD001 -->

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

По умолчанию хранилище ключей, используемое для подписания отладочных версий приложения Xamarin. Android, можно найти по следующему адресу:

**В.\\пользователи\\[имя_пользователя]\\AppData\\Local\\XamarinMono\\для Android\\Debug. хранилище**

Информация о хранилище ключей отображается при вызове команды `keytool` из JDK. Это средство обычно находится в каталоге bin Java:

**C:\\Program Files (x86\\)\\Java JDK [версия\\]\\bin keytool. exe**

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

По умолчанию хранилище ключей, используемое для подписания отладочных версий приложения Xamarin. Android, можно найти по следующему адресу:

**/Users/[USERNAME]/.локал/шаре/ксамарин/моно для Android/Debug. хранилище**

Информация о хранилище ключей отображается при вызове команды `keytool` из JDK. Это средство обычно находится в каталоге bin Java:

**/System/Library/Java/JavaVirtualMachines/[VERSION].jdk/Contents/Home/bin/keytool**

-----

Запустите keytool с помощью следующей команды (используя приведенные выше пути к файлам):

```shell
keytool -list -v -keystore [STORE FILENAME] -alias [KEY NAME] -storepass [STORE PASSWORD] -keypass [KEY PASSWORD]
```

### <a name="debugkeystore-example"></a>Пример отладки хранилища ключей

Для ключа отладки по умолчанию (который автоматически создается для отладки) используйте следующую команду:

# <a name="visual-studiotabwindows"></a>[Visual Studio](#tab/windows)

```cmd
keytool.exe -list -v -keystore "C:\Users\[USERNAME]\AppData\Local\Xamarin\Mono for Android\debug.keystore" -alias androiddebugkey -storepass android -keypass android
```

# <a name="visual-studio-for-mactabmacos"></a>[Visual Studio для Mac](#tab/macos)

```bash
keytool -list -v -keystore /Users/[USERNAME]/.local/share/Xamarin/Mono\ for\ Android/debug.keystore -alias androiddebugkey -storepass android -keypass android
```

-----

### <a name="production-keys"></a>Производственные ключи

При развертывании приложения на Google Play оно должно быть [подписано с помощью закрытого ключа](~/android/deploy-test/signing/index.md).
`keytool` Необходимо будет выполнить с данными закрытого ключа и полученный отпечаток SHA-1, используемый для создания рабочего ключа API Google Maps. Не забудьте обновить файл **AndroidManifest. XML** с правильным ключом API-карты Google Maps перед развертыванием.

### <a name="keytool-output"></a>Выходные данные keytool

В окне консоли вы должны увидеть нечто вроде следующего:

```shell
Alias name: androiddebugkey
Creation date: Jan 01, 2016
Entry type: PrivateKeyEntry
Certificate chain length: 1
Certificate[1]:
Owner: CN=Android Debug, O=Android, C=US
Issuer: CN=Android Debug, O=Android, C=US
Serial number: 4aa9b300
Valid from: Mon Jan 01 08:04:04 UTC 2013 until: Mon Jan 01 18:04:04 PST 2033
Certificate fingerprints:
    MD5:  AE:9F:95:D0:A6:86:89:BC:A8:70:BA:34:FF:6A:AC:F9
    SHA1: BB:0D:AC:74:D3:21:E1:43:07:71:9B:62:90:AF:A1:66:6E:44:5D:75
    Signature algorithm name: SHA1withRSA
    Version: 3
```

В этом разделе будет использоваться отпечаток SHA-1 (указывается после **SHA1**).

## <a name="creating-an-api-project"></a>Создание проекта API

После получения отпечатка SHA-1 для подписанного хранилища ключей необходимо создать новый проект в консоли Google API (или добавить службу Google Maps Android API v2 в существующий проект).

1. В браузере перейдите на [панель мониторинга & служб API консоли Google Developers](https://console.developers.google.com/apis/dashboard/) и щелкните **выбрать проект**. Щелкните имя проекта или создайте новое, щелкнув **создать проект**:

   [![Кнопка создания проекта в консоли разработчика Google](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs-sml.png)](obtaining-a-google-maps-api-key-images/01-google-developer-console-vs.png#lightbox)

2. Если вы создали новый проект, введите имя проекта в появившемся диалоговом окне **Новый проект** . В этом диалоговом окне будет изготовлен уникальный идентификатор проекта, основанный на имени проекта. Затем нажмите кнопку **создать** , как показано в следующем примере:

   [![Новый проект называется Ксамаринмапсдемо](obtaining-a-google-maps-api-key-images/02-new-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/02-new-project-vs.png#lightbox)

3. Через минуту вы создадите проект и перейдете на страницу **панели мониторинга** проекта. В нем щелкните **включить интерфейсы API и службы**:

   [![Щелчок Google Maps Android API в разделе Library](obtaining-a-google-maps-api-key-images/03-api-selection-vs-sml.png)](obtaining-a-google-maps-api-key-images/03-api-selection-vs.png#lightbox)

4. На странице **Библиотека API** щелкните **Map SDK for Android**. На следующей странице щелкните **включить** , чтобы включить службу для этого проекта:

   [![Нажатие кнопки "включить" в разделе "панель мониторинга"](obtaining-a-google-maps-api-key-images/04-enable-api-vs-sml.png)](obtaining-a-google-maps-api-key-images/04-enable-api-vs.png#lightbox)

На этом этапе проект API создан и в него добавлен интерфейс Google Maps Android API версии 2. Однако этот API нельзя использовать в проекте, пока вы не создадите для него учетные данные. В следующем разделе объясняется, как создать ключ API и белый список приложения Xamarin. Android, чтобы ему было разрешено использовать этот ключ.

## <a name="obtaining-the-api-key"></a>Получение ключа API

После создания проекта API **консоли разработчика Google** необходимо создать ключ API Android. Приложения Xamarin. Android должны иметь ключ API, прежде чем им будет предоставлен доступ к API-интерфейсу карт Android версии 2.

1. На отображаемой странице **SDK Maps для Android** (после нажатия кнопки **включить** на предыдущем шаге) перейдите на вкладку **учетные данные** и нажмите кнопку **CREATE CREDENTIAL (создать учетные данные** ).

   [![Сообщение SDK Maps для учетных данных Android](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs-sml.png)](obtaining-a-google-maps-api-key-images/05-api-is-enabled-vs.png#lightbox)

2. Щелкните **ключ API**:

   [![Добавление учетных данных в диалоговое окно проекта](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs-sml.png)](obtaining-a-google-maps-api-key-images/06-add-credentials-to-your-project-vs.png#lightbox)

3. После нажатия этой кнопки создается ключ API. Далее необходимо ограничить этот ключ, чтобы только приложение вызывало API с помощью этого ключа. Нажмите кнопку **ограничить ключ**:

   [![Нажатие кнопки ограничить ключ на странице учетные данные](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs-sml.png)](obtaining-a-google-maps-api-key-images/07-generate-api-key-vs.png#lightbox)

4. Измените поле **имени** из **ключа API с 1** на имя, которое поможет вспомнить, какой ключ используется для (в этом примере используется**ксамаринмапсдемокэй** ). Затем выберите переключатель **приложения Android** :

   [![Выбор приложений Android на странице учетных данных](obtaining-a-google-maps-api-key-images/08-key-restriction-vs-sml.png)](obtaining-a-google-maps-api-key-images/08-key-restriction-vs.png#lightbox)

5. Чтобы добавить отпечаток SHA-1, щелкните **+ Добавить имя пакета и отпечаток**:

   [![Щелкните Добавить имя пакета и отпечаток](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/09-add-package-fingerprint-vs.png#lightbox)

6. Введите имя пакета приложения и введите отпечаток сертификата SHA-1 (полученный с `keytool` помощью, как описано выше в этом разделе). В следующем примере указывается имя пакета для `XamarinMapsDemo` , за которым следует отпечаток сертификата SHA-1, полученный из **Debug. хранилище**.

   [![Указано имя пакета — com. Xamarin. документацию. Android. Map](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs-sml.png)](obtaining-a-google-maps-api-key-images/10-enter-package-and-sha1-vs.png#lightbox)

7. Обратите внимание, что, чтобы пакет APK для доступа к Google карты, необходимо включить отпечатки пальцев SHA-1 и упаковать имена для каждого хранилища ключей (отладочную и окончательную), которые используются для входа пакет APK. Например если вы используете один компьютер для отладки и другой компьютер для создания выпуска APK, должно содержать отпечаток SHA-1 сертификата из хранилища ключей отладки первого компьютера и отпечаток SHA-1 сертификата из хранилища ключей выпуска из второй компьютер. Щелкните **+ Добавить имя пакета и отпечаток** , чтобы добавить еще один отпечаток и имя пакета, как показано в следующем примере:

   [![При добавлении другого отпечатка создается еще один сертификат SHA-1](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs-sml.png)](obtaining-a-google-maps-api-key-images/11-second-fingerprint-vs.png#lightbox)

8. Чтобы сохранить внесенные изменения, нажмите кнопку **Save** (Сохранить). Далее вы вернетесь к списку ключей API. Если у вас есть другие ключи API, созданные ранее, они также будут перечислены здесь. В этом примере отображается только один ключ API (созданный в предыдущих шагах):

   [![Ксамаринмапсдемокэй отображается в списке ключей API](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs-sml.png)](obtaining-a-google-maps-api-key-images/12-list-of-apis-vs.png#lightbox)

## <a name="connect-the-project-to-a-billable-account"></a>Подключение проекта к оплачиваемому счету

Начиная с июня 11 2018, ключ API не будет работать, если проект не подключен к оплачиваемому счету (даже если служба по-прежнему доступна для мобильных приложений).

1. Нажмите кнопку меню-гамбургер и выберите страницу **выставления счетов** :

   [![Выбор пункта меню «гамбургер» для выставления счетов](obtaining-a-google-maps-api-key-images/13-goto-billing-vs-sml.png)](obtaining-a-google-maps-api-key-images/13-goto-billing-vs.png#lightbox)

2. Свяжите проект с учетной записью выставления счетов, щелкнув **связать учетную запись выставления** счетов, а затем **создать УЧЕТную запись выставления счетов** в отображаемом всплывающем окне (если у вас нет учетной записи, вам будет предложено создать новую):

   [![Связать проект с учетной записью выставления счетов](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs-sml.png)](obtaining-a-google-maps-api-key-images/14-link-billing-account-vs.png#lightbox)

## <a name="adding-the-key-to-your-project"></a>Добавление ключа в проект

Наконец, добавьте этот ключ API в файл **AndroidManifest. XML** приложения Xamarin. Android. В следующем примере `YOUR_API_KEY` необходимо заменить ключом API, созданным на предыдущих шагах:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    android:versionName="4.10" package="com.xamarin.docs.android.mapsandlocationdemo"
    android:versionCode="10">
...
  <application android:label="@string/app_name">
    <!-- Put your Google Maps V2 API Key here. -->
    <meta-data android:name="com.google.android.maps.v2.API_KEY" android:value="YOUR_API_KEY" />
    <meta-data android:name="com.google.android.gms.version" android:value="@integer/google_play_services_version" />
  </application>
</manifest>
```

## <a name="related-links"></a>Связанные ссылки

- [Консоль API Google](https://code.google.com/apis/console/)
- [Ключ API Google Maps](https://developers.google.com/maps/documentation/android/start#the_google_maps_api_key)
- [keytool](http://docs.oracle.com/javase/6/docs/technotes/tools/windows/keytool.html.)
