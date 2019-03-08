---
title: ASP.NET Core 中的回應快取中介軟體
author: guardrex
description: 了解如何設定和使用 ASP.NET Core 中的回應快取中介軟體。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/20/2019
uid: performance/caching/middleware
ms.openlocfilehash: c7c3dbd0c9cf029fa6921d77450e780768c8aa6e
ms.sourcegitcommit: 0945078a09c372f17e9b003758ed87e99c2449f4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/21/2019
ms.locfileid: "56647911"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="87ab5-103">ASP.NET Core 中的回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="87ab5-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="87ab5-104">藉由[Luke Latham](https://github.com/guardrex)和[John Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="87ab5-104">By [Luke Latham](https://github.com/guardrex) and [John Luo](https://github.com/JunTaoLuo)</span></span>

<span data-ttu-id="87ab5-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="87ab5-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="87ab5-106">這篇文章說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87ab5-106">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="87ab5-107">中介軟體會判斷何時可快取回應、存放回應，以及從快取提供回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-107">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="87ab5-108">如需 HTTP 快取和 `ResponseCache` 屬性的簡介，請參閱[回應快取](xref:performance/caching/response)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-108">For an introduction to HTTP caching and the `ResponseCache` attribute, see [Response Caching](xref:performance/caching/response).</span></span>

## <a name="package"></a><span data-ttu-id="87ab5-109">套件</span><span class="sxs-lookup"><span data-stu-id="87ab5-109">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="87ab5-110">參考 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)或加入對 [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="87ab5-110">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="87ab5-111">參考[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)或增加對 [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="87ab5-111">Reference the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="87ab5-112">增加對 [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) 套件的套件參考。</span><span class="sxs-lookup"><span data-stu-id="87ab5-112">Add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="87ab5-113">組態</span><span class="sxs-lookup"><span data-stu-id="87ab5-113">Configuration</span></span>

<span data-ttu-id="87ab5-114">在  `Startup.ConfigureServices`，將中介軟體新增至服務集合。</span><span class="sxs-lookup"><span data-stu-id="87ab5-114">In `Startup.ConfigureServices`, add the middleware to the service collection.</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=9)]

