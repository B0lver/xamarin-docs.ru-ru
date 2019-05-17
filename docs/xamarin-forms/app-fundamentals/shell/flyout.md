---
title: Всплывающее меню оболочки Xamarin.Forms
description: Всплывающее меню выполняет роль главного меню для приложения оболочки. Его можно вызвать специальным значком или жестом пальцем от края экрана. Всплывающее меню состоит из необязательного заголовка, вложенных элементов всплывающего меню и необязательных пунктов меню.
ms.prod: xamarin
ms.assetid: FEDE51EB-577E-4B3E-9890-B7C1A5E52516
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 05/06/2019
ms.openlocfilehash: 05ce2536c04306c2881ccc5dfa5e2016c9025b11
ms.sourcegitcommit: 9d90a26cbe13ebd106f55ba4a5445f28d9c18a1a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65054494"
---
# <a name="xamarinforms-shell-flyout"></a>Всплывающее меню оболочки Xamarin.Forms

![](~/media/shared/preview.png "Этот API в настоящее время предоставляется в режиме предварительной версии")

[![Скачать пример](~/media/shared/download.png) Скачать пример](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)

Всплывающее меню выполняет роль главного меню для приложения оболочки. Его можно вызвать специальным значком или жестом пальцем от края экрана. Всплывающее меню состоит из необязательного заголовка, вложенных элементов всплывающего меню и необязательных пунктов меню.

![Снимок экрана со всплывающим меню и заметками в оболочке](flyout-images/flyout-annotated.png "Всплывающее меню с заметками")

