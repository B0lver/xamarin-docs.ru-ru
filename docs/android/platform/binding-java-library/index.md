---
title: Привязка библиотеки Java
description: Сообщество Android имеет множество библиотек Java, которые можно использовать в приложении. В этом руководство объясняется, как внедрить библиотеки Java в приложение Xamarin.Android, создав библиотеку привязок.
ms.prod: xamarin
ms.assetid: B39FF1D5-69C3-8A76-D268-C227A23C9485
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 05/01/2017
ms.openlocfilehash: d40a23076ec8f405e57ec40de47ec9ad2261d85d
ms.sourcegitcommit: b0ea451e18504e6267b896732dd26df64ddfa843
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/13/2020
ms.locfileid: "73027603"
---
# <a name="binding-a-java-library"></a>Привязка библиотеки Java

_Сообщество Android имеет множество библиотек Java, которые можно использовать в приложении. В этом руководство объясняется, как внедрить библиотеки Java в приложение Xamarin.Android, создав библиотеку привязок._

## <a name="overview"></a>Обзор

Экосистема библиотек сторонних производителей для Android уже очень велика. С учетом этого часто лучше использовать готовую библиотеку Android, чем создавать новую. Xamarin.Android предлагает два способа использования таких библиотек:

- Создайте *библиотеку привязок*, которая автоматически заключает библиотеку в программы-оболочки C#, позволяя обращаться к коду Java с помощью вызовов C#.

- Используйте *собственный интерфейс Java* (*JNI*), чтобы отправлять вызовы напрямую в библиотеку кода Java. Платформа программирования JNI позволяет вызывать код Java из собственных приложений или библиотек. Она также является вызываемой.

В этом руководстве мы опишем первый вариант, то есть создание *библиотеки привязок*, которая заключает одну или несколько существующих библиотек Java в сборку, связанную с приложением. См. сведения об [использовании JNI](~/android/platform/java-integration/working-with-jni.md).

Xamarin.Android реализует привязки через *вызываемые программы-оболочки Managed Callable Wrappers* (*MCW*). MCW служат мостом для JNI, который используется при вызове кода Java из управляемого кода. Вызываемые оболочки управляемого кода поддерживают также подклассы типов Java и переопределение виртуальных методов для типов Java. Аналогично, когда управляемый код нужно вызывать из кода среды выполнения Android, это выполняется через другой мост JNI — Android Callable Wrappers (ACW). Эта [архитектура](~/android/internals/architecture.md) показана на следующей схеме.

