---
title: Приступая к работе с DataPages
ms.prod: xamarin
ms.assetid: 6416E5FA-6384-4298-BAA1-A89381E47210
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: d06c3f95f55edb4870279bfc7d5326be4af4b3bc
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="getting-started-with-datapages"></a>Приступая к работе с DataPages

![](~/media/shared/preview.png "Этот API в настоящее время находится в предварительной версии")

> [!IMPORTANT]
> Требуется DataPages [Xamarin.Forms темы](~/xamarin-forms/user-interface/themes/index.md) ссылку для отображения.


Чтобы приступить к созданию страницы простой-диск с данными, DataPages предварительную версию, выполните следующие действия. Этот демонстрационный используют жестко закодировано стиля («события») в области предварительного просмотра сборках, работает только с определенного формата JSON в коде.

[![](get-started-images/demo-sml.png "Образец DataPages приложения")](get-started-images/demo.png#lightbox "DataPages примера приложения")

## <a name="1-add-nuget-packages"></a>1. Добавление пакетов NuGet

Добавьте эти пакеты Nuget Xamarin.Forms PCL и проекты приложений:

* Xamarin.Forms.Pages
* Xamarin.Forms.Theme.Base
* Для реализации темы Nuget (например) Xamarin.Forms.Themes.Light)

## <a name="2-add-theme-reference"></a>2. Добавление ссылки на темы

В **App.xaml** файл, добавьте пользовательский `xmlns:mytheme` для темы и обеспечить темы объединяется в словарь ресурсов приложения:

```xaml
<Application xmlns="http://xamarin.com/schemas/2014/forms"
  xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
  xmlns:mytheme="clr-namespace:Xamarin.Forms.Themes;assembly=Xamarin.Forms.Theme.Light"
  x:Class="DataPagesDemo.App">
    <Application.Resources>
        <ResourceDictionary MergedWith="mytheme:LightThemeResources" />
    </Application.Resources>
</Application>
```

