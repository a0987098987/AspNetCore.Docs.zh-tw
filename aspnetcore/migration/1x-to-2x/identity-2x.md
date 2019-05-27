---
title: 將驗證和身分識別移轉至 ASP.NET Core 2.0
author: scottaddie
description: 本文概述了最常見的步驟移轉 ASP.NET Core 1.x 驗證和身分識別為 ASP.NET Core 2.0。
ms.author: scaddie
ms.date: 12/18/2018
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 086deac51af186012315d5b6a1236c92c8980037
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039246"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="de942-103">將驗證和身分識別移轉至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="de942-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="de942-104">藉由[Scott Addie](https://github.com/scottaddie)和[Hao 是一隻](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="de942-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="de942-105">ASP.NET Core 2.0 具有新的模型來驗證和[識別](xref:security/authentication/identity)可簡化使用服務的組態。</span><span class="sxs-lookup"><span data-stu-id="de942-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="de942-106">使用驗證或識別的 ASP.NET Core 1.x 應用程式可以更新為使用新的模型，如下所述。</span><span class="sxs-lookup"><span data-stu-id="de942-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="de942-107">驗證中介軟體和服務</span><span class="sxs-lookup"><span data-stu-id="de942-107">Authentication Middleware and services</span></span>

<span data-ttu-id="de942-108">在 1.x 專案中，驗證已透過中介軟體。</span><span class="sxs-lookup"><span data-stu-id="de942-108">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="de942-109">中介軟體的方法會叫用每個您想要支援的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="de942-109">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="de942-110">下列範例中，1.x 設定 Facebook 驗證使用中身分識別*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-110">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="de942-111">在 2.0 專案中，驗證是透過服務設定。</span><span class="sxs-lookup"><span data-stu-id="de942-111">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="de942-112">在中註冊每個驗證配置`ConfigureServices`方法*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="de942-112">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="de942-113">`UseIdentity`方法會取代`UseAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="de942-113">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="de942-114">下例 2.0 設定 Facebook 驗證使用中身分識別*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-114">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="de942-115">`UseAuthentication`方法會將單一驗證中介軟體元件，負責自動驗證和遠端驗證要求的處理。</span><span class="sxs-lookup"><span data-stu-id="de942-115">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="de942-116">它會取代所有個別的中介軟體元件的單一、 共同的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="de942-116">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="de942-117">以下是每個主要的驗證配置的 2.0 移轉指示。</span><span class="sxs-lookup"><span data-stu-id="de942-117">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="de942-118">以 cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="de942-118">Cookie-based authentication</span></span>

<span data-ttu-id="de942-119">選取其中一個，在兩個選項，並進行必要的變更，在*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-119">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="de942-120">使用身分識別的 cookie</span><span class="sxs-lookup"><span data-stu-id="de942-120">Use cookies with Identity</span></span>
    - <span data-ttu-id="de942-121">取代`UseIdentity`具有`UseAuthentication`在`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-121">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="de942-122">叫用`AddIdentity`方法中的`ConfigureServices`將 cookie 驗證服務的方法。</span><span class="sxs-lookup"><span data-stu-id="de942-122">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="de942-123">（選擇性） 叫用`ConfigureApplicationCookie`或是`ConfigureExternalCookie`方法中的`ConfigureServices`調整的身分識別的 cookie 設定的方法。</span><span class="sxs-lookup"><span data-stu-id="de942-123">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="de942-124">使用沒有身分識別的 cookie</span><span class="sxs-lookup"><span data-stu-id="de942-124">Use cookies without Identity</span></span>
    - <span data-ttu-id="de942-125">取代`UseCookieAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-125">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="de942-126">叫用`AddAuthentication`並`AddCookie`中的方法`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-126">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="de942-127">JWT 持有人驗證</span><span class="sxs-lookup"><span data-stu-id="de942-127">JWT Bearer Authentication</span></span>

<span data-ttu-id="de942-128">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-128">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="de942-129">取代`UseJwtBearerAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-129">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="de942-130">叫用`AddJwtBearer`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-130">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="de942-131">此程式碼片段不會使用身分識別，因此應該藉由傳遞設定的預設配置`JwtBearerDefaults.AuthenticationScheme`至`AddAuthentication`方法。</span><span class="sxs-lookup"><span data-stu-id="de942-131">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="de942-132">OpenID Connect (OIDC) 驗證</span><span class="sxs-lookup"><span data-stu-id="de942-132">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="de942-133">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-133">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="de942-134">取代`UseOpenIdConnectAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-134">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="de942-135">叫用`AddOpenIdConnect`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-135">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="de942-136">取代`PostLogoutRedirectUri`中的屬性`OpenIdConnectOptions`動作`SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="de942-136">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="de942-137">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="de942-137">Facebook authentication</span></span>

<span data-ttu-id="de942-138">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="de942-139">取代`UseFacebookAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="de942-140">叫用`AddFacebook`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="de942-141">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="de942-141">Google authentication</span></span>

<span data-ttu-id="de942-142">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="de942-143">取代`UseGoogleAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="de942-144">叫用`AddGoogle`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="de942-145">Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="de942-145">Microsoft Account authentication</span></span>

<span data-ttu-id="de942-146">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="de942-147">取代`UseMicrosoftAccountAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="de942-148">叫用`AddMicrosoftAccount`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="de942-149">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="de942-149">Twitter authentication</span></span>

<span data-ttu-id="de942-150">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="de942-151">取代`UseTwitterAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="de942-152">叫用`AddTwitter`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="de942-153">設定預設的驗證配置</span><span class="sxs-lookup"><span data-stu-id="de942-153">Setting default authentication schemes</span></span>

<span data-ttu-id="de942-154">在 1.x`AutomaticAuthenticate`和`AutomaticChallenge`的屬性[AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底類別要設定一種驗證配置。</span><span class="sxs-lookup"><span data-stu-id="de942-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="de942-155">發生不好的方法，強制執行。</span><span class="sxs-lookup"><span data-stu-id="de942-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="de942-156">在 2.0 中，這兩個屬性有為個別的屬性移除`AuthenticationOptions`執行個體。</span><span class="sxs-lookup"><span data-stu-id="de942-156">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="de942-157">在設定這些`AddAuthentication`方法呼叫內`ConfigureServices`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-157">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="de942-158">在上述程式碼片段中，預設配置設為`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie")。</span><span class="sxs-lookup"><span data-stu-id="de942-158">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="de942-159">或者，使用的多載的版本`AddAuthentication`方法來設定多個屬性。</span><span class="sxs-lookup"><span data-stu-id="de942-159">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="de942-160">在下列的多載的方法範例中，預設配置設為`CookieAuthenticationDefaults.AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="de942-160">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="de942-161">驗證配置或者在您的個人內指定`[Authorize]`屬性或授權原則。</span><span class="sxs-lookup"><span data-stu-id="de942-161">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="de942-162">如果下列條件之一成立，請在 2.0 中定義的預設配置：</span><span class="sxs-lookup"><span data-stu-id="de942-162">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="de942-163">您希望使用者會自動登入</span><span class="sxs-lookup"><span data-stu-id="de942-163">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="de942-164">您使用`[Authorize]`屬性或授權原則，而不指定結構描述</span><span class="sxs-lookup"><span data-stu-id="de942-164">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="de942-165">此規則的例外是`AddIdentity`方法。</span><span class="sxs-lookup"><span data-stu-id="de942-165">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="de942-166">這個方法會將 cookie 加入寄件者和預設值進行驗證，而且挑戰應用程式 cookie 配置的集`IdentityConstants.ApplicationScheme`。</span><span class="sxs-lookup"><span data-stu-id="de942-166">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="de942-167">此外，它將預設登入的配置設定為外部 cookie `IdentityConstants.ExternalScheme`。</span><span class="sxs-lookup"><span data-stu-id="de942-167">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="de942-168">使用 HttpContext 驗證延伸模組</span><span class="sxs-lookup"><span data-stu-id="de942-168">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="de942-169">`IAuthenticationManager`介面是 1.x 驗證系統的主要進入點。</span><span class="sxs-lookup"><span data-stu-id="de942-169">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="de942-170">它已取代為一組新的`HttpContext`中的擴充方法`Microsoft.AspNetCore.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="de942-170">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="de942-171">比方說，1.x 專案參考`Authentication`屬性：</span><span class="sxs-lookup"><span data-stu-id="de942-171">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="de942-172">在 2.0 專案中，匯入`Microsoft.AspNetCore.Authentication`命名空間，並刪除`Authentication`屬性參考：</span><span class="sxs-lookup"><span data-stu-id="de942-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="de942-173">Windows 驗證 (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="de942-173">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="de942-174">有兩種變化的 Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="de942-174">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="de942-175">主應用程式只允許已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="de942-175">The host only allows authenticated users</span></span>
2. <span data-ttu-id="de942-176">主機允許匿名和已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="de942-176">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="de942-177">上述第一種變化不會受到 2.0 的變更。</span><span class="sxs-lookup"><span data-stu-id="de942-177">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="de942-178">上述第二種變化會受到 2.0 的變更。</span><span class="sxs-lookup"><span data-stu-id="de942-178">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="de942-179">例如，您可能會允許匿名使用者到您的應用程式在 IIS 或[HTTP.sys](xref:fundamentals/servers/httpsys)層但授權的使用者，在控制器層級。</span><span class="sxs-lookup"><span data-stu-id="de942-179">As an example, you may be allowing anonymous users into your app at the IIS or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="de942-180">在此案例中，設定為預設配置`IISDefaults.AuthenticationScheme`在`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-180">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `Startup.ConfigureServices` method:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="de942-181">若未設定的預設配置據以防止授權要求，要求無法運作。</span><span class="sxs-lookup"><span data-stu-id="de942-181">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="de942-182">IdentityCookieOptions 執行個體</span><span class="sxs-lookup"><span data-stu-id="de942-182">IdentityCookieOptions instances</span></span>

<span data-ttu-id="de942-183">2.0 變更的副作用是切換到使用名為選項，而不是 cookie 選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="de942-183">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="de942-184">不再能夠自訂身分識別的 cookie 配置名稱。</span><span class="sxs-lookup"><span data-stu-id="de942-184">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="de942-185">比方說，1.x 專案使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)傳遞`IdentityCookieOptions`參數插入*AccountController.cs*。</span><span class="sxs-lookup"><span data-stu-id="de942-185">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="de942-186">從提供的執行個體來存取外部的 cookie 驗證配置：</span><span class="sxs-lookup"><span data-stu-id="de942-186">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="de942-187">上述建構函式插入將不再需要在 2.0 專案中，而`_externalCookieScheme`欄位可以刪除：</span><span class="sxs-lookup"><span data-stu-id="de942-187">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="de942-188">`IdentityConstants.ExternalScheme`常數可以直接使用：</span><span class="sxs-lookup"><span data-stu-id="de942-188">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="de942-189">加入 POCO IdentityUser 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="de942-189">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="de942-190">Entity Framework (EF) Core 導覽屬性的基底`IdentityUser`POCO （純舊 CLR 物件） 已移除。</span><span class="sxs-lookup"><span data-stu-id="de942-190">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="de942-191">如果您的 1.x 專案會使用這些屬性，以手動方式將他們新增回 2.0 的專案：</span><span class="sxs-lookup"><span data-stu-id="de942-191">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="de942-192">若要執行 EF Core 移轉時，請避免重複的外部索引鍵，將下列內容加入您`IdentityDbContext`類別`OnModelCreating`方法 (之後`base.OnModelCreating();`呼叫):</span><span class="sxs-lookup"><span data-stu-id="de942-192">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="de942-193">取代 GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="de942-193">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="de942-194">同步方法`GetExternalAuthenticationSchemes`移除是因為希望的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="de942-194">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="de942-195">1.x 專案中有下列的程式碼*ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="de942-195">1.x projects have the following code in *ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="de942-196">這個方法會出現在*Login.cshtml*太：</span><span class="sxs-lookup"><span data-stu-id="de942-196">This method appears in *Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?range=62,75-84)]

<span data-ttu-id="de942-197">在 2.0 專案中，使用`GetExternalAuthenticationSchemesAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="de942-197">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="de942-198">在  *Login.cshtml*，則`AuthenticationScheme`中存取屬性`foreach`迴圈變更為`Name`:</span><span class="sxs-lookup"><span data-stu-id="de942-198">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?range=62,75-84)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="de942-199">ManageLoginsViewModel 屬性變更</span><span class="sxs-lookup"><span data-stu-id="de942-199">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="de942-200">A`ManageLoginsViewModel`中使用物件`ManageLogins`動作*ManageController.cs*。</span><span class="sxs-lookup"><span data-stu-id="de942-200">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="de942-201">在 1.x 專案中，該物件的`OtherLogins`屬性傳回型別是`IList<AuthenticationDescription>`。</span><span class="sxs-lookup"><span data-stu-id="de942-201">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="de942-202">這個傳回型別需要先匯入`Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="de942-202">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="de942-203">在 2.0 專案中，傳回的型別變更為  `IList<AuthenticationScheme>`。</span><span class="sxs-lookup"><span data-stu-id="de942-203">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="de942-204">這個新的傳回型別必須替換`Microsoft.AspNetCore.Http.Authentication`匯入具有`Microsoft.AspNetCore.Authentication`匯入。</span><span class="sxs-lookup"><span data-stu-id="de942-204">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="de942-205">其他資源</span><span class="sxs-lookup"><span data-stu-id="de942-205">Additional resources</span></span>

<span data-ttu-id="de942-206">如需其他詳細資料和討論，請參閱[Auth 2.0 討論](https://github.com/aspnet/Security/issues/1338)GitHub 上的提出問題。</span><span class="sxs-lookup"><span data-stu-id="de942-206">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
