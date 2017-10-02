---
title: "回應快取中 ASP.NET Core"
author: rick-anderson
description: "了解如何使用快取以降低頻寬，並提升效能的回應。"
keywords: "ASP.NET Core，快取 HTTP 標頭的回應"
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.assetid: cb42035a-60b0-472e-a614-cb79f443f654
ms.prod: asp.net-core
uid: performance/caching/response
ms.openlocfilehash: 79d9246632aae0fe9c3629fd7202842836828151
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="03c06-104">回應快取中 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03c06-104">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="03c06-105">由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="03c06-105">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="03c06-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="03c06-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="03c06-107">回應快取可減少用戶端或 proxy 可讓 web 伺服器的要求數目。</span><span class="sxs-lookup"><span data-stu-id="03c06-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="03c06-108">回應快取也可以減少網頁伺服器執行產生回應的工作。</span><span class="sxs-lookup"><span data-stu-id="03c06-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="03c06-109">指定您要用戶端、 proxy、 和中的介軟體來快取回應的標頭會控制快取回應。</span><span class="sxs-lookup"><span data-stu-id="03c06-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="03c06-110">Web 伺服器可以快取的回應，當您將加入[回應快取中介軟體](xref:performance/caching/middleware)。</span><span class="sxs-lookup"><span data-stu-id="03c06-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="03c06-111">HTTP 回應的快取</span><span class="sxs-lookup"><span data-stu-id="03c06-111">HTTP-based response caching</span></span>

