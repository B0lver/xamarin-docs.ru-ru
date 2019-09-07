---
title: Microsoft Azure мобильные приложения
description: В этом документе содержатся ссылки на руководства, в которых описывается создание приложения Xamarin, подключенного к Azure. В нем обсуждается работа с компонентом Xamarin Azure, пользователями и Push-уведомлениями.
ms.prod: xamarin
ms.assetid: 7B9AA8D9-C181-4C33-8AB0-2F56E4DBFC03
author: conceptdev
ms.author: crdun
ms.date: 04/02/2017
ms.openlocfilehash: b94bae8fb1b7c990c5b2478a0da143960a0bcc55
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70765966"
---
# <a name="microsoft-azure-mobile-apps"></a>Microsoft Azure мобильные приложения

_Примеры и загрузка кода для портал Azureной документации._

<!--
NOTE TO AUTHORS: this page is referenced from
https://azure.microsoft.com/develop/mobile/xamarin/
as https://developer xamarin com/guides/cross-platform/data-cloud/mobile-services/
A redirect has been put in place to /mobile-apps/ HOWEVER the /Resources/ .ZIP files are still located in /mobile-services/ so that the following permalinks don't break

The ZIPs in /Resources/ are also referenced by inbound links
Getting Started http://go.microsoft.com/fwlink/p/?LinkId=331359
Get started with data http://go.microsoft.com/fwlink/p/?LinkId=331302
Get started with push http://go.microsoft.com/fwlink/p/?LinkId=331303
Get started with authentication http://go.microsoft.com/fwlink/p/?LinkId=331328
Get started with Notification Hubs http://go.microsoft.com/fwlink/p/?LinkId=331329
Validate and modify data  http://go.microsoft.com/fwlink/p/?LinkId=331330
-->

Эти ссылки предназначены для документации по Xamarin, доступной на веб-сайте [мобильных приложений Azure](https://docs.microsoft.com/azure/app-service-mobile/) .
Добавление функций Azure в приложение Xamarin путем скачивания [мобильного клиента Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/).

## <a name="working-with-the-xamarin-azure-component"></a>Работа с компонентом Xamarin Azure

Общая документация по [работе с клиентской библиотекой Xamarin (компонентом)](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-dotnet-how-to-use-client-library) для выполнения различных задач с помощью мобильных приложений Azure. На этой странице содержится множество примеров фрагментов кода без подробного объяснения и примеров, доступных в каждой из пошаговых руководств, перечисленных ниже.

## <a name="getting-started"></a>Начало работы

В этой статье содержатся пошаговые инструкции по созданию и запуску первого приложения Xamarin Azure.
Здесь рассматривается создание нового мобильного приложения Azure на портале, а затем Загрузка и запуск предварительно настроенного приложения.

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started/)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started/)
- [Xamarin.Forms](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-forms-get-started)

<!--
## Validate, Modify and Augment Data in Scripts

Demonstrates how to add server-side scripts to Azure Mobile Services data tables to implement server-side validation and other functionality.

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#errors)
-->

<!--
## Add Paging to Data

A quick example of paging large sets of data using Skip() and Take().

- [iOS](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
- [Android](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-how-to-use-client-library/#paging)
-->

## <a name="get-started-with-users"></a>Начало работы с пользователями

Содержит полные инструкции по настройке и кодированию экрана входа с помощью мобильных служб Azure. Поддерживаемые поставщики проверки подлинности включают в себя Microsoft, Google, Facebook и Twitter.

- [iOS](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-ios-get-started-users/)
- [Android](https://azure.microsoft.com/documentation/articles/app-service-mobile-xamarin-android-get-started-users/)

## <a name="authorize-users-in-scripts"></a>Авторизация пользователей в скриптах

Пример кода для интерфейсов JavaScript

- [TODO. js](https://github.com/Azure/azure-mobile-apps-node/blob/master/samples/personal-table/tables/TodoItem.js#L38)

## <a name="get-started-with-push"></a>Начало работы с Push-уведомлениями

Выполните инструкции по настройке push-уведомлений на сайтах Apple и Google, а затем отправьте push-уведомление из мобильных служб Azure на устройство.

- [iOS](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-ios-get-started-push)
- [Android](https://docs.microsoft.com/azure/app-service-mobile/app-service-mobile-xamarin-android-get-started-push)

## <a name="get-started-with-notification-hubs"></a>Приступая к работе с центрами уведомлений

Выполните инструкции по настройке push-уведомлений на сайтах Apple и Google, настройте центр уведомлений Azure, а затем создайте push-уведомления на устройствах.

- [iOS](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-ios-push-notification-apns-get-started)
- [Android](https://docs.microsoft.com/azure/notification-hubs/xamarin-notification-hubs-push-notifications-android-gcm)

## <a name="related-links"></a>Связанные ссылки

- [GettingStarted (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GettingStarted)
- [GetStartedWithData (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithData)
- [Жетстартедвисусерс (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithUsers)
- [Жетстартедвиспуш (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/GetStartedWithPush)
- [NotificationHubs (пример)](https://github.com/xamarin/mobile-samples/tree/master/Azure/NotificationHubs)
- [Мобильный клиент Azure](https://www.nuget.org/packages/Microsoft.Azure.Mobile.Client/)
- [Схема обучения мобильных приложений Azure](https://azure.microsoft.com/documentation/learning-paths/appservice-mobileapps/)

<!--
- [ValidateModifyData (sample)](https://github.com/xamarin/mobile-samples/tree/master/Azure/ValidateModifyData)
-->
