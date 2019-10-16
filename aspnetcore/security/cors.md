---
title: 啟用 ASP.NET Core 中的跨原始來源要求（CORS）
author: rick-anderson
description: 瞭解 CORS 如何作為標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始來源要求。
ms.author: riande
ms.custom: mvc
ms.date: 04/07/2019
uid: security/cors
ms.openlocfilehash: a02b3497684979c1a9e792437f9f1a4c467600f0
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187262"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="8e7c4-103">啟用 ASP.NET Core 中的跨原始來源要求（CORS）</span><span class="sxs-lookup"><span data-stu-id="8e7c4-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="8e7c4-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="8e7c4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="8e7c4-105">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="8e7c4-106">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="8e7c4-107">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="8e7c4-108">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="8e7c4-109">有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="8e7c4-110">如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="8e7c4-111">[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="8e7c4-112">是一種 W3C 標準，可讓伺服器放寬相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="8e7c4-113">**不**是安全性功能，CORS 放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="8e7c4-114">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="8e7c4-115">如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="8e7c4-116">允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="8e7c4-117">比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="8e7c4-118">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8e7c4-118">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="8e7c4-119">相同原始來源</span><span class="sxs-lookup"><span data-stu-id="8e7c4-119">Same origin</span></span>

<span data-ttu-id="8e7c4-120">如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="8e7c4-121">這兩個 Url 有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="8e7c4-122">這些 Url 的來源不同于前兩個 Url：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="8e7c4-123">`https://example.net`&ndash;不同的網域</span><span class="sxs-lookup"><span data-stu-id="8e7c4-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="8e7c4-124">`https://www.example.com/foo.html`&ndash;不同子域</span><span class="sxs-lookup"><span data-stu-id="8e7c4-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="8e7c4-125">`http://example.com/foo.html`&ndash;不同的配置</span><span class="sxs-lookup"><span data-stu-id="8e7c4-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="8e7c4-126">`https://example.com:9000/foo.html`&ndash;不同的埠</span><span class="sxs-lookup"><span data-stu-id="8e7c4-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="8e7c4-127">比較原始來源時，Internet Explorer 不會考慮連接埠。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="8e7c4-128">具有已命名原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="8e7c4-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="8e7c4-129">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="8e7c4-130">下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="8e7c4-131">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-131">The preceding code:</span></span>

* <span data-ttu-id="8e7c4-132">將原則名稱設定為 "\_myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="8e7c4-133">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="8e7c4-134"><xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*>呼叫擴充方法，以啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="8e7c4-135">使用<xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> [lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)的呼叫。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="8e7c4-136">Lambda 會接受<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>物件。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="8e7c4-137">這篇文章稍後會`WithOrigins`說明[設定選項](#cors-policy-options)，例如。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="8e7c4-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*>方法呼叫會將 CORS 服務新增至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="8e7c4-139">如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="8e7c4-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder>方法可以連鎖方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="8e7c4-141">注意：URL**不**能包含尾端斜線（`/`）。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="8e7c4-142">如果 URL 以`/`結束，則`false`比較會傳回，而且不會傳回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8e7c4-143">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-143">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code ommitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code ommited.
}
```

> [!WARNING]
> <span data-ttu-id="8e7c4-144">使用端點路由，必須將 CORS 中介軟體設定為在呼叫`UseRouting`和`UseEndpoints`之間執行。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-144">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="8e7c4-145">不正確的設定會導致中介軟體停止正常運作。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-145">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="8e7c4-146">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-146">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="8e7c4-147">注意： `UseCors`必須在之前`UseMvc`呼叫。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-147">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="8e7c4-148">請參閱[在 Razor Pages、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-148">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="8e7c4-149">如需測試上述程式碼的指示，請參閱[測試 CORS](#test) 。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-149">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="8e7c4-150">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="8e7c4-150">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="8e7c4-151">使用端點路由，可以使用`RequireCors`一組擴充方法，針對每個端點來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-151">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="8e7c4-152">同樣地，您也可以針對所有控制器啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-152">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="8e7c4-153">啟用具有屬性的 CORS</span><span class="sxs-lookup"><span data-stu-id="8e7c4-153">Enable CORS with attributes</span></span>

<span data-ttu-id="8e7c4-154">EnableCors 屬性提供全域套用 CORS 的替代方法。 [ &lbrack; &rbrack; ](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="8e7c4-154">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="8e7c4-155">`[EnableCors]`屬性會啟用所選結束點的 CORS，而不是所有結束點。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-155">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="8e7c4-156">使用`[EnableCors]`指定預設原則，並`[EnableCors("{Policy String}")]`指定原則。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-156">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="8e7c4-157">`[EnableCors]`屬性可套用至：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-157">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="8e7c4-158">Razor 頁面`PageModel`</span><span class="sxs-lookup"><span data-stu-id="8e7c4-158">Razor Page `PageModel`</span></span>
* <span data-ttu-id="8e7c4-159">控制器</span><span class="sxs-lookup"><span data-stu-id="8e7c4-159">Controller</span></span>
* <span data-ttu-id="8e7c4-160">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="8e7c4-160">Controller action method</span></span>

<span data-ttu-id="8e7c4-161">您可以使用屬性，將不同的`[EnableCors]`原則套用至控制器/頁面模型/動作。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-161">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="8e7c4-162">`[EnableCors]`當屬性套用至控制器/頁面模型/動作方法，且在中介軟體中啟用 CORS 時，會套用這兩個原則。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-162">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="8e7c4-163">我們建議您不要結合原則。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-163">We recommend against combining policies.</span></span> <span data-ttu-id="8e7c4-164">`[EnableCors]`使用屬性或中介軟體，而不是同時在相同的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-164">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="8e7c4-165">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-165">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="8e7c4-166">下列程式碼會建立 CORS 預設原則和名為`"AnotherPolicy"`的原則：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-166">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="8e7c4-167">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="8e7c4-167">Disable CORS</span></span>

<span data-ttu-id="8e7c4-168">DisableCors 屬性會停用控制器/頁面模型/動作的 CORS。 [ &lbrack; &rbrack; ](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)</span><span class="sxs-lookup"><span data-stu-id="8e7c4-168">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="8e7c4-169">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="8e7c4-169">CORS policy options</span></span>

<span data-ttu-id="8e7c4-170">本節說明可在 CORS 原則中設定的各種選項：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-170">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="8e7c4-171">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="8e7c4-171">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="8e7c4-172">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="8e7c4-172">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="8e7c4-173">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="8e7c4-173">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="8e7c4-174">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="8e7c4-174">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="8e7c4-175">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="8e7c4-175">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="8e7c4-176">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="8e7c4-176">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="8e7c4-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>會在中`Startup.ConfigureServices`呼叫。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-177"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="8e7c4-178">針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-178">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="8e7c4-179">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="8e7c4-179">Set the allowed origins</span></span>

<span data-ttu-id="8e7c4-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*>允許任何配置（`http`或`https`）來自所有來源的 CORS 要求。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="8e7c4-180"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="8e7c4-181">`AllowAnyOrigin`不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-181">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="8e7c4-182">指定`AllowAnyOrigin` 和`AllowCredentials`是不安全的設定，而且可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-182">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8e7c4-183">當應用程式已設定這兩種方法時，CORS 服務會傳回不正確 CORS 回應。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-183">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="8e7c4-184">指定`AllowAnyOrigin` 和`AllowCredentials`是不安全的設定，而且可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-184">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="8e7c4-185">針對安全的應用程式，如果用戶端必須授權自己來存取伺服器資源，請指定來源的確切清單。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-185">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="8e7c4-186">`AllowAnyOrigin`會影響預檢要求和`Access-Control-Allow-Origin`標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-186">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="8e7c4-187">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-187">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="8e7c4-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*>將原則的屬性設定為函式，以便在評估是否允許來源時，允許來源符合已設定的萬用字元網域。 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> &ndash;</span><span class="sxs-lookup"><span data-stu-id="8e7c4-188"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="8e7c4-189">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="8e7c4-189">Set the allowed HTTP methods</span></span>

<span data-ttu-id="8e7c4-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-190"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="8e7c4-191">允許任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-191">Allows any HTTP method:</span></span>
* <span data-ttu-id="8e7c4-192">會影響預檢要求和`Access-Control-Allow-Methods`標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-192">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="8e7c4-193">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-193">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="8e7c4-194">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="8e7c4-194">Set the allowed request headers</span></span>

<span data-ttu-id="8e7c4-195">若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*） <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> ，請呼叫並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-195">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="8e7c4-196">若要允許所有作者要求標頭<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>，請呼叫：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-196">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="8e7c4-197">此設定會影響預檢要求和`Access-Control-Request-Headers`標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-197">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="8e7c4-198">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-198">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="8e7c4-199">只有在`WithHeaders` `Access-Control-Request-Headers`完全符合中`WithHeaders`所述標頭的情況下傳送的標頭時，才可以將 CORS 中介軟體原則與指定的特定標頭比對。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-199">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="8e7c4-200">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-200">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8e7c4-201">CORS 中介軟體會拒絕具有下列要求標頭的預檢`Content-Language`要求，因為（[HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)） `WithHeaders`未列于：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-201">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="8e7c4-202">應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-202">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8e7c4-203">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-203">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="8e7c4-204">CORS 中介軟體一律允許傳送中`Access-Control-Request-Headers`的四個標頭，而不論 c 中設定的值為何。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-204">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="8e7c4-205">此標頭清單包含：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-205">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="8e7c4-206">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-206">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="8e7c4-207">CORS 中介軟體會以下列要求標頭成功回應預檢要求， `Content-Language`因為一律會列入允許清單：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-207">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="8e7c4-208">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="8e7c4-208">Set the exposed response headers</span></span>

<span data-ttu-id="8e7c4-209">根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-209">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="8e7c4-210">如需詳細資訊， [請參閱 W3C 跨原始來源資源分享（術語）：簡單的回應](https://www.w3.org/TR/cors/#simple-response-header)標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-210">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="8e7c4-211">預設可用的回應標頭為：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-211">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="8e7c4-212">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-212">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="8e7c4-213">若要讓應用程式使用其他標頭， <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>請呼叫：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-213">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="8e7c4-214">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="8e7c4-214">Credentials in cross-origin requests</span></span>

<span data-ttu-id="8e7c4-215">認證需要在 CORS 要求中進行特殊處理。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-215">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="8e7c4-216">根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-216">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="8e7c4-217">認證包括 cookie 和 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-217">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="8e7c4-218">若要使用跨原始來源要求傳送認證，用戶端必須將`XMLHttpRequest.withCredentials`設`true`為。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-218">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="8e7c4-219">直接`XMLHttpRequest`使用：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-219">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="8e7c4-220">使用 jQuery：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-220">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="8e7c4-221">使用[FETCH API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-221">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="8e7c4-222">伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-222">The server must allow the credentials.</span></span> <span data-ttu-id="8e7c4-223">若要允許跨原始來源認證， <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>請呼叫：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-223">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="8e7c4-224">HTTP 回應包含`Access-Control-Allow-Credentials`標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-224">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="8e7c4-225">如果瀏覽器傳送認證，但回應未包含有效`Access-Control-Allow-Credentials`的標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-225">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="8e7c4-226">允許跨原始來源認證會有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-226">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="8e7c4-227">另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-227">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="8e7c4-228">CORS 規格也會指出`"*"` `Access-Control-Allow-Credentials`如果標頭存在，將原始來源設定為（所有來源）是不正確。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-228">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="8e7c4-229">預檢要求</span><span class="sxs-lookup"><span data-stu-id="8e7c4-229">Preflight requests</span></span>

<span data-ttu-id="8e7c4-230">針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-230">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="8e7c4-231">此要求稱為*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-231">This request is called a *preflight request*.</span></span> <span data-ttu-id="8e7c4-232">當下列條件成立時，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-232">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="8e7c4-233">要求方法為 GET、HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-233">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="8e7c4-234">應用程式不會設定`Accept`、 `Accept-Language`、 `Content-Language` `Content-Type`、或`Last-Event-ID`以外的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-234">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="8e7c4-235">如果設定了標頭，則會有下列其中一個值： `Content-Type`</span><span class="sxs-lookup"><span data-stu-id="8e7c4-235">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="8e7c4-236">針對用戶端要求設定的要求標頭規則會套用至應用程式透過在`setRequestHeader` `XMLHttpRequest`物件上呼叫所設定的標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-236">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="8e7c4-237">CORS 規格會呼叫這些標頭的*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-237">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="8e7c4-238">此規則不適用於瀏覽器可以設定的標頭，例如`User-Agent`、 `Host`或`Content-Length`。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-238">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="8e7c4-239">以下是預檢要求的範例：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-239">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="8e7c4-240">預先飛行要求會使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-240">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="8e7c4-241">其中包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-241">It includes two special headers:</span></span>

* <span data-ttu-id="8e7c4-242">`Access-Control-Request-Method`：將用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-242">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="8e7c4-243">`Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-243">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="8e7c4-244">如先前所述，這不會包含瀏覽器所設定的標`User-Agent`頭，例如。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-244">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="8e7c4-245">CORS 預檢要求可能會包含`Access-Control-Request-Headers`標頭，這會向伺服器指出與實際要求一起傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-245">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="8e7c4-246">若要允許特定標頭<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>，請呼叫：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-246">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="8e7c4-247">若要允許所有作者要求標頭<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>，請呼叫：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-247">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="8e7c4-248">瀏覽器的設定`Access-Control-Request-Headers`方式並不完全一致。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-248">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="8e7c4-249">如果您將標頭`"*"`設定為（或使用<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>）以外的任何專案，您應該`Accept`至少`Content-Type`包含、 `Origin`和，再加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-249">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="8e7c4-250">以下是預檢要求的回應範例（假設伺服器允許此要求）：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-250">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="8e7c4-251">回應會包含`Access-Control-Allow-Methods`標頭，其中會列出允許的方法， `Access-Control-Allow-Headers`並選擇性地列出標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-251">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="8e7c4-252">如果預檢要求成功，瀏覽器就會傳送實際的要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-252">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="8e7c4-253">如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-253">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="8e7c4-254">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-254">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="8e7c4-255">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="8e7c4-255">Set the preflight expiration time</span></span>

