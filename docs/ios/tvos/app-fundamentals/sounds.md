---
title: Воспроизведение звука с AVAudioPlayer
description: В этой статье показано, как использовать вспомогательный класс для управления воспроизведение звука с помощью AVAudioPlayer.
ms.prod: xamarin
ms.assetid: E0305572-DC64-48BB-BD97-0A5096E6CA04
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/16/2017
ms.openlocfilehash: c50aea9c4c35e91c2baa98c94db2fd7c61136d69
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="playing-sound-with-avaudioplayer"></a>Воспроизведение звука с AVAudioPlayer

_В этой статье показано, как использовать вспомогательный класс для управления воспроизведение звука с помощью AVAudioPlayer._

## <a name="about-the-avaudioplayer"></a>О AVAudioPlayer

`AVAudioPlayer` Используется для воспроизведения звуковых данных из памяти или файла. Apple рекомендует использовать этот класс для воспроизведения звука в приложении, если Вы поступаете сети потоковой передачи или требуют аудио небольшой задержки ввода-вывода.

Можно использовать `AVAudioPlayer` делать следующее:

- Воспроизведение звуков любой продолжительности с необязательным циклов.
- Воспроизведение звуков несколько одновременно с необязательным синхронизации.
- Управлять тома, скорость воспроизведения и стерео позиционирование для каждого звуки воспроизведения.
- Поддерживает функции, такие как Перемотка вперед или перемотка назад.
- Получите уровень воспроизведения программных продуктов.

`AVAudioPlayer` поддерживает звуков в любом формате аудио, предоставляемых операций ввода-вывода, tvOS и OS X например `.aif`, `.wav` или `.mp3`.

## <a name="playing-sounds-in-tvos"></a>Воспроизведение звуков в tvOS

Поскольку tvOS поддерживает те же классы звуковых элементов, поскольку iOS, см. в разделе iOS [игральной звука с AVAudioPlayer](http://developer.xamarin.com/recipes/ios/media/sound/avaudioplayer/) документации полные сведения о воспроизведение аудио в приложении Xamarin.tvOS.



## <a name="related-links"></a>Связанные ссылки

- [Справочник по AVAudioPlayer](https://developer.apple.com/library/ios/documentation/AVFoundation/Reference/AVAudioPlayerClassReference/)
