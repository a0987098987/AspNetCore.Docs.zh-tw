---
title: "ASP.NET Core 中的錯誤處理"
author: ardalis
description: "了解如何在 ASP.NET Core 應用程式中處理錯誤。"
ms.author: tdykstra
manager: wpickett
ms.date: 11/30/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/error-handling
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 49507e90cd659be5da08df17e175297adad0fea1
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-error-handling-in-aspnet-core"></a><span data-ttu-id="ab4ea-103">Introduction to ASP.NET Core 中的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="ab4ea-103">Introduction to Error Handling in ASP.NET Core</span></span>

<span data-ttu-id="ab4ea-104">作者：[Steve Smith](https://ardalis.com/) 和 [Tom Dykstra](https://github.com/tdykstra/)</span><span class="sxs-lookup"><span data-stu-id="ab4ea-104">By [Steve Smith](https://ardalis.com/) and [Tom Dykstra](https://github.com/tdykstra/)</span></span>

<span data-ttu-id="ab4ea-105">本文涵蓋常見 appoaches 處理 ASP.NET Core 應用程式中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-105">This article covers common appoaches to handling errors in ASP.NET Core apps.</span></span>

<span data-ttu-id="ab4ea-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ab4ea-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/error-handling/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="the-developer-exception-page"></a><span data-ttu-id="ab4ea-107">[開發人員的例外狀況] 頁面</span><span class="sxs-lookup"><span data-stu-id="ab4ea-107">The developer exception page</span></span>

<span data-ttu-id="ab4ea-108">若要設定應用程式顯示一個頁面，會顯示例外狀況的相關詳細的資訊，請安裝`Microsoft.AspNetCore.Diagnostics`NuGet 封裝，並將一條線[啟動類別中設定方法](startup.md):</span><span class="sxs-lookup"><span data-stu-id="ab4ea-108">To configure an app to display a page that shows detailed information about exceptions, install the `Microsoft.AspNetCore.Diagnostics` NuGet package and add a line to the [Configure method in the Startup class](startup.md):</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=7)]

<span data-ttu-id="ab4ea-109">Put`UseDeveloperExceptionPage`之前您要攔截例外狀況中，例如任何中介軟體`app.UseMvc`。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-109">Put `UseDeveloperExceptionPage` before any middleware you want to catch exceptions in, such as `app.UseMvc`.</span></span>

>[!WARNING]
> <span data-ttu-id="ab4ea-110">啟用開發人員例外狀況頁面**只有應用程式在執行時在開發環境中**。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-110">Enable the developer exception page **only when the app is running in the Development environment**.</span></span> <span data-ttu-id="ab4ea-111">您不想在生產環境中執行的應用程式時，公開共用詳細例外狀況資訊。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-111">You don't want to share detailed exception information publicly when the app runs in production.</span></span> <span data-ttu-id="ab4ea-112">[深入了解設定環境](environments.md)。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-112">[Learn more about configuring environments](environments.md).</span></span>

<span data-ttu-id="ab4ea-113">若要查看 [開發人員的例外狀況] 頁面，執行範例應用程式的環境設定為`Development`，並加入`?throw=true`基底 url 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-113">To see the developer exception page, run the sample application with the environment set to `Development`, and add `?throw=true` to the base URL of the app.</span></span> <span data-ttu-id="ab4ea-114">此頁面包含例外狀況，並要求的相關資訊的數個索引標籤。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-114">The page includes several tabs with information about the exception and the request.</span></span> <span data-ttu-id="ab4ea-115">第一個索引標籤包含堆疊追蹤。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-115">The first tab includes a stack trace.</span></span> 

![堆疊追蹤](error-handling/_static/developer-exception-page.png)

<span data-ttu-id="ab4ea-117">如果有的話，[下一步] 索引標籤將顯示的查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-117">The next tab shows the query string parameters, if any.</span></span>

![查詢字串參數](error-handling/_static/developer-exception-page-query.png)

