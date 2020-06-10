---
title: Разметка JSON MonoTouch.Dialog
description: В этом документе описывается синтаксис JSON, который можно использовать для создания пользовательского интерфейса Xamarin. iOS с помощью функции "некасание".
ms.prod: xamarin
ms.assetid: 59F3E18C-3A73-69B8-DA5E-21B19B9DFB98
ms.technology: xamarin-ios
ms.date: 11/25/2015
author: davidortinau
ms.author: daortin
ms.openlocfilehash: fc6066155a4171b106e772c1fe6fe7ee3e5c67cf
ms.sourcegitcommit: 93e6358aac2ade44e8b800f066405b8bc8df2510
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/09/2020
ms.locfileid: "84573512"
---
# <a name="monotouchdialog-json-markup"></a>Разметка JSON MonoTouch.Dialog

На этой странице описывается разметка JSON, принятая в [Жсонелемент](xref:MonoTouch.Dialog.JsonElement) диалогового окна.

Начнем с примера. Ниже приведен полный JSON-файл, который можно передать в Жсонелемент.

```json
{     
    "title": "Json Sample",
    "sections": [ 
        {
            "header": "Booleans",
            "footer": "Slider or image-based",
            "id": "first-section",
            "elements": [
                { 
                    "type": "boolean",
                    "caption": "Demo of a Boolean",
                    "value": true
                }, {
                    "type": "boolean",
                    "caption": "Boolean using images",
                    "value": false,
                    "on": "favorite.png",
                    "off": "~/favorited.png"
                }, {
                    "type": "root",
                    "title": "Tap for nested controller",
                    "sections": [
                        {
                            "header": "Nested view!",
                            "elements": [
                                {
                                    "type": "boolean",
                                    "caption": "Just a boolean",
                                    "id": "the-boolean",
                                    "value": false
                                }, {
                                    "type": "string",
                                    "caption": "Welcome to the nested controller"
                                }
                            ]
                        }
                    ]
                }
            ]
        }, {
            "header": "Entries",
            "elements" : [
                {
                    "type": "entry",
                    "caption": "Username",
                    "value": "",
                    "placeholder": "Your account username"
                }
            ]
        }
    ]
}
```

