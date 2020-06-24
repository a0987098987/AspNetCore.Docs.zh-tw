---
title: 啟用 ASP.NET Core 中的跨原始來源要求（CORS）
author: rick-anderson
description: 瞭解 CORS 如何作為標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始來源要求。
ms.author: riande
ms.custom: mvc
ms.date: 04/17/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/cors
ms.openlocfilehash: 1a52a2425eeba2bc62253e96fe6d2465562c154e
ms.sourcegitcommit: 5e462c3328c70f95969d02adce9c71592049f54c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85292759"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="b5978-103">啟用 ASP.NET Core 中的跨原始來源要求（CORS）</span><span class="sxs-lookup"><span data-stu-id="b5978-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b5978-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="b5978-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="b5978-105">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="b5978-106">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="b5978-107">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="b5978-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="b5978-108">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="b5978-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="b5978-109">有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-109">Sometimes, you might want to allow other sites to make cross-origin requests to your app.</span></span> <span data-ttu-id="b5978-110">如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="b5978-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="b5978-111">[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：</span><span class="sxs-lookup"><span data-stu-id="b5978-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="b5978-112">是一種 W3C 標準，可讓伺服器放寬相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="b5978-113">**不**是安全性功能，CORS 放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="b5978-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="b5978-114">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="b5978-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="b5978-115">如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="b5978-116">允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="b5978-117">比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="b5978-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="b5978-118">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="b5978-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="b5978-119">相同的來源</span><span class="sxs-lookup"><span data-stu-id="b5978-119">Same origin</span></span>

<span data-ttu-id="b5978-120">如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。</span><span class="sxs-lookup"><span data-stu-id="b5978-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="b5978-121">這兩個 Url 具有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="b5978-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="b5978-122">這些 Url 的來源不同于前兩個 Url：</span><span class="sxs-lookup"><span data-stu-id="b5978-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="b5978-123">`https://example.net`：不同的網域</span><span class="sxs-lookup"><span data-stu-id="b5978-123">`https://example.net`: Different domain</span></span>
* <span data-ttu-id="b5978-124">`https://www.example.com/foo.html`：不同的子域</span><span class="sxs-lookup"><span data-stu-id="b5978-124">`https://www.example.com/foo.html`: Different subdomain</span></span>
* <span data-ttu-id="b5978-125">`http://example.com/foo.html`：不同的配置</span><span class="sxs-lookup"><span data-stu-id="b5978-125">`http://example.com/foo.html`: Different scheme</span></span>
* <span data-ttu-id="b5978-126">`https://example.com:9000/foo.html`：不同的埠</span><span class="sxs-lookup"><span data-stu-id="b5978-126">`https://example.com:9000/foo.html`: Different port</span></span>

## <a name="enable-cors"></a><span data-ttu-id="b5978-127">啟用 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-127">Enable CORS</span></span>

<span data-ttu-id="b5978-128">有三種方式可啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="b5978-128">There are three ways to enable CORS:</span></span>

