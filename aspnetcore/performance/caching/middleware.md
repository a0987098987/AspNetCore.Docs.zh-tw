---
<span data-ttu-id="c64f2-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-102">'Blazor'</span></span>
- <span data-ttu-id="c64f2-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-103">'Identity'</span></span>
- <span data-ttu-id="c64f2-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-105">'Razor'</span></span>
- <span data-ttu-id="c64f2-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-106">'SignalR' uid:</span></span> 

---
# <a name="response-caching-middleware-in-aspnet-core"></a><span data-ttu-id="c64f2-107">ASP.NET Core 中的回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="c64f2-107">Response Caching Middleware in ASP.NET Core</span></span>

<span data-ttu-id="c64f2-108">作者：[John Luo](https://github.com/JunTaoLuo)</span><span class="sxs-lookup"><span data-stu-id="c64f2-108">By [John Luo](https://github.com/JunTaoLuo)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c64f2-109">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c64f2-109">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="c64f2-110">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-110">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="c64f2-111">如需 HTTP 快取和屬性的簡介 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) ，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="c64f2-111">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="c64f2-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c64f2-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="c64f2-113">組態</span><span class="sxs-lookup"><span data-stu-id="c64f2-113">Configuration</span></span>

<span data-ttu-id="c64f2-114">回應快取中介軟體可透過共用架構隱含地用於 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="c64f2-114">Response Caching Middleware is implicitly available for ASP.NET Core apps via the shared framework.</span></span>

<span data-ttu-id="c64f2-115">在中 `Startup.ConfigureServices` ，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="c64f2-115">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="c64f2-116">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，這會將中介軟體新增至中的要求處理管線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="c64f2-116">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/3.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=16)]

