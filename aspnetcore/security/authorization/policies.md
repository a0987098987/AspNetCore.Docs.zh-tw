---
title: ASP.NET Core 中的原則為基礎的授權
author: rick-anderson
description: 了解如何建立和使用授權原則的處理常式，以強制執行的 ASP.NET Core 應用程式中的授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: ea9d687d3810c104d5b3fa39033849c21569709b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891825"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="d2c8e-103">ASP.NET Core 中的原則為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="d2c8e-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="d2c8e-104">基本上，[角色為基礎的授權](xref:security/authorization/roles)並[宣告型授權](xref:security/authorization/claims)使用需求，需求處理常式，並預先設定的原則。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="d2c8e-105">這些建置組塊支援程式碼中的授權評估的運算式。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="d2c8e-106">結果會是更豐富、 可重複使用、 可測試的授權結構。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="d2c8e-107">授權原則是由一或多個需求所組成。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="d2c8e-108">它會註冊為授權服務的組態，在`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="d2c8e-109">在上述範例中，會建立 「 AtLeast21 」 原則。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="d2c8e-110">它有一個需求&mdash;的最短使用期限，做為參數的需求，提供。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="d2c8e-111">將原則套用至 MVC 控制器</span><span class="sxs-lookup"><span data-stu-id="d2c8e-111">Applying policies to MVC controllers</span></span>

<span data-ttu-id="d2c8e-112">如果您使用 Razor 頁面，請參閱[將原則套用至 Razor Pages](#applying-policies-to-razor-pages)本文件中。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-112">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="d2c8e-113">原則會套用至控制器使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-113">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d2c8e-114">例如：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-114">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="d2c8e-115">將原則套用至 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="d2c8e-115">Applying policies to Razor Pages</span></span>

<span data-ttu-id="d2c8e-116">原則時，會套用至 Razor 頁面上，使用`[Authorize]`原則名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-116">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="d2c8e-117">例如: </span><span class="sxs-lookup"><span data-stu-id="d2c8e-117">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="d2c8e-118">原則也可以套用至 Razor Pages 使用[授權慣例](xref:security/authorization/razor-pages-authorization)。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-118">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="d2c8e-119">需求</span><span class="sxs-lookup"><span data-stu-id="d2c8e-119">Requirements</span></span>

<span data-ttu-id="d2c8e-120">授權需求是原則可用來評估目前的使用者主體的資料參數的集合。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-120">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="d2c8e-121">在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-121">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="d2c8e-122">需求會實作[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的 marker 介面。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-122">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="d2c8e-123">無法實作參數化的最短使用期限需求如下所示：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-123">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="d2c8e-124">如果授權原則包含多個授權需求，所有的需求必須傳遞成功原則評估的順序。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-124">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="d2c8e-125">換句話說，在處理新增至單一授權原則的多個授權需求**AND**為基礎。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-125">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="d2c8e-126">要求不需要有資料或屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-126">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="d2c8e-127">授權的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c8e-127">Authorization handlers</span></span>

<span data-ttu-id="d2c8e-128">授權的處理常式會負責需求之屬性的評估。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-128">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="d2c8e-129">授權的處理常式會評估針對所提供的需求[AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)來判斷是否允許存取。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-129">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="d2c8e-130">需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-130">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="d2c8e-131">處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中`TRequirement`是要處理的需求。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-131">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="d2c8e-132">或者，可能會實作處理常式[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多個類型的需求。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-132">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="d2c8e-133">使用處理常式，如有一項需求</span><span class="sxs-lookup"><span data-stu-id="d2c8e-133">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="d2c8e-134">一對一關聯性的最短使用期限處理常式會利用唯一的要求範例如下：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-134">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="d2c8e-135">上述程式碼會判斷目前的使用者主體是否宣告其已由已知且受信任的簽發者發出的出生日期。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-135">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="d2c8e-136">宣告遺漏時，不會發生授權，在此情況下完成的工作就會傳回。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-136">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="d2c8e-137">當宣告存在時，會計算使用者的年齡。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-137">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="d2c8e-138">如果使用者滿足需求所定義的最低存在時間，授權已被視為成功。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-138">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="d2c8e-139">當授權是否成功，`context.Succeed`滿足的需求做為其唯一的參數叫用。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-139">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="d2c8e-140">使用多個需求的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2c8e-140">Use a handler for multiple requirements</span></span>

<span data-ttu-id="d2c8e-141">在其中的權限的處理常式可以處理三種不同類型的需求的一對多關聯性的範例如下：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-141">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="d2c8e-142">上述程式碼會周遊[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;標記為成功不包含需求的屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-142">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="d2c8e-143">針對`ReadPermission`需求，使用者必須是擁有者或贊助者來存取要求的資源。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-143">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="d2c8e-144">若是`EditPermission`或`DeletePermission`需求，他或她必須存取要求的資源擁有者。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-144">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="d2c8e-145">處理常式註冊</span><span class="sxs-lookup"><span data-stu-id="d2c8e-145">Handler registration</span></span>

<span data-ttu-id="d2c8e-146">在設定期間的服務集合中註冊處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-146">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="d2c8e-147">例如: </span><span class="sxs-lookup"><span data-stu-id="d2c8e-147">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="d2c8e-148">上述程式碼會註冊`MinimumAgeHandler`為叫用單一`services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-148">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="d2c8e-149">可以使用任何內建註冊處理常式[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-149">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="d2c8e-150">項目應該處理常式傳回？</span><span class="sxs-lookup"><span data-stu-id="d2c8e-150">What should a handler return?</span></span>

