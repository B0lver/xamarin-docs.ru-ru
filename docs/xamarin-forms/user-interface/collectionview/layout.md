---
title: Макет CollectionView Xamarin. Forms
description: По умолчанию элементы CollectionView будут отображаться в вертикальном списке. Однако можно указать вертикальные и горизонтальные списки и сетки.
ms.prod: xamarin
ms.assetid: 5FE78207-1BD6-4706-91EF-B13932321FC9
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/22/2019
ms.openlocfilehash: 6c2b3d8bad621db3110fe25041125c5694f21180
ms.sourcegitcommit: eedc6032eb5328115cb0d99ca9c8de48be40b6fa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/07/2020
ms.locfileid: "78917446"
---
# <a name="xamarinforms-collectionview-layout"></a>Макет CollectionView Xamarin. Forms

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства, управляющие макетом.

- [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout)типа [`IItemsLayout`](xref:Xamarin.Forms.IItemsLayout)указывает используемый макет.
- [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy)типа [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy)указывает применяемую стратегию мер элементов.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

По умолчанию в [`CollectionView`](xref:Xamarin.Forms.CollectionView) будут отображаться элементы в вертикальном списке. Однако можно использовать любой из следующих макетов:

- Вертикальный список — список из одного столбца, который увеличивается по вертикали при добавлении новых элементов.
- Горизонтальный список — один список строк, который увеличивается горизонтально по мере добавления новых элементов.
- Вертикальная сетка — сетка с несколькими столбцами, которая растет вертикально по мере добавления новых элементов.
- Горизонтальная сетка — многострочная сетка, которая растет горизонтально по мере добавления новых элементов.

Эти макеты можно указать, задав для свойства [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) значение класс, производный от класса [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) . Этот класс определяет следующие свойства:

- [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation)типа [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation)задает направление, в котором [`CollectionView`](xref:Xamarin.Forms.CollectionView) расширяется по мере добавления элементов.
- [`SnapPointsAlignment`](xref:Xamarin.Forms.ItemsLayout.SnapPointsAlignment)типа [`SnapPointsAlignment`](xref:Xamarin.Forms.SnapPointsAlignment)задает способ выравнивания точек привязки по элементам.
- [`SnapPointsType`](xref:Xamarin.Forms.ItemsLayout.SnapPointsType)типа [`SnapPointsType`](xref:Xamarin.Forms.SnapPointsType)задает поведение точек привязки при прокрутке.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных. Дополнительные сведения о точках привязки см. в разделе [точки привязки](scrolling.md#snap-points) в [CollectionViewе прокрутки Xamarin. Forms](scrolling.md) .

Перечисление [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) определяет следующие члены:

- `Vertical` указывает, что [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет расширяться по вертикали при добавлении элементов.
- `Horizontal` указывает, что [`CollectionView`](xref:Xamarin.Forms.CollectionView) будет расширяться по горизонтали по мере добавления элементов.

Класс `LinearItemsLayout` наследует от класса [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) и определяет свойство `ItemSpacing` типа `double`, представляющее пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0. Класс `LinearItemsLayout` также определяет статические `Vertical` и элементы `Horizontal`. Эти члены можно использовать для создания вертикальных или горизонтальных списков соответственно. Кроме того, можно создать объект `LinearItemsLayout`, указав в качестве аргумента член перечисления [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

Класс [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) наследует от класса [`ItemsLayout`](xref:Xamarin.Forms.ItemsLayout) и определяет следующие свойства:

- `VerticalItemSpacing`, тип `double`, представляющий вертикальное пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0.
- `HorizontalItemSpacing`, тип `double`, представляющий горизонтальное пустое пространство вокруг каждого элемента. Значение этого свойства по умолчанию равно 0, а его значение всегда должно быть больше или равно 0.
- `Span`типа `int`, представляющего число столбцов или строк, отображаемых в сетке. Значение этого свойства по умолчанию равно 1, а его значение всегда должно быть больше или равно 1.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

> [!NOTE]
> для выполнения макета [`CollectionView`](xref:Xamarin.Forms.CollectionView) использует собственные обработчики макетов.

## <a name="vertical-list"></a>Вертикальный список

По умолчанию [`CollectionView`](xref:Xamarin.Forms.CollectionView) отображает свои элементы в вертикальном макете списка. Поэтому нет необходимости устанавливать свойство [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) для использования этого макета:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="Auto" />
                    <RowDefinition Height="Auto" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="Auto" />
                    <ColumnDefinition Width="Auto" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Однако для полноты [`CollectionView`](xref:Xamarin.Forms.CollectionView) можно задать отображение элементов в вертикальном списке, задав свойству [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) значение `VerticalList`.

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="VerticalList">
    ...
</CollectionView>
```

Кроме того, это можно сделать, задав для свойства [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `LinearItemsLayout` объект, указав в качестве значения свойства `Orientation` элемент перечисления `Vertical` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Vertical
};
```

В результате получается список из одного столбца, который по вертикали увеличивается по мере добавления новых элементов:

[![Снимок экрана с вертикальным макетом списка CollectionView на iOS и Android](layout-images/vertical-list.png "Вертикальный макет списка CollectionView")](layout-images/vertical-list-large.png#lightbox "Вертикальный макет списка CollectionView")

## <a name="horizontal-list"></a>Горизонтальный список

[`CollectionView`](xref:Xamarin.Forms.CollectionView) может отображать свои элементы в горизонтальном списке, присвоив свойству [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) значение `HorizontalList`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                ItemsLayout="HorizontalList">
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Кроме того, этот макет можно также выполнить, задав для свойства [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) `LinearItemsLayout` объект, указав в качестве значения свойства `Orientation` элемент перечисления `Horizontal` [`ItemsLayoutOrientation`](xref:Xamarin.Forms.ItemsLayoutOrientation) .

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Horizontal" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = LinearItemsLayout.Horizontal
};
```

В результате получается один список строк, который увеличивается горизонтально по мере добавления новых элементов:

[![Снимок экрана с макетом горизонтального списка CollectionView в iOS и Android](layout-images/horizontal-list.png "CollectionView горизонтальный макет списка")](layout-images/horizontal-list-large.png#lightbox "CollectionView горизонтальный макет списка")

## <a name="vertical-grid"></a>Вертикальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView) могут отображать свои элементы в вертикальной сетке, присвоив свойству [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) значение [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объекта, свойству [`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) которого присвоено значение `Vertical`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="80" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
};
```

По умолчанию вертикальные [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) будут отображать элементы в одном столбце. Однако в этом примере свойству `GridItemsLayout.Span` присваивается значение 2. В результате получается сетка из двух столбцов, которая увеличивается по вертикали при добавлении новых элементов:

[![Снимок экрана с вертикальным макетом сетки CollectionView в iOS и Android](layout-images/vertical-grid.png "Макет вертикальной сетки CollectionView")](layout-images/vertical-grid-large.png#lightbox "Макет вертикальной сетки CollectionView")

## <a name="horizontal-grid"></a>Горизонтальная сетка

[`CollectionView`](xref:Xamarin.Forms.CollectionView) могут отображать свои элементы в горизонтальной сетке, присвоив свойству [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) значение [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объекта, свойству[`Orientation`](xref:Xamarin.Forms.ItemsLayout.Orientation) которого присвоено значение `Horizontal`:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Horizontal"
                        Span="4" />
    </CollectionView.ItemsLayout>
    <CollectionView.ItemTemplate>
        <DataTemplate>
            <Grid Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="35" />
                    <RowDefinition Height="35" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="70" />
                    <ColumnDefinition Width="140" />
                </Grid.ColumnDefinitions>
                <Image Grid.RowSpan="2"
                       Source="{Binding ImageUrl}"
                       Aspect="AspectFill"
                       HeightRequest="60"
                       WidthRequest="60" />
                <Label Grid.Column="1"
                       Text="{Binding Name}"
                       FontAttributes="Bold"
                       LineBreakMode="TailTruncation" />
                <Label Grid.Row="1"
                       Grid.Column="1"
                       Text="{Binding Location}"
                       LineBreakMode="TailTruncation"
                       FontAttributes="Italic"
                       VerticalOptions="End" />
            </Grid>
        </DataTemplate>
    </CollectionView.ItemTemplate>
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(4, ItemsLayoutOrientation.Horizontal)
};
```

По умолчанию горизонтальные [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) будут отображать элементы в одной строке. Однако в этом примере свойству `GridItemsLayout.Span` присваивается значение 4. В результате получается сетка из четырех строк, которая увеличивается горизонтально по мере добавления новых элементов:

[![Снимок экрана с горизонтальным макетом сетки CollectionView в iOS и Android](layout-images/horizontal-grid.png "Горизонтальный макет сетки CollectionView")](layout-images/horizontal-grid-large.png#lightbox "Горизонтальный макет сетки CollectionView")

## <a name="headers-and-footers"></a>Верхние и нижние колонтитулы

[`CollectionView`](xref:Xamarin.Forms.CollectionView) может представлять колонтитулы, которые прокручивается с элементами списка. Верхний и нижний колонтитулы могут быть строками, представлениями или [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектами.

[`CollectionView`](xref:Xamarin.Forms.CollectionView) определяет следующие свойства для указания верхнего и нижнего колонтитулов:

- `Header`типа `object`указывает строку, привязку или представление, которое будет отображаться в начале списка.
- `HeaderTemplate`типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)указывает `DataTemplate`, используемое для форматирования `Header`.
- `Footer`типа `object`указывает строку, привязку или представление, которое будет отображаться в конце списка.
- `FooterTemplate`типа [`DataTemplate`](xref:Xamarin.Forms.DataTemplate)указывает `DataTemplate`, используемое для форматирования `Footer`.

Эти свойства поддерживаются [`BindableProperty`](xref:Xamarin.Forms.BindableProperty) объектами. Это означает, что свойства могут быть целевыми объектами привязок данных.

При добавлении заголовка к макету, который увеличивается горизонтально, слева направо заголовок отображается слева от списка. Аналогично, при добавлении нижнего колонтитула к макету, который увеличивается горизонтально, слева направо нижний колонтитул отображается справа от списка.

### <a name="display-strings-in-the-header-and-footer"></a>Отображение строк в верхнем и нижнем колонтитулах

Свойствам `Header` и `Footer` можно присвоить значения `string`, как показано в следующем примере:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="Monkeys"
                Footer="2019">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    Header = "Monkeys",
    Footer = "2019"
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Этот код приводит к следующим снимкам экрана с заголовком на снимке экрана iOS и нижним колонтитулом, показанным на снимке экрана Android:

[![Снимок экрана: заголовок и нижний колонтитул CollectionView строки в iOS и Android](layout-images/header-footer-string.png "Верхний и нижний колонтитулы строки CollectionView")](layout-images/header-footer-string-large.png#lightbox "Верхний и нижний колонтитулы строки CollectionView")

### <a name="display-views-in-the-header-and-footer"></a>Отображение представлений в верхнем и нижнем колонтитулах

Для каждого свойства `Header` и `Footer` можно задать представление. Это может быть одно представление или представление, содержащее несколько дочерних представлений. В следующем примере показаны свойства `Header` и `Footer`, для каждого из которых задан объект [`StackLayout`](xref:Xamarin.Forms.StackLayout) , содержащий объект [`Label`](xref:Xamarin.Forms.Label) :

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.Header>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Monkeys"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Header>
    <CollectionView.Footer>
        <StackLayout BackgroundColor="LightGray">
            <Label Margin="10,0,0,0"
                   Text="Friends of Xamarin Monkey"
                   FontSize="Small"
                   FontAttributes="Bold" />
        </StackLayout>
    </CollectionView.Footer>
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    Header = new StackLayout
    {
        Children =
        {
            new Label { Text = "Monkeys", ... }
        }
    },
    Footer = new StackLayout
    {
        Children =
        {
            new Label { Text = "Friends of Xamarin Monkey", ... }
        }
    }
};
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Этот код приводит к следующим снимкам экрана с заголовком на снимке экрана iOS и нижним колонтитулом, показанным на снимке экрана Android:

[![Снимок экрана верхнего и нижнего колонтитулов CollectionView с помощью представлений в iOS и Android](layout-images/header-footer-view.png "Верхний и нижний колонтитулы представления CollectionView")](layout-images/header-footer-view-large.png#lightbox "Верхний и нижний колонтитулы представления CollectionView")

### <a name="display-a-templated-header-and-footer"></a>Отображение шаблона верхнего и нижнего колонтитулов

Свойства `HeaderTemplate` и `FooterTemplate` можно задать для [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) объектов, которые используются для форматирования верхнего и нижнего колонтитулов. В этом сценарии свойства `Header` и `Footer` должны быть привязаны к текущему источнику для применения шаблонов, как показано в следующем примере:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}"
                Header="{Binding .}"
                Footer="{Binding .}">
    <CollectionView.HeaderTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Monkeys"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.HeaderTemplate>
    <CollectionView.FooterTemplate>
        <DataTemplate>
            <StackLayout BackgroundColor="LightGray">
                <Label Margin="10,0,0,0"
                       Text="Friends of Xamarin Monkey"
                       FontSize="Small"
                       FontAttributes="Bold" />
            </StackLayout>
        </DataTemplate>
    </CollectionView.FooterTemplate>
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    HeaderTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    }),
    FooterTemplate = new DataTemplate(() =>
    {
        return new StackLayout { };
    })
};
collectionView.SetBinding(ItemsView.HeaderProperty, ".");
collectionView.SetBinding(ItemsView.FooterProperty, ".");
collectionView.SetBinding(ItemsView.ItemsSourceProperty, "Monkeys");
```

Этот код приводит к следующим снимкам экрана с заголовком на снимке экрана iOS и нижним колонтитулом, показанным на снимке экрана Android:

[![Снимок экрана верхнего и нижнего колонтитулов CollectionView с помощью шаблонов в iOS и Android](layout-images/header-footer-template.png "Верхний и нижний колонтитулы шаблона CollectionView")](layout-images/header-footer-template-large.png#lightbox "Верхний и нижний колонтитулы шаблона CollectionView")

## <a name="item-spacing"></a>Промежуток между элементами

По умолчанию каждый элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) не имеет пустого пространства вокруг него. Это поведение можно изменить, задав свойства в макете элементов, который используется `CollectionView`.

Когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) задает для свойства [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) объект `LinearItemsLayout`, свойству `LinearItemsLayout.ItemSpacing` может быть присвоено значение `double`, представляющее пустое пространство вокруг каждого элемента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
        <LinearItemsLayout Orientation="Vertical"
                           ItemSpacing="20" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> Свойство `LinearItemsLayout.ItemSpacing` имеет набор обратного вызова проверки, который гарантирует, что значение свойства всегда больше или равно 0.

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new LinearItemsLayout(ItemsLayoutOrientation.Vertical)
    {
        ItemSpacing = 20
    }
};
```

