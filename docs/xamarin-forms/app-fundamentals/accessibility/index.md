---
title: Специальные возможности в Xamarin.Forms
description: Если вы создаете приложение со специальными возможностями, им смогут пользоваться люди с различными потребностями.
ms.prod: xamarin
ms.assetid: 99B8A8E8-6F5E-46BC-9639-1C4A6D301049
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 10/15/2018
ms.openlocfilehash: 20ea72e588e2e3b1d575bd593446bf6724d73d8c
ms.sourcegitcommit: 482aef652bdaa440561252b6a1a1c0a40583cd32
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/21/2019
ms.locfileid: "65971059"
---
# <a name="xamarinforms-accessibility"></a>Специальные возможности в Xamarin.Forms

_Если вы создаете приложение со специальными возможностями, им смогут пользоваться люди с различными потребностями._

Специальные возможности в приложении Xamarin.Forms требуют обдумывания макета и дизайна многих элементов пользовательского интерфейса. Рекомендации по аспектам, которые нужно учесть, см. в разделе [Контрольный список для специальных возможностей](~/cross-platform/app-fundamentals/accessibility.md). Многие вопросы реализации специальных возможностей, например крупный шрифт и подходящие настройки цвета и контрастности, уже решаются API-интерфейсами в Xamarin.Forms.

Руководства по [специальным возможностям в Android](~/android/app-fundamentals/accessibility.md) и [специальным возможностям в iOS](~/ios/app-fundamentals/accessibility.md) содержат сведения о собственных API, предоставляемых Xamarin, а [руководство по специальным возможностям универсальной платформы Windows на сайте MSDN](https://msdn.microsoft.com/windows/uwp/accessibility/basic-accessibility-information) объясняет собственный подход на этой платформе. Эти API используются для полной реализации приложений со специальными возможностями на каждой платформе.

Xamarin.Forms в данный момент не имеет *встроенной* поддержки всех API специальных возможностей, доступных на каждом из базовых платформ. Тем не менее он поддерживает задание свойств автоматизации в элементах пользовательского интерфейса для поддержки средств чтения с экрана и помощи в навигации, а это самые важные компоненты создания приложений со специальными возможностями. Дополнительные сведения см. в разделе [Свойства автоматизации](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md).

В приложениях Xamarin.Forms можно указать последовательность табуляции для элементов управления, чтобы сделать приложение более удобным и доступным. Дополнительные сведения см. в разделе [Специальные возможности клавиатуры](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md).

Другие API специальных возможностей (например, [PostNotification в iOS](~/ios/app-fundamentals/accessibility.md)) лучше подходят для реализации [`DependencyService`](~/xamarin-forms/app-fundamentals/dependency-service/index.md) или [пользовательского отрисовщика](~/xamarin-forms/app-fundamentals/custom-renderer/index.md). Они не рассматриваются в данном руководстве.

## <a name="testing-accessibility"></a>Тестирование специальных возможностей

Приложения Xamarin.Forms обычно предназначены для нескольких платформ, а значит тестирование функций специальных возможностей необходимо проводить на конкретных платформах. Перейдите по следующим ссылкам и узнайте, как протестировать специальные возможности на каждой платформе:

- [**Тестирование в iOS**](~/ios/app-fundamentals/accessibility.md)
- [**Тестирование в Android**](~/android/app-fundamentals/accessibility.md)
- [**Windows AccScope (MSDN)** ](https://msdn.microsoft.com/library/windows/desktop/dn433239)

## <a name="related-links"></a>Связанные ссылки

- [Кросс-платформенные специальные возможности](~/cross-platform/app-fundamentals/accessibility.md)
- [Свойства автоматизации](~/xamarin-forms/app-fundamentals/accessibility/automation-properties.md)
- [Специальные возможности клавиатуры](~/xamarin-forms/app-fundamentals/accessibility/keyboard.md)
