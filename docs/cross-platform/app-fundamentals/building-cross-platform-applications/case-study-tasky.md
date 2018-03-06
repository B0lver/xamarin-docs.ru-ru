---
title: "Практический пример: Tasky"
description: "В этом документе описывается применение принципы создание кросс-платформенных приложений в Tasky переносимой образец приложения. Она затрагивает Разработка мобильных приложений, написания общего кода для повторного использования и реализация специфический для платформы проектов, предназначенных для iOS, Android и Windows Phone платформы."
ms.topic: article
ms.prod: xamarin
ms.assetid: B581B2D0-9890-C383-C654-0B0E12DAD5A6
ms.technology: xamarin-cross-platform
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: 5bd19e04934f6b86143c93c759c0c2ac76956a7e
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="case-study-tasky"></a>Практический пример: Tasky

_В этом документе описывается применение принципы создание кросс-платформенных приложений в Tasky переносимой образец приложения. Она затрагивает Разработка мобильных приложений, написания общего кода для повторного использования и реализация специфический для платформы проектов, предназначенных для iOS, Android и Windows Phone платформы._

*Tasky* *переносимой* — это простая задача список приложение. В этом документе рассматривается, как оно было разработано и построена, следуя указаниям из [создание кросс-платформенных приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) документа. Обсуждение охватывают следующие области:


### <a name="design"></a>Конструктор

В этом разделе описывается общий подход для запуска нового проекта кросс платформенных мобильных приложений, таких как создание требования, создание экрана макеты и Определение ключевых возможностей код.

 <a name="Common_Code" />


### <a name="common-code"></a>Общий код

Объясняет, как кросс платформенных создается база данных и бизнес-уровня. Объект `TaskItemManager` класс записывается предоставляют простой «API», доступ к которому уровень пользовательского интерфейса. Сведения о общий код реализации инкапсулируется `TaskItemManager` и `TaskItem` классы, которые он возвращает. Каждая платформа обращается к общий код из **переносимой Library(PCL) класса**

 <a name="Platform-Specific_Applications" />


### <a name="platform-specific-applications"></a>Специфический для платформы приложений

Tasky выполняется на iOS, Android и Windows Phone. Каждое приложение платформой реализует собственный пользовательский интерфейс для выполнения следующих:

1.  Отображение списка задач
2.  Создание, изменение, сохранение и удаление задачи.

 <a name="Design_Process" />


# <a name="design-process"></a>Процесс разработки

Рекомендуется создать для карты дорог, необходимо добиться, прежде чем начать кодирование. Это особенно верно для кросс платформенной разработки, где создается функциональные возможности, предоставляемого несколькими способами. Начиная с очистить представление о том, что вы создаете экономит время и усилия позже на стадии разработки.

 <a name="Requirements" />


## <a name="requirements"></a>Требования

Первым шагом при разработке приложения — для определения необходимых компонентов. Это могут быть высокоуровневые цели или подробно описаны варианты использования. Tasky имеет простой функциональных требований.

 -  Просмотр списка задач
 -  Добавление, изменение и удаление задач
 -  Задать состояние задач «выполнить»


Рассмотрите возможность использования функций конкретную платформу.  Можно Tasky воспользоваться преимуществами зонирование iOS или Windows Phone Live плитки? Даже если вы не используете специфический для платформы функции в первой версии, следует запланировать заранее убедитесь, что business & уровни данных может изменяться соответственно.

 <a name="User_Interface_Design" />


## <a name="user-interface-design"></a>Проектирование пользовательского интерфейса

Начните с Структура высокого уровня, который может быть реализован на целевых платформах. Будьте внимательны, чтобы примечание содержится код платформы пользовательского интерфейса ограничения. Например `TabBarController` в iOS могут отображаться более пяти кнопок, тогда как эквивалент Windows Phone может отображаться до четырех.
Рисование экрана потока, используя средство по вашему выбору (бумаги работает).

 [ ![](case-study-tasky-images/taskydesign.png "Рисование последовательности экранов, с помощью средства works бумаги ваш выбор")](case-study-tasky-images/taskydesign.png)

 <a name="Data_Model" />


