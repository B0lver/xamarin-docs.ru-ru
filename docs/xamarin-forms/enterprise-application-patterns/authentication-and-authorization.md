---
title: "Аутентификация и авторизация"
ms.topic: article
ms.prod: xamarin
ms.assetid: e3f27b4c-f7f5-4839-a48c-30bcb919c59e
ms.technology: xamarin-forms
author: davidbritch
ms.author: dabritch
ms.date: 08/08/2017
ms.openlocfilehash: 8b1715c8e7c3e9bb296577acd3d09a0f22488250
ms.sourcegitcommit: 61f5ecc5a2b5dcfbefdef91664d7460c0ee2f357
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/28/2018
---
# <a name="authentication-and-authorization"></a>Аутентификация и авторизация

Проверка подлинности — это процесс получения идентификационных учетных данных, такие как имя и пароль пользователя и проверки этих учетных данных на наличие полномочий. Если учетные данные являются допустимыми, сущность отправки учетных данных считается прошедшим проверку подлинности. После проверки подлинности удостоверения процесс авторизации определяет, является ли это удостоверение имеет доступ к данному ресурсу.

Существует множество подходов к интеграции проверки подлинности и авторизации в приложении Xamarin.Forms, который обменивается данными с веб-приложения ASP.NET MVC, включая поставщиков внешней проверки подлинности, например Microsoft, Google, с помощью ASP.NET Core Identity, Facebook или Twitter и проверки подлинности по промежуточного слоя. Мобильное приложение eShopOnContainers выполняет проверку подлинности и авторизации с помощью микрослужбу контейнерного удостоверений, использующий IdentityServer 4. Мобильное приложение запрашивает IdentityServer, маркеры безопасности для проверки подлинности пользователя или для доступа к ресурсу. Для IdentityServer для выдачи токенов от имени пользователя пользователь должен Войдите на IdentityServer. Однако IdentityServer не предоставляет пользовательский интерфейс или базы данных для проверки подлинности. Таким образом в приложении eShopOnContainers ссылку для этой цели используется ASP.NET Core Identity.

## <a name="authentication"></a>Проверка подлинности

Требуется проверка подлинности, когда приложению требуется проверить удостоверение текущего пользователя. ASP.NET Core основной механизм для идентификации пользователей — система членства ASP.NET Core Identity, где хранятся сведения о пользователе в хранилище данных, настроены разработчиком. Как правило это хранилище данных будет хранилище EntityFramework, хотя пользовательских хранилищах или пакеты сторонних производителей может использоваться для хранения идентификационных данных в хранилище Azure, DocumentDB или других местах.

Для сценарии проверки подлинности, которые используют хранилище данных локального пользователя, и которые сохраняют сведения об удостоверении между запросами через файлы cookie (как обычно в веб-приложениях ASP.NET MVC), ASP.NET Core Identity — это подходящее решение. Тем не менее файлы cookie не всегда являются естественным означает сохранения и передачи данных. Например веб-приложения ASP.NET Core, RESTful конечные точки, которые запрашиваются из мобильного приложения обычно требуется использование токена проверки подлинности носителя, так как в этом случае нельзя использовать файлы cookie. Тем не менее токены носителя можно легко получать и включены в состав заголовка авторизации веб-запросов, выполненных из мобильного приложения.

### <a name="issuing-bearer-tokens-using-identityserver-4"></a>Выдача токенов носителя с помощью IdentityServer 4

