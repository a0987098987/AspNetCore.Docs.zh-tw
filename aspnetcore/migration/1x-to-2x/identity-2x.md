---
title: 將驗證和身分識別移轉至 ASP.NET Core 2.0
author: scottaddie
description: 本文概述了最常見的步驟移轉 ASP.NET Core 1.x 驗證和身分識別為 ASP.NET Core 2.0。
ms.author: scaddie
ms.date: 06/21/2019
uid: migration/1x-to-2x/identity-2x
ms.openlocfilehash: c83356e12fa5ae581b369265b9d857b08445ed51
ms.sourcegitcommit: 9f11685382eb1f4dd0fb694dea797adacedf9e20
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/21/2019
ms.locfileid: "67313737"
---
# <a name="migrate-authentication-and-identity-to-aspnet-core-20"></a><span data-ttu-id="e17af-103">將驗證和身分識別移轉至 ASP.NET Core 2.0</span><span class="sxs-lookup"><span data-stu-id="e17af-103">Migrate authentication and Identity to ASP.NET Core 2.0</span></span>

<span data-ttu-id="e17af-104">藉由[Scott Addie](https://github.com/scottaddie)和[Hao 是一隻](https://github.com/HaoK)</span><span class="sxs-lookup"><span data-stu-id="e17af-104">By [Scott Addie](https://github.com/scottaddie) and [Hao Kung](https://github.com/HaoK)</span></span>

<span data-ttu-id="e17af-105">ASP.NET Core 2.0 具有新的模型進行驗證並[識別](xref:security/authentication/identity)，可簡化使用服務組態。</span><span class="sxs-lookup"><span data-stu-id="e17af-105">ASP.NET Core 2.0 has a new model for authentication and [Identity](xref:security/authentication/identity) that simplifies configuration by using services.</span></span> <span data-ttu-id="e17af-106">使用驗證或識別的 ASP.NET Core 1.x 應用程式可以更新為使用新的模型，如下所述。</span><span class="sxs-lookup"><span data-stu-id="e17af-106">ASP.NET Core 1.x applications that use authentication or Identity can be updated to use the new model as outlined below.</span></span>

## <a name="update-namespaces"></a><span data-ttu-id="e17af-107">更新命名空間</span><span class="sxs-lookup"><span data-stu-id="e17af-107">Update namespaces</span></span>

<span data-ttu-id="e17af-108">在 1.x 中，類別這類`IdentityRole`並`IdentityUser`中找不到`Microsoft.AspNetCore.Identity.EntityFrameworkCore`命名空間。</span><span class="sxs-lookup"><span data-stu-id="e17af-108">In 1.x, classes such `IdentityRole` and `IdentityUser` were found in the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` namespace.</span></span>

<span data-ttu-id="e17af-109">在 2.0 中，<xref:Microsoft.AspNetCore.Identity>命名空間成為數個這類類別的新家。</span><span class="sxs-lookup"><span data-stu-id="e17af-109">In 2.0, the <xref:Microsoft.AspNetCore.Identity> namespace became the new home for several of such classes.</span></span> <span data-ttu-id="e17af-110">使用預設的身分識別程式碼，包括受影響的類別`ApplicationUser`和`Startup`。</span><span class="sxs-lookup"><span data-stu-id="e17af-110">With the default Identity code, affected classes include `ApplicationUser` and `Startup`.</span></span> <span data-ttu-id="e17af-111">調整您`using`陳述式來解析受影響的參考。</span><span class="sxs-lookup"><span data-stu-id="e17af-111">Adjust your `using` statements to resolve the affected references.</span></span>

<a name="auth-middleware"></a>

## <a name="authentication-middleware-and-services"></a><span data-ttu-id="e17af-112">驗證中介軟體和服務</span><span class="sxs-lookup"><span data-stu-id="e17af-112">Authentication Middleware and services</span></span>

<span data-ttu-id="e17af-113">在 1.x 專案中，驗證已透過中介軟體。</span><span class="sxs-lookup"><span data-stu-id="e17af-113">In 1.x projects, authentication is configured via middleware.</span></span> <span data-ttu-id="e17af-114">中介軟體的方法會叫用每個您想要支援的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="e17af-114">A middleware method is invoked for each authentication scheme you want to support.</span></span>

<span data-ttu-id="e17af-115">下列範例中，1.x 設定 Facebook 驗證使用中身分識別*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-115">The following 1.x example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="e17af-116">在 2.0 專案中，驗證是透過服務設定。</span><span class="sxs-lookup"><span data-stu-id="e17af-116">In 2.0 projects, authentication is configured via services.</span></span> <span data-ttu-id="e17af-117">在中註冊每個驗證配置`ConfigureServices`方法*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="e17af-117">Each authentication scheme is registered in the `ConfigureServices` method of *Startup.cs*.</span></span> <span data-ttu-id="e17af-118">`UseIdentity`方法會取代`UseAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="e17af-118">The `UseIdentity` method is replaced with `UseAuthentication`.</span></span>

<span data-ttu-id="e17af-119">下例 2.0 設定 Facebook 驗證使用中身分識別*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-119">The following 2.0 example configures Facebook authentication with Identity in *Startup.cs*:</span></span>

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

<span data-ttu-id="e17af-120">`UseAuthentication`方法會將單一驗證中介軟體元件，也就是負責自動驗證和遠端驗證要求的處理。</span><span class="sxs-lookup"><span data-stu-id="e17af-120">The `UseAuthentication` method adds a single authentication middleware component, which is responsible for automatic authentication and the handling of remote authentication requests.</span></span> <span data-ttu-id="e17af-121">它會取代所有個別的中介軟體元件的單一、 共同的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="e17af-121">It replaces all of the individual middleware components with a single, common middleware component.</span></span>

<span data-ttu-id="e17af-122">以下是每個主要的驗證配置的 2.0 移轉指示。</span><span class="sxs-lookup"><span data-stu-id="e17af-122">Below are 2.0 migration instructions for each major authentication scheme.</span></span>

### <a name="cookie-based-authentication"></a><span data-ttu-id="e17af-123">以 cookie 為基礎的驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-123">Cookie-based authentication</span></span>

<span data-ttu-id="e17af-124">選取其中一個，在兩個選項，並進行必要的變更，在*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-124">Select one of the two options below, and make the necessary changes in *Startup.cs*:</span></span>

1. <span data-ttu-id="e17af-125">使用身分識別的 cookie</span><span class="sxs-lookup"><span data-stu-id="e17af-125">Use cookies with Identity</span></span>
    - <span data-ttu-id="e17af-126">取代`UseIdentity`具有`UseAuthentication`在`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-126">Replace `UseIdentity` with `UseAuthentication` in the `Configure` method:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="e17af-127">叫用`AddIdentity`方法中的`ConfigureServices`將 cookie 驗證服務的方法。</span><span class="sxs-lookup"><span data-stu-id="e17af-127">Invoke the `AddIdentity` method in the `ConfigureServices` method to add the cookie authentication services.</span></span>
    - <span data-ttu-id="e17af-128">（選擇性） 叫用`ConfigureApplicationCookie`或是`ConfigureExternalCookie`方法中的`ConfigureServices`調整的身分識別的 cookie 設定的方法。</span><span class="sxs-lookup"><span data-stu-id="e17af-128">Optionally, invoke the `ConfigureApplicationCookie` or `ConfigureExternalCookie` method in the `ConfigureServices` method to tweak the Identity cookie settings.</span></span>

        ```csharp
        services.AddIdentity<ApplicationUser, IdentityRole>()
                .AddEntityFrameworkStores<ApplicationDbContext>()
                .AddDefaultTokenProviders();

        services.ConfigureApplicationCookie(options => options.LoginPath = "/Account/LogIn");
        ```

2. <span data-ttu-id="e17af-129">使用沒有身分識別的 cookie</span><span class="sxs-lookup"><span data-stu-id="e17af-129">Use cookies without Identity</span></span>
    - <span data-ttu-id="e17af-130">取代`UseCookieAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-130">Replace the `UseCookieAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

        ```csharp
        app.UseAuthentication();
        ```

    - <span data-ttu-id="e17af-131">叫用`AddAuthentication`並`AddCookie`中的方法`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-131">Invoke the `AddAuthentication` and `AddCookie` methods in the `ConfigureServices` method:</span></span>

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

### <a name="jwt-bearer-authentication"></a><span data-ttu-id="e17af-132">JWT 持有人驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-132">JWT Bearer Authentication</span></span>

<span data-ttu-id="e17af-133">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-133">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e17af-134">取代`UseJwtBearerAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-134">Replace the `UseJwtBearerAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e17af-135">叫用`AddJwtBearer`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-135">Invoke the `AddJwtBearer` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
            .AddJwtBearer(options =>
            {
                options.Audience = "http://localhost:5001/";
                options.Authority = "http://localhost:5000/";
            });
    ```

    <span data-ttu-id="e17af-136">此程式碼片段不會使用身分識別，因此應該藉由傳遞設定的預設配置`JwtBearerDefaults.AuthenticationScheme`至`AddAuthentication`方法。</span><span class="sxs-lookup"><span data-stu-id="e17af-136">This code snippet doesn't use Identity, so the default scheme should be set by passing `JwtBearerDefaults.AuthenticationScheme` to the `AddAuthentication` method.</span></span>

### <a name="openid-connect-oidc-authentication"></a><span data-ttu-id="e17af-137">OpenID Connect (OIDC) 驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-137">OpenID Connect (OIDC) authentication</span></span>

<span data-ttu-id="e17af-138">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-138">Make the following changes in *Startup.cs*:</span></span>

- <span data-ttu-id="e17af-139">取代`UseOpenIdConnectAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-139">Replace the `UseOpenIdConnectAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e17af-140">叫用`AddOpenIdConnect`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-140">Invoke the `AddOpenIdConnect` method in the `ConfigureServices` method:</span></span>

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

- <span data-ttu-id="e17af-141">取代`PostLogoutRedirectUri`中的屬性`OpenIdConnectOptions`動作`SignedOutRedirectUri`:</span><span class="sxs-lookup"><span data-stu-id="e17af-141">Replace the `PostLogoutRedirectUri` property in the `OpenIdConnectOptions` action with `SignedOutRedirectUri`:</span></span>

    ```csharp
    .AddOpenIdConnect(options =>
    {
        options.SignedOutRedirectUri = "https://contoso.com";
    });
    ```
    
### <a name="facebook-authentication"></a><span data-ttu-id="e17af-142">Facebook 驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-142">Facebook authentication</span></span>

<span data-ttu-id="e17af-143">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-143">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e17af-144">取代`UseFacebookAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-144">Replace the `UseFacebookAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e17af-145">叫用`AddFacebook`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-145">Invoke the `AddFacebook` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddFacebook(options =>
            {
                options.AppId = Configuration["auth:facebook:appid"];
                options.AppSecret = Configuration["auth:facebook:appsecret"];
            });
    ```

### <a name="google-authentication"></a><span data-ttu-id="e17af-146">Google 驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-146">Google authentication</span></span>

<span data-ttu-id="e17af-147">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-147">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e17af-148">取代`UseGoogleAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-148">Replace the `UseGoogleAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e17af-149">叫用`AddGoogle`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-149">Invoke the `AddGoogle` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddGoogle(options =>
            {
                options.ClientId = Configuration["auth:google:clientid"];
                options.ClientSecret = Configuration["auth:google:clientsecret"];
            });
    ```

### <a name="microsoft-account-authentication"></a><span data-ttu-id="e17af-150">Microsoft 帳戶驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-150">Microsoft Account authentication</span></span>

<span data-ttu-id="e17af-151">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-151">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e17af-152">取代`UseMicrosoftAccountAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-152">Replace the `UseMicrosoftAccountAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e17af-153">叫用`AddMicrosoftAccount`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-153">Invoke the `AddMicrosoftAccount` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddMicrosoftAccount(options =>
            {
                options.ClientId = Configuration["auth:microsoft:clientid"];
                options.ClientSecret = Configuration["auth:microsoft:clientsecret"];
            });
    ```

### <a name="twitter-authentication"></a><span data-ttu-id="e17af-154">Twitter 驗證</span><span class="sxs-lookup"><span data-stu-id="e17af-154">Twitter authentication</span></span>

<span data-ttu-id="e17af-155">在變更下列*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-155">Make the following changes in *Startup.cs*:</span></span>
- <span data-ttu-id="e17af-156">取代`UseTwitterAuthentication`方法呼叫中`Configure`方法`UseAuthentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-156">Replace the `UseTwitterAuthentication` method call in the `Configure` method with `UseAuthentication`:</span></span>

    ```csharp
    app.UseAuthentication();
    ```

