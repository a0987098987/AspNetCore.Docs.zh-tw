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
# <a name="resource-based-authorization-in-aspnet-core"></a>ASP.NET Core 中的資源為基礎的授權

授權策略取決於所存取之資源。 請考慮具有 [作者] 內容的文件。 只有作者都可以更新文件。 因此，文件必須從擷取的資料存放區才能進行授權的評估。

資料繫結之前和在頁面處理常式或載入的文件的動作執行之前，就會發生屬性評估。 基於這些理由，使用宣告式授權`[Authorize]`屬性不敷使用。 相反地，您可以叫用自訂的授權方法&mdash;又稱為樣式*命令式授權*。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([如何下載](xref:index#how-to-download-a-sample))。

[建立 ASP.NET Core 應用程式與受保護的授權的使用者資料](xref:security/authorization/secure-data)包含範例應用程式使用資源為基礎的授權。

## <a name="use-imperative-authorization"></a>使用命令式的授權

授權會實作成[IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice)服務，且會註冊內的服務集合中`Startup`類別。 服務可透過[相依性插入](xref:fundamentals/dependency-injection)頁面處理常式或動作。

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` 有兩個`AuthorizeAsync`方法多載︰ 一個接受資源的原則名稱和其他接受資源和一份需求來評估。

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

在下列範例中，要保護的資源載入自訂`Document`物件。 `AuthorizeAsync`多載會叫用來判斷目前使用者是否可以編輯提供的文件。 自訂的 「 EditPolicy 「 授權原則會納入決策。 請參閱[自訂原則為基礎的授權](xref:security/authorization/policies)如需建立授權原則的詳細資訊。

> [!NOTE]
> 下列範例假設已在執行驗證的程式碼及組`User`屬性。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>撰寫資源為基礎的處理常式

資源為基礎的授權並不太大差異比撰寫處理常式[撰寫的一般需求處理常式](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler)。 建立自訂需求類別，並實作需求處理常式類別。 如需有關建立需求類別的詳細資訊，請參閱[需求](xref:security/authorization/policies#requirements)。

處理常式類別指定需求和資源類型。 例如，處理常式利用`SameAuthorRequirement`和`Document`資源如下所示：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

在上述範例中，您可想像一下`SameAuthorRequirement`是特殊案例的更多一般`SpecificAuthorRequirement`類別。 `SpecificAuthorRequirement`類別 （未顯示） 包含`Name`屬性代表作者的名稱。 `Name`屬性無法設定為目前的使用者。

註冊處理常式中的與需求`Startup.ConfigureServices`:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>作業的需求

如果您這樣做決策的 CRUD （建立、 讀取、 更新、 刪除） 作業的結果為基礎，使用[OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement)協助程式類別。 這個類別可讓您撰寫單一的處理常式，而不是個別的類別，每個作業類型。 若要使用它，提供一些作業名稱：

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

處理常式的使用，如下所示，實作`OperationAuthorizationRequirement`需求和`Document`資源：

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

上述的處理常式會驗證作業使用的資源、 使用者的身分識別和需求之`Name`屬性。

若要呼叫的作業資源的處理常式，指定的作業時叫用`AuthorizeAsync`頁面處理常式或動作中。 下列範例會判斷是否允許已驗證的使用者檢視提供的文件。

> [!NOTE]
> 下列範例假設已在執行驗證的程式碼及組`User`屬性。

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

如果授權成功，則會傳回檢視文件的頁面。 如果授權失敗，但使用者驗證時，傳回`ForbidResult`會通知任何授權失敗的驗證中介軟體。 A`ChallengeResult`必須執行驗證時，會傳回。 互動式的瀏覽器用戶端，可能適合將使用者重新導向至登入頁面。

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

如果授權成功，則會傳回文件的檢視。 如果授權失敗，傳回`ChallengeResult`授權失敗，，和中介軟體可以採取適當的回應會通知任何驗證中介軟體。 適當的回應可能會傳回 401 或 403 狀態碼。 互動式的瀏覽器用戶端，這可能表示將使用者重新導向至登入頁面。

::: moniker-end
