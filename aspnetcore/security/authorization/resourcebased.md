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
# <a name="resource-based-authorization-in-aspnet-core"></a>ASP.NET Core 中以資源為基礎的授權

授權策略會根據所存取的資源而定。 假設有一個具有 author 屬性的檔。 只有作者可以更新檔。 因此，必須先從資料存放區中取出檔，才能進行授權評估。

屬性評估會在資料系結之前，以及在執行載入檔的頁面處理常式或動作之前進行。 基於這些理由，具有 `[Authorize]` 屬性的宣告式授權並不足夠。 相反地，您可以叫用自訂授權方法，&mdash;稱為*命令式授權*的樣式。

::: moniker range=">= aspnetcore-3.0"
[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([如何下載](xref:index#how-to-download-a-sample))。
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([如何下載](xref:index#how-to-download-a-sample))。
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([如何下載](xref:index#how-to-download-a-sample))。
::: moniker-end

[建立具有受授權保護之使用者資料的 ASP.NET Core 應用程式](xref:security/authorization/secure-data)包含使用以資源為基礎之授權的範例應用程式。

## <a name="use-imperative-authorization"></a>使用命令式授權

授權會實作為[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，並在 `Startup` 類別內的服務集合中註冊。 此服務可透過相依性[插入](xref:fundamentals/dependency-injection)頁面處理常式或動作來取得。

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` 有兩個 `AuthorizeAsync` 方法多載：一個接受資源和原則名稱，另一個則接受資源，以及要評估的需求清單。

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

在下列範例中，要保護的資源會載入至自訂的 `Document` 物件。 系統會叫用 `AuthorizeAsync` 多載，以判斷是否允許目前的使用者編輯提供的檔。 自訂的 "EditPolicy" 授權原則會納入決定。 如需建立授權原則的詳細資訊，請參閱[自訂以原則為基礎的授權](xref:security/authorization/policies)。

> [!NOTE]
> 下列程式碼範例假設已執行驗證，並設定 `User` 屬性。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>撰寫以資源為基礎的處理常式

針對以資源為基礎的授權撰寫處理常式，與[撰寫單純的需求處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)並沒有太大差異。 建立自訂需求類別，並執行需求處理常式類別。 如需建立需求類別的詳細資訊，請參閱[需求](xref:security/authorization/policies#requirements)。

處理常式類別會同時指定需求和資源類型。 例如，利用 `SameAuthorRequirement` 和 `Document` 資源的處理常式如下：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

在上述範例中，假設 `SameAuthorRequirement` 是更泛型 `SpecificAuthorRequirement` 類別的特殊案例。 `SpecificAuthorRequirement` 類別（未顯示）包含代表作者名稱的 `Name` 屬性。 `Name` 屬性可以設定為目前的使用者。

在 `Startup.ConfigureServices`中註冊需求和處理常式：

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=4-8,10)]
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/2_2/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

### <a name="operational-requirements"></a>操作需求

如果您要根據 CRUD （建立、讀取、更新、刪除）作業的結果進行決策，請使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper 類別。 這個類別可讓您針對每個作業類型撰寫單一處理程式，而不是個別的類別。 若要使用它，請提供一些作業名稱：

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

處理常式會依照下列方式，使用 `OperationAuthorizationRequirement` 需求和 `Document` 資源來執行：

 ::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

上述處理常式會使用資源、使用者的身分識別和需求的 `Name` 屬性來驗證作業。

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a>操作資源處理常式的挑戰和禁止

本節說明如何處理挑戰和禁止動作結果，以及挑戰和禁止的差異。

若要呼叫操作資源處理常式，請在頁面處理常式或動作中叫用 `AuthorizeAsync` 時指定作業。 下列範例會決定是否允許已驗證的使用者查看提供的檔。

> [!NOTE]
> 下列程式碼範例假設已執行驗證，並設定 `User` 屬性。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

如果授權成功，則會傳回用於查看檔的頁面。 如果授權失敗，但使用者已通過驗證，則傳回 `ForbidResult` 會通知任何驗證中介軟體授權失敗。 當必須執行驗證時，會傳回 `ChallengeResult`。 若為互動式瀏覽器用戶端，將使用者重新導向至登入頁面可能是適當的方式。

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

如果授權成功，則會傳回檔的視圖。 如果授權失敗，傳回 `ChallengeResult` 會通知任何驗證中介軟體，授權失敗，中介軟體可以接受適當的回應。 適當的回應可能會傳回401或403狀態碼。 對於互動式瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。

::: moniker-end