<span data-ttu-id="87ab5-115">設定應用程式透過 `UseResponseCaching` 擴充方法使用中介軟體，此擴充方法會將中介軟體新增到要求處理管線。</span><span class="sxs-lookup"><span data-stu-id="87ab5-115">Configure the app to use the middleware with the `UseResponseCaching` extension method, which adds the middleware to the request processing pipeline.</span></span> <span data-ttu-id="87ab5-116">範例應用程式會新增 [`Cache-Control`](https://tools.ietf.org/html/rfc7234#section-5.2) 標頭到回應中，這會快取可快取的回應最多 10 秒。</span><span class="sxs-lookup"><span data-stu-id="87ab5-116">The sample app adds a [`Cache-Control`](https://tools.ietf.org/html/rfc7234#section-5.2) header to the response that caches cacheable responses for up to 10 seconds.</span></span> <span data-ttu-id="87ab5-117">只有當後續邀求的 [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) 標頭符合原始要求的 [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) 時，範例才會傳送 [`Vary`](https://tools.ietf.org/html/rfc7231#section-7.1.4) 標頭，以設定中介軟體提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-117">The sample sends a [`Vary`](https://tools.ietf.org/html/rfc7231#section-7.1.4) header to configure the middleware to serve a cached response only if the [`Accept-Encoding`](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span> <span data-ttu-id="87ab5-118">在接下來的程式碼範例中，[CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) 與 [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) 需要 `using` 陳述式 (針對 [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) 命名空間)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-118">In the code example that follows, [CacheControlHeaderValue](/dotnet/api/microsoft.net.http.headers.cachecontrolheadervalue) and [HeaderNames](/dotnet/api/microsoft.net.http.headers.headernames) require a `using` statement for the [Microsoft.Net.Http.Headers](/dotnet/api/microsoft.net.http.headers) namespace.</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17,22-29)]

<span data-ttu-id="87ab5-119">回應快取中介軟體只會快取產生 200 (沒問題) 狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-119">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="87ab5-120">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-120">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="87ab5-121">包含內容的已驗證的用戶端的回應必須標示為不可快取，以防止從儲存和處理這些回應的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87ab5-121">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="87ab5-122">請參閱[快取的條件](#conditions-for-caching)如需有關如何在中介軟體，判斷回應是否可快取。</span><span class="sxs-lookup"><span data-stu-id="87ab5-122">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="87ab5-123">選項</span><span class="sxs-lookup"><span data-stu-id="87ab5-123">Options</span></span>

<span data-ttu-id="87ab5-124">中介軟體會提供三個選項可控制的回應快取。</span><span class="sxs-lookup"><span data-stu-id="87ab5-124">The middleware offers three options for controlling response caching.</span></span>

| <span data-ttu-id="87ab5-125">選項</span><span class="sxs-lookup"><span data-stu-id="87ab5-125">Option</span></span>                | <span data-ttu-id="87ab5-126">描述</span><span class="sxs-lookup"><span data-stu-id="87ab5-126">Description</span></span> |
| --------------------- | ----------- |
| <span data-ttu-id="87ab5-127">UseCaseSensitivePaths</span><span class="sxs-lookup"><span data-stu-id="87ab5-127">UseCaseSensitivePaths</span></span> | <span data-ttu-id="87ab5-128">決定回應會快取在區分大小寫的路徑上。</span><span class="sxs-lookup"><span data-stu-id="87ab5-128">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="87ab5-129">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="87ab5-129">The default value is `false`.</span></span> |
| <span data-ttu-id="87ab5-130">MaximumBodySize</span><span class="sxs-lookup"><span data-stu-id="87ab5-130">MaximumBodySize</span></span>       | <span data-ttu-id="87ab5-131">回應內文，以位元組為單位的最大的快取大小。</span><span class="sxs-lookup"><span data-stu-id="87ab5-131">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="87ab5-132">預設值是`64 * 1024 * 1024`(64 MB)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-132">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <span data-ttu-id="87ab5-133">SizeLimit</span><span class="sxs-lookup"><span data-stu-id="87ab5-133">SizeLimit</span></span>             | <span data-ttu-id="87ab5-134">回應快取中介軟體，以位元組為單位的大小限制。</span><span class="sxs-lookup"><span data-stu-id="87ab5-134">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="87ab5-135">預設值是`100 * 1024 * 1024`(100 MB)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-135">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |

<span data-ttu-id="87ab5-136">下列範例會設定中的介軟體：</span><span class="sxs-lookup"><span data-stu-id="87ab5-136">The following example configures the middleware to:</span></span>

* <span data-ttu-id="87ab5-137">快取回應小於或等於 1024 個位元組。</span><span class="sxs-lookup"><span data-stu-id="87ab5-137">Cache responses smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="87ab5-138">將回應儲存透過區分大小寫的路徑 (例如`/page1`和`/Page1`分開儲存)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-138">Store the responses by case-sensitive paths (for example, `/page1` and `/Page1` are stored separately).</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.UseCaseSensitivePaths = true;
    options.MaximumBodySize = 1024;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="87ab5-139">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="87ab5-139">VaryByQueryKeys</span></span>

<span data-ttu-id="87ab5-140">使用 MVC/Web API 控制器或 Razor 頁面的頁面模型時`ResponseCache`屬性指定設定適當的標頭的回應快取所需的參數。</span><span class="sxs-lookup"><span data-stu-id="87ab5-140">When using MVC/Web API controllers or Razor Pages page models, the `ResponseCache` attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="87ab5-141">唯一的參數`ResponseCache`嚴格需要的中介軟體的屬性是`VaryByQueryKeys`，不會對應到實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="87ab5-141">The only parameter of the `ResponseCache` attribute that strictly requires the middleware is `VaryByQueryKeys`, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="87ab5-142">如需詳細資訊，請參閱 < [ResponseCache 屬性](xref:performance/caching/response#responsecache-attribute)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-142">For more information, see [ResponseCache Attribute](xref:performance/caching/response#responsecache-attribute).</span></span>

<span data-ttu-id="87ab5-143">不使用時`ResponseCache`屬性，回應快取可因使用`VaryByQueryKeys`功能。</span><span class="sxs-lookup"><span data-stu-id="87ab5-143">When not using the `ResponseCache` attribute, response caching can be varied with the `VaryByQueryKeys` feature.</span></span> <span data-ttu-id="87ab5-144">使用`ResponseCachingFeature`直接從`IFeatureCollection`的`HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="87ab5-144">Use the `ResponseCachingFeature` directly from the `IFeatureCollection` of the `HttpContext`:</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();
if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="87ab5-145">使用單一的值等於`*`在`VaryByQueryKeys`會快取所要求的所有查詢參數而有所不同。</span><span class="sxs-lookup"><span data-stu-id="87ab5-145">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="87ab5-146">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="87ab5-146">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="87ab5-147">中介軟體的回應快取是使用 HTTP 標頭設定的。</span><span class="sxs-lookup"><span data-stu-id="87ab5-147">Response caching by the middleware is configured using HTTP headers.</span></span>

| <span data-ttu-id="87ab5-148">標頭</span><span class="sxs-lookup"><span data-stu-id="87ab5-148">Header</span></span> | <span data-ttu-id="87ab5-149">詳細資料</span><span class="sxs-lookup"><span data-stu-id="87ab5-149">Details</span></span> |
| ------ | ------- |
| <span data-ttu-id="87ab5-150">Authorization</span><span class="sxs-lookup"><span data-stu-id="87ab5-150">Authorization</span></span> | <span data-ttu-id="87ab5-151">如果此標頭存在，回應不會被加入快取中。</span><span class="sxs-lookup"><span data-stu-id="87ab5-151">The response isn't cached if the header exists.</span></span> |
| <span data-ttu-id="87ab5-152">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="87ab5-152">Cache-Control</span></span> | <span data-ttu-id="87ab5-153">中介軟體只會考慮快取標有 `public` 快取指示詞的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-153">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="87ab5-154">使用下列參數控制快取：</span><span class="sxs-lookup"><span data-stu-id="87ab5-154">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="87ab5-155">max-age</span><span class="sxs-lookup"><span data-stu-id="87ab5-155">max-age</span></span></li><li><span data-ttu-id="87ab5-156">max-stale&#8224;</span><span class="sxs-lookup"><span data-stu-id="87ab5-156">max-stale&#8224;</span></span></li><li><span data-ttu-id="87ab5-157">min-fresh</span><span class="sxs-lookup"><span data-stu-id="87ab5-157">min-fresh</span></span></li><li><span data-ttu-id="87ab5-158">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="87ab5-158">must-revalidate</span></span></li><li><span data-ttu-id="87ab5-159">no-cache</span><span class="sxs-lookup"><span data-stu-id="87ab5-159">no-cache</span></span></li><li><span data-ttu-id="87ab5-160">no-store</span><span class="sxs-lookup"><span data-stu-id="87ab5-160">no-store</span></span></li><li><span data-ttu-id="87ab5-161">only-if-cached</span><span class="sxs-lookup"><span data-stu-id="87ab5-161">only-if-cached</span></span></li><li><span data-ttu-id="87ab5-162">private</span><span class="sxs-lookup"><span data-stu-id="87ab5-162">private</span></span></li><li><span data-ttu-id="87ab5-163">public</span><span class="sxs-lookup"><span data-stu-id="87ab5-163">public</span></span></li><li><span data-ttu-id="87ab5-164">s maxage</span><span class="sxs-lookup"><span data-stu-id="87ab5-164">s-maxage</span></span></li><li><span data-ttu-id="87ab5-165">proxy-revalidate&#8225;</span><span class="sxs-lookup"><span data-stu-id="87ab5-165">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="87ab5-166">&#8224;如果沒有指定限制到 `max-stale`，中介軟體不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="87ab5-166">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="87ab5-167">&#8225;`proxy-revalidate`具有與 `must-revalidate` 相同的效果。</span><span class="sxs-lookup"><span data-stu-id="87ab5-167">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="87ab5-168">如需詳細資訊，請參閱[RFC 7231：要求快取控制指示詞](https://tools.ietf.org/html/rfc7234#section-5.2.1)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-168">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| <span data-ttu-id="87ab5-169">Pragma</span><span class="sxs-lookup"><span data-stu-id="87ab5-169">Pragma</span></span> | <span data-ttu-id="87ab5-170">要求中的 `Pragma: no-cache` 標頭會產生與 `Cache-Control: no-cache` 相同的效果。</span><span class="sxs-lookup"><span data-stu-id="87ab5-170">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="87ab5-171">此標頭會由 `Cache-Control` 中的相關指示詞覆寫 (如果有的話)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-171">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="87ab5-172">已考慮與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="87ab5-172">Considered for backward compatibility with HTTP/1.0.</span></span> |
| <span data-ttu-id="87ab5-173">Set-Cookie</span><span class="sxs-lookup"><span data-stu-id="87ab5-173">Set-Cookie</span></span> | <span data-ttu-id="87ab5-174">如果此標頭存在，回應不會被加入快取中。</span><span class="sxs-lookup"><span data-stu-id="87ab5-174">The response isn't cached if the header exists.</span></span> <span data-ttu-id="87ab5-175">設定一或多個 cookie 的要求處理管線中的任何中介軟體會防止從快取的回應的回應快取中介軟體 (例如[cookie 架構 TempData 提供者](xref:fundamentals/app-state#tempdata))。</span><span class="sxs-lookup"><span data-stu-id="87ab5-175">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| <span data-ttu-id="87ab5-176">Vary</span><span class="sxs-lookup"><span data-stu-id="87ab5-176">Vary</span></span> | <span data-ttu-id="87ab5-177">`Vary`標頭會由另一個標頭用來改變快取回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-177">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="87ab5-178">例如，透過包含 `Vary: Accept-Encoding` 標頭，以編碼方式快取回應，這會分別為具有 `Accept-Encoding: gzip` 與 `Accept-Encoding: text/plain` 的標頭的要求快取回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-178">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="87ab5-179">一律不會儲存具有開頭值 `*` 的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-179">A response with a header value of `*` is never stored.</span></span> |
| <span data-ttu-id="87ab5-180">Expires</span><span class="sxs-lookup"><span data-stu-id="87ab5-180">Expires</span></span> | <span data-ttu-id="87ab5-181">除非由其他 `Cache-Control` 標頭覆寫，否則不會存儲或檢索被此標頭視為過時的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-181">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| <span data-ttu-id="87ab5-182">If-None-Match</span><span class="sxs-lookup"><span data-stu-id="87ab5-182">If-None-Match</span></span> | <span data-ttu-id="87ab5-183">如果值不是 `*` 且回應的 `ETag` 與提供的任何值都不相符，則從快取提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-183">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="87ab5-184">否則，會提供 304 (未修改) 的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-184">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| <span data-ttu-id="87ab5-185">If-Modified-Since</span><span class="sxs-lookup"><span data-stu-id="87ab5-185">If-Modified-Since</span></span> | <span data-ttu-id="87ab5-186">如果`If-None-Match`標頭不存在，如果快取的回應日期比所提供的值從快取提供完整的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-186">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="87ab5-187">否則，會提供 304 (未修改) 的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-187">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| <span data-ttu-id="87ab5-188">Date</span><span class="sxs-lookup"><span data-stu-id="87ab5-188">Date</span></span> | <span data-ttu-id="87ab5-189">提供快取時，如果原始回應中未提供 `Date` 標頭，則由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="87ab5-189">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| <span data-ttu-id="87ab5-190">Content-Length</span><span class="sxs-lookup"><span data-stu-id="87ab5-190">Content-Length</span></span> | <span data-ttu-id="87ab5-191">從快取提供時，如果原始回應中未提供 `Content-Length` 標頭，則由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="87ab5-191">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| <span data-ttu-id="87ab5-192">Age</span><span class="sxs-lookup"><span data-stu-id="87ab5-192">Age</span></span> | <span data-ttu-id="87ab5-193">在 `Age`原始回應中傳送的標頭會被忽略。</span><span class="sxs-lookup"><span data-stu-id="87ab5-193">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="87ab5-194">中介軟體提供快取的回應時會計算新的值。</span><span class="sxs-lookup"><span data-stu-id="87ab5-194">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="87ab5-195">快取會遵守要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="87ab5-195">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="87ab5-196">中介軟體會遵守的規則[快取的 HTTP 1.1 規格](https://tools.ietf.org/html/rfc7234#section-5.2)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-196">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="87ab5-197">規則需要接受有效的快取`Cache-Control`用戶端傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="87ab5-197">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="87ab5-198">在規格中，在用戶端可要求`no-cache`標頭值並強制產生新的回應，每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="87ab5-198">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="87ab5-199">目前沒有任何開發人員會控制此快取的行為時使用的中介軟體，因為中介軟體會遵守官方的快取規格。</span><span class="sxs-lookup"><span data-stu-id="87ab5-199">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="87ab5-200">進一步控制快取行為的詳細資訊，探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="87ab5-200">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="87ab5-201">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="87ab5-201">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="87ab5-202">疑難排解</span><span class="sxs-lookup"><span data-stu-id="87ab5-202">Troubleshooting</span></span>

<span data-ttu-id="87ab5-203">如果快取行為，將無法如預期般運作，請確認回應的快取，而且能夠從快取所提供。</span><span class="sxs-lookup"><span data-stu-id="87ab5-203">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="87ab5-204">檢查要求的連入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="87ab5-204">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="87ab5-205">啟用[記錄](xref:fundamentals/logging/index)來協助偵錯。</span><span class="sxs-lookup"><span data-stu-id="87ab5-205">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="87ab5-206">當進行測試和疑難排解快取行為，在瀏覽器可能設定影響不想要的方式快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="87ab5-206">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="87ab5-207">例如，可能會設定瀏覽器`Cache-Control`標頭`no-cache`或`max-age=0`時重新整理頁面。</span><span class="sxs-lookup"><span data-stu-id="87ab5-207">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="87ab5-208">下列工具可以明確設定要求標頭，並正適合測試快取：</span><span class="sxs-lookup"><span data-stu-id="87ab5-208">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="87ab5-209">Fiddler</span><span class="sxs-lookup"><span data-stu-id="87ab5-209">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="87ab5-210">Postman</span><span class="sxs-lookup"><span data-stu-id="87ab5-210">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="87ab5-211">快取的條件</span><span class="sxs-lookup"><span data-stu-id="87ab5-211">Conditions for caching</span></span>

* <span data-ttu-id="87ab5-212">伺服器回應 200 （確定） 狀態碼必須產生要求。</span><span class="sxs-lookup"><span data-stu-id="87ab5-212">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="87ab5-213">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="87ab5-213">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="87ab5-214">在  `Startup.Configure`，需要壓縮的中介軟體之前，必須放回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="87ab5-214">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require compression.</span></span> <span data-ttu-id="87ab5-215">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="87ab5-215">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="87ab5-216">`Authorization`不得存在於標頭。</span><span class="sxs-lookup"><span data-stu-id="87ab5-216">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="87ab5-217">`Cache-Control` 標頭參數必須有效，且必須標示為回應`public`而未標示為`private`。</span><span class="sxs-lookup"><span data-stu-id="87ab5-217">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="87ab5-218">`Pragma: no-cache`標頭不能有如果`Cache-Control`標頭不存在，作為`Cache-Control`標頭會覆寫`Pragma`標頭時出現。</span><span class="sxs-lookup"><span data-stu-id="87ab5-218">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="87ab5-219">`Set-Cookie`不得存在於標頭。</span><span class="sxs-lookup"><span data-stu-id="87ab5-219">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="87ab5-220">`Vary` 標頭參數必須是有效且不等於`*`。</span><span class="sxs-lookup"><span data-stu-id="87ab5-220">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="87ab5-221">`Content-Length`標頭值 (如果設定) 必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="87ab5-221">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="87ab5-222">[IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature)不會使用。</span><span class="sxs-lookup"><span data-stu-id="87ab5-222">The [IHttpSendFileFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpsendfilefeature) isn't used.</span></span>
* <span data-ttu-id="87ab5-223">回應不是所指定的過時`Expires`標頭和`max-age`和`s-maxage`快取指示詞。</span><span class="sxs-lookup"><span data-stu-id="87ab5-223">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="87ab5-224">回應緩衝必須成功，而且回應的大小必須小於所設定，或預設`SizeLimit`。</span><span class="sxs-lookup"><span data-stu-id="87ab5-224">Response buffering must be successful, and the size of the response must be smaller than the configured or default `SizeLimit`.</span></span>
* <span data-ttu-id="87ab5-225">回應必須是根據快取[RFC 7234](https://tools.ietf.org/html/rfc7234)規格。</span><span class="sxs-lookup"><span data-stu-id="87ab5-225">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="87ab5-226">比方說，`no-store`指示詞不能存在於要求或回應標頭欄位。</span><span class="sxs-lookup"><span data-stu-id="87ab5-226">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="87ab5-227">請參閱*第 3 節：將回應儲存在快取*的[RFC 7234](https://tools.ietf.org/html/rfc7234)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="87ab5-227">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="87ab5-228">防偽系統是否有產生安全的權杖，以防止跨網站要求偽造 (CSRF) 攻擊集`Cache-Control`並`Pragma`標頭`no-cache`，因此不會快取的回應。</span><span class="sxs-lookup"><span data-stu-id="87ab5-228">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="87ab5-229">如需如何停用 HTML 表單元素的 antiforgery 權杖的資訊，請參閱[ASP.NET Core antiforgery 組態](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration)。</span><span class="sxs-lookup"><span data-stu-id="87ab5-229">For information on how to disable antiforgery tokens for HTML form elements, see [ASP.NET Core antiforgery configuration](xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87ab5-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="87ab5-230">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
