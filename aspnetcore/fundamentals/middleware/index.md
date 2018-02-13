---
title: "ASP.NET Core 中介軟體"
author: rick-anderson
description: "了解 ASP.NET Core 中介軟體和要求管線。"
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 158f11875f22f8f9dba6f7f109123717b9da8d18
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="de6e7-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="de6e7-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="de6e7-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="de6e7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="de6e7-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="de6e7-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="de6e7-106">什麼是中介軟體？</span><span class="sxs-lookup"><span data-stu-id="de6e7-106">What is middleware?</span></span>

<span data-ttu-id="de6e7-107">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="de6e7-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="de6e7-108">每個元件皆：</span><span class="sxs-lookup"><span data-stu-id="de6e7-108">Each component:</span></span>

* <span data-ttu-id="de6e7-109">選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="de6e7-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="de6e7-110">可以在叫用管線中下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="de6e7-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="de6e7-111">要求委派用於建置要求管線。</span><span class="sxs-lookup"><span data-stu-id="de6e7-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="de6e7-112">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="de6e7-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="de6e7-113">要求委派的設定方式為 [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) 和 [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="de6e7-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="de6e7-114">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或可於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="de6e7-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="de6e7-115">這些可重複使用的類別及內嵌匿名方法皆為*中介軟體*，*或中介軟體元件*。</span><span class="sxs-lookup"><span data-stu-id="de6e7-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="de6e7-116">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="de6e7-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="de6e7-117">[將 HTTP 模組移轉至中介軟體](xref:migration/http-modules)說明 ASP.NET Core 和 ASP.NET 4.x 中的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="de6e7-117">[Migrating HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="de6e7-118">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="de6e7-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="de6e7-119">ASP.NET Core 要求管線包含一連串的要求委派，會接連呼叫下個委派，如圖表所示 (執行緒遵循著黑色箭頭)：</span><span class="sxs-lookup"><span data-stu-id="de6e7-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![要求處理模式說明要求的抵達、透過三個中介軟體處理及離開應用程式的回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="de6e7-123">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="de6e7-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="de6e7-124">委派也可決定不將要求傳遞到下個委派，這就是所謂對要求管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="de6e7-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="de6e7-125">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="de6e7-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="de6e7-126">例如，靜態檔案中介軟體可傳回靜態檔案的要求，並對剩餘管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="de6e7-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="de6e7-127">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="de6e7-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="de6e7-128">ASP.NET Core 應用程式設定處理所有要求的單一要求委派，將作業盡可能地簡化。</span><span class="sxs-lookup"><span data-stu-id="de6e7-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="de6e7-129">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="de6e7-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="de6e7-130">反而會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="de6e7-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[Main](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="de6e7-131">第一個 [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="de6e7-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="de6e7-132">您可以使用 [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions)將多個要求委派鏈結在一起。</span><span class="sxs-lookup"><span data-stu-id="de6e7-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="de6e7-133">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="de6e7-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="de6e7-134">(請記得，您可以*不*呼叫*下個*參數來對管線執行最少運算。)您通常可以在下個委派的前後執行動作，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="de6e7-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[Main](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="de6e7-135">請勿在回應傳送至用戶端後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="de6e7-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="de6e7-136">回應啟動後，變更 `HttpResponse` 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="de6e7-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="de6e7-137">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="de6e7-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="de6e7-138">在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="de6e7-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="de6e7-139">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="de6e7-139">May cause a protocol violation.</span></span> <span data-ttu-id="de6e7-140">例如，寫入超過指定的 `content-length`。</span><span class="sxs-lookup"><span data-stu-id="de6e7-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="de6e7-141">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="de6e7-141">May corrupt the body format.</span></span> <span data-ttu-id="de6e7-142">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="de6e7-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="de6e7-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) 是實用的提示，可表示是否已傳送標頭與 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="de6e7-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="de6e7-144">排序</span><span class="sxs-lookup"><span data-stu-id="de6e7-144">Ordering</span></span>

<span data-ttu-id="de6e7-145">中介軟體在 `Configure` 方法中的新增順序，定義了要求時叫用中介軟體的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="de6e7-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="de6e7-146">對安全性、效能與功能性而言，此順序相當重要。</span><span class="sxs-lookup"><span data-stu-id="de6e7-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="de6e7-147">Configure 方法 (如下所示) 新增了下列中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="de6e7-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="de6e7-148">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="de6e7-148">Exception/error handling</span></span>
2. <span data-ttu-id="de6e7-149">靜態檔案伺服器</span><span class="sxs-lookup"><span data-stu-id="de6e7-149">Static file server</span></span>
3. <span data-ttu-id="de6e7-150">驗證</span><span class="sxs-lookup"><span data-stu-id="de6e7-150">Authentication</span></span>
4. <span data-ttu-id="de6e7-151">MVC</span><span class="sxs-lookup"><span data-stu-id="de6e7-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="de6e7-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="de6e7-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseExceptionHandler("/Home/Error"); // Call first to catch exceptions
                                            // thrown in the following middleware.

    app.UseStaticFiles();                   // Return static files and end pipeline.

    app.UseAuthentication();               // Authenticate before you access
                                           // secure resources.

    app.UseMvcWithDefaultRoute();          // Add MVC to the request pipeline.
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="de6e7-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="de6e7-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

-----------

<span data-ttu-id="de6e7-154">上述程式碼中，`UseExceptionHandler` 是第一個新增至管線的中介軟體元件，因此其會與後續呼叫所發生的所有例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="de6e7-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="de6e7-155">靜態檔案中介軟體會提早在管線中呼叫，以便其可處理要求並執行最少運算，而無需一一處理剩餘的元件。</span><span class="sxs-lookup"><span data-stu-id="de6e7-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="de6e7-156">靜態檔案中介軟體提供**免**授權檢查。</span><span class="sxs-lookup"><span data-stu-id="de6e7-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="de6e7-157">其提供的所有檔案，包括在 *wwwroot* 下的那些檔案，皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="de6e7-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="de6e7-158">如需保護靜態檔案的方法，請參閱[使用靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="de6e7-158">See [Working with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="de6e7-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="de6e7-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="de6e7-160">若要求未經靜態檔案中介軟體處理，則會傳遞至身分識別中介軟體 (`app.UseAuthentication`)，其會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="de6e7-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="de6e7-161">身分識別不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="de6e7-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="de6e7-162">雖然身分識別會驗證要求，但驗證 (與拒絕) 只在 MVC 選取特定 Razor 頁面或控制器及動作後才發生。</span><span class="sxs-lookup"><span data-stu-id="de6e7-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="de6e7-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="de6e7-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="de6e7-164">若要求未經靜態檔案中介軟體處理，則會傳遞至身分識別中介軟體 (`app.UseIdentity`)，其會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="de6e7-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="de6e7-165">身分識別不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="de6e7-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="de6e7-166">雖然身分識別會驗證要求，但驗證 (與拒絕) 只在 MVC 選取特定 Razor 頁面或控制器及動作後才發生。</span><span class="sxs-lookup"><span data-stu-id="de6e7-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="de6e7-167">下列範例會示範中介軟體順序，其中靜態檔案中介軟體在回應壓縮中介軟體前處理靜態檔案的要求。</span><span class="sxs-lookup"><span data-stu-id="de6e7-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="de6e7-168">此中介軟體的順序不壓縮靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="de6e7-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="de6e7-169">可壓縮來自 [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 的 MVC 回應。</span><span class="sxs-lookup"><span data-stu-id="de6e7-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseStaticFiles();         // Static files not compressed
                                  // by middleware.
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

<a name="middleware-run-map-use"></a>

### <a name="use-run-and-map"></a><span data-ttu-id="de6e7-170">使用、執行與對應</span><span class="sxs-lookup"><span data-stu-id="de6e7-170">Use, Run, and Map</span></span>

<span data-ttu-id="de6e7-171">您可以使用 `Use`、`Run` 及 `Map` 設定 HTTP 管線。</span><span class="sxs-lookup"><span data-stu-id="de6e7-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="de6e7-172">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派)。</span><span class="sxs-lookup"><span data-stu-id="de6e7-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="de6e7-173">`Run` 為慣例，且有些中介軟體元件可能公開執行在管線尾端的 `Run[Middleware]` 方法。</span><span class="sxs-lookup"><span data-stu-id="de6e7-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="de6e7-174">`Map*` 擴充方法則用作分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="de6e7-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="de6e7-175">[對應](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions)會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="de6e7-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="de6e7-176">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="de6e7-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="de6e7-177">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="de6e7-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="de6e7-178">要求</span><span class="sxs-lookup"><span data-stu-id="de6e7-178">Request</span></span> | <span data-ttu-id="de6e7-179">回應</span><span class="sxs-lookup"><span data-stu-id="de6e7-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="de6e7-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="de6e7-180">localhost:1234</span></span> | <span data-ttu-id="de6e7-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="de6e7-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="de6e7-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="de6e7-182">localhost:1234/map1</span></span> | <span data-ttu-id="de6e7-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="de6e7-183">Map Test 1</span></span> |
| <span data-ttu-id="de6e7-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="de6e7-184">localhost:1234/map2</span></span> | <span data-ttu-id="de6e7-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="de6e7-185">Map Test 2</span></span> |
| <span data-ttu-id="de6e7-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="de6e7-186">localhost:1234/map3</span></span> | <span data-ttu-id="de6e7-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="de6e7-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="de6e7-188">使用 `Map` 時，相符的路徑線段會從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="de6e7-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="de6e7-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) 會依據指定述詞的結果分支要求管線。</span><span class="sxs-lookup"><span data-stu-id="de6e7-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="de6e7-190">類型 `Func<HttpContext, bool>` 的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="de6e7-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="de6e7-191">在下列範例中，述詞用於偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="de6e7-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[Main](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="de6e7-192">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="de6e7-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="de6e7-193">要求</span><span class="sxs-lookup"><span data-stu-id="de6e7-193">Request</span></span> | <span data-ttu-id="de6e7-194">回應</span><span class="sxs-lookup"><span data-stu-id="de6e7-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="de6e7-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="de6e7-195">localhost:1234</span></span> | <span data-ttu-id="de6e7-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="de6e7-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="de6e7-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="de6e7-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="de6e7-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="de6e7-198">Branch used = master</span></span>|

<span data-ttu-id="de6e7-199">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="de6e7-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="de6e7-200">`Map` 也可以一次對應多個線段，例如：</span><span class="sxs-lookup"><span data-stu-id="de6e7-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="de6e7-201">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="de6e7-201">Built-in middleware</span></span>

<span data-ttu-id="de6e7-202">ASP.NET Core 隨附下列中介軟體元件，及其應新增之順序的說明：</span><span class="sxs-lookup"><span data-stu-id="de6e7-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="de6e7-203">中介軟體</span><span class="sxs-lookup"><span data-stu-id="de6e7-203">Middleware</span></span> | <span data-ttu-id="de6e7-204">描述</span><span class="sxs-lookup"><span data-stu-id="de6e7-204">Description</span></span> | <span data-ttu-id="de6e7-205">訂單</span><span class="sxs-lookup"><span data-stu-id="de6e7-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="de6e7-206">驗證</span><span class="sxs-lookup"><span data-stu-id="de6e7-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="de6e7-207">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-207">Provides authentication support.</span></span> | <span data-ttu-id="de6e7-208">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="de6e7-209">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="de6e7-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="de6e7-210">CORS</span><span class="sxs-lookup"><span data-stu-id="de6e7-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="de6e7-211">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="de6e7-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="de6e7-212">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="de6e7-213">診斷</span><span class="sxs-lookup"><span data-stu-id="de6e7-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="de6e7-214">設定診斷。</span><span class="sxs-lookup"><span data-stu-id="de6e7-214">Configures diagnostics.</span></span> | <span data-ttu-id="de6e7-215">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="de6e7-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="de6e7-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="de6e7-217">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="de6e7-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="de6e7-218">在使用更新欄位的元件之前 (例如：配置、主機、ClientIP、方法)。</span><span class="sxs-lookup"><span data-stu-id="de6e7-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="de6e7-219">回應快取</span><span class="sxs-lookup"><span data-stu-id="de6e7-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="de6e7-220">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-220">Provides support for caching responses.</span></span> | <span data-ttu-id="de6e7-221">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="de6e7-222">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="de6e7-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="de6e7-223">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="de6e7-224">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="de6e7-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="de6e7-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="de6e7-226">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-226">Provides localization support.</span></span> | <span data-ttu-id="de6e7-227">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="de6e7-228">路由傳送</span><span class="sxs-lookup"><span data-stu-id="de6e7-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="de6e7-229">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="de6e7-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="de6e7-230">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="de6e7-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="de6e7-231">工作階段</span><span class="sxs-lookup"><span data-stu-id="de6e7-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="de6e7-232">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="de6e7-233">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="de6e7-234">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="de6e7-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="de6e7-235">提供處理靜態檔案和目錄瀏覽的支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="de6e7-236">若要求符合檔案則為終端機。</span><span class="sxs-lookup"><span data-stu-id="de6e7-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="de6e7-237">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="de6e7-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="de6e7-238">提供重寫 URL及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="de6e7-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="de6e7-239">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="de6e7-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="de6e7-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="de6e7-241">允許 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="de6e7-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="de6e7-242">在必須接受 WebSocket 要求的元件之前。</span><span class="sxs-lookup"><span data-stu-id="de6e7-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="de6e7-243">寫入中介軟體</span><span class="sxs-lookup"><span data-stu-id="de6e7-243">Writing middleware</span></span>

<span data-ttu-id="de6e7-244">中介軟體通常封裝在類別中，並以擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="de6e7-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="de6e7-245">請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="de6e7-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[Main](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="de6e7-246">請注意：上述範例程式碼用於示範中介軟體元件的建立。</span><span class="sxs-lookup"><span data-stu-id="de6e7-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="de6e7-247">如需 ASP.NET Core 的內建當地語系化支援，請參閱[全球化與當地語系化](xref:fundamentals/localization)。</span><span class="sxs-lookup"><span data-stu-id="de6e7-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="de6e7-248">您可藉由傳遞文化特性來測試中介軟體，例如 `http://localhost:7997/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="de6e7-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="de6e7-249">下列程式碼將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="de6e7-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddleware.cs)]

<span data-ttu-id="de6e7-250">下列擴充方法透過 [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) 公開中介軟體：</span><span class="sxs-lookup"><span data-stu-id="de6e7-250">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[Main](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="de6e7-251">下列程式碼從 `Configure` 呼叫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="de6e7-251">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[Main](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="de6e7-252">中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="de6e7-252">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="de6e7-253">中介軟體每次*應用程式存留期*就建構一次。</span><span class="sxs-lookup"><span data-stu-id="de6e7-253">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="de6e7-254">若您需要在要求內與中介軟體共用服務，請參閱*依要求的相依性*。</span><span class="sxs-lookup"><span data-stu-id="de6e7-254">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="de6e7-255">中介軟體元件可透過建構函式參數，解析其來自相依性插入的相依性。</span><span class="sxs-lookup"><span data-stu-id="de6e7-255">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="de6e7-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) 也可直接接受其它參數。</span><span class="sxs-lookup"><span data-stu-id="de6e7-256">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="de6e7-257">依要求的相依性</span><span class="sxs-lookup"><span data-stu-id="de6e7-257">Per-request dependencies</span></span>

<span data-ttu-id="de6e7-258">因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的*已限定範圍*存留期服務不會與其它插入相依性的類型共用。</span><span class="sxs-lookup"><span data-stu-id="de6e7-258">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="de6e7-259">如果您必須在中介軟體和其他類型間共用*已限定範圍*的服務，請將這些服務新增至 `Invoke` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="de6e7-259">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="de6e7-260">`Invoke` 方法可以接受相依性插入所填入的其他參數。</span><span class="sxs-lookup"><span data-stu-id="de6e7-260">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="de6e7-261">例如: </span><span class="sxs-lookup"><span data-stu-id="de6e7-261">For example:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="de6e7-262">其他資源</span><span class="sxs-lookup"><span data-stu-id="de6e7-262">Additional resources</span></span>

* [<span data-ttu-id="de6e7-263">將 HTTP 模組移轉至中介軟體</span><span class="sxs-lookup"><span data-stu-id="de6e7-263">Migrating HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="de6e7-264">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="de6e7-264">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="de6e7-265">要求的功能</span><span class="sxs-lookup"><span data-stu-id="de6e7-265">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="de6e7-266">Factory 式中介軟體啟用</span><span class="sxs-lookup"><span data-stu-id="de6e7-266">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="de6e7-267">Factory 式中介軟體啟用與協力廠商容器</span><span class="sxs-lookup"><span data-stu-id="de6e7-267">Factory-based middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
