---
title: Запись Xamarin.Forms
description: В этой статье описываются способы использования класса входа Xamarin.Forms для принятия однострочный текст или пароля, вводимого в приложении.
ms.prod: xamarin
ms.assetid: 9923C541-3C10-4D14-BAB5-C4D6C514FB1E
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2018
ms.openlocfilehash: 040d01dceaa6c5dd785d2423c72247ee602b98db
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70767707"
---
# <a name="xamarinforms-entry"></a>Запись Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)

_Однострочный текст или пароля_

Xamarin.Forms [ `Entry` ](xref:Xamarin.Forms.Entry) используется для ввода текста на одну строку. `Entry`, Например [ `Editor` ](xref:Xamarin.Forms.Editor) Просмотр, поддерживает несколько типов клавиатуры. Кроме того `Entry` можно использовать в качестве поля пароля.

## <a name="display-customization"></a>Настройки отображения

### <a name="setting-and-reading-text"></a>Установка и чтение текста

`Entry`, Как и с другими представлениями представления текста предоставляет [ `Text` ](xref:Xamarin.Forms.Entry.Text) свойство. Это свойство может использоваться для задания и считывать текст, представленный `Entry`. В следующем примере показано задание `Text` свойства в XAML:

```xaml
<Entry Text="I am an Entry" />
```

В C#:

```csharp
var MyEntry = new Entry { Text = "I am an Entry" };
```

Чтобы прочитать текст, доступ к `Text` свойства в C#:

```csharp
var text = MyEntry.Text;
```

### <a name="setting-placeholder-text"></a>Параметр замещающий текст

