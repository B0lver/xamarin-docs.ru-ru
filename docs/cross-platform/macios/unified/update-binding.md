---
title: Миграция привязки в Unified API
description: В этой статье рассматриваются шаги, необходимые для обновления существующего проекта привязки Xamarin для поддержки унифицированных интерфейсов API для приложений Xamarin. IOS и Xamarin. Mac.
ms.prod: xamarin
ms.assetid: 5E2A3251-D17F-4F9C-9EA0-6321FEBE8577
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: c8f55dd2d300da80a57c06f15cf185558cfc5e41
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015048"
---
# <a name="migrating-a-binding-to-the-unified-api"></a>Миграция привязки в Unified API

_В этой статье рассматриваются шаги, необходимые для обновления существующего проекта привязки Xamarin для поддержки унифицированных интерфейсов API для приложений Xamarin. IOS и Xamarin. Mac._

## <a name="overview"></a>Обзор

Начиная с 1 февраля 2015 Apple требуется, чтобы все новые отправки в iTunes и Mac App Store были 64-разрядными приложениями. В результате все новые приложения Xamarin. iOS или Xamarin. Mac должны использовать новые Unified API вместо существующих классических API-интерфейсов, поддерживающих технологию MonoMac, для поддержки 64-разрядной версии.

Кроме того, любой проект привязки Xamarin также должен поддерживать новые унифицированные API для включения в 64-разрядный проект Xamarin. iOS или Xamarin. Mac. В этой статье рассматриваются шаги, необходимые для обновления существующего проекта привязки с целью использования Unified API.

## <a name="requirements"></a>Требования

Для выполнения действий, описанных в этой статье, необходимо выполнить следующие действия.

- **Visual Studio для Mac** — последняя версия Visual Studio для Mac установлена и настроена на компьютере разработчика.
- **Apple Mac** — Apple Mac требуется для создания проектов привязки для iOS и Mac.

Проекты привязки не поддерживаются в Visual Studio на компьютере Windows.

## <a name="modify-the-using-statements"></a>Изменение инструкций using

Унифицированные API упрощают совместное использование кода между Mac и iOS, а также позволяют поддерживать 32 и 64 разрядных приложений с одним и тем же двоичным файлом. Путем удаления префиксов _MonoMac_ и с _Несенсорного касания_ из пространств имен можно упростить общий доступ для проектов приложений Xamarin. Mac и Xamarin. iOS.

В результате нам потребуется изменить любые контракты привязки (и другие файлы `.cs` в нашем проекте привязки), чтобы удалить префиксы _MonoMac_ и нестандартного _касания_ из наших `using`ных инструкций.

Например, при наличии в контракте привязки следующих операторов using:

```csharp
using System;
using System.Drawing;
using MonoTouch.Foundation;
using MonoTouch.UIKit;
using MonoTouch.ObjCRuntime;
```

Мы бы выставить префикс `MonoTouch`, который приводит к следующим действиям:

```csharp
using System;
using System.Drawing;
using Foundation;
using UIKit;
using ObjCRuntime;
```

Опять же, это необходимо сделать для любого файла `.cs` в нашем проекте привязки. После этого изменения на месте следующим шагом является обновление нашего проекта привязки для использования новых собственных типов данных.

Дополнительные сведения о Unified API см. в документации по [Unified API](~/cross-platform/macios/unified/index.md) . Дополнительную информацию о поддержке 32 и 64 разрядов приложений и сведения о платформах см. в документации по архитектуре [32 и 64](~/cross-platform/macios/32-and-64/index.md) .

## <a name="update-to-native-data-types"></a>Обновление собственных типов данных

Цель-C сопоставляет `NSInteger` тип данных с `int32_t` в 32-разрядных системах и `int64_t` в 64-разрядных системах. Чтобы соответствовать этому поведению, новый Unified API заменяет предыдущее использование `int` (который в .NET определен как всегда `System.Int32`) на новый тип данных: `System.nint`.

Наряду с новым типом данных `nint` Unified API вводит типы `nuint` и `nfloat`, которые также сопоставляются с типами `NSUInteger` и `CGFloat`.

