---
title: "2D рисования"
description: "CROSS платформы 2D Рисование с SkiaSharp"
ms.topic: article
ms.prod: xamarin
ms.assetid: A8A61421-4544-422A-A7E0-9355C67DF21E
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 09/14/2017
ms.openlocfilehash: 03089e4760ebf19849cd4d34cafb7047d8915a4d
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="2d-drawing"></a>2D рисования

SkiaSharp предоставляет возможности API C# это двумерной графики. Питание включено [библиотеки Skia Google](http://skia.org), той же библиотеке, лежащих в основе графических стеки Google Chrome, Firefox и Android.

[ ![](images/ide-sml.png "SkiaSharp предоставляет возможности API C# это двумерной графики")](images/ide.png)

SkiaSharp является переносимой библиотеки и удобно поставляется как [пакет NuGet кросс платформенных](https://www.nuget.org/packages/SkiaSharp)и поддерживает следующие платформы готового: macOS, Xamarin.Android и Xamarin.iOS, а также рабочий стол Windows.

## <a name="introduction-to-skiasharpgraphics-gamesskiasharpintroductionmd"></a>[Общие сведения о SkiaSharp](~/graphics-games/skiasharp/introduction.md)

Обзор основных понятий SkiaSharp и образец кода для отрисовки графики, текст, растровые изображения и использовать фильтры образа.

## <a name="skiasharp-tutorials-for-xamarinformsxamarin-formsuser-interfacegraphicsskiasharpindexmd"></a>[Учебники по SkiaSharp для Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/index.md)

Описание способов работы с кросс-графики платформы, подготовки к просмотру в Xamarin.Forms:

- [Основы рисования](~/xamarin-forms/user-interface/graphics/skiasharp/basics/index.md)
  * [Рисование простых круга](~/xamarin-forms/user-interface/graphics/skiasharp/basics/circle.md)
  * [Интеграция с помощью Xamarin.Forms](~/xamarin-forms/user-interface/graphics/skiasharp/basics/integration.md)
  * [Пикселей и аппаратно независимых единицах](~/xamarin-forms/user-interface/graphics/skiasharp/basics/pixels.md)
  * [Базовая анимация](~/xamarin-forms/user-interface/graphics/skiasharp/basics/animation.md)
  * [Интеграция текста и графики](~/xamarin-forms/user-interface/graphics/skiasharp/basics/text.md)
  * [Основы растрового изображения](~/xamarin-forms/user-interface/graphics/skiasharp/basics/bitmaps.md)
- [Строки и пути](~/xamarin-forms/user-interface/graphics/skiasharp/paths/index.md)
  * [Строки и пунктирные завершения отрезков](~/xamarin-forms/user-interface/graphics/skiasharp/paths/lines.md)
  * [Основные сведения о пути](~/xamarin-forms/user-interface/graphics/skiasharp/paths/paths.md)
  * [Типы заполнения путь](~/xamarin-forms/user-interface/graphics/skiasharp/paths/fill-types.md)
  * [Ломаных линий и параметрических уравнений](~/xamarin-forms/user-interface/graphics/skiasharp/paths/polylines.md)
  * [Точек и тире](~/xamarin-forms/user-interface/graphics/skiasharp/paths/dots.md)
  * [Рисование пальцем](~/xamarin-forms/user-interface/graphics/skiasharp/paths/finger-paint.md)
- [Преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/index.md)
  * [Преобразования для преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/translate.md)
  * [Преобразование изменения масштаба](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/scale.md)
  * [Преобразования вращения](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/rotate.md)
  * [Наклон преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/skew.md)
  * [Матрица преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/matrix.md)
  * [Манипуляции сенсорного ввода](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/touch.md)
  * [Сходные без преобразования](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/non-affine.md)
  * [Трехмерного поворота](~/xamarin-forms/user-interface/graphics/skiasharp/transforms/3d-rotation.md)
- [Кривых и контуров](~/xamarin-forms/user-interface/graphics/skiasharp/curves/index.md)
  * [Три способа, чтобы нарисовать дугу](~/xamarin-forms/user-interface/graphics/skiasharp/curves/arcs.md)
  * [Три типа кривых Безье](~/xamarin-forms/user-interface/graphics/skiasharp/curves/beziers.md)
  * [Путь данных SVG](~/xamarin-forms/user-interface/graphics/skiasharp/curves/path-data.md)
  * [Обрезка с использованием пути и областей](~/xamarin-forms/user-interface/graphics/skiasharp/curves/clipping.md)
  * [Путь эффекты](~/xamarin-forms/user-interface/graphics/skiasharp/curves/effects.md)
  * [Путей и текста](~/xamarin-forms/user-interface/graphics/skiasharp/curves/text-paths.md)
  * [Сведения о пути и перечисления](~/xamarin-forms/user-interface/graphics/skiasharp/curves/information.md)

## <a name="platform-specific-notesgraphics-gamesskiasharpplatformmd"></a>[Определенные заметки о платформе](~/graphics-games/skiasharp/platform.md)

На этой странице описаны инструкции по SkiaSharp установке на различных платформах, включая iOS, Android, macOS и Windows.

## <a name="api-documentationhttpsdeveloperxamarincomapinamespaceskiasharp"></a>[Документация по API](https://developer.xamarin.com/api/namespace/SkiaSharp/)

Можно просмотреть [документации по API](https://developer.xamarin.com/api/namespace/SkiaSharp/) для SkiaSharp на сайте.

## <a name="work-in-progress"></a>Выполняемая работа

SkiaSharp является незавершенным, мы совместное использование с нашего сообщества. Хотя привязку важные части Skia API, объем работы остается сделать. Мы используем стабильный API C, отображаются Skia, и мы планируем будет продолжаться, добавляющие нашей работе для привязок C Skia раскрывается полностью API-интерфейсам.

Чтобы помочь нам руководства наши усилия привязки, оставьте комментарии и предложения как проблемы в репозитории GitHub [http://github.com/mono/SkiaSharp](http://github.com/mono/SkiaSharp).
