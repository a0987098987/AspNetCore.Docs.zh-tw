---
title: "使用沒有 ASP.NET Core 身分識別的 Cookie 驗證"
author: rick-anderson
description: "需使用沒有 ASP.NET Core 身分識別的 cookie 驗證的說明"
keywords: ASP.NET Core cookie
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: 2bdcbf95-8d9d-4537-a4a0-a5ee439dcb62
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/cookie
ms.openlocfilehash: 60ac318cb47b5a5b4c651c88e60d43772ce59958
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a>使用沒有 ASP.NET Core 身分識別的 Cookie 驗證

<a name="security-authentication-cookie-middleware"></a>

ASP.NET Core 1.x 提供 cookie[中介軟體](../../fundamentals/middleware.md#fundamentals-middleware)的序列化經過加密的 cookie 的使用者主體，然後在後續要求中，驗證 cookie，會重新建立主體，並將其指派給`HttpContext.User`屬性. 如果您想要提供您自己的登入畫面和使用者資料庫，您可以使用 cookie 中介軟體為獨立的功能。

一項重大變更在 ASP.NET Core 2.x 是 cookie 中介軟體不存在。 相反地，`UseAuthentication`方法引動過程中的`Configure`方法*Startup.cs*新增可設定 AuthenticationMiddleware`HttpContext.User`屬性。

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a>加入和設定

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

完成下列步驟：

- 如果不使用`Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage)，安裝新版 2.0 +`Microsoft.AspNetCore.Authentication.Cookies`專案中的 NuGet 封裝。

- 叫用`UseAuthentication`方法中的`Configure`方法*Startup.cs*檔案：

    ```csharp
    app.UseAuthentication();
    ```

- 叫用`AddAuthentication`和`AddCookie`方法`ConfigureServices`方法*Startup.cs*檔案：

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie(options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

完成下列步驟：

- 安裝`Microsoft.AspNetCore.Authentication.Cookies`專案中的 NuGet 封裝。 此套件包含 cookie 中介軟體。

- 加入下列行以`Configure`方法在您*Startup.cs*檔案之後，再`app.UseMvc()`陳述式：

    ```csharp
    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AccessDeniedPath = "/Account/Forbidden/",
        AuthenticationScheme = "MyCookieAuthenticationScheme",
        AutomaticAuthenticate = true,
        AutomaticChallenge = true,
        LoginPath = "/Account/Unauthorized/"
    });
    ```

---

上述的程式碼片段設定部分或全部的下列選項：

* `AccessDeniedPath`-這是相對路徑，要求重新導向當使用者嘗試存取資源，但未通過任何[授權原則](xref:security/authorization/policies#security-authorization-policies-based)該資源。

* `AuthenticationScheme`-這是特定的 cookie 驗證配置會依已知的值。 當多個執行個體的 cookie 驗證，而您想要這非常有用[授權限制為一個執行個體](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme)。

* `AutomaticAuthenticate`-此旗標是只適用於 ASP.NET Core 1.x。 它會指出應該執行每個要求的 cookie 驗證，並嘗試驗證，重新建構建立它的任何序列化的主體。

* `AutomaticChallenge`-此旗標是只適用於 ASP.NET Core 1.x。 它會指出 1.x 的 cookie 驗證應該重新導向的瀏覽器`LoginPath`或`AccessDeniedPath`授權就會失敗。

* `LoginPath`-這是要要求重新導向使用者嘗試存取資源，但尚未經過驗證時的相對路徑。

[其他選項](xref:security/authentication/cookie#security-authentication-cookie-options)包含了可設定的 cookie 驗證建立時，任何宣告的簽發者名稱的 cookie 驗證卸除的 cookie 並在 cookie 上各種不同的安全性內容的網域。 根據預設的 cookie 驗證會使用適當的安全性選項的任何 cookie，它會建立，例如：
- 設定以防止在用戶端 JavaScript 中的 cookie 存取 HttpOnly 旗標
- 如果要求已透過 HTTPS 旅行，限制 HTTPS 的 cookie

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a>建立了身分識別的 cookie。

若要建立保留您的使用者資訊的 cookie，您必須建構[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)保存您想要序列化在 cookie 中的資訊。 一旦您擁有適當`ClaimsPrincipal`物件，呼叫下列控制器方法內：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

這會建立加密的 cookie，並將它加入至目前回應。 `AuthenticationScheme`期間指定[組態](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring)呼叫時，必須使用`SignInAsync`。

實際上，使用的加密是 ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系統。 如果您正在裝載多部電腦，負載平衡，或使用 web 伺服陣列上，則您需要[設定資料保護](xref:security/data-protection/configuration/overview#data-protection-configuring)使用相同的索引鍵環形與應用程式識別碼。

## <a name="signing-out"></a>登出

若要登出目前的使用者，並刪除其 cookie，呼叫下列您的控制器內：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a>對回應後端的變更

>[!WARNING]
> 一旦建立主體的 cookie 之後，它會變成身分識別的單一來源。 即使您停用使用者後端系統中，cookie 驗證並不了解，以及使用者保持登入，只要其 cookie 為有效。

Cookie 驗證提供一系列的選項類別中的事件。 `ValidateAsync()`事件可以用來攔截並覆寫 cookie 身分識別驗證。

請考慮可能會有"LastChanged 」 資料行的後端使用者資料庫。 若要讓 cookie 失效資料庫變更時，您應該先，當[建立 cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)，新增 「 LastChanged"宣告，包含目前的值。 當資料庫變更時，應該更新 「 LastChanged"值。

若要實作的覆寫`ValidateAsync()`事件，您必須撰寫具有下列簽章的方法：

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

ASP.NET Core 身分識別的一部分實作這項檢查其`SecurityStampValidator`。 範例如下所示：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

這會接著會固定在 cookie 中的服務登錄期間`ConfigureServices`方法*Startup.cs*:

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie(options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
public static class LastChangedValidator
{
    public static async Task ValidateAsync(CookieValidatePrincipalContext context)
    {
        // Pull database from registered DI services.
        var userRepository = context.HttpContext.RequestServices.GetRequiredService<IUserRepository>();
        var userPrincipal = context.Principal;

        // Look for the last changed claim.
        string lastChanged;
        lastChanged = (from c in userPrincipal.Claims
                        where c.Type == "LastUpdated"
                        select c.Value).FirstOrDefault();

        if (string.IsNullOrEmpty(lastChanged) ||
            !userRepository.ValidateLastChanged(userPrincipal, lastChanged))
        {
            context.RejectPrincipal();
            await context.HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
        }
    }
}
```

