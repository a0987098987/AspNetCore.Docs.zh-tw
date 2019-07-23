---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 02/28/2019
uid: performance/caching/response
ms.openlocfilehash: 2e247dcff2cbaa3711a9206d7237a061ae351e1d
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892465"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="fa877-103">ASP.NET Core 中的回應快取</span><span class="sxs-lookup"><span data-stu-id="fa877-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="fa877-104">藉由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="fa877-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="fa877-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="fa877-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="fa877-106">回應快取可減少用戶端或 proxy 會向 web 伺服器提出的要求數目。</span><span class="sxs-lookup"><span data-stu-id="fa877-106">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="fa877-107">回應快取也可以減少 web 伺服器執行產生回應的工作。</span><span class="sxs-lookup"><span data-stu-id="fa877-107">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="fa877-108">回應快取是由指定您想用戶端、 proxy 和中的介軟體來快取回應標頭來控制。</span><span class="sxs-lookup"><span data-stu-id="fa877-108">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="fa877-109">[ResponseCache 屬性](#responsecache-attribute)參與設定快取標頭，快取的回應時，可能會接受用戶端的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-109">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="fa877-110">[回應快取中介軟體](xref:performance/caching/middleware)可以用來在伺服器上的快取回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-110">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="fa877-111">中介軟體可以使用<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>影響伺服器端快取行為的屬性。</span><span class="sxs-lookup"><span data-stu-id="fa877-111">The middleware can use <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="fa877-112">以 HTTP 為基礎的回應快取</span><span class="sxs-lookup"><span data-stu-id="fa877-112">HTTP-based response caching</span></span>

