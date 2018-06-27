---
title: 處理 ASP.NET Core 中的錯誤
author: ardalis
description: 了解如何在 ASP.NET Core 應用程式中處理錯誤。
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 11/30/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/error-handling
ms.openlocfilehash: 86041cf58dd88bea153eefed63a1985b6ddcacd8
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/04/2018
ms.locfileid: "34566707"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="60609-103">處理 ASP.NET Core 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="60609-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="60609-104">作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="60609-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="60609-105">本文涵蓋處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="60609-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="60609-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="60609-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="60609-107">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="60609-107">The developer exception page</span></span>

<span data-ttu-id="60609-108">若要設定應用程式以顯示例外狀況的詳細資訊頁面，請安裝 `Microsoft.AspNetCore.Diagnostics` NuGet 套件，並於[在 Startup 類別中設定方法](xref:fundamentals/startup)中新增一行文字：</span><span class="sxs-lookup"><span data-stu-id="60609-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](xref:fundamentals/startup):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="60609-109">將 `UseDeveloperExceptionPage` 放在任何您要攔截例外狀況到其中的中介軟體之前，例如 `app.UseMvc`。</span><span class="sxs-lookup"><span data-stu-id="60609-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="60609-110">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="60609-111">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="60609-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="60609-112">[進一步了解環境的設定](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="60609-112">[Learn more about configuring environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="60609-113">若要查看開發人員例外狀況頁面，請將環境設定為 `Development`，並將 `?throw=true` 新增至應用程式的基底 URL，以執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="60609-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="60609-114">此頁面包含數個索引標籤，內含例外狀況與要求的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="60609-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="60609-115">第一個索引標籤包含堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="60609-115">The first tab includes a stack trace.</span></span> 

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="60609-117">下一個索引標籤則會顯示查詢字串參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="60609-117">The next tab shows the query string parameters, if any.</span></span>

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="60609-119">此要求沒有任何 Cookie；但如果有，它們會出現在 [Cookie] 索引標籤上。您可以看到傳遞至最後一個索引標籤的標頭。</span><span class="sxs-lookup"><span data-stu-id="60609-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="60609-121">設定自訂的例外狀況處理頁面</span><span class="sxs-lookup"><span data-stu-id="60609-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="60609-122">當應用程式不在 `Development` 環境中執行時，請設定使用例外處理常式頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-122">Configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="60609-123">在 Razor Pages 應用程式中，[dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages 範本會在 [頁面] 資料夾中提供 [錯誤] 頁面和 `ErrorModel` 頁面模型類別。</span><span class="sxs-lookup"><span data-stu-id="60609-123">In a Razor Pages app, the [dotnet new](/dotnet/core/tools/dotnet-new) Razor Pages template provides an Error page and `ErrorModel` page model class in the *Pages* folder.</span></span>

<span data-ttu-id="60609-124">在 MVC 應用程式中，請勿使用 HTTP 方法屬性 (如 `HttpGet`) 裝飾錯誤處理常式動作方法。</span><span class="sxs-lookup"><span data-stu-id="60609-124">In an MVC app, don't decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="60609-125">明確的動詞命令可防止某些要求取得方法。</span><span class="sxs-lookup"><span data-stu-id="60609-125">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="60609-126">允許匿名存取方法，以便未經驗證的使用者能夠收到錯誤檢視。</span><span class="sxs-lookup"><span data-stu-id="60609-126">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

<span data-ttu-id="60609-127">例如，[dotnet new](/dotnet/core/tools/dotnet-new) MVC 範本提供的下列錯誤處理常式方法會出現在主控制器中：</span><span class="sxs-lookup"><span data-stu-id="60609-127">For example, the following error handler method is provided by the [dotnet new](/dotnet/core/tools/dotnet-new) MVC template and appears in the Home controller:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="60609-128">設定狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="60609-128">Configuring status code pages</span></span>

