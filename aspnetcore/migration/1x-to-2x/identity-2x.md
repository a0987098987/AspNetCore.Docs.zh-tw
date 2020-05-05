---
title: Identity將驗證遷移到 ASP.NET Core 2。0
author: scottaddie
description: 本文概述遷移 ASP.NET Core 1.x 驗證和Identity ASP.NET Core 2.0 的最常見步驟。
ms.author: scaddie
ms.date: 06/21/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: e828446716d88d92aeb587874421a5751dcb6de0
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82769497"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="ed38c-103">Identity將驗證遷移到 ASP.NET Core 2。0</span><span class="sxs-lookup"><span data-stu-id="ed38c-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="ed38c-104">由[Scott Addie](https://github.com/scottaddie)和[Hao Kung](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="ed38c-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="ed38c-105">ASP.NET Core 2.0 具有用於驗證的新模型， [Identity](xref:security/authentication/identity)並使用服務來簡化設定。</span><span class="sxs-lookup"><span data-stu-id="ed38c-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="ed38c-106">使用驗證的 ASP.NET Core 1.x 應用程式，或Identity可以更新為使用新的模型，如下所述。</span><span class="sxs-lookup"><span data-stu-id="ed38c-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="ed38c-107">更新命名空間</span><span class="sxs-lookup"><span data-stu-id="ed38c-107">Update namespaces</span></span>

<span data-ttu-id="ed38c-108">在1.x 中，在命名空間`IdentityRole`中`IdentityUser`找到如和的`Microsoft.AspNetCore.Identity.EntityFrameworkCore`類別。</span><span class="sxs-lookup"><span data-stu-id="ed38c-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="ed38c-109">在2.0 中， <xref:Microsoft.AspNetCore.Identity>命名空間會成為這類類別的新首頁。</span><span class="sxs-lookup"><span data-stu-id="ed38c-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="ed38c-110">使用預設Identity程式碼時，受影響`ApplicationUser`的`Startup`類別包括和。</span><span class="sxs-lookup"><span data-stu-id="ed38c-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="ed38c-111">調整您`using`的語句，以解決受影響的參考。</span><span class="sxs-lookup"><span data-stu-id="ed38c-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="ed38c-112">驗證中介軟體和服務</span><span class="sxs-lookup"><span data-stu-id="ed38c-112">Authentication Middleware and services</span></span>

<span data-ttu-id="ed38c-113">在1.x 專案中，驗證是透過中介軟體來設定。</span><span class="sxs-lookup"><span data-stu-id="ed38c-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="ed38c-114">系統會針對您想要支援的每個驗證配置叫用中介軟體方法。</span><span class="sxs-lookup"><span data-stu-id="ed38c-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="ed38c-115">下列1.x 範例會使用Identity *Startup.cs*中的來設定 Facebook 驗證：</span><span class="sxs-lookup"><span data-stu-id="ed38c-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.UseIdentity();
    app.UseFacebookAuthentication(new FacebookOptions {
        AppId = Configuration["auth:facebook:appid"],
        AppSecret = Configuration["auth:facebook:appsecret"]
    });
}
```

<span data-ttu-id="ed38c-116">在2.0 專案中，會透過服務來設定驗證。</span><span class="sxs-lookup"><span data-stu-id="ed38c-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="ed38c-117">每個驗證配置都會在`ConfigureServices` *Startup.cs*的方法中註冊。</span><span class="sxs-lookup"><span data-stu-id="ed38c-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="ed38c-118">`UseIdentity`方法已由取代`UseAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="ed38c-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="ed38c-119">下列2.0 範例會使用Identity *Startup.cs*中的來設定 Facebook 驗證：</span><span class="sxs-lookup"><span data-stu-id="ed38c-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="ed38c-120">`UseAuthentication`方法會新增單一驗證中介軟體元件，負責自動驗證和處理遠端驗證要求。</span><span class="sxs-lookup"><span data-stu-id="ed38c-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="ed38c-121">它會以單一通用中介軟體元件取代所有個別中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="ed38c-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="ed38c-122">以下是每個主要驗證配置的2.0 遷移指示。</span><span class="sxs-lookup"><span data-stu-id="ed38c-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="ed38c-123">以 Cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-123">Cookie-based authentication</span></span>