<span data-ttu-id="03c06-112">[HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234)描述網際網路快取的行為方式。</span><span class="sxs-lookup"><span data-stu-id="03c06-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="03c06-113">主要用於快取的 HTTP 標頭是[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)，用來指定快取*指示詞*。</span><span class="sxs-lookup"><span data-stu-id="03c06-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="03c06-114">要求對其的方式從用戶端伺服器和個回應傳回給用戶端從伺服器進行其方法，指示詞會控制快取行為。</span><span class="sxs-lookup"><span data-stu-id="03c06-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="03c06-115">要求和回應移動透過 proxy 伺服器和 proxy 伺服器也必須符合 HTTP 1.1 快取的規格。</span><span class="sxs-lookup"><span data-stu-id="03c06-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="03c06-116">一般`Cache-Control`指示詞會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="03c06-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="03c06-117">指示詞</span><span class="sxs-lookup"><span data-stu-id="03c06-117">Directive</span></span>                                                       | <span data-ttu-id="03c06-118">動作</span><span class="sxs-lookup"><span data-stu-id="03c06-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="03c06-119">public</span><span class="sxs-lookup"><span data-stu-id="03c06-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="03c06-120">快取可能會將回應儲存。</span><span class="sxs-lookup"><span data-stu-id="03c06-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="03c06-121">private</span><span class="sxs-lookup"><span data-stu-id="03c06-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="03c06-122">回應不能儲存的共用快取。</span><span class="sxs-lookup"><span data-stu-id="03c06-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="03c06-123">私用快取可以儲存和重複使用的回應。</span><span class="sxs-lookup"><span data-stu-id="03c06-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="03c06-124">保留時間上限</span><span class="sxs-lookup"><span data-stu-id="03c06-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="03c06-125">用戶端不接受其年齡大於指定的秒數的回應。</span><span class="sxs-lookup"><span data-stu-id="03c06-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="03c06-126">範例： `max-age=60` （60 秒）， `max-age=2592000` （1 個月）</span><span class="sxs-lookup"><span data-stu-id="03c06-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="03c06-127">無快取</span><span class="sxs-lookup"><span data-stu-id="03c06-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="03c06-128">**在要求上**： 快取不能使用的預存的回應滿足要求。</span><span class="sxs-lookup"><span data-stu-id="03c06-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="03c06-129">注意： 為用戶端，來源伺服器重新產生的回應和中介軟體會更新其快取中的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="03c06-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="03c06-130">**回應**： 回應未必須用於後續的要求，但不在原始伺服器上的驗證。</span><span class="sxs-lookup"><span data-stu-id="03c06-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="03c06-131">無存放區</span><span class="sxs-lookup"><span data-stu-id="03c06-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="03c06-132">**在要求上**： 快取不能儲存的要求。</span><span class="sxs-lookup"><span data-stu-id="03c06-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="03c06-133">**回應**： 快取不能儲存任何回應的一部分。</span><span class="sxs-lookup"><span data-stu-id="03c06-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="03c06-134">其他扮演的角色中快取的快取標頭會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="03c06-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="03c06-135">頁首</span><span class="sxs-lookup"><span data-stu-id="03c06-135">Header</span></span>                                                     | <span data-ttu-id="03c06-136">函式</span><span class="sxs-lookup"><span data-stu-id="03c06-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="03c06-137">存留期</span><span class="sxs-lookup"><span data-stu-id="03c06-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="03c06-138">估計的時間，以秒為單位，因為產生的回應，或是在原始伺服器成功驗證。</span><span class="sxs-lookup"><span data-stu-id="03c06-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="03c06-139">到期</span><span class="sxs-lookup"><span data-stu-id="03c06-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="03c06-140">日期/時間之後的回應會被視為過時了。</span><span class="sxs-lookup"><span data-stu-id="03c06-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="03c06-141">Pragma</span><span class="sxs-lookup"><span data-stu-id="03c06-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="03c06-142">回溯相容性 HTTP/1.0 會在快取設定存在`no-cache`行為。</span><span class="sxs-lookup"><span data-stu-id="03c06-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="03c06-143">如果`Cache-Control`標頭已存在，`Pragma`標頭會被忽略。</span><span class="sxs-lookup"><span data-stu-id="03c06-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="03c06-144">而有所不同</span><span class="sxs-lookup"><span data-stu-id="03c06-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="03c06-145">指定快取的回應必須不傳送除非所有的`Vary`標頭欄位相符的原始要求的快取的回應和新的要求。</span><span class="sxs-lookup"><span data-stu-id="03c06-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="03c06-146">以 HTTP 為基礎的快取方面要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="03c06-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="03c06-147">[HTTP 1.1 快取的快取控制標頭的規格](https://tools.ietf.org/html/rfc7234#section-5.2)需要遵守有效的快取`Cache-Control`用戶端傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="03c06-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="03c06-148">用戶端可要求`no-cache`標頭值，強制產生新的回應，每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="03c06-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="03c06-149">永遠接受用戶端`Cache-Control`要求標頭就有意義，如果您考慮 HTTP 快取的目標。</span><span class="sxs-lookup"><span data-stu-id="03c06-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="03c06-150">官方規格，在快取是用來減少滿足要求的用戶端、 proxy 和伺服器在網路上的延遲和網路負擔。</span><span class="sxs-lookup"><span data-stu-id="03c06-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="03c06-151">它不一定能夠控制在來源伺服器上的負載。</span><span class="sxs-lookup"><span data-stu-id="03c06-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="03c06-152">沒有目前開發人員控制此快取行為時沒有使用[回應快取中介軟體](xref:performance/caching/middleware)由於中介軟體遵守正式快取規格。</span><span class="sxs-lookup"><span data-stu-id="03c06-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="03c06-153">[未來的增強功能的中介軟體](https://github.com/aspnet/ResponseCaching/issues/96)設定要略過要求的中介軟體將會允許`Cache-Control`標頭決定要提供快取的回應時。</span><span class="sxs-lookup"><span data-stu-id="03c06-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="03c06-154">這會提供您更好的控制負載伺服器上有機會時使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="03c06-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="03c06-155">在 ASP.NET Core 其他快取技術</span><span class="sxs-lookup"><span data-stu-id="03c06-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="03c06-156">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="03c06-156">In-memory caching</span></span>

