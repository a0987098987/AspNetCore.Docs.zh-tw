---
title: 處理 ASP.NET Core 中的錯誤
author: tdykstra
description: 了解如何處理 ASP.NET Core 應用程式中的錯誤。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: d809c70b3fae6b2d21d5ec0871298d905b873d5d
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665359"
---
# <a name="handle-errors-in-aspnet-core"></a>處理 ASP.NET Core 中的錯誤

作者：[Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex) 及 [Steve Smith](https://ardalis.com/)

本文說明處理 ASP.NET Core 應用程式錯誤的常見方法。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="developer-exception-page"></a>開發人員例外狀況頁面

若要設定應用程式以顯示提供要求例外狀況詳細資訊的頁面，請使用「開發人員例外狀況頁面」。 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件 (於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)提供) 會提供該頁面。 當應用程式在開發[環境](xref:fundamentals/environments)中執行時，新增一行到 `Startup.Configure` 方法：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseDeveloperExceptionPage)]

將對 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> 的呼叫放置於任何想要攔截例外狀況的中介軟體之前。

> [!WARNING]
> **僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。 當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。 如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。

若要查看開發人員例外狀況頁面，請將環境設定為 `Development` 並執行範例應用程式，然後將 `?throw=true` 新增至應用程式的基底 URL。 此頁面包含下列和例外狀況與要求有關的資訊：

* 堆疊追蹤
* 查詢字串參數 (如果有的話)
* Cookie (如果有的話)
* 頁首

## <a name="configure-a-custom-exception-handling-page"></a>設定自訂的例外狀況處理頁面

當應用程式不在開發環境中執行時，請呼叫 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 擴充方法來新增例外狀況處理中介軟體。 中介軟體：

* 可攔截例外狀況。
* 可記錄例外狀況。
* 可在所指示頁面或控制器的替代管線中重新執行要求。 如果回應已啟動，就不會重新執行要求。

在範例應用程式的下列範例中，<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 會在非開發環境中加入例外狀況處理中介軟體。 擴充方法會在攔截並記錄例外狀況之後，針對重新執行的要求，於 `/Error` 端點指定一個錯誤頁面或控制器：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler1)]

Razor Pages 應用程式範本會在 Pages 資料夾中，提供一個錯誤頁面 (*.cshtml*) 與 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 類別 (`ErrorModel`)。

在 MVC 應用程式中，MVC 應用程式範本中會包含下列錯誤處理常式方法，並出現在 Home 控制器中：

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

請勿使用 HTTP 方法屬性 (如 `HttpGet`) 裝飾錯誤處理常式動作方法。 明確的動詞命令可防止某些要求取得方法。 允許匿名存取方法，以便未經驗證的使用者能夠收到錯誤檢視。

## <a name="access-the-exception"></a>存取例外狀況

使用 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 來存取例外狀況或控制器或頁面中的原始要求路徑：

* 您可以從 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature.Path> 屬性取得路徑。
* 從繼承的 [IExceptionHandlerFeature.Error](xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature.Error) 屬性讀取 <xref:System.Exception?displayProperty=fullName>。

```csharp
// using Microsoft.AspNetCore.Diagnostics;

var exceptionHandlerPathFeature = 
    HttpContext.Features.Get<IExceptionHandlerPathFeature>();
var path = exceptionHandlerPathFeature?.Path;
var error = exceptionHandlerPathFeature?.Error;
```

> [!WARNING]
> 請**勿**從 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> 或 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 提供錯誤資訊給用戶端。 提供錯誤有安全性風險。

## <a name="configure-custom-exception-handling-code"></a>設定自訂例外狀況處理程式碼

為端點提供服務以處理具有[自訂例外狀況處理頁面](#configure-a-custom-exception-handling-page)之錯誤的方式是提供 lambda 給 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>。 搭配 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 使用 lambda 可讓您在傳回回應之前存取錯誤。

範例應用程式示範 `Startup.Configure` 中的自訂例外狀況處理程式碼。 使用 [索引] 頁面上的 [擲回例外狀況] 連結觸發例外狀況。 下列 lambda 會執行：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_UseExceptionHandler2)]

