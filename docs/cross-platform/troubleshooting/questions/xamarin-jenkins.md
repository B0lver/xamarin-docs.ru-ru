---
title: Почему не поддерживается Jenkins с Xamarin
ms.topic: troubleshooting
ms.prod: xamarin
ms.assetid: 9951F980-2C6C-47C0-8A35-A78F06C20BEB
author: asb3993
ms.author: amburns
ms.openlocfilehash: 37fc134f7e97af74f5bb019f3262972273f0c4cf
ms.sourcegitcommit: 0a72c7dea020b965378b6314f558bf5360dbd066
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/09/2018
---
# <a name="why-isnt-jenkins-supported-by-xamarin"></a>Почему не поддерживается Jenkins с Xamarin

## <a name="jenkins-support-explanation"></a>Описание поддержки Jenkins

Jenkins — это набор элементов конфигурации открытым исходным кодом; из-за указанного количества проблем, вызванных непосредственно Jenkins *сам* будет необходимо будет считаться проблем, с которой был взят код; например, [основного репозитория Jenkins](https://github.com/jenkinsci/jenkins), или репозиторий для [ Jenkins.App](https://github.com/stisti/jenkins-app).

Исключением является о проблемах, которые могут быть изолированы от конкретной ошибки в средства Xamarin; Если есть подозрение, что это может быть установлено, можно проверить ваш [варианты поддержки](~/cross-platform/troubleshooting/support-options.md), хотя еще раз, проблема может быть что-нибудь за пределами поддержку Xamarin команды можно *непосредственно* Справка по.

## <a name="setup-jenkins-with-xamarin"></a>Программа установки Jenkins с помощью Xamarin

Как отмечалось выше проблемы Jenkins не поддерживается напрямую с нашей группой; [с помощью Jenkins, с помощью Xamarin](~/tools/ci/jenkins-walkthrough.md) руководства можно использовать для сервер Jenkins CI, интегрированного с помощью Xamarin. 

## <a name="fixes-for-common-issues"></a>Способы решения распространенных проблем
### <a name="jenkins-is-unable-to-find-the-android-sdk"></a>Не удается найти пакет SDK Android Jenkins

Сообщение об ошибке для этой проблемы выглядит следующим образом:

> Ошибка XA5205: не удалось найти каталог пакета SDK для Android. Задайте через /p:AndroidSdkDirectory

Настройка расположения пакета SDK варианты зависят от точного подключаемого модуля Jenkins Android, который вы используете; для поиска как задать это хорошая — руководства подключаемого модуля. Например, [подключаемого модуля Android эмулятор](https://wiki.jenkins-ci.org/display/JENKINS/Android+Emulator+Plugin#AndroidEmulatorPlugin-Systemconfiguration) автоматически ищет SDK, но если его не удается найти; расположение можно также задать через страницу Jenkins конфигурации системы для этого подключаемого модуля. 


## <a name="deprecated-errors"></a>Устаревшие ошибок

> [!IMPORTANT]
> Эта проблема устранена в последних версиях Xamarin. Тем не менее, если эта проблема возникает на последнюю версию программного обеспечения, проверьте файл [новую ошибку](~/cross-platform/troubleshooting/questions/howto-file-bug.md) с помощью полное управление версиями сведения и полный создать выходные данные журнала.



### <a name="jenkins-reports-an-invalid-xamarin-license"></a>Jenkins сообщает недействительную лицензию Xamarin
Сообщения об ошибках для этой проблемы обычно имеют нечто похожее на

> Ошибка XA9008: построение из командной строки, требуется лицензия бизнеса

или

> Ошибка: Starter Edition Xamarin.iOS не поддерживает построение вне Xamarin Studio 

Наиболее распространенной причиной этого сценария является использование Jenkins при входе с помощью учетной записи пользователя, не связанные с лицензию Xamarin. Разрешения, проще всего установить Jenkins как приложение напрямую через учетную запись пользователя. Здесь описаны этого процесса и некоторые дополнительные соображения. [https://forums.xamarin.com/discussion/comment/99397/#Comment_99397](https://forums.xamarin.com/discussion/comment/99397/#Comment_99397)
