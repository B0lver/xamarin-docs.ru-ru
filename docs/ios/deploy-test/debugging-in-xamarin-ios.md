---
title: Отладка приложений Xamarin.iOS
description: Этот документ описывает, как использовать отладчик в Visual Studio для Mac или Visual Studio 2017 для отладки приложения Xamarin.iOS, включая задание точек останова, беспроводную отладку и многое другое.
ms.prod: xamarin
ms.assetid: 05460010-99E1-DC38-F855-2D691EF54484
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/19/2017
ms.openlocfilehash: 4b21a69e49c8c7fd79de8edac9858c4714657f1c
ms.sourcegitcommit: b56b3f906d2c05a3f1be219ef41be8b79e519b8e
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/25/2018
ms.locfileid: "39242320"
---
# <a name="debugging-xamarinios-apps"></a>Отладка приложений Xamarin.iOS

_Для отладки приложений Xamarin.iOS можно использовать встроенный отладчик Visual Studio или Visual Studio для Mac._

Используйте встроенное средство отладки Visual Studio для Mac при отладке кода C# и других управляемых языков. Используйте [LLDB](http://lldb.llvm.org/tutorial.html) при отладке кода C, C++ или Objective C, привязываемого к проекту Xamarin.iOS.


> [!NOTE]
> При компиляции в режиме отладки приложения характеризуются низкой скоростью и большим размером, так как Xamarin.iOS нужно инструментировать каждую строку кода. Обязательно переведите сборку в режим выпуска, прежде чем осуществлять выпуск.

Отладчик Xamarin.iOS интегрирован в IDE и позволяет разработчикам выполнять отладку приложений Xamarin.iOS, созданных на любом поддерживаемом Xamarin.iOS управляемом языке, в симуляторе или на устройстве.

Отладчик Xamarin.iOS использует ["мягкий" отладчик Mono](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/), который обеспечивает взаимодействие сгенерированного кода и среды выполнения Mono со средой IDE в процессе отладки. Этим он отличается от "жестких" отладчиков, таких как LLDB или MDB, которые контролируют выполнение отлаживаемой программы без взаимодействия с ней и учета ее особенностей.

## <a name="setting-breakpoints"></a>Задание точек останова