<span data-ttu-id="fa877-113">[快取的 HTTP 1.1 規格](https://tools.ietf.org/html/rfc7234)說明網際網路快取的行為方式。</span><span class="sxs-lookup"><span data-stu-id="fa877-113">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="fa877-114">主要用於快取的 HTTP 標頭[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)，這用來指定快取*指示詞*。</span><span class="sxs-lookup"><span data-stu-id="fa877-114">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="fa877-115">要求對它們的方式從用戶端伺服器內容和回應給用戶端從伺服器進行的途中，指示詞會控制快取行為。</span><span class="sxs-lookup"><span data-stu-id="fa877-115">The directives control caching behavior as requests make their way from clients to servers and as responses make their way from servers back to clients.</span></span> <span data-ttu-id="fa877-116">要求和回應移動透過 proxy 伺服器和 proxy 伺服器也必須符合 HTTP 1.1 快取規格。</span><span class="sxs-lookup"><span data-stu-id="fa877-116">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="fa877-117">常見`Cache-Control`指示詞會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="fa877-117">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="fa877-118">指示詞</span><span class="sxs-lookup"><span data-stu-id="fa877-118">Directive</span></span>                                                       | <span data-ttu-id="fa877-119">動作</span><span class="sxs-lookup"><span data-stu-id="fa877-119">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="fa877-120">public</span><span class="sxs-lookup"><span data-stu-id="fa877-120">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="fa877-121">快取可以儲存回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-121">A cache may store the response.</span></span> |
| [<span data-ttu-id="fa877-122">private</span><span class="sxs-lookup"><span data-stu-id="fa877-122">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="fa877-123">回應不能儲存的共用快取。</span><span class="sxs-lookup"><span data-stu-id="fa877-123">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="fa877-124">私用快取可以儲存和重複使用的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-124">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="fa877-125">max-age</span><span class="sxs-lookup"><span data-stu-id="fa877-125">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="fa877-126">用戶端不接受其年齡大於指定的秒數的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-126">The client doesn't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="fa877-127">例如：`max-age=60` （60 秒）， `max-age=2592000` （1 個月）</span><span class="sxs-lookup"><span data-stu-id="fa877-127">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="fa877-128">no-cache</span><span class="sxs-lookup"><span data-stu-id="fa877-128">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="fa877-129">**在要求上**:快取來滿足要求，絕不能使用的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-129">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="fa877-130">用戶端中，原始伺服器重新產生的回應和中介軟體會更新其快取中的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-130">The origin server regenerates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="fa877-131">**在回應上**:回應不必須用於後續的要求，而不需在來源伺服器上的驗證。</span><span class="sxs-lookup"><span data-stu-id="fa877-131">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="fa877-132">no-store</span><span class="sxs-lookup"><span data-stu-id="fa877-132">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="fa877-133">**在要求上**:快取不得儲存要求。</span><span class="sxs-lookup"><span data-stu-id="fa877-133">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="fa877-134">**在回應上**:快取不得儲存回應的任何部分。</span><span class="sxs-lookup"><span data-stu-id="fa877-134">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="fa877-135">其他扮演的角色中快取的快取標頭會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="fa877-135">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="fa877-136">標頭</span><span class="sxs-lookup"><span data-stu-id="fa877-136">Header</span></span>                                                     | <span data-ttu-id="fa877-137">功能</span><span class="sxs-lookup"><span data-stu-id="fa877-137">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="fa877-138">Age</span><span class="sxs-lookup"><span data-stu-id="fa877-138">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="fa877-139">估計的秒數之後產生回應，或在原始伺服器已成功驗證的時間量。</span><span class="sxs-lookup"><span data-stu-id="fa877-139">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="fa877-140">Expires</span><span class="sxs-lookup"><span data-stu-id="fa877-140">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="fa877-141">之後，回應會被視為過時時間。</span><span class="sxs-lookup"><span data-stu-id="fa877-141">The time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="fa877-142">Pragma</span><span class="sxs-lookup"><span data-stu-id="fa877-142">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="fa877-143">回溯相容性，使用 HTTP/1.0 會在快取設定存在`no-cache`行為。</span><span class="sxs-lookup"><span data-stu-id="fa877-143">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="fa877-144">如果`Cache-Control`標頭已存在，`Pragma`標頭被忽略。</span><span class="sxs-lookup"><span data-stu-id="fa877-144">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="fa877-145">Vary</span><span class="sxs-lookup"><span data-stu-id="fa877-145">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="fa877-146">指定快取的回應必須不傳送除非所有的`Vary`快取回的應的原始要求和新的要求中符合的標頭欄位。</span><span class="sxs-lookup"><span data-stu-id="fa877-146">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="fa877-147">以 HTTP 為基礎的快取方面要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="fa877-147">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="fa877-148">[Cache-control 標頭的 HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234#section-5.2)需要接受有效的快取`Cache-Control`用戶端傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="fa877-148">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="fa877-149">用戶端可要求`no-cache`標頭值並強制產生新的回應，每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa877-149">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="fa877-150">一律接受用戶端`Cache-Control`才有您考慮的目標 HTTP 快取要求標頭強大的意義。</span><span class="sxs-lookup"><span data-stu-id="fa877-150">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="fa877-151">正式規格，在快取的目的在於降低跨網路的用戶端、 proxy 和伺服器滿足要求的延遲和網路額外負荷。</span><span class="sxs-lookup"><span data-stu-id="fa877-151">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="fa877-152">它不一定是控制原始伺服器上的負載的方式。</span><span class="sxs-lookup"><span data-stu-id="fa877-152">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="fa877-153">沒有此快取的行為沒有開發人員控制使用時[回應快取中介軟體](xref:performance/caching/middleware)因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="fa877-153">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="fa877-154">[計劃中介軟體的增強功能](https://github.com/aspnet/AspNetCore/issues/2612)是設定來略過要求的中介軟體的好機會`Cache-Control`時決定要做為快取回的應標頭。</span><span class="sxs-lookup"><span data-stu-id="fa877-154">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="fa877-155">計劃的增強功能提供更好的控制伺服器負載的機會。</span><span class="sxs-lookup"><span data-stu-id="fa877-155">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="fa877-156">ASP.NET Core 中的其他快取技術</span><span class="sxs-lookup"><span data-stu-id="fa877-156">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="fa877-157">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="fa877-157">In-memory caching</span></span>

<span data-ttu-id="fa877-158">記憶體中快取會使用伺服器記憶體儲存快取的資料。</span><span class="sxs-lookup"><span data-stu-id="fa877-158">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="fa877-159">這種類型的快取是適用於單一伺服器或多部伺服器，使用*黏性工作階段*。</span><span class="sxs-lookup"><span data-stu-id="fa877-159">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="fa877-160">黏性工作階段，表示用戶端的要求會一律路由傳送至相同的伺服器進行處理。</span><span class="sxs-lookup"><span data-stu-id="fa877-160">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="fa877-161">如需詳細資訊，請參閱 <xref:performance/caching/memory>。</span><span class="sxs-lookup"><span data-stu-id="fa877-161">For more information, see <xref:performance/caching/memory>.</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="fa877-162">分散式快取</span><span class="sxs-lookup"><span data-stu-id="fa877-162">Distributed Cache</span></span>

<span data-ttu-id="fa877-163">若要將資料儲存在記憶體中，應用程式裝載在雲端或伺服器的伺服器陣列時使用分散式快取。</span><span class="sxs-lookup"><span data-stu-id="fa877-163">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="fa877-164">處理要求的伺服器之間共用快取。</span><span class="sxs-lookup"><span data-stu-id="fa877-164">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="fa877-165">用戶端可以提交的要求，如果用戶端快取的資料可用，由群組中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa877-165">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="fa877-166">ASP.NET Core 提供 SQL Server 和分散式的 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="fa877-166">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="fa877-167">如需詳細資訊，請參閱 <xref:performance/caching/distributed>。</span><span class="sxs-lookup"><span data-stu-id="fa877-167">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="fa877-168">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="fa877-168">Cache Tag Helper</span></span>

<span data-ttu-id="fa877-169">使用快取標籤協助程式會快取中的 MVC 檢視或 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="fa877-169">Cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="fa877-170">快取標籤協助程式會使用記憶體中快取來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="fa877-170">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="fa877-171">如需詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="fa877-171">For more information, see <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="fa877-172">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="fa877-172">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="fa877-173">快取中的 MVC 檢視或 Razor 頁面的內容，在分散式雲端或 web 伺服陣列案例中，使用分散式快取標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="fa877-173">Cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="fa877-174">分散式快取標籤協助程式會使用 SQL Server 或 Redis 來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="fa877-174">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="fa877-175">如需詳細資訊，請參閱 <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="fa877-175">For more information, see <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="fa877-176">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="fa877-176">ResponseCache attribute</span></span>

<span data-ttu-id="fa877-177"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>指定回應的快取中設定適當的標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="fa877-177">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="fa877-178">停用快取內容，其中包含已驗證的用戶端的資訊。</span><span class="sxs-lookup"><span data-stu-id="fa877-178">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="fa877-179">啟用快取應該只針對不會變更使用者的身分識別或使用者已登入為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="fa877-179">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="fa877-180"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 指定清單的查詢索引鍵的值而異的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-180"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="fa877-181">單一值時`*`是所有的回應要求查詢字串參數提供中介軟體而異。</span><span class="sxs-lookup"><span data-stu-id="fa877-181">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span>

<span data-ttu-id="fa877-182">[回應快取中介軟體](xref:performance/caching/middleware)必須設定啟用<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>屬性。</span><span class="sxs-lookup"><span data-stu-id="fa877-182">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="fa877-183">否則，會擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="fa877-183">Otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="fa877-184">沒有對應的 HTTP 標頭，如<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>屬性。</span><span class="sxs-lookup"><span data-stu-id="fa877-184">There isn't a corresponding HTTP header for the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="fa877-185">回應快取中介軟體處理一項 HTTP 功能屬性。</span><span class="sxs-lookup"><span data-stu-id="fa877-185">The property is an HTTP feature handled by Response Caching Middleware.</span></span> <span data-ttu-id="fa877-186">做為快取回的應中介軟體，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="fa877-186">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="fa877-187">例如，請考慮要求和下表所示的結果的順序。</span><span class="sxs-lookup"><span data-stu-id="fa877-187">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="fa877-188">要求</span><span class="sxs-lookup"><span data-stu-id="fa877-188">Request</span></span>                          | <span data-ttu-id="fa877-189">結果</span><span class="sxs-lookup"><span data-stu-id="fa877-189">Result</span></span>                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | <span data-ttu-id="fa877-190">從伺服器傳回。</span><span class="sxs-lookup"><span data-stu-id="fa877-190">Returned from the server.</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="fa877-191">傳回從中介軟體。</span><span class="sxs-lookup"><span data-stu-id="fa877-191">Returned from middleware.</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="fa877-192">從伺服器傳回。</span><span class="sxs-lookup"><span data-stu-id="fa877-192">Returned from the server.</span></span> |

<span data-ttu-id="fa877-193">第一個要求是由伺服器傳回，而且快取中介軟體中。</span><span class="sxs-lookup"><span data-stu-id="fa877-193">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="fa877-194">第二個要求會傳回由中介軟體中，因為查詢字串比對前一個要求。</span><span class="sxs-lookup"><span data-stu-id="fa877-194">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="fa877-195">第三個要求不在快取中介軟體，因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="fa877-195">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span>

<span data-ttu-id="fa877-196"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>用來設定並建立 (透過<xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>。</span><span class="sxs-lookup"><span data-stu-id="fa877-196">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> is used to configure and create (via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) a <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>.</span></span> <span data-ttu-id="fa877-197"><xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter>之更新適當的 HTTP 標頭和回應的功能。</span><span class="sxs-lookup"><span data-stu-id="fa877-197">The <xref:Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter> performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="fa877-198">篩選器：</span><span class="sxs-lookup"><span data-stu-id="fa877-198">The filter:</span></span>

* <span data-ttu-id="fa877-199">移除任何現有的標頭，如`Vary`， `Cache-Control`，和`Pragma`。</span><span class="sxs-lookup"><span data-stu-id="fa877-199">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span>
* <span data-ttu-id="fa877-200">寫出在設定的屬性為根據適當的標頭<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>。</span><span class="sxs-lookup"><span data-stu-id="fa877-200">Writes out the appropriate headers based on the properties set in the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.</span></span>
* <span data-ttu-id="fa877-201">更新快取 HTTP 功能，如果回應<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>設定。</span><span class="sxs-lookup"><span data-stu-id="fa877-201">Updates the response caching HTTP feature if <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> is set.</span></span>

### <a name="vary"></a><span data-ttu-id="fa877-202">Vary</span><span class="sxs-lookup"><span data-stu-id="fa877-202">Vary</span></span>

<span data-ttu-id="fa877-203">此標頭，才會寫入時<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader>屬性設定。</span><span class="sxs-lookup"><span data-stu-id="fa877-203">This header is only written when the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property is set.</span></span> <span data-ttu-id="fa877-204">將屬性設定為`Vary`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="fa877-204">The property set to the `Vary` property's value.</span></span> <span data-ttu-id="fa877-205">下列範例會使用<xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader>屬性：</span><span class="sxs-lookup"><span data-stu-id="fa877-205">The following sample uses the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

<span data-ttu-id="fa877-206">使用範例應用程式，請檢視瀏覽器的網路工具的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="fa877-206">Using the sample app, view the response headers with the browser's network tools.</span></span> <span data-ttu-id="fa877-207">下列的回應標頭與 Cache1 頁面回應一起傳送：</span><span class="sxs-lookup"><span data-stu-id="fa877-207">The following response headers are sent with the Cache1 page response:</span></span>

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a><span data-ttu-id="fa877-208">NoStore 和 Location.None</span><span class="sxs-lookup"><span data-stu-id="fa877-208">NoStore and Location.None</span></span>

<span data-ttu-id="fa877-209"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 大部分的其他屬性會覆寫。</span><span class="sxs-lookup"><span data-stu-id="fa877-209"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> overrides most of the other properties.</span></span> <span data-ttu-id="fa877-210">當這個屬性設定為`true`，則`Cache-Control`標頭設定為`no-store`。</span><span class="sxs-lookup"><span data-stu-id="fa877-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="fa877-211">如果<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>設為`None`:</span><span class="sxs-lookup"><span data-stu-id="fa877-211">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is set to `None`:</span></span>

* <span data-ttu-id="fa877-212">`Cache-Control` 設定為 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="fa877-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="fa877-213">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="fa877-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="fa877-214">如果<xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore>已`false`並<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>是`None`， `Cache-Control`，和`Pragma`設為`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="fa877-214">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is `false` and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is `None`, `Cache-Control`, and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="fa877-215"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 通常會設定為`true`錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="fa877-215"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is typically set to `true` for error pages.</span></span> <span data-ttu-id="fa877-216">範例應用程式中的 [Cache2] 頁面會產生指示用戶端不要儲存回應的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="fa877-216">The Cache2 page in the sample app produces response headers that instruct the client not to store the response.</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

<span data-ttu-id="fa877-217">範例應用程式會傳回包含下列標頭 Cache2 頁面：</span><span class="sxs-lookup"><span data-stu-id="fa877-217">The sample app returns the Cache2 page with the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="fa877-218">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="fa877-218">Location and Duration</span></span>

<span data-ttu-id="fa877-219">若要啟用快取<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration>必須設為正值和<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>必須是`Any`（預設值） 或`Client`。</span><span class="sxs-lookup"><span data-stu-id="fa877-219">To enable caching, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> must be set to a positive value and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="fa877-220">在此情況下，`Cache-Control`標頭設定為後面的位置值`max-age`的回應。</span><span class="sxs-lookup"><span data-stu-id="fa877-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="fa877-221"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>選項`Any`並`Client`轉譯成`Cache-Control`標頭值`public`和`private`分別。</span><span class="sxs-lookup"><span data-stu-id="fa877-221"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="fa877-222">如先前所述，設定<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>要`None`同時設定`Cache-Control`並`Pragma`標頭`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="fa877-222">As noted previously, setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="fa877-223">下列範例示範 Cache3 頁面模型，從範例應用程式和設定所產生的標頭<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration>並保留預設值<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>值：</span><span class="sxs-lookup"><span data-stu-id="fa877-223">The following example shows the Cache3 page model from the sample app and the headers produced by setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> and leaving the default <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> value:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

<span data-ttu-id="fa877-224">範例應用程式會傳回下列標頭的 Cache3 頁面：</span><span class="sxs-lookup"><span data-stu-id="fa877-224">The sample app returns the Cache3 page with the following header:</span></span>

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a><span data-ttu-id="fa877-225">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="fa877-225">Cache profiles</span></span>

<span data-ttu-id="fa877-226">而不必重複許多的控制器動作屬性上的回應快取設定，快取設定檔可設定選項設定中的 MVC/Razor Pages `Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="fa877-226">Instead of duplicating response cache settings on many controller action attributes, cache profiles can be configured as options when setting up MVC/Razor Pages in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="fa877-227">在參考的快取設定檔中找到的值會做的預設值<xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>並會覆寫為任何屬性上指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="fa877-227">Values found in a referenced cache profile are used as the defaults by the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="fa877-228">設定快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="fa877-228">Set up a cache profile.</span></span> <span data-ttu-id="fa877-229">下列範例顯示 30 的第二個快取設定檔，在範例應用程式的`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fa877-229">The following example shows a 30 second cache profile in the sample app's `Startup.ConfigureServices`:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="fa877-230">範例應用程式的 Cache4 頁面模型參考`Default30`快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="fa877-230">The sample app's Cache4 page model references the `Default30` cache profile:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<span data-ttu-id="fa877-231"><xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>可以套用至：</span><span class="sxs-lookup"><span data-stu-id="fa877-231">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> can be applied to:</span></span>

* <span data-ttu-id="fa877-232">Razor 頁面處理常式 （類別）&ndash;屬性無法套用至處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="fa877-232">Razor Page handlers (classes) &ndash; Attributes can't be applied to handler methods.</span></span>
* <span data-ttu-id="fa877-233">MVC 控制站 （類別）。</span><span class="sxs-lookup"><span data-stu-id="fa877-233">MVC controllers (classes).</span></span>
* <span data-ttu-id="fa877-234">MVC 動作 （方法）&ndash;方法層級屬性會覆寫類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="fa877-234">MVC actions (methods) &ndash; Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="fa877-235">套用至 Cache4 頁面回應所產生的標頭`Default30`快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="fa877-235">The resulting header applied to the Cache4 page response by the `Default30` cache profile:</span></span>

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a><span data-ttu-id="fa877-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="fa877-236">Additional resources</span></span>

* [<span data-ttu-id="fa877-237">將回應儲存在快取</span><span class="sxs-lookup"><span data-stu-id="fa877-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="fa877-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="fa877-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
