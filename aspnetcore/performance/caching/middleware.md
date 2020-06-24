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
ms.openlocfilehash: 93ac4e7e159f2b1f031e48a44c2297a741ba7b1c
ms.sourcegitcommit: 5e462c3328c70f95969d02adce9c71592049f54c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/24/2020
ms.locfileid: "85292642"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="80dae-103">ASP.NET Core 中的回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="80dae-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="80dae-104">作者：[John Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="80dae-104">By [John Luo](https://github.com/JunTaoLuo)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="80dae-105">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dae-105">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="80dae-106">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-106">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="80dae-107">如需 HTTP 快取和屬性的簡介 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) ，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="80dae-107">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="80dae-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="80dae-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="80dae-109">設定</span><span class="sxs-lookup"><span data-stu-id="80dae-109">Configuration</span></span>

<span data-ttu-id="80dae-110">回應快取中介軟體可透過共用架構隱含地用於 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="80dae-110">Response Caching Middleware is implicitly available for ASP.NET Core apps via the shared framework.</span></span>

<span data-ttu-id="80dae-111">在中 `Startup.ConfigureServices` ，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="80dae-111">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="80dae-112">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，這會將中介軟體新增至中的要求處理管線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="80dae-112">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=17)]

> [!WARNING]
> <span data-ttu-id="80dae-113"><xref:Owin.CorsExtensions.UseCors%2A><xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A>使用[CORS 中介軟體](xref:security/cors)之前，必須先呼叫。</span><span class="sxs-lookup"><span data-stu-id="80dae-113"><xref:Owin.CorsExtensions.UseCors%2A> must be called before <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching%2A> when using [CORS middleware](xref:security/cors).</span></span>

