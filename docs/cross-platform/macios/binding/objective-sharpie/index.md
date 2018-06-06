---
title: Создание привязок с целевой Sharpie
description: В этом разделе содержатся вводные сведения о цели Sharpie, Xamarin средство командной строки, используемые для автоматизации процесса создания привязки в библиотеку Objective-C
ms.prod: xamarin
ms.assetid: 9C0A932C-7601-4357-B3F7-62ABAC835019
author: asb3993
ms.author: amburns
ms.date: 10/11/2017
ms.openlocfilehash: ae644038aa8b54f0d57b61767882dec8754040c8
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34780761"
---
# <a name="creating-bindings-with-objective-sharpie"></a>Создание привязок с целевой Sharpie

_В этом разделе содержатся вводные сведения о цели Sharpie, Xamarin средство командной строки, используемые для автоматизации процесса создания привязки в библиотеку Objective-C_

- [Общие сведения о](#overview) & [журнала](#history)
- [Начало работы](get-started.md)
- [Средства и команды](tools.md)
- [Функции](platform/index.md)
- [Примеры](examples/index.md)
- [Полное Пошаговое руководство](~/ios/platform/binding-objective-c/walkthrough.md)
- [История выпусков](releases.md)

## <a name="overview"></a>Обзор

Цели Sharpie — это средство командной строки для начальной загрузки на первом этапе привязки.
Он работает путем синтаксического анализа заголовочные файлы для сопоставления открытого API-интерфейса в собственной библиотеки [определения привязки](~/cross-platform/macios/binding/objective-c-libraries.md#The_API_definition_file) (этот процесс, ранее вручную выполненной).

Цели Sharpie использует Clang анализировать файлы заголовка, поэтому привязки как точные и подробный. Это может значительно сократить время и усилия, требуемые для создания привязки качества.

> [!IMPORTANT]
> Цели Sharpie — это средство для опытных разработчиков Xamarin, обладающие углубленными знаниями Objective-C (и по расширению, C). Прежде чем привязать цель библиотеки необходимо четко понимать, как выполнить сборку собственной библиотеки в командной строке (и полного понимания того, как работает собственной библиотеки).

## <a name="history"></a>Журнал

Мы были развивается и с помощью Sharpie цель внутренним образом в Xamarin за последние три года. Благодаря возможности Sharpie цель API-интерфейсы появившиеся в Xamarin.iOS и Xamarin.Mac начиная с iOS 8, Mac OS X 10.10 и watchOS 2.0 был загружен, полностью с целью Sharpie. Xamarin во многом зависит от цели Sharpie внутренним образом для создания собственных продуктов.

Однако цель Sharpie является очень сложных средство, которое требует дополнительных знаний о Objective-C и C, как использовать в командной строке компилятора clang и объединить обычно как собственные библиотеки. Из-за высокого уровня полосы, мы полагаем, что с графическим интерфейсом пользователя мастер устанавливает неправильный ожидания, и таким образом, Sharpie цель в данный момент доступна только как средство командной строки.

## <a name="related-links"></a>Связанные ссылки

- [Цели Sharpie загрузки](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie.pkg)
- [Пошаговое руководство: Привязка библиотеку Objective c.](~/ios/platform/binding-objective-c/walkthrough.md)
- [Привязка библиотек Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Сведения о привязке](~/cross-platform/macios/binding/overview.md)
- [Привязки типов Справочник](~/cross-platform/macios/binding/binding-types-reference.md)
- [Xamarin для разработчиков Objective-C](~/ios/get-started/objective-c-developers/index.md)
