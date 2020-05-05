---
title: ASP.NET Core 中的自訂授權原則提供者
author: mjrousos
description: 瞭解如何在 ASP.NET Core 應用程式中使用自訂的 IAuthorizationPolicyProvider，以動態方式產生授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 1db78e5b2cea964471e4eea090f713f6af5f4740
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82777536"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="a7a3c-103">在 ASP.NET Core 中使用 IAuthorizationPolicyProvider 的自訂授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="a7a3c-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="a7a3c-104">由[Mike Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="a7a3c-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="a7a3c-105">通常在使用以[原則為基礎的授權](xref:security/authorization/policies)時，會藉`AuthorizationOptions.AddPolicy`由呼叫作為授權服務設定的一部分來註冊原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="a7a3c-106">在某些情況下，不可能（或需要）以這種方式註冊所有授權原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="a7a3c-107">在這些情況下，您可以使用自`IAuthorizationPolicyProvider`定義來控制授權原則的提供方式。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="a7a3c-108">自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能有用的案例範例包括：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="a7a3c-109">使用外部服務來提供原則評估。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="a7a3c-110">使用較大範圍的原則（例如，針對不同的房間號碼或年齡），因此使用`AuthorizationOptions.AddPolicy`呼叫來新增每個授權原則並不合理。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn't make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="a7a3c-111">在執行時間根據外部資料源（例如資料庫）中的資訊建立原則，或透過另一個機制動態判斷授權需求。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="a7a3c-112">從[AspNetCore GitHub 存放庫](https://github.com/dotnet/AspNetCore)中[查看或下載範例程式碼](https://github.com/dotnet/aspnetcore/tree/v3.1.3/src/Security/samples/CustomPolicyProvider)。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-112">[View or download sample code](https://github.com/dotnet/aspnetcore/tree/v3.1.3/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="a7a3c-113">下載 dotnet/AspNetCore 存放庫 ZIP 檔案。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-113">Download the dotnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="a7a3c-114">解壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-114">Unzip the file.</span></span> <span data-ttu-id="a7a3c-115">流覽至*src/Security/samples/CustomPolicyProvider*專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="a7a3c-116">自訂原則抓取</span><span class="sxs-lookup"><span data-stu-id="a7a3c-116">Customize policy retrieval</span></span>

<span data-ttu-id="a7a3c-117">ASP.NET Core 應用程式會使用`IAuthorizationPolicyProvider`介面的執行來取出授權原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="a7a3c-118">根據預設，會註冊並使用[DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) 。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="a7a3c-119">`DefaultAuthorizationPolicyProvider`傳回`IServiceCollection.AddAuthorization`呼叫中所`AuthorizationOptions`提供的原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="a7a3c-120">在應用程式的相依性`IAuthorizationPolicyProvider` [插入](xref:fundamentals/dependency-injection)容器中註冊不同的執行，以自訂此行為。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-120">Customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="a7a3c-121">此`IAuthorizationPolicyProvider`介面包含三個 api：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-121">The `IAuthorizationPolicyProvider` interface contains three APIs:</span></span>

* <span data-ttu-id="a7a3c-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)會傳回指定名稱的授權原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="a7a3c-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)會傳回預設授權原則（未指定原則的`[Authorize]`屬性所使用的原則）。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 
* <span data-ttu-id="a7a3c-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync)會傳回 fallback 授權原則（未指定任何原則時，授權中介軟體所使用的原則）。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) returns the fallback authorization policy (the policy used by the Authorization Middleware when no policy is specified).</span></span> 

<span data-ttu-id="a7a3c-125">藉由執行這些 Api，您可以自訂如何提供授權原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-125">By implementing these APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="a7a3c-126">參數化授權屬性範例</span><span class="sxs-lookup"><span data-stu-id="a7a3c-126">Parameterized authorize attribute example</span></span>

<span data-ttu-id="a7a3c-127">其中一個有用`IAuthorizationPolicyProvider`的案例，就是`[Authorize]`啟用其需求取決於參數的自訂屬性。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-127">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="a7a3c-128">例如，在以[原則為基礎的授權](xref:security/authorization/policies)檔中，以存留期為基礎的（"AtLeast21"）原則是用來做為範例。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-128">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="a7a3c-129">如果應用程式中的不同控制器動作應該提供給*不同*年齡的使用者使用，則有許多不同的以年齡為基礎的原則可能會很有用。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-129">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="a7a3c-130">您可以使用自訂`AuthorizationOptions` `IAuthorizationPolicyProvider`動態產生原則，而不是註冊應用程式在中所需的所有不同以年齡為基礎的原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-130">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="a7a3c-131">若要更輕鬆地使用原則，您可以使用像`[MinimumAgeAuthorize(20)]`是的自訂授權屬性來標注動作。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-131">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="a7a3c-132">自訂授權屬性</span><span class="sxs-lookup"><span data-stu-id="a7a3c-132">Custom Authorization attributes</span></span>

<span data-ttu-id="a7a3c-133">授權原則是以其名稱來識別。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-133">Authorization policies are identified by their names.</span></span> <span data-ttu-id="a7a3c-134">先前所`MinimumAgeAuthorizeAttribute`述的自訂必須將引數對應到可以用來抓取對應授權原則的字串。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-134">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="a7a3c-135">您可以藉由衍生自`AuthorizeAttribute`並讓`Age`屬性包裝`AuthorizeAttribute.Policy`屬性，來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-135">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="a7a3c-136">這個屬性型別具有`Policy`以硬式編碼前置詞（`"MinimumAge"`）為基礎的字串，以及透過函式傳入的整數。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-136">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="a7a3c-137">您可以使用與其他`Authorize`屬性相同的方式將它套用至動作，不同之處在于它會使用整數做為參數。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-137">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="a7a3c-138">自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="a7a3c-138">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="a7a3c-139">自訂`MinimumAgeAuthorizeAttribute`可讓您輕鬆地要求授權原則，以取得所需的最短存留期。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-139">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="a7a3c-140">下一個要解決的問題是確保授權原則適用于所有這些不同的年齡。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-140">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="a7a3c-141">這就是有用`IAuthorizationPolicyProvider`的地方。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-141">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="a7a3c-142">使用`MinimumAgeAuthorizationAttribute`時，授權原則名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該會藉由下列方式產生授權原則：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-142">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="a7a3c-143">正在剖析原則名稱中的年齡。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-143">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="a7a3c-144">使用`AuthorizationPolicyBuilder`建立新的`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="a7a3c-144">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="a7a3c-145">在此和下列範例中，會假設使用者是透過 cookie 進行驗證。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-145">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="a7a3c-146">應該`AuthorizationPolicyBuilder`使用至少一個授權配置名稱來建立，或一律成功。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-146">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="a7a3c-147">否則，沒有關于如何為使用者提供挑戰的資訊，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-147">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="a7a3c-148">根據年齡將需求新增至原則`AuthorizationPolicyBuilder.AddRequirements`。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-148">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="a7a3c-149">在其他案例中，您可以`RequireClaim`改用`RequireRole`、或`RequireUserName` 。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-149">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="a7a3c-150">多個授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="a7a3c-150">Multiple authorization policy providers</span></span>

<span data-ttu-id="a7a3c-151">使用自訂`IAuthorizationPolicyProvider`的執行時，請記住，ASP.NET Core 只會使用一個`IAuthorizationPolicyProvider`的實例。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-151">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="a7a3c-152">如果自訂提供者無法針對將使用的所有原則名稱提供授權原則，它應該會延遲到備份提供者。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-152">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should defer to a backup provider.</span></span> 

<span data-ttu-id="a7a3c-153">例如，假設有一個應用程式同時需要自訂年齡原則和更傳統的以角色為基礎的原則抓取。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-153">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="a7a3c-154">這類應用程式可以使用自訂授權原則提供者：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-154">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="a7a3c-155">嘗試剖析原則名稱。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-155">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="a7a3c-156">如果原則名稱不包含年齡，則`DefaultAuthorizationPolicyProvider`會呼叫不同的原則提供者（例如）。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-156">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="a7a3c-157">如上所`IAuthorizationPolicyProvider`示的範例執行可以更新為使用， `DefaultAuthorizationPolicyProvider`方法是在其函式中建立備份原則提供者（以在原則名稱不符合預期的 ' MinimumAge ' 和 age 模式時使用）。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-157">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a backup policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="a7a3c-158">然後，可以`GetPolicyAsync`將方法更新為使用， `BackupPolicyProvider`而不是傳回 null：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-158">Then, the `GetPolicyAsync` method can be updated to use the `BackupPolicyProvider` instead of returning null:</span></span>

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="a7a3c-159">預設原則</span><span class="sxs-lookup"><span data-stu-id="a7a3c-159">Default policy</span></span>

<span data-ttu-id="a7a3c-160">除了提供命名的授權原則之外，自訂`IAuthorizationPolicyProvider`需要執行`GetDefaultPolicyAsync`以提供`[Authorize]`屬性的授權原則，而不需指定原則名稱。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-160">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="a7a3c-161">在許多情況下，此授權屬性只需要已驗證的使用者，因此您可以透過呼叫來`RequireAuthenticatedUser`建立必要的原則：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-161">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="a7a3c-162">如同自訂`IAuthorizationPolicyProvider`的所有層面，您可以視需要自訂此項。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-162">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="a7a3c-163">在某些情況下，您可能會想要從回復取得預設原則`IAuthorizationPolicyProvider`。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-163">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="fallback-policy"></a><span data-ttu-id="a7a3c-164">Fallback 原則</span><span class="sxs-lookup"><span data-stu-id="a7a3c-164">Fallback policy</span></span>

<span data-ttu-id="a7a3c-165">自訂`IAuthorizationPolicyProvider`可以選擇性地`GetFallbackPolicyAsync`執行，以提供[結合原則](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine)時和未指定任何原則時所使用的原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-165">A custom `IAuthorizationPolicyProvider` can optionally implement `GetFallbackPolicyAsync` to provide a policy that's used when [combining policies](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) and when no policies are specified.</span></span> <span data-ttu-id="a7a3c-166">如果`GetFallbackPolicyAsync`傳回非 null 原則，則當要求未指定任何原則時，授權中介軟體會使用傳回的原則。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-166">If `GetFallbackPolicyAsync` returns a non-null policy, the returned policy is used by the Authorization Middleware when no policies are specified for the request.</span></span>

<span data-ttu-id="a7a3c-167">如果沒有必要的回溯原則，提供者可以傳回`null`或延遲到回溯提供者：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-167">If no fallback policy is required, the provider can return `null` or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="a7a3c-168">使用自訂 IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="a7a3c-168">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="a7a3c-169">若要從使用自訂`IAuthorizationPolicyProvider`原則，您必須：</span><span class="sxs-lookup"><span data-stu-id="a7a3c-169">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="a7a3c-170">使用相依性`AuthorizationHandler`插入（如以[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers)中所述）來註冊適當的類型，如同所有以原則為基礎的授權案例。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-170">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="a7a3c-171">在應用程式`IAuthorizationPolicyProvider`的相依性插入服務集合（在中`Startup.ConfigureServices`）中註冊自訂類型，以取代預設原則提供者。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-171">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="a7a3c-172">您可以在`IAuthorizationPolicyProvider` [Dotnet/aspnetcore GitHub 存放庫](https://github.com/dotnet/aspnetcore/tree/v3.1.3/src/Security/samples/CustomPolicyProvider)中取得完整的自訂範例。</span><span class="sxs-lookup"><span data-stu-id="a7a3c-172">A complete custom `IAuthorizationPolicyProvider` sample is available in the [dotnet/aspnetcore GitHub repository](https://github.com/dotnet/aspnetcore/tree/v3.1.3/src/Security/samples/CustomPolicyProvider).</span></span>
