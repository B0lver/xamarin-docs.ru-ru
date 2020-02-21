---
title: Обнаружение темного режима в приложениях Xamarin. Forms
description: Темный режим может поддерживаться в любом приложении Xamarin. Forms с помощью сочетания ResourceDictionary, Динамикресаурцес и набора знаний платформы.
ms.prod: xamarin
ms.assetid: D10506DD-BAA0-437F-A4AD-882D16E7B60D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 02/19/2020
ms.openlocfilehash: 7136e3240a39321b2d67ca29c16a0758cf5c4cfb
ms.sourcegitcommit: 524fc148bad17272bda83c50775771daa45bfd7e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/20/2020
ms.locfileid: "77486292"
---
# <a name="detect-dark-mode-in-xamarinforms-applications"></a>Обнаружение темного режима в приложениях Xamarin. Forms

Приложения Xamarin. Forms могут реагировать на изменения темы операционной системы с помощью той же стратегии, которая позволяет поддерживать [их](theming.md). Основное различие заключается в том, как активируется изменение темы. Темный режим относится к более широкому набору предпочтений внешнего вида, который можно задать на уровне операционной системы, а также о том, какие приложения должны отвечать и реагировать немедленно.

Ниже приведены минимальные требования к использованию темного режима в приложениях Xamarin. Forms.

- iOS 13 или более поздняя.
- Android 9 или более поздней версии (Android 9 поддерживает режимы отображения, которые можно запрашивать).
- Android 10 или более поздней версии (Android 10 уведомляет приложения об изменениях режима).
- UWP v 10.0.10240.0 или более поздней версии.
- Xamarin. Forms (любая версия).

Ниже приведен процесс поддержки режимов внешнего вида, включая «светлый» и «темный».

1. Определите ресурсы, общие для всего приложения, в [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary).
2. Определите тему словаря ресурсов для каждого режима оформления, который предоставляет переопределения для каждого стиля, который вы хотите изменить.
3. Задайте тему режима отображения по умолчанию в файле **app. XAML** приложения.
4. Задайте для приложения стиль, используя расширение разметки `DynamicResource`, в котором должны реагировать стили при изменении режима отображения.
5. Добавьте код для прослушивания изменений темы ОС и загрузки соответствующей темы приложения.

На следующих снимках экрана показаны страницы с темами для светлых и темных режимов:

[![Снимок экрана: Главная страница приложения с темой, на снимке экрана iOS и android](theming-images/main-page-both-themes.png "Главная страница приложения с темой")](theming-images/main-page-both-themes-large.png#lightbox "Главная страница приложения с темой")
на [ ![странице сведений в приложении с темой, в iOS и Android](theming-images/detail-page-both-themes.png "Страница сведений о приложении с темой")](theming-images/detail-page-both-themes-large.png#lightbox "Страница сведений о приложении с темой")

## <a name="define-themes"></a>Определение тем

Пошаговые инструкции по созданию темных и светлых тем см. [в этом разделе](theming.md) .

## <a name="react-to-appearance-mode-changes"></a>Реагирование на изменения режима отображения

Режим отображения на устройстве может измениться по ряду причин, в зависимости от того, как пользователь настроил свои предпочтения, включая явное выбор режима, времени суток или таких факторов, как низкая-лампочка. Вам потребуется добавить код платформы, чтобы приложение может реагировать на эти изменения, а в следующих разделах это более подробно обсуждается.

### <a name="android"></a>Android

В файле **MainActivity.CS** приложения добавьте поле `ConfigChanges.UiMode` в свойство `ConfigurationChanges` в атрибуте `Activity`, чтобы ваше приложение получало уведомления об изменениях в режиме пользовательского интерфейса:

```csharp
[Activity(
    // other settings
    ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation | ConfigChanges.UiMode)]
```

В том же файле **MainActivity.CS** Переопределите метод `OnConfigurationChanged`:

```csharp
public override void OnConfigurationChanged(Configuration newConfig)
{
    base.OnConfigurationChanged(newConfig);

    if ((newConfig.UiMode & UiMode.NightNo) != 0)
    {
        // change to light theme
        // e.g. App.Current.Resources = new YourLightTheme();
    }
    else
    {
        // change to dark theme
        // e.g. App.Current.Resources = new YourDarkTheme();
    }
}
```

### <a name="ios"></a>iOS

В iOS изменения режима отображения уведомляются о UIViewController, который в Xamarin. Forms является `ContentPage`. Чтобы переопределить этот метод, создайте пользовательский модуль подготовки отчетов в проекте iOS с именем `PageRenderer.cs`:

```csharp
using System;
using UIKit;
using Xamarin.Forms;
using Xamarin.Forms.Platform.iOS;

[assembly: ExportRenderer(typeof(ContentPage), typeof(YourApp.iOS.Renderers.PageRenderer))]
namespace YourApp.iOS.Renderers
{
    public class PageRenderer : Xamarin.Forms.Platform.iOS.PageRenderer
    {
        protected override void OnElementChanged(VisualElementChangedEventArgs e)
        {
            base.OnElementChanged(e);

            if (e.OldElement != null || Element == null)
            {
                return;
            }

            try
            {
                SetAppTheme();
            }
            catch (Exception ex)
            {
                System.Diagnostics.Debug.WriteLine($"\t\t\tERROR: {ex.Message}");
            }
        }

        public override void TraitCollectionDidChange(UITraitCollection previousTraitCollection)
        {
            base.TraitCollectionDidChange(previousTraitCollection);

            if(this.TraitCollection.UserInterfaceStyle != previousTraitCollection.UserInterfaceStyle)
            {
                SetAppTheme();
            }
        }

        void SetAppTheme()
        {
            if (this.TraitCollection.UserInterfaceStyle == UIUserInterfaceStyle.Dark)
            {
                // change to dark theme
                // e.g. App.Current.Resources = new YourDarkTheme();
            }
            else
            {
                // change to light theme
                // e.g. App.Current.Resources = new YourLightTheme();
            }
        }
    }
}
```

Переопределение метода `TraitCollectionDidChange` позволяет действовать при изменении `UserInterfaceStyle`.

### <a name="uwp"></a>UWP

В UWP добавьте следующий код в файл **MainPage.XAML.CS** приложения:

```csharp
public sealed partial class MainPage
{

    UISettings uiSettings;

    public MainPage()
    {
        this.InitializeComponent();

        LoadApplication(new YourApp.App());

        uiSettings = new UISettings();
        uiSettings.ColorValuesChanged += ColorValuesChanged;
    }

    private void ColorValuesChanged(UISettings sender, object args)
    {
        var backgroundColor = sender.GetColorValue(UIColorType.Background);
        var isDarkMode = backgroundColor == Colors.Black;
        if(isDarkMode)
        {
            Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
            {
                // change to dark theme
                // e.g. App.Current.Resources = new YourDarkTheme();
            });
        }
        else
        {
            Xamarin.Essentials.MainThread.BeginInvokeOnMainThread(() =>
            {
                // change to light theme
                // e.g. App.Current.Resources = new YourLightTheme();
            });
        }
    }
}
```

## <a name="set-dark-and-light-themes"></a>Установка темных и светлых тем

После [выполнения этих рекомендаций можно](theming.md) легко настроить новую тему для приложения в соответствующем расположении кода платформы, приведенном выше. Чтобы загрузить новую тему, просто замените текущий словарь ресурсов приложения на выбранный словарь ресурсов с темой.

```csharp
App.Current.Resources = new YourDarkTheme();
```

## <a name="related-links"></a>Связанные ссылки

- [Их (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-theming/)
- [Словари ресурсов](~/xamarin-forms/xaml/resource-dictionaries.md)
- [Динамические стили в Xamarin. Forms](~/xamarin-forms/user-interface/styles/xaml/dynamic.md)
- [Задание стиля приложений Xamarin.Forms с помощью стилей XAML](~/xamarin-forms/user-interface/styles/xaml/index.md)
