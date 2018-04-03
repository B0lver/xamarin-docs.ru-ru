---
title: Сведения о BouncingGame
description: В этом пошаговом руководстве показано, как реализовать простой прыгающего игры шара с использованием CocosSharp.
ms.topic: article
ms.prod: xamarin
ms.assetid: AC9FD56F-6E4A-40DA-8168-45A761D869FD
ms.technology: xamarin-cross-platform
author: charlespetzold
ms.author: chape
ms.date: 03/29/2018
ms.openlocfilehash: fdf5d28637de839b63f4c7c71c7f9413daeb8c29
ms.sourcegitcommit: 4f1b508caa8e7b6ccf85d167ea700a5d28b0347e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/03/2018
---
# <a name="bouncinggame-details"></a>Сведения о BouncingGame

> [!TIP]
> Код BouncingGame можно найти в [Xamarin samples](https://developer.xamarin.com/samples/mobile/BouncingGame/). Лучше всего далее в этом руководстве, загрузите и изучите код, как руководство, ссылающейся на него.

В этом пошаговом руководстве показано, как реализовать простой прыгающего игры шара с использованием CocosSharp. 

 - Распаковка содержимого игр
 - Общие элементы visual CocosSharp
 - Визуальные элементы для добавления `GameLayer`
 - Реализация логики каждые кадра

Наш завершении игры будет выглядеть следующим образом:

![BouncingGame, завершения](bouncing-game-images/image1.png "BouncingGame, завершено")

## <a name="unzipping-game-content"></a>Распаковка содержимого игр

Игры разработчики часто используют термин *содержимого* для ссылки на файлы без кода, которые обычно создаются средой visual исполнители, игры конструкторы или конструкторы аудио. Общие типы содержимого включают файлы, используемые для отображения визуальных элементов, воспроизведение звука или управлять поведением искусственный интеллект (AI). С точки зрения группой разработки игр содержимого обычно создается путем не программистов. Наш игры включает два типа содержимого:

 - PNG-файлы можно определить внешний вид шар и ракетка.
 - Один xnb-файл для определения шрифт отображения оценку (более подробно рассмотрены при мы добавляем отображение оценка ниже)

Это содержимое, используемое здесь можно найти в [содержимого zip](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true). Мы будем нужны эти файлы загружаются и распаковываются в расположении, которое мы будет обращаться к далее в этом пошаговом руководстве.

## <a name="common-cocossharp-visual-elements"></a>Общие элементы visual CocosSharp

CocosSharp предоставляет несколько классов, используемых для отображения визуальных элементов. Некоторые элементы напрямую видны, то время как другие используются для организации. В игре, мы будем использовать следующие:

 - `CCNode` — Базовый класс для всех визуальных объектов в CocosSharp. `CCNode` Класс содержит `AddChild` функции, который может использоваться для построения иерархии родители потомки также называют *визуального дерева* или *граф сценой*. Все классы, упомянутые ниже наследуются от `CCNode`.
 - `CCScene` — Корневой визуального дерева для всех CocosSharp игр. Все визуальные элементы должны быть частью визуального дерева с `CCScene` корневого элемента, или они не будут видны.
 - `CCLayer` — Контейнер для визуального объекты, такие как `CCSprite`. Как следует из имен, `CCLayer` класс используется для управления каким образом визуальные элементы слоя поверх друг с другом.
 - `CCSprite` Отображает изображение или часть изображения. `CCSprite` экземпляры могут располагаться, размер и предоставляют несколько визуальных эффектов.
 - `CCLabel` Отображает строку на экране. Шрифт, используемый `CCLabel` определяется в xnb-файл. Мы обсудим xnbs ниже более подробно.

Сведения об использовании различных типов мы обсудим один `CCSprite`. Необходимо добавить визуальные элементы `CCLayer`, и должен иметь визуальное дерево `CCScene` в качестве корня. Таким образом, иерархическое отношение одного и того же `CCSprite` бы `CCScene`  >  `CCLayer`  >  `CCSprite`.

## <a name="adding-visual-elements-to-gamelayer"></a>Добавление GameLayer визуальных элементов

### <a name="adding-the-paddle-sprite"></a>Добавление sprite ракетка

Сначала мы настроим игры в фоновом режиме для черный, а также добавить один `CCSprite` подготовки к просмотру на экране. Изменить `GameLayer` добавляемый класс `CCSprite` следующим образом:

```csharp
using System;
using System.Collections.Generic;
using CocosSharp;
namespace BouncingGame
{
    public class GameLayer : CCLayer
    {
        CCSprite paddleSprite;
        public GameLayer () : base(CCColor4B.Black)
        {
            // "paddle" refers to the paddle.png image
            paddleSprite = new CCSprite ("paddle");
            paddleSprite.PositionX = 100;
            paddleSprite.PositionY = 100;
            AddChild (paddleSprite);
        }
        protected override void AddedToScene ()
        {
            base.AddedToScene ();
            // Use the bounds to layout the positioning of the drawable assets
            CCRect bounds = VisibleBoundsWorldspace;
            // Register for touch events
            var touchListener = new CCEventListenerTouchAllAtOnce ();
            touchListener.OnTouchesEnded = OnTouchesEnded;
            AddEventListener (touchListener, this);
        }
        void OnTouchesEnded (List<CCTouch> touches, CCEvent touchEvent)
        {
            if (touches.Count > 0)
            {
                // Perform touch handling here
            }
        }
    }
}
```

Приведенный выше код создает один `CCSprite` и добавляет его в качестве дочернего элемента `GameLayer`. `CCSprite` Конструктор позволяет определить файл изображения для использования в качестве строки. Наш код предписывает CocosSharp искать файл с именем `paddle` (расширение указано, которой будут обсуждаться далее в этом руководстве). CocosSharp будет искать любые имена файлов `paddle` в корневой папке содержимого (который является **содержимого**) и все папки, добавленные к `gameView.ContentManager.SearchPaths` (как описано в предыдущем разделе).

Мы добавим файлы непосредственно в корне **содержимого** папки для iOS и Android. Чтобы сделать это, щелкните правой кнопкой мыши или щелкните элемент управления на **содержимого** папки в проекте iOS и выберите **добавить** > **добавить файлы...** Когда мы распаковал содержимое, ранее перейдите и выберите **paddle.png**. Если будет задан вопрос о том, как добавить файл в папку, следует выбрать **копирования** параметр:

![Добавить файл в папку диалоговое окно](bouncing-game-images/image2.png "добавьте файл в папку диалоговое окно")

Далее мы добавим файл проекта для Android. Щелкните правой кнопкой мыши или нажмите клавишу Control на эту папку контента (который находится в **активы** папки для проектов Android) и выберите команду select **добавить** > **добавить файлы...** . На этот раз перейдите к проекту iOS **содержимого** папки. На вопрос о том, как добавить файл, выберите **добавить ссылку** параметр:

![Добавить файл в папку диалоговое окно](bouncing-game-images/addalink.png "добавить файл в папку диалоговое окно")

Мы рассмотрим, почему файлы требовалось добавлять для обоих проектов ниже. Каждый проект **содержимого** папки теперь содержат **paddle.png** файла:

![Содержимое папки содержимого](bouncing-game-images/image3.png "содержимое содержимого папки")

Если мы запустим игра будет отображен `CCSprite` отрисовывается:

![Ракетка нарисованы на экране](bouncing-game-images/image4.png "ракетка нарисованы на экране")

### <a name="file-details"></a>Сведения о файле

Пока мы добавили один файл проекта, и процесс был довольно прост. Мы просто добавляется **paddle.png** файл **содержимого** папки со ссылкой на него в коде. Давайте рассмотрим некоторые соображения при работе с файлами в CocosSharp некоторое время.

#### <a name="capitalization"></a>Регистр букв

Обратите внимание, что имя файла и строка используется в коде для доступа к файлу и в нижнем регистре. Это вызвано тем, некоторые платформы (например, симулятор Windows desktop и iOS) не зависят от регистра, пока других платформ (таких как устройства iOS и Android) чувствительны к регистру. Мы будем использовать все файлы в нижнем регистре для оставшейся части этого учебника, чтобы файлы и код до тех пор кросс платформенных насколько это возможно.

#### <a name="extensions"></a>Расширения

При обращении к файлу ракетка конструктор для создания спрайта не включает расширением «PNG». Расширение файлов, обычно пропущено при Организуя работу над проектами CocosSharp как расширения файла того же типа активов могут отличаться от платформы. Например звуковые файлы может быть .aiff, MP3 или .wma форматов файлов, в зависимости от платформы. Если оставить отключение расширения позволит один и тот же код работать на всех платформах, независимо от расширения файла.

#### <a name="content-in-platform-specific-projects"></a>Содержимое в проекты под конкретные платформы

В отличие от большинства файлов кода, которые могут находиться в PCL, содержимого, необходимо добавить файлы для каждого проекта под конкретную платформу. CocosSharp требуется по двум причинам:

1. Каждая платформа имеет различные **построения действий**. Следует использовать содержимое, добавленное в проекты iOS **BundleResource** действие построения. Следует использовать содержимое, добавленное в проектов Android **AndroidAsset** действие построения.
2. Некоторые платформы требуют форматы файлов конкретную платформу. Например файлы шрифтов .xnb различаются iOS и Android, как будет показано далее в этом пошаговом руководстве.

Если формат файла не отличается от платформы (например, .png), каждая платформа можно использовать тот же файл, где один или несколько платформ могут сопоставить файл из одного места.

## <a name="orientation"></a>Ориентация

Так же, как любое другое приложение CocosSharp приложения можно запускать в книжной и альбомной ориентации. Мы будем настроим игры для запуска в режиме книжной ориентации. Во-первых мы изменим код решения в игре для обработки пропорции книжной ориентации. Чтобы сделать это, измените `width` и `height` значения в `LoadGame` метод в `ViewController` класса в iOS и `MainActivity` класса на Android:

```csharp
void LoadGame (object sender, EventArgs e)
{
    CCGameView gameView = sender as CCGameView;

    if (gameView != null)
    {
        var contentSearchPaths = new List<string> () { "Fonts", "Sounds" };
        CCSizeI viewSize = gameView.ViewSize;

        int width = 768;
        int height = 1027;
    // ...
```

Далее нам нужно будет изменить каждого проекта под конкретную платформу для работы в режиме книжной ориентации.

### <a name="ios-orientation"></a>Ориентация iOS

Чтобы изменить ориентацию проект iOS, выберите **Info.plist** файла в **BouncingGame.iOS** проекта, а также изменить **iPhone/iPod данных развертывания** и **iPad данных развертывания** включают только книжной ориентации:

![Параметры ориентации](bouncing-game-images/image5.png "параметры ориентации")

### <a name="android-orientation"></a>Android ориентации

Чтобы изменить ориентацию проекта Android, откройте файл MainActivity.cs в проекте BouncingGame.Android. Измените определение атрибута действия, поэтому указывает только в книжной ориентации следующим образом:

```csharp
[Activity (
    Label = "BouncingGame.Android",
    AlwaysRetainTaskState = true,
    Icon = "@drawable/icon",
    Theme = "@android:style/Theme.NoTitleBar",
    ScreenOrientation = ScreenOrientation.Portrait,
    LaunchMode = LaunchMode.SingleInstance,
    MainLauncher = true,
    ConfigurationChanges = ConfigChanges.Orientation | ConfigChanges.ScreenSize | ConfigChanges.Keyboard | ConfigChanges.KeyboardHidden)
]
public class MainActivity : Activity
{ 
...
```

## <a name="default-coordinate-system"></a>Система координат по умолчанию

Наш код, который создает экземпляр `CCSprite`, задает `PositionX` и `PositionY` значения до 100. По умолчанию, это означает, что `CCSprite` будет находиться на 100 пикселей центра вверх и вправо относительно левой нижней части экрана. Система координат можно изменить путем присоединения `CCCamera` для `CCLayer`. Мы не будет работать с `CCCamera` в этот проект, но Дополнительные сведения о `CCCamera` можно найти в [CocosSharp API docs](https://developer.xamarin.com/api/type/CocosSharp.CCLayer/).

100 пикселей, упомянутых выше «игры» пикселей, в отличие от пикселей на оборудовании. Это означает, что выполнение же игры на устройстве другое разрешение (таких как iPad и iPhone) приведет к объекты положения и размера правильно относительно физического экрана. В частности, игры в видимой области всегда будут 1024 пикселей в высоту и 768 пикселей в ширину, как это разрешение мы указали ранее в `LoadGame` метод.

## <a name="adding-the-ball-sprite"></a>Добавление sprite шар

Теперь, когда мы уже знакомы с основами работы с `CCSprite`, мы добавим второй `CCSprite` — шар. Мы будем следующие шаги, которые очень похожи на том, как мы создали ракетка `CCSprite`. 

Во-первых, мы добавим **ball.png** образ из распакованную папку в проекте iOS **содержимого** папки. Выберите, чтобы **копирования** файл **содержимого** каталога. Выполните те же действия, как описано выше, чтобы добавить ссылку на **ball.png** файл в проект Android.

Далее создайте `CCSprite` этот шарик, добавив элемент с именем `ballSprite` для `GameScene` класса, а также код создания экземпляра для `ballSprite`. После завершения `GameLayer` класса будет выглядеть следующим образом:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
    }
```

## <a name="adding-the-score-label"></a>Добавление метки оценка

Последний элемент visual, мы добавим в игру `CCLabel` для отображения, сколько раз пользователь успешно отклонено шар. `CCLabel` Используется шрифт, указанный в конструкторе для отображения строк на экране.

Добавьте следующий код для создания `CCLabel` экземпляра в `GameLayer`. После завершения работы, код должен выглядеть следующим образом:

```csharp
public class GameLayer : CCLayer
{
    CCSprite paddleSprite;
    CCSprite ballSprite;
    CCLabel scoreLabel;
    public GameLayer () : base (CCColor4B.Black)
    {
        // "paddle" refers to the paddle.png image
        paddleSprite = new CCSprite ("paddle");
        paddleSprite.PositionX = 100;
        paddleSprite.PositionY = 100;
        AddChild (paddleSprite);
        ballSprite = new CCSprite ("ball");
        ballSprite.PositionX = 320;
        ballSprite.PositionY = 600;
        AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "Arial", 20, CCLabelFormat.SystemFont);
        scoreLabel.PositionX = 50;
        scoreLabel.PositionY = 1000;
        scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
        AddChild (scoreLabel);
    }
    // ...
```

Обратите внимание, что использование scoreLabel `AnchorPoint` из `CCPoint.AnchorUpperLeft`, означающее, что `PositionX` и `PositionY` значения определить его положение верхнего левого. Это позволит нам позиции `scoreLabel` относительно верхней левой части экрана без необходимости учитывать метки измерения.

## <a name="implementing-every-frame-logic"></a>Реализация логики каждые кадра

Таким образом игра представляет статический сцены. Мы будем добавлять логику для управления движением объектов в сцене путем добавления кода, который обновляет положение объектов с высокой частотой. В этом случае код будет выполняться шестьдесят раз в секунду - также называют 60 *кадров* в секунду (Если оборудование не может обрабатывать редкого обновления). В частности мы будем добавлять логики совершения шарик попадает и возврата к ракетка, чтобы переместить ракетка согласно входных данных и обновление проигрывателя оценка каждый раз шарик сохранилась ракетка.

`Schedule` Метод, предоставляемый `CCNode` класса, позволяет добавить логику каждые кадра игры. Мы добавим код после `// New code` GameLayer конструктор:

```csharp
public GameLayer () : base (CCColor4B.Black)
{
    // "paddle" refers to the paddle.png image
    paddleSprite = new CCSprite ("paddle");
    paddleSprite.PositionX = 100;
    paddleSprite.PositionY = 100;
    AddChild (paddleSprite);
    ballSprite = new CCSprite ("ball");
    ballSprite.PositionX = 320;
    ballSprite.PositionY = 600;
    AddChild (ballSprite);
        scoreLabel = new CCLabel ("Score: 0", "arial", 22, CCLabelFormat.SpriteFont);
    scoreLabel.PositionX = 50;
    scoreLabel.PositionY = 1000;
    scoreLabel.AnchorPoint = CCPoint.AnchorUpperLeft;
    AddChild (scoreLabel);
    // New code:
    Schedule (RunGameLogic);
}
```

Теперь создайте `RunGameLogic` метод в `GameLayer` класс, который размещается вся логика каждые кадра:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
}
```

Параметр float определяет, как долго кадр задается в секундах. Мы будем использовать это значение при реализации перемещения шар.

### <a name="making-the-ball-fall"></a>Делая попадают шар

Можно сделать попадают либо вручную реализации тяжести кодом или с помощью встроенной функции модуля Box2D в CocosSharp шар. Механизм моделирования физических программы Box2D доступен CocosSharp игры. Очень мощный и эффективный, но необходимо написать код установки. Так как физическое моделирование очень прост, здесь он будет реализован вручную.

Для реализации тяжести нам нужно будет хранилище текущего X и Y скорости шара. Мы добавим два члена в `GameLayer` класса:

```csharp
float ballXVelocity;
float ballYVelocity;
// How much to modify the ball's y velocity per second:
const float gravity = 140;
```

Далее можно реализовать падающим логику в `RunGameLogic`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
}
```

