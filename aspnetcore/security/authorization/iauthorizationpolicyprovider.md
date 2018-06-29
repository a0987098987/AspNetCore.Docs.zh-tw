---
title: 在 ASP.NET Core 自訂授權原則提供者
author: mjrousos
description: 了解如何使用 ASP.NET Core 應用程式中的自訂 IAuthorizationPolicyProvider 動態產生的授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 218d7a495655598046671093c0cfe7b9622aca5e
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077598"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="628c0-103">使用 ASP.NET Core IAuthorizationPolicyProvider 自訂授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="628c0-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="628c0-104">由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="628c0-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="628c0-105">通常當使用[原則為基礎的授權](xref:security/authorization/policies)，原則會藉由呼叫註冊`AuthorizationOptions.AddPolicy`授權服務組態的一部分。</span><span class="sxs-lookup"><span data-stu-id="628c0-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="628c0-106">在某些情況下，它可能無法可能 （或需要這樣做） 以這種方式註冊所有授權原則。</span><span class="sxs-lookup"><span data-stu-id="628c0-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="628c0-107">在這些情況下，您可以使用自訂`IAuthorizationPolicyProvider`控制授權原則提供的方式。</span><span class="sxs-lookup"><span data-stu-id="628c0-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="628c0-108">案例的範例位置自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能會很有用包括：</span><span class="sxs-lookup"><span data-stu-id="628c0-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="628c0-109">您可以使用外部服務來提供原則評估。</span><span class="sxs-lookup"><span data-stu-id="628c0-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="628c0-110">使用原則的大範圍 （適用於其他房間數字或時代，例如），因此不會更有意義加入與每個個別的授權原則`AuthorizationOptions.AddPolicy`呼叫。</span><span class="sxs-lookup"><span data-stu-id="628c0-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="628c0-111">在執行階段根據外部資料來源 （例如資料庫） 中的資訊建立原則，或透過其他機制以動態方式判斷授權需求。</span><span class="sxs-lookup"><span data-stu-id="628c0-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="628c0-112">自訂原則抓取</span><span class="sxs-lookup"><span data-stu-id="628c0-112">Customizing policy retrieval</span></span>