- <span data-ttu-id="e17af-157">叫用`AddTwitter`方法中的`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="e17af-157">Invoke the `AddTwitter` method in the `ConfigureServices` method:</span></span>

    ```csharp
    services.AddAuthentication()
            .AddTwitter(options =>
            {
                options.ConsumerKey = Configuration["auth:twitter:consumerkey"];
                options.ConsumerSecret = Configuration["auth:twitter:consumersecret"];
            });
    ```

### <a name="setting-default-authentication-schemes"></a><span data-ttu-id="e17af-158">設定預設的驗證配置</span><span class="sxs-lookup"><span data-stu-id="e17af-158">Setting default authentication schemes</span></span>

<span data-ttu-id="e17af-159">在 1.x`AutomaticAuthenticate`和`AutomaticChallenge`的屬性[AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1)基底類別要設定一種驗證配置。</span><span class="sxs-lookup"><span data-stu-id="e17af-159">In 1.x, the `AutomaticAuthenticate` and `AutomaticChallenge` properties of the [AuthenticationOptions](/dotnet/api/Microsoft.AspNetCore.Builder.AuthenticationOptions?view=aspnetcore-1.1) base class were intended to be set on a single authentication scheme.</span></span> <span data-ttu-id="e17af-160">發生不好的方法，強制執行。</span><span class="sxs-lookup"><span data-stu-id="e17af-160">There was no good way to enforce this.</span></span>

<span data-ttu-id="e17af-161">在 2.0 中，這兩個屬性有為個別的屬性移除`AuthenticationOptions`執行個體。</span><span class="sxs-lookup"><span data-stu-id="e17af-161">In 2.0, these two properties have been removed as properties on the individual `AuthenticationOptions` instance.</span></span> <span data-ttu-id="e17af-162">在設定這些`AddAuthentication`方法呼叫內`ConfigureServices`方法*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-162">They can be configured in the `AddAuthentication` method call within the `ConfigureServices` method of *Startup.cs*:</span></span>

```csharp
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme);
```

<span data-ttu-id="e17af-163">在上述程式碼片段中，預設配置設為`CookieAuthenticationDefaults.AuthenticationScheme`("Cookie")。</span><span class="sxs-lookup"><span data-stu-id="e17af-163">In the preceding code snippet, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme` ("Cookies").</span></span>

