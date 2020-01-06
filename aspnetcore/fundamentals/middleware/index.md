---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/19/2019
uid: fundamentals/middleware/index
ms.openlocfilehash: 63566c1642e17ad333bb65b122330d11c4472aff
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355009"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="1b757-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="1b757-103">ASP.NET Core Middleware</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1b757-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="1b757-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1b757-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="1b757-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="1b757-106">每個元件：</span><span class="sxs-lookup"><span data-stu-id="1b757-106">Each component:</span></span>

* <span data-ttu-id="1b757-107">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="1b757-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="1b757-108">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="1b757-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="1b757-109">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="1b757-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="1b757-110">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="1b757-111">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="1b757-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="1b757-112">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="1b757-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="1b757-113">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="1b757-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="1b757-114">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="1b757-115">當中介軟體短路時，稱為「終端中介軟體」，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="1b757-116"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="1b757-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="1b757-117">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="1b757-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="1b757-118">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="1b757-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="1b757-119">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="1b757-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="1b757-120">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="1b757-120">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="1b757-124">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="1b757-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="1b757-125">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="1b757-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="1b757-126">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="1b757-127">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="1b757-128">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="1b757-129">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="1b757-129">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="1b757-130">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="1b757-130">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="1b757-131">您可以「不」呼叫「下一個」參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-131">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="1b757-132">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="1b757-132">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=5-10)]

<span data-ttu-id="1b757-133">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="1b757-133">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="1b757-134">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="1b757-134">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="1b757-135">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="1b757-135">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="1b757-136">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="1b757-136">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="1b757-137">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="1b757-137">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="1b757-138">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="1b757-138">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="1b757-139">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-139">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="1b757-140">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-140">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="1b757-141">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="1b757-141">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="1b757-142">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="1b757-142">May cause a protocol violation.</span></span> <span data-ttu-id="1b757-143">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="1b757-143">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="1b757-144">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="1b757-144">May corrupt the body format.</span></span> <span data-ttu-id="1b757-145">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="1b757-145">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="1b757-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="1b757-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<span data-ttu-id="1b757-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派不會收到 `next` 參數。</span><span class="sxs-lookup"><span data-stu-id="1b757-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegates don't receive a `next` parameter.</span></span> <span data-ttu-id="1b757-148">第一個 `Run` 委派一律是 terminal 並終止管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-148">The first `Run` delegate is always terminal and terminates the pipeline.</span></span> <span data-ttu-id="1b757-149">`Run` 是慣例。</span><span class="sxs-lookup"><span data-stu-id="1b757-149">`Run` is a convention.</span></span> <span data-ttu-id="1b757-150">某些中介軟體元件可能會公開在管線結尾處執行 `Run[Middleware]` 方法：</span><span class="sxs-lookup"><span data-stu-id="1b757-150">Some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=12-15)]

<span data-ttu-id="1b757-151">在上述範例中，`Run` 委派會將 `"Hello from 2nd delegate."` 寫入至回應，然後終止管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-151">In the preceding example, the `Run` delegate writes `"Hello from 2nd delegate."` to the response and then terminates the pipeline.</span></span> <span data-ttu-id="1b757-152">如果在 `Run` 委派後面加入另一個 `Use` 或 `Run` 委派，則不會呼叫它。</span><span class="sxs-lookup"><span data-stu-id="1b757-152">If another `Use` or `Run` delegate is added after the `Run` delegate, it's not called.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="1b757-153">中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="1b757-153">Middleware order</span></span>

<span data-ttu-id="1b757-154">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="1b757-154">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="1b757-155">順序對於安全性、效能和功能非常**重要**。</span><span class="sxs-lookup"><span data-stu-id="1b757-155">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="1b757-156">下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="1b757-156">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

<span data-ttu-id="1b757-157">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="1b757-157">In the preceding code:</span></span>

* <span data-ttu-id="1b757-158">使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。</span><span class="sxs-lookup"><span data-stu-id="1b757-158">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="1b757-159">並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。</span><span class="sxs-lookup"><span data-stu-id="1b757-159">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="1b757-160">例如，`UseCors`、`UseAuthentication`和 `UseAuthorization` 必須以顯示的循序執行。</span><span class="sxs-lookup"><span data-stu-id="1b757-160">For example, `UseCors`, `UseAuthentication`, and `UseAuthorization` must go in the order shown.</span></span>