> [!WARNING]
> 請**勿**從 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> 或 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 提供錯誤資訊給用戶端。 提供錯誤有安全性風險。

## <a name="configure-status-code-pages"></a>設定狀態碼頁面

根據預設，ASP.NET Core 應用程式不會提供 HTTP 狀態碼 (例如「404 - 找不到」) 等狀態碼頁面。 應用程式會傳回狀態碼和空白回應主體。 若要提供狀態碼頁面，請使用狀態碼頁面中介軟體。

[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件 (於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)提供) 會提供中介軟體。

將一行程式碼新增至 `Startup.Configure` 方法：

```csharp
app.UseStatusCodePages();
```

要求處理中介軟體之前，應該先呼叫 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 方法 (例如，靜態檔案中介軟體和 MVC 中介軟體)。

根據預設，狀態碼頁面中介軟體會針對常見狀態碼 (例如「404 - 找不到」) 新增純文字處理常式：

```
Status Code: 404; Not Found
```

中介軟體可支援幾種擴充方法，可允許您自訂其行為。

多載 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 會採用 lambda 運算式，可用來處理自訂錯誤處理邏輯，並手動撰寫回應：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

多載 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 會採用內容類型和格式字串，可用來自訂內容類型和回應文字：

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a>重新導向並重新執行擴充方法

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>：

* 傳送「302 - 已找到」狀態碼傳送給用戶端。
* 將用戶端重新導向到 URL 範本中提供的位置。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> 通常是在下列應用程式相關情況下使用：

* 應用程式應將用戶端重新導向至不同的端點時 (通常是在由其他應用程式處理錯誤的情況下)。 針對 Web 應用程式，用戶端的瀏覽器網址列會反映重新導向後的端點。
* 應用程式不應該保留並傳回原始狀態碼與初始重新導向回應時。

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>：

* 傳回原始狀態碼給用戶端。
* 可透過使用替代路徑重新執行要求管線來產生回應本文。

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> 通常是在應用程式應執行以下作業時使用：

* 在不重新導向至其他端點的情況下處理要求。 針對 Web 應用程式，用戶端的瀏覽器網址列會反映原始要求的端點。
* 保留並傳回原始狀態碼與回應時。

範本可能包含該狀態碼的預留位置 (`{0}`)。 範本的開頭必須是正斜線 (`/`)。 使用預留位置時，請確認端點 (頁面或控制器) 可以處理路徑區段。 例如適用於錯誤的 Razor Page 應接受具備 `@page` 指示詞的選擇性路徑區段值：

```cshtml
@page "{code?}"
```

在 Razor 頁面處理常式方法或 MVC 控制器中，可以針對特定要求停用狀態碼頁面。 若要停用狀態碼頁面，請嘗試從要求的 [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) 集合中擷取 <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>，然後停用此功能 (若可用的話)：

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

若要在應用程式中使用指向端點的 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 多載，請建立該端點的 MVC 檢視或 Razor 頁面。 例如，Razor Pages 應用程式範本會產生下列頁面和頁面模型類別：

*Error.cshtml*：

::: moniker range=">= aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to the <strong>Development</strong> environment displays 
    detailed information about the error that occurred.
</p>
<p>
    <strong>The Development environment shouldn't be enabled for deployed 
    applications.</strong> It can result in displaying sensitive information 
    from exceptions to end users. For local debugging, enable the 
    <strong>Development</strong> environment by setting the 
    <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to 
    <strong>Development</strong> and restarting the app.
