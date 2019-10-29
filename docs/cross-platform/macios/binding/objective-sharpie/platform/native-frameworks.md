---
title: Привязка собственных платформ
description: В этом документе описывается использование параметра целевой платформы Шарпие для создания привязки к библиотеке, распространяемой в качестве платформы.
ms.prod: xamarin
ms.assetid: 91AE058A-3A1F-41A9-9DE4-4B96880A1869
author: davidortinau
ms.author: daortin
ms.date: 01/15/2016
ms.openlocfilehash: 78c489518833705432610e83453c3c04bf1cca53
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016094"
---
# <a name="binding-native-frameworks"></a>Привязка собственных платформ

Иногда собственная библиотека распространяется как [платформа](https://developer.apple.com/library/mac/documentation/MacOSX/Conceptual/BPFrameworks/Concepts/WhatAreFrameworks.html). Цель Шарпие предоставляет удобную функцию для привязки правильно определенных платформ с помощью параметра `-framework`.

Например, привязка [платформы Adobe Creative SDK](https://creativesdk.adobe.com/downloads.html) для iOS проста:

```
$ sharpie bind \
    -framework AdobeCreativeSDKFoundation.framework \
    -sdk iphoneos8.1
```

В некоторых случаях платформа будет указывать **info. plist** , указывающий, в каком пакете SDK должна быть скомпилирована платформа. Если эта информация существует и ни один параметр `-sdk` не передан, Целевая Шарпие будет вычислять его из **info. plist** (ключ `DTSDKName` или сочетание `DTPlatformName` и `DTPlatformVersion`ных ключей).

Параметр `-framework` не допускает передачи явных файлов заголовков. Файл заголовка "тег" выбирается по соглашению на основе имени платформы. Если не удается найти заголовок управляющего объекта, Целевая Шарпие не будет пытаться привязать платформу, и необходимо вручную выполнить привязку, предоставив правильные файлы заголовков (-ов) для синтаксического анализа, а также аргументы платформы для Clang (например, `-F` Параметр пути поиска платформы).

В этом случае указание `-framework` является просто ярлыком. Следующие аргументы привязки идентичны `-framework` краткости.
Особая важность — это `-F .` путь поиска платформы, предоставленный Clang (Обратите внимание на пробел и период, которые требуются как часть команды).

```
$ sharpie bind \
    -sdk iphoneos8.1 \
    AdobeCreativeSDKFoundation.framework/Headers/AdobeCreativeSDKFoundation.h \
    -scope AdobeCreativeSDKFoundation.framework/Headers \
    -c -F .
```
