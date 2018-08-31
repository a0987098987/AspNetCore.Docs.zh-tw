---
title: 處理 ASP.NET Core 中的錯誤
author: ardalis
description: 了解如何處理 ASP.NET Core 應用程式中的錯誤。
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/05/2018
uid: fundamentals/error-handling
ms.openlocfilehash: ff04ebeb6a682ec924afe896fd6716010a63f7cd
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41751475"
---
# <a name="handle-errors-in-aspnet-core"></a>處理 ASP.NET Core 中的錯誤

作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)

本文說明處理 ASP.NET Core 應用程式錯誤的常見方法。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x/ErrorHandlingSample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="the-developer-exception-page"></a>開發人員例外狀況頁面

::: moniker range=">= aspnetcore-2.1"

若要設定應用程式以顯示提供例外狀況詳細資訊的頁面，請使用「開發人員例外狀況頁面」。 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件 (提供於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)) 會提供該頁面。 將一行程式碼新增至 `Startup.Configure` 方法：

::: moniker-end

::: moniker range="= aspnetcore-2.0"

若要設定應用程式以顯示提供例外狀況詳細資訊的頁面，請使用「開發人員例外狀況頁面」。 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件 (提供於 [Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)) 會提供該頁面。 將一行程式碼新增至 `Startup.Configure` 方法：

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要設定應用程式以顯示提供例外狀況詳細資訊的頁面，請使用「開發人員例外狀況頁面」。 在專案檔中新增 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件的套件參考可提供該頁面。 將一行程式碼新增至 `Startup.Configure` 方法：

::: moniker-end

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

將對 [UseDeveloperExceptionPage](/dotnet/api/microsoft.aspnetcore.builder.developerexceptionpageextensions.usedeveloperexceptionpage) 的呼叫放置於任何您要攔截例外狀況 (例如 `app.UseMvc`) 的中介軟體之前。

>[!WARNING]
> **僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。 當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。 [進一步了解環境的設定](xref:fundamentals/environments)。

若要查看開發人員例外狀況頁面，請將環境設定為 `Development` 並執行範例應用程式，然後將 `?throw=true` 新增至應用程式的基底 URL。 此頁面包含數個索引標籤，內含例外狀況與要求的相關資訊。 第一個索引標籤包含堆疊追蹤：

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

下一個索引標籤會顯示查詢字串參數 (如果有)：

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

如果要求具有 cookie，它們會顯示在 [Cookie] 索引標籤。標頭會顯示在最後一個索引標籤中：

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a>設定自訂的例外狀況處理頁面

當應用程式不在 `Development` 環境中執行時，請設定使用例外處理常式頁面：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

