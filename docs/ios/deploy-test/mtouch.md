---
title: mtouch
ms.topic: article
ms.prod: xamarin
ms.assetid: BCA491DA-E4C1-8689-3EC9-E4C72495A798
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.openlocfilehash: b1b61e7ce1bae413f132cfe1e6c051a53b786f98
ms.sourcegitcommit: 6cd40d190abe38edd50fc74331be15324a845a28
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/27/2018
---
# <a name="mtouch"></a>mtouch


Приложения iPhone поставляются в виде пакетов приложений. Это каталоги с расширением `.app`, содержащие код и данные приложения, файлы конфигурации и манифест, из которых iPhone сможет получить сведения о приложении.

Преобразование исполняемого файла .NET в приложение выполняется в основном с помощью команды `mtouch`. При этом интегрируется множество действий, необходимых для создания пакета приложения. Это средство также позволяет запустить приложение в симуляторе и развернуть программное обеспечение на настоящем устройстве iPhone или iPod Touch.


## <a name="detailed-instructions"></a>Подробные инструкции

Изучите страницу руководства по [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)), где описаны все варианты использования этого средства.


## <a name="building"></a>Сборка

Команда `mtouch` позволяет компилировать код тремя разными способами:

-  компиляция для тестирования на симуляторе;
-  компиляция для развертывания на устройстве;
-  развертывание исполняемого файла на устройстве.


### <a name="building-for-the-simulator"></a>Компиляция для симулятора

Самый распространенный и часто используемый сценарий на начальных этапах работы — это проверка приложений в симуляторе. Для нее вам нужно скомпилировать код в пакет для симулятора с помощью `mtouch -sim`. Это выполняется следующим образом:

```bash
$ mtouch -sim Hello.app hello.exe
```

### <a name="building-for-the-device"></a>Компиляция для устройства

Чтобы создать пакет программного обеспечения для устройства, скомпилируйте приложения с параметром `mtouch -dev`, указав также имя сертификата для подписи этого приложения. Ниже показано, как скомпилировать приложение для устройства:

```bash
$ mtouch -dev -c "iPhone Developer: Miguel de Icaza" foo.exe
```

В этом примере мы используем для подписи приложения сертификат разработчика iPhone с именем Мигель де Иказа (Miguel de Icaza). Это очень важный шаг, без него физические устройства не будут загружать приложение.

 <a name="Running_your_Application" />


## <a name="running-your-application"></a>Запуск приложения


### <a name="launching-on-the-simulator"></a>Запуск в симуляторе

После создания пакета приложения запуск в симуляторе выполняется очень просто:

```bash
$ mtouch --sdkroot /Applications/Xcode.app -launchsim Hello.app 
```

Если не задан флаг `--sdkroot`, для него по умолчанию будет установлено значение пути xcode-select, что приведет к появлению следующего предупреждения:

> eg: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer (предупреждение MT0061: не указан параметр Xcode.app с помощью ключа --sdkroot, используется системное значение Xcode, возвращаемое командой 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer). 

Приведенная выше командная строка даст примерно следующий результат:

```bash
Launching application
Application launched
PID: 98460
Press enter to terminate the application
```



Настоятельно рекомендуем сохранять в файлах стандартные потоки вывода и ошибок, которые наверняка пригодятся при отладке. Выходные данные `Console.WriteLine` направляются в `stdout`, а выходные данные `Console.Error.WriteLine` и все остальные сообщения среды выполнения направляются в `stderr`.

Чтобы сохранить их, установите флаги `--stdout` и `--stderr`.

```bash
../../tools/mtouch/mtouch --launchsim=Hello.app --stdout=output --stderr=error
```

Теперь в случае сбоя приложения вы сможете проверить выходные данные и сообщения об ошибках для диагностики проблемы.


### <a name="deploying-to-a-device"></a>Развертывание на устройстве

Для развертывания на устройстве необходимо подготовить его в соответствии с описанием в документе компании Apple [об управлении устройствами](http://developer.apple.com/library/ios/#documentation/Xcode/Conceptual/ios_development_workflow/00-About_the_iOS_Application_Development_Workflow/introduction.html). Подготовив устройство, разверните на нем скомпилированные APP-файлы с помощью команды mtouch. Для этого ее нужно выполнить следующим образом:

```bash
$ mtouch —sdkroot /Applications/Xcode.app -installdev=MyApp.app
```

Если не задан флаг `--sdkroot`, для него по умолчанию будет установлено значение пути xcode-select, что приведет к появлению следующего предупреждения:

> eg: warning MT0061: No Xcode.app specified (using --sdkroot), using the system Xcode as reported by 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer (предупреждение MT0061: не указан параметр Xcode.app с помощью ключа --sdkroot, используется системное значение Xcode, возвращаемое командой 'xcode-select --print-path': /Applications/Xcode.app/Contents/Developer). 

Эти действия обычно выполняются с помощью Visual Studio для Mac.

## <a name="reference"></a>Ссылка

Сведения о других параметрах командной строки см. на странице документации по команде [mtouch(1)](http://docs.go-mono.com/?link=man%3amtouch(1)).



## <a name="related-links"></a>Связанные ссылки

- [mtouch(1)](http://iosapi.xamarin.com/?link=man%3amtouch(1))
