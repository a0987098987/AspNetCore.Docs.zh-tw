---
title: 移轉 ASP.NET Core 中介軟體的 HTTP 處理常式和模組
author: rick-anderson
description: ''
manager: wpickett
ms.author: tdykstra
ms.date: 12/07/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/http-modules
ms.openlocfilehash: cbdef871ffc3269e3118d23ed20306a71b9df030
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="migrate-http-handlers-and-modules-to-aspnet-core-middleware"></a><span data-ttu-id="63ead-102">移轉 ASP.NET Core 中介軟體的 HTTP 處理常式和模組</span><span class="sxs-lookup"><span data-stu-id="63ead-102">Migrate HTTP handlers and modules to ASP.NET Core middleware</span></span>

<span data-ttu-id="63ead-103">由[Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span><span class="sxs-lookup"><span data-stu-id="63ead-103">By [Matt Perdeck](https://www.linkedin.com/in/mattperdeck)</span></span>

<span data-ttu-id="63ead-104">本文將說明如何移轉現有的 ASP.NET [HTTP 模組和處理常式 system.webserver](/iis/configuration/system.webserver/)到 ASP.NET Core[中介軟體](xref:fundamentals/middleware/index)。</span><span class="sxs-lookup"><span data-stu-id="63ead-104">This article shows how to migrate existing ASP.NET [HTTP modules and handlers from system.webserver](/iis/configuration/system.webserver/) to ASP.NET Core [middleware](xref:fundamentals/middleware/index).</span></span>

## <a name="modules-and-handlers-revisited"></a><span data-ttu-id="63ead-105">模組和處理常式進行重新瀏覽</span><span class="sxs-lookup"><span data-stu-id="63ead-105">Modules and handlers revisited</span></span>

<span data-ttu-id="63ead-106">之前 ASP.NET Core 中介軟體，讓我們先複習一下 HTTP 模組和處理常式的運作方式：</span><span class="sxs-lookup"><span data-stu-id="63ead-106">Before proceeding to ASP.NET Core middleware, let's first recap how HTTP modules and handlers work:</span></span>

![模組處理常式](http-modules/_static/moduleshandlers.png)

<span data-ttu-id="63ead-108">**處理常式包括：**</span><span class="sxs-lookup"><span data-stu-id="63ead-108">**Handlers are:**</span></span>

   * <span data-ttu-id="63ead-109">類別可實作[IHttpHandler](/dotnet/api/system.web.ihttphandler)</span><span class="sxs-lookup"><span data-stu-id="63ead-109">Classes that implement [IHttpHandler](/dotnet/api/system.web.ihttphandler)</span></span>

   * <span data-ttu-id="63ead-110">用來處理要求，以指定的檔案名稱或副檔名，例如 *.report*</span><span class="sxs-lookup"><span data-stu-id="63ead-110">Used to handle requests with a given file name or extension, such as *.report*</span></span>

   * <span data-ttu-id="63ead-111">[設定](/iis/configuration/system.webserver/handlers/)中*Web.config*</span><span class="sxs-lookup"><span data-stu-id="63ead-111">[Configured](/iis/configuration/system.webserver/handlers/) in *Web.config*</span></span>

<span data-ttu-id="63ead-112">**模組為：**</span><span class="sxs-lookup"><span data-stu-id="63ead-112">**Modules are:**</span></span>

   * <span data-ttu-id="63ead-113">類別可實作[IHttpModule](/dotnet/api/system.web.ihttpmodule)</span><span class="sxs-lookup"><span data-stu-id="63ead-113">Classes that implement [IHttpModule](/dotnet/api/system.web.ihttpmodule)</span></span>

   * <span data-ttu-id="63ead-114">叫用每個要求</span><span class="sxs-lookup"><span data-stu-id="63ead-114">Invoked for every request</span></span>

   * <span data-ttu-id="63ead-115">能夠最少運算 （停止進一步處理的要求）</span><span class="sxs-lookup"><span data-stu-id="63ead-115">Able to short-circuit (stop further processing of a request)</span></span>

   * <span data-ttu-id="63ead-116">無法加入至 HTTP 回應，或建立自己</span><span class="sxs-lookup"><span data-stu-id="63ead-116">Able to add to the HTTP response, or create their own</span></span>

   * <span data-ttu-id="63ead-117">[設定](/iis/configuration/system.webserver/modules/)中*Web.config*</span><span class="sxs-lookup"><span data-stu-id="63ead-117">[Configured](/iis/configuration/system.webserver/modules/) in *Web.config*</span></span>

<span data-ttu-id="63ead-118">**模組處理內送要求的順序取決於：**</span><span class="sxs-lookup"><span data-stu-id="63ead-118">**The order in which modules process incoming requests is determined by:**</span></span>

   1. <span data-ttu-id="63ead-119">[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)，這是由 ASP.NET 所引發的系列事件： [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest)， [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest)等等。每個模組都可以建立一或多個事件的處理常式。</span><span class="sxs-lookup"><span data-stu-id="63ead-119">The [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx), which is a series events fired by ASP.NET: [BeginRequest](/dotnet/api/system.web.httpapplication.beginrequest), [AuthenticateRequest](/dotnet/api/system.web.httpapplication.authenticaterequest), etc. Each module can create a handler for one or more events.</span></span>

   2. <span data-ttu-id="63ead-120">相同事件中所設定的順序*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="63ead-120">For the same event, the order in which they're configured in *Web.config*.</span></span>

<span data-ttu-id="63ead-121">除了模組，您可以加入處理常式的生命週期事件，以您*Global.asax.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="63ead-121">In addition to modules, you can add handlers for the life cycle events to your *Global.asax.cs* file.</span></span> <span data-ttu-id="63ead-122">在 設定模組中的處理常式之後，執行這些處理常式。</span><span class="sxs-lookup"><span data-stu-id="63ead-122">These handlers run after the handlers in the configured modules.</span></span>

## <a name="from-handlers-and-modules-to-middleware"></a><span data-ttu-id="63ead-123">從處理常式和模組中介軟體</span><span class="sxs-lookup"><span data-stu-id="63ead-123">From handlers and modules to middleware</span></span>

<span data-ttu-id="63ead-124">**中介軟體會簡單比 HTTP 模組和處理常式：**</span><span class="sxs-lookup"><span data-stu-id="63ead-124">**Middleware are simpler than HTTP modules and handlers:**</span></span>

   * <span data-ttu-id="63ead-125">模組、 處理常式、 *Global.asax.cs*， *Web.config* （除了 IIS 組態） 和應用程式生命週期已消失</span><span class="sxs-lookup"><span data-stu-id="63ead-125">Modules, handlers, *Global.asax.cs*, *Web.config* (except for IIS configuration) and the application life cycle are gone</span></span>

   * <span data-ttu-id="63ead-126">模組和處理常式的角色有接手中介軟體</span><span class="sxs-lookup"><span data-stu-id="63ead-126">The roles of both modules and handlers have been taken over by middleware</span></span>

   * <span data-ttu-id="63ead-127">使用程式碼設定中介軟體，而非在*Web.config*</span><span class="sxs-lookup"><span data-stu-id="63ead-127">Middleware are configured using code rather than in *Web.config*</span></span>

   * <span data-ttu-id="63ead-128">[管線分支](xref:fundamentals/middleware/index#middleware-run-map-use)可讓您將要求傳送至特定中介軟體，根據不僅還在要求標頭、 查詢字串和其他內容的 URL。</span><span class="sxs-lookup"><span data-stu-id="63ead-128">[Pipeline branching](xref:fundamentals/middleware/index#middleware-run-map-use) lets you send requests to specific middleware, based on not only the URL but also on request headers, query strings, etc.</span></span>

<span data-ttu-id="63ead-129">**中介軟體是非常類似於模組：**</span><span class="sxs-lookup"><span data-stu-id="63ead-129">**Middleware are very similar to modules:**</span></span>

   * <span data-ttu-id="63ead-130">在每個要求主體中叫用</span><span class="sxs-lookup"><span data-stu-id="63ead-130">Invoked in principle for every request</span></span>

   * <span data-ttu-id="63ead-131">能夠藉由將要求，最少運算[不將要求傳遞至下一個中介軟體](#http-modules-shortcircuiting-middleware)</span><span class="sxs-lookup"><span data-stu-id="63ead-131">Able to short-circuit a request, by [not passing the request to the next middleware](#http-modules-shortcircuiting-middleware)</span></span>

   * <span data-ttu-id="63ead-132">可以建立自己的 HTTP 回應</span><span class="sxs-lookup"><span data-stu-id="63ead-132">Able to create their own HTTP response</span></span>

<span data-ttu-id="63ead-133">**中介軟體和模組會處理不同的順序：**</span><span class="sxs-lookup"><span data-stu-id="63ead-133">**Middleware and modules are processed in a different order:**</span></span>

   * <span data-ttu-id="63ead-134">中介軟體的順序根據的順序所在它們正在插入要求管線時之模組的順序主要是根據[應用程式生命週期](https://msdn.microsoft.com/library/ms227673.aspx)事件</span><span class="sxs-lookup"><span data-stu-id="63ead-134">Order of middleware is based on the order in which they're inserted into the request pipeline, while order of modules is mainly based on [application life cycle](https://msdn.microsoft.com/library/ms227673.aspx) events</span></span>

   * <span data-ttu-id="63ead-135">順序中介軟體的回應時，針對要求，從該反向模組的順序是相同的要求和回應</span><span class="sxs-lookup"><span data-stu-id="63ead-135">Order of middleware for responses is the reverse from that for requests, while order of modules is the same for requests and responses</span></span>

   * <span data-ttu-id="63ead-136">請參閱[IApplicationBuilder 建立中介軟體管線](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span><span class="sxs-lookup"><span data-stu-id="63ead-136">See [Create a middleware pipeline with IApplicationBuilder](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)</span></span>

![中介軟體](http-modules/_static/middleware.png)

<span data-ttu-id="63ead-138">請注意如何在上圖中，驗證中介軟體 short-circuited 要求。</span><span class="sxs-lookup"><span data-stu-id="63ead-138">Note how in the image above, the authentication middleware short-circuited the request.</span></span>

## <a name="migrating-module-code-to-middleware"></a><span data-ttu-id="63ead-139">中介軟體的移轉模組程式碼</span><span class="sxs-lookup"><span data-stu-id="63ead-139">Migrating module code to middleware</span></span>

<span data-ttu-id="63ead-140">現有的 HTTP 模組會看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="63ead-140">An existing HTTP module will look similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyModule.cs?highlight=6,8,24,31)]

<span data-ttu-id="63ead-141">中所示[中介軟體](xref:fundamentals/middleware/index) 頁面上，ASP.NET Core 中介軟體是公開的類別`Invoke`方法擷取`HttpContext`並傳回`Task`。</span><span class="sxs-lookup"><span data-stu-id="63ead-141">As shown in the [Middleware](xref:fundamentals/middleware/index) page, an ASP.NET Core middleware is a class that exposes an `Invoke` method taking an `HttpContext` and returning a `Task`.</span></span> <span data-ttu-id="63ead-142">您新的中介軟體看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="63ead-142">Your new middleware will look like this:</span></span>

<a name="http-modules-usemiddleware"></a>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddleware.cs?highlight=9,13,20,24,28,30,32)]

<span data-ttu-id="63ead-143">上述的中介軟體範本上建立從區段[寫入中介軟體](xref:fundamentals/middleware/index#middleware-writing-middleware)。</span><span class="sxs-lookup"><span data-stu-id="63ead-143">The preceding middleware template was taken from the section on [writing middleware](xref:fundamentals/middleware/index#middleware-writing-middleware).</span></span>

<span data-ttu-id="63ead-144">*MyMiddlewareExtensions* helper 類別可讓您更輕鬆地設定中的介軟體在您`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="63ead-144">The *MyMiddlewareExtensions* helper class makes it easier to configure your middleware in your `Startup` class.</span></span> <span data-ttu-id="63ead-145">`UseMyMiddleware`方法將中介軟體類別加入至要求管線。</span><span class="sxs-lookup"><span data-stu-id="63ead-145">The `UseMyMiddleware` method adds your middleware class to the request pipeline.</span></span> <span data-ttu-id="63ead-146">中介軟體所需的服務取得插入的中介軟體的建構函式中。</span><span class="sxs-lookup"><span data-stu-id="63ead-146">Services required by the middleware get injected in the middleware's constructor.</span></span>

<a name="http-modules-shortcircuiting-middleware"></a>

<span data-ttu-id="63ead-147">您可以在模組可能會終止要求，例如，如果使用者未獲授權：</span><span class="sxs-lookup"><span data-stu-id="63ead-147">Your module might terminate a request, for example if the user isn't authorized:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Modules/MyTerminatingModule.cs?highlight=9,10,11,12,13&name=snippet_Terminate)]

<span data-ttu-id="63ead-148">中介軟體所不會呼叫處理`Invoke`在管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-148">A middleware handles this by not calling `Invoke` on the next middleware in the pipeline.</span></span> <span data-ttu-id="63ead-149">請記住這並不完全終止要求，因為前一個 middlewares 將仍會叫用時回應回通過管線。</span><span class="sxs-lookup"><span data-stu-id="63ead-149">Keep in mind that this doesn't fully terminate the request, because previous middlewares will still be invoked when the response makes its way back through the pipeline.</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyTerminatingMiddleware.cs?highlight=7,8&name=snippet_Terminate)]

<span data-ttu-id="63ead-150">當您將模組的功能移轉到新的中介軟體時，您可能會發現不會編譯程式碼，因為`HttpContext`類別已經大幅變更 ASP.NET Core 中。</span><span class="sxs-lookup"><span data-stu-id="63ead-150">When you migrate your module's functionality to your new middleware, you may find that your code doesn't compile because the `HttpContext` class has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="63ead-151">[稍後](#migrating-to-the-new-httpcontext)，您會看到如何將移轉至新的 ASP.NET Core HttpContext。</span><span class="sxs-lookup"><span data-stu-id="63ead-151">[Later on](#migrating-to-the-new-httpcontext), you'll see how to migrate to the new ASP.NET Core HttpContext.</span></span>

## <a name="migrating-module-insertion-into-the-request-pipeline"></a><span data-ttu-id="63ead-152">移轉模組插入要求管線</span><span class="sxs-lookup"><span data-stu-id="63ead-152">Migrating module insertion into the request pipeline</span></span>

<span data-ttu-id="63ead-153">HTTP 模組通常會加入至要求管線使用*Web.config*:</span><span class="sxs-lookup"><span data-stu-id="63ead-153">HTTP modules are typically added to the request pipeline using *Web.config*:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32-33,36,43,50,101)]

<span data-ttu-id="63ead-154">轉換所[新增您新的中介軟體](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder)要求管線中您`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="63ead-154">Convert this by [adding your new middleware](xref:fundamentals/middleware/index#creating-a-middleware-pipeline-with-iapplicationbuilder) to the request pipeline in your `Startup` class:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=16)]

<span data-ttu-id="63ead-155">您可以在該處插入新的中介軟體管線中的確切取決於它處理為模組的事件 (`BeginRequest`，`EndRequest`等等) 和其清單中的模組中的順序*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="63ead-155">The exact spot in the pipeline where you insert your new middleware depends on the event that it handled as a module (`BeginRequest`, `EndRequest`, etc.) and its order in your list of modules in *Web.config*.</span></span>

<span data-ttu-id="63ead-156">如先前所述，沒有應用程式生命週期中有 ASP.NET Core 並且由中介軟體處理回應的順序不同模組所使用的順序。</span><span class="sxs-lookup"><span data-stu-id="63ead-156">As previously stated, there's no application life cycle in ASP.NET Core and the order in which responses are processed by middleware differs from the order used by modules.</span></span> <span data-ttu-id="63ead-157">這可以決定排序更具挑戰性。</span><span class="sxs-lookup"><span data-stu-id="63ead-157">This could make your ordering decision more challenging.</span></span>

<span data-ttu-id="63ead-158">如果順序會變成問題，您可以在模組無法分成多個可以獨立地排序的中介軟體元件。</span><span class="sxs-lookup"><span data-stu-id="63ead-158">If ordering becomes a problem, you could split your module into multiple middleware components that can be ordered independently.</span></span>

## <a name="migrating-handler-code-to-middleware"></a><span data-ttu-id="63ead-159">移轉的處理常式程式碼中介軟體</span><span class="sxs-lookup"><span data-stu-id="63ead-159">Migrating handler code to middleware</span></span>

<span data-ttu-id="63ead-160">HTTP 處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="63ead-160">An HTTP handler looks something like this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/HttpHandlers/ReportHandler.cs?highlight=5,7,13,14,15,16)]

<span data-ttu-id="63ead-161">在 ASP.NET Core 專案中，會轉換這個中介軟體如下所示：</span><span class="sxs-lookup"><span data-stu-id="63ead-161">In your ASP.NET Core project, you would translate this to a middleware similar to this:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/ReportHandlerMiddleware.cs?highlight=7,9,13,20,21,22,23,40,42,44)]

