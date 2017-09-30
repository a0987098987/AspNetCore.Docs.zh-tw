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
ms.openlocfilehash: af3ffe418521d5d97f5d14ca9c904c21b4d4ff89
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="using-cookie-authentication-without-aspnet-core-identity"></a><span data-ttu-id="4aed2-104">使用沒有 ASP.NET Core 身分識別的 Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="4aed2-104">Using Cookie Authentication without ASP.NET Core Identity</span></span>

<a name="security-authentication-cookie-middleware"></a>

<span data-ttu-id="4aed2-105">ASP.NET Core 1.x 提供 cookie[中介軟體](../../fundamentals/middleware.md#fundamentals-middleware)的序列化經過加密的 cookie 的使用者主體，然後在後續要求中，驗證 cookie，會重新建立主體，並將其指派給`HttpContext.User`屬性.</span><span class="sxs-lookup"><span data-stu-id="4aed2-105">ASP.NET Core 1.x provides cookie [middleware](../../fundamentals/middleware.md#fundamentals-middleware) which serializes a user principal into an encrypted cookie and then, on subsequent requests, validates the cookie, recreates the principal, and assigns it to the `HttpContext.User` property.</span></span> <span data-ttu-id="4aed2-106">如果您想要提供您自己的登入畫面和使用者資料庫，您可以使用 cookie 中介軟體為獨立的功能。</span><span class="sxs-lookup"><span data-stu-id="4aed2-106">If you want to provide your own login screens and user databases, you can use the cookie middleware as a standalone feature.</span></span>

<span data-ttu-id="4aed2-107">一項重大變更在 ASP.NET Core 2.x 是 cookie 中介軟體不存在。</span><span class="sxs-lookup"><span data-stu-id="4aed2-107">A major change in ASP.NET Core 2.x is that the cookie middleware is absent.</span></span> <span data-ttu-id="4aed2-108">相反地，`UseAuthentication`方法引動過程中的`Configure`方法*Startup.cs*新增可設定 AuthenticationMiddleware`HttpContext.User`屬性。</span><span class="sxs-lookup"><span data-stu-id="4aed2-108">Instead, the `UseAuthentication` method invocation in the `Configure` method of *Startup.cs* adds the AuthenticationMiddleware which sets the `HttpContext.User` property.</span></span>

<a name="security-authentication-cookie-middleware-configuring"></a>

## <a name="adding-and-configuring"></a><span data-ttu-id="4aed2-109">加入和設定</span><span class="sxs-lookup"><span data-stu-id="4aed2-109">Adding and configuring</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4aed2-111">完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="4aed2-111">Complete the following steps:</span></span>

- <span data-ttu-id="4aed2-112">如果不使用`Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage)，安裝新版 2.0 +`Microsoft.AspNetCore.Authentication.Cookies`專案中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="4aed2-112">If not using the `Microsoft.AspNetCore.All` [metapackage](xref:fundamentals/metapackage), install version 2.0+ of the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span>

- <span data-ttu-id="4aed2-113">叫用`UseAuthentication`方法中的`Configure`方法*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="4aed2-113">Invoke the `UseAuthentication` method in the `Configure` method of the *Startup.cs* file:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4aed2-114">叫用`AddAuthentication`和`AddCookie`方法`ConfigureServices`方法*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="4aed2-114">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method of the *Startup.cs* file:</span></span>

    ```csharp
    services.AddAuthentication("MyCookieAuthenticationScheme")
            .AddCookie("MyCookieAuthenticationScheme", options => {
                options.AccessDeniedPath = "/Account/Forbidden/";
                options.LoginPath = "/Account/Unauthorized/";
            });
    ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4aed2-116">完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="4aed2-116">Complete the following steps:</span></span>

- <span data-ttu-id="4aed2-117">安裝`Microsoft.AspNetCore.Authentication.Cookies`專案中的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="4aed2-117">Install the `Microsoft.AspNetCore.Authentication.Cookies` NuGet package in your project.</span></span> <span data-ttu-id="4aed2-118">此套件包含 cookie 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="4aed2-118">This package contains the cookie middleware.</span></span>

- <span data-ttu-id="4aed2-119">加入下列行以`Configure`方法在您*Startup.cs*檔案之後，再`app.UseMvc()`陳述式：</span><span class="sxs-lookup"><span data-stu-id="4aed2-119">Add the following lines to the `Configure` method in your *Startup.cs* file before the `app.UseMvc()` statement:</span></span>

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

<span data-ttu-id="4aed2-120">上述的程式碼片段設定部分或全部的下列選項：</span><span class="sxs-lookup"><span data-stu-id="4aed2-120">The code snippets above configure some or all of the following options:</span></span>

* <span data-ttu-id="4aed2-121">`AccessDeniedPath`-這是相對路徑，要求重新導向當使用者嘗試存取資源，但未通過任何[授權原則](xref:security/authorization/policies#security-authorization-policies-based)該資源。</span><span class="sxs-lookup"><span data-stu-id="4aed2-121">`AccessDeniedPath` - This is the relative path to which requests redirect when a user attempts to access a resource but does not pass any [authorization policies](xref:security/authorization/policies#security-authorization-policies-based) for that resource.</span></span>

* <span data-ttu-id="4aed2-122">`AuthenticationScheme`-這是特定的 cookie 驗證配置會依已知的值。</span><span class="sxs-lookup"><span data-stu-id="4aed2-122">`AuthenticationScheme` - This is a value by which a particular cookie authentication scheme is known.</span></span> <span data-ttu-id="4aed2-123">當多個執行個體的 cookie 驗證，而您想要這非常有用[授權限制為一個執行個體](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme)。</span><span class="sxs-lookup"><span data-stu-id="4aed2-123">This is useful when there are multiple instances of cookie authentication and you want to [limit authorization to one instance](xref:security/authorization/limitingidentitybyscheme#security-authorization-limiting-by-scheme).</span></span>

* <span data-ttu-id="4aed2-124">`AutomaticAuthenticate`-此旗標是只適用於 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="4aed2-124">`AutomaticAuthenticate` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="4aed2-125">它會指出應該執行每個要求的 cookie 驗證，並嘗試驗證，重新建構建立它的任何序列化的主體。</span><span class="sxs-lookup"><span data-stu-id="4aed2-125">It indicates that the cookie authentication should run on every request and attempt to validate and reconstruct any serialized principal it created.</span></span>

* <span data-ttu-id="4aed2-126">`AutomaticChallenge`-此旗標是只適用於 ASP.NET Core 1.x。</span><span class="sxs-lookup"><span data-stu-id="4aed2-126">`AutomaticChallenge` - This flag is relevant only for ASP.NET Core 1.x.</span></span> <span data-ttu-id="4aed2-127">它會指出 1.x 的 cookie 驗證應該重新導向的瀏覽器`LoginPath`或`AccessDeniedPath`授權就會失敗。</span><span class="sxs-lookup"><span data-stu-id="4aed2-127">It indicates that the 1.x cookie authentication should redirect the browser to the `LoginPath` or `AccessDeniedPath` when authorization fails.</span></span>

* <span data-ttu-id="4aed2-128">`LoginPath`-這是要要求重新導向使用者嘗試存取資源，但尚未經過驗證時的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="4aed2-128">`LoginPath` - This is the relative path to which requests redirect when a user attempts to access a resource but has not been authenticated.</span></span>

<span data-ttu-id="4aed2-129">[其他選項](xref:security/authentication/cookie#security-authentication-cookie-options)包含了可設定的 cookie 驗證建立時，任何宣告的簽發者名稱的 cookie 驗證卸除的 cookie 並在 cookie 上各種不同的安全性內容的網域。</span><span class="sxs-lookup"><span data-stu-id="4aed2-129">[Other options](xref:security/authentication/cookie#security-authentication-cookie-options) include the ability to set the issuer for any claims the cookie authentication creates, the name of the cookie the authentication drops, the domain for the cookie and various security properties on the cookie.</span></span> <span data-ttu-id="4aed2-130">根據預設的 cookie 驗證會使用適當的安全性選項的任何 cookie，它會建立，例如：</span><span class="sxs-lookup"><span data-stu-id="4aed2-130">By default, the cookie authentication uses appropriate security options for any cookies it creates, such as:</span></span>
- <span data-ttu-id="4aed2-131">設定以防止在用戶端 JavaScript 中的 cookie 存取 HttpOnly 旗標</span><span class="sxs-lookup"><span data-stu-id="4aed2-131">Setting the HttpOnly flag to prevent cookie access in client-side JavaScript</span></span>
- <span data-ttu-id="4aed2-132">如果要求已透過 HTTPS 旅行，限制 HTTPS 的 cookie</span><span class="sxs-lookup"><span data-stu-id="4aed2-132">Limiting the cookie to HTTPS if a request has traveled over HTTPS</span></span>

<a name="security-authentication-cookie-middleware-creating-a-cookie"></a>

## <a name="creating-an-identity-cookie"></a><span data-ttu-id="4aed2-133">建立了身分識別的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4aed2-133">Creating an Identity cookie</span></span>

<span data-ttu-id="4aed2-134">若要建立保留您的使用者資訊的 cookie，您必須建構[ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal)保存您想要序列化在 cookie 中的資訊。</span><span class="sxs-lookup"><span data-stu-id="4aed2-134">To create a cookie holding your user information, you must construct a [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) holding the information you wish to be serialized in the cookie.</span></span> <span data-ttu-id="4aed2-135">一旦您擁有適當`ClaimsPrincipal`物件，呼叫下列控制器方法內：</span><span class="sxs-lookup"><span data-stu-id="4aed2-135">Once you have a suitable `ClaimsPrincipal` object, call the following inside your controller method:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-136">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-136">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync("MyCookieAuthenticationScheme", principal);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync("MyCookieAuthenticationScheme", principal);
```

---

<span data-ttu-id="4aed2-138">這會建立加密的 cookie，並將它加入至目前回應。</span><span class="sxs-lookup"><span data-stu-id="4aed2-138">This creates an encrypted cookie and adds it to the current response.</span></span> <span data-ttu-id="4aed2-139">`AuthenticationScheme`期間指定[組態](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring)呼叫時，必須使用`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-139">The `AuthenticationScheme` specified during [configuration](xref:security/authentication/cookie#security-authentication-cookie-middleware-configuring) must be used when calling `SignInAsync`.</span></span>

<span data-ttu-id="4aed2-140">實際上，使用的加密是 ASP.NET Core[資料保護](xref:security/data-protection/using-data-protection#security-data-protection-getting-started)系統。</span><span class="sxs-lookup"><span data-stu-id="4aed2-140">Under the covers, the encryption used is ASP.NET Core's [Data Protection](xref:security/data-protection/using-data-protection#security-data-protection-getting-started) system.</span></span> <span data-ttu-id="4aed2-141">如果您正在裝載多部電腦，負載平衡，或使用 web 伺服陣列上，則您需要[設定資料保護](xref:security/data-protection/configuration/overview#data-protection-configuring)使用相同的索引鍵環形與應用程式識別碼。</span><span class="sxs-lookup"><span data-stu-id="4aed2-141">If you are hosting on multiple machines, load balancing, or using a web farm, then you need to [configure data protection](xref:security/data-protection/configuration/overview#data-protection-configuring) to use the same key ring and application identifier.</span></span>

## <a name="signing-out"></a><span data-ttu-id="4aed2-142">登出</span><span class="sxs-lookup"><span data-stu-id="4aed2-142">Signing out</span></span>

<span data-ttu-id="4aed2-143">若要登出目前的使用者，並刪除其 cookie，呼叫下列您的控制器內：</span><span class="sxs-lookup"><span data-stu-id="4aed2-143">To sign out the current user and delete their cookie, call the following inside your controller:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignOutAsync("MyCookieAuthenticationScheme");
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-145">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-145">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignOutAsync("MyCookieAuthenticationScheme");
```

---

## <a name="reacting-to-back-end-changes"></a><span data-ttu-id="4aed2-146">對回應後端的變更</span><span class="sxs-lookup"><span data-stu-id="4aed2-146">Reacting to back-end changes</span></span>

>[!WARNING]
> <span data-ttu-id="4aed2-147">一旦建立主體的 cookie 之後，它會變成身分識別的單一來源。</span><span class="sxs-lookup"><span data-stu-id="4aed2-147">Once a principal cookie has been created, it becomes the single source of identity.</span></span> <span data-ttu-id="4aed2-148">即使您停用使用者後端系統中，cookie 驗證並不了解，以及使用者保持登入，只要其 cookie 為有效。</span><span class="sxs-lookup"><span data-stu-id="4aed2-148">Even if you disable a user in your back-end systems, the cookie authentication has no knowledge of this, and a user stays logged in as long as their cookie is valid.</span></span>

<span data-ttu-id="4aed2-149">Cookie 驗證提供一系列的選項類別中的事件。</span><span class="sxs-lookup"><span data-stu-id="4aed2-149">The cookie authentication provides a series of events in its option class.</span></span> <span data-ttu-id="4aed2-150">`ValidateAsync()`事件可以用來攔截並覆寫 cookie 身分識別驗證。</span><span class="sxs-lookup"><span data-stu-id="4aed2-150">The `ValidateAsync()` event can be used to intercept and override validation of the cookie identity.</span></span>

<span data-ttu-id="4aed2-151">請考慮可能會有"LastChanged 」 資料行的後端使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="4aed2-151">Consider a back-end user database that may have a "LastChanged" column.</span></span> <span data-ttu-id="4aed2-152">若要讓 cookie 失效資料庫變更時，您應該先，當[建立 cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)，新增 「 LastChanged"宣告，包含目前的值。</span><span class="sxs-lookup"><span data-stu-id="4aed2-152">In order to invalidate a cookie when the database changes, you should first, when [creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie), add a "LastChanged" claim containing the current value.</span></span> <span data-ttu-id="4aed2-153">當資料庫變更時，應該更新 「 LastChanged"值。</span><span class="sxs-lookup"><span data-stu-id="4aed2-153">When the database changes, the "LastChanged" value should be updated.</span></span>

<span data-ttu-id="4aed2-154">若要實作的覆寫`ValidateAsync()`事件，您必須撰寫具有下列簽章的方法：</span><span class="sxs-lookup"><span data-stu-id="4aed2-154">To implement an override for the `ValidateAsync()` event, you must write a method with the following signature:</span></span>

```csharp
Task ValidateAsync(CookieValidatePrincipalContext context);
```

<span data-ttu-id="4aed2-155">ASP.NET Core 身分識別的一部分實作這項檢查其`SecurityStampValidator`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-155">ASP.NET Core Identity implements this check as part of its `SecurityStampValidator`.</span></span> <span data-ttu-id="4aed2-156">範例如下所示：</span><span class="sxs-lookup"><span data-stu-id="4aed2-156">An example looks like the following:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-157">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-157">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

<span data-ttu-id="4aed2-158">這會接著會固定在 cookie 中的服務登錄期間`ConfigureServices`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aed2-158">This would then be wired up during cookie service registration in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication("MyCookieAuthenticationScheme")
        .AddCookie("MyCookieAuthenticationScheme", options =>
        {
            options.Events = new CookieAuthenticationEvents
            {
                OnValidatePrincipal = LastChangedValidator.ValidateAsync
            };
        });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="4aed2-160">這會接著會固定在 cookie 驗證組態設定期間`Configure`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4aed2-160">This would then be wired up during cookie authentication configuration in the `Configure` method of *Startup.cs*:</span></span>

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

<span data-ttu-id="4aed2-161">在更新其名稱的範例，請考慮&mdash;並不會影響安全性，以任何方式的決策。</span><span class="sxs-lookup"><span data-stu-id="4aed2-161">Consider the example in which their name has been updated &mdash; a decision which doesn't affect security in any way.</span></span> <span data-ttu-id="4aed2-162">如果您想要非破壞性的方式更新使用者的主體，您可以呼叫`context.ReplacePrincipal()`並設定`context.ShouldRenew`屬性`true`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-162">If you want to non-destructively update the user principal, you can call `context.ReplacePrincipal()` and set the `context.ShouldRenew` property to `true`.</span></span>

<a name="security-authentication-cookie-options"></a>

## <a name="controlling-cookie-options"></a><span data-ttu-id="4aed2-163">控制 cookie 選項</span><span class="sxs-lookup"><span data-stu-id="4aed2-163">Controlling cookie options</span></span>

<span data-ttu-id="4aed2-164">[CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions)類別隨附微調所建立的 cookie 的各種組態選項。</span><span class="sxs-lookup"><span data-stu-id="4aed2-164">The [CookieAuthenticationOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.cookieauthenticationoptions) class comes with various configuration options to fine-tune the cookies being created.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-165">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-165">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4aed2-166">2.x 統一的 Api 可用來設定 cookie 的 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="4aed2-166">ASP.NET Core 2.x unifies the APIs used for configuring cookies.</span></span> <span data-ttu-id="4aed2-167">應用程式開發介面已標示為過時，1.x 和新`Cookie`型別的屬性`CookieBuilder`已導入`CookieAuthenticationOptions`類別。</span><span class="sxs-lookup"><span data-stu-id="4aed2-167">The 1.x APIs have been marked as obsolete, and a new `Cookie` property of type `CookieBuilder` has been introduced in the `CookieAuthenticationOptions` class.</span></span> <span data-ttu-id="4aed2-168">建議您將移轉至 2.x 應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="4aed2-168">It's recommended that you migrate to the 2.x APIs.</span></span>

* <span data-ttu-id="4aed2-169">`ClaimsIssuer`是要用於簽發者[簽發者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)cookie 驗證所建立的任何宣告上的屬性。</span><span class="sxs-lookup"><span data-stu-id="4aed2-169">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the cookie authentication.</span></span>

* <span data-ttu-id="4aed2-170">`CookieBuilder.Domain`是要提供 cookie 的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="4aed2-170">`CookieBuilder.Domain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="4aed2-171">根據預設，這是要求傳送至的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4aed2-171">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="4aed2-172">瀏覽器，只是要比對的主機名稱的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4aed2-172">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="4aed2-173">您可能想要調整這在您的網域中有任何主機可用的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4aed2-173">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="4aed2-174">例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，`staging.www.contoso.com`等等。</span><span class="sxs-lookup"><span data-stu-id="4aed2-174">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="4aed2-175">`CookieBuilder.HttpOnly`這旗標，表示是否 cookie 應該只能由伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="4aed2-175">`CookieBuilder.HttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="4aed2-176">預設值為`true`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-176">This defaults to `true`.</span></span> <span data-ttu-id="4aed2-177">變更此值可能會開啟 cookie 竊取您的應用程式應您的應用程式有跨網站指令碼處理的 bug。</span><span class="sxs-lookup"><span data-stu-id="4aed2-177">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="4aed2-178">`CookieBuilder.Path`可用來隔離應用程式相同的主機名稱上執行。</span><span class="sxs-lookup"><span data-stu-id="4aed2-178">`CookieBuilder.Path` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="4aed2-179">如果您有執行的應用程式`/app1`而且想要限制的 cookie 發行給只會傳送到該應用程式，則您應該將`CookiePath`屬性`/app1`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-179">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="4aed2-180">如此一來，cookie 才可用來要求`/app1`或其下的任何項目。</span><span class="sxs-lookup"><span data-stu-id="4aed2-180">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="4aed2-181">`CookieBuilder.SameSite`表示瀏覽器是否應該允許要附加至相同站台或跨網站要求的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4aed2-181">`CookieBuilder.SameSite` indicates whether the browser should allow the cookie to be attached to same-site or cross-site requests.</span></span> <span data-ttu-id="4aed2-182">預設值為`SameSiteMode.Lax`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-182">This defaults to `SameSiteMode.Lax`.</span></span>

* <span data-ttu-id="4aed2-183">`CookieBuilder.SecurePolicy`這一個旗標，表示是否建立的 cookie 會受限於 HTTPS、 HTTP 或 HTTPS 或為要求相同的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="4aed2-183">`CookieBuilder.SecurePolicy` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="4aed2-184">預設值為`SameAsRequest`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-184">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="4aed2-185">`ExpireTimeSpan`是`TimeSpan`之後 cookie 已過期。</span><span class="sxs-lookup"><span data-stu-id="4aed2-185">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="4aed2-186">它會加入至目前的日期和時間，若要建立 cookie 的到期日。</span><span class="sxs-lookup"><span data-stu-id="4aed2-186">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="4aed2-187">`SlidingExpiration`指出是否 cookie 的到期日重設時超過一半的旗標`ExpireTimeSpan`間隔。</span><span class="sxs-lookup"><span data-stu-id="4aed2-187">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="4aed2-188">新的到期日向前移動到目前日期加上`ExpireTimespan`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-188">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="4aed2-189">[絕對到期時間](xref:security/authentication/cookie#security-authentication-absolute-expiry)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-189">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="4aed2-190">絕對到期可以改善應用程式的安全性限制的驗證 cookie 為有效的時間量。</span><span class="sxs-lookup"><span data-stu-id="4aed2-190">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="4aed2-191">示範如何使用`CookieAuthenticationOptions`中`ConfigureServices`方法*Startup.cs*遵循：</span><span class="sxs-lookup"><span data-stu-id="4aed2-191">An example of using `CookieAuthenticationOptions` in the `ConfigureServices` method of *Startup.cs* follows:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="4aed2-193">`ClaimsIssuer`是要用於簽發者[簽發者](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer)上任何中介軟體所建立的宣告屬性。</span><span class="sxs-lookup"><span data-stu-id="4aed2-193">`ClaimsIssuer` is the issuer to be used for the [Issuer](https://docs.microsoft.com/dotnet/api/system.security.claims.claim.issuer) property on any claims created by the middleware.</span></span>

* <span data-ttu-id="4aed2-194">`CookieDomain`是要提供 cookie 的網域名稱。</span><span class="sxs-lookup"><span data-stu-id="4aed2-194">`CookieDomain` is the domain name to which the cookie is served.</span></span> <span data-ttu-id="4aed2-195">根據預設，這是要求傳送至的主機名稱。</span><span class="sxs-lookup"><span data-stu-id="4aed2-195">By default, this is the host name the request was sent to.</span></span> <span data-ttu-id="4aed2-196">瀏覽器，只是要比對的主機名稱的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4aed2-196">The browser only serves the cookie to a matching host name.</span></span> <span data-ttu-id="4aed2-197">您可能想要調整這在您的網域中有任何主機可用的 cookie。</span><span class="sxs-lookup"><span data-stu-id="4aed2-197">You may wish to adjust this to have cookies available to any host in your domain.</span></span> <span data-ttu-id="4aed2-198">例如，若要設定 cookie 網域`.contoso.com`使其可`contoso.com`， `www.contoso.com`，`staging.www.contoso.com`等等。</span><span class="sxs-lookup"><span data-stu-id="4aed2-198">For example, setting the cookie domain to `.contoso.com` makes it available to `contoso.com`, `www.contoso.com`, `staging.www.contoso.com`, etc.</span></span>

* <span data-ttu-id="4aed2-199">`CookieHttpOnly`這旗標，表示是否 cookie 應該只能由伺服器存取。</span><span class="sxs-lookup"><span data-stu-id="4aed2-199">`CookieHttpOnly` is a flag indicating if the cookie should be accessible only to servers.</span></span> <span data-ttu-id="4aed2-200">預設值為`true`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-200">This defaults to `true`.</span></span> <span data-ttu-id="4aed2-201">變更此值可能會開啟 cookie 竊取您的應用程式應您的應用程式有跨網站指令碼處理的 bug。</span><span class="sxs-lookup"><span data-stu-id="4aed2-201">Changing this value may open your application to cookie theft should your application have a Cross-Site Scripting bug.</span></span>

* <span data-ttu-id="4aed2-202">`CookiePath`可用來隔離應用程式相同的主機名稱上執行。</span><span class="sxs-lookup"><span data-stu-id="4aed2-202">`CookiePath` can be used to isolate applications running on the same host name.</span></span> <span data-ttu-id="4aed2-203">如果您有執行的應用程式`/app1`而且想要限制的 cookie 發行給只會傳送到該應用程式，則您應該將`CookiePath`屬性`/app1`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-203">If you have an app running in `/app1` and want to limit the cookies issued to just be sent to that application, then you should set the `CookiePath` property to `/app1`.</span></span> <span data-ttu-id="4aed2-204">如此一來，cookie 才可用來要求`/app1`或其下的任何項目。</span><span class="sxs-lookup"><span data-stu-id="4aed2-204">By doing so, the cookie is only available to requests to `/app1` or anything underneath it.</span></span>

* <span data-ttu-id="4aed2-205">`CookieSecure`這一個旗標，表示是否建立的 cookie 會受限於 HTTPS、 HTTP 或 HTTPS 或為要求相同的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="4aed2-205">`CookieSecure` is a flag indicating if the cookie created should be limited to HTTPS, HTTP or HTTPS, or the same protocol as the request.</span></span> <span data-ttu-id="4aed2-206">預設值為`SameAsRequest`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-206">This defaults to `SameAsRequest`.</span></span>

* <span data-ttu-id="4aed2-207">`ExpireTimeSpan`是`TimeSpan`之後 cookie 已過期。</span><span class="sxs-lookup"><span data-stu-id="4aed2-207">`ExpireTimeSpan` is the `TimeSpan` after which the cookie expires.</span></span> <span data-ttu-id="4aed2-208">它會加入至目前的日期和時間，若要建立 cookie 的到期日。</span><span class="sxs-lookup"><span data-stu-id="4aed2-208">It's added to the current date and time to create the expiry date for the cookie.</span></span>

* <span data-ttu-id="4aed2-209">`SlidingExpiration`指出是否 cookie 的到期日重設時超過一半的旗標`ExpireTimeSpan`間隔。</span><span class="sxs-lookup"><span data-stu-id="4aed2-209">`SlidingExpiration` is a flag indicating whether the cookie expiration date resets when more than half of the `ExpireTimeSpan` interval has passed.</span></span> <span data-ttu-id="4aed2-210">新的到期日向前移動到目前日期加上`ExpireTimespan`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-210">The new expiry date is moved forward to be the current date plus the `ExpireTimespan`.</span></span> <span data-ttu-id="4aed2-211">[絕對到期時間](xref:security/authentication/cookie#security-authentication-absolute-expiry)可以透過設定`AuthenticationProperties`類別呼叫時`SignInAsync`。</span><span class="sxs-lookup"><span data-stu-id="4aed2-211">An [absolute expiry time](xref:security/authentication/cookie#security-authentication-absolute-expiry) can be set by using the `AuthenticationProperties` class when calling `SignInAsync`.</span></span> <span data-ttu-id="4aed2-212">絕對到期可以改善應用程式的安全性限制的驗證 cookie 為有效的時間量。</span><span class="sxs-lookup"><span data-stu-id="4aed2-212">An absolute expiry can improve the security of your application by limiting the amount of time for which the authentication cookie is valid.</span></span>

<span data-ttu-id="4aed2-213">示範如何使用`CookieAuthenticationOptions`中`Configure`方法*Startup.cs*遵循：</span><span class="sxs-lookup"><span data-stu-id="4aed2-213">An example of using `CookieAuthenticationOptions` in the `Configure` method of *Startup.cs* follows:</span></span>

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

## <a name="persistent-cookies-and-absolute-expiry-times"></a><span data-ttu-id="4aed2-214">永續性 cookie 和絕對到期時間</span><span class="sxs-lookup"><span data-stu-id="4aed2-214">Persistent cookies and absolute expiry times</span></span>

<span data-ttu-id="4aed2-215">您可能想要在瀏覽器工作階段之間保存，並識別和 cookie 傳輸它絕對到期 cookie 到期。</span><span class="sxs-lookup"><span data-stu-id="4aed2-215">You may want the cookie expiry to persist across browser sessions and want an absolute expiry to the identity and the cookie transporting it.</span></span> <span data-ttu-id="4aed2-216">此持續性，才應該啟用明確的使用者同意的情況下，透過 「 記住我 核取方塊上登入或類似的機制。</span><span class="sxs-lookup"><span data-stu-id="4aed2-216">This persistence should only be enabled with explicit user consent, via a "Remember Me" checkbox on login or a similar mechanism.</span></span> <span data-ttu-id="4aed2-217">您可以執行下列步驟使用`AuthenticationProperties`參數`SignInAsync`時呼叫方法[登入身分識別，以及建立 cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie)。</span><span class="sxs-lookup"><span data-stu-id="4aed2-217">You can do these things by using the `AuthenticationProperties` parameter on the `SignInAsync` method called when [signing in an identity and creating the cookie](xref:security/authentication/cookie#security-authentication-cookie-middleware-creating-a-cookie).</span></span> <span data-ttu-id="4aed2-218">例如: </span><span class="sxs-lookup"><span data-stu-id="4aed2-218">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-219">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-219">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="4aed2-220">`AuthenticationProperties`在上述程式碼片段中，所使用的類別位於`Microsoft.AspNetCore.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="4aed2-220">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
await HttpContext.Authentication.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        IsPersistent = true
    });
```

<span data-ttu-id="4aed2-222">`AuthenticationProperties`在上述程式碼片段中，所使用的類別位於`Microsoft.AspNetCore.Http.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="4aed2-222">The `AuthenticationProperties` class, used in the preceding code snippet, resides in the `Microsoft.AspNetCore.Http.Authentication` namespace.</span></span>

---

<span data-ttu-id="4aed2-223">上述程式碼片段會建立身分識別和對應 cookie 未透過瀏覽器封閉區段。</span><span class="sxs-lookup"><span data-stu-id="4aed2-223">The preceding code snippet creates an identity and corresponding cookie which survives through browser closures.</span></span> <span data-ttu-id="4aed2-224">任何透過先前設定的滑動逾期設定[cookie 選項](xref:security/authentication/cookie#security-authentication-cookie-options)被仍接受。</span><span class="sxs-lookup"><span data-stu-id="4aed2-224">Any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options) are still honored.</span></span> <span data-ttu-id="4aed2-225">如果 cookie 已過期而關閉瀏覽器，瀏覽器之後重新啟動時清除它。</span><span class="sxs-lookup"><span data-stu-id="4aed2-225">If the cookie expires whilst the browser is closed, the browser clears it once it is restarted.</span></span>

<a name="security-authentication-absolute-expiry"></a>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4aed2-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
await HttpContext.SignInAsync(
    "MyCookieAuthenticationScheme",
    principal,
    new AuthenticationProperties
    {
        ExpiresUtc = DateTime.UtcNow.AddMinutes(20)
    });
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4aed2-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4aed2-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="4aed2-228">上述程式碼片段會建立身分識別和對應的 cookie 可延續 20 分鐘。</span><span class="sxs-lookup"><span data-stu-id="4aed2-228">The preceding code snippet creates an identity and corresponding cookie which lasts for 20 minutes.</span></span> <span data-ttu-id="4aed2-229">這會忽略之前透過設定任何滑動逾期設定[cookie 選項](xref:security/authentication/cookie#security-authentication-cookie-options)。</span><span class="sxs-lookup"><span data-stu-id="4aed2-229">This ignores any sliding expiration settings previously configured via [cookie options](xref:security/authentication/cookie#security-authentication-cookie-options).</span></span>

<span data-ttu-id="4aed2-230">`ExpiresUtc`和`IsPersistent`屬性互斥。</span><span class="sxs-lookup"><span data-stu-id="4aed2-230">The `ExpiresUtc` and `IsPersistent` properties are mutually exclusive.</span></span>