[IdentityServer 4](https://github.com/IdentityServer/IdentityServer4) — это платформа OpenID Connect и OAuth 2.0 открытым исходным кодом, для ASP.NET Core, который может использоваться для многих сценариев проверки подлинности и авторизации, включая выдает маркеры безопасности для локальных пользователей ASP.NET Core Identity.

> [!NOTE]
> OpenID Connect и OAuth 2.0 очень похожи, имея различные обязанности.

OpenID Connect представляет собой уровень проверки подлинности на основе протокола OAuth 2.0. OAuth 2 — это протокол, который позволяет приложениям для запроса маркеров доступа от службы маркеров безопасности и использовать их для взаимодействия с API-интерфейсы. Делегирование снижает сложность в клиентские приложения и API-интерфейсы с момента можно централизованной проверки подлинности и авторизации.

Сочетание OpenID Connect и OAuth 2.0 объединения двух основных компонентов системы безопасности проблемы проверки подлинности и доступа к API и IdentityServer 4 — это реализация этих протоколов.

В приложениях, использующих прямой обмен данными клиента микрослужбу, например приложение ссылку eShopOnContainers микрослужбу выделенный проверки подлинности, выступающего в качестве токенов безопасности службы (STS) можно использовать для проверки подлинности пользователей, как показано на рисунке 9-1. Дополнительные сведения о прямой обмен данными клиента микрослужбу разделе [обмен данными между клиентом и Микрослужбами](~/xamarin-forms/enterprise-application-patterns/containerized-microservices.md#communication_between_client_and_microservices).

![](authentication-and-authorization-images/authentication.png "Проверка подлинности с микрослужбу выделенной проверки подлинности")

**Рис. 9-1:** проверку подлинности путем микрослужбу выделенной проверки подлинности

Мобильное приложение eShopOnContainers взаимодействует с микрослужбу удостоверение, использующий IdentityServer 4 для выполнения проверки подлинности и управление доступом для API-интерфейсов. Таким образом мобильное приложение запрашивает маркеры IdentityServer, для проверки подлинности пользователя или для доступа к ресурсу:

-   Проверка подлинности с IdentityServer достигается, запрашивая мобильного приложения *удостоверение* маркер, который представляет результат процесса проверки подлинности. Как минимум он содержит идентификатор пользователя и сведения о том, как и когда пользователь проверку подлинности. Он также может содержать дополнительные учетные данные.
-   Доступ к ресурсу с IdentityServer достигается путем запроса мобильное приложение *доступа* маркер, позволяющий получить доступ к ресурсу API. Клиенты запроса маркеров доступа и пересылать их в API. Маркеры доступа содержат сведения о клиенте и пользователь (при его наличии). API-интерфейсы, затем использовать эту информацию для авторизации доступа к их данным.

> [!NOTE]
> **Примечание**: клиент должен быть зарегистрирован с IdentityServer, прежде чем он сможет запрашивать токены.

### <a name="adding-identityserver-to-a-web-application"></a>Добавление IdentityServer веб-приложение

Чтобы веб-приложения ASP.NET Core для использования IdentityServer 4 ее необходимо добавлять решение Visual Studio для веб-приложения. Дополнительные сведения см. в разделе [Установка и общие сведения о](https://identityserver4.readthedocs.io/en/release/quickstarts/0_overview.html) в документации по IdentityServer.

После IdentityServer включен в решение Visual Studio для веб-приложения, ее необходимо добавлять в конвейер обработки запросов HTTP для веб-приложения, чтобы обслуживать запросы к конечным точкам OpenID Connect и OAuth 2.0. Это можно сделать в `Configure` метода в веб-приложении `Startup` класса, как показано в следующем примере кода:

```csharp
public void Configure(  
    IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)  
{  
    ...  
    app.UseIdentity();  
    ...  
}
```

Порядок имеет значение в конвейер обработки запросов HTTP для веб-приложения. Таким образом IdentityServer необходимо добавить в конвейер, прежде чем платформа пользовательского интерфейса, который реализует экран входа.

### <a name="configuring-identityserver"></a>Настройка IdentityServer

IdentityServer должна быть настроена в `ConfigureServices` метод веб-приложения `Startup` класса путем вызова `services.AddIdentityServer` метода, как показано в следующем примере кода из приложения eShopOnContainers ссылку:

```csharp
public void ConfigureServices(IServiceCollection services)  
{  
    ...  
    services.AddIdentityServer(x => x.IssuerUri = "null")  
        .AddSigningCredential(Certificate.Get())                 
        .AddAspNetIdentity<ApplicationUser>()  
        .AddConfigurationStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .AddOperationalStore(builder =>  
            builder.UseSqlServer(connectionString, options =>  
                options.MigrationsAssembly(migrationsAssembly)))  
        .Services.AddTransient<IProfileService, ProfileService>();  
}
```

После вызова метода `services.AddIdentityServer` метод, дополнительные fluent API называются настройте следующие компоненты:

-   Учетные данные, используемые для подписывания.
-   API и удостоверения ресурсы, которые пользователи могут запрашивать доступ к.
-   Клиенты, которым будет выполняться подключение для запроса маркеров.
-   Удостоверение ASP.NET Core.

>💡 **Совет**: динамически загрузить конфигурацию IdentityServer 4. IdentityServer 4 API-интерфейсы позволяют настроить IdentityServer из списка в памяти объектов конфигурации. В приложении ссылку eShopOnContainers этих коллекций в памяти жестко запрограммированы в приложение. Однако в рабочих сценариях их можно загрузить динамически с помощью файла конфигурации или из базы данных.

Сведения о настройке IdentityServer для использования ASP.NET Core Identity см. в разделе [с помощью ASP.NET Identity Core](https://identityserver4.readthedocs.io/en/release/quickstarts/6_aspnet_identity.html) в документации по IdentityServer.

#### <a name="configuring-api-resources"></a>Настройка ресурсов API

При настройке ресурсов API `AddInMemoryApiResources` метод ожидает `IEnumerable<ApiResource>` коллекции. В следующем примере кода показан `GetApis` метод, который содержит эту коллекцию на eShopOnContainers обращение к приложению:

```csharp
public static IEnumerable<ApiResource> GetApis()  
{  
    return new List<ApiResource>  
    {  
        new ApiResource("orders", "Orders Service"),  
        new ApiResource("basket", "Basket Service")  
    };  
}
```

Этот метод указывает, что IdentityServer следует защитить заказов и Корзина API-интерфейсы. Таким образом, IdentityServer управляемый доступ маркеры будут необходимы для внесения вызовы этих интерфейсов API. Дополнительные сведения о `ApiResource` введите см. в разделе [ресурсов API](https://identityserver4.readthedocs.io/en/release/reference/api_resource.html#refapiresource) в документации по IdentityServer 4.

#### <a name="configuring-identity-resources"></a>Настройка идентификатора ресурсов

При настройке ресурсы identity `AddInMemoryIdentityResources` метод ожидает `IEnumerable<IdentityResource>` коллекции. Удостоверение ресурсы — это данные, такие как идентификатор пользователя, имя или адрес электронной почты. Каждый ресурс идентификаторов имеет уникальное имя и произвольных утверждений можно назначить типы, которые будут включены в токен идентификации пользователя. В следующем примере кода показан `GetResources` метод, который содержит эту коллекцию на eShopOnContainers обращение к приложению:

```csharp
public static IEnumerable<IdentityResource> GetResources()  
{  
    return new List<IdentityResource>  
    {  
        new IdentityResources.OpenId(),  
        new IdentityResources.Profile()  
    };  
}
```

Спецификация OpenID Connect задает некоторые [ресурсы standard identity](https://openid.net/specs/openid-connect-core-1_0.html#ScopeClaims). Минимальным требованием является обеспечивается при отправке уникальный идентификатор для пользователей. Это достигается путем предоставления доступа к `IdentityResources.OpenId` ресурс идентификаторов.

> [!NOTE]
> `IdentityResources` Класс поддерживает всех областей, определенных в спецификации OpenID Connect (openid, электронной почты, профиль, телефона и адрес).

IdentityServer также поддерживает определение ресурсов пользовательское удостоверение. Дополнительные сведения см. в разделе [определение ресурсов пользовательское удостоверение](https://identityserver4.readthedocs.io/en/release/topics/resources.html#defining-custom-identity-resources) в документации по IdentityServer. Дополнительные сведения о `IdentityResource` введите см. в разделе [ресурс идентификаторов](https://identityserver4.readthedocs.io/en/release/reference/identity_resource.html) в документации по IdentityServer 4.

#### <a name="configuring-clients"></a>Настройка клиентов

Клиенты — приложения, которые можно запрашивать IdentityServer маркеры. Как правило следующие параметры должны быть определены для каждого клиента как минимум:

-   Идентификатор клиента уникальным.
-   Разрешенные взаимодействия со службой маркеров (известный как тип предоставления).
-   Расположение, где отправляются токены доступом и удостоверениями (известный как URI перенаправления).
-   Список ресурсов, которые клиент может получить доступ к (известный как области).

При настройке клиентов, `AddInMemoryClients` метод ожидает `IEnumerable<Client>` коллекции. В следующем примере кода показана конфигурация для мобильного приложения eShopOnContainers в `GetClients` метод, который содержит эту коллекцию на eShopOnContainers обращение к приложению:

```csharp
public static IEnumerable<Client> GetClients(Dictionary<string,string> clientsUrl)
{
    return new List<Client>
    {
        ...
        new Client
        {
            ClientId = "xamarin",
            ClientName = "eShop Xamarin OpenId Client",
            AllowedGrantTypes = GrantTypes.Hybrid,
            ClientSecrets =
            {
                new Secret("secret".Sha256())
            },
            RedirectUris = { clientsUrl["Xamarin"] },
            RequireConsent = false,
            RequirePkce = true,
            PostLogoutRedirectUris = { $"{clientsUrl["Xamarin"]}/Account/Redirecting" },
            AllowedCorsOrigins = { "http://eshopxamarin" },
            AllowedScopes = new List<string>
            {
                IdentityServerConstants.StandardScopes.OpenId,
                IdentityServerConstants.StandardScopes.Profile,
                IdentityServerConstants.StandardScopes.OfflineAccess,
                "orders",
                "basket"
            },
            AllowOfflineAccess = true,
            AllowAccessTokensViaBrowser = true
        },
        ...
    };
}
```

Эта конфигурация указывает данные для следующих свойств:

-   `ClientId`: Уникальный идентификатор для клиента.
-   `ClientName`: Клиент отображаемое имя, которое используется для регистрации и разрешения экрана.
-   `AllowedGrantTypes`: Указывает, как клиент хочет взаимодействовать с IdentityServer. Дополнительные сведения см. [настройка потока проверки подлинности](#configuring_the_authentication_flow).
-   `ClientSecrets`: Указывает секретный учетные данные клиента, которые будут использоваться при запросе токены из конечной точки токена.
-   `RedirectUris`: Задает допустимый URL-адреса, для которого возвращаются токены или коды авторизации.
-   `RequireConsent`: Указывает, требуется ли на экран согласия.
-   `RequirePkce`: Указывает, должны ли клиенты, использующие код авторизации отправлять ключ проверки.
-   `PostLogoutRedirectUris`: Задает допустимый URL-адреса для перенаправления после выхода.
-   `AllowedCorsOrigins`: Указывает источник клиента, чтобы IdentityServer могут разрешить вызовы независимо от источника от начала координат.
-   `AllowedScopes`: Указывает ресурсы, которые клиент имеет доступ к. По умолчанию клиент не имеет доступа к ресурсам.
-   `AllowOfflineAccess`: Указывает, может ли клиент запрашивать токены обновления.

<a name="configuring_the_authentication_flow" />

#### <a name="configuring-the-authentication-flow"></a>Настройка потока проверки подлинности

Порядок проверки подлинности между клиентом и IdentityServer можно настроить, указав тип предоставления в `Client.AllowedGrantTypes` свойство. Спецификации OpenID Connect и OAuth 2.0 определить количество потоков проверки подлинности, включая:

-   Неявные. Этот поток оптимизирован для приложений на основе браузера и должен использоваться для пользователя только для проверки подлинности или запросы маркера проверки подлинности и доступа. Все маркеры передаются через браузер и поэтому дополнительные функции, как маркеры обновления не разрешены.
-   Код авторизации. Этот процесс предоставляет возможность извлекать токены на обратный канал, в отличие от переднего канала браузера также поддерживать проверку подлинности клиента.
-   Гибридный. Этот поток представляет собой сочетание неявный и типы разрешений кода авторизации. Токен идентификации передается по каналу браузера и содержит ответ знаком протокола и другие артефакты, такие как код авторизации. После успешной проверки ответа обратного канала должен использоваться для получения доступа и токен обновления.

> [!TIP]
> Используйте гибридные потока проверки подлинности. Поток проверки подлинности гибридного снижает число атак, применимые к каналу браузера и является рекомендуемый рабочий процесс для собственных приложений, которые нужно извлекать токены доступа (и возможно токенов обновления).

Дополнительные сведения о потоках проверки подлинности см. в разделе [типов](https://identityserver4.readthedocs.io/en/release/topics/grant_types.html) в документации по IdentityServer 4.

### <a name="performing-authentication"></a>Выполнение проверки подлинности

Для IdentityServer для выдачи токенов от имени пользователя пользователь должен Войдите на IdentityServer. Однако IdentityServer не предоставляет пользовательский интерфейс или базы данных для проверки подлинности. Таким образом в приложении eShopOnContainers ссылку для этой цели используется ASP.NET Core Identity.

EShopOnContainers мобильное приложение использует для проверки подлинности с IdentityServer гибридных поток проверки подлинности, как показано на рисунке 9-2.

![](authentication-and-authorization-images/sign-in.png "Высокоуровневый обзор процесса входа в систему")

**Рис. 9-2:** высокоуровневый обзор процесса входа в систему

Знак в запросе к `<base endpoint>:5105/connect/authorize`. После успешной проверки подлинности IdentityServer возвращает ответ проверки подлинности, содержащий код авторизации и токен удостоверения. Код авторизации и отправить их `<base endpoint>:5105/connect/token`, который отвечает с доступом, удостоверений и токенов обновления.

EShopOnContainers мобильное приложение знаки внешней IdentityServer, отправляя запрос на `<base endpoint>:5105/connect/endsession`, с добавлением дополнительных параметров. После выхода происходит IdentityServer отвечает, отправляя URI перенаправления выхода post мобильного приложения. Этот процесс показан на рис. 9-3.

![](authentication-and-authorization-images/sign-out.png "Высокоуровневый обзор процесс выхода")

**Рис. 9-3:** высокоуровневый обзор процесс выхода

В мобильном приложении eShopOnContainers осуществляется взаимодействие с IdentityServer `IdentityService` класса, который реализует `IIdentityService` интерфейса. Определяет этот интерфейс, реализующий класс должен предоставить `CreateAuthorizationRequest`, `CreateLogoutRequest`, и `GetTokenAsync` методы.

#### <a name="signing-in"></a>Войдите в

Когда пользователь нажимает кнопку **входа** кнопку `LoginView`, `SignInCommand` в `LoginViewModel` выполняется класс, который в свою очередь выполняет `SignInAsync` метод. Этот метод показан в следующем примере кода:

```csharp
private async Task SignInAsync()  
{  
    ...  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    IsLogin = true;  
    ...  
}
```

Этот метод вызывает `CreateAuthorizationRequest` метод `IdentityService` класса, как показано в следующем примере кода:

```csharp
public string CreateAuthorizationRequest()
{
    // Create URI to authorization endpoint
    var authorizeRequest = new AuthorizeRequest(GlobalSetting.Instance.IdentityEndpoint);

    // Dictionary with values for the authorize request
    var dic = new Dictionary<string, string>();
    dic.Add("client_id", GlobalSetting.Instance.ClientId);
    dic.Add("client_secret", GlobalSetting.Instance.ClientSecret); 
    dic.Add("response_type", "code id_token");
    dic.Add("scope", "openid profile basket orders locations marketing offline_access");
    dic.Add("redirect_uri", GlobalSetting.Instance.IdentityCallback);
    dic.Add("nonce", Guid.NewGuid().ToString("N"));
    dic.Add("code_challenge", CreateCodeChallenge());
    dic.Add("code_challenge_method", "S256");

    // Add CSRF token to protect against cross-site request forgery attacks.
    var currentCSRFToken = Guid.NewGuid().ToString("N");
    dic.Add("state", currentCSRFToken);

    var authorizeUri = authorizeRequest.Create(dic); 
    return authorizeUri;
}

```

Этот метод создает URI для элемента IdentityServer [конечная точка авторизации](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), с нужными параметрами. Конечная точка авторизации находится в `/connect/authorize` с порта 5105 базовый конечной точки в виде параметра пользователя. Дополнительные сведения о пользовательских параметрах см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> Реализация ключ подтверждения для расширения кода Exchange (PKCE) для OAuth уменьшается уязвимость eShopOnContainers мобильного приложения. Используется, если оно перехватывается в PKCE защищает код авторизации. Это достигается путем создания секретный verifier, хэш-значение которого передается в запросе на авторизацию, клиент и представленное нехешированной при активации код авторизации. Дополнительные сведения о PKCE см. в разделе [ключ подтверждения для обмена кода открытые клиентами OAuth](https://tools.ietf.org/html/rfc7636) на веб-сайте Internet Engineering Task Force.

Полученный URI хранится в `LoginUrl` свойство `LoginViewModel` класса. Когда `IsLogin` свойство становится `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) в `LoginView` становится видимым. `WebView` Привязки данных его [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) свойства `LoginUrl` свойство `LoginViewModel` класса и поэтому IdentityServer отправляет запрос входа при `LoginUrl` свойству Конечная точка авторизации IdentityServer элемента. Когда IdentityServer получает этот запрос, и пользователь не прошел проверку подлинности, `WebView` будет перенаправлен на страницу настроенного входа, как показано на рисунке 9-4.

![](authentication-and-authorization-images/login.png "Страницы входа, отображаемого элементом WebView")

**Рис. 9-4:** страницы входа, отображаемого элементом WebView

После завершения входа [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) будут перенаправляться в возвращаемый URI. Это `WebView` навигации приведет к `NavigateAsync` метод `LoginViewModel` класса должно быть выполнено, как показано в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    var authResponse = new AuthorizeResponse(url);  
    if (!string.IsNullOrWhiteSpace(authResponse.Code))  
    {  
        var userToken = await _identityService.GetTokenAsync(authResponse.Code);  
        string accessToken = userToken.AccessToken;  

        if (!string.IsNullOrWhiteSpace(accessToken))  
        {  
            Settings.AuthAccessToken = accessToken;  
            Settings.AuthIdToken = authResponse.IdentityToken;  

            await NavigationService.NavigateToAsync<MainViewModel>();  
            await NavigationService.RemoveLastFromBackStackAsync();  
        }  
    }  
    ...  
}
```

Этот метод выполняет синтаксический анализ, содержащийся в URI, возвращаемый ответ проверки подлинности и при условии, что код авторизации присутствует, он отправляет запрос IdentityServer [конечная точка маркера](https://identityserver4.readthedocs.io/en/release/endpoints/token.html), передав код авторизации PKCE секретный проверки и другие необходимые параметры. Конечная точка маркера — в `/connect/token` с порта 5105 базовый конечной точки в виде параметра пользователя. Дополнительные сведения о пользовательских параметрах см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

>💡 **Совет**: проверка возвращают идентификаторы URI. Несмотря на то, что eShopOnContainers мобильное приложение не проверяет возвращаемое URI, рекомендуется проверить, что возвращаемый URI относится в известном расположении для предотвращения атак перенаправления open.

Если конечная точка маркера получает код авторизации и проверки секретный PKCE, то в ответ маркера доступа, маркер и маркер обновления. Маркер доступа (который предоставляет доступ к ресурсам API) и маркер затем сохраняются в виде параметров приложения и выполнять навигацию по страницам. Таким образом, общий эффект в мобильном приложении eShopOnContainers это: условии, что пользователи получают возможность успешно пройти проверку подлинности с IdentityServer, переходе к `MainView` страница, которая является [ `TabbedPage` ](https://developer.xamarin.com/api/type/Xamarin.Forms.TabbedPage/) Отображает, `CatalogView` как его выбранной вкладки.

Сведения о навигации по страницам см. в разделе [навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md). Сведения о том, как [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) навигации вызывает метод модели представления для выполнения см. в разделе [вызов навигации с помощью поведений](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Сведения о параметрах приложений см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers также позволяет макетов вход при настройке приложения для использования служб макетов в `SettingsView`. В этом режиме приложения не взаимодействуют с IdentityServer, вместо этого, что позволяет пользователю войти, используя учетные данные.

#### <a name="signing-out"></a>Подписи out

Когда пользователь нажимает кнопку **Выход** кнопку в `ProfileView`, `LogoutCommand` в `ProfileViewModel` выполняется класс, который в свою очередь выполняет `LogoutAsync` метод. Этот метод выполняет навигацию по страницам для `LoginView` страницы, передав `LogoutParameter` экземпляр значение `true` как параметр. Дополнительные сведения о передаче параметров во время перемещения по страницам см. в разделе [передача параметров во время навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md#passing_parameters_during_navigation).

При создании представления и осуществлен переход, `InitializeAsync` модели представления связанного представления метода, затем выполняет `Logout` метод `LoginViewModel` класса, как показано в следующем примере кода:

```csharp
private void Logout()  
{  
    var authIdToken = Settings.AuthIdToken;  
    var logoutRequest = _identityService.CreateLogoutRequest(authIdToken);  

    if (!string.IsNullOrEmpty(logoutRequest))  
    {  
        // Logout  
        LoginUrl = logoutRequest;  
    }  
    ...  
}
```

Этот метод вызывает `CreateLogoutRequest` метод `IdentityService` класса, передав токен идентификации получить из параметров приложения как параметр. Дополнительные сведения о параметрах приложений см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md). В следующем примере кода показан `CreateLogoutRequest` метод:

```csharp
public string CreateLogoutRequest(string token)  
{  
    ...  
    return string.Format("{0}?id_token_hint={1}&post_logout_redirect_uri={2}",   
        GlobalSetting.Instance.LogoutEndpoint,  
        token,  
        GlobalSetting.Instance.LogoutCallback);  
}
```

Этот метод создает URI для элемента IdentityServer [окончания сеанса конечной точки](https://identityserver4.readthedocs.io/en/release/endpoints/endsession.html#refendsession), с нужными параметрами. Конечная точка окончания сеанса находится в `/connect/endsession` с порта 5105 базовый конечной точки в виде параметра пользователя. Дополнительные сведения о пользовательских параметрах см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

Полученный URI хранится в `LoginUrl` свойство `LoginViewModel` класса. Хотя `IsLogin` свойство `true`, [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) в `LoginView` является видимым. `WebView` Привязки данных его [ `Source` ](https://developer.xamarin.com/api/property/Xamarin.Forms.WebView.Source/) свойства `LoginUrl` свойство `LoginViewModel` класса и поэтому запрашивает выхода IdentityServer при `LoginUrl` свойству Конечная точка IdentityServer элемента окончания сеанса. Когда IdentityServer получает этот запрос, при условии, что пользователь, выполнивший вход, выполняется выход. Объект cookie, управляемых по промежуточного слоя проверки подлинности файла cookie из ASP.NET Core отслеживаются проверки подлинности. Таким образом выходе из IdentityServer удаляет файл cookie проверки подлинности и отправляет перенаправления после выхода из системы, URI обратно клиенту.

В мобильном приложении [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) будет перенаправлен на URI перенаправления выхода post. Это `WebView` навигации приведет к `NavigateAsync` метод `LoginViewModel` класса должно быть выполнено, как показано в следующем примере кода:

```csharp
private async Task NavigateAsync(string url)  
{  
    ...  
    Settings.AuthAccessToken = string.Empty;  
    Settings.AuthIdToken = string.Empty;  
    IsLogin = false;  
    LoginUrl = _identityService.CreateAuthorizationRequest();  
    ...  
}
```

Этот метод удаляет маркер идентификатора и токен доступа из параметров приложения и задает `IsLogin` свойства `false`, чего [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) на `LoginView` страницу, чтобы стать невидимым . Наконец `LoginUrl` свойству IdentityServer URI из [конечная точка авторизации](https://identityserver4.readthedocs.io/en/release/endpoints/authorize.html), с нужными параметрами, в процессе подготовки в следующий раз, когда пользователь инициирует вход в.

Сведения о навигации по страницам см. в разделе [навигации](~/xamarin-forms/enterprise-application-patterns/navigation.md). Сведения о том, как [ `WebView` ](https://developer.xamarin.com/api/type/Xamarin.Forms.WebView/) навигации вызывает метод модели представления для выполнения см. в разделе [вызов навигации с помощью поведений](~/xamarin-forms/enterprise-application-patterns/navigation.md#invoking_navigation_using_behaviors). Сведения о параметрах приложений см. в разделе [управления конфигурацией](~/xamarin-forms/enterprise-application-patterns/configuration-management.md).

> [!NOTE]
> EShopOnContainers также позволяет макетирование выхода, когда приложение настроено для использования служб макетов в SettingsView. В этом режиме приложение не взаимодействовать с IdentityServer и вместо этого очищает все хранимые токены из параметров приложения.

<a name="authorization" />

## <a name="authorization"></a>Авторизация

После проверки подлинности ASP.NET Core web API-интерфейсы часто требуется для разрешения доступа, которая позволяет службе с API доступны для некоторых пользователей, прошедших проверку подлинности, но не для всех.

Ограничение доступа к маршрут ASP.NET MVC основных компонентов может осуществляться путем применения атрибута авторизовать контроллер или действие, которое ограничивает доступ к контроллеру или действие, прошедшим проверку подлинности, как показано в следующем примере кода:

```csharp
[Authorize]  
public class BasketController : Controller  
{  
    ...  
}
```

Если неавторизованный пользователь пытается получить доступ к контроллеру или действию, который отмечен атрибутом `Authorize` атрибут, платформа MVC возвращает код состояния 401 (несанкционированный) HTTP.

> [!NOTE]
> Параметры могут задаваться на `Authorize` атрибута для ограничения API для конкретных пользователей. Дополнительные сведения см. в разделе [авторизации](/aspnet/core/security/authorization/introduction/).

IdentityServer можно интегрировать в рабочий процесс авторизации, чтобы токены доступа обеспечивает контроль авторизации. Этот подход показан на рисунке 9-5.

![](authentication-and-authorization-images/authorization.png "Авторизации в маркер доступа")

**Рис. 9-5:** авторизации в маркер доступа

Мобильное приложение eShopOnContainers взаимодействует с микрослужбу удостоверений и запрашивает маркер доступа в рамках процесса проверки подлинности. API-интерфейсы, предоставляемые микрослужбами упорядочение и Корзина как часть запросов доступа затем передается маркер доступа. Маркеры доступа содержат сведения о клиенте и пользователь. API-интерфейсы, затем использовать эту информацию для авторизации доступа к их данным. Сведения о настройке IdentityServer для защиты API-интерфейсов см. в разделе [Настройка ресурсов API](#configuring-api-resources).

### <a name="configuring-identityserver-to-perform-authorization"></a>Настройка IdentityServer для выполнения авторизации

Для выполнения авторизации с помощью IdentityServer, необходимо добавить его авторизации по промежуточного слоя в конвейере HTTP веб-приложения. По промежуточного слоя будет добавлен в `ConfigureAuth` метод веб-приложения `Startup` класс, который запускается из `Configure` метода и демонстрируется в следующем примере кода из приложения eShopOnContainers ссылку:

```csharp
protected virtual void ConfigureAuth(IApplicationBuilder app)  
{  
    var identityUrl = Configuration.GetValue<string>("IdentityUrl");  
    app.UseIdentityServerAuthentication(new IdentityServerAuthenticationOptions  
    {  
        Authority = identityUrl.ToString(),  
        ScopeName = "basket",  
        RequireHttpsMetadata = false  
    });  
} 
```

Этот метод гарантирует, что API может осуществляться только с действительный маркер доступа. По промежуточного слоя проверяет входящий токен, чтобы убедиться, что оно отправлено с надежным издателем и проверяет, что маркер является допустимым для использования с API, который получит его. Таким образом перейдя к контроллеру порядок сортировки или корзины вернет 401 (несанкционированный) код состояния HTTP, указывающее, что требуется маркер доступа.

> [!NOTE]
> По промежуточного слоя IdentityServer авторизации необходимо добавить в конвейер запросов HTTP для веб-приложения перед добавлением MVC с `app.UseMvc()` или `app.UseMvcWithDefaultRoute()`.

### <a name="making-access-requests-to-apis"></a>Создание запросов доступа к API-интерфейсы

При выполнении запросов к микрослужбами упорядочение и Корзина, доступ маркера, полученные из IdentityServer во время процесса проверки подлинности, должен быть включен в запрос, как показано в следующем примере кода:

```csharp
var authToken = Settings.AuthAccessToken;  
Order = await _ordersService.GetOrderAsync(Convert.ToInt32(order.OrderNumber), authToken);
```

Токен доступа хранится как параметр приложения и получить из хранилища платформой и включены в вызове `GetOrderAsync` метод `OrderService` класса.

Аналогично маркер доступа должен быть включен при отправке данных IdentityServer защищенным API, как показано в следующем примере кода:

```csharp
var authToken = Settings.AuthAccessToken;  
await _basketService.UpdateBasketAsync(new CustomerBasket  
{  
    BuyerId = userInfo.UserId,   
    Items = BasketItems.ToList()  
}, authToken);
```

Маркер доступа извлекается из хранилища конкретную платформу и включены в вызове `UpdateBasketAsync` метод `BasketService` класса.

`RequestProvider` Класс, в мобильном приложении eShopOnContainers использует `HttpClient` класс делать запросы к RESTful API-интерфейсы, предоставляемые приложением eShopOnContainers ссылки. При запросе внесения для упорядочения и Корзина API, которые требуют наличия авторизации, действительный маркер доступа должен быть включен в запрос. Это достигается путем добавления маркер доступа к заголовкам `HttpClient` экземпляра, как показано в следующем примере кода:

```csharp
httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", token);
```

`DefaultRequestHeaders` Свойство `HttpClient` класс предоставляет заголовки, которые отправляются с каждым запросом и добавляется новый токен доступа `Authorization` префиксом со строкой заголовка `Bearer`. Если запрос отправлен API RESTful, значение `Authorization` извлекаются и проверить, чтобы убедиться, что отправляются от доверенного издателя и используется для определения, имеет ли пользователь разрешение на вызов API-Интерфейс, получает его заголовок.

Дополнительные сведения о как мобильное приложение eShopOnContainers позволяет веб-запросов см. в разделе [доступ к удаленным данным](~/xamarin-forms/enterprise-application-patterns/accessing-remote-data.md).

## <a name="summary"></a>Сводка

Существует множество подходов к интеграции проверки подлинности и авторизации в приложении Xamarin.Forms, который обменивается данными с веб-приложение ASP.NET MVC. Мобильное приложение eShopOnContainers выполняет проверку подлинности и авторизации с помощью микрослужбу контейнерного удостоверений, использующий IdentityServer 4. IdentityServer является открытой OpenID Connect и OAuth 2.0 платформу ASP.NET Core, которое интегрируется с ASP.NET Identity Core для выполнения проверки подлинности маркера носителя.

Мобильное приложение запрашивает IdentityServer, маркеры безопасности для проверки подлинности пользователя или для доступа к ресурсу. При доступе к ресурсу, маркер доступа должны включаться в запрос API-интерфейсов, требующие авторизации. По промежуточного слоя IdentityServer проверяет входящие токены доступа, убедитесь, что они передаются от доверенного издателя и что они являются допустимыми для использования с помощью API, который их получает.


## <a name="related-links"></a>Связанные ссылки

- [Загрузить электронную книгу (2 МБ в формате PDF)](https://aka.ms/xamarinpatternsebook)
- [eShopOnContainers (GitHub) (пример)](https://github.com/dotnet-architecture/eShopOnContainers)
