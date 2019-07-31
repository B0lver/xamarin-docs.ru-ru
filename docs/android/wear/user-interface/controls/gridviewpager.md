---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: d924bac99b4edc5f41afd024d56fb0c6ace2613d
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68647632"
---
# <a name="gridviewpager"></a>GridViewPager

В образце [гридвиевпажер](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager) показано, как реализовать шаблон навигации для средства выбора 2D для выполнения износа Android.

![Пример снимка экрана Гридвиевпажер на квадратном дисплее](gridviewpager-images/gridviewpager.png)

Сначала добавьте пакет NuGet для [поддержки Xamarin Android](https://www.nuget.org/packages/Xamarin.Android.Wear/) в проект.

XML-файл макета выглядит следующим образом:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Создайте[`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(или подкласс, например[`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
для предоставления представлений, отображаемых при переходе пользователя.

В [примере адаптера](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) показано, как реализовать необходимые методы, включая переопределения для `RowCount`, `GetColumnCount` `GetBackground`, и`GetFragment`

Присоединить адаптер, как показано ниже.

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Связанные ссылки

- [Документ средства выбора 2D в Google](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Документация по Android. support. носимого пользователем](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [Гридвиевпажер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/wear-gridviewpager)