<span data-ttu-id="ed38c-124">選取下列兩個選項的其中一個，並在*Startup.cs*中進行必要的變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="ed38c-125">使用 cookie 搭配Identity</span><span class="sxs-lookup"><span data-stu-id="ed38c-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="ed38c-126">在`UseIdentity`方法`UseAuthentication`中， `Configure`將取代為：</span><span class="sxs-lookup"><span data-stu-id="ed38c-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="ed38c-127">叫用`AddIdentity` `ConfigureServices`方法中的方法，以新增 cookie 驗證服務。</span><span class="sxs-lookup"><span data-stu-id="ed38c-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="ed38c-128">（選擇性）在`ConfigureApplicationCookie` `ConfigureExternalCookie` `ConfigureServices`方法中叫用或方法，以Identity調整 cookie 設定。</span><span class="sxs-lookup"><span data-stu-id="ed38c-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="ed38c-129">不使用 cookieIdentity</span><span class="sxs-lookup"><span data-stu-id="ed38c-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="ed38c-130">將`UseCookieAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="ed38c-131">叫用`AddAuthentication` `ConfigureServices`方法`AddCookie`中的和方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User,
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options =>
                {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="ed38c-132">JWT 持有人驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="ed38c-133">在*Startup.cs*中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ed38c-134">將`UseJwtBearerAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ed38c-135">在`ConfigureServices`方法`AddJwtBearer`中叫用方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="ed38c-136">此程式碼片段不會Identity使用，因此應該藉由傳遞`JwtBearerDefaults.AuthenticationScheme`至`AddAuthentication`方法來設定預設配置。</span><span class="sxs-lookup"><span data-stu-id="ed38c-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="ed38c-137">OpenID Connect （OIDC）驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="ed38c-138">在*Startup.cs*中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="ed38c-139">將`UseOpenIdConnectAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ed38c-140">在`ConfigureServices`方法`AddOpenIdConnect`中叫用方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options =>
    {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options =>
    {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

- <span data-ttu-id="ed38c-141">將`PostLogoutRedirectUri` `OpenIdConnectOptions`動作中的屬性取代為`SignedOutRedirectUri`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="ed38c-142">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-142">Facebook authentication</span></span>

<span data-ttu-id="ed38c-143">在*Startup.cs*中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ed38c-144">將`UseFacebookAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ed38c-145">在`ConfigureServices`方法`AddFacebook`中叫用方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="ed38c-146">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-146">Google authentication</span></span>

<span data-ttu-id="ed38c-147">在*Startup.cs*中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ed38c-148">將`UseGoogleAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ed38c-149">在`ConfigureServices`方法`AddGoogle`中叫用方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="ed38c-150">Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-150">Microsoft Account authentication</span></span>

<span data-ttu-id="ed38c-151">如需 Microsoft 帳戶驗證的詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/14455)。</span><span class="sxs-lookup"><span data-stu-id="ed38c-151">For more information on Microsoft account authentication, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/14455).</span></span>

<span data-ttu-id="ed38c-152">在*Startup.cs*中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-152">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ed38c-153">將`UseMicrosoftAccountAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-153">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ed38c-154">在`ConfigureServices`方法`AddMicrosoftAccount`中叫用方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-154">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="ed38c-155">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="ed38c-155">Twitter authentication</span></span>

<span data-ttu-id="ed38c-156">在*Startup.cs*中進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ed38c-156">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="ed38c-157">將`UseTwitterAuthentication` `Configure`方法中的方法呼叫取代為`UseAuthentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-157">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="ed38c-158">在`ConfigureServices`方法`AddTwitter`中叫用方法：</span><span class="sxs-lookup"><span data-stu-id="ed38c-158">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="ed38c-159">設定預設驗證配置</span><span class="sxs-lookup"><span data-stu-id="ed38c-159">Setting default authentication schemes</span></span>

<span data-ttu-id="ed38c-160">在1.x 中， [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基類`AutomaticAuthenticate`的`AutomaticChallenge`和屬性是要在單一驗證配置上設定。</span><span class="sxs-lookup"><span data-stu-id="ed38c-160">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="ed38c-161">沒有任何好方法可以強制執行此作業。</span><span class="sxs-lookup"><span data-stu-id="ed38c-161">There was no good way to enforce this.</span></span>

<span data-ttu-id="ed38c-162">在2.0 中，這兩個屬性已移除為個別`AuthenticationOptions`實例上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ed38c-162">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="ed38c-163">您可以在*Startup.cs*的`ConfigureServices`方法`AddAuthentication`中，于方法呼叫中設定它們：</span><span class="sxs-lookup"><span data-stu-id="ed38c-163">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="ed38c-164">在上述程式碼片段中，預設配置會設定為`CookieAuthenticationDefaults.AuthenticationScheme` （"cookie"）。</span><span class="sxs-lookup"><span data-stu-id="ed38c-164">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="ed38c-165">或者，使用`AddAuthentication`方法的多載版本來設定一個以上的屬性。</span><span class="sxs-lookup"><span data-stu-id="ed38c-165">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="ed38c-166">在下列多載方法範例中，預設配置設定為`CookieAuthenticationDefaults.AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="ed38c-166">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="ed38c-167">您也可以在個別`[Authorize]`屬性或授權原則內指定驗證配置。</span><span class="sxs-lookup"><span data-stu-id="ed38c-167">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="ed38c-168">如果下列其中一個條件成立，請在2.0 中定義預設配置：</span><span class="sxs-lookup"><span data-stu-id="ed38c-168">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="ed38c-169">您想要讓使用者自動登入</span><span class="sxs-lookup"><span data-stu-id="ed38c-169">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="ed38c-170">您可以使用`[Authorize]`屬性或授權原則，而不需要指定配置</span><span class="sxs-lookup"><span data-stu-id="ed38c-170">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="ed38c-171">此規則的`AddIdentity`例外狀況是方法。</span><span class="sxs-lookup"><span data-stu-id="ed38c-171">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="ed38c-172">這個方法會為您新增 cookie，並將預設的驗證和挑戰配置設定為`IdentityConstants.ApplicationScheme`應用程式 cookie。</span><span class="sxs-lookup"><span data-stu-id="ed38c-172">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="ed38c-173">此外，它會將預設登入配置設定為外部 cookie `IdentityConstants.ExternalScheme`。</span><span class="sxs-lookup"><span data-stu-id="ed38c-173">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="ed38c-174">使用 HttpCoNtext 驗證延伸模組</span><span class="sxs-lookup"><span data-stu-id="ed38c-174">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="ed38c-175">`IAuthenticationManager`介面是 1. x 驗證系統的主要進入點。</span><span class="sxs-lookup"><span data-stu-id="ed38c-175">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="ed38c-176">它已由`Microsoft.AspNetCore.Authentication`命名空間中的一組`HttpContext`新擴充方法取代。</span><span class="sxs-lookup"><span data-stu-id="ed38c-176">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="ed38c-177">例如，1. x 個專案會參考`Authentication`屬性：</span><span class="sxs-lookup"><span data-stu-id="ed38c-177">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="ed38c-178">在2.0 專案中，匯`Microsoft.AspNetCore.Authentication`入命名空間，並`Authentication`刪除屬性參考：</span><span class="sxs-lookup"><span data-stu-id="ed38c-178">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="ed38c-179">Windows 驗證（HTTP.sys/IISIntegration）</span><span class="sxs-lookup"><span data-stu-id="ed38c-179">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="ed38c-180">Windows 驗證有兩種變化：</span><span class="sxs-lookup"><span data-stu-id="ed38c-180">There are two variations of Windows authentication:</span></span>

* <span data-ttu-id="ed38c-181">主機只允許已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="ed38c-181">The host only allows authenticated users.</span></span> <span data-ttu-id="ed38c-182">這項差異不會受到2.0 變更的影響。</span><span class="sxs-lookup"><span data-stu-id="ed38c-182">This variation isn't affected by the 2.0 changes.</span></span>
* <span data-ttu-id="ed38c-183">主機允許匿名和已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="ed38c-183">The host allows both anonymous and authenticated users.</span></span> <span data-ttu-id="ed38c-184">這項變化會受到2.0 變更的影響。</span><span class="sxs-lookup"><span data-stu-id="ed38c-184">This variation is affected by the 2.0 changes.</span></span> <span data-ttu-id="ed38c-185">例如，應用程式應該允許[IIS](xref:host-and-deploy/iis/index)或[HTTP.sys](xref:fundamentals/servers/httpsys)層的匿名使用者，但在控制站層級授權使用者。</span><span class="sxs-lookup"><span data-stu-id="ed38c-185">For example, the app should allow anonymous users at the [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorize users at the controller level.</span></span> <span data-ttu-id="ed38c-186">在此案例中，請在`Startup.ConfigureServices`方法中設定預設配置。</span><span class="sxs-lookup"><span data-stu-id="ed38c-186">In this scenario, set the default scheme in the `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="ed38c-187">針對[IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)，將預設配置設為`IISDefaults.AuthenticationScheme`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-187">For [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), set the default scheme to `IISDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="ed38c-188">針對[HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)，將預設配置設為`HttpSysDefaults.AuthenticationScheme`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-188">For [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), set the default scheme to `HttpSysDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="ed38c-189">無法設定預設配置，會導致授權（挑戰）要求無法使用下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="ed38c-189">Failure to set the default scheme prevents the authorize (challenge) request from working with the following exception:</span></span>

  > <span data-ttu-id="ed38c-190">`System.InvalidOperationException`：未指定 authenticationScheme，而且找不到任何 DefaultChallengeScheme。</span><span class="sxs-lookup"><span data-stu-id="ed38c-190">`System.InvalidOperationException`: No authenticationScheme was specified, and there was no DefaultChallengeScheme found.</span></span>

<span data-ttu-id="ed38c-191">如需詳細資訊，請參閱<xref:security/authentication/windowsauth>。</span><span class="sxs-lookup"><span data-stu-id="ed38c-191">For more information, see <xref:security/authentication/windowsauth>.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="ed38c-192">IdentityCookieOptions 實例</span><span class="sxs-lookup"><span data-stu-id="ed38c-192">IdentityCookieOptions instances</span></span>

<span data-ttu-id="ed38c-193">2.0 變更的副作用是，切換為使用已命名的選項，而不是 cookie 選項實例。</span><span class="sxs-lookup"><span data-stu-id="ed38c-193">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="ed38c-194">已移除自訂Identity cookie 配置名稱的功能。</span><span class="sxs-lookup"><span data-stu-id="ed38c-194">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="ed38c-195">例如，1.x 專案會使用函`IdentityCookieOptions`式[插入](xref:mvc/controllers/dependency-injection#constructor-injection)將參數傳遞至*AccountController.cs*和*ManageController.cs*。</span><span class="sxs-lookup"><span data-stu-id="ed38c-195">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="ed38c-196">外部 cookie 驗證配置會從提供的實例進行存取：</span><span class="sxs-lookup"><span data-stu-id="ed38c-196">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="ed38c-197">上述的函式插入在2.0 專案中會變得`_externalCookieScheme`不必要，而且可以刪除欄位：</span><span class="sxs-lookup"><span data-stu-id="ed38c-197">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="ed38c-198">1.x 專案使用`_externalCookieScheme`欄位，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ed38c-198">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="ed38c-199">在2.0 專案中，將上述程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="ed38c-199">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="ed38c-200">`IdentityConstants.ExternalScheme`常數可以直接使用。</span><span class="sxs-lookup"><span data-stu-id="ed38c-200">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="ed38c-201">藉由匯入`SignOutAsync`下列命名空間來解析新增的呼叫：</span><span class="sxs-lookup"><span data-stu-id="ed38c-201">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="ed38c-202">新增 IdentityUser POCO 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="ed38c-202">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="ed38c-203">基底`IdentityUser` POCO （簡單的 CLR 物件）的 ENTITY FRAMEWORK （EF） Core 導覽屬性已經移除。</span><span class="sxs-lookup"><span data-stu-id="ed38c-203">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="ed38c-204">如果您的1.x 專案使用這些屬性，請手動將其新增回2.0 專案：</span><span class="sxs-lookup"><span data-stu-id="ed38c-204">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

```csharp
/// <summary>
/// Navigation property for the roles this user belongs to.
/// </summary>
public virtual ICollection<IdentityUserRole<int>> Roles { get; } = new List<IdentityUserRole<int>>();

/// <summary>
/// Navigation property for the claims this user possesses.
/// </summary>
public virtual ICollection<IdentityUserClaim<int>> Claims { get; } = new List<IdentityUserClaim<int>>();

/// <summary>
/// Navigation property for this users login accounts.
/// </summary>
public virtual ICollection<IdentityUserLogin<int>> Logins { get; } = new List<IdentityUserLogin<int>>();
```

<span data-ttu-id="ed38c-205">若要在執行 EF Core 遷移時避免重複的外鍵，請將`IdentityDbContext`下列內容`OnModelCreating`新增至類別的`base.OnModelCreating();`方法（在呼叫之後）：</span><span class="sxs-lookup"><span data-stu-id="ed38c-205">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Core Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Core Identity table names and more.
    // Add your customizations after calling base.OnModelCreating(builder);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Claims)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Logins)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);

    builder.Entity<ApplicationUser>()
        .HasMany(e => e.Roles)
        .WithOne()
        .HasForeignKey(e => e.UserId)
        .IsRequired()
        .OnDelete(DeleteBehavior.Cascade);
}
```

<a name="synchronous-method-removal"></a>

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="ed38c-206">取代 GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="ed38c-206">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="ed38c-207">已移除同步`GetExternalAuthenticationSchemes`方法，以改用非同步版本。</span><span class="sxs-lookup"><span data-stu-id="ed38c-207">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="ed38c-208">1.x 專案在 controller */ManageController*中具有下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="ed38c-208">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="ed38c-209">這個方法也會出現在*Views/Account/Login 中。 cshtml* ：</span><span class="sxs-lookup"><span data-stu-id="ed38c-209">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="ed38c-210">在2.0 專案中，請<xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>使用方法。</span><span class="sxs-lookup"><span data-stu-id="ed38c-210">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="ed38c-211">*ManageController.cs*中的變更與下列程式碼類似：</span><span class="sxs-lookup"><span data-stu-id="ed38c-211">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="ed38c-212">在*登入. cshtml*中`AuthenticationScheme` ， `foreach`迴圈中存取的屬性會`Name`變更為：</span><span class="sxs-lookup"><span data-stu-id="ed38c-212">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="ed38c-213">ManageLoginsViewModel 屬性變更</span><span class="sxs-lookup"><span data-stu-id="ed38c-213">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="ed38c-214">`ManageLoginsViewModel`物件用於`ManageLogins` *ManageController.cs*的動作中。</span><span class="sxs-lookup"><span data-stu-id="ed38c-214">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="ed38c-215">在1.x 專案中，物件的`OtherLogins`屬性傳回型別為。 `IList<AuthenticationDescription>`</span><span class="sxs-lookup"><span data-stu-id="ed38c-215">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="ed38c-216">此傳回類型需要匯入`Microsoft.AspNetCore.Http.Authentication`：</span><span class="sxs-lookup"><span data-stu-id="ed38c-216">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="ed38c-217">在2.0 專案中，傳回型別會`IList<AuthenticationScheme>`變更為。</span><span class="sxs-lookup"><span data-stu-id="ed38c-217">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="ed38c-218">這個新的傳回型別需要`Microsoft.AspNetCore.Http.Authentication`以匯入`Microsoft.AspNetCore.Authentication`取代匯入。</span><span class="sxs-lookup"><span data-stu-id="ed38c-218">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="ed38c-219">其他資源</span><span class="sxs-lookup"><span data-stu-id="ed38c-219">Additional resources</span></span>

<span data-ttu-id="ed38c-220">如需詳細資訊，請參閱 GitHub 上的[Auth 2.0 問題討論](https://github.com/aspnet/Security/issues/1338)。</span><span class="sxs-lookup"><span data-stu-id="ed38c-220">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
