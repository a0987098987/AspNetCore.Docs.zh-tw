---
title: 在 ASP.NET Core 原則為基礎的授權
author: rick-anderson
description: 了解如何建立和使用授權原則的處理常式來強制執行的 ASP.NET Core 應用程式中的授權需求。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 411fee90bdccfb45c33f5d4ccd7864c83c614e70
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072857"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="c1f2f-103">在 ASP.NET Core 原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="c1f2f-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="c1f2f-104">基本上，[角色為基礎的授權](xref:security/authorization/roles)和[宣告型授權](xref:security/authorization/claims)使用需求、 要求處理常式和預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="c1f2f-105">這些建置組塊支援程式碼中的授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="c1f2f-106">結果會是更豐富、 可重複使用、 可測試性授權結構。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="c1f2f-107">授權原則是由一或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="c1f2f-108">在將它註冊為授權服務的組態，`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="c1f2f-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="c1f2f-109">在上述範例中，會建立 「 AtLeast21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="c1f2f-110">它有一個需求&mdash;的最低存在時間，提供做為參數的需求。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="c1f2f-111">原則適用於使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="c1f2f-112">例如: </span><span class="sxs-lookup"><span data-stu-id="c1f2f-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="c1f2f-113">需求</span><span class="sxs-lookup"><span data-stu-id="c1f2f-113">Requirements</span></span>

<span data-ttu-id="c1f2f-114">授權需求是原則可以用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="c1f2f-115">在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="c1f2f-116">需求實作[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空白標記的介面。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="c1f2f-117">參數化的最低年齡要求實作，如下所示：</span><span class="sxs-lookup"><span data-stu-id="c1f2f-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="c1f2f-118">一項需求不需要將資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="c1f2f-119">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="c1f2f-119">Authorization handlers</span></span>

<span data-ttu-id="c1f2f-120">授權處理常式負責需求之屬性的評估。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="c1f2f-121">授權的處理常式會評估需求，提供對[AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)來判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-121">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="c1f2f-122">可以有一項需求[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="c1f2f-123">處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中`TRequirement`是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-123">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="c1f2f-124">或者，處理常式可能會實作[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多個類型的需求。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-124">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="c1f2f-125">使用的一項需求的處理常式</span><span class="sxs-lookup"><span data-stu-id="c1f2f-125">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="c1f2f-126">最短使用期限處理常式會利用一個需求是一對一關聯性的範例如下：</span><span class="sxs-lookup"><span data-stu-id="c1f2f-126">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="c1f2f-127">上述程式碼會判斷目前的使用者主體已宣告已知且受信任的簽發者所發出的出生日期。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-127">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="c1f2f-128">宣告遺漏時，無法進行授權，在此情況下會傳回已完成的工作。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-128">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="c1f2f-129">當宣告存在時，會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-129">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="c1f2f-130">如果使用者符合此需求所定義的最低存在時間，授權會被視為成功。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-130">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="c1f2f-131">當授權是否成功，`context.Succeed`以滿足需求做為其唯一參數叫用。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-131">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="c1f2f-132">使用多個需求的處理常式</span><span class="sxs-lookup"><span data-stu-id="c1f2f-132">Use a handler for multiple requirements</span></span>

<span data-ttu-id="c1f2f-133">權限的處理常式會利用三個需求的一對多關聯性的範例如下：</span><span class="sxs-lookup"><span data-stu-id="c1f2f-133">The following is an example of a one-to-many relationship in which a permission handler utilizes three requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="c1f2f-134">上述程式碼會周遊[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;標記為成功不包含要求的屬性。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-134">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="c1f2f-135">如果使用者具有讀取權限，他或她必須是擁有者或贊助者來存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-135">If the user has read permission, he or she must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="c1f2f-136">如果使用者編輯或刪除權限，他或她必須存取要求之資源的擁有者。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-136">If the user has edit or delete permission, he or she must be an owner to access the requested resource.</span></span> <span data-ttu-id="c1f2f-137">當授權是否成功，`context.Succeed`以滿足需求做為其唯一參數叫用。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-137">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="c1f2f-138">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="c1f2f-138">Handler registration</span></span>

<span data-ttu-id="c1f2f-139">處理常式都登錄在設定期間的服務集合。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-139">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="c1f2f-140">例如: </span><span class="sxs-lookup"><span data-stu-id="c1f2f-140">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="c1f2f-141">每個處理常式，會叫用加入至服務集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-141">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="c1f2f-142">功能應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="c1f2f-142">What should a handler return?</span></span>

<span data-ttu-id="c1f2f-143">請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-143">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="c1f2f-144">狀態為成功或失敗的方式？</span><span class="sxs-lookup"><span data-stu-id="c1f2f-144">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="c1f2f-145">處理常式呼叫來表示成功`context.Succeed(IAuthorizationRequirement requirement)`，傳遞需求已順利通過驗證。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-145">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="c1f2f-146">一般而言，處理失敗，因為可能會成功的相同需求的其他處理常式不需要處理常式。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-146">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="c1f2f-147">若要保證失敗，即使其他需求的處理常式會成功，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-147">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="c1f2f-148">當設定為`false`、 [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性 （可在 ASP.NET Core 1.1 及更新版本） short-circuits 的處理常式執行時`context.Fail`呼叫。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-148">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="c1f2f-149">`InvokeHandlersAfterFailure` 預設為`true`，在此情況下會呼叫所有的處理常式。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-149">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="c1f2f-150">這可讓以產生副作用，例如記錄，一律會發生需求即使`context.Fail`已經在另一個處理常式中呼叫。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-150">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="c1f2f-151">為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="c1f2f-151">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="c1f2f-152">在您想要評估的情況下**或**為基礎，實作多個處理常式為單一的需求。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-152">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="c1f2f-153">例如，Microsoft 有門只開啟與索引鍵的卡片。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-153">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="c1f2f-154">如果您在家離開您金鑰卡，接線生列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-154">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="c1f2f-155">在此案例中，您必須是單一的必要條件， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-155">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="c1f2f-156">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="c1f2f-156">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="c1f2f-157">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1f2f-157">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="c1f2f-158">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="c1f2f-158">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="c1f2f-159">請確定這兩個處理常式會[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-159">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="c1f2f-160">如果任一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-160">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="c1f2f-161">符合原則中使用函式</span><span class="sxs-lookup"><span data-stu-id="c1f2f-161">Using a func to fulfill a policy</span></span>

<span data-ttu-id="c1f2f-162">可能的情況下在哪些完成原則簡單程式碼來表達。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-162">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="c1f2f-163">可提供`Func<AuthorizationHandlerContext, bool>`時設定與原則`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-163">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="c1f2f-164">例如，先前`BadgeEntryHandler`可以改寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="c1f2f-164">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="c1f2f-165">在處理常式中存取的 MVC 要求內容</span><span class="sxs-lookup"><span data-stu-id="c1f2f-165">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="c1f2f-166">`HandleRequirementAsync`授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`和`TRequirement`所處理。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-166">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="c1f2f-167">任何將物件加入至可用的架構，例如 MVC 或 Jabbr`Resource`屬性`AuthorizationHandlerContext`傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-167">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="c1f2f-168">MVC 的執行個體的傳遞，例如[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)中`Resource`屬性。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-168">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="c1f2f-169">此屬性可存取`HttpContext`， `RouteData`，和 else MVC 和 Razor 頁面所提供的所有項目。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-169">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="c1f2f-170">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-170">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="c1f2f-171">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="c1f2f-171">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="c1f2f-172">您應該轉換`Resource`屬性使用`as`關鍵字，然後確認 轉型已成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：</span><span class="sxs-lookup"><span data-stu-id="c1f2f-172">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
