---
title: ASP.NET Core 中以原則為基礎的授權
author: rick-anderson
description: 瞭解如何建立和使用授權原則處理常式，以在 ASP.NET Core 應用程式中強制執行授權需求。
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/09/2020
ms.locfileid: "75828954"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>ASP.NET Core 中以原則為基礎的授權

::: moniker range=">= aspnetcore-3.0"

在幕後，以[角色為基礎的授權](xref:security/authorization/roles)和[宣告式授權](xref:security/authorization/claims)會使用需求、需求處理常式和預先設定的原則。 這些組建區塊支援程式碼中的授權評估運算式。 結果是更豐富、可重複使用、可測試的授權結構。

授權原則是由一個或多個需求所組成。 在 `Startup.ConfigureServices` 方法中，它會註冊為授權服務設定的一部分：

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

在上述範例中，會建立 "AtLeast21" 原則。 它有一項需求，&mdash;最短的存留期，並以參數的形式提供給需求。

## <a name="iauthorizationservice"></a>IAuthorizationService 

判斷授權是否成功的主要服務 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>：

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

上述程式碼會反白顯示[IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)的兩個方法。

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> 是沒有方法的標記服務，以及用來追蹤授權是否成功的機制。

每個 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> 都會負責檢查是否符合需求：
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

<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> 類別是處理常式用來標示是否符合需求的內容：

```csharp
 context.Succeed(requirement)
```

下列程式碼顯示授權服務的簡化（並加上批註）預設的執行方式：

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

下列程式碼顯示一般的 `ConfigureServices`：

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

使用 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> 或 `[Authorize(Policy = "Something")]` 進行授權。

## <a name="applying-policies-to-mvc-controllers"></a>將原則套用至 MVC 控制器

