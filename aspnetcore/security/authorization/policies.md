---
title: ASP.NET核心中的基於策略的授權
author: rick-anderson
description: 瞭解如何創建和使用授權策略處理程式,以在ASP.NET酷應用中強制實施授權要求。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2020
uid: security/authorization/policies
ms.openlocfilehash: 66412a6020ea8f12427c8c5b466e1b2eedccf0f9
ms.sourcegitcommit: 77c046331f3d633d7cc247ba77e58b89e254f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488757"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="d8545-103">ASP.NET核心中的基於策略的授權</span><span class="sxs-lookup"><span data-stu-id="d8545-103">Policy-based authorization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d8545-104">在封面下方,[基於角色的授權](xref:security/authorization/roles)和[基於聲明的授權](xref:security/authorization/claims)使用要求、需求處理程式和預配置的策略。</span><span class="sxs-lookup"><span data-stu-id="d8545-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="d8545-105">這些構建基塊支援代碼中授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="d8545-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="d8545-106">結果是更豐富、可重用、可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="d8545-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="d8545-107">授權策略由一個或多個要求組成。</span><span class="sxs-lookup"><span data-stu-id="d8545-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="d8545-108">它在 方法中註冊為授權服務配置的`Startup.ConfigureServices`一 部分:</span><span class="sxs-lookup"><span data-stu-id="d8545-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

<span data-ttu-id="d8545-109">在前面的示例中,將創建一個"AtLeast21"策略。</span><span class="sxs-lookup"><span data-stu-id="d8545-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="d8545-110">它有一個最低年齡&mdash;的單一要求,作為要求的參數提供。</span><span class="sxs-lookup"><span data-stu-id="d8545-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="d8545-111">I 授權服務</span><span class="sxs-lookup"><span data-stu-id="d8545-111">IAuthorizationService</span></span> 

<span data-ttu-id="d8545-112">確定授權是否成功的主要服務是<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="d8545-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="d8545-113">前面的代碼突出顯示了[I 授權服務的](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)兩種方法。</span><span class="sxs-lookup"><span data-stu-id="d8545-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="d8545-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>是一個沒有方法的標記服務,是跟蹤授權是否成功的機制。</span><span class="sxs-lookup"><span data-stu-id="d8545-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="d8545-115">每個<xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler>公司負責檢查是否滿足要求:</span><span class="sxs-lookup"><span data-stu-id="d8545-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
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