<span data-ttu-id="e17af-164">或者，使用的多載的版本`AddAuthentication`方法來設定多個屬性。</span><span class="sxs-lookup"><span data-stu-id="e17af-164">Alternatively, use an overloaded version of the `AddAuthentication` method to set more than one property.</span></span> <span data-ttu-id="e17af-165">在下列的多載的方法範例中，預設配置設為`CookieAuthenticationDefaults.AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="e17af-165">In the following overloaded method example, the default scheme is set to `CookieAuthenticationDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="e17af-166">驗證配置或者在您的個人內指定`[Authorize]`屬性或授權原則。</span><span class="sxs-lookup"><span data-stu-id="e17af-166">The authentication scheme may alternatively be specified within your individual `[Authorize]` attributes or authorization policies.</span></span>

```csharp
services.AddAuthentication(options =>
{
    options.DefaultScheme = CookieAuthenticationDefaults.AuthenticationScheme;
    options.DefaultChallengeScheme = OpenIdConnectDefaults.AuthenticationScheme;
});
```

<span data-ttu-id="e17af-167">如果下列條件之一成立，請在 2.0 中定義的預設配置：</span><span class="sxs-lookup"><span data-stu-id="e17af-167">Define a default scheme in 2.0 if one of the following conditions is true:</span></span>
- <span data-ttu-id="e17af-168">您希望使用者會自動登入</span><span class="sxs-lookup"><span data-stu-id="e17af-168">You want the user to be automatically signed in</span></span>
- <span data-ttu-id="e17af-169">您使用`[Authorize]`屬性或授權原則，而不指定結構描述</span><span class="sxs-lookup"><span data-stu-id="e17af-169">You use the `[Authorize]` attribute or authorization policies without specifying schemes</span></span>

