---
title: "ASP.NET Core 壓縮回應中介軟體"
author: guardrex
description: "了解回應壓縮以及如何在 ASP.NET Core 應用程式中使用回應壓縮中介軟體。"
ms.author: riande
manager: wpickett
ms.date: 08/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: performance/response-compression
ms.openlocfilehash: 9270287b62f91ddb81d6a347dd583e1cbb32f3c3
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="response-compression-middleware-for-aspnet-core"></a><span data-ttu-id="b5cab-103">ASP.NET Core 壓縮回應中介軟體</span><span class="sxs-lookup"><span data-stu-id="b5cab-103">Response Compression Middleware for ASP.NET Core</span></span>

<span data-ttu-id="b5cab-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="b5cab-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="b5cab-105">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b5cab-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b5cab-106">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="b5cab-106">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="b5cab-107">降低回應的大小通常應用程式的回應能力通常會大幅增加。</span><span class="sxs-lookup"><span data-stu-id="b5cab-107">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="b5cab-108">若要減少內容大小的方法之一是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="b5cab-108">One way to reduce payload sizes is to compress an app's responses.</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="b5cab-109">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="b5cab-109">When to use Response Compression Middleware</span></span>
<span data-ttu-id="b5cab-110">使用 IIS、 Apache 或 Nginx 中的伺服器回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="b5cab-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="b5cab-111">中介軟體的效能可能不符合伺服器模組。</span><span class="sxs-lookup"><span data-stu-id="b5cab-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="b5cab-112">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)和[Kestrel](xref:fundamentals/servers/kestrel)目前並未提供內建壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="b5cab-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) and [Kestrel](xref:fundamentals/servers/kestrel) don't currently offer built-in compression support.</span></span>

