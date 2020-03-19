---
title: 啟用 ASP.NET Core 中的跨原始來源要求（CORS）
author: rick-anderson
description: 瞭解 CORS 如何作為標準，以允許或拒絕 ASP.NET Core 應用程式中的跨原始來源要求。
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2020
uid: security/cors
ms.openlocfilehash: 09cc296ebf605907371619124cac00883beb6abb
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511570"
---
# <a name="enable-cross-origin-requests-cors-in-aspnet-core"></a><span data-ttu-id="7ffca-103">啟用 ASP.NET Core 中的跨原始來源要求（CORS）</span><span class="sxs-lookup"><span data-stu-id="7ffca-103">Enable Cross-Origin Requests (CORS) in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7ffca-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="7ffca-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ffca-105">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-105">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="7ffca-106">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-106">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="7ffca-107">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="7ffca-107">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="7ffca-108">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="7ffca-108">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="7ffca-109">有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-109">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="7ffca-110">如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-110">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="7ffca-111">[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：</span><span class="sxs-lookup"><span data-stu-id="7ffca-111">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="7ffca-112">是一種 W3C 標準，可讓伺服器放寬相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-112">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="7ffca-113">**不**是安全性功能，CORS 放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="7ffca-113">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="7ffca-114">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="7ffca-114">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="7ffca-115">如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-115">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="7ffca-116">允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-116">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="7ffca-117">比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-117">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="7ffca-118">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7ffca-118">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="7ffca-119">相同原始來源</span><span class="sxs-lookup"><span data-stu-id="7ffca-119">Same origin</span></span>

<span data-ttu-id="7ffca-120">如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。</span><span class="sxs-lookup"><span data-stu-id="7ffca-120">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="7ffca-121">這兩個 Url 具有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="7ffca-121">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="7ffca-122">這些 Url 的來源不同于前兩個 Url：</span><span class="sxs-lookup"><span data-stu-id="7ffca-122">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="7ffca-123">`https://example.net` &ndash; 不同的網域</span><span class="sxs-lookup"><span data-stu-id="7ffca-123">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="7ffca-124">`https://www.example.com/foo.html` &ndash; 不同的子域</span><span class="sxs-lookup"><span data-stu-id="7ffca-124">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="7ffca-125">`http://example.com/foo.html` &ndash; 不同的配置</span><span class="sxs-lookup"><span data-stu-id="7ffca-125">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="7ffca-126">`https://example.com:9000/foo.html` &ndash; 不同的埠</span><span class="sxs-lookup"><span data-stu-id="7ffca-126">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="7ffca-127">在比較原始來源時，Internet Explorer 不會考慮此埠。</span><span class="sxs-lookup"><span data-stu-id="7ffca-127">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="7ffca-128">具有已命名原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-128">CORS with named policy and middleware</span></span>

<span data-ttu-id="7ffca-129">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-129">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="7ffca-130">下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="7ffca-130">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="7ffca-131">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="7ffca-131">The preceding code:</span></span>

* <span data-ttu-id="7ffca-132">將原則名稱設定為 "\_myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="7ffca-132">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="7ffca-133">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="7ffca-133">The policy name is arbitrary.</span></span>
* <span data-ttu-id="7ffca-134">呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，以啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-134">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="7ffca-135">使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)呼叫 <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>。</span><span class="sxs-lookup"><span data-stu-id="7ffca-135">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="7ffca-136">Lambda 會採用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。</span><span class="sxs-lookup"><span data-stu-id="7ffca-136">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="7ffca-137">本文稍後會說明設定[選項](#cors-policy-options)，例如 `WithOrigins`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-137">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="7ffca-138"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> 方法呼叫會將 CORS 服務新增至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="7ffca-138">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="7ffca-139">如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-139">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="7ffca-140"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 方法可以連鎖方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="7ffca-140">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="7ffca-141">注意： URL**不**能包含尾端斜線（`/`）。</span><span class="sxs-lookup"><span data-stu-id="7ffca-141">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="7ffca-142">如果 URL 以 `/`終止，則比較會傳回 `false` 而且不會傳回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-142">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<a name="acpall"></a>

### <a name="apply-cors-policies-to-all-endpoints"></a><span data-ttu-id="7ffca-143">將 CORS 原則套用至所有端點</span><span class="sxs-lookup"><span data-stu-id="7ffca-143">Apply CORS policies to all endpoints</span></span>

<span data-ttu-id="7ffca-144">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="7ffca-144">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // Preceding code omitted.
    app.UseRouting();

    app.UseCors();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapControllers();
    });

    // Following code omitted.
}
```

> [!WARNING]
> <span data-ttu-id="7ffca-145">透過端點路由，必須設定 CORS 中介軟體，以便在 `UseRouting` 和 `UseEndpoints`的呼叫之間執行。</span><span class="sxs-lookup"><span data-stu-id="7ffca-145">With endpoint routing, the CORS middleware must be configured to execute between the calls to `UseRouting` and `UseEndpoints`.</span></span> <span data-ttu-id="7ffca-146">不正確的設定會導致中介軟體停止正常運作。</span><span class="sxs-lookup"><span data-stu-id="7ffca-146">Incorrect configuration will cause the middleware to stop functioning correctly.</span></span>

<span data-ttu-id="7ffca-147">請參閱[在 Razor Pages、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-147">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="7ffca-148">如需測試上述程式碼的指示，請參閱[測試 CORS](#test) 。</span><span class="sxs-lookup"><span data-stu-id="7ffca-148">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

<a name="ecors"></a>

## <a name="enable-cors-with-endpoint-routing"></a><span data-ttu-id="7ffca-149">使用端點路由來啟用 Cors</span><span class="sxs-lookup"><span data-stu-id="7ffca-149">Enable Cors with endpoint routing</span></span>

<span data-ttu-id="7ffca-150">使用端點路由，可以使用一組 `RequireCors` 的擴充方法，針對每個端點來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-150">With endpoint routing, CORS can be enabled on a per-endpoint basis using the `RequireCors` set of extension methods.</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapGet("/echo", async context => context.Response.WriteAsync("echo"))
    .RequireCors("policy-name");
});

```

<span data-ttu-id="7ffca-151">同樣地，您也可以針對所有控制器啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="7ffca-151">Similarly, CORS can also be enabled for all controllers:</span></span>

```csharp
app.UseEndpoints(endpoints =>
{
  endpoints.MapControllers().RequireCors("policy-name");
});
```

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="7ffca-152">啟用具有屬性的 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-152">Enable CORS with attributes</span></span>

