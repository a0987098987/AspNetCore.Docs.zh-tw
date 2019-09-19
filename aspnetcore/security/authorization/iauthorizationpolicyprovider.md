---
title: ASP.NET Core 中的自訂授權原則提供者
author: mjrousos
description: 瞭解如何在 ASP.NET Core 應用程式中使用自訂的 IAuthorizationPolicyProvider，以動態方式產生授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108059"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="52884-103">在 ASP.NET Core 中使用 IAuthorizationPolicyProvider 的自訂授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="52884-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="52884-104">由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="52884-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="52884-105">通常在使用以[原則為基礎的授權](xref:security/authorization/policies)時，會藉`AuthorizationOptions.AddPolicy`由呼叫作為授權服務設定的一部分來註冊原則。</span><span class="sxs-lookup"><span data-stu-id="52884-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="52884-106">在某些情況下，不可能（或需要）以這種方式註冊所有授權原則。</span><span class="sxs-lookup"><span data-stu-id="52884-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="52884-107">在這些情況下，您可以使用自`IAuthorizationPolicyProvider`定義來控制授權原則的提供方式。</span><span class="sxs-lookup"><span data-stu-id="52884-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="52884-108">自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能有用的案例範例包括：</span><span class="sxs-lookup"><span data-stu-id="52884-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="52884-109">使用外部服務來提供原則評估。</span><span class="sxs-lookup"><span data-stu-id="52884-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="52884-110">使用較大範圍的原則（例如，針對不同的房間號碼或年齡），因此使用`AuthorizationOptions.AddPolicy`呼叫來新增每個授權原則並不合理。</span><span class="sxs-lookup"><span data-stu-id="52884-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="52884-111">在執行時間根據外部資料源（例如資料庫）中的資訊建立原則，或透過另一個機制動態判斷授權需求。</span><span class="sxs-lookup"><span data-stu-id="52884-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="52884-112">從[AspNetCore GitHub 存放庫](https://github.com/aspnet/AspNetCore)中[查看或下載範例程式碼](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)。</span><span class="sxs-lookup"><span data-stu-id="52884-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="52884-113">下載 aspnet/AspNetCore 存放庫 ZIP 檔案。</span><span class="sxs-lookup"><span data-stu-id="52884-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="52884-114">解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="52884-114">Unzip the file.</span></span> <span data-ttu-id="52884-115">流覽至*src/Security/samples/CustomPolicyProvider*專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="52884-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="52884-116">自訂原則抓取</span><span class="sxs-lookup"><span data-stu-id="52884-116">Customize policy retrieval</span></span>

<span data-ttu-id="52884-117">ASP.NET Core 應用程式會使用`IAuthorizationPolicyProvider`介面的執行來取出授權原則。</span><span class="sxs-lookup"><span data-stu-id="52884-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="52884-118">根據預設，會註冊並使用[DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) 。</span><span class="sxs-lookup"><span data-stu-id="52884-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="52884-119">`DefaultAuthorizationPolicyProvider`傳回`AuthorizationOptions` 呼叫`IServiceCollection.AddAuthorization`中所提供的原則。</span><span class="sxs-lookup"><span data-stu-id="52884-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="52884-120">您可以在應用程式的相依性`IAuthorizationPolicyProvider` [插入](xref:fundamentals/dependency-injection)容器中註冊不同的執行，以自訂此行為。</span><span class="sxs-lookup"><span data-stu-id="52884-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="52884-121">此`IAuthorizationPolicyProvider`介面包含兩個 api：</span><span class="sxs-lookup"><span data-stu-id="52884-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="52884-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)會傳回指定名稱的授權原則。</span><span class="sxs-lookup"><span data-stu-id="52884-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="52884-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)會傳回預設授權原則（未指定原則的`[Authorize]`屬性所使用的原則）。</span><span class="sxs-lookup"><span data-stu-id="52884-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="52884-124">藉由執行這兩個 Api，您可以自訂如何提供授權原則。</span><span class="sxs-lookup"><span data-stu-id="52884-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="52884-125">參數化授權屬性範例</span><span class="sxs-lookup"><span data-stu-id="52884-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="52884-126">其中一個有用`IAuthorizationPolicyProvider`的案例，就是`[Authorize]`啟用其需求取決於參數的自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="52884-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="52884-127">例如，在以[原則為基礎的授權](xref:security/authorization/policies)檔中，以存留期為基礎的（"AtLeast21"）原則是用來做為範例。</span><span class="sxs-lookup"><span data-stu-id="52884-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="52884-128">如果應用程式中的不同控制器動作應該提供給*不同*年齡的使用者使用，則有許多不同的以年齡為基礎的原則可能會很有用。</span><span class="sxs-lookup"><span data-stu-id="52884-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="52884-129">您可以使用自訂`AuthorizationOptions` `IAuthorizationPolicyProvider`動態產生原則，而不是註冊應用程式在中所需的所有不同以年齡為基礎的原則。</span><span class="sxs-lookup"><span data-stu-id="52884-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="52884-130">若要更輕鬆地使用原則，您可以使用像`[MinimumAgeAuthorize(20)]`是的自訂授權屬性來標注動作。</span><span class="sxs-lookup"><span data-stu-id="52884-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="52884-131">自訂授權屬性</span><span class="sxs-lookup"><span data-stu-id="52884-131">Custom Authorization attributes</span></span>