<span data-ttu-id="1b757-161">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="1b757-161">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="1b757-162">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="1b757-162">Exception/error handling</span></span>
   * <span data-ttu-id="1b757-163">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="1b757-163">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="1b757-164">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="1b757-164">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="1b757-165">資料庫錯誤頁面中介軟體會報告資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="1b757-165">Database Error Page Middleware reports database runtime errors.</span></span>
   * <span data-ttu-id="1b757-166">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="1b757-166">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="1b757-167">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-167">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="1b757-168">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1b757-168">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="1b757-169">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="1b757-169">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="1b757-170">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="1b757-170">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="1b757-171">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="1b757-171">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="1b757-172">路由中介軟體（`UseRouting`）以路由傳送要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-172">Routing Middleware (`UseRouting`) to route requests.</span></span>
1. <span data-ttu-id="1b757-173">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1b757-173">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="1b757-174">授權中介軟體（`UseAuthorization`）會授權使用者存取安全的資源。</span><span class="sxs-lookup"><span data-stu-id="1b757-174">Authorization Middleware (`UseAuthorization`) authorizes a user to access secure resources.</span></span>
1. <span data-ttu-id="1b757-175">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="1b757-175">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="1b757-176">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1b757-176">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="1b757-177">端點路由中介軟體（使用 `MapRazorPages``UseEndpoints`），以將 Razor Pages 端點新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-177">Endpoint Routing Middleware (`UseEndpoints` with `MapRazorPages`) to add Razor Pages endpoints to the request pipeline.</span></span>

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

<span data-ttu-id="1b757-178">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="1b757-178">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="1b757-179"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="1b757-179"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="1b757-180">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-180">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="1b757-181">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-181">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="1b757-182">靜態檔案中介軟體**不會**執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="1b757-182">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="1b757-183">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="1b757-183">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="1b757-184">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="1b757-184">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="1b757-185">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="1b757-185">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="1b757-186">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-186">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="1b757-187">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="1b757-187">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="1b757-188">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="1b757-188">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="1b757-189">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="1b757-189">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="1b757-190">Razor Pages 回應可以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="1b757-190">The Razor Pages responses can be compressed.</span></span>

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

## <a name="branch-the-middleware-pipeline"></a><span data-ttu-id="1b757-191">分支中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="1b757-191">Branch the middleware pipeline</span></span>

<span data-ttu-id="1b757-192"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="1b757-192"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="1b757-193">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-193">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="1b757-194">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-194">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="1b757-195">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="1b757-195">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="1b757-196">要求</span><span class="sxs-lookup"><span data-stu-id="1b757-196">Request</span></span>             | <span data-ttu-id="1b757-197">回應</span><span class="sxs-lookup"><span data-stu-id="1b757-197">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="1b757-198">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="1b757-198">localhost:1234</span></span>      | <span data-ttu-id="1b757-199">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="1b757-199">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="1b757-200">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="1b757-200">localhost:1234/map1</span></span> | <span data-ttu-id="1b757-201">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="1b757-201">Map Test 1</span></span>                   |
| <span data-ttu-id="1b757-202">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="1b757-202">localhost:1234/map2</span></span> | <span data-ttu-id="1b757-203">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="1b757-203">Map Test 2</span></span>                   |
| <span data-ttu-id="1b757-204">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="1b757-204">localhost:1234/map3</span></span> | <span data-ttu-id="1b757-205">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="1b757-205">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="1b757-206">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="1b757-206">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="1b757-207">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="1b757-207">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="1b757-208">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="1b757-208">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

<span data-ttu-id="1b757-209"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-209"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="1b757-210">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-210">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="1b757-211">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="1b757-211">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?highlight=14-15)]

<span data-ttu-id="1b757-212">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="1b757-212">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="1b757-213">要求</span><span class="sxs-lookup"><span data-stu-id="1b757-213">Request</span></span>                       | <span data-ttu-id="1b757-214">回應</span><span class="sxs-lookup"><span data-stu-id="1b757-214">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="1b757-215">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="1b757-215">localhost:1234</span></span>                | <span data-ttu-id="1b757-216">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="1b757-216">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="1b757-217">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="1b757-217">localhost:1234/?branch=master</span></span> | <span data-ttu-id="1b757-218">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="1b757-218">Branch used = master</span></span>         |