</p>
```

*Error.cshtml.cs*：

```csharp
[ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

```cshtml
@page
@model ErrorModel
@{
    ViewData["Title"] = "Error";
}

<h1 class="text-danger">Error.</h1>
<h2 class="text-danger">An error occurred while processing your request.</h2>

@if (Model.ShowRequestId)
{
    <p>
        <strong>Request ID:</strong> <code>@Model.RequestId</code>
    </p>
}

<h3>Development Mode</h3>
<p>
    Swapping to <strong>Development</strong> environment will display more detailed 
    information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications
    </strong>, as it can result in sensitive information from exceptions being 
    displayed to end users. For local debugging, development environment can be 
    enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment 
    variable to <strong>Development</strong>, and restarting the application.
</p>
```

*Error.cshtml.cs*：

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, 
        NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

::: moniker-end

## <a name="exception-handling-code"></a>例外狀況處理程式碼

例外狀況處理頁面中的程式碼，可擲回例外狀況。 一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。

另外也請注意，一旦傳送回應的標頭之後：

* 應用程式就無法變更回應的狀態碼。
* 無法執行任何例外狀況頁面或處理常式。 回應必須完成，否則會中止連線。

## <a name="server-exception-handling"></a>伺服器例外狀況處理

除了應用程式中的例外狀況處理邏輯之外，[伺服器實作](xref:fundamentals/servers/index)也可以處理一些例外狀況。 如果伺服器在回應標頭傳送之前攔截到例外狀況，伺服器會傳送「500 - 內部伺服器錯誤」回應，且沒有回應本文。 如果伺服器在回應標頭傳送之後攔截到例外狀況，伺服器會關閉連線。 應用程式未處理的要求會由伺服器來處理。 當伺服器處理要求時，任何發生的例外狀況均由伺服器的例外狀況處理功能來處理。 應用程式的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響此行為。

## <a name="startup-exception-handling"></a>啟動例外狀況處理

只有裝載層可以處理應用程式啟動期間發生的例外狀況。 您可以使用 [Web 主機](xref:fundamentals/host/web-host)，將 `captureStartupErrors` 與 `detailedErrors` 索引鍵搭配使用來[設定主機對啟動期間所發生錯誤的回應行為](xref:fundamentals/host/web-host#detailed-errors)。

如果錯誤是在主機位址/連接埠繫結之後發生，則裝載層只會顯示擷取到的啟動錯誤的錯誤頁面。 如果任何繫結因為任何原因而失敗：

* 裝載層會記錄重大例外狀況。
* Dotnet 會處理損毀狀況。
* 當應用程式在 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器上執行時，不會顯示任何錯誤頁面。

在 [IIS](/iis) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:host-and-deploy/aspnet-core-module)會傳回 *502.5 - 處理序失敗*。 如需詳細資訊，請參閱<xref:host-and-deploy/iis/troubleshoot>。 如需使用 Azure App Service 針對啟動問題進行疑難排解的資訊，請參閱 <xref:host-and-deploy/azure-apps/troubleshoot>。

## <a name="aspnet-core-mvc-error-handling"></a>ASP.NET Core MVC 錯誤處理

[MVC](xref:mvc/overview) 應用程式提供一些其他選項以處理錯誤，例如設定例外狀況篩選條件，以及執行模型驗證。

### <a name="exception-filters"></a>例外狀況篩選條件

在 MVC 應用程式中，您可以全域設定例外狀況篩選條件，或以每個控制器或每個動作基準來設定。 這些篩選會處理在控制器動作或其他篩選條件執行期間發生但的任何未處理例外狀況。 否則不呼叫這些篩選。 如需詳細資訊，請參閱<xref:mvc/controllers/filters#exception-filters>。

> [!TIP]
> 例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像例外狀況處理中介軟體那麼有彈性。 我們建議使用中介軟體。 請只在需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選條件。

### <a name="handle-model-state-errors"></a>處理模型狀態錯誤

在叫用每個控制器動作之前，會先進行[模型驗證](xref:mvc/models/validation)，而動作方法必須負責檢查 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) 並適當地回應。

某些應用程式會選擇遵循標準慣例來處理[模型驗證](xref:mvc/models/validation)錯誤；在這種情況下，就很適合在[篩選](xref:mvc/controllers/filters)中實作這類原則。 您應該測試您的動作在無效模型狀態中有何行為。 如需詳細資訊，請參閱<xref:mvc/controllers/testing>。

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
