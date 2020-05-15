---
title: ASP.NET Core 中的回應快取中介軟體
author: rick-anderson
description: 了解如何設定和使用 ASP.NET Core 中的回應快取中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: performance/caching/middleware
ms.openlocfilehash: 10359f25223d5c8e89f983633d24fbcac8a22deb
ms.sourcegitcommit: 688b6f448d87b6f7f4440182d72388eaa68d2935
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2020
ms.locfileid: "83393824"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="8138b-103">ASP.NET Core 中的回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="8138b-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="8138b-104">作者：[John Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="8138b-104">By [John Luo](https://github.com/JunTaoLuo)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8138b-105">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8138b-105">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="8138b-106">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-106">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="8138b-107">如需 HTTP 快取和屬性的簡介 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) ，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="8138b-107">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="8138b-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8138b-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="8138b-109">設定</span><span class="sxs-lookup"><span data-stu-id="8138b-109">Configuration</span></span>

<span data-ttu-id="8138b-110">回應快取中介軟體可透過共用架構隱含地用於 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8138b-110">Response Caching Middleware is implicitly available for ASP.NET Core apps via the shared framework.</span></span>

<span data-ttu-id="8138b-111">在中 `Startup.ConfigureServices` ，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="8138b-111">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="8138b-112">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，這會將中介軟體新增至中的要求處理管線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="8138b-112">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