<span data-ttu-id="60609-129">根據預設，應用程式不會提供 HTTP 狀態碼「404 (找不到)」等豐富的狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-129">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="60609-130">若要提供狀態碼頁面，請將下一行新增至 `Startup.Configure` 方法，以設定狀態碼頁面中介軟體：</span><span class="sxs-lookup"><span data-stu-id="60609-130">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="60609-131">根據預設，狀態碼頁面中介軟體會針對常見狀態碼 (例如 404) 新增簡單的純文字處理常式：</span><span class="sxs-lookup"><span data-stu-id="60609-131">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 頁面](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="60609-133">中介軟體可支援幾種擴充方法。</span><span class="sxs-lookup"><span data-stu-id="60609-133">The middleware supports several extension methods.</span></span> <span data-ttu-id="60609-134">其中一種方法採用 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="60609-134">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="60609-135">另一個方法則採用內容類型和格式字串：</span><span class="sxs-lookup"><span data-stu-id="60609-135">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="60609-136">也有重新導向和重新執行的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="60609-136">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="60609-137">重新導向方法會將 302 狀態碼傳送給用戶端：</span><span class="sxs-lookup"><span data-stu-id="60609-137">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="60609-138">重新執行方法則傳會將原始的狀態碼傳回給用戶端，同時也會針對重新導向 URL 執行處理常式：</span><span class="sxs-lookup"><span data-stu-id="60609-138">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="60609-139">在 Razor 頁面處理常式方法或 MVC 控制器中，可以針對特定要求停用狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-139">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="60609-140">若要停用狀態碼頁面，請嘗試從要求的 [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) 集合中擷取 [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature)，然後停用此功能 (若可用的話)：</span><span class="sxs-lookup"><span data-stu-id="60609-140">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

<span data-ttu-id="60609-141">如果在應用程式中使用指向端點的 `UseStatusCodePages*` 多載，請建立該端點的 MVC 檢視或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-141">If using a `UseStatusCodePages*` overload that points to an endpoint within the app, create an MVC view or Razor Page for the endpoint.</span></span> <span data-ttu-id="60609-142">例如，Razor Pages 應用程式的 [dotnet new](/dotnet/core/tools/dotnet-new) 範本會產生下列頁面和頁面模型類別：</span><span class="sxs-lookup"><span data-stu-id="60609-142">For example, the [dotnet new](/dotnet/core/tools/dotnet-new) template for a Razor Pages app produces the following page and page model class:</span></span>

<span data-ttu-id="60609-143">*Error.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="60609-143">*Error.cshtml*:</span></span>

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
    Swapping to <strong>Development</strong> environment will display more detailed information about the error that occurred.
</p>
<p>
    <strong>Development environment should not be enabled in deployed applications</strong>, as it can result in sensitive information from exceptions being displayed to end users. For local debugging, development environment can be enabled by setting the <strong>ASPNETCORE_ENVIRONMENT</strong> environment variable to <strong>Development</strong>, and restarting the application.
</p>
```

<span data-ttu-id="60609-144">*Error.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="60609-144">*Error.cshtml.cs*:</span></span>

```csharp
public class ErrorModel : PageModel
{
    public string RequestId { get; set; }

    public bool ShowRequestId => !string.IsNullOrEmpty(RequestId);

