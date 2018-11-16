---
title: ASP.NET Core 中的資源為基礎的授權
author: scottaddie
description: 了解如何在 ASP.NET Core 應用程式中實作資源為基礎的授權，Authorize 屬性不敷使用時。
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a163caa26b277fbee6b9d61f8f1d16a60c75b03
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818365"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="73aaa-103">ASP.NET Core 中的資源為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="73aaa-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="73aaa-104">授權策略取決於所存取之資源。</span><span class="sxs-lookup"><span data-stu-id="73aaa-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="73aaa-105">請考慮具有 [作者] 內容的文件。</span><span class="sxs-lookup"><span data-stu-id="73aaa-105">Consider a document that has an author property.</span></span> <span data-ttu-id="73aaa-106">只有作者都可以更新文件。</span><span class="sxs-lookup"><span data-stu-id="73aaa-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="73aaa-107">因此，文件必須從擷取的資料存放區才能進行授權的評估。</span><span class="sxs-lookup"><span data-stu-id="73aaa-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="73aaa-108">資料繫結之前和在頁面處理常式或載入的文件的動作執行之前，就會發生屬性評估。</span><span class="sxs-lookup"><span data-stu-id="73aaa-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="73aaa-109">基於這些理由，使用宣告式授權`[Authorize]`屬性不敷使用。</span><span class="sxs-lookup"><span data-stu-id="73aaa-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="73aaa-110">相反地，您可以叫用自訂的授權方法&mdash;又稱為樣式*命令式授權*。</span><span class="sxs-lookup"><span data-stu-id="73aaa-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="73aaa-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="73aaa-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="73aaa-112">[建立 ASP.NET Core 應用程式與受保護的授權的使用者資料](xref:security/authorization/secure-data)包含範例應用程式使用資源為基礎的授權。</span><span class="sxs-lookup"><span data-stu-id="73aaa-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="73aaa-113">使用命令式的授權</span><span class="sxs-lookup"><span data-stu-id="73aaa-113">Use imperative authorization</span></span>

<span data-ttu-id="73aaa-114">授權會實作成[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，且會註冊內的服務集合中`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="73aaa-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="73aaa-115">服務可透過[相依性插入](xref:fundamentals/dependency-injection)頁面處理常式或動作。</span><span class="sxs-lookup"><span data-stu-id="73aaa-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="73aaa-116">`IAuthorizationService` 有兩個`AuthorizeAsync`方法多載︰ 一個接受資源的原則名稱和其他接受資源和一份需求來評估。</span><span class="sxs-lookup"><span data-stu-id="73aaa-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="73aaa-117">在下列範例中，要保護的資源載入自訂`Document`物件。</span><span class="sxs-lookup"><span data-stu-id="73aaa-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="73aaa-118">`AuthorizeAsync`多載會叫用來判斷目前使用者是否可以編輯提供的文件。</span><span class="sxs-lookup"><span data-stu-id="73aaa-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="73aaa-119">自訂的 「 EditPolicy 「 授權原則會納入決策。</span><span class="sxs-lookup"><span data-stu-id="73aaa-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="73aaa-120">請參閱[自訂原則為基礎的授權](xref:security/authorization/policies)如需建立授權原則的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="73aaa-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="73aaa-121">下列範例假設已在執行驗證的程式碼及組`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="73aaa-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="73aaa-122">撰寫資源為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="73aaa-122">Write a resource-based handler</span></span>

<span data-ttu-id="73aaa-123">資源為基礎的授權並不太大差異比撰寫處理常式[撰寫的一般需求處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)。</span><span class="sxs-lookup"><span data-stu-id="73aaa-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="73aaa-124">建立自訂需求類別，並實作需求處理常式類別。</span><span class="sxs-lookup"><span data-stu-id="73aaa-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="73aaa-125">如需有關建立需求類別的詳細資訊，請參閱[需求](xref:security/authorization/policies#requirements)。</span><span class="sxs-lookup"><span data-stu-id="73aaa-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="73aaa-126">處理常式類別指定需求和資源類型。</span><span class="sxs-lookup"><span data-stu-id="73aaa-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="73aaa-127">例如，處理常式利用`SameAuthorRequirement`和`Document`資源如下所示：</span><span class="sxs-lookup"><span data-stu-id="73aaa-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="73aaa-128">在上述範例中，您可想像一下`SameAuthorRequirement`是特殊案例的更多一般`SpecificAuthorRequirement`類別。</span><span class="sxs-lookup"><span data-stu-id="73aaa-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="73aaa-129">`SpecificAuthorRequirement`類別 （未顯示） 包含`Name`屬性代表作者的名稱。</span><span class="sxs-lookup"><span data-stu-id="73aaa-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="73aaa-130">`Name`屬性無法設定為目前的使用者。</span><span class="sxs-lookup"><span data-stu-id="73aaa-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="73aaa-131">註冊處理常式中的與需求`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="73aaa-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="73aaa-132">作業的需求</span><span class="sxs-lookup"><span data-stu-id="73aaa-132">Operational requirements</span></span>

<span data-ttu-id="73aaa-133">如果您這樣做決策的 CRUD （建立、 讀取、 更新、 刪除） 作業的結果為基礎，使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)協助程式類別。</span><span class="sxs-lookup"><span data-stu-id="73aaa-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="73aaa-134">這個類別可讓您撰寫單一的處理常式，而不是個別的類別，每個作業類型。</span><span class="sxs-lookup"><span data-stu-id="73aaa-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="73aaa-135">若要使用它，提供一些作業名稱：</span><span class="sxs-lookup"><span data-stu-id="73aaa-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="73aaa-136">處理常式的使用，如下所示，實作`OperationAuthorizationRequirement`需求和`Document`資源：</span><span class="sxs-lookup"><span data-stu-id="73aaa-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="73aaa-137">上述的處理常式會驗證作業使用的資源、 使用者的身分識別和需求之`Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="73aaa-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="73aaa-138">若要呼叫的作業資源的處理常式，指定的作業時叫用`AuthorizeAsync`頁面處理常式或動作中。</span><span class="sxs-lookup"><span data-stu-id="73aaa-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="73aaa-139">下列範例會判斷是否允許已驗證的使用者檢視提供的文件。</span><span class="sxs-lookup"><span data-stu-id="73aaa-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="73aaa-140">下列範例假設已在執行驗證的程式碼及組`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="73aaa-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="73aaa-141">如果授權成功，則會傳回檢視文件的頁面。</span><span class="sxs-lookup"><span data-stu-id="73aaa-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="73aaa-142">如果授權失敗，但使用者驗證時，傳回`ForbidResult`會通知任何授權失敗的驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="73aaa-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="73aaa-143">A`ChallengeResult`必須執行驗證時，會傳回。</span><span class="sxs-lookup"><span data-stu-id="73aaa-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="73aaa-144">互動式的瀏覽器用戶端，可能適合將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="73aaa-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="73aaa-145">如果授權成功，則會傳回文件的檢視。</span><span class="sxs-lookup"><span data-stu-id="73aaa-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="73aaa-146">如果授權失敗，傳回`ChallengeResult`授權失敗，，和中介軟體可以採取適當的回應會通知任何驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="73aaa-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="73aaa-147">適當的回應可能會傳回 401 或 403 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="73aaa-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="73aaa-148">互動式的瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="73aaa-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