這會接著會固定在 cookie 驗證組態設定期間`Configure`方法*Startup.cs*:

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

在更新其名稱的範例，請考慮&mdash;並不會影響安全性，以任何方式的決策。 如果您想要非破壞性的方式更新使用者的主體，您可以呼叫`context.ReplacePrincipal()`並設定`context.ShouldRenew`屬性`true`。

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a>控制 cookie 選項

[CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)類別隨附微調所建立的 cookie 的各種組態選項。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

2.x 統一的 Api 可用來設定 cookie 的 ASP.NET Core。 應用程式開發介面已標示為過時，1.x 和新`Cookie`型別的屬性`CookieBuilder`已導入`CookieAuthenticationOptions`類別。 建議您將移轉至 2.x 應用程式開發介面。

* `ClaimsIssuer`是要用於簽發者[簽發者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)cookie 驗證所建立的任何宣告上的屬性。

* `CookieBuilder.Domain`是要提供 cookie 的網域名稱。 根據預設，這是要求傳送至的主機名稱。 瀏覽器，只是要比對的主機名稱的 cookie。 您可能想要調整這在您的網域中有任何主機可用的 cookie。 例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，`staging.www.contoso.com`等等。

* `CookieBuilder.HttpOnly`這旗標，表示是否 cookie 應該只能由伺服器存取。 預設值為`true`。 變更此值可能會開啟 cookie 竊取您的應用程式應您的應用程式有跨網站指令碼處理的 bug。

* `CookieBuilder.Path`可用來隔離應用程式相同的主機名稱上執行。 如果您有執行的應用程式`/app1`而且想要限制的 cookie 發行給只會傳送到該應用程式，則您應該將`CookiePath`屬性`/app1`。 如此一來，cookie 才可用來要求`/app1`或其下的任何項目。

* `CookieBuilder.SameSite`表示瀏覽器是否應該允許要附加至相同站台或跨網站要求的 cookie。 預設值為`SameSiteMode.Lax`。

* `CookieBuilder.SecurePolicy`這一個旗標，表示是否建立的 cookie 會受限於 HTTPS、 HTTP 或 HTTPS 或為要求相同的通訊協定。 預設值為`SameAsRequest`。

* `ExpireTimeSpan`是`TimeSpan`之後 cookie 已過期。 它會加入至目前的日期和時間，若要建立 cookie 的到期日。