## <a name="data-model"></a>Модель данных

Узнать, какие данные должны храниться помогут определить механизм сохраняемости. В разделе [доступа к данным кросс-платформенных](~/cross-platform/app-fundamentals/index.md) сведения о механизмы доступное хранилище и помощи в выборе между ними. Для этого проекта мы будем работать с SQLite.NET.

Tasky необходимо хранить три свойства для каждого TaskItem:

 -  **Имя** — строка
 -  **Заметки о** — строка
 -  **Сделать** — логический


 <a name="Core_Functionality" />


## <a name="core-functionality"></a>Основные функции

Рассмотрите API, пользовательский интерфейс будет необходимо использовать в соответствии с требованиями. Список дел требуются следующие функции:

 -   **Список всех задач** — для отображения списка всех доступных задач основного экрана
 -  **Получить одну задачу** — когда входящая строка задачи
 -  **Сохраните одну задачу** — при изменении задачи
 -  **Удаление одной задачи** — при удалении задачи
 -  **Создать пустую задачу** — при создании новой задачи


Чтобы обеспечить повторное использование кода, этот интерфейс API должен быть реализован один раз в *переносимой библиотеки классов*.

 <a name="Implementation" />


## <a name="implementation"></a>Реализация

После разработки приложения согласованных рассмотрим, как он может быть реализован в виде кросс платформенные приложения. Это может быть архитектура приложения. Следуйте инструкциям в [создание кросс-платформенных приложений](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md) документа, код приложения должен быть неработающие вниз на следующие части:

 -   **Общий код** — общие проекта, содержит код повторно используется для хранения данных задачи, предоставляют класс модели и API-Интерфейс для управления сохранение и загрузку данных.
 -   **Код платформы** — проекты под конкретные платформы, реализующих собственного пользовательского интерфейса для каждой операционной системы, используя общий код в качестве серверной части.


 [ ![](case-study-tasky-images/taskypro-architecture.png "Проекты под конкретные платформы реализовать собственный пользовательский Интерфейс для каждой операционной системы, используя общий код в качестве серверной части")](case-study-tasky-images/taskypro-architecture.png)

В следующих разделах описываются эти две части.

 <a name="Common_(PCL)_Code" />


# <a name="common-pcl-code"></a>Общий код (PCL)

Tasky Portable использует стратегию переносимой библиотеки классов для совместного использования общего кода. В разделе [варианты совместного использования кода](~/cross-platform/app-fundamentals/code-sharing.md) документа описание варианты совместного использования кода.

Весь общий код, включая уровень доступа к данным, кода базы данных и контракты помещается в проекте библиотеки.


Ниже показана весь проект PCL. Весь код в переносимой библиотеке совместим с каждой целевой платформы. При развертывании каждого собственного приложения будет ссылаться на эту библиотеку.

![](case-study-tasky-images/portable-project.png "При развертывании каждого собственного приложения будет ссылаться на эту библиотеку")

Класс приведенной ниже схеме показана классы, сгруппированные по слоя. `SQLiteConnection` Класс является стандартный код из пакета Sqlite NET. Остальные классы находятся пользовательский код для Tasky. `TaskItemManager` И `TaskItem` классы представляют API, который предоставляется специфический для платформы приложений.

 [ ![](case-study-tasky-images/classdiagram-core.png "Классы TaskItemManager и TaskItem представляют API, который предоставляется специфический для платформы приложений")](case-study-tasky-images/classdiagram-core.png)

Использование пространств имен с отдельными слоями, помогает управлять ссылки между каждого слоя. Проекты под конкретные платформы необходимо только включить `using` инструкции для бизнес-слое. Уровень доступа к данным и уровень данных должны инкапсулироваться интерфейсом API, предоставляемый `TaskItemManager` бизнес-слое.

 <a name="References" />