<span data-ttu-id="1b757-219"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> 也會根據給定述詞的結果來分支要求管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-219"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> also branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="1b757-220">與 `MapWhen`不同的是，如果此分支進行短路或包含終端機中介軟體，則會重新加入主要管線：</span><span class="sxs-lookup"><span data-stu-id="1b757-220">Unlike with `MapWhen`, this branch is rejoined to the main pipeline if it does short-circuit or contain a terminal middleware:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupUseWhen.cs?highlight=23-24)]

<span data-ttu-id="1b757-221">在上述範例中，回應為「來自主要管線的 Hello」。</span><span class="sxs-lookup"><span data-stu-id="1b757-221">In the preceding example, a response of "Hello from main pipeline."</span></span> <span data-ttu-id="1b757-222">會針對所有要求寫入。</span><span class="sxs-lookup"><span data-stu-id="1b757-222">is written for all requests.</span></span> <span data-ttu-id="1b757-223">如果要求中包含查詢字串變數 `branch`，則會在重新加入主要管線之前記錄其值。</span><span class="sxs-lookup"><span data-stu-id="1b757-223">If the request includes a query string variable `branch`, its value is logged before the main pipeline is rejoined.</span></span>

## <a name="built-in-middleware"></a><span data-ttu-id="1b757-224">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="1b757-224">Built-in middleware</span></span>