<span data-ttu-id="03c06-157">記憶體中快取使用伺服器記憶體來儲存快取的資料。</span><span class="sxs-lookup"><span data-stu-id="03c06-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="03c06-158">這種類型的快取是適用於單一伺服器或多部伺服器使用*黏性工作階段*。</span><span class="sxs-lookup"><span data-stu-id="03c06-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="03c06-159">自黏工作階段表示，從用戶端要求一律將路由傳送至相同的伺服器進行處理。</span><span class="sxs-lookup"><span data-stu-id="03c06-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="03c06-160">如需詳細資訊，請參閱[介紹記憶體中快取中 ASP.NET Core](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="03c06-160">For more information, see [Introduction to in-memory caching in ASP.NET Core](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="03c06-161">分散式快取</span><span class="sxs-lookup"><span data-stu-id="03c06-161">Distributed Cache</span></span>

<span data-ttu-id="03c06-162">若要將資料儲存在記憶體中，當應用程式裝載於雲端或伺服器陣列中使用分散式快取。</span><span class="sxs-lookup"><span data-stu-id="03c06-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="03c06-163">處理要求的伺服器之間共用快取。</span><span class="sxs-lookup"><span data-stu-id="03c06-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="03c06-164">用戶端可以提交要求處理的群組中的任何伺服器，且可用的用戶端快取的資料。</span><span class="sxs-lookup"><span data-stu-id="03c06-164">A client can submit a request that is handled by any server in the group and cached data for the client is available.</span></span> <span data-ttu-id="03c06-165">ASP.NET Core 提供 SQL Server 和分散式的 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="03c06-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="03c06-166">如需詳細資訊，請參閱[使用分散式快取](xref:performance/caching/distributed)。</span><span class="sxs-lookup"><span data-stu-id="03c06-166">For more information, see [Working with a distributed cache](xref:performance/caching/distributed).</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="03c06-167">快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="03c06-167">Cache Tag Helper</span></span>

