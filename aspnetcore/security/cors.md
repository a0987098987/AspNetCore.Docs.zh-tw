---
title: 啟用 ASP.NET Core 中的跨原始來源要求（CORS）
author: rick-anderson
description: 瞭解 CORS 如何作為標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始來源要求。
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: e0e0e1abf1ecaa12038b3ee1bdaa384d979be254
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78666247"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="fcc79-103">啟用 ASP.NET Core 中的跨原始來源要求（CORS）</span><span class="sxs-lookup"><span data-stu-id="fcc79-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

<span data-ttu-id="fcc79-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="fcc79-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="fcc79-105">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="fcc79-106">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="fcc79-107">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="fcc79-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="fcc79-108">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="fcc79-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="fcc79-109">有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="fcc79-110">如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="fcc79-111">[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：</span><span class="sxs-lookup"><span data-stu-id="fcc79-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="fcc79-112">是一種 W3C 標準，可讓伺服器放寬相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="fcc79-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="fcc79-113">**不**是安全性功能，CORS 放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="fcc79-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="fcc79-114">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="fcc79-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="fcc79-115">如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="fcc79-116">允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="fcc79-117">比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="fcc79-118">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fcc79-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="fcc79-119">相同原始來源</span><span class="sxs-lookup"><span data-stu-id="fcc79-119">Same origin</span></span>

<span data-ttu-id="fcc79-120">如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。</span><span class="sxs-lookup"><span data-stu-id="fcc79-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="fcc79-121">這兩個 Url 具有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="fcc79-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="fcc79-122">這些 Url 的來源不同于前兩個 Url：</span><span class="sxs-lookup"><span data-stu-id="fcc79-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="fcc79-123">`https://example.net` &ndash; 不同的網域</span><span class="sxs-lookup"><span data-stu-id="fcc79-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="fcc79-124">`https://www.example.com/foo.html` &ndash; 不同的子域</span><span class="sxs-lookup"><span data-stu-id="fcc79-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="fcc79-125">`http://example.com/foo.html` &ndash; 不同的配置</span><span class="sxs-lookup"><span data-stu-id="fcc79-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="fcc79-126">`https://example.com:9000/foo.html` &ndash; 不同的埠</span><span class="sxs-lookup"><span data-stu-id="fcc79-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="fcc79-127">在比較原始來源時，Internet Explorer 不會考慮此埠。</span><span class="sxs-lookup"><span data-stu-id="fcc79-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="fcc79-128">具有已命名原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="fcc79-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="fcc79-129">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="fcc79-130">下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="fcc79-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="fcc79-131">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="fcc79-131">The preceding code:</span></span>

* <span data-ttu-id="fcc79-132">將原則名稱設定為 "\_myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="fcc79-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="fcc79-133">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="fcc79-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="fcc79-134">呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，以啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="fcc79-135">使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)呼叫 <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>。</span><span class="sxs-lookup"><span data-stu-id="fcc79-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="fcc79-136">Lambda 會採用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。</span><span class="sxs-lookup"><span data-stu-id="fcc79-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="fcc79-137">本文稍後會說明設定[選項](#cors-policy-options)，例如 `WithOrigins`。</span><span class="sxs-lookup"><span data-stu-id="fcc79-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="fcc79-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> 方法呼叫會將 CORS 服務新增至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="fcc79-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="fcc79-139">如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="fcc79-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 方法可以連鎖方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="fcc79-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="fcc79-141">注意： URL**不**能包含尾端斜線（`/`）。</span><span class="sxs-lookup"><span data-stu-id="fcc79-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="fcc79-142">如果 URL 以 `/`終止，則比較會傳回 `false` 而且不會傳回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

::: moniker range=">= aspnetcore-3.0"

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="fcc79-143">將 CORS 原則套用至所有端點</span><span class="sxs-lookup"><span data-stu-id="fcc79-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="fcc79-144">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="fcc79-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
> <span data-ttu-id="fcc79-145">透過端點路由，必須設定 CORS 中介軟體，以便在 `UseRouting` 和 `UseEndpoints`的呼叫之間執行。</span><span class="sxs-lookup"><span data-stu-id="fcc79-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="fcc79-146">不正確的設定會導致中介軟體停止正常運作。</span><span class="sxs-lookup"><span data-stu-id="fcc79-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
<span data-ttu-id="fcc79-147">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="fcc79-147">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="fcc79-148">注意：必須在 `UseMvc`之前呼叫 `UseCors`。</span><span class="sxs-lookup"><span data-stu-id="fcc79-148">Note: `UseCors` must be called before `UseMvc`.</span></span>

::: moniker-end

<span data-ttu-id="fcc79-149">請參閱[在 Razor Pages、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。</span><span class="sxs-lookup"><span data-stu-id="fcc79-149">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="fcc79-150">如需測試上述程式碼的指示，請參閱[測試 CORS](#test) 。</span><span class="sxs-lookup"><span data-stu-id="fcc79-150">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

::: moniker range=">= aspnetcore-3.0"

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="fcc79-151">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="fcc79-151">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="fcc79-152">使用端點路由，可以使用一組 `RequireCors` 的擴充方法，針對每個端點來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-152">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="fcc79-153">同樣地，您也可以針對所有控制器啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="fcc79-153">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```
::: moniker-end

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="fcc79-154">啟用具有屬性的 CORS</span><span class="sxs-lookup"><span data-stu-id="fcc79-154">Enable CORS with attributes</span></span>

<span data-ttu-id="fcc79-155">[&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="fcc79-155">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="fcc79-156">`[EnableCors]` 屬性會啟用所選結束點的 CORS，而不是所有結束點。</span><span class="sxs-lookup"><span data-stu-id="fcc79-156">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="fcc79-157">使用 `[EnableCors]` 指定預設原則，並 `[EnableCors("{Policy String}")]` 指定原則。</span><span class="sxs-lookup"><span data-stu-id="fcc79-157">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="fcc79-158">`[EnableCors]` 屬性可以套用至：</span><span class="sxs-lookup"><span data-stu-id="fcc79-158">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="fcc79-159">Razor 頁面 `PageModel`</span><span class="sxs-lookup"><span data-stu-id="fcc79-159">Razor Page `PageModel`</span></span>
* <span data-ttu-id="fcc79-160">控制器</span><span class="sxs-lookup"><span data-stu-id="fcc79-160">Controller</span></span>
* <span data-ttu-id="fcc79-161">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="fcc79-161">Controller action method</span></span>

<span data-ttu-id="fcc79-162">您可以使用 `[EnableCors]` 屬性，將不同的原則套用至控制器/頁面模型/動作。</span><span class="sxs-lookup"><span data-stu-id="fcc79-162">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="fcc79-163">當 `[EnableCors]` 屬性套用至控制器/頁面模型/動作方法，而且在中介軟體中啟用 CORS 時，就會套用這兩個原則。</span><span class="sxs-lookup"><span data-stu-id="fcc79-163">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="fcc79-164">我們建議您不要結合原則。</span><span class="sxs-lookup"><span data-stu-id="fcc79-164">We recommend against combining policies.</span></span> <span data-ttu-id="fcc79-165">使用 `[EnableCors]` 屬性或中介軟體，而不是同時在相同的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="fcc79-165">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="fcc79-166">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="fcc79-166">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="fcc79-167">下列程式碼會建立 CORS 預設原則和名為 `"AnotherPolicy"`的原則：</span><span class="sxs-lookup"><span data-stu-id="fcc79-167">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="fcc79-168">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="fcc79-168">Disable CORS</span></span>

<span data-ttu-id="fcc79-169">[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性會停用控制器/頁面模型/動作的 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-169">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="fcc79-170">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="fcc79-170">CORS policy options</span></span>

<span data-ttu-id="fcc79-171">本節說明可在 CORS 原則中設定的各種選項：</span><span class="sxs-lookup"><span data-stu-id="fcc79-171">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="fcc79-172">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="fcc79-172">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="fcc79-173">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="fcc79-173">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="fcc79-174">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="fcc79-174">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="fcc79-175">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="fcc79-175">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="fcc79-176">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="fcc79-176">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="fcc79-177">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="fcc79-177">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="fcc79-178">`Startup.ConfigureServices`會呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>。</span><span class="sxs-lookup"><span data-stu-id="fcc79-178"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fcc79-179">針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="fcc79-179">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="fcc79-180">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="fcc79-180">Set the allowed origins</span></span>

<span data-ttu-id="fcc79-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許所有來源的 CORS 要求搭配任何配置（`http` 或 `https`）。</span><span class="sxs-lookup"><span data-stu-id="fcc79-181"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="fcc79-182">`AllowAnyOrigin` 並不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-182">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

::: moniker range=">= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="fcc79-183">指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-183">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fcc79-184">當應用程式已設定這兩種方法時，CORS 服務會傳回不正確 CORS 回應。</span><span class="sxs-lookup"><span data-stu-id="fcc79-184">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="fcc79-185">指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-185">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="fcc79-186">針對安全的應用程式，如果用戶端必須授權自己來存取伺服器資源，請指定來源的確切清單。</span><span class="sxs-lookup"><span data-stu-id="fcc79-186">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

::: moniker-end

<span data-ttu-id="fcc79-187">`AllowAnyOrigin` 會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-187">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="fcc79-188">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="fcc79-188">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="fcc79-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 會將原則的 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> 屬性設為函式，以便在評估是否允許來源時，讓原始來源符合已設定的萬用字元網域。</span><span class="sxs-lookup"><span data-stu-id="fcc79-189"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

::: moniker-end

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="fcc79-190">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="fcc79-190">Set the allowed HTTP methods</span></span>

<span data-ttu-id="fcc79-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="fcc79-191"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="fcc79-192">允許任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="fcc79-192">Allows any HTTP method:</span></span>
* <span data-ttu-id="fcc79-193">會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-193">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="fcc79-194">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="fcc79-194">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="fcc79-195">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="fcc79-195">Set the allowed request headers</span></span>

<span data-ttu-id="fcc79-196">若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="fcc79-196">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fcc79-197">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：</span><span class="sxs-lookup"><span data-stu-id="fcc79-197">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fcc79-198">此設定會影響預檢要求和 `Access-Control-Request-Headers` 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-198">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="fcc79-199">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="fcc79-199">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="fcc79-200">只有在 `Access-Control-Request-Headers` 中傳送的標頭完全符合 `WithHeaders`中所述的標頭時，才可以將 CORS 中介軟體原則符合 `WithHeaders` 指定的特定標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-200">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="fcc79-201">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="fcc79-201">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fcc79-202">CORS 中介軟體會拒絕具有下列要求標頭的預檢要求，因為 `Content-Language` （[HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)）未列在 `WithHeaders`中：</span><span class="sxs-lookup"><span data-stu-id="fcc79-202">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="fcc79-203">應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-203">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fcc79-204">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-204">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="fcc79-205">CORS 中介軟體一律允許傳送 `Access-Control-Request-Headers` 中的四個標頭，而不論 c 中設定的值為何。</span><span class="sxs-lookup"><span data-stu-id="fcc79-205">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="fcc79-206">此標頭清單包含：</span><span class="sxs-lookup"><span data-stu-id="fcc79-206">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="fcc79-207">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="fcc79-207">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="fcc79-208">CORS 中介軟體會以下列要求標頭成功回應預檢要求，因為 `Content-Language` 一律會列入允許清單：</span><span class="sxs-lookup"><span data-stu-id="fcc79-208">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

::: moniker-end

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="fcc79-209">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="fcc79-209">Set the exposed response headers</span></span>

<span data-ttu-id="fcc79-210">根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcc79-210">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="fcc79-211">如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-211">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="fcc79-212">預設可用的回應標頭為：</span><span class="sxs-lookup"><span data-stu-id="fcc79-212">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="fcc79-213">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="fcc79-213">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="fcc79-214">若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>：</span><span class="sxs-lookup"><span data-stu-id="fcc79-214">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="fcc79-215">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="fcc79-215">Credentials in cross-origin requests</span></span>

<span data-ttu-id="fcc79-216">認證需要在 CORS 要求中進行特殊處理。</span><span class="sxs-lookup"><span data-stu-id="fcc79-216">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="fcc79-217">根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="fcc79-217">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="fcc79-218">認證包括 cookie 和 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="fcc79-218">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="fcc79-219">若要使用跨原始來源要求傳送認證，用戶端必須將 `XMLHttpRequest.withCredentials` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="fcc79-219">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="fcc79-220">直接使用 `XMLHttpRequest`：</span><span class="sxs-lookup"><span data-stu-id="fcc79-220">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="fcc79-221">使用 jQuery：</span><span class="sxs-lookup"><span data-stu-id="fcc79-221">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="fcc79-222">使用[FETCH API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)：</span><span class="sxs-lookup"><span data-stu-id="fcc79-222">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="fcc79-223">伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="fcc79-223">The server must allow the credentials.</span></span> <span data-ttu-id="fcc79-224">若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>：</span><span class="sxs-lookup"><span data-stu-id="fcc79-224">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="fcc79-225">HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="fcc79-225">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="fcc79-226">如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="fcc79-226">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="fcc79-227">允許跨原始來源認證會有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="fcc79-227">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="fcc79-228">另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。</span><span class="sxs-lookup"><span data-stu-id="fcc79-228">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="fcc79-229">CORS 規格也指出，如果 `Access-Control-Allow-Credentials` 標頭存在，設定 `"*"` （所有來源）的來源會無效。</span><span class="sxs-lookup"><span data-stu-id="fcc79-229">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="fcc79-230">預檢要求</span><span class="sxs-lookup"><span data-stu-id="fcc79-230">Preflight requests</span></span>

<span data-ttu-id="fcc79-231">針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-231">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="fcc79-232">此要求稱為*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="fcc79-232">This request is called a *preflight request*.</span></span> <span data-ttu-id="fcc79-233">當下列條件成立時，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="fcc79-233">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="fcc79-234">要求方法為 GET、HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="fcc79-234">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="fcc79-235">應用程式不會設定 `Accept`、`Accept-Language`、`Content-Language`、`Content-Type`或 `Last-Event-ID`以外的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-235">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="fcc79-236">如果設定了 `Content-Type` 標頭，就會有下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="fcc79-236">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="fcc79-237">針對用戶端要求設定的要求標頭規則會套用至應用程式所設定的標頭，方法是呼叫 `XMLHttpRequest` 物件上的 `setRequestHeader`。</span><span class="sxs-lookup"><span data-stu-id="fcc79-237">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="fcc79-238">CORS 規格會呼叫這些標頭的*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="fcc79-238">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="fcc79-239">此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent`、`Host`或 `Content-Length`。</span><span class="sxs-lookup"><span data-stu-id="fcc79-239">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="fcc79-240">以下是預檢要求的範例：</span><span class="sxs-lookup"><span data-stu-id="fcc79-240">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="fcc79-241">預先飛行要求會使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="fcc79-241">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="fcc79-242">其中包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="fcc79-242">It includes two special headers:</span></span>

* <span data-ttu-id="fcc79-243">`Access-Control-Request-Method`：將用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="fcc79-243">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="fcc79-244">`Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。</span><span class="sxs-lookup"><span data-stu-id="fcc79-244">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="fcc79-245">如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="fcc79-245">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="fcc79-246">CORS 預檢要求可能會包含 `Access-Control-Request-Headers` 標頭，這會向伺服器指出與實際要求一起傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-246">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="fcc79-247">若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>：</span><span class="sxs-lookup"><span data-stu-id="fcc79-247">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="fcc79-248">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：</span><span class="sxs-lookup"><span data-stu-id="fcc79-248">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="fcc79-249">瀏覽器在 `Access-Control-Request-Headers`中的設定方式並不完全一致。</span><span class="sxs-lookup"><span data-stu-id="fcc79-249">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="fcc79-250">如果您將標頭設定為不是 `"*"` （或使用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>）以外的任何專案，您應該至少包含 `Accept`、`Content-Type`和 `Origin`，加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-250">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="fcc79-251">以下是預檢要求的回應範例（假設伺服器允許此要求）：</span><span class="sxs-lookup"><span data-stu-id="fcc79-251">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="fcc79-252">回應包含 `Access-Control-Allow-Methods` 標頭，其中會列出允許的方法，並可選擇是否使用 `Access-Control-Allow-Headers` 標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-252">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="fcc79-253">如果預檢要求成功，瀏覽器就會傳送實際的要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-253">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="fcc79-254">如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-254">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="fcc79-255">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-255">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="fcc79-256">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="fcc79-256">Set the preflight expiration time</span></span>

<span data-ttu-id="fcc79-257">`Access-Control-Max-Age` 標頭會指定可以快取對預檢要求回應的時間長度。</span><span class="sxs-lookup"><span data-stu-id="fcc79-257">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="fcc79-258">若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>：</span><span class="sxs-lookup"><span data-stu-id="fcc79-258">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="fcc79-259">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="fcc79-259">How CORS works</span></span>

<span data-ttu-id="fcc79-260">本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求中會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="fcc79-260">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="fcc79-261">CORS**不**是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="fcc79-261">CORS is **not** a security feature.</span></span> <span data-ttu-id="fcc79-262">CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。</span><span class="sxs-lookup"><span data-stu-id="fcc79-262">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="fcc79-263">例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="fcc79-263">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="fcc79-264">藉由允許 CORS，您的 API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="fcc79-264">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="fcc79-265">而是由用戶端（瀏覽器）強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-265">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="fcc79-266">伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。</span><span class="sxs-lookup"><span data-stu-id="fcc79-266">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="fcc79-267">例如，下列任何一項工具都會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="fcc79-267">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="fcc79-268">Fiddler</span><span class="sxs-lookup"><span data-stu-id="fcc79-268">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="fcc79-269">Postman</span><span class="sxs-lookup"><span data-stu-id="fcc79-269">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="fcc79-270">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="fcc79-270">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="fcc79-271">網頁瀏覽器，方法是在網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="fcc79-271">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="fcc79-272">這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)要求的方式，否則會禁止。</span><span class="sxs-lookup"><span data-stu-id="fcc79-272">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="fcc79-273">瀏覽器（不含 CORS）無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-273">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="fcc79-274">在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。</span><span class="sxs-lookup"><span data-stu-id="fcc79-274">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="fcc79-275">JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="fcc79-275">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="fcc79-276">允許跨原始來源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="fcc79-276">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="fcc79-277">[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-277">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="fcc79-278">如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-278">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="fcc79-279">不需要自訂 JavaScript 程式碼來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-279">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="fcc79-280">以下是跨原始來源要求的範例。</span><span class="sxs-lookup"><span data-stu-id="fcc79-280">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="fcc79-281">`Origin` 標頭會提供提出要求之網站的網域。</span><span class="sxs-lookup"><span data-stu-id="fcc79-281">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="fcc79-282">`Origin` 標頭是必要的，而且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="fcc79-282">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="fcc79-283">如果伺服器允許此要求，它會在回應中設定 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-283">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="fcc79-284">此標頭的值會比對要求中的 `Origin` 標頭，或為 `"*"`的萬用字元值，這表示允許任何來源：</span><span class="sxs-lookup"><span data-stu-id="fcc79-284">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="fcc79-285">如果回應未包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="fcc79-285">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="fcc79-286">具體而言，瀏覽器不允許此要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-286">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="fcc79-287">即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcc79-287">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="fcc79-288">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="fcc79-288">Test CORS</span></span>

<span data-ttu-id="fcc79-289">測試 CORS：</span><span class="sxs-lookup"><span data-stu-id="fcc79-289">To test CORS:</span></span>

1. <span data-ttu-id="fcc79-290">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-290">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="fcc79-291">或者，您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-291">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="fcc79-292">使用本檔中的其中一種方法來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="fcc79-292">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="fcc79-293">例如：</span><span class="sxs-lookup"><span data-stu-id="fcc79-293">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="fcc79-294">`WithOrigins("https://localhost:<port>");` 應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-294">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="fcc79-295">建立 web 應用程式專案（Razor Pages 或 MVC）。</span><span class="sxs-lookup"><span data-stu-id="fcc79-295">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="fcc79-296">此範例會使用 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="fcc79-296">The sample uses Razor Pages.</span></span> <span data-ttu-id="fcc79-297">您可以在與 API 專案相同的方案中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcc79-297">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="fcc79-298">將下列反白顯示的程式碼新增至*Index. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="fcc79-298">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="fcc79-299">在上述程式碼中，將 `url: 'https://<web app>.azurewebsites.net/api/values/1',` 取代為已部署應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="fcc79-299">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="fcc79-300">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="fcc79-300">Deploy the API project.</span></span> <span data-ttu-id="fcc79-301">例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-301">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="fcc79-302">從桌面執行 Razor Pages 或 MVC 應用程式，然後按一下 [**測試**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fcc79-302">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="fcc79-303">使用 F12 工具來檢查錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="fcc79-303">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="fcc79-304">從 `WithOrigins` 移除 localhost 來源，並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcc79-304">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="fcc79-305">或者，使用不同的埠來執行用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="fcc79-305">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="fcc79-306">例如，從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="fcc79-306">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="fcc79-307">使用用戶端應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="fcc79-307">Test with the client app.</span></span> <span data-ttu-id="fcc79-308">CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="fcc79-308">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="fcc79-309">使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。</span><span class="sxs-lookup"><span data-stu-id="fcc79-309">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="fcc79-310">視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：</span><span class="sxs-lookup"><span data-stu-id="fcc79-310">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="fcc79-311">使用 Microsoft Edge：</span><span class="sxs-lookup"><span data-stu-id="fcc79-311">Using Microsoft Edge:</span></span>

     <span data-ttu-id="fcc79-312">**SEC7120： [CORS] 來源 `https://localhost:44375` 在跨原始來源資源的存取控制-允許來源回應標頭中找不到 `https://localhost:44375` `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="fcc79-312">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="fcc79-313">使用 Chrome：</span><span class="sxs-lookup"><span data-stu-id="fcc79-313">Using Chrome:</span></span>

     <span data-ttu-id="fcc79-314">**來源 `https://localhost:44375` 的 `https://webapi.azurewebsites.net/api/values/1` 存取 XMLHttpRequest 已被 CORS 原則封鎖：要求的資源上沒有任何「存取控制-允許來源」標頭。**</span><span class="sxs-lookup"><span data-stu-id="fcc79-314">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="fcc79-315">具有 CORS 功能的端點可以使用工具（例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。</span><span class="sxs-lookup"><span data-stu-id="fcc79-315">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="fcc79-316">使用工具時，`Origin` 標頭所指定之要求的來源，必須與接收要求的主機不同。</span><span class="sxs-lookup"><span data-stu-id="fcc79-316">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="fcc79-317">如果要求不是根據 `Origin` 標頭的值而*跨原始來源*：</span><span class="sxs-lookup"><span data-stu-id="fcc79-317">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="fcc79-318">CORS 中介軟體不需要處理要求。</span><span class="sxs-lookup"><span data-stu-id="fcc79-318">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="fcc79-319">回應中不會傳回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="fcc79-319">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="fcc79-320">IIS 中的 CORS</span><span class="sxs-lookup"><span data-stu-id="fcc79-320">CORS in IIS</span></span>

<span data-ttu-id="fcc79-321">部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。</span><span class="sxs-lookup"><span data-stu-id="fcc79-321">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="fcc79-322">若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="fcc79-322">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="fcc79-323">其他資源</span><span class="sxs-lookup"><span data-stu-id="fcc79-323">Additional resources</span></span>

* [<span data-ttu-id="fcc79-324">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="fcc79-324">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="fcc79-325">IIS CORS 模組使用者入門</span><span class="sxs-lookup"><span data-stu-id="fcc79-325">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)
