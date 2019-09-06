---
title: Где найти dSYM-файл, чтобы выразить символами журналы с данными об аварийном завершении iOS?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CB8607B9-FFDA-4617-8210-8E43EC512588
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 05/09/2018
ms.openlocfilehash: edd5a2c1ed2efdffc9bd28bb06ac19348615eefc
ms.sourcegitcommit: 933de144d1fbe7d412e49b743839cae4bfcac439
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/04/2019
ms.locfileid: "70292114"
---
# <a name="where-can-i-find-the-dsym-file-to-symbolicate-ios-crash-logs"></a>Где найти dSYM-файл, чтобы выразить символами журналы с данными об аварийном завершении iOS?

При создании приложения iOS с Visual Studio для Mac или Visual Studio 2017 файл. dSYM, необходимый для симболикате отчетов о сбоях, будет помещен в ту же иерархию каталогов, что и файл проекта приложения (. csproj). Точное расположение зависит от параметров сборки проекта:

- Если вы включили сборки для конкретных устройств, dSYM можно найти в следующем каталоге:

    **&lt;каталог&gt;проекта/bin/&lt;конфигурация&gt;платформы&gt; /девице-буилдс/устройство&lt;/&lt;&gt;- &lt; версия ОС&gt;/**

    Например:
  
    **TestApp/bin/iPhone/Release/Device-Builds/iPhone 8.4-11.3.1/**

- Если вы не включили сборки для конкретных устройств, dSYM можно найти в следующем каталоге:

    **&lt;Конфигурация платформы&gt;/bin/каталога&gt;проекта&lt;/&lt;&gt;/**

    Например:

    **TestApp/bin/iPhone/Release/**

> [!NOTE]
> В рамках процесса сборки Visual Studio 2017 копирует файл dSYM из узла сборки Mac в Windows. Если dSYM-файл не отображается в Windows, убедитесь, что вы настроили параметры сборки приложения, чтобы [создать IPA-файл](~/ios/deploy-test/app-distribution/ipa-support.md).

## <a name="see-also"></a>См. также

- [Файлы аварийного завершения iOS симболикатинг (Xamarin. iOS)](https://www.jmillerdev.net/symbolicating-ios-crash-files-xamarin-ios/)
- [Пояснение журналов аварийного завершения работы приложений iOS](https://www.raywenderlich.com/23704/demystifying-ios-application-crash-logs)

