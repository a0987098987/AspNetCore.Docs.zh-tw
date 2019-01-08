---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
ms.author: riande
ms.date: 01/07/2018
uid: performance/caching/response
ms.openlocfilehash: 5fbcaddff6e53d01a19ba8a7455c719feb614326
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098944"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="9738e-103">ASP.NET Core 中的回應快取</span><span class="sxs-lookup"><span data-stu-id="9738e-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="9738e-104">藉由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="9738e-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="9738e-105">回應快取中的 Razor 頁面是適用於 ASP.NET Core 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9738e-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="9738e-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9738e-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="9738e-107">回應快取可減少用戶端或 proxy 會向 web 伺服器提出的要求數目。</span><span class="sxs-lookup"><span data-stu-id="9738e-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="9738e-108">回應快取也可以減少 web 伺服器執行產生回應的工作。</span><span class="sxs-lookup"><span data-stu-id="9738e-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="9738e-109">回應快取是由指定您想用戶端、 proxy 和中的介軟體來快取回應標頭來控制。</span><span class="sxs-lookup"><span data-stu-id="9738e-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="9738e-110">[ResponseCache 屬性](#responsecache-attribute)參與設定快取標頭，快取的回應時，可能會接受用戶端的回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-110">The [ResponseCache attribute](#responsecache-attribute) participates in setting response caching headers, which clients may honor when caching responses.</span></span> <span data-ttu-id="9738e-111">[回應快取中介軟體](xref:performance/caching/middleware)可以用來在伺服器上的快取回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-111">[Response Caching Middleware](xref:performance/caching/middleware) can be used to cache responses on the server.</span></span> <span data-ttu-id="9738e-112">中介軟體可以使用`ResponseCache`屬性來影響伺服器端快取行為的內容。</span><span class="sxs-lookup"><span data-stu-id="9738e-112">The middleware can use `ResponseCache` attribute properties to influence server-side caching behavior.</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="9738e-113">以 HTTP 為基礎的回應快取</span><span class="sxs-lookup"><span data-stu-id="9738e-113">HTTP-based response caching</span></span>

<span data-ttu-id="9738e-114">[快取的 HTTP 1.1 規格](https://tools.ietf.org/html/rfc7234)說明網際網路快取的行為方式。</span><span class="sxs-lookup"><span data-stu-id="9738e-114">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="9738e-115">主要用於快取的 HTTP 標頭[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)，這用來指定快取*指示詞*。</span><span class="sxs-lookup"><span data-stu-id="9738e-115">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="9738e-116">要求對它們的方式從用戶端伺服器內容和回應給用戶端從伺服器進行的途中，指示詞會控制快取行為。</span><span class="sxs-lookup"><span data-stu-id="9738e-116">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="9738e-117">要求和回應移動透過 proxy 伺服器和 proxy 伺服器也必須符合 HTTP 1.1 快取規格。</span><span class="sxs-lookup"><span data-stu-id="9738e-117">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="9738e-118">常見`Cache-Control`指示詞會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="9738e-118">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="9738e-119">指示詞</span><span class="sxs-lookup"><span data-stu-id="9738e-119">Directive</span></span>                                                       | <span data-ttu-id="9738e-120">動作</span><span class="sxs-lookup"><span data-stu-id="9738e-120">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="9738e-121">public</span><span class="sxs-lookup"><span data-stu-id="9738e-121">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="9738e-122">快取可以儲存回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-122">A cache may store the response.</span></span> |
| [<span data-ttu-id="9738e-123">private</span><span class="sxs-lookup"><span data-stu-id="9738e-123">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="9738e-124">回應不能儲存的共用快取。</span><span class="sxs-lookup"><span data-stu-id="9738e-124">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="9738e-125">私用快取可以儲存和重複使用的回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-125">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="9738e-126">max-age</span><span class="sxs-lookup"><span data-stu-id="9738e-126">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="9738e-127">用戶端不會接受的回應其年齡大於指定的秒數。</span><span class="sxs-lookup"><span data-stu-id="9738e-127">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="9738e-128">例如：`max-age=60` （60 秒）， `max-age=2592000` （1 個月）</span><span class="sxs-lookup"><span data-stu-id="9738e-128">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="9738e-129">no-cache</span><span class="sxs-lookup"><span data-stu-id="9738e-129">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="9738e-130">**在要求上**:快取來滿足要求，絕不能使用的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-130">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="9738e-131">注意:原始伺服器重新產生回應的用戶端和中介軟體會更新其快取中的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-131">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="9738e-132">**在回應上**:回應不必須用於後續的要求，而不需在來源伺服器上的驗證。</span><span class="sxs-lookup"><span data-stu-id="9738e-132">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="9738e-133">no-store</span><span class="sxs-lookup"><span data-stu-id="9738e-133">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="9738e-134">**在要求上**:快取不得儲存要求。</span><span class="sxs-lookup"><span data-stu-id="9738e-134">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="9738e-135">**在回應上**:快取不得儲存回應的任何部分。</span><span class="sxs-lookup"><span data-stu-id="9738e-135">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="9738e-136">其他扮演的角色中快取的快取標頭會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="9738e-136">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="9738e-137">頁首</span><span class="sxs-lookup"><span data-stu-id="9738e-137">Header</span></span>                                                     | <span data-ttu-id="9738e-138">功能</span><span class="sxs-lookup"><span data-stu-id="9738e-138">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="9738e-139">存留期</span><span class="sxs-lookup"><span data-stu-id="9738e-139">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="9738e-140">估計的秒數之後產生回應，或在原始伺服器已成功驗證的時間量。</span><span class="sxs-lookup"><span data-stu-id="9738e-140">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="9738e-141">到期</span><span class="sxs-lookup"><span data-stu-id="9738e-141">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="9738e-142">日期/時間之後，回應會被視為過時。</span><span class="sxs-lookup"><span data-stu-id="9738e-142">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="9738e-143">Pragma</span><span class="sxs-lookup"><span data-stu-id="9738e-143">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="9738e-144">回溯相容性，使用 HTTP/1.0 會在快取設定存在`no-cache`行為。</span><span class="sxs-lookup"><span data-stu-id="9738e-144">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="9738e-145">如果`Cache-Control`標頭已存在，`Pragma`標頭被忽略。</span><span class="sxs-lookup"><span data-stu-id="9738e-145">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="9738e-146">而有所不同</span><span class="sxs-lookup"><span data-stu-id="9738e-146">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="9738e-147">指定快取的回應必須不傳送除非所有的`Vary`快取回的應的原始要求和新的要求中符合的標頭欄位。</span><span class="sxs-lookup"><span data-stu-id="9738e-147">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="9738e-148">以 HTTP 為基礎的快取方面要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="9738e-148">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="9738e-149">[Cache-control 標頭的 HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234#section-5.2)需要接受有效的快取`Cache-Control`用戶端傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="9738e-149">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="9738e-150">用戶端可要求`no-cache`標頭值並強制產生新的回應，每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="9738e-150">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="9738e-151">一律接受用戶端`Cache-Control`才有您考慮的目標 HTTP 快取要求標頭強大的意義。</span><span class="sxs-lookup"><span data-stu-id="9738e-151">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="9738e-152">正式規格，在快取的目的在於降低跨網路的用戶端、 proxy 和伺服器滿足要求的延遲和網路額外負荷。</span><span class="sxs-lookup"><span data-stu-id="9738e-152">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="9738e-153">它不一定是控制原始伺服器上的負載的方式。</span><span class="sxs-lookup"><span data-stu-id="9738e-153">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="9738e-154">沒有此快取的行為沒有開發人員控制使用時[回應快取中介軟體](xref:performance/caching/middleware)因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="9738e-154">There's no developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="9738e-155">[計劃中介軟體的增強功能](https://github.com/aspnet/AspNetCore/issues/2612)是設定來略過要求的中介軟體的好機會`Cache-Control`時決定要做為快取回的應標頭。</span><span class="sxs-lookup"><span data-stu-id="9738e-155">[Planned enhancements to the middleware](https://github.com/aspnet/AspNetCore/issues/2612) are an opportunity to configure the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="9738e-156">計劃的增強功能提供更好的控制伺服器負載的機會。</span><span class="sxs-lookup"><span data-stu-id="9738e-156">Planned enhancements provide an opportunity to better control server load.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="9738e-157">ASP.NET Core 中的其他快取技術</span><span class="sxs-lookup"><span data-stu-id="9738e-157">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="9738e-158">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="9738e-158">In-memory caching</span></span>

<span data-ttu-id="9738e-159">記憶體中快取會使用伺服器記憶體儲存快取的資料。</span><span class="sxs-lookup"><span data-stu-id="9738e-159">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="9738e-160">這種類型的快取是適用於單一伺服器或多部伺服器，使用*黏性工作階段*。</span><span class="sxs-lookup"><span data-stu-id="9738e-160">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="9738e-161">黏性工作階段，表示用戶端的要求會一律路由傳送至相同的伺服器進行處理。</span><span class="sxs-lookup"><span data-stu-id="9738e-161">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="9738e-162">如需詳細資訊，請參閱 <<c0> [ 快取於記憶體中](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="9738e-162">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="9738e-163">分散式快取</span><span class="sxs-lookup"><span data-stu-id="9738e-163">Distributed Cache</span></span>

<span data-ttu-id="9738e-164">若要將資料儲存在記憶體中，應用程式裝載在雲端或伺服器的伺服器陣列時使用分散式快取。</span><span class="sxs-lookup"><span data-stu-id="9738e-164">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="9738e-165">處理要求的伺服器之間共用快取。</span><span class="sxs-lookup"><span data-stu-id="9738e-165">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="9738e-166">用戶端可以提交的要求，如果用戶端快取的資料可用，由群組中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="9738e-166">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="9738e-167">ASP.NET Core 提供 SQL Server 和分散式的 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="9738e-167">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="9738e-168">如需詳細資訊，請參閱<xref:performance/caching/distributed>。</span><span class="sxs-lookup"><span data-stu-id="9738e-168">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="9738e-169">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="9738e-169">Cache Tag Helper</span></span>

<span data-ttu-id="9738e-170">您可以使用快取標籤協助程式，快取的 MVC 檢視或 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="9738e-170">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="9738e-171">快取標籤協助程式會使用記憶體中快取來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="9738e-171">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="9738e-172">如需詳細資訊，請參閱 < [ASP.NET Core MVC 中的快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="9738e-172">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="9738e-173">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="9738e-173">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="9738e-174">您可以使用分散式快取標籤協助程式，快取中的 MVC 檢視或分散式的雲端或 web 伺服陣列案例中的 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="9738e-174">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="9738e-175">分散式快取標籤協助程式會使用 SQL Server 或 Redis 來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="9738e-175">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="9738e-176">如需詳細資訊，請參閱 <<c0> [ 分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="9738e-176">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="9738e-177">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="9738e-177">ResponseCache attribute</span></span>

<span data-ttu-id="9738e-178">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)指定回應的快取中設定適當的標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="9738e-178">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="9738e-179">停用快取內容，其中包含已驗證的用戶端的資訊。</span><span class="sxs-lookup"><span data-stu-id="9738e-179">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="9738e-180">啟用快取應該只針對不會變更使用者的身分識別或使用者已登入為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="9738e-180">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="9738e-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)查詢金鑰之指定清單的值而異的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-181">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="9738e-182">單一值時`*`是所有的回應要求查詢字串參數提供中介軟體而異。</span><span class="sxs-lookup"><span data-stu-id="9738e-182">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="9738e-183">`VaryByQueryKeys` 需要 ASP.NET Core 1.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9738e-183">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="9738e-184">[回應快取中介軟體](xref:performance/caching/middleware)必須設定啟用`VaryByQueryKeys`屬性; 否則會擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9738e-184">[Response Caching Middleware](xref:performance/caching/middleware) must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="9738e-185">沒有對應的 HTTP 標頭，如`VaryByQueryKeys`屬性。</span><span class="sxs-lookup"><span data-stu-id="9738e-185">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="9738e-186">回應快取中介軟體處理一項 HTTP 功能屬性。</span><span class="sxs-lookup"><span data-stu-id="9738e-186">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="9738e-187">做為快取回的應中介軟體，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="9738e-187">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="9738e-188">例如，請考慮要求和下表所示的結果的順序。</span><span class="sxs-lookup"><span data-stu-id="9738e-188">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="9738e-189">要求</span><span class="sxs-lookup"><span data-stu-id="9738e-189">Request</span></span>                          | <span data-ttu-id="9738e-190">結果</span><span class="sxs-lookup"><span data-stu-id="9738e-190">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="9738e-191">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="9738e-191">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="9738e-192">傳回的中介軟體</span><span class="sxs-lookup"><span data-stu-id="9738e-192">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="9738e-193">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="9738e-193">Returned from server</span></span>     |

<span data-ttu-id="9738e-194">第一個要求是由伺服器傳回，而且快取中介軟體中。</span><span class="sxs-lookup"><span data-stu-id="9738e-194">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="9738e-195">第二個要求會傳回由中介軟體中，因為查詢字串比對前一個要求。</span><span class="sxs-lookup"><span data-stu-id="9738e-195">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="9738e-196">第三個要求不在快取中介軟體，因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="9738e-196">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="9738e-197">`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)。</span><span class="sxs-lookup"><span data-stu-id="9738e-197">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="9738e-198">`ResponseCacheFilter`之更新適當的 HTTP 標頭和回應的功能。</span><span class="sxs-lookup"><span data-stu-id="9738e-198">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="9738e-199">篩選器：</span><span class="sxs-lookup"><span data-stu-id="9738e-199">The filter:</span></span>

* <span data-ttu-id="9738e-200">移除任何現有的標頭，如`Vary`， `Cache-Control`，和`Pragma`。</span><span class="sxs-lookup"><span data-stu-id="9738e-200">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="9738e-201">寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。</span><span class="sxs-lookup"><span data-stu-id="9738e-201">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="9738e-202">更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。</span><span class="sxs-lookup"><span data-stu-id="9738e-202">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="9738e-203">而有所不同</span><span class="sxs-lookup"><span data-stu-id="9738e-203">Vary</span></span>

<span data-ttu-id="9738e-204">此標頭，才會寫入時`VaryByHeader`屬性設定。</span><span class="sxs-lookup"><span data-stu-id="9738e-204">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="9738e-205">它會設定為`Vary`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="9738e-205">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="9738e-206">下列範例會使用`VaryByHeader`屬性：</span><span class="sxs-lookup"><span data-stu-id="9738e-206">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="9738e-207">您可以檢視您的瀏覽器的網路工具的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="9738e-207">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="9738e-208">下圖顯示的輸出上的 Edge F12**網路**索引標籤時`About2`動作方法會重新整理：</span><span class="sxs-lookup"><span data-stu-id="9738e-208">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![呼叫 About2 動作方法時，在 [網路] 索引標籤上邊緣 F12 輸出](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="9738e-210">NoStore 和 Location.None</span><span class="sxs-lookup"><span data-stu-id="9738e-210">NoStore and Location.None</span></span>

<span data-ttu-id="9738e-211">`NoStore` 大部分的其他屬性會覆寫。</span><span class="sxs-lookup"><span data-stu-id="9738e-211">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="9738e-212">當這個屬性設定為`true`，則`Cache-Control`標頭設定為`no-store`。</span><span class="sxs-lookup"><span data-stu-id="9738e-212">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="9738e-213">如果`Location`設為`None`:</span><span class="sxs-lookup"><span data-stu-id="9738e-213">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="9738e-214">`Cache-Control` 設定為 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="9738e-214">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="9738e-215">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="9738e-215">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="9738e-216">如果`NoStore`是`false`並`Location`是`None`，`Cache-Control`並`Pragma`設為`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="9738e-216">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="9738e-217">您通常會設定`NoStore`至`true`錯誤頁面上。</span><span class="sxs-lookup"><span data-stu-id="9738e-217">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="9738e-218">例如: </span><span class="sxs-lookup"><span data-stu-id="9738e-218">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="9738e-219">這會導致下列標頭：</span><span class="sxs-lookup"><span data-stu-id="9738e-219">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="9738e-220">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="9738e-220">Location and Duration</span></span>

<span data-ttu-id="9738e-221">若要啟用快取`Duration`必須設為正值和`Location`必須是`Any`（預設值） 或`Client`。</span><span class="sxs-lookup"><span data-stu-id="9738e-221">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="9738e-222">在此情況下，`Cache-Control`標頭設定為後面的位置值`max-age`的回應。</span><span class="sxs-lookup"><span data-stu-id="9738e-222">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="9738e-223">`Location`選項`Any`並`Client`轉譯成`Cache-Control`標頭值`public`和`private`分別。</span><span class="sxs-lookup"><span data-stu-id="9738e-223">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="9738e-224">如先前所述，設定`Location`要`None`同時設定`Cache-Control`並`Pragma`標頭`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="9738e-224">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="9738e-225">以下範例顯示標頭產生 splittunneling`Duration`並保留預設值`Location`值：</span><span class="sxs-lookup"><span data-stu-id="9738e-225">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="9738e-226">這會產生下列標頭：</span><span class="sxs-lookup"><span data-stu-id="9738e-226">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="9738e-227">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="9738e-227">Cache profiles</span></span>

<span data-ttu-id="9738e-228">而不必重複`ResponseCache`許多的控制器動作屬性，快取設定檔上的設定可以設定為選項，設定中的 MVC 時`ConfigureServices`方法中的`Startup`。</span><span class="sxs-lookup"><span data-stu-id="9738e-228">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="9738e-229">在參考的快取設定檔中找到的值會做的預設值`ResponseCache`屬性，並會覆寫為任何屬性上指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="9738e-229">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="9738e-230">設定快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="9738e-230">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="9738e-231">參考的快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="9738e-231">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="9738e-232">`ResponseCache`可以套用屬性，包括動作 （方法），以及控制站 （類別）。</span><span class="sxs-lookup"><span data-stu-id="9738e-232">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="9738e-233">方法層級屬性會覆寫類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="9738e-233">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="9738e-234">在上述範例中，類別層級屬性會指定持續時間為 30 秒，雖然方法層級屬性會參考快取設定檔設定為 60 秒的持續時間。</span><span class="sxs-lookup"><span data-stu-id="9738e-234">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="9738e-235">產生的標頭：</span><span class="sxs-lookup"><span data-stu-id="9738e-235">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="9738e-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="9738e-236">Additional resources</span></span>

* [<span data-ttu-id="9738e-237">將回應儲存在快取</span><span class="sxs-lookup"><span data-stu-id="9738e-237">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="9738e-238">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="9738e-238">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
