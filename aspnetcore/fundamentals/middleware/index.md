---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/02/2020
uid: fundamentals/middleware/index
ms.openlocfilehash: 6698e269e0a6480cd5a03c59f9a19da31e23bf69
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089145"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="87734-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="87734-103">ASP.NET Core Middleware</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="87734-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="87734-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="87734-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="87734-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="87734-106">每個元件：</span><span class="sxs-lookup"><span data-stu-id="87734-106">Each component:</span></span>

* <span data-ttu-id="87734-107">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="87734-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="87734-108">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="87734-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="87734-109">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="87734-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="87734-110">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="87734-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="87734-111">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="87734-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="87734-112">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="87734-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="87734-113">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="87734-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="87734-114">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="87734-115">當中介軟體短路時，稱為「終端中介軟體」，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="87734-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="87734-116"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="87734-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="87734-117">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="87734-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="87734-118">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="87734-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="87734-119">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="87734-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="87734-120">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="87734-120">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="87734-124">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="87734-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="87734-125">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="87734-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="87734-126">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="87734-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="87734-127">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="87734-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="87734-128">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="87734-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="87734-129">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="87734-129">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="87734-130">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="87734-130">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="87734-131">您可以「不」呼叫「下一個」參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-131">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="87734-132">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="87734-132">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=5-10)]

<span data-ttu-id="87734-133">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="87734-133">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="87734-134">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="87734-134">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="87734-135">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="87734-135">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="87734-136">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="87734-136">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="87734-137">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="87734-137">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="87734-138">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="87734-138">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="87734-139">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-139">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="87734-140">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-140">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="87734-141">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="87734-141">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="87734-142">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87734-142">May cause a protocol violation.</span></span> <span data-ttu-id="87734-143">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="87734-143">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="87734-144">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="87734-144">May corrupt the body format.</span></span> <span data-ttu-id="87734-145">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="87734-145">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="87734-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="87734-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<span data-ttu-id="87734-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派不會收到 `next` 參數。</span><span class="sxs-lookup"><span data-stu-id="87734-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegates don't receive a `next` parameter.</span></span> <span data-ttu-id="87734-148">第一個 `Run` 委派一律是 terminal 並終止管線。</span><span class="sxs-lookup"><span data-stu-id="87734-148">The first `Run` delegate is always terminal and terminates the pipeline.</span></span> <span data-ttu-id="87734-149">`Run` 是慣例。</span><span class="sxs-lookup"><span data-stu-id="87734-149">`Run` is a convention.</span></span> <span data-ttu-id="87734-150">某些中介軟體元件可能會公開在管線結尾處執行 `Run[Middleware]` 方法：</span><span class="sxs-lookup"><span data-stu-id="87734-150">Some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=12-15)]

<span data-ttu-id="87734-151">在上述範例中，`Run` 委派會將 `"Hello from 2nd delegate."` 寫入至回應，然後終止管線。</span><span class="sxs-lookup"><span data-stu-id="87734-151">In the preceding example, the `Run` delegate writes `"Hello from 2nd delegate."` to the response and then terminates the pipeline.</span></span> <span data-ttu-id="87734-152">如果在 `Run` 委派後面加入另一個 `Use` 或 `Run` 委派，則不會呼叫它。</span><span class="sxs-lookup"><span data-stu-id="87734-152">If another `Use` or `Run` delegate is added after the `Run` delegate, it's not called.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="87734-153">中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="87734-153">Middleware order</span></span>

<span data-ttu-id="87734-154">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="87734-154">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="87734-155">順序對於安全性、效能和功能非常**重要**。</span><span class="sxs-lookup"><span data-stu-id="87734-155">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="87734-156">下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="87734-156">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

<span data-ttu-id="87734-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="87734-157">In the preceding code:</span></span>