<span data-ttu-id="63ead-162">此中介軟體是非常類似於對應到模組的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-162">This middleware is very similar to the middleware corresponding to modules.</span></span> <span data-ttu-id="63ead-163">唯一的差異在於，以下是不需要呼叫`_next.Invoke(context)`。</span><span class="sxs-lookup"><span data-stu-id="63ead-163">The only real difference is that here there's no call to `_next.Invoke(context)`.</span></span> <span data-ttu-id="63ead-164">這很合理，因為處理常式的要求管線結尾應該會有沒有下一個中介軟體要叫用。</span><span class="sxs-lookup"><span data-stu-id="63ead-164">That makes sense, because the handler is at the end of the request pipeline, so there will be no next middleware to invoke.</span></span>

## <a name="migrating-handler-insertion-into-the-request-pipeline"></a><span data-ttu-id="63ead-165">移轉處理常式插入要求管線</span><span class="sxs-lookup"><span data-stu-id="63ead-165">Migrating handler insertion into the request pipeline</span></span>

<span data-ttu-id="63ead-166">設定 HTTP 處理常式是在*Web.config* ，看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="63ead-166">Configuring an HTTP handler is done in *Web.config* and looks something like this:</span></span>

[!code-xml[](../migration/http-modules/sample/Asp.Net4/Asp.Net4/Web.config?highlight=6&range=1-3,32,46-48,50,101)]

