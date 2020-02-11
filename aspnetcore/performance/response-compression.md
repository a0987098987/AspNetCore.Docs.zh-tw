---
title: ASP.NET Core 中的回應壓縮
author: guardrex
description: 了解回應壓縮及如何使用 ASP.NET Core 應用程式中的回應壓縮中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/response-compression
ms.openlocfilehash: d37b05edd55ac0d3910855563b819114cf815b43
ms.sourcegitcommit: 235623b6e5a5d1841139c82a11ac2b4b3f31a7a9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/10/2020
ms.locfileid: "77114805"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="7b283-103">ASP.NET Core 中的回應壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-103">Response compression in ASP.NET Core</span></span>

<span data-ttu-id="7b283-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7b283-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7b283-105">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="7b283-105">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="7b283-106">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="7b283-106">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="7b283-107">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-107">One way to reduce payload sizes is to compress an app's responses.</span></span>

<span data-ttu-id="7b283-108">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b283-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="7b283-109">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="7b283-109">When to use Response Compression Middleware</span></span>

<span data-ttu-id="7b283-110">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="7b283-110">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="7b283-111">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="7b283-111">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="7b283-112">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="7b283-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="7b283-113">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b283-113">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="7b283-114">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="7b283-114">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="7b283-115">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="7b283-115">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="7b283-116">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="7b283-116">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="7b283-117">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-117">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="7b283-118">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="7b283-118">Hosting directly on:</span></span>
  * <span data-ttu-id="7b283-119">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="7b283-119">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="7b283-120">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="7b283-120">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="7b283-121">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-121">Response compression</span></span>