<span data-ttu-id="e17af-170">此規則的例外是`AddIdentity`方法。</span><span class="sxs-lookup"><span data-stu-id="e17af-170">An exception to this rule is the `AddIdentity` method.</span></span> <span data-ttu-id="e17af-171">這個方法會將 cookie 加入寄件者和預設值進行驗證，而且挑戰應用程式 cookie 配置的集`IdentityConstants.ApplicationScheme`。</span><span class="sxs-lookup"><span data-stu-id="e17af-171">This method adds cookies for you and sets the default authenticate and challenge schemes to the application cookie `IdentityConstants.ApplicationScheme`.</span></span> <span data-ttu-id="e17af-172">此外，它將預設登入的配置設定為外部 cookie `IdentityConstants.ExternalScheme`。</span><span class="sxs-lookup"><span data-stu-id="e17af-172">Additionally, it sets the default sign-in scheme to the external cookie `IdentityConstants.ExternalScheme`.</span></span>

<a name="obsolete-interface"></a>

## <a name="use-httpcontext-authentication-extensions"></a><span data-ttu-id="e17af-173">使用 HttpContext 驗證延伸模組</span><span class="sxs-lookup"><span data-stu-id="e17af-173">Use HttpContext authentication extensions</span></span>

<span data-ttu-id="e17af-174">`IAuthenticationManager`介面是 1.x 驗證系統的主要進入點。</span><span class="sxs-lookup"><span data-stu-id="e17af-174">The `IAuthenticationManager` interface is the main entry point into the 1.x authentication system.</span></span> <span data-ttu-id="e17af-175">它已取代為一組新的`HttpContext`中的擴充方法`Microsoft.AspNetCore.Authentication`命名空間。</span><span class="sxs-lookup"><span data-stu-id="e17af-175">It has been replaced with a new set of `HttpContext` extension methods in the `Microsoft.AspNetCore.Authentication` namespace.</span></span>