[ `Entry` ](xref:Xamarin.Forms.Entry) Можно задать, чтобы показать текст заполнителя, если он не хранит ввод данных пользователем. Это достигается путем установки [ `Placeholder` ](xref:Xamarin.Forms.Entry.Placeholder) свойства `string`и часто используется для указания типа содержимого, которое подходит для `Entry`. Кроме того, цвет текста заполнителя можно управлять, задав [ `PlaceholderColor` ](xref:Xamarin.Forms.Entry.PlaceholderColor) свойства [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Entry Placeholder="Username" PlaceholderColor="Olive" />
```

```csharp
var entry = new Entry { Placeholder = "Username", PlaceholderColor = Color.Olive };
```

> [!NOTE]
> Ширина `Entry` может быть определена путем задания его `WidthRequest` свойство. Не зависят от ширину `Entry` , определенное на основе значения из его `Text` свойство.

### <a name="preventing-text-entry"></a>Предотвращение ввода текста

Пользователям может быть запрещено изменять текст [`Entry`](xref:Xamarin.Forms.Entry) в, `IsReadOnly` задавая свойство, которое по умолчанию `false`имеет значение, равным `true`:

```xaml
<Entry Text="This is a read-only Entry"
       IsReadOnly="true" />
```

```csharp
var entry = new Entry { Text = "This is a read-only Entry", IsReadOnly = true });
```

> [!NOTE]
> Свойство не изменяет внешний вид [`Entry`](xref:Xamarin.Forms.Entry), в отличие от `IsEnabled` свойства `Entry` , которое также изменяет внешний вид изображения на серый. `IsReadonly`

### <a name="limiting-input-length"></a>Ограничение длины входных

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Свойство может использоваться для ограничения длины входных данных, допустимое для [ `Entry` ](xref:Xamarin.Forms.Entry). Это свойство должно быть присвоено положительное целое число:

```xaml
<Entry ... MaxLength="10" />
```

```csharp
var entry = new Entry { ... MaxLength = 10 };
```

Объект [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) свойства значение 0 указывает, что входные данные не будет разрешено и значение `int.MaxValue`, который является значением по умолчанию для [ `Entry` ](xref:Xamarin.Forms.Entry), указывает, что не действующие ограничения на число символов, которые могут быть введены.

### <a name="password-fields"></a>Поля пароля

`Entry` предоставляет `IsPassword` свойство. Когда `IsPassword` является `true`, содержимое поля будут представлены в виде кругов черным:

В XAML:

```xaml
<Entry IsPassword="true" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true };
```

![Пример ввода пароля](entry-images/password.png)

Заполнители, которые могут использоваться с экземплярами `Entry` , настроенные в качестве пароля:

В XAML:

```xaml
<Entry IsPassword="true" Placeholder="Password" />
```

В C#:

```csharp
var MyEntry = new Entry { IsPassword = true, Placeholder = "Password" };
```

![Пример ввода пароля и заполнителя](entry-images/passwordplaceholder.png)

### <a name="setting-the-cursor-position-and-text-selection-length"></a>Задание позиции курсора и длину выделения текста

[ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition) Свойство может использоваться для возврата или задания позиции, по которому будет вставить следующий символ в строки, хранящейся в [ `Text` ](xref:Xamarin.Forms.Entry.Text) свойство:

```xaml
<Entry Text="Cursor position set" CursorPosition="5" />
```

```csharp
var entry = new Entry { Text = "Cursor position set", CursorPosition = 5 };
```

Значение по умолчанию [ `CursorPosition` ](xref:Xamarin.Forms.Entry.CursorPosition) свойство является 0, означающее, что текст будет вставлен в начало файла `Entry`.

Кроме того [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength) свойство может использоваться для возврата или задать длину выделенного текста в пределах `Entry`:

```xaml
<Entry Text="Cursor position and selection length set" CursorPosition="2" SelectionLength="10" />
```

```csharp
var entry = new Entry { Text = "Cursor position and selection length set", CursorPosition = 2, SelectionLength = 10 };
```

Значение по умолчанию [ `SelectionLength` ](xref:Xamarin.Forms.Entry.SelectionLength) свойство является 0, означающее, что текст не выбран.

### <a name="customizing-the-keyboard"></a>Настройка клавиатуры

Клавиатуры, выводится тогда, когда пользователи взаимодействуют с [ `Entry` ](xref:Xamarin.Forms.Entry) могут быть заданы программно с помощью [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) свойства, чтобы одно из следующих свойств из [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) класса:

- [`Chat`](xref:Xamarin.Forms.Keyboard.Chat) — используется для сообщения и мест, где полезны эмодзи.
- [`Default`](xref:Xamarin.Forms.Keyboard.Default) — это клавиатура по умолчанию.
- [`Email`](xref:Xamarin.Forms.Keyboard.Email) используется для ввода адресов электронной почты.
- [`Numeric`](xref:Xamarin.Forms.Keyboard.Numeric) — цифровая клавиатура.
- [`Plain`](xref:Xamarin.Forms.Keyboard.Plain) предназначена для ввода текста, если нет заданных [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags).
- [`Telephone`](xref:Xamarin.Forms.Keyboard.Telephone) — клавиатура для ввода телефонных номеров.
- [`Text`](xref:Xamarin.Forms.Keyboard.Text) — текстовая клавиатура.
- [`Url`](xref:Xamarin.Forms.Keyboard.Url) — клавиатура для ввода путей к файлам и веб-адресов.

Это можно сделать в XAML следующим образом:

```xaml
<Entry Keyboard="Chat" />
```

Ниже приведен аналогичный код C#:

```csharp
var entry = new Entry { Keyboard = Keyboard.Chat };
```

Примеры каждого клавиатуры можно найти в нашей [рецепты](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/Controls/choose-keyboard-for-entry) репозитория.

[ `Keyboard` ](xref:Xamarin.Forms.Keyboard) Класс также имеет [ `Create` ](xref:Xamarin.Forms.Keyboard.Create*) фабричный метод, который может использоваться для настройки клавиатуры, указав режим регистр букв, проверка орфографии и предложения. Значения перечисления [`KeyboardFlags`](xref:Xamarin.Forms.KeyboardFlags) задаются как аргументы метода, при этом возвращается настроенное свойство `Keyboard`. Перечисление `KeyboardFlags` имеет такие значения:

- [`None`](xref:Xamarin.Forms.KeyboardFlags.None) указывает, что клавиатура не имеет никаких дополнительных функций.
- [`CapitalizeSentence`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeSentence) указывает, что первые слова во всех вводимых предложениях автоматически начинаются с прописных букв.
- [`Spellcheck`](xref:Xamarin.Forms.KeyboardFlags.Spellcheck) указывает, что для вводимого текста выполняется проверка орфографии.
- [`Suggestions`](xref:Xamarin.Forms.KeyboardFlags.Suggestions) указывает, что для вводимых слов предлагается завершение.
- [`CapitalizeWord`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeWord) указывает, что все слова автоматически начинаются с прописных букв.
- [`CapitalizeCharacter`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeCharacter) указывает, что все символы автоматически пишутся прописными буквами.
- [`CapitalizeNone`](xref:Xamarin.Forms.KeyboardFlags.CapitalizeNone) указывает, что автоматическая подстановка прописных букв не выполняется.
- [`All`](xref:Xamarin.Forms.KeyboardFlags.All) указывает, что для вводимого текста выполняется проверка орфографии, завершение слов и автоматическое написание предложений с прописной буквы.

В следующем примере кода XAML показано, как настроить значение по умолчанию [`Keyboard`](xref:Xamarin.Forms.Keyboard), чтобы включить предложение завершения слов и написание всех символов прописными буквами:

```xaml
<Entry Placeholder="Enter text here">
    <Entry.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Entry.Keyboard>
