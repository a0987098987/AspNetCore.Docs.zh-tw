---
title: ASP.NET Core 中的自訂授權原則提供者
author: mjrousos
description: 瞭解如何在 ASP.NET Core 應用程式中使用自訂的 IAuthorizationPolicyProvider，以動態方式產生授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 4f6a4ea209ebe30759f9f14b15b0385399b36ead
ms.sourcegitcommit: 231780c8d7848943e5e9fd55e93f437f7e5a371d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/15/2019
ms.locfileid: "74116066"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="6c5f6-103">在 ASP.NET Core 中使用 IAuthorizationPolicyProvider 的自訂授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="6c5f6-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="6c5f6-104">由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="6c5f6-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="6c5f6-105">通常在使用以[原則為基礎的授權](xref:security/authorization/policies)時，會藉由呼叫 `AuthorizationOptions.AddPolicy` 做為授權服務設定的一部分來註冊原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="6c5f6-106">在某些情況下，不可能（或需要）以這種方式註冊所有授權原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="6c5f6-107">在這些情況下，您可以使用自訂 `IAuthorizationPolicyProvider` 來控制授權原則的提供方式。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="6c5f6-108">自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能有用的案例範例包括：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="6c5f6-109">使用外部服務來提供原則評估。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="6c5f6-110">使用較大範圍的原則（例如，針對不同的房間號碼或年齡），因此使用 `AuthorizationOptions.AddPolicy` 呼叫來新增各個授權原則並不合理。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="6c5f6-111">在執行時間根據外部資料源（例如資料庫）中的資訊建立原則，或透過另一個機制動態判斷授權需求。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="6c5f6-112">從[AspNetCore GitHub 存放庫](https://github.com/aspnet/AspNetCore)中[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="6c5f6-113">下載 aspnet/AspNetCore 存放庫 ZIP 檔案。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="6c5f6-114">解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-114">Unzip the file.</span></span> <span data-ttu-id="6c5f6-115">流覽至*src/Security/samples/CustomPolicyProvider*專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="6c5f6-116">自訂原則抓取</span><span class="sxs-lookup"><span data-stu-id="6c5f6-116">Customize policy retrieval</span></span>

<span data-ttu-id="6c5f6-117">ASP.NET Core 應用程式會使用 `IAuthorizationPolicyProvider` 介面的執行來取出授權原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="6c5f6-118">根據預設，會註冊並使用[DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) 。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="6c5f6-119">`DefaultAuthorizationPolicyProvider` 會從 `IServiceCollection.AddAuthorization` 呼叫中提供的 `AuthorizationOptions` 傳回原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="6c5f6-120">您可以在應用程式的相依性[插入](xref:fundamentals/dependency-injection)容器中註冊不同的 `IAuthorizationPolicyProvider` 實作為自訂此行為。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="6c5f6-121">`IAuthorizationPolicyProvider` 介面包含三個 Api：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-121">The `IAuthorizationPolicyProvider` interface contains three APIs:</span></span>

* <span data-ttu-id="6c5f6-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)會傳回指定名稱的授權原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="6c5f6-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)會傳回預設的授權原則（用於 `[Authorize]` 屬性的原則，而不會指定原則）。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 
* <span data-ttu-id="6c5f6-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync)會傳回 fallback 授權原則（未指定任何原則時，授權中介軟體所使用的原則）。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) returns the fallback authorization policy (the policy used by the Authorization Middleware when no policy is specified).</span></span> 

<span data-ttu-id="6c5f6-125">藉由執行這些 Api，您可以自訂如何提供授權原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-125">By implementing these APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="6c5f6-126">參數化授權屬性範例</span><span class="sxs-lookup"><span data-stu-id="6c5f6-126">Parameterized authorize attribute example</span></span>

