---
title: 使用沒有 ASP.NET Core 身分識別的 cookie 驗證
author: rick-anderson
description: 了解如何使用沒有 ASP.NET Core 身分識別的 cookie 驗證。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/07/2019
uid: security/authentication/cookie
ms.openlocfilehash: bbba2e77f806e1ed30bb734763cdbaedc1471d62
ms.sourcegitcommit: 91cc1f07ef178ab709ea42f8b3a10399c970496e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/08/2019
ms.locfileid: "67622737"
---
# <a name="use-cookie-authentication-without-aspnet-core-identity"></a>使用沒有 ASP.NET Core 身分識別的 cookie 驗證

藉由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Luke Latham](https://github.com/guardrex)

ASP.NET Core Identity 是完整且功能完整的驗證提供者來建立及維護的登入。 不過，您可以使用沒有 ASP.NET Core 身分識別的 cookie 為基礎的驗證驗證提供者。 如需詳細資訊，請參閱 <xref:security/authentication/identity>。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authentication/cookie/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

基於示範目的，範例應用程式中，假設使用者林麗莉 Rodriguez 的使用者帳戶會是硬式編碼到應用程式。 使用**電子郵件**使用者名稱`maria.rodriguez@contoso.com`和任何登入使用者的密碼。 使用者通過驗證`AuthenticateUser`方法中的*Pages/Account/Login.cshtml.cs*檔案。 在真實世界範例中，您會驗證使用者，對資料庫。

## <a name="configuration"></a>組態

如果應用程式不會使用[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，建立的專案檔中的套件參考[Microsoft.AspNetCore.Authentication.Cookies](https://www.nuget.org/packages/Microsoft.AspNetCore.Authentication.Cookies/)封裝。

在 `Startup.ConfigureServices`方法，建立驗證中介軟體服務<xref:Microsoft.Extensions.DependencyInjection.AuthenticationServiceCollectionExtensions.AddAuthentication*>和<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>方法：

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet1)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme> 傳遞至`AddAuthentication`設定應用程式的預設驗證配置。 `AuthenticationScheme` 當有多個執行個體的 cookie 驗證，而且您想要時非常有用[特定的結構描述的授權](xref:security/authorization/limitingidentitybyscheme)。 設定`AuthenticationScheme`要[CookieAuthenticationDefaults.AuthenticationScheme](xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationDefaults.AuthenticationScheme)配置會提供 [Cookie] 的值。 您可以提供區分配置任何字串值。

應用程式的驗證配置與不同應用程式的 cookie 驗證配置。 當 cookie 驗證配置不提供給<xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*>，它會使用`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie")。

驗證 cookie<xref:Microsoft.AspNetCore.Http.CookieBuilder.IsEssential>屬性設定為`true`預設。 當網站訪客未同意資料收集時，允許驗證 cookie。 如需詳細資訊，請參閱 <xref:security/gdpr#essential-cookies>。

在 `Startup.Configure`方法中，呼叫`UseAuthentication`方法來叫用設定驗證中介軟體`HttpContext.User`屬性。 呼叫`UseAuthentication`方法之前呼叫`UseMvcWithDefaultRoute`或`UseMvc`:

[!code-csharp[](cookie/samples/2.x/CookieSample/Startup.cs?name=snippet2)]

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationOptions>類別用來設定驗證提供者選項。

設定`CookieAuthenticationOptions`中的驗證服務組態中`Startup.ConfigureServices`方法：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        ...
    });
```

## <a name="cookie-policy-middleware"></a>Cookie 原則中介軟體

[Cookie 原則中介軟體](xref:Microsoft.AspNetCore.CookiePolicy.CookiePolicyMiddleware)啟用 cookie 原則功能。 將中介軟體新增至應用程式處理管線是區分順序&mdash;它只會影響在管線中的已註冊的下游元件。

```csharp
app.UseCookiePolicy(cookiePolicyOptions);
```

使用<xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions>Cookie 原則中介軟體，來控制全域性質的 cookie 處理和攔截到 cookie 處理處理常式，當您附加或刪除 cookie 時提供。

預設值<xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy>值是`SameSiteMode.Lax`允許 OAuth2 驗證。 嚴格強制執行的相同站台原則`SameSiteMode.Strict`，將`MinimumSameSitePolicy`。 雖然 OAuth2 和其他跨原始來源的驗證配置，這項設定會中斷，它的權限提高 cookie 的用戶端進行跨原始來源要求處理的應用程式的其他類型的安全性層級。

```csharp
var cookiePolicyOptions = new CookiePolicyOptions
{
    MinimumSameSitePolicy = SameSiteMode.Strict,
};
```

Cookie 原則中介軟體設定`MinimumSameSitePolicy`可能會影響的設定`Cookie.SameSite`在`CookieAuthenticationOptions`根據下面的矩陣圖的設定。

| MinimumSameSitePolicy | Cookie.SameSite | 結果 Cookie.SameSite 設定 |
| --------------------- | --------------- | --------------------------------- |
| SameSiteMode.None     | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Lax      | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Lax<br>SameSiteMode.Lax<br>SameSiteMode.Strict |
| SameSiteMode.Strict   | SameSiteMode.None<br>SameSiteMode.Lax<br>SameSiteMode.Strict | SameSiteMode.Strict<br>SameSiteMode.Strict<br>SameSiteMode.Strict |

## <a name="create-an-authentication-cookie"></a>建立驗證 cookie

若要建立 cookie 保留使用者資訊，請建構<xref:System.Security.Claims.ClaimsPrincipal>。 使用者資訊會序列化並儲存在 cookie 中。 

建立<xref:System.Security.Claims.ClaimsIdentity>任何具有必要<xref:System.Security.Claims.Claim>s 和呼叫<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignInAsync*>使用者來登入：

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet1)]

