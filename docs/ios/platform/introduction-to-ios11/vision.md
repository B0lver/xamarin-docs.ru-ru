---
title: Инфраструктура концепции в Xamarin. iOS
description: В этом документе описывается, как использовать платформу для работы с iOS 11 в Xamarin. iOS. В частности, он описывает обнаружение прямоугольников и обнаружение лиц.
ms.prod: xamarin
ms.assetid: 7273ED68-7B7D-4252-B3A0-02DB2E357A8C
ms.technology: xamarin-ios
author: davidortinau
ms.author: daortin
ms.date: 08/31/2017
ms.openlocfilehash: b58e7b1fffed3253d9765401d52f16b751db134d
ms.sourcegitcommit: eca3b01098dba004d367292c8b0d74b58c4e1206
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/13/2020
ms.locfileid: "79306107"
---
# <a name="vision-framework-in-xamarinios"></a>Инфраструктура концепции в Xamarin. iOS

Инфраструктура концепции добавляет ряд новых функций обработки изображений в iOS 11, включая:

- [Определение прямоугольника](#rectangles)
- [обнаружение лиц](#faces)
- Анализ изображений Машинное обучение (рассматривается в [CoreML](~/ios/platform/introduction-to-ios11/coreml.md))
- Определение штрихкода
- Анализ выравнивания изображений
- Обнаружение текста
- Обнаружение горизонта
- Отслеживание & обнаружения объектов

![Фотография с обнаруженными тремя прямоугольниками](vision-images/found-rectangles-tiny.png) ![Обнаружена фотография с двумя лицами](vision-images/xamarin-home-faces-tiny.png)

Обнаружение и обнаружение лиц прямоугольников рассматриваются более подробно ниже.

<a name="rectangles" />

## <a name="rectangle-detection"></a>Определение прямоугольника

В [примере висионректс](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles) показано, как обработать изображение и нарисовать обнаруженные на нем прямоугольники.

### <a name="1-initialize-the-vision-request"></a>1. Инициализация запроса концепции

В `ViewDidLoad`создайте `VNDetectRectanglesRequest`, который ссылается на метод `HandleRectangles`, который будет вызываться в конце каждого запроса:

Необходимо также задать свойство `MaximumObservations`, в противном случае оно будет по умолчанию иметь значение 1, и будет возвращен только один результат.

```csharp
RectangleRequest = new VNDetectRectanglesRequest(HandleRectangles);
RectangleRequest.MaximumObservations = 10;
```

### <a name="2-start-the-vision-processing"></a>2. Начните обработку концепции

Следующий код начинает обработку запроса. В примере **висионректс** этот код выполняется после выбора пользователем изображения:

```csharp
// Run the rectangle detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {RectangleRequest}, out NSError error);
});
```

Этот обработчик передает `ciImage` инфраструктуре концепции `VNDetectRectanglesRequest`, созданной на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. обработка результатов обработки концепции

После завершения обнаружения прямоугольника платформа выполняет метод `HandleRectangles`, а также сводку, показанную ниже.

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNRectangleObservation>();
  // ... omitted error handling ...
  bool atLeastOneValid = false;
  foreach (var o in observations){
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  if (!atLeastOneValid) return;
  // Show the pre-processed image
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Отображение результатов

Метод `OverlayRectangles` в образце **висионректанглес** имеет три функции:

- Подготовка к просмотру исходного изображения,
- Рисование прямоугольника для указания места обнаружения каждого из них и
- Добавление текстовой метки для каждого прямоугольника с помощью Кореграфикс.

Просмотрите [Источник образца](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles) для точного метода кореграфикс.

![Фотография с обнаруженными тремя прямоугольниками](vision-images/found-rectangles-phone-sml.png)

### <a name="5-further-processing"></a>5. Дальнейшая обработка

Обнаружение прямоугольника часто является просто первым шагом в цепочке операций, например в [этом коремлвисион примере](~/ios/platform/introduction-to-ios11/coreml.md#coremlvision), где прямоугольники передаются в модель CoreML для анализа рукописных цифр.

<a name="faces" />

## <a name="face-detection"></a>Обнаружение лиц

[Пример висионфацес](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces) работает аналогично образцу **висионректанглес** , используя другой класс запроса концепции.

### <a name="1-initialize-the-vision-request"></a>1. Инициализация запроса концепции

В `ViewDidLoad`создайте `VNDetectFaceRectanglesRequest`, который ссылается на метод `HandleRectangles`, который будет вызываться в конце каждого запроса.

```csharp
FaceRectangleRequest = new VNDetectFaceRectanglesRequest(HandleRectangles);
```

### <a name="2-start-the-vision-processing"></a>2. Начните обработку концепции

Следующий код начинает обработку запроса. В примере **висионфацес** этот код выполняется после выбора пользователем изображения:

```csharp
// Run the face detector
var handler = new VNImageRequestHandler(ciImage, uiImage.Orientation.ToCGImagePropertyOrientation(), new VNImageOptions());
DispatchQueue.DefaultGlobalQueue.DispatchAsync(()=>{
  handler.Perform(new VNRequest[] {FaceRectangleRequest}, out NSError error);
});
```

Этот обработчик передает `ciImage` инфраструктуре концепции `VNDetectFaceRectanglesRequest`, созданной на шаге 1.

### <a name="3-handle-the-results-of-vision-processing"></a>3. обработка результатов обработки концепции

После завершения обнаружения распознавания лиц обработчик выполняет `HandleRectangles` метод, который выполняет обработку ошибок и отображает границы обнаруженных фрагментов и вызывает `OverlayRectangles` для рисования ограничивающих прямоугольников на исходном изображении:

```csharp
private void HandleRectangles(VNRequest request, NSError error){
  var observations = request.GetResults<VNFaceObservation>();
  // ... omitted error handling...
  var summary = "";
  var imageSize = InputImage.Extent.Size;
  bool atLeastOneValid = false;
  Console.WriteLine("Faces:");
  summary += "Faces:";
  foreach (var o in observations) {
    // Verify detected rectangle is valid. omitted
    var boundingBox = o.BoundingBox.Scaled(imageSize);
    if (InputImage.Extent.Contains(boundingBox)) {
      atLeastOneValid |= true;
    }
  }
  // Show the pre-processed image (on UI thread)
  DispatchQueue.MainQueue.DispatchAsync(() =>
  {
    ClassificationLabel.Text = summary;
    ImageView.Image = OverlayRectangles(RawImage, imageSize, observations);
  });
}
```

### <a name="4-display-the-results"></a>4. Отображение результатов

Метод `OverlayRectangles` в образце **висионфацес** имеет три функции:

- Подготовка к просмотру исходного изображения,
- Рисование прямоугольника для каждого обнаруженного лица и
- Добавление текстовой метки для каждой грани с помощью Кореграфикс.

Просмотрите [Источник образца](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces) для точного метода кореграфикс.

![Обнаружена фотография с двумя лицами](vision-images/found-faces-phone-sml.png)

### <a name="5-further-processing"></a>5. Дальнейшая обработка

Инфраструктура концепции включает дополнительные возможности для определения возможностей лиц, таких как глаза и рот. Используйте тип `VNDetectFaceLandmarksRequest`, который будет возвращать `VNFaceObservation` результаты, как на шаге 3 выше, но с дополнительными данными `VNFaceLandmark`.

## <a name="related-links"></a>Связанные ссылки

- [Прямоугольники концепции (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionrectangles)
- [Лица с видением (пример)](https://docs.microsoft.com/samples/xamarin/ios-samples/ios11-visionfaces)
- [Усовершенствования в основном образе — фильтры, металл, концепция и многое другое (ВВДК) (видео)](https://developer.apple.com/videos/play/wwdc2017/510/)