<span data-ttu-id="63ead-167">您可以將此轉換將新處理常式中介軟體新增至要求管線中您`Startup`類別細明體，類似於轉換從模組的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-167">You could convert this by adding your new handler middleware to the request pipeline in your `Startup` class, similar to middleware converted from modules.</span></span> <span data-ttu-id="63ead-168">該方法的問題是，它會將所有的要求傳送至新處理常式中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-168">The problem with that approach is that it would send all requests to your new handler middleware.</span></span> <span data-ttu-id="63ead-169">不過，您只想連線到中介軟體具有特定副檔名的要求。</span><span class="sxs-lookup"><span data-stu-id="63ead-169">However, you only want requests with a given extension to reach your middleware.</span></span> <span data-ttu-id="63ead-170">這樣會使您與您的 HTTP 處理常式有相同的功能。</span><span class="sxs-lookup"><span data-stu-id="63ead-170">That would give you the same functionality you had with your HTTP handler.</span></span>

<span data-ttu-id="63ead-171">其中一個解決方案是分支，具有給定延伸模組，要求的管線使用`MapWhen`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="63ead-171">One solution is to branch the pipeline for requests with a given extension, using the `MapWhen` extension method.</span></span> <span data-ttu-id="63ead-172">這麼做在同一個`Configure`方法讓您加入其他中介軟體：</span><span class="sxs-lookup"><span data-stu-id="63ead-172">You do this in the same `Configure` method where you add the other middleware:</span></span>

