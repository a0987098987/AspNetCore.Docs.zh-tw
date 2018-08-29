---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
ms.author: riande
ms.custom: mvc
ms.date: 08/21/2018
uid: fundamentals/middleware/index
ms.openlocfilehash: e6dc76b7cb80e0dfda102df5aefb5d9ce9b821ed
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055802"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="609d4-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="609d4-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="609d4-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="609d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="609d4-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="609d4-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="609d4-106">每個元件：</span><span class="sxs-lookup"><span data-stu-id="609d4-106">Each component:</span></span>

* <span data-ttu-id="609d4-107">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="609d4-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="609d4-108">可以在叫用管線中下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="609d4-108">Can perform work before and after the next component in the pipeline is invoked.</span></span>

<span data-ttu-id="609d4-109">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="609d4-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="609d4-110">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="609d4-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="609d4-111">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="609d4-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="609d4-112">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="609d4-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="609d4-113">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="609d4-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="609d4-114">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="609d4-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span>

<span data-ttu-id="609d4-115"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="609d4-115"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides more middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="609d4-116">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="609d4-116">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="609d4-117">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="609d4-117">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="609d4-118">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="609d4-118">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="609d4-119">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="609d4-119">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="609d4-123">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="609d4-123">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="609d4-124">委派也可決定不將要求傳遞到下個委派，這就是所謂*對要求管線執行最少運算*。</span><span class="sxs-lookup"><span data-stu-id="609d4-124">A delegate can also decide to not pass a request to the next delegate, which is called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="609d4-125">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="609d4-125">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="609d4-126">例如，靜態檔案中介軟體可傳回靜態檔案的要求，並對剩餘管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="609d4-126">For example, Static Files Middleware can return a request for a static file and short-circuit the rest of the pipeline.</span></span> <span data-ttu-id="609d4-127">處理例外狀況的委派會提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="609d4-127">Exception-handling delegates are called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="609d4-128">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="609d4-128">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="609d4-129">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="609d4-129">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="609d4-130">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="609d4-130">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs?name=snippet1)]

<span data-ttu-id="609d4-131">第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="609d4-131">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="609d4-132">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="609d4-132">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="609d4-133">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="609d4-133">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="609d4-134">您可以「不」呼叫「下一個」參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="609d4-134">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="609d4-135">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="609d4-135">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?name=snippet1)]

> [!WARNING]
> <span data-ttu-id="609d4-136">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="609d4-136">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="609d4-137">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="609d4-137">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="609d4-138">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="609d4-138">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="609d4-139">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="609d4-139">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="609d4-140">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="609d4-140">May cause a protocol violation.</span></span> <span data-ttu-id="609d4-141">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="609d4-141">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="609d4-142">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="609d4-142">May corrupt the body format.</span></span> <span data-ttu-id="609d4-143">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="609d4-143">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="609d4-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="609d4-144"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="609d4-145">訂單</span><span class="sxs-lookup"><span data-stu-id="609d4-145">Order</span></span>

<span data-ttu-id="609d4-146">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="609d4-146">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="609d4-147">對安全性、效能與功能性而言，此順序相當重要。</span><span class="sxs-lookup"><span data-stu-id="609d4-147">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="609d4-148">下列 `Configure` 方法會新增下列中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="609d4-148">The following `Configure` method adds the following middleware components:</span></span>

