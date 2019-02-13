---
title: 使用沒有 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 說明的使用沒有 ASP.NET Core 身分識別的 cookie 驗證
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: f05e5b83359ec1739115293e092eaed0c811c046
ms.sourcegitcommit: 3c2ba9a0d833d2a096d9d800ba67a1a7f9491af0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/07/2019
ms.locfileid: "55854376"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>使用沒有 ASP.NET Core 身分識別的 cookie 驗證

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

當您在先前的驗證主題中所見[ASP.NET Core Identity](xref:security/authentication/identity)是完整且功能完整的驗證提供者來建立及維護的登入。 不過，您可能要使用您自己的自訂驗證邏輯使用有時 cookie 型驗證。 您可以使用以 cookie 為基礎的驗證作為獨立驗證提供者沒有 ASP.NET Core 身分識別。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

基於示範目的，範例應用程式中，假設使用者林麗莉 Rodriguez 的使用者帳戶會是硬式編碼到應用程式。 使用電子郵件使用者名稱 」maria.rodriguez@contoso.com」 和任何登入使用者的密碼。 使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。 在真實世界範例中，您會驗證使用者，對資料庫。

如需移轉以 cookie 為基礎的驗證，從 ASP.NET Core 1.x 至 2.0，請參閱[移轉驗證和 ASP.NET Core 2.0 主題 （Cookie 型驗證） 的身分識別](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)。

若要使用 ASP.NET Core 身分識別，請參閱[身分識別簡介](xref:security/authentication/identity)主題。

## <a name="configuration"></a>組態

::: moniker range=">= aspnetcore-2.0"

