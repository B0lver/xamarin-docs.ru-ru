---
ms.openlocfilehash: cef0b8f56639e7bc8571ab01b820dfd54b074472
ms.sourcegitcommit: 3f0e4f10e5def19122588bb05f26ab2baa9df6eb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/23/2020
ms.locfileid: "67277428"
---
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Для работы с этим руководством у вас должен быть последний выпуск Visual Studio 2019 с установленной рабочей нагрузкой **Разработка мобильных приложений на .NET**. Кроме того, вам потребуется компьютер Mac для сборки учебного приложения на iOS. Сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md). Сведения о подключении Visual Studio 2019 к узлу сборки Mac см. в статье [Связывание с Mac при разработке для Xamarin.iOS](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio и создайте пустое приложение Xamarin.Forms **WebServiceTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **WebServiceTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. В **обозревателе решений** выберите проект **WebServiceTutorial**, щелкните правой кнопкой мыши и выберите **Управление пакетами NuGet...**:

    ![Снимок экрана: выбранный пункт меню "Добавить пакеты NuGet"](../images/vs/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В разделе **Диспетчер пакетов NuGet** выберите вкладку **Обзор**, выполните поиск пакета NuGet **Newtonsoft.Json**, выберите его и нажмите кнопку **Установить**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet Newtonsoft.Json в диспетчере пакетов NuGet](../images/vs/add-package.png "Пакет NuGet Newtonsoft.Json")

    Этот пакет будет использоваться для включения десериализации JSON в приложение.

1. Соберите решение, чтобы убедиться в отсутствии ошибок.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Для работы с этим руководством вам нужно установить Visual Studio для Mac (последний выпуск) с поддержкой платформ Android и iOS. Кроме того, вам потребуется Xcode (последний выпуск). Дополнительные сведения об установке платформы Xamarin см. в статье [Установка Xamarin](~/get-started/installation/index.md).

1. Запустите Visual Studio для Mac и создайте пустое приложение Xamarin.Forms **WebServiceTutorial**. Убедитесь, что в качестве механизма общего кода в приложении используется .NET Standard.

    > [!IMPORTANT]
    > Фрагменты кода на C# и XAML из этого руководства предполагают, что решение называется **WebServiceTutorial**. Выбор другого имени приведет к ошибкам сборки при копировании кода из этого руководства в решение.

    Дополнительные сведения о создаваемой библиотеке .NET Standard см. в разделе [Структура приложения Xamarin.Forms](~/get-started/first-app/index.md) статьи [Краткое руководство по Xamarin.Forms глубокое погружение в обработку](~/get-started/first-app/index.md).

1. На **панели решений** выберите проект **WebServiceTutorial**, щелкните правой кнопкой мыши и выберите **Добавить > Добавить пакеты NuGet...**:

    ![Снимок экрана: выбранный пункт меню "Добавить пакеты NuGet"](../images/vsmac/add-nuget-packages.png "Элемент меню "Добавление пакетов NuGet"")

1. В окне **Добавление пакетов** выполните поиск пакета NuGet **Newtonsoft.Json**, выберите его и нажмите кнопку **Добавить пакет**, чтобы добавить его в проект:

    ![Снимок экрана: пакет NuGet Newtonsoft.Json в диспетчере пакетов NuGet](../images/vsmac/add-package.png "Пакет NuGet Newtonsoft.Json")

    Этот пакет будет использоваться для включения десериализации JSON в приложение.

1. Создайте решение, чтобы убедиться в отсутствии ошибок.