</Entry>
```

Ниже приведен аналогичный код C#:

```csharp
var entry = new Entry { Placeholder = "Enter text here" };
entry.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

#### <a name="customizing-the-return-key"></a>Настройка клавиша Return

Внешний вид возвращаемого значения ключа на Экранная клавиатура, который является отображается, когда [ `Entry` ](xref:Xamarin.Forms.Entry) имеет фокус, можно настроить, задав [ `ReturnType` ](xref:Xamarin.Forms.Entry.ReturnType) свойства со значением [ `ReturnType` ](xref:Xamarin.Forms.ReturnType) перечисления:

- [`Default`](xref:Xamarin.Forms.ReturnType.Default) — Указывает, что нет определенного ключа возвращаемого значения не требуется и что будет использоваться значение по умолчанию платформы.
- [`Done`](xref:Xamarin.Forms.ReturnType.Done) — Указывает, «Готово» клавиша return.
- [`Go`](xref:Xamarin.Forms.ReturnType.Go) — Указывает ключ return «Далее».
- [`Next`](xref:Xamarin.Forms.ReturnType.Next) — Указывает ключ return «Далее».
- [`Search`](xref:Xamarin.Forms.ReturnType.Search) — Указывает ключ возвращаемого значения «Поиск».
- [`Send`](xref:Xamarin.Forms.ReturnType.Send) — Указывает ключ возвращаемого значения «Отправить».

В следующем примере XAML показано, как задать возвращаемое ключ:

```xaml
<Entry ReturnType="Send" />
```

Ниже приведен аналогичный код C#:

```csharp
var entry = new Entry { ReturnType = ReturnType.Send };
```

> [!NOTE]
> Клавиша return точный вид зависит от платформы. В iOS клавиша return — это кнопка, основанные на тексте. Тем не менее Android и универсальной платформы Windows, клавиша return находится кнопка значки.

При нажатии клавиши ВВОД, [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) вызывает событие и любые `ICommand` определяется [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) свойство выполняется. Кроме того любой `object` определяется [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) свойства будут передаваться `ICommand` как параметр. Дополнительные сведения о командах см. в разделе [интерфейс команда](~/xamarin-forms/app-fundamentals/data-binding/commanding.md).

### <a name="enabling-and-disabling-spell-checking"></a>Включение и отключение проверка орфографии

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Свойство, управляет ли проверка орфографии включена. По умолчанию задано значение `true`. Как пользователь вводит текст, заданный опечаток.

