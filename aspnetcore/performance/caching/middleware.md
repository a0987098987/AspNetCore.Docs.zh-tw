---
title: ASP.NET Core 中的回應快取中介軟體
author: rick-anderson
description: 了解如何設定和使用 ASP.NET Core 中的回應快取中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/caching/middleware
ms.openlocfilehash: 4deac15538d4607bd611c4e072daae39447681c1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78655733"
---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="04cad-103">ASP.NET Core 中的回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="04cad-103">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="04cad-104">作者：[John Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="04cad-104">By [John Luo](https://github.com/JunTaoLuo)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="04cad-105">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="04cad-105">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="04cad-106">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-106">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="04cad-107">如需 HTTP 快取和[`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性的簡介，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="04cad-107">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="04cad-108">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="04cad-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="04cad-109">組態</span><span class="sxs-lookup"><span data-stu-id="04cad-109">Configuration</span></span>

<span data-ttu-id="04cad-110">回應快取中介軟體可透過共用架構隱含地用於 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="04cad-110">Response Caching Middleware is implicitly available for ASP.NET Core apps via the shared framework.</span></span>

<span data-ttu-id="04cad-111">在 `Startup.ConfigureServices`中，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="04cad-111">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="04cad-112">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，將中介軟體新增至 `Startup.Configure`中的要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="04cad-112">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

<span data-ttu-id="04cad-113">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="04cad-113">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="04cad-114">快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)&ndash; 快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-114">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="04cad-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; 設定中介軟體只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-115">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

<span data-ttu-id="04cad-116">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-116">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="04cad-117">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="04cad-117">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="04cad-118">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-118">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="04cad-119">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="04cad-119">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="04cad-120">選項。</span><span class="sxs-lookup"><span data-stu-id="04cad-120">Options</span></span>

<span data-ttu-id="04cad-121">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="04cad-121">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="04cad-122">選項</span><span class="sxs-lookup"><span data-stu-id="04cad-122">Option</span></span> | <span data-ttu-id="04cad-123">描述</span><span class="sxs-lookup"><span data-stu-id="04cad-123">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="04cad-124">回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="04cad-124">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="04cad-125">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="04cad-125">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="04cad-126">回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="04cad-126">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="04cad-127">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="04cad-127">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="04cad-128">決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-128">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="04cad-129">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="04cad-129">The default value is `false`.</span></span> |

<span data-ttu-id="04cad-130">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="04cad-130">The following example configures the middleware to:</span></span>

* <span data-ttu-id="04cad-131">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-131">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="04cad-132">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="04cad-132">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="04cad-133">例如，`/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="04cad-133">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="04cad-134">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="04cad-134">VaryByQueryKeys</span></span>

<span data-ttu-id="04cad-135">使用 MVC/Web API 控制器或 Razor Pages 頁面模型時， [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性會指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="04cad-135">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="04cad-136">嚴格要求中介軟體的 `[ResponseCache]` 屬性的唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>，這不會對應到實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-136">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="04cad-137">如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute>。</span><span class="sxs-lookup"><span data-stu-id="04cad-137">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="04cad-138">當不使用 `[ResponseCache]` 屬性時，回應快取可能會隨著 `VaryByQueryKeys`而不同。</span><span class="sxs-lookup"><span data-stu-id="04cad-138">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="04cad-139">直接從[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Features)使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature>：</span><span class="sxs-lookup"><span data-stu-id="04cad-139">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="04cad-140">使用等於 `*` 的單一值 `VaryByQueryKeys` 會依所有要求查詢參數來改變快取。</span><span class="sxs-lookup"><span data-stu-id="04cad-140">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="04cad-141">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="04cad-141">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="04cad-142">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="04cad-142">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="04cad-143">頁首</span><span class="sxs-lookup"><span data-stu-id="04cad-143">Header</span></span> | <span data-ttu-id="04cad-144">詳細資料</span><span class="sxs-lookup"><span data-stu-id="04cad-144">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="04cad-145">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-145">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="04cad-146">中介軟體只會考慮以 `public` cache 指示詞標示的快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-146">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="04cad-147">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="04cad-147">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="04cad-148">最大壽命</span><span class="sxs-lookup"><span data-stu-id="04cad-148">max-age</span></span></li><li><span data-ttu-id="04cad-149">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="04cad-149">max-stale&#8224;</span></span></li><li><span data-ttu-id="04cad-150">最小-全新</span><span class="sxs-lookup"><span data-stu-id="04cad-150">min-fresh</span></span></li><li><span data-ttu-id="04cad-151">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="04cad-151">must-revalidate</span></span></li><li><span data-ttu-id="04cad-152">no-cache</span><span class="sxs-lookup"><span data-stu-id="04cad-152">no-cache</span></span></li><li><span data-ttu-id="04cad-153">否-存放區</span><span class="sxs-lookup"><span data-stu-id="04cad-153">no-store</span></span></li><li><span data-ttu-id="04cad-154">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="04cad-154">only-if-cached</span></span></li><li><span data-ttu-id="04cad-155">私用</span><span class="sxs-lookup"><span data-stu-id="04cad-155">private</span></span></li><li><span data-ttu-id="04cad-156">公開</span><span class="sxs-lookup"><span data-stu-id="04cad-156">public</span></span></li><li><span data-ttu-id="04cad-157">s-maxage</span><span class="sxs-lookup"><span data-stu-id="04cad-157">s-maxage</span></span></li><li><span data-ttu-id="04cad-158">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="04cad-158">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="04cad-159">&#8224;如果未指定 `max-stale`的限制，中介軟體不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="04cad-159">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="04cad-160">&#8225;`proxy-revalidate` 與 `must-revalidate`的效果相同。</span><span class="sxs-lookup"><span data-stu-id="04cad-160">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="04cad-161">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="04cad-161">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="04cad-162">要求中的 `Pragma: no-cache` 標頭會產生與 `Cache-Control: no-cache`相同的效果。</span><span class="sxs-lookup"><span data-stu-id="04cad-162">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="04cad-163">`Cache-Control` 標頭中的相關指示詞會覆寫此標頭（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="04cad-163">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="04cad-164">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="04cad-164">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="04cad-165">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-165">The response isn't cached if the header exists.</span></span> <span data-ttu-id="04cad-166">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="04cad-166">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="04cad-167">`Vary` 標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-167">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="04cad-168">例如，藉由包含 `Vary: Accept-Encoding` 標頭，以編碼方式快取回應，其會快取標頭 `Accept-Encoding: gzip` 和 `Accept-Encoding: text/plain` 的要求回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-168">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="04cad-169">永遠不會儲存標頭值為 `*` 的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-169">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="04cad-170">除非由其他 `Cache-Control` 標頭覆寫，否則此標頭所視為過時的回應不會儲存或抓取。</span><span class="sxs-lookup"><span data-stu-id="04cad-170">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="04cad-171">如果值不 `*`，而且回應的 `ETag` 不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-171">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="04cad-172">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-172">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="04cad-173">如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-173">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="04cad-174">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-174">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="04cad-175">從快取提供服務時，如果原始回應未提供 `Date` 標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="04cad-175">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="04cad-176">從快取提供服務時，如果原始回應未提供 `Content-Length` 標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="04cad-176">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="04cad-177">會忽略原始回應中所傳送的 `Age` 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-177">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="04cad-178">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="04cad-178">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="04cad-179">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="04cad-179">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="04cad-180">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="04cad-180">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="04cad-181">這些規則需要快取以接受用戶端所傳送的有效 `Cache-Control` 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-181">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="04cad-182">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-182">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="04cad-183">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="04cad-183">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="04cad-184">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="04cad-184">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="04cad-185">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="04cad-185">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="04cad-186">疑難排解</span><span class="sxs-lookup"><span data-stu-id="04cad-186">Troubleshooting</span></span>

<span data-ttu-id="04cad-187">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="04cad-187">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="04cad-188">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-188">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="04cad-189">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="04cad-189">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="04cad-190">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-190">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="04cad-191">例如，瀏覽器可能會在重新整理頁面時，將 `Cache-Control` 標頭設定為 `no-cache` 或 `max-age=0`。</span><span class="sxs-lookup"><span data-stu-id="04cad-191">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="04cad-192">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="04cad-192">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="04cad-193">Fiddler</span><span class="sxs-lookup"><span data-stu-id="04cad-193">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="04cad-194">Postman</span><span class="sxs-lookup"><span data-stu-id="04cad-194">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="04cad-195">快取的條件</span><span class="sxs-lookup"><span data-stu-id="04cad-195">Conditions for caching</span></span>

* <span data-ttu-id="04cad-196">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-196">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="04cad-197">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="04cad-197">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="04cad-198">在 `Startup.Configure`中，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="04cad-198">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="04cad-199">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="04cad-199">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="04cad-200">`Authorization` 標頭必須不存在。</span><span class="sxs-lookup"><span data-stu-id="04cad-200">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="04cad-201">`Cache-Control` 標頭參數必須是有效的，而且必須將回應標記為 `public` 而不是標示為 `private`。</span><span class="sxs-lookup"><span data-stu-id="04cad-201">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="04cad-202">如果 `Cache-Control` 標頭不存在，則不能出現 `Pragma: no-cache` 標頭，因為 `Cache-Control` 標頭會在出現時覆寫 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-202">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="04cad-203">`Set-Cookie` 標頭必須不存在。</span><span class="sxs-lookup"><span data-stu-id="04cad-203">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="04cad-204">`Vary` 標頭參數必須有效，而且不等於 `*`。</span><span class="sxs-lookup"><span data-stu-id="04cad-204">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="04cad-205">`Content-Length` 標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="04cad-205">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="04cad-206">不使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>。</span><span class="sxs-lookup"><span data-stu-id="04cad-206">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="04cad-207">回應不得過時，如 `Expires` 標頭和 `max-age` 和 `s-maxage` cache 指示詞所指定。</span><span class="sxs-lookup"><span data-stu-id="04cad-207">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="04cad-208">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="04cad-208">Response buffering must be successful.</span></span> <span data-ttu-id="04cad-209">回應的大小必須小於設定的 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>或預設值。</span><span class="sxs-lookup"><span data-stu-id="04cad-209">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="04cad-210">回應的主體大小必須小於設定的或預設 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>。</span><span class="sxs-lookup"><span data-stu-id="04cad-210">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="04cad-211">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="04cad-211">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="04cad-212">例如，[`no-store`] 指示詞不能存在於 [要求] 或 [回應標頭] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="04cad-212">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="04cad-213">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-213">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="04cad-214">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設定為 `no-cache`，如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-214">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="04cad-215">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>。</span><span class="sxs-lookup"><span data-stu-id="04cad-215">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04cad-216">其他資源</span><span class="sxs-lookup"><span data-stu-id="04cad-216">Additional resources</span></span>

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

<span data-ttu-id="04cad-217">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="04cad-217">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="04cad-218">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-218">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="04cad-219">如需 HTTP 快取和[`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性的簡介，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="04cad-219">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="04cad-220">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="04cad-220">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="04cad-221">組態</span><span class="sxs-lookup"><span data-stu-id="04cad-221">Configuration</span></span>

<span data-ttu-id="04cad-222">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)套件。</span><span class="sxs-lookup"><span data-stu-id="04cad-222">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="04cad-223">在 `Startup.ConfigureServices`中，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="04cad-223">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="04cad-224">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，將中介軟體新增至 `Startup.Configure`中的要求處理管線：</span><span class="sxs-lookup"><span data-stu-id="04cad-224">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

<span data-ttu-id="04cad-225">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="04cad-225">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="04cad-226">快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)&ndash; 快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-226">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2) &ndash; Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="04cad-227">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; 設定中介軟體只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-227">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) &ndash; Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

