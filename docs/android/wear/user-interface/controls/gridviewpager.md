---
title: GridViewPager
ms.prod: xamarin
ms.assetid: A1CDD5F0-049B-4DFA-A268-8A875D26A675
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/02/2018
ms.openlocfilehash: 81bb4e302f81b58eec91ea2a2aef985adbf72e2c
ms.sourcegitcommit: 4b402d1c508fa84e4fc3171a6e43b811323948fc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/23/2019
ms.locfileid: "61285296"
---
# <a name="gridviewpager"></a>GridViewPager

[GridViewPager](https://developer.xamarin.com/samples/GridViewPager/) образце показано, как реализовать шаблон навигации 2D выбора для Android Wear.

![Пример снимка экрана GridViewPager на квадратным дисплеем](gridviewpager-images/gridviewpager.png)

Сначала добавьте [поддержку Xamarin Android Wear](https://www.nuget.org/packages/Xamarin.Android.Wear/) свой проект пакет NuGet.

Макет XML выглядит следующим образом:

```xml
<android.support.wearable.view.GridViewPager xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/pager"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:keepScreenOn="true" />
```

Создание [`GridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/GridPagerAdapter.html)
(или подкласс, такие как [`FragmentGridPagerAdapter`](https://developer.android.com/reference/android/support/wearable/view/FragmentGridPagerAdapter.html)
для предоставления представления для отображения имени пользователя переход.

[Пример адаптера](https://github.com/xamarin/monodroid-samples/blob/master/wear/GridViewPager/GridViewPager/SimpleGridPagerAdapter.cs) показано, как реализовать требуемые методы, включая переопределения для `RowCount`, `GetColumnCount`, `GetBackground`, и `GetFragment`

Подключите адаптер, как показано:

```csharp
pager.Adapter = new SimpleGridPagerAdapter (this, FragmentManager);
```



## <a name="related-links"></a>Связанные ссылки

- [Google 2D средство выбора документа](https://developer.android.com/training/wearables/ui/2d-picker.html)
- [Документация Android.support.wearable](https://developer.android.com/reference/android/support/wearable/view/package-summary.html)
- [GridViewPager (пример)](https://developer.xamarin.com/samples/GridViewPager/)
