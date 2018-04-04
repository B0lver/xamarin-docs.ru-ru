---
title: Жизненный цикл приложения
description: Как реагировать на жизненного цикла приложения
ms.prod: xamarin
ms.assetid: 69B416CF-B243-4790-AB29-F030B32465BE
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/19/2016
ms.openlocfilehash: 511591482a0e7512be34f6a210c6f44a1826be24
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="app-lifecycle"></a>Жизненный цикл приложения

`Application` Базовый класс предоставляет следующие возможности:

* [Методы жизненного цикла](#Lifecycle_Methods) `OnStart`, `OnSleep`, и `OnResume`.
* [События переходов модального](#modal) `ModalPushing`, `ModalPushed`, `ModalPopping`, и `ModalPopped`.

<a name="Lifecycle_Methods" />

## <a name="lifecycle-methods"></a>Методы жизненного цикла

`Application` Класс содержит три виртуальные методы, которые могут быть переопределены для обработки методов жизненного цикла:

* **OnStart** -вызывается при запуске приложения.

* **OnSleep** -вызывается каждый раз, приложение выходит в фоновом режиме.

* **OnResume** — вызывается, когда приложение возобновляется после отправки в фоновом режиме.

Следует отметить, что *не* метод для завершения приложения.
В обычных условиях (т. е. не сбоя) завершение приложения будет происходить от *OnSleep* состояния без дополнительных уведомления в код.

Чтобы увидеть, когда эти методы вызываются, реализовать `WriteLine` вызов в каждом (как показано ниже) и тестирование на каждой платформе.

```csharp
protected override void OnStart()
{
    Debug.WriteLine ("OnStart");
}
protected override void OnSleep()
{
    Debug.WriteLine ("OnSleep");
}
protected override void OnResume()
{
    Debug.WriteLine ("OnResume");
}
```

При обновлении *старые* Xamarin.Forms приложений (например) необходимо создать с помощью Xamarin.Forms 1.3 или более ранних версий), убедитесь, что включает Android основных действий `ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation` в `[Activity()]` атрибута. Если это не представлен будет контролироваться `OnStart` метод вызывается для поворота, а также при первом запуске приложения. Этот атрибут автоматически включается в текущее приложение шаблонами Xamarin.Forms.

<a name="modal" />

## <a name="modal-navigation-events"></a>События модальное навигации

Существует четыре новых событий в `Application` класса в Xamarin.Forms 1.4, каждый из которых собственные аргументы событий:

* **ModalPushing** - `ModalPushingEventArgs`
* **ModalPushed** - `ModalPushedEventArgs`
* **ModalPopping** - `ModalPoppingEventArgs` класс содержит `Cancel` свойство. Когда `Cancel` равно `true` модальное pop отменяется.
* **ModalPopped** - `ModalPoppedEventArgs`

Эти события вы сможете лучше управлять жизненного цикла приложения, давая возможность реагировать на модальное страницы отображаются и закрывается.

> [!NOTE]
> Для реализации методов жизненного цикла приложения и события модальное навигации, все предварительной`Application` способы создания приложения Xamarin.Forms (т. е. приложения, написанные в версии 1.2 или более ранних версий, используйте статический `GetMainPage` метод) были обновлены, чтобы создать по умолчанию `Application` , установленного в качестве родительского для `MainPage`.
>
> Xamarin.Forms приложений, использующих к поведению предыдущих версий необходимо обновить до `Application` подкласс, как описано в статье [класс приложения](~/xamarin-forms/app-fundamentals/application-class.md) страницы.