## <a name="references"></a>Ссылки

Переносимые библиотеки классов должны работать на различных платформах с различными уровнями поддержки функций платформы и инфраструктуры. Поэтому существуют ограничения, в которых можно использовать пакеты и библиотеки платформы. Например, не поддерживает Xamarin.iOS c# `dynamic` ключевое слово, поэтому переносимой библиотеке классов не может использовать любой пакет, который зависит от динамического кода, несмотря на то, что такой код будет работать на Android. Visual Studio для Mac не позволяет добавление несовместимых пакетов и ссылок, но необходимо учитывать ограничения во избежание непредвиденных ситуаций позже.

Примечание: Вы увидите что проектов ссылаются framework библиотек, которые еще не используется. Эти ссылки включены в состав шаблонов проектов Xamarin. При компиляции приложения процесс связывания приведет к удалению кода без ссылок, поэтому, даже если `System.Xml` был ссылки, он не включается в конечного приложения, так как мы не используете какие-либо функции Xml.

 <a name="Data_Layer_(DL)" />


## <a name="data-layer-dl"></a>Уровень данных (DL)

Уровень данных содержит код, выполняющий физического хранилища данных — для базы данных, неструктурированные файлы или другого механизма. Уровень Tasky данных состоит из двух частей: SQLite-NET-library и добавить его подключить пользовательский код.

Tasky зависит от пакета nuget Sqlite net (опубликованное Фрэнк Kreuger) для внедрения SQLite NET код, который предоставляет интерфейс объектно-реляционного сопоставления (ORM) базы данных. `TaskItemDatabase` Класс наследует от `SQLiteConnection` и добавляет необходимые создание, чтение, обновление, удаление (CRUD) методы для чтения и записи данных в SQLite. Он является реализацией простой стандартного универсальных методов CRUD, можно повторно использовать в других проектах.

`TaskItemDatabase` Является одноэлементным множеством, гарантируя, что все доступа происходит относительно тот же экземпляр. Чтобы предотвратить одновременный доступ из нескольких потоков используется блокировка.

 <a name="SQLite_on_WIndows_Phone" />


### <a name="sqlite-on-windows-phone"></a>SQLite на Windows Phone

При iOS и Android, которые поставляются с SQLite как часть операционной системы Windows Phone отсутствует совместимый СУБД. Для совместного использования кода для всех трех платформ требуется версия Windows phone в собственном коде SQLite. В разделе [работа с локальной базы данных](~/xamarin-forms/app-fundamentals/databases.md) Дополнительные сведения о настройке проекта Windows Phone для Sqlite.

 <a name="Using_an_Interface_to_Generalize_Data_Access" />


### <a name="using-an-interface-to-generalize-data-access"></a>С помощью интерфейса, чтобы обобщить доступа к данным

Уровень данных зависит от `BL.Contracts.IBusinessIdentity` , чтобы его можно реализовать методы доступа абстрактный данных, которые требуют наличия первичного ключа. Любой класс бизнес-уровне, который реализует интерфейс затем могут быть сохранены на уровне данных.

Интерфейс определяет только целочисленное свойство в качестве первичного ключа:

```csharp
public interface IBusinessEntity {
    int ID { get; set; }
}
```

Базовый класс реализует интерфейс и добавляет атрибуты SQLite NET, чтобы пометить его как первичный ключ с автоматическим приращением. Любой класс, реализующий этот базовый класс бизнес-слое могут затем сохраняться на уровне данных:

```csharp
public abstract class BusinessEntityBase : IBusinessEntity {
    public BusinessEntityBase () {}
    [PrimaryKey, AutoIncrement]
    public int ID { get; set; }
}
```

Пример универсальных методов на уровне данных, которые используют интерфейс — это `GetItem<T>` метод:

