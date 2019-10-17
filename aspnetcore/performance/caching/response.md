---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 10/15/2019
uid: performance/caching/response
ms.openlocfilehash: 4ebac97689347245d25e0954b33729d78dd1b516
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378825"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="35042-103">ASP.NET Core 中的回應快取</span><span class="sxs-lookup"><span data-stu-id="35042-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="35042-104">作者： [John 羅文](https://github.com/JunTaoLuo)、 [Rick Anderson](https://twitter.com/RickAndMSFT)、 [Steve Smith](https://ardalis.com/)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="35042-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="35042-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="35042-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="35042-106">回應快取可減少用戶端或 proxy 對 web 伺服器提出的要求數目。</span><span class="sxs-lookup"><span data-stu-id="35042-106">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="35042-107">回應快取也會減少 web 伺服器執行以產生回應的工作量。</span><span class="sxs-lookup"><span data-stu-id="35042-107">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="35042-108">回應快取是由標頭控制，可指定您希望用戶端、proxy 和中介軟體快取回應的方式。</span><span class="sxs-lookup"><span data-stu-id="35042-108">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="35042-109">[ResponseCache 屬性](#responsecache-attribute)會參與設定回應快取標頭，用戶端可能會在快取回應時接受。</span><span class="sxs-lookup"><span data-stu-id="35042-109">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="35042-110">[回應快取中介軟體](xref:performance/caching/middleware)可以用來快取伺服器上的回應。</span><span class="sxs-lookup"><span data-stu-id="35042-110">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="35042-111">中介軟體可以使用 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 屬性來影響伺服器端快取行為。</span><span class="sxs-lookup"><span data-stu-id="35042-111">The middleware can use <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="35042-112">以 HTTP 為基礎的回應快取</span><span class="sxs-lookup"><span data-stu-id="35042-112">HTTP-based response caching</span></span>

<span data-ttu-id="35042-113">[HTTP 1.1](https://tools.ietf.org/html/rfc7234)快取規格描述網際網路快取的行為。</span><span class="sxs-lookup"><span data-stu-id="35042-113">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="35042-114">用於快取的主要 HTTP 標頭是快取[控制](https://tools.ietf.org/html/rfc7234#section-5.2)，用來指定快取指示*詞。*</span><span class="sxs-lookup"><span data-stu-id="35042-114">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="35042-115">指示詞會控制快取行為，因為要求會從用戶端傳送到伺服器，而回應會從伺服器傳回用戶端。</span><span class="sxs-lookup"><span data-stu-id="35042-115">The directives control caching behavior as requests make their way from clients to servers and as responses make their way from servers back to clients.</span></span> <span data-ttu-id="35042-116">要求和回應會在 proxy 伺服器上移動，而 proxy 伺服器也必須符合 HTTP 1.1 快取規格。</span><span class="sxs-lookup"><span data-stu-id="35042-116">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="35042-117">一般 `Cache-Control` 指示詞如下表所示。</span><span class="sxs-lookup"><span data-stu-id="35042-117">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="35042-118">指示詞</span><span class="sxs-lookup"><span data-stu-id="35042-118">Directive</span></span>                                                       | <span data-ttu-id="35042-119">動作</span><span class="sxs-lookup"><span data-stu-id="35042-119">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="35042-120">public</span><span class="sxs-lookup"><span data-stu-id="35042-120">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="35042-121">快取可能會儲存回應。</span><span class="sxs-lookup"><span data-stu-id="35042-121">A cache may store the response.</span></span> |
| [<span data-ttu-id="35042-122">private</span><span class="sxs-lookup"><span data-stu-id="35042-122">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="35042-123">回應不得由共用快取儲存。</span><span class="sxs-lookup"><span data-stu-id="35042-123">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="35042-124">私用快取可能會儲存並重複使用回應。</span><span class="sxs-lookup"><span data-stu-id="35042-124">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="35042-125">最大壽命</span><span class="sxs-lookup"><span data-stu-id="35042-125">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="35042-126">用戶端不接受其年齡大於指定秒數的回應。</span><span class="sxs-lookup"><span data-stu-id="35042-126">The client doesn't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="35042-127">範例： `max-age=60` （60秒），`max-age=2592000` （1個月）</span><span class="sxs-lookup"><span data-stu-id="35042-127">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="35042-128">無-快取</span><span class="sxs-lookup"><span data-stu-id="35042-128">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="35042-129">**在要求上**：快取不得使用預存回應來滿足要求。</span><span class="sxs-lookup"><span data-stu-id="35042-129">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="35042-130">源伺服器會重新產生用戶端的回應，中介軟體會在其快取中更新儲存的回應。</span><span class="sxs-lookup"><span data-stu-id="35042-130">The origin server regenerates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="35042-131">**回應時**：回應不得用於後續要求，而不需在源伺服器上進行驗證。</span><span class="sxs-lookup"><span data-stu-id="35042-131">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="35042-132">否-存放區</span><span class="sxs-lookup"><span data-stu-id="35042-132">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="35042-133">**在要求上**：快取不得儲存要求。</span><span class="sxs-lookup"><span data-stu-id="35042-133">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="35042-134">**回應時**：快取不得儲存回應的任何部分。</span><span class="sxs-lookup"><span data-stu-id="35042-134">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="35042-135">下表顯示在快取中扮演角色的其他快取標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-135">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="35042-136">頁首</span><span class="sxs-lookup"><span data-stu-id="35042-136">Header</span></span>                                                     | <span data-ttu-id="35042-137">功能</span><span class="sxs-lookup"><span data-stu-id="35042-137">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="35042-138">存在</span><span class="sxs-lookup"><span data-stu-id="35042-138">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="35042-139">在源伺服器上產生或成功驗證回應後的時間量估計（以秒為單位）。</span><span class="sxs-lookup"><span data-stu-id="35042-139">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="35042-140">失效</span><span class="sxs-lookup"><span data-stu-id="35042-140">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="35042-141">回應被視為過時的時間。</span><span class="sxs-lookup"><span data-stu-id="35042-141">The time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="35042-142">雜</span><span class="sxs-lookup"><span data-stu-id="35042-142">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="35042-143">存在，以提供與 HTTP/1.0 快取的回溯相容性，以設定 `no-cache` 的行為。</span><span class="sxs-lookup"><span data-stu-id="35042-143">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="35042-144">如果 `Cache-Control` 標頭存在，則會忽略 @no__t 1 標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-144">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="35042-145">相同</span><span class="sxs-lookup"><span data-stu-id="35042-145">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="35042-146">指定在快取回應的原始要求和新要求中都符合所有的 @no__t 0 標頭欄位時，不能傳送快取的回應。</span><span class="sxs-lookup"><span data-stu-id="35042-146">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="35042-147">以 HTTP 為基礎的快取會遵循要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="35042-147">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="35042-148">快取[控制標頭的 HTTP 1.1](https://tools.ietf.org/html/rfc7234#section-5.2)快取規格需要快取，以接受用戶端所傳送的有效 @no__t 1 標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-148">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="35042-149">用戶端可以使用 @no__t 0 的標頭值提出要求，並強制服務器為每個要求產生新的回應。</span><span class="sxs-lookup"><span data-stu-id="35042-149">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="35042-150">如果您考慮 HTTP 快取的目標，一律會遵循用戶端 `Cache-Control` 要求標頭是合理的。</span><span class="sxs-lookup"><span data-stu-id="35042-150">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="35042-151">在官方規格中，快取的目的是要降低跨用戶端、proxy 和伺服器網路上滿足要求的延遲和網路負擔。</span><span class="sxs-lookup"><span data-stu-id="35042-151">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="35042-152">這不一定是控制源伺服器負載的方式。</span><span class="sxs-lookup"><span data-stu-id="35042-152">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="35042-153">使用回應快取[中介軟體](xref:performance/caching/middleware)時，不會有開發人員控制此快取行為，因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="35042-153">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="35042-154">[規劃中介軟體的增強功能](https://github.com/aspnet/AspNetCore/issues/2612)，是在決定提供快取回應時，設定中介軟體以忽略要求的 @no__t 1 標頭的機會。</span><span class="sxs-lookup"><span data-stu-id="35042-154">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="35042-155">規劃的增強功能可讓您有機會更妥善控制伺服器負載。</span><span class="sxs-lookup"><span data-stu-id="35042-155">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="35042-156">ASP.NET Core 中的其他快取技術</span><span class="sxs-lookup"><span data-stu-id="35042-156">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="35042-157">記憶體內部快取</span><span class="sxs-lookup"><span data-stu-id="35042-157">In-memory caching</span></span>

<span data-ttu-id="35042-158">記憶體內部快取會使用伺服器記憶體來儲存快取的資料。</span><span class="sxs-lookup"><span data-stu-id="35042-158">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="35042-159">這種類型的快取適用于單一伺服器，或使用*粘滯會話*的多部伺服器。</span><span class="sxs-lookup"><span data-stu-id="35042-159">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="35042-160">「粘滯話」表示用戶端的要求一律會路由傳送至相同的伺服器進行處理。</span><span class="sxs-lookup"><span data-stu-id="35042-160">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="35042-161">如需詳細資訊，請參閱<xref:performance/caching/memory>。</span><span class="sxs-lookup"><span data-stu-id="35042-161">For more information, see <xref:performance/caching/memory>.</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="35042-162">分散式快取</span><span class="sxs-lookup"><span data-stu-id="35042-162">Distributed Cache</span></span>

<span data-ttu-id="35042-163">當應用程式裝載于雲端或伺服器陣列時，使用分散式快取將資料儲存在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="35042-163">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="35042-164">快取會在處理要求的伺服器之間共用。</span><span class="sxs-lookup"><span data-stu-id="35042-164">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="35042-165">如果用戶端的快取資料可供使用，則用戶端可以提交由群組中的任何伺服器所處理的要求。</span><span class="sxs-lookup"><span data-stu-id="35042-165">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="35042-166">ASP.NET Core 提供 SQL Server 和 Redis 分散式快取。</span><span class="sxs-lookup"><span data-stu-id="35042-166">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="35042-167">如需詳細資訊，請參閱<xref:performance/caching/distributed>。</span><span class="sxs-lookup"><span data-stu-id="35042-167">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="35042-168">快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="35042-168">Cache Tag Helper</span></span>

<span data-ttu-id="35042-169">使用快取標記協助程式，從 MVC 視圖或 Razor 頁面快取內容。</span><span class="sxs-lookup"><span data-stu-id="35042-169">Cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="35042-170">快取標記協助程式會使用記憶體內部快取來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="35042-170">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="35042-171">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="35042-171">For more information, see <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>.</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="35042-172">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="35042-172">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="35042-173">使用分散式快取標記協助程式，從分散式雲端或 web 伺服陣列案例中的 MVC 視圖或 Razor 頁面快取內容。</span><span class="sxs-lookup"><span data-stu-id="35042-173">Cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="35042-174">分散式快取標記協助程式會使用 SQL Server 或 Redis 來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="35042-174">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="35042-175">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="35042-175">For more information, see <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>.</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="35042-176">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="35042-176">ResponseCache attribute</span></span>

<span data-ttu-id="35042-177">@No__t-0 指定在回應快取中設定適當標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="35042-177">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="35042-178">針對包含已驗證用戶端資訊的內容停用快取。</span><span class="sxs-lookup"><span data-stu-id="35042-178">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="35042-179">應該只針對不會根據使用者身分識別或使用者是否已登入而變更的內容來啟用快取。</span><span class="sxs-lookup"><span data-stu-id="35042-179">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="35042-180"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 會根據指定的查詢索引鍵清單值，來改變預存的回應。</span><span class="sxs-lookup"><span data-stu-id="35042-180"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="35042-181">當提供了 `*` 的單一值時，中介軟體會依所有要求查詢字串參數來改變回應。</span><span class="sxs-lookup"><span data-stu-id="35042-181">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span>

<span data-ttu-id="35042-182">必須啟用回應快取[中介軟體](xref:performance/caching/middleware)，才能設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> 屬性。</span><span class="sxs-lookup"><span data-stu-id="35042-182">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="35042-183">否則，就會擲回執行時間例外狀況。</span><span class="sxs-lookup"><span data-stu-id="35042-183">Otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="35042-184">@No__t-0 屬性沒有對應的 HTTP 標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-184">There isn't a corresponding HTTP header for the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> property.</span></span> <span data-ttu-id="35042-185">屬性是由回應快取中介軟體所處理的 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="35042-185">The property is an HTTP feature handled by Response Caching Middleware.</span></span> <span data-ttu-id="35042-186">若要讓中介軟體提供快取的回應，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="35042-186">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="35042-187">例如，請考慮下表所示的要求和結果順序。</span><span class="sxs-lookup"><span data-stu-id="35042-187">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="35042-188">要求</span><span class="sxs-lookup"><span data-stu-id="35042-188">Request</span></span>                          | <span data-ttu-id="35042-189">結果</span><span class="sxs-lookup"><span data-stu-id="35042-189">Result</span></span>                    |
| -------------------------------- | ------------------------- |
| `http://example.com?key1=value1` | <span data-ttu-id="35042-190">從伺服器傳回。</span><span class="sxs-lookup"><span data-stu-id="35042-190">Returned from the server.</span></span> |
| `http://example.com?key1=value1` | <span data-ttu-id="35042-191">從中介軟體傳回。</span><span class="sxs-lookup"><span data-stu-id="35042-191">Returned from middleware.</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="35042-192">從伺服器傳回。</span><span class="sxs-lookup"><span data-stu-id="35042-192">Returned from the server.</span></span> |

<span data-ttu-id="35042-193">第一個要求是由伺服器傳回，並在中介軟體中快取。</span><span class="sxs-lookup"><span data-stu-id="35042-193">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="35042-194">中介軟體會傳回第二個要求，因為查詢字串符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="35042-194">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="35042-195">第三個要求不在中介軟體快取中，因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="35042-195">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span>

<span data-ttu-id="35042-196">@No__t-0 是用來設定和建立（透過 <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>） `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter`。</span><span class="sxs-lookup"><span data-stu-id="35042-196">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> is used to configure and create (via <xref:Microsoft.AspNetCore.Mvc.Filters.IFilterFactory>) a `Microsoft.AspNetCore.Mvc.Internal.ResponseCacheFilter`.</span></span> <span data-ttu-id="35042-197">@No__t-0 會執行更新適當 HTTP 標頭和回應功能的工作。</span><span class="sxs-lookup"><span data-stu-id="35042-197">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="35042-198">篩選準則：</span><span class="sxs-lookup"><span data-stu-id="35042-198">The filter:</span></span>

* <span data-ttu-id="35042-199">移除 `Vary`、`Cache-Control` 和 `Pragma` 的任何現有標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-199">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span>
* <span data-ttu-id="35042-200">根據 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 中設定的屬性寫出適當的標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-200">Writes out the appropriate headers based on the properties set in the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute>.</span></span>
* <span data-ttu-id="35042-201">如果已設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys>，則更新回應快取 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="35042-201">Updates the response caching HTTP feature if <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByQueryKeys> is set.</span></span>

### <a name="vary"></a><span data-ttu-id="35042-202">相同</span><span class="sxs-lookup"><span data-stu-id="35042-202">Vary</span></span>

<span data-ttu-id="35042-203">只有在設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 屬性時，才會寫入這個標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-203">This header is only written when the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property is set.</span></span> <span data-ttu-id="35042-204">屬性設定為 `Vary` 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="35042-204">The property set to the `Vary` property's value.</span></span> <span data-ttu-id="35042-205">下列範例會使用 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> 屬性：</span><span class="sxs-lookup"><span data-stu-id="35042-205">The following sample uses the <xref:Microsoft.AspNetCore.Mvc.CacheProfile.VaryByHeader> property:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache1.cshtml.cs?name=snippet)]

<span data-ttu-id="35042-206">使用範例應用程式，使用瀏覽器的網路工具來查看回應標頭。</span><span class="sxs-lookup"><span data-stu-id="35042-206">Using the sample app, view the response headers with the browser's network tools.</span></span> <span data-ttu-id="35042-207">下列回應標頭會與 Cache1 頁面回應一起傳送：</span><span class="sxs-lookup"><span data-stu-id="35042-207">The following response headers are sent with the Cache1 page response:</span></span>

```
Cache-Control: public,max-age=30
Vary: User-Agent
```

### <a name="nostore-and-locationnone"></a><span data-ttu-id="35042-208">NoStore 和 Location。無</span><span class="sxs-lookup"><span data-stu-id="35042-208">NoStore and Location.None</span></span>

<span data-ttu-id="35042-209"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 會覆寫大部分的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="35042-209"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> overrides most of the other properties.</span></span> <span data-ttu-id="35042-210">當這個屬性設定為 `true` 時，@no__t 1 標頭會設定為 `no-store`。</span><span class="sxs-lookup"><span data-stu-id="35042-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="35042-211">如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 設定為 `None`：</span><span class="sxs-lookup"><span data-stu-id="35042-211">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is set to `None`:</span></span>

* <span data-ttu-id="35042-212">`Cache-Control` 設定為 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="35042-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="35042-213">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="35042-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="35042-214">如果 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 是 `false`，而 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 是 `None`，`Cache-Control`，而 `Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="35042-214">If <xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is `false` and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> is `None`, `Cache-Control`, and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="35042-215"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> 通常會設定為錯誤網頁的 `true`。</span><span class="sxs-lookup"><span data-stu-id="35042-215"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.NoStore> is typically set to `true` for error pages.</span></span> <span data-ttu-id="35042-216">範例應用程式中的 [Cache2] 頁面會產生回應標頭，以指示用戶端不要儲存回應。</span><span class="sxs-lookup"><span data-stu-id="35042-216">The Cache2 page in the sample app produces response headers that instruct the client not to store the response.</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache2.cshtml.cs?name=snippet)]

<span data-ttu-id="35042-217">範例應用程式會傳回具有下列標頭的 Cache2 頁面：</span><span class="sxs-lookup"><span data-stu-id="35042-217">The sample app returns the Cache2 page with the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="35042-218">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="35042-218">Location and Duration</span></span>

<span data-ttu-id="35042-219">若要啟用快取，<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 必須設定為正數值，<xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 必須是 `Any` （預設值）或 `Client`。</span><span class="sxs-lookup"><span data-stu-id="35042-219">To enable caching, <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> must be set to a positive value and <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="35042-220">在此情況下，`Cache-Control` 標頭會設定為位置值，後面接著回應的 `max-age`。</span><span class="sxs-lookup"><span data-stu-id="35042-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="35042-221"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 的 `Any` 和 @no__t 2 的選項會分別轉譯為 `public` 和 `private` 的 @no__t 3 標頭值。</span><span class="sxs-lookup"><span data-stu-id="35042-221"><xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location>'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="35042-222">如先前所述，將 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 設定為 `None` 會將 `Cache-Control` 和 @no__t 3 標頭設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="35042-222">As noted previously, setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="35042-223">下列範例顯示範例應用程式中的 Cache3 頁面模型，以及設定 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> 並保留預設 <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> 值所產生的標頭：</span><span class="sxs-lookup"><span data-stu-id="35042-223">The following example shows the Cache3 page model from the sample app and the headers produced by setting <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Duration> and leaving the default <xref:Microsoft.AspNetCore.Mvc.CacheProfile.Location> value:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache3.cshtml.cs?name=snippet)]

<span data-ttu-id="35042-224">範例應用程式會傳回具有下列標頭的 Cache3 頁面：</span><span class="sxs-lookup"><span data-stu-id="35042-224">The sample app returns the Cache3 page with the following header:</span></span>

```
Cache-Control: public,max-age=10
```

### <a name="cache-profiles"></a><span data-ttu-id="35042-225">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="35042-225">Cache profiles</span></span>

<span data-ttu-id="35042-226">快取設定檔在 `Startup.ConfigureServices` 中設定 MVC/Razor Pages 時，可以設定為選項，而不是複製許多控制器動作屬性的回應快取設定。</span><span class="sxs-lookup"><span data-stu-id="35042-226">Instead of duplicating response cache settings on many controller action attributes, cache profiles can be configured as options when setting up MVC/Razor Pages in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="35042-227">在參考的快取設定檔中找到的值，會用來做為 <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> 的預設值，並由屬性上指定的任何屬性加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="35042-227">Values found in a referenced cache profile are used as the defaults by the <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="35042-228">設定快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="35042-228">Set up a cache profile.</span></span> <span data-ttu-id="35042-229">下列範例顯示範例應用程式 `Startup.ConfigureServices` 中的30秒快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="35042-229">The following example shows a 30 second cache profile in the sample app's `Startup.ConfigureServices`:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

<span data-ttu-id="35042-230">範例應用程式的 Cache4 頁面模型會參照 `Default30` 快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="35042-230">The sample app's Cache4 page model references the `Default30` cache profile:</span></span>

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Pages/Cache4.cshtml.cs?name=snippet)]

<span data-ttu-id="35042-231">@No__t-0 可套用至：</span><span class="sxs-lookup"><span data-stu-id="35042-231">The <xref:Microsoft.AspNetCore.Mvc.ResponseCacheAttribute> can be applied to:</span></span>

* <span data-ttu-id="35042-232">Razor Page 處理常式（類別） &ndash; 屬性無法套用至處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="35042-232">Razor Page handlers (classes) &ndash; Attributes can't be applied to handler methods.</span></span>
* <span data-ttu-id="35042-233">MVC 控制器（類別）。</span><span class="sxs-lookup"><span data-stu-id="35042-233">MVC controllers (classes).</span></span>
* <span data-ttu-id="35042-234">MVC 動作（方法） &ndash; 方法層級屬性會覆寫在類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="35042-234">MVC actions (methods) &ndash; Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="35042-235">由 @no__t 0 快取設定檔套用至 Cache4 頁面回應的產生標頭：</span><span class="sxs-lookup"><span data-stu-id="35042-235">The resulting header applied to the Cache4 page response by the `Default30` cache profile:</span></span>

```
Cache-Control: public,max-age=30
```

## <a name="additional-resources"></a><span data-ttu-id="35042-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="35042-236">Additional resources</span></span>

* [<span data-ttu-id="35042-237">將回應儲存在快取中</span><span class="sxs-lookup"><span data-stu-id="35042-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="35042-238">Cache-控制項</span><span class="sxs-lookup"><span data-stu-id="35042-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
