---
title: "ASP.NET Core 中介軟體"
author: rick-anderson
description: "說明中介軟體，以及要求管線。"
keywords: "ASP.NET Core 中, 介軟體管線、 委派"
ms.author: riande
manager: wpickett
ms.date: 08/14/2017
ms.topic: article
ms.assetid: db9a86ab-46c2-40e0-baed-86e38c16af1f
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/middleware
ms.openlocfilehash: 80e27c94b3c60a181c45f8e006126a7e7dd3d425
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="aspnet-core-middleware-fundamentals"></a><span data-ttu-id="98aaf-104">ASP.NET Core 中介軟體的基本概念</span><span class="sxs-lookup"><span data-stu-id="98aaf-104">ASP.NET Core Middleware Fundamentals</span></span>

<a name=fundamentals-middleware></a>

<span data-ttu-id="98aaf-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="98aaf-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

[<span data-ttu-id="98aaf-106">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="98aaf-106">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)

## <a name="what-is-middleware"></a><span data-ttu-id="98aaf-107">什麼是中介軟體</span><span class="sxs-lookup"><span data-stu-id="98aaf-107">What is middleware</span></span>

<span data-ttu-id="98aaf-108">中介軟體是組裝至應用程式管線來處理要求和回應的軟體。</span><span class="sxs-lookup"><span data-stu-id="98aaf-108">Middleware is software that is assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="98aaf-109">每個元件：</span><span class="sxs-lookup"><span data-stu-id="98aaf-109">Each component:</span></span>

* <span data-ttu-id="98aaf-110">您可以選擇是否要將要求傳遞至管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="98aaf-110">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="98aaf-111">之前和之後叫用管線中的下一個元件時，就可以執行工作。</span><span class="sxs-lookup"><span data-stu-id="98aaf-111">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="98aaf-112">要求的委派可以用來建立要求管線。</span><span class="sxs-lookup"><span data-stu-id="98aaf-112">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="98aaf-113">將要求委派會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="98aaf-113">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="98aaf-114">要求使用設定委派[執行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)，[對應](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)，和[使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)擴充方法。</span><span class="sxs-lookup"><span data-stu-id="98aaf-114">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="98aaf-115">個別要求委派可以指定內建為匿名方法 （稱為內建的中介軟體），或是它可以定義可重複使用的類別中。</span><span class="sxs-lookup"><span data-stu-id="98aaf-115">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="98aaf-116">這些可重複使用的類別和內嵌匿名方法*中介軟體*，或*中介軟體元件*。</span><span class="sxs-lookup"><span data-stu-id="98aaf-116">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="98aaf-117">要求管線中的每個中介軟體元件負責叫用下一個元件在管線中，或者如果適當的話，最少運算鏈結。</span><span class="sxs-lookup"><span data-stu-id="98aaf-117">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="98aaf-118">[中介軟體的移轉 HTTP 模組](../migration/http-modules.md)說明要求管線中 ASP.NET Core 版本和舊版之間的差異，並提供更多的中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="98aaf-118">[Migrating HTTP Modules to Middleware](../migration/http-modules.md) explains the difference between request pipelines in ASP.NET Core and the previous versions and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="98aaf-119">建立與 IApplicationBuilder 的中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="98aaf-119">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="98aaf-120">ASP.NET Core 要求管線包含一連串要求委派，呼叫其中一個之後，這個圖表可顯示 （執行緒的執行如下所示的黑色箭號）：</span><span class="sxs-lookup"><span data-stu-id="98aaf-120">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![顯示要求抵達，透過三個 middlewares，並讓應用程式保持回應處理的要求處理模式。](middleware/_static/request-delegate-pipeline.png)