```csharp
public T GetItem<T> (int id) where T : BL.Contracts.IBusinessEntity, new ()
{
    lock (locker) {
        return Table<T>().FirstOrDefault(x => x.ID == id);
    }
}
```

 <a name="Locking_to_prevent_Concurrent_Access" />


### <a name="locking-to-prevent-concurrent-access"></a>Блокировки для предотвращения одновременного доступа

Объект [блокировки](http://msdn.microsoft.com/en-us/library/c5kehkcz(v=vs.100).aspx) реализуется в `TaskItemDatabase` класса, чтобы предотвратить одновременный доступ к базе данных. Это гарантирует сериализуется одновременный доступ из различных потоков (в противном случае компонент пользовательского интерфейса может пытаться чтение из базы данных, в то же время, обновляется в фоновом потоке). Здесь показан пример реализации блокировки:

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

Большая часть кода уровня данных может использоваться повторно в других проектах. Код только от приложения в слое- `CreateTable<TaskItem>` вызов в `TaskItemDatabase` конструктора.

 <a name="Data_Access_Layer_(DAL)" />


## <a name="data-access-layer-dal"></a>Уровень доступа к данным (DAL)

`TaskItemRepository` Класс инкапсулирует механизм хранения данных со строгой типизацией API, который позволяет `TaskItem` объектов должен быть создан, удален, получать и обновлять.

 <a name="Using_Conditional_Compilation" />


### <a name="using-conditional-compilation"></a>С помощью условной компиляции

Класс использует условной компиляции, чтобы задать расположение файла — это пример реализации расхождение платформы. Свойство, которое возвращает путь компилируется в другой код для каждой платформы. Код и директивы компилятора платформой, показаны ниже:

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

В зависимости от платформы, то выходные данные будут «<app
path>/Library/TaskDB.db3» для iOS, «<app
path>/Documents/TaskDB.db3» для Android или просто «TaskDB.db3» для Windows Phone.

## <a name="business-layer-bl"></a>Бизнес-уровне (белым Списком)

Бизнес-слое реализует классы модели и оболочкой для управления ими.
В Tasky модель является `TaskItem` класса и `TaskItemManager` реализует шаблон «Фасадной», чтобы предоставить API-Интерфейс для управления `TaskItems`.

 <a name="Façade" />


### <a name="faade"></a>Фасадной

 `TaskItemManager` заключает в оболочку `DAL.TaskItemRepository` для предоставления Get, сохранить и удалить методы, которые будет ссылаться приложения и слои пользовательского интерфейса.

Бизнес-правил и логике помещается здесь при необходимости — например все правила проверки, которые должны быть удовлетворены перед сохранением объекта.

 <a name="API_for_Platform-Specific_Code" />


## <a name="api-for-platform-specific-code"></a>API-Интерфейс для кода для конкретной платформы

После записи общий код пользовательского интерфейса должны быть построены для сбора и отображения данных, предоставляемых ею. `TaskItemManager` Класс реализует шаблон оболочкой, предоставляют простой API для кода приложения для доступа к.

Код, написанный в каждом проекте платформой будут обычно тесно связаны на собственный пакет SDK для этого устройства и доступ только к общим кодом с помощью API, определенный по `TaskItemManager`. Сюда входят методы и business класс он предоставляет, такие как `TaskItem`.

Образы не общей для платформ а добавлены независимо для каждого проекта. Это важно, так как каждая платформа обработки изображений по-разному, используя другие имена файлов, каталоги и способы их устранения.

В остальных разделах детали реализации платформой Tasky пользовательского интерфейса.

 <a name="iOS_App" />


# <a name="ios-app"></a>Приложение iOS

Существует лишь небольшое число классов, необходимый для реализации iOS Tasky приложения, использующего общий проект переносимой библиотеки Классов для хранения и извлечения данных. Ниже приводится проекта Xamarin.iOS завершения операций ввода-вывода.

 ![](case-study-tasky-images/taskyios-solution.png "Здесь отображается проект iOS")

Классы отображаются на этой диаграмме группируются в слои.

 [ ![](case-study-tasky-images/classdiagram-android.png "На этой диаграмме группируются в слои отображаются классы")](case-study-tasky-images/classdiagram-android.png)

 <a name="References" />


## <a name="references"></a>Ссылки

Приложения iOS ссылается на библиотеки пакета SDK для конкретных платформ — например. Xamarin.iOS и MonoTouch.Dialog-1.

Он также должен ссылаться `TaskyPortableLibrary` проект переносимой библиотеки Классов.
В списке ссылок на показан ниже:

 ![](case-study-tasky-images/taskyios-references.png "Здесь показан список ссылок")

Уровень приложений и уровень пользовательского интерфейса реализуются в этом проекте с помощью этих ссылок.

 <a name="Application_Layer_(AL)" />


## <a name="application-layer-al"></a>Уровень приложения (п.)

Уровень приложения классы платформой необходимый для привязки объектами, предоставляемыми PCL к пользовательскому Интерфейсу. Приложение iOS конкретных имеет два класса, чтобы отобразить задачи:

 -   **EditingSource** — этот класс используется для привязки к пользовательскому интерфейсу списки задач. Поскольку `MonoTouch.Dialog` был использован для списка задач, нам нужно реализовать этого вспомогательного объекта для включения функции проведите для удаления в `UITableView` . Проведите для удаления распространено на iOS, но не Android или Windows Phone, поэтому конкретного проекта iOS — единственный, который его реализует.
 -   **TaskDialog** — этот класс используется для привязки одну задачу в пользовательский интерфейс. Она использует `MonoTouch.Dialog` API отражения для «wrap» `TaskItem` объекта с классом, который содержит правильные атрибуты, чтобы разрешить экран ввода правильно отформатировать.


`TaskDialog` Класс использует `MonoTouch.Dialog` атрибуты для создания экрана в зависимости от свойств класса. Класс выглядит следующим образом:

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

Обратите внимание `OnTap` атрибуты требует имя метода — эти методы должны существовать в классе где `MonoTouch.Dialog.BindingContext` создается (в этом случае `HomeScreen` классов, описанных в следующем разделе).

 <a name="User_Interface_Layer_(UI)" />


## <a name="user-interface-layer-ui"></a>Уровень пользовательского интерфейса (UI)

Уровень пользовательского интерфейса состоит из следующих классов:

1.   **AppDelegate** — содержит вызовы API внешний вид для стиля шрифты и цвета, используемые в приложении. Tasky простое приложение, поэтому существуют никакие другие задачи инициализации, работающих в `FinishedLaunching` .
2.   **Экраны** — подклассы `UIViewController` , которые определяют каждого экрана и его поведение. Экраны связывают пользовательского интерфейса с классами уровня приложения и общий API ( `TaskItemManager` ). В этом примере создаются экраны в коде, но они могут, были разработаны с помощью Xcode интерфейс построителя или конструктора раскадровки.
3.   **Образы** — визуальные элементы являются важной частью каждого приложения. Tasky имеет изображения заставки экрана и значок, которые должны быть указаны в обычном режиме и Сетчатка разрешения для операций ввода-вывода.


 <a name="Home_Screen" />


### <a name="home-screen"></a>Начальный экран

На экране Главная `MonoTouch.Dialog` экран, отображающий список задач в базе данных SQLite. Он наследуется от `DialogViewController` и выполняет код, чтобы задать `Root` содержать коллекцию `TaskItem` объекты для отображения.

 [ ![](case-study-tasky-images/ios-taskylist.png "Он наследуется от DialogViewController и реализует код, чтобы задать корневой раздел содержит коллекцию объектов TaskItem для отображения")](case-study-tasky-images/ios-taskylist.png)

Ниже приведены два основных метода, относящиеся к отображения данных и работы со списком задач.

1.   **PopulateTable** — использует бизнес-слое `TaskManager.GetTasks` метод для извлечения коллекции `TaskItem` объектов для отображения.
2.   **Выбран** — когда затронутых строк, отображение задачи в новый экран.


 <a name="Task_Details_Screen" />


### <a name="task-details-screen"></a>Экран сведений задач

Сведения о задаче является экран ввода, который позволяет задачам удалены или изменены.

Использует tasky `MonoTouch.Dialog`по API отражения для отображения на экране, значит, будет не `UIViewController` реализации. Вместо этого `HomeScreen` класс создает и отображает `DialogViewController` с помощью `TaskDialog` класса из уровня приложения.

На этом снимке экрана показано пустой экран, который демонстрирует `Entry` атрибут задания текста водяного знака в **имя** и **заметки** поля:

 [ ![](case-study-tasky-images/ios-taskydetail.png "На этом снимке экрана показано пустой экран, демонстрирующий атрибут запись задания текста водяного знака в поля «имя» и заметки")](case-study-tasky-images/ios-taskydetail.png)

Функциональные возможности **сведения о задаче** экрана (например, сохранение или удаление задачи) должен быть реализован в `HomeScreen` класса, так как это место, куда `MonoTouch.Dialog.BindingContext` создается. Следующие `HomeScreen` методы поддерживают экрана сведения о задаче:

1.   **ShowTaskDetails** — создает `MonoTouch.Dialog.BindingContext` для отображения на экране. Он создает экран ввода, использование отражения для получения имен свойств и типов из `TaskDialog` класса. Дополнительные сведения, например текста водяного знака для полей ввода, реализуется с помощью атрибутов в свойствах.
2.   **SaveTask** — этот метод указывается в `TaskDialog` класса через `OnTap` атрибута. Он вызывается, когда **Сохранить** нажата и использует `MonoTouch.Dialog.BindingContext` для извлечения данных, введенных пользователем перед сохранением изменений с помощью `TaskItemManager` .
3.   **DeleteTask** — этот метод указывается в `TaskDialog` класса через `OnTap` атрибута. Она использует `TaskItemManager` для удаления данных с помощью первичного ключа (идентификатор свойство).


 <a name="Android_App" />


# <a name="android-app"></a>Android App

Весь проект полностью Xamarin.Android показанные на рисунке ниже:

 ![](case-study-tasky-images/taskyandroid-solution.png "Приведенное ниже проекта Android")

Схема классов с классами, сгруппированные по слоя:

 [ ![](case-study-tasky-images/classdiagram-android.png "Схема классов, с классами, сгруппированные по слоя")](case-study-tasky-images/classdiagram-android.png)

 <a name="References" />


## <a name="references"></a>Ссылки

Проект приложения Android необходима ссылка на сборку Xamarin.Android платформой, чтобы получить доступ к классам, из пакета SDK для Android.

Она также должна ссылаться в проект PCL (например) TaskyPortableLibrary) для доступа к коду общих данных и бизнес-уровня.

 ![](case-study-tasky-images/taskyandroid-references.png "TaskyPortableLibrary для доступа к коду общих данных и бизнес-уровня")

 <a name="Application_Layer_(AL)" />


