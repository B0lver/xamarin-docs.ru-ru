---
title: Общие подклассы nsobject в Xamarin.iOS
description: В этом документе описывается создание создать подклассы nsobject. Он проверяет, что можно и нельзя выполнить, рассматриваются статический регистратор и смотрит на производительность.
ms.prod: xamarin
ms.assetid: BB99EBD7-308A-C865-1829-4DFFDB1BBCA4
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/21/2017
ms.openlocfilehash: 4d2ab7a546925e92bb59f626279dcbf9c3c4ab4f
ms.sourcegitcommit: 93b1e2255d59c8ca6674485938f26bd425740dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67157699"
---
# <a name="generic-subclasses-of-nsobject-in-xamarinios"></a>Общие подклассы nsobject в Xamarin.iOS

## <a name="using-generics-with-nsobjects"></a>Использование универсальных типов с NSObjects

Возможно, для использования универсальных шаблонов в подклассы `NSObject`, например [UIView](xref:UIKit.UIView):

```csharp
class Foo<T> : UIView {
    public Foo (CGRect x) : base (x) {}
    public override void Draw (CoreGraphics.CGRect rect)
    {
        Console.WriteLine ("T: {0}. Type: {1}", typeof (T), GetType ().Name);
    }
}
```

Поскольку объекты Этот подкласс `NSObject` регистрируются в среде выполнения Objective-C существуют некоторые ограничения относительно возможности универсальных подклассы `NSObject` типов.
    
## <a name="considerations-for-generic-subclasses-of-nsobject"></a>Приводятся рекомендации по общие подклассы NSObject

В этом документе описаны ограничения в ограниченную поддержку универсальных подклассы `NSObjects`.
    
### <a name="generic-type-arguments-in-member-signatures"></a>Аргументы универсального типа в сигнатурах членов

Все аргументы универсального типа в сигнатуре члена, подвержен Objective-C должны иметь `NSObject` ограничение.

**Хороший**:

```csharp
class Generic<T> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: Параметр универсального типа является `NSObject`, поэтому сигнатура селектор для `myMethod:` можно безопасно предоставить Objective-C (всегда будет иметь `NSObject` или его подкласс).

**Неверный**:

```csharp
class Generic<T> : NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
    }
}
```

**Причина**: невозможно создать подпись Objective-C для экспортированные элементы, которые могут вызывать код Objective-C, так как подпись будет отличаться в зависимости от точного типа универсального типа `T`.

**Хороший**:

```csharp
class Generic<T> : NSObject
{
    T storage;

    [Export ("myMethod:")]
    public void MyMethod (NSObject value)
    {
    }
}
```

**Причина**: можно произвольные аргументы универсального типа, до тех пор, пока они не принимают частью сигнатуры экспортированного элемента.

**Хороший**:

```csharp
class Generic<T, U> : NSObject where T: NSObject
{
    [Export ("myMethod:")]
    public void MyMethod (T value)
    {
        Console.WriteLine (typeof (U));
    }
}
```

**Причина**: `T` экспорта параметров в Objective-C `MyMethod` должен поддерживать `NSObject`, неограниченный тип `U` не является частью сигнатуры.
    
### <a name="instantiations-of-generic-types-from-objective-c"></a>Создание экземпляров универсальных типов из Objective-C

Создание экземпляров универсальных типов из Objective-C не допускается. Обычно это происходит, когда управляемый тип используется в файл xib или раскадровке.

Рассмотрим это определение класса, который предоставляет конструктор, принимающий `IntPtr` (Xamarin.iOS способ создания C# объекта из собственного экземпляра Objective-C):
    
```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}
```

Хотя выше конструкции годится, во время выполнения, то будет создано исключение во Objective-C пытается создать его экземпляр.

Это происходит потому, что Objective-C не поддерживает понятие универсальных типов, и он не может задавать точный универсальный тип для создания.

Эту проблему можно обойти, создав специализированный подкласс универсального типа. Пример:
    
```csharp
class Generic<T> : NSObject where T : NSObject
{
    public Generic () {}
    public Generic (IntPtr ptr) : base (ptr) {}
}

class GenericUIView : Generic<UIView>
{
}
```

Теперь отсутствует неоднозначность, класс `GenericUIView` может использоваться в файлы xib или раскадровке.

## <a name="no-support-for-generic-methods"></a>Отсутствует поддержка универсальных методов

### <a name="generic-methods-are-not-allowed"></a>Универсальные методы не допускаются.

Следующий код не будет компилироваться.

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyMethod<T> (T argument)
    {
    }
}
```

**Причина**: Это недопустимо, так как Xamarin.iOS не знает, что тип, используемый для аргумента типа `T` при вызове метода из Objective-C.

Альтернативным вариантом является создание это специальный метод, а вместо этого экспорт:

```csharp
class MyClass : NSObject
{
    [Export ("myMethod")]
    public void MyUIViewMethod (UIView argument)
    {
        MyMethod<UIView> (argument);
    }
    public void MyMethod<T> (T argument)
    {
    }
}
```

### <a name="no-exported-static-members-allowed"></a>Не допускается экспортированного статические члены

Если она размещается внутри универсального подкласс можно не предоставлять статические элементы в Objective-C `NSObject`.

Пример неподдерживаемым сценарием.

```csharp
class Generic<T> : NSObject where T : NSObject
{
    [Export ("myMethod:")]
    public static void MyMethod ()
    {
    }

    [Export ("myProperty")]
    public static T MyProperty { get; set; }
}
```

**Причина:** Так же, как универсальные методы, необходимо определить, какой тип для аргумента универсального типа среда выполнения Xamarin.iOS `T`.

К членам экземпляра используется сам экземпляр (так, как никогда не будет экземпляр `Generic<T>`, всегда будет иметь `Generic<SomeSpecificClass>`), но для статических элементов эта информация отсутствует.

Обратите внимание, что это относится даже к рассматриваемый элемент не использует аргумент типа `T` любым способом.

Вместо этого в данном случае является создание специализированным подклассом.

```csharp
class GenericUIView : Generic<UIView>
{
    [Export ("myUIViewMethod")]
    public static void MyUIViewMethod ()
    {
        MyMethod ();
    }

    [Export ("myProperty")]
    public static UIView MyUIViewProperty {
        get { return MyProperty; }
        set { MyProperty = value; }
    }
}

class Generic<T> : NSObject where T : NSObject
{
    public static void MyMethod () {}
    public static T MyProperty { get; set; }
}
```

## <a name="performance"></a>Производительность

Статические регистратор не удается разрешить член, экспортированный в универсальном типе во время сборки, как обычно, при этом приходится искать во время выполнения. Это означает, что вызов метода из Objective-C является немного медленнее, чем вызов членов из не являющихся универсальными.

