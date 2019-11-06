---
title: Общие сведения о распространении приложений Xamarin.iOS
description: В этом документе содержатся общие сведения о методах распространения приложений Xamarin.iOS, и приводятся ссылки на более подробные документы по этой теме.
ms.prod: xamarin
ms.assetid: 341D36DB-BB07-FA94-BCC9-5F8C0B18C179
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/18/2017
ms.openlocfilehash: e8d4be4b06c051386afa0358856a6df49abb6653
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73026470"
---
# <a name="xamarinios-app-distribution-overview"></a>Общие сведения о распространении приложений Xamarin.iOS

_В этом документе содержатся общие сведения о методах распространения приложений Xamarin.iOS, и приводятся ссылки на подробную документацию по этой теме._

После разработки приложения Xamarin.iOS наступает следующий этап жизненного цикла разработки ПО — распространение приложения пользователям (см. выделенную часть на схеме ниже):

[![](images/publishingdiagram.png "After the iOS app has been developed, the next step is to distribute the app to users, as shown in the highlighted section of this diagram")](images/publishingdiagram.png#lightbox)

Компания Apple предоставляет следующие способы распространения приложения iOS, поддерживаемые в Xamarin.iOS:

1. [**Магазин приложений**](#App_Store_Distribution)
2. [**Внутреннее (корпоративное) распространение**](#In-House_Distribution)
3. [**Прямое распространение**](#Ad_Hoc_Distribution)

Для реализации всех этих сценариев требуется подготовить приложения с помощью соответствующего *профиля подготовки*. Профили подготовки — это файлы, содержащие сведения о подписывании кода, а также идентификатор приложения и подходящий механизм распространения. Для распространения не через Магазин приложений они также содержат сведения о том, на каких устройствах можно развертывать приложения.

<a name="App_Store_Distribution"/>

## <a name="app-store-distribution"></a>Распространение через App Store

> [!IMPORTANT]
> Корпорация Apple [объявила](https://developer.apple.com/ios/submit/), что начиная с марта 2019 г. все публикуемые в App Store приложения и обновления должны быть собраны с использованием пакета SDK для iOS 12.1 или более поздних версий, входящего в Xcode версии 10.1 и выше.
> Кроме того, приложения должны поддерживать размеры экранов iPhone XS и iPad Pro с диагональю 12,9 дюйма.

Это основной способ распространения приложений iOS на устройствах iOS потребителям. Все приложения, отправляемые в Магазин приложений, требуют утверждения Apple.

Приложения отправляются в Магазин приложений через портал *iTunes Connect*. Дополнительные сведения о настройке и использовании портала для подготовки приложения Xamarin.iOS для публикации в Магазин приложений см. в руководстве [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md).

Важно отметить, что доступ к iTunes Connect имеют только разработчики, которые участвуют в **программе для разработчиков Apple**. У участников **корпоративной программы для разработчиков Apple** доступа нет.

Дополнительные сведения см. в руководстве [Распространение через Магазин приложений](~/ios/deploy-test/app-distribution/app-store-distribution/index.md).

<a name="In-House_Distribution"/>

## <a name="in-house-distribution"></a>Внутреннее распространение

В рамках внутреннего распространения (иногда называемого *корпоративным распространением*) участники **корпоративной программы для разработчиков Apple** могут распространять приложения другим участникам в той же организации. Преимуществами внутреннего распространения являются отсутствие проверки в Магазине приложений и отсутствие ограничений на количество устройств для установки приложения. Однако важно отметить, что участники **корпоративной программы для разработчиков Apple** **не** имеют доступ к iTunes Connect, поэтому за распространение приложения отвечает держатель лицензии.

Дополнительные сведения о настройке и внутреннем распространении приложений см. в [руководстве по внутреннему распространению](~/ios/deploy-test/app-distribution/in-house-distribution.md).

<a name="Ad_Hoc_Distribution"/>

## <a name="ad-hoc-distribution"></a>Прямое распространение

Пользователи могут протестировать приложения Xamarin.iOS в рамках прямого распространения, доступного по **программе для разработчиков Apple** и **корпоративной программе для разработчиков Apple** и позволяющего проверить до 100 устройств iOS. Лучшим вариантом прямого распространения является распространение в пределах компании, когда использование iTunes Connect не подходит.

Дополнительные сведения о настройке и внутреннем распространении приложений см. в [руководстве по прямому распространению](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md).

## <a name="summary"></a>Сводка

В этой статье давался краткий обзор механизмов распространения, доступных для приложений Xamarin.iOS. Были представлены такие варианты распространения, как iTunes App Store, прямое и внутреннее, и приводились ссылки на более подробные сведения.

## <a name="related-links"></a>Связанные ссылки

- [Распространение через Магазин приложений](~/ios/deploy-test/app-distribution/app-store-distribution/index.md)
- [Настройка приложения в iTunes Connect](~/ios/deploy-test/app-distribution/app-store-distribution/itunesconnect.md)
- [Публикация в App Store](~/ios/deploy-test/app-distribution/app-store-distribution/publishing-to-the-app-store.md)
- [Внутреннее распространение](~/ios/deploy-test/app-distribution/in-house-distribution.md)
- [Прямое распространение](~/ios/deploy-test/app-distribution/ad-hoc-distribution.md)
- [Файл iTunesMetadata.plist](~/ios/deploy-test/app-distribution/itunesmetadata.md)
- [Поддержка IPA](~/ios/deploy-test/app-distribution/ipa-support.md)
- [Устранение неполадок](~/ios/deploy-test/troubleshooting.md)
