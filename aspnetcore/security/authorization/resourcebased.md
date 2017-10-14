---
title: "以資源為基礎的授權"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: d3575619c53e77dadc293ea2bb7dc72501a8a1e3
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/13/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="736b2-103">以資源為基礎的授權</span><span class="sxs-lookup"><span data-stu-id="736b2-103">Resource Based Authorization</span></span>

<a name="security-authorization-resource-based"></a>

<span data-ttu-id="736b2-104">通常授權存取的資源而定。</span><span class="sxs-lookup"><span data-stu-id="736b2-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="736b2-105">例如，文件可能會有 author 屬性。</span><span class="sxs-lookup"><span data-stu-id="736b2-105">For example, a document may have an author property.</span></span> <span data-ttu-id="736b2-106">文件作者會允許進行更新，所以後才能進行授權評估，必須從 文件儲存機制載入資源。</span><span class="sxs-lookup"><span data-stu-id="736b2-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="736b2-107">無法完成具有授權屬性，因為屬性評估會發生資料繫結之前和動作中執行您自己的程式碼載入的資源之前。</span><span class="sxs-lookup"><span data-stu-id="736b2-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="736b2-108">而不是宣告式授權屬性方法中，我們必須使用必要的授權，其中的開發人員呼叫他們自己的程式碼中的授權函式。</span><span class="sxs-lookup"><span data-stu-id="736b2-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="736b2-109">在程式碼中的授權</span><span class="sxs-lookup"><span data-stu-id="736b2-109">Authorizing within your code</span></span>

<span data-ttu-id="736b2-110">為服務時，實作授權`IAuthorizationService`服務集合中已註冊且可透過使用[相依性插入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)控制站進行存取。</span><span class="sxs-lookup"><span data-stu-id="736b2-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

<span data-ttu-id="736b2-111">`IAuthorizationService`有兩個方法，其中一個您傳遞的資源和原則名稱和其他您傳遞的資源和需求來評估的清單。</span><span class="sxs-lookup"><span data-stu-id="736b2-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name="security-authorization-resource-based-imperative"></a>

<span data-ttu-id="736b2-112">若要呼叫服務，載入您的資源，您的動作內再呼叫`AuthorizeAsync`您需要的多載。</span><span class="sxs-lookup"><span data-stu-id="736b2-112">To call the service, load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="736b2-113">例如: </span><span class="sxs-lookup"><span data-stu-id="736b2-113">For example:</span></span>

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="736b2-114">撰寫基礎資源的處理常式</span><span class="sxs-lookup"><span data-stu-id="736b2-114">Writing a resource based handler</span></span>

<span data-ttu-id="736b2-115">寫入資源基礎授權的處理常式沒有很多不同的 to[撰寫一般需求的處理常式](policies.md#security-authorization-policies-based-authorization-handler)。</span><span class="sxs-lookup"><span data-stu-id="736b2-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="736b2-116">建立一項規定，，然後實作需求的處理常式指定之需求之前以及資源類型。</span><span class="sxs-lookup"><span data-stu-id="736b2-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="736b2-117">比方說，這可能會接受文件資源的處理常式將如下所示：</span><span class="sxs-lookup"><span data-stu-id="736b2-117">For example, a handler which might accept a Document resource would look as follows:</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="736b2-118">不要忘記您也需要註冊您的處理常式中`ConfigureServices`方法：</span><span class="sxs-lookup"><span data-stu-id="736b2-118">Don't forget you also need to register your handler in the `ConfigureServices` method:</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="736b2-119">操作需求</span><span class="sxs-lookup"><span data-stu-id="736b2-119">Operational Requirements</span></span>

<span data-ttu-id="736b2-120">如果您要進行作業，例如讀取、 寫入、 更新和刪除的決策，您可以使用`OperationAuthorizationRequirement`類別`Microsoft.AspNetCore.Authorization.Infrastructure`命名空間。</span><span class="sxs-lookup"><span data-stu-id="736b2-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="736b2-121">這個預先建立的需求類別可讓您撰寫具有參數化的作業名稱的單一處理常式，而不要建立個別的類別，每個作業。</span><span class="sxs-lookup"><span data-stu-id="736b2-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="736b2-122">若要使用它，提供一些作業名稱：</span><span class="sxs-lookup"><span data-stu-id="736b2-122">To use it, provide some operation names:</span></span>

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

<span data-ttu-id="736b2-123">您的處理常式無法再使用來實作，如下所示，假設`Document`類別做為資源：</span><span class="sxs-lookup"><span data-stu-id="736b2-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource:</span></span>

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="736b2-124">您可以看到處理常式運作上`OperationAuthorizationRequirement`。</span><span class="sxs-lookup"><span data-stu-id="736b2-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="736b2-125">內部處理常式的程式碼進行其評估時，必須採取提供需求納入考量的 Name 屬性。</span><span class="sxs-lookup"><span data-stu-id="736b2-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="736b2-126">若要呼叫您要呼叫時，指定作業的作業資源處理常式`AuthorizeAsync`在您的動作。</span><span class="sxs-lookup"><span data-stu-id="736b2-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="736b2-127">例如: </span><span class="sxs-lookup"><span data-stu-id="736b2-127">For example:</span></span>

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

<span data-ttu-id="736b2-128">這個範例會檢查使用者是否能夠執行目前的 「 讀取 」 操作`document`執行個體。</span><span class="sxs-lookup"><span data-stu-id="736b2-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="736b2-129">如果授權成功，將會傳回文件的檢視。</span><span class="sxs-lookup"><span data-stu-id="736b2-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="736b2-130">如果授權會失敗並傳回`ChallengeResult`會通知所有驗證中介軟體授權失敗，且中介軟體可以採取適當的回應，例如傳回 401 或 403 狀態碼，或將使用者重新導向至登入頁面互動式瀏覽器用戶端。</span><span class="sxs-lookup"><span data-stu-id="736b2-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