<span data-ttu-id="e17af-176">比方說，1.x 專案參考`Authentication`屬性：</span><span class="sxs-lookup"><span data-stu-id="e17af-176">For example, 1.x projects reference an `Authentication` property:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="e17af-177">在 2.0 專案中，匯入`Microsoft.AspNetCore.Authentication`命名空間，並刪除`Authentication`屬性參考：</span><span class="sxs-lookup"><span data-stu-id="e17af-177">In 2.0 projects, import the `Microsoft.AspNetCore.Authentication` namespace, and delete the `Authentication` property references:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<a name="windows-auth-changes"></a>

## <a name="windows-authentication-httpsys--iisintegration"></a><span data-ttu-id="e17af-178">Windows 驗證 (HTTP.sys / IISIntegration)</span><span class="sxs-lookup"><span data-stu-id="e17af-178">Windows Authentication (HTTP.sys / IISIntegration)</span></span>

<span data-ttu-id="e17af-179">有兩種變化的 Windows 驗證：</span><span class="sxs-lookup"><span data-stu-id="e17af-179">There are two variations of Windows authentication:</span></span>

* <span data-ttu-id="e17af-180">主應用程式只允許已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="e17af-180">The host only allows authenticated users.</span></span> <span data-ttu-id="e17af-181">這項差異不會受到 2.0 的變更。</span><span class="sxs-lookup"><span data-stu-id="e17af-181">This variation isn't affected by the 2.0 changes.</span></span>
* <span data-ttu-id="e17af-182">主機允許匿名和已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="e17af-182">The host allows both anonymous and authenticated users.</span></span> <span data-ttu-id="e17af-183">這項差異會影響 2.0 的變更。</span><span class="sxs-lookup"><span data-stu-id="e17af-183">This variation is affected by the 2.0 changes.</span></span> <span data-ttu-id="e17af-184">例如，應用程式應該允許匿名使用者[IIS](xref:host-and-deploy/iis/index)或是[HTTP.sys](xref:fundamentals/servers/httpsys)圖層，但是授權在控制器層級的使用者。</span><span class="sxs-lookup"><span data-stu-id="e17af-184">For example, the app should allow anonymous users at the [IIS](xref:host-and-deploy/iis/index) or [HTTP.sys](xref:fundamentals/servers/httpsys) layer but authorize users at the controller level.</span></span> <span data-ttu-id="e17af-185">在此案例中，請在中設定的預設配置`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="e17af-185">In this scenario, set the default scheme in the `Startup.ConfigureServices` method.</span></span>

  <span data-ttu-id="e17af-186">針對[Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/)，若要設定的預設配置`IISDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="e17af-186">For [Microsoft.AspNetCore.Server.IISIntegration](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.IISIntegration/), set the default scheme to `IISDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.IISIntegration;

  services.AddAuthentication(IISDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="e17af-187">針對[Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/)，若要設定的預設配置`HttpSysDefaults.AuthenticationScheme`:</span><span class="sxs-lookup"><span data-stu-id="e17af-187">For [Microsoft.AspNetCore.Server.HttpSys](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.HttpSys/), set the default scheme to `HttpSysDefaults.AuthenticationScheme`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Server.HttpSys;

  services.AddAuthentication(HttpSysDefaults.AuthenticationScheme);
  ```

  <span data-ttu-id="e17af-188">若要設定的預設配置失敗可防止授權 （挑戰） 要求使用下列的例外狀況：</span><span class="sxs-lookup"><span data-stu-id="e17af-188">Failure to set the default scheme prevents the authorize (challenge) request from working with the following exception:</span></span>

  > <span data-ttu-id="e17af-189">`System.InvalidOperationException`：指定沒有 authenticationScheme，並沒有找到任何 DefaultChallengeScheme。</span><span class="sxs-lookup"><span data-stu-id="e17af-189">`System.InvalidOperationException`: No authenticationScheme was specified, and there was no DefaultChallengeScheme found.</span></span>

