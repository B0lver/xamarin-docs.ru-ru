---
title: Расширение примера RecyclerView
description: Добавление обработчиков событий Click в приложение RecyclerView example.
ms.prod: xamarin
ms.assetid: 707EE1CE-C164-485B-944C-82C6795E8A24
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 07/13/2018
ms.openlocfilehash: fd813427836b0250b84941eca54d6bbe6219518e
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68645337"
---
# <a name="extending-the-recyclerview-example"></a>Расширение примера RecyclerView


Основное приложение, описанное в [базовом RecyclerView примере,](~/android/user-interface/layouts/recycler-view/recyclerview-example.md) фактически &ndash; не выполняет прокрутку и отображает фиксированный список элементов фотографий для упрощения просмотра. В реальных приложениях пользователи хотят иметь возможность взаимодействовать с приложением, коснувшись элементов на экране. Кроме того, базовый источник данных может измениться (или измениться приложением), а содержимое дисплея должно соответствовать этим изменениям. В следующих разделах вы узнаете, как управлять событиями щелчка элемента и обновлять `RecyclerView` при изменении базового источника данных.


### <a name="handling-item-click-events"></a>Обработка событий щелчка элемента

Когда пользователь обращается к элементу в `RecyclerView`, создается событие нажатия кнопки для уведомления приложения о том, какой элемент был затронут. Это событие не создается `RecyclerView` &ndash; вместо этого, представление элемента (которое заключено в заполнитель представления) определяет касания и сообщает о них как о событиях щелчка.

Чтобы продемонстрировать, как можно обработать события нажатия кнопки, выполните следующие действия, чтобы узнать, как будет изменено базовое приложение для просмотра фотографий, чтобы сообщить, какая фотография была затронуты пользователем. При возникновении события щелчка элемента в примере приложения происходит следующая последовательность:

1.  Фотография `CardView` обнаруживает событие щелчка элемента и уведомляет адаптер.

2.  Адаптер перенаправляет событие (с информацией о позиции элемента) в обработчик элемента события Click.

3.  Обработчик нажатия элемента действия реагирует на событие щелчка элемента.

Во `ItemClick` -первых, `PhotoAlbumAdapter` в определение класса добавляется член обработчика событий:

```csharp
public event EventHandler<int> ItemClick;
```

Далее добавляется метод обработчика событий «элемент-щелчок» `MainActivity`в.
Этот обработчик кратко отображает всплывающее уведомление, указывающее, какой элемент фотографии был затронуты:

```csharp
void OnItemClick (object sender, int position)
{
    int photoNum = position + 1;
    Toast.MakeText(this, "This is photo number " + photoNum, ToastLength.Short).Show();
}

```

Далее требуется строка кода для регистрации `OnItemClick` `PhotoAlbumAdapter`обработчика в. Лучше всего сделать это сразу после `PhotoAlbumAdapter` создания: 

```csharp
mAdapter = new PhotoAlbumAdapter (mPhotoAlbum);
mAdapter.ItemClick += OnItemClick;

```

В этом базовом примере регистрация обработчика `OnCreate` выполняется в методе основного действия, но в рабочем приложении может регистрироваться `OnResume` и отменена регистрация обработчика в `OnPause` &ndash; разделе Дополнительные сведения о [жизненном цикле действия](~/android/app-fundamentals/activity-lifecycle/index.md) . об.

`PhotoAlbumAdapter`Теперь будет вызываться `OnItemClick` при получении события щелчка элемента. Следующим шагом является создание обработчика в адаптере, который вызывает это `ItemClick` событие. Следующий метод, `OnClick`, добавляется сразу после `ItemCount` метода адаптера:

```csharp
void OnClick (int position)
{
    if (ItemClick != null)
        ItemClick (this, position);
}
```

Этот `OnClick` метод является прослушивателем адаптера для событий щелчка элемента из представлений элементов. Прежде чем прослушиватель можно будет зарегистрировать в представлении элемента (через заполнитель представления элемента), `PhotoViewHolder` необходимо изменить конструктор, чтобы принять этот метод в качестве дополнительного аргумента, и зарегистрировать `OnClick` событие в представлении `Click` элемента.
Вот измененный `PhotoViewHolder` конструктор:

```csharp
public PhotoViewHolder (View itemView, Action<int> listener)
    : base (itemView)
{
    Image = itemView.FindViewById<ImageView> (Resource.Id.imageView);
    Caption = itemView.FindViewById<TextView> (Resource.Id.textView);

    itemView.Click += (sender, e) => listener (base.LayoutPosition);
}

```

Параметр содержит ссылку на объект `CardView` , затронутый пользователем. `itemView` Обратите внимание, что базовый класс "владелец представления" знает позицию макета элемента`CardView`(), которую он представляет ( `LayoutPosition` через свойство), и эта позиция `OnClick` передается методу адаптера при возникновении события щелчка элемента. `OnCreateViewHolder` Метод адаптера изменяется для передачи `OnClick` метода адаптера конструктору представления.

