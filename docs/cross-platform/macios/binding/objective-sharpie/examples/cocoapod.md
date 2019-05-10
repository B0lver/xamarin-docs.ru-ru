---
title: Реальный пример, с помощью CocoaPods
description: В этом документе показано, как использовать Sharpie цели для автоматического создания C# определений из cocoapod для службы привязки.
ms.prod: xamarin
ms.assetid: 233B781D-5841-4250-9F63-0585231D2112
author: asb3993
ms.author: amburns
ms.date: 03/28/2018
ms.openlocfilehash: 266f2bca1f4a96242b17080e60c9f43cb956a5fd
ms.sourcegitcommit: bf18425f97b48661ab6b775195eac76b356eeba0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/01/2019
ms.locfileid: "64977873"
---
# <a name="real-world-example-using-cocoapods"></a>Реальный пример, с помощью CocoaPods

> [!NOTE]
> В этом примере используется [использовать AFNetworking CocoaPod](https://cocoapods.org/pods/AFNetworking).

Новое в версии 3.0, поддерживает привязки CocoaPods Sharpie цели и даже включает команду (`sharpie pod`) для облегчения загрузки, Настройка и создание CocoaPods очень просто. Вы должны [ознакомиться с CocoaPods](https://cocoapods.org) в целом перед использованием этой функции.

## <a name="creating-a-binding-for-a-cocoapod"></a>Создание привязки для cocoapod для службы

`sharpie pod` Команда имеет один глобальный параметр и двух команд:

```bash
$ sharpie pod -help
usage: sharpie pod [OPTIONS] COMMAND [COMMAND_OPTIONS]

Pod Options:
  -d, -dir DIR     Use DIR as the CocoaPods binding directory,
                   defaulting to the current directory

Available Commands:
  init         Initialize a new Xamarin C# CocoaPods binding project
  bind         Bind an existing Xamarin C# CocoaPods project
```

`init` Подкоманда также содержит некоторые полезные справки:

```bash
$ sharpie pod init -help
usage: sharpie pod init [INIT_OPTIONS] TARGET_SDK POD_SPEC_NAMES

Init Options:
  -f, -force       Initialize a new Podfile and run actions against
                   it even if one already exists
```

Несколько имен cocoapod для службы и subspec имена можно указать для `init`.

```bash
$ sharpie pod init ios AFNetworking
** Setting up CocoaPods master repo ...
   (this may take a while the first time)
** Searching for requested CocoaPods ...
** Working directory:
**   - Writing Podfile ...
**   - Installing CocoaPods ...
**     (running `pod install --no-integrate --no-repo-update`)
Analyzing dependencies
Downloading dependencies
Installing AFNetworking (2.6.0)
Generating Pods project
Sending stats
** 🍻 Success! You can now use other `sharpie podn`  commands.
```

После настройки cocoapod для вашей службы теперь можно создать привязки:

```bash
$ sharpie pod bind
```

Это приведет к в проекте Xcode cocoapod для службы сборки и затем вычисляется и анализируются с целью Sharpie. Большой объем вывода консоли будет создан, но должно привести к определению привязки, в конце:

```bash
(... lots of build output ...)

Parsing 19 header files...

Binding...
  [write] ApiDefinitions.cs
  [write] StructsAndEnums.cs

Done.
```

## <a name="next-steps"></a>Следующие шаги

После создания **ApiDefinitions.cs** и **StructsAndEnums.cs** файлы, рассмотрим следующую документацию для создания сборки для использования в приложениях:

- [Общие сведения о привязке Objective-C](~/cross-platform/macios/binding/overview.md)
- [Привязка библиотек Objective-C](~/cross-platform/macios/binding/objective-c-libraries.md)
- [Пошаговое руководство: Привязка библиотек Objective-C iOS](~/ios/platform/binding-objective-c/walkthrough.md)