<span data-ttu-id="ab4ea-119">此要求沒有任何 cookie，但如果有，它們會出現在上**Cookie**  索引標籤。您可以看到最後一個索引標籤中所傳遞的標頭。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-119">This request didn't have any cookies, but if it did, they would appear on the **Cookies** tab. You can see the headers that were passed in the last tab.</span></span>

![標頭](error-handling/_static/developer-exception-page-headers.png)

## <a name="configuring-a-custom-exception-handling-page"></a><span data-ttu-id="ab4ea-121">設定自訂的例外狀況處理頁面</span><span class="sxs-lookup"><span data-stu-id="ab4ea-121">Configuring a custom exception handling page</span></span>

<span data-ttu-id="ab4ea-122">它是個不錯的主意設定不以執行應用程式時所要使用的例外狀況處理常式頁面`Development`環境。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-122">It's a good idea to configure an exception handler page to use when the app is not running in the `Development` environment.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_DevExceptionPage&highlight=11)]

<span data-ttu-id="ab4ea-123">在 MVC 應用程式中不明確裝飾錯誤處理常式的動作方法，以 HTTP 方法的屬性，例如`HttpGet`。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-123">In an MVC app, don't explicitly decorate the error handler action method with HTTP method attributes, such as `HttpGet`.</span></span> <span data-ttu-id="ab4ea-124">使用明確的動詞命令無法防止某些要求的方法。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-124">Using explicit verbs could prevent some requests from reaching the method.</span></span>

```csharp
[Route("/Error")]
public IActionResult Index()
{
    // Handle error here
}
```

## <a name="configuring-status-code-pages"></a><span data-ttu-id="ab4ea-125">設定狀態的字碼頁</span><span class="sxs-lookup"><span data-stu-id="ab4ea-125">Configuring status code pages</span></span>

<span data-ttu-id="ab4ea-126">根據預設，您的應用程式不會提供豐富的狀態字碼頁的 HTTP 狀態碼 500 （內部伺服器錯誤） 或 404 （找不到） 等。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-126">By default, your app will not provide a rich status code page for HTTP status codes such as 500 (Internal Server Error) or 404 (Not Found).</span></span> <span data-ttu-id="ab4ea-127">您可以設定`StatusCodePagesMiddleware`將這一行加入`Configure`方法：</span><span class="sxs-lookup"><span data-stu-id="ab4ea-127">You can configure the `StatusCodePagesMiddleware` by adding a line to the `Configure` method:</span></span>

```csharp
app.UseStatusCodePages();
```

<span data-ttu-id="ab4ea-128">根據預設，此中介軟體會加入常見狀態碼 404 等簡單、 純文字的處理常式：</span><span class="sxs-lookup"><span data-stu-id="ab4ea-128">By default, this middleware adds simple, text-only handlers for common status codes, such as 404:</span></span>

![404 頁面](error-handling/_static/default-404-status-code.png)

<span data-ttu-id="ab4ea-130">中介軟體可支援幾種不同的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-130">The middleware supports several different extension methods.</span></span> <span data-ttu-id="ab4ea-131">另一個則採用 lambda 運算式，另一個採用內容類型和格式字串。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-131">One takes a lambda expression, another takes a content type and format string.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePages)]

```csharp
app.UseStatusCodePages("text/plain", "Status code page, status code: {0}");
```

<span data-ttu-id="ab4ea-132">也有重新導向的擴充方法。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-132">There are also redirect extension methods.</span></span> <span data-ttu-id="ab4ea-133">其中一個會 302 狀態碼傳送給用戶端，而其中一個原始的狀態碼傳回給用戶端，但也會重新導向 URL 執行此處理常式。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-133">One sends a 302 status code to the client, and one returns the original status code to the client but also executes the handler for the redirect URL.</span></span>

[!code-csharp[Main](error-handling/sample/Startup.cs?name=snippet_StatusCodePagesWithRedirect)]