<span data-ttu-id="52884-132">授權原則是以其名稱來識別。</span><span class="sxs-lookup"><span data-stu-id="52884-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="52884-133">先前所`MinimumAgeAuthorizeAttribute`述的自訂必須將引數對應到可以用來抓取對應授權原則的字串。</span><span class="sxs-lookup"><span data-stu-id="52884-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="52884-134">您可以藉由衍生自`AuthorizeAttribute`並`Age`讓屬性包裝`AuthorizeAttribute.Policy`屬性，來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="52884-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="52884-135">這個屬性型別具有`Policy`以硬式編碼前置詞（`"MinimumAge"`）為基礎的字串，以及透過函式傳入的整數。</span><span class="sxs-lookup"><span data-stu-id="52884-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="52884-136">您可以使用與其他`Authorize`屬性相同的方式將它套用至動作，不同之處在于它會使用整數做為參數。</span><span class="sxs-lookup"><span data-stu-id="52884-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="52884-137">自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="52884-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="52884-138">自訂`MinimumAgeAuthorizeAttribute`可讓您輕鬆地要求授權原則，以取得所需的最短存留期。</span><span class="sxs-lookup"><span data-stu-id="52884-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="52884-139">下一個要解決的問題是確保授權原則適用于所有這些不同的年齡。</span><span class="sxs-lookup"><span data-stu-id="52884-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="52884-140">這就是有用`IAuthorizationPolicyProvider`的地方。</span><span class="sxs-lookup"><span data-stu-id="52884-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="52884-141">使用`MinimumAgeAuthorizationAttribute`時，授權原則名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該會藉由下列方式產生授權原則：</span><span class="sxs-lookup"><span data-stu-id="52884-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="52884-142">正在剖析原則名稱中的年齡。</span><span class="sxs-lookup"><span data-stu-id="52884-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="52884-143">使用`AuthorizationPolicyBuilder`建立新的`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="52884-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="52884-144">在此和下列範例中，會假設使用者是透過 cookie 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="52884-144">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="52884-145">應該`AuthorizationPolicyBuilder`使用至少一個授權配置名稱來建立，或一律成功。</span><span class="sxs-lookup"><span data-stu-id="52884-145">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="52884-146">否則，沒有關于如何為使用者提供挑戰的資訊，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52884-146">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="52884-147">根據年齡`AuthorizationPolicyBuilder.AddRequirements`將需求新增至原則。</span><span class="sxs-lookup"><span data-stu-id="52884-147">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="52884-148">在其他案例中，您`RequireClaim`可以改用、 `RequireRole`或`RequireUserName` 。</span><span class="sxs-lookup"><span data-stu-id="52884-148">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="52884-149">多個授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="52884-149">Multiple authorization policy providers</span></span>