Тем не менее для некоторых сценариев входа текст, таких, как ввод имени пользователя, проверка орфографии предоставляет негативной и должен быть отключен, задав [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойства `false`:

```xaml
<Entry ... IsSpellCheckEnabled="false" />
```

```csharp
var entry = new Entry { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Когда [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойству `false`и пользовательских сочетаний не используется, средство проверки орфографии собственного будет отключена. Тем не менее если [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) имеет был набор, который отключает орфографии поиска, например [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` свойство учитывается. Таким образом, свойство не позволяет включить проверку орфографии для `Keyboard` , явно отключает его.

### <a name="enabling-and-disabling-text-prediction"></a>Включение и отключение прогнозирования текста

[ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) Свойство, управляет ли прогнозирование текста и автоматическое исправление текста включен. По умолчанию задано значение `true`. Как пользователь вводит текст, представляется word прогнозов.

Тем не менее, для некоторых сценариев входа текст, например ввода имени пользователя, прогнозирование текста и текста исправление обеспечивает взаимодействие с отрицательным и следует отключить, задав [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) свойства `false`:

```xaml
<Entry ... IsTextPredictionEnabled="false" />
```

```csharp
var entry = new Entry { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> При [ `IsTextPredictionEnabled` ](xref:Xamarin.Forms.Entry.IsTextPredictionEnabled) свойству `false`, и пользовательских сочетаний не используется, прогнозирование текста и автоматическое исправление текста отключено. Тем не менее если [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) было задано, прогнозирование текста отключает, `IsTextPredictionEnabled` свойство учитывается. Таким образом, свойство не может использоваться для прогнозирования текста для `Keyboard` , явно отключает его.

### <a name="colors"></a>Цвета

Использовать настраиваемый фон и цвета текста с помощью следующих привязываемые свойства можно задать запись.

- **TextColor** &ndash; задает цвет текста.
- **BackgroundColor** &ndash; задает цвет фона для текста показано.

Особое внимание необходимо, чтобы гарантировать, что цвета будет доступна для использования на каждой платформе. Так как каждая платформа имеет различные значения по умолчанию для цветов текста и фона, часто необходимо задать оба, если задается один.

Чтобы задать цвет текста записи, используйте следующий код:

В XAML:

```xaml
<Entry TextColor="Green" />
```

В C#:

```csharp
var entry = new Entry();
entry.TextColor = Color.Green;
```

![Пример записи TextColor](entry-images/textcolor.png)

Обратите внимание, что заполнитель не влияет на заданный `TextColor`.

Чтобы задать цвет фона в XAML:

```xaml
<Entry BackgroundColor="#2c3e50" />
```

В C#:

```csharp
var entry = new Entry();
entry.BackgroundColor = Color.FromHex("#2c3e50");
```

![Пример записи BackgroundColor](entry-images/textbackgroundcolor.png)

Следите за тем, чтобы убедиться в том, что цвета фона и текста, которые вы выбрали могут использоваться на каждой платформе и не закрывают любой текст заполнителя.

## <a name="events-and-interactivity"></a>События и интерактивность

Запись предоставляет два события:

- [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged) &ndash; возникает при изменении текста в записи. Содержит текст до и после изменения.
- [`Completed`](xref:Xamarin.Forms.Entry.Completed) &ndash; вызывается, когда пользователь завершен входные данные с помощью возвращаемого значения клавиши на клавиатуре.

> [!NOTE]
> Класс, от которого [`Entry`](xref:Xamarin.Forms.Entry) наследует, также имеет [`Focused`](xref:Xamarin.Forms.VisualElement.Focused) события и [`Unfocused`.](xref:Xamarin.Forms.VisualElement.Unfocused) [`VisualElement`](xref:Xamarin.Forms.VisualElement)

### <a name="completed"></a>Завершено

`Completed` Событие используется для реагирования на завершение взаимодействия с записью. `Completed`вызывается, когда пользователь заканчивает ввод с полем, нажав клавишу Return на клавиатуре (или нажав клавишу TAB в UWP). Обработчик для события представляет собой Универсальное событие обработчика, используя отправителя и `EventArgs`:

```csharp
void Entry_Completed (object sender, EventArgs e)
{
    var text = ((Entry)sender).Text; //cast sender to access the properties of the Entry
}
```

Завершенное событие можно подписаться в XAML:

```xaml
<Entry Completed="Entry_Completed" />
```

и C#:

```csharp
var entry = new Entry ();
entry.Completed += Entry_Completed;
```

После [ `Completed` ](xref:Xamarin.Forms.Entry.Completed) любое событие запускается `ICommand` определяется [ `ReturnCommand` ](xref:Xamarin.Forms.Entry.ReturnCommand) выполняется свойство, с помощью `object` определяется [ `ReturnCommandParameter` ](xref:Xamarin.Forms.Entry.ReturnCommandParameter) свойства, передаваемые `ICommand`.

### <a name="textchanged"></a>TextChanged

`TextChanged` Событие используется для реагирования на изменения в содержимое поля.

`TextChanged` возникает каждый раз, когда `Text` из `Entry` изменения. Обработчик для события принимает экземпляр `TextChangedEventArgs`. `TextChangedEventArgs` предоставляет доступ к старое и новое значения `Entry` `Text` через `OldTextValue` и `NewTextValue` свойства:

```csharp
void Entry_TextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

`TextChanged` Событие можно подписаться в XAML:

```xaml
<Entry TextChanged="Entry_TextChanged" />
```

и C#:

```csharp
var entry = new Entry ();
entry.TextChanged += Entry_TextChanged;
```

## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-text)
- [Операция API](xref:Xamarin.Forms.Entry)
