---
title: Обновление существующих приложений Xamarin. Forms
description: В этом документе описаны действия, которые необходимо выполнить для обновления приложения Xamarin. Forms с Classic API на Unified API.
ms.prod: xamarin
ms.assetid: C2F0D1D1-256D-44A4-AAC9-B06A0CB41E70
author: davidortinau
ms.author: daortin
ms.date: 03/29/2017
ms.openlocfilehash: 48519431a65fba0cdc61062021ad86fb53854ef3
ms.sourcegitcommit: 3ea19e3a51515b30349d03c70a5b3acd7eca7fe7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/01/2019
ms.locfileid: "73425461"
---
# <a name="updating-existing-xamarinforms-apps"></a>Обновление существующих приложений Xamarin. Forms

_Выполните следующие действия, чтобы обновить существующее приложение Xamarin. Forms для использования Unified API и Update до версии 1.3.1_

> [!IMPORTANT]
> Так как Xamarin. Forms 1.3.1 является первым выпуском, поддерживающим Unified API, все решение должно быть обновлено для использования последней версии одновременно с переносом приложения iOS в единое время. Это означает, что в дополнение к обновлению проекта iOS для единой поддержки вам также потребуется изменить код во _всех_ проектах решения.

Обновление выполняется в два этапа:

1. Перенесите приложение iOS в Unified API с помощью сборки Visual Studio для Mac в средстве миграции.

    - Используйте средство миграции для автоматического обновления проекта.

    - Обновите собственные API-интерфейсы iOS, как описано в инструкциях по [обновлению приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md) (в частности, в коде настраиваемого модуля подготовки отчетов или службы зависимостей).

2. Обновите все решение до Xamarin. Forms версии 1,3.

    1. Установите пакет NuGet Xamarin. Forms 1.3.1.

    2. Обновите класс `App` в общем коде.

    3. Обновите `AppDelegate` в проекте iOS.

    4. Обновите `MainActivity` в проекте Android.

    5. Обновите `MainPage` в проекте Windows Phone.

## <a name="1-ios-app-unified-migration"></a>1. приложение iOS (унифицированная миграция)

Часть миграции требует обновления Xamarin. Forms до версии 1,3, которая поддерживает Unified API. Чтобы создать правильные ссылки на сборки, сначала необходимо обновить проект iOS для использования Unified API.

### <a name="migration-tool"></a>Средство миграции

Щелкните проект iOS, чтобы он был выбран, а затем выберите **проект > перейти на Xamarin. iOS Unified API...** и примите предупреждающее сообщение, которое появляется.

![](updating-xamarin-forms-apps-images/beta-tool1.png "Choose Project > Migrate to Xamarin.iOS Unified API... and agree to the warning message that appears")

Это будет автоматически:

- Измените тип проекта для поддержки унифицированного 64-разрядного API.
- Измените ссылку на платформу на **Xamarin. iOS** (замените старую ссылку на **касание** ).
- Измените ссылки на пространство имен в коде, чтобы удалить префикс `MonoTouch`.
- Обновите файл **CSPROJ** , чтобы использовать правильные целевые объекты сборки для Unified API.

**Очистите** и выполните **сборку** проекта, чтобы убедиться в отсутствии других ошибок для исправления. Никаких дополнительных действий не требуется. Эти шаги более подробно описаны в [Unified API документах](~/cross-platform/macios/unified/updating-ios-apps.md).

### <a name="update-native-ios-apis-if-required"></a>Обновление собственных интерфейсов API iOS (при необходимости)

Если добавлен дополнительный машинный код iOS (например, пользовательские модули подготовки отчетов или службы зависимостей), может потребоваться выполнить дополнительные исправления кода вручную. Выполните повторную компиляцию приложения и ознакомьтесь с [инструкциями по обновлению существующих приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md) , чтобы получить дополнительные сведения об изменениях, которые могут потребоваться. [Эти советы](~/cross-platform/macios/unified/updating-tips.md) также помогут определить необходимые изменения.

## <a name="2-xamarinforms-131-update"></a>2. Xamarin. Forms 1.3.1 обновление

После обновления приложения iOS до Unified API необходимо обновить остальную часть решения до Xamarin. Forms версии 1.3.1. В том числе следующее:

- Обновление пакета NuGet Xamarin. Forms в каждом проекте.
- Изменение кода для использования новых классов Xamarin. Forms `Application`, `FormsApplicationDelegate` (iOS), `FormsApplicationActivity` (Android) и `FormsApplicationPage` (Windows Phone).

Ниже описаны эти действия.

### <a name="21-update-nuget-in-all-projects"></a>2,1. обновление NuGet во всех проектах

Обновите Xamarin. Forms до 1.3.1 предварительной версии с помощью диспетчера пакетов NuGet для всех проектов в решении: PCL (при наличии), iOS, Android и Windows Phone. Рекомендуется **Удалить и повторно добавить** пакет NuGet для Xamarin. Forms, чтобы обновить его до версии 1,3.

