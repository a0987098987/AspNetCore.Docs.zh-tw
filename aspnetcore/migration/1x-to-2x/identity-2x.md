---
title: "移轉的驗證和身分識別，ASP.NET Core 2.0"
author: scottaddie
description: "本文概述了最常見的步驟移轉 ASP.NET Core 1.x 驗證和身分識別為 ASP.NET Core 2.0。"
keywords: "ASP.NET Core，身分識別驗證"
ms.author: scaddie
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: 6505378c718d2ec676b534ddeb141231faf3fca3
ms.sourcegitcommit: 8cafdd1dd409d5070d227100ba0e094c779ac47b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2017
---
# <a name="migrating-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="4c602-104">移轉的驗證和身分識別，ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="4c602-104">Migrating Authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="4c602-105">由[Scott Addie](https://github.com/scottaddie)和[Hao 是一隻](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="4c602-105">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="4c602-106">ASP.NET Core 2.0 具有新的模型來驗證和[識別](xref:security/authentication/identity)可簡化使用服務的組態。</span><span class="sxs-lookup"><span data-stu-id="4c602-106">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) which simplifies configuration by using services.</span></span> <span data-ttu-id="4c602-107">使用驗證或識別的 ASP.NET Core 1.x 應用程式可以更新為使用新的模型，如下所述。</span><span class="sxs-lookup"><span data-stu-id="4c602-107">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="4c602-108">驗證中介軟體和服務</span><span class="sxs-lookup"><span data-stu-id="4c602-108">Authentication Middleware and Services</span></span>
<span data-ttu-id="4c602-109">1.x 專案中設定驗證透過中介軟體。</span><span class="sxs-lookup"><span data-stu-id="4c602-109">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="4c602-110">中介軟體方法會叫用每個您想要支援的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="4c602-110">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="4c602-111">下例會 1.x 設定 Facebook 驗證在中使用識別*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-111">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="4c602-112">在 2.0 專案中，驗證是透過服務設定。</span><span class="sxs-lookup"><span data-stu-id="4c602-112">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="4c602-113">在中註冊每個驗證配置`ConfigureServices`方法*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="4c602-113">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="4c602-114">`UseIdentity`方法取代`UseAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="4c602-114">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="4c602-115">下例會 2.0 設定 Facebook 驗證在中使用識別*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-115">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddIdentity<ApplicationUser, IdentityRole>()
            .AddEntityFrameworkStores<ApplicationDbContext>();

    // If you want to tweak Identity cookies, they're no longer part of IdentityOptions.
    services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
}

