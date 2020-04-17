---
title: ASP.NET核心中的基於策略的授權
author: rick-anderson
description: 瞭解如何創建和使用授權策略處理程式,以在ASP.NET酷應用中強制實施授權要求。
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2020
uid: security/authorization/policies
ms.openlocfilehash: 66412a6020ea8f12427c8c5b466e1b2eedccf0f9
ms.sourcegitcommit: 77c046331f3d633d7cc247ba77e58b89e254f487
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81488757"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>ASP.NET核心中的基於策略的授權

::: moniker range=">= aspnetcore-3.0"

在封面下方,[基於角色的授權](xref:security/authorization/roles)和[基於聲明的授權](xref:security/authorization/claims)使用要求、需求處理程式和預配置的策略。 這些構建基塊支援代碼中授權評估的運算式。 結果是更豐富、可重用、可測試的授權結構。

授權策略由一個或多個要求組成。 它在 方法中註冊為授權服務配置的`Startup.ConfigureServices`一 部分:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

在前面的示例中,將創建一個"AtLeast21"策略。 它有一個最低年齡&mdash;的單一要求,作為要求的參數提供。

## <a name="iauthorizationservice"></a>I 授權服務 

確定授權是否成功的主要服務是<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

前面的代碼突出顯示了[I 授權服務的](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)兩種方法。

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>是一個沒有方法的標記服務,是跟蹤授權是否成功的機制。

每個<xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler>公司負責檢查是否滿足要求:
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

類別<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext>是處理程式用於標記是否滿足要求的內容:

```csharp
 context.Succeed(requirement)
```

以下代碼顯示授權服務的簡化(並帶有註解)預設實現:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

