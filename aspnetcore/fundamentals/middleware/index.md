---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 5/6/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/middleware/index
ms.openlocfilehash: 69c253171c51e08802b82415245a66921168ec80
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85404258"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="a0813-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="a0813-103">ASP.NET Core Middleware</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a0813-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="a0813-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a0813-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="a0813-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="a0813-106">每個元件：</span><span class="sxs-lookup"><span data-stu-id="a0813-106">Each component:</span></span>

* <span data-ttu-id="a0813-107">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="a0813-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="a0813-108">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="a0813-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="a0813-109">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="a0813-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="a0813-110">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="a0813-111">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a0813-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="a0813-112">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="a0813-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="a0813-113">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」**，也稱為「中介軟體元件」**。</span><span class="sxs-lookup"><span data-stu-id="a0813-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="a0813-114">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="a0813-115">當中介軟體短路時，稱為「終端中介軟體」\*\*，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="a0813-116"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="a0813-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="a0813-117">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="a0813-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="a0813-118">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="a0813-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="a0813-119">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="a0813-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="a0813-120">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="a0813-120">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="a0813-124">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="a0813-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="a0813-125">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="a0813-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="a0813-126">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="a0813-127">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="a0813-128">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="a0813-129">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="a0813-129">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="a0813-130">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="a0813-130">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="a0813-131">您可以「不」\*\* 呼叫「下一個」\*\* 參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-131">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="a0813-132">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a0813-132">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=5-10)]

<span data-ttu-id="a0813-133">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="a0813-133">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="a0813-134">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="a0813-134">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="a0813-135">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="a0813-135">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="a0813-136">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="a0813-136">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="a0813-137">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="a0813-137">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="a0813-138">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="a0813-138">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="a0813-139">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-139">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="a0813-140">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-140">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="a0813-141">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="a0813-141">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="a0813-142">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="a0813-142">May cause a protocol violation.</span></span> <span data-ttu-id="a0813-143">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a0813-143">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="a0813-144">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="a0813-144">May corrupt the body format.</span></span> <span data-ttu-id="a0813-145">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="a0813-145">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="a0813-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="a0813-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<span data-ttu-id="a0813-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>委派不會收到 `next` 參數。</span><span class="sxs-lookup"><span data-stu-id="a0813-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegates don't receive a `next` parameter.</span></span> <span data-ttu-id="a0813-148">第一個 `Run` 委派一律是 terminal，並終止管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-148">The first `Run` delegate is always terminal and terminates the pipeline.</span></span> <span data-ttu-id="a0813-149">`Run`是慣例。</span><span class="sxs-lookup"><span data-stu-id="a0813-149">`Run` is a convention.</span></span> <span data-ttu-id="a0813-150">某些中介軟體元件可能會公開在 `Run[Middleware]` 管線結束時執行的方法：</span><span class="sxs-lookup"><span data-stu-id="a0813-150">Some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=12-15)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="a0813-151">在上述範例中， `Run` 委派會寫入 `"Hello from 2nd delegate."` 至回應，然後終止管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-151">In the preceding example, the `Run` delegate writes `"Hello from 2nd delegate."` to the response and then terminates the pipeline.</span></span> <span data-ttu-id="a0813-152">如果 `Use` `Run` 在委派之後加入另一個或委派 `Run` ，則不會呼叫它。</span><span class="sxs-lookup"><span data-stu-id="a0813-152">If another `Use` or `Run` delegate is added after the `Run` delegate, it's not called.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="a0813-153">中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="a0813-153">Middleware order</span></span>

<span data-ttu-id="a0813-154">下圖顯示 ASP.NET Core MVC 和頁面應用程式的完整要求處理管線 Razor 。</span><span class="sxs-lookup"><span data-stu-id="a0813-154">The following diagram shows the complete request processing pipeline for ASP.NET Core MVC and Razor Pages apps.</span></span> <span data-ttu-id="a0813-155">您可以查看在一般應用程式中，如何排序現有的中介軟體，以及新增自訂中介軟體的位置。</span><span class="sxs-lookup"><span data-stu-id="a0813-155">You can see how, in a typical app, existing middlewares are ordered and where custom middlewares are added.</span></span> <span data-ttu-id="a0813-156">您可以完整控制如何重新排序現有的中介軟體，或在您的案例中插入新的自訂中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a0813-156">You have full control over how to reorder existing middlewares or inject new custom middlewares as necessary for your scenarios.</span></span>

