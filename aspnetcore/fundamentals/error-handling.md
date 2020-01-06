---
title: 處理 ASP.NET Core 中的錯誤
author: rick-anderson
description: 了解如何處理 ASP.NET Core 應用程式中的錯誤。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: fundamentals/error-handling
ms.openlocfilehash: c20d8757eef80fdbb73b1b7a9933a3c0be9bb8ed
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358973"
---
# <a name="handle-errors-in-aspnet-core"></a><span data-ttu-id="b0ce6-103">處理 ASP.NET Core 中的錯誤</span><span class="sxs-lookup"><span data-stu-id="b0ce6-103">Handle errors in ASP.NET Core</span></span>

<span data-ttu-id="b0ce6-104">作者：[Tom Dykstra](https://github.com/tdykstra/)、[Luke Latham](https://github.com/guardrex) 及 [Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b0ce6-104">By [Tom Dykstra](https://github.com/tdykstra/), [Luke Latham](https://github.com/guardrex), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b0ce6-105">本文涵蓋在 ASP.NET Core web 應用程式中處理錯誤的常見方法。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-105">This article covers common approaches to handling errors in ASP.NET Core web apps.</span></span> <span data-ttu-id="b0ce6-106">請參閱 web Api 的 <xref:web-api/handle-errors>。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-106">See <xref:web-api/handle-errors> for web APIs.</span></span>

<span data-ttu-id="b0ce6-107">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span> <span data-ttu-id="b0ce6-108">（[如何下載](xref:index#how-to-download-a-sample)）。本文包含如何在範例應用程式中設定預處理器指示詞（`#if`、`#endif`、`#define`）以啟用不同案例的指示。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-108">([How to download](xref:index#how-to-download-a-sample).) The article includes instructions about how to set preprocessor directives (`#if`, `#endif`, `#define`) in the sample app to enable different scenarios.</span></span>

## <a name="developer-exception-page"></a><span data-ttu-id="b0ce6-109">開發人員例外狀況頁面</span><span class="sxs-lookup"><span data-stu-id="b0ce6-109">Developer Exception Page</span></span>

<span data-ttu-id="b0ce6-110">「開發人員例外狀況頁面」會顯示要求例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-110">The *Developer Exception Page* displays detailed information about request exceptions.</span></span> <span data-ttu-id="b0ce6-111">該頁面是由 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) \(英文\) 套件所提供，其位於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-111">The page is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="b0ce6-112">當應用程式在開發[環境](xref:fundamentals/environments)中執行時，新增程式碼到 `Startup.Configure` 方法以啟用頁面：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-112">Add code to the `Startup.Configure` method to enable the page when the app is running in the Development [environment](xref:fundamentals/environments):</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=1-4)]

<span data-ttu-id="b0ce6-113">將對 <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> 的呼叫放置於任何您想要攔截例外狀況的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-113">Place the call to <xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*> before any middleware that you want to catch exceptions.</span></span>

> [!WARNING]
> <span data-ttu-id="b0ce6-114">**僅有當應用程式是在開發環境中執行時**，才啟用開發人員例外狀況頁面。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-114">Enable the Developer Exception Page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="b0ce6-115">當應用程式在生產環境中執行時，您不會想要公開共用例外狀況的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-115">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="b0ce6-116">如需設定環境的詳細資訊，請參閱<xref:fundamentals/environments>。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-116">For more information on configuring environments, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="b0ce6-117">此頁面包含下列和例外狀況與要求有關的資訊：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-117">The page includes the following information about the exception and the request:</span></span>

* <span data-ttu-id="b0ce6-118">堆疊追蹤</span><span class="sxs-lookup"><span data-stu-id="b0ce6-118">Stack trace</span></span>
* <span data-ttu-id="b0ce6-119">查詢字串參數 (如果有的話)</span><span class="sxs-lookup"><span data-stu-id="b0ce6-119">Query string parameters (if any)</span></span>
* <span data-ttu-id="b0ce6-120">Cookie (如果有的話)</span><span class="sxs-lookup"><span data-stu-id="b0ce6-120">Cookies (if any)</span></span>
* <span data-ttu-id="b0ce6-121">標頭</span><span class="sxs-lookup"><span data-stu-id="b0ce6-121">Headers</span></span>

<span data-ttu-id="b0ce6-122">若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看開發人員例外狀況頁面，請使用 `DevEnvironment` 前置處理器指示詞，並選取首頁上的 [觸發例外狀況]。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-122">To see the Developer Exception Page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `DevEnvironment` preprocessor directive and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-page"></a><span data-ttu-id="b0ce6-123">例外處理常式頁面</span><span class="sxs-lookup"><span data-stu-id="b0ce6-123">Exception handler page</span></span>

<span data-ttu-id="b0ce6-124">若要針對生產環境設定自訂錯誤處理頁面，請使用例外狀況處理中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-124">To configure a custom error handling page for the Production environment, use the Exception Handling Middleware.</span></span> <span data-ttu-id="b0ce6-125">中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-125">The middleware:</span></span>

* <span data-ttu-id="b0ce6-126">攔截並記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-126">Catches and logs exceptions.</span></span>
* <span data-ttu-id="b0ce6-127">可在所指示頁面或控制器的替代管線中重新執行要求。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-127">Re-executes the request in an alternate pipeline for the page or controller indicated.</span></span> <span data-ttu-id="b0ce6-128">如果回應已啟動，就不會重新執行要求。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-128">The request isn't re-executed if the response has started.</span></span>

<span data-ttu-id="b0ce6-129">在下列範例中，<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 會在非開發環境中加入例外狀況處理中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-129">In the following example, <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> adds the Exception Handling Middleware in non-Development environments:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_DevPageAndHandlerPage&highlight=5-9)]

<span data-ttu-id="b0ce6-130">Razor Pages 應用程式範本會在 *Pages* 資料夾中提供錯誤頁面 ( *.cshtml*) 與 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 類別 (`ErrorModel`)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-130">The Razor Pages app template provides an Error page (*.cshtml*) and <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> class (`ErrorModel`) in the *Pages* folder.</span></span> <span data-ttu-id="b0ce6-131">針對 MVC 應用程式，專案範本會包含 Error 動作方法和 Error 檢視。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-131">For an MVC app, the project template includes an Error action method and an Error view.</span></span> <span data-ttu-id="b0ce6-132">以下為動作方法：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-132">Here's the action method:</span></span>

```csharp
[AllowAnonymous]
public IActionResult Error()
{
    return View(new ErrorViewModel 
        { RequestId = Activity.Current?.Id ?? HttpContext.TraceIdentifier });
}
```

<span data-ttu-id="b0ce6-133">請勿使用 HTTP 方法屬性（例如 `HttpGet`）來標記錯誤處理常式動作方法。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-133">Don't mark the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="b0ce6-134">明確的動詞命令可防止某些要求取得方法。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-134">Explicit verbs prevent some requests from reaching the method.</span></span> <span data-ttu-id="b0ce6-135">允許匿名存取方法，以便未經驗證的使用者能夠收到錯誤檢視。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-135">Allow anonymous access to the method so that unauthenticated users are able to receive the error view.</span></span>

### <a name="access-the-exception"></a><span data-ttu-id="b0ce6-136">存取例外狀況</span><span class="sxs-lookup"><span data-stu-id="b0ce6-136">Access the exception</span></span>

<span data-ttu-id="b0ce6-137">使用 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 來存取例外狀況或和錯誤處理常式控制器或頁面中的原始要求路徑：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-137">Use <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to access the exception and the original request path in an error handler controller or page:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/Error.cshtml.cs?name=snippet_ExceptionHandlerPathFeature&3,7)]

> [!WARNING]
> <span data-ttu-id="b0ce6-138">請**勿**提供敏感性的錯誤資訊給用戶端。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-138">Do **not** serve sensitive error information to clients.</span></span> <span data-ttu-id="b0ce6-139">提供錯誤有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-139">Serving errors is a security risk.</span></span>

<span data-ttu-id="b0ce6-140">若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看例外狀況處理頁面，請使用 `ProdEnvironment` 和 `ErrorHandlerPage` 前置處理器指示詞，並選取首頁上的 [觸發例外狀況]。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-140">To see the exception handling page in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerPage` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="exception-handler-lambda"></a><span data-ttu-id="b0ce6-141">例外處理常式 Lambda</span><span class="sxs-lookup"><span data-stu-id="b0ce6-141">Exception handler lambda</span></span>

<span data-ttu-id="b0ce6-142">[自訂例外處理常式頁面](#exception-handler-page)的替代方法，便是將 Lambda 提供給 <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-142">An alternative to a [custom exception handler page](#exception-handler-page) is to provide a lambda to <xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>.</span></span> <span data-ttu-id="b0ce6-143">使用 lambda 可讓您在傳回回應之前存取錯誤。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-143">Using a lambda allows access to the error before returning the response.</span></span>

<span data-ttu-id="b0ce6-144">以下是將 Lambda 用於例外狀況處理的範例：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-144">Here's an example of using a lambda for exception handling:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_HandlerPageLambda)]

<span data-ttu-id="b0ce6-145">在上述程式碼中，會新增 `await context.Response.WriteAsync(new string(' ', 512));`，因此 Internet Explorer 瀏覽器會顯示錯誤訊息，而不是 IE 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-145">In the preceding code, `await context.Response.WriteAsync(new string(' ', 512));` is added so the Internet Explorer browser displays the error message rather than an IE error message.</span></span> <span data-ttu-id="b0ce6-146">如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/16144) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-146">For more information, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/16144).</span></span>

> [!WARNING]
> <span data-ttu-id="b0ce6-147">請**勿**從 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> 或 <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> 提供錯誤資訊給用戶端。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-147">Do **not** serve sensitive error information from <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerFeature> or <xref:Microsoft.AspNetCore.Diagnostics.IExceptionHandlerPathFeature> to clients.</span></span> <span data-ttu-id="b0ce6-148">提供錯誤有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-148">Serving errors is a security risk.</span></span>

<span data-ttu-id="b0ce6-149">若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看例外狀況處理 Lambda 的結果，請使用 `ProdEnvironment` 和 `ErrorHandlerLambda` 前置處理器指示詞，並選取首頁上的 [觸發例外狀況]。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-149">To see the result of the exception handling lambda in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use the `ProdEnvironment` and `ErrorHandlerLambda` preprocessor directives, and select **Trigger an exception** on the home page.</span></span>

## <a name="usestatuscodepages"></a><span data-ttu-id="b0ce6-150">UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="b0ce6-150">UseStatusCodePages</span></span>

<span data-ttu-id="b0ce6-151">根據預設，ASP.NET Core 應用程式不會提供 HTTP 狀態碼 (例如「404 - 找不到」) 等狀態碼頁面。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-151">By default, an ASP.NET Core app doesn't provide a status code page for HTTP status codes, such as *404 - Not Found*.</span></span> <span data-ttu-id="b0ce6-152">應用程式會傳回狀態碼和空白回應主體。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-152">The app returns a status code and an empty response body.</span></span> <span data-ttu-id="b0ce6-153">若要提供狀態碼頁面，請使用狀態碼頁面中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-153">To provide status code pages, use Status Code Pages middleware.</span></span>

<span data-ttu-id="b0ce6-154">該中介軟體是由 [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) \(英文\) 套件所提供，其位於 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-154">The middleware is made available by the [Microsoft.AspNetCore.Diagnostics](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics/) package, which is in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="b0ce6-155">若要針對常見的錯誤狀態碼啟用預設的純文字處理常式，請呼叫 `Startup.Configure` 方法中的 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*>：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-155">To enable default text-only handlers for common error status codes, call <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> in the `Startup.Configure` method:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePages)]

