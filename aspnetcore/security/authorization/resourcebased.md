---
title: ASP.NET Core 中以資源為基礎的授權
author: scottaddie
description: 瞭解當授權屬性不足夠時，如何在 ASP.NET Core 應用程式中執行以資源為基礎的授權。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 2be611c754583d996db7107f341b1be03cef73cf
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78664798"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="47b36-103">ASP.NET Core 中以資源為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="47b36-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="47b36-104">授權策略會根據所存取的資源而定。</span><span class="sxs-lookup"><span data-stu-id="47b36-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="47b36-105">假設有一個具有 author 屬性的檔。</span><span class="sxs-lookup"><span data-stu-id="47b36-105">Consider a document that has an author property.</span></span> <span data-ttu-id="47b36-106">只有作者可以更新檔。</span><span class="sxs-lookup"><span data-stu-id="47b36-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="47b36-107">因此，必須先從資料存放區中取出檔，才能進行授權評估。</span><span class="sxs-lookup"><span data-stu-id="47b36-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="47b36-108">屬性評估會在資料系結之前，以及在執行載入檔的頁面處理常式或動作之前進行。</span><span class="sxs-lookup"><span data-stu-id="47b36-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="47b36-109">基於這些理由，具有 `[Authorize]` 屬性的宣告式授權並不足夠。</span><span class="sxs-lookup"><span data-stu-id="47b36-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="47b36-110">相反地，您可以叫用自訂授權方法，&mdash;稱為*命令式授權*的樣式。</span><span class="sxs-lookup"><span data-stu-id="47b36-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="47b36-111">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="47b36-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
<span data-ttu-id="47b36-112">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="47b36-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="47b36-113">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="47b36-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

<span data-ttu-id="47b36-114">[建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式](xref:security/authorization/secure-data)包含使用以資源為基礎之授權的範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b36-114">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="47b36-115">使用命令式授權</span><span class="sxs-lookup"><span data-stu-id="47b36-115">Use imperative authorization</span></span>

