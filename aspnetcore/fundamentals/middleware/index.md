---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
manager: wpickett
ms.author: riande
ms.date: 01/22/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/middleware/index
ms.openlocfilehash: 3312b27f936340a73243224c1a716fe421f178bc
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="970b4-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="970b4-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="970b4-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="970b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="970b4-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="970b4-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/middleware/index/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="what-is-middleware"></a><span data-ttu-id="970b4-106">什麼是中介軟體？</span><span class="sxs-lookup"><span data-stu-id="970b4-106">What is middleware?</span></span>

<span data-ttu-id="970b4-107">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="970b4-107">Middleware is software that's assembled into an application pipeline to handle requests and responses.</span></span> <span data-ttu-id="970b4-108">每個元件：</span><span class="sxs-lookup"><span data-stu-id="970b4-108">Each component:</span></span>

* <span data-ttu-id="970b4-109">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="970b4-109">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="970b4-110">可以在叫用管線中下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="970b4-110">Can perform work before and after the next component in the pipeline is invoked.</span></span> 

<span data-ttu-id="970b4-111">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="970b4-111">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="970b4-112">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="970b4-112">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="970b4-113">要求委派的設定方式為使用 [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions)、[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) 和 [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="970b4-113">Request delegates are configured using [Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions), [Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions), and [Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) extension methods.</span></span> <span data-ttu-id="970b4-114">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="970b4-114">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="970b4-115">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，或「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="970b4-115">These reusable classes and in-line anonymous methods are *middleware*, or *middleware components*.</span></span> <span data-ttu-id="970b4-116">要求管線中的每個中介軟體元件負責叫用管線中的下一個元件，或於適當時，對鏈結執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="970b4-116">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline, or short-circuiting the chain if appropriate.</span></span>

<span data-ttu-id="970b4-117">[將 HTTP 模組遷移至中介軟體](xref:migration/http-modules)說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="970b4-117">[Migrate HTTP Modules to Middleware](xref:migration/http-modules) explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="creating-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="970b4-118">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="970b4-118">Creating a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="970b4-119">ASP.NET Core 要求管線包含一連串的要求委派，而系統會對其逐一接連呼叫，如圖表所示 (執行緒遵循著黑色箭頭)：</span><span class="sxs-lookup"><span data-stu-id="970b4-119">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other, as this diagram shows (the thread of execution follows the black arrows):</span></span>

![要求處理模式說明要求的抵達、透過三個中介軟體進行處理，以及離開應用程式的回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="970b4-123">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="970b4-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="970b4-124">委派也可決定不將要求傳遞到下個委派，這就是所謂對要求管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="970b4-124">A delegate can also decide to not pass a request to the next delegate, which is called short-circuiting the request pipeline.</span></span> <span data-ttu-id="970b4-125">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="970b4-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="970b4-126">例如，靜態檔案中介軟體可傳回靜態檔案的要求，並對剩餘管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="970b4-126">For example, the static file middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="970b4-127">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="970b4-127">Exception-handling delegates need to be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="970b4-128">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="970b4-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="970b4-129">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="970b4-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="970b4-130">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="970b4-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/sample/Middleware/Startup.cs)]

<span data-ttu-id="970b4-131">第一個 [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="970b4-131">The first [app.Run](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.runextensions) delegate terminates the pipeline.</span></span>

<span data-ttu-id="970b4-132">您可以使用 [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions) 將多個要求委派鏈結在一起。</span><span class="sxs-lookup"><span data-stu-id="970b4-132">You can chain multiple request delegates together with [app.Use](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.useextensions).</span></span> <span data-ttu-id="970b4-133">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="970b4-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="970b4-134">(請記得，您可以*不*呼叫*下個*參數來對管線執行最少運算。)您通常可以在下個委派的前後執行動作，如此範例所示：</span><span class="sxs-lookup"><span data-stu-id="970b4-134">(Remember that you can short-circuit the pipeline by *not* calling the *next* parameter.) You can typically perform actions both before and after the next delegate, as this example demonstrates:</span></span>

[!code-csharp[](index/sample/Chain/Startup.cs?name=snippet1)]

>[!WARNING]
> <span data-ttu-id="970b4-135">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="970b4-135">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="970b4-136">回應啟動後，變更 `HttpResponse` 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="970b4-136">Changes to `HttpResponse` after the response has started will throw an exception.</span></span> <span data-ttu-id="970b4-137">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="970b4-137">For example, changes such as setting headers, status code, etc,  will throw an exception.</span></span> <span data-ttu-id="970b4-138">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="970b4-138">Writing to the response body after calling `next`:</span></span>
> - <span data-ttu-id="970b4-139">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="970b4-139">May cause a protocol violation.</span></span> <span data-ttu-id="970b4-140">例如，寫入超過指定 `content-length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="970b4-140">For example, writing more than the stated `content-length`.</span></span>
> - <span data-ttu-id="970b4-141">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="970b4-141">May corrupt the body format.</span></span> <span data-ttu-id="970b4-142">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="970b4-142">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="970b4-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) 是實用的提示，可表示是否已傳送標頭與 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="970b4-143">[HttpResponse.HasStarted](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.httpresponsefeature#Microsoft_AspNetCore_Http_Features_HttpResponseFeature_HasStarted) is a useful hint to indicate if headers have been sent and/or the body has been written to.</span></span>

## <a name="ordering"></a><span data-ttu-id="970b4-144">排序</span><span class="sxs-lookup"><span data-stu-id="970b4-144">Ordering</span></span>

<span data-ttu-id="970b4-145">`Configure` 方法新增中介軟體元件的順序，定義了要求時叫用中介軟體的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="970b4-145">The order that middleware components are added in the `Configure` method defines the order in which they're invoked on requests, and the reverse order for the response.</span></span> <span data-ttu-id="970b4-146">對安全性、效能與功能性而言，此順序相當重要。</span><span class="sxs-lookup"><span data-stu-id="970b4-146">This ordering is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="970b4-147">Configure 方法 (如下所示) 新增了下列中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="970b4-147">The Configure method (shown below) adds the following middleware components:</span></span>

1. <span data-ttu-id="970b4-148">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="970b4-148">Exception/error handling</span></span>
2. <span data-ttu-id="970b4-149">靜態檔案伺服器</span><span class="sxs-lookup"><span data-stu-id="970b4-149">Static file server</span></span>
3. <span data-ttu-id="970b4-150">驗證</span><span class="sxs-lookup"><span data-stu-id="970b4-150">Authentication</span></span>
4. <span data-ttu-id="970b4-151">MVC</span><span class="sxs-lookup"><span data-stu-id="970b4-151">MVC</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="970b4-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="970b4-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="970b4-153">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="970b4-153">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="970b4-154">上述程式碼中，`UseExceptionHandler` 是第一個新增至管線的中介軟體元件，因此其會與後續呼叫所發生的所有例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="970b4-154">In the code above, `UseExceptionHandler` is the first middleware component added to the pipeline—therefore, it catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="970b4-155">系統會提早在管線中呼叫靜態檔案中介軟體，以便其處理要求並執行最少運算，而無需一一處理剩餘的元件。</span><span class="sxs-lookup"><span data-stu-id="970b4-155">The static file middleware is called early in the pipeline so it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="970b4-156">靜態檔案中介軟體**不提供**授權檢查。</span><span class="sxs-lookup"><span data-stu-id="970b4-156">The static file middleware provides **no** authorization checks.</span></span> <span data-ttu-id="970b4-157">其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="970b4-157">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="970b4-158">如需保護靜態檔案的方法，請參閱[使用靜態檔案](xref:fundamentals/static-files)。</span><span class="sxs-lookup"><span data-stu-id="970b4-158">See [Work with static files](xref:fundamentals/static-files) for an approach to secure static files.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="970b4-159">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="970b4-159">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)


<span data-ttu-id="970b4-160">若要求未經靜態檔案中介軟體處理，則會將其傳遞至身分識別中介軟體 (`app.UseAuthentication`)，以執行驗證。</span><span class="sxs-lookup"><span data-stu-id="970b4-160">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseAuthentication`), which performs authentication.</span></span> <span data-ttu-id="970b4-161">身分識別不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="970b4-161">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="970b4-162">雖然身分識別會驗證要求，但只有在 MVC 選取特定 Razor 頁面或控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="970b4-162">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific Razor Page or controller and action.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="970b4-163">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="970b4-163">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="970b4-164">若要求未經靜態檔案中介軟體處理，則會將其傳遞至身分識別中介軟體 (`app.UseIdentity`)，以執行驗證。</span><span class="sxs-lookup"><span data-stu-id="970b4-164">If the request isn't handled by the static file middleware, it's passed on to the Identity middleware (`app.UseIdentity`), which performs authentication.</span></span> <span data-ttu-id="970b4-165">身分識別不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="970b4-165">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="970b4-166">雖然身分識別會驗證要求，但只有在 MVC 選取特定控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="970b4-166">Although Identity authenticates requests,  authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

-----------

<span data-ttu-id="970b4-167">下列範例會示範中介軟體的順序，其中靜態檔案中介軟體會在回應壓縮中介軟體之前，先處理靜態檔案的要求。</span><span class="sxs-lookup"><span data-stu-id="970b4-167">The following example demonstrates a middleware ordering where requests for static files are handled by the static file middleware before the response compression middleware.</span></span> <span data-ttu-id="970b4-168">在這種中介軟體的順序下，不會壓縮靜態檔案，</span><span class="sxs-lookup"><span data-stu-id="970b4-168">Static files are not compressed with this ordering of the middleware.</span></span> <span data-ttu-id="970b4-169">而會壓縮來自 [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 的 MVC 回應。</span><span class="sxs-lookup"><span data-stu-id="970b4-169">The MVC responses from [UseMvcWithDefaultRoute](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mvcapplicationbuilderextensions#Microsoft_AspNetCore_Builder_MvcApplicationBuilderExtensions_UseMvcWithDefaultRoute_Microsoft_AspNetCore_Builder_IApplicationBuilder_) can be compressed.</span></span>

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

### <a name="use-run-and-map"></a><span data-ttu-id="970b4-170">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="970b4-170">Use, Run, and Map</span></span>

<span data-ttu-id="970b4-171">您可以使用 `Use`、`Run` 及 `Map` 設定 HTTP 管線。</span><span class="sxs-lookup"><span data-stu-id="970b4-171">You configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="970b4-172">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="970b4-172">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="970b4-173">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="970b4-173">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="970b4-174">`Map*` 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="970b4-174">`Map*` extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="970b4-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="970b4-175">[Map](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapextensions) branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="970b4-176">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="970b4-176">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/sample/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="970b4-177">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="970b4-177">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="970b4-178">要求</span><span class="sxs-lookup"><span data-stu-id="970b4-178">Request</span></span> | <span data-ttu-id="970b4-179">回應</span><span class="sxs-lookup"><span data-stu-id="970b4-179">Response</span></span> |
| --- | --- |
| <span data-ttu-id="970b4-180">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="970b4-180">localhost:1234</span></span> | <span data-ttu-id="970b4-181">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="970b4-181">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="970b4-182">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="970b4-182">localhost:1234/map1</span></span> | <span data-ttu-id="970b4-183">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="970b4-183">Map Test 1</span></span> |
| <span data-ttu-id="970b4-184">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="970b4-184">localhost:1234/map2</span></span> | <span data-ttu-id="970b4-185">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="970b4-185">Map Test 2</span></span> |
| <span data-ttu-id="970b4-186">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="970b4-186">localhost:1234/map3</span></span> | <span data-ttu-id="970b4-187">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="970b4-187">Hello from non-Map delegate.</span></span>  |

<span data-ttu-id="970b4-188">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="970b4-188">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="970b4-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="970b4-189">[MapWhen](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="970b4-190">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="970b4-190">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="970b4-191">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="970b4-191">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/sample/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="970b4-192">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="970b4-192">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="970b4-193">要求</span><span class="sxs-lookup"><span data-stu-id="970b4-193">Request</span></span> | <span data-ttu-id="970b4-194">回應</span><span class="sxs-lookup"><span data-stu-id="970b4-194">Response</span></span> |
| --- | --- |
| <span data-ttu-id="970b4-195">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="970b4-195">localhost:1234</span></span> | <span data-ttu-id="970b4-196">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="970b4-196">Hello from non-Map delegate.</span></span>  |
| <span data-ttu-id="970b4-197">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="970b4-197">localhost:1234/?branch=master</span></span> | <span data-ttu-id="970b4-198">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="970b4-198">Branch used = master</span></span>|

<span data-ttu-id="970b4-199">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="970b4-199">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="970b4-200">`Map` 也可以一次比對多個線段，例如：</span><span class="sxs-lookup"><span data-stu-id="970b4-200">`Map` can also match multiple segments at once, for example:</span></span>

 ```csharp
app.Map("/level1/level2", HandleMultiSeg);
```

## <a name="built-in-middleware"></a><span data-ttu-id="970b4-201">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="970b4-201">Built-in middleware</span></span>

<span data-ttu-id="970b4-202">ASP.NET Core 隨附下列中介軟體元件，以及新增這些元件時必須按照的順序說明：</span><span class="sxs-lookup"><span data-stu-id="970b4-202">ASP.NET Core ships with the following middleware components, as well as a description of the order in which they should be added:</span></span>

| <span data-ttu-id="970b4-203">中介軟體</span><span class="sxs-lookup"><span data-stu-id="970b4-203">Middleware</span></span> | <span data-ttu-id="970b4-204">描述</span><span class="sxs-lookup"><span data-stu-id="970b4-204">Description</span></span> | <span data-ttu-id="970b4-205">訂單</span><span class="sxs-lookup"><span data-stu-id="970b4-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="970b4-206">驗證</span><span class="sxs-lookup"><span data-stu-id="970b4-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="970b4-207">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="970b4-207">Provides authentication support.</span></span> | <span data-ttu-id="970b4-208">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="970b4-209">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="970b4-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="970b4-210">CORS</span><span class="sxs-lookup"><span data-stu-id="970b4-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="970b4-211">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="970b4-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="970b4-212">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="970b4-213">診斷</span><span class="sxs-lookup"><span data-stu-id="970b4-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="970b4-214">設定診斷。</span><span class="sxs-lookup"><span data-stu-id="970b4-214">Configures diagnostics.</span></span> | <span data-ttu-id="970b4-215">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="970b4-216">ForwardedHeaders/HttpOverrides</span><span class="sxs-lookup"><span data-stu-id="970b4-216">ForwardedHeaders/HttpOverrides</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="970b4-217">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="970b4-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="970b4-218">在使用更新欄位的元件之前 (例如：配置、主機、ClientIP、方法)。</span><span class="sxs-lookup"><span data-stu-id="970b4-218">Before components that consume the updated fields (examples: Scheme, Host, ClientIP, Method).</span></span> |
| [<span data-ttu-id="970b4-219">回應快取</span><span class="sxs-lookup"><span data-stu-id="970b4-219">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="970b4-220">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="970b4-220">Provides support for caching responses.</span></span> | <span data-ttu-id="970b4-221">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-221">Before components that require caching.</span></span> |
| [<span data-ttu-id="970b4-222">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="970b4-222">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="970b4-223">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="970b4-223">Provides support for compressing responses.</span></span> | <span data-ttu-id="970b4-224">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-224">Before components that require compression.</span></span> |
| [<span data-ttu-id="970b4-225">RequestLocalization</span><span class="sxs-lookup"><span data-stu-id="970b4-225">RequestLocalization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="970b4-226">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="970b4-226">Provides localization support.</span></span> | <span data-ttu-id="970b4-227">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-227">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="970b4-228">路由傳送</span><span class="sxs-lookup"><span data-stu-id="970b4-228">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="970b4-229">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="970b4-229">Defines and constrains request routes.</span></span> | <span data-ttu-id="970b4-230">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="970b4-230">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="970b4-231">工作階段</span><span class="sxs-lookup"><span data-stu-id="970b4-231">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="970b4-232">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="970b4-232">Provides support for managing user sessions.</span></span> | <span data-ttu-id="970b4-233">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-233">Before components that require Session.</span></span> |
| [<span data-ttu-id="970b4-234">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="970b4-234">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="970b4-235">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="970b4-235">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="970b4-236">若要求符合檔案則為終端機。</span><span class="sxs-lookup"><span data-stu-id="970b4-236">Terminal if a request matches files.</span></span> |
| [<span data-ttu-id="970b4-237">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="970b4-237">URL Rewriting </span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="970b4-238">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="970b4-238">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="970b4-239">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-239">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="970b4-240">WebSockets</span><span class="sxs-lookup"><span data-stu-id="970b4-240">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="970b4-241">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="970b4-241">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="970b4-242">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="970b4-242">Before components that are required to accept WebSocket requests.</span></span> |

<a name="middleware-writing-middleware"></a>

## <a name="writing-middleware"></a><span data-ttu-id="970b4-243">寫入中介軟體</span><span class="sxs-lookup"><span data-stu-id="970b4-243">Writing middleware</span></span>

<span data-ttu-id="970b4-244">中介軟體通常封裝在類別中，並以擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="970b4-244">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="970b4-245">請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="970b4-245">Consider the following middleware, which sets the culture for the current request from the query string:</span></span>

[!code-csharp[](index/sample/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="970b4-246">請注意：上述範例程式碼用於示範中介軟體元件的建立。</span><span class="sxs-lookup"><span data-stu-id="970b4-246">Note: The sample code above is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="970b4-247">如需 ASP.NET Core 的內建當地語系化支援，請參閱[全球化與當地語系化](xref:fundamentals/localization)。</span><span class="sxs-lookup"><span data-stu-id="970b4-247">See [ Globalization and localization](xref:fundamentals/localization) for ASP.NET Core's built-in localization support.</span></span>

<span data-ttu-id="970b4-248">您可藉由傳遞文化特性來測試中介軟體，例如 `http://localhost:7997/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="970b4-248">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="970b4-249">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="970b4-249">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddleware.cs)]

> [!NOTE]
> <span data-ttu-id="970b4-250">在 ASP.NET Core 1.x 中，中介軟體 `Task` 方法的名稱必須是 `Invoke`。</span><span class="sxs-lookup"><span data-stu-id="970b4-250">In ASP.NET Core 1.x, the middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="970b4-251">在 ASP.NET Core 2.0 或更新版本中，此名稱可以是 `Invoke` 或 `InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="970b4-251">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

<span data-ttu-id="970b4-252">下列擴充方法透過 [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) 公開中介軟體：</span><span class="sxs-lookup"><span data-stu-id="970b4-252">The following extension method exposes the middleware through [IApplicationBuilder](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder):</span></span>

[!code-csharp[](index/sample/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="970b4-253">下列程式碼會從 `Configure` 呼叫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="970b4-253">The following code calls the middleware from `Configure`:</span></span>

[!code-csharp[](index/sample/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="970b4-254">中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](http://deviq.com/explicit-dependencies-principle/)。</span><span class="sxs-lookup"><span data-stu-id="970b4-254">Middleware should follow the [Explicit Dependencies Principle](http://deviq.com/explicit-dependencies-principle/) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="970b4-255">中介軟體會在每次「應用程式存留期」就建構一次。</span><span class="sxs-lookup"><span data-stu-id="970b4-255">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="970b4-256">若您需要在要求內與中介軟體共用服務，請參閱下方的「依要求的相依性」。</span><span class="sxs-lookup"><span data-stu-id="970b4-256">See *Per-request dependencies* below if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="970b4-257">中介軟體元件可透過建構函式參數，解析其來自相依性插入的相依性。</span><span class="sxs-lookup"><span data-stu-id="970b4-257">Middleware components can resolve their dependencies from dependency injection through constructor parameters.</span></span> <span data-ttu-id="970b4-258">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) 也可直接接受其它參數。</span><span class="sxs-lookup"><span data-stu-id="970b4-258">[`UseMiddleware<T>`](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.builder.usemiddlewareextensions#methods_summary) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="970b4-259">依要求的相依性</span><span class="sxs-lookup"><span data-stu-id="970b4-259">Per-request dependencies</span></span>

<span data-ttu-id="970b4-260">因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」存留期服務不會與其它插入相依性的類型共用。</span><span class="sxs-lookup"><span data-stu-id="970b4-260">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors are not  shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="970b4-261">如果您必須在中介軟體和其他類型間共用「已限定範圍」的服務，請將這些服務新增至 `Invoke` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="970b4-261">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="970b4-262">`Invoke` 方法可以接受相依性插入所填入的其他參數。</span><span class="sxs-lookup"><span data-stu-id="970b4-262">The `Invoke` method can accept additional parameters that are populated by dependency injection.</span></span> <span data-ttu-id="970b4-263">例如: </span><span class="sxs-lookup"><span data-stu-id="970b4-263">For example:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="970b4-264">其他資源</span><span class="sxs-lookup"><span data-stu-id="970b4-264">Additional resources</span></span>

* [<span data-ttu-id="970b4-265">將 HTTP 模組遷移至中介軟體</span><span class="sxs-lookup"><span data-stu-id="970b4-265">Migrate HTTP Modules to Middleware</span></span>](xref:migration/http-modules)
* [<span data-ttu-id="970b4-266">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="970b4-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="970b4-267">要求功能</span><span class="sxs-lookup"><span data-stu-id="970b4-267">Request Features</span></span>](xref:fundamentals/request-features)
* [<span data-ttu-id="970b4-268">Factory 式中介軟體啟用</span><span class="sxs-lookup"><span data-stu-id="970b4-268">Factory-based middleware activation</span></span>](xref:fundamentals/middleware/extensibility)
* [<span data-ttu-id="970b4-269">以協力廠商容器啟用中介軟體</span><span class="sxs-lookup"><span data-stu-id="970b4-269">Middleware activation with a third-party container</span></span>](xref:fundamentals/middleware/extensibility-third-party-container)