```csharp
app.UseStatusCodePagesWithReExecute("/error/{0}");
```

<span data-ttu-id="ab4ea-134">如果您需要停用狀態的特定要求的字碼頁，您可以這樣做：</span><span class="sxs-lookup"><span data-stu-id="ab4ea-134">If you need to disable status code pages for certain requests, you can do so:</span></span>

```csharp
var statusCodePagesFeature = context.Features.Get<IStatusCodePagesFeature>();
if (statusCodePagesFeature != null)
{
  statusCodePagesFeature.Enabled = false;
}
```

## <a name="exception-handling-code"></a><span data-ttu-id="ab4ea-135">例外狀況處理程式碼</span><span class="sxs-lookup"><span data-stu-id="ab4ea-135">Exception-handling code</span></span>

<span data-ttu-id="ab4ea-136">例外狀況處理網頁中的程式碼，可擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-136">Code in exception handling pages can throw exceptions.</span></span> <span data-ttu-id="ab4ea-137">它通常是個不錯的主意，生產環境錯誤頁來組成純粹靜態內容。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-137">It's often a good idea for production error pages to consist of purely static content.</span></span>

<span data-ttu-id="ab4ea-138">此外，請注意，一旦傳送回應標頭之後，您無法變更回應的狀態碼，也不可以任何例外狀況網頁或處理常式執行。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-138">Also, be aware that once the headers for a response have been sent, you can't change the response's status code, nor can any exception pages or handlers run.</span></span> <span data-ttu-id="ab4ea-139">回應必須完成或中止連線。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-139">The response must be completed or the connection aborted.</span></span>

## <a name="server-exception-handling"></a><span data-ttu-id="ab4ea-140">伺服器例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="ab4ea-140">Server exception handling</span></span>

<span data-ttu-id="ab4ea-141">除了例外狀況處理應用程式中的邏輯[伺服器](servers/index.md)裝載您的應用程式會執行一些例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-141">In addition to the exception handling logic in your app, the [server](servers/index.md) hosting your app performs some exception handling.</span></span> <span data-ttu-id="ab4ea-142">如果標頭傳送之前，伺服器會攔截例外狀況，伺服器會傳送 500 內部伺服器錯誤回應，且沒有主體。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-142">If the server catches an exception before the headers are sent, the server sends a 500 Internal Server Error response with no body.</span></span> <span data-ttu-id="ab4ea-143">如果傳送的標頭後，伺服器會攔截例外狀況，伺服器就會關閉連接。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-143">If the server catches an exception after the headers have been sent, the server closes the connection.</span></span> <span data-ttu-id="ab4ea-144">不由您的應用程式的要求是由伺服器處理。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-144">Requests that aren't handled by your app are handled by the server.</span></span> <span data-ttu-id="ab4ea-145">任何發生的例外狀況由伺服器的例外狀況處理。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-145">Any exception that occurs is handled by the server's exception handling.</span></span> <span data-ttu-id="ab4ea-146">任何設定自訂錯誤網頁，或例外狀況處理中介軟體或篩選器不會影響這個行為。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-146">Any configured custom error pages or exception handling middleware or filters don't affect this behavior.</span></span>

## <a name="startup-exception-handling"></a><span data-ttu-id="ab4ea-147">啟動例外狀況處理</span><span class="sxs-lookup"><span data-stu-id="ab4ea-147">Startup exception handling</span></span>

