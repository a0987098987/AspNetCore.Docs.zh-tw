---
title: 處理 ASP.NET Core 中的錯誤
author: tdykstra
description: 了解如何處理 ASP.NET Core 應用程式中的錯誤。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 03/01/2019
uid: fundamentals/error-handling
ms.openlocfilehash: a2ae2cb25c8cc5048b189b4035abbfc32a29aaff
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345486"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="80dad-103">處理 ASP.NET Core 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="80dad-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="80dad-104">作者：[Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex) 及 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="80dad-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="80dad-105">此文章說明處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="80dad-105">This article covers common approaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="80dad-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="80dad-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/samples/2.x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="80dad-107">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="80dad-107">Developer Exception Page</span></span>

<span data-ttu-id="80dad-108">若要設定應用程式以顯示提供例外狀況詳細資訊的頁面，請使用「開發人員例外狀況頁面」。</span><span class="sxs-lookup"><span data-stu-id="80dad-108">To configure an app to display a page that shows detailed information about exceptions, use the *Developer Exception Page*.</span></span> <span data-ttu-id="80dad-109">[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件 (於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)提供) 會提供該頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-109">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="80dad-110">將一行程式碼新增至 `Startup.Configure` 方法：</span><span class="sxs-lookup"><span data-stu-id="80dad-110">Add a line to the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=5)]

<span data-ttu-id="80dad-111">將對 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> 的呼叫放置於任何想要攔截例外狀況的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="80dad-111">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> in front of any middleware where you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="80dad-112">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-112">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="80dad-113">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="80dad-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="80dad-114">如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="80dad-114">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="80dad-115">若要查看開發人員例外狀況頁面，請將環境設定為 `Development` 並執行範例應用程式，然後將 `?throw=true` 新增至應用程式的基底 URL。</span><span class="sxs-lookup"><span data-stu-id="80dad-115">To see the Developer Exception Page, run the sample app with the environment set to `Development` and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="80dad-116">此頁面包含下列和例外狀況與要求有關的資訊：</span><span class="sxs-lookup"><span data-stu-id="80dad-116">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="80dad-117">堆疊追蹤</span><span class="sxs-lookup"><span data-stu-id="80dad-117">Stack trace</span></span>
* <span data-ttu-id="80dad-118">查詢字串參數 (如果有的話)</span><span class="sxs-lookup"><span data-stu-id="80dad-118">Query string parameters (if any)</span></span>
* <span data-ttu-id="80dad-119">Cookie (如果有的話)</span><span class="sxs-lookup"><span data-stu-id="80dad-119">Cookies (if any)</span></span>
* <span data-ttu-id="80dad-120">頁首</span><span class="sxs-lookup"><span data-stu-id="80dad-120">Headers</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="80dad-121">設定自訂的例外狀況處理頁面</span><span class="sxs-lookup"><span data-stu-id="80dad-121">Configure a custom exception handling page</span></span>

<span data-ttu-id="80dad-122">當應用程式不在開發環境中執行時，請呼叫 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 擴充方法來新增例外狀況處理中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dad-122">When the app isn't running in the Development environment, call the <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> extension method to add Exception Handling Middleware.</span></span> <span data-ttu-id="80dad-123">中介軟體：</span><span class="sxs-lookup"><span data-stu-id="80dad-123">The middleware:</span></span>

* <span data-ttu-id="80dad-124">可攔截例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-124">Catches exceptions.</span></span>
* <span data-ttu-id="80dad-125">可記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-125">Logs exceptions.</span></span>
* <span data-ttu-id="80dad-126">可在所指示頁面或控制器的替代管線中重新執行要求。</span><span class="sxs-lookup"><span data-stu-id="80dad-126">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="80dad-127">如果回應已啟動，就不會重新執行要求。</span><span class="sxs-lookup"><span data-stu-id="80dad-127">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="80dad-128">在範例應用程式的下列範例中，<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 會在非開發環境中加入例外狀況處理中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dad-128">In the following example from the sample app, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments.</span></span> <span data-ttu-id="80dad-129">擴充方法會在攔截並記錄例外狀況之後，針對重新執行的要求，於 `/Error` 端點指定一個錯誤頁面或控制器：</span><span class="sxs-lookup"><span data-stu-id="80dad-129">The extension method specifies an error page or controller at the `/Error` endpoint for re-executed requests after exceptions are caught and logged:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevExceptionPage&highlight=9)]