<span data-ttu-id="d8545-116">類別<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext>是處理程式用於標記是否滿足要求的內容:</span><span class="sxs-lookup"><span data-stu-id="d8545-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="d8545-117">以下代碼顯示授權服務的簡化(並帶有註解)預設實現:</span><span class="sxs-lookup"><span data-stu-id="d8545-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="d8545-118">以下代碼顯示了典型的`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d8545-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="d8545-119">使用<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>或`[Authorize(Policy = "Something")]`授權。</span><span class="sxs-lookup"><span data-stu-id="d8545-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="d8545-120">將策略應用於 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="d8545-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="d8545-121">如果您使用的是 Razor 頁面,請參考文件中[將策略套用 Razor 頁面](#applying-policies-to-razor-pages)。</span><span class="sxs-lookup"><span data-stu-id="d8545-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="d8545-122">策略通過使用具有策略名稱`[Authorize]`的屬性應用於控制器。</span><span class="sxs-lookup"><span data-stu-id="d8545-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d8545-123">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="d8545-124">將策略應用於剃刀頁面</span><span class="sxs-lookup"><span data-stu-id="d8545-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="d8545-125">策略通過使用具有策略名稱`[Authorize]`的屬性應用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="d8545-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d8545-126">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="d8545-127">策略也可以通過使用[授權約定](xref:security/authorization/razor-pages-authorization)應用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="d8545-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="d8545-128">需求</span><span class="sxs-lookup"><span data-stu-id="d8545-128">Requirements</span></span>

<span data-ttu-id="d8545-129">授權要求是策略可用於評估當前用戶主體的數據參數的集合。</span><span class="sxs-lookup"><span data-stu-id="d8545-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="d8545-130">在我們的「AtLeast21」政策中,要求是最小年齡的單個&mdash;參數。</span><span class="sxs-lookup"><span data-stu-id="d8545-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="d8545-131">要求實現[I 授權要求](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement),這是一個空標記介面。</span><span class="sxs-lookup"><span data-stu-id="d8545-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="d8545-132">參數化的最低年齡要求可執行如下:</span><span class="sxs-lookup"><span data-stu-id="d8545-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="d8545-133">如果授權策略包含多個授權要求,則必須通過所有要求才能成功策略評估。</span><span class="sxs-lookup"><span data-stu-id="d8545-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="d8545-134">換句話說,添加到單個授權策略中的多個授權要求將基於**AND**處理。</span><span class="sxs-lookup"><span data-stu-id="d8545-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="d8545-135">要求不需要具有數據或屬性。</span><span class="sxs-lookup"><span data-stu-id="d8545-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="d8545-136">授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d8545-136">Authorization handlers</span></span>

<span data-ttu-id="d8545-137">授權處理程序負責評估需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8545-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="d8545-138">授權處理程式根據提供的[授權處理程式計算](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)需求,以確定是否允許訪問。</span><span class="sxs-lookup"><span data-stu-id="d8545-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="d8545-139">要求可以[有多個處理程式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="d8545-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="d8545-140">處理程式可以繼承[授權處理\<程式 t要求>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1),其中`TRequirement`需要處理。</span><span class="sxs-lookup"><span data-stu-id="d8545-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="d8545-141">或者,處理程式可以實現[I授權處理程式](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的要求。</span><span class="sxs-lookup"><span data-stu-id="d8545-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="d8545-142">對一個要求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d8545-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="d8545-143">下面是一對一關係的示例,其中最小年齡處理程式使用單個要求:</span><span class="sxs-lookup"><span data-stu-id="d8545-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="d8545-144">前面的代碼確定當前用戶主體是否具有由已知和受信任的頒發者頒發的出生日期聲明。</span><span class="sxs-lookup"><span data-stu-id="d8545-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="d8545-145">當缺少聲明時,無法發生授權,在這種情況下,將返回已完成的任務。</span><span class="sxs-lookup"><span data-stu-id="d8545-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="d8545-146">當聲明存在時,將計算用戶的年齡。</span><span class="sxs-lookup"><span data-stu-id="d8545-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="d8545-147">如果用戶滿足要求定義的最低年齡,則授權被視為成功。</span><span class="sxs-lookup"><span data-stu-id="d8545-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="d8545-148">當授權成功時,`context.Succeed`將調用滿足的要求作為其唯一參數。</span><span class="sxs-lookup"><span data-stu-id="d8545-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="d8545-149">對多個要求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d8545-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="d8545-150">下面是一對多關係的範例,其中許可權處理程式可以處理三種不同類型的要求:</span><span class="sxs-lookup"><span data-stu-id="d8545-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="d8545-151">前面的代碼遍歷[Pending要求](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;包含未標記為成功的要求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8545-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="d8545-152">對於要求`ReadPermission`,使用者必須是擁有者或贊助者才能訪問請求的資源。</span><span class="sxs-lookup"><span data-stu-id="d8545-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="d8545-153">如果是`EditPermission``DeletePermission`或要求,他或她必須是訪問請求的資源的擁有者。</span><span class="sxs-lookup"><span data-stu-id="d8545-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="d8545-154">處理程式註冊</span><span class="sxs-lookup"><span data-stu-id="d8545-154">Handler registration</span></span>

<span data-ttu-id="d8545-155">在配置期間,處理程式在服務集合中註冊。</span><span class="sxs-lookup"><span data-stu-id="d8545-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="d8545-156">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-156">For example:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

<span data-ttu-id="d8545-157">前面的代碼通過調用`MinimumAgeHandler``services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`註冊為單例。</span><span class="sxs-lookup"><span data-stu-id="d8545-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="d8545-158">處理程式可以使用任何內置[服務存留期進行](xref:fundamentals/dependency-injection#service-lifetimes)註冊。</span><span class="sxs-lookup"><span data-stu-id="d8545-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="d8545-159">處理程式應返回什麼?</span><span class="sxs-lookup"><span data-stu-id="d8545-159">What should a handler return?</span></span>

<span data-ttu-id="d8545-160">請注意,`Handle`[處理程式範例中](#security-authorization-handler-example)的方法不返回任何值。</span><span class="sxs-lookup"><span data-stu-id="d8545-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="d8545-161">如何指示成功或失敗的狀態?</span><span class="sxs-lookup"><span data-stu-id="d8545-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="d8545-162">處理程序通過調用`context.Succeed(IAuthorizationRequirement requirement)`來表示成功,傳遞已成功驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="d8545-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="d8545-163">處理程式通常不需要處理故障,因為相同要求的其他處理程式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="d8545-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="d8545-164">為了保證失敗,即使其他需求處理程式成功,請呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="d8545-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="d8545-165">如果處理程式調用`context.Succeed``context.Fail`或 ,仍調用所有其他處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="d8545-166">這允許要求產生副作用,如日誌記錄,即使另一個處理程式已成功驗證或未通過要求也是如此。</span><span class="sxs-lookup"><span data-stu-id="d8545-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="d8545-167">當設置為`false`[時,InvokeHandlersAfter 失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性(在 ASP.NET Core 1.1 和更高版本`context.Fail`中可用)在調用時 短路處理程式的執行。</span><span class="sxs-lookup"><span data-stu-id="d8545-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="d8545-168">`InvokeHandlersAfterFailure`默認值為`true`,在這種情況下,將調用所有處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="d8545-169">即使身份驗證失敗,也會調用授權處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="d8545-170">為什麼需要多個處理程序來滿足需求?</span><span class="sxs-lookup"><span data-stu-id="d8545-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="d8545-171">如果希望評估基於**OR,** 則為單個要求實現多個處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="d8545-172">例如,微軟的大門只打開鑰匙卡。</span><span class="sxs-lookup"><span data-stu-id="d8545-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="d8545-173">如果您將鑰匙卡留在家中,接待員會列印臨時貼紙,併為您開門。</span><span class="sxs-lookup"><span data-stu-id="d8545-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="d8545-174">在這種情況下,您將有一個要求,*即"構建入口*",但有多個處理程式,每個處理程式都檢查單個要求。</span><span class="sxs-lookup"><span data-stu-id="d8545-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="d8545-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="d8545-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="d8545-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d8545-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="d8545-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d8545-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="d8545-178">確保兩個處理程式都[已註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="d8545-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="d8545-179">如果任一處理程式在策略計算 時`BuildingEntryRequirement`成功 ,則策略計算將成功。</span><span class="sxs-lookup"><span data-stu-id="d8545-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="d8545-180">使用 func 實作原則</span><span class="sxs-lookup"><span data-stu-id="d8545-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="d8545-181">在某些情況下,實現策略在代碼中很容易表達。</span><span class="sxs-lookup"><span data-stu-id="d8545-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="d8545-182">在與策略產生器設定策略時,`Func<AuthorizationHandlerContext, bool>``RequireAssertion`可以提供 .</span><span class="sxs-lookup"><span data-stu-id="d8545-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="d8545-183">例如,可以重寫前`BadgeEntryHandler`一個,如下所示:</span><span class="sxs-lookup"><span data-stu-id="d8545-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="d8545-184">在處理程式中存取 MVC 要求上下文</span><span class="sxs-lookup"><span data-stu-id="d8545-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="d8545-185">在`HandleRequirementAsync`授權處理程式中實現的方法有兩個參數:和`AuthorizationHandlerContext``TRequirement`正在處理的參數。</span><span class="sxs-lookup"><span data-stu-id="d8545-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="d8545-186">MVC 或 Jabbr 等框架`Resource`可免費向 的`AuthorizationHandlerContext`屬性添加任何物件 以傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="d8545-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="d8545-187">例如,MVC 在屬性中傳遞[授權篩選器上下文](/dotnet/api/?term=AuthorizationFilterContext)的`Resource`實例。</span><span class="sxs-lookup"><span data-stu-id="d8545-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="d8545-188">此屬性提供對`HttpContext``RouteData`的訪問許可權,以及 MVC 和 Razor 頁面提供的所有內容。</span><span class="sxs-lookup"><span data-stu-id="d8545-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="d8545-189">`Resource`屬性的使用是特定於框架的。</span><span class="sxs-lookup"><span data-stu-id="d8545-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="d8545-190">使用 屬性`Resource`中的資訊會將授權策略限制為特定框架。</span><span class="sxs-lookup"><span data-stu-id="d8545-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="d8545-191">應使用`Resource``is`關鍵字強制轉換屬性,然後確認強制轉換已成功確保代碼在其他框架上執行時不會崩潰與`InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="d8545-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

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

