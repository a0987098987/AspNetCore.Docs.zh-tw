---
title: 在 ASP.NET Core 原則為基礎的授權
author: rick-anderson
description: 了解如何建立和使用授權原則的處理常式來強制執行的 ASP.NET Core 應用程式中的授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 81ca6ad9ddba3de094762f5608bb6a5719bca7a1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277978"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>在 ASP.NET Core 原則為基礎的授權

基本上，[角色為基礎的授權](xref:security/authorization/roles)和[宣告型授權](xref:security/authorization/claims)使用需求、 要求處理常式和預先設定的原則。 這些建置組塊支援程式碼中的授權評估的運算式。 結果會是更豐富、 可重複使用、 可測試性授權結構。

授權原則是由一或多個需求所組成。 在將它註冊為授權服務的組態，`Startup.ConfigureServices`方法：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

在上述範例中，會建立 「 AtLeast21 」 原則。 它有一個需求&mdash;的最低存在時間，提供做為參數的需求。

原則適用於使用`[Authorize]`原則名稱的屬性。 例如: 

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>需求

授權需求是原則可以用來評估目前的使用者主體的資料參數的集合。 在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。 需求實作[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空白標記的介面。 參數化的最低年齡要求實作，如下所示：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> 一項需求不需要將資料或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權的處理常式

授權處理常式負責需求之屬性的評估。 授權的處理常式會評估需求，提供對[AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)來判斷是否允許存取。

可以有一項需求[多個處理常式](#security-authorization-policies-based-multiple-handlers)。 處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中`TRequirement`是要處理的需求。 或者，處理常式可能會實作[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多個類型的需求。

### <a name="use-a-handler-for-one-requirement"></a>使用的一項需求的處理常式

<a name="security-authorization-handler-example"></a>

最短使用期限處理常式會利用一個需求是一對一關聯性的範例如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

上述程式碼會判斷目前的使用者主體已宣告已知且受信任的簽發者所發出的出生日期。 宣告遺漏時，無法進行授權，在此情況下會傳回已完成的工作。 當宣告存在時，會計算使用者的年齡。 如果使用者符合此需求所定義的最低存在時間，授權會被視為成功。 當授權是否成功，`context.Succeed`以滿足需求做為其唯一參數叫用。

### <a name="use-a-handler-for-multiple-requirements"></a>使用多個需求的處理常式

權限的處理常式會利用三個需求的一對多關聯性的範例如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

上述程式碼會周遊[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;標記為成功不包含要求的屬性。 如果使用者具有讀取權限，他或她必須是擁有者或贊助者來存取要求的資源。 如果使用者編輯或刪除權限，他或她必須存取要求之資源的擁有者。 當授權是否成功，`context.Succeed`以滿足需求做為其唯一參數叫用。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理常式註冊

處理常式都登錄在設定期間的服務集合。 例如: 

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

每個處理常式，會叫用加入至服務集合`services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`。

## <a name="what-should-a-handler-return"></a>功能應該處理常式傳回？

請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。 狀態為成功或失敗的方式？

* 處理常式呼叫來表示成功`context.Succeed(IAuthorizationRequirement requirement)`，傳遞需求已順利通過驗證。

* 一般而言，處理失敗，因為可能會成功的相同需求的其他處理常式不需要處理常式。

* 若要保證失敗，即使其他需求的處理常式會成功，呼叫`context.Fail`。

當設定為`false`、 [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性 （可在 ASP.NET Core 1.1 及更新版本） short-circuits 的處理常式執行時`context.Fail`呼叫。 `InvokeHandlersAfterFailure` 預設為`true`，在此情況下會呼叫所有的處理常式。 這可讓以產生副作用，例如記錄，一律會發生需求即使`context.Fail`已經在另一個處理常式中呼叫。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>為什麼需要多個處理常式之需求？

在您想要評估的情況下**或**為基礎，實作多個處理常式為單一的需求。 例如，Microsoft 有門只開啟與索引鍵的卡片。 如果您在家離開您金鑰卡，接線生列印暫存的貼紙，並會為您開啟媒體櫃門。 在此案例中，您必須是單一的必要條件， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

請確定這兩個處理常式會[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。 如果任一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。

## <a name="using-a-func-to-fulfill-a-policy"></a>符合原則中使用函式

可能的情況下在哪些完成原則簡單程式碼來表達。 可提供`Func<AuthorizationHandlerContext, bool>`時設定與原則`RequireAssertion`原則產生器。

例如，先前`BadgeEntryHandler`可以改寫，如下所示：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>在處理常式中存取的 MVC 要求內容

`HandleRequirementAsync`授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`和`TRequirement`所處理。 任何將物件加入至可用的架構，例如 MVC 或 Jabbr`Resource`屬性`AuthorizationHandlerContext`傳遞額外資訊。

MVC 的執行個體的傳遞，例如[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)中`Resource`屬性。 此屬性可存取`HttpContext`， `RouteData`，和 else MVC 和 Razor 頁面所提供的所有項目。

使用`Resource`屬性是特定的架構。 使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。 您應該轉換`Resource`屬性使用`as`關鍵字，然後確認 轉型已成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