## <a name="application-layer-al"></a>Уровень приложения (п.)

Аналогично версии iOS, которую мы рассматривали ранее, уровень приложения в Android версии классы платформой необходимый для привязки объектами, предоставляемыми ядер и пользовательского интерфейса.

 **TaskListAdapter** — для отображения списка<T> объектов, нам нужно реализовать адаптер для отображения пользовательских объектов в `ListView`. Адаптер определяет, какой макет используется для каждого элемента в списке — в этом случае код использует встроенные макетом Android `SimpleListItemChecked`.

 <a name="User_Interface_(UI)" />


## <a name="user-interface-ui"></a>Пользовательский интерфейс (UI)

Уровень пользовательского интерфейса приложения Android представляет собой комбинацию из XML-разметку и код.

 -   **Ресурсы и макет** — макеты экрана и строка ячейки реализован в виде файлов AXML конструктора. AXML может быть вручную записи или размещенным визуально с помощью Xamarin пользовательского интерфейса конструктора для Android.
 -   **Ресурсы и Drawable** — изображения (значки) и настраиваемой кнопки.
 -   **Экраны** — подклассов действия, которые определяют каждого экрана и его поведение. Связывает пользовательский Интерфейс с классами уровня приложения и общий API (`TaskItemManager`).


 <a name="Home_Screen" />


