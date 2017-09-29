---
title: "以原則為基礎的自訂授權"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 5021b5d20f6d9b9a4d8889f25b5e41f2c9306f64
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="49d35-103">以原則為基礎的自訂授權</span><span class="sxs-lookup"><span data-stu-id="49d35-103">Custom Policy-Based Authorization</span></span>

<a name=security-authorization-policies-based></a>

<span data-ttu-id="49d35-104">基本上[角色授權](roles.md#security-authorization-role-based)和[宣告授權](claims.md#security-authorization-claims-based)使需求的使用、 需求和預先設定的原則的處理常式。</span><span class="sxs-lookup"><span data-stu-id="49d35-104">Underneath the covers the [role authorization](roles.md#security-authorization-role-based) and [claims authorization](claims.md#security-authorization-claims-based) make use of a requirement, a handler for the requirement and a pre-configured policy.</span></span> <span data-ttu-id="49d35-105">這些建置組塊可讓您快速的程式碼，以便更豐富、 可重複使用和授權可輕鬆地測試結構授權評估。</span><span class="sxs-lookup"><span data-stu-id="49d35-105">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="49d35-106">授權原則是由一或多個需求所組成，並中註冊應用程式啟動時做為授權服務組態的一部分`ConfigureServices`中*Startup.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="49d35-106">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

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

<span data-ttu-id="49d35-107">您可以查看與單一的需求，並確認的最低存在時間，這做為參數傳遞的需求，建立 「 Over21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="49d35-107">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="49d35-108">原則會套用使用`Authorize`藉由指定原則名稱，例如，屬性</span><span class="sxs-lookup"><span data-stu-id="49d35-108">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

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

## <a name="requirements"></a><span data-ttu-id="49d35-109">需求</span><span class="sxs-lookup"><span data-stu-id="49d35-109">Requirements</span></span>

<span data-ttu-id="49d35-110">授權需求是原則可以用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="49d35-110">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="49d35-111">在我們的最短使用期限原則的需求，我們有會是單一參數的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="49d35-111">In our Minimum Age policy the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="49d35-112">必須實作一項需求`IAuthorizationRequirement`。</span><span class="sxs-lookup"><span data-stu-id="49d35-112">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="49d35-113">這是空的標記介面。</span><span class="sxs-lookup"><span data-stu-id="49d35-113">This is an empty, marker interface.</span></span> <span data-ttu-id="49d35-114">參數化的最低年齡要求 」 可能的實作，如下所示。</span><span class="sxs-lookup"><span data-stu-id="49d35-114">A parameterized minimum age requirement might be implemented as follows;</span></span>

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

<span data-ttu-id="49d35-115">一項需求不需要將資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="49d35-115">A requirement doesn't need to have data or properties.</span></span>

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a><span data-ttu-id="49d35-116">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="49d35-116">Authorization Handlers</span></span>

<span data-ttu-id="49d35-117">授權處理常式會負責評估的一項需求的任何屬性。</span><span class="sxs-lookup"><span data-stu-id="49d35-117">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="49d35-118">授權的處理常式必須評估他們提供`AuthorizationHandlerContext`決定如果允許授權。</span><span class="sxs-lookup"><span data-stu-id="49d35-118">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="49d35-119">可以有一項需求[多個處理常式](policies.md#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="49d35-119">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="49d35-120">處理常式必須繼承`AuthorizationHandler<T>`其中 T 是它所處理的需求。</span><span class="sxs-lookup"><span data-stu-id="49d35-120">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name=security-authorization-handler-example></a>

<span data-ttu-id="49d35-121">最短使用期限處理常式可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="49d35-121">The minimum age handler might look like this:</span></span>

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

<span data-ttu-id="49d35-122">上方的程式碼我們先來看看目前的使用者主體已宣告它發出的簽發者，我們已經知道與信任的出生日期。</span><span class="sxs-lookup"><span data-stu-id="49d35-122">In the code above we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="49d35-123">如果宣告是遺漏我們無法授權讓我們決定傳回。</span><span class="sxs-lookup"><span data-stu-id="49d35-123">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="49d35-124">如果我們擁有宣告時，我們找出舊的使用者，且符合傳入要求的最低存在時間然後授權已成功。</span><span class="sxs-lookup"><span data-stu-id="49d35-124">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="49d35-125">成功授權之後我們呼叫`context.Succeed()`中的需求，已成功做為參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="49d35-125">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name=security-authorization-policies-based-handler-registration></a>

<span data-ttu-id="49d35-126">處理常式必須是集合中註冊服務在設定期間例如</span><span class="sxs-lookup"><span data-stu-id="49d35-126">Handlers must be registered in the services collection during configuration, for example;</span></span>

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

<span data-ttu-id="49d35-127">每個處理常式加入使用 services 集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`傳送處理常式類別中。</span><span class="sxs-lookup"><span data-stu-id="49d35-127">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="49d35-128">功能應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="49d35-128">What should a handler return?</span></span>

<span data-ttu-id="49d35-129">您可以看到我們[處理常式範例](policies.md#security-authorization-handler-example)，`Handle()`方法有沒有傳回值，因此如何執行我們表示成功或失敗？</span><span class="sxs-lookup"><span data-stu-id="49d35-129">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="49d35-130">處理常式呼叫來表示成功`context.Succeed(IAuthorizationRequirement requirement)`，傳遞需求已順利通過驗證。</span><span class="sxs-lookup"><span data-stu-id="49d35-130">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="49d35-131">處理常式不需一般而言，處理失敗，因為相同的需求的其他處理常式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="49d35-131">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="49d35-132">若要保證失敗，即使成功之需求的其他處理常式，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="49d35-132">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="49d35-133">不論您呼叫您的處理常式內之需求的所有處理常式會呼叫時的原則需要的需求。</span><span class="sxs-lookup"><span data-stu-id="49d35-133">Regardless of what you call inside your handler all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="49d35-134">這可讓具有副作用，例如記錄，就會一律進行需求即使`context.Fail()`已經在另一個處理常式中呼叫。</span><span class="sxs-lookup"><span data-stu-id="49d35-134">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="49d35-135">為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="49d35-135">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="49d35-136">在您想要評估的情況下**或**實作單一需求的多個處理常式的基礎。</span><span class="sxs-lookup"><span data-stu-id="49d35-136">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="49d35-137">例如，Microsoft 有門只開啟與索引鍵的卡片。</span><span class="sxs-lookup"><span data-stu-id="49d35-137">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="49d35-138">如果您在家離開您金鑰卡接線生列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="49d35-138">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="49d35-139">在此案例中，您會有一個需求， *EnterBuilding*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="49d35-139">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

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

<span data-ttu-id="49d35-140">現在，假設這兩個處理常式會[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)當原則評估`EnterBuildingRequirement`如果任一個處理常式成功原則評估會成功。</span><span class="sxs-lookup"><span data-stu-id="49d35-140">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="49d35-141">符合原則中使用函式</span><span class="sxs-lookup"><span data-stu-id="49d35-141">Using a func to fulfill a policy</span></span>

<span data-ttu-id="49d35-142">可能的情況下是簡單程式碼來表達履行原則。</span><span class="sxs-lookup"><span data-stu-id="49d35-142">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="49d35-143">很可能只需要提供`Func<AuthorizationHandlerContext, bool>`時設定與原則`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="49d35-143">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="49d35-144">例如先前`BadgeEntryHandler`可以改寫，如下所示。</span><span class="sxs-lookup"><span data-stu-id="49d35-144">For example the previous `BadgeEntryHandler` could be rewritten as follows;</span></span>

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

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="49d35-145">在處理常式中存取的 MVC 要求內容</span><span class="sxs-lookup"><span data-stu-id="49d35-145">Accessing MVC Request Context In Handlers</span></span>

<span data-ttu-id="49d35-146">`Handle`授權處理常式中，您必須實作的方法有兩個參數，`AuthorizationContext`和`Requirement`所處理。</span><span class="sxs-lookup"><span data-stu-id="49d35-146">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="49d35-147">任何將物件加入至可用的架構，例如 MVC 或 Jabbr`Resource`屬性`AuthorizationContext`通過額外的資訊。</span><span class="sxs-lookup"><span data-stu-id="49d35-147">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="49d35-148">MVC 的執行個體的傳遞，例如`Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext`資源屬性用來存取 HttpContext、 RouteData 和所有項目中提供其他 MVC。</span><span class="sxs-lookup"><span data-stu-id="49d35-148">For example MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="49d35-149">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="49d35-149">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="49d35-150">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="49d35-150">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="49d35-151">您應該轉換`Resource`屬性使用`as`關鍵字，然後檢查已轉型成功確定不會損毀您的程式碼與`InvalidCastExceptions`其他架構; 上執行時</span><span class="sxs-lookup"><span data-stu-id="49d35-151">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
