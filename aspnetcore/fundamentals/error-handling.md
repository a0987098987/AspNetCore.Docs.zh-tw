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
ms.openlocfilehash: 5443cbeb1ef95c579e5fc12b625babbfa27c7ec2
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="d7132-103">處理 ASP.NET Core 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="d7132-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="d7132-104">作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="d7132-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="d7132-105">本文涵蓋處理 ASP.NET Core 應用程式錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="d7132-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="d7132-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d7132-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="d7132-107">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="d7132-107">The developer exception page</span></span>

<span data-ttu-id="d7132-108">若要設定應用程式以顯示例外狀況的詳細資訊頁面，請安裝 `Microsoft.AspNetCore.Diagnostics` NuGet 套件，並於[在 Startup 類別中設定方法](startup.md)中新增一行文字：</span><span class="sxs-lookup"><span data-stu-id="d7132-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="d7132-109">將 `UseDeveloperExceptionPage` 放在任何您要攔截例外狀況到其中的中介軟體之前，例如 `app.UseMvc`。</span><span class="sxs-lookup"><span data-stu-id="d7132-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="d7132-110">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="d7132-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="d7132-111">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="d7132-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="d7132-112">[進一步了解環境的設定](environments.md)。</span><span class="sxs-lookup"><span data-stu-id="d7132-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="d7132-113">若要查看開發人員例外狀況頁面，請將環境設定為 `Development`，並將 `?throw=true` 新增至應用程式的基底 URL，以執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="d7132-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="d7132-114">此頁面包含數個索引標籤，內含例外狀況與要求的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d7132-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="d7132-115">第一個索引標籤包含堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="d7132-115">The first tab includes a stack trace.</span></span> 

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="d7132-117">下一個索引標籤則會顯示查詢字串參數 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="d7132-117">The next tab shows the query string parameters, if any.</span></span>

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="d7132-119">此要求沒有任何 Cookie；但如果有，它們會出現在 [Cookie] 索引標籤上。您可以看到傳遞至最後一個索引標籤的標頭。</span><span class="sxs-lookup"><span data-stu-id="d7132-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="d7132-121">設定自訂的例外狀況處理頁面</span><span class="sxs-lookup"><span data-stu-id="d7132-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="d7132-122">當應用程式不在 `Development` 環境中執行時，建議您設定使用例外處理常式頁面。</span><span class="sxs-lookup"><span data-stu-id="d7132-122">It's a good idea to configure an exception handler page to use when the app isn't running in the `Development` environment.</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="d7132-123">在 MVC 應用程式中，請不要使用 HTTP 方法屬性 (例如 `HttpGet`) 明確裝飾錯誤處理常式的動作方法。</span><span class="sxs-lookup"><span data-stu-id="d7132-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="d7132-124">使用明確的動詞時，可能會導致方法收不到某些要求。</span><span class="sxs-lookup"><span data-stu-id="d7132-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="d7132-125">設定狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="d7132-125">Configuring status code pages</span></span>

<span data-ttu-id="d7132-126">根據預設，應用程式不會提供 HTTP 狀態碼「404 (找不到)」等豐富的狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="d7132-126">By default, an app doesn't provide a rich status code page for HTTP status codes, such as *404 Not Found*.</span></span> <span data-ttu-id="d7132-127">若要提供狀態碼頁面，請將下一行新增至 `Startup.Configure` 方法，以設定狀態碼頁面中介軟體：</span><span class="sxs-lookup"><span data-stu-id="d7132-127">To provide status code pages, configure the Status Code Pages Middleware by adding a line to the `Startup.Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="d7132-128">根據預設，狀態碼頁面中介軟體會針對常見狀態碼 (例如 404) 新增簡單的純文字處理常式：</span><span class="sxs-lookup"><span data-stu-id="d7132-128">By default, Status Code Pages Middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 頁面](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="d7132-130">中介軟體可支援幾種擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d7132-130">The middleware supports several extension methods.</span></span> <span data-ttu-id="d7132-131">其中一種方法採用 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="d7132-131">One method takes a lambda expression:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="d7132-132">另一個方法則採用內容類型和格式字串：</span><span class="sxs-lookup"><span data-stu-id="d7132-132">Another method takes a content type and format string:</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="d7132-133">也有重新導向和重新執行的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d7132-133">There are also redirect and re-execute extension methods.</span></span> <span data-ttu-id="d7132-134">重新導向方法會將 302 狀態碼傳送給用戶端：</span><span class="sxs-lookup"><span data-stu-id="d7132-134">The redirect method sends a 302 status code to the client:</span></span>

