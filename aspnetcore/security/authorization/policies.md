---
title: ASP.NET Core 中的原則為基礎的授權
author: rick-anderson
description: 了解如何建立和使用授權原則的處理常式，以強制執行的 ASP.NET Core 應用程式中的授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 67337c847ba71df3fe61250996ec944632ad5d57
ms.sourcegitcommit: 1bb3f3f1905b4e7d4ca1b314f2ce6ee5dd8be75f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/11/2019
ms.locfileid: "66837346"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="c8306-103">ASP.NET Core 中的原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="c8306-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c8306-104">基本上，[角色為基礎的授權](xref:security/authorization/roles)並[宣告型授權](xref:security/authorization/claims)使用需求，需求處理常式，並預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="c8306-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="c8306-105">這些建置組塊支援程式碼中的授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="c8306-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="c8306-106">結果會是更豐富、 可重複使用、 可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="c8306-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="c8306-107">授權原則是由一或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="c8306-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="c8306-108">它會註冊為授權服務的組態，在`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="c8306-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="c8306-109">在上述範例中，會建立 「 AtLeast21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="c8306-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="c8306-110">它有一個需求&mdash;的最短使用期限，做為參數的需求，提供。</span><span class="sxs-lookup"><span data-stu-id="c8306-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="c8306-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="c8306-111">IAuthorizationService</span></span> 

<span data-ttu-id="c8306-112">在判斷授權是否成功的主要服務<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="c8306-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="c8306-113">上述程式碼會反白顯示兩種[IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)。</span><span class="sxs-lookup"><span data-stu-id="c8306-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="c8306-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> 是標記服務有沒有任何方法，以及追蹤授權是否成功的機制。</span><span class="sxs-lookup"><span data-stu-id="c8306-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="c8306-115">每個<xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler>負責檢查是否符合需求：</span><span class="sxs-lookup"><span data-stu-id="c8306-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

<span data-ttu-id="c8306-116"><xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext>類別是處理常式使用標記是否已符合需求：</span><span class="sxs-lookup"><span data-stu-id="c8306-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="c8306-117">下列程式碼顯示的簡化 （和註解標註） 預設實作授權服務：</span><span class="sxs-lookup"><span data-stu-id="c8306-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="c8306-118">下列程式碼示範典型`ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="c8306-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="c8306-119">使用<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>或`[Authorize(Policy = "Something"]`進行授權。</span><span class="sxs-lookup"><span data-stu-id="c8306-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something"]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="c8306-120">將原則套用至 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="c8306-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="c8306-121">如果您使用 Razor 頁面，請參閱[將原則套用至 Razor Pages](#applying-policies-to-razor-pages)本文件中。</span><span class="sxs-lookup"><span data-stu-id="c8306-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="c8306-122">原則會套用至控制器使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="c8306-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c8306-123">例如:</span><span class="sxs-lookup"><span data-stu-id="c8306-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="c8306-124">將原則套用至 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="c8306-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="c8306-125">原則時，會套用至 Razor 頁面上，使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="c8306-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c8306-126">例如:</span><span class="sxs-lookup"><span data-stu-id="c8306-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="c8306-127">原則也可以套用至 Razor Pages 使用[授權慣例](xref:security/authorization/razor-pages-authorization)。</span><span class="sxs-lookup"><span data-stu-id="c8306-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="c8306-128">需求</span><span class="sxs-lookup"><span data-stu-id="c8306-128">Requirements</span></span>

<span data-ttu-id="c8306-129">授權需求是原則可用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="c8306-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="c8306-130">在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="c8306-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="c8306-131">需求會實作[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的 marker 介面。</span><span class="sxs-lookup"><span data-stu-id="c8306-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="c8306-132">無法實作參數化的最短使用期限需求如下所示：</span><span class="sxs-lookup"><span data-stu-id="c8306-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="c8306-133">如果授權原則包含多個授權需求，所有的需求必須傳遞成功原則評估的順序。</span><span class="sxs-lookup"><span data-stu-id="c8306-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="c8306-134">換句話說，在處理新增至單一授權原則的多個授權需求**AND**為基礎。</span><span class="sxs-lookup"><span data-stu-id="c8306-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="c8306-135">要求不需要有資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="c8306-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="c8306-136">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c8306-136">Authorization handlers</span></span>

<span data-ttu-id="c8306-137">授權的處理常式會負責需求之屬性的評估。</span><span class="sxs-lookup"><span data-stu-id="c8306-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="c8306-138">授權的處理常式會評估針對所提供的需求[AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)來判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="c8306-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="c8306-139">需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="c8306-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="c8306-140">處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中`TRequirement`是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="c8306-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="c8306-141">或者，可能會實作處理常式[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多個類型的需求。</span><span class="sxs-lookup"><span data-stu-id="c8306-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="c8306-142">使用處理常式，如有一項需求</span><span class="sxs-lookup"><span data-stu-id="c8306-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="c8306-143">一對一關聯性的最短使用期限處理常式會利用唯一的要求範例如下：</span><span class="sxs-lookup"><span data-stu-id="c8306-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="c8306-144">上述程式碼會判斷目前的使用者主體是否宣告其已由已知且受信任的簽發者發出的出生日期。</span><span class="sxs-lookup"><span data-stu-id="c8306-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="c8306-145">宣告遺漏時，不會發生授權，在此情況下完成的工作就會傳回。</span><span class="sxs-lookup"><span data-stu-id="c8306-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="c8306-146">當宣告存在時，會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="c8306-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="c8306-147">如果使用者滿足需求所定義的最低存在時間，授權已被視為成功。</span><span class="sxs-lookup"><span data-stu-id="c8306-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="c8306-148">當授權是否成功，`context.Succeed`滿足的需求做為其唯一的參數叫用。</span><span class="sxs-lookup"><span data-stu-id="c8306-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="c8306-149">使用多個需求的處理常式</span><span class="sxs-lookup"><span data-stu-id="c8306-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="c8306-150">在其中的權限的處理常式可以處理三種不同類型的需求的一對多關聯性的範例如下：</span><span class="sxs-lookup"><span data-stu-id="c8306-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="c8306-151">上述程式碼會周遊[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;標記為成功不包含需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="c8306-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="c8306-152">針對`ReadPermission`需求，使用者必須是擁有者或贊助者來存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="c8306-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="c8306-153">若是`EditPermission`或`DeletePermission`需求，他或她必須存取要求的資源擁有者。</span><span class="sxs-lookup"><span data-stu-id="c8306-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="c8306-154">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="c8306-154">Handler registration</span></span>

<span data-ttu-id="c8306-155">在設定期間的服務集合中註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="c8306-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="c8306-156">例如:</span><span class="sxs-lookup"><span data-stu-id="c8306-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="c8306-157">上述程式碼會註冊`MinimumAgeHandler`為叫用單一`services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`。</span><span class="sxs-lookup"><span data-stu-id="c8306-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="c8306-158">可以使用任何內建註冊處理常式[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="c8306-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="c8306-159">項目應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="c8306-159">What should a handler return?</span></span>

<span data-ttu-id="c8306-160">請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="c8306-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="c8306-161">如何為成功或失敗處的狀態？</span><span class="sxs-lookup"><span data-stu-id="c8306-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="c8306-162">處理常式會表示成功呼叫`context.Succeed(IAuthorizationRequirement requirement)`，傳遞的需求，已驗證成功。</span><span class="sxs-lookup"><span data-stu-id="c8306-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="c8306-163">一般而言，處理失敗，因為可能會成功的相同需求的其他處理常式，不必處理常式。</span><span class="sxs-lookup"><span data-stu-id="c8306-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="c8306-164">若要保證失敗，即使其他要求處理常式會成功，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="c8306-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="c8306-165">如果處理常式會呼叫`context.Succeed`或`context.Fail`，仍會在所有其他的處理常式呼叫。</span><span class="sxs-lookup"><span data-stu-id="c8306-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="c8306-166">這可讓產生副作用，例如記錄，即使另一個處理常式已順利驗證或失敗的需求，會發生的需求。</span><span class="sxs-lookup"><span data-stu-id="c8306-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="c8306-167">當設定為`false`，則[InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性 （可在 ASP.NET Core 1.1 和更新版本） 的縮短處理常式執行時`context.Fail`呼叫。</span><span class="sxs-lookup"><span data-stu-id="c8306-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="c8306-168">`InvokeHandlersAfterFailure` 預設為`true`，在此情況下會呼叫所有的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c8306-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="c8306-169">即使驗證失敗，會呼叫授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="c8306-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="c8306-170">我為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="c8306-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="c8306-171">在您想要評估的情況下**或**為基礎，實作單一需求的多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="c8306-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="c8306-172">例如，Microsoft 有只能開啟與索引鍵的卡片的門。</span><span class="sxs-lookup"><span data-stu-id="c8306-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="c8306-173">如果您在家離開您關鍵賀卡，接待員列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="c8306-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="c8306-174">在此案例中，您必須是單一的需求， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="c8306-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="c8306-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="c8306-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="c8306-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c8306-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="c8306-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c8306-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="c8306-178">請確定這兩個處理常式[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="c8306-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="c8306-179">如果其中一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。</span><span class="sxs-lookup"><span data-stu-id="c8306-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="c8306-180">若要滿足原則使用 func</span><span class="sxs-lookup"><span data-stu-id="c8306-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="c8306-181">可能有哪些履行原則是簡單的程式碼中的情況。</span><span class="sxs-lookup"><span data-stu-id="c8306-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="c8306-182">可提供`Func<AuthorizationHandlerContext, bool>`時設定您的原則與`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="c8306-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="c8306-183">比方說前, 一個`BadgeEntryHandler`重寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="c8306-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="c8306-184">存取 MVC 處理常式中的要求內容</span><span class="sxs-lookup"><span data-stu-id="c8306-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="c8306-185">`HandleRequirementAsync`您在授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`而`TRequirement`處理。</span><span class="sxs-lookup"><span data-stu-id="c8306-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="c8306-186">MVC 或 Jabbr 之類的架構可用於將任何物件加入`Resource`屬性上的`AuthorizationHandlerContext`傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="c8306-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="c8306-187">例如，MVC 會將傳遞的執行個體[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)在`Resource`屬性。</span><span class="sxs-lookup"><span data-stu-id="c8306-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="c8306-188">這個屬性會提供存取權`HttpContext`， `RouteData`，和其他提供 MVC 和 Razor 頁面的所有項目。</span><span class="sxs-lookup"><span data-stu-id="c8306-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="c8306-189">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="c8306-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="c8306-190">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="c8306-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="c8306-191">您應該將轉型`Resource`屬性使用`is`關鍵字，然後確認 轉型成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：</span><span class="sxs-lookup"><span data-stu-id="c8306-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
