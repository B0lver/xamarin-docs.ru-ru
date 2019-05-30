---
title: Навигация в Xamarin.Forms
description: В этом руководстве содержатся сведения о переходах в приложениях Xamarin.Forms. Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы.
ms.prod: xamarin
ms.assetid: BC5D0C6C-D5A9-4B12-A492-ED1F570CEC87
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/01/2017
ms.openlocfilehash: f67ab15466da118d12c280d597972d2d11f8e600
ms.sourcegitcommit: 6ad272c2c7b0c3c30e375ad17ce6296ac1ce72b2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66178113"
---
# <a name="xamarinforms-navigation"></a>Навигация в Xamarin.Forms

_Xamarin.Forms предоставляет ряд различных способов перехода по страницам в зависимости от используемого типа страницы._

![](images/page-types.png "Типы страниц в Xamarin.Forms")

## <a name="hierarchical-navigationhierarchicalmd"></a>[Иерархическая навигация](hierarchical.md)

Класс [`NavigationPage`](xref:Xamarin.Forms.NavigationPage) обеспечивает иерархическую навигацию, при которой пользователь может переходить по страницам вперед и назад по своему желанию. Этот класс реализует навигацию на основе стека объектов [`Page`](xref:Xamarin.Forms.Page) по методу LIFO (последним поступил — первым обслужен).

## <a name="tabbedpagetabbed-pagemd"></a>[TabbedPage](tabbed-page.md)

Xamarin.Forms [`TabbedPage`](xref:Xamarin.Forms.TabbedPage) состоит из списка вкладок и большой области сведений, где каждая вкладка загружает содержимое в область сведений.

## <a name="carouselpagecarousel-pagemd"></a>[CarouselPage](carousel-page.md)

Xamarin.Forms [`CarouselPage`](xref:Xamarin.Forms.CarouselPage) — это страница, по которой пользователи могут проводить из стороны в сторону, чтобы переходить по страницам содержимого, например по страницам коллекции.

## <a name="masterdetailpagemaster-detail-pagemd"></a>[MasterDetailPage](master-detail-page.md)

Xamarin.Forms [`MasterDetailPage`](xref:Xamarin.Forms.MasterDetailPage) представляет собой страницу, управляющую двумя страницами связанных данных — главной страницей, которая представляет элементы, и страницей сведений, которая представляет сведения об элементах на главной странице.

## <a name="modal-pagesmodalmd"></a>[Модальные страницы](modal.md)

Xamarin.Forms также поддерживает модальные страницы. На модальной странице пользователь должен выполнить отдельную задачу, причем он не может уйти с этой страницы, пока задача не будет выполнена или отменена.