<span data-ttu-id="98aaf-124">每個委派之前和之後的下一個委派，可以執行作業。</span><span class="sxs-lookup"><span data-stu-id="98aaf-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="98aaf-125">委派，也可以決定要將要求傳遞至下一步的委派，會呼叫最少運算要求管線。</span><span class="sxs-lookup"><span data-stu-id="98aaf-125">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="98aaf-126">最少運算通常會因為它可讓以避免不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="98aaf-126">Short-circuiting is often desirable because it allows unnecessary work to be avoided.</span></span> <span data-ttu-id="98aaf-127">比方說，靜態檔案中介軟體可以傳回靜態檔案的要求，並最少運算管線的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="98aaf-127">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="98aaf-128">例外狀況處理委派需要呼叫及早在管線中，讓它們可以攔截管線的後續階段中發生例外狀況。</span><span class="sxs-lookup"><span data-stu-id="98aaf-128">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="98aaf-129">最簡單的可能 ASP.NET Core 應用程式設定單一要求委派會處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="98aaf-129">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="98aaf-130">此案例不包含實際的要求管線。</span><span class="sxs-lookup"><span data-stu-id="98aaf-130">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="98aaf-131">相反地，單一的匿名函式會呼叫每個 HTTP 要求的回應。</span><span class="sxs-lookup"><span data-stu-id="98aaf-131">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

