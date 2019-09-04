---
title: Где я могу найти информацию о версии и журналы
description: В этом документе описывается, где искать сведения о версии Xamarin и журналах. Эти сведения полезны при диагностике проблем, отправке ошибок или получении поддержки.
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: CF386485-EAB0-4B9E-AA17-CB1B6462E505
author: asb3993
ms.author: amburns
ms.date: 03/29/2017
ms.openlocfilehash: cd7a2026a4d1d1458455733a6f2710364cc7fec7
ms.sourcegitcommit: c9651cad80c2865bc628349d30e82721c01ddb4a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70226781"
---
# <a name="where-can-i-find-my-version-information-and-logs"></a>Где я могу найти информацию о версии и журналы

## <a name="outline"></a>Контур

- [Сведения о версии](#version-information)
  - Сведения о версии Windows
  - Сведения о версии Mac
  - Android SDK Tools, Platform-Tools, Build-Tools
- [Журналы IDE и установщика](#ide-and-installer-logs)
  - [Журналы Windows](#windows-logs)
    - Xamarin Studio
    - Xamarin для Visual Studio
    - Универсальный установщик Xamarin
    - Отдельные `.msi` установщики, подробные журналы
    - Запуск Visual Studio, подробные журналы
  - [Журналы Mac](#mac-logs)
    - Узел сборки
  - Visual Studio для Mac
    - Xamarin Studio
    - Установщик Xamarin
- [Подробные выходные данные сборки](#verbose-build-output-logs)
- [Журналы отладки для приложений Xamarin. Android и Xamarin. iOS](#debug-logs-for-xamarin-apps)
  - Журналы `adb` logcat для Android
  - журналы симуляторов iOS (на Mac)
  - журналы устройств iOS (на Mac)

## <a name="a-idversion-information-nameversion-information-version-information"></a><a id="version-information" name="version-information" />Сведения о версии

Обычно лучше всего отправить обратно все сведения из кнопок **Копировать информацию** . В противном случае часто требуется запросить дополнительную информацию. Например, при устранении неполадок могут быть важны версии операционной системы, версия Xcode, установленные уровни API Android и версия .NET.

### <a name="a-idwindows-version-information-namewindows-version-information-windows-version-information"></a><a id="windows-version-information" name="windows-version-information" />Сведения о версии Windows

#### <a name="xamarin-studio"></a>Xamarin Studio

**Справка > о > показывать сведения > копирование сведений [кнопка]**

#### <a name="visual-studio"></a>Visual Studio

**Справка > о Microsoft Visual Studio > Копировать сведения [кнопка]**

### <a name="a-idmac-version-information-namemac-version-information-mac-version-information"></a><a id="mac-version-information" name="mac-version-information" />Сведения о версии Mac

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Visual Studio > о Visual Studio > Отображение подробных сведений > копирование информации [кнопка]**

### <a name="a-idandroid-sdk-tools-versions-nameandroid-sdk-tools-versions-android-sdk-tools-platform-tools-build-tools"></a><a id="android-sdk-tools-versions" name="android-sdk-tools-versions" />Android SDK Tools, Platform-Tools, Build-Tools

Откройте диспетчер пакет SDK для Android и Создайте снимок экрана с разделом "лучшие **средства** ".

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Средства > открыть пакет SDK для Android Manager**

#### <a name="visual-studio"></a>Visual Studio

**Средства > Android > открыть диспетчер пакет SDK для Android...**

## <a name="a-idide-and-installer-logs-nameide-and-installer-logs-ide-and-installer-logs"></a><a id="ide-and-installer-logs" name="ide-and-installer-logs" />Журналы IDE и установщика

Для каждого расположения журнала обязательно заархивируйте и прикрепите всю папку журнала.

### <a name="a-idwindows-logs-namewindows-logs-windows-logs"></a><a id="windows-logs" name="windows-logs" />Журналы Windows

#### <a name="a-idwindows-logs-xamarin-vs-namewindows-logs-xamarin-vs--visual-studio-tools-for-xamarin"></a><a id="windows-logs-xamarin-vs" name="windows-logs-xamarin-vs" />Инструменты Visual Studio для Xamarin

`%LOCALAPPDATA%\Xamarin\Logs`

#### <a name="a-idvs-2017-namevs-2017--visual-studio-2017"></a><a id="vs-2017" name="vs-2017" />Visual Studio 2017

[Как получить журналы установки Visual Studio](https://docs.microsoft.com/visualstudio/install/troubleshooting-installation-issues#how-to-get-the-visual-studio-installation-logs)

#### <a name="a-idvs-2015-namevs-2015--visual-studio-2015"></a><a id="vs-2015" name="vs-2015" />Visual Studio 2015

#### <a name="a-idwindows-universal-installer-namewindows-universal-installer--xamarin-universal-installer"></a><a id="windows-universal-installer" name="windows-universal-installer" />Универсальный установщик Xamarin

`%LOCALAPPDATA%\Xamarin\Universal`

Это журналы из `XamarinInstaller.exe` установщика.

#### <a name="a-idindividual-msi-installers-verbose-logs-nameindividual-msi-installers-verbose-logs-individual-msi-installers-verbose-logs"></a><a id="individual-msi-installers-verbose-logs" name="individual-msi-installers-verbose-logs" />Отдельные `.msi` установщики, подробные журналы

```csharp
msiexec /i Xamarin.msi /l*vx "%USERPROFILE%\Desktop\Xamarin.log"
```

Справочные материалы. [Параметры командной строки](https://msdn.microsoft.com/library/aa367988.aspx)

#### <a name="a-idvisual-studio-startup-verbose-logs-namevisual-studio-startup-verbose-logs-visual-studio-startup-verbose-logs"></a><a id="visual-studio-startup-verbose-logs" name="visual-studio-startup-verbose-logs" />Запуск Visual Studio, подробные журналы

```csharp
devenv.exe /log "%USERPROFILE%\Desktop\VisualStudio.log"
```

Ссылка: [/log (devenv. exe)](https://msdn.microsoft.com/library/ms241272.aspx)

### <a name="a-idmac-logs-namemac-logs-mac-logs"></a><a id="mac-logs" name="mac-logs" />Журналы Mac

Можно выбрать пункт меню **переход > переход к папке** в Finder, а затем скопировать и вставить любой из этих путей в диалоговое окно.

#### <a name="a-idmac-logs-visual-studio-namemac-logs-visual-studio-visual-studio-for-mac"></a><a id="mac-logs-visual-studio" name="mac-logs-visual-studio" />Visual Studio для Mac

`~/Library/Logs/VisualStudio/7.0`(это значение может изменяться в зависимости от используемой версии).

Эту папку также можно открыть с помощью "Help-> открыть каталог журналов".

#### <a name="a-idmac-logs-xamarin-studio-namemac-logs-xamarin-studio-xamarin-studio"></a><a id="mac-logs-xamarin-studio" name="mac-logs-xamarin-studio" />Xamarin Studio

`~/Library/Logs/XamarinStudio-6.0`(это значение может изменяться в зависимости от используемой версии).

Эту папку также можно открыть с помощью "Help-> открыть каталог журналов".

#### <a name="a-idmac-universal-installer-namemac-universal-installer-xamarin-universal-installer"></a><a id="mac-universal-installer" name="mac-universal-installer" />Универсальный установщик Xamarin

`~/Library/Logs/XamarinInstaller/Universal`

Это журналы из `XamarinInstaller.dmg` установщика.

#### <a name="a-idmac-build-host-namemac-build-host-xamarin-build-host"></a><a id="mac-build-host" name="mac-build-host" />Узел сборки Xamarin

`~/Library/Logs/Xamarin-[MAJOR.MINOR]`

## <a name="a-idverbose-build-output-logs-nameverbose-build-output-logs-verbose-build-output"></a><a id="verbose-build-output-logs" name="verbose-build-output-logs" />Подробные выходные данные сборки

1. Включите [выходные данные диагностики MSBuild](~/android/troubleshooting/troubleshooting.md#Diagnostic_MSBuild_Output).

2. Для приложений iOS также включите **подробные выходные данные mtouch** , добавив `-v -v -v -v` **Свойства проекта > iOS Build > General (TAB) > Дополнительные параметры > Дополнительные аргументы mtouch**.

3. Очистите и перестройте проект.

4. Скопируйте и вставьте выходные данные сборки из IDE в текстовый файл.
     - Visual Studio (Windows): **Просмотр выходных данных > > Отображение выходных данных: Строит**
     - Visual Studio для Mac: **Просмотр > Pad > ошибок > выходные данные сборки (вкладка)**

## <a name="a-iddebug-logs-for-xamarin-apps-namedebug-logs-for-xamarin-apps-debug-logs-for-xamarinandroid-and-xamarinios-apps"></a><a id="debug-logs-for-xamarin-apps" name="debug-logs-for-xamarin-apps" />Журналы отладки для приложений Xamarin. Android и Xamarin. iOS

### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Просмотр выходных данных > приложения > Pad**

(Обратите внимание, что этот пункт меню будет отображаться только после запуска приложения.)

### <a name="visual-studio"></a>Visual Studio

**Просмотр выходных данных > > Отображение выходных данных: См**

### <a name="a-idadb-logcat-nameadb-logcat-android-adbhttpsdeveloperandroidcomtoolshelpadbhtml-logcat-logs"></a><a id="adb-logcat" name="adb-logcat" />Журналы [`adb`](https://developer.android.com/tools/help/adb.html) logcat для Android

После выполнения `adb` команды присоедините файл **android_logcat. txt** к рабочему столу. В этих инструкциях предполагается, что подключено только одно устройство.

См. также страницу [журнала отладки Android](~/android/deploy-test/debugging/android-debug-log.md) .

#### <a name="visual-studio"></a>Visual Studio

1. **Средства > Android > запустить командную строку ADB для Android**
2. Очистите журнал:`adb logcat -c`
3. Воспроизведите ошибку.
4. Вывод журнала:`adb logcat -vtime -d > "%USERPROFILE%\Desktop\android_logcat.txt"`

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

1. **Средства > открыть пакет SDK для Android командной строки**
2. Очистите журнал:`adb logcat -c`
3. Воспроизведите ошибку.
4. Вывод журнала:`adb logcat -vtime -d > ~/Desktop/android_logcat.txt`

### <a name="a-idios-simulator-logs-nameios-simulator-logs-ios-simulator-logs-on-mac"></a><a id="ios-simulator-logs" name="ios-simulator-logs" />журналы симуляторов iOS (на Mac)

- Чтобы получить доступ к системному журналу, выберите **отладка > открыть системный журнал...** в приложении для симуляторов iOS.

- Чтобы просмотреть отчеты о сбоях в симуляторе, откройте консоль. app и перейдите `~/Library/Logs > DiagnosticReports`по адресу.

### <a name="a-idios-device-logs-nameios-device-logs-ios-device-logs-on-mac"></a><a id="ios-device-logs" name="ios-device-logs" />журналы устройств iOS (на Mac)

#### <a name="visual-studio-for-mac"></a>Visual Studio для Mac

**Просмотр > Pad > Журнал устройств iOS**

#### <a name="xcode"></a>Xcode

**Окна > устройств > $ {DeviceName}**

Отчеты о сбоях доступны при нажатии кнопки **Просмотр журналов устройств** . Системный журнал для устройства отображается в нижней части окна со стрелкой раскрытия.

#### <a name="xcode-5"></a>Xcode 5

**Окно > Организатор > устройств (вкладка) > $ {DeviceName}**
