---
title: Устранение неполадок книг Xamarin на Android
ms.prod: xamarin
ms.assetid: F1BD293B-4EB7-4C18-A699-718AB2844DFB
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.openlocfilehash: 03c775f6f14263878f9f4a4633ba72931b233b17
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="troubleshooting-xamarin-workbooks-on-android"></a>Устранение неполадок книг Xamarin на Android

## <a name="emulator-support"></a>Поддержка эмулятора

Для запуска книги Android, эмулятор Android должен быть доступен для использования. Физические устройства Android не поддерживаются.

Корпорация Майкрософт рекомендует эмулятора Google вместе с HAXM, если компьютер поддерживает его.
При наличии Hyper-V включен на компьютере перейдите с помощью Visual Studio эмулятор Android.

Необходимо иметь эмулятор под управлением Android 5.0 или более поздней версии. Эмуляторы ARM не поддерживаются. Используйте `x86` или `x86_64` только для устройств.

Ознакомьтесь с [нашей документации по настройке эмуляторы Android] [ android-emu] Если вы не знакомы с процессом.

> [!NOTE]
> Книги, 1.1 и более ранних версиях будет попробуйте (и ошибкой!) для использования эмуляторов ARM, если они доступны. Чтобы обойти это, эмулятор запуска x86 по своему усмотрению до открытия или создания книги Android. Книги будут всегда рекомендуется использовать для подключения к запущенный эмулятор, пока она совместима.

## <a name="workbooks-wont-load"></a>Не будет загружать книги

### <a name="workbook-window-spins-forever-never-loads-windows"></a>Окно книги навсегда, вращается никогда не загружает (Windows)

Сначала проверьте наличие доступа к сети полностью работу симулятора тестирование любой веб-сайт в веб-браузере эмулятора.

### <a name="visual-studio-android-emulator-cannot-connect-to-the-internet"></a>Эмулятор Android Visual Studio не удается подключиться к Интернету

Если симулятор не имеет доступа к сети, может потребоваться для исправления вашего сетевого коммутатора Hyper-V выполните следующие действия. Повторите это действие периодически может потребоваться в том случае, если переключаться между часто сети Wi-Fi:

0. **Убедитесь в том, что критические сетевые операции выполнены, как это могут быть временно разорваны Windows из Интернета.**
1. Закройте эмуляторы.
2. Откройте `Hyper-V Manager`.
3. В разделе `Actions`откройте `Virtual Switch Manager...`.
4. Удалите все виртуальные коммутаторы.
5. Нажмите кнопку `OK`.
6. Запустите эмулятор VS Android. Возможно, вам будет предложено повторно создать виртуальный сетевой коммутатор.
7. Тест браузера Android эмулятора Visual STUDIO можно получить доступ к Интернет.

[android-emu]: https://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debug-on-emulator/


## <a name="related-links"></a>Связанные ссылки

- [Регистрации ошибок](~/tools/workbooks/install.md#reporting-bugs)