<span data-ttu-id="98aaf-132">[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-132">[!code-csharp[Main](middleware/sample/Middleware/Startup.cs)]</span></span>

<span data-ttu-id="98aaf-133">第一個[應用程式。執行](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)委派終止管線。</span><span class="sxs-lookup"><span data-stu-id="98aaf-133">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="98aaf-134">您可以鏈結在一起的多個要求委派[應用程式。使用](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)。</span><span class="sxs-lookup"><span data-stu-id="98aaf-134">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="98aaf-135">`next`參數代表管線中的下一個委派。</span><span class="sxs-lookup"><span data-stu-id="98aaf-135">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="98aaf-136">(請記住，您就可以縮短由管線*不*呼叫*下一步*參數。)您可以通常執行動作之前和之後的下一個委派，如這個範例所示：</span><span class="sxs-lookup"><span data-stu-id="98aaf-136">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

<span data-ttu-id="98aaf-137">[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-137">[!code-csharp[Main](middleware/sample/Chain/Startup.cs?name=snippet1)]</span></span>

>[!WARNING]
> <span data-ttu-id="98aaf-138">請勿呼叫`next.Invoke`回應傳送至用戶端之後。</span><span class="sxs-lookup"><span data-stu-id="98aaf-138">Do not call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="98aaf-139">若要變更`HttpResponse`開始回應後將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="98aaf-139">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="98aaf-140">例如，變更，例如設定標頭、 狀態碼和其他內容，將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="98aaf-140">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="98aaf-141">寫入回應主體之後呼叫`next`:</span><span class="sxs-lookup"><span data-stu-id="98aaf-141">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="98aaf-142">可能會造成通訊協定違規。</span><span class="sxs-lookup"><span data-stu-id="98aaf-142">May cause a protocol violation.</span></span> <span data-ttu-id="98aaf-143">例如，寫入多個述`content-length`。</span><span class="sxs-lookup"><span data-stu-id="98aaf-143">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="98aaf-144">可能會損毀的本文格式。</span><span class="sxs-lookup"><span data-stu-id="98aaf-144">May corrupt the body format.</span></span> <span data-ttu-id="98aaf-145">例如，HTML 頁尾寫入 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="98aaf-145">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="98aaf-146">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted)是有用的提示，指出如果在傳送標頭和/或本文寫入。</span><span class="sxs-lookup"><span data-stu-id="98aaf-146">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="98aaf-147">排序</span><span class="sxs-lookup"><span data-stu-id="98aaf-147">Ordering</span></span>

<span data-ttu-id="98aaf-148">順序中新增中介軟體元件`Configure`方法定義在要求中，會呼叫的順序和回應的相反順序。</span><span class="sxs-lookup"><span data-stu-id="98aaf-148">The order that middleware components are added in the `Configure` method defines the order in which they are invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="98aaf-149">此順序相當重要的安全性、 效能和功能。</span><span class="sxs-lookup"><span data-stu-id="98aaf-149">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="98aaf-150">Configure 方法 （如下所示） 會加入下列的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="98aaf-150">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="98aaf-151">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="98aaf-151">Exception/error handling</span></span>
2. <span data-ttu-id="98aaf-152">靜態檔案伺服器</span><span class="sxs-lookup"><span data-stu-id="98aaf-152">Static file server</span></span>
3. <span data-ttu-id="98aaf-153">驗證</span><span class="sxs-lookup"><span data-stu-id="98aaf-153">Authentication</span></span>
4. <span data-ttu-id="98aaf-154">MVC</span><span class="sxs-lookup"><span data-stu-id="98aaf-154">MVC</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseIdentity();                     // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

<span data-ttu-id="98aaf-155">在上述程式碼中`UseExceptionHandler`是第一個新增至管線的中介軟體元件，因此，它會攔截稍後呼叫中發生任何例外狀況。</span><span class="sxs-lookup"><span data-stu-id="98aaf-155">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="98aaf-156">靜態檔案中介軟體稱為及早在管線中，讓它可以處理要求，並最少運算無須經過剩餘的元件。</span><span class="sxs-lookup"><span data-stu-id="98aaf-156">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="98aaf-157">靜態檔案中介軟體提供**沒有**授權檢查。</span><span class="sxs-lookup"><span data-stu-id="98aaf-157">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="98aaf-158">任何檔案由提供服務，包括下*wwwroot*，都可以公開使用。</span><span class="sxs-lookup"><span data-stu-id="98aaf-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="98aaf-159">請參閱[靜態檔案處理](xref:fundamentals/static-files)如需安全靜態檔案的方法。</span><span class="sxs-lookup"><span data-stu-id="98aaf-159">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

<span data-ttu-id="98aaf-160">如果要求不會處理靜態檔案中介軟體，它會傳遞給身分識別的中介軟體 (`app.UseIdentity`)，它會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="98aaf-160">If the request is not handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="98aaf-161">身分識別不最少運算未經驗證的要求。</span><span class="sxs-lookup"><span data-stu-id="98aaf-161">Identity does not short-circuit unauthenticated requests.</span></span> <span data-ttu-id="98aaf-162">雖然身分識別進行驗證的要求時，授權 （及拒絕） 後才會 MVC 選取特定的控制器和動作。</span><span class="sxs-lookup"><span data-stu-id="98aaf-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

<span data-ttu-id="98aaf-163">下列範例會示範中介軟體訂購靜態檔案的要求處理前回應壓縮中介軟體的靜態檔案中介軟體的位置。</span><span class="sxs-lookup"><span data-stu-id="98aaf-163">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="98aaf-164">具有此中介軟體的順序，不會壓縮靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="98aaf-164">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="98aaf-165">MVC 回應[UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_)可以壓縮。</span><span class="sxs-lookup"><span data-stu-id="98aaf-165">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name=middleware-run-map-use></a>

### <a name="use-run-and-map"></a><span data-ttu-id="98aaf-166">使用、 執行和對應</span><span class="sxs-lookup"><span data-stu-id="98aaf-166">Use, Run, and Map</span></span>

<span data-ttu-id="98aaf-167">您可以設定 HTTP 管線使用`Use`， `Run`，和`Map`。</span><span class="sxs-lookup"><span data-stu-id="98aaf-167">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="98aaf-168">`Use`方法可以縮短管線 (亦即，如果它不會呼叫`next`要求委派)。</span><span class="sxs-lookup"><span data-stu-id="98aaf-168">The `Use` method can short-circuit the pipeline (that is, if it does not call a `next` request delegate).</span></span> <span data-ttu-id="98aaf-169">`Run`是一項慣例，以及一些中介軟體元件暴露`Run[Middleware]`在管線結尾處執行的方法。</span><span class="sxs-lookup"><span data-stu-id="98aaf-169">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="98aaf-170">`Map*`分支管線，依照慣例可用擴充功能。</span><span class="sxs-lookup"><span data-stu-id="98aaf-170">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="98aaf-171">[地圖](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)分支要求管線會根據給定的要求路徑的相符項目。</span><span class="sxs-lookup"><span data-stu-id="98aaf-171">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="98aaf-172">如果要求路徑的開頭指定的路徑，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="98aaf-172">If the request path starts with the given path, the branch is executed.</span></span>

<span data-ttu-id="98aaf-173">[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-173">[!code-csharp[Main](middleware/sample/Chain/StartupMap.cs?name=snippet1)]</span></span>

<span data-ttu-id="98aaf-174">下表顯示的要求和回應從`http://localhost:1234`使用先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="98aaf-174">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="98aaf-175">要求</span><span class="sxs-lookup"><span data-stu-id="98aaf-175">Request</span></span> | <span data-ttu-id="98aaf-176">回應</span><span class="sxs-lookup"><span data-stu-id="98aaf-176">Response</span></span> |
| --- | --- |
| <span data-ttu-id="98aaf-177">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="98aaf-177">localhost:1234</span></span> | <span data-ttu-id="98aaf-178">從非對應委派的 hello。</span><span class="sxs-lookup"><span data-stu-id="98aaf-178">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="98aaf-179">localhost:1234 / map1</span><span class="sxs-lookup"><span data-stu-id="98aaf-179">localhost:1234/map1</span></span> | <span data-ttu-id="98aaf-180">測試 1 對應</span><span class="sxs-lookup"><span data-stu-id="98aaf-180">Map Test 1</span></span> |
| <span data-ttu-id="98aaf-181">localhost:1234 / map2</span><span class="sxs-lookup"><span data-stu-id="98aaf-181">localhost:1234/map2</span></span> | <span data-ttu-id="98aaf-182">測試 2 對應</span><span class="sxs-lookup"><span data-stu-id="98aaf-182">Map Test 2</span></span> |
| <span data-ttu-id="98aaf-183">localhost:1234 / map3</span><span class="sxs-lookup"><span data-stu-id="98aaf-183">localhost:1234/map3</span></span> | <span data-ttu-id="98aaf-184">從非對應委派的 hello。</span><span class="sxs-lookup"><span data-stu-id="98aaf-184">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="98aaf-185">當`Map`是使用，相符的路徑區段已從`HttpRequest.Path`和附加至`HttpRequest.PathBase`針對每個要求。</span><span class="sxs-lookup"><span data-stu-id="98aaf-185">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="98aaf-186">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions)分支要求管線會根據給定的述詞的結果。</span><span class="sxs-lookup"><span data-stu-id="98aaf-186">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="98aaf-187">型別的任何述詞`Func<HttpContext, bool>`可用來將要求對應至新分支的管線。</span><span class="sxs-lookup"><span data-stu-id="98aaf-187">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="98aaf-188">在下列範例中，使用述詞來偵測是否存在的查詢字串變數`branch`:</span><span class="sxs-lookup"><span data-stu-id="98aaf-188">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

<span data-ttu-id="98aaf-189">[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-189">[!code-csharp[Main](middleware/sample/Chain/StartupMapWhen.cs?name=snippet1)]</span></span>

<span data-ttu-id="98aaf-190">下表顯示的要求和回應從`http://localhost:1234`使用先前的程式碼：</span><span class="sxs-lookup"><span data-stu-id="98aaf-190">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="98aaf-191">要求</span><span class="sxs-lookup"><span data-stu-id="98aaf-191">Request</span></span> | <span data-ttu-id="98aaf-192">回應</span><span class="sxs-lookup"><span data-stu-id="98aaf-192">Response</span></span> |
| --- | --- |
| <span data-ttu-id="98aaf-193">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="98aaf-193">localhost:1234</span></span> | <span data-ttu-id="98aaf-194">從非對應委派的 hello。</span><span class="sxs-lookup"><span data-stu-id="98aaf-194">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="98aaf-195">localhost:1234 /？ 分支 = master</span><span class="sxs-lookup"><span data-stu-id="98aaf-195">localhost:1234/?branch=master</span></span> | <span data-ttu-id="98aaf-196">使用分支 = master</span><span class="sxs-lookup"><span data-stu-id="98aaf-196">Branch used = master</span></span>|

<span data-ttu-id="98aaf-197">`Map`支援巢狀結構，例如：</span><span class="sxs-lookup"><span data-stu-id="98aaf-197">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
       level1App.Map("/level2a", level2AApp => {
           // "/level1/level2a"
           //...
       });
       level1App.Map("/level2b", level2BApp => {
           // "/level1/level2b"
           //...
       });
   });
   ```

<span data-ttu-id="98aaf-198">`Map`也可以符合多個區段同時發生，例如：</span><span class="sxs-lookup"><span data-stu-id="98aaf-198">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="98aaf-199">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="98aaf-199">Built-in middleware</span></span>

<span data-ttu-id="98aaf-200">ASP.NET Core 隨附下列的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="98aaf-200">ASP.NET Core ships with the following middleware components:</span></span>

| <span data-ttu-id="98aaf-201">中介軟體</span><span class="sxs-lookup"><span data-stu-id="98aaf-201">Middleware</span></span> | <span data-ttu-id="98aaf-202">說明</span><span class="sxs-lookup"><span data-stu-id="98aaf-202">Description</span></span> |
| ----- | ------- |
| [<span data-ttu-id="98aaf-203">驗證</span><span class="sxs-lookup"><span data-stu-id="98aaf-203">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="98aaf-204">提供的驗證支援。</span><span class="sxs-lookup"><span data-stu-id="98aaf-204">Provides authentication support.</span></span> |
| [<span data-ttu-id="98aaf-205">CORS</span><span class="sxs-lookup"><span data-stu-id="98aaf-205">CORS</span></span>](xref:security/cors) | <span data-ttu-id="98aaf-206">設定跨原始資源共用。</span><span class="sxs-lookup"><span data-stu-id="98aaf-206">Configures Cross-Origin Resource Sharing.</span></span> |
| [<span data-ttu-id="98aaf-207">回應快取</span><span class="sxs-lookup"><span data-stu-id="98aaf-207">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="98aaf-208">提供支援快取回應。</span><span class="sxs-lookup"><span data-stu-id="98aaf-208">Provides support for caching responses.</span></span> |
| [<span data-ttu-id="98aaf-209">回應的壓縮</span><span class="sxs-lookup"><span data-stu-id="98aaf-209">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="98aaf-210">提供支援壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="98aaf-210">Provides support for compressing responses.</span></span> |
| [<span data-ttu-id="98aaf-211">路由傳送</span><span class="sxs-lookup"><span data-stu-id="98aaf-211">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="98aaf-212">定義，並限制要求的路由。</span><span class="sxs-lookup"><span data-stu-id="98aaf-212">Defines and constrains request routes.</span></span> |
| [<span data-ttu-id="98aaf-213">工作階段</span><span class="sxs-lookup"><span data-stu-id="98aaf-213">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="98aaf-214">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="98aaf-214">Provides support for managing user sessions.</span></span> |
| [<span data-ttu-id="98aaf-215">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="98aaf-215">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="98aaf-216">提供靜態檔案和目錄瀏覽提供服務的支援。</span><span class="sxs-lookup"><span data-stu-id="98aaf-216">Provides support for serving static files and directory browsing.</span></span> |
| [<span data-ttu-id="98aaf-217">URL 重寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="98aaf-217">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="98aaf-218">提供重寫 Url，並將要求重新導向的支援。</span><span class="sxs-lookup"><span data-stu-id="98aaf-218">Provides support for rewriting URLs and redirecting requests.</span></span> |

<a name=middleware-writing-middleware></a>

## <a name="writing-middleware"></a><span data-ttu-id="98aaf-219">寫入中介軟體</span><span class="sxs-lookup"><span data-stu-id="98aaf-219">Writing middleware</span></span>

<span data-ttu-id="98aaf-220">中介軟體通常是封裝在類別和使用擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="98aaf-220">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="98aaf-221">請考慮下列的中介軟體會從查詢字串設定為目前要求的文化特性：</span><span class="sxs-lookup"><span data-stu-id="98aaf-221">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

<span data-ttu-id="98aaf-222">[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-222">[!code-csharp[Main](middleware/sample/Culture/StartupCulture.cs?name=snippet1)]</span></span>

<span data-ttu-id="98aaf-223">注意： 上述範例程式碼會示範建立中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="98aaf-223">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="98aaf-224">請參閱[全球化和當地語系化](xref:fundamentals/localization)ASP.NET Core 內建的當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="98aaf-224">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="98aaf-225">您可以藉由傳遞文化特性，例如測試中介軟體`http://localhost:7997/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="98aaf-225">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="98aaf-226">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="98aaf-226">The following code moves the middleware delegate to a class:</span></span>