[!code-csharp[](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="d7132-135">重新執行方法則傳會將原始的狀態碼傳回給用戶端，同時也會針對重新導向 URL 執行處理常式：</span><span class="sxs-lookup"><span data-stu-id="d7132-135">The re-execute method returns the original status code to the client but also executes the handler for the redirect URL:</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="d7132-136">在 Razor 頁面處理常式方法或 MVC 控制器中，可以針對特定要求停用狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="d7132-136">Status code pages can be disabled for specific requests in a Razor Pages handler method or in an MVC controller.</span></span> <span data-ttu-id="d7132-137">若要停用狀態碼頁面，請嘗試從要求的 [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) 集合中擷取 [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature)，然後停用此功能 (若可用的話)：</span><span class="sxs-lookup"><span data-stu-id="d7132-137">To disable status code pages, attempt to retrieve the [IStatusCodePagesFeature](/dotnet/api/microsoft.aspnetcore.diagnostics.istatuscodepagesfeature) from the request's [HttpContext.Features](/dotnet/api/microsoft.aspnetcore.http.httpcontext.features) collection and disable the feature if it's available:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="d7132-138">例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="d7132-138">Exception-handling code</span></span>

<span data-ttu-id="d7132-139">例外狀況處理頁面中的程式碼，可擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d7132-139">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="d7132-140">一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。</span><span class="sxs-lookup"><span data-stu-id="d7132-140">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="d7132-141">另請注意，一旦傳送回應標頭之後，您就無法變更回應的狀態碼，也不能執行任何例外狀況頁面或處理常式。</span><span class="sxs-lookup"><span data-stu-id="d7132-141">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="d7132-142">回應必須完成，否則會中止連線。</span><span class="sxs-lookup"><span data-stu-id="d7132-142">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="d7132-143">伺服器例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="d7132-143">Server exception handling</span></span>

<span data-ttu-id="d7132-144">除了應用程式中的例外狀況處理邏輯，裝載應用程式的[伺服器](servers/index.md)也會執行一些例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="d7132-144">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="d7132-145">如果伺服器在標頭傳送之前攔截到例外狀況，則伺服器會傳送「500 內部伺服器錯誤」回應，且沒有本文。</span><span class="sxs-lookup"><span data-stu-id="d7132-145">If the server catches an exception before the headers are sent, the server sends a *500 Internal Server Error* response with no body.</span></span> <span data-ttu-id="d7132-146">如果伺服器在標頭傳送之後攔截到例外狀況，則伺服器會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="d7132-146">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="d7132-147">應用程式未處理的要求會由伺服器來處理。</span><span class="sxs-lookup"><span data-stu-id="d7132-147">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="d7132-148">任何發生的例外狀況均由伺服器的例外狀況功能來處理。</span><span class="sxs-lookup"><span data-stu-id="d7132-148">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="d7132-149">任何已設定的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響這個行為。</span><span class="sxs-lookup"><span data-stu-id="d7132-149">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="d7132-150">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="d7132-150">Startup exception handling</span></span>

<span data-ttu-id="d7132-151">只有裝載層可以處理應用程式啟動期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d7132-151">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="d7132-152">您可以使用 `captureStartupErrors` 和 `detailedErrors` 索引鍵，[設定主機對啟動期間所發生錯誤的回應行為](hosting.md#detailed-errors)。</span><span class="sxs-lookup"><span data-stu-id="d7132-152">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="d7132-153">如果錯誤是在主機位址/連接埠繫結之後發生，則裝載層只會顯示擷取到的啟動錯誤的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="d7132-153">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="d7132-154">在 [Kestrel](xref:fundamentals/servers/kestrel) 伺服器上執行應用程式時，如果繫結因為任何原因失敗，裝載層會記錄 dotnet 處理序損毀的重大例外狀況，但不會顯示任何錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="d7132-154">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed when the app is running on the [Kestrel](xref:fundamentals/servers/kestrel) server.</span></span>

<span data-ttu-id="d7132-155">在 [IIS](/iis) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:fundamentals/servers/aspnet-core-module)會傳回 *502.5 處理序失敗*。</span><span class="sxs-lookup"><span data-stu-id="d7132-155">When running on [IIS](/iis) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 Process Failure* is returned by the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) if the process can't be started.</span></span> <span data-ttu-id="d7132-156">請遵循[針對 IIS 上的 ASP.NET Core 進行疑難排解](xref:host-and-deploy/iis/troubleshoot)主題中的疑難排解建議。</span><span class="sxs-lookup"><span data-stu-id="d7132-156">Follow the troubleshooting advice in the [Troubleshoot ASP.NET Core on IIS](xref:host-and-deploy/iis/troubleshoot) topic.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="d7132-157">ASP.NET MVC 錯誤處理</span><span class="sxs-lookup"><span data-stu-id="d7132-157">ASP.NET MVC error handling</span></span>

