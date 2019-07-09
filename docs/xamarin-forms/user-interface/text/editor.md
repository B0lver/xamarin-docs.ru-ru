---
title: Редактор Xamarin.Forms
description: В этой статье объясняется, как использовать элемент управления редактора Xamarin.Forms принимать ввод многострочного текста в приложении.
ms.prod: xamarin
ms.assetid: 7074DB3A-30D2-4A6B-9A89-B029EEF20B07
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/26/2018
ms.openlocfilehash: 97bb5ec954f36e48d8ae115baf8738862e5a8358
ms.sourcegitcommit: c1d85b2c62ad84c22bdee37874ad30128581bca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/08/2019
ms.locfileid: "67649544"
---
# <a name="xamarinforms-editor"></a>Редактор Xamarin.Forms

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)

_Ввод многострочного текста_

[ `Editor` ](xref:Xamarin.Forms.Editor) Элемент управления используется для ввода нескольких линий. В этой статье рассматриваются следующие действия:

- **[Настройки](#customization)**  &ndash; параметры клавиатуры и цвета.
- **[Интерактивные возможности](#interactivity)**  &ndash; события, которые можно прослушивать для обеспечения взаимодействия.

## <a name="customization"></a>Настройка

### <a name="setting-and-reading-text"></a>Установка и чтение текста

[ `Editor` ](xref:Xamarin.Forms.Editor), Как и с другими представлениями представления текста предоставляет `Text` свойство. Это свойство может использоваться для задания и считывать текст, представленный `Editor`. В следующем примере показано задание `Text` свойства в XAML:

```xaml
<Editor Text="I am an Editor" />
```

В C#:

```csharp
var MyEditor = new Editor { Text = "I am an Editor" };
```

Чтобы прочитать текст, доступ к `Text` свойства в C#:

```csharp
var text = MyEditor.Text;
```

### <a name="setting-placeholder-text"></a>Параметр замещающий текст

[ `Editor` ](xref:Xamarin.Forms.Editor) Можно задать, чтобы показать текст заполнителя, если он не хранит ввод данных пользователем. Это достигается путем установки [ `Placeholder` ](xref:Xamarin.Forms.Editor.Placeholder) свойства `string`и часто используется для указания типа содержимого, которое подходит для `Editor`. Кроме того, цвет текста заполнителя можно управлять, задав [ `PlaceholderColor` ](xref:Xamarin.Forms.Editor.PlaceholderColor) свойства [ `Color` ](xref:Xamarin.Forms.Color):

```xaml
<Editor Placeholder="Enter text here" PlaceholderColor="Olive" />
```

```csharp
var editor = new Editor { Placeholder = "Enter text here", PlaceholderColor = Color.Olive };
```

### <a name="preventing-text-entry"></a>Предотвращение ввод текста

Пользователи могут быть защищены от изменения текста в [ `Editor` ](xref:Xamarin.Forms.Editor) , задав `IsReadOnly` свойство, которое имеет значение по умолчанию из `false`, `true`:

```xaml
<Editor Text="This is a read-only Editor"
        IsReadOnly="true" />
```

```csharp
var editor = new Editor { Text = "This is a read-only Editor", IsReadOnly = true });
```

> [!NOTE]
> `IsReadonly` Свойство не приводит к изменению внешнего вида [ `Editor` ](xref:Xamarin.Forms.Editor), в отличие от `IsEnabled` свойство, которое также изменяет внешний вид `Editor` к серому.

### <a name="limiting-input-length"></a>Ограничение длины входных

[ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) Свойство может использоваться для ограничения длины входных данных, допустимое для [ `Editor` ](xref:Xamarin.Forms.Editor). Это свойство должно быть присвоено положительное целое число:

```xaml
<Editor ... MaxLength="10" />
```

```csharp
var editor = new Editor { ... MaxLength = 10 };
```

Объект [ `MaxLength` ](xref:Xamarin.Forms.InputView.MaxLength) свойства значение 0 указывает, что входные данные не будет разрешено и значение `int.MaxValue`, который является значением по умолчанию для [ `Editor` ](xref:Xamarin.Forms.Editor), указывает, что не действующие ограничения на число символов, которые могут быть введены.

### <a name="auto-sizing-an-editor"></a>Автоматического изменения размера редактор

[ `Editor` ](xref:Xamarin.Forms.Editor) Можно делать автоматическое масштабирование на его содержимое, устанавливая [ `Editor.AutoSize` ](xref:Xamarin.Forms.Editor.AutoSize) свойства [ `TextChanges` ](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges), который представляет собой значение [ `EditoAutoSizeOption` ](xref:Xamarin.Forms.EditorAutoSizeOption) перечисления. Это перечисление имеет два значения.

- [`Disabled`](xref:Xamarin.Forms.EditorAutoSizeOption.Disabled) Указывает, что автоматическое изменение размеров отключено и значение по умолчанию.
- [`TextChanges`](xref:Xamarin.Forms.EditorAutoSizeOption.TextChanges) Указывает, что включено автоматическое изменение размеров.

Это можно сделать в коде следующим образом:

```xaml
<Editor Text="Enter text here" AutoSize="TextChanges" />
```

```csharp
var editor = new Editor { Text = "Enter text here", AutoSize = EditorAutoSizeOption.TextChanges };
```

Если включено автоматическое изменение размеров, высота [ `Editor` ](xref:Xamarin.Forms.Editor) увеличивается, когда пользователь заполняет его с текстом, а высота будет уменьшаться, как пользователь удаляет текст.

> [!NOTE]
> [ `Editor` ](xref:Xamarin.Forms.Editor) Будет if не Авторазмер [ `HeightRequest` ](xref:Xamarin.Forms.VisualElement.HeightRequest) свойства.

### <a name="customizing-the-keyboard"></a>Настройка клавиатуры

Клавиатуры, выводится тогда, когда пользователи взаимодействуют с [ `Editor` ](xref:Xamarin.Forms.Editor) могут быть заданы программно с помощью [ `Keyboard` ](xref:Xamarin.Forms.InputView.Keyboard) свойства, чтобы одно из следующих свойств из [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) класса:

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
<Editor Keyboard="Chat" />
```

Ниже приведен аналогичный код C#:

```csharp
var editor = new Editor { Keyboard = Keyboard.Chat };
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
<Editor>
    <Editor.Keyboard>
        <Keyboard x:FactoryMethod="Create">
            <x:Arguments>
                <KeyboardFlags>Suggestions,CapitalizeCharacter</KeyboardFlags>
            </x:Arguments>
        </Keyboard>
    </Editor.Keyboard>
</Editor>
```

Ниже приведен аналогичный код C#:

```csharp
var editor = new Editor();
editor.Keyboard = Keyboard.Create(KeyboardFlags.Suggestions | KeyboardFlags.CapitalizeCharacter);
```

### <a name="enabling-and-disabling-spell-checking"></a>Включение и отключение проверка орфографии

[ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) Свойство, управляет ли проверка орфографии включена. По умолчанию задано значение `true`. Как пользователь вводит текст, заданный опечаток.

Однако для некоторых сценариях записи текста, например ввода имени пользователя, проверка орфографии дает негативной, поэтому следует отключить, задав [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойства `false`:

```xaml
<Editor ... IsSpellCheckEnabled="false" />
```

```csharp
var editor = new Editor { ... IsSpellCheckEnabled = false };
```

> [!NOTE]
> Когда [ `IsSpellCheckEnabled` ](xref:Xamarin.Forms.InputView.IsSpellCheckEnabled) свойству `false`и пользовательских сочетаний не используется, средство проверки орфографии собственного будет отключена. Тем не менее если [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) имеет был набор, который отключает орфографии поиска, например [ `Keyboard.Chat` ](xref:Xamarin.Forms.Keyboard.Chat), `IsSpellCheckEnabled` свойство учитывается. Таким образом, свойство не позволяет включить проверку орфографии для `Keyboard` , явно отключает его.

### <a name="enabling-and-disabling-text-prediction"></a>Включение и отключение прогнозирования текста

`IsTextPredictionEnabled` Свойство, управляет ли прогнозирование текста и автоматическое исправление текста включен. По умолчанию задано значение `true`. Как пользователь вводит текст, представляется word прогнозов.

Тем не менее, для некоторых сценариев входа текст, например ввода имени пользователя, прогнозирование текста и текста исправление обеспечивает взаимодействие с отрицательным и следует отключить, задав `IsTextPredictionEnabled` свойства `false`:

```xaml
<Editor ... IsTextPredictionEnabled="false" />
```

```csharp
var editor = new Editor { ... IsTextPredictionEnabled = false };
```

> [!NOTE]
> При `IsTextPredictionEnabled` свойству `false`, и пользовательских сочетаний не используется, прогнозирование текста и автоматическое исправление текста отключено. Тем не менее если [ `Keyboard` ](xref:Xamarin.Forms.Keyboard) было задано, прогнозирование текста отключает, `IsTextPredictionEnabled` свойство учитывается. Таким образом, свойство не может использоваться для прогнозирования текста для `Keyboard` , явно отключает его.

### <a name="colors"></a>Цвета

`Editor` можно задать, чтобы использовать пользовательский цвет фона через `BackgroundColor` свойство. Особое внимание необходимо, чтобы гарантировать, что цвета будет доступна для использования на каждой платформе. Так как каждая платформа имеет различные значения по умолчанию для цвета текста, может потребоваться задать пользовательский цвет фона для каждой платформы. См. в разделе [работа с платформы Tweaks](~/xamarin-forms/platform/device.md) Дополнительные сведения об оптимизации пользовательский Интерфейс для каждой платформы.

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        //dark blue on UWP & Android, light blue on iOS
        var editor = new Editor { BackgroundColor = Device.RuntimePlatform == Device.iOS ? Color.FromHex("#A4EAFF") : Color.FromHex("#2c3e50") };
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
    xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
    x:Class="TextSample.EditorPage"
    Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor>
                <Editor.BackgroundColor>
                    <OnPlatform x:TypeArguments="x:Color">
                        <On Platform="iOS" Value="#a4eaff" />
                        <On Platform="Android, UWP" Value="#2c3e50" />
                    </OnPlatform>
                </Editor.BackgroundColor>
            </Editor>
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

![](editor-images/textbackgroundcolor.png "Редактор с BackgroundColor пример")

Убедитесь, что цвета фона и текста, которые вы выбрали могут использоваться на каждой платформе и не закрывают любой текст заполнителя.

## <a name="interactivity"></a>Интерактивность

`Editor` предоставляет два события:

- [TextChanged](xref:Xamarin.Forms.Editor.TextChanged) &ndash; возникает при изменении текста в редакторе. Содержит текст до и после изменения.
- [Завершено](xref:Xamarin.Forms.Editor.Completed) &ndash; вызывается, когда пользователь завершен входные данные с помощью возвращаемого значения клавиши на клавиатуре.

> [!NOTE]
> [ `VisualElement` ](xref:Xamarin.Forms.VisualElement) Класс, из которого [ `Entry` ](xref:Xamarin.Forms.Entry) наследует, также имеет [ `Focused` ](xref:Xamarin.Forms.VisualElement.Focused) и [ `Unfocused` ](xref:Xamarin.Forms.VisualElement.Unfocused)события.

### <a name="completed"></a>Завершено

`Completed` Событие используется для реагирования на завершение взаимодействия с `Editor`. `Completed` вызывается, когда пользователь завершает входных данных с полем, введя клавиша return на клавиатуре (или нажав клавишу Tab на UWP). Обработчик для события представляет собой Универсальное событие обработчика, используя отправителя и `EventArgs`:

```csharp
void EditorCompleted (object sender, EventArgs e)
{
    var text = ((Editor)sender).Text; // sender is cast to an Editor to enable reading the `Text` property of the view.
}
```

Завершенное событие можно подписаться в коде и XAML:

В C#:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.Completed += EditorCompleted;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor Completed="EditorCompleted" />
        </StackLayout>
    </ContentPage.Content>
</Contentpage>
```

### <a name="textchanged"></a>TextChanged

`TextChanged` Событие используется для реагирования на изменения в содержимое поля.

`TextChanged` возникает каждый раз, когда `Text` из `Editor` изменения. Обработчик для события принимает экземпляр `TextChangedEventArgs`. `TextChangedEventArgs` предоставляет доступ к старое и новое значения `Editor` `Text` через `OldTextValue` и `NewTextValue` свойства:

```csharp
void EditorTextChanged (object sender, TextChangedEventArgs e)
{
    var oldText = e.OldTextValue;
    var newText = e.NewTextValue;
}
```

Завершенное событие можно подписаться в коде и XAML:

В коде:

```csharp
public partial class EditorPage : ContentPage
{
    public EditorPage ()
    {
        InitializeComponent ();
        var layout = new StackLayout { Padding = new Thickness(5,10) };
        this.Content = layout;
        var editor = new Editor ();
        editor.TextChanged += EditorTextChanged;
        layout.Children.Add(editor);
    }
}
```

В XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="TextSample.EditorPage"
Title="Editor Demo">
    <ContentPage.Content>
        <StackLayout Padding="5,10">
            <Editor TextChanged="EditorTextChanged" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```


## <a name="related-links"></a>Связанные ссылки

- [Текст (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Text)
- [Редактор API](xref:Xamarin.Forms.Editor)
