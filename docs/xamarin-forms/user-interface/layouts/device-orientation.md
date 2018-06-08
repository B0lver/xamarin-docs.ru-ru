---
title: Ориентации устройства
description: Понять, как для размещения приложения, которые прекрасно выглядят в книжной и альбомной ориентации.
ms.prod: xamarin
ms.assetid: 11A1D327-2DF3-4F3B-810D-6C95B71D27B2
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 12/09/2015
ms.openlocfilehash: 95049e2d44b369bfa879226e17d2bad20b7b5408
ms.sourcegitcommit: d80d93957040a14b4638a91b0eac797cfaade840
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/07/2018
ms.locfileid: "34848139"
---
# <a name="device-orientation"></a>Ориентации устройства

Важно учитывать, как приложение будет использоваться и как альбомная ориентация могут быть включены для повышения удобства работы пользователей. Отдельные макеты можно специально приспособленная для хранения нескольких ориентации и лучше всего использовать доступное пространство. На уровне приложения можно отключать и включать поворота.

В этой статье подробно рассматривается создание приложений с использованием функции ориентации устройства и содержит следующие подразделы:

- **[Управление Ориентация](#Controlling_Orientation)**  &ndash; понять, как управлять ориентацией на уровне приложений по каждой платформы.
- **[Отклик на изменения в ориентации](#Reacting_to_Changes_in_Orientation)**  &ndash; научиться получать уведомления о и реагировать на, изменяет ориентацию.
- **[Динамический макет](#Responsive_Layout)**  &ndash; сведения о создании макетов, которые автоматически работайте с альбомной и портретной ориентации.

<a name="Controlling_Orientation" />

## <a name="controlling-orientation"></a>Управление ориентации

Используя Xamarin.Forms, поддерживаемый метод управления ориентацией устройства — использовать параметры для каждого отдельного проекта.

### <a name="ios"></a>iOS

В iOS, ориентации устройства настроена для приложений, использующих **Info.plist** файла. Этот файл будет содержать параметры ориентации для iPhone и iPod, а также параметры для iPad, если приложение включает его в качестве целевого объекта. Ниже приведены инструкции, относящиеся к IDE. Используйте параметры интегрированной среды разработки в верхней части этого документа, чтобы выбрать какие инструкции, которые вы хотите проверить.

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

В Visual Studio откройте проект iOS и откройте **Info.plist**. Файл откроется в новой панели настройки, начиная с вкладкой iPhone данных развертывания:

![iPhone данных развертывания в Visual Studio](device-orientation-images/orientation-vs-iphone.png)

Чтобы изменить ориентацию iPad, выделите **iPad данных развертывания** в верхней левой части панели, затем выберите из доступных ориентации:

![Поддерживаемое устройство ориентации в Visual Studio](device-orientation-images/orientation-vs-ipad.png)

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

В Visual Studio для Mac, откройте проект iOS и откройте **Info.plist**. В разделе **приложения** вкладку, разделы можно найти задание ориентации:

![iPhone данных развертывания в Visual Studio для Mac](device-orientation-images/orientation-xam-ui.png)

Если вы хотите использовать для изменения значений, с помощью интерфейса редактора значений ключа, выберите **источника**> вкладку в нижней части экрана:

![Поддерживаемые ориентации устройства в Visual Studio для Mac](device-orientation-images/orientation-xam-source.png)

-----

### <a name="android"></a>Android

Чтобы управлять ориентацией на Android, откройте **MainActivity.cs** и установите ориентацию с помощью декорирования атрибут `MainActivity` класса:

```csharp
namespace MyRotatingApp.Droid
{
    [Activity (Label = "MyRotatingApp.Droid", Icon = "@drawable/icon", MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation,
    ScreenOrientation = ScreenOrientation.Landscape)] //This is what controls orientation
    public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
    {
        protected override void OnCreate (Bundle bundle)
...
```

Xamarin.Android поддерживает несколько параметров для указания ориентации:

- **Альбомная ориентация** &ndash; принудительно ориентации приложения на альбомная вне зависимости от данных датчика.
- **Книжная** &ndash; принудительно ориентации приложения на Книжная, вне зависимости от данных датчика.
- **Пользователь** &ndash; вызывает приложения должны быть представлены с помощью предпочтительная ориентация пользователя.
- **За** &ndash; вызывает ориентации приложения должен совпадать с ориентацию [действия](https://developer.xamarin.com/api/type/Android.App.Activity/) стоящий за ней.
- **Датчик** &ndash; вызывает ориентация приложения будет определяться датчика, даже если пользователь отключает автоматический поворот.
- **SensorLandscape** &ndash; предписывает приложению использовать альбомная ориентация при использовании датчиков для изменения направления экрана испытывает (благодаря чему экрана не рассматривать как направленные острым концом вниз).
- **SensorPortrait** &ndash; предписывает приложению использовать книжной ориентации при использовании датчиков для изменения направления экрана испытывает (благодаря чему экрана не рассматривать как направленные острым концом вниз).
- **ReverseLandscape** &ndash; предписывает приложению использовать альбомная ориентация с выходом в обратном направлении из обычного, таким образом, чтобы отображаться «сверху вниз.»
- **ReversePortrait** &ndash; предписывает приложению использовать книжной ориентации, с выходом в обратном направлении из обычного, таким образом, чтобы отображаться «сверху вниз.»
- **FullSensor** &ndash; предписывает приложению использовать данные датчика для выбора правильной ориентации (из возможных 4).
- **FullUser** &ndash; предписывает приложению использовать персональные настройки пользователя ориентации. Если включен автоматический поворот, можно использовать все 4 ориентации.
- **UserLandscape** &ndash; _\[не поддерживается\]_ вызывает приложение для использования альбомной ориентации, если у пользователя нет автоматический поворот включен, в этом случае он будет использовать датчик для определения ориентации. Этот параметр приведет к разрыву компиляции.
- **UserPortrait** &ndash; _\[не поддерживается\]_ предписывает приложению использовать книжной ориентации, если у пользователя нет автоматический поворот включен, в этом случае он будет использовать датчик для определения ориентации. Этот параметр приведет к разрыву компиляции.
- **Блокировки** &ndash; _\[не поддерживается\]_ предписывает приложению использовать ориентацию экрана, то при запуске, без ответа на изменения в устройство физического Ориентация. Этот параметр приведет к разрыву компиляции.

Обратите внимание, что собственных API Android обеспечивают значительную степень контроля над управление ориентации, включая параметры, которые явно указана другая пользователя выражено предпочтения.

### <a name="universal-windows-platform"></a>Универсальная платформа Windows

В универсальной платформы Windows (UWP), поддерживаемые ориентации задаются в **Package.appxmanifest** файла. Открытия манифеста раскроют панель конфигурации, где можно выбрать поддерживаемые ориентации.

<a name="Reacting_to_Changes_in_Orientation" />

## <a name="reacting-to-changes-in-orientation"></a>Отклик на изменения в ориентации

Xamarin.Forms не предоставляет все собственные события для уведомления приложения изменение ориентации в общем коде. Тем не менее `SizeChanged` событие `Page` применяется при ширины или высоты `Page` изменения. Если ширина `Page` больше, чем высота устройство находится в режиме альбомной ориентации. Дополнительные сведения см. в разделе [выводит изображение, в зависимости от ориентации экрана](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/).

> [!NOTE]
> Нет существующих, свободного пакет NuGet для получения уведомлений об изменениях ориентацию в общем коде. В разделе [в репозитории GitHub](https://github.com/aliozgur/Xamarin.Plugins/tree/master/DeviceOrientation) для получения дополнительной информации.

Кроме того, можно переопределить [ `OnSizeAllocated` ](https://developer.xamarin.com/api/member/Xamarin.Forms.Page.OnSizeAllocated(System.Double,System.Double)/) метод `Page`, вставка любого макета изменить логику. `OnSizeAllocated` Метод вызывается всякий раз, когда `Page` выделяется новый размер, который происходит whenver при повороте устройства. Обратите внимание, что базовая реализация `OnSizeAllocated` выполняет функции важные макета, поэтому необходимо вызвать базовую реализацию в переопределении:

```csharp
protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
}
```

Невозможность выполнения этого шага приведет к сбою страницы.

Обратите внимание, что `OnSizeAllocated` метод может вызываться несколько раз при повороте устройства. Изменение макета при каждом трате ресурсов и может привести к мерцание. Рассмотрите возможность использования переменной экземпляра на странице для отслеживания, является ли ориентация в горизонтальной или вертикальной и перерисовывать только при наличии изменений:

```csharp
private double width = 0;
private double height = 0;

protected override void OnSizeAllocated(double width, double height)
{
    base.OnSizeAllocated(width, height); //must be called
    if (this.width != width || this.height != height)
    {
        this.width = width;
        this.height = height;
        //reconfigure layout
    }
}
```

После обнаружения изменений в ориентации устройства может потребоваться добавить или удалить дополнительные представления из пользовательского интерфейса для реагирования на изменение в доступное пространство. Например рассмотрим встроенных калькулятора на каждой из платформ в книжной ориентации:

![](device-orientation-images/calculator-portrait.png "Калькулятор в книжной ориентации")

и альбомной ориентации.

![](device-orientation-images/calculator-landscape.png "Калькулятор в альбомной ориентации")

Обратите внимание, что приложения воспользоваться дискового пространства путем добавления дополнительных функциональных возможностей в альбомной ориентации.

<a name="Responsive_Layout" />

## <a name="responsive-layout"></a>Динамический макет

Существует возможность разработки интерфейсов с помощью встроенных макетов, чтобы они корректно переход при повороте устройства. При проектировании интерфейсы, которые будут продолжать быть привлекательным, чтобы реагировать на изменения в ориентации учитывайте следующие общие правила:

- **Обратите внимание на коэффициенты** &ndash; изменения в ориентации может вызвать проблемы, при определенных предположений в отношении коэффициенты. Например представление, которое будет иметь достаточно места в 1/3 расстояние по вертикали в книжной ориентации экрана может не поместиться в 1/3 расстояние по вертикали в альбомной ориентации.
- **Будьте внимательны с абсолютными значениями** &ndash; абсолютные (пикселей) значения, которые имеют смысл в книжной ориентации не имеет смысла в альбомной ориентации. При необходимости абсолютные значения используйте вложенных макетов, чтобы изолировать их влияние. Например, было бы разумно использовать абсолютные значения в `TableView` `ItemTemplate` когда шаблон элемента имеет гарантированную универсальный высоту.

Приведенные выше правила также применяются при реализации интерфейсов нескольких размеров и являются обычно считаются рекомендации. Остальная часть в этом руководстве объясняется, конкретные примеры макетов отвечать на запросы, используя каждый из первичного макетов в Xamarin.Forms.

> [!NOTE]
> Для ясности в следующих разделах демонстрируются способы реализации макетов отвечать на запросы, с помощью только одного типа `Layout` одновременно. На практике часто бывает проще использовать разные `Layout`s для достижения нужного макета с помощью простых и интуитивно `Layout` для каждого компонента.

### <a name="stacklayout"></a>StackLayout

Рассмотрим следующее приложение, отображаемых в книжной ориентации.

![](device-orientation-images/photo-stack-portrait.png "Приложение фото в книжной ориентации")

и альбомной ориентации.

![](device-orientation-images/photo-stack-landscape.png "Приложение фото в альбомной ориентации")

Что достигается следующим кодом XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.StackLayoutPageXaml"
Title="Stack Photo Editor - XAML">
    <ContentPage.Content>
        <StackLayout Spacing="10" Padding="5" Orientation="Vertical"
        x:Name="outerStack"> <!-- can change orientation to make responsive -->
            <ScrollView>
                <StackLayout Spacing="5" HorizontalOptions="FillAndExpand"
                    WidthRequest="1000">
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Name: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer.jpg"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Date: " WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="07/05/2015"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Label Text="Tags:" WidthRequest="75"
                            HorizontalOptions="Start" />
                        <Entry Text="deer, tiger"
                            HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                    <StackLayout Orientation="Horizontal">
                        <Button Text="Save" HorizontalOptions="FillAndExpand" />
                    </StackLayout>
                </StackLayout>
            </ScrollView>
            <Image  Source="deer.jpg" />
        </StackLayout>
    </ContentPage.Content>
</ContentPage>
```

C# позволяет изменить ориентацию `outerStack` зависимости от ориентации устройства:

```csharp
protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            outerStack.Orientation = StackOrientation.Horizontal;
        } else {
            outerStack.Orientation = StackOrientation.Vertical;
        }
    }
}
```

Обратите внимание на следующее условия:

- `outerStack` настраивается для отображения изображений и элементов управления в виде горизонтальной или вертикальной стека в зависимости от ориентации, чтобы лучше всего воспользоваться преимуществами доступное пространство.


### <a name="absolutelayout"></a>AbsoluteLayout

Рассмотрим следующее приложение, отображаемых в книжной ориентации.

![](device-orientation-images/photo-abs-portrait.png "Приложение фото в книжной ориентации")

и альбомной ориентации.

![](device-orientation-images/photo-abs-landscape.png "Приложение фото в альбомной ориентации")

Что достигается следующим кодом XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.AbsoluteLayoutPageXaml"
Title="AbsoluteLayout - XAML" BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <AbsoluteLayout>
            <ScrollView AbsoluteLayout.LayoutBounds="0,0,1,1"
                AbsoluteLayout.LayoutFlags="PositionProportional,SizeProportional">
                <AbsoluteLayout>
                    <Image Source="deer.jpg"
                        AbsoluteLayout.LayoutBounds=".5,0,300,300"
                        AbsoluteLayout.LayoutFlags="PositionProportional" />
                    <BoxView Color="#CC1A7019" AbsoluteLayout.LayoutBounds=".5
                        300,.7,50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" />
                    <Label Text="deer.jpg" AbsoluteLayout.LayoutBounds = ".5
                        310,1, 50" AbsoluteLayout.LayoutFlags="XProportional
                        WidthProportional" HorizontalTextAlignment="Center" TextColor="White" />
                </AbsoluteLayout>
            </ScrollView>
            <Button Text="Previous" AbsoluteLayout.LayoutBounds="0,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional"
                BackgroundColor="White" TextColor="Green" BorderRadius="0" />
            <Button Text="Next" AbsoluteLayout.LayoutBounds="1,1,.5,60"
                AbsoluteLayout.LayoutFlags="PositionProportional
                    WidthProportional" BackgroundColor="White"
                    TextColor="Green" BorderRadius="0" />
        </AbsoluteLayout>
    </ContentPage.Content>
</ContentPage>
```

Обратите внимание на следующее условия:

- Таким образом макета страницы нет необходимости для процедурный код, чтобы вызвать скорости реагирования.
- `ScrollView` Используется, чтобы разрешить метку, которая должна быть видимыми, даже если высота экрана не меньше, чем сумма фиксированной высоты кнопок и изображения.


### <a name="relativelayout"></a>RelativeLayout

Рассмотрим следующее приложение, отображаемых в книжной ориентации.

![](device-orientation-images/photo-rel-portrait.png "Приложение фото в книжной ориентации")

и альбомной ориентации.

![](device-orientation-images/photo-rel-landscape.png "Приложение фото в альбомной ориентации")

Что достигается следующим кодом XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.RelativeLayoutPageXaml"
Title="RelativeLayout - XAML"
BackgroundImage="deer.jpg">
    <ContentPage.Content>
        <RelativeLayout x:Name="outerLayout">
            <BoxView BackgroundColor="#AA1A7019"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}" />
            <ScrollView
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=1}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=0,Constant=0}">
                <RelativeLayout>
                    <Image Source="deer.jpg" x:Name="imageDeer"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.8}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=.1}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=10}" />
                    <Label Text="deer.jpg" HorizontalTextAlignment="Center"
                        RelativeLayout.WidthConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=1}"
                        RelativeLayout.HeightConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Height,Factor=0,Constant=75}"
                        RelativeLayout.XConstraint="{ConstraintExpression
                            Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                        RelativeLayout.YConstraint="{ConstraintExpression
                            Type=RelativeToView,ElementName=imageDeer,Property=Height,Factor=1,Constant=20}" />
                </RelativeLayout>

            </ScrollView>

            <Button Text="Previous" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=0}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                 />
            <Button Text="Next" BackgroundColor="White" TextColor="Green" BorderRadius="0"
                RelativeLayout.XConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                RelativeLayout.YConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Height,Factor=1,Constant=-60}"
                RelativeLayout.HeightConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=0,Constant=60}"
                RelativeLayout.WidthConstraint="{ConstraintExpression
                    Type=RelativeToParent,Property=Width,Factor=.5}"
                />
        </RelativeLayout>
    </ContentPage.Content>