<span data-ttu-id="80dae-114">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="80dae-114">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="80dae-115">[Cache-控制項](https://tools.ietf.org/html/rfc7234#section-5.2)：快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-115">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2): Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="80dae-116">[不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)：只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會設定中介軟體來提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-116">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4): Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

<span data-ttu-id="80dae-117">前面的標頭不會寫入至回應，並會在控制器、動作或頁面上被覆寫 Razor ：</span><span class="sxs-lookup"><span data-stu-id="80dae-117">The preceding headers are not written to the response and are overridden when a controller, action, or Razor Page:</span></span>

* <span data-ttu-id="80dae-118">具有[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="80dae-118">Has a [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute.</span></span> <span data-ttu-id="80dae-119">即使未設定屬性，這也適用。</span><span class="sxs-lookup"><span data-stu-id="80dae-119">This applies even if a property isn't set.</span></span> <span data-ttu-id="80dae-120">例如，省略[VaryByHeader](/aspnet/core/performance/caching/response#vary)屬性將會從回應中移除對應的標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-120">For example, omitting the [VaryByHeader](/aspnet/core/performance/caching/response#vary) property will cause the corresponding header to be removed from the response.</span></span>

<span data-ttu-id="80dae-121">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-121">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="80dae-122">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="80dae-122">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="80dae-123">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-123">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="80dae-124">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="80dae-124">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="80dae-125">選項</span><span class="sxs-lookup"><span data-stu-id="80dae-125">Options</span></span>

<span data-ttu-id="80dae-126">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="80dae-126">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="80dae-127">選項</span><span class="sxs-lookup"><span data-stu-id="80dae-127">Option</span></span> | <span data-ttu-id="80dae-128">描述</span><span class="sxs-lookup"><span data-stu-id="80dae-128">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="80dae-129">回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="80dae-129">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="80dae-130">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="80dae-130">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="80dae-131">回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="80dae-131">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="80dae-132">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="80dae-132">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="80dae-133">決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-133">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="80dae-134">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="80dae-134">The default value is `false`.</span></span> |

<span data-ttu-id="80dae-135">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="80dae-135">The following example configures the middleware to:</span></span>

* <span data-ttu-id="80dae-136">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-136">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="80dae-137">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="80dae-137">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="80dae-138">例如， `/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="80dae-138">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="80dae-139">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="80dae-139">VaryByQueryKeys</span></span>

<span data-ttu-id="80dae-140">使用 MVC/Web API 控制器或 Razor 頁面模型時，屬性會 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) 指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="80dae-140">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="80dae-141">`[ResponseCache]`嚴格需要中介軟體的屬性唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys> ，這並不會對應至實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-141">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="80dae-142">如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-142">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="80dae-143">當不使用 `[ResponseCache]` 屬性時，回應快取可以與不同 `VaryByQueryKeys` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-143">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="80dae-144">直接使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> [HttpCoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)：</span><span class="sxs-lookup"><span data-stu-id="80dae-144">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="80dae-145">使用等於 in 的單一值 `*` ，會依 `VaryByQueryKeys` 所有要求查詢參數而改變快取。</span><span class="sxs-lookup"><span data-stu-id="80dae-145">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="80dae-146">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="80dae-146">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="80dae-147">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="80dae-147">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="80dae-148">頁首</span><span class="sxs-lookup"><span data-stu-id="80dae-148">Header</span></span> | <span data-ttu-id="80dae-149">詳細資料</span><span class="sxs-lookup"><span data-stu-id="80dae-149">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="80dae-150">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-150">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="80dae-151">中介軟體只會考慮以 cache 指示詞標示的快取回應 `public` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-151">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="80dae-152">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="80dae-152">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="80dae-153">最大壽命</span><span class="sxs-lookup"><span data-stu-id="80dae-153">max-age</span></span></li><li><span data-ttu-id="80dae-154">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="80dae-154">max-stale&#8224;</span></span></li><li><span data-ttu-id="80dae-155">最小-全新</span><span class="sxs-lookup"><span data-stu-id="80dae-155">min-fresh</span></span></li><li><span data-ttu-id="80dae-156">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="80dae-156">must-revalidate</span></span></li><li><span data-ttu-id="80dae-157">no-cache</span><span class="sxs-lookup"><span data-stu-id="80dae-157">no-cache</span></span></li><li><span data-ttu-id="80dae-158">否-存放區</span><span class="sxs-lookup"><span data-stu-id="80dae-158">no-store</span></span></li><li><span data-ttu-id="80dae-159">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="80dae-159">only-if-cached</span></span></li><li><span data-ttu-id="80dae-160">private</span><span class="sxs-lookup"><span data-stu-id="80dae-160">private</span></span></li><li><span data-ttu-id="80dae-161">public</span><span class="sxs-lookup"><span data-stu-id="80dae-161">public</span></span></li><li><span data-ttu-id="80dae-162">s-maxage</span><span class="sxs-lookup"><span data-stu-id="80dae-162">s-maxage</span></span></li><li><span data-ttu-id="80dae-163">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="80dae-163">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="80dae-164">&#8224;如果沒有指定任何限制 `max-stale` ，中介軟體就不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="80dae-164">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="80dae-165">&#8225;與 `proxy-revalidate` 具有相同的效果 `must-revalidate` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-165">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="80dae-166">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="80dae-166">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="80dae-167">`Pragma: no-cache`要求中的標頭會產生與相同的效果 `Cache-Control: no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-167">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="80dae-168">此標頭會由標頭中的相關指示詞覆寫 `Cache-Control` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="80dae-168">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="80dae-169">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="80dae-169">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="80dae-170">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-170">The response isn't cached if the header exists.</span></span> <span data-ttu-id="80dae-171">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="80dae-171">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="80dae-172">`Vary`標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-172">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="80dae-173">例如，藉由包含標頭的編碼方式來快取回應，這會快取 `Vary: Accept-Encoding` 標頭和個別要求的回應 `Accept-Encoding: gzip` `Accept-Encoding: text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-173">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="80dae-174">永遠不會儲存標頭值為的回應 `*` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-174">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="80dae-175">除非其他標頭覆寫，否則不會儲存或抓取此標頭所視為過時的回應 `Cache-Control` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-175">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="80dae-176">如果值不是 `*` ，而且 `ETag` 回應的不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-176">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="80dae-177">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-177">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="80dae-178">如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-178">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="80dae-179">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-179">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="80dae-180">從快取提供服務時， `Date` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="80dae-180">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="80dae-181">從快取提供服務時， `Content-Length` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="80dae-181">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="80dae-182">`Age`會忽略原始回應中所傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-182">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="80dae-183">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="80dae-183">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="80dae-184">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="80dae-184">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="80dae-185">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="80dae-185">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="80dae-186">這些規則需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-186">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="80dae-187">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-187">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="80dae-188">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="80dae-188">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="80dae-189">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="80dae-189">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="80dae-190">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="80dae-190">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="80dae-191">疑難排解</span><span class="sxs-lookup"><span data-stu-id="80dae-191">Troubleshooting</span></span>

<span data-ttu-id="80dae-192">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="80dae-192">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="80dae-193">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-193">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="80dae-194">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="80dae-194">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="80dae-195">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-195">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="80dae-196">例如，瀏覽器可能會在重新整理 `Cache-Control` 頁面時，將標頭設定為 `no-cache` 或 `max-age=0` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-196">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="80dae-197">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="80dae-197">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="80dae-198">Fiddler</span><span class="sxs-lookup"><span data-stu-id="80dae-198">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="80dae-199">Postman</span><span class="sxs-lookup"><span data-stu-id="80dae-199">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="80dae-200">快取的條件</span><span class="sxs-lookup"><span data-stu-id="80dae-200">Conditions for caching</span></span>

* <span data-ttu-id="80dae-201">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-201">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="80dae-202">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="80dae-202">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="80dae-203">在中 `Startup.Configure` ，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="80dae-203">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="80dae-204">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-204">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="80dae-205">`Authorization`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="80dae-205">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="80dae-206">`Cache-Control`標頭參數必須是有效的，而且回應必須標記為 `public` 且未標記 `private` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-206">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="80dae-207">`Pragma: no-cache`如果標頭不存在，標頭不能出現 `Cache-Control` ，因為標頭會 `Cache-Control` `Pragma` 在出現時覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-207">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="80dae-208">`Set-Cookie`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="80dae-208">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="80dae-209">`Vary`標頭參數必須有效，而且不等於 `*` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-209">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="80dae-210">`Content-Length`標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="80dae-210">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="80dae-211"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>未使用。</span><span class="sxs-lookup"><span data-stu-id="80dae-211">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="80dae-212">回應不得過時，如 `Expires` 標頭和和快取指示詞所指定 `max-age` `s-maxage` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-212">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="80dae-213">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="80dae-213">Response buffering must be successful.</span></span> <span data-ttu-id="80dae-214">回應的大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-214">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="80dae-215">回應的主體大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-215">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="80dae-216">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="80dae-216">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="80dae-217">例如，指示詞 `no-store` 不能存在於要求或回應標頭欄位中。</span><span class="sxs-lookup"><span data-stu-id="80dae-217">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="80dae-218">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-218">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="80dae-219">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設為， `no-cache` 如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-219">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="80dae-220">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-220">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80dae-221">其他資源</span><span class="sxs-lookup"><span data-stu-id="80dae-221">Additional resources</span></span>

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

<span data-ttu-id="80dae-222">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="80dae-222">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="80dae-223">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-223">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="80dae-224">如需 HTTP 快取和屬性的簡介 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) ，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="80dae-224">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="80dae-225">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="80dae-225">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="80dae-226">設定</span><span class="sxs-lookup"><span data-stu-id="80dae-226">Configuration</span></span>

<span data-ttu-id="80dae-227">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)套件。</span><span class="sxs-lookup"><span data-stu-id="80dae-227">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="80dae-228">在中 `Startup.ConfigureServices` ，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="80dae-228">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="80dae-229">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，這會將中介軟體新增至中的要求處理管線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="80dae-229">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

<span data-ttu-id="80dae-230">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="80dae-230">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="80dae-231">[Cache-控制項](https://tools.ietf.org/html/rfc7234#section-5.2)：快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-231">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2): Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="80dae-232">[不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)：只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會設定中介軟體來提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-232">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4): Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

<span data-ttu-id="80dae-233">前面的標頭不會寫入至回應，並會在控制器、動作或頁面上被覆寫 Razor ：</span><span class="sxs-lookup"><span data-stu-id="80dae-233">The preceding headers are not written to the response and are overridden when a controller, action, or Razor Page:</span></span>

* <span data-ttu-id="80dae-234">具有[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="80dae-234">Has a [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute.</span></span> <span data-ttu-id="80dae-235">即使未設定屬性，這也適用。</span><span class="sxs-lookup"><span data-stu-id="80dae-235">This applies even if a property isn't set.</span></span> <span data-ttu-id="80dae-236">例如，省略[VaryByHeader](/aspnet/core/performance/caching/response#vary)屬性將會從回應中移除對應的標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-236">For example, omitting the [VaryByHeader](/aspnet/core/performance/caching/response#vary) property will cause the corresponding header to be removed from the response.</span></span>

<span data-ttu-id="80dae-237">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-237">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="80dae-238">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="80dae-238">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="80dae-239">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-239">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="80dae-240">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="80dae-240">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="80dae-241">選項</span><span class="sxs-lookup"><span data-stu-id="80dae-241">Options</span></span>

<span data-ttu-id="80dae-242">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="80dae-242">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="80dae-243">選項</span><span class="sxs-lookup"><span data-stu-id="80dae-243">Option</span></span> | <span data-ttu-id="80dae-244">描述</span><span class="sxs-lookup"><span data-stu-id="80dae-244">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="80dae-245">回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="80dae-245">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="80dae-246">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="80dae-246">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="80dae-247">回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="80dae-247">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="80dae-248">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="80dae-248">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="80dae-249">決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-249">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="80dae-250">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="80dae-250">The default value is `false`.</span></span> |

<span data-ttu-id="80dae-251">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="80dae-251">The following example configures the middleware to:</span></span>

* <span data-ttu-id="80dae-252">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-252">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="80dae-253">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="80dae-253">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="80dae-254">例如， `/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="80dae-254">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="80dae-255">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="80dae-255">VaryByQueryKeys</span></span>

<span data-ttu-id="80dae-256">使用 MVC/Web API 控制器或 Razor 頁面模型時，屬性會 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) 指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="80dae-256">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="80dae-257">`[ResponseCache]`嚴格需要中介軟體的屬性唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys> ，這並不會對應至實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-257">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="80dae-258">如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-258">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="80dae-259">當不使用 `[ResponseCache]` 屬性時，回應快取可以與不同 `VaryByQueryKeys` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-259">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="80dae-260">直接使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> [HttpCoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)：</span><span class="sxs-lookup"><span data-stu-id="80dae-260">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="80dae-261">使用等於 in 的單一值 `*` ，會依 `VaryByQueryKeys` 所有要求查詢參數而改變快取。</span><span class="sxs-lookup"><span data-stu-id="80dae-261">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="80dae-262">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="80dae-262">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="80dae-263">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="80dae-263">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="80dae-264">頁首</span><span class="sxs-lookup"><span data-stu-id="80dae-264">Header</span></span> | <span data-ttu-id="80dae-265">詳細資料</span><span class="sxs-lookup"><span data-stu-id="80dae-265">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="80dae-266">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-266">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="80dae-267">中介軟體只會考慮以 cache 指示詞標示的快取回應 `public` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-267">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="80dae-268">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="80dae-268">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="80dae-269">最大壽命</span><span class="sxs-lookup"><span data-stu-id="80dae-269">max-age</span></span></li><li><span data-ttu-id="80dae-270">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="80dae-270">max-stale&#8224;</span></span></li><li><span data-ttu-id="80dae-271">最小-全新</span><span class="sxs-lookup"><span data-stu-id="80dae-271">min-fresh</span></span></li><li><span data-ttu-id="80dae-272">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="80dae-272">must-revalidate</span></span></li><li><span data-ttu-id="80dae-273">no-cache</span><span class="sxs-lookup"><span data-stu-id="80dae-273">no-cache</span></span></li><li><span data-ttu-id="80dae-274">否-存放區</span><span class="sxs-lookup"><span data-stu-id="80dae-274">no-store</span></span></li><li><span data-ttu-id="80dae-275">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="80dae-275">only-if-cached</span></span></li><li><span data-ttu-id="80dae-276">private</span><span class="sxs-lookup"><span data-stu-id="80dae-276">private</span></span></li><li><span data-ttu-id="80dae-277">public</span><span class="sxs-lookup"><span data-stu-id="80dae-277">public</span></span></li><li><span data-ttu-id="80dae-278">s-maxage</span><span class="sxs-lookup"><span data-stu-id="80dae-278">s-maxage</span></span></li><li><span data-ttu-id="80dae-279">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="80dae-279">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="80dae-280">&#8224;如果沒有指定任何限制 `max-stale` ，中介軟體就不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="80dae-280">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="80dae-281">&#8225;與 `proxy-revalidate` 具有相同的效果 `must-revalidate` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-281">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="80dae-282">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="80dae-282">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="80dae-283">`Pragma: no-cache`要求中的標頭會產生與相同的效果 `Cache-Control: no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-283">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="80dae-284">此標頭會由標頭中的相關指示詞覆寫 `Cache-Control` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="80dae-284">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="80dae-285">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="80dae-285">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="80dae-286">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-286">The response isn't cached if the header exists.</span></span> <span data-ttu-id="80dae-287">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="80dae-287">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="80dae-288">`Vary`標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-288">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="80dae-289">例如，藉由包含標頭的編碼方式來快取回應，這會快取 `Vary: Accept-Encoding` 標頭和個別要求的回應 `Accept-Encoding: gzip` `Accept-Encoding: text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-289">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="80dae-290">永遠不會儲存標頭值為的回應 `*` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-290">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="80dae-291">除非其他標頭覆寫，否則不會儲存或抓取此標頭所視為過時的回應 `Cache-Control` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-291">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="80dae-292">如果值不是 `*` ，而且 `ETag` 回應的不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-292">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="80dae-293">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-293">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="80dae-294">如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-294">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="80dae-295">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-295">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="80dae-296">從快取提供服務時， `Date` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="80dae-296">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="80dae-297">從快取提供服務時， `Content-Length` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="80dae-297">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="80dae-298">`Age`會忽略原始回應中所傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-298">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="80dae-299">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="80dae-299">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="80dae-300">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="80dae-300">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="80dae-301">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="80dae-301">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="80dae-302">這些規則需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-302">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="80dae-303">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-303">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="80dae-304">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="80dae-304">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="80dae-305">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="80dae-305">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="80dae-306">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="80dae-306">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="80dae-307">疑難排解</span><span class="sxs-lookup"><span data-stu-id="80dae-307">Troubleshooting</span></span>

<span data-ttu-id="80dae-308">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="80dae-308">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="80dae-309">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-309">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="80dae-310">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="80dae-310">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="80dae-311">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-311">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="80dae-312">例如，瀏覽器可能會在重新整理 `Cache-Control` 頁面時，將標頭設定為 `no-cache` 或 `max-age=0` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-312">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="80dae-313">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="80dae-313">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="80dae-314">Fiddler</span><span class="sxs-lookup"><span data-stu-id="80dae-314">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="80dae-315">Postman</span><span class="sxs-lookup"><span data-stu-id="80dae-315">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="80dae-316">快取的條件</span><span class="sxs-lookup"><span data-stu-id="80dae-316">Conditions for caching</span></span>

* <span data-ttu-id="80dae-317">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-317">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="80dae-318">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="80dae-318">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="80dae-319">在中 `Startup.Configure` ，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="80dae-319">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="80dae-320">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-320">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="80dae-321">`Authorization`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="80dae-321">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="80dae-322">`Cache-Control`標頭參數必須是有效的，而且回應必須標記為 `public` 且未標記 `private` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-322">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="80dae-323">`Pragma: no-cache`如果標頭不存在，標頭不能出現 `Cache-Control` ，因為標頭會 `Cache-Control` `Pragma` 在出現時覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="80dae-323">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="80dae-324">`Set-Cookie`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="80dae-324">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="80dae-325">`Vary`標頭參數必須有效，而且不等於 `*` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-325">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="80dae-326">`Content-Length`標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="80dae-326">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="80dae-327"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>未使用。</span><span class="sxs-lookup"><span data-stu-id="80dae-327">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="80dae-328">回應不得過時，如 `Expires` 標頭和和快取指示詞所指定 `max-age` `s-maxage` 。</span><span class="sxs-lookup"><span data-stu-id="80dae-328">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="80dae-329">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="80dae-329">Response buffering must be successful.</span></span> <span data-ttu-id="80dae-330">回應的大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-330">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="80dae-331">回應的主體大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-331">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="80dae-332">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="80dae-332">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="80dae-333">例如，指示詞 `no-store` 不能存在於要求或回應標頭欄位中。</span><span class="sxs-lookup"><span data-stu-id="80dae-333">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="80dae-334">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-334">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="80dae-335">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設為， `no-cache` 如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="80dae-335">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="80dae-336">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="80dae-336">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80dae-337">其他資源</span><span class="sxs-lookup"><span data-stu-id="80dae-337">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
