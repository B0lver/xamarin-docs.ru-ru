---
title: Касание и жесты в Xamarin. Android
description: 'Сенсорные экраны на многих современных устройствах позволяют пользователям быстро и эффективно взаимодействовать с устройствами естественным и интуитивно понятным способом. Это взаимодействие не ограничивается простостью простого обнаружения сенсорных устройств. также можно использовать жесты. Например, жест сжатия для масштабирования — это очень распространенный пример: сжатие части экрана с двумя пальцами, которые пользователь может увеличить или уменьшить. В этом руководством рассматриваются сенсорные и жесты в Android.'
ms.prod: xamarin
ms.assetid: 61874769-978A-4562-9B2A-7FFD45F58B38
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/16/2018
ms.openlocfilehash: b4740b91b3d59a3c50696af06eec4ff82bbf9b1e
ms.sourcegitcommit: 4e399f6fa72993b9580d41b93050be935544ffaa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91455200"
---
# <a name="touch-and-gestures-in-xamarinandroid"></a>Касание и жесты в Xamarin. Android

_Сенсорные экраны на многих современных устройствах позволяют пользователям быстро и эффективно взаимодействовать с устройствами естественным и интуитивно понятным способом. Это взаимодействие не ограничивается простостью простого обнаружения сенсорных устройств. также можно использовать жесты. Например, жест сжатия для масштабирования — это очень распространенный пример: сжатие части экрана с двумя пальцами, которые пользователь может увеличить или уменьшить. В этом руководством рассматриваются сенсорные и жесты в Android._

## <a name="touch-overview"></a>Обзор сенсорного ввода

iOS и Android похожи на способы их управления. Оба варианта могут поддерживать несколько точек связи с помощью экранных и сложных жестов. В этом руководством рассматриваются некоторые сходства, а также особенности реализации сенсорного ввода и жестов на обеих платформах.

Android использует `MotionEvent` объект для инкапсуляции сенсорных данных, а методы объекта представления — для прослушивания касаний.

Помимо захвата сенсорных данных, как iOS, так и Android предоставляют средства для интерпретации шаблонов касаний на жесты. Эти распознаватели жестов могут использоваться для интерпретации команд, относящихся к приложению, таких как поворот изображения или включение страницы. Android предоставляет несколько поддерживаемых жестов, а также ресурсы для упрощения добавления сложных пользовательских жестов.

Независимо от того, работаете ли вы с Android или iOS, возможность выбора между касаниями и распознавателями жестов может быть запутанной. В этом пошаговом руководством рекомендуется, чтобы для распознавателей жестов были предоставлены предпочтения. Распознаватели жестов реализуются как дискретные классы, которые обеспечивают более четкое разделение проблем и лучшую инкапсуляцию. Это упрощает совместное использование логики разными представлениями, уменьшая объем написанного кода.

В этом руководство используется аналогичный формат для каждой операционной системы: сначала появились и объясняются API сенсора платформы, так как они основаны на создании сенсорных взаимодействий. Затем мы подробно рассмотрим мир распознавателей жестов — сначала ознакомьтесь с некоторыми общими жестами и создайте собственные жесты для приложений. Наконец, вы узнаете, как отслеживать отдельные пальцы с помощью низкоуровневого отслеживания касаний, чтобы создать программу-заливку пальца.

## <a name="sections"></a>Разделы

- [Сенсорные технологии в Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [Пошаговое руководство. Использование сенсорного ввода в Android](~/android/app-fundamentals/touch/android-touch-walkthrough.md)
- [Отслеживание нескольких касаний](touch-tracking.md)

## <a name="summary"></a>Сводка

В этом пошаговом окне мы рассмотрели касание в Android. В обеих операционных системах мы узнали, как включить сенсорный ввод и как реагировать на события касания. Далее мы узнали о жестах и некоторых распознавателях жестов, предоставляемых Android и iOS для решения некоторых из наиболее распространенных ситуаций. Мы рассмотрели, как создавать пользовательские жесты и реализовывать их в приложениях. В пошаговом руководстве демонстрируются основные понятия и интерфейсы API для каждой операционной системы, а также показано, как отреагировать на них отдельные пальцы.

## <a name="related-links"></a>Связанные ссылки

- [Запуск Android Touch (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-touch-start)
- [Окончательное касание Android (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-touch-final)
- [Финжерпаинт (пример)](/samples/xamarin/monodroid-samples/applicationfundamentals-fingerpaint)