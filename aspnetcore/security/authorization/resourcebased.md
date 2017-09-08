---
title: "以資源為基礎的授權"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a>以資源為基礎的授權

<a name=security-authorization-resource-based></a>

通常授權存取的資源而定。 例如文件可能 author 屬性。 文件作者會允許進行更新，所以後才能進行授權評估，必須從 文件儲存機制載入資源。 無法完成具有授權屬性，因為屬性評估會發生資料繫結之前和動作中執行您自己的程式碼載入的資源之前。 而不是宣告式授權屬性方法中，我們必須使用必要的授權，其中的開發人員呼叫他們自己的程式碼中的授權函式。

## <a name="authorizing-within-your-code"></a>在程式碼中的授權

為服務時，實作授權`IAuthorizationService`服務集合中已註冊且可透過使用[相依性插入](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection)控制站進行存取。

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

`IAuthorizationService`有兩個方法，其中一個您傳遞的資源和原則名稱和其他您傳遞的資源和需求來評估的清單。

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

您的資源，您的動作內呼叫服務負載然後呼叫`AuthorizeAsync`您需要的多載。 例如

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

## <a name="writing-a-resource-based-handler"></a>撰寫基礎資源的處理常式

寫入資源基礎授權的處理常式沒有很多不同的 to[撰寫一般需求的處理常式](policies.md#security-authorization-policies-based-authorization-handler)。 建立一項規定，，然後實作需求的處理常式指定之需求之前以及資源類型。 比方說，這可能會接受文件資源的處理常式可能看起來如下。

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

不要忘記您也需要註冊您的處理常式中`ConfigureServices`方法。

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>操作需求

如果您要進行作業，例如讀取、 寫入、 更新和刪除的決策，您可以使用`OperationAuthorizationRequirement`類別`Microsoft.AspNetCore.Authorization.Infrastructure`命名空間。 這個預先建立的需求類別可讓您撰寫具有參數化的作業名稱的單一處理常式，而不要建立個別的類別，每個作業。 若要使用它會提供一些作業名稱：

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

您的處理常式無法再使用來實作，如下所示，假設`Document`類別做為資源。

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

您可以看到處理常式運作上`OperationAuthorizationRequirement`。 內部處理常式的程式碼進行其評估時，必須採取提供需求納入考量的 Name 屬性。

若要呼叫您要呼叫時，指定作業的作業資源處理常式`AuthorizeAsync`在您的動作。 例如

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

這個範例會檢查使用者是否能夠執行目前的 「 讀取 」 操作`document`執行個體。 如果授權成功，將會傳回文件的檢視。 如果授權會失敗並傳回`ChallengeResult`會通知所有驗證中介軟體授權失敗，且中介軟體可以採取適當的回應，例如傳回 401 或 403 狀態碼，或將使用者重新導向至登入頁面互動式瀏覽器用戶端。
