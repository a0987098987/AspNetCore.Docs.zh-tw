---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/08/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: 8f5c3aabf17e78ae9675048602317c54f08e82a7
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259808"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="7410c-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="7410c-103">ASP.NET Core Middleware</span></span>

<span data-ttu-id="7410c-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="7410c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="7410c-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="7410c-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="7410c-106">每個元件：</span><span class="sxs-lookup"><span data-stu-id="7410c-106">Each component:</span></span>

* <span data-ttu-id="7410c-107">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="7410c-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="7410c-108">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="7410c-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="7410c-109">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="7410c-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="7410c-110">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="7410c-111">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7410c-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="7410c-112">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="7410c-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="7410c-113">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="7410c-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="7410c-114">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="7410c-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="7410c-115">當中介軟體短路時，稱為「終端中介軟體」，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="7410c-116"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="7410c-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="7410c-117">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="7410c-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="7410c-118">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="7410c-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="7410c-119">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="7410c-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="7410c-120">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="7410c-120">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="7410c-124">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="7410c-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="7410c-125">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="7410c-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="7410c-126">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="7410c-127">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="7410c-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="7410c-128">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="7410c-129">第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="7410c-129">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="7410c-130">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="7410c-130">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="7410c-131">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="7410c-131">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="7410c-132">您可以「不」呼叫「下一個」參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="7410c-132">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="7410c-133">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="7410c-133">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

<span data-ttu-id="7410c-134">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="7410c-134">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="7410c-135">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="7410c-135">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="7410c-136">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="7410c-136">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="7410c-137">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="7410c-137">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="7410c-138">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="7410c-138">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="7410c-139">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="7410c-139">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="7410c-140">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7410c-140">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="7410c-141">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7410c-141">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="7410c-142">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="7410c-142">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="7410c-143">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="7410c-143">May cause a protocol violation.</span></span> <span data-ttu-id="7410c-144">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="7410c-144">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="7410c-145">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="7410c-145">May corrupt the body format.</span></span> <span data-ttu-id="7410c-146">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="7410c-146">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="7410c-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="7410c-147"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

## <a name="order"></a><span data-ttu-id="7410c-148">順序</span><span class="sxs-lookup"><span data-stu-id="7410c-148">Order</span></span>

<span data-ttu-id="7410c-149">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="7410c-149">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="7410c-150">對安全性、效能與功能性而言，此順序相當重要。</span><span class="sxs-lookup"><span data-stu-id="7410c-150">The order is critical for security, performance, and functionality.</span></span>

<span data-ttu-id="7410c-151">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="7410c-151">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

::: moniker range=">= aspnetcore-3.0"

1. <span data-ttu-id="7410c-152">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="7410c-152">Exception/error handling</span></span>
   * <span data-ttu-id="7410c-153">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="7410c-153">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="7410c-154">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="7410c-154">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="7410c-155">資料庫錯誤頁面中介軟體會報告資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="7410c-155">Database Error Page Middleware reports database runtime errors.</span></span>
   * <span data-ttu-id="7410c-156">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="7410c-156">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="7410c-157">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7410c-157">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="7410c-158">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7410c-158">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="7410c-159">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7410c-159">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="7410c-160">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="7410c-160">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="7410c-161">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="7410c-161">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="7410c-162">路由中介軟體（`UseRouting`）以路由傳送要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-162">Routing Middleware (`UseRouting`) to route requests.</span></span>
1. <span data-ttu-id="7410c-163">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="7410c-163">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="7410c-164">授權中介軟體（`UseAuthorization`）會授權使用者存取安全的資源。</span><span class="sxs-lookup"><span data-stu-id="7410c-164">Authorization Middleware (`UseAuthorization`) authorizes a user to access secure resources.</span></span>
1. <span data-ttu-id="7410c-165">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="7410c-165">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="7410c-166">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7410c-166">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="7410c-167">端點路由中介軟體（`UseEndpoints` 加 `MapRazorPages`），以將 Razor Pages 端點新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="7410c-167">Endpoint Routing Middleware (`UseEndpoints` with `MapRazorPages`) to add Razor Pages endpoints to the request pipeline.</span></span>

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

<span data-ttu-id="7410c-168">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="7410c-168">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="7410c-169"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="7410c-169"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="7410c-170">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7410c-170">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="7410c-171">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="7410c-171">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="7410c-172">靜態檔案中介軟體**不會**執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="7410c-172">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="7410c-173">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="7410c-173">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="7410c-174">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="7410c-174">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="7410c-175">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="7410c-175">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="7410c-176">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="7410c-176">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="7410c-177">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="7410c-177">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="7410c-178">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="7410c-178">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="7410c-179">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="7410c-179">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="7410c-180">Razor Pages 回應可以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7410c-180">The Razor Pages responses can be compressed.</span></span>

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

