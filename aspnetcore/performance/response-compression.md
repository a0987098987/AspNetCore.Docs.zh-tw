---
title: ASP.NET Core 中的回應壓縮
author: guardrex
description: 深入了解回應壓縮，以及如何使用 ASP.NET Core 應用程式中的回應壓縮中介軟體。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/21/2018
uid: performance/response-compression
ms.openlocfilehash: 3a01c2d572c0026944347f736f9658a7872e6c35
ms.sourcegitcommit: 4d5f8680d68b39c411b46c73f7014f8aa0f12026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/24/2018
ms.locfileid: "47028280"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="ffaaf-103">ASP.NET Core 中的回應壓縮</span><span class="sxs-lookup"><span data-stu-id="ffaaf-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="ffaaf-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ffaaf-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ffaaf-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ffaaf-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="ffaaf-106">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="ffaaf-107">減少回應的大小通常應用程式的回應速度通常會大幅增加。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="ffaaf-108">減少承載大小的方法之一是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="ffaaf-109">回應壓縮中介軟體的使用時機</span><span class="sxs-lookup"><span data-stu-id="ffaaf-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="ffaaf-110">使用 IIS、 Apache 或 Nginx 中的伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="ffaaf-111">中介軟體的效能可能不會符合伺服器模組。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="ffaaf-112">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)並[Kestrel](xref:fundamentals/servers/kestrel)目前並未提供內建壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="ffaaf-113">當您準備時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="ffaaf-114">無法使用下列伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="ffaaf-115">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="ffaaf-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="ffaaf-116">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="ffaaf-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="ffaaf-117">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="ffaaf-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="ffaaf-118">直接在裝載：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-118">Hosting directly on:</span></span>
  * <span data-ttu-id="ffaaf-119">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)(先前稱為[WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="ffaaf-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="ffaaf-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="ffaaf-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="ffaaf-121">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="ffaaf-121">Response compression</span></span>

<span data-ttu-id="ffaaf-122">通常，任何原本不壓縮的回應可以獲益於回應壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="ffaaf-123">回應原本不壓縮通常包括： CSS、 JavaScript、 HTML、 XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="ffaaf-124">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="ffaaf-125">如果您嘗試以進一步將壓縮的原生壓縮的回應，降低小的大小和傳輸的時間將可能會執行處理壓縮花費的時間。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="ffaaf-126">不壓縮檔案小於大約 150 1000年位元組 （取決於檔案的內容及壓縮的效率）。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="ffaaf-127">壓縮的小檔案的額外負荷可能會產生比未壓縮的檔案較大的壓縮的檔。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="ffaaf-128">當用戶端可以處理壓縮的內容時，用戶端必須傳送通知其功能的伺服器`Accept-Encoding`與要求的標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="ffaaf-129">當伺服器傳送壓縮的內容時，它必須包含資訊`Content-Encoding`標頭壓縮的回應編碼的方式。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="ffaaf-130">中介軟體所支援的內容編碼表示法會顯示下表中。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

::: moniker range=">= aspnetcore-2.2"

| <span data-ttu-id="ffaaf-131">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="ffaaf-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="ffaaf-132">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="ffaaf-132">Middleware Supported</span></span> | <span data-ttu-id="ffaaf-133">描述</span><span class="sxs-lookup"><span data-stu-id="ffaaf-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="ffaaf-134">[是] （預設值）</span><span class="sxs-lookup"><span data-stu-id="ffaaf-134">Yes (default)</span></span>        | [<span data-ttu-id="ffaaf-135">Brotli 壓縮的資料格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="ffaaf-136">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-136">No</span></span>                   | [<span data-ttu-id="ffaaf-137">DEFLATE 壓縮的資料格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="ffaaf-138">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-138">No</span></span>                   | [<span data-ttu-id="ffaaf-139">W3C 有效的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="ffaaf-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="ffaaf-140">是</span><span class="sxs-lookup"><span data-stu-id="ffaaf-140">Yes</span></span>                  | [<span data-ttu-id="ffaaf-141">Gzip 檔案格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="ffaaf-142">是</span><span class="sxs-lookup"><span data-stu-id="ffaaf-142">Yes</span></span>                  | <span data-ttu-id="ffaaf-143">「 沒有編碼 」 的識別項： 必須未編碼的回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="ffaaf-144">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-144">No</span></span>                   | [<span data-ttu-id="ffaaf-145">Java 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="ffaaf-146">是</span><span class="sxs-lookup"><span data-stu-id="ffaaf-146">Yes</span></span>                  | <span data-ttu-id="ffaaf-147">編碼不明確要求任何可用的內容</span><span class="sxs-lookup"><span data-stu-id="ffaaf-147">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| <span data-ttu-id="ffaaf-148">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="ffaaf-148">`Accept-Encoding` header values</span></span> | <span data-ttu-id="ffaaf-149">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="ffaaf-149">Middleware Supported</span></span> | <span data-ttu-id="ffaaf-150">描述</span><span class="sxs-lookup"><span data-stu-id="ffaaf-150">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="ffaaf-151">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-151">No</span></span>                   | [<span data-ttu-id="ffaaf-152">Brotli 壓縮的資料格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-152">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="ffaaf-153">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-153">No</span></span>                   | [<span data-ttu-id="ffaaf-154">DEFLATE 壓縮的資料格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-154">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="ffaaf-155">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-155">No</span></span>                   | [<span data-ttu-id="ffaaf-156">W3C 有效的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="ffaaf-156">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="ffaaf-157">[是] （預設值）</span><span class="sxs-lookup"><span data-stu-id="ffaaf-157">Yes (default)</span></span>        | [<span data-ttu-id="ffaaf-158">Gzip 檔案格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-158">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="ffaaf-159">是</span><span class="sxs-lookup"><span data-stu-id="ffaaf-159">Yes</span></span>                  | <span data-ttu-id="ffaaf-160">「 沒有編碼 」 的識別項： 必須未編碼的回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-160">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="ffaaf-161">否</span><span class="sxs-lookup"><span data-stu-id="ffaaf-161">No</span></span>                   | [<span data-ttu-id="ffaaf-162">Java 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="ffaaf-162">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="ffaaf-163">是</span><span class="sxs-lookup"><span data-stu-id="ffaaf-163">Yes</span></span>                  | <span data-ttu-id="ffaaf-164">編碼不明確要求任何可用的內容</span><span class="sxs-lookup"><span data-stu-id="ffaaf-164">Any available content encoding not explicitly requested</span></span> |

::: moniker-end

<span data-ttu-id="ffaaf-165">如需詳細資訊，請參閱 < [IANA 官方內容撰寫程式碼清單](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-165">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="ffaaf-166">中介軟體可讓您新增額外的壓縮提供者的自訂`Accept-Encoding`標頭值。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-166">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="ffaaf-167">如需詳細資訊，請參閱 <<c0> [ 自訂提供者](#custom-providers)如下。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-167">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="ffaaf-168">中介軟體可以回應的品質值 (qvalue， `q`) 加權時由用戶端傳送到設定優先權的壓縮配置。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-168">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="ffaaf-169">如需詳細資訊，請參閱 < [RFC 7231： 接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-169">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="ffaaf-170">壓縮演算法受限於壓縮速度與壓縮的效能之間取捨。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-170">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="ffaaf-171">*有效性*在此內容中是指輸出的大小在壓縮後的。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-171">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="ffaaf-172">最小的大小之後，即可最*最佳*壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-172">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="ffaaf-173">參與要求的標頭，傳送、 快取，並接收壓縮的內容是下表中所述。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-173">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="ffaaf-174">頁首</span><span class="sxs-lookup"><span data-stu-id="ffaaf-174">Header</span></span>             | <span data-ttu-id="ffaaf-175">角色</span><span class="sxs-lookup"><span data-stu-id="ffaaf-175">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="ffaaf-176">表示編碼配置給用戶端可接受的內容，從用戶端傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-176">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="ffaaf-177">表示承載中的內容編碼，從伺服器傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-177">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="ffaaf-178">壓縮時，`Content-Length`移除標頭，因為本文的內容變更時壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-178">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="ffaaf-179">壓縮時，`Content-MD5`移除標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-179">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="ffaaf-180">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-180">Specifies the MIME type of the content.</span></span> <span data-ttu-id="ffaaf-181">每個回應應該指定其`Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-181">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="ffaaf-182">中介軟體會檢查此值，以判斷應該將壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-182">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="ffaaf-183">中介軟體會指定一組[預設 MIME 類型](#mime-types)即可進行編碼，但您可以取代或新增 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-183">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="ffaaf-184">值是伺服器所傳送`Accept-Encoding`用戶端和 proxy`Vary`標頭會指出用戶端或 proxy，應快取 （而有所不同） 回應為基礎的值`Accept-Encoding`要求標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-184">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="ffaaf-185">傳回內容的結果`Vary: Accept-Encoding`標頭時，壓縮和未壓縮的回應會分別快取。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-185">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="ffaaf-186">瀏覽的功能與回應壓縮中介軟體[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-186">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="ffaaf-187">此範例會說明：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-187">The sample illustrates:</span></span>

* <span data-ttu-id="ffaaf-188">使用 Gzip 和自訂壓縮提供者的應用程式回應的壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-188">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="ffaaf-189">如何將 MIME 類型加入至壓縮的 MIME 類型的預設清單。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-189">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="ffaaf-190">Package</span><span class="sxs-lookup"><span data-stu-id="ffaaf-190">Package</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ffaaf-191">若要在專案中包含中介軟體，將參考加入[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，其中包括[Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-191">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ffaaf-192">若要在專案中包含中介軟體，將參考加入[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)，其中包括[Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-192">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ffaaf-193">若要在專案中包含中介軟體，將參考加入[Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-193">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

::: moniker-end

## <a name="configuration"></a><span data-ttu-id="ffaaf-194">組態</span><span class="sxs-lookup"><span data-stu-id="ffaaf-194">Configuration</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="ffaaf-195">下列程式碼示範如何啟用預設的 MIME 類型和壓縮提供者在回應壓縮中介軟體 ([Brotli](#brotli-compression-provider)並[Gzip](#gzip-compression-provider)):</span><span class="sxs-lookup"><span data-stu-id="ffaaf-195">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="ffaaf-196">下列程式碼示範如何啟用預設的 MIME 類型回應壓縮中介軟體和[Gzip 壓縮提供者](#gzip-compression-provider):</span><span class="sxs-lookup"><span data-stu-id="ffaaf-196">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

> [!NOTE]
> <span data-ttu-id="ffaaf-197">使用之類的工具[Fiddler](http://www.telerik.com/fiddler)， [Firebug](http://getfirebug.com/)，或[Postman](https://www.getpostman.com/)設`Accept-Encoding`要求標頭，然後研究回應標頭、 大小和主體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-197">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="ffaaf-198">將要求提交到範例應用程式，而不需要`Accept-Encoding`標頭，並觀察回應是未壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-198">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="ffaaf-199">`Content-Encoding`和`Vary`標頭不存在的回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-199">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler 視窗會顯示沒有 Accept-encoding 標頭之要求的結果。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="ffaaf-202">將要求提交到範例應用程式與`Accept-Encoding: gzip`標頭，並觀察壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-202">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="ffaaf-203">`Content-Encoding`和`Vary`標頭有回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-203">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![顯示包含 Accept-encoding 標頭之要求的結果而值為 gzip 的 fiddler 視窗。](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="ffaaf-207">提供者</span><span class="sxs-lookup"><span data-stu-id="ffaaf-207">Providers</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a><span data-ttu-id="ffaaf-208">Brotli 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="ffaaf-208">Brotli Compression Provider</span></span>

<span data-ttu-id="ffaaf-209">使用`BrotliCompressionProvider`壓縮回應[Brotli 壓縮的資料格式](https://tools.ietf.org/html/rfc7932)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-209">Use the `BrotliCompressionProvider` to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="ffaaf-210">如果任何壓縮提供者明確不新增至<xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="ffaaf-210">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="ffaaf-211">預設會新增 Brotli 壓縮提供者連同壓縮提供者的陣列[Gzip 壓縮提供者](#gzip-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-211">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="ffaaf-212">當用戶端支援 Brotli 壓縮的資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-212">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="ffaaf-213">如果用戶端不支援 Brotli，壓縮會預設為 Gzip 當用戶端支援 Gzip 壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-213">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="ffaaf-214">明確地新增任何壓縮提供者時，必須加入 Brotoli 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-214">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

<span data-ttu-id="ffaaf-215">設定壓縮層級使用`BrotliCompressionProviderOptions`。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-215">Set the compression level with `BrotliCompressionProviderOptions`.</span></span> <span data-ttu-id="ffaaf-216">Brotli 壓縮提供者預設為最快的壓縮層級 ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel))，這可能不會產生最有效的壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-216">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="ffaaf-217">如果想要最有效的壓縮，請設定最佳的壓縮中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-217">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="ffaaf-218">壓縮層級</span><span class="sxs-lookup"><span data-stu-id="ffaaf-218">Compression Level</span></span> | <span data-ttu-id="ffaaf-219">描述</span><span class="sxs-lookup"><span data-stu-id="ffaaf-219">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="ffaaf-220">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="ffaaf-220">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="ffaaf-221">即使未以最佳方式壓縮所產生的輸出，應儘速完成壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-221">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="ffaaf-222">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="ffaaf-222">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="ffaaf-223">應該不執行任何壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-223">No compression should be performed.</span></span> |
| [<span data-ttu-id="ffaaf-224">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="ffaaf-224">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="ffaaf-225">回應應該以最佳方式壓縮，即使壓縮需要更多的時間才能完成。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-225">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="ffaaf-226">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="ffaaf-226">Gzip Compression Provider</span></span>

<span data-ttu-id="ffaaf-227">使用<xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider>壓縮回應[Gzip 檔案格式](https://tools.ietf.org/html/rfc1952)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-227">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="ffaaf-228">如果任何壓縮提供者明確不新增至<xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span><span class="sxs-lookup"><span data-stu-id="ffaaf-228">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ffaaf-229">Gzip 壓縮提供者會加入預設的壓縮提供者，以及陣列[Brotli 壓縮提供者](#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-229">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="ffaaf-230">當用戶端支援 Brotli 壓縮的資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-230">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="ffaaf-231">如果用戶端不支援 Brotli，壓縮會預設為 Gzip 當用戶端支援 Gzip 壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-231">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ffaaf-232">Gzip 壓縮提供者會依預設加入壓縮提供者的陣列。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-232">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="ffaaf-233">當用戶端支援 Gzip 壓縮，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-233">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="ffaaf-234">明確地新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-234">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=5)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5)]

::: moniker-end

<span data-ttu-id="ffaaf-235">設定壓縮層級使用<xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-235">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="ffaaf-236">Gzip 壓縮提供者預設為最快的壓縮層級 ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel))，這可能不會產生最有效的壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-236">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="ffaaf-237">如果想要最有效的壓縮，請設定最佳的壓縮中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-237">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="ffaaf-238">壓縮層級</span><span class="sxs-lookup"><span data-stu-id="ffaaf-238">Compression Level</span></span> | <span data-ttu-id="ffaaf-239">描述</span><span class="sxs-lookup"><span data-stu-id="ffaaf-239">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="ffaaf-240">CompressionLevel.Fastest</span><span class="sxs-lookup"><span data-stu-id="ffaaf-240">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="ffaaf-241">即使未以最佳方式壓縮所產生的輸出，應儘速完成壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-241">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="ffaaf-242">CompressionLevel.NoCompression</span><span class="sxs-lookup"><span data-stu-id="ffaaf-242">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="ffaaf-243">應該不執行任何壓縮。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-243">No compression should be performed.</span></span> |
| [<span data-ttu-id="ffaaf-244">CompressionLevel.Optimal</span><span class="sxs-lookup"><span data-stu-id="ffaaf-244">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="ffaaf-245">回應應該以最佳方式壓縮，即使壓縮需要更多的時間才能完成。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-245">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="ffaaf-246">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="ffaaf-246">Custom providers</span></span>

<span data-ttu-id="ffaaf-247">建立使用自訂壓縮實作<xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-247">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="ffaaf-248"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*>表示將內容編碼這個`ICompressionProvider`產生。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-248">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="ffaaf-249">中介軟體會使用此資訊來選擇清單中指定為基礎的提供者`Accept-Encoding`要求標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-249">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="ffaaf-250">使用範例應用程式，在用戶端提交的要求`Accept-Encoding: mycustomcompression`標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-250">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="ffaaf-251">中介軟體會使用自訂壓縮實作，並傳回包含回應`Content-Encoding: mycustomcompression`標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-251">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="ffaaf-252">用戶端必須能夠解壓縮自訂壓縮實作，才能讓自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-252">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

```csharp
public class CustomCompressionProvider : ICompressionProvider
{
    public string EncodingName => "mycustomcompression";
    public bool SupportsFlush => true;

    public Stream CreateStream(Stream outputStream)
    {
        // Create a custom compression stream wrapper here
        return outputStream;
    }
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6,12-15)]

[!code-csharp[](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

::: moniker-end

<span data-ttu-id="ffaaf-253">將要求提交到範例應用程式與`Accept-Encoding: mycustomcompression`標頭，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-253">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="ffaaf-254">`Vary`和`Content-Encoding`標頭有回應。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-254">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="ffaaf-255">此範例不壓縮 （未顯示），回應主體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-255">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="ffaaf-256">沒有在壓縮實作`CustomCompressionProvider`類別的範例。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-256">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="ffaaf-257">不過，此範例會顯示您將在其中實作這類的壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-257">However, the sample shows where you would implement such a compression algorithm.</span></span>

![顯示結果的要求包含 Accept-encoding 標頭的值，並針對 mycustomcompression fiddler 視窗。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="ffaaf-260">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="ffaaf-260">MIME types</span></span>

<span data-ttu-id="ffaaf-261">中介軟體會指定一組預設的壓縮的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-261">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="ffaaf-262">取代或附加的回應壓縮中介軟體選項的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-262">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="ffaaf-263">請注意該萬用字元 MIME 類型，例如`text/*`不支援。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-263">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="ffaaf-264">範例應用程式新增的 MIME 類型`image/svg+xml`和壓縮，並提供 ASP.NET Core 橫幅影像 (*banner.svg*)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-264">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression(options =>
    {
        options.Providers.Add<BrotliCompressionProvider>();
        options.Providers.Add<GzipCompressionProvider>();
        options.Providers.Add<CustomCompressionProvider>();
        options.MimeTypes = 
            ResponseCompressionDefaults.MimeTypes.Concat(
                new[] { "image/svg+xml" });
    });
}
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

[!code-csharp[](response-compression/samples/2.x/Startup.cs?name=snippet1&highlight=7-9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7-9)]

::: moniker-end

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="ffaaf-265">使用安全的通訊協定的壓縮</span><span class="sxs-lookup"><span data-stu-id="ffaaf-265">Compression with secure protocol</span></span>

<span data-ttu-id="ffaaf-266">透過安全的連線壓縮的回應可以控制與`EnableForHttps`選項，預設會停用。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-266">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="ffaaf-267">使用動態產生的頁面壓縮可能會導致安全性問題這類[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))並[缺口](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-267">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="ffaaf-268">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="ffaaf-268">Adding the Vary header</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ffaaf-269">當壓縮回應基礎`Accept-Encoding`標頭，有可能的多個壓縮的版本回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-269">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="ffaaf-270">若要指示用戶端和 proxy 的快取多個版本存在，並應該儲存`Vary`標頭加`Accept-Encoding`值。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-270">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="ffaaf-271">在 ASP.NET Core 2.0 或更新版本中中, 介軟體新增`Vary`標頭壓縮回應時，自動。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-271">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ffaaf-272">當壓縮回應基礎`Accept-Encoding`標頭，有可能的多個壓縮的版本回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-272">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="ffaaf-273">若要指示用戶端和 proxy 的快取多個版本存在，並應該儲存`Vary`標頭加`Accept-Encoding`值。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-273">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="ffaaf-274">在 ASP.NET Core 1.x 中，新增`Vary`至回應的標頭以手動方式完成：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-274">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually:</span></span>

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="ffaaf-275">Nginx 背後時的中介軟體問題的反向 proxy</span><span class="sxs-lookup"><span data-stu-id="ffaaf-275">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="ffaaf-276">當要求 proxy 處理的 Nginx`Accept-Encoding`標頭移除。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-276">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="ffaaf-277">這可防止壓縮回應的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-277">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="ffaaf-278">如需詳細資訊，請參閱 < [NGINX： 壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-278">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="ffaaf-279">此問題會追蹤[找出 nginx (BasicMiddleware #123) 的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-279">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="ffaaf-280">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="ffaaf-280">Working with IIS dynamic compression</span></span>

<span data-ttu-id="ffaaf-281">如果您有作用中 IIS 動態壓縮模組在您想要停用的應用程式伺服器層級設定時，停用的模組使用的補充*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-281">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="ffaaf-282">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-282">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="ffaaf-283">疑難排解</span><span class="sxs-lookup"><span data-stu-id="ffaaf-283">Troubleshooting</span></span>

<span data-ttu-id="ffaaf-284">使用之類的工具[Fiddler](https://www.telerik.com/fiddler)， [Firebug](https://getfirebug.com/)，或[Postman](https://www.getpostman.com/)，這可讓您設定`Accept-Encoding`要求標頭，然後研究回應標頭、 大小和主體。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-284">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="ffaaf-285">根據預設，回應壓縮中介軟體會壓縮回應，符合下列條件：</span><span class="sxs-lookup"><span data-stu-id="ffaaf-285">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="ffaaf-286">`Accept-Encoding`標頭已存在的值`br`， `gzip`， `*`，或符合您所建立的自訂壓縮提供者的自訂編碼方式。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-286">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="ffaaf-287">值不能`identity`或有某個品質值 (qvalue， `q`) 設定為 0 （零）。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-287">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="ffaaf-288">MIME 類型 (`Content-Type`) 必須設定，而且必須符合上設定的 MIME 類型<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-288">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="ffaaf-289">要求必須包含`Content-Range`標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-289">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="ffaaf-290">要求必須使用不安全的通訊協定 (http)，除非已在回應壓縮中介軟體選項中設定安全的通訊協定 (https)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-290">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="ffaaf-291">*請注意危險[上述](#compression-with-secure-protocol)時啟用安全的內容壓縮。*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-291">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="ffaaf-292">`Accept-Encoding`標頭已存在的值`gzip`， `*`，或符合您所建立的自訂壓縮提供者的自訂編碼方式。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-292">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="ffaaf-293">值不能`identity`或有某個品質值 (qvalue， `q`) 設定為 0 （零）。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-293">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="ffaaf-294">MIME 類型 (`Content-Type`) 必須設定，而且必須符合上設定的 MIME 類型<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-294">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="ffaaf-295">要求必須包含`Content-Range`標頭。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-295">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="ffaaf-296">要求必須使用不安全的通訊協定 (http)，除非已在回應壓縮中介軟體選項中設定安全的通訊協定 (https)。</span><span class="sxs-lookup"><span data-stu-id="ffaaf-296">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="ffaaf-297">*請注意危險[上述](#compression-with-secure-protocol)時啟用安全的內容壓縮。*</span><span class="sxs-lookup"><span data-stu-id="ffaaf-297">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="ffaaf-298">其他資源</span><span class="sxs-lookup"><span data-stu-id="ffaaf-298">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="ffaaf-299">Mozilla Developer Network： 接受編碼</span><span class="sxs-lookup"><span data-stu-id="ffaaf-299">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="ffaaf-300">RFC 7231 節 3.1.2.1： 內容 Codings</span><span class="sxs-lookup"><span data-stu-id="ffaaf-300">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="ffaaf-301">第 4.2.3 RFC 7230 節： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="ffaaf-301">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="ffaaf-302">GZIP 檔案格式規格 4.3 版</span><span class="sxs-lookup"><span data-stu-id="ffaaf-302">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
