---
title: Перевод текста с помощью Translator API
description: API Microsoft Translator можно использовать для перевода речи и текста с помощью REST API. В этой статье объясняется, как использовать API Microsoft Translator текста для перевода текста с одного языка на другой в приложении Xamarin.Forms.
ms.prod: xamarin
ms.assetid: 68330242-92C5-46F1-B1E3-2395D8823B0C
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 02/08/2017
ms.openlocfilehash: 841b1d4abab5e4c09249174b221da20794771a86
ms.sourcegitcommit: db422e33438f1b5c55852e6942c3d1d75dc025c4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2020
ms.locfileid: "76725572"
---
# <a name="text-translation-using-the-translator-api"></a>Перевод текста с помощью Translator API

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)

_API Microsoft Translator можно использовать для перевода речи и текста с помощью REST API. В этой статье объясняется, как использовать API перевода текстов Майкрософт для перевода текста с одного языка на другой в приложении Xamarin. Forms._

## <a name="overview"></a>Обзор

Translator API состоит из двух компонентов:

- Перевод текста REST API, перевод текста с одного языка на текст другого языка. API автоматически определяет язык текста, который был отправлен перед его преобразование.
- Перевод речи REST API для транскрипция речи с одного языка в текст другого языка. API также сочетает в себе возможности преобразования текста в речь для обратного перевода переведенного текста.

Эта статья посвящена перевод текста с одного языка в другой с помощью API перевода текстов.

> [!NOTE]
> Если у вас еще нет [подписки Azure](/azure/guides/developer/azure-developer-guide#understanding-accounts-subscriptions-and-billing), создайте [бесплатную учетную запись Azure](https://aka.ms/azfree-docs-mobileapps), прежде чем начать работу.

Чтобы использовать API перевода текстов необходимо получить ключ API. Это можно получить с [помощью подписки на API перевода текстов Майкрософт](/azure/cognitive-services/translator/translator-text-how-to-signup/).

Дополнительные сведения о API перевода текстов Майкрософт см. в [документации по API перевода текстов](/azure/cognitive-services/translator/).

## <a name="authentication"></a>Аутентификация

Для каждого запроса к API перевода текстов требуется маркер доступа JSON Web Token (JWT), который можно получить из службы токенов «переопределяющие службы» по адресу `https://api.cognitive.microsoft.com/sts/v1.0/issueToken`. Маркер можно получить, выполнив запрос POST к службе маркеров, указав заголовок `Ocp-Apim-Subscription-Key`, содержащий ключ API в качестве значения.

В следующем примере кода показано, как запросить доступ маркера от службы маркеров:

```csharp
public AuthenticationService(string apiKey)
{
    subscriptionKey = apiKey;
    httpClient = new HttpClient();
    httpClient.DefaultRequestHeaders.Add("Ocp-Apim-Subscription-Key", apiKey);
}
...
async Task<string> FetchTokenAsync(string fetchUri)
{
    UriBuilder uriBuilder = new UriBuilder(fetchUri);
    uriBuilder.Path += "/issueToken";
    var result = await httpClient.PostAsync(uriBuilder.Uri.AbsoluteUri, null);
    return await result.Content.ReadAsStringAsync();
}
```

Токен возвращаемый доступа, представляющий собой текст в кодировке Base64, имеет время окончания срока действия 10 минут. Таким образом пример приложения обновляет токен доступа каждые 9 минут.

Маркер доступа должен быть указан в каждом вызове API перевода текстов в виде заголовка `Authorization` с префиксом строки `Bearer`, как показано в следующем примере кода:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);
```

Дополнительные сведения о службе маркеров для переприятных служб см. в разделе [Проверка подлинности](/azure/cognitive-services/translator/reference/v3-0-reference#authentication).

## <a name="performing-text-translation"></a>Выполняет перевод текста

Преобразование текста можно достичь, создав запрос GET к `translate` API на `https://api.microsofttranslator.com/v2/http.svc/translate`. В примере приложения метод `TranslateTextAsync` вызывает процесс преобразования текста:

```csharp
public async Task<string> TranslateTextAsync(string text)
{
  ...
  string requestUri = GenerateRequestUri(Constants.TextTranslatorEndpoint, text, "en", "de");
  string accessToken = authenticationService.GetAccessToken();
  var response = await SendRequestAsync(requestUri, accessToken);
  var xml = XDocument.Parse(response);
  return xml.Root.Value;
}
```

Метод `TranslateTextAsync` создает URI запроса и извлекает маркер доступа из службы маркеров. Затем запрос на перевод текста отправляется в `translate` API, который возвращает XML-ответ, содержащий результат. Ответ в формате XML анализируется, и результат перевода возвращается в вызывающий метод для отображения.

Дополнительные сведения о интерфейсах API для преобразования текста см. в разделе [API перевода текстов](/azure/cognitive-services/translator/reference/v3-0-reference).

### <a name="configuring-text-translation"></a>Настройка преобразования текста

Процесс преобразования текста можно настроить путем указания параметров запроса HTTP:

```csharp
string GenerateRequestUri(string endpoint, string text, string to)
{
  string requestUri = endpoint;
  requestUri += string.Format("?text={0}", Uri.EscapeUriString(text));
  requestUri += string.Format("&to={0}", to);
  return requestUri;
}
```

Этот метод задает текст для перевода, а также язык перевода текста. Список языков, поддерживаемых Microsoft Translator, см. [в статье Поддерживаемые языки в API перевода текстов Microsoft](/azure/cognitive-services/translator/languages/).

> [!NOTE]
> Если приложению необходимо знать, на каком языке находится текст, можно вызвать API `Detect`, чтобы определить язык текстовой строки.

### <a name="sending-the-request"></a>Отправка запроса

Метод `SendRequestAsync` делает запрос GET к преобразованию текста REST API и возвращает ответ:

```csharp
async Task<string> SendRequestAsync(string url, string bearerToken)
{
    if (httpClient == null)
    {
        httpClient = new HttpClient();
    }
    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", bearerToken);

    var response = await httpClient.GetAsync(url);
    return await response.Content.ReadAsStringAsync();
}
```

Этот метод создает запрос GET, добавляя маркер доступа к заголовку `Authorization` с префиксом строки `Bearer`. Затем запрос GET отправляется в `translate` API с URL-адресом запроса, который указывает текст для перевода, и язык, на который нужно перевести текст. Ответ считывается и возвращается вызывающему методу.

API `translate` отправит код состояния HTTP 200 (ОК) в ответе при условии, что запрос действителен, что означает, что запрос выполнен успешно и запрошенные сведения находятся в ответе. Список возможных ответов об ошибках см. в разделе ответные сообщения при [получении](/azure/cognitive-services/translator/reference/v3-0-translate).

### <a name="processing-the-response"></a>Обработка ответа

В ответе API возвращаются в формате XML. Следующий XML-данных показывает типичный успешный ответ сообщение:

```xml
<string xmlns="http://schemas.microsoft.com/2003/10/Serialization/">Morgen kaufen gehen ein</string>
```

В примере приложения XML-ответ разбивается на `XDocument` экземпляр, и корневое значение XML возвращается вызывающему методу для отображения, как показано на следующих снимках экрана:

![](text-translation-images/text-translation.png "Text Translation to German")

## <a name="summary"></a>Сводка

В этой статье описываются способы использования API Microsoft Translator Text перевод текста с одного языка на текст другого языка, в приложении Xamarin.Forms. Помимо перевода текста, API Microsoft Translator можно также транскрипция речи с одного языка в текст другого языка.

## <a name="related-links"></a>Связанные ссылки

- [Документация по API перевода текстов](/azure/cognitive-services/translator/)
- [Использование веб-службы RESTFUL](~/xamarin-forms/data-cloud/web-services/rest.md)
- [Cognitive Services ToDo (пример)](https://docs.microsoft.com/samples/xamarin/xamarin-forms-samples/webservices-todocognitiveservices)
- [API перевода текстов](/azure/cognitive-services/translator/reference/v3-0-reference)