</ContentPage>

```

Обратите внимание на следующее условия:

- Таким образом макета страницы нет необходимости для процедурный код, чтобы вызвать скорости реагирования.
- `ScrollView` Используется, чтобы разрешить метку, которая должна быть видимыми, даже если высота экрана не меньше, чем сумма фиксированной высоты кнопок и изображения.

### <a name="grid"></a>Grid

Рассмотрим следующее приложение, отображаемых в книжной ориентации.

![](device-orientation-images/photo-grid-portrait.png "Приложение фото в книжной ориентации")

и альбомной ориентации.

![](device-orientation-images/photo-grid-landscape.png "Приложение фото в альбомной ориентации")

Что достигается следующим кодом XAML:

```xaml
<?xml version="1.0" encoding="UTF-8"?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
x:Class="ResponsiveLayout.GridPageXaml"
Title="Grid - XAML">
    <ContentPage.Content>
        <Grid x:Name="outerGrid">
            <Grid.RowDefinitions>
                <RowDefinition Height="*" />
                <RowDefinition Height="60" />
            </Grid.RowDefinitions>
            <Grid x:Name="innerGrid" Grid.Row="0" Padding="10">
                <Grid.RowDefinitions>
                    <RowDefinition Height="*" />
                </Grid.RowDefinitions>
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Image Source="deer.jpg" Grid.Row="0" Grid.Column="0" HeightRequest="300" WidthRequest="300" />
                <Grid x:Name="controlsGrid" Grid.Row="0" Grid.Column="1" >
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                        <RowDefinition Height="Auto" />
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition Width="Auto" />
                        <ColumnDefinition Width="*" />
                    </Grid.ColumnDefinitions>
                    <Label Text="Name:" Grid.Row="0" Grid.Column="0" />
                    <Label Text="Date:" Grid.Row="1" Grid.Column="0" />
                    <Label Text="Tags:" Grid.Row="2" Grid.Column="0" />
                    <Entry Grid.Row="0" Grid.Column="1" />
                    <Entry Grid.Row="1" Grid.Column="1" />
                    <Entry Grid.Row="2" Grid.Column="1" />
                </Grid>
            </Grid>
            <Grid x:Name="buttonsGrid" Grid.Row="1">
                <Grid.ColumnDefinitions>
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                    <ColumnDefinition Width="*" />
                </Grid.ColumnDefinitions>
                <Button Text="Previous" Grid.Column="0" />
                <Button Text="Save" Grid.Column="1" />
                <Button Text="Next" Grid.Column="2" />
            </Grid>
        </Grid>
    </ContentPage.Content>
