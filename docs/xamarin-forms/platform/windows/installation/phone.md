---
title: Добавление приложения Windows Phone
ms.prod: xamarin
ms.assetid: B598FA9D-6818-4CC9-8191-838C156DB9DA
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/16/2016
ms.openlocfilehash: 55bd4bdcfde4c91ad5c9b94bef486207466e135d
ms.sourcegitcommit: dc882e9631b4ed52596b944a6fbbdde309346943
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/26/2018
---
# <a name="adding-a-windows-phone-app"></a>Добавление приложения Windows Phone


Во-первых, если вы использовали шаблоне Xamarin.Forms PCL [обновить профиль](~/xamarin-forms/platform/windows/installation/index.md), следуйте приведенным ниже инструкциям:

1. **Щелкните правой кнопкой мыши решение > Добавить > Новый проект...**  и добавьте **пустое приложение (Windows Phone)**

  ![](phone-images/add-wp81.png "Добавить диалоговое окно нового проекта")

2. **Щелкните правой кнопкой мыши только что созданный проект > Управление пакетами NuGet...**  и добавьте **Xamarin.Forms** пакета.

3. **правой кнопкой мыши проект > Добавить > справочник** и создать ссылку на проект в общий проект приложения Xamarin.Forms.

  ![](phone-images/addref.png "Диалоговое окно диспетчера ссылок")

4. Изменить **App.xaml.cs** для включения `Init()` вызов метода, в `OnLaunched` метод около строки 67:

```csharp
// add this line
Xamarin.Forms.Forms.Init (e); // requires LaunchActivatedEventArgs
// above this existing line
if (e.PreviousExecutionState == ApplicationExecutionState.Terminated) {}
```

 5. Изменить **MainPage.xaml** -измените корневой элемент `<Page` для `<forms:WindowsPhonePage` *и* определить `xmlns:forms` , он использует:

```xaml
<forms:WindowsPhonePage
...
   xmlns:forms="using:Xamarin.Forms.Platform.WinRT"
...
</forms:WindowsPhonePage>
```

 6. Изменить **MainPage.xaml.cs** удаление `: PhonePage` описатель наследования для имени класса.

```csharp
public sealed partial class MainPage  // REMOVE ": PhonePage"
```

 7. В по-прежнему **MainPage.xaml.cs**, добавьте `LoadApplication` вызов в `MainPage` конструктор (около строки 28) для запуска приложения Xamarin.Forms:

```csharp
// below this existing line
this.InitializeComponent();
// add this line
LoadApplication(new YOUR_NAMESPACE.App());
```

8. Дважды щелкните **Package.appxmanifest** для задания этих возможностей, которые часто требуются:

  * Интернет (клиент и сервер)

9. Наконец добавьте все локальные ресурсы (например) файлы изображений) с существующие проекты платформы, которые необходимы.