Этот код приводит к постановке в вертикальный список с одним столбцом с отступом 20 вокруг каждого элемента:

[![Снимок экрана CollectionView с промежутком между элементами в iOS и Android](layout-images/vertical-list-spacing.png "CollectionView между элементами")](layout-images/vertical-list-spacing-large.png#lightbox "CollectionView между элементами")

Когда [`CollectionView`](xref:Xamarin.Forms.CollectionView) задает для свойства [`ItemsLayout`](xref:Xamarin.Forms.StructuredItemsView.ItemsLayout) [`GridItemsLayout`](xref:Xamarin.Forms.GridItemsLayout) объект, свойства `GridItemsLayout.VerticalItemSpacing` и `GridItemsLayout.HorizontalItemSpacing` могут быть установлены в `double` значения, представляющие пустое пространство по вертикали и по горизонтали вокруг каждого элемента:

```xaml
<CollectionView ItemsSource="{Binding Monkeys}">
    <CollectionView.ItemsLayout>
       <GridItemsLayout Orientation="Vertical"
                        Span="2"
                        VerticalItemSpacing="20"
                        HorizontalItemSpacing="30" />
    </CollectionView.ItemsLayout>
    ...
</CollectionView>
```

> [!NOTE]
> Свойства `GridItemsLayout.VerticalItemSpacing` и `GridItemsLayout.HorizontalItemSpacing` имеют набор обратных вызовов проверки, которые гарантируют, что значения свойств всегда больше или равны 0.

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemsLayout = new GridItemsLayout(2, ItemsLayoutOrientation.Vertical)
    {
        VerticalItemSpacing = 20,
        HorizontalItemSpacing = 30
    }
};
```

Этот код приводит к вертикальной сетке с двумя столбцами с вертикальным расстоянием от 20 вокруг каждого элемента, а расстояние по горизонтали — 30 вокруг каждого элемента:

[![Снимок экрана с CollectionView с промежутком между элементами на Android](layout-images/vertical-grid-spacing.png "CollectionView между элементами")](layout-images/vertical-grid-spacing-large.png#lightbox "CollectionView между элементами")

## <a name="item-sizing"></a>Размер элемента

По умолчанию каждый элемент в [`CollectionView`](xref:Xamarin.Forms.CollectionView) по отдельности измеряется и изменяется, при условии, что элементы пользовательского интерфейса в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate) не указывают фиксированные размеры. Это поведение, которое можно изменить, задается значением свойства [`CollectionView.ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) . Это значение свойства может быть задано одним из членов перечисления [`ItemSizingStrategy`](xref:Xamarin.Forms.ItemSizingStrategy) :

