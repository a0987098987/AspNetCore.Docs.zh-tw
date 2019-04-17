---
title: ASP.NET Core 中的自訂授權原則提供者
author: mjrousos
description: 了解如何在 ASP.NET Core 應用程式中使用自訂 IAuthorizationPolicyProvider，來動態產生的授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e17372bb0ec9091c385a70b1e907eaa3cff24003
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614405"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="772dc-103">在 ASP.NET Core 中使用 IAuthorizationPolicyProvider 的自訂授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="772dc-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="772dc-104">藉由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="772dc-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="772dc-105">通常使用時[原則為基礎的授權](xref:security/authorization/policies)，藉由呼叫註冊原則`AuthorizationOptions.AddPolicy`授權服務組態的一部分。</span><span class="sxs-lookup"><span data-stu-id="772dc-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="772dc-106">在某些情況下，它可能不是辦法 （或不想） 來註冊所有授權原則以這種方式。</span><span class="sxs-lookup"><span data-stu-id="772dc-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="772dc-107">在這些情況下，您可以使用自訂`IAuthorizationPolicyProvider`來控制如何提供授權原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="772dc-108">案例範例，其中自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能會很有用包括：</span><span class="sxs-lookup"><span data-stu-id="772dc-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="772dc-109">您可以使用外部服務，提供原則評估。</span><span class="sxs-lookup"><span data-stu-id="772dc-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="772dc-110">使用大範圍的原則 （適用於不同的空間數字或年齡，例如），因此沒有任何意義加入具有每個個別的授權原則`AuthorizationOptions.AddPolicy`呼叫。</span><span class="sxs-lookup"><span data-stu-id="772dc-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="772dc-111">在執行階段根據外部資料來源 （例如資料庫） 中的資訊建立原則，或透過其他機制以動態方式判斷授權需求。</span><span class="sxs-lookup"><span data-stu-id="772dc-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="772dc-112">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)從[AspNetCore GitHub 存放庫](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="772dc-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="772dc-113">下載 aspnet/AspNetCore 存放庫的 ZIP 檔案。</span><span class="sxs-lookup"><span data-stu-id="772dc-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="772dc-114">將檔案解壓縮。</span><span class="sxs-lookup"><span data-stu-id="772dc-114">Unzip the file.</span></span> <span data-ttu-id="772dc-115">瀏覽至*src/Security/範例/CustomPolicyProvider*專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="772dc-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="772dc-116">自訂原則抓取</span><span class="sxs-lookup"><span data-stu-id="772dc-116">Customize policy retrieval</span></span>