При необходимости вы можете задать для фона всплывающего меню цвет [`Color`](xref:Xamarin.Forms.Color), используя привязываемое свойство `Shell.FlyoutBackgroundColor`. Это свойство также можно задать из каскадной таблицы стилей (CSS). Подробные сведения см. в статье [об особых свойствах оболочки Xamarin.Forms](~/xamarin-forms/user-interface/styles/css/index.md#xamarinforms-shell-specific-properties).

## <a name="flyout-icon"></a>Значок всплывающего меню

По умолчанию приложения оболочки оснащаются значком "гамбургера", при нажатии которого открывается всплывающее меню. Вы можете изменить этот значок, присвоив привязываемому свойству `Shell.FlyoutIcon` с типом [`ImageSource`](xref:Xamarin.Forms.ImageSource) значение, соответствующее нужному значку:

```xaml
<Shell ...
       FlyoutIcon="flyouticon.png">
    ...       
</Shell>
```

## <a name="flyout-behavior"></a>Поведение всплывающего меню

Всплывающее меню отображается, если нажать кнопку "гамбургер" или провести пальцем от края экрана. Но вы можете изменить это поведение, задав в присоединенном свойстве `Shell.FlyoutBehavior` один из членов перечисления `FlyoutBehavior`:

- `Disabled` указывает, что пользователь не может открывать всплывающее меню;
- `Flyout` указывает, что пользователь может открывать и закрывать всплывающее меню. Это значение по умолчанию для свойства `FlyoutBehavior`;
- `Locked` указывает, что пользователь не может закрыть всплывающее меню, и это меню не перекрывает содержимое.

В следующем примере показано, как отключить всплывающее меню.

```xaml
<Shell ...
       FlyoutBehavior="Disabled">
    ...
</Shell>
```

> [!NOTE]
> Присоединенному свойству `FlyoutBehavior` можно присвоить значение `Shell`, `FlyoutItem`, `ShellContent` или объект страницы, чтобы переопределить поведение всплывающего меню по умолчанию.

Кроме того, всплывающее меню можно открывать и закрывать программным образом, задавая привязываемому свойству `Shell.FlyoutIsPresented` значение `boolean`, которое определяет видимость всплывающего меню на текущий момент:

```csharp
Shell.Current.FlyoutIsPresented = false;
```

## <a name="flyout-header"></a>Заголовок всплывающего меню

Заголовок всплывающего меню — это содержимое, которое при необходимости отображается в верхней части панели; его внешний вид, определяемый `object`, можно задать с помощью значения свойства `Shell.FlyoutHeader`:

```xaml
<Shell.FlyoutHeader>
    <controls:FlyoutHeader />
</Shell.FlyoutHeader>
```

Тип `FlyoutHeader` показан в примере ниже.

```xaml
<ContentView xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             x:Class="Xaminals.Controls.FlyoutHeader"
             HeightRequest="200">
    <Grid BackgroundColor="Black">
        <Image Aspect="AspectFill"
               Source="xamarinstore.jpg"
               Opacity="0.6" />
        <Label Text="Animals"
               TextColor="White"
               FontAttributes="Bold"
               HorizontalTextAlignment="Center"
               VerticalTextAlignment="Center" />
    </Grid>
</ContentView>
```

Вот результат его применения к заголовку всплывающего меню:

![Снимок экрана с заголовком всплывающего меню](flyout-images/flyout-header.png "Заголовок всплывающего меню")

Кроме того, вид заголовка всплывающего меню можно определить через свойство `Shell.FlyoutHeaderTemplate`, присвоив ему значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell.FlyoutHeaderTemplate>
    <DataTemplate>
        <Grid BackgroundColor="Black"
              HeightRequest="200">
            <Image Aspect="AspectFill"
                   Source="xamarinstore.jpg"
                   Opacity="0.6" />
            <Label Text="Animals"
                   TextColor="White"
                   FontAttributes="Bold"
                   HorizontalTextAlignment="Center"
                   VerticalTextAlignment="Center" />
        </Grid>            
    </DataTemplate>
</Shell.FlyoutHeaderTemplate>
```

По умолчанию заголовок всплывающего меню будет зафиксирован во всплывающем элементе, хотя приведенное ниже его содержимое прокручивается в том случае, если элементов много. Тем не менее, это поведение можно изменить, задав в привязываемом свойстве `Shell.FlyoutHeaderBehavior` один из членов перечисления `FlyoutHeaderBehavior`:

- `Default` означает, что для полос прокрутки будет использоваться поведение, установленное для платформы по умолчанию. Это значение по умолчанию для свойства `FlyoutHeaderBehavior`.
- `Fixed` означает, что заголовок всплывающего меню все время остается видимым и не изменяется.
- `Scroll` указывает, что заголовок всплывающего меню пропадает с экрана, прокручиваясь вместе с другими элементами.
- `CollapseOnScroll` указывает, что заголовок всплывающего меню сворачивается до заглавия во время прокрутки элементов.

В следующем примере показано, как свернуть заголовок всплывающего меню при прокрутке пользователем:

```xaml
<Shell ...
       FlyoutHeaderBehavior="CollapseOnScroll">
    ...
</Shell>
```

## <a name="flyout-items"></a>Элементы всплывающего меню

Каждый подкласс объекта `Shell` должен содержать один или несколько объектов `FlyoutItem`, где каждый объект `FlyoutItem` представляет один элемент всплывающего меню. Следующий пример создает всплывающее меню с заголовком и двумя элементами:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <FlyoutItem Title="Cats"
                Icon="cat.png">
        <Tab>
            <ShellContent>
                <views:CatsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
    <FlyoutItem Title="Dogs"
                Icon="dog.png">
        <Tab>
            <ShellContent>
                <views:DogsPage />
            </ShellContent>
        </Tab>
    </FlyoutItem>
</Shell>
```

В этом примере каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage) доступен только через элементы всплывающего меню:

[![Снимок экрана двухстраничного приложения оболочки с элементами всплывающего меню для iOS и Android](flyout-images/two-page-app-flyout.png "Двухстраничное приложение оболочки с элементами всплывающего меню")](flyout-images/two-page-app-flyout-large.png#lightbox "Двухстраничное приложение оболочки с элементами всплывающего меню")

> [!NOTE]
> Если заголовок всплывающего меню отсутствует, элементы всплывающего меню отображаются от самого верха всплывающего меню. В противном случае они отображаются под заголовком всплывающего меню.

Оболочка содержит операторы неявного преобразования, которые позволяют упростить визуальную иерархию оболочки без добавления новых представлений в визуальное дерево. Это возможно благодаря тому, что производный объект `Shell` может содержать только объекты `FlyoutItem`, которые могут содержать только объекты `Tab`, которые могут содержать только объекты `ShellContent`. Эти операторы неявного преобразования позволяют удалить из предыдущего примера объекты `FlyoutItem`, `Tab` и `ShellContent`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       x:Class="Xaminals.AppShell">
    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>
    <views:CatsPage Icon="cat.png" />
    <views:DogsPage Icon="dog.png" />
</Shell>
```

Это неявное преобразование автоматически заключает каждый объект [`ContentPage`](xref:Xamarin.Forms.ContentPage) в объекты `ShellContent`, которые заключаются в объекты `Tab`, которые заключаются в объекты `FlyoutItem`.

> [!IMPORTANT]
> В приложении оболочки каждый [`ContentPage`](xref:Xamarin.Forms.ContentPage), который является дочерним элементом объекта `ShellContent`, создается во время запуска приложения. Такой метод добавления объектов `ShellContent` приводит к созданию дополнительных страниц во время запуска приложения, что может замедлять запуск. Но оболочка может создавать страницы и по требованию, реагируя на переходы. Дополнительные сведения см. в разделе [об эффективности загрузки страниц](tabs.md#efficient-page-loading) в [руководстве по вкладкам оболочки Xamarin.Forms](tabs.md).

### <a name="flyoutitem-class"></a>Класс FlyoutItem

Класс `FlyoutItem` включает следующие свойства, которые определяют его внешний вид и поведение.

- `FlyoutDisplayOptions` с типом `FlyoutDisplayOptions` определяет, как этот элемент и его дочерние элементы отображаются во всплывающем меню. Значение по умолчанию — `AsSingleItem`.
- `CurrentItem` с типом `Tab` обозначает выбранный элемент.
- `Items` с типом `ShellSectionCollection` определяет все вкладки в `FlyoutItem`.
- `FlyoutIcon` с типом `ImageSource` — значок, который используется для элемента. Если это свойство не установлено, по умолчанию ему присваивается значение свойства `Icon`.
- `Icon` с типом `ImageSource` определяет значок, который отображается в частях хрома, не относящихся к всплывающему меню.
- `IsChecked` с типом `boolean` определяет, выделен ли этот элемент во всплывающем элементе в настоящий момент.
- `IsEnabled` с типом `boolean` определяет, можно ли выбрать элемент в хроме.
- `IsTabStop` с типом `bool` указывает, включается ли `FlyoutItem` в навигацию по клавише TAB. Значение по умолчанию — `true`, а если оно равно `false`, элемент `FlyoutItem` игнорируется инфраструктурой навигации по клавише TAB, независимо от значения `TabIndex`.
- `TabIndex` с типом `int` обозначает порядок, в котором объекты `FlyoutItem` получают фокус при переходе пользователя между элементами посредством нажатия клавиши TAB. Это свойство по умолчанию имеет значение 0.
- `Title` с типом `string` определяет заголовок, отображаемый на вкладке в пользовательском интерфейсе.
- `Route` с типом `string` — строка, используемая для адресации элемента.

Все эти свойства, кроме свойства `Route`, поддерживаются объектами [`BindableProperty`](xref:Xamarin.Forms.BindableProperty), то есть их можно указывать в качестве целевых для привязки данных.

> [!NOTE]
> Все объекты `FlyoutItem` в подклассах объекта Shell добавляются в коллекцию `Shell.Items`, которая определяет список элементов, отображаемых во всплывающем меню.

Кроме того, класс `FlyoutItem` предоставляет следующие переопределяемые методы:

- `OnTabIndexPropertyChanged` вызывается при изменении свойства `TabIndex`;
- `OnTabStopPropertyChanged` вызывается при изменении свойства `IsTabStop`;
- `TabIndexDefaultValueCreator` возвращает `int` и позволяет задать значение по умолчанию для свойства `TabIndex`;
- `TabStopDefaultValueCreator` возвращает `bool` и позволяет задать значение по умолчанию для свойства `TabStop`.

## <a name="flyout-display-options"></a>Параметры отображения всплывающего меню

Перечисление `FlyoutDisplayOptions` определяет следующие члены:

- `AsSingleItem` указывает, будет ли элемент отображаться как один элемент;
- `AsMultipleItems` указывает, что сам элемент и его дочерние элементы будут отображаться во всплывающем меню как группа элементов.

Если вы установите для свойства `FlyoutItem.FlyoutDisplayOptions` значение `AsMultipleItems`, будет создан элемент всплывающего меню для каждого `Tab` в `FlyoutItem`:

```xaml
<Shell xmlns="http://xamarin.com/schemas/2014/forms"
       xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
       xmlns:controls="clr-namespace:Xaminals.Controls"
       xmlns:views="clr-namespace:Xaminals.Views"
       FlyoutHeaderBehavior="CollapseOnScroll"
       x:Class="Xaminals.AppShell">

    <Shell.FlyoutHeader>
        <controls:FlyoutHeader />
    </Shell.FlyoutHeader>

    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        <Tab Title="Domestic"
             Icon="paw.png">
            <ShellContent Title="Cats"
                          Icon="cat.png"
                          ContentTemplate="{DataTemplate views:CatsPage}" />
            <ShellContent Title="Dogs"
                          Icon="dog.png"
                          ContentTemplate="{DataTemplate views:DogsPage}" />
        </Tab>
        <ShellContent Title="Monkeys"
                      Icon="monkey.png"
                      ContentTemplate="{DataTemplate views:MonkeysPage}" />
        <ShellContent Title="Elephants"
                      Icon="elephant.png"
                      ContentTemplate="{DataTemplate views:ElephantsPage}" />  
        <ShellContent Title="Bears"
                      Icon="bear.png"
                      ContentTemplate="{DataTemplate views:BearsPage}" />
    </FlyoutItem>

    <ShellContent Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />    
</Shell>
```

В этом примере элементы всплывающего меню создаются для объекта `Tab`, производного от объекта `FlyoutItem`, и объектов `Shellontent`, производных от объекта `FlyoutItem`. Это связано с тем, что каждый объект `ShellContent`, производный от объекта `FlyoutItem`, автоматически упаковывается в объект `Tab`. Кроме того, элемент всплывающего меню создается для последнего объекта `ShellContent`, который упаковывается в объект `Tab`, а затем в объект `FlyoutItem`.

Все это создает такие элементы всплывающего меню:

[![Снимок экрана всплывающего меню с объектами FlyoutItem для iOS и Android](flyout-images/flyout-reduced.png "Всплывающее меню оболочки с объектами FlyoutItem")](flyout-images/flyout-reduced-large.png#lightbox "Всплывающее меню оболочки с объектами FlyoutItem")

## <a name="define-flyoutitem-appearance"></a>Определение внешнего вида FlyoutItem

Внешний вид каждого `FlyoutItem` можно настроить, присвоив свойству `Shell.ItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell ...>
    ...
    <Shell.ItemTemplate>
        <DataTemplate>
            <Grid>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="0.2*" />
                    <ColumnDefinition Width="0.8*" />
                </Grid.ColumnDefinitions>
                <Image Source="{Binding Icon}"
                       Margin="5"
                       HeightRequest="45" />
                <Label Grid.Column="1"
                       Text="{Binding Title}"
                       FontAttributes="Italic"
                       VerticalTextAlignment="Center" />
            </Grid>
        </DataTemplate>
    </Shell.ItemTemplate>
</Shell>
```

Этот пример отображает заголовок каждого объекта `FlyoutItem` курсивом:

[![Снимок экрана шаблонных объектов FlyoutItem для iOS и Android](flyout-images/flyoutitem-templated.png "Шаблонные объекты FlyoutItem в оболочке")](flyout-images/flyoutitem-templated-large.png#lightbox "Шаблонные объекты FlyoutItem в оболочке")

> [!NOTE]
> Оболочка предоставляет свойства `Title` и `Icon` для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в `ItemTemplate`.

## <a name="flyoutitem-tab-order"></a>Последовательность табуляции для FlyoutItem

По умолчанию последовательность табуляции для объектов `FlyoutItem` соответствует порядку, в котором они перечислены в XAML или программно добавлены в дочернюю коллекцию. Этот порядок определяет порядок навигации по элементам `FlyoutItem` с клавиатуры, и часто порядок по умолчанию является оптимальным.

Вы можете изменить установленную по умолчанию последовательность табуляции, установив свойство `FlyoutItem.TabIndex`. Оно обозначает порядок, в котором объекты `FlyoutItem` будут получать фокус при переходе пользователя между элементами путем нажатия клавиши TAB. По умолчанию свойство равно 0, при этом оно может принимать любое значение `int`.

Следующие правила применяются при использовании последовательности табуляции по умолчанию или задании свойства `TabIndex`:

- Объекты `FlyoutItem` со значением 0 для свойства `TabIndex` добавляются в последовательность табуляции в том порядке, в котором они объявлены в XAML или дочерних коллекциях.
- Объекты `FlyoutItem` со значением `TabIndex` больше 0 добавляются в последовательность табуляции с учетом значений `TabIndex`.
- Объекты `FlyoutItem` со значением `TabIndex` меньше 0 добавляются в последовательность табуляции и отображаются раньше, чем любой объект с нулевым значением.
- Конфликты для `TabIndex`, устраняемые в порядке объявления.

Когда последовательность табуляции определена, нажатие клавиши TAB переключает фокус между объектами `FlyoutItem` в порядке возрастания значений `TabIndex`, а после достижения конечного элемента управления возвращает фокус в начало.

Кроме настройки последовательности табуляции для объектов `FlyoutItem`, может потребоваться исключить из этой последовательности некоторые объекты. Это можно сделать с помощью свойства `FlyoutItem.IsTabStop`, которое указывает, включается ли `FlyoutItem` в навигацию по клавише TAB. Значение по умолчанию — `true`, а если оно равно `false`, элемент `FlyoutItem` игнорируется инфраструктурой навигации по клавише TAB, независимо от значения `TabIndex`.

## <a name="set-the-current-flyoutitem"></a>Настройка текущего FlyoutItem

Класс `Shell` имеет привязываемое свойство с именем `CurrentItem` и типом `FlyoutItem`, которое представляет выбранный в данный момент `FlyoutItem`. При первом запуске приложения оболочки это свойство принимает значение первого `FlyoutItem` в производном объекте `Shell`. Но этому свойству можно присвоить значение другого `FlyoutItem`, как показано в следующем примере:

```xaml
<Shell ...
       CurrentItem="{x:Reference aboutItem}">
    <FlyoutItem Title="Animals"
                FlyoutDisplayOptions="AsMultipleItems">
        ...
    </FlyoutItem>
    <ShellContent x:Name="aboutItem"
                  Title="About"
                  Icon="info.png"
                  ContentTemplate="{DataTemplate views:AboutPage}" />
</Shell>
```

Этот код задает объект `ShellContent` с именем `aboutItem` в качестве значения свойства `CurrentItem`, что приводит к его отображению. В нашем примере используется неявное преобразование для помещения объекта `ShellContent` в объект `Tab`, который упаковывается в объект `FlyoutItem`.

Эквивалентный код на C# выглядит так:

```csharp
Shell.Current.CurrentItem = aboutItem;
```

## <a name="menu-items"></a>Пункты меню

Пункты меню могут отображаться во всплывающем меню под элементами всплывающего меню. Каждый пункт меню представлен объектом [`MenuItem`](xref:Xamarin.Forms.MenuItem).

> [!NOTE]
> Класс `MenuItem` имеет событие [`Clicked`](xref:Xamarin.Forms.MenuItem.Clicked) и свойство [`Command`](xref:Xamarin.Forms.MenuItem.Command). Это означает, что объекты `MenuItem` позволяют выполнять действия в ответ на касание `MenuItem`. Сюда относятся такие сценарии, как навигация и открытие веб-браузера на определенной веб-странице.

Коллекция `Shell.MenuItems` определяет список объектов [`MenuItem`](xref:Xamarin.Forms.MenuItem), которые будут отображаться во всплывающем меню. Эту коллекцию можно заполнить объектами `MenuItem`, как показано в следующем примере:

```xaml
<Shell ...
       x:Name="self">
    ...            
    <Shell.MenuItems>
        <MenuItem Text="Random"
                  Icon="random.png"
                  BindingContext="{x:Reference self}"
                  Command="{Binding RandomPageCommand}" />
        <MenuItem Text="Help"
                  Icon="help.png"
                  BindingContext="{x:Reference self}"
                  Command="{Binding HelpCommand}"
                  CommandParameter="https://docs.microsoft.com/xamarin/xamarin-forms/app-fundamentals/shell" />
    </Shell.MenuItems>    
</Shell>
```

Этот код добавляет во всплывающее меню два объекта [`MenuItem`](xref:Xamarin.Forms.MenuItem):

[![Снимок экрана всплывающего меню с объектами MenuItem для iOS и Android](flyout-images/flyout.png "Всплывающее меню оболочки с объектами MenuItem")](flyout-images/flyout-large.png#lightbox "Всплывающее меню оболочки с объектами MenuItem")

Первый объект [`MenuItem`](xref:Xamarin.Forms.MenuItem) выполняет `ICommand` с именем `RandomPageCommand`, который ведет на случайную страницу в приложении. Второй объект `MenuItem` выполняет `ICommand` с именем `HelpCommand`, который открывает в веб-браузере URL-адрес, заданный свойством `CommandParameter`. [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) для каждого `MenuItem` получает значение производного объекта `Shell`.

## <a name="define-menuitem-appearance"></a>Определение внешнего вида MenuItem

Внешний вид каждого `MenuItem` можно настроить, присвоив свойству `Shell.MenuItemTemplate` значение [`DataTemplate`](xref:Xamarin.Forms.DataTemplate):

```xaml
<Shell.MenuItemTemplate>
    <DataTemplate>
        <Grid>
            <Grid.ColumnDefinitions>
                <ColumnDefinition Width="0.2*" />
                <ColumnDefinition Width="0.8*" />
            </Grid.ColumnDefinitions>
            <Image Source="{Binding Icon}"
                   Margin="5"
                   HeightRequest="45" />
            <Label Grid.Column="1"
                   Text="{Binding Text}"
                   FontAttributes="Italic"
                   VerticalTextAlignment="Center" />
        </Grid>
    </DataTemplate>
</Shell.MenuItemTemplate>
```

Этот пример отображает заголовок каждого объекта `MenuItem` курсивом:

[![Снимок экрана шаблонных объектов в MenuItem для iOS и Android](flyout-images/menuitem-templated.png "Шаблонные объекты MenuItem в оболочке")](flyout-images/menuitem-templated-large.png#lightbox "Шаблонные объекты MenuItem в оболочке")

> [!NOTE]
> Оболочка предоставляет свойства [`Text`](xref:Xamarin.Forms.MenuItem.Text) и [`Icon`](xref:Xamarin.Forms.MenuItem.Icon) для [`BindingContext`](xref:Xamarin.Forms.BindableObject.BindingContext) в `MenuItemTemplate`.

## <a name="related-links"></a>Связанные ссылки

- [Xaminals (пример)](https://github.com/xamarin/xamarin-forms-samples/tree/forms40/UserInterface/Xaminals/)
