---
ms.assetid: 1BB412D1-FC3D-4E69-8B01-B976A3DB6328
title: 'WPF vs. Xamarin.Forms: Сходства и различия'
description: В этом документе сравниваются и противопоставляются WPF в Xamarin.Forms. В нем описывается, шаблоны элементов управления, XAML, инфраструктура привязки, шаблоны данных, элемент управления ItemsControl, UserControl, навигации и URL-адрес навигации.
author: asb3993
ms.author: amburns
ms.date: 04/26/2017
ms.openlocfilehash: 4d6585715b2fc118bb350c242abccbc68791ec0b
ms.sourcegitcommit: 6e955f6851794d58334d41f7a550d93a47e834d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/12/2018
ms.locfileid: "38998522"
---
# <a name="wpf-vs-xamarinforms-similarities--differences"></a>WPF vs. Xamarin.Forms: Сходства и различия

## <a name="control-templates"></a>Шаблоны элементов управления

WPF поддерживает концепцию *шаблоны элементов управления* инструкциями визуализации для элемента управления (`Button`, `ListBox`и т. д.). Как упоминалось выше, Xamarin.Forms использует конкретный _отрисовки_ для этого классы, которые взаимодействуют с собственной платформы (Android, iOS, и т.д.) для визуализации элемента управления.

Тем не менее Xamarin.Forms _does_ имеют `ControlTemplate` тип — он используется для использования тем `Page` объектов. Он предоставляет определение для `Page` , предоставляет согласованные содержимое, но позволяет пользователю страницы для изменения цвета, шрифты и др. и даже добавить элементы для уникальности к приложению.

Типичные способы использования для этого моментов например диалоговых окон проверки подлинности, запросы, а также возможность типовых, но тем страницы внешнего вида и поведения, можно настроить в приложении. В рамках этой поддержки используются многие знакомы с именем WPF элементы управления:

1. `ContentPage`
2. `ContentView`
3. `ContentPresenter`
4. `TemplateBinding`

Но важно знать, что они _не_ обслуживает той же цели в Xamarin.Forms. Дополнительные сведения об этой функции, ознакомьтесь с [страницы документации](~/xamarin-forms/app-fundamentals/templates/control-templates/index.md).

## <a name="xaml"></a>XAML

XAML используется в качестве декларативный язык разметки для WPF и Xamarin.Forms. В большинстве случаев синтаксис совпадает — основное различие заключается в объекты, которые определяются или создаются на графиках XAML.

- Xamarin.Forms поддерживает [спецификации XAML 2009](/dotnet/framework/xaml-services/xaml-2009-language-features/); это упрощает определение данных, таких как `string`s, `int`s, т. д., а также определение универсальных типов и передача аргументов в конструкторы.