Приведенная выше разметка создает следующий пользовательский интерфейс:

 [![](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png "The UI created by the given markup")](monotouch.dialog-json-markup-images/screen-shot-2012-03-02-at-11.31.31-am.png#lightbox)

Каждый элемент в дереве может содержать свойство `"id"` . В среде выполнения можно ссылаться на отдельные разделы или элементы с помощью индексатора Жсонелемент. Пример:

```csharp
var jsonElement = JsonElement.FromFile ("demo.json");

var firstSection = jsonElement ["first-section"] as Section;

var theBoolean = jsonElement ["the-boolean"] as BooleanElement;
```

 <a name="Root_Element_Syntax"></a>

## <a name="root-element-syntax"></a>Синтаксис корневого элемента

Корневой элемент содержит следующие значения:

- `title`
- Среда `sections` (необязательно)

Корневой элемент может отображаться внутри раздела как элемент для создания вложенного контроллера. В этом случае `"type"` необходимо задать для дополнительного свойства значение`"root"`

 <a name="url"></a>

### <a name="url"></a>URL-адрес

Если `"url"` свойство задано, то если пользователь наберет на этот RootElement, код запросит файл из указанного URL-адреса и сделает его содержимым новой информацией. Его можно использовать для создания расширения пользовательского интерфейса с сервера в зависимости от того, что касается пользователь.

 <a name="group"></a>

### <a name="group"></a>group

Если этот параметр задан, то этот параметр задает GroupName для корневого элемента. Имена групп используются для выбора сводки, которая отображается как значение корневого элемента из одного из вложенных элементов в элементе. Это либо значение флажка, либо значение переключателя.

 <a name="radioselected"></a>

### <a name="radioselected"></a>радиоселектед

Определяет элемент Радио, выбранный во вложенных элементах

 <a name="title"></a>

### <a name="title"></a>title

Если он есть, это будет название, используемое для RootElement

 <a name="type"></a>

### <a name="type"></a>тип

Должен иметь значение, `"root"` если оно отображается в разделе (используется для вложенных контроллеров).

 <a name="sections"></a>

### <a name="sections"></a>разделы

Это массив JSON с отдельными разделами

 <a name="Section_Syntax"></a>

## <a name="section-syntax"></a>Синтаксис раздела

Раздел содержит:

- Среда `header` (необязательно)
- Среда `footer` (необязательно)
- `elements` массив

 <a name="header"></a>

### <a name="header"></a>заголовок

При наличии текста заголовок отображается в виде заголовка раздела.

 <a name="footer"></a>

### <a name="footer"></a>Нижний колонтитул

При наличии нижнего колонтитула отображается в нижней части раздела.

 <a name="elements"></a>

### <a name="elements"></a>элементы

Это массив элементов. Каждый элемент должен содержать по крайней мере один ключ — `"type"` ключ, используемый для задания типа создаваемого элемента.
Некоторые элементы имеют общие свойства, такие как `"caption"` и `"value"` . Ниже перечислены поддерживаемые элементы.

- `string`элементы (с стилизацией и без него)
- `entry`строки (обычные или пароли)
- `boolean`значения (с использованием параметров или изображений)

Строковые элементы можно использовать в качестве кнопок, предоставляя метод, вызываемый при касании пользователем либо ячейки, либо принадлежности.

 <a name="Rendering_Elements"></a>

## <a name="rendering-elements"></a>Отрисовка элементов

Элементы отрисовки основаны на C# Стринжелемент и Стиледстринжелемент, и они могут отображать сведения различными способами, и их можно визуализировать различными способами. Простейшие элементы могут быть созданы следующим образом:

```json
{
    "type": "string",
    "caption": "Json Serializer"
}
```

Будет показана простая строка со всеми заданными по умолчанию: шрифт, фон, цвет текста и оформление. Можно подключить действия к этим элементам и сделать их похожими на кнопки, задав `"ontap"` свойство или `"onaccessorytap"` Свойства:

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos"
}
```

Приведенный выше вызовет метод "Шовфотос" в классе "Acme. Фотобиблиотека". `"onaccessorytap"`Метод аналогичен, но он будет вызываться только в том случае, если пользователь нажмет на эту принадлежность вместо того, чтобы коснуться ячейки. Чтобы включить эту функцию, необходимо также задать принадлежность:

```json
{
    "type": "string",
    "caption": "View Photos",
    "ontap": "Acme.PhotoLibrary.ShowPhotos",
    "accessory": "detail-disclosure",
    "onaccessorytap": "Acme.PhotoLibrary.ShowStats"
}
```

Элементы отрисовки могут отображать две строки одновременно, одна — заголовок, а другая — значение. Способ подготовки к просмотру строк зависит от стиля. его можно задать с помощью `"style"` Свойства. По умолчанию отображается заголовок слева, а справа — значение. Дополнительные сведения см. в разделе о стиле. Цвета кодируются с помощью символа "#", за которым следуют шестнадцатеричные числа, представляющие значения красного, зеленого, синего и, возможно, альфа-канала. Содержимое может быть закодировано в краткой форме (3 или 4 шестнадцатиричных цифры), представляющей значения RGB или RGBA. Или длинная форма (6 или 8 цифр), представляющая значения RGB или RGBA. Короткая версия — это краткая форма для написания одной и той же шестнадцатеричной цифры дважды. Таким образом, константа "#1bc" интепретед красным = 0x11, Green = 0xBB и Blue = 0xCC. Если альфа-значение отсутствует, то цвет непрозрачен. Некоторые примеры.

```json
"background": "#f00"
"background": "#fa08f880"
```

 <a name="accessory"></a>

### <a name="accessory"></a>периферийное устройство

Определяет тип принадлежности, отображаемого в элементе отрисовки, возможные значения:

- `checkmark`
- `detail-disclosure`
- `disclosure-indicator`

Если значение отсутствует, периферия не отображается

 <a name="background"></a>

### <a name="background"></a>background

Свойство Background задает цвет фона для ячейки. Значение является либо URL-адресом изображения (в данном случае будет вызван асинхронный загрузчик изображений, а фоновый файл будет обновлен после скачивания образа), либо может быть цветом, заданным с помощью синтаксиса Color.

 <a name="caption"></a>

### <a name="caption"></a>caption

Основная строка, отображаемая в элементе отрисовки. Шрифт и цвет можно настроить, задав `"textcolor"` `"font"` Свойства и. Стиль отрисовки определяется `"style"` свойством.

 <a name="color_and_detailcolor"></a>

### <a name="color-and-detailcolor"></a>цвет и детаилколор

Цвет, используемый для основного текста или подробного текста.

 <a name="detailfont_and_font"></a>

### <a name="detailfont-and-font"></a>детаилфонт и Font

Шрифт, используемый для заголовка или текста сведений. Формат спецификации шрифта — это имя шрифта, которое затем может быть задано тире и размером точки.
Ниже приведены допустимые спецификации шрифтов.

- Helvetica
- "Helvetica-14"

 <a name="linebreak"></a>

### <a name="linebreak"></a>linebreak

Определяет, как разбиваются линии. Допустимые значения:

- `character-wrap`
- `clip`
- `head-truncation`
- `middle-truncation`
- `tail-truncation`
- `word-wrap`

`character-wrap`И, и `word-wrap` могут использоваться вместе со `"lines"` свойством, имеющим значение 0, чтобы превратить элемент отрисовки в многострочный элемент.

 <a name="ontap_and_onaccessorytap"></a>

### <a name="ontap-and-onaccessorytap"></a>ONTAP и онакцессоритап

Эти свойства должны указывать на имя статического метода в приложении, которое принимает объект в качестве параметра. При создании иерархии с помощью методов Жсондиалог. Фромфиле или Жсондиалог. Фромжсон можно передать необязательное значение объекта. Затем это значение объекта передается методам. Его можно использовать для передачи некоторого контекста в статический метод. Пример.

```csharp
class Foo {
    Foo ()
    {
        root = JsonDialog.FromJson (myJson, this);
    }

