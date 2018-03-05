---
title: "Настройка приложений Mac"
description: "В этом руководстве описывается настройка приложения Xamarin.Mac для публикации."
ms.topic: article
ms.prod: xamarin
ms.assetid: fea66a34-1581-4cd6-b714-3fbff215a542
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 04/12/2017
ms.openlocfilehash: 8f9294c10f8d3287a2985ede9aadf84ce663c38a
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="mac-app-configuration"></a>Настройка приложений Mac

_В этом руководстве описано, как настроить приложение Xamarin.Mac для публикации._


## <a name="mac-app-configuration"></a>Настройка приложений Mac

Щелкните правой кнопкой мыши проект приложения Mac в Visual Studio для Mac и выберите пункт **Параметры**.


### <a name="application-settings"></a>Параметры приложения

Чтобы изменить параметры приложения в приложении Xamarin.Mac, дважды щелкните файл **Info.plist** на **Панели решения**:

![Выбор файла info.plist](app-configuration-images/config04.png "Selecting the Info.plist file")

Появятся доступные для приложения параметры:

 [![Редактирование файла info.plist](app-configuration-images/config01.png "Editing the Info.plist file")](app-configuration-images/config01-large.png)

Для запуска приложений Mac, созданных с помощью Xamarin.Mac, необходимо выполнить следующие системные требования:

- наличие компьютера Mac под управлением Mac OS X 10.7 или более поздней версии.


### <a name="signing-settings"></a>Параметры подписывания

В разделе **Подписывание Mac** в диалоговом окне **Параметры проекта** можно подписать приложение Xamarin.Mac для тестирования, самостоятельного выпуска или выпуска через магазин Apple App Store:

[![Редактор подписывания Mac](app-configuration-images/config02.png "The Mac Signing window")](app-configuration-images/config02-large.png)

Здесь выбираются значения для удостоверения, профиля подготовки и любых настраиваемых назначений, используемые для подписывания приложения после компиляции. При необходимости можно подписать установщик, используемый для установки приложения на других компьютерах Mac.


### <a name="build-settings"></a>Параметры сборки

В разделе **Сборка Mac** диалогового окна **Параметры проекта** можно выбирать архитектуру для приложения Xamarin.Mac, администрировать версии, поддерживаемые приложением macOS, и при необходимости создавать пакеты установки после успешной компиляции приложения.

 [![Изменение параметров сборки](app-configuration-images/config03.png "Editing the build options")](app-configuration-images/config03-large.png)


## <a name="related-links"></a>Связанные ссылки

- [Установка](/visualstudio/mac/installation/)
- [Пример кода приложения "Привет, Mac"](~/mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)
