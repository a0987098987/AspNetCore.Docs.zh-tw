---
title: ASP.NET Core 中的自訂授權原則提供者
author: mjrousos
description: 了解如何在 ASP.NET Core 應用程式中使用自訂 IAuthorizationPolicyProvider，來動態產生的授權原則。
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: ef3e81da6fb9e2e332b553607be35fcd79e9362d
ms.sourcegitcommit: ec71fd5a988f927ae301813aae5ff764feb3bb6a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/12/2019
ms.locfileid: "54249369"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>在 ASP.NET Core 中使用 IAuthorizationPolicyProvider 的自訂授權原則提供者 

藉由[Mike Rousos](https://github.com/mjrousos)

通常使用時[原則為基礎的授權](xref:security/authorization/policies)，藉由呼叫註冊原則`AuthorizationOptions.AddPolicy`授權服務組態的一部分。 在某些情況下，它可能不是辦法 （或不想） 來註冊所有授權原則以這種方式。 在這些情況下，您可以使用自訂`IAuthorizationPolicyProvider`來控制如何提供授權原則。

案例範例，其中自訂[IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能會很有用包括：

* 您可以使用外部服務，提供原則評估。
* 使用大範圍的原則 （適用於不同的空間數字或年齡，例如），因此沒有任何意義加入具有每個個別的授權原則`AuthorizationOptions.AddPolicy`呼叫。
* 在執行階段根據外部資料來源 （例如資料庫） 中的資訊建立原則，或透過其他機制以動態方式判斷授權需求。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/AuthSamples/)從[AspNetCore GitHub 存放庫](https://github.com/aspnet/AspNetCore)。 下載 aspnet/AuthSamples 存放庫的 ZIP 檔案。
將解壓縮*AuthSamples 解壓縮*檔案。 瀏覽至*範例/CustomPolicyProvider*專案資料夾。

## <a name="customize-policy-retrieval"></a>自訂原則抓取

ASP.NET Core 應用程式使用的實作`IAuthorizationPolicyProvider`介面擷取授權原則。 根據預設， [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)在註冊及使用。 `DefaultAuthorizationPolicyProvider` 傳回從原則`AuthorizationOptions`中提供`IServiceCollection.AddAuthorization`呼叫。

您可以自訂此行為由註冊不同`IAuthorizationPolicyProvider`的應用程式中實作[相依性插入](xref:fundamentals/dependency-injection)容器。 

`IAuthorizationPolicyProvider`介面包含兩個 Api:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)傳回指定之名稱的授權原則。
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)傳回預設的授權原則 (所用的原則`[Authorize]`屬性時一定要指定的原則)。 

藉由實作這兩個 Api，您可以自訂授權原則提供的方式。

## <a name="parameterized-authorize-attribute-example"></a>參數化授權屬性範例

其中一個案例所在`IAuthorizationPolicyProvider`很有用啟用自訂`[Authorize]`其需求取決於參數的屬性。 例如，在[原則為基礎的授權](xref:security/authorization/policies)文件，根據年齡 (「 AtLeast21") 作為範例使用了原則。 如果應用程式中的不同控制器動作應該會提供給使用者*不同*年齡，可能有助於將許多不同年齡為基礎的原則。 而不是註冊所有不同年齡為基礎的原則，應用程式必須在`AuthorizationOptions`，您可以產生動態地自訂原則`IAuthorizationPolicyProvider`。 若要讓使用原則更容易，您可以加上註解動作等的自訂授權屬性`[MinimumAgeAuthorize(20)]`。

## <a name="custom-authorization-attributes"></a>自訂授權屬性

授權原則會以其名稱識別。 自訂`MinimumAgeAuthorizeAttribute`描述之前必須將引數對應至字串，可用來擷取對應的授權原則。 您可以藉由衍生自`AuthorizeAttribute`並進行`Age`屬性的自動換行`AuthorizeAttribute.Policy`屬性。

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

此屬性類型有`Policy`字串，根據的硬式編碼的前置詞 (`"MinimumAge"`) 和建構函式透過傳入整數。

您可以將其套用到在同一個其他的動作`Authorize`屬性不同之處在於它會接受整數作為參數。

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>自訂 IAuthorizationPolicyProvider

自訂`MinimumAgeAuthorizeAttribute`輕鬆地要求授權原則的任何所需的最低存在時間。 要解決的下一個問題確保所有這些不同的年齡，授權原則可供使用。 這正是`IAuthorizationPolicyProvider`很有用。

使用時`MinimumAgeAuthorizationAttribute`，授權原則名稱會遵循模式`"MinimumAge" + Age`，因此自訂`IAuthorizationPolicyProvider`應該產生的授權原則：

* 剖析將年齡從原則名稱。
* 使用`AuthorizationPolicyBuilder`來建立新的 `AuthorizationPolicy`
* 新增至原則的需求為基礎的年齡，而`AuthorizationPolicyBuilder.AddRequirements`。 在其他情況下，您可能會使用`RequireClaim`， `RequireRole`，或`RequireUserName`改。

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

## <a name="multiple-authorization-policy-providers"></a>多個授權原則提供者

當使用自訂`IAuthorizationPolicyProvider`實作，請記住，ASP.NET Core 只會使用一個執行個體`IAuthorizationPolicyProvider`。 如果自訂提供者不能提供所有的原則名稱的授權原則，它應該改為備份的提供者。 原則名稱可能包含來自預設原則`[Authorize]`沒有名稱的屬性。

例如，請考慮應用程式需要自訂的存留期原則和更傳統的以角色為基礎的原則抓取。 這類應用程式可以使用自訂授權原則提供者的：

* 嘗試剖析原則名稱。 
* 呼叫不同的原則提供者 (例如`DefaultAuthorizationPolicyProvider`) 如果原則名稱未包含的年齡。

## <a name="default-policy"></a>預設原則

除了提供具名的授權原則，自訂`IAuthorizationPolicyProvider`必須實作`GetDefaultPolicyAsync`提供的授權原則`[Authorize]`屬性沒有指定原則名稱。

在許多情況下，此授權屬性只需要已驗證的使用者，讓您可以將必要的原則，藉由呼叫`RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

使用自訂的所有層面`IAuthorizationPolicyProvider`，您可以視需要自訂。 在某些情況下：

* 可能不會使用預設的授權原則。
* 擷取預設的原則可以委派給後援`IAuthorizationPolicyProvider`。

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>使用自訂 IAuthorizationPolicyProvider

若要使用自訂原則來自`IAuthorizationPolicyProvider`，您必須：

* 註冊適當`AuthorizationHandler`具有相依性插入的型別 (中所述[原則為基礎的授權](xref:security/authorization/policies#authorization-handlers))，就像使用所有的原則為基礎的授權案例。
* 註冊自訂`IAuthorizationPolicyProvider`應用程式的相依性插入服務集合中的型別 (在`Startup.ConfigureServices`) 來取代預設的原則提供者。

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

完整的自訂`IAuthorizationPolicyProvider`範例都提供[aspnet/AuthSamples GitHub 存放庫](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider)。
