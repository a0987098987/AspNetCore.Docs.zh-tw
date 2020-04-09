---
title: ASP.NET Core 中介軟體
author: rick-anderson
description: 了解 ASP.NET Core 中介軟體和要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2020
uid: fundamentals/middleware/index
ms.openlocfilehash: 6bf8ed823386ca4e1cf78982f7fba41fba429db8
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80751064"
---
# <a name="aspnet-core-middleware"></a><span data-ttu-id="3405f-103">ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="3405f-103">ASP.NET Core Middleware</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3405f-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="3405f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3405f-105">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="3405f-105">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="3405f-106">每個元件：</span><span class="sxs-lookup"><span data-stu-id="3405f-106">Each component:</span></span>

* <span data-ttu-id="3405f-107">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="3405f-107">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="3405f-108">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="3405f-108">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="3405f-109">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="3405f-109">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="3405f-110">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-110">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="3405f-111">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="3405f-111">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="3405f-112">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="3405f-112">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="3405f-113">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」**，也稱為「中介軟體元件」**。</span><span class="sxs-lookup"><span data-stu-id="3405f-113">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="3405f-114">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-114">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="3405f-115">當中介軟體短路時，稱為「終端中介軟體」\*\*，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-115">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="3405f-116"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="3405f-116"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="3405f-117">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="3405f-117">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="3405f-118">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="3405f-118">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="3405f-119">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="3405f-119">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="3405f-120">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="3405f-120">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="3405f-124">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="3405f-124">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="3405f-125">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="3405f-125">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="3405f-126">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-126">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="3405f-127">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="3405f-127">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="3405f-128">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-128">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="3405f-129">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="3405f-129">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="3405f-130">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="3405f-130">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="3405f-131">您可以「不」\*\* 呼叫「下一個」\*\* 參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-131">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="3405f-132">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="3405f-132">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=5-10)]

<span data-ttu-id="3405f-133">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="3405f-133">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="3405f-134">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="3405f-134">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="3405f-135">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="3405f-135">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="3405f-136">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="3405f-136">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="3405f-137">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="3405f-137">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="3405f-138">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="3405f-138">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="3405f-139">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-139">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="3405f-140">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-140">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="3405f-141">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="3405f-141">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="3405f-142">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="3405f-142">May cause a protocol violation.</span></span> <span data-ttu-id="3405f-143">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="3405f-143">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="3405f-144">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="3405f-144">May corrupt the body format.</span></span> <span data-ttu-id="3405f-145">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="3405f-145">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="3405f-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="3405f-146"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<span data-ttu-id="3405f-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>不合法的參數`next`。</span><span class="sxs-lookup"><span data-stu-id="3405f-147"><xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegates don't receive a `next` parameter.</span></span> <span data-ttu-id="3405f-148">第一`Run`個委託始終是終端並終止管道。</span><span class="sxs-lookup"><span data-stu-id="3405f-148">The first `Run` delegate is always terminal and terminates the pipeline.</span></span> <span data-ttu-id="3405f-149">`Run`是一種慣例。</span><span class="sxs-lookup"><span data-stu-id="3405f-149">`Run` is a convention.</span></span> <span data-ttu-id="3405f-150">某些中間元件元件可能會公開`Run[Middleware]`在導管末尾執行的方法:</span><span class="sxs-lookup"><span data-stu-id="3405f-150">Some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs?highlight=12-15)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="3405f-151">在前面的範例中,`Run`委託寫`"Hello from 2nd delegate."`入 回應,然後終止管道。</span><span class="sxs-lookup"><span data-stu-id="3405f-151">In the preceding example, the `Run` delegate writes `"Hello from 2nd delegate."` to the response and then terminates the pipeline.</span></span> <span data-ttu-id="3405f-152">如果在`Run`委託`Use`之後`Run`添加了另一個或委託,則不調用它。</span><span class="sxs-lookup"><span data-stu-id="3405f-152">If another `Use` or `Run` delegate is added after the `Run` delegate, it's not called.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="3405f-153">中介順序</span><span class="sxs-lookup"><span data-stu-id="3405f-153">Middleware order</span></span>

<span data-ttu-id="3405f-154">下圖顯示了 ASP.NET核心 MVC 和 Razor 頁面應用的完整請求處理管道。</span><span class="sxs-lookup"><span data-stu-id="3405f-154">The following diagram shows the complete request processing pipeline for ASP.NET Core MVC and Razor Pages apps.</span></span> <span data-ttu-id="3405f-155">您可以看到,在典型的應用中,如何訂購現有中間件,以及在哪裡添加自定義中間件。</span><span class="sxs-lookup"><span data-stu-id="3405f-155">You can see how, in a typical app, existing middlewares are ordered and where custom middlewares are added.</span></span> <span data-ttu-id="3405f-156">您可以完全控制如何重新排序現有中間件或為方案進行必要的新的自定義中間件。</span><span class="sxs-lookup"><span data-stu-id="3405f-156">You have full control over how to reorder existing middlewares or inject new custom middlewares as necessary for your scenarios.</span></span>

![ASP.NET核心的中間元件導管](index/_static/middleware-pipeline.svg)

<span data-ttu-id="3405f-158">上圖中的**端點**中間件執行相應應用&mdash;類型 MVC 或 Razor Pages 的篩選管道。</span><span class="sxs-lookup"><span data-stu-id="3405f-158">The **Endpoint** middleware in the preceding diagram executes the filter pipeline for the corresponding app type&mdash;MVC or Razor Pages.</span></span>

![ASP.NET核心過濾器導管](index/_static/mvc-endpoint.svg)

<span data-ttu-id="3405f-160">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="3405f-160">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="3405f-161">該順序對於安全性、性能和功能**至關重要**。</span><span class="sxs-lookup"><span data-stu-id="3405f-161">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="3405f-162">以下`Startup.Configure`方法按建議的順序新增與安全相關的中間元件元件:</span><span class="sxs-lookup"><span data-stu-id="3405f-162">The following `Startup.Configure` method adds security-related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/StartupAll3.cs?name=snippet)]

<span data-ttu-id="3405f-163">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="3405f-163">In the preceding code:</span></span>

* <span data-ttu-id="3405f-164">註釋在使用[單個使用者帳戶](xref:security/authentication/identity)創建新 Web 應用時未添加的中間件。</span><span class="sxs-lookup"><span data-stu-id="3405f-164">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="3405f-165">並不是每個中間件都需要按這個確切的順序去做,但很多都這樣。</span><span class="sxs-lookup"><span data-stu-id="3405f-165">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="3405f-166">例如, `UseCors` `UseAuthentication`,`UseAuthorization`和必須按顯示的順序進行。</span><span class="sxs-lookup"><span data-stu-id="3405f-166">For example, `UseCors`, `UseAuthentication`, and `UseAuthorization` must go in the order shown.</span></span>

<span data-ttu-id="3405f-167">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="3405f-167">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="3405f-168">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="3405f-168">Exception/error handling</span></span>
   * <span data-ttu-id="3405f-169">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="3405f-169">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="3405f-170">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="3405f-170">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="3405f-171">資料庫錯誤頁中間件報告資料庫運行時錯誤。</span><span class="sxs-lookup"><span data-stu-id="3405f-171">Database Error Page Middleware reports database runtime errors.</span></span>
   * <span data-ttu-id="3405f-172">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="3405f-172">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="3405f-173">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-173">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="3405f-174">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="3405f-174">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="3405f-175">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="3405f-175">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="3405f-176">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="3405f-176">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="3405f-177">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="3405f-177">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="3405f-178">路由中間件`UseRouting`( ) 以路由請求。</span><span class="sxs-lookup"><span data-stu-id="3405f-178">Routing Middleware (`UseRouting`) to route requests.</span></span>