### <a name="moving-the-paddle-with-touch-input"></a>Перемещение ракетка с сенсорного ввода

Теперь, когда снижается шарик, мы добавим перемещение по горизонтали ракетка с помощью `CCEventListenerTouchAllAtOnce` объекта. Этот объект предоставляет ряд событий. В этом случае мы хотим получать уведомление, если переместить точки касания, можно настроить положение ракетка. `GameLayer` Уже создает `CCEventListenerTouchAllAtOnce`, поэтому нам просто нужно назначить `OnTouchesMoved` делегата. Чтобы сделать это, измените метод AddedToScene следующим образом:

```csharp
protected override void AddedToScene ()
{
    base.AddedToScene ();
    // Use the bounds to layout the positioning of the drawable assets
    CCRect bounds = VisibleBoundsWorldspace;
    // Register for touch events
    var touchListener = new CCEventListenerTouchAllAtOnce ();
    touchListener.OnTouchesEnded = OnTouchesEnded;
    // new code:
    touchListener.OnTouchesMoved = HandleTouchesMoved;
    AddEventListener (touchListener, this);
}
```

Далее мы реализовать `HandleTouchesMoved`:

```csharp
void HandleTouchesMoved (System.Collections.Generic.List<CCTouch> touches, CCEvent touchEvent)
{
    // we only care about the first touch:
    var locationOnScreen = touches [0].Location;
    paddleSprite.PositionX = locationOnScreen.X;
}
```