<span data-ttu-id="6c5f6-127">`IAuthorizationPolicyProvider` 很有用的其中一個案例，就是啟用其需求相依于參數的自訂 `[Authorize]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-127">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="6c5f6-128">例如，在以[原則為基礎的授權](xref:security/authorization/policies)檔中，以存留期為基礎的（"AtLeast21"）原則是用來做為範例。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-128">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="6c5f6-129">如果應用程式中的不同控制器動作應該提供給*不同*年齡的使用者使用，則有許多不同的以年齡為基礎的原則可能會很有用。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-129">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="6c5f6-130">您可以使用自訂 `IAuthorizationPolicyProvider`，以動態方式產生原則，而不是在 `AuthorizationOptions`中註冊應用程式所需的所有不同存留期原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-130">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="6c5f6-131">若要更輕鬆地使用原則，您可以使用如 `[MinimumAgeAuthorize(20)]`的自訂授權屬性來標注動作。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-131">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="6c5f6-132">自訂授權屬性</span><span class="sxs-lookup"><span data-stu-id="6c5f6-132">Custom Authorization attributes</span></span>

<span data-ttu-id="6c5f6-133">授權原則是以其名稱來識別。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-133">Authorization policies are identified by their names.</span></span> <span data-ttu-id="6c5f6-134">先前所述的自訂 `MinimumAgeAuthorizeAttribute` 必須將引數對應到可以用來抓取對應授權原則的字串。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-134">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="6c5f6-135">若要這麼做，您可以從 `AuthorizeAttribute` 衍生，並讓 `Age` 屬性包裝 `AuthorizeAttribute.Policy` 屬性。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-135">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="6c5f6-136">此屬性類型具有根據硬式編碼前置詞（`"MinimumAge"`）的 `Policy` 字串，以及透過函式傳入的整數。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-136">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="6c5f6-137">您可以使用與其他 `Authorize` 屬性相同的方式將它套用至動作，不同的是它會使用整數做為參數。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-137">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="6c5f6-138">自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="6c5f6-138">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="6c5f6-139">自訂 `MinimumAgeAuthorizeAttribute` 可讓您輕鬆地要求授權原則，以取得所需的任何最短存留期。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-139">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="6c5f6-140">下一個要解決的問題是確保授權原則適用于所有這些不同的年齡。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-140">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="6c5f6-141">這就是 `IAuthorizationPolicyProvider` 很有用的地方。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-141">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="6c5f6-142">使用 `MinimumAgeAuthorizationAttribute`時，授權原則名稱會遵循模式 `"MinimumAge" + Age`，因此自訂 `IAuthorizationPolicyProvider` 應該會藉由下列方式產生授權原則：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-142">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="6c5f6-143">正在剖析原則名稱中的年齡。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-143">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="6c5f6-144">使用 `AuthorizationPolicyBuilder` 建立新的 `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="6c5f6-144">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="6c5f6-145">在此和下列範例中，會假設使用者是透過 cookie 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-145">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="6c5f6-146">應該使用至少一個授權配置名稱來建立 `AuthorizationPolicyBuilder`，或一律成功。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-146">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="6c5f6-147">否則，沒有關于如何為使用者提供挑戰的資訊，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-147">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="6c5f6-148">根據 `AuthorizationPolicyBuilder.AddRequirements`的存在時間，將需求新增至原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-148">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="6c5f6-149">在其他案例中，您可以改用 `RequireClaim`、`RequireRole`或 `RequireUserName`。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-149">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="6c5f6-150">多個授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="6c5f6-150">Multiple authorization policy providers</span></span>

<span data-ttu-id="6c5f6-151">使用自訂 `IAuthorizationPolicyProvider` 的實作為時，請記住，ASP.NET Core 只會使用一個 `IAuthorizationPolicyProvider`實例。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-151">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="6c5f6-152">如果自訂提供者無法針對將使用的所有原則名稱提供授權原則，它應該會延遲到備份提供者。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-152">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should defer to a backup provider.</span></span> 

