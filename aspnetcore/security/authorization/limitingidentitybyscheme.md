---
title: 在 ASP.NET Core 中使用特定配置進行授權
author: rick-anderson
description: 本文說明如何在使用多個驗證方法時，將身分識別限制為特定的配置。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 11/08/2019
uid: security/authorization/limitingidentitybyscheme
ms.openlocfilehash: 9c173a4589279b03bc12b4b7dea594fae88cf471
ms.sourcegitcommit: 0b0e485a8a6dfcc65a7a58b365622b3839f4d624
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/01/2020
ms.locfileid: "76928391"
---
# <a name="authorize-with-a-specific-scheme-in-aspnet-core"></a><span data-ttu-id="f4381-103">在 ASP.NET Core 中使用特定配置進行授權</span><span class="sxs-lookup"><span data-stu-id="f4381-103">Authorize with a specific scheme in ASP.NET Core</span></span>

<span data-ttu-id="f4381-104">在某些情況下，例如單頁應用程式（Spa），通常會使用多個驗證方法。</span><span class="sxs-lookup"><span data-stu-id="f4381-104">In some scenarios, such as Single Page Applications (SPAs), it's common to use multiple authentication methods.</span></span> <span data-ttu-id="f4381-105">例如，應用程式可能會使用以 cookie 為基礎的驗證來登入，並針對 JavaScript 要求進行 JWT 持有人驗證。</span><span class="sxs-lookup"><span data-stu-id="f4381-105">For example, the app may use cookie-based authentication to log in and JWT bearer authentication for JavaScript requests.</span></span> <span data-ttu-id="f4381-106">在某些情況下，應用程式可能會有多個驗證處理常式實例。</span><span class="sxs-lookup"><span data-stu-id="f4381-106">In some cases, the app may have multiple instances of an authentication handler.</span></span> <span data-ttu-id="f4381-107">例如，兩個 cookie 處理常式，其中一個包含基本身分識別，而另一個則是在觸發多重要素驗證（MFA）時建立。</span><span class="sxs-lookup"><span data-stu-id="f4381-107">For example, two cookie handlers where one contains a basic identity and one is created when a multi-factor authentication (MFA) has been triggered.</span></span> <span data-ttu-id="f4381-108">因為使用者要求的作業需要額外的安全性，所以可能會觸發 MFA。</span><span class="sxs-lookup"><span data-stu-id="f4381-108">MFA may be triggered because the user requested an operation that requires extra security.</span></span> <span data-ttu-id="f4381-109">如需在使用者要求需要 MFA 的資源時強制執行 MFA 的詳細資訊，請參閱 GitHub 問題[保護區段與 mfa](https://github.com/aspnet/AspNetCore.Docs/issues/15791#issuecomment-580464195)。</span><span class="sxs-lookup"><span data-stu-id="f4381-109">For more information on enforcing MFA when a user requests a resource that requires MFA, see the GitHub issue [Protect section with MFA](https://github.com/aspnet/AspNetCore.Docs/issues/15791#issuecomment-580464195).</span></span>

<span data-ttu-id="f4381-110">驗證架構會在驗證期間設定驗證服務時命名。</span><span class="sxs-lookup"><span data-stu-id="f4381-110">An authentication scheme is named when the authentication service is configured during authentication.</span></span> <span data-ttu-id="f4381-111">例如：</span><span class="sxs-lookup"><span data-stu-id="f4381-111">For example:</span></span>

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

<span data-ttu-id="f4381-112">在上述程式碼中，已新增兩個驗證處理常式：一個用於 cookie，一個用於持有人。</span><span class="sxs-lookup"><span data-stu-id="f4381-112">In the preceding code, two authentication handlers have been added: one for cookies and one for bearer.</span></span>