Учитывая приведенный выше, нам нужно изучить наш API и убедиться, что все экземпляры `NSInteger`, `NSUInteger` и `CGFloat`, которые ранее сопоставлены `int`, `uint` и `float` обновлены до новой `nint`типы `nuint` и `nfloat`.

Например, для определения метода цели-C:

```csharp
-(NSInteger) add:(NSInteger)operandUn and:(NSInteger) operandDeux;
```

Если предыдущий контракт привязки имел следующее определение:

```csharp
[Export("add:and:")]
int Add(int operandUn, int operandDeux);
```

Новая привязка будет обновлена следующим образом:

```csharp
[Export("add:and:")]
nint Add(nint operandUn, nint operandDeux);
```

Если мы сопоставлены с более новой версией библиотеки стороннего производителя по сравнению с тем, к чему мы изначально связались, необходимо просмотреть файлы заголовков `.h` для библиотеки и проверить, завершаются ли какие-либо явные вызовы `int`, `int32_t``unsigned int``uint32_t` или `float` были обновлены до `NSInteger`, `NSUInteger` или `CGFloat`. В таком случае те же изменения типов `nint`, `nuint` и `nfloat` должны быть внесены и в их сопоставления.

Дополнительные сведения об этих изменениях типа данных см. в документе [native Types](~/cross-platform/macios/nativetypes.md) .

## <a name="update-the-coregraphics-types"></a>Обновление типов Кореграфикс

Типы данных Point, Size и Rectangle, используемые с `CoreGraphics`, используют биты 32 или 64 в зависимости от устройства, на котором они выполняются. Когда Xamarin изначально привязан к интерфейсам API iOS и Mac, мы использовали существующие структуры данных, которые соответствуют типам данных в `System.Drawing` (например,`RectangleF`).

Из-за требований к поддержке 64 бит и новых собственных типов данных необходимо внести следующие изменения в существующий код при вызове методов `CoreGraphic`:

- **Кгрект** — используйте `CGRect` вместо `RectangleF` при определении прямоугольных областей с плавающей запятой.
- **Кгсизе** — используйте `CGSize` вместо `SizeF` при определении размеров с плавающей запятой (ширина и высота).
- **Кгпоинт** — используйте `CGPoint` вместо `PointF` при определении расположения с плавающей запятой (координаты X и Y).

Учитывая приведенный выше код, необходимо изучить наш API и убедиться, что любой экземпляр `CGRect`, `CGSize` или `CGPoint`, ранее привязанный к `RectangleF`, `SizeF` или `PointF`, был изменен на собственный тип `CGRect`, `CGSize` или `CGPoint` напрямую.

Например, при наличии инициализатора цели-C:

```csharp
- (id)initWithFrame:(CGRect)frame;
```

Если предыдущая привязка включала следующий код:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (RectangleF frame);

```

Мы будем обновлять этот код следующим образом:

```csharp
[Export ("initWithFrame:")]
IntPtr Constructor (CGRect frame);

```

После внесения всех изменений кода необходимо изменить наш проект привязки или создать файл для привязки к унифицированным API.

## <a name="modify-the-binding-project"></a>Изменение проекта привязки

В качестве последнего шага по обновлению нашего проекта привязки для использования унифицированных интерфейсов API необходимо либо изменить `MakeFile`, используемый для сборки проекта, либо тип проекта Xamarin (если привязка выполняется в Visual Studio для Mac) и указать, что _btouch_ следует привязать к Универсальные API, а не классические.

### <a name="updating-a-makefile"></a>Обновление файла MakeFile

Если мы используем Makefile для создания нашего проекта привязки в Xamarin. DLL, необходимо включить параметр командной строки `--new-style` и вызвать `btouch-native` вместо `btouch`.

Таким образом, вы задаете следующие `MakeFile`:

<!--markdownlint-disable MD010 -->
```makefile
BINDDIR=/src/binding
XBUILD=/Applications/Xcode.app/Contents/Developer/usr/bin/xcodebuild
PROJECT_ROOT=XMBindingLibrarySample
PROJECT=$(PROJECT_ROOT)/XMBindingLibrarySample.xcodeproj
TARGET=XMBindingLibrarySample
BTOUCH=/Developer/MonoTouch/usr/bin/btouch

