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
ms.openlocfilehash: 4bcbaa1854016d7393d019a9c275bc8221974d7a
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84145612"
---
# <a name="migrate-from-claimsprincipalcurrent"></a>從 ClaimsPrincipal 遷移

在 ASP.NET 4.x 專案中，通常會使用[ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal.current)來抓取目前已驗證使用者的身分識別和宣告。 在 ASP.NET Core 中，已不再設定此屬性。 必須更新程式碼，以透過不同的方法來取得目前已驗證的使用者身分識別。

## <a name="context-specific-state-instead-of-static-state"></a>內容特定狀態，而不是靜態狀態

使用 ASP.NET Core 時， `ClaimsPrincipal.Current` 不會設定和的值 `Thread.CurrentPrincipal` 。 這些屬性都代表靜態狀態，ASP.NET Core 通常會避免這種情況。 相反地，ASP.NET Core 會使用相依性[插入](xref:fundamentals/dependency-injection)（DI）來提供相依性，例如目前使用者的身分識別。 從 DI 取得目前使用者的身分識別也會更具測試性，因為可以輕鬆地插入測試身分識別。

## <a name="retrieve-the-current-user-in-an-aspnet-core-app"></a>在 ASP.NET Core 應用程式中取出目前的使用者

有數個選項可讓您在 ASP.NET Core 中抓取目前已驗證的使用者， `ClaimsPrincipal` 以取代 `ClaimsPrincipal.Current` ：

* **ControllerBase. User**。 MVC 控制器可以使用其[使用者](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.user)屬性來存取目前已驗證的使用者。
* **HttpCoNtext**。 具有目前 `HttpContext` （中介軟體）存取權的元件可以從 HttpCoNtext 取得目前的使用者 `ClaimsPrincipal` 。 [HttpContext.User](/dotnet/api/microsoft.aspnetcore.http.httpcontext.user)
* **從呼叫端傳入**。 沒有目前存取權的程式庫 `HttpContext` 通常是從控制器或中介軟體元件呼叫，而且可以將目前使用者的身分識別當做引數傳遞。
* **IHttpCoNtextAccessor**。 要遷移至 ASP.NET Core 的專案可能太大，而無法輕鬆地將目前使用者的身分識別傳遞至所有必要的位置。 在這種情況下，您可以使用[IHttpCoNtextAccessor](/dotnet/api/microsoft.aspnetcore.http.ihttpcontextaccessor)作為因應措施。 `IHttpContextAccessor`能夠存取目前的 `HttpContext` （如果有的話）。 如果正在使用 DI，請參閱 <xref:fundamentals/httpcontext> 。 在程式碼中取得目前使用者的身分識別，但尚未更新為使用 ASP.NET Core 的 DI 驅動架構的短期解決方案會是：

  * 在 `IHttpContextAccessor` 中呼叫[AddHttpCoNtextAccessor](https://github.com/aspnet/Hosting/issues/793) ，使其可在 DI 容器中使用 `Startup.ConfigureServices` 。
  * 在 `IHttpContextAccessor` 啟動期間取得實例，並將它儲存在靜態變數中。 實例可供先前從靜態屬性取得目前使用者的程式碼使用。
  * 使用取得目前使用者的 `ClaimsPrincipal` `HttpContextAccessor.HttpContext?.User` 。 如果在 HTTP 要求的內容外部使用此程式碼，則 `HttpContext` 為 null。

使用儲存在靜態變數中的實例的最後一個選項，違反了偏好插入靜態相依性之相依性的 `IHttpContextAccessor` ASP.NET Core 原則。 請改為規劃最終 `IHttpContextAccessor` 從 DI 取得實例。 不過，靜態協助程式在遷移使用的大型現有 ASP.NET 應用程式時，可能是很有用的橋接器 `ClaimsPrincipal.Current` 。
