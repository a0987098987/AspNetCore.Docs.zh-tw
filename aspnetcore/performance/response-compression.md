---
title: ASP.NET Core 中的回應壓縮
author: guardrex
description: 了解回應壓縮及如何使用 ASP.NET Core 應用程式中的回應壓縮中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: performance/response-compression
ms.openlocfilehash: 04b2ffd7047e8b127968adb5d40e0141365fb5fe
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880915"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="88d39-103">ASP.NET Core 中的回應壓縮</span><span class="sxs-lookup"><span data-stu-id="88d39-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="88d39-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="88d39-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="88d39-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="88d39-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="88d39-106">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="88d39-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="88d39-107">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="88d39-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="88d39-108">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="88d39-109">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="88d39-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="88d39-110">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="88d39-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="88d39-111">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="88d39-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="88d39-112">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="88d39-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="88d39-113">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="88d39-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="88d39-114">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="88d39-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="88d39-115">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="88d39-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="88d39-116">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="88d39-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="88d39-117">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="88d39-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="88d39-118">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="88d39-118">Hosting directly on:</span></span>
  * <span data-ttu-id="88d39-119">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="88d39-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="88d39-120">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="88d39-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="88d39-121">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="88d39-121">Response compression</span></span>

<span data-ttu-id="88d39-122">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="88d39-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="88d39-123">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="88d39-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="88d39-124">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="88d39-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="88d39-125">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="88d39-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="88d39-126">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="88d39-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="88d39-127">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="88d39-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="88d39-128">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="88d39-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="88d39-129">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="88d39-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="88d39-130">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="88d39-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="88d39-131">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="88d39-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="88d39-132">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="88d39-132">Middleware Supported</span></span> | <span data-ttu-id="88d39-133">描述</span><span class="sxs-lookup"><span data-stu-id="88d39-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="88d39-134">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="88d39-134">Yes (default)</span></span>        | [<span data-ttu-id="88d39-135">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="88d39-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="88d39-136">否</span><span class="sxs-lookup"><span data-stu-id="88d39-136">No</span></span>                   | [<span data-ttu-id="88d39-137">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="88d39-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="88d39-138">否</span><span class="sxs-lookup"><span data-stu-id="88d39-138">No</span></span>                   | [<span data-ttu-id="88d39-139">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="88d39-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="88d39-140">是</span><span class="sxs-lookup"><span data-stu-id="88d39-140">Yes</span></span>                  | [<span data-ttu-id="88d39-141">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="88d39-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="88d39-142">是</span><span class="sxs-lookup"><span data-stu-id="88d39-142">Yes</span></span>                  | <span data-ttu-id="88d39-143">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="88d39-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="88d39-144">否</span><span class="sxs-lookup"><span data-stu-id="88d39-144">No</span></span>                   | [<span data-ttu-id="88d39-145">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="88d39-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="88d39-146">是</span><span class="sxs-lookup"><span data-stu-id="88d39-146">Yes</span></span>                  | <span data-ttu-id="88d39-147">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="88d39-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="88d39-148">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="88d39-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="88d39-149">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="88d39-149">Middleware Supported</span></span> | <span data-ttu-id="88d39-150">描述</span><span class="sxs-lookup"><span data-stu-id="88d39-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="88d39-151">否</span><span class="sxs-lookup"><span data-stu-id="88d39-151">No</span></span>                   | [<span data-ttu-id="88d39-152">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="88d39-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="88d39-153">否</span><span class="sxs-lookup"><span data-stu-id="88d39-153">No</span></span>                   | [<span data-ttu-id="88d39-154">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="88d39-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="88d39-155">否</span><span class="sxs-lookup"><span data-stu-id="88d39-155">No</span></span>                   | [<span data-ttu-id="88d39-156">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="88d39-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="88d39-157">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="88d39-157">Yes (default)</span></span>        | [<span data-ttu-id="88d39-158">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="88d39-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="88d39-159">是</span><span class="sxs-lookup"><span data-stu-id="88d39-159">Yes</span></span>                  | <span data-ttu-id="88d39-160">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="88d39-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="88d39-161">否</span><span class="sxs-lookup"><span data-stu-id="88d39-161">No</span></span>                   | [<span data-ttu-id="88d39-162">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="88d39-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="88d39-163">是</span><span class="sxs-lookup"><span data-stu-id="88d39-163">Yes</span></span>                  | <span data-ttu-id="88d39-164">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="88d39-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="88d39-165">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="88d39-165">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="88d39-166">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="88d39-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="88d39-167">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="88d39-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="88d39-168">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="88d39-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="88d39-169">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="88d39-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="88d39-170">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="88d39-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="88d39-171">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="88d39-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="88d39-172">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="88d39-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="88d39-173">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="88d39-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="88d39-174">標頭</span><span class="sxs-lookup"><span data-stu-id="88d39-174">Header</span></span>             | <span data-ttu-id="88d39-175">角色</span><span class="sxs-lookup"><span data-stu-id="88d39-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="88d39-176">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="88d39-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="88d39-177">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="88d39-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="88d39-178">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="88d39-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="88d39-179">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="88d39-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="88d39-180">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="88d39-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="88d39-181">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="88d39-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="88d39-182">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="88d39-183">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="88d39-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="88d39-184">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="88d39-185">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="88d39-186">使用[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="88d39-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="88d39-187">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="88d39-187">The sample illustrates:</span></span>

* <span data-ttu-id="88d39-188">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="88d39-189">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="88d39-190">套件</span><span class="sxs-lookup"><span data-stu-id="88d39-190">Package</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88d39-191">回應壓縮中介軟體是由[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)套件所提供，它會隱含地包含在 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="88d39-191">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="88d39-192">若要在專案中包含中介軟體，請新增[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)的參考，其中包含[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="88d39-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="88d39-193">組態</span><span class="sxs-lookup"><span data-stu-id="88d39-193">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88d39-194">下列程式碼示範如何針對預設 MIME 類型和壓縮提供者（[Brotli](#brotli-compression-provider)和[Gzip](#gzip-compression-provider)）啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="88d39-194">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="88d39-195">下列程式碼示範如何為預設 MIME 類型和[Gzip 壓縮提供者](#gzip-compression-provider)啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="88d39-195">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

::: moniker-end

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddResponseCompression();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        app.UseResponseCompression();
    }
}
```

<span data-ttu-id="88d39-196">附註：</span><span class="sxs-lookup"><span data-stu-id="88d39-196">Notes:</span></span>

* <span data-ttu-id="88d39-197">`app.UseResponseCompression` 必須在 `app.UseMvc`之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="88d39-197">`app.UseResponseCompression` must be called before `app.UseMvc`.</span></span>
* <span data-ttu-id="88d39-198">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="88d39-198">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="88d39-199">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-199">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="88d39-200">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="88d39-200">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88d39-203">使用 `Accept-Encoding: br` 標頭（Brotli 壓縮）將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-203">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="88d39-204">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="88d39-204">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 br 之要求的結果。](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="88d39-208">使用 `Accept-Encoding: gzip` 標頭將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-208">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="88d39-209">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="88d39-209">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和 gzip 值之要求的結果。](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a><span data-ttu-id="88d39-213">提供者</span><span class="sxs-lookup"><span data-stu-id="88d39-213">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="88d39-214">Brotli 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="88d39-214">Brotli Compression Provider</span></span>

<span data-ttu-id="88d39-215">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider>，以[Brotli 壓縮資料格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-215">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="88d39-216">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="88d39-216">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="88d39-217">Brotli 壓縮提供者預設會加入至壓縮提供者陣列，以及[Gzip 壓縮提供者](#gzip-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="88d39-217">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="88d39-218">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-218">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="88d39-219">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="88d39-219">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="88d39-220">明確新增任何壓縮提供者時，必須新增 Brotoli 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="88d39-220">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88d39-221">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="88d39-221">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="88d39-222">Brotli 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-222">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="88d39-223">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-223">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="88d39-224">Compression Level</span><span class="sxs-lookup"><span data-stu-id="88d39-224">Compression Level</span></span> | <span data-ttu-id="88d39-225">描述</span><span class="sxs-lookup"><span data-stu-id="88d39-225">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="88d39-226">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="88d39-226">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="88d39-227">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="88d39-227">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="88d39-228">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="88d39-228">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="88d39-229">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-229">No compression should be performed.</span></span> |
| [<span data-ttu-id="88d39-230">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="88d39-230">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="88d39-231">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="88d39-231">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<BrotliCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

::: moniker-end

### <a name="gzip-compression-provider"></a><span data-ttu-id="88d39-232">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="88d39-232">Gzip Compression Provider</span></span>

<span data-ttu-id="88d39-233">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-233">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="88d39-234">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="88d39-234">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="88d39-235">Gzip 壓縮提供者預設會加入壓縮提供者的陣列，以及[Brotli 壓縮提供者](#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="88d39-235">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="88d39-236">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-236">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="88d39-237">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="88d39-237">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="88d39-238">Gzip 壓縮提供者預設會加入至壓縮提供者陣列。</span><span class="sxs-lookup"><span data-stu-id="88d39-238">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="88d39-239">當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="88d39-239">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="88d39-240">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="88d39-240">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

::: moniker-end

<span data-ttu-id="88d39-241">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="88d39-241">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="88d39-242">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-242">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="88d39-243">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-243">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="88d39-244">Compression Level</span><span class="sxs-lookup"><span data-stu-id="88d39-244">Compression Level</span></span> | <span data-ttu-id="88d39-245">描述</span><span class="sxs-lookup"><span data-stu-id="88d39-245">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="88d39-246">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="88d39-246">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="88d39-247">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="88d39-247">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="88d39-248">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="88d39-248">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="88d39-249">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-249">No compression should be performed.</span></span> |
| [<span data-ttu-id="88d39-250">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="88d39-250">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="88d39-251">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="88d39-251">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();

    services.Configure<GzipCompressionProviderOptions>(options => 
    {
        options.Level = CompressionLevel.Fastest;
    });
}
```

### <a name="custom-providers"></a><span data-ttu-id="88d39-252">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="88d39-252">Custom providers</span></span>

<span data-ttu-id="88d39-253">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="88d39-253">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="88d39-254"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="88d39-254">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="88d39-255">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="88d39-255">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="88d39-256">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="88d39-256">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="88d39-257">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-257">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="88d39-258">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="88d39-258">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="88d39-259">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="88d39-259">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="88d39-260">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="88d39-260">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="88d39-261">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="88d39-261">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="88d39-262">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="88d39-262">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="88d39-263">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="88d39-263">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="88d39-266">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="88d39-266">MIME types</span></span>

<span data-ttu-id="88d39-267">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="88d39-267">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="88d39-268">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="88d39-268">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="88d39-269">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="88d39-269">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="88d39-270">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="88d39-270">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="88d39-271">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="88d39-271">Compression with secure protocol</span></span>

<span data-ttu-id="88d39-272">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="88d39-272">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="88d39-273">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="88d39-273">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="88d39-274">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="88d39-274">Adding the Vary header</span></span>

<span data-ttu-id="88d39-275">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="88d39-275">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="88d39-276">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="88d39-276">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="88d39-277">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="88d39-277">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="88d39-278">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="88d39-278">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="88d39-279">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="88d39-279">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="88d39-280">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="88d39-280">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="88d39-281">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="88d39-281">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="88d39-282">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="88d39-282">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="88d39-283">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="88d39-283">Working with IIS dynamic compression</span></span>

<span data-ttu-id="88d39-284">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="88d39-284">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="88d39-285">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="88d39-285">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="88d39-286">疑難排解</span><span class="sxs-lookup"><span data-stu-id="88d39-286">Troubleshooting</span></span>

<span data-ttu-id="88d39-287">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="88d39-287">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="88d39-288">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="88d39-288">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="88d39-289">`Accept-Encoding` 標頭存在，其值為 `br`、`gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="88d39-289">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="88d39-290">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="88d39-290">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="88d39-291">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="88d39-291">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="88d39-292">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="88d39-292">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="88d39-293">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="88d39-293">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="88d39-294">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="88d39-294">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="88d39-295">`Accept-Encoding` 標頭存在，其值為 `gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="88d39-295">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="88d39-296">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="88d39-296">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="88d39-297">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="88d39-297">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="88d39-298">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="88d39-298">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="88d39-299">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="88d39-299">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="88d39-300">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="88d39-300">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="88d39-301">其他資源</span><span class="sxs-lookup"><span data-stu-id="88d39-301">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="88d39-302">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="88d39-302">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="88d39-303">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="88d39-303">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="88d39-304">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="88d39-304">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="88d39-305">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="88d39-305">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)