1. <span data-ttu-id="609d4-149">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="609d4-149">Exception/error handling</span></span>
2. <span data-ttu-id="609d4-150">靜態檔案伺服器</span><span class="sxs-lookup"><span data-stu-id="609d4-150">Static file server</span></span>
3. <span data-ttu-id="609d4-151">驗證</span><span class="sxs-lookup"><span data-stu-id="609d4-151">Authentication</span></span>
4. <span data-ttu-id="609d4-152">MVC</span><span class="sxs-lookup"><span data-stu-id="609d4-152">MVC</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    if (env.IsDevelopment())
    {
        // When the app runs in the Development environment:
        //   Use the Developer Exception Page to report app runtime errors.
        //   Use the Database Error Page to report database runtime errors.
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        // When the app doesn't run in the Development environment:
        //   Enable the Exception Handler Middleware to catch exceptions
        //     thrown in the following middlewares.
        //   Use the HTTP Strict Transport Security Protocol (HSTS)
        //     Middleware.
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    // Use HTTPS Redirection Middleware to redirect HTTP requests to HTTPS.
    app.UseHttpsRedirection();

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Use Cookie Policy Middleware to conform to EU General Data 
    //   Protection Regulation (GDPR) regulations.
    app.UseCookiePolicy();

    // Authenticate before the user accesses secure resources.
    app.UseAuthentication();

    // Add MVC to the request pipeline.
    app.UseMvc();
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
public void Configure(IApplicationBuilder app)
{
    // Enable the Exception Handler Middleware to catch exceptions
    //   thrown in the following middlewares.
    app.UseExceptionHandler("/Home/Error");

    // Return static files and end the pipeline.
    app.UseStaticFiles();

    // Authenticate before you access secure resources.
    app.UseIdentity();

    // Add MVC to the request pipeline.
    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

<span data-ttu-id="609d4-153">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="609d4-153">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="609d4-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="609d4-154"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="609d4-155">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="609d4-155">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="609d4-156">系統會提早在管線中呼叫靜態檔案中介軟體，以便該軟體可處理要求並執行最少運算，而無需一一處理剩餘的元件。</span><span class="sxs-lookup"><span data-stu-id="609d4-156">Static Files Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="609d4-157">靜態檔案中介軟體**不**提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="609d4-157">The Static Files Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="609d4-158">其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="609d4-158">Any files served by it, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="609d4-159">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="609d4-159">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="609d4-160">若要求未經靜態檔案中介軟體處理，則會將其傳遞至驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)，以執行驗證。</span><span class="sxs-lookup"><span data-stu-id="609d4-160">If the request isn't handled by the Static Files Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="609d4-161">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="609d4-161">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="609d4-162">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="609d4-162">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="609d4-163">若要求未經靜態檔案中介軟體處理，則會將其傳遞至身分識別中介軟體 (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>)，以執行驗證。</span><span class="sxs-lookup"><span data-stu-id="609d4-163">If the request isn't handled by Static Files Middleware, it's passed on to the Identity Middleware (<xref:Microsoft.AspNetCore.Builder.BuilderExtensions.UseIdentity*>), which performs authentication.</span></span> <span data-ttu-id="609d4-164">身分識別不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="609d4-164">Identity doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="609d4-165">雖然身分識別會驗證要求，但只有在 MVC 選取特定控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="609d4-165">Although Identity authenticates requests, authorization (and rejection) occurs only after MVC selects a specific controller and action.</span></span>

::: moniker-end

<span data-ttu-id="609d4-166">下列範例會示範中介軟體的順序，其中靜態檔案中介軟體會比回應壓縮中介軟體先處理靜態檔案的要求。</span><span class="sxs-lookup"><span data-stu-id="609d4-166">The following example demonstrates a middleware order where requests for static files are handled by Static Files Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="609d4-167">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="609d4-167">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="609d4-168">來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。</span><span class="sxs-lookup"><span data-stu-id="609d4-168">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files not compressed by Static Files Middleware.
    app.UseStaticFiles();
    app.UseResponseCompression();
    app.UseMvcWithDefaultRoute();
}
```

### <a name="use-run-and-map"></a><span data-ttu-id="609d4-169">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="609d4-169">Use, Run, and Map</span></span>

<span data-ttu-id="609d4-170">設定 HTTP 管線的方法是使用 `Use`、`Run` 及 `Map`。</span><span class="sxs-lookup"><span data-stu-id="609d4-170">Configure the HTTP pipeline using `Use`, `Run`, and `Map`.</span></span> <span data-ttu-id="609d4-171">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="609d4-171">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="609d4-172">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="609d4-172">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="609d4-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="609d4-173"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="609d4-174">`Map*` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="609d4-174">`Map*` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="609d4-175">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="609d4-175">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs?name=snippet1)]

<span data-ttu-id="609d4-176">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="609d4-176">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="609d4-177">要求</span><span class="sxs-lookup"><span data-stu-id="609d4-177">Request</span></span>             | <span data-ttu-id="609d4-178">回應</span><span class="sxs-lookup"><span data-stu-id="609d4-178">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="609d4-179">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="609d4-179">localhost:1234</span></span>      | <span data-ttu-id="609d4-180">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="609d4-180">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="609d4-181">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="609d4-181">localhost:1234/map1</span></span> | <span data-ttu-id="609d4-182">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="609d4-182">Map Test 1</span></span>                   |
| <span data-ttu-id="609d4-183">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="609d4-183">localhost:1234/map2</span></span> | <span data-ttu-id="609d4-184">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="609d4-184">Map Test 2</span></span>                   |
| <span data-ttu-id="609d4-185">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="609d4-185">localhost:1234/map3</span></span> | <span data-ttu-id="609d4-186">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="609d4-186">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="609d4-187">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="609d4-187">When `Map` is used, the matched path segment(s) are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="609d4-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="609d4-188">[MapWhen](/dotnet/api/microsoft.aspnetcore.builder.mapwhenextensions) branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="609d4-189">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="609d4-189">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="609d4-190">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="609d4-190">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?name=snippet1)]

<span data-ttu-id="609d4-191">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="609d4-191">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="609d4-192">要求</span><span class="sxs-lookup"><span data-stu-id="609d4-192">Request</span></span>                       | <span data-ttu-id="609d4-193">回應</span><span class="sxs-lookup"><span data-stu-id="609d4-193">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="609d4-194">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="609d4-194">localhost:1234</span></span>                | <span data-ttu-id="609d4-195">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="609d4-195">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="609d4-196">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="609d4-196">localhost:1234/?branch=master</span></span> | <span data-ttu-id="609d4-197">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="609d4-197">Branch used = master</span></span>         |

<span data-ttu-id="609d4-198">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="609d4-198">`Map` supports nesting, for example:</span></span>

```csharp
app.Map("/level1", level1App => {
    level1App.Map("/level2a", level2AApp => {
        // "/level1/level2a" processing
    });
    level1App.Map("/level2b", level2BApp => {
        // "/level1/level2b" processing
    });
});
   ```

<span data-ttu-id="609d4-199">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="609d4-199">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?name=snippet1&highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="609d4-200">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="609d4-200">Built-in middleware</span></span>

<span data-ttu-id="609d4-201">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="609d4-201">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="609d4-202">「順序」欄位說明中介軟體在要求管線中的位置，以及中介軟體可終止要求並防止其他中介軟體處理要求的情況。</span><span class="sxs-lookup"><span data-stu-id="609d4-202">The *Order* column provides notes on the middleware's placement in the request pipeline and under what conditions the middleware may terminate the request and prevent other middleware from processing a request.</span></span>

| <span data-ttu-id="609d4-203">中介軟體</span><span class="sxs-lookup"><span data-stu-id="609d4-203">Middleware</span></span> | <span data-ttu-id="609d4-204">描述</span><span class="sxs-lookup"><span data-stu-id="609d4-204">Description</span></span> | <span data-ttu-id="609d4-205">訂單</span><span class="sxs-lookup"><span data-stu-id="609d4-205">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="609d4-206">驗證</span><span class="sxs-lookup"><span data-stu-id="609d4-206">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="609d4-207">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="609d4-207">Provides authentication support.</span></span> | <span data-ttu-id="609d4-208">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-208">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="609d4-209">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="609d4-209">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="609d4-210">CORS</span><span class="sxs-lookup"><span data-stu-id="609d4-210">CORS</span></span>](xref:security/cors) | <span data-ttu-id="609d4-211">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="609d4-211">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="609d4-212">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-212">Before components that use CORS.</span></span> |
| [<span data-ttu-id="609d4-213">診斷</span><span class="sxs-lookup"><span data-stu-id="609d4-213">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="609d4-214">設定診斷。</span><span class="sxs-lookup"><span data-stu-id="609d4-214">Configures diagnostics.</span></span> | <span data-ttu-id="609d4-215">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-215">Before components that generate errors.</span></span> |
| [<span data-ttu-id="609d4-216">轉送標頭</span><span class="sxs-lookup"><span data-stu-id="609d4-216">Forwarded Headers</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions) | <span data-ttu-id="609d4-217">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="609d4-217">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="609d4-218">在使用更新欄位的元件之前 (例如：配置、主機、用戶端 IP、方法)。</span><span class="sxs-lookup"><span data-stu-id="609d4-218">Before components that consume the updated fields (examples: scheme, host, client IP, method).</span></span> |
| [<span data-ttu-id="609d4-219">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="609d4-219">HTTP Method Override</span></span>](/dotnet/api/microsoft.aspnetcore.builder.httpmethodoverrideextensions) | <span data-ttu-id="609d4-220">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="609d4-220">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="609d4-221">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-221">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="609d4-222">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="609d4-222">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="609d4-223">將所有 HTTP 要求重新都導向至 HTTPS (ASP.NET Core 2.1 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="609d4-223">Redirect all HTTP requests to HTTPS (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="609d4-224">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-224">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="609d4-225">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="609d4-225">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="609d4-226">增強安全性的中介軟體，可新增特殊的回應標頭 (ASP.NET Core 2.1 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="609d4-226">Security enhancement middleware that adds a special response header (ASP.NET Core 2.1 or later).</span></span> | <span data-ttu-id="609d4-227">在傳送回應前和修改要求的元件後 (例如，轉送標頭、URL 重寫)。</span><span class="sxs-lookup"><span data-stu-id="609d4-227">Before responses are sent and after components that modify requests (for example, Forwarded Headers, URL Rewriting).</span></span> |
| [<span data-ttu-id="609d4-228">MVC</span><span class="sxs-lookup"><span data-stu-id="609d4-228">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="609d4-229">使用 MVC/Razor 頁面處理要求 (ASP.NET Core 2.0 或更新版本)。</span><span class="sxs-lookup"><span data-stu-id="609d4-229">Processes requests with MVC/Razor Pages (ASP.NET Core 2.0 or later).</span></span> | <span data-ttu-id="609d4-230">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="609d4-230">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="609d4-231">OWIN</span><span class="sxs-lookup"><span data-stu-id="609d4-231">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="609d4-232">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="609d4-232">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="609d4-233">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="609d4-233">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="609d4-234">回應快取</span><span class="sxs-lookup"><span data-stu-id="609d4-234">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="609d4-235">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="609d4-235">Provides support for caching responses.</span></span> | <span data-ttu-id="609d4-236">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-236">Before components that require caching.</span></span> |
| [<span data-ttu-id="609d4-237">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="609d4-237">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="609d4-238">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="609d4-238">Provides support for compressing responses.</span></span> | <span data-ttu-id="609d4-239">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-239">Before components that require compression.</span></span> |
| [<span data-ttu-id="609d4-240">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="609d4-240">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="609d4-241">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="609d4-241">Provides localization support.</span></span> | <span data-ttu-id="609d4-242">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-242">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="609d4-243">路由傳送</span><span class="sxs-lookup"><span data-stu-id="609d4-243">Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="609d4-244">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="609d4-244">Defines and constrains request routes.</span></span> | <span data-ttu-id="609d4-245">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="609d4-245">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="609d4-246">工作階段</span><span class="sxs-lookup"><span data-stu-id="609d4-246">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="609d4-247">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="609d4-247">Provides support for managing user sessions.</span></span> | <span data-ttu-id="609d4-248">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-248">Before components that require Session.</span></span> |
| [<span data-ttu-id="609d4-249">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="609d4-249">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="609d4-250">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="609d4-250">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="609d4-251">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="609d4-251">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="609d4-252">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="609d4-252">URL Rewriting</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="609d4-253">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="609d4-253">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="609d4-254">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-254">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="609d4-255">WebSockets</span><span class="sxs-lookup"><span data-stu-id="609d4-255">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="609d4-256">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="609d4-256">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="609d4-257">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="609d4-257">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="write-middleware"></a><span data-ttu-id="609d4-258">撰寫中介軟體</span><span class="sxs-lookup"><span data-stu-id="609d4-258">Write middleware</span></span>

<span data-ttu-id="609d4-259">中介軟體通常封裝在類別中，並以擴充方法公開。</span><span class="sxs-lookup"><span data-stu-id="609d4-259">Middleware is generally encapsulated in a class and exposed with an extension method.</span></span> <span data-ttu-id="609d4-260">請考慮下列中介軟體，其會為來自查詢字串的目前要求設定文化特性：</span><span class="sxs-lookup"><span data-stu-id="609d4-260">Consider the following middleware, which sets the culture for the current request from a query string:</span></span>

[!code-csharp[](index/snapshot/Culture/StartupCulture.cs?name=snippet1)]

<span data-ttu-id="609d4-261">上述範例程式碼用於示範中介軟體元件的建立。</span><span class="sxs-lookup"><span data-stu-id="609d4-261">The preceding sample code is used to demonstrate creating a middleware component.</span></span> <span data-ttu-id="609d4-262">如需 ASP.NET Core 的內建當地語系化支援，請參閱 <xref:fundamentals/localization>。</span><span class="sxs-lookup"><span data-stu-id="609d4-262">For ASP.NET Core's built-in localization support, see <xref:fundamentals/localization>.</span></span>

<span data-ttu-id="609d4-263">您可藉由傳遞文化特性來測試中介軟體，例如 `http://localhost:7997/?culture=no`。</span><span class="sxs-lookup"><span data-stu-id="609d4-263">You can test the middleware by passing in the culture, for example `http://localhost:7997/?culture=no`.</span></span>

<span data-ttu-id="609d4-264">下列程式碼會將中介軟體委派移至類別：</span><span class="sxs-lookup"><span data-stu-id="609d4-264">The following code moves the middleware delegate to a class:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddleware.cs)]

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="609d4-265">中介軟體 `Task` 方法的名稱必須是 `Invoke`。</span><span class="sxs-lookup"><span data-stu-id="609d4-265">The middleware `Task` method's name must be `Invoke`.</span></span> <span data-ttu-id="609d4-266">在 ASP.NET Core 2.0 或更新版本中，此名稱可以是 `Invoke` 或 `InvokeAsync`。</span><span class="sxs-lookup"><span data-stu-id="609d4-266">In ASP.NET Core 2.0 or later, the name can be either `Invoke` or `InvokeAsync`.</span></span>

