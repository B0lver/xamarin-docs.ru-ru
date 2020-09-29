---
title: Работа с навигацией watchOS в Xamarin
description: В этом документе описывается работа с навигацией в приложении watchOS. В нем обсуждаются модальные интерфейсы, иерархическая навигация и интерфейсы на основе страниц.
ms.prod: xamarin
ms.assetid: 71A64C10-75C8-4159-A547-6A704F3B5C2E
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 03/17/2017
ms.openlocfilehash: a44d68426ff03ba0b6ab41f57e339caebce62c39
ms.sourcegitcommit: 00e6a61eb82ad5b0dd323d48d483a74bedd814f2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/29/2020
ms.locfileid: "91436709"
---
# <a name="working-with-watchos-navigation-in-xamarin"></a>Работа с навигацией watchOS в Xamarin

Простейший параметр навигации, доступный в контрольном списке, представляет собой простое [модальное всплывающее окно](#modal) , отображаемое поверх текущей сцены.

Для приложений с несколькими сценами доступно две парадигмы навигации:

- [Иерархическая навигация](#Hierarchical_Navigation)
- [Интерфейсы на основе страниц](#Page-Based_Interfaces)

<a name="modal"></a>

## <a name="modal-interfaces"></a>Модальные интерфейсы

Используйте `PresentController` метод, чтобы открыть контроллер интерфейса в модальном режиме. Контроллер интерфейса уже должен быть определен в **интерфейсе. Storyboard**.

```csharp
PresentController ("pageController","some context info");
```

Модально представленные контроллеры используют весь экран (охватывает предыдущую сцену). По умолчанию для заголовка задано значение **Отмена** , и его нажатие приведет к отклонению контроллера.

Чтобы программно закрыть модально представленный контроллер, вызовите `DismissController` .

```csharp
DismissController();
```

Модальные экраны могут быть либо одной сценой, либо использовать макет на основе страниц.

<a name="Hierarchical_Navigation"></a>

## <a name="hierarchical-navigation"></a>Иерархическая навигация

Представляет собой такие сцены, как стек, к которому можно перейти обратно, аналогично тому, как это `UINavigationController` работает в iOS. Монтажные кадры можно помещаться в стек навигации и выключать (программно или по выбору пользователя).

![Сцены можно помещаться в стек навигации](navigation-images/hierarchy-1.png) ![Сцены можно извлекать из стека навигации](navigation-images/hierarchy-2.png)

Как и в iOS, левый-краевый прокрутка переходит назад к родительскому контроллеру в иерархическом стеке навигации.

Примеры [ватчкиткаталог](/samples/xamarin/ios-samples/watchos-watchkitcatalog) и [ватчтаблес](/samples/xamarin/ios-samples/watchos-watchtables) включают иерархическую навигацию.

### <a name="pushing-and-popping-in-code"></a>Отправка и выталкивания в коде

Для контрольного набора не требуется создание контроллера навигации с чрезмерным арчингом, как iOS. просто отправьте контроллер с помощью `PushController` метода, и стек навигации будет создан автоматически.

```csharp
PushController("secondPageController","some context info");
```

Экран контрольного значения будет включать кнопку **назад** в левом верхнем углу, но можно также программно удалить сцену из стека навигации с помощью `PopController` .

```csharp
PopController();
```

Как и в iOS, можно также вернуться к корню стека навигации с помощью `PopToRootController` .

```csharp
PopToRootController();
```

### <a name="using-segues"></a>Использование переходов

Переходов можно создать между сценами в раскадровке, чтобы определить иерархическую навигацию. Чтобы получить контекст для целевой сцены, операционная система вызывает метод `GetContextForSegue` для инициализации нового контроллера интерфейса.

```csharp
public override NSObject GetContextForSegue (string segueIdentifier)
{
  if (segueIdentifier == "mySegue") {
    return new NSString("some context info");
  }
  return base.GetContextForSegue (segueIdentifier);
}
```

<a name="Page-Based_Interfaces"></a>

## <a name="page-based-interfaces"></a>Интерфейсы на основе страниц

Интерфейсы на основе страниц проведите слева направо, аналогично тому, как `UIPageViewController` работает в iOS. Точки индикаторов отображаются вдоль нижней части экрана, чтобы показать, какая страница в настоящий момент отображается.

![Пример первой страницы](navigation-images/paged-1.png) ![Вторая страница образца](navigation-images/paged-2.png) ![Пример пятой страницы](navigation-images/paged-5.png)

Чтобы сделать интерфейс на основе страниц основным ИНТЕРФЕЙСом для вашего приложения наблюдения, используйте `ReloadRootControllers` с массивом контроллеров интерфейса и контекстов:

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
ReloadRootControllers (controllerNames, contexts);
```

Также можно предоставить контроллер на основе страниц, который не является корневым, используя `PresentController` один из других сцен в приложении.

```csharp
var controllerNames = new [] { "pageController", "pageController", "pageController", "pageController", "pageController" };
var contexts = new [] { "First", "Second", "Third", "Fourth", "Fifth" };
PresentController (controllerNames, contexts);
```

## <a name="related-links"></a>Связанные ссылки

- [Ватчкиткаталог (пример)](/samples/xamarin/ios-samples/watchos-watchkitcatalog)
- [Ватчтаблес (пример)](https://developer.xamarin.com//samples/monotouch/watchOS/WatchTables/)