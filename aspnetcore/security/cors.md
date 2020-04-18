---
title: 在 ASP.NET 核心中開啟跨源要求 (CORS)
author: rick-anderson
description: 瞭解 CORS 如何作為在 ASP.NET 酷應用中允許或拒絕跨源請求的標準。
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
uid: security/cors
ms.openlocfilehash: 56a339d9018f619af38aecc6f4c2ff40c3c43d2f
ms.sourcegitcommit: 3d07e21868dafc503530ecae2cfa18a7490b58a6
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2020
ms.locfileid: "81642690"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="15899-103">在 ASP.NET 核心中開啟跨源要求 (CORS)</span><span class="sxs-lookup"><span data-stu-id="15899-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="15899-104">由[里克·安德森](https://twitter.com/RickAndMSFT)和[柯克·拉金](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="15899-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="15899-105">本文演示如何在ASP.NET核心應用中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="15899-106">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="15899-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="15899-107">此限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="15899-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="15899-108">同源策略可防止惡意網站從其他網站讀取敏感數據。</span><span class="sxs-lookup"><span data-stu-id="15899-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="15899-109">有時,您可能希望允許其他網站向你的應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-109">Sometimes, you might want to allow other sites to make cross-origin requests to your app.</span></span> <span data-ttu-id="15899-110">有關詳細資訊,請參閱[Mozilla CORS 文章](https://developer.mozilla.org/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="15899-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="15899-111">[跨源資源分享](https://www.w3.org/TR/cors/)(CORS):</span><span class="sxs-lookup"><span data-stu-id="15899-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="15899-112">是允許伺服器放鬆同源策略的 W3C 標準。</span><span class="sxs-lookup"><span data-stu-id="15899-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="15899-113">**CORS 不是**安全功能,可放鬆安全性。</span><span class="sxs-lookup"><span data-stu-id="15899-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="15899-114">通過允許 CORS,API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="15899-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="15899-115">有關詳細資訊,請參閱[CORS 的工作原理](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="15899-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="15899-116">允許伺服器顯式允許某些跨源請求,同時拒絕其他請求。</span><span class="sxs-lookup"><span data-stu-id="15899-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="15899-117">比早期的技術(如[JSONP](/dotnet/framework/wcf/samples/jsonp))更安全、更靈活。</span><span class="sxs-lookup"><span data-stu-id="15899-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="15899-118">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15899-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="15899-119">同一源</span><span class="sxs-lookup"><span data-stu-id="15899-119">Same origin</span></span>

<span data-ttu-id="15899-120">如果兩個 URL 具有相同的方案、主機和埠[(RFC 6454),](https://tools.ietf.org/html/rfc6454)則它們具有相同的源源。</span><span class="sxs-lookup"><span data-stu-id="15899-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="15899-121">這兩個網址 有相同的來源:</span><span class="sxs-lookup"><span data-stu-id="15899-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="15899-122">這些網址與前兩個網址具有不同的來源:</span><span class="sxs-lookup"><span data-stu-id="15899-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="15899-123">`https://example.net`&ndash;不同的網域</span><span class="sxs-lookup"><span data-stu-id="15899-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="15899-124">`https://www.example.com/foo.html`&ndash;不同的子域</span><span class="sxs-lookup"><span data-stu-id="15899-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="15899-125">`http://example.com/foo.html`&ndash;不同的機制</span><span class="sxs-lookup"><span data-stu-id="15899-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="15899-126">`https://example.com:9000/foo.html`&ndash;不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="15899-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

## <a name="enable-cors"></a><span data-ttu-id="15899-127">啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-127">Enable CORS</span></span>

<span data-ttu-id="15899-128">有三種方法可以啟用 CORS:</span><span class="sxs-lookup"><span data-stu-id="15899-128">There are three ways to enable CORS:</span></span>

* <span data-ttu-id="15899-129">在中間件中使用[命名政策](#np)或[預設政策](#dp)。</span><span class="sxs-lookup"><span data-stu-id="15899-129">In middleware using a [named policy](#np) or [default policy](#dp).</span></span>
* <span data-ttu-id="15899-130">使用[終結點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="15899-130">Using [endpoint routing](#ecors).</span></span>
* <span data-ttu-id="15899-131">使用[[啟用 Cors]](#attr)屬性。</span><span class="sxs-lookup"><span data-stu-id="15899-131">With the [[EnableCors]](#attr) attribute.</span></span>

<span data-ttu-id="15899-132">將[[EnableCors]](#attr)屬性與命名策略一起使用,可在限制支援 CORS 的終結點時提供最佳控制。</span><span class="sxs-lookup"><span data-stu-id="15899-132">Using the [[EnableCors]](#attr) attribute with a named policy provides the finest control in limiting endpoints that support CORS.</span></span>

<span data-ttu-id="15899-133">每種方法都詳見以下各節。</span><span class="sxs-lookup"><span data-stu-id="15899-133">Each approach is detailed in the following sections.</span></span>

<a name="np"></a>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="15899-134">包含命名政策與中間件的 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-134">CORS with named policy and middleware</span></span>

<span data-ttu-id="15899-135">CORS 中間件處理跨源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-135">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="15899-136">以下代碼將 CORS 政策應用於具有指定來源的所有應用終結點:</span><span class="sxs-lookup"><span data-stu-id="15899-136">The following code applies a CORS policy to all the app's endpoints with the specified origins:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=3,9,31)]

<span data-ttu-id="15899-137">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="15899-137">The preceding code:</span></span>

* <span data-ttu-id="15899-138">將策略名稱設定到`_myAllowSpecificOrigins`。</span><span class="sxs-lookup"><span data-stu-id="15899-138">Sets the policy name to `_myAllowSpecificOrigins`.</span></span> <span data-ttu-id="15899-139">策略名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="15899-139">The policy name is arbitrary.</span></span>
* <span data-ttu-id="15899-140">調用<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>擴充方法`_myAllowSpecificOrigins`並指定 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="15899-140">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method and specifies the  `_myAllowSpecificOrigins` CORS policy.</span></span> <span data-ttu-id="15899-141">`UseCors`添加 CORS 中間件。</span><span class="sxs-lookup"><span data-stu-id="15899-141">`UseCors` adds the CORS middleware.</span></span> <span data-ttu-id="15899-142">呼叫`UseCors`後`UseRouting`必須發出`UseAuthorization`,但之前 。</span><span class="sxs-lookup"><span data-stu-id="15899-142">The call to `UseCors` must be placed after `UseRouting`, but before `UseAuthorization`.</span></span> <span data-ttu-id="15899-143">有關詳細資訊,請參閱[中間件訂單](xref:fundamentals/middleware/index#middleware-order)。</span><span class="sxs-lookup"><span data-stu-id="15899-143">For more information, see [Middleware order](xref:fundamentals/middleware/index#middleware-order).</span></span>
* <span data-ttu-id="15899-144">使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的調用。</span><span class="sxs-lookup"><span data-stu-id="15899-144">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="15899-145">lambda 取<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>一個物件。</span><span class="sxs-lookup"><span data-stu-id="15899-145">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="15899-146">[配置選項](#cors-policy-options)(`WithOrigins`如 )在本文的後面部分介紹。</span><span class="sxs-lookup"><span data-stu-id="15899-146">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>
* <span data-ttu-id="15899-147">為所有控制器`_myAllowSpecificOrigins`終結點啟用 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="15899-147">Enables the `_myAllowSpecificOrigins` CORS policy for all controller endpoints.</span></span> <span data-ttu-id="15899-148">請參閱[終結點路由](#ecors),將 CORS 策略應用於特定終結點。</span><span class="sxs-lookup"><span data-stu-id="15899-148">See [endpoint routing](#ecors) to apply a CORS policy to specific endpoints.</span></span>

<span data-ttu-id="15899-149">使用端點路由時,***必須***將 CORS 中間件配置為`UseRouting`在`UseEndpoints`調用和 之間執行。</span><span class="sxs-lookup"><span data-stu-id="15899-149">With endpoint routing, the CORS middleware ***must*** be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span>

<span data-ttu-id="15899-150">有關測試代碼的說明,請參閱[測試 CORS,](#testc)這些說明與前面的代碼類似。</span><span class="sxs-lookup"><span data-stu-id="15899-150">See [Test CORS](#testc) for instructions on testing code similar to the preceding code.</span></span>

<span data-ttu-id="15899-151">方法<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>呼叫將 CORS 服務新增到應用的服務容器:</span><span class="sxs-lookup"><span data-stu-id="15899-151">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="15899-152">關於詳細資訊,請參考此文件中的[CORS 政策選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="15899-152">For more information, see [CORS policy options](#cpo) in this document.</span></span>

<span data-ttu-id="15899-153">這些方法<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>可以連結,如以下代碼所示:</span><span class="sxs-lookup"><span data-stu-id="15899-153">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> methods can be chained, as shown in the following code:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup2.cs?name=snippet)]

<span data-ttu-id="15899-154">注意: 指定的網址**不能**包含尾`/`隨斜槓 ( 。</span><span class="sxs-lookup"><span data-stu-id="15899-154">Note: The specified URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="15899-155">如果 URL`/`終止與`false`,則比較返回,並且不返回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-155">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="dp"></a>

### <a name="cors-with-default-policy-and-middleware"></a><span data-ttu-id="15899-156">具有預設政策和中間件的 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-156">CORS with default policy and middleware</span></span>

<span data-ttu-id="15899-157">以下突顯的代碼啟用預設 CORS 政策:</span><span class="sxs-lookup"><span data-stu-id="15899-157">The following highlighted code enables the default CORS policy:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupDefaultPolicy.cs?name=snippet2&highlight=7,29)]

<span data-ttu-id="15899-158">前面的代碼將預設的 CORS 策略應用於所有控制器終結點。</span><span class="sxs-lookup"><span data-stu-id="15899-158">The preceding code applies the default CORS policy to all controller endpoints.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="15899-159">使用端點路由啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="15899-159">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="15899-160">使用`RequireCors`目前使用每個的終結點的 CORS***不支援***[自動預取要求](#apf)。</span><span class="sxs-lookup"><span data-stu-id="15899-160">Enabling CORS on a per-endpoint basis using `RequireCors` currently does ***not*** support [automatic preflight requests](#apf).</span></span> <span data-ttu-id="15899-161">有關詳細資訊,請參閱此[GitHub 問題和](https://github.com/dotnet/aspnetcore/issues/20709)[使用終結點路由和 [HttpOptions] 測試 CORS。](#tcer)</span><span class="sxs-lookup"><span data-stu-id="15899-161">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/20709) and [Test CORS with endpoint routing and [HttpOptions]](#tcer).</span></span>

<span data-ttu-id="15899-162">使用端點路由,可以使用擴充<xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*>方法集,按終結點啟用 CORS:</span><span class="sxs-lookup"><span data-stu-id="15899-162">With endpoint routing, CORS can be enabled on a per-endpoint basis using the <xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*> set of extension methods:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPt.cs?name=snippet2&highlight=3,7-15,32,40,43)]

<span data-ttu-id="15899-163">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="15899-163">In the preceding code:</span></span>

* <span data-ttu-id="15899-164">`app.UseCors`啟用 CORS 中間件。</span><span class="sxs-lookup"><span data-stu-id="15899-164">`app.UseCors` enables the CORS middleware.</span></span> <span data-ttu-id="15899-165">由於尚未設定預設政策,`app.UseCors()`因此單獨無法啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-165">Because a default policy hasn't been configured, `app.UseCors()` alone doesn't enable CORS.</span></span>
* <span data-ttu-id="15899-166">`/echo`和控制器終結點允許使用指定的策略進行交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-166">The `/echo` and controller endpoints allow cross-origin requests using the specified policy.</span></span>
* <span data-ttu-id="15899-167">`/echo2`和 Razor Pages 終結點***不允許***跨源請求,因為未指定預設策略。</span><span class="sxs-lookup"><span data-stu-id="15899-167">The `/echo2` and Razor Pages endpoints do ***not*** allow cross-origin requests because no default policy was specified.</span></span>

<span data-ttu-id="15899-168">[[禁用 Cors]](#dc)屬性***不會***停用由 終結`RequireCors`點路由啟用的 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-168">The [[DisableCors]](#dc) attribute does ***not***  disable CORS that has been enabled by endpoint routing with `RequireCors`.</span></span>

<span data-ttu-id="15899-169">有關測試代碼的指令,請參閱[使用端點路由和 [HttpOptions] 測試 CORS。](#tcer)</span><span class="sxs-lookup"><span data-stu-id="15899-169">See [Test CORS with endpoint routing and [HttpOptions]](#tcer) for instructions on testing code similar to the preceding.</span></span>

<a name="attr"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="15899-170">使用屬性啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-170">Enable CORS with attributes</span></span>

<span data-ttu-id="15899-171">使用[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性啟用 CORS 並將命名策略應用於僅對需要 CORS 的終結點提供最佳控制。</span><span class="sxs-lookup"><span data-stu-id="15899-171">Enabling CORS with the [[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute and applying a named policy to only those endpoints that require CORS provides the finest control.</span></span>

<span data-ttu-id="15899-172">[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供了全域應用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="15899-172">The [[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="15899-173">該`[EnableCors]`屬性為選擇的終結點啟用 CORS,而不是所有終結點:</span><span class="sxs-lookup"><span data-stu-id="15899-173">The `[EnableCors]` attribute enables CORS for selected endpoints, rather than all endpoints:</span></span>

* <span data-ttu-id="15899-174">`[EnableCors]`指定預設策略。</span><span class="sxs-lookup"><span data-stu-id="15899-174">`[EnableCors]` specifies the default policy.</span></span>
* <span data-ttu-id="15899-175">`[EnableCors("{Policy String}")]`指定命名策略。</span><span class="sxs-lookup"><span data-stu-id="15899-175">`[EnableCors("{Policy String}")]` specifies a named policy.</span></span>

<span data-ttu-id="15899-176">該`[EnableCors]`屬性可應用於:</span><span class="sxs-lookup"><span data-stu-id="15899-176">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="15899-177">剃刀頁面`PageModel`</span><span class="sxs-lookup"><span data-stu-id="15899-177">Razor Page `PageModel`</span></span>
* <span data-ttu-id="15899-178">控制器</span><span class="sxs-lookup"><span data-stu-id="15899-178">Controller</span></span>
* <span data-ttu-id="15899-179">控制器操作方法</span><span class="sxs-lookup"><span data-stu-id="15899-179">Controller action method</span></span>

<span data-ttu-id="15899-180">可以將不同的策略應用於具有屬性的`[EnableCors]`控制器、頁面模型或操作方法。</span><span class="sxs-lookup"><span data-stu-id="15899-180">Different policies can be applied to controllers, page models, or action methods with the `[EnableCors]` attribute.</span></span> <span data-ttu-id="15899-181">當該`[EnableCors]`屬性應用於控制器、頁面模型或操作方法,並在中間件中啟用 CORS 時,將應用***這兩個***策略。</span><span class="sxs-lookup"><span data-stu-id="15899-181">When the `[EnableCors]` attribute is applied to a controller, page model, or action method, and CORS is enabled in middleware, ***both*** policies are applied.</span></span> <span data-ttu-id="15899-182">***我們建議不要合併策略。使用相同的應用程式的屬性***`[EnableCors]`***或中間件,而不是兩者在同一應用中。***</span><span class="sxs-lookup"><span data-stu-id="15899-182">***We recommend against combining policies. Use the*** `[EnableCors]` ***attribute or middleware, not both in the same app.***</span></span>

<span data-ttu-id="15899-183">以下代碼對每種方法應用不同的原則:</span><span class="sxs-lookup"><span data-stu-id="15899-183">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="15899-184">以下代碼建立兩個 CORS 政策:</span><span class="sxs-lookup"><span data-stu-id="15899-184">The following code creates two CORS policies:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup3.cs?name=snippet&highlight=12-28,44)]

<span data-ttu-id="15899-185">為了最好地控制限制 CORS 請求:</span><span class="sxs-lookup"><span data-stu-id="15899-185">For the finest control of limiting CORS requests:</span></span>

* <span data-ttu-id="15899-186">與`[EnableCors("MyPolicy")]`命名策略一起使用。</span><span class="sxs-lookup"><span data-stu-id="15899-186">Use `[EnableCors("MyPolicy")]` with a named policy.</span></span>
* <span data-ttu-id="15899-187">不要定義預設策略。</span><span class="sxs-lookup"><span data-stu-id="15899-187">Don't define a default policy.</span></span>
* <span data-ttu-id="15899-188">不要使用[終結點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="15899-188">Don't use [endpoint routing](#ecors).</span></span>

<span data-ttu-id="15899-189">下一節中的代碼與前面的清單一應合。</span><span class="sxs-lookup"><span data-stu-id="15899-189">The code in the next section meets the preceding list.</span></span>

<span data-ttu-id="15899-190">有關測試代碼的說明,請參閱[測試 CORS,](#testc)這些說明與前面的代碼類似。</span><span class="sxs-lookup"><span data-stu-id="15899-190">See [Test CORS](#testc) for instructions on testing code similar to the preceding code.</span></span>

<a name="dc"></a>

### <a name="disable-cors"></a><span data-ttu-id="15899-191">關閉 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-191">Disable CORS</span></span>

<span data-ttu-id="15899-192">[[禁用 Cors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性***不會***停用終結點[路由](#ecors)已啟用的 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-192">The [[DisableCors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute does ***not***  disable CORS that has been enabled by [endpoint routing](#ecors).</span></span>

<span data-ttu-id="15899-193">以下代碼定義 CORS`"MyPolicy"`政策 :</span><span class="sxs-lookup"><span data-stu-id="15899-193">The following code defines the CORS policy `"MyPolicy"`:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTestMyPolicy.cs?name=snippet)]

<span data-ttu-id="15899-194">以下代碼關閉操作的`GetValues2`CORS:</span><span class="sxs-lookup"><span data-stu-id="15899-194">The following code disables CORS for the `GetValues2` action:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet&highlight=1,23)]

<span data-ttu-id="15899-195">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="15899-195">The preceding code:</span></span>

* <span data-ttu-id="15899-196">不支援帶[終結點路由](#ecors)的 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-196">Doesn't enable CORS with [endpoint routing](#ecors).</span></span>
* <span data-ttu-id="15899-197">不定義[預設的 CORS 政策](#dp)。</span><span class="sxs-lookup"><span data-stu-id="15899-197">Doesn't define a [default CORS policy](#dp).</span></span>
* <span data-ttu-id="15899-198">使用[[啟用 Cors("我的策略")]](#attr)`"MyPolicy"`為控制器啟用 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="15899-198">Uses [[EnableCors("MyPolicy")]](#attr) to enable the `"MyPolicy"` CORS policy for the controller.</span></span>
* <span data-ttu-id="15899-199">禁用方法的`GetValues2`CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-199">Disables CORS for the `GetValues2` method.</span></span>

<span data-ttu-id="15899-200">有關測試上述代碼的說明,請參閱[測試 CORS。](#testc)</span><span class="sxs-lookup"><span data-stu-id="15899-200">See [Test CORS](#testc) for instructions on testing the preceding code.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="15899-201">CORS 政策選項</span><span class="sxs-lookup"><span data-stu-id="15899-201">CORS policy options</span></span>

<span data-ttu-id="15899-202">本節介紹可在 CORS 策略中設定的各種選項:</span><span class="sxs-lookup"><span data-stu-id="15899-202">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="15899-203">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="15899-203">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="15899-204">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="15899-204">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="15899-205">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="15899-205">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="15899-206">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="15899-206">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="15899-207">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="15899-207">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="15899-208">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="15899-208">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="15899-209"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>在`Startup.ConfigureServices`中 呼叫 。</span><span class="sxs-lookup"><span data-stu-id="15899-209"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="15899-210">對於某些選項,首先閱讀[「CORS 的工作原理」](#how-cors)部分可能會有所説明。</span><span class="sxs-lookup"><span data-stu-id="15899-210">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="15899-211">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="15899-211">Set the allowed origins</span></span>

<span data-ttu-id="15899-212"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>&ndash;允許使用任何方案 (`http``https`或 ) 從所有來源請求 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="15899-212"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="15899-213">`AllowAnyOrigin`不安全,因為*任何網站*都可以向應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-213">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="15899-214">指定`AllowAnyOrigin``AllowCredentials`和 是不安全的配置,可能會導致跨網站請求偽造。</span><span class="sxs-lookup"><span data-stu-id="15899-214">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="15899-215">當應用同時配置這兩種方法時,CORS 服務返回無效的 CORS 回應。</span><span class="sxs-lookup"><span data-stu-id="15899-215">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="15899-216">`AllowAnyOrigin`影響預檢請求和`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-216">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="15899-217">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-217">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="15899-218"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash;將<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略的屬性設為一個函數,允許源在評估是否允許原點時匹配配置的通配符域。</span><span class="sxs-lookup"><span data-stu-id="15899-218"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="15899-219">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="15899-219">Set the allowed HTTP methods</span></span>

<span data-ttu-id="15899-220"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="15899-220"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="15899-221">允許任何 HTTP 方法:</span><span class="sxs-lookup"><span data-stu-id="15899-221">Allows any HTTP method:</span></span>
* <span data-ttu-id="15899-222">影響印前請求和`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-222">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="15899-223">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-223">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="15899-224">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="15899-224">Set the allowed request headers</span></span>

<span data-ttu-id="15899-225">要允許在 CORS 請求中傳送特定標頭,請呼叫作者[要求標頭](https://xhr.spec.whatwg.org/#request),呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭:</span><span class="sxs-lookup"><span data-stu-id="15899-225">To allow specific headers to be sent in a CORS request, called [author request headers](https://xhr.spec.whatwg.org/#request), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

<span data-ttu-id="15899-226">要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers),請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="15899-226">To allow all [author request headers](https://www.w3.org/TR/cors/#author-request-headers), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

<span data-ttu-id="15899-227">`AllowAnyHeader`影響預取要求與[存取控制要求標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)。</span><span class="sxs-lookup"><span data-stu-id="15899-227">`AllowAnyHeader` affects preflight requests and the [Access-Control-Request-Headers](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method) header.</span></span> <span data-ttu-id="15899-228">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-228">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="15899-229">僅當中`Access-Control-Request-Headers`發送的標頭與`WithHeaders`中 所述標頭`WithHeaders`完全匹配時 ,才能與 指定的特定標頭匹配 CORS 中間件策略。</span><span class="sxs-lookup"><span data-stu-id="15899-229">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="15899-230">例如,請考慮配置如下的應用:</span><span class="sxs-lookup"><span data-stu-id="15899-230">For instance, consider an app configured as follows:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet4)]

<span data-ttu-id="15899-231">CORS 中介件拒絕具有以下請求標頭的預檢要求,`Content-Language`因為 ([標題名稱. Content 語言](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) 未在`WithHeaders`中列出:</span><span class="sxs-lookup"><span data-stu-id="15899-231">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="15899-232">該應用程式返回*200 OK*回應,但不會將 CORS 標頭髮送回。</span><span class="sxs-lookup"><span data-stu-id="15899-232">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="15899-233">因此,瀏覽器不會嘗試跨源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-233">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="15899-234">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="15899-234">Set the exposed response headers</span></span>

<span data-ttu-id="15899-235">默認情況下,瀏覽器不會向應用公開所有響應標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-235">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="15899-236">有關詳細資訊,請參閱[W3C 跨源資源分享(術語):簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="15899-236">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="15899-237">預設情況下可用的回應標頭是:</span><span class="sxs-lookup"><span data-stu-id="15899-237">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="15899-238">CORS 規範呼叫這些標頭*為簡單回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="15899-238">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="15899-239">要使其他標頭可供應用使用,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="15899-239">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet5)]
### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="15899-240">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="15899-240">Credentials in cross-origin requests</span></span>

<span data-ttu-id="15899-241">憑據需要在 CORS 請求中特殊處理。</span><span class="sxs-lookup"><span data-stu-id="15899-241">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="15899-242">默認情況下,瀏覽器不會發送具有跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-242">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="15899-243">認證包括 Cookie 和 HTTP 驗證方案。</span><span class="sxs-lookup"><span data-stu-id="15899-243">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="15899-244">要使用跨源要求傳送認證,客戶端必須設定`XMLHttpRequest.withCredentials``true`為 。</span><span class="sxs-lookup"><span data-stu-id="15899-244">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="15899-245">直接`XMLHttpRequest`使用:</span><span class="sxs-lookup"><span data-stu-id="15899-245">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="15899-246">使用 jQuery:</span><span class="sxs-lookup"><span data-stu-id="15899-246">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="15899-247">使用[擷取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="15899-247">Using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="15899-248">伺服器必須允許憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-248">The server must allow the credentials.</span></span> <span data-ttu-id="15899-249">要允許跨源認證,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="15899-249">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet6)]

<span data-ttu-id="15899-250">HTTP 回應包括`Access-Control-Allow-Credentials`一個標頭,該標頭告訴瀏覽器伺服器允許跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-250">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="15899-251">如果瀏覽器發送憑據,但回應不包含有效的`Access-Control-Allow-Credentials`標頭,則瀏覽器不會向應用公開回應,並且跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="15899-251">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="15899-252">允許跨源憑據存在安全風險。</span><span class="sxs-lookup"><span data-stu-id="15899-252">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="15899-253">另一個域中的網站可以在使用者不知情的情況下代表使用者向應用發送登錄使用者的憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-253">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="15899-254">CORS 規範還規定,如果標頭存在`"*"``Access-Control-Allow-Credentials`, 則將原點設置為(所有源)無效。</span><span class="sxs-lookup"><span data-stu-id="15899-254">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

<a name="pref"></a>

## <a name="preflight-requests"></a><span data-ttu-id="15899-255">預先要求</span><span class="sxs-lookup"><span data-stu-id="15899-255">Preflight requests</span></span>

<span data-ttu-id="15899-256">對於某些 CORS 請求,瀏覽器在發出實際請求之前會發送其他[OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)請求。</span><span class="sxs-lookup"><span data-stu-id="15899-256">For some CORS requests, the browser sends an additional [OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS) request before making the actual request.</span></span> <span data-ttu-id="15899-257">此要求為[預先要求](https://developer.mozilla.org/docs/Glossary/Preflight_request)。</span><span class="sxs-lookup"><span data-stu-id="15899-257">This request is called a [preflight request](https://developer.mozilla.org/docs/Glossary/Preflight_request).</span></span> <span data-ttu-id="15899-258">如果以下***所有條件都***為 true,瀏覽器可以跳過預檢請求:</span><span class="sxs-lookup"><span data-stu-id="15899-258">The browser can skip the preflight request if ***all*** the following conditions are true:</span></span>

* <span data-ttu-id="15899-259">請求方法是 GET、頭或 POST。</span><span class="sxs-lookup"><span data-stu-id="15899-259">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="15899-260">套用不設定`Accept`要求 標頭以外`Accept-Language``Content-Language``Content-Type`的`Last-Event-ID`、、、、 或 。</span><span class="sxs-lookup"><span data-stu-id="15899-260">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="15899-261">標頭`Content-Type`(如果已設定)具有以下值之一:</span><span class="sxs-lookup"><span data-stu-id="15899-261">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="15899-262">為用戶端請求設置的請求標頭規則適用於應用通過調用`setRequestHeader`物件設置`XMLHttpRequest`的 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-262">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="15899-263">CORS 規範呼叫這些標頭[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)。</span><span class="sxs-lookup"><span data-stu-id="15899-263">The CORS specification calls these headers [author request headers](https://www.w3.org/TR/cors/#author-request-headers).</span></span> <span data-ttu-id="15899-264">這個規則不適用於瀏覽器可以設定的標頭,例如`User-Agent` `Host` `Content-Length` 。</span><span class="sxs-lookup"><span data-stu-id="15899-264">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="15899-265">下面是與本文件[「測試」](#testc)部分中的 **[放置測試]** 按鈕所做的預檢請求類似的範例回應。</span><span class="sxs-lookup"><span data-stu-id="15899-265">The following is an example response similar to the preflight request made from the **[Put test]** button in the [Test CORS](#testc) section of this document.</span></span>

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

<span data-ttu-id="15899-266">預檢請求使用[HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)方法。</span><span class="sxs-lookup"><span data-stu-id="15899-266">The preflight request uses the [HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS) method.</span></span> <span data-ttu-id="15899-267">它可能包括以下標頭:</span><span class="sxs-lookup"><span data-stu-id="15899-267">It may include the following headers:</span></span>

* <span data-ttu-id="15899-268">[存取控制-請求方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method):將用於實際請求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="15899-268">[Access-Control-Request-Method](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method): The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="15899-269">[存取控制-請求-標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers):應用在實際請求上設置的請求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="15899-269">[Access-Control-Request-Headers](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers): A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="15899-270">如前所述,這不包括瀏覽器設定的標頭,如`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="15899-270">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>
* [<span data-ttu-id="15899-271">存取控制-允許方法</span><span class="sxs-lookup"><span data-stu-id="15899-271">Access-Control-Allow-Methods</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

<span data-ttu-id="15899-272">如果預檢請求被拒絕,應用將返回回應`200 OK`,但不會設置 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-272">If the preflight request is denied, the app returns a `200 OK` response but doesn't set the CORS headers.</span></span> <span data-ttu-id="15899-273">因此,瀏覽器不會嘗試跨源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-273">Therefore, the browser doesn't attempt the cross-origin request.</span></span> <span data-ttu-id="15899-274">有關被拒絕的印前請求的範例,請參閱本文件的[「測試 CORS」](#testc)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-274">For an example of a denied preflight request, see the [Test CORS](#testc) section of this document.</span></span>

<span data-ttu-id="15899-275">使用 F12 工具,主控台應用程式會顯示類似於以下錯誤之一的錯誤,具體取決於瀏覽器:</span><span class="sxs-lookup"><span data-stu-id="15899-275">Using the F12 tools, the console app shows an error similar to one of the following, depending on the browser:</span></span>

* <span data-ttu-id="15899-276">Firefox: 跨源請求被阻止: 同一源策略不允許讀`https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5`取 中的遠端資源。</span><span class="sxs-lookup"><span data-stu-id="15899-276">Firefox: Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at `https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5`.</span></span> <span data-ttu-id="15899-277">(原因:CORS 請求未成功)。</span><span class="sxs-lookup"><span data-stu-id="15899-277">(Reason: CORS request did not succeed).</span></span> [<span data-ttu-id="15899-278">深入了解</span><span class="sxs-lookup"><span data-stu-id="15899-278">Learn More</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS/Errors/CORSDidNotSucceed)
* <span data-ttu-id="15899-279">基於鉻:CORS 策略阻止了以https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5『 來自https://cors3.azurewebsites.net源' 獲取的訪問:對印前請求的回應未通過訪問控制檢查:請求的資源上不存在"訪問-控制-允許-原點"標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-279">Chromium based: Access to fetch at 'https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5' from origin 'https://cors3.azurewebsites.net' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span> <span data-ttu-id="15899-280">如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。</span><span class="sxs-lookup"><span data-stu-id="15899-280">If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</span></span>

<span data-ttu-id="15899-281">要允許特定標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="15899-281">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

<span data-ttu-id="15899-282">要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers),請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="15899-282">To allow all [author request headers](https://www.w3.org/TR/cors/#author-request-headers), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

<span data-ttu-id="15899-283">瀏覽器的設置`Access-Control-Request-Headers`方式不一致。</span><span class="sxs-lookup"><span data-stu-id="15899-283">Browsers aren't consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="15899-284">如果任一:</span><span class="sxs-lookup"><span data-stu-id="15899-284">If either:</span></span>

* <span data-ttu-id="15899-285">標頭設定為`"*"`</span><span class="sxs-lookup"><span data-stu-id="15899-285">Headers are set to anything other than `"*"`</span></span>
* <span data-ttu-id="15899-286"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>呼叫:`Accept`至少`Content-Type`包括`Origin`, 和, 以及要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-286"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*> is called: Include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<a name="apf"></a>

### <a name="automatic-preflight-request-code"></a><span data-ttu-id="15899-287">自動預先要求代碼</span><span class="sxs-lookup"><span data-stu-id="15899-287">Automatic preflight request code</span></span>

<span data-ttu-id="15899-288">應用 CORS 策略時,</span><span class="sxs-lookup"><span data-stu-id="15899-288">When the CORS policy is applied either:</span></span>

* <span data-ttu-id="15899-289">透過 呼`app.UseCors``Startup.Configure`叫 在全球範圍內呼叫 。</span><span class="sxs-lookup"><span data-stu-id="15899-289">Globally by calling `app.UseCors` in `Startup.Configure`.</span></span>
* <span data-ttu-id="15899-290">使用`[EnableCors]`屬性。</span><span class="sxs-lookup"><span data-stu-id="15899-290">Using the `[EnableCors]` attribute.</span></span>

<span data-ttu-id="15899-291">ASP.NET核心回應預檢選項請求。</span><span class="sxs-lookup"><span data-stu-id="15899-291">ASP.NET Core responds to the preflight OPTIONS request.</span></span>

<span data-ttu-id="15899-292">使用`RequireCors`目前基於每個終結點啟用 CORS***不支援***自動預檢請求。</span><span class="sxs-lookup"><span data-stu-id="15899-292">Enabling CORS on a per-endpoint basis using `RequireCors` currently does ***not*** support automatic preflight requests.</span></span>

<span data-ttu-id="15899-293">本文件的[「測試 CORS」](#testc)部分演示了此行為。</span><span class="sxs-lookup"><span data-stu-id="15899-293">The [Test CORS](#testc) section of this document demonstrates this behavior.</span></span>

<a name="pro"></a>

### <a name="httpoptions-attribute-for-preflight-requests"></a><span data-ttu-id="15899-294">預先要求的 [HttpOptions] 屬性</span><span class="sxs-lookup"><span data-stu-id="15899-294">[HttpOptions] attribute for preflight requests</span></span>

<span data-ttu-id="15899-295">當使用適當的策略啟用 CORS 時,ASP.NET核心通常會自動回應 CORS 預檢請求。</span><span class="sxs-lookup"><span data-stu-id="15899-295">When CORS is enabled with the appropriate policy, ASP.NET Core generally responds to CORS preflight requests automatically.</span></span> <span data-ttu-id="15899-296">在某些情況下,情況可能並非如此。</span><span class="sxs-lookup"><span data-stu-id="15899-296">In some scenarios, this may not be the case.</span></span> <span data-ttu-id="15899-297">例如,將[CORS 與終結點路由一起使用](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="15899-297">For example, using [CORS with endpoint routing](#ecors).</span></span>

<span data-ttu-id="15899-298">以下代碼使用[[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute)屬性為 OPTIONS 請求建立終結點:</span><span class="sxs-lookup"><span data-stu-id="15899-298">The following code uses the [[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute) attribute to create endpoints for OPTIONS requests:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet&highlight=5-17)]

<span data-ttu-id="15899-299">有關測試上述代碼的說明,請參閱[使用終結點路由和 [HttpOptions] 測試 CORS。](#tcer)</span><span class="sxs-lookup"><span data-stu-id="15899-299">See [Test CORS with endpoint routing and [HttpOptions]](#tcer) for instructions on testing the preceding code.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="15899-300">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="15899-300">Set the preflight expiration time</span></span>

<span data-ttu-id="15899-301">標頭`Access-Control-Max-Age`指定對預檢請求的回應可以緩存多長時間。</span><span class="sxs-lookup"><span data-stu-id="15899-301">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="15899-302">要設定這個標頭,請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="15899-302">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet7)]
<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="15899-303">CORS 的工作原理</span><span class="sxs-lookup"><span data-stu-id="15899-303">How CORS works</span></span>

<span data-ttu-id="15899-304">本節介紹在 HTTP 消息級別發生的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)請求中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="15899-304">This section describes what happens in a [CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="15899-305">CORS**不是**一個安全功能。</span><span class="sxs-lookup"><span data-stu-id="15899-305">CORS is **not** a security feature.</span></span> <span data-ttu-id="15899-306">CORS 是一種 W3C 標準,允許伺服器放鬆同源策略。</span><span class="sxs-lookup"><span data-stu-id="15899-306">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="15899-307">例如,惡意參與者可能會對您的網站使用[跨網站腳本 (XSS),](xref:security/cross-site-scripting)並對其啟用 CORS 的網站執行跨網站請求以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="15899-307">For example, a malicious actor could use [Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="15899-308">通過允許 CORS,API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="15899-308">An API isn't safer by allowing CORS.</span></span>
  * <span data-ttu-id="15899-309">由用戶端(瀏覽器)來強制實施 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-309">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="15899-310">伺服器執行請求並返回回應,返回錯誤的是用戶端並阻止回應。</span><span class="sxs-lookup"><span data-stu-id="15899-310">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="15899-311">例如,以下任何工具將顯示伺服器回應:</span><span class="sxs-lookup"><span data-stu-id="15899-311">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="15899-312">Fiddler</span><span class="sxs-lookup"><span data-stu-id="15899-312">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="15899-313">Postman</span><span class="sxs-lookup"><span data-stu-id="15899-313">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="15899-314">.NET HTTPClient</span><span class="sxs-lookup"><span data-stu-id="15899-314">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="15899-315">通過在位址列中輸入 URL 來訪問 Web 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="15899-315">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="15899-316">這是伺服器允許瀏覽器執行跨源[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)請求的一種方式,否則將禁止這樣做。</span><span class="sxs-lookup"><span data-stu-id="15899-316">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="15899-317">沒有 CORS 的瀏覽器無法執行跨源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-317">Browsers without CORS can't do cross-origin requests.</span></span> <span data-ttu-id="15899-318">在 CORS 之前[,JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)曾用於規避此限制。</span><span class="sxs-lookup"><span data-stu-id="15899-318">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="15899-319">JSONP 不使用 XHR,`<script>`它使用 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="15899-319">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="15899-320">允許跨源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="15899-320">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="15899-321">[CORS 規範](https://www.w3.org/TR/cors/)引入了幾個支援跨源請求的新 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-321">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="15899-322">如果瀏覽器支援 CORS,它將自動為跨源請求設置這些標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-322">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="15899-323">啟用 CORS 不需要自訂 JavaScript 代碼。</span><span class="sxs-lookup"><span data-stu-id="15899-323">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="15899-324">已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的[PUT 測試按鈕](https://cors3.azurewebsites.net/test)</span><span class="sxs-lookup"><span data-stu-id="15899-324">The  [PUT test button](https://cors3.azurewebsites.net/test) on the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)</span></span>

<span data-ttu-id="15899-325">下面是從[「值](https://cors3.azurewebsites.net/)」測試`https://cors1.azurewebsites.net/api/values`按鈕到的跨源請求的範例。</span><span class="sxs-lookup"><span data-stu-id="15899-325">The following is an example of a cross-origin request from the [Values](https://cors3.azurewebsites.net/) test button to `https://cors1.azurewebsites.net/api/values`.</span></span> <span data-ttu-id="15899-326">標頭`Origin`:</span><span class="sxs-lookup"><span data-stu-id="15899-326">The `Origin` header:</span></span>

* <span data-ttu-id="15899-327">提供發出請求的網站的域。</span><span class="sxs-lookup"><span data-stu-id="15899-327">Provides the domain of the site that's making the request.</span></span>
* <span data-ttu-id="15899-328">是必需的,並且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="15899-328">Is required and must be different from the host.</span></span>

<span data-ttu-id="15899-329">**一般標頭**</span><span class="sxs-lookup"><span data-stu-id="15899-329">**General headers**</span></span>

```
Request URL: https://cors1.azurewebsites.net/api/values
Request Method: GET
Status Code: 200 OK
```

<span data-ttu-id="15899-330">**回應標頭**</span><span class="sxs-lookup"><span data-stu-id="15899-330">**Response headers**</span></span>

```
Content-Encoding: gzip
Content-Type: text/plain; charset=utf-8
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors1.azurewebsites.net
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Powered-By: ASP.NET
```

<span data-ttu-id="15899-331">**要求標頭**</span><span class="sxs-lookup"><span data-stu-id="15899-331">**Request headers**</span></span>

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

<span data-ttu-id="15899-332">在`OPTIONS`請求中,伺服器在回應中設置**回應**`Access-Control-Allow-Origin: {allowed origin}`標頭標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-332">In `OPTIONS` requests, the server sets the **Response headers** `Access-Control-Allow-Origin: {allowed origin}` header in the response.</span></span> <span data-ttu-id="15899-333">例如,已部署[的範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) [,刪除 [啟用 Cors]](https://cors1.azurewebsites.net/test?number=2)按鈕`OPTIONS`請求包含以下標頭:</span><span class="sxs-lookup"><span data-stu-id="15899-333">For example, the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI), [Delete [EnableCors]](https://cors1.azurewebsites.net/test?number=2) button `OPTIONS` request contains the following  headers:</span></span>

<span data-ttu-id="15899-334">**一般標頭**</span><span class="sxs-lookup"><span data-stu-id="15899-334">**General headers**</span></span>

```
Request URL: https://cors3.azurewebsites.net/api/TodoItems2/MyDelete2/5
Request Method: OPTIONS
Status Code: 204 No Content
```

<span data-ttu-id="15899-335">**回應標頭**</span><span class="sxs-lookup"><span data-stu-id="15899-335">**Response headers**</span></span>

```
Access-Control-Allow-Headers: Content-Type,x-custom-header
Access-Control-Allow-Methods: PUT,DELETE,GET,OPTIONS
Access-Control-Allow-Origin: https://cors1.azurewebsites.net
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors3.azurewebsites.net
Vary: Origin
X-Powered-By: ASP.NET
```

<span data-ttu-id="15899-336">**要求標頭**</span><span class="sxs-lookup"><span data-stu-id="15899-336">**Request headers**</span></span>

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

<span data-ttu-id="15899-337">在前面的**回應標頭中**,伺服器在回應中設置[訪問控制允許源](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-337">In the preceding **Response headers**, the server sets the [Access-Control-Allow-Origin](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) header in the response.</span></span> <span data-ttu-id="15899-338">此`https://cors1.azurewebsites.net`標頭的值與請求中的`Origin`標頭匹配。</span><span class="sxs-lookup"><span data-stu-id="15899-338">The `https://cors1.azurewebsites.net` value of this header matches the `Origin` header from the request.</span></span>

<span data-ttu-id="15899-339">如果<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>呼叫`Access-Control-Allow-Origin: *`, 則傳回的通配符值。</span><span class="sxs-lookup"><span data-stu-id="15899-339">If <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> is called, the `Access-Control-Allow-Origin: *`, the wildcard value, is returned.</span></span> <span data-ttu-id="15899-340">`AllowAnyOrigin`允許任何來源。</span><span class="sxs-lookup"><span data-stu-id="15899-340">`AllowAnyOrigin` allows any origin.</span></span>

<span data-ttu-id="15899-341">如果回應不包括標頭,`Access-Control-Allow-Origin`則跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="15899-341">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="15899-342">具體來說,瀏覽器不允許請求。</span><span class="sxs-lookup"><span data-stu-id="15899-342">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="15899-343">即使伺服器返回成功的回應,瀏覽器也不會使回應對用戶端應用可用。</span><span class="sxs-lookup"><span data-stu-id="15899-343">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="options"></a>

### <a name="display-options-requests"></a><span data-ttu-id="15899-344">顯示選項要求</span><span class="sxs-lookup"><span data-stu-id="15899-344">Display OPTIONS requests</span></span>

<span data-ttu-id="15899-345">默認情況下,Chrome 和邊緣瀏覽器不會在 F12 工具的網路選項卡上顯示選項請求。</span><span class="sxs-lookup"><span data-stu-id="15899-345">By default, the Chrome and Edge browsers don't show OPTIONS requests on the network tab of the F12 tools.</span></span> <span data-ttu-id="15899-346">要在這些瀏覽器中顯示選項請求,</span><span class="sxs-lookup"><span data-stu-id="15899-346">To display OPTIONS requests in these browsers:</span></span>

* <span data-ttu-id="15899-347">`chrome://flags/#out-of-blink-cors` 或 `edge://flags/#out-of-blink-cors`</span><span class="sxs-lookup"><span data-stu-id="15899-347">`chrome://flags/#out-of-blink-cors` or `edge://flags/#out-of-blink-cors`</span></span>
* <span data-ttu-id="15899-348">禁用標誌。</span><span class="sxs-lookup"><span data-stu-id="15899-348">disable the flag.</span></span>
* <span data-ttu-id="15899-349">重新啟動。</span><span class="sxs-lookup"><span data-stu-id="15899-349">restart.</span></span>

<span data-ttu-id="15899-350">默認情況下,Firefox 會顯示選項請求。</span><span class="sxs-lookup"><span data-stu-id="15899-350">Firefox shows OPTIONS requests by default.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="15899-351">IIS 的 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-351">CORS in IIS</span></span>

<span data-ttu-id="15899-352">部署到IIS時,如果伺服器未配置為允許匿名訪問,則CORS必須在Windows身份驗證之前運行。</span><span class="sxs-lookup"><span data-stu-id="15899-352">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="15899-353">為了支援此專案,需要為應用程式安裝和設定[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="15899-353">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

<a name="testc"></a>

## <a name="test-cors"></a><span data-ttu-id="15899-354">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-354">Test CORS</span></span>

<span data-ttu-id="15899-355">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)具有用於測試 CORS 的代碼。</span><span class="sxs-lookup"><span data-stu-id="15899-355">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) has code to test CORS.</span></span> <span data-ttu-id="15899-356">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="15899-356">See [how to download](xref:index#how-to-download-a-sample).</span></span> <span data-ttu-id="15899-357">該範例為新增 Razor 頁面的 API 專案:</span><span class="sxs-lookup"><span data-stu-id="15899-357">The sample is an API project with Razor Pages added:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTest2.cs?name=snippet2)]

  > [!WARNING]
  > <span data-ttu-id="15899-358">`WithOrigins("https://localhost:<port>");`應僅用於測試類似於[下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors)的範例應用。</span><span class="sxs-lookup"><span data-stu-id="15899-358">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors).</span></span>

<span data-ttu-id="15899-359">下面`ValuesController`提供測試的終結點:</span><span class="sxs-lookup"><span data-stu-id="15899-359">The following `ValuesController` provides the endpoints for testing:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet)]

<span data-ttu-id="15899-360">[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs)由[Rick.Docs.sample.RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet 包提供,並顯示路線資訊。</span><span class="sxs-lookup"><span data-stu-id="15899-360">[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs) is provided by the [Rick.Docs.Samples.RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet package and displays route information.</span></span>

<span data-ttu-id="15899-361">使用以下方法之一測試前面的範例碼:</span><span class="sxs-lookup"><span data-stu-id="15899-361">Test the preceding sample code by using one of the following approaches:</span></span>

* <span data-ttu-id="15899-362">在中使用部署的範例[https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/)應用。</span><span class="sxs-lookup"><span data-stu-id="15899-362">Use the deployed sample app at [https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/).</span></span> <span data-ttu-id="15899-363">無需下載示例。</span><span class="sxs-lookup"><span data-stu-id="15899-363">There is no need to download the sample.</span></span>
* <span data-ttu-id="15899-364">`dotnet run`使用`https://localhost:5001`的預設網址 執行範例。</span><span class="sxs-lookup"><span data-stu-id="15899-364">Run the sample with `dotnet run` using the default URL of `https://localhost:5001`.</span></span>
* <span data-ttu-id="15899-365">從 Visual Studio 運行範例,埠設定為 44398,`https://localhost:44398`用於 URL。</span><span class="sxs-lookup"><span data-stu-id="15899-365">Run the sample from Visual Studio with the port set to 44398 for a URL of `https://localhost:44398`.</span></span>

<span data-ttu-id="15899-366">將瀏覽器與 F12 工具一起使用:</span><span class="sxs-lookup"><span data-stu-id="15899-366">Using a browser with the F12 tools:</span></span>

* <span data-ttu-id="15899-367">選擇「**值」** 按鈕並檢視 **「網路」** 選項卡中的標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-367">Select the **Values** button and review the headers in the **Network** tab.</span></span>
* <span data-ttu-id="15899-368">選擇**PUT 測試**按鈕。</span><span class="sxs-lookup"><span data-stu-id="15899-368">Select the **PUT test** button.</span></span> <span data-ttu-id="15899-369">關於顯示 OPTIONS 要求的說明,請參考[顯示選項要求](#options)。</span><span class="sxs-lookup"><span data-stu-id="15899-369">See [Display OPTIONS requests](#options) for instructions on displaying the OPTIONS request.</span></span> <span data-ttu-id="15899-370">**PUT 測試**創建兩個請求,一個選項預檢請求和 PUT 請求。</span><span class="sxs-lookup"><span data-stu-id="15899-370">The **PUT test** creates two requests, an OPTIONS preflight request and the PUT request.</span></span>
* <span data-ttu-id="15899-371">選擇按鈕**`GetValues2 [DisableCors]`** 以觸發失敗的 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="15899-371">Select the **`GetValues2 [DisableCors]`** button to trigger a failed CORS request.</span></span> <span data-ttu-id="15899-372">如文檔中所述,回應返回 200 成功,但未發出 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="15899-372">As mentioned in the document, the response returns 200 success, but the CORS request is not made.</span></span> <span data-ttu-id="15899-373">選擇 **「控制台」** 選項卡以檢視 CORS 錯誤。</span><span class="sxs-lookup"><span data-stu-id="15899-373">Select the **Console** tab to see the CORS error.</span></span> <span data-ttu-id="15899-374">根據瀏覽器的不同,將顯示類似於以下內容的錯誤:</span><span class="sxs-lookup"><span data-stu-id="15899-374">Depending on the browser, an error similar to the following is displayed:</span></span>

     <span data-ttu-id="15899-375">CORS`'https://cors1.azurewebsites.net/api/values/GetValues2'`策略 阻止`'https://cors3.azurewebsites.net'`了從 源提取的訪問:請求的資源上不存在"訪問-控制-允許源"標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-375">Access to fetch at `'https://cors1.azurewebsites.net/api/values/GetValues2'` from origin `'https://cors3.azurewebsites.net'` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span> <span data-ttu-id="15899-376">如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。</span><span class="sxs-lookup"><span data-stu-id="15899-376">If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</span></span>
     
<span data-ttu-id="15899-377">支援 CORS 的終結點可以使用工具進行測試,例如[捲曲](https://curl.haxx.se/)[、Fiddler](https://www.telerik.com/fiddler)或[Postman。](https://www.getpostman.com/)</span><span class="sxs-lookup"><span data-stu-id="15899-377">CORS-enabled endpoints can be tested with a tool, such as [curl](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler), or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="15899-378">使用工具時,`Origin`標頭指定的請求的來源必須不同於接收請求的主機。</span><span class="sxs-lookup"><span data-stu-id="15899-378">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="15899-379">如果要求不是基於標頭的值*的跨源*: `Origin`</span><span class="sxs-lookup"><span data-stu-id="15899-379">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="15899-380">CORS 中間件無需處理請求。</span><span class="sxs-lookup"><span data-stu-id="15899-380">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="15899-381">回應中未返回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-381">CORS headers aren't returned in the response.</span></span>

<span data-ttu-id="15899-382">以下指令用於`curl`送出帶有資訊的 OPTIONS 請求:</span><span class="sxs-lookup"><span data-stu-id="15899-382">The following command uses `curl` to issue an OPTIONS request with information:</span></span>

```bash
curl -X OPTIONS https://cors3.azurewebsites.net/api/TodoItems2/5 -i
```

<!--
curl come with Git. Add to path variable
C:\Program Files\Git\mingw64\bin\
-->

<a name="tcer"></a>

### <a name="test-cors-with-endpoint-routing-and-httpoptions"></a><span data-ttu-id="15899-383">使用端點路由與 [httpOptions] 測試 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-383">Test CORS with endpoint routing and [HttpOptions]</span></span>

<span data-ttu-id="15899-384">使用`RequireCors`目前使用每個的終結點的 CORS***不支援***[自動預取要求](#apf)。</span><span class="sxs-lookup"><span data-stu-id="15899-384">Enabling CORS on a per-endpoint basis using `RequireCors` currently does ***not*** support [automatic preflight requests](#apf).</span></span> <span data-ttu-id="15899-385">請考慮以下使用[終結點路由啟用 CORS 的代碼](#ecors):</span><span class="sxs-lookup"><span data-stu-id="15899-385">Consider the following code which uses [endpoint routing to enable CORS](#ecors):</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPointBugTest.cs?name=snippet2)]

<span data-ttu-id="15899-386">以下內容`TodoItems1Controller`提供測試的終結點:</span><span class="sxs-lookup"><span data-stu-id="15899-386">The following `TodoItems1Controller` provides endpoints for testing:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems1Controller.cs?name=snippet2)]

<span data-ttu-id="15899-387">從已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的[測試頁](https://cors1.azurewebsites.net/test?number=1)測試前面的代碼。</span><span class="sxs-lookup"><span data-stu-id="15899-387">Test the preceding code from the [test page](https://cors1.azurewebsites.net/test?number=1) of the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI).</span></span>

<span data-ttu-id="15899-388">**刪除 [啟用 Cors]** 和**GET [啟用Cors]** 按鈕成功,因為`[EnableCors]`端點具有並回應預檢請求。</span><span class="sxs-lookup"><span data-stu-id="15899-388">The **Delete [EnableCors]** and **GET [EnableCors]** buttons succeed, because the endpoints have `[EnableCors]` and respond to preflight requests.</span></span> <span data-ttu-id="15899-389">其他終結點失敗。</span><span class="sxs-lookup"><span data-stu-id="15899-389">The other endpoints fails.</span></span> <span data-ttu-id="15899-390">**GET**按鈕失敗,因為[JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js)傳送:</span><span class="sxs-lookup"><span data-stu-id="15899-390">The **GET** button fails, because the [JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js) sends:</span></span>

```javascript
 headers: {
      "Content-Type": "x-custom-header"
 },
```

<span data-ttu-id="15899-391">以下內容`TodoItems2Controller`提供類似的終結點,但包括用於回應 OPTIONS 請求的顯式代碼:</span><span class="sxs-lookup"><span data-stu-id="15899-391">The following `TodoItems2Controller` provides similar endpoints, but includes explicit code to respond to OPTIONS requests:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet2)]

<span data-ttu-id="15899-392">從已部署範例的[測試頁](https://cors1.azurewebsites.net/test?number=2)測試前面的代碼。</span><span class="sxs-lookup"><span data-stu-id="15899-392">Test the preceding code from the [test page](https://cors1.azurewebsites.net/test?number=2) of the deployed sample.</span></span> <span data-ttu-id="15899-393">在**控制器**下拉清單中,選擇 **「預檢**」,然後**設定控制器**。</span><span class="sxs-lookup"><span data-stu-id="15899-393">In the **Controller** drop down list, select **Preflight** and then **Set Controller**.</span></span> <span data-ttu-id="15899-394">對`TodoItems2Controller`終結點的所有 CORS 調用都成功。</span><span class="sxs-lookup"><span data-stu-id="15899-394">All the CORS calls to the `TodoItems2Controller` endpoints succeed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15899-395">其他資源</span><span class="sxs-lookup"><span data-stu-id="15899-395">Additional resources</span></span>

* [<span data-ttu-id="15899-396">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="15899-396">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="15899-397">開始使用 IIS CORS 模組</span><span class="sxs-lookup"><span data-stu-id="15899-397">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="15899-398">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="15899-398">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="15899-399">本文演示如何在ASP.NET核心應用中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-399">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="15899-400">流覽器安全性可防止網頁向與服務網頁的域不同的域發出請求。</span><span class="sxs-lookup"><span data-stu-id="15899-400">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="15899-401">此限制稱為*同源原則*。</span><span class="sxs-lookup"><span data-stu-id="15899-401">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="15899-402">同源策略可防止惡意網站從其他網站讀取敏感數據。</span><span class="sxs-lookup"><span data-stu-id="15899-402">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="15899-403">有時,您可能希望允許其他網站向你的應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-403">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="15899-404">有關詳細資訊,請參閱[Mozilla CORS 文章](https://developer.mozilla.org/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="15899-404">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="15899-405">[跨源資源分享](https://www.w3.org/TR/cors/)(CORS):</span><span class="sxs-lookup"><span data-stu-id="15899-405">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="15899-406">是允許伺服器放鬆同源策略的 W3C 標準。</span><span class="sxs-lookup"><span data-stu-id="15899-406">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="15899-407">**CORS 不是**安全功能,可放鬆安全性。</span><span class="sxs-lookup"><span data-stu-id="15899-407">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="15899-408">通過允許 CORS,API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="15899-408">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="15899-409">有關詳細資訊,請參閱[CORS 的工作原理](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="15899-409">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="15899-410">允許伺服器顯式允許某些跨源請求,同時拒絕其他請求。</span><span class="sxs-lookup"><span data-stu-id="15899-410">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="15899-411">比早期的技術(如[JSONP](/dotnet/framework/wcf/samples/jsonp))更安全、更靈活。</span><span class="sxs-lookup"><span data-stu-id="15899-411">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="15899-412">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15899-412">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="15899-413">同一源</span><span class="sxs-lookup"><span data-stu-id="15899-413">Same origin</span></span>

<span data-ttu-id="15899-414">如果兩個 URL 具有相同的方案、主機和埠[(RFC 6454),](https://tools.ietf.org/html/rfc6454)則它們具有相同的源源。</span><span class="sxs-lookup"><span data-stu-id="15899-414">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="15899-415">這兩個網址 有相同的來源:</span><span class="sxs-lookup"><span data-stu-id="15899-415">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="15899-416">這些網址與前兩個網址具有不同的來源:</span><span class="sxs-lookup"><span data-stu-id="15899-416">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="15899-417">`https://example.net`&ndash;不同的網域</span><span class="sxs-lookup"><span data-stu-id="15899-417">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="15899-418">`https://www.example.com/foo.html`&ndash;不同的子域</span><span class="sxs-lookup"><span data-stu-id="15899-418">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="15899-419">`http://example.com/foo.html`&ndash;不同的機制</span><span class="sxs-lookup"><span data-stu-id="15899-419">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="15899-420">`https://example.com:9000/foo.html`&ndash;不同的連接埠</span><span class="sxs-lookup"><span data-stu-id="15899-420">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="15899-421">在比較源時,Internet Explorer 不考慮埠。</span><span class="sxs-lookup"><span data-stu-id="15899-421">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="15899-422">包含命名政策與中間件的 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-422">CORS with named policy and middleware</span></span>

<span data-ttu-id="15899-423">CORS 中間件處理跨源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-423">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="15899-424">以下代碼支援具有指定來源的整個應用的 CORS:</span><span class="sxs-lookup"><span data-stu-id="15899-424">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="15899-425">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="15899-425">The preceding code:</span></span>

* <span data-ttu-id="15899-426">將策略名稱設為"myAllow\_特定原點"</span><span class="sxs-lookup"><span data-stu-id="15899-426">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="15899-427">策略名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="15899-427">The policy name is arbitrary.</span></span>
* <span data-ttu-id="15899-428">呼叫<xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>式擴充方法,該方法啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-428">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="15899-429">使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的調用。</span><span class="sxs-lookup"><span data-stu-id="15899-429">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="15899-430">lambda 取<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>一個物件。</span><span class="sxs-lookup"><span data-stu-id="15899-430">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="15899-431">[配置選項](#cors-policy-options)(`WithOrigins`如 )在本文的後面部分介紹。</span><span class="sxs-lookup"><span data-stu-id="15899-431">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="15899-432">方法<xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>呼叫將 CORS 服務新增到應用的服務容器:</span><span class="sxs-lookup"><span data-stu-id="15899-432">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="15899-433">關於詳細資訊,請參考此文件中的[CORS 政策選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="15899-433">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="15899-434">該方法<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>可以連結方法,如以下代碼所示:</span><span class="sxs-lookup"><span data-stu-id="15899-434">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="15899-435">注意: 網址**不得**包含`/`尾隨斜槓 ( 。</span><span class="sxs-lookup"><span data-stu-id="15899-435">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="15899-436">如果 URL`/`終止與`false`,則比較返回,並且不返回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-436">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="15899-437">以下代碼透過 CORS 的裝置將 CORS 政策應用於所有應用終結點:</span><span class="sxs-lookup"><span data-stu-id="15899-437">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="15899-438">注意:`UseCors`必須在`UseMvc`之前 呼叫 。</span><span class="sxs-lookup"><span data-stu-id="15899-438">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="15899-439">請參閱[在 Razor 頁面、控制器和操作方法中啟用 CORS,](#ecors)以便在頁面/控制器/操作級別應用 CORS 策略。</span><span class="sxs-lookup"><span data-stu-id="15899-439">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="15899-440">有關測試代碼的說明,請參閱[測試 CORS,](#test)這些說明與前面的代碼類似。</span><span class="sxs-lookup"><span data-stu-id="15899-440">See [Test CORS](#test) for instructions on testing code similar to the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="15899-441">使用屬性啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-441">Enable CORS with attributes</span></span>

<span data-ttu-id="15899-442">[啟用 Cors&rbrack;屬性提供了全域應用 CORS 的替代方法。 &lbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="15899-442">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="15899-443">該`[EnableCors]`屬性為選定的端點啟用 CORS,而不是所有端點。</span><span class="sxs-lookup"><span data-stu-id="15899-443">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="15899-444">指定`[EnableCors]`預設策略和`[EnableCors("{Policy String}")]`指定策略。</span><span class="sxs-lookup"><span data-stu-id="15899-444">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="15899-445">該`[EnableCors]`屬性可應用於:</span><span class="sxs-lookup"><span data-stu-id="15899-445">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="15899-446">剃刀頁面`PageModel`</span><span class="sxs-lookup"><span data-stu-id="15899-446">Razor Page `PageModel`</span></span>
* <span data-ttu-id="15899-447">控制器</span><span class="sxs-lookup"><span data-stu-id="15899-447">Controller</span></span>
* <span data-ttu-id="15899-448">控制器操作方法</span><span class="sxs-lookup"><span data-stu-id="15899-448">Controller action method</span></span>

<span data-ttu-id="15899-449">您可以使用`[EnableCors]`屬性對控制器/頁面模型/操作應用不同的策略。</span><span class="sxs-lookup"><span data-stu-id="15899-449">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="15899-450">當該`[EnableCors]`屬性應用於控制器/頁面模型/操作方法,並在中間件中啟用 CORS 時,將應用***這兩個***策略。</span><span class="sxs-lookup"><span data-stu-id="15899-450">When the `[EnableCors]` attribute is applied to a controllers/page model/action method, and CORS is enabled in middleware, ***both*** policies are applied.</span></span> <span data-ttu-id="15899-451">我們建議***不要***合併策略。</span><span class="sxs-lookup"><span data-stu-id="15899-451">We recommend ***not*** combining policies.</span></span> <span data-ttu-id="15899-452">使用屬性`[EnableCors]`或中間件,\***而不是兩者**。</span><span class="sxs-lookup"><span data-stu-id="15899-452">Use the `[EnableCors]` attribute or middleware, \***not both**.</span></span> <span data-ttu-id="15899-453">使用`[EnableCors]`時 **,不要**定義預設策略。</span><span class="sxs-lookup"><span data-stu-id="15899-453">When using `[EnableCors]`, do **not** define a default policy.</span></span>

<span data-ttu-id="15899-454">以下代碼對每種方法應用不同的原則:</span><span class="sxs-lookup"><span data-stu-id="15899-454">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="15899-455">以下代碼建立 CORS 預設政策與名為`"AnotherPolicy"`的策略 :</span><span class="sxs-lookup"><span data-stu-id="15899-455">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="15899-456">關閉 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-456">Disable CORS</span></span>

<span data-ttu-id="15899-457">[禁用 Cors&rbrack;屬性禁用控制器/頁面模型/操作的 CORS。 &lbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="15899-457">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="15899-458">CORS 政策選項</span><span class="sxs-lookup"><span data-stu-id="15899-458">CORS policy options</span></span>

<span data-ttu-id="15899-459">本節介紹可在 CORS 策略中設定的各種選項:</span><span class="sxs-lookup"><span data-stu-id="15899-459">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="15899-460">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="15899-460">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="15899-461">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="15899-461">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="15899-462">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="15899-462">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="15899-463">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="15899-463">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="15899-464">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="15899-464">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="15899-465">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="15899-465">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="15899-466"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>在`Startup.ConfigureServices`中 呼叫 。</span><span class="sxs-lookup"><span data-stu-id="15899-466"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="15899-467">對於某些選項,首先閱讀[「CORS 的工作原理」](#how-cors)部分可能會有所説明。</span><span class="sxs-lookup"><span data-stu-id="15899-467">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="15899-468">設定允許的原點</span><span class="sxs-lookup"><span data-stu-id="15899-468">Set the allowed origins</span></span>

<span data-ttu-id="15899-469"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>&ndash;允許使用任何方案 (`http``https`或 ) 從所有來源請求 CORS 請求。</span><span class="sxs-lookup"><span data-stu-id="15899-469"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="15899-470">`AllowAnyOrigin`不安全,因為*任何網站*都可以向應用發出交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-470">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="15899-471">指定`AllowAnyOrigin``AllowCredentials`和 是不安全的配置,可能會導致跨網站請求偽造。</span><span class="sxs-lookup"><span data-stu-id="15899-471">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="15899-472">對於安全應用,如果客戶端必須授權自己訪問伺服器資源,請指定確切的來源清單。</span><span class="sxs-lookup"><span data-stu-id="15899-472">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="15899-473">`AllowAnyOrigin`影響預檢請求和`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-473">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="15899-474">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-474">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="15899-475"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>&ndash;將<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*>策略的屬性設為一個函數,允許源在評估是否允許原點時匹配配置的通配符域。</span><span class="sxs-lookup"><span data-stu-id="15899-475"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="15899-476">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="15899-476">Set the allowed HTTP methods</span></span>

<span data-ttu-id="15899-477"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="15899-477"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="15899-478">允許任何 HTTP 方法:</span><span class="sxs-lookup"><span data-stu-id="15899-478">Allows any HTTP method:</span></span>
* <span data-ttu-id="15899-479">影響印前請求和`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-479">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="15899-480">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-480">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="15899-481">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="15899-481">Set the allowed request headers</span></span>

<span data-ttu-id="15899-482">要允許在 CORS 請求中傳送特定標頭,請呼叫作者*要求標頭*,呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>並指定允許的標頭:</span><span class="sxs-lookup"><span data-stu-id="15899-482">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="15899-483">要允許所有作者要求標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="15899-483">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="15899-484">此設置會影響預檢請求和`Access-Control-Request-Headers`標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-484">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="15899-485">有關詳細資訊,請參閱[預檢請求](#preflight-requests)部分。</span><span class="sxs-lookup"><span data-stu-id="15899-485">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="15899-486">無論 CorsPolicy.header 中配置`Access-Control-Request-Headers`的值 如何,CORS 中間件始終允許發送 中的四個標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-486">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="15899-487">這個標頭清單包括:</span><span class="sxs-lookup"><span data-stu-id="15899-487">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="15899-488">例如,請考慮配置如下的應用:</span><span class="sxs-lookup"><span data-stu-id="15899-488">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="15899-489">CORS 中間件使用以下請求標頭成功回應預檢請求,`Content-Language`因為 始終被列入白名單:</span><span class="sxs-lookup"><span data-stu-id="15899-489">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="15899-490">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="15899-490">Set the exposed response headers</span></span>

<span data-ttu-id="15899-491">默認情況下,瀏覽器不會向應用公開所有響應標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-491">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="15899-492">有關詳細資訊,請參閱[W3C 跨源資源分享(術語):簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="15899-492">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="15899-493">預設情況下可用的回應標頭是:</span><span class="sxs-lookup"><span data-stu-id="15899-493">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="15899-494">CORS 規範呼叫這些標頭*為簡單回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="15899-494">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="15899-495">要使其他標頭可供應用使用,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="15899-495">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="15899-496">跨源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="15899-496">Credentials in cross-origin requests</span></span>

<span data-ttu-id="15899-497">憑據需要在 CORS 請求中特殊處理。</span><span class="sxs-lookup"><span data-stu-id="15899-497">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="15899-498">默認情況下,瀏覽器不會發送具有跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-498">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="15899-499">認證包括 Cookie 和 HTTP 驗證方案。</span><span class="sxs-lookup"><span data-stu-id="15899-499">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="15899-500">要使用跨源要求傳送認證,客戶端必須設定`XMLHttpRequest.withCredentials``true`為 。</span><span class="sxs-lookup"><span data-stu-id="15899-500">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="15899-501">直接`XMLHttpRequest`使用:</span><span class="sxs-lookup"><span data-stu-id="15899-501">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="15899-502">使用 jQuery:</span><span class="sxs-lookup"><span data-stu-id="15899-502">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="15899-503">使用[擷取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span><span class="sxs-lookup"><span data-stu-id="15899-503">Using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="15899-504">伺服器必須允許憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-504">The server must allow the credentials.</span></span> <span data-ttu-id="15899-505">要允許跨源認證,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span><span class="sxs-lookup"><span data-stu-id="15899-505">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="15899-506">HTTP 回應包括`Access-Control-Allow-Credentials`一個標頭,該標頭告訴瀏覽器伺服器允許跨源請求的憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-506">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="15899-507">如果瀏覽器發送憑據,但回應不包含有效的`Access-Control-Allow-Credentials`標頭,則瀏覽器不會向應用公開回應,並且跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="15899-507">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="15899-508">允許跨源憑據存在安全風險。</span><span class="sxs-lookup"><span data-stu-id="15899-508">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="15899-509">另一個域中的網站可以在使用者不知情的情況下代表使用者向應用發送登錄使用者的憑據。</span><span class="sxs-lookup"><span data-stu-id="15899-509">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="15899-510">CORS 規範還規定,如果標頭存在`"*"``Access-Control-Allow-Credentials`, 則將原點設置為(所有源)無效。</span><span class="sxs-lookup"><span data-stu-id="15899-510">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="15899-511">預先要求</span><span class="sxs-lookup"><span data-stu-id="15899-511">Preflight requests</span></span>

<span data-ttu-id="15899-512">對於某些 CORS 請求,瀏覽器在發出實際請求之前發送其他請求。</span><span class="sxs-lookup"><span data-stu-id="15899-512">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="15899-513">此要求為*預先要求*。</span><span class="sxs-lookup"><span data-stu-id="15899-513">This request is called a *preflight request*.</span></span> <span data-ttu-id="15899-514">如果以下條件為 true,瀏覽器可以跳過預檢請求:</span><span class="sxs-lookup"><span data-stu-id="15899-514">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="15899-515">請求方法是 GET、頭或 POST。</span><span class="sxs-lookup"><span data-stu-id="15899-515">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="15899-516">套用不設定`Accept`要求 標頭以外`Accept-Language``Content-Language``Content-Type`的`Last-Event-ID`、、、、 或 。</span><span class="sxs-lookup"><span data-stu-id="15899-516">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="15899-517">標頭`Content-Type`(如果已設定)具有以下值之一:</span><span class="sxs-lookup"><span data-stu-id="15899-517">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="15899-518">為用戶端請求設置的請求標頭規則適用於應用通過調用`setRequestHeader`物件設置`XMLHttpRequest`的 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-518">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="15899-519">CORS 規範呼叫這些標頭*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="15899-519">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="15899-520">這個規則不適用於瀏覽器可以設定的標頭,例如`User-Agent` `Host` `Content-Length` 。</span><span class="sxs-lookup"><span data-stu-id="15899-520">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="15899-521">以下是預檢要求的範例:</span><span class="sxs-lookup"><span data-stu-id="15899-521">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="15899-522">飛行前請求使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="15899-522">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="15899-523">它包括兩個特殊的標頭:</span><span class="sxs-lookup"><span data-stu-id="15899-523">It includes two special headers:</span></span>

* <span data-ttu-id="15899-524">`Access-Control-Request-Method`:將用於實際請求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="15899-524">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="15899-525">`Access-Control-Request-Headers`:應用在實際請求上設置的請求標頭的清單。</span><span class="sxs-lookup"><span data-stu-id="15899-525">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="15899-526">如前所述,這不包括瀏覽器設定的標頭,如`User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="15899-526">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<!-- I think this needs to be removed, was put here accidently -->

<span data-ttu-id="15899-527">當使用適當的策略啟用 CORS 時,ASP.NET核心通常會自動回應 CORS 預檢請求。</span><span class="sxs-lookup"><span data-stu-id="15899-527">When CORS is enabled with the appropriate policy, ASP.NET Core generally automatically responds to CORS preflight requests.</span></span> <span data-ttu-id="15899-528">[有關預檢要求,請參閱 [HttpOptions] 屬性](#pro)。</span><span class="sxs-lookup"><span data-stu-id="15899-528">See [[HttpOptions] attribute for preflight requests](#pro).</span></span>

<span data-ttu-id="15899-529">CORS 預檢請求可能包含一`Access-Control-Request-Headers`個 標頭,該標頭向伺服器指示與實際請求一起發送的標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-529">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="15899-530">要允許特定標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span><span class="sxs-lookup"><span data-stu-id="15899-530">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="15899-531">要允許所有作者要求標頭,請呼叫<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span><span class="sxs-lookup"><span data-stu-id="15899-531">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="15899-532">流覽器`Access-Control-Request-Headers`的設置 方式並不完全一致。</span><span class="sxs-lookup"><span data-stu-id="15899-532">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="15899-533">如果將標頭設置為 (或`"*"`<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>使用 ) 以外的任何內容,則`Accept`至少`Content-Type``Origin`應包括和 ,以及要支援的任何自定義標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-533">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="15899-534">以下是對預檢請求的範例回應(假設伺服器允許該請求):</span><span class="sxs-lookup"><span data-stu-id="15899-534">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="15899-535">回應包括列出`Access-Control-Allow-Methods`允許的方法的標頭,並選擇一`Access-Control-Allow-Headers`個 標頭,其中列出了允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-535">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="15899-536">如果預檢請求成功,瀏覽器將發送實際請求。</span><span class="sxs-lookup"><span data-stu-id="15899-536">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="15899-537">如果預檢請求被拒絕,應用將返回*200 OK*回應,但不會將 CORS 標頭髮送回。</span><span class="sxs-lookup"><span data-stu-id="15899-537">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="15899-538">因此,瀏覽器不會嘗試跨源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-538">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="15899-539">設定預先過期時間</span><span class="sxs-lookup"><span data-stu-id="15899-539">Set the preflight expiration time</span></span>

<span data-ttu-id="15899-540">標頭`Access-Control-Max-Age`指定對預檢請求的回應可以緩存多長時間。</span><span class="sxs-lookup"><span data-stu-id="15899-540">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="15899-541">要設定這個標頭,請<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>呼叫 :</span><span class="sxs-lookup"><span data-stu-id="15899-541">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="15899-542">CORS 的工作原理</span><span class="sxs-lookup"><span data-stu-id="15899-542">How CORS works</span></span>

<span data-ttu-id="15899-543">本節介紹在 HTTP 消息級別發生的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)請求中發生的情況。</span><span class="sxs-lookup"><span data-stu-id="15899-543">This section describes what happens in a [CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="15899-544">CORS**不是**一個安全功能。</span><span class="sxs-lookup"><span data-stu-id="15899-544">CORS is **not** a security feature.</span></span> <span data-ttu-id="15899-545">CORS 是一種 W3C 標準,允許伺服器放鬆同源策略。</span><span class="sxs-lookup"><span data-stu-id="15899-545">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="15899-546">例如,惡意參與者可以使用[防止跨網站腳本 (XSS)](xref:security/cross-site-scripting)攻擊您的網站,並對其啟用 CORS 的網站執行跨網站請求以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="15899-546">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="15899-547">通過允許 CORS,您的 API 並不更安全。</span><span class="sxs-lookup"><span data-stu-id="15899-547">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="15899-548">由用戶端(瀏覽器)來強制實施 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-548">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="15899-549">伺服器執行請求並返回回應,返回錯誤的是用戶端並阻止回應。</span><span class="sxs-lookup"><span data-stu-id="15899-549">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="15899-550">例如,以下任何工具將顯示伺服器回應:</span><span class="sxs-lookup"><span data-stu-id="15899-550">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="15899-551">Fiddler</span><span class="sxs-lookup"><span data-stu-id="15899-551">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="15899-552">Postman</span><span class="sxs-lookup"><span data-stu-id="15899-552">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="15899-553">.NET HTTPClient</span><span class="sxs-lookup"><span data-stu-id="15899-553">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="15899-554">通過在位址列中輸入 URL 來訪問 Web 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="15899-554">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="15899-555">這是伺服器允許瀏覽器執行跨源[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API)請求的一種方式,否則將禁止這樣做。</span><span class="sxs-lookup"><span data-stu-id="15899-555">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="15899-556">瀏覽器(沒有 CORS)不能執行交叉源請求。</span><span class="sxs-lookup"><span data-stu-id="15899-556">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="15899-557">在 CORS 之前[,JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)曾用於規避此限制。</span><span class="sxs-lookup"><span data-stu-id="15899-557">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="15899-558">JSONP 不使用 XHR,`<script>`它使用 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="15899-558">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="15899-559">允許跨源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="15899-559">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="15899-560">[CORS 規範](https://www.w3.org/TR/cors/)引入了幾個支援跨源請求的新 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-560">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="15899-561">如果瀏覽器支援 CORS,它將自動為跨源請求設置這些標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-561">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="15899-562">啟用 CORS 不需要自訂 JavaScript 代碼。</span><span class="sxs-lookup"><span data-stu-id="15899-562">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="15899-563">下面是跨源請求的示例。</span><span class="sxs-lookup"><span data-stu-id="15899-563">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="15899-564">標頭`Origin`提供發出請求的網站的域。</span><span class="sxs-lookup"><span data-stu-id="15899-564">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="15899-565">標頭`Origin`是必需的,並且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="15899-565">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="15899-566">如果伺服器允許請求,它將在`Access-Control-Allow-Origin`回應中設置標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-566">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="15899-567">此標頭的值與請求中的`Origin`標頭匹配,或者是通配符`"*"`值 ,這意味著允許任何源:</span><span class="sxs-lookup"><span data-stu-id="15899-567">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="15899-568">如果回應不包括標頭,`Access-Control-Allow-Origin`則跨源請求將失敗。</span><span class="sxs-lookup"><span data-stu-id="15899-568">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="15899-569">具體來說,瀏覽器不允許請求。</span><span class="sxs-lookup"><span data-stu-id="15899-569">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="15899-570">即使伺服器返回成功的回應,瀏覽器也不會使回應對用戶端應用可用。</span><span class="sxs-lookup"><span data-stu-id="15899-570">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="15899-571">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-571">Test CORS</span></span>

<span data-ttu-id="15899-572">測試 CORS:</span><span class="sxs-lookup"><span data-stu-id="15899-572">To test CORS:</span></span>

1. <span data-ttu-id="15899-573">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="15899-573">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="15899-574">或您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="15899-574">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="15899-575">使用本文件中的一種方法啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="15899-575">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="15899-576">例如：</span><span class="sxs-lookup"><span data-stu-id="15899-576">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="15899-577">`WithOrigins("https://localhost:<port>");`應僅用於測試類似於[下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)的範例應用。</span><span class="sxs-lookup"><span data-stu-id="15899-577">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="15899-578">創建 Web 應用專案(剃鬚頁面或 MVC)。</span><span class="sxs-lookup"><span data-stu-id="15899-578">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="15899-579">該示例使用剃刀頁。</span><span class="sxs-lookup"><span data-stu-id="15899-579">The sample uses Razor Pages.</span></span> <span data-ttu-id="15899-580">您可以在與 API 專案相同的解決方案中創建 Web 應用。</span><span class="sxs-lookup"><span data-stu-id="15899-580">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="15899-581">將以下突顯的代碼加入*到 Index.cshtml*檔:</span><span class="sxs-lookup"><span data-stu-id="15899-581">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="15899-582">在前面的代碼中,替換為`url: 'https://<web app>.azurewebsites.net/api/values/1',`已部署應用的 URL。</span><span class="sxs-lookup"><span data-stu-id="15899-582">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="15899-583">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="15899-583">Deploy the API project.</span></span> <span data-ttu-id="15899-584">例如,[部署到 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="15899-584">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="15899-585">從桌面運行剃刀頁面或 MVC 應用,然後單擊 **「測試**」按鈕。</span><span class="sxs-lookup"><span data-stu-id="15899-585">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="15899-586">使用 F12 工具檢視錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="15899-586">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="15899-587">從`WithOrigins`中刪除本地主機源並部署應用。</span><span class="sxs-lookup"><span data-stu-id="15899-587">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="15899-588">或者,使用其他埠運行客戶端應用。</span><span class="sxs-lookup"><span data-stu-id="15899-588">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="15899-589">例如,從可視化工作室運行。</span><span class="sxs-lookup"><span data-stu-id="15899-589">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="15899-590">使用用戶端應用進行測試。</span><span class="sxs-lookup"><span data-stu-id="15899-590">Test with the client app.</span></span> <span data-ttu-id="15899-591">CORS 失敗傳回錯誤,但錯誤消息對 JavaScript 不可用。</span><span class="sxs-lookup"><span data-stu-id="15899-591">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="15899-592">使用 F12 工具中的主控台選項卡檢視錯誤。</span><span class="sxs-lookup"><span data-stu-id="15899-592">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="15899-593">根據瀏覽器的不同,您會收到類似於以下內容的錯誤(在 F12 工具控制台中):</span><span class="sxs-lookup"><span data-stu-id="15899-593">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="15899-594">使用微軟邊緣:</span><span class="sxs-lookup"><span data-stu-id="15899-594">Using Microsoft Edge:</span></span>

     <span data-ttu-id="15899-595">**SEC7120: [CORS]`https://localhost:44375`在`https://localhost:44375`「訪問 -控制-允許-原點」回應標頭中找不到跨源資源`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="15899-595">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="15899-596">使用鉻:</span><span class="sxs-lookup"><span data-stu-id="15899-596">Using Chrome:</span></span>

     <span data-ttu-id="15899-597">**CORS 策略阻止了`https://webapi.azurewebsites.net/api/values/1`從`https://localhost:44375`源 處對 XMLHTTPRequest 的訪問:請求的資源上不存在"訪問-控制-允許源"標頭。**</span><span class="sxs-lookup"><span data-stu-id="15899-597">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="15899-598">支援 CORS 的終結點可以使用工具進行測試,例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)。</span><span class="sxs-lookup"><span data-stu-id="15899-598">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="15899-599">使用工具時,`Origin`標頭指定的請求的來源必須不同於接收請求的主機。</span><span class="sxs-lookup"><span data-stu-id="15899-599">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="15899-600">如果要求不是基於標頭的值*的跨源*: `Origin`</span><span class="sxs-lookup"><span data-stu-id="15899-600">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="15899-601">CORS 中間件無需處理請求。</span><span class="sxs-lookup"><span data-stu-id="15899-601">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="15899-602">回應中未返回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="15899-602">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="15899-603">IIS 的 CORS</span><span class="sxs-lookup"><span data-stu-id="15899-603">CORS in IIS</span></span>

<span data-ttu-id="15899-604">部署到IIS時,如果伺服器未配置為允許匿名訪問,則CORS必須在Windows身份驗證之前運行。</span><span class="sxs-lookup"><span data-stu-id="15899-604">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="15899-605">為了支援此專案,需要為應用程式安裝和設定[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="15899-605">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="15899-606">其他資源</span><span class="sxs-lookup"><span data-stu-id="15899-606">Additional resources</span></span>

* [<span data-ttu-id="15899-607">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="15899-607">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="15899-608">開始使用 IIS CORS 模組</span><span class="sxs-lookup"><span data-stu-id="15899-608">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end