<span data-ttu-id="8138b-113">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="8138b-113">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="8138b-114">[Cache-控制項](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash;快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="8138b-115">[差異](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash;只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會設定中介軟體來提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

<span data-ttu-id="8138b-116">前面的標頭不會寫入至回應，並會在控制器、動作或 Razor 頁面時遭到覆寫：</span><span class="sxs-lookup"><span data-stu-id="8138b-116">The preceding headers are not written to the response and are overridden when a controller, action, or Razor Page:</span></span>

* <span data-ttu-id="8138b-117">具有[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="8138b-117">Has a [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute.</span></span> <span data-ttu-id="8138b-118">即使未設定屬性，這也適用。</span><span class="sxs-lookup"><span data-stu-id="8138b-118">This applies even if a property isn't set.</span></span> <span data-ttu-id="8138b-119">例如，省略[VaryByHeader](/aspnet/core/performance/caching/response#vary)屬性將會從回應中移除對應的標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-119">For example, omitting the [VaryByHeader](/aspnet/core/performance/caching/response#vary) property will cause the corresponding header to be removed from the response.</span></span>

<span data-ttu-id="8138b-120">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-120">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="8138b-121">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="8138b-121">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="8138b-122">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-122">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="8138b-123">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="8138b-123">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="8138b-124">選項</span><span class="sxs-lookup"><span data-stu-id="8138b-124">Options</span></span>

<span data-ttu-id="8138b-125">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="8138b-125">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="8138b-126">選項</span><span class="sxs-lookup"><span data-stu-id="8138b-126">Option</span></span> | <span data-ttu-id="8138b-127">說明</span><span class="sxs-lookup"><span data-stu-id="8138b-127">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="8138b-128">回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="8138b-128">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="8138b-129">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="8138b-129">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="8138b-130">回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="8138b-130">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="8138b-131">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="8138b-131">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="8138b-132">決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-132">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="8138b-133">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="8138b-133">The default value is `false`.</span></span> |

<span data-ttu-id="8138b-134">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="8138b-134">The following example configures the middleware to:</span></span>

* <span data-ttu-id="8138b-135">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-135">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="8138b-136">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="8138b-136">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="8138b-137">例如， `/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="8138b-137">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="8138b-138">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="8138b-138">VaryByQueryKeys</span></span>

<span data-ttu-id="8138b-139">使用 MVC/Web API 控制器或 Razor Pages 頁面模型時， [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) 屬性會指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="8138b-139">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="8138b-140">`[ResponseCache]`嚴格需要中介軟體的屬性唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys> ，這並不會對應至實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-140">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="8138b-141">如需詳細資訊，請參閱<xref:performance/caching/response#responsecache-attribute>。</span><span class="sxs-lookup"><span data-stu-id="8138b-141">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="8138b-142">當不使用 `[ResponseCache]` 屬性時，回應快取可以與不同 `VaryByQueryKeys` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-142">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="8138b-143">直接使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> [HttpCoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)：</span><span class="sxs-lookup"><span data-stu-id="8138b-143">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="8138b-144">使用等於 in 的單一值 `*` ，會依 `VaryByQueryKeys` 所有要求查詢參數而改變快取。</span><span class="sxs-lookup"><span data-stu-id="8138b-144">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="8138b-145">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="8138b-145">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="8138b-146">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="8138b-146">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="8138b-147">頁首</span><span class="sxs-lookup"><span data-stu-id="8138b-147">Header</span></span> | <span data-ttu-id="8138b-148">詳細資料</span><span class="sxs-lookup"><span data-stu-id="8138b-148">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="8138b-149">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-149">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="8138b-150">中介軟體只會考慮以 cache 指示詞標示的快取回應 `public` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-150">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="8138b-151">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="8138b-151">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="8138b-152">最大壽命</span><span class="sxs-lookup"><span data-stu-id="8138b-152">max-age</span></span></li><li><span data-ttu-id="8138b-153">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="8138b-153">max-stale&#8224;</span></span></li><li><span data-ttu-id="8138b-154">最小-全新</span><span class="sxs-lookup"><span data-stu-id="8138b-154">min-fresh</span></span></li><li><span data-ttu-id="8138b-155">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="8138b-155">must-revalidate</span></span></li><li><span data-ttu-id="8138b-156">no-cache</span><span class="sxs-lookup"><span data-stu-id="8138b-156">no-cache</span></span></li><li><span data-ttu-id="8138b-157">否-存放區</span><span class="sxs-lookup"><span data-stu-id="8138b-157">no-store</span></span></li><li><span data-ttu-id="8138b-158">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="8138b-158">only-if-cached</span></span></li><li><span data-ttu-id="8138b-159">private</span><span class="sxs-lookup"><span data-stu-id="8138b-159">private</span></span></li><li><span data-ttu-id="8138b-160">public</span><span class="sxs-lookup"><span data-stu-id="8138b-160">public</span></span></li><li><span data-ttu-id="8138b-161">s-maxage</span><span class="sxs-lookup"><span data-stu-id="8138b-161">s-maxage</span></span></li><li><span data-ttu-id="8138b-162">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="8138b-162">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="8138b-163">&#8224;如果沒有指定任何限制 `max-stale` ，中介軟體就不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="8138b-163">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="8138b-164">&#8225;與 `proxy-revalidate` 具有相同的效果 `must-revalidate` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-164">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="8138b-165">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="8138b-165">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="8138b-166">`Pragma: no-cache`要求中的標頭會產生與相同的效果 `Cache-Control: no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-166">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="8138b-167">此標頭會由標頭中的相關指示詞覆寫 `Cache-Control` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="8138b-167">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="8138b-168">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="8138b-168">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="8138b-169">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-169">The response isn't cached if the header exists.</span></span> <span data-ttu-id="8138b-170">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="8138b-170">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="8138b-171">`Vary`標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-171">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="8138b-172">例如，藉由包含標頭的編碼方式來快取回應，這會快取 `Vary: Accept-Encoding` 標頭和個別要求的回應 `Accept-Encoding: gzip` `Accept-Encoding: text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-172">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="8138b-173">永遠不會儲存標頭值為的回應 `*` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-173">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="8138b-174">除非其他標頭覆寫，否則不會儲存或抓取此標頭所視為過時的回應 `Cache-Control` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-174">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="8138b-175">如果值不是 `*` ，而且 `ETag` 回應的不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-175">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="8138b-176">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-176">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="8138b-177">如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-177">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="8138b-178">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-178">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="8138b-179">從快取提供服務時， `Date` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="8138b-179">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="8138b-180">從快取提供服務時， `Content-Length` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="8138b-180">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="8138b-181">`Age`會忽略原始回應中所傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-181">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="8138b-182">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="8138b-182">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="8138b-183">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="8138b-183">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="8138b-184">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="8138b-184">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="8138b-185">這些規則需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-185">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="8138b-186">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-186">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="8138b-187">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="8138b-187">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="8138b-188">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="8138b-188">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="8138b-189">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="8138b-189">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="8138b-190">疑難排解</span><span class="sxs-lookup"><span data-stu-id="8138b-190">Troubleshooting</span></span>

<span data-ttu-id="8138b-191">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="8138b-191">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="8138b-192">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-192">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="8138b-193">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="8138b-193">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="8138b-194">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-194">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="8138b-195">例如，瀏覽器可能會在重新整理 `Cache-Control` 頁面時，將標頭設定為 `no-cache` 或 `max-age=0` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-195">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="8138b-196">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="8138b-196">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="8138b-197">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8138b-197">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="8138b-198">Postman</span><span class="sxs-lookup"><span data-stu-id="8138b-198">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="8138b-199">快取的條件</span><span class="sxs-lookup"><span data-stu-id="8138b-199">Conditions for caching</span></span>

* <span data-ttu-id="8138b-200">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-200">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="8138b-201">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="8138b-201">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="8138b-202">在中 `Startup.Configure` ，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="8138b-202">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="8138b-203">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="8138b-203">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="8138b-204">`Authorization`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="8138b-204">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="8138b-205">`Cache-Control`標頭參數必須是有效的，而且回應必須標記為 `public` 且未標記 `private` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-205">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="8138b-206">`Pragma: no-cache`如果標頭不存在，標頭不能出現 `Cache-Control` ，因為標頭會 `Cache-Control` `Pragma` 在出現時覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-206">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="8138b-207">`Set-Cookie`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="8138b-207">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="8138b-208">`Vary`標頭參數必須有效，而且不等於 `*` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-208">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="8138b-209">`Content-Length`標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="8138b-209">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="8138b-210"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>未使用。</span><span class="sxs-lookup"><span data-stu-id="8138b-210">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="8138b-211">回應不得過時，如 `Expires` 標頭和和快取指示詞所指定 `max-age` `s-maxage` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-211">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="8138b-212">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="8138b-212">Response buffering must be successful.</span></span> <span data-ttu-id="8138b-213">回應的大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> 。</span><span class="sxs-lookup"><span data-stu-id="8138b-213">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="8138b-214">回應的主體大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> 。</span><span class="sxs-lookup"><span data-stu-id="8138b-214">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="8138b-215">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="8138b-215">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="8138b-216">例如，指示詞 `no-store` 不能存在於要求或回應標頭欄位中。</span><span class="sxs-lookup"><span data-stu-id="8138b-216">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="8138b-217">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-217">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="8138b-218">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設為， `no-cache` 如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-218">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="8138b-219">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="8138b-219">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8138b-220">其他資源</span><span class="sxs-lookup"><span data-stu-id="8138b-220">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="8138b-221">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8138b-221">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="8138b-222">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-222">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="8138b-223">如需 HTTP 快取和屬性的簡介 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) ，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="8138b-223">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="8138b-224">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="8138b-224">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="8138b-225">設定</span><span class="sxs-lookup"><span data-stu-id="8138b-225">Configuration</span></span>

<span data-ttu-id="8138b-226">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)套件。</span><span class="sxs-lookup"><span data-stu-id="8138b-226">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="8138b-227">在中 `Startup.ConfigureServices` ，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="8138b-227">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="8138b-228">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，這會將中介軟體新增至中的要求處理管線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="8138b-228">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

<span data-ttu-id="8138b-229">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="8138b-229">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="8138b-230">[Cache-控制項](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash;快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-230">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="8138b-231">[差異](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash;只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會設定中介軟體來提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-231">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

<span data-ttu-id="8138b-232">前面的標頭不會寫入至回應，並會在控制器、動作或 Razor 頁面時遭到覆寫：</span><span class="sxs-lookup"><span data-stu-id="8138b-232">The preceding headers are not written to the response and are overridden when a controller, action, or Razor Page:</span></span>

* <span data-ttu-id="8138b-233">具有[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="8138b-233">Has a [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute.</span></span> <span data-ttu-id="8138b-234">即使未設定屬性，這也適用。</span><span class="sxs-lookup"><span data-stu-id="8138b-234">This applies even if a property isn't set.</span></span> <span data-ttu-id="8138b-235">例如，省略[VaryByHeader](/aspnet/core/performance/caching/response#vary)屬性將會從回應中移除對應的標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-235">For example, omitting the [VaryByHeader](/aspnet/core/performance/caching/response#vary) property will cause the corresponding header to be removed from the response.</span></span>

<span data-ttu-id="8138b-236">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-236">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="8138b-237">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="8138b-237">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="8138b-238">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-238">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="8138b-239">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="8138b-239">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="8138b-240">選項</span><span class="sxs-lookup"><span data-stu-id="8138b-240">Options</span></span>

<span data-ttu-id="8138b-241">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="8138b-241">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="8138b-242">選項</span><span class="sxs-lookup"><span data-stu-id="8138b-242">Option</span></span> | <span data-ttu-id="8138b-243">說明</span><span class="sxs-lookup"><span data-stu-id="8138b-243">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="8138b-244">回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="8138b-244">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="8138b-245">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="8138b-245">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="8138b-246">回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="8138b-246">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="8138b-247">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="8138b-247">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="8138b-248">決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-248">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="8138b-249">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="8138b-249">The default value is `false`.</span></span> |

<span data-ttu-id="8138b-250">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="8138b-250">The following example configures the middleware to:</span></span>

* <span data-ttu-id="8138b-251">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-251">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="8138b-252">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="8138b-252">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="8138b-253">例如， `/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="8138b-253">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="8138b-254">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="8138b-254">VaryByQueryKeys</span></span>

<span data-ttu-id="8138b-255">使用 MVC/Web API 控制器或 Razor Pages 頁面模型時， [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) 屬性會指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="8138b-255">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="8138b-256">`[ResponseCache]`嚴格需要中介軟體的屬性唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys> ，這並不會對應至實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-256">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="8138b-257">如需詳細資訊，請參閱<xref:performance/caching/response#responsecache-attribute>。</span><span class="sxs-lookup"><span data-stu-id="8138b-257">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="8138b-258">當不使用 `[ResponseCache]` 屬性時，回應快取可以與不同 `VaryByQueryKeys` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-258">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="8138b-259">直接使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> [HttpCoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)：</span><span class="sxs-lookup"><span data-stu-id="8138b-259">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="8138b-260">使用等於 in 的單一值 `*` ，會依 `VaryByQueryKeys` 所有要求查詢參數而改變快取。</span><span class="sxs-lookup"><span data-stu-id="8138b-260">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="8138b-261">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="8138b-261">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="8138b-262">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="8138b-262">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="8138b-263">頁首</span><span class="sxs-lookup"><span data-stu-id="8138b-263">Header</span></span> | <span data-ttu-id="8138b-264">詳細資料</span><span class="sxs-lookup"><span data-stu-id="8138b-264">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="8138b-265">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-265">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="8138b-266">中介軟體只會考慮以 cache 指示詞標示的快取回應 `public` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-266">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="8138b-267">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="8138b-267">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="8138b-268">最大壽命</span><span class="sxs-lookup"><span data-stu-id="8138b-268">max-age</span></span></li><li><span data-ttu-id="8138b-269">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="8138b-269">max-stale&#8224;</span></span></li><li><span data-ttu-id="8138b-270">最小-全新</span><span class="sxs-lookup"><span data-stu-id="8138b-270">min-fresh</span></span></li><li><span data-ttu-id="8138b-271">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="8138b-271">must-revalidate</span></span></li><li><span data-ttu-id="8138b-272">no-cache</span><span class="sxs-lookup"><span data-stu-id="8138b-272">no-cache</span></span></li><li><span data-ttu-id="8138b-273">否-存放區</span><span class="sxs-lookup"><span data-stu-id="8138b-273">no-store</span></span></li><li><span data-ttu-id="8138b-274">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="8138b-274">only-if-cached</span></span></li><li><span data-ttu-id="8138b-275">private</span><span class="sxs-lookup"><span data-stu-id="8138b-275">private</span></span></li><li><span data-ttu-id="8138b-276">public</span><span class="sxs-lookup"><span data-stu-id="8138b-276">public</span></span></li><li><span data-ttu-id="8138b-277">s-maxage</span><span class="sxs-lookup"><span data-stu-id="8138b-277">s-maxage</span></span></li><li><span data-ttu-id="8138b-278">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="8138b-278">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="8138b-279">&#8224;如果沒有指定任何限制 `max-stale` ，中介軟體就不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="8138b-279">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="8138b-280">&#8225;與 `proxy-revalidate` 具有相同的效果 `must-revalidate` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-280">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="8138b-281">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="8138b-281">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="8138b-282">`Pragma: no-cache`要求中的標頭會產生與相同的效果 `Cache-Control: no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-282">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="8138b-283">此標頭會由標頭中的相關指示詞覆寫 `Cache-Control` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="8138b-283">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="8138b-284">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="8138b-284">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="8138b-285">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-285">The response isn't cached if the header exists.</span></span> <span data-ttu-id="8138b-286">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="8138b-286">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="8138b-287">`Vary`標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-287">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="8138b-288">例如，藉由包含標頭的編碼方式來快取回應，這會快取 `Vary: Accept-Encoding` 標頭和個別要求的回應 `Accept-Encoding: gzip` `Accept-Encoding: text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-288">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="8138b-289">永遠不會儲存標頭值為的回應 `*` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-289">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="8138b-290">除非其他標頭覆寫，否則不會儲存或抓取此標頭所視為過時的回應 `Cache-Control` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-290">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="8138b-291">如果值不是 `*` ，而且 `ETag` 回應的不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-291">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="8138b-292">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-292">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="8138b-293">如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-293">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="8138b-294">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-294">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="8138b-295">從快取提供服務時， `Date` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="8138b-295">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="8138b-296">從快取提供服務時， `Content-Length` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="8138b-296">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="8138b-297">`Age`會忽略原始回應中所傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-297">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="8138b-298">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="8138b-298">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="8138b-299">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="8138b-299">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="8138b-300">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="8138b-300">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="8138b-301">這些規則需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-301">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="8138b-302">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-302">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="8138b-303">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="8138b-303">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="8138b-304">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="8138b-304">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="8138b-305">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="8138b-305">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="8138b-306">疑難排解</span><span class="sxs-lookup"><span data-stu-id="8138b-306">Troubleshooting</span></span>

<span data-ttu-id="8138b-307">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="8138b-307">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="8138b-308">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-308">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="8138b-309">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="8138b-309">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="8138b-310">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-310">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="8138b-311">例如，瀏覽器可能會在重新整理 `Cache-Control` 頁面時，將標頭設定為 `no-cache` 或 `max-age=0` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-311">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="8138b-312">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="8138b-312">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="8138b-313">Fiddler</span><span class="sxs-lookup"><span data-stu-id="8138b-313">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="8138b-314">Postman</span><span class="sxs-lookup"><span data-stu-id="8138b-314">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="8138b-315">快取的條件</span><span class="sxs-lookup"><span data-stu-id="8138b-315">Conditions for caching</span></span>

* <span data-ttu-id="8138b-316">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-316">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="8138b-317">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="8138b-317">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="8138b-318">在中 `Startup.Configure` ，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="8138b-318">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="8138b-319">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="8138b-319">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="8138b-320">`Authorization`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="8138b-320">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="8138b-321">`Cache-Control`標頭參數必須是有效的，而且回應必須標記為 `public` 且未標記 `private` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-321">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="8138b-322">`Pragma: no-cache`如果標頭不存在，標頭不能出現 `Cache-Control` ，因為標頭會 `Cache-Control` `Pragma` 在出現時覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="8138b-322">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="8138b-323">`Set-Cookie`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="8138b-323">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="8138b-324">`Vary`標頭參數必須有效，而且不等於 `*` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-324">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="8138b-325">`Content-Length`標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="8138b-325">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="8138b-326"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>未使用。</span><span class="sxs-lookup"><span data-stu-id="8138b-326">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="8138b-327">回應不得過時，如 `Expires` 標頭和和快取指示詞所指定 `max-age` `s-maxage` 。</span><span class="sxs-lookup"><span data-stu-id="8138b-327">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="8138b-328">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="8138b-328">Response buffering must be successful.</span></span> <span data-ttu-id="8138b-329">回應的大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> 。</span><span class="sxs-lookup"><span data-stu-id="8138b-329">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="8138b-330">回應的主體大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> 。</span><span class="sxs-lookup"><span data-stu-id="8138b-330">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="8138b-331">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="8138b-331">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="8138b-332">例如，指示詞 `no-store` 不能存在於要求或回應標頭欄位中。</span><span class="sxs-lookup"><span data-stu-id="8138b-332">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="8138b-333">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-333">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="8138b-334">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設為， `no-cache` 如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="8138b-334">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="8138b-335">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="8138b-335">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8138b-336">其他資源</span><span class="sxs-lookup"><span data-stu-id="8138b-336">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