</ContentPage>
```

А также следующие процедурный код для обработки изменений поворота:

```csharp
private double width;
private double height;

protected override void OnSizeAllocated (double width, double height){
    base.OnSizeAllocated (width, height);
    if (width != this.width || height != this.height) {
        this.width = width;
        this.height = height;
        if (width > height) {
            innerGrid.RowDefinitions.Clear();
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.RowDefinitions.Add (new RowDefinition{ Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition { Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 1, 0);
        } else {
            innerGrid.ColumnDefinitions.Clear ();
            innerGrid.ColumnDefinitions.Add (new ColumnDefinition{ Width = new GridLength (1, GridUnitType.Star) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Auto) });
            innerGrid.RowDefinitions.Add (new RowDefinition { Height = new GridLength (1, GridUnitType.Star) });
            innerGrid.Children.Remove (controlsGrid);
            innerGrid.Children.Add (controlsGrid, 0, 1);
        }
    }
}
```

Обратите внимание на следующее условия:

- Таким образом макета страницы имеется метод для изменения положения элементов управления сетки.


## <a name="related-links"></a>Связанные ссылки

- [Макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/Layout/)
- [Пример BusinessTumble (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/BusinessTumble/)
- [Динамический макет (пример)](https://developer.xamarin.com/samples/xamarin-forms/UserInterface/ResponsiveLayout)
- [Выводит изображение, в зависимости от ориентации экрана](https://developer.xamarin.com/recipes/cross-platform/xamarin-forms/controls/screen-orientation/)
