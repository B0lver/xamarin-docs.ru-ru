---
title: Настройка проектов Windows
description: Xamarin.FormsВ старых решениях (или созданных на macOS) не будет универсальная платформа Windows проектов, поэтому в этой статье объясняется, как добавить новый проект UWP в существующее Xamarin.Forms решение.
ms.prod: xamarin
ms.assetid: A0774D2E-6994-4D91-84E8-DAB66FC92320
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/10/2018
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 06f7a138ee27fe095b99a55917267aaa6e01998e
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86939026"
---
# <a name="setup-windows-projects"></a>Настройка проектов Windows

_Добавление новых проектов Windows в существующее Xamarin.Forms решение_

В старых Xamarin.Forms решениях (или созданных на macOS) не будет проектов приложений универсальная платформа Windows (UWP). Поэтому необходимо вручную добавить проект UWP для создания приложения Windows 10 (UWP).

## <a name="add-a-universal-windows-platform-app"></a>Добавление универсальная платформа Windows приложения

Для создания приложений UWP рекомендуется **Visual Studio 2019** в **Windows 10** . Дополнительные сведения о универсальная платформа Windows см. в разделе [Введение в универсальная платформа Windows](/windows/uwp/get-started/universal-application-platform-guide/).

UWP доступен в Xamarin.Forms 2,1 и более поздних версиях и Xamarin.Forms . Карты поддерживаются в Xamarin.Forms 2,2 и более поздних версиях.

Полезные советы см. в разделе <a href="#troubleshooting">Устранение неполадок</a> .

Выполните эти инструкции, чтобы добавить приложение UWP, которое будет работать на телефонах, планшетах и настольных компьютерах под управлением Windows 10:

 одного. Щелкните решение правой кнопкой мыши и выберите **добавить > новый проект...** и добавьте пустой проект **приложения (универсальное приложение Windows)** :

  ![Диалоговое окно "Добавить новый проект"](universal-images/add-wu.png)

 открыт. В диалоговом окне **новый универсальная платформа Windows проект** Выберите минимальную и целевую версии Windows 10, на которых будет выполняться приложение:

  ![Диалоговое окно нового универсальная платформа Windows проекта](universal-images/target-version.png)

 3-5. Щелкните правой кнопкой мыши проект UWP и выберите пункт **Управление пакетами NuGet...** и добавьте **Xamarin.Forms** пакет. Убедитесь, что другие проекты в решении также обновлены до той же версии Xamarin.Forms пакета.

 четырех. Убедитесь, что новый проект UWP будет построен в окне **сборки > Configuration Manager** (это, вероятно, не будет выполнено по умолчанию). Тикайте поля **Сборка** и **развертывание** для универсального проекта:

  [![Окно Configuration Manager](universal-images/configuration-sml.png)](universal-images/configuration.png#lightbox "Окно Configuration Manager")

 5.0. Щелкните проект правой кнопкой мыши и выберите **добавить > ссылку** , а затем создайте ссылку на Xamarin.Forms проект приложения (.NET Standard или общий проект).

  ![Диалоговое окно «Диспетчер ссылок»](universal-images/addref-sml.png)

 1-6. В проекте UWP измените **app.XAML.CS** , чтобы включить `Init` вызов метода внутри `OnLaunched` метода, окружающего строку 52:

```csharp
// under this line
rootFrame.NavigationFailed += OnNavigationFailed;
// add this line
Xamarin.Forms.Forms.Init (e); // requires the `e` parameter
```

 7. В проекте UWP измените **MainPage. XAML** , удалив элемент, `Grid` содержащийся в `Page` элементе.

 8. В **MainPage. XAML**добавьте новую `xmlns` запись для `Xamarin.Forms.Platform.UWP` :

```csharp
xmlns:forms="using:Xamarin.Forms.Platform.UWP"
```

 8. В **MainPage. XAML**Измените корневой `<Page` элемент на `<forms:WindowsPage` :

```xaml
<forms:WindowsPage
...
   xmlns:forms="using:Xamarin.Forms.Platform.UWP"
...
</forms:WindowsPage>
```

 штук. В проекте UWP измените **MainPage.XAML.CS** , чтобы удалить `: Page` спецификатор наследования для имени класса (поскольку теперь он будет наследовать из `WindowsPage` -за изменений, внесенных на предыдущем шаге):

```csharp
public sealed partial class MainPage  // REMOVE ": Page"
```

 стр. В **MainPage.XAML.CS**добавьте `LoadApplication` вызов в `MainPage` конструкторе, чтобы запустить Xamarin.Forms Приложение:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

> [!NOTE]
> Аргумент `LoadApplication` метода является `Xamarin.Forms.Application` экземпляром, определенным в проекте .NET Standard.

<!--
11 . Double-click **Package.appxmanifest** to set these capabilities
  that are often required:

  Capabilities set:

  * Internet (Client)
  * Location
-->

Двенадцать. Добавьте локальные ресурсы (например, файлы изображений) из существующих проектов платформы, которые необходимы.

## <a name="troubleshooting"></a>Диагностика

### <a name="target-invocation-exception-when-using-compile-with-net-native-tool-chain"></a>"Целевое исключение вызова" при использовании "Compile с .NET Nativeной цепочкой инструментов"

Если приложение UWP ссылается на несколько сборок (например, сторонние библиотеки элементов управления или само приложение разделено на несколько библиотек), Xamarin.Forms может быть невозможно загрузить объекты из этих сборок (например, пользовательские модули подготовки отчетов).

Это может произойти при использовании **цепочки инструментов Compile с .NET Native** , которая является параметром для приложений UWP в окне **свойства > сборка > общие** для проекта.

Это можно исправить с помощью перегрузки вызова, зависящей от UWP, `Forms.Init` в **app.XAML.CS** , как показано в приведенном ниже коде (необходимо заменить `ClassInOtherAssembly` фактическим классом, на который ссылается код):

```csharp
// You'll need to add `using System.Reflection;`
List<Assembly> assembliesToInclude = new List<Assembly>();

// Now, add in all the assemblies your app uses
assembliesToInclude.Add(typeof (ClassInOtherAssembly).GetTypeInfo().Assembly);

// Also do this for all your other 3rd party libraries
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
// replaces Xamarin.Forms.Forms.Init(e);
```

Добавьте запись для каждой сборки, добавленной в качестве ссылки в обозреватель решений, с помощью прямой ссылки или NuGet.

#### <a name="dependency-services-and-net-native-compilation"></a>Компиляция служб зависимостей и .NET Native

Сборки выпуска с использованием .NET Nativeной компиляции могут не разрешать службы зависимостей, определенные вне основного исполняемого файла приложения (например, в отдельном проекте или библиотеке).

Используйте `DependencyService.Register<T>()` метод, чтобы вручную зарегистрировать классы служб зависимостей. В соответствии с приведенным выше примером добавьте метод Register следующим образом:

```csharp
Xamarin.Forms.Forms.Init(e, assembliesToInclude);
Xamarin.Forms.DependencyService.Register<ClassInOtherAssembly>(); // add this
```
