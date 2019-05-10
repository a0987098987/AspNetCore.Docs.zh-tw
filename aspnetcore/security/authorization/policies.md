---
title: ASP.NET Core 中的原則為基礎的授權
author: rick-anderson
description: 了解如何建立和使用授權原則的處理常式，以強制執行的 ASP.NET Core 應用程式中的授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: ea9d687d3810c104d5b3fa39033849c21569709b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891825"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>ASP.NET Core 中的原則為基礎的授權

基本上，[角色為基礎的授權](xref:security/authorization/roles)並[宣告型授權](xref:security/authorization/claims)使用需求，需求處理常式，並預先設定的原則。 這些建置組塊支援程式碼中的授權評估的運算式。 結果會是更豐富、 可重複使用、 可測試的授權結構。

授權原則是由一或多個需求所組成。 它會註冊為授權服務的組態，在`Startup.ConfigureServices`方法：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

在上述範例中，會建立 「 AtLeast21 」 原則。 它有一個需求&mdash;的最短使用期限，做為參數的需求，提供。

## <a name="applying-policies-to-mvc-controllers"></a>將原則套用至 MVC 控制器

如果您使用 Razor 頁面，請參閱[將原則套用至 Razor Pages](#applying-policies-to-razor-pages)本文件中。

原則會套用至控制器使用`[Authorize]`原則名稱的屬性。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>將原則套用至 Razor Pages

原則時，會套用至 Razor 頁面上，使用`[Authorize]`原則名稱的屬性。 例如: 

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

原則也可以套用至 Razor Pages 使用[授權慣例](xref:security/authorization/razor-pages-authorization)。

## <a name="requirements"></a>需求

授權需求是原則可用來評估目前的使用者主體的資料參數的集合。 在我們的 「 AtLeast21 」 原則的需求是單一參數&mdash;的最低存在時間。 需求會實作[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的 marker 介面。 無法實作參數化的最短使用期限需求如下所示：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

如果授權原則包含多個授權需求，所有的需求必須傳遞成功原則評估的順序。 換句話說，在處理新增至單一授權原則的多個授權需求**AND**為基礎。

> [!NOTE]
> 要求不需要有資料或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權的處理常式

授權的處理常式會負責需求之屬性的評估。 授權的處理常式會評估針對所提供的需求[AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)來判斷是否允許存取。

需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。 處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中`TRequirement`是要處理的需求。 或者，可能會實作處理常式[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多個類型的需求。

### <a name="use-a-handler-for-one-requirement"></a>使用處理常式，如有一項需求

<a name="security-authorization-handler-example"></a>

一對一關聯性的最短使用期限處理常式會利用唯一的要求範例如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

上述程式碼會判斷目前的使用者主體是否宣告其已由已知且受信任的簽發者發出的出生日期。 宣告遺漏時，不會發生授權，在此情況下完成的工作就會傳回。 當宣告存在時，會計算使用者的年齡。 如果使用者滿足需求所定義的最低存在時間，授權已被視為成功。 當授權是否成功，`context.Succeed`滿足的需求做為其唯一的參數叫用。

### <a name="use-a-handler-for-multiple-requirements"></a>使用多個需求的處理常式

在其中的權限的處理常式可以處理三種不同類型的需求的一對多關聯性的範例如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

上述程式碼會周遊[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;標記為成功不包含需求的屬性。 針對`ReadPermission`需求，使用者必須是擁有者或贊助者來存取要求的資源。 若是`EditPermission`或`DeletePermission`需求，他或她必須存取要求的資源擁有者。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理常式註冊

在設定期間的服務集合中註冊處理常式。 例如: 

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

上述程式碼會註冊`MinimumAgeHandler`為叫用單一`services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`。 可以使用任何內建註冊處理常式[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)。

## <a name="what-should-a-handler-return"></a>項目應該處理常式傳回？

請注意，`Handle`方法中的[處理常式範例](#security-authorization-handler-example)不傳回任何值。 如何為成功或失敗處的狀態？

* 處理常式會表示成功呼叫`context.Succeed(IAuthorizationRequirement requirement)`，傳遞的需求，已驗證成功。

* 一般而言，處理失敗，因為可能會成功的相同需求的其他處理常式，不必處理常式。

* 若要保證失敗，即使其他要求處理常式會成功，呼叫`context.Fail`。

如果處理常式會呼叫`context.Succeed`或`context.Fail`，仍會在所有其他的處理常式呼叫。 這可讓產生副作用，例如記錄，即使另一個處理常式已順利驗證或失敗的需求，會發生的需求。 當設定為`false`，則[InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性 （可在 ASP.NET Core 1.1 和更新版本） 的縮短處理常式執行時`context.Fail`呼叫。 `InvokeHandlersAfterFailure` 預設為`true`，在此情況下會呼叫所有的處理常式。

> [!NOTE]
> 即使驗證失敗，會呼叫授權處理常式。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>我為什麼需要多個處理常式之需求？

在您想要評估的情況下**或**為基礎，實作單一需求的多個處理常式。 例如，Microsoft 有只能開啟與索引鍵的卡片的門。 如果您在家離開您關鍵賀卡，接待員列印暫存的貼紙，並會為您開啟媒體櫃門。 在此案例中，您必須是單一的需求， *BuildingEntry*，但多個處理常式，每個檢查單一的需求。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

請確定這兩個處理常式[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。 如果其中一個處理常式成功時原則評估`BuildingEntryRequirement`，成功的原則評估。

## <a name="using-a-func-to-fulfill-a-policy"></a>若要滿足原則使用 func

可能有哪些履行原則是簡單的程式碼中的情況。 可提供`Func<AuthorizationHandlerContext, bool>`時設定您的原則與`RequireAssertion`原則產生器。

比方說前, 一個`BadgeEntryHandler`重寫，如下所示：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>存取 MVC 處理常式中的要求內容

`HandleRequirementAsync`您在授權處理常式中實作的方法有兩個參數：`AuthorizationHandlerContext`而`TRequirement`處理。 MVC 或 Jabbr 之類的架構可用於將任何物件加入`Resource`屬性上的`AuthorizationHandlerContext`傳遞額外資訊。

例如，MVC 會將傳遞的執行個體[AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext)在`Resource`屬性。 這個屬性會提供存取權`HttpContext`， `RouteData`，和其他提供 MVC 和 Razor 頁面的所有項目。

使用`Resource`屬性是特定的架構。 使用中的資訊`Resource`屬性會限制您的授權原則，以特定的架構。 您應該將轉型`Resource`屬性使用`is`關鍵字，然後確認 轉型成功以確保不會損毀您的程式碼與`InvalidCastException`其他架構上執行時：

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
