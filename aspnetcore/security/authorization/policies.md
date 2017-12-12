---
title: "以原則為基礎的自訂授權"
author: rick-anderson
description: "本文件說明如何建立和使用 ASP.NET Core 應用程式中的自訂授權原則的處理常式。"
keywords: "ASP.NET Core 授權、 自訂原則、 授權原則"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a>以原則為基礎的自訂授權

<a name="security-authorization-policies-based"></a>

基本上，[角色授權](roles.md)和[宣告授權](claims.md)使需求的使用、 需求，並預先設定的原則的處理常式。 這些建置組塊可讓您快速的程式碼，以便更豐富、 可重複使用和授權可輕鬆地測試結構授權評估。

授權原則是由一或多個需求所組成，並中註冊應用程式啟動時做為授權服務組態的一部分`ConfigureServices`中*Startup.cs*檔案。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

您可以查看與單一的需求，並確認的最低存在時間，這做為參數傳遞的需求，建立 「 Over21 」 原則。

原則會套用使用`Authorize`藉由指定原則名稱，例如，屬性

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>需求

授權需求是原則可以用來評估目前的使用者主體的資料參數的集合。 在最短使用期限原則中，我們的需求會是單一參數的最低存在時間。 必須實作一項需求`IAuthorizationRequirement`。 這是空的標記介面。 參數化的最低年齡要求 」 可能的實作，如下所示。

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

一項需求不需要將資料或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權的處理常式

授權處理常式會負責評估的一項需求的任何屬性。 授權的處理常式必須評估他們提供`AuthorizationHandlerContext`決定如果允許授權。 可以有一項需求[多個處理常式](policies.md#security-authorization-policies-based-multiple-handlers)。 處理常式必須繼承`AuthorizationHandler<T>`其中 T 是它所處理的需求。

<a name="security-authorization-handler-example"></a>

最短使用期限處理常式可能如下所示：

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

上述程式碼，我們先來看看目前的使用者主體已宣告它發出的簽發者，我們已經知道與信任的出生日期。 如果宣告是遺漏我們無法授權讓我們決定傳回。 如果我們擁有宣告時，我們找出舊的使用者，且符合傳入要求的最低存在時間然後授權已成功。 成功授權之後我們呼叫`context.Succeed()`中的需求，已成功做為參數傳遞。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理常式註冊
處理常式必須是集合中註冊服務在設定期間例如

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

每個處理常式加入使用 services 集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`傳送處理常式類別中。

## <a name="what-should-a-handler-return"></a>功能應該處理常式傳回？

您可以看到我們[處理常式範例](policies.md#security-authorization-handler-example)，`Handle()`方法有沒有傳回值，因此如何執行我們表示成功或失敗？

* 處理常式呼叫來表示成功`context.Succeed(IAuthorizationRequirement requirement)`，傳遞需求已順利通過驗證。

* 處理常式不需一般而言，處理失敗，因為相同的需求的其他處理常式可能會成功。

* 若要保證失敗，即使成功之需求的其他處理常式，呼叫`context.Fail`。

不論您呼叫您的處理常式內，將原則需要需求時呼叫之需求的所有處理常式。 這可讓具有副作用，例如記錄，就會一律進行需求即使`context.Fail()`已經在另一個處理常式中呼叫。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>為什麼需要多個處理常式之需求？

在您想要評估的情況下**或**實作單一需求的多個處理常式的基礎。 例如，Microsoft 有門只開啟與索引鍵的卡片。 如果您在家離開您金鑰卡接線生列印暫存的貼紙，並會為您開啟媒體櫃門。 在此案例中，您會有一個需求， *EnterBuilding*，但多個處理常式，每個檢查單一的需求。

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

現在，假設這兩個處理常式會[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)當原則評估`EnterBuildingRequirement`如果任一個處理常式成功原則評估會成功。

## <a name="using-a-func-to-fulfill-a-policy"></a>符合原則中使用函式

可能的情況下是簡單程式碼來表達履行原則。 很可能只需要提供`Func<AuthorizationHandlerContext, bool>`時設定與原則`RequireAssertion`原則產生器。

例如先前`BadgeEntryHandler`可以改寫，如下所示：

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>在處理常式中存取的 MVC 要求內容

`Handle`授權處理常式中，您必須實作的方法有兩個參數，`AuthorizationContext`和`Requirement`所處理。 任何將物件加入至可用的架構，例如 MVC 或 Jabbr`Resource`屬性`AuthorizationContext`通過額外的資訊。

MVC 的執行個體的傳遞，例如`Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext`資源屬性用來存取 HttpContext、 RouteData 和所有項目中提供其他 MVC。

使用`Resource`屬性是特定的架構。 使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。 您應該轉換`Resource`屬性使用`as`關鍵字，然後檢查已轉型成功確定不會損毀您的程式碼與`InvalidCastExceptions`其他架構; 上執行時

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
