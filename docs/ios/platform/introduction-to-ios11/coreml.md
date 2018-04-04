---
title: Общие сведения о CoreML
description: Машинного обучения для мобильных приложений на iOS 11
ms.prod: xamarin
ms.assetid: BE1E2CA1-E3AE-4C90-914C-CFDBD1DCB82B
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 08/30/2016
ms.openlocfilehash: 412a534829349dbbc3f3b76b166882fa6e0e1cd1
ms.sourcegitcommit: 945df041e2180cb20af08b83cc703ecd1aedc6b0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/04/2018
---
# <a name="introduction-to-coreml"></a>Общие сведения о CoreML

_Машинного обучения для мобильных приложений на iOS 11_

CoreML переводит машинного обучения для iOS — приложения могут получить преимущество моделей обученной машинного обучения для выполнения всех видов задач, от решения проблем для распознавания изображений.

В этой вводной статье рассматриваются следующие вопросы:

- [Приступая к работе с CoreML](#coreml)
- [С помощью CoreML с помощью платформы концепции](#coremlvision)

<a name="coreml" />

## <a name="getting-started-with-coreml"></a>Приступая к работе с CoreML

Следующие шаги описывают Добавление CoreML проект iOS. Ссылаться на [Mars Habitat Pricer образец](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) для практического примера.

![Снимок экрана режима MARS Habitat стоимости дома](coreml-images/marspricer-heading.png)

### <a name="1-add-the-model-to-the-project"></a>1. Добавьте в проект модели

Добавить модель скомпилированных (каталог с **.modelc** расширение) для **ресурсов** каталог проекта. Содержимое каталога всегда должны иметь действие построения **BundleResource**:

![В папке ресурсы должны находиться скомпилированных модели](coreml-images/resources-modelc.png)

[Образцы](https://developer.xamarin.com/samples/monotouch/ios11/) использовать модели, скомпилированные в Xcode 9 или вручную с помощью следующей команды терминалов:

```csharp
xcrun coremlcompiler compile {model.mlmodel} {outputFolder}
```

> [!NOTE]
> **.model** файлы _должен_ будет скомпилирована в **.modelc** прежде чем их можно использовать с CoreML

### <a name="2-load-the-model"></a>2. Загрузить модель

Прежде чем использовать модель, загрузите ее с помощью `MLModel.FromUrl` статического метода:

```csharp
var assetPath = NSBundle.MainBundle.GetUrlForResource("NameOfModel", "mlmodelc");
model = MLModel.FromUrl(assetPath, out NSError error1);
```

### <a name="3-set-the-parameters"></a>3. Задайте параметры

Параметры модели передаются и выхода из системы с использованием класса контейнера, который реализует `IMLFeatureProvider`.

Классы поставщиков для функции ведут себя как словарь строки и `MLFeatureValue`s, где каждое значение функция может быть простая строка или число, массив или данные или буфер пикселя, содержащего изображение.

Ниже приведен исходный код для однозначных компонент поставщика:

```csharp
public class MyInput : NSObject, IMLFeatureProvider
{
  public double MyParam { get; set; }
  public NSSet<NSString> FeatureNames => new NSSet<NSString>(new NSString("myParam"));
  public MLFeatureValue GetFeatureValue(string featureName)
  {
    if (featureName == "myParam")
      return MLFeatureValue.FromDouble(MyParam);
    return MLFeatureValue.FromDouble(0); // default value
  }
```

С помощью классов следующим образом, входные параметры могут быть предоставлены в виде, воспринимаемое CoreML. Имена функций (таких как `myParam` в примере кода) должны соответствовать ожидаемому модели.

### <a name="4-run-the-model"></a>4. Запустите модели

С помощью модели необходимо, чтобы поставщик функций для создания экземпляра и параметры, затем, `GetPrediction` вызвать метод:

```csharp
var input = new MyInput {MyParam = 13};
var outFeatures = model.GetPrediction(inputFeatures, out NSError error2);
```

### <a name="5-extract-the-results"></a>5. Извлечение результатов

Результат прогноза `outFeatures` также является экземпляром класса `IMLFeatureProvider`; выход значения могут быть доступны с помощью `GetFeatureValue` с именем каждого выходного параметра (например, `theResult`), как показано в этом примере:

```csharp
var result = outFeatures.GetFeatureValue("theResult").DoubleValue; // eg. 6227020800
```

<a name="coremlvision" />

## <a name="using-coreml-with-the-vision-framework"></a>С помощью CoreML с помощью платформы концепции

CoreML может также использоваться совместно с платформой концепции для выполнения операций над изображения, например распознавание фигур, идентификатор объекта и других задач.

Перечисленные ниже шаги описывают, как CoreML и концепции используются совместно в [CoreMLVision пример](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/). В образце объединены [распознавания прямоугольники](~/ios/platform/introduction-to-ios11/vision.md#rectangles) из framework концепции с _MNINSTClassifier_ CoreML модель для определения рукописные цифрой в фотографии.

![Распознавание изображений числа 3](coreml-images/vision3.png) ![Распознавание изображений числа 5](coreml-images/vision5.png)

### <a name="1-create-a-vision-coreml-model"></a>1. Создать модель CoreML концепции

Модель CoreML _MNISTClassifier_ загружается и затем помещается в `VNCoreMLModel` которого делает модели доступными для Vision задачи. Этот код также создает два запроса концепции: сначала для нахождения прямоугольники, изображения, а затем для обработки прямоугольник с моделью CoreML:

```csharp
// Load the ML model
var assetPath = NSBundle.MainBundle.GetUrlForResource("MNISTClassifier", "mlmodelc");
var mlModel = MLModel.FromUrl(assetPath, out NSError mlErr);
var vModel = VNCoreMLModel.FromMLModel(mlModel, out NSError vnErr);

// Initialize Vision requests
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
ClassificationRequest = new VNCoreMLRequest(vModel, HandleClassification);
```

По-прежнему должен реализовать класс `HandleRectangles` и `HandleClassification` методы для запросов концепции, показанный в шагах 3 и 4 ниже.

### <a name="2-start-the-vision-processing"></a>2. Начало обработки концепции

Следующий код запускает обработку запроса. В **CoreMLVision** примера, этот код выполняется после пользователь выбрал изображения:

```csharp
// Run the rectangle detector, which upon completion runs the ML classifier.
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Этот обработчик передает `ciImage` для платформы концепции `VNDetectRectanglesRequest` , созданного на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. Обрабатывать результаты обработки концепции

После завершения обнаружения прямоугольник выполняет `HandleRectangles` метод, который обрезает изображение для извлечения первого прямоугольника, преобразует в оттенки серого прямоугольника изображения и передает их CoreML модели классификации.

`request` Параметр, переданный в этот метод содержит сведения о запросе концепции и с помощью `GetResults<VNRectangleObservation>()` метод, то возвращается список прямоугольников, найденный в образе. Первый прямоугольник `observations[0]` извлечены и передается в модель CoreML:

```csharp
void HandleRectangles(VNRequest request, NSError error) {
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  var detectedRectangle = observations[0]; // first rectangle
  // ... omitted cropping and greyscale conversion ...
  // Run the Core ML MNIST classifier -- results in handleClassification method
  var handler = new VNImageRequestHandler(correctedImage, new VNImageOptions());
  DispatchQueue.DefaultGlobalQueue.DispatchAsync(() => {
    handler.Perform(new VNRequest[] { ClassificationRequest }, out NSError err);
  });
}
```

`ClassificationRequest` Была инициализирована в шаге 1, чтобы использовать `HandleClassification` метода, определенного в следующем шаге.

### <a name="4-handle-the-coreml"></a>4. Дескриптор CoreML

`request` Параметр, переданный в этот метод содержит сведения о запросе CoreML и с помощью `GetResults<VNClassificationObservation>()` метод, он возвращает список возможных результатов, упорядоченные по достоверности (наивысший достоверности первой):

```csharp
void HandleClassification(VNRequest request, NSError error){
  var observations = request.GetResults<VNClassificationObservation>();
  ... omitted error handling ...
  var best = observations[0]; // first/best classification result
  // render in UI
  DispatchQueue.MainQueue.DispatchAsync(()=>{
    ClassificationLabel.Text = $"Classification: {best.Identifier} Confidence: {best.Confidence * 100f:#.00}%";
  });
}
```



## <a name="samples"></a>Примеры

Существует три примера CoreML попробовать:

* [Mars Habitat стоимости дома образец](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/) содержит простые числовые входы и выходы.

* [Образец концепции & CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/) принимает параметр изображений и используется для идентификации квадратный областей в образе, передаваемыми CoreML модель, которая распознает одиночными цифрами концепции.

* Наконец [пример распознавания изображений CoreML](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/) использует CoreML для определения возможности фото. По умолчанию использует меньшее **SqueezeNet** модель (5 МБ), но оно было записано, можно загрузить и использовать больший **VGG16** модели (553 МБ). Дополнительные сведения см. в разделе [файл readme для образца](https://github.com/xamarin/ios-samples/blob/master/ios11/CoreMLImageRecognition/CoreMLImageRecognition/README.md).


## <a name="related-links"></a>Связанные ссылки

- [Машинное обучение (Apple)](https://developer.apple.com/machine-learning/)
- [Пример CoreML (режим Mars Habitat) (пример)](https://developer.xamarin.com/samples/monotouch/ios11/CoreML/)
- [CoreML и концепции (номер распознавания) (пример)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLVision/)
- [Распознавание изображений CoreML (пример)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLImageRecognition/)
- [CoreML с концепции настраиваемый Azure (пример)](https://developer.xamarin.com/samples/monotouch/ios11/CoreMLAzureModel)
- [Знакомство с приложением CoreML (WWDC) (видео)](https://developer.apple.com/videos/play/wwdc2017/703/)