<span data-ttu-id="7ffca-153">[&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="7ffca-153">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="7ffca-154">`[EnableCors]` 屬性會啟用所選結束點的 CORS，而不是所有結束點。</span><span class="sxs-lookup"><span data-stu-id="7ffca-154">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="7ffca-155">使用 `[EnableCors]` 指定預設原則，並 `[EnableCors("{Policy String}")]` 指定原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-155">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="7ffca-156">`[EnableCors]` 屬性可以套用至：</span><span class="sxs-lookup"><span data-stu-id="7ffca-156">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="7ffca-157">Razor 頁面 `PageModel`</span><span class="sxs-lookup"><span data-stu-id="7ffca-157">Razor Page `PageModel`</span></span>
* <span data-ttu-id="7ffca-158">控制器</span><span class="sxs-lookup"><span data-stu-id="7ffca-158">Controller</span></span>
* <span data-ttu-id="7ffca-159">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="7ffca-159">Controller action method</span></span>

<span data-ttu-id="7ffca-160">您可以使用 `[EnableCors]` 屬性，將不同的原則套用至控制器/頁面模型/動作。</span><span class="sxs-lookup"><span data-stu-id="7ffca-160">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="7ffca-161">當 `[EnableCors]` 屬性套用至控制器/頁面模型/動作方法，而且在中介軟體中啟用 CORS 時，就會套用這兩個原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-161">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="7ffca-162">我們建議您不要結合原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-162">We recommend against combining policies.</span></span> <span data-ttu-id="7ffca-163">使用 `[EnableCors]` 屬性或中介軟體，而不是同時在相同的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="7ffca-163">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="7ffca-164">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="7ffca-164">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="7ffca-165">下列程式碼會建立 CORS 預設原則和名為 `"AnotherPolicy"`的原則：</span><span class="sxs-lookup"><span data-stu-id="7ffca-165">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="7ffca-166">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-166">Disable CORS</span></span>

<span data-ttu-id="7ffca-167">[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性會停用控制器/頁面模型/動作的 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-167">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="7ffca-168">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="7ffca-168">CORS policy options</span></span>

<span data-ttu-id="7ffca-169">本節說明可在 CORS 原則中設定的各種選項：</span><span class="sxs-lookup"><span data-stu-id="7ffca-169">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="7ffca-170">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="7ffca-170">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="7ffca-171">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="7ffca-171">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="7ffca-172">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-172">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="7ffca-173">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-173">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="7ffca-174">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="7ffca-174">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="7ffca-175">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="7ffca-175">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="7ffca-176">`Startup.ConfigureServices`會呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>。</span><span class="sxs-lookup"><span data-stu-id="7ffca-176"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7ffca-177">針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="7ffca-177">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="7ffca-178">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="7ffca-178">Set the allowed origins</span></span>

<span data-ttu-id="7ffca-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許所有來源的 CORS 要求搭配任何配置（`http` 或 `https`）。</span><span class="sxs-lookup"><span data-stu-id="7ffca-179"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="7ffca-180">`AllowAnyOrigin` 並不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-180">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="7ffca-181">指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-181">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7ffca-182">當應用程式已設定這兩種方法時，CORS 服務會傳回不正確 CORS 回應。</span><span class="sxs-lookup"><span data-stu-id="7ffca-182">The CORS service returns an invalid CORS response when an app is configured with both methods.</span></span>

<span data-ttu-id="7ffca-183">`AllowAnyOrigin` 會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-183">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="7ffca-184">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="7ffca-184">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="7ffca-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 會將原則的 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> 屬性設為函式，以便在評估是否允許來源時，讓原始來源符合已設定的萬用字元網域。</span><span class="sxs-lookup"><span data-stu-id="7ffca-185"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="7ffca-186">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="7ffca-186">Set the allowed HTTP methods</span></span>

<span data-ttu-id="7ffca-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="7ffca-187"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="7ffca-188">允許任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="7ffca-188">Allows any HTTP method:</span></span>
* <span data-ttu-id="7ffca-189">會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-189">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="7ffca-190">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="7ffca-190">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="7ffca-191">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-191">Set the allowed request headers</span></span>

<span data-ttu-id="7ffca-192">若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="7ffca-192">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7ffca-193">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-193">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7ffca-194">此設定會影響預檢要求和 `Access-Control-Request-Headers` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-194">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="7ffca-195">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="7ffca-195">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>


<span data-ttu-id="7ffca-196">只有在 `Access-Control-Request-Headers` 中傳送的標頭完全符合 `WithHeaders`中所述的標頭時，才可以將 CORS 中介軟體原則符合 `WithHeaders` 指定的特定標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-196">A CORS Middleware policy match to specific headers specified by `WithHeaders` is only possible when the headers sent in `Access-Control-Request-Headers` exactly match the headers stated in `WithHeaders`.</span></span>

<span data-ttu-id="7ffca-197">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="7ffca-197">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7ffca-198">CORS 中介軟體會拒絕具有下列要求標頭的預檢要求，因為 `Content-Language` （[HeaderNames. ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)）未列在 `WithHeaders`中：</span><span class="sxs-lookup"><span data-stu-id="7ffca-198">CORS Middleware declines a preflight request with the following request header because `Content-Language` ([HeaderNames.ContentLanguage](xref:Microsoft.Net.Http.Headers.HeaderNames.ContentLanguage)) isn't listed in `WithHeaders`:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

<span data-ttu-id="7ffca-199">應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-199">The app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7ffca-200">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-200">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="7ffca-201">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-201">Set the exposed response headers</span></span>

<span data-ttu-id="7ffca-202">根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-202">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="7ffca-203">如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-203">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="7ffca-204">預設可用的回應標頭為：</span><span class="sxs-lookup"><span data-stu-id="7ffca-204">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="7ffca-205">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="7ffca-205">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="7ffca-206">若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-206">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="7ffca-207">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="7ffca-207">Credentials in cross-origin requests</span></span>

<span data-ttu-id="7ffca-208">認證需要在 CORS 要求中進行特殊處理。</span><span class="sxs-lookup"><span data-stu-id="7ffca-208">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="7ffca-209">根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="7ffca-209">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="7ffca-210">認證包括 cookie 和 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="7ffca-210">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="7ffca-211">若要使用跨原始來源要求傳送認證，用戶端必須將 `XMLHttpRequest.withCredentials` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-211">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="7ffca-212">直接使用 `XMLHttpRequest`：</span><span class="sxs-lookup"><span data-stu-id="7ffca-212">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="7ffca-213">使用 jQuery：</span><span class="sxs-lookup"><span data-stu-id="7ffca-213">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="7ffca-214">使用[FETCH API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)：</span><span class="sxs-lookup"><span data-stu-id="7ffca-214">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="7ffca-215">伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="7ffca-215">The server must allow the credentials.</span></span> <span data-ttu-id="7ffca-216">若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-216">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="7ffca-217">HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="7ffca-217">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="7ffca-218">如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="7ffca-218">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="7ffca-219">允許跨原始來源認證會有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="7ffca-219">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="7ffca-220">另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。</span><span class="sxs-lookup"><span data-stu-id="7ffca-220">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="7ffca-221">CORS 規格也指出，如果 `Access-Control-Allow-Credentials` 標頭存在，設定 `"*"` （所有來源）的來源會無效。</span><span class="sxs-lookup"><span data-stu-id="7ffca-221">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="7ffca-222">預檢要求</span><span class="sxs-lookup"><span data-stu-id="7ffca-222">Preflight requests</span></span>

<span data-ttu-id="7ffca-223">針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-223">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="7ffca-224">此要求稱為*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="7ffca-224">This request is called a *preflight request*.</span></span> <span data-ttu-id="7ffca-225">當下列條件成立時，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="7ffca-225">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="7ffca-226">要求方法為 GET、HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="7ffca-226">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="7ffca-227">應用程式不會設定 `Accept`、`Accept-Language`、`Content-Language`、`Content-Type`或 `Last-Event-ID`以外的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-227">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="7ffca-228">如果設定了 `Content-Type` 標頭，就會有下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="7ffca-228">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="7ffca-229">針對用戶端要求設定的要求標頭規則會套用至應用程式所設定的標頭，方法是呼叫 `XMLHttpRequest` 物件上的 `setRequestHeader`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-229">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="7ffca-230">CORS 規格會呼叫這些標頭的*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="7ffca-230">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="7ffca-231">此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent`、`Host`或 `Content-Length`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-231">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="7ffca-232">以下是預檢要求的範例：</span><span class="sxs-lookup"><span data-stu-id="7ffca-232">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="7ffca-233">預先飛行要求會使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="7ffca-233">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="7ffca-234">其中包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="7ffca-234">It includes two special headers:</span></span>

* <span data-ttu-id="7ffca-235">`Access-Control-Request-Method`：將用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="7ffca-235">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="7ffca-236">`Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。</span><span class="sxs-lookup"><span data-stu-id="7ffca-236">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="7ffca-237">如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-237">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="7ffca-238">CORS 預檢要求可能會包含 `Access-Control-Request-Headers` 標頭，這會向伺服器指出與實際要求一起傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-238">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="7ffca-239">若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-239">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7ffca-240">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-240">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7ffca-241">瀏覽器在 `Access-Control-Request-Headers`中的設定方式並不完全一致。</span><span class="sxs-lookup"><span data-stu-id="7ffca-241">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="7ffca-242">如果您將標頭設定為不是 `"*"` （或使用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>）以外的任何專案，您應該至少包含 `Accept`、`Content-Type`和 `Origin`，加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-242">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="7ffca-243">以下是預檢要求的回應範例（假設伺服器允許此要求）：</span><span class="sxs-lookup"><span data-stu-id="7ffca-243">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="7ffca-244">回應包含 `Access-Control-Allow-Methods` 標頭，其中會列出允許的方法，並可選擇是否使用 `Access-Control-Allow-Headers` 標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-244">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="7ffca-245">如果預檢要求成功，瀏覽器就會傳送實際的要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-245">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="7ffca-246">如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-246">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7ffca-247">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-247">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="7ffca-248">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="7ffca-248">Set the preflight expiration time</span></span>

<span data-ttu-id="7ffca-249">`Access-Control-Max-Age` 標頭會指定可以快取對預檢要求回應的時間長度。</span><span class="sxs-lookup"><span data-stu-id="7ffca-249">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="7ffca-250">若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-250">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="7ffca-251">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="7ffca-251">How CORS works</span></span>

<span data-ttu-id="7ffca-252">本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求中會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="7ffca-252">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="7ffca-253">CORS**不**是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="7ffca-253">CORS is **not** a security feature.</span></span> <span data-ttu-id="7ffca-254">CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-254">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="7ffca-255">例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="7ffca-255">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="7ffca-256">藉由允許 CORS，您的 API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="7ffca-256">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="7ffca-257">而是由用戶端（瀏覽器）強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-257">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="7ffca-258">伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。</span><span class="sxs-lookup"><span data-stu-id="7ffca-258">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="7ffca-259">例如，下列任何一項工具都會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="7ffca-259">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="7ffca-260">Fiddler</span><span class="sxs-lookup"><span data-stu-id="7ffca-260">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="7ffca-261">Postman</span><span class="sxs-lookup"><span data-stu-id="7ffca-261">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="7ffca-262">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="7ffca-262">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="7ffca-263">網頁瀏覽器，方法是在網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="7ffca-263">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="7ffca-264">這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)要求的方式，否則會禁止。</span><span class="sxs-lookup"><span data-stu-id="7ffca-264">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="7ffca-265">瀏覽器（不含 CORS）無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-265">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="7ffca-266">在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。</span><span class="sxs-lookup"><span data-stu-id="7ffca-266">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="7ffca-267">JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="7ffca-267">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="7ffca-268">允許跨原始來源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="7ffca-268">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="7ffca-269">[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-269">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="7ffca-270">如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-270">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="7ffca-271">不需要自訂 JavaScript 程式碼來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-271">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="7ffca-272">以下是跨原始來源要求的範例。</span><span class="sxs-lookup"><span data-stu-id="7ffca-272">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="7ffca-273">`Origin` 標頭會提供提出要求之網站的網域。</span><span class="sxs-lookup"><span data-stu-id="7ffca-273">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="7ffca-274">`Origin` 標頭是必要的，而且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="7ffca-274">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="7ffca-275">如果伺服器允許此要求，它會在回應中設定 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-275">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="7ffca-276">此標頭的值會比對要求中的 `Origin` 標頭，或為 `"*"`的萬用字元值，這表示允許任何來源：</span><span class="sxs-lookup"><span data-stu-id="7ffca-276">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="7ffca-277">如果回應未包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="7ffca-277">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="7ffca-278">具體而言，瀏覽器不允許此要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-278">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="7ffca-279">即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-279">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="7ffca-280">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-280">Test CORS</span></span>

<span data-ttu-id="7ffca-281">測試 CORS：</span><span class="sxs-lookup"><span data-stu-id="7ffca-281">To test CORS:</span></span>

1. <span data-ttu-id="7ffca-282">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-282">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="7ffca-283">或者，您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-283">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="7ffca-284">使用本檔中的其中一種方法來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-284">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="7ffca-285">例如：</span><span class="sxs-lookup"><span data-stu-id="7ffca-285">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="7ffca-286">`WithOrigins("https://localhost:<port>");` 應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-286">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="7ffca-287">建立 web 應用程式專案（Razor Pages 或 MVC）。</span><span class="sxs-lookup"><span data-stu-id="7ffca-287">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="7ffca-288">此範例會使用 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="7ffca-288">The sample uses Razor Pages.</span></span> <span data-ttu-id="7ffca-289">您可以在與 API 專案相同的方案中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-289">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="7ffca-290">將下列反白顯示的程式碼新增至*Index. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="7ffca-290">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="7ffca-291">在上述程式碼中，將 `url: 'https://<web app>.azurewebsites.net/api/values/1',` 取代為已部署應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="7ffca-291">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="7ffca-292">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="7ffca-292">Deploy the API project.</span></span> <span data-ttu-id="7ffca-293">例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-293">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="7ffca-294">從桌面執行 Razor Pages 或 MVC 應用程式，然後按一下 [**測試**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7ffca-294">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="7ffca-295">使用 F12 工具來檢查錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7ffca-295">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="7ffca-296">從 `WithOrigins` 移除 localhost 來源，並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-296">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="7ffca-297">或者，使用不同的埠來執行用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-297">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="7ffca-298">例如，從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="7ffca-298">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="7ffca-299">使用用戶端應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="7ffca-299">Test with the client app.</span></span> <span data-ttu-id="7ffca-300">CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7ffca-300">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="7ffca-301">使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。</span><span class="sxs-lookup"><span data-stu-id="7ffca-301">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="7ffca-302">視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：</span><span class="sxs-lookup"><span data-stu-id="7ffca-302">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="7ffca-303">使用 Microsoft Edge：</span><span class="sxs-lookup"><span data-stu-id="7ffca-303">Using Microsoft Edge:</span></span>

     <span data-ttu-id="7ffca-304">**SEC7120： [CORS] 來源 `https://localhost:44375` 在跨原始來源資源的存取控制-允許來源回應標頭中找不到 `https://localhost:44375` `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="7ffca-304">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="7ffca-305">使用 Chrome：</span><span class="sxs-lookup"><span data-stu-id="7ffca-305">Using Chrome:</span></span>

     <span data-ttu-id="7ffca-306">**來源 `https://localhost:44375` 的 `https://webapi.azurewebsites.net/api/values/1` 存取 XMLHttpRequest 已被 CORS 原則封鎖：要求的資源上沒有任何「存取控制-允許來源」標頭。**</span><span class="sxs-lookup"><span data-stu-id="7ffca-306">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="7ffca-307">具有 CORS 功能的端點可以使用工具（例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。</span><span class="sxs-lookup"><span data-stu-id="7ffca-307">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="7ffca-308">使用工具時，`Origin` 標頭所指定之要求的來源，必須與接收要求的主機不同。</span><span class="sxs-lookup"><span data-stu-id="7ffca-308">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="7ffca-309">如果要求不是根據 `Origin` 標頭的值而*跨原始來源*：</span><span class="sxs-lookup"><span data-stu-id="7ffca-309">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="7ffca-310">CORS 中介軟體不需要處理要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-310">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="7ffca-311">回應中不會傳回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-311">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="7ffca-312">IIS 中的 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-312">CORS in IIS</span></span>

<span data-ttu-id="7ffca-313">部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。</span><span class="sxs-lookup"><span data-stu-id="7ffca-313">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="7ffca-314">若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-314">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ffca-315">其他資源</span><span class="sxs-lookup"><span data-stu-id="7ffca-315">Additional resources</span></span>

* [<span data-ttu-id="7ffca-316">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="7ffca-316">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="7ffca-317">IIS CORS 模組使用者入門</span><span class="sxs-lookup"><span data-stu-id="7ffca-317">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7ffca-318">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="7ffca-318">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ffca-319">本文說明如何在 ASP.NET Core 應用程式中啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-319">This article shows how to enable CORS in an ASP.NET Core app.</span></span>

<span data-ttu-id="7ffca-320">瀏覽器安全性可防止網頁向不同于服務網頁的網域提出要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-320">Browser security prevents a web page from making requests to a different domain than the one that served the web page.</span></span> <span data-ttu-id="7ffca-321">這種限制稱為「*相同來源原則*」。</span><span class="sxs-lookup"><span data-stu-id="7ffca-321">This restriction is called the *same-origin policy*.</span></span> <span data-ttu-id="7ffca-322">相同來源的原則可防止惡意網站從另一個網站讀取敏感性資料。</span><span class="sxs-lookup"><span data-stu-id="7ffca-322">The same-origin policy prevents a malicious site from reading sensitive data from another site.</span></span> <span data-ttu-id="7ffca-323">有時候，您可能會想要允許其他網站向您的應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-323">Sometimes, you might want to allow other sites make cross-origin requests to your app.</span></span> <span data-ttu-id="7ffca-324">如需詳細資訊，請參閱[MOZILLA CORS 一文](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-324">For more information, see the [Mozilla CORS article](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS).</span></span>

<span data-ttu-id="7ffca-325">[跨原始來源資源分享](https://www.w3.org/TR/cors/)（CORS）：</span><span class="sxs-lookup"><span data-stu-id="7ffca-325">[Cross Origin Resource Sharing](https://www.w3.org/TR/cors/) (CORS):</span></span>

* <span data-ttu-id="7ffca-326">是一種 W3C 標準，可讓伺服器放寬相同的來源原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-326">Is a W3C standard that allows a server to relax the same-origin policy.</span></span>
* <span data-ttu-id="7ffca-327">**不**是安全性功能，CORS 放寬安全性。</span><span class="sxs-lookup"><span data-stu-id="7ffca-327">Is **not** a security feature, CORS relaxes security.</span></span> <span data-ttu-id="7ffca-328">藉由允許 CORS，API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="7ffca-328">An API is not safer by allowing CORS.</span></span> <span data-ttu-id="7ffca-329">如需詳細資訊，請參閱[CORS 的運作方式](#how-cors)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-329">For more information, see [How CORS works](#how-cors).</span></span>
* <span data-ttu-id="7ffca-330">允許伺服器明確允許某些跨原始來源要求，同時拒絕其他要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-330">Allows a server to explicitly allow some cross-origin requests while rejecting others.</span></span>
* <span data-ttu-id="7ffca-331">比先前的技術更安全且更具彈性，例如[JSONP](/dotnet/framework/wcf/samples/jsonp)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-331">Is safer and more flexible than earlier techniques, such as [JSONP](/dotnet/framework/wcf/samples/jsonp).</span></span>

<span data-ttu-id="7ffca-332">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7ffca-332">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="same-origin"></a><span data-ttu-id="7ffca-333">相同原始來源</span><span class="sxs-lookup"><span data-stu-id="7ffca-333">Same origin</span></span>

<span data-ttu-id="7ffca-334">如果兩個 Url 具有相同的配置、主機和埠（[RFC 6454](https://tools.ietf.org/html/rfc6454)），就會有相同的來源。</span><span class="sxs-lookup"><span data-stu-id="7ffca-334">Two URLs have the same origin if they have identical schemes, hosts, and ports ([RFC 6454](https://tools.ietf.org/html/rfc6454)).</span></span>

<span data-ttu-id="7ffca-335">這兩個 Url 具有相同的來源：</span><span class="sxs-lookup"><span data-stu-id="7ffca-335">These two URLs have the same origin:</span></span>

* `https://example.com/foo.html`
* `https://example.com/bar.html`

<span data-ttu-id="7ffca-336">這些 Url 的來源不同于前兩個 Url：</span><span class="sxs-lookup"><span data-stu-id="7ffca-336">These URLs have different origins than the previous two URLs:</span></span>

* <span data-ttu-id="7ffca-337">`https://example.net` &ndash; 不同的網域</span><span class="sxs-lookup"><span data-stu-id="7ffca-337">`https://example.net` &ndash; Different domain</span></span>
* <span data-ttu-id="7ffca-338">`https://www.example.com/foo.html` &ndash; 不同的子域</span><span class="sxs-lookup"><span data-stu-id="7ffca-338">`https://www.example.com/foo.html` &ndash; Different subdomain</span></span>
* <span data-ttu-id="7ffca-339">`http://example.com/foo.html` &ndash; 不同的配置</span><span class="sxs-lookup"><span data-stu-id="7ffca-339">`http://example.com/foo.html` &ndash; Different scheme</span></span>
* <span data-ttu-id="7ffca-340">`https://example.com:9000/foo.html` &ndash; 不同的埠</span><span class="sxs-lookup"><span data-stu-id="7ffca-340">`https://example.com:9000/foo.html` &ndash; Different port</span></span>

<span data-ttu-id="7ffca-341">在比較原始來源時，Internet Explorer 不會考慮此埠。</span><span class="sxs-lookup"><span data-stu-id="7ffca-341">Internet Explorer doesn't consider the port when comparing origins.</span></span>

## <a name="cors-with-named-policy-and-middleware"></a><span data-ttu-id="7ffca-342">具有已命名原則和中介軟體的 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-342">CORS with named policy and middleware</span></span>

<span data-ttu-id="7ffca-343">CORS 中介軟體會處理跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-343">CORS Middleware handles cross-origin requests.</span></span> <span data-ttu-id="7ffca-344">下列程式碼會針對具有指定來源的整個應用程式啟用 CORS：</span><span class="sxs-lookup"><span data-stu-id="7ffca-344">The following code enables CORS for the entire app with the specified origin:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet&highlight=8,14-23,38)]

<span data-ttu-id="7ffca-345">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="7ffca-345">The preceding code:</span></span>

* <span data-ttu-id="7ffca-346">將原則名稱設定為 "\_myAllowSpecificOrigins"。</span><span class="sxs-lookup"><span data-stu-id="7ffca-346">Sets the policy name to "\_myAllowSpecificOrigins".</span></span> <span data-ttu-id="7ffca-347">原則名稱是任意的。</span><span class="sxs-lookup"><span data-stu-id="7ffca-347">The policy name is arbitrary.</span></span>
* <span data-ttu-id="7ffca-348">呼叫 <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> 擴充方法，以啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-348">Calls the <xref:Microsoft.AspNetCore.Builder.CorsMiddlewareExtensions.UseCors*> extension method, which enables CORS.</span></span>
* <span data-ttu-id="7ffca-349">使用[lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)呼叫 <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*>。</span><span class="sxs-lookup"><span data-stu-id="7ffca-349">Calls <xref:Microsoft.Extensions.DependencyInjection.CorsServiceCollectionExtensions.AddCors*> with a [lambda expression](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions).</span></span> <span data-ttu-id="7ffca-350">Lambda 會採用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 物件。</span><span class="sxs-lookup"><span data-stu-id="7ffca-350">The lambda takes a <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> object.</span></span> <span data-ttu-id="7ffca-351">本文稍後會說明設定[選項](#cors-policy-options)，例如 `WithOrigins`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-351">[Configuration options](#cors-policy-options), such as `WithOrigins`, are described later in this article.</span></span>

<span data-ttu-id="7ffca-352"><xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> 方法呼叫會將 CORS 服務新增至應用程式的服務容器：</span><span class="sxs-lookup"><span data-stu-id="7ffca-352">The <xref:Microsoft.Extensions.DependencyInjection.MvcCorsMvcCoreBuilderExtensions.AddCors*> method call adds CORS services to the app's service container:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup.cs?name=snippet2)]

<span data-ttu-id="7ffca-353">如需詳細資訊，請參閱本檔中的[CORS 原則選項](#cpo)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-353">For more information, see [CORS policy options](#cpo) in this document .</span></span>

<span data-ttu-id="7ffca-354"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> 方法可以連鎖方法，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="7ffca-354">The <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder> method can chain methods, as shown in the following code:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Startup2.cs?name=snippet2)]

<span data-ttu-id="7ffca-355">注意： URL**不**能包含尾端斜線（`/`）。</span><span class="sxs-lookup"><span data-stu-id="7ffca-355">Note: The URL must **not** contain a trailing slash (`/`).</span></span> <span data-ttu-id="7ffca-356">如果 URL 以 `/`終止，則比較會傳回 `false` 而且不會傳回任何標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-356">If the URL terminates with `/`, the comparison returns `false` and no header is returned.</span></span>

<span data-ttu-id="7ffca-357">下列程式碼會透過 CORS 中介軟體將 CORS 原則套用至所有應用程式端點：</span><span class="sxs-lookup"><span data-stu-id="7ffca-357">The following code applies CORS policies to all the apps endpoints via CORS Middleware:</span></span>
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
<span data-ttu-id="7ffca-358">注意：必須在 `UseMvc`之前呼叫 `UseCors`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-358">Note: `UseCors` must be called before `UseMvc`.</span></span>

<span data-ttu-id="7ffca-359">請參閱[在 Razor Pages、控制器和動作方法中啟用 cors](#ecors) ，以在頁面/控制器/動作層級套用 cors 原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-359">See [Enable CORS in Razor Pages, controllers, and action methods](#ecors) to apply CORS policy at the page/controller/action level.</span></span>

<span data-ttu-id="7ffca-360">如需測試上述程式碼的指示，請參閱[測試 CORS](#test) 。</span><span class="sxs-lookup"><span data-stu-id="7ffca-360">See [Test CORS](#test) for instructions on testing the preceding code.</span></span>

## <a name="enable-cors-with-attributes"></a><span data-ttu-id="7ffca-361">啟用具有屬性的 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-361">Enable CORS with attributes</span></span>

<span data-ttu-id="7ffca-362">[&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute)屬性提供全域套用 CORS 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="7ffca-362">The [&lbrack;EnableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.EnableCorsAttribute) attribute provides an alternative to applying CORS globally.</span></span> <span data-ttu-id="7ffca-363">`[EnableCors]` 屬性會啟用所選結束點的 CORS，而不是所有結束點。</span><span class="sxs-lookup"><span data-stu-id="7ffca-363">The `[EnableCors]` attribute enables CORS for selected end points, rather than all end points.</span></span>

<span data-ttu-id="7ffca-364">使用 `[EnableCors]` 指定預設原則，並 `[EnableCors("{Policy String}")]` 指定原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-364">Use `[EnableCors]` to specify the default policy and `[EnableCors("{Policy String}")]` to specify a policy.</span></span>

<span data-ttu-id="7ffca-365">`[EnableCors]` 屬性可以套用至：</span><span class="sxs-lookup"><span data-stu-id="7ffca-365">The `[EnableCors]` attribute can be applied to:</span></span>

* <span data-ttu-id="7ffca-366">Razor 頁面 `PageModel`</span><span class="sxs-lookup"><span data-stu-id="7ffca-366">Razor Page `PageModel`</span></span>
* <span data-ttu-id="7ffca-367">控制器</span><span class="sxs-lookup"><span data-stu-id="7ffca-367">Controller</span></span>
* <span data-ttu-id="7ffca-368">控制器動作方法</span><span class="sxs-lookup"><span data-stu-id="7ffca-368">Controller action method</span></span>

<span data-ttu-id="7ffca-369">您可以使用 `[EnableCors]` 屬性，將不同的原則套用至控制器/頁面模型/動作。</span><span class="sxs-lookup"><span data-stu-id="7ffca-369">You can apply different policies to controller/page-model/action with the  `[EnableCors]` attribute.</span></span> <span data-ttu-id="7ffca-370">當 `[EnableCors]` 屬性套用至控制器/頁面模型/動作方法，而且在中介軟體中啟用 CORS 時，就會套用這兩個原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-370">When the `[EnableCors]` attribute is applied to a controllers/page-model/action method, and CORS is enabled in middleware, both policies are applied.</span></span> <span data-ttu-id="7ffca-371">我們建議您不要結合原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-371">We recommend against combining policies.</span></span> <span data-ttu-id="7ffca-372">使用 `[EnableCors]` 屬性或中介軟體，而不是同時在相同的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="7ffca-372">Use the `[EnableCors]` attribute or middleware, not both in the same app.</span></span>

<span data-ttu-id="7ffca-373">下列程式碼會將不同的原則套用至每個方法：</span><span class="sxs-lookup"><span data-stu-id="7ffca-373">The following code applies a different policy to each method:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/Controllers/WidgetController.cs?name=snippet&highlight=6,14)]

<span data-ttu-id="7ffca-374">下列程式碼會建立 CORS 預設原則和名為 `"AnotherPolicy"`的原則：</span><span class="sxs-lookup"><span data-stu-id="7ffca-374">The following code creates a CORS default policy and a policy named `"AnotherPolicy"`:</span></span>

[!code-csharp[](cors/sample/Cors/WebAPI/StartupMultiPolicy.cs?name=snippet&highlight=12-28)]

### <a name="disable-cors"></a><span data-ttu-id="7ffca-375">停用 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-375">Disable CORS</span></span>

<span data-ttu-id="7ffca-376">[&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute)屬性會停用控制器/頁面模型/動作的 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-376">The [&lbrack;DisableCors&rbrack;](xref:Microsoft.AspNetCore.Cors.DisableCorsAttribute) attribute disables CORS for the controller/page-model/action.</span></span>

<a name="cpo"></a>

## <a name="cors-policy-options"></a><span data-ttu-id="7ffca-377">CORS 原則選項</span><span class="sxs-lookup"><span data-stu-id="7ffca-377">CORS policy options</span></span>

<span data-ttu-id="7ffca-378">本節說明可在 CORS 原則中設定的各種選項：</span><span class="sxs-lookup"><span data-stu-id="7ffca-378">This section describes the various options that can be set in a CORS policy:</span></span>

* [<span data-ttu-id="7ffca-379">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="7ffca-379">Set the allowed origins</span></span>](#set-the-allowed-origins)
* [<span data-ttu-id="7ffca-380">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="7ffca-380">Set the allowed HTTP methods</span></span>](#set-the-allowed-http-methods)
* [<span data-ttu-id="7ffca-381">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-381">Set the allowed request headers</span></span>](#set-the-allowed-request-headers)
* [<span data-ttu-id="7ffca-382">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-382">Set the exposed response headers</span></span>](#set-the-exposed-response-headers)
* [<span data-ttu-id="7ffca-383">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="7ffca-383">Credentials in cross-origin requests</span></span>](#credentials-in-cross-origin-requests)
* [<span data-ttu-id="7ffca-384">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="7ffca-384">Set the preflight expiration time</span></span>](#set-the-preflight-expiration-time)

<span data-ttu-id="7ffca-385">`Startup.ConfigureServices`會呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*>。</span><span class="sxs-lookup"><span data-stu-id="7ffca-385"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsOptions.AddPolicy*> is called in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="7ffca-386">針對某些選項，閱讀[CORS 的運作方式](#how-cors)一節可能會很有説明。</span><span class="sxs-lookup"><span data-stu-id="7ffca-386">For some options, it may be helpful to read the [How CORS works](#how-cors) section first.</span></span>

## <a name="set-the-allowed-origins"></a><span data-ttu-id="7ffca-387">設定允許的原始來源</span><span class="sxs-lookup"><span data-stu-id="7ffca-387">Set the allowed origins</span></span>

<span data-ttu-id="7ffca-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; 允許所有來源的 CORS 要求搭配任何配置（`http` 或 `https`）。</span><span class="sxs-lookup"><span data-stu-id="7ffca-388"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyOrigin*> &ndash; Allows CORS requests from all origins with any scheme (`http` or `https`).</span></span> <span data-ttu-id="7ffca-389">`AllowAnyOrigin` 並不安全，因為*任何網站*都可以對應用程式發出跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-389">`AllowAnyOrigin` is insecure because *any website* can make cross-origin requests to the app.</span></span>

> [!NOTE]
> <span data-ttu-id="7ffca-390">指定 `AllowAnyOrigin` 和 `AllowCredentials` 是不安全的設定，可能會導致跨網站偽造要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-390">Specifying `AllowAnyOrigin` and `AllowCredentials` is an insecure configuration and can result in cross-site request forgery.</span></span> <span data-ttu-id="7ffca-391">針對安全的應用程式，如果用戶端必須授權自己來存取伺服器資源，請指定來源的確切清單。</span><span class="sxs-lookup"><span data-stu-id="7ffca-391">For a secure app, specify an exact list of origins if the client must authorize itself to access server resources.</span></span>

<span data-ttu-id="7ffca-392">`AllowAnyOrigin` 會影響預檢要求和 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-392">`AllowAnyOrigin` affects preflight requests and the `Access-Control-Allow-Origin` header.</span></span> <span data-ttu-id="7ffca-393">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="7ffca-393">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="7ffca-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; 會將原則的 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> 屬性設為函式，以便在評估是否允許來源時，讓原始來源符合已設定的萬用字元網域。</span><span class="sxs-lookup"><span data-stu-id="7ffca-394"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetIsOriginAllowedToAllowWildcardSubdomains*> &ndash; Sets the <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.IsOriginAllowed*> property of the policy to be a function that allows origins to match a configured wildcard domain when evaluating if the origin is allowed.</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=100-105&highlight=4-5)]

### <a name="set-the-allowed-http-methods"></a><span data-ttu-id="7ffca-395">設定允許的 HTTP 方法</span><span class="sxs-lookup"><span data-stu-id="7ffca-395">Set the allowed HTTP methods</span></span>

<span data-ttu-id="7ffca-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span><span class="sxs-lookup"><span data-stu-id="7ffca-396"><xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyMethod*>:</span></span>

* <span data-ttu-id="7ffca-397">允許任何 HTTP 方法：</span><span class="sxs-lookup"><span data-stu-id="7ffca-397">Allows any HTTP method:</span></span>
* <span data-ttu-id="7ffca-398">會影響預檢要求和 `Access-Control-Allow-Methods` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-398">Affects preflight requests and the `Access-Control-Allow-Methods` header.</span></span> <span data-ttu-id="7ffca-399">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="7ffca-399">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

### <a name="set-the-allowed-request-headers"></a><span data-ttu-id="7ffca-400">設定允許的要求標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-400">Set the allowed request headers</span></span>

<span data-ttu-id="7ffca-401">若要允許在 CORS 要求中傳送特定標頭（稱為*author 要求標頭*），請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> 並指定允許的標頭：</span><span class="sxs-lookup"><span data-stu-id="7ffca-401">To allow specific headers to be sent in a CORS request, called *author request headers*, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*> and specify the allowed headers:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7ffca-402">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-402">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7ffca-403">此設定會影響預檢要求和 `Access-Control-Request-Headers` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-403">This setting affects preflight requests and the `Access-Control-Request-Headers` header.</span></span> <span data-ttu-id="7ffca-404">如需詳細資訊，請參閱[預檢要求](#preflight-requests)一節。</span><span class="sxs-lookup"><span data-stu-id="7ffca-404">For more information, see the [Preflight requests](#preflight-requests) section.</span></span>

<span data-ttu-id="7ffca-405">CORS 中介軟體一律允許傳送 `Access-Control-Request-Headers` 中的四個標頭，而不論 c 中設定的值為何。</span><span class="sxs-lookup"><span data-stu-id="7ffca-405">CORS Middleware always allows four headers in the `Access-Control-Request-Headers` to be sent regardless of the values configured in CorsPolicy.Headers.</span></span> <span data-ttu-id="7ffca-406">此標頭清單包含：</span><span class="sxs-lookup"><span data-stu-id="7ffca-406">This list of headers includes:</span></span>

* `Accept`
* `Accept-Language`
* `Content-Language`
* `Origin`

<span data-ttu-id="7ffca-407">例如，假設有一個應用程式設定如下：</span><span class="sxs-lookup"><span data-stu-id="7ffca-407">For instance, consider an app configured as follows:</span></span>

```csharp
app.UseCors(policy => policy.WithHeaders(HeaderNames.CacheControl));
```

<span data-ttu-id="7ffca-408">CORS 中介軟體會以下列要求標頭成功回應預檢要求，因為 `Content-Language` 一律會列入允許清單：</span><span class="sxs-lookup"><span data-stu-id="7ffca-408">CORS Middleware responds successfully to a preflight request with the following request header because `Content-Language` is always whitelisted:</span></span>

```
Access-Control-Request-Headers: Cache-Control, Content-Language
```

### <a name="set-the-exposed-response-headers"></a><span data-ttu-id="7ffca-409">設定公開的回應標頭</span><span class="sxs-lookup"><span data-stu-id="7ffca-409">Set the exposed response headers</span></span>

<span data-ttu-id="7ffca-410">根據預設，瀏覽器不會將所有的回應標頭公開給應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-410">By default, the browser doesn't expose all of the response headers to the app.</span></span> <span data-ttu-id="7ffca-411">如需詳細資訊，請參閱[W3C 跨原始來源資源分享（術語）：簡單的回應標頭](https://www.w3.org/TR/cors/#simple-response-header)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-411">For more information, see [W3C Cross-Origin Resource Sharing (Terminology): Simple Response Header](https://www.w3.org/TR/cors/#simple-response-header).</span></span>

<span data-ttu-id="7ffca-412">預設可用的回應標頭為：</span><span class="sxs-lookup"><span data-stu-id="7ffca-412">The response headers that are available by default are:</span></span>

* `Cache-Control`
* `Content-Language`
* `Content-Type`
* `Expires`
* `Last-Modified`
* `Pragma`

<span data-ttu-id="7ffca-413">CORS 規格會呼叫這些標頭*簡單的回應標頭*。</span><span class="sxs-lookup"><span data-stu-id="7ffca-413">The CORS specification calls these headers *simple response headers*.</span></span> <span data-ttu-id="7ffca-414">若要讓應用程式使用其他標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-414">To make other headers available to the app, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithExposedHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=73-78&highlight=5)]

### <a name="credentials-in-cross-origin-requests"></a><span data-ttu-id="7ffca-415">跨原始來源要求中的認證</span><span class="sxs-lookup"><span data-stu-id="7ffca-415">Credentials in cross-origin requests</span></span>

<span data-ttu-id="7ffca-416">認證需要在 CORS 要求中進行特殊處理。</span><span class="sxs-lookup"><span data-stu-id="7ffca-416">Credentials require special handling in a CORS request.</span></span> <span data-ttu-id="7ffca-417">根據預設，瀏覽器不會傳送具有跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="7ffca-417">By default, the browser doesn't send credentials with a cross-origin request.</span></span> <span data-ttu-id="7ffca-418">認證包括 cookie 和 HTTP 驗證配置。</span><span class="sxs-lookup"><span data-stu-id="7ffca-418">Credentials include cookies and HTTP authentication schemes.</span></span> <span data-ttu-id="7ffca-419">若要使用跨原始來源要求傳送認證，用戶端必須將 `XMLHttpRequest.withCredentials` 設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-419">To send credentials with a cross-origin request, the client must set `XMLHttpRequest.withCredentials` to `true`.</span></span>

<span data-ttu-id="7ffca-420">直接使用 `XMLHttpRequest`：</span><span class="sxs-lookup"><span data-stu-id="7ffca-420">Using `XMLHttpRequest` directly:</span></span>

```javascript
var xhr = new XMLHttpRequest();
xhr.open('get', 'https://www.example.com/api/test');
xhr.withCredentials = true;
```

<span data-ttu-id="7ffca-421">使用 jQuery：</span><span class="sxs-lookup"><span data-stu-id="7ffca-421">Using jQuery:</span></span>

```javascript
$.ajax({
  type: 'get',
  url: 'https://www.example.com/api/test',
  xhrFields: {
    withCredentials: true
  }
});
```

<span data-ttu-id="7ffca-422">使用[FETCH API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)：</span><span class="sxs-lookup"><span data-stu-id="7ffca-422">Using the [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):</span></span>

```javascript
fetch('https://www.example.com/api/test', {
    credentials: 'include'
});
```

<span data-ttu-id="7ffca-423">伺服器必須允許認證。</span><span class="sxs-lookup"><span data-stu-id="7ffca-423">The server must allow the credentials.</span></span> <span data-ttu-id="7ffca-424">若要允許跨原始來源認證，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-424">To allow cross-origin credentials, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowCredentials*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=82-87&highlight=5)]

<span data-ttu-id="7ffca-425">HTTP 回應包含 `Access-Control-Allow-Credentials` 標頭，它會告訴瀏覽器伺服器允許跨原始來源要求的認證。</span><span class="sxs-lookup"><span data-stu-id="7ffca-425">The HTTP response includes an `Access-Control-Allow-Credentials` header, which tells the browser that the server allows credentials for a cross-origin request.</span></span>

<span data-ttu-id="7ffca-426">如果瀏覽器傳送認證，但回應未包含有效的 `Access-Control-Allow-Credentials` 標頭，則瀏覽器不會向應用程式公開回應，且跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="7ffca-426">If the browser sends credentials but the response doesn't include a valid `Access-Control-Allow-Credentials` header, the browser doesn't expose the response to the app, and the cross-origin request fails.</span></span>

<span data-ttu-id="7ffca-427">允許跨原始來源認證會有安全性風險。</span><span class="sxs-lookup"><span data-stu-id="7ffca-427">Allowing cross-origin credentials is a security risk.</span></span> <span data-ttu-id="7ffca-428">另一個網域的網站可以代表使用者將登入使用者的認證傳送給應用程式，而不需要使用者的知識。</span><span class="sxs-lookup"><span data-stu-id="7ffca-428">A website at another domain can send a signed-in user's credentials to the app on the user's behalf without the user's knowledge.</span></span> <!-- TODO Review: When using `AllowCredentials`, all CORS enabled domains must be trusted.
I don't like "all CORS enabled domains must be trusted", because it implies that if you're not using  `AllowCredentials`, domains don't need to be trusted. -->

<span data-ttu-id="7ffca-429">CORS 規格也指出，如果 `Access-Control-Allow-Credentials` 標頭存在，設定 `"*"` （所有來源）的來源會無效。</span><span class="sxs-lookup"><span data-stu-id="7ffca-429">The CORS specification also states that setting origins to `"*"` (all origins) is invalid if the `Access-Control-Allow-Credentials` header is present.</span></span>

### <a name="preflight-requests"></a><span data-ttu-id="7ffca-430">預檢要求</span><span class="sxs-lookup"><span data-stu-id="7ffca-430">Preflight requests</span></span>

<span data-ttu-id="7ffca-431">針對某些 CORS 要求，瀏覽器會在提出實際要求之前傳送額外的要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-431">For some CORS requests, the browser sends an additional request before making the actual request.</span></span> <span data-ttu-id="7ffca-432">此要求稱為*預檢要求*。</span><span class="sxs-lookup"><span data-stu-id="7ffca-432">This request is called a *preflight request*.</span></span> <span data-ttu-id="7ffca-433">當下列條件成立時，瀏覽器可以略過預檢要求：</span><span class="sxs-lookup"><span data-stu-id="7ffca-433">The browser can skip the preflight request if the following conditions are true:</span></span>

* <span data-ttu-id="7ffca-434">要求方法為 GET、HEAD 或 POST。</span><span class="sxs-lookup"><span data-stu-id="7ffca-434">The request method is GET, HEAD, or POST.</span></span>
* <span data-ttu-id="7ffca-435">應用程式不會設定 `Accept`、`Accept-Language`、`Content-Language`、`Content-Type`或 `Last-Event-ID`以外的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-435">The app doesn't set request headers other than `Accept`, `Accept-Language`, `Content-Language`, `Content-Type`, or `Last-Event-ID`.</span></span>
* <span data-ttu-id="7ffca-436">如果設定了 `Content-Type` 標頭，就會有下列其中一個值：</span><span class="sxs-lookup"><span data-stu-id="7ffca-436">The `Content-Type` header, if set, has one of the following values:</span></span>
  * `application/x-www-form-urlencoded`
  * `multipart/form-data`
  * `text/plain`

<span data-ttu-id="7ffca-437">針對用戶端要求設定的要求標頭規則會套用至應用程式所設定的標頭，方法是呼叫 `XMLHttpRequest` 物件上的 `setRequestHeader`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-437">The rule on request headers set for the client request applies to headers that the app sets by calling `setRequestHeader` on the `XMLHttpRequest` object.</span></span> <span data-ttu-id="7ffca-438">CORS 規格會呼叫這些標頭的*作者要求標頭*。</span><span class="sxs-lookup"><span data-stu-id="7ffca-438">The CORS specification calls these headers *author request headers*.</span></span> <span data-ttu-id="7ffca-439">此規則不適用於瀏覽器可以設定的標頭，例如 `User-Agent`、`Host`或 `Content-Length`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-439">The rule doesn't apply to headers the browser can set, such as `User-Agent`, `Host`, or `Content-Length`.</span></span>

<span data-ttu-id="7ffca-440">以下是預檢要求的範例：</span><span class="sxs-lookup"><span data-stu-id="7ffca-440">The following is an example of a preflight request:</span></span>

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

<span data-ttu-id="7ffca-441">預先飛行要求會使用 HTTP OPTIONS 方法。</span><span class="sxs-lookup"><span data-stu-id="7ffca-441">The pre-flight request uses the HTTP OPTIONS method.</span></span> <span data-ttu-id="7ffca-442">其中包含兩個特殊標頭：</span><span class="sxs-lookup"><span data-stu-id="7ffca-442">It includes two special headers:</span></span>

* <span data-ttu-id="7ffca-443">`Access-Control-Request-Method`：將用於實際要求的 HTTP 方法。</span><span class="sxs-lookup"><span data-stu-id="7ffca-443">`Access-Control-Request-Method`: The HTTP method that will be used for the actual request.</span></span>
* <span data-ttu-id="7ffca-444">`Access-Control-Request-Headers`：應用程式在實際要求上設定的要求標頭清單。</span><span class="sxs-lookup"><span data-stu-id="7ffca-444">`Access-Control-Request-Headers`: A list of request headers that the app sets on the actual request.</span></span> <span data-ttu-id="7ffca-445">如先前所述，這不會包含瀏覽器所設定的標頭，例如 `User-Agent`。</span><span class="sxs-lookup"><span data-stu-id="7ffca-445">As stated earlier, this doesn't include headers that the browser sets, such as `User-Agent`.</span></span>

<span data-ttu-id="7ffca-446">CORS 預檢要求可能會包含 `Access-Control-Request-Headers` 標頭，這會向伺服器指出與實際要求一起傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-446">A CORS preflight request might include an `Access-Control-Request-Headers` header, which indicates to the server the headers that are sent with the actual request.</span></span>

<span data-ttu-id="7ffca-447">若要允許特定標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-447">To allow specific headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.WithHeaders*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=55-60&highlight=5)]

<span data-ttu-id="7ffca-448">若要允許所有作者要求標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-448">To allow all author request headers, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.AllowAnyHeader*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=64-69&highlight=5)]

<span data-ttu-id="7ffca-449">瀏覽器在 `Access-Control-Request-Headers`中的設定方式並不完全一致。</span><span class="sxs-lookup"><span data-stu-id="7ffca-449">Browsers aren't entirely consistent in how they set `Access-Control-Request-Headers`.</span></span> <span data-ttu-id="7ffca-450">如果您將標頭設定為不是 `"*"` （或使用 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>）以外的任何專案，您應該至少包含 `Accept`、`Content-Type`和 `Origin`，加上您想要支援的任何自訂標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-450">If you set headers to anything other than `"*"` (or use <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicy.AllowAnyHeader*>), you should include at least `Accept`, `Content-Type`, and `Origin`, plus any custom headers that you want to support.</span></span>

<span data-ttu-id="7ffca-451">以下是預檢要求的回應範例（假設伺服器允許此要求）：</span><span class="sxs-lookup"><span data-stu-id="7ffca-451">The following is an example response to the preflight request (assuming that the server allows the request):</span></span>

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

<span data-ttu-id="7ffca-452">回應包含 `Access-Control-Allow-Methods` 標頭，其中會列出允許的方法，並可選擇是否使用 `Access-Control-Allow-Headers` 標頭，其中會列出允許的標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-452">The response includes an `Access-Control-Allow-Methods` header that lists the allowed methods and optionally an `Access-Control-Allow-Headers` header, which lists the allowed headers.</span></span> <span data-ttu-id="7ffca-453">如果預檢要求成功，瀏覽器就會傳送實際的要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-453">If the preflight request succeeds, the browser sends the actual request.</span></span>

<span data-ttu-id="7ffca-454">如果預檢要求遭到拒絕，應用程式會傳回*200 OK*回應，但不會傳送 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-454">If the preflight request is denied, the app returns a *200 OK* response but doesn't send the CORS headers back.</span></span> <span data-ttu-id="7ffca-455">因此，瀏覽器不會嘗試跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-455">Therefore, the browser doesn't attempt the cross-origin request.</span></span>

### <a name="set-the-preflight-expiration-time"></a><span data-ttu-id="7ffca-456">設定預檢到期時間</span><span class="sxs-lookup"><span data-stu-id="7ffca-456">Set the preflight expiration time</span></span>

<span data-ttu-id="7ffca-457">`Access-Control-Max-Age` 標頭會指定可以快取對預檢要求回應的時間長度。</span><span class="sxs-lookup"><span data-stu-id="7ffca-457">The `Access-Control-Max-Age` header specifies how long the response to the preflight request can be cached.</span></span> <span data-ttu-id="7ffca-458">若要設定此標頭，請呼叫 <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>：</span><span class="sxs-lookup"><span data-stu-id="7ffca-458">To set this header, call <xref:Microsoft.AspNetCore.Cors.Infrastructure.CorsPolicyBuilder.SetPreflightMaxAge*>:</span></span>

[!code-csharp[](cors/sample/CorsExample4/Startup.cs?range=91-96&highlight=5)]

<a name="how-cors"></a>

## <a name="how-cors-works"></a><span data-ttu-id="7ffca-459">CORS 的運作方式</span><span class="sxs-lookup"><span data-stu-id="7ffca-459">How CORS works</span></span>

<span data-ttu-id="7ffca-460">本節說明在 HTTP 訊息層級的[CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)要求中會發生什麼事。</span><span class="sxs-lookup"><span data-stu-id="7ffca-460">This section describes what happens in a [CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS) request at the level of the HTTP messages.</span></span>

* <span data-ttu-id="7ffca-461">CORS**不**是安全性功能。</span><span class="sxs-lookup"><span data-stu-id="7ffca-461">CORS is **not** a security feature.</span></span> <span data-ttu-id="7ffca-462">CORS 是一種 W3C 標準，可讓伺服器放寬相同的原始原則。</span><span class="sxs-lookup"><span data-stu-id="7ffca-462">CORS is a W3C standard that allows a server to relax the same-origin policy.</span></span>
  * <span data-ttu-id="7ffca-463">例如，惡意執行者可能會對您的網站使用[防止跨網站腳本（XSS）](xref:security/cross-site-scripting) ，並對其已啟用 CORS 的網站執行跨網站要求，以竊取資訊。</span><span class="sxs-lookup"><span data-stu-id="7ffca-463">For example, a malicious actor could use [Prevent Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) against your site and execute a cross-site request to their CORS enabled site to steal information.</span></span>
* <span data-ttu-id="7ffca-464">藉由允許 CORS，您的 API 不會更安全。</span><span class="sxs-lookup"><span data-stu-id="7ffca-464">Your API is not safer by allowing CORS.</span></span>
  * <span data-ttu-id="7ffca-465">而是由用戶端（瀏覽器）強制執行 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-465">It's up to the client (browser) to enforce CORS.</span></span> <span data-ttu-id="7ffca-466">伺服器會執行要求並傳迴響應，這是傳回錯誤並封鎖回應的用戶端。</span><span class="sxs-lookup"><span data-stu-id="7ffca-466">The server executes the request and returns the response, it's the client that returns an error and blocks the response.</span></span> <span data-ttu-id="7ffca-467">例如，下列任何一項工具都會顯示伺服器回應：</span><span class="sxs-lookup"><span data-stu-id="7ffca-467">For example, any of the following tools will display the server response:</span></span>
    * [<span data-ttu-id="7ffca-468">Fiddler</span><span class="sxs-lookup"><span data-stu-id="7ffca-468">Fiddler</span></span>](https://www.telerik.com/fiddler)
    * [<span data-ttu-id="7ffca-469">Postman</span><span class="sxs-lookup"><span data-stu-id="7ffca-469">Postman</span></span>](https://www.getpostman.com/)
    * [<span data-ttu-id="7ffca-470">.NET HttpClient</span><span class="sxs-lookup"><span data-stu-id="7ffca-470">.NET HttpClient</span></span>](/dotnet/csharp/tutorials/console-webapiclient)
    * <span data-ttu-id="7ffca-471">網頁瀏覽器，方法是在網址列中輸入 URL。</span><span class="sxs-lookup"><span data-stu-id="7ffca-471">A web browser by entering the URL in the address bar.</span></span>
* <span data-ttu-id="7ffca-472">這是讓伺服器允許瀏覽器執行跨原始[XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)或[提取 API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API)要求的方式，否則會禁止。</span><span class="sxs-lookup"><span data-stu-id="7ffca-472">It's a way for a server to allow browsers to execute a cross-origin [XHR](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest) or [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) request that otherwise would be forbidden.</span></span>
  * <span data-ttu-id="7ffca-473">瀏覽器（不含 CORS）無法執行跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-473">Browsers (without CORS) can't do cross-origin requests.</span></span> <span data-ttu-id="7ffca-474">在 CORS 之前， [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp)是用來規避這種限制。</span><span class="sxs-lookup"><span data-stu-id="7ffca-474">Before CORS, [JSONP](https://www.w3schools.com/js/js_json_jsonp.asp) was used to circumvent this restriction.</span></span> <span data-ttu-id="7ffca-475">JSONP 不會使用 XHR，它會使用 `<script>` 標記來接收回應。</span><span class="sxs-lookup"><span data-stu-id="7ffca-475">JSONP doesn't use XHR, it uses the `<script>` tag to receive the response.</span></span> <span data-ttu-id="7ffca-476">允許跨原始來源載入腳本。</span><span class="sxs-lookup"><span data-stu-id="7ffca-476">Scripts are allowed to be loaded cross-origin.</span></span>

<span data-ttu-id="7ffca-477">[CORS 規格](https://www.w3.org/TR/cors/)引進數個新的 HTTP 標頭，可啟用跨原始來源要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-477">The [CORS specification](https://www.w3.org/TR/cors/) introduced several new HTTP headers that enable cross-origin requests.</span></span> <span data-ttu-id="7ffca-478">如果瀏覽器支援 CORS，它會針對跨原始來源要求自動設定這些標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-478">If a browser supports CORS, it sets these headers automatically for cross-origin requests.</span></span> <span data-ttu-id="7ffca-479">不需要自訂 JavaScript 程式碼來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-479">Custom JavaScript code isn't required to enable CORS.</span></span>

<span data-ttu-id="7ffca-480">以下是跨原始來源要求的範例。</span><span class="sxs-lookup"><span data-stu-id="7ffca-480">The following is an example of a cross-origin request.</span></span> <span data-ttu-id="7ffca-481">`Origin` 標頭會提供提出要求之網站的網域。</span><span class="sxs-lookup"><span data-stu-id="7ffca-481">The `Origin` header provides the domain of the site that's making the request.</span></span> <span data-ttu-id="7ffca-482">`Origin` 標頭是必要的，而且必須與主機不同。</span><span class="sxs-lookup"><span data-stu-id="7ffca-482">The `Origin` header is required and must be different from the host.</span></span>

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

<span data-ttu-id="7ffca-483">如果伺服器允許此要求，它會在回應中設定 `Access-Control-Allow-Origin` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-483">If the server allows the request, it sets the `Access-Control-Allow-Origin` header in the response.</span></span> <span data-ttu-id="7ffca-484">此標頭的值會比對要求中的 `Origin` 標頭，或為 `"*"`的萬用字元值，這表示允許任何來源：</span><span class="sxs-lookup"><span data-stu-id="7ffca-484">The value of this header either matches the `Origin` header from the request or is the wildcard value `"*"`, meaning that any origin is allowed:</span></span>

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

<span data-ttu-id="7ffca-485">如果回應未包含 `Access-Control-Allow-Origin` 標頭，則跨原始來源要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="7ffca-485">If the response doesn't include the `Access-Control-Allow-Origin` header, the cross-origin request fails.</span></span> <span data-ttu-id="7ffca-486">具體而言，瀏覽器不允許此要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-486">Specifically, the browser disallows the request.</span></span> <span data-ttu-id="7ffca-487">即使伺服器傳回成功的回應，瀏覽器也不會將回應提供給用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-487">Even if the server returns a successful response, the browser doesn't make the response available to the client app.</span></span>

<a name="test"></a>

## <a name="test-cors"></a><span data-ttu-id="7ffca-488">測試 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-488">Test CORS</span></span>

<span data-ttu-id="7ffca-489">測試 CORS：</span><span class="sxs-lookup"><span data-stu-id="7ffca-489">To test CORS:</span></span>

1. <span data-ttu-id="7ffca-490">[建立 API 專案](xref:tutorials/first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-490">[Create an API project](xref:tutorials/first-web-api).</span></span> <span data-ttu-id="7ffca-491">或者，您可以[下載範例](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-491">Alternatively, you can [download the sample](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/cors/sample/Cors).</span></span>
1. <span data-ttu-id="7ffca-492">使用本檔中的其中一種方法來啟用 CORS。</span><span class="sxs-lookup"><span data-stu-id="7ffca-492">Enable CORS using one of the approaches in this document.</span></span> <span data-ttu-id="7ffca-493">例如：</span><span class="sxs-lookup"><span data-stu-id="7ffca-493">For example:</span></span>

  [!code-csharp[](cors/sample/Cors/WebAPI/StartupTest.cs?name=snippet2&highlight=13-18)]

  > [!WARNING]
  > <span data-ttu-id="7ffca-494">`WithOrigins("https://localhost:<port>");` 應該僅用於測試範例應用程式，類似于[下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-494">`WithOrigins("https://localhost:<port>");` should only be used for testing a sample app similar to the [download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/security/cors/sample/Cors).</span></span>

1. <span data-ttu-id="7ffca-495">建立 web 應用程式專案（Razor Pages 或 MVC）。</span><span class="sxs-lookup"><span data-stu-id="7ffca-495">Create a web app project (Razor Pages or MVC).</span></span> <span data-ttu-id="7ffca-496">此範例會使用 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="7ffca-496">The sample uses Razor Pages.</span></span> <span data-ttu-id="7ffca-497">您可以在與 API 專案相同的方案中建立 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-497">You can create the web app in the same solution as the API project.</span></span>
1. <span data-ttu-id="7ffca-498">將下列反白顯示的程式碼新增至*Index. cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="7ffca-498">Add the following highlighted code to the *Index.cshtml* file:</span></span>

  [!code-csharp[](cors/sample/Cors/ClientApp/Pages/Index2.cshtml?highlight=7-99)]

1. <span data-ttu-id="7ffca-499">在上述程式碼中，將 `url: 'https://<web app>.azurewebsites.net/api/values/1',` 取代為已部署應用程式的 URL。</span><span class="sxs-lookup"><span data-stu-id="7ffca-499">In the preceding code, replace `url: 'https://<web app>.azurewebsites.net/api/values/1',` with the URL to the deployed app.</span></span>
1. <span data-ttu-id="7ffca-500">部署 API 專案。</span><span class="sxs-lookup"><span data-stu-id="7ffca-500">Deploy the API project.</span></span> <span data-ttu-id="7ffca-501">例如，[部署至 Azure](xref:host-and-deploy/azure-apps/index)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-501">For example, [deploy to Azure](xref:host-and-deploy/azure-apps/index).</span></span>
1. <span data-ttu-id="7ffca-502">從桌面執行 Razor Pages 或 MVC 應用程式，然後按一下 [**測試**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="7ffca-502">Run the Razor Pages or MVC app from the desktop and click on the **Test** button.</span></span> <span data-ttu-id="7ffca-503">使用 F12 工具來檢查錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7ffca-503">Use the F12 tools to review error messages.</span></span>
1. <span data-ttu-id="7ffca-504">從 `WithOrigins` 移除 localhost 來源，並部署應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-504">Remove the localhost origin from `WithOrigins` and deploy the app.</span></span> <span data-ttu-id="7ffca-505">或者，使用不同的埠來執行用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="7ffca-505">Alternatively, run the client app with a different port.</span></span> <span data-ttu-id="7ffca-506">例如，從 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="7ffca-506">For example, run from Visual Studio.</span></span>
1. <span data-ttu-id="7ffca-507">使用用戶端應用程式進行測試。</span><span class="sxs-lookup"><span data-stu-id="7ffca-507">Test with the client app.</span></span> <span data-ttu-id="7ffca-508">CORS 失敗會傳回錯誤，但 JavaScript 無法使用錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="7ffca-508">CORS failures return an error, but the error message isn't available to JavaScript.</span></span> <span data-ttu-id="7ffca-509">使用 F12 工具中的 [主控台] 索引標籤來查看錯誤。</span><span class="sxs-lookup"><span data-stu-id="7ffca-509">Use the console tab in the F12 tools to see the error.</span></span> <span data-ttu-id="7ffca-510">視瀏覽器而定，您會收到類似下列的錯誤（在 F12 工具主控台中）：</span><span class="sxs-lookup"><span data-stu-id="7ffca-510">Depending on the browser, you get an error (in the F12 tools console) similar to the following:</span></span>

   * <span data-ttu-id="7ffca-511">使用 Microsoft Edge：</span><span class="sxs-lookup"><span data-stu-id="7ffca-511">Using Microsoft Edge:</span></span>

     <span data-ttu-id="7ffca-512">**SEC7120： [CORS] 來源 `https://localhost:44375` 在跨原始來源資源的存取控制-允許來源回應標頭中找不到 `https://localhost:44375` `https://webapi.azurewebsites.net/api/values/1`**</span><span class="sxs-lookup"><span data-stu-id="7ffca-512">**SEC7120: [CORS] The origin `https://localhost:44375` did not find `https://localhost:44375` in the Access-Control-Allow-Origin response header for cross-origin  resource at `https://webapi.azurewebsites.net/api/values/1`**</span></span>

   * <span data-ttu-id="7ffca-513">使用 Chrome：</span><span class="sxs-lookup"><span data-stu-id="7ffca-513">Using Chrome:</span></span>

     <span data-ttu-id="7ffca-514">**來源 `https://localhost:44375` 的 `https://webapi.azurewebsites.net/api/values/1` 存取 XMLHttpRequest 已被 CORS 原則封鎖：要求的資源上沒有任何「存取控制-允許來源」標頭。**</span><span class="sxs-lookup"><span data-stu-id="7ffca-514">**Access to XMLHttpRequest at `https://webapi.azurewebsites.net/api/values/1` from origin `https://localhost:44375` has been blocked by CORS policy: No 'Access-Control-Allow-Origin' header is present on the requested resource.**</span></span>
     
<span data-ttu-id="7ffca-515">具有 CORS 功能的端點可以使用工具（例如[Fiddler](https://www.telerik.com/fiddler)或[Postman](https://www.getpostman.com/)）進行測試。</span><span class="sxs-lookup"><span data-stu-id="7ffca-515">CORS-enabled endpoints can be tested with a tool, such as [Fiddler](https://www.telerik.com/fiddler) or [Postman](https://www.getpostman.com/).</span></span> <span data-ttu-id="7ffca-516">使用工具時，`Origin` 標頭所指定之要求的來源，必須與接收要求的主機不同。</span><span class="sxs-lookup"><span data-stu-id="7ffca-516">When using a tool, the origin of the request specified by the `Origin` header must differ from the host receiving the request.</span></span> <span data-ttu-id="7ffca-517">如果要求不是根據 `Origin` 標頭的值而*跨原始來源*：</span><span class="sxs-lookup"><span data-stu-id="7ffca-517">If the request isn't *cross-origin* based on the value of the `Origin` header:</span></span>

* <span data-ttu-id="7ffca-518">CORS 中介軟體不需要處理要求。</span><span class="sxs-lookup"><span data-stu-id="7ffca-518">There's no need for CORS Middleware to process the request.</span></span>
* <span data-ttu-id="7ffca-519">回應中不會傳回 CORS 標頭。</span><span class="sxs-lookup"><span data-stu-id="7ffca-519">CORS headers aren't returned in the response.</span></span>

## <a name="cors-in-iis"></a><span data-ttu-id="7ffca-520">IIS 中的 CORS</span><span class="sxs-lookup"><span data-stu-id="7ffca-520">CORS in IIS</span></span>

<span data-ttu-id="7ffca-521">部署到 IIS 時，如果伺服器未設定為允許匿名存取，CORS 就必須在 Windows 驗證之前執行。</span><span class="sxs-lookup"><span data-stu-id="7ffca-521">When deploying to IIS, CORS has to run before Windows Authentication if the server isn't configured to allow anonymous access.</span></span> <span data-ttu-id="7ffca-522">若要支援此案例，必須安裝並設定應用程式的[IIS CORS 模組](https://www.iis.net/downloads/microsoft/iis-cors-module)。</span><span class="sxs-lookup"><span data-stu-id="7ffca-522">To support this scenario, the [IIS CORS module](https://www.iis.net/downloads/microsoft/iis-cors-module) needs to be installed and configured for the app.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7ffca-523">其他資源</span><span class="sxs-lookup"><span data-stu-id="7ffca-523">Additional resources</span></span>

* [<span data-ttu-id="7ffca-524">跨原始來源資源分享（CORS）</span><span class="sxs-lookup"><span data-stu-id="7ffca-524">Cross-Origin Resource Sharing (CORS)</span></span>](https://developer.mozilla.org/docs/Web/HTTP/CORS)
* [<span data-ttu-id="7ffca-525">IIS CORS 模組使用者入門</span><span class="sxs-lookup"><span data-stu-id="7ffca-525">Getting started with the IIS CORS module</span></span>](https://blogs.iis.net/iisteam/getting-started-with-the-iis-cors-module)

::: moniker-end