### <a name="home-screen"></a>Начальный экран

Начального экрана состоит из подкласса действия `HomeScreen` и `HomeScreen.axml` файл, который определяет макет (положение кнопки и список задач). Этот экран выглядит следующим образом:

 [ ![](case-study-tasky-images/android-taskylist.png "Этот экран выглядит следующим образом")](case-study-tasky-images/android-taskylist.png)

Код начального экрана определяет обработчики для кнопки и при выборе элементов в списке, а также заполнение списка в `OnResume` метод (так что он отражает изменения, внесенные в экран сведений задачи). Данные загружаются с помощью бизнес-слое `TaskItemManager` и `TaskListAdapter` из уровня приложения.

 <a name="Task_Details_Screen" />


### <a name="task-details-screen"></a>Экран сведений задач

Экран сведений задач также включает `Activity` подкласс и файл AXML макета. Макет определяет расположение элемента управления вводом и определяет поведение для загрузки и сохранения, класс C# `TaskItem` объектов.

 [ ![](case-study-tasky-images/android-taskydetail.png "Этот класс определяет поведение для загрузки и сохранения объектов TaskItem")](case-study-tasky-images/android-taskydetail.png)

Все ссылки на библиотеки PCL выполняются через `TaskItemManager` класса.

 <a name="Windows_Phone_App" />


