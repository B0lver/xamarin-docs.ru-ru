---
Title: Description: ' особенности платформы позволяют использовать функциональные возможности, доступные только на определенной платформе, без реализации пользовательских модулей подготовки отчетов или эффектов. В этой статье объясняется, как использовать конкретную платформу iOS, которая отключает Xamarin.Forms цветовой режим прежних версий.
MS. произв. MS. AssetID: MS. Technology: Автор: MS. author: MS. Дата: нет-Loc:
- 'Xamarin.Forms'
- 'Xamarin.Essentials'

---

# <a name="visualelement-legacy-color-mode-on-ios"></a>Устаревший цветовой режим Висуалелемент в iOS

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)

Некоторые Xamarin.Forms представления имеют устаревший цветовой режим. В этом режиме, если [`IsEnabled`](xref:Xamarin.Forms.VisualElement.IsEnabled) свойство представления имеет значение `false` , представление переопределит цвета, заданные пользователем, с помощью собственных цветов по умолчанию для отключенного состояния. Для обеспечения обратной совместимости этот стандартный цветовой режим по умолчанию для поддерживаемых представлений остается прежним.

Эта платформа iOS отключается для этого устаревшего цветового режима в [`VisualElement`](xref:Xamarin.Forms.VisualElement) , поэтому цвета, заданные пользователем в представлении, остаются даже в том случае, если представление отключено. Он используется в XAML путем присвоения [`VisualElement.IsLegacyColorModeEnabled`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific.VisualElement.IsLegacyColorModeEnabledProperty) свойству присоединенного свойства значения `false` :

```xaml
<ContentPage ...
             xmlns:ios="clr-namespace:Xamarin.Forms.PlatformConfiguration.iOSSpecific;assembly=Xamarin.Forms.Core">
    <StackLayout>
        ...
        <Button Text="Button"
                TextColor="Blue"
                BackgroundColor="Bisque"
                ios:VisualElement.IsLegacyColorModeEnabled="False" />
        ...
    </StackLayout>
</ContentPage>
```

Кроме того, его можно использовать в C# с помощью API-интерфейса Fluent:

```csharp
using Xamarin.Forms.PlatformConfiguration;
using Xamarin.Forms.PlatformConfiguration.iOSSpecific;
...

_legacyColorModeDisabledButton.On<iOS>().SetIsLegacyColorModeEnabled(false);
```

`VisualElement.On<iOS>`Метод указывает, что эта платформа будет запускаться только в iOS. [ `VisualElement.SetIsLegacyColorModeEnabled` ] (Xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Сетислегациколормодинаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент}, System. Boolean)) в [`Xamarin.Forms.PlatformConfiguration.iOSSpecific`](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific) пространстве имен используется для управления тем, отключен ли устаревший цветовой режим. Кроме того, [ `VisualElement.GetIsLegacyColorModeEnabled` ] (xref: Xamarin.Forms . Платформконфигуратион. ИосспеЦифик. Висуалелемент. Жетислегациколормодинаблед ( Xamarin.Forms . Иплатформелементконфигуратион { Xamarin.Forms . Платформконфигуратион. iOS, Xamarin.Forms . Висуалелемент})). можно использовать метод, чтобы возвращать, отключен ли устаревший цветовой режим.

В результате можно отключить устаревший цветовой режим, чтобы цвета, заданные для представления пользователем, оставались даже при отключенном представлении:

![](legacy-color-mode-images/legacy-color-mode-disabled.png "Legacy color mode disabled")

> [!NOTE]
> При задании [`VisualStateGroup`](xref:Xamarin.Forms.VisualStateGroup) для представления устаревший цветовой режим не учитывается полностью. Дополнительные сведения о визуальных состояниях см. [в разделе Xamarin.Forms Диспетчер визуальных состояний](~/xamarin-forms/user-interface/visual-state-manager.md).

## <a name="related-links"></a>Связанные ссылки

- [ПлатформспеЦификс (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/userinterface-platformspecifics)
- [Создание особенностей платформы](~/xamarin-forms/platform/platform-specifics/index.md#creating-platform-specifics)
- [API ИосспеЦифик](xref:Xamarin.Forms.PlatformConfiguration.iOSSpecific)
