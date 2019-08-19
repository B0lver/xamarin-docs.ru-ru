---
title: Изменения дополнительных платформ iOS 10
description: В этом документе описываются незначительные изменения и улучшения существующих платформ в iOS 10, а затем описывается использование этих обновлений в Xamarin. iOS.
ms.prod: xamarin
ms.assetid: 0E2217F1-FC96-4D0A-ABAB-D40AD8F96502
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 03/29/2017
ms.openlocfilehash: 5aad72de5d894a83d734cd53fce3ac060125d740
ms.sourcegitcommit: 3ea9ee034af9790d2b0dc0893435e997bd06e587
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2019
ms.locfileid: "68656941"
---
# <a name="additional-ios-10-frameworks-changes"></a>Изменения дополнительных платформ iOS 10

_В этой статье рассматриваются дополнительные, незначительные изменения или усовершенствования существующих платформ для iOS 10._

## <a name="av-foundation-framework-additions"></a>Дополнения AV Foundation Framework

Платформа Авфаундатион включает следующие усовершенствования.

- В iOS 10 разработчик больше не должен реализовывать различные поведения [авплайеритем](xref:AVFoundation.AVPlayerItem) на основе типа содержимого. Просто задайте `Rate` свойство, и авфаундатион определит, когда будет доступно достаточное содержимое для воспроизведения без ожидания.
- Новый класс [авкаптурефотуутпут](xref:AVFoundation.AVCaptureFileOutput) заменяет устаревший `AVCaptureStillImageOutput` класс и предоставляет единый метод для обработки всех рабочих процессов фотосъемки, предоставляя сложный контроль и мониторинг процесса записи и поддержку новых такие функции, как динамические фотографии и формат записи RAW.
- Новый `AVPlayerLooper` класс упрощает циклический перебор заданной части мультимедиа во время воспроизведения.
- `AVAssetDownloadURLSession` Класс позволяет загружать и более поздние воспроизводить Fairplay зашифрованные HLS потоки.
- По умолчанию класс [авкаптуресессион](xref:AVFoundation.AVCaptureSession) автоматически поддерживает широкие и широкие записи с широким спектром цветов, если оборудование устройства поддерживает его. Дополнительные сведения см. в справочнике по [совместимости устройств iOS](https://developer.apple.com/library/prerelease/content/documentation/DeviceInformation/Reference/iOSDeviceCompatibility/Introduction/Introduction.html#//apple_ref/doc/uid/TP40013599) Apple.

## <a name="avkit-additions"></a>Дополнения Авкит

Платформа авкит теперь включает новое `UpdatesNowPlayingInfoCenter` свойство, которое указывает, когда будет обновлен информационный центр.

## <a name="core-data-enhancements"></a>Усовершенствования основных данных

в iOS 10 включены следующие усовершенствования основной платформы данных:

- [Нсманажедобжектконтекст](xref:CoreData.NSManagedObjectContext) объекты с хранилищами данных SQLite в режиме журнала Wal поддерживают новую функцию создания запросов, где контексты управляемых объектов (MOC) могут быть закреплены в конкретных версиях базы данных для последующей выборки и сбоя транзакций.
- Корневые объекты [нсманажедобжектконтекст](xref:CoreData.NSManagedObjectContext) поддерживают одновременную ошибку и выборку без сериализации.
- Класс [нсперсистентсторекурдинатор](xref:CoreData.NSPersistentStoreCoordinator) поддерживает пул хранилищ данных SQLite.
- Было добавлено несколько новых удобных методов для `NSManagedObject` упрощения выборки и создания подклассов.
- Использование высокого уровня `NSPersistenceContainer` для ссылки на [нсманажедобжектмодел](xref:CoreData.NSManagedObjectModel) и другие основные ресурсы по `NSPersistentStoreCoordinator`настройке данных.

Дополнительные сведения см. в справочнике по [основной платформе данных](https://developer.apple.com/reference/coredata)Apple.

## <a name="core-image-enhancements"></a>Усовершенствования основных образов

iOS 10 вносит следующие улучшения в основную платформу образов:

- Теперь разработчик может обрабатывать изображения в цветовом пространстве за пределами рабочего пространства контекста образа путем преобразования в цветовое пространство и из него до и после обработки.
- Для устройств iOS, использующих процессоры A8 или A9, теперь поддерживается формат необработанных изображений. Теперь основной образ обеспечивает поддержку декодирования необработанных изображений из встроенной камеры Исигхт или сторонней камеры. Используйте методы `FilterWithImageData` или `FilterWithImageURL` класса [CIFilter](xref:CoreImage.CIFilter) для обработки необработанных изображений.
- Для подготовки к `UIImage` просмотру в `UIImageView` объектах были сделаны некоторые улучшения производительности отрисовки (при резервном копировании в хранилищах образов основных образов). 
- `UIImage`объекты с тегами Wide-охват будут отображаться в виде цвета с широкими охватами в `UIImageView` объектах на устройствах iOS, поддерживающих широкий цвет.
- Код ядра образа ядра теперь может запрашивать определенные форматы вывода пикселей.
- Метод `ImageWithExtent` класса [CIFilter](xref:CoreImage.CIFilter) можно использовать для вставки пользовательской обработки в операцию фильтрации. Основное изображение будет вызывать заданный обратный вызов между фильтрами при обработке изображения для вывода или показа.

Кроме того, добавлены следующие новые фильтры основных образов:

- `CINinePartTiled`
- `CINinePartStretched`
- `CIHueSaturationValueGradient`
- `CIEdgePreserveUpsampleFilter`
- `CIClamp`

## <a name="core-motion-additions"></a>Основные дополнения движения

В iOS 10 Основная платформа Motion включает события педометер, которые позволяют приложению получать быстрые уведомления о приостановке и возобновлении отслеживания во время работы приложения в режиме реального времени. Используйте [кмпедометер](xref:CoreMotion.CMPedometer) для регистрации в качестве основного или фонового события педометер.

## <a name="foundation-enhancements"></a>Усовершенствования в фундаменте

В платформу Foundation для iOS 10 были внесены следующие улучшения.

- Используйте новый класс [нсмеасурементформаттер](https://developer.apple.com/reference/foundation/nsmeasurementformatter) , чтобы отформатировать локализованные измерения для отображения конечному пользователю.
- Новый класс [нсдатеинтервал](https://developer.apple.com/reference/foundation/nsdateinterval) используется для вычисления интервалов даты и времени, таких как длительность, для сравнения интервалов и тестирования для пересечения интервалов.
- Используйте новый класс [нсмеасуремент](https://developer.apple.com/reference/foundation/nsmeasurement) для преобразования между разными единицами измерения (ЕИ) или вычисления значений в разных уомс.

- Используйте новые классы [нсунит](https://developer.apple.com/reference/foundation/nsunit) и [нсдименсион](https://developer.apple.com/reference/foundation/nsdimension) для представления конкретных уомс.
- К классу [нслокал](https://developer.apple.com/reference/foundation/nslocale) были добавлены несколько новых свойств для получения локальных сведений и доступных форматов отображаемых данных.

## <a name="gamekit-enhancements"></a>Усовершенствования GameKit

В GameKit Framework в iOS 10 были внесены следующие улучшения.

- **Game Centerное приложение** устарело и удалено из iOS. Если приложение использует GameKit, оно _должно_ предоставлять собственный интерфейс для отображения таких gamekit функций, как списки лидеров и т. д. 
- Новый тип учетной записи только iCloud реализован классом [гкклаудплайер](https://developer.apple.com/reference/gamekit/gkcloudplayer) .
- Новый класс [гкгамесессион](https://developer.apple.com/reference/gamekit/gkgamesession) предоставляет обобщенное решение для управления хранилищем постоянных данных на Game Center. `GKGameSession`ведет список игроков, и приложение отвечает за реализацию того, как и когда дата участника сохраняется, извлекается или обменивается между игроками. Во многих случаях игровые сеансы могут заменять существующие соответствия, основанные на включении, совпадения в режиме реального времени или постоянные способы сохранения игр.

## <a name="gameplaykit-enhancements"></a>Усовершенствования Гамеплайкит

В Гамеплайкит Framework в iOS 10 были внесены следующие улучшения.

- Используйте новый класс [гкмешграф](https://developer.apple.com/reference/gameplaykit/gkmeshgraph) для обеспечения высокопроизводительных и естественных путей.
- Было добавлено описание процедурного создания шума, которое можно использовать для улучшения реальных текстур в естественном виде, добавления факта к перемещению камеры и помощи в создании полнофункциональных игр.
- Используйте пространственное секционирование, чтобы секционировать реальные данные игры для эффективного поиска.
- Добавлен новый Монте-Карло специалист по стратегиям ([гкмонтекарлостратегист](https://developer.apple.com/reference/gameplaykit/gkmontecarlostrategist)) для исчерпывающего возможного вычисления перемещения.
- поддержка трехмерных объектов добавлена к существующим агентам и поведениям поиска пути с помощью новых классов [GKAgent3D](https://developer.apple.com/reference/gameplaykit/gkagent3d) и [GKGraphNode3D](https://developer.apple.com/reference/gameplaykit/gkgraphnode3d) .
- Новые классы [гксцене](https://developer.apple.com/reference/gameplaykit/gkscene) и [Гкскнодекомпонент](https://developer.apple.com/reference/gameplaykit/gksknodecomponent) делают объединение гамеплайкит и SpriteKit проще, чем когда-либо.
- Добавлен новый API дерева принятия решений ([гкдеЦисионтри](https://developer.apple.com/reference/gameplaykit/gkdecisiontree) и [гкдеЦисионноде](https://developer.apple.com/reference/gameplaykit/gkdecisionnode)) для улучшения искусственного проектирования.

## <a name="healthkit-enhancements"></a>Усовершенствования HealthKit

В HealthKit Framework в iOS 10 были внесены следующие улучшения.

- Добавлены новые ключи метаданных для типов `HKWeatherConditionClear` погоды (например, и `HKWeatherConditionCloudy`), а также были добавлены типы тренировок (такие `HKWorkoutActivityTypeWheelchairRunPace`как `HKWorkoutActivityTypeFlexibility` и).
- Добавлен новый `HKCDADocument` класс, который представляет документ в формате клинической практике Document Architecture (CDA).
- Используйте новый класс [хкворкаутконфигуратион](https://developer.apple.com/reference/healthkit/hkworkoutconfiguration) , чтобы указать `ActivityType` и `LocationType` для тренировки.
- Для работы с коляска связанными данными о работоспособности были добавлены новые [хквхилчаирусеобжект](https://developer.apple.com/reference/healthkit/hkwheelchairuseobject) и `WheelchairUse` метод класса [хкхеалссторе](https://developer.apple.com/reference/healthkit/hkhealthstore) .

## <a name="homekit-enhancements"></a>Усовершенствования HomeKit

В HomeKit Framework в iOS 10 были внесены следующие улучшения.

- Добавлены новые службы и характеристики.
- IPad можно настроить для работы в качестве центра HomeKit, чтобы предоставить удаленный доступ к аксессуарам, запустить триггеры автоматизации и включить разрешения общего пользователя.
- Добавлена поддержка для дурбелл и камер.
- Для аксессуаров указаны дополнительные контексты и конфигурации.

Дополнительные сведения см. в документации по [HomeKit](~/ios/platform/homekit.md) .

## <a name="metal-enhancements"></a>Усовершенствования в металлах

В среде с iOS 10 были сделаны следующие улучшения:

- Трехмерные приложения и игры теперь могут использовать тесселяцию для эффективного отображения сложных сцен и геометрии с помощью GPU.
- Обеспечение точного контроля за выделением ресурсов для оптимизации производительности металлических приложений с помощью куч ресурсов и целевых объектов визуализации, поддерживающих память.
- Используйте специализацию функции, чтобы создать высокооптимизированный набор функций для комбинирования материалов и освещения для сцены.

Дополнительные сведения см. в статье [по программированию для металлического программирования](https://developer.apple.com/library/prerelease/content/documentation/Miscellaneous/Conceptual/MetalProgrammingGuide/Introduction/Introduction.html#//apple_ref/doc/uid/TP40014221)Apple.

## <a name="modelio-enhancements"></a>Усовершенствования Моделио

В Моделио Framework в iOS 10 были внесены следующие улучшения.

- Теперь поддерживается формат файла USD.
- В класс [мдлвокселаррай](https://developer.apple.com/reference/modelio/mdlvoxelarray) добавлена поддержка поля со знакомого расстояния.
- Используйте новый `MDLLightProbeIrradianceDataSource` класс, чтобы упростить размещение зонда.
- Используйте новый `MDLMaterialPropertyGraph` класс, чтобы легко поддерживать изменения среды выполнения в моделях.

## <a name="photos-enhancements"></a>Усовершенствования фотографий

В платформу Photos в iOS 10 были внесены следующие улучшения.

- Используйте классы [Циимажепроцессоринпут](https://developer.apple.com/reference/coreimage/ciimageprocessorinput) и [Циимажепроцессораутпут](https://developer.apple.com/reference/coreimage/ciimageprocessoroutput) , чтобы воспользоваться преимуществами новой функции процессора основных образов для внесения изменений.
- Редактирование фотографий в реальном времени теперь доступно для приложений, поддерживающих платформу фото, и для расширений редактирования фотографий (для использования в фотографиях и в фотоаппаратных приложениях).
- Используйте новый класс [фливефотоедитингконтекст](https://developer.apple.com/reference/photos/phlivephotoeditingcontext) , чтобы применить изменения как к видео, так и по прежнему в содержимом живых фотографий.

## <a name="replaykit-enhancements"></a>Усовершенствования Реплайкит

В Реплайкит Framework в iOS 10 были внесены следующие улучшения.

- Используйте классы [рпскринрекордер](https://developer.apple.com/reference/replaykit/rpscreenrecorder), [рпброадкастактивитивиевконтроллер](https://developer.apple.com/reference/replaykit/rpbroadcastactivityviewcontroller) и [рпброадкастконтроллер](https://developer.apple.com/reference/replaykit/rpbroadcastcontroller) для поддержки вещания записанного мультимедиа с помощью сторонних сайтов.
- Для поддержки Реплайкитных широковещательных служб сторонних поставщиков в приложении требуется пользовательский интерфейс вещания и расширения Broadcast Upload.

## <a name="scenekit-enhancements"></a>Усовершенствования SceneKit

В SceneKit Framework в iOS 10 были внесены следующие улучшения.

- Класс [скнкамера](xref:SceneKit.SCNCamera) обеспечивает более высокую реальную работу с помощью функций и эффектов HDR. Используйте адаптивную раскрытие, чтобы создать автоматические эффекты или использовать вигнеттинг, регулировку цвета и цветовую раскраску для добавления филлматик эффектов в игру.
- SceneKit теперь включает новую систему физической отрисовки (PBR) для более реалистичных результатов с более простым созданием ресурсов.
- Используйте новую модель заливки [скнлигхтингмоделфисикаллибасед](https://developer.apple.com/reference/scenekit/scnlightingmodelphysicallybased) для произведения широкого спектра реалистичных эффектов заливки, при этом требуются только три фундаментальных `Metalness` свойства `Roughness`(`Diffuse`и).
- Так как заливка типа данных PBR лучше всего работает с освещением на `LightingEnvironment` основе среды, используйте свойство, чтобы назначить освещение на основе изображения для всей сцены.
- `IESProfileURL` Используйте свойство для импорта реальных осветительных источников, определяющих освещение на основе реальных значений, таких как интенсивность (в люменах) и цветовая температура (в градусах Кельвина).
- Функции PBR и HDR-камеры предоставляют лучшие результаты по сравнению с традиционными методами отрисовки, и в результате SceneKit теперь выполняет все цветовые вычисления в линейном цветовом пространстве (при отображении цветовой гаммы P3 на устройствах с широким цветом).
- SceneKit теперь цвет соответствует всем цветам, считывая сведения о цветовых профилях.
- SceneKit интерпретирует значения цветовых компонентов в линейном цветовом пространстве RGB для всех типов шейдера.
- Как линейное, так и расширенное отображение цветового пространства можно отключить, `SCNDisableLinearSpaceRendering` указав `SCNDisableWideGamut` `Info.plist`ключи и в приложении.
- Создайте произвольный многоугольник приматов (загружается из файлов или создается программно), чтобы указать геометрию с новым классом [скнжеометрипримитиветипеполигон](https://developer.apple.com/reference/scenekit/1772322-scenekit_enumerations/scngeometryprimitivetype/scngeometryprimitivetypepolygon) .
- Так как SceneKit считывает и корректирует сведения о цветовых профилях в образах текстур, используйте каталоги активов для всех изображений, чтобы обеспечить их использование.

## <a name="spritekit-enhancements"></a>Усовершенствования SpriteKit

В SpriteKit Framework в iOS 10 были внесены следующие улучшения.

- Пользовательские шейдеры могут предоставлять атрибуты (`SKAttribute`), которые можно настроить отдельно для каждого узла, использующего шейдер, путем предоставления значения атрибута (`SKAttributeValue`).
- Тилемапс теперь поддерживает фигуры мозаичных, шестиугольников и изометрических плиток для двумерных, 2,5 и прокрутых игр с `SKTileMapMode`помощью `SKTileGroup`классов `SKTileGroupRule` , `SKTileSet` и.
- Используйте новый `SKWarpGeometry` класс для растяжения или искажения отрисовки [скспритеноде](xref:SpriteKit.SKSpriteNode) или [скеффектноде](xref:SpriteKit.SKEffectNode) . Новый класс [скактион](xref:SpriteKit.SKAction) можно использовать для анимации переходов между эффектами деформации.
- Класс [сквиев](xref:SpriteKit.SKView) предоставляет несколько новых методов, позволяющих точно контролировать время и способ отрисовки сцены.

## <a name="scrollview-enhancements"></a>Усовершенствования Скроллвиев

В iOS 10,3 были внесены следующие улучшения в элемент управления Скроллвиев:

- `UIScrollView`Теперь включите `IndexDisplayMode` свойство, чтобы управлять отображением индекса, когда пользователь выполняет прокрутку в `UIScrollViewIndexDisplayMode` виде:
    - `Automatic`— Отображение индекса управляется операционной системой.
    - `AlwaysHidden`— Отображение индекса всегда скрыто.

См. [Пример иостенсри](https://docs.microsoft.com/samples/xamarin/ios-samples/ios10-iostenthree) для использования.

## <a name="uikit-enhancements"></a>Усовершенствования UIKit

В UIKit Framework в iOS 10 были внесены следующие улучшения.

- Новый API [уипастебоард](xref:UIKit.UIPasteboard) предоставляет новые параметры (такие как ограничения времени существования) и автоматически объявляет совместимые типы содержимого для общих типов классов.
- Добавлена новая интерактивная поддержка для однообъектной анимации на основе объектов, которую можно связать с жестами. См. сведения в справочнике по [UIViewAnimating Protocol Reference](https://developer.apple.com/reference/uikit/uiviewanimating), [UIViewPropertyAnimator Class Reference](https://developer.apple.com/reference/uikit/uiviewpropertyanimator), [UITimingCurveProvider Protocol Reference](https://developer.apple.com/reference/uikit/uitimingcurveprovider), [UICubicTimingParameters Class Reference](https://developer.apple.com/reference/uikit/uicubictimingparameters) и [UISpringTimingParameter Class Reference](https://developer.apple.com/reference/uikit/uispringtimingparameters).
- Новый `UIPreviewInteraction` и`UIPreviewInteractionDelegate` позволяет приложению-разработчикам предоставлять пользовательский интерфейс для операций просмотра и POP.
- Новый `UIAccessibilityCustomRotor` класс позволяет приложению предоставлять настраиваемые, зависящие от контекста функции для вспомогательных технологий, таких как передача голоса.
- Используйте символы `UIAccessibilityAssistiveTouchStatusDidChangeNotification` и, чтобы определить, включен ли параметр AssistiveTouch. `UIAccessibilityIsAssistiveTouchRunning`
- Используйте символы `UIAccessibilityHearingDevicePairedEarDidChangeNotification` и, чтобы получить состояние любого парного MFiного средства для слуха. `UIAccessibilityHearingDevicePairedEar`
- Для поддержки динамического типа в метках текстовые поля и текстовые поля используют новый `PreferredFontForTextStyle` метод `UIFont` класса.
- Чтобы решить, должен ли элемент обновлять свой шрифт при `UIContentSizeCategory` изменении устройства, `AdjustsFontForContentSizeCategory` используйте свойство `UIContentSizeCategoryAdjusting` делегата.
- `OpenURL` Метод`UIApplication` класса вызывается асинхронно и теперь поддерживает обработчик завершения, который вызывается после завершения действия Open.
- Инициируйте CloudKit общий доступ и измените его свойства с `UICloudSharingController` помощью `UICloudSharingControllerDelegate` новых классов и.
- Воспользуйтесь преимуществами предвыбранных ячеек, чтобы улучшить процесс `UICollectionViews` прокрутки с помощью нового `UICollectionViewDataSourcePrefetching` делегата.
- Теперь разработчик может управлять внешним видом значка для элементов панели вкладок (таких как цвет текста и фона).
- Элемент управления обновлением теперь поддерживается во всех подклассах прокрутки и просмотра с прокруткой (например, `UICollectionView`).

## <a name="webkit-enhancements"></a>Усовершенствования WebKit

В WebKit Framework в iOS 10 были внесены следующие улучшения.

- В `WKWebView` класс добавлена поддержка просмотра и POP. `ShouldPreviewElement` Используйте метод, чтобы определить, должна ли в данном веб-представлении отображаться предварительный просмотр.


## <a name="related-links"></a>Связанные ссылки

- [Примеры iOS 10](https://docs.microsoft.com/samples/browse/?products=xamarin&term=Xamarin.iOS+iOS10)
- [Новые возможности iOS 10](https://developer.apple.com/library/prerelease/content/releasenotes/General/WhatsNewIniOS/Articles/iOS10.html#//apple_ref/doc/uid/TP40017084-SW1)