<span data-ttu-id="80dad-130">Razor Pages 應用程式範本會在 Pages 資料夾中，提供一個錯誤頁面 (*.cshtml*) 與 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 類別 (`ErrorModel`)。</span><span class="sxs-lookup"><span data-stu-id="80dad-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the Pages folder.</span></span>

<span data-ttu-id="80dad-131">在 MVC 應用程式中，MVC 應用程式範本中會包含下列錯誤處理常式方法，並出現在 Home 控制器中：</span><span class="sxs-lookup"><span data-stu-id="80dad-131">In an MVC app, the following error handler method is included in the MVC app template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="80dad-132">請勿使用 HTTP 方法屬性 (如 `HttpGet`) 裝飾錯誤處理常式動作方法。</span><span class="sxs-lookup"><span data-stu-id="80dad-132">Don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="80dad-133">明確的動詞命令可防止某些要求取得方法。</span><span class="sxs-lookup"><span data-stu-id="80dad-133">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="80dad-134">允許匿名存取方法，以便未經驗證的使用者能夠收到錯誤檢視。</span><span class="sxs-lookup"><span data-stu-id="80dad-134">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

## <a name="configure-status-code-pages"></a><span data-ttu-id="80dad-135">設定狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="80dad-135">Configure status code pages</span></span>

<span data-ttu-id="80dad-136">根據預設，ASP.NET Core 應用程式不會提供 HTTP 狀態碼 (例如「404 - 找不到」) 等狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-136">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="80dad-137">應用程式會傳回狀態碼和空白回應主體。</span><span class="sxs-lookup"><span data-stu-id="80dad-137">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="80dad-138">若要提供狀態碼頁面，請使用狀態碼頁面中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dad-138">To provide status code pages, use Status Code Pages Middleware.</span></span>

<span data-ttu-id="80dad-139">[Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) 套件 (於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)提供) 會提供中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dad-139">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is available in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="80dad-140">將一行程式碼新增至 `Startup.Configure` 方法：</span><span class="sxs-lookup"><span data-stu-id="80dad-140">Add a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="80dad-141">要求處理中介軟體之前，應該先呼叫 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 方法 (例如，靜態檔案中介軟體和 MVC 中介軟體)。</span><span class="sxs-lookup"><span data-stu-id="80dad-141">Call the <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> method before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="80dad-142">根據預設，狀態碼頁面中介軟體會針對常見狀態碼 (例如「404 - 找不到」) 新增純文字處理常式：</span><span class="sxs-lookup"><span data-stu-id="80dad-142">By default, Status Code Pages Middleware adds text-only handlers for common status codes, such as *404 - Not Found*:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="80dad-143">中介軟體可支援幾種擴充方法，可允許您自訂其行為。</span><span class="sxs-lookup"><span data-stu-id="80dad-143">The middleware supports several extension methods that allow you to customize its behavior.</span></span>

<span data-ttu-id="80dad-144">多載 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 會採用 lambda 運算式，可用來處理自訂錯誤處理邏輯，並手動撰寫回應：</span><span class="sxs-lookup"><span data-stu-id="80dad-144">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a lambda expression, which you can use to process custom error-handling logic and manually write the response:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="80dad-145">多載 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 會採用內容類型和格式字串，可用來自訂內容類型和回應文字：</span><span class="sxs-lookup"><span data-stu-id="80dad-145">An overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> takes a content type and format string, which you can use to customize the content type and response text:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