<span data-ttu-id="c64f2-117">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="c64f2-117">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="c64f2-118">[Cache-控制項](https://tools.ietf.org/html/rfc7234#section-5.2)：快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-118">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2): Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="c64f2-119">[不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)：只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會設定中介軟體來提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-119">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4): Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/3.x/AddHeaders.cs)]

<span data-ttu-id="c64f2-120">前面的標頭不會寫入至回應，並會在控制器、動作或頁面上被覆寫 Razor ：</span><span class="sxs-lookup"><span data-stu-id="c64f2-120">The preceding headers are not written to the response and are overridden when a controller, action, or Razor Page:</span></span>

* <span data-ttu-id="c64f2-121">具有[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="c64f2-121">Has a [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute.</span></span> <span data-ttu-id="c64f2-122">即使未設定屬性，這也適用。</span><span class="sxs-lookup"><span data-stu-id="c64f2-122">This applies even if a property isn't set.</span></span> <span data-ttu-id="c64f2-123">例如，省略[VaryByHeader](/aspnet/core/performance/caching/response#vary)屬性將會從回應中移除對應的標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-123">For example, omitting the [VaryByHeader](/aspnet/core/performance/caching/response#vary) property will cause the corresponding header to be removed from the response.</span></span>

<span data-ttu-id="c64f2-124">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-124">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="c64f2-125">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="c64f2-125">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="c64f2-126">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-126">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="c64f2-127">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="c64f2-127">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="c64f2-128">選項</span><span class="sxs-lookup"><span data-stu-id="c64f2-128">Options</span></span>

<span data-ttu-id="c64f2-129">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="c64f2-129">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="c64f2-130">選項</span><span class="sxs-lookup"><span data-stu-id="c64f2-130">Option</span></span> | <span data-ttu-id="c64f2-131">描述</span><span class="sxs-lookup"><span data-stu-id="c64f2-131">Description</span></span> |
| ---
<span data-ttu-id="c64f2-132">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-132">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-133">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-133">'Blazor'</span></span>
- <span data-ttu-id="c64f2-134">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-134">'Identity'</span></span>
- <span data-ttu-id="c64f2-135">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-135">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-136">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-136">'Razor'</span></span>
- <span data-ttu-id="c64f2-137">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-137">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-138">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-138">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-139">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-139">'Blazor'</span></span>
- <span data-ttu-id="c64f2-140">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-140">'Identity'</span></span>
- <span data-ttu-id="c64f2-141">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-141">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-142">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-142">'Razor'</span></span>
- <span data-ttu-id="c64f2-143">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-143">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c64f2-144">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-144">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-145">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-145">'Blazor'</span></span>
- <span data-ttu-id="c64f2-146">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-146">'Identity'</span></span>
- <span data-ttu-id="c64f2-147">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-147">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-148">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-148">'Razor'</span></span>
- <span data-ttu-id="c64f2-149">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-149">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c64f2-150">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-150">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-151">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-151">'Blazor'</span></span>
- <span data-ttu-id="c64f2-152">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-152">'Identity'</span></span>
- <span data-ttu-id="c64f2-153">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-153">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-154">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-154">'Razor'</span></span>
- <span data-ttu-id="c64f2-155">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-155">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-156">------ | |<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> |回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-156">------ | | <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="c64f2-157">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-157">The default value is `64 * 1024 * 1024` (64 MB).</span></span> <span data-ttu-id="c64f2-158">| |<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> |回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-158">| | <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="c64f2-159">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-159">The default value is `100 * 1024 * 1024` (100 MB).</span></span> <span data-ttu-id="c64f2-160">| |<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> |決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-160">| | <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="c64f2-161">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="c64f2-161">The default value is `false`.</span></span> |

<span data-ttu-id="c64f2-162">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="c64f2-162">The following example configures the middleware to:</span></span>

* <span data-ttu-id="c64f2-163">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-163">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="c64f2-164">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="c64f2-164">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="c64f2-165">例如， `/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="c64f2-165">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="c64f2-166">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="c64f2-166">VaryByQueryKeys</span></span>

<span data-ttu-id="c64f2-167">使用 MVC/Web API 控制器或 Razor 頁面模型時，屬性會 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) 指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="c64f2-167">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="c64f2-168">`[ResponseCache]`嚴格需要中介軟體的屬性唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys> ，這並不會對應至實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-168">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="c64f2-169">如需詳細資訊，請參閱<xref:performance/caching/response#responsecache-attribute>。</span><span class="sxs-lookup"><span data-stu-id="c64f2-169">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="c64f2-170">當不使用 `[ResponseCache]` 屬性時，回應快取可以與不同 `VaryByQueryKeys` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-170">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="c64f2-171">直接使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> [HttpCoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)：</span><span class="sxs-lookup"><span data-stu-id="c64f2-171">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="c64f2-172">使用等於 in 的單一值 `*` ，會依 `VaryByQueryKeys` 所有要求查詢參數而改變快取。</span><span class="sxs-lookup"><span data-stu-id="c64f2-172">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="c64f2-173">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="c64f2-173">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="c64f2-174">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="c64f2-174">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="c64f2-175">Header</span><span class="sxs-lookup"><span data-stu-id="c64f2-175">Header</span></span> | <span data-ttu-id="c64f2-176">詳細資料</span><span class="sxs-lookup"><span data-stu-id="c64f2-176">Details</span></span> |
| ---
<span data-ttu-id="c64f2-177">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-177">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-178">'Blazor'</span></span>
- <span data-ttu-id="c64f2-179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-179">'Identity'</span></span>
- <span data-ttu-id="c64f2-180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-180">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-181">'Razor'</span></span>
- <span data-ttu-id="c64f2-182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-182">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-183">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-183">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-184">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-184">'Blazor'</span></span>
- <span data-ttu-id="c64f2-185">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-185">'Identity'</span></span>
- <span data-ttu-id="c64f2-186">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-186">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-187">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-187">'Razor'</span></span>
- <span data-ttu-id="c64f2-188">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-188">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-189">---- | |`Authorization` |如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-189">---- | | `Authorization` | The response isn't cached if the header exists.</span></span> <span data-ttu-id="c64f2-190">| |`Cache-Control` |中介軟體只會考慮以 cache 指示詞標示的快取回應 `public` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-190">| | `Cache-Control` | The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="c64f2-191">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="c64f2-191">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="c64f2-192">最大壽命</span><span class="sxs-lookup"><span data-stu-id="c64f2-192">max-age</span></span></li><li><span data-ttu-id="c64f2-193">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="c64f2-193">max-stale&#8224;</span></span></li><li><span data-ttu-id="c64f2-194">最小-全新</span><span class="sxs-lookup"><span data-stu-id="c64f2-194">min-fresh</span></span></li><li><span data-ttu-id="c64f2-195">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="c64f2-195">must-revalidate</span></span></li><li><span data-ttu-id="c64f2-196">no-cache</span><span class="sxs-lookup"><span data-stu-id="c64f2-196">no-cache</span></span></li><li><span data-ttu-id="c64f2-197">否-存放區</span><span class="sxs-lookup"><span data-stu-id="c64f2-197">no-store</span></span></li><li><span data-ttu-id="c64f2-198">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="c64f2-198">only-if-cached</span></span></li><li><span data-ttu-id="c64f2-199">private</span><span class="sxs-lookup"><span data-stu-id="c64f2-199">private</span></span></li><li><span data-ttu-id="c64f2-200">public</span><span class="sxs-lookup"><span data-stu-id="c64f2-200">public</span></span></li><li><span data-ttu-id="c64f2-201">s-maxage</span><span class="sxs-lookup"><span data-stu-id="c64f2-201">s-maxage</span></span></li><li><span data-ttu-id="c64f2-202">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="c64f2-202">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="c64f2-203">&#8224;如果沒有指定任何限制 `max-stale` ，中介軟體就不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="c64f2-203">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="c64f2-204">&#8225;與 `proxy-revalidate` 具有相同的效果 `must-revalidate` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-204">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="c64f2-205">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="c64f2-205">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> <span data-ttu-id="c64f2-206">| |`Pragma` |`Pragma: no-cache`要求中的標頭會產生與相同的效果 `Cache-Control: no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-206">| | `Pragma` | A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="c64f2-207">此標頭會由標頭中的相關指示詞覆寫 `Cache-Control` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-207">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="c64f2-208">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="c64f2-208">Considered for backward compatibility with HTTP/1.0.</span></span> <span data-ttu-id="c64f2-209">| |`Set-Cookie` |如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-209">| | `Set-Cookie` | The response isn't cached if the header exists.</span></span> <span data-ttu-id="c64f2-210">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-210">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  <span data-ttu-id="c64f2-211">| |`Vary` |`Vary`標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-211">| | `Vary` | The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="c64f2-212">例如，藉由包含標頭的編碼方式來快取回應，這會快取 `Vary: Accept-Encoding` 標頭和個別要求的回應 `Accept-Encoding: gzip` `Accept-Encoding: text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-212">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="c64f2-213">永遠不會儲存標頭值為的回應 `*` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-213">A response with a header value of `*` is never stored.</span></span> <span data-ttu-id="c64f2-214">| |`Expires` |除非其他標頭覆寫，否則不會儲存或抓取此標頭所視為過時的回應 `Cache-Control` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-214">| | `Expires` | A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> <span data-ttu-id="c64f2-215">| |`If-None-Match` |如果值不是 `*` ，而且 `ETag` 回應的不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-215">| | `If-None-Match` | The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="c64f2-216">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-216">Otherwise, a 304 (Not Modified) response is served.</span></span> <span data-ttu-id="c64f2-217">| |`If-Modified-Since` |如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-217">| | `If-Modified-Since` | If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="c64f2-218">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-218">Otherwise, a *304 - Not Modified* response is served.</span></span> <span data-ttu-id="c64f2-219">| |`Date` |從快取提供服務時， `Date` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="c64f2-219">| | `Date` | When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> <span data-ttu-id="c64f2-220">| |`Content-Length` |從快取提供服務時， `Content-Length` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="c64f2-220">| | `Content-Length` | When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> <span data-ttu-id="c64f2-221">| |`Age` |`Age`會忽略原始回應中所傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-221">| | `Age` | The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="c64f2-222">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="c64f2-222">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="c64f2-223">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="c64f2-223">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="c64f2-224">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="c64f2-224">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="c64f2-225">這些規則需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-225">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="c64f2-226">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-226">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="c64f2-227">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="c64f2-227">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="c64f2-228">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="c64f2-228">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="c64f2-229">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="c64f2-229">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="c64f2-230">疑難排解</span><span class="sxs-lookup"><span data-stu-id="c64f2-230">Troubleshooting</span></span>

<span data-ttu-id="c64f2-231">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="c64f2-231">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="c64f2-232">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-232">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="c64f2-233">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="c64f2-233">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="c64f2-234">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-234">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="c64f2-235">例如，瀏覽器可能會在重新整理 `Cache-Control` 頁面時，將標頭設定為 `no-cache` 或 `max-age=0` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-235">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="c64f2-236">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="c64f2-236">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="c64f2-237">Fiddler</span><span class="sxs-lookup"><span data-stu-id="c64f2-237">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="c64f2-238">Postman</span><span class="sxs-lookup"><span data-stu-id="c64f2-238">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="c64f2-239">快取的條件</span><span class="sxs-lookup"><span data-stu-id="c64f2-239">Conditions for caching</span></span>

* <span data-ttu-id="c64f2-240">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-240">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="c64f2-241">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="c64f2-241">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="c64f2-242">在中 `Startup.Configure` ，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="c64f2-242">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="c64f2-243">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="c64f2-243">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="c64f2-244">`Authorization`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="c64f2-244">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="c64f2-245">`Cache-Control`標頭參數必須是有效的，而且回應必須標記為 `public` 且未標記 `private` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-245">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="c64f2-246">`Pragma: no-cache`如果標頭不存在，標頭不能出現 `Cache-Control` ，因為標頭會 `Cache-Control` `Pragma` 在出現時覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-246">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="c64f2-247">`Set-Cookie`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="c64f2-247">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="c64f2-248">`Vary`標頭參數必須有效，而且不等於 `*` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-248">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="c64f2-249">`Content-Length`標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="c64f2-249">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="c64f2-250"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>未使用。</span><span class="sxs-lookup"><span data-stu-id="c64f2-250">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="c64f2-251">回應不得過時，如 `Expires` 標頭和和快取指示詞所指定 `max-age` `s-maxage` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-251">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="c64f2-252">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="c64f2-252">Response buffering must be successful.</span></span> <span data-ttu-id="c64f2-253">回應的大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-253">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="c64f2-254">回應的主體大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-254">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="c64f2-255">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="c64f2-255">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="c64f2-256">例如，指示詞 `no-store` 不能存在於要求或回應標頭欄位中。</span><span class="sxs-lookup"><span data-stu-id="c64f2-256">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="c64f2-257">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-257">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="c64f2-258">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設為， `no-cache` 如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-258">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="c64f2-259">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-259">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c64f2-260">其他資源</span><span class="sxs-lookup"><span data-stu-id="c64f2-260">Additional resources</span></span>

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

<span data-ttu-id="c64f2-261">本文說明如何在 ASP.NET Core 應用程式中設定回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="c64f2-261">This article explains how to configure Response Caching Middleware in an ASP.NET Core app.</span></span> <span data-ttu-id="c64f2-262">中介軟體會決定何時可快取回應、儲存回應，以及提供來自快取的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-262">The middleware determines when responses are cacheable, stores responses, and serves responses from cache.</span></span> <span data-ttu-id="c64f2-263">如需 HTTP 快取和屬性的簡介 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) ，請參閱[回應](xref:performance/caching/response)快取。</span><span class="sxs-lookup"><span data-stu-id="c64f2-263">For an introduction to HTTP caching and the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute, see [Response Caching](xref:performance/caching/response).</span></span>

<span data-ttu-id="c64f2-264">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="c64f2-264">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/middleware/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="configuration"></a><span data-ttu-id="c64f2-265">組態</span><span class="sxs-lookup"><span data-stu-id="c64f2-265">Configuration</span></span>

<span data-ttu-id="c64f2-266">使用[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)，或將套件參考新增至[AspNetCore. ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/)套件。</span><span class="sxs-lookup"><span data-stu-id="c64f2-266">Use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.ResponseCaching](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCaching/) package.</span></span>

<span data-ttu-id="c64f2-267">在中 `Startup.ConfigureServices` ，將回應快取中介軟體新增至服務集合：</span><span class="sxs-lookup"><span data-stu-id="c64f2-267">In `Startup.ConfigureServices`, add the Response Caching Middleware to the service collection:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet1&highlight=3)]

