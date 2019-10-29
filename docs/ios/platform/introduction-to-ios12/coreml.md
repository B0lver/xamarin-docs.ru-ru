---
title: Ядро машинного обучения 2 в Xamarin. iOS
description: В этом документе описаны обновления ядра машинного обучения, доступные в составе iOS 12. В частности, он просматривает улучшения производительности, связанные с новым API-интерфейсом прогнозирования пакетов.
ms.prod: xamarin
ms.assetid: 408E752C-2C78-4B20-8B43-A6B89B7E6D1B
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/15/2018
ms.openlocfilehash: 6245873385caa23e37d5499daa822fa0b699ac1e
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73032025"
---
# <a name="core-ml-2-in-xamarinios"></a>Ядро машинного обучения 2 в Xamarin. iOS

Core ML — это технология машинного обучения, доступная в iOS, macOS, tvOS и watchOS. Это позволяет приложениям создавать прогнозы на основе моделей машинного обучения.

В iOS 12 Core ML включает API пакетной обработки. Этот API делает основной МАШИНный механизм более эффективным и обеспечивает улучшение производительности в сценариях, где модель используется для создания последовательности прогнозов.

## <a name="sample-app-marshabitatcoremltimer"></a>Пример приложения: Маршабитаткоремлтимер

Чтобы продемонстрировать пакетные прогнозы с помощью Core ML, ознакомьтесь с примером приложения [маршабитаткоремлтимер](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer) . В этом примере используется основная модель машинного обучения, обученная для прогнозирования затрат на создание Habitat в режиме MARS на основе различных входных элементов: число солнечных панелей, число гринхаусес и число Акрес.

Фрагменты кода в этом документе взяты из этого примера.

## <a name="generate-sample-data"></a>Создание демонстрационных данных

В `ViewController`метод `ViewDidLoad` примера приложения вызывает `LoadMLModel`, который загружает в себя базовую модель машинного обучения:

```csharp
void LoadMLModel()
{
    var assetPath = NSBundle.MainBundle.GetUrlForResource("CoreMLModel/MarsHabitatPricer", "mlmodelc");
    model = MLModel.Create(assetPath, out NSError mlErr);
}
```

Затем пример приложения создает 100 000 `MarsHabitatPricerInput` объектов для использования в качестве входных данных для последовательных прогнозов машинного обучения ML. Каждый созданный пример имеет случайное значение, заданное для числа солнечных панелей, числа гринхаусес и числа Акрес:

```csharp
async void CreateInputs(int num)
{
    // ...
    Random r = new Random();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            double solarPanels = r.NextDouble() * MaxSolarPanels;
            double greenHouses = r.NextDouble() * MaxGreenHouses;
            double acres = r.NextDouble() * MaxAcres;
            inputs[i] = new MarsHabitatPricerInput(solarPanels, greenHouses, acres);
        }
    });
    // ...
}
```

При нажатии любой из трех кнопок приложения выполняются две последовательности прогнозов: один с помощью цикла `for`, а другой — с помощью нового метода пакетной `GetPredictions`, представленного в iOS 12:

```csharp
async void RunTest(int num)
{
    // ...
    await FetchNonBatchResults(num);
    // ...
    await FetchBatchResults(num);
    // ...
}
```

## <a name="for-loop"></a>for - цикл

`for`ная версия цикла теста по алгоритму Байеса выполняет итерацию указанного числа входов, вызывая [`GetPrediction`](xref:CoreML.MLModel.GetPrediction*) для каждого из них и удаляя результат. Метод показывает, сколько времени требуется для выполнения прогнозов:

```csharp
async Task FetchNonBatchResults(int num)
{
    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        for (int i = 0; i < num; i++)
        {
            IMLFeatureProvider output = model.GetPrediction(inputs[i], out NSError error);
        }
    });
    stopWatch.Stop();
    nonBatchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="getpredictions-new-batch-api"></a>Прогнозирование (новый API пакетной службы)

Пакетная версия теста создает объект `MLArrayBatchProvider` из входного массива (поскольку этот входной параметр является обязательным для метода `GetPredictions`), создает [`MLPredictionOptions`](xref:CoreML.MLPredictionOptions)
Объект, который не позволяет вычислениям прогноза быть ограничен ЦП и использует `GetPredictions` API для выборки прогнозов и повторно отклоняет результат:

```csharp
async Task FetchBatchResults(int num)
{
    var batch = new MLArrayBatchProvider(inputs.Take(num).ToArray());
    var options = new MLPredictionOptions()
    {
        UsesCpuOnly = false
    };

    Stopwatch stopWatch = Stopwatch.StartNew();
    await Task.Run(() =>
    {
        model.GetPredictions(batch, options, out NSError error);
    });
    stopWatch.Stop();
    batchMilliseconds = stopWatch.ElapsedMilliseconds;
}
```

## <a name="results"></a>Результаты

Как на симуляторе, так и на устройстве, `GetPredictions` завершается быстрее, чем основные прогнозы машинного обучения на основе цикла.

## <a name="related-links"></a>Связанные ссылки

- [Пример приложения — Маршабитаткоремлтимер](https://docs.microsoft.com/samples/xamarin/ios-samples/ios12-marshabitatcoremltimer)
- [Новые возможности в Core ML, часть 1 (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/708/)
- [Новые возможности в Core ML, часть 2 (ВВДК 2018)](https://developer.apple.com/videos/play/wwdc2018/709/)
- [Общие сведения о ядре ML в Xamarin. iOS](https://docs.microsoft.com/xamarin/ios/platform/introduction-to-ios11/coreml)
- [Core ML (Apple)](https://developer.apple.com/documentation/coreml?language=objc)
- [Работа с моделями ядра машинного обучения](https://developer.apple.com/machine-learning/build-run-models/)
