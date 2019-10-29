---
title: Оптимизация сборки
description: В этом документе объясняются различные оптимизации, применяемые во время сборки для приложений Xamarin. iOS и Xamarin. Mac.
ms.prod: xamarin
ms.assetid: 84B67E31-B217-443D-89E5-CFE1923CB14E
author: davidortinau
ms.author: daortin
ms.date: 04/16/2018
ms.openlocfilehash: 22028743742a618bd7347d5e49153defecd4e3bb
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73015174"
---
# <a name="build-optimizations"></a>Оптимизация сборки

В этом документе объясняются различные оптимизации, применяемые во время сборки для приложений Xamarin. iOS и Xamarin. Mac.

## <a name="remove-uiapplicationensureuithread--nsapplicationensureuithread"></a>Удалите UIApplication. Енсуреуисреад/Нсаппликатион. Енсуреуисреад

Удаляет вызовы [UIApplication. енсуреуисреад][1] (для Xamarin. IOS) или `NSApplication.EnsureUIThread` (для Xamarin. Mac).

Эта оптимизация изменит следующий тип кода:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    global::UIKit.UIApplication.EnsureUIThread ();
    // ...
}
```

в следующие:

```csharp
public virtual void AddChildViewController (UIViewController childController)
{
    // ...
}
```

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

По умолчанию он включен для сборок выпуска.

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]remove-uithread-checks` mtouch/MMP.

[1]: https://docs.microsoft.com/dotnet/api/UIKit.UIApplication.EnsureUIThread

## <a name="inline-intptrsize"></a>Встроенный IntPtr. size

Встроенное значение `IntPtr.Size` в соответствии с целевой платформой.

Эта оптимизация изменит следующий тип кода:

```csharp
if (IntPtr.Size == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

в следующее (при создании для 64-разрядной платформы):

```csharp
if (8 == 8) {
    Console.WriteLine ("64-bit platform");
} else {
    Console.WriteLine ("32-bit platform");
}
```

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

По умолчанию он включен при нацеливании на одну архитектуру или на сборку платформы (**Xamarin. iOS. dll**, **Xamarin. TVOS. dll**, **Xamarin. WatchOS. dll** или **Xamarin. Mac. dll**).

При использовании нескольких архитектур эта оптимизация создает разные сборки для 32-разрядной версии и 64-разрядной версии приложения, и обе версии должны быть включены в приложение, эффективно увеличивая окончательный размер приложения, а не уменьшая им.

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]inline-intptr-size` mtouch/MMP.

## <a name="inline-nsobjectisdirectbinding"></a>Встроенный Нсобжект. Исдиректбиндинг

`NSObject.IsDirectBinding` — это свойство экземпляра, которое определяет, имеет ли конкретный экземпляр тип оболочки или нет (тип оболочки является управляемым типом, который сопоставляется с собственным типом; например, тип управляемого `UIKit.UIView` сопоставляется с типом собственного `UIView`, а наоборот — с типом пользователя. , в данном случае `class MyUIView : UIKit.UIView` будет типом пользователя).

Необходимо иметь представление о значении `IsDirectBinding` при вызове цели-C, поскольку значение определяет используемую версию `objc_msgSend`.

При наличии только следующего кода:

```csharp
class UIView : NSObject {
    public virtual string SomeProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class NSUrl : NSObject {
    public virtual string SomeOtherProperty {
        get {
            if (IsDirectBinding) {
                return "true";
            } else {
                return "false"
            }
        }
    }
}

class MyUIView : UIView {
}
```

Можно определить, что в `UIView.SomeProperty` значение `IsDirectBinding` не является константой и не может быть встроенным:

```csharp
void uiView = new UIView ();
Console.WriteLine (uiView.SomeProperty); /* prints 'true' */
void myView = new MyUIView ();
Console.WriteLine (myView.SomeProperty); // prints 'false'
```

Однако можно просмотреть все типы в приложении и определить, что не существует типов, которые наследуют от `NSUrl`, и, таким образом, можно обеспечить возможность встраивания `IsDirectBinding` значения в константную `true`:

```csharp
void myURL = new NSUrl ();
Console.WriteLine (myURL.SomeOtherProperty); // prints 'true'
// There's no way to make SomeOtherProperty print anything but 'true', since there are no NSUrl subclasses.
```

В частности, эта оптимизация изменит код следующего типа (это код привязки для `NSUrl.AbsoluteUrl`):

