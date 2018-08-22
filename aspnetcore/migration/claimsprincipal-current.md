---
title: 移轉 ClaimsPrincipal.Current 從
author: mjrousos
description: 了解如何將從要擷取目前已驗證的使用者的身分識別與 ASP.NET Core 中的宣告的 ClaimsPrincipal.Current 轉移。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/04/2018
uid: migration/claimsprincipal-current
ms.openlocfilehash: 35c3389798041e141c45bf0a76fa9d7285212768
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823715"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>移轉 ClaimsPrincipal.Current 從

在 ASP.NET 4.x 專案中，已使用通用[ClaimsPrincipal.Current](/dotnet/api/system.security.claims.claimsprincipal.current)若要擷取目前已驗證使用者的身分識別和宣告。 在 ASP.NET Core 無法再設定這個屬性。 根據它的程式碼需要更新，以取得目前已驗證的使用者的身分識別，透過不同的方式。

## <a name="context-specific-data-instead-of-static-data"></a>特定內容的資料，而不是靜態的資料

使用 ASP.NET Core，兩者的值時`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`未設定。 這些屬性都代表靜態狀態，通常可以避免 ASP.NET Core。 相反地，ASP.NET Core 架構是從特定內容的服務集合擷取相依性 （例如目前的使用者身分識別） (使用其[相依性插入](xref:fundamentals/dependency-injection)(DI) 模型)。 什麼是等等`Thread.CurrentPrincipal`是執行緒靜態的因此它可能不會保存在某些非同步案例中的變更 (和`ClaimsPrincipal.Current`只會呼叫`Thread.CurrentPrincipal`預設)。

若要了解的各種問題執行緒靜態成員可能導致在非同步的情況下，請考慮下列程式碼片段：

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

上述的範例程式碼集`Thread.CurrentPrincipal`並檢查其值之前和之後等候的非同步呼叫。 `Thread.CurrentPrincipal` 特有*執行緒*在其上設定，而方法為可能會繼續執行不同的執行緒上的 await 之後。 因此，`Thread.CurrentPrincipal`存在時它會先檢查，但在呼叫之後為 null `await Task.Yield()`。

從應用程式的 DI 服務集合中取得目前使用者的身分識別太多測試，因為可以輕鬆地插入測試身分識別。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>擷取目前使用者的 ASP.NET Core 應用程式中

有幾個選項來擷取目前已驗證的使用者的`ClaimsPrincipal`中取代 ASP.NET Core `ClaimsPrincipal.Current`:

* **ControllerBase.User**。 MVC 控制站可以存取目前已驗證的使用者與他們[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性。
* **HttpContext.User**。 存取目前的元件`HttpContext`（例如中的介軟體） 可以取得目前的使用者`ClaimsPrincipal`從[HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)。
* **從呼叫端傳入**。 目前無法存取的程式庫`HttpContext`通常稱為從控制器或中介軟體元件，且可以做為引數傳遞的目前使用者的身分識別。
* **IHttpContextAccessor**。 正在移轉至 ASP.NET Core 專案可能太大，無法輕鬆地將目前使用者的身分識別傳遞給所有必要的位置。 在此情況下， [IHttpContextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)可用因應措施。 `IHttpContextAccessor` 能夠存取目前`HttpContext`（如果有的話）。 若要在尚未尚未更新為使用 ASP.NET Core DI 驅動的架構的程式碼中取得目前使用者的身分識別的短期方案是：

  * 製作`IHttpContextAccessor`DI 容器藉由呼叫中[AddHttpContextAccessor](https://github.com/aspnet/Hosting/issues/793)在`Startup.ConfigureServices`。
  * 取得的執行個體`IHttpContextAccessor`在啟動期間並將它儲存到靜態變數中。 執行個體是開放給先前已從靜態屬性擷取目前使用者的程式碼中使用。
  * 擷取目前的使用者`ClaimsPrincipal`使用`HttpContextAccessor.HttpContext?.User`。 如果這個程式碼會使用 HTTP 要求的內容之外`HttpContext`為 null。

最終選項，使用`IHttpContextAccessor`，違反 （靜態相依性偏好插入相依性） 的 ASP.NET Core 原則。 計劃最後會移除對靜態的相依性`IHttpContextAccessor`協助程式。 它可以是很有用的橋接器，不過，移轉大型的現有 ASP.NET 應用程式先前使用時`ClaimsPrincipal.Current`。