> [!NOTE]
> Версия Xamarin. Forms 1.3.1 в настоящее время находится в *предварительной версии*. Это означает, что необходимо выбрать вариант **предварительной версии** в NuGet (через флажок в Visual Studio для Mac или раскрывающийся список в Visual Studio) для просмотра последней предварительной версии.

> [!IMPORTANT]
> Если вы используете Visual Studio, убедитесь, что установлена последняя версия диспетчера пакетов NuGet. Более старые версии NuGet в Visual Studio не будут правильно устанавливать единую версию Xamarin. Forms 1.3.1. Выберите **инструменты > расширения и обновления...** и щелкните список **установленных** , чтобы убедиться, что **Диспетчер пакетов NuGet для Visual Studio** имеет версию не ниже 2.8.5. Если это старое, щелкните список **обновлений** , чтобы скачать последнюю версию.

После обновления пакета NuGet до Xamarin. Forms 1.3.1 Внесите следующие изменения в каждом проекте, чтобы выполнить обновление до нового класса `Xamarin.Forms.Application`.

### <a name="22-portable-class-library-or-shared-project"></a>2,2. Переносимая библиотека классов (или общий проект)

Измените файл **app.CS** таким образом, чтобы:

- Класс `App` теперь наследуется от `Application`.
- Для свойства `MainPage` задана первая страница содержимого, которую вы хотите отобразить.

```csharp
public class App : Application // superclass new in 1.3
{
    public App ()
    {
        // The root page of your application
        MainPage = new ContentPage {...}; // property new in 1.3
    }
```

Мы полностью удалили метод `GetMainPage`, а вместо этого установили *свойство* `MainPage` в подклассе `Application`.

Этот новый `Application` базовый класс также поддерживает переопределения `OnStart`, `OnSleep`и `OnResume`, позволяющие управлять жизненным циклом приложения.

Затем класс `App` передается в новый метод `LoadApplication` в каждом проекте приложения, как описано ниже.

### <a name="23-ios-app"></a>2,3 приложение iOS

Измените файл **AppDelegate.CS** таким образом, чтобы:

- Класс наследует от `FormsApplicationDelegate` (вместо `UIApplicationDelegate` ранее).
- `LoadApplication` вызывается с новым экземпляром `App`.

```csharp
[Register ("AppDelegate")]
public partial class AppDelegate :
    global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate // superclass new in 1.3
{
    public override bool FinishedLaunching (UIApplication app, NSDictionary options)
    {
        global::Xamarin.Forms.Forms.Init ();

        LoadApplication (new App ());  // method is new in 1.3

        return base.FinishedLaunching (app, options);
    }
}
```

### <a name="23-android-app"></a>2,3 приложение Android

Измените файл **MainActivity.CS** таким образом, чтобы:

- Класс наследует от `FormsApplicationActivity` (вместо `FormsActivity` ранее).
- `LoadApplication` вызывается с новым экземпляром `App`

```csharp
[Activity (Label = "YOURAPPNAM", Icon = "@drawable/icon", MainLauncher = true,
ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity :
    global::Xamarin.Forms.Platform.Android.FormsApplicationActivity // superclass new in 1.3
{
    protected override void OnCreate (Bundle bundle)
    {
        base.OnCreate (bundle);

        global::Xamarin.Forms.Forms.Init (this, bundle);

        LoadApplication (new App ()); // method is new in 1.3
    }
}
```

### <a name="24-windows-phone-app"></a>2,4 Windows Phone приложение

Нам нужно обновить **MainPage** и XAML, и код поддержки.

Измените файл **MainPage. XAML** , чтобы:

- Корневой элемент XAML должен быть `winPhone:FormsApplicationPage`.
- Атрибут `xmlns:phone` должен быть *изменен* на `xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"`

Ниже приведен обновленный пример. необходимо только изменить эти объекты (остальные атрибуты должны остаться одинаковыми):

```xml
<winPhone:FormsApplicationPage
   ...
   xmlns:winPhone="clr-namespace:Xamarin.Forms.Platform.WinPhone;assembly=Xamarin.Forms.Platform.WP8"
    ...>
</winPhone:FormsApplicationPage>
```

Измените файл **MainPage.XAML.CS** таким образом, чтобы:

- Класс наследует от `FormsApplicationPage` (вместо `PhoneApplicationPage` ранее).
- `LoadApplication` вызывается с новым экземпляром класса `App` Xamarin. Forms. Может потребоваться полная квалификация этой ссылки, так как Windows Phone имеет собственный класс `App` уже определен.

```csharp
public partial class MainPage : global::Xamarin.Forms.Platform.WinPhone.FormsApplicationPage // superclass new in 1.3
{
    public MainPage()
    {
        InitializeComponent();
        SupportedOrientations = SupportedPageOrientation.PortraitOrLandscape;

        global::Xamarin.Forms.Forms.Init();
        LoadApplication(new YOUR_APP_NAMESPACE.App()); // new in 1.3
    }
 }
```

### <a name="troubleshooting"></a>Устранение неполадок

Иногда после обновления пакета NuGet Xamarin. Forms вы увидите сообщение об ошибке, аналогичное следующему: Это происходит, когда компонент обновления NuGet не полностью удаляет ссылки на старые версии из файлов **CSPROJ** .

