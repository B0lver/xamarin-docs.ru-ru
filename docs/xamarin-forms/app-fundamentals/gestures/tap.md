---
title: Добавление распознавателя жестов касания
description: В этой статье объясняется, как использовать жест касания для распознавания касания в приложении Xamarin.Forms. Распознавание касания реализовано с помощью класса TapGestureRecognizer.
ms.prod: xamarin
ms.assetid: 1D150BAF-4157-49BC-90A0-153323B8EBCF
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/21/2016
ms.openlocfilehash: e2e5a71d7cddee66612c12bc6091c57426b3ed47
ms.sourcegitcommit: be6f6a8f77679bb9675077ed25b5d2c753580b74
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/07/2018
ms.locfileid: "53059964"
---
# <a name="adding-a-tap-gesture-recognizer"></a>Добавление распознавателя жестов касания

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)

_Жест касания используется для распознавания касания и реализован с помощью класса TapGestureRecognizer._

Чтобы элемент пользовательского интерфейса можно было выбирать жестом касания, необходимо создать экземпляр [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer), реализовать обработку события [`Tapped`](xref:Xamarin.Forms.TapGestureRecognizer.Tapped) и добавить новый распознаватель жестов в коллекцию [`GestureRecognizers`](xref:Xamarin.Forms.View.GestureRecognizers) элемента пользовательского интерфейса. В следующем примере кода показан распознаватель `TapGestureRecognizer`, присоединенный к элементу [`Image`](xref:Xamarin.Forms.Image):

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.Tapped += (s, e) => {
    // handle the tap
};
image.GestureRecognizers.Add(tapGestureRecognizer);
```

По умолчанию изображение будет реагировать на одиночные касания. Для реагирования на двойные (или множественные) касания задайте свойство [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired).

```csharp
tapGestureRecognizer.NumberOfTapsRequired = 2; // double-tap
```

Если свойство [`NumberOfTapsRequired`](xref:Xamarin.Forms.TapGestureRecognizer.NumberOfTapsRequired) имеет значение больше единицы, обработчик событий выполняется только при том условии, что касания были совершены в течение определенного периода времени (настроить его нельзя). Если второе или последующие касания не были совершены в течение этого периода, они игнорируются и счетчик касаний сбрасывается.

<a name="Using_Xaml" />

## <a name="using-xaml"></a>Использование XAML

Распознаватель жестов можно добавить к элементу управления в XAML с помощью присоединенных свойств. Синтаксис для добавления [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) к изображению показан ниже (в этом случае определяется событие *двойного касания*).

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
                Tapped="OnTapGestureRecognizerTapped"
                NumberOfTapsRequired="2" />
  </Image.GestureRecognizers>
</Image>
```

Код обработчика событий (в этом примере) увеличивает значение счетчика и преобразовывает цветное изображение в черно-белое.

```csharp
void OnTapGestureRecognizerTapped(object sender, EventArgs args)
{
    tapCount++;
    var imageSender = (Image)sender;
    // watch the monkey go from color to black&white!
    if (tapCount % 2 == 0) {
        imageSender.Source = "tapped.jpg";
    } else {
        imageSender.Source = "tapped_bw.jpg";
    }
}
```

## <a name="using-icommand"></a>Использование интерфейса ICommand

Приложения на основе шаблона "модель — представление — модель представления" (MVVM) обычно используют интерфейс `ICommand` вместо подключения обработчиков событий напрямую. Распознаватель [`TapGestureRecognizer`](xref:Xamarin.Forms.TapGestureRecognizer) поддерживает `ICommand` путем настройки привязки в коде:

```csharp
var tapGestureRecognizer = new TapGestureRecognizer();
tapGestureRecognizer.SetBinding (TapGestureRecognizer.CommandProperty, "TapCommand");
image.GestureRecognizers.Add(tapGestureRecognizer);
```

или путем использования XAML:

```xaml
<Image Source="tapped.jpg">
    <Image.GestureRecognizers>
        <TapGestureRecognizer
            Command="{Binding TapCommand}"
            CommandParameter="Image1" />
    </Image.GestureRecognizers>
</Image>
```

Полный код для этой модели представления можно найти в примере. Ниже представлена соответствующая реализация `Command`.

```csharp
public class TapViewModel : INotifyPropertyChanged
{
    int taps = 0;
    ICommand tapCommand;
    public TapViewModel () {
        // configure the TapCommand with a method
        tapCommand = new Command (OnTapped);
    }
    public ICommand TapCommand {
        get { return tapCommand; }
    }
    void OnTapped (object s)  {
        taps++;
        Debug.WriteLine ("parameter: " + s);
    }
    //region INotifyPropertyChanged code omitted
}
```


## <a name="related-links"></a>Связанные ссылки

- [TapGesture (пример)](https://developer.xamarin.com/samples/xamarin-forms/WorkingWithGestures/TapGesture/)
- [GestureRecognizer](xref:Xamarin.Forms.GestureRecognizer)
- [TapGestureRecognizer](xref:Xamarin.Forms.TapGestureRecognizer)
