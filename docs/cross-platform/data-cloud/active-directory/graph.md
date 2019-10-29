---
title: Доступ к API Graph
description: В этом документе описывается добавление Azure Active Directory проверки подлинности в мобильное приложение, созданное с помощью Xamarin.
ms.prod: xamarin
ms.assetid: F94A9FF4-068E-4B71-81FE-46920745380D
author: davidortinau
ms.author: daortin
ms.date: 03/23/2017
ms.openlocfilehash: 7ccfc082f86d0a0c6f8d29a477101edb72f9c92f
ms.sourcegitcommit: 2fbe4932a319af4ebc829f65eb1fb1816ba305d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2019
ms.locfileid: "73016632"
---
# <a name="accessing-the-graph-api"></a>Доступ к API Graph

Выполните следующие действия, чтобы использовать API Graph в приложении Xamarin:

1. [Регистрация на Azure Active Directory](~/cross-platform/data-cloud/active-directory/get-started/register.md) на портале *WindowsAzure.com* , а затем
2. [Настройка служб](~/cross-platform/data-cloud/active-directory/get-started/configure.md).

## <a name="step-3-adding-active-directory-authentication-to-an-app"></a>Шаг 3. Добавление Active Directory проверки подлинности в приложение

В приложении добавьте ссылку на **библиотеку проверки Подлинности Azure Active Directory (Azure ADAL)** с помощью диспетчера пакетов NuGet в Visual Studio или Visual Studio для Mac.
Убедитесь, что выбран параметр **Показывать предварительные пакеты** , чтобы включить этот пакет, так как он по-прежнему доступен в предварительной версии.

> [!IMPORTANT]
> Примечание. в настоящее время Azure ADAL 3,0 сейчас является предварительной версией и может привести к критическим изменениям до выпуска окончательной версии. 

![](graph-images/06.-adal-nuget-package.jpg "Add a reference to Azure Active Directory Authentication Library (Azure ADAL)")

Теперь в приложении необходимо добавить следующие переменные уровня класса, необходимые для потока проверки подлинности.

```csharp
//Client ID
public static string clientId = "25927d3c-.....-63f2304b90de";
public static string commonAuthority = "https://login.windows.net/common"
//Redirect URI
public static Uri returnUri = new Uri("http://xam-demo-redirect");
//Graph URI if you've given permission to Azure Active Directory
const string graphResourceUri = "https://graph.windows.net";
public static string graphApiVersion = "2013-11-08";
//AuthenticationResult will hold the result after authentication completes
AuthenticationResult authResult = null;
```

Обратите внимание на то, что `commonAuthority`. Когда конечная точка проверки подлинности `common`, приложение становится **многоклиентским**, что означает, что любой пользователь может использовать вход с учетными данными Active Directory. После проверки подлинности этот пользователь будет работать в контексте собственного Active Directory — т. е. он увидит подробные сведения о его Active Directory.

### <a name="write-method-to-acquire-access-token"></a>Метод Write для получения маркера доступа

Следующий код (для Android) начнет проверку подлинности и после завершения присвоит результат в `authResult`. Реализации iOS и Windows Phone немного отличаются: второй параметр (`Activity`) отличается в iOS и отсутствует в Windows Phone.

```csharp
public static async Task<AuthenticationResult> GetAccessToken
            (string serviceResourceId, Activity activity)
{
    authContext = new AuthenticationContext(Authority);
    if (authContext.TokenCache.ReadItems().Count() > 0)
        authContext = new AuthenticationContext(authContext.TokenCache.ReadItems().First().Authority);
    var authResult = await authContext.AcquireTokenAsync(serviceResourceId, clientId, returnUri, new AuthorizationParameters(activity));
    return authResult;
}  
```

В приведенном выше коде `AuthenticationContext` отвечает за проверку подлинности с помощью Коммонаусорити. Он имеет `AcquireTokenAsync` метод, принимающий параметры в качестве ресурса, к которому необходимо получить доступ, в данном случае `graphResourceUri`, `clientId`и `returnUri`. После завершения проверки подлинности приложение вернется к `returnUri`. Этот код останется одинаковым для всех платформ, однако последний параметр, `AuthorizationParameters`, будет отличаться на разных платформах и отвечает за управление потоком проверки подлинности.

В случае Android или iOS мы передаем параметр `this` в `AuthorizationParameters(this)` для совместного использования контекста, в то время как в Windows он передается без какого-либо параметра в качестве нового `AuthorizationParameters()`.

### <a name="handle-continuation-for-android"></a>Обработку продолжения для Android

После завершения проверки подлинности последовательность должна вернуться в приложение. В случае Android он обрабатывается следующим кодом, который следует добавить в **MainActivity.CS**:

```csharp
protected override void OnActivityResult(int requestCode, Result resultCode, Intent data)
{
  base.OnActivityResult(requestCode, resultCode, data);
  AuthenticationAgentContinuationHelper.SetAuthenticationAgentContinuationEventArgs(requestCode, resultCode, data);
}
```

### <a name="handle-continuation-for-windows-phone"></a>Обработку продолжения для Windows Phone

Для Windows Phone измените метод `OnActivated` в файле **app.XAML.CS** , используя приведенный ниже код.

```csharp
protected override void OnActivated(IActivatedEventArgs args)
{
#if WINDOWS_PHONE_APP
  if (args is IWebAuthenticationBrokerContinuationEventArgs)
  {
     WebAuthenticationBrokerContinuationHelper.SetWebAuthenticationBrokerContinuationEventArgs(args as IWebAuthenticationBrokerContinuationEventArgs);
  }
#endif
  base.OnActivated(args);
}
```

Теперь при запуске приложения появится диалоговое окно проверки подлинности.
После успешной проверки подлинности потребуются разрешения на доступ к ресурсам (в нашем случае API Graph):

![](graph-images/08.-authentication-flow.jpg "Upon successful authentication, it will ask your permissions to access the resources in our case Graph API")

Если проверка подлинности прошла успешно и вы разрешили приложению доступ к ресурсам, вы должны получить `AccessToken` и `RefreshToken` поле со списком в `authResult`. Эти маркеры необходимы для дальнейших вызовов API и для авторизации с Azure Active Directory в фоновом режиме.

![](graph-images/07.-access-token-for-authentication.jpg "These tokens are   required for further API calls and for authorization with Azure Active Directory behind the scenes")

Например, приведенный ниже код позволяет получить список пользователей из Active Directory. URL-адрес Web API можно заменить на веб-API, защищенный с помощью Azure AD.

```csharp
var client = new HttpClient();
var request = new HttpRequestMessage(HttpMethod.Get,
    "https://graph.windows.net/tendulkar.onmicrosoft.com/users?api-version=2013-04-05");
request.Headers.Authorization =
  new AuthenticationHeaderValue("Bearer", authResult.AccessToken);
var response = await client.SendAsync(request);
var content = await response.Content.ReadAsStringAsync();
```