[![Архитектура моста JNI в Android](images/architecture.png)](images/architecture.png#lightbox)

Библиотека привязок — это сборка, содержащая вызываемые оболочки управляемого кода для типов Java. Предположим, нам нужно заключить тип Java `MyClass` в библиотеку привязок:

```java
package com.xamarin.mycode;

public class MyClass
{
    public String myMethod (int i) { ... }
}
```

Создав библиотеку привязок для **JAR**-файла, содержащую `MyClass`, мы сможем создать его экземпляр и обращаться к его методам прямо из кода C#:

```csharp
var instance = new MyClass ();

string result = instance.MyMethod (42);
```

Чтобы создать библиотеку привязок, вам потребуется шаблон *библиотеки привязок Java* для Xamarin.Android. Полученный таким образом проект привязки содержит сборку .NET с несколькими классами MCW, один или несколько **JAR**-файлов и ресурсы для встроенных проектов библиотеки Android. Вы также можете создать библиотеки привязок для AAR-файлов (архив Android) и проектов библиотеки Android для Eclipse. Указывая ссылку на полученный DLL-файл сборки библиотеки привязок, вы сможете применить существующую библиотеку Java в проекте Xamarin.Android.

Создавая ссылки на типы из библиотеки привязок, нужно указывать пространство имен этой библиотеки привязок. Обычно директива `using` добавляется в начало файла исходного кода на C#, что является версией пространства имен .NET для имени пакета Java. Например, у пакета Java для вашего файла **.jar** привязки может быть следующее имя:

```csharp
com.company.package
```

Тогда вам потребуется включить следующий оператор `using` в начало файла с исходным кодом C#, чтобы получить доступ к типам из **JAR**-файла привязки:

```csharp
using Com.Company.Package;
```

При создании привязки для существующей библиотеки Android необходимо учитывать следующие моменты.

- **Существуют ли внешние зависимости у этой библиотеки?** &ndash; Все зависимости Java, указанные для библиотеки Android, должны быть включены в проект Xamarin.Android в виде **ReferenceJar** или **EmbeddedReferenceJar**. Все собственные сборки должны быть добавлены в проект привязки как **EmbeddedNativeLibrary**.  

- **Для какой версии Android API предназначена библиотека Android?** &ndash; Нет возможности понизить уровень API Android, поэтому сразу проект Xamarin.Android должен предназначаться для использования с тем же (или более высоким) уровнем API, который использует подключаемая библиотека Android.

- **Какая версия JDK использовалась для компиляции этой библиотеки?** &ndash; Ошибки привязки могут быть связаны с тем, что библиотека Android создавалась не с той версией JDK, которую использует Xamarin.Android. Если есть возможность, перекомпилируйте библиотеку Android, используя ту же версию пакета JDK, которая используется в вашей установке Xamarin.Android.

## <a name="build-actions"></a>Действия при сборке

При создании библиотеки привязок вы указываете *действия сборки* для файлов **.jar** или .aar, которые включаются в этот проект библиотеки привязок. Каждое из действий привязок определяет метод внедрения файлов **.jar** и .aar в библиотеку привязок. Эти действия сборки представлены в следующем списке.

- `EmbeddedJar` &ndash; внедряет **.jar** в полученный DLL-файл библиотеки привязок в виде внедренного ресурса. Это самое простое и самое распространенное действие сборки. Используйте этот вариант, если вам нужно автоматически компилировать **.jar** в байтовый код и распространять вместе с библиотекой привязок.

- `InputJar` &ndash; не внедряет **.jar** в полученный DLL-файл библиотеки привязок. В этом варианта DLL-файл будет содержать зависимость времени выполнения от файла **.jar**. Используйте этот вариант, если вы не собираетесь включать **.jar** в библиотеку привязок (например, из-за ограничений лицензирования). Если вы выберете этот вариант, нужно обеспечить присутствие исходного файла **.jar** на всех устройствах, где будет выполняться приложение.

- `LibraryProjectZip` &ndash; внедряет файл .aar в полученный DLL-файл библиотеки привязок. Этот вариант аналогичен использованию EmbeddedJar за исключением того, что вы можете обращаться к ресурсам и коду из привязанного файла .aar. Выберите этот вариант, если вы решили включить файл .aar в библиотеку привязок.

- `ReferenceJar` &ndash; указывает ссылочный **.jar**, то есть такой файл **.jar**, от которого зависит включенный в привязку файл **.jar** или файл .aar. Ссылочный файл **.jar** используется только для разрешения зависимостей времени компиляции. Если вы укажете такое действие сборки, привязки C# для ссылочного файла **.jar** не создаются и он не включается в итоговый DLL-файл библиотеки привязок. Используйте этот вариант, если вы собираетесь создать дополнительную библиотеку привязок для ссылочного файла **.jar**, но она еще не готова. Это действие сборки удобно для упаковки нескольких файлов **.jar** и (или) файлов .aar в несколько взаимозависимых библиотек привязки.

- `EmbeddedReferenceJar` &ndash; внедряет ссылочный файл **.jar** в полученный DLL-файл библиотеки привязок. Используйте это действие сборки, если вы собираетесь создать в библиотеке привязок привязки C# как для исходного файла **.jar** или .aar, так и для всех файлов **.jar**, на которые он ссылается.

- `EmbeddedNativeLibrary` &ndash; внедряет в привязку собственный файл **.so**. Это действие сборки применяется для файлов **.so**, от которых зависит файл **.jar**, для которого создается привязка. Возможно, вам придется вручную загрузить библиотеку **.so** перед выполнением кода из библиотеки Java. Это действие описано ниже.

Более подробно действия сборки описаны в следующих руководствах.

Кроме того, указанные ниже действия сборки помогут вам импортировать документацию по API для Java и преобразовать их в документацию XML для C#.

- `JavaDocJar` используется для ссылки на файл .jar архива Javadoc для библиотеки Java, которая соответствует стилю пакетов Maven (обычно это `FOOBAR-javadoc**.jar**`).
- `JavaDocIndex` используется для ссылки на файл `index.html` в справочной документации HTML по API.
- `JavaSourceJar` дополняет `JavaDocJar`, чтобы сначала создать JavaDoc из исходных документов, а затем применить полученный результат как `JavaDocIndex` для библиотеки Java, которая соответствует стилю пакетов Maven (обычно это `FOOBAR-sources**.jar**`).

Документация по API должна иметь стандартный формат doclet из пакетов SDK для Java 8, Java 7 или Java 6 (это три разных формата) или стиль DroidDoc.

## <a name="including-a-native-library-in-a-binding"></a>Включение собственной библиотеки в привязку

Возможно, вам потребуется включить библиотеку **.so** в проект привязки Xamarin.Android в составе привязки библиотеки Java. При выполнении кода Java из оболочки Xamarin.Android не сможет вызвать JNI и выдаст ошибку _java.lang.UnsatisfiedLinkError: Native method not found:_ (встроенный метод не обнаружен) в журнале LogCat для приложения.

Чтобы исправить это, вручную загрузите нужную библиотеку **SO** с помощью вызова `Java.Lang.JavaSystem.LoadLibrary`. Предположим для примера, что проект Xamarin.Android использует общую библиотеку **libpocketsphinx_jni.so**, включенную в проект с помощью действия привязки **EmbeddedNativeLibrary**. В этом случае следующий фрагмент кода (который нужно добавить перед использованием общей библиотеки) загрузит нужную библиотеку **SO**.

```csharp
Java.Lang.JavaSystem.LoadLibrary("pocketsphinx_jni");
```

## <a name="adapting-java-apis-to-ceparsl"></a>Адаптация API для Java к использованию C&eparsl;

Генератор привязок Xamarin.Android заменяет некоторые идиомы и шаблоны Java в соответствии с технологиями .NET. В следующем списке описывается сопоставление Java с C#/.NET:

- _Методы получения и настройки_ в Java соответствуют _свойствам_ в .NET.

- _Поля_ в Java соответствуют _свойствам_ в .NET.

- _Прослушиватели и интерфейсы прослушивателей_ в Java соответствуют _событиям_ в .NET. Параметры методов в интерфейсах обратного вызова будут представлены подклассом `EventArgs`.

- _Статический вложенный класс_ в Java соответствует _вложенному классу_ в .NET.

- _Внутренний класс_ в Java соответствует _вложенному классу_ с конструктором экземпляра в C#.

## <a name="binding-scenarios"></a>Сценарии привязки данных

Следующие руководства по привязке данных помогут вам правильно организовать одну или несколько библиотек Java для включения в приложение.

- Руководство по [использованию привязки файла .jar](~/android/platform/binding-java-library/binding-a-jar.md) содержит пошаговую инструкцию по созданию библиотек привязок для файлов **.jar**.

- Руководство по [использованию привязки файла .aar](~/android/platform/binding-java-library/binding-an-aar.md) содержит пошаговую инструкцию по созданию библиотек привязок для файлов .aar. Именно это руководство поможет вам создать привязку для библиотеки Android Studio.

- Руководство по [использованию привязки проекта библиотеки Eclipse](~/android/platform/binding-java-library/binding-a-library-project.md) включает пошаговую инструкцию по созданию библиотек привязок из проектов библиотек Android. Именно это руководство поможет вам создать привязку для проекта библиотеки Eclipse Android.

- Руководство по [настройке привязок](~/android/platform/binding-java-library/customizing-bindings/index.md) описывает модификации, которые нужно вручную внести в привязки для устранения ошибок сборки и получения API, в большей степени соответствующего стандартам C#.

- Руководство по [устранению неполадок с привязками](~/android/platform/binding-java-library/troubleshooting-bindings.md) включает типичные сценарии возникновения ошибок, описывает возможные причины и предлагает рекомендации по их устранению.

## <a name="related-links"></a>Связанные ссылки

- [Работа с JNI](~/android/platform/java-integration/working-with-jni.md)
- [Метаданные GAPI](https://www.mono-project.com/docs/gui/gtksharp/gapi/#metadata)
- [Использование собственных библиотек](~/android/platform/native-libraries.md)