如果應用程式不會使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，建立的專案檔中的套件參考[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)封裝 (版本 2.1.0 或更新版本）。

在 `ConfigureServices`方法，建立驗證中介軟體服務`AddAuthentication`和`AddCookie`方法：

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` 傳遞至`AddAuthentication`設定應用程式的預設驗證配置。 `AuthenticationScheme` 當有多個執行個體的 cookie 驗證，而且您想要時非常有用[特定的結構描述的授權](xref:security/authorization/limitingidentitybyscheme)。 設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。 您可以提供區分配置任何字串值。

應用程式的驗證配置與不同應用程式的 cookie 驗證配置。 當 cookie 驗證配置不提供給<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>，它會使用[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) ("Cookie")。

在 `Configure`方法，請使用`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。 呼叫`UseAuthentication`方法之前呼叫`UseMvcWithDefaultRoute`或`UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**AddCookie Options**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)類別用來設定驗證提供者選項。

| 選項 | 描述 |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | 提供要找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ForbidAsync`。 預設值為 `/Account/AccessDenied`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | 若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證服務所建立的任何宣告的屬性。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | 其中提供 cookie 的網域名稱。 根據預設，這是要求的主機名稱。 瀏覽器只在要求中，cookie 傳送相符的主機名稱。 您可能想要調整該項目可在網域中擁有任何主機可用的 cookie。 例如，若要設定 cookie 網域`.contoso.com`並提供給`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | 旗標，指出是否 cookie 應該僅供伺服器存取。 此值變更為`false`允許用戶端指令碼，以存取 cookie，可能會用來開啟您的應用程式應有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。 預設值為 `true`。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | 設定 cookie 的名稱。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | 用來隔離在相同的主機名稱上執行的應用程式。 如果您在執行的應用程式`/app1`想要將 cookie 限制為該應用程式，請設定`CookiePath`屬性設`/app1`。 如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | 表示瀏覽器是否應該允許 cookie 附加至相同站台要求 (`SameSiteMode.Strict`) 或跨網站要求使用安全的 HTTP 方法和相同站台要求 (`SameSiteMode.Lax`)。 當設定為`SameSiteMode.None`，未設定的 cookie 標頭值。 請注意， [Cookie 原則中介軟體](#cookie-policy-middleware)可能會覆寫您所提供的值。 若要支援 OAuth 驗證，預設值是`SameSiteMode.Lax`。 如需詳細資訊，請參閱 < [SameSite cookie 原則因為損毀的 OAuth 驗證](https://github.com/aspnet/Security/issues/1231)。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | 旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。 預設值為 `CookieSecurePolicy.SameAsRequest`。 |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | 設定組`DataProtectionProvider`用來建立預設`TicketDataFormat`。 如果`TicketDataFormat`屬性設定，`DataProtectionProvider`選項不會使用。 如果未提供，則會使用應用程式的預設資料保護提供者。 |
| [事件](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | 處理常式會呼叫提供者，讓應用程式控制項的特定處理時間點上的方法。 如果`Events`不提供的預設執行個體提供呼叫方法時，不做任何動作。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | 若要取得做為服務類型`Events`而非屬性的執行個體。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan`之後儲存在 cookie 內的驗證票證已過期。 `ExpireTimeSpan` 會加入至目前的時間來建立票證的到期時間。 `ExpiredTimeSpan`值一律會移到加密的 AuthTicket 由伺服器進行驗證。 它也可能會進入[Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)標頭，但是只有`IsPersistent`設定。 若要設定`IsPersistent`要`true`，設定[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)傳遞至`SignInAsync`。 預設值`ExpireTimeSpan`為 14 天。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | 提供要找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ChallengeAsync`。 產生 401 的目前 URL 加入至`LoginPath`所命名的查詢字串參數為`ReturnUrlParameter`。 一次要求`LoginPath`授與新登入的身分識別，`ReturnUrlParameter`值用來將瀏覽器重新導向回到原始的未經授權的狀態程式碼的 URL。 預設值為 `/Account/Login`。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | 如果`LogoutPath`提供，以處理常式中，則該路徑的要求重新導向的值為基礎`ReturnUrlParameter`。 預設值為 `/Account/Logout`。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | 判斷由 302 已找到 （重新導向 URL） 回應的處理常式會附加查詢字串參數的名稱。 `ReturnUrlParameter` 在要求抵達時，會使用`LoginPath`或`LogoutPath`執行登入或登出動作後，瀏覽器傳回至原始 URL。 預設值為 `ReturnUrl`。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | 選擇性的容器，用來儲存跨要求的身分識別。 使用時，只有工作階段識別碼會傳送至用戶端。 `SessionStore` 可用來降低大型的身分識別的潛在問題。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | 旗標，指出是否要以動態方式發出新的更新的到期時間的 cookie。 這可能會發生的任何位置的目前 cookie 到期期間超過 50%過期的要求。 新的到期日往前移動目前的日期加上`ExpireTimespan`。 [絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。 絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。 預設值為 `true`。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat`用來保護且取消保護身分識別和其他屬性儲存在 cookie 值中。 如果未提供，`TicketDataFormat`會使用建立[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)。 |
| [驗證](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | 檢查的選項都是有效的方法。 |

設定`CookieAuthenticationOptions`中的驗證服務組態中`ConfigureServices`方法：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

ASP.NET Core 1.x 使用 cookie[中介軟體](xref:fundamentals/middleware/index)，序列化的加密 cookie 的使用者主體。 在後續的要求，對 cookie 進行驗證，以及主體會重新建立並指派給`HttpContext.User`屬性。

安裝[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)在專案中的 NuGet 套件。 此套件包含的 cookie 中介軟體。

使用`UseCookieAuthentication`方法中的`Configure`方法，在您*Startup.cs*檔案之後，再`UseMvc`或`UseMvcWithDefaultRoute`:

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions()
{
    AccessDeniedPath = "/Account/Forbidden/",
    AuthenticationScheme = CookieAuthenticationDefaults.AuthenticationScheme,
    AutomaticAuthenticate = true,
    AutomaticChallenge = true,
    LoginPath = "/Account/Unauthorized/"
});
```

**CookieAuthenticationOptions 選項**

[CookieAuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.CookieAuthenticationOptions?view=aspnetcore-1.1)類別用來設定驗證提供者選項。

| 選項 | 描述 |
| ------ | ----------- |
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | 設定驗證配置。 `AuthenticationScheme` 當有多個執行個體的驗證，而且您想要時非常有用[特定的結構描述的授權](xref:security/authorization/limitingidentitybyscheme)。 設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。 您可以提供區分配置任何字串值。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | 設定值，表示 cookie 驗證，應該在每個要求上執行，並且在嘗試驗證，並重新建構它建立任何序列化的主體。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | 如果為 true，則驗證中介軟體會處理自動的挑戰。 如果為 false，驗證中介軟體只會改變明確指出時的回應`AuthenticationScheme`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | 若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證中介軟體所建立的任何宣告的屬性。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | 其中提供 cookie 的網域名稱。 根據預設，這是要求的主機名稱。 瀏覽器，只是要比對的主機名稱的 cookie。 您可能想要調整該項目可在網域中擁有任何主機可用的 cookie。 例如，若要設定 cookie 網域`.contoso.com`並提供給`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | 旗標，指出是否 cookie 應該僅供伺服器存取。 此值變更為`false`允許用戶端指令碼，以存取 cookie，可能會用來開啟您的應用程式應有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。 預設值為 `true`。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | 用來隔離在相同的主機名稱上執行的應用程式。 如果您在執行的應用程式`/app1`想要將 cookie 限制為該應用程式，請設定`CookiePath`屬性設`/app1`。 如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | 旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)，HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。 預設值為 `CookieSecurePolicy.SameAsRequest`。 |
| [描述](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | 其他資訊提供給應用程式的驗證類型的詳細資訊。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan`之後驗證票證已過期。 它會新增至目前的時間來建立票證的到期時間。 若要使用`ExpireTimeSpan`，您必須設定`IsPersistent`要`true`中`AuthenticationProperties`傳遞至`SignInAsync`。 預設值為 14 天。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | 旗標，指出是否 cookie 的到期日重設時超過一半的`ExpireTimeSpan`經過這段間隔。 新的 exipiration 時間往前移動目前的日期加上`ExpireTimespan`。 [絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。 絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。 預設值為 `true`。 |

設定`CookieAuthenticationOptions`Cookie 驗證中介軟體中`Configure`方法：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

::: moniker-end

## <a name="cookie-policy-middleware"></a>Cookie 原則中介軟體

[Cookie 原則中介軟體](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)可讓應用程式中的 cookie 原則功能。 將中介軟體新增至應用程式處理管線是敏感; 的順序它只會影響已註冊後，管線中的元件。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions)提供給 Cookie 原則中介軟體可讓您控制全域性質的 cookie 處理和攔截到的 cookie 處理處理常式，當您附加或刪除 cookie 時。

| 屬性 | 描述 |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | 會影響是否 cookie 必須 HttpOnly，這是旗標，指出是否 cookie 應該僅供伺服器存取。 預設值為 `HttpOnlyPolicy.None`。 |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | 會影響的 cookie 相同站台屬性 （如下所示）。 預設值為 `SameSiteMode.Lax`。 此選項可供 ASP.NET Core 2.0 +。 |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | 呼叫時，附加 cookie。 |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | 刪除 cookie 時，會呼叫它。 |
| [Secure](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | 會影響是否 cookie 必須是安全。 預設值為 `CookieSecurePolicy.None`。 |

**MinimumSameSitePolicy** (ASP.NET Core 2.0 + 只)

預設值`MinimumSameSitePolicy`值是`SameSiteMode.Lax`允許 OAuth2 驗證。 嚴格強制執行的相同站台原則`SameSiteMode.Strict`，將`MinimumSameSitePolicy`。 雖然 OAuth2 和其他跨原始來源的驗證配置，這項設定會中斷，它的權限提高 cookie 的用戶端進行跨原始來源要求處理的應用程式的其他類型的安全性層級。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Cookie 原則中介軟體設定`MinimumSameSitePolicy`可能會影響您設定`Cookie.SameSite`在`CookieAuthenticationOptions`根據下面的矩陣圖的設定。

| MinimumSameSitePolicy | Cookie.SameSite | 結果 Cookie.SameSite 設定 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>建立驗證 cookie

若要建立保留使用者資訊的 cookie，您必須建構[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)。 使用者資訊會序列化並儲存在 cookie 中。 

::: moniker range=">= aspnetcore-2.0"

建立[ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)任何具有必要[宣告](/dotnet/api/system.security.claims.claim)s 和呼叫[Addtoroleasync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)使用者來登入：

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

呼叫[Addtoroleasync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)使用者來登入：

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker-end

`SignInAsync` 建立加密的 cookie，並將它新增至目前回應。 如果您未指定`AuthenticationScheme`，會使用預設配置。

實際上，使用的加密是 ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系統。 如果您裝載多部機器、 負載平衡的應用程式，或使用 web 伺服陣列上的應用程式，則您必須[設定資料保護](xref:security/data-protection/configuration/overview)使用相同的金鑰環及應用程式識別碼。

## <a name="sign-out"></a>登出

::: moniker range=">= aspnetcore-2.0"

若要將目前的使用者登出並刪除其 cookie，呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要將目前的使用者登出並刪除其 cookie，呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

::: moniker-end

如果您未使用`CookieAuthenticationDefaults.AuthenticationScheme`（或"Cookie"） 做為配置 (例如，"ContosoCookie 」)，提供設定的驗證提供者時所使用的配置。 否則，會使用預設的配置。

## <a name="react-to-back-end-changes"></a>回應後端的變更

一旦建立 cookie，它會變成身分識別的單一來源。 即使您停用使用者，在您的後端系統中，cookie 驗證系統一無所知，與使用者保持登入，只要其 cookie 為有效。

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)事件，在 ASP.NET Core 2.x 或[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1) 1.x 可用來攔截和覆寫 cookie 身分識別驗證的 ASP.NET Core 中的方法。 此方法可減輕存取應用程式的已撤銷使用者的風險。

Cookie 驗證的其中一個方法根據追蹤的使用者資料庫已變更時。 如果資料庫沒有已變更，因為使用者的 cookie 的發出，則不需要重新驗證使用者，如果其 cookie 仍然有效。 若要實作此案例中，資料庫中實作`IUserRepository`會針對此範例中，儲存`LastChanged`值。 當任何使用者會在資料庫中，更新`LastChanged`值設定為目前的時間。

若要以基礎資料庫變更時，使其失效的 cookie`LastChanged`值，請建立與 cookie`LastChanged`宣告包含目前`LastChanged`從資料庫的值：

```csharp
var claims = new List<Claim>
{
    new Claim(ClaimTypes.Name, user.Email),
    new Claim("LastChanged", {Database Value})
};

var claimsIdentity = new ClaimsIdentity(
    claims, 
    CookieAuthenticationDefaults.AuthenticationScheme);

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme, 
    new ClaimsPrincipal(claimsIdentity));