* <span data-ttu-id="87734-158">使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。</span><span class="sxs-lookup"><span data-stu-id="87734-158">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="87734-159">並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。</span><span class="sxs-lookup"><span data-stu-id="87734-159">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="87734-160">例如，`UseCors`、`UseAuthentication`和 `UseAuthorization` 必須以顯示的循序執行。</span><span class="sxs-lookup"><span data-stu-id="87734-160">For example, `UseCors`, `UseAuthentication`, and `UseAuthorization` must go in the order shown.</span></span>

<span data-ttu-id="87734-161">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="87734-161">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="87734-162">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="87734-162">Exception/error handling</span></span>
   * <span data-ttu-id="87734-163">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="87734-163">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="87734-164">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="87734-164">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="87734-165">資料庫錯誤頁面中介軟體會報告資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="87734-165">Database Error Page Middleware reports database runtime errors.</span></span>
   * <span data-ttu-id="87734-166">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="87734-166">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="87734-167">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-167">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="87734-168">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="87734-168">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="87734-169">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="87734-169">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="87734-170">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="87734-170">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="87734-171">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="87734-171">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="87734-172">路由中介軟體（`UseRouting`）以路由傳送要求。</span><span class="sxs-lookup"><span data-stu-id="87734-172">Routing Middleware (`UseRouting`) to route requests.</span></span>
1. <span data-ttu-id="87734-173">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="87734-173">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="87734-174">授權中介軟體（`UseAuthorization`）會授權使用者存取安全的資源。</span><span class="sxs-lookup"><span data-stu-id="87734-174">Authorization Middleware (`UseAuthorization`) authorizes a user to access secure resources.</span></span>
1. <span data-ttu-id="87734-175">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="87734-175">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="87734-176">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87734-176">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="87734-177">端點路由中介軟體（使用 `MapRazorPages``UseEndpoints`），以將 Razor Pages 端點新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="87734-177">Endpoint Routing Middleware (`UseEndpoints` with `MapRazorPages`) to add Razor Pages endpoints to the request pipeline.</span></span>

<!--

FUTURE UPDATE

On the next topic overhaul/release update, add API crosslink to "Database Error Page Middleware" in Item 1 of the list ...

Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage*

... when available via the API docs.

-->