<span data-ttu-id="8e7c4-256">`Access-Control-Max-Age`標頭會指定快取回應要求的時間長度。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-256">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="8e7c4-257">若要設定此標頭<xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>，請呼叫：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-257">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="8e7c4-258">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="8e7c4-258">How CORS works</span></span>

<span data-ttu-id="8e7c4-259">本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求中會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-259">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="8e7c4-260">CORS 是**不**安全的功能。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-260">CORS is **not** a security feature.</span></span> <span data-ttu-id="8e7c4-261">CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-261">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="8e7c4-262">例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-262">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="8e7c4-263">藉由允許 CORS，您的 API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-263">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="8e7c4-264">而是由用戶端（瀏覽器）強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-264">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="8e7c4-265">伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-265">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="8e7c4-266">例如，下列任何一項工具都會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-266">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="8e7c4-267">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8e7c4-267">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="8e7c4-268">Postman</span><span class="sxs-lookup"><span data-stu-id="8e7c4-268">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="8e7c4-269">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="8e7c4-269">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="8e7c4-270">網頁瀏覽器，方法是在網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-270">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="8e7c4-271">這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)要求的方式，否則會禁止。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-271">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="8e7c4-272">瀏覽器（不含 CORS）無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-272">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="8e7c4-273">在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-273">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="8e7c4-274">JSONP 不會使用 XHR，它會`<script>`使用標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-274">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="8e7c4-275">允許跨原始來源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-275">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="8e7c4-276">[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-276">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="8e7c4-277">如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-277">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="8e7c4-278">不需要自訂 JavaScript 程式碼來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-278">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="8e7c4-279">以下是跨原始來源要求的範例。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-279">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="8e7c4-280">`Origin`標頭會提供提出要求的網站網域：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-280">The `Origin` header provides the domain of the site that's making the request:</span></span>

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