<span data-ttu-id="52884-150">使用自訂`IAuthorizationPolicyProvider`的執行時，請記住，ASP.NET Core 只會使用一個`IAuthorizationPolicyProvider`的實例。</span><span class="sxs-lookup"><span data-stu-id="52884-150">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="52884-151">如果自訂提供者無法針對將使用的所有原則名稱提供授權原則，則應該切換回備份提供者。</span><span class="sxs-lookup"><span data-stu-id="52884-151">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="52884-152">例如，假設有一個應用程式同時需要自訂年齡原則和更傳統的以角色為基礎的原則抓取。</span><span class="sxs-lookup"><span data-stu-id="52884-152">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="52884-153">這類應用程式可以使用自訂授權原則提供者：</span><span class="sxs-lookup"><span data-stu-id="52884-153">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="52884-154">嘗試剖析原則名稱。</span><span class="sxs-lookup"><span data-stu-id="52884-154">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="52884-155">如果原則名稱不包含年齡，則`DefaultAuthorizationPolicyProvider`會呼叫不同的原則提供者（例如）。</span><span class="sxs-lookup"><span data-stu-id="52884-155">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="52884-156">如上所`IAuthorizationPolicyProvider`示的範例執行可以更新為使用， `DefaultAuthorizationPolicyProvider`方法是在其函式中建立回溯原則提供者（如果原則名稱不符合預期的 ' MinimumAge ' + age 模式，則會使用此功能）。</span><span class="sxs-lookup"><span data-stu-id="52884-156">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="52884-157">然後， `GetPolicyAsync`可以將方法更新為`FallbackPolicyProvider`使用，而不是傳回 null：</span><span class="sxs-lookup"><span data-stu-id="52884-157">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="52884-158">預設原則</span><span class="sxs-lookup"><span data-stu-id="52884-158">Default policy</span></span>

<span data-ttu-id="52884-159">除了提供命名的授權原則之外，自訂`IAuthorizationPolicyProvider`需要執行`GetDefaultPolicyAsync`以`[Authorize]`提供屬性的授權原則，而不需指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="52884-159">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="52884-160">在許多情況下，此授權屬性只需要已驗證的使用者，因此您可以透過呼叫來`RequireAuthenticatedUser`建立必要的原則：</span><span class="sxs-lookup"><span data-stu-id="52884-160">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="52884-161">如同自訂`IAuthorizationPolicyProvider`的所有層面，您可以視需要自訂此項。</span><span class="sxs-lookup"><span data-stu-id="52884-161">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="52884-162">在某些情況下，您可能會想要從`IAuthorizationPolicyProvider`回復取得預設原則。</span><span class="sxs-lookup"><span data-stu-id="52884-162">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="52884-163">必要原則</span><span class="sxs-lookup"><span data-stu-id="52884-163">Required policy</span></span>

<span data-ttu-id="52884-164">自訂`IAuthorizationPolicyProvider`需要執行`GetRequiredPolicyAsync`的，可選擇性地提供一律需要的原則。</span><span class="sxs-lookup"><span data-stu-id="52884-164">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="52884-165">如果`GetRequiredPolicyAsync`傳回非 null 的原則，該原則將會與所要求的任何其他（名稱或預設）原則結合。</span><span class="sxs-lookup"><span data-stu-id="52884-165">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="52884-166">如果不需要任何必要的原則，則提供者可以只傳回 null 或延後至回復提供者：</span><span class="sxs-lookup"><span data-stu-id="52884-166">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="52884-167">使用自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="52884-167">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="52884-168">若要從`IAuthorizationPolicyProvider`使用自訂原則，您必須：</span><span class="sxs-lookup"><span data-stu-id="52884-168">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="52884-169">使用相依性`AuthorizationHandler`插入（如以[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers)中所述）來註冊適當的類型，如同所有以原則為基礎的授權案例。</span><span class="sxs-lookup"><span data-stu-id="52884-169">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="52884-170">在應用程式`IAuthorizationPolicyProvider`的相依性插入服務集合（在中`Startup.ConfigureServices`）中註冊自訂類型，以取代預設原則提供者。</span><span class="sxs-lookup"><span data-stu-id="52884-170">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="52884-171">您可以在`IAuthorizationPolicyProvider` [aspnet/AuthSamples GitHub 存放庫](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)中取得完整的自訂範例。</span><span class="sxs-lookup"><span data-stu-id="52884-171">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