>Ваш\_проект. csproj: ошибка: этот проект ссылается на пакеты NuGet, отсутствующие на этом компьютере. Включите восстановление пакета NuGet, чтобы скачать их.  Для получения дополнительной информации см. https://go.microsoft.com/fwlink/?LinkID=322105. Отсутствующий файл:.. /.. /packages/Xamarin.Forms.1.2.3.6257/build/portable-win + net45 + wp80 + MonoAndroid10 + MonoTouch10/Xamarin. Forms. targets. (Ваш проект\_)

Чтобы устранить эти ошибки, откройте **CSPROJ** -файл в текстовом редакторе и найдите элементы `<Target`, которые ссылаются на более ранние версии Xamarin. Forms, например элемент, показанный ниже. Необходимо вручную удалить весь элемент из файла **CSPROJ** и сохранить изменения.

```csharp
  <Target Name="EnsureNuGetPackageBuildImports" BeforeTargets="PrepareForBuild">
    <PropertyGroup>
      <ErrorText>This project references NuGet package(s) that are missing on this computer. Enable NuGet Package Restore to download them.  For more information, see https://go.microsoft.com/fwlink/?LinkID=322105. The missing file is {0}.</ErrorText>
    </PropertyGroup>
    <Error Condition="!Exists('..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets')" Text="$([System.String]::Format('$(ErrorText)', '..\..\packages\Xamarin.Forms.1.2.3.6257\build\portable-win+net45+wp80+MonoAndroid10+MonoTouch10\Xamarin.Forms.targets'))" />
  </Target>
```

После удаления этих старых ссылок проект должен быть успешно построен.

## <a name="considerations"></a>Особенности

При преобразовании существующего проекта Xamarin. Forms из Classic API в новый Unified API следует учитывать следующие моменты, если это приложение полагается на один или несколько компонентов или пакетов NuGet.

### <a name="components"></a>Компоненты

Любой компонент, который вы включили в приложение, также потребуется обновить до Unified API или при попытке компиляции возникнет конфликт. Для любого включаемого компонента замените текущую версию новой версией из хранилища компонентов Xamarin, которое поддерживает Unified API и выполните чистую сборку. Все компоненты, которые еще не были преобразованы автором, будут отображать предупреждение только 32 бит в хранилище компонентов.

### <a name="nuget-support"></a>Поддержка NuGet

Хотя мы внесены изменения в NuGet для работы со службой поддержки Unified API, новый выпуск NuGet не был новым, поэтому мы рассказывающие, как получить NuGet для распознавания новых интерфейсов API.

До этого времени, как и для компонентов, необходимо переключить любой пакет NuGet, который вы включили в проект, в версию, поддерживающую унифицированные интерфейсы API, и затем выполнить чистую сборку.

> [!IMPORTANT]
> Если у вас есть ошибка в форме _"Ошибка 3 не может одновременно включать" и ". dll" и "Xamarin. iOS. dll" в одном проекте Xamarin. iOS, на "Xamarin. iOS. dll" имеется явная ссылка, а "0.0.000" — "XXX, Version =, Culture = нейтральный, PublicKeyToken = null ""_ после преобразования приложения в унифицированные API, обычно это происходит из-за наличия в проекте компонента или пакета NuGet, который не был обновлен до Unified API. Необходимо удалить существующий компонент или NuGet, обновить версию, поддерживающую унифицированные API-интерфейсы, и выполнить чистую сборку.

## <a name="enabling-64-bit-builds-of-xamarinios-apps"></a>Включение 64 разрядных сборок приложений Xamarin. iOS

Для мобильного приложения Xamarin. iOS, которое было преобразовано в Unified API, разработчику по-прежнему необходимо включить сборку приложения на 64 разрядных компьютеров из параметров приложения. Подробные инструкции по 64 включению поразрядных сборок см. в статье **включение 64 разрядов для приложений Xamarin. iOS** в документе [рекомендации по платформе 32/64 bit](~/cross-platform/macios/32-and-64/index.md#enable-64) .

## <a name="summary"></a>Сводка

Теперь приложение Xamarin. Forms должно быть обновлено до версии 1.3.1, а приложение iOS перенесено в Unified API (которое поддерживает 64-разрядные архитектуры на платформе iOS).

Как отмечалось выше, если приложение Xamarin. Forms включает машинный код, например пользовательские модули подготовки отчетов или службы зависимостей, для использования новых типов, [появившихся в Unified API](~/cross-platform/macios/index.md), также может потребоваться обновление.

## <a name="related-links"></a>Связанные ссылки

- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-apps.md)
- [Обновление приложений iOS](~/cross-platform/macios/unified/updating-ios-apps.md)
- [Работа с собственными типами в кроссплатформенных приложениях](~/cross-platform/macios/native-types-cross-platform.md)
- [Обновление советов](~/cross-platform/macios/unified/updating-tips.md)
- [Различия между классическими и Unified APIми](https://github.com/xamarin/release-notes-archive/blob/master/release-notes/ios/api_changes/classic-vs-unified-8.6.0/index.md)
