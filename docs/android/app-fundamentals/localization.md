---
title: Локализация Android
description: В этом документе представлены функции локализации пакет SDK для Android и способы доступа к ним с помощью Xamarin.
ms.prod: xamarin
ms.assetid: D1277939-A1E8-468E-B136-820D816AF853
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 03/01/2018
ms.openlocfilehash: ae97297b81d33c4b9f814d4b3639984b05ce3d72
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73021655"
---
# <a name="android-localization"></a>Локализация Android

_В этом документе представлены функции локализации пакет SDK для Android и способы доступа к ним с помощью Xamarin._

## <a name="android-platform-features"></a>Возможности на платформе Android

В этом разделе описаны основные возможности локализации Android. Перейдите к [следующему разделу](#basics) , чтобы просмотреть конкретный код и примеры.

### <a name="locale"></a>Языковой стандарт

Пользователи выбирают язык в **параметрах > язык & входные данные**. Этот выбор контролирует как отображаемый язык, так и региональные параметры (например, для форматирования даты и чисел).

Текущий языковой стандарт можно запрашивать с помощью `Resources`текущего контекста:

```csharp
var lang = Resources.Configuration.Locale; // eg. "es_ES"
```

Это значение будет идентификатором локали, который содержит код языка и код локали, разделенные символом подчеркивания. Для справки ниже приведен список языковых [стандартов Java](https://www.oracle.com/technetwork/java/javase/locales-137662.html) и [языков с поддержкой Android через StackOverflow](https://stackoverflow.com/questions/7973023/what-is-the-list-of-supported-languages-locales-on-android).

Вот наиболее распространенные примеры.

- `en_US` для английского языка (США)
- `es_ES` для испанского языка (Испания)
- `ja_JP` для японского языка (Япония)
- `zh_CN` для китайского языка (Китай)
- `zh_TW` для китайского языка (Тайвань)
- `pt_PT` для португальского языка (Португалия)
- `pt_BR` для португальского языка (Бразилия)

### <a name="locale_changed"></a>LOCALE_CHANGED

Android создает `android.intent.action.LOCALE_CHANGED`, когда пользователь изменяет свой выбор языка.

Действия могут решить эту операцию, задав атрибут `android:configChanges` для действия следующим образом:

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges = ConfigChanges.Locale | ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
```

<a name="basics" />

## <a name="internationalization-basics-in-android"></a>Основы интернационализации в Android

Стратегия локализации Android состоит из следующих основных частей.

- Папки ресурсов для хранения локализованных строк, изображений и других ресурсов.

- метод `GetText`, используемый для извлечения локализованных строк в коде

- `@string/id` в файлах AXML для автоматического размещения локализованных строк в макетах.

### <a name="resource-folders"></a>Папки ресурсов

Приложения Android управляют большинством содержимого в папках ресурсов, например:

- **Макет** — содержит файлы макета AXML.
- **нарисованный** — содержит изображения и другие рисуемые ресурсы.
- **значения** — содержит строки.
- **RAW** — содержит файлы данных.

Большинство разработчиков уже знакомы с использованием суффиксов **dpi** в **порисованном** каталоге для предоставления нескольких версий образа, позволяя Android выбрать правильную версию для каждого устройства. Этот же механизм используется для предоставления нескольких языковых переводов путем суффикса каталогов ресурсов с идентификаторами языка и региональных параметров.

![Снимок экрана: ресурсы, рисуемые папки и ресурсы/значения для нескольких культурных идентификаторов](localization-images/resources.png)

> [!NOTE]
> При указании языка верхнего уровня, например `es`, требуются только два символа. Однако при указании полного языкового стандарта формат имени каталога требует тире и строчной буквы **r** для разделения двух частей, например **PT-РБР** или **zh-РКН**. Сравните это со значением, возвращаемым в коде, которое имеет символ подчеркивания (например, `pt_BR`). Оба они отличаются от значения, используемого классом .NET `CultureInfo`, который имеет только тире (например, `pt-BR`). Учитывайте эти различия при работе на платформах Xamarin.

#### <a name="stringsxml-file-format"></a>Формат файла strings. XML

Каталог локализованных **значений** (например, **значения-ES** или **Values-PT-РБР**) должны содержать файл с именем **Strings. XML** , который будет содержать переведенный текст для этого языкового стандарта.

Каждая Преобразуемая строка является XML-элементом с ИДЕНТИФИКАТОРом ресурса, указанным в качестве атрибута `name` и переведенной строкой в качестве значения:

```xml
<string name="app_name">TaskyL10n</string>
```

Необходимо экранировать в соответствии с обычными XML-правилами, а `name` должен быть допустимым ИДЕНТИФИКАТОРом ресурса Android (без пробелов или тире). Ниже приведен пример файла строк по умолчанию (Английская версия) для примера.

**значения/strings. XML**

```xml
<resources>
    <string name="app_name">TaskyL10n</string>
    <string name="taskadd">Add Task</string>
    <string name="taskname">Name</string>
    <string name="tasknotes">Notes</string>
    <string name="taskdone">Done</string>
    <string name="taskcancel">Cancel</string>
</resources>
```

Значения каталога на испанском языке **-ES** содержат файл с тем же именем (**Strings. XML**), который содержит переводы:

**валуес-ЕС/strings. XML**

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <string name="app_name">TaskyLeon</string>
    <string name="taskadd">agregar tarea</string>
    <string name="taskname">Nombre</string>
    <string name="tasknotes">Notas</string>
    <string name="taskdone">Completo</string>
    <string name="taskcancel">Cancelar</string>
</resources>
```

![Снимок экрана с несколькими значениями папки, каждая из которых содержит файл strings. XML](localization-images/values.png)

При настройке строковых файлов переведенные значения можно ссылаться как в макетах, так и в коде.

### <a name="axml-layout-files"></a>Файлы макета AXML

Для ссылки на локализованные строки в файлах макета используйте синтаксис `@string/id`. Этот фрагмент кода XML из примера показывает `text` свойства, заданные с использованием локализованных идентификаторов ресурсов (некоторые другие атрибуты опущены):

```xml
<TextView
    android:id="@+id/NameLabel"
    android:text="@string/taskname"
    ... />
<CheckBox
    android:id="@+id/chkDone"
    android:text="@string/taskdone"
    ... />
```

### <a name="gettext-method"></a>Метод GetText

Чтобы получить переведенные строки в коде, используйте метод `GetText` и передайте идентификатор ресурса:

```csharp
var cancelText = Resources.GetText (Resource.String.taskcancel);
```

#### <a name="quantity-strings"></a>Строки количества

Строковые ресурсы Android также позволяют создавать *строки количества* , позволяющие переводчикам предоставлять различные переводы для различных количеств, например:

- "Осталось 1 задача".
- «По-прежнему есть две задачи».

(вместо универсального "есть n задач, слева").

В **файле strings. XML**

```xml
<plurals name="numberOfTasks">
   <!--
      As a developer, you should always supply "one" and "other"
      strings. Your translators will know which strings are actually
      needed for their language.
    -->
   <item quantity="one">There is %d task left.</item>
   <item quantity="other">There are %d tasks still to do.</item>
 </plurals>
```

Чтобы отобразить полную строку, используйте метод `GetQuantityString`, передав идентификатор ресурса и отображаемое значение (которое передается дважды). Второй параметр используется Android для определения используемой строки `quantity`, третий параметр — это значение *, которое фактически* подставляется в строку (оба являются обязательными).

```csharp
var translated = Resources.GetQuantityString (
                    Resource.Plurals.numberOfTasks, taskcount, taskcount);`
```

Допустимые параметры `quantity`:

- нуль
- one
- Эти
- определенные
- many
- другие

Они более подробно описаны в [документах Android](https://developer.android.com/guide/topics/resources/string-resource.html#Plurals). Если на данном языке не требуется обработка "Special", то эти `quantity` строки будут игнорироваться (например, на английском языке используются только `one` и `other`; указание строки `zero` не будет действовать, так как не будет использоваться).

### <a name="images"></a>образы,

Локализованные образы следуют тем же правилам, что и файлы со строками. все образы, упоминаемые в приложении, должны быть помещены в **нарисованные** каталоги, поэтому существует резервная версия.

Образы, зависящие от языкового стандарта, затем должны размещаться в готовых для рисования папках, таких как **Draw-ES** или **Draw-JA** (также можно добавить описатели DPI).

На этом снимке экрана четыре изображения сохраняются в **порисованном** каталоге, но только в одном файле **Flag. png**имеют локализованные копии в других каталогах.

![Снимок экрана нескольких нарисованных папок, каждый из которых содержит один или несколько локализованных PNG-файлов](localization-images/drawable.png)

#### <a name="other-resource-types"></a>Другие типы ресурсов

Вы также можете предоставить другие типы альтернативных ресурсов для конкретного языка, включая макеты, анимации и необработанные файлы. Это означает, что можно предоставить конкретный макет экрана для одного или нескольких целевых языков, например, можно создать макет специально для немецкого языка, который позволяет использовать очень длинные текстовые метки.

В Android 4,2 появилась поддержка [языков с письмом справа налево (RTL)](https://android-developers.blogspot.fr/2013/03/native-rtl-support-in-android-42.html) , если задан параметр приложения `android:supportsRtl="true"`. Квалификатор ресурса `"ldrtl"` можно включить в имя каталога, чтобы содержать пользовательские макеты, предназначенные для вывода справа налево.

Дополнительные сведения об именовании и откате каталога ресурсов см. в документации по Android для [предоставления альтернативных ресурсов](https://developer.android.com/guide/topics/resources/providing-resources.html#AlternativeResources).

### <a name="app-name"></a>Имя приложения

Имя приложения легко локализовать с помощью `@string/id` в для действия `MainLauncher`.

```csharp
[Activity (Label = "@string/app_name", MainLauncher = true, Icon="@drawable/launcher",
    ConfigurationChanges =  ConfigChanges.Orientation | ConfigChanges.Locale)]
```

### <a name="right-to-left-rtl-languages"></a>Языки с направлением письма справа налево

Android 4,2 и более поздние версии обеспечивают полную поддержку макетов RTL, подробно описанных в [блоге о поддержке RTL](https://android-developers.blogspot.dk/2013/03/native-rtl-support-in-android-42.html).

При использовании Android 4,2 (API уровня 17) и более поздних версий значения выравнивания можно указать с помощью `start` и `end` вместо `left` и `right` (например, `android:paddingStart`). Существуют также новые API, такие как `LayoutDirection`, `TextDirection`и `TextAlignment`, помогающие создавать экраны для читателей с письмом справа налево.

На следующем снимке экрана показан [Пример локализованной **задачи** ](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) на арабском языке.

[![снимок экрана приложения для задач на арабском языке](localization-images/rtl-ar-sml.png)](localization-images/rtl-ar.png#lightbox) 

На следующем снимке экрана показан [локализованный пример **задачи** ](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n) на иврите:

[![снимок экрана приложения задач на иврите](localization-images/rtl-he-sml.png)](localization-images/rtl-he.png#lightbox)

Текст справа налево локализуется с помощью файлов **Strings. XML** так же, как текст на LTR.

## <a name="testing"></a>Тестирование

Обязательно выполните тщательное тестирование языкового стандарта по умолчанию. Приложение аварийно завершит работу, если ресурсы по умолчанию не могут быть загружены по какой-либо причине (т. е. отсутствуют).

### <a name="emulator-testing"></a>Тестирование эмулятора

Инструкции по настройке эмулятора для определенного языкового стандарта с помощью оболочки ADB см. в разделе о [тестировании Google в Android Emulator](https://developer.android.com/guide/topics/resources/localization.html#testing) .

```shell
adb shell setprop persist.sys.locale fr-CA;stop;sleep 5;start
```

### <a name="device-testing"></a>Тестирование устройства

Чтобы выполнить тестирование на устройстве, измените язык в приложении " **Параметры** ".

> [!TIP]
> Запишите значки и расположение пунктов меню, чтобы можно было вернуть язык к исходному значению.

## <a name="summary"></a>Сводка

В этой статье рассматриваются основы локализации приложений Android с помощью встроенной обработки ресурсов. Дополнительные сведения о i18n и l10n для приложений iOS, Android и кросс-платформенных (включая Xamarin. Forms) см. в [этом межплатформенном руководством](~/cross-platform/app-fundamentals/localization.md).

## <a name="related-links"></a>Связанные ссылки

- [Задача (локализованная в коде) (пример)](https://github.com/conceptdev/xamarin-samples/tree/master/TaskyL10n)
- [Локализация Android с ресурсами](https://developer.android.com/guide/topics/resources/localization.html)
- [Общие сведения о локализации на разных платформах](~/cross-platform/app-fundamentals/localization.md)
- [Локализация Xamarin. Forms](~/xamarin-forms/app-fundamentals/localization/index.md)
- [Локализация в iOS](~/ios/app-fundamentals/localization/index.md)
