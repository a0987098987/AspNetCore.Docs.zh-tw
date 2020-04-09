---
title: 比較 gRPC 服務與 HTTP API
author: jamesnk
description: 瞭解 gRPC 與 HTTP API 的比較情況,以及建議的方案是什麼。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 12/05/2019
no-loc:
- SignalR
uid: grpc/comparison
ms.openlocfilehash: 2dff64f1f2d67b8a1e676acf6cf131b684099750
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80405875"
---
# <a name="compare-grpc-services-with-http-apis"></a><span data-ttu-id="8ffdb-103">比較 gRPC 服務與 HTTP API</span><span class="sxs-lookup"><span data-stu-id="8ffdb-103">Compare gRPC services with HTTP APIs</span></span>

<span data-ttu-id="8ffdb-104">由[詹姆斯·牛頓-金](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="8ffdb-105">本文介紹了[gRPC 服務](https://grpc.io/docs/guides/)如何與 HTTP API(包括 ASP.NET 核心[Web API](xref:web-api/index)進行比較。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-105">This article explains how [gRPC services](https://grpc.io/docs/guides/) compare to HTTP APIs (including ASP.NET Core [web APIs](xref:web-api/index)).</span></span> <span data-ttu-id="8ffdb-106">用於為應用提供 API 的技術是一個重要的選擇,gRPC 與 HTTP API 相比提供了獨特的優勢。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-106">The technology used to provide an API for your app is an important choice, and gRPC offers unique benefits compared to HTTP APIs.</span></span> <span data-ttu-id="8ffdb-107">本文討論了 gRPC 的優點和缺點,並推薦了與其他技術使用 gRPC 的方案。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-107">This article discusses the strengths and weaknesses of gRPC and recommends scenarios for using gRPC over other technologies.</span></span>

## <a name="high-level-comparison"></a><span data-ttu-id="8ffdb-108">進階比較</span><span class="sxs-lookup"><span data-stu-id="8ffdb-108">High-level comparison</span></span>

<span data-ttu-id="8ffdb-109">下表提供了使用 JSON 的 gRPC 和 HTTP API 之間的功能的進階比較。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-109">The following table offers a high-level comparison of features between gRPC and HTTP APIs with JSON.</span></span>

| <span data-ttu-id="8ffdb-110">功能</span><span class="sxs-lookup"><span data-stu-id="8ffdb-110">Feature</span></span>          | <span data-ttu-id="8ffdb-111">gRPC</span><span class="sxs-lookup"><span data-stu-id="8ffdb-111">gRPC</span></span>                                               | <span data-ttu-id="8ffdb-112">帶有 JSON 的 HTTP API</span><span class="sxs-lookup"><span data-stu-id="8ffdb-112">HTTP APIs with JSON</span></span>           |
| ---------------- | -------------------------------------------------- | ----------------------------- |
| <span data-ttu-id="8ffdb-113">合約</span><span class="sxs-lookup"><span data-stu-id="8ffdb-113">Contract</span></span>         | <span data-ttu-id="8ffdb-114">必要 (*.proto*)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-114">Required (*.proto*)</span></span>                                | <span data-ttu-id="8ffdb-115">選擇(開啟 API)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-115">Optional (OpenAPI)</span></span>            |
| <span data-ttu-id="8ffdb-116">通訊協定</span><span class="sxs-lookup"><span data-stu-id="8ffdb-116">Protocol</span></span>         | <span data-ttu-id="8ffdb-117">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="8ffdb-117">HTTP/2</span></span>                                             | <span data-ttu-id="8ffdb-118">HTTP</span><span class="sxs-lookup"><span data-stu-id="8ffdb-118">HTTP</span></span>                          |
| <span data-ttu-id="8ffdb-119">Payload</span><span class="sxs-lookup"><span data-stu-id="8ffdb-119">Payload</span></span>          | [<span data-ttu-id="8ffdb-120">原發(小,二進位)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-120">Protobuf (small, binary)</span></span>](#performance)           | <span data-ttu-id="8ffdb-121">JSON(大,人類可讀)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-121">JSON (large, human readable)</span></span>  |
| <span data-ttu-id="8ffdb-122">規定性</span><span class="sxs-lookup"><span data-stu-id="8ffdb-122">Prescriptiveness</span></span> | [<span data-ttu-id="8ffdb-123">嚴格的規格</span><span class="sxs-lookup"><span data-stu-id="8ffdb-123">Strict specification</span></span>](#strict-specification)      | <span data-ttu-id="8ffdb-124">鬆散。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-124">Loose.</span></span> <span data-ttu-id="8ffdb-125">任何 HTTP 都有效。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-125">Any HTTP is valid.</span></span>     |
| <span data-ttu-id="8ffdb-126">串流</span><span class="sxs-lookup"><span data-stu-id="8ffdb-126">Streaming</span></span>        | [<span data-ttu-id="8ffdb-127">用戶端、伺服器、雙向</span><span class="sxs-lookup"><span data-stu-id="8ffdb-127">Client, server, bi-directional</span></span>](#streaming)       | <span data-ttu-id="8ffdb-128">用戶端、伺服器</span><span class="sxs-lookup"><span data-stu-id="8ffdb-128">Client, server</span></span>                |
| <span data-ttu-id="8ffdb-129">瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="8ffdb-129">Browser support</span></span>  | [<span data-ttu-id="8ffdb-130">否(需要 grpc-web)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-130">No (requires grpc-web)</span></span>](#limited-browser-support) | <span data-ttu-id="8ffdb-131">是</span><span class="sxs-lookup"><span data-stu-id="8ffdb-131">Yes</span></span>                           |
| <span data-ttu-id="8ffdb-132">安全性</span><span class="sxs-lookup"><span data-stu-id="8ffdb-132">Security</span></span>         | <span data-ttu-id="8ffdb-133">傳輸(TLS)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-133">Transport (TLS)</span></span>                                    | <span data-ttu-id="8ffdb-134">傳輸(TLS)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-134">Transport (TLS)</span></span>               |
| <span data-ttu-id="8ffdb-135">用戶端代碼產生</span><span class="sxs-lookup"><span data-stu-id="8ffdb-135">Client code-generation</span></span> | [<span data-ttu-id="8ffdb-136">是</span><span class="sxs-lookup"><span data-stu-id="8ffdb-136">Yes</span></span>](#code-generation)                      | <span data-ttu-id="8ffdb-137">OpenAPI + 第三方工具</span><span class="sxs-lookup"><span data-stu-id="8ffdb-137">OpenAPI + third-party tooling</span></span> |

## <a name="grpc-strengths"></a><span data-ttu-id="8ffdb-138">gRPC 優勢</span><span class="sxs-lookup"><span data-stu-id="8ffdb-138">gRPC strengths</span></span>

### <a name="performance"></a><span data-ttu-id="8ffdb-139">效能</span><span class="sxs-lookup"><span data-stu-id="8ffdb-139">Performance</span></span>

<span data-ttu-id="8ffdb-140">gRPC 訊息使用[Protobuf(](https://developers.google.com/protocol-buffers/docs/overview)一種高效的二進位訊息格式)進行序列化。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-140">gRPC messages are serialized using [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), an efficient binary message format.</span></span> <span data-ttu-id="8ffdb-141">Protobuf 在伺服器和用戶端上非常快速地序列化。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-141">Protobuf serializes very quickly on the server and client.</span></span> <span data-ttu-id="8ffdb-142">Protobuf 序列化會導致小消息負載,在有限的頻寬方案中(如移動應用)非常重要。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-142">Protobuf serialization results in small message payloads, important in limited bandwidth scenarios like mobile apps.</span></span>

<span data-ttu-id="8ffdb-143">gRPC 專為 HTTP/2 設計,這是 HTTP 的主要修訂版,與 HTTP 1.x 具有顯著的性能優勢:</span><span class="sxs-lookup"><span data-stu-id="8ffdb-143">gRPC is designed for HTTP/2, a major revision of HTTP that provides significant performance benefits over HTTP 1.x:</span></span>

* <span data-ttu-id="8ffdb-144">二進位框架和壓縮。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-144">Binary framing and compression.</span></span> <span data-ttu-id="8ffdb-145">HTTP/2 協定在發送和接收方面緊湊且高效。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-145">HTTP/2 protocol is compact and efficient both in sending and receiving.</span></span>
* <span data-ttu-id="8ffdb-146">通過單個 TCP 連接對多個 HTTP/2 調用進行多路複用。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-146">Multiplexing of multiple HTTP/2 calls over a single TCP connection.</span></span> <span data-ttu-id="8ffdb-147">多路複用消除了[線頭阻塞](https://en.wikipedia.org/wiki/Head-of-line_blocking)。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-147">Multiplexing eliminates [head-of-line blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking).</span></span>

### <a name="code-generation"></a><span data-ttu-id="8ffdb-148">產生程式碼</span><span class="sxs-lookup"><span data-stu-id="8ffdb-148">Code generation</span></span>

<span data-ttu-id="8ffdb-149">所有 gRPC 框架都為代碼生成提供一流的支援。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-149">All gRPC frameworks provide first-class support for code generation.</span></span> <span data-ttu-id="8ffdb-150">gRPC 開發的核心檔案是[.proto 檔案](https://developers.google.com/protocol-buffers/docs/proto3),它定義了 gRPC 服務和消息的協定。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-150">A core file to gRPC development is the [.proto file](https://developers.google.com/protocol-buffers/docs/proto3), which defines the contract of gRPC services and messages.</span></span> <span data-ttu-id="8ffdb-151">從這個檔gRPC框架將編寫代碼生成服務基類、消息和完整的用戶端。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-151">From this file gRPC frameworks will code generate a service base class, messages, and a complete client.</span></span>

<span data-ttu-id="8ffdb-152">通過在伺服器和客戶端之間共用 *.proto*檔案,可以端到端生成消息和客戶端代碼。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-152">By sharing the *.proto* file between the server and client, messages and client code can be generated from end to end.</span></span> <span data-ttu-id="8ffdb-153">用戶端的代碼生成消除了用戶端和伺服器上的消息重複,並為您創建了一個強類型用戶端。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-153">Code generation of the client eliminates duplication of messages on the client and server, and creates a strongly-typed client for you.</span></span> <span data-ttu-id="8ffdb-154">無需編寫用戶端可在具有許多服務的應用程式中節省大量開發時間。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-154">Not having to write a client saves significant development time in applications with many services.</span></span>

### <a name="strict-specification"></a><span data-ttu-id="8ffdb-155">嚴格的規格</span><span class="sxs-lookup"><span data-stu-id="8ffdb-155">Strict specification</span></span>

<span data-ttu-id="8ffdb-156">JSON 的 HTTP API 的正式規範不存在。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-156">A formal specification for HTTP API with JSON doesn't exist.</span></span> <span data-ttu-id="8ffdb-157">開發人員辯論 URL、HTTP 謂詞和回應代碼的最佳格式。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-157">Developers debate the best format of URLs, HTTP verbs, and response codes.</span></span>

<span data-ttu-id="8ffdb-158">[gRPC 規範](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)對 gRPC 服務必須遵循的格式有規定性。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-158">The [gRPC specification](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) is prescriptive about the format a gRPC service must follow.</span></span> <span data-ttu-id="8ffdb-159">gRPC 消除了爭論並節省了開發人員時間,因為 gRPC 跨平臺和實現是一致的。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-159">gRPC eliminates debate and saves developer time because gRPC is consistent across platforms and implementations.</span></span>

### <a name="streaming"></a><span data-ttu-id="8ffdb-160">串流</span><span class="sxs-lookup"><span data-stu-id="8ffdb-160">Streaming</span></span>

<span data-ttu-id="8ffdb-161">HTTP/2 為長期即時通信流提供了基礎。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-161">HTTP/2 provides a foundation for long-lived, real-time communication streams.</span></span> <span data-ttu-id="8ffdb-162">gRPC 為 HTTP/2 流式處理提供一流支援。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-162">gRPC provides first-class support for streaming through HTTP/2.</span></span>

<span data-ttu-id="8ffdb-163">gRPC 服務支援所有串流式處理組合:</span><span class="sxs-lookup"><span data-stu-id="8ffdb-163">A gRPC service supports all streaming combinations:</span></span>

* <span data-ttu-id="8ffdb-164">一元(無流)</span><span class="sxs-lookup"><span data-stu-id="8ffdb-164">Unary (no streaming)</span></span>
* <span data-ttu-id="8ffdb-165">伺服器到用戶端串流式處理</span><span class="sxs-lookup"><span data-stu-id="8ffdb-165">Server to client streaming</span></span>
* <span data-ttu-id="8ffdb-166">用戶端到伺服器流式處理</span><span class="sxs-lookup"><span data-stu-id="8ffdb-166">Client to server streaming</span></span>
* <span data-ttu-id="8ffdb-167">雙向流式處理</span><span class="sxs-lookup"><span data-stu-id="8ffdb-167">Bi-directional streaming</span></span>

### <a name="deadlinetimeouts-and-cancellation"></a><span data-ttu-id="8ffdb-168">截止日期/超時和取消</span><span class="sxs-lookup"><span data-stu-id="8ffdb-168">Deadline/timeouts and cancellation</span></span>

<span data-ttu-id="8ffdb-169">gRPC 允許用戶端指定他們願意等待 RPC 完成的時間。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-169">gRPC allows clients to specify how long they are willing to wait for an RPC to complete.</span></span> <span data-ttu-id="8ffdb-170">[截止時間](https://grpc.io/blog/deadlines)將發送到伺服器,伺服器可以決定,如果超過截止時間,該伺服器將執行什麼操作。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-170">The [deadline](https://grpc.io/blog/deadlines) is sent to the server, and the server can decide what action to take if it exceeds the deadline.</span></span> <span data-ttu-id="8ffdb-171">例如,伺服器可能會取消在超時時正在進行的 gRPC/HTTP/資料庫請求。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-171">For example, the server might cancel in-progress gRPC/HTTP/database requests on timeout.</span></span>

<span data-ttu-id="8ffdb-172">通過子 gRPC 調用傳播截止時間並取消有助於強制實施資源使用限制。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-172">Propagating the deadline and cancellation through child gRPC calls helps enforce resource usage limits.</span></span>

## <a name="grpc-recommended-scenarios"></a><span data-ttu-id="8ffdb-173">gRPC 建議的機制</span><span class="sxs-lookup"><span data-stu-id="8ffdb-173">gRPC recommended scenarios</span></span>

<span data-ttu-id="8ffdb-174">gRPC 非常適合以下機制:</span><span class="sxs-lookup"><span data-stu-id="8ffdb-174">gRPC is well suited to the following scenarios:</span></span>

* <span data-ttu-id="8ffdb-175">**微服務**&ndash;gRPC 專為低延遲和高吞吐量通信而設計。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-175">**Microservices** &ndash; gRPC is designed for low latency and high throughput communication.</span></span> <span data-ttu-id="8ffdb-176">gRPC 非常適合效率至關重要的輕量級微服務。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-176">gRPC is great for lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="8ffdb-177">**點對點即時通信**&ndash;gRPC 對雙向流流具有出色的支援。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-177">**Point-to-point real-time communication** &ndash; gRPC has excellent support for bi-directional streaming.</span></span> <span data-ttu-id="8ffdb-178">gRPC 服務無需輪詢即可即時推送消息。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-178">gRPC services can push messages in real-time without polling.</span></span>
* <span data-ttu-id="8ffdb-179">**多格樂環境**&ndash;gRPC 工具支援所有流行的開發語言,使 gRPC 成為多語言環境的最佳選擇。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-179">**Polyglot environments** &ndash; gRPC tooling supports all popular development languages, making gRPC a good choice for multi-language environments.</span></span>
* <span data-ttu-id="8ffdb-180">**網路受限環境**&ndash;gRPC 消息使用 Protobuf(一種輕量級消息格式)進行序列化。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-180">**Network constrained environments** &ndash; gRPC messages are serialized with Protobuf, a lightweight message format.</span></span> <span data-ttu-id="8ffdb-181">gRPC 消息始終小於等效的 JSON 消息。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-181">A gRPC message is always smaller than an equivalent JSON message.</span></span>

## <a name="grpc-weaknesses"></a><span data-ttu-id="8ffdb-182">gRPC 弱點</span><span class="sxs-lookup"><span data-stu-id="8ffdb-182">gRPC weaknesses</span></span>

### <a name="limited-browser-support"></a><span data-ttu-id="8ffdb-183">有限的瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="8ffdb-183">Limited browser support</span></span>

<span data-ttu-id="8ffdb-184">今天不可能直接從瀏覽器調用 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-184">It's impossible to directly call a gRPC service from a browser today.</span></span> <span data-ttu-id="8ffdb-185">gRPC 大量使用 HTTP/2 功能,沒有瀏覽器提供支援 gRPC 用戶端的 Web 請求所需的控制等級。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-185">gRPC heavily uses HTTP/2 features and no browser provides the level of control required over web requests to support a gRPC client.</span></span> <span data-ttu-id="8ffdb-186">例如,瀏覽器不允許調用方要求使用 HTTP/2,或提供對基礎 HTTP/2 幀的訪問。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-186">For example, browsers do not allow a caller to require that HTTP/2 be used, or provide access to underlying HTTP/2 frames.</span></span>

<span data-ttu-id="8ffdb-187">[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html)是 gRPC 團隊的附加技術,在瀏覽器中提供有限的 gRPC 支援。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-187">[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html) is an additional technology from the gRPC team that provides limited gRPC support in the browser.</span></span> <span data-ttu-id="8ffdb-188">gRPC-Web 由兩部分組成:支援所有現代瀏覽器的 JavaScript 用戶端和伺服器上的 gRPC-Web 代理。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-188">gRPC-Web consists of two parts: a JavaScript client that supports all modern browsers, and a gRPC-Web proxy on the server.</span></span> <span data-ttu-id="8ffdb-189">gRPC-Web 用戶端呼叫代理,代理將在 gRPC 請求上轉寄到 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-189">The gRPC-Web client calls the proxy and the proxy will forward on the gRPC requests to the gRPC server.</span></span>

<span data-ttu-id="8ffdb-190">並非所有 gRPC 的功能都由 gRPC-Web 支援。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-190">Not all of gRPC's features are supported by gRPC-Web.</span></span> <span data-ttu-id="8ffdb-191">不支援用戶端和雙向流,並且對伺服器流的支援有限。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-191">Client and bi-directional streaming isn't supported, and there is limited support for server streaming.</span></span>

> [!TIP]
> <span data-ttu-id="8ffdb-192">.NET 核心對 gRPC-Web 具有實驗性支援。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-192">.NET Core has experimental support for gRPC-Web.</span></span> <span data-ttu-id="8ffdb-193">請訪問<xref:grpc/browser>瞭解更多資訊。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-193">Visit <xref:grpc/browser> for more information.</span></span>

### <a name="not-human-readable"></a><span data-ttu-id="8ffdb-194">不可人類可讀</span><span class="sxs-lookup"><span data-stu-id="8ffdb-194">Not human readable</span></span>

<span data-ttu-id="8ffdb-195">HTTP API 請求以文本形式發送,可以由人類讀取和創建。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-195">HTTP API requests are sent as text and can be read and created by humans.</span></span>

<span data-ttu-id="8ffdb-196">默認情況下,gRPC 消息使用 Protobuf 進行編碼。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-196">gRPC messages are encoded with Protobuf by default.</span></span> <span data-ttu-id="8ffdb-197">雖然 Protobuf 是有效的傳送和接收, 其二進位格式不是人類可讀的.</span><span class="sxs-lookup"><span data-stu-id="8ffdb-197">While Protobuf is efficient to send and receive, its binary format isn't human readable.</span></span> <span data-ttu-id="8ffdb-198">Protobuf 要求消息在 *.proto*檔中指定的介面描述才能正確反序列化。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-198">Protobuf requires the message's interface description specified in the *.proto* file to properly deserialize.</span></span> <span data-ttu-id="8ffdb-199">需要額外的工具來分析導線上的 Protobuf 有效負載,並手動撰寫請求。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-199">Additional tooling is required to analyze Protobuf payloads on the wire and to compose requests by hand.</span></span>

<span data-ttu-id="8ffdb-200">[存在伺服器反射](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)和[gRPC 命令列工具](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md)等功能,以幫助處理二進制 Protobuf 消息。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-200">Features such as [server reflection](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) and the [gRPC command line tool](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) exist to assist with binary Protobuf messages.</span></span> <span data-ttu-id="8ffdb-201">此外,Protobuf 訊息支援[轉換和從 JSON](https://developers.google.com/protocol-buffers/docs/proto3#json)。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-201">Also, Protobuf messages support [conversion to and from JSON](https://developers.google.com/protocol-buffers/docs/proto3#json).</span></span> <span data-ttu-id="8ffdb-202">內建的 JSON 轉換提供了一種在調試時將 Protobuf 消息轉換為和從人類可讀形式轉換的有效方法。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-202">The built-in JSON conversion provides an efficient way to convert Protobuf messages to and from human readable form when debugging.</span></span>

## <a name="alternative-framework-scenarios"></a><span data-ttu-id="8ffdb-203">替代框架機制</span><span class="sxs-lookup"><span data-stu-id="8ffdb-203">Alternative framework scenarios</span></span>

<span data-ttu-id="8ffdb-204">在以下情況下,建議透過 gRPC 使用其他框架:</span><span class="sxs-lookup"><span data-stu-id="8ffdb-204">Other frameworks are recommended over gRPC in the following scenarios:</span></span>

* <span data-ttu-id="8ffdb-205">瀏覽器中不完全支援**瀏覽器可訪問的 API** &ndash; gRPC。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-205">**Browser accessible APIs** &ndash; gRPC isn't fully supported in the browser.</span></span> <span data-ttu-id="8ffdb-206">gRPC-Web 可以提供瀏覽器支援,但它有限制並引入了伺服器代理。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-206">gRPC-Web can offer browser support, but it has limitations and introduces a server proxy.</span></span>
* <span data-ttu-id="8ffdb-207">**廣播即時通信**&ndash;gRPC 支援通過流進行即時通信,但不存在將消息廣播到已註冊連接的概念。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-207">**Broadcast real-time communication** &ndash; gRPC supports real-time communication via streaming, but the concept of broadcasting a message out to registered connections doesn't exist.</span></span> <span data-ttu-id="8ffdb-208">例如,在聊天室方案中,應向聊天室中的所有用戶端發送新的聊天訊息,需要每個 gRPC 調用才能單獨將新的聊天訊息流式傳輸到用戶端。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-208">For example in a chat room scenario where new chat messages should be sent to all clients in the chat room, each gRPC call is required to individually stream new chat messages to the client.</span></span> <span data-ttu-id="8ffdb-209">[SignalR](xref:signalr/introduction)是此方案的有用框架。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-209">[SignalR](xref:signalr/introduction) is a useful framework for this scenario.</span></span> SignalR<span data-ttu-id="8ffdb-210">具有持久連接和對廣播消息的內置支援的概念。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-210"> has the concept of persistent connections and built-in support for broadcasting messages.</span></span>
* <span data-ttu-id="8ffdb-211">**進程間通信**&ndash;進程必須承載 HTTP/2 伺服器才能接受傳入的 gRPC 調用。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-211">**Inter-process communication** &ndash; A process must host an HTTP/2 server to accept incoming gRPC calls.</span></span> <span data-ttu-id="8ffdb-212">對於 Windows,進程間通信[管道](/dotnet/standard/io/pipe-operations)是一種快速、輕量級的通信方法。</span><span class="sxs-lookup"><span data-stu-id="8ffdb-212">For Windows, inter-process communication [pipes](/dotnet/standard/io/pipe-operations) is a fast, lightweight method of communication.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8ffdb-213">其他資源</span><span class="sxs-lookup"><span data-stu-id="8ffdb-213">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