[!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=27-34)]

<span data-ttu-id="63ead-173">`MapWhen` 採用這些參數：</span><span class="sxs-lookup"><span data-stu-id="63ead-173">`MapWhen` takes these parameters:</span></span>

1. <span data-ttu-id="63ead-174">採用 lambda`HttpContext`並傳回`true`如果要求應該向分支。</span><span class="sxs-lookup"><span data-stu-id="63ead-174">A lambda that takes the `HttpContext` and returns `true` if the request should go down the branch.</span></span> <span data-ttu-id="63ead-175">這表示您可以建立分支不只是根據它們的副檔名，而要求標頭、 查詢字串參數和其他內容的要求。</span><span class="sxs-lookup"><span data-stu-id="63ead-175">This means you can branch requests not just based on their extension, but also on request headers, query string parameters, etc.</span></span>

2. <span data-ttu-id="63ead-176">採用 lambda `IApplicationBuilder` ，並將分支的所有中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-176">A lambda that takes an `IApplicationBuilder` and adds all the middleware for the branch.</span></span> <span data-ttu-id="63ead-177">這表示您可以加入其他中介軟體至分支前面處理常式中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-177">This means you can add additional middleware to the branch in front of your handler middleware.</span></span>

<span data-ttu-id="63ead-178">中介軟體新增至管線之前分支將會叫用的所有要求。分支將會有任何影響，在其上。</span><span class="sxs-lookup"><span data-stu-id="63ead-178">Middleware added to the pipeline before the branch will be invoked on all requests; the branch will have no impact on them.</span></span>