<span data-ttu-id="8e7c4-281">如果伺服器允許此要求，它會在回應`Access-Control-Allow-Origin`中設定標頭。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-281">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="8e7c4-282">此標頭的值會比對`Origin`要求中的標頭，或為萬用字元`"*"`值，表示允許任何來源：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-282">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="8e7c4-283">如果回應不包含`Access-Control-Allow-Origin`標頭，則跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-283">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="8e7c4-284">具體而言，瀏覽器不允許此要求。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-284">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="8e7c4-285">即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-285">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="8e7c4-286">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="8e7c4-286">Test CORS</span></span>

<span data-ttu-id="8e7c4-287">測試 CORS：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-287">To test CORS:</span></span>

1. <span data-ttu-id="8e7c4-288">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-288">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="8e7c4-289">或者，您可以[下載範例](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-289">Alternatively, you can [download the sample](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="8e7c4-290">使用本檔中的其中一種方法來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-290">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="8e7c4-291">例如：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-291">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="8e7c4-292">`WithOrigins("https://localhost:<port>");`應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-292">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="8e7c4-293">建立 web 應用程式專案（Razor Pages 或 MVC）。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-293">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="8e7c4-294">此範例會使用 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-294">The sample uses Razor Pages.</span></span> <span data-ttu-id="8e7c4-295">您可以在與 API 專案相同的方案中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-295">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="8e7c4-296">將下列反白顯示的程式碼新增至*Index. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-296">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="8e7c4-297">在上述程式碼中， `url: 'https://<web app>.azurewebsites.net/api/values/1',`將取代為已部署應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-297">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="8e7c4-298">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-298">Deploy the API project.</span></span> <span data-ttu-id="8e7c4-299">例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-299">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="8e7c4-300">從桌面執行 Razor Pages 或 MVC 應用程式，然後按一下 [**測試**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-300">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="8e7c4-301">使用 F12 工具來檢查錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-301">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="8e7c4-302">從`WithOrigins`移除 localhost 來源，並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-302">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="8e7c4-303">或者，使用不同的埠來執行用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-303">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="8e7c4-304">例如，從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-304">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="8e7c4-305">使用用戶端應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-305">Test with the client app.</span></span> <span data-ttu-id="8e7c4-306">CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-306">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="8e7c4-307">使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e7c4-307">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="8e7c4-308">視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-308">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="8e7c4-309">使用 Microsoft Edge：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-309">Using Microsoft Edge:</span></span>

     <span data-ttu-id="8e7c4-310">**SEC7120： [CORS] 來源`https://localhost:44375`在的跨原始來源資源的存取控制-允許來源回應標頭中找不到`https://localhost:44375``https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="8e7c4-310">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="8e7c4-311">使用 Chrome：</span><span class="sxs-lookup"><span data-stu-id="8e7c4-311">Using Chrome:</span></span>

     <span data-ttu-id="8e7c4-312">**CORS 原則已封鎖`https://webapi.azurewebsites.net/api/values/1`從來源`https://localhost:44375`存取 XMLHttpRequest：要求的資源上沒有任何「存取控制-允許-來源」標頭。**</span><span class="sxs-lookup"><span data-stu-id="8e7c4-312">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8e7c4-313">其他資源</span><span class="sxs-lookup"><span data-stu-id="8e7c4-313">Additional resources</span></span>

* [<span data-ttu-id="8e7c4-314">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="8e7c4-314">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