public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory) {
    app.UseAuthentication();
}
```

<span data-ttu-id="4c602-116">`UseAuthentication`方法會將單一驗證中介軟體元件，負責自動驗證和遠端驗證要求的處理。</span><span class="sxs-lookup"><span data-stu-id="4c602-116">The `UseAuthentication` method adds a single authentication middleware component which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="4c602-117">它會取代所有個別的中介軟體元件與單一、 共同的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="4c602-117">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="4c602-118">以下是每個主要的驗證配置 2.0 的移轉指示。</span><span class="sxs-lookup"><span data-stu-id="4c602-118">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="4c602-119">Cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-119">Cookie-based Authentication</span></span>
<span data-ttu-id="4c602-120">選取其中一個兩個選項，並進行必要的變更在*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-120">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="4c602-121">使用 cookie 識別</span><span class="sxs-lookup"><span data-stu-id="4c602-121">Use cookies with Identity</span></span>
    - <span data-ttu-id="4c602-122">取代`UseIdentity`與`UseAuthentication`中`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-122">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="4c602-123">叫用`AddIdentity`方法中的`ConfigureServices`方法，將 cookie 驗證服務。</span><span class="sxs-lookup"><span data-stu-id="4c602-123">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="4c602-124">（選擇性） 叫用`ConfigureApplicationCookie`或`ConfigureExternalCookie`方法中的`ConfigureServices`調整識別 cookie 設定的方法。</span><span class="sxs-lookup"><span data-stu-id="4c602-124">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();
    
        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="4c602-125">身分識別不使用 cookie</span><span class="sxs-lookup"><span data-stu-id="4c602-125">Use cookies without Identity</span></span>
    - <span data-ttu-id="4c602-126">取代`UseCookieAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-126">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
  
        ```csharp
        app.UseAuthentication();
        ```
 
    - <span data-ttu-id="4c602-127">叫用`AddAuthentication`和`AddCookie`方法`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-127">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

        ```csharp
        // If you don't want the cookie to be automatically authenticated and assigned to HttpContext.User, 
        // remove the CookieAuthenticationDefaults.AuthenticationScheme parameter passed to AddAuthentication.
        services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
                .AddCookie(options => {
                    options.LoginPath = "/Account/LogIn";
                    options.LogoutPath = "/Account/LogOff";
                });
        ```

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="4c602-128">JWT Bearer 驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-128">JWT Bearer Authentication</span></span>
<span data-ttu-id="4c602-129">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-129">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="4c602-130">取代`UseJwtBearerAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-130">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4c602-131">叫用`AddJwtBearer`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-131">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options => {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="4c602-132">此程式碼片段不會使用身分識別，因此應該藉由傳遞設定的預設配置`JwtBearerDefaults.AuthenticationScheme`至`AddAuthentication`方法。</span><span class="sxs-lookup"><span data-stu-id="4c602-132">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="4c602-133">OpenID Connect (OIDC) 驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-133">OpenID Connect (OIDC) Authentication</span></span>
<span data-ttu-id="4c602-134">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-134">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="4c602-135">取代`UseOpenIdConnectAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-135">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4c602-136">叫用`AddOpenIdConnect`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-136">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(options => {
        options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
        options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
    })
    .AddCookie()
    .AddOpenIdConnect(options => {
        options.Authority = Configuration["auth:oidc:authority"];
        options.ClientId = Configuration["auth:oidc:clientid"];
    });
    ```

### <a name="facebook-authentication"></a><span data-ttu-id="4c602-137">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-137">Facebook Authentication</span></span>
<span data-ttu-id="4c602-138">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-138">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="4c602-139">取代`UseFacebookAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-139">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4c602-140">叫用`AddFacebook`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-140">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>
    
    ```csharp
    services.AddAuthentication()
            .AddFacebook(options => {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="4c602-141">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-141">Google Authentication</span></span>
<span data-ttu-id="4c602-142">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-142">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="4c602-143">取代`UseGoogleAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-143">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4c602-144">叫用`AddGoogle`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-144">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options => {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });    
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="4c602-145">Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-145">Microsoft Account Authentication</span></span>
<span data-ttu-id="4c602-146">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-146">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="4c602-147">取代`UseMicrosoftAccountAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-147">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4c602-148">叫用`AddMicrosoftAccount`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-148">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options => {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ``` 

### <a name="twitter-authentication"></a><span data-ttu-id="4c602-149">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="4c602-149">Twitter Authentication</span></span>
<span data-ttu-id="4c602-150">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-150">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="4c602-151">取代`UseTwitterAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-151">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>
 
    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="4c602-152">叫用`AddTwitter`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-152">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options => {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="4c602-153">設定預設的驗證配置</span><span class="sxs-lookup"><span data-stu-id="4c602-153">Setting Default Authentication Schemes</span></span>
<span data-ttu-id="4c602-154">在 1.x`AutomaticAuthenticate`和`AutomaticChallenge`屬性要用來將一種驗證配置。</span><span class="sxs-lookup"><span data-stu-id="4c602-154">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="4c602-155">沒有強制這個好方法。</span><span class="sxs-lookup"><span data-stu-id="4c602-155">There was no good way to enforce this.</span></span>

<span data-ttu-id="4c602-156">在 2.0 中，這兩個屬性已做為個別的旗標移除`AuthenticationOptions`執行個體中，移到基底[AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions)類別。</span><span class="sxs-lookup"><span data-stu-id="4c602-156">In 2.0, these two properties have been removed as flags on the individual `AuthenticationOptions` instance and have moved into the base [AuthenticationOptions](/aspnet/core/api/microsoft.aspnetcore.builder.authenticationoptions) class.</span></span> <span data-ttu-id="4c602-157">屬性可以設定在`AddAuthentication`方法呼叫內`ConfigureServices`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-157">The properties can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="4c602-158">或者，使用的多載的版本`AddAuthentication`方法來設定多個屬性。</span><span class="sxs-lookup"><span data-stu-id="4c602-158">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="4c602-159">在下列多載的方法範例中，預設配置設定為`CookieAuthenticationDefaults.AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="4c602-159">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="4c602-160">驗證配置，或者可以指定在您的個人`[Authorize]`屬性或授權原則。</span><span class="sxs-lookup"><span data-stu-id="4c602-160">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options => {
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="4c602-161">如果下列條件的其中一種情況，請在 2.0 中定義的預設配置：</span><span class="sxs-lookup"><span data-stu-id="4c602-161">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="4c602-162">您想要自動登入的使用者</span><span class="sxs-lookup"><span data-stu-id="4c602-162">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="4c602-163">您使用`[Authorize]`屬性或授權原則，而不指定結構描述</span><span class="sxs-lookup"><span data-stu-id="4c602-163">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="4c602-164">此規則的例外是`AddIdentity`方法。</span><span class="sxs-lookup"><span data-stu-id="4c602-164">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="4c602-165">這個方法會將 cookie 加入適合您和設定預設的驗證和應用程式 cookie 挑戰配置`IdentityConstants.ApplicationScheme`。</span><span class="sxs-lookup"><span data-stu-id="4c602-165">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="4c602-166">此外，它將預設登入的配置設定為外部 cookie `IdentityConstants.ExternalScheme`。</span><span class="sxs-lookup"><span data-stu-id="4c602-166">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="4c602-167">使用 HttpContext 驗證延伸模組</span><span class="sxs-lookup"><span data-stu-id="4c602-167">Use HttpContext Authentication Extensions</span></span>
<span data-ttu-id="4c602-168">`IAuthenticationManager`介面是 1.x 驗證系統的主要進入點。</span><span class="sxs-lookup"><span data-stu-id="4c602-168">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="4c602-169">它已取代為一組新的`HttpContext`中的擴充方法`Microsoft.AspNetCore.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="4c602-169">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="4c602-170">例如，1.x 專案參考`Authentication`屬性：</span><span class="sxs-lookup"><span data-stu-id="4c602-170">For example, 1.x projects reference an `Authentication` property:</span></span>

<span data-ttu-id="4c602-171">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span><span class="sxs-lookup"><span data-stu-id="4c602-171">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span></span>

<span data-ttu-id="4c602-172">在 2.0 專案中，匯入`Microsoft.AspNetCore.Authentication`命名空間，並刪除`Authentication`屬性參考：</span><span class="sxs-lookup"><span data-stu-id="4c602-172">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

<span data-ttu-id="4c602-173">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span><span class="sxs-lookup"><span data-stu-id="4c602-173">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span></span>

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="4c602-174">Windows 驗證 (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="4c602-174">Windows Authentication (HTTP.sys / IISIntegration)</span></span>
<span data-ttu-id="4c602-175">有兩種變化，Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="4c602-175">There are two variations of Windows authentication:</span></span>
1. <span data-ttu-id="4c602-176">主應用程式只允許已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="4c602-176">The host only allows authenticated users</span></span>
2. <span data-ttu-id="4c602-177">主應用程式允許匿名和已驗證的使用者</span><span class="sxs-lookup"><span data-stu-id="4c602-177">The host allows both anonymous and authenticated users</span></span>

<span data-ttu-id="4c602-178">上述第一種變異不會受到 2.0 的變更。</span><span class="sxs-lookup"><span data-stu-id="4c602-178">The first variation described above is unaffected by the 2.0 changes.</span></span>

<span data-ttu-id="4c602-179">2.0 的變更會影響上述第二種變化。</span><span class="sxs-lookup"><span data-stu-id="4c602-179">The second variation described above is affected by the 2.0 changes.</span></span> <span data-ttu-id="4c602-180">例如，您可能會允許匿名使用者到您的應用程式在 IIS 或[HTTP.sys](xref:fundamentals/servers/weblistener)圖層在控制器層級但授權使用者。</span><span class="sxs-lookup"><span data-stu-id="4c602-180">As an example, you may be allowing anonymous users into your application at the IIS or [HTTP.sys](xref:fundamentals/servers/weblistener) layer but authorizing users at the Controller level.</span></span> <span data-ttu-id="4c602-181">在此案例中，設定的預設配置為`IISDefaults.AuthenticationScheme`中`ConfigureServices`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-181">In this scenario, set the default scheme to `IISDefaults.AuthenticationScheme` in the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(IISDefaults.AuthenticationScheme);
```

<span data-ttu-id="4c602-182">若未設定的預設配置據以防止授權要求挑戰無法運作。</span><span class="sxs-lookup"><span data-stu-id="4c602-182">Failure to set the default scheme accordingly prevents the authorize request to challenge from working.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="4c602-183">IdentityCookieOptions 執行個體</span><span class="sxs-lookup"><span data-stu-id="4c602-183">IdentityCookieOptions Instances</span></span>
<span data-ttu-id="4c602-184">2.0 變更的副作用是切換到使用名為選項，而不是 cookie 的選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="4c602-184">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="4c602-185">移除自訂身分識別 cookie 結構描述名稱的功能。</span><span class="sxs-lookup"><span data-stu-id="4c602-185">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="4c602-186">比方說，1.x 專案使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)傳遞`IdentityCookieOptions`參數插入*AccountController.cs*。</span><span class="sxs-lookup"><span data-stu-id="4c602-186">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs*.</span></span> <span data-ttu-id="4c602-187">從提供的執行個體來存取外部的 cookie 驗證配置：</span><span class="sxs-lookup"><span data-stu-id="4c602-187">The external cookie authentication scheme is accessed from the provided instance:</span></span>

<span data-ttu-id="4c602-188">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]</span><span class="sxs-lookup"><span data-stu-id="4c602-188">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]</span></span>