```csharp
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseRouting();
    app.UseAuthentication();
    app.UseAuthorization();
    app.UseSession();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

<span data-ttu-id="87734-178">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 命名空間在 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 上公開。</span><span class="sxs-lookup"><span data-stu-id="87734-178">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="87734-179"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="87734-179"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="87734-180">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-180">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="87734-181">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-181">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="87734-182">靜態檔案中介軟體**不會**執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="87734-182">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="87734-183">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="87734-183">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="87734-184">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="87734-184">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="87734-185">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="87734-185">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="87734-186">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-186">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="87734-187">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="87734-187">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="87734-188">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="87734-188">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="87734-189">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="87734-189">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="87734-190">Razor Pages 回應可以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="87734-190">The Razor Pages responses can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

<span data-ttu-id="87734-191">對於單一頁面應用程式，SPA 中介軟體 <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> 通常是中介軟體管線中的最後一個。</span><span class="sxs-lookup"><span data-stu-id="87734-191">For single page applications, the SPA middleware <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> usually comes last in the middleware pipeline.</span></span> <span data-ttu-id="87734-192">SPA 中介軟體最後一步：</span><span class="sxs-lookup"><span data-stu-id="87734-192">The SPA middleware comes last:</span></span>

* <span data-ttu-id="87734-193">以允許所有其他中介軟體先回應相符的要求。</span><span class="sxs-lookup"><span data-stu-id="87734-193">To allow all other middlewares to respond to matching requests first.</span></span>
* <span data-ttu-id="87734-194">允許 Spa 與用戶端路由針對伺服器應用程式無法辨識的所有路由執行。</span><span class="sxs-lookup"><span data-stu-id="87734-194">To allow SPAs with client-side routing to run for all routes that are unrecognized by the server app.</span></span>

<span data-ttu-id="87734-195">如需單一頁面應用程式的詳細資訊，請參閱[回應](xref:spa/react)和[角度](xref:spa/angular)專案範本的指南。</span><span class="sxs-lookup"><span data-stu-id="87734-195">For more details on single page applications, see the guides for the [React](xref:spa/react) and [Angular](xref:spa/angular) project templates.</span></span>

## <a name="branch-the-middleware-pipeline"></a><span data-ttu-id="87734-196">分支中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="87734-196">Branch the middleware pipeline</span></span>

<span data-ttu-id="87734-197"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="87734-197"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="87734-198">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="87734-198">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="87734-199">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="87734-199">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="87734-200">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="87734-200">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="87734-201">要求</span><span class="sxs-lookup"><span data-stu-id="87734-201">Request</span></span>             | <span data-ttu-id="87734-202">回應</span><span class="sxs-lookup"><span data-stu-id="87734-202">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="87734-203">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="87734-203">localhost:1234</span></span>      | <span data-ttu-id="87734-204">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="87734-204">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="87734-205">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="87734-205">localhost:1234/map1</span></span> | <span data-ttu-id="87734-206">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="87734-206">Map Test 1</span></span>                   |
| <span data-ttu-id="87734-207">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="87734-207">localhost:1234/map2</span></span> | <span data-ttu-id="87734-208">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="87734-208">Map Test 2</span></span>                   |
| <span data-ttu-id="87734-209">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="87734-209">localhost:1234/map3</span></span> | <span data-ttu-id="87734-210">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="87734-210">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="87734-211">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="87734-211">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="87734-212">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="87734-212">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="87734-213">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="87734-213">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

<span data-ttu-id="87734-214"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="87734-214"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="87734-215">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="87734-215">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="87734-216">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="87734-216">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?highlight=14-15)]

<span data-ttu-id="87734-217">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="87734-217">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="87734-218">要求</span><span class="sxs-lookup"><span data-stu-id="87734-218">Request</span></span>                       | <span data-ttu-id="87734-219">回應</span><span class="sxs-lookup"><span data-stu-id="87734-219">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="87734-220">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="87734-220">localhost:1234</span></span>                | <span data-ttu-id="87734-221">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="87734-221">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="87734-222">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="87734-222">localhost:1234/?branch=master</span></span> | <span data-ttu-id="87734-223">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="87734-223">Branch used = master</span></span>         |

<span data-ttu-id="87734-224"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> 也會根據給定述詞的結果來分支要求管線。</span><span class="sxs-lookup"><span data-stu-id="87734-224"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> also branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="87734-225">與 `MapWhen`不同的是，如果此分支進行短路或包含終端機中介軟體，則會重新加入主要管線：</span><span class="sxs-lookup"><span data-stu-id="87734-225">Unlike with `MapWhen`, this branch is rejoined to the main pipeline if it does short-circuit or contain a terminal middleware:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupUseWhen.cs?highlight=23-24)]

<span data-ttu-id="87734-226">在上述範例中，回應為「來自主要管線的 Hello」。</span><span class="sxs-lookup"><span data-stu-id="87734-226">In the preceding example, a response of "Hello from main pipeline."</span></span> <span data-ttu-id="87734-227">會針對所有要求寫入。</span><span class="sxs-lookup"><span data-stu-id="87734-227">is written for all requests.</span></span> <span data-ttu-id="87734-228">如果要求中包含查詢字串變數 `branch`，則會在重新加入主要管線之前記錄其值。</span><span class="sxs-lookup"><span data-stu-id="87734-228">If the request includes a query string variable `branch`, its value is logged before the main pipeline is rejoined.</span></span>

## <a name="built-in-middleware"></a><span data-ttu-id="87734-229">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="87734-229">Built-in middleware</span></span>

<span data-ttu-id="87734-230">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="87734-230">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="87734-231">「順序」欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="87734-231">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="87734-232">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="87734-232">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="87734-233">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="87734-233">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="87734-234">中介軟體</span><span class="sxs-lookup"><span data-stu-id="87734-234">Middleware</span></span> | <span data-ttu-id="87734-235">描述</span><span class="sxs-lookup"><span data-stu-id="87734-235">Description</span></span> | <span data-ttu-id="87734-236">單</span><span class="sxs-lookup"><span data-stu-id="87734-236">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="87734-237">驗證</span><span class="sxs-lookup"><span data-stu-id="87734-237">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="87734-238">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="87734-238">Provides authentication support.</span></span> | <span data-ttu-id="87734-239">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="87734-239">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="87734-240">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="87734-240">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="87734-241">授權</span><span class="sxs-lookup"><span data-stu-id="87734-241">Authorization</span></span>](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*) | <span data-ttu-id="87734-242">提供授權支援。</span><span class="sxs-lookup"><span data-stu-id="87734-242">Provides authorization support.</span></span> | <span data-ttu-id="87734-243">緊接在驗證中介軟體之後。</span><span class="sxs-lookup"><span data-stu-id="87734-243">Immediately after the Authentication Middleware.</span></span> |
| [<span data-ttu-id="87734-244">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="87734-244">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="87734-245">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="87734-245">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="87734-246">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="87734-246">Before middleware that issues cookies.</span></span> <span data-ttu-id="87734-247">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="87734-247">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="87734-248">CORS</span><span class="sxs-lookup"><span data-stu-id="87734-248">CORS</span></span>](xref:security/cors) | <span data-ttu-id="87734-249">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="87734-249">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="87734-250">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-250">Before components that use CORS.</span></span> |
| [<span data-ttu-id="87734-251">診斷</span><span class="sxs-lookup"><span data-stu-id="87734-251">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="87734-252">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87734-252">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="87734-253">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-253">Before components that generate errors.</span></span> <span data-ttu-id="87734-254">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="87734-254">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="87734-255">轉送標頭</span><span class="sxs-lookup"><span data-stu-id="87734-255">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="87734-256">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="87734-256">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="87734-257">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="87734-257">Before components that consume the updated fields.</span></span> <span data-ttu-id="87734-258">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="87734-258">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="87734-259">健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="87734-259">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="87734-260">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="87734-260">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="87734-261">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="87734-261">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="87734-262">標頭傳播</span><span class="sxs-lookup"><span data-stu-id="87734-262">Header Propagation</span></span>](xref:fundamentals/http-requests#header-propagation-middleware) | <span data-ttu-id="87734-263">將 HTTP 標頭從傳入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="87734-263">Propagates HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> |
| [<span data-ttu-id="87734-264">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="87734-264">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="87734-265">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="87734-265">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="87734-266">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-266">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="87734-267">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="87734-267">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="87734-268">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="87734-268">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="87734-269">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-269">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="87734-270">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="87734-270">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="87734-271">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="87734-271">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="87734-272">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="87734-272">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="87734-273">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="87734-273">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="87734-274">MVC</span><span class="sxs-lookup"><span data-stu-id="87734-274">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="87734-275">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="87734-275">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="87734-276">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="87734-276">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="87734-277">OWIN</span><span class="sxs-lookup"><span data-stu-id="87734-277">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="87734-278">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="87734-278">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="87734-279">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="87734-279">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="87734-280">回應快取</span><span class="sxs-lookup"><span data-stu-id="87734-280">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="87734-281">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-281">Provides support for caching responses.</span></span> | <span data-ttu-id="87734-282">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-282">Before components that require caching.</span></span> |
| [<span data-ttu-id="87734-283">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="87734-283">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="87734-284">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-284">Provides support for compressing responses.</span></span> | <span data-ttu-id="87734-285">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-285">Before components that require compression.</span></span> |
| [<span data-ttu-id="87734-286">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="87734-286">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="87734-287">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="87734-287">Provides localization support.</span></span> | <span data-ttu-id="87734-288">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-288">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="87734-289">端點路由</span><span class="sxs-lookup"><span data-stu-id="87734-289">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="87734-290">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="87734-290">Defines and constrains request routes.</span></span> | <span data-ttu-id="87734-291">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="87734-291">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="87734-292">SPA</span><span class="sxs-lookup"><span data-stu-id="87734-292">SPA</span></span>](xref:Microsoft.AspNetCore.Builder.SpaApplicationBuilderExtensions.UseSpa*) | <span data-ttu-id="87734-293">藉由傳回單一頁面應用程式（SPA）的預設頁面，處理來自中介軟體鏈中此點的所有要求</span><span class="sxs-lookup"><span data-stu-id="87734-293">Handles all requests from this point in the middleware chain by returning the default page for the Single Page Application (SPA)</span></span> | <span data-ttu-id="87734-294">在鏈晚期，讓提供靜態檔案、MVC 動作等的其他中介軟體優先。</span><span class="sxs-lookup"><span data-stu-id="87734-294">Late in the chain, so that other middleware for serving static files, MVC actions, etc., takes precedence.</span></span>|
| [<span data-ttu-id="87734-295">工作階段</span><span class="sxs-lookup"><span data-stu-id="87734-295">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="87734-296">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-296">Provides support for managing user sessions.</span></span> | <span data-ttu-id="87734-297">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-297">Before components that require Session.</span></span> | 
| [<span data-ttu-id="87734-298">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="87734-298">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="87734-299">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="87734-299">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="87734-300">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="87734-300">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="87734-301">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="87734-301">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="87734-302">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-302">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="87734-303">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-303">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="87734-304">WebSockets</span><span class="sxs-lookup"><span data-stu-id="87734-304">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="87734-305">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87734-305">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="87734-306">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-306">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="87734-307">其他資源</span><span class="sxs-lookup"><span data-stu-id="87734-307">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="87734-308">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="87734-308">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="87734-309">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="87734-309">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="87734-310">每個元件：</span><span class="sxs-lookup"><span data-stu-id="87734-310">Each component:</span></span>

* <span data-ttu-id="87734-311">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="87734-311">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="87734-312">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="87734-312">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="87734-313">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="87734-313">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="87734-314">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="87734-314">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="87734-315">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="87734-315">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="87734-316">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="87734-316">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="87734-317">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="87734-317">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="87734-318">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-318">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="87734-319">當中介軟體短路時，稱為「終端中介軟體」，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="87734-319">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="87734-320"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="87734-320"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="87734-321">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="87734-321">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="87734-322">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="87734-322">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="87734-323">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="87734-323">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="87734-324">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="87734-324">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="87734-328">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="87734-328">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="87734-329">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="87734-329">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="87734-330">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="87734-330">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="87734-331">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="87734-331">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="87734-332">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="87734-332">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="87734-333">第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="87734-333">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="87734-334">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="87734-334">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="87734-335">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="87734-335">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="87734-336">您可以「不」呼叫「下一個」參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-336">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="87734-337">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="87734-337">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

<span data-ttu-id="87734-338">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="87734-338">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="87734-339">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="87734-339">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="87734-340">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="87734-340">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="87734-341">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="87734-341">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="87734-342">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="87734-342">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="87734-343">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="87734-343">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="87734-344">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-344">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="87734-345">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-345">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="87734-346">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="87734-346">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="87734-347">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87734-347">May cause a protocol violation.</span></span> <span data-ttu-id="87734-348">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="87734-348">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="87734-349">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="87734-349">May corrupt the body format.</span></span> <span data-ttu-id="87734-350">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="87734-350">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="87734-351"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="87734-351"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="87734-352">中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="87734-352">Middleware order</span></span>

<span data-ttu-id="87734-353">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="87734-353">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="87734-354">順序對於安全性、效能和功能非常**重要**。</span><span class="sxs-lookup"><span data-stu-id="87734-354">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="87734-355">下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="87734-355">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

<span data-ttu-id="87734-356">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="87734-356">In the preceding code:</span></span>

* <span data-ttu-id="87734-357">使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。</span><span class="sxs-lookup"><span data-stu-id="87734-357">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="87734-358">並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。</span><span class="sxs-lookup"><span data-stu-id="87734-358">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="87734-359">例如，`UseCors` 和 `UseAuthentication` 必須以顯示的循序執行。</span><span class="sxs-lookup"><span data-stu-id="87734-359">For example, `UseCors` and `UseAuthentication` must go in the order shown.</span></span>

<span data-ttu-id="87734-360">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="87734-360">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="87734-361">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="87734-361">Exception/error handling</span></span>
   * <span data-ttu-id="87734-362">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="87734-362">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="87734-363">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="87734-363">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="87734-364">資料錯誤頁面中介軟體 (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) 會回報資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="87734-364">Database Error Page Middleware (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) reports database runtime errors.</span></span>
   * <span data-ttu-id="87734-365">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="87734-365">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="87734-366">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-366">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="87734-367">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="87734-367">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="87734-368">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="87734-368">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="87734-369">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="87734-369">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="87734-370">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="87734-370">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="87734-371">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="87734-371">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="87734-372">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="87734-372">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="87734-373">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87734-373">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="87734-374">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) 以將 MVC 新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="87734-374">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) to add MVC to the request pipeline.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();
    app.UseMvc();
}
```