以下代碼顯示了典型的`ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

使用<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>或`[Authorize(Policy = "Something")]`授權。

## <a name="applying-policies-to-mvc-controllers"></a>將策略應用於 MVC 控制器

如果您使用的是 Razor 頁面,請參考文件中[將策略套用 Razor 頁面](#applying-policies-to-razor-pages)。

策略通過使用具有策略名稱`[Authorize]`的屬性應用於控制器。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>將策略應用於剃刀頁面

策略通過使用具有策略名稱`[Authorize]`的屬性應用於 Razor 頁面。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

策略也可以通過使用[授權約定](xref:security/authorization/razor-pages-authorization)應用於 Razor 頁面。

## <a name="requirements"></a>需求

授權要求是策略可用於評估當前用戶主體的數據參數的集合。 在我們的「AtLeast21」政策中,要求是最小年齡的單個&mdash;參數。 要求實現[I 授權要求](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement),這是一個空標記介面。 參數化的最低年齡要求可執行如下:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

如果授權策略包含多個授權要求,則必須通過所有要求才能成功策略評估。 換句話說,添加到單個授權策略中的多個授權要求將基於**AND**處理。

> [!NOTE]
> 要求不需要具有數據或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權處理常式

授權處理程序負責評估需求的屬性。 授權處理程式根據提供的[授權處理程式計算](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)需求,以確定是否允許訪問。

要求可以[有多個處理程式](#security-authorization-policies-based-multiple-handlers)。 處理程式可以繼承[授權處理\<程式 t要求>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1),其中`TRequirement`需要處理。 或者,處理程式可以實現[I授權處理程式](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的要求。

### <a name="use-a-handler-for-one-requirement"></a>對一個要求使用處理程式

<a name="security-authorization-handler-example"></a>

下面是一對一關係的示例,其中最小年齡處理程式使用單個要求:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

前面的代碼確定當前用戶主體是否具有由已知和受信任的頒發者頒發的出生日期聲明。 當缺少聲明時,無法發生授權,在這種情況下,將返回已完成的任務。 當聲明存在時,將計算用戶的年齡。 如果用戶滿足要求定義的最低年齡,則授權被視為成功。 當授權成功時,`context.Succeed`將調用滿足的要求作為其唯一參數。

### <a name="use-a-handler-for-multiple-requirements"></a>對多個要求使用處理程式

下面是一對多關係的範例,其中許可權處理程式可以處理三種不同類型的要求:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

前面的代碼遍歷[Pending要求](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;包含未標記為成功的要求的屬性。 對於要求`ReadPermission`,使用者必須是擁有者或贊助者才能訪問請求的資源。 如果是`EditPermission``DeletePermission`或要求,他或她必須是訪問請求的資源的擁有者。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理程式註冊

在配置期間,處理程式在服務集合中註冊。 例如：

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

前面的代碼通過調用`MinimumAgeHandler``services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`註冊為單例。 處理程式可以使用任何內置[服務存留期進行](xref:fundamentals/dependency-injection#service-lifetimes)註冊。

## <a name="what-should-a-handler-return"></a>處理程式應返回什麼?

請注意,`Handle`[處理程式範例中](#security-authorization-handler-example)的方法不返回任何值。 如何指示成功或失敗的狀態?

* 處理程序通過調用`context.Succeed(IAuthorizationRequirement requirement)`來表示成功,傳遞已成功驗證的要求。

* 處理程式通常不需要處理故障,因為相同要求的其他處理程式可能會成功。

* 為了保證失敗,即使其他需求處理程式成功,請呼叫`context.Fail`。

如果處理程式調用`context.Succeed``context.Fail`或 ,仍調用所有其他處理程式。 這允許要求產生副作用,如日誌記錄,即使另一個處理程式已成功驗證或未通過要求也是如此。 當設置為`false`[時,InvokeHandlersAfter 失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性(在 ASP.NET Core 1.1 和更高版本`context.Fail`中可用)在調用時 短路處理程式的執行。 `InvokeHandlersAfterFailure`默認值為`true`,在這種情況下,將調用所有處理程式。

> [!NOTE]
> 即使身份驗證失敗,也會調用授權處理程式。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>為什麼需要多個處理程序來滿足需求?

如果希望評估基於**OR,** 則為單個要求實現多個處理程式。 例如,微軟的大門只打開鑰匙卡。 如果您將鑰匙卡留在家中,接待員會列印臨時貼紙,併為您開門。 在這種情況下,您將有一個要求,*即"構建入口*",但有多個處理程式,每個處理程式都檢查單個要求。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

確保兩個處理程式都[已註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。 如果任一處理程式在策略計算 時`BuildingEntryRequirement`成功 ,則策略計算將成功。

## <a name="using-a-func-to-fulfill-a-policy"></a>使用 func 實作原則

在某些情況下,實現策略在代碼中很容易表達。 在與策略產生器設定策略時,`Func<AuthorizationHandlerContext, bool>``RequireAssertion`可以提供 .

例如,可以重寫前`BadgeEntryHandler`一個,如下所示:

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a>在處理程式中存取 MVC 要求上下文

在`HandleRequirementAsync`授權處理程式中實現的方法有兩個參數:和`AuthorizationHandlerContext``TRequirement`正在處理的參數。 MVC 或 Jabbr 等框架`Resource`可免費向 的`AuthorizationHandlerContext`屬性添加任何物件 以傳遞額外資訊。

例如,MVC 在屬性中傳遞[授權篩選器上下文](/dotnet/api/?term=AuthorizationFilterContext)的`Resource`實例。 此屬性提供對`HttpContext``RouteData`的訪問許可權,以及 MVC 和 Razor 頁面提供的所有內容。

`Resource`屬性的使用是特定於框架的。 使用 屬性`Resource`中的資訊會將授權策略限制為特定框架。 應使用`Resource``is`關鍵字強制轉換屬性,然後確認強制轉換已成功確保代碼在其他框架上執行時不會崩潰與`InvalidCastException`。

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

在封面下方,[基於角色的授權](xref:security/authorization/roles)和[基於聲明的授權](xref:security/authorization/claims)使用要求、需求處理程式和預配置的策略。 這些構建基塊支援代碼中授權評估的運算式。 結果是更豐富、可重用、可測試的授權結構。

授權策略由一個或多個要求組成。 它在 方法中註冊為授權服務配置的`Startup.ConfigureServices`一 部分:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

在前面的示例中,將創建一個"AtLeast21"策略。 它有一個最低年齡&mdash;的單一要求,作為要求的參數提供。

## <a name="iauthorizationservice"></a>I 授權服務 

確定授權是否成功的主要服務是<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

前面的代碼突出顯示了[I 授權服務的](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)兩種方法。

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>是一個沒有方法的標記服務,是跟蹤授權是否成功的機制。

每個<xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler>公司負責檢查是否滿足要求:
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

類別<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext>是處理程式用於標記是否滿足要求的內容:

```csharp
 context.Succeed(requirement)
