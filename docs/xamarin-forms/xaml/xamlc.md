---
title: Компиляция XAML в Xamarin.Forms
description: В этой статье объясняется, как XAML можно при необходимости компилируется непосредственно в промежуточный язык (IL) с помощью компилятора Xamarin.Forms XAML (XAMLC).
ms.prod: xamarin
ms.assetid: 9A2D10A6-5DFC-485F-A75A-2F7B98314025
ms.technology: xamarin-forms
author: charlespetzold
ms.author: chape
ms.date: 08/22/2018
ms.openlocfilehash: de1ac47a56bf8d75eefb2ed6c7237f2f63f56ecc
ms.sourcegitcommit: e268fd44422d0bbc7c944a678e2cc633a0493122
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/25/2018
ms.locfileid: "50105952"
---
# <a name="xaml-compilation-in-xamarinforms"></a>Компиляция XAML в Xamarin.Forms

_Можно при необходимости скомпилировать XAML напрямую в промежуточный язык (IL) с помощью компилятора XAML (XAMLC)._

Компиляция XAML предлагает ряд преимуществ:

- Он проверяет XAML во время компиляции, уведомляя пользователя об ошибках.
- Он сокращает часть времени загрузки и создания элементов XAML.
- Он позволяет сократить размер файла окончательной сборки, так как больше не добавляет XAML-файлы.

Компиляция XAML отключен по умолчанию для обеспечения обратной совместимости. Его можно включить на уровне класса и сборки, добавив [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) атрибута.

В следующем примере кода показано включение компиляции XAML на уровне сборки:

```csharp
using Xamarin.Forms.Xaml;
...
[assembly: XamlCompilation (XamlCompilationOptions.Compile)]
namespace PhotoApp
{
  ...
}
```

В этом примере проверку всех XAML, содержатся внутри сборки во время компиляции будет выполняться, с ошибками XAML, сообщаемый во время компиляции, а не во время выполнения. Таким образом `assembly` префикс для [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) атрибут указывает, что атрибут применяется ко всей сборке.

> [!NOTE]
> [ `XamlCompilation` ](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute) Атрибут и [ `XamlCompilationOptions` ](xref:Xamarin.Forms.Xaml.XamlCompilationOptions) перечисления находятся в `Xamarin.Forms.Xaml` пространство имен, которое необходимо импортировать их использования.

В следующем примере кода показано включение компиляции XAML на уровне класса:

```csharp
using Xamarin.Forms.Xaml;
...
[XamlCompilation (XamlCompilationOptions.Compile)]
public class HomePage : ContentPage
{
  ...
}
```

В этом примере XAML для проверки во время компиляции `HomePage` класс будет выполнена, и выводятся сообщения об ошибках в ходе процесса компиляции.

> [!NOTE]
> Скомпилированный привязки можно включить для повышения производительности привязки данных в приложениях Xamarin.Forms. Дополнительные сведения см. в разделе [компиляции привязки](~/xamarin-forms/app-fundamentals/data-binding/compiled-bindings.md).

## <a name="related-links"></a>Связанные ссылки

- [`XamlCompilation`](xref:Xamarin.Forms.Xaml.XamlCompilationAttribute)
- [`XamlCompilationOptions`](xref:Xamarin.Forms.Xaml.XamlCompilationOptions)
