---
title: 將 HTTP 處理常式和模組遷移至 ASP.NET Core 中介軟體
author: rick-anderson
description: ''
ms.author: riande
ms.date: 12/07/2016
uid: migration/http-modules
ms.openlocfilehash: bdf27ccb742d4bc05bac71e6c96d71c38dcb4b62
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659681"
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="caee0-102">將 HTTP 處理常式和模組遷移至 ASP.NET Core 中介軟體</span><span class="sxs-lookup"><span data-stu-id="caee0-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="caee0-103">本文說明如何將 ASP.NET[的現有 HTTP 模組和處理常式，從 system.webserver](/iis/configuration/system.webserver/)遷移至 ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="caee0-103">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="caee0-104">已再次進行模組和處理常式</span><span class="sxs-lookup"><span data-stu-id="caee0-104">Modules and handlers revisited</span></span>

<span data-ttu-id="caee0-105">在繼續 ASP.NET Core 中介軟體之前，讓我們先回顧一下 HTTP 模組和處理常式的操作方式：</span><span class="sxs-lookup"><span data-stu-id="caee0-105">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![模組處理常式](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="caee0-107">**處理常式包括：**</span><span class="sxs-lookup"><span data-stu-id="caee0-107">**Handlers are:**</span></span>

* <span data-ttu-id="caee0-108">執行[IHttpHandler](/dotnet/api/system.web.ihttphandler)的類別</span><span class="sxs-lookup"><span data-stu-id="caee0-108">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

* <span data-ttu-id="caee0-109">用來處理指定的檔案名或副檔名的要求，例如 *. report*</span><span class="sxs-lookup"><span data-stu-id="caee0-109">Used to handle requests with a given file name or extension, such as *.report*</span></span>

* <span data-ttu-id="caee0-110">*在 web.config*中[設定](/iis/configuration/system.webserver/handlers/)</span><span class="sxs-lookup"><span data-stu-id="caee0-110">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="caee0-111">**模組包括：**</span><span class="sxs-lookup"><span data-stu-id="caee0-111">**Modules are:**</span></span>

* <span data-ttu-id="caee0-112">執行[IHttpModule](/dotnet/api/system.web.ihttpmodule)的類別</span><span class="sxs-lookup"><span data-stu-id="caee0-112">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

* <span data-ttu-id="caee0-113">針對每個要求叫用</span><span class="sxs-lookup"><span data-stu-id="caee0-113">Invoked for every request</span></span>

* <span data-ttu-id="caee0-114">能夠短路（停止進一步處理要求）</span><span class="sxs-lookup"><span data-stu-id="caee0-114">Able to short-circuit (stop further processing of a request)</span></span>

* <span data-ttu-id="caee0-115">能夠新增至 HTTP 回應，或自行建立</span><span class="sxs-lookup"><span data-stu-id="caee0-115">Able to add to the HTTP response, or create their own</span></span>

* <span data-ttu-id="caee0-116">*在 web.config*中[設定](/iis/configuration/system.webserver/modules/)</span><span class="sxs-lookup"><span data-stu-id="caee0-116">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="caee0-117">**模組處理傳入要求的順序取決於：**</span><span class="sxs-lookup"><span data-stu-id="caee0-117">**The order in which modules process incoming requests is determined by:**</span></span>

1. <span data-ttu-id="caee0-118">[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)，這是由 ASP.NET 所引發的數列事件： [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest)、 [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)等等。每個模組都可以建立一個或多個事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="caee0-118">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

2. <span data-ttu-id="caee0-119">針對相同的事件，這是在*web.config*中設定的順序。</span><span class="sxs-lookup"><span data-stu-id="caee0-119">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="caee0-120">除了模組之外，您還可以將生命週期事件的處理常式新增至*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="caee0-120">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="caee0-121">這些處理常式會在已設定之模組中的處理常式之後執行。</span><span class="sxs-lookup"><span data-stu-id="caee0-121">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="caee0-122">從處理常式和模組到中介軟體</span><span class="sxs-lookup"><span data-stu-id="caee0-122">From handlers and modules to middleware</span></span>

<span data-ttu-id="caee0-123">**中介軟體比 HTTP 模組和處理常式簡單：**</span><span class="sxs-lookup"><span data-stu-id="caee0-123">**Middleware are simpler than HTTP modules and handlers:**</span></span>

* <span data-ttu-id="caee0-124">模組、處理常式、 *Global.asax.cs*、 *web.config （IIS*設定除外）和應用程式生命週期都已消失</span><span class="sxs-lookup"><span data-stu-id="caee0-124">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

* <span data-ttu-id="caee0-125">中介軟體已接管模組和處理常式的角色</span><span class="sxs-lookup"><span data-stu-id="caee0-125">The roles of both modules and handlers have been taken over by middleware</span></span>

* <span data-ttu-id="caee0-126">中介軟體是使用程式碼（而不*是 web.config）* 來設定</span><span class="sxs-lookup"><span data-stu-id="caee0-126">Middleware are configured using code rather than in *Web.config*</span></span>

* <span data-ttu-id="caee0-127">[管線分支](xref:fundamentals/middleware/index#use-run-and-map)可讓您將要求傳送至特定中介軟體，而不只是 URL，也會根據要求標頭、查詢字串等。</span><span class="sxs-lookup"><span data-stu-id="caee0-127">[Pipeline branching](xref:fundamentals/middleware/index#use-run-and-map) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="caee0-128">**中介軟體非常類似模組：**</span><span class="sxs-lookup"><span data-stu-id="caee0-128">**Middleware are very similar to modules:**</span></span>

* <span data-ttu-id="caee0-129">在每個要求的主體中叫用</span><span class="sxs-lookup"><span data-stu-id="caee0-129">Invoked in principle for every request</span></span>

* <span data-ttu-id="caee0-130">無法將[要求傳遞至下一個中介軟體](#http-modules-shortcircuiting-middleware)，藉此縮短要求的路線</span><span class="sxs-lookup"><span data-stu-id="caee0-130">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

* <span data-ttu-id="caee0-131">能夠建立自己的 HTTP 回應</span><span class="sxs-lookup"><span data-stu-id="caee0-131">Able to create their own HTTP response</span></span>

<span data-ttu-id="caee0-132">**中介軟體和模組會以不同的順序進行處理：**</span><span class="sxs-lookup"><span data-stu-id="caee0-132">**Middleware and modules are processed in a different order:**</span></span>

* <span data-ttu-id="caee0-133">中介軟體的順序是根據它們插入要求管線的順序，而模組的順序則主要是根據[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)事件</span><span class="sxs-lookup"><span data-stu-id="caee0-133">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

* <span data-ttu-id="caee0-134">回應的中介軟體順序與要求相反，而模組的順序則與要求和回應相同</span><span class="sxs-lookup"><span data-stu-id="caee0-134">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

* <span data-ttu-id="caee0-135">請參閱[使用 IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="caee0-135">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![中介軟體](http-modules/_static/middleware.png)

<span data-ttu-id="caee0-137">請注意，在上圖中，驗證中介軟體會簡短縮短要求。</span><span class="sxs-lookup"><span data-stu-id="caee0-137">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="caee0-138">將模組程式碼遷移至中介軟體</span><span class="sxs-lookup"><span data-stu-id="caee0-138">Migrating module code to middleware</span></span>

<span data-ttu-id="caee0-139">現有的 HTTP 模組看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="caee0-139">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="caee0-140">如[中介軟體](xref:fundamentals/middleware/index)頁面所示，ASP.NET Core 中介軟體是一個類別，它會公開取得 `HttpContext` 並傳回 `Task`的 `Invoke` 方法。</span><span class="sxs-lookup"><span data-stu-id="caee0-140">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="caee0-141">您的新中介軟體看起來會像這樣：</span><span class="sxs-lookup"><span data-stu-id="caee0-141">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="caee0-142">先前的中介軟體範本取自[撰寫中介軟體](xref:fundamentals/middleware/write)的一節。</span><span class="sxs-lookup"><span data-stu-id="caee0-142">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/write).</span></span>

<span data-ttu-id="caee0-143">*MyMiddlewareExtensions* helper 類別可讓您更輕鬆地在 `Startup` 類別中設定中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-143">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="caee0-144">`UseMyMiddleware` 方法會將中介軟體類別新增至要求管線。</span><span class="sxs-lookup"><span data-stu-id="caee0-144">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="caee0-145">中介軟體所需的服務會插入中介軟體的函式中。</span><span class="sxs-lookup"><span data-stu-id="caee0-145">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="caee0-146">您的模組可能會終止要求，例如，如果使用者未獲授權：</span><span class="sxs-lookup"><span data-stu-id="caee0-146">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="caee0-147">中介軟體會藉由不在管線中的下一個中介軟體上呼叫 `Invoke` 來處理這種情況。</span><span class="sxs-lookup"><span data-stu-id="caee0-147">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="caee0-148">請記住，這並不會完全終止要求，因為當回應透過管線回傳時，仍然會叫用先前的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-148">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="caee0-149">當您將模組的功能遷移至新的中介軟體時，可能會發現您的程式碼未編譯，因為 ASP.NET Core 中的 `HttpContext` 類別已大幅變更。</span><span class="sxs-lookup"><span data-stu-id="caee0-149">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="caee0-150">[稍後](#migrating-to-the-new-httpcontext)，您將瞭解如何遷移至新的 ASP.NET Core HttpCoNtext。</span><span class="sxs-lookup"><span data-stu-id="caee0-150">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="caee0-151">將模組插入遷移至要求管線</span><span class="sxs-lookup"><span data-stu-id="caee0-151">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="caee0-152">HTTP 模組通常*會使用 web.config*新增至要求管線：</span><span class="sxs-lookup"><span data-stu-id="caee0-152">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="caee0-153">將[新的中介軟體](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder)新增至 `Startup` 類別中的要求管線，以轉換此項：</span><span class="sxs-lookup"><span data-stu-id="caee0-153">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#create-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="caee0-154">管線中您插入新中介軟體的確切位置，取決於它處理為模組的事件（`BeginRequest`、`EndRequest`等）及其*在 web.config 中*的模組清單中的順序。</span><span class="sxs-lookup"><span data-stu-id="caee0-154">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="caee0-155">如先前所述，ASP.NET Core 中沒有應用程式生命週期，中介軟體處理回應的順序與模組使用的順序不同。</span><span class="sxs-lookup"><span data-stu-id="caee0-155">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="caee0-156">這可能會讓您的訂購決策更具挑戰性。</span><span class="sxs-lookup"><span data-stu-id="caee0-156">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="caee0-157">如果順序變成問題，您可以將模組分割成可獨立排序的多個中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="caee0-157">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="caee0-158">將處理常式程式碼遷移至中介軟體</span><span class="sxs-lookup"><span data-stu-id="caee0-158">Migrating handler code to middleware</span></span>

<span data-ttu-id="caee0-159">HTTP 處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="caee0-159">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="caee0-160">在您的 ASP.NET Core 專案中，您會將其轉譯為中介軟體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="caee0-160">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="caee0-161">這個中介軟體非常類似于模組的對應中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-161">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="caee0-162">唯一的真正差異在於，這裡沒有 `_next.Invoke(context)`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="caee0-162">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="caee0-163">這是合理的，因為處理常式是在要求管線的結尾，因此不會叫用下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-163">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="caee0-164">將處理常式插入遷移至要求管線</span><span class="sxs-lookup"><span data-stu-id="caee0-164">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="caee0-165">設定 HTTP 處理常式*是在 web.config*中完成，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="caee0-165">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="caee0-166">您可以將新的處理常式中介軟體新增至 `Startup` 類別中的要求管線，以進行轉換，類似于從模組轉換的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-166">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="caee0-167">該方法的問題在於，它會將所有要求傳送至新的處理常式中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-167">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="caee0-168">不過，您只想要要求具有指定的延伸模組，才能到達您的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-168">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="caee0-169">這會提供您與 HTTP 處理常式相同的功能。</span><span class="sxs-lookup"><span data-stu-id="caee0-169">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="caee0-170">其中一個解決方案是使用 `MapWhen` 擴充方法，將管線分支給具有給定副檔名的要求。</span><span class="sxs-lookup"><span data-stu-id="caee0-170">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="caee0-171">您可以在新增其他中介軟體的相同 `Configure` 方法中執行此動作：</span><span class="sxs-lookup"><span data-stu-id="caee0-171">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="caee0-172">`MapWhen` 會採用下列參數：</span><span class="sxs-lookup"><span data-stu-id="caee0-172">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="caee0-173">Lambda，會採用 `HttpContext` 並傳回 `true` （如果要求應該在分支下）。</span><span class="sxs-lookup"><span data-stu-id="caee0-173">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="caee0-174">這表示您不僅可以根據要求的延伸模組，也會根據要求標頭、查詢字串參數等來分支要求。</span><span class="sxs-lookup"><span data-stu-id="caee0-174">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="caee0-175">會採用 `IApplicationBuilder` 並新增分支之所有中介軟體的 lambda。</span><span class="sxs-lookup"><span data-stu-id="caee0-175">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="caee0-176">這表示您可以將其他中介軟體新增至處理常式中介軟體前方的分支。</span><span class="sxs-lookup"><span data-stu-id="caee0-176">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="caee0-177">在所有要求上叫用分支之前，已將中介軟體新增至管線;分支不會對它們產生任何影響。</span><span class="sxs-lookup"><span data-stu-id="caee0-177">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="caee0-178">使用選項模式載入中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="caee0-178">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="caee0-179">有些模組和處理常式都有*儲存在 web.config*中的設定選項。不過，在 ASP.NET Core 會使用新的設定模型來取代*web.config*。</span><span class="sxs-lookup"><span data-stu-id="caee0-179">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="caee0-180">新的設定[系統](xref:fundamentals/configuration/index)會提供您下列選項來解決此問題：</span><span class="sxs-lookup"><span data-stu-id="caee0-180">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="caee0-181">將選項直接插入中介軟體，如下一[節](#loading-middleware-options-through-direct-injection)所示。</span><span class="sxs-lookup"><span data-stu-id="caee0-181">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="caee0-182">使用[選項模式](xref:fundamentals/configuration/options)：</span><span class="sxs-lookup"><span data-stu-id="caee0-182">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="caee0-183">建立類別來存放中介軟體選項，例如：</span><span class="sxs-lookup"><span data-stu-id="caee0-183">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="caee0-184">儲存選項值</span><span class="sxs-lookup"><span data-stu-id="caee0-184">Store the option values</span></span>

   <span data-ttu-id="caee0-185">設定系統可讓您將選項值儲存在您想要的任何位置。</span><span class="sxs-lookup"><span data-stu-id="caee0-185">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="caee0-186">不過，大部分的網站都使用*appsettings*，所以我們會採用這種方法：</span><span class="sxs-lookup"><span data-stu-id="caee0-186">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="caee0-187">這裡的*MyMiddlewareOptionsSection*是區段名稱。</span><span class="sxs-lookup"><span data-stu-id="caee0-187">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="caee0-188">它不一定要與選項類別的名稱相同。</span><span class="sxs-lookup"><span data-stu-id="caee0-188">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="caee0-189">將選項值與選項類別產生關聯</span><span class="sxs-lookup"><span data-stu-id="caee0-189">Associate the option values with the options class</span></span>

    <span data-ttu-id="caee0-190">選項模式會使用 ASP.NET Core 的相依性插入架構，將選項類型（例如 `MyMiddlewareOptions`）與具有實際選項的 `MyMiddlewareOptions` 物件產生關聯。</span><span class="sxs-lookup"><span data-stu-id="caee0-190">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="caee0-191">更新您的 `Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="caee0-191">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="caee0-192">如果您使用的是*appsettings*，請將它新增至 `Startup` 的函式中的 configuration builder：</span><span class="sxs-lookup"><span data-stu-id="caee0-192">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="caee0-193">設定選項服務：</span><span class="sxs-lookup"><span data-stu-id="caee0-193">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="caee0-194">將您的選項與選項類別產生關聯：</span><span class="sxs-lookup"><span data-stu-id="caee0-194">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="caee0-195">將選項插入中介軟體的函式。</span><span class="sxs-lookup"><span data-stu-id="caee0-195">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="caee0-196">這類似于將選項插入控制器中。</span><span class="sxs-lookup"><span data-stu-id="caee0-196">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="caee0-197">將中介軟體新增至 `IApplicationBuilder` 的[UseMiddleware](#http-modules-usemiddleware)擴充方法會負責相依性插入。</span><span class="sxs-lookup"><span data-stu-id="caee0-197">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="caee0-198">這不限於 `IOptions` 物件。</span><span class="sxs-lookup"><span data-stu-id="caee0-198">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="caee0-199">中介軟體所需的任何其他物件都可以用這種方式插入。</span><span class="sxs-lookup"><span data-stu-id="caee0-199">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="caee0-200">透過直接插入來載入中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="caee0-200">Loading middleware options through direct injection</span></span>

<span data-ttu-id="caee0-201">[選項] 模式的優點是它會在選項值及其取用者之間建立鬆散結合。</span><span class="sxs-lookup"><span data-stu-id="caee0-201">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="caee0-202">當您將 options 類別與實際的選項值建立關聯之後，任何其他類別都可以透過相依性插入架構取得選項的存取權。</span><span class="sxs-lookup"><span data-stu-id="caee0-202">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="caee0-203">不需要傳遞選項值。</span><span class="sxs-lookup"><span data-stu-id="caee0-203">There's no need to pass around options values.</span></span>

<span data-ttu-id="caee0-204">不過，如果您想要使用相同的中介軟體兩次，但有不同的選項，則會中斷。</span><span class="sxs-lookup"><span data-stu-id="caee0-204">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="caee0-205">例如，在允許不同角色的不同分支中使用的授權中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-205">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="caee0-206">您無法將兩個不同的選項物件與一個選項類別產生關聯。</span><span class="sxs-lookup"><span data-stu-id="caee0-206">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="caee0-207">解決方法是使用 `Startup` 類別中的實際選項值來取得 options 物件，並將它們直接傳遞至中介軟體的每個實例。</span><span class="sxs-lookup"><span data-stu-id="caee0-207">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="caee0-208">將第二個金鑰新增至*appsettings*</span><span class="sxs-lookup"><span data-stu-id="caee0-208">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="caee0-209">若要將第二組選項新增至*appsettings* ，請使用新的金鑰來唯一識別它：</span><span class="sxs-lookup"><span data-stu-id="caee0-209">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="caee0-210">取出選項值，並將它們傳遞至中介軟體。</span><span class="sxs-lookup"><span data-stu-id="caee0-210">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="caee0-211">`Use...` 擴充方法（會將中介軟體新增至管線）是傳遞選項值的邏輯位置：</span><span class="sxs-lookup"><span data-stu-id="caee0-211">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="caee0-212">啟用中介軟體以接受選項參數。</span><span class="sxs-lookup"><span data-stu-id="caee0-212">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="caee0-213">提供 `Use...` 擴充方法的多載（會採用 options 參數，並將它傳遞給 `UseMiddleware`）。</span><span class="sxs-lookup"><span data-stu-id="caee0-213">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="caee0-214">當以參數呼叫 `UseMiddleware` 時，它會在具現化中介軟體物件時，將參數傳遞至中介軟體的函式。</span><span class="sxs-lookup"><span data-stu-id="caee0-214">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="caee0-215">請注意，這會在 `OptionsWrapper` 物件中包裝 options 物件。</span><span class="sxs-lookup"><span data-stu-id="caee0-215">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="caee0-216">這會依照中介軟體的所需來執行 `IOptions`。</span><span class="sxs-lookup"><span data-stu-id="caee0-216">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="caee0-217">遷移至新的 HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="caee0-217">Migrating to the new HttpContext</span></span>

<span data-ttu-id="caee0-218">您稍早看到，中介軟體中的 `Invoke` 方法會接受 `HttpContext`類型的參數：</span><span class="sxs-lookup"><span data-stu-id="caee0-218">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="caee0-219">ASP.NET Core 中的 `HttpContext` 已大幅變更。</span><span class="sxs-lookup"><span data-stu-id="caee0-219">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="caee0-220">本節說明如何將[system.web](/dotnet/api/system.web.httpcontext)的最常使用屬性轉譯為新的 `Microsoft.AspNetCore.Http.HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="caee0-220">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="caee0-221">HttpCoNtext</span><span class="sxs-lookup"><span data-stu-id="caee0-221">HttpContext</span></span>

<span data-ttu-id="caee0-222">**HttpCoNtext 的專案**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-222">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="caee0-223">**唯一要求識別碼（不含 System.web. HttpCoNtext 對應）**</span><span class="sxs-lookup"><span data-stu-id="caee0-223">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="caee0-224">為每個要求提供唯一的識別碼。</span><span class="sxs-lookup"><span data-stu-id="caee0-224">Gives you a unique id for each request.</span></span> <span data-ttu-id="caee0-225">在記錄中包含非常有用的。</span><span class="sxs-lookup"><span data-stu-id="caee0-225">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="caee0-226">HttpCoNtext 要求</span><span class="sxs-lookup"><span data-stu-id="caee0-226">HttpContext.Request</span></span>

<span data-ttu-id="caee0-227">**HttpMethod**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-227">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="caee0-228">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-228">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="caee0-229">**RawUrl**會轉譯為：（& i）</span><span class="sxs-lookup"><span data-stu-id="caee0-229">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="caee0-230">**IsSecureConnection**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-230">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="caee0-231">**UserHostAddress**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-231">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="caee0-232">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-232">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="caee0-233">**RequestCoNtext. RouteData**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-233">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="caee0-234">**HttpcoNtext.current**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-234">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="caee0-235">**UserAgent**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-235">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="caee0-236">**UrlReferrer**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-236">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="caee0-237">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-237">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="caee0-238">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-238">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="caee0-239">只有在 content 子類型為*x-www-表單 urlencoded*或*表單資料*時，才讀取表單值。</span><span class="sxs-lookup"><span data-stu-id="caee0-239">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="caee0-240">**InputStream**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-240">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="caee0-241">請只在管線結尾的處理常式類型中介軟體中使用此程式碼。</span><span class="sxs-lookup"><span data-stu-id="caee0-241">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="caee0-242">您可以讀取每個要求只顯示一次的原始本文。</span><span class="sxs-lookup"><span data-stu-id="caee0-242">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="caee0-243">在第一次讀取之後嘗試讀取本文的中介軟體將會讀取空白主體。</span><span class="sxs-lookup"><span data-stu-id="caee0-243">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="caee0-244">這並不適用于讀取如先前所示的表單，因為這是從緩衝區完成的。</span><span class="sxs-lookup"><span data-stu-id="caee0-244">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="caee0-245">HttpCoNtext 回應</span><span class="sxs-lookup"><span data-stu-id="caee0-245">HttpContext.Response</span></span>

<span data-ttu-id="caee0-246">**StatusDescription**會轉譯為下列內容：</span><span class="sxs-lookup"><span data-stu-id="caee0-246">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="caee0-247">**ContentEncoding**和**HTTPcoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-247">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="caee0-248">**HttpCoNtext**本身的 ContentType 也會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-248">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="caee0-249">**HttpCoNtext**會轉譯為：</span><span class="sxs-lookup"><span data-stu-id="caee0-249">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="caee0-250">**HttpCoNtext. Response TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="caee0-250">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="caee0-251">[這裡](../fundamentals/request-features.md#middleware-and-request-features)會討論如何提供檔案。</span><span class="sxs-lookup"><span data-stu-id="caee0-251">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="caee0-252">**HttpCoNtext. 標頭**</span><span class="sxs-lookup"><span data-stu-id="caee0-252">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="caee0-253">傳送回應標頭很複雜，因為如果您在任何專案都已寫入回應主體之後設定它們，則不會將它們送出。</span><span class="sxs-lookup"><span data-stu-id="caee0-253">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="caee0-254">解決方案是設定回呼方法，在寫入回應開始之前，會先呼叫它。</span><span class="sxs-lookup"><span data-stu-id="caee0-254">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="caee0-255">這最好是在中介軟體的 `Invoke` 方法開始時執行。</span><span class="sxs-lookup"><span data-stu-id="caee0-255">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="caee0-256">這是設定回應標頭的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="caee0-256">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="caee0-257">下列程式碼會設定名為 `SetHeaders`的回呼方法：</span><span class="sxs-lookup"><span data-stu-id="caee0-257">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="caee0-258">`SetHeaders` 回呼方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="caee0-258">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="caee0-259">**HttpCoNtext. 回應 Cookie**</span><span class="sxs-lookup"><span data-stu-id="caee0-259">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="caee0-260">Cookie 會以*設定-Cookie*回應標頭傳送至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="caee0-260">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="caee0-261">因此，傳送 cookie 需要與用來傳送回應標頭相同的回呼：</span><span class="sxs-lookup"><span data-stu-id="caee0-261">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="caee0-262">`SetCookies` 回呼方法如下所示：</span><span class="sxs-lookup"><span data-stu-id="caee0-262">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="caee0-263">其他資源</span><span class="sxs-lookup"><span data-stu-id="caee0-263">Additional resources</span></span>

* [<span data-ttu-id="caee0-264">HTTP 處理常式和 HTTP 模組總覽</span><span class="sxs-lookup"><span data-stu-id="caee0-264">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="caee0-265">組態</span><span class="sxs-lookup"><span data-stu-id="caee0-265">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="caee0-266">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="caee0-266">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="caee0-267">中介軟體</span><span class="sxs-lookup"><span data-stu-id="caee0-267">Middleware</span></span>](xref:fundamentals/middleware/index)