```csharp
if (IsDirectBinding) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

в следующее (когда можно определить, что в приложении нет подклассов `NSUrl`):

```csharp
if (true) {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSend (this.Handle, Selector.GetHandle ("absoluteURL")));
} else {
    return Runtime.GetNSObject<NSUrl> (global::ObjCRuntime.Messaging.IntPtr_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("absoluteURL")));
}
```

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

Он всегда включен по умолчанию для Xamarin. iOS и всегда отключен по умолчанию для Xamarin. Mac (поскольку можно динамически загружать сборки в Xamarin. Mac, невозможно определить, что определенный класс никогда не является подклассом).

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]inline-isdirectbinding` mtouch/MMP.

## <a name="inline-runtimearch"></a>Встроенная среда выполнения. Arch

Эта оптимизация изменит следующий тип кода:

```csharp
if (Runtime.Arch == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

в следующее (при сборке для устройства):

```csharp
if (Arch.DEVICE == Arch.DEVICE) {
    Console.WriteLine ("Running on device");
} else {
    Console.WriteLine ("Running in the simulator");
}
```

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

Он всегда включен по умолчанию для Xamarin. iOS (он недоступен для Xamarin. Mac).

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]inline-runtime-arch` mtouch.

## <a name="dead-code-elimination"></a>Исключение неработающего кода

Эта оптимизация изменит следующий тип кода:

```csharp
if (true) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

бить

```csharp
Console.WriteLine ("Doing this");
```

Он также будет оценивать постоянные сравнения, как в следующем:

```csharp
if (8 == 8) {
    Console.WriteLine ("Doing this");
} else {
    Console.WriteLine ("Not doing this");
}
```

и определить, что выражение `8 == 8` является всегда истинным, и уменьшите его до:

```csharp
Console.WriteLine ("Doing this");
```

Это мощная оптимизация при использовании вместе с оптимизациями встраивания, поскольку она может преобразовывать код следующего типа (это код привязки для `NFCIso15693ReadMultipleBlocksConfiguration.Range`):

```csharp
NSRange ret;
if (IsDirectBinding) {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret (out ret, this.Handle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
    }
} else {
    if (Runtime.Arch == Arch.DEVICE) {
        if (IntPtr.Size == 8) {
            ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
        } else {
            global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret (out ret, this.SuperHandle, Selector.GetHandle ("range"));
        }
    } else if (IntPtr.Size == 8) {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    } else {
        ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("range"));
    }
}
return ret;
```

в это (при создании для 64-разрядного устройства, а также в том случае, если в приложении не существует `NFCIso15693ReadMultipleBlocksConfiguration` подклассов):

```csharp
NSRange ret;
ret = global::ObjCRuntime.Messaging.NSRange_objc_msgSend (this.Handle, Selector.GetHandle ("range"));
return ret;
```

Компилятор AOT уже может исключить неиспользуемый код, но такой способ оптимизации выполняется внутри компоновщика. Это означает, что компоновщик может видеть, что существует несколько методов, которые больше не используются, и поэтому могут быть удалены (если не используется в других местах). :

* `global::ObjCRuntime.Messaging.NSRange_objc_msgSend_stret`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper`
* `global::ObjCRuntime.Messaging.NSRange_objc_msgSendSuper_stret`

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

Он всегда включен по умолчанию (если компоновщик включен).

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]dead-code-elimination` mtouch/MMP.

## <a name="optimize-calls-to-blockliteralsetupblock"></a>Оптимизация вызовов Блокклитерал. Сетупблокк

В среде выполнения Xamarin. iOS/Mac должна быть известна сигнатура блока при создании блока цели-C для управляемого делегата. Это может быть довольно дорогостоящая операция. Эта оптимизация вычисляет подпись блока во время сборки и изменяет IL для вызова метода `SetupBlock`, который вместо этого принимает сигнатуру в качестве аргумента. Это позволяет избежать необходимости в вычислении подписи во время выполнения.

Тесты производительности показывают, что это ускоряет вызов блока на множители от 10 до 15.

Будет преобразован следующий [код](https://github.com/xamarin/xamarin-macios/blob/018f7153441d9d7e0f58e2046f39eeb46f1ff480/src/UIKit/UIAccessibility.cs#L198-L211):

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlock (callback, completionHandler);
    // ...
}
```

бить

```csharp
public static void RequestGuidedAccessSession (bool enable, Action<bool> completionHandler)
{
    // ...
    block_handler.SetupBlockImpl (callback, completionHandler, true, "v@?B");
    // ...
}
```

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

