---
title: 使用不含 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 瞭解如何在不 ASP.NET Core 身分識別的情況下使用 cookie 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/11/2020
uid: security/authentication/cookie
ms.openlocfilehash: 62a3d247dade6c83156a8378407d5e3891713fd1
ms.sourcegitcommit: 85564ee396c74c7651ac47dd45082f3f1803f7a2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/12/2020
ms.locfileid: "77172123"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>使用不含 ASP.NET Core 身分識別的 cookie 驗證

由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core 身分識別是完整且功能完整的驗證提供者，可用於建立和維護登入。 不過，不含 ASP.NET Core 身分識別的 cookie 型驗證提供者可以使用。 如需詳細資訊，請參閱<xref:security/authentication/identity>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

針對範例應用程式中的示範用途，假設使用者的使用者帳戶（Maria Rodriguez）已硬式編碼到應用程式中。 使用**電子郵件**位址 `maria.rodriguez@contoso.com` 和任何密碼來登入使用者。 使用者會在*頁面/帳戶/登入. cshtml .cs*檔案的 `AuthenticateUser` 方法中進行驗證。 在真實世界的範例中，使用者會針對資料庫進行驗證。

## <a name="configuration"></a>組態

如果應用程式不使用 [Microsoft.AspNetCore 中繼套件](xref:fundamentals/metapackage-app)，請在Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)中建立 AspNetCore 的套件參考套件。

在 `Startup.ConfigureServices` 方法中，使用 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 和 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> 方法來建立驗證中介軟體服務：

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> 傳遞至 `AddAuthentication` 會設定應用程式的預設驗證配置。 當有多個 cookie 驗證實例，而您想要以[特定的配置進行授權](xref:security/authorization/limitingidentitybyscheme)時，`AuthenticationScheme` 會很有用。 將設定為 [CookieAuthenticationDefaults AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) 時，會為配置提供 "cookie" 的值。`AuthenticationScheme` 您可以提供可區分配置的任何字串值。

應用程式的驗證配置與應用程式的 cookie 驗證配置不同。 未提供 cookie 驗證配置給 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>時，會使用 `CookieAuthenticationDefaults.AuthenticationScheme` （「Cookie」）。

根據預設，驗證 cookie 的 <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> 屬性會設定為 [`true`]。 當網站訪客未同意資料收集時，允許驗證 cookie。 如需詳細資訊，請參閱<xref:security/gdpr#essential-cookies>。

在 `Startup.Configure`中，呼叫 `UseAuthentication` 和 `UseAuthorization` 以設定 `HttpContext.User` 屬性，並針對要求執行授權中介軟體。 呼叫 `UseAuthentication` 並 `UseAuthorization` 方法，再呼叫 `UseEndpoints`：