<span data-ttu-id="772dc-117">ASP.NET Core 應用程式使用的實作`IAuthorizationPolicyProvider`介面擷取授權原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="772dc-118">根據預設， [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)在註冊及使用。</span><span class="sxs-lookup"><span data-stu-id="772dc-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="772dc-119">`DefaultAuthorizationPolicyProvider` 傳回從原則`AuthorizationOptions`中提供`IServiceCollection.AddAuthorization`呼叫。</span><span class="sxs-lookup"><span data-stu-id="772dc-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="772dc-120">您可以自訂此行為由註冊不同`IAuthorizationPolicyProvider`的應用程式中實作[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="772dc-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="772dc-121">`IAuthorizationPolicyProvider`介面包含兩個 Api:</span><span class="sxs-lookup"><span data-stu-id="772dc-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="772dc-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)傳回指定之名稱的授權原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="772dc-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)傳回預設的授權原則 (所用的原則`[Authorize]`屬性時一定要指定的原則)。</span><span class="sxs-lookup"><span data-stu-id="772dc-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="772dc-124">藉由實作這兩個 Api，您可以自訂授權原則提供的方式。</span><span class="sxs-lookup"><span data-stu-id="772dc-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="772dc-125">參數化授權屬性範例</span><span class="sxs-lookup"><span data-stu-id="772dc-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="772dc-126">其中一個案例所在`IAuthorizationPolicyProvider`很有用啟用自訂`[Authorize]`其需求取決於參數的屬性。</span><span class="sxs-lookup"><span data-stu-id="772dc-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="772dc-127">例如，在[原則為基礎的授權](xref:security/authorization/policies)文件，根據年齡 (「 AtLeast21") 作為範例使用了原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="772dc-128">如果應用程式中的不同控制器動作應該會提供給使用者*不同*年齡，可能有助於將許多不同年齡為基礎的原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="772dc-129">而不是註冊所有不同年齡為基礎的原則，應用程式必須在`AuthorizationOptions`，您可以產生動態地自訂原則`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="772dc-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="772dc-130">若要讓使用原則更容易，您可以加上註解動作等的自訂授權屬性`[MinimumAgeAuthorize(20)]`。</span><span class="sxs-lookup"><span data-stu-id="772dc-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="772dc-131">自訂授權屬性</span><span class="sxs-lookup"><span data-stu-id="772dc-131">Custom Authorization attributes</span></span>

<span data-ttu-id="772dc-132">授權原則會以其名稱識別。</span><span class="sxs-lookup"><span data-stu-id="772dc-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="772dc-133">自訂`MinimumAgeAuthorizeAttribute`描述之前必須將引數對應至字串，可用來擷取對應的授權原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="772dc-134">您可以藉由衍生自`AuthorizeAttribute`並進行`Age`屬性的自動換行`AuthorizeAttribute.Policy`屬性。</span><span class="sxs-lookup"><span data-stu-id="772dc-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="772dc-135">此屬性類型有`Policy`字串，根據的硬式編碼的前置詞 (`"MinimumAge"`) 和建構函式透過傳入整數。</span><span class="sxs-lookup"><span data-stu-id="772dc-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="772dc-136">您可以將其套用到在同一個其他的動作`Authorize`屬性不同之處在於它會接受整數作為參數。</span><span class="sxs-lookup"><span data-stu-id="772dc-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="772dc-137">Custom IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="772dc-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="772dc-138">自訂`MinimumAgeAuthorizeAttribute`輕鬆地要求授權原則的任何所需的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="772dc-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="772dc-139">要解決的下一個問題確保所有這些不同的年齡，授權原則可供使用。</span><span class="sxs-lookup"><span data-stu-id="772dc-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="772dc-140">這正是`IAuthorizationPolicyProvider`很有用。</span><span class="sxs-lookup"><span data-stu-id="772dc-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="772dc-141">使用時`MinimumAgeAuthorizationAttribute`，授權原則名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該產生的授權原則：</span><span class="sxs-lookup"><span data-stu-id="772dc-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="772dc-142">剖析將年齡從原則名稱。</span><span class="sxs-lookup"><span data-stu-id="772dc-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="772dc-143">使用`AuthorizationPolicyBuilder`來建立新的 `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="772dc-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="772dc-144">新增至原則的需求為基礎的年齡，而`AuthorizationPolicyBuilder.AddRequirements`。</span><span class="sxs-lookup"><span data-stu-id="772dc-144">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="772dc-145">在其他情況下，您可能會使用`RequireClaim`， `RequireRole`，或`RequireUserName`改。</span><span class="sxs-lookup"><span data-stu-id="772dc-145">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="772dc-146">多個授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="772dc-146">Multiple authorization policy providers</span></span>

<span data-ttu-id="772dc-147">當使用自訂`IAuthorizationPolicyProvider`實作，請記住，ASP.NET Core 只會使用一個執行個體`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="772dc-147">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="772dc-148">如果自訂提供者不能為所有的原則名稱將用於提供授權原則，它應該改為備份的提供者。</span><span class="sxs-lookup"><span data-stu-id="772dc-148">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="772dc-149">例如，假設應用程式需要自訂的存留期原則和更傳統的以角色為基礎的原則抓取。</span><span class="sxs-lookup"><span data-stu-id="772dc-149">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="772dc-150">這類應用程式可以使用自訂授權原則提供者的：</span><span class="sxs-lookup"><span data-stu-id="772dc-150">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="772dc-151">嘗試剖析原則名稱。</span><span class="sxs-lookup"><span data-stu-id="772dc-151">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="772dc-152">呼叫不同的原則提供者 (例如`DefaultAuthorizationPolicyProvider`) 如果原則名稱未包含的年齡。</span><span class="sxs-lookup"><span data-stu-id="772dc-152">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="772dc-153">此範例`IAuthorizationPolicyProvider`如上所示的實作，請更新為使用`DefaultAuthorizationPolicyProvider`藉由建立一個後援的原則提供者其建構函式中 （使用原則名稱不符合其預期的模式 'MinimumAge' + 年齡）。</span><span class="sxs-lookup"><span data-stu-id="772dc-153">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="772dc-154">然後，`GetPolicyAsync`方法，請更新為使用`FallbackPolicyProvider`而不是傳回 null:</span><span class="sxs-lookup"><span data-stu-id="772dc-154">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="772dc-155">預設原則</span><span class="sxs-lookup"><span data-stu-id="772dc-155">Default policy</span></span>

<span data-ttu-id="772dc-156">除了提供具名的授權原則，自訂`IAuthorizationPolicyProvider`必須實作`GetDefaultPolicyAsync`提供的授權原則`[Authorize]`屬性沒有指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="772dc-156">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="772dc-157">在許多情況下，此授權屬性只需要已驗證的使用者，讓您可以將必要的原則，藉由呼叫`RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="772dc-157">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="772dc-158">使用自訂的所有層面`IAuthorizationPolicyProvider`，您可以視需要自訂。</span><span class="sxs-lookup"><span data-stu-id="772dc-158">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="772dc-159">在某些情況下，可能會想要預設的原則擷取後援`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="772dc-159">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="772dc-160">必要的原則</span><span class="sxs-lookup"><span data-stu-id="772dc-160">Required policy</span></span>

<span data-ttu-id="772dc-161">自訂`IAuthorizationPolicyProvider`需要實作`GetRequiredPolicyAsync`若要選擇性地提供的原則，一律為必要項。</span><span class="sxs-lookup"><span data-stu-id="772dc-161">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="772dc-162">如果`GetRequiredPolicyAsync`傳回非 null 原則，該原則會結合 （具名或預設） 任何其他要求的原則。</span><span class="sxs-lookup"><span data-stu-id="772dc-162">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="772dc-163">如果不需要任何必要的原則，提供者只會傳回 null 或延後至後援的提供者：</span><span class="sxs-lookup"><span data-stu-id="772dc-163">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="772dc-164">使用自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="772dc-164">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="772dc-165">若要使用自訂原則來自`IAuthorizationPolicyProvider`，您必須：</span><span class="sxs-lookup"><span data-stu-id="772dc-165">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="772dc-166">註冊適當`AuthorizationHandler`具有相依性插入的型別 (中所述[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers))，就像使用所有的原則為基礎的授權案例。</span><span class="sxs-lookup"><span data-stu-id="772dc-166">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="772dc-167">註冊自訂`IAuthorizationPolicyProvider`應用程式的相依性插入服務集合中的型別 (在`Startup.ConfigureServices`) 來取代預設的原則提供者。</span><span class="sxs-lookup"><span data-stu-id="772dc-167">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="772dc-168">完整的自訂`IAuthorizationPolicyProvider`範例都提供[aspnet/AuthSamples GitHub 存放庫](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)。</span><span class="sxs-lookup"><span data-stu-id="772dc-168">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