![ASP.NET Core 中介軟體管線](index/_static/middleware-pipeline.svg)

<span data-ttu-id="a0813-158">上圖中的**端點**中介軟體會執行對應應用程式類型 &mdash; MVC 或頁面的篩選準則管線 Razor 。</span><span class="sxs-lookup"><span data-stu-id="a0813-158">The **Endpoint** middleware in the preceding diagram executes the filter pipeline for the corresponding app type&mdash;MVC or Razor Pages.</span></span>

![ASP.NET Core 篩選器管線](index/_static/mvc-endpoint.svg)

<span data-ttu-id="a0813-160">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="a0813-160">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="a0813-161">順序對於安全性、效能和功能非常**重要**。</span><span class="sxs-lookup"><span data-stu-id="a0813-161">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="a0813-162">下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="a0813-162">The following `Startup.Configure` method adds security-related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

<span data-ttu-id="a0813-163">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="a0813-163">In the preceding code:</span></span>

* <span data-ttu-id="a0813-164">使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。</span><span class="sxs-lookup"><span data-stu-id="a0813-164">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="a0813-165">並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。</span><span class="sxs-lookup"><span data-stu-id="a0813-165">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="a0813-166">例如：</span><span class="sxs-lookup"><span data-stu-id="a0813-166">For example:</span></span>
  * <span data-ttu-id="a0813-167">`UseCors`、 `UseAuthentication` 和 `UseAuthorization` 必須以顯示的循序執行。</span><span class="sxs-lookup"><span data-stu-id="a0813-167">`UseCors`, `UseAuthentication`, and `UseAuthorization` must go in the order shown.</span></span>
  * <span data-ttu-id="a0813-168">`UseCors`目前必須在 `UseResponseCaching` [此 bug](https://github.com/dotnet/aspnetcore/issues/23218)之前進行。</span><span class="sxs-lookup"><span data-stu-id="a0813-168">`UseCors` currently must go before `UseResponseCaching` due to [this bug](https://github.com/dotnet/aspnetcore/issues/23218).</span></span>

<span data-ttu-id="a0813-169">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="a0813-169">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="a0813-170">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="a0813-170">Exception/error handling</span></span>
   * <span data-ttu-id="a0813-171">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="a0813-171">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="a0813-172">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="a0813-172">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="a0813-173">資料庫錯誤頁面中介軟體會報告資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="a0813-173">Database Error Page Middleware reports database runtime errors.</span></span>
   * <span data-ttu-id="a0813-174">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="a0813-174">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="a0813-175">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-175">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="a0813-176">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="a0813-176">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="a0813-177">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a0813-177">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="a0813-178">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="a0813-178">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="a0813-179">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="a0813-179">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="a0813-180">路由中介軟體（ `UseRouting` ）以路由傳送要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-180">Routing Middleware (`UseRouting`) to route requests.</span></span>
1. <span data-ttu-id="a0813-181">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="a0813-181">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="a0813-182">授權中介軟體（ `UseAuthorization` ）會授權使用者存取安全的資源。</span><span class="sxs-lookup"><span data-stu-id="a0813-182">Authorization Middleware (`UseAuthorization`) authorizes a user to access secure resources.</span></span>
1. <span data-ttu-id="a0813-183">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="a0813-183">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="a0813-184">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a0813-184">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="a0813-185">端點路由中介軟體（ `UseEndpoints` 含 `MapRazorPages` ），以將 Razor 頁面端點新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-185">Endpoint Routing Middleware (`UseEndpoints` with `MapRazorPages`) to add Razor Pages endpoints to the request pipeline.</span></span>

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

<span data-ttu-id="a0813-186">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="a0813-186">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="a0813-187"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="a0813-187"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="a0813-188">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-188">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="a0813-189">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-189">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="a0813-190">靜態檔案中介軟體**不**提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="a0813-190">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="a0813-191">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="a0813-191">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="a0813-192">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="a0813-192">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="a0813-193">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="a0813-193">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="a0813-194">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-194">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a0813-195">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器和動作之後，才會進行授權（和拒絕）。</span><span class="sxs-lookup"><span data-stu-id="a0813-195">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="a0813-196">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="a0813-196">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="a0813-197">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="a0813-197">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="a0813-198">Razor頁面回應可以壓縮。</span><span class="sxs-lookup"><span data-stu-id="a0813-198">The Razor Pages responses can be compressed.</span></span>

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

<span data-ttu-id="a0813-199">對於單一頁面應用程式（Spa），SPA 中介軟體 <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> 通常是在中介軟體管線中的最後一個。</span><span class="sxs-lookup"><span data-stu-id="a0813-199">For Single Page Applications (SPAs), the SPA middleware <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> usually comes last in the middleware pipeline.</span></span> <span data-ttu-id="a0813-200">SPA 中介軟體最後一步：</span><span class="sxs-lookup"><span data-stu-id="a0813-200">The SPA middleware comes last:</span></span>

* <span data-ttu-id="a0813-201">以允許所有其他中介軟體先回應相符的要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-201">To allow all other middlewares to respond to matching requests first.</span></span>
* <span data-ttu-id="a0813-202">允許 Spa 與用戶端路由針對伺服器應用程式無法辨識的所有路由執行。</span><span class="sxs-lookup"><span data-stu-id="a0813-202">To allow SPAs with client-side routing to run for all routes that are unrecognized by the server app.</span></span>

<span data-ttu-id="a0813-203">如需 Spa 的詳細資訊，請參閱[回應](xref:spa/react)和[角度](xref:spa/angular)專案範本的指南。</span><span class="sxs-lookup"><span data-stu-id="a0813-203">For more details on SPAs, see the guides for the [React](xref:spa/react) and [Angular](xref:spa/angular) project templates.</span></span>

### <a name="forwarded-headers-middleware-order"></a><span data-ttu-id="a0813-204">轉送的標頭中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="a0813-204">Forwarded Headers Middleware order</span></span>

[!INCLUDE[](~/includes/ForwardedHeaders.md)]

## <a name="branch-the-middleware-pipeline"></a><span data-ttu-id="a0813-205">分支中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="a0813-205">Branch the middleware pipeline</span></span>

<span data-ttu-id="a0813-206"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="a0813-206"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="a0813-207">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-207">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="a0813-208">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-208">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="a0813-209">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="a0813-209">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="a0813-210">要求</span><span class="sxs-lookup"><span data-stu-id="a0813-210">Request</span></span>             | <span data-ttu-id="a0813-211">回應</span><span class="sxs-lookup"><span data-stu-id="a0813-211">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="a0813-212">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a0813-212">localhost:1234</span></span>      | <span data-ttu-id="a0813-213">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a0813-213">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="a0813-214">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="a0813-214">localhost:1234/map1</span></span> | <span data-ttu-id="a0813-215">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="a0813-215">Map Test 1</span></span>                   |
| <span data-ttu-id="a0813-216">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="a0813-216">localhost:1234/map2</span></span> | <span data-ttu-id="a0813-217">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="a0813-217">Map Test 2</span></span>                   |
| <span data-ttu-id="a0813-218">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="a0813-218">localhost:1234/map3</span></span> | <span data-ttu-id="a0813-219">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a0813-219">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="a0813-220">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="a0813-220">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="a0813-221">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="a0813-221">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="a0813-222">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="a0813-222">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

<span data-ttu-id="a0813-223"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-223"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="a0813-224">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-224">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="a0813-225">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="a0813-225">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?highlight=14-15)]

