---
title: Представления в книгах Xamarin
description: В этом документе описывается конвейер представление Xamarin книги, позволяющая отрисовку форматированного результаты для любой код, который возвращает значение.
ms.prod: xamarin
ms.assetid: 5C7A60E3-1427-47C9-A022-720F25ECB031
author: topgenorth
ms.author: toopge
ms.date: 03/30/2017
ms.openlocfilehash: d4d8fa164b9f52e2c5331aa2c08fdddf232572d4
ms.sourcegitcommit: ea1dc12a3c2d7322f234997daacbfdb6ad542507
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/05/2018
ms.locfileid: "34794167"
---
# <a name="representations-in-xamarin-workbooks"></a>Представления в книгах Xamarin

## <a name="representations"></a>представления

В рамках сеанса книги или инспектора код, который выполняется и результатом (например, метод, возвращающий значение, либо результат выражения) обрабатывается по конвейеру представление в агенте. Все объекты, за исключением примитивы, такие как целые числа, для создания интерактивных элементов диаграмм, будут отражены и проходит через процесс предоставляет альтернативные представления, которые клиент может отображать большим количеством параметров. Объекты любого размера и глубины безопасно поддерживаются (включая циклы и бесконечной перечислимых значений) из-за отложенной и интерактивные отражения и удаленное взаимодействие.

Книги Xamarin предоставляет несколько общих типов для всех агентов, а также клиентам, которые позволяют для форматированного отображения результатов. [`Color`][xir-color] Это один пример такого типа, например iOS агент где отвечает за преобразование `CGColor` или `UIColor` объектов в `Xamarin.Interactive.Representations.Color` объекта.

Помимо общих представлений интеграции пакета SDK предоставляет API для сериализации пользовательского представления в агенте и визуализации представления в клиенте.

## <a name="external-representations"></a>Внешних представлений

[`Xamarin.Interactive.IAgent.RepresentationManager`][repman] предоставляет возможность регистрировать [ `RepresentationProvider` ] [ repp], который интеграцию необходимо реализовать для преобразования из любой объект зависит от формы, для подготовки к просмотру. Необходимо реализовать эти зависит от формы [ `ISerializableObject` ] [ serobj] интерфейса.

Реализация `ISerializableObject` интерфейс добавляет метод сериализации, который точно управляет процессом сериализации объектов. `Serialize` Метод ожидает, что разработчик именно будет указано, какие свойства должны быть сериализованы, и окончательное имя, которые будут. Просмотрев `Person` объекта в нашем [`KitchenSink` образец] [образец], мы видим, как это работает:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; }

  // Rest of the code is omitted…

  void ISerializableObject.Serialize (ObjectSerializer serializer)
    => serializer.Property (nameof (Name), Name);
}
```

Если мы хотели предоставить надмножество или подмножество свойств из исходного объекта, это можно сделать с помощью `Serialize`. Например, мы делаем примерно таким образом обеспечивается предварительно вычисленных `Age` свойства `Person`:

```csharp
public sealed class Person : ISerializableObject
{
  public string Name { get; set; }
  public DateTime DateOfBirth { get; set; }

  // <snip>

  void ISerializableObject.Serialize (ObjectSerializer serializer)
  {
    serializer.Property (nameof (Name), Name);
    serializer.Property (nameof (DateOfBirth), DateOfBirth);

    // Let's pre-compute an Age property that's the person's age in years,
    // so we don't have to compute it in the renderer.
    var age = (DateTime.MinValue + (DateTime.Now - DateOfBirth)).Year - 1;
    serializer.Property ("Age", age)
  }
}
```

> [!NOTE]
> API-интерфейсы, создания `ISerializableObject` непосредственно объекты не обязательно должны быть обработаны `RepresentationProvider`. Если требуется отобразить объект **не** `ISerializableObject`, требуется обрабатывать его в ваш `RepresentationProvider`.

### <a name="rendering-a-representation"></a>Подготовка к просмотру представление

Модули подготовки отчетов, реализованных в JavaScript и будет иметь доступ к версии JavaScript объекта, представленного через `ISerializableObject`. JavaScript копирования также будет иметь `$type` строковое свойство, которое указывает имя типа .NET.

Мы рекомендуем использовать TypeScript для интеграции клиентского кода, компилируемого конечно в ванильные JavaScript. В любом случае пакет SDK предоставляет [типизаций] [ typings] которого ссылается непосредственно TypeScript или просто указывается вручную при записи ванильные JavaScript является предпочтительным.

Точка основного интеграции для подготовки к просмотру `xamarin.interactive.RendererRegistry`:

```js
xamarin.interactive.RendererRegistry.registerRenderer(
  function (source) {
    if (source.$type === "SampleExternalIntegration.Person")
      return new PersonRenderer;
    return undefined;
  }
);
```

Здесь `PersonRenderer` реализует `Renderer` интерфейса. В разделе [типизаций] [ typings] для получения дополнительных сведений.

[typings]: https://github.com/xamarin/Workbooks/blob/master/SDK/typings/xamarin-interactive.d.ts
[xir-color]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.Color/
[repman]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.IRepresentationManager/
[repp]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Representations.RepresentationProvider/
[serobj]: https://developer.xamarin.com/api/type/Xamarin.Interactive.Serialization.ISerializableObject/