<span data-ttu-id="87734-375">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 命名空間在 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 上公開。</span><span class="sxs-lookup"><span data-stu-id="87734-375">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="87734-376"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="87734-376"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="87734-377">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="87734-377">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="87734-378">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-378">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="87734-379">靜態檔案中介軟體**不會**執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="87734-379">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="87734-380">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="87734-380">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="87734-381">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="87734-381">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="87734-382">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="87734-382">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="87734-383">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="87734-383">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="87734-384">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="87734-384">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="87734-385">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="87734-385">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="87734-386">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="87734-386">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="87734-387">來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。</span><span class="sxs-lookup"><span data-stu-id="87734-387">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a><span data-ttu-id="87734-388">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="87734-388">Use, Run, and Map</span></span>

<span data-ttu-id="87734-389">設定 HTTP 管線的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 及 <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>。</span><span class="sxs-lookup"><span data-stu-id="87734-389">Configure the HTTP pipeline using <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, and <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>.</span></span> <span data-ttu-id="87734-390">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="87734-390">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="87734-391">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="87734-391">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="87734-392"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="87734-392"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="87734-393">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="87734-393">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="87734-394">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="87734-394">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="87734-395">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="87734-395">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="87734-396">要求</span><span class="sxs-lookup"><span data-stu-id="87734-396">Request</span></span>             | <span data-ttu-id="87734-397">回應</span><span class="sxs-lookup"><span data-stu-id="87734-397">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="87734-398">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="87734-398">localhost:1234</span></span>      | <span data-ttu-id="87734-399">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="87734-399">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="87734-400">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="87734-400">localhost:1234/map1</span></span> | <span data-ttu-id="87734-401">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="87734-401">Map Test 1</span></span>                   |
| <span data-ttu-id="87734-402">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="87734-402">localhost:1234/map2</span></span> | <span data-ttu-id="87734-403">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="87734-403">Map Test 2</span></span>                   |
| <span data-ttu-id="87734-404">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="87734-404">localhost:1234/map3</span></span> | <span data-ttu-id="87734-405">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="87734-405">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="87734-406">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="87734-406">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="87734-407"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="87734-407"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="87734-408">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="87734-408">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="87734-409">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="87734-409">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

