---
title: Специальные возможности в iOS
description: В этом документе описывается доступность в iOS, обсуждаются различные свойства и функции, которые можно использовать для того, чтобы приложение было доступно максимальному числу пользователей.
ms.prod: xamarin
ms.assetid: 88D59B36-05A3-4356-AE29-EC2B69CE7162
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/18/2016
ms.openlocfilehash: 943cdfaee07bc4fd4ed3273840036055ad40b89a
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70766752"
---
# <a name="accessibility-on-ios"></a>Специальные возможности в iOS

На этой странице описывается, как использовать интерфейсы API специальных возможностей iOS для создания приложений в соответствии с [контрольным списком специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md).
См. страницы специальных [возможностей Android](~/android/app-fundamentals/accessibility.md) и [Специальные возможности OS X](~/mac/app-fundamentals/accessibility.md) для других API-интерфейсов платформы.

## <a name="describing-ui-elements"></a>Описание элементов пользовательского интерфейса

iOS предоставляет `AccessibilityLabel` разработчикам `AccessibilityHint` свойства и для добавления описательного текста, который может использоваться средством чтения с экрана VoiceOver, чтобы сделать элементы управления более доступными. Элементы управления также могут быть помечены одной или несколькими характеристиками, которые предоставляют дополнительный контекст в доступных режимах.

Некоторые элементы управления могут быть недоступными (например, метка для текстового ввода или изображение, которое является чисто декоративным) — `IsAccessibilityElement` предоставляется для отключения специальных возможностей в этих случаях.

**Конструктор пользовательского интерфейса**

**Панель свойств** содержит раздел со специальными возможностями, позволяющий изменять эти параметры при выборе элемента управления в КОНСТРУКТОРЕ пользовательского интерфейса iOS:

![](accessibility-images/ios-designer-sml.png "Параметры специальных возможностей")

**C#**

Эти свойства также можно задать непосредственно в коде:

```csharp
usernameInput.AccessibilityLabel = "Search";
usernameInput.Hint = "Press Enter after typing to search employee list";
someLabel.IsAccessibilityElement = false;
displayOnlyText.AccessibilityTraits = UIAccessibilityTrait.Header | UIAccessibilityTrait.Selected;
```

### <a name="what-is-accessibilityidentifier"></a>Что такое Акцессибилитидентифиер?

`AccessibilityIdentifier` Используется для задания уникального ключа, который можно использовать для ссылки на элементы пользовательского интерфейса через API UIAutomation.

Значение параметра `AccessibilityIdentifier` никогда не будет обводиться или отображаться для пользователя.

<a name="postnotification" />

## <a name="postnotification"></a>Уведомление

`UIAccessibility.PostNotification` Метод позволяет вызывать события пользователю за пределами прямого взаимодействия (например, при взаимодействии с конкретным элементом управления).

### <a name="announcement"></a>Замечание

Объявление может быть отправлено из кода, чтобы информировать пользователя о том, что изменилось состояние (например, завершение фоновой операции). Это может сопровождаться визуальным индикатором в пользовательском интерфейсе:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.Announcement,
    new NSString(@"Item was saved"));
```

### <a name="layoutchanged"></a>лайаутчанжед

`LayoutChanged` Объявление используется, когда макет экрана:

```csharp
UIAccessibility.PostNotification (
  UIAccessibilityPostNotification.LayoutChanged,
    someControl);  // someControl gets focus
```

## <a name="accessibility-and-localization"></a>Специальные возможности и локализация

Свойства специальных возможностей, такие как метка и подсказка, могут быть локализованы так же, как и другие тексты в пользовательском интерфейсе.

**Файл mainstoryboard. strings**

Если пользовательский интерфейс располагается в раскадровке, можно предоставить переводы для свойств специальных возможностей так же, как и другие свойства. В приведенном ниже примере у `UITextField` a есть **идентификатор** `Pqa-aa-ury` локализации и два свойства специальных возможностей, устанавливаемые на испанском языке:

```csharp
/* Accessibility */
"Pqa-aa-ury.accessibilityLabel" = "Notas input";
"Pqa-aa-ury.accessibilityHint" = "escriba más información";
```

Этот файл будет помещен в каталог **ES. лпрож** для испанского содержимого.

**Локализуемые. strings**

Кроме того, переводы можно добавить в файл **Localizable. strings** в каталоге локализованного содержимого (например, **ES. лпрож** для испанского языка):

```csharp
/* Accessibility */
"Notes" = "Notas input";
"Provide more information" = "escriba más información";
```

Эти переводы можно использовать с C# помощью `LocalizedString` метода:

```csharp
notesText.AccessibilityLabel = NSBundle.MainBundle.LocalizedString ("Notes", "");
notesText.AccessibilityHint = NSBundle.MainBundle.LocalizedString ("Provide more information", "");
```

Дополнительные сведения о локализации содержимого см. в [руководстве по локализации iOS](~/ios/app-fundamentals/localization/index.md) .

<a name="testing" />

## <a name="testing-accessibility"></a>Тестирование специальных возможностей

VoiceOver включается в приложении " **Параметры** " путем перехода к **общему > Специальные возможности > VoiceOver**:

![](accessibility-images/settings-sml.png "Настройка скорости речи")

На экране **Специальные возможности** также приводятся параметры масштабирования, размера текста, цвета & контрастности, параметров речи и других параметров конфигурации.

Выполните следующие [инструкции VoiceOver](https://developer.apple.com/library/ios/technotes/TestingAccessibilityOfiOSApps/TestAccessibilityonYourDevicewithVoiceOver/TestAccessibilityonYourDevicewithVoiceOver.html) , чтобы проверить доступность на устройствах iOS.

## <a name="simulator-testing"></a>Тестирование имитатора

При тестировании в симуляторе доступен **инспектор специальных возможностей** , позволяющий проверить правильность настройки свойств и событий специальных возможностей. Чтобы включить инспектор в приложении " **Параметры** ", перейдите к **общей > Специальные возможности > специальные возможности инспектора специальных возможностей**:

![](accessibility-images/settings-inspector-sml.png "Включить инспектор специальных возможностей")

После включения окно инспектора отображается на экране iOS в любое время.
Ниже приведен пример выходных данных, когда выбрана строка представления таблицы. Обратите внимание, что **Метка** содержит предложение, которое дает содержимое строки, а также то, что это «готово» (элемент «выполнено» (Internet Explorer)):

![](accessibility-images/tableview-a11y-sml.png "Использование инспектора специальных возможностей")

Пока инспектор виден, используйте значок "X" в левом верхнем углу, чтобы временно показать и скрыть наложение, а также включить или отключить параметры специальных возможностей.

## <a name="related-links"></a>Связанные ссылки

- [Кросс-платформенные специальные возможности](~/cross-platform/app-fundamentals/accessibility.md)
- [Специальные возможности iOS (Apple)](https://developer.apple.com/library/ios/documentation/UserExperience/Conceptual/iPhoneAccessibility/Accessibility_on_iPhone/Accessibility_on_iPhone.html)
- [VoiceOver iOS](http://www.apple.com/accessibility/ios/voiceover/)
