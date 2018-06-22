---
title: Создание пакетов APK для конкретного ABI
description: В этом документе объясняется, как с помощью Xamarin.Android создать пакет APK, предназначенный для одного конкретного интерфейса ABI.
ms.prod: xamarin
ms.assetid: D21B195B-4530-4EB2-8704-5C4349A2CDD8
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/15/2018
ms.openlocfilehash: 89a78c8dd1243dcfea9d14bd9758c5403d1d21ef
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
ms.locfileid: "30769082"
---
# <a name="building-abi-specific-apks"></a>Создание пакетов APK для конкретного ABI

_В этом документе объясняется, как с помощью Xamarin.Android создать пакет APK, предназначенный для одного конкретного интерфейса ABI._



## <a name="overview"></a>Обзор

В некоторых ситуациях для приложения удобно использовать несколько пакетов APK, которые имеют одинаковые имена и подписаны с применением одного хранилища ключей, но каждый из которых скомпилирован для конкретного устройства или конфигурации Android. Мы не рекомендуем применять такой подход, ведь намного проще использовать один APK, поддерживающий несколько устройств и конфигураций. Но иногда возникают ситуации, в которых полезно создать несколько APK, в частности:

-  **Уменьшение размера APK**. В Google Play для файлов APK действует ограничение в 100 МБ. Создавая отдельные APK для конкретных устройств, вы можете уменьшить размер APK, так как каждый из них будет содержать лишь ограниченный набор файлов и ресурсов.

-  **Поддержка разных архитектур ЦП**. Если ваше приложение использует общие библиотеки для конкретных процессоров, их можно включать только в поставку для этих ЦП.


