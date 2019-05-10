---
title: ASP.NET Core 中的特定結構描述的授權
author: rick-anderson
description: 這篇文章說明如何使用多個驗證方法時，限制至特定的結構描述的身分識別。
ms.author: riande
ms.date: 10/22/2018
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 778bb61f472ab2e76f85da5999d3c79238188f19
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897335"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="326b9-103">ASP.NET Core 中的特定結構描述的授權</span><span class="sxs-lookup"><span data-stu-id="326b9-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="326b9-104">在某些情況下，例如單一頁面應用程式 (Spa)，它會使用多個驗證方法。</span><span class="sxs-lookup"><span data-stu-id="326b9-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="326b9-105">比方說，應用程式可能會使用以 cookie 為基礎的驗證來登入和 JWT 持有人驗證 JavaScript 要求。</span><span class="sxs-lookup"><span data-stu-id="326b9-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="326b9-106">在某些情況下，應用程式可能有多個執行個體的驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="326b9-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="326b9-107">例如，兩個，其中包含基本的身分識別的 cookie 處理常式，另一個被建立時已觸發 multi-factor authentication (MFA)。</span><span class="sxs-lookup"><span data-stu-id="326b9-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="326b9-108">因為使用者要求的作業需要額外的安全性，可能會觸發 MFA。</span><span class="sxs-lookup"><span data-stu-id="326b9-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="326b9-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="326b9-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="326b9-110">驗證服務設定期間驗證時，名為一種驗證配置。</span><span class="sxs-lookup"><span data-stu-id="326b9-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="326b9-111">例如：</span><span class="sxs-lookup"><span data-stu-id="326b9-111">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication()
        .AddCookie(options => {
            options.LoginPath = "/Account/Unauthorized/";
            options.AccessDeniedPath = "/Account/Forbidden/";
        })
        .AddJwtBearer(options => {
            options.Audience = "http://localhost:5001/";
            options.Authority = "http://localhost:5000/";
        });
