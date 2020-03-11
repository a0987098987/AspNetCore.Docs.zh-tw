---
title: 從 ClaimsPrincipal 遷移
author: mjrousos
description: 瞭解如何從 ClaimsPrincipal 遷移，以取得目前已驗證的使用者身分識別和 ASP.NET Core 中的宣告。
ms.author: scaddie
ms.custom: mvc
ms.date: 03/26/2019
uid: migration/claimsprincipal-current
ms.openlocfilehash: f7472f5b851d3869da3d26b881e276ce4ca004fb
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659310"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>從 ClaimsPrincipal 遷移

在 ASP.NET 4.x 專案中，通常會使用[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal.current)來抓取目前已驗證使用者的身分識別和宣告。 在 ASP.NET Core 中，已不再設定此屬性。 必須更新程式碼，以透過不同的方法來取得目前已驗證的使用者身分識別。

## <a name="context-specific-data-instead-of-static-data"></a>內容特定的資料，而不是靜態資料

使用 ASP.NET Core 時，不會同時設定 `ClaimsPrincipal.Current` 和 `Thread.CurrentPrincipal` 的值。 這些屬性都代表靜態狀態，ASP.NET Core 通常會避免這種情況。 相反地，ASP.NET Core 的架構是從內容特定服務集合（使用其相依性[插入](xref:fundamentals/dependency-injection)（DI）模型）抓取相依性（例如目前使用者的身分識別）。 此外，`Thread.CurrentPrincipal` 是執行緒靜態的，因此它可能不會在某些非同步案例中保存變更（而且 `ClaimsPrincipal.Current` 預設只會呼叫 `Thread.CurrentPrincipal`）。

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

上述範例程式碼會設定 `Thread.CurrentPrincipal`，並在等候非同步呼叫之前和之後檢查其值。 `Thread.CurrentPrincipal` 專屬於其設定所在的*執行緒*，而方法可能會在 await 之後繼續在不同的執行緒上執行。 因此，在第一次檢查時，`Thread.CurrentPrincipal` 會存在，但在呼叫 `await Task.Yield()`之後，會是 null。

從應用程式的 DI 服務集合取得目前使用者的身分識別，也會更具測試性，因為可以輕鬆地插入測試身分識別。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>在 ASP.NET Core 應用程式中取出目前的使用者

有數個選項可讓您在 ASP.NET Core 中抓取目前已驗證的使用者 `ClaimsPrincipal`，以取代 `ClaimsPrincipal.Current`：

* **ControllerBase. User**。 MVC 控制器可以使用其[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性來存取目前已驗證的使用者。
* **HttpCoNtext**。 具有目前 `HttpContext` （例如中介軟體）存取權的元件，可以從[HttpCoNtext](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)取得目前使用者的 `ClaimsPrincipal`。
* **從呼叫端傳入**。 不需要存取目前 `HttpContext` 的程式庫通常是從控制器或中介軟體元件呼叫，而且可以將目前使用者的身分識別當做引數傳遞。
* **IHttpCoNtextAccessor**。 要遷移至 ASP.NET Core 的專案可能太大，而無法輕鬆地將目前使用者的身分識別傳遞至所有必要的位置。 在這種情況下，您可以使用[IHttpCoNtextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)作為因應措施。 `IHttpContextAccessor` 可以存取目前的 `HttpContext` （如果有的話）。 如果正在使用 DI，請參閱 <xref:fundamentals/httpcontext>。 在程式碼中取得目前使用者的身分識別，但尚未更新為使用 ASP.NET Core 的 DI 驅動架構的短期解決方案會是：

  * 藉由呼叫 `Startup.ConfigureServices`中的[AddHttpCoNtextAccessor](https://github.com/aspnet/Hosting/issues/793) ，讓 DI 容器中的 `IHttpContextAccessor` 可供使用。
  * 在啟動期間取得 `IHttpContextAccessor` 的實例，並將它儲存在靜態變數中。 實例可供先前從靜態屬性取得目前使用者的程式碼使用。
  * 使用 `HttpContextAccessor.HttpContext?.User`取出目前使用者的 `ClaimsPrincipal`。 如果在 HTTP 要求的內容外部使用此程式碼，則 `HttpContext` 為 null。

最後一個選項是使用儲存在靜態變數中的 `IHttpContextAccessor` 實例，就跟 ASP.NET Core 原則（偏好插入靜態相依性的相依性）相反。 請改為根據相依性插入，規劃最終取出 `IHttpContextAccessor` 實例。 不過，靜態協助程式可以是很有用的橋接器，但在遷移先前使用 `ClaimsPrincipal.Current`的大型現有 ASP.NET 應用程式時。