<span data-ttu-id="d7132-158">[MVC](xref:mvc/overview) 應用程式提供一些其他選項以處理錯誤，例如設定例外狀況篩選條件，以及執行模型驗證。</span><span class="sxs-lookup"><span data-stu-id="d7132-158">[MVC](xref:mvc/overview) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="d7132-159">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="d7132-159">Exception Filters</span></span>

<span data-ttu-id="d7132-160">在 MVC 應用程式中，您可以全域設定例外狀況篩選條件，或以每個控制器或每個動作基準來設定。</span><span class="sxs-lookup"><span data-stu-id="d7132-160">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="d7132-161">這些篩選條件會處理任何在控制器動作或其他篩選條件執行期間發生但未處理的例外狀況；否則的話，就不會呼叫這些篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d7132-161">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="d7132-162">如需深入了解例外狀況篩選條件，請參閱[篩選條件](../mvc/controllers/filters.md)。</span><span class="sxs-lookup"><span data-stu-id="d7132-162">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="d7132-163">例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像錯誤處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="d7132-163">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="d7132-164">一般情況下通常使用中介軟體；只有當您需要根據選擇的 MVC 動作執行「不同的」錯誤處理時，才使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="d7132-164">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="d7132-165">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="d7132-165">Handling Model State Errors</span></span>

<span data-ttu-id="d7132-166">在叫用每個控制器動作之前，會先進行[模型驗證](../mvc/models/validation.md)，而動作方法必須負責檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="d7132-166">[Model validation](../mvc/models/validation.md) occurs prior to invoking each controller action, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="d7132-167">某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤；在這種情況下，就很適合在[篩選條件](../mvc/controllers/filters.md)中實作這類原則。</span><span class="sxs-lookup"><span data-stu-id="d7132-167">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="d7132-168">您應該測試您的動作在無效模型狀態中有何行為。</span><span class="sxs-lookup"><span data-stu-id="d7132-168">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="d7132-169">深入了解[測試控制器邏輯](../mvc/controllers/testing.md)。</span><span class="sxs-lookup"><span data-stu-id="d7132-169">Learn more in [Test controller logic](../mvc/controllers/testing.md).</span></span>



