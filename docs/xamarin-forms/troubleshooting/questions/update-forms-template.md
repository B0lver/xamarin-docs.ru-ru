---
title: Можно ли обновить Xamarin.Forms шаблон по умолчанию до более нового пакета NuGet?
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 160FBE13-26EB-4B4F-9248-A5CBE58FDD7F
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 04/25/2017
no-loc:
- Xamarin.Forms
- Xamarin.Essentials
ms.openlocfilehash: 9dc5b8e63a35c3a0b797d0794af7a31aea4969c9
ms.sourcegitcommit: 122b8ba3dcf4bc59368a16c44e71846b11c136c5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/30/2020
ms.locfileid: "91555597"
---
# <a name="can-i-update-the-no-locxamarinforms-default-template-to-a-newer-nuget-package"></a>Можно ли обновить Xamarin.Forms шаблон по умолчанию до более нового пакета NuGet?

В этом руководством в Xamarin.Forms качестве примера используется шаблон библиотеки .NET Standard, но один и тот же общий метод также будет работать для Xamarin.Forms шаблона общего проекта. Это руководство написано с примером обновления с Xamarin.Forms 1.5.1.6471 на 2.1.0.6529, но для этого можно задать другие версии по умолчанию.

1. Скопируйте исходный шаблон `.zip` из:

    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\T\PT\Cross-Platform\Xamarin.Forms.PCL.zip`

2. Распакуйте во `.zip` временное расположение.

3. Измените все вхождения старой версии Xamarin.Forms пакета на новую версию, которую вы хотите использовать.
    * `FormsTemplate\FormsTemplate.vstemplate`
    * `FormsTemplate.Android\FormsTemplate.Android.vstemplate`
    * `FormsTemplate.iOS\FormsTemplate.iOS.vstemplate`

    Пример: `<package id="Xamarin.Forms" version="1.5.1.6471" />` -> `<package id="Xamarin.Forms" version="2.1.0.6529" />`.

4. Измените элемент Name основного [файла многопроектного шаблона](/visualstudio/ide/how-to-create-multi-project-templates) ( `Xamarin.Forms.PCL.vstemplate` ), чтобы сделать его уникальным. Пример:

    > `<Name>Blank App (Xamarin.Forms Portable) - 2.1.0.6529</Name>`

5. Повторно заархивировать всю папку шаблона. Обязательно сопоставьте исходную файловую структуру `.zip` файла. `Xamarin.Forms.PCL.vstemplate`Файл должен находиться в верхней части `.zip` файла, а не в папках.

6. Создайте подкаталог "Мобильные приложения" в папке шаблонов Visual Studio для каждого пользователя:
    > `%USERPROFILE%\Documents\Visual Studio 2013\Templates\ProjectTemplates\Visual C#\Mobile Apps`

7. Скопируйте новую ZIP-папку шаблона в новый каталог "Мобильные приложения".

8. Скачайте пакет NuGet, соответствующий версии, из шага 3. Например, [ https://nuget.org/api/v2/package/ Xamarin.Forms /2.1.0.6529](https://nuget.org/api/v2/package/Xamarin.Forms/2.1.0.6529) (см. также [https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file](https://stackoverflow.com/questions/8597375/how-to-get-the-url-of-a-nupkg-file) ) и скопируйте его в соответствующую вложенную папку в папке расширения Xamarin Visual Studio:
    > `C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\IDE\Extensions\Xamarin\Xamarin\[Xamarin Version]\Packages`