<span data-ttu-id="b0ce6-156">要求處理中介軟體 (例如靜態檔案中介軟體和 MVC 中介軟體) 之前，應該先呼叫 `UseStatusCodePages`。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-156">Call `UseStatusCodePages` before request handling middleware (for example, Static File Middleware and MVC Middleware).</span></span>

<span data-ttu-id="b0ce6-157">以下是預設處理常式所有顯示文字的範例：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-157">Here's an example of text displayed by the default handlers:</span></span>

```
Status Code: 404; Not Found
```

<span data-ttu-id="b0ce6-158">若要在[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples) \(英文\) 中查看其中一種狀態碼頁面格式，請使用其中一個以 `StatusCodePages` 為前置處理器指示詞，並選取首頁上的 [觸發 404]。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-158">To see one of the various status code page formats in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples), use one of the preprocessor directives that begin with `StatusCodePages`, and select **Trigger a 404** on the home page.</span></span>

## <a name="usestatuscodepages-with-format-string"></a><span data-ttu-id="b0ce6-159">具格式字串的 UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="b0ce6-159">UseStatusCodePages with format string</span></span>

<span data-ttu-id="b0ce6-160">若要自訂回應內容類型和文字，請使用 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 的多載，其會採用內容類型和格式字串：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-160">To customize the response content type and text, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a content type and format string:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesFormatString)]