```

以下代碼顯示授權服務的簡化(並帶有註解)預設實現:

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

以下代碼顯示了典型的`ConfigureServices`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

使用<xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>或`[Authorize(Policy = "Something")]`授權。

## <a name="applying-policies-to-mvc-controllers"></a>將策略應用於 MVC 控制器

如果您使用的是 Razor 頁面,請參考文件中[將策略套用 Razor 頁面](#applying-policies-to-razor-pages)。

策略通過使用具有策略名稱`[Authorize]`的屬性應用於控制器。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>將策略應用於剃刀頁面

策略通過使用具有策略名稱`[Authorize]`的屬性應用於 Razor 頁面。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

策略也可以通過使用[授權約定](xref:security/authorization/razor-pages-authorization)應用於 Razor 頁面。

## <a name="requirements"></a>需求

授權要求是策略可用於評估當前用戶主體的數據參數的集合。 在我們的「AtLeast21」政策中,要求是最小年齡的單個&mdash;參數。 要求實現[I 授權要求](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement),這是一個空標記介面。 參數化的最低年齡要求可執行如下:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

如果授權策略包含多個授權要求,則必須通過所有要求才能成功策略評估。 換句話說,添加到單個授權策略中的多個授權要求將基於**AND**處理。

> [!NOTE]
> 要求不需要具有數據或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權處理常式

授權處理程序負責評估需求的屬性。 授權處理程式根據提供的[授權處理程式計算](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)需求,以確定是否允許訪問。

要求可以[有多個處理程式](#security-authorization-policies-based-multiple-handlers)。 處理程式可以繼承[授權處理\<程式 t要求>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1),其中`TRequirement`需要處理。 或者,處理程式可以實現[I授權處理程式](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的要求。

### <a name="use-a-handler-for-one-requirement"></a>對一個要求使用處理程式

<a name="security-authorization-handler-example"></a>

下面是一對一關係的示例,其中最小年齡處理程式使用單個要求:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

前面的代碼確定當前用戶主體是否具有由已知和受信任的頒發者頒發的出生日期聲明。 當缺少聲明時,無法發生授權,在這種情況下,將返回已完成的任務。 當聲明存在時,將計算用戶的年齡。 如果用戶滿足要求定義的最低年齡,則授權被視為成功。 當授權成功時,`context.Succeed`將調用滿足的要求作為其唯一參數。

### <a name="use-a-handler-for-multiple-requirements"></a>對多個要求使用處理程式

下面是一對多關係的範例,其中許可權處理程式可以處理三種不同類型的要求:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

前面的代碼遍歷[Pending要求](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;包含未標記為成功的要求的屬性。 對於要求`ReadPermission`,使用者必須是擁有者或贊助者才能訪問請求的資源。 如果是`EditPermission``DeletePermission`或要求,他或她必須是訪問請求的資源的擁有者。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理程式註冊

在配置期間,處理程式在服務集合中註冊。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

前面的代碼通過調用`MinimumAgeHandler``services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`註冊為單例。 處理程式可以使用任何內置[服務存留期進行](xref:fundamentals/dependency-injection#service-lifetimes)註冊。

## <a name="what-should-a-handler-return"></a>處理程式應返回什麼?

請注意,`Handle`[處理程式範例中](#security-authorization-handler-example)的方法不返回任何值。 如何指示成功或失敗的狀態?

* 處理程序通過調用`context.Succeed(IAuthorizationRequirement requirement)`來表示成功,傳遞已成功驗證的要求。

* 處理程式通常不需要處理故障,因為相同要求的其他處理程式可能會成功。

* 為了保證失敗,即使其他需求處理程式成功,請呼叫`context.Fail`。

如果處理程式調用`context.Succeed``context.Fail`或 ,仍調用所有其他處理程式。 這允許要求產生副作用,如日誌記錄,即使另一個處理程式已成功驗證或未通過要求也是如此。 當設置為`false`[時,InvokeHandlersAfter 失敗](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性(在 ASP.NET Core 1.1 和更高版本`context.Fail`中可用)在調用時 短路處理程式的執行。 `InvokeHandlersAfterFailure`默認值為`true`,在這種情況下,將調用所有處理程式。

> [!NOTE]
> 即使身份驗證失敗,也會調用授權處理程式。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>為什麼需要多個處理程序來滿足需求?

如果希望評估基於**OR,** 則為單個要求實現多個處理程式。 例如,微軟的大門只打開鑰匙卡。 如果您將鑰匙卡留在家中,接待員會列印臨時貼紙,併為您開門。 在這種情況下,您將有一個要求,*即"構建入口*",但有多個處理程式,每個處理程式都檢查單個要求。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

確保兩個處理程式都[已註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。 如果任一處理程式在策略計算 時`BuildingEntryRequirement`成功 ,則策略計算將成功。

## <a name="using-a-func-to-fulfill-a-policy"></a>使用 func 實作原則

在某些情況下,實現策略在代碼中很容易表達。 在與策略產生器設定策略時,`Func<AuthorizationHandlerContext, bool>``RequireAssertion`可以提供 .

例如,可以重寫前`BadgeEntryHandler`一個,如下所示:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>在處理程式中存取 MVC 要求上下文

在`HandleRequirementAsync`授權處理程式中實現的方法有兩個參數:和`AuthorizationHandlerContext``TRequirement`正在處理的參數。 MVC 或 SignalR 等框架`Resource`可免費向 的`AuthorizationHandlerContext`屬性添加任何物件 以傳遞額外資訊。

使用終結點路由時,授權通常由授權中間件處理。 在這種情況下,`Resource`屬性是<xref:Microsoft.AspNetCore.Http.Endpoint>的實體。 終結點可用於探測要路由到的資源的基礎。 例如：

```csharp
if (context.Resource is Endpoint endpoint)
{
   var actionDescriptor = endpoint.Metadata.GetMetadata<ControllerActionDescriptor>();
   ...
}
```

對於傳統路由,或者當授權作為MVC授權篩選器的一部分發生時,值`Resource`是實例。 <xref:Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext> 此屬性提供對`HttpContext``RouteData`的訪問許可權,以及 MVC 和 Razor 頁面提供的所有內容。

`Resource`屬性的使用是特定於框架的。 使用 屬性`Resource`中的資訊會將授權策略限制為特定框架。 應使用`Resource``is`關鍵字強制轉換屬性,然後確認強制轉換已成功確保代碼在其他框架上執行時不會崩潰與`InvalidCastException`。

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end
