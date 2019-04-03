---
title: Использование собственных библиотек
ms.prod: xamarin
ms.assetid: 7AA6CEC8-C09E-BBDA-FDD6-E40559143548
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 03/09/2018
ms.openlocfilehash: 1b0771a0ccc2597ebd800468b82044e4020d9d94
ms.sourcegitcommit: c4be32ef914465e808d89767c4d5ee72afe93cc6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/02/2019
ms.locfileid: "58854617"
---
# <a name="using-native-libraries"></a>Использование собственных библиотек

Xamarin.Android поддерживает использование собственных библиотек с помощью стандартного механизма PInvoke. Также можно связать дополнительные собственные библиотеки, которые не являются частью операционной системы в вашей apk-файла.

Для развертывания собственной библиотеки с помощью приложения Xamarin.Android, добавьте в проект библиотеку двоичные и задать его **действие при построении** для **AndroidNativeLibrary**.

Чтобы развернуть собственной библиотеки в проекте библиотеки Xamarin.Android, добавьте двоичные библиотеку в проект и задайте его **действие при построении** для **EmbeddedNativeLibrary**.

Обратите внимание на то, что так как Android поддерживает несколько бинарных интерфейсов приложений (ABI), Xamarin.Android необходимо знать какого ABI создана собственная библиотека для.
Это можно сделать двумя способами:

1.  Путь «проверки»
1.  С помощью `AndroidNativeLibrary/Abi` элемент в файле проекта


При сканировании пути имя родительского каталога собственной библиотеки используется для указания целевого ABI библиотеки. Таким образом при добавлении `lib/armeabi/libfoo.so` в проект, затем ABI будет быть «сканируются» как `armeabi`.

Кроме того можно изменить файл проекта, чтобы явно указать интерфейс ABI для использования:

```xml
<ItemGroup>
    <AndroidNativeLibrary Include="path/to/libfoo.so">
        <Abi>armeabi</Abi>
    </AndroidNativeLibrary>
</ItemGroup>
```

Дополнительные сведения об использовании собственных библиотек см. в разделе [взаимодействия с собственными библиотеками](https://www.mono-project.com/docs/advanced/pinvoke/).

## <a name="debugging-native-code-with-visual-studio"></a>Отладка машинного кода с помощью Visual Studio

Если вы используете *Visual Studio 2019* или *Visual Studio 2017*, нет необходимости изменять файлы проекта, как описано выше.
Для построения и отладки C++ в решении Xamarin.Android, добавив ссылку проекта на C++ **динамической общей библиотеки (Android)** проекта.

Чтобы выполнить отладку машинного кода C++ в проекте, выполните следующие действия.

1. Дважды щелкните проект **свойства** и выберите **параметры Android** страницы.
2. Прокрутите вниз до раздела **параметры отладки**.
3. В **отладчик** раскрывающемся меню выберите **C++** (вместо значения по умолчанию **.Net (Xamarin)**).

Visual Studio C++ разработчики могут видеть [SanAngeles_NativeDebug](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/) выборку, чтобы попробовать выполнить отладку C++ от 2019 г. Visual Studio или Visual Studio 2017 с Xamarin; и ссылаться на наш [блога](https://blog.xamarin.com/build-and-debug-c-libraries-in-xamarin-android-apps-with-visual-studio-2015/) Дополнительные сведения.



## <a name="related-links"></a>Связанные ссылки

- [SanAngeles_NativeDebug (пример)](https://developer.xamarin.com/samples/monodroid/SanAngeles_NDK/)
- [Разработка собственных приложений Xamarin Android](https://blogs.msdn.microsoft.com/vcblog/2015/02/23/developing-xamarin-android-native-applications/)
