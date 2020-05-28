---
<span data-ttu-id="34b9b-101">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-101">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-102">'Blazor'</span></span>
- <span data-ttu-id="34b9b-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-103">'Identity'</span></span>
- <span data-ttu-id="34b9b-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-105">'Razor'</span></span>
- <span data-ttu-id="34b9b-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-106">'SignalR' uid:</span></span> 

---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="34b9b-107">ASP.NET Core 中的回應快取</span><span class="sxs-lookup"><span data-stu-id="34b9b-107">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="34b9b-108">作者： [John 羅文](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="34b9b-108">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="34b9b-109">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="34b9b-109">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="34b9b-110">回應快取可減少用戶端或 proxy 對 web 伺服器提出的要求數目。</span><span class="sxs-lookup"><span data-stu-id="34b9b-110">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="34b9b-111">回應快取也會減少 web 伺服器執行以產生回應的工作量。</span><span class="sxs-lookup"><span data-stu-id="34b9b-111">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="34b9b-112">回應快取是由標頭控制，可指定您希望用戶端、proxy 和中介軟體快取回應的方式。</span><span class="sxs-lookup"><span data-stu-id="34b9b-112">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="34b9b-113">[ResponseCache 屬性](#responsecache-attribute)會參與設定回應快取標頭。</span><span class="sxs-lookup"><span data-stu-id="34b9b-113">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers.</span></span> <span data-ttu-id="34b9b-114">用戶端和中繼 proxy 應接受[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格底下快取回應的標頭。</span><span class="sxs-lookup"><span data-stu-id="34b9b-114">Clients and intermediate proxies should honor the headers for caching responses under the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234).</span></span>

<span data-ttu-id="34b9b-115">若為遵循 HTTP 1.1 快取規格的伺服器端快取，請使用回應快取[中介軟體](xref:performance/caching/middleware)。</span><span class="sxs-lookup"><span data-stu-id="34b9b-115">For server-side caching that follows the HTTP 1.1 Caching specification, use [Response Caching Middleware](xref:performance/caching/middleware).</span></span> <span data-ttu-id="34b9b-116">中介軟體可以使用 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 屬性來影響伺服器端快取行為。</span><span class="sxs-lookup"><span data-stu-id="34b9b-116">The middleware can use the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="34b9b-117">以 HTTP 為基礎的回應快取</span><span class="sxs-lookup"><span data-stu-id="34b9b-117">HTTP-based response caching</span></span>

<span data-ttu-id="34b9b-118">[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格描述網際網路快取的行為。</span><span class="sxs-lookup"><span data-stu-id="34b9b-118">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="34b9b-119">用於快取的主要 HTTP 標頭是快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)，用來指定快取指示*詞。*</span><span class="sxs-lookup"><span data-stu-id="34b9b-119">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="34b9b-120">指示詞會控制快取行為，因為要求會從用戶端傳送到伺服器，而回應會從伺服器傳回用戶端。</span><span class="sxs-lookup"><span data-stu-id="34b9b-120">The directives control caching behavior as requests make their way from clients to servers and as responses make their way from servers back to clients.</span></span> <span data-ttu-id="34b9b-121">要求和回應會在 proxy 伺服器上移動，而 proxy 伺服器也必須符合 HTTP 1.1 快取規格。</span><span class="sxs-lookup"><span data-stu-id="34b9b-121">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="34b9b-122">通用指示詞如下 `Cache-Control` 表所示。</span><span class="sxs-lookup"><span data-stu-id="34b9b-122">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="34b9b-123">指示詞</span><span class="sxs-lookup"><span data-stu-id="34b9b-123">Directive</span></span>                                                       | <span data-ttu-id="34b9b-124">動作</span><span class="sxs-lookup"><span data-stu-id="34b9b-124">Action</span></span> |
| ---
<span data-ttu-id="34b9b-125">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-125">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-126">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-126">'Blazor'</span></span>
- <span data-ttu-id="34b9b-127">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-127">'Identity'</span></span>
- <span data-ttu-id="34b9b-128">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-128">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-129">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-129">'Razor'</span></span>
- <span data-ttu-id="34b9b-130">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-130">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-131">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-131">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-132">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-132">'Blazor'</span></span>
- <span data-ttu-id="34b9b-133">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-133">'Identity'</span></span>
- <span data-ttu-id="34b9b-134">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-134">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-135">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-135">'Razor'</span></span>
- <span data-ttu-id="34b9b-136">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-136">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-137">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-137">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-138">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-138">'Blazor'</span></span>
- <span data-ttu-id="34b9b-139">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-139">'Identity'</span></span>
- <span data-ttu-id="34b9b-140">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-140">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-141">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-141">'Razor'</span></span>
- <span data-ttu-id="34b9b-142">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-142">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-143">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-143">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-144">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-144">'Blazor'</span></span>
- <span data-ttu-id="34b9b-145">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-145">'Identity'</span></span>
- <span data-ttu-id="34b9b-146">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-146">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-147">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-147">'Razor'</span></span>
- <span data-ttu-id="34b9b-148">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-148">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-149">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-149">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-150">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-150">'Blazor'</span></span>
- <span data-ttu-id="34b9b-151">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-151">'Identity'</span></span>
- <span data-ttu-id="34b9b-152">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-152">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-153">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-153">'Razor'</span></span>
- <span data-ttu-id="34b9b-154">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-154">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-155">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-155">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-156">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-156">'Blazor'</span></span>
- <span data-ttu-id="34b9b-157">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-157">'Identity'</span></span>
- <span data-ttu-id="34b9b-158">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-158">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-159">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-159">'Razor'</span></span>
- <span data-ttu-id="34b9b-160">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-160">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-161">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-161">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-162">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-162">'Blazor'</span></span>
- <span data-ttu-id="34b9b-163">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-163">'Identity'</span></span>
- <span data-ttu-id="34b9b-164">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-164">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-165">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-165">'Razor'</span></span>
- <span data-ttu-id="34b9b-166">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-166">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-167">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-167">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-168">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-168">'Blazor'</span></span>
- <span data-ttu-id="34b9b-169">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-169">'Identity'</span></span>
- <span data-ttu-id="34b9b-170">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-170">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-171">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-171">'Razor'</span></span>
- <span data-ttu-id="34b9b-172">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-172">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-173">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-173">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-174">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-174">'Blazor'</span></span>
- <span data-ttu-id="34b9b-175">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-175">'Identity'</span></span>
- <span data-ttu-id="34b9b-176">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-176">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-177">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-177">'Razor'</span></span>
- <span data-ttu-id="34b9b-178">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-178">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-179">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-179">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-180">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-180">'Blazor'</span></span>
- <span data-ttu-id="34b9b-181">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-181">'Identity'</span></span>
- <span data-ttu-id="34b9b-182">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-182">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-183">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-183">'Razor'</span></span>
- <span data-ttu-id="34b9b-184">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-184">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-185">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-185">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-186">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-186">'Blazor'</span></span>
- <span data-ttu-id="34b9b-187">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-187">'Identity'</span></span>
- <span data-ttu-id="34b9b-188">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-188">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-189">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-189">'Razor'</span></span>
- <span data-ttu-id="34b9b-190">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-190">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-191">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-191">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-192">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-192">'Blazor'</span></span>
- <span data-ttu-id="34b9b-193">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-193">'Identity'</span></span>
- <span data-ttu-id="34b9b-194">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-194">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-195">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-195">'Razor'</span></span>
- <span data-ttu-id="34b9b-196">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-196">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-197">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-197">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-198">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-198">'Blazor'</span></span>
- <span data-ttu-id="34b9b-199">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-199">'Identity'</span></span>
- <span data-ttu-id="34b9b-200">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-200">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-201">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-201">'Razor'</span></span>
- <span data-ttu-id="34b9b-202">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-202">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-203">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-203">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-204">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-204">'Blazor'</span></span>
- <span data-ttu-id="34b9b-205">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-205">'Identity'</span></span>
- <span data-ttu-id="34b9b-206">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-206">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-207">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-207">'Razor'</span></span>
- <span data-ttu-id="34b9b-208">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-208">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-209">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-209">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-210">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-210">'Blazor'</span></span>
- <span data-ttu-id="34b9b-211">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-211">'Identity'</span></span>
- <span data-ttu-id="34b9b-212">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-212">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-213">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-213">'Razor'</span></span>
- <span data-ttu-id="34b9b-214">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-214">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-215">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-215">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-216">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-216">'Blazor'</span></span>
- <span data-ttu-id="34b9b-217">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-217">'Identity'</span></span>
- <span data-ttu-id="34b9b-218">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-218">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-219">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-219">'Razor'</span></span>
- <span data-ttu-id="34b9b-220">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-220">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-221">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-221">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-222">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-222">'Blazor'</span></span>
- <span data-ttu-id="34b9b-223">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-223">'Identity'</span></span>
- <span data-ttu-id="34b9b-224">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-224">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-225">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-225">'Razor'</span></span>
- <span data-ttu-id="34b9b-226">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-226">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-227">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-227">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-228">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-228">'Blazor'</span></span>
- <span data-ttu-id="34b9b-229">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-229">'Identity'</span></span>
- <span data-ttu-id="34b9b-230">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-230">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-231">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-231">'Razor'</span></span>
- <span data-ttu-id="34b9b-232">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-232">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-233">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-233">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-234">'Blazor'</span></span>
- <span data-ttu-id="34b9b-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-235">'Identity'</span></span>
- <span data-ttu-id="34b9b-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-237">'Razor'</span></span>
- <span data-ttu-id="34b9b-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-238">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-239">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-239">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-240">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-240">'Blazor'</span></span>
- <span data-ttu-id="34b9b-241">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-241">'Identity'</span></span>
- <span data-ttu-id="34b9b-242">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-242">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-243">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-243">'Razor'</span></span>
- <span data-ttu-id="34b9b-244">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-244">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-245">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-245">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-246">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-246">'Blazor'</span></span>
- <span data-ttu-id="34b9b-247">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-247">'Identity'</span></span>
- <span data-ttu-id="34b9b-248">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-248">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-249">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-249">'Razor'</span></span>
- <span data-ttu-id="34b9b-250">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-250">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-251">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-251">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-252">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-252">'Blazor'</span></span>
- <span data-ttu-id="34b9b-253">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-253">'Identity'</span></span>
- <span data-ttu-id="34b9b-254">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-254">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-255">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-255">'Razor'</span></span>
- <span data-ttu-id="34b9b-256">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-256">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-257">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-257">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-258">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-258">'Blazor'</span></span>
- <span data-ttu-id="34b9b-259">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-259">'Identity'</span></span>
- <span data-ttu-id="34b9b-260">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-260">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-261">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-261">'Razor'</span></span>
- <span data-ttu-id="34b9b-262">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-262">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-263">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-263">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-264">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-264">'Blazor'</span></span>
- <span data-ttu-id="34b9b-265">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-265">'Identity'</span></span>
- <span data-ttu-id="34b9b-266">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-266">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-267">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-267">'Razor'</span></span>
- <span data-ttu-id="34b9b-268">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-268">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-269">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-269">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-270">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-270">'Blazor'</span></span>
- <span data-ttu-id="34b9b-271">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-271">'Identity'</span></span>
- <span data-ttu-id="34b9b-272">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-272">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-273">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-273">'Razor'</span></span>
- <span data-ttu-id="34b9b-274">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-274">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-275">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-275">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-276">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-276">'Blazor'</span></span>
- <span data-ttu-id="34b9b-277">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-277">'Identity'</span></span>
- <span data-ttu-id="34b9b-278">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-278">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-279">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-279">'Razor'</span></span>
- <span data-ttu-id="34b9b-280">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-280">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-281">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-281">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-282">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-282">'Blazor'</span></span>
- <span data-ttu-id="34b9b-283">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-283">'Identity'</span></span>
- <span data-ttu-id="34b9b-284">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-284">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-285">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-285">'Razor'</span></span>
- <span data-ttu-id="34b9b-286">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-286">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-287">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-287">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-288">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-288">'Blazor'</span></span>
- <span data-ttu-id="34b9b-289">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-289">'Identity'</span></span>
- <span data-ttu-id="34b9b-290">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-290">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-291">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-291">'Razor'</span></span>
- <span data-ttu-id="34b9b-292">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-292">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-293">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-293">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-294">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-294">'Blazor'</span></span>
- <span data-ttu-id="34b9b-295">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-295">'Identity'</span></span>
- <span data-ttu-id="34b9b-296">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-296">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-297">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-297">'Razor'</span></span>
- <span data-ttu-id="34b9b-298">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-298">'SignalR' uid:</span></span> 

<span data-ttu-id="34b9b-299">-------------------------------- |---標題： author： description： monikerRange： ms-chap： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-299">-------------------------------- | --- title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-300">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-300">'Blazor'</span></span>
- <span data-ttu-id="34b9b-301">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-301">'Identity'</span></span>
- <span data-ttu-id="34b9b-302">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-302">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-303">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-303">'Razor'</span></span>
- <span data-ttu-id="34b9b-304">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-304">'SignalR' uid:</span></span> 

<span data-ttu-id="34b9b-305">--- | |[公用](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)|快取可能會儲存回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-305">--- | | [public](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | A cache may store the response.</span></span> <span data-ttu-id="34b9b-306">| |[私](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)用 |回應不得由共用快取儲存。</span><span class="sxs-lookup"><span data-stu-id="34b9b-306">| | [private](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | The response must not be stored by a shared cache.</span></span> <span data-ttu-id="34b9b-307">私用快取可能會儲存並重複使用回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-307">A private cache may store and reuse the response.</span></span> <span data-ttu-id="34b9b-308">| |[最大壽命](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)|用戶端不接受其年齡大於指定秒數的回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-308">| | [max-age](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | The client doesn't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="34b9b-309">範例： `max-age=60` （60秒）、 `max-age=2592000` （1個月） | |[無-](https://tools.ietf.org/html/rfc7234#section-5.2.1.4)  |  快取**在要求上**：快取不得使用預存回應來滿足要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-309">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month) | | [no-cache](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | **On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="34b9b-310">源伺服器會重新產生用戶端的回應，中介軟體會在其快取中更新儲存的回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-310">The origin server regenerates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="34b9b-311">**回應時**：回應不得用於後續要求，而不需在源伺服器上進行驗證。</span><span class="sxs-lookup"><span data-stu-id="34b9b-311">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> <span data-ttu-id="34b9b-312">| |[否-存放區](https://tools.ietf.org/html/rfc7234#section-5.2.1.5)  | **在要求上**：快取不得儲存要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-312">| | [no-store](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | **On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="34b9b-313">**回應時**：快取不得儲存回應的任何部分。</span><span class="sxs-lookup"><span data-stu-id="34b9b-313">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="34b9b-314">下表顯示在快取中扮演角色的其他快取標頭。</span><span class="sxs-lookup"><span data-stu-id="34b9b-314">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="34b9b-315">Header</span><span class="sxs-lookup"><span data-stu-id="34b9b-315">Header</span></span>                                                     | <span data-ttu-id="34b9b-316">函式</span><span class="sxs-lookup"><span data-stu-id="34b9b-316">Function</span></span> |
| ---
<span data-ttu-id="34b9b-317">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-317">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-318">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-318">'Blazor'</span></span>
- <span data-ttu-id="34b9b-319">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-319">'Identity'</span></span>
- <span data-ttu-id="34b9b-320">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-320">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-321">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-321">'Razor'</span></span>
- <span data-ttu-id="34b9b-322">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-322">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-323">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-323">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-324">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-324">'Blazor'</span></span>
- <span data-ttu-id="34b9b-325">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-325">'Identity'</span></span>
- <span data-ttu-id="34b9b-326">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-326">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-327">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-327">'Razor'</span></span>
- <span data-ttu-id="34b9b-328">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-328">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-329">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-329">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-330">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-330">'Blazor'</span></span>
- <span data-ttu-id="34b9b-331">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-331">'Identity'</span></span>
- <span data-ttu-id="34b9b-332">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-332">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-333">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-333">'Razor'</span></span>
- <span data-ttu-id="34b9b-334">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-334">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-335">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-335">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-336">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-336">'Blazor'</span></span>
- <span data-ttu-id="34b9b-337">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-337">'Identity'</span></span>
- <span data-ttu-id="34b9b-338">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-338">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-339">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-339">'Razor'</span></span>
- <span data-ttu-id="34b9b-340">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-340">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-341">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-341">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-342">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-342">'Blazor'</span></span>
- <span data-ttu-id="34b9b-343">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-343">'Identity'</span></span>
- <span data-ttu-id="34b9b-344">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-344">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-345">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-345">'Razor'</span></span>
- <span data-ttu-id="34b9b-346">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-346">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-347">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-347">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-348">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-348">'Blazor'</span></span>
- <span data-ttu-id="34b9b-349">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-349">'Identity'</span></span>
- <span data-ttu-id="34b9b-350">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-350">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-351">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-351">'Razor'</span></span>
- <span data-ttu-id="34b9b-352">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-352">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-353">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-353">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-354">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-354">'Blazor'</span></span>
- <span data-ttu-id="34b9b-355">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-355">'Identity'</span></span>
- <span data-ttu-id="34b9b-356">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-356">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-357">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-357">'Razor'</span></span>
- <span data-ttu-id="34b9b-358">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-358">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-359">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-359">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-360">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-360">'Blazor'</span></span>
- <span data-ttu-id="34b9b-361">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-361">'Identity'</span></span>
- <span data-ttu-id="34b9b-362">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-362">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-363">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-363">'Razor'</span></span>
- <span data-ttu-id="34b9b-364">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-364">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-365">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-365">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-366">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-366">'Blazor'</span></span>
- <span data-ttu-id="34b9b-367">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-367">'Identity'</span></span>
- <span data-ttu-id="34b9b-368">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-368">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-369">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-369">'Razor'</span></span>
- <span data-ttu-id="34b9b-370">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-370">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-371">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-371">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-372">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-372">'Blazor'</span></span>
- <span data-ttu-id="34b9b-373">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-373">'Identity'</span></span>
- <span data-ttu-id="34b9b-374">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-374">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-375">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-375">'Razor'</span></span>
- <span data-ttu-id="34b9b-376">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-376">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-377">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-377">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-378">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-378">'Blazor'</span></span>
- <span data-ttu-id="34b9b-379">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-379">'Identity'</span></span>
- <span data-ttu-id="34b9b-380">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-380">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-381">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-381">'Razor'</span></span>
- <span data-ttu-id="34b9b-382">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-382">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-383">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-383">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-384">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-384">'Blazor'</span></span>
- <span data-ttu-id="34b9b-385">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-385">'Identity'</span></span>
- <span data-ttu-id="34b9b-386">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-386">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-387">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-387">'Razor'</span></span>
- <span data-ttu-id="34b9b-388">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-388">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-389">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-389">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-390">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-390">'Blazor'</span></span>
- <span data-ttu-id="34b9b-391">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-391">'Identity'</span></span>
- <span data-ttu-id="34b9b-392">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-392">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-393">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-393">'Razor'</span></span>
- <span data-ttu-id="34b9b-394">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-394">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-395">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-395">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-396">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-396">'Blazor'</span></span>
- <span data-ttu-id="34b9b-397">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-397">'Identity'</span></span>
- <span data-ttu-id="34b9b-398">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-398">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-399">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-399">'Razor'</span></span>
- <span data-ttu-id="34b9b-400">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-400">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-401">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-401">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-402">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-402">'Blazor'</span></span>
- <span data-ttu-id="34b9b-403">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-403">'Identity'</span></span>
- <span data-ttu-id="34b9b-404">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-404">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-405">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-405">'Razor'</span></span>
- <span data-ttu-id="34b9b-406">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-406">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-407">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-407">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-408">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-408">'Blazor'</span></span>
- <span data-ttu-id="34b9b-409">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-409">'Identity'</span></span>
- <span data-ttu-id="34b9b-410">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-410">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-411">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-411">'Razor'</span></span>
- <span data-ttu-id="34b9b-412">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-412">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-413">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-413">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-414">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-414">'Blazor'</span></span>
- <span data-ttu-id="34b9b-415">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-415">'Identity'</span></span>
- <span data-ttu-id="34b9b-416">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-416">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-417">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-417">'Razor'</span></span>
- <span data-ttu-id="34b9b-418">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-418">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-419">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-419">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-420">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-420">'Blazor'</span></span>
- <span data-ttu-id="34b9b-421">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-421">'Identity'</span></span>
- <span data-ttu-id="34b9b-422">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-422">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-423">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-423">'Razor'</span></span>
- <span data-ttu-id="34b9b-424">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-424">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-425">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-425">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-426">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-426">'Blazor'</span></span>
- <span data-ttu-id="34b9b-427">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-427">'Identity'</span></span>
- <span data-ttu-id="34b9b-428">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-428">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-429">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-429">'Razor'</span></span>
- <span data-ttu-id="34b9b-430">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-430">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-431">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-431">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-432">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-432">'Blazor'</span></span>
- <span data-ttu-id="34b9b-433">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-433">'Identity'</span></span>
- <span data-ttu-id="34b9b-434">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-434">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-435">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-435">'Razor'</span></span>
- <span data-ttu-id="34b9b-436">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-436">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-437">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-437">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-438">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-438">'Blazor'</span></span>
- <span data-ttu-id="34b9b-439">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-439">'Identity'</span></span>
- <span data-ttu-id="34b9b-440">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-440">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-441">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-441">'Razor'</span></span>
- <span data-ttu-id="34b9b-442">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-442">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-443">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-443">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-444">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-444">'Blazor'</span></span>
- <span data-ttu-id="34b9b-445">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-445">'Identity'</span></span>
- <span data-ttu-id="34b9b-446">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-446">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-447">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-447">'Razor'</span></span>
- <span data-ttu-id="34b9b-448">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-448">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-449">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-449">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-450">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-450">'Blazor'</span></span>
- <span data-ttu-id="34b9b-451">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-451">'Identity'</span></span>
- <span data-ttu-id="34b9b-452">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-452">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-453">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-453">'Razor'</span></span>
- <span data-ttu-id="34b9b-454">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-454">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-455">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-455">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-456">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-456">'Blazor'</span></span>
- <span data-ttu-id="34b9b-457">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-457">'Identity'</span></span>
- <span data-ttu-id="34b9b-458">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-458">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-459">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-459">'Razor'</span></span>
- <span data-ttu-id="34b9b-460">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-460">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-461">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-461">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-462">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-462">'Blazor'</span></span>
- <span data-ttu-id="34b9b-463">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-463">'Identity'</span></span>
- <span data-ttu-id="34b9b-464">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-464">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-465">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-465">'Razor'</span></span>
- <span data-ttu-id="34b9b-466">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-466">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-467">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-467">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-468">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-468">'Blazor'</span></span>
- <span data-ttu-id="34b9b-469">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-469">'Identity'</span></span>
- <span data-ttu-id="34b9b-470">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-470">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-471">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-471">'Razor'</span></span>
- <span data-ttu-id="34b9b-472">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-472">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-473">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-473">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-474">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-474">'Blazor'</span></span>
- <span data-ttu-id="34b9b-475">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-475">'Identity'</span></span>
- <span data-ttu-id="34b9b-476">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-476">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-477">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-477">'Razor'</span></span>
- <span data-ttu-id="34b9b-478">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-478">'SignalR' uid:</span></span> 

<span data-ttu-id="34b9b-479">----------------------------- |---標題： author： description： monikerRange： ms-chap： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-479">----------------------------- | --- title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-480">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-480">'Blazor'</span></span>
- <span data-ttu-id="34b9b-481">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-481">'Identity'</span></span>
- <span data-ttu-id="34b9b-482">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-482">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-483">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-483">'Razor'</span></span>
- <span data-ttu-id="34b9b-484">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-484">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-485">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-485">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-486">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-486">'Blazor'</span></span>
- <span data-ttu-id="34b9b-487">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-487">'Identity'</span></span>
- <span data-ttu-id="34b9b-488">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-488">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-489">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-489">'Razor'</span></span>
- <span data-ttu-id="34b9b-490">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-490">'SignalR' uid:</span></span> 

<span data-ttu-id="34b9b-491">---- | |[年齡](https://tools.ietf.org/html/rfc7234#section-5.1)|在源伺服器上產生或成功驗證回應後的時間量估計（以秒為單位）。</span><span class="sxs-lookup"><span data-stu-id="34b9b-491">---- | | [Age](https://tools.ietf.org/html/rfc7234#section-5.1)     | An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> <span data-ttu-id="34b9b-492">| |[過期](https://tools.ietf.org/html/rfc7234#section-5.3)|回應被視為過時的時間。</span><span class="sxs-lookup"><span data-stu-id="34b9b-492">| | [Expires](https://tools.ietf.org/html/rfc7234#section-5.3) | The time after which the response is considered stale.</span></span> <span data-ttu-id="34b9b-493">| |[Pragma](https://tools.ietf.org/html/rfc7234#section-5.4) |存在於與 HTTP/1.0 快取的回溯相容性，以進行設定 `no-cache` 行為。</span><span class="sxs-lookup"><span data-stu-id="34b9b-493">| | [Pragma](https://tools.ietf.org/html/rfc7234#section-5.4)  | Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="34b9b-494">如果 `Cache-Control` 標頭存在，則 `Pragma` 會忽略標頭。</span><span class="sxs-lookup"><span data-stu-id="34b9b-494">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> <span data-ttu-id="34b9b-495">| |[Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4) |指定不應傳送快取的回應，除非所有 `Vary` 標頭欄位都符合快取回應的原始要求和新的要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-495">| | [Vary](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="34b9b-496">以 HTTP 為基礎的快取會遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="34b9b-496">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="34b9b-497">快取[控制標頭的 HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格需要快取以接受 `Cache-Control` 用戶端所傳送的有效標頭。</span><span class="sxs-lookup"><span data-stu-id="34b9b-497">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="34b9b-498">用戶端可以使用 `no-cache` 標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-498">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="34b9b-499">如果您考慮 HTTP 快取的目標，一律接受用戶端 `Cache-Control` 要求標頭是合理的。</span><span class="sxs-lookup"><span data-stu-id="34b9b-499">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="34b9b-500">在官方規格中，快取的目的是要降低跨用戶端、proxy 和伺服器網路上滿足要求的延遲和網路負擔。</span><span class="sxs-lookup"><span data-stu-id="34b9b-500">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="34b9b-501">這不一定是控制源伺服器負載的方式。</span><span class="sxs-lookup"><span data-stu-id="34b9b-501">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="34b9b-502">使用回應快取[中介軟體](xref:performance/caching/middleware)時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="34b9b-502">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="34b9b-503">[中介軟體的規劃增強功能](https://github.com/dotnet/AspNetCore/issues/2612)，是在決定提供快取回應時，設定中介軟體忽略要求 `Cache-Control` 標頭的機會。</span><span class="sxs-lookup"><span data-stu-id="34b9b-503">[Planned enhancements to the middleware](https://github.com/dotnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="34b9b-504">規劃的增強功能可讓您有機會更妥善控制伺服器負載。</span><span class="sxs-lookup"><span data-stu-id="34b9b-504">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="34b9b-505">ASP.NET Core 中的其他快取技術</span><span class="sxs-lookup"><span data-stu-id="34b9b-505">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="34b9b-506">記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="34b9b-506">In-memory caching</span></span>

<span data-ttu-id="34b9b-507">記憶體內部快取會使用伺服器記憶體來儲存快取的資料。</span><span class="sxs-lookup"><span data-stu-id="34b9b-507">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="34b9b-508">這種類型的快取適用于單一伺服器，或使用*粘滯會話*的多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="34b9b-508">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="34b9b-509">「粘滯話」表示用戶端的要求一律會路由傳送至相同的伺服器進行處理。</span><span class="sxs-lookup"><span data-stu-id="34b9b-509">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="34b9b-510">如需詳細資訊，請參閱<xref:performance/caching/memory>。</span><span class="sxs-lookup"><span data-stu-id="34b9b-510">For more information, see <xref:performance/caching/memory>.</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="34b9b-511">分散式快取</span><span class="sxs-lookup"><span data-stu-id="34b9b-511">Distributed Cache</span></span>

<span data-ttu-id="34b9b-512">當應用程式裝載于雲端或伺服器陣列時，使用分散式快取將資料儲存在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="34b9b-512">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="34b9b-513">快取會在處理要求的伺服器之間共用。</span><span class="sxs-lookup"><span data-stu-id="34b9b-513">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="34b9b-514">如果用戶端的快取資料可供使用，則用戶端可以提交由群組中的任何伺服器所處理的要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-514">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="34b9b-515">ASP.NET Core 適用于 SQL Server、 [Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)和[NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)分散式快取。</span><span class="sxs-lookup"><span data-stu-id="34b9b-515">ASP.NET Core works with SQL Server, [Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis), and [NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/) distributed caches.</span></span>

<span data-ttu-id="34b9b-516">如需詳細資訊，請參閱<xref:performance/caching/distributed>。</span><span class="sxs-lookup"><span data-stu-id="34b9b-516">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="34b9b-517">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="34b9b-517">Cache Tag Helper</span></span>

<span data-ttu-id="34b9b-518">使用快取標籤協助程式從 MVC 視圖或頁面快取內容 Razor 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-518">Cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="34b9b-519">快取標記協助程式會使用記憶體內部快取來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="34b9b-519">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="34b9b-520">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="34b9b-520">For more information, see <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="34b9b-521">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="34b9b-521">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="34b9b-522">使用分散式快取標記協助程式 Razor ，從分散式雲端或 web 伺服陣列案例中的 MVC 視圖或頁面快取內容。</span><span class="sxs-lookup"><span data-stu-id="34b9b-522">Cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="34b9b-523">分散式快取標記協助程式會使用 SQL Server、 [Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis)或[NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/)來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="34b9b-523">The Distributed Cache Tag Helper uses SQL Server, [Redis](https://www.nuget.org/packages/Microsoft.Extensions.Caching.StackExchangeRedis), or [NCache](https://www.nuget.org/packages/Alachisoft.NCache.OpenSource.SDK/) to store data.</span></span>

<span data-ttu-id="34b9b-524">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="34b9b-524">For more information, see <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="34b9b-525">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="34b9b-525">ResponseCache attribute</span></span>

<span data-ttu-id="34b9b-526">會 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 指定在回應快取中設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="34b9b-526">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="34b9b-527">針對包含已驗證用戶端資訊的內容停用快取。</span><span class="sxs-lookup"><span data-stu-id="34b9b-527">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="34b9b-528">應該只針對不會根據使用者身分識別或使用者是否已登入而變更的內容來啟用快取。</span><span class="sxs-lookup"><span data-stu-id="34b9b-528">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="34b9b-529"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>依據指定的查詢索引鍵清單值，改變儲存的回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-529"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="34b9b-530">當提供單一值時 `*` ，中介軟體會依所有要求查詢字串參數來改變回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-530">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span>

<span data-ttu-id="34b9b-531">必須啟用回應快取[中介軟體](xref:performance/caching/middleware)才能設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 屬性。</span><span class="sxs-lookup"><span data-stu-id="34b9b-531">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="34b9b-532">否則，就會擲回執行時間例外狀況。</span><span class="sxs-lookup"><span data-stu-id="34b9b-532">Otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="34b9b-533">屬性沒有對應的 HTTP 標頭 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-533">There isn't a corresponding HTTP header for the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="34b9b-534">屬性是由回應快取中介軟體所處理的 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="34b9b-534">The property is an HTTP feature handled by Response Caching Middleware.</span></span> <span data-ttu-id="34b9b-535">若要讓中介軟體提供快取的回應，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-535">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="34b9b-536">例如，請考慮下表所示的要求和結果順序。</span><span class="sxs-lookup"><span data-stu-id="34b9b-536">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="34b9b-537">要求</span><span class="sxs-lookup"><span data-stu-id="34b9b-537">Request</span></span>                          | <span data-ttu-id="34b9b-538">結果</span><span class="sxs-lookup"><span data-stu-id="34b9b-538">Result</span></span>                    |
| ---
<span data-ttu-id="34b9b-539">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-539">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-540">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-540">'Blazor'</span></span>
- <span data-ttu-id="34b9b-541">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-541">'Identity'</span></span>
- <span data-ttu-id="34b9b-542">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-542">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-543">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-543">'Razor'</span></span>
- <span data-ttu-id="34b9b-544">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-544">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-545">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-545">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-546">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-546">'Blazor'</span></span>
- <span data-ttu-id="34b9b-547">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-547">'Identity'</span></span>
- <span data-ttu-id="34b9b-548">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-548">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-549">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-549">'Razor'</span></span>
- <span data-ttu-id="34b9b-550">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-550">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-551">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-551">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-552">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-552">'Blazor'</span></span>
- <span data-ttu-id="34b9b-553">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-553">'Identity'</span></span>
- <span data-ttu-id="34b9b-554">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-554">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-555">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-555">'Razor'</span></span>
- <span data-ttu-id="34b9b-556">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-556">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-557">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-557">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-558">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-558">'Blazor'</span></span>
- <span data-ttu-id="34b9b-559">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-559">'Identity'</span></span>
- <span data-ttu-id="34b9b-560">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-560">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-561">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-561">'Razor'</span></span>
- <span data-ttu-id="34b9b-562">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-562">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-563">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-563">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-564">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-564">'Blazor'</span></span>
- <span data-ttu-id="34b9b-565">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-565">'Identity'</span></span>
- <span data-ttu-id="34b9b-566">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-566">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-567">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-567">'Razor'</span></span>
- <span data-ttu-id="34b9b-568">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-568">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-569">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-569">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-570">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-570">'Blazor'</span></span>
- <span data-ttu-id="34b9b-571">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-571">'Identity'</span></span>
- <span data-ttu-id="34b9b-572">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-572">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-573">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-573">'Razor'</span></span>
- <span data-ttu-id="34b9b-574">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-574">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-575">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-575">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-576">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-576">'Blazor'</span></span>
- <span data-ttu-id="34b9b-577">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-577">'Identity'</span></span>
- <span data-ttu-id="34b9b-578">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-578">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-579">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-579">'Razor'</span></span>
- <span data-ttu-id="34b9b-580">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-580">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-581">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-581">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-582">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-582">'Blazor'</span></span>
- <span data-ttu-id="34b9b-583">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-583">'Identity'</span></span>
- <span data-ttu-id="34b9b-584">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-584">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-585">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-585">'Razor'</span></span>
- <span data-ttu-id="34b9b-586">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-586">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-587">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-587">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-588">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-588">'Blazor'</span></span>
- <span data-ttu-id="34b9b-589">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-589">'Identity'</span></span>
- <span data-ttu-id="34b9b-590">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-590">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-591">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-591">'Razor'</span></span>
- <span data-ttu-id="34b9b-592">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-592">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-593">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-593">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-594">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-594">'Blazor'</span></span>
- <span data-ttu-id="34b9b-595">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-595">'Identity'</span></span>
- <span data-ttu-id="34b9b-596">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-596">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-597">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-597">'Razor'</span></span>
- <span data-ttu-id="34b9b-598">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-598">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-599">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-599">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-600">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-600">'Blazor'</span></span>
- <span data-ttu-id="34b9b-601">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-601">'Identity'</span></span>
- <span data-ttu-id="34b9b-602">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-602">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-603">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-603">'Razor'</span></span>
- <span data-ttu-id="34b9b-604">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-604">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-605">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-605">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-606">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-606">'Blazor'</span></span>
- <span data-ttu-id="34b9b-607">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-607">'Identity'</span></span>
- <span data-ttu-id="34b9b-608">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-608">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-609">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-609">'Razor'</span></span>
- <span data-ttu-id="34b9b-610">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-610">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-611">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-611">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-612">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-612">'Blazor'</span></span>
- <span data-ttu-id="34b9b-613">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-613">'Identity'</span></span>
- <span data-ttu-id="34b9b-614">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-614">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-615">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-615">'Razor'</span></span>
- <span data-ttu-id="34b9b-616">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-616">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-617">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-617">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-618">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-618">'Blazor'</span></span>
- <span data-ttu-id="34b9b-619">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-619">'Identity'</span></span>
- <span data-ttu-id="34b9b-620">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-620">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-621">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-621">'Razor'</span></span>
- <span data-ttu-id="34b9b-622">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-622">'SignalR' uid:</span></span> 

<span data-ttu-id="34b9b-623">---------------- |---標題： author： description： monikerRange： ms-chap： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-623">---------------- | --- title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-624">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-624">'Blazor'</span></span>
- <span data-ttu-id="34b9b-625">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-625">'Identity'</span></span>
- <span data-ttu-id="34b9b-626">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-626">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-627">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-627">'Razor'</span></span>
- <span data-ttu-id="34b9b-628">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-628">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-629">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-629">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-630">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-630">'Blazor'</span></span>
- <span data-ttu-id="34b9b-631">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-631">'Identity'</span></span>
- <span data-ttu-id="34b9b-632">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-632">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-633">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-633">'Razor'</span></span>
- <span data-ttu-id="34b9b-634">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-634">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-635">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-635">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-636">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-636">'Blazor'</span></span>
- <span data-ttu-id="34b9b-637">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-637">'Identity'</span></span>
- <span data-ttu-id="34b9b-638">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-638">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-639">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-639">'Razor'</span></span>
- <span data-ttu-id="34b9b-640">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-640">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-641">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-641">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-642">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-642">'Blazor'</span></span>
- <span data-ttu-id="34b9b-643">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-643">'Identity'</span></span>
- <span data-ttu-id="34b9b-644">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-644">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-645">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-645">'Razor'</span></span>
- <span data-ttu-id="34b9b-646">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-646">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-647">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-647">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-648">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-648">'Blazor'</span></span>
- <span data-ttu-id="34b9b-649">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-649">'Identity'</span></span>
- <span data-ttu-id="34b9b-650">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-650">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-651">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-651">'Razor'</span></span>
- <span data-ttu-id="34b9b-652">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-652">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-653">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-653">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-654">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-654">'Blazor'</span></span>
- <span data-ttu-id="34b9b-655">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-655">'Identity'</span></span>
- <span data-ttu-id="34b9b-656">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-656">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-657">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-657">'Razor'</span></span>
- <span data-ttu-id="34b9b-658">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-658">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-659">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-659">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-660">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-660">'Blazor'</span></span>
- <span data-ttu-id="34b9b-661">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-661">'Identity'</span></span>
- <span data-ttu-id="34b9b-662">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-662">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-663">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-663">'Razor'</span></span>
- <span data-ttu-id="34b9b-664">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-664">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-665">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-665">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-666">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-666">'Blazor'</span></span>
- <span data-ttu-id="34b9b-667">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-667">'Identity'</span></span>
- <span data-ttu-id="34b9b-668">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-668">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-669">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-669">'Razor'</span></span>
- <span data-ttu-id="34b9b-670">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-670">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-671">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-671">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-672">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-672">'Blazor'</span></span>
- <span data-ttu-id="34b9b-673">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-673">'Identity'</span></span>
- <span data-ttu-id="34b9b-674">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-674">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-675">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-675">'Razor'</span></span>
- <span data-ttu-id="34b9b-676">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-676">'SignalR' uid:</span></span> 

-
<span data-ttu-id="34b9b-677">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="34b9b-677">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="34b9b-678">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-678">'Blazor'</span></span>
- <span data-ttu-id="34b9b-679">'Identity'</span><span class="sxs-lookup"><span data-stu-id="34b9b-679">'Identity'</span></span>
- <span data-ttu-id="34b9b-680">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="34b9b-680">'Let's Encrypt'</span></span>
- <span data-ttu-id="34b9b-681">'Razor'</span><span class="sxs-lookup"><span data-stu-id="34b9b-681">'Razor'</span></span>
- <span data-ttu-id="34b9b-682">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="34b9b-682">'SignalR' uid:</span></span> 

<span data-ttu-id="34b9b-683">------------- | |`http://example.com?key1=value1` |從伺服器傳回。</span><span class="sxs-lookup"><span data-stu-id="34b9b-683">------------- | | `http://example.com?key1=value1` | Returned from the server.</span></span> <span data-ttu-id="34b9b-684">| |`http://example.com?key1=value1` |從中介軟體傳回。</span><span class="sxs-lookup"><span data-stu-id="34b9b-684">| | `http://example.com?key1=value1` | Returned from middleware.</span></span> <span data-ttu-id="34b9b-685">| |`http://example.com?key1=value2` |從伺服器傳回。</span><span class="sxs-lookup"><span data-stu-id="34b9b-685">| | `http://example.com?key1=value2` | Returned from the server.</span></span> |

<span data-ttu-id="34b9b-686">第一個要求是由伺服器傳回，並在中介軟體中快取。</span><span class="sxs-lookup"><span data-stu-id="34b9b-686">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="34b9b-687">中介軟體會傳回第二個要求，因為查詢字串符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-687">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="34b9b-688">第三個要求不在中介軟體快取中，因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="34b9b-688">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span>

<span data-ttu-id="34b9b-689"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>是用來設定和建立（via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory> ） `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-689">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> is used to configure and create (via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) a `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter`.</span></span> <span data-ttu-id="34b9b-690">會 `ResponseCacheFilter` 執行更新適當 HTTP 標頭和回應功能的工作。</span><span class="sxs-lookup"><span data-stu-id="34b9b-690">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="34b9b-691">篩選準則：</span><span class="sxs-lookup"><span data-stu-id="34b9b-691">The filter:</span></span>

* <span data-ttu-id="34b9b-692">移除、和的任何現有標頭 `Vary` `Cache-Control` `Pragma` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-692">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span>
* <span data-ttu-id="34b9b-693">根據中設定的屬性寫出適當的標頭 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-693">Writes out the appropriate headers based on the properties set in the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.</span></span>
* <span data-ttu-id="34b9b-694">如果已設定，則更新回應快取 HTTP 功能 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-694">Updates the response caching HTTP feature if <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> is set.</span></span>

### <a name="vary"></a><span data-ttu-id="34b9b-695">相同</span><span class="sxs-lookup"><span data-stu-id="34b9b-695">Vary</span></span>

<span data-ttu-id="34b9b-696">只有在設定屬性時，才會寫入這個標頭 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-696">This header is only written when the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property is set.</span></span> <span data-ttu-id="34b9b-697">屬性會設定為 `Vary` 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="34b9b-697">The property set to the `Vary` property's value.</span></span> <span data-ttu-id="34b9b-698">下列範例會使用 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 屬性：</span><span class="sxs-lookup"><span data-stu-id="34b9b-698">The following sample uses the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

<span data-ttu-id="34b9b-699">使用範例應用程式，使用瀏覽器的網路工具來查看回應標頭。</span><span class="sxs-lookup"><span data-stu-id="34b9b-699">Using the sample app, view the response headers with the browser's network tools.</span></span> <span data-ttu-id="34b9b-700">下列回應標頭會與 Cache1 頁面回應一起傳送：</span><span class="sxs-lookup"><span data-stu-id="34b9b-700">The following response headers are sent with the Cache1 page response:</span></span>

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a><span data-ttu-id="34b9b-701">NoStore 和 Location。無</span><span class="sxs-lookup"><span data-stu-id="34b9b-701">NoStore and Location.None</span></span>

<span data-ttu-id="34b9b-702"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>覆寫大部分的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="34b9b-702"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> overrides most of the other properties.</span></span> <span data-ttu-id="34b9b-703">當這個屬性設定為時 `true` ， `Cache-Control` 標頭會設定為 `no-store` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-703">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="34b9b-704">如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 設定為 `None` ：</span><span class="sxs-lookup"><span data-stu-id="34b9b-704">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is set to `None`:</span></span>

* <span data-ttu-id="34b9b-705">`Cache-Control` 設定為 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="34b9b-705">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="34b9b-706">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="34b9b-706">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="34b9b-707">如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 是 `false` <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ，而且是 `None` 、和，則 `Cache-Control` `Pragma` 會設定為 `no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-707">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is `false` and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is `None`, `Cache-Control`, and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="34b9b-708"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>對於錯誤網頁，通常會設定為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-708"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is typically set to `true` for error pages.</span></span> <span data-ttu-id="34b9b-709">範例應用程式中的 [Cache2] 頁面會產生回應標頭，以指示用戶端不要儲存回應。</span><span class="sxs-lookup"><span data-stu-id="34b9b-709">The Cache2 page in the sample app produces response headers that instruct the client not to store the response.</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

<span data-ttu-id="34b9b-710">範例應用程式會傳回具有下列標頭的 Cache2 頁面：</span><span class="sxs-lookup"><span data-stu-id="34b9b-710">The sample app returns the Cache2 page with the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="34b9b-711">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="34b9b-711">Location and Duration</span></span>

<span data-ttu-id="34b9b-712">若要啟用快取， <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 必須將設為正值，而且 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 必須是 `Any` （預設值）或 `Client` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-712">To enable caching, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> must be set to a positive value and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="34b9b-713">架構會將 `Cache-Control` 標頭設定為位置值，後面接著 `max-age` 回應的。</span><span class="sxs-lookup"><span data-stu-id="34b9b-713">The framework sets the `Cache-Control` header to the location value followed by the `max-age` of the response.</span></span>

<span data-ttu-id="34b9b-714"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>的選項 `Any` 和會 `Client` 分別轉譯為 `Cache-Control` 和的標頭值 `public` `private` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-714"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="34b9b-715">如[NoStore 和 Location. None](#nostore-and-locationnone)區段中所述，將設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 為 `None` `Cache-Control` ，並將 `Pragma` 標頭設定為 `no-cache` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-715">As noted in the [NoStore and Location.None](#nostore-and-locationnone) section, setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="34b9b-716">`Location.Any`（ `Cache-Control` 設為 `public` ）表示*用戶端或任何中繼 proxy*可能會快取此值，包括回應快取[中介軟體](xref:performance/caching/middleware)。</span><span class="sxs-lookup"><span data-stu-id="34b9b-716">`Location.Any` (`Cache-Control` set to `public`) indicates that the *client or any intermediate proxy* may cache the value, including [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

<span data-ttu-id="34b9b-717">`Location.Client`（ `Cache-Control` 設為 `private` ）表示*只有用戶端*可以快取此值。</span><span class="sxs-lookup"><span data-stu-id="34b9b-717">`Location.Client` (`Cache-Control` set to `private`) indicates that *only the client* may cache the value.</span></span> <span data-ttu-id="34b9b-718">任何中繼快取都不應該快取值，包括回應快取[中介軟體](xref:performance/caching/middleware)。</span><span class="sxs-lookup"><span data-stu-id="34b9b-718">No intermediate cache should cache the value, including [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

<span data-ttu-id="34b9b-719">快取控制標頭只會在和如何快取回應時，提供用戶端和中繼 proxy 的指引。</span><span class="sxs-lookup"><span data-stu-id="34b9b-719">Cache control headers merely provide guidance to clients and intermediary proxies when and how to cache responses.</span></span> <span data-ttu-id="34b9b-720">不保證用戶端和 proxy 會接受[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格。</span><span class="sxs-lookup"><span data-stu-id="34b9b-720">There's no guarantee that clients and proxies will honor the [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234).</span></span> <span data-ttu-id="34b9b-721">[回應快取中介軟體](xref:performance/caching/middleware)一律會遵循規格所配置的快取規則。</span><span class="sxs-lookup"><span data-stu-id="34b9b-721">[Response Caching Middleware](xref:performance/caching/middleware) always follows the caching rules laid out by the specification.</span></span>

<span data-ttu-id="34b9b-722">下列範例顯示來自範例應用程式的 Cache3 頁面模型，以及設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 和保留預設值所產生的標頭 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> ：</span><span class="sxs-lookup"><span data-stu-id="34b9b-722">The following example shows the Cache3 page model from the sample app and the headers produced by setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> and leaving the default <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> value:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

<span data-ttu-id="34b9b-723">範例應用程式會傳回具有下列標頭的 Cache3 頁面：</span><span class="sxs-lookup"><span data-stu-id="34b9b-723">The sample app returns the Cache3 page with the following header:</span></span>

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a><span data-ttu-id="34b9b-724">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="34b9b-724">Cache profiles</span></span>

<span data-ttu-id="34b9b-725">快取設定檔在中設定 MVC/Pages 時，可以設定為選項，而不是複製許多控制器動作屬性的回應快取設定 Razor `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="34b9b-725">Instead of duplicating response cache settings on many controller action attributes, cache profiles can be configured as options when setting up MVC/Razor Pages in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="34b9b-726">在參考的快取設定檔中找到的值是用來做為的預設值 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> ，而且會由屬性上指定的任何屬性加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="34b9b-726">Values found in a referenced cache profile are used as the defaults by the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="34b9b-727">設定快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="34b9b-727">Set up a cache profile.</span></span> <span data-ttu-id="34b9b-728">下列範例顯示範例應用程式中的30秒快取設定檔 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="34b9b-728">The following example shows a 30 second cache profile in the sample app's `Startup.ConfigureServices`:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="34b9b-729">範例應用程式的 Cache4 頁面模型會參考快取 `Default30` 設定檔：</span><span class="sxs-lookup"><span data-stu-id="34b9b-729">The sample app's Cache4 page model references the `Default30` cache profile:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<span data-ttu-id="34b9b-730"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>可套用至：</span><span class="sxs-lookup"><span data-stu-id="34b9b-730">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> can be applied to:</span></span>

* Razor<span data-ttu-id="34b9b-731">頁面處理常式（類別）：屬性無法套用至處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="34b9b-731"> Page handlers (classes): Attributes can't be applied to handler methods.</span></span>
* <span data-ttu-id="34b9b-732">MVC 控制器（類別）。</span><span class="sxs-lookup"><span data-stu-id="34b9b-732">MVC controllers (classes).</span></span>
* <span data-ttu-id="34b9b-733">MVC 動作（方法）：方法層級屬性會覆寫在類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="34b9b-733">MVC actions (methods): Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="34b9b-734">由快取設定檔套用至 Cache4 頁面回應的產生標頭 `Default30` ：</span><span class="sxs-lookup"><span data-stu-id="34b9b-734">The resulting header applied to the Cache4 page response by the `Default30` cache profile:</span></span>

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a><span data-ttu-id="34b9b-735">其他資源</span><span class="sxs-lookup"><span data-stu-id="34b9b-735">Additional resources</span></span>

* [<span data-ttu-id="34b9b-736">將回應儲存在快取中</span><span class="sxs-lookup"><span data-stu-id="34b9b-736">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="34b9b-737">Cache-控制項</span><span class="sxs-lookup"><span data-stu-id="34b9b-737">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
