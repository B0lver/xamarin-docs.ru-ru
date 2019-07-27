---
title: GridLayout
ms.prod: xamarin
ms.assetid: B69A4BF5-9CFB-443A-9F7B-062D1E498F61
ms.technology: xamarin-android
author: conceptdev
ms.author: crdun
ms.date: 02/06/2018
ms.openlocfilehash: ef965bed5b50402e1659e45c55a02f9382001521
ms.sourcegitcommit: b07e0259d7b30413673a793ebf4aec2b75bb9285
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/26/2019
ms.locfileid: "68511483"
---
# <a name="xamarinandroid-gridlayout"></a>Xamarin. Android GridLayout

— Это новый `ViewGroup` подкласс, который поддерживает размещение представлений в двухмерной сетке аналогично таблице HTML, как показано ниже: `GridLayout`

 [![Обрезанное сообщение GridLayout, отображающее четыре ячейки](grid-layout-images/21-gridlayoutcropped.png)](grid-layout-images/21-gridlayoutcropped.png#lightbox)

 `GridLayout`работает с иерархией с плоским представлением, где дочерние представления задают свои расположения в сетке, указывая строки и столбцы, в которых они должны находиться. Таким образом, *GridLayout* может позиционировать представления в сетке, не требуя, чтобы в промежуточных представлениях была представлена табличная структура, например, в строках таблицы, используемых в таблелайаут. Благодаря поддержке плоской иерархии свойство *GridLayout* может более быстро разметки своих дочерних представлений. Давайте взглянем на пример, иллюстрирующий то, что эта концепция фактически означает в коде.


## <a name="creating-a-grid-layout"></a>Создание макета сетки

Следующий XML-код добавляет `TextView` несколько элементов управления в значение *GridLayout*.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip" />
</GridLayout>
```

Макет изменит размеры строк и столбцов таким образом, чтобы ячейки могли вместить их содержимое, как показано на следующей схеме:

 [![Схема макета, показывающая две ячейки слева меньше, чем справа](grid-layout-images/gridlayout-cells.png)](grid-layout-images/gridlayout-cells.png#lightbox)

При запуске в приложении происходит следующий пользовательский интерфейс:

 [![Снимок экрана: приложение Гридлайаутдемо, отображающее четыре ячейки](grid-layout-images/01-gridlayout.png)](grid-layout-images/01-gridlayout.png#lightbox)



## <a name="specifying-orientation"></a>Указание ориентации

Обратите внимание, `TextView` что в приведенном выше коде XML не указана строка или столбец. Если эти значения не указаны, объект `GridLayout` назначает каждое дочернее представление по порядку на основе ориентации. Например, изменим ориентацию свойства GridLayout с горизонтального на вертикальную по вертикали следующим образом:

```xml
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2"
        android:orientation="vertical">
</GridLayout>
```

Теперь элемент `GridLayout` будет располагать ячейки сверху вниз в каждом столбце, а не слева направо, как показано ниже:

 [![Диаграмма, иллюстрирующая расположение ячеек в вертикальной ориентации](grid-layout-images/gridlayoutorientation.png)](grid-layout-images/gridlayoutorientation.png#lightbox)

Это приводит к следующему пользовательскому интерфейсу во время выполнения:

 [![Снимок экрана Гридлайаутдемо с ячейками, расположенными в вертикальной ориентации](grid-layout-images/02-gridlayout.png)](grid-layout-images/02-gridlayout.png#lightbox)



### <a name="specifying-explicit-position"></a>Указание явной позицией

Если нужно явно управлять положением дочерних представлений в `GridLayout`, можно задать свои `layout_row` атрибуты и `layout_column` . Например, следующий код XML приведет к отображению макета на первом снимке экрана (показанном выше), независимо от ориентации.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="2"
        android:columnCount="2">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="1" />
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="1"
            android:layout_column="1"  />
</GridLayout>
```



### <a name="specifying-spacing"></a>Указание интервала

Существует несколько вариантов, которые будут предоставлять пространство между дочерними представлениями `GridLayout`. Можно использовать `layout_margin` атрибут, чтобы задать поля для каждого дочернего представления напрямую, как показано ниже.

```xml
<TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0"
            android:layout_margin="10dp" />
```

Кроме того, в Android 4 теперь доступен новое представление `Space` с общими шагами общего назначения. Чтобы использовать его, просто добавьте его в качестве дочернего представления.
Например, приведенный ниже XML-код `GridLayout` добавляет дополнительную строку в, присвоив ей `rowcount` значение `Space` 3, и добавляет представление, которое предоставляет интервалы `TextViews`между.

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="3"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"         
            android:layout_height="50dp" />    
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="1" />
</GridLayout>
```

Этот XML-код создает отступ `GridLayout` в, как показано ниже:

 [![Снимок экрана Гридлайаутдемо, иллюстрирующий большие ячейки с отступами](grid-layout-images/03-gridlayout.png)](grid-layout-images/03-gridlayout.png#lightbox)

Преимуществом использования нового `Space` представления является то, что оно позволяет использовать интервалы и не требует от нас устанавливать атрибуты для каждого дочернего представления.



### <a name="spanning-columns-and-rows"></a>Объединение столбцов и строк

`GridLayout` Также поддерживает ячейки, охватывающие несколько столбцов и строк. Например, предположим, что мы добавляем еще одну строку, содержащую кнопку `GridLayout` , как показано ниже:

```xml
<?xml version="1.0" encoding="utf-8"?>
<GridLayout xmlns:android="http://schemas.android.com/apk/res/android"
        android:layout_width="match_parent"
        android:layout_height="match_parent"    
        android:rowCount="4"
        android:columnCount="2"
        android:orientation="vertical">
     <TextView
            android:text="Cell 0"
            android:textSize="14dip"
            android:layout_row="0"
            android:layout_column="0" />
     <TextView
            android:text="Cell 1"
            android:textSize="14dip"
            android:layout_row="0"        
            android:layout_column="1" />
     <Space
            android:layout_row="1"
            android:layout_column="0"
            android:layout_width="50dp"        
            android:layout_height="50dp" />   
     <TextView
            android:text="Cell 2"
            android:textSize="14dip"
            android:layout_row="2"        
            android:layout_column="0" />
     <TextView
            android:text="Cell 3"
            android:textSize="14dip"        
            android:layout_row="2"        
            android:layout_column="1" />
     <Button
            android:id="@+id/myButton"
            android:text="@string/hello"        
            android:layout_row="3"
            android:layout_column="0" />
</GridLayout>
```

Это приведет к тому, что первый столбец `GridLayout` растягивается в соответствии с размером кнопки, как показано здесь:

[![Снимок экрана Гридлайаутдемо с кнопкой, охватывающей только первый столбец](grid-layout-images/04-gridlayout.png)](grid-layout-images/04-gridlayout.png#lightbox)

Чтобы избежать растягивания первого столбца, можно установить кнопку, чтобы она занимала два столбца, задав его ColumnSpan следующим образом:

```xml
<Button
    android:id="@+id/myButton"
    android:text="@string/hello"       
    android:layout_row="3"
    android:layout_column="0"
    android:layout_columnSpan="2" />
```

Это приводит к отображению макета `TextViews` , аналогичного макету, который мы ранее делали, с кнопкой, добавленной в нижнюю `GridLayout` часть, как показано ниже:

 [![Снимок экрана Гридлайаутдемо с кнопкой, охватывающей оба столбца](grid-layout-images/05-gridlayout.png)](grid-layout-images/05-gridlayout.png#lightbox)


## <a name="related-links"></a>Связанные ссылки

- [Гридлайаутдемо (пример)](https://developer.xamarin.com/samples/monodroid/PlatformFeatures/ICS_Samples/GridLayoutDemo/)
- [Знакомство со Южным Сандвичевым](http://www.android.com/about/ice-cream-sandwich/)
- [Платформа Android 4,0](https://developer.android.com/sdk/android-4.0.html)