# <a name="windows-phone-app"></a>Приложения Windows Phone
Весь проект Windows Phone:

 ![](case-study-tasky-images/taskywp7-solution.png "Приложения Windows Phone весь проект Windows Phone")

На следующей схеме представлены классы, сгруппированные в слои:

 [ ![](case-study-tasky-images/classdiagram-wp7.png "Эта диаграмма представляет классы, сгруппированные в слои")](case-study-tasky-images/classdiagram-wp7.png)

 <a name="References" />


## <a name="references"></a>Ссылки

Проекта под конкретную платформу должен ссылаться на необходимые библиотеки для конкретных платформ (таких как `Microsoft.Phone` и `System.Windows`) для создания является приложением Windows Phone.

Она также должна ссылаться в проект PCL (например) `TaskyPortableLibrary`) для использования `TaskItem` класса и базы данных.

 ![](case-study-tasky-images/taskywp7-references.png "TaskyPortableLibrary на использование TaskItem класса и базы данных")

 <a name="Application_Layer_(AL)" />


## <a name="application-layer-al"></a>Уровень приложения (п.)

Опять же как и в iOS и Android версии, на уровне приложения состоит из невизуальных элементы, которые помогают для привязки данных к пользовательскому интерфейсу.

 <a name="ViewModels" />


### <a name="viewmodels"></a>ViewModels

ViewModels переносить данные из PCL ( `TaskItemManager`) и предоставляет их способом, который может использоваться привязка данных Silverlight/XAML. Это пример поведения специфический для платформы (как описано в документе кросс-платформенных приложений).

 <a name="User_Interface_(UI)" />


## <a name="user-interface-ui"></a>Пользовательский интерфейс (UI)

XAML имеет уникальную возможность привязки данных, который может быть объявлен в разметке и сократить объем кода, необходимого для отображения объектов.