[!code-csharp[](cookie/samples/3.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> 類別是用來設定驗證提供者選項。

在服務設定中，為 `Startup.ConfigureServices` 方法中的驗證設定 `CookieAuthenticationOptions`：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Cookie 原則中介軟體

[Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)可啟用 cookie 原則功能。 將中介軟體新增至應用程式處理管線會區分順序&mdash;它只會影響在管線中註冊的下游元件。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

使用提供給 Cookie 原則中介軟體的 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>，控制 cookie 處理的全域特性，並在附加或刪除 cookie 時連結至 cookie 處理處理常式。

預設 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> 值是 `SameSiteMode.Lax` 以允許 OAuth2 authentication。 若要嚴格地強制執行相同的網站原則 `SameSiteMode.Strict`，請設定 `MinimumSameSitePolicy`。 雖然這項設定會中斷 OAuth2 和其他跨原始來源驗證配置，但它會提升不依賴跨原始來源要求處理之其他類型應用程式的 cookie 安全性層級。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

`MinimumSameSitePolicy` 的 Cookie 原則中介軟體設定，可能會根據下列矩陣，影響 `CookieAuthenticationOptions` 設定中 `Cookie.SameSite` 的設定。

| MinimumSameSitePolicy | Cookie. SameSite | 結果 Cookie. SameSite 設定 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode。無     | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict |
| SameSiteMode. 寬鬆      | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict | SameSiteMode. 寬鬆<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict |
| SameSiteMode。 Strict   | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict | SameSiteMode。 Strict<br>SameSiteMode。 Strict<br>SameSiteMode。 Strict |

## <a name="create-an-authentication-cookie"></a>建立驗證 cookie

若要建立保留使用者資訊的 cookie，請建立 <xref:System.Security.Claims.ClaimsPrincipal>。 使用者資訊會序列化並儲存在 cookie 中。 

建立具有任何必要 <xref:System.Security.Claims.Claim>的 <xref:System.Security.Claims.ClaimsIdentity>，並呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> 以登入使用者：

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` 會建立加密的 cookie，並將其新增至目前的回應。 如果未指定 `AuthenticationScheme`，則會使用預設配置。

ASP.NET Core 的[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。 若為裝載于多部電腦上的應用程式、跨應用程式的負載平衡，或使用 web 伺服陣列，請[將資料保護設定](xref:security/data-protection/configuration/overview)為使用相同的金鑰環形和應用程式識別碼。

## <a name="sign-out"></a>登出

若要登出目前的使用者並刪除其 cookie，請呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>：

[!code-csharp[](cookie/samples/3.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

如果 `CookieAuthenticationDefaults.AuthenticationScheme` （或「Cookie」）不是用來做為配置（例如，"ContosoCookie"），請提供設定驗證提供者時所使用的架構。 否則，會使用預設配置。

## <a name="react-to-back-end-changes"></a>對後端變更做出回應

一旦建立 cookie，cookie 就是身分識別的單一來源。 如果在後端系統中停用使用者帳戶：

* 應用程式的 cookie 驗證系統會繼續根據驗證 cookie 來處理要求。
* 只要驗證 cookie 有效，使用者就會繼續登入應用程式。

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> 事件可以用來攔截和覆寫 cookie 身分識別的驗證。 在每個要求上驗證 cookie，可降低撤銷存取應用程式之使用者的風險。

Cookie 驗證的一種方法是以追蹤使用者資料庫變更的時間為基礎。 如果自使用者的 cookie 發行後，資料庫尚未變更，則不需要重新驗證使用者（如果其 cookie 仍然有效）。 在範例應用程式中，資料庫會在 `IUserRepository` 中執行，並儲存 `LastChanged` 值。 當資料庫中的使用者更新時，`LastChanged` 值會設為目前的時間。

若要在資料庫根據 `LastChanged` 值變更時使 cookie 失效，請使用包含資料庫目前 `LastChanged` 值的 `LastChanged` 宣告來建立 cookie：

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

若要為 `ValidatePrincipal` 事件執行覆寫，請在衍生自 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>的類別中，撰寫具有下列簽章的方法：

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

以下是 `CookieAuthenticationEvents`的範例執行：

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

在 `Startup.ConfigureServices` 方法中的 cookie 服務註冊期間註冊事件實例。 為您的 `CustomCookieAuthenticationEvents` 類別提供[限定範圍的服務註冊](xref:fundamentals/dependency-injection#service-lifetimes)：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

考慮到使用者名稱更新的情況，&mdash;不會以任何方式影響安全性的決策。 如果您想要以非破壞性的更新使用者主體，請呼叫 `context.ReplacePrincipal`，並將 `context.ShouldRenew` 屬性設定為 [`true`]。

> [!WARNING]
> 這裡所述的方法會在每個要求上觸發。 在每個要求上驗證所有使用者的驗證 cookie，可能會導致應用程式的效能大幅下降。

## <a name="persistent-cookies"></a>持續性 cookie

您可能想要 cookie 跨瀏覽器會話保存。 只有在登入或類似的機制上有 [記住我] 核取方塊的明確使用者同意，才能啟用此持續性。 

下列程式碼片段會建立可透過瀏覽器結束不受的身分識別和對應的 cookie。 系統會接受先前設定的任何滑動到期設定。 如果 cookie 在瀏覽器關閉時過期，瀏覽器會在重新開機後清除 cookie。

將 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>中的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> 設定為 `true`：

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>絕對 cookie 到期日

絕對到期時間可以使用 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>來設定。 若要建立持續性 cookie，也必須設定 `IsPersistent`。 否則，會使用以會話為基礎的存留期來建立 cookie，而且可能會在它所保留的驗證票證之前或之後過期。 設定 `ExpiresUtc` 時，它會覆寫 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>的 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions.ExpireTimeSpan> 選項值（如果已設定）。

下列程式碼片段會建立持續20分鐘的身分識別和對應的 cookie。 這會忽略先前設定的任何滑動到期設定。

```csharp
// using Microsoft.AspNetCore.Authentication;

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

::: moniker range="< aspnetcore-3.0"

ASP.NET Core 身分識別是完整且功能完整的驗證提供者，可用於建立和維護登入。 不過，您可以使用 cookie 型驗證驗證提供者，而不需要 ASP.NET Core 身分識別。 如需詳細資訊，請參閱<xref:security/authentication/identity>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

針對範例應用程式中的示範用途，假設使用者的使用者帳戶（Maria Rodriguez）已硬式編碼到應用程式中。 使用**電子郵件**位址 `maria.rodriguez@contoso.com` 和任何密碼來登入使用者。 使用者會在*頁面/帳戶/登入. cshtml .cs*檔案的 `AuthenticateUser` 方法中進行驗證。 在真實世界的範例中，使用者會針對資料庫進行驗證。

## <a name="configuration"></a>組態

如果應用程式不使用 [Microsoft.AspNetCore 中繼套件](xref:fundamentals/metapackage-app)，請在Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)中建立 AspNetCore 的套件參考套件。

在 `Startup.ConfigureServices` 方法中，使用 <xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*> 和 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*> 方法來建立驗證中介軟體服務：

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> 傳遞至 `AddAuthentication` 會設定應用程式的預設驗證配置。 當有多個 cookie 驗證實例，而您想要以[特定的配置進行授權](xref:security/authorization/limitingidentitybyscheme)時，`AuthenticationScheme` 會很有用。 將設定為 [CookieAuthenticationDefaults AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme) 時，會為配置提供 "cookie" 的值。`AuthenticationScheme` 您可以提供可區分配置的任何字串值。

應用程式的驗證配置與應用程式的 cookie 驗證配置不同。 未提供 cookie 驗證配置給 <xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>時，會使用 `CookieAuthenticationDefaults.AuthenticationScheme` （「Cookie」）。

根據預設，驗證 cookie 的 <xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential> 屬性會設定為 [`true`]。 當網站訪客未同意資料收集時，允許驗證 cookie。 如需詳細資訊，請參閱<xref:security/gdpr#essential-cookies>。

在 `Startup.Configure` 方法中，呼叫 `UseAuthentication` 方法，以叫用設定 `HttpContext.User` 屬性的驗證中介軟體。 呼叫 `UseAuthentication` 方法，再呼叫 `UseMvcWithDefaultRoute` 或 `UseMvc`：

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions> 類別是用來設定驗證提供者選項。

在服務設定中，為 `Startup.ConfigureServices` 方法中的驗證設定 `CookieAuthenticationOptions`：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Cookie 原則中介軟體

[Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)可啟用 cookie 原則功能。 將中介軟體新增至應用程式處理管線會區分順序&mdash;它只會影響在管線中註冊的下游元件。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

使用提供給 Cookie 原則中介軟體的 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>，控制 cookie 處理的全域特性，並在附加或刪除 cookie 時連結至 cookie 處理處理常式。

預設 <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy> 值是 `SameSiteMode.Lax` 以允許 OAuth2 authentication。 若要嚴格地強制執行相同的網站原則 `SameSiteMode.Strict`，請設定 `MinimumSameSitePolicy`。 雖然這項設定會中斷 OAuth2 和其他跨原始來源驗證配置，但它會提升不依賴跨原始來源要求處理之其他類型應用程式的 cookie 安全性層級。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

`MinimumSameSitePolicy` 的 Cookie 原則中介軟體設定，可能會根據下列矩陣，影響 `CookieAuthenticationOptions` 設定中 `Cookie.SameSite` 的設定。

| MinimumSameSitePolicy | Cookie. SameSite | 結果 Cookie. SameSite 設定 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode。無     | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict |
| SameSiteMode. 寬鬆      | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict | SameSiteMode. 寬鬆<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict |
| SameSiteMode。 Strict   | SameSiteMode。無<br>SameSiteMode. 寬鬆<br>SameSiteMode。 Strict | SameSiteMode。 Strict<br>SameSiteMode。 Strict<br>SameSiteMode。 Strict |

## <a name="create-an-authentication-cookie"></a>建立驗證 cookie

若要建立保留使用者資訊的 cookie，請建立 <xref:System.Security.Claims.ClaimsPrincipal>。 使用者資訊會序列化並儲存在 cookie 中。 

建立具有任何必要 <xref:System.Security.Claims.Claim>的 <xref:System.Security.Claims.ClaimsIdentity>，並呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*> 以登入使用者：

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` 會建立加密的 cookie，並將其新增至目前的回應。 如果未指定 `AuthenticationScheme`，則會使用預設配置。

ASP.NET Core 的[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。 若為裝載于多部電腦上的應用程式、跨應用程式的負載平衡，或使用 web 伺服陣列，請[將資料保護設定](xref:security/data-protection/configuration/overview)為使用相同的金鑰環形和應用程式識別碼。

## <a name="sign-out"></a>登出

若要登出目前的使用者並刪除其 cookie，請呼叫 <xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>：

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

如果 `CookieAuthenticationDefaults.AuthenticationScheme` （或「Cookie」）不是用來做為配置（例如，"ContosoCookie"），請提供設定驗證提供者時所使用的架構。 否則，會使用預設配置。

## <a name="react-to-back-end-changes"></a>對後端變更做出回應

一旦建立 cookie，cookie 就是身分識別的單一來源。 如果在後端系統中停用使用者帳戶：

* 應用程式的 cookie 驗證系統會繼續根據驗證 cookie 來處理要求。
* 只要驗證 cookie 有效，使用者就會繼續登入應用程式。

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*> 事件可以用來攔截和覆寫 cookie 身分識別的驗證。 在每個要求上驗證 cookie，可降低撤銷存取應用程式之使用者的風險。

Cookie 驗證的一種方法是以追蹤使用者資料庫變更的時間為基礎。 如果自使用者的 cookie 發行後，資料庫尚未變更，則不需要重新驗證使用者（如果其 cookie 仍然有效）。 在範例應用程式中，資料庫會在 `IUserRepository` 中執行，並儲存 `LastChanged` 值。 當資料庫中的使用者更新時，`LastChanged` 值會設為目前的時間。

若要在資料庫根據 `LastChanged` 值變更時使 cookie 失效，請使用包含資料庫目前 `LastChanged` 值的 `LastChanged` 宣告來建立 cookie：

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

若要為 `ValidatePrincipal` 事件執行覆寫，請在衍生自 <xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>的類別中，撰寫具有下列簽章的方法：

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

以下是 `CookieAuthenticationEvents`的範例執行：

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

在 `Startup.ConfigureServices` 方法中的 cookie 服務註冊期間註冊事件實例。 為您的 `CustomCookieAuthenticationEvents` 類別提供[限定範圍的服務註冊](xref:fundamentals/dependency-injection#service-lifetimes)：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

考慮到使用者名稱更新的情況，&mdash;不會以任何方式影響安全性的決策。 如果您想要以非破壞性的更新使用者主體，請呼叫 `context.ReplacePrincipal`，並將 `context.ShouldRenew` 屬性設定為 [`true`]。

> [!WARNING]
> 這裡所述的方法會在每個要求上觸發。 在每個要求上驗證所有使用者的驗證 cookie，可能會導致應用程式的效能大幅下降。

## <a name="persistent-cookies"></a>持續性 cookie

您可能想要 cookie 跨瀏覽器會話保存。 只有在登入或類似的機制上有 [記住我] 核取方塊的明確使用者同意，才能啟用此持續性。 

下列程式碼片段會建立可透過瀏覽器結束不受的身分識別和對應的 cookie。 系統會接受先前設定的任何滑動到期設定。 如果 cookie 在瀏覽器關閉時過期，瀏覽器會在重新開機後清除 cookie。

將 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>中的 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent> 設定為 `true`：

```csharp
// using Microsoft.AspNetCore.Authentication;

await HttpContext.SignInAsync(
    CookieAuthenticationDefaults.AuthenticationScheme,
    new ClaimsPrincipal(claimsIdentity),
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

## <a name="absolute-cookie-expiration"></a>絕對 cookie 到期日

絕對到期時間可以使用 <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>來設定。 若要建立持續性 cookie，也必須設定 `IsPersistent`。 否則，會使用以會話為基礎的存留期來建立 cookie，而且可能會在它所保留的驗證票證之前或之後過期。 設定 `ExpiresUtc` 時，它會覆寫 <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>的 <xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan> 選項值（如果已設定）。

下列程式碼片段會建立持續20分鐘的身分識別和對應的 cookie。 這會忽略先前設定的任何滑動到期設定。

```csharp
// using Microsoft.AspNetCore.Authentication;

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

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [以原則為基礎的角色檢查](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