<span data-ttu-id="a0813-226">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="a0813-226">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="a0813-227">要求</span><span class="sxs-lookup"><span data-stu-id="a0813-227">Request</span></span>                       | <span data-ttu-id="a0813-228">回應</span><span class="sxs-lookup"><span data-stu-id="a0813-228">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="a0813-229">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a0813-229">localhost:1234</span></span>                | <span data-ttu-id="a0813-230">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a0813-230">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="a0813-231">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="a0813-231">localhost:1234/?branch=master</span></span> | <span data-ttu-id="a0813-232">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="a0813-232">Branch used = master</span></span>         |

<span data-ttu-id="a0813-233"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*>也會根據給定述詞的結果來分支要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-233"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> also branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="a0813-234">與不同的 `MapWhen` 是，這個分支會重新加入主要管線（如果它不是短路或包含終端機中介軟體）：</span><span class="sxs-lookup"><span data-stu-id="a0813-234">Unlike with `MapWhen`, this branch is rejoined to the main pipeline if it doesn't short-circuit or contain a terminal middleware:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupUseWhen.cs?highlight=25-26)]

<span data-ttu-id="a0813-235">在上述範例中，回應為「來自主要管線的 Hello」。</span><span class="sxs-lookup"><span data-stu-id="a0813-235">In the preceding example, a response of "Hello from main pipeline."</span></span> <span data-ttu-id="a0813-236">會針對所有要求寫入。</span><span class="sxs-lookup"><span data-stu-id="a0813-236">is written for all requests.</span></span> <span data-ttu-id="a0813-237">如果要求包含查詢字串變數，則 `branch` 會在重新加入主要管線之前記錄其值。</span><span class="sxs-lookup"><span data-stu-id="a0813-237">If the request includes a query string variable `branch`, its value is logged before the main pipeline is rejoined.</span></span>

