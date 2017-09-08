---
title: "要求 ASP.NET 核心的功能"
author: ardalis
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: e8d04ef7df34fe1421b2c52f137511fc6baae674
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="request-features-in-aspnet-core"></a><span data-ttu-id="2a6c0-103">要求 ASP.NET 核心的功能</span><span class="sxs-lookup"><span data-stu-id="2a6c0-103">Request Features in ASP.NET Core</span></span>

<span data-ttu-id="2a6c0-104">由[Steve Smith](http://ardalis.com)</span><span class="sxs-lookup"><span data-stu-id="2a6c0-104">By [Steve Smith](http://ardalis.com)</span></span>

<span data-ttu-id="2a6c0-105">Web 伺服器實作的介面中所定義的 HTTP 要求和回應相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-105">Web server implementation details related to HTTP requests and responses are defined in interfaces.</span></span> <span data-ttu-id="2a6c0-106">伺服器實作和中介軟體會使用這些介面來建立及修改應用程式的裝載管線。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-106">These interfaces are used by server implementations and middleware to create and modify the application's hosting pipeline.</span></span>

## <a name="feature-interfaces"></a><span data-ttu-id="2a6c0-107">功能的介面</span><span class="sxs-lookup"><span data-stu-id="2a6c0-107">Feature interfaces</span></span>

<span data-ttu-id="2a6c0-108">ASP.NET Core 定義 HTTP 功能介面數目`Microsoft.AspNetCore.Http.Features`伺服器用來識別其支援的功能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-108">ASP.NET Core defines a number of HTTP feature interfaces in `Microsoft.AspNetCore.Http.Features` which are used by servers to identify the features they support.</span></span> <span data-ttu-id="2a6c0-109">下列功能的介面會處理要求，並傳回回應：</span><span class="sxs-lookup"><span data-stu-id="2a6c0-109">The following feature interfaces handle requests and return responses:</span></span>

<span data-ttu-id="2a6c0-110">`IHttpRequestFeature`定義 HTTP 要求，包括通訊協定、 路徑、 查詢字串、 標頭和主體的結構。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-110">`IHttpRequestFeature` Defines the structure of an HTTP request, including the protocol, path, query string, headers, and body.</span></span>

<span data-ttu-id="2a6c0-111">`IHttpResponseFeature`定義 HTTP 回應，包括狀態碼、 標頭和回應主體的結構。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-111">`IHttpResponseFeature` Defines the structure of an HTTP response, including the status code, headers, and body of the response.</span></span>

<span data-ttu-id="2a6c0-112">`IHttpAuthenticationFeature`定義用來根據使用者識別的支援`ClaimsPrincipal`和指定驗證處理常式。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-112">`IHttpAuthenticationFeature` Defines support for identifying users based on a `ClaimsPrincipal` and specifying an authentication handler.</span></span>

<span data-ttu-id="2a6c0-113">`IHttpUpgradeFeature`定義支援[HTTP 升級](https://tools.ietf.org/html/rfc2616.html#section-14.42)，可讓用戶端指定的其他通訊協定想要使用如果想要切換通訊協定的伺服器。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-113">`IHttpUpgradeFeature` Defines support for [HTTP Upgrades](https://tools.ietf.org/html/rfc2616.html#section-14.42), which allow the client to specify which additional protocols it would like to use if the server wishes to switch protocols.</span></span>

<span data-ttu-id="2a6c0-114">`IHttpBufferingFeature`定義用來停用緩衝處理的要求和/或回應的方法。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-114">`IHttpBufferingFeature` Defines methods for disabling buffering of requests and/or responses.</span></span>

<span data-ttu-id="2a6c0-115">`IHttpConnectionFeature`定義本機和遠端位址與連接埠的屬性。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-115">`IHttpConnectionFeature` Defines properties for local and remote addresses and ports.</span></span>

<span data-ttu-id="2a6c0-116">`IHttpRequestLifetimeFeature`定義支援中止的連線，或偵測如果要求已終止提前，例如為 「 依用戶端中斷連線。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-116">`IHttpRequestLifetimeFeature` Defines support for aborting connections, or detecting if a request has been terminated prematurely, such as by a client disconnect.</span></span>

<span data-ttu-id="2a6c0-117">`IHttpSendFileFeature`定義以非同步方式傳送檔案的方法。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-117">`IHttpSendFileFeature` Defines a method for sending files asynchronously.</span></span>

<span data-ttu-id="2a6c0-118">`IHttpWebSocketFeature`定義應用程式開發介面支援 web 通訊端。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-118">`IHttpWebSocketFeature` Defines an API for supporting web sockets.</span></span>

<span data-ttu-id="2a6c0-119">`IHttpRequestIdentifierFeature`加入您可以實作來唯一識別要求的屬性。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-119">`IHttpRequestIdentifierFeature` Adds a property that can be implemented to uniquely identify requests.</span></span>

<span data-ttu-id="2a6c0-120">`ISessionFeature`定義`ISessionFactory`和`ISession`來支援使用者工作階段的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-120">`ISessionFeature` Defines `ISessionFactory` and `ISession` abstractions for supporting user sessions.</span></span>

<span data-ttu-id="2a6c0-121">`ITlsConnectionFeature`定義應用程式開發介面來擷取用戶端憑證。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-121">`ITlsConnectionFeature` Defines an API for retrieving client certificates.</span></span>

<span data-ttu-id="2a6c0-122">`ITlsTokenBindingFeature`定義為使用 TLS 語彙基元繫結參數的方法。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-122">`ITlsTokenBindingFeature` Defines methods for working with TLS token binding parameters.</span></span>

> [!NOTE]
> <span data-ttu-id="2a6c0-123">`ISessionFeature`不是伺服器功能，而藉由`SessionMiddleware`(請參閱[管理應用程式狀態](app-state.md))。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-123">`ISessionFeature` is not a server feature, but is implemented by the `SessionMiddleware` (see [Managing Application State](app-state.md)).</span></span>

## <a name="feature-collections"></a><span data-ttu-id="2a6c0-124">功能集合</span><span class="sxs-lookup"><span data-stu-id="2a6c0-124">Feature collections</span></span>

<span data-ttu-id="2a6c0-125">`Features`屬性`HttpContext`提供介面來取得和設定可用的 HTTP 功能，目前的要求。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-125">The `Features` property of `HttpContext` provides an interface for getting and setting the available HTTP features for the current request.</span></span> <span data-ttu-id="2a6c0-126">由於功能集合是可變動的要求內容中，即使中, 介軟體可用來修改該集合，並加入其他功能的支援。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-126">Since the feature collection is mutable even within the context of a request, middleware can be used to modify the collection and add support for additional features.</span></span>

## <a name="middleware-and-request-features"></a><span data-ttu-id="2a6c0-127">中介軟體和要求的功能</span><span class="sxs-lookup"><span data-stu-id="2a6c0-127">Middleware and request features</span></span>

<span data-ttu-id="2a6c0-128">雖然伺服器負責建立功能集合中, 介軟體新增到此集合和取用集合中的功能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-128">While servers are responsible for creating the feature collection, middleware can both add to this collection and consume features from the collection.</span></span> <span data-ttu-id="2a6c0-129">例如，`StaticFileMiddleware`存取`IHttpSendFileFeature`功能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-129">For example, the `StaticFileMiddleware` accesses the `IHttpSendFileFeature` feature.</span></span> <span data-ttu-id="2a6c0-130">如果有此功能，它用來傳送要求的靜態檔案從其實體路徑。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-130">If the feature exists, it is used to send the requested static file from its physical path.</span></span> <span data-ttu-id="2a6c0-131">否則，較慢的替代方法用來傳送檔案。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-131">Otherwise, a slower alternative method is used to send the file.</span></span> <span data-ttu-id="2a6c0-132">如果有的話，`IHttpSendFileFeature`允許開啟檔案，並執行直接核心模式複製到網路卡的作業系統。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-132">When available, the `IHttpSendFileFeature` allows the operating system to open the file and perform a direct kernel mode copy to the network card.</span></span>

<span data-ttu-id="2a6c0-133">此外中, 介軟體可以新增至伺服器所建立的功能集合。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-133">Additionally, middleware can add to the feature collection established by the server.</span></span> <span data-ttu-id="2a6c0-134">即使可以由中介軟體，讓中介軟體來加強伺服器的功能取代現有的功能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-134">Existing features can even be replaced by middleware, allowing the middleware to augment the functionality of the server.</span></span> <span data-ttu-id="2a6c0-135">加入至集合的功能可立即用於其他中介軟體或基礎應用程式本身要求管線中的更新版本。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-135">Features added to the collection are available immediately to other middleware or the underlying application itself later in the request pipeline.</span></span>

<span data-ttu-id="2a6c0-136">藉由結合自訂伺服器實作和特定中介軟體的增強功能，可用於建構精確的應用程式需要的功能集。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-136">By combining custom server implementations and specific middleware enhancements, the precise set of features an application requires can be constructed.</span></span> <span data-ttu-id="2a6c0-137">這可以讓遺漏要加入，而不需要在伺服器中，變更的功能，可確保只有少量的功能會公開，以減少攻擊介面區，並改善效能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-137">This allows missing features to be added without requiring a change in server, and ensures only the minimal amount of features are exposed, thus limiting attack surface area and improving performance.</span></span>

## <a name="summary"></a><span data-ttu-id="2a6c0-138">總結</span><span class="sxs-lookup"><span data-stu-id="2a6c0-138">Summary</span></span>

<span data-ttu-id="2a6c0-139">功能的介面定義給定的要求可能支援的特定 HTTP 功能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-139">Feature interfaces define specific HTTP features that a given request may support.</span></span> <span data-ttu-id="2a6c0-140">伺服器定義集合的功能，以及一組初始的該伺服器所支援的功能，但中介軟體可以用來增強這些功能。</span><span class="sxs-lookup"><span data-stu-id="2a6c0-140">Servers define collections of features, and the initial set of features supported by that server, but middleware can be used to enhance these features.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a6c0-141">其他資源</span><span class="sxs-lookup"><span data-stu-id="2a6c0-141">Additional Resources</span></span>

* [<span data-ttu-id="2a6c0-142">伺服器</span><span class="sxs-lookup"><span data-stu-id="2a6c0-142">Servers</span></span>](servers/index.md)

* [<span data-ttu-id="2a6c0-143">中介軟體</span><span class="sxs-lookup"><span data-stu-id="2a6c0-143">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="2a6c0-144">開啟 Web 介面 for.NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="2a6c0-144">Open Web Interface for .NET (OWIN)</span></span>](owin.md)
