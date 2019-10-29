---
title: Интерфейс API внешнего вида в Xamarin. iOS
description: iOS позволяет применять параметры визуальных свойств на уровне статического класса, а не на отдельных объектах, чтобы изменение применялось ко всем экземплярам этого элемента управления в приложении.
ms.prod: xamarin
ms.assetid: C1727F0C-82B1-D085-D46F-C6383FF04B16
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 11/15/2018
ms.openlocfilehash: 7a7f0fe9d0dc07d892686e6596f3cc09a2587513
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73003379"
---
# <a name="appearance-api-in-xamarinios"></a>Интерфейс API внешнего вида в Xamarin. iOS

_iOS позволяет применять параметры визуальных свойств на уровне статического класса, а не на отдельных объектах, чтобы изменение применялось ко всем экземплярам этого элемента управления в приложении._

Эта функция предоставляется в Xamarin. iOS через статическое свойство `Appearance` для всех элементов управления UIKit, которые его поддерживают. Таким образом, внешний вид (свойства как цвет оттенка и фоновое изображение) можно легко настроить, чтобы обеспечить единообразное оформление приложения. API внешнего вида был представлен в iOS 5, а некоторые части его не рекомендуются в iOS 9. он по-прежнему является хорошим способом достижения некоторых стилей и эффектов в приложениях Xamarin. iOS.

## <a name="overview"></a>Обзор

iOS позволяет настраивать внешний вид многих элементов управления UIKit, чтобы обеспечить соответствие стандартных элементов управления фирменной символике, которую вы хотите применить к вашему приложению.

Существует два разных способа применения пользовательского внешнего вида.

- **Непосредственно на экземпляре элемента управления** — можно задать цвет оттенка, фоновое изображение и положение заголовка (а также другие атрибуты) во многих элементах управления, включая панели инструментов, панели навигации, кнопки и ползунки.

- **Задать значения по умолчанию для статического свойства внешнего вида** — настраиваемые атрибуты для каждого элемента управления предоставляются через статическое свойство `Appearance`. Все настройки, применяемые к этим свойствам, будут использоваться по умолчанию для любого элемента управления этого типа, созданного после установки свойства.

Пример приложения "внешний вид" демонстрирует все три метода, как показано на снимках экрана:

[![](introduction-to-the-appearance-api-images/appearance01-sml.png "The Appearance sample application demonstrates all three methods")](introduction-to-the-appearance-api-images/appearance01.png#lightbox)

Начиная с iOS 8, прокси-сервер внешнего вида был расширен до Траитколлектионс.
 `AppearanceForTraitCollection` можно использовать для задания внешнего вида по умолчанию для определенной коллекции признаков. Дополнительные сведения см. в статье [Введение в раскадровки](~/ios/user-interface/storyboards/unified-storyboards.md) .

## <a name="setting-appearance-properties"></a>Задание свойств внешнего вида

На первом экране класс статического внешнего вида используется для стиля кнопок и желтых/оранжевый элементов следующим образом:

```csharp
// Set the default appearance values
UIButton.Appearance.TintColor = UIColor.LightGray;
UIButton.Appearance.SetTitleColor(UIColor.FromRGB(0,127,14), UIControlState.Normal);

UISlider.Appearance.ThumbTintColor = UIColor.Red;
UISlider.Appearance.MinimumTrackTintColor = UIColor.Orange;
UISlider.Appearance.MaximumTrackTintColor = UIColor.Yellow;

UIProgressView.Appearance.ProgressTintColor = UIColor.Yellow;
UIProgressView.Appearance.TrackTintColor = UIColor.Orange;
```

Стили зеленого элемента задаются подобным образом в методе `ViewDidLoad`, который переопределяет значения по умолчанию и статический класс *внешнего вида* :

```csharp
slider2.ThumbTintColor = UIColor.FromRGB (0,127,70); // dark green
slider2.MinimumTrackTintColor = UIColor.FromRGB (66,255,63);
slider2.MaximumTrackTintColor = UIColor.FromRGB (197,255,132);
```

```csharp
progress2.ProgressTintColor = UIColor.FromRGB (66,255,63);
progress2.TrackTintColor = UIColor.FromRGB (197,255,132);
```

## <a name="using-uiappearance-in-xamarinforms"></a>Использование Уиаппеаранце в Xamarin. Forms

API внешнего вида можно использовать при [оформлении стиля приложения iOS](~/xamarin-forms/platform/ios/formatting.md#uiappearance) в решениях Xamarin. Forms. Несколько строк в классе `AppDelegate` могут помочь реализовать определенную цветовую схему без необходимости создания [пользовательского модуля подготовки](~/xamarin-forms/app-fundamentals/custom-renderer/index.md)отчетов.

### <a name="custom-themes-and-uiappearance"></a>Пользовательские темы и Уиаппеаранце

iOS позволяет многим визуальным атрибутам элементов управления пользовательского интерфейса быть «темой» с помощью интерфейсов API *уиаппеаранце* , чтобы заставить все экземпляры определенного элемента управления иметь одинаковый внешний вид. Он предоставляется как свойство Appearance для многих классов элементов управления пользовательского интерфейса, а не для отдельных экземпляров элемента управления. Установка свойства дисплея для свойства static `Appearance` влияет на все элементы управления этого типа в приложении.

Чтобы лучше понять концепцию, рассмотрим пример.

Чтобы изменить конкретный `UISegmentedControl` для пурпурного оттенка, мы будем ссылаться на конкретный элемент управления на нашем экране следующим образом `ViewDidLoad`:

```csharp
sg1.TintColor = UIColor.Magenta;
```

Также можно задать значение на панели свойств конструктора:

[![](introduction-to-the-appearance-api-images/propertiespadtint.png "Properties Pad Tint")](introduction-to-the-appearance-api-images/propertiespadtint.png#lightbox)

На рисунке ниже показано, что этот параметр задает оттенок только для элемента управления с именем "SG1".

[![](introduction-to-the-appearance-api-images/image53.png "Setting the individual control tint")](introduction-to-the-appearance-api-images/image53.png#lightbox)

Установка многих элементов управления таким способом будет полностью неэффективной, поэтому мы можем задать статическое свойство `Appearance` для самого класса. Это показано в следующем коде:

```csharp
UISegmentedControl.Appearance.TintColor = UIColor.Magenta;
```

На приведенном ниже рисунке показаны как сегментированные элементы управления, для которых установлен стиль «пурпурный»:

[![](introduction-to-the-appearance-api-images/image54.png "Setting the Appearance control tint")](introduction-to-the-appearance-api-images/image54.png#lightbox)

`Appearance` свойства должны быть установлены на раннем этапе жизненного цикла приложения, например в событии `FinishedLaunching` AppDelegate или в ViewController до отображения затронутых элементов управления.

Более подробные сведения см. в статье [Введение в интерфейс API внешнего вида](~/ios/user-interface/ios-ui/introduction-to-the-appearance-api.md) .

## <a name="related-links"></a>Связанные ссылки

- [Внешний вид (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/appearance)
- [Справочник по протоколу Уиаппеаранце](https://developer.apple.com/library/ios/documentation/UIKit/Reference/UIAppearance_Protocol/)
- [Внешний вид в Xamarin. Forms](~/xamarin-forms/platform/ios/formatting.md#uiappearance)
