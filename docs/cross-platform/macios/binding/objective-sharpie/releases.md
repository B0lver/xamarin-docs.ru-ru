---
title: Журнал выпусков цели Шарпие
description: В этом документе описывается история выпусков целевого Шарпиеа, используемая для автоматизации создания C# привязок в коде цели-C.
ms.prod: xamarin
ms.assetid: 1F4A1BE1-7205-43F4-89D0-6C8672F52598
author: davidortinau
ms.author: daortin
ms.date: 10/11/2017
ms.openlocfilehash: f60be3f7dc14749f5cd58d5228c17fa85282cd78
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725354"
---
# <a name="objective-sharpie-release-history"></a>Журнал выпусков цели Шарпие

## <a name="34-october-11-2017"></a>3,4 (11 октября 2017 г.)

[Скачать 3.4.0 v](https://dl.xamarin.com/objective-sharpie/ObjectiveSharpie-3.4.0.pkg)

* Поддержка Xcode 9: iOS 11, macOS 10,13, tvOS 11 и watchOS 4
* Теперь должны быть исправлены проблемы, связанные с SIMD и tgmath
* Данные телеметрии полностью удалены

## <a name="33-august-3-2016"></a>3,3 (3 августа 2016 г.)

[Скачать 3.3.0 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.3.0.pkg)

* Поддержка Xcode 8 Beta 4, iOS 10, macOS 10,12, tvOS 10 и watchOS 3.
* Обновлено до последней версии главной сборки Clang (2016-08-02)
* [Сохранить параметры отправки телеметрии](https://twitter.com/Symbiatch/status/760373403878559744) из `sharpie pod bind` в `sharpie bind`.

## <a name="32-june-14-2016"></a>3,2 (14 июня 2016 г.)

[Скачать 3.2.3 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.2.3.pkg)

* Поддержка Xcode 8 Beta 1, iOS 10, macOS 10,12, tvOS 10 и watchOS 3.

## <a name="31-may-31-2016"></a>3,1 (31 мая 2016 г.)

[Скачать 3.1.1 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.1.1.pkg)

* Поддержка CocoaPods 1,0
* Повышение надежности привязки CocoaPods путем создания собственного `.framework` и последующей привязки
* Скопируйте `.framework` CocoaPods и определение привязки в каталог `Binding`, чтобы упростить интеграцию с проектами привязки Xamarin. iOS и Xamarin. Mac

## <a name="30-october-5-2015"></a>3,0 (5 октября, 2015)

[Скачать 3.0.8 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-3.0.8.pkg)

* Поддержка новых возможностей языка C, включая упрощенные универсальные шаблоны и допустимость значений NULL, как представлено в Xcode 7
* Поддержка последних пакетов SDK для iOS и Mac.
* Поддержка создания и анализа проекта Xcode! Теперь вы можете передать полный проект Xcode в цель Шарпие, и он будет лучше понять, что нужно делать (например, `sharpie bind Project.xcodeproj -sdk ios`).
* Поддержка [CocoaPods](https://cocoapods.org) ! Теперь вы можете настраивать, собирать и привязывать CocoaPods непосредственно из цели Шарпие (например, `sharpie pod init ios AFNetworking && sharpie pod bind`).
* При использовании платформ привязки модули Clang будут включены, если платформа поддерживает их, что приведет к правильному анализу платформы, так как структура платформы определяется ее `module.modulemap`.
* Для проектов Xcode, которые создают продукт платформы, проанализируйте этот продукт, а не промежуточные целевые объекты продукта как целевые объекты, не являющиеся платформой, в проекте Xcode могут по-прежнему иметь неоднозначности, которые невозможно разрешить автоматически.

## <a name="216-march-17-2015"></a>2.1.6 (17 марта, 2015)

* Фиксированная привязка выражения бинарного оператора: левая часть выражения неправильно переключена вправо (например, `1 << 0` была неправильно привязана как `0 << 1`). Благодарим за это, Kemp Адам!
* Исправлена проблема с `NSInteger` и `NSUInteger` быть привязана как `int` и `uint` вместо `nint` и `nuint` на i386; `-DNS_BUILD_32_LIKE_64` теперь передается в Clang, чтобы выполнить синтаксический анализ `objc/NSObjCRuntime.h` правильно работать в i386.
* Архитектура по умолчанию для пакетов SDK для Mac OS X (например, `-sdk macosx10.10`) теперь x86_64 вместо i386, поэтому `-arch` можно опустить, если не требуется переопределение значения по умолчанию.

## <a name="210-march-15-2015"></a>2.1.0 (15 марта, 2015)

[Скачать 2.1.0 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.1.0.pkg)

* [бкск # 27849](https://bugzilla.xamarin.com/show_bug.cgi?id=27849): Убедитесь, что `using ObjCRuntime;` создается при использовании `ArgumentSemantic`.
* [бкск # 27850](https://bugzilla.xamarin.com/show_bug.cgi?id=27850): Убедитесь, что `using System.Runtime.InteropServices;` создается при использовании `DllImport`.
* [бкск # 27852](https://bugzilla.xamarin.com/show_bug.cgi?id=27852): `DllImport` по умолчанию для загрузки символов из `__Internal`.
* [бкск # 27848](https://bugzilla.xamarin.com/show_bug.cgi?id=27848): Skip onобъявленные объявления контейнеров цели-C.
* [бкск # 27846](https://bugzilla.xamarin.com/show_bug.cgi?id=27846): Связывание типов протоколов с одной квалификацией как с конкретными интерфейсами (`id<Foo>` как `Foo` вместо `Foundation.NSObject<Foo>`).
* [бкск # 28037](https://bugzilla.xamarin.com/show_bug.cgi?id=28037): привяжите `UInt32`, `UInt64`и `Int64` литералы как `Int32`, чтобы удалить `u` и/или `uL` суффиксы, если значения могут безопасно соответствовать `Int32`.
* [бкск # 28038](https://bugzilla.xamarin.com/show_bug.cgi?id=28038): исправление сопоставления имен перечислений, если исходное собственное имя начинается с префикса `k`.
* `sizeof` выражения C, тип аргумента которых не соответствует C# примитивному типу, будет вычислен в Clang и привязан как целочисленный литерал, чтобы избежать создания C#недопустимого.
* Исправление синтаксиса цели-C для свойств, тип которых является блоком (код цели-C отображается в комментариях над объявлениями с ограничениями).
* Привяжите типы распадалось к исходному типу (`int[]` декайс `int*` во время семантического анализа в Clang, но привяжите его как исходный `int[]`).

Благодарим Дейв Дункин о многочисленных ошибках, исправленных в этом выпуске.

## <a name="200-march-9-2015"></a>2.0.0:9 марта 2015 г.

[Скачать 2.0.0 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-2.0.0.pkg)

Цель Шарпие 2,0 — это основной выпуск, который содержит Улучшенный драйвер на основе Clang и средство синтаксического анализа и новый механизм привязки на основе Нрефактори. Эти улучшенные компоненты предоставляют _более_ эффективные привязки, в особенности привязка типов. Были сделаны многие другие улучшения, которые являются внутренними для целевых Шарпие, которые выдают множество видимых пользователю функций в будущих выпусках.

Цель Шарпие 2,0 основана на Clang 3.6.1.

### <a name="type-binding-improvements"></a>Улучшения привязки типов

* Теперь поддерживаются блоки цели-C. Это включает в себя анонимные/встраиваемые блоки и блоки с именами с помощью `typedef`. Анонимные блоки будут привязаны как `System.Action` или `System.Func` делегаты, тогда как именованные блоки будут привязаны как строго именованные типы `delegate`.

* Существует улучшенный эвристический подход к именованию для анонимных перечислений, которым непосредственно предшествует `typedef` разрешение встроенного целочисленного типа, такого как `long` или `int`.

* Теперь указатели C привязаны C# `unsafe` указателями вместо `System.IntPtr`. Это приводит к большей ясности в привязке, когда может потребоваться включить параметры указателя в `out` или `ref` параметры. Невозможно всегда определять, должен ли параметр быть `out` или `ref`, поэтому указатель сохраняется в привязке, чтобы обеспечить более простой аудит.

* Исключением из приведенной выше привязки указателя является то, что в качестве параметра обнаружен 2-позиционный указатель на объект цели-C. В таких случаях соглашение является предопределенным и параметр будет привязан как `out` (например, `NSError **error` → `out NSError error`).

### <a name="verify-attribute"></a>Проверить атрибут

Часто вы обнаружите, что привязки, созданные с помощью цели Шарпие, теперь будут помечены атрибутом `[Verify]`. Эти атрибуты указывают на то, что необходимо _убедиться_ , что цель шарпиеа правильно, сравнив привязку с исходным объявлением c/объектив-C (которое будет предоставлено в комментарии над объявлением привязки).

_Рекомендуется использовать_ проверку для всех привязанных объявлений, но, скорее всего, _требуется_ для объявлений, помеченных атрибутом `[Verify]`. Это связано с тем, что во многих ситуациях недостаточно метаданных в исходном исходном коде, чтобы определить, как лучше всего создавать привязку. Вам может потребоваться сослаться на документацию или комментарии к коду в файлах заголовков, чтобы обеспечить наилучшее решение для привязки.

Дополнительные сведения см. в документации по [проверке атрибутов](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md) .

### <a name="other-notable-improvements"></a>Другие важные улучшения

* `using` операторы теперь создаются на основе привязанных типов. Например, если `NSURL` был привязан, будет также создана `using Foundation;` инструкция.

* объявления `struct` и `union` теперь будут привязаны, используя `[FieldOffset]`ный прием для объединений.

* Значения перечисления с инициализаторами константных выражений теперь будут правильно привязаны. полное выражение преобразуется в C#.

* Методы и блоки Variadic теперь привязаны.

* Платформы теперь поддерживаются с помощью параметра `-framework`. Дополнительные сведения см. в документации по [привязке собственных платформ](~/cross-platform/macios/binding/objective-sharpie/index.md) .

* Исходный код цели-C будет автоматически обнаружен, что следует исключить необходимость передачи `-ObjC` или `-xobjective-c` в Clang вручную.

* Использование модуля Clang (`@import`) теперь определяется автоматически, что исключает необходимость вручную передавать `-fmodules` Clang для библиотек, использующих новую поддержку модуля в Clang.

* Теперь Xamarin Unified API является целевым объектом привязки по умолчанию. Используйте параметр `-classic` для назначения только 32-разрядных Classic API.

### <a name="notable-bug-fixes"></a>Исправления важных ошибок

* Исправление привязки `instancetype` при использовании в категории "цель-C"
* Полные имена в категориях "цель-C"
* Префиксы протокола цели-C `I` (например, `INSCopying` вместо `NSCopying`)

## <a name="1135-december-21-2014"></a>1.1.35:21 декабря 2014 г.

[Скачать 1.1.35 v](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.35.pkg)

Исправлены незначительные ошибки.

## <a name="111-december-15-2014"></a>1.1.1:15 декабря 2014 г.

[Загрузите версию 1.1.1](https://download.xamarin.com/objective-sharpie/ObjectiveSharpie-1.1.1.pkg)

Версия 1.1.1 была первым основным выпуском после 1,5 лет внутреннего использования и разработки в Xamarin после первоначальной предварительной версии Шарпие в апреле 2013. Этот выпуск является первым, который обычно считается стабильным и пригодным для использования в самых разных собственных библиотеках, в котором используется новая серверная часть Clang.