::: moniker-end

<span data-ttu-id="609d4-267">下列擴充方法透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 公開中介軟體：</span><span class="sxs-lookup"><span data-stu-id="609d4-267">The following extension method exposes the middleware through <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder>:</span></span>

[!code-csharp[](index/snapshot/Culture/RequestCultureMiddlewareExtensions.cs)]

<span data-ttu-id="609d4-268">下列程式碼會從 `Startup.Configure` 呼叫中介軟體：</span><span class="sxs-lookup"><span data-stu-id="609d4-268">The following code calls the middleware from `Startup.Configure`:</span></span>

[!code-csharp[](index/snapshot/Culture/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="609d4-269">中介軟體應於其建構函式中公開其相依性，以遵循[明確的相依性原則](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies)。</span><span class="sxs-lookup"><span data-stu-id="609d4-269">Middleware should follow the [Explicit Dependencies Principle](/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#explicit-dependencies) by exposing its dependencies in its constructor.</span></span> <span data-ttu-id="609d4-270">中介軟體會在每次「應用程式存留期」就建構一次。</span><span class="sxs-lookup"><span data-stu-id="609d4-270">Middleware is constructed once per *application lifetime*.</span></span> <span data-ttu-id="609d4-271">若您需要在要求內與中介軟體共用服務，請參閱[依要求的相依性](#per-request-dependencies)一節。</span><span class="sxs-lookup"><span data-stu-id="609d4-271">See the [Per-request dependencies](#per-request-dependencies) section if you need to share services with middleware within a request.</span></span>

<span data-ttu-id="609d4-272">中介軟體元件可透過建構函式參數，解析其來自[相依性插入 (DI)](xref:fundamentals/dependency-injection) 的相依性。</span><span class="sxs-lookup"><span data-stu-id="609d4-272">Middleware components can resolve their dependencies from [dependency injection (DI)](xref:fundamentals/dependency-injection) through constructor parameters.</span></span> <span data-ttu-id="609d4-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) 也可直接接受其它參數。</span><span class="sxs-lookup"><span data-stu-id="609d4-273">[UseMiddleware&lt;T&gt;](/dotnet/api/microsoft.aspnetcore.builder.usemiddlewareextensions.usemiddleware#Microsoft_AspNetCore_Builder_UseMiddlewareExtensions_UseMiddleware_Microsoft_AspNetCore_Builder_IApplicationBuilder_System_Type_System_Object___) can also accept additional parameters directly.</span></span>

### <a name="per-request-dependencies"></a><span data-ttu-id="609d4-274">依要求的相依性</span><span class="sxs-lookup"><span data-stu-id="609d4-274">Per-request dependencies</span></span>

<span data-ttu-id="609d4-275">因為中介軟體建構於應用程式啟動時，而非依要求建構，所以在每個要求期間，中介軟體建構函式使用的「已限定範圍」存留期服務不會與其它插入相依性的類型共用。</span><span class="sxs-lookup"><span data-stu-id="609d4-275">Because middleware is constructed at app startup, not per-request, *scoped* lifetime services used by middleware constructors aren't shared with other dependency-injected types during each request.</span></span> <span data-ttu-id="609d4-276">如果您必須在中介軟體和其他類型間共用「已限定範圍」的服務，請將這些服務新增至 `Invoke` 方法的簽章。</span><span class="sxs-lookup"><span data-stu-id="609d4-276">If you must share a *scoped* service between your middleware and other types, add these services to the `Invoke` method's signature.</span></span> <span data-ttu-id="609d4-277">`Invoke` 方法可以接受 DI 所填入的其他參數：</span><span class="sxs-lookup"><span data-stu-id="609d4-277">The `Invoke` method can accept additional parameters that are populated by DI:</span></span>

```csharp
public class CustomMiddleware
{
    private readonly RequestDelegate _next;

    public CustomMiddleware(RequestDelegate next)
    {
        _next = next;
    }

    // IMyScopedService is injected into Invoke
    public async Task Invoke(HttpContext httpContext, IMyScopedService svc)
    {
        svc.MyProperty = 1000;
        await _next(httpContext);
    }
}
```

## <a name="additional-resources"></a><span data-ttu-id="609d4-278">其他資源</span><span class="sxs-lookup"><span data-stu-id="609d4-278">Additional resources</span></span>

* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