    static void Callback (object obj)
    {
        Foo myFoo = (Foo) obj;
        obj.Callback ();
    }
}
```

 <a name="lines"></a>

### <a name="lines"></a>lines

Если задано нулевое значение, автоматическое изменение размера элемента будет осуществляться в зависимости от содержимого содержащихся в нем строк. Чтобы это работало, необходимо также присвоить `"linebreak"` свойству значение `"character-wrap"` или `"word-wrap"` .

 <a name="style"></a>

### <a name="style"></a>Стиль

Стиль определяет вид стиля ячейки, который будет использоваться для отрисовки содержимого и соответствует значениям перечисления Уитаблевиевцеллстиле.
Допустимые значения:

- `"default"`
- `"value1"`
- `"value2"`
- `"subtitle"`: текст с подзаголовоком.

 <a name="subtitle"></a>

### <a name="subtitle"></a>subtitle

Значение, используемое для подзаголовка. Это сочетание клавиш для задания стиля `"subtitle"` и задания `"value"` свойства в виде строки.
Это делается и с одной записью.

 <a name="textcolor"></a>

### <a name="textcolor"></a>TextColor

Цвет, используемый для текста.

 <a name="value"></a>

### <a name="value"></a>значение

Вторичное значение, отображаемое в элементе отрисовки. Этот параметр влияет на макет `"style"` . Шрифт и цвет можно настроить с помощью параметров `"detailfont"` и `"detailcolor"` .

 <a name="Boolean_Elements"></a>

## <a name="boolean-elements"></a>Логические элементы

Логические элементы должны задавать тип `"bool"` , может содержать `"caption"` для вывода, а параметр `"value"` — как true, так и false. Если `"on"` `"off"` заданы свойства и, предполагается, что они являются изображениями. Образы разрешаются относительно текущего рабочего каталога в приложении. Если вы хотите ссылаться на файлы, относительные для пакета, можно использовать `"~"` ярлык для представления каталога пакета приложений. Например, это `"~/favorite.png"` будет файл избранного. PNG, содержащийся в файле пакета. Пример.

```json
{ 
    "type": "boolean",
    "caption": "Demo of a Boolean",
    "value": true
},

