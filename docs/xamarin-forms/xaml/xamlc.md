---
title: Компиляция XAML
description: При необходимости можно воспользоваться компилятором XAML (XAMLC) и скомпилировать XAML напрямую в промежуточный язык (IL).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 01/21/2016
ms.openlocfilehash: fc4c7df6011fdf8d263b9fca88a5ffb551ec78e3
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="xaml-compilation"></a>Компиляция XAML

_XAML можно скомпилировать непосредственно в промежуточный язык (IL), с помощью компилятора XAML (XAMLC) при необходимости._

Компилятор XAMLC предлагает ряд преимуществ, которые перечислены далее:

- Он проверяет XAML во время компиляции, уведомляя пользователя об ошибках.
- Он сокращает часть времени загрузки и создания элементов XAML.
- Он позволяет сократить размер файла окончательной сборки, так как больше не добавляет XAML-файлы.

XAMLC отключен по умолчанию для обеспечения обратной совместимости. Включить на уровне сборки и класса, добавив `XamlCompilation` атрибута.

В следующем примере кода показано включение XAMLC на уровне сборки:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

В этом примере все XAML-кода проверки во время компиляции внутри `PhotoApp` выполняется пространства имен, с ошибками XAML, сообщаемый во время компиляции, а не во время выполнения.
`assembly` Префикс для `XamlCompilation` атрибут указывает, что атрибут применяется ко всей сборке.

В следующем примере кода показано включение XAMLC на уровне класса:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

В этом примере проверка кода XAML для компиляции `HomePage` класса будет выполняться, и выводятся сообщения об ошибках в процессе компиляции.

> [!NOTE]
> `XamlCompilation` Атрибута и `XamlCompilationOptions` перечисления находятся в `Xamarin.Forms.Xaml` пространства имен, которое необходимо импортировать их использования.


## <a name="related-links"></a>Связанные ссылки

- [XamlCompilation](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationAttribute/)
- [XamlCompilationOptions](https://developer.xamarin.com/api/type/Xamarin.Forms.Xaml.XamlCompilationOptions/)