<span data-ttu-id="b5cab-113">當您準備使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="b5cab-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="b5cab-114">無法使用下列伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="b5cab-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="b5cab-115">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="b5cab-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="b5cab-116">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="b5cab-116">Apache mod_deflate module</span></span>](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="b5cab-117">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="b5cab-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="b5cab-118">主機上直接：</span><span class="sxs-lookup"><span data-stu-id="b5cab-118">Hosting directly on:</span></span>
  * <span data-ttu-id="b5cab-119">[HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)(先前稱為[WebListener](xref:fundamentals/servers/weblistener))</span><span class="sxs-lookup"><span data-stu-id="b5cab-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called [WebListener](xref:fundamentals/servers/weblistener))</span></span>
  * [<span data-ttu-id="b5cab-120">Kestrel</span><span class="sxs-lookup"><span data-stu-id="b5cab-120">Kestrel</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="b5cab-121">回應的壓縮</span><span class="sxs-lookup"><span data-stu-id="b5cab-121">Response compression</span></span>
<span data-ttu-id="b5cab-122">通常，任何原本不壓縮的回應可以從回應壓縮中受益。</span><span class="sxs-lookup"><span data-stu-id="b5cab-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="b5cab-123">回應原本不壓縮通常包括： CSS、 JavaScript、 HTML、 XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="b5cab-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="b5cab-124">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="b5cab-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="b5cab-125">如果您嘗試以進一步壓縮原生壓縮的回應，小型其他致使大小和傳輸的時間將很可能會被掩蓋處理壓縮花費的時間。</span><span class="sxs-lookup"><span data-stu-id="b5cab-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="b5cab-126">不壓縮檔案小於大約 150-1000年位元組 （取決於檔案的內容和壓縮效率）。</span><span class="sxs-lookup"><span data-stu-id="b5cab-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="b5cab-127">壓縮的小檔案的額外負荷可能會產生比未壓縮的檔案較大的壓縮的檔。</span><span class="sxs-lookup"><span data-stu-id="b5cab-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="b5cab-128">當用戶端可以處理壓縮的內容時，用戶端必須透過傳送通知的伺服器，其功能`Accept-Encoding`與要求標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="b5cab-129">當伺服器傳送壓縮的內容時，它必須包括中的資訊`Content-Encoding`標頭壓縮的回應編碼的方式。</span><span class="sxs-lookup"><span data-stu-id="b5cab-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="b5cab-130">下表中，會顯示由中介軟體所支援的內容編碼方式指定。</span><span class="sxs-lookup"><span data-stu-id="b5cab-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="b5cab-131">`Accept-Encoding`標頭值</span><span class="sxs-lookup"><span data-stu-id="b5cab-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="b5cab-132">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="b5cab-132">Middleware Supported</span></span> | <span data-ttu-id="b5cab-133">描述</span><span class="sxs-lookup"><span data-stu-id="b5cab-133">Description</span></span>                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | <span data-ttu-id="b5cab-134">否</span><span class="sxs-lookup"><span data-stu-id="b5cab-134">No</span></span>                   | <span data-ttu-id="b5cab-135">Brotli 壓縮的資料格式</span><span class="sxs-lookup"><span data-stu-id="b5cab-135">Brotli Compressed Data Format</span></span>                               |
| `compress`                      | <span data-ttu-id="b5cab-136">否</span><span class="sxs-lookup"><span data-stu-id="b5cab-136">No</span></span>                   | <span data-ttu-id="b5cab-137">UNIX 「 壓縮 」 的資料格式</span><span class="sxs-lookup"><span data-stu-id="b5cab-137">UNIX "compress" data format</span></span>                                 |
| `deflate`                       | <span data-ttu-id="b5cab-138">否</span><span class="sxs-lookup"><span data-stu-id="b5cab-138">No</span></span>                   | <span data-ttu-id="b5cab-139">「 deflate 」 內的 「 zlib 」 資料格式的壓縮的資料</span><span class="sxs-lookup"><span data-stu-id="b5cab-139">"deflate" compressed data inside the "zlib" data format</span></span>     |
| `exi`                           | <span data-ttu-id="b5cab-140">否</span><span class="sxs-lookup"><span data-stu-id="b5cab-140">No</span></span>                   | <span data-ttu-id="b5cab-141">W3C 有效率 XML 交換</span><span class="sxs-lookup"><span data-stu-id="b5cab-141">W3C Efficient XML Interchange</span></span>                               |
| `gzip`                          | <span data-ttu-id="b5cab-142">[是] （預設值）</span><span class="sxs-lookup"><span data-stu-id="b5cab-142">Yes (default)</span></span>        | <span data-ttu-id="b5cab-143">gzip 檔案格式</span><span class="sxs-lookup"><span data-stu-id="b5cab-143">gzip file format</span></span>                                            |
| `identity`                      | <span data-ttu-id="b5cab-144">[是]</span><span class="sxs-lookup"><span data-stu-id="b5cab-144">Yes</span></span>                  | <span data-ttu-id="b5cab-145">「 無編碼 」 的識別項： 必須編碼回應。</span><span class="sxs-lookup"><span data-stu-id="b5cab-145">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="b5cab-146">否</span><span class="sxs-lookup"><span data-stu-id="b5cab-146">No</span></span>                   | <span data-ttu-id="b5cab-147">Java 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="b5cab-147">Network Transfer Format for Java Archives</span></span>                   |
| `*`                             | <span data-ttu-id="b5cab-148">[是]</span><span class="sxs-lookup"><span data-stu-id="b5cab-148">Yes</span></span>                  | <span data-ttu-id="b5cab-149">任何可用的內容編碼不明確要求</span><span class="sxs-lookup"><span data-stu-id="b5cab-149">Any available content encoding not explicitly requested</span></span>     |

<span data-ttu-id="b5cab-150">如需詳細資訊，請參閱[IANA 官方內容編碼清單](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-150">For more information, see the [IANA Official Content Coding List](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="b5cab-151">中介軟體可讓您新增額外的壓縮提供者的自訂`Accept-Encoding`標頭值。</span><span class="sxs-lookup"><span data-stu-id="b5cab-151">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="b5cab-152">如需詳細資訊，請參閱[自訂提供者](#custom-providers)下方。</span><span class="sxs-lookup"><span data-stu-id="b5cab-152">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="b5cab-153">中介軟體是否能夠對回應品質值 (qvalue， `q`) 加權時由用戶端傳送至設定優先權的壓縮配置。</span><span class="sxs-lookup"><span data-stu-id="b5cab-153">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="b5cab-154">如需詳細資訊，請參閱[RFC 7231： 接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-154">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="b5cab-155">壓縮演算法受限於壓縮速度與壓縮的效能之間權衡取捨。</span><span class="sxs-lookup"><span data-stu-id="b5cab-155">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="b5cab-156">*有效性*在此內容中參照輸出大小的壓縮後。</span><span class="sxs-lookup"><span data-stu-id="b5cab-156">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="b5cab-157">最小大小達成最*最佳*壓縮。</span><span class="sxs-lookup"><span data-stu-id="b5cab-157">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="b5cab-158">參與要求的標頭，傳送、 快取，並接收壓縮的內容是下表中所述。</span><span class="sxs-lookup"><span data-stu-id="b5cab-158">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="b5cab-159">頁首</span><span class="sxs-lookup"><span data-stu-id="b5cab-159">Header</span></span>             | <span data-ttu-id="b5cab-160">角色</span><span class="sxs-lookup"><span data-stu-id="b5cab-160">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="b5cab-161">表示配置給用戶端可接受的編碼方式的內容，從用戶端傳送到伺服器。</span><span class="sxs-lookup"><span data-stu-id="b5cab-161">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="b5cab-162">指出在裝載中的內容編碼方式，從伺服器傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="b5cab-162">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="b5cab-163">壓縮時，`Content-Length`移除標頭，因為主體的內容變更壓縮回應時。</span><span class="sxs-lookup"><span data-stu-id="b5cab-163">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="b5cab-164">壓縮時，`Content-MD5`移除標頭，因為本文內容已變更，雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="b5cab-164">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="b5cab-165">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="b5cab-165">Specifies the MIME type of the content.</span></span> <span data-ttu-id="b5cab-166">應該指定每個回應其`Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="b5cab-166">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="b5cab-167">中介軟體會檢查此值來判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="b5cab-167">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="b5cab-168">中介軟體中指定的一組[預設 MIME 類型](#mime-types)即可進行編碼，但您可以取代或新增 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="b5cab-168">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="b5cab-169">值是伺服器所傳送`Accept-Encoding`到用戶端和 proxy，`Vary`標頭指出的用戶端或 proxy，它應該快取 （而有所不同） 的值為基礎的回應`Accept-Encoding`要求標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-169">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="b5cab-170">傳回具有內容的結果`Vary: Accept-Encoding`標頭是同時壓縮，而且未壓縮的回應會分別快取。</span><span class="sxs-lookup"><span data-stu-id="b5cab-170">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="b5cab-171">您可以瀏覽與回應壓縮中介軟體功能[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-171">You can explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="b5cab-172">此範例將說明：</span><span class="sxs-lookup"><span data-stu-id="b5cab-172">The sample illustrates:</span></span>
* <span data-ttu-id="b5cab-173">使用 gzip 和自訂壓縮提供者的應用程式回應的壓縮。</span><span class="sxs-lookup"><span data-stu-id="b5cab-173">The compression of app responses using gzip and custom compression providers.</span></span>
* <span data-ttu-id="b5cab-174">如何將 MIME 類型加入至壓縮的 MIME 類型的預設清單。</span><span class="sxs-lookup"><span data-stu-id="b5cab-174">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="b5cab-175">Package</span><span class="sxs-lookup"><span data-stu-id="b5cab-175">Package</span></span>
<span data-ttu-id="b5cab-176">若要在專案中包含中介軟體，將參考加入[ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝，或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)封裝。</span><span class="sxs-lookup"><span data-stu-id="b5cab-176">To include the middleware in your project, add a reference to the [`Microsoft.AspNetCore.ResponseCompression`](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package or use the [`Microsoft.AspNetCore.All`](https://www.nuget.org/packages/Microsoft.AspNetCore.All/) package.</span></span> <span data-ttu-id="b5cab-177">這項功能是適用於 ASP.NET Core 1.1 版為目標的應用程式或更新版本。</span><span class="sxs-lookup"><span data-stu-id="b5cab-177">This feature is available for apps that target ASP.NET Core 1.1 or later.</span></span>

## <a name="configuration"></a><span data-ttu-id="b5cab-178">組態</span><span class="sxs-lookup"><span data-stu-id="b5cab-178">Configuration</span></span>
<span data-ttu-id="b5cab-179">下列程式碼示範如何啟用回應壓縮中介軟體，以搭配預設 gzip 壓縮和預設的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="b5cab-179">The following code shows how to enable the Response Compression Middleware with the with the default gzip compression and for default MIME types.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5cab-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5cab-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> <span data-ttu-id="b5cab-182">使用這類工具[Fiddler](http://www.telerik.com/fiddler)， [firebug 這類](http://getfirebug.com/)，或[郵差](https://www.getpostman.com/)設定`Accept-Encoding`要求標頭和研究回應標頭、 大小和主體。</span><span class="sxs-lookup"><span data-stu-id="b5cab-182">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="b5cab-183">將要求提交到範例應用程式，而不`Accept-Encoding`標頭，並觀察回應未壓縮。</span><span class="sxs-lookup"><span data-stu-id="b5cab-183">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="b5cab-184">`Content-Encoding`和`Vary`標頭中沒有顯示的回應。</span><span class="sxs-lookup"><span data-stu-id="b5cab-184">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![Fiddler 視窗顯示沒有 Accept-encoding 標頭之要求的結果。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="b5cab-187">將要求提交到範例應用程式與`Accept-Encoding: gzip`標頭，並觀察壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="b5cab-187">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="b5cab-188">`Content-Encoding`和`Vary`標頭會出現在回應上。</span><span class="sxs-lookup"><span data-stu-id="b5cab-188">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![顯示具有 Accept-encoding 標頭之要求的結果而值為 gzip fiddler 視窗。](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="b5cab-192">提供者</span><span class="sxs-lookup"><span data-stu-id="b5cab-192">Providers</span></span>
### <a name="gzipcompressionprovider"></a><span data-ttu-id="b5cab-193">GzipCompressionProvider</span><span class="sxs-lookup"><span data-stu-id="b5cab-193">GzipCompressionProvider</span></span>
<span data-ttu-id="b5cab-194">使用`GzipCompressionProvider`壓縮回應以 gzip。</span><span class="sxs-lookup"><span data-stu-id="b5cab-194">Use the `GzipCompressionProvider` to compress responses with gzip.</span></span> <span data-ttu-id="b5cab-195">如果未指定，這是預設壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="b5cab-195">This is the default compression provider if none are specified.</span></span> <span data-ttu-id="b5cab-196">您可以設定壓縮層級與`GzipCompressionProviderOptions`。</span><span class="sxs-lookup"><span data-stu-id="b5cab-196">You can set the compression level with the `GzipCompressionProviderOptions`.</span></span> 

<span data-ttu-id="b5cab-197">Gzip 壓縮提供者預設為最快的壓縮層級 (`CompressionLevel.Fastest`)，這可能不會產生最有效的壓縮。</span><span class="sxs-lookup"><span data-stu-id="b5cab-197">The gzip compression provider defaults to the fastest compression level (`CompressionLevel.Fastest`), which might not produce the most efficient compression.</span></span> <span data-ttu-id="b5cab-198">如果想要使用最有效率的壓縮，您可以設定最佳的壓縮的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b5cab-198">If the most efficient compression is desired, you can configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="b5cab-199">壓縮層級</span><span class="sxs-lookup"><span data-stu-id="b5cab-199">Compression Level</span></span>                | <span data-ttu-id="b5cab-200">描述</span><span class="sxs-lookup"><span data-stu-id="b5cab-200">Description</span></span>                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | <span data-ttu-id="b5cab-201">即使未以最佳方式壓縮所產生的輸出，應該盡快，完成壓縮。</span><span class="sxs-lookup"><span data-stu-id="b5cab-201">Compression should complete as quickly as possible, even if the resulting output is not optimally compressed.</span></span> |
| `CompressionLevel.NoCompression` | <span data-ttu-id="b5cab-202">您應該不執行任何壓縮。</span><span class="sxs-lookup"><span data-stu-id="b5cab-202">No compression should be performed.</span></span>                                                                           |
| `CompressionLevel.Optimal`       | <span data-ttu-id="b5cab-203">回應應以最佳方式壓縮，即使壓縮會使用更多時間來完成。</span><span class="sxs-lookup"><span data-stu-id="b5cab-203">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span>                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5cab-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5cab-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a><span data-ttu-id="b5cab-206">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="b5cab-206">MIME types</span></span>
<span data-ttu-id="b5cab-207">中介軟體會指定一組預設的 MIME 類型的壓縮：</span><span class="sxs-lookup"><span data-stu-id="b5cab-207">The middleware specifies a default set of MIME types for compression:</span></span>
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

<span data-ttu-id="b5cab-208">您可以取代或附加的回應壓縮中介軟體選項的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="b5cab-208">You can replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="b5cab-209">請注意，萬用字元 MIME 類型，例如`text/*`不支援。</span><span class="sxs-lookup"><span data-stu-id="b5cab-209">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="b5cab-210">範例應用程式新增 MIME 類型`image/svg+xml`和壓縮，並做 ASP.NET Core 橫幅影像 (*banner.svg*)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-210">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5cab-211">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-211">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5cab-212">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-212">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a><span data-ttu-id="b5cab-213">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="b5cab-213">Custom providers</span></span>
<span data-ttu-id="b5cab-214">您可以建立自訂壓縮實作與`ICompressionProvider`。</span><span class="sxs-lookup"><span data-stu-id="b5cab-214">You can create custom compression implementations with `ICompressionProvider`.</span></span> <span data-ttu-id="b5cab-215">`EncodingName`代表內容的編碼這個`ICompressionProvider`產生。</span><span class="sxs-lookup"><span data-stu-id="b5cab-215">The `EncodingName` represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="b5cab-216">中介軟體會使用此資訊來選擇根據清單中指定的提供者`Accept-Encoding`要求標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-216">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="b5cab-217">使用範例應用程式，在用戶端提交的要求`Accept-Encoding: mycustomcompression`標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-217">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="b5cab-218">中介軟體會使用自訂壓縮實作，並傳回與回應`Content-Encoding: mycustomcompression`標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-218">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="b5cab-219">用戶端必須能夠解壓縮自訂壓縮實作，才能讓自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="b5cab-219">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b5cab-220">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-220">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b5cab-221">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b5cab-221">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

<span data-ttu-id="b5cab-222">將要求提交到範例應用程式與`Accept-Encoding: mycustomcompression`標頭，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-222">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="b5cab-223">`Vary`和`Content-Encoding`標頭會出現在回應上。</span><span class="sxs-lookup"><span data-stu-id="b5cab-223">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="b5cab-224">此範例不被壓縮 （未顯示），回應主體。</span><span class="sxs-lookup"><span data-stu-id="b5cab-224">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="b5cab-225">沒有在壓縮實作`CustomCompressionProvider`範例的類別。</span><span class="sxs-lookup"><span data-stu-id="b5cab-225">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="b5cab-226">不過，此範例會示範您可以在其中實作這類的壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="b5cab-226">However, the sample shows where you would implement such a compression algorithm.</span></span>

![顯示具有 Accept-encoding 標頭之要求的結果而值為 mycustomcompression fiddler 視窗。](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="b5cab-229">使用安全通訊協定的壓縮</span><span class="sxs-lookup"><span data-stu-id="b5cab-229">Compression with secure protocol</span></span>
<span data-ttu-id="b5cab-230">透過安全的連線壓縮的回應可以控制與`EnableForHttps`選項，預設會停用。</span><span class="sxs-lookup"><span data-stu-id="b5cab-230">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="b5cab-231">使用動態產生的頁面壓縮可能會導致安全性問題例如[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))和[破壞](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="b5cab-231">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="b5cab-232">加入 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="b5cab-232">Adding the Vary header</span></span>
<span data-ttu-id="b5cab-233">當壓縮回應基礎`Accept-Encoding`標頭，但是會有潛在的多個壓縮的版本回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="b5cab-233">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="b5cab-234">若要指示用戶端和 proxy 快取多個版本存在，而且應該儲存`Vary`標頭加入使用`Accept-Encoding`值。</span><span class="sxs-lookup"><span data-stu-id="b5cab-234">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="b5cab-235">在 ASP.NET Core 1.x 加入`Vary`標頭至回應以手動方式完成。</span><span class="sxs-lookup"><span data-stu-id="b5cab-235">In ASP.NET Core 1.x, adding the `Vary` header to the response is accomplished manually.</span></span> <span data-ttu-id="b5cab-236">在 ASP.NET Core 2.x 中, 介軟體新增`Vary`標頭壓縮回應時，自動。</span><span class="sxs-lookup"><span data-stu-id="b5cab-236">In ASP.NET Core 2.x, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

<span data-ttu-id="b5cab-237">**ASP.NET Core 只 1.x**</span><span class="sxs-lookup"><span data-stu-id="b5cab-237">**ASP.NET Core 1.x only**</span></span>

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="b5cab-238">位於 Nginx 反向 proxy 後方的中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="b5cab-238">Middleware issue when behind an Nginx reverse proxy</span></span>
<span data-ttu-id="b5cab-239">當要求 Nginx，由代理`Accept-Encoding`標頭會移除。</span><span class="sxs-lookup"><span data-stu-id="b5cab-239">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="b5cab-240">這可防止壓縮回應的中介軟體。</span><span class="sxs-lookup"><span data-stu-id="b5cab-240">This prevents the middleware from compressing the response.</span></span> <span data-ttu-id="b5cab-241">如需詳細資訊，請參閱[NGINX： 壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-241">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="b5cab-242">此問題會追蹤[找出 Nginx (BasicMiddleware #123) 傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-242">This issue is tracked by [Figure out pass-through compression for Nginx (BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="b5cab-243">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="b5cab-243">Working with IIS dynamic compression</span></span>
<span data-ttu-id="b5cab-244">如果您有使用中 IIS 動態壓縮模組在您想要停用應用程式的伺服器層級設定，您可以新增至與您*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="b5cab-244">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, you can do so with an addition to your *web.config* file.</span></span> <span data-ttu-id="b5cab-245">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-245">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="b5cab-246">疑難排解</span><span class="sxs-lookup"><span data-stu-id="b5cab-246">Troubleshooting</span></span>
<span data-ttu-id="b5cab-247">使用這類工具[Fiddler](http://www.telerik.com/fiddler)， [firebug 這類](http://getfirebug.com/)，或[郵差](https://www.getpostman.com/)，可讓您設定`Accept-Encoding`要求標頭和研究回應標頭、 大小和主體。</span><span class="sxs-lookup"><span data-stu-id="b5cab-247">Use a tool like [Fiddler](http://www.telerik.com/fiddler), [Firebug](http://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="b5cab-248">回應壓縮中介軟體壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="b5cab-248">The Response Compression Middleware compresses responses that meet the following conditions:</span></span>
* <span data-ttu-id="b5cab-249">`Accept-Encoding`標頭已存在的值與`gzip`， `*`，或自訂編碼符合您所建立的自訂壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="b5cab-249">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="b5cab-250">值不能`identity`或有品質值 (qvalue， `q`) 設定為 0 （零）。</span><span class="sxs-lookup"><span data-stu-id="b5cab-250">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="b5cab-251">MIME 類型 (`Content-Type`) 必須設定，而且必須符合上設定 MIME 類型`ResponseCompressionOptions`。</span><span class="sxs-lookup"><span data-stu-id="b5cab-251">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the `ResponseCompressionOptions`.</span></span>
* <span data-ttu-id="b5cab-252">要求必須包含`Content-Range`標頭。</span><span class="sxs-lookup"><span data-stu-id="b5cab-252">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="b5cab-253">除非回應壓縮中介軟體選項中設定安全通訊協定 (https)，要求必須使用不安全的通訊協定 (http)。</span><span class="sxs-lookup"><span data-stu-id="b5cab-253">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="b5cab-254">*請注意危險[上述](#compression-with-secure-protocol)時啟用安全內容的壓縮。*</span><span class="sxs-lookup"><span data-stu-id="b5cab-254">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b5cab-255">其他資源</span><span class="sxs-lookup"><span data-stu-id="b5cab-255">Additional resources</span></span>
* [<span data-ttu-id="b5cab-256">應用程式啟動</span><span class="sxs-lookup"><span data-stu-id="b5cab-256">Application Startup</span></span>](xref:fundamentals/startup)
* [<span data-ttu-id="b5cab-257">中介軟體</span><span class="sxs-lookup"><span data-stu-id="b5cab-257">Middleware</span></span>](xref:fundamentals/middleware)
* [<span data-ttu-id="b5cab-258">Mozilla Developer Network： 接受編碼</span><span class="sxs-lookup"><span data-stu-id="b5cab-258">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="b5cab-259">RFC 7231 區段 3.1.2.1： 內容 Codings</span><span class="sxs-lookup"><span data-stu-id="b5cab-259">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="b5cab-260">RFC 7230 第 4.2.3 節： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="b5cab-260">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="b5cab-261">GZIP 檔案格式規格 4.3 版</span><span class="sxs-lookup"><span data-stu-id="b5cab-261">GZIP file format specification version 4.3</span></span>](http://www.ietf.org/rfc/rfc1952.txt)