<span data-ttu-id="4c602-189">上述建構函式的資料隱碼變成不需要在 2.0 專案中，而`_externalCookieScheme`刪除欄位，可以：</span><span class="sxs-lookup"><span data-stu-id="4c602-189">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

<span data-ttu-id="4c602-190">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]</span><span class="sxs-lookup"><span data-stu-id="4c602-190">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]</span></span>

<span data-ttu-id="4c602-191">`IdentityConstants.ExternalScheme`常數可以直接使用：</span><span class="sxs-lookup"><span data-stu-id="4c602-191">The `IdentityConstants.ExternalScheme` constant can be used directly:</span></span>

<span data-ttu-id="4c602-192">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span><span class="sxs-lookup"><span data-stu-id="4c602-192">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]</span></span>

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="4c602-193">新增 IdentityUser POCO 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="4c602-193">Add IdentityUser POCO Navigation Properties</span></span>
<span data-ttu-id="4c602-194">基底的 Entity Framework (EF) 核心導覽屬性`IdentityUser`POCO （純舊 CLR 物件） 已經移除。</span><span class="sxs-lookup"><span data-stu-id="4c602-194">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="4c602-195">如果 1.x 專案使用這些屬性，以手動方式將它們加回 2.0 的專案：</span><span class="sxs-lookup"><span data-stu-id="4c602-195">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="4c602-196">若要執行 EF 核心移轉時，請避免重複的外部索引鍵，將下列內容加入您`IdentityDbContext`類別`OnModelCreating`方法 (之後`base.OnModelCreating();`呼叫):</span><span class="sxs-lookup"><span data-stu-id="4c602-196">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

