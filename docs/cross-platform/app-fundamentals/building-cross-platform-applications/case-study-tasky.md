---
title: 'Пример межплатформенного приложения: задача'
description: В этом документе описывается, как приложение, переносимое с помощью задач, было создано и создано в виде кросс-платформенного мобильного приложения. В нем обсуждаются требования приложения, интерфейс, модель данных, основные функциональные возможности, реализация и многое другое.
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 71d5ed3512980086d244acc5a604d7b33a5dd77c
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84571354"
---
# <a name="cross-platform-app-case-study-tasky"></a>Пример межплатформенного приложения: задача

*Tasky* *Переносимая* задача — это простое приложение со списком задач. В этом документе описывается, как он был разработан и создан, следуя указаниям в статье [Создание кросс-платформенных приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) . Обсуждение охватывает следующие области:

<a name="Design_Process"></a>

## <a name="design-process"></a>Процесс проектирования

Рекомендуется создать дорожную карту для того, что нужно достичь, прежде чем начать программировать. Это особенно справедливо для межплатформенной разработки, где вы создаете функциональные возможности, которые будут предоставляться несколькими способами. Начиная с четкого представления о том, что вы создаете, вы экономите время и усилия позже в цикле разработки.

 <a name="Requirements"></a>

### <a name="requirements"></a>Требования

Первым шагом в проектировании приложения является обнаружение требуемых функций. Это могут быть цели высокого уровня или подробные варианты использования. Задача обладает простыми функциональными требованиями:

- Просмотр списка задач
- Добавление, изменение и удаление задач
- Задание состояния задачи "Готово"

Следует рассмотреть возможность использования функций, зависящих от платформы.  Могут ли задачи воспользоваться преимуществами геоограждения iOS или Windows Phone живых плиток? Даже если в первой версии не используются функции, специфичные для платформы, необходимо заранее спланировать, чтобы бизнес-& уровни данных могли их разместить.

 <a name="User_Interface_Design"></a>

### <a name="user-interface-design"></a>Проектирование пользовательского интерфейса

