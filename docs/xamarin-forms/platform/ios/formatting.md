---
title: Добавление форматирования, относящегося к iOS
description: В этой статье объясняется, как задать внешний вид iOS без использования пользовательского модуля подготовки Xamarin. Forms.
ms.prod: xamarin
ms.assetid: CE50E207-D092-4D88-8439-1B51F178E7ED
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/29/2016
ms.openlocfilehash: be353c6274dcf69946740e2d195b9e4d64208313
ms.sourcegitcommit: 1dd7d09b60fcb1bf15ba54831ed3dd46aa5240cb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/28/2019
ms.locfileid: "70121576"
---
# <a name="adding-ios-specific-formatting"></a>Добавление форматирования, относящегося к iOS

Одним из способов настройки форматирования для iOS является создание [пользовательского модуля подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md) отчетов для элемента управления и установка стилей и цветов для каждой платформы.

Другие параметры для управления отображением внешнего вида приложения для iOS в Xamarin. Forms:

- Настройка параметров вывода в [ **info. plist**](#info-plist)
- Установка стилей элементов управления через [ `UIAppearance` API](#uiappearance)

Эти альтернативы обсуждаются ниже.

<a name="info-plist"/>

## <a name="customizing-infoplist"></a>Настройка info. plist

Файл **info. plist** позволяет настроить некоторые аспекты Рендереринг приложения iOS, например способ отображения строки состояния (и состояние).

Например, в [примере TODO](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo) используется следующий код для установки цвета и цвета текста панели навигации на всех платформах:

```csharp
var nav = new NavigationPage (new TodoListPage ());
nav.BarBackgroundColor = Color.FromHex("91CA47");
nav.BarTextColor = Color.White;
```

Результат показан в следующем фрагменте экрана. Обратите внимание, что элементы строки состояния являются черными (не могут быть заданы в Xamarin. Forms, так как это функция для конкретной платформы).

![](theme-images/status-default-sml.png "данные для iOS")

В идеале строка состояния также будет содержать белый вид, что можно выполнить непосредственно в проекте iOS. Добавьте следующие записи в файле **info. plist** , чтобы строка состояния была белыми:

![](theme-images/info-plist.png "Записи сведений iOS. plist")

или измените соответствующий файл **info. plist** непосредственно для включения:

```xml
<key>UIStatusBarStyle</key>
<string>UIStatusBarStyleLightContent</string>
<key>UIViewControllerBasedStatusBarAppearance</key>
<false/>
```

Теперь при запуске приложения панель навигации отображается зеленым цветом, а ее текст — белым (из-за форматирования Xamarin. Forms) *, а* текст строки состояния также является белым благодаря конфигурации iOS:

![](theme-images/status-white-sml.png "данные для iOS")

<a name="uiappearance"/>

## <a name="uiappearance-api"></a>API Уиаппеаранце

API можно использовать для установки свойств визуального элемента во многих элементах управления iOS *без* необходимости создания [пользовательского модуля подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов. [ `UIAppearance` ](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)

Добавление одной строки кода в метод **AppDelegate.CS** `FinishedLaunching` может иметь стиль для всех элементов управления заданного типа, используя их `Appearance` свойство. Следующий код содержит два примера: глобально стилизацию строки табуляции и переключения управления.

**AppDelegate.CS** в проекте iOS

```csharp
public override bool FinishedLaunching (UIApplication app, NSDictionary options)
{
  // tab bar
    UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
  // switch
    UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
    // required Xamarin.Forms code
    Forms.Init ();
    LoadApplication (new App ());
    return base.FinishedLaunching (app, options);
}
```

### <a name="uitabbar"></a>уитаббар

По умолчанию выбранный значок панели вкладок в элементе[`TabbedPage`](~/xamarin-forms/app-fundamentals/navigation/tabbed-page.md)
будет синим:

![](theme-images/tabbar-default.png "Значок панели вкладок iOS по умолчанию в Таббедпаже")

Чтобы изменить это поведение, задайте `UITabBar.Appearance` свойство:

```csharp
UITabBar.Appearance.SelectedImageTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

В результате выбранная вкладка будет зеленым:

![](theme-images/tabbar-custom.png "Зеленый значок панели вкладок iOS в Таббедпаже")

Использование этого API позволяет настраивать внешний вид Xamarin. Forms `TabbedPage` в iOS с небольшими объемами кода. Дополнительные сведения об использовании пользовательского модуля подготовки отчетов для установки определенного шрифта для вкладки см. в описании инструкции по [настройке вкладок](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs) .

### <a name="uiswitch"></a>UISwitch

`Switch` Элемент управления — это еще один пример, который можно легко присвоить стилю:

```csharp
UISwitch.Appearance.OnTintColor = UIColor.FromRGB(0x91, 0xCA, 0x47); // green
```

Эти два снимка экрана показывают элемент управления `UISwitch` по умолчанию слева и настроенную версию (параметр `Appearance`) справа в [примере TODO](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/todo):

![](theme-images/switch-default.png "По умолчанию цвет UISwitch") ![](theme-images/switch-custom.png "настроить цвет UISwitch")

### <a name="other-controls"></a>Другие элементы управления

Многие элементы управления пользовательского интерфейса iOS могут иметь цвета по умолчанию и другие атрибуты, заданные с помощью [ `UIAppearance` API](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md).



## <a name="related-links"></a>Связанные ссылки

- [уиаппеаранце](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md)
- [Настройка вкладок](https://github.com/xamarin/recipes/tree/master/Recipes/xamarin-forms/iOS/customize-tabs)