<span data-ttu-id="d8545-192">在封面下方,[基於角色的授權](xref:security/authorization/roles)和[基於聲明的授權](xref:security/authorization/claims)使用要求、需求處理程式和預配置的策略。</span><span class="sxs-lookup"><span data-stu-id="d8545-192">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="d8545-193">這些構建基塊支援代碼中授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="d8545-193">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="d8545-194">結果是更豐富、可重用、可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="d8545-194">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="d8545-195">授權策略由一個或多個要求組成。</span><span class="sxs-lookup"><span data-stu-id="d8545-195">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="d8545-196">它在 方法中註冊為授權服務配置的`Startup.ConfigureServices`一 部分:</span><span class="sxs-lookup"><span data-stu-id="d8545-196">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="d8545-197">在前面的示例中,將創建一個"AtLeast21"策略。</span><span class="sxs-lookup"><span data-stu-id="d8545-197">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="d8545-198">它有一個最低年齡&mdash;的單一要求,作為要求的參數提供。</span><span class="sxs-lookup"><span data-stu-id="d8545-198">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="d8545-199">I 授權服務</span><span class="sxs-lookup"><span data-stu-id="d8545-199">IAuthorizationService</span></span> 

<span data-ttu-id="d8545-200">確定授權是否成功的主要服務是<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="d8545-200">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="d8545-201">前面的代碼突出顯示了[I 授權服務的](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)兩種方法。</span><span class="sxs-lookup"><span data-stu-id="d8545-201">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="d8545-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>是一個沒有方法的標記服務,是跟蹤授權是否成功的機制。</span><span class="sxs-lookup"><span data-stu-id="d8545-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="d8545-203">每個<xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler>公司負責檢查是否滿足要求:</span><span class="sxs-lookup"><span data-stu-id="d8545-203">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
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