## <a name="usestatuscodepages-with-lambda"></a><span data-ttu-id="b0ce6-161">具 Lambda 的 UseStatusCodePages</span><span class="sxs-lookup"><span data-stu-id="b0ce6-161">UseStatusCodePages with lambda</span></span>

<span data-ttu-id="b0ce6-162">若要指定自訂錯誤處理和回應撰寫程式碼，請使用 <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> 的多載，其會採用 Lambda 運算式：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-162">To specify custom error-handling and response-writing code, use the overload of <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePages*> that takes a lambda expression:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesLambda)]

## <a name="usestatuscodepageswithredirects"></a><span data-ttu-id="b0ce6-163">UseStatusCodePagesWithRedirects</span><span class="sxs-lookup"><span data-stu-id="b0ce6-163">UseStatusCodePagesWithRedirects</span></span>

<span data-ttu-id="b0ce6-164"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-164">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithRedirects*> extension method:</span></span>

* <span data-ttu-id="b0ce6-165">傳送「302 - 已找到」狀態碼傳送給用戶端。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-165">Sends a *302 - Found* status code to the client.</span></span>
* <span data-ttu-id="b0ce6-166">將用戶端重新導向到 URL 範本中提供的位置。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-166">Redirects the client to the location provided in the URL template.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