## <a name="loading-middleware-options-using-the-options-pattern"></a><span data-ttu-id="63ead-179">載入使用選項模式的中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="63ead-179">Loading middleware options using the options pattern</span></span>

<span data-ttu-id="63ead-180">部分模組和處理常式有儲存在組態選項*Web.config*。不過，在 ASP.NET Core 新的組態模型會使用取代*Web.config*。</span><span class="sxs-lookup"><span data-stu-id="63ead-180">Some modules and handlers have configuration options that are stored in *Web.config*. However, in ASP.NET Core a new configuration model is used in place of *Web.config*.</span></span>

<span data-ttu-id="63ead-181">新[組態系統](xref:fundamentals/configuration/index)提供您下列選項可用來解決這個問題：</span><span class="sxs-lookup"><span data-stu-id="63ead-181">The new [configuration system](xref:fundamentals/configuration/index) gives you these options to solve this:</span></span>

* <span data-ttu-id="63ead-182">直接插入到中介軟體選項中所示[下一節](#loading-middleware-options-through-direct-injection)。</span><span class="sxs-lookup"><span data-stu-id="63ead-182">Directly inject the options into the middleware, as shown in the [next section](#loading-middleware-options-through-direct-injection).</span></span>

* <span data-ttu-id="63ead-183">使用[選項模式](xref:fundamentals/configuration/options):</span><span class="sxs-lookup"><span data-stu-id="63ead-183">Use the [options pattern](xref:fundamentals/configuration/options):</span></span>

1. <span data-ttu-id="63ead-184">建立類別以包裝您的中介軟體選項，例如：</span><span class="sxs-lookup"><span data-stu-id="63ead-184">Create a class to hold your middleware options, for example:</span></span>

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Options)]

2. <span data-ttu-id="63ead-185">儲存選項值</span><span class="sxs-lookup"><span data-stu-id="63ead-185">Store the option values</span></span>

   <span data-ttu-id="63ead-186">組態系統，可讓您儲存選項任何地方需要的值。</span><span class="sxs-lookup"><span data-stu-id="63ead-186">The configuration system allows you to store option values anywhere you want.</span></span> <span data-ttu-id="63ead-187">不過，最站台使用*appsettings.json*，因此我們將方法：</span><span class="sxs-lookup"><span data-stu-id="63ead-187">However, most sites use *appsettings.json*, so we'll take that approach:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,14-18)]

   <span data-ttu-id="63ead-188">*MyMiddlewareOptionsSection*以下是區段名稱。</span><span class="sxs-lookup"><span data-stu-id="63ead-188">*MyMiddlewareOptionsSection* here is a section name.</span></span> <span data-ttu-id="63ead-189">它不一定要與您的選項類別名稱相同。</span><span class="sxs-lookup"><span data-stu-id="63ead-189">It doesn't have to be the same as the name of your options class.</span></span>