Приступая к процессу отладки, в первую очередь необходимо [задать для приложения точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Для этого щелкните на полях редактора рядом с номером той строки кода, где нужно установить точку:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging1.png "Задание точек останова")](debugging-in-xamarin-ios-images/debugging1.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging1a.png "Задание точек останова")](debugging-in-xamarin-ios-images/debugging1a.png#lightbox)

-----

Вы можете просмотреть все точки останова, заданные в коде, на **панели точек останова**:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/image0a.png "Панель точек останова")](debugging-in-xamarin-ios-images/image0a.png#lightbox)

 Если панель точки останова не отображается автоматически, выберите _"Представление" > "Отладка Windows" > "Точки останова"_
 
# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/image0.png "Панель точек останова")](debugging-in-xamarin-ios-images/image0.png#lightbox)

 Если панель точки останова не отображается автоматически, выберите _"Отладка" > "Windows" > "Точки останова"_
 
-----

Прежде чем приступать к отладке любого приложения, обязательно переведите конфигурацию в режим **Отладки**, поскольку он содержит полезный набор инструментов для выполнения отладки, включая точки останова, визуализаторы данных и просмотр стека вызовов:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7.png "Отладка в симуляторе")](debugging-in-xamarin-ios-images/debugging7.png#lightbox)
[ ![](debugging-in-xamarin-ios-images/debugging7a.png "Отладка на физическом устройстве")](debugging-in-xamarin-ios-images/debugging7a.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7c.png "Отладка в симуляторе")](debugging-in-xamarin-ios-images/debugging7c.png#lightbox)
[ ![](debugging-in-xamarin-ios-images/debugging7d.png "Отладка на физическом устройстве")](debugging-in-xamarin-ios-images/debugging7d.png#lightbox)

-----

## <a name="start-debugging"></a>Начало отладки
Чтобы начать отладку, выберите целевое или аналогичное устройство в IDE:

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

[![](debugging-in-xamarin-ios-images/debugging7b.png "Выбор целевого устройства")](debugging-in-xamarin-ios-images/debugging7b.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

[![](debugging-in-xamarin-ios-images/debugging7e.png "Выбор целевого устройства")](debugging-in-xamarin-ios-images/debugging7e.png#lightbox)

-----



После этого разверните приложение, нажав кнопку **Воспроизвести**.

При попадании в точку останова код выделяется желтым цветом:

[![](debugging-in-xamarin-ios-images/image2.png "Код будет выделен желтым цветом")](debugging-in-xamarin-ios-images/image2.png#lightbox)

На этом этапе вы можете использовать средства отладки, например проверку значений объектов, для получения дополнительных сведений о том, что происходит в коде:

[![](debugging-in-xamarin-ios-images/image3.png "Отображение значения цвета")](debugging-in-xamarin-ios-images/image3.png#lightbox)

## <a name="conditional-breakpoints"></a>Условные точки останова

Вы также можете задавать правила, определяющие условия активации точки останова. Это называется добавлением *условной точки останова*.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Чтобы задать условную точку останова, перейдите в окно **Свойства точки останова**, что можно сделать двумя способами:


- Чтобы добавить условную точку останова, щелкните правой кнопкой мыши поле редактора слева от номера строки кода, где требуется задать точку останова, а затем выберите пункт "Создать точку останова":

    [![](debugging-in-xamarin-ios-images/image4.png "Выбор новой точки останова")](debugging-in-xamarin-ios-images/image4.png#lightbox)

- Чтобы добавить условие в существующую точку останова, щелкните ее правой кнопкой мыши и выберите пункт ***Свойства точки останова** или на **панели точек останова** нажмите кнопку свойств, как показано ниже:

    [![](debugging-in-xamarin-ios-images/image5.png "Панель точек останова")](debugging-in-xamarin-ios-images/image5.png#lightbox)


Затем вы можете ввести условие, при котором эта точка останова активируется:

[![](debugging-in-xamarin-ios-images/image6.png "Ввод условия для точки останова")](debugging-in-xamarin-ios-images/image6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Чтобы задать условную точку останова в Visual Studio 2015, сначала [задайте обычную точку останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint). Щелкните точку останова правой кнопкой мыши, чтобы открыть контекстное меню:

 [![](debugging-in-xamarin-ios-images/image4vs.png "Контекстное меню точки останова")](debugging-in-xamarin-ios-images/image4vs.png#lightbox)

Выберите пункт **Условия…**, чтобы открыть меню _Параметры точки останова_:

 [![](debugging-in-xamarin-ios-images/image6vs.png "Меню параметров точки останова")](debugging-in-xamarin-ios-images/image6vs.png#lightbox)

Здесь вы можете ввести условия, при которых эта точка останова активируется

Дополнительные сведения об использовании условных точек останова в более ранних версиях Visual Studio см. в [документации по Visual Studio](https://docs.microsoft.com/visualstudio/debugger/using-breakpoints).

-----

## <a name="navigating-through-code"></a>Переход по коду

При достижении точки останова инструменты отладки позволяют получить контроль над выполнением программы. В вашей IDE появятся четыре кнопки для запуска и пошагового выполнения кода.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

В Visual Studio для Mac они будут выглядеть так:

 [![](debugging-in-xamarin-ios-images/image7.png "Инструменты отладки позволяют разработчику контролировать выполнение программы")](debugging-in-xamarin-ios-images/image7.png#lightbox)

Эти особые значения приведены ниже.

- **Воспроизведение/остановка** — запускает и останавливает выполнение кода до следующей точки останова.
- **Шаг с обходом** — выполняет следующую строку кода. Если она содержит вызов функции, эта функция выполняется, а затем выполнение останавливается на _следующей_ за ней строке.
- **Шаг с заходом** — также выполняет следующую строку кода. Если она является вызовом функции, эта команда останавливается на первой строке функции, позволяя продолжить ее отладку по строкам. Если следующая строка не является функцией, команда работает аналогично шагу с обходом.
- **Шаг с выходом** — возвращается к строке, где была вызвана текущая функция.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В Visual Studio они будут выглядеть так:

[![](debugging-in-xamarin-ios-images/image7vs.png "Инструменты отладки позволяют разработчику контролировать выполнение программы")](debugging-in-xamarin-ios-images/image7vs.png#lightbox)

Эти особые значения приведены ниже.

- **Воспроизведение/остановка** — запускает и останавливает выполнение кода до следующей точки останова.
- **Шаг с обходом (F11)**  — выполняет следующую строку кода. Если она содержит вызов функции, эта функция выполняется, а затем выполнение останавливается на _следующей_ за ней строке.
- **Шаг с заходом (F10)**  — также выполняет следующую строку кода. Если она является вызовом функции, эта команда останавливается на первой строке функции, позволяя продолжить ее отладку по строкам. Если следующая строка не является функцией, команда работает аналогично шагу с обходом.
- **Шаг с выходом (Shift + F11)**  — возвращается к строке, где была вызвана текущая функция.

Дополнительные сведения в об отладке см. в документации по [перемещению по коду с помощью отладчика Visual Studio](https://docs.microsoft.com/visualstudio/debugger/navigating-through-code-with-the-debugger).

-----

### <a name="breakpoints"></a>Точки останова

Необходимо отметить, что iOS дает приложению всего несколько секунд (10) на запуск и выполнение метода `FinishedLaunching` в делегате приложения. Если приложение не выполнит метод в течение 10 секунд, iOS прервет этот процесс.

Это означает, что задать точки останова в коде запуска вашей программы практически невозможно. Если вы хотите выполнить отладку кода запуска, необходимо задержать часть его инициализации и поместить ее в метод, вызываемый таймером, или в любой другой метод обратного вызова, который выполняется после завершения процесса FinishedLaunching.

## <a name="device-diagnostics"></a>Диагностика устройства

Если при настройке отладчика возникает ошибка, включите подробную диагностику, добавив "-v -v -v" к дополнительным аргументам mtouch в параметрах проекта. Это позволит отобразить подробные сведения об ошибке в консоли устройства.

 <a name="WiFi_Debugging" />

## <a name="wireless-debugging"></a>Отладка по беспроводному соединению

По умолчанию Xamarin.iOS использует USB-подключение для отладки приложений на устройствах. Однако иногда при разработке приложений, в которых применяется ExternalEccessory, необходимо использовать USB-устройство для проверки подключения и отключения кабеля. В таких случаях вы можете выполнять отладку по беспроводной сети.

Дополнительные сведения о развертывании и отладке по беспроводному соединению см. в [этом руководстве](~/ios/deploy-test/wireless-deployment.md).

<a name="Technical_Details" />

## <a name="technical-details"></a>Технические сведения

Xamarin.iOS использует новый "мягкий" отладчик Mono. В отличие от стандартного отладчика Mono, который представляет собой программу, управляющую отдельным процессом с помощью интерфейсов операционной системы, "мягкий" отладчик открывает доступ к функциям отладки среды выполнения Mono через соединительный протокол.

При запуске отлаживаемое приложение обращается к отладчику, и отладчик начинает работу. Xamarin.iOS для Visual Studio использует агент Xamarin для Mac в качестве посредника между приложением (в Visual Studio) и отладчиком.

Когда отладка выполняется на устройстве, "мягкий" отладчик обеспечивает тесное взаимодействие с приложением. Это означает, что двоичные файлы сборки, создаваемые в процессе отладки, будут иметь больший размер, так как отладка подразумевает инструментирование с включением дополнительного кода в каждой точке последовательности.

<a name="Accessing_the_Console" />


## <a name="accessing-the-console"></a>Доступ к консоли

Журналы аварийного завершения и выходные данные класса консоли будут отправляться в консоль iPhone. Для получения доступа к этой консоли используйте Organizer (Организатор) в Xcode, выбрав в нем нужное устройство.

Если вы не хотите запускать Xcode, воспользуйтесь [утилитой конфигурации iPhone](http://www.apple.com/support/iphone/enterprise/), которая обеспечивает прямой доступ к консоли. Она также позволяет открывать журналы консоли с компьютера Windows, если вы выполняете отладку в полевых условиях.

Для пользователей Visual Studio в окне вывода доступно несколько журналов, однако для получения более подробных журналов перейдите на компьютер Mac.

-----

<a name="Debugging_Mono's_Class_Libraries" />


## <a name="debugging-monos-class-libraries"></a>Отладка библиотек классов Mono

Xamarin.iOS поставляется с исходным кодом для библиотек классов Mono, что позволяет вам легко изучать внутренние взаимодействия через пошаговое выполнение из отладчика.

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Поскольку эта функция требует больше памяти во время отладки, по умолчанию она отключена.


Чтобы включить ее, снимите флажок **Отладка только кода проекта (без захода в код платформы)** в меню _"Visual Studio для Mac" > "Параметры" > "Отладчик"_, как показано ниже:

[![](debugging-in-xamarin-ios-images/debugging6.png "Отладка библиотек классов Mono")](debugging-in-xamarin-ios-images/debugging6.png#lightbox)

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Для отладки библиотек классов в Visual Studio необходимо снять флажок **Только мой код** в меню _"Отладка" > "Параметры"_. В узле _"Отладка" > "Общие"_ снимите флажок **Включить только мой код**:

[![](debugging-in-xamarin-ios-images/debugging6vs.png "Отладка библиотек классов Mono")](debugging-in-xamarin-ios-images/debugging6vs.png#lightbox)

-----

После этого вы сможете запускать приложение и использовать пошаговое выполнение для любой из базовых библиотек классов Mono.


## <a name="related-links"></a>Связанные ссылки

- [Отладка с помощью Xamarin](/visualstudio/mac/debugging/)
- [Визуализация данных](/visualstudio/mac/data-visualizations/)
- [Задание точки останова](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/set_a_breakpoint)
- [Пошаговое прохождение кода](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/step_through_code)
- [Вывод информации в окно журнала](https://github.com/xamarin/recipes/tree/master/Recipes/cross-platform/ide/debugging/output_information_to_log_window)
