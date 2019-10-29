---
title: Управление фрагментами
ms.prod: xamarin
ms.assetid: 02C5E8F0-32EF-4FD9-DC8B-04650E20722C
ms.technology: xamarin-android
author: davidortinau
ms.author: daortin
ms.date: 02/07/2018
ms.openlocfilehash: 3733ed37abe9604e77529db4864f601d2b473280
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027323"
---
# <a name="managing-fragments"></a>Управление фрагментами

Для упрощения управления фрагментами Android предоставляет класс `FragmentManager`. У каждого действия есть экземпляр `Android.App.FragmentManager`, который будет находить или динамически изменять его фрагменты. Каждый набор этих изменений называется *транзакцией*и выполняется с помощью одного из интерфейсов API, содержащихся в классе `Android.App.FragmentTransation`, который управляется `FragmentManager`. Действие может запустить транзакцию следующего вида:

```csharp
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
```

Эти изменения фрагментов выполняются в экземпляре `FragmentTransaction` с помощью таких методов, как `Add()`, `Remove(),` и `Replace().` изменения затем применяются с помощью `Commit()`. Изменения в транзакции не выполняются немедленно.
Вместо этого они планируются для выполнения в потоке пользовательского интерфейса действия как можно скорее.

В следующем примере показано, как добавить фрагмент в существующий контейнер:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// The fragment will have the ID of Resource.Id.fragment_container.
fragmentTx.Add(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Commit the transaction.
fragmentTx.Commit();
```

Если транзакция фиксируется после вызова `Activity.OnSaveInstanceState()`, будет выдано исключение. Это происходит потому, что когда действие сохраняет свое состояние, Android также сохраняет состояние всех размещенных фрагментов. Если после этой точки фиксируются какие-либо транзакции фрагментов, состояние этих транзакций будет потеряно при восстановлении действия.

Можно сохранить транзакции фрагмента в [стеке](https://developer.android.com/guide/topics/fundamentals/tasks-and-back-stack.html) действия, сделав вызов `FragmentTransaction.AddToBackStack()`. Это позволяет пользователю перемещаться назад через изменения фрагмента при нажатии кнопки **назад** . Без вызова этого метода фрагменты, которые удаляются, будут уничтожены и будут недоступны, если пользователь вернется назад по действию.

В следующем примере показано, как использовать метод `AddToBackStack` `FragmentTransaction` для замены одного фрагмента, сохраняя состояние первого фрагмента в стеке назад:

```csharp
// Create a new fragment and a transaction.
FragmentTransaction fragmentTx = this.FragmentManager.BeginTransaction();
DetailsFragment aDifferentDetailsFrag = new DetailsFragment();

// Replace the fragment that is in the View fragment_container (if applicable).
fragmentTx.Replace(Resource.Id.fragment_container, aDifferentDetailsFrag);

// Add the transaction to the back stack.
fragmentTx.AddToBackStack(null);

// Commit the transaction.
fragmentTx.Commit();
```

## <a name="communicating-with-fragments"></a>Взаимодействие с фрагментами

*Фрагментманажер* знает обо всех фрагментах, присоединенных к действию, и предоставляет два метода, помогающие найти эти фрагменты:

- **Финдфрагментбид** &ndash; этот метод найдет фрагмент, используя идентификатор, указанный в файле макета, или идентификатор контейнера, когда фрагмент был добавлен как часть транзакции.

- **Финдфрагментбитаг** &ndash; этот метод используется для поиска фрагмента, имеющего тег, который был предоставлен в файле макета или был добавлен в транзакцию.

Как фрагменты, так и действия ссылаются на `FragmentManager`, поэтому для обмена данными между ними используются одни и те же методы. Приложение может найти фрагмент ссылки с помощью одного из этих двух методов, привести ссылку на соответствующий тип, а затем напрямую вызвать методы в фрагменте. В следующем фрагменте кода приведен пример.

Кроме того, действие может использовать `FragmentManager` для поиска фрагментов:

```csharp
var emailList = FragmentManager.FindFragmentById<EmailListFragment>(Resource.Id.email_list_fragment);
emailList.SomeCustomMethod(parameter1, parameter2);
```

### <a name="communicating-with-the-activity"></a>Взаимодействие с действием

Фрагмент может использовать свойство `Fragment.Activity` для ссылки на его узел. Приведя действие к более конкретному типу, можно вызвать методы и свойства на своем узле, как показано в следующем примере:

```csharp
var myActivity = (MyActivity) this.Activity;
myActivity.SomeCustomMethod();
```
