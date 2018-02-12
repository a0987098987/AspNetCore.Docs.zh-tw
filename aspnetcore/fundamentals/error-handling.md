---
title: "ASP.NET Core 中的錯誤處理"
author: ardalis
description: "了解如何在 ASP.NET Core 應用程式中處理錯誤。"
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 5b0cda7b79b8a9523d1ba6a9b321d22d3ccc753a
ms.sourcegitcommit: 18d1dc86770f2e272d93c7e1cddfc095c5995d9e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a>ASP.NET Core 中的錯誤處理簡介

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)

本文涵蓋處理 ASP.NET Core 應用程式錯誤的常見方法。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>開發人員例外狀況頁面

若要設定應用程式以顯示例外狀況的詳細資訊頁面，請安裝 `Microsoft.AspNetCore.Diagnostics` NuGet 套件，並於[在 Startup 類別中設定方法](startup.md)中新增一行文字：

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

將 `UseDeveloperExceptionPage` 放在任何您要攔截例外狀況到其中的中介軟體之前，例如 `app.UseMvc`。

>[!WARNING]
> **僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。 當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。 [進一步了解環境的設定](environments.md)。

若要查看開發人員例外狀況頁面，請將環境設定為 `Development`，並將 `?throw=true` 新增至應用程式的基底 URL，以執行範例應用程式。 此頁面包含數個索引標籤，內含例外狀況與要求的相關資訊。 第一個索引標籤包含堆疊追蹤。 

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

下一個索引標籤則會顯示查詢字串參數 (如果有的話)。

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

此要求沒有任何 Cookie；但如果有，它們會出現在 [Cookie] 索引標籤上。您可以看到傳遞至最後一個索引標籤的標頭。

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>設定自訂的例外狀況處理頁面

當應用程式不在 `Development` 環境中執行時，建議您設定使用例外處理常式頁面。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

在 MVC 應用程式中，請不要使用 HTTP 方法屬性 (例如 `HttpGet`) 明確裝飾錯誤處理常式的動作方法。 使用明確的動詞時，可能會導致方法收不到某些要求。

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a>設定狀態碼頁面

根據預設，您的應用程式不會提供 HTTP 狀態碼 500 (內部伺服器錯誤) 或 404 (找不到) 等豐富的狀態碼頁面。 您可以將這一行新增至 `Configure` 方法，以設定 `StatusCodePagesMiddleware`：

```csharp
app.UseStatusCodePages();
```

根據預設，此中介軟體會針對常見狀態碼 (如 404) 新增簡單的純文字處理常式：

![404 頁面](error-handling/_static/default-404-status-code.png)

中介軟體可支援幾種不同的擴充方法。 其中一種是採用 Lambda 運算式，另一個則是採用內容類型和格式字串，

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

也有重新導向的擴充方法。 其中一個會將 302 狀態碼傳送給用戶端，而另一個會將原始的狀態碼傳回給用戶端，同時也會針對重新導向 URL 執行處理常式。

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

如果您需要停用特定要求的狀態碼頁面，您可以這樣做：

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>例外狀況處理程式碼

例外狀況處理頁面中的程式碼，可擲回例外狀況。 一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。

另請注意，一旦傳送回應標頭之後，您就無法變更回應的狀態碼，也不能執行任何例外狀況頁面或處理常式。 回應必須完成，否則會中止連線。

## <a name="server-exception-handling"></a>伺服器例外狀況處理

除了應用程式中的例外狀況處理邏輯，裝載應用程式的[伺服器](servers/index.md)也會執行一些例外狀況處理。 如果伺服器在標頭傳送之前攔截到例外狀況，則伺服器會傳送「500 內部伺服器錯誤」回應，且沒有內內文。 如果伺服器在標頭傳送之後攔截到例外狀況，則伺服器會關閉連線。 應用程式未處理的要求會由伺服器來處理。 任何發生的例外狀況均由伺服器的例外狀況功能來處理。 任何已設定的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響這個行為。

## <a name="startup-exception-handling"></a>啟動例外狀況處理

只有裝載層可以處理應用程式啟動期間發生的例外狀況。 您可以使用 `captureStartupErrors` 和 `detailedErrors` 索引鍵，[設定主機對啟動期間所發生錯誤的回應行為](hosting.md#detailed-errors)。

如果錯誤是在主機位址/連接埠繫結之後發生，則裝載層只會顯示擷取到的啟動錯誤的錯誤頁面。 如果繫結因為任何原因失敗，裝載層會記錄 dotnet 處理序損毀的重大例外狀況，但不會顯示任何錯誤頁面。

## <a name="aspnet-mvc-error-handling"></a>ASP.NET MVC 錯誤處理

[MVC](xref:mvc/overview) 應用程式提供一些其他選項以處理錯誤，例如設定例外狀況篩選條件，以及執行模型驗證。

### <a name="exception-filters"></a>例外狀況篩選條件

在 MVC 應用程式中，您可以全域設定例外狀況篩選條件，或以每個控制器或每個動作基準來設定。 這些篩選條件會處理任何在控制器動作或其他篩選條件執行期間發生但未處理的例外狀況；否則的話，就不會呼叫這些篩選條件。 如需深入了解例外狀況篩選條件，請參閱[篩選條件](../mvc/controllers/filters.md)。

>[!TIP]
> 例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像錯誤處理中介軟體那麼有彈性。 一般情況下通常使用中介軟體；只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選條件。

### <a name="handling-model-state-errors"></a>處理模型狀態錯誤

在叫用每個控制器動作之前，會先進行[模型驗證](../mvc/models/validation.md)，而動作方法必須負責檢查 `ModelState.IsValid` 並做出適當回應。

某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤；在這種情況下，就很適合在[篩選條件](../mvc/controllers/filters.md)中實作這類原則。 您應該測試您的動作在無效模型狀態中有何行為。 深入了解[測試控制器邏輯](../mvc/controllers/testing.md)。



