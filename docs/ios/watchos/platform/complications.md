---
title: watchOS сложности в Xamarin
description: В этом документе описывается работа с watchOS осложнениями в Xamarin. В нем обсуждается, как добавить сложность, написать сложность, шаблоны и предоставить пример кода.
ms.prod: xamarin
ms.assetid: 7ACD9A2B-CF69-46EA-B0C8-10E7D81216E8
ms.technology: xamarin-ios
author: conceptdev
ms.author: crdun
ms.date: 07/03/2017
ms.openlocfilehash: a4d96b9a092dfc534f40e3e62737af2a3a0004f5
ms.sourcegitcommit: 57f815bf0024b1afe9754c0e28054fc0a53ce302
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70768618"
---
# <a name="watchos-complications-in-xamarin"></a>watchOS сложности в Xamarin

_watchOS позволяет разработчикам писать пользовательские сложности для просмотра лиц_

На этой странице объясняются различные типы возможных осложнений и описывается, как добавить сложность в приложение watchOS 3.

Обратите внимание, что каждое приложение watchOS может иметь только одну сложность.

Начните с ознакомления с [документацией Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ManagingComplications.html) , чтобы определить, подходит ли ваше приложение для усложнения. Существует 5 `CLKComplicationFamily` типов отображаемых данных:

