---
title: Средства & задания Шарпие команды
description: В этом документе представлен обзор средств, входящих в состав цели Шарпие, и аргументов командной строки для их использования.
ms.prod: xamarin
ms.assetid: A84E209B-8932-4CC1-BAD1-7FD51F798A97
author: davidortinau
ms.author: daortin
ms.date: 10/05/2015
ms.openlocfilehash: 2179154aa6dc78a8b0b6b418d780e7996f8e557d
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015928"
---
# <a name="objective-sharpie-tools--commands"></a>Средства & задания Шарпие команды

_Общие сведения о средствах, входящих в состав цели Шарпие, и аргументы командной строки для их использования._

После успешной [установки](~/cross-platform/macios/binding/objective-sharpie/get-started.md)цели Шарпие откройте терминал и ознакомьтесь с *командами* цели Шарпие, которые должны предложить:

```
$ sharpie -help
usage: sharpie [OPTIONS] TOOL [TOOL_OPTIONS]

Options:
  -h, -help                Show detailed help
  -v, -version             Show version information

Telemetry Options:
  -tlm-about               Show a detailed overview of what usage and binding
                             information will be submitted to Xamarin by
                             default when using Objective Sharpie.
  -tlm-do-not-submit       Do not submit any usage or binding information to
                             Xamarin. Run 'sharpie -tml-about' for more
                             information.
  -tlm-do-not-identify     Do not submit Xamarin account information when
                             submitting usage or binding information to Xamarin
                             for analysis. Binding attempts and usage data will
                             be submitted anonymously if this option is
                             specified.

Available Tools:
  xcode              Get information about Xcode installations and available SDKs.
  pod                Create a Xamarin C# binding to Objective-C CocoaPods
  bind               Create a Xamarin C# binding to Objective-C APIs
  update             Update to the latest release of Objective Sharpie
  verify-docs        Show cross reference documentation for [Verify] attributes
  docs               Open the Objective Sharpie online documentation
```

Целевое Шарпие предоставляет следующие средства:

|Средство|Описание|
|--- |--- |
|**Xcode**|Содержит сведения о текущей установке Xcode и доступных версиях пакетов SDK для iOS и Mac. Эти сведения будут использоваться позже при формировании привязок.|
|**модуль**|Выполняет поиск, настраивает, устанавливает (в локальном каталоге) и привязывает библиотеки цели-C [кокоапод](https://cocoapods.org/) , доступные в репозитории основных спецификаций. Это средство оценивает установленный Кокоапод для автоматического вывода правильных входных данных для передачи в средство `bind` ниже. Новое в 3,0!|
|**bind**|Выполняет синтаксический анализ файлов заголовков (`*.h`) в библиотеке цели-C в начальных файлах [ApiDefinition.cs и StructsAndEnums.CS](~/cross-platform/macios/binding/objective-sharpie/platform/apidefinitions-structsandenums.md) .|
|**update**|Проверяет наличие новых версий цели Шарпие и загружает и запускает установщик, если он доступен.|
|**Проверка-документация**|Отображает подробные сведения об атрибутах `[Verify]`.|
|**Документы**|Переход к этому документу в веб-браузере по умолчанию.|

Чтобы получить справку по определенному целевому инструменту Шарпие, введите имя средства и параметр `-help`. Например, `sharpie xcode -help` возвращает следующие выходные данные:

```
$ sharpie xcode -help
usage: sharpie xcode [OPTIONS]

Options:
  -h, -help        Show detailed help
  -v, -verbose     Be verbose with output

Xcode Options:
  -sdks            List all available Xcode SDKs. Pass -verbose for more details.
```

Прежде чем можно будет начать процесс привязки, необходимо получить сведения о текущих установленных пакетах SDK, введя следующую команду в `sharpie xcode -sdks`терминалов. Выходные данные могут отличаться в зависимости от того, какие версии Xcode установлены. Цель Шарпие ищет пакеты SDK, установленные в любом `Xcode*.app` в каталоге `/Applications`:

```
$ sharpie xcode -sdks
sdk: appletvos9.0    arch: arm64
sdk: iphoneos9.1     arch: arm64   armv7
sdk: iphoneos9.0     arch: arm64   armv7
sdk: iphoneos8.4     arch: arm64   armv7
sdk: macosx10.11     arch: x86_64  i386
sdk: macosx10.10     arch: x86_64  i386
sdk: watchos2.0      arch: armv7
```

На приведенном выше примере мы можем увидеть, что на нашем компьютере установлен пакет SDK `iphoneos9.1` и `arm64`а поддержка архитектуры. Мы будем использовать это значение для всех примеров в этом разделе. После этой информации мы готовы к анализу файлов заголовков библиотек цели-C в начальном `ApiDefinition.cs` и `StructsAndEnums.cs` для проекта привязки.