<span data-ttu-id="b0ce6-167">URL 範本可以針對狀態碼包含 `{0}` 預留位置，如範例所示。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-167">The URL template can include a `{0}` placeholder for the status code, as shown in the example.</span></span> <span data-ttu-id="b0ce6-168">如果 URL 範本是以波狀符號 (~) 為開頭，該波狀符號會被應用程式的 `PathBase`取代。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-168">If the URL template starts with a tilde (~), the tilde is replaced by the app's `PathBase`.</span></span> <span data-ttu-id="b0ce6-169">如果您指向應用程式內的端點，請針對該端點建立 MVC 檢視或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-169">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="b0ce6-170">如需 Razor Pages 範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)中的 *Pages/StatusCode.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-170">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="b0ce6-171">此方法通常是在下列應用程式相關情況下使用：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-171">This method is commonly used when the app:</span></span>

* <span data-ttu-id="b0ce6-172">應用程式應將用戶端重新導向至不同的端點時 (通常是在由其他應用程式處理錯誤的情況下)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-172">Should redirect the client to a different endpoint, usually in cases where a different app processes the error.</span></span> <span data-ttu-id="b0ce6-173">針對 Web 應用程式，用戶端的瀏覽器網址列會反映重新導向後的端點。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-173">For web apps, the client's browser address bar reflects the redirected endpoint.</span></span>
* <span data-ttu-id="b0ce6-174">應用程式不應該保留並傳回原始狀態碼與初始重新導向回應時。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-174">Shouldn't preserve and return the original status code with the initial redirect response.</span></span>

## <a name="usestatuscodepageswithreexecute"></a><span data-ttu-id="b0ce6-175">UseStatusCodePagesWithReExecute</span><span class="sxs-lookup"><span data-stu-id="b0ce6-175">UseStatusCodePagesWithReExecute</span></span>

<span data-ttu-id="b0ce6-176"><xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> 擴充方法：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-176">The <xref:Microsoft.AspNetCore.Builder.StatusCodePagesExtensions.UseStatusCodePagesWithReExecute*> extension method:</span></span>