Начните с высокого уровня проекта, который можно реализовать на целевых платформах. Обратите внимание на ограничения пользовательского интерфейса Platform-указанной. Например, в `TabBarController` iOS может отображаться более пяти кнопок, в то время как Windows Phone эквивалент может отображать до четырех.
Нарисуйте экранный поток с помощью выбранного инструмента (документ работает).

 [![](case-study-tasky-images/taskydesign.png "Draw the screen-flow using the tool of your choice paper works")](case-study-tasky-images/taskydesign.png#lightbox)

 <a name="Data_Model"></a>

### <a name="data-model"></a>Модель данных

Знание того, какие данные необходимо сохранить, поможет определить используемый механизм сохраняемости. Сведения о доступных механизмах хранения см. в разделе [межплатформенный доступ к данным](~/cross-platform/app-fundamentals/index.md) и помощь в выборе между ними. Для этого проекта мы будем использовать SQLite.NET.

Задаче необходимо хранить три свойства для каждого "TaskItem":

- **Name** — строка.
- **Заметки** — строка
- **Готово** — логическое значение

 <a name="Core_Functionality"></a>

### <a name="core-functionality"></a>Основные функции

Рассмотрим интерфейс API, который должен использоваться пользовательским интерфейсом для удовлетворения требований. Для списка задач требуются следующие функции:

- **Перечислить все задачи** — отображение главного экрана со списком всех доступных задач
- **Получить одну задачу** — при затронутой строке задачи
- **Сохранение одной задачи** — при редактировании задачи
- **Удаление одной задачи** — при удалении задачи
- **Создание пустой задачи** — при создании новой задачи

Чтобы обеспечить повторное использование кода, этот API должен быть реализован в *переносимой библиотеке классов*.

 <a name="Implementation"></a>

### <a name="implementation"></a>Реализация

После согласования архитектуры приложения подумайте, как ее можно реализовать в качестве кросс-платформенного приложения. Это станет архитектурой приложения. Следуя указаниям в документе [Создание кросс-платформенных приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) , код приложения должен быть разбит на следующие части:

- **Общий код** — это общий проект, содержащий повторно применяемый код для хранения данных задачи; Предоставьте класс модели и API для управления сохранением и загрузкой данных.
- **Код, зависящий от платформы** — зависящие от платформы проекты, которые реализуют собственный пользовательский интерфейс для каждой операционной системы, используя общий код в качестве серверной части.

[![](case-study-tasky-images/taskypro-architecture.png "Platform-specific projects implement a native UI for each operating system, utilizing the common code as the back end")](case-study-tasky-images/taskypro-architecture.png#lightbox)

Эти две части описаны в следующих разделах.

 <a name="Common_(PCL)_Code"></a>

## <a name="common-pcl-code"></a>Стандартный код (PCL)

Переносимая задача использует стратегию переносимой библиотеки классов для совместного использования общего кода. Описание параметров совместного использования кода см. в документе с [параметрами общего кода](~/cross-platform/app-fundamentals/code-sharing.md) .

Весь распространенный код, включая уровень доступа к данным, код базы данных и контракты, помещается в проект библиотеки.

Полный проект PCL показан ниже. Весь код в переносимой библиотеке совместим с каждой целевой платформой. При развертывании каждое собственное приложение будет ссылаться на эту библиотеку.

![](case-study-tasky-images/portable-project.png "When deployed, each native app will reference that library")

На схеме классов ниже показаны классы, сгруппированные по слоям. `SQLiteConnection`Класс — это стандартный код из пакета SQLite-NET. Остальные классы являются пользовательским кодом для задачи. `TaskItemManager`Классы и `TaskItem` представляют API, который предоставляется приложениям для конкретной платформы.

 [![](case-study-tasky-images/classdiagram-core.png "The TaskItemManager and TaskItem classes represent the API that is exposed to the platform-specific applications")](case-study-tasky-images/classdiagram-core.png#lightbox)

Использование пространств имен для разделения уровней помогает управлять ссылками между слоями. Проекты, зависящие от платформы, должны включать только `using` оператор для бизнес-уровня. Уровень доступа к данным и уровень данных должны инкапсулироваться через API, предоставляемый `TaskItemManager` на уровне бизнес-данных.

 <a name="References"></a>

### <a name="references"></a>Ссылки

Переносимые библиотеки классов необходимо использовать на нескольких платформах, каждая из которых имеет различные уровни поддержки для функций платформы и платформы. По этой причине существуют ограничения на то, какие пакеты и библиотеки платформы можно использовать. Например, Xamarin. iOS не поддерживает `dynamic` ключевое слово c#, поэтому Переносимая библиотека классов не может использовать какой-либо пакет, зависящий от динамического кода, даже если такой код будет работать на Android. Visual Studio для Mac не позволит вам добавлять несовместимые пакеты и ссылки, но следует помнить об ограничениях, чтобы избежать непредвиденных сюрпризов.

Примечание. Вы увидите, что проекты ссылаются на библиотеки платформы, которые не использовались. Эти ссылки входят в состав шаблонов проектов Xamarin. При компиляции приложений процесс связывания удаляет код без ссылок, поэтому, несмотря на то, что на него есть `System.Xml` ссылка, он не будет включен в окончательное приложение, так как мы не используем какие-либо функции XML.

 <a name="Data_Layer_(DL)"></a>

### <a name="data-layer-dl"></a>Уровень данных (DL)

Уровень данных содержит код, выполняющий физическое хранение данных — будь то база данных, неструктурированные файлы или другой механизм. Уровень данных задачи состоит из двух частей: библиотеки SQLite-NET и пользовательского кода, добавленного для их подключения.

Для внедрения кода SQLite-NET, предоставляющего интерфейс базы данных объектно-реляционного сопоставления (ORM), задача основана на пакете NuGet-NET (публикуется Федором Креужер). `TaskItemDatabase`Класс наследует от класса `SQLiteConnection` и добавляет необходимые методы создания, чтения, обновления и удаления (CRUD) для чтения и записи данных в SQLite. Это простая шаблонная реализация универсальных методов CRUD, которые можно использовать повторно в других проектах.

`TaskItemDatabase`— Это одноэлементный набор, гарантирующий, что весь доступ выполняется к тому же экземпляру. Блокировка используется для предотвращения параллельного доступа из нескольких потоков.

 <a name="SQLite_on_WIndows_Phone"></a>

#### <a name="sqlite-on-windows-phone"></a>SQLite на Windows Phone

В то время как iOS и Android поставляются с SQLite в составе операционной системы, Windows Phone не содержит совместимого ядра СУБД. Для совместного использования кода на всех трех платформах требуется версия программы SQLite, встроенная в Windows Phone. Дополнительные сведения о настройке проекта Windows Phone для SQLite см. в разделе [Работа с локальной базой данных](~/xamarin-forms/data-cloud/data/databases.md) .

 <a name="Using_an_Interface_to_Generalize_Data_Access"></a>

#### <a name="using-an-interface-to-generalize-data-access"></a>Использование интерфейса для обобщения доступа к данным

Уровень данных зависит от `BL.Contracts.IBusinessIdentity` того, что он может реализовать абстрактные методы доступа к данным, требующие первичного ключа. Любой класс бизнес-слоя, реализующий интерфейс, может быть сохранен на уровне данных.

Интерфейс просто указывает целочисленное свойство, которое будет использоваться в качестве первичного ключа:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Базовый класс реализует интерфейс и добавляет атрибуты SQLite-NET, чтобы пометить его как первичный ключ с автоматическим приращением. Затем любой класс в бизнес-слое, реализующий этот базовый класс, может быть сохранен на уровне данных:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

Примером универсальных методов на уровне данных, использующем интерфейс, является следующий `GetItem<T>` метод:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access"></a>

#### <a name="locking-to-prevent-concurrent-access"></a>Блокировка для предотвращения параллельного доступа

[Блокировка](https://msdn.microsoft.com/library/c5kehkcz(v=vs.100).aspx) реализуется в `TaskItemDatabase` классе для предотвращения параллельного доступа к базе данных. Это позволяет обеспечить сериализацию параллельного доступа из разных потоков (в противном случае компонент пользовательского интерфейса может попытаться прочитать базу данных одновременно с его обновлением в фоновом потоке). Ниже показан пример реализации блокировки.

```csharp
static object locker = new object ();
public IEnumerable<T> GetItems<T> () where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return (from i in Table<T> () select i).ToList ();
    }
}
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

Большая часть кода уровня данных может быть использована повторно в других проектах. Единственный код, зависящий от приложения, в слое является `CreateTable<TaskItem>` вызовом в `TaskItemDatabase` конструкторе.

 <a name="Data_Access_Layer_(DAL)"></a>

### <a name="data-access-layer-dal"></a>Уровень доступа к данным (DAL)

`TaskItemRepository`Класс инкапсулирует механизм хранения данных с помощью строго типизированного API, который позволяет `TaskItem` создавать, удалять, извлекать и обновлять объекты.

 <a name="Using_Conditional_Compilation"></a>

#### <a name="using-conditional-compilation"></a>Использование условной компиляции

Для задания расположения файла класс использует условную компиляцию. это пример реализации расхождения платформы. Свойство, возвращающее путь, компилируется в другой код на каждой платформе. Код и директивы компилятора для конкретной платформы показаны ниже:

```csharp
public static string DatabaseFilePath {
    get {
        var sqliteFilename = "TaskDB.db3";
#if SILVERLIGHT
        // Windows Phone expects a local path, not absolute
        var path = sqliteFilename;
#else
#if __ANDROID__
        // Just use whatever directory SpecialFolder.Personal returns
        string libraryPath = Environment.GetFolderPath(Environment.SpecialFolder.Personal); ;
#else
        // we need to put in /Library/ on iOS5.1+ to meet Apple's iCloud terms
        // (they don't want non-user-generated data in Documents)
        string documentsPath = Environment.GetFolderPath (Environment.SpecialFolder.Personal); // Documents folder
        string libraryPath = Path.Combine (documentsPath, "..", "Library"); // Library folder
#endif
        var path = Path.Combine (libraryPath, sqliteFilename);
                #endif
                return path;
    }
}
```

В зависимости от платформы выходные данные будут иметь значение " <app
path> /Library/TaskDB.db3" для iOS, " <app
path> /Documents/TaskDB.db3" для Android или просто "таскдб. db3" для Windows Phone.

### <a name="business-layer-bl"></a>Бизнес-уровень (BL)

Бизнес-уровень реализует классы модели и фасадной для управления ими.
В задаче модель является `TaskItem` классом и `TaskItemManager` реализует шаблон фасадной для предоставления API для управления `TaskItems` .

 <a name="Façade"></a>

#### <a name="faade"></a>Фасадной

 `TaskItemManager`заключает в оболочку `DAL.TaskItemRepository` для предоставления методов Get, Save и DELETE, на которые будут ссылаться приложения и уровни пользовательского интерфейса.

Бизнес-правила и логика помещаются здесь, если это необходимо, например для всех правил проверки, которые должны быть удовлетворены перед сохранением объекта.

 <a name="API_for_Platform-Specific_Code"></a>

### <a name="api-for-platform-specific-code"></a>API для кода, зависящего от платформы

После написания общего кода пользовательский интерфейс должен быть построен для получения и просмотра данных, предоставляемых им. `TaskItemManager`Класс реализует шаблон фасадной, чтобы предоставить простой API для доступа к коду приложения.

Код, написанный для каждого проекта конкретной платформы, как правило, тесно связан с собственным пакетом SDK этого устройства и имеет доступ только к общему коду, использующему API, определенный `TaskItemManager` . Сюда входят методы и бизнес-классы, предоставляемые им, такие как `TaskItem` .

Изображения не являются общими для разных платформ, но добавляются независимо в каждый проект. Это важно, поскольку каждая платформа обрабатывает образы по-разному, используя разные имена файлов, каталоги и разрешения.

В остальных разделах рассматриваются сведения о реализации пользовательского интерфейса задачи для конкретной платформы.

 <a name="iOS_App"></a>

## <a name="ios-app"></a>Приложение iOS

Существует несколько классов, необходимых для реализации приложения задач iOS с помощью общего проекта PCL для хранения и извлечения данных. Полный проект iOS Xamarin. iOS показан ниже:

 ![](case-study-tasky-images/taskyios-solution.png "iOS project is shown here")

Классы показаны на этой диаграмме, сгруппированные по слоям.

 [![](case-study-tasky-images/classdiagram-android.png "The classes are shown in this diagram, grouped into layers")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References"></a>

### <a name="references"></a>Ссылки

Приложение iOS ссылается на библиотеки пакета SDK для конкретной платформы — например, Xamarin. iOS и однокасание. Dialog-1.

Он также должен ссылаться на `TaskyPortableLibrary` проект PCL.
Список ссылок показан здесь:

 ![](case-study-tasky-images/taskyios-references.png "The references list is shown here")

Уровень приложения и уровень пользовательского интерфейса реализуются в этом проекте с помощью этих ссылок.

 <a name="Application_Layer_(AL)"></a>

### <a name="application-layer-al"></a>Уровень приложения (AL)

Уровень приложения содержит классы для конкретных платформ, необходимые для привязки объектов, предоставляемых PCL, к пользовательскому интерфейсу. Приложение для iOS имеет два класса, помогающие отображать задачи:

- **Едитингсаурце** — этот класс используется для привязки списков задач к пользовательскому интерфейсу. Так как использовался `MonoTouch.Dialog` для списка задач, нам нужно реализовать этот вспомогательный метод, чтобы включить функции считывания в `UITableView` . Прокрутка на удаление встречается в iOS, но не в Android или Windows Phone, поэтому проект, предназначенный для iOS, является единственным, который его реализует.
- **Таскдиалог** — этот класс используется для привязки одной задачи к пользовательскому интерфейсу. Он использует `MonoTouch.Dialog` API отражения для "упаковки" `TaskItem` объекта с классом, который содержит правильные атрибуты, чтобы обеспечить правильное форматирование экрана ввода.

`TaskDialog`Класс использует `MonoTouch.Dialog` атрибуты для создания экрана на основе свойств класса. Класс выглядит следующим образом:

```csharp
public class TaskDialog {
    public TaskDialog (TaskItem task)
    {
        Name = task.Name;
        Notes = task.Notes;
        Done = task.Done;
    }
    [Entry("task name")]
    public string Name { get; set; }
    [Entry("other task info")]
    public string Notes { get; set; }
    [Entry("Done")]
    public bool Done { get; set; }
    [Section ("")]
    [OnTap ("SaveTask")]    // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Save;
    [Section ("")]
    [OnTap ("DeleteTask")]  // method in HomeScreen
    [Alignment (UITextAlignment.Center)]
    public string Delete;
}
```

Обратите внимание, что для `OnTap` атрибутов требуется имя метода — эти методы должны существовать в классе, где `MonoTouch.Dialog.BindingContext` создается (в данном случае `HomeScreen` класс, обсуждаемый в следующем разделе).

 <a name="User_Interface_Layer_(UI)"></a>

### <a name="user-interface-layer-ui"></a>Уровень пользовательского интерфейса

Слой пользовательского интерфейса состоит из следующих классов:

1. **AppDelegate** — содержит вызовы API внешнего вида для стиля шрифтов и цветов, используемых в приложении. Задача — это простое приложение, которое не выполняет никаких других задач инициализации в `FinishedLaunching` .
2. **Экраны** — подклассы `UIViewController` , определяющие каждый экран и его поведение. Экраны связывают интерфейс пользователя с классами уровня приложения и общим API ( `TaskItemManager` ). В этом примере экраны создаются в коде, но они могут быть спроектированы с помощью Interface Builder или конструктора раскадровки Xcode.
3. **Изображения** — визуальные элементы являются важной частью каждого приложения. Задача содержит экран-заставку и изображения значков, которые для iOS должны предоставляться в обычном и Retinaм разрешении.

 <a name="Home_Screen"></a>

#### <a name="home-screen"></a>Главная страница

Начальный экран — это `MonoTouch.Dialog` экран, на котором отображается список задач из базы данных SQLite. Он наследует от `DialogViewController` и реализует код, чтобы установить в значение, `Root` содержащее коллекцию `TaskItem` объектов для вывода.

 [![](case-study-tasky-images/ios-taskylist.png "It inherits from DialogViewController and implements code to set the Root to contain a collection of TaskItem objects for display")](case-study-tasky-images/ios-taskylist.png#lightbox)

Ниже приведены два основных метода отображения списка задач и взаимодействия с ним.

1. **Популатетабле** — использует метод бизнес-уровня `TaskManager.GetTasks` для получения коллекции `TaskItem` объектов для вывода.
2. **Выбрано** — когда строка затронута, отображает задачу на новом экране.

 <a name="Task_Details_Screen"></a>

#### <a name="task-details-screen"></a>Экран сведений о задаче

Сведения о задаче — это экран ввода, позволяющий изменять или удалять задачи.

Задача использует `MonoTouch.Dialog` API отражения для отображения экрана, поэтому нет `UIViewController` реализации. Вместо этого `HomeScreen` класс создает экземпляр и отображает `DialogViewController` с помощью класса на `TaskDialog` уровне приложения.

На этом снимке экрана показан пустой экран, демонстрирующий `Entry` атрибут установки текста водяного знака в полях " **имя** " и " **Примечания** ":

 [![](case-study-tasky-images/ios-taskydetail.png "This screenshot shows an empty screen that demonstrates the Entry attribute setting the watermark text in the Name and Notes fields")](case-study-tasky-images/ios-taskydetail.png#lightbox)

Функциональность экрана **сведений о задачах** (например, сохранение или удаление задачи) должна быть реализована в `HomeScreen` классе, так как в этом случае `MonoTouch.Dialog.BindingContext` создается. `HomeScreen`Экран сведений о задачах поддерживается следующими методами.

1. **Шовтаскдетаилс** — создает объект `MonoTouch.Dialog.BindingContext` для отрисовки экрана. Он создает экран ввода с помощью отражения для получения имен и типов свойств из `TaskDialog` класса. Дополнительные сведения, например текст водяного знака для полей ввода, реализуются с помощью атрибутов свойств.
2. **Саветаск** — на этот метод имеется ссылка в `TaskDialog` классе через `OnTap` атрибут. Он вызывается при нажатии кнопки **Save** и использует `MonoTouch.Dialog.BindingContext` для получения данных, вводимых пользователем, перед сохранением изменений с помощью `TaskItemManager` .
3. **Делететаск** — на этот метод имеется ссылка в `TaskDialog` классе через `OnTap` атрибут. Он использует `TaskItemManager` для удаления данных с помощью первичного ключа (свойство ID).

 <a name="Android_App"></a>

## <a name="android-app"></a>Приложение Android

Полный проект Xamarin. Android приведен ниже.

 ![](case-study-tasky-images/taskyandroid-solution.png "Android project is pictured here")

Схема классов с классами, сгруппированными по слоям:

 [![](case-study-tasky-images/classdiagram-android.png "The class diagram, with classes grouped by layer")](case-study-tasky-images/classdiagram-android.png#lightbox)

 <a name="References"></a>

### <a name="references"></a>Ссылки

Проект приложения Android должен ссылаться на сборку Xamarin. Android, относящуюся к конкретной платформе, для доступа к классам из пакет SDK для Android.

Он также должен ссылаться на проект PCL (например, Таскипортаблелибрари) для доступа к общим данным и коду бизнес-уровня.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary to access the common data and business layer code")

 <a name="Application_Layer_(AL)"></a>

### <a name="application-layer-al"></a>Уровень приложения (AL)

Как и в версии iOS, рассмотренной ранее, уровень приложения в версии Android содержит классы для конкретных платформ, необходимые для привязки объектов, предоставляемых ядром, к пользовательскому интерфейсу.

 **Тасклистадаптер** — для вывода списка \<T> объектов, необходимых для реализации адаптера для вывода пользовательских объектов в `ListView` . Адаптер определяет, какой макет используется для каждого элемента в списке. в этом случае код использует встроенный макет Android `SimpleListItemChecked` .

 <a name="User_Interface_(UI)"></a>

### <a name="user-interface-ui"></a>Пользовательский интерфейс

Слой пользовательского интерфейса приложения Android представляет собой сочетание кода и XML-разметки.

- **Ресурсы и макет** — макеты экрана и структура ячеек строк, реализованная как AXML-файлы. AXML может быть написана вручную или отображаться визуально с помощью конструктора пользовательского интерфейса Xamarin для Android.
- **Ресурсы и нарисованные** — изображения (значки) и настраиваемая кнопка.
- **Экраны** — подклассы действия, которые определяют каждый экран и его поведение. Связывает вместе пользовательский интерфейс с классами уровня приложения и общим API ( `TaskItemManager` ).

 <a name="Home_Screen"></a>

#### <a name="home-screen"></a>Главная страница

Начальный экран состоит из подкласса действия `HomeScreen` и `HomeScreen.axml` файла, определяющего макет (положение кнопки и списка задач). Экран выглядит следующим образом:

 [![](case-study-tasky-images/android-taskylist.png "The screen looks like this")](case-study-tasky-images/android-taskylist.png#lightbox)

В коде на начальном экране определяются обработчики для нажатия кнопки и выбора элементов в списке, а также заполнения списка в `OnResume` методе (чтобы он отражал изменения, внесенные на экране сведений о задаче). Данные загружаются с использованием бизнес-уровня `TaskItemManager` и `TaskListAdapter` с уровня приложения.

 <a name="Task_Details_Screen"></a>

#### <a name="task-details-screen"></a>Экран сведений о задаче

Экран сведения о задаче также состоит из `Activity` подкласса и файла макета AXML. Макет определяет расположение элементов управления вводом, а класс C# определяет поведение для загрузки и сохранения `TaskItem` объектов.

 [![](case-study-tasky-images/android-taskydetail.png "The class defines the behavior to load and save TaskItem objects")](case-study-tasky-images/android-taskydetail.png#lightbox)

Все ссылки на библиотеку PCL проходят через `TaskItemManager` класс.

 <a name="Windows_Phone_App"></a>

## <a name="windows-phone-app"></a>Приложение Windows Phone
Полный проект Windows Phone:

 ![](case-study-tasky-images/taskywp7-solution.png "Windows Phone App The complete Windows Phone project")

На приведенной ниже схеме представлены классы, сгруппированные по уровням.

 [![](case-study-tasky-images/classdiagram-wp7.png "This diagram presents the classes grouped into layers")](case-study-tasky-images/classdiagram-wp7.png#lightbox)

 <a name="References"></a>

### <a name="references"></a>Ссылки

Проект для конкретной платформы должен ссылаться на необходимые библиотеки для конкретной платформы (например `Microsoft.Phone` , и `System.Windows` ) для создания допустимого Windows Phone приложения.

Он также должен ссылаться на проект PCL (например, `TaskyPortableLibrary`) для использования `TaskItem` класса и базы данных.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary to utilize the TaskItem class and database")

 <a name="Application_Layer_(AL)"></a>

### <a name="application-layer-al"></a>Уровень приложения (AL)

Опять же, как и в версиях iOS и Android, уровень приложения состоит из невизуальных элементов, помогающих привязать данные к пользовательскому интерфейсу.

 <a name="ViewModels"></a>

#### <a name="viewmodels"></a>Модели представлений

ViewModels переносит данные из PCL ( `TaskItemManager` ) и представляет их способом, который может использоваться при привязке данных Silverlight или XAML. Это пример поведения, зависящего от платформы (как описано в документе о кросс-платформенных приложениях).

 <a name="User_Interface_(UI)"></a>

### <a name="user-interface-ui"></a>Пользовательский интерфейс

XAML имеет уникальную возможность привязки данных, которую можно объявить в разметке и уменьшить объем кода, необходимого для просмотра объектов:

1. **Страницы** — файлы XAML и их фоновый код определяют пользовательский интерфейс и ссылаются на проекты VIEWMODEL и PCL для просмотра и получения данных.
2. **Изображения** : экран-заставка, изображения фона и значков являются ключевой частью пользовательского интерфейса.

 <a name="MainPage"></a>

#### <a name="mainpage"></a>MainPage

Класс MainPage использует `TaskListViewModel` для вывода данных с помощью функций привязки данных XAML. `DataContext`Для страницы задана модель представления, которая заполняется асинхронно. `{Binding}`Синтаксис в XAML определяет, как отображаются данные.

 <a name="TaskDetailsPage"></a>

#### <a name="taskdetailspage"></a>таскдетаилспаже

Каждая задача отображается путем привязки `TaskViewModel` к XAML, определенному в таскдетаилспаже. XAML. Данные задачи извлекаются с помощью `TaskItemManager` в бизнес-слое.

 <a name="Results"></a>

## <a name="results"></a>Результаты

Итоговые приложения выглядят следующим образом на каждой платформе:

 <a name="iOS"></a>

### <a name="ios"></a>iOS

Приложение использует стандартную структуру пользовательского интерфейса iOS, например кнопку "Добавить", расположенную на панели навигации, и используя встроенный значок " **плюс" (+)** . Он также использует `UINavigationController` поведение кнопки "назад" по умолчанию и поддерживает "Прокрутка на удаление" в таблице.

 [![](case-study-tasky-images/ios-taskylist.png "Он также использует поведение Уинавигатионконтроллер по умолчанию для кнопки возврата и поддерживает прокрутку на удаление в таблице.")](case-study-tasky-images/ios-taskylist.png#lightbox) [![](case-study-tasky-images/ios-taskylist.png "Он также использует поведение Уинавигатионконтроллер по умолчанию для кнопки возврата и поддерживает прокрутку на удаление в таблице.")](case-study-tasky-images/ios-taskylist.png#lightbox)

 <a name="Android"></a>

### <a name="android"></a>Android

Приложение Android использует встроенные элементы управления, включая встроенный макет для строк, требующих отображения "Tick". В дополнение к экранной кнопке «Back» поддерживается поведение оборудования и системы обратно.

 [![](case-study-tasky-images/android-taskylist.png "The hardware/system back behavior is supported in addition to an on-screen back button")](case-study-tasky-images/android-taskylist.png#lightbox)[![](case-study-tasky-images/android-taskylist.png "The hardware/system back behavior is supported in addition to an on-screen back button")](case-study-tasky-images/android-taskylist.png#lightbox)

 <a name="Windows_Phone"></a>

### <a name="windows-phone"></a>Windows Phone

Приложение Windows Phone использует стандартный макет, заполняя панель приложения в нижней части экрана вместо панели навигации вверху.

 [![](case-study-tasky-images/wp-taskylist.png "Приложение Windows Phone использует стандартный макет, заполняя панель приложения в нижней части экрана вместо панели навигации в верхней части окна")](case-study-tasky-images/wp-taskylist.png#lightbox) [![](case-study-tasky-images/wp-taskylist.png "Приложение Windows Phone использует стандартный макет, заполняя панель приложения в нижней части экрана вместо панели навигации в верхней части окна")](case-study-tasky-images/wp-taskylist.png#lightbox)

 <a name="Summary"></a>

## <a name="summary"></a>Сводка

В этом документе представлено подробное описание принципов применения структур многоуровневых приложений к простому приложению для упрощения повторного использования кода на трех мобильных платформах: iOS, Android и Windows Phone.

В нем описан процесс разработки уровней приложений и обсуждаются функции кода, &amp; реализованные на каждом уровне.

Код можно скачать с сайта [GitHub](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Связанные ссылки

- [Создание кросс-платформенных приложений (основной документ)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Переносимый пример приложения с задачами (GitHub)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)