* <span data-ttu-id="b5978-129">在中介軟體中，使用[已命名的原則](#np)或[預設原則](#dp)。</span><span class="sxs-lookup"><span data-stu-id="b5978-129">In middleware using a [named policy](#np) or [default policy](#dp).</span></span>
* <span data-ttu-id="b5978-130">使用[端點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-130">Using [endpoint routing](#ecors).</span></span>
* <span data-ttu-id="b5978-131">具有[[EnableCors]](#attr)屬性的。</span><span class="sxs-lookup"><span data-stu-id="b5978-131">With the [[EnableCors]](#attr) attribute.</span></span>

<span data-ttu-id="b5978-132">將[[EnableCors]](#attr)屬性與已命名的原則搭配使用，可在限制支援 CORS 的端點時提供最精細的控制。</span><span class="sxs-lookup"><span data-stu-id="b5978-132">Using the [[EnableCors]](#attr) attribute with a named policy provides the finest control in limiting endpoints that support CORS.</span></span>

> [!WARNING]
> <span data-ttu-id="b5978-133"><xref:Owin.CorsExtensions.UseCors%2A>在使用之前，必須先呼叫 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A> `UseResponseCaching` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-133"><xref:Owin.CorsExtensions.UseCors%2A> must be called before <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A> when using `UseResponseCaching`.</span></span>

<span data-ttu-id="b5978-134">下列各節將詳細說明每一種方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-134">Each approach is detailed in the following sections.</span></span>

<a name="np"></a>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="b5978-135">具有已命名原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-135">CORS with named policy and middleware</span></span>

<span data-ttu-id="b5978-136">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-136">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="b5978-137">下列程式碼會將 CORS 原則套用至具有指定來源的所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="b5978-137">The following code applies a CORS policy to all the app's endpoints with the specified origins:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=3,9,32)]

<span data-ttu-id="b5978-138">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5978-138">The preceding code:</span></span>

* <span data-ttu-id="b5978-139">將原則名稱設定為 `_myAllowSpecificOrigins` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-139">Sets the policy name to `_myAllowSpecificOrigins`.</span></span> <span data-ttu-id="b5978-140">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="b5978-140">The policy name is arbitrary.</span></span>
* <span data-ttu-id="b5978-141">呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，並指定 `_myAllowSpecificOrigins` CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-141">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method and specifies the  `_myAllowSpecificOrigins` CORS policy.</span></span> <span data-ttu-id="b5978-142">`UseCors`新增 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b5978-142">`UseCors` adds the CORS middleware.</span></span> <span data-ttu-id="b5978-143">對的呼叫 `UseCors` 必須放在之後 `UseRouting` （但之前） `UseAuthorization` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-143">The call to `UseCors` must be placed after `UseRouting`, but before `UseAuthorization`.</span></span> <span data-ttu-id="b5978-144">如需詳細資訊，請參閱[中介軟體順序](xref:fundamentals/middleware/index#middleware-order)。</span><span class="sxs-lookup"><span data-stu-id="b5978-144">For more information, see [Middleware order](xref:fundamentals/middleware/index#middleware-order).</span></span>
* <span data-ttu-id="b5978-145"><xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的呼叫。</span><span class="sxs-lookup"><span data-stu-id="b5978-145">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="b5978-146">Lambda 會接受 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。</span><span class="sxs-lookup"><span data-stu-id="b5978-146">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="b5978-147">[Configuration options](#cors-policy-options) `WithOrigins` 這篇文章稍後會說明設定選項，例如。</span><span class="sxs-lookup"><span data-stu-id="b5978-147">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>
* <span data-ttu-id="b5978-148">啟用 `_myAllowSpecificOrigins` 所有控制器端點的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-148">Enables the `_myAllowSpecificOrigins` CORS policy for all controller endpoints.</span></span> <span data-ttu-id="b5978-149">請參閱[端點路由](#ecors)以將 CORS 原則套用至特定端點。</span><span class="sxs-lookup"><span data-stu-id="b5978-149">See [endpoint routing](#ecors) to apply a CORS policy to specific endpoints.</span></span>
* <span data-ttu-id="b5978-150">使用回應快取[中介軟體](xref:performance/caching/middleware)時，請 <xref:Owin.CorsExtensions.UseCors%2A> 在之前呼叫 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A> 。</span><span class="sxs-lookup"><span data-stu-id="b5978-150">When using [Response Caching Middleware](xref:performance/caching/middleware), call <xref:Owin.CorsExtensions.UseCors%2A> before <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A>.</span></span>

<span data-ttu-id="b5978-151">使用端點路由，**必須**將 CORS 中介軟體設定為在呼叫和之間執行 `UseRouting` `UseEndpoints` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-151">With endpoint routing, the CORS middleware **must** be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span>

<span data-ttu-id="b5978-152">如需測試程式碼的相關指示，請參閱[測試 CORS](#testc) ，類似于上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="b5978-152">See [Test CORS](#testc) for instructions on testing code similar to the preceding code.</span></span>

<span data-ttu-id="b5978-153"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將 CORS 服務新增至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="b5978-153">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="b5978-154">如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="b5978-154">For more information, see [CORS policy options](#cpo) in this document.</span></span>

<span data-ttu-id="b5978-155"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以連結，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="b5978-155">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> methods can be chained, as shown in the following code:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup2.cs?name=snippet)]

<span data-ttu-id="b5978-156">注意：指定的 URL**不**能包含尾端斜線（ `/` ）。</span><span class="sxs-lookup"><span data-stu-id="b5978-156">Note: The specified URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="b5978-157">如果 URL 以結束 `/` ，則比較會 `false` 傳回，而且不會傳回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-157">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="dp"></a>

### <a name="cors-with-default-policy-and-middleware"></a><span data-ttu-id="b5978-158">具有預設原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-158">CORS with default policy and middleware</span></span>

<span data-ttu-id="b5978-159">下列反白顯示的程式碼會啟用預設 CORS 原則：</span><span class="sxs-lookup"><span data-stu-id="b5978-159">The following highlighted code enables the default CORS policy:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupDefaultPolicy.cs?name=snippet2&highlight=7,29)]

<span data-ttu-id="b5978-160">上述程式碼會將預設 CORS 原則套用至所有控制器端點。</span><span class="sxs-lookup"><span data-stu-id="b5978-160">The preceding code applies the default CORS policy to all controller endpoints.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="b5978-161">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="b5978-161">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="b5978-162">使用時，以每個端點為基礎啟用 CORS `RequireCors` ，目前**不**支援[自動預檢要求](#apf)。</span><span class="sxs-lookup"><span data-stu-id="b5978-162">Enabling CORS on a per-endpoint basis using `RequireCors` currently does **not** support [automatic preflight requests](#apf).</span></span> <span data-ttu-id="b5978-163">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/aspnetcore/issues/20709)和[使用端點路由測試 CORS 和 [HttpOptions]](#tcer)。</span><span class="sxs-lookup"><span data-stu-id="b5978-163">For more information, see [this GitHub issue](https://github.com/dotnet/aspnetcore/issues/20709) and [Test CORS with endpoint routing and [HttpOptions]](#tcer).</span></span>

<span data-ttu-id="b5978-164">使用端點路由，可以使用一組擴充方法，針對每個端點來啟用 CORS <xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-164">With endpoint routing, CORS can be enabled on a per-endpoint basis using the <xref:Microsoft.AspNetCore.Builder.CorsEndpointConventionBuilderExtensions.RequireCors*> set of extension methods:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPt.cs?name=snippet2&highlight=3,7-15,32,40,43)]

<span data-ttu-id="b5978-165">在上述程式碼中：</span><span class="sxs-lookup"><span data-stu-id="b5978-165">In the preceding code:</span></span>

* <span data-ttu-id="b5978-166">`app.UseCors`啟用 CORS 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b5978-166">`app.UseCors` enables the CORS middleware.</span></span> <span data-ttu-id="b5978-167">因為尚未設定預設原則，所以 `app.UseCors()` 單獨不會啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-167">Because a default policy hasn't been configured, `app.UseCors()` alone doesn't enable CORS.</span></span>
* <span data-ttu-id="b5978-168">`/echo`和控制器端點允許使用指定的原則進行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-168">The `/echo` and controller endpoints allow cross-origin requests using the specified policy.</span></span>
* <span data-ttu-id="b5978-169">`/echo2`和 Razor 頁面端點**不**允許跨原始來源要求，因為未指定預設原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-169">The `/echo2` and Razor Pages endpoints do **not** allow cross-origin requests because no default policy was specified.</span></span>

<span data-ttu-id="b5978-170">[[DisableCors]](#dc) **屬性不會停用**端點路由所啟用的 CORS `RequireCors` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-170">The [[DisableCors]](#dc) attribute does **not**  disable CORS that has been enabled by endpoint routing with `RequireCors`.</span></span>

<span data-ttu-id="b5978-171">如需測試類似上述程式碼的指示，請參閱[使用端點路由和 [HttpOptions] 測試 CORS](#tcer) 。</span><span class="sxs-lookup"><span data-stu-id="b5978-171">See [Test CORS with endpoint routing and [HttpOptions]](#tcer) for instructions on testing code similar to the preceding.</span></span>

<a name="attr"></a>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="b5978-172">啟用具有屬性的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-172">Enable CORS with attributes</span></span>

<span data-ttu-id="b5978-173">使用[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性啟用 CORS，並將已命名的原則套用至只有需要 CORS 的端點可提供最精細的控制。</span><span class="sxs-lookup"><span data-stu-id="b5978-173">Enabling CORS with the [[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute and applying a named policy to only those endpoints that require CORS provides the finest control.</span></span>

<span data-ttu-id="b5978-174">[[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-174">The [[EnableCors]](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="b5978-175">`[EnableCors]`屬性會啟用所選端點的 CORS，而不是所有端點：</span><span class="sxs-lookup"><span data-stu-id="b5978-175">The `[EnableCors]` attribute enables CORS for selected endpoints, rather than all endpoints:</span></span>

* <span data-ttu-id="b5978-176">`[EnableCors]`指定預設原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-176">`[EnableCors]` specifies the default policy.</span></span>
* <span data-ttu-id="b5978-177">`[EnableCors("{Policy String}")]`指定已命名的原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-177">`[EnableCors("{Policy String}")]` specifies a named policy.</span></span>

<span data-ttu-id="b5978-178">`[EnableCors]`屬性可套用至：</span><span class="sxs-lookup"><span data-stu-id="b5978-178">The `[EnableCors]` attribute can be applied to:</span></span>

* Razor<span data-ttu-id="b5978-179">本頁`PageModel`</span><span class="sxs-lookup"><span data-stu-id="b5978-179"> Page `PageModel`</span></span>
* <span data-ttu-id="b5978-180">控制器</span><span class="sxs-lookup"><span data-stu-id="b5978-180">Controller</span></span>
* <span data-ttu-id="b5978-181">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="b5978-181">Controller action method</span></span>

<span data-ttu-id="b5978-182">您可以使用屬性，將不同的原則套用至控制器、頁面模型或動作方法 `[EnableCors]` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-182">Different policies can be applied to controllers, page models, or action methods with the `[EnableCors]` attribute.</span></span> <span data-ttu-id="b5978-183">當 `[EnableCors]` 屬性套用至控制器、頁面模型或動作方法，而且在中介軟體中啟用 CORS 時，會套用**這兩個**原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-183">When the `[EnableCors]` attribute is applied to a controller, page model, or action method, and CORS is enabled in middleware, **both** policies are applied.</span></span> <span data-ttu-id="b5978-184">**我們建議您不要結合原則。使用** `[EnableCors]` **屬性或中介軟體，而不是同時在相同的應用程式中。**</span><span class="sxs-lookup"><span data-stu-id="b5978-184">**We recommend against combining policies. Use the** `[EnableCors]` **attribute or middleware, not both in the same app.**</span></span>

<span data-ttu-id="b5978-185">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="b5978-185">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="b5978-186">下列程式碼會建立兩個 CORS 原則：</span><span class="sxs-lookup"><span data-stu-id="b5978-186">The following code creates two CORS policies:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Startup3.cs?name=snippet&highlight=12-28,44)]

<span data-ttu-id="b5978-187">針對限制 CORS 要求的最精細控制：</span><span class="sxs-lookup"><span data-stu-id="b5978-187">For the finest control of limiting CORS requests:</span></span>

* <span data-ttu-id="b5978-188">使用 `[EnableCors("MyPolicy")]` 具有已命名原則的。</span><span class="sxs-lookup"><span data-stu-id="b5978-188">Use `[EnableCors("MyPolicy")]` with a named policy.</span></span>
* <span data-ttu-id="b5978-189">不要定義預設原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-189">Don't define a default policy.</span></span>
* <span data-ttu-id="b5978-190">請勿使用[端點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-190">Don't use [endpoint routing](#ecors).</span></span>

<span data-ttu-id="b5978-191">下一節中的程式碼會符合前述清單。</span><span class="sxs-lookup"><span data-stu-id="b5978-191">The code in the next section meets the preceding list.</span></span>

<span data-ttu-id="b5978-192">如需測試程式碼的相關指示，請參閱[測試 CORS](#testc) ，類似于上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="b5978-192">See [Test CORS](#testc) for instructions on testing code similar to the preceding code.</span></span>

<a name="dc"></a>

### <a name="disable-cors"></a><span data-ttu-id="b5978-193">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-193">Disable CORS</span></span>

<span data-ttu-id="b5978-194">[[DisableCors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) **屬性不會停用**[端點路由](#ecors)已啟用的 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-194">The [[DisableCors]](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute does **not**  disable CORS that has been enabled by [endpoint routing](#ecors).</span></span>

<span data-ttu-id="b5978-195">下列程式碼會定義 CORS 原則 `"MyPolicy"` ：</span><span class="sxs-lookup"><span data-stu-id="b5978-195">The following code defines the CORS policy `"MyPolicy"`:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTestMyPolicy.cs?name=snippet)]

<span data-ttu-id="b5978-196">下列程式碼會停用動作的 CORS `GetValues2` ：</span><span class="sxs-lookup"><span data-stu-id="b5978-196">The following code disables CORS for the `GetValues2` action:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet&highlight=1,23)]

<span data-ttu-id="b5978-197">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5978-197">The preceding code:</span></span>

* <span data-ttu-id="b5978-198">不會使用[端點路由](#ecors)來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-198">Doesn't enable CORS with [endpoint routing](#ecors).</span></span>
* <span data-ttu-id="b5978-199">不會定義[預設的 CORS 原則](#dp)。</span><span class="sxs-lookup"><span data-stu-id="b5978-199">Doesn't define a [default CORS policy](#dp).</span></span>
* <span data-ttu-id="b5978-200">使用[[EnableCors （"MyPolicy"）]](#attr)來啟用 `"MyPolicy"` 控制器的 CORS 原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-200">Uses [[EnableCors("MyPolicy")]](#attr) to enable the `"MyPolicy"` CORS policy for the controller.</span></span>
* <span data-ttu-id="b5978-201">停用方法的 CORS `GetValues2` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-201">Disables CORS for the `GetValues2` method.</span></span>

<span data-ttu-id="b5978-202">如需測試上述程式碼的指示，請參閱[測試 CORS](#testc) 。</span><span class="sxs-lookup"><span data-stu-id="b5978-202">See [Test CORS](#testc) for instructions on testing the preceding code.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="b5978-203">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="b5978-203">CORS policy options</span></span>

<span data-ttu-id="b5978-204">本節說明可在 CORS 原則中設定的各種選項：</span><span class="sxs-lookup"><span data-stu-id="b5978-204">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="b5978-205">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="b5978-205">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="b5978-206">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="b5978-206">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="b5978-207">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-207">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="b5978-208">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-208">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="b5978-209">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="b5978-209">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="b5978-210">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="b5978-210">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="b5978-211"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>會在中呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-211"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b5978-212">針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="b5978-212">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="b5978-213">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="b5978-213">Set the allowed origins</span></span>

<span data-ttu-id="b5978-214"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>：允許使用任何配置（或）來自所有來源的 CORS 要求 `http` `https` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-214"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="b5978-215">`AllowAnyOrigin`不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-215">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="b5978-216">指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，而且可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-216">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="b5978-217">當應用程式已設定這兩種方法時，CORS 服務會傳回不正確 CORS 回應。</span><span class="sxs-lookup"><span data-stu-id="b5978-217">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="b5978-218">`AllowAnyOrigin`會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-218">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="b5978-219">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-219">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="b5978-220"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>：將原則的屬性設定為函式 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> ，以便在評估是否允許來源時，允許來源符合已設定的萬用字元網域。</span><span class="sxs-lookup"><span data-stu-id="b5978-220"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>: Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="b5978-221">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="b5978-221">Set the allowed HTTP methods</span></span>

<span data-ttu-id="b5978-222"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="b5978-222"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="b5978-223">允許任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="b5978-223">Allows any HTTP method:</span></span>
* <span data-ttu-id="b5978-224">會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-224">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="b5978-225">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-225">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="b5978-226">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-226">Set the allowed request headers</span></span>

<span data-ttu-id="b5978-227">若要允許在 CORS 要求中傳送特定標頭（稱為[author 要求標頭](https://xhr.spec.whatwg.org/#request)），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="b5978-227">To allow specific headers to be sent in a CORS request, called [author request headers](https://xhr.spec.whatwg.org/#request), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

<span data-ttu-id="b5978-228">若要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-228">To allow all [author request headers](https://www.w3.org/TR/cors/#author-request-headers), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

<span data-ttu-id="b5978-229">`AllowAnyHeader`會影響預檢要求和[存取控制要求](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)標頭標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-229">`AllowAnyHeader` affects preflight requests and the [Access-Control-Request-Headers](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method) header.</span></span> <span data-ttu-id="b5978-230">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-230">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="b5978-231">`WithHeaders`只有在 `Access-Control-Request-Headers` 完全符合中所述標頭的情況下傳送的標頭時，才可以將 CORS 中介軟體原則與指定的特定標頭比對 `WithHeaders` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-231">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="b5978-232">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="b5978-232">For instance, consider an app configured as follows:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet4)]

<span data-ttu-id="b5978-233">CORS 中介軟體會拒絕具有下列要求標頭的預檢要求，因為 `Content-Language` （[HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)）未列于 `WithHeaders` ：</span><span class="sxs-lookup"><span data-stu-id="b5978-233">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="b5978-234">應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-234">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="b5978-235">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-235">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="b5978-236">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-236">Set the exposed response headers</span></span>

<span data-ttu-id="b5978-237">根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-237">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="b5978-238">如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="b5978-238">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="b5978-239">預設可用的回應標頭為：</span><span class="sxs-lookup"><span data-stu-id="b5978-239">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="b5978-240">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="b5978-240">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="b5978-241">若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-241">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet5)]
### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="b5978-242">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="b5978-242">Credentials in cross-origin requests</span></span>

<span data-ttu-id="b5978-243">認證需要在 CORS 要求中進行特殊處理。</span><span class="sxs-lookup"><span data-stu-id="b5978-243">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="b5978-244">根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="b5978-244">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="b5978-245">認證包括 cookie 和 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="b5978-245">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="b5978-246">若要使用跨原始來源要求傳送認證，用戶端必須將設 `XMLHttpRequest.withCredentials` 為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-246">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="b5978-247">`XMLHttpRequest`直接使用：</span><span class="sxs-lookup"><span data-stu-id="b5978-247">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="b5978-248">使用 jQuery：</span><span class="sxs-lookup"><span data-stu-id="b5978-248">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="b5978-249">使用[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)：</span><span class="sxs-lookup"><span data-stu-id="b5978-249">Using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="b5978-250">伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="b5978-250">The server must allow the credentials.</span></span> <span data-ttu-id="b5978-251">若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-251">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet6)]

<span data-ttu-id="b5978-252">HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="b5978-252">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="b5978-253">如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="b5978-253">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="b5978-254">允許跨原始來源認證會有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="b5978-254">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="b5978-255">另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。</span><span class="sxs-lookup"><span data-stu-id="b5978-255">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="b5978-256">CORS 規格也會指出如果標頭存在，將原始來源設定為 `"*"` （所有來源）是不正確 `Access-Control-Allow-Credentials` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-256">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

<a name="pref"></a>

## <a name="preflight-requests"></a><span data-ttu-id="b5978-257">預檢要求</span><span class="sxs-lookup"><span data-stu-id="b5978-257">Preflight requests</span></span>

<span data-ttu-id="b5978-258">針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的[選項](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-258">For some CORS requests, the browser sends an additional [OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS) request before making the actual request.</span></span> <span data-ttu-id="b5978-259">此要求稱為[預檢要求](https://developer.mozilla.org/docs/Glossary/Preflight_request)。</span><span class="sxs-lookup"><span data-stu-id="b5978-259">This request is called a [preflight request](https://developer.mozilla.org/docs/Glossary/Preflight_request).</span></span> <span data-ttu-id="b5978-260">如果下列**所有**條件都成立，瀏覽器就可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="b5978-260">The browser can skip the preflight request if **all** the following conditions are true:</span></span>

* <span data-ttu-id="b5978-261">要求方法為 GET、HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="b5978-261">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="b5978-262">應用程式不會設定、、、或以外的要求標頭 `Accept` `Accept-Language` `Content-Language` `Content-Type` `Last-Event-ID` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-262">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="b5978-263">`Content-Type`如果設定了標頭，則會有下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="b5978-263">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="b5978-264">針對用戶端要求設定的要求標頭規則會套用至應用程式透過在物件上呼叫所設定的標頭 `setRequestHeader` `XMLHttpRequest` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-264">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="b5978-265">CORS 規格會呼叫這些標頭的[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)。</span><span class="sxs-lookup"><span data-stu-id="b5978-265">The CORS specification calls these headers [author request headers](https://www.w3.org/TR/cors/#author-request-headers).</span></span> <span data-ttu-id="b5978-266">此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent` 、 `Host` 或 `Content-Length` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-266">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="b5978-267">以下是範例回應，類似于此檔的[測試 CORS](#testc)一節中 **[Put test]** 按鈕所提出的預檢要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-267">The following is an example response similar to the preflight request made from the **[Put test]** button in the [Test CORS](#testc) section of this document.</span></span>

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

<span data-ttu-id="b5978-268">預檢要求會使用[HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS)方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-268">The preflight request uses the [HTTP OPTIONS](https://developer.mozilla.org/docs/Web/HTTP/Methods/OPTIONS) method.</span></span> <span data-ttu-id="b5978-269">其中可能包含下列標頭：</span><span class="sxs-lookup"><span data-stu-id="b5978-269">It may include the following headers:</span></span>

* <span data-ttu-id="b5978-270">[存取控制-要求-方法](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method)：將用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-270">[Access-Control-Request-Method](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Request-Method): The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="b5978-271">[存取控制-要求-標頭](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers)：應用程式在實際要求上設定的要求標頭清單。</span><span class="sxs-lookup"><span data-stu-id="b5978-271">[Access-Control-Request-Headers](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Headers): A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="b5978-272">如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-272">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>
* [<span data-ttu-id="b5978-273">存取控制-允許方法</span><span class="sxs-lookup"><span data-stu-id="b5978-273">Access-Control-Allow-Methods</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Methods)

<span data-ttu-id="b5978-274">如果預檢要求遭到拒絕，應用程式會傳回 `200 OK` 回應，但不會設定 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-274">If the preflight request is denied, the app returns a `200 OK` response but doesn't set the CORS headers.</span></span> <span data-ttu-id="b5978-275">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-275">Therefore, the browser doesn't attempt the cross-origin request.</span></span> <span data-ttu-id="b5978-276">如需拒絕預檢要求的範例，請參閱本檔的[測試 CORS](#testc)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-276">For an example of a denied preflight request, see the [Test CORS](#testc) section of this document.</span></span>

<span data-ttu-id="b5978-277">使用 F12 工具時，主控台應用程式會顯示類似下列其中一項的錯誤，視瀏覽器而定：</span><span class="sxs-lookup"><span data-stu-id="b5978-277">Using the F12 tools, the console app shows an error similar to one of the following, depending on the browser:</span></span>

* <span data-ttu-id="b5978-278">Firefox：跨原始來源要求已封鎖：相同的來源原則不允許讀取位於的遠端資源 `https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-278">Firefox: Cross-Origin Request Blocked: The Same Origin Policy disallows reading the remote resource at `https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5`.</span></span> <span data-ttu-id="b5978-279">（原因： CORS 要求未成功）。</span><span class="sxs-lookup"><span data-stu-id="b5978-279">(Reason: CORS request did not succeed).</span></span> [<span data-ttu-id="b5978-280">深入了解</span><span class="sxs-lookup"><span data-stu-id="b5978-280">Learn More</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS/Errors/CORSDidNotSucceed)
* <span data-ttu-id="b5978-281">以 Chromium 為基礎：從來源 ' ' 提取 ' ' 的存取權已 https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5 https://cors3.azurewebsites.net 遭 CORS 原則封鎖：對預檢要求的回應未通過存取控制檢查：要求的資源上沒有任何「存取控制-允許-來源」標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-281">Chromium based: Access to fetch at 'https://cors1.azurewebsites.net/api/TodoItems1/MyDelete2/5' from origin 'https://cors3.azurewebsites.net' has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span> <span data-ttu-id="b5978-282">如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。</span><span class="sxs-lookup"><span data-stu-id="b5978-282">If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</span></span>

<span data-ttu-id="b5978-283">若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-283">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet2)]

<span data-ttu-id="b5978-284">若要允許所有[作者要求標頭](https://www.w3.org/TR/cors/#author-request-headers)，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-284">To allow all [author request headers](https://www.w3.org/TR/cors/#author-request-headers), call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet3)]

<span data-ttu-id="b5978-285">瀏覽器的設定方式並不一致 `Access-Control-Request-Headers` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-285">Browsers aren't consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="b5978-286">如果其中一項：</span><span class="sxs-lookup"><span data-stu-id="b5978-286">If either:</span></span>

* <span data-ttu-id="b5978-287">標頭會設定為以外的任何專案`"*"`</span><span class="sxs-lookup"><span data-stu-id="b5978-287">Headers are set to anything other than `"*"`</span></span>
* <span data-ttu-id="b5978-288"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>會呼叫：至少包含 `Accept` 、 `Content-Type` 和 `Origin` ，再加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-288"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*> is called: Include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<a name="apf"></a>

### <a name="automatic-preflight-request-code"></a><span data-ttu-id="b5978-289">自動預檢要求碼</span><span class="sxs-lookup"><span data-stu-id="b5978-289">Automatic preflight request code</span></span>

<span data-ttu-id="b5978-290">套用 CORS 原則時：</span><span class="sxs-lookup"><span data-stu-id="b5978-290">When the CORS policy is applied either:</span></span>

* <span data-ttu-id="b5978-291">`app.UseCors`在中呼叫以全域方式 `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-291">Globally by calling `app.UseCors` in `Startup.Configure`.</span></span>
* <span data-ttu-id="b5978-292">使用 `[EnableCors]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="b5978-292">Using the `[EnableCors]` attribute.</span></span>

<span data-ttu-id="b5978-293">ASP.NET Core 回應預檢 OPTIONS 要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-293">ASP.NET Core responds to the preflight OPTIONS request.</span></span>

<span data-ttu-id="b5978-294">使用時，以每個端點為基礎啟用 CORS `RequireCors` ，目前**不**支援自動預檢要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-294">Enabling CORS on a per-endpoint basis using `RequireCors` currently does **not** support automatic preflight requests.</span></span>

<span data-ttu-id="b5978-295">本檔的[測試 CORS](#testc)一節會示範此行為。</span><span class="sxs-lookup"><span data-stu-id="b5978-295">The [Test CORS](#testc) section of this document demonstrates this behavior.</span></span>

<a name="pro"></a>

### <a name="httpoptions-attribute-for-preflight-requests"></a><span data-ttu-id="b5978-296">[HttpOptions] 預檢要求的屬性</span><span class="sxs-lookup"><span data-stu-id="b5978-296">[HttpOptions] attribute for preflight requests</span></span>

<span data-ttu-id="b5978-297">以適當的原則啟用 CORS 時，ASP.NET Core 通常會自動回應 CORS 預檢要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-297">When CORS is enabled with the appropriate policy, ASP.NET Core generally responds to CORS preflight requests automatically.</span></span> <span data-ttu-id="b5978-298">在某些情況下，這可能不是這種情況。</span><span class="sxs-lookup"><span data-stu-id="b5978-298">In some scenarios, this may not be the case.</span></span> <span data-ttu-id="b5978-299">例如，搭配使用[CORS 與端點路由](#ecors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-299">For example, using [CORS with endpoint routing](#ecors).</span></span>

<span data-ttu-id="b5978-300">下列程式碼會使用[[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute)屬性來建立選項要求的端點：</span><span class="sxs-lookup"><span data-stu-id="b5978-300">The following code uses the [[HttpOptions]](xref:Microsoft.AspNetCore.Mvc.HttpOptionsAttribute) attribute to create endpoints for OPTIONS requests:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet&highlight=5-17)]

<span data-ttu-id="b5978-301">如需測試上述程式碼的指示，請參閱[使用端點路由和 [HttpOptions] 測試 CORS](#tcer) 。</span><span class="sxs-lookup"><span data-stu-id="b5978-301">See [Test CORS with endpoint routing and [HttpOptions]](#tcer) for instructions on testing the preceding code.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="b5978-302">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="b5978-302">Set the preflight expiration time</span></span>

<span data-ttu-id="b5978-303">`Access-Control-Max-Age`標頭會指定快取回應要求的時間長度。</span><span class="sxs-lookup"><span data-stu-id="b5978-303">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="b5978-304">若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-304">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupAllowSubdomain.cs?name=snippet7)]
<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="b5978-305">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="b5978-305">How CORS works</span></span>

<span data-ttu-id="b5978-306">本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)要求中會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="b5978-306">This section describes what happens in a [CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="b5978-307">CORS**不**是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="b5978-307">CORS is **not** a security feature.</span></span> <span data-ttu-id="b5978-308">CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-308">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="b5978-309">例如，惡意執行者可以對您的網站使用[跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="b5978-309">For example, a malicious actor could use [Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="b5978-310">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="b5978-310">An API isn't safer by allowing CORS.</span></span>
  * <span data-ttu-id="b5978-311">而是由用戶端（瀏覽器）強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-311">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="b5978-312">伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b5978-312">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="b5978-313">例如，下列任何一項工具都會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="b5978-313">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="b5978-314">Fiddler</span><span class="sxs-lookup"><span data-stu-id="b5978-314">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="b5978-315">Postman</span><span class="sxs-lookup"><span data-stu-id="b5978-315">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="b5978-316">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="b5978-316">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="b5978-317">網頁瀏覽器，方法是在網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="b5978-317">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="b5978-318">這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)要求的方式，否則會禁止。</span><span class="sxs-lookup"><span data-stu-id="b5978-318">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="b5978-319">沒有 CORS 的瀏覽器無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-319">Browsers without CORS can't do cross-origin requests.</span></span> <span data-ttu-id="b5978-320">在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。</span><span class="sxs-lookup"><span data-stu-id="b5978-320">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="b5978-321">JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="b5978-321">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="b5978-322">允許跨原始來源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="b5978-322">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="b5978-323">[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-323">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="b5978-324">如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-324">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="b5978-325">不需要自訂 JavaScript 程式碼來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-325">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="b5978-326">已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)上的 [ [PUT test] 按鈕](https://cors3.azurewebsites.net/test)</span><span class="sxs-lookup"><span data-stu-id="b5978-326">The  [PUT test button](https://cors3.azurewebsites.net/test) on the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)</span></span>

<span data-ttu-id="b5978-327">以下是從 [[值](https://cors3.azurewebsites.net/)] [測試] 按鈕到的跨原始來源要求的範例 `https://cors1.azurewebsites.net/api/values` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-327">The following is an example of a cross-origin request from the [Values](https://cors3.azurewebsites.net/) test button to `https://cors1.azurewebsites.net/api/values`.</span></span> <span data-ttu-id="b5978-328">`Origin`標頭：</span><span class="sxs-lookup"><span data-stu-id="b5978-328">The `Origin` header:</span></span>

* <span data-ttu-id="b5978-329">提供提出要求的網站網域。</span><span class="sxs-lookup"><span data-stu-id="b5978-329">Provides the domain of the site that's making the request.</span></span>
* <span data-ttu-id="b5978-330">是必要的，而且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="b5978-330">Is required and must be different from the host.</span></span>

<span data-ttu-id="b5978-331">**一般標頭**</span><span class="sxs-lookup"><span data-stu-id="b5978-331">**General headers**</span></span>

```
Request URL: https://cors1.azurewebsites.net/api/values
Request Method: GET
Status Code: 200 OK
```

<span data-ttu-id="b5978-332">**回應標頭**</span><span class="sxs-lookup"><span data-stu-id="b5978-332">**Response headers**</span></span>

```
Content-Encoding: gzip
Content-Type: text/plain; charset=utf-8
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors1.azurewebsites.net
Transfer-Encoding: chunked
Vary: Accept-Encoding
X-Powered-By: ASP.NET
```

<span data-ttu-id="b5978-333">**要求標頭**</span><span class="sxs-lookup"><span data-stu-id="b5978-333">**Request headers**</span></span>

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

<span data-ttu-id="b5978-334">在 `OPTIONS` 要求中，伺服器會在回應中設定**回應標頭** `Access-Control-Allow-Origin: {allowed origin}` 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-334">In `OPTIONS` requests, the server sets the **Response headers** `Access-Control-Allow-Origin: {allowed origin}` header in the response.</span></span> <span data-ttu-id="b5978-335">例如，已部署的[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) [Delete [EnableCors]](https://cors1.azurewebsites.net/test?number=2)按鈕 `OPTIONS` 要求包含下列標頭：</span><span class="sxs-lookup"><span data-stu-id="b5978-335">For example, the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI), [Delete [EnableCors]](https://cors1.azurewebsites.net/test?number=2) button `OPTIONS` request contains the following  headers:</span></span>

<span data-ttu-id="b5978-336">**一般標頭**</span><span class="sxs-lookup"><span data-stu-id="b5978-336">**General headers**</span></span>

```
Request URL: https://cors3.azurewebsites.net/api/TodoItems2/MyDelete2/5
Request Method: OPTIONS
Status Code: 204 No Content
```

<span data-ttu-id="b5978-337">**回應標頭**</span><span class="sxs-lookup"><span data-stu-id="b5978-337">**Response headers**</span></span>

```
Access-Control-Allow-Headers: Content-Type,x-custom-header
Access-Control-Allow-Methods: PUT,DELETE,GET,OPTIONS
Access-Control-Allow-Origin: https://cors1.azurewebsites.net
Server: Microsoft-IIS/10.0
Set-Cookie: ARRAffinity=8f...;Path=/;HttpOnly;Domain=cors3.azurewebsites.net
Vary: Origin
X-Powered-By: ASP.NET
```

<span data-ttu-id="b5978-338">**要求標頭**</span><span class="sxs-lookup"><span data-stu-id="b5978-338">**Request headers**</span></span>

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

<span data-ttu-id="b5978-339">在上述**回應標頭**中，伺服器會在回應中設定[存取控制-允許來源](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin)標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-339">In the preceding **Response headers**, the server sets the [Access-Control-Allow-Origin](https://developer.mozilla.org/docs/Web/HTTP/Headers/Access-Control-Allow-Origin) header in the response.</span></span> <span data-ttu-id="b5978-340">`https://cors1.azurewebsites.net`此標頭的值符合 `Origin` 要求的標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-340">The `https://cors1.azurewebsites.net` value of this header matches the `Origin` header from the request.</span></span>

<span data-ttu-id="b5978-341">如果 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> 呼叫，則 `Access-Control-Allow-Origin: *` 會傳回萬用字元值。</span><span class="sxs-lookup"><span data-stu-id="b5978-341">If <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> is called, the `Access-Control-Allow-Origin: *`, the wildcard value, is returned.</span></span> <span data-ttu-id="b5978-342">`AllowAnyOrigin`允許任何來源。</span><span class="sxs-lookup"><span data-stu-id="b5978-342">`AllowAnyOrigin` allows any origin.</span></span>

<span data-ttu-id="b5978-343">如果回應不包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="b5978-343">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="b5978-344">具體而言，瀏覽器不允許此要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-344">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="b5978-345">即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-345">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="options"></a>

### <a name="display-options-requests"></a><span data-ttu-id="b5978-346">顯示選項要求</span><span class="sxs-lookup"><span data-stu-id="b5978-346">Display OPTIONS requests</span></span>

<span data-ttu-id="b5978-347">根據預設，Chrome 和 Edge 瀏覽器不會在 F12 工具的 [網路] 索引標籤上顯示 [選項要求]。</span><span class="sxs-lookup"><span data-stu-id="b5978-347">By default, the Chrome and Edge browsers don't show OPTIONS requests on the network tab of the F12 tools.</span></span> <span data-ttu-id="b5978-348">若要在這些瀏覽器中顯示選項要求：</span><span class="sxs-lookup"><span data-stu-id="b5978-348">To display OPTIONS requests in these browsers:</span></span>

* <span data-ttu-id="b5978-349">`chrome://flags/#out-of-blink-cors` 或 `edge://flags/#out-of-blink-cors`</span><span class="sxs-lookup"><span data-stu-id="b5978-349">`chrome://flags/#out-of-blink-cors` or `edge://flags/#out-of-blink-cors`</span></span>
* <span data-ttu-id="b5978-350">停用旗標。</span><span class="sxs-lookup"><span data-stu-id="b5978-350">disable the flag.</span></span>
* <span data-ttu-id="b5978-351">後.</span><span class="sxs-lookup"><span data-stu-id="b5978-351">restart.</span></span>

<span data-ttu-id="b5978-352">Firefox 預設會顯示選項要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-352">Firefox shows OPTIONS requests by default.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="b5978-353">IIS 中的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-353">CORS in IIS</span></span>

<span data-ttu-id="b5978-354">部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。</span><span class="sxs-lookup"><span data-stu-id="b5978-354">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="b5978-355">若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="b5978-355">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

<a name="testc"></a>

## <a name="test-cors"></a><span data-ttu-id="b5978-356">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-356">Test CORS</span></span>

<span data-ttu-id="b5978-357">[範例下載](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)包含測試 CORS 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b5978-357">The [sample download](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI) has code to test CORS.</span></span> <span data-ttu-id="b5978-358">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="b5978-358">See [how to download](xref:index#how-to-download-a-sample).</span></span> <span data-ttu-id="b5978-359">範例是已加入頁面的 API 專案 Razor ：</span><span class="sxs-lookup"><span data-stu-id="b5978-359">The sample is an API project with Razor Pages added:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupTest2.cs?name=snippet2)]

  > [!WARNING]
  > <span data-ttu-id="b5978-360">`WithOrigins("https://localhost:<port>");`應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-360">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/3.1sample/Cors).</span></span>

<span data-ttu-id="b5978-361">以下 `ValuesController` 提供測試的端點：</span><span class="sxs-lookup"><span data-stu-id="b5978-361">The following `ValuesController` provides the endpoints for testing:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/ValuesController.cs?name=snippet)]

<span data-ttu-id="b5978-362">[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs)是由Rick.Doc的[RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet 封裝所提供，並顯示路由資訊。</span><span class="sxs-lookup"><span data-stu-id="b5978-362">[MyDisplayRouteInfo](https://github.com/Rick-Anderson/RouteInfo/blob/master/Microsoft.Docs.Samples.RouteInfo/ControllerContextExtensions.cs) is provided by the [Rick.Docs.Samples.RouteInfo](https://www.nuget.org/packages/Rick.Docs.Samples.RouteInfo) NuGet package and displays route information.</span></span>

<span data-ttu-id="b5978-363">使用下列其中一種方法來測試前面的範例程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5978-363">Test the preceding sample code by using one of the following approaches:</span></span>

* <span data-ttu-id="b5978-364">使用在上部署的範例應用程式 [https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/) 。</span><span class="sxs-lookup"><span data-stu-id="b5978-364">Use the deployed sample app at [https://cors3.azurewebsites.net/](https://cors3.azurewebsites.net/).</span></span> <span data-ttu-id="b5978-365">不需要下載範例。</span><span class="sxs-lookup"><span data-stu-id="b5978-365">There is no need to download the sample.</span></span>
* <span data-ttu-id="b5978-366">`dotnet run`使用的預設 URL 來執行範例 `https://localhost:5001` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-366">Run the sample with `dotnet run` using the default URL of `https://localhost:5001`.</span></span>
* <span data-ttu-id="b5978-367">從 Visual Studio 執行範例，並將的埠設定為44398，以取得的 URL `https://localhost:44398` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-367">Run the sample from Visual Studio with the port set to 44398 for a URL of `https://localhost:44398`.</span></span>

<span data-ttu-id="b5978-368">使用具有 F12 工具的瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="b5978-368">Using a browser with the F12 tools:</span></span>

* <span data-ttu-id="b5978-369">選取 [**值**] 按鈕，並查看 [**網路**] 索引標籤中的標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-369">Select the **Values** button and review the headers in the **Network** tab.</span></span>
* <span data-ttu-id="b5978-370">選取 [ **PUT test** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5978-370">Select the **PUT test** button.</span></span> <span data-ttu-id="b5978-371">如需顯示選項要求的指示，請參閱[顯示選項要求](#options)。</span><span class="sxs-lookup"><span data-stu-id="b5978-371">See [Display OPTIONS requests](#options) for instructions on displaying the OPTIONS request.</span></span> <span data-ttu-id="b5978-372">**PUT 測試**會建立兩個要求，一個選項預檢要求和 PUT 要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-372">The **PUT test** creates two requests, an OPTIONS preflight request and the PUT request.</span></span>
* <span data-ttu-id="b5978-373">選取 [] **`GetValues2 [DisableCors]`** 按鈕以觸發失敗的 CORS 要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-373">Select the **`GetValues2 [DisableCors]`** button to trigger a failed CORS request.</span></span> <span data-ttu-id="b5978-374">如檔中所述，回應會傳回200成功，但不會進行 CORS 要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-374">As mentioned in the document, the response returns 200 success, but the CORS request is not made.</span></span> <span data-ttu-id="b5978-375">選取 [**主控台**] 索引標籤，以查看 CORS 錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5978-375">Select the **Console** tab to see the CORS error.</span></span> <span data-ttu-id="b5978-376">視瀏覽器而定，會顯示類似下列的錯誤：</span><span class="sxs-lookup"><span data-stu-id="b5978-376">Depending on the browser, an error similar to the following is displayed:</span></span>

     <span data-ttu-id="b5978-377">`'https://cors1.azurewebsites.net/api/values/GetValues2'`CORS 原則已封鎖從原始來源提取的存取權 `'https://cors3.azurewebsites.net'` ：要求的資源上沒有任何「存取控制-允許來源」標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-377">Access to fetch at `'https://cors1.azurewebsites.net/api/values/GetValues2'` from origin `'https://cors3.azurewebsites.net'` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.</span></span> <span data-ttu-id="b5978-378">如果不透明回應適合您的需求，請將要求的模式設定為 'no-cors' 以在停用 CORS 之下擷取資源。</span><span class="sxs-lookup"><span data-stu-id="b5978-378">If an opaque response serves your needs, set the request's mode to 'no-cors' to fetch the resource with CORS disabled.</span></span>
     
<span data-ttu-id="b5978-379">已啟用 CORS 的端點可以使用工具（例如，[捲曲](https://curl.haxx.se/)、 [Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。</span><span class="sxs-lookup"><span data-stu-id="b5978-379">CORS-enabled endpoints can be tested with a tool, such as [curl](https://curl.haxx.se/), [Fiddler](https://www.telerik.com/fiddler), or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="b5978-380">使用工具時，標頭所指定的要求原點必須與 `Origin` 接收要求的主機不同。</span><span class="sxs-lookup"><span data-stu-id="b5978-380">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="b5978-381">如果要求不是根據標頭的值而*跨原始來源* `Origin` ：</span><span class="sxs-lookup"><span data-stu-id="b5978-381">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="b5978-382">CORS 中介軟體不需要處理要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-382">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="b5978-383">回應中不會傳回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-383">CORS headers aren't returned in the response.</span></span>

<span data-ttu-id="b5978-384">下列命令會使用 `curl` 來發出具有下列資訊的選項要求：</span><span class="sxs-lookup"><span data-stu-id="b5978-384">The following command uses `curl` to issue an OPTIONS request with information:</span></span>

```bash
curl -X OPTIONS https://cors3.azurewebsites.net/api/TodoItems2/5 -i
```

<!--
curl come with Git. Add to path variable
C:\Program Files\Git\mingw64\bin\
-->

<a name="tcer"></a>

### <a name="test-cors-with-endpoint-routing-and-httpoptions"></a><span data-ttu-id="b5978-385">使用端點路由和 [HttpOptions] 測試 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-385">Test CORS with endpoint routing and [HttpOptions]</span></span>

<span data-ttu-id="b5978-386">使用時，以每個端點為基礎啟用 CORS `RequireCors` ，目前**不**支援[自動預檢要求](#apf)。</span><span class="sxs-lookup"><span data-stu-id="b5978-386">Enabling CORS on a per-endpoint basis using `RequireCors` currently does **not** support [automatic preflight requests](#apf).</span></span> <span data-ttu-id="b5978-387">請考慮使用[端點路由來啟用 CORS](#ecors)的下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5978-387">Consider the following code which uses [endpoint routing to enable CORS](#ecors):</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/StartupEndPointBugTest.cs?name=snippet2)]

<span data-ttu-id="b5978-388">以下 `TodoItems1Controller` 提供測試的端點：</span><span class="sxs-lookup"><span data-stu-id="b5978-388">The following `TodoItems1Controller` provides endpoints for testing:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems1Controller.cs?name=snippet2)]

<span data-ttu-id="b5978-389">從已部署[範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI)的 [[測試] 頁面](https://cors1.azurewebsites.net/test?number=1)測試上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="b5978-389">Test the preceding code from the [test page](https://cors1.azurewebsites.net/test?number=1) of the deployed [sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI).</span></span>

<span data-ttu-id="b5978-390">**Delete [EnableCors]** 和**GET [EnableCors]** 按鈕成功，因為端點有 `[EnableCors]` 並回應預檢要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-390">The **Delete [EnableCors]** and **GET [EnableCors]** buttons succeed, because the endpoints have `[EnableCors]` and respond to preflight requests.</span></span> <span data-ttu-id="b5978-391">其他端點失敗。</span><span class="sxs-lookup"><span data-stu-id="b5978-391">The other endpoints fails.</span></span> <span data-ttu-id="b5978-392">[**取得**] 按鈕會失敗，因為[JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js)會傳送：</span><span class="sxs-lookup"><span data-stu-id="b5978-392">The **GET** button fails, because the [JavaScript](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/3.1sample/Cors/WebAPI/wwwroot/js/MyJS.js) sends:</span></span>

```javascript
 headers: {
      "Content-Type": "x-custom-header"
 },
```

<span data-ttu-id="b5978-393">以下 `TodoItems2Controller` 提供類似的端點，但包含明確的程式碼來回應選項要求：</span><span class="sxs-lookup"><span data-stu-id="b5978-393">The following `TodoItems2Controller` provides similar endpoints, but includes explicit code to respond to OPTIONS requests:</span></span>

[!code-csharp[](cors/3.1sample/Cors/WebAPI/Controllers/TodoItems2Controller.cs?name=snippet2)]

<span data-ttu-id="b5978-394">從已部署範例的 [[測試] 頁面](https://cors1.azurewebsites.net/test?number=2)測試上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="b5978-394">Test the preceding code from the [test page](https://cors1.azurewebsites.net/test?number=2) of the deployed sample.</span></span> <span data-ttu-id="b5978-395">在 [**控制器**] 下拉式清單中，選取 [**預檢**]，然後**設定 [控制器**]。</span><span class="sxs-lookup"><span data-stu-id="b5978-395">In the **Controller** drop down list, select **Preflight** and then **Set Controller**.</span></span> <span data-ttu-id="b5978-396">所有對端點的 CORS 呼叫都會 `TodoItems2Controller` 成功。</span><span class="sxs-lookup"><span data-stu-id="b5978-396">All the CORS calls to the `TodoItems2Controller` endpoints succeed.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5978-397">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5978-397">Additional resources</span></span>

* [<span data-ttu-id="b5978-398">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="b5978-398">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="b5978-399">IIS CORS 模組使用者入門</span><span class="sxs-lookup"><span data-stu-id="b5978-399">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b5978-400">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b5978-400">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b5978-401">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-401">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="b5978-402">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-402">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="b5978-403">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="b5978-403">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="b5978-404">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="b5978-404">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="b5978-405">有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-405">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="b5978-406">如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="b5978-406">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="b5978-407">[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：</span><span class="sxs-lookup"><span data-stu-id="b5978-407">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="b5978-408">是一種 W3C 標準，可讓伺服器放寬相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-408">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="b5978-409">**不**是安全性功能，CORS 放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="b5978-409">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="b5978-410">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="b5978-410">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="b5978-411">如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-411">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="b5978-412">允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-412">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="b5978-413">比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="b5978-413">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="b5978-414">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="b5978-414">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="b5978-415">相同的來源</span><span class="sxs-lookup"><span data-stu-id="b5978-415">Same origin</span></span>

<span data-ttu-id="b5978-416">如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。</span><span class="sxs-lookup"><span data-stu-id="b5978-416">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="b5978-417">這兩個 Url 具有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="b5978-417">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="b5978-418">這些 Url 的來源不同于前兩個 Url：</span><span class="sxs-lookup"><span data-stu-id="b5978-418">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="b5978-419">`https://example.net`：不同的網域</span><span class="sxs-lookup"><span data-stu-id="b5978-419">`https://example.net`: Different domain</span></span>
* <span data-ttu-id="b5978-420">`https://www.example.com/foo.html`：不同的子域</span><span class="sxs-lookup"><span data-stu-id="b5978-420">`https://www.example.com/foo.html`: Different subdomain</span></span>
* <span data-ttu-id="b5978-421">`http://example.com/foo.html`：不同的配置</span><span class="sxs-lookup"><span data-stu-id="b5978-421">`http://example.com/foo.html`: Different scheme</span></span>
* <span data-ttu-id="b5978-422">`https://example.com:9000/foo.html`：不同的埠</span><span class="sxs-lookup"><span data-stu-id="b5978-422">`https://example.com:9000/foo.html`: Different port</span></span>

<span data-ttu-id="b5978-423">在比較原始來源時，Internet Explorer 不會考慮此埠。</span><span class="sxs-lookup"><span data-stu-id="b5978-423">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="b5978-424">具有已命名原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-424">CORS with named policy and middleware</span></span>

<span data-ttu-id="b5978-425">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-425">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="b5978-426">下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="b5978-426">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="b5978-427">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="b5978-427">The preceding code:</span></span>

* <span data-ttu-id="b5978-428">將原則名稱設定為 " \_ myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="b5978-428">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="b5978-429">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="b5978-429">The policy name is arbitrary.</span></span>
* <span data-ttu-id="b5978-430">呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，以啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-430">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="b5978-431"><xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的呼叫。</span><span class="sxs-lookup"><span data-stu-id="b5978-431">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="b5978-432">Lambda 會接受 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。</span><span class="sxs-lookup"><span data-stu-id="b5978-432">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="b5978-433">[Configuration options](#cors-policy-options) `WithOrigins` 這篇文章稍後會說明設定選項，例如。</span><span class="sxs-lookup"><span data-stu-id="b5978-433">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="b5978-434"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將 CORS 服務新增至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="b5978-434">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="b5978-435">如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="b5978-435">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="b5978-436"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以連鎖方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="b5978-436">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="b5978-437">注意： URL**不**能包含尾端斜線（ `/` ）。</span><span class="sxs-lookup"><span data-stu-id="b5978-437">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="b5978-438">如果 URL 以結束 `/` ，則比較會 `false` 傳回，而且不會傳回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-438">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="b5978-439">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="b5978-439">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="b5978-440">注意： `UseCors` 必須在之前呼叫 `UseMvc` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-440">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="b5978-441">請參閱[在頁面 Razor 、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-441">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="b5978-442">如需測試程式碼的相關指示，請參閱[測試 CORS](#test) ，類似于上述程式碼。</span><span class="sxs-lookup"><span data-stu-id="b5978-442">See [Test CORS](#test) for instructions on testing code similar to the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="b5978-443">啟用具有屬性的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-443">Enable CORS with attributes</span></span>

<span data-ttu-id="b5978-444">[ &lbrack; EnableCors &rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-444">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="b5978-445">`[EnableCors]`屬性會啟用所選結束點的 CORS，而不是所有結束點。</span><span class="sxs-lookup"><span data-stu-id="b5978-445">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="b5978-446">使用 `[EnableCors]` 指定預設原則，並 `[EnableCors("{Policy String}")]` 指定原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-446">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="b5978-447">`[EnableCors]`屬性可套用至：</span><span class="sxs-lookup"><span data-stu-id="b5978-447">The `[EnableCors]` attribute can be applied to:</span></span>

* Razor<span data-ttu-id="b5978-448">本頁`PageModel`</span><span class="sxs-lookup"><span data-stu-id="b5978-448"> Page `PageModel`</span></span>
* <span data-ttu-id="b5978-449">控制器</span><span class="sxs-lookup"><span data-stu-id="b5978-449">Controller</span></span>
* <span data-ttu-id="b5978-450">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="b5978-450">Controller action method</span></span>

<span data-ttu-id="b5978-451">您可以使用屬性，將不同的原則套用至控制器/頁面模型/動作 `[EnableCors]` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-451">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="b5978-452">當 `[EnableCors]` 屬性套用至控制器/頁面模型/動作方法，且在中介軟體中啟用 CORS 時，會套用**這兩個**原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-452">When the `[EnableCors]` attribute is applied to a controllers/page model/action method, and CORS is enabled in middleware, **both** policies are applied.</span></span> <span data-ttu-id="b5978-453">我們建議您**不要**結合原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-453">We recommend **not** combining policies.</span></span> <span data-ttu-id="b5978-454">請使用 `[EnableCors]` 屬性或中介軟體，**而不是兩者**。</span><span class="sxs-lookup"><span data-stu-id="b5978-454">Use the `[EnableCors]` attribute or middleware, **not both**.</span></span> <span data-ttu-id="b5978-455">使用時 `[EnableCors]` ，請勿**not**定義預設原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-455">When using `[EnableCors]`, do **not** define a default policy.</span></span>

<span data-ttu-id="b5978-456">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="b5978-456">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="b5978-457">下列程式碼會建立 CORS 預設原則和名為的原則 `"AnotherPolicy"` ：</span><span class="sxs-lookup"><span data-stu-id="b5978-457">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="b5978-458">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-458">Disable CORS</span></span>

<span data-ttu-id="b5978-459">[ &lbrack; DisableCors &rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性會停用控制器/頁面模型/動作的 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-459">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="b5978-460">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="b5978-460">CORS policy options</span></span>

<span data-ttu-id="b5978-461">本節說明可在 CORS 原則中設定的各種選項：</span><span class="sxs-lookup"><span data-stu-id="b5978-461">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="b5978-462">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="b5978-462">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="b5978-463">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="b5978-463">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="b5978-464">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-464">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="b5978-465">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-465">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="b5978-466">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="b5978-466">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="b5978-467">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="b5978-467">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="b5978-468"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>會在中呼叫 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-468"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b5978-469">針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="b5978-469">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="b5978-470">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="b5978-470">Set the allowed origins</span></span>

<span data-ttu-id="b5978-471"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>：允許使用任何配置（或）來自所有來源的 CORS 要求 `http` `https` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-471"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>: Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="b5978-472">`AllowAnyOrigin`不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-472">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="b5978-473">指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，而且可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-473">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="b5978-474">針對安全的應用程式，如果用戶端必須授權自己來存取伺服器資源，請指定來源的確切清單。</span><span class="sxs-lookup"><span data-stu-id="b5978-474">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="b5978-475">`AllowAnyOrigin`會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-475">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="b5978-476">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-476">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="b5978-477"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>：將原則的屬性設定為函式 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> ，以便在評估是否允許來源時，允許來源符合已設定的萬用字元網域。</span><span class="sxs-lookup"><span data-stu-id="b5978-477"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>: Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="b5978-478">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="b5978-478">Set the allowed HTTP methods</span></span>

<span data-ttu-id="b5978-479"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="b5978-479"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="b5978-480">允許任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="b5978-480">Allows any HTTP method:</span></span>
* <span data-ttu-id="b5978-481">會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-481">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="b5978-482">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-482">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="b5978-483">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-483">Set the allowed request headers</span></span>

<span data-ttu-id="b5978-484">若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="b5978-484">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="b5978-485">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-485">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="b5978-486">此設定會影響預檢要求和 `Access-Control-Request-Headers` 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-486">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="b5978-487">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="b5978-487">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="b5978-488">CORS 中介軟體一律允許傳送中的四個標頭， `Access-Control-Request-Headers` 而不論 c 中設定的值為何。</span><span class="sxs-lookup"><span data-stu-id="b5978-488">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="b5978-489">此標頭清單包含：</span><span class="sxs-lookup"><span data-stu-id="b5978-489">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="b5978-490">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="b5978-490">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="b5978-491">CORS 中介軟體會以下列要求標頭成功回應預檢要求，因為 `Content-Language` 一律會列入允許清單：</span><span class="sxs-lookup"><span data-stu-id="b5978-491">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="b5978-492">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="b5978-492">Set the exposed response headers</span></span>

<span data-ttu-id="b5978-493">根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-493">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="b5978-494">如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="b5978-494">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="b5978-495">預設可用的回應標頭為：</span><span class="sxs-lookup"><span data-stu-id="b5978-495">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="b5978-496">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="b5978-496">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="b5978-497">若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-497">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="b5978-498">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="b5978-498">Credentials in cross-origin requests</span></span>

<span data-ttu-id="b5978-499">認證需要在 CORS 要求中進行特殊處理。</span><span class="sxs-lookup"><span data-stu-id="b5978-499">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="b5978-500">根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="b5978-500">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="b5978-501">認證包括 cookie 和 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="b5978-501">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="b5978-502">若要使用跨原始來源要求傳送認證，用戶端必須將設 `XMLHttpRequest.withCredentials` 為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-502">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="b5978-503">`XMLHttpRequest`直接使用：</span><span class="sxs-lookup"><span data-stu-id="b5978-503">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="b5978-504">使用 jQuery：</span><span class="sxs-lookup"><span data-stu-id="b5978-504">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="b5978-505">使用[FETCH API](https://developer.mozilla.org/docs/Web/API/Fetch_API)：</span><span class="sxs-lookup"><span data-stu-id="b5978-505">Using the [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="b5978-506">伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="b5978-506">The server must allow the credentials.</span></span> <span data-ttu-id="b5978-507">若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-507">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="b5978-508">HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="b5978-508">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="b5978-509">如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="b5978-509">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="b5978-510">允許跨原始來源認證會有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="b5978-510">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="b5978-511">另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。</span><span class="sxs-lookup"><span data-stu-id="b5978-511">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="b5978-512">CORS 規格也會指出如果標頭存在，將原始來源設定為 `"*"` （所有來源）是不正確 `Access-Control-Allow-Credentials` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-512">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="b5978-513">預檢要求</span><span class="sxs-lookup"><span data-stu-id="b5978-513">Preflight requests</span></span>

<span data-ttu-id="b5978-514">針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-514">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="b5978-515">此要求稱為*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="b5978-515">This request is called a *preflight request*.</span></span> <span data-ttu-id="b5978-516">當下列條件成立時，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="b5978-516">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="b5978-517">要求方法為 GET、HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="b5978-517">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="b5978-518">應用程式不會設定、、、或以外的要求標頭 `Accept` `Accept-Language` `Content-Language` `Content-Type` `Last-Event-ID` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-518">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="b5978-519">`Content-Type`如果設定了標頭，則會有下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="b5978-519">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="b5978-520">針對用戶端要求設定的要求標頭規則會套用至應用程式透過在物件上呼叫所設定的標頭 `setRequestHeader` `XMLHttpRequest` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-520">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="b5978-521">CORS 規格會呼叫這些標頭的*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="b5978-521">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="b5978-522">此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent` 、 `Host` 或 `Content-Length` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-522">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="b5978-523">以下是預檢要求的範例：</span><span class="sxs-lookup"><span data-stu-id="b5978-523">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="b5978-524">預先飛行要求會使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-524">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="b5978-525">其中包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="b5978-525">It includes two special headers:</span></span>

* <span data-ttu-id="b5978-526">`Access-Control-Request-Method`：將用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="b5978-526">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="b5978-527">`Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。</span><span class="sxs-lookup"><span data-stu-id="b5978-527">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="b5978-528">如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-528">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<!-- I think this needs to be removed, was put here accidently -->

<span data-ttu-id="b5978-529">以適當的原則啟用 CORS 時，ASP.NET Core 通常會自動回應 CORS 預檢要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-529">When CORS is enabled with the appropriate policy, ASP.NET Core generally automatically responds to CORS preflight requests.</span></span> <span data-ttu-id="b5978-530">請參閱[預檢要求的 [HttpOptions] 屬性](#pro)。</span><span class="sxs-lookup"><span data-stu-id="b5978-530">See [[HttpOptions] attribute for preflight requests](#pro).</span></span>

<span data-ttu-id="b5978-531">CORS 預檢要求可能會包含 `Access-Control-Request-Headers` 標頭，這會向伺服器指出與實際要求一起傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-531">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="b5978-532">若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-532">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="b5978-533">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-533">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="b5978-534">瀏覽器的設定方式並不完全一致 `Access-Control-Request-Headers` 。</span><span class="sxs-lookup"><span data-stu-id="b5978-534">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="b5978-535">如果您將標頭設定為 `"*"` （或使用）以外的任何專案 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*> ，您應該至少包含 `Accept` 、 `Content-Type` 和，再 `Origin` 加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-535">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="b5978-536">以下是預檢要求的回應範例（假設伺服器允許此要求）：</span><span class="sxs-lookup"><span data-stu-id="b5978-536">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="b5978-537">回應 `Access-Control-Allow-Methods` 會包含標頭，其中會列出允許的方法，並選擇性地列出 `Access-Control-Allow-Headers` 標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-537">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="b5978-538">如果預檢要求成功，瀏覽器就會傳送實際的要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-538">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="b5978-539">如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-539">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="b5978-540">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-540">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="b5978-541">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="b5978-541">Set the preflight expiration time</span></span>

<span data-ttu-id="b5978-542">`Access-Control-Max-Age`標頭會指定快取回應要求的時間長度。</span><span class="sxs-lookup"><span data-stu-id="b5978-542">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="b5978-543">若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*> ：</span><span class="sxs-lookup"><span data-stu-id="b5978-543">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="b5978-544">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="b5978-544">How CORS works</span></span>

<span data-ttu-id="b5978-545">本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS)要求中會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="b5978-545">This section describes what happens in a [CORS](https://developer.mozilla.org/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="b5978-546">CORS**不**是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="b5978-546">CORS is **not** a security feature.</span></span> <span data-ttu-id="b5978-547">CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。</span><span class="sxs-lookup"><span data-stu-id="b5978-547">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="b5978-548">例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="b5978-548">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="b5978-549">藉由允許 CORS，您的 API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="b5978-549">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="b5978-550">而是由用戶端（瀏覽器）強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-550">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="b5978-551">伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。</span><span class="sxs-lookup"><span data-stu-id="b5978-551">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="b5978-552">例如，下列任何一項工具都會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="b5978-552">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="b5978-553">Fiddler</span><span class="sxs-lookup"><span data-stu-id="b5978-553">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="b5978-554">Postman</span><span class="sxs-lookup"><span data-stu-id="b5978-554">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="b5978-555">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="b5978-555">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="b5978-556">網頁瀏覽器，方法是在網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="b5978-556">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="b5978-557">這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/docs/Web/API/Fetch_API)要求的方式，否則會禁止。</span><span class="sxs-lookup"><span data-stu-id="b5978-557">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="b5978-558">瀏覽器（不含 CORS）無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-558">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="b5978-559">在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。</span><span class="sxs-lookup"><span data-stu-id="b5978-559">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="b5978-560">JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="b5978-560">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="b5978-561">允許跨原始來源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="b5978-561">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="b5978-562">[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-562">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="b5978-563">如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-563">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="b5978-564">不需要自訂 JavaScript 程式碼來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-564">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="b5978-565">以下是跨原始來源要求的範例。</span><span class="sxs-lookup"><span data-stu-id="b5978-565">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="b5978-566">`Origin`標頭會提供提出要求之網站的網域。</span><span class="sxs-lookup"><span data-stu-id="b5978-566">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="b5978-567">`Origin`標頭是必要的，而且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="b5978-567">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="b5978-568">如果伺服器允許此要求，它會 `Access-Control-Allow-Origin` 在回應中設定標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-568">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="b5978-569">此標頭的值 `Origin` 會比對要求中的標頭，或為萬用字元值 `"*"` ，表示允許任何來源：</span><span class="sxs-lookup"><span data-stu-id="b5978-569">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="b5978-570">如果回應不包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="b5978-570">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="b5978-571">具體而言，瀏覽器不允許此要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-571">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="b5978-572">即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-572">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="b5978-573">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-573">Test CORS</span></span>

<span data-ttu-id="b5978-574">測試 CORS：</span><span class="sxs-lookup"><span data-stu-id="b5978-574">To test CORS:</span></span>

1. <span data-ttu-id="b5978-575">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="b5978-575">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="b5978-576">或者，您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-576">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="b5978-577">使用本檔中的其中一種方法來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="b5978-577">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="b5978-578">例如：</span><span class="sxs-lookup"><span data-stu-id="b5978-578">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="b5978-579">`WithOrigins("https://localhost:<port>");`應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="b5978-579">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="b5978-580">建立 web 應用程式專案（ Razor 頁面或 MVC）。</span><span class="sxs-lookup"><span data-stu-id="b5978-580">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="b5978-581">此範例會使用 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="b5978-581">The sample uses Razor Pages.</span></span> <span data-ttu-id="b5978-582">您可以在與 API 專案相同的方案中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-582">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="b5978-583">將下列反白顯示的程式碼新增至*Index. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="b5978-583">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="b5978-584">在上述程式碼中，將取代為已 `url: 'https://<web app>.azurewebsites.net/api/values/1',` 部署應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="b5978-584">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="b5978-585">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="b5978-585">Deploy the API project.</span></span> <span data-ttu-id="b5978-586">例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="b5978-586">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="b5978-587">Razor從桌面執行頁面或 MVC 應用程式，然後按一下 [**測試**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="b5978-587">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="b5978-588">使用 F12 工具來檢查錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b5978-588">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="b5978-589">從移除 localhost 來源 `WithOrigins` ，並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-589">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="b5978-590">或者，使用不同的埠來執行用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="b5978-590">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="b5978-591">例如，從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="b5978-591">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="b5978-592">使用用戶端應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="b5978-592">Test with the client app.</span></span> <span data-ttu-id="b5978-593">CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="b5978-593">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="b5978-594">使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。</span><span class="sxs-lookup"><span data-stu-id="b5978-594">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="b5978-595">視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：</span><span class="sxs-lookup"><span data-stu-id="b5978-595">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="b5978-596">使用 Microsoft Edge：</span><span class="sxs-lookup"><span data-stu-id="b5978-596">Using Microsoft Edge:</span></span>

     <span data-ttu-id="b5978-597">**SEC7120： [CORS] 來源在 `https://localhost:44375` `https://localhost:44375` 的跨原始來源資源的存取控制-允許來源回應標頭中找不到`https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="b5978-597">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="b5978-598">使用 Chrome：</span><span class="sxs-lookup"><span data-stu-id="b5978-598">Using Chrome:</span></span>

     <span data-ttu-id="b5978-599">**`https://webapi.azurewebsites.net/api/values/1` `https://localhost:44375` CORS 原則已封鎖從來源存取 XMLHttpRequest：要求的資源上沒有任何「存取控制-允許來源」標頭。**</span><span class="sxs-lookup"><span data-stu-id="b5978-599">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="b5978-600">具有 CORS 功能的端點可以使用工具（例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。</span><span class="sxs-lookup"><span data-stu-id="b5978-600">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="b5978-601">使用工具時，標頭所指定的要求原點必須與 `Origin` 接收要求的主機不同。</span><span class="sxs-lookup"><span data-stu-id="b5978-601">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="b5978-602">如果要求不是根據標頭的值而*跨原始來源* `Origin` ：</span><span class="sxs-lookup"><span data-stu-id="b5978-602">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="b5978-603">CORS 中介軟體不需要處理要求。</span><span class="sxs-lookup"><span data-stu-id="b5978-603">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="b5978-604">回應中不會傳回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="b5978-604">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="b5978-605">IIS 中的 CORS</span><span class="sxs-lookup"><span data-stu-id="b5978-605">CORS in IIS</span></span>

<span data-ttu-id="b5978-606">部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。</span><span class="sxs-lookup"><span data-stu-id="b5978-606">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="b5978-607">若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="b5978-607">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5978-608">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5978-608">Additional resources</span></span>

* [<span data-ttu-id="b5978-609">跨原始來源資源分享 (CORS)</span><span class="sxs-lookup"><span data-stu-id="b5978-609">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="b5978-610">IIS CORS 模組使用者入門</span><span class="sxs-lookup"><span data-stu-id="b5978-610">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end
