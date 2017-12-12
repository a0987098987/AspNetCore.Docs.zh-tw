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
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="11025-104">以原則為基礎的自訂授權</span><span class="sxs-lookup"><span data-stu-id="11025-104">Custom policy-based authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="11025-105">基本上，[角色授權](roles.md)和[宣告授權](claims.md)使需求的使用、 需求，並預先設定的原則的處理常式。</span><span class="sxs-lookup"><span data-stu-id="11025-105">Underneath the covers, the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement, and a pre-configured policy.</span></span> <span data-ttu-id="11025-106">這些建置組塊可讓您快速的程式碼，以便更豐富、 可重複使用和授權可輕鬆地測試結構授權評估。</span><span class="sxs-lookup"><span data-stu-id="11025-106">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="11025-107">授權原則是由一或多個需求所組成，並中註冊應用程式啟動時做為授權服務組態的一部分`ConfigureServices`中*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="11025-107">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

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

<span data-ttu-id="11025-108">您可以查看與單一的需求，並確認的最低存在時間，這做為參數傳遞的需求，建立 「 Over21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="11025-108">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="11025-109">原則會套用使用`Authorize`藉由指定原則名稱，例如，屬性</span><span class="sxs-lookup"><span data-stu-id="11025-109">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

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

## <a name="requirements"></a><span data-ttu-id="11025-110">需求</span><span class="sxs-lookup"><span data-stu-id="11025-110">Requirements</span></span>

<span data-ttu-id="11025-111">授權需求是原則可以用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="11025-111">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="11025-112">在最短使用期限原則中，我們的需求會是單一參數的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="11025-112">In our Minimum Age policy, the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="11025-113">必須實作一項需求`IAuthorizationRequirement`。</span><span class="sxs-lookup"><span data-stu-id="11025-113">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="11025-114">這是空的標記介面。</span><span class="sxs-lookup"><span data-stu-id="11025-114">This is an empty, marker interface.</span></span> <span data-ttu-id="11025-115">參數化的最低年齡要求 」 可能的實作，如下所示。</span><span class="sxs-lookup"><span data-stu-id="11025-115">A parameterized minimum age requirement might be implemented as follows;</span></span>

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

<span data-ttu-id="11025-116">一項需求不需要將資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="11025-116">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="11025-117">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="11025-117">Authorization handlers</span></span>