**Важно:** необходимо также выполнить шаги для [загружать темы сборки (см. ниже)](#loadtheme) , добавив некоторые стандартный код для iOS `AppDelegate` и Android `MainActivity`. Это будет улучшена в будущем предварительного выпуска.


## <a name="3-add-a-xaml-page"></a>3. Добавьте страницу XAML

Добавьте новую страницу XAML в приложение Xamarin.Forms и *измените базовый класс* из `ContentPage` для `Xamarin.Forms.Pages.ListDataPage`. Это должна быть выполнена в C# и XAML:

**Файл C#**

```csharp
public partial class SessionDataPage : Xamarin.Forms.Pages.ListDataPage // was ContentPage
{
  public SessionDataPage ()
  {
    InitializeComponent ();
  }
}
```

**XAML-файла**

Помимо изменения корневого элемента, чтобы `<p:ListDataPage>` пользовательского пространства имен для `xmlns:p` также должен быть добавлен:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage">

    <ContentPage.Content></ContentPage.Content>

</p:ListDataPage>
```

**Подкласс приложения**

Изменение `App` конструктора класса, чтобы `MainPage` равно `NavigationPage` содержащий новый `SessionDataPage`. На странице навигации *должен* использоваться.

```csharp
MainPage = new NavigationPage (new SessionDataPage ());
```

## <a name="3-add-the-datasource"></a>3. Добавление источника данных

Удалить `Content` элемента и замените ее строкой `p:ListDataPage.DataSource` для заполнения страницы с данными. В примере ниже удаленного Json выполняется загрузка файла данных с URL-адреса.

**Примечание:** предварительной версии *требует* `StyleClass` атрибут, чтобы задать отрисовки подсказок для источника данных. `StyleClass="Events"` Ссылается на макет, который является стандартным в предварительной версии и содержит стили *жестко* для сопоставления используемого источника данных JSON.

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<p:ListDataPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:p="clr-namespace:Xamarin.Forms.Pages;assembly=Xamarin.Forms.Pages"
             x:Class="DataPagesDemo.SessionDataPage"
             Title="Sessions" StyleClass="Events">

    <p:ListDataPage.DataSource>
        <p:JsonDataSource Source="http://demo3143189.mockable.io/sessions" />
    </p:ListDataPage.DataSource>

</p:ListDataPage>
```

**Данные JSON**

Пример данных JSON из [Демонстрация источника](http://demo3143189.mockable.io/sessions) показано ниже:

```json
[{
  "end": "2016-04-27T18:00:00Z",
  "start": "2016-04-27T17:15:00Z",
  "abstract": "The new Apple TV has been released, and YOU can be one of the first developers to write apps for it. To make things even better, you can build these apps in C#! This session will introduce the basics of how to create a tvOS app with Xamarin, including: differences between tvOS and iOS APIs, TV user interface best practices, responding to user input, as well as the capabilities and limitations of building apps for a television. Grab some popcorn—this is going to be good!",
  "title": "As Seen On TV … Bringing C# to the Living Room",
  "presenter": "Matthew Soucoup",
  "biography": "Matthew is a Xamarin MVP and Certified Xamarin Developer from Madison, WI. He founded his company Code Mill Technologies and started the Madison Mobile .Net Developers Group.  Matt regularly speaks on .Net and Xamarin development at user groups, code camps and conferences throughout the Midwest. Matt gardens hot peppers, rides bikes, and loves Wisconsin micro-brews and cheese.",
  "image": "http://i.imgur.com/ASj60DP.jpg",
  "avatar": "http://i.imgur.com/ASj60DP.jpg",
  "room": "Crick"
}]
```

## <a name="4-run"></a>4. Запустите файл!

Указанные выше шаги, приводить к работе страницы данных:

[![](get-started-images/demo-sml.png "Образец DataPages приложения")](get-started-images/demo.png#lightbox "DataPages примера приложения")

Это происходит потому, что готовые стиль **«События»** существует в пакет Nuget Light темы и стили, определенные, совпадает с источником данных (например) «title», «image», «докладчика»).

«События» `StyleClass` созданный для отображения `ListDataPage` управления с пользовательским `CardView` управления, являющийся определено в Xamarin.Forms.Pages. `CardView` Управления имеет три свойства: `ImageSource`, `Text`, и `Detail`. Темы — жестко задано для привязки три поля источника данных (из файла JSON) для этих свойств для отображения.

## <a name="5-customize"></a>5. Настройка

Наследуемые стиля можно переопределить, указав шаблон и с помощью привязки источников данных. Приведенный ниже код XAML объявляет пользовательский шаблон для каждой строки, используя новый `ListItemControl` и `{p:DataSourceBinding}` синтаксис, входящей в состав **Xamarin.Forms.Pages** Nuget:

```xaml
<p:ListDataPage.DefaultItemTemplate>
    <DataTemplate>
        <ViewCell>
            <p:ListItemControl
                Title="{p:DataSourceBinding title}"
                Detail="{p:DataSourceBinding room}"
                ImageSource="{p:DataSourceBinding image}"
                DataSource="{Binding Value}"
                HeightRequest="90"
            >
            </p:ListItemControl>
        </ViewCell>
    </DataTemplate>
</p:ListDataPage.DefaultItemTemplate>
```

Предоставляя `DataTemplate` этот код переопределяет `StyleClass` и использует вместо него макет по умолчанию для `ListItemControl`.

[![](get-started-images/custom-sml.png "Образец DataPages приложения")](get-started-images/custom.png#lightbox "DataPages примера приложения")

Разработчики, которые предпочитают C# в XAML можно создавать данные источника привязки слишком (не забудьте включить `using Xamarin.Forms.Pages;` инструкции):

```csharp
SetBinding (TitleProperty, new DataSourceBinding ("title"));
```


Это немного больше усилий, чтобы создать темы с нуля (см. [темы руководства](~/xamarin-forms/user-interface/themes/index.md)), но будущих предварительных выпусках поможет упростить это делать.


## <a name="troubleshooting"></a>Устранение неполадок

<a name="loadtheme" />

## <a name="could-not-load-file-or-assembly-xamarinformsthemelight-or-one-of-its-dependencies"></a>Не удалось загрузить файл или сборку «Xamarin.Forms.Theme.Light» или одну из ее зависимостей

В предварительном выпуске темы может быть не удалось загрузить во время выполнения. Добавьте код, показанный ниже, в соответствующие проекты для устранения этой ошибки.

**iOS**

В **AppDelegate.cs** добавьте следующие строки после `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.iOS.UnderlineEffect);
```

**Android**

В **MainActivity.cs** добавьте следующие строки после `LoadApplication`

```csharp
var x = typeof(Xamarin.Forms.Themes.DarkThemeResources);
x = typeof(Xamarin.Forms.Themes.LightThemeResources);
x = typeof(Xamarin.Forms.Themes.Android.UnderlineEffect);
```



## <a name="related-links"></a>Связанные ссылки

- [Образец DataPagesDemo](https://github.com/xamarin/xamarin-forms-samples/tree/master/Pages/DataPagesDemo)