1. <span data-ttu-id="3405f-179">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="3405f-179">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="3405f-180">授權中間件`UseAuthorization`() 授權使用者存取安全資源。</span><span class="sxs-lookup"><span data-stu-id="3405f-180">Authorization Middleware (`UseAuthorization`) authorizes a user to access secure resources.</span></span>
1. <span data-ttu-id="3405f-181">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="3405f-181">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="3405f-182">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3405f-182">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="3405f-183">終結點路由中間件`UseEndpoints`(`MapRazorPages`帶 ) 以將 Razor 頁面終結點添加到請求管道。</span><span class="sxs-lookup"><span data-stu-id="3405f-183">Endpoint Routing Middleware (`UseEndpoints` with `MapRazorPages`) to add Razor Pages endpoints to the request pipeline.</span></span>

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

<span data-ttu-id="3405f-184">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="3405f-184">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="3405f-185"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="3405f-185"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="3405f-186">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-186">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="3405f-187">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-187">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="3405f-188">靜態文件中間件**不**提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="3405f-188">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="3405f-189">靜態檔中間件提供的任何檔,包括*wwwroot*下的檔,都是公開的。</span><span class="sxs-lookup"><span data-stu-id="3405f-189">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="3405f-190">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="3405f-190">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="3405f-191">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="3405f-191">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="3405f-192">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-192">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="3405f-193">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="3405f-193">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="3405f-194">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="3405f-194">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="3405f-195">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="3405f-195">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="3405f-196">可以壓縮剃刀頁回應。</span><span class="sxs-lookup"><span data-stu-id="3405f-196">The Razor Pages responses can be compressed.</span></span>

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

<span data-ttu-id="3405f-197">對於單頁應用程式 (SPA),SPA<xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*>中間件通常排在中間件管道中。</span><span class="sxs-lookup"><span data-stu-id="3405f-197">For Single Page Applications (SPAs), the SPA middleware <xref:Microsoft.Extensions.DependencyInjection.SpaStaticFilesExtensions.UseSpaStaticFiles*> usually comes last in the middleware pipeline.</span></span> <span data-ttu-id="3405f-198">SPA 中間件排在最後:</span><span class="sxs-lookup"><span data-stu-id="3405f-198">The SPA middleware comes last:</span></span>

* <span data-ttu-id="3405f-199">允許所有其他中間件首先回應匹配的請求。</span><span class="sxs-lookup"><span data-stu-id="3405f-199">To allow all other middlewares to respond to matching requests first.</span></span>
* <span data-ttu-id="3405f-200">允許具有用戶端路由的 SPA 運行伺服器應用無法識別的所有路由。</span><span class="sxs-lookup"><span data-stu-id="3405f-200">To allow SPAs with client-side routing to run for all routes that are unrecognized by the server app.</span></span>

<span data-ttu-id="3405f-201">有關 SpA 的更多詳細資訊,請參閱["反應](xref:spa/react)"和"[角"](xref:spa/angular)專案範本的指南。</span><span class="sxs-lookup"><span data-stu-id="3405f-201">For more details on SPAs, see the guides for the [React](xref:spa/react) and [Angular](xref:spa/angular) project templates.</span></span>

## <a name="branch-the-middleware-pipeline"></a><span data-ttu-id="3405f-202">分支中介元件導管</span><span class="sxs-lookup"><span data-stu-id="3405f-202">Branch the middleware pipeline</span></span>

<span data-ttu-id="3405f-203"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="3405f-203"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="3405f-204">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-204">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="3405f-205">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-205">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="3405f-206">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="3405f-206">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="3405f-207">要求</span><span class="sxs-lookup"><span data-stu-id="3405f-207">Request</span></span>             | <span data-ttu-id="3405f-208">回應</span><span class="sxs-lookup"><span data-stu-id="3405f-208">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="3405f-209">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="3405f-209">localhost:1234</span></span>      | <span data-ttu-id="3405f-210">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="3405f-210">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="3405f-211">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="3405f-211">localhost:1234/map1</span></span> | <span data-ttu-id="3405f-212">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="3405f-212">Map Test 1</span></span>                   |
| <span data-ttu-id="3405f-213">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="3405f-213">localhost:1234/map2</span></span> | <span data-ttu-id="3405f-214">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="3405f-214">Map Test 2</span></span>                   |
| <span data-ttu-id="3405f-215">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="3405f-215">localhost:1234/map3</span></span> | <span data-ttu-id="3405f-216">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="3405f-216">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="3405f-217">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="3405f-217">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="3405f-218">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="3405f-218">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="3405f-219">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="3405f-219">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

<span data-ttu-id="3405f-220"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-220"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="3405f-221">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-221">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="3405f-222">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="3405f-222">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs?highlight=14-15)]

<span data-ttu-id="3405f-223">下表顯示使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應：</span><span class="sxs-lookup"><span data-stu-id="3405f-223">The following table shows the requests and responses from `http://localhost:1234` using the previous code:</span></span>

| <span data-ttu-id="3405f-224">要求</span><span class="sxs-lookup"><span data-stu-id="3405f-224">Request</span></span>                       | <span data-ttu-id="3405f-225">回應</span><span class="sxs-lookup"><span data-stu-id="3405f-225">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="3405f-226">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="3405f-226">localhost:1234</span></span>                | <span data-ttu-id="3405f-227">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="3405f-227">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="3405f-228">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="3405f-228">localhost:1234/?branch=master</span></span> | <span data-ttu-id="3405f-229">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="3405f-229">Branch used = master</span></span>         |

<span data-ttu-id="3405f-230"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*>還基於給定謂詞的結果分支請求管道。</span><span class="sxs-lookup"><span data-stu-id="3405f-230"><xref:Microsoft.AspNetCore.Builder.UseWhenExtensions.UseWhen*> also branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="3405f-231">與`MapWhen`不同,如果此分支不短路或包含終端中間件,則將其重新加入主管道:</span><span class="sxs-lookup"><span data-stu-id="3405f-231">Unlike with `MapWhen`, this branch is rejoined to the main pipeline if it doesn't short-circuit or contain a terminal middleware:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupUseWhen.cs?highlight=25-26)]

<span data-ttu-id="3405f-232">在前面的範例中,回應「您從主管道發出」。</span><span class="sxs-lookup"><span data-stu-id="3405f-232">In the preceding example, a response of "Hello from main pipeline."</span></span> <span data-ttu-id="3405f-233">為所有請求編寫。</span><span class="sxs-lookup"><span data-stu-id="3405f-233">is written for all requests.</span></span> <span data-ttu-id="3405f-234">如果請求包含查詢字串變數`branch`,則在重新聯接主管道之前記錄其值。</span><span class="sxs-lookup"><span data-stu-id="3405f-234">If the request includes a query string variable `branch`, its value is logged before the main pipeline is rejoined.</span></span>

## <a name="built-in-middleware"></a><span data-ttu-id="3405f-235">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="3405f-235">Built-in middleware</span></span>

