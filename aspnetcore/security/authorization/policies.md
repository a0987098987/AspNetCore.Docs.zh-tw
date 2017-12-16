---
title: "在 ASP.NET Core 自訂以原則為基礎的授權"
author: rick-anderson
description: "了解如何建立及使用自訂授權原則的處理常式來強制執行的 ASP.NET Core 應用程式中的授權需求。"
keywords: "ASP.NET Core 授權、 自訂原則、 授權原則"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="1f3b3-104">以原則為基礎的自訂授權</span><span class="sxs-lookup"><span data-stu-id="1f3b3-104">Custom policy-based authorization</span></span>

<span data-ttu-id="1f3b3-105">基本上，[角色為基礎的授權](xref:security/authorization/roles)和[宣告型授權](xref:security/authorization/claims)使用需求、 要求處理常式和預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="1f3b3-106">這些建置組塊支援程式碼中的授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="1f3b3-107">結果會是更豐富、 可重複使用、 可測試性授權結構。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="1f3b3-108">授權原則是由一或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="1f3b3-109">在將它註冊為授權服務的組態，`ConfigureServices`方法`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="1f3b3-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="1f3b3-110">在上述範例中，會建立 「 AtLeast21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="1f3b3-111">它有一個需求的最低存在時間，其提供做為參數的需求。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="1f3b3-112">原則適用於使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="1f3b3-113">例如: </span><span class="sxs-lookup"><span data-stu-id="1f3b3-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="1f3b3-114">需求</span><span class="sxs-lookup"><span data-stu-id="1f3b3-114">Requirements</span></span>

<span data-ttu-id="1f3b3-115">授權需求是原則可以用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="1f3b3-116">在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="1f3b3-117">需求實作`IAuthorizationRequirement`，這是空白標記的介面。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="1f3b3-118">參數化的最低年齡要求實作，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1f3b3-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="1f3b3-119">一項需求不需要將資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="1f3b3-120">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="1f3b3-120">Authorization handlers</span></span>

<span data-ttu-id="1f3b3-121">授權處理常式負責需求之屬性的評估。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="1f3b3-122">授權的處理常式會評估需求，提供對`AuthorizationHandlerContext`來判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="1f3b3-123">可以有一項需求[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="1f3b3-124">處理常式繼承`AuthorizationHandler<T>`，其中`T`是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="1f3b3-125">最短使用期限處理常式可能如下所示：</span><span class="sxs-lookup"><span data-stu-id="1f3b3-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="1f3b3-126">上述程式碼會判斷目前的使用者主體已宣告已知且受信任的簽發者所發出的出生日期。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="1f3b3-127">宣告遺漏時，無法進行授權，在此情況下會傳回已完成的工作。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="1f3b3-128">當宣告存在時，會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="1f3b3-129">如果使用者符合此需求所定義的最低存在時間，授權會被視為成功。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="1f3b3-130">當授權是否成功，`context.Succeed`用來叫用做為參數的滿足需求。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="1f3b3-131">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="1f3b3-131">Handler registration</span></span>

<span data-ttu-id="1f3b3-132">處理常式都登錄在設定期間的服務集合。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="1f3b3-133">例如: </span><span class="sxs-lookup"><span data-stu-id="1f3b3-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="1f3b3-134">每個處理常式，會叫用加入至服務集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="1f3b3-135">功能應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="1f3b3-135">What should a handler return?</span></span>

<span data-ttu-id="1f3b3-136">請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="1f3b3-137">狀態為成功或失敗的方式？</span><span class="sxs-lookup"><span data-stu-id="1f3b3-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="1f3b3-138">處理常式呼叫來表示成功`context.Succeed(IAuthorizationRequirement requirement)`，傳遞需求已順利通過驗證。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="1f3b3-139">處理常式不需一般而言，處理失敗，因為相同的需求的其他處理常式可能會成功。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="1f3b3-140">若要保證失敗，即使其他需求的處理常式會成功，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="1f3b3-141">不論您呼叫您的處理常式內，將原則需要需求時呼叫之需求的所有處理常式。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="1f3b3-142">這可讓具有副作用，例如記錄，就會一律進行需求即使`context.Fail()`已經在另一個處理常式中呼叫。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="1f3b3-143">為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="1f3b3-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="1f3b3-144">在您想要評估的情況下**或**為基礎，實作多個處理常式為單一的需求。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="1f3b3-145">例如，Microsoft 有門只開啟與索引鍵的卡片。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="1f3b3-146">如果您在家離開您金鑰卡，接線生列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="1f3b3-147">在此案例中，您必須是單一的必要條件， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="1f3b3-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="1f3b3-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="1f3b3-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="1f3b3-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="1f3b3-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="1f3b3-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="1f3b3-151">請確定這兩個處理常式會[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="1f3b3-152">如果任一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="1f3b3-153">符合原則中使用函式</span><span class="sxs-lookup"><span data-stu-id="1f3b3-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="1f3b3-154">可能的情況下在哪些完成原則簡單程式碼來表達。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="1f3b3-155">可提供`Func<AuthorizationHandlerContext, bool>`時設定與原則`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="1f3b3-156">例如，先前`BadgeEntryHandler`可以改寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1f3b3-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="1f3b3-157">在處理常式中存取的 MVC 要求內容</span><span class="sxs-lookup"><span data-stu-id="1f3b3-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="1f3b3-158">`HandleRequirementAsync`授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`和`TRequirement`所處理。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="1f3b3-159">任何將物件加入至可用的架構，例如 MVC 或 Jabbr`Resource`屬性`AuthorizationHandlerContext`傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="1f3b3-160">MVC 的執行個體的傳遞，例如[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)中`Resource`屬性。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="1f3b3-161">此屬性可存取`HttpContext`， `RouteData`，和 else MVC 和 Razor 頁面所提供的所有項目。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="1f3b3-162">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="1f3b3-163">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="1f3b3-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="1f3b3-164">您應該轉換`Resource`屬性使用`as`關鍵字，然後確認 轉型已成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：</span><span class="sxs-lookup"><span data-stu-id="1f3b3-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