3. <span data-ttu-id="63ead-190">選項值關聯的選項類別</span><span class="sxs-lookup"><span data-stu-id="63ead-190">Associate the option values with the options class</span></span>

    <span data-ttu-id="63ead-191">選項模式使用 ASP.NET Core 相依性插入架構相關聯的選項類型 (例如`MyMiddlewareOptions`) 與`MyMiddlewareOptions`具有實際的選項物件。</span><span class="sxs-lookup"><span data-stu-id="63ead-191">The options pattern uses ASP.NET Core's dependency injection framework to associate the options type (such as `MyMiddlewareOptions`) with a `MyMiddlewareOptions` object that has the actual options.</span></span>

    <span data-ttu-id="63ead-192">更新您`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="63ead-192">Update your `Startup` class:</span></span>

   1. <span data-ttu-id="63ead-193">如果您使用*appsettings.json*，將它加入至組態產生器中`Startup`建構函式：</span><span class="sxs-lookup"><span data-stu-id="63ead-193">If you're using *appsettings.json*, add it to the configuration builder in the `Startup` constructor:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Ctor&highlight=5-6)]

   2. <span data-ttu-id="63ead-194">設定選項服務：</span><span class="sxs-lookup"><span data-stu-id="63ead-194">Configure the options service:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

   3. <span data-ttu-id="63ead-195">將您的選項與選項類別產生關聯：</span><span class="sxs-lookup"><span data-stu-id="63ead-195">Associate your options with your options class:</span></span>

      [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_ConfigureServices&highlight=6-8)]

4. <span data-ttu-id="63ead-196">將插入至中介軟體建構函式的選項。</span><span class="sxs-lookup"><span data-stu-id="63ead-196">Inject the options into your middleware constructor.</span></span> <span data-ttu-id="63ead-197">這是類似於插入到控制器的選項。</span><span class="sxs-lookup"><span data-stu-id="63ead-197">This is similar to injecting options into a controller.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_MiddlewareWithParams&highlight=4,7,10,15-16)]

   <span data-ttu-id="63ead-198">[UseMiddleware](#http-modules-usemiddleware)擴充方法，將它新增至中的介軟體`IApplicationBuilder`，負責相依性插入。</span><span class="sxs-lookup"><span data-stu-id="63ead-198">The [UseMiddleware](#http-modules-usemiddleware) extension method that adds your middleware to the `IApplicationBuilder` takes care of dependency injection.</span></span>

   <span data-ttu-id="63ead-199">這並不限於`IOptions`物件。</span><span class="sxs-lookup"><span data-stu-id="63ead-199">This isn't limited to `IOptions` objects.</span></span> <span data-ttu-id="63ead-200">這種方式可插入您的中介軟體所需要的任何其他物件。</span><span class="sxs-lookup"><span data-stu-id="63ead-200">Any other object that your middleware requires can be injected this way.</span></span>

## <a name="loading-middleware-options-through-direct-injection"></a><span data-ttu-id="63ead-201">載入經由直接資料隱碼的中介軟體選項</span><span class="sxs-lookup"><span data-stu-id="63ead-201">Loading middleware options through direct injection</span></span>

<span data-ttu-id="63ead-202">選項模式的優點，它會建立鬆散選項值和其取用者之間的連結。</span><span class="sxs-lookup"><span data-stu-id="63ead-202">The options pattern has the advantage that it creates loose coupling between options values and their consumers.</span></span> <span data-ttu-id="63ead-203">一旦您已建立的選項類別關聯的實際選項值，任何其他類別可以取得存取透過相依性插入架構選項。</span><span class="sxs-lookup"><span data-stu-id="63ead-203">Once you've associated an options class with the actual options values, any other class can get access to the options through the dependency injection framework.</span></span> <span data-ttu-id="63ead-204">沒有需要通過選項值。</span><span class="sxs-lookup"><span data-stu-id="63ead-204">There's no need to pass around options values.</span></span>

<span data-ttu-id="63ead-205">這會細分不過如果您想要使用不同的選項兩次，使用相同的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-205">This breaks down though if you want to use the same middleware twice, with different options.</span></span> <span data-ttu-id="63ead-206">例如授權中介軟體可讓不同角色的不同分支中使用。</span><span class="sxs-lookup"><span data-stu-id="63ead-206">For example an authorization middleware used in different branches allowing different roles.</span></span> <span data-ttu-id="63ead-207">您無法將兩個不同的選項物件與一個選項類別。</span><span class="sxs-lookup"><span data-stu-id="63ead-207">You can't associate two different options objects with the one options class.</span></span>

<span data-ttu-id="63ead-208">解決方法是取得選項物件中的實際選項值與您`Startup`類別，並傳遞至每個執行個體的中介軟體直接。</span><span class="sxs-lookup"><span data-stu-id="63ead-208">The solution is to get the options objects with the actual options values in your `Startup` class and pass those directly to each instance of your middleware.</span></span>

1. <span data-ttu-id="63ead-209">新增第二個機碼*appsettings.json*</span><span class="sxs-lookup"><span data-stu-id="63ead-209">Add a second key to *appsettings.json*</span></span>

   <span data-ttu-id="63ead-210">若要加入第二組選項以*appsettings.json*檔案中，以唯一識別它使用新的金鑰：</span><span class="sxs-lookup"><span data-stu-id="63ead-210">To add a second set of options to the *appsettings.json* file, use a new key to uniquely identify it:</span></span>

   [!code-json[](http-modules/sample/Asp.Net.Core/appsettings.json?range=1,10-18&highlight=2-5)]

2. <span data-ttu-id="63ead-211">擷取選項值，並將其傳遞至中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-211">Retrieve options values and pass them to middleware.</span></span> <span data-ttu-id="63ead-212">`Use...` （這會加入到管線的中介軟體） 的擴充方法是將選項值在邏輯上：</span><span class="sxs-lookup"><span data-stu-id="63ead-212">The `Use...` extension method (which adds your middleware to the pipeline) is a logical place to pass in the option values:</span></span> 

   [!code-csharp[](http-modules/sample/Asp.Net.Core/Startup.cs?name=snippet_Configure&highlight=20-23)]

3. <span data-ttu-id="63ead-213">可讓中介軟體選項參數。</span><span class="sxs-lookup"><span data-stu-id="63ead-213">Enable middleware to take an options parameter.</span></span> <span data-ttu-id="63ead-214">提供的多載`Use...`擴充方法 (採用 options 參數，並將其傳遞給`UseMiddleware`)。</span><span class="sxs-lookup"><span data-stu-id="63ead-214">Provide an overload of the `Use...` extension method (that takes the options parameter and passes it to `UseMiddleware`).</span></span> <span data-ttu-id="63ead-215">當`UseMiddleware`稱為參數，它將參數傳遞到中介軟體建構函式時，它會具現化中介軟體物件。</span><span class="sxs-lookup"><span data-stu-id="63ead-215">When `UseMiddleware` is called with parameters, it passes the parameters to your middleware constructor when it instantiates the middleware object.</span></span>

   [!code-csharp[](../migration/http-modules/sample/Asp.Net.Core/Middleware/MyMiddlewareWithParams.cs?name=snippet_Extensions&highlight=9-14)]

   <span data-ttu-id="63ead-216">請注意這會在將選項物件的包裝`OptionsWrapper`物件。</span><span class="sxs-lookup"><span data-stu-id="63ead-216">Note how this wraps the options object in an `OptionsWrapper` object.</span></span> <span data-ttu-id="63ead-217">這會實作`IOptions`，如預期般的中介軟體建構函式。</span><span class="sxs-lookup"><span data-stu-id="63ead-217">This implements `IOptions`, as expected by the middleware constructor.</span></span>

## <a name="migrating-to-the-new-httpcontext"></a><span data-ttu-id="63ead-218">移轉至新的 HttpContext</span><span class="sxs-lookup"><span data-stu-id="63ead-218">Migrating to the new HttpContext</span></span>

<span data-ttu-id="63ead-219">您稍早所見，`Invoke`中介軟體中的方法參數的型別`HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="63ead-219">You saw earlier that the `Invoke` method in your middleware takes a parameter of type `HttpContext`:</span></span>