1.   **Страницы** — XAML-файлов и их вспомогательных определения пользовательского интерфейса и ссылки на ViewModels и проект переносимой библиотеки Классов для отображения и сбора данных.
2.   **Образы** — изображения заставки экрана, фона и значок — это основной элемент пользовательского интерфейса.


 <a name="MainPage" />


### <a name="mainpage"></a>MainPage

Использует класс MainPage `TaskListViewModel` для отображения данных с помощью возможностей привязки данных в XAML. Страницы `DataContext` задать модель представления, которая заполняется асинхронно. `{Binding}` Синтаксиса в XAML определяет способ отображения поля данных.

 <a name="TaskDetailsPage" />


### <a name="taskdetailspage"></a>TaskDetailsPage

Каждая задача отображается путем привязки `TaskViewModel` в XAML, определенных в TaskDetailsPage.xaml. Задача данные извлекаются через `TaskItemManager` бизнес-слое.

 <a name="Results" />


# <a name="results"></a>Результаты

В результате приложения выглядеть следующим образом для каждой платформы.

 <a name="iOS" />


### <a name="ios"></a>iOS

В приложении используется стандартное операций ввода-вывода пользовательского интерфейса, таких как кнопка «Добавить» расположенный на панели навигации и встроенного **плюс (+)** значок. Она также использует значение по умолчанию `UINavigationController` «назад» и нажмите кнопку поведение поддерживает 'проведите delete' в таблице.

 [ ![](case-study-tasky-images/ios-taskylist.png "Он также использует UINavigationController кнопки "Назад" по умолчанию и поддерживает проведите для удаления в таблице") ](case-study-tasky-images/ios-taskylist.png) [ ![ ] (case-study-tasky-images/ios-taskydetail.png "она также использует значение по умолчанию UINavigationController резервное поведение кнопки и поддерживает проведите для удаления в таблице")](case-study-tasky-images/ios-taskydetail.png)

 <a name="Android" />


### <a name="android"></a>Android

Приложение Android использует встроенные элементы управления, включая встроенные макет для строк, которые требуют отображается «такт». Помимо кнопки на экране возврата поддерживается назад работы оборудования или системы.

 [ ![](case-study-tasky-images/android-taskylist.png "Помимо кнопки на экране возврата поддерживается назад работы оборудования или системы") ](case-study-tasky-images/android-taskylist.png) [ ![ ] (case-study-tasky-images/android-taskydetail.png "назад работы оборудования или системы поддерживается в дополнение к инструкциям на экране Кнопка "Назад"")](case-study-tasky-images/android-taskydetail.png)

 <a name="Windows_Phone" />


### <a name="windows-phone"></a>Windows Phone

Приложения Windows Phone использует стандартный макет, заполнение панели приложения в нижней части экрана, а не панели навигации в верхней.

 [ ![](case-study-tasky-images/wp-taskylist.png "Приложения Windows Phone использует стандартный макет, заполнение панели приложения в нижней части экрана, а не панели навигации в верхней") ](case-study-tasky-images/wp-taskylist.png) [ ![ ] (case-study-tasky-images/wp-taskydetail.png "приложения Windows Phone использует стандарт макет, заполнение панели приложения в нижней части экрана, а не панели навигации в верхней")](case-study-tasky-images/wp-taskydetail.png)

 <a name="Summary" />


# <a name="summary"></a>Сводка

Этот документ предоставляет подробное описание применения принципов разработки многоуровневого приложения для простого приложения для облегчения повторного использования кода три мобильных платформ: iOS, Android и Windows Phone.

Описывается процесс, используемый для создания уровней приложения и рассматриваются какой код &amp; функциональность реализована в каждом слое.

Код можно загрузить из [github](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable).

## <a name="related-links"></a>Связанные ссылки

- [Создание кросс-платформенных приложений (основного документа)](~/cross-platform/app-fundamentals/building-cross-platform-applications/index.md)
- [Tasky переносимой пример приложения (github)](https://github.com/xamarin/mobile-samples/tree/master/TaskyPortable)