{
    "type": "boolean",
    "caption": "Boolean using images",
    "value": false,
    "on": "favorite.png",
    "off": "~/favorited.png"
}
```

 <a name="type"></a>

### <a name="type"></a>тип

Для типа можно задать значение `"boolean"` или `"checkbox"` . Если задано значение Boolean, будет использоваться Уислидер или Images (если `"on"` заданы и, и `"off"` ). Если задано значение CheckBox, будет использоваться флажок. `"group"`Свойство можно использовать, чтобы пометить логический элемент как принадлежащий определенной группе. Это полезно, если содержащий корневой элемент также имеет `"group"` свойство, как корневой каталог будет суммировать результаты с учетом всех логических значений (или флажков), принадлежащих одной группе.

 <a name="Entry_Elements"></a>

## <a name="entry-elements"></a>Элементы записи

Элементы записи используются для предоставления пользователю возможности ввода данных. Для элементов entry используется тип `"entry"` или `"password"` . Для `"caption"` свойства задан текст, отображаемый справа, а для параметра задается `"value"` начальное значение. `"placeholder"`Используется для отображения подсказки пользователю для пустых записей (она отображается серым цветом). Ниже приводится несколько примеров.

```json
{
    "type": "entry",
    "caption": "Username",
    "value": "",
    "placeholder": "Your account username"
}, {
    "type": "password",
    "caption": "Password",
    "value": "",
    "placeholder": "You password"
}, {
    "type": "entry",
    "caption": "Zip Code",
    "value": "01010",
    "placeholder": "your zip code",
    "keyboard": "numbers"
}, {
    "type": "entry",
    "return-key": "route",
    "caption": "Entry with 'route'",
    "placeholder": "captialization all + no corrections",
    "capitalization": "all",
    "autocorrect": "no"
}
```

 <a name="autocorrect"></a>

### <a name="autocorrect"></a>задать

Определяет стиль автоматического исправления, используемый для записи. Возможные значения: true или false (строки `"yes"` и `"no"` ).

 <a name="capitalization"></a>

### <a name="capitalization"></a>написание прописными буквами

Стиль капитализации, используемый для записи. Допустимые значения:

- `all`
- `none`
- `sentences`
- `words`

 <a name="caption"></a>

### <a name="caption"></a>caption

Заголовок, используемый для записи

 <a name="keyboard"></a>

### <a name="keyboard"></a>клавиатура

Тип клавиатуры, используемый для ввода данных. Допустимые значения:

- `ascii`
- `decimal`
- `default`
- `email`
- `name`
- `numbers`
- `numbers-and-punctuation`
- `twitter`
- `url`

 <a name="placeholder"></a>

### <a name="placeholder"></a>заполнитель

Текст подсказки, отображаемый, если запись имеет пустое значение.

 <a name="return-key"></a>

### <a name="return-key"></a>ключ возврата

Метка, используемая для ключа возврата. Допустимые значения:

- `default`
- `done`
- `emergencycall`
- `go`
- `google`
- `join`
- `next`
- `route`
- `search`
- `send`
- `yahoo`

 <a name="value"></a>

### <a name="value"></a>значение

Начальное значение для записи

 <a name="Radio_Elements"></a>

## <a name="radio-elements"></a>Элементы Радио

Элементы переключателей имеют тип `"radio"` . Выбранный элемент выбирается `radioselected` свойством содержащего его корневого элемента.
Кроме того, если для свойства задано значение `"group"` , этот переключатель принадлежит этой группе.

 <a name="Date_and_Time_Elements"></a>

## <a name="date-and-time-elements"></a>Элементы даты и времени

Типы элементов `"datetime"` `"date"` и `"time"` используются для отрисовки дат с использованием времени, дат или времени. Эти элементы принимают в качестве параметров заголовок и значение. Значение может быть записано в любом формате, поддерживаемом функцией .NET DateTime. Parse. Пример.

```json
"header": "Dates and Times",
"elements": [
    {
        "type": "datetime",
        "caption": "Date and Time",
        "value": "Sat, 01 Nov 2008 19:35:00 GMT"
    }, {
        "type": "date",
        "caption": "Date",
        "value": "10/10"
    }, {
        "type": "time",
        "caption": "Time",
        "value": "11:23"
    }                       
]
```

 <a name="Html/Web_Element"></a>

## <a name="htmlweb-element"></a>HTML/Web-элемент

Можно создать ячейку, при нажатии которой будет внедрен Уивебвиев, который визуализирует содержимое указанного URL-адреса (локального или удаленного), используя `"html"` тип. Единственными двумя свойствами для этого элемента являются `"caption"` и `"url"` :

```json
{
    "type": "html",
    "caption": "Miguel's blog",
    "url": "https://tirania.org/blog" 
}
```
