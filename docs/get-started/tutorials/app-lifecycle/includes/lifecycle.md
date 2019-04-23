---
ms.openlocfilehash: 8bee6d7145cdc9bf22c01c12a4fb34cfb31cd9f4
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61187561"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **AppLifecycleTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **AppLifecycleTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** в проекте **AppLifecycleTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Затем, в файле **App.xaml.cs** обновите методы `OnStart`, `OnSleep` и `OnResume`, переопределив их следующим образом:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    Этот код обновит переопределения метода жизненного цикла приложения с помощью инструкций `Console.WriteLine`, которые указывают, когда был вызван каждый из методов.

    - Метод `OnStart` вызывается при запуске приложения.
    - Метод `OnSleep` вызывается когда приложение переводится в фоновый режим.
    - Метод `OnResume` вызывается при выходе из фонового режима.

    > [!NOTE]
    > Метода, отвечающего за завершение работы приложения, не существует. В обычных условиях завершение работы приложения происходит из-за метода `OnSleep`.

1. На панели инструментов Visual Studio нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения), чтобы запустить приложение в выбранном удаленном симуляторе iOS или эмуляторе Android. При запуске приложения вызывается метод `OnStart`, а затем **OnStart** выводится в Visual Studio в окно **Вывод**.

    ```
    [Mono] Found as 'java_interop_jnienv_get_object_array_element'.
    OnStart
    [OpenGLRenderer] HWUI GL Pipeline
    ```

    Когда приложение переходит в фоновый режим (после нажатия кнопки "Домой" в iOS или Android), вызывается метод `OnSleep`.

    ```
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    OnSleep
    [Mono] Image addref System.Runtime.Serialization[0x83ee19c0] -> System.Runtime.Serialization.dll[0x83f57b00]: 2
    ```

    Затем, когда восстанавливается работа приложения (после нажатия значка приложения в iOS, нажатия кнопки "Обзор" в Android, и выбора приложения AppLifecycleTutorial), вызывается метод `OnResume`.

    ```
    Thread finished: <Thread Pool> #5
    OnResume
    [EGL_emulation] eglMakeCurrent: 0x83ee2920: ver 3 0 (tinfo 0x8357eff0)
    ```

    > [!NOTE]
    > В этих блоках с кодом приведен пример результата выполнения приложения на Android.

    Дополнительные сведения о жизненном цикле приложения Xamarin.Forms см. в статье [Жизненный цикл приложения Xamarin.Forms](~/xamarin-forms/app-fundamentals/app-lifecycle.md).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **AppLifecycleTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Для фрагментов кода на C# и XAML в этом руководстве необходимо решение с именем **AppLifecycleTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **Панели решения** в проекте **AppLifecycleTutorial** разверните **App.xaml** и дважды щелкните файл **App.xaml.cs**, чтобы открыть его. Затем, в файле **App.xaml.cs** обновите методы `OnStart`, `OnSleep` и `OnResume`, переопределив их следующим образом:

    ```csharp
    protected override void OnStart()
    {
        Console.WriteLine("OnStart");
    }

    protected override void OnSleep()
    {
        Console.WriteLine("OnSleep");
    }

    protected override void OnResume()
    {
        Console.WriteLine("OnResume");
    }
    ```

    Этот код обновит переопределения метода жизненного цикла приложения с помощью инструкций `Console.WriteLine`, которые указывают, когда был вызван каждый из методов.

    - Метод `OnStart` вызывается при запуске приложения.
    - Метод `OnSleep` вызывается когда приложение переводится в фоновый режим.
    - Метод `OnResume` вызывается при выходе из фонового режима.

    > [!NOTE]
    > Метода, отвечающего за завершение работы приложения, не существует. В обычных условиях завершение работы приложения происходит из-за метода `OnSleep`.

1. Чтобы запустить приложения в выбранном симуляторе iOS или эмуляторе Android, нажмите кнопку **Пуск** (треугольная кнопка, похожая на кнопку воспроизведения) на панели инструментов Visual Studio для Mac. При запуске приложения вызывается метод `OnStart`, а затем **OnStart** выводится в Visual Studio для Mac в окно **Вывод приложения**.

    ```
    2019-02-11 12:05:23.164761+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:05:23.165297+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    2019-02-11 12:05:23.685430+0000 AppLifecycleTutorial.iOS[4089:361037] OnStart
    ```

    Когда приложение переходит в фоновый режим (после нажатия кнопки "Домой" в iOS или Android), вызывается метод `OnSleep`.

    ```
    2019-02-11 12:06:12.394301+0000 AppLifecycleTutorial.iOS[4089:361037] OnSleep
    Thread started: <Thread Pool> #3
    Thread started: <Thread Pool> #4
    Thread started: <Thread Pool> #5
    Thread started: <Thread Pool> #6
    2019-02-11 12:06:13.056968+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4089
    2019-02-11 12:06:13.057264+0000 AppLifecycleTutorial.iOS[4089:361037] SecTaskCopyDebugDescription: AppLifecycleTuto[4089]/0#-1 LF=0
    ```

    Затем, когда восстанавливается работа приложения (после нажатия значка приложения в iOS, нажатия кнопки "Обзор" в Android, и выбора приложения AppLifecycleTutorial), вызывается метод `OnResume`.

    ```
    2019-02-11 12:07:10.321905+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskLoadEntitlements failed error=22 cs_flags=200, pid=4130
    2019-02-11 12:07:10.322557+0000 AppLifecycleTutorial.iOS[4130:365695] SecTaskCopyDebugDescription: AppLifecycleTuto[4130]/0#-1 LF=0
    2019-02-11 12:07:17.110938+0000 AppLifecycleTutorial.iOS[4130:365695] OnResume
    ```

    > [!NOTE]
    > В этих блоках с кодом приведен пример результата выполнения приложения на iOS.

    Дополнительные сведения о жизненном цикле приложения Xamarin.Forms см. в статье [Жизненный цикл приложения Xamarin.Forms](~/xamarin-forms/app-fundamentals/app-lifecycle.md).
