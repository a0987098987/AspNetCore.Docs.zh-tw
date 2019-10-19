---
title: ASP.NET Core 中以資源為基礎的授權
author: scottaddie
description: 瞭解當授權屬性不足夠時，如何在 ASP.NET Core 應用程式中執行以資源為基礎的授權。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 835592521c714e270595e1448ae6e0aed1707b77
ms.sourcegitcommit: a166291c6708f5949c417874108332856b53b6a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/18/2019
ms.locfileid: "72590009"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="977f3-103">ASP.NET Core 中以資源為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="977f3-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="977f3-104">授權策略會根據所存取的資源而定。</span><span class="sxs-lookup"><span data-stu-id="977f3-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="977f3-105">假設有一個具有 author 屬性的檔。</span><span class="sxs-lookup"><span data-stu-id="977f3-105">Consider a document that has an author property.</span></span> <span data-ttu-id="977f3-106">只有作者可以更新檔。</span><span class="sxs-lookup"><span data-stu-id="977f3-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="977f3-107">因此，必須先從資料存放區中取出檔，才能進行授權評估。</span><span class="sxs-lookup"><span data-stu-id="977f3-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="977f3-108">屬性評估會在資料系結之前，以及在執行載入檔的頁面處理常式或動作之前進行。</span><span class="sxs-lookup"><span data-stu-id="977f3-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="977f3-109">基於這些理由，具有 `[Authorize]` 屬性的宣告式授權並不足夠。</span><span class="sxs-lookup"><span data-stu-id="977f3-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="977f3-110">相反地，您可以叫用自訂授權方法 &mdash;a 樣式，也就是「*命令式授權*」。</span><span class="sxs-lookup"><span data-stu-id="977f3-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="977f3-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="977f3-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="977f3-112">[建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式](xref:security/authorization/secure-data)包含使用以資源為基礎之授權的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="977f3-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="977f3-113">使用命令式授權</span><span class="sxs-lookup"><span data-stu-id="977f3-113">Use imperative authorization</span></span>