<span data-ttu-id="11025-118">授權處理常式會負責評估的一項需求的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="11025-118">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="11025-119">授權的處理常式必須評估他們提供`AuthorizationHandlerContext`決定如果允許授權。</span><span class="sxs-lookup"><span data-stu-id="11025-119">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="11025-120">可以有一項需求[多個處理常式](policies.md#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="11025-120">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="11025-121">處理常式必須繼承`AuthorizationHandler<T>`其中 T 是它所處理的需求。</span><span class="sxs-lookup"><span data-stu-id="11025-121">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="11025-122">最短使用期限處理常式可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="11025-122">The minimum age handler might look like this:</span></span>

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

<span data-ttu-id="11025-123">上述程式碼，我們先來看看目前的使用者主體已宣告它發出的簽發者，我們已經知道與信任的出生日期。</span><span class="sxs-lookup"><span data-stu-id="11025-123">In the code above, we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="11025-124">如果宣告是遺漏我們無法授權讓我們決定傳回。</span><span class="sxs-lookup"><span data-stu-id="11025-124">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="11025-125">如果我們擁有宣告時，我們找出舊的使用者，且符合傳入要求的最低存在時間然後授權已成功。</span><span class="sxs-lookup"><span data-stu-id="11025-125">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="11025-126">成功授權之後我們呼叫`context.Succeed()`中的需求，已成功做為參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="11025-126">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="11025-127">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="11025-127">Handler registration</span></span>
<span data-ttu-id="11025-128">處理常式必須是集合中註冊服務在設定期間例如</span><span class="sxs-lookup"><span data-stu-id="11025-128">Handlers must be registered in the services collection during configuration, for example;</span></span>

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

<span data-ttu-id="11025-129">每個處理常式加入使用 services 集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`傳送處理常式類別中。</span><span class="sxs-lookup"><span data-stu-id="11025-129">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="11025-130">功能應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="11025-130">What should a handler return?</span></span>

<span data-ttu-id="11025-131">您可以看到我們[處理常式範例](policies.md#security-authorization-handler-example)，`Handle()`方法有沒有傳回值，因此如何執行我們表示成功或失敗？</span><span class="sxs-lookup"><span data-stu-id="11025-131">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="11025-132">處理常式呼叫來表示成功`context.Succeed(IAuthorizationRequirement requirement)`，傳遞需求已順利通過驗證。</span><span class="sxs-lookup"><span data-stu-id="11025-132">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="11025-133">處理常式不需一般而言，處理失敗，因為相同的需求的其他處理常式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="11025-133">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="11025-134">若要保證失敗，即使成功之需求的其他處理常式，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="11025-134">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="11025-135">不論您呼叫您的處理常式內，將原則需要需求時呼叫之需求的所有處理常式。</span><span class="sxs-lookup"><span data-stu-id="11025-135">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="11025-136">這可讓具有副作用，例如記錄，就會一律進行需求即使`context.Fail()`已經在另一個處理常式中呼叫。</span><span class="sxs-lookup"><span data-stu-id="11025-136">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="11025-137">為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="11025-137">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="11025-138">在您想要評估的情況下**或**實作單一需求的多個處理常式的基礎。</span><span class="sxs-lookup"><span data-stu-id="11025-138">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="11025-139">例如，Microsoft 有門只開啟與索引鍵的卡片。</span><span class="sxs-lookup"><span data-stu-id="11025-139">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="11025-140">如果您在家離開您金鑰卡接線生列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="11025-140">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="11025-141">在此案例中，您會有一個需求， *EnterBuilding*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="11025-141">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

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

<span data-ttu-id="11025-142">現在，假設這兩個處理常式會[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)當原則評估`EnterBuildingRequirement`如果任一個處理常式成功原則評估會成功。</span><span class="sxs-lookup"><span data-stu-id="11025-142">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="11025-143">符合原則中使用函式</span><span class="sxs-lookup"><span data-stu-id="11025-143">Using a func to fulfill a policy</span></span>

<span data-ttu-id="11025-144">可能的情況下是簡單程式碼來表達履行原則。</span><span class="sxs-lookup"><span data-stu-id="11025-144">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="11025-145">很可能只需要提供`Func<AuthorizationHandlerContext, bool>`時設定與原則`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="11025-145">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="11025-146">例如先前`BadgeEntryHandler`可以改寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="11025-146">For example the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

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

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="11025-147">在處理常式中存取的 MVC 要求內容</span><span class="sxs-lookup"><span data-stu-id="11025-147">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="11025-148">`Handle`授權處理常式中，您必須實作的方法有兩個參數，`AuthorizationContext`和`Requirement`所處理。</span><span class="sxs-lookup"><span data-stu-id="11025-148">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="11025-149">任何將物件加入至可用的架構，例如 MVC 或 Jabbr`Resource`屬性`AuthorizationContext`通過額外的資訊。</span><span class="sxs-lookup"><span data-stu-id="11025-149">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="11025-150">MVC 的執行個體的傳遞，例如`Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext`資源屬性用來存取 HttpContext、 RouteData 和所有項目中提供其他 MVC。</span><span class="sxs-lookup"><span data-stu-id="11025-150">For example, MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="11025-151">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="11025-151">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="11025-152">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="11025-152">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="11025-153">您應該轉換`Resource`屬性使用`as`關鍵字，然後檢查已轉型成功確定不會損毀您的程式碼與`InvalidCastExceptions`其他架構; 上執行時</span><span class="sxs-lookup"><span data-stu-id="11025-153">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
