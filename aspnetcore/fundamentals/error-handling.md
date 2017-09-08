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
ms.openlocfilehash: 5898892c63e978adfabf9939394fef4ea1848d49
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="52a12-104">Introduction to ASP.NET Core 中的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="52a12-104">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="52a12-105">由[Steve Smith](http://ardalis.com)和[Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="52a12-105">By [Steve Smith](http://ardalis.com) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="52a12-106">本文涵蓋常見 appoaches 處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="52a12-106">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

[<span data-ttu-id="52a12-107">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="52a12-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample)

## <a name="the-developer-exception-page"></a><span data-ttu-id="52a12-108">[開發人員的例外狀況] 頁面</span><span class="sxs-lookup"><span data-stu-id="52a12-108">The developer exception page</span></span>

<span data-ttu-id="52a12-109">若要設定應用程式顯示一個頁面，會顯示例外狀況的相關詳細的資訊，請安裝`Microsoft.AspNetCore.Diagnostics`NuGet 封裝，並將一條線[啟動類別中設定方法](startup.md):</span><span class="sxs-lookup"><span data-stu-id="52a12-109">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

<span data-ttu-id="52a12-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span><span class="sxs-lookup"><span data-stu-id="52a12-110">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]</span></span>

<span data-ttu-id="52a12-111">Put`UseDeveloperExceptionPage`之前您要攔截例外狀況中，例如任何中介軟體`app.UseMvc`。</span><span class="sxs-lookup"><span data-stu-id="52a12-111">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="52a12-112">啟用開發人員例外狀況頁面**只有應用程式在執行時在開發環境中**。</span><span class="sxs-lookup"><span data-stu-id="52a12-112">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="52a12-113">您不想在生產環境中執行的應用程式時，公開共用詳細例外狀況資訊。</span><span class="sxs-lookup"><span data-stu-id="52a12-113">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="52a12-114">[深入了解設定環境](environments.md)。</span><span class="sxs-lookup"><span data-stu-id="52a12-114">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="52a12-115">若要查看 [開發人員的例外狀況] 頁面，執行範例應用程式的環境設定為`Development`，並加入`?throw=true`基底 url 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="52a12-115">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="52a12-116">此頁面包含例外狀況，並要求的相關資訊的數個索引標籤。</span><span class="sxs-lookup"><span data-stu-id="52a12-116">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="52a12-117">第一個索引標籤包含堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="52a12-117">The first tab includes a stack trace.</span></span> 

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="52a12-119">如果有的話，[下一步] 索引標籤將顯示的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="52a12-119">The next tab shows the query string parameters, if any.</span></span>

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="52a12-121">此要求沒有任何 cookie，但如果有，它們會出現在上**Cookie**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="52a12-121">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab.</span></span> <span data-ttu-id="52a12-122">您可以看到最後一個索引標籤中所傳遞的標頭。</span><span class="sxs-lookup"><span data-stu-id="52a12-122">You can see the headers that were passed in the last tab.</span></span>

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="52a12-124">設定自訂的例外狀況處理頁面</span><span class="sxs-lookup"><span data-stu-id="52a12-124">Configuring a custom exception handling page</span></span>

<span data-ttu-id="52a12-125">它是個不錯的主意設定不以執行應用程式時所要使用的例外狀況處理常式頁面`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="52a12-125">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

<span data-ttu-id="52a12-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span><span class="sxs-lookup"><span data-stu-id="52a12-126">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]</span></span>

<span data-ttu-id="52a12-127">在 MVC 應用程式中不明確裝飾錯誤處理常式的動作方法，以 HTTP 方法的屬性，例如`HttpGet`。</span><span class="sxs-lookup"><span data-stu-id="52a12-127">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="52a12-128">使用明確的動詞命令無法防止某些要求的方法。</span><span class="sxs-lookup"><span data-stu-id="52a12-128">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="52a12-129">設定狀態的字碼頁</span><span class="sxs-lookup"><span data-stu-id="52a12-129">Configuring status code pages</span></span>

<span data-ttu-id="52a12-130">根據預設，您的應用程式不會提供豐富的狀態字碼頁的 HTTP 狀態碼 500 （內部伺服器錯誤） 或 404 （找不到） 等。</span><span class="sxs-lookup"><span data-stu-id="52a12-130">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="52a12-131">您可以設定`StatusCodePagesMiddleware`將這一行加入`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="52a12-131">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="52a12-132">根據預設，此中介軟體會加入常見狀態碼 404 等簡單、 純文字的處理常式：</span><span class="sxs-lookup"><span data-stu-id="52a12-132">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 頁面](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="52a12-134">中介軟體可支援幾種不同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="52a12-134">The middleware supports several different extension methods.</span></span> <span data-ttu-id="52a12-135">另一個則採用 lambda 運算式，另一個採用內容類型和格式字串。</span><span class="sxs-lookup"><span data-stu-id="52a12-135">One takes a lambda expression, another takes a content type and format string.</span></span>

<span data-ttu-id="52a12-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span><span class="sxs-lookup"><span data-stu-id="52a12-136">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]</span></span>

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="52a12-137">也有重新導向的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="52a12-137">There are also redirect extension methods.</span></span> <span data-ttu-id="52a12-138">其中一個會 302 狀態碼傳送給用戶端，而其中一個原始的狀態碼傳回給用戶端，但也會重新導向 URL 執行此處理常式。</span><span class="sxs-lookup"><span data-stu-id="52a12-138">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

<span data-ttu-id="52a12-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span><span class="sxs-lookup"><span data-stu-id="52a12-139">[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]</span></span>

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="52a12-140">如果您需要停用狀態的特定要求的字碼頁，您可以這樣做：</span><span class="sxs-lookup"><span data-stu-id="52a12-140">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="52a12-141">例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="52a12-141">Exception-handling code</span></span>

<span data-ttu-id="52a12-142">例外狀況處理網頁中的程式碼，可擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52a12-142">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="52a12-143">它通常是個不錯的主意，生產環境錯誤頁來組成純粹靜態內容。</span><span class="sxs-lookup"><span data-stu-id="52a12-143">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="52a12-144">此外，請注意，一旦傳送回應標頭之後，您無法變更回應的狀態碼，也不可以任何例外狀況網頁或處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="52a12-144">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="52a12-145">回應必須完成或中止連線。</span><span class="sxs-lookup"><span data-stu-id="52a12-145">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="52a12-146">伺服器例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="52a12-146">Server exception handling</span></span>

<span data-ttu-id="52a12-147">除了例外狀況處理應用程式中的邏輯[伺服器](servers/index.md)裝載您的應用程式將會執行一些例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="52a12-147">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app will perform some exception handling.</span></span> <span data-ttu-id="52a12-148">如果之前已傳送標頭傳送 500 內部伺服器錯誤回應，且沒有主體伺服器會攔截例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52a12-148">If the server catches an exception before the headers have been sent it sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="52a12-149">如果傳送的標頭後，它會攔截例外狀況，就會關閉連線。</span><span class="sxs-lookup"><span data-stu-id="52a12-149">If it catches an exception after the headers have been sent, it closes the connection.</span></span> <span data-ttu-id="52a12-150">不會處理您的應用程式的要求將由伺服器和伺服器的例外狀況處理，就會發生任何例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="52a12-150">Requests that are not handled by your app will be handled by the server, and any exception that occurs will be handled by the server's exception handling.</span></span> <span data-ttu-id="52a12-151">任何自訂錯誤網頁或例外狀況處理中介軟體或您已設定您的應用程式的篩選器不會影響這個行為。</span><span class="sxs-lookup"><span data-stu-id="52a12-151">Any custom error pages or exception handling middleware or filters you have configured for your app will not affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="52a12-152">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="52a12-152">Startup exception handling</span></span>

<span data-ttu-id="52a12-153">只裝載層可以處理應用程式啟動期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52a12-153">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="52a12-154">應用程式啟動期間發生例外狀況可能會影響伺服器的行為。</span><span class="sxs-lookup"><span data-stu-id="52a12-154">Exceptions that occur during app startup can impact server behavior.</span></span> <span data-ttu-id="52a12-155">例如，如果發生例外狀況，才能呼叫`KestrelServerOptions.UseHttps`，裝載層攔截例外狀況，可啟動伺服器，並且顯示錯誤頁面上的非 SSL 連接埠。</span><span class="sxs-lookup"><span data-stu-id="52a12-155">For example, if an exception happens before you call `KestrelServerOptions.UseHttps`, the hosting layer catches the exception, starts the server, and displays an error page on the non-SSL port.</span></span> <span data-ttu-id="52a12-156">如果發生例外狀況，執行該行之後，則改為透過 HTTPS 提供錯誤網頁。</span><span class="sxs-lookup"><span data-stu-id="52a12-156">If an exception happens after that line executes, the error page is served over HTTPS instead.</span></span>

<span data-ttu-id="52a12-157">您可以[設定主機的行為方式以回應錯誤在啟動期間](hosting.md#configuring-a-host)使用`CaptureStartupErrors`和`detailedErrors`索引鍵。</span><span class="sxs-lookup"><span data-stu-id="52a12-157">You can [configure how the host will behave in response to errors during startup](hosting.md#configuring-a-host) using `CaptureStartupErrors` and the `detailedErrors` key.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="52a12-158">ASP.NET MVC 的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="52a12-158">ASP.NET MVC error handling</span></span>

<span data-ttu-id="52a12-159">[MVC](../mvc/index.md)應用程式有錯誤，例如設定例外狀況篩選條件，以及執行模型驗證處理一些其他選項。</span><span class="sxs-lookup"><span data-stu-id="52a12-159">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="52a12-160">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="52a12-160">Exception Filters</span></span>

<span data-ttu-id="52a12-161">全域或根據每個控制站或每個動作的 MVC 應用程式中，可以設定例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="52a12-161">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="52a12-162">這些篩選器處理控制器動作，或另一個篩選，在執行期間發生任何未處理例外狀況，而不會呼叫否則。</span><span class="sxs-lookup"><span data-stu-id="52a12-162">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="52a12-163">深入了解例外狀況篩選條件中[篩選](../mvc/controllers/filters.md)。</span><span class="sxs-lookup"><span data-stu-id="52a12-163">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="52a12-164">例外狀況篩選條件則適合用於設陷 MVC 動作中發生的例外狀況，但是它們並不具有彈性，錯誤處理中介軟體。</span><span class="sxs-lookup"><span data-stu-id="52a12-164">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="52a12-165">偏好以一般的情況下中, 介軟體，並使用篩選器只需要執行錯誤處理*不同*根據選擇的 MVC 動作。</span><span class="sxs-lookup"><span data-stu-id="52a12-165">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="52a12-166">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="52a12-166">Handling Model State Errors</span></span>

<span data-ttu-id="52a12-167">[模型驗證](../mvc/models/validation.md)發生之前所叫用每個控制器動作和動作方法必須負責檢查`ModelState.IsValid`和作出適當回應。</span><span class="sxs-lookup"><span data-stu-id="52a12-167">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="52a12-168">某些應用程式都可以選擇在此情況下遵循標準處理模型驗證錯誤，慣例[篩選](../mvc/controllers/filters.md)可能是適當的位置，實作這類的原則。</span><span class="sxs-lookup"><span data-stu-id="52a12-168">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="52a12-169">您應該測試您的動作具有無效的模型狀態的行為方式。</span><span class="sxs-lookup"><span data-stu-id="52a12-169">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="52a12-170">進一步了解[測試控制器邏輯](../mvc/controllers/testing.md)。</span><span class="sxs-lookup"><span data-stu-id="52a12-170">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