<span data-ttu-id="628c0-113">ASP.NET Core 應用程式使用的實作`IAuthorizationPolicyProvider`介面，以擷取授權原則。</span><span class="sxs-lookup"><span data-stu-id="628c0-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="628c0-114">根據預設， [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)註冊並使用。</span><span class="sxs-lookup"><span data-stu-id="628c0-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="628c0-115">`DefaultAuthorizationPolicyProvider` 傳回從原則`AuthorizationOptions`中提供`IServiceCollection.AddAuthorization`呼叫。</span><span class="sxs-lookup"><span data-stu-id="628c0-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="628c0-116">您可以自訂此行為，藉由註冊不同`IAuthorizationPolicyProvider`的應用程式中實作[相依性插入](xref:fundamentals/dependency-injection)容器。</span><span class="sxs-lookup"><span data-stu-id="628c0-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="628c0-117">`IAuthorizationPolicyProvider`介面包含兩個 Api:</span><span class="sxs-lookup"><span data-stu-id="628c0-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="628c0-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)傳回具有所指定名稱的授權原則。</span><span class="sxs-lookup"><span data-stu-id="628c0-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="628c0-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0)傳回預設的授權原則 (所用的原則`[Authorize]`屬性沒有指定的原則)。</span><span class="sxs-lookup"><span data-stu-id="628c0-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="628c0-120">藉由實作這些兩個 Api，您可以自訂如何提供授權原則。</span><span class="sxs-lookup"><span data-stu-id="628c0-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="628c0-121">參數化授權屬性範例</span><span class="sxs-lookup"><span data-stu-id="628c0-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="628c0-122">其中一個情況其中`IAuthorizationPolicyProvider`很有用時啟用自訂`[Authorize]`其需求取決於參數的屬性。</span><span class="sxs-lookup"><span data-stu-id="628c0-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="628c0-123">例如，在[原則為基礎的授權](xref:security/authorization/policies)文件集，根據年齡 (「 AtLeast21 」) 原則已做為取樣。</span><span class="sxs-lookup"><span data-stu-id="628c0-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="628c0-124">如果應用程式中的不同控制器動作應該可供使用者*不同*年齡，它可能會很實用，因為許多不同年齡為基礎的原則。</span><span class="sxs-lookup"><span data-stu-id="628c0-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="628c0-125">而不是註冊的所有不同年齡為基礎原則的應用程式必須在`AuthorizationOptions`，您可以產生動態地自訂原則`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="628c0-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="628c0-126">若要讓您使用原則更容易，您可以標註具有類似的自訂授權屬性動作`[MinimumAgeAuthorize(20)]`。</span><span class="sxs-lookup"><span data-stu-id="628c0-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="628c0-127">自訂授權屬性</span><span class="sxs-lookup"><span data-stu-id="628c0-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="628c0-128">授權原則的名稱來識別。</span><span class="sxs-lookup"><span data-stu-id="628c0-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="628c0-129">自訂`MinimumAgeAuthorizeAttribute`描述之前必須將引數對應到可以用來擷取對應的授權原則的字串。</span><span class="sxs-lookup"><span data-stu-id="628c0-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="628c0-130">您可以藉由衍生自`AuthorizeAttribute`並進行`Age`屬性換行`AuthorizeAttribute.Policy`屬性。</span><span class="sxs-lookup"><span data-stu-id="628c0-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
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

<span data-ttu-id="628c0-131">此屬性型別具有`Policy`字串根據硬式編碼的前置詞 (`"MinimumAge"`) 和整數傳入透過建構函式。</span><span class="sxs-lookup"><span data-stu-id="628c0-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="628c0-132">您可以將其套用動作，在相同方式其他`Authorize`屬性不同之處在於它接受整數做為參數。</span><span class="sxs-lookup"><span data-stu-id="628c0-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="628c0-133">自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="628c0-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="628c0-134">自訂`MinimumAgeAuthorizeAttribute`會讓您輕鬆地要求授權原則的任何所需的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="628c0-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="628c0-135">若要解決的下一個問題確定授權原則會使用所有這些不同的年齡。</span><span class="sxs-lookup"><span data-stu-id="628c0-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="628c0-136">這是 where`IAuthorizationPolicyProvider`很有用。</span><span class="sxs-lookup"><span data-stu-id="628c0-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="628c0-137">當使用`MinimumAgeAuthorizationAttribute`，授權原則的名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該產生的授權原則：</span><span class="sxs-lookup"><span data-stu-id="628c0-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="628c0-138">剖析將年齡從原則名稱。</span><span class="sxs-lookup"><span data-stu-id="628c0-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="628c0-139">使用`AuthorizationPolicyBuilder`來建立新的 `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="628c0-139">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="628c0-140">新增至原則的需求會根據與年齡`AuthorizationPolicyBuilder.AddRequirements`。</span><span class="sxs-lookup"><span data-stu-id="628c0-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="628c0-141">在其他情況下，您可以使用`RequireClaim`， `RequireRole`，或`RequireUserName`改為。</span><span class="sxs-lookup"><span data-stu-id="628c0-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="628c0-142">多個授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="628c0-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="628c0-143">當使用自訂`IAuthorizationPolicyProvider`實作中，請注意，ASP.NET Core 只會使用一個執行個體`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="628c0-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="628c0-144">如果自訂提供者無法提供的所有原則名稱的授權原則，它應該改為備份提供者。</span><span class="sxs-lookup"><span data-stu-id="628c0-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="628c0-145">原則名稱可能會包含這些來自於預設原則`[Authorize]`屬性沒有名稱。</span><span class="sxs-lookup"><span data-stu-id="628c0-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="628c0-146">例如，請考慮應用程式需要自訂的存在時間原則和更傳統的以角色為基礎的原則抓取。</span><span class="sxs-lookup"><span data-stu-id="628c0-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="628c0-147">這類應用程式可以使用自訂授權原則提供者的：</span><span class="sxs-lookup"><span data-stu-id="628c0-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="628c0-148">嘗試剖析原則名稱。</span><span class="sxs-lookup"><span data-stu-id="628c0-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="628c0-149">呼叫不同的原則提供者 (例如`DefaultAuthorizationPolicyProvider`) 如果原則名稱未包含的年齡。</span><span class="sxs-lookup"><span data-stu-id="628c0-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="628c0-150">預設原則</span><span class="sxs-lookup"><span data-stu-id="628c0-150">Default policy</span></span>

<span data-ttu-id="628c0-151">除了提供具名的授權原則，自訂`IAuthorizationPolicyProvider`必須實作`GetDefaultPolicyAsync`提供的授權原則`[Authorize]`沒有指定原則名稱屬性。</span><span class="sxs-lookup"><span data-stu-id="628c0-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="628c0-152">在許多情況下，這個授權屬性只需要驗證的使用者，因此您可以藉由呼叫所需的原則`RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="628c0-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="628c0-153">使用自訂的所有層面`IAuthorizationPolicyProvider`，您可以視需要自訂。</span><span class="sxs-lookup"><span data-stu-id="628c0-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="628c0-154">在某些情況下：</span><span class="sxs-lookup"><span data-stu-id="628c0-154">In some cases:</span></span>

* <span data-ttu-id="628c0-155">可能不會使用預設的授權原則。</span><span class="sxs-lookup"><span data-stu-id="628c0-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="628c0-156">擷取預設的原則可以委派給後援`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="628c0-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="628c0-157">使用自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="628c0-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="628c0-158">若要使用自訂原則從`IAuthorizationPolicyProvider`，您必須：</span><span class="sxs-lookup"><span data-stu-id="628c0-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="628c0-159">註冊適當`AuthorizationHandler`具有相依性插入的型別 (述[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers))，就像使用所有的原則為基礎的授權案例。</span><span class="sxs-lookup"><span data-stu-id="628c0-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="628c0-160">註冊自訂`IAuthorizationPolicyProvider`應用程式的相依性插入式服務集合中的型別 (在`Startup.ConfigureServices`) 來取代預設原則提供者。</span><span class="sxs-lookup"><span data-stu-id="628c0-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="628c0-161">完成自訂`IAuthorizationPolicyProvider`範例可用於[aspnet/AuthSamples GitHub 儲存機制](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider)。</span><span class="sxs-lookup"><span data-stu-id="628c0-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