<span data-ttu-id="98aaf-227">[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-227">[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddleware.cs)]</span></span>

<span data-ttu-id="98aaf-228">下列擴充方法會公開透過中的介軟體[IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span><span class="sxs-lookup"><span data-stu-id="98aaf-228">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

<span data-ttu-id="98aaf-229">[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-229">[!code-csharp[Main](middleware/sample/Culture/RequestCultureMiddlewareExtensions.cs)]</span></span>

<span data-ttu-id="98aaf-230">下列程式碼呼叫的中介軟體從`Configure`:</span><span class="sxs-lookup"><span data-stu-id="98aaf-230">The following code calls the middleware from `Configure`:</span></span>

<span data-ttu-id="98aaf-231">[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="98aaf-231">[!code-csharp[Main](middleware/sample/Culture/Startup.cs?name=snippet1&highlight=5)]</span></span>

<span data-ttu-id="98aaf-232">中介軟體應該遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)藉由公開在其建構函式及其相依性。</span><span class="sxs-lookup"><span data-stu-id="98aaf-232">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="98aaf-233">中介軟體建構一次，每個*應用程式存留期*。</span><span class="sxs-lookup"><span data-stu-id="98aaf-233">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="98aaf-234">請參閱*每個要求的相依性*下方如果您需要與中介軟體要求內的共用服務。</span><span class="sxs-lookup"><span data-stu-id="98aaf-234">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="98aaf-235">中介軟體元件可以解決其相依性的相依性插入透過建構函式的參數。</span><span class="sxs-lookup"><span data-stu-id="98aaf-235">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="98aaf-236">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary)也可以接受其他參數直接。</span><span class="sxs-lookup"><span data-stu-id="98aaf-236">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="98aaf-237">每個要求的相依性</span><span class="sxs-lookup"><span data-stu-id="98aaf-237">Per-request dependencies</span></span>

<span data-ttu-id="98aaf-238">中介軟體建構應用程式啟動時，無法個別要求，因為*範圍*中介軟體建構函式所使用的存留時間服務不會與其他相依性插入類型共用期間每個要求。</span><span class="sxs-lookup"><span data-stu-id="98aaf-238">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="98aaf-239">如果您必須共用*範圍*服務中介軟體和其他類型之間，加入這些服務可`Invoke`方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="98aaf-239">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="98aaf-240">`Invoke`方法可以接受其他參數會填入相依性插入。</span><span class="sxs-lookup"><span data-stu-id="98aaf-240">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="98aaf-241">例如: </span><span class="sxs-lookup"><span data-stu-id="98aaf-241">For example:</span></span>

```c#
public class MyMiddleware
{
    private readonly RequestDelegate _next;

    public MyMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="resources"></a><span data-ttu-id="98aaf-242">資源</span><span class="sxs-lookup"><span data-stu-id="98aaf-242">Resources</span></span>

* [<span data-ttu-id="98aaf-243">此文件中使用的範例程式碼</span><span class="sxs-lookup"><span data-stu-id="98aaf-243">Sample code used in this doc</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/sample)
* [<span data-ttu-id="98aaf-244">將 HTTP 模組移轉至中介軟體</span><span class="sxs-lookup"><span data-stu-id="98aaf-244">Migrating HTTP Modules to Middleware</span></span>](../migration/http-modules.md)
* [<span data-ttu-id="98aaf-245">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="98aaf-245">Application Startup</span></span>](startup.md)
* [<span data-ttu-id="98aaf-246">要求的功能</span><span class="sxs-lookup"><span data-stu-id="98aaf-246">Request Features</span></span>](request-features.md)
