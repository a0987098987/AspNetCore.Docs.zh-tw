---
title: ASP.NET Core 中的原則為基礎的授權
author: rick-anderson
description: 了解如何建立和使用授權原則的處理常式，以強制執行的 ASP.NET Core 應用程式中的授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 4e8a9ac6c0594f9bab67214aaa8cab9199cca29d
ms.sourcegitcommit: cec77d5ad8a0cedb1ecbec32834111492afd0cd2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/10/2019
ms.locfileid: "54207391"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="a190c-103">ASP.NET Core 中的原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="a190c-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="a190c-104">基本上，[角色為基礎的授權](xref:security/authorization/roles)並[宣告型授權](xref:security/authorization/claims)使用需求，需求處理常式，並預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="a190c-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="a190c-105">這些建置組塊支援程式碼中的授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="a190c-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="a190c-106">結果會是更豐富、 可重複使用、 可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="a190c-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="a190c-107">授權原則是由一或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="a190c-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="a190c-108">它會註冊為授權服務的組態，在`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="a190c-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="a190c-109">在上述範例中，會建立 「 AtLeast21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="a190c-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="a190c-110">它有一個需求&mdash;的最短使用期限，做為參數的需求，提供。</span><span class="sxs-lookup"><span data-stu-id="a190c-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="a190c-111">原則會套用使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="a190c-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="a190c-112">例如: </span><span class="sxs-lookup"><span data-stu-id="a190c-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="a190c-113">需求</span><span class="sxs-lookup"><span data-stu-id="a190c-113">Requirements</span></span>