```csharp
protected override void OnModelCreating(ModelBuilder builder)
{
    base.OnModelCreating(builder);
    // Customize the ASP.NET Identity model and override the defaults if needed.
    // For example, you can rename the ASP.NET Identity table names and more.
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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="4c602-197">取代 GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="4c602-197">Replace GetExternalAuthenticationSchemes</span></span>
<span data-ttu-id="4c602-198">同步方法`GetExternalAuthenticationSchemes`改用非同步版本中移除。</span><span class="sxs-lookup"><span data-stu-id="4c602-198">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="4c602-199">1.x 專案中有下列的程式碼*ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="4c602-199">1.x projects have the following code in *ManageController.cs*:</span></span>

<span data-ttu-id="4c602-200">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]</span><span class="sxs-lookup"><span data-stu-id="4c602-200">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]</span></span>

<span data-ttu-id="4c602-201">這個方法會出現在*Login.cshtml*太：</span><span class="sxs-lookup"><span data-stu-id="4c602-201">This method appears in *Login.cshtml* too:</span></span>

<span data-ttu-id="4c602-202">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Account/Login.cshtml?range=62,75-84)]</span><span class="sxs-lookup"><span data-stu-id="4c602-202">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Account/Login.cshtml?range=62,75-84)]</span></span>

<span data-ttu-id="4c602-203">在 2.0 專案中，使用`GetExternalAuthenticationSchemesAsync`方法：</span><span class="sxs-lookup"><span data-stu-id="4c602-203">In 2.0 projects, use the `GetExternalAuthenticationSchemesAsync` method:</span></span>

<span data-ttu-id="4c602-204">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]</span><span class="sxs-lookup"><span data-stu-id="4c602-204">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]</span></span>

<span data-ttu-id="4c602-205">在*Login.cshtml*、`AuthenticationScheme`中存取屬性`foreach`迴圈變為`Name`:</span><span class="sxs-lookup"><span data-stu-id="4c602-205">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

<span data-ttu-id="4c602-206">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Views/Account/Login.cshtml?range=62,75-84)]</span><span class="sxs-lookup"><span data-stu-id="4c602-206">[!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Views/Account/Login.cshtml?range=62,75-84)]</span></span>

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="4c602-207">ManageLoginsViewModel 屬性變更</span><span class="sxs-lookup"><span data-stu-id="4c602-207">ManageLoginsViewModel Property Change</span></span>
<span data-ttu-id="4c602-208">A`ManageLoginsViewModel`物件用於`ManageLogins`動作*ManageController.cs*。</span><span class="sxs-lookup"><span data-stu-id="4c602-208">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="4c602-209">1.x 專案中，物件的`OtherLogins`屬性傳回型別是`IList<AuthenticationDescription>`。</span><span class="sxs-lookup"><span data-stu-id="4c602-209">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="4c602-210">這個傳回類型需要先匯入`Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="4c602-210">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

<span data-ttu-id="4c602-211">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span><span class="sxs-lookup"><span data-stu-id="4c602-211">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span></span>

<span data-ttu-id="4c602-212">在 2.0 專案中，傳回型別變更為`IList<AuthenticationScheme>`。</span><span class="sxs-lookup"><span data-stu-id="4c602-212">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="4c602-213">這個新的傳回型別必須替換`Microsoft.AspNetCore.Http.Authentication`匯入`Microsoft.AspNetCore.Authentication`匯入。</span><span class="sxs-lookup"><span data-stu-id="4c602-213">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

<span data-ttu-id="4c602-214">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span><span class="sxs-lookup"><span data-stu-id="4c602-214">[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]</span></span>

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="4c602-215">其他資源</span><span class="sxs-lookup"><span data-stu-id="4c602-215">Additional Resources</span></span>
<span data-ttu-id="4c602-216">如需其他詳細資料和討論，請參閱[Auth 2.0 討論](https://github.com/aspnet/Security/issues/1338)GitHub 上的問題。</span><span class="sxs-lookup"><span data-stu-id="4c602-216">For additional details and discussion, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