```csharp
public async Task Invoke(HttpContext context)
```

<span data-ttu-id="63ead-220">`HttpContext` 已經大幅變更 ASP.NET Core 中。</span><span class="sxs-lookup"><span data-stu-id="63ead-220">`HttpContext` has significantly changed in ASP.NET Core.</span></span> <span data-ttu-id="63ead-221">本節說明如何將最常用的屬性轉譯[System.Web.HttpContext](/dotnet/api/system.web.httpcontext)新`Microsoft.AspNetCore.Http.HttpContext`。</span><span class="sxs-lookup"><span data-stu-id="63ead-221">This section shows how to translate the most commonly used properties of [System.Web.HttpContext](/dotnet/api/system.web.httpcontext) to the new `Microsoft.AspNetCore.Http.HttpContext`.</span></span>

### <a name="httpcontext"></a><span data-ttu-id="63ead-222">HttpContext</span><span class="sxs-lookup"><span data-stu-id="63ead-222">HttpContext</span></span>

<span data-ttu-id="63ead-223">**HttpContext.Items**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-223">**HttpContext.Items** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Items)]

<span data-ttu-id="63ead-224">**唯一的要求識別碼 （沒有 System.Web.HttpContext 對應項目）**</span><span class="sxs-lookup"><span data-stu-id="63ead-224">**Unique request ID (no System.Web.HttpContext counterpart)**</span></span>

<span data-ttu-id="63ead-225">針對每個要求，讓您的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="63ead-225">Gives you a unique id for each request.</span></span> <span data-ttu-id="63ead-226">要包含在記錄中非常有用。</span><span class="sxs-lookup"><span data-stu-id="63ead-226">Very useful to include in your logs.</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Trace)]

### <a name="httpcontextrequest"></a><span data-ttu-id="63ead-227">HttpContext.Request</span><span class="sxs-lookup"><span data-stu-id="63ead-227">HttpContext.Request</span></span>

<span data-ttu-id="63ead-228">**HttpContext.Request.HttpMethod**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-228">**HttpContext.Request.HttpMethod** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Method)]

<span data-ttu-id="63ead-229">**HttpContext.Request.QueryString**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-229">**HttpContext.Request.QueryString** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Query)]

<span data-ttu-id="63ead-230">**HttpContext.Request.Url**和**HttpContext.Request.RawUrl**轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-230">**HttpContext.Request.Url** and **HttpContext.Request.RawUrl** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Url)]

<span data-ttu-id="63ead-231">**HttpContext.Request.IsSecureConnection**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-231">**HttpContext.Request.IsSecureConnection** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Secure)]

<span data-ttu-id="63ead-232">**HttpContext.Request.UserHostAddress**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-232">**HttpContext.Request.UserHostAddress** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Host)]

<span data-ttu-id="63ead-233">**HttpContext.Request.Cookies**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-233">**HttpContext.Request.Cookies** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Cookies)]

<span data-ttu-id="63ead-234">**HttpContext.Request.RequestContext.RouteData**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-234">**HttpContext.Request.RequestContext.RouteData** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Route)]

<span data-ttu-id="63ead-235">**HttpContext.Request.Headers**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-235">**HttpContext.Request.Headers** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Headers)]

<span data-ttu-id="63ead-236">**HttpContext.Request.UserAgent**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-236">**HttpContext.Request.UserAgent** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Agent)]

<span data-ttu-id="63ead-237">**HttpContext.Request.UrlReferrer**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-237">**HttpContext.Request.UrlReferrer** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Referrer)]

<span data-ttu-id="63ead-238">**HttpContext.Request.ContentType**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-238">**HttpContext.Request.ContentType** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Type)]

<span data-ttu-id="63ead-239">**HttpContext.Request.Form**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-239">**HttpContext.Request.Form** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Form)]

> [!WARNING]
> <span data-ttu-id="63ead-240">讀取表單值，只有內容的子類型為*x-www-表單-urlencoded*或*表單資料*。</span><span class="sxs-lookup"><span data-stu-id="63ead-240">Read form values only if the content sub type is *x-www-form-urlencoded* or *form-data*.</span></span>

<span data-ttu-id="63ead-241">**HttpContext.Request.InputStream**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-241">**HttpContext.Request.InputStream** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Input)]

> [!WARNING]
> <span data-ttu-id="63ead-242">此程式碼只能用在處理常式型別中介軟體，在管線結尾處。</span><span class="sxs-lookup"><span data-stu-id="63ead-242">Use this code only in a handler type middleware, at the end of a pipeline.</span></span>
>
><span data-ttu-id="63ead-243">每個要求只能出現一次如上所示，您可以閱讀原始的主體。</span><span class="sxs-lookup"><span data-stu-id="63ead-243">You can read the raw body as shown above only once per request.</span></span> <span data-ttu-id="63ead-244">嘗試讀取內容之後第一次讀取會讀取空本文, 的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="63ead-244">Middleware trying to read the body after the first read will read an empty body.</span></span>
>
><span data-ttu-id="63ead-245">這不適用於讀取因為緩衝區完成表單為稍早所示。</span><span class="sxs-lookup"><span data-stu-id="63ead-245">This doesn't apply to reading a form as shown earlier, because that's done from a buffer.</span></span>