<span data-ttu-id="6c5f6-153">例如，假設有一個應用程式同時需要自訂年齡原則和更傳統的以角色為基礎的原則抓取。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-153">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="6c5f6-154">這類應用程式可以使用自訂授權原則提供者：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-154">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="6c5f6-155">嘗試剖析原則名稱。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-155">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="6c5f6-156">如果原則名稱不包含年齡，會呼叫不同的原則提供者（例如 `DefaultAuthorizationPolicyProvider`）。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-156">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="6c5f6-157">如上所示的範例 `IAuthorizationPolicyProvider` 執行，可以藉由在其程式化中建立備份原則提供者（在原則名稱不符合預期的 ' MinimumAge ' + age 模式時使用），更新為使用 `DefaultAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-157">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a backup policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="6c5f6-158">然後，`GetPolicyAsync` 方法可以更新為使用 `BackupPolicyProvider`，而不是傳回 null：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-158">Then, the `GetPolicyAsync` method can be updated to use the `BackupPolicyProvider` instead of returning null:</span></span>

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="6c5f6-159">預設原則</span><span class="sxs-lookup"><span data-stu-id="6c5f6-159">Default policy</span></span>

<span data-ttu-id="6c5f6-160">除了提供命名的授權原則之外，自訂 `IAuthorizationPolicyProvider` 還需要執行 `GetDefaultPolicyAsync`，以提供 `[Authorize]` 屬性的授權原則，而不需指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-160">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="6c5f6-161">在許多情況下，此授權屬性只需要已驗證的使用者，因此您可以使用 `RequireAuthenticatedUser`的呼叫來建立必要的原則：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-161">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="6c5f6-162">如同自訂 `IAuthorizationPolicyProvider`的所有層面，您可以視需要自訂此項。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-162">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="6c5f6-163">在某些情況下，您可能會想要從回溯 `IAuthorizationPolicyProvider`取出預設原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-163">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="fallback-policy"></a><span data-ttu-id="6c5f6-164">Fallback 原則</span><span class="sxs-lookup"><span data-stu-id="6c5f6-164">Fallback policy</span></span>

<span data-ttu-id="6c5f6-165">自訂 `IAuthorizationPolicyProvider` 可以選擇性地執行 `GetFallbackPolicyAsync`，以提供[結合原則](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine)時和未指定任何原則時所使用的原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-165">A custom `IAuthorizationPolicyProvider` can optionally implement `GetFallbackPolicyAsync` to provide a policy that's used when [combining policies](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) and when no policies are specified.</span></span> <span data-ttu-id="6c5f6-166">如果 `GetFallbackPolicyAsync` 傳回非 null 的原則，則當要求未指定任何原則時，授權中介軟體會使用傳回的原則。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-166">If `GetFallbackPolicyAsync` returns a non-null policy, the returned policy is used by the Authorization Middleware when no policies are specified for the request.</span></span>

<span data-ttu-id="6c5f6-167">如果沒有必要的回溯原則，提供者可以將 `null` 或延遲傳回給回溯提供者：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-167">If no fallback policy is required, the provider can return `null` or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="6c5f6-168">使用自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="6c5f6-168">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="6c5f6-169">若要從 `IAuthorizationPolicyProvider`使用自訂原則，您必須：</span><span class="sxs-lookup"><span data-stu-id="6c5f6-169">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="6c5f6-170">使用相依性插入來註冊適當的 `AuthorizationHandler` 類型（如以[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers)中所述），如同所有以原則為基礎的授權案例。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-170">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="6c5f6-171">在應用程式的相依性插入服務集合（在 `Startup.ConfigureServices`中）中註冊自訂 `IAuthorizationPolicyProvider` 類型，以取代預設原則提供者。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-171">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="6c5f6-172">您可以在[aspnet/AuthSamples GitHub 存放庫](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)中取得完整的自訂 `IAuthorizationPolicyProvider` 範例。</span><span class="sxs-lookup"><span data-stu-id="6c5f6-172">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
