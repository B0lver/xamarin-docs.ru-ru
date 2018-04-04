---
title: Работа с группами приложений
ms.prod: xamarin
ms.technology: xamarin-ios
ms.assetid: 6968606B-C287-424F-A321-2492E12BC0BB
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/17/2017
ms.openlocfilehash: 341f6b4f5049a39817f2923901472f35b254bead
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="working-with-app-groups"></a>Работа с группами приложений


Группы приложений предоставляют различным приложениям (или одному приложению и его расширениям) общее место хранения файлов. Группы приложений можно использовать для следующих данных:

- Apple Watch [параметры](~/ios/watchos/app-fundamentals/settings.md).
- Общие [NSUserDefaults](~/ios/watchos/app-fundamentals/parent-app.md#nsuserdefaults).
- Общие [файлы](~/ios/watchos/app-fundamentals/parent-app.md#files).

## <a name="configure-an-app-group"></a>Настройка группы приложений

Общее расположение настраивается с помощью [группы приложения](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19), настроенной в **сертификатов, профили & идентификаторы** статьи на [центра разработки iOS](https://developer.apple.com/devcenter/ios/). Это значение также должна иметься ссылка в каждом проекте **Entitlements.plist**.

### <a name="provisioning"></a>Подготовка

Группа приложений будет иметь идентификатора, который обычно имеет свой идентификатор пакета с `group.` префикс. Например, можно использовать идентификатор набора `com.xamarin.WatchSettings` и группа приложения `group.com.xamarin.WatchSettings`.

[![](app-groups-images/app-group-sml.png "Используйте идентификатор пакета com.xamarin.WatchSettings и group.com.xamarin.WatchSettings группы приложений")](app-groups-images/app-group.png#lightbox)

### <a name="entitlementsplist"></a>Entitlements.plist

А также настройка профиля подготовки, **включить группы приложений** в **Entitlements.plist** и введите идентификатор выбора:

[![](app-groups-images/entitlements-sml.png "Настройка plist-файл и введите идентификатор")](app-groups-images/entitlements.png#lightbox)


### <a name="deployment"></a>Развертывание

Убедитесь, настроен правильно в группе приложений вашей [развертывания](~/ios/watchos/deploy-test/index.md#App_Groups) подготовки.


Дополнительные сведения см. в разделе [возможности группами приложений](~/ios/deploy-test/provisioning/capabilities/app-groups-capabilities.md) документации.


## <a name="related-links"></a>Связанные ссылки

- [Apple обмена данными с содержащего приложения](https://developer.apple.com/library/ios/documentation/General/Conceptual/ExtensibilityPG/ExtensionScenarios.html)
- [Doc Apple группы приложений](https://developer.apple.com/library/ios/documentation/Miscellaneous/Reference/EntitlementKeyReference/Chapters/EnablingAppSandbox.html#//apple_ref/doc/uid/TP40011195-CH4-SW19)