    [ResponseCache(Duration = 0, Location = ResponseCacheLocation.None, NoStore = true)]
    public void OnGet()
    {
        RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier;
    }
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="60609-145">例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="60609-145">Exception-handling code</span></span>

<span data-ttu-id="60609-146">例外狀況處理頁面中的程式碼，可擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="60609-146">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="60609-147">一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。</span><span class="sxs-lookup"><span data-stu-id="60609-147">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="60609-148">另請注意，一旦傳送回應標頭之後，您就無法變更回應的狀態碼，也不能執行任何例外狀況頁面或處理常式。</span><span class="sxs-lookup"><span data-stu-id="60609-148">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="60609-149">回應必須完成，否則會中止連線。</span><span class="sxs-lookup"><span data-stu-id="60609-149">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="60609-150">伺服器例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="60609-150">Server exception handling</span></span>

<span data-ttu-id="60609-151">除了應用程式中的例外狀況處理邏輯，裝載應用程式的[伺服器](xref:fundamentals/servers/index)也會執行一些例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="60609-151">In addition to the exception handling logic in your app, the [server](xref:fundamentals/servers/index) hosting your app performs some exception handling.</span></span> <span data-ttu-id="60609-152">如果伺服器在標頭傳送之前攔截到例外狀況，則伺服器會傳送「500 內部伺服器錯誤」回應，且沒有本文。</span><span class="sxs-lookup"><span data-stu-id="60609-152">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="60609-153">如果伺服器在標頭傳送之後攔截到例外狀況，則伺服器會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="60609-153">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="60609-154">應用程式未處理的要求會由伺服器來處理。</span><span class="sxs-lookup"><span data-stu-id="60609-154">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="60609-155">任何發生的例外狀況均由伺服器的例外狀況功能來處理。</span><span class="sxs-lookup"><span data-stu-id="60609-155">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="60609-156">任何已設定的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響這個行為。</span><span class="sxs-lookup"><span data-stu-id="60609-156">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="60609-157">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="60609-157">Startup exception handling</span></span>

<span data-ttu-id="60609-158">只有裝載層可以處理應用程式啟動期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="60609-158">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="60609-159">使用 [Web 主機](xref:fundamentals/host/web-host)，您可以將 `captureStartupErrors` 與 `detailedErrors` 索引鍵搭配使用來[設定主機對啟動期間所發生錯誤的回應行為](xref:fundamentals/host/web-host#detailed-errors)。</span><span class="sxs-lookup"><span data-stu-id="60609-159">Using the [Web Host](xref:fundamentals/host/web-host), you can [configure how the host behaves in response to errors during startup](xref:fundamentals/host/web-host#detailed-errors) with the `captureStartupErrors` and `detailedErrors` keys.</span></span>

<span data-ttu-id="60609-160">如果錯誤是在主機位址/連接埠繫結之後發生，則裝載層只會顯示擷取到的啟動錯誤的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-160">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="60609-161">在 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器上執行應用程式時，如果繫結因為任何原因失敗，裝載層會記錄 dotnet 處理序損毀的重大例外狀況，但不會顯示任何錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="60609-161">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="60609-162">在 [IIS](/iis) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:fundamentals/servers/aspnet-core-module)會傳回 *502.5 處理序失敗*。</span><span class="sxs-lookup"><span data-stu-id="60609-162">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="60609-163">請遵循[針對 IIS 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/iis/troubleshoot)主題中的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="60609-163">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="60609-164">ASP.NET MVC 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="60609-164">ASP.NET MVC error handling</span></span>

<span data-ttu-id="60609-165">[MVC](xref:mvc/overview) 應用程式提供一些其他選項以處理錯誤，例如設定例外狀況篩選條件，以及執行模型驗證。</span><span class="sxs-lookup"><span data-stu-id="60609-165">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="60609-166">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="60609-166">Exception Filters</span></span>

<span data-ttu-id="60609-167">在 MVC 應用程式中，您可以全域設定例外狀況篩選條件，或以每個控制器或每個動作基準來設定。</span><span class="sxs-lookup"><span data-stu-id="60609-167">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="60609-168">這些篩選條件會處理任何在控制器動作或其他篩選條件執行期間發生但未處理的例外狀況；否則的話，就不會呼叫這些篩選條件。</span><span class="sxs-lookup"><span data-stu-id="60609-168">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="60609-169">如需深入了解例外狀況篩選條件，請參閱[篩選條件](xref:mvc/controllers/filters)。</span><span class="sxs-lookup"><span data-stu-id="60609-169">Learn more about exception filters in [Filters](xref:mvc/controllers/filters).</span></span>

> [!TIP]
> <span data-ttu-id="60609-170">例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="60609-170">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="60609-171">一般情況下通常使用中介軟體；只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="60609-171">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="60609-172">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="60609-172">Handling Model State Errors</span></span>

<span data-ttu-id="60609-173">在叫用每個控制器動作之前，會先進行[模型驗證](xref:mvc/models/validation)，而動作方法必須負責檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="60609-173">[Model validation](xref:mvc/models/validation) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="60609-174">某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤；在這種情況下，就很適合在[篩選條件](xref:mvc/controllers/filters)中實作這類原則。</span><span class="sxs-lookup"><span data-stu-id="60609-174">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](xref:mvc/controllers/filters) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="60609-175">您應該測試您的動作在無效模型狀態中有何行為。</span><span class="sxs-lookup"><span data-stu-id="60609-175">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="60609-176">深入了解[測試控制器邏輯](xref:mvc/controllers/testing)。</span><span class="sxs-lookup"><span data-stu-id="60609-176">Learn more in [Test controller logic](xref:mvc/controllers/testing).</span></span>