* <span data-ttu-id="b0ce6-177">傳回原始狀態碼給用戶端。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-177">Returns the original status code to the client.</span></span>
* <span data-ttu-id="b0ce6-178">可透過使用替代路徑重新執行要求管線來產生回應本文。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-178">Generates the response body by re-executing the request pipeline using an alternate path.</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Startup.cs?name=snippet_StatusCodePagesWithReExecute)]

<span data-ttu-id="b0ce6-179">如果您指向應用程式內的端點，請針對該端點建立 MVC 檢視或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-179">If you point to an endpoint within the app, create an MVC view or Razor page for the endpoint.</span></span> <span data-ttu-id="b0ce6-180">如需 Razor Pages 範例，請參閱[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples)中的 *Pages/StatusCode.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-180">For a Razor Pages example, see *Pages/StatusCode.cshtml* in the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/error-handling/samples).</span></span>

<span data-ttu-id="b0ce6-181">此方法通常是在下列應用程式相關情況下使用：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-181">This method is commonly used when the app should:</span></span>

* <span data-ttu-id="b0ce6-182">在不重新導向至其他端點的情況下處理要求。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-182">Process the request without redirecting to a different endpoint.</span></span> <span data-ttu-id="b0ce6-183">針對 Web 應用程式，用戶端的瀏覽器網址列會反映原始要求的端點。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-183">For web apps, the client's browser address bar reflects the originally requested endpoint.</span></span>
* <span data-ttu-id="b0ce6-184">保留並傳回原始狀態碼與回應時。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-184">Preserve and return the original status code with the response.</span></span>

<span data-ttu-id="b0ce6-185">URL 和查詢字串範本可能會包含該狀態碼的預留位置 (`{0}`)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-185">The URL and query string templates may include a placeholder (`{0}`) for the status code.</span></span> <span data-ttu-id="b0ce6-186">URL 範本的開頭必須是斜線 (`/`)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-186">The URL template must start with a slash (`/`).</span></span> <span data-ttu-id="b0ce6-187">在路徑中使用預留位置時，請確認端點 (頁面或控制器) 可以處理路徑線段。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-187">When using a placeholder in the path, confirm that the endpoint (page or controller) can process the path segment.</span></span> <span data-ttu-id="b0ce6-188">例如適用於錯誤的 Razor Page 應接受具備 `@page` 指示詞的選擇性路徑區段值：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-188">For example, a Razor Page for errors should accept the optional path segment value with the `@page` directive:</span></span>

```cshtml
@page "{code?}"
```

<span data-ttu-id="b0ce6-189">處理錯誤的端點可以取得產生該錯誤的原始 URL，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-189">The endpoint that processes the error can get the original URL that generated the error, as shown in the following example:</span></span>

[!code-csharp[](error-handling/samples/2.x/ErrorHandlingSample/Pages/StatusCode.cshtml.cs?name=snippet_StatusCodeReExecute)]

## <a name="disable-status-code-pages"></a><span data-ttu-id="b0ce6-190">停用狀態碼頁面</span><span class="sxs-lookup"><span data-stu-id="b0ce6-190">Disable status code pages</span></span>

