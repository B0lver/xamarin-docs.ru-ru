---
title: Привет, iOS
description: Из этого состоящего из двух частей руководства вы узнаете, как создать базовое приложение Xamarin.iOS в Visual Studio для Mac или Visual Studio. Вы также получите представление об основах разработки приложений iOS с помощью Xamarin. В нем рассматриваются средства, понятия и действия, необходимые для создания и развертывания приложения Xamarin.iOS.
ms.topic: article
ms.prod: xamarin
ms.assetid: D3868F3A-4EED-BDDF-45AA-665102C39634
ms.technology: xamarin-ios
author: bradumbaugh
ms.author: brumbaug
ms.date: 03/23/2017
ms.openlocfilehash: 6a26bb42ee0abcec70d17633152dd12bd200a737
ms.sourcegitcommit: 73bd0c7e5f237f0a1be70a6c1384309bb26609d5
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="helloios-quickstart"></a>Краткое руководство по Hello.iOS

Это руководство описывает создание приложения, которое преобразует введенный пользователем буквенно-цифровой телефонный номер в числовой телефонный номер и затем набирает его. Окончательный вариант приложения выглядит примерно так:

 [![](hello-ios-quickstart-images/image1.png "Приложение краткого руководства по Hello.iOS")](hello-ios-quickstart-images/image1.png#lightbox)


<a name="Requirements" />

## <a name="requirements"></a>Требования

Для разработки на iOS с помощью Xamarin требуется следующее:

-  Компьютер Mac под управлением macOS Sierra 10.12 или более поздней версии
-  Последняя версия Xcode и пакета SDK iOS из [App Store](https://itunes.apple.com/us/app/xcode/id497799835?mt=12).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

Xamarin.iOS работает со следующими конфигурациями:

-  Последняя версия Visual Studio для Mac, удовлетворяющая указанным выше требованиям.

Пошаговые инструкции по установке см. в [руководстве по установке Xamarin.iOS на Mac](~/ios/get-started/installation/mac.md).

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

Xamarin.iOS работает со следующими конфигурациями:

-  Последняя версия Visual Studio 2015 или 2017 Professional или более поздней версии на базе Windows 7 или более поздней версии, связанная с узлом сборки Mac, удовлетворяющим указанным выше требованиям.

Пошаговые инструкции по установке см. в [руководстве по установке Xamarin.iOS в Windows](~/ios/get-started/installation/windows/index.md).

-----

Перед началом работы скачайте [набор значков приложения Xamarin](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true).

# <a name="visual-studio-for-mactabvsmac"></a>[Visual Studio для Mac](#tab/vsmac)

## <a name="visual-studio-for-mac-walkthrough"></a>Пошаговое руководство по Visual Studio для Mac

Это пошаговое руководство описывает создание приложения Phoneword, которое преобразует буквенно-цифровой телефонный номер в числовой.

1. Запустите Visual Studio для Mac из папки **Applications** (Приложения) или из **Spotlight**, чтобы открыть экран запуска:

  ![](hello-ios-quickstart-images/image2new.png "Экран запуска")

На экране запуска нажмите кнопку **Новый проект...** , чтобы создать новое решение Xamarin.iOS:

![](hello-ios-quickstart-images/image3new.png "Решение iOS")


2. В диалоговом окне **Новое решение** выберите шаблон **iOS > Приложение > Приложение одного представления**, убедившись, что выбран C#. Нажмите кнопку **Далее**:

  ![](hello-ios-quickstart-images/image4new.png "Выбор приложения одного представления")

3. Настройте приложение. Присвойте ему **имя** `Phoneword_iOS`, а для остальных параметров сохраните значения по умолчанию. Нажмите кнопку **Далее**:

  ![](hello-ios-quickstart-images/image5new.png "Ввод имени приложения")

4. Оставьте имеющееся имя для проекта и решения. Выберите расположение проекта или оставьте значение по умолчанию:

  ![](hello-ios-quickstart-images/image6new.png "Выбор расположения проекта")

5. Нажмите кнопку **Создать**, чтобы создать **Решение**.

6. Откройте файл **Main.storyboard**, дважды щелкнув его на **Панели решения**. Это позволяет создавать пользовательский интерфейс визуально:

  ![](hello-ios-quickstart-images/image7new.png "Конструктор iOS")

  Обратите внимание на то, что _классы размера_ включены по умолчанию. Чтобы узнать о них подробнее, см. руководство по [унифицированным раскадровкам](~/ios/user-interface/storyboards/unified-storyboards.md).

8. Откройте **панель элементов**, введите "метка" в поле поиска и перетащите элемент **Метка** в область конструктора (в центре):

  ![](hello-ios-quickstart-images/image8new.png "Перетаскивание элемента "Метка" в область конструктора в центре")

  > [!NOTE]
  > Вы можете открыть **Панель свойств** или **Панель элементов** в любое время, перейдя в раздел **Вид > Панели**.

9. Используя маркеры *перетаскивания элементов управления* (кружки вокруг элемента управления), сделайте метку шире:

  ![](hello-ios-quickstart-images/image9.png "Увеличение ширины метки")


10. Выбрав элемент **Метка** в области конструктора, используйте **Панель свойств**, чтобы изменить свойство **Текст** элемента **Метка** на "Enter a Phoneword" (Введите слово-номер):

  ![](hello-ios-quickstart-images/image10.png "Установка метки для ввода слова-номера")

11. Выполните поиск строки "текстовое поле" на панели элементов и перетащите элемент **Текстовое поле** с **панели элементов** в область конструктора, поместив его под элементом **Метка**. Измените ширину, чтобы элемент **Текстовое поле** был такой же ширины, что и **Метка**:

  ![](hello-ios-quickstart-images/image12new.png "Выравнивание ширины текстового поля и метки")


12. Выбрав элемент **Текстовое поле** в области конструктора, измените для **текстового поля** свойство **Имя** в разделе "Удостоверение" на **панели свойств**, указав новое значение `PhoneNumberText`, а для свойства **Текст** введите "1-855-XAMARIN":

  ![](hello-ios-quickstart-images/image13new.png "Изменение значения свойства "Заголовок" на 1-855-XAMARIN")


13. Перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под элементом **Текстовое поле**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с элементами **Текстовое поле** и **Метка**:

  ![](hello-ios-quickstart-images/image14new.png "Настройка ширины элемента "Кнопка", чтобы выровнять его с элементами "Текстовое поле" и "Метка"")


14. Выбрав элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **Панели свойств** на `TranslateButton`. Измените значение свойства **Заголовок** на "Translate" (Преобразовать):

  ![](hello-ios-quickstart-images/image15new.png "Изменение значения свойства "Заголовок" на "Translate" (Преобразовать)")


15. Повторите два предыдущих действия, перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под первым элементом **Кнопка**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с первым элементом **Кнопка**:

  ![](hello-ios-quickstart-images/image16new.png "Настройка ширины элемента "Кнопка", чтобы выровнять его с первым элементом "Кнопка"")


16. Выбрав второй элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **Панели свойств** на `CallButton`. Измените значение свойства **Заголовок** на "Call" (Вызов):

  ![](hello-ios-quickstart-images/image17new.png "Изменение значения свойства "Заголовок" на "Call" (Вызов)")

  Сохраните изменения, выбрав **Файл > Сохранить** или нажав клавиши **⌘+S**.


17. В приложение нужно добавить логику для преобразования телефонных номеров из буквенно-цифровых в цифровые. Добавьте новый файл в проект, щелкнув правой кнопкой мыши проект **Phoneword_iOS** на **Панели решения** и выбрав **Добавить > Новый файл...** или нажав клавиши **⌘+N**:

  ![](hello-ios-quickstart-images/image18.png "Добавление нового файла в проект")


18. В диалоговом окне **Новый файл** выберите **Общие > Пустой класс** и назовите новый файл `PhoneTranslator`:

  ![](hello-ios-quickstart-images/image19.png "Выбор параметра "Пустой класс" и присвоение имени PhoneTranslator новому файлу")


19. Создается пустой класс C#. Удалите код шаблона и замените его следующим:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword_iOS
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

    Сохраните файл **PhoneTranslator.cs** и закройте его.

20. Добавьте код для подключения пользовательского интерфейса. Для этого дважды щелкните файл **ViewController.cs** на **Панели решения**, чтобы открыть его:

  ![](hello-ios-quickstart-images/image20new.png "Добавление кода для подключения пользовательского интерфейса")


21. Начните с подключения `TranslateButton`. Найдите метод `ViewDidLoad` в классе **ViewController** и добавьте следующий код под вызовом `base.ViewDidLoad()`:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {
        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(
            PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call ", UIControlState.Normal);
            CallButton.Enabled = false;
        } else {
            CallButton.SetTitle ("Call " + translatedNumber,
                UIControlState.Normal);
            CallButton.Enabled = true;
        }
    };
    ```

    Включите `using Phoneword_iOS;`, если пространство имен файла отличается.

22. Добавьте код, реагирующий на нажатие пользователем второй кнопки, которая называется `CallButton`. Поместите следующий код после кода для `TranslateButton` и добавьте `using Foundation;` в начало файла:

    ```csharp
        CallButton.TouchUpInside += (object sender, EventArgs e) => {
            // Use URL handler with tel: prefix to invoke Apple's Phone app...
            var url = new NSUrl ("tel:" + translatedNumber);

            // ...otherwise show an alert dialog
            if (!UIApplication.SharedApplication.OpenUrl (url)) {
                var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                PresentViewController (alert, true, null);
            }
        };
    ```


23. Сохраните изменения и выполните сборку приложения, выбрав **Сборка > Собрать все** или нажав клавиши **⌘+B**.  Если приложение компилируется, в верхней части интегрированной среды разработки отображается сообщение об успешном выполнении:

  ![](hello-ios-quickstart-images/image21.png "Отображение сообщения об успешном выполнении в верхней части интегрированной среды разработки")

  При наличии ошибок просмотрите предыдущие шаги и исправьте все ошибки, пока сборка приложения не будет проходить успешно.

27. Наконец, протестируйте приложение в **симуляторе iOS**. В верхнем левом углу интегрированной среды разработки выберите значение **Отладка** в первом раскрывающемся списке и **iPhone 8 Plus iOS x.x** во втором, а затем нажмите кнопку **Запустить** (треугольная кнопка, которая напоминает кнопку воспроизведения):

  ![](hello-ios-quickstart-images/image27new.png "Нажатие кнопки "Запустить"")

  > [!NOTE]
  > Сейчас, в связи с требованиями Apple, при создании кода для устройства или симулятора может потребоваться сертификат разработки или *удостоверение подписывания*. Чтобы настроить их, следуйте указаниям в руководстве по [подготовке устройств](~/ios/get-started/installation/device-provisioning/manual-provisioning.md).

28. В результате приложение запускается в симуляторе iOS:

  ![](hello-ios-quickstart-images/image28.png "Выполнение приложения в симуляторе iOS")

  Симулятор iOS не поддерживает телефонные звонки; при попытке выполнить вызов отображается диалоговое окно предупреждения:

  ![](hello-ios-quickstart-images/image29.png "Диалоговое окно предупреждения при попытке выполнения вызова")

# <a name="visual-studiotabvswin"></a>[Visual Studio](#tab/vswin)

## <a name="visual-studio-walkthrough"></a>Пошаговое руководство по Visual Studio

Это пошаговое руководство описывает создание приложения Phoneword, которое преобразует буквенно-цифровой телефонный номер в числовой.

**Примечание**. В этом пошаговом руководстве используется Visual Studio Enterprise 2017 на виртуальной машине Windows 10. Ваша конфигурация может отличаться при условии соблюдения приведенных выше требований, но имейте в виду, что некоторые снимки экрана могут отличаться от вашей конфигурации.

> [!NOTE]
> Прежде чем продолжить работу с пошаговым руководством, вам нужно подключиться к компьютеру Mac из Visual Studio. Это вызвано тем, что Xamarin.iOS использует инструменты Apple при сборке и запуске конструктора и приложений iOS. Чтобы настроить это, выполните инструкции из руководства по [подключению к Mac](~/ios/get-started/installation/windows/connecting-to-mac/index.md).

1. Запустите Visual Studio из меню **Пуск**:

  ![](hello-ios-quickstart-images/image001-.png "Начальный экран")

  В поле поиска под элементом **Новое решение** введите _приложение с одним представлением_  и выберите **Приложение с одним представлением (iPhone)**, чтобы создать решение Xamarin.iOS:

  ![](hello-ios-quickstart-images/image002-.png "Добавление приложения с одним представлением")


2. Назовите проект и решение `Phoneword`, как показано ниже:

  ![](hello-ios-quickstart-images/vs-image3.png "Присвоение проекту имени PhonewordiOS, а новому решению — имени Phoneword")


3. Нажмите кнопку **ОК** для создания проекта.

4. Убедитесь, что значок агента Mac Xamarin на панели инструментов отображается зеленым цветом.

    ![Убедитесь, что значок агента Mac Xamarin на панели инструментов отображается зеленым цветом](hello-ios-quickstart-images/vs-image4.png)

    Если это не так, то есть отсутствует подключение к компьютеру сборки Mac, выполните инструкции из [руководства по выбору конфигурации](~/ios/get-started/installation/windows/connecting-to-mac/index.md), чтобы настроить подключение.


5. Откройте файл **Main.storyboard** в конструкторе iOS, дважды щелкнув его в **обозревателе решений**:

  ![](hello-ios-quickstart-images/vs-image7.png "Конструктор iOS")

6. Откройте вкладку **Панель элементов**, введите "метка" в поле поиска и перетащите элемент **Метка** в область конструктора (в центре):

  ![](hello-ios-quickstart-images/vs-image8.png "Перетаскивание элемента "Метка" в область конструктора в центре")


7. Захватите *маркеры перетаскивания* и растяните метку в стороны:

  ![](hello-ios-quickstart-images/vs-image9.png "Увеличение ширины метки")


8. Выбрав элемент **Метка** в области конструктора, используйте **окна свойств**, чтобы изменить свойство **Текст** элемента **Метка** на "Enter a Phoneword" (Введите слово-номер):

  ![](hello-ios-quickstart-images/vs-image10.png "Изменение свойства "Текст" метки на "Enter a Phoneword" (Введите слово-номер)")

  > [!NOTE]
  > Вы можете открыть **Свойства** или **Панель элементов** в любое время. Для этого перейдите в меню **Вид**.


9. Выполните поиск строки "текстовое поле" на панели элементов и перетащите элемент **Текстовое поле** с **панели элементов** в область конструктора, поместив его под элементом **Метка**. Измените ширину, чтобы элемент **Текстовое поле** был такой же ширины, что и **Метка**:

  ![](hello-ios-quickstart-images/vs-image12.png "Настройка ширины элемента "Текстовое поле", чтобы выровнять его с элементом "Метка"")


10. Выбрав элемент **Текстовое поле** в области конструктора, измените его**свойство** **Имя** в разделе "Удостоверение" **свойств** на `PhoneNumberText`, а также измените свойство **Текст** на "1-855-XAMARIN":

  ![](hello-ios-quickstart-images/vs-image13.png "Изменение значения свойства "Текст" на 1-855-XAMARIN")


11. Перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под элементом **Текстовое поле**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с элементами **Текстовое поле** и **Метка**:

  ![](hello-ios-quickstart-images/vs-image14.png "Настройка ширины элемента "Кнопка", чтобы выровнять его с элементами "Текстовое поле" и "Метка"")


12. Выбрав элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **свойств** на `TranslateButton`. Измените значение свойства **Заголовок** на "Translate" (Преобразовать):

  ![](hello-ios-quickstart-images/vs-image15.png "Изменение значения свойства "Заголовок" на "Translate" (Преобразовать)")


13. Повторите два предыдущих действия, перетащите элемент **Кнопка** из **панели элементов** в область конструктора и поместите его под первым элементом **Кнопка**. Настройте ширину элемента **Кнопка**, чтобы выровнять его с первым элементом **Кнопка**:

  ![](hello-ios-quickstart-images/vs-image16.png "Настройка ширины элемента "Кнопка", чтобы выровнять его с первым элементом "Кнопка"")


14. Выбрав второй элемент **Кнопка** в области конструктора, измените свойство **Имя** в разделе **Удостоверение** **свойств** на `CallButton`. Измените значение свойства **Заголовок** на "Call" (Вызов):

  ![](hello-ios-quickstart-images/vs-image17.png "Изменение значения свойства "Заголовок" на "Call" (Вызов)")

  Сохраните изменения, выбрав **Файл > Сохранить все** или нажав клавиши **CTRL+S**.

15. Добавьте код, который преобразует телефонные номера из буквенно-цифровой записи в цифровую. Для этого сначала добавьте новый файл в проект, щелкнув правой кнопкой мыши проект **Phoneword** в **обозревателе решений** и выбрав **Добавить > Новый элемент...** или нажав клавиши **CTRL+SHIFT+A**:

  ![](hello-ios-quickstart-images/vs-image18.png "Добавление кода для преобразования телефонных номеров из буквенно-цифровых в цифровые")


16. В диалоговом окне **Новый файл** выберите **Apple > Класс** и назовите новый файл `PhoneTranslator`:

  ![](hello-ios-quickstart-images/vs-image19.png "Добавление нового класса с именем PhoneTranslator")

  > [!IMPORTANT]
  > Убедитесь, что выбран шаблон класса, в значке которого указан C#. В противном случае ссылка на этот класс может быть невозможна.


17. Создается класс C#. Удалите код шаблона и замените его следующим:

    ```csharp
    using System.Text;
    using System;

    namespace Phoneword
    {
        public static class PhoneTranslator
        {
            public static string ToNumber(string raw)
            {
                if (string.IsNullOrWhiteSpace(raw)) {
                    return "";
                } else {
                    raw = raw.ToUpperInvariant();
                }

                var newNumber = new StringBuilder();
                foreach (var c in raw)
                {
                    if (" -0123456789".Contains(c)) {
                        newNumber.Append(c);
                    } else {
                        var result = TranslateToNumber(c);
                        if (result != null) {
                            newNumber.Append(result);
                        }
                    }
                    // otherwise we've skipped a non-numeric char
                }
                return newNumber.ToString();
            }

            static bool Contains (this string keyString, char c)
            {
                return keyString.IndexOf(c) >= 0;
            }

            static int? TranslateToNumber(char c)
            {
                if ("ABC".Contains(c)) {
                    return 2;
                } else if ("DEF".Contains(c)) {
                    return 3;
                } else if ("GHI".Contains(c)) {
                    return 4;
                } else if ("JKL".Contains(c)) {
                    return 5;
                } else if ("MNO".Contains(c)) {
                    return 6;
                } else if ("PQRS".Contains(c)) {
                    return 7;
                } else if ("TUV".Contains(c)) {
                    return 8;
                } else if ("WXYZ".Contains(c)) {
                    return 9;
                }
                return null;
            }
        }
    }
    ```

  Сохраните файл **PhoneTranslator.cs** и закройте его.

18. Дважды щелкните файл **ViewController.cs** в **обозревателе решений**, чтобы открыть его и добавить логику, обрабатывающую взаимодействие с кнопками:

  ![](hello-ios-quickstart-images/vs-image20.png "Добавление логики для обработки взаимодействия с кнопками")


19. Начните с подключения `TranslateButton`. В классе **ViewController** найдите метод `ViewDidLoad`. Добавьте следующий код кнопки внутрь `ViewDidLoad`, под вызовом `base.ViewDidLoad()`:

    ```csharp
    string translatedNumber = "";

    TranslateButton.TouchUpInside += (object sender, EventArgs e) => {

        // Convert the phone number with text to a number
        // using PhoneTranslator.cs
        translatedNumber = PhoneTranslator.ToNumber(PhoneNumberText.Text);

        // Dismiss the keyboard if text field was tapped
        PhoneNumberText.ResignFirstResponder ();

        if (translatedNumber == "") {
            CallButton.SetTitle ("Call", UIControlState.Normal);
            CallButton.Enabled = false;
            }
        else {
            CallButton.SetTitle ("Call " + translatedNumber, UIControlState.Normal);
            CallButton.Enabled = true;
            }
    };
    ```
    Включите `using Phoneword;`, если пространство имен файла отличается.

20. Добавьте код, реагирующий на нажатие пользователем второй кнопки, которая называется `CallButton`. Поместите следующий код после кода для `TranslateButton` и добавьте `using Foundation;` в начало файла:

    ```csharp
    CallButton.TouchUpInside += (object sender, EventArgs e) => {
        var url = new NSUrl ("tel:" + translatedNumber);

            // Use URL handler with tel: prefix to invoke Apple's Phone app,
            // otherwise show an alert dialog

        if (!UIApplication.SharedApplication.OpenUrl (url)) {
                        var alert = UIAlertController.Create ("Not supported", "Scheme 'tel:' is not supported on this device", UIAlertControllerStyle.Alert);
                        alert.AddAction (UIAlertAction.Create ("Ok", UIAlertActionStyle.Default, null));
                        PresentViewController (alert, true, null);
                    }
    };
    ```


21. Сохраните изменения и выполните сборку приложения, выбрав **Сборка > Построить решение** или нажав клавиши **CTRL+SHIFT+B**.  Если приложение компилируется, в нижней части интегрированной среды разработки отображается сообщение об успешном выполнении:

  ![](hello-ios-quickstart-images/vs-image21.png "Отображение сообщения об успешном выполнении в нижней части интегрированной среды разработки")

  При наличии ошибок просмотрите предыдущие шаги и исправьте все ошибки, пока сборка приложения не будет проходить успешно.

22. Наконец, протестируйте приложение в **удаленном симуляторе iOS**. На панели инструментов интегрированной среды разработки выберите **Отладка** и **iPhone 8 Plus iOS x.x** в раскрывающихся меню и щелкните значок **Запустить** (зеленый треугольник, похожий на кнопку воспроизведения):

  ![](hello-ios-quickstart-images/vs-image27.png "Нажатие кнопки "Запустить"")

23. В результате приложение запускается в симуляторе iOS:

  ![](hello-ios-quickstart-images/vs-image28.png "Выполнение приложения в симуляторе iOS")

  Симулятор iOS не поддерживает телефонные звонки; при попытке выполнить вызов отображается диалоговое окно предупреждения:

  ![](hello-ios-quickstart-images/vs-image29.png "Диалоговое окно предупреждения, отображаемое при попытке выполнения вызова")

-----

Поздравляем! Вы создали свое первое приложение Xamarin.iOS!

Пришло время закрепить и углубить приобретенные знания и навыки с помощью раздела [Привет, iOS: теперь подробнее](~/ios/get-started/hello-ios/hello-ios-deepdive.md).


## <a name="related-links"></a>Связанные ссылки

- [Значки приложения Xamarin и изображения при запуске (пример)](https://github.com/xamarin/ios-samples/blob/master/Hello_iOS/Resources/XamarinAppIconsandLaunchImages.zip?raw=true)
- [Привет, iOS (пример)](https://developer.xamarin.com/samples/monotouch/Hello_iOS/)
- [Рекомендации по работе с человеческим интерфейсом iOS](http://developer.apple.com/library/ios/#documentation/UserExperience/Conceptual/MobileHIG/Introduction/Introduction.html)
- [Портал подготовки iOS](https://developer.apple.com/ios/manage/overview/index.action)
