---
title: 處理 ASP.NET Core 中的錯誤
author: rick-anderson
description: 了解如何處理 ASP.NET Core 應用程式中的錯誤。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 07/10/2019
uid: fundamentals/error-handling
ms.openlocfilehash: 652a97a6b7fbe4c8cc678b86a92eea59937e809c
ms.sourcegitcommit: 8835b6777682da6fb3becf9f9121c03f89dc7614
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2019
ms.locfileid: "69975575"
---
# <a name="handle-errors-in-aspnet-core"></a>處理 ASP.NET Core 中的錯誤

作者：[Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex) 及 [Steve Smith](https://ardalis.com/)

本文說明處理 ASP.NET Core 應用程式錯誤的常見方法。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)。 ([如何下載](xref:index#how-to-download-a-sample)。)該文章包含在範例應用程式中設定前置處理器指示詞 (`#if`、`#endif`、`#define`) 以啟用不同案例的指示。

## <a name="developer-exception-page"></a>開發人員例外狀況頁面

「開發人員例外狀況頁面」  會顯示要求例外狀況的詳細資訊。 該頁面是由 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) \(英文\) 套件所提供，其位於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。 當應用程式在開發[環境](xref:fundamentals/environments)中執行時，新增程式碼到 `Startup.Configure` 方法以啟用頁面：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

將對 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> 的呼叫放置於任何您想要攔截例外狀況的中介軟體之前。

> [!WARNING]
> **僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。 當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。 如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。

此頁面包含下列和例外狀況與要求有關的資訊：

* 堆疊追蹤
* 查詢字串參數 (如果有的話)
* Cookie (如果有的話)
* 頁首

若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看開發人員例外狀況頁面，請使用 `DevEnvironment` 前置處理器指示詞，並選取首頁上的 [觸發例外狀況]  。

## <a name="exception-handler-page"></a>例外處理常式頁面

若要針對生產環境設定自訂錯誤處理頁面，請使用例外狀況處理中介軟體。 中介軟體：

* 攔截並記錄例外狀況。
* 可在所指示頁面或控制器的替代管線中重新執行要求。 如果回應已啟動，就不會重新執行要求。

在下列範例中，<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 會在非開發環境中加入例外狀況處理中介軟體：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

Razor Pages 應用程式範本會在 *Pages* 資料夾中提供錯誤頁面 ( *.cshtml*) 與 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 類別 (`ErrorModel`)。 針對 MVC 應用程式，專案範本會包含 Error 動作方法和 Error 檢視。 以下為動作方法：

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

請勿使用 HTTP 方法屬性 (如 `HttpGet`) 裝飾錯誤處理常式動作方法。 明確的動詞命令可防止某些要求取得方法。 允許匿名存取方法，以便未經驗證的使用者能夠收到錯誤檢視。

### <a name="access-the-exception"></a>存取例外狀況

使用 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 來存取例外狀況或和錯誤處理常式控制器或頁面中的原始要求路徑：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> 請**勿**提供敏感性的錯誤資訊給用戶端。 提供錯誤有安全性風險。

若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看例外狀況處理頁面，請使用 `ProdEnvironment` 和 `ErrorHandlerPage` 前置處理器指示詞，並選取首頁上的 [觸發例外狀況]  。

## <a name="exception-handler-lambda"></a>例外處理常式 Lambda

[自訂例外處理常式頁面](#exception-handler-page)的替代方法，便是將 Lambda 提供給 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>。 使用 lambda 可讓您在傳回回應之前存取錯誤。

以下是將 Lambda 用於例外狀況處理的範例：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

> [!WARNING]
> 請**勿**從 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> 或 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 提供錯誤資訊給用戶端。 提供錯誤有安全性風險。

若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看例外狀況處理 Lambda 的結果，請使用 `ProdEnvironment` 和 `ErrorHandlerLambda` 前置處理器指示詞，並選取首頁上的 [觸發例外狀況]  。

## <a name="usestatuscodepages"></a>UseStatusCodePages

根據預設，ASP.NET Core 應用程式不會提供 HTTP 狀態碼 (例如「404 - 找不到」  ) 等狀態碼頁面。 應用程式會傳回狀態碼和空白回應主體。 若要提供狀態碼頁面，請使用狀態碼頁面中介軟體。

該中介軟體是由 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) \(英文\) 套件所提供，其位於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。

若要針對常見的錯誤狀態碼啟用預設的純文字處理常式，請呼叫 `Startup.Configure` 方法中的 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

要求處理中介軟體 (例如靜態檔案中介軟體和 MVC 中介軟體) 之前，應該先呼叫 `UseStatusCodePages`。

以下是預設處理常式所有顯示文字的範例：

```
Status Code: 404; Not Found
```

若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看其中一種狀態碼頁面格式，請使用其中一個以 `StatusCodePages` 為前置處理器指示詞，並選取首頁上的 [觸發 404]  。

## <a name="usestatuscodepages-with-format-string"></a>具格式字串的 UseStatusCodePages

若要自訂回應內容類型和文字，請使用 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 的多載，其會採用內容類型和格式字串：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a>具 Lambda 的 UseStatusCodePages

若要指定自訂錯誤處理和回應撰寫程式碼，請使用 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 的多載，其會採用 Lambda 運算式：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirect"></a>UseStatusCodePagesWithRedirect

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> 擴充方法：

* 傳送「302 - 已找到」  狀態碼傳送給用戶端。
* 將用戶端重新導向到 URL 範本中提供的位置。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

URL 範本可以針對狀態碼包含 `{0}` 預留位置，如範例所示。 如果 URL 範本是以波狀符號 (~) 為開頭，該波狀符號會被應用程式的 `PathBase`取代。 如果您指向應用程式內的端點，請針對該端點建立 MVC 檢視或 Razor 頁面。 如需 Razor Pages 範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)中的 *Pages/StatusCode.cshtml*。