>[!NOTE]
><span data-ttu-id="f4381-113">指定預設配置會導致 `HttpContext.User` 屬性設定為該身分識別。</span><span class="sxs-lookup"><span data-stu-id="f4381-113">Specifying the default scheme results in the `HttpContext.User` property being set to that identity.</span></span> <span data-ttu-id="f4381-114">如果不想要該行為，請叫用 `AddAuthentication`的無參數形式來停用它。</span><span class="sxs-lookup"><span data-stu-id="f4381-114">If that behavior isn't desired, disable it by invoking the parameterless form of `AddAuthentication`.</span></span>

## <a name="selecting-the-scheme-with-the-authorize-attribute"></a><span data-ttu-id="f4381-115">選取具有 [授權] 屬性的配置</span><span class="sxs-lookup"><span data-stu-id="f4381-115">Selecting the scheme with the Authorize attribute</span></span>

<span data-ttu-id="f4381-116">在授權的時候，應用程式會指出要使用的處理常式。</span><span class="sxs-lookup"><span data-stu-id="f4381-116">At the point of authorization, the app indicates the handler to be used.</span></span> <span data-ttu-id="f4381-117">藉由將以逗號分隔的驗證架構清單傳遞給 `[Authorize]`，選取應用程式將授權的處理常式。</span><span class="sxs-lookup"><span data-stu-id="f4381-117">Select the handler with which the app will authorize by passing a comma-delimited list of authentication schemes to `[Authorize]`.</span></span> <span data-ttu-id="f4381-118">不論是否已設定預設值，`[Authorize]` 屬性都會指定要使用的驗證配置或配置。</span><span class="sxs-lookup"><span data-stu-id="f4381-118">The `[Authorize]` attribute specifies the authentication scheme or schemes to use regardless of whether a default is configured.</span></span> <span data-ttu-id="f4381-119">例如：</span><span class="sxs-lookup"><span data-stu-id="f4381-119">For example:</span></span>

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

<span data-ttu-id="f4381-120">在上述範例中，cookie 和持有人處理常式都會執行，而且有機會建立和附加目前使用者的身分識別。</span><span class="sxs-lookup"><span data-stu-id="f4381-120">In the preceding example, both the cookie and bearer handlers run and have a chance to create and append an identity for the current user.</span></span> <span data-ttu-id="f4381-121">藉由僅指定單一配置，會執行對應的處理常式。</span><span class="sxs-lookup"><span data-stu-id="f4381-121">By specifying a single scheme only, the corresponding handler runs.</span></span>

```csharp
[Authorize(AuthenticationSchemes = 
    JwtBearerDefaults.AuthenticationScheme)]
public class MixedController : Controller
```

<span data-ttu-id="f4381-122">在上述程式碼中，只有具有「持有人」配置的處理常式才會執行。</span><span class="sxs-lookup"><span data-stu-id="f4381-122">In the preceding code, only the handler with the "Bearer" scheme runs.</span></span> <span data-ttu-id="f4381-123">系統會忽略任何以 cookie 為基礎的身分識別。</span><span class="sxs-lookup"><span data-stu-id="f4381-123">Any cookie-based identities are ignored.</span></span>

## <a name="selecting-the-scheme-with-policies"></a><span data-ttu-id="f4381-124">選取具有原則的配置</span><span class="sxs-lookup"><span data-stu-id="f4381-124">Selecting the scheme with policies</span></span>

<span data-ttu-id="f4381-125">如果您想要在[原則](xref:security/authorization/policies)中指定所需的配置，可以在新增原則時設定 `AuthenticationSchemes` 集合：</span><span class="sxs-lookup"><span data-stu-id="f4381-125">If you prefer to specify the desired schemes in [policy](xref:security/authorization/policies), you can set the `AuthenticationSchemes` collection when adding your policy:</span></span>

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

