---
title: ASP.NET Core 中的回應快取
author: rick-anderson
description: 了解如何使用回應快取來降低頻寬需求，並提升 ASP.NET Core 應用程式的效能。
ms.author: riande
ms.date: 09/20/2017
uid: performance/caching/response
ms.openlocfilehash: bbf5b649bac9d31aa6d0ecdc3828648677b05716
ms.sourcegitcommit: 4d74644f11e0dac52b4510048490ae731c691496
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/25/2018
ms.locfileid: "50090689"
---
# <a name="response-caching-in-aspnet-core"></a><span data-ttu-id="8d0a9-103">ASP.NET Core 中的回應快取</span><span class="sxs-lookup"><span data-stu-id="8d0a9-103">Response caching in ASP.NET Core</span></span>

<span data-ttu-id="8d0a9-104">藉由[John Luo](https://github.com/JunTaoLuo)， [Rick Anderson](https://twitter.com/RickAndMSFT)， [Steve Smith](https://ardalis.com/)，和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="8d0a9-104">By [John Luo](https://github.com/JunTaoLuo), [Rick Anderson](https://twitter.com/RickAndMSFT), [Steve Smith](https://ardalis.com/), and [Luke Latham](https://github.com/guardrex)</span></span>

> [!NOTE]
> <span data-ttu-id="8d0a9-105">回應快取中的 Razor 頁面是適用於 ASP.NET Core 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-105">Response caching in Razor Pages is available in ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="8d0a9-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8d0a9-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/caching/response/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="8d0a9-107">回應快取可減少用戶端或 proxy 會向 web 伺服器提出的要求數目。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-107">Response caching reduces the number of requests a client or proxy makes to a web server.</span></span> <span data-ttu-id="8d0a9-108">回應快取也可以減少 web 伺服器執行產生回應的工作。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-108">Response caching also reduces the amount of work the web server performs to generate a response.</span></span> <span data-ttu-id="8d0a9-109">回應快取是由指定您想用戶端、 proxy 和中的介軟體來快取回應標頭來控制。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-109">Response caching is controlled by headers that specify how you want client, proxy, and middleware to cache responses.</span></span>

<span data-ttu-id="8d0a9-110">Web 伺服器可以快取的回應，當您將新增[回應快取中介軟體](xref:performance/caching/middleware)。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-110">The web server can cache responses when you add [Response Caching Middleware](xref:performance/caching/middleware).</span></span>

## <a name="http-based-response-caching"></a><span data-ttu-id="8d0a9-111">以 HTTP 為基礎的回應快取</span><span class="sxs-lookup"><span data-stu-id="8d0a9-111">HTTP-based response caching</span></span>

<span data-ttu-id="8d0a9-112">[快取的 HTTP 1.1 規格](https://tools.ietf.org/html/rfc7234)說明網際網路快取的行為方式。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-112">The [HTTP 1.1 Caching specification](https://tools.ietf.org/html/rfc7234) describes how Internet caches should behave.</span></span> <span data-ttu-id="8d0a9-113">主要用於快取的 HTTP 標頭[Cache-control](https://tools.ietf.org/html/rfc7234#section-5.2)，這用來指定快取*指示詞*。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-113">The primary HTTP header used for caching is [Cache-Control](https://tools.ietf.org/html/rfc7234#section-5.2), which is used to specify cache *directives*.</span></span> <span data-ttu-id="8d0a9-114">要求對它們的方式從用戶端伺服器內容和回應給用戶端從伺服器進行的途中，指示詞會控制快取行為。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-114">The directives control caching behavior as requests make their way from clients to servers and as reponses make their way from servers back to clients.</span></span> <span data-ttu-id="8d0a9-115">要求和回應移動透過 proxy 伺服器和 proxy 伺服器也必須符合 HTTP 1.1 快取規格。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-115">Requests and responses move through proxy servers, and proxy servers must also conform to the HTTP 1.1 Caching specification.</span></span>

<span data-ttu-id="8d0a9-116">常見`Cache-Control`指示詞會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-116">Common `Cache-Control` directives are shown in the following table.</span></span>

| <span data-ttu-id="8d0a9-117">指示詞</span><span class="sxs-lookup"><span data-stu-id="8d0a9-117">Directive</span></span>                                                       | <span data-ttu-id="8d0a9-118">動作</span><span class="sxs-lookup"><span data-stu-id="8d0a9-118">Action</span></span> |
| --------------------------------------------------------------- | ------ |
| [<span data-ttu-id="8d0a9-119">public</span><span class="sxs-lookup"><span data-stu-id="8d0a9-119">public</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.5)   | <span data-ttu-id="8d0a9-120">快取可以儲存回應。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-120">A cache may store the response.</span></span> |
| [<span data-ttu-id="8d0a9-121">private</span><span class="sxs-lookup"><span data-stu-id="8d0a9-121">private</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.2.6)  | <span data-ttu-id="8d0a9-122">回應不能儲存的共用快取。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-122">The response must not be stored by a shared cache.</span></span> <span data-ttu-id="8d0a9-123">私用快取可以儲存和重複使用的回應。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-123">A private cache may store and reuse the response.</span></span> |
| [<span data-ttu-id="8d0a9-124">max-age</span><span class="sxs-lookup"><span data-stu-id="8d0a9-124">max-age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.1)  | <span data-ttu-id="8d0a9-125">用戶端不會接受的回應其年齡大於指定的秒數。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-125">The client won't accept a response whose age is greater than the specified number of seconds.</span></span> <span data-ttu-id="8d0a9-126">範例： `max-age=60` （60 秒）， `max-age=2592000` （1 個月）</span><span class="sxs-lookup"><span data-stu-id="8d0a9-126">Examples: `max-age=60` (60 seconds), `max-age=2592000` (1 month)</span></span> |
| [<span data-ttu-id="8d0a9-127">no-cache</span><span class="sxs-lookup"><span data-stu-id="8d0a9-127">no-cache</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.4) | <span data-ttu-id="8d0a9-128">**在要求上**： 快取必須使用預存的回應來滿足要求。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-128">**On requests**: A cache must not use a stored response to satisfy the request.</span></span> <span data-ttu-id="8d0a9-129">注意： 用戶端中，原始伺服器重新產生的回應和中介軟體會更新其快取中的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-129">Note: The origin server re-generates the response for the client, and the middleware updates the stored response in its cache.</span></span><br><br><span data-ttu-id="8d0a9-130">**在回應上**： 回應絕不會用於後續的要求，而不需在來源伺服器上的驗證。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-130">**On responses**: The response must not be used for a subsequent request without validation on the origin server.</span></span> |
| [<span data-ttu-id="8d0a9-131">no-store</span><span class="sxs-lookup"><span data-stu-id="8d0a9-131">no-store</span></span>](https://tools.ietf.org/html/rfc7234#section-5.2.1.5) | <span data-ttu-id="8d0a9-132">**在要求上**： 快取不得儲存要求。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-132">**On requests**: A cache must not store the request.</span></span><br><br><span data-ttu-id="8d0a9-133">**在回應上**： 快取不得儲存回應的任何部分。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-133">**On responses**: A cache must not store any part of the response.</span></span> |

<span data-ttu-id="8d0a9-134">其他扮演的角色中快取的快取標頭會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-134">Other cache headers that play a role in caching are shown in the following table.</span></span>

| <span data-ttu-id="8d0a9-135">頁首</span><span class="sxs-lookup"><span data-stu-id="8d0a9-135">Header</span></span>                                                     | <span data-ttu-id="8d0a9-136">功能</span><span class="sxs-lookup"><span data-stu-id="8d0a9-136">Function</span></span> |
| ---------------------------------------------------------- | -------- |
| [<span data-ttu-id="8d0a9-137">存留期</span><span class="sxs-lookup"><span data-stu-id="8d0a9-137">Age</span></span>](https://tools.ietf.org/html/rfc7234#section-5.1)     | <span data-ttu-id="8d0a9-138">估計的秒數之後產生回應，或在原始伺服器已成功驗證的時間量。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-138">An estimate of the amount of time in seconds since the response was generated or successfully validated at the origin server.</span></span> |
| [<span data-ttu-id="8d0a9-139">到期</span><span class="sxs-lookup"><span data-stu-id="8d0a9-139">Expires</span></span>](https://tools.ietf.org/html/rfc7234#section-5.3) | <span data-ttu-id="8d0a9-140">日期/時間之後，回應會被視為過時。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-140">The date/time after which the response is considered stale.</span></span> |
| [<span data-ttu-id="8d0a9-141">Pragma</span><span class="sxs-lookup"><span data-stu-id="8d0a9-141">Pragma</span></span>](https://tools.ietf.org/html/rfc7234#section-5.4)  | <span data-ttu-id="8d0a9-142">回溯相容性，使用 HTTP/1.0 會在快取設定存在`no-cache`行為。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-142">Exists for backwards compatibility with HTTP/1.0 caches for setting `no-cache` behavior.</span></span> <span data-ttu-id="8d0a9-143">如果`Cache-Control`標頭已存在，`Pragma`標頭被忽略。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-143">If the `Cache-Control` header is present, the `Pragma` header is ignored.</span></span> |
| [<span data-ttu-id="8d0a9-144">而有所不同</span><span class="sxs-lookup"><span data-stu-id="8d0a9-144">Vary</span></span>](https://tools.ietf.org/html/rfc7231#section-7.1.4)  | <span data-ttu-id="8d0a9-145">指定快取的回應必須不傳送除非所有的`Vary`快取回的應的原始要求和新的要求中符合的標頭欄位。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-145">Specifies that a cached response must not be sent unless all of the `Vary` header fields match in both the cached response's original request and the new request.</span></span> |

## <a name="http-based-caching-respects-request-cache-control-directives"></a><span data-ttu-id="8d0a9-146">以 HTTP 為基礎的快取方面要求快取控制指示詞</span><span class="sxs-lookup"><span data-stu-id="8d0a9-146">HTTP-based caching respects request Cache-Control directives</span></span>

<span data-ttu-id="8d0a9-147">[Cache-control 標頭的 HTTP 1.1 快取規格](https://tools.ietf.org/html/rfc7234#section-5.2)需要接受有效的快取`Cache-Control`用戶端傳送的標頭。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-147">The [HTTP 1.1 Caching specification for the Cache-Control header](https://tools.ietf.org/html/rfc7234#section-5.2) requires a cache to honor a valid `Cache-Control` header sent by the client.</span></span> <span data-ttu-id="8d0a9-148">用戶端可要求`no-cache`標頭值並強制產生新的回應，每個要求的伺服器。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-148">A client can make requests with a `no-cache` header value and force the server to generate a new response for every request.</span></span>

<span data-ttu-id="8d0a9-149">一律接受用戶端`Cache-Control`才有您考慮的目標 HTTP 快取要求標頭強大的意義。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-149">Always honoring client `Cache-Control` request headers makes sense if you consider the goal of HTTP caching.</span></span> <span data-ttu-id="8d0a9-150">正式規格，在快取的目的在於降低跨網路的用戶端、 proxy 和伺服器滿足要求的延遲和網路額外負荷。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-150">Under the official specification, caching is meant to reduce the latency and network overhead of satisfying requests across a network of clients, proxies, and servers.</span></span> <span data-ttu-id="8d0a9-151">它不一定是控制原始伺服器上的負載的方式。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-151">It isn't necessarily a way to control the load on an origin server.</span></span>

<span data-ttu-id="8d0a9-152">沒有任何目前開發人員控制這個快取的行為時使用[回應快取中介軟體](xref:performance/caching/middleware)因為中介軟體會遵守官方快取規格。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-152">There's no current developer control over this caching behavior when using the [Response Caching Middleware](xref:performance/caching/middleware) because the middleware adheres to the official caching specification.</span></span> <span data-ttu-id="8d0a9-153">[未來的增強功能中, 介軟體](https://github.com/aspnet/ResponseCaching/issues/96)允許設定要略過要求的中介軟體`Cache-Control`時決定要做為快取回的應標頭。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-153">[Future enhancements to the middleware](https://github.com/aspnet/ResponseCaching/issues/96) will permit configuring the middleware to ignore a request's `Cache-Control` header when deciding to serve a cached response.</span></span> <span data-ttu-id="8d0a9-154">這會讓您將以您的伺服器更能控制負載時使用的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-154">This will offer you an opportunity to better control the load on your server when you use the middleware.</span></span>

## <a name="other-caching-technology-in-aspnet-core"></a><span data-ttu-id="8d0a9-155">ASP.NET Core 中的其他快取技術</span><span class="sxs-lookup"><span data-stu-id="8d0a9-155">Other caching technology in ASP.NET Core</span></span>

### <a name="in-memory-caching"></a><span data-ttu-id="8d0a9-156">記憶體中快取</span><span class="sxs-lookup"><span data-stu-id="8d0a9-156">In-memory caching</span></span>

<span data-ttu-id="8d0a9-157">記憶體中快取會使用伺服器記憶體儲存快取的資料。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-157">In-memory caching uses server memory to store cached data.</span></span> <span data-ttu-id="8d0a9-158">這種類型的快取是適用於單一伺服器或多部伺服器，使用*黏性工作階段*。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-158">This type of caching is suitable for a single server or multiple servers using *sticky sessions*.</span></span> <span data-ttu-id="8d0a9-159">黏性工作階段，表示用戶端的要求會一律路由傳送至相同的伺服器進行處理。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-159">Sticky sessions means that the requests from a client are always routed to the same server for processing.</span></span>

<span data-ttu-id="8d0a9-160">如需詳細資訊，請參閱 <<c0> [ 快取於記憶體中](xref:performance/caching/memory)。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-160">For more information, see [Cache in-memory](xref:performance/caching/memory).</span></span>

### <a name="distributed-cache"></a><span data-ttu-id="8d0a9-161">分散式快取</span><span class="sxs-lookup"><span data-stu-id="8d0a9-161">Distributed Cache</span></span>

<span data-ttu-id="8d0a9-162">若要將資料儲存在記憶體中，應用程式裝載在雲端或伺服器的伺服器陣列時使用分散式快取。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-162">Use a distributed cache to store data in memory when the app is hosted in a cloud or server farm.</span></span> <span data-ttu-id="8d0a9-163">處理要求的伺服器之間共用快取。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-163">The cache is shared across the servers that process requests.</span></span> <span data-ttu-id="8d0a9-164">用戶端可以提交的要求，如果用戶端快取的資料可用，由群組中的任何伺服器。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-164">A client can submit a request that's handled by any server in the group if cached data for the client is available.</span></span> <span data-ttu-id="8d0a9-165">ASP.NET Core 提供 SQL Server 和分散式的 Redis 快取。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-165">ASP.NET Core offers SQL Server and Redis distributed caches.</span></span>

<span data-ttu-id="8d0a9-166">如需詳細資訊，請參閱<xref:performance/caching/distributed>。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-166">For more information, see <xref:performance/caching/distributed>.</span></span>

### <a name="cache-tag-helper"></a><span data-ttu-id="8d0a9-167">快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="8d0a9-167">Cache Tag Helper</span></span>

<span data-ttu-id="8d0a9-168">您可以使用快取標籤協助程式，快取的 MVC 檢視或 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-168">You can cache the content from an MVC view or Razor Page with the Cache Tag Helper.</span></span> <span data-ttu-id="8d0a9-169">快取標籤協助程式會使用記憶體中快取來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-169">The Cache Tag Helper uses in-memory caching to store data.</span></span>

<span data-ttu-id="8d0a9-170">如需詳細資訊，請參閱 < [ASP.NET Core MVC 中的快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-170">For more information, see [Cache Tag Helper in ASP.NET Core MVC](xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper).</span></span>

### <a name="distributed-cache-tag-helper"></a><span data-ttu-id="8d0a9-171">分散式快取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="8d0a9-171">Distributed Cache Tag Helper</span></span>

<span data-ttu-id="8d0a9-172">您可以使用分散式快取標籤協助程式，快取中的 MVC 檢視或分散式的雲端或 web 伺服陣列案例中的 Razor 頁面的內容。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-172">You can cache the content from an MVC view or Razor Page in distributed cloud or web farm scenarios with the Distributed Cache Tag Helper.</span></span> <span data-ttu-id="8d0a9-173">分散式快取標籤協助程式會使用 SQL Server 或 Redis 來儲存資料。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-173">The Distributed Cache Tag Helper uses SQL Server or Redis to store data.</span></span>

<span data-ttu-id="8d0a9-174">如需詳細資訊，請參閱 <<c0> [ 分散式快取標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-174">For more information, see [Distributed Cache Tag Helper](xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper).</span></span>

## <a name="responsecache-attribute"></a><span data-ttu-id="8d0a9-175">ResponseCache 屬性</span><span class="sxs-lookup"><span data-stu-id="8d0a9-175">ResponseCache attribute</span></span>

<span data-ttu-id="8d0a9-176">[ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute)指定回應的快取中設定適當的標頭所需的參數。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-176">The [ResponseCacheAttribute](/dotnet/api/Microsoft.AspNetCore.Mvc.ResponseCacheAttribute) specifies the parameters necessary for setting appropriate headers in response caching.</span></span>

> [!WARNING]
> <span data-ttu-id="8d0a9-177">停用快取內容，其中包含已驗證的用戶端的資訊。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-177">Disable caching for content that contains information for authenticated clients.</span></span> <span data-ttu-id="8d0a9-178">啟用快取應該只針對不會變更使用者的身分識別或使用者已登入為基礎的內容。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-178">Caching should only be enabled for content that doesn't change based on a user's identity or whether a user is signed in.</span></span>

<span data-ttu-id="8d0a9-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys)查詢金鑰之指定清單的值而異的預存的回應。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-179">[VaryByQueryKeys](/dotnet/api/microsoft.aspnetcore.mvc.responsecacheattribute.varybyquerykeys) varies the stored response by the values of the given list of query keys.</span></span> <span data-ttu-id="8d0a9-180">單一值時`*`是所有的回應要求查詢字串參數提供中介軟體而異。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-180">When a single value of `*` is provided, the middleware varies responses by all request query string parameters.</span></span> <span data-ttu-id="8d0a9-181">`VaryByQueryKeys` 需要 ASP.NET Core 1.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-181">`VaryByQueryKeys` requires ASP.NET Core 1.1 or later.</span></span>

<span data-ttu-id="8d0a9-182">必須啟用回應快取中介軟體，才能設定`VaryByQueryKeys`屬性; 否則會擲回執行階段例外狀況。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-182">The Response Caching Middleware must be enabled to set the `VaryByQueryKeys` property; otherwise, a runtime exception is thrown.</span></span> <span data-ttu-id="8d0a9-183">沒有對應的 HTTP 標頭，如`VaryByQueryKeys`屬性。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-183">There isn't a corresponding HTTP header for the `VaryByQueryKeys` property.</span></span> <span data-ttu-id="8d0a9-184">回應快取中介軟體處理一項 HTTP 功能屬性。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-184">The property is an HTTP feature handled by the Response Caching Middleware.</span></span> <span data-ttu-id="8d0a9-185">做為快取回的應中介軟體，查詢字串和查詢字串值必須符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-185">For the middleware to serve a cached response, the query string and query string value must match a previous request.</span></span> <span data-ttu-id="8d0a9-186">例如，請考慮要求和下表所示的結果的順序。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-186">For example, consider the sequence of requests and results shown in the following table.</span></span>

| <span data-ttu-id="8d0a9-187">要求</span><span class="sxs-lookup"><span data-stu-id="8d0a9-187">Request</span></span>                          | <span data-ttu-id="8d0a9-188">結果</span><span class="sxs-lookup"><span data-stu-id="8d0a9-188">Result</span></span>                   |
| -------------------------------- | ------------------------ |
| `http://example.com?key1=value1` | <span data-ttu-id="8d0a9-189">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="8d0a9-189">Returned from server</span></span>     |
| `http://example.com?key1=value1` | <span data-ttu-id="8d0a9-190">傳回的中介軟體</span><span class="sxs-lookup"><span data-stu-id="8d0a9-190">Returned from middleware</span></span> |
| `http://example.com?key1=value2` | <span data-ttu-id="8d0a9-191">從伺服器傳回</span><span class="sxs-lookup"><span data-stu-id="8d0a9-191">Returned from server</span></span>     |

<span data-ttu-id="8d0a9-192">第一個要求是由伺服器傳回，而且快取中介軟體中。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-192">The first request is returned by the server and cached in middleware.</span></span> <span data-ttu-id="8d0a9-193">第二個要求會傳回由中介軟體中，因為查詢字串比對前一個要求。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-193">The second request is returned by middleware because the query string matches the previous request.</span></span> <span data-ttu-id="8d0a9-194">第三個要求不在快取中介軟體，因為查詢字串值不符合先前的要求。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-194">The third request isn't in the middleware cache because the query string value doesn't match a previous request.</span></span> 

<span data-ttu-id="8d0a9-195">`ResponseCacheAttribute`用來設定並建立 (透過`IFilterFactory`) [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter)。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-195">The `ResponseCacheAttribute` is used to configure and create (via `IFilterFactory`) a [ResponseCacheFilter](/dotnet/api/microsoft.aspnetcore.mvc.internal.responsecachefilter).</span></span> <span data-ttu-id="8d0a9-196">`ResponseCacheFilter`之更新適當的 HTTP 標頭和回應的功能。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-196">The `ResponseCacheFilter` performs the work of updating the appropriate HTTP headers and features of the response.</span></span> <span data-ttu-id="8d0a9-197">篩選器：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-197">The filter:</span></span>

* <span data-ttu-id="8d0a9-198">移除任何現有的標頭，如`Vary`， `Cache-Control`，和`Pragma`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-198">Removes any existing headers for `Vary`, `Cache-Control`, and `Pragma`.</span></span> 
* <span data-ttu-id="8d0a9-199">寫出在設定的屬性為根據適當的標頭`ResponseCacheAttribute`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-199">Writes out the appropriate headers based on the properties set in the `ResponseCacheAttribute`.</span></span> 
* <span data-ttu-id="8d0a9-200">更新快取 HTTP 功能，如果回應`VaryByQueryKeys`設定。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-200">Updates the response caching HTTP feature if `VaryByQueryKeys` is set.</span></span>

### <a name="vary"></a><span data-ttu-id="8d0a9-201">而有所不同</span><span class="sxs-lookup"><span data-stu-id="8d0a9-201">Vary</span></span>

<span data-ttu-id="8d0a9-202">此標頭，才會寫入時`VaryByHeader`屬性設定。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-202">This header is only written when the `VaryByHeader` property is set.</span></span> <span data-ttu-id="8d0a9-203">它會設定為`Vary`屬性的值。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-203">It's set to the `Vary` property's value.</span></span> <span data-ttu-id="8d0a9-204">下列範例會使用`VaryByHeader`屬性：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-204">The following sample uses the `VaryByHeader` property:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_VaryByHeader&highlight=1)]

::: moniker-end

<span data-ttu-id="8d0a9-205">您可以檢視您的瀏覽器的網路工具的回應標頭。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-205">You can view the response headers with your browser's network tools.</span></span> <span data-ttu-id="8d0a9-206">下圖顯示的輸出上的 Edge F12**網路**索引標籤時`About2`動作方法會重新整理：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-206">The following image shows the Edge F12 output on the **Network** tab when the `About2` action method is refreshed:</span></span>

![呼叫 About2 動作方法時，在 [網路] 索引標籤上邊緣 F12 輸出](response/_static/vary.png)

### <a name="nostore-and-locationnone"></a><span data-ttu-id="8d0a9-208">NoStore 和 Location.None</span><span class="sxs-lookup"><span data-stu-id="8d0a9-208">NoStore and Location.None</span></span>

<span data-ttu-id="8d0a9-209">`NoStore` 大部分的其他屬性會覆寫。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-209">`NoStore` overrides most of the other properties.</span></span> <span data-ttu-id="8d0a9-210">當這個屬性設定為`true`，則`Cache-Control`標頭設定為`no-store`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-210">When this property is set to `true`, the `Cache-Control` header is set to `no-store`.</span></span> <span data-ttu-id="8d0a9-211">如果`Location`設為`None`:</span><span class="sxs-lookup"><span data-stu-id="8d0a9-211">If `Location` is set to `None`:</span></span>

* <span data-ttu-id="8d0a9-212">`Cache-Control` 設定為 `no-store,no-cache`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-212">`Cache-Control` is set to `no-store,no-cache`.</span></span>
* <span data-ttu-id="8d0a9-213">`Pragma` 設定為 `no-cache`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-213">`Pragma` is set to `no-cache`.</span></span>

<span data-ttu-id="8d0a9-214">如果`NoStore`是`false`並`Location`是`None`，`Cache-Control`並`Pragma`設為`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-214">If `NoStore` is `false` and `Location` is `None`, `Cache-Control` and `Pragma` are set to `no-cache`.</span></span>

<span data-ttu-id="8d0a9-215">您通常會設定`NoStore`至`true`錯誤頁面上。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-215">You typically set `NoStore` to `true` on error pages.</span></span> <span data-ttu-id="8d0a9-216">例如: </span><span class="sxs-lookup"><span data-stu-id="8d0a9-216">For example:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet1&highlight=1)]

::: moniker-end

<span data-ttu-id="8d0a9-217">這會導致下列標頭：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-217">This results in the following headers:</span></span>

```
Cache-Control: no-store,no-cache
Pragma: no-cache
```

### <a name="location-and-duration"></a><span data-ttu-id="8d0a9-218">位置和持續時間</span><span class="sxs-lookup"><span data-stu-id="8d0a9-218">Location and Duration</span></span>

<span data-ttu-id="8d0a9-219">若要啟用快取`Duration`必須設為正值和`Location`必須是`Any`（預設值） 或`Client`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-219">To enable caching, `Duration` must be set to a positive value and `Location` must be either `Any` (the default) or `Client`.</span></span> <span data-ttu-id="8d0a9-220">在此情況下，`Cache-Control`標頭設定為後面的位置值`max-age`的回應。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-220">In this case, the `Cache-Control` header is set to the location value followed by the `max-age` of the response.</span></span>

> [!NOTE]
> <span data-ttu-id="8d0a9-221">`Location`選項`Any`並`Client`轉譯成`Cache-Control`標頭值`public`和`private`分別。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-221">`Location`'s options of `Any` and `Client` translate into `Cache-Control` header values of `public` and `private`, respectively.</span></span> <span data-ttu-id="8d0a9-222">如先前所述，設定`Location`要`None`同時設定`Cache-Control`並`Pragma`標頭`no-cache`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-222">As noted previously, setting `Location` to `None` sets both `Cache-Control` and `Pragma` headers to `no-cache`.</span></span>

<span data-ttu-id="8d0a9-223">以下範例顯示標頭產生 splittunneling`Duration`並保留預設值`Location`值：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-223">Below is an example showing the headers produced by setting `Duration` and leaving the default `Location` value:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_duration&highlight=1)]

::: moniker-end

<span data-ttu-id="8d0a9-224">這會產生下列標頭：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-224">This produces the following header:</span></span>

```
Cache-Control: public,max-age=60
```

### <a name="cache-profiles"></a><span data-ttu-id="8d0a9-225">快取設定檔</span><span class="sxs-lookup"><span data-stu-id="8d0a9-225">Cache profiles</span></span>

<span data-ttu-id="8d0a9-226">而不必重複`ResponseCache`許多的控制器動作屬性，快取設定檔上的設定可以設定為選項，設定中的 MVC 時`ConfigureServices`方法中的`Startup`。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-226">Instead of duplicating `ResponseCache` settings on many controller action attributes, cache profiles can be configured as options when setting up MVC in the `ConfigureServices` method in `Startup`.</span></span> <span data-ttu-id="8d0a9-227">在參考的快取設定檔中找到的值會做的預設值`ResponseCache`屬性，並會覆寫為任何屬性上指定的屬性。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-227">Values found in a referenced cache profile are used as the defaults by the `ResponseCache` attribute and are overridden by any properties specified on the attribute.</span></span>

<span data-ttu-id="8d0a9-228">設定快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-228">Setting up a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Startup.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="8d0a9-229">參考的快取設定檔：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-229">Referencing a cache profile:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](response/samples/2.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response/samples/1.x/ResponseCacheSample/Controllers/HomeController.cs?name=snippet_controller&highlight=1,4)]

::: moniker-end

<span data-ttu-id="8d0a9-230">`ResponseCache`可以套用屬性，包括動作 （方法），以及控制站 （類別）。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-230">The `ResponseCache` attribute can be applied both to actions (methods) and controllers (classes).</span></span> <span data-ttu-id="8d0a9-231">方法層級屬性會覆寫類別層級屬性中指定的設定。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-231">Method-level attributes override the settings specified in class-level attributes.</span></span>

<span data-ttu-id="8d0a9-232">在上述範例中，類別層級屬性會指定持續時間為 30 秒，雖然方法層級屬性會參考快取設定檔設定為 60 秒的持續時間。</span><span class="sxs-lookup"><span data-stu-id="8d0a9-232">In the above example, a class-level attribute specifies a duration of 30 seconds, while a method-level attribute references a cache profile with a duration set to 60 seconds.</span></span>

<span data-ttu-id="8d0a9-233">產生的標頭：</span><span class="sxs-lookup"><span data-stu-id="8d0a9-233">The resulting header:</span></span>

```
Cache-Control: public,max-age=60
```

## <a name="additional-resources"></a><span data-ttu-id="8d0a9-234">其他資源</span><span class="sxs-lookup"><span data-stu-id="8d0a9-234">Additional resources</span></span>

* [<span data-ttu-id="8d0a9-235">將回應儲存在快取</span><span class="sxs-lookup"><span data-stu-id="8d0a9-235">Storing Responses in Caches</span></span>](https://tools.ietf.org/html/rfc7234#section-3)
* [<span data-ttu-id="8d0a9-236">Cache-Control</span><span class="sxs-lookup"><span data-stu-id="8d0a9-236">Cache-Control</span></span>](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.9)
* <xref:performance/caching/memory>
* <xref:performance/caching/distributed>
* <xref:fundamentals/change-tokens>
* <xref:performance/caching/middleware>
* <xref:mvc/views/tag-helpers/builtin-th/cache-tag-helper>
* <xref:mvc/views/tag-helpers/builtin-th/distributed-cache-tag-helper>