Несколько пакетов APK усложняют процессы распространения, но эта проблема решена в Google Play. Google Play обеспечит доставку на устройство правильной версии APK, определяя ее по коду версии приложения и другим метаданным из файла **AndroidManifest.XML**. Подробный алгоритм и ограничения, применяемые Google Play для поддержки нескольких пакетов APK в приложении, можно узнать из [документации Google о поддержке нескольких APK](http://developer.android.com/google/play/publishing/multiple-apks.html).

Это руководство описывает, как организовать для приложения Xamarin.Android сборку нескольких пакетов APK, каждый из которых предназначен для конкретной версии ABI. Здесь рассматриваются следующие темы.

1.  Создание уникального *кода версии* для APK.
1.  Создание временной версии **AndroidManifest.XML**, которая будет использоваться для этого APK.
1.  Сборка приложения с помощью **AndroidManifest.XML**, созданного на предыдущем шаге.
1.  Подготовка APK к выпуску, включая подписывание и оптимизацию для архива.


В конце этой статьи представлено пошаговое руководство по выполнению всех этих шагов в сценарии [Rake](http://martinfowler.com/articles/rake.html).



### <a name="creating-the-version-code-for-the-apk"></a>Создание кода версии для APK

Google рекомендует соблюдать определенные правила для кодов версий, составляя их из семи цифр (этот алгоритм описан в разделе, посвященном *использованию схемы кода версии*, [документа о поддержке нескольких APK](http://developer.android.com/google/play/publishing/multiple-apks.html)).
Расширив эту схему до восьми цифр, вы сможете добавить в код версии сведения об ABI, что позволит Google Play распределять на устройства правильные пакеты APK. Следующий список содержит описание восьми цифр кода версии для этого формата (индексы в коде нумеруются слева направо).

-   **Индекс 0** (выделен красным на схеме ниже): &ndash; целое число, обозначающее ABI:
    -   1 &ndash; `armeabi`
    -   2 &ndash; `armeabi-v7a`
    -   6 &ndash; `x86`

-   **Индексы 1–2** (выделены оранжевым на схеме ниже): &ndash; минимальный уровень API, поддерживаемый приложением.

-   **Индексы 3–4** (выделены синим на схеме ниже): &ndash; поддерживаемые размеры экрана.
    -   1 &ndash; маленький;
    -   2 &ndash; обычный;
    -   3 &ndash; большой;
    -   4 &ndash; очень большой.

-   **Индексы 5–7** (выделены зеленым на схеме ниже): &ndash; уникальный номер кода версии. 
    Это значение определяется разработчиком. Оно должно увеличиваться для каждого следующего общедоступного выпуска приложения.

На следующей схеме показано положение каждого элемента кода из списка выше.

[![Схема формата для кода версии из восьми цифр с цветовым кодированием](abi-specific-apks-images/image00.png)](abi-specific-apks-images/image00.png#lightbox)


Google Play будет доставлять на устройства правильные пакеты APK, используя конфигурацию `versionCode` и APK. На устройство всегда доставляется APK с самым большим кодом версии. Для примера предположим, что у приложения есть три пакета APK с указанными здесь кодами версий.

-  11413456: номер ABI — `armeabi`; уровень API — 14; экраны от малого до большого размеров; номер версии — 456.
-  21423456 : номер ABI — `armeabi-v7a`; уровень API — 14; экраны обычного или большого размеров; номер версии — 456.
-  61423456 : номер ABI — `x86`; уровень API — 14; экраны обычного или большого размеров; номер версии — 456.

Разработчик этого приложения обнаружил и исправил ошибку, которая проявлялась только в `armeabi-v7a`. Он увеличивает версию приложения до 457 и создает новый пакет APK, у которого `android:versionCode` имеет значение 21423457. Коды версий для версий `armeabi` и `x86` остаются неизменными.

Теперь разработчик создает новые функции и (или) исправляет ошибки в версии для x86, переходя при этом на более новый API (API уровня 19), и назначает этой версии номер 500. Теперь `versionCode` будет иметь новое значение 61923500, а версии для armeabi и armeabi-v7a остаются без изменений. К этому моменту сформировались следующие коды версий.

-  11413456: номер ABI — `armeabi`; уровень API — 14; экраны от малого до большого размеров; имя версии — 456.
-  21423457 : номер ABI — `armeabi-v7a`; уровень API — 14; экраны нормального или большого размеров; имя версии — 457.
-  61923500 : номер ABI — `x86`; уровень API — 19; экраны обычного или большого размеров; имя версии — 500.


Разработчику может потребоваться немало усилий, чтобы вручную отслеживать все эти версии. Процесс вычисления правильных значений `android:versionCode` и сборки APK следует автоматизировать.
Пример такой автоматизации будет рассмотрен в пошаговом руководстве в конце этой статьи.


### <a name="create-a-temporary-androidmanifestxml"></a>Создание временного файла AndroidManifest.XML

Этот шаг не является обязательным, но создание временных файлов **AndroidManifest.XML** для каждого интерфейса ABI позволит предотвратить ряд проблем, возникающих при утечке данных из одного пакета APK в другой. Например, очень важно сохранять уникальный атрибут `android:versionCode` для каждого APK.

Механизм контроля будет разным для разных систем выполнения скриптов, но обычно он включает получение копии манифеста Android, который использовался во время разработки, внесение необходимых изменений и применение манифеста во время сборки.



### <a name="compiling-the-apk"></a>Компиляция пакета APK

Сборку APK для каждого ABI лучше всего выполнять с помощью `xbuild` или `msbuild`, как показано в следующем примере командной строки:

```bash
/Library/Frameworks/Mono.framework/Commands/xbuild /t:Package /p:AndroidSupportedAbis=<TARGET_ABI> /p:IntermediateOutputPath=obj.<TARGET_ABI>/ /p:AndroidManifest=<PATH_TO_ANDROIDMANIFEST.XML> /p:OutputPath=bin.<TARGET_ABI> /p:Configuration=Release <CSPROJ FILE>
```

В следующем списке описывается каждый параметр командной строки:

-   `/t:Package` &ndash; создает Android APK, подписанный с помощью хранилища ключей отладки.

-   `/p:AndroidSupportedAbis=<TARGET_ABI>` &ndash; целевой интерфейс ABI. Допускаются значения `armeabi`, `armeabi-v7a` и `x86`.

-   `/p:IntermediateOutputPath=obj.<TARGET_ABI>/` &ndash; каталог, который будет содержать промежуточные файлы, созданные в процессе сборки. При необходимости Xamarin.Android создаст этот каталог с именем, совпадающим с именем интерфейса ABI, например `obj.armeabi-v7a`. Мы рекомендуем использовать отдельную папку для каждого интерфейса ABI, чтобы предотвратить проблемы, возникающие при "утечке" файлов из одной сборки в другую. Обратите внимание, что это значение должно завершаться разделителем каталогов (например, `/` для OS X).

-   `/p:AndroidManifest` &ndash; указывает путь к файлу **AndroidManifest.XML**, который следует использовать в процессе сборки.

-   `/p:OutputPath=bin.<TARGET_ABI>` &ndash; каталог, где будет размещена готовая версия APK. Xamarin.Android создаст этот каталог с именем, совпадающим с именем интерфейса ABI, например `bin.armeabi-v7a`.

-   `/p:Configuration=Release` &ndash; обозначает выполнение сборки выпуска для APK. Отладочные сборки не всегда загружаются в Google Play.

-   `<CS_PROJ FILE>` &ndash; — обозначает путь к файлу `.csproj` для проекта Xamarin.Android.



### <a name="sign-and-zipalign-the-apk"></a>Подписывание пакета APK и оптимизация для архива

Каждый пакет APK необходимо подписать, чтобы распространять его через Google Play. Для этого можно применить приложение `jarsigner`, которое входит в комплект средств для разработчиков Java. Пример запуска `jarsigner` из командной строки приведен ниже:

```shell
jarsigner -verbose -sigalg SHA1withRSA -digestalg SHA1 -keystore <PATH/TO/KEYSTORE> -storepass <PASSWORD> -signedjar <PATH/FOR/SIGNED_JAR> <PATH/FOR/JAR/TO/SIGN> <NAME_OF_KEY_IN_KEYSTORE>
```

Все приложения Xamarin.Android должны быть оптимизированы для архива, чтобы их можно было запустить на устройстве. Ниже представлен формат командной строки, которая позволяет это сделать:

```shell
zipalign -f -v 4 <SIGNED_APK_TO_ZIPALIGN> <PATH/TO/ZIP_ALIGNED.APK>
```


## <a name="automating-apk-creation-with-rake"></a>Автоматизация создания APK с помощью Rake

Пример [OneABIPerAPK](https://github.com/xamarin/monodroid-samples/tree/master/OneABIPerAPK) содержит простой проект Android, в котором демонстрируется вычисление номера версии ABI и сборка трех отдельных APK для каждого из следующих ABI:

-  armeabi;
-  armeabi-v7a;
-  x86


Файл [rakefile](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb) в этом примере выполняет все шаги, описанные в предыдущих разделах:

1. [Создание android:versionCode](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L30) для APK.

1. [Сохранение android:versionCode](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L55) в пользовательском файле **AndroidManifest.XML** для этого APK.

1. [Компиляция сборки выпуска](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L63) для проекта Xamarin.Android, предназначенной для конкретного ABI и использующей **AndroidManifest.XML**, созданный на предыдущем шаге.

1. [Подписывание APK ](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L66) с помощью рабочего хранилища ключей.

1. [Оптимизация](https://github.com/xamarin/monodroid-samples/blob/master/OneABIPerAPK/Rakefile.rb#L67) APK для архива.


Чтобы скомпилировать сразу все APK, входящие в приложения, запустите задачу Rake `build` из командной строки:

```shell
$ rake build
==> Building an APK for ABI armeabi with ./Properties/AndroidManifest.xml.armeabi, android:versionCode = 10814120.
==> Building an APK for ABI x86 with ./Properties/AndroidManifest.xml.x86, android:versionCode = 60814120.
==> Building an APK for ABI armeabi-v7a with ./Properties/AndroidManifest.xml.armeabi-v7a, android:versionCode = 20814120.
```

Когда выполнение задачи Rake завершится, у вас будет три папки `bin` с файлом `xamarin.helloworld.apk`. На следующем снимке экрана показаны все эти папки и их содержимое:

[![Расположение папок для каждой платформы с файлами xamarin.helloworld.apk](abi-specific-apks-images/image01.png)](abi-specific-apks-images/image01.png#lightbox)


> [!NOTE]
> Процесс сборки, описанный в этом руководстве, можно реализовать во многих системах сборки. Например, такой режим точно поддерживают [PowerShell](http://technet.microsoft.com/en-ca/scriptcenter/powershell.aspx) / [psake](https://github.com/psake/psake) и [Fake](http://fsharp.github.io/FAKE/), но мы пока не можем предложить для них готовых примеров.


## <a name="summary"></a>Сводка

В этом руководстве представлены некоторые рекомендации о том, как создать Android APK для определенных интерфейсов ABI. Подробно обсуждается один из вариантов создания `android:versionCodes` для отслеживания архитектуры ЦП, для которой предназначен пакет APK. Также здесь предоставлено пошаговое руководство по сборке тестового проекта с помощью сценариев Rake.



## <a name="related-links"></a>Связанные ссылки

- [OneABIPerAPK (пример)](https://developer.xamarin.com/samples/OneABIPerAPK/)
- [Публикация приложения](~/android/deploy-test/publishing/index.md)
- [Поддержка нескольких APK для Google Play](http://developer.android.com/google/play/publishing/multiple-apks.html)