<span data-ttu-id="ab4ea-148">只裝載層可以處理應用程式啟動期間發生的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-148">Only the hosting layer can handle exceptions that take place during app startup.</span></span> <span data-ttu-id="ab4ea-149">您可以[設定主應用程式的運作方式的錯誤回應在啟動期間](hosting.md#detailed-errors)使用`captureStartupErrors`和`detailedErrors`索引鍵。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-149">You can [configure how the host behaves in response to errors during startup](hosting.md#detailed-errors) using `captureStartupErrors` and the `detailedErrors` key.</span></span>

<span data-ttu-id="ab4ea-150">裝載可以只會顯示錯誤頁面擷取的啟動錯誤，如果主機位址/連接埠繫結之後，就會發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-150">Hosting can only show an error page for a captured startup error if the error occurs after host address/port binding.</span></span> <span data-ttu-id="ab4ea-151">如果任何繫結失敗，因為任何原因，裝載層記錄重大例外狀況 dotnet 處理序損毀，並不顯示任何錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-151">If any binding fails for any reason, the hosting layer logs a critical exception, the dotnet process crashes, and no error page is displayed.</span></span>

## <a name="aspnet-mvc-error-handling"></a><span data-ttu-id="ab4ea-152">ASP.NET MVC 的錯誤處理</span><span class="sxs-lookup"><span data-stu-id="ab4ea-152">ASP.NET MVC error handling</span></span>

<span data-ttu-id="ab4ea-153">[MVC](../mvc/index.md)應用程式有錯誤，例如設定例外狀況篩選條件，以及執行模型驗證處理一些其他選項。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-153">[MVC](../mvc/index.md) apps have some additional options for handling errors, such as configuring exception filters and performing model validation.</span></span>

### <a name="exception-filters"></a><span data-ttu-id="ab4ea-154">例外狀況篩選條件</span><span class="sxs-lookup"><span data-stu-id="ab4ea-154">Exception Filters</span></span>

<span data-ttu-id="ab4ea-155">全域或根據每個控制站或每個動作的 MVC 應用程式中，可以設定例外狀況篩選條件。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-155">Exception filters can be configured globally or on a per-controller or per-action basis in an MVC app.</span></span> <span data-ttu-id="ab4ea-156">這些篩選器處理控制器動作，或另一個篩選，在執行期間發生任何未處理例外狀況，而不會呼叫否則。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-156">These filters handle any unhandled exception that occurs during the execution of a controller action or another filter, and are not called otherwise.</span></span> <span data-ttu-id="ab4ea-157">深入了解例外狀況篩選條件中[篩選](../mvc/controllers/filters.md)。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-157">Learn more about exception filters in [Filters](../mvc/controllers/filters.md).</span></span>

>[!TIP]
> <span data-ttu-id="ab4ea-158">例外狀況篩選條件則適合用於設陷 MVC 動作中發生的例外狀況，但是它們並不具有彈性，錯誤處理中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-158">Exception filters are good for trapping exceptions that occur within MVC actions, but they're not as flexible as error handling middleware.</span></span> <span data-ttu-id="ab4ea-159">偏好以一般的情況下中, 介軟體，並使用篩選器只需要執行錯誤處理*不同*根據選擇的 MVC 動作。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-159">Prefer middleware for the general case, and use filters only where you need to do error handling *differently* based on which MVC action was chosen.</span></span>

### <a name="handling-model-state-errors"></a><span data-ttu-id="ab4ea-160">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="ab4ea-160">Handling Model State Errors</span></span>

<span data-ttu-id="ab4ea-161">[模型驗證](../mvc/models/validation.md)發生之前所叫用每個控制器動作和動作方法必須負責檢查`ModelState.IsValid`和作出適當回應。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-161">[Model validation](../mvc/models/validation.md) occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span>

<span data-ttu-id="ab4ea-162">某些應用程式都可以選擇在此情況下遵循標準處理模型驗證錯誤，慣例[篩選](../mvc/controllers/filters.md)可能是適當的位置，實作這類的原則。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-162">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a [filter](../mvc/controllers/filters.md) may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="ab4ea-163">您應該測試您的動作具有無效的模型狀態的行為方式。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-163">You should test how your actions behave with invalid model states.</span></span> <span data-ttu-id="ab4ea-164">進一步了解[測試控制器邏輯](../mvc/controllers/testing.md)。</span><span class="sxs-lookup"><span data-stu-id="ab4ea-164">Learn more in [Testing controller logic](../mvc/controllers/testing.md).</span></span>