```csharp
PhotoViewHolder vh = new PhotoViewHolder (itemView, OnClick);
```

Теперь, когда вы создаете и запускаете пример приложения для просмотра фотографий, при нажатии фотографии на экране появится всплывающее уведомление, в котором сообщается об этой фотографии:

[![Пример всплывающего уведомления, отображаемого при касании фотокарты](extending-the-example-images/01-photo-selected-sml.png)](extending-the-example-images/01-photo-selected.png#lightbox)

В этом примере демонстрируется только один подход к реализации обработчиков событий с помощью `RecyclerView`. Другой подход, который можно использовать здесь, — поместить события в держатель представления и подписать адаптер на эти события. Если в примере Photo App `ImageView` была предоставлена возможность редактирования фотографий, то для `TextView` и внутри каждого из них `CardView`потребуются отдельные события. в `TextView` этом случае будет `EditView` запущено диалоговое окно, позволяющее пользователю изменять в заголовке и касании `ImageView` на элементе запустится средство редактирования фотографий, позволяющее пользователю обрезать или вращать фотографию. В зависимости от потребностей приложения необходимо разработать лучший подход к обработке и реагированию на события касания.

Чтобы продемонстрировать, `RecyclerView` как может обновляться набор данных, можно изменить пример приложения для просмотра фотографий, чтобы случайно выбрать фотографию в источнике данных и заменить ее первой фотографией. Во-первых, в пример макета **Main. axml** для фотографии добавляется кнопка **случайного выбора** :

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:orientation="vertical"
    android:layout_width="fill_parent"
    android:layout_height="fill_parent">
    <Button
        android:id="@+id/randPickButton"
        android:layout_width="fill_parent"
        android:layout_height="wrap_content"
        android:gravity="center_horizontal"
        android:textAppearance="?android:attr/textAppearanceLarge"
        android:text="Random Pick" />
    <android.support.v7.widget.RecyclerView
        android:id="@+id/recyclerView"
        android:scrollbars="vertical"
        android:layout_width="fill_parent"
        android:layout_height="fill_parent" />
</LinearLayout>
```

Затем в конце `OnCreate` метода основного действия добавляется код для `Random Pick` нахождение кнопки в макете и присоединения к нему обработчика:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        // Randomly swap a photo with the first photo:
        int idx = mPhotoAlbum.RandomSwap();
    }
};

```

Этот обработчик вызывает `RandomSwap` метод фотоальбома при нажатии кнопки **случайного выбора** . `RandomSwap` Метод случайным образом меняет местами фотографию с первой фотографией в источнике данных, а затем возвращает индекс фотографии с произвольным переключением. При компиляции и запуске примера приложения с помощью этого кода нажатие кнопки случайного **выбора** не приводит к изменению экрана, так как `RecyclerView` не учитывает изменение источника данных.

Чтобы поддерживать `RecyclerView` обновление после изменения источника данных, необходимо изменить обработчик щелчка **случайного выбора** , чтобы `NotifyItemChanged` вызвать метод адаптера для каждого элемента в коллекции, который был изменен (в данном случае два элемента изменились: первая фотография и пересъемная фотография). Это приведет `RecyclerView` к обновлению его отображения, чтобы оно соответствовало новому состоянию источника данных:

```csharp
Button randomPickBtn = FindViewById<Button>(Resource.Id.randPickButton);

randomPickBtn.Click += delegate
{
    if (mPhotoAlbum != null)
    {
        int idx = mPhotoAlbum.RandomSwap();

        // First photo has changed:
        mAdapter.NotifyItemChanged(0);

        // Swapped photo has changed:
        mAdapter.NotifyItemChanged(idx);
    }
};

```

Теперь, когда нажата кнопка **случайного выбора** , `RecyclerView` обновляет отображение, чтобы показать, что фото, расположенное в коллекции, заменено первым фото в коллекции:

[![Первый снимок экрана перед переключением, второй снимок экрана после переключения](extending-the-example-images/02-random-pick-sml.png)](extending-the-example-images/02-random-pick.png#lightbox)

Конечно, `NotifyDataSetChanged` можно было бы вызвать, вместо того чтобы совершать два `NotifyItemChanged`вызова, но при этом произойдет принудительное `RecyclerView` обновление всей коллекции, несмотря на то, что были изменены только два элемента в коллекции. Вызов `NotifyItemChanged` гораздо эффективнее, чем вызов `NotifyDataSetChanged`.


## <a name="related-links"></a>Связанные ссылки

- [Рециклервиевер (пример)](https://docs.microsoft.com/samples/xamarin/monodroid-samples/android50-recyclerviewer)
- [RecyclerView](~/android/user-interface/layouts/recycler-view/index.md)
- [RecyclerView части и функциональные возможности](~/android/user-interface/layouts/recycler-view/parts-and-functionality.md)
- [Пример базового RecyclerView](~/android/user-interface/layouts/recycler-view/recyclerview-example.md)
- [RecyclerView](https://developer.android.com/reference/android/support/v7/widget/RecyclerView.html)