<span data-ttu-id="c64f2-268">將應用程式設定為使用中介軟體搭配 <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> 擴充方法，這會將中介軟體新增至中的要求處理管線 `Startup.Configure` ：</span><span class="sxs-lookup"><span data-stu-id="c64f2-268">Configure the app to use the middleware with the <xref:Microsoft.AspNetCore.Builder.ResponseCachingExtensions.UseResponseCaching*> extension method, which adds the middleware to the request processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](middleware/samples/2.x/ResponseCachingMiddleware/Startup.cs?name=snippet2&highlight=14)]

<span data-ttu-id="c64f2-269">範例應用程式會新增標頭，以在後續要求中控制快取：</span><span class="sxs-lookup"><span data-stu-id="c64f2-269">The sample app adds headers to control caching on subsequent requests:</span></span>

* <span data-ttu-id="c64f2-270">[Cache-控制項](https://tools.ietf.org/html/rfc7234#section-5.2)：快取最多10秒的快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-270">[Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2): Caches cacheable responses for up to 10 seconds.</span></span>
* <span data-ttu-id="c64f2-271">[不同](https://tools.ietf.org/html/rfc7231#section-7.1.4)：只有在後續要求的[接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)標頭符合原始要求的時，才會設定中介軟體來提供快取的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-271">[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4): Configures the middleware to serve a cached response only if the [Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4) header of subsequent requests matches that of the original request.</span></span>

[!code-csharp[](middleware/samples_snippets/2.x/AddHeaders.cs)]

<span data-ttu-id="c64f2-272">前面的標頭不會寫入至回應，並會在控制器、動作或頁面上被覆寫 Razor ：</span><span class="sxs-lookup"><span data-stu-id="c64f2-272">The preceding headers are not written to the response and are overridden when a controller, action, or Razor Page:</span></span>

* <span data-ttu-id="c64f2-273">具有[[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)屬性。</span><span class="sxs-lookup"><span data-stu-id="c64f2-273">Has a [[ResponseCache]](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute.</span></span> <span data-ttu-id="c64f2-274">即使未設定屬性，這也適用。</span><span class="sxs-lookup"><span data-stu-id="c64f2-274">This applies even if a property isn't set.</span></span> <span data-ttu-id="c64f2-275">例如，省略[VaryByHeader](/aspnet/core/performance/caching/response#vary)屬性將會從回應中移除對應的標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-275">For example, omitting the [VaryByHeader](/aspnet/core/performance/caching/response#vary) property will cause the corresponding header to be removed from the response.</span></span>

<span data-ttu-id="c64f2-276">回應快取中介軟體只會快取會產生200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-276">Response Caching Middleware only caches server responses that result in a 200 (OK) status code.</span></span> <span data-ttu-id="c64f2-277">中介軟體會忽略任何其他回應，包括[錯誤頁面](xref:fundamentals/error-handling)。</span><span class="sxs-lookup"><span data-stu-id="c64f2-277">Any other responses, including [error pages](xref:fundamentals/error-handling), are ignored by the middleware.</span></span>

> [!WARNING]
> <span data-ttu-id="c64f2-278">包含已驗證用戶端內容的回應必須標示為無法快取，以防止中介軟體儲存和提供這些回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-278">Responses containing content for authenticated clients must be marked as not cacheable to prevent the middleware from storing and serving those responses.</span></span> <span data-ttu-id="c64f2-279">如需中介軟體如何判斷回應是否可快取的詳細資訊，請參閱快取的[條件](#conditions-for-caching)。</span><span class="sxs-lookup"><span data-stu-id="c64f2-279">See [Conditions for caching](#conditions-for-caching) for details on how the middleware determines if a response is cacheable.</span></span>

## <a name="options"></a><span data-ttu-id="c64f2-280">選項</span><span class="sxs-lookup"><span data-stu-id="c64f2-280">Options</span></span>

<span data-ttu-id="c64f2-281">回應快取選項如下表所示。</span><span class="sxs-lookup"><span data-stu-id="c64f2-281">Response caching options are shown in the following table.</span></span>

| <span data-ttu-id="c64f2-282">選項</span><span class="sxs-lookup"><span data-stu-id="c64f2-282">Option</span></span> | <span data-ttu-id="c64f2-283">描述</span><span class="sxs-lookup"><span data-stu-id="c64f2-283">Description</span></span> |
| ---
<span data-ttu-id="c64f2-284">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-284">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-285">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-285">'Blazor'</span></span>
- <span data-ttu-id="c64f2-286">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-286">'Identity'</span></span>
- <span data-ttu-id="c64f2-287">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-287">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-288">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-288">'Razor'</span></span>
- <span data-ttu-id="c64f2-289">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-289">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-290">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-290">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-291">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-291">'Blazor'</span></span>
- <span data-ttu-id="c64f2-292">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-292">'Identity'</span></span>
- <span data-ttu-id="c64f2-293">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-293">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-294">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-294">'Razor'</span></span>
- <span data-ttu-id="c64f2-295">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-295">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c64f2-296">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-296">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-297">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-297">'Blazor'</span></span>
- <span data-ttu-id="c64f2-298">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-298">'Identity'</span></span>
- <span data-ttu-id="c64f2-299">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-299">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-300">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-300">'Razor'</span></span>
- <span data-ttu-id="c64f2-301">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-301">'SignalR' uid:</span></span> 

-
<span data-ttu-id="c64f2-302">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-302">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-303">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-303">'Blazor'</span></span>
- <span data-ttu-id="c64f2-304">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-304">'Identity'</span></span>
- <span data-ttu-id="c64f2-305">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-305">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-306">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-306">'Razor'</span></span>
- <span data-ttu-id="c64f2-307">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-307">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-308">------ | |<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> |回應主體的最大可快取大小（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-308">------ | | <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> | The largest cacheable size for the response body in bytes.</span></span> <span data-ttu-id="c64f2-309">預設值為 `64 * 1024 * 1024` （64 MB）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-309">The default value is `64 * 1024 * 1024` (64 MB).</span></span> <span data-ttu-id="c64f2-310">| |<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> |回應快取中介軟體的大小限制（以位元組為單位）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-310">| | <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> | The size limit for the response cache middleware in bytes.</span></span> <span data-ttu-id="c64f2-311">預設值為 `100 * 1024 * 1024` （100 MB）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-311">The default value is `100 * 1024 * 1024` (100 MB).</span></span> <span data-ttu-id="c64f2-312">| |<xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> |決定是否在區分大小寫的路徑上快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-312">| | <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.UseCaseSensitivePaths> | Determines if responses are cached on case-sensitive paths.</span></span> <span data-ttu-id="c64f2-313">預設值是 `false`。</span><span class="sxs-lookup"><span data-stu-id="c64f2-313">The default value is `false`.</span></span> |

<span data-ttu-id="c64f2-314">下列範例會將中介軟體設定為：</span><span class="sxs-lookup"><span data-stu-id="c64f2-314">The following example configures the middleware to:</span></span>

* <span data-ttu-id="c64f2-315">具有小於或等於1024位元組之主體大小的快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-315">Cache responses with a body size smaller than or equal to 1,024 bytes.</span></span>
* <span data-ttu-id="c64f2-316">將回應儲存為區分大小寫的路徑。</span><span class="sxs-lookup"><span data-stu-id="c64f2-316">Store the responses by case-sensitive paths.</span></span> <span data-ttu-id="c64f2-317">例如， `/page1` 和 `/Page1` 會分別儲存。</span><span class="sxs-lookup"><span data-stu-id="c64f2-317">For example, `/page1` and `/Page1` are stored separately.</span></span>

```csharp
services.AddResponseCaching(options =>
{
    options.MaximumBodySize = 1024;
    options.UseCaseSensitivePaths = true;
});
```

## <a name="varybyquerykeys"></a><span data-ttu-id="c64f2-318">VaryByQueryKeys</span><span class="sxs-lookup"><span data-stu-id="c64f2-318">VaryByQueryKeys</span></span>

<span data-ttu-id="c64f2-319">使用 MVC/Web API 控制器或 Razor 頁面模型時，屬性會 [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) 指定為回應快取設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="c64f2-319">When using MVC / web API controllers or Razor Pages page models, the [`[ResponseCache]`](xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) attribute specifies the parameters necessary for setting the appropriate headers for response caching.</span></span> <span data-ttu-id="c64f2-320">`[ResponseCache]`嚴格需要中介軟體的屬性唯一參數是 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys> ，這並不會對應至實際的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-320">The only parameter of the `[ResponseCache]` attribute that strictly requires the middleware is <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute.VaryByQueryKeys>, which doesn't correspond to an actual HTTP header.</span></span> <span data-ttu-id="c64f2-321">如需詳細資訊，請參閱<xref:performance/caching/response#responsecache-attribute>。</span><span class="sxs-lookup"><span data-stu-id="c64f2-321">For more information, see <xref:performance/caching/response#responsecache-attribute>.</span></span>

<span data-ttu-id="c64f2-322">當不使用 `[ResponseCache]` 屬性時，回應快取可以與不同 `VaryByQueryKeys` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-322">When not using the `[ResponseCache]` attribute, response caching can be varied with `VaryByQueryKeys`.</span></span> <span data-ttu-id="c64f2-323">直接使用 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> [HttpCoNtext 功能](xref:Microsoft.AspNetCore.Http.HttpContext.Features)：</span><span class="sxs-lookup"><span data-stu-id="c64f2-323">Use the <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingFeature> directly from the [HttpContext.Features](xref:Microsoft.AspNetCore.Http.HttpContext.Features):</span></span>

```csharp
var responseCachingFeature = context.HttpContext.Features.Get<IResponseCachingFeature>();

if (responseCachingFeature != null)
{
    responseCachingFeature.VaryByQueryKeys = new[] { "MyKey" };
}
```

<span data-ttu-id="c64f2-324">使用等於 in 的單一值 `*` ，會依 `VaryByQueryKeys` 所有要求查詢參數而改變快取。</span><span class="sxs-lookup"><span data-stu-id="c64f2-324">Using a single value equal to `*` in `VaryByQueryKeys` varies the cache by all request query parameters.</span></span>

## <a name="http-headers-used-by-response-caching-middleware"></a><span data-ttu-id="c64f2-325">回應快取中介軟體所使用的 HTTP 標頭</span><span class="sxs-lookup"><span data-stu-id="c64f2-325">HTTP headers used by Response Caching Middleware</span></span>

<span data-ttu-id="c64f2-326">下表提供會影響回應快取的 HTTP 標頭資訊。</span><span class="sxs-lookup"><span data-stu-id="c64f2-326">The following table provides information on HTTP headers that affect response caching.</span></span>

| <span data-ttu-id="c64f2-327">Header</span><span class="sxs-lookup"><span data-stu-id="c64f2-327">Header</span></span> | <span data-ttu-id="c64f2-328">詳細資料</span><span class="sxs-lookup"><span data-stu-id="c64f2-328">Details</span></span> |
| ---
<span data-ttu-id="c64f2-329">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-329">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-330">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-330">'Blazor'</span></span>
- <span data-ttu-id="c64f2-331">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-331">'Identity'</span></span>
- <span data-ttu-id="c64f2-332">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-332">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-333">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-333">'Razor'</span></span>
- <span data-ttu-id="c64f2-334">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-334">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-335">--- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="c64f2-335">--- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="c64f2-336">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-336">'Blazor'</span></span>
- <span data-ttu-id="c64f2-337">'Identity'</span><span class="sxs-lookup"><span data-stu-id="c64f2-337">'Identity'</span></span>
- <span data-ttu-id="c64f2-338">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="c64f2-338">'Let's Encrypt'</span></span>
- <span data-ttu-id="c64f2-339">'Razor'</span><span class="sxs-lookup"><span data-stu-id="c64f2-339">'Razor'</span></span>
- <span data-ttu-id="c64f2-340">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="c64f2-340">'SignalR' uid:</span></span> 

<span data-ttu-id="c64f2-341">---- | |`Authorization` |如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-341">---- | | `Authorization` | The response isn't cached if the header exists.</span></span> <span data-ttu-id="c64f2-342">| |`Cache-Control` |中介軟體只會考慮以 cache 指示詞標示的快取回應 `public` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-342">| | `Cache-Control` | The middleware only considers caching responses marked with the `public` cache directive.</span></span> <span data-ttu-id="c64f2-343">使用下列參數來控制快取：</span><span class="sxs-lookup"><span data-stu-id="c64f2-343">Control caching with the following parameters:</span></span><ul><li><span data-ttu-id="c64f2-344">最大壽命</span><span class="sxs-lookup"><span data-stu-id="c64f2-344">max-age</span></span></li><li><span data-ttu-id="c64f2-345">最大-過時&#8224;</span><span class="sxs-lookup"><span data-stu-id="c64f2-345">max-stale&#8224;</span></span></li><li><span data-ttu-id="c64f2-346">最小-全新</span><span class="sxs-lookup"><span data-stu-id="c64f2-346">min-fresh</span></span></li><li><span data-ttu-id="c64f2-347">must-revalidate</span><span class="sxs-lookup"><span data-stu-id="c64f2-347">must-revalidate</span></span></li><li><span data-ttu-id="c64f2-348">no-cache</span><span class="sxs-lookup"><span data-stu-id="c64f2-348">no-cache</span></span></li><li><span data-ttu-id="c64f2-349">否-存放區</span><span class="sxs-lookup"><span data-stu-id="c64f2-349">no-store</span></span></li><li><span data-ttu-id="c64f2-350">僅限-快取</span><span class="sxs-lookup"><span data-stu-id="c64f2-350">only-if-cached</span></span></li><li><span data-ttu-id="c64f2-351">private</span><span class="sxs-lookup"><span data-stu-id="c64f2-351">private</span></span></li><li><span data-ttu-id="c64f2-352">public</span><span class="sxs-lookup"><span data-stu-id="c64f2-352">public</span></span></li><li><span data-ttu-id="c64f2-353">s-maxage</span><span class="sxs-lookup"><span data-stu-id="c64f2-353">s-maxage</span></span></li><li><span data-ttu-id="c64f2-354">proxy-重新驗證&#8225;</span><span class="sxs-lookup"><span data-stu-id="c64f2-354">proxy-revalidate&#8225;</span></span></li></ul><span data-ttu-id="c64f2-355">&#8224;如果沒有指定任何限制 `max-stale` ，中介軟體就不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="c64f2-355">&#8224;If no limit is specified to `max-stale`, the middleware takes no action.</span></span><br><span data-ttu-id="c64f2-356">&#8225;與 `proxy-revalidate` 具有相同的效果 `must-revalidate` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-356">&#8225;`proxy-revalidate` has the same effect as `must-revalidate`.</span></span><br><br><span data-ttu-id="c64f2-357">如需詳細資訊，請參閱[RFC 7231：要求](https://tools.ietf.org/html/rfc7234#section-5.2.1)快取控制指示詞。</span><span class="sxs-lookup"><span data-stu-id="c64f2-357">For more information, see [RFC 7231: Request Cache-Control Directives](https://tools.ietf.org/html/rfc7234#section-5.2.1).</span></span> <span data-ttu-id="c64f2-358">| |`Pragma` |`Pragma: no-cache`要求中的標頭會產生與相同的效果 `Cache-Control: no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-358">| | `Pragma` | A `Pragma: no-cache` header in the request produces the same effect as `Cache-Control: no-cache`.</span></span> <span data-ttu-id="c64f2-359">此標頭會由標頭中的相關指示詞覆寫 `Cache-Control` （如果有的話）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-359">This header is overridden by the relevant directives in the `Cache-Control` header, if present.</span></span> <span data-ttu-id="c64f2-360">視為與 HTTP/1.0 的回溯相容性。</span><span class="sxs-lookup"><span data-stu-id="c64f2-360">Considered for backward compatibility with HTTP/1.0.</span></span> <span data-ttu-id="c64f2-361">| |`Set-Cookie` |如果標頭存在，則不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-361">| | `Set-Cookie` | The response isn't cached if the header exists.</span></span> <span data-ttu-id="c64f2-362">要求處理管線中設定一或多個 cookie 的任何中介軟體，可防止回應快取中介軟體快取回應（例如，以[cookie 為基礎的 TempData 提供者](xref:fundamentals/app-state#tempdata)）。</span><span class="sxs-lookup"><span data-stu-id="c64f2-362">Any middleware in the request processing pipeline that sets one or more cookies prevents the Response Caching Middleware from caching the response (for example, the [cookie-based TempData provider](xref:fundamentals/app-state#tempdata)).</span></span>  <span data-ttu-id="c64f2-363">| |`Vary` |`Vary`標頭是用來依另一個標頭來改變快取的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-363">| | `Vary` | The `Vary` header is used to vary the cached response by another header.</span></span> <span data-ttu-id="c64f2-364">例如，藉由包含標頭的編碼方式來快取回應，這會快取 `Vary: Accept-Encoding` 標頭和個別要求的回應 `Accept-Encoding: gzip` `Accept-Encoding: text/plain` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-364">For example, cache responses by encoding by including the `Vary: Accept-Encoding` header, which caches responses for requests with headers `Accept-Encoding: gzip` and `Accept-Encoding: text/plain` separately.</span></span> <span data-ttu-id="c64f2-365">永遠不會儲存標頭值為的回應 `*` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-365">A response with a header value of `*` is never stored.</span></span> <span data-ttu-id="c64f2-366">| |`Expires` |除非其他標頭覆寫，否則不會儲存或抓取此標頭所視為過時的回應 `Cache-Control` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-366">| | `Expires` | A response deemed stale by this header isn't stored or retrieved unless overridden by other `Cache-Control` headers.</span></span> <span data-ttu-id="c64f2-367">| |`If-None-Match` |如果值不是 `*` ，而且 `ETag` 回應的不符合任何提供的值，則會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-367">| | `If-None-Match` | The full response is served from cache if the value isn't `*` and the `ETag` of the response doesn't match any of the values provided.</span></span> <span data-ttu-id="c64f2-368">否則，會提供304（未修改）的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-368">Otherwise, a 304 (Not Modified) response is served.</span></span> <span data-ttu-id="c64f2-369">| |`If-Modified-Since` |如果 `If-None-Match` 標頭不存在，當快取的回應日期比提供的值還新時，就會從快取中提供完整回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-369">| | `If-Modified-Since` | If the `If-None-Match` header isn't present, a full response is served from cache if the cached response date is newer than the value provided.</span></span> <span data-ttu-id="c64f2-370">否則，會提供*304-未修改*的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-370">Otherwise, a *304 - Not Modified* response is served.</span></span> <span data-ttu-id="c64f2-371">| |`Date` |從快取提供服務時， `Date` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="c64f2-371">| | `Date` | When serving from cache, the `Date` header is set by the middleware if it wasn't provided on the original response.</span></span> <span data-ttu-id="c64f2-372">| |`Content-Length` |從快取提供服務時， `Content-Length` 如果原始回應未提供標頭，則會由中介軟體設定。</span><span class="sxs-lookup"><span data-stu-id="c64f2-372">| | `Content-Length` | When serving from cache, the `Content-Length` header is set by the middleware if it wasn't provided on the original response.</span></span> <span data-ttu-id="c64f2-373">| |`Age` |`Age`會忽略原始回應中所傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-373">| | `Age` | The `Age` header sent in the original response is ignored.</span></span> <span data-ttu-id="c64f2-374">中介軟體會在服務快取回應時計算新的值。</span><span class="sxs-lookup"><span data-stu-id="c64f2-374">The middleware computes a new value when serving a cached response.</span></span> |

## <a name="caching-respects-request-cache-control-directives"></a><span data-ttu-id="c64f2-375">快取遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="c64f2-375">Caching respects request Cache-Control directives</span></span>

<span data-ttu-id="c64f2-376">中介軟體會遵循[HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格的規則。</span><span class="sxs-lookup"><span data-stu-id="c64f2-376">The middleware respects the rules of the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234#section-5.2).</span></span> <span data-ttu-id="c64f2-377">這些規則需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-377">The rules require a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="c64f2-378">在規格底下，用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-378">Under the specification, a client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span> <span data-ttu-id="c64f2-379">目前，使用中介軟體時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="c64f2-379">Currently, there's no developer control over this caching behavior when using the middleware because the middleware adheres to the official caching specification.</span></span>

<span data-ttu-id="c64f2-380">若要更充分掌控快取行為，請探索 ASP.NET Core 的其他快取功能。</span><span class="sxs-lookup"><span data-stu-id="c64f2-380">For more control over caching behavior, explore other caching features of ASP.NET Core.</span></span> <span data-ttu-id="c64f2-381">請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="c64f2-381">See the following topics:</span></span>

* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

## <a name="troubleshooting"></a><span data-ttu-id="c64f2-382">疑難排解</span><span class="sxs-lookup"><span data-stu-id="c64f2-382">Troubleshooting</span></span>

<span data-ttu-id="c64f2-383">如果快取行為不符合預期，請確認回應可快取且能夠從快取中提供服務。</span><span class="sxs-lookup"><span data-stu-id="c64f2-383">If caching behavior isn't as expected, confirm that responses are cacheable and capable of being served from the cache.</span></span> <span data-ttu-id="c64f2-384">檢查要求的傳入標頭和回應的傳出標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-384">Examine the request's incoming headers and the response's outgoing headers.</span></span> <span data-ttu-id="c64f2-385">啟用[記錄功能](xref:fundamentals/logging/index)以協助進行調試。</span><span class="sxs-lookup"><span data-stu-id="c64f2-385">Enable [logging](xref:fundamentals/logging/index) to help with debugging.</span></span>

<span data-ttu-id="c64f2-386">在測試和疑難排解快取行為時，瀏覽器可能會以不必要的方式設定會影響快取的要求標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-386">When testing and troubleshooting caching behavior, a browser may set request headers that affect caching in undesirable ways.</span></span> <span data-ttu-id="c64f2-387">例如，瀏覽器可能會在重新整理 `Cache-Control` 頁面時，將標頭設定為 `no-cache` 或 `max-age=0` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-387">For example, a browser may set the `Cache-Control` header to `no-cache` or `max-age=0` when refreshing a page.</span></span> <span data-ttu-id="c64f2-388">下列工具可以明確設定要求標頭，並慣用來測試快取：</span><span class="sxs-lookup"><span data-stu-id="c64f2-388">The following tools can explicitly set request headers and are preferred for testing caching:</span></span>

* [<span data-ttu-id="c64f2-389">Fiddler</span><span class="sxs-lookup"><span data-stu-id="c64f2-389">Fiddler</span></span>](https://www.telerik.com/fiddler)
* [<span data-ttu-id="c64f2-390">Postman</span><span class="sxs-lookup"><span data-stu-id="c64f2-390">Postman</span></span>](https://www.getpostman.com/)

### <a name="conditions-for-caching"></a><span data-ttu-id="c64f2-391">快取的條件</span><span class="sxs-lookup"><span data-stu-id="c64f2-391">Conditions for caching</span></span>

* <span data-ttu-id="c64f2-392">要求必須產生具有200（確定）狀態碼的伺服器回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-392">The request must result in a server response with a 200 (OK) status code.</span></span>
* <span data-ttu-id="c64f2-393">要求方法必須是 GET 或 HEAD。</span><span class="sxs-lookup"><span data-stu-id="c64f2-393">The request method must be GET or HEAD.</span></span>
* <span data-ttu-id="c64f2-394">在中 `Startup.Configure` ，回應快取中介軟體必須放在需要快取的中介軟體前面。</span><span class="sxs-lookup"><span data-stu-id="c64f2-394">In `Startup.Configure`, Response Caching Middleware must be placed before middleware that require caching.</span></span> <span data-ttu-id="c64f2-395">如需詳細資訊，請參閱<xref:fundamentals/middleware/index>。</span><span class="sxs-lookup"><span data-stu-id="c64f2-395">For more information, see <xref:fundamentals/middleware/index>.</span></span>
* <span data-ttu-id="c64f2-396">`Authorization`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="c64f2-396">The `Authorization` header must not be present.</span></span>
* <span data-ttu-id="c64f2-397">`Cache-Control`標頭參數必須是有效的，而且回應必須標記為 `public` 且未標記 `private` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-397">`Cache-Control` header parameters must be valid, and the response must be marked `public` and not marked `private`.</span></span>
* <span data-ttu-id="c64f2-398">`Pragma: no-cache`如果標頭不存在，標頭不能出現 `Cache-Control` ，因為標頭會 `Cache-Control` `Pragma` 在出現時覆寫標頭。</span><span class="sxs-lookup"><span data-stu-id="c64f2-398">The `Pragma: no-cache` header must not be present if the `Cache-Control` header isn't present, as the `Cache-Control` header overrides the `Pragma` header when present.</span></span>
* <span data-ttu-id="c64f2-399">`Set-Cookie`標頭不得存在。</span><span class="sxs-lookup"><span data-stu-id="c64f2-399">The `Set-Cookie` header must not be present.</span></span>
* <span data-ttu-id="c64f2-400">`Vary`標頭參數必須有效，而且不等於 `*` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-400">`Vary` header parameters must be valid and not equal to `*`.</span></span>
* <span data-ttu-id="c64f2-401">`Content-Length`標頭值（如果設定）必須符合回應主體的大小。</span><span class="sxs-lookup"><span data-stu-id="c64f2-401">The `Content-Length` header value (if set) must match the size of the response body.</span></span>
* <span data-ttu-id="c64f2-402"><xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature>未使用。</span><span class="sxs-lookup"><span data-stu-id="c64f2-402">The <xref:Microsoft.AspNetCore.Http.Features.IHttpSendFileFeature> isn't used.</span></span>
* <span data-ttu-id="c64f2-403">回應不得過時，如 `Expires` 標頭和和快取指示詞所指定 `max-age` `s-maxage` 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-403">The response must not be stale as specified by the `Expires` header and the `max-age` and `s-maxage` cache directives.</span></span>
* <span data-ttu-id="c64f2-404">回應緩衝處理必須成功。</span><span class="sxs-lookup"><span data-stu-id="c64f2-404">Response buffering must be successful.</span></span> <span data-ttu-id="c64f2-405">回應的大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit> 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-405">The size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.SizeLimit>.</span></span> <span data-ttu-id="c64f2-406">回應的主體大小必須小於已設定或預設值 <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize> 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-406">The body size of the response must be smaller than the configured or default <xref:Microsoft.AspNetCore.ResponseCaching.ResponseCachingOptions.MaximumBodySize>.</span></span>
* <span data-ttu-id="c64f2-407">回應必須根據[RFC 7234](https://tools.ietf.org/html/rfc7234)規格進行快取。</span><span class="sxs-lookup"><span data-stu-id="c64f2-407">The response must be cacheable according to the [RFC 7234](https://tools.ietf.org/html/rfc7234) specifications.</span></span> <span data-ttu-id="c64f2-408">例如，指示詞 `no-store` 不能存在於要求或回應標頭欄位中。</span><span class="sxs-lookup"><span data-stu-id="c64f2-408">For example, the `no-store` directive must not exist in request or response header fields.</span></span> <span data-ttu-id="c64f2-409">如需詳細資訊，請參閱*第3節：在* [RFC 7234](https://tools.ietf.org/html/rfc7234)的快取中儲存回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-409">See *Section 3: Storing Responses in Caches* of [RFC 7234](https://tools.ietf.org/html/rfc7234) for details.</span></span>

> [!NOTE]
> <span data-ttu-id="c64f2-410">用來產生安全權杖以防止跨網站偽造要求（CSRF）攻擊的 Antiforgery 系統會將 `Cache-Control` 和 `Pragma` 標頭設為， `no-cache` 如此就不會快取回應。</span><span class="sxs-lookup"><span data-stu-id="c64f2-410">The Antiforgery system for generating secure tokens to prevent Cross-Site Request Forgery (CSRF) attacks sets the `Cache-Control` and `Pragma` headers to `no-cache` so that responses aren't cached.</span></span> <span data-ttu-id="c64f2-411">如需如何停用 HTML 表單專案之 antiforgery token 的詳細資訊，請參閱 <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration> 。</span><span class="sxs-lookup"><span data-stu-id="c64f2-411">For information on how to disable antiforgery tokens for HTML form elements, see <xref:security/anti-request-forgery#aspnet-core-antiforgery-configuration>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c64f2-412">其他資源</span><span class="sxs-lookup"><span data-stu-id="c64f2-412">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/response>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>

::: moniker-end