[![](complications-images/all-complications-sml.png "Доступны 5 типов Клккомпликатионфамили: Круглый маленький, модульный малый, модульный крупный, утилитарные малый, утилитарные большой")](complications-images/all-complications.png#lightbox)

Приложения могут реализовывать только один стиль (или все пять) в зависимости от отображаемых данных.
Вы также можете поддерживать командировку времени, предоставляя значения в прошлом и (или) будущем по мере того, как пользователь включит Digital Crown.

<a name="adding" />

## <a name="adding-a-complication"></a>Добавление усложнения

### <a name="configuration"></a>Конфигурация

Сложности можно добавить в приложение Watch во время создания или добавить в существующее решение вручную.

### <a name="add-new-project"></a>Добавить новый проект...

Мастер **добавления нового проекта...** содержит флажок, который автоматически создает класс контроллера усложнения и настраивает файл **info. plist** :

![](complications-images/file-new-project-sml.png "Флажок \"включить усложнение\"")

### <a name="existing-projects"></a>Существующие проекты

Чтобы добавить сложность в существующий проект, выполните следующие действия.

1. Создайте новый файл класса **ComplicationController.CS** и реализуйте `CLKComplicationDataSource`.
2. Настройте **info. plist** приложения, чтобы обеспечить сложность и удостоверения, которые поддерживаются семействами с усложненными наборами.

Эти действия подробно описаны ниже.

<a name="clkcomplicationcontroller" />

### <a name="clkcomplicationdatasource-class"></a>Класс Клккомпликатиондатасаурце

Следующий C# шаблон включает минимум обязательных методов для реализации `CLKComplicationDataSource`.

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
  public ComplicationController ()
  {
  }
  public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
  {
  }
  public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
  {
  }
  public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
  {
  }
}
```

Следуйте инструкциям по [написанию инструкций по усложнению](#writing) , чтобы добавить код в этот класс.

### <a name="infoplist"></a>Info.plist

В файле **info. plist** расширения Watch должно быть указано имя `CLKComplicationDataSource` и какие семейства осложнений, которые вы хотите поддерживать:

[![](complications-images/complications-config-sml.png "Типы семейства осложнений")](complications-images/complications-config.png#lightbox)

В списке записей **класса источника данных** будут показаны имена классов, которые `CLKComplicationDataSource` содержат логику для подклассов.

## <a name="clkcomplicationdatasource"></a>клккомпликатиондатасаурце

Все функции усложнения реализуются в одном классе, переопределяя методы из `CLKComplicationDataSource` абстрактного класса (который `ICLKComplicationDataSource` реализует интерфейс).

### <a name="required-methods"></a>Обязательные методы

Для выполнения усложнения необходимо реализовать следующие методы:

- `GetPlaceholderTemplate`— Возврат статического отображения, используемого во время настройки, или если приложению не удается предоставить значение.
- `GetCurrentTimelineEntry`— Вычислять правильное отображение при выполнении усложнения.
- `GetSupportedTimeTravelDirections`— `CLKComplicationTimeTravelDirections` Возвращает параметры, такие как `None`, `Forward` `Backward`, или `Forward | Backward`.

### <a name="privacy"></a>Конфиденциальность

Сложности, отображающие персональные данные

- `GetPrivacyBehavior` - `CLKComplicationPrivacyBehavior.ShowOnLockScreen` или `HideOnLockScreen`

Если этот метод возвращает `HideOnLockScreen` значение, то сложность будет показывать либо значок, либо имя приложения (а не любые данные), если контрольные значения заблокированы.

### <a name="updates"></a>Обновления

- `GetNextRequestedUpdateDate`— Возвращать время, когда операционная система должна далее запрашивать у приложения обновленные отображаемые данные.

Вы также можете принудительно выполнить обновление из приложения iOS.

### <a name="supporting-time-travel"></a>Поддержка поездок во время

Поддержка поездок по времени необязательна и управляется `GetSupportedTimeTravelDirections` методом. Если он возвращает `Forward`, `Backward`или `Forward | Backward` необходимо реализовать следующие методы

- `GetTimelineStartDate`
- `GetTimelineEndDate`
- `GetTimelineEntriesBeforeDate`
- `GetTimelineEntriesAfterDate`

<a name="writing" />

## <a name="writing-a-complication"></a>Написание усложняется

Сложности в диапазоне от простых данных до отображения сложных изображений и данных с поддержкой поездок по времени. В приведенном ниже коде показано, как создать простую сложность одного шаблона.

<!--
The [sample]() for this article supports more template styles.
-->

## <a name="sample-code"></a>Пример кода

В `UtilitarianLarge` этом примере поддерживается только шаблон, поэтому его можно выбрать только для отдельных наблюдателей, которые поддерживают этот тип сложности. При *выборе* осложнений на контрольном экране отображается **Моя сложность** , а при *запуске* отображается текстовый **минуты**  (с учетом части времени).

```csharp
[Register ("ComplicationController")]
public class ComplicationController : CLKComplicationDataSource
{
    public ComplicationController ()
    {
    }
    public ComplicationController (IntPtr p) : base (p)
    {
    }
    public override void GetCurrentTimelineEntry (CLKComplication complication, Action<CLKComplicationTimelineEntry> handler)
    {
        CLKComplicationTimelineEntry entry = null;
    var complicationDisplay = "MINUTE " + DateTime.Now.Minute.ToString(); // text to display on watch face
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge)
        {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText(complicationDisplay); // dynamic display
            entry = CLKComplicationTimelineEntry.Create(NSDate.Now, textTemplate);
        } else {
            Console.WriteLine("Complication family timeline not supported (" + complication.Family + ")");
        }
        handler (entry);
    }
    public override void GetPlaceholderTemplate (CLKComplication complication, Action<CLKComplicationTemplate> handler)
    {
        CLKComplicationTemplate template = null;
        if (complication.Family == CLKComplicationFamily.UtilitarianLarge) {
            var textTemplate = new CLKComplicationTemplateUtilitarianLargeFlat ();
            textTemplate.TextProvider = CLKSimpleTextProvider.FromText ("MY COMPLICATION"); // static display
            template = textTemplate;
        } else {
            Console.WriteLine ("Complication family placeholder not not supported (" + complication.Family + ")");
        }
        handler (template);
    }
    public override void GetSupportedTimeTravelDirections (CLKComplication complication, Action<CLKComplicationTimeTravelDirections> handler)
    {
        handler (CLKComplicationTimeTravelDirections.None);
    }
}
```

<a name="templates" />

## <a name="complication-templates"></a>Шаблоны усложнения

Существует ряд различных шаблонов, доступных для каждого стиля усложнения.
Шаблоны **кольца** позволяют отображать круг в стиле выполнения вокруг сложности, которая может использоваться для отображения хода выполнения или другого графического значения.

[Документация по Клккомпликатионтемплате Apple](https://developer.apple.com/reference/clockkit/clkcomplicationtemplate)

### <a name="circular-small"></a>Круглый маленький

Имена этих классов шаблонов начинаются с `CLKComplicationTemplateCircularSmall`префикса:

- **Рингимаже** — отображение одного изображения с обработкой вокруг него.
- **Рингтекст** — отображение одной строки текста с обработкой вокруг него.
- **Симплеимаже** — просто отображение маленького отдельного изображения.
- **Симплетекст** — просто отображать небольшой фрагмент текста.
- **Стаккимаже** — отображение изображения и строки текста, расположенной над другим
- **Стакктекст** — отображение двух строк текста.

### <a name="modular-small"></a>Модульный малый

Имена этих классов шаблонов начинаются с `CLKComplicationTemplateModularSmall`префикса:

- **Колумнстекст** — отображает маленькую сетку текстовых значений (2 строки и 2 столбца).
- **Рингимаже** — отображение одного изображения с обработкой вокруг него.
- **Рингтекст** — отображение одной строки текста с обработкой вокруг него.
- **Симплеимаже** — просто отображение маленького отдельного изображения.
- **Симплетекст** — просто отображать небольшой фрагмент текста.
- **Стаккимаже** — отображение изображения и строки текста, расположенной над другим
- **Стакктекст** — отображение двух строк текста.

### <a name="modular-large"></a>Модульный крупный

Имена этих классов шаблонов начинаются с `CLKComplicationTemplateModularLarge`префикса:

- **Столбцы** — отображает сетку из 3 строк с двумя столбцами, при необходимости включая изображение слева от каждой строки.
- **Стандардбоди** — отображение строки заголовка полужирным шрифтом с двумя строками обычного текста. В заголовке можно дополнительно отобразить изображение слева.
- **Таблица** — отображение строки заголовка полужирным шрифтом с таблицей 2x2, расположенной под ним. В заголовке можно дополнительно отобразить изображение слева.
- **Таллбоди** — отображение строки заголовка полужирным шрифтом с более крупной строкой текста ниже.

### <a name="utilitarian-small"></a>Утилитарные малый

Имена этих классов шаблонов начинаются с `CLKComplicationTemplateUtilitarianSmall`префикса:

- **Плоский** — отображает изображение и некоторый текст в одной строке (текст должен быть коротким).
- **Рингимаже** — отображение одного изображения с обработкой вокруг него.
- **Рингтекст** — отображение одной строки текста с обработкой вокруг него.
- **Квадратный** — Отображение квадратного изображения (40px или 44px квадрат для 38 или часы Apple Watch соответственно).

### <a name="utilitarian-large"></a>Утилитарные крупный

Существует только один шаблон для этого стиля сложности: `CLKComplicationTemplateUtilitarianLargeFlat`.
Он отображает одно изображение и некоторый текст в одной строке.

## <a name="related-links"></a>Связанные ссылки

- [Документы Apple](https://developer.apple.com/library/watchos/documentation/General/Conceptual/WatchKitProgrammingGuide/ComplicationEssentials.html)
- [Видео ВВДК](https://developer.apple.com/videos/play/wwdc2015-209/)
