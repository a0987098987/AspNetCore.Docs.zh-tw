---
title: 在 ASP.NET Core 自訂授權原則提供者
author: mjrousos
description: 了解如何使用 ASP.NET Core 應用程式中的自訂 IAuthorizationPolicyProvider 動態產生的授權原則。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: a5bad88b37d38b819b960b1eb27808d891268c01
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/17/2018
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>使用 ASP.NET Core IAuthorizationPolicyProvider 自訂授權原則提供者 

由[Mike Rousos](https://github.com/mjrousos)

通常當使用[原則為基礎的授權](xref:security/authorization/policies)，原則會藉由呼叫註冊`AuthorizationOptions.AddPolicy`授權服務組態的一部分。 在某些情況下，它可能無法可能 （或需要這樣做） 以這種方式註冊所有授權原則。 在這些情況下，您可以使用自訂`IAuthorizationPolicyProvider`控制授權原則提供的方式。

案例的範例位置自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能會很有用包括：

* 您可以使用外部服務來提供原則評估。
* 使用原則的大範圍 （適用於其他房間數字或時代，例如），因此不會更有意義加入與每個個別的授權原則`AuthorizationOptions.AddPolicy`呼叫。
* 在執行階段根據外部資料來源 （例如資料庫） 中的資訊建立原則，或透過其他機制以動態方式判斷授權需求。

## <a name="customizing-policy-retrieval"></a>自訂原則抓取

ASP.NET Core 應用程式使用的實作`IAuthorizationPolicyProvider`介面，以擷取授權原則。 根據預設， [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)註冊並使用。 `DefaultAuthorizationPolicyProvider` 傳回從原則`AuthorizationOptions`中提供`IServiceCollection.AddAuthorization`呼叫。

您可以自訂此行為，藉由註冊不同`IAuthorizationPolicyProvider`的應用程式中實作[相依性插入](xref:fundamentals/dependency-injection)容器。 

`IAuthorizationPolicyProvider`介面包含兩個 Api:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)傳回具有所指定名稱的授權原則。
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0)傳回預設的授權原則 (所用的原則`[Authorize]`屬性沒有指定的原則)。 

藉由實作這些兩個 Api，您可以自訂如何提供授權原則。

## <a name="parameterized-authorize-attribute-example"></a>參數化授權屬性範例

其中一個情況其中`IAuthorizationPolicyProvider`很有用時啟用自訂`[Authorize]`其需求取決於參數的屬性。 例如，在[原則為基礎的授權](xref:security/authorization/policies)文件集，根據年齡 (「 AtLeast21 」) 原則已做為取樣。 如果應用程式中的不同控制器動作應該可供使用者*不同*年齡，它可能會很實用，因為許多不同年齡為基礎的原則。 而不是註冊的所有不同年齡為基礎原則的應用程式必須在`AuthorizationOptions`，您可以產生動態地自訂原則`IAuthorizationPolicyProvider`。 若要讓您使用原則更容易，您可以標註具有類似的自訂授權屬性動作`[MinimumAgeAuthorize(20)]`。

## <a name="custom-authorization-attributes"></a>自訂授權屬性

授權原則的名稱來識別。 自訂`MinimumAgeAuthorizeAttribute`描述之前必須將引數對應到可以用來擷取對應的授權原則的字串。 您可以藉由衍生自`AuthorizeAttribute`並進行`Age`屬性換行`AuthorizeAttribute.Policy`屬性。

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

此屬性型別具有`Policy`字串根據硬式編碼的前置詞 (`"MinimumAge"`) 和整數傳入透過建構函式。

您可以將其套用動作，在相同方式其他`Authorize`屬性不同之處在於它接受整數做為參數。

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>自訂 IAuthorizationPolicyProvider

自訂`MinimumAgeAuthorizeAttribute`會讓您輕鬆地要求授權原則的任何所需的最低存在時間。 若要解決的下一個問題確定授權原則會使用所有這些不同的年齡。 這是 where`IAuthorizationPolicyProvider`很有用。

當使用`MinimumAgeAuthorizationAttribute`，授權原則的名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該產生的授權原則：

* 剖析將年齡從原則名稱。
* 使用`AuthorizationPolicyBuiler`來建立新的 `AuthorizationPolicy`
* 新增至原則的需求會根據與年齡`AuthorizationPolicyBuilder.AddRequirements`。 在其他情況下，您可以使用`RequireClaim`， `RequireRole`，或`RequireUserName`改為。

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

## <a name="multiple-authorization-policy-providers"></a>多個授權原則提供者

當使用自訂`IAuthorizationPolicyProvider`實作中，請注意，ASP.NET Core 只會使用一個執行個體`IAuthorizationPolicyProvider`。 如果自訂提供者無法提供的所有原則名稱的授權原則，它應該改為備份提供者。 原則名稱可能會包含這些來自於預設原則`[Authorize]`屬性沒有名稱。

例如，請考慮應用程式需要自訂的存在時間原則和更傳統的以角色為基礎的原則抓取。 這類應用程式可以使用自訂授權原則提供者的：

* 嘗試剖析原則名稱。 
* 呼叫不同的原則提供者 (例如`DefaultAuthorizationPolicyProvider`) 如果原則名稱未包含的年齡。

## <a name="default-policy"></a>預設原則

除了提供具名的授權原則，自訂`IAuthorizationPolicyProvider`必須實作`GetDefaultPolicyAsync`提供的授權原則`[Authorize]`沒有指定原則名稱屬性。

在許多情況下，這個授權屬性只需要驗證的使用者，因此您可以藉由呼叫所需的原則`RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

使用自訂的所有層面`IAuthorizationPolicyProvider`，您可以視需要自訂。 在某些情況下：

* 可能不會使用預設的授權原則。
* 擷取預設的原則可以委派給後援`IAuthorizationPolicyProvider`。

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>使用自訂 IAuthorizationPolicyProvider

若要使用自訂原則從`IAuthorizationPolicyProvider`，您必須：

* 註冊適當`AuthorizationHandler`具有相依性插入的型別 (述[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers))，就像使用所有的原則為基礎的授權案例。
* 註冊自訂`IAuthorizationPolicyProvider`應用程式的相依性插入式服務集合中的型別 (在`Startup.ConfigureServices`) 來取代預設原則提供者。

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

完成自訂`IAuthorizationPolicyProvider`範例可用於[aspnet/AuthSamples GitHub 儲存機制](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider)。