<span data-ttu-id="977f3-114">授權會實作為[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，並在 `Startup` 類別內的服務集合中註冊。</span><span class="sxs-lookup"><span data-stu-id="977f3-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="977f3-115">此服務可透過相依性[插入](xref:fundamentals/dependency-injection)頁面處理常式或動作來取得。</span><span class="sxs-lookup"><span data-stu-id="977f3-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="977f3-116">`IAuthorizationService` 有兩個 `AuthorizeAsync` 方法多載：一個接受資源和原則名稱，另一個則接受資源，以及要評估的需求清單。</span><span class="sxs-lookup"><span data-stu-id="977f3-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="977f3-117">在下列範例中，要保護的資源會載入至自訂的 `Document` 物件。</span><span class="sxs-lookup"><span data-stu-id="977f3-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="977f3-118">系統會叫用 `AuthorizeAsync` 多載，以判斷是否允許目前的使用者編輯提供的檔。</span><span class="sxs-lookup"><span data-stu-id="977f3-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="977f3-119">自訂的 "EditPolicy" 授權原則會納入決定。</span><span class="sxs-lookup"><span data-stu-id="977f3-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="977f3-120">如需建立授權原則的詳細資訊，請參閱[自訂以原則為基礎的授權](xref:security/authorization/policies)。</span><span class="sxs-lookup"><span data-stu-id="977f3-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="977f3-121">下列程式碼範例假設已執行驗證，並設定 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="977f3-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="977f3-122">撰寫以資源為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="977f3-122">Write a resource-based handler</span></span>

<span data-ttu-id="977f3-123">針對以資源為基礎的授權撰寫處理常式，與[撰寫單純的需求處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)並沒有太大差異。</span><span class="sxs-lookup"><span data-stu-id="977f3-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="977f3-124">建立自訂需求類別，並執行需求處理常式類別。</span><span class="sxs-lookup"><span data-stu-id="977f3-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="977f3-125">如需建立需求類別的詳細資訊，請參閱[需求](xref:security/authorization/policies#requirements)。</span><span class="sxs-lookup"><span data-stu-id="977f3-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="977f3-126">處理常式類別會同時指定需求和資源類型。</span><span class="sxs-lookup"><span data-stu-id="977f3-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="977f3-127">例如，利用 `SameAuthorRequirement` 和 `Document` 資源的處理常式如下：</span><span class="sxs-lookup"><span data-stu-id="977f3-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="977f3-128">在上述範例中，假設 `SameAuthorRequirement` 是更泛型 `SpecificAuthorRequirement` 類別的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="977f3-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="977f3-129">@No__t_0 類別（未顯示）包含代表作者名稱的 `Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="977f3-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="977f3-130">@No__t_0 屬性可以設定為目前的使用者。</span><span class="sxs-lookup"><span data-stu-id="977f3-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="977f3-131">在 `Startup.ConfigureServices` 中註冊需求和處理常式：</span><span class="sxs-lookup"><span data-stu-id="977f3-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="977f3-132">操作需求</span><span class="sxs-lookup"><span data-stu-id="977f3-132">Operational requirements</span></span>

<span data-ttu-id="977f3-133">如果您要根據 CRUD （建立、讀取、更新、刪除）作業的結果進行決策，請使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper 類別。</span><span class="sxs-lookup"><span data-stu-id="977f3-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="977f3-134">這個類別可讓您針對每個作業類型撰寫單一處理程式，而不是個別的類別。</span><span class="sxs-lookup"><span data-stu-id="977f3-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="977f3-135">若要使用它，請提供一些作業名稱：</span><span class="sxs-lookup"><span data-stu-id="977f3-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="977f3-136">處理常式會依照下列方式，使用 `OperationAuthorizationRequirement` 需求和 `Document` 資源來執行：</span><span class="sxs-lookup"><span data-stu-id="977f3-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="977f3-137">上述處理常式會使用資源、使用者的身分識別和需求的 `Name` 屬性來驗證作業。</span><span class="sxs-lookup"><span data-stu-id="977f3-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="977f3-138">操作資源處理常式的挑戰和禁止</span><span class="sxs-lookup"><span data-stu-id="977f3-138">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="977f3-139">本節說明如何處理挑戰和禁止動作結果，以及挑戰和禁止的差異。</span><span class="sxs-lookup"><span data-stu-id="977f3-139">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="977f3-140">若要呼叫操作資源處理常式，請在頁面處理常式或動作中叫用 `AuthorizeAsync` 時指定作業。</span><span class="sxs-lookup"><span data-stu-id="977f3-140">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="977f3-141">下列範例會決定是否允許已驗證的使用者查看提供的檔。</span><span class="sxs-lookup"><span data-stu-id="977f3-141">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="977f3-142">下列程式碼範例假設已執行驗證，並設定 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="977f3-142">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="977f3-143">如果授權成功，則會傳回用於查看檔的頁面。</span><span class="sxs-lookup"><span data-stu-id="977f3-143">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="977f3-144">如果授權失敗，但使用者已通過驗證，則傳回 `ForbidResult` 會通知任何驗證中介軟體授權失敗。</span><span class="sxs-lookup"><span data-stu-id="977f3-144">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="977f3-145">當必須執行驗證時，會傳回 `ChallengeResult`。</span><span class="sxs-lookup"><span data-stu-id="977f3-145">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="977f3-146">若為互動式瀏覽器用戶端，將使用者重新導向至登入頁面可能是適當的方式。</span><span class="sxs-lookup"><span data-stu-id="977f3-146">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="977f3-147">如果授權成功，則會傳回檔的視圖。</span><span class="sxs-lookup"><span data-stu-id="977f3-147">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="977f3-148">如果授權失敗，傳回 `ChallengeResult` 會通知任何驗證中介軟體，授權失敗，中介軟體可以接受適當的回應。</span><span class="sxs-lookup"><span data-stu-id="977f3-148">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="977f3-149">適當的回應可能會傳回401或403狀態碼。</span><span class="sxs-lookup"><span data-stu-id="977f3-149">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="977f3-150">對於互動式瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="977f3-150">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
