---
title: Файлы ApiDefinitions & Структсанденумс
description: В этом документе описаны файлы ApiDefinitions.cs и StructsAndEnums.cs, которые создает целевое Шарпие. Эти файлы затем используются для доступа к коду цели-C из C#.
ms.prod: xamarin
ms.assetid: AC2087C0-BA54-46D8-B70C-6972941C8F73
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 8d4a05745d8d2ec6e05abd519aef4b9827655e06
ms.sourcegitcommit: 008bcbd37b6c96a7be2baf0633d066931d41f61a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/22/2020
ms.locfileid: "86930433"
---
# <a name="apidefinitions--structsandenums-files"></a>Файлы ApiDefinitions & Структсанденумс

При успешном выполнении цели Шарпие создает `Binding/ApiDefinitions.cs` `Binding/StructsAndEnums.cs` файлы и.
Эти два файла добавляются в проект привязки в Visual Studio для Mac или передаются непосредственно в `btouch` `bmac` средства или для создания окончательной привязки.

В *некоторых* случаях эти файлы могут быть достаточно необходимыми, но чаще разработчику потребуется вручную изменить эти созданные файлы, чтобы устранить проблемы, которые не удалось автоматически обработать с помощью средства (например, помеченного [ `Verify` атрибутом](~/cross-platform/macios/binding/objective-sharpie/platform/verify.md)).

Ниже приведены некоторые из следующих шагов.

- **Настройка имен**. иногда требуется изменить имена методов и классов в соответствии с рекомендациями по проектированию .NET Framework.
- **Методы или свойства**: эвристика, используемая целью Шарпие, иногда выбирает метод, который будет включен в свойство. На этом этапе можно решить, является ли это поведение предполагаемым.
- **Подключение событий**. Вы можете связать классы с классами делегатов и автоматически создавать события для них.
- **Подключение уведомлений**. невозможно извлечь контракт API уведомлений из чистых файлов заголовков. Это потребует обращения к документации по API. Если вам нужны строго типизированные уведомления, необходимо обновить результат.
- **API контроль**. на этом этапе вы можете предоставить дополнительные конструкторы, добавить методы (чтобы разрешить синтаксис C# при инициализации), перегрузить оператор и реализовать собственные интерфейсы в файле дополнительных определений.

Сведения о том, как эти файлы помещаются в процесс привязки, см [. на](~/cross-platform/macios/binding/objective-c-libraries.md) приведенной ниже схеме.

![Процесс привязки показан на этой схеме](apidefinitions-structsandenums-images/binding-flowchart.png)

Дополнительные сведения о содержимом этих файлов см. в [справочнике по типам привязки](~/cross-platform/macios/binding/binding-types-reference.md) .
