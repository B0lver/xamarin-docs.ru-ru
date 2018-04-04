---
title: Работа с ввода текста
ms.prod: xamarin
ms.assetid: E9CDF1DE-4233-4C39-99A9-C0AA643D314D
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 9dec6f754590abf6db8829f555376b423b7a7da7
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-text-input"></a>Работа с ввода текста

Apple Watch не предоставляет клавиатуры, позволяющих пользователям вводить текст, однако он поддерживает некоторые альтернативные решения friendly Контрольные значения:

- При выборе из списка предварительно определенных параметров текста
- Диктовки Siri
- Выбор emoji
- Scribble распознавания буквы с буквы (появился в watchOS 3).

Имитатор в настоящее время не поддерживает диктовки, но можно по-прежнему проверить ввода текста контроллера другие параметры, например Scribble, как показано ниже:

![](text-input-images/textinput-sml.png "Тестирование параметр scribble")

Чтобы принимать ввод текста в приложении контрольных значений:

1. Создайте массив строк предварительно заданных параметров.
2. Вызовите `PresentTextInputController` с массивом, следует ли разрешить emoji или нет и `Action` , вызываемый при завершении пользователем.
3. В действие завершения для ввода результат проверки и предпринять соответствующие действия в приложении (возможно задание значение текста метки).

Следующий фрагмент кода показывает три предварительно определенных параметров пользователю:

```csharp
var suggest = new string[] {"Get groceries", "Buy gas", "Post letter"};

PresentTextInputController (suggest, WatchKit.WKTextInputMode.AllowEmoji, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
    // this only works if result is a text response (Plain or AllowEmoji)
        enteredText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (enteredText);
        // do something, such as myLabel.SetText(enteredText);
    }
});
```

`WKTextInputMode` Перечисление имеет три значения:

- Обычный
- AllowEmoji
- AllowAnimatedEmoji

## <a name="plain"></a>Обычный

Если режим Обычный пользователь может выбрать:

- Диктовки,
- Scribble, или
- из предварительно определенного списка, передаваемые приложением.

[![](text-input-images/plain-scribble-sml.png "Диктовки Scribble, или из предварительно определенного списка, предоставляющий приложения")](text-input-images/plain-scribble.png#lightbox)

Результат всегда возвращается в виде `NSObject` , может быть приведен к `string`.

## <a name="emoji"></a>Emoji

Существует два вида emoji:

- Регулярные emoji Юникода
- Изображения анимированного

Когда пользователь выбирает emoji Юникода, он возвращается как строка.

Если выбран анимированного изображения emoji `result` выполнения будет содержать обработчик `NSData` , содержащий emoji `UIImage`.

## <a name="accepting-dictation-only"></a>Принимает только диктовки

Перенаправить пользователя непосредственно к экрану диктовки без отображения какие-либо предложения (или параметр Scribble):

- передать пустой массив список предложений и
- Set `WatchKit.WKTextInputMode.Plain`.

```csharp
PresentTextInputController (new string[0], WatchKit.WKTextInputMode.Plain, (result) => {
    // action when the "text input" is complete
    if (result != null && result.Count > 0) {
        dictatedText = result.GetItem<NSObject>(0).ToString();
        Console.WriteLine (dictatedText);
        // do something, such as myLabel.SetText(dictatedText);
    }
});
```

Когда пользователь говоря, на экране Контрольное значение отображается следующий экран, включая текст, его понятное (например «Это тест»):

![](text-input-images/dictation.png "Когда пользователь говоря, окно контрольных значений отображается текст, как понятно")

Как только они клавишу **сделать** возвращается текст кнопки.



## <a name="related-links"></a>Связанные ссылки

- [Apple doc текста и меток](https://developer.apple.com/library/ios/documentation/General/Conceptual/WatchKitProgrammingGuide/TextandLabels.html)
- [Введение в watchOS 3](~/ios/watchos/platform/introduction-to-watchos3/index.md)