<span data-ttu-id="f4381-126">在上述範例中，"Over18" 原則只會針對「持有人」處理常式所建立的身分識別來執行。</span><span class="sxs-lookup"><span data-stu-id="f4381-126">In the preceding example, the "Over18" policy only runs against the identity created by the "Bearer" handler.</span></span> <span data-ttu-id="f4381-127">藉由設定 `[Authorize]` 屬性的 `Policy` 屬性來使用原則：</span><span class="sxs-lookup"><span data-stu-id="f4381-127">Use the policy by setting the `[Authorize]` attribute's `Policy` property:</span></span>

```csharp
[Authorize(Policy = "Over18")]
public class RegistrationController : Controller
```

::: moniker range=">= aspnetcore-2.0"

## <a name="use-multiple-authentication-schemes"></a><span data-ttu-id="f4381-128">使用多個驗證配置</span><span class="sxs-lookup"><span data-stu-id="f4381-128">Use multiple authentication schemes</span></span>

<span data-ttu-id="f4381-129">某些應用程式可能需要支援多種類型的驗證。</span><span class="sxs-lookup"><span data-stu-id="f4381-129">Some apps may need to support multiple types of authentication.</span></span> <span data-ttu-id="f4381-130">例如，您的應用程式可能會從 Azure Active Directory 和使用者資料庫驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="f4381-130">For example, your app might authenticate users from Azure Active Directory and from a users database.</span></span> <span data-ttu-id="f4381-131">另一個範例是從 Active Directory 同盟服務和 Azure Active Directory B2C 驗證使用者的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f4381-131">Another example is an app that authenticates users from both Active Directory Federation Services and Azure Active Directory B2C.</span></span> <span data-ttu-id="f4381-132">在此情況下，應用程式應該接受來自數個簽發者的 JWT 持有人權杖。</span><span class="sxs-lookup"><span data-stu-id="f4381-132">In this case, the app should accept a JWT bearer token from several issuers.</span></span>

<span data-ttu-id="f4381-133">新增您想要接受的所有驗證配置。</span><span class="sxs-lookup"><span data-stu-id="f4381-133">Add all authentication schemes you'd like to accept.</span></span> <span data-ttu-id="f4381-134">例如，`Startup.ConfigureServices` 中的下列程式碼會新增具有不同簽發者的兩個 JWT 持有人驗證配置：</span><span class="sxs-lookup"><span data-stu-id="f4381-134">For example, the following code in `Startup.ConfigureServices` adds two JWT bearer authentication schemes with different issuers:</span></span>

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
> <span data-ttu-id="f4381-135">只有一個 JWT 持有人驗證會向 `JwtBearerDefaults.AuthenticationScheme`的預設驗證配置進行註冊。</span><span class="sxs-lookup"><span data-stu-id="f4381-135">Only one JWT bearer authentication is registered with the default authentication scheme `JwtBearerDefaults.AuthenticationScheme`.</span></span> <span data-ttu-id="f4381-136">額外的驗證必須使用唯一的驗證配置進行註冊。</span><span class="sxs-lookup"><span data-stu-id="f4381-136">Additional authentication has to be registered with a unique authentication scheme.</span></span>

<span data-ttu-id="f4381-137">下一個步驟是更新預設授權原則，以接受這兩種驗證配置。</span><span class="sxs-lookup"><span data-stu-id="f4381-137">The next step is to update the default authorization policy to accept both authentication schemes.</span></span> <span data-ttu-id="f4381-138">例如：</span><span class="sxs-lookup"><span data-stu-id="f4381-138">For example:</span></span>

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

<span data-ttu-id="f4381-139">當預設的授權原則遭到覆寫時，可以使用控制器中的 `[Authorize]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="f4381-139">As the default authorization policy is overridden, it's possible to use the `[Authorize]` attribute in controllers.</span></span> <span data-ttu-id="f4381-140">然後，控制器會接受第一個或第二個簽發者所發出的 JWT 要求。</span><span class="sxs-lookup"><span data-stu-id="f4381-140">The controller then accepts requests with JWT issued by the first or second issuer.</span></span>

::: moniker-end