::: moniker-end

::: moniker range="< aspnetcore-3.0"

1. <span data-ttu-id="7410c-181">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="7410c-181">Exception/error handling</span></span>
   * <span data-ttu-id="7410c-182">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="7410c-182">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="7410c-183">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="7410c-183">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="7410c-184">資料錯誤頁面中介軟體 (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) 會回報資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="7410c-184">Database Error Page Middleware (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) reports database runtime errors.</span></span>
   * <span data-ttu-id="7410c-185">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="7410c-185">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="7410c-186">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7410c-186">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="7410c-187">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7410c-187">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="7410c-188">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7410c-188">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="7410c-189">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="7410c-189">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="7410c-190">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="7410c-190">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="7410c-191">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="7410c-191">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="7410c-192">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="7410c-192">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="7410c-193">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7410c-193">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="7410c-194">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) 以將 MVC 新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="7410c-194">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) to add MVC to the request pipeline.</span></span>

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

<span data-ttu-id="7410c-195">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="7410c-195">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="7410c-196"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="7410c-196"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="7410c-197">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="7410c-197">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="7410c-198">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="7410c-198">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="7410c-199">靜態檔案中介軟體**不會**執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="7410c-199">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="7410c-200">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="7410c-200">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="7410c-201">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="7410c-201">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="7410c-202">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="7410c-202">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="7410c-203">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="7410c-203">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="7410c-204">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="7410c-204">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="7410c-205">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="7410c-205">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="7410c-206">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="7410c-206">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="7410c-207">來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。</span><span class="sxs-lookup"><span data-stu-id="7410c-207">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

::: moniker-end

## <a name="use-run-and-map"></a><span data-ttu-id="7410c-208">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="7410c-208">Use, Run, and Map</span></span>

<span data-ttu-id="7410c-209">設定 HTTP 管線的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 及 <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>。</span><span class="sxs-lookup"><span data-stu-id="7410c-209">Configure the HTTP pipeline using <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, and <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>.</span></span> <span data-ttu-id="7410c-210">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="7410c-210">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="7410c-211">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="7410c-211">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="7410c-212"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="7410c-212"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="7410c-213">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="7410c-213">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="7410c-214">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="7410c-214">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="7410c-215">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="7410c-215">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="7410c-216">要求</span><span class="sxs-lookup"><span data-stu-id="7410c-216">Request</span></span>             | <span data-ttu-id="7410c-217">回應</span><span class="sxs-lookup"><span data-stu-id="7410c-217">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="7410c-218">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="7410c-218">localhost:1234</span></span>      | <span data-ttu-id="7410c-219">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="7410c-219">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="7410c-220">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="7410c-220">localhost:1234/map1</span></span> | <span data-ttu-id="7410c-221">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="7410c-221">Map Test 1</span></span>                   |
| <span data-ttu-id="7410c-222">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="7410c-222">localhost:1234/map2</span></span> | <span data-ttu-id="7410c-223">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="7410c-223">Map Test 2</span></span>                   |
| <span data-ttu-id="7410c-224">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="7410c-224">localhost:1234/map3</span></span> | <span data-ttu-id="7410c-225">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="7410c-225">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="7410c-226">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="7410c-226">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="7410c-227"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="7410c-227"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="7410c-228">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="7410c-228">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="7410c-229">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="7410c-229">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

<span data-ttu-id="7410c-230">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="7410c-230">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="7410c-231">要求</span><span class="sxs-lookup"><span data-stu-id="7410c-231">Request</span></span>                       | <span data-ttu-id="7410c-232">回應</span><span class="sxs-lookup"><span data-stu-id="7410c-232">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="7410c-233">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="7410c-233">localhost:1234</span></span>                | <span data-ttu-id="7410c-234">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="7410c-234">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="7410c-235">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="7410c-235">localhost:1234/?branch=master</span></span> | <span data-ttu-id="7410c-236">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="7410c-236">Branch used = master</span></span>         |

<span data-ttu-id="7410c-237">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="7410c-237">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="7410c-238">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="7410c-238">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="7410c-239">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="7410c-239">Built-in middleware</span></span>

