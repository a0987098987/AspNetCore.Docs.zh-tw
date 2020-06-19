---
title: ASP.NET Core 中以原則為基礎的授權
author: rick-anderson
description: 瞭解如何建立和使用授權原則處理常式，以在 ASP.NET Core 應用程式中強制執行授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authorization/policies
ms.openlocfilehash: 533bddc9c4499dad99cfdb3089045ea10aed4548
ms.sourcegitcommit: 4437f4c149f1ef6c28796dcfaa2863b4c088169c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85074153"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="d2f7e-103">ASP.NET Core 中以原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="d2f7e-103">Policy-based authorization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d2f7e-104">在幕後，以[角色為基礎的授權](xref:security/authorization/roles)和[宣告式授權](xref:security/authorization/claims)會使用需求、需求處理常式和預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="d2f7e-105">這些組建區塊支援程式碼中的授權評估運算式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="d2f7e-106">結果是更豐富、可重複使用、可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="d2f7e-107">授權原則是由一個或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="d2f7e-108">在方法中，它會註冊為授權服務設定的一部分 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

<span data-ttu-id="d2f7e-109">在上述範例中，會建立 "AtLeast21" 原則。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="d2f7e-110">它具有 &mdash; 最短使用期限的單一需求，這是以參數的形式提供給需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="d2f7e-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="d2f7e-111">IAuthorizationService</span></span> 

<span data-ttu-id="d2f7e-112">判斷授權是否成功的主要服務為 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="d2f7e-113">上述程式碼會反白顯示[IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)的兩個方法。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="d2f7e-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>是不含任何方法的標記服務，以及用來追蹤授權是否成功的機制。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="d2f7e-115">每個 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> 都會負責檢查是否符合需求：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="d2f7e-116">此 <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> 類別是處理常式用來標示是否符合需求的內容：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="d2f7e-117">下列程式碼顯示授權服務的簡化（並加上批註）預設的執行方式：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="d2f7e-118">下列程式碼顯示一般 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-118">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

<span data-ttu-id="d2f7e-119">使用 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> 或 `[Authorize(Policy = "Something")]` 進行授權。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="apply-policies-to-mvc-controllers"></a><span data-ttu-id="d2f7e-120">將原則套用至 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="d2f7e-120">Apply policies to MVC controllers</span></span>