`SignInAsync` 建立加密的 cookie，並將它新增至目前回應。 如果`AuthenticationScheme`未指定，會使用預設配置。

ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection)系統用於加密。 裝載多部電腦上的應用程式，負載平衡的應用程式，或使用 web 伺服陣列[設定資料保護](xref:security/data-protection/configuration/overview)使用相同的金鑰環及應用程式識別碼。

## <a name="sign-out"></a>登出

若要將目前的使用者登出並刪除其 cookie，呼叫<xref:Microsoft.AspNetCore.Authentication.AuthenticationHttpContextExtensions.SignOutAsync*>:

[!code-csharp[](cookie/samples/2.x/CookieSample/Pages/Account/Login.cshtml.cs?name=snippet2)]

如果`CookieAuthenticationDefaults.AuthenticationScheme`（或 [Cookie]） 不做為配置 (例如，"ContosoCookie 」)，提供設定的驗證提供者時所使用的配置。 否則，會使用預設的配置。

## <a name="react-to-back-end-changes"></a>回應後端的變更

一旦建立 cookie，cookie 就會是身分識別的單一來源。 如果使用者帳戶已停用後端系統中：

* 應用程式的 cookie 驗證系統會繼續處理要求的驗證 cookie 為基礎。
* 只要驗證 cookie 為有效，則使用者會保持登入應用程式。

<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents.ValidatePrincipal*>事件可用來攔截和覆寫 cookie 身分識別的驗證。 驗證每個要求的 cookie，可降低已撤銷使用者存取應用程式的風險。

Cookie 驗證的其中一個方法根據追蹤的使用者資料庫變更時。 如果資料庫沒有已變更，因為使用者的 cookie 的發出，則不需要重新驗證使用者，如果其 cookie 仍然有效。 在範例應用程式中實作資料庫`IUserRepository`，並將儲存`LastChanged`值。 在資料庫中，更新使用者時`LastChanged`值設定為目前的時間。

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

若要實作的覆寫`ValidatePrincipal`事件，一種方法具有下列簽章，衍生自的類別中的寫入<xref:Microsoft.AspNetCore.Authentication.Cookies.CookieAuthenticationEvents>:

```csharp
ValidatePrincipal(CookieValidatePrincipalContext)
```

以下是範例實作`CookieAuthenticationEvents`:

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

在 cookie 中的服務註冊期間註冊的事件執行個體`Startup.ConfigureServices`方法。 提供[範圍服務登錄](xref:fundamentals/dependency-injection#service-lifetimes)針對您`CustomCookieAuthenticationEvents`類別：

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.EventsType = typeof(CustomCookieAuthenticationEvents);
    });

services.AddScoped<CustomCookieAuthenticationEvents>();
```

更新的使用者名稱的情況下，請考慮&mdash;並不會影響以任何方式的安全性決策。 如果您想要非破壞性的方式更新使用者主體，呼叫`context.ReplacePrincipal`並設定`context.ShouldRenew`屬性設`true`。

> [!WARNING]
> 此處所述的方法是在每個要求時觸發。 驗證每個要求上的所有使用者的驗證 cookie，可能會導致應用程式對大量的效能產生負面影響。

## <a name="persistent-cookies"></a>永續性 cookie

您可能想要在瀏覽器工作階段之間保存的 cookie。 這個持續性，才應該啟用明確的使用者同意的情況與 「 還記得我 」 核取方塊在登入或類似的機制。 

下列程式碼片段會建立身分識別和對應的 cookie，透過瀏覽器結束時仍然有效。 會遵守任何先前設定的滑動逾期設定。 如果 cookie 已過期的瀏覽器關閉時，瀏覽器在重新啟動之後，就會清除的 cookie。

設定<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.IsPersistent>要`true`在<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties>:

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

## <a name="absolute-cookie-expiration"></a>絕對的 cookie 到期日

可以使用設定絕對到期時間<xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties.ExpiresUtc>。 若要建立永續性 cookie，`IsPersistent`也必須設定。 否則，cookie 會建立具有工作階段為基礎的存留期，而且可能過期之前或之後，它會保存驗證票證。 當`ExpiresUtc`設定，它會覆寫的值<xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.ExpireTimeSpan>選擇<xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions>，如果設定。

下列程式碼片段會建立身分識別和對應的 cookie 可持續 20 分鐘的時間。 這會忽略任何先前設定的滑動逾期設定。

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

## <a name="additional-resources"></a>其他資源

* <xref:security/authorization/limitingidentitybyscheme>
* <xref:security/authorization/claims>
* [以原則為基礎的角色檢查](xref:security/authorization/roles#policy-based-role-checks)
* <xref:host-and-deploy/web-farm>
