---
title: 使用特定的結構描述中 ASP.NET Core 授權
author: rick-anderson
description: 本文說明如何使用多個驗證方法時，限制特定的結構描述的識別。
ms.author: riande
ms.date: 10/12/2017
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 231c664006ee7ff91f471aa8d16c1fd18dcbabb1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278196"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="a06a8-103">使用特定的結構描述中 ASP.NET Core 授權</span><span class="sxs-lookup"><span data-stu-id="a06a8-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="a06a8-104">在某些情況下，例如單一頁面應用程式 (SPAs)，它會使用多個驗證方法。</span><span class="sxs-lookup"><span data-stu-id="a06a8-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="a06a8-105">例如，應用程式可能會使用 cookie 基本驗證來登入和 JWT bearer 驗證進行 JavaScript 要求。</span><span class="sxs-lookup"><span data-stu-id="a06a8-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="a06a8-106">在某些情況下，應用程式可能會有多個執行個體的驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="a06a8-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="a06a8-107">例如，兩個位置其中一個包含基本的身分識別的 cookie 處理常式，另一個會建立時已觸發多因素驗證 (MFA)。</span><span class="sxs-lookup"><span data-stu-id="a06a8-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="a06a8-108">因為使用者要求的作業需要額外的安全性，可能會觸發 MFA。</span><span class="sxs-lookup"><span data-stu-id="a06a8-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a06a8-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a06a8-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="a06a8-110">在驗證期間設定的驗證服務時，會命名為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="a06a8-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="a06a8-111">例如: </span><span class="sxs-lookup"><span data-stu-id="a06a8-111">For example:</span></span>

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

<span data-ttu-id="a06a8-112">在上述程式碼中，已經加入兩個驗證處理常式： 一個用於 cookie，另一個用於承載。</span><span class="sxs-lookup"><span data-stu-id="a06a8-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="a06a8-113">指定預設的配置會導致`HttpContext.User`屬性設定為該身分識別。</span><span class="sxs-lookup"><span data-stu-id="a06a8-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="a06a8-114">如果不需要該行為，請停用它叫用的無參數形式`AddAuthentication`。</span><span class="sxs-lookup"><span data-stu-id="a06a8-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a06a8-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a06a8-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="a06a8-116">在驗證期間驗證 middlewares 設定時，會命名為驗證配置。</span><span class="sxs-lookup"><span data-stu-id="a06a8-116">Authentication schemes are named when authentication middlewares are configured during authentication.</span></span> <span data-ttu-id="a06a8-117">例如: </span><span class="sxs-lookup"><span data-stu-id="a06a8-117">For example:</span></span>

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

<span data-ttu-id="a06a8-118">在上述程式碼中，已經加入兩個驗證 middlewares： 一個用於 cookie，另一個用於承載。</span><span class="sxs-lookup"><span data-stu-id="a06a8-118">In the preceding code, two authentication middlewares have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="a06a8-119">指定預設的配置會導致`HttpContext.User`屬性設定為該身分識別。</span><span class="sxs-lookup"><span data-stu-id="a06a8-119">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="a06a8-120">如果不需要該行為，請停用它藉由設定`AuthenticationOptions.AutomaticAuthenticate`屬性`false`。</span><span class="sxs-lookup"><span data-stu-id="a06a8-120">If that behavior isn't desired, disable it by setting the `AuthenticationOptions.AutomaticAuthenticate` property to `false`.</span></span>

---

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="a06a8-121">選取配置和授權屬性</span><span class="sxs-lookup"><span data-stu-id="a06a8-121">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="a06a8-122">在授權應用程式會指出要使用的處理常式。</span><span class="sxs-lookup"><span data-stu-id="a06a8-122">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="a06a8-123">選取的應用程式將授權藉由傳遞驗證配置的逗號分隔清單的處理常式`[Authorize]`。</span><span class="sxs-lookup"><span data-stu-id="a06a8-123">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="a06a8-124">`[Authorize]`屬性指定的配置，來使用，不論預設值設定的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="a06a8-124">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="a06a8-125">例如: </span><span class="sxs-lookup"><span data-stu-id="a06a8-125">For example:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a06a8-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a06a8-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a06a8-127">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a06a8-127">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="a06a8-128">在上述範例中，cookie 和承載的處理常式會執行，並有機會建立並附加目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="a06a8-128">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="a06a8-129">藉由指定單一配置，會執行對應的處理常式。</span><span class="sxs-lookup"><span data-stu-id="a06a8-129">By specifying a single scheme only, the corresponding handler runs.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a06a8-130">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a06a8-130">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a06a8-131">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a06a8-131">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
[Authorize(ActiveAuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

---

<span data-ttu-id="a06a8-132">在上述程式碼，只使用"Bearer"配置處理常式會執行。</span><span class="sxs-lookup"><span data-stu-id="a06a8-132">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="a06a8-133">以 cookie 為基礎的所有識別都會被都忽略。</span><span class="sxs-lookup"><span data-stu-id="a06a8-133">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="a06a8-134">選取原則的配置</span><span class="sxs-lookup"><span data-stu-id="a06a8-134">Selecting the scheme with policies</span></span>

<span data-ttu-id="a06a8-135">如果您想要指定在所需的配置[原則](xref:security/authorization/policies)，您可以設定`AuthenticationSchemes`集合加入您的原則時：</span><span class="sxs-lookup"><span data-stu-id="a06a8-135">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="a06a8-136">在上述範例中，"Over18"原則只會執行比對"Bearer"的處理常式所建立的識別。</span><span class="sxs-lookup"><span data-stu-id="a06a8-136">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="a06a8-137">使用此原則設定`[Authorize]`屬性的`Policy`屬性：</span><span class="sxs-lookup"><span data-stu-id="a06a8-137">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```