<span data-ttu-id="04cad-228">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-228">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="04cad-229">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="04cad-229">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="04cad-230">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-230">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="04cad-231">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="04cad-231">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="04cad-232">選項。</span><span class="sxs-lookup"><span data-stu-id="04cad-232">Options</span></span>

<span data-ttu-id="04cad-233">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="04cad-233">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="04cad-234">選項</span><span class="sxs-lookup"><span data-stu-id="04cad-234">Option</span></span> | <span data-ttu-id="04cad-235">描述</span><span class="sxs-lookup"><span data-stu-id="04cad-235">Description</span></span> |
| ------ | ----------- |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | <span data-ttu-id="04cad-236">回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="04cad-236">The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="04cad-237">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="04cad-237">The default value is `64 * 1024 * 1024` (64 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | <span data-ttu-id="04cad-238">回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="04cad-238">The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="04cad-239">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="04cad-239">The default value is `100 * 1024 * 1024` (100 MB).</span></span> |
| <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | <span data-ttu-id="04cad-240">決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-240">Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="04cad-241">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="04cad-241">The default value is `false`.</span></span> |

<span data-ttu-id="04cad-242">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="04cad-242">The following example configures the middleware to:</span></span>

* <span data-ttu-id="04cad-243">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-243">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="04cad-244">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="04cad-244">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="04cad-245">例如，`/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="04cad-245">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="04cad-246">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="04cad-246">VaryByQueryKeys</span></span>

<span data-ttu-id="04cad-247">使用 MVC/Web API 控制器或 Razor Pages 頁面模型時， [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性會指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="04cad-247">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="04cad-248">嚴格要求中介軟體的 `[ResponseCache]` 屬性的唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>，這不會對應到實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-248">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="04cad-249">如需詳細資訊，請參閱 <xref:performance/caching/response#responsecache-attribute>。</span><span class="sxs-lookup"><span data-stu-id="04cad-249">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="04cad-250">當不使用 `[ResponseCache]` 屬性時，回應快取可能會隨著 `VaryByQueryKeys`而不同。</span><span class="sxs-lookup"><span data-stu-id="04cad-250">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="04cad-251">直接從[HttpCoNtext](xref:Microsoft.AspNetCore.Http.HttpContext.Features)使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature>：</span><span class="sxs-lookup"><span data-stu-id="04cad-251">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="04cad-252">使用等於 `*` 的單一值 `VaryByQueryKeys` 會依所有要求查詢參數來改變快取。</span><span class="sxs-lookup"><span data-stu-id="04cad-252">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="04cad-253">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="04cad-253">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="04cad-254">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="04cad-254">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="04cad-255">頁首</span><span class="sxs-lookup"><span data-stu-id="04cad-255">Header</span></span> | <span data-ttu-id="04cad-256">詳細資料</span><span class="sxs-lookup"><span data-stu-id="04cad-256">Details</span></span> |
| ------ | ------- |
| `Authorization` | <span data-ttu-id="04cad-257">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-257">The response isn't cached if the header exists.</span></span> |
| `Cache-Control` | <span data-ttu-id="04cad-258">中介軟體只會考慮以 `public` cache 指示詞標示的快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-258">The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="04cad-259">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="04cad-259">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="04cad-260">最大壽命</span><span class="sxs-lookup"><span data-stu-id="04cad-260">max-age</span></span></li><li><span data-ttu-id="04cad-261">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="04cad-261">max-stale&#8224;</span></span></li><li><span data-ttu-id="04cad-262">最小-全新</span><span class="sxs-lookup"><span data-stu-id="04cad-262">min-fresh</span></span></li><li><span data-ttu-id="04cad-263">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="04cad-263">must-revalidate</span></span></li><li><span data-ttu-id="04cad-264">no-cache</span><span class="sxs-lookup"><span data-stu-id="04cad-264">no-cache</span></span></li><li><span data-ttu-id="04cad-265">否-存放區</span><span class="sxs-lookup"><span data-stu-id="04cad-265">no-store</span></span></li><li><span data-ttu-id="04cad-266">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="04cad-266">only-if-cached</span></span></li><li><span data-ttu-id="04cad-267">私用</span><span class="sxs-lookup"><span data-stu-id="04cad-267">private</span></span></li><li><span data-ttu-id="04cad-268">公開</span><span class="sxs-lookup"><span data-stu-id="04cad-268">public</span></span></li><li><span data-ttu-id="04cad-269">s-maxage</span><span class="sxs-lookup"><span data-stu-id="04cad-269">s-maxage</span></span></li><li><span data-ttu-id="04cad-270">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="04cad-270">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="04cad-271">&#8224;如果未指定 `max-stale`的限制，中介軟體不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="04cad-271">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="04cad-272">&#8225;`proxy-revalidate` 與 `must-revalidate`的效果相同。</span><span class="sxs-lookup"><span data-stu-id="04cad-272">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="04cad-273">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="04cad-273">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> |
| `Pragma` | <span data-ttu-id="04cad-274">要求中的 `Pragma: no-cache` 標頭會產生與 `Cache-Control: no-cache`相同的效果。</span><span class="sxs-lookup"><span data-stu-id="04cad-274">A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="04cad-275">`Cache-Control` 標頭中的相關指示詞會覆寫此標頭（如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="04cad-275">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="04cad-276">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="04cad-276">Considered for backward compatibility with HTTP/1.0.</span></span> |
| `Set-Cookie` | <span data-ttu-id="04cad-277">如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-277">The response isn't cached if the header exists.</span></span> <span data-ttu-id="04cad-278">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="04cad-278">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  |
| `Vary` | <span data-ttu-id="04cad-279">`Vary` 標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-279">The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="04cad-280">例如，藉由包含 `Vary: Accept-Encoding` 標頭，以編碼方式快取回應，其會快取標頭 `Accept-Encoding: gzip` 和 `Accept-Encoding: text/plain` 的要求回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-280">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="04cad-281">永遠不會儲存標頭值為 `*` 的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-281">A response with a header value of `*` is never stored.</span></span> |
| `Expires` | <span data-ttu-id="04cad-282">除非由其他 `Cache-Control` 標頭覆寫，否則此標頭所視為過時的回應不會儲存或抓取。</span><span class="sxs-lookup"><span data-stu-id="04cad-282">A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> |
| `If-None-Match` | <span data-ttu-id="04cad-283">如果值不 `*`，而且回應的 `ETag` 不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-283">The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="04cad-284">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-284">Otherwise, a 304 (Not Modified) response is served.</span></span> |
| `If-Modified-Since` | <span data-ttu-id="04cad-285">如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-285">If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="04cad-286">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-286">Otherwise, a *304 - Not Modified* response is served.</span></span> |
| `Date` | <span data-ttu-id="04cad-287">從快取提供服務時，如果原始回應未提供 `Date` 標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="04cad-287">When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Content-Length` | <span data-ttu-id="04cad-288">從快取提供服務時，如果原始回應未提供 `Content-Length` 標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="04cad-288">When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> |
| `Age` | <span data-ttu-id="04cad-289">會忽略原始回應中所傳送的 `Age` 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-289">The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="04cad-290">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="04cad-290">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="04cad-291">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="04cad-291">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="04cad-292">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="04cad-292">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="04cad-293">這些規則需要快取以接受用戶端所傳送的有效 `Cache-Control` 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-293">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="04cad-294">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-294">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="04cad-295">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="04cad-295">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="04cad-296">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="04cad-296">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="04cad-297">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="04cad-297">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="04cad-298">疑難排解</span><span class="sxs-lookup"><span data-stu-id="04cad-298">Troubleshooting</span></span>

<span data-ttu-id="04cad-299">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="04cad-299">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="04cad-300">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-300">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="04cad-301">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="04cad-301">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="04cad-302">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-302">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="04cad-303">例如，瀏覽器可能會在重新整理頁面時，將 `Cache-Control` 標頭設定為 `no-cache` 或 `max-age=0`。</span><span class="sxs-lookup"><span data-stu-id="04cad-303">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="04cad-304">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="04cad-304">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="04cad-305">Fiddler</span><span class="sxs-lookup"><span data-stu-id="04cad-305">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="04cad-306">Postman</span><span class="sxs-lookup"><span data-stu-id="04cad-306">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="04cad-307">快取的條件</span><span class="sxs-lookup"><span data-stu-id="04cad-307">Conditions for caching</span></span>

* <span data-ttu-id="04cad-308">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-308">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="04cad-309">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="04cad-309">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="04cad-310">在 `Startup.Configure`中，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="04cad-310">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="04cad-311">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="04cad-311">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="04cad-312">`Authorization` 標頭必須不存在。</span><span class="sxs-lookup"><span data-stu-id="04cad-312">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="04cad-313">`Cache-Control` 標頭參數必須是有效的，而且必須將回應標記為 `public` 而不是標示為 `private`。</span><span class="sxs-lookup"><span data-stu-id="04cad-313">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="04cad-314">如果 `Cache-Control` 標頭不存在，則不能出現 `Pragma: no-cache` 標頭，因為 `Cache-Control` 標頭會在出現時覆寫 `Pragma` 標頭。</span><span class="sxs-lookup"><span data-stu-id="04cad-314">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="04cad-315">`Set-Cookie` 標頭必須不存在。</span><span class="sxs-lookup"><span data-stu-id="04cad-315">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="04cad-316">`Vary` 標頭參數必須有效，而且不等於 `*`。</span><span class="sxs-lookup"><span data-stu-id="04cad-316">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="04cad-317">`Content-Length` 標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="04cad-317">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="04cad-318">不使用 <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>。</span><span class="sxs-lookup"><span data-stu-id="04cad-318">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="04cad-319">回應不得過時，如 `Expires` 標頭和 `max-age` 和 `s-maxage` cache 指示詞所指定。</span><span class="sxs-lookup"><span data-stu-id="04cad-319">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="04cad-320">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="04cad-320">Response buffering must be successful.</span></span> <span data-ttu-id="04cad-321">回應的大小必須小於設定的 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>或預設值。</span><span class="sxs-lookup"><span data-stu-id="04cad-321">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="04cad-322">回應的主體大小必須小於設定的或預設 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>。</span><span class="sxs-lookup"><span data-stu-id="04cad-322">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="04cad-323">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="04cad-323">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="04cad-324">例如，[`no-store`] 指示詞不能存在於 [要求] 或 [回應標頭] 欄位中。</span><span class="sxs-lookup"><span data-stu-id="04cad-324">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="04cad-325">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-325">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="04cad-326">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設定為 `no-cache`，如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="04cad-326">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="04cad-327">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>。</span><span class="sxs-lookup"><span data-stu-id="04cad-327">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="04cad-328">其他資源</span><span class="sxs-lookup"><span data-stu-id="04cad-328">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