<span data-ttu-id="1b757-225">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="1b757-225">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="1b757-226">「順序」欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="1b757-226">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="1b757-227">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="1b757-227">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="1b757-228">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="1b757-228">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="1b757-229">中介軟體</span><span class="sxs-lookup"><span data-stu-id="1b757-229">Middleware</span></span> | <span data-ttu-id="1b757-230">描述</span><span class="sxs-lookup"><span data-stu-id="1b757-230">Description</span></span> | <span data-ttu-id="1b757-231">使用</span><span class="sxs-lookup"><span data-stu-id="1b757-231">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="1b757-232">驗證</span><span class="sxs-lookup"><span data-stu-id="1b757-232">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="1b757-233">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-233">Provides authentication support.</span></span> | <span data-ttu-id="1b757-234">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-234">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="1b757-235">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="1b757-235">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="1b757-236">授權</span><span class="sxs-lookup"><span data-stu-id="1b757-236">Authorization</span></span>](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*) | <span data-ttu-id="1b757-237">提供授權支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-237">Provides authorization support.</span></span> | <span data-ttu-id="1b757-238">緊接在驗證中介軟體之後。</span><span class="sxs-lookup"><span data-stu-id="1b757-238">Immediately after the Authentication Middleware.</span></span> |
| [<span data-ttu-id="1b757-239">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="1b757-239">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="1b757-240">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="1b757-240">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="1b757-241">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-241">Before middleware that issues cookies.</span></span> <span data-ttu-id="1b757-242">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="1b757-242">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="1b757-243">CORS</span><span class="sxs-lookup"><span data-stu-id="1b757-243">CORS</span></span>](xref:security/cors) | <span data-ttu-id="1b757-244">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="1b757-244">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="1b757-245">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-245">Before components that use CORS.</span></span> |
| [<span data-ttu-id="1b757-246">診斷</span><span class="sxs-lookup"><span data-stu-id="1b757-246">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="1b757-247">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1b757-247">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="1b757-248">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-248">Before components that generate errors.</span></span> <span data-ttu-id="1b757-249">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="1b757-249">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="1b757-250">轉送標頭</span><span class="sxs-lookup"><span data-stu-id="1b757-250">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="1b757-251">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-251">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="1b757-252">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-252">Before components that consume the updated fields.</span></span> <span data-ttu-id="1b757-253">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="1b757-253">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="1b757-254">健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1b757-254">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="1b757-255">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="1b757-255">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="1b757-256">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="1b757-256">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="1b757-257">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="1b757-257">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="1b757-258">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="1b757-258">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="1b757-259">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-259">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="1b757-260">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="1b757-260">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="1b757-261">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="1b757-261">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="1b757-262">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-262">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="1b757-263">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="1b757-263">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="1b757-264">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="1b757-264">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="1b757-265">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="1b757-265">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="1b757-266">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="1b757-266">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="1b757-267">MVC</span><span class="sxs-lookup"><span data-stu-id="1b757-267">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="1b757-268">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-268">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="1b757-269">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="1b757-269">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="1b757-270">OWIN</span><span class="sxs-lookup"><span data-stu-id="1b757-270">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="1b757-271">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="1b757-271">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="1b757-272">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="1b757-272">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="1b757-273">回應快取</span><span class="sxs-lookup"><span data-stu-id="1b757-273">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="1b757-274">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-274">Provides support for caching responses.</span></span> | <span data-ttu-id="1b757-275">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-275">Before components that require caching.</span></span> |
| [<span data-ttu-id="1b757-276">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="1b757-276">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="1b757-277">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-277">Provides support for compressing responses.</span></span> | <span data-ttu-id="1b757-278">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-278">Before components that require compression.</span></span> |
| [<span data-ttu-id="1b757-279">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="1b757-279">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="1b757-280">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-280">Provides localization support.</span></span> | <span data-ttu-id="1b757-281">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-281">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="1b757-282">端點路由</span><span class="sxs-lookup"><span data-stu-id="1b757-282">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="1b757-283">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="1b757-283">Defines and constrains request routes.</span></span> | <span data-ttu-id="1b757-284">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="1b757-284">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="1b757-285">Session</span><span class="sxs-lookup"><span data-stu-id="1b757-285">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="1b757-286">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-286">Provides support for managing user sessions.</span></span> | <span data-ttu-id="1b757-287">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-287">Before components that require Session.</span></span> |
| [<span data-ttu-id="1b757-288">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="1b757-288">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="1b757-289">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="1b757-289">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="1b757-290">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="1b757-290">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="1b757-291">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="1b757-291">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="1b757-292">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-292">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="1b757-293">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-293">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="1b757-294">WebSockets</span><span class="sxs-lookup"><span data-stu-id="1b757-294">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="1b757-295">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="1b757-295">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="1b757-296">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-296">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="1b757-297">其他資源</span><span class="sxs-lookup"><span data-stu-id="1b757-297">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1b757-298">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="1b757-298">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="1b757-299">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="1b757-299">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="1b757-300">每個元件：</span><span class="sxs-lookup"><span data-stu-id="1b757-300">Each component:</span></span>

* <span data-ttu-id="1b757-301">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="1b757-301">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="1b757-302">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="1b757-302">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="1b757-303">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="1b757-303">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="1b757-304">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-304">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="1b757-305">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="1b757-305">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="1b757-306">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="1b757-306">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="1b757-307">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」，也稱為「中介軟體元件」。</span><span class="sxs-lookup"><span data-stu-id="1b757-307">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="1b757-308">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-308">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="1b757-309">當中介軟體短路時，稱為「終端中介軟體」，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-309">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="1b757-310"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="1b757-310"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="1b757-311">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="1b757-311">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="1b757-312">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="1b757-312">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="1b757-313">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="1b757-313">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="1b757-314">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="1b757-314">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="1b757-318">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="1b757-318">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="1b757-319">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="1b757-319">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="1b757-320">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-320">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="1b757-321">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-321">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="1b757-322">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-322">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="1b757-323">第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-323">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="1b757-324">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="1b757-324">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="1b757-325">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="1b757-325">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="1b757-326">您可以「不」呼叫「下一個」參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-326">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="1b757-327">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="1b757-327">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

<span data-ttu-id="1b757-328">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="1b757-328">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="1b757-329">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="1b757-329">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="1b757-330">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="1b757-330">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="1b757-331">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="1b757-331">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="1b757-332">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="1b757-332">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="1b757-333">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="1b757-333">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="1b757-334">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-334">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="1b757-335">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-335">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="1b757-336">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="1b757-336">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="1b757-337">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="1b757-337">May cause a protocol violation.</span></span> <span data-ttu-id="1b757-338">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="1b757-338">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="1b757-339">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="1b757-339">May corrupt the body format.</span></span> <span data-ttu-id="1b757-340">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="1b757-340">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="1b757-341"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="1b757-341"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="1b757-342">中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="1b757-342">Middleware order</span></span>

<span data-ttu-id="1b757-343">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="1b757-343">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="1b757-344">順序對於安全性、效能和功能非常**重要**。</span><span class="sxs-lookup"><span data-stu-id="1b757-344">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="1b757-345">下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="1b757-345">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

<span data-ttu-id="1b757-346">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="1b757-346">In the preceding code:</span></span>

* <span data-ttu-id="1b757-347">使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。</span><span class="sxs-lookup"><span data-stu-id="1b757-347">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="1b757-348">並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。</span><span class="sxs-lookup"><span data-stu-id="1b757-348">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="1b757-349">例如，`UseCors` 和 `UseAuthentication` 必須以顯示的循序執行。</span><span class="sxs-lookup"><span data-stu-id="1b757-349">For example, `UseCors` and `UseAuthentication` must go in the order shown.</span></span>

<span data-ttu-id="1b757-350">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="1b757-350">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="1b757-351">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="1b757-351">Exception/error handling</span></span>
   * <span data-ttu-id="1b757-352">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="1b757-352">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="1b757-353">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="1b757-353">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="1b757-354">資料錯誤頁面中介軟體 (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) 會回報資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="1b757-354">Database Error Page Middleware (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) reports database runtime errors.</span></span>
   * <span data-ttu-id="1b757-355">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="1b757-355">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="1b757-356">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-356">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="1b757-357">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="1b757-357">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="1b757-358">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="1b757-358">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="1b757-359">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="1b757-359">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="1b757-360">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="1b757-360">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="1b757-361">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="1b757-361">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="1b757-362">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="1b757-362">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="1b757-363">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1b757-363">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="1b757-364">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) 以將 MVC 新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="1b757-364">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) to add MVC to the request pipeline.</span></span>

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

<span data-ttu-id="1b757-365">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="1b757-365">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="1b757-366"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="1b757-366"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="1b757-367">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="1b757-367">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="1b757-368">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-368">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="1b757-369">靜態檔案中介軟體**不會**執行授權檢查。</span><span class="sxs-lookup"><span data-stu-id="1b757-369">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="1b757-370">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="1b757-370">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="1b757-371">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="1b757-371">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="1b757-372">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="1b757-372">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="1b757-373">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="1b757-373">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="1b757-374">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="1b757-374">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="1b757-375">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="1b757-375">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="1b757-376">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="1b757-376">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="1b757-377">來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。</span><span class="sxs-lookup"><span data-stu-id="1b757-377">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a><span data-ttu-id="1b757-378">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="1b757-378">Use, Run, and Map</span></span>

<span data-ttu-id="1b757-379">設定 HTTP 管線的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 及 <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>。</span><span class="sxs-lookup"><span data-stu-id="1b757-379">Configure the HTTP pipeline using <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, and <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>.</span></span> <span data-ttu-id="1b757-380">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="1b757-380">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="1b757-381">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="1b757-381">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="1b757-382"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="1b757-382"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="1b757-383">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-383">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="1b757-384">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-384">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="1b757-385">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="1b757-385">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="1b757-386">要求</span><span class="sxs-lookup"><span data-stu-id="1b757-386">Request</span></span>             | <span data-ttu-id="1b757-387">回應</span><span class="sxs-lookup"><span data-stu-id="1b757-387">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="1b757-388">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="1b757-388">localhost:1234</span></span>      | <span data-ttu-id="1b757-389">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="1b757-389">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="1b757-390">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="1b757-390">localhost:1234/map1</span></span> | <span data-ttu-id="1b757-391">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="1b757-391">Map Test 1</span></span>                   |
| <span data-ttu-id="1b757-392">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="1b757-392">localhost:1234/map2</span></span> | <span data-ttu-id="1b757-393">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="1b757-393">Map Test 2</span></span>                   |
| <span data-ttu-id="1b757-394">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="1b757-394">localhost:1234/map3</span></span> | <span data-ttu-id="1b757-395">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="1b757-395">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="1b757-396">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="1b757-396">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="1b757-397"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-397"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="1b757-398">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="1b757-398">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="1b757-399">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="1b757-399">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

<span data-ttu-id="1b757-400">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="1b757-400">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="1b757-401">要求</span><span class="sxs-lookup"><span data-stu-id="1b757-401">Request</span></span>                       | <span data-ttu-id="1b757-402">回應</span><span class="sxs-lookup"><span data-stu-id="1b757-402">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="1b757-403">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="1b757-403">localhost:1234</span></span>                | <span data-ttu-id="1b757-404">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="1b757-404">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="1b757-405">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="1b757-405">localhost:1234/?branch=master</span></span> | <span data-ttu-id="1b757-406">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="1b757-406">Branch used = master</span></span>         |

<span data-ttu-id="1b757-407">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="1b757-407">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="1b757-408">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="1b757-408">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="1b757-409">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="1b757-409">Built-in middleware</span></span>

<span data-ttu-id="1b757-410">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="1b757-410">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="1b757-411">「順序」欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="1b757-411">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="1b757-412">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」。</span><span class="sxs-lookup"><span data-stu-id="1b757-412">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="1b757-413">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="1b757-413">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="1b757-414">中介軟體</span><span class="sxs-lookup"><span data-stu-id="1b757-414">Middleware</span></span> | <span data-ttu-id="1b757-415">描述</span><span class="sxs-lookup"><span data-stu-id="1b757-415">Description</span></span> | <span data-ttu-id="1b757-416">使用</span><span class="sxs-lookup"><span data-stu-id="1b757-416">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="1b757-417">驗證</span><span class="sxs-lookup"><span data-stu-id="1b757-417">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="1b757-418">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-418">Provides authentication support.</span></span> | <span data-ttu-id="1b757-419">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-419">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="1b757-420">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="1b757-420">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="1b757-421">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="1b757-421">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="1b757-422">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="1b757-422">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="1b757-423">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-423">Before middleware that issues cookies.</span></span> <span data-ttu-id="1b757-424">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="1b757-424">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="1b757-425">CORS</span><span class="sxs-lookup"><span data-stu-id="1b757-425">CORS</span></span>](xref:security/cors) | <span data-ttu-id="1b757-426">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="1b757-426">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="1b757-427">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-427">Before components that use CORS.</span></span> |
| [<span data-ttu-id="1b757-428">診斷</span><span class="sxs-lookup"><span data-stu-id="1b757-428">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="1b757-429">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="1b757-429">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="1b757-430">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-430">Before components that generate errors.</span></span> <span data-ttu-id="1b757-431">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="1b757-431">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="1b757-432">轉送標頭</span><span class="sxs-lookup"><span data-stu-id="1b757-432">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="1b757-433">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-433">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="1b757-434">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-434">Before components that consume the updated fields.</span></span> <span data-ttu-id="1b757-435">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="1b757-435">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="1b757-436">健康狀態檢查</span><span class="sxs-lookup"><span data-stu-id="1b757-436">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="1b757-437">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="1b757-437">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="1b757-438">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="1b757-438">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="1b757-439">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="1b757-439">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="1b757-440">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="1b757-440">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="1b757-441">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-441">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="1b757-442">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="1b757-442">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="1b757-443">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="1b757-443">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="1b757-444">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-444">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="1b757-445">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="1b757-445">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="1b757-446">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="1b757-446">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="1b757-447">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="1b757-447">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="1b757-448">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="1b757-448">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="1b757-449">MVC</span><span class="sxs-lookup"><span data-stu-id="1b757-449">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="1b757-450">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="1b757-450">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="1b757-451">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="1b757-451">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="1b757-452">OWIN</span><span class="sxs-lookup"><span data-stu-id="1b757-452">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="1b757-453">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="1b757-453">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="1b757-454">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="1b757-454">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="1b757-455">回應快取</span><span class="sxs-lookup"><span data-stu-id="1b757-455">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="1b757-456">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-456">Provides support for caching responses.</span></span> | <span data-ttu-id="1b757-457">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-457">Before components that require caching.</span></span> |
| [<span data-ttu-id="1b757-458">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="1b757-458">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="1b757-459">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-459">Provides support for compressing responses.</span></span> | <span data-ttu-id="1b757-460">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-460">Before components that require compression.</span></span> |
| [<span data-ttu-id="1b757-461">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="1b757-461">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="1b757-462">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-462">Provides localization support.</span></span> | <span data-ttu-id="1b757-463">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-463">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="1b757-464">端點路由</span><span class="sxs-lookup"><span data-stu-id="1b757-464">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="1b757-465">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="1b757-465">Defines and constrains request routes.</span></span> | <span data-ttu-id="1b757-466">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="1b757-466">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="1b757-467">Session</span><span class="sxs-lookup"><span data-stu-id="1b757-467">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="1b757-468">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-468">Provides support for managing user sessions.</span></span> | <span data-ttu-id="1b757-469">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-469">Before components that require Session.</span></span> |
| [<span data-ttu-id="1b757-470">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="1b757-470">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="1b757-471">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="1b757-471">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="1b757-472">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="1b757-472">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="1b757-473">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="1b757-473">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="1b757-474">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="1b757-474">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="1b757-475">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-475">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="1b757-476">WebSockets</span><span class="sxs-lookup"><span data-stu-id="1b757-476">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="1b757-477">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="1b757-477">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="1b757-478">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="1b757-478">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="1b757-479">其他資源</span><span class="sxs-lookup"><span data-stu-id="1b757-479">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end