### <a name="redirect-and-re-execute-extension-methods"></a><span data-ttu-id="80dad-146">重新導向並重新執行擴充方法</span><span class="sxs-lookup"><span data-stu-id="80dad-146">Redirect and re-execute extension methods</span></span>

<span data-ttu-id="80dad-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>：</span><span class="sxs-lookup"><span data-stu-id="80dad-147"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*>:</span></span>

* <span data-ttu-id="80dad-148">傳送「302 - 已找到」狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="80dad-148">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="80dad-149">將用戶端重新導向到 URL 範本中提供的位置。</span><span class="sxs-lookup"><span data-stu-id="80dad-149">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="80dad-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> 通常是在下列應用程式相關情況下使用：</span><span class="sxs-lookup"><span data-stu-id="80dad-150"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> is commonly used when the app:</span></span>

* <span data-ttu-id="80dad-151">應用程式應將用戶端重新導向至不同的端點時 (通常是在由其他應用程式處理錯誤的情況下)。</span><span class="sxs-lookup"><span data-stu-id="80dad-151">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="80dad-152">針對 Web 應用程式，用戶端的瀏覽器網址列會反映重新導向後的端點。</span><span class="sxs-lookup"><span data-stu-id="80dad-152">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="80dad-153">應用程式不應該保留並傳回原始狀態碼與初始重新導向回應時。</span><span class="sxs-lookup"><span data-stu-id="80dad-153">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

<span data-ttu-id="80dad-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>：</span><span class="sxs-lookup"><span data-stu-id="80dad-154"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*>:</span></span>

