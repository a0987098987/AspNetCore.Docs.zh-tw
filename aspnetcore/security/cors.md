---
title: 在 ASP.NET 核心中開啟跨源要求 (CORS)
author: rick-anderson
description: 瞭解 CORS 如何作為在 ASP.NET 酷應用中允許或拒絕跨源請求的標準。
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: e7731fd967c206679ac93209fdb84f40367bea37
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440905"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="c0621-103">在 ASP.NET 核心中開啟跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="c0621-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c0621-104">由[里克·安德森](https://twitter.com/RickAndMSFT)和[柯克·拉金](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="c0621-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="c0621-105">本文演示如何在ASP.NET核心應用中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="c0621-106">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="c0621-107">此限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="c0621-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="c0621-108">同源策略可防止惡意網站從其他網站讀取敏感數據。</span><span class="sxs-lookup"><span data-stu-id="c0621-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="c0621-109">有時,您可能希望允許其他網站向你的應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-109">Sometimes, you might want to allow other sites to make cross-origin requests to your app.</span></span> <span data-ttu-id="c0621-110">有關詳細資訊,請參閱[Mozilla CORS 文章](https://developer.mozilla.org/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="c0621-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="c0621-111">[跨源資源分享](https://www.w3.org/TR/cors/)(CORS):</span><span class="sxs-lookup"><span data-stu-id="c0621-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="c0621-112">是允許伺服器放鬆同源策略的 W3C 標準。</span><span class="sxs-lookup"><span data-stu-id="c0621-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="c0621-113">**CORS 不是**安全功能,可放鬆安全性。</span><span class="sxs-lookup"><span data-stu-id="c0621-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="c0621-114">通過允許 CORS,API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="c0621-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="c0621-115">有關詳細資訊,請參閱[CORS 的工作原理](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="c0621-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="c0621-116">允許伺服器顯式允許某些跨源請求,同時拒絕其他請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="c0621-117">比早期的技術(如[JSONP](/dotnet/framework/wcf/samples/jsonp))更安全、更靈活。</span><span class="sxs-lookup"><span data-stu-id="c0621-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="c0621-118">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c0621-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="c0621-119">同一源</span><span class="sxs-lookup"><span data-stu-id="c0621-119">Same origin</span></span>

<span data-ttu-id="c0621-120">如果兩個 URL 具有相同的方案、主機和埠[(RFC 6454),](https://tools.ietf.org/html/rfc6454)則它們具有相同的源源。</span><span class="sxs-lookup"><span data-stu-id="c0621-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="c0621-121">這兩個網址 有相同的來源:</span><span class="sxs-lookup"><span data-stu-id="c0621-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="c0621-122">這些網址與前兩個網址具有不同的來源:</span><span class="sxs-lookup"><span data-stu-id="c0621-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="c0621-123">`https://example.net`&ndash;不同的網域</span><span class="sxs-lookup"><span data-stu-id="c0621-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="c0621-124">`https://www.example.com/foo.html`&ndash;不同的子域</span><span class="sxs-lookup"><span data-stu-id="c0621-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="c0621-125">`http://example.com/foo.html`&ndash;不同的機制</span><span class="sxs-lookup"><span data-stu-id="c0621-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="c0621-126">`https://example.com:9000/foo.html`&ndash;不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="c0621-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

## <a name="enable-cors"></a><span data-ttu-id="c0621-127">啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-127">Enable CORS</span></span>

<span data-ttu-id="c0621-128">有三種方法可以啟用 CORS:</span><span class="sxs-lookup"><span data-stu-id="c0621-128">There are three ways to enable CORS:</span></span>

* <span data-ttu-id="c0621-129">在中間件中使用[命名政策](#np)或[預設政策](#dp)。</span><span class="sxs-lookup"><span data-stu-id="c0621-129">In middleware using a [named policy](#np) or [default policy](#dp).</span></span>
* <span data-ttu-id="c0621-130">使用[終結點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="c0621-130">Using [endpoint routing](#ecors).</span></span>
* <span data-ttu-id="c0621-131">使用[[啟用 Cors]](#attr)屬性。</span><span class="sxs-lookup"><span data-stu-id="c0621-131">With the [[EnableCors]](#attr) attribute.</span></span>

<span data-ttu-id="c0621-132">將[[EnableCors]](#attr)屬性與命名策略一起使用,可在限制支援 CORS 的終結點時提供最佳控制。</span><span class="sxs-lookup"><span data-stu-id="c0621-132">Using the [[EnableCors]](#attr) attribute with a named policy provides the finest control in limiting endpoints that support CORS.</span></span>

<span data-ttu-id="c0621-133">每種方法都詳見以下各節。</span><span class="sxs-lookup"><span data-stu-id="c0621-133">Each approach is detailed in the following sections.</span></span>

<a name="np"></a>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="c0621-134">包含命名政策與中間件的 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-134">CORS with named policy and middleware</span></span>

<span data-ttu-id="c0621-135">CORS 中間件處理跨源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-135">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="c0621-136">以下代碼將 CORS 政策應用於具有指定來源的所有應用終結點:</span><span class="sxs-lookup"><span data-stu-id="c0621-136">The following code applies a CORS policy to all the app's endpoints with the specified origins:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=3,9,31)]

<span data-ttu-id="c0621-137">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c0621-137">The preceding code:</span></span>

* <span data-ttu-id="c0621-138">將策略名稱設定到`_myAllowSpecificOrigins`。</span><span class="sxs-lookup"><span data-stu-id="c0621-138">Sets the policy name to `_myAllowSpecificOrigins`.</span></span> <span data-ttu-id="c0621-139">策略名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="c0621-139">The policy name is arbitrary.</span></span>
* <span data-ttu-id="c0621-140">調用<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>擴充方法`_myAllowSpecificOrigins`並指定 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-140">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method and specifies the  `_myAllowSpecificOrigins` CORS policy.</span></span> <span data-ttu-id="c0621-141">`UseCors`添加 CORS 中間件。</span><span class="sxs-lookup"><span data-stu-id="c0621-141">`UseCors` adds the CORS middleware.</span></span>
* <span data-ttu-id="c0621-142">使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的調用。</span><span class="sxs-lookup"><span data-stu-id="c0621-142">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="c0621-143">lambda 取<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>一個物件。</span><span class="sxs-lookup"><span data-stu-id="c0621-143">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="c0621-144">[配置選項](#cors-policy-options)(`WithOrigins`如 )在本文的後面部分介紹。</span><span class="sxs-lookup"><span data-stu-id="c0621-144">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>
* <span data-ttu-id="c0621-145">為所有控制器`_myAllowSpecificOrigins`終結點啟用 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-145">Enables the `_myAllowSpecificOrigins` CORS policy for all controller endpoints.</span></span> <span data-ttu-id="c0621-146">請參閱[終結點路由](#ecors),將 CORS 策略應用於特定終結點。</span><span class="sxs-lookup"><span data-stu-id="c0621-146">See [endpoint routing](#ecors) to apply a CORS policy to specific endpoints.</span></span>

<span data-ttu-id="c0621-147">使用端點路由時,***必須***將 CORS 中間件配置為`UseRouting`在`UseEndpoints`調用和 之間執行。</span><span class="sxs-lookup"><span data-stu-id="c0621-147">With endpoint routing, the CORS middleware ***must*** be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span>

<span data-ttu-id="c0621-148">有關測試代碼的說明,請參閱[測試 CORS,](#testc)這些說明與前面的代碼類似。</span><span class="sxs-lookup"><span data-stu-id="c0621-148">See [Test CORS](#testc) for instructions on testing code similar to the preceding code.</span></span>

<span data-ttu-id="c0621-149">方法<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>呼叫將 CORS 服務新增到應用的服務容器:</span><span class="sxs-lookup"><span data-stu-id="c0621-149">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="c0621-150">關於詳細資訊,請參考此文件中的[CORS 政策選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="c0621-150">For more information, see [CORS policy options](#cpo) in this document.</span></span>

<span data-ttu-id="c0621-151">這些方法<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>可以連結,如以下代碼所示:</span><span class="sxs-lookup"><span data-stu-id="c0621-151">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> methods can be chained, as shown in the following code:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup2.cs?name=snippet)]

<span data-ttu-id="c0621-152">注意: 指定的網址**不能**包含尾`/`隨斜槓 ( 。</span><span class="sxs-lookup"><span data-stu-id="c0621-152">Note: The specified URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="c0621-153">如果 URL`/`終止與`false`,則比較返回,並且不返回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-153">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="dp"></a>

### <a name="cors-with-default-policy-and-middleware"></a><span data-ttu-id="c0621-154">具有預設政策和中間件的 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-154">CORS with default policy and middleware</span></span>

<span data-ttu-id="c0621-155">以下突顯的代碼啟用預設 CORS 政策:</span><span class="sxs-lookup"><span data-stu-id="c0621-155">The following highlighted code enables the default CORS policy:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupDefaultPolicy.cs?name=snippet2&highlight=7,29)]

<span data-ttu-id="c0621-156">前面的代碼將預設的 CORS 策略應用於所有控制器終結點。</span><span class="sxs-lookup"><span data-stu-id="c0621-156">The preceding code applies the default CORS policy to all controller endpoints.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="c0621-157">使用端點路由啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="c0621-157">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="c0621-158">使用`RequireCors`目前使用每個的終結點的 CORS***不支援***[自動預取要求](#apf)。</span><span class="sxs-lookup"><span data-stu-id="c0621-158">Enabling CORS on a per-endpoint basis using `RequireCors` currently does ***not*** support [automatic preflight requests](#apf).</span></span> <span data-ttu-id="c0621-159">有關詳細資訊,請參閱此[GitHub 問題和](https://github.com/dotnet/aspnetcore/issues/20709)[使用終結點路由和 [HttpOptions] 測試 CORS。](#tcer)</span><span class="sxs-lookup"><span data-stu-id="c0621-159">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/20709) and [Test CORS with endpoint routing and [HttpOptions]](#tcer).</span></span>

<span data-ttu-id="c0621-160">使用端點路由,可以使用擴充<xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*>方法集,按終結點啟用 CORS:</span><span class="sxs-lookup"><span data-stu-id="c0621-160">With endpoint routing, CORS can be enabled on a per-endpoint basis using the <xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*> set of extension methods:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPt.cs?name=snippet2&highlight=3,7-15,32,40,43)]

<span data-ttu-id="c0621-161">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="c0621-161">In the preceding code:</span></span>

* <span data-ttu-id="c0621-162">`app.UseCors`啟用 CORS 中間件。</span><span class="sxs-lookup"><span data-stu-id="c0621-162">`app.UseCors` enables the CORS middleware.</span></span> <span data-ttu-id="c0621-163">由於尚未設定預設政策,`app.UseCors()`因此單獨無法啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-163">Because a default policy hasn't been configured, `app.UseCors()` alone doesn't enable CORS.</span></span>
* <span data-ttu-id="c0621-164">`/echo`和控制器終結點允許使用指定的策略進行交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-164">The `/echo` and controller endpoints allow cross-origin requests using the specified policy.</span></span>
* <span data-ttu-id="c0621-165">`/echo2`和 Razor Pages 終結點***不允許***跨源請求,因為未指定預設策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-165">The `/echo2` and Razor Pages endpoints do ***not*** allow cross-origin requests because no default policy was specified.</span></span>

<span data-ttu-id="c0621-166">[[禁用 Cors]](#dc)屬性***不會***停用由 終結`RequireCors`點路由啟用的 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-166">The [[DisableCors]](#dc) attribute does ***not***  disable CORS that has been enabled by endpoint routing with `RequireCors`.</span></span>

<span data-ttu-id="c0621-167">有關測試代碼的指令,請參閱[使用端點路由和 [HttpOptions] 測試 CORS。](#tcer)</span><span class="sxs-lookup"><span data-stu-id="c0621-167">See [Test CORS with endpoint routing and [HttpOptions]](#tcer) for instructions on testing code similar to the preceding.</span></span>

<a name="attr"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="c0621-168">使用屬性啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-168">Enable CORS with attributes</span></span>

<span data-ttu-id="c0621-169">使用[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性啟用 CORS 並將命名策略應用於僅對需要 CORS 的終結點提供最佳控制。</span><span class="sxs-lookup"><span data-stu-id="c0621-169">Enabling CORS with the [[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute and applying a named policy to only those endpoints that require CORS provides the finest control.</span></span>

<span data-ttu-id="c0621-170">[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供了全域應用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="c0621-170">The [[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="c0621-171">該`[EnableCors]`屬性為選擇的終結點啟用 CORS,而不是所有終結點:</span><span class="sxs-lookup"><span data-stu-id="c0621-171">The `[EnableCors]` attribute enables CORS for selected endpoints, rather than all endpoints:</span></span>

* <span data-ttu-id="c0621-172">`[EnableCors]`指定預設策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-172">`[EnableCors]` specifies the default policy.</span></span>
* <span data-ttu-id="c0621-173">`[EnableCors("{Policy String}")]`指定命名策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-173">`[EnableCors("{Policy String}")]` specifies a named policy.</span></span>

<span data-ttu-id="c0621-174">該`[EnableCors]`屬性可應用於:</span><span class="sxs-lookup"><span data-stu-id="c0621-174">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="c0621-175">剃刀頁面`PageModel`</span><span class="sxs-lookup"><span data-stu-id="c0621-175">Razor Page `PageModel`</span></span>
* <span data-ttu-id="c0621-176">控制器</span><span class="sxs-lookup"><span data-stu-id="c0621-176">Controller</span></span>
* <span data-ttu-id="c0621-177">控制器操作方法</span><span class="sxs-lookup"><span data-stu-id="c0621-177">Controller action method</span></span>

<span data-ttu-id="c0621-178">可以將不同的策略應用於具有屬性的`[EnableCors]`控制器、頁面模型或操作方法。</span><span class="sxs-lookup"><span data-stu-id="c0621-178">Different policies can be applied to controllers, page models, or action methods with the `[EnableCors]` attribute.</span></span> <span data-ttu-id="c0621-179">當該`[EnableCors]`屬性應用於控制器、頁面模型或操作方法,並在中間件中啟用 CORS 時,將應用***這兩個***策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-179">When the `[EnableCors]` attribute is applied to a controller, page model, or action method, and CORS is enabled in middleware, ***both*** policies are applied.</span></span> <span data-ttu-id="c0621-180">***我們建議不要合併策略。使用相同的應用程式的屬性***`[EnableCors]`***或中間件,而不是兩者在同一應用中。***</span><span class="sxs-lookup"><span data-stu-id="c0621-180">***We recommend against combining policies. Use the*** `[EnableCors]` ***attribute or middleware, not both in the same app.***</span></span>

<span data-ttu-id="c0621-181">以下代碼對每種方法應用不同的原則:</span><span class="sxs-lookup"><span data-stu-id="c0621-181">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="c0621-182">以下代碼建立兩個 CORS 政策:</span><span class="sxs-lookup"><span data-stu-id="c0621-182">The following code creates two CORS policies:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup3.cs?name=snippet&highlight=12-28,44)]

<span data-ttu-id="c0621-183">為了最好地控制限制 CORS 請求:</span><span class="sxs-lookup"><span data-stu-id="c0621-183">For the finest control of limiting CORS requests:</span></span>

* <span data-ttu-id="c0621-184">與`[EnableCors("MyPolicy")]`命名策略一起使用。</span><span class="sxs-lookup"><span data-stu-id="c0621-184">Use `[EnableCors("MyPolicy")]` with a named policy.</span></span>
* <span data-ttu-id="c0621-185">不要定義預設策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-185">Don't define a default policy.</span></span>
* <span data-ttu-id="c0621-186">不要使用[終結點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="c0621-186">Don't use [endpoint routing](#ecors).</span></span>

<span data-ttu-id="c0621-187">下一節中的代碼與前面的清單一應合。</span><span class="sxs-lookup"><span data-stu-id="c0621-187">The code in the next section meets the preceding list.</span></span>

<span data-ttu-id="c0621-188">有關測試代碼的說明,請參閱[測試 CORS,](#testc)這些說明與前面的代碼類似。</span><span class="sxs-lookup"><span data-stu-id="c0621-188">See [Test CORS](#testc) for instructions on testing code similar to the preceding code.</span></span>

<a name="dc"></a>

### <a name="disable-cors"></a><span data-ttu-id="c0621-189">關閉 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-189">Disable CORS</span></span>

<span data-ttu-id="c0621-190">[[禁用 Cors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性***不會***停用終結點[路由](#ecors)已啟用的 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-190">The [[DisableCors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute does ***not***  disable CORS that has been enabled by [endpoint routing](#ecors).</span></span>

<span data-ttu-id="c0621-191">以下代碼定義 CORS`"MyPolicy"`政策 :</span><span class="sxs-lookup"><span data-stu-id="c0621-191">The following code defines the CORS policy `"MyPolicy"`:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTestMyPolicy.cs?name=snippet)]

<span data-ttu-id="c0621-192">以下代碼關閉操作的`GetValues2`CORS:</span><span class="sxs-lookup"><span data-stu-id="c0621-192">The following code disables CORS for the `GetValues2` action:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet&highlight=1,23)]

<span data-ttu-id="c0621-193">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c0621-193">The preceding code:</span></span>

* <span data-ttu-id="c0621-194">不支援帶[終結點路由](#ecors)的 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-194">Doesn't enable CORS with [endpoint routing](#ecors).</span></span>
* <span data-ttu-id="c0621-195">不定義[預設的 CORS 政策](#dp)。</span><span class="sxs-lookup"><span data-stu-id="c0621-195">Doesn't define a [default CORS policy](#dp).</span></span>
* <span data-ttu-id="c0621-196">使用[[啟用 Cors("我的策略")]](#attr)`"MyPolicy"`為控制器啟用 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-196">Uses [[EnableCors("MyPolicy")]](#attr) to enable the `"MyPolicy"` CORS policy for the controller.</span></span>
* <span data-ttu-id="c0621-197">禁用方法的`GetValues2`CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-197">Disables CORS for the `GetValues2` method.</span></span>

<span data-ttu-id="c0621-198">有關測試上述代碼的說明,請參閱[測試 CORS。](#testc)</span><span class="sxs-lookup"><span data-stu-id="c0621-198">See [Test CORS](#testc) for instructions on testing the preceding code.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="c0621-199">CORS 政策選項</span><span class="sxs-lookup"><span data-stu-id="c0621-199">CORS policy options</span></span>

<span data-ttu-id="c0621-200">本節介紹可在 CORS 策略中設定的各種選項:</span><span class="sxs-lookup"><span data-stu-id="c0621-200">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="c0621-201">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="c0621-201">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="c0621-202">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="c0621-202">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="c0621-203">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-203">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="c0621-204">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-204">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="c0621-205">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="c0621-205">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="c0621-206">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="c0621-206">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="c0621-207"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>在`Startup.ConfigureServices`中 呼叫 。</span><span class="sxs-lookup"><span data-stu-id="c0621-207"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c0621-208">對於某些選項,首先閱讀[「CORS 的工作原理」](#how-cors)部分可能會有所説明。</span><span class="sxs-lookup"><span data-stu-id="c0621-208">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="c0621-209">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="c0621-209">Set the allowed origins</span></span>

<span data-ttu-id="c0621-210"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>&ndash;允許使用任何方案 (`http``https`或 ) 從所有來源請求 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-210"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="c0621-211">`AllowAnyOrigin`不安全,因為*任何網站*都可以向應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-211">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="c0621-212">指定`AllowAnyOrigin``AllowCredentials`和 是不安全的配置,可能會導致跨網站請求偽造。</span><span class="sxs-lookup"><span data-stu-id="c0621-212">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="c0621-213">當應用同時配置這兩種方法時,CORS 服務返回無效的 CORS 回應。</span><span class="sxs-lookup"><span data-stu-id="c0621-213">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="c0621-214">`AllowAnyOrigin`影響預檢請求和`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-214">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="c0621-215">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-215">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="c0621-216"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash;將<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略的屬性設為一個函數,允許源在評估是否允許原點時匹配配置的通配符域。</span><span class="sxs-lookup"><span data-stu-id="c0621-216"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="c0621-217">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="c0621-217">Set the allowed HTTP methods</span></span>

<span data-ttu-id="c0621-218"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-218"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="c0621-219">允許任何 HTTP 方法:</span><span class="sxs-lookup"><span data-stu-id="c0621-219">Allows any HTTP method:</span></span>
* <span data-ttu-id="c0621-220">影響印前請求和`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-220">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="c0621-221">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-221">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="c0621-222">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-222">Set the allowed request headers</span></span>

<span data-ttu-id="c0621-223">要允許在 CORS 請求中傳送特定標頭,請呼叫作者[要求標頭](https://xhr.spec.whatwg.org/#request),呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭:</span><span class="sxs-lookup"><span data-stu-id="c0621-223">To allow specific headers to be sent in a CORS request, called [author request headers](https://xhr.spec.whatwg.org/#request), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

<span data-ttu-id="c0621-224">要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers),請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="c0621-224">To allow all [author request headers](https://www.w3.org/TR/cors/#author-request-headers), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

<span data-ttu-id="c0621-225">`AllowAnyHeader`影響預取要求與[存取控制要求標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)。</span><span class="sxs-lookup"><span data-stu-id="c0621-225">`AllowAnyHeader` affects preflight requests and the [Access-Control-Request-Headers](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method) header.</span></span> <span data-ttu-id="c0621-226">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-226">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="c0621-227">僅當中`Access-Control-Request-Headers`發送的標頭與`WithHeaders`中 所述標頭`WithHeaders`完全匹配時 ,才能與 指定的特定標頭匹配 CORS 中間件策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-227">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="c0621-228">例如,請考慮配置如下的應用:</span><span class="sxs-lookup"><span data-stu-id="c0621-228">For instance, consider an app configured as follows:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet4)]

<span data-ttu-id="c0621-229">CORS 中介件拒絕具有以下請求標頭的預檢要求,`Content-Language`因為 ([標題名稱. Content 語言](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 未在`WithHeaders`中列出:</span><span class="sxs-lookup"><span data-stu-id="c0621-229">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="c0621-230">該應用程式返回*200 OK*回應,但不會將 CORS 標頭髮送回。</span><span class="sxs-lookup"><span data-stu-id="c0621-230">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="c0621-231">因此,瀏覽器不會嘗試跨源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-231">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="c0621-232">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-232">Set the exposed response headers</span></span>

<span data-ttu-id="c0621-233">默認情況下,瀏覽器不會向應用公開所有響應標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-233">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="c0621-234">有關詳細資訊,請參閱[W3C 跨源資源分享(術語):簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="c0621-234">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="c0621-235">預設情況下可用的回應標頭是:</span><span class="sxs-lookup"><span data-stu-id="c0621-235">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="c0621-236">CORS 規範呼叫這些標頭*為簡單回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="c0621-236">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="c0621-237">要使其他標頭可供應用使用,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-237">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet5)]
### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="c0621-238">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="c0621-238">Credentials in cross-origin requests</span></span>

<span data-ttu-id="c0621-239">憑據需要在 CORS 請求中特殊處理。</span><span class="sxs-lookup"><span data-stu-id="c0621-239">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="c0621-240">默認情況下,瀏覽器不會發送具有跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-240">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="c0621-241">認證包括 Cookie 和 HTTP 驗證方案。</span><span class="sxs-lookup"><span data-stu-id="c0621-241">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="c0621-242">要使用跨源要求傳送認證,客戶端必須設定`XMLHttpRequest.withCredentials``true`為 。</span><span class="sxs-lookup"><span data-stu-id="c0621-242">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="c0621-243">直接`XMLHttpRequest`使用:</span><span class="sxs-lookup"><span data-stu-id="c0621-243">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="c0621-244">使用 jQuery:</span><span class="sxs-lookup"><span data-stu-id="c0621-244">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="c0621-245">使用[擷取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="c0621-245">Using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="c0621-246">伺服器必須允許憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-246">The server must allow the credentials.</span></span> <span data-ttu-id="c0621-247">要允許跨源認證,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-247">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet6)]

<span data-ttu-id="c0621-248">HTTP 回應包括`Access-Control-Allow-Credentials`一個標頭,該標頭告訴瀏覽器伺服器允許跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-248">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="c0621-249">如果瀏覽器發送憑據,但回應不包含有效的`Access-Control-Allow-Credentials`標頭,則瀏覽器不會向應用公開回應,並且跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="c0621-249">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="c0621-250">允許跨源憑據存在安全風險。</span><span class="sxs-lookup"><span data-stu-id="c0621-250">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="c0621-251">另一個域中的網站可以在使用者不知情的情況下代表使用者向應用發送登錄使用者的憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-251">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="c0621-252">CORS 規範還規定,如果標頭存在`"*"``Access-Control-Allow-Credentials`, 則將原點設置為(所有源)無效。</span><span class="sxs-lookup"><span data-stu-id="c0621-252">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

<a name="pref"></a>

## <a name="preflight-requests"></a><span data-ttu-id="c0621-253">預先要求</span><span class="sxs-lookup"><span data-stu-id="c0621-253">Preflight requests</span></span>

<span data-ttu-id="c0621-254">對於某些 CORS 請求,瀏覽器在發出實際請求之前會發送其他[OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-254">For some CORS requests, the browser sends an additional [OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS) request before making the actual request.</span></span> <span data-ttu-id="c0621-255">此要求為[預先要求](https://developer.mozilla.org/docs/Glossary/Preflight_request)。</span><span class="sxs-lookup"><span data-stu-id="c0621-255">This request is called a [preflight request](https://developer.mozilla.org/docs/Glossary/Preflight_request).</span></span> <span data-ttu-id="c0621-256">如果以下***所有條件都***為 true,瀏覽器可以跳過預檢請求:</span><span class="sxs-lookup"><span data-stu-id="c0621-256">The browser can skip the preflight request if ***all*** the following conditions are true:</span></span>

* <span data-ttu-id="c0621-257">請求方法是 GET、頭或 POST。</span><span class="sxs-lookup"><span data-stu-id="c0621-257">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="c0621-258">套用不設定`Accept`要求 標頭以外`Accept-Language``Content-Language``Content-Type`的`Last-Event-ID`、、、、 或 。</span><span class="sxs-lookup"><span data-stu-id="c0621-258">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="c0621-259">標頭`Content-Type`(如果已設定)具有以下值之一:</span><span class="sxs-lookup"><span data-stu-id="c0621-259">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="c0621-260">為用戶端請求設置的請求標頭規則適用於應用通過調用`setRequestHeader`物件設置`XMLHttpRequest`的 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-260">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="c0621-261">CORS 規範呼叫這些標頭[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)。</span><span class="sxs-lookup"><span data-stu-id="c0621-261">The CORS specification calls these headers [author request headers](https://www.w3.org/TR/cors/#author-request-headers).</span></span> <span data-ttu-id="c0621-262">這個規則不適用於瀏覽器可以設定的標頭,例如`User-Agent` `Host` `Content-Length` 。</span><span class="sxs-lookup"><span data-stu-id="c0621-262">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="c0621-263">下面是與本文件[「測試」](#testc)部分中的 **[放置測試]** 按鈕所做的預檢請求類似的範例回應。</span><span class="sxs-lookup"><span data-stu-id="c0621-263">The following is an example response similar to the preflight request made from the **[Put test]** button in the [Test CORS](#testc) section of this document.</span></span>

```
General:
Request URL: https://cors3.azurewebsites.net/api/values/5
Request Method: OPTIONS
Status Code: 204 No Content

Response Headers:
Access-Control-Allow-Methods: PUT,DELETE,GET
Access-Control-Allow-Origin: https://cors1.azurewebsites.net
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f8...8;Path=/;HttpOnly;Domain=cors1.azurewebsites.net
Vary: Origin

Request Headers:
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Access-Control-Request-Method: PUT
Connection: keep-alive
Host: cors3.azurewebsites.net
Origin: https://cors1.azurewebsites.net
Referer: https://cors1.azurewebsites.net/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0
```

<span data-ttu-id="c0621-264">預檢請求使用[HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)方法。</span><span class="sxs-lookup"><span data-stu-id="c0621-264">The preflight request uses the [HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS) method.</span></span> <span data-ttu-id="c0621-265">它可能包括以下標頭:</span><span class="sxs-lookup"><span data-stu-id="c0621-265">It may include the following headers:</span></span>

* <span data-ttu-id="c0621-266">[存取控制-請求方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method):將用於實際請求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="c0621-266">[Access-Control-Request-Method](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method): The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="c0621-267">[存取控制-請求-標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers):應用在實際請求上設置的請求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="c0621-267">[Access-Control-Request-Headers](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers): A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="c0621-268">如前所述,這不包括瀏覽器設定的標頭,如`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="c0621-268">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>
* [<span data-ttu-id="c0621-269">存取控制-允許方法</span><span class="sxs-lookup"><span data-stu-id="c0621-269">Access-Control-Allow-Methods</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

<span data-ttu-id="c0621-270">如果預檢請求被拒絕,應用將返回回應`200 OK`,但不會設置 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-270">If the preflight request is denied, the app returns a `200 OK` response but doesn't set the CORS headers.</span></span> <span data-ttu-id="c0621-271">因此,瀏覽器不會嘗試跨源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-271">Therefore, the browser doesn't attempt the cross-origin request.</span></span> <span data-ttu-id="c0621-272">有關被拒絕的印前請求的範例,請參閱本文件的[「測試 CORS」](#testc)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-272">For an example of a denied preflight request, see the [Test CORS](#testc) section of this document.</span></span>

<span data-ttu-id="c0621-273">使用 F12 工具,主控台應用程式會顯示類似於以下錯誤之一的錯誤,具體取決於瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="c0621-273">Using the F12 tools, the console app shows an error similar to one of the following, depending on the browser:</span></span>

* <span data-ttu-id="c0621-274">Firefox: 跨源請求被阻止: 同一源策略不允許讀`https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5`取 中的遠端資源。</span><span class="sxs-lookup"><span data-stu-id="c0621-274">Firefox: Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at `https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5`.</span></span> <span data-ttu-id="c0621-275">(原因:CORS 請求未成功)。</span><span class="sxs-lookup"><span data-stu-id="c0621-275">(Reason: CORS request did not succeed).</span></span> [<span data-ttu-id="c0621-276">深入了解</span><span class="sxs-lookup"><span data-stu-id="c0621-276">Learn More</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS/Errors/CORSDidNotSucceed)
* <span data-ttu-id="c0621-277">基於鉻:CORS 策略阻止了以https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5『 來自https://cors3.azurewebsites.net源' 獲取的訪問:對印前請求的回應未通過訪問控制檢查:請求的資源上不存在"訪問-控制-允許-原點"標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-277">Chromium based: Access to fetch at 'https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5' from origin 'https://cors3.azurewebsites.net' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span> <span data-ttu-id="c0621-278">如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。</span><span class="sxs-lookup"><span data-stu-id="c0621-278">If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</span></span>

<span data-ttu-id="c0621-279">要允許特定標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-279">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

<span data-ttu-id="c0621-280">要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers),請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="c0621-280">To allow all [author request headers](https://www.w3.org/TR/cors/#author-request-headers), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

<span data-ttu-id="c0621-281">瀏覽器的設置`Access-Control-Request-Headers`方式不一致。</span><span class="sxs-lookup"><span data-stu-id="c0621-281">Browsers aren't consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="c0621-282">如果任一:</span><span class="sxs-lookup"><span data-stu-id="c0621-282">If either:</span></span>

* <span data-ttu-id="c0621-283">標頭設定為`"*"`</span><span class="sxs-lookup"><span data-stu-id="c0621-283">Headers are set to anything other than `"*"`</span></span>
* <span data-ttu-id="c0621-284"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>呼叫:`Accept`至少`Content-Type`包括`Origin`, 和, 以及要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-284"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*> is called: Include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<a name="apf"></a>

### <a name="automatic-preflight-request-code"></a><span data-ttu-id="c0621-285">自動預先要求代碼</span><span class="sxs-lookup"><span data-stu-id="c0621-285">Automatic preflight request code</span></span>

<span data-ttu-id="c0621-286">應用 CORS 策略時,</span><span class="sxs-lookup"><span data-stu-id="c0621-286">When the CORS policy is applied either:</span></span>

* <span data-ttu-id="c0621-287">透過 呼`app.UseCors``Startup.Configure`叫 在全球範圍內呼叫 。</span><span class="sxs-lookup"><span data-stu-id="c0621-287">Globally by calling `app.UseCors` in `Startup.Configure`.</span></span>
* <span data-ttu-id="c0621-288">使用`[EnableCors]`屬性。</span><span class="sxs-lookup"><span data-stu-id="c0621-288">Using the `[EnableCors]` attribute.</span></span>

<span data-ttu-id="c0621-289">ASP.NET核心回應預檢選項請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-289">ASP.NET Core responds to the preflight OPTIONS request.</span></span>

<span data-ttu-id="c0621-290">使用`RequireCors`目前基於每個終結點啟用 CORS***不支援***自動預檢請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-290">Enabling CORS on a per-endpoint basis using `RequireCors` currently does ***not*** support automatic preflight requests.</span></span>

<span data-ttu-id="c0621-291">本文件的[「測試 CORS」](#testc)部分演示了此行為。</span><span class="sxs-lookup"><span data-stu-id="c0621-291">The [Test CORS](#testc) section of this document demonstrates this behavior.</span></span>

<a name="pro"></a>

### <a name="httpoptions-attribute-for-preflight-requests"></a><span data-ttu-id="c0621-292">預先要求的 [HttpOptions] 屬性</span><span class="sxs-lookup"><span data-stu-id="c0621-292">[HttpOptions] attribute for preflight requests</span></span>

<span data-ttu-id="c0621-293">當使用適當的策略啟用 CORS 時,ASP.NET核心通常會自動回應 CORS 預檢請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-293">When CORS is enabled with the appropriate policy, ASP.NET Core generally responds to CORS preflight requests automatically.</span></span> <span data-ttu-id="c0621-294">在某些情況下,情況可能並非如此。</span><span class="sxs-lookup"><span data-stu-id="c0621-294">In some scenarios, this may not be the case.</span></span> <span data-ttu-id="c0621-295">例如,將[CORS 與終結點路由一起使用](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="c0621-295">For example, using [CORS with endpoint routing](#ecors).</span></span>

<span data-ttu-id="c0621-296">以下代碼使用[[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute)屬性為 OPTIONS 請求建立終結點:</span><span class="sxs-lookup"><span data-stu-id="c0621-296">The following code uses the [[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute) attribute to create endpoints for OPTIONS requests:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet&highlight=5-17)]

<span data-ttu-id="c0621-297">有關測試上述代碼的說明,請參閱[使用終結點路由和 [HttpOptions] 測試 CORS。](#tcer)</span><span class="sxs-lookup"><span data-stu-id="c0621-297">See [Test CORS with endpoint routing and [HttpOptions]](#tcer) for instructions on testing the preceding code.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="c0621-298">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="c0621-298">Set the preflight expiration time</span></span>

<span data-ttu-id="c0621-299">標頭`Access-Control-Max-Age`指定對預檢請求的回應可以緩存多長時間。</span><span class="sxs-lookup"><span data-stu-id="c0621-299">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="c0621-300">要設定這個標頭,請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="c0621-300">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet7)]
<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="c0621-301">CORS 的工作原理</span><span class="sxs-lookup"><span data-stu-id="c0621-301">How CORS works</span></span>

<span data-ttu-id="c0621-302">本節介紹在 HTTP 消息級別發生的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)請求中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="c0621-302">This section describes what happens in a [CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="c0621-303">CORS**不是**一個安全功能。</span><span class="sxs-lookup"><span data-stu-id="c0621-303">CORS is **not** a security feature.</span></span> <span data-ttu-id="c0621-304">CORS 是一種 W3C 標準,允許伺服器放鬆同源策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-304">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="c0621-305">例如,惡意參與者可能會對您的網站使用[跨網站腳本 (XSS),](xref:security/cross-site-scripting)並對其啟用 CORS 的網站執行跨網站請求以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="c0621-305">For example, a malicious actor could use [Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="c0621-306">通過允許 CORS,API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="c0621-306">An API isn't safer by allowing CORS.</span></span>
  * <span data-ttu-id="c0621-307">由用戶端(瀏覽器)來強制實施 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-307">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="c0621-308">伺服器執行請求並返回回應,返回錯誤的是用戶端並阻止回應。</span><span class="sxs-lookup"><span data-stu-id="c0621-308">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="c0621-309">例如,以下任何工具將顯示伺服器回應:</span><span class="sxs-lookup"><span data-stu-id="c0621-309">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="c0621-310">Fiddler</span><span class="sxs-lookup"><span data-stu-id="c0621-310">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="c0621-311">Postman</span><span class="sxs-lookup"><span data-stu-id="c0621-311">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="c0621-312">.NET HTTPClient</span><span class="sxs-lookup"><span data-stu-id="c0621-312">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="c0621-313">通過在位址列中輸入 URL 來訪問 Web 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c0621-313">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="c0621-314">這是伺服器允許瀏覽器執行跨源[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)請求的一種方式,否則將禁止這樣做。</span><span class="sxs-lookup"><span data-stu-id="c0621-314">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="c0621-315">沒有 CORS 的瀏覽器無法執行跨源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-315">Browsers without CORS can't do cross-origin requests.</span></span> <span data-ttu-id="c0621-316">在 CORS 之前[,JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)曾用於規避此限制。</span><span class="sxs-lookup"><span data-stu-id="c0621-316">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="c0621-317">JSONP 不使用 XHR,`<script>`它使用 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="c0621-317">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="c0621-318">允許跨源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="c0621-318">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="c0621-319">[CORS 規範](https://www.w3.org/TR/cors/)引入了幾個支援跨源請求的新 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-319">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="c0621-320">如果瀏覽器支援 CORS,它將自動為跨源請求設置這些標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-320">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="c0621-321">啟用 CORS 不需要自訂 JavaScript 代碼。</span><span class="sxs-lookup"><span data-stu-id="c0621-321">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="c0621-322">已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的[PUT 測試按鈕](https://cors3.azurewebsites.net/test)</span><span class="sxs-lookup"><span data-stu-id="c0621-322">The  [PUT test button](https://cors3.azurewebsites.net/test) on the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)</span></span>

<span data-ttu-id="c0621-323">下面是從[「值](https://cors3.azurewebsites.net/)」測試`https://cors1.azurewebsites.net/api/values`按鈕到的跨源請求的範例。</span><span class="sxs-lookup"><span data-stu-id="c0621-323">The following is an example of a cross-origin request from the [Values](https://cors3.azurewebsites.net/) test button to `https://cors1.azurewebsites.net/api/values`.</span></span> <span data-ttu-id="c0621-324">標頭`Origin`:</span><span class="sxs-lookup"><span data-stu-id="c0621-324">The `Origin` header:</span></span>

* <span data-ttu-id="c0621-325">提供發出請求的網站的域。</span><span class="sxs-lookup"><span data-stu-id="c0621-325">Provides the domain of the site that's making the request.</span></span>
* <span data-ttu-id="c0621-326">是必需的,並且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="c0621-326">Is required and must be different from the host.</span></span>

<span data-ttu-id="c0621-327">**一般標頭**</span><span class="sxs-lookup"><span data-stu-id="c0621-327">**General headers**</span></span>

```
Request URL: https://cors1.azurewebsites.net/api/values
Request Method: GET
Status Code: 200 OK
```

<span data-ttu-id="c0621-328">**回應標頭**</span><span class="sxs-lookup"><span data-stu-id="c0621-328">**Response headers**</span></span>

```
Content-Encoding: gzip
Content-Type: text/plain; charset=utf-8
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors1.azurewebsites.net
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Powered-By: ASP.NET
```

<span data-ttu-id="c0621-329">**要求標頭**</span><span class="sxs-lookup"><span data-stu-id="c0621-329">**Request headers**</span></span>

```
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Connection: keep-alive
Host: cors1.azurewebsites.net
Origin: https://cors3.azurewebsites.net
Referer: https://cors3.azurewebsites.net/
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0 ...
```

<span data-ttu-id="c0621-330">在`OPTIONS`請求中,伺服器在回應中設置**回應**`Access-Control-Allow-Origin: {allowed origin}`標頭標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-330">In `OPTIONS` requests, the server sets the **Response headers** `Access-Control-Allow-Origin: {allowed origin}` header in the response.</span></span> <span data-ttu-id="c0621-331">例如,已部署[的範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) [,刪除 [啟用 Cors]](https://cors1.azurewebsites.net/test?number=2)按鈕`OPTIONS`請求包含以下標頭:</span><span class="sxs-lookup"><span data-stu-id="c0621-331">For example, the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI), [Delete [EnableCors]](https://cors1.azurewebsites.net/test?number=2) button `OPTIONS` request contains the following  headers:</span></span>

<span data-ttu-id="c0621-332">**一般標頭**</span><span class="sxs-lookup"><span data-stu-id="c0621-332">**General headers**</span></span>

```
Request URL: https://cors3.azurewebsites.net/api/TodoItems2/MyDelete2/5
Request Method: OPTIONS
Status Code: 204 No Content
```

<span data-ttu-id="c0621-333">**回應標頭**</span><span class="sxs-lookup"><span data-stu-id="c0621-333">**Response headers**</span></span>

```
Access-Control-Allow-Headers: Content-Type,x-custom-header
Access-Control-Allow-Methods: PUT,DELETE,GET,OPTIONS
Access-Control-Allow-Origin: https://cors1.azurewebsites.net
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors3.azurewebsites.net
Vary: Origin
X-Powered-By: ASP.NET
```

<span data-ttu-id="c0621-334">**要求標頭**</span><span class="sxs-lookup"><span data-stu-id="c0621-334">**Request headers**</span></span>

```
Accept: */*
Accept-Encoding: gzip, deflate, br
Accept-Language: en-US,en;q=0.9
Access-Control-Request-Headers: content-type
Access-Control-Request-Method: DELETE
Connection: keep-alive
Host: cors3.azurewebsites.net
Origin: https://cors1.azurewebsites.net
Referer: https://cors1.azurewebsites.net/test?number=2
Sec-Fetch-Dest: empty
Sec-Fetch-Mode: cors
Sec-Fetch-Site: cross-site
User-Agent: Mozilla/5.0
```

<span data-ttu-id="c0621-335">在前面的**回應標頭中**,伺服器在回應中設置[訪問控制允許源](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-335">In the preceding **Response headers**, the server sets the [Access-Control-Allow-Origin](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) header in the response.</span></span> <span data-ttu-id="c0621-336">此`https://cors1.azurewebsites.net`標頭的值與請求中的`Origin`標頭匹配。</span><span class="sxs-lookup"><span data-stu-id="c0621-336">The `https://cors1.azurewebsites.net` value of this header matches the `Origin` header from the request.</span></span>

<span data-ttu-id="c0621-337">如果<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>呼叫`Access-Control-Allow-Origin: *`, 則傳回的通配符值。</span><span class="sxs-lookup"><span data-stu-id="c0621-337">If <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> is called, the `Access-Control-Allow-Origin: *`, the wildcard value, is returned.</span></span> <span data-ttu-id="c0621-338">`AllowAnyOrigin`允許任何來源。</span><span class="sxs-lookup"><span data-stu-id="c0621-338">`AllowAnyOrigin` allows any origin.</span></span>

<span data-ttu-id="c0621-339">如果回應不包括標頭,`Access-Control-Allow-Origin`則跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="c0621-339">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="c0621-340">具體來說,瀏覽器不允許請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-340">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="c0621-341">即使伺服器返回成功的回應,瀏覽器也不會使回應對用戶端應用可用。</span><span class="sxs-lookup"><span data-stu-id="c0621-341">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="options"></a>

### <a name="display-options-requests"></a><span data-ttu-id="c0621-342">顯示選項要求</span><span class="sxs-lookup"><span data-stu-id="c0621-342">Display OPTIONS requests</span></span>

<span data-ttu-id="c0621-343">默認情況下,Chrome 和邊緣瀏覽器不會在 F12 工具的網路選項卡上顯示選項請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-343">By default, the Chrome and Edge browsers don't show OPTIONS requests on the network tab of the F12 tools.</span></span> <span data-ttu-id="c0621-344">要在這些瀏覽器中顯示選項請求,</span><span class="sxs-lookup"><span data-stu-id="c0621-344">To display OPTIONS requests in these browsers:</span></span>

* <span data-ttu-id="c0621-345">`chrome://flags/#out-of-blink-cors` 或 `edge://flags/#out-of-blink-cors`</span><span class="sxs-lookup"><span data-stu-id="c0621-345">`chrome://flags/#out-of-blink-cors` or `edge://flags/#out-of-blink-cors`</span></span>
* <span data-ttu-id="c0621-346">禁用標誌。</span><span class="sxs-lookup"><span data-stu-id="c0621-346">disable the flag.</span></span>
* <span data-ttu-id="c0621-347">重新啟動。</span><span class="sxs-lookup"><span data-stu-id="c0621-347">restart.</span></span>

<span data-ttu-id="c0621-348">默認情況下,Firefox 會顯示選項請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-348">Firefox shows OPTIONS requests by default.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="c0621-349">IIS 的 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-349">CORS in IIS</span></span>

<span data-ttu-id="c0621-350">部署到IIS時,如果伺服器未配置為允許匿名訪問,則CORS必須在Windows身份驗證之前運行。</span><span class="sxs-lookup"><span data-stu-id="c0621-350">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="c0621-351">為了支援此專案,需要為應用程式安裝和設定[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="c0621-351">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

<a name="testc"></a>

## <a name="test-cors"></a><span data-ttu-id="c0621-352">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-352">Test CORS</span></span>

<span data-ttu-id="c0621-353">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)具有用於測試 CORS 的代碼。</span><span class="sxs-lookup"><span data-stu-id="c0621-353">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) has code to test CORS.</span></span> <span data-ttu-id="c0621-354">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="c0621-354">See [how to download](xref:index#how-to-download-a-sample).</span></span> <span data-ttu-id="c0621-355">該範例為新增 Razor 頁面的 API 專案:</span><span class="sxs-lookup"><span data-stu-id="c0621-355">The sample is an API project with Razor Pages added:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTest2.cs?name=snippet2)]

  > [!WARNING]
  > <span data-ttu-id="c0621-356">`WithOrigins("https://localhost:<port>");`應僅用於測試類似於[下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors)的範例應用。</span><span class="sxs-lookup"><span data-stu-id="c0621-356">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors).</span></span>

<span data-ttu-id="c0621-357">下面`ValuesController`提供測試的終結點:</span><span class="sxs-lookup"><span data-stu-id="c0621-357">The following `ValuesController` provides the endpoints for testing:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet)]

<span data-ttu-id="c0621-358">[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs)由[Rick.Docs.sample.RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet 包提供,並顯示路線資訊。</span><span class="sxs-lookup"><span data-stu-id="c0621-358">[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs) is provided by the [Rick.Docs.Samples.RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet package and displays route information.</span></span>

<span data-ttu-id="c0621-359">使用以下方法之一測試前面的範例碼:</span><span class="sxs-lookup"><span data-stu-id="c0621-359">Test the preceding sample code by using one of the following approaches:</span></span>

* <span data-ttu-id="c0621-360">在中使用部署的範例[https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/)應用。</span><span class="sxs-lookup"><span data-stu-id="c0621-360">Use the deployed sample app at [https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/).</span></span> <span data-ttu-id="c0621-361">無需下載示例。</span><span class="sxs-lookup"><span data-stu-id="c0621-361">There is no need to download the sample.</span></span>
* <span data-ttu-id="c0621-362">`dotnet run`使用`https://localhost:5001`的預設網址 執行範例。</span><span class="sxs-lookup"><span data-stu-id="c0621-362">Run the sample with `dotnet run` using the default URL of `https://localhost:5001`.</span></span>
* <span data-ttu-id="c0621-363">從 Visual Studio 運行範例,埠設定為 44398,`https://localhost:44398`用於 URL。</span><span class="sxs-lookup"><span data-stu-id="c0621-363">Run the sample from Visual Studio with the port set to 44398 for a URL of `https://localhost:44398`.</span></span>

<span data-ttu-id="c0621-364">將瀏覽器與 F12 工具一起使用:</span><span class="sxs-lookup"><span data-stu-id="c0621-364">Using a browser with the F12 tools:</span></span>

* <span data-ttu-id="c0621-365">選擇「**值」** 按鈕並檢視 **「網路」** 選項卡中的標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-365">Select the **Values** button and review the headers in the **Network** tab.</span></span>
* <span data-ttu-id="c0621-366">選擇**PUT 測試**按鈕。</span><span class="sxs-lookup"><span data-stu-id="c0621-366">Select the **PUT test** button.</span></span> <span data-ttu-id="c0621-367">關於顯示 OPTIONS 要求的說明,請參考[顯示選項要求](#options)。</span><span class="sxs-lookup"><span data-stu-id="c0621-367">See [Display OPTIONS requests](#options) for instructions on displaying the OPTIONS request.</span></span> <span data-ttu-id="c0621-368">**PUT 測試**創建兩個請求,一個選項預檢請求和 PUT 請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-368">The **PUT test** creates two requests, an OPTIONS preflight request and the PUT request.</span></span>
* <span data-ttu-id="c0621-369">選擇按鈕**`GetValues2 [DisableCors]`** 以觸發失敗的 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-369">Select the **`GetValues2 [DisableCors]`** button to trigger a failed CORS request.</span></span> <span data-ttu-id="c0621-370">如文檔中所述,回應返回 200 成功,但未發出 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-370">As mentioned in the document, the response returns 200 success, but the CORS request is not made.</span></span> <span data-ttu-id="c0621-371">選擇 **「控制台」** 選項卡以檢視 CORS 錯誤。</span><span class="sxs-lookup"><span data-stu-id="c0621-371">Select the **Console** tab to see the CORS error.</span></span> <span data-ttu-id="c0621-372">根據瀏覽器的不同,將顯示類似於以下內容的錯誤:</span><span class="sxs-lookup"><span data-stu-id="c0621-372">Depending on the browser, an error similar to the following is displayed:</span></span>

     <span data-ttu-id="c0621-373">CORS`'https://cors1.azurewebsites.net/api/values/GetValues2'`策略 阻止`'https://cors3.azurewebsites.net'`了從 源提取的訪問:請求的資源上不存在"訪問-控制-允許源"標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-373">Access to fetch at `'https://cors1.azurewebsites.net/api/values/GetValues2'` from origin `'https://cors3.azurewebsites.net'` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span> <span data-ttu-id="c0621-374">如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。</span><span class="sxs-lookup"><span data-stu-id="c0621-374">If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</span></span>
     
<span data-ttu-id="c0621-375">支援 CORS 的終結點可以使用工具進行測試,例如[捲曲](https://curl.haxx.se/)[、Fiddler](https://www.telerik.com/fiddler)或[Postman。](https://www.getpostman.com/)</span><span class="sxs-lookup"><span data-stu-id="c0621-375">CORS-enabled endpoints can be tested with a tool, such as [curl](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler), or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="c0621-376">使用工具時,`Origin`標頭指定的請求的來源必須不同於接收請求的主機。</span><span class="sxs-lookup"><span data-stu-id="c0621-376">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="c0621-377">如果要求不是基於標頭的值*的跨源*: `Origin`</span><span class="sxs-lookup"><span data-stu-id="c0621-377">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="c0621-378">CORS 中間件無需處理請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-378">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="c0621-379">回應中未返回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-379">CORS headers aren't returned in the response.</span></span>

<span data-ttu-id="c0621-380">以下指令用於`curl`送出帶有資訊的 OPTIONS 請求:</span><span class="sxs-lookup"><span data-stu-id="c0621-380">The following command uses `curl` to issue an OPTIONS request with information:</span></span>

```bash
curl -X OPTIONS https://cors3.azurewebsites.net/api/TodoItems2/5 -i
```

<!--
curl come with Git. Add to path variable
C:\Program Files\Git\mingw64\bin\
-->

<a name="tcer"></a>

### <a name="test-cors-with-endpoint-routing-and-httpoptions"></a><span data-ttu-id="c0621-381">使用端點路由與 [httpOptions] 測試 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-381">Test CORS with endpoint routing and [HttpOptions]</span></span>

<span data-ttu-id="c0621-382">使用`RequireCors`目前使用每個的終結點的 CORS***不支援***[自動預取要求](#apf)。</span><span class="sxs-lookup"><span data-stu-id="c0621-382">Enabling CORS on a per-endpoint basis using `RequireCors` currently does ***not*** support [automatic preflight requests](#apf).</span></span> <span data-ttu-id="c0621-383">請考慮以下使用[終結點路由啟用 CORS 的代碼](#ecors):</span><span class="sxs-lookup"><span data-stu-id="c0621-383">Consider the following code which uses [endpoint routing to enable CORS](#ecors):</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPointBugTest.cs?name=snippet2)]

<span data-ttu-id="c0621-384">以下內容`TodoItems1Controller`提供測試的終結點:</span><span class="sxs-lookup"><span data-stu-id="c0621-384">The following `TodoItems1Controller` provides endpoints for testing:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems1Controller.cs?name=snippet2)]

<span data-ttu-id="c0621-385">從已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的[測試頁](https://cors1.azurewebsites.net/test?number=1)測試前面的代碼。</span><span class="sxs-lookup"><span data-stu-id="c0621-385">Test the preceding code from the [test page](https://cors1.azurewebsites.net/test?number=1) of the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI).</span></span>

<span data-ttu-id="c0621-386">**刪除 [啟用 Cors]** 和**GET [啟用Cors]** 按鈕成功,因為`[EnableCors]`端點具有並回應預檢請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-386">The **Delete [EnableCors]** and **GET [EnableCors]** buttons succeed, because the endpoints have `[EnableCors]` and respond to preflight requests.</span></span> <span data-ttu-id="c0621-387">其他終結點失敗。</span><span class="sxs-lookup"><span data-stu-id="c0621-387">The other endpoints fails.</span></span> <span data-ttu-id="c0621-388">**GET**按鈕失敗,因為[JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js)傳送:</span><span class="sxs-lookup"><span data-stu-id="c0621-388">The **GET** button fails, because the [JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js) sends:</span></span>

```javascript
 headers: {
      "Content-Type": "x-custom-header"
 },
```

<span data-ttu-id="c0621-389">以下內容`TodoItems2Controller`提供類似的終結點,但包括用於回應 OPTIONS 請求的顯式代碼:</span><span class="sxs-lookup"><span data-stu-id="c0621-389">The following `TodoItems2Controller` provides similar endpoints, but includes explicit code to respond to OPTIONS requests:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet2)]

<span data-ttu-id="c0621-390">從已部署範例的[測試頁](https://cors1.azurewebsites.net/test?number=2)測試前面的代碼。</span><span class="sxs-lookup"><span data-stu-id="c0621-390">Test the preceding code from the [test page](https://cors1.azurewebsites.net/test?number=2) of the deployed sample.</span></span> <span data-ttu-id="c0621-391">在**控制器**下拉清單中,選擇 **「預檢**」,然後**設定控制器**。</span><span class="sxs-lookup"><span data-stu-id="c0621-391">In the **Controller** drop down list, select **Preflight** and then **Set Controller**.</span></span> <span data-ttu-id="c0621-392">對`TodoItems2Controller`終結點的所有 CORS 調用都成功。</span><span class="sxs-lookup"><span data-stu-id="c0621-392">All the CORS calls to the `TodoItems2Controller` endpoints succeed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0621-393">其他資源</span><span class="sxs-lookup"><span data-stu-id="c0621-393">Additional resources</span></span>

* [<span data-ttu-id="c0621-394">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="c0621-394">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="c0621-395">開始使用 IIS CORS 模組</span><span class="sxs-lookup"><span data-stu-id="c0621-395">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c0621-396">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c0621-396">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c0621-397">本文演示如何在ASP.NET核心應用中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-397">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="c0621-398">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-398">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="c0621-399">此限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="c0621-399">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="c0621-400">同源策略可防止惡意網站從其他網站讀取敏感數據。</span><span class="sxs-lookup"><span data-stu-id="c0621-400">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="c0621-401">有時,您可能希望允許其他網站向你的應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-401">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="c0621-402">有關詳細資訊,請參閱[Mozilla CORS 文章](https://developer.mozilla.org/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="c0621-402">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="c0621-403">[跨源資源分享](https://www.w3.org/TR/cors/)(CORS):</span><span class="sxs-lookup"><span data-stu-id="c0621-403">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="c0621-404">是允許伺服器放鬆同源策略的 W3C 標準。</span><span class="sxs-lookup"><span data-stu-id="c0621-404">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="c0621-405">**CORS 不是**安全功能,可放鬆安全性。</span><span class="sxs-lookup"><span data-stu-id="c0621-405">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="c0621-406">通過允許 CORS,API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="c0621-406">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="c0621-407">有關詳細資訊,請參閱[CORS 的工作原理](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="c0621-407">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="c0621-408">允許伺服器顯式允許某些跨源請求,同時拒絕其他請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-408">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="c0621-409">比早期的技術(如[JSONP](/dotnet/framework/wcf/samples/jsonp))更安全、更靈活。</span><span class="sxs-lookup"><span data-stu-id="c0621-409">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="c0621-410">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c0621-410">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="c0621-411">同一源</span><span class="sxs-lookup"><span data-stu-id="c0621-411">Same origin</span></span>

<span data-ttu-id="c0621-412">如果兩個 URL 具有相同的方案、主機和埠[(RFC 6454),](https://tools.ietf.org/html/rfc6454)則它們具有相同的源源。</span><span class="sxs-lookup"><span data-stu-id="c0621-412">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="c0621-413">這兩個網址 有相同的來源:</span><span class="sxs-lookup"><span data-stu-id="c0621-413">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="c0621-414">這些網址與前兩個網址具有不同的來源:</span><span class="sxs-lookup"><span data-stu-id="c0621-414">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="c0621-415">`https://example.net`&ndash;不同的網域</span><span class="sxs-lookup"><span data-stu-id="c0621-415">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="c0621-416">`https://www.example.com/foo.html`&ndash;不同的子域</span><span class="sxs-lookup"><span data-stu-id="c0621-416">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="c0621-417">`http://example.com/foo.html`&ndash;不同的機制</span><span class="sxs-lookup"><span data-stu-id="c0621-417">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="c0621-418">`https://example.com:9000/foo.html`&ndash;不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="c0621-418">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="c0621-419">在比較源時,Internet Explorer 不考慮埠。</span><span class="sxs-lookup"><span data-stu-id="c0621-419">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="c0621-420">包含命名政策與中間件的 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-420">CORS with named policy and middleware</span></span>

<span data-ttu-id="c0621-421">CORS 中間件處理跨源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-421">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="c0621-422">以下代碼支援具有指定來源的整個應用的 CORS:</span><span class="sxs-lookup"><span data-stu-id="c0621-422">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="c0621-423">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="c0621-423">The preceding code:</span></span>

* <span data-ttu-id="c0621-424">將策略名稱設為"myAllow\_特定原點"</span><span class="sxs-lookup"><span data-stu-id="c0621-424">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="c0621-425">策略名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="c0621-425">The policy name is arbitrary.</span></span>
* <span data-ttu-id="c0621-426">呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>式擴充方法,該方法啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-426">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="c0621-427">使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的調用。</span><span class="sxs-lookup"><span data-stu-id="c0621-427">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="c0621-428">lambda 取<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>一個物件。</span><span class="sxs-lookup"><span data-stu-id="c0621-428">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="c0621-429">[配置選項](#cors-policy-options)(`WithOrigins`如 )在本文的後面部分介紹。</span><span class="sxs-lookup"><span data-stu-id="c0621-429">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="c0621-430">方法<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>呼叫將 CORS 服務新增到應用的服務容器:</span><span class="sxs-lookup"><span data-stu-id="c0621-430">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="c0621-431">關於詳細資訊,請參考此文件中的[CORS 政策選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="c0621-431">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="c0621-432">該方法<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>可以連結方法,如以下代碼所示:</span><span class="sxs-lookup"><span data-stu-id="c0621-432">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="c0621-433">注意: 網址**不得**包含`/`尾隨斜槓 ( 。</span><span class="sxs-lookup"><span data-stu-id="c0621-433">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="c0621-434">如果 URL`/`終止與`false`,則比較返回,並且不返回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-434">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="c0621-435">以下代碼透過 CORS 的裝置將 CORS 政策應用於所有應用終結點:</span><span class="sxs-lookup"><span data-stu-id="c0621-435">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseHsts();
    }

    app.UseCors();

    app.UseHttpsRedirection();
    app.UseMvc();
}
```
<span data-ttu-id="c0621-436">注意:`UseCors`必須在`UseMvc`之前 呼叫 。</span><span class="sxs-lookup"><span data-stu-id="c0621-436">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="c0621-437">請參閱[在 Razor 頁面、控制器和操作方法中啟用 CORS,](#ecors)以便在頁面/控制器/操作級別應用 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-437">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="c0621-438">有關測試代碼的說明,請參閱[測試 CORS,](#test)這些說明與前面的代碼類似。</span><span class="sxs-lookup"><span data-stu-id="c0621-438">See [Test CORS](#test) for instructions on testing code similar to the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="c0621-439">使用屬性啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-439">Enable CORS with attributes</span></span>

<span data-ttu-id="c0621-440">[啟用 Cors&rbrack;屬性提供了全域應用 CORS 的替代方法。 &lbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="c0621-440">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="c0621-441">該`[EnableCors]`屬性為選定的端點啟用 CORS,而不是所有端點。</span><span class="sxs-lookup"><span data-stu-id="c0621-441">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="c0621-442">指定`[EnableCors]`預設策略和`[EnableCors("{Policy String}")]`指定策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-442">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="c0621-443">該`[EnableCors]`屬性可應用於:</span><span class="sxs-lookup"><span data-stu-id="c0621-443">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="c0621-444">剃刀頁面`PageModel`</span><span class="sxs-lookup"><span data-stu-id="c0621-444">Razor Page `PageModel`</span></span>
* <span data-ttu-id="c0621-445">控制器</span><span class="sxs-lookup"><span data-stu-id="c0621-445">Controller</span></span>
* <span data-ttu-id="c0621-446">控制器操作方法</span><span class="sxs-lookup"><span data-stu-id="c0621-446">Controller action method</span></span>

<span data-ttu-id="c0621-447">您可以使用`[EnableCors]`屬性對控制器/頁面模型/操作應用不同的策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-447">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="c0621-448">當該`[EnableCors]`屬性應用於控制器/頁面模型/操作方法,並在中間件中啟用 CORS 時,將應用***這兩個***策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-448">When the `[EnableCors]` attribute is applied to a controllers/page model/action method, and CORS is enabled in middleware, ***both*** policies are applied.</span></span> <span data-ttu-id="c0621-449">我們建議***不要***合併策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-449">We recommend ***not*** combining policies.</span></span> <span data-ttu-id="c0621-450">使用屬性`[EnableCors]`或中間件,\***而不是兩者**。</span><span class="sxs-lookup"><span data-stu-id="c0621-450">Use the `[EnableCors]` attribute or middleware, \***not both**.</span></span> <span data-ttu-id="c0621-451">使用`[EnableCors]`時 **,不要**定義預設策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-451">When using `[EnableCors]`, do **not** define a default policy.</span></span>

<span data-ttu-id="c0621-452">以下代碼對每種方法應用不同的原則:</span><span class="sxs-lookup"><span data-stu-id="c0621-452">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="c0621-453">以下代碼建立 CORS 預設政策與名為`"AnotherPolicy"`的策略 :</span><span class="sxs-lookup"><span data-stu-id="c0621-453">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="c0621-454">關閉 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-454">Disable CORS</span></span>

<span data-ttu-id="c0621-455">[禁用 Cors&rbrack;屬性禁用控制器/頁面模型/操作的 CORS。 &lbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="c0621-455">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="c0621-456">CORS 政策選項</span><span class="sxs-lookup"><span data-stu-id="c0621-456">CORS policy options</span></span>

<span data-ttu-id="c0621-457">本節介紹可在 CORS 策略中設定的各種選項:</span><span class="sxs-lookup"><span data-stu-id="c0621-457">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="c0621-458">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="c0621-458">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="c0621-459">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="c0621-459">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="c0621-460">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-460">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="c0621-461">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-461">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="c0621-462">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="c0621-462">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="c0621-463">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="c0621-463">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="c0621-464"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>在`Startup.ConfigureServices`中 呼叫 。</span><span class="sxs-lookup"><span data-stu-id="c0621-464"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="c0621-465">對於某些選項,首先閱讀[「CORS 的工作原理」](#how-cors)部分可能會有所説明。</span><span class="sxs-lookup"><span data-stu-id="c0621-465">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="c0621-466">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="c0621-466">Set the allowed origins</span></span>

<span data-ttu-id="c0621-467"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>&ndash;允許使用任何方案 (`http``https`或 ) 從所有來源請求 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-467"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="c0621-468">`AllowAnyOrigin`不安全,因為*任何網站*都可以向應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-468">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="c0621-469">指定`AllowAnyOrigin``AllowCredentials`和 是不安全的配置,可能會導致跨網站請求偽造。</span><span class="sxs-lookup"><span data-stu-id="c0621-469">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="c0621-470">對於安全應用,如果客戶端必須授權自己訪問伺服器資源,請指定確切的來源清單。</span><span class="sxs-lookup"><span data-stu-id="c0621-470">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="c0621-471">`AllowAnyOrigin`影響預檢請求和`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-471">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="c0621-472">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-472">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="c0621-473"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash;將<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略的屬性設為一個函數,允許源在評估是否允許原點時匹配配置的通配符域。</span><span class="sxs-lookup"><span data-stu-id="c0621-473"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="c0621-474">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="c0621-474">Set the allowed HTTP methods</span></span>

<span data-ttu-id="c0621-475"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-475"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="c0621-476">允許任何 HTTP 方法:</span><span class="sxs-lookup"><span data-stu-id="c0621-476">Allows any HTTP method:</span></span>
* <span data-ttu-id="c0621-477">影響印前請求和`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-477">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="c0621-478">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-478">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="c0621-479">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-479">Set the allowed request headers</span></span>

<span data-ttu-id="c0621-480">要允許在 CORS 請求中傳送特定標頭,請呼叫作者*要求標頭*,呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭:</span><span class="sxs-lookup"><span data-stu-id="c0621-480">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="c0621-481">要允許所有作者要求標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-481">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="c0621-482">此設置會影響預檢請求和`Access-Control-Request-Headers`標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-482">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="c0621-483">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="c0621-483">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="c0621-484">無論 CorsPolicy.header 中配置`Access-Control-Request-Headers`的值 如何,CORS 中間件始終允許發送 中的四個標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-484">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="c0621-485">這個標頭清單包括:</span><span class="sxs-lookup"><span data-stu-id="c0621-485">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="c0621-486">例如,請考慮配置如下的應用:</span><span class="sxs-lookup"><span data-stu-id="c0621-486">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="c0621-487">CORS 中間件使用以下請求標頭成功回應預檢請求,`Content-Language`因為 始終被列入白名單:</span><span class="sxs-lookup"><span data-stu-id="c0621-487">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="c0621-488">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="c0621-488">Set the exposed response headers</span></span>

<span data-ttu-id="c0621-489">默認情況下,瀏覽器不會向應用公開所有響應標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-489">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="c0621-490">有關詳細資訊,請參閱[W3C 跨源資源分享(術語):簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="c0621-490">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="c0621-491">預設情況下可用的回應標頭是:</span><span class="sxs-lookup"><span data-stu-id="c0621-491">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="c0621-492">CORS 規範呼叫這些標頭*為簡單回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="c0621-492">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="c0621-493">要使其他標頭可供應用使用,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-493">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="c0621-494">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="c0621-494">Credentials in cross-origin requests</span></span>

<span data-ttu-id="c0621-495">憑據需要在 CORS 請求中特殊處理。</span><span class="sxs-lookup"><span data-stu-id="c0621-495">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="c0621-496">默認情況下,瀏覽器不會發送具有跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-496">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="c0621-497">認證包括 Cookie 和 HTTP 驗證方案。</span><span class="sxs-lookup"><span data-stu-id="c0621-497">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="c0621-498">要使用跨源要求傳送認證,客戶端必須設定`XMLHttpRequest.withCredentials``true`為 。</span><span class="sxs-lookup"><span data-stu-id="c0621-498">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="c0621-499">直接`XMLHttpRequest`使用:</span><span class="sxs-lookup"><span data-stu-id="c0621-499">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="c0621-500">使用 jQuery:</span><span class="sxs-lookup"><span data-stu-id="c0621-500">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="c0621-501">使用[擷取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="c0621-501">Using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="c0621-502">伺服器必須允許憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-502">The server must allow the credentials.</span></span> <span data-ttu-id="c0621-503">要允許跨源認證,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-503">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="c0621-504">HTTP 回應包括`Access-Control-Allow-Credentials`一個標頭,該標頭告訴瀏覽器伺服器允許跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-504">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="c0621-505">如果瀏覽器發送憑據,但回應不包含有效的`Access-Control-Allow-Credentials`標頭,則瀏覽器不會向應用公開回應,並且跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="c0621-505">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="c0621-506">允許跨源憑據存在安全風險。</span><span class="sxs-lookup"><span data-stu-id="c0621-506">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="c0621-507">另一個域中的網站可以在使用者不知情的情況下代表使用者向應用發送登錄使用者的憑據。</span><span class="sxs-lookup"><span data-stu-id="c0621-507">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="c0621-508">CORS 規範還規定,如果標頭存在`"*"``Access-Control-Allow-Credentials`, 則將原點設置為(所有源)無效。</span><span class="sxs-lookup"><span data-stu-id="c0621-508">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="c0621-509">預先要求</span><span class="sxs-lookup"><span data-stu-id="c0621-509">Preflight requests</span></span>

<span data-ttu-id="c0621-510">對於某些 CORS 請求,瀏覽器在發出實際請求之前發送其他請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-510">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="c0621-511">此要求為*預先要求*。</span><span class="sxs-lookup"><span data-stu-id="c0621-511">This request is called a *preflight request*.</span></span> <span data-ttu-id="c0621-512">如果以下條件為 true,瀏覽器可以跳過預檢請求:</span><span class="sxs-lookup"><span data-stu-id="c0621-512">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="c0621-513">請求方法是 GET、頭或 POST。</span><span class="sxs-lookup"><span data-stu-id="c0621-513">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="c0621-514">套用不設定`Accept`要求 標頭以外`Accept-Language``Content-Language``Content-Type`的`Last-Event-ID`、、、、 或 。</span><span class="sxs-lookup"><span data-stu-id="c0621-514">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="c0621-515">標頭`Content-Type`(如果已設定)具有以下值之一:</span><span class="sxs-lookup"><span data-stu-id="c0621-515">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="c0621-516">為用戶端請求設置的請求標頭規則適用於應用通過調用`setRequestHeader`物件設置`XMLHttpRequest`的 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-516">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="c0621-517">CORS 規範呼叫這些標頭*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="c0621-517">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="c0621-518">這個規則不適用於瀏覽器可以設定的標頭,例如`User-Agent` `Host` `Content-Length` 。</span><span class="sxs-lookup"><span data-stu-id="c0621-518">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="c0621-519">以下是預檢要求的範例:</span><span class="sxs-lookup"><span data-stu-id="c0621-519">The following is an example of a preflight request:</span></span>

```
OPTIONS https://myservice.azurewebsites.net/api/test HTTP/1.1
Accept: */*
Origin: https://myclient.azurewebsites.net
Access-Control-Request-Method: PUT
Access-Control-Request-Headers: accept, x-my-custom-header
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
Content-Length: 0
```

<span data-ttu-id="c0621-520">飛行前請求使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="c0621-520">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="c0621-521">它包括兩個特殊的標頭:</span><span class="sxs-lookup"><span data-stu-id="c0621-521">It includes two special headers:</span></span>

* <span data-ttu-id="c0621-522">`Access-Control-Request-Method`:將用於實際請求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="c0621-522">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="c0621-523">`Access-Control-Request-Headers`:應用在實際請求上設置的請求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="c0621-523">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="c0621-524">如前所述,這不包括瀏覽器設定的標頭,如`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="c0621-524">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<!-- I think this needs to be removed, was put here accidently -->

<span data-ttu-id="c0621-525">當使用適當的策略啟用 CORS 時,ASP.NET核心通常會自動回應 CORS 預檢請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-525">When CORS is enabled with the appropriate policy, ASP.NET Core generally automatically responds to CORS preflight requests.</span></span> <span data-ttu-id="c0621-526">[有關預檢要求,請參閱 [HttpOptions] 屬性](#pro)。</span><span class="sxs-lookup"><span data-stu-id="c0621-526">See [[HttpOptions] attribute for preflight requests](#pro).</span></span>

<span data-ttu-id="c0621-527">CORS 預檢請求可能包含一`Access-Control-Request-Headers`個 標頭,該標頭向伺服器指示與實際請求一起發送的標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-527">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="c0621-528">要允許特定標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-528">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="c0621-529">要允許所有作者要求標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="c0621-529">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="c0621-530">流覽器`Access-Control-Request-Headers`的設置 方式並不完全一致。</span><span class="sxs-lookup"><span data-stu-id="c0621-530">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="c0621-531">如果將標頭設置為 (或`"*"`<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>使用 ) 以外的任何內容,則`Accept`至少`Content-Type``Origin`應包括和 ,以及要支援的任何自定義標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-531">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="c0621-532">以下是對預檢請求的範例回應(假設伺服器允許該請求):</span><span class="sxs-lookup"><span data-stu-id="c0621-532">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Length: 0
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Access-Control-Allow-Headers: x-my-custom-header
Access-Control-Allow-Methods: PUT
Date: Wed, 20 May 2015 06:33:22 GMT
```

<span data-ttu-id="c0621-533">回應包括列出`Access-Control-Allow-Methods`允許的方法的標頭,並選擇一`Access-Control-Allow-Headers`個 標頭,其中列出了允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-533">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="c0621-534">如果預檢請求成功,瀏覽器將發送實際請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-534">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="c0621-535">如果預檢請求被拒絕,應用將返回*200 OK*回應,但不會將 CORS 標頭髮送回。</span><span class="sxs-lookup"><span data-stu-id="c0621-535">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="c0621-536">因此,瀏覽器不會嘗試跨源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-536">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="c0621-537">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="c0621-537">Set the preflight expiration time</span></span>

<span data-ttu-id="c0621-538">標頭`Access-Control-Max-Age`指定對預檢請求的回應可以緩存多長時間。</span><span class="sxs-lookup"><span data-stu-id="c0621-538">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="c0621-539">要設定這個標頭,請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="c0621-539">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="c0621-540">CORS 的工作原理</span><span class="sxs-lookup"><span data-stu-id="c0621-540">How CORS works</span></span>

<span data-ttu-id="c0621-541">本節介紹在 HTTP 消息級別發生的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)請求中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="c0621-541">This section describes what happens in a [CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="c0621-542">CORS**不是**一個安全功能。</span><span class="sxs-lookup"><span data-stu-id="c0621-542">CORS is **not** a security feature.</span></span> <span data-ttu-id="c0621-543">CORS 是一種 W3C 標準,允許伺服器放鬆同源策略。</span><span class="sxs-lookup"><span data-stu-id="c0621-543">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="c0621-544">例如,惡意參與者可以使用[防止跨網站腳本 (XSS)](xref:security/cross-site-scripting)攻擊您的網站,並對其啟用 CORS 的網站執行跨網站請求以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="c0621-544">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="c0621-545">通過允許 CORS,您的 API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="c0621-545">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="c0621-546">由用戶端(瀏覽器)來強制實施 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-546">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="c0621-547">伺服器執行請求並返回回應,返回錯誤的是用戶端並阻止回應。</span><span class="sxs-lookup"><span data-stu-id="c0621-547">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="c0621-548">例如,以下任何工具將顯示伺服器回應:</span><span class="sxs-lookup"><span data-stu-id="c0621-548">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="c0621-549">Fiddler</span><span class="sxs-lookup"><span data-stu-id="c0621-549">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="c0621-550">Postman</span><span class="sxs-lookup"><span data-stu-id="c0621-550">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="c0621-551">.NET HTTPClient</span><span class="sxs-lookup"><span data-stu-id="c0621-551">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="c0621-552">通過在位址列中輸入 URL 來訪問 Web 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c0621-552">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="c0621-553">這是伺服器允許瀏覽器執行跨源[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)請求的一種方式,否則將禁止這樣做。</span><span class="sxs-lookup"><span data-stu-id="c0621-553">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="c0621-554">瀏覽器(沒有 CORS)不能執行交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-554">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="c0621-555">在 CORS 之前[,JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)曾用於規避此限制。</span><span class="sxs-lookup"><span data-stu-id="c0621-555">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="c0621-556">JSONP 不使用 XHR,`<script>`它使用 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="c0621-556">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="c0621-557">允許跨源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="c0621-557">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="c0621-558">[CORS 規範](https://www.w3.org/TR/cors/)引入了幾個支援跨源請求的新 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-558">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="c0621-559">如果瀏覽器支援 CORS,它將自動為跨源請求設置這些標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-559">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="c0621-560">啟用 CORS 不需要自訂 JavaScript 代碼。</span><span class="sxs-lookup"><span data-stu-id="c0621-560">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="c0621-561">下面是跨源請求的示例。</span><span class="sxs-lookup"><span data-stu-id="c0621-561">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="c0621-562">標頭`Origin`提供發出請求的網站的域。</span><span class="sxs-lookup"><span data-stu-id="c0621-562">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="c0621-563">標頭`Origin`是必需的,並且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="c0621-563">The `Origin` header is required and must be different from the host.</span></span>

```
GET https://myservice.azurewebsites.net/api/test HTTP/1.1
Referer: https://myclient.azurewebsites.net/
Accept: */*
Accept-Language: en-US
Origin: https://myclient.azurewebsites.net
Accept-Encoding: gzip, deflate
User-Agent: Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)
Host: myservice.azurewebsites.net
```

<span data-ttu-id="c0621-564">如果伺服器允許請求,它將在`Access-Control-Allow-Origin`回應中設置標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-564">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="c0621-565">此標頭的值與請求中的`Origin`標頭匹配,或者是通配符`"*"`值 ,這意味著允許任何源:</span><span class="sxs-lookup"><span data-stu-id="c0621-565">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

```
HTTP/1.1 200 OK
Cache-Control: no-cache
Pragma: no-cache
Content-Type: text/plain; charset=utf-8
Access-Control-Allow-Origin: https://myclient.azurewebsites.net
Date: Wed, 20 May 2015 06:27:30 GMT
Content-Length: 12

Test message
```

<span data-ttu-id="c0621-566">如果回應不包括標頭,`Access-Control-Allow-Origin`則跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="c0621-566">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="c0621-567">具體來說,瀏覽器不允許請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-567">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="c0621-568">即使伺服器返回成功的回應,瀏覽器也不會使回應對用戶端應用可用。</span><span class="sxs-lookup"><span data-stu-id="c0621-568">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="c0621-569">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-569">Test CORS</span></span>

<span data-ttu-id="c0621-570">測試 CORS:</span><span class="sxs-lookup"><span data-stu-id="c0621-570">To test CORS:</span></span>

1. <span data-ttu-id="c0621-571">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="c0621-571">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="c0621-572">或您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="c0621-572">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="c0621-573">使用本文件中的一種方法啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="c0621-573">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="c0621-574">例如：</span><span class="sxs-lookup"><span data-stu-id="c0621-574">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="c0621-575">`WithOrigins("https://localhost:<port>");`應僅用於測試類似於[下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)的範例應用。</span><span class="sxs-lookup"><span data-stu-id="c0621-575">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="c0621-576">創建 Web 應用專案(剃鬚頁面或 MVC)。</span><span class="sxs-lookup"><span data-stu-id="c0621-576">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="c0621-577">該示例使用剃刀頁。</span><span class="sxs-lookup"><span data-stu-id="c0621-577">The sample uses Razor Pages.</span></span> <span data-ttu-id="c0621-578">您可以在與 API 專案相同的解決方案中創建 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="c0621-578">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="c0621-579">將以下突顯的代碼加入*到 Index.cshtml*檔:</span><span class="sxs-lookup"><span data-stu-id="c0621-579">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="c0621-580">在前面的代碼中,替換為`url: 'https://<web app>.azurewebsites.net/api/values/1',`已部署應用的 URL。</span><span class="sxs-lookup"><span data-stu-id="c0621-580">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="c0621-581">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="c0621-581">Deploy the API project.</span></span> <span data-ttu-id="c0621-582">例如,[部署到 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="c0621-582">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="c0621-583">從桌面運行剃刀頁面或 MVC 應用,然後單擊 **「測試**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="c0621-583">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="c0621-584">使用 F12 工具檢視錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c0621-584">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="c0621-585">從`WithOrigins`中刪除本地主機源並部署應用。</span><span class="sxs-lookup"><span data-stu-id="c0621-585">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="c0621-586">或者,使用其他埠運行客戶端應用。</span><span class="sxs-lookup"><span data-stu-id="c0621-586">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="c0621-587">例如,從可視化工作室運行。</span><span class="sxs-lookup"><span data-stu-id="c0621-587">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="c0621-588">使用用戶端應用進行測試。</span><span class="sxs-lookup"><span data-stu-id="c0621-588">Test with the client app.</span></span> <span data-ttu-id="c0621-589">CORS 失敗傳回錯誤,但錯誤消息對 JavaScript 不可用。</span><span class="sxs-lookup"><span data-stu-id="c0621-589">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="c0621-590">使用 F12 工具中的主控台選項卡檢視錯誤。</span><span class="sxs-lookup"><span data-stu-id="c0621-590">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="c0621-591">根據瀏覽器的不同,您會收到類似於以下內容的錯誤(在 F12 工具控制台中):</span><span class="sxs-lookup"><span data-stu-id="c0621-591">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="c0621-592">使用微軟邊緣:</span><span class="sxs-lookup"><span data-stu-id="c0621-592">Using Microsoft Edge:</span></span>

     <span data-ttu-id="c0621-593">**SEC7120: [CORS]`https://localhost:44375`在`https://localhost:44375`「訪問 -控制-允許-原點」回應標頭中找不到跨源資源`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="c0621-593">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="c0621-594">使用鉻:</span><span class="sxs-lookup"><span data-stu-id="c0621-594">Using Chrome:</span></span>

     <span data-ttu-id="c0621-595">**CORS 策略阻止了`https://webapi.azurewebsites.net/api/values/1`從`https://localhost:44375`源 處對 XMLHTTPRequest 的訪問:請求的資源上不存在"訪問-控制-允許源"標頭。**</span><span class="sxs-lookup"><span data-stu-id="c0621-595">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="c0621-596">支援 CORS 的終結點可以使用工具進行測試,例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)。</span><span class="sxs-lookup"><span data-stu-id="c0621-596">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="c0621-597">使用工具時,`Origin`標頭指定的請求的來源必須不同於接收請求的主機。</span><span class="sxs-lookup"><span data-stu-id="c0621-597">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="c0621-598">如果要求不是基於標頭的值*的跨源*: `Origin`</span><span class="sxs-lookup"><span data-stu-id="c0621-598">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="c0621-599">CORS 中間件無需處理請求。</span><span class="sxs-lookup"><span data-stu-id="c0621-599">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="c0621-600">回應中未返回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="c0621-600">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="c0621-601">IIS 的 CORS</span><span class="sxs-lookup"><span data-stu-id="c0621-601">CORS in IIS</span></span>

<span data-ttu-id="c0621-602">部署到IIS時,如果伺服器未配置為允許匿名訪問,則CORS必須在Windows身份驗證之前運行。</span><span class="sxs-lookup"><span data-stu-id="c0621-602">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="c0621-603">為了支援此專案,需要為應用程式安裝和設定[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="c0621-603">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0621-604">其他資源</span><span class="sxs-lookup"><span data-stu-id="c0621-604">Additional resources</span></span>

* [<span data-ttu-id="c0621-605">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="c0621-605">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="c0621-606">開始使用 IIS CORS 模組</span><span class="sxs-lookup"><span data-stu-id="c0621-606">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end
