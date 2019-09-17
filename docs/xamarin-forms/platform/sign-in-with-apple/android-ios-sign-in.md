---
title: Использование входа с Apple для Xamarin. Forms
description: Узнайте, как реализовать вход с помощью Apple в мобильных приложениях Xamarin. Forms.
ms.prod: xamarin
ms.assetid: 2E47E7F2-93D4-4CA3-9E66-247466D25E4D
ms.technology: xamarin-forms
author: davidortinau
ms.author: daortin
ms.date: 09/10/2019
ms.openlocfilehash: 088ba2d82b7bfbe770a41ce19208a8e95d477edb
ms.sourcegitcommit: cf56d2bae34dc0f8e94c2d3d28d5f460d59807bf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/13/2019
ms.locfileid: "70986132"
---
# <a name="use-sign-in-with-apple-in-xamarinforms"></a>Использование входа с Apple в Xamarin. Forms

![Сейчас этот API находится в предварительной версии](~/media/shared/preview.png)

[![Загрузить образец](~/media/shared/download.png) загрузить пример](https://github.com/Redth/Xamarin.AppleSignIn.Sample)

Войдите в систему с помощью Apple для всех новых приложений в iOS 13, использующих сторонние службы проверки подлинности. Сведения о реализации между iOS и Android сильно отличаются. В этом руководстве описано, как это можно сделать сегодня в Xamarin. Forms.

В этом руководство и примере для управления входом с помощью Apple используются определенные службы платформы.

- Использование Android с помощью универсальной веб-службы. речь идет о функциях Azure с OpenID Connect/OpenAuth
- iOS использует собственный API для проверки подлинности в iOS 13 и возвращается к универсальной веб-службе для iOS 12 и ниже.

## <a name="a-sample-apple-sign-in-flow"></a>Пример последовательности входа Apple

В этом примере предлагается реализация упрямого для входа Apple для работы в приложении Xamarin. Forms.

Для работы с потоком проверки подлинности мы используем две функции Azure:

1. `applesignin_auth`— Создает URL-адрес авторизации для входа Apple и перенаправляет его.  Это делается на стороне сервера, а не в мобильном приложении, поэтому мы можем кэшировать `state` и проверять его, когда серверы Apple отправляют обратный вызов.
2. `applesignin_callback`— Обрабатывает обратный вызов после Apple и безопасно обменивается кодом авторизации для маркера доступа и маркера идентификации.  Наконец, он перенаправляется обратно в схему универсального кода ресурса (URI) приложения, передавая токены в фрагменте URL-адреса.

Мобильное приложение регистрирует себя для `xamarinformsapplesignin://` `applesignin_callback` работы с настраиваемой схемой универсального кода ресурса (URI), которую мы выбрали (в данном случае), чтобы функция могла передавать маркеры обратно в нее.

Когда пользователь запускает проверку подлинности, выполняются следующие действия.

1. Мобильное приложение создает `nonce` значение и `state` передает `applesignin_auth` его в функцию Azure.
2. Функция Azure создает URL-адрес авторизации входа Apple (с помощью предоставленного `state` и `nonce`) и перенаправляет в него браузер мобильного приложения. `applesignin_auth`
3. Пользователь вводит свои учетные данные безопасно на странице авторизации входа Apple, размещенной на серверах Apple.
4. По завершении процесса входа Apple на серверах Apple компания Apple перенаправляет его в, `redirect_uri` который будет `applesignin_callback` использоваться в качестве функции Azure.
5. Запрос от Apple, отправленный `applesignin_callback` в функцию, проверяется, чтобы убедиться, что возвращено правильное `state` значение, и что утверждения маркера идентификатора допустимы.
6. `code`Функция Azure обменивается данными, опубликованными в ней компанией Apple, для маркера доступа, маркера обновления и маркера идентификации (который содержит утверждения об идентификаторе пользователя, имени и электронной почте). `applesignin_callback`
7. Функция Azure наконец перенаправляет обратно в схему URI приложения (`xamarinformsapplesignin://`), добавляя фрагмент URI с токенами (например, `xamarinformsapplesignin://#access_token=...&refresh_token=...&id_token=...`). `applesignin_callback`
8. Мобильное приложение анализирует фрагмент кода URI в `AppleAccount` и проверяет `nonce` полученное утверждение на соответствие `nonce` созданному в начале потока.
9. Теперь мобильное приложение прошло проверку подлинности.

## <a name="azure-functions"></a>Функции Azure

В этом примере используются функции Azure. Кроме того, контроллер ASP.NET Core или аналогичное решение веб-сервера может обеспечить одну и ту же функциональность.

### <a name="configuration"></a>Конфигурация

При использовании функций Azure необходимо настроить несколько параметров приложения:

- `APPLE_SIGNIN_KEY_ID`— Это ваш `KeyId` ранее.
- `APPLE_SIGNIN_TEAM_ID`— Обычно это _идентификатор команды_ , найденный в [профиле членства](https://developer.apple.com/account/#/membership)
- `APPLE_SIGNIN_SERVER_ID`: `ServerId` Это из предыдущего.  Это *не* _идентификатор набора_приложений, а *идентификатор* созданного *идентификатора служб* .
- `APPLE_SIGNIN_APP_CALLBACK_URI`— Это настраиваемая схема URI, которую вы хотите перенаправить обратно в приложение.  В этом примере `xamarinformsapplesignin://` используется.
- `APPLE_SIGNIN_REDIRECT_URI`— *URL-адрес перенаправления* , который вы настраиваете при создании *идентификатора служб* в разделе "Настройка *входа Apple* ".  Для тестирования он может выглядеть примерно так:`http://local.test:7071/api/applesignin_callback`
- `APPLE_SIGNIN_P8_KEY`— Текстовое содержимое `.p8` файла со `\n` всеми удаленными символами новой строки, так что это одна длинная строка

### <a name="security-considerations"></a>Замечания по безопасности

**Никогда не** Храните ключ P8 внутри кода приложения. Код приложения прост в скачивании и разборке. 

Также считается неплохой практикой использования `WebView` для размещения потока проверки подлинности и перехвата событий навигации по URL-адресу для получения кода авторизации. Сейчас в настоящее время нет полностью безопасного способа для управления входом с помощью Apple на устройствах, не iOS13 +, без размещения кода на сервере для управления обменом маркерами. Мы рекомендуем размещать код создания URL-адреса авторизации на сервере, чтобы можно было кэшировать состояние и проверять его, когда Apple отправляет ответ на ваш сервер.

## <a name="a-cross-platform-sign-in-service"></a>Кросс-платформенный вход в службу

С помощью Xamarin. Forms DependencyService можно создавать отдельные службы проверки подлинности, использующие службы платформы в iOS, а также универсальную веб-службу для Android и другие платформы, не поддерживающие iOS, на основе общего интерфейса.

```csharp
public interface IAppleSignInService
{
    bool Callback(string url);

    Task<AppleAccount> SignInAsync();
}
```

В iOS используются собственные API:

```csharp
public class AppleSignInServiceiOS : IAppleSignInService
{
#if __IOS__13
    AuthManager authManager;
#endif

    bool Is13 => UIDevice.CurrentDevice.CheckSystemVersion(13, 0);
    WebAppleSignInService webSignInService;

    public AppleSignInServiceiOS()
    {
        if (!Is13)
            webSignInService = new WebAppleSignInService();
    }

    public async Task<AppleAccount> SignInAsync()
    {
        // Fallback to web for older iOS versions
        if (!Is13)
            return await webSignInService.SignInAsync();

        AppleAccount appleAccount = default;

#if __IOS__13
        var provider = new ASAuthorizationAppleIdProvider();
        var req = provider.CreateRequest();

        authManager = new AuthManager(UIApplication.SharedApplication.KeyWindow);

        req.RequestedScopes = new[] { ASAuthorizationScope.FullName, ASAuthorizationScope.Email };
        var controller = new ASAuthorizationController(new[] { req });

        controller.Delegate = authManager;
        controller.PresentationContextProvider = authManager;

        controller.PerformRequests();

        var creds = await authManager.Credentials;

        if (creds == null)
            return null;

        appleAccount = new AppleAccount();
        appleAccount.IdToken = JwtToken.Decode(new NSString(creds.IdentityToken, NSStringEncoding.UTF8).ToString());
        appleAccount.Email = creds.Email;
        appleAccount.UserId = creds.User;
        appleAccount.Name = NSPersonNameComponentsFormatter.GetLocalizedString(creds.FullName, NSPersonNameComponentsFormatterStyle.Default, NSPersonNameComponentsFormatterOptions.Phonetic);
        appleAccount.RealUserStatus = creds.RealUserStatus.ToString();
#endif

        return appleAccount;
    }

    public bool Callback(string url) => true;
}

#if __IOS__13
class AuthManager : NSObject, IASAuthorizationControllerDelegate, IASAuthorizationControllerPresentationContextProviding
{
    public Task<ASAuthorizationAppleIdCredential> Credentials
        => tcsCredential?.Task;

    TaskCompletionSource<ASAuthorizationAppleIdCredential> tcsCredential;

    UIWindow presentingAnchor;

    public AuthManager(UIWindow presentingWindow)
    {
        tcsCredential = new TaskCompletionSource<ASAuthorizationAppleIdCredential>();
        presentingAnchor = presentingWindow;
    }

    public UIWindow GetPresentationAnchor(ASAuthorizationController controller)
        => presentingAnchor;

    [Export("authorizationController:didCompleteWithAuthorization:")]
    public void DidComplete(ASAuthorizationController controller, ASAuthorization authorization)
    {
        var creds = authorization.GetCredential<ASAuthorizationAppleIdCredential>();
        tcsCredential?.TrySetResult(creds);
    }

    [Export("authorizationController:didCompleteWithError:")]
    public void DidComplete(ASAuthorizationController controller, NSError error)
        => tcsCredential?.TrySetException(new Exception(error.LocalizedDescription));
}
#endif
```

Флаг `__IOS__13` компиляции используется для обеспечения поддержки iOS 13, а также устаревших версий, которые переводятся в универсальную веб-службу.

В Android используется универсальная веб-служба с функциями Azure:

```csharp
public class WebAppleSignInService : IAppleSignInService
{
    // IMPORTANT: This is what you register each native platform's url handler to be
    public const string CallbackUriScheme = "xamarinformsapplesignin";
    public const string InitialAuthUrl = "http://local.test:7071/api/applesignin_auth";

    string currentState;
    string currentNonce;

    TaskCompletionSource<AppleAccount> tcsAccount = null;

    public bool Callback(string url)
    {
        // Only handle the url with our callback uri scheme
        if (!url.StartsWith(CallbackUriScheme + "://"))
            return false;

        // Ensure we have a task waiting
        if (tcsAccount != null && !tcsAccount.Task.IsCompleted)
        {
            try
            {
                // Parse the account from the url the app opened with
                var account = AppleAccount.FromUrl(url);

                // IMPORTANT: Validate the nonce returned is the same as our originating request!!
                if (!account.IdToken.Nonce.Equals(currentNonce))
                    tcsAccount.TrySetException(new InvalidOperationException("Invalid or non-matching nonce returned"));

                // Set our account result
                tcsAccount.TrySetResult(account);
            }
            catch (Exception ex)
            {
                tcsAccount.TrySetException(ex);
            }
        }

        tcsAccount.TrySetResult(null);
        return false;
    }

    public async Task<AppleAccount> SignInAsync()
    {
        tcsAccount = new TaskCompletionSource<AppleAccount>();

        // Generate state and nonce which the server will use to initial the auth
        // with Apple.  The nonce should flow all the way back to us when our function
        // redirects to our app
        currentState = Util.GenerateState();
        currentNonce = Util.GenerateNonce();

        // Start the auth request on our function (which will redirect to apple)
        // inside a browser (either SFSafariViewController, Chrome Custom Tabs, or native browser)
        await Xamarin.Essentials.Browser.OpenAsync($"{InitialAuthUrl}?&state={currentState}&nonce={currentNonce}",
            Xamarin.Essentials.BrowserLaunchMode.SystemPreferred);

        return await tcsAccount.Task;
    }
}```

## Summary

This article described the steps necessary to setup Sign In with Apple for use in your Xamarin.Forms applications.

## Related links

- [XamarinFormsAppleSignIn (Sample)](https://github.com/Redth/Xamarin.AppleSignIn.Sample)
- [Sign In with Apple Guidelines](https://developer.apple.com/design/human-interface-guidelines/sign-in-with-apple/overview/)