Он включен по умолчанию при использовании статического регистратора (в Xamarin. iOS статический регистратор включен по умолчанию для сборок устройств, а в Xamarin. Mac статический регистратор включен по умолчанию для сборок выпуска).

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]blockliteral-setupblock` mtouch/MMP.

## <a name="optimize-support-for-protocols"></a>Оптимизация поддержки протоколов

Среда выполнения Xamarin. iOS/Mac требует сведения о том, как управляемые типы реализуют протоколы цели-C. Эти сведения хранятся в интерфейсах (и атрибутах в этих интерфейсах), которые не являются очень эффективным форматом и не являются дружественными для компоновщика.

Одним из примеров является то, что эти интерфейсы хранят сведения обо всех членах протокола в атрибуте `[ProtocolMember]`, который, помимо прочего, содержит ссылки на типы параметров этих элементов. Это означает, что при простой реализации такого интерфейса компоновщик сохранит все типы, используемые в этом интерфейсе, даже для необязательных элементов, которые приложение никогда не вызывает или не реализует.

В результате этой оптимизации статический регистратор будет хранить все необходимые сведения в эффективном формате, который использует небольшую память, которая легко и быстро находит во время выполнения.

Он также обучить компоновщику, что ему не обязательно нужно сохранять эти интерфейсы или ни один из связанных атрибутов.

Для этой оптимизации требуется включить компоновщик и статический регистратор.

В Xamarin. iOS эта оптимизация включена по умолчанию, если включены и компоновщик, и статический регистратор.

В Xamarin. Mac эта оптимизация никогда не включена по умолчанию, так как Xamarin. Mac поддерживает загрузку сборок динамически, и эти сборки могут быть не известны во время сборки (и, таким же, не оптимизированы).

Поведение по умолчанию можно переопределить, передав `--optimize=-register-protocols` mtouch/MMP.

## <a name="remove-the-dynamic-registrar"></a>Удаление динамического регистратора

Как Xamarin. iOS, так и среда выполнения Xamarin. Mac включают поддержку [регистрации управляемых типов](~/ios/internals/registrar.md) с помощью среды выполнения цели-C. Это может быть сделано во время сборки или в среде выполнения (или частично во время сборки, а остальное в среде выполнения), но если оно полностью выполняется во время сборки, это означает, что вспомогательный код для его выполнения в среде может быть удален. Это приводит к значительному уменьшению размера приложения, в частности для небольших приложений, таких как расширения или приложения watchOS.

Для этой оптимизации требуется включить статический регистратор и компоновщик.

Компоновщик попытается определить, можно ли удалить динамический регистратор, и, если да, будет пытаться удалить его.

Так как Xamarin. Mac поддерживает динамическую загрузку сборок во время выполнения (которые не были известны в момент сборки), во время сборки невозможно определить, является ли это безнадежной оптимизацией. Это означает, что эта оптимизация никогда не включена по умолчанию для приложений Xamarin. Mac.

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]remove-dynamic-registrar` mtouch/MMP.

Если значение по умолчанию переопределено для удаления динамического регистратора, компоновщик выдаст предупреждения, если обнаружит, что он незащищен (но динамический регистратор по-прежнему будет удален).

## <a name="inline-runtimedynamicregistrationsupported"></a>Встроенная среда выполнения. Динамикрегистратионсуппортед

Встроенное значение `Runtime.DynamicRegistrationSupported`, определенное во время сборки.

