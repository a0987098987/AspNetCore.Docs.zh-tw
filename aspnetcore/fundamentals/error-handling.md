---
title: "ASP.NET Core 中的錯誤處理"
author: ardalis
description: "說明如何在 ASP.NET Core 應用程式中處理錯誤"
keywords: "ASP.NET Core，錯誤處理的例外狀況處理，"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.assetid: 4db51023-c8a6-4119-bbbe-3917e272c260
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 96a4fed19887a7a9eba08ec70296147f22e41569
ms.sourcegitcommit: 368aabde4de3728a8e5a8c016a2ec61f9c0854bf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>Introduction to ASP.NET Core 中的錯誤處理

由[Steve Smith](https://ardalis.com/)和[Tom Dykstra](https://github.com/tdykstra/)

本文涵蓋常見 appoaches 處理 ASP.NET Core 應用程式中的錯誤。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a>[開發人員的例外狀況] 頁面

若要設定應用程式顯示一個頁面，會顯示例外狀況的相關詳細的資訊，請安裝`Microsoft.AspNetCore.Diagnostics`NuGet 封裝，並將一條線[啟動類別中設定方法](startup.md):

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

Put`UseDeveloperExceptionPage`之前您要攔截例外狀況中，例如任何中介軟體`app.UseMvc`。

>[!WARNING]
> 啟用開發人員例外狀況頁面**只有應用程式在執行時在開發環境中**。 您不想在生產環境中執行的應用程式時，公開共用詳細例外狀況資訊。 [深入了解設定環境](environments.md)。

若要查看 [開發人員的例外狀況] 頁面，執行範例應用程式的環境設定為`Development`，並加入`?throw=true`基底 url 的應用程式。 此頁面包含例外狀況，並要求的相關資訊的數個索引標籤。 第一個索引標籤包含堆疊追蹤。 

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

如果有的話，[下一步] 索引標籤將顯示的查詢字串參數。

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

此要求沒有任何 cookie，但如果有，它們會出現在上**Cookie**  索引標籤。您可以看到最後一個索引標籤中所傳遞的標頭。

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>設定自訂的例外狀況處理頁面

它是個不錯的主意設定不以執行應用程式時所要使用的例外狀況處理常式頁面`Development`環境。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

在 MVC 應用程式中不明確裝飾錯誤處理常式的動作方法，以 HTTP 方法的屬性，例如`HttpGet`。 使用明確的動詞命令無法防止某些要求的方法。

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>設定狀態的字碼頁

根據預設，您的應用程式不會提供豐富的狀態字碼頁的 HTTP 狀態碼 500 （內部伺服器錯誤） 或 404 （找不到） 等。 您可以設定`StatusCodePagesMiddleware`將這一行加入`Configure`方法：

```csharp
app.UseStatusCodePages();
```

根據預設，此中介軟體會加入常見狀態碼 404 等簡單、 純文字的處理常式：

![404 頁面](error-handling/_static/default-404-status-code.png)

中介軟體可支援幾種不同的擴充方法。 另一個則採用 lambda 運算式，另一個採用內容類型和格式字串。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

也有重新導向的擴充方法。 其中一個會 302 狀態碼傳送給用戶端，而其中一個原始的狀態碼傳回給用戶端，但也會重新導向 URL 執行此處理常式。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

如果您需要停用狀態的特定要求的字碼頁，您可以這樣做：

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>例外狀況處理程式碼

例外狀況處理網頁中的程式碼，可擲回例外狀況。 它通常是個不錯的主意，生產環境錯誤頁來組成純粹靜態內容。

此外，請注意，一旦傳送回應標頭之後，您無法變更回應的狀態碼，也不可以任何例外狀況網頁或處理常式執行。 回應必須完成或中止連線。

## <a name="server-exception-handling"></a>伺服器例外狀況處理

除了例外狀況處理應用程式中的邏輯[伺服器](servers/index.md)裝載您的應用程式會執行一些例外狀況處理。 如果標頭傳送之前，伺服器會攔截例外狀況，伺服器會傳送 500 內部伺服器錯誤回應，且沒有主體。 如果傳送的標頭後，伺服器會攔截例外狀況，伺服器就會關閉連接。 不由您的應用程式的要求是由伺服器處理。 任何發生的例外狀況由伺服器的例外狀況處理。 任何設定自訂錯誤網頁，或例外狀況處理中介軟體或篩選器不會影響這個行為。

## <a name="startup-exception-handling"></a>啟動例外狀況處理

只裝載層可以處理應用程式啟動期間發生的例外狀況。 您可以[設定主應用程式的運作方式的錯誤回應在啟動期間](hosting.md#detailed-errors)使用`captureStartupErrors`和`detailedErrors`索引鍵。

裝載可以只會顯示錯誤頁面擷取的啟動錯誤，如果主機位址/連接埠繫結之後，就會發生錯誤。 如果任何繫結失敗，因為任何原因，裝載層記錄重大例外狀況 dotnet 處理序損毀，並不顯示任何錯誤頁面。

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC 的錯誤處理

[MVC](../mvc/index.md)應用程式有錯誤，例如設定例外狀況篩選條件，以及執行模型驗證處理一些其他選項。

### <a name="exception-filters"></a>例外狀況篩選條件

全域或根據每個控制站或每個動作的 MVC 應用程式中，可以設定例外狀況篩選條件。 這些篩選器處理控制器動作，或另一個篩選，在執行期間發生任何未處理例外狀況，而不會呼叫否則。 深入了解例外狀況篩選條件中[篩選](../mvc/controllers/filters.md)。

>[!TIP]
> 例外狀況篩選條件則適合用於設陷 MVC 動作中發生的例外狀況，但是它們並不具有彈性，錯誤處理中介軟體。 偏好以一般的情況下中, 介軟體，並使用篩選器只需要執行錯誤處理*不同*根據選擇的 MVC 動作。

### <a name="handling-model-state-errors"></a>處理模型狀態錯誤

[模型驗證](../mvc/models/validation.md)發生之前所叫用每個控制器動作和動作方法必須負責檢查`ModelState.IsValid`和作出適當回應。

某些應用程式都可以選擇在此情況下遵循標準處理模型驗證錯誤，慣例[篩選](../mvc/controllers/filters.md)可能是適當的位置，實作這類的原則。 您應該測試您的動作具有無效的模型狀態的行為方式。 進一步了解[測試控制器邏輯](../mvc/controllers/testing.md)。