all: XMBindingLibrary.dll

libXMBindingLibrarySample-i386.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphonesimulator -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphonesimulator/lib$(TARGET).a $@

libXMBindingLibrarySample-arm64.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch arm64 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySample-armv7.a:
    $(XBUILD) -project $(PROJECT) -target $(TARGET) -sdk iphoneos -arch armv7 -configuration Release clean build
    -mv $(PROJECT_ROOT)/build/Release-iphoneos/lib$(TARGET).a $@

libXMBindingLibrarySampleUniversal.a: libXMBindingLibrarySample-armv7.a libXMBindingLibrarySample-i386.a libXMBindingLibrarySample-arm64.a
    lipo -create -output $@ $^

XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a

clean:
    -rm -f *.a *.dll
```
<!--markdownlint-enable MD010 -->

Нам нужно переключиться с вызова `btouch` на `btouch-native`, поэтому мы намерены настроить наше определение макроса следующим образом:

```makefile
BTOUCH=/Developer/MonoTouch/usr/bin/btouch-native
```

Мы будем обновлять вызов `btouch` и добавим параметр `--new-style` следующим образом:

<!--markdownlint-disable MD010 -->
```makefile
XMBindingLibrary.dll: AssemblyInfo.cs XMBindingLibrarySample.cs extras.cs libXMBindingLibrarySampleUniversal.a
    $(BTOUCH) -unsafe --new-style -out:$@ XMBindingLibrarySample.cs -x=AssemblyInfo.cs -x=extras.cs --link-with=libXMBindingLibrarySampleUniversal.a,libXMBindingLibrarySampleUniversal.a
```
<!--markdownlint-enable MD010 -->

Теперь мы можем выполнить `MakeFile` в качестве обычного, чтобы создать новую 64-разрядную версию нашего API.

### <a name="updating-a-binding-project-type"></a>Обновление типа проекта привязки

Если для создания API используется шаблон проекта привязки Visual Studio для Mac, необходимо выполнить обновление до новой Unified API версии шаблона проекта привязки. Самый простой способ сделать это — запустить новый проект привязки Unified API и скопировать весь существующий код и параметры.

Выполните следующие действия:

1. Запустите Visual Studio для Mac.
2. Выберите **файл** > **создать** > **решение...**
3. В диалоговом окне новое решение выберите **ios** > **Unified API** > **проекте привязки iOS**: 

    [![](update-binding-images/image01new.png "In the New Solution Dialog Box, select iOS / Unified API / iOS Binding Project")](update-binding-images/image01new.png#lightbox)
4. В диалоговом окне "Настройка нового проекта" введите **имя** нового проекта привязки и нажмите кнопку **ОК** .
5. Включите 64-разрядную версию библиотеки цели-C, для которой вы собираетесь создавать привязки.
6. Скопируйте исходный код из существующего проекта привязки 32 bit Classic API (например, `ApiDefinition.cs` и `StructsAndEnums.cs` Files).
7. Внесите указанные выше изменения в файлы исходного кода.

После внесения всех этих изменений можно создать новую 64-разрядную версию API, как и в случае с 32-разрядной версией.

## <a name="summary"></a>Сводка

В этой статье мы показали изменения, которые необходимо внести в существующий проект привязки Xamarin для поддержки новых унифицированных API-интерфейсов и 64-разрядных устройств, и шаги, необходимые для создания новой версии API, совместимой с 64-разрядной версией.

## <a name="related-links"></a>Связанные ссылки

- [Mac и iOS](~/cross-platform/macios/index.md)
- [Unified API](~/cross-platform/macios/nativetypes.md)
- [32/64. рекомендации по разрядной платформе](~/cross-platform/macios/32-and-64/index.md)
- [Обновление существующих приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Unified API](~/cross-platform/macios/unified/index.md)
- [биндингсампле](https://docs.microsoft.com/samples/xamarin/ios-samples/bindingsample/)
