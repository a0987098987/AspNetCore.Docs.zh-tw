---
title: 將 HTTP 處理常式和模組遷移至 ASP.NET Core 中介軟體
author: rick-anderson
description: ''
ms.author: riande
ms.date: 12/07/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/http-modules
ms.openlocfilehash: 214e3fa86a1418f04a5e292cdc1b4baac8c75643
ms.sourcegitcommit: 4437f4c149f1ef6c28796dcfaa2863b4c088169c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/19/2020
ms.locfileid: "85074178"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="38e0c-102">將 HTTP 處理常式和模組遷移至 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="38e0c-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="38e0c-103">本文說明如何將 ASP.NET[的現有 HTTP 模組和處理常式，從 system.webserver](/iis/configuration/system.webserver/)遷移至 ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="38e0c-103">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="38e0c-104">已再次進行模組和處理常式</span><span class="sxs-lookup"><span data-stu-id="38e0c-104">Modules and handlers revisited</span></span>

<span data-ttu-id="38e0c-105">在繼續 ASP.NET Core 中介軟體之前，讓我們先回顧一下 HTTP 模組和處理常式的操作方式：</span><span class="sxs-lookup"><span data-stu-id="38e0c-105">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![模組處理常式](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="38e0c-107">**處理常式包括：**</span><span class="sxs-lookup"><span data-stu-id="38e0c-107">**Handlers are:**</span></span>

* <span data-ttu-id="38e0c-108">執行[IHttpHandler](/dotnet/api/system.web.ihttphandler)的類別</span><span class="sxs-lookup"><span data-stu-id="38e0c-108">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

* <span data-ttu-id="38e0c-109">用來處理指定的檔案名或副檔名的要求，例如 *. report*</span><span class="sxs-lookup"><span data-stu-id="38e0c-109">Used to handle requests with a given file name or extension, such as *.report*</span></span>

* <span data-ttu-id="38e0c-110">在*Web.config*中[設定](/iis/configuration/system.webserver/handlers/)</span><span class="sxs-lookup"><span data-stu-id="38e0c-110">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="38e0c-111">**模組包括：**</span><span class="sxs-lookup"><span data-stu-id="38e0c-111">**Modules are:**</span></span>

* <span data-ttu-id="38e0c-112">執行[IHttpModule](/dotnet/api/system.web.ihttpmodule)的類別</span><span class="sxs-lookup"><span data-stu-id="38e0c-112">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

* <span data-ttu-id="38e0c-113">針對每個要求叫用</span><span class="sxs-lookup"><span data-stu-id="38e0c-113">Invoked for every request</span></span>

* <span data-ttu-id="38e0c-114">能夠短路（停止進一步處理要求）</span><span class="sxs-lookup"><span data-stu-id="38e0c-114">Able to short-circuit (stop further processing of a request)</span></span>

* <span data-ttu-id="38e0c-115">能夠新增至 HTTP 回應，或自行建立</span><span class="sxs-lookup"><span data-stu-id="38e0c-115">Able to add to the HTTP response, or create their own</span></span>

* <span data-ttu-id="38e0c-116">在*Web.config*中[設定](/iis/configuration/system.webserver/modules/)</span><span class="sxs-lookup"><span data-stu-id="38e0c-116">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="38e0c-117">**模組處理傳入要求的順序取決於：**</span><span class="sxs-lookup"><span data-stu-id="38e0c-117">**The order in which modules process incoming requests is determined by:**</span></span>

1. <span data-ttu-id="38e0c-118">[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)，這是由 ASP.NET 所引發的數列事件： [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest)、 [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)等等。每個模組都可以建立一個或多個事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="38e0c-118">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

2. <span data-ttu-id="38e0c-119">針對相同的事件，在*Web.config*中設定的順序。</span><span class="sxs-lookup"><span data-stu-id="38e0c-119">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="38e0c-120">除了模組之外，您還可以將生命週期事件的處理常式新增至*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="38e0c-120">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="38e0c-121">這些處理常式會在已設定之模組中的處理常式之後執行。</span><span class="sxs-lookup"><span data-stu-id="38e0c-121">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="38e0c-122">從處理常式和模組到中介軟體</span><span class="sxs-lookup"><span data-stu-id="38e0c-122">From handlers and modules to middleware</span></span>

<span data-ttu-id="38e0c-123">**中介軟體比 HTTP 模組和處理常式簡單：**</span><span class="sxs-lookup"><span data-stu-id="38e0c-123">**Middleware are simpler than HTTP modules and handlers:**</span></span>

* <span data-ttu-id="38e0c-124">模組、處理常式、 *Global.asax.cs*、 *Web.config* （IIS 設定除外）和應用程式生命週期都已消失</span><span class="sxs-lookup"><span data-stu-id="38e0c-124">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

* <span data-ttu-id="38e0c-125">中介軟體已接管模組和處理常式的角色</span><span class="sxs-lookup"><span data-stu-id="38e0c-125">The roles of both modules and handlers have been taken over by middleware</span></span>

* <span data-ttu-id="38e0c-126">中介軟體是使用程式碼來設定，而不是在*Web.config*</span><span class="sxs-lookup"><span data-stu-id="38e0c-126">Middleware are configured using code rather than in *Web.config*</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="38e0c-127">[管線分支](xref:fundamentals/middleware/index#branch-the-middleware-pipeline)可讓您將要求傳送至特定中介軟體，而不只是 URL，也會根據要求標頭、查詢字串等。</span><span class="sxs-lookup"><span data-stu-id="38e0c-127">[Pipeline branching](xref:fundamentals/middleware/index#branch-the-middleware-pipeline) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

::: moniker-end
::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="38e0c-128">[管線分支](xref:fundamentals/middleware/index#use-run-and-map)可讓您將要求傳送至特定中介軟體，而不只是 URL，也會根據要求標頭、查詢字串等。</span><span class="sxs-lookup"><span data-stu-id="38e0c-128">[Pipeline branching](xref:fundamentals/middleware/index#use-run-and-map) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

::: moniker-end

<span data-ttu-id="38e0c-129">**中介軟體非常類似模組：**</span><span class="sxs-lookup"><span data-stu-id="38e0c-129">**Middleware are very similar to modules:**</span></span>

* <span data-ttu-id="38e0c-130">在每個要求的主體中叫用</span><span class="sxs-lookup"><span data-stu-id="38e0c-130">Invoked in principle for every request</span></span>

* <span data-ttu-id="38e0c-131">無法將[要求傳遞至下一個中介軟體](#http-modules-shortcircuiting-middleware)，藉此縮短要求的路線</span><span class="sxs-lookup"><span data-stu-id="38e0c-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

* <span data-ttu-id="38e0c-132">能夠建立自己的 HTTP 回應</span><span class="sxs-lookup"><span data-stu-id="38e0c-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="38e0c-133">**中介軟體和模組會以不同的順序進行處理：**</span><span class="sxs-lookup"><span data-stu-id="38e0c-133">**Middleware and modules are processed in a different order:**</span></span>

* <span data-ttu-id="38e0c-134">中介軟體的順序是根據它們插入要求管線的順序，而模組的順序則主要是根據[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)事件</span><span class="sxs-lookup"><span data-stu-id="38e0c-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

* <span data-ttu-id="38e0c-135">回應的中介軟體順序與要求相反，而模組的順序則與要求和回應相同</span><span class="sxs-lookup"><span data-stu-id="38e0c-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

* <span data-ttu-id="38e0c-136">請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="38e0c-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![中介軟體](http-modules/_static/middleware.png)

<span data-ttu-id="38e0c-138">請注意，在上圖中，驗證中介軟體會簡短縮短要求。</span><span class="sxs-lookup"><span data-stu-id="38e0c-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="38e0c-139">將模組程式碼遷移至中介軟體</span><span class="sxs-lookup"><span data-stu-id="38e0c-139">Migrating module code to middleware</span></span>

<span data-ttu-id="38e0c-140">現有的 HTTP 模組看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="38e0c-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="38e0c-141">如[中介軟體](xref:fundamentals/middleware/index)頁面所示，ASP.NET Core 中介軟體是一個類別，它會公開 `Invoke` 採用 `HttpContext` 並傳回的方法 `Task` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="38e0c-142">您的新中介軟體看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="38e0c-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="38e0c-143">先前的中介軟體範本取自[撰寫中介軟體](xref:fundamentals/middleware/write)的一節。</span><span class="sxs-lookup"><span data-stu-id="38e0c-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/write).</span></span>

<span data-ttu-id="38e0c-144">*MyMiddlewareExtensions* helper 類別可讓您更輕鬆地在類別中設定中介軟體 `Startup` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="38e0c-145">`UseMyMiddleware`方法會將中介軟體類別新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="38e0c-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="38e0c-146">中介軟體所需的服務會插入中介軟體的函式中。</span><span class="sxs-lookup"><span data-stu-id="38e0c-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="38e0c-147">您的模組可能會終止要求，例如，如果使用者未獲授權：</span><span class="sxs-lookup"><span data-stu-id="38e0c-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="38e0c-148">中介軟體會藉由不在 `Invoke` 管線中的下一個中介軟體上呼叫來處理這種情況。</span><span class="sxs-lookup"><span data-stu-id="38e0c-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="38e0c-149">請記住，這並不會完全終止要求，因為當回應透過管線回傳時，仍然會叫用先前的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="38e0c-150">當您將模組的功能遷移至新的中介軟體時，您可能會發現程式碼不會編譯，因為在 `HttpContext` ASP.NET Core 中，類別已大幅變更。</span><span class="sxs-lookup"><span data-stu-id="38e0c-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="38e0c-151">[稍後](#migrating-to-the-new-httpcontext)，您將瞭解如何遷移至新的 ASP.NET Core HttpCoNtext。</span><span class="sxs-lookup"><span data-stu-id="38e0c-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="38e0c-152">將模組插入遷移至要求管線</span><span class="sxs-lookup"><span data-stu-id="38e0c-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="38e0c-153">HTTP 模組通常會使用*Web.config*新增至要求管線：</span><span class="sxs-lookup"><span data-stu-id="38e0c-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="38e0c-154">將您的[新中介軟體新增](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)至類別中的要求管線，以轉換此項 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="38e0c-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="38e0c-155">管線中您插入新中介軟體的確切位置，取決於它在Web.config的模組清單中處理為模組的事件（ `BeginRequest` 、 `EndRequest` 等）及其順序。 *Web.config*</span><span class="sxs-lookup"><span data-stu-id="38e0c-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="38e0c-156">如先前所述，ASP.NET Core 中沒有應用程式生命週期，中介軟體處理回應的順序與模組使用的順序不同。</span><span class="sxs-lookup"><span data-stu-id="38e0c-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="38e0c-157">這可能會讓您的訂購決策更具挑戰性。</span><span class="sxs-lookup"><span data-stu-id="38e0c-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="38e0c-158">如果順序變成問題，您可以將模組分割成可獨立排序的多個中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="38e0c-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="38e0c-159">將處理常式程式碼遷移至中介軟體</span><span class="sxs-lookup"><span data-stu-id="38e0c-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="38e0c-160">HTTP 處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="38e0c-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="38e0c-161">在您的 ASP.NET Core 專案中，您會將其轉譯為中介軟體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="38e0c-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="38e0c-162">這個中介軟體非常類似于模組的對應中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="38e0c-163">唯一的真正差異在於，這裡沒有對的呼叫 `_next.Invoke(context)` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="38e0c-164">這是合理的，因為處理常式是在要求管線的結尾，因此不會叫用下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="38e0c-165">將處理常式插入遷移至要求管線</span><span class="sxs-lookup"><span data-stu-id="38e0c-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="38e0c-166">設定 HTTP 處理常式是在*Web.config*中完成，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="38e0c-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="38e0c-167">您可以將新的處理常式中介軟體新增至類別中的要求管線，以進行轉換 `Startup` ，類似于從模組轉換的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="38e0c-168">該方法的問題在於，它會將所有要求傳送至新的處理常式中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="38e0c-169">不過，您只想要要求具有指定的延伸模組，才能到達您的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="38e0c-170">這會提供您與 HTTP 處理常式相同的功能。</span><span class="sxs-lookup"><span data-stu-id="38e0c-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="38e0c-171">其中一個解決方法是使用擴充方法，將管線分支給具有給定延伸的要求 `MapWhen` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="38e0c-172">您可以在 `Configure` 新增其他中介軟體的相同方法中執行此動作：</span><span class="sxs-lookup"><span data-stu-id="38e0c-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="38e0c-173">`MapWhen`會採用下列參數：</span><span class="sxs-lookup"><span data-stu-id="38e0c-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="38e0c-174">使用的 lambda， `HttpContext` 如果要求應該在分支中，則會傳回 `true` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="38e0c-175">這表示您不僅可以根據要求的延伸模組，也會根據要求標頭、查詢字串參數等來分支要求。</span><span class="sxs-lookup"><span data-stu-id="38e0c-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="38e0c-176">採用 `IApplicationBuilder` 並新增分支之所有中介軟體的 lambda。</span><span class="sxs-lookup"><span data-stu-id="38e0c-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="38e0c-177">這表示您可以將其他中介軟體新增至處理常式中介軟體前方的分支。</span><span class="sxs-lookup"><span data-stu-id="38e0c-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="38e0c-178">在所有要求上叫用分支之前，已將中介軟體新增至管線;分支不會對它們產生任何影響。</span><span class="sxs-lookup"><span data-stu-id="38e0c-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="38e0c-179">使用選項模式載入中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="38e0c-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="38e0c-180">某些模組和處理常式都有儲存在*Web.config*中的設定選項。不過，在 ASP.NET Core 會使用新的設定模型來取代*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="38e0c-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="38e0c-181">新的設定[系統](xref:fundamentals/configuration/index)會提供您下列選項來解決此問題：</span><span class="sxs-lookup"><span data-stu-id="38e0c-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="38e0c-182">將選項直接插入中介軟體，如下一[節](#loading-middleware-options-through-direct-injection)所示。</span><span class="sxs-lookup"><span data-stu-id="38e0c-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="38e0c-183">使用[選項模式](xref:fundamentals/configuration/options)：</span><span class="sxs-lookup"><span data-stu-id="38e0c-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="38e0c-184">建立類別來存放中介軟體選項，例如：</span><span class="sxs-lookup"><span data-stu-id="38e0c-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="38e0c-185">儲存選項值</span><span class="sxs-lookup"><span data-stu-id="38e0c-185">Store the option values</span></span>

   <span data-ttu-id="38e0c-186">設定系統可讓您將選項值儲存在您想要的任何位置。</span><span class="sxs-lookup"><span data-stu-id="38e0c-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="38e0c-187">不過，大部分的網站都使用*appsettings.js*，因此我們將採用該方法：</span><span class="sxs-lookup"><span data-stu-id="38e0c-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="38e0c-188">這裡的*MyMiddlewareOptionsSection*是區段名稱。</span><span class="sxs-lookup"><span data-stu-id="38e0c-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="38e0c-189">它不一定要與選項類別的名稱相同。</span><span class="sxs-lookup"><span data-stu-id="38e0c-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="38e0c-190">將選項值與選項類別產生關聯</span><span class="sxs-lookup"><span data-stu-id="38e0c-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="38e0c-191">選項模式會使用 ASP.NET Core 的相依性插入架構，將選項類型（例如 `MyMiddlewareOptions` ）與 `MyMiddlewareOptions` 具有實際選項的物件產生關聯。</span><span class="sxs-lookup"><span data-stu-id="38e0c-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="38e0c-192">更新您的 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="38e0c-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="38e0c-193">如果您使用*上的appsettings.js*，請將它新增至函式中的設定產生器 `Startup` ：</span><span class="sxs-lookup"><span data-stu-id="38e0c-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="38e0c-194">設定選項服務：</span><span class="sxs-lookup"><span data-stu-id="38e0c-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="38e0c-195">將您的選項與選項類別產生關聯：</span><span class="sxs-lookup"><span data-stu-id="38e0c-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="38e0c-196">將選項插入中介軟體的函式。</span><span class="sxs-lookup"><span data-stu-id="38e0c-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="38e0c-197">這類似于將選項插入控制器中。</span><span class="sxs-lookup"><span data-stu-id="38e0c-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="38e0c-198">將中介軟體新增至的[UseMiddleware](#http-modules-usemiddleware)擴充方法 `IApplicationBuilder` 會負責相依性插入。</span><span class="sxs-lookup"><span data-stu-id="38e0c-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="38e0c-199">這不限於 `IOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="38e0c-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="38e0c-200">中介軟體所需的任何其他物件都可以用這種方式插入。</span><span class="sxs-lookup"><span data-stu-id="38e0c-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="38e0c-201">透過直接插入來載入中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="38e0c-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="38e0c-202">[選項] 模式的優點是它會在選項值及其取用者之間建立鬆散結合。</span><span class="sxs-lookup"><span data-stu-id="38e0c-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="38e0c-203">當您將 options 類別與實際的選項值建立關聯之後，任何其他類別都可以透過相依性插入架構取得選項的存取權。</span><span class="sxs-lookup"><span data-stu-id="38e0c-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="38e0c-204">不需要傳遞選項值。</span><span class="sxs-lookup"><span data-stu-id="38e0c-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="38e0c-205">不過，如果您想要使用相同的中介軟體兩次，但有不同的選項，則會中斷。</span><span class="sxs-lookup"><span data-stu-id="38e0c-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="38e0c-206">例如，在允許不同角色的不同分支中使用的授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="38e0c-207">您無法將兩個不同的選項物件與一個選項類別產生關聯。</span><span class="sxs-lookup"><span data-stu-id="38e0c-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="38e0c-208">解決方法是使用類別中的實際選項值來取得 options 物件 `Startup` ，並將它們直接傳遞至中介軟體的每個實例。</span><span class="sxs-lookup"><span data-stu-id="38e0c-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="38e0c-209">*在appsettings.js上*新增第二個金鑰</span><span class="sxs-lookup"><span data-stu-id="38e0c-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="38e0c-210">若要將第二組選項新增至*appsettings.json*檔案，請使用新的金鑰來唯一識別它：</span><span class="sxs-lookup"><span data-stu-id="38e0c-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="38e0c-211">取出選項值，並將它們傳遞至中介軟體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="38e0c-212">`Use...`擴充方法（會將中介軟體新增至管線）是傳遞選項值的邏輯位置：</span><span class="sxs-lookup"><span data-stu-id="38e0c-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="38e0c-213">啟用中介軟體以接受選項參數。</span><span class="sxs-lookup"><span data-stu-id="38e0c-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="38e0c-214">提供擴充方法的多載 `Use...` （會採用 options 參數並將它傳遞給 `UseMiddleware` ）。</span><span class="sxs-lookup"><span data-stu-id="38e0c-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="38e0c-215">當 `UseMiddleware` 以參數呼叫時，它會在具現化中介軟體物件時，將參數傳遞至中介軟體的函式。</span><span class="sxs-lookup"><span data-stu-id="38e0c-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="38e0c-216">請注意，這會將選項物件包裝在 `OptionsWrapper` 物件中。</span><span class="sxs-lookup"><span data-stu-id="38e0c-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="38e0c-217">這會依照 `IOptions` 中介軟體函式的預期來執行。</span><span class="sxs-lookup"><span data-stu-id="38e0c-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="38e0c-218">遷移至新的 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="38e0c-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="38e0c-219">您稍早看到 `Invoke` 中介軟體中的方法採用類型的參數 `HttpContext` ：</span><span class="sxs-lookup"><span data-stu-id="38e0c-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="38e0c-220">`HttpContext`在 ASP.NET Core 中已大幅變更。</span><span class="sxs-lookup"><span data-stu-id="38e0c-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="38e0c-221">本節說明如何將[system.web](/dotnet/api/system.web.httpcontext)的最常使用屬性轉譯為新的 `Microsoft.AspNetCore.Http.HttpContext` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="38e0c-222">HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="38e0c-222">HttpContext</span></span>

<span data-ttu-id="38e0c-223">**HttpCoNtext 的專案**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="38e0c-224">**唯一要求識別碼（不含 System.web. HttpCoNtext 對應）**</span><span class="sxs-lookup"><span data-stu-id="38e0c-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="38e0c-225">為每個要求提供唯一的識別碼。</span><span class="sxs-lookup"><span data-stu-id="38e0c-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="38e0c-226">在記錄中包含非常有用的。</span><span class="sxs-lookup"><span data-stu-id="38e0c-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="38e0c-227">HttpCoNtext 要求</span><span class="sxs-lookup"><span data-stu-id="38e0c-227">HttpContext.Request</span></span>

<span data-ttu-id="38e0c-228">**HttpMethod**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="38e0c-229">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="38e0c-230">**HttpContext.Request.Url** **RawUrl**會轉譯為：（& i）</span><span class="sxs-lookup"><span data-stu-id="38e0c-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="38e0c-231">**IsSecureConnection**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="38e0c-232">**UserHostAddress**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="38e0c-233">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="38e0c-234">**RequestCoNtext. RouteData**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="38e0c-235">**HttpcoNtext.current**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="38e0c-236">**UserAgent**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="38e0c-237">**UrlReferrer**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="38e0c-238">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="38e0c-239">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="38e0c-240">只有在 content 子類型為*x-www-表單 urlencoded*或*表單資料*時，才讀取表單值。</span><span class="sxs-lookup"><span data-stu-id="38e0c-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="38e0c-241">**InputStream**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="38e0c-242">請只在管線結尾的處理常式類型中介軟體中使用此程式碼。</span><span class="sxs-lookup"><span data-stu-id="38e0c-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="38e0c-243">您可以讀取每個要求只顯示一次的原始本文。</span><span class="sxs-lookup"><span data-stu-id="38e0c-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="38e0c-244">在第一次讀取之後嘗試讀取本文的中介軟體將會讀取空白主體。</span><span class="sxs-lookup"><span data-stu-id="38e0c-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="38e0c-245">這並不適用于讀取如先前所示的表單，因為這是從緩衝區完成的。</span><span class="sxs-lookup"><span data-stu-id="38e0c-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="38e0c-246">HttpCoNtext 回應</span><span class="sxs-lookup"><span data-stu-id="38e0c-246">HttpContext.Response</span></span>

<span data-ttu-id="38e0c-247">**HttpContext.Response.Status** **StatusDescription**會轉譯為下列內容：</span><span class="sxs-lookup"><span data-stu-id="38e0c-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="38e0c-248">**ContentEncoding**和**HTTPcoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="38e0c-249">**HttpCoNtext**本身的 ContentType 也會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="38e0c-250">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="38e0c-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="38e0c-251">**HttpCoNtext. Response TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="38e0c-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="38e0c-252">[這裡](../fundamentals/request-features.md#middleware-and-request-features)會討論如何提供檔案。</span><span class="sxs-lookup"><span data-stu-id="38e0c-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="38e0c-253">**HttpCoNtext. 標頭**</span><span class="sxs-lookup"><span data-stu-id="38e0c-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="38e0c-254">傳送回應標頭很複雜，因為如果您在任何專案都已寫入回應主體之後設定它們，則不會將它們送出。</span><span class="sxs-lookup"><span data-stu-id="38e0c-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="38e0c-255">解決方案是設定回呼方法，在寫入回應開始之前，會先呼叫它。</span><span class="sxs-lookup"><span data-stu-id="38e0c-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="38e0c-256">這是在中介軟體的方法開頭進行的最佳做法 `Invoke` 。</span><span class="sxs-lookup"><span data-stu-id="38e0c-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="38e0c-257">這是設定回應標頭的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="38e0c-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="38e0c-258">下列程式碼會設定名為的回呼方法 `SetHeaders` ：</span><span class="sxs-lookup"><span data-stu-id="38e0c-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="38e0c-259">`SetHeaders`回呼方法看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="38e0c-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="38e0c-260">**HttpCoNtext. 回應 Cookie**</span><span class="sxs-lookup"><span data-stu-id="38e0c-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="38e0c-261">Cookie 會以*設定-Cookie*回應標頭傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="38e0c-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="38e0c-262">因此，傳送 cookie 需要與用來傳送回應標頭相同的回呼：</span><span class="sxs-lookup"><span data-stu-id="38e0c-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="38e0c-263">`SetCookies`回呼方法看起來會像下面這樣：</span><span class="sxs-lookup"><span data-stu-id="38e0c-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="38e0c-264">其他資源</span><span class="sxs-lookup"><span data-stu-id="38e0c-264">Additional resources</span></span>

* [<span data-ttu-id="38e0c-265">HTTP 處理常式和 HTTP 模組總覽</span><span class="sxs-lookup"><span data-stu-id="38e0c-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="38e0c-266">設定</span><span class="sxs-lookup"><span data-stu-id="38e0c-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="38e0c-267">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="38e0c-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="38e0c-268">中介軟體</span><span class="sxs-lookup"><span data-stu-id="38e0c-268">Middleware</span></span>](xref:fundamentals/middleware/index)
