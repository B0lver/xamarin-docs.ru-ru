---
title: "Xamarin.Android и JDK 9"
description: "В этой статье описывается устранение ошибок Java Development Kit (JDK) 9 в Xamarin.Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 7DCF0985-F77D-4A68-AC54-10C9846E189A
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 9e1dd2fac015783e06bad6656f09a687f3abba2c
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="xamarinandroid-and-jdk-9"></a>Xamarin.Android и JDK 9

_В этой статье описывается устранение ошибок Java Development Kit (JDK) 9 в Xamarin.Android._


## <a name="overview"></a>Обзор

Xamarin.Android использует Java Development Kit (JDK) для интеграции с Android SDK для приложений Android построения и запуска конструктора Android. Последние версии пакета SDK Android (API 24 и более поздние версии) требуют JDK 8 (1.8). **Поскольку средства пакета SDK для Android, доступного в Google не совместимы с JDK 9, Xamarin.Android не работает с JDK 9.**

## <a name="jdk-9-errors"></a>JDK 9 ошибок

Если сборка проекта Xamarin.Android 9 JDK установлен, вы получите явную ошибку, указывающее, что JDK 9 не поддерживается.

```shell
Building with JDK Version `9.0.4` is not supported. Please install JDK version `1.8.0`. See https://aka.ms/xamarin/jdk9-errors  
```

Для устранения этих ошибок, необходимо установить JDK 8 (1.8), как описано в статье [как обновить Java Development Kit (JDK) версии?](~/android/troubleshooting/questions/update-jdk.md).


## <a name="checking-the-jdk-version"></a>Проверка версии JDK

Можно проверить, чтобы просмотреть текущую версию Java, вы установили, введя следующую команду (JDK `bin` каталог должен быть в вашей `PATH`):

```shell
java -version
```

Если JDK 9 установлен, появится сообщение, подобное следующему:

```shell
java version "9.0.4"
Java(TM) SE Runtime Environment (build 9.0.4+11)
Java HotSpot(TM) 64-Bit Server VM (build 9.0.4+11, mixed mode)
```

Если JDK 9 установлен, необходимо установить Java JDK 8 (1.8). Сведения о способах установки JDK 8 см. в разделе [как обновить версию Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

## <a name="known-issues-with-jdk-9"></a>Известные проблемы с JDK 9

### <a name="apksigner"></a>apksigner

Имеется известная проблема с apksigner и JDK 9, в котором `apksigner.bat` файла вызывается `apksigner.jar` с `-Djava.ext.dirs` вместо `-classpath` которого ожидает JDK 9. Рекомендуется использовать JDK 8 (1.8). Сведения о способах установки JDK 8 см. в разделе [как обновить версию Java Development Kit (JDK)?](~/android/troubleshooting/questions/update-jdk.md)

После удаления JDK 9, убедитесь, что следующий путь не указывает на `PATH` переменной среды, как оно по-прежнему будет указывать JDK 9: `C:\ProgramData\Oracle\Java\javapath`. После удаления, `java -version` в командной строке должно отображаться JDK 8.