```

<span data-ttu-id="326b9-112">在上述程式碼中，已新增兩個驗證處理常式： 一個用於 cookie，一個用於持有人。</span><span class="sxs-lookup"><span data-stu-id="326b9-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="326b9-113">指定預設結構描述會導致`HttpContext.User`屬性設定為該身分識別。</span><span class="sxs-lookup"><span data-stu-id="326b9-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="326b9-114">如果不需要該行為，將它停用藉由叫用的無參數形式`AddAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="326b9-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="326b9-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="326b9-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="326b9-116">在驗證期間設定驗證中介軟體時，會命名為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="326b9-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="326b9-117">例如：</span><span class="sxs-lookup"><span data-stu-id="326b9-117">For example:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env, ILoggerFactory loggerFactory)
{
    // Code omitted for brevity

    app.UseCookieAuthentication(new CookieAuthenticationOptions()
    {
        AuthenticationScheme = "Cookie",
        LoginPath = "/Account/Unauthorized/",
        AccessDeniedPath = "/Account/Forbidden/",
        AutomaticAuthenticate = false
    });
    
    app.UseJwtBearerAuthentication(new JwtBearerOptions()
    {
        AuthenticationScheme = "Bearer",
        AutomaticAuthenticate = false,
        Audience = "http://localhost:5001/",
        Authority = "http://localhost:5000/",
        RequireHttpsMetadata = false
    });
```

<span data-ttu-id="326b9-118">在上述程式碼中，已新增兩個驗證中介軟體： 一個用於 cookie，一個用於持有人。</span><span class="sxs-lookup"><span data-stu-id="326b9-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="326b9-119">指定預設結構描述會導致`HttpContext.User`屬性設定為該身分識別。</span><span class="sxs-lookup"><span data-stu-id="326b9-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="326b9-120">如果不需要該行為，將它停用藉由設定`AuthenticationOptions.AutomaticAuthenticate`屬性設`false`。</span><span class="sxs-lookup"><span data-stu-id="326b9-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="326b9-121">選取 Authorize 屬性結構描述</span><span class="sxs-lookup"><span data-stu-id="326b9-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="326b9-122">在 授權應用程式會指出要使用的處理常式。</span><span class="sxs-lookup"><span data-stu-id="326b9-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="326b9-123">選取與應用程式會藉由傳遞以逗號分隔清單中的驗證配置授權的處理常式`[Authorize]`。</span><span class="sxs-lookup"><span data-stu-id="326b9-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="326b9-124">`[Authorize]`屬性會指定驗證配置，或是不論預設值設定使用的配置。</span><span class="sxs-lookup"><span data-stu-id="326b9-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="326b9-125">例如：</span><span class="sxs-lookup"><span data-stu-id="326b9-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="326b9-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="326b9-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="326b9-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="326b9-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = AuthSchemes)]
public class MixedController : Controller
    // Requires the following imports:
    // using Microsoft.AspNetCore.Authentication.Cookies;
    // using Microsoft.AspNetCore.Authentication.JwtBearer;
    private const string AuthSchemes =
        CookieAuthenticationDefaults.AuthenticationScheme + "," +
        JwtBearerDefaults.AuthenticationScheme;
```

---

<span data-ttu-id="326b9-128">在上述範例中，將 cookie 與 持有人的處理常式會執行，並有機會建立並附加目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="326b9-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="326b9-129">藉由指定單一配置，會執行對應的處理常式。</span><span class="sxs-lookup"><span data-stu-id="326b9-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="326b9-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="326b9-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="326b9-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="326b9-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="326b9-132">在上述程式碼中，只有具有"Bearer"配置處理常式會執行。</span><span class="sxs-lookup"><span data-stu-id="326b9-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="326b9-133">任何以 cookie 為基礎的身分識別會被忽略。</span><span class="sxs-lookup"><span data-stu-id="326b9-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="326b9-134">選取的配置原則</span><span class="sxs-lookup"><span data-stu-id="326b9-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="326b9-135">如果您想要指定在所需的配置[原則](xref:security/authorization/policies)，您可以設定`AuthenticationSchemes`集合加入您的原則時：</span><span class="sxs-lookup"><span data-stu-id="326b9-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("Over18", policy =>
    {
        policy.AuthenticationSchemes.Add(JwtBearerDefaults.AuthenticationScheme);
        policy.RequireAuthenticatedUser();
        policy.Requirements.Add(new MinimumAgeRequirement());
    });
});
```

<span data-ttu-id="326b9-136">在上述範例中，"Over18"原則只會針對執行 「 持有人 」 處理常式所建立的身分識別。</span><span class="sxs-lookup"><span data-stu-id="326b9-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="326b9-137">使用原則，只要`[Authorize]`屬性的`Policy`屬性：</span><span class="sxs-lookup"><span data-stu-id="326b9-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="326b9-138">使用多個驗證配置</span><span class="sxs-lookup"><span data-stu-id="326b9-138">Use multiple authentication schemes</span></span>

<span data-ttu-id="326b9-139">某些應用程式可能需要支援多種類型的驗證。</span><span class="sxs-lookup"><span data-stu-id="326b9-139">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="326b9-140">例如，您的應用程式可能會驗證使用者從 Azure Active Directory 和使用者資料庫。</span><span class="sxs-lookup"><span data-stu-id="326b9-140">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="326b9-141">另一個範例是驗證使用者，從 Active Directory Federation Services 與 Azure Active Directory B2C 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="326b9-141">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="326b9-142">在此情況下，應用程式應該接受從數個簽發者的 JWT 持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="326b9-142">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="326b9-143">新增所有您想要接受的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="326b9-143">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="326b9-144">例如，下列程式碼中`Startup.ConfigureServices`新增兩個不同的簽發者使用 JWT 持有人驗證配置：</span><span class="sxs-lookup"><span data-stu-id="326b9-144">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthentication(JwtBearerDefaults.AuthenticationScheme)
        .AddJwtBearer(options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://localhost:5000/identity/";
        })
        .AddJwtBearer("AzureAD", options =>
        {
            options.Audience = "https://localhost:5000/";
            options.Authority = "https://login.microsoftonline.com/eb971100-6f99-4bdc-8611-1bc8edd7f436/";
        });
}
```

> [!NOTE]
> <span data-ttu-id="326b9-145">只有一個 JWT 持有人驗證已註冊的預設驗證配置`JwtBearerDefaults.AuthenticationScheme`。</span><span class="sxs-lookup"><span data-stu-id="326b9-145">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="326b9-146">註冊以唯一的驗證配置需要額外的驗證。</span><span class="sxs-lookup"><span data-stu-id="326b9-146">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="326b9-147">下一個步驟是更新預設的授權原則，以接受兩個驗證配置。</span><span class="sxs-lookup"><span data-stu-id="326b9-147">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="326b9-148">例如：</span><span class="sxs-lookup"><span data-stu-id="326b9-148">For example:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Code omitted for brevity

    services.AddAuthorization(options =>
    {
        var defaultAuthorizationPolicyBuilder = new AuthorizationPolicyBuilder(
            JwtBearerDefaults.AuthenticationScheme,
            "AzureAD");
        defaultAuthorizationPolicyBuilder = 
            defaultAuthorizationPolicyBuilder.RequireAuthenticatedUser();
        options.DefaultPolicy = defaultAuthorizationPolicyBuilder.Build();
    });
}
```

<span data-ttu-id="326b9-149">因為預設的授權原則覆寫時，就可以使用`[Authorize]`控制器中的屬性。</span><span class="sxs-lookup"><span data-stu-id="326b9-149">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="326b9-150">然後，控制器會接受要求的第一個或第二個簽發者所簽發的 jwt。</span><span class="sxs-lookup"><span data-stu-id="326b9-150">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