## <a name="built-in-middleware"></a><span data-ttu-id="a0813-238">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="a0813-238">Built-in middleware</span></span>

<span data-ttu-id="a0813-239">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="a0813-239">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="a0813-240">「順序」\*\* 欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="a0813-240">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="a0813-241">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」\*\*。</span><span class="sxs-lookup"><span data-stu-id="a0813-241">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="a0813-242">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="a0813-242">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="a0813-243">中介軟體</span><span class="sxs-lookup"><span data-stu-id="a0813-243">Middleware</span></span> | <span data-ttu-id="a0813-244">說明</span><span class="sxs-lookup"><span data-stu-id="a0813-244">Description</span></span> | <span data-ttu-id="a0813-245">單</span><span class="sxs-lookup"><span data-stu-id="a0813-245">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="a0813-246">驗證</span><span class="sxs-lookup"><span data-stu-id="a0813-246">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="a0813-247">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-247">Provides authentication support.</span></span> | <span data-ttu-id="a0813-248">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-248">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="a0813-249">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="a0813-249">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="a0813-250">授權</span><span class="sxs-lookup"><span data-stu-id="a0813-250">Authorization</span></span>](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*) | <span data-ttu-id="a0813-251">提供授權支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-251">Provides authorization support.</span></span> | <span data-ttu-id="a0813-252">緊接在驗證中介軟體之後。</span><span class="sxs-lookup"><span data-stu-id="a0813-252">Immediately after the Authentication Middleware.</span></span> |
| [<span data-ttu-id="a0813-253">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="a0813-253">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="a0813-254">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="a0813-254">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="a0813-255">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-255">Before middleware that issues cookies.</span></span> <span data-ttu-id="a0813-256">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="a0813-256">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="a0813-257">CORS</span><span class="sxs-lookup"><span data-stu-id="a0813-257">CORS</span></span>](xref:security/cors) | <span data-ttu-id="a0813-258">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="a0813-258">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="a0813-259">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-259">Before components that use CORS.</span></span> <span data-ttu-id="a0813-260">`UseCors`目前必須在 `UseResponseCaching` [此 bug](https://github.com/dotnet/aspnetcore/issues/23218)之前進行。</span><span class="sxs-lookup"><span data-stu-id="a0813-260">`UseCors` currently must go before `UseResponseCaching` due to [this bug](https://github.com/dotnet/aspnetcore/issues/23218).</span></span>|
| [<span data-ttu-id="a0813-261">診斷</span><span class="sxs-lookup"><span data-stu-id="a0813-261">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="a0813-262">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a0813-262">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="a0813-263">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-263">Before components that generate errors.</span></span> <span data-ttu-id="a0813-264">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="a0813-264">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="a0813-265">轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="a0813-265">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="a0813-266">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-266">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="a0813-267">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-267">Before components that consume the updated fields.</span></span> <span data-ttu-id="a0813-268">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="a0813-268">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="a0813-269">健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="a0813-269">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="a0813-270">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="a0813-270">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="a0813-271">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="a0813-271">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="a0813-272">標頭傳播</span><span class="sxs-lookup"><span data-stu-id="a0813-272">Header Propagation</span></span>](xref:fundamentals/http-requests#header-propagation-middleware) | <span data-ttu-id="a0813-273">將 HTTP 標頭從傳入要求傳播到傳出的 HTTP 用戶端要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-273">Propagates HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> |
| [<span data-ttu-id="a0813-274">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="a0813-274">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="a0813-275">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="a0813-275">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="a0813-276">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-276">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="a0813-277">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="a0813-277">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="a0813-278">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a0813-278">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="a0813-279">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-279">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a0813-280">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a0813-280">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="a0813-281">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="a0813-281">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="a0813-282">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="a0813-282">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="a0813-283">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="a0813-283">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="a0813-284">MVC</span><span class="sxs-lookup"><span data-stu-id="a0813-284">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="a0813-285">處理 MVC/Pages 的要求 Razor 。</span><span class="sxs-lookup"><span data-stu-id="a0813-285">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="a0813-286">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="a0813-286">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="a0813-287">OWIN</span><span class="sxs-lookup"><span data-stu-id="a0813-287">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="a0813-288">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="a0813-288">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="a0813-289">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="a0813-289">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="a0813-290">回應快取</span><span class="sxs-lookup"><span data-stu-id="a0813-290">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="a0813-291">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-291">Provides support for caching responses.</span></span> | <span data-ttu-id="a0813-292">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-292">Before components that require caching.</span></span> <span data-ttu-id="a0813-293">`UseCORS`必須早于 `UseResponseCaching` 。</span><span class="sxs-lookup"><span data-stu-id="a0813-293">`UseCORS` must come before `UseResponseCaching`.</span></span>|
| [<span data-ttu-id="a0813-294">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="a0813-294">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="a0813-295">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-295">Provides support for compressing responses.</span></span> | <span data-ttu-id="a0813-296">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-296">Before components that require compression.</span></span> |
| [<span data-ttu-id="a0813-297">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="a0813-297">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="a0813-298">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-298">Provides localization support.</span></span> | <span data-ttu-id="a0813-299">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-299">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="a0813-300">端點路由</span><span class="sxs-lookup"><span data-stu-id="a0813-300">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="a0813-301">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="a0813-301">Defines and constrains request routes.</span></span> | <span data-ttu-id="a0813-302">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="a0813-302">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="a0813-303">SPA</span><span class="sxs-lookup"><span data-stu-id="a0813-303">SPA</span></span>](xref:Microsoft.AspNetCore.Builder.SpaApplicationBuilderExtensions.UseSpa*) | <span data-ttu-id="a0813-304">藉由傳回單一頁面應用程式（SPA）的預設頁面，處理來自中介軟體鏈中此點的所有要求</span><span class="sxs-lookup"><span data-stu-id="a0813-304">Handles all requests from this point in the middleware chain by returning the default page for the Single Page Application (SPA)</span></span> | <span data-ttu-id="a0813-305">在鏈晚期，讓提供靜態檔案、MVC 動作等的其他中介軟體優先。</span><span class="sxs-lookup"><span data-stu-id="a0813-305">Late in the chain, so that other middleware for serving static files, MVC actions, etc., takes precedence.</span></span>|
| [<span data-ttu-id="a0813-306">本次</span><span class="sxs-lookup"><span data-stu-id="a0813-306">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="a0813-307">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-307">Provides support for managing user sessions.</span></span> | <span data-ttu-id="a0813-308">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-308">Before components that require Session.</span></span> | 
| [<span data-ttu-id="a0813-309">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="a0813-309">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="a0813-310">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="a0813-310">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="a0813-311">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="a0813-311">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="a0813-312">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="a0813-312">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="a0813-313">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-313">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="a0813-314">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-314">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a0813-315">WebSocket</span><span class="sxs-lookup"><span data-stu-id="a0813-315">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="a0813-316">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="a0813-316">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="a0813-317">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-317">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="a0813-318">其他資源</span><span class="sxs-lookup"><span data-stu-id="a0813-318">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:test/middleware>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a0813-319">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="a0813-319">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a0813-320">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="a0813-320">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="a0813-321">每個元件：</span><span class="sxs-lookup"><span data-stu-id="a0813-321">Each component:</span></span>

* <span data-ttu-id="a0813-322">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="a0813-322">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="a0813-323">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="a0813-323">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="a0813-324">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="a0813-324">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="a0813-325">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-325">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="a0813-326">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="a0813-326">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="a0813-327">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="a0813-327">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="a0813-328">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」**，也稱為「中介軟體元件」**。</span><span class="sxs-lookup"><span data-stu-id="a0813-328">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="a0813-329">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-329">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="a0813-330">當中介軟體短路時，稱為「終端中介軟體」\*\*，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-330">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="a0813-331"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="a0813-331"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="a0813-332">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="a0813-332">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="a0813-333">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="a0813-333">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="a0813-334">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="a0813-334">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="a0813-335">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="a0813-335">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="a0813-339">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="a0813-339">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="a0813-340">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="a0813-340">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="a0813-341">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-341">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="a0813-342">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-342">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="a0813-343">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-343">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="a0813-344">第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-344">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="a0813-345">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="a0813-345">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="a0813-346">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="a0813-346">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="a0813-347">您可以「不」\*\* 呼叫「下一個」\*\* 參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-347">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="a0813-348">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a0813-348">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

<span data-ttu-id="a0813-349">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="a0813-349">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="a0813-350">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="a0813-350">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="a0813-351">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="a0813-351">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="a0813-352">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="a0813-352">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="a0813-353">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="a0813-353">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="a0813-354">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="a0813-354">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="a0813-355">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-355">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="a0813-356">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-356">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="a0813-357">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="a0813-357">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="a0813-358">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="a0813-358">May cause a protocol violation.</span></span> <span data-ttu-id="a0813-359">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="a0813-359">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="a0813-360">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="a0813-360">May corrupt the body format.</span></span> <span data-ttu-id="a0813-361">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="a0813-361">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="a0813-362"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="a0813-362"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="a0813-363">中介軟體順序</span><span class="sxs-lookup"><span data-stu-id="a0813-363">Middleware order</span></span>

<span data-ttu-id="a0813-364">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="a0813-364">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="a0813-365">順序對於安全性、效能和功能非常**重要**。</span><span class="sxs-lookup"><span data-stu-id="a0813-365">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="a0813-366">下列 `Startup.Configure` 方法會以建議的順序新增安全性相關中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="a0813-366">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

<span data-ttu-id="a0813-367">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="a0813-367">In the preceding code:</span></span>

* <span data-ttu-id="a0813-368">使用[個別使用者帳戶](xref:security/authentication/identity)建立新的 web 應用程式時，未新增的中介軟體會加上批註。</span><span class="sxs-lookup"><span data-stu-id="a0813-368">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="a0813-369">並非每個中介軟體都必須以這種完全相同的循序執行，但也有許多。</span><span class="sxs-lookup"><span data-stu-id="a0813-369">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="a0813-370">例如， `UseCors` 和 `UseAuthentication` 必須以顯示的循序執行。</span><span class="sxs-lookup"><span data-stu-id="a0813-370">For example, `UseCors` and `UseAuthentication` must go in the order shown.</span></span>

<span data-ttu-id="a0813-371">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="a0813-371">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="a0813-372">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="a0813-372">Exception/error handling</span></span>
   * <span data-ttu-id="a0813-373">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="a0813-373">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="a0813-374">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="a0813-374">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="a0813-375">資料錯誤頁面中介軟體 (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) 會回報資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="a0813-375">Database Error Page Middleware (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) reports database runtime errors.</span></span>
   * <span data-ttu-id="a0813-376">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="a0813-376">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="a0813-377">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-377">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="a0813-378">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="a0813-378">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="a0813-379">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a0813-379">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="a0813-380">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="a0813-380">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="a0813-381">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="a0813-381">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="a0813-382">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="a0813-382">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="a0813-383">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="a0813-383">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="a0813-384">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a0813-384">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="a0813-385">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) 以將 MVC 新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="a0813-385">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) to add MVC to the request pipeline.</span></span>

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

<span data-ttu-id="a0813-386">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="a0813-386">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="a0813-387"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="a0813-387"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="a0813-388">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a0813-388">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="a0813-389">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-389">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="a0813-390">靜態檔案中介軟體**不**提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="a0813-390">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="a0813-391">靜態檔案中介軟體所提供的任何檔案（包括*wwwroot*底下的檔案）皆可公開使用。</span><span class="sxs-lookup"><span data-stu-id="a0813-391">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="a0813-392">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="a0813-392">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="a0813-393">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="a0813-393">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="a0813-394">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="a0813-394">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="a0813-395">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器和動作之後，才會進行授權（和拒絕）。</span><span class="sxs-lookup"><span data-stu-id="a0813-395">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="a0813-396">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="a0813-396">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="a0813-397">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="a0813-397">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="a0813-398">來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。</span><span class="sxs-lookup"><span data-stu-id="a0813-398">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a><span data-ttu-id="a0813-399">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="a0813-399">Use, Run, and Map</span></span>

<span data-ttu-id="a0813-400">設定 HTTP 管線的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 及 <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>。</span><span class="sxs-lookup"><span data-stu-id="a0813-400">Configure the HTTP pipeline using <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, and <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>.</span></span> <span data-ttu-id="a0813-401">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="a0813-401">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="a0813-402">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="a0813-402">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="a0813-403"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="a0813-403"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="a0813-404">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-404">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="a0813-405">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-405">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="a0813-406">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="a0813-406">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="a0813-407">要求</span><span class="sxs-lookup"><span data-stu-id="a0813-407">Request</span></span>             | <span data-ttu-id="a0813-408">回應</span><span class="sxs-lookup"><span data-stu-id="a0813-408">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="a0813-409">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a0813-409">localhost:1234</span></span>      | <span data-ttu-id="a0813-410">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a0813-410">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="a0813-411">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="a0813-411">localhost:1234/map1</span></span> | <span data-ttu-id="a0813-412">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="a0813-412">Map Test 1</span></span>                   |
| <span data-ttu-id="a0813-413">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="a0813-413">localhost:1234/map2</span></span> | <span data-ttu-id="a0813-414">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="a0813-414">Map Test 2</span></span>                   |
| <span data-ttu-id="a0813-415">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="a0813-415">localhost:1234/map3</span></span> | <span data-ttu-id="a0813-416">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a0813-416">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="a0813-417">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="a0813-417">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="a0813-418"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-418"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="a0813-419">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="a0813-419">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="a0813-420">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="a0813-420">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

<span data-ttu-id="a0813-421">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="a0813-421">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="a0813-422">要求</span><span class="sxs-lookup"><span data-stu-id="a0813-422">Request</span></span>                       | <span data-ttu-id="a0813-423">回應</span><span class="sxs-lookup"><span data-stu-id="a0813-423">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="a0813-424">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="a0813-424">localhost:1234</span></span>                | <span data-ttu-id="a0813-425">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="a0813-425">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="a0813-426">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="a0813-426">localhost:1234/?branch=master</span></span> | <span data-ttu-id="a0813-427">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="a0813-427">Branch used = master</span></span>         |

<span data-ttu-id="a0813-428">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="a0813-428">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="a0813-429">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="a0813-429">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="a0813-430">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="a0813-430">Built-in middleware</span></span>

<span data-ttu-id="a0813-431">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="a0813-431">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="a0813-432">「順序」\*\* 欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="a0813-432">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="a0813-433">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」\*\*。</span><span class="sxs-lookup"><span data-stu-id="a0813-433">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="a0813-434">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="a0813-434">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="a0813-435">中介軟體</span><span class="sxs-lookup"><span data-stu-id="a0813-435">Middleware</span></span> | <span data-ttu-id="a0813-436">說明</span><span class="sxs-lookup"><span data-stu-id="a0813-436">Description</span></span> | <span data-ttu-id="a0813-437">單</span><span class="sxs-lookup"><span data-stu-id="a0813-437">Order</span></span> |
| ---------- | ----------- | ----- |
| [<span data-ttu-id="a0813-438">驗證</span><span class="sxs-lookup"><span data-stu-id="a0813-438">Authentication</span></span>](xref:security/authentication/identity) | <span data-ttu-id="a0813-439">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-439">Provides authentication support.</span></span> | <span data-ttu-id="a0813-440">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-440">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="a0813-441">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="a0813-441">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="a0813-442">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="a0813-442">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="a0813-443">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="a0813-443">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="a0813-444">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-444">Before middleware that issues cookies.</span></span> <span data-ttu-id="a0813-445">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="a0813-445">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="a0813-446">CORS</span><span class="sxs-lookup"><span data-stu-id="a0813-446">CORS</span></span>](xref:security/cors) | <span data-ttu-id="a0813-447">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="a0813-447">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="a0813-448">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-448">Before components that use CORS.</span></span> |
| [<span data-ttu-id="a0813-449">診斷</span><span class="sxs-lookup"><span data-stu-id="a0813-449">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="a0813-450">提供開發人員例外狀況頁面、例外狀況處理、狀態字碼頁，以及新應用程式的預設網頁的數個個別中介軟體。</span><span class="sxs-lookup"><span data-stu-id="a0813-450">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="a0813-451">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-451">Before components that generate errors.</span></span> <span data-ttu-id="a0813-452">終端機的例外狀況，或為新的應用程式提供預設的網頁。</span><span class="sxs-lookup"><span data-stu-id="a0813-452">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="a0813-453">轉送的標頭</span><span class="sxs-lookup"><span data-stu-id="a0813-453">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="a0813-454">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="a0813-454">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="a0813-455">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-455">Before components that consume the updated fields.</span></span> <span data-ttu-id="a0813-456">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="a0813-456">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="a0813-457">健康情況檢查</span><span class="sxs-lookup"><span data-stu-id="a0813-457">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="a0813-458">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="a0813-458">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="a0813-459">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="a0813-459">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="a0813-460">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="a0813-460">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="a0813-461">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="a0813-461">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="a0813-462">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-462">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="a0813-463">HTTPS 重新導向</span><span class="sxs-lookup"><span data-stu-id="a0813-463">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="a0813-464">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="a0813-464">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="a0813-465">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-465">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a0813-466">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="a0813-466">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="a0813-467">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="a0813-467">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="a0813-468">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="a0813-468">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="a0813-469">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="a0813-469">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="a0813-470">MVC</span><span class="sxs-lookup"><span data-stu-id="a0813-470">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="a0813-471">處理 MVC/Pages 的要求 Razor 。</span><span class="sxs-lookup"><span data-stu-id="a0813-471">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="a0813-472">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="a0813-472">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="a0813-473">OWIN</span><span class="sxs-lookup"><span data-stu-id="a0813-473">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="a0813-474">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="a0813-474">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="a0813-475">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="a0813-475">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="a0813-476">回應快取</span><span class="sxs-lookup"><span data-stu-id="a0813-476">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="a0813-477">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-477">Provides support for caching responses.</span></span> | <span data-ttu-id="a0813-478">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-478">Before components that require caching.</span></span> |
| [<span data-ttu-id="a0813-479">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="a0813-479">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="a0813-480">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-480">Provides support for compressing responses.</span></span> | <span data-ttu-id="a0813-481">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-481">Before components that require compression.</span></span> |
| [<span data-ttu-id="a0813-482">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="a0813-482">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="a0813-483">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-483">Provides localization support.</span></span> | <span data-ttu-id="a0813-484">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-484">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="a0813-485">端點路由</span><span class="sxs-lookup"><span data-stu-id="a0813-485">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="a0813-486">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="a0813-486">Defines and constrains request routes.</span></span> | <span data-ttu-id="a0813-487">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="a0813-487">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="a0813-488">本次</span><span class="sxs-lookup"><span data-stu-id="a0813-488">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="a0813-489">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-489">Provides support for managing user sessions.</span></span> | <span data-ttu-id="a0813-490">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-490">Before components that require Session.</span></span> |
| [<span data-ttu-id="a0813-491">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="a0813-491">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="a0813-492">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="a0813-492">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="a0813-493">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="a0813-493">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="a0813-494">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="a0813-494">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="a0813-495">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="a0813-495">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="a0813-496">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-496">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="a0813-497">WebSocket</span><span class="sxs-lookup"><span data-stu-id="a0813-497">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="a0813-498">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="a0813-498">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="a0813-499">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="a0813-499">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="a0813-500">其他資源</span><span class="sxs-lookup"><span data-stu-id="a0813-500">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:test/middleware>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end
