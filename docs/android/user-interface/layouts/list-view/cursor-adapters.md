---
title: Использование CursorAdapters
ms.prod: xamarin
ms.assetid: 60DE467E-A5DA-4420-52E5-D86AD1678FE6
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 10/25/2017
ms.openlocfilehash: a2af40d7af5cac39cc92c8e2bc88b5fbcf81291e
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68510048"
---
# <a name="using-cursoradapters-with-xamarinandroid"></a>Использование Курсорадаптерс с Xamarin. Android

Android предоставляет классы адаптеров, специально предназначенные для вывода данных из запроса к базе данных SQLite:

 **Симплекурсорадаптер** — аналогичен, `ArrayAdapter` так как он может использоваться без подклассов. Просто укажите необходимые параметры (например, сведения о курсоре и макете) в конструкторе, а затем присвойте `ListView`значение.

 **Курсорадаптер** — базовый класс, который можно унаследовать, если требуется больший контроль над привязкой значений данных к элементам управления макета (например, скрытие или отображение элементов управления или изменение их свойств).

Адаптеры курсоров обеспечивают высокопроизводительный способ прокрутки длинных списков данных, хранящихся в SQLite. В коде, использующем код, необходимо определить SQL- `Cursor` запрос в объекте, а затем описать, как создать и заполнить представления для каждой строки.


## <a name="creating-an-sqlite-database"></a>Создание базы данных SQLite

Для демонстрации использования адаптеров курсоров требуется простая реализация SQLite базы данных. Код в **симплекурсортаблеадаптер/вежетабледатабасе. CS** содержит код и SQL для создания таблицы и заполнения ее данными.
Полный `VegetableDatabase` класс показан здесь:

```csharp
class VegetableDatabase  : SQLiteOpenHelper {
   public static readonly string create_table_sql =
       "CREATE TABLE [vegetables] ([_id] INTEGER PRIMARY KEY AUTOINCREMENT NOT NULL UNIQUE, [name] TEXT NOT NULL UNIQUE)";
   public static readonly string DatabaseName = "vegetables.db";
   public static readonly int DatabaseVersion = 1;
   public VegetableDatabase(Context context) : base(context, DatabaseName, null, DatabaseVersion) { }
   public override void OnCreate(SQLiteDatabase db)
   {
       db.ExecSQL(create_table_sql);
       // seed with data
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Vegetables')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Fruits')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Flower Buds')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Legumes')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Bulbs')");
       db.ExecSQL("INSERT INTO vegetables (name) VALUES ('Tubers')");
   }
   public override void OnUpgrade(SQLiteDatabase db, int oldVersion, int newVersion)
   {   // not required until second version :)
       throw new NotImplementedException();
   }
}
```

Экземпляр класса будет создан `OnCreate` в методе `HomeScreen` действия. `VegetableDatabase` Базовый класс управляет настройкой файла базы данных и гарантирует, что SQL в своем `OnCreate` методе выполняется только один раз. `SQLiteOpenHelper` Этот класс используется в следующих двух примерах для `SimpleCursorAdapter` и. `CursorAdapter`

Для работы запрос курсора должен иметь целочисленный столбец `CursorAdapter` .  `_id` Если в базовой таблице нет целочисленного столбца с именем `_id` , используйте псевдоним столбца для другого уникального целого числа в элементе `RawQuery` , составляющем курсор. Дополнительные сведения см. в документации по [Android](xref:Android.Widget.CursorAdapter) .


### <a name="creating-the-cursor"></a>Создание курсора

В примерах используется `RawQuery` для преобразования SQL-запроса `Cursor` в объект. Список столбцов, возвращаемый курсором, определяет столбцы данных, доступные для вывода в адаптере курсора. Код, создающий базу данных в методе **симплекурсортаблеадаптер/хомескрин. CS** `OnCreate` , показан здесь:

```csharp
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null); // cursor query
StartManagingCursor(cursor);
// use either SimpleCursorAdapter or CursorAdapter subclass here!
```

Любой код, который `StartManagingCursor` вызывает, должен `StopManagingCursor`также вызывать. В примерах `OnCreate` используются для запуска и `OnDestroy` для закрытия курсора. `OnDestroy` Метод содержит следующий код:

```csharp
StopManagingCursor(cursor);
cursor.Close();
```

После того как приложение получило доступ к базе данных SQLite и создала объект Cursor, как показано ниже, он `SimpleCursorAdapter` может использовать либо `CusorAdapter` подкласс, либо подкласс `ListView`для отображения строк в.


## <a name="using-simplecursoradapter"></a>Использование Симплекурсорадаптер

`SimpleCursorAdapter`аналогичен `ArrayAdapter`, но специализированным для использования с SQLite. Для этого не требуется подкласс — просто задайте некоторые простые параметры при создании объекта, а затем присвойте его `ListView` `Adapter` свойству.

