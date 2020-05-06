---
title: 從 ClaimsPrincipal 遷移
author: mjrousos
description: 瞭解如何從 ClaimsPrincipal 遷移，以取得目前已驗證的使用者身分識別和 ASP.NET Core 中的宣告。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/claimsprincipal-current
ms.openlocfilehash: 5f6b5b960eacf176088d9fc60f9ba16014e613fc
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776086"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>從 ClaimsPrincipal 遷移

在 ASP.NET 4.x 專案中，通常會使用[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal.current)來抓取目前已驗證使用者的身分識別和宣告。 在 ASP.NET Core 中，已不再設定此屬性。 必須更新程式碼，以透過不同的方法來取得目前已驗證的使用者身分識別。

## <a name="context-specific-data-instead-of-static-data"></a>內容特定的資料，而不是靜態資料

使用 ASP.NET Core 時，不會設定`ClaimsPrincipal.Current`和`Thread.CurrentPrincipal`的值。 這些屬性都代表靜態狀態，ASP.NET Core 通常會避免這種情況。 相反地，ASP.NET Core 的架構是從內容特定服務集合（使用其相依性[插入](xref:fundamentals/dependency-injection)（DI）模型）抓取相依性（例如目前使用者的身分識別）。 還有什麼`Thread.CurrentPrincipal`是「執行緒靜態」，因此它可能不會在某些非同步案例中保存變更`ClaimsPrincipal.Current` （而且`Thread.CurrentPrincipal`預設只會呼叫）。

若要瞭解執行緒靜態成員在非同步案例中可能會導致的問題，請考慮下列程式碼片段：

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

上述範例程式碼會`Thread.CurrentPrincipal`在等候非同步呼叫之前和之後，設定並檢查其值。 `Thread.CurrentPrincipal`專屬於其設定所在的*執行緒*，而方法可能會在 await 之後繼續在不同的執行緒上執行。 因此， `Thread.CurrentPrincipal`在第一次檢查時，會出現，但在呼叫之後`await Task.Yield()`是 null。

從應用程式的 DI 服務集合取得目前使用者的身分識別，也會更具測試性，因為可以輕鬆地插入測試身分識別。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>在 ASP.NET Core 應用程式中取出目前的使用者

有數個選項可讓您在 ASP.NET Core 中抓取`ClaimsPrincipal`目前已驗證的使用者`ClaimsPrincipal.Current`，以取代：

* **ControllerBase. User**。 MVC 控制器可以使用其[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性來存取目前已驗證的使用者。
* **HttpCoNtext**。 具有目前`HttpContext` （中介軟體）存取權的元件可以從`ClaimsPrincipal` [HttpCoNtext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)取得目前的使用者。
* **從呼叫端傳入**。 沒有目前`HttpContext`存取權的程式庫通常是從控制器或中介軟體元件呼叫，而且可以將目前使用者的身分識別當做引數傳遞。
* **IHttpCoNtextAccessor**。 要遷移至 ASP.NET Core 的專案可能太大，而無法輕鬆地將目前使用者的身分識別傳遞至所有必要的位置。 在這種情況下，您可以使用[IHttpCoNtextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)作為因應措施。 `IHttpContextAccessor`能夠存取目前`HttpContext`的（如果有的話）。 如果正在使用 DI，請參閱<xref:fundamentals/httpcontext>。 在程式碼中取得目前使用者的身分識別，但尚未更新為使用 ASP.NET Core 的 DI 驅動架構的短期解決方案會是：

  * 在`IHttpContextAccessor`中`Startup.ConfigureServices`呼叫[AddHttpCoNtextAccessor](https://github.com/aspnet/Hosting/issues/793) ，使其可在 DI 容器中使用。
  * 在啟動`IHttpContextAccessor`期間取得實例，並將它儲存在靜態變數中。 實例可供先前從靜態屬性取得目前使用者的程式碼使用。
  * 使用`HttpContextAccessor.HttpContext?.User`取得目前使用者的`ClaimsPrincipal` 。 如果在 HTTP 要求的內容外部使用此`HttpContext`程式碼，則為 null。

使用儲存在靜態變數中`IHttpContextAccessor`的實例的最後一個選項，與 ASP.NET Core 的原則相反（偏好插入靜態相依性的相依性）。 計畫最後改為`IHttpContextAccessor`從相依性插入取得實例。 不過，靜態協助程式可以是很有用的橋接器，而是在遷移先前使用`ClaimsPrincipal.Current`的大型現有 ASP.NET 應用程式時。