<span data-ttu-id="b0ce6-191">若要停用 MVC 控制器或動作方法的狀態字碼頁面，請使用[`[SkipStatusCodePages]`](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-191">To disable status code pages for an MVC controller or action method, use the [`[SkipStatusCodePages]`](xref:Microsoft.AspNetCore.Mvc.SkipStatusCodePagesAttribute) attribute.</span></span>

<span data-ttu-id="b0ce6-192">若要停用 Razor 頁面處理常式方法或 MVC 控制器中的特定要求狀態碼頁面，請使用 <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-192">To disable status code pages for specific requests in a Razor Pages handler method or in an MVC controller, use <xref:Microsoft.AspNetCore.Diagnostics.IStatusCodePagesFeature>:</span></span>

```csharp
var statusCodePagesFeature = HttpContext.Features.Get<IStatusCodePagesFeature>();

if (statusCodePagesFeature != null)
{
    statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="b0ce6-193">例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="b0ce6-193">Exception-handling code</span></span>

<span data-ttu-id="b0ce6-194">例外狀況處理頁面中的程式碼，可擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-194">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="b0ce6-195">一般來說，較好的做法是讓生產環境的錯誤頁面由純靜態內容組成。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-195">It's often a good idea for production error pages to consist of purely static content.</span></span>

### <a name="response-headers"></a><span data-ttu-id="b0ce6-196">回應標頭</span><span class="sxs-lookup"><span data-stu-id="b0ce6-196">Response headers</span></span>

<span data-ttu-id="b0ce6-197">一旦傳送回應的標頭之後：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-197">Once the headers for a response are sent:</span></span>

* <span data-ttu-id="b0ce6-198">應用程式就無法變更回應的狀態碼。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-198">The app can't change the response's status code.</span></span>
* <span data-ttu-id="b0ce6-199">無法執行任何例外狀況頁面或處理常式。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-199">Any exception pages or handlers can't run.</span></span> <span data-ttu-id="b0ce6-200">回應必須完成，否則會中止連線。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-200">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="b0ce6-201">伺服器例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="b0ce6-201">Server exception handling</span></span>

<span data-ttu-id="b0ce6-202">除了應用程式中的例外狀況處理邏輯之外，[HTTP 伺服器實作](xref:fundamentals/servers/index)也可以處理一些例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-202">In addition to the exception handling logic in your app, the [HTTP server implementation](xref:fundamentals/servers/index) can handle some exceptions.</span></span> <span data-ttu-id="b0ce6-203">如果伺服器在回應標頭傳送之前攔截到例外狀況，伺服器會傳送「500 - 內部伺服器錯誤」回應，且沒有回應本文。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-203">If the server catches an exception before response headers are sent, the server sends a *500 - Internal Server Error* response without a response body.</span></span> <span data-ttu-id="b0ce6-204">如果伺服器在回應標頭傳送之後攔截到例外狀況，伺服器會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-204">If the server catches an exception after response headers are sent, the server closes the connection.</span></span> <span data-ttu-id="b0ce6-205">應用程式未處理的要求會由伺服器來處理。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-205">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="b0ce6-206">當伺服器處理要求時，任何發生的例外狀況均由伺服器的例外狀況處理功能來處理。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-206">Any exception that occurs when the server is handling the request is handled by the server's exception handling.</span></span> <span data-ttu-id="b0ce6-207">應用程式的自訂錯誤頁面、例外狀況處理中介軟體或篩選條件並不會影響此行為。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-207">The app's custom error pages, exception handling middleware, and filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="b0ce6-208">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="b0ce6-208">Startup exception handling</span></span>

<span data-ttu-id="b0ce6-209">只有裝載層可以處理應用程式啟動期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-209">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="b0ce6-210">可以將主機設定為會[擷取啟動錯誤](xref:fundamentals/host/web-host#capture-startup-errors)和[擷取詳細錯誤](xref:fundamentals/host/web-host#detailed-errors)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-210">The host can be configured to [capture startup errors](xref:fundamentals/host/web-host#capture-startup-errors) and [capture detailed errors](xref:fundamentals/host/web-host#detailed-errors).</span></span>

<span data-ttu-id="b0ce6-211">只有在錯誤是於主機位址/連接埠繫結之後發生的情況下，裝載層才能顯示已擷取之啟動錯誤的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-211">The hosting layer can show an error page for a captured startup error only if the error occurs after host address/port binding.</span></span> <span data-ttu-id="b0ce6-212">如果繫結失敗：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-212">If binding fails:</span></span>

* <span data-ttu-id="b0ce6-213">裝載層會記錄重大例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-213">The hosting layer logs a critical exception.</span></span>
* <span data-ttu-id="b0ce6-214">Dotnet 會處理損毀狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-214">The dotnet process crashes.</span></span>
* <span data-ttu-id="b0ce6-215">當 HTTP 伺服器是 [Kestrel](xref:fundamentals/servers/kestrel) 時，不會顯示任何錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-215">No error page is displayed when the HTTP server is [Kestrel](xref:fundamentals/servers/kestrel).</span></span>

<span data-ttu-id="b0ce6-216">在 [IIS](/iis) (或 Azure App Service) 或 [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) 上執行時，如果無法啟動處理序，[模組](xref:host-and-deploy/aspnet-core-module)會傳回 *502.5 - 處理序失敗*。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-216">When running on [IIS](/iis) (or Azure App Service) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), a *502.5 - Process Failure* is returned by the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) if the process can't start.</span></span> <span data-ttu-id="b0ce6-217">如需詳細資訊，請參閱<xref:test/troubleshoot-azure-iis>。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-217">For more information, see <xref:test/troubleshoot-azure-iis>.</span></span>

## <a name="database-error-page"></a><span data-ttu-id="b0ce6-218">資料庫錯誤頁面</span><span class="sxs-lookup"><span data-stu-id="b0ce6-218">Database error page</span></span>

<span data-ttu-id="b0ce6-219">資料庫錯誤頁面中介軟體會捕捉資料庫相關的例外狀況，可使用 Entity Framework 遷移來解決。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-219">Database Error Page Middleware captures database-related exceptions that can be resolved by using Entity Framework migrations.</span></span> <span data-ttu-id="b0ce6-220">發生這些例外狀況時，系統會產生具解決該問題之可能動作詳細資料的 HTML 回應。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-220">When these exceptions occur, an HTML response with details of possible actions to resolve the issue is generated.</span></span> <span data-ttu-id="b0ce6-221">此頁面僅應該於開發環境中啟用。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-221">This page should be enabled only in the Development environment.</span></span> <span data-ttu-id="b0ce6-222">將程式碼加入 `Startup.Configure` 來啟用該頁面：</span><span class="sxs-lookup"><span data-stu-id="b0ce6-222">Enable the page by adding code to `Startup.Configure`:</span></span>

```csharp
if (env.IsDevelopment())
{
    app.UseDatabaseErrorPage();
}
```

<!-- FUTURE UPDATE: On the next topic overhaul/release update, add API crosslink to this section for xref:Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage* when available via the API docs. -->

## <a name="exception-filters"></a><span data-ttu-id="b0ce6-223">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="b0ce6-223">Exception filters</span></span>

<span data-ttu-id="b0ce6-224">在 MVC 應用程式中，您能以全域設定例外狀況篩選條件，或是以每個控制器或每個動作為基礎的方式設定。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-224">In MVC apps, exception filters can be configured globally or on a per-controller or per-action basis.</span></span> <span data-ttu-id="b0ce6-225">在 Razor Pages 應用程式中，您能以全域或每個頁面模型的方式設定它們。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-225">In Razor Pages apps, they can be configured globally or per page model.</span></span> <span data-ttu-id="b0ce6-226">這些篩選會處理在控制器動作或其他篩選條件執行期間發生但的任何未處理例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-226">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter.</span></span> <span data-ttu-id="b0ce6-227">如需詳細資訊，請參閱<xref:mvc/controllers/filters#exception-filters>。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-227">For more information, see <xref:mvc/controllers/filters#exception-filters>.</span></span>

> [!TIP]
> <span data-ttu-id="b0ce6-228">例外狀況篩選條件適合用來截獲 MVC 動作中發生的例外狀況，但是它們並不像例外狀況處理中介軟體那麼有彈性。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-228">Exception filters are useful for trapping exceptions that occur within MVC actions, but they're not as flexible as the Exception Handling Middleware.</span></span> <span data-ttu-id="b0ce6-229">我們建議使用中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-229">We recommend using the middleware.</span></span> <span data-ttu-id="b0ce6-230">請只在需要根據已選擇的 MVC 動作執行不同的錯誤處理時，才使用篩選條件。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-230">Use filters only where you need to perform error handling differently based on which MVC action is chosen.</span></span>

## <a name="model-state-errors"></a><span data-ttu-id="b0ce6-231">模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="b0ce6-231">Model state errors</span></span>

<span data-ttu-id="b0ce6-232">如需如何處理模型狀態錯誤的相關資訊，請參閱[模型繫結](xref:mvc/models/model-binding)和[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="b0ce6-232">For information about how to handle model state errors, see [Model binding](xref:mvc/models/model-binding) and [Model validation](xref:mvc/models/validation).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b0ce6-233">其他資源</span><span class="sxs-lookup"><span data-stu-id="b0ce6-233">Additional resources</span></span>

* <xref:test/troubleshoot-azure-iis>
* <xref:host-and-deploy/azure-iis-errors-reference>