在 Razor Pages 應用程式中，[dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages 範本會在 [頁面] 資料夾中提供 [錯誤] 頁面和 `ErrorModel` 頁面模型類別。

在 MVC 應用程式中，請勿使用 HTTP 方法屬性 (如 `HttpGet`) 裝飾錯誤處理常式動作方法。 明確的動詞命令可防止某些要求取得方法。 允許匿名存取方法，以便未經驗證的使用者能夠收到錯誤檢視。

例如，[dotnet new](/dotnet/core/tools/dotnet-new) MVC 範本提供的下列錯誤處理常式方法會出現在主控制器中：

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a>設定狀態碼頁面

根據預設，應用程式不會提供 HTTP 狀態碼「404 (找不到)」等豐富的狀態碼頁面。 若要提供狀態碼頁面，請將下一行新增至 `Startup.Configure` 方法，以設定狀態碼頁面中介軟體：

```csharp
app.UseStatusCodePages();
```

根據預設，狀態碼頁面中介軟體會針對常見狀態碼 (例如 404) 新增純文字處理常式：

![404 頁面](error-handling/_static/default-404-status-code.png)

中介軟體可支援幾種擴充方法。 其中一種方法採用 Lambda 運算式：

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

另一個方法則採用內容類型和格式字串：

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

也有重新導向和重新執行的擴充方法。 重新導向方法會將 *302 已找到*狀態碼，傳送到用戶端，並將該用戶端重新導向至提供的位置 URL 範本。 範本可能包含該狀態碼的 `{0}` 預留位置。 開頭為 `~` 的 URL，已於前方加上基底路徑。 未以 `~` 開頭的 URL，會原狀使用。

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

重新執行方法會將原始狀態碼傳回給用戶端，並會指定應由透過使用替代路徑來重新執行要求管線的方法，產生回應主體。 此路徑可包含以下狀態碼的 `{0}` 預留位置：

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

在 Razor 頁面處理常式方法或 MVC 控制器中，可以針對特定要求停用狀態碼頁面。 若要停用狀態碼頁面，請嘗試從要求的 [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) 集合中擷取 [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature)，然後停用此功能 (若可用的話)：

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

如果在應用程式中使用指向端點的 `UseStatusCodePages*` 多載，請建立該端點的 MVC 檢視或 Razor 頁面。 例如，Razor Pages 應用程式的 [dotnet new](/dotnet/core/tools/dotnet-new) 範本會產生下列頁面和頁面模型類別：

*Error.cshtml*：

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

## <a name="exception-handling-code"></a>例外狀況處理程式碼

例外狀況處理頁面中的程式碼，可擲回例外狀況。 一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。

另請注意，一旦傳送回應標頭之後，您就無法變更回應的狀態碼，也不能執行任何例外狀況頁面或處理常式。 回應必須完成，否則會中止連線。

## <a name="server-exception-handling"></a>伺服器例外狀況處理

除了應用程式中的例外狀況處理邏輯，裝載應用程式的[伺服器](xref:fundamentals/servers/index)也會執行一些例外狀況處理。 如果伺服器在標頭傳送之前攔截到例外狀況，則伺服器會傳送「500 內部伺服器錯誤」回應，且沒有本文。 如果伺服器在標頭傳送之後攔截到例外狀況，則伺服器會關閉連線。 應用程式未處理的要求會由伺服器來處理。 任何發生的例外狀況均由伺服器的例外狀況功能來處理。 任何已設定的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響這個行為。

## <a name="startup-exception-handling"></a>啟動例外狀況處理

只有裝載層可以處理應用程式啟動期間發生的例外狀況。 使用 [Web 主機](xref:fundamentals/host/web-host)，您可以將 `captureStartupErrors` 與 `detailedErrors` 索引鍵搭配使用來[設定主機對啟動期間所發生錯誤的回應行為](xref:fundamentals/host/web-host#detailed-errors)。

如果錯誤是在主機位址/連接埠繫結之後發生，則裝載層只會顯示擷取到的啟動錯誤的錯誤頁面。 在 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器上執行應用程式時，如果繫結因為任何原因失敗，裝載層會記錄 dotnet 處理序損毀的重大例外狀況，但不會顯示任何錯誤頁面。

在 [IIS](/iis) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:fundamentals/servers/aspnet-core-module)會傳回 *502.5 處理序失敗*。 如需在裝載於 IIS 上時針對啟動問題進行疑難排解的資訊，請參閱 <xref:host-and-deploy/iis/troubleshoot>。 如需使用 Azure App Service 針對啟動問題進行疑難排解的資訊，請參閱 <xref:host-and-deploy/azure-apps/troubleshoot>。

## <a name="aspnet-core-mvc-error-handling"></a>ASP.NET Core MVC 錯誤處理

[MVC](xref:mvc/overview) 應用程式提供一些其他選項以處理錯誤，例如設定例外狀況篩選條件，以及執行模型驗證。

### <a name="exception-filters"></a>例外狀況篩選條件

在 MVC 應用程式中，您可以全域設定例外狀況篩選條件，或以每個控制器或每個動作基準來設定。 這些篩選會處理在控制器動作或其他篩選條件執行期間發生但的任何未處理例外狀況。 否則不呼叫這些篩選。 若要深入了解，請參閱[篩選](xref:mvc/controllers/filters)。

> [!TIP]
> 例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像錯誤處理中介軟體那麼有彈性。 一般建議使用中介軟體，只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選。

### <a name="handling-model-state-errors"></a>處理模型狀態錯誤

在叫用每個控制器動作之前，會先進行[模型驗證](xref:mvc/models/validation)，而動作方法必須負責檢查 `ModelState.IsValid` 並做出適當回應。

某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤；在這種情況下，就很適合在[篩選](xref:mvc/controllers/filters)中實作這類原則。 您應該測試您的動作在無效模型狀態中有何行為。 深入了解[測試控制器邏輯](xref:mvc/controllers/testing)。

## <a name="additional-resources"></a>其他資源

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