- `MeasureAllItems` — каждый элемент измеряется по отдельности. Это значение по умолчанию.
- `MeasureFirstItem` — измеряется только первый элемент, и все последующие элементы получают тот же размер, что и первый элемент.

> [!IMPORTANT]
> Стратегия изменения размера `MeasureFirstItem` приводит к повышению производительности при использовании в ситуациях, когда размер элемента должен быть одинаковым для всех элементов.

В следующем примере кода показано задание свойства [`ItemSizingStrategy`](xref:Xamarin.Forms.StructuredItemsView.ItemSizingStrategy) :

```xaml
<CollectionView ...
                ItemSizingStrategy="MeasureFirstItem">
    ...
</CollectionView>
```

Ниже приведен аналогичный код C#:

```csharp
CollectionView collectionView = new CollectionView
{
    ...
    ItemSizingStrategy = ItemSizingStrategy.MeasureFirstItem
};
```

## <a name="dynamic-resizing-of-items"></a>Динамическое изменение размера элементов

Элементы в [`CollectionView`](xref:Xamarin.Forms.CollectionView) могут динамически изменяться во время выполнения путем изменения связанных с макетом свойств элементов в [`DataTemplate`](xref:Xamarin.Forms.DataTemplate). Например, следующий пример кода изменяет свойства [`HeightRequest`](xref:Xamarin.Forms.VisualElement.HeightRequest) и [`WidthRequest`](xref:Xamarin.Forms.VisualElement.WidthRequest) объекта [`Image`](xref:Xamarin.Forms.Image) :