<span data-ttu-id="e17af-190">如需詳細資訊，請參閱 <xref:security/authentication/windowsauth>。</span><span class="sxs-lookup"><span data-stu-id="e17af-190">For more information, see <xref:security/authentication/windowsauth>.</span></span>

<a name="identity-cookie-options"></a>

## <a name="identitycookieoptions-instances"></a><span data-ttu-id="e17af-191">IdentityCookieOptions 執行個體</span><span class="sxs-lookup"><span data-stu-id="e17af-191">IdentityCookieOptions instances</span></span>

<span data-ttu-id="e17af-192">2\.0 變更的副作用是切換到使用名為選項，而不是 cookie 選項執行個體。</span><span class="sxs-lookup"><span data-stu-id="e17af-192">A side effect of the 2.0 changes is the switch to using named options instead of cookie options instances.</span></span> <span data-ttu-id="e17af-193">不再能夠自訂身分識別的 cookie 配置名稱。</span><span class="sxs-lookup"><span data-stu-id="e17af-193">The ability to customize the Identity cookie scheme names is removed.</span></span>

<span data-ttu-id="e17af-194">比方說，1.x 專案使用[建構函式插入](xref:mvc/controllers/dependency-injection#constructor-injection)傳遞`IdentityCookieOptions`參數插入*AccountController.cs*並*ManageController.cs*。</span><span class="sxs-lookup"><span data-stu-id="e17af-194">For example, 1.x projects use [constructor injection](xref:mvc/controllers/dependency-injection#constructor-injection) to pass an `IdentityCookieOptions` parameter into *AccountController.cs* and *ManageController.cs*.</span></span> <span data-ttu-id="e17af-195">從提供的執行個體來存取外部的 cookie 驗證配置：</span><span class="sxs-lookup"><span data-stu-id="e17af-195">The external cookie authentication scheme is accessed from the provided instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor&highlight=4,11)]

<span data-ttu-id="e17af-196">上述建構函式插入將不再需要在 2.0 專案中，而`_externalCookieScheme`欄位可以刪除：</span><span class="sxs-lookup"><span data-stu-id="e17af-196">The aforementioned constructor injection becomes unnecessary in 2.0 projects, and the `_externalCookieScheme` field can be deleted:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AccountControllerConstructor)]