<span data-ttu-id="d2f7e-121">如果您使用 Razor 的是頁面，請參閱本檔中的[將原則套用至 Razor 頁面](#apply-policies-to-razor-pages)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-121">If you're using Razor Pages, see [Apply policies to Razor Pages](#apply-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="d2f7e-122">原則會套用至控制器，方法是使用 `[Authorize]` 具有原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d2f7e-123">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="apply-policies-to-razor-pages"></a><span data-ttu-id="d2f7e-124">將原則套用至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d2f7e-124">Apply policies to Razor Pages</span></span>

<span data-ttu-id="d2f7e-125">原則會套用至 Razor 頁面，方法是使用 `[Authorize]` 具有原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d2f7e-126">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="d2f7e-127">您也可以 Razor 使用[授權慣例](xref:security/authorization/razor-pages-authorization)，將原則套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="d2f7e-128">需求</span><span class="sxs-lookup"><span data-stu-id="d2f7e-128">Requirements</span></span>

<span data-ttu-id="d2f7e-129">授權需求是一種資料參數集合，原則可以用來評估目前的使用者主體。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="d2f7e-130">在我們的「AtLeast21」原則中，需求是 &mdash; 最短存留期的單一參數。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="d2f7e-131">需求會執行[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的標記介面。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="d2f7e-132">參數化最短存留期需求的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="d2f7e-133">如果授權原則包含多個授權需求，則必須通過所有需求，原則評估才會成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="d2f7e-134">換句話說，新增至單一授權原則的多個授權需求會以一**和**為基礎來處理。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f7e-135">需求不需要有資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="d2f7e-136">授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d2f7e-136">Authorization handlers</span></span>

<span data-ttu-id="d2f7e-137">授權處理常式負責評估需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="d2f7e-138">授權處理常式會針對提供的[AuthorizationHandlerCoNtext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)評估需求，以判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="d2f7e-139">需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="d2f7e-140">處理常式可能會[繼承 \<TRequirement> AuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中 `TRequirement` 是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="d2f7e-141">或者，處理常式可能會執行[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="d2f7e-142">針對一個需求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d2f7e-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="d2f7e-143">以下是一對一關聯性的範例，其中最小存留期處理常式會利用單一需求：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="d2f7e-144">上述程式碼會判斷目前的使用者主體是否具有由已知且受信任簽發者所發行的「出生日期」。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="d2f7e-145">當宣告遺失時，就無法進行授權，在這種情況下會傳回已完成的工作。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="d2f7e-146">當宣告存在時，就會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="d2f7e-147">如果使用者符合需求所定義的最短存留期，則會將授權視為成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="d2f7e-148">當授權成功時， `context.Succeed` 會叫用滿足需求做為其唯一參數的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="d2f7e-149">針對多項需求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d2f7e-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="d2f7e-150">以下是一對多關聯性的範例，其中的許可權處理常式可處理三種不同類型的需求：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="d2f7e-151">上述程式碼會[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements) &mdash; 包含未標示為成功之需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="d2f7e-152">針對 `ReadPermission` 需求，使用者必須是「擁有者」或「贊助者」，才能存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="d2f7e-153">如果是 `EditPermission` 或 `DeletePermission` 需求，他或她必須是擁有者，才能存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="d2f7e-154">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="d2f7e-154">Handler registration</span></span>

<span data-ttu-id="d2f7e-155">在設定期間，會在服務集合中註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="d2f7e-156">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-156">For example:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

<span data-ttu-id="d2f7e-157">上述程式碼會藉由叫用將註冊 `MinimumAgeHandler` 為 singleton `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="d2f7e-158">您可以使用任何內建[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)來註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="d2f7e-159">處理常式會傳回什麼？</span><span class="sxs-lookup"><span data-stu-id="d2f7e-159">What should a handler return?</span></span>

<span data-ttu-id="d2f7e-160">請注意， `Handle` [處理常式範例](#security-authorization-handler-example)中的方法不會傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="d2f7e-161">如何指出成功或失敗的狀態？</span><span class="sxs-lookup"><span data-stu-id="d2f7e-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="d2f7e-162">處理常式會藉由呼叫來表示成功 `context.Succeed(IAuthorizationRequirement requirement)` ，傳遞已成功驗證的需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="d2f7e-163">處理常式不需要處理失敗的一般情況，因為相同需求的其他處理常式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="d2f7e-164">若要保證失敗，即使其他需求處理常式成功，也請呼叫 `context.Fail` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="d2f7e-165">如果處理常式呼叫 `context.Succeed` 或 `context.Fail` ，仍然會呼叫所有其他處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="d2f7e-166">這可讓需求產生副作用，例如記錄，即使另一個處理常式已成功驗證或失敗，也會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="d2f7e-167">當設定為時 `false` ， [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性（可在 ASP.NET Core 1.1 和更新版本中使用）會在呼叫時，縮短執行處理常式的時間 `context.Fail` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="d2f7e-168">`InvokeHandlersAfterFailure`預設為 `true` ，在此情況下會呼叫所有處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f7e-169">即使驗證失敗，也會呼叫授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="d2f7e-170">為什麼需要多個處理常式才能進行需求？</span><span class="sxs-lookup"><span data-stu-id="d2f7e-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="d2f7e-171">如果您想要以**或**為基礎進行評估，請針對單一需求執行多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="d2f7e-172">例如，Microsoft 有門只能以金鑰卡開啟。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="d2f7e-173">如果您在家中保留金鑰卡，則接待員會列印暫時的貼紙，並為您開啟大門。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="d2f7e-174">在此案例中，您會有單一需求、 *BuildingEntry*，但有多個處理常式，每一個都檢查單一需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="d2f7e-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="d2f7e-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="d2f7e-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d2f7e-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="d2f7e-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d2f7e-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="d2f7e-178">請確定這兩個處理常式都已[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="d2f7e-179">當原則評估時，如果任一處理程式成功 `BuildingEntryRequirement` ，原則評估就會成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="use-a-func-to-fulfill-a-policy"></a><span data-ttu-id="d2f7e-180">使用 func 來完成原則</span><span class="sxs-lookup"><span data-stu-id="d2f7e-180">Use a func to fulfill a policy</span></span>

<span data-ttu-id="d2f7e-181">在某些情況下，在程式碼中可以簡單地表達原則。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="d2f7e-182">當您使用原則產生器來設定原則時，可以提供 `Func<AuthorizationHandlerContext, bool>` `RequireAssertion` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="d2f7e-183">例如，先前的改寫方式如下所示 `BadgeEntryHandler` ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="access-mvc-request-context-in-handlers"></a><span data-ttu-id="d2f7e-184">存取處理常式中的 MVC 要求內容</span><span class="sxs-lookup"><span data-stu-id="d2f7e-184">Access MVC request context in handlers</span></span>

<span data-ttu-id="d2f7e-185">`HandleRequirementAsync`您在授權處理常式中執行的方法有兩個參數： `AuthorizationHandlerContext` 和 `TRequirement` 您正在處理的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="d2f7e-186">MVC 或等架構可 SignalR 自由地將任何物件加入至的 `Resource` 屬性， `AuthorizationHandlerContext` 以傳遞額外的資訊。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-186">Frameworks such as MVC or SignalR are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="d2f7e-187">使用端點路由時，授權通常會由授權中介軟體處理。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-187">When using endpoint routing, authorization is typically handled by the Authorization Middleware.</span></span> <span data-ttu-id="d2f7e-188">在此情況下， `Resource` 屬性是的實例 <xref:Microsoft.AspNetCore.Http.Endpoint> 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-188">In this case, the `Resource` property is an instance of <xref:Microsoft.AspNetCore.Http.Endpoint>.</span></span> <span data-ttu-id="d2f7e-189">端點可用來探查您要路由的基礎資源。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-189">The endpoint can be used to probe the underlying resource to which you're routing.</span></span> <span data-ttu-id="d2f7e-190">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-190">For example:</span></span>

```csharp
if (context.Resource is Endpoint endpoint)
{
   var actionDescriptor = endpoint.Metadata.GetMetadata<ControllerActionDescriptor>();
   ...
}
```

<span data-ttu-id="d2f7e-191">端點不會提供對目前的存取權 `HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-191">The endpoint doesn't provide access to the current `HttpContext`.</span></span> <span data-ttu-id="d2f7e-192">使用端點路由時，請使用 `IHttpContextAcessor` 來存取 `HttpContext` 授權處理常式內的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-192">When using endpoint routing, use `IHttpContextAcessor` to access `HttpContext` inside of an authorization handler.</span></span> <span data-ttu-id="d2f7e-193">如需詳細資訊，請參閱[從自訂群組件使用 HttpCoNtext](xref:fundamentals/httpcontext#use-httpcontext-from-custom-components)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-193">For more information, see [Use HttpContext from custom components](xref:fundamentals/httpcontext#use-httpcontext-from-custom-components).</span></span>

<span data-ttu-id="d2f7e-194">使用傳統路由，或在 MVC 的授權篩選準則中發生授權時，的值 `Resource` 就是 <xref:Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext> 實例。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-194">With traditional routing, or when authorization happens as part of MVC's authorization filter, the value of `Resource` is an <xref:Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext> instance.</span></span> <span data-ttu-id="d2f7e-195">這個屬性可讓您 `HttpContext` 存取 `RouteData` MVC 和頁面所提供的、和其他所有專案 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-195">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="d2f7e-196">屬性的使用 `Resource` 是架構特有的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-196">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="d2f7e-197">使用屬性中的資訊會將 `Resource` 您的授權原則限制為特定架構。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-197">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="d2f7e-198">您應該 `Resource` 使用關鍵字來轉換屬性 `is` ，然後確認轉換已成功，以確保您的程式碼在 `InvalidCastException` 其他架構上執行時不會損毀：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-198">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d2f7e-199">在幕後，以[角色為基礎的授權](xref:security/authorization/roles)和[宣告式授權](xref:security/authorization/claims)會使用需求、需求處理常式和預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-199">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="d2f7e-200">這些組建區塊支援程式碼中的授權評估運算式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-200">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="d2f7e-201">結果是更豐富、可重複使用、可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-201">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="d2f7e-202">授權原則是由一個或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-202">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="d2f7e-203">在方法中，它會註冊為授權服務設定的一部分 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-203">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="d2f7e-204">在上述範例中，會建立 "AtLeast21" 原則。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-204">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="d2f7e-205">它具有 &mdash; 最短使用期限的單一需求，這是以參數的形式提供給需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-205">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="d2f7e-206">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="d2f7e-206">IAuthorizationService</span></span> 

<span data-ttu-id="d2f7e-207">判斷授權是否成功的主要服務為 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-207">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

[!INCLUDE[request localized comments](~/includes/code-comments-loc.md)]

<span data-ttu-id="d2f7e-208">上述程式碼會反白顯示[IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)的兩個方法。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-208">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="d2f7e-209"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>是不含任何方法的標記服務，以及用來追蹤授權是否成功的機制。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-209"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="d2f7e-210">每個 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> 都會負責檢查是否符合需求：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-210">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="d2f7e-211">此 <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> 類別是處理常式用來標示是否符合需求的內容：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-211">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="d2f7e-212">下列程式碼顯示授權服務的簡化（並加上批註）預設的執行方式：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-212">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="d2f7e-213">下列程式碼顯示一般 `ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-213">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

<span data-ttu-id="d2f7e-214">使用 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> 或 `[Authorize(Policy = "Something")]` 進行授權。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-214">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="apply-policies-to-mvc-controllers"></a><span data-ttu-id="d2f7e-215">將原則套用至 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="d2f7e-215">Apply policies to MVC controllers</span></span>

<span data-ttu-id="d2f7e-216">如果您使用 Razor 的是頁面，請參閱本檔中的[將原則套用至 Razor 頁面](#apply-policies-to-razor-pages)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-216">If you're using Razor Pages, see [Apply policies to Razor Pages](#apply-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="d2f7e-217">原則會套用至控制器，方法是使用 `[Authorize]` 具有原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-217">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d2f7e-218">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-218">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="apply-policies-to-razor-pages"></a><span data-ttu-id="d2f7e-219">將原則套用至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d2f7e-219">Apply policies to Razor Pages</span></span>

<span data-ttu-id="d2f7e-220">原則會套用至 Razor 頁面，方法是使用 `[Authorize]` 具有原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-220">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d2f7e-221">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-221">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="d2f7e-222">您也可以 Razor 使用[授權慣例](xref:security/authorization/razor-pages-authorization)，將原則套用至頁面。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-222">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="d2f7e-223">需求</span><span class="sxs-lookup"><span data-stu-id="d2f7e-223">Requirements</span></span>

<span data-ttu-id="d2f7e-224">授權需求是一種資料參數集合，原則可以用來評估目前的使用者主體。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-224">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="d2f7e-225">在我們的「AtLeast21」原則中，需求是 &mdash; 最短存留期的單一參數。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-225">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="d2f7e-226">需求會執行[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的標記介面。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-226">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="d2f7e-227">參數化最短存留期需求的執行方式如下：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-227">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="d2f7e-228">如果授權原則包含多個授權需求，則必須通過所有需求，原則評估才會成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-228">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="d2f7e-229">換句話說，新增至單一授權原則的多個授權需求會以一**和**為基礎來處理。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-229">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f7e-230">需求不需要有資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-230">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="d2f7e-231">授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d2f7e-231">Authorization handlers</span></span>

<span data-ttu-id="d2f7e-232">授權處理常式負責評估需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-232">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="d2f7e-233">授權處理常式會針對提供的[AuthorizationHandlerCoNtext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)評估需求，以判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-233">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="d2f7e-234">需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-234">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="d2f7e-235">處理常式可能會[繼承 \<TRequirement> AuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中 `TRequirement` 是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-235">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="d2f7e-236">或者，處理常式可能會執行[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-236">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="d2f7e-237">針對一個需求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d2f7e-237">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="d2f7e-238">以下是一對一關聯性的範例，其中最小存留期處理常式會利用單一需求：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-238">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="d2f7e-239">上述程式碼會判斷目前的使用者主體是否具有由已知且受信任簽發者所發行的「出生日期」。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-239">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="d2f7e-240">當宣告遺失時，就無法進行授權，在這種情況下會傳回已完成的工作。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-240">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="d2f7e-241">當宣告存在時，就會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-241">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="d2f7e-242">如果使用者符合需求所定義的最短存留期，則會將授權視為成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-242">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="d2f7e-243">當授權成功時， `context.Succeed` 會叫用滿足需求做為其唯一參數的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-243">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="d2f7e-244">針對多項需求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d2f7e-244">Use a handler for multiple requirements</span></span>

<span data-ttu-id="d2f7e-245">以下是一對多關聯性的範例，其中的許可權處理常式可處理三種不同類型的需求：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-245">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="d2f7e-246">上述程式碼會[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements) &mdash; 包含未標示為成功之需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-246">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="d2f7e-247">針對 `ReadPermission` 需求，使用者必須是「擁有者」或「贊助者」，才能存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-247">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="d2f7e-248">如果是 `EditPermission` 或 `DeletePermission` 需求，他或她必須是擁有者，才能存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-248">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="d2f7e-249">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="d2f7e-249">Handler registration</span></span>

<span data-ttu-id="d2f7e-250">在設定期間，會在服務集合中註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-250">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="d2f7e-251">例如：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-251">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="d2f7e-252">上述程式碼會藉由叫用將註冊 `MinimumAgeHandler` 為 singleton `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-252">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="d2f7e-253">您可以使用任何內建[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)來註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-253">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="d2f7e-254">處理常式會傳回什麼？</span><span class="sxs-lookup"><span data-stu-id="d2f7e-254">What should a handler return?</span></span>

<span data-ttu-id="d2f7e-255">請注意， `Handle` [處理常式範例](#security-authorization-handler-example)中的方法不會傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-255">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="d2f7e-256">如何指出成功或失敗的狀態？</span><span class="sxs-lookup"><span data-stu-id="d2f7e-256">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="d2f7e-257">處理常式會藉由呼叫來表示成功 `context.Succeed(IAuthorizationRequirement requirement)` ，傳遞已成功驗證的需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-257">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="d2f7e-258">處理常式不需要處理失敗的一般情況，因為相同需求的其他處理常式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-258">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="d2f7e-259">若要保證失敗，即使其他需求處理常式成功，也請呼叫 `context.Fail` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-259">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="d2f7e-260">如果處理常式呼叫 `context.Succeed` 或 `context.Fail` ，仍然會呼叫所有其他處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-260">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="d2f7e-261">這可讓需求產生副作用，例如記錄，即使另一個處理常式已成功驗證或失敗，也會發生這種情況。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-261">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="d2f7e-262">當設定為時 `false` ， [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性（可在 ASP.NET Core 1.1 和更新版本中使用）會在呼叫時，縮短執行處理常式的時間 `context.Fail` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-262">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="d2f7e-263">`InvokeHandlersAfterFailure`預設為 `true` ，在此情況下會呼叫所有處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-263">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="d2f7e-264">即使驗證失敗，也會呼叫授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-264">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="d2f7e-265">為什麼需要多個處理常式才能進行需求？</span><span class="sxs-lookup"><span data-stu-id="d2f7e-265">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="d2f7e-266">如果您想要以**或**為基礎進行評估，請針對單一需求執行多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-266">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="d2f7e-267">例如，Microsoft 有門只能以金鑰卡開啟。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-267">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="d2f7e-268">如果您在家中保留金鑰卡，則接待員會列印暫時的貼紙，並為您開啟大門。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-268">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="d2f7e-269">在此案例中，您會有單一需求、 *BuildingEntry*，但有多個處理常式，每一個都檢查單一需求。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-269">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="d2f7e-270">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="d2f7e-270">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="d2f7e-271">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d2f7e-271">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="d2f7e-272">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d2f7e-272">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="d2f7e-273">請確定這兩個處理常式都已[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-273">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="d2f7e-274">當原則評估時，如果任一處理程式成功 `BuildingEntryRequirement` ，原則評估就會成功。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-274">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="use-a-func-to-fulfill-a-policy"></a><span data-ttu-id="d2f7e-275">使用 func 來完成原則</span><span class="sxs-lookup"><span data-stu-id="d2f7e-275">Use a func to fulfill a policy</span></span>

<span data-ttu-id="d2f7e-276">在某些情況下，在程式碼中可以簡單地表達原則。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-276">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="d2f7e-277">當您使用原則產生器來設定原則時，可以提供 `Func<AuthorizationHandlerContext, bool>` `RequireAssertion` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-277">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="d2f7e-278">例如，先前的改寫方式如下所示 `BadgeEntryHandler` ：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-278">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="access-mvc-request-context-in-handlers"></a><span data-ttu-id="d2f7e-279">存取處理常式中的 MVC 要求內容</span><span class="sxs-lookup"><span data-stu-id="d2f7e-279">Access MVC request context in handlers</span></span>

<span data-ttu-id="d2f7e-280">`HandleRequirementAsync`您在授權處理常式中執行的方法有兩個參數： `AuthorizationHandlerContext` 和 `TRequirement` 您正在處理的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-280">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="d2f7e-281">MVC 或等架構可 SignalR 自由地將任何物件加入至的 `Resource` 屬性， `AuthorizationHandlerContext` 以傳遞額外的資訊。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-281">Frameworks such as MVC or SignalR are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="d2f7e-282">例如，MVC 會在屬性中傳遞[AuthorizationFilterCoNtext](/dotnet/api/?term=AuthorizationFilterContext)的實例 `Resource` 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-282">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="d2f7e-283">這個屬性可讓您 `HttpContext` 存取 `RouteData` MVC 和頁面所提供的、和其他所有專案 Razor 。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-283">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="d2f7e-284">屬性的使用 `Resource` 是架構特有的。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-284">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="d2f7e-285">使用屬性中的資訊會將 `Resource` 您的授權原則限制為特定架構。</span><span class="sxs-lookup"><span data-stu-id="d2f7e-285">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="d2f7e-286">您應該 `Resource` 使用關鍵字來轉換屬性 `is` ，然後確認轉換已成功，以確保您的程式碼在 `InvalidCastException` 其他架構上執行時不會損毀：</span><span class="sxs-lookup"><span data-stu-id="d2f7e-286">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end
