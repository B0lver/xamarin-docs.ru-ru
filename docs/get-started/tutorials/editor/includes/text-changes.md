---
ms.openlocfilehash: 19afeed47f1c06c89c58dfd996d360698b19fc9c
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "61373411"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. В **MainPage.xaml** измените объявление [`Editor`](xref:Xamarin.Forms.Editor) таким образом, чтобы оно установило обработчик для событий [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) и [`Completed`](xref:Xamarin.Forms.Editor.Completed).

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    Этот код устанавливает событие [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) для обработчика событий `OnEditorTextChanged`, а [`Completed`](xref:Xamarin.Forms.Editor.Completed) — для обработчика `OnEditorCompleted`. На следующем шаге создаются оба обработчика событий.

1. В **обозревателе решений** в проекте **EditorTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в файле **MainPage.xaml.cs** добавьте обработчики событий `OnEditorTextChanged` и `OnEditorCompleted` в класс.

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    Если текст в [`Editor`](xref:Xamarin.Forms.Editor) изменяется, выполняется метод `OnEditorTextChanged`. Аргумент `sender` является объектом `Editor`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Editor`. Аргумент [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) содержит старые и новые текстовые значения (до и после изменения текста).

    По завершении редактирования выполняется метод `OnEditorCompleted`. Это достигается путем расфокусировки [`Editor`](xref:Xamarin.Forms.Editor) или дополнительно нажатием кнопки "Готово" на iOS. Аргумент `sender` является объектом `Editor`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Editor`.

    > [!IMPORTANT]
    > Любой текст, введенный в [`Editor`](xref:Xamarin.Forms.Editor), хранится в свойстве [`Text`](xref:Xamarin.Forms.Editor.Text).

1. На панели инструментов Visual Studio нажмите кнопку **Запуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS удаленной работы или эмуляторе Android:

    [![Снимок экрана: редактор, содержащий текст, в iOS и Android](../images/text-changes.png "Редактор с текстом")](../images/text-changes-large.png#lightbox "Редактор с текстом")

    Установите точки останова в два обработчика событий, введите текст в [`Editor`](xref:Xamarin.Forms.Editor) и просмотрите запуск событий [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged). Расфокусируйте `Editor`, чтобы просмотреть запуск событий [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    Дополнительные сведения о событиях [`Editor`](xref:Xamarin.Forms.Editor) см. в разделе [Интерактивность](~/xamarin-forms/user-interface/text/editor.md#interactivity) в руководстве [Редактор Xamarin.Forms](~/xamarin-forms/user-interface/text/editor.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. В **MainPage.xaml** измените объявление [`Editor`](xref:Xamarin.Forms.Editor) таким образом, чтобы оно установило обработчик для событий [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) и [`Completed`](xref:Xamarin.Forms.Editor.Completed).

    ```xaml
    <Editor Placeholder="Enter multi-line text here"
            HeightRequest="200"
            TextChanged="OnEditorTextChanged"
            Completed="OnEditorCompleted" />
    ```

    Этот код устанавливает событие [`TextChanged`](xref:Xamarin.Forms.Editor.TextChanged) для обработчика событий `OnEditorTextChanged`, а [`Completed`](xref:Xamarin.Forms.Editor.Completed) — для обработчика `OnEditorCompleted`. На следующем шаге создаются оба обработчика событий.

1. На **Панели решений** в проекте **EditorTutorial** разверните узел **MainPage.xaml** и дважды щелкните файл **MainPage.xaml.cs**, чтобы открыть его. Затем в файле **MainPage.xaml.cs** добавьте обработчики событий `OnEditorTextChanged` и `OnEditorCompleted` в класс.

    ```csharp
    void OnEditorTextChanged(object sender, TextChangedEventArgs e)
    {
        string oldText = e.OldTextValue;
        string newText = e.NewTextValue;
    }

    void OnEditorCompleted(object sender, EventArgs e)
    {
        string text = ((Editor)sender).Text;
    }
    ```

    Если текст в [`Editor`](xref:Xamarin.Forms.Editor) изменяется, выполняется метод `OnEditorTextChanged`. Аргумент `sender` является объектом `Editor`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Editor`. Аргумент [`TextChangedEventArgs`](xref:Xamarin.Forms.TextChangedEventArgs) содержит старые и новые текстовые значения (до и после изменения текста).

    По завершении редактирования выполняется метод `OnEditorCompleted`. Это достигается путем расфокусировки [`Editor`](xref:Xamarin.Forms.Editor) или дополнительно нажатием кнопки "Готово" на iOS. Аргумент `sender` является объектом `Editor`, ответственным за запуск события `TextChanged`, и может использоваться для доступа к объекту `Editor`.

    > [!IMPORTANT]
    > Любой текст, введенный в [`Editor`](xref:Xamarin.Forms.Editor), хранится в свойстве [`Text`](xref:Xamarin.Forms.Editor.Text).

1. На панели инструментов Visual Studio для Mac нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) для запуска приложения в выбранном симуляторе iOS или эмуляторе Android.

    [![Снимок экрана: редактор, содержащий текст, в iOS и Android](../images/text-changes.png "Редактор с текстом")](../images/text-changes-large.png#lightbox "Редактор с текстом")

    Установите точки останова в два обработчика событий, введите текст в [`Editor`](xref:Xamarin.Forms.Editor) и просмотрите запуск событий [`TextChanged`](xref:Xamarin.Forms.Entry.TextChanged). Расфокусируйте `Editor`, чтобы просмотреть запуск событий [`Completed`](xref:Xamarin.Forms.Entry.Completed).

    Дополнительные сведения о событиях [`Editor`](xref:Xamarin.Forms.Editor) см. в разделе [Интерактивность](~/xamarin-forms/user-interface/text/editor.md#interactivity) в руководстве [Редактор Xamarin.Forms](~/xamarin-forms/user-interface/text/editor.md).
