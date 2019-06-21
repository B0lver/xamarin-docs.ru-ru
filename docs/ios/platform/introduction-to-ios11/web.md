---
title: Изменения WebKit и Safari в iOS 11
description: В этом документе рассматриваются изменения, внесенные в WebKit и платформу служб Safari в iOS 11. Он описывает способы работы с Задание стиля обновлений в SFSafariViewController и новых возможностях WKWebView.
ms.prod: xamarin
ms.assetid: C74B2E94-177C-43D4-8D6C-9B528773C120
ms.technology: xamarin-ios
author: lobrien
ms.author: laobri
ms.date: 09/12/2017
ms.openlocfilehash: 5ced73b1f3f5b8207ae1258dcb01a78c94df217d
ms.sourcegitcommit: a153623a69b5cb125f672df8007838afa32e9edf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2019
ms.locfileid: "67268918"
---
# <a name="webkit-and-safari-changes-in-ios-11"></a>Изменения WebKit и Safari в iOS 11

iOS 11 предлагает новый вариант браузера Safari — Safari 11.0 — включая изменения WebKit и SafariServices. В этом руководстве рассматриваются эти изменения.

## <a name="safariservices"></a>SafariServices

`SFSafariViewController` впервые появился в iOS 9 как параметр для отображения веб-содержимого или проверки подлинности пользователей из вашего приложения. Дополнительные сведения о его функциях можно найти в [веб-представления](~/ios/user-interface/controls/uiwebview.md#safariviewcontroller) руководства.

iOS 11 был представлен стиля обновлений на контроллер представления Safari, предоставление пользователям более удобную работу между приложением и веб-узла. Например удаление из адресной строки теперь предоставляет контроллер представления вида в приложении браузера, а не мини-обозреватель Safari. Вы также можете настроить цвет в соответствии с цветовой схемы приложения, задав `preferredBarTintColor` и `PreferredControlTintColor` свойства:

```csharp
sfViewController.PreferredControlTintColor = UIColor.White;
sfViewController.PreferredBarTintColor = UIColor.Purple;
```

В следующем фрагменте кода отображает столбцы фиолетовым до белого, как показано на следующем рисунке:

![Полосы SFSafariViewController, подготавливается к просмотру в фиолетовым до белого](web-images/image1.png)

Кнопки закрытия, представленные в контроллер представления Safari также можно изменить, задав `DismissButtonStyle` значение `Done`, `Close`, или `Cancel`:

```csharp
sfViewController.DismissButtonStyle = SFSafariViewControllerDismissButtonStyle.Close;
```

![Отклонить изменения текста кнопки](web-images/image2.png)

Это значение может быть изменено во время `SFSafariViewController` представлены.


В зависимости от содержимого, который отображается в контроллер представления Safari возможно, необходимо, чтобы гарантировать, что строки меню не свернуть прокрутке пользователем. Эта функция включена, задав новое `BarCollapsedEnabled` свойства `false`:

```csharp
var config = new SFSafariViewControllerConfiguration();
config.BarCollapsingEnabled = false;

var sfViewController = new SFSafariViewController(url, config);
```

![Панель свертывание отключена](web-images/image3.png)

Apple внесла обновлений к конфиденциальности в контроллере представления Safari в iOS 11. Теперь Просмотр данных, такие как файлы cookie и локального хранилища существуют только на основе каждого приложения, а не для всех экземпляров контроллера представления Safari. При этом сохраняется закрытыми в пределах приложения обзора действий пользователей.

Дополнительные функции например перетаскивание поддержку URL-адресов и поддержка `window.open()` также были добавлены `SFSafariViewController` в iOS 11. Можно найти дополнительные сведения об этих новых функциях в [документации Apple SFSafariViewController](https://developer.apple.com/documentation/safariservices/sfsafariviewcontroller?changes=latest_minor).


## <a name="webkit"></a>WebKit

`WKWebView` появилась как часть WebKit в iOS 8 как средство отображения веб-содержимого для пользователей. Гораздо более возможность настройки, чем `SFSafariViewController`, что позволяет создавать собственные навигации и пользовательский интерфейс.

Apple представила три основные улучшения для `WKWebView` с iOS 11: 

- Возможность управления файлами cookie
- Фильтрация содержимого
- Загрузка пользовательских ресурсов. 

Файлы «cookie» осуществляется с помощью нового [ `WKHttpCookieStore` ](https://developer.apple.com/documentation/webkit/wkhttpcookiestore) класс, который позволяет добавлять и удалять файлы cookie, чтобы получить все файлы cookie, хранящиеся в WKWebView и для наблюдения за хранилище для изменения файла cookie.

Содержимого, фильтрации позволяет управлять тип содержимого, которое пользователь увидит, позволяя убедитесь, что это безопасные, семейств удобных и при необходимости, доступно только для выбора группы пользователей. Это реализуется с помощью нового [ `WKContentRuleList` ](https://developer.apple.com/documentation/webkit/wkcontentrulelist) класса, указав пары триггеры и действия в формате JSON. Дополнительные сведения об этих триггерах и действиях можно найти в компании Apple [содержимого правила блокировки](https://developer.apple.com/library/content/documentation/Extensions/Conceptual/ContentBlockingRules/Introduction/Introduction.html) руководства.

iOS 11 теперь позволяет настраивать `WKWebView` с помощью настраиваемых ресурсов, загрузка для веб-содержимому. Это реализуется с помощью `IWKUrlSchemeHandler` интерфейс, который позволяет обрабатывать схемы URL-адресов, не включены в пакет средств Web. Этот интерфейс содержит метод start и stop, который должен быть реализован:

```csharp
public class MyHandler : NSObject, IWKUrlSchemeHandler {

    [Export("webView:startURLSchemeTask:")]
    public void StartUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        
        // Implement a IWKUrlSchemeTask here
        var response = new NSUrlResponse(urlSchemeTask.Request.Url, "text/html", ContentLength, null);
        urlSchemeTask.DidReceiveResponse(response);
        urlSchemeTask.DidReceiveData(someData);
        urlSchemeTask.DidFinish();
    }

    [Export("webView:stopURLSchemeTask:")]
    public void StopUrlSchemeTask(WKWebView webView, IWKUrlSchemeTask urlSchemeTask){
        throw new NotImplementedException();
    }

}
``` 

После реализации обработчика, использовать его для задания `SetUrlSchemeHandler` свойство `WKWebViewConfiguration`. Затем загрузите URL-адрес чего-либо, использующий настраиваемой схемой:

```csharp
var config = new WKWebViewConfiguration();
config.SetUrlSchemeHandler(new MyHandler(), "xamarin-asset");

webView = new WKWebView (View.Frame, config);
webView.LoadRequest (new NSUrlRequest("xamarin-asset://xamarin.com"));
```

