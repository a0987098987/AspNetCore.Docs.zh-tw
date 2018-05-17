---
title: 在 ASP.NET Core 資源為基礎的授權
author: scottaddie
description: 了解如何授權屬性不敷使用時，ASP.NET Core 應用程式中實作資源為基礎的授權。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 3be2d9b9aef18763fbdba78e044dd6b68ddcef0c
ms.sourcegitcommit: 9bc34b8269d2a150b844c3b8646dcb30278a95ea
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/12/2018
---
# <a name="resource-based-authorization-in-aspnet-core"></a>在 ASP.NET Core 資源為基礎的授權

授權策略取決於所存取的資源。 請考慮具有 author 屬性的文件。 只有作者可更新的文件。 因此，在文件之前必須擷取從資料存放區授權評估可能會發生。

資料繫結之前以及執行網頁處理常式或動作，它會載入文件之前，就會出現屬性評估。 基於這些理由，宣告式授權搭配`[Authorize]`屬性不敷使用。 相反地，您可以叫用自訂授權方法&mdash;樣式，稱為必要授權。

使用[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 來瀏覽本主題中所述的功能。

[建立 ASP.NET Core 應用程式與受保護的授權的使用者資料](xref:security/authorization/secure-data)包含範例應用程式使用的資源為基礎的授權。

## <a name="use-imperative-authorization"></a>使用必要的授權

授權會實作為[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，且會註冊內的服務集合中`Startup`類別。 服務可透過[相依性插入](xref:fundamentals/dependency-injection#fundamentals-dependency-injection)網頁處理常式或動作。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` 有兩個`AuthorizeAsync`方法多載： 一個接受資源和原則名稱以及其他資源，以及一份需求來評估所接受。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

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

下列範例中，在要保護的資源載入自訂`Document`物件。 `AuthorizeAsync`多載會叫用來判斷目前使用者是否可以編輯提供的文件。 自訂的 「 EditPolicy 「 授權原則會納入決策。 請參閱[自訂原則為基礎的授權](xref:security/authorization/policies)如需有關建立授權原則。

> [!NOTE]
> 下列程式碼範例假設已執行驗證，以及組`User`屬性。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>寫入資源為基礎的處理常式

撰寫處理常式的資源為基礎的授權沒有很多不同[撰寫一般需求的處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)。 建立自訂需求類別，並實作需求的處理常式類別。 此處理常式類別會指定需求和資源類型。 例如，處理常式利用`SameAuthorRequirement`需求和`Document`資源看起來像這樣：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

註冊需求和處理常式中的`Startup.ConfigureServices`方法：

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>操作需求

如果您正在根據 CRUD （建立、 讀取、 更新、 刪除） 作業的結果，使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)協助程式類別。 此類別可讓您撰寫每個作業類型的單一處理常式，而不是個別的類別。 若要使用它，提供一些作業名稱：

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

此處理常式，如下所示，使用實作`OperationAuthorizationRequirement`需求和`Document`資源：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

上述的處理常式驗證作業使用的資源、 使用者的身分識別和需求之`Name`屬性。

若要呼叫的作業資源的處理常式，指定的作業時叫用`AuthorizeAsync`網頁處理常式或動作中。 下列範例會判斷是否允許已驗證的使用者檢視提供的文件。

> [!NOTE]
> 下列程式碼範例假設已執行驗證，以及組`User`屬性。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

如果授權成功，則會傳回檢視文件的頁面。 如果授權失敗，但使用者進行驗證時，傳回`ForbidResult`通知授權失敗的任何驗證中介軟體。 A`ChallengeResult`必須執行驗證時，會傳回。 互動式瀏覽器用戶端，可能適合將使用者重新導向至登入頁面。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

如果授權成功，則會傳回文件的檢視。 如果授權失敗，傳回`ChallengeResult`告知任何驗證中介軟體，，授權失敗，且中介軟體可以採取適當的回應。 適當的回應可以傳回 401 或 403 狀態碼。 互動式瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。

---