Если динамический регистратор удален (см. раздел [Удаление оптимизации динамического регистратора](#remove-the-dynamic-registrar) ), это константа, `false` значение, в противном случае — константа `true` значение.

Эта оптимизация изменит следующий тип кода:

```csharp
if (Runtime.DynamicRegistrationSupported) {
    Console.WriteLine ("do something");
} else {
    throw new Exception ("dynamic registration is not supported");
}
```

в следующее время удаления динамического регистратора:

```csharp
throw new Exception ("dynamic registration is not supported");
```

в следующее время, когда не удаляется динамический регистратор:

```csharp
Console.WriteLine ("do something");
```

Для этой оптимизации компоновщик должен быть включен и применяться только к методам с атрибутом `[BindingImpl (BindingImplOptions.Optimizable)]`.

Он всегда включен по умолчанию (если компоновщик включен).

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]inline-dynamic-registration-supported` mtouch/MMP.

## <a name="precompute-methods-to-create-managed-delegates-for-objective-c-blocks"></a>Предварительное вычисление методов для создания управляемых делегатов для блоков цели-C

Когда цель-C вызывает селектор, который принимает блок в качестве параметра, а затем управляемый код перезаписал этот метод, среда выполнения Xamarin. iOS/Xamarin. Mac должна создать делегат для этого блока.

Код привязки, создаваемый генератором привязки, будет включать атрибут `[BlockProxy]`, который указывает тип с помощью метода `Create`, который может сделать это.

Учитывая следующий код цели-C:

```objc
@interface ObjCBlockTester : NSObject {
}
-(void) classCallback: (void (^)())completionHandler;
-(void) callClassCallback;
@end

@implementation ObjCBlockTester
-(void) classCallback: (void (^)())completionHandler
{
}

-(void) callClassCallback
{
    [self classCallback: ^()
    {
        NSLog (@"called!");
    }];
}
@end
```

и следующий код привязки:

```csharp
[BaseType (typeof (NSObject))]
interface ObjCBlockTester
{
    [Export ("classCallback:")]
    void ClassCallback (Action completionHandler);
}
```

Генератор создаст:

```csharp
[Register("ObjCBlockTester", true)]
public unsafe partial class ObjCBlockTester : NSObject {
    // unrelated code...

    [Export ("callClassCallback")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public virtual void CallClassCallback ()
    {
        if (IsDirectBinding) {
            ApiDefinition.Messaging.void_objc_msgSend (this.Handle, Selector.GetHandle ("callClassCallback"));
        } else {
            ApiDefinition.Messaging.void_objc_msgSendSuper (this.SuperHandle, Selector.GetHandle ("callClassCallback"));
        }
    }

    [Export ("classCallback:")]
    [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
    public unsafe virtual void ClassCallback ([BlockProxy (typeof (Trampolines.NIDActionArity1V0))] System.Action completionHandler)
    {
        // ...

    }
}

static class Trampolines
{
    [UnmanagedFunctionPointerAttribute (CallingConvention.Cdecl)]
    [UserDelegateType (typeof (System.Action))]
    internal delegate void DActionArity1V0 (IntPtr block);

    static internal class SDActionArity1V0 {
        static internal readonly DActionArity1V0 Handler = Invoke;

        [MonoPInvokeCallback (typeof (DActionArity1V0))]
        static unsafe void Invoke (IntPtr block) {
            var descriptor = (BlockLiteral *) block;
            var del = (System.Action) (descriptor->Target);
            if (del != null)
                del (obj);
        }
    }

    internal class NIDActionArity1V0 {
        IntPtr blockPtr;
        DActionArity1V0 invoker;

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe NIDActionArity1V0 (BlockLiteral *block)
        {
            blockPtr = _Block_copy ((IntPtr) block);
            invoker = block->GetDelegateForBlock<DActionArity1V0> ();
        }

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        ~NIDActionArity1V0 ()
        {
            _Block_release (blockPtr);
        }

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        public unsafe static System.Action Create (IntPtr block)
        {
            if (block == IntPtr.Zero)
                return null;
            if (BlockLiteral.IsManagedBlock (block)) {
                var existing_delegate = ((BlockLiteral *) block)->Target as System.Action;
                if (existing_delegate != null)
                    return existing_delegate;
            }
            return new NIDActionArity1V0 ((BlockLiteral *) block).Invoke;
        }

        [Preserve (Conditional=true)]
        [BindingImpl (BindingImplOptions.GeneratedCode | BindingImplOptions.Optimizable)]
        unsafe void Invoke ()
        {
            invoker (blockPtr);
        }
    }
}
```

Когда цель-C вызывает `[ObjCBlockTester callClassCallback]`, среда выполнения Xamarin. iOS/Xamarin. Mac будет просматривать атрибут `[BlockProxy (typeof (Trampolines.NIDActionArity1V0))]` параметра. Затем он ищет метод `Create` для этого типа и вызывает этот метод для создания делегата.

Эта оптимизация найдет метод `Create` во время сборки, а статический регистратор создаст код, который ищет метод во время выполнения, используя маркеры метаданных, а не использует атрибут и отражение (это происходит гораздо быстрее, а также позволяет компоновщику удалите соответствующий код среды выполнения, сделав приложение меньшим.

Если MMP/mtouch не удается найти метод `Create`, будет отображено предупреждение MT4174/MM4174, и поиск будет выполняться во время выполнения.
Наиболее вероятной причиной является написание вручную кода привязки без обязательных атрибутов `[BlockProxy]`.

Для этой оптимизации требуется включить статический регистратор.

Он всегда включен по умолчанию (при условии, что включен статический регистратор).

Поведение по умолчанию можно переопределить, передав `--optimize=[+|-]static-delegate-to-block-lookup` mtouch/MMP.