* <span data-ttu-id="80dad-155">傳回原始狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="80dad-155">Returns the original status code to the client.</span></span>
* <span data-ttu-id="80dad-156">可透過使用替代路徑重新執行要求管線來產生回應本文。</span><span class="sxs-lookup"><span data-stu-id="80dad-156">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/Error/{0}");
```

<span data-ttu-id="80dad-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> 通常是在應用程式應執行以下作業時使用：</span><span class="sxs-lookup"><span data-stu-id="80dad-157"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> is commonly used when the app should:</span></span>

* <span data-ttu-id="80dad-158">在不重新導向至其他端點的情況下處理要求。</span><span class="sxs-lookup"><span data-stu-id="80dad-158">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="80dad-159">針對 Web 應用程式，用戶端的瀏覽器網址列會反映原始要求的端點。</span><span class="sxs-lookup"><span data-stu-id="80dad-159">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="80dad-160">保留並傳回原始狀態碼與回應時。</span><span class="sxs-lookup"><span data-stu-id="80dad-160">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="80dad-161">範本可能包含該狀態碼的預留位置 (`{0}`)。</span><span class="sxs-lookup"><span data-stu-id="80dad-161">Templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="80dad-162">範本的開頭必須是正斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="80dad-162">The template must start with a forward slash (`/`).</span></span> <span data-ttu-id="80dad-163">使用預留位置時，請確認端點 (頁面或控制器) 可以處理路徑區段。</span><span class="sxs-lookup"><span data-stu-id="80dad-163">When using a placeholder, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="80dad-164">例如適用於錯誤的 Razor Page 應接受具備 `@page` 指示詞的選擇性路徑區段值：</span><span class="sxs-lookup"><span data-stu-id="80dad-164">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="80dad-165">在 Razor 頁面處理常式方法或 MVC 控制器中，可以針對特定要求停用狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-165">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="80dad-166">若要停用狀態碼頁面，請嘗試從要求的 [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) 集合中擷取 <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>，然後停用此功能 (若可用的話)：</span><span class="sxs-lookup"><span data-stu-id="80dad-166">To disable status code pages, attempt to retrieve the <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature> from the request's [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="80dad-167">若要在應用程式中使用指向端點的 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 多載，請建立該端點的 MVC 檢視或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-167">To use a <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="80dad-168">例如，Razor Pages 應用程式範本會產生下列頁面和頁面模型類別：</span><span class="sxs-lookup"><span data-stu-id="80dad-168">For example, the Razor Pages app template produces the following page and page model class:</span></span>

<span data-ttu-id="80dad-169">*Error.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="80dad-169">*Error.cshtml*:</span></span>

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

<span data-ttu-id="80dad-170">*Error.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="80dad-170">*Error.cshtml.cs*:</span></span>

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

<span data-ttu-id="80dad-171">*Error.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="80dad-171">*Error.cshtml.cs*:</span></span>

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

## <a name="exception-handling-code"></a><span data-ttu-id="80dad-172">例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="80dad-172">Exception-handling code</span></span>

<span data-ttu-id="80dad-173">例外狀況處理頁面中的程式碼，可擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-173">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="80dad-174">一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。</span><span class="sxs-lookup"><span data-stu-id="80dad-174">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="80dad-175">另外也請注意，一旦傳送回應的標頭之後：</span><span class="sxs-lookup"><span data-stu-id="80dad-175">Also, be aware that once the headers for a response are sent:</span></span>

* <span data-ttu-id="80dad-176">應用程式就無法變更回應的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="80dad-176">The app can't change the response's status code.</span></span>
* <span data-ttu-id="80dad-177">無法執行任何例外狀況頁面或處理常式。</span><span class="sxs-lookup"><span data-stu-id="80dad-177">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="80dad-178">回應必須完成，否則會中止連線。</span><span class="sxs-lookup"><span data-stu-id="80dad-178">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="80dad-179">伺服器例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="80dad-179">Server exception handling</span></span>

<span data-ttu-id="80dad-180">除了應用程式中的例外狀況處理邏輯之外，[伺服器實作](xref:fundamentals/servers/index)也可以處理一些例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-180">In addition to the exception handling logic in your app, the [server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="80dad-181">如果伺服器在回應標頭傳送之前攔截到例外狀況，伺服器會傳送「500 - 內部伺服器錯誤」回應，且沒有回應本文。</span><span class="sxs-lookup"><span data-stu-id="80dad-181">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="80dad-182">如果伺服器在回應標頭傳送之後攔截到例外狀況，伺服器會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="80dad-182">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="80dad-183">應用程式未處理的要求會由伺服器來處理。</span><span class="sxs-lookup"><span data-stu-id="80dad-183">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="80dad-184">任何發生的例外狀況均由伺服器的例外狀況功能來處理。</span><span class="sxs-lookup"><span data-stu-id="80dad-184">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="80dad-185">任何已設定的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響這個行為。</span><span class="sxs-lookup"><span data-stu-id="80dad-185">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="80dad-186">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="80dad-186">Startup exception handling</span></span>

<span data-ttu-id="80dad-187">只有裝載層可以處理應用程式啟動期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-187">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="80dad-188">您可以使用 [Web 主機](xref:fundamentals/host/web-host)，將 `captureStartupErrors` 與 `detailedErrors` 索引鍵搭配使用來[設定主機對啟動期間所發生錯誤的回應行為](xref:fundamentals/host/web-host#detailed-errors)。</span><span class="sxs-lookup"><span data-stu-id="80dad-188">Using [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="80dad-189">如果錯誤是在主機位址/連接埠繫結之後發生，則裝載層只會顯示擷取到的啟動錯誤的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-189">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="80dad-190">如果任何繫結因為任何原因而失敗：</span><span class="sxs-lookup"><span data-stu-id="80dad-190">If any binding fails for any reason:</span></span>

* <span data-ttu-id="80dad-191">裝載層會記錄重大例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-191">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="80dad-192">Dotnet 會處理損毀狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-192">The dotnet process crashes.</span></span>
* <span data-ttu-id="80dad-193">當應用程式在 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器上執行時，不會顯示任何錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="80dad-193">No error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="80dad-194">在 [IIS](/iis) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:host-and-deploy/aspnet-core-module)會傳回 *502.5 - 處理序失敗*。</span><span class="sxs-lookup"><span data-stu-id="80dad-194">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="80dad-195">如需詳細資訊，請參閱<xref:host-and-deploy/iis/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="80dad-195">For more information, see <xref:host-and-deploy/iis/troubleshoot>.</span></span> <span data-ttu-id="80dad-196">如需使用 Azure App Service 針對啟動問題進行疑難排解的資訊，請參閱 <xref:host-and-deploy/azure-apps/troubleshoot>。</span><span class="sxs-lookup"><span data-stu-id="80dad-196">For information on troubleshooting startup issues with Azure App Service, see <xref:host-and-deploy/azure-apps/troubleshoot>.</span></span>

## <a name="aspnet-core-mvc-error-handling"></a><span data-ttu-id="80dad-197">ASP.NET Core MVC 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="80dad-197">ASP.NET Core MVC error handling</span></span>

<span data-ttu-id="80dad-198">[MVC](xref:mvc/overview) 應用程式提供一些其他選項以處理錯誤，例如設定例外狀況篩選條件，以及執行模型驗證。</span><span class="sxs-lookup"><span data-stu-id="80dad-198">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="80dad-199">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="80dad-199">Exception filters</span></span>

<span data-ttu-id="80dad-200">在 MVC 應用程式中，您可以全域設定例外狀況篩選條件，或以每個控制器或每個動作基準來設定。</span><span class="sxs-lookup"><span data-stu-id="80dad-200">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="80dad-201">這些篩選會處理在控制器動作或其他篩選條件執行期間發生但的任何未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="80dad-201">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="80dad-202">否則不呼叫這些篩選。</span><span class="sxs-lookup"><span data-stu-id="80dad-202">These filters aren't called otherwise.</span></span> <span data-ttu-id="80dad-203">若要深入了解，請參閱 <xref:mvc/controllers/filters>。</span><span class="sxs-lookup"><span data-stu-id="80dad-203">To learn more, see <xref:mvc/controllers/filters>.</span></span>

> [!TIP]
> <span data-ttu-id="80dad-204">例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="80dad-204">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="80dad-205">我們建議使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dad-205">We recommend the use of middleware.</span></span> <span data-ttu-id="80dad-206">請只在需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="80dad-206">Use filters only where you need to perform error handling *differently* based on which MVC action is chosen.</span></span>

### <a name="handle-model-state-errors"></a><span data-ttu-id="80dad-207">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="80dad-207">Handle model state errors</span></span>

<span data-ttu-id="80dad-208">在叫用每個控制器動作之前，會先進行[模型驗證](xref:mvc/models/validation)，而動作方法必須負責檢查 [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) 並適當地回應。</span><span class="sxs-lookup"><span data-stu-id="80dad-208">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) and react appropriately.</span></span>

<span data-ttu-id="80dad-209">某些應用程式會選擇遵循標準慣例來處理[模型驗證](xref:mvc/models/validation)錯誤；在這種情況下，就很適合在[篩選](xref:mvc/controllers/filters)中實作這類原則。</span><span class="sxs-lookup"><span data-stu-id="80dad-209">Some apps choose to follow a standard convention for dealing with [model validation](xref:mvc/models/validation) errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="80dad-210">您應該測試您的動作在無效模型狀態中有何行為。</span><span class="sxs-lookup"><span data-stu-id="80dad-210">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="80dad-211">如需詳細資訊，請參閱<xref:mvc/controllers/testing>。</span><span class="sxs-lookup"><span data-stu-id="80dad-211">For more information, see <xref:mvc/controllers/testing>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80dad-212">其他資源</span><span class="sxs-lookup"><span data-stu-id="80dad-212">Additional resources</span></span>

* <xref:host-and-deploy/azure-iis-errors-reference>
* <xref:host-and-deploy/iis/troubleshoot>
* <xref:host-and-deploy/azure-apps/troubleshoot>
