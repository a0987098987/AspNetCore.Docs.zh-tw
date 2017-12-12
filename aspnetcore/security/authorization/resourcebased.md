---
title: "在 ASP.NET Core 資源為基礎的授權"
author: scottaddie
description: "了解如何授權屬性不敷使用時，ASP.NET Core 應用程式中實作資源為基礎的授權。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 708f306da740870b106cbeeb96879480f8745439
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="d2b23-103">資源為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="d2b23-103">Resource-based authorization</span></span>

<span data-ttu-id="d2b23-104">作者：[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="d2b23-104">By [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="d2b23-105">授權策略取決於所存取的資源。</span><span class="sxs-lookup"><span data-stu-id="d2b23-105">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="d2b23-106">請考慮具有 author 屬性的文件。</span><span class="sxs-lookup"><span data-stu-id="d2b23-106">Consider a document which has an author property.</span></span> <span data-ttu-id="d2b23-107">只有作者可更新的文件。</span><span class="sxs-lookup"><span data-stu-id="d2b23-107">Only the author is allowed to update the document.</span></span> <span data-ttu-id="d2b23-108">因此，在文件之前必須擷取從資料存放區授權評估可能會發生。</span><span class="sxs-lookup"><span data-stu-id="d2b23-108">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="d2b23-109">資料繫結之前以及執行網頁處理常式或動作，它會載入文件之前，就會出現屬性評估。</span><span class="sxs-lookup"><span data-stu-id="d2b23-109">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="d2b23-110">基於這些理由，宣告式授權搭配`[Authorize]`屬性不敷使用。</span><span class="sxs-lookup"><span data-stu-id="d2b23-110">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="d2b23-111">相反地，您可以叫用自訂授權方法&mdash;樣式，稱為必要授權。</span><span class="sxs-lookup"><span data-stu-id="d2b23-111">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="d2b23-112">使用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 來瀏覽本主題中所述的功能。</span><span class="sxs-lookup"><span data-stu-id="d2b23-112">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="d2b23-113">使用必要的授權</span><span class="sxs-lookup"><span data-stu-id="d2b23-113">Use imperative authorization</span></span>

<span data-ttu-id="d2b23-114">授權會實作為[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，且會註冊內的服務集合中`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="d2b23-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="d2b23-115">服務可透過[相依性插入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)網頁處理常式或動作。</span><span class="sxs-lookup"><span data-stu-id="d2b23-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="d2b23-116">`IAuthorizationService`有兩個`AuthorizeAsync`方法多載： 一個接受資源和原則名稱以及其他資源，以及一份需求來評估所接受。</span><span class="sxs-lookup"><span data-stu-id="d2b23-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2b23-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2b23-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="d2b23-119">下列範例中，在要保護的資源載入自訂`Document`物件。</span><span class="sxs-lookup"><span data-stu-id="d2b23-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="d2b23-120">`AuthorizeAsync`多載會叫用來判斷目前使用者是否可以編輯提供的文件。</span><span class="sxs-lookup"><span data-stu-id="d2b23-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="d2b23-121">自訂的 「 EditPolicy 「 授權原則會納入決策。</span><span class="sxs-lookup"><span data-stu-id="d2b23-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="d2b23-122">請參閱[自訂原則為基礎的授權](xref:security/authorization/policies)如需有關建立授權原則。</span><span class="sxs-lookup"><span data-stu-id="d2b23-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="d2b23-123">下列程式碼範例假設已執行驗證，以及組`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2b23-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2b23-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2b23-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="d2b23-126">寫入資源為基礎的處理常式</span><span class="sxs-lookup"><span data-stu-id="d2b23-126">Write a resource-based handler</span></span>

<span data-ttu-id="d2b23-127">撰寫處理常式的資源為基礎的授權沒有很多不同[撰寫一般需求的處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)。</span><span class="sxs-lookup"><span data-stu-id="d2b23-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="d2b23-128">建立自訂需求類別，並實作需求的處理常式類別。</span><span class="sxs-lookup"><span data-stu-id="d2b23-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="d2b23-129">此處理常式類別會指定需求和資源類型。</span><span class="sxs-lookup"><span data-stu-id="d2b23-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="d2b23-130">例如，處理常式利用`SameAuthorRequirement`需求和`Document`資源看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d2b23-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2b23-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2b23-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="d2b23-133">註冊需求和處理常式中的`Startup.ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="d2b23-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="d2b23-134">操作需求</span><span class="sxs-lookup"><span data-stu-id="d2b23-134">Operational requirements</span></span>

<span data-ttu-id="d2b23-135">如果您正在根據結果的 CRUD (**C**建立， **R**頭， **U**更新， **D**elete) 作業，請使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)協助程式類別。</span><span class="sxs-lookup"><span data-stu-id="d2b23-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="d2b23-136">此類別可讓您撰寫每個作業類型的單一處理常式，而不是個別的類別。</span><span class="sxs-lookup"><span data-stu-id="d2b23-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="d2b23-137">若要使用它，提供一些作業名稱：</span><span class="sxs-lookup"><span data-stu-id="d2b23-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="d2b23-138">此處理常式，如下所示，使用實作`OperationAuthorizationRequirement`需求和`Document`資源：</span><span class="sxs-lookup"><span data-stu-id="d2b23-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2b23-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2b23-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="d2b23-141">上述的處理常式驗證作業使用的資源、 使用者的身分識別和需求之`Name`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2b23-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="d2b23-142">若要呼叫的作業資源的處理常式，指定的作業時叫用`AuthorizeAsync`網頁處理常式或動作中。</span><span class="sxs-lookup"><span data-stu-id="d2b23-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="d2b23-143">下列範例會判斷是否允許已驗證的使用者檢視提供的文件。</span><span class="sxs-lookup"><span data-stu-id="d2b23-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="d2b23-144">下列程式碼範例假設已執行驗證，以及組`User`屬性。</span><span class="sxs-lookup"><span data-stu-id="d2b23-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d2b23-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="d2b23-146">如果授權成功，則會傳回檢視文件的頁面。</span><span class="sxs-lookup"><span data-stu-id="d2b23-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="d2b23-147">如果授權失敗，但使用者進行驗證時，傳回`ForbidResult`通知授權失敗的任何驗證中介軟體。</span><span class="sxs-lookup"><span data-stu-id="d2b23-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="d2b23-148">A`ChallengeResult`必須執行驗證時，會傳回。</span><span class="sxs-lookup"><span data-stu-id="d2b23-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="d2b23-149">互動式瀏覽器用戶端，可能適合將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="d2b23-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d2b23-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d2b23-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="d2b23-151">如果授權成功，則會傳回文件的檢視。</span><span class="sxs-lookup"><span data-stu-id="d2b23-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="d2b23-152">如果授權失敗，傳回`ChallengeResult`告知任何驗證中介軟體，，授權失敗，且中介軟體可以採取適當的回應。</span><span class="sxs-lookup"><span data-stu-id="d2b23-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="d2b23-153">適當的回應可以傳回 401 或 403 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="d2b23-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="d2b23-154">互動式瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="d2b23-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