### <a name="implementing-ball-collision"></a>Реализация шарик конфликтов

Если мы запустим игры, теперь мы Обратите внимание, что шарик продолжится до ракетка. Мы будем реализовывать *конфликтов* (логика для обработки перекрывающихся объектов игры) в коде каждый кадр. Так как перемещение объектов замена помещает каждый кадр, проверка конфликтов является обычно также выполняется каждый кадр. Мы будем также добавлять скорости по оси X шарик достигает ракетка добавление некоторых запрос к игре, когда это означает, что нам нужно сохранить шарик двигаться по краю экрана. Это нужно сделать `RunGameLogic` кода после применения скорости для `ballSprite` после `// New Code`:

```csharp
void RunGameLogic(float frameTimeInSeconds)
{
    // This is a linear approximation, so not 100% accurate
    ballYVelocity += frameTimeInSeconds * -gravity;
    ballSprite.PositionX += ballXVelocity * frameTimeInSeconds;
    ballSprite.PositionY += ballYVelocity * frameTimeInSeconds;
    // New Code:
    // Check if the two CCSprites overlap...
    bool doesBallOverlapPaddle = ballSprite.BoundingBoxTransformedToParent.IntersectsRect(
        paddleSprite.BoundingBoxTransformedToParent);
    // ... and if the ball is moving downward.
    bool isMovingDownward = ballYVelocity < 0;
    if (doesBallOverlapPaddle && isMovingDownward)
    {
        // First let's invert the velocity:
        ballYVelocity *= -1;
        // Then let's assign a random value to the ball's x velocity:
        const float minXVelocity = -300;
        const float maxXVelocity = 300;
        ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    }
    // First let’s get the ball position:   
    float ballRight = ballSprite.BoundingBoxTransformedToParent.MaxX;
    float ballLeft = ballSprite.BoundingBoxTransformedToParent.MinX;
    // Then let’s get the screen edges
    float screenRight = VisibleBoundsWorldspace.MaxX;
    float screenLeft = VisibleBoundsWorldspace.MinX;
    // Check if the ball is either too far to the right or left:    
    bool shouldReflectXVelocity = 
        (ballRight > screenRight && ballXVelocity > 0) ||
        (ballLeft < screenLeft && ballXVelocity < 0);
    if (shouldReflectXVelocity)
    {
        ballXVelocity *= -1;
    }
}
```

