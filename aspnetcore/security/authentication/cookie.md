---
title: 使用沒有 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 需使用沒有 ASP.NET Core 身分識別的 cookie 驗證的說明
ms.author: riande
ms.date: 10/11/2017
uid: security/authentication/cookie
ms.openlocfilehash: 2064e4d6406ce3ca3ce28732f113e8c81447aace
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275602"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>使用沒有 ASP.NET Core 身分識別的 cookie 驗證

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

您在先前的驗證主題中，看到[ASP.NET Core 識別](xref:security/authentication/identity)是完整的功能完整的驗證提供者來建立及維護的登入。 不過，您可以使用 cookie 架構驗證情況下使用您自己的自訂驗證邏輯。 您可以做為獨立驗證提供者，而 ASP.NET Core 識別不使用 cookie 為基礎的驗證。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

為了示範之用的範例應用程式中，假設使用者 Maria Rodriguez 的使用者帳戶是在應用程式的硬式編碼。 使用電子郵件使用者名稱 」maria.rodriguez@contoso.com"和任何登入使用者的密碼。 使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。 在真實世界範例中，您會驗證使用者，對資料庫。

如需有關移轉 cookie 為基礎的驗證，從 ASP.NET Core 詳細 1.x 為 2.0，請參閱[移轉的驗證和 ASP.NET Core 2.0 主題 （Cookie 架構驗證） 的身分識別](xref:migration/1x-to-2x/identity-2x#cookie-based-authentication)。

若要使用 ASP.NET Core 身分識別，請參閱[識別簡介](xref:security/authentication/identity)主題。

## <a name="configuration"></a>組態

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

如果應用程式不會使用[Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app)，在專案檔中建立封裝參考[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)封裝 (版本 2.1.0 或更新版本）。

在`ConfigureServices`方法，建立驗證中介軟體服務與`AddAuthentication`和`AddCookie`方法：

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

`AuthenticationScheme` 傳遞至`AddAuthentication`設定應用程式的預設驗證配置。 `AuthenticationScheme` 有多個執行個體的驗證 cookie，而且您想要時相當實用[授權特定的結構描述與](xref:security/authorization/limitingidentitybyscheme)。 設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。 您可以提供區別配置任何字串值。

在`Configure`方法，請使用`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。 呼叫`UseAuthentication`方法之前先呼叫`UseMvcWithDefaultRoute`或`UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

**AddCookie 選項**

[CookieAuthenticationOptions](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions?view=aspnetcore-2.0)類別用來設定驗證提供者選項。

| 選項 | 描述 |
| ------ | ----------- |
| [AccessDeniedPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.accessdeniedpath?view=aspnetcore-2.0) | 提供找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ForbidAsync`。 預設值是 `/Account/AccessDenied`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.claimsissuer?view=aspnetcore-2.0) | 若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證服務所建立的任何宣告上的屬性。 |
| [Cookie.Domain](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.domain?view=aspnetcore-2.0) | 其中提供 cookie 的網域名稱。 根據預設，這是要求的主機名稱。 瀏覽器只在要求中，cookie 傳送相符的主機名稱。 您可能想要調整這在您的網域中有任何主機可用的 cookie。 例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。 |
| [Cookie.Expiration](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.expiration?view=aspnetcore-2.0) | 取得或設定 cookie 的壽命。 目前這個選項沒有 ops，就會變成過時中 ASP.NET Core 2.1 +。 使用`ExpireTimeSpan`選項可用來設定的 cookie 到期。 如需詳細資訊，請參閱[釐清 CookieAuthenticationOptions.Cookie.Expiration 行為](https://github.com/aspnet/Security/issues/1293)。 |
| [Cookie.HttpOnly](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.httponly?view=aspnetcore-2.0) | 旗標，指出是否 cookie 應該只能由伺服器存取。 變更此值為`false`允許存取 cookie 的用戶端指令碼，可能會用來開啟您的應用程式應該有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。 預設值是 `true`。 |
| [Cookie.Name](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.name?view=aspnetcore-2.0) | 設定 cookie 的名稱。 |
| [Cookie.Path](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.path?view=aspnetcore-2.0) | 用來隔離在相同的主機名稱上執行的應用程式。 如果您有在執行的應用程式`/app1`想要限制該應用程式的 cookie，請設定`CookiePath`屬性`/app1`。 如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。 |
| [Cookie.SameSite](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.samesite?view=aspnetcore-2.0) | 表示瀏覽器是否應該允許 cookie 附加至相同站台要求只 (`SameSiteMode.Strict`) 或跨網站要求使用安全的 HTTP 方法和相同站台要求 (`SameSiteMode.Lax`)。 當設定為`SameSiteMode.None`，未設定的 cookie 標頭值。 請注意， [Cookie 原則中介軟體](#cookie-policy-middleware)可能會覆寫您所提供的值。 若要支援 OAuth 驗證，預設值是`SameSiteMode.Lax`。 如需詳細資訊，請參閱[因為 SameSite cookie 原則而中斷的 OAuth 驗證](https://github.com/aspnet/Security/issues/1231)。 |
| [Cookie.SecurePolicy](/dotnet/api/microsoft.aspnetcore.http.cookiebuilder.securepolicy?view=aspnetcore-2.0) | 旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)、 HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或為要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。 預設值是 `CookieSecurePolicy.SameAsRequest`。 |
| [DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0) | 設定`DataProtectionProvider`用來建立預設`TicketDataFormat`。 如果`TicketDataFormat`設定屬性，則`DataProtectionProvider`選項不會使用。 如果未提供，則會使用應用程式的預設資料保護提供者。 |
| [事件](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.events?view=aspnetcore-2.0) | 此處理常式呼叫方法上提供的應用程式控制項在特定處理的點上的提供者。 如果`Events`不提供的預設執行個體提供，呼叫的方法時，不做任何動作。 |
| [EventsType](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.eventstype?view=aspnetcore-2.0) | 若要取得做為服務類型`Events`而不是屬性的執行個體。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.expiretimespan?view=aspnetcore-2.0) | `TimeSpan`之後儲存在 cookie 內的驗證票證已過期。 `ExpireTimeSpan` 會加入至目前的時間來建立票證的到期時間。 `ExpiredTimeSpan`值一律會移到加密伺服器所驗證的 AuthTicket。 它也可能會進入[Set-cookie](https://tools.ietf.org/html/rfc6265#section-4.1)標頭，但是只有`IsPersistent`設定。 若要設定`IsPersistent`至`true`，設定[AuthenticationProperties](/dotnet/api/microsoft.aspnetcore.authentication.authenticationproperties)傳遞至`SignInAsync`。 預設值`ExpireTimeSpan`為 14 天。 |
| [LoginPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.loginpath?view=aspnetcore-2.0) | 提供找到 302 （重新導向 URL） 所提供的路徑時所觸發`HttpContext.ChallengeAsync`。 產生 401 的目前 URL 加入至`LoginPath`所命名的查詢字串參數為`ReturnUrlParameter`。 一次要求`LoginPath`授與新的登入身分，`ReturnUrlParameter`值會用來將瀏覽器重新導向回到造成原始未授權的狀態碼的 URL。 預設值是 `/Account/Login`。 |
| [LogoutPath](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.logoutpath?view=aspnetcore-2.0) | 如果`LogoutPath`提供，加入處理常式，則該路徑的要求重新導向的值為基礎`ReturnUrlParameter`。 預設值是 `/Account/Logout`。 |
| [ReturnUrlParameter](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.returnurlparameter?view=aspnetcore-2.0) | 決定後面 302 發現 （重新導向 URL） 回應的處理常式附加的查詢字串參數的名稱。 `ReturnUrlParameter` 在要求抵達時，會使用`LoginPath`或`LogoutPath`執行登入或登出動作後，傳回瀏覽器的原始 url。 預設值是 `ReturnUrl`。 |
| [SessionStore](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.sessionstore?view=aspnetcore-2.0) | 選擇性的容器，用來儲存跨要求的身分識別。 使用時，工作階段識別碼會傳送至用戶端。 `SessionStore` 可用來降低大型的身分識別的潛在問題。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-2.0) | 旗標，指出是否要以動態方式發出新的更新的到期時間的 cookie。 這可能會發生在任何位置的目前 cookie 的逾期期限已超過 50%過期的要求。 新的到期日會向前移至目前日期加上`ExpireTimespan`。 [絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。 絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。 預設值是 `true`。 |
| [TicketDataFormat](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.ticketdataformat?view=aspnetcore-2.0) | `TicketDataFormat`用來保護且取消保護身分識別，並且會儲存在 cookie 值中的其他屬性。 如果未提供，`TicketDataFormat`使用建立[DataProtectionProvider](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationoptions.dataprotectionprovider?view=aspnetcore-2.0)。 |
| [驗證](/dotnet/api/microsoft.aspnetcore.authentication.authenticationschemeoptions.validate?view=aspnetcore-2.0) | 檢查的選項都有效的方法。 |

設定`CookieAuthenticationOptions`中驗證的服務組態中`ConfigureServices`方法：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

ASP.NET Core 1.x 使用 cookie[中介軟體](xref:fundamentals/middleware/index)，序列化為使用者主體已加密的 cookie。 在後續要求中，對 cookie 進行驗證，以及主體已重新建立並指派給`HttpContext.User`屬性。

安裝[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)專案中的 NuGet 封裝。 此套件包含 cookie 中介軟體。

使用`UseCookieAuthentication`方法中的`Configure`方法在您*Startup.cs*檔案之後，再`UseMvc`或`UseMvcWithDefaultRoute`:

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
| [AuthenticationScheme](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.authenticationscheme?view=aspnetcore-1.1) | 設定驗證配置。 `AuthenticationScheme` 有多個執行個體的驗證，而且您想要時相當實用[授權特定的結構描述與](xref:security/authorization/limitingidentitybyscheme)。 設定`AuthenticationScheme`至`CookieAuthenticationDefaults.AuthenticationScheme`配置會提供 [Cookie] 的值。 您可以提供區別配置任何字串值。 |
| [AutomaticAuthenticate](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticauthenticate?view=aspnetcore-1.1) | 設定值，指出應該執行每個要求的 cookie 驗證，並嘗試驗證，重新建構建立它的任何序列化的主體。 |
| [AutomaticChallenge](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.automaticchallenge?view=aspnetcore-1.1) | 如果為 true，驗證中介軟體會處理自動挑戰。 如果為 false，驗證中介軟體僅改變回應明確指出時`AuthenticationScheme`。 |
| [ClaimsIssuer](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.claimsissuer?view=aspnetcore-1.1) | 若要使用的簽發者[簽發者](/dotnet/api/system.security.claims.claim.issuer)cookie 驗證中介軟體所建立的任何宣告上的屬性。 |
| [CookieDomain](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiedomain?view=aspnetcore-1.1) | 其中提供 cookie 的網域名稱。 根據預設，這是要求的主機名稱。 瀏覽器，只是要比對的主機名稱的 cookie。 您可能想要調整這在您的網域中有任何主機可用的 cookie。 例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，和`staging.www.contoso.com`。 |
| [CookieHttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiehttponly?view=aspnetcore-1.1) | 旗標，指出是否 cookie 應該只能由伺服器存取。 變更此值為`false`允許存取 cookie 的用戶端指令碼，可能會用來開啟您的應用程式應該有的 cookie 竊取您的應用程式[跨網站指令碼 (XSS)](xref:security/cross-site-scripting)弱點。 預設值是 `true`。 |
| [CookiePath](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiepath?view=aspnetcore-1.1) | 用來隔離在相同的主機名稱上執行的應用程式。 如果您有在執行的應用程式`/app1`想要限制該應用程式的 cookie，請設定`CookiePath`屬性`/app1`。 如此一來，cookie 才可用的要求`/app1`和其下的任何應用程式。 |
| [CookieSecure](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.cookiesecure?view=aspnetcore-1.1) | 旗標，指出是否建立的 cookie 會受限於 HTTPS (`CookieSecurePolicy.Always`)、 HTTP 或 HTTPS (`CookieSecurePolicy.None`)，或為要求相同的通訊協定 (`CookieSecurePolicy.SameAsRequest`)。 預設值是 `CookieSecurePolicy.SameAsRequest`。 |
| [描述](/dotnet/api/microsoft.aspnetcore.builder.authenticationoptions.description?view=aspnetcore-1.1) | 其他資訊才可以提供給應用程式的驗證類型的詳細資訊。 |
| [ExpireTimeSpan](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.expiretimespan?view=aspnetcore-1.1) | `TimeSpan`之後驗證票證已過期。 它會加入到目前的時間來建立票證的到期時間。 若要使用`ExpireTimeSpan`，您必須設定`IsPersistent`至`true`中`AuthenticationProperties`傳遞至`SignInAsync`。 預設值為 14 天。 |
| [SlidingExpiration](/dotnet/api/microsoft.aspnetcore.builder.cookieauthenticationoptions.slidingexpiration?view=aspnetcore-1.1) | 旗標，指出是否 cookie 的到期日重設時超過一半的`ExpireTimeSpan`間隔。 新的 exipiration 時間往前移動到目前日期加上`ExpireTimespan`。 [絕對的 cookie 到期時間](xref:security/authentication/cookie#absolute-cookie-expiration)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。 絕對到期時間可以改善您的應用程式的安全性限制的驗證 cookie 為有效的時間量。 預設值是 `true`。 |

設定`CookieAuthenticationOptions`Cookie 驗證中介軟體中`Configure`方法：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    ...
});
```

---

## <a name="cookie-policy-middleware"></a>Cookie 的原則中介軟體

[Cookie 的原則中介軟體](/dotnet/api/microsoft.aspnetcore.cookiepolicy.cookiepolicymiddleware)可讓應用程式中的 cookie 原則功能。 將中介軟體新增到應用程式處理管線是順序機密;它只會影響註冊之後，管線中的元件。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

 [CookiePolicyOptions](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions) Cookie 原則中介軟體提供讓您控制 cookie 處理處理常式的全域性質的 cookie 處理和攔截 cookie 被附加或刪除時。

| 屬性 | 描述 |
| -------- | ----------- |
| [HttpOnly](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.httponly) | 會影響是否 cookie 必須 HttpOnly，這是旗標，指出是否 cookie 應該只能由伺服器存取。 預設值是 `HttpOnlyPolicy.None`。 |
| [MinimumSameSitePolicy](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.minimumsamesitepolicy) | 會影響 cookie 的相同站台屬性 （請參閱下文）。 預設值是 `SameSiteMode.Lax`。 此選項適用於 ASP.NET Core 2.0 +。 |
| [OnAppendCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.onappendcookie) | Cookie 會附加時呼叫。 |
| [OnDeleteCookie](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.ondeletecookie) | 刪除 cookie 時呼叫。 |
| [安全](/dotnet/api/microsoft.aspnetcore.builder.cookiepolicyoptions.secure) | 會影響是否 cookie 必須是安全的。 預設值是 `CookieSecurePolicy.None`。 |

**MinimumSameSitePolicy** （ASP.NET 2.0 + 僅限核心）

預設值`MinimumSameSitePolicy`值是`SameSiteMode.Lax`允許 OAuth2 驗證。 嚴格地強制執行的相同站台原則`SameSiteMode.Strict`，將`MinimumSameSitePolicy`。 雖然這項設定中斷 OAuth2 和其他的跨原始驗證配置時，它會模仿不要依賴跨原始要求處理的應用程式的其他類型的 cookie 安全性層級。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Cookie 的原則中介軟體設定`MinimumSameSitePolicy`可能會影響您設定`Cookie.SameSite`中`CookieAuthenticationOptions`根據矩陣下方的設定。

| MinimumSameSitePolicy | Cookie.SameSite | 結果 Cookie.SameSite 設定 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>建立驗證 cookie

若要建立的保留使用者資訊的 cookie，您必須建構[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal)。 使用者資訊會序列化並儲存在 cookie 中。 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

建立[ClaimsIdentity](/dotnet/api/system.security.claims.claimsidentity)與任何所需[宣告](/dotnet/api/system.security.claims.claim)s 和呼叫[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signinasync?view=aspnetcore-2.0)登入使用者：

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

呼叫[SignInAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signinasync?view=aspnetcore-1.1)登入使用者：

```csharp
await HttpContext.Authentication.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity));
```

---

`SignInAsync` 建立加密的 cookie，並將它加入至目前回應。 如果您未指定`AuthenticationScheme`，則會使用預設配置。

實際上，使用的加密是 ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系統。 如果您裝載的多部電腦、 負載平衡的應用程式，或使用 web 伺服陣列上的應用程式，則您必須[設定資料保護](xref:security/data-protection/configuration/overview)使用相同的索引鍵環形和應用程式識別碼。

## <a name="sign-out"></a>登出

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

若要登出目前的使用者，並刪除其 cookie，請呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhttpcontextextensions.signoutasync?view=aspnetcore-2.0):

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

若要登出目前的使用者，並刪除其 cookie，請呼叫[SignOutAsync](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1.signoutasync?view=aspnetcore-1.1):

```csharp
await HttpContext.Authentication.SignOutAsync(
    CookieAuthenticationDefaults.AuthenticationScheme);
```

---

如果您不使用`CookieAuthenticationDefaults.AuthenticationScheme`（也"稱為 Cookie"） 做為配置 (例如，"ContosoCookie")，提供設定的驗證提供者時所使用的配置。 否則，會使用預設配置。

## <a name="react-to-back-end-changes"></a>回應端的變更

一旦建立 cookie 時，它會變成身分識別的單一來源。 即使您停用後端系統中的使用者，cookie 驗證系統並不了解，以及使用者保持登入，只要其 cookie 為有效。

[ValidatePrincipal](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents.validateprincipal)中 ASP.NET Core 事件 2.x 或[ValidateAsync](/dotnet/api/microsoft.aspnetcore.identity.isecuritystampvalidator.validateasync?view=aspnetcore-1.1)中 ASP.NET Core 1.x 可以用來攔截並覆寫 cookie 身分識別的驗證方法。 這種方法可減少已撤銷使用者存取應用程式的風險。

驗證 cookie 的其中一個方法根據追蹤的使用者資料庫已變更時。 如果資料庫沒有已變更，因為使用者的 cookie 發出，但是沒有需要重新驗證使用者，其 cookie 是否仍然有效。 若要實作此案例中，資料庫中，這實作於`IUserRepository`會針對此範例中，儲存`LastChanged`值。 在資料庫中，更新任何使用者時`LastChanged`值設定為目前的時間。

若要讓 cookie 失效時的資料庫變更為基礎`LastChanged`值，請建立與 cookie`LastChanged`宣告包含目前`LastChanged`從資料庫的值：

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

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

若要實作的覆寫`ValidatePrincipal`事件，將您從衍生類別中的下列簽章的方法寫入[CookieAuthenticationEvents](/dotnet/api/microsoft.aspnetcore.authentication.cookies.cookieauthenticationevents):

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

範例如下所示：

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

在 cookie 中的服務登錄期間註冊事件執行個體`ConfigureServices`方法。 提供已設定領域的服務註冊您`CustomCookieAuthenticationEvents`類別：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

若要實作的覆寫`ValidateAsync`事件，將具有下列簽章的方法寫入：

```csharp
ValidateAsync(CookieValidatePrincipalContext)
```

ASP.NET Core 身分識別的一部分實作這項檢查其[SecurityStampValidator](/dotnet/api/microsoft.aspnetcore.identity.securitystampvalidator-1.validateasync)。 範例如下所示：

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

在 cookie 驗證組態設定期間註冊事件`Configure`方法：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    Events = new CookieAuthenticationEvents
    {
        OnValidatePrincipal = LastChangedValidator.ValidateAsync
    }
});
```

---

請考慮在使用者的名稱更新的情況下&mdash;並不會影響安全性，以任何方式的決策。 如果您想要非破壞性的方式更新使用者的主體，呼叫`context.ReplacePrincipal`並設定`context.ShouldRenew`屬性`true`。

> [!WARNING]
> 此處所述的方法是在每個要求觸發的。 這會導致應用程式對效能帶來負面影響。

## <a name="persistent-cookies"></a>永續性 cookie

您可能想要在瀏覽器工作階段之間保存的 cookie。 此持續性，才應該啟用明確的使用者同意 「 記住我 核取方塊上登入或類似的機制。 

下列程式碼片段會建立身分識別和對應未透過瀏覽器封閉區段的 cookie。 會接受任何先前設定的滑動逾期設定。 如果 cookie 過期時就會關閉瀏覽器，瀏覽器之後重新啟動時，就會清除 cookie。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="absolute-cookie-expiration"></a>絕對的 cookie 到期

您可以設定與絕對到期時間`ExpiresUtc`。 您也必須設定`IsPersistent`，否則`ExpiresUtc`會被忽略，並建立單一工作階段 cookie。 當`ExpiresUtc`上設定`SignInAsync`，它會覆寫的值`ExpireTimeSpan`選項`CookieAuthenticationOptions`，如果設定。

下列程式碼片段會建立身分識別和對應的 cookie 會持續 20 分鐘。 這會忽略任何先前設定的滑動逾期設定。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

---

## <a name="additional-resources"></a>其他資源

* [Auth 2.0 變更 / 移轉公告](https://github.com/aspnet/Announcements/issues/262)
* [以配置限制身分識別](xref:security/authorization/limitingidentitybyscheme)
* [宣告式授權](xref:security/authorization/claims)
* [以原則為基礎的角色檢查](xref:security/authorization/roles#policy-based-role-checks)