```

::: moniker range=">= aspnetcore-2.0"

若要實作的覆寫`ValidatePrincipal`事件，一種方法具有下列簽章，您可以從衍生類別中的寫入[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

範例看起來如下所示：

```csharp
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Authentication;
using Microsoft.AspNetCore.Authentication.Cookies;

public class CustomCookieAuthenticationEvents : CookieAuthenticationEvents
{
    private readonly IUserRepository _userRepository;

    public CustomCookieAuthenticationEvents(IUserRepository userRepository)
    {
        // Get the database from registered DI services.
        _userRepository = userRepository;
    }

    public override async Task ValidatePrincipal(CookieValidatePrincipalContext context)
    {
        var userPrincipal = context.Principal;

        // Look for the LastChanged claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !_userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

在 cookie 中的服務註冊期間註冊的事件執行個體`ConfigureServices`方法。 提供已設定領域的服務註冊您`CustomCookieAuthenticationEvents`類別：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要實作的覆寫`ValidateAsync`事件時，寫入具有下列簽章的方法：

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core 身分識別的一部分實作這項檢查其[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)。 範例看起來如下所示：

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = 
            context.HttpContext.RequestServices
                .GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        var lastChanged = (from c in userPrincipal.Claims
                           where c.Type == "LastChanged"
                           select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(lastChanged))
        {
            context.RejectPrincipal();

            await context.HttpContext.SignOutAsync(
                CookieAuthenticationDefaults.AuthenticationScheme);
        }
    }
}
```

註冊的事件中的 cookie 驗證組態期間`Configure`方法：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

::: moniker-end

更新的使用者名稱的情況下，請考慮&mdash;並不會影響以任何方式的安全性決策。 如果您想要非破壞性的方式更新使用者主體，呼叫`context.ReplacePrincipal`並設定`context.ShouldRenew`屬性設`true`。

> [!WARNING]
> 此處所述的方法是在每個要求時觸發。 這會導致應用程式對大量的效能產生負面影響。

## <a name="persistent-cookies"></a>永續性 cookie

您可能想要在瀏覽器工作階段之間保存的 cookie。 這個持續性，才應該啟用明確的使用者同意，「 還記得我 」 核取方塊上登入或類似的機制。 

下列程式碼片段會建立身分識別和對應的 cookie，透過瀏覽器結束時仍然有效。 會遵守任何先前設定的滑動逾期設定。 如果 cookie 已過期的瀏覽器關閉時，瀏覽器在重新啟動之後，就會清除的 cookie。

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties?view=aspnetcore-2.0)類別位於`Microsoft.AspNetCore.Authentication`命名空間。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.http.authentication.authenticationproperties?view=aspnetcore-1.1)類別位於`Microsoft.AspNetCore.Http.Authentication`命名空間。

::: moniker-end

## <a name="absolute-cookie-expiration"></a>絕對的 cookie 到期日

您可以設定絕對到期時間與`ExpiresUtc`。 您也必須設定`IsPersistent`; 否則即為`ExpiresUtc`會被忽略，並建立單一工作階段 cookie。 當`ExpiresUtc`上設定`SignInAsync`，它會覆寫的值`ExpireTimeSpan`選擇`CookieAuthenticationOptions`，如果設定。

下列程式碼片段會建立身分識別和對應的 cookie 可持續 20 分鐘的時間。 這會忽略任何先前設定的滑動逾期設定。

::: moniker range=">= aspnetcore-2.0"

```csharp
await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true,
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

::: moniker-end

## <a name="additional-resources"></a>其他資源

* [Auth 2.0 變更 / 移轉公告](https://github.com/aspnet/Announcements/issues/262)
* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [以原則為基礎的角色檢查](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
