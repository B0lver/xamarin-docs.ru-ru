---
title: Реальный пример, с помощью проекта Xcode
description: В этом документе описывается, как для использования в качестве прямого ввода для Sharpie цели, что упрощает процесс создания проекта Xcode C# привязки для кода Objective-C.
ms.prod: xamarin
ms.assetid: 168AA64C-E181-4937-A1F2-AD095B9A36F2
author: asb3993
ms.author: amburns
ms.date: 01/15/2016
ms.openlocfilehash: 05c55dc7cd20de2d216d1f267ea5a73631748a0a
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61265261"
---
# <a name="real-world-example-using-an-xcode-project"></a>Реальный пример, с помощью проекта Xcode

**В этом примере используется [POP библиотеки из Facebook](https://github.com/facebook/pop).**

Новое в версии 3.0, цель Sharpie поддерживает проекты Xcode как входные данные. Эти проекты указать правильный заголовок файлы и флаги компилятора, необходимые для компиляции собственной библиотеки и таким образом, необходимо привязать его слишком. Цели Sharpie выберет первый _целевой_ и задана конфигурация по умолчанию проекта, если не выдал вам иные инструкции.

Прежде чем Sharpie цели пытается выполнить синтаксический анализ файлов проекта и заголовок, его необходимо построить. Проекты часто имеют этапов построения, которые правильно структурировать файлы заголовков для внешнего использования и интеграции, поэтому она лучше всегда собирался весь проект, прежде чем привязать его.

<pre>$ <b>git clone https://github.com/facebook/pop.git</b>
Cloning into 'pop'...
   <em>(more git clone output)</em>

$ <b>cd pop</b>
$ <b>sharpie bind pop.xcodeproj -sdk iphoneos9.0</b></pre>

## <a name="related-links"></a>Связанные ссылки

- [Курс Xamarin University. Создание библиотеки привязки Objective-C](https://university.xamarin.com/classes/track/all#building-an-objective-c-bindings-library)
- [Курс Xamarin University. Создание библиотеки привязки Objective-C с помощью цели Sharpie](https://university.xamarin.com/classes/track/all#build-an-objective-c-bindings-library-with-objective-sharpie)