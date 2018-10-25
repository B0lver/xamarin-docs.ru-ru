---
title: Особенности платформы
description: Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов.
ms.prod: xamarin
ms.assetid: 4729DB9C-8800-4E29-9D66-3BE13C5F8C94
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/01/2018
ms.openlocfilehash: 284cce95b4b91d10c6516fa2880a48437340db49
ms.sourcegitcommit: 7f6127c2f425fadc675b77d14de7a36103cff675
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/24/2018
ms.locfileid: "38997303"
---
# <a name="platform-specifics"></a>Особенности платформы

_Особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов._

Следующие функциональные возможности платформы предоставляется для представления, страницы и макеты Xamarin.Forms:

|iOS|Android|Windows|
|--- |--- |--- |
|[VisualElement.BlurEffect](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#blur)|[VisualElement.Elevation](~/xamarin-forms/platform/platform-specifics/consuming/android.md#elevation)|[VisualElement.AccessKey, VisualElement.AccessKeyPlacement, VisualElement.AccessKeyHorizontalOffset и VisualElement.AccessKeyVerticalOffset](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#visualelement-accesskeys)|
|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#legacy-color-mode)|[VisualElement.IsLegacyColorModeEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#legacy-color-mode)|
|[VisualElement.IsShadowEnabled](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#drop-shadow)|

Предоставляются следующие функциональные возможности платформы для представления Xamarin.Forms:

|iOS|Android|Windows|
|--- |--- |--- |
|[Entry.AdjustsFontSizeToFitWidth](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#adjust_font_size)|[Button.UseDefaultPadding и Button.UseDefaultShadow](~/xamarin-forms/platform/platform-specifics/consuming/android.md#button-padding-shadow)|[InputView.DetectReadingOrderFromContent Label.DetectReadingOrderFromContent](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#inputview-readingorder)|
|[Entry.CursorColor](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#entry-cursorcolor)|[Entry.ImeOptions](~/xamarin-forms/platform/platform-specifics/consuming/android.md#entry-imeoptions)|[ListView.SelectionMode](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#listview-selectionmode)|
|[ListView.SeparatorStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#listview-separatorstyle)|[ListView.IsFastScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#fastscroll)|[SearchBar.IsSpellCheckEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#searchbar-spellcheck)|
|[Picker.UpdateMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#picker_update_mode)|[WebView.MixedContentMode](~/xamarin-forms/platform/platform-specifics/consuming/android.md#webview-mixed-content)|[WebView.IsJavaScriptAlertEnabled](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#webview-javascript-alert)|
|[Slider.UpdateOnTap](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#slider-updateontap)|

Следующие функциональные возможности платформы предоставляется для страницы Xamarin.Forms:

|iOS|Android|Windows|
|--- |--- |--- |
|[NavigationPage.HideSeparatorBar](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#navigationpage-hideseparatorbar)|[NavigationPage.BarHeight](~/xamarin-forms/platform/platform-specifics/consuming/android.md#navigationpage-barheight)|[MasterDetailPage.CollapsedPaneWidth и MasterDetailPage.CollapseStyle](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#collapsable_navigation_bar)|
|[NavigationPage.IsNavigationBarTranslucent](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#translucent_navigation_bar)|[TabbedPage.IsSmoothScrollEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-transition-animations)|[Page.ToolbarPlacement](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#toolbar_placement)|
|[NavigationPage.StatusBarTextColorMode](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#status_bar_color_mode)|[TabbedPage.IsSwipePagingEnabled](~/xamarin-forms/platform/platform-specifics/consuming/android.md#enable_swipe_paging)|[TabbedPage.HeaderIconsEnabled и TabbedPage.HeaderIconsSize](~/xamarin-forms/platform/platform-specifics/consuming/windows.md#tabbedpage-icons)|
|[NavigationPage.PrefersLargeTitles](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#large_title)|[TabbedPage.ToolbarPlacement TabbedPage.BarItemColor и TabbedPage.BarSelectedItemColor](~/xamarin-forms/platform/platform-specifics/consuming/android.md#tabbedpage-toolbar)|
|[Page.ModalPresentationStyle](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#modal-page-presentation-style)|
|[Page.PrefersStatusBarHidden и Page.PreferredStatusBarUpdateAnimation](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#set_status_bar_visibility)|
|[Page.UseSafeArea](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#safe_area_layout)|

Следующие функциональные возможности платформы предоставляется для макеты Xamarin.Forms:

|iOS|
|--- |
|[ScrollView.ShouldDelayContentTouches](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#delay_content_touches)|

Предоставляются следующие функциональные возможности платформы для Xamarin.Forms [ `Application` ](xref:Xamarin.Forms.Application) класса:

|iOS|Android|
|--- |--- |
|[Application.PanGestureRecognizerShouldRecognizeSimultaneously](~/xamarin-forms/platform/platform-specifics/consuming/ios.md#simultaneous-pan-gesture)|[Application.WindowSoftInputModeAdjust](~/xamarin-forms/platform/platform-specifics/consuming/android.md#soft_input_mode)|
||[Application.SendDisappearingEventOnPause Application.SendAppearingEventOnResume и Application.ShouldPreserveKeyboardOnResume](~/xamarin-forms/platform/platform-specifics/consuming/android.md#disable_lifecycle_events)|

## <a name="consuming-platform-specifics"></a>Использование особенностей платформы

Процесс для использования платформы через XAML или через API текучего кода выглядит следующим образом:

1. Добавить `xmlns` объявление или `using` директив для [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) пространства имен.
1. Добавить `xmlns` объявление или `using` директиву для пространства имен, которое содержит функциональные возможности платформы:
    1. В iOS это [ `Xamarin.Forms.PlatformConfiguration.iOSSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространства имен.
    1. В Android, это [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific) пространства имен. Для Android AppCompat это [ `Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat` ](xref:Xamarin.Forms.PlatformConfiguration.AndroidSpecific.AppCompat) пространства имен.
    1. На универсальной платформе Windows, это [ `Xamarin.Forms.PlatformConfiguration.WindowsSpecific` ](xref:Xamarin.Forms.PlatformConfiguration.WindowsSpecific) пространства имен.
1. Применить конкретной платформы из XAML или кода с помощью `On<T>` текучего API. Значение `T` может быть [ `iOS` ](xref:Xamarin.Forms.PlatformConfiguration.iOS), [ `Android` ](xref:Xamarin.Forms.PlatformConfiguration.Android), или [ `Windows` ](xref:Xamarin.Forms.PlatformConfiguration.Windows) типы из [ `Xamarin.Forms.PlatformConfiguration` ](xref:Xamarin.Forms.PlatformConfiguration) пространства имен.

> [!NOTE]
> Обратите внимание на то, что попытка использовать платформы на платформе, где он недоступен не приведет ошибку. Вместо этого код будет выполняться без применения конкретной платформы.

Особенности платформы востребован `On<T>` текучего кода API из [ `IPlatformElementConfiguration` ](xref:Xamarin.Forms.IPlatformElementConfiguration`2) объектов. Благодаря этому несколько особенностей платформы должен быть вызван на один и тот же объект с каскадным метод.

Дополнительные сведения о особенностей платформы, см. в разделе [особенностей платформы в использование](~/xamarin-forms/platform/platform-specifics/consuming/index.md) и [особенностей платформы в создании](~/xamarin-forms/platform/platform-specifics/creating.md).

## <a name="related-links"></a>Связанные ссылки

- [Использование особенностей платформы](~/xamarin-forms/platform/platform-specifics/consuming/index.md)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/creating.md)
- [PlatformSpecifics (пример)](https://developer.xamarin.com/samples/xamarin-forms/userinterface/platformspecifics/)
- [PlatformConfiguration](xref:Xamarin.Forms.PlatformConfiguration)