* `SlidingExpiration`指出是否 cookie 的到期日重設時超過一半的旗標`ExpireTimeSpan`間隔。 新的到期日向前移動到目前日期加上`ExpireTimespan`。 [絕對到期時間](xref:security/authentication/cookie#security-authentication-absolute-expiry)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。 絕對到期可以改善應用程式的安全性限制的驗證 cookie 為有效的時間量。

示範如何使用`CookieAuthenticationOptions`中`ConfigureServices`方法*Startup.cs*遵循：

```csharp
services.AddAuthentication()
        .AddCookie(options =>
        {
            options.Cookie.Name = "AuthCookie";
            options.Cookie.Domain = "contoso.com";
            options.Cookie.Path = "/";
            options.Cookie.HttpOnly = true;
            options.Cookie.SameSite = SameSiteMode.Lax;
            options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

* `ClaimsIssuer`是要用於簽發者[簽發者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)上任何中介軟體所建立的宣告屬性。

* `CookieDomain`是要提供 cookie 的網域名稱。 根據預設，這是要求傳送至的主機名稱。 瀏覽器，只是要比對的主機名稱的 cookie。 您可能想要調整這在您的網域中有任何主機可用的 cookie。 例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，`staging.www.contoso.com`等等。

* `CookieHttpOnly`這旗標，表示是否 cookie 應該只能由伺服器存取。 預設值為`true`。 變更此值可能會開啟 cookie 竊取您的應用程式應您的應用程式有跨網站指令碼處理的 bug。

* `CookiePath`可用來隔離應用程式相同的主機名稱上執行。 如果您有執行的應用程式`/app1`而且想要限制的 cookie 發行給只會傳送到該應用程式，則您應該將`CookiePath`屬性`/app1`。 如此一來，cookie 才可用來要求`/app1`或其下的任何項目。

* `CookieSecure`這一個旗標，表示是否建立的 cookie 會受限於 HTTPS、 HTTP 或 HTTPS 或為要求相同的通訊協定。 預設值為`SameAsRequest`。

* `ExpireTimeSpan`是`TimeSpan`之後 cookie 已過期。 它會加入至目前的日期和時間，若要建立 cookie 的到期日。

* `SlidingExpiration`指出是否 cookie 的到期日重設時超過一半的旗標`ExpireTimeSpan`間隔。 新的到期日向前移動到目前日期加上`ExpireTimespan`。 [絕對到期時間](xref:security/authentication/cookie#security-authentication-absolute-expiry)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。 絕對到期可以改善應用程式的安全性限制的驗證 cookie 為有效的時間量。

示範如何使用`CookieAuthenticationOptions`中`Configure`方法*Startup.cs*遵循：

```csharp
app.UseCookieAuthentication(new CookieAuthenticationOptions
{
    CookieName = "AuthCookie",
    CookieDomain = "contoso.com",
    CookiePath = "/",
    CookieHttpOnly = true,
    CookieSecure = CookieSecurePolicy.Always
});
```

---

## <a name="persistent-cookies-and-absolute-expiry-times"></a>永續性 cookie 和絕對到期時間

您可能想要在瀏覽器工作階段之間保存，並識別和 cookie 傳輸它絕對到期 cookie 到期。 此持續性，才應該啟用明確的使用者同意的情況下，透過 「 記住我 核取方塊上登入或類似的機制。 您可以執行下列步驟使用`AuthenticationProperties`參數`SignInAsync`時呼叫方法[登入身分識別，以及建立 cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)。 例如: 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`AuthenticationProperties`在上述程式碼片段中，所使用的類別位於`Microsoft.AspNetCore.Authentication`命名空間。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

`AuthenticationProperties`在上述程式碼片段中，所使用的類別位於`Microsoft.AspNetCore.Http.Authentication`命名空間。

---

上述程式碼片段會建立身分識別和對應 cookie 未透過瀏覽器封閉區段。 任何透過先前設定的滑動逾期設定[cookie 選項](xref:security/authentication/cookie#security-authentication-cookie-options)被仍接受。 如果 cookie 已過期而關閉瀏覽器，瀏覽器之後重新啟動時清除它。

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

---

上述程式碼片段會建立身分識別和對應的 cookie 可延續 20 分鐘。 這會忽略之前透過設定任何滑動逾期設定[cookie 選項](xref:security/authentication/cookie#security-authentication-cookie-options)。

`ExpiresUtc`和`IsPersistent`屬性互斥。