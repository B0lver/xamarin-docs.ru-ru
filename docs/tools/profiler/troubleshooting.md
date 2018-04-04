---
title: Профилировщик Xamarin, устранение неполадок
description: Устранение неполадок профилировщик Xamarin
ms.prod: xamarin
ms.assetid: 0060E9D1-C003-4E4C-ADE8-B406978FE891
ms.technology: xamarin-cross-platform
author: topgenorth
ms.author: toopge
ms.date: 10/27/2017
ms.openlocfilehash: 4844c999ceddcee89d4f45f6e41dd4c7f2caf054
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="xamarin-profiler-troubleshooting"></a>Профилировщик Xamarin, устранение неполадок

_Устранение неполадок профилировщик Xamarin_

## <a name="logging-and-diagnostics"></a>Ведение журнала и диагностики

Команда Xamarin может помочь отслеживать проблемы, если предоставить нам сведения, включая:

- Демонстрация проблему, сбоя или ошибкой и вплоть до его рабочего процесса.
- Выходные данные журнала (см. ниже).
- **.Mlpd** , генерируемых для сеанса профилирования (см. ниже).

### <a name="getting-log-outputs"></a>Получение выходных данных журнала
На Mac для сохранения журналов `~/Library/Logs/Xamarin.Profiler/Profiler.<date>.log`.

В Windows, они сохраняются в `%appdata%Local//Xamarin/Log/Xamarin.Profiler/Profiler.<date>.log` при каждой отправке проблемы включите последнего журнала.

Мы добавляем дополнительные ведения журнала постоянно, поэтому этот выход должен увеличиваться и становятся более полезным со временем.

<a name="gen_mlpd" />

### <a name="generating-mlpd-files"></a>Создание файлов .mlpd

**.Mlpd** файл создается сжатый профилировщика mono среды выполнения. Графический пользовательский Интерфейс профилировщик Xamarin считывает данные из **.mlpd** и отображает его пользователю. **.mlpd** файлы полезны, средства отладки для Xamarin, так как они позволяют наши инженеры диагностики проблем, профилировщик может возникнуть с данными.

**.Mlpd** для текущего сеанса, автоматически сохраняется в компьютере Mac `/tmp` каталога и могут идентифицироваться по меткам. Если включено ведение журнала, первые выходные данные будут путь к **.mlpd** файла. **.Mlpd** обычно файл будет сохранен в каталоге запуска ~/var и папок...

**.Mlpd** для текущего сеанса также можно сохранять, выбрав **файл > Сохранить как...** из меню профилировщика:

**Visual Studio для Mac**:

![](troubleshooting-images/image17.png "Сохранение файла .mlpd в Visual Studio для Mac")

**Visual Studio**:

![](troubleshooting-images/image17-vs.png "Сохранение файла .mlpd в Visual Studio")


Важно обратить внимание, что **.mlpd** содержат большой объем сведений, а размер файла будет большой.

## <a name="troubleshooting"></a>Устранение неполадок

В следующем списке перечислены общие рекомендации, обойти это ограничение, советы и рекомендации по использованию профилировщика.

> [!NOTE]
> **Примечание**: вы должны быть Visual Studio **Enterprise** подписчике, чтобы разблокировать этот компонент в любом Visual Studio Enterprise на Windows и Visual Studio для Mac.

#### <a name="i-cant-see-the-ios-profiler-option-or-it-is-greyed-out-visual-studio-and-visual-studio-for-mac"></a>Не виден параметр профилировщик операций ввода-вывода или неактивна [Visual Studio для Mac и Visual Studio]

Проверьте следующие параметры для решения этой проблемы:

- Убедитесь, что вы используете конфигурацию отладки
- Убедитесь, что вы используете сборщик мусора SGen.
- Убедитесь, платформа — [поддерживается](~/tools/profiler/index.md#Profiler_Support).
- Обеспечьте правой лицензии.
- Убедитесь, что вы вошли в и должным образом проверку подлинности.
- [Visual Studio] Необходимо использовать [Visual Studio Enterprise](https://www.visualstudio.com/vs/enterprise/) и иметь действующую лицензию Enterprise.


#### <a name="i-get-an-error-when-i-try-to-launch-the-profiler"></a>Произошла ошибка при попытке запуска профилировщика

Если при запуске в это поле ошибки с помощью профилировщика в Visual Studio:

![](troubleshooting-images/error.png "Поле ошибки при использовании профилировщика в Visual Studio")

Это обычно из-за невозможности запуска, чтобы имитатор или эмулятор. Попробуйте и запустить приложение, как правило, устранить проблемы, которые он предоставляет, а затем повторите попытку использовать профилировщик.

#### <a name="to-watch-a-specific-thread"></a>Для отслеживания определенного потока

Если поток, который вы хотите посмотреть специально, было бы идеальным для имени потока в самом начиная ее создания, чтобы получить get `ThreadName` вместо `0x0`. Например задать имя потока в качестве пользовательского интерфейса можно использовать следующий код:


```csharp
RunOnUiThread (() => {
  Thread.CurrentThread.Name  = "UI";
});
```



## <a name="related-links"></a>Связанные ссылки

- [Пошаговое руководство. Использование профилировщик Xamarin](~/tools/profiler/index.md)
- [Рекомендации по памятью и производительностью](~/cross-platform/deploy-test/memory-perf-best-practices.md)
- [Заметки о выпуске](https://developer.xamarin.com/releases/profiler/preview/)