<span data-ttu-id="7410c-240">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="7410c-240">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="7410c-241">「順序」欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="7410c-241">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="7410c-242">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="7410c-242">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="7410c-243">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="7410c-243">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="7410c-244">中介軟體</span><span class="sxs-lookup"><span data-stu-id="7410c-244">Middleware</span></span> | <span data-ttu-id="7410c-245">描述</span><span class="sxs-lookup"><span data-stu-id="7410c-245">Description</span></span> | <span data-ttu-id="7410c-246">順序</span><span class="sxs-lookup"><span data-stu-id="7410c-246">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="7410c-247">驗證</span><span class="sxs-lookup"><span data-stu-id="7410c-247">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="7410c-248">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="7410c-248">Provides authentication support.</span></span> | <span data-ttu-id="7410c-249">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-249">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="7410c-250">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="7410c-250">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="7410c-251">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="7410c-251">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="7410c-252">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="7410c-252">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="7410c-253">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-253">Before middleware that issues cookies.</span></span> <span data-ttu-id="7410c-254">例如：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="7410c-254">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="7410c-255">CORS</span><span class="sxs-lookup"><span data-stu-id="7410c-255">CORS</span></span>](xref:security/cors) | <span data-ttu-id="7410c-256">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="7410c-256">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="7410c-257">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-257">Before components that use CORS.</span></span> |
| [<span data-ttu-id="7410c-258">診斷</span><span class="sxs-lookup"><span data-stu-id="7410c-258">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="7410c-259">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="7410c-259">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="7410c-260">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-260">Before components that generate errors.</span></span> <span data-ttu-id="7410c-261">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="7410c-261">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="7410c-262">轉送標頭</span><span class="sxs-lookup"><span data-stu-id="7410c-262">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="7410c-263">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-263">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="7410c-264">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-264">Before components that consume the updated fields.</span></span> <span data-ttu-id="7410c-265">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="7410c-265">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="7410c-266">健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="7410c-266">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="7410c-267">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="7410c-267">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="7410c-268">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="7410c-268">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="7410c-269">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="7410c-269">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="7410c-270">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="7410c-270">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="7410c-271">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-271">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="7410c-272">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="7410c-272">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="7410c-273">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="7410c-273">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="7410c-274">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-274">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="7410c-275">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="7410c-275">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="7410c-276">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="7410c-276">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="7410c-277">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="7410c-277">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="7410c-278">例如：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="7410c-278">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="7410c-279">MVC</span><span class="sxs-lookup"><span data-stu-id="7410c-279">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="7410c-280">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="7410c-280">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="7410c-281">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="7410c-281">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="7410c-282">OWIN</span><span class="sxs-lookup"><span data-stu-id="7410c-282">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="7410c-283">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="7410c-283">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="7410c-284">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="7410c-284">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="7410c-285">回應快取</span><span class="sxs-lookup"><span data-stu-id="7410c-285">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="7410c-286">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="7410c-286">Provides support for caching responses.</span></span> | <span data-ttu-id="7410c-287">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-287">Before components that require caching.</span></span> |
| [<span data-ttu-id="7410c-288">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="7410c-288">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="7410c-289">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="7410c-289">Provides support for compressing responses.</span></span> | <span data-ttu-id="7410c-290">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-290">Before components that require compression.</span></span> |
| [<span data-ttu-id="7410c-291">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="7410c-291">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="7410c-292">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="7410c-292">Provides localization support.</span></span> | <span data-ttu-id="7410c-293">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-293">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="7410c-294">端點路由</span><span class="sxs-lookup"><span data-stu-id="7410c-294">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="7410c-295">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="7410c-295">Defines and constrains request routes.</span></span> | <span data-ttu-id="7410c-296">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="7410c-296">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="7410c-297">工作階段</span><span class="sxs-lookup"><span data-stu-id="7410c-297">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="7410c-298">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="7410c-298">Provides support for managing user sessions.</span></span> | <span data-ttu-id="7410c-299">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-299">Before components that require Session.</span></span> |
| [<span data-ttu-id="7410c-300">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="7410c-300">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="7410c-301">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="7410c-301">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="7410c-302">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="7410c-302">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="7410c-303">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="7410c-303">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="7410c-304">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="7410c-304">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="7410c-305">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-305">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="7410c-306">WebSockets</span><span class="sxs-lookup"><span data-stu-id="7410c-306">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="7410c-307">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="7410c-307">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="7410c-308">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="7410c-308">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="7410c-309">其他資源</span><span class="sxs-lookup"><span data-stu-id="7410c-309">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>