### <a name="httpcontextresponse"></a><span data-ttu-id="63ead-246">HttpContext.Response</span><span class="sxs-lookup"><span data-stu-id="63ead-246">HttpContext.Response</span></span>

<span data-ttu-id="63ead-247">**HttpContext.Response.Status**和**HttpContext.Response.StatusDescription**轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-247">**HttpContext.Response.Status** and **HttpContext.Response.StatusDescription** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Status)]

<span data-ttu-id="63ead-248">**HttpContext.Response.ContentEncoding**和**HttpContext.Response.ContentType**轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-248">**HttpContext.Response.ContentEncoding** and **HttpContext.Response.ContentType** translate to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespType)]

<span data-ttu-id="63ead-249">**HttpContext.Response.ContentType**上自己也會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-249">**HttpContext.Response.ContentType** on its own also translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_RespTypeOnly)]

<span data-ttu-id="63ead-250">**HttpContext.Response.Output**會轉譯成：</span><span class="sxs-lookup"><span data-stu-id="63ead-250">**HttpContext.Response.Output** translates to:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_Output)]

<span data-ttu-id="63ead-251">**HttpContext.Response.TransmitFile**</span><span class="sxs-lookup"><span data-stu-id="63ead-251">**HttpContext.Response.TransmitFile**</span></span>

<span data-ttu-id="63ead-252">檔案服務會討論[這裡](../fundamentals/request-features.md#middleware-and-request-features)。</span><span class="sxs-lookup"><span data-stu-id="63ead-252">Serving up a file is discussed [here](../fundamentals/request-features.md#middleware-and-request-features).</span></span>

<span data-ttu-id="63ead-253">**HttpContext.Response.Headers**</span><span class="sxs-lookup"><span data-stu-id="63ead-253">**HttpContext.Response.Headers**</span></span>

<span data-ttu-id="63ead-254">傳送回應標頭複雜，是因為，如果您將任何項目寫入至回應主體之後，它們將不會傳送。</span><span class="sxs-lookup"><span data-stu-id="63ead-254">Sending response headers is complicated by the fact that if you set them after anything has been written to the response body, they will not be sent.</span></span>

<span data-ttu-id="63ead-255">方案是將設定寫入回應啟動之前將權限呼叫的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="63ead-255">The solution is to set a callback method that will be called right before writing to the response starts.</span></span> <span data-ttu-id="63ead-256">最好的做法是在開始`Invoke`中介軟體中的方法。</span><span class="sxs-lookup"><span data-stu-id="63ead-256">This is best done at the start of the `Invoke` method in your middleware.</span></span> <span data-ttu-id="63ead-257">它是設定您的回應標頭這個回呼方法。</span><span class="sxs-lookup"><span data-stu-id="63ead-257">It's this callback method that sets your response headers.</span></span>

<span data-ttu-id="63ead-258">下列程式碼設定呼叫的回呼方法`SetHeaders`:</span><span class="sxs-lookup"><span data-stu-id="63ead-258">The following code sets a callback method called `SetHeaders`:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="63ead-259">`SetHeaders`回呼方法會看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="63ead-259">The `SetHeaders` callback method would look like this:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetHeaders)]

<span data-ttu-id="63ead-260">**HttpContext.Response.Cookies**</span><span class="sxs-lookup"><span data-stu-id="63ead-260">**HttpContext.Response.Cookies**</span></span>

<span data-ttu-id="63ead-261">周遊到中的瀏覽器的 cookie *Set-cookie*回應標頭。</span><span class="sxs-lookup"><span data-stu-id="63ead-261">Cookies travel to the browser in a *Set-Cookie* response header.</span></span> <span data-ttu-id="63ead-262">如此一來，傳送 cookie 需要所用相同的回呼傳送回應標頭：</span><span class="sxs-lookup"><span data-stu-id="63ead-262">As a result, sending cookies requires the same callback as used for sending response headers:</span></span>

```csharp
public async Task Invoke(HttpContext httpContext)
{
    // ...
    httpContext.Response.OnStarting(SetCookies, state: httpContext);
    httpContext.Response.OnStarting(SetHeaders, state: httpContext);
```

<span data-ttu-id="63ead-263">`SetCookies`回呼方法會如下所示：</span><span class="sxs-lookup"><span data-stu-id="63ead-263">The `SetCookies` callback method would look like the following:</span></span>

[!code-csharp[](http-modules/sample/Asp.Net.Core/Middleware/HttpContextDemoMiddleware.cs?name=snippet_SetCookies)]

## <a name="additional-resources"></a><span data-ttu-id="63ead-264">其他資源</span><span class="sxs-lookup"><span data-stu-id="63ead-264">Additional resources</span></span>

* [<span data-ttu-id="63ead-265">HTTP 處理常式和 HTTP 模組概觀</span><span class="sxs-lookup"><span data-stu-id="63ead-265">HTTP Handlers and HTTP Modules Overview</span></span>](/iis/configuration/system.webserver/)
* [<span data-ttu-id="63ead-266">組態</span><span class="sxs-lookup"><span data-stu-id="63ead-266">Configuration</span></span>](xref:fundamentals/configuration/index)
* [<span data-ttu-id="63ead-267">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="63ead-267">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="63ead-268">中介軟體</span><span class="sxs-lookup"><span data-stu-id="63ead-268">Middleware</span></span>](xref:fundamentals/middleware/index)
