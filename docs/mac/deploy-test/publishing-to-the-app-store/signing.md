---
title: "Подписывание с использованием идентификатора разработчика"
description: "В этом руководстве содержатся сведения о подписывании приложения Xamarin.Mac с использованием идентификатора разработчика для публикации."
ms.topic: article
ms.prod: xamarin
ms.assetid: cf7b733b-e08f-4f56-a233-264b29ee4c97
ms.technology: xamarin-mac
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/14/2017
ms.openlocfilehash: 46d5527a33b82a795029f62900e782d644671f0d
ms.sourcegitcommit: 30055c534d9caf5dffcfdeafd6f08e666fb870a8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/09/2018
---
# <a name="sign-with-developer-id"></a>Подписывание с использованием идентификатора разработчика

Если приложение планируется распространять напрямую пользователям macOS, компания Apple рекомендует подписать его код с помощью идентификатора разработчика, чтобы устанавливать в системах macOS с включенным **привратником**. Если приложение не было подписано, **привратник** запретит его устанавливать, выводя предупреждение (пользователи могут обойти это ограничение, удерживая нажатой клавишу CTRL при запуске).

Дополнительные сведения об [идентификаторе разработчика и привратнике](https://developer.apple.com/resources/developer-id/) и [распространении за пределами Mac App Store](https://developer.apple.com/library/content/documentation/IDEs/Conceptual/AppDistributionGuide/Introduction/Introduction.html) см. на веб-сайте Apple.

## <a name="code-signing-options"></a>Параметры подписывания кода

Чтобы создать приложение, напрямую развертываемое для пользователей (а не через Mac App Store), в **параметрах подписывания** используйте значение **Идентификатор разработчика**. Параметр "Конфигурация" должен иметь значение **Выпуск**.

 [![](signing-images/config02.png "Параметры подписывания Mac")](signing-images/config02.png#lightbox)


## <a name="build"></a>Построить

Перед выполнением сборки убедитесь, что выбрана правильная конфигурация, и в окне **Сборка Mac** выберите параметр создания пакета установки.

[![](signing-images/config03.png "Параметры сборки")](signing-images/config03.png#lightbox)

При сборке приложения выводится предложение об использовании обоих сертификатов:

 [![](signing-images/image57.png "Разрешить доступ к цепочке ключей")](signing-images/image57.png#lightbox)

 [![](signing-images/image58.png "Разрешить доступ к цепочке ключей")](signing-images/image58.png#lightbox)

После сборки приложения щелкните проект правой кнопкой мыши и выберите команду **Открыть содержащую папку**, чтобы найти файл пакета (в каталоге `bin/Release`). В этом файле содержится установщик для приложения, поэтому его можно распространять любому пользователю macOS для установки.

 [![](signing-images/image59.png "Выбор пакета приложения в Finder")](signing-images/image59.png#lightbox)

## <a name="related-links"></a>Связанные ссылки

- [Установка](~//mac/get-started/installation.md)
- [Пример кода приложения "Привет, Mac"](~//mac/get-started/hello-mac.md)
- [Распространение приложений в Mac App Store](https://developer.apple.com/devcenter/mac/checklist/)
- [Руководство по работе с инструментами. Подписывание кода приложения](https://developer.apple.com/library/mac/#documentation/ToolsLanguages/Conceptual/OSXWorkflowGuide/CodeSigning/CodeSigning.html)
- [Идентификатор разработчика и привратник](https://developer.apple.com/resources/developer-id/)