<span data-ttu-id="87734-410">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="87734-410">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="87734-411">要求</span><span class="sxs-lookup"><span data-stu-id="87734-411">Request</span></span>                       | <span data-ttu-id="87734-412">回應</span><span class="sxs-lookup"><span data-stu-id="87734-412">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="87734-413">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="87734-413">localhost:1234</span></span>                | <span data-ttu-id="87734-414">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="87734-414">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="87734-415">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="87734-415">localhost:1234/?branch=master</span></span> | <span data-ttu-id="87734-416">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="87734-416">Branch used = master</span></span>         |

<span data-ttu-id="87734-417">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="87734-417">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="87734-418">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="87734-418">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="87734-419">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="87734-419">Built-in middleware</span></span>

<span data-ttu-id="87734-420">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="87734-420">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="87734-421">「順序」欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="87734-421">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="87734-422">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="87734-422">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="87734-423">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="87734-423">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="87734-424">中介軟體</span><span class="sxs-lookup"><span data-stu-id="87734-424">Middleware</span></span> | <span data-ttu-id="87734-425">描述</span><span class="sxs-lookup"><span data-stu-id="87734-425">Description</span></span> | <span data-ttu-id="87734-426">單</span><span class="sxs-lookup"><span data-stu-id="87734-426">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="87734-427">驗證</span><span class="sxs-lookup"><span data-stu-id="87734-427">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="87734-428">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="87734-428">Provides authentication support.</span></span> | <span data-ttu-id="87734-429">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="87734-429">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="87734-430">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="87734-430">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="87734-431">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="87734-431">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="87734-432">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="87734-432">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="87734-433">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="87734-433">Before middleware that issues cookies.</span></span> <span data-ttu-id="87734-434">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="87734-434">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="87734-435">CORS</span><span class="sxs-lookup"><span data-stu-id="87734-435">CORS</span></span>](xref:security/cors) | <span data-ttu-id="87734-436">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="87734-436">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="87734-437">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-437">Before components that use CORS.</span></span> |
| [<span data-ttu-id="87734-438">診斷</span><span class="sxs-lookup"><span data-stu-id="87734-438">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="87734-439">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87734-439">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="87734-440">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-440">Before components that generate errors.</span></span> <span data-ttu-id="87734-441">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="87734-441">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="87734-442">轉送標頭</span><span class="sxs-lookup"><span data-stu-id="87734-442">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="87734-443">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="87734-443">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="87734-444">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="87734-444">Before components that consume the updated fields.</span></span> <span data-ttu-id="87734-445">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="87734-445">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="87734-446">健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="87734-446">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="87734-447">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="87734-447">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="87734-448">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="87734-448">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="87734-449">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="87734-449">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="87734-450">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="87734-450">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="87734-451">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-451">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="87734-452">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="87734-452">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="87734-453">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="87734-453">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="87734-454">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-454">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="87734-455">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="87734-455">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="87734-456">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="87734-456">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="87734-457">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="87734-457">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="87734-458">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="87734-458">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="87734-459">MVC</span><span class="sxs-lookup"><span data-stu-id="87734-459">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="87734-460">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="87734-460">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="87734-461">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="87734-461">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="87734-462">OWIN</span><span class="sxs-lookup"><span data-stu-id="87734-462">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="87734-463">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="87734-463">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="87734-464">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="87734-464">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="87734-465">回應快取</span><span class="sxs-lookup"><span data-stu-id="87734-465">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="87734-466">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-466">Provides support for caching responses.</span></span> | <span data-ttu-id="87734-467">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-467">Before components that require caching.</span></span> |
| [<span data-ttu-id="87734-468">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="87734-468">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="87734-469">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-469">Provides support for compressing responses.</span></span> | <span data-ttu-id="87734-470">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-470">Before components that require compression.</span></span> |
| [<span data-ttu-id="87734-471">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="87734-471">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="87734-472">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="87734-472">Provides localization support.</span></span> | <span data-ttu-id="87734-473">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-473">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="87734-474">端點路由</span><span class="sxs-lookup"><span data-stu-id="87734-474">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="87734-475">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="87734-475">Defines and constrains request routes.</span></span> | <span data-ttu-id="87734-476">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="87734-476">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="87734-477">工作階段</span><span class="sxs-lookup"><span data-stu-id="87734-477">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="87734-478">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-478">Provides support for managing user sessions.</span></span> | <span data-ttu-id="87734-479">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-479">Before components that require Session.</span></span> |
| [<span data-ttu-id="87734-480">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="87734-480">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="87734-481">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="87734-481">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="87734-482">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="87734-482">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="87734-483">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="87734-483">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="87734-484">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="87734-484">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="87734-485">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-485">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="87734-486">WebSockets</span><span class="sxs-lookup"><span data-stu-id="87734-486">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="87734-487">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="87734-487">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="87734-488">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="87734-488">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="87734-489">其他資源</span><span class="sxs-lookup"><span data-stu-id="87734-489">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end