此方法通常是在下列應用程式相關情況下使用：

* 應用程式應將用戶端重新導向至不同的端點時 (通常是在由其他應用程式處理錯誤的情況下)。 針對 Web 應用程式，用戶端的瀏覽器網址列會反映重新導向後的端點。
* 應用程式不應該保留並傳回原始狀態碼與初始重新導向回應時。

## <a name="usestatuscodepageswithreexecute"></a>UseStatusCodePagesWithReExecute

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> 擴充方法：

* 傳回原始狀態碼給用戶端。
* 可透過使用替代路徑重新執行要求管線來產生回應本文。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

如果您指向應用程式內的端點，請針對該端點建立 MVC 檢視或 Razor 頁面。 如需 Razor Pages 範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)中的 *Pages/StatusCode.cshtml*。

此方法通常是在下列應用程式相關情況下使用：

* 在不重新導向至其他端點的情況下處理要求。 針對 Web 應用程式，用戶端的瀏覽器網址列會反映原始要求的端點。
* 保留並傳回原始狀態碼與回應時。

URL 和查詢字串範本可能會包含該狀態碼的預留位置 (`{0}`)。 URL 範本的開頭必須是斜線 (`/`)。 在路徑中使用預留位置時，請確認端點 (頁面或控制器) 可以處理路徑線段。 例如適用於錯誤的 Razor Page 應接受具備 `@page` 指示詞的選擇性路徑區段值：

```cshtml
@page "{code?}"
```

處理錯誤的端點可以取得產生該錯誤的原始 URL，如下列範例所示：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a>停用狀態碼頁面

若要停用 MVC 控制器或動作方法的狀態碼頁面，請使用 [[SkipStatusCodePages]](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) 屬性。

若要停用 Razor 頁面處理常式方法或 MVC 控制器中的特定要求狀態碼頁面，請使用 <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>：

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a>例外狀況處理程式碼

例外狀況處理頁面中的程式碼，可擲回例外狀況。 一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。

### <a name="response-headers"></a>回應標頭

一旦傳送回應的標頭之後：

* 應用程式就無法變更回應的狀態碼。
* 無法執行任何例外狀況頁面或處理常式。 回應必須完成，否則會中止連線。

## <a name="server-exception-handling"></a>伺服器例外狀況處理

除了應用程式中的例外狀況處理邏輯之外，[HTTP 伺服器實作](xref:fundamentals/servers/index)也可以處理一些例外狀況。 如果伺服器在回應標頭傳送之前攔截到例外狀況，伺服器會傳送「500 - 內部伺服器錯誤」  回應，且沒有回應本文。 如果伺服器在回應標頭傳送之後攔截到例外狀況，伺服器會關閉連線。 應用程式未處理的要求會由伺服器來處理。 當伺服器處理要求時，任何發生的例外狀況均由伺服器的例外狀況處理功能來處理。 應用程式的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響此行為。

## <a name="startup-exception-handling"></a>啟動例外狀況處理

只有裝載層可以處理應用程式啟動期間發生的例外狀況。 可以將主機設定為會[擷取啟動錯誤](xref:fundamentals/host/web-host#capture-startup-errors)和[擷取詳細錯誤](xref:fundamentals/host/web-host#detailed-errors)。

只有在錯誤是於主機位址/連接埠繫結之後發生的情況下，裝載層才能顯示已擷取之啟動錯誤的錯誤頁面。 如果繫結失敗：

* 裝載層會記錄重大例外狀況。
* Dotnet 會處理損毀狀況。
* 當 HTTP 伺服器是 [Kestrel](xref:fundamentals/servers/kestrel) 時，不會顯示任何錯誤頁面。

在 [IIS](/iis) (或 Azure App Service) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:host-and-deploy/aspnet-core-module)會傳回 *502.5 - 處理序失敗*。 如需詳細資訊，請參閱 <xref:test/troubleshoot-azure-iis>。

## <a name="database-error-page"></a>資料庫錯誤頁面

[資料庫錯誤頁面](<xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*>)中介軟體能擷取資料庫相關的例外狀況，其可透過使用 Entity Framework 移轉來解決。 發生這些例外狀況時，系統會產生具解決該問題之可能動作詳細資料的 HTML 回應。 此頁面僅應該於開發環境中啟用。 將程式碼加入 `Startup.Configure` 來啟用該頁面：

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

## <a name="exception-filters"></a>例外狀況篩選條件

在 MVC 應用程式中，您能以全域設定例外狀況篩選條件，或是以每個控制器或每個動作為基礎的方式設定。 在 Razor Pages 應用程式中，您能以全域或每個頁面模型的方式設定它們。 這些篩選會處理在控制器動作或其他篩選條件執行期間發生但的任何未處理例外狀況。 如需詳細資訊，請參閱 <xref:mvc/controllers/filters#exception-filters>。

> [!TIP]
> 例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像例外狀況處理中介軟體那麼有彈性。 我們建議使用中介軟體。 請只在需要根據已選擇的 MVC 動作執行不同的錯誤處理時，才使用篩選條件。

## <a name="model-state-errors"></a>模型狀態錯誤

如需如何處理模型狀態錯誤的相關資訊，請參閱[模型繫結](xref:mvc/models/model-binding)和[模型驗證](xref:mvc/models/validation)。

## <a name="additional-resources"></a>其他資源

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