<span data-ttu-id="03c06-168">您可以與快取標記協助程式快取中的 MVC 檢視或 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="03c06-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="03c06-169">快取標記協助程式使用記憶體中快取來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="03c06-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="03c06-170">如需詳細資訊，請參閱[ASP.NET Core MVC 中的快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="03c06-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="03c06-171">分散式快取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="03c06-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="03c06-172">您可以使用分散式快取標記協助程式快取的 MVC 檢視或分散式的雲端或 web 伺服陣列案例中的 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="03c06-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="03c06-173">分散式快取標記協助程式用來儲存資料的 SQL Server 或 Redis。</span><span class="sxs-lookup"><span data-stu-id="03c06-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="03c06-174">如需詳細資訊，請參閱[分散式快取標記協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="03c06-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="03c06-175">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="03c06-175">ResponseCache attribute</span></span>

<span data-ttu-id="03c06-176">`ResponseCacheAttribute`指定在回應快取中設定適當的標頭的必要參數。</span><span class="sxs-lookup"><span data-stu-id="03c06-176">The `ResponseCacheAttribute` specifies the parameters necessary for setting appropriate headers in response caching.</span></span> <span data-ttu-id="03c06-177">請參閱[ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute)參數的描述。</span><span class="sxs-lookup"><span data-stu-id="03c06-177">See [ResponseCacheAttribute](/aspnet/core/api/microsoft.aspnetcore.mvc.responsecacheattribute) for a description of the parameters.</span></span>

> [!WARNING]
> <span data-ttu-id="03c06-178">停用快取內容，其中包含已驗證的用戶端的資訊。</span><span class="sxs-lookup"><span data-stu-id="03c06-178">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="03c06-179">快取，才應該啟用並不會變更使用者的身分識別或使用者是否登入為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="03c06-179">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is logged in.</span></span>

<span data-ttu-id="03c06-180">`VaryByQueryKeys string[]`（需要 ASP.NET Core 1.1 版和更新版本）： 設定時，回應快取中介軟體會因預存的回應指定的查詢索引鍵清單的值。</span><span class="sxs-lookup"><span data-stu-id="03c06-180">`VaryByQueryKeys string[]` (requires ASP.NET Core 1.1 and higher): When set, the Response Caching Middleware varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="03c06-181">必須啟用回應快取中介軟體，才能設定`VaryByQueryKeys`屬性; 否則擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="03c06-181">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="03c06-182">沒有對應的 HTTP 標頭`VaryByQueryKeys`屬性。</span><span class="sxs-lookup"><span data-stu-id="03c06-182">There's no corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="03c06-183">這個屬性是 HTTP 功能由回應快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="03c06-183">This property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="03c06-184">中介軟體提供快取回的應，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="03c06-184">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="03c06-185">例如，請考慮要求和結果下表所示的順序。</span><span class="sxs-lookup"><span data-stu-id="03c06-185">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="03c06-186">要求</span><span class="sxs-lookup"><span data-stu-id="03c06-186">Request</span></span>                          | <span data-ttu-id="03c06-187">結果</span><span class="sxs-lookup"><span data-stu-id="03c06-187">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="03c06-188">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="03c06-188">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="03c06-189">所傳回的中介軟體</span><span class="sxs-lookup"><span data-stu-id="03c06-189">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="03c06-190">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="03c06-190">Returned from server</span></span>     |

<span data-ttu-id="03c06-191">第一個要求是由伺服器傳回，而且快取中介軟體。</span><span class="sxs-lookup"><span data-stu-id="03c06-191">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="03c06-192">第二個要求會傳回由中介軟體，因為查詢字串比對前一個要求。</span><span class="sxs-lookup"><span data-stu-id="03c06-192">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="03c06-193">第三項要求不在中介軟體快取因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="03c06-193">The third request is not in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="03c06-194">`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) `ResponseCacheFilter`。</span><span class="sxs-lookup"><span data-stu-id="03c06-194">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a `ResponseCacheFilter`.</span></span> <span data-ttu-id="03c06-195">`ResponseCacheFilter`執行的工作更新適當的 HTTP 標頭和回應的功能。</span><span class="sxs-lookup"><span data-stu-id="03c06-195">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="03c06-196">篩選器：</span><span class="sxs-lookup"><span data-stu-id="03c06-196">The filter:</span></span>

* <span data-ttu-id="03c06-197">移除任何現有的標頭的`Vary`， `Cache-Control`，和`Pragma`。</span><span class="sxs-lookup"><span data-stu-id="03c06-197">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="03c06-198">寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。</span><span class="sxs-lookup"><span data-stu-id="03c06-198">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="03c06-199">更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。</span><span class="sxs-lookup"><span data-stu-id="03c06-199">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="03c06-200">而有所不同</span><span class="sxs-lookup"><span data-stu-id="03c06-200">Vary</span></span>

<span data-ttu-id="03c06-201">此標頭僅寫入時`VaryByHeader`屬性設定。</span><span class="sxs-lookup"><span data-stu-id="03c06-201">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="03c06-202">設定為`Vary`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="03c06-202">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="03c06-203">下列範例會使用`VaryByHeader`屬性：</span><span class="sxs-lookup"><span data-stu-id="03c06-203">The following sample uses the `VaryByHeader` property:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

<span data-ttu-id="03c06-204">您可以檢視您的瀏覽器網路工具的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="03c06-204">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="03c06-205">下圖顯示輸出的邊緣 F12**網路**索引標籤時`About2`動作方法會重新整理：</span><span class="sxs-lookup"><span data-stu-id="03c06-205">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![About2 動作方法呼叫時，在 [網路] 索引標籤上邊緣 F12 輸出](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="03c06-207">NoStore 和 Location.None</span><span class="sxs-lookup"><span data-stu-id="03c06-207">NoStore and Location.None</span></span>

<span data-ttu-id="03c06-208">`NoStore`大部分的其他屬性會覆寫。</span><span class="sxs-lookup"><span data-stu-id="03c06-208">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="03c06-209">當這個屬性設定為`true`、`Cache-Control`標頭設定為`no-store`。</span><span class="sxs-lookup"><span data-stu-id="03c06-209">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="03c06-210">如果`Location`設`None`:</span><span class="sxs-lookup"><span data-stu-id="03c06-210">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="03c06-211">`Cache-Control` 設定為 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="03c06-211">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="03c06-212">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="03c06-212">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="03c06-213">如果`NoStore`是`false`和`Location`是`None`，`Cache-Control`和`Pragma`設為`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="03c06-213">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="03c06-214">您通常將`NoStore`至`true`錯誤頁面上。</span><span class="sxs-lookup"><span data-stu-id="03c06-214">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="03c06-215">例如: </span><span class="sxs-lookup"><span data-stu-id="03c06-215">For example:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

<span data-ttu-id="03c06-216">這會導致下列標頭：</span><span class="sxs-lookup"><span data-stu-id="03c06-216">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="03c06-217">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="03c06-217">Location and Duration</span></span>

<span data-ttu-id="03c06-218">若要啟用快取，`Duration`必須設為正值和`Location`都必須在`Any`（預設值） 或`Client`。</span><span class="sxs-lookup"><span data-stu-id="03c06-218">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="03c06-219">在此情況下，`Cache-Control`後面的位置值來設定標頭`max-age`的回應。</span><span class="sxs-lookup"><span data-stu-id="03c06-219">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="03c06-220">`Location`選項`Any`和`Client`轉譯`Cache-Control`標頭值的`public`和`private`分別。</span><span class="sxs-lookup"><span data-stu-id="03c06-220">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="03c06-221">如先前所述，設定`Location`至`None`會同時設定`Cache-Control`和`Pragma`標頭`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="03c06-221">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="03c06-222">以下範例，示範標頭所產生的設定`Duration`並讓預設`Location`值：</span><span class="sxs-lookup"><span data-stu-id="03c06-222">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

<span data-ttu-id="03c06-223">這會產生下列標頭：</span><span class="sxs-lookup"><span data-stu-id="03c06-223">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="03c06-224">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="03c06-224">Cache profiles</span></span>

<span data-ttu-id="03c06-225">而不必重複`ResponseCache`上許多的控制器動作屬性，快取設定檔的設定可以設定為選項，設定在 MVC 時`ConfigureServices`方法中的`Startup`。</span><span class="sxs-lookup"><span data-stu-id="03c06-225">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="03c06-226">參考的快取設定檔中的值會使用由預設`ResponseCache`屬性，並會覆寫屬性指定任何屬性。</span><span class="sxs-lookup"><span data-stu-id="03c06-226">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="03c06-227">設定快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="03c06-227">Setting up a cache profile:</span></span>

[!code-csharp[Main](response/sample/Startup.cs?name=snippet1)] 

<span data-ttu-id="03c06-228">參考快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="03c06-228">Referencing a cache profile:</span></span>

[!code-csharp[Main](response/sample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

<span data-ttu-id="03c06-229">`ResponseCache`屬性可以套用到動作 （方法） 和控制站 （類別）。</span><span class="sxs-lookup"><span data-stu-id="03c06-229">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="03c06-230">方法層級屬性會覆寫類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="03c06-230">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="03c06-231">在上述範例中，類別層級屬性指定持續時間為 30 秒，而方法層級屬性設定為 60 秒持續時間的參考快取設定檔。</span><span class="sxs-lookup"><span data-stu-id="03c06-231">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="03c06-232">產生的標頭：</span><span class="sxs-lookup"><span data-stu-id="03c06-232">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="03c06-233">其他資源</span><span class="sxs-lookup"><span data-stu-id="03c06-233">Additional resources</span></span>

* [<span data-ttu-id="03c06-234">快取 http 規格</span><span class="sxs-lookup"><span data-stu-id="03c06-234">Caching in HTTP from the specification</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="03c06-235">快取控制</span><span class="sxs-lookup"><span data-stu-id="03c06-235">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* [<span data-ttu-id="03c06-236">回應快取中介軟體</span><span class="sxs-lookup"><span data-stu-id="03c06-236">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