<span data-ttu-id="7b283-122">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="7b283-122">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="7b283-123">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="7b283-123">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="7b283-124">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="7b283-124">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="7b283-125">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="7b283-125">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="7b283-126">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="7b283-126">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="7b283-127">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="7b283-127">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="7b283-128">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="7b283-128">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="7b283-129">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-129">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="7b283-130">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-130">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="7b283-131">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="7b283-131">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7b283-132">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="7b283-132">Middleware Supported</span></span> | <span data-ttu-id="7b283-133">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-133">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7b283-134">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="7b283-134">Yes (default)</span></span>        | [<span data-ttu-id="7b283-135">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="7b283-135">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7b283-136">否</span><span class="sxs-lookup"><span data-stu-id="7b283-136">No</span></span>                   | [<span data-ttu-id="7b283-137">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="7b283-137">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7b283-138">否</span><span class="sxs-lookup"><span data-stu-id="7b283-138">No</span></span>                   | [<span data-ttu-id="7b283-139">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="7b283-139">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7b283-140">是</span><span class="sxs-lookup"><span data-stu-id="7b283-140">Yes</span></span>                  | [<span data-ttu-id="7b283-141">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="7b283-141">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7b283-142">是</span><span class="sxs-lookup"><span data-stu-id="7b283-142">Yes</span></span>                  | <span data-ttu-id="7b283-143">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-143">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7b283-144">否</span><span class="sxs-lookup"><span data-stu-id="7b283-144">No</span></span>                   | [<span data-ttu-id="7b283-145">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="7b283-145">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7b283-146">是</span><span class="sxs-lookup"><span data-stu-id="7b283-146">Yes</span></span>                  | <span data-ttu-id="7b283-147">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-147">Any available content encoding not explicitly requested</span></span> |

<span data-ttu-id="7b283-148">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="7b283-148">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="7b283-149">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="7b283-149">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="7b283-150">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="7b283-150">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="7b283-151">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="7b283-151">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="7b283-152">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="7b283-152">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="7b283-153">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="7b283-153">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="7b283-154">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="7b283-154">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="7b283-155">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="7b283-155">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="7b283-156">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-156">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="7b283-157">頁首</span><span class="sxs-lookup"><span data-stu-id="7b283-157">Header</span></span>             | <span data-ttu-id="7b283-158">角色</span><span class="sxs-lookup"><span data-stu-id="7b283-158">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="7b283-159">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="7b283-159">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="7b283-160">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-160">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="7b283-161">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="7b283-161">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="7b283-162">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="7b283-162">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="7b283-163">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-163">Specifies the MIME type of the content.</span></span> <span data-ttu-id="7b283-164">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="7b283-164">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="7b283-165">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-165">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="7b283-166">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-166">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="7b283-167">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-167">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="7b283-168">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-168">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="7b283-169">使用[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="7b283-169">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="7b283-170">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="7b283-170">The sample illustrates:</span></span>

* <span data-ttu-id="7b283-171">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-171">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="7b283-172">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-172">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="7b283-173">Package</span><span class="sxs-lookup"><span data-stu-id="7b283-173">Package</span></span>

<span data-ttu-id="7b283-174">回應壓縮中介軟體是由[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)套件所提供，它會隱含地包含在 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="7b283-174">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

## <a name="configuration"></a><span data-ttu-id="7b283-175">組態</span><span class="sxs-lookup"><span data-stu-id="7b283-175">Configuration</span></span>

<span data-ttu-id="7b283-176">下列程式碼示範如何針對預設 MIME 類型和壓縮提供者（[Brotli](#brotli-compression-provider)和[Gzip](#gzip-compression-provider)）啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b283-176">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

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

<span data-ttu-id="7b283-177">注意：</span><span class="sxs-lookup"><span data-stu-id="7b283-177">Notes:</span></span>

* <span data-ttu-id="7b283-178">`app.UseResponseCompression` 必須在壓縮回應的任何中介軟體之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="7b283-178">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="7b283-179">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#middleware-order>。</span><span class="sxs-lookup"><span data-stu-id="7b283-179">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="7b283-180">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="7b283-180">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="7b283-181">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-181">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="7b283-182">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-182">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="7b283-185">使用 `Accept-Encoding: br` 標頭（Brotli 壓縮）將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-185">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="7b283-186">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-186">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 br 之要求的結果。](response-compression/_static/request-compressed-br.png)

## <a name="providers"></a><span data-ttu-id="7b283-190">提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-190">Providers</span></span>

### <a name="brotli-compression-provider"></a><span data-ttu-id="7b283-191">Brotli 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-191">Brotli Compression Provider</span></span>

<span data-ttu-id="7b283-192">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider>，以[Brotli 壓縮資料格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-192">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="7b283-193">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="7b283-193">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7b283-194">Brotli 壓縮提供者預設會加入至壓縮提供者陣列，以及[Gzip 壓縮提供者](#gzip-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="7b283-194">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="7b283-195">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-195">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7b283-196">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="7b283-196">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7b283-197">明確新增任何壓縮提供者時，必須新增 Brotoli 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="7b283-197">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="7b283-198">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="7b283-198">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="7b283-199">Brotli 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-199">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7b283-200">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-200">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7b283-201">Compression Level</span><span class="sxs-lookup"><span data-stu-id="7b283-201">Compression Level</span></span> | <span data-ttu-id="7b283-202">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-202">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7b283-203">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="7b283-203">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-204">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="7b283-204">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7b283-205">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7b283-205">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-206">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-206">No compression should be performed.</span></span> |
| [<span data-ttu-id="7b283-207">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="7b283-207">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-208">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="7b283-208">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="7b283-209">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-209">Gzip Compression Provider</span></span>

<span data-ttu-id="7b283-210">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-210">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="7b283-211">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="7b283-211">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7b283-212">Gzip 壓縮提供者預設會加入壓縮提供者的陣列，以及[Brotli 壓縮提供者](#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="7b283-212">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="7b283-213">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-213">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7b283-214">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="7b283-214">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7b283-215">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="7b283-215">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="7b283-216">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="7b283-216">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="7b283-217">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-217">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7b283-218">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-218">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7b283-219">Compression Level</span><span class="sxs-lookup"><span data-stu-id="7b283-219">Compression Level</span></span> | <span data-ttu-id="7b283-220">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-220">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7b283-221">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="7b283-221">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-222">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="7b283-222">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7b283-223">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7b283-223">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-224">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-224">No compression should be performed.</span></span> |
| [<span data-ttu-id="7b283-225">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="7b283-225">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-226">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="7b283-226">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="7b283-227">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-227">Custom providers</span></span>

<span data-ttu-id="7b283-228">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-228">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="7b283-229"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-229">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="7b283-230">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="7b283-230">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="7b283-231">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="7b283-231">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7b283-232">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-232">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7b283-233">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="7b283-233">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]


<span data-ttu-id="7b283-234">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-234">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="7b283-235">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-235">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="7b283-236">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="7b283-236">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="7b283-237">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="7b283-237">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="7b283-238">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="7b283-238">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="7b283-241">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="7b283-241">MIME types</span></span>

<span data-ttu-id="7b283-242">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="7b283-242">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="7b283-243">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-243">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="7b283-244">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="7b283-244">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="7b283-245">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="7b283-245">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="7b283-246">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-246">Compression with secure protocol</span></span>

<span data-ttu-id="7b283-247">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="7b283-247">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="7b283-248">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="7b283-248">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="7b283-249">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="7b283-249">Adding the Vary header</span></span>

<span data-ttu-id="7b283-250">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="7b283-250">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="7b283-251">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="7b283-251">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="7b283-252">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-252">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="7b283-253">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="7b283-253">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="7b283-254">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-254">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="7b283-255">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-255">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="7b283-256">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="7b283-256">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="7b283-257">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="7b283-257">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="7b283-258">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-258">Working with IIS dynamic compression</span></span>

<span data-ttu-id="7b283-259">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="7b283-259">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="7b283-260">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="7b283-260">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7b283-261">疑難排解</span><span class="sxs-lookup"><span data-stu-id="7b283-261">Troubleshooting</span></span>

<span data-ttu-id="7b283-262">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="7b283-262">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="7b283-263">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="7b283-263">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="7b283-264">`Accept-Encoding` 標頭存在，其值為 `br`、`gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-264">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7b283-265">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="7b283-265">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7b283-266">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-266">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7b283-267">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-267">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7b283-268">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="7b283-268">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7b283-269">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="7b283-269">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b283-270">其他資源</span><span class="sxs-lookup"><span data-stu-id="7b283-270">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="7b283-271">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-271">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="7b283-272">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="7b283-272">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="7b283-273">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-273">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="7b283-274">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="7b283-274">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="7b283-275">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="7b283-275">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="7b283-276">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="7b283-276">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="7b283-277">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-277">One way to reduce payload sizes is to compress an app's responses.</span></span>

<span data-ttu-id="7b283-278">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b283-278">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="7b283-279">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="7b283-279">When to use Response Compression Middleware</span></span>

<span data-ttu-id="7b283-280">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="7b283-280">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="7b283-281">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="7b283-281">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="7b283-282">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="7b283-282">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="7b283-283">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b283-283">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="7b283-284">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="7b283-284">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="7b283-285">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="7b283-285">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="7b283-286">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="7b283-286">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="7b283-287">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-287">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="7b283-288">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="7b283-288">Hosting directly on:</span></span>
  * <span data-ttu-id="7b283-289">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="7b283-289">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="7b283-290">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="7b283-290">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="7b283-291">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-291">Response compression</span></span>

<span data-ttu-id="7b283-292">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="7b283-292">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="7b283-293">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="7b283-293">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="7b283-294">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="7b283-294">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="7b283-295">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="7b283-295">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="7b283-296">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="7b283-296">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="7b283-297">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="7b283-297">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="7b283-298">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="7b283-298">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="7b283-299">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-299">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="7b283-300">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-300">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="7b283-301">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="7b283-301">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7b283-302">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="7b283-302">Middleware Supported</span></span> | <span data-ttu-id="7b283-303">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-303">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7b283-304">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="7b283-304">Yes (default)</span></span>        | [<span data-ttu-id="7b283-305">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="7b283-305">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7b283-306">否</span><span class="sxs-lookup"><span data-stu-id="7b283-306">No</span></span>                   | [<span data-ttu-id="7b283-307">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="7b283-307">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7b283-308">否</span><span class="sxs-lookup"><span data-stu-id="7b283-308">No</span></span>                   | [<span data-ttu-id="7b283-309">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="7b283-309">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7b283-310">是</span><span class="sxs-lookup"><span data-stu-id="7b283-310">Yes</span></span>                  | [<span data-ttu-id="7b283-311">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="7b283-311">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7b283-312">是</span><span class="sxs-lookup"><span data-stu-id="7b283-312">Yes</span></span>                  | <span data-ttu-id="7b283-313">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-313">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7b283-314">否</span><span class="sxs-lookup"><span data-stu-id="7b283-314">No</span></span>                   | [<span data-ttu-id="7b283-315">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="7b283-315">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7b283-316">是</span><span class="sxs-lookup"><span data-stu-id="7b283-316">Yes</span></span>                  | <span data-ttu-id="7b283-317">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-317">Any available content encoding not explicitly requested</span></span> |

<span data-ttu-id="7b283-318">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="7b283-318">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="7b283-319">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="7b283-319">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="7b283-320">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="7b283-320">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="7b283-321">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="7b283-321">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="7b283-322">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="7b283-322">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="7b283-323">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="7b283-323">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="7b283-324">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="7b283-324">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="7b283-325">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="7b283-325">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="7b283-326">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-326">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="7b283-327">頁首</span><span class="sxs-lookup"><span data-stu-id="7b283-327">Header</span></span>             | <span data-ttu-id="7b283-328">角色</span><span class="sxs-lookup"><span data-stu-id="7b283-328">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="7b283-329">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="7b283-329">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="7b283-330">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-330">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="7b283-331">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="7b283-331">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="7b283-332">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="7b283-332">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="7b283-333">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-333">Specifies the MIME type of the content.</span></span> <span data-ttu-id="7b283-334">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="7b283-334">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="7b283-335">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-335">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="7b283-336">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-336">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="7b283-337">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-337">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="7b283-338">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-338">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="7b283-339">使用[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="7b283-339">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="7b283-340">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="7b283-340">The sample illustrates:</span></span>

* <span data-ttu-id="7b283-341">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-341">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="7b283-342">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-342">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="7b283-343">Package</span><span class="sxs-lookup"><span data-stu-id="7b283-343">Package</span></span>

<span data-ttu-id="7b283-344">若要在專案中包含中介軟體，請新增[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)的參考，其中包含[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="7b283-344">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

## <a name="configuration"></a><span data-ttu-id="7b283-345">組態</span><span class="sxs-lookup"><span data-stu-id="7b283-345">Configuration</span></span>

<span data-ttu-id="7b283-346">下列程式碼示範如何針對預設 MIME 類型和壓縮提供者（[Brotli](#brotli-compression-provider)和[Gzip](#gzip-compression-provider)）啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b283-346">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

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

<span data-ttu-id="7b283-347">注意：</span><span class="sxs-lookup"><span data-stu-id="7b283-347">Notes:</span></span>

* <span data-ttu-id="7b283-348">`app.UseResponseCompression` 必須在壓縮回應的任何中介軟體之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="7b283-348">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="7b283-349">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#middleware-order>。</span><span class="sxs-lookup"><span data-stu-id="7b283-349">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="7b283-350">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="7b283-350">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="7b283-351">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-351">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="7b283-352">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-352">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="7b283-355">使用 `Accept-Encoding: br` 標頭（Brotli 壓縮）將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-355">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="7b283-356">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-356">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 br 之要求的結果。](response-compression/_static/request-compressed-br.png)

## <a name="providers"></a><span data-ttu-id="7b283-360">提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-360">Providers</span></span>

### <a name="brotli-compression-provider"></a><span data-ttu-id="7b283-361">Brotli 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-361">Brotli Compression Provider</span></span>

<span data-ttu-id="7b283-362">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider>，以[Brotli 壓縮資料格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-362">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="7b283-363">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="7b283-363">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7b283-364">Brotli 壓縮提供者預設會加入至壓縮提供者陣列，以及[Gzip 壓縮提供者](#gzip-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="7b283-364">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="7b283-365">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-365">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7b283-366">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="7b283-366">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7b283-367">明確新增任何壓縮提供者時，必須新增 Brotoli 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="7b283-367">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="7b283-368">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="7b283-368">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="7b283-369">Brotli 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-369">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7b283-370">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-370">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7b283-371">Compression Level</span><span class="sxs-lookup"><span data-stu-id="7b283-371">Compression Level</span></span> | <span data-ttu-id="7b283-372">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-372">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7b283-373">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="7b283-373">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-374">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="7b283-374">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7b283-375">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7b283-375">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-376">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-376">No compression should be performed.</span></span> |
| [<span data-ttu-id="7b283-377">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="7b283-377">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-378">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="7b283-378">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="7b283-379">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-379">Gzip Compression Provider</span></span>

<span data-ttu-id="7b283-380">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-380">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="7b283-381">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="7b283-381">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7b283-382">Gzip 壓縮提供者預設會加入壓縮提供者的陣列，以及[Brotli 壓縮提供者](#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="7b283-382">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="7b283-383">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-383">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="7b283-384">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="7b283-384">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7b283-385">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="7b283-385">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="7b283-386">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="7b283-386">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="7b283-387">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-387">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7b283-388">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-388">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7b283-389">Compression Level</span><span class="sxs-lookup"><span data-stu-id="7b283-389">Compression Level</span></span> | <span data-ttu-id="7b283-390">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-390">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7b283-391">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="7b283-391">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-392">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="7b283-392">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7b283-393">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7b283-393">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-394">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-394">No compression should be performed.</span></span> |
| [<span data-ttu-id="7b283-395">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="7b283-395">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-396">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="7b283-396">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="7b283-397">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-397">Custom providers</span></span>

<span data-ttu-id="7b283-398">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-398">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="7b283-399"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-399">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="7b283-400">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="7b283-400">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="7b283-401">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="7b283-401">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7b283-402">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-402">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7b283-403">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="7b283-403">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

<span data-ttu-id="7b283-404">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-404">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="7b283-405">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-405">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="7b283-406">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="7b283-406">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="7b283-407">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="7b283-407">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="7b283-408">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="7b283-408">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="7b283-411">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="7b283-411">MIME types</span></span>

<span data-ttu-id="7b283-412">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="7b283-412">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="7b283-413">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-413">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="7b283-414">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="7b283-414">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="7b283-415">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="7b283-415">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="7b283-416">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-416">Compression with secure protocol</span></span>

<span data-ttu-id="7b283-417">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="7b283-417">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="7b283-418">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="7b283-418">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="7b283-419">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="7b283-419">Adding the Vary header</span></span>

<span data-ttu-id="7b283-420">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="7b283-420">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="7b283-421">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="7b283-421">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="7b283-422">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-422">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="7b283-423">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="7b283-423">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="7b283-424">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-424">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="7b283-425">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-425">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="7b283-426">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="7b283-426">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="7b283-427">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="7b283-427">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="7b283-428">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-428">Working with IIS dynamic compression</span></span>

<span data-ttu-id="7b283-429">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="7b283-429">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="7b283-430">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="7b283-430">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7b283-431">疑難排解</span><span class="sxs-lookup"><span data-stu-id="7b283-431">Troubleshooting</span></span>

<span data-ttu-id="7b283-432">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="7b283-432">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="7b283-433">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="7b283-433">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="7b283-434">`Accept-Encoding` 標頭存在，其值為 `br`、`gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-434">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7b283-435">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="7b283-435">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7b283-436">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-436">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7b283-437">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-437">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7b283-438">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="7b283-438">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7b283-439">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="7b283-439">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b283-440">其他資源</span><span class="sxs-lookup"><span data-stu-id="7b283-440">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="7b283-441">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-441">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="7b283-442">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="7b283-442">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="7b283-443">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-443">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="7b283-444">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="7b283-444">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="7b283-445">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="7b283-445">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="7b283-446">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="7b283-446">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="7b283-447">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-447">One way to reduce payload sizes is to compress an app's responses.</span></span>

<span data-ttu-id="7b283-448">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7b283-448">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="7b283-449">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="7b283-449">When to use Response Compression Middleware</span></span>

<span data-ttu-id="7b283-450">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="7b283-450">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="7b283-451">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="7b283-451">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="7b283-452">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="7b283-452">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="7b283-453">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b283-453">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="7b283-454">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="7b283-454">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="7b283-455">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="7b283-455">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="7b283-456">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="7b283-456">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="7b283-457">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-457">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="7b283-458">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="7b283-458">Hosting directly on:</span></span>
  * <span data-ttu-id="7b283-459">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="7b283-459">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="7b283-460">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="7b283-460">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="7b283-461">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-461">Response compression</span></span>

<span data-ttu-id="7b283-462">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="7b283-462">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="7b283-463">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="7b283-463">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="7b283-464">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="7b283-464">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="7b283-465">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="7b283-465">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="7b283-466">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="7b283-466">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="7b283-467">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="7b283-467">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="7b283-468">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="7b283-468">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="7b283-469">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-469">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="7b283-470">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-470">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="7b283-471">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="7b283-471">`Accept-Encoding` header values</span></span> | <span data-ttu-id="7b283-472">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="7b283-472">Middleware Supported</span></span> | <span data-ttu-id="7b283-473">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-473">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="7b283-474">否</span><span class="sxs-lookup"><span data-stu-id="7b283-474">No</span></span>                   | [<span data-ttu-id="7b283-475">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="7b283-475">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="7b283-476">否</span><span class="sxs-lookup"><span data-stu-id="7b283-476">No</span></span>                   | [<span data-ttu-id="7b283-477">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="7b283-477">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="7b283-478">否</span><span class="sxs-lookup"><span data-stu-id="7b283-478">No</span></span>                   | [<span data-ttu-id="7b283-479">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="7b283-479">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="7b283-480">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="7b283-480">Yes (default)</span></span>        | [<span data-ttu-id="7b283-481">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="7b283-481">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="7b283-482">是</span><span class="sxs-lookup"><span data-stu-id="7b283-482">Yes</span></span>                  | <span data-ttu-id="7b283-483">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-483">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="7b283-484">否</span><span class="sxs-lookup"><span data-stu-id="7b283-484">No</span></span>                   | [<span data-ttu-id="7b283-485">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="7b283-485">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="7b283-486">是</span><span class="sxs-lookup"><span data-stu-id="7b283-486">Yes</span></span>                  | <span data-ttu-id="7b283-487">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-487">Any available content encoding not explicitly requested</span></span> |

<span data-ttu-id="7b283-488">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="7b283-488">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="7b283-489">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="7b283-489">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="7b283-490">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="7b283-490">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="7b283-491">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="7b283-491">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="7b283-492">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="7b283-492">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="7b283-493">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="7b283-493">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="7b283-494">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="7b283-494">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="7b283-495">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="7b283-495">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="7b283-496">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-496">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="7b283-497">頁首</span><span class="sxs-lookup"><span data-stu-id="7b283-497">Header</span></span>             | <span data-ttu-id="7b283-498">角色</span><span class="sxs-lookup"><span data-stu-id="7b283-498">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="7b283-499">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="7b283-499">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="7b283-500">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="7b283-500">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="7b283-501">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="7b283-501">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="7b283-502">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="7b283-502">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="7b283-503">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-503">Specifies the MIME type of the content.</span></span> <span data-ttu-id="7b283-504">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="7b283-504">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="7b283-505">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-505">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="7b283-506">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-506">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="7b283-507">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-507">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="7b283-508">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-508">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="7b283-509">使用[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="7b283-509">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="7b283-510">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="7b283-510">The sample illustrates:</span></span>

* <span data-ttu-id="7b283-511">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-511">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="7b283-512">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-512">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="7b283-513">Package</span><span class="sxs-lookup"><span data-stu-id="7b283-513">Package</span></span>

<span data-ttu-id="7b283-514">若要在專案中包含中介軟體，請新增[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)的參考，其中包含[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="7b283-514">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

## <a name="configuration"></a><span data-ttu-id="7b283-515">組態</span><span class="sxs-lookup"><span data-stu-id="7b283-515">Configuration</span></span>

<span data-ttu-id="7b283-516">下列程式碼示範如何為預設 MIME 類型和[Gzip 壓縮提供者](#gzip-compression-provider)啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="7b283-516">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="7b283-517">注意：</span><span class="sxs-lookup"><span data-stu-id="7b283-517">Notes:</span></span>

* <span data-ttu-id="7b283-518">`app.UseResponseCompression` 必須在壓縮回應的任何中介軟體之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="7b283-518">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="7b283-519">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#middleware-order>。</span><span class="sxs-lookup"><span data-stu-id="7b283-519">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="7b283-520">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="7b283-520">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="7b283-521">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-521">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="7b283-522">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-522">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="7b283-525">使用 `Accept-Encoding: gzip` 標頭將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-525">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="7b283-526">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-526">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和 gzip 值之要求的結果。](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="7b283-530">提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-530">Providers</span></span>

### <a name="gzip-compression-provider"></a><span data-ttu-id="7b283-531">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-531">Gzip Compression Provider</span></span>

<span data-ttu-id="7b283-532">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-532">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="7b283-533">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="7b283-533">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="7b283-534">Gzip 壓縮提供者預設會加入至壓縮提供者陣列。</span><span class="sxs-lookup"><span data-stu-id="7b283-534">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="7b283-535">當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="7b283-535">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="7b283-536">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="7b283-536">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="7b283-537">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="7b283-537">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="7b283-538">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-538">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="7b283-539">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-539">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="7b283-540">Compression Level</span><span class="sxs-lookup"><span data-stu-id="7b283-540">Compression Level</span></span> | <span data-ttu-id="7b283-541">描述</span><span class="sxs-lookup"><span data-stu-id="7b283-541">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="7b283-542">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="7b283-542">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-543">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="7b283-543">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="7b283-544">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="7b283-544">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-545">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-545">No compression should be performed.</span></span> |
| [<span data-ttu-id="7b283-546">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="7b283-546">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="7b283-547">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="7b283-547">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="7b283-548">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="7b283-548">Custom providers</span></span>

<span data-ttu-id="7b283-549">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="7b283-549">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="7b283-550"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-550">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="7b283-551">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="7b283-551">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="7b283-552">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="7b283-552">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7b283-553">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-553">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="7b283-554">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="7b283-554">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

<span data-ttu-id="7b283-555">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-555">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="7b283-556">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="7b283-556">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="7b283-557">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="7b283-557">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="7b283-558">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="7b283-558">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="7b283-559">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="7b283-559">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="7b283-562">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="7b283-562">MIME types</span></span>

<span data-ttu-id="7b283-563">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="7b283-563">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="7b283-564">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-564">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="7b283-565">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="7b283-565">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="7b283-566">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="7b283-566">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="7b283-567">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-567">Compression with secure protocol</span></span>

<span data-ttu-id="7b283-568">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="7b283-568">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="7b283-569">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="7b283-569">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="7b283-570">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="7b283-570">Adding the Vary header</span></span>

<span data-ttu-id="7b283-571">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="7b283-571">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="7b283-572">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="7b283-572">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="7b283-573">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-573">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="7b283-574">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="7b283-574">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="7b283-575">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-575">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="7b283-576">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="7b283-576">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="7b283-577">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="7b283-577">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="7b283-578">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="7b283-578">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="7b283-579">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="7b283-579">Working with IIS dynamic compression</span></span>

<span data-ttu-id="7b283-580">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="7b283-580">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="7b283-581">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="7b283-581">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7b283-582">疑難排解</span><span class="sxs-lookup"><span data-stu-id="7b283-582">Troubleshooting</span></span>

<span data-ttu-id="7b283-583">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="7b283-583">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="7b283-584">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="7b283-584">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="7b283-585">`Accept-Encoding` 標頭存在，其值為 `gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="7b283-585">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="7b283-586">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="7b283-586">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="7b283-587">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="7b283-587">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="7b283-588">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="7b283-588">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="7b283-589">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="7b283-589">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="7b283-590">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="7b283-590">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7b283-591">其他資源</span><span class="sxs-lookup"><span data-stu-id="7b283-591">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="7b283-592">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-592">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="7b283-593">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="7b283-593">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="7b283-594">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="7b283-594">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="7b283-595">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="7b283-595">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end
