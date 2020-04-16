---
title: ASP.NET核心中的自訂授權策略提供者
author: mjrousos
description: 瞭解如何在ASP.NET核心應用中使用自定義I授權策略提供程式動態生成授權策略。
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 2a8b189cc9f17529a962a1f9642c7bb199d5781b
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440918"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="b40c8-103">自訂授權政策提供者使用 i 授權政策提供者ASP.NET核心</span><span class="sxs-lookup"><span data-stu-id="b40c8-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="b40c8-104">由[邁克·盧梭斯](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="b40c8-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="b40c8-105">通常,在使用[基於策略的授權](xref:security/authorization/policies)時,策略是通過`AuthorizationOptions.AddPolicy`調用 作為授權服務配置的一部分進行註冊的。</span><span class="sxs-lookup"><span data-stu-id="b40c8-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="b40c8-106">在某些情況下,不可能(或不需要)以這種方式註冊所有授權策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="b40c8-107">在這些情況下,可以使用自定義來控制`IAuthorizationPolicyProvider`如何提供授權策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="b40c8-108">自訂[I 授權政策提供者](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能有用的方案範例包括:</span><span class="sxs-lookup"><span data-stu-id="b40c8-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="b40c8-109">使用外部服務提供策略評估。</span><span class="sxs-lookup"><span data-stu-id="b40c8-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="b40c8-110">使用大量策略(例如,針對不同的房間號碼或年齡),因此使用`AuthorizationOptions.AddPolicy`調用添加每個單獨的授權策略沒有意義。</span><span class="sxs-lookup"><span data-stu-id="b40c8-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn't make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="b40c8-111">根據外部資料來源(如資料庫)中的資訊在執行時創建策略,或透過另一種機制動態確定授權要求。</span><span class="sxs-lookup"><span data-stu-id="b40c8-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="b40c8-112">從[AspNetCore GitHub 儲存函式庫](https://github.com/dotnet/AspNetCore)[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)。</span><span class="sxs-lookup"><span data-stu-id="b40c8-112">[View or download sample code](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="b40c8-113">下載點網/阿斯普內科存儲庫 ZIP 檔。</span><span class="sxs-lookup"><span data-stu-id="b40c8-113">Download the dotnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="b40c8-114">解壓縮檔。</span><span class="sxs-lookup"><span data-stu-id="b40c8-114">Unzip the file.</span></span> <span data-ttu-id="b40c8-115">導航到*src/安全/範例/自定義策略提供程式*專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="b40c8-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="b40c8-116">自訂策略檢索</span><span class="sxs-lookup"><span data-stu-id="b40c8-116">Customize policy retrieval</span></span>

<span data-ttu-id="b40c8-117">ASP.NET核心應用使用介面的`IAuthorizationPolicyProvider`實現來檢索授權策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="b40c8-118">預設情況下,[預設授權策略提供程式](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)已註冊和使用。</span><span class="sxs-lookup"><span data-stu-id="b40c8-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="b40c8-119">`DefaultAuthorizationPolicyProvider`從`IServiceCollection.AddAuthorization`呼叫中提供`AuthorizationOptions`的策略返回策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="b40c8-120">通過在應用[的依賴項注入](xref:fundamentals/dependency-injection)容器`IAuthorizationPolicyProvider`中註冊 不同的實現來自定義此行為。</span><span class="sxs-lookup"><span data-stu-id="b40c8-120">Customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="b40c8-121">該`IAuthorizationPolicyProvider`介面包含三個 API:</span><span class="sxs-lookup"><span data-stu-id="b40c8-121">The `IAuthorizationPolicyProvider` interface contains three APIs:</span></span>

* <span data-ttu-id="b40c8-122">[獲取策略同步](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)返回給定名稱的授權策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="b40c8-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)傳回預設授權原則`[Authorize]`(用於未指定策略的屬性的策略)。</span><span class="sxs-lookup"><span data-stu-id="b40c8-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 
* <span data-ttu-id="b40c8-124">[GetBackbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync)傳回回退授權策略(未指定策略時授權中間件使用的策略)。</span><span class="sxs-lookup"><span data-stu-id="b40c8-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) returns the fallback authorization policy (the policy used by the Authorization Middleware when no policy is specified).</span></span> 

<span data-ttu-id="b40c8-125">通過實現這些 API,您可以自定義如何提供授權策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-125">By implementing these APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="b40c8-126">參數化授權屬性範例</span><span class="sxs-lookup"><span data-stu-id="b40c8-126">Parameterized authorize attribute example</span></span>

<span data-ttu-id="b40c8-127">一`IAuthorizationPolicyProvider`個有用的方案是啟用其要求`[Authorize]`依賴於參數的自定義屬性。</span><span class="sxs-lookup"><span data-stu-id="b40c8-127">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="b40c8-128">例如,[在基於策略的授權](xref:security/authorization/policies)文檔中,使用基於年齡("AtLeast21")的策略作為示例。</span><span class="sxs-lookup"><span data-stu-id="b40c8-128">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="b40c8-129">如果應用中的不同控制器操作應提供給*不同*年齡的使用者,則使用許多不同的基於年齡的策略可能很有用。</span><span class="sxs-lookup"><span data-stu-id="b40c8-129">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="b40c8-130">無需註冊應用程式將需要的所有`AuthorizationOptions`不同基於年齡的策略,而是可以使用自`IAuthorizationPolicyProvider`定義 動態生成策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-130">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="b40c8-131">為了簡化使用策略,可以使用自定義授權屬性(如`[MinimumAgeAuthorize(20)]`)對操作進行分給。</span><span class="sxs-lookup"><span data-stu-id="b40c8-131">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="b40c8-132">自訂授權屬性</span><span class="sxs-lookup"><span data-stu-id="b40c8-132">Custom Authorization attributes</span></span>

<span data-ttu-id="b40c8-133">授權策略由其名稱識別。</span><span class="sxs-lookup"><span data-stu-id="b40c8-133">Authorization policies are identified by their names.</span></span> <span data-ttu-id="b40c8-134">前面描述的`MinimumAgeAuthorizeAttribute`自定義需要將參數映射到可用於檢索相應授權策略的字串中。</span><span class="sxs-lookup"><span data-stu-id="b40c8-134">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="b40c8-135">可以通過派生`AuthorizeAttribute`和`Age`使 屬性`AuthorizeAttribute.Policy`換行屬性 來執行此操作。</span><span class="sxs-lookup"><span data-stu-id="b40c8-135">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="b40c8-136">此屬性類型具有一個`Policy`基於硬編碼首碼`"MinimumAge"`( ) 的字串,以及透過建構函數傳入的整數。</span><span class="sxs-lookup"><span data-stu-id="b40c8-136">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="b40c8-137">可以將其應用於操作的方式與其他`Authorize`屬性相同,只不過它採用整數作為參數。</span><span class="sxs-lookup"><span data-stu-id="b40c8-137">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="b40c8-138">自訂授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="b40c8-138">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="b40c8-139">該自定義`MinimumAgeAuthorizeAttribute`使請求任何所需的最低年齡的授權策略變得容易。</span><span class="sxs-lookup"><span data-stu-id="b40c8-139">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="b40c8-140">要解決的下一個問題是確保授權策略可用於所有這些不同年齡。</span><span class="sxs-lookup"><span data-stu-id="b40c8-140">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="b40c8-141">這是有用的`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="b40c8-141">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="b40c8-142">使用`MinimumAgeAuthorizationAttribute`時,授權策略名稱將遵循`"MinimumAge" + Age`模式 ,因此`IAuthorizationPolicyProvider`自定義 應通過以下方式生成授權策略:</span><span class="sxs-lookup"><span data-stu-id="b40c8-142">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="b40c8-143">從策略名稱分析年齡。</span><span class="sxs-lookup"><span data-stu-id="b40c8-143">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="b40c8-144">建立`AuthorizationPolicyBuilder`新`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="b40c8-144">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="b40c8-145">在此和下面的示例中,假定用戶通過 Cookie 進行身份驗證。</span><span class="sxs-lookup"><span data-stu-id="b40c8-145">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="b40c8-146">`AuthorizationPolicyBuilder`應至少使用一個授權方案名稱構造 ,或者始終成功。</span><span class="sxs-lookup"><span data-stu-id="b40c8-146">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="b40c8-147">否則,沒有關於如何向使用者提供質詢的資訊,並且將引發異常。</span><span class="sxs-lookup"><span data-stu-id="b40c8-147">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="b40c8-148">根據具有`AuthorizationPolicyBuilder.AddRequirements`的年齡向策略添加要求。</span><span class="sxs-lookup"><span data-stu-id="b40c8-148">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="b40c8-149">在其他機制中,您可以使用,`RequireClaim``RequireUserName`或是改`RequireRole`為 使用 。</span><span class="sxs-lookup"><span data-stu-id="b40c8-149">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="b40c8-150">多個授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="b40c8-150">Multiple authorization policy providers</span></span>

<span data-ttu-id="b40c8-151">使用自定義`IAuthorizationPolicyProvider`實現時,請記住,ASP.NET Core`IAuthorizationPolicyProvider`僅使用 的一個實例。</span><span class="sxs-lookup"><span data-stu-id="b40c8-151">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="b40c8-152">如果自定義提供程式無法為將使用的所有策略名稱提供授權策略,則應將其服從備份提供程式。</span><span class="sxs-lookup"><span data-stu-id="b40c8-152">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should defer to a backup provider.</span></span> 

<span data-ttu-id="b40c8-153">例如,考慮一個既需要自定義年齡策略又需要更傳統的基於角色的策略檢索的應用程式。</span><span class="sxs-lookup"><span data-stu-id="b40c8-153">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="b40c8-154">此類應用可以使用自訂授權政策提供者:</span><span class="sxs-lookup"><span data-stu-id="b40c8-154">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="b40c8-155">嘗試分析策略名稱。</span><span class="sxs-lookup"><span data-stu-id="b40c8-155">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="b40c8-156">如果策略名稱不包含年齡,則調用`DefaultAuthorizationPolicyProvider`其他策略提供程式(如 )。</span><span class="sxs-lookup"><span data-stu-id="b40c8-156">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="b40c8-157">可以通過`IAuthorizationPolicyProvider`在其建構函數中創建備份策略提供程式來`DefaultAuthorizationPolicyProvider`更新 上述範例實現以使用 (在策略名稱與其預期模式"最小年齡"+ 年齡不匹配時使用)。</span><span class="sxs-lookup"><span data-stu-id="b40c8-157">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a backup policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="b40c8-158">然後,`GetPolicyAsync`可以更新該方法以`BackupPolicyProvider`使用 而不是傳回 null:</span><span class="sxs-lookup"><span data-stu-id="b40c8-158">Then, the `GetPolicyAsync` method can be updated to use the `BackupPolicyProvider` instead of returning null:</span></span>

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="b40c8-159">預設原則</span><span class="sxs-lookup"><span data-stu-id="b40c8-159">Default policy</span></span>

<span data-ttu-id="b40c8-160">除了提供命名授權策略外,還需要`IAuthorizationPolicyProvider`實現`GetDefaultPolicyAsync`為未指定策略`[Authorize]`名稱的屬性提供授權策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-160">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="b40c8-161">在許多情況下,此授權屬性只需要經過身份驗證的使用者,因此可以使用呼叫`RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="b40c8-161">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="b40c8-162">與自定義`IAuthorizationPolicyProvider`的所有方面一樣,您可以根據需要自定義此內容。</span><span class="sxs-lookup"><span data-stu-id="b40c8-162">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="b40c8-163">在某些情況下,可能需要從回退`IAuthorizationPolicyProvider`檢索預設策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-163">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="fallback-policy"></a><span data-ttu-id="b40c8-164">遞迴退原則</span><span class="sxs-lookup"><span data-stu-id="b40c8-164">Fallback policy</span></span>

<span data-ttu-id="b40c8-165">自定義`IAuthorizationPolicyProvider`可以選擇`GetFallbackPolicyAsync`實現 以提供在[組合策略](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine)時和未指定策略時使用的策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-165">A custom `IAuthorizationPolicyProvider` can optionally implement `GetFallbackPolicyAsync` to provide a policy that's used when [combining policies](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) and when no policies are specified.</span></span> <span data-ttu-id="b40c8-166">如果`GetFallbackPolicyAsync`返回非空策略,則當未為請求指定策略時,授權中間件將使用返回的策略。</span><span class="sxs-lookup"><span data-stu-id="b40c8-166">If `GetFallbackPolicyAsync` returns a non-null policy, the returned policy is used by the Authorization Middleware when no policies are specified for the request.</span></span>

<span data-ttu-id="b40c8-167">如果不需要回退策略,提供程式可以返回`null`或延遲回退提供者:</span><span class="sxs-lookup"><span data-stu-id="b40c8-167">If no fallback policy is required, the provider can return `null` or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="b40c8-168">使用自訂 I 授權政策提供者</span><span class="sxs-lookup"><span data-stu-id="b40c8-168">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="b40c8-169">要使用的自訂策略`IAuthorizationPolicyProvider`,必須:</span><span class="sxs-lookup"><span data-stu-id="b40c8-169">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="b40c8-170">使用依賴項`AuthorizationHandler`注入註冊適當的類型(在[基於策略的授權](xref:security/authorization/policies#authorization-handlers)中描述),與所有基於策略的授權方案一樣。</span><span class="sxs-lookup"><span data-stu-id="b40c8-170">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="b40c8-171">在應用的依賴`IAuthorizationPolicyProvider`項注入服務集合(在中)`Startup.ConfigureServices`中註冊自定義類型以替換預設策略提供程式。</span><span class="sxs-lookup"><span data-stu-id="b40c8-171">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="b40c8-172">完整的自定義`IAuthorizationPolicyProvider`示例在[dotnet/aspnetcore GitHub 儲存庫](https://github.com/dotnet/aspnetcore/tree/ea555458dc61e04314598c25b3ab8c56362a5123/src/Security/samples/CustomPolicyProvider)中可用。</span><span class="sxs-lookup"><span data-stu-id="b40c8-172">A complete custom `IAuthorizationPolicyProvider` sample is available in the [dotnet/aspnetcore GitHub repository](https://github.com/dotnet/aspnetcore/tree/ea555458dc61e04314598c25b3ab8c56362a5123/src/Security/samples/CustomPolicyProvider).</span></span>