<span data-ttu-id="d2c8e-151">請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-151">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="d2c8e-152">如何為成功或失敗處的狀態？</span><span class="sxs-lookup"><span data-stu-id="d2c8e-152">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="d2c8e-153">處理常式會表示成功呼叫`context.Succeed(IAuthorizationRequirement requirement)`，傳遞的需求，已驗證成功。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-153">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="d2c8e-154">一般而言，處理失敗，因為可能會成功的相同需求的其他處理常式，不必處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-154">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="d2c8e-155">若要保證失敗，即使其他要求處理常式會成功，呼叫`context.Fail`。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-155">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="d2c8e-156">如果處理常式會呼叫`context.Succeed`或`context.Fail`，仍會在所有其他的處理常式呼叫。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-156">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="d2c8e-157">這可讓產生副作用，例如記錄，即使另一個處理常式已順利驗證或失敗的需求，會發生的需求。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-157">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="d2c8e-158">當設定為`false`，則[InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性 （可在 ASP.NET Core 1.1 和更新版本） 的縮短處理常式執行時`context.Fail`呼叫。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-158">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="d2c8e-159">`InvokeHandlersAfterFailure` 預設為`true`，在此情況下會呼叫所有的處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-159">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="d2c8e-160">即使驗證失敗，會呼叫授權處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-160">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="d2c8e-161">我為什麼需要多個處理常式之需求？</span><span class="sxs-lookup"><span data-stu-id="d2c8e-161">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="d2c8e-162">在您想要評估的情況下**或**為基礎，實作單一需求的多個處理常式。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-162">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="d2c8e-163">例如，Microsoft 有只能開啟與索引鍵的卡片的門。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-163">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="d2c8e-164">如果您在家離開您關鍵賀卡，接待員列印暫存的貼紙，並會為您開啟媒體櫃門。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-164">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="d2c8e-165">在此案例中，您必須是單一的需求， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-165">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="d2c8e-166">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="d2c8e-166">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="d2c8e-167">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d2c8e-167">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="d2c8e-168">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="d2c8e-168">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="d2c8e-169">請確定這兩個處理常式[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-169">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="d2c8e-170">如果其中一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-170">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="d2c8e-171">若要滿足原則使用 func</span><span class="sxs-lookup"><span data-stu-id="d2c8e-171">Using a func to fulfill a policy</span></span>

<span data-ttu-id="d2c8e-172">可能有哪些履行原則是簡單的程式碼中的情況。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-172">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="d2c8e-173">可提供`Func<AuthorizationHandlerContext, bool>`時設定您的原則與`RequireAssertion`原則產生器。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-173">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="d2c8e-174">比方說前, 一個`BadgeEntryHandler`重寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-174">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="d2c8e-175">存取 MVC 處理常式中的要求內容</span><span class="sxs-lookup"><span data-stu-id="d2c8e-175">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="d2c8e-176">`HandleRequirementAsync`您在授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`而`TRequirement`處理。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-176">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="d2c8e-177">MVC 或 Jabbr 之類的架構可用於將任何物件加入`Resource`屬性上的`AuthorizationHandlerContext`傳遞額外資訊。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-177">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="d2c8e-178">例如，MVC 會將傳遞的執行個體[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)在`Resource`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-178">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="d2c8e-179">這個屬性會提供存取權`HttpContext`， `RouteData`，和其他提供 MVC 和 Razor 頁面的所有項目。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-179">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="d2c8e-180">使用`Resource`屬性是特定的架構。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-180">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="d2c8e-181">使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。</span><span class="sxs-lookup"><span data-stu-id="d2c8e-181">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="d2c8e-182">您應該將轉型`Resource`屬性使用`is`關鍵字，然後確認 轉型成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：</span><span class="sxs-lookup"><span data-stu-id="d2c8e-182">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
