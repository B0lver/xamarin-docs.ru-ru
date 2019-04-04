---
title: Переносимые Visual Basic.NET
description: В этом руководстве объясняется, как Visual Basic можно использовать для записи проекты переносимой библиотеки классов (PCL), которые могут использоваться в решениях, предназначенных для Xamarin.iOS и Xamarin.Android.
ms.prod: xamarin
ms.assetid: f264c632-8feb-4015-a5e5-cb9c681c787d
author: asb3993
ms.author: amburns
ms.date: 03/23/2017
ms.openlocfilehash: e4c8c43b4df1a7bfc5436f14564c6d0164216c46
ms.sourcegitcommit: 57e8a0a10246ff9a4bd37f01d67ddc635f81e723
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57669249"
---
# <a name="portable-visual-basicnet"></a>Переносимые Visual Basic.NET

Проекты Xamarin iOS и Android не поддерживаются Visual Basic; Однако разработчики могут использовать переносимые библиотеки классов для миграции существующего кода Visual Basic для iOS и Android, или записи значительную часть логики своих приложений в Visual Basic. Приложения Xamarin.Forms можно создавать полностью в Visual Basic (за исключением фонового кода XAML, пользовательские модули подготовки отчетов и служб зависимостей).

## <a name="requirements"></a>Требования

Поддержка библиотек переносимый класс была добавлена в Xamarin.Android 4.10.1, Xamarin.iOS 7.0.4 и Xamarin Studio 4.2, это означает, что все проекты Xamarin, созданных с помощью этих средств можно включить сборок переносимой библиотеки Классов Visual Basic.

Создание и компиляция переносимые библиотеки классов Visual Basic необходимо использовать Visual Studio на Windows (Visual Studio 2012 или более поздняя).

> [!NOTE]
> Библиотеки Visual Basic можно создавать только и скомпилирован с использованием Visual Studio. Xamarin.iOS и Xamarin.Android не поддерживает язык Visual Basic.
>
> Если вы работаете только в Visual Studio могут ссылаться на проект Visual Basic проектов Xamarin.iOS и Xamarin.Android.
>
> Если ваши проекты iOS и Android должен также быть загружен в Visual Studio для Mac вы должны ссылаться на выходной сборки переносимой библиотеки Классов Visual Basic.


## <a name="creating-a-visual-basicnet-pcl"></a>Создание Visual Basic.NET PCL

В этом разделе описано, как создать Visual Basic переносимой библиотеки классов с помощью Visual Studio.
Затем библиотеку можно ссылаться в других проектах, в том числе приложения Xamarin.iOS, Xamarin.Android и Xamarin.Forms.

### <a name="creating-a-pcl"></a>Создание PCL

При добавлении переносимую библиотеку Классов Visual Basic в Visual Studio необходимо выбрать профиль, который описывает, какие платформы библиотеки должны быть совместимы с. Профили описаны во введении к документу переносимой библиотеки Классов.

Ниже приведены действия, чтобы создать переносимую библиотеку Классов и выберите его профиль.

1.  В **новый проект** выберите **Visual Basic > Библиотека классов (переносимая)** параметр:

    [![](images/image1-sml.png "Создание нового переносимой библиотеки Visual Basic")](images/image1.png#lightbox)

1.  Visual Studio будет немедленно предложено со следующими **Добавление переносимой библиотеки классов** диалоговое окно, чтобы профиль можно настроить. Установка флажка платформы, необходимые для поддержки и нажмите клавишу **ОК**.

    [![](images/image2-sml.png "Выберите профиль PCL, выбор платформ")](images/image2.png#lightbox)

1.  Проект переносимой библиотеки Классов Visual Basic будет отображаться, как показано в **обозревателе решений** следующим образом:

    [![](images/image3-sml.png "Пустой проект переносимой библиотеки Классов Visual Studio")](images/image3.png#lightbox)


Переносимая библиотека Классов теперь готов для кода Visual Basic для добавления. Проекты переносимой библиотеки Классов можно ссылаться в других проектах (проекты приложений, проекты библиотек и даже в других проектах переносимой библиотеки Классов).

### <a name="editing-the-pcl-profile"></a>Изменение профиля PCL

Профиль PCL, (который определяет, какие платформы, Переносимая библиотека Классов совместима с) можно просмотреть и изменить, щелкнув правой кнопкой мыши на проект и выбрав **свойства > Библиотека > изменение...** . На этом снимке экрана показан итоговый диалогового окна:

 [![](images/image4-sml.png "Изменение профиля PCL в свойствах проекта")](images/image4.png#lightbox)

Если профиль изменяется после кода уже был добавлен в переносимую библиотеку классов, это возможно, что библиотека больше не будут компилироваться, если код ссылается на функции, которые не являются частью вновь выбранный профиль.


## <a name="summary"></a>Сводка

В этой статье показано, как использовать код Visual Basic в приложениях Xamarin, с помощью Visual Studio и переносимых библиотек классов. Несмотря на то, что Xamarin не поддерживает напрямую Visual Basic, компиляции Visual Basic в переносимую библиотеку Классов позволяет код, созданный с помощью Visual Basic должны быть включены в приложениях iOS и Android.

Ниже описывается использование Visual Basic.NET PCL в собственном режиме или приложений Xamarin.Forms:

- [Создания собственных приложений Xamarin.iOS и Xamarin.Android, использующих VB](native-apps.md)
- [Создание приложений Xamarin.Forms с помощью VB](xamarin-forms.md)


## <a name="related-links"></a>Связанные ссылки

- [TaskyPortableVB (пример)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/TaskyPortableVB)
- [XamarinFormsVB (пример)](https://github.com/xamarin/mobile-samples/tree/master/VisualBasic/XamarinFormsVB)
- [Кроссплатформенная разработка в .NET Framework (Microsoft)](https://msdn.microsoft.com/library/gg597391(v=vs.110).aspx)
