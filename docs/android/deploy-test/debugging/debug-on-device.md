---
title: "Отладка на устройстве"
description: "В этой статье содержатся сведения об отладке приложения Xamarin.Android на физическом устройстве Android."
ms.topic: article
ms.prod: xamarin
ms.assetid: 153D3746-A27F-198B-48FE-D219C0133A79
ms.technology: xamarin-android
author: mgmclemore
ms.author: mamcle
ms.date: 02/16/2018
ms.openlocfilehash: 5e9874fba52b576494be5ac42ff13fdd0be4d9e7
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="debug-on-device"></a>Отладка на устройстве

_В этой статье содержатся сведения об отладке приложения Xamarin.Android на физическом устройстве Android._

## <a name="debug-on-device-overview"></a>Обзор отладки на устройстве

Отладку приложения Xamarin.Android можно выполнить на устройстве Android с помощью Visual Studio для Mac или Visual Studio. Перед отладкой устройство необходимо [настроить для разработки](~/android/get-started/installation/set-up-device-for-development.md) и подключить к ПК или Mac.

<a name="Debug_Application" />

## <a name="debug-application"></a>Отладка приложения

После подключения устройства к компьютеру отладка приложения Xamarin.Android выполняется так же, как отладка любого продукта Xamarin или приложения .NET. Убедитесь, что в интегрированной среде разработки выбраны конфигурация **Отладка** и внешнее устройство. Это необходимо, чтобы обеспечить доступность соответствующих отладочных символов и возможность подключения среды IDE к запущенному приложению: 

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

![Выбрана конфигурация "Отладка"](debug-on-device-images/image1-vs.png)

Затем в коде устанавливается точка останова:

![Точка останова, установленная в строке кода](debug-on-device-images/image2-vs.png)

После выбора устройства Xamarin.Android подключится к устройству, развернет приложение и запустит его. По достижении точки останова отладчик остановит приложение, которое затем проходит отладку так же, как любые другие приложения C#: 

![Достигнута точка останова](debug-on-device-images/image3-vs.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

![Выбрана конфигурация "Отладка"](debug-on-device-images/image1-xs.png)

Затем в коде устанавливается точка останова:

![Точка останова, установленная в строке кода](debug-on-device-images/image2-xs.png)

После выбора устройства Xamarin.Android подключится к устройству, развернет приложение и запустит его. По достижении точки останова отладчик остановит приложение, которое затем проходит отладку так же, как любые другие приложения C#: 

![Достигнута точка останова](debug-on-device-images/image3-xs.png)

-----


<a name="Summary" />

## <a name="summary"></a>Сводка

В этом документе рассматривалась процедура отладки приложения Xamarin.Android путем установки точки останова и выбора целевого устройства.


## <a name="related-links"></a>Связанные ссылки

- [Настройка устройства для разработки](~/android/get-started/installation/set-up-device-for-development.md)
- [Установка атрибута Debuggable](~/android/deploy-test/debuggable-attribute.md)