如果您是使用 Razor Pages，請參閱本檔中的[將原則套用至 Razor Pages](#applying-policies-to-razor-pages) 。

原則會套用至控制器，方法是使用 `[Authorize]` 屬性加上原則名稱。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>將原則套用至 Razor Pages

原則會套用至 Razor Pages，方法是使用具有原則名稱的 `[Authorize]` 屬性。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

您也可以使用[授權慣例](xref:security/authorization/razor-pages-authorization)，將原則套用至 Razor Pages。

## <a name="requirements"></a>需求

授權需求是一種資料參數集合，原則可以用來評估目前的使用者主體。 在我們的「AtLeast21」原則中，需求是&mdash;最短存留期的單一參數。 需求會執行[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的標記介面。 參數化最短存留期需求的執行方式如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

如果授權原則包含多個授權需求，則必須通過所有需求，原則評估才會成功。 換句話說，新增至單一授權原則的多個授權需求會以一**和**為基礎來處理。

> [!NOTE]
> 需求不需要有資料或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權處理常式

授權處理常式負責評估需求的屬性。 授權處理常式會針對提供的[AuthorizationHandlerCoNtext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)評估需求，以判斷是否允許存取。

需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。 處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中 `TRequirement` 是要處理的需求。 或者，處理常式可能會執行[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的需求。

### <a name="use-a-handler-for-one-requirement"></a>針對一個需求使用處理程式

<a name="security-authorization-handler-example"></a>

以下是一對一關聯性的範例，其中最小存留期處理常式會利用單一需求：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

上述程式碼會判斷目前的使用者主體是否具有由已知且受信任簽發者所發行的「出生日期」。 當宣告遺失時，就無法進行授權，在這種情況下會傳回已完成的工作。 當宣告存在時，就會計算使用者的年齡。 如果使用者符合需求所定義的最短存留期，則會將授權視為成功。 當授權成功時，會叫用 `context.Succeed`，並以滿足的需求作為其唯一的參數。

### <a name="use-a-handler-for-multiple-requirements"></a>針對多項需求使用處理程式

以下是一對多關聯性的範例，其中的許可權處理常式可處理三種不同類型的需求：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

上述程式碼會將[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;包含未標示為成功之需求的屬性。 針對 `ReadPermission` 需求，使用者必須是擁有者或贊助者，才能存取要求的資源。 在 `EditPermission` 或 `DeletePermission` 需求的情況下，他或她必須是擁有者，才能存取要求的資源。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理常式註冊

在設定期間，會在服務集合中註冊處理常式。 例如：

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

上述程式碼會藉由叫用 `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`，將 `MinimumAgeHandler` 註冊為單一實例。 您可以使用任何內建[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)來註冊處理常式。

## <a name="what-should-a-handler-return"></a>處理常式會傳回什麼？

請注意，[處理常式範例](#security-authorization-handler-example)中的 `Handle` 方法不會傳回任何值。 如何指出成功或失敗的狀態？

* 處理常式會藉由呼叫 `context.Succeed(IAuthorizationRequirement requirement)`來表示成功，傳遞已成功驗證的需求。

* 處理常式不需要處理失敗的一般情況，因為相同需求的其他處理常式可能會成功。

* 若要保證失敗，即使其他需求處理常式成功，也請呼叫 `context.Fail`。

如果處理常式呼叫 `context.Succeed` 或 `context.Fail`，仍然會呼叫所有其他處理常式。 這可讓需求產生副作用，例如記錄，即使另一個處理常式已成功驗證或失敗，也會發生這種情況。 當設定為 `false`時， [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性（可在 ASP.NET Core 1.1 和更新版本中使用）會在呼叫 `context.Fail` 時，縮短執行處理常式。 `InvokeHandlersAfterFailure` 預設為 `true`，在此情況下會呼叫所有處理常式。

> [!NOTE]
> 即使驗證失敗，也會呼叫授權處理常式。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>為什麼需要多個處理常式才能進行需求？

如果您想要以**或**為基礎進行評估，請針對單一需求執行多個處理常式。 例如，Microsoft 有門只能以金鑰卡開啟。 如果您在家中保留金鑰卡，則接待員會列印暫時的貼紙，並為您開啟大門。 在此案例中，您會有單一需求、 *BuildingEntry*，但有多個處理常式，每一個都檢查單一需求。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

請確定這兩個處理常式都已[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。 當原則評估 `BuildingEntryRequirement`時，如果任一處理程式成功，原則評估就會成功。

## <a name="using-a-func-to-fulfill-a-policy"></a>使用 func 來完成原則

在某些情況下，在程式碼中可以簡單地表達原則。 使用 `RequireAssertion` 原則產生器來設定原則時，可以提供 `Func<AuthorizationHandlerContext, bool>`。

例如，先前的 `BadgeEntryHandler` 可以改寫如下：

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a>存取處理常式中的 MVC 要求內容

您在授權處理常式中執行的 `HandleRequirementAsync` 方法有兩個參數： `AuthorizationHandlerContext` 和您正在處理的 `TRequirement`。 MVC 或 Jabbr 這類架構可自由地將任何物件新增至 `AuthorizationHandlerContext` 上的 `Resource` 屬性，以傳遞額外的資訊。

例如，MVC 會在 `Resource` 屬性中傳遞[AuthorizationFilterCoNtext](/dotnet/api/?term=AuthorizationFilterContext)的實例。 這個屬性可讓您存取 MVC 和 Razor Pages 所提供的 `HttpContext`、`RouteData`和其他所有專案。

`Resource` 屬性的使用是架構特定的。 使用 `Resource` 屬性中的資訊會將您的授權原則限制為特定架構。 您應該使用 `is` 關鍵字來轉換 `Resource` 屬性，然後確認轉換已成功，以確保您的程式碼在其他架構上執行時不會損毀 `InvalidCastException`：

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

在幕後，以[角色為基礎的授權](xref:security/authorization/roles)和[宣告式授權](xref:security/authorization/claims)會使用需求、需求處理常式和預先設定的原則。 這些組建區塊支援程式碼中的授權評估運算式。 結果是更豐富、可重複使用、可測試的授權結構。

授權原則是由一個或多個需求所組成。 在 `Startup.ConfigureServices` 方法中，它會註冊為授權服務設定的一部分：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

在上述範例中，會建立 "AtLeast21" 原則。 它有一項需求，&mdash;最短的存留期，並以參數的形式提供給需求。

## <a name="iauthorizationservice"></a>IAuthorizationService 

判斷授權是否成功的主要服務 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>：

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

上述程式碼會反白顯示[IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs)的兩個方法。

<xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> 是沒有方法的標記服務，以及用來追蹤授權是否成功的機制。

每個 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> 都會負責檢查是否符合需求：
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

<xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> 類別是處理常式用來標示是否符合需求的內容：

```csharp
 context.Succeed(requirement)
```

下列程式碼顯示授權服務的簡化（並加上批註）預設的執行方式：

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

下列程式碼顯示一般的 `ConfigureServices`：

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

使用 <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> 或 `[Authorize(Policy = "Something")]` 進行授權。

## <a name="applying-policies-to-mvc-controllers"></a>將原則套用至 MVC 控制器

如果您是使用 Razor Pages，請參閱本檔中的[將原則套用至 Razor Pages](#applying-policies-to-razor-pages) 。

原則會套用至控制器，方法是使用 `[Authorize]` 屬性加上原則名稱。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>將原則套用至 Razor Pages

原則會套用至 Razor Pages，方法是使用具有原則名稱的 `[Authorize]` 屬性。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

您也可以使用[授權慣例](xref:security/authorization/razor-pages-authorization)，將原則套用至 Razor Pages。

## <a name="requirements"></a>需求

授權需求是一種資料參數集合，原則可以用來評估目前的使用者主體。 在我們的「AtLeast21」原則中，需求是&mdash;最短存留期的單一參數。 需求會執行[IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement)，這是空的標記介面。 參數化最短存留期需求的執行方式如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

如果授權原則包含多個授權需求，則必須通過所有需求，原則評估才會成功。 換句話說，新增至單一授權原則的多個授權需求會以一**和**為基礎來處理。

> [!NOTE]
> 需求不需要有資料或屬性。

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>授權處理常式

授權處理常式負責評估需求的屬性。 授權處理常式會針對提供的[AuthorizationHandlerCoNtext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext)評估需求，以判斷是否允許存取。

需求可以有[多個處理常式](#security-authorization-policies-based-multiple-handlers)。 處理常式可能會繼承[AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1)，其中 `TRequirement` 是要處理的需求。 或者，處理常式可能會執行[IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler)來處理多種類型的需求。

### <a name="use-a-handler-for-one-requirement"></a>針對一個需求使用處理程式

<a name="security-authorization-handler-example"></a>

以下是一對一關聯性的範例，其中最小存留期處理常式會利用單一需求：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

上述程式碼會判斷目前的使用者主體是否具有由已知且受信任簽發者所發行的「出生日期」。 當宣告遺失時，就無法進行授權，在這種情況下會傳回已完成的工作。 當宣告存在時，就會計算使用者的年齡。 如果使用者符合需求所定義的最短存留期，則會將授權視為成功。 當授權成功時，會叫用 `context.Succeed`，並以滿足的需求作為其唯一的參數。

### <a name="use-a-handler-for-multiple-requirements"></a>針對多項需求使用處理程式

以下是一對多關聯性的範例，其中的許可權處理常式可處理三種不同類型的需求：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

上述程式碼會將[PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;包含未標示為成功之需求的屬性。 針對 `ReadPermission` 需求，使用者必須是擁有者或贊助者，才能存取要求的資源。 在 `EditPermission` 或 `DeletePermission` 需求的情況下，他或她必須是擁有者，才能存取要求的資源。

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>處理常式註冊

在設定期間，會在服務集合中註冊處理常式。 例如：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

上述程式碼會藉由叫用 `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`，將 `MinimumAgeHandler` 註冊為單一實例。 您可以使用任何內建[服務存留期](xref:fundamentals/dependency-injection#service-lifetimes)來註冊處理常式。

## <a name="what-should-a-handler-return"></a>處理常式會傳回什麼？

請注意，[處理常式範例](#security-authorization-handler-example)中的 `Handle` 方法不會傳回任何值。 如何指出成功或失敗的狀態？

* 處理常式會藉由呼叫 `context.Succeed(IAuthorizationRequirement requirement)`來表示成功，傳遞已成功驗證的需求。

* 處理常式不需要處理失敗的一般情況，因為相同需求的其他處理常式可能會成功。

* 若要保證失敗，即使其他需求處理常式成功，也請呼叫 `context.Fail`。

如果處理常式呼叫 `context.Succeed` 或 `context.Fail`，仍然會呼叫所有其他處理常式。 這可讓需求產生副作用，例如記錄，即使另一個處理常式已成功驗證或失敗，也會發生這種情況。 當設定為 `false`時， [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure)屬性（可在 ASP.NET Core 1.1 和更新版本中使用）會在呼叫 `context.Fail` 時，縮短執行處理常式。 `InvokeHandlersAfterFailure` 預設為 `true`，在此情況下會呼叫所有處理常式。

> [!NOTE]
> 即使驗證失敗，也會呼叫授權處理常式。

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>為什麼需要多個處理常式才能進行需求？

如果您想要以**或**為基礎進行評估，請針對單一需求執行多個處理常式。 例如，Microsoft 有門只能以金鑰卡開啟。 如果您在家中保留金鑰卡，則接待員會列印暫時的貼紙，並為您開啟大門。 在此案例中，您會有單一需求、 *BuildingEntry*，但有多個處理常式，每一個都檢查單一需求。

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

請確定這兩個處理常式都已[註冊](xref:security/authorization/policies#security-authorization-policies-based-handler-registration)。 當原則評估 `BuildingEntryRequirement`時，如果任一處理程式成功，原則評估就會成功。

## <a name="using-a-func-to-fulfill-a-policy"></a>使用 func 來完成原則

在某些情況下，在程式碼中可以簡單地表達原則。 使用 `RequireAssertion` 原則產生器來設定原則時，可以提供 `Func<AuthorizationHandlerContext, bool>`。

例如，先前的 `BadgeEntryHandler` 可以改寫如下：

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>存取處理常式中的 MVC 要求內容

您在授權處理常式中執行的 `HandleRequirementAsync` 方法有兩個參數： `AuthorizationHandlerContext` 和您正在處理的 `TRequirement`。 MVC 或 Jabbr 這類架構可自由地將任何物件新增至 `AuthorizationHandlerContext` 上的 `Resource` 屬性，以傳遞額外的資訊。

例如，MVC 會在 `Resource` 屬性中傳遞[AuthorizationFilterCoNtext](/dotnet/api/?term=AuthorizationFilterContext)的實例。 這個屬性可讓您存取 MVC 和 Razor Pages 所提供的 `HttpContext`、`RouteData`和其他所有專案。

`Resource` 屬性的使用是架構特定的。 使用 `Resource` 屬性中的資訊會將您的授權原則限制為特定架構。 您應該使用 `is` 關鍵字來轉換 `Resource` 屬性，然後確認轉換已成功，以確保您的程式碼在其他架構上執行時不會損毀 `InvalidCastException`：

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end