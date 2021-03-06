---
title: Задание стиля приложений Xamarin.Forms с помощью стилей XAML
description: В этом руководство объясняется, как настроить внешний вид Xamarin.Forms приложения с помощью стилей XAML.
ms.prod: xamarin
ms.assetid: 344A34AA-B19A-4765-BC8A-875D9A6B5EA8
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 01/30/2019
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 72effe15d3456b5a48cbf5d09e889600134ac686
ms.sourcegitcommit: 32d2476a5f9016baa231b7471c88c1d4ccc08eb8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2020
ms.locfileid: "84903094"
---
# <a name="styling-xamarinforms-apps-using-xaml-styles"></a>Задание стиля приложений Xamarin.Forms с помощью стилей XAML

## <a name="introduction"></a>[Введение](introduction.md)

Xamarin.Formsприложения часто содержат несколько элементов управления, имеющих одинаковый внешний вид. Настройка внешнего вида каждого отдельного элемента управления может быть повторена и подвержена ошибкам. Вместо этого можно создать стили, которые настраивают внешний вид элемента управления, группируя и устанавливая свойства, доступные в типе элемента управления.

## <a name="explicit-styles"></a>[Явные стили](explicit.md)

*Явный* стиль — это тот, который выборочно применяется к элементам управления путем задания их [`Style`](xref:Xamarin.Forms.NavigableElement.Style) свойств.

## <a name="implicit-styles"></a>[Неявные стили](implicit.md)

*Неявный* стиль — это тот, который используется всеми элементами управления [`TargetType`](xref:Xamarin.Forms.Style.TargetType) , не требуя, чтобы каждый элемент управления ссылался на стиль.

## <a name="global-styles"></a>[Глобальные стили](application.md)

Стили можно сделать доступными глобально, добавив их в приложение [`ResourceDictionary`](xref:Xamarin.Forms.ResourceDictionary) . Это помогает избежать дублирования стилей на страницах или в элементах управления.

## <a name="style-inheritance"></a>[Наследование стиля](inheritance.md)

Стили могут наследоваться от других стилей для сокращения дублирования и включения повторного использования.

## <a name="dynamic-styles"></a>[Динамические стили](dynamic.md)

Стили не реагируют на изменения свойств и остаются неизменными в течение всего приложения. Однако приложения могут динамически реагировать на изменения стиля во время выполнения с помощью динамических ресурсов.

## <a name="device-styles"></a>[Стили устройств](device.md)

Xamarin.Formsвключает шесть *динамических* стилей, известных как стили *устройств* , в [`Devices.Styles`](xref:Xamarin.Forms.Device.Styles) классе. Все шесть стилей можно применять только к [`Label`](xref:Xamarin.Forms.Label) экземплярам.

## <a name="style-classes"></a>[Классы стилей](style-class.md)

Xamarin.Formsклассы стилей позволяют применять к элементу управления несколько стилей, не прибегая к наследованию стиля.