<span data-ttu-id="a190c-114">授權需求是原則可用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="a190c-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="a190c-115">在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="a190c-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="a190c-116">需求會實作[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的 marker 介面。</span><span class="sxs-lookup"><span data-stu-id="a190c-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="a190c-117">無法實作參數化的最短使用期限需求如下所示：</span><span class="sxs-lookup"><span data-stu-id="a190c-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="a190c-118">要求不需要有資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="a190c-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="a190c-119">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="a190c-119">Authorization handlers</span></span>

<span data-ttu-id="a190c-120">授權的處理常式會負責需求之屬性的評估。</span><span class="sxs-lookup"><span data-stu-id="a190c-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="a190c-121">授權的處理常式會評估針對所提供的需求[AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)來判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="a190c-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="a190c-122">需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="a190c-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="a190c-123">處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中`TRequirement`是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="a190c-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="a190c-124">或者，可能會實作處理常式[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多個類型的需求。</span><span class="sxs-lookup"><span data-stu-id="a190c-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="a190c-125">使用處理常式，如有一項需求</span><span class="sxs-lookup"><span data-stu-id="a190c-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="a190c-126">一對一關聯性的最短使用期限處理常式會利用唯一的要求範例如下：</span><span class="sxs-lookup"><span data-stu-id="a190c-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="a190c-127">上述程式碼會判斷目前的使用者主體是否宣告其已由已知且受信任的簽發者發出的出生日期。</span><span class="sxs-lookup"><span data-stu-id="a190c-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="a190c-128">宣告遺漏時，不會發生授權，在此情況下完成的工作就會傳回。</span><span class="sxs-lookup"><span data-stu-id="a190c-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="a190c-129">當宣告存在時，會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="a190c-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="a190c-130">如果使用者滿足需求所定義的最低存在時間，授權已被視為成功。</span><span class="sxs-lookup"><span data-stu-id="a190c-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="a190c-131">當授權是否成功，`context.Succeed`滿足的需求做為其唯一的參數叫用。</span><span class="sxs-lookup"><span data-stu-id="a190c-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="a190c-132">使用多個需求的處理常式</span><span class="sxs-lookup"><span data-stu-id="a190c-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="a190c-133">在其中的權限的處理常式可以處理三種不同類型的需求的一對多關聯性的範例如下：</span><span class="sxs-lookup"><span data-stu-id="a190c-133">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="a190c-134">上述程式碼會周遊[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;標記為成功不包含需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="a190c-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="a190c-135">針對`ReadPermission`需求，使用者必須是擁有者或贊助者來存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="a190c-135">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="a190c-136">若是`EditPermission`或`DeletePermission`需求，他或她必須存取要求的資源擁有者。</span><span class="sxs-lookup"><span data-stu-id="a190c-136">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="a190c-137">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="a190c-137">Handler registration</span></span>

<span data-ttu-id="a190c-138">在設定期間的服務集合中註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="a190c-138">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="a190c-139">例如: </span><span class="sxs-lookup"><span data-stu-id="a190c-139">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="a190c-140">每個處理常式會叫用時加入的服務集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`。</span><span class="sxs-lookup"><span data-stu-id="a190c-140">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="a190c-141">項目應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="a190c-141">What should a handler return?</span></span>

<span data-ttu-id="a190c-142">請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="a190c-142">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="a190c-143">如何為成功或失敗處的狀態？</span><span class="sxs-lookup"><span data-stu-id="a190c-143">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="a190c-144">處理常式會表示成功呼叫`context.Succeed(IAuthorizationRequirement requirement)`，傳遞的需求，已驗證成功。</span><span class="sxs-lookup"><span data-stu-id="a190c-144">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="a190c-145">一般而言，處理失敗，因為可能會成功的相同需求的其他處理常式，不必處理常式。</span><span class="sxs-lookup"><span data-stu-id="a190c-145">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="a190c-146">若要保證失敗，即使其他要求處理常式會成功，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="a190c-146">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="a190c-147">當設定為`false`，則[InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性 （可在 ASP.NET Core 1.1 和更新版本） 的縮短處理常式執行時`context.Fail`呼叫。</span><span class="sxs-lookup"><span data-stu-id="a190c-147">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="a190c-148">`InvokeHandlersAfterFailure` 預設為`true`，在此情況下會呼叫所有的處理常式。</span><span class="sxs-lookup"><span data-stu-id="a190c-148">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="a190c-149">這可讓產生副作用，例如記錄，這一律會發生的需求即使`context.Fail`已在另一個處理常式中呼叫。</span><span class="sxs-lookup"><span data-stu-id="a190c-149">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="a190c-150">我為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="a190c-150">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="a190c-151">在您想要評估的情況下**或**為基礎，實作單一需求的多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="a190c-151">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="a190c-152">例如，Microsoft 有只能開啟與索引鍵的卡片的門。</span><span class="sxs-lookup"><span data-stu-id="a190c-152">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="a190c-153">如果您在家離開您關鍵賀卡，接待員列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="a190c-153">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="a190c-154">在此案例中，您必須是單一的需求， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="a190c-154">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="a190c-155">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="a190c-155">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="a190c-156">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="a190c-156">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="a190c-157">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="a190c-157">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="a190c-158">請確定這兩個處理常式[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="a190c-158">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="a190c-159">如果其中一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。</span><span class="sxs-lookup"><span data-stu-id="a190c-159">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="a190c-160">若要滿足原則使用 func</span><span class="sxs-lookup"><span data-stu-id="a190c-160">Using a func to fulfill a policy</span></span>

<span data-ttu-id="a190c-161">可能有哪些履行原則是簡單的程式碼中的情況。</span><span class="sxs-lookup"><span data-stu-id="a190c-161">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="a190c-162">可提供`Func<AuthorizationHandlerContext, bool>`時設定您的原則與`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="a190c-162">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="a190c-163">比方說前, 一個`BadgeEntryHandler`重寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a190c-163">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="a190c-164">存取 MVC 處理常式中的要求內容</span><span class="sxs-lookup"><span data-stu-id="a190c-164">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="a190c-165">`HandleRequirementAsync`您在授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`而`TRequirement`處理。</span><span class="sxs-lookup"><span data-stu-id="a190c-165">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="a190c-166">MVC 或 Jabbr 之類的架構可用於將任何物件加入`Resource`屬性上的`AuthorizationHandlerContext`傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="a190c-166">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="a190c-167">例如，MVC 會將傳遞的執行個體[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)在`Resource`屬性。</span><span class="sxs-lookup"><span data-stu-id="a190c-167">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="a190c-168">這個屬性會提供存取權`HttpContext`， `RouteData`，和其他提供 MVC 和 Razor 頁面的所有項目。</span><span class="sxs-lookup"><span data-stu-id="a190c-168">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="a190c-169">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="a190c-169">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="a190c-170">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="a190c-170">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="a190c-171">您應該將轉型`Resource`屬性使用`as`關鍵字，然後確認 轉型成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：</span><span class="sxs-lookup"><span data-stu-id="a190c-171">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
