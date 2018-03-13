---
title: "Подписи"
ms.topic: article
ms.prod: xamarin
ms.assetid: 54DA1221-13E4-4D45-B263-5F22A0AC7B53
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 07/11/2017
ms.openlocfilehash: 0fdeecc4465aa5709b452ef0b591ec4e5c262e3d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="labels"></a>Подписи

`UILabel` Управления используется для отображения одного или нескольких строк читать только текст. 

## <a name="implementing-a-label"></a>Реализация метки

Новая метка создается путем создания экземпляра [ `UILabel` ](https://developer.xamarin.com/api/type/UIKit.UILabel/):

```csharp
UILabel label = new UILabel();
```

### <a name="labels-and-storyboards"></a>Метки и раскадровки

Также можно добавить метку к пользовательскому Интерфейсу, при использовании конструктора iOS. Поиск **метка** в **элементов** и перетащите его к представлению:

![Метки в панели элементов](labels-images/image3.png)

На панели свойств можно изменить следующие свойства:

![Панель свойств метки](labels-images/image2.png)

- **Контекст текст** — обычный или атрибутами. Обычный текст позволяет установить [атрибуты форматирования](#Formatting_Text_and_Label) на всю строку. Тексты атрибутами можно задавать разные символы или слова в строке форматирования.
- **Цвет шрифта, выравнивания** — атрибуты форматирования, применимый к метке.
- **Строки** — задает число строк, которые могут занимать метку. Задайте значение 0, чтобы разрешить метку, используемую столько строк.
- **Поведение** — можно присвоить значение Enabled или выделенная. Enabled имеет значение по умолчанию отключенного текста будет отображаться более светлым серым цветом. Выделено отключен по умолчанию и позволяет перерисовки с выделенной состояние при выборе пользователем.
- **Baselane и символ новой строки** — 
    - Определяет базовые показатели, как текст будет положение, если размеры шрифтов, отличается от указанного.
    - Разрывы строк определяют, как строка будет в оболочку или усечена, если она больше чем одну строку.
- **Автоматическое сжатие** — определяет, как будет минимальным размером шрифта в метке, при необходимости.
- **Выделено, тень, смещение** — позволяет задать цвет выделенной и тени, и смещение тени.

## <a name="truncating-and-wrapping"></a>Усечение и упаковки

Сведения об использовании в строке прерывает выполнение операций ввода-вывода, описаны в [усечение и переноса текста](https://developer.xamarin.com/recipes/ios/standard_controls/labels/uilabel-truncate-wrap-text/) инструкций.

<a name="Formatting_Text_and_Label"/>

## <a name="formatting-text-and-label"></a>Форматирование текста и метки

Чтобы отформатировать строку, можно использовать в метке можно либо задать атрибуты на всю строку форматирования или атрибутами строки можно использовать. В следующих примерах реализовать их:

```csharp
label = new UILabel(){
                Text = "Hello, this is a string",
                Font = UIFont.FromName("Papyrus", 20f),
                TextColor = UIColor.Magenta,
                TextAlignment = UITextAlignment.Center
            };
```

```csharp
label.AttributedText = new NSAttributedString(
                "This is some formatted text",
                font: UIFont.FromName("GillSans", 16.0f),
                foregroundColor: UIColor.Blue,
                backgroundColor: UIColor.White
            );
```

Дополнительные сведения о стилях текста с помощью `NSAttributedString` посвящены [стиль текста](https://developer.xamarin.com/recipes/ios/standard_controls/text_field/style_text/) инструкций.

По умолчанию имеют метки `Enabled` установлено значение true, но его можно установить отключается для предоставления пользователю, указание, что определенный элемент управления отключен:

```csharp
label.Enabled = false;
```

Это задает метку для светлой серым цветом, как показано на следующем рисунке примере экрана ограничения в iOS:

![Неактивная кнопка с iOS](labels-images/image1.png)

Можно также задать цвета текста выделения и тени в тексте метки для дополнительные эффекты:

```csharp
label.Highlighted = true;
label.HighlightedTextColor = UIColor.Cyan;

label.ShadowColor = UIColor.Black;
label.ShadowOffset = new CoreGraphics.CGSize(1.0f, 1.0f);
```

Что отображается текст следующим образом:

![Выделение цветом и тень набор из текста](labels-images/image4.png)

Дополнительные сведения об изменении шрифта UILabel посвящены [изменить шрифт](https://developer.xamarin.com/recipes/ios/standard_controls/labels/change_the_font/) инструкций.





