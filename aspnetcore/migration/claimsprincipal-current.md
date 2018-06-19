---
title: 從 ClaimsPrincipal.Current 移轉
author: mjrousos
description: 了解如何將從要擷取目前已驗證的使用者的身分識別與 ASP.NET Core 中的宣告的 ClaimsPrincipal.Current 轉移。
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/claimsprincipal-current
ms.openlocfilehash: ea43d17e76380baf57cd9debbc508e8812cfa4a6
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2018
ms.locfileid: "33851530"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>從 ClaimsPrincipal.Current 移轉

在 ASP.NET 專案中，已使用[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)擷取目前驗證使用者的身分識別和宣告。 在 ASP.NET Core 無法再設定這個屬性。 取決於它的程式碼需要更新，以取得目前已驗證的使用者的身分識別，透過不同的方式。

## <a name="context-specific-data-instead-of-static-data"></a>特定內容的資料，而非靜態資料

使用 ASP.NET Core，兩者的值時`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`未設定。 這些屬性都代表靜態狀態，通常可以避免 ASP.NET Core。 相反地，ASP.NET Core 架構是從特定內容的服務集合擷取相依性 （例如目前的使用者身分識別） (使用其[相依性插入](xref:fundamentals/dependency-injection)(DI) 模型)。 什麼是多個，`Thread.CurrentPrincipal`是執行緒靜態的因此它可能不是保存在某些非同步案例中的變更 (和`ClaimsPrincipal.Current`只會呼叫`Thread.CurrentPrincipal`依預設)。

若要了解的各種問題執行緒靜態成員可能導致在非同步案例中，請考慮下列程式碼片段：

```csharp
// Create a ClaimsPrincipal and set Thread.CurrentPrincipal
var identity = new ClaimsIdentity();
identity.AddClaim(new Claim(ClaimTypes.Name, "User1"));
Thread.CurrentPrincipal = new ClaimsPrincipal(identity);

// Check the current user
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");

// For the method to complete asynchronously
await Task.Yield();

// Check the current user after
Console.WriteLine($"Current user: {Thread.CurrentPrincipal?.Identity.Name}");
```

上述範例程式碼集`Thread.CurrentPrincipal`，並檢查其值之前和之後等候非同步呼叫。 `Thread.CurrentPrincipal` 特定*執行緒*在其上設定，而方法可能繼續的 await 之後不同的執行緒上執行。 因此，`Thread.CurrentPrincipal`時的存在，當第一次檢查，但在呼叫之後為 null `await Task.Yield()`。

取得目前使用者的身分識別，從應用程式的 DI 服務集合太多測試，因為測試身分識別可輕鬆地插入。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>擷取目前使用者的 ASP.NET Core 應用程式中

有幾個選項來擷取目前已驗證的使用者的`ClaimsPrincipal`中取代 ASP.NET Core `ClaimsPrincipal.Current`:

* **ControllerBase.User**。 MVC 控制站可以存取與目前已驗證的使用者其[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性。
* **HttpContext.User**。 元件的存取權目前`HttpContext`（中的介軟體，例如） 可以取得目前使用者的`ClaimsPrincipal`從[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)。
* **傳入呼叫端從**。 如果沒有為目前的權限的程式庫`HttpContext`通常稱為從控制器或中介軟體元件，且可以有目前使用者的身分識別做為引數。
* **IHttpContextAccessor**。 在移轉至 ASP.NET Core ASP.NET 專案可能太大，無法輕鬆地將目前的使用者識別傳遞給所有必要的位置。 在這種情況下， [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)可用來當做因應措施。 `IHttpContextAccessor` 可以存取目前`HttpContext`（如果有的話）。 若要在尚未尚未更新為使用 ASP.NET Core DI 驅動的架構的程式碼中取得目前使用者的身分識別的短期方案是：

  * 請`IHttpContextAccessor`藉由呼叫 DI 容器中[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)中`Startup.ConfigureServices`。
  * 取得的執行個體`IHttpContextAccessor`在啟動期間並將它儲存在靜態變數中。 執行個體才可以提供給先前已從靜態屬性擷取目前使用者的程式碼。
  * 擷取目前使用者的`ClaimsPrincipal`使用`HttpContextAccessor.HttpContext?.User`。 如果此程式碼使用的 HTTP 要求，環境之外`HttpContext`為 null。

最終選項，使用`IHttpContextAccessor`，違反 （靜態相依性偏好插入相依性） 的 ASP.NET 核心原則。 打算最後移除的相依性靜態`IHttpContextAccessor`協助程式。 就很有用的橋接器，不過，移轉大型的現有 ASP.NET 應用程式先前使用`ClaimsPrincipal.Current`。