<span data-ttu-id="e17af-197">1.x 專案中使用`_externalCookieScheme`欄位，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e17af-197">1.x projects used the `_externalCookieScheme` field as follows:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="e17af-198">在 2.0 專案中，以下列內容取代上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="e17af-198">In 2.0 projects, replace the preceding code with the following.</span></span> <span data-ttu-id="e17af-199">`IdentityConstants.ExternalScheme`常數可以直接使用。</span><span class="sxs-lookup"><span data-stu-id="e17af-199">The `IdentityConstants.ExternalScheme` constant can be used directly.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationProperty)]

<span data-ttu-id="e17af-200">解決新增`SignOutAsync`呼叫匯入下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="e17af-200">Resolve the newly added `SignOutAsync` call by importing the following namespace:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/AccountController.cs?name=snippet_AuthenticationImport)]

<a name="navigation-properties"></a>

## <a name="add-identityuser-poco-navigation-properties"></a><span data-ttu-id="e17af-201">加入 POCO IdentityUser 導覽屬性</span><span class="sxs-lookup"><span data-stu-id="e17af-201">Add IdentityUser POCO navigation properties</span></span>

<span data-ttu-id="e17af-202">Entity Framework (EF) Core 導覽屬性的基底`IdentityUser`POCO （純舊 CLR 物件） 已移除。</span><span class="sxs-lookup"><span data-stu-id="e17af-202">The Entity Framework (EF) Core navigation properties of the base `IdentityUser` POCO (Plain Old CLR Object) have been removed.</span></span> <span data-ttu-id="e17af-203">如果您的 1.x 專案會使用這些屬性，以手動方式將他們新增回 2.0 的專案：</span><span class="sxs-lookup"><span data-stu-id="e17af-203">If your 1.x project used these properties, manually add them back to the 2.0 project:</span></span>

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

<span data-ttu-id="e17af-204">若要執行 EF Core 移轉時，請避免重複的外部索引鍵，將下列內容加入您`IdentityDbContext`類別`OnModelCreating`方法 (之後`base.OnModelCreating();`呼叫):</span><span class="sxs-lookup"><span data-stu-id="e17af-204">To prevent duplicate foreign keys when running EF Core Migrations, add the following to your `IdentityDbContext` class' `OnModelCreating` method (after the `base.OnModelCreating();` call):</span></span>

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

## <a name="replace-getexternalauthenticationschemes"></a><span data-ttu-id="e17af-205">取代 GetExternalAuthenticationSchemes</span><span class="sxs-lookup"><span data-stu-id="e17af-205">Replace GetExternalAuthenticationSchemes</span></span>

<span data-ttu-id="e17af-206">同步方法`GetExternalAuthenticationSchemes`移除是因為希望的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="e17af-206">The synchronous method `GetExternalAuthenticationSchemes` was removed in favor of an asynchronous version.</span></span> <span data-ttu-id="e17af-207">1.x 專案中有下列的程式碼*Controllers/ManageController.cs*:</span><span class="sxs-lookup"><span data-stu-id="e17af-207">1.x projects have the following code in *Controllers/ManageController.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemes)]