<span data-ttu-id="d8545-204">類別<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext>是處理程式用於標記是否滿足要求的內容:</span><span class="sxs-lookup"><span data-stu-id="d8545-204">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="d8545-205">以下代碼顯示授權服務的簡化(並帶有註解)預設實現:</span><span class="sxs-lookup"><span data-stu-id="d8545-205">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="d8545-206">以下代碼顯示了典型的`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d8545-206">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="d8545-207">使用<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>或`[Authorize(Policy = "Something")]`授權。</span><span class="sxs-lookup"><span data-stu-id="d8545-207">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="d8545-208">將策略應用於 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="d8545-208">Applying policies to MVC controllers</span></span>

<span data-ttu-id="d8545-209">如果您使用的是 Razor 頁面,請參考文件中[將策略套用 Razor 頁面](#applying-policies-to-razor-pages)。</span><span class="sxs-lookup"><span data-stu-id="d8545-209">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="d8545-210">策略通過使用具有策略名稱`[Authorize]`的屬性應用於控制器。</span><span class="sxs-lookup"><span data-stu-id="d8545-210">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d8545-211">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-211">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="d8545-212">將策略應用於剃刀頁面</span><span class="sxs-lookup"><span data-stu-id="d8545-212">Applying policies to Razor Pages</span></span>

<span data-ttu-id="d8545-213">策略通過使用具有策略名稱`[Authorize]`的屬性應用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="d8545-213">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d8545-214">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-214">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="d8545-215">策略也可以通過使用[授權約定](xref:security/authorization/razor-pages-authorization)應用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="d8545-215">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="d8545-216">需求</span><span class="sxs-lookup"><span data-stu-id="d8545-216">Requirements</span></span>

<span data-ttu-id="d8545-217">授權要求是策略可用於評估當前用戶主體的數據參數的集合。</span><span class="sxs-lookup"><span data-stu-id="d8545-217">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="d8545-218">在我們的「AtLeast21」政策中,要求是最小年齡的單個&mdash;參數。</span><span class="sxs-lookup"><span data-stu-id="d8545-218">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="d8545-219">要求實現[I 授權要求](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement),這是一個空標記介面。</span><span class="sxs-lookup"><span data-stu-id="d8545-219">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="d8545-220">參數化的最低年齡要求可執行如下:</span><span class="sxs-lookup"><span data-stu-id="d8545-220">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="d8545-221">如果授權策略包含多個授權要求,則必須通過所有要求才能成功策略評估。</span><span class="sxs-lookup"><span data-stu-id="d8545-221">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="d8545-222">換句話說,添加到單個授權策略中的多個授權要求將基於**AND**處理。</span><span class="sxs-lookup"><span data-stu-id="d8545-222">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="d8545-223">要求不需要具有數據或屬性。</span><span class="sxs-lookup"><span data-stu-id="d8545-223">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="d8545-224">授權處理常式</span><span class="sxs-lookup"><span data-stu-id="d8545-224">Authorization handlers</span></span>

<span data-ttu-id="d8545-225">授權處理程序負責評估需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8545-225">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="d8545-226">授權處理程式根據提供的[授權處理程式計算](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)需求,以確定是否允許訪問。</span><span class="sxs-lookup"><span data-stu-id="d8545-226">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="d8545-227">要求可以[有多個處理程式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="d8545-227">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="d8545-228">處理程式可以繼承[授權處理\<程式 t要求>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1),其中`TRequirement`需要處理。</span><span class="sxs-lookup"><span data-stu-id="d8545-228">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="d8545-229">或者,處理程式可以實現[I授權處理程式](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的要求。</span><span class="sxs-lookup"><span data-stu-id="d8545-229">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="d8545-230">對一個要求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d8545-230">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="d8545-231">下面是一對一關係的示例,其中最小年齡處理程式使用單個要求:</span><span class="sxs-lookup"><span data-stu-id="d8545-231">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="d8545-232">前面的代碼確定當前用戶主體是否具有由已知和受信任的頒發者頒發的出生日期聲明。</span><span class="sxs-lookup"><span data-stu-id="d8545-232">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="d8545-233">當缺少聲明時,無法發生授權,在這種情況下,將返回已完成的任務。</span><span class="sxs-lookup"><span data-stu-id="d8545-233">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="d8545-234">當聲明存在時,將計算用戶的年齡。</span><span class="sxs-lookup"><span data-stu-id="d8545-234">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="d8545-235">如果用戶滿足要求定義的最低年齡,則授權被視為成功。</span><span class="sxs-lookup"><span data-stu-id="d8545-235">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="d8545-236">當授權成功時,`context.Succeed`將調用滿足的要求作為其唯一參數。</span><span class="sxs-lookup"><span data-stu-id="d8545-236">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="d8545-237">對多個要求使用處理程式</span><span class="sxs-lookup"><span data-stu-id="d8545-237">Use a handler for multiple requirements</span></span>

<span data-ttu-id="d8545-238">下面是一對多關係的範例,其中許可權處理程式可以處理三種不同類型的要求:</span><span class="sxs-lookup"><span data-stu-id="d8545-238">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="d8545-239">前面的代碼遍歷[Pending要求](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;包含未標記為成功的要求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8545-239">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="d8545-240">對於要求`ReadPermission`,使用者必須是擁有者或贊助者才能訪問請求的資源。</span><span class="sxs-lookup"><span data-stu-id="d8545-240">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="d8545-241">如果是`EditPermission``DeletePermission`或要求,他或她必須是訪問請求的資源的擁有者。</span><span class="sxs-lookup"><span data-stu-id="d8545-241">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="d8545-242">處理程式註冊</span><span class="sxs-lookup"><span data-stu-id="d8545-242">Handler registration</span></span>

<span data-ttu-id="d8545-243">在配置期間,處理程式在服務集合中註冊。</span><span class="sxs-lookup"><span data-stu-id="d8545-243">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="d8545-244">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-244">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="d8545-245">前面的代碼通過調用`MinimumAgeHandler``services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`註冊為單例。</span><span class="sxs-lookup"><span data-stu-id="d8545-245">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="d8545-246">處理程式可以使用任何內置[服務存留期進行](xref:fundamentals/dependency-injection#service-lifetimes)註冊。</span><span class="sxs-lookup"><span data-stu-id="d8545-246">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="d8545-247">處理程式應返回什麼?</span><span class="sxs-lookup"><span data-stu-id="d8545-247">What should a handler return?</span></span>

<span data-ttu-id="d8545-248">請注意,`Handle`[處理程式範例中](#security-authorization-handler-example)的方法不返回任何值。</span><span class="sxs-lookup"><span data-stu-id="d8545-248">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="d8545-249">如何指示成功或失敗的狀態?</span><span class="sxs-lookup"><span data-stu-id="d8545-249">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="d8545-250">處理程序通過調用`context.Succeed(IAuthorizationRequirement requirement)`來表示成功,傳遞已成功驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="d8545-250">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="d8545-251">處理程式通常不需要處理故障,因為相同要求的其他處理程式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="d8545-251">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="d8545-252">為了保證失敗,即使其他需求處理程式成功,請呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="d8545-252">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="d8545-253">如果處理程式調用`context.Succeed``context.Fail`或 ,仍調用所有其他處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-253">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="d8545-254">這允許要求產生副作用,如日誌記錄,即使另一個處理程式已成功驗證或未通過要求也是如此。</span><span class="sxs-lookup"><span data-stu-id="d8545-254">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="d8545-255">當設置為`false`[時,InvokeHandlersAfter 失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性(在 ASP.NET Core 1.1 和更高版本`context.Fail`中可用)在調用時 短路處理程式的執行。</span><span class="sxs-lookup"><span data-stu-id="d8545-255">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="d8545-256">`InvokeHandlersAfterFailure`默認值為`true`,在這種情況下,將調用所有處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-256">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="d8545-257">即使身份驗證失敗,也會調用授權處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-257">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="d8545-258">為什麼需要多個處理程序來滿足需求?</span><span class="sxs-lookup"><span data-stu-id="d8545-258">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="d8545-259">如果希望評估基於**OR,** 則為單個要求實現多個處理程式。</span><span class="sxs-lookup"><span data-stu-id="d8545-259">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="d8545-260">例如,微軟的大門只打開鑰匙卡。</span><span class="sxs-lookup"><span data-stu-id="d8545-260">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="d8545-261">如果您將鑰匙卡留在家中,接待員會列印臨時貼紙,併為您開門。</span><span class="sxs-lookup"><span data-stu-id="d8545-261">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="d8545-262">在這種情況下,您將有一個要求,*即"構建入口*",但有多個處理程式,每個處理程式都檢查單個要求。</span><span class="sxs-lookup"><span data-stu-id="d8545-262">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="d8545-263">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="d8545-263">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="d8545-264">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d8545-264">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="d8545-265">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d8545-265">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="d8545-266">確保兩個處理程式都[已註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="d8545-266">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="d8545-267">如果任一處理程式在策略計算 時`BuildingEntryRequirement`成功 ,則策略計算將成功。</span><span class="sxs-lookup"><span data-stu-id="d8545-267">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="d8545-268">使用 func 實作原則</span><span class="sxs-lookup"><span data-stu-id="d8545-268">Using a func to fulfill a policy</span></span>

<span data-ttu-id="d8545-269">在某些情況下,實現策略在代碼中很容易表達。</span><span class="sxs-lookup"><span data-stu-id="d8545-269">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="d8545-270">在與策略產生器設定策略時,`Func<AuthorizationHandlerContext, bool>``RequireAssertion`可以提供 .</span><span class="sxs-lookup"><span data-stu-id="d8545-270">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="d8545-271">例如,可以重寫前`BadgeEntryHandler`一個,如下所示:</span><span class="sxs-lookup"><span data-stu-id="d8545-271">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="d8545-272">在處理程式中存取 MVC 要求上下文</span><span class="sxs-lookup"><span data-stu-id="d8545-272">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="d8545-273">在`HandleRequirementAsync`授權處理程式中實現的方法有兩個參數:和`AuthorizationHandlerContext``TRequirement`正在處理的參數。</span><span class="sxs-lookup"><span data-stu-id="d8545-273">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="d8545-274">MVC 或 SignalR 等框架`Resource`可免費向 的`AuthorizationHandlerContext`屬性添加任何物件 以傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="d8545-274">Frameworks such as MVC or SignalR are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="d8545-275">使用終結點路由時,授權通常由授權中間件處理。</span><span class="sxs-lookup"><span data-stu-id="d8545-275">When using endpoint routing, authorization is typically handled by the Authorization Middleware.</span></span> <span data-ttu-id="d8545-276">在這種情況下,`Resource`屬性是<xref:Microsoft.AspNetCore.Http.Endpoint>的實體。</span><span class="sxs-lookup"><span data-stu-id="d8545-276">In this case, the `Resource` property is an instance of <xref:Microsoft.AspNetCore.Http.Endpoint>.</span></span> <span data-ttu-id="d8545-277">終結點可用於探測要路由到的資源的基礎。</span><span class="sxs-lookup"><span data-stu-id="d8545-277">The endpoint can be used to probe the underlying the resource to which you're routing.</span></span> <span data-ttu-id="d8545-278">例如：</span><span class="sxs-lookup"><span data-stu-id="d8545-278">For example:</span></span>

```csharp
if (context.Resource is Endpoint endpoint)
{
   var actionDescriptor = endpoint.Metadata.GetMetadata<ControllerActionDescriptor>();
   ...
}
```

<span data-ttu-id="d8545-279">對於傳統路由,或者當授權作為MVC授權篩選器的一部分發生時,值`Resource`是實例。 <xref:Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext></span><span class="sxs-lookup"><span data-stu-id="d8545-279">With traditional routing, or when authorization happens as part of MVC's authorization filter, the value of `Resource` is an <xref:Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext> instance.</span></span> <span data-ttu-id="d8545-280">此屬性提供對`HttpContext``RouteData`的訪問許可權,以及 MVC 和 Razor 頁面提供的所有內容。</span><span class="sxs-lookup"><span data-stu-id="d8545-280">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="d8545-281">`Resource`屬性的使用是特定於框架的。</span><span class="sxs-lookup"><span data-stu-id="d8545-281">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="d8545-282">使用 屬性`Resource`中的資訊會將授權策略限制為特定框架。</span><span class="sxs-lookup"><span data-stu-id="d8545-282">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="d8545-283">應使用`Resource``is`關鍵字強制轉換屬性,然後確認強制轉換已成功確保代碼在其他框架上執行時不會崩潰與`InvalidCastException`。</span><span class="sxs-lookup"><span data-stu-id="d8545-283">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end