## <a name="adding-scoring"></a>Добавление оценки

Теперь пригодный для воспроизведения, игры, последний шаг — Добавление логики для оценки. Во-первых, мы добавим элемент оценка GameLayer класс с именем `score`:

```csharp
int score;
```

Мы будем использовать эту переменную для отслеживания проигрывателя оценка и для отображения его с помощью `scoreLabel`. Для этого добавьте следующий код в инструкцию if в `RunGameLogic` при шар и ракетка перекрываются:

```csharp
// ...
if (doesBallOverlapPaddle && isMovingDownward)
{
    // First let's invert the velocity:
    ballYVelocity *= -1;
    // Then let's assign a random to the ball's x velocity:
    const float minXVelocity = -300;
    const float maxXVelocity = 300;
    ballXVelocity = CCRandom.GetRandomFloat (minXVelocity, maxXVelocity);
    // New code:
    score++;
    scoreLabel.Text = "Score: " + score;
}
// ...
```

Теперь мы запустить игру и в разделе, игра отображает оценку увеличит как шарик будет передаваться из ракетка:

![Завершенная игры с увеличивающимся оценкой](bouncing-game-images/image1.png "завершения игры с увеличивающимся индексом")

## <a name="summary"></a>Сводка

В этом пошаговом руководстве представлены создание кросс платформенной игры с графикой, физических и входных данных с помощью CocosSharp. Он является первым шагом в начало разработки игры CocosSharp. Мы рассмотрели некоторые из наиболее распространенных классов CocosSharp, создание визуального дерева, чтобы правильно отрисовкой объектов и как реализовать логику игры каждые кадра.

В этом пошаговом руководстве рассматривается только небольшую часть предлагает ядро CocosSharp игры. Сведения и пошаговые руководства на другие разделы CocosSharp см. в разделе [остальной части руководства CocosSharp](~/graphics-games/cocossharp/index.md).

## <a name="related-links"></a>Связанные ссылки

- [Завершенная игры (пример)](https://developer.xamarin.com/samples/mobile/BouncingGame/)
- [Игр (пример)](https://github.com/xamarin/mobile-samples/blob/master/BouncingGame/Resources/Content.zip?raw=true)
