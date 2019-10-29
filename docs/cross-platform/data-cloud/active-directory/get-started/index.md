---
title: Azure Active Directory
description: В этом документе описаны действия, которые необходимо выполнить, чтобы разрешить проверку подлинности мобильного приложения с помощью Azure Active Directory.
ms.prod: xamarin
ms.assetid: 70B3C2AB-CB4D-420C-9CFA-20CCFA0E3C78
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 18604ffa4980d47149d8feed257460e782f30bee
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016645"
---
# <a name="azure-active-directory"></a>Azure Active Directory

_Регистрация приложения для использования Azure Active Directory_

Azure Active Directory позволяет разработчикам защищать ресурсы, такие как файлы, ссылки и веб-интерфейсы API, используя ту же учетную запись организации, которую сотрудники используют для входа в системы или проверки своих сообщений электронной почты.

Разработка мобильных приложений, которые могут проходить проверку подлинности в Azure Active Directory состоит из трех этапов.
Первые два шага обычно одинаковы, независимо от того, какие службы вы планируете использовать. Третий шаг отличается для каждого типа службы:

  1. [Регистрация на Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) на портале *WindowsAzure.com* , а затем
  2. [Настройка служб](~/cross-platform/data-cloud/active-directory/get-started/configure.md).
  3. Разрабатывайте мобильные приложения с помощью служб.

Примеры различных служб, к которым можно получить доступ:

- [API Graph](~/cross-platform/data-cloud/active-directory/graph.md)
- Веб-интерфейс API
- Office 365

## <a name="conclusion"></a>Заключение

С помощью описанных выше действий можно проверить подлинность мобильных приложений по Azure Active Directory. Библиотека проверки подлинности Active Directory (ADAL) значительно упрощает работу с меньшим количеством строк кода, сохраняя при этом большую часть кода и таким образом делая его общей для разных платформ.

## <a name="related-links"></a>Связанные ссылки

- [Пример Microsoft NativeClient](https://github.com/AzureADSamples/NativeClient-MultiTarget-DotNet)
