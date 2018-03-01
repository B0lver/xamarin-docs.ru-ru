---
title: "Используя поставщик содержимого"
ms.topic: article
ms.prod: xamarin
ms.assetid: 251F7557-328D-0132-F39D-595920A28B87
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/06/2018
ms.openlocfilehash: 5c90b140719e0dafb36dad1da75f52ba27e75e25
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="using-a-contentprovider"></a>Используя поставщик содержимого

CursorAdapters может также использоваться для отображения данных из поставщик содержимого.
ContentProviders позволяют получить доступ к данным, доступных в других приложениях (включая данные системы Android как контакты, мультимедиа и календаря).

Это предпочтительный способ доступа к поставщик содержимого — с помощью LoaderManager CursorLoader. LoaderManager впервые появился в Android 3.0 (уровень API 11, Honeycomb) для перемещения блокирующих задачах вне основного потока, а использование CursorLoader позволяет загружаться в потоке до, привязанной к ListView для отображения данных.

Ссылаться на [введение в ContentProviders](~/android/platform/content-providers/index.md) для получения дополнительной информации.