Параметры для конструктора Симплекурсорадаптер:

 **Context** — ссылка на содержащее действие.

 **Layout** — идентификатор ресурса для используемого представления строк.

 **Икурсор** — курсор, содержащий запрос SQLite для отображаемых данных.

 **Из** массива строк — массив строк, соответствующий именам столбцов в курсоре.

 **В** массив целых чисел — массив идентификаторов макета, соответствующих элементам управления в макете строки. Значение столбца, указанного в `from` массиве, будет привязано к ControlID, указанному в этом массиве в том же индексе.

Массивы `to` и должны иметь одинаковое число записей, поскольку они формируют сопоставление источника данных с элементами управления макета в представлении. `from`

Пример кода **симплекурсортаблеадаптер/хомескрин. CS** подсоединяется `SimpleCursorAdapter` следующим образом:

```csharp
// which columns map to which layout controls
string[] fromColumns = new string[] {"name"};
int[] toControlIDs = new int[] {Android.Resource.Id.Text1};
// use a SimpleCursorAdapter
listView.Adapter = new SimpleCursorAdapter (this, Android.Resource.Layout.SimpleListItem1, cursor,
       fromColumns,
       toControlIDs);
```

`SimpleCursorAdapter`— Это быстрый и простой способ вывода SQLite- `ListView`данных в. Основное ограничение заключается в том, что он может привязывать значения столбцов только к элементам управления для отображения, но не позволяет изменять другие аспекты макета строки (например, отображение или скрытие элементов управления или изменение свойств).


## <a name="subclassing-cursoradapter"></a>Подклассировать Курсорадаптер

Подкласс имеет те же преимущества производительности, что и `SimpleCursorAdapter` для отображения данных из SQLite, но он также обеспечивает полный контроль над созданием и структурой каждого представления строк. `CursorAdapter` `GetItemId` `GetView` `this[]` `Count` Реализация сильно отличается от `BaseAdapter` подкласса, так как не переопределяет, или индексатор. `CursorAdapter`

При наличии рабочей базы данных SQLite необходимо переопределить два метода для создания `CursorAdapter` подкласса:

- **Биндвиев** — заданное представление, обновите его, чтобы отобразить данные в указанном курсоре.

- **Неввиев** — вызывается, `ListView` когда для отображения требуется новое представление. Будет `CursorAdapter` относиться к повторному выполнению представлений ( `GetView` в отличие от метода для обычных адаптеров).

Подклассы адаптера в предыдущих примерах имеют методы для возвращения количества строк и получения текущего элемента — `CursorAdapter` не требуются эти методы, так как эти сведения могут быть получены из самого курсора. Разделение создания и заполнения каждого представления на эти два метода `CursorAdapter` обеспечивает повторное использование представления. Это отличается от обычного адаптера, в котором можно игнорировать `convertView` параметр `BaseAdapter.GetView` метода.


### <a name="implementing-the-cursoradapter"></a>Реализация Курсорадаптер

Код в **курсортаблеадаптер/хомескринкурсорадаптер. CS** содержит `CursorAdapter` подкласс. Он сохраняет ссылку контекста, переданную в конструктор, чтобы он мог получить доступ `LayoutInflater` `NewView` к в методе. Полный класс выглядит следующим образом:

```csharp
public class HomeScreenCursorAdapter : CursorAdapter {
   Activity context;
   public HomeScreenCursorAdapter(Activity context, ICursor c)
       : base(context, c)
   {
       this.context = context;
   }
   public override void BindView(View view, Context context, ICursor cursor)
   {
       var textView = view.FindViewById<TextView>(Android.Resource.Id.Text1);
       textView.Text = cursor.GetString(1); // 'name' is column 1 in the cursor query
   }
   public override View NewView(Context context, ICursor cursor, ViewGroup parent)
   {
       return this.context.LayoutInflater.Inflate(Android.Resource.Layout.SimpleListItem1, parent, false);
   }
}
```


### <a name="assigning-the-cursoradapter"></a>Назначение Курсорадаптер

В окне `Activity` , в котором будет `ListView`отображаться, создать курсор и `CursorAdapter` затем назначить его представлению списка.

Код, выполняющий это действие в методе **курсортаблеадаптер/хомескрин. CS** `OnCreate` , показан здесь:

```csharp
// create the cursor
vdb = new VegetableDatabase(this);
cursor = vdb.ReadableDatabase.RawQuery("SELECT * FROM vegetables", null);
StartManagingCursor(cursor);

// create the CursorAdapter
listView.Adapter = (IListAdapter)new HomeScreenCursorAdapter(this, cursor, false);
```

Метод содержит вызов метода `StopManagingCursor` , описанный выше. `OnDestroy`



## <a name="related-links"></a>Связанные ссылки

- [Симплекурсортаблеадаптер (пример)](https://developer.xamarin.com/samples/monodroid/SimpleCursorTableAdapter/)
- [Курсортаблеадаптер (пример)](https://developer.xamarin.com/samples/monodroid/CursorTableAdapter/)