<span data-ttu-id="3405f-236">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="3405f-236">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="3405f-237">「順序」\*\* 欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="3405f-237">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="3405f-238">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」\*\*。</span><span class="sxs-lookup"><span data-stu-id="3405f-238">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="3405f-239">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="3405f-239">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="3405f-240">中介軟體</span><span class="sxs-lookup"><span data-stu-id="3405f-240">Middleware</span></span> | <span data-ttu-id="3405f-241">描述</span><span class="sxs-lookup"><span data-stu-id="3405f-241">Description</span></span> | <span data-ttu-id="3405f-242">單</span><span class="sxs-lookup"><span data-stu-id="3405f-242">Order</span></span> |
| ---------- | ----------- | ----- |
| <span data-ttu-id="3405f-243">[驗證][](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="3405f-243">[Authentication](xref:security/authentication/identity)</span></span> | <span data-ttu-id="3405f-244">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-244">Provides authentication support.</span></span> | <span data-ttu-id="3405f-245">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-245">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="3405f-246">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="3405f-246">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="3405f-247">授權</span><span class="sxs-lookup"><span data-stu-id="3405f-247">Authorization</span></span>](xref:Microsoft.AspNetCore.Builder.AuthorizationAppBuilderExtensions.UseAuthorization*) | <span data-ttu-id="3405f-248">提供授權支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-248">Provides authorization support.</span></span> | <span data-ttu-id="3405f-249">立即在身份驗證中間件之後。</span><span class="sxs-lookup"><span data-stu-id="3405f-249">Immediately after the Authentication Middleware.</span></span> |
| [<span data-ttu-id="3405f-250">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="3405f-250">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="3405f-251">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="3405f-251">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="3405f-252">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-252">Before middleware that issues cookies.</span></span> <span data-ttu-id="3405f-253">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="3405f-253">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="3405f-254">CORS</span><span class="sxs-lookup"><span data-stu-id="3405f-254">CORS</span></span>](xref:security/cors) | <span data-ttu-id="3405f-255">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="3405f-255">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="3405f-256">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-256">Before components that use CORS.</span></span> |
| [<span data-ttu-id="3405f-257">診斷</span><span class="sxs-lookup"><span data-stu-id="3405f-257">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="3405f-258">提供開發人員異常頁、異常處理、狀態代碼頁和新應用的預設網頁的幾個單獨的中間件。</span><span class="sxs-lookup"><span data-stu-id="3405f-258">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="3405f-259">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-259">Before components that generate errors.</span></span> <span data-ttu-id="3405f-260">用於異常或為新應用提供預設網頁的終端。</span><span class="sxs-lookup"><span data-stu-id="3405f-260">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="3405f-261">轉寄的標頭</span><span class="sxs-lookup"><span data-stu-id="3405f-261">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="3405f-262">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-262">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="3405f-263">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-263">Before components that consume the updated fields.</span></span> <span data-ttu-id="3405f-264">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="3405f-264">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="3405f-265">執行狀況檢查</span><span class="sxs-lookup"><span data-stu-id="3405f-265">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="3405f-266">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="3405f-266">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="3405f-267">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="3405f-267">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="3405f-268">標題傳播</span><span class="sxs-lookup"><span data-stu-id="3405f-268">Header Propagation</span></span>](xref:fundamentals/http-requests#header-propagation-middleware) | <span data-ttu-id="3405f-269">將 HTTP 標頭從傳入請求傳播到傳出的 HTTP 用戶端請求。</span><span class="sxs-lookup"><span data-stu-id="3405f-269">Propagates HTTP headers from the incoming request to the outgoing HTTP Client requests.</span></span> |
| [<span data-ttu-id="3405f-270">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="3405f-270">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="3405f-271">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="3405f-271">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="3405f-272">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-272">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="3405f-273">HTTPS 重定向</span><span class="sxs-lookup"><span data-stu-id="3405f-273">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="3405f-274">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="3405f-274">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="3405f-275">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-275">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="3405f-276">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="3405f-276">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="3405f-277">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="3405f-277">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="3405f-278">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="3405f-278">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="3405f-279">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="3405f-279">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="3405f-280">MVC</span><span class="sxs-lookup"><span data-stu-id="3405f-280">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="3405f-281">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-281">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="3405f-282">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="3405f-282">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="3405f-283">OWIN</span><span class="sxs-lookup"><span data-stu-id="3405f-283">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="3405f-284">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="3405f-284">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="3405f-285">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="3405f-285">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="3405f-286">回應快取</span><span class="sxs-lookup"><span data-stu-id="3405f-286">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="3405f-287">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-287">Provides support for caching responses.</span></span> | <span data-ttu-id="3405f-288">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-288">Before components that require caching.</span></span> |
| [<span data-ttu-id="3405f-289">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="3405f-289">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="3405f-290">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-290">Provides support for compressing responses.</span></span> | <span data-ttu-id="3405f-291">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-291">Before components that require compression.</span></span> |
| [<span data-ttu-id="3405f-292">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="3405f-292">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="3405f-293">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-293">Provides localization support.</span></span> | <span data-ttu-id="3405f-294">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-294">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="3405f-295">端點路由</span><span class="sxs-lookup"><span data-stu-id="3405f-295">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="3405f-296">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="3405f-296">Defines and constrains request routes.</span></span> | <span data-ttu-id="3405f-297">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="3405f-297">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="3405f-298">SPA</span><span class="sxs-lookup"><span data-stu-id="3405f-298">SPA</span></span>](xref:Microsoft.AspNetCore.Builder.SpaApplicationBuilderExtensions.UseSpa*) | <span data-ttu-id="3405f-299">以返回單頁應用程式 (SPA) 的預設頁面來處理中間元件鍊中此時的所有請求</span><span class="sxs-lookup"><span data-stu-id="3405f-299">Handles all requests from this point in the middleware chain by returning the default page for the Single Page Application (SPA)</span></span> | <span data-ttu-id="3405f-300">鏈的後期,以便其他中間件用於提供靜態檔、MVC 操作等,優先。</span><span class="sxs-lookup"><span data-stu-id="3405f-300">Late in the chain, so that other middleware for serving static files, MVC actions, etc., takes precedence.</span></span>|
| [<span data-ttu-id="3405f-301">工作階段</span><span class="sxs-lookup"><span data-stu-id="3405f-301">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="3405f-302">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-302">Provides support for managing user sessions.</span></span> | <span data-ttu-id="3405f-303">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-303">Before components that require Session.</span></span> | 
| [<span data-ttu-id="3405f-304">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="3405f-304">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="3405f-305">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="3405f-305">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="3405f-306">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="3405f-306">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="3405f-307">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="3405f-307">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="3405f-308">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-308">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="3405f-309">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-309">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="3405f-310">WebSocket</span><span class="sxs-lookup"><span data-stu-id="3405f-310">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="3405f-311">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="3405f-311">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="3405f-312">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-312">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="3405f-313">其他資源</span><span class="sxs-lookup"><span data-stu-id="3405f-313">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3405f-314">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Steve Smith](https://ardalis.com/) 撰寫</span><span class="sxs-lookup"><span data-stu-id="3405f-314">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="3405f-315">中介軟體為組成應用程式管線的軟體，用以處理要求與回應。</span><span class="sxs-lookup"><span data-stu-id="3405f-315">Middleware is software that's assembled into an app pipeline to handle requests and responses.</span></span> <span data-ttu-id="3405f-316">每個元件：</span><span class="sxs-lookup"><span data-stu-id="3405f-316">Each component:</span></span>

* <span data-ttu-id="3405f-317">可選擇是否要將要求傳送到管線中的下一個元件。</span><span class="sxs-lookup"><span data-stu-id="3405f-317">Chooses whether to pass the request to the next component in the pipeline.</span></span>
* <span data-ttu-id="3405f-318">可以下一個元件的前後執行工作。</span><span class="sxs-lookup"><span data-stu-id="3405f-318">Can perform work before and after the next component in the pipeline.</span></span>

<span data-ttu-id="3405f-319">要求委派用於建置要求管線，</span><span class="sxs-lookup"><span data-stu-id="3405f-319">Request delegates are used to build the request pipeline.</span></span> <span data-ttu-id="3405f-320">其會處理每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-320">The request delegates handle each HTTP request.</span></span>

<span data-ttu-id="3405f-321">要求委派的設定方式為使用 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>、<xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 和 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> 擴充方法。</span><span class="sxs-lookup"><span data-stu-id="3405f-321">Request delegates are configured using <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>, and <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*> extension methods.</span></span> <span data-ttu-id="3405f-322">您可將個別要求委派指定為內嵌匿名方法 (在內嵌中介軟體中呼叫)，或於可重複使用的類別中加以定義。</span><span class="sxs-lookup"><span data-stu-id="3405f-322">An individual request delegate can be specified in-line as an anonymous method (called in-line middleware), or it can be defined in a reusable class.</span></span> <span data-ttu-id="3405f-323">這些可重複使用的類別及內嵌匿名方法皆為「中介軟體」**，也稱為「中介軟體元件」**。</span><span class="sxs-lookup"><span data-stu-id="3405f-323">These reusable classes and in-line anonymous methods are *middleware*, also called *middleware components*.</span></span> <span data-ttu-id="3405f-324">要求管線中的每個中介軟體元件負責叫用管線中下一個元件，或對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-324">Each middleware component in the request pipeline is responsible for invoking the next component in the pipeline or short-circuiting the pipeline.</span></span> <span data-ttu-id="3405f-325">當中介軟體短路時，稱為「終端中介軟體」\*\*，因為它會防止接下來的中介軟體處理要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-325">When a middleware short-circuits, it's called a *terminal middleware* because it prevents further middleware from processing the request.</span></span>

<span data-ttu-id="3405f-326"><xref:migration/http-modules> 說明 ASP.NET Core 和 ASP.NET 4.x 之間的要求管線差異，並提供更多中介軟體範例。</span><span class="sxs-lookup"><span data-stu-id="3405f-326"><xref:migration/http-modules> explains the difference between request pipelines in ASP.NET Core and ASP.NET 4.x and provides additional middleware samples.</span></span>

## <a name="create-a-middleware-pipeline-with-iapplicationbuilder"></a><span data-ttu-id="3405f-327">使用 IApplicationBuilder 建立中介軟體管線</span><span class="sxs-lookup"><span data-stu-id="3405f-327">Create a middleware pipeline with IApplicationBuilder</span></span>

<span data-ttu-id="3405f-328">ASP.NET Core 要求管線由要求委派序列組成，並會一個接著一個呼叫。</span><span class="sxs-lookup"><span data-stu-id="3405f-328">The ASP.NET Core request pipeline consists of a sequence of request delegates, called one after the other.</span></span> <span data-ttu-id="3405f-329">下圖說明此概念。</span><span class="sxs-lookup"><span data-stu-id="3405f-329">The following diagram demonstrates the concept.</span></span> <span data-ttu-id="3405f-330">執行緒遵循黑色箭號執行。</span><span class="sxs-lookup"><span data-stu-id="3405f-330">The thread of execution follows the black arrows.</span></span>

![要求處理模式說明要求抵達，經過三個中介軟體所處理，然後應用程式送出回應。](index/_static/request-delegate-pipeline.png)

<span data-ttu-id="3405f-334">每一個委派皆能在下個委派的前後執行作業。</span><span class="sxs-lookup"><span data-stu-id="3405f-334">Each delegate can perform operations before and after the next delegate.</span></span> <span data-ttu-id="3405f-335">處理例外狀況的委派必須提前在管線中呼叫，以便其可與管線後續階段中所發生的例外狀況達成一致。</span><span class="sxs-lookup"><span data-stu-id="3405f-335">Exception-handling delegates should be called early in the pipeline, so they can catch exceptions that occur in later stages of the pipeline.</span></span>

<span data-ttu-id="3405f-336">最簡潔的 ASP.NET Core 應用程式會設定單一要求委派來處理所有要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-336">The simplest possible ASP.NET Core app sets up a single request delegate that handles all requests.</span></span> <span data-ttu-id="3405f-337">此情況不包含實際要求管線。</span><span class="sxs-lookup"><span data-stu-id="3405f-337">This case doesn't include an actual request pipeline.</span></span> <span data-ttu-id="3405f-338">反之，系統會呼叫單一匿名函式來回應每個 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-338">Instead, a single anonymous function is called in response to every HTTP request.</span></span>

[!code-csharp[](index/snapshot/Middleware/Startup.cs)]

<span data-ttu-id="3405f-339">第一個 <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 委派會終止管線。</span><span class="sxs-lookup"><span data-stu-id="3405f-339">The first <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> delegate terminates the pipeline.</span></span>

<span data-ttu-id="3405f-340">將多個要求委派鏈結在一起的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>。</span><span class="sxs-lookup"><span data-stu-id="3405f-340">Chain multiple request delegates together with <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>.</span></span> <span data-ttu-id="3405f-341">`next` 參數代表管線中的下個委派。</span><span class="sxs-lookup"><span data-stu-id="3405f-341">The `next` parameter represents the next delegate in the pipeline.</span></span> <span data-ttu-id="3405f-342">您可以「不」\*\* 呼叫「下一個」\*\* 參數來對管線執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-342">You can short-circuit the pipeline by *not* calling the *next* parameter.</span></span> <span data-ttu-id="3405f-343">您通常可以在下個委派的前後執行動作，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="3405f-343">You can typically perform actions both before and after the next delegate, as the following example demonstrates:</span></span>

[!code-csharp[](index/snapshot/Chain/Startup.cs)]

<span data-ttu-id="3405f-344">當委派不將要求傳遞到下一個委派時，這就是所謂*讓要求管線短路*。</span><span class="sxs-lookup"><span data-stu-id="3405f-344">When a delegate doesn't pass a request to the next delegate, it's called *short-circuiting the request pipeline*.</span></span> <span data-ttu-id="3405f-345">因為最少運算可避免不必要的工作，所以經常使用。</span><span class="sxs-lookup"><span data-stu-id="3405f-345">Short-circuiting is often desirable because it avoids unnecessary work.</span></span> <span data-ttu-id="3405f-346">例如，[靜態檔案中介軟體](xref:fundamentals/static-files)可以做為*終端中介軟體*使用，方式是處理靜態檔案的要求，並對剩餘的管線執行短路。</span><span class="sxs-lookup"><span data-stu-id="3405f-346">For example, [Static File Middleware](xref:fundamentals/static-files) can act as a *terminal middleware* by processing a request for a static file and short-circuiting the rest of the pipeline.</span></span> <span data-ttu-id="3405f-347">在中介軟體之前新增到管線且終結進一步處理的中介軟體在其 `next.Invoke` 陳述式之後仍然處理程式碼。</span><span class="sxs-lookup"><span data-stu-id="3405f-347">Middleware added to the pipeline before the middleware that terminates further processing still processes code after their `next.Invoke` statements.</span></span> <span data-ttu-id="3405f-348">不過，查看下列有關嘗試寫入已傳送之回應的警告。</span><span class="sxs-lookup"><span data-stu-id="3405f-348">However, see the following warning about attempting to write to a response that has already been sent.</span></span>

> [!WARNING]
> <span data-ttu-id="3405f-349">請不要在回應已傳送給用戶端之後呼叫 `next.Invoke`。</span><span class="sxs-lookup"><span data-stu-id="3405f-349">Don't call `next.Invoke` after the response has been sent to the client.</span></span> <span data-ttu-id="3405f-350">回應啟動後，變更為 <xref:Microsoft.AspNetCore.Http.HttpResponse> 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-350">Changes to <xref:Microsoft.AspNetCore.Http.HttpResponse> after the response has started throw an exception.</span></span> <span data-ttu-id="3405f-351">例如，變更設定標頭、狀態碼等都會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-351">For example, changes such as setting headers and a status code throw an exception.</span></span> <span data-ttu-id="3405f-352">若在呼叫 `next` 後寫入回應本文：</span><span class="sxs-lookup"><span data-stu-id="3405f-352">Writing to the response body after calling `next`:</span></span>
>
> * <span data-ttu-id="3405f-353">可能導致違反通訊協定。</span><span class="sxs-lookup"><span data-stu-id="3405f-353">May cause a protocol violation.</span></span> <span data-ttu-id="3405f-354">例如，寫入超過指定 `Content-Length` 的內容。</span><span class="sxs-lookup"><span data-stu-id="3405f-354">For example, writing more than the stated `Content-Length`.</span></span>
> * <span data-ttu-id="3405f-355">可能損毀本文格式。</span><span class="sxs-lookup"><span data-stu-id="3405f-355">May corrupt the body format.</span></span> <span data-ttu-id="3405f-356">例如，將 HTML 頁尾寫入 CSS 檔。</span><span class="sxs-lookup"><span data-stu-id="3405f-356">For example, writing an HTML footer to a CSS file.</span></span>
>
> <span data-ttu-id="3405f-357"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> 是實用的提示，可表示是否已傳送標頭 (或) 是否已寫入本文。</span><span class="sxs-lookup"><span data-stu-id="3405f-357"><xref:Microsoft.AspNetCore.Http.HttpResponse.HasStarted*> is a useful hint to indicate if headers have been sent or the body has been written to.</span></span>

<a name="order"></a>

## <a name="middleware-order"></a><span data-ttu-id="3405f-358">中介順序</span><span class="sxs-lookup"><span data-stu-id="3405f-358">Middleware order</span></span>

<span data-ttu-id="3405f-359">`Startup.Configure` 方法內中介軟體元件的新增順序可定義在要求時叫用中介軟體元件的順序及回應的反向順序。</span><span class="sxs-lookup"><span data-stu-id="3405f-359">The order that middleware components are added in the `Startup.Configure` method defines the order in which the middleware components are invoked on requests and the reverse order for the response.</span></span> <span data-ttu-id="3405f-360">該順序對於安全性、性能和功能**至關重要**。</span><span class="sxs-lookup"><span data-stu-id="3405f-360">The order is **critical** for security, performance, and functionality.</span></span>

<span data-ttu-id="3405f-361">以下`Startup.Configure`方法按建議的順序新增安全相關的中間元件元件:</span><span class="sxs-lookup"><span data-stu-id="3405f-361">The following `Startup.Configure` method adds security related middleware components in the recommended order:</span></span>

[!code-csharp[](index/snapshot/Startup22.cs?name=snippet)]

<span data-ttu-id="3405f-362">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="3405f-362">In the preceding code:</span></span>

* <span data-ttu-id="3405f-363">註釋在使用[單個使用者帳戶](xref:security/authentication/identity)創建新 Web 應用時未添加的中間件。</span><span class="sxs-lookup"><span data-stu-id="3405f-363">Middleware that is not added when creating a new web app with [individual users accounts](xref:security/authentication/identity) is commented out.</span></span>
* <span data-ttu-id="3405f-364">並不是每個中間件都需要按這個確切的順序去做,但很多都這樣。</span><span class="sxs-lookup"><span data-stu-id="3405f-364">Not every middleware needs to go in this exact order, but many do.</span></span> <span data-ttu-id="3405f-365">例如,`UseCors`並且`UseAuthentication`必須 按顯示的順序進行。</span><span class="sxs-lookup"><span data-stu-id="3405f-365">For example, `UseCors` and `UseAuthentication` must go in the order shown.</span></span>

<span data-ttu-id="3405f-366">下列 `Startup.Configure` 方法會新增適用於一般應用程式案例的中介軟體元件：</span><span class="sxs-lookup"><span data-stu-id="3405f-366">The following `Startup.Configure` method adds middleware components for common app scenarios:</span></span>

1. <span data-ttu-id="3405f-367">例外狀況/錯誤處理</span><span class="sxs-lookup"><span data-stu-id="3405f-367">Exception/error handling</span></span>
   * <span data-ttu-id="3405f-368">當應用程式在開發環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="3405f-368">When the app runs in the Development environment:</span></span>
     * <span data-ttu-id="3405f-369">開發人員例外狀況頁面中介軟體 (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) 會回報應用程式執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="3405f-369">Developer Exception Page Middleware (<xref:Microsoft.AspNetCore.Builder.DeveloperExceptionPageExtensions.UseDeveloperExceptionPage*>) reports app runtime errors.</span></span>
     * <span data-ttu-id="3405f-370">資料錯誤頁面中介軟體 (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) 會回報資料庫執行階段錯誤。</span><span class="sxs-lookup"><span data-stu-id="3405f-370">Database Error Page Middleware (`Microsoft.AspNetCore.Builder.DatabaseErrorPageExtensions.UseDatabaseErrorPage`) reports database runtime errors.</span></span>
   * <span data-ttu-id="3405f-371">當應用程式在生產環境中執行時：</span><span class="sxs-lookup"><span data-stu-id="3405f-371">When the app runs in the Production environment:</span></span>
     * <span data-ttu-id="3405f-372">例外狀況處理常式中介軟體 (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) 會攔截在下列中介軟體中擲回的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-372">Exception Handler Middleware (<xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*>) catches exceptions thrown in the following middlewares.</span></span>
     * <span data-ttu-id="3405f-373">HTTP 靜態傳輸安全性通訊協定 (HSTS) 中介軟體 (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) 會新增 `Strict-Transport-Security` 標頭。</span><span class="sxs-lookup"><span data-stu-id="3405f-373">HTTP Strict Transport Security Protocol (HSTS) Middleware (<xref:Microsoft.AspNetCore.Builder.HstsBuilderExtensions.UseHsts*>) adds the `Strict-Transport-Security` header.</span></span>
1. <span data-ttu-id="3405f-374">HTTPS 重新導向中介軟體 (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) 會將 HTTP 要求重新導向到 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="3405f-374">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) redirects HTTP requests to HTTPS.</span></span>
1. <span data-ttu-id="3405f-375">靜態檔案中介軟體 (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) 會傳回靜態檔案並縮短進一步的要求處理時間。</span><span class="sxs-lookup"><span data-stu-id="3405f-375">Static File Middleware (<xref:Microsoft.AspNetCore.Builder.StaticFileExtensions.UseStaticFiles*>) returns static files and short-circuits further request processing.</span></span>
1. <span data-ttu-id="3405f-376">Cookie 原則中介軟體 (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) 會使應用程式符合歐盟一般資料保護歸調 (GDPR) 法規。</span><span class="sxs-lookup"><span data-stu-id="3405f-376">Cookie Policy Middleware (<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*>) conforms the app to the EU General Data Protection Regulation (GDPR) regulations.</span></span>
1. <span data-ttu-id="3405f-377">驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) 會嘗試在允許使用者存取安全資源之前先驗證使用者。</span><span class="sxs-lookup"><span data-stu-id="3405f-377">Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>) attempts to authenticate the user before they're allowed access to secure resources.</span></span>
1. <span data-ttu-id="3405f-378">工作階段中介軟體 (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) 會建立並維護工作階段狀態。</span><span class="sxs-lookup"><span data-stu-id="3405f-378">Session Middleware (<xref:Microsoft.AspNetCore.Builder.SessionMiddlewareExtensions.UseSession*>) establishes and maintains session state.</span></span> <span data-ttu-id="3405f-379">若應用程式使用工作階段狀態，請在 Cookie 原則中介軟體之後、MVC 中介軟體之前呼叫工作階段中介軟體。</span><span class="sxs-lookup"><span data-stu-id="3405f-379">If the app uses session state, call Session Middleware after Cookie Policy Middleware and before MVC Middleware.</span></span>
1. <span data-ttu-id="3405f-380">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) 以將 MVC 新增到要求管線。</span><span class="sxs-lookup"><span data-stu-id="3405f-380">MVC (<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>) to add MVC to the request pipeline.</span></span>

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

<span data-ttu-id="3405f-381">在上面的範例程式碼中，每個中介軟體擴充方法都會透過 <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> 命名空間在 <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> 上公開。</span><span class="sxs-lookup"><span data-stu-id="3405f-381">In the preceding example code, each middleware extension method is exposed on <xref:Microsoft.AspNetCore.Builder.IApplicationBuilder> through the <xref:Microsoft.AspNetCore.Builder?displayProperty=fullName> namespace.</span></span>

<span data-ttu-id="3405f-382"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> 是第一個新增到管道的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="3405f-382"><xref:Microsoft.AspNetCore.Builder.ExceptionHandlerExtensions.UseExceptionHandler*> is the first middleware component added to the pipeline.</span></span> <span data-ttu-id="3405f-383">因此，例外處理常式中介軟體會攔截後續呼叫中發生的所有例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3405f-383">Therefore, the Exception Handler Middleware catches any exceptions that occur in later calls.</span></span>

<span data-ttu-id="3405f-384">靜態檔案中介軟體會提前在管線中呼叫，以便其無須逐一處理剩餘的元件，就能處理要求及執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-384">Static File Middleware is called early in the pipeline so that it can handle requests and short-circuit without going through the remaining components.</span></span> <span data-ttu-id="3405f-385">靜態文件中間件**不**提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="3405f-385">The Static File Middleware provides **no** authorization checks.</span></span> <span data-ttu-id="3405f-386">靜態檔中間件提供的任何檔,包括*wwwroot*下的檔,都是公開的。</span><span class="sxs-lookup"><span data-stu-id="3405f-386">Any files served by Static File Middleware, including those under *wwwroot*, are publicly available.</span></span> <span data-ttu-id="3405f-387">如需保護靜態檔案的方法，請參閱 <xref:fundamentals/static-files>。</span><span class="sxs-lookup"><span data-stu-id="3405f-387">For an approach to secure static files, see <xref:fundamentals/static-files>.</span></span>

<span data-ttu-id="3405f-388">若靜態檔案中介軟體未處理要求，該要求會繼續傳遞給執行驗證的驗證中介軟體 (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>)。</span><span class="sxs-lookup"><span data-stu-id="3405f-388">If the request isn't handled by the Static File Middleware, it's passed on to the Authentication Middleware (<xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>), which performs authentication.</span></span> <span data-ttu-id="3405f-389">驗證不會對未經驗證的要求執行最少運算。</span><span class="sxs-lookup"><span data-stu-id="3405f-389">Authentication doesn't short-circuit unauthenticated requests.</span></span> <span data-ttu-id="3405f-390">雖然驗證中介軟體會驗證要求，但只有在 MVC 選取特定 Razor 頁面或 MVC 控制器及動作後，才會進行驗證 (與拒絕)。</span><span class="sxs-lookup"><span data-stu-id="3405f-390">Although Authentication Middleware authenticates requests, authorization (and rejection) occurs only after MVC selects a specific Razor Page or MVC controller and action.</span></span>

<span data-ttu-id="3405f-391">下列範例示範靜態檔案中介軟體在回應壓縮中介軟體之前處理靜態檔案要求之前的靜態檔案順序。</span><span class="sxs-lookup"><span data-stu-id="3405f-391">The following example demonstrates a middleware order where requests for static files are handled by Static File Middleware before Response Compression Middleware.</span></span> <span data-ttu-id="3405f-392">靜態檔案並不會以此中介軟體順序壓縮。</span><span class="sxs-lookup"><span data-stu-id="3405f-392">Static files aren't compressed with this middleware order.</span></span> <span data-ttu-id="3405f-393">來自 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> 的 MVC 回應可壓縮。</span><span class="sxs-lookup"><span data-stu-id="3405f-393">The MVC responses from <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvcWithDefaultRoute*> can be compressed.</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // Static files aren't compressed by Static File Middleware.
    app.UseStaticFiles();

    app.UseResponseCompression();

    app.UseMvcWithDefaultRoute();
}
```

## <a name="use-run-and-map"></a><span data-ttu-id="3405f-394">Use、Run 與 Map</span><span class="sxs-lookup"><span data-stu-id="3405f-394">Use, Run, and Map</span></span>

<span data-ttu-id="3405f-395">設定 HTTP 管線的方法是使用 <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>、<xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*> 及 <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>。</span><span class="sxs-lookup"><span data-stu-id="3405f-395">Configure the HTTP pipeline using <xref:Microsoft.AspNetCore.Builder.UseExtensions.Use*>, <xref:Microsoft.AspNetCore.Builder.RunExtensions.Run*>, and <xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*>.</span></span> <span data-ttu-id="3405f-396">`Use` 方法可對管線執行最少運算 (亦即，如果其不呼叫 `next` 要求委派的情況下)。</span><span class="sxs-lookup"><span data-stu-id="3405f-396">The `Use` method can short-circuit the pipeline (that is, if it doesn't call a `next` request delegate).</span></span> <span data-ttu-id="3405f-397">`Run` 是一種慣例，且有些中介軟體元件可能將執行於管線尾端的 `Run[Middleware]` 方法公開。</span><span class="sxs-lookup"><span data-stu-id="3405f-397">`Run` is a convention, and some middleware components may expose `Run[Middleware]` methods that run at the end of the pipeline.</span></span>

<span data-ttu-id="3405f-398"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> 擴充方法則是用來分支管線的慣例。</span><span class="sxs-lookup"><span data-stu-id="3405f-398"><xref:Microsoft.AspNetCore.Builder.MapExtensions.Map*> extensions are used as a convention for branching the pipeline.</span></span> <span data-ttu-id="3405f-399">`Map` 會依據指定要求路徑的相符項目將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-399">`Map` branches the request pipeline based on matches of the given request path.</span></span> <span data-ttu-id="3405f-400">如果要求路徑以指定路徑為開頭，則會執行分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-400">If the request path starts with the given path, the branch is executed.</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMap.cs)]

<span data-ttu-id="3405f-401">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="3405f-401">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="3405f-402">要求</span><span class="sxs-lookup"><span data-stu-id="3405f-402">Request</span></span>             | <span data-ttu-id="3405f-403">回應</span><span class="sxs-lookup"><span data-stu-id="3405f-403">Response</span></span>                     |
| ------------------- | ---------------------------- |
| <span data-ttu-id="3405f-404">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="3405f-404">localhost:1234</span></span>      | <span data-ttu-id="3405f-405">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="3405f-405">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="3405f-406">localhost:1234/map1</span><span class="sxs-lookup"><span data-stu-id="3405f-406">localhost:1234/map1</span></span> | <span data-ttu-id="3405f-407">Map Test 1</span><span class="sxs-lookup"><span data-stu-id="3405f-407">Map Test 1</span></span>                   |
| <span data-ttu-id="3405f-408">localhost:1234/map2</span><span class="sxs-lookup"><span data-stu-id="3405f-408">localhost:1234/map2</span></span> | <span data-ttu-id="3405f-409">Map Test 2</span><span class="sxs-lookup"><span data-stu-id="3405f-409">Map Test 2</span></span>                   |
| <span data-ttu-id="3405f-410">localhost:1234/map3</span><span class="sxs-lookup"><span data-stu-id="3405f-410">localhost:1234/map3</span></span> | <span data-ttu-id="3405f-411">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="3405f-411">Hello from non-Map delegate.</span></span> |

<span data-ttu-id="3405f-412">使用 `Map` 時，會將相符的路徑線段從 `HttpRequest.Path` 移除，並附加至每個要求的 `HttpRequest.PathBase`。</span><span class="sxs-lookup"><span data-stu-id="3405f-412">When `Map` is used, the matched path segments are removed from `HttpRequest.Path` and appended to `HttpRequest.PathBase` for each request.</span></span>

<span data-ttu-id="3405f-413"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> 會依據指定述詞的結果將要求管線分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-413"><xref:Microsoft.AspNetCore.Builder.MapWhenExtensions.MapWhen*> branches the request pipeline based on the result of the given predicate.</span></span> <span data-ttu-id="3405f-414">`Func<HttpContext, bool>` 類型的任何述詞皆可用來將要求對應至管線的新分支。</span><span class="sxs-lookup"><span data-stu-id="3405f-414">Any predicate of type `Func<HttpContext, bool>` can be used to map requests to a new branch of the pipeline.</span></span> <span data-ttu-id="3405f-415">下列範例會使用述詞來偵測查詢字串變數 `branch` 是否存在：</span><span class="sxs-lookup"><span data-stu-id="3405f-415">In the following example, a predicate is used to detect the presence of a query string variable `branch`:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMapWhen.cs)]

<span data-ttu-id="3405f-416">下表說明使用上述程式碼後，來自 `http://localhost:1234` 的要求及回應。</span><span class="sxs-lookup"><span data-stu-id="3405f-416">The following table shows the requests and responses from `http://localhost:1234` using the previous code.</span></span>

| <span data-ttu-id="3405f-417">要求</span><span class="sxs-lookup"><span data-stu-id="3405f-417">Request</span></span>                       | <span data-ttu-id="3405f-418">回應</span><span class="sxs-lookup"><span data-stu-id="3405f-418">Response</span></span>                     |
| ----------------------------- | ---------------------------- |
| <span data-ttu-id="3405f-419">localhost:1234</span><span class="sxs-lookup"><span data-stu-id="3405f-419">localhost:1234</span></span>                | <span data-ttu-id="3405f-420">Hello from non-Map delegate.</span><span class="sxs-lookup"><span data-stu-id="3405f-420">Hello from non-Map delegate.</span></span> |
| <span data-ttu-id="3405f-421">localhost:1234/?branch=master</span><span class="sxs-lookup"><span data-stu-id="3405f-421">localhost:1234/?branch=master</span></span> | <span data-ttu-id="3405f-422">Branch used = master</span><span class="sxs-lookup"><span data-stu-id="3405f-422">Branch used = master</span></span>         |

<span data-ttu-id="3405f-423">`Map` 支援巢狀項目，例如：</span><span class="sxs-lookup"><span data-stu-id="3405f-423">`Map` supports nesting, for example:</span></span>

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

<span data-ttu-id="3405f-424">`Map` 也可以一次比對多個線段：</span><span class="sxs-lookup"><span data-stu-id="3405f-424">`Map` can also match multiple segments at once:</span></span>

[!code-csharp[](index/snapshot/Chain/StartupMultiSeg.cs?highlight=13)]

## <a name="built-in-middleware"></a><span data-ttu-id="3405f-425">內建的中介軟體</span><span class="sxs-lookup"><span data-stu-id="3405f-425">Built-in middleware</span></span>

<span data-ttu-id="3405f-426">ASP.NET Core 隨附下列中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="3405f-426">ASP.NET Core ships with the following middleware components.</span></span> <span data-ttu-id="3405f-427">「順序」\*\* 欄說明 中介軟體在要求處理管線中的位置，以及中介軟體可終止要求處理的情況。</span><span class="sxs-lookup"><span data-stu-id="3405f-427">The *Order* column provides notes on middleware placement in the request processing pipeline and under what conditions the middleware may terminate request processing.</span></span> <span data-ttu-id="3405f-428">當中介軟體將要求處理管線短路並防止接下來的下游中介軟體處理要求時，這就是所謂的「終端中介軟體」\*\*。</span><span class="sxs-lookup"><span data-stu-id="3405f-428">When a middleware short-circuits the request processing pipeline and prevents further downstream middleware from processing a request, it's called a *terminal middleware*.</span></span> <span data-ttu-id="3405f-429">如需詳細資訊，請參閱[使用 IApplicationBuilder 建立中介軟體管線](#create-a-middleware-pipeline-with-iapplicationbuilder)。</span><span class="sxs-lookup"><span data-stu-id="3405f-429">For more information on short-circuiting, see the [Create a middleware pipeline with IApplicationBuilder](#create-a-middleware-pipeline-with-iapplicationbuilder) section.</span></span>

| <span data-ttu-id="3405f-430">中介軟體</span><span class="sxs-lookup"><span data-stu-id="3405f-430">Middleware</span></span> | <span data-ttu-id="3405f-431">描述</span><span class="sxs-lookup"><span data-stu-id="3405f-431">Description</span></span> | <span data-ttu-id="3405f-432">單</span><span class="sxs-lookup"><span data-stu-id="3405f-432">Order</span></span> |
| ---------- | ----------- | ----- |
| <span data-ttu-id="3405f-433">[驗證][](xref:security/authentication/identity)</span><span class="sxs-lookup"><span data-stu-id="3405f-433">[Authentication](xref:security/authentication/identity)</span></span> | <span data-ttu-id="3405f-434">提供驗證支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-434">Provides authentication support.</span></span> | <span data-ttu-id="3405f-435">在需要 `HttpContext.User` 之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-435">Before `HttpContext.User` is needed.</span></span> <span data-ttu-id="3405f-436">OAuth 回呼的終端機。</span><span class="sxs-lookup"><span data-stu-id="3405f-436">Terminal for OAuth callbacks.</span></span> |
| [<span data-ttu-id="3405f-437">Cookie 原則</span><span class="sxs-lookup"><span data-stu-id="3405f-437">Cookie Policy</span></span>](xref:security/gdpr) | <span data-ttu-id="3405f-438">追蹤使用者對用於儲存個人資訊的同意，並強制執行 Cookie 欄位的最低標準，例如 `secure` 和 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="3405f-438">Tracks consent from users for storing personal information and enforces minimum standards for cookie fields, such as `secure` and `SameSite`.</span></span> | <span data-ttu-id="3405f-439">在發出 Cookie 的中介軟體之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-439">Before middleware that issues cookies.</span></span> <span data-ttu-id="3405f-440">範例：驗證、工作階段、MVC (TempData)。</span><span class="sxs-lookup"><span data-stu-id="3405f-440">Examples: Authentication, Session, MVC (TempData).</span></span> |
| [<span data-ttu-id="3405f-441">CORS</span><span class="sxs-lookup"><span data-stu-id="3405f-441">CORS</span></span>](xref:security/cors) | <span data-ttu-id="3405f-442">設定跨原始來源資源共用。</span><span class="sxs-lookup"><span data-stu-id="3405f-442">Configures Cross-Origin Resource Sharing.</span></span> | <span data-ttu-id="3405f-443">在使用 CORS 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-443">Before components that use CORS.</span></span> |
| [<span data-ttu-id="3405f-444">診斷</span><span class="sxs-lookup"><span data-stu-id="3405f-444">Diagnostics</span></span>](xref:fundamentals/error-handling) | <span data-ttu-id="3405f-445">提供開發人員異常頁、異常處理、狀態代碼頁和新應用的預設網頁的幾個單獨的中間件。</span><span class="sxs-lookup"><span data-stu-id="3405f-445">Several separate middlewares that provide a developer exception page, exception handling, status code pages, and the default web page for new apps.</span></span> | <span data-ttu-id="3405f-446">在產生錯誤的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-446">Before components that generate errors.</span></span> <span data-ttu-id="3405f-447">用於異常或為新應用提供預設網頁的終端。</span><span class="sxs-lookup"><span data-stu-id="3405f-447">Terminal for exceptions or serving the default web page for new apps.</span></span> |
| [<span data-ttu-id="3405f-448">轉寄的標頭</span><span class="sxs-lookup"><span data-stu-id="3405f-448">Forwarded Headers</span></span>](xref:host-and-deploy/proxy-load-balancer) | <span data-ttu-id="3405f-449">將設為 Proxy 的標頭轉送到目前要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-449">Forwards proxied headers onto the current request.</span></span> | <span data-ttu-id="3405f-450">在使用更新方法的欄位之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-450">Before components that consume the updated fields.</span></span> <span data-ttu-id="3405f-451">範例：配置、主機，用戶端 IP、方法。</span><span class="sxs-lookup"><span data-stu-id="3405f-451">Examples: scheme, host, client IP, method.</span></span> |
| [<span data-ttu-id="3405f-452">執行狀況檢查</span><span class="sxs-lookup"><span data-stu-id="3405f-452">Health Check</span></span>](xref:host-and-deploy/health-checks) | <span data-ttu-id="3405f-453">檢查 ASP.NET Core 應用程式及其相依性的健康狀態，例如檢查資料庫可用性。</span><span class="sxs-lookup"><span data-stu-id="3405f-453">Checks the health of an ASP.NET Core app and its dependencies, such as checking database availability.</span></span> | <span data-ttu-id="3405f-454">若某項要求與健康狀態檢查端點相符，則會是終端機。</span><span class="sxs-lookup"><span data-stu-id="3405f-454">Terminal if a request matches a health check endpoint.</span></span> |
| [<span data-ttu-id="3405f-455">HTTP 方法覆寫</span><span class="sxs-lookup"><span data-stu-id="3405f-455">HTTP Method Override</span></span>](xref:Microsoft.AspNetCore.Builder.HttpMethodOverrideExtensions) | <span data-ttu-id="3405f-456">允許傳入的 POST 要求覆寫方法。</span><span class="sxs-lookup"><span data-stu-id="3405f-456">Allows an incoming POST request to override the method.</span></span> | <span data-ttu-id="3405f-457">在使用更新方法的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-457">Before components that consume the updated method.</span></span> |
| [<span data-ttu-id="3405f-458">HTTPS 重定向</span><span class="sxs-lookup"><span data-stu-id="3405f-458">HTTPS Redirection</span></span>](xref:security/enforcing-ssl#require-https) | <span data-ttu-id="3405f-459">將所有 HTTP 要求都重新導向至 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="3405f-459">Redirect all HTTP requests to HTTPS.</span></span> | <span data-ttu-id="3405f-460">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-460">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="3405f-461">HTTP 嚴格的傳輸安全性 (HSTS)</span><span class="sxs-lookup"><span data-stu-id="3405f-461">HTTP Strict Transport Security (HSTS)</span></span>](xref:security/enforcing-ssl#http-strict-transport-security-protocol-hsts) | <span data-ttu-id="3405f-462">增強安全性的中介軟體，可新增特殊的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="3405f-462">Security enhancement middleware that adds a special response header.</span></span> | <span data-ttu-id="3405f-463">在傳送回應前和修改要求的元件後。</span><span class="sxs-lookup"><span data-stu-id="3405f-463">Before responses are sent and after components that modify requests.</span></span> <span data-ttu-id="3405f-464">範例：轉送的標頭、URL 重寫。</span><span class="sxs-lookup"><span data-stu-id="3405f-464">Examples: Forwarded Headers, URL Rewriting.</span></span> |
| [<span data-ttu-id="3405f-465">MVC</span><span class="sxs-lookup"><span data-stu-id="3405f-465">MVC</span></span>](xref:mvc/overview) | <span data-ttu-id="3405f-466">使用 MVC/Razor 頁面處理要求。</span><span class="sxs-lookup"><span data-stu-id="3405f-466">Processes requests with MVC/Razor Pages.</span></span> | <span data-ttu-id="3405f-467">若要求符合路由則終止。</span><span class="sxs-lookup"><span data-stu-id="3405f-467">Terminal if a request matches a route.</span></span> |
| [<span data-ttu-id="3405f-468">OWIN</span><span class="sxs-lookup"><span data-stu-id="3405f-468">OWIN</span></span>](xref:fundamentals/owin) | <span data-ttu-id="3405f-469">以 OWIN 為基礎之應用程式、伺服器和中介軟體的 Interop。</span><span class="sxs-lookup"><span data-stu-id="3405f-469">Interop with OWIN-based apps, servers, and middleware.</span></span> | <span data-ttu-id="3405f-470">若 OWIN 中介軟體完全處理要求則終止。</span><span class="sxs-lookup"><span data-stu-id="3405f-470">Terminal if the OWIN Middleware fully processes the request.</span></span> |
| [<span data-ttu-id="3405f-471">回應快取</span><span class="sxs-lookup"><span data-stu-id="3405f-471">Response Caching</span></span>](xref:performance/caching/middleware) | <span data-ttu-id="3405f-472">提供快取回應的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-472">Provides support for caching responses.</span></span> | <span data-ttu-id="3405f-473">在需要快取的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-473">Before components that require caching.</span></span> |
| [<span data-ttu-id="3405f-474">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="3405f-474">Response Compression</span></span>](xref:performance/response-compression) | <span data-ttu-id="3405f-475">提供壓縮回應的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-475">Provides support for compressing responses.</span></span> | <span data-ttu-id="3405f-476">在需要壓縮的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-476">Before components that require compression.</span></span> |
| [<span data-ttu-id="3405f-477">要求當地語系化</span><span class="sxs-lookup"><span data-stu-id="3405f-477">Request Localization</span></span>](xref:fundamentals/localization) | <span data-ttu-id="3405f-478">提供當地語系化支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-478">Provides localization support.</span></span> | <span data-ttu-id="3405f-479">在偵測當地語系化的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-479">Before localization sensitive components.</span></span> |
| [<span data-ttu-id="3405f-480">端點路由</span><span class="sxs-lookup"><span data-stu-id="3405f-480">Endpoint Routing</span></span>](xref:fundamentals/routing) | <span data-ttu-id="3405f-481">定義並限制要求路由。</span><span class="sxs-lookup"><span data-stu-id="3405f-481">Defines and constrains request routes.</span></span> | <span data-ttu-id="3405f-482">比對路由的終端機。</span><span class="sxs-lookup"><span data-stu-id="3405f-482">Terminal for matching routes.</span></span> |
| [<span data-ttu-id="3405f-483">工作階段</span><span class="sxs-lookup"><span data-stu-id="3405f-483">Session</span></span>](xref:fundamentals/app-state) | <span data-ttu-id="3405f-484">提供管理使用者工作階段的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-484">Provides support for managing user sessions.</span></span> | <span data-ttu-id="3405f-485">在需要工作階段的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-485">Before components that require Session.</span></span> |
| [<span data-ttu-id="3405f-486">靜態檔案</span><span class="sxs-lookup"><span data-stu-id="3405f-486">Static Files</span></span>](xref:fundamentals/static-files) | <span data-ttu-id="3405f-487">支援靜態檔案的提供和目錄瀏覽。</span><span class="sxs-lookup"><span data-stu-id="3405f-487">Provides support for serving static files and directory browsing.</span></span> | <span data-ttu-id="3405f-488">若要求符合檔案則終止。</span><span class="sxs-lookup"><span data-stu-id="3405f-488">Terminal if a request matches a file.</span></span> |
| [<span data-ttu-id="3405f-489">URL 重寫</span><span class="sxs-lookup"><span data-stu-id="3405f-489">URL Rewrite</span></span>](xref:fundamentals/url-rewriting) | <span data-ttu-id="3405f-490">提供重寫 URL 及重新導向要求的支援。</span><span class="sxs-lookup"><span data-stu-id="3405f-490">Provides support for rewriting URLs and redirecting requests.</span></span> | <span data-ttu-id="3405f-491">在使用 URL 的元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-491">Before components that consume the URL.</span></span> |
| [<span data-ttu-id="3405f-492">WebSocket</span><span class="sxs-lookup"><span data-stu-id="3405f-492">WebSockets</span></span>](xref:fundamentals/websockets) | <span data-ttu-id="3405f-493">啟用 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="3405f-493">Enables the WebSockets protocol.</span></span> | <span data-ttu-id="3405f-494">在接受 WebSocket 要求的必要元件之前。</span><span class="sxs-lookup"><span data-stu-id="3405f-494">Before components that are required to accept WebSocket requests.</span></span> |

## <a name="additional-resources"></a><span data-ttu-id="3405f-495">其他資源</span><span class="sxs-lookup"><span data-stu-id="3405f-495">Additional resources</span></span>

* <xref:fundamentals/middleware/write>
* <xref:migration/http-modules>
* <xref:fundamentals/startup>
* <xref:fundamentals/request-features>
* <xref:fundamentals/middleware/extensibility>
* <xref:fundamentals/middleware/extensibility-third-party-container>

::: moniker-end
