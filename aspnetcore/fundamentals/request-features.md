---
title: ASP.NET Core 中的要求功能
author: ardalis
description: 了解有關 HTTP 要求和回應的網頁伺服器實作詳細資料，其定義於 ASP.NET Core 的介面中。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/request-features
ms.openlocfilehash: e26a1a7b35d40c1214bbc40269571545cbd2c235
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776021"
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="d8f0f-103">ASP.NET Core 中的要求功能</span><span class="sxs-lookup"><span data-stu-id="d8f0f-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="d8f0f-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="d8f0f-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="d8f0f-105">有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="d8f0f-106">伺服器實作與中介軟體會使用這些介面，建立及修改應用程式的裝載管線。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="d8f0f-107">功能介面</span><span class="sxs-lookup"><span data-stu-id="d8f0f-107">Feature interfaces</span></span>

<span data-ttu-id="d8f0f-108">ASP.NET Core 可定義 `Microsoft.AspNetCore.Http.Features` 中伺服器用來識別其支援功能的 HTTP 功能介面的數目。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="d8f0f-109">下列功能介面會處理要求並傳回回應：</span><span class="sxs-lookup"><span data-stu-id="d8f0f-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="d8f0f-110">`IHttpRequestFeature` 定義 HTTP 要求的結構，包括通訊協定、路徑、查詢字串、標頭和主體。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="d8f0f-111">`IHttpResponseFeature` 定義 HTTP 回應的結構，包括狀態碼、標頭和回應主體。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="d8f0f-112">`IHttpAuthenticationFeature` 定義根據 `ClaimsPrincipal` 識別使用者和指定驗證處理常式的支援。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="d8f0f-113">`IHttpUpgradeFeature` 定義 [HTTP 升級](https://tools.ietf.org/html/rfc2616.html#section-14.42)的支援，這可讓用戶端指定它在伺服器想要切換通訊協定時要使用哪些其他通訊協定。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="d8f0f-114">`IHttpBufferingFeature` 定義停用要求和/或回應之緩衝處理的方法。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="d8f0f-115">`IHttpConnectionFeature` 定義本機和遠端位址與連接埠的屬性。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="d8f0f-116">`IHttpRequestLifetimeFeature`　定義中止連線或偵測要求是否已提前終止 (例如由用戶端中斷連線) 的支援。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="d8f0f-117">`IHttpSendFileFeature` 定義以非同步方式傳送檔案的方法。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="d8f0f-118">`IHttpWebSocketFeature` 定義支援 Web 通訊端的 API。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="d8f0f-119">`IHttpRequestIdentifierFeature` 新增您可以實作的屬性來唯一識別要求。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="d8f0f-120">`ISessionFeature` 定義支援使用者工作階段的 `ISessionFactory` 和 `ISession` 抽象概念。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="d8f0f-121">`ITlsConnectionFeature` 定義擷取用戶端憑證的 API。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="d8f0f-122">`ITlsTokenBindingFeature` 定義使用 TLS 權杖繫結參數的方法。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="d8f0f-123">`ISessionFeature` 不是伺服器功能，但卻由 `SessionMiddleware` 實作 (請參閱[管理應用程式狀態](app-state.md))。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-123">`ISessionFeature` isn't a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="d8f0f-124">功能集合</span><span class="sxs-lookup"><span data-stu-id="d8f0f-124">Feature collections</span></span>

<span data-ttu-id="d8f0f-125">`HttpContext` 的 `Features` 屬性提供一個介面來取得和設定目前要求的可用 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="d8f0f-126">由於功能集合即使在要求內容中都是可變動的，因此可以使用中介軟體來修改該集合，並新增其他功能的支援。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="d8f0f-127">中介軟體和要求功能</span><span class="sxs-lookup"><span data-stu-id="d8f0f-127">Middleware and request features</span></span>

<span data-ttu-id="d8f0f-128">雖然伺服器負責建立功能集合，但中介軟體可以同時新增至此集合和取用集合中的功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="d8f0f-129">例如，`StaticFileMiddleware` 可存取 `IHttpSendFileFeature` 功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="d8f0f-130">如果有該功能，它用來從其實體路徑傳送要求的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-130">If the feature exists, it's used to send the requested static file from its physical path.</span></span> <span data-ttu-id="d8f0f-131">否則，會使用較慢的替代方法來傳送檔案。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="d8f0f-132">可用時，`IHttpSendFileFeature` 允許作業系統開啟檔案，並執行直接核心模式複製到網路卡。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="d8f0f-133">此外，中介軟體還可以新增至伺服器所建立的功能集合。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="d8f0f-134">現有的功能甚至可以取代為中介軟體，讓中介軟體來增強伺服器的功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="d8f0f-135">新增至集合的功能稍後在要求管線中，可立即用於其他中介軟體或基礎應用程式本身。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="d8f0f-136">藉由結合自訂伺服器實作和特定中介軟體增強功能，可建構應用程式所需的一組精確功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="d8f0f-137">這允許新增遺漏的功能而不需要變更伺服器，並確保只公開少量的功能，以減少受攻擊面區域並改善效能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="d8f0f-138">摘要</span><span class="sxs-lookup"><span data-stu-id="d8f0f-138">Summary</span></span>

<span data-ttu-id="d8f0f-139">功能介面定義給定的要求可能支援的特定 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="d8f0f-140">伺服器定義功能的集合，以及一組該伺服器所支援，但中介軟體可用來增強這些功能的初始功能。</span><span class="sxs-lookup"><span data-stu-id="d8f0f-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d8f0f-141">其他資源</span><span class="sxs-lookup"><span data-stu-id="d8f0f-141">Additional resources</span></span>

* [<span data-ttu-id="d8f0f-142">伺服器</span><span class="sxs-lookup"><span data-stu-id="d8f0f-142">Servers</span></span>](xref:fundamentals/servers/index)
* [<span data-ttu-id="d8f0f-143">中介軟體</span><span class="sxs-lookup"><span data-stu-id="d8f0f-143">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="d8f0f-144">Open Web Interface for .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="d8f0f-144">Open Web Interface for .NET (OWIN)</span></span>](xref:fundamentals/owin)
