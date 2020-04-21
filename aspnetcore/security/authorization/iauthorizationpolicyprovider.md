---
title: ASP.NET核心中的自訂授權策略提供者
author: mjrousos
description: 瞭解如何在ASP.NET核心應用中使用自定義I授權策略提供程式動態生成授權策略。
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 2c67e25ff73bc8c3a5f3af4730a509b2385fc1cf
ms.sourcegitcommit: 5547d920f322e5a823575c031529e4755ab119de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/21/2020
ms.locfileid: "81661776"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>自訂授權政策提供者使用 i 授權政策提供者ASP.NET核心 

由[邁克·盧梭斯](https://github.com/mjrousos)

通常,在使用[基於策略的授權](xref:security/authorization/policies)時,策略是通過`AuthorizationOptions.AddPolicy`調用 作為授權服務配置的一部分進行註冊的。 在某些情況下,不可能(或不需要)以這種方式註冊所有授權策略。 在這些情況下,可以使用自定義來控制`IAuthorizationPolicyProvider`如何提供授權策略。

自訂[I 授權政策提供者](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider)可能有用的方案範例包括:

* 使用外部服務提供策略評估。
* 使用大量策略(例如,針對不同的房間號碼或年齡),因此使用`AuthorizationOptions.AddPolicy`調用添加每個單獨的授權策略沒有意義。
* 根據外部資料來源(如資料庫)中的資訊在執行時創建策略,或透過另一種機制動態確定授權要求。

從[AspNetCore GitHub 儲存函式庫](https://github.com/dotnet/AspNetCore)[檢視或下載範例代碼](https://github.com/dotnet/aspnetcore/tree/v3.1.3/src/Security/samples/CustomPolicyProvider)。 下載點網/阿斯普內科存儲庫 ZIP 檔。 解壓縮檔。 導航到*src/安全/範例/自定義策略提供程式*專案資料夾。

## <a name="customize-policy-retrieval"></a>自訂策略檢索

ASP.NET核心應用使用介面的`IAuthorizationPolicyProvider`實現來檢索授權策略。 預設情況下,[預設授權策略提供程式](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider)已註冊和使用。 `DefaultAuthorizationPolicyProvider`從`IServiceCollection.AddAuthorization`呼叫中提供`AuthorizationOptions`的策略返回策略。

通過在應用[的依賴項注入](xref:fundamentals/dependency-injection)容器`IAuthorizationPolicyProvider`中註冊 不同的實現來自定義此行為。 

該`IAuthorizationPolicyProvider`介面包含三個 API:

* [獲取策略同步](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_)返回給定名稱的授權策略。
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync)傳回預設授權原則`[Authorize]`(用於未指定策略的屬性的策略)。 
* [GetBackbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync)傳回回退授權策略(未指定策略時授權中間件使用的策略)。 

通過實現這些 API,您可以自定義如何提供授權策略。

## <a name="parameterized-authorize-attribute-example"></a>參數化授權屬性範例

一`IAuthorizationPolicyProvider`個有用的方案是啟用其要求`[Authorize]`依賴於參數的自定義屬性。 例如,[在基於策略的授權](xref:security/authorization/policies)文檔中,使用基於年齡("AtLeast21")的策略作為示例。 如果應用中的不同控制器操作應提供給*不同*年齡的使用者,則使用許多不同的基於年齡的策略可能很有用。 無需註冊應用程式將需要的所有`AuthorizationOptions`不同基於年齡的策略,而是可以使用自`IAuthorizationPolicyProvider`定義 動態生成策略。 為了簡化使用策略,可以使用自定義授權屬性(如`[MinimumAgeAuthorize(20)]`)對操作進行分給。

## <a name="custom-authorization-attributes"></a>自訂授權屬性

授權策略由其名稱識別。 前面描述的`MinimumAgeAuthorizeAttribute`自定義需要將參數映射到可用於檢索相應授權策略的字串中。 可以通過派生`AuthorizeAttribute`和`Age`使 屬性`AuthorizeAttribute.Policy`換行屬性 來執行此操作。

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

此屬性類型具有一個`Policy`基於硬編碼首碼`"MinimumAge"`( ) 的字串,以及透過建構函數傳入的整數。

可以將其應用於操作的方式與其他`Authorize`屬性相同,只不過它採用整數作為參數。

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>自訂授權原則提供者

該自定義`MinimumAgeAuthorizeAttribute`使請求任何所需的最低年齡的授權策略變得容易。 要解決的下一個問題是確保授權策略可用於所有這些不同年齡。 這是有用的`IAuthorizationPolicyProvider`。

使用`MinimumAgeAuthorizationAttribute`時,授權策略名稱將遵循`"MinimumAge" + Age`模式 ,因此`IAuthorizationPolicyProvider`自定義 應通過以下方式生成授權策略:

* 從策略名稱分析年齡。
* 建立`AuthorizationPolicyBuilder`新`AuthorizationPolicy`
* 在此和下面的示例中,假定用戶通過 Cookie 進行身份驗證。 `AuthorizationPolicyBuilder`應至少使用一個授權方案名稱構造 ,或者始終成功。 否則,沒有關於如何向使用者提供質詢的資訊,並且將引發異常。
* 根據具有`AuthorizationPolicyBuilder.AddRequirements`的年齡向策略添加要求。 在其他機制中,您可以使用,`RequireClaim``RequireUserName`或是改`RequireRole`為 使用 。

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

## <a name="multiple-authorization-policy-providers"></a>多個授權原則提供者

使用自定義`IAuthorizationPolicyProvider`實現時,請記住,ASP.NET Core`IAuthorizationPolicyProvider`僅使用 的一個實例。 如果自定義提供程式無法為將使用的所有策略名稱提供授權策略,則應將其服從備份提供程式。 

例如,考慮一個既需要自定義年齡策略又需要更傳統的基於角色的策略檢索的應用程式。 此類應用可以使用自訂授權政策提供者:

* 嘗試分析策略名稱。 
* 如果策略名稱不包含年齡,則調用`DefaultAuthorizationPolicyProvider`其他策略提供程式(如 )。

可以通過`IAuthorizationPolicyProvider`在其建構函數中創建備份策略提供程式來`DefaultAuthorizationPolicyProvider`更新 上述範例實現以使用 (在策略名稱與其預期模式"最小年齡"+ 年齡不匹配時使用)。

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

然後,`GetPolicyAsync`可以更新該方法以`BackupPolicyProvider`使用 而不是傳回 null:

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>預設原則

除了提供命名授權策略外,還需要`IAuthorizationPolicyProvider`實現`GetDefaultPolicyAsync`為未指定策略`[Authorize]`名稱的屬性提供授權策略。

在許多情況下,此授權屬性只需要經過身份驗證的使用者,因此可以使用呼叫`RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

與自定義`IAuthorizationPolicyProvider`的所有方面一樣,您可以根據需要自定義此內容。 在某些情況下,可能需要從回退`IAuthorizationPolicyProvider`檢索預設策略。

## <a name="fallback-policy"></a>遞迴退原則

自定義`IAuthorizationPolicyProvider`可以選擇`GetFallbackPolicyAsync`實現 以提供在[組合策略](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine)時和未指定策略時使用的策略。 如果`GetFallbackPolicyAsync`返回非空策略,則當未為請求指定策略時,授權中間件將使用返回的策略。

如果不需要回退策略,提供程式可以返回`null`或延遲回退提供者:

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>使用自訂 I 授權政策提供者

要使用的自訂策略`IAuthorizationPolicyProvider`,必須:

* 使用依賴項`AuthorizationHandler`注入註冊適當的類型(在[基於策略的授權](xref:security/authorization/policies#authorization-handlers)中描述),與所有基於策略的授權方案一樣。
* 在應用的依賴`IAuthorizationPolicyProvider`項注入服務集合(在中)`Startup.ConfigureServices`中註冊自定義類型以替換預設策略提供程式。

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

完整的自定義`IAuthorizationPolicyProvider`示例在[dotnet/aspnetcore GitHub 儲存庫](https://github.com/dotnet/aspnetcore/tree/v3.1.3/src/Security/samples/CustomPolicyProvider)中可用。