<span data-ttu-id="47b36-116">授權會實作為[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，並在 `Startup` 類別內的服務集合中註冊。</span><span class="sxs-lookup"><span data-stu-id="47b36-116">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="47b36-117">此服務可透過相依性[插入](xref:fundamentals/dependency-injection)頁面處理常式或動作來取得。</span><span class="sxs-lookup"><span data-stu-id="47b36-117">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="47b36-118">`IAuthorizationService` 有兩個 `AuthorizeAsync` 方法多載：一個接受資源和原則名稱，另一個則接受資源，以及要評估的需求清單。</span><span class="sxs-lookup"><span data-stu-id="47b36-118">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="47b36-119">在下列範例中，要保護的資源會載入至自訂的 `Document` 物件。</span><span class="sxs-lookup"><span data-stu-id="47b36-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="47b36-120">系統會叫用 `AuthorizeAsync` 多載，以判斷是否允許目前的使用者編輯提供的檔。</span><span class="sxs-lookup"><span data-stu-id="47b36-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="47b36-121">自訂的 "EditPolicy" 授權原則會納入決定。</span><span class="sxs-lookup"><span data-stu-id="47b36-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="47b36-122">如需建立授權原則的詳細資訊，請參閱[自訂以原則為基礎的授權](xref:security/authorization/policies)。</span><span class="sxs-lookup"><span data-stu-id="47b36-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="47b36-123">下列程式碼範例假設已執行驗證，並設定 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="47b36-123">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="47b36-124">撰寫以資源為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="47b36-124">Write a resource-based handler</span></span>

<span data-ttu-id="47b36-125">針對以資源為基礎的授權撰寫處理常式，與[撰寫單純的需求處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)並沒有太大差異。</span><span class="sxs-lookup"><span data-stu-id="47b36-125">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="47b36-126">建立自訂需求類別，並執行需求處理常式類別。</span><span class="sxs-lookup"><span data-stu-id="47b36-126">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="47b36-127">如需建立需求類別的詳細資訊，請參閱[需求](xref:security/authorization/policies#requirements)。</span><span class="sxs-lookup"><span data-stu-id="47b36-127">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="47b36-128">處理常式類別會同時指定需求和資源類型。</span><span class="sxs-lookup"><span data-stu-id="47b36-128">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="47b36-129">例如，利用 `SameAuthorRequirement` 和 `Document` 資源的處理常式如下：</span><span class="sxs-lookup"><span data-stu-id="47b36-129">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="47b36-130">在上述範例中，假設 `SameAuthorRequirement` 是更泛型 `SpecificAuthorRequirement` 類別的特殊案例。</span><span class="sxs-lookup"><span data-stu-id="47b36-130">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="47b36-131">`SpecificAuthorRequirement` 類別（未顯示）包含代表作者名稱的 `Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="47b36-131">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="47b36-132">`Name` 屬性可以設定為目前的使用者。</span><span class="sxs-lookup"><span data-stu-id="47b36-132">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="47b36-133">在 `Startup.ConfigureServices`中註冊需求和處理常式：</span><span class="sxs-lookup"><span data-stu-id="47b36-133">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=4-8,10)]
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/2_2/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

### <a name="operational-requirements"></a><span data-ttu-id="47b36-134">操作需求</span><span class="sxs-lookup"><span data-stu-id="47b36-134">Operational requirements</span></span>

<span data-ttu-id="47b36-135">如果您要根據 CRUD （建立、讀取、更新、刪除）作業的結果進行決策，請使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper 類別。</span><span class="sxs-lookup"><span data-stu-id="47b36-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="47b36-136">這個類別可讓您針對每個作業類型撰寫單一處理程式，而不是個別的類別。</span><span class="sxs-lookup"><span data-stu-id="47b36-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="47b36-137">若要使用它，請提供一些作業名稱：</span><span class="sxs-lookup"><span data-stu-id="47b36-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="47b36-138">處理常式會依照下列方式，使用 `OperationAuthorizationRequirement` 需求和 `Document` 資源來執行：</span><span class="sxs-lookup"><span data-stu-id="47b36-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

 ::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="47b36-139">上述處理常式會使用資源、使用者的身分識別和需求的 `Name` 屬性來驗證作業。</span><span class="sxs-lookup"><span data-stu-id="47b36-139">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="47b36-140">操作資源處理常式的挑戰和禁止</span><span class="sxs-lookup"><span data-stu-id="47b36-140">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="47b36-141">本節說明如何處理挑戰和禁止動作結果，以及挑戰和禁止的差異。</span><span class="sxs-lookup"><span data-stu-id="47b36-141">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="47b36-142">若要呼叫操作資源處理常式，請在頁面處理常式或動作中叫用 `AuthorizeAsync` 時指定作業。</span><span class="sxs-lookup"><span data-stu-id="47b36-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="47b36-143">下列範例會決定是否允許已驗證的使用者查看提供的檔。</span><span class="sxs-lookup"><span data-stu-id="47b36-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="47b36-144">下列程式碼範例假設已執行驗證，並設定 `User` 屬性。</span><span class="sxs-lookup"><span data-stu-id="47b36-144">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="47b36-145">如果授權成功，則會傳回用於查看檔的頁面。</span><span class="sxs-lookup"><span data-stu-id="47b36-145">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="47b36-146">如果授權失敗，但使用者已通過驗證，則傳回 `ForbidResult` 會通知任何驗證中介軟體授權失敗。</span><span class="sxs-lookup"><span data-stu-id="47b36-146">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="47b36-147">當必須執行驗證時，會傳回 `ChallengeResult`。</span><span class="sxs-lookup"><span data-stu-id="47b36-147">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="47b36-148">若為互動式瀏覽器用戶端，將使用者重新導向至登入頁面可能是適當的方式。</span><span class="sxs-lookup"><span data-stu-id="47b36-148">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="47b36-149">如果授權成功，則會傳回檔的視圖。</span><span class="sxs-lookup"><span data-stu-id="47b36-149">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="47b36-150">如果授權失敗，傳回 `ChallengeResult` 會通知任何驗證中介軟體，授權失敗，中介軟體可以接受適當的回應。</span><span class="sxs-lookup"><span data-stu-id="47b36-150">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="47b36-151">適當的回應可能會傳回401或403狀態碼。</span><span class="sxs-lookup"><span data-stu-id="47b36-151">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="47b36-152">對於互動式瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="47b36-152">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