```csharp
void OnImageTapped(object sender, EventArgs e)
{
    Image image = sender as Image;
    image.HeightRequest = image.WidthRequest = image.HeightRequest.Equals(60) ? 100 : 60;
}
```

Обработчик событий `OnImageTapped` выполняется в ответ на касание объекта [`Image`](xref:Xamarin.Forms.Image) и изменяет размеры изображения, чтобы его было проще Просмотреть:

[![Снимок экрана CollectionView с динамическим изменением размера элемента в iOS и Android](layout-images/runtime-resizing.png "Изменение размера динамического элемента CollectionView")](layout-images/runtime-resizing-large.png#lightbox "Изменение размера динамического элемента CollectionView")

## <a name="right-to-left-layout"></a>Макет с письмом справа налево

[`CollectionView`](xref:Xamarin.Forms.CollectionView) можно разформатировать содержимое в направлении с направлением справа налево, задав для свойства [`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) значение [`RightToLeft`](xref:Xamarin.Forms.FlowDirection.RightToLeft). Однако в идеале свойство `FlowDirection` должно быть установлено на странице или в корневом макете, что приводит к тому, что все элементы на странице или в корневом макете отвечают на направление потока:

```xaml
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="CollectionViewDemos.Views.VerticalListFlowDirectionPage"
             Title="Vertical list (RTL FlowDirection)"
             FlowDirection="RightToLeft">
    <StackLayout Margin="20">
        <CollectionView ItemsSource="{Binding Monkeys}">
            ...
        </CollectionView>
    </StackLayout>
</ContentPage>
```

[`FlowDirection`](xref:Xamarin.Forms.VisualElement.FlowDirection) по умолчанию для элемента с родительским элементом является [`MatchParent`](xref:Xamarin.Forms.FlowDirection.MatchParent). Таким образом, [`CollectionView`](xref:Xamarin.Forms.CollectionView) наследует значение свойства `FlowDirection` из [`StackLayout`](xref:Xamarin.Forms.StackLayout), которое, в свою очередь, наследует значение свойства `FlowDirection` из [`ContentPage`](xref:Xamarin.Forms.ContentPage). В результате отображается макет справа налево, показанный на следующих снимках экрана:

[![Снимок экрана с вертикальным макетом списка справа налево на CollectionView в iOS и Android](layout-images/vertical-list-rtl.png "CollectionView вертикальный макет списка справа налево")](layout-images/vertical-list-rtl-large.png#lightbox "CollectionView вертикальный макет списка справа налево")

Дополнительные сведения о направлении потока см. [в разделе локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md).

## <a name="related-links"></a>Связанные ссылки

- [CollectionView (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-collectionviewdemos/)
- [Локализация справа налево](~/xamarin-forms/app-fundamentals/localization/right-to-left.md)
- [Прокрутка Xamarin. Forms CollectionView](scrolling.md)