<span data-ttu-id="e17af-208">這個方法會出現在*Views/Account/Login.cshtml*太：</span><span class="sxs-lookup"><span data-stu-id="e17af-208">This method appears in *Views/Account/Login.cshtml* too:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemes&highlight=2)]

<span data-ttu-id="e17af-209">在 2.0 專案中，使用<xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*>方法。</span><span class="sxs-lookup"><span data-stu-id="e17af-209">In 2.0 projects, use the <xref:Microsoft.AspNetCore.Identity.SignInManager`1.GetExternalAuthenticationSchemesAsync*> method.</span></span> <span data-ttu-id="e17af-210">中的變更*ManageController.cs*類似下列的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e17af-210">The change in *ManageController.cs* resembles the following code:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Controllers/ManageController.cs?name=snippet_GetExternalAuthenticationSchemesAsync)]

<span data-ttu-id="e17af-211">在  *Login.cshtml*，則`AuthenticationScheme`中存取屬性`foreach`迴圈變更為`Name`:</span><span class="sxs-lookup"><span data-stu-id="e17af-211">In *Login.cshtml*, the `AuthenticationScheme` property accessed in the `foreach` loop changes to `Name`:</span></span>

[!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Views/Account/Login.cshtml?name=snippet_GetExtAuthNSchemesAsync&highlight=2,19)]

<a name="property-change"></a>

## <a name="manageloginsviewmodel-property-change"></a><span data-ttu-id="e17af-212">ManageLoginsViewModel 屬性變更</span><span class="sxs-lookup"><span data-stu-id="e17af-212">ManageLoginsViewModel property change</span></span>

<span data-ttu-id="e17af-213">A`ManageLoginsViewModel`中使用物件`ManageLogins`動作*ManageController.cs*。</span><span class="sxs-lookup"><span data-stu-id="e17af-213">A `ManageLoginsViewModel` object is used in the `ManageLogins` action of *ManageController.cs*.</span></span> <span data-ttu-id="e17af-214">在 1.x 專案中，該物件的`OtherLogins`屬性傳回型別是`IList<AuthenticationDescription>`。</span><span class="sxs-lookup"><span data-stu-id="e17af-214">In 1.x projects, the object's `OtherLogins` property return type is `IList<AuthenticationDescription>`.</span></span> <span data-ttu-id="e17af-215">這個傳回型別需要先匯入`Microsoft.AspNetCore.Http.Authentication`:</span><span class="sxs-lookup"><span data-stu-id="e17af-215">This return type requires an import of `Microsoft.AspNetCore.Http.Authentication`:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<span data-ttu-id="e17af-216">在 2.0 專案中，傳回的型別變更為  `IList<AuthenticationScheme>`。</span><span class="sxs-lookup"><span data-stu-id="e17af-216">In 2.0 projects, the return type changes to `IList<AuthenticationScheme>`.</span></span> <span data-ttu-id="e17af-217">這個新的傳回型別必須替換`Microsoft.AspNetCore.Http.Authentication`匯入具有`Microsoft.AspNetCore.Authentication`匯入。</span><span class="sxs-lookup"><span data-stu-id="e17af-217">This new return type requires replacing the `Microsoft.AspNetCore.Http.Authentication` import with a `Microsoft.AspNetCore.Authentication` import.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Models/ManageViewModels/ManageLoginsViewModel.cs?name=snippet_ManageLoginsViewModel&highlight=2,11)]

<a name="additional-resources"></a>

## <a name="additional-resources"></a><span data-ttu-id="e17af-218">其他資源</span><span class="sxs-lookup"><span data-stu-id="e17af-218">Additional resources</span></span>

<span data-ttu-id="e17af-219">如需詳細資訊，請參閱 < [Auth 2.0 討論](https://github.com/aspnet/Security/issues/1338)GitHub 上的提出問題。</span><span class="sxs-lookup"><span data-stu-id="e17af-219">For more information, see the [Discussion for Auth 2.0](https://github.com/aspnet/Security/issues/1338) issue on GitHub.</span></span>