- В настоящее время нет способа для динамической загрузки XAML в WPF можно с помощью `XamlReader`. Вы можете получить те же базовые функции с [пакет NuGet](https://www.nuget.org/packages/Xamarin.Forms.Dynamic/) на то, что.

### <a name="markup-extensions"></a>Расширения разметки

Xamarin.Forms поддерживает расширение XAML через расширения разметки, как WPF. По умолчанию он имеет же основных стандартных блоках:

1. `{x:Array}`
2. `{Binding}`
3. `{DynamicResource}`
4. `{x:Null}`
5. `{x:Static}`
6. `{StaticResource}`
7. `{x:Type}`

Кроме того, он включает в себя `{x:Reference}` из спецификации XAML 2009 и `{TemplateBinding}` расширение разметки, который используется для специализированную версию `ControlTemplate` поддерживаемых Xamarin.Forms.

> [!WARNING]
> `ControlTemplate` Поддержки отличается - несмотря на то, что он имеет то же имя.

Xamarin.Forms поддерживает также - расширений собственной разметки, но реализация несколько отличается. В WPF, должен быть производным от `MarkupExtension` -абстрактный базовый класс. В Xamarin.Forms, которое заменяется интерфейс `IMarkupExtension` или `IMarkupExtension<T>` которого является более гибким.

Так же, как WPF, является один необходимый метод `ProvideValue` метод для возврата значения из расширения разметки.

## <a name="binding-infrastructure"></a>Инфраструктура привязки

Одно из основных понятий, переносятся — это инфраструктура привязки данных для подключения к свойствам данных .NET свойства визуальных элементов. Это позволяет архитектурные шаблоны, такие как MVVM. Базовая архитектура идентичен — у вас есть связываемый базовый класс [BindableObject](xref:Xamarin.Forms.BindableObject), в WPF, это [DependencyObject](https://msdn.microsoft.com/en-us/library/system.windows.dependencyobject(v=vs.110).aspx) класса. Этот базовый класс используется в качестве предка для корневого для всех объектов, которые будут участвовать в качестве целевых объектов в привязке данных. Производные классы, то предоставлять [BindableProperty](xref:Xamarin.Forms.BindableProperty) объекты, которые действуют в качестве резервного хранилища для значений свойств (они определяются как [DependencyProperty](https://msdn.microsoft.com/library/system.windows.dependencyproperty(v=vs.110).aspx) объектов в WPF).

### <a name="defining-bindable-properties"></a>Определение привязываемые свойства

Определению привязываемые свойства в Xamarin.Forms совпадает со значением WPF:
1. Объект должен быть производным от `BindableObject`.
2. Должна существовать открытое статическое поле типа `BindableProperty` объявить, чтобы определить ключ резервного хранилища для свойства.
3. Должно быть оболочки свойства открытого экземпляра, который использует `GetValue` и `SetValue` для извлечения и измените значение свойства.

Полный пример см. в разделе [свойства связывания в Xamarin.Forms](~/xamarin-forms/xaml/bindable-properties.md).

### <a name="attached-properties"></a>Вложенные свойства

Присоединенные свойства являются подмножеством свойство, используемое, и они работают так же, как в WPF. Основное различие — что оболочки свойства в данном случае это пропущен и заменены набор статических get и set методы в класс-владелец. См. в разделе [присоединенного свойства в Xamarin.Forms](~/xamarin-forms/xaml/attached-properties.md) Дополнительные сведения.

### <a name="using-the-binding-engine"></a>С помощью обработчик привязки

Процесс использования обработчик привязки совпадает в WPF. Его можно использовать в коде путем создания `Binding` объект привязан к объект источника (любой тип .NET) и значение необязательного свойства (Если пропущен, он считает исходный объект самого - свойства так же, как WPF). Затем можно использовать `SetBinding` на любом `BindableObject` для привязки к `BindableProperty`.

Кроме того, можно определить отношение привязки в XAML с помощью `BindingExtension`. Он имеет те же базовые значения, что расширение в WPF.

Поддержка привязки и ядра больше похожи на реализацию Silverlight чем WPF. Существует несколько отсутствующих компонентов, которые не были реализованы в Xamarin.Forms.

- Не поддерживается для следующих компонентов в привязках:
    - BindingGroupName
    - BindsDirectlyToSource
    - IsAsync
    - MultiBinding
    - NotifyOnSourceUpdated
    - NotifyOnTargetUpdated
    - NotifyOnValidationError
    - UpdateSourceTrigger
    - UpdateSourceExceptionFilter
    - ValidatesOnDataErrors
    - ValidatesOnExceptions
    - Коллекции ValidationRules
    - XPath
    - XmlNamespaceManager
- `Binding.Mode` не поддерживает `OneTime`, вместо этого просто использовать `OneWay`.

#### <a name="relativesource"></a>RelativeSource

Не поддерживается для `RelativeSource` привязки. В WPF они позволяют выполнять привязку к другим визуальным элементам, определенные в XAML. В Xamarin.Forms, такая же функциональность достигается путем использования `{x:Reference}` расширение разметки. Например предположим, что у нас есть элемент управления с именем «otherControl», имеющий свойство текста, можно выполнить привязку к его следующим образом:

**WPF**

```xaml
Text={Binding RelativeSource={RelativeSource otherControl}, Path=Text}
```

**Xamarin.Forms**

```xaml
Text={Binding Source={x:Reference otherControl}, Path=Text}
```

Такая же функция может использоваться для `{RelativeSource Self}` компонентов. Тем не менее не поддерживается для нахождения предков по типу (`{RelativeSource FindAncestor}`).

#### <a name="binding-context"></a>Контекст привязки

В WPF, можно определить `DataContext` значение какие reprents свойства источника привязки по умолчанию. Если источник привязки не определен, используется значение этого свойства. Значение наследуется вниз визуального дерева, что позволяет определить на более высоком уровне и затем используются дочерние элементы.

В Xamarin.Forms, эту же функцию по, но имя свойства `BindingContext`.

#### <a name="value-converters"></a>преобразователи значений;

Преобразователи значений, полностью поддерживаются в Xamarin.Forms — так же, как WPF. Используется интерфейс фигуру, но Xamarin.Forms есть интерфейсом, определенным в `Xamarin.Forms` пространства имен.

### <a name="model-view-viewmodel"></a>Model-View-ViewModel

MVVM полностью поддерживается WPF и Xamarin.Forms.

WPF включает встроенную в `RoutedCommand` — иногда используется; Xamarin.Forms не имеет встроенных команд поддержки за пределами `ICommand` определение интерфейса. Можно включить ряд инфраструктур MVVM, чтобы добавить необходимые базовые классы для реализации MVVM.

#### <a name="inotifypropertychanged-and-inotifycollectionchanged"></a>INotifyPropertyChanged и INotifyCollectionChanged

Оба интерфейса полностью поддерживаются в привязках Xamarin.Forms. В отличие от многих платформы на основе XAML уведомления об изменении свойств, которые могут возникать в фоновых потоках в Xamarin.Forms (как WPF) и обработчик привязки будет правильно перевести их в потоке пользовательского интерфейса.

Кроме того, обе эти среды поддерживают `SynchronziationContext` и `async` / `await` сделать маршалинга в правильном потоке. WPF включает в себя `Dispatcher` класс на все визуальные элементы, Xamarin.Forms имеет статический метод `Device.BeginInvokeOnMainThread` которого также можно (хотя `SynchronizationContext` является предпочтительным для кросс платформенного кода).

- Платформа Xamarin.Forms включает `ObservableCollection<T>` поддерживающей уведомления об изменении коллекции.
- Можно использовать `BindingBase.EnableCollectionSynchronization` для включения обновлений между потоками для коллекции. API-интерфейса несколько отличается от варианта WPF, [Проверьте документацию об использовании](xref:Xamarin.Forms.BindingBase.EnableCollectionSynchronization*).

## <a name="data-templates"></a>Шаблоны данных

Шаблоны данных поддерживаются в Xamarin.Forms для настройки обработки `ListView` строки (ячейка). В отличие от WPF, который можно использовать `DataTemplate`s для любого ориентированных на содержимое элемента управления, в настоящее время Xamarin.Forms используются только для `ListView`. Определение шаблона может быть определен как встроенный (назначенный `ItemTemplate` свойство), или как ресурс в `ResourceDictionary`.

Кроме того они не достаточной гибкостью, как их соответствующим составляющим WPF.

1. Корневой элемент `DataTemplate` необходимо _всегда_ быть `ViewCell` объекта.
2. Триггеры данных полностью поддерживаются в шаблон данных, но необходимо включить `DataType` свойство, указывающее тип свойства, с которой связан триггер.
3. `DataTemplateSelector` также поддерживается, но является производным от `DataTemplate` и поэтому просто назначается непосредственно `ItemTemplate` свойство (и `ItemTemplateSelector` в WPF).

## <a name="itemscontrol"></a>Элемент управления ItemsControl

Нет нет встроенных equivelent для `ItemsControl` в Xamarin.Forms; но [настраиваемой классификации для Xamarin.Forms доступен здесь](https://github.com/xamarinhq/xamu-infrastructure/blob/master/src/XamU.Infrastructure/Controls/ItemsControl.cs).

## <a name="user-controls"></a>Пользовательские элементы управления

В WPF `UserControl`s используются для предоставления часть пользовательского интерфейса, чем были связаны дополнительные поведения. В Xamarin.Forms, мы используем `ContentView` для той же цели. Поддерживают привязку и включения в XAML.

## <a name="navigation"></a>Навигация

WPF включает редко используемые `NavigationService` которого может использоваться в счет навигации «браузера по принципу». Большинство приложений не утруждали себя с этим тем не менее и вместо этого использовать разные `Window` элементов, или разные части окна для отображения данных.

На различных устройствах phone _экраны_ часто являются решением, и поэтому Xamarin.Forms включает в себя поддержку нескольких видов навигации:

| Стиль навигации | Тип страницы |
|--- |--- |
|На основе стека (отправку/извлечение)|NavigationPage|
|«Основной/подробности»|MasterDetailPage|
|Вкладки|TabbedPage|
|Проведите влево/вправо|CarouselView|

`NavigationPage` — Наиболее распространенный подход, и каждая страница имеет `Navigation` свойство, которое может использоваться для передаче или страницы в и из стека навигации. Это ближайший equivelent для `NavigationService` в WPF.

### <a name="url-navigation"></a>URL-адрес навигации

WPF — это технология для рабочей среды и могут принимать параметры командной строки для направления поведения при запуске. Можно использовать Xamarin.Forms [глубокое связывание URL-адрес](https://blog.xamarin.com/deep-link-content-with-xamarin-forms-url-navigation/) для перехода на страницу при запуске.
