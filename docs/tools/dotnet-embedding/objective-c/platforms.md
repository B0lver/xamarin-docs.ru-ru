---
title: Платформы Objective c.
ms.prod: xamarin
ms.assetid: 43253BE4-A03A-4646-9A14-32C05174E672
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 11/14/2017
ms.openlocfilehash: 7dc4dc4ea0ab55ff603b9e8d6fa905336159222a
ms.sourcegitcommit: 775a7d1cbf04090eb75d0f822df57b8d8cff0c63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="objective-c-platforms"></a>Платформы Objective c.


Внедрение .NET можно ориентироваться на различные платформы при создании кода Objective-C:

* macOS
* iOS
* tvOS
* watchOS [еще не реализован]

Выбрана платформа, передав `--platform=<platform>` embeddinator аргумента командной строки.

При построении для платформ iOS, tvOS и watchOS embeddinator всегда будет создаваться платформу, которая содержит Xamarin.iOS, поскольку Xamarin.iOS содержит много среды выполнения поддерживает код, который необходим для этих платформ.

Однако при построении macOS платформы, можно выбрать, следует ли созданный framework внедрять Xamarin.Mac или нет. Можно отменить внедрение Xamarin.Mac Если привязанной сборки ссылается Xamarin.Mac.dll (прямо или косвенно), и этот параметр выбран, передав `--platform=macOS` для embeddinator.

Если привязанной сборки содержит ссылку на Xamarin.Mac.dll, необходимо внедрить Xamarin.Mac и дополнительно embeddinator необходимо знать, какие требуемая версия .NET framework для использования.

Существует три возможных целевые платформы Xamarin.Mac: `modern` (прежнее название — `mobile`), `full` и `system` (разница между каждым описан в Xamarin.Mac [требуемой версии .NET framework] [ 1] документации), и каждый выбирается путем передачи `--platform=macOS-modern`, `--platform=macOS-full` или `--platform=macOS-system` для embeddinator.

[1]: ~/mac/platform/target-framework.md
