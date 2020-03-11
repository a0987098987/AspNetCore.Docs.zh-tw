---
title: ASP.NET Core 中的回應壓縮
author: rick-anderson
description: 了解回應壓縮及如何使用 ASP.NET Core 應用程式中的回應壓縮中介軟體。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/07/2020
uid: performance/response-compression
ms.openlocfilehash: aae0b8d74fc424cc81c046e9042279856865bf6a
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78665974"
---
# <a name="response-compression-in-aspnet-core"></a><span data-ttu-id="78c03-103">ASP.NET Core 中的回應壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-103">Response compression in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="78c03-104">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="78c03-104">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="78c03-105">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="78c03-105">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="78c03-106">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-106">One way to reduce payload sizes is to compress an app's responses.</span></span>

<span data-ttu-id="78c03-107">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78c03-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="78c03-108">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="78c03-108">When to use Response Compression Middleware</span></span>

<span data-ttu-id="78c03-109">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="78c03-109">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="78c03-110">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="78c03-110">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="78c03-111">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="78c03-111">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="78c03-112">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="78c03-112">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="78c03-113">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="78c03-113">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="78c03-114">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="78c03-114">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="78c03-115">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="78c03-115">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="78c03-116">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-116">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="78c03-117">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="78c03-117">Hosting directly on:</span></span>
  * <span data-ttu-id="78c03-118">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="78c03-118">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="78c03-119">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="78c03-119">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="78c03-120">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-120">Response compression</span></span>

<span data-ttu-id="78c03-121">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="78c03-121">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="78c03-122">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="78c03-122">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="78c03-123">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="78c03-123">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="78c03-124">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="78c03-124">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="78c03-125">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="78c03-125">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="78c03-126">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="78c03-126">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="78c03-127">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="78c03-127">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="78c03-128">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-128">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="78c03-129">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-129">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="78c03-130">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="78c03-130">`Accept-Encoding` header values</span></span> | <span data-ttu-id="78c03-131">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="78c03-131">Middleware Supported</span></span> | <span data-ttu-id="78c03-132">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-132">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="78c03-133">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="78c03-133">Yes (default)</span></span>        | [<span data-ttu-id="78c03-134">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="78c03-134">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="78c03-135">否</span><span class="sxs-lookup"><span data-stu-id="78c03-135">No</span></span>                   | [<span data-ttu-id="78c03-136">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="78c03-136">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="78c03-137">否</span><span class="sxs-lookup"><span data-stu-id="78c03-137">No</span></span>                   | [<span data-ttu-id="78c03-138">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="78c03-138">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="78c03-139">是</span><span class="sxs-lookup"><span data-stu-id="78c03-139">Yes</span></span>                  | [<span data-ttu-id="78c03-140">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="78c03-140">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="78c03-141">是</span><span class="sxs-lookup"><span data-stu-id="78c03-141">Yes</span></span>                  | <span data-ttu-id="78c03-142">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-142">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="78c03-143">否</span><span class="sxs-lookup"><span data-stu-id="78c03-143">No</span></span>                   | [<span data-ttu-id="78c03-144">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="78c03-144">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="78c03-145">是</span><span class="sxs-lookup"><span data-stu-id="78c03-145">Yes</span></span>                  | <span data-ttu-id="78c03-146">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-146">Any available content encoding not explicitly requested</span></span> |

<span data-ttu-id="78c03-147">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="78c03-147">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="78c03-148">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="78c03-148">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="78c03-149">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="78c03-149">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="78c03-150">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="78c03-150">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="78c03-151">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="78c03-151">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="78c03-152">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="78c03-152">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="78c03-153">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="78c03-153">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="78c03-154">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="78c03-154">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="78c03-155">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-155">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="78c03-156">頁首</span><span class="sxs-lookup"><span data-stu-id="78c03-156">Header</span></span>             | <span data-ttu-id="78c03-157">角色</span><span class="sxs-lookup"><span data-stu-id="78c03-157">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="78c03-158">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="78c03-158">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="78c03-159">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-159">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="78c03-160">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="78c03-160">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="78c03-161">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="78c03-161">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="78c03-162">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-162">Specifies the MIME type of the content.</span></span> <span data-ttu-id="78c03-163">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="78c03-163">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="78c03-164">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-164">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="78c03-165">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-165">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="78c03-166">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-166">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="78c03-167">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-167">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="78c03-168">使用[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="78c03-168">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="78c03-169">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="78c03-169">The sample illustrates:</span></span>

* <span data-ttu-id="78c03-170">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-170">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="78c03-171">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-171">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="78c03-172">Package</span><span class="sxs-lookup"><span data-stu-id="78c03-172">Package</span></span>

<span data-ttu-id="78c03-173">回應壓縮中介軟體是由[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)套件所提供，它會隱含地包含在 ASP.NET Core 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="78c03-173">Response Compression Middleware is provided by the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package, which is implicitly included in ASP.NET Core apps.</span></span>

## <a name="configuration"></a><span data-ttu-id="78c03-174">組態</span><span class="sxs-lookup"><span data-stu-id="78c03-174">Configuration</span></span>

<span data-ttu-id="78c03-175">下列程式碼示範如何針對預設 MIME 類型和壓縮提供者（[Brotli](#brotli-compression-provider)和[Gzip](#gzip-compression-provider)）啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="78c03-175">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

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

<span data-ttu-id="78c03-176">注意：</span><span class="sxs-lookup"><span data-stu-id="78c03-176">Notes:</span></span>

* <span data-ttu-id="78c03-177">`app.UseResponseCompression` 必須在壓縮回應的任何中介軟體之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="78c03-177">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="78c03-178">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#middleware-order>。</span><span class="sxs-lookup"><span data-stu-id="78c03-178">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="78c03-179">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="78c03-179">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="78c03-180">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-180">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="78c03-181">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-181">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="78c03-184">使用 `Accept-Encoding: br` 標頭（Brotli 壓縮）將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-184">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="78c03-185">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-185">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 br 之要求的結果。](response-compression/_static/request-compressed-br.png)

## <a name="providers"></a><span data-ttu-id="78c03-189">提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-189">Providers</span></span>

### <a name="brotli-compression-provider"></a><span data-ttu-id="78c03-190">Brotli 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-190">Brotli Compression Provider</span></span>

<span data-ttu-id="78c03-191">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider>，以[Brotli 壓縮資料格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-191">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="78c03-192">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="78c03-192">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="78c03-193">Brotli 壓縮提供者預設會加入至壓縮提供者陣列，以及[Gzip 壓縮提供者](#gzip-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="78c03-193">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="78c03-194">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-194">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="78c03-195">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="78c03-195">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="78c03-196">明確新增任何壓縮提供者時，必須新增 Brotoli 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="78c03-196">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="78c03-197">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="78c03-197">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="78c03-198">Brotli 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-198">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="78c03-199">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-199">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="78c03-200">Compression Level</span><span class="sxs-lookup"><span data-stu-id="78c03-200">Compression Level</span></span> | <span data-ttu-id="78c03-201">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-201">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="78c03-202">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="78c03-202">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-203">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="78c03-203">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="78c03-204">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="78c03-204">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-205">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-205">No compression should be performed.</span></span> |
| [<span data-ttu-id="78c03-206">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="78c03-206">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-207">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="78c03-207">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="78c03-208">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-208">Gzip Compression Provider</span></span>

<span data-ttu-id="78c03-209">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-209">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="78c03-210">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="78c03-210">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="78c03-211">Gzip 壓縮提供者預設會加入壓縮提供者的陣列，以及[Brotli 壓縮提供者](#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="78c03-211">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="78c03-212">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-212">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="78c03-213">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="78c03-213">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="78c03-214">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="78c03-214">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="78c03-215">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="78c03-215">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="78c03-216">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-216">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="78c03-217">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-217">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="78c03-218">Compression Level</span><span class="sxs-lookup"><span data-stu-id="78c03-218">Compression Level</span></span> | <span data-ttu-id="78c03-219">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-219">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="78c03-220">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="78c03-220">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-221">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="78c03-221">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="78c03-222">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="78c03-222">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-223">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-223">No compression should be performed.</span></span> |
| [<span data-ttu-id="78c03-224">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="78c03-224">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-225">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="78c03-225">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="78c03-226">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-226">Custom providers</span></span>

<span data-ttu-id="78c03-227">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-227">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="78c03-228"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-228">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="78c03-229">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="78c03-229">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="78c03-230">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="78c03-230">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="78c03-231">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-231">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="78c03-232">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="78c03-232">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/3.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]


<span data-ttu-id="78c03-233">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-233">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="78c03-234">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-234">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="78c03-235">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="78c03-235">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="78c03-236">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="78c03-236">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="78c03-237">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="78c03-237">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="78c03-240">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="78c03-240">MIME types</span></span>

<span data-ttu-id="78c03-241">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="78c03-241">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="78c03-242">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-242">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="78c03-243">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="78c03-243">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="78c03-244">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="78c03-244">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

[!code-csharp[](response-compression/samples/3.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="78c03-245">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-245">Compression with secure protocol</span></span>

<span data-ttu-id="78c03-246">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="78c03-246">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="78c03-247">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="78c03-247">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="78c03-248">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="78c03-248">Adding the Vary header</span></span>

<span data-ttu-id="78c03-249">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="78c03-249">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="78c03-250">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="78c03-250">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="78c03-251">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-251">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="78c03-252">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="78c03-252">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="78c03-253">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-253">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="78c03-254">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-254">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="78c03-255">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="78c03-255">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="78c03-256">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="78c03-256">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="78c03-257">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-257">Working with IIS dynamic compression</span></span>

<span data-ttu-id="78c03-258">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="78c03-258">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="78c03-259">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="78c03-259">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="78c03-260">疑難排解</span><span class="sxs-lookup"><span data-stu-id="78c03-260">Troubleshooting</span></span>

<span data-ttu-id="78c03-261">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="78c03-261">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="78c03-262">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="78c03-262">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="78c03-263">`Accept-Encoding` 標頭存在，其值為 `br`、`gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-263">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="78c03-264">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="78c03-264">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="78c03-265">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-265">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="78c03-266">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-266">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="78c03-267">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="78c03-267">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="78c03-268">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="78c03-268">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78c03-269">其他資源</span><span class="sxs-lookup"><span data-stu-id="78c03-269">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="78c03-270">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-270">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="78c03-271">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="78c03-271">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="78c03-272">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-272">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="78c03-273">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="78c03-273">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="78c03-274">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="78c03-274">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="78c03-275">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="78c03-275">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="78c03-276">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-276">One way to reduce payload sizes is to compress an app's responses.</span></span>

<span data-ttu-id="78c03-277">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78c03-277">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="78c03-278">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="78c03-278">When to use Response Compression Middleware</span></span>

<span data-ttu-id="78c03-279">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="78c03-279">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="78c03-280">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="78c03-280">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="78c03-281">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="78c03-281">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="78c03-282">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="78c03-282">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="78c03-283">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="78c03-283">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="78c03-284">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="78c03-284">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="78c03-285">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="78c03-285">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="78c03-286">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-286">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="78c03-287">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="78c03-287">Hosting directly on:</span></span>
  * <span data-ttu-id="78c03-288">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="78c03-288">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="78c03-289">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="78c03-289">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="78c03-290">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-290">Response compression</span></span>

<span data-ttu-id="78c03-291">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="78c03-291">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="78c03-292">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="78c03-292">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="78c03-293">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="78c03-293">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="78c03-294">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="78c03-294">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="78c03-295">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="78c03-295">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="78c03-296">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="78c03-296">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="78c03-297">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="78c03-297">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="78c03-298">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-298">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="78c03-299">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-299">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="78c03-300">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="78c03-300">`Accept-Encoding` header values</span></span> | <span data-ttu-id="78c03-301">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="78c03-301">Middleware Supported</span></span> | <span data-ttu-id="78c03-302">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-302">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="78c03-303">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="78c03-303">Yes (default)</span></span>        | [<span data-ttu-id="78c03-304">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="78c03-304">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="78c03-305">否</span><span class="sxs-lookup"><span data-stu-id="78c03-305">No</span></span>                   | [<span data-ttu-id="78c03-306">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="78c03-306">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="78c03-307">否</span><span class="sxs-lookup"><span data-stu-id="78c03-307">No</span></span>                   | [<span data-ttu-id="78c03-308">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="78c03-308">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="78c03-309">是</span><span class="sxs-lookup"><span data-stu-id="78c03-309">Yes</span></span>                  | [<span data-ttu-id="78c03-310">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="78c03-310">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="78c03-311">是</span><span class="sxs-lookup"><span data-stu-id="78c03-311">Yes</span></span>                  | <span data-ttu-id="78c03-312">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-312">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="78c03-313">否</span><span class="sxs-lookup"><span data-stu-id="78c03-313">No</span></span>                   | [<span data-ttu-id="78c03-314">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="78c03-314">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="78c03-315">是</span><span class="sxs-lookup"><span data-stu-id="78c03-315">Yes</span></span>                  | <span data-ttu-id="78c03-316">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-316">Any available content encoding not explicitly requested</span></span> |

<span data-ttu-id="78c03-317">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="78c03-317">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="78c03-318">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="78c03-318">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="78c03-319">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="78c03-319">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="78c03-320">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="78c03-320">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="78c03-321">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="78c03-321">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="78c03-322">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="78c03-322">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="78c03-323">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="78c03-323">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="78c03-324">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="78c03-324">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="78c03-325">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-325">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="78c03-326">頁首</span><span class="sxs-lookup"><span data-stu-id="78c03-326">Header</span></span>             | <span data-ttu-id="78c03-327">角色</span><span class="sxs-lookup"><span data-stu-id="78c03-327">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="78c03-328">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="78c03-328">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="78c03-329">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-329">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="78c03-330">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="78c03-330">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="78c03-331">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="78c03-331">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="78c03-332">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-332">Specifies the MIME type of the content.</span></span> <span data-ttu-id="78c03-333">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="78c03-333">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="78c03-334">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-334">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="78c03-335">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-335">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="78c03-336">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-336">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="78c03-337">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-337">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="78c03-338">使用[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="78c03-338">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="78c03-339">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="78c03-339">The sample illustrates:</span></span>

* <span data-ttu-id="78c03-340">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-340">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="78c03-341">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-341">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="78c03-342">Package</span><span class="sxs-lookup"><span data-stu-id="78c03-342">Package</span></span>

<span data-ttu-id="78c03-343">若要在專案中包含中介軟體，請新增[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)的參考，其中包含[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="78c03-343">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

## <a name="configuration"></a><span data-ttu-id="78c03-344">組態</span><span class="sxs-lookup"><span data-stu-id="78c03-344">Configuration</span></span>

<span data-ttu-id="78c03-345">下列程式碼示範如何針對預設 MIME 類型和壓縮提供者（[Brotli](#brotli-compression-provider)和[Gzip](#gzip-compression-provider)）啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="78c03-345">The following code shows how to enable the Response Compression Middleware for default MIME types and compression providers ([Brotli](#brotli-compression-provider) and [Gzip](#gzip-compression-provider)):</span></span>

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

<span data-ttu-id="78c03-346">注意：</span><span class="sxs-lookup"><span data-stu-id="78c03-346">Notes:</span></span>

* <span data-ttu-id="78c03-347">`app.UseResponseCompression` 必須在壓縮回應的任何中介軟體之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="78c03-347">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="78c03-348">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#middleware-order>。</span><span class="sxs-lookup"><span data-stu-id="78c03-348">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="78c03-349">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="78c03-349">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="78c03-350">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-350">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="78c03-351">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-351">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="78c03-354">使用 `Accept-Encoding: br` 標頭（Brotli 壓縮）將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-354">Submit a request to the sample app with the `Accept-Encoding: br` header (Brotli compression) and observe that the response is compressed.</span></span> <span data-ttu-id="78c03-355">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-355">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 br 之要求的結果。](response-compression/_static/request-compressed-br.png)

## <a name="providers"></a><span data-ttu-id="78c03-359">提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-359">Providers</span></span>

### <a name="brotli-compression-provider"></a><span data-ttu-id="78c03-360">Brotli 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-360">Brotli Compression Provider</span></span>

<span data-ttu-id="78c03-361">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider>，以[Brotli 壓縮資料格式](https://tools.ietf.org/html/rfc7932)來壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-361">Use the <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProvider> to compress responses with the [Brotli compressed data format](https://tools.ietf.org/html/rfc7932).</span></span>

<span data-ttu-id="78c03-362">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="78c03-362">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="78c03-363">Brotli 壓縮提供者預設會加入至壓縮提供者陣列，以及[Gzip 壓縮提供者](#gzip-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="78c03-363">The Brotli Compression Provider is added by default to the array of compression providers along with the [Gzip compression provider](#gzip-compression-provider).</span></span>
* <span data-ttu-id="78c03-364">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-364">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="78c03-365">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="78c03-365">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="78c03-366">明確新增任何壓縮提供者時，必須新增 Brotoli 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="78c03-366">The Brotoli Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=5)]

<span data-ttu-id="78c03-367">使用 <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="78c03-367">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.BrotliCompressionProviderOptions>.</span></span> <span data-ttu-id="78c03-368">Brotli 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-368">The Brotli Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="78c03-369">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-369">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="78c03-370">Compression Level</span><span class="sxs-lookup"><span data-stu-id="78c03-370">Compression Level</span></span> | <span data-ttu-id="78c03-371">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-371">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="78c03-372">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="78c03-372">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-373">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="78c03-373">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="78c03-374">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="78c03-374">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-375">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-375">No compression should be performed.</span></span> |
| [<span data-ttu-id="78c03-376">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="78c03-376">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-377">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="78c03-377">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="gzip-compression-provider"></a><span data-ttu-id="78c03-378">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-378">Gzip Compression Provider</span></span>

<span data-ttu-id="78c03-379">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-379">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="78c03-380">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="78c03-380">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="78c03-381">Gzip 壓縮提供者預設會加入壓縮提供者的陣列，以及[Brotli 壓縮提供者](#brotli-compression-provider)。</span><span class="sxs-lookup"><span data-stu-id="78c03-381">The Gzip Compression Provider is added by default to the array of compression providers along with the [Brotli Compression Provider](#brotli-compression-provider).</span></span>
* <span data-ttu-id="78c03-382">當用戶端支援 Brotli 壓縮資料格式時，壓縮會預設為 Brotli 壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-382">Compression defaults to Brotli compression when the Brotli compressed data format is supported by the client.</span></span> <span data-ttu-id="78c03-383">如果用戶端不支援 Brotli，當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="78c03-383">If Brotli isn't supported by the client, compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="78c03-384">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="78c03-384">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="78c03-385">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="78c03-385">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="78c03-386">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-386">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="78c03-387">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-387">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="78c03-388">Compression Level</span><span class="sxs-lookup"><span data-stu-id="78c03-388">Compression Level</span></span> | <span data-ttu-id="78c03-389">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-389">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="78c03-390">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="78c03-390">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-391">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="78c03-391">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="78c03-392">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="78c03-392">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-393">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-393">No compression should be performed.</span></span> |
| [<span data-ttu-id="78c03-394">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="78c03-394">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-395">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="78c03-395">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="78c03-396">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-396">Custom providers</span></span>

<span data-ttu-id="78c03-397">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-397">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="78c03-398"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-398">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="78c03-399">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="78c03-399">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="78c03-400">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="78c03-400">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="78c03-401">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-401">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="78c03-402">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="78c03-402">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

<span data-ttu-id="78c03-403">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-403">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="78c03-404">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-404">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="78c03-405">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="78c03-405">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="78c03-406">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="78c03-406">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="78c03-407">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="78c03-407">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="78c03-410">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="78c03-410">MIME types</span></span>

<span data-ttu-id="78c03-411">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="78c03-411">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="78c03-412">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-412">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="78c03-413">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="78c03-413">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="78c03-414">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="78c03-414">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="78c03-415">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-415">Compression with secure protocol</span></span>

<span data-ttu-id="78c03-416">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="78c03-416">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="78c03-417">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="78c03-417">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="78c03-418">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="78c03-418">Adding the Vary header</span></span>

<span data-ttu-id="78c03-419">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="78c03-419">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="78c03-420">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="78c03-420">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="78c03-421">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-421">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="78c03-422">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="78c03-422">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="78c03-423">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-423">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="78c03-424">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-424">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="78c03-425">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="78c03-425">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="78c03-426">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="78c03-426">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="78c03-427">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-427">Working with IIS dynamic compression</span></span>

<span data-ttu-id="78c03-428">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="78c03-428">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="78c03-429">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="78c03-429">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="78c03-430">疑難排解</span><span class="sxs-lookup"><span data-stu-id="78c03-430">Troubleshooting</span></span>

<span data-ttu-id="78c03-431">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="78c03-431">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="78c03-432">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="78c03-432">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="78c03-433">`Accept-Encoding` 標頭存在，其值為 `br`、`gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-433">The `Accept-Encoding` header is present with a value of `br`, `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="78c03-434">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="78c03-434">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="78c03-435">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-435">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="78c03-436">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-436">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="78c03-437">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="78c03-437">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="78c03-438">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="78c03-438">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78c03-439">其他資源</span><span class="sxs-lookup"><span data-stu-id="78c03-439">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="78c03-440">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-440">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="78c03-441">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="78c03-441">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="78c03-442">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-442">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="78c03-443">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="78c03-443">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="78c03-444">網路頻寬是有限的資源。</span><span class="sxs-lookup"><span data-stu-id="78c03-444">Network bandwidth is a limited resource.</span></span> <span data-ttu-id="78c03-445">減少回應的大小通常會增加應用程式的回應性，通常會大幅提升。</span><span class="sxs-lookup"><span data-stu-id="78c03-445">Reducing the size of the response usually increases the responsiveness of an app, often dramatically.</span></span> <span data-ttu-id="78c03-446">減少承載大小的其中一種方法是壓縮應用程式的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-446">One way to reduce payload sizes is to compress an app's responses.</span></span>

<span data-ttu-id="78c03-447">[檢視或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="78c03-447">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-response-compression-middleware"></a><span data-ttu-id="78c03-448">使用回應壓縮中介軟體的時機</span><span class="sxs-lookup"><span data-stu-id="78c03-448">When to use Response Compression Middleware</span></span>

<span data-ttu-id="78c03-449">在 IIS、Apache 或 Nginx 中使用以伺服器為基礎的回應壓縮技術。</span><span class="sxs-lookup"><span data-stu-id="78c03-449">Use server-based response compression technologies in IIS, Apache, or Nginx.</span></span> <span data-ttu-id="78c03-450">中介軟體的效能可能與伺服器模組不相符。</span><span class="sxs-lookup"><span data-stu-id="78c03-450">The performance of the middleware probably won't match that of the server modules.</span></span> <span data-ttu-id="78c03-451">[Http.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前未提供內建的壓縮支援。</span><span class="sxs-lookup"><span data-stu-id="78c03-451">[HTTP.sys server](xref:fundamentals/servers/httpsys) server and [Kestrel](xref:fundamentals/servers/kestrel) server don't currently offer built-in compression support.</span></span>

<span data-ttu-id="78c03-452">當您是時，請使用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="78c03-452">Use Response Compression Middleware when you're:</span></span>

* <span data-ttu-id="78c03-453">無法使用下列以伺服器為基礎的壓縮技術：</span><span class="sxs-lookup"><span data-stu-id="78c03-453">Unable to use the following server-based compression technologies:</span></span>
  * [<span data-ttu-id="78c03-454">IIS 動態壓縮模組</span><span class="sxs-lookup"><span data-stu-id="78c03-454">IIS Dynamic Compression module</span></span>](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [<span data-ttu-id="78c03-455">Apache mod_deflate 模組</span><span class="sxs-lookup"><span data-stu-id="78c03-455">Apache mod_deflate module</span></span>](https://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [<span data-ttu-id="78c03-456">Nginx 壓縮和解壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-456">Nginx Compression and Decompression</span></span>](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* <span data-ttu-id="78c03-457">直接裝載于：</span><span class="sxs-lookup"><span data-stu-id="78c03-457">Hosting directly on:</span></span>
  * <span data-ttu-id="78c03-458">Http.sys[伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）</span><span class="sxs-lookup"><span data-stu-id="78c03-458">[HTTP.sys server](xref:fundamentals/servers/httpsys) (formerly called WebListener)</span></span>
  * [<span data-ttu-id="78c03-459">Kestrel 伺服器</span><span class="sxs-lookup"><span data-stu-id="78c03-459">Kestrel server</span></span>](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a><span data-ttu-id="78c03-460">回應壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-460">Response compression</span></span>

<span data-ttu-id="78c03-461">通常，任何未原生壓縮的回應都可以因回應壓縮而受益。</span><span class="sxs-lookup"><span data-stu-id="78c03-461">Usually, any response not natively compressed can benefit from response compression.</span></span> <span data-ttu-id="78c03-462">原本不壓縮的回應通常包括： CSS、JavaScript、HTML、XML 和 JSON。</span><span class="sxs-lookup"><span data-stu-id="78c03-462">Responses not natively compressed typically include: CSS, JavaScript, HTML, XML, and JSON.</span></span> <span data-ttu-id="78c03-463">您不應該壓縮原生壓縮的資產，例如 PNG 檔案。</span><span class="sxs-lookup"><span data-stu-id="78c03-463">You shouldn't compress natively compressed assets, such as PNG files.</span></span> <span data-ttu-id="78c03-464">如果您嘗試進一步壓縮原生壓縮的回應，則在處理壓縮所花費的時間內，任何小型額外的大小和傳輸時間縮減可能會失色。</span><span class="sxs-lookup"><span data-stu-id="78c03-464">If you attempt to further compress a natively compressed response, any small additional reduction in size and transmission time will likely be overshadowed by the time it took to process the compression.</span></span> <span data-ttu-id="78c03-465">不要壓縮小於150-1000 個位元組的檔案（視檔案的內容和壓縮效率而定）。</span><span class="sxs-lookup"><span data-stu-id="78c03-465">Don't compress files smaller than about 150-1000 bytes (depending on the file's content and the efficiency of compression).</span></span> <span data-ttu-id="78c03-466">壓縮小型檔案的額外負荷可能會產生比未壓縮檔案更大的壓縮檔案。</span><span class="sxs-lookup"><span data-stu-id="78c03-466">The overhead of compressing small files may produce a compressed file larger than the uncompressed file.</span></span>

<span data-ttu-id="78c03-467">當用戶端可以處理壓縮內容時，用戶端必須透過要求傳送 `Accept-Encoding` 標頭，以通知伺服器其功能。</span><span class="sxs-lookup"><span data-stu-id="78c03-467">When a client can process compressed content, the client must inform the server of its capabilities by sending the `Accept-Encoding` header with the request.</span></span> <span data-ttu-id="78c03-468">當伺服器傳送壓縮的內容時，它必須包含 `Content-Encoding` 標頭中的資訊，以了解壓縮回應的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-468">When a server sends compressed content, it must include information in the `Content-Encoding` header on how the compressed response is encoded.</span></span> <span data-ttu-id="78c03-469">下表顯示中介軟體支援的內容編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-469">Content encoding designations supported by the middleware are shown in the following table.</span></span>

| <span data-ttu-id="78c03-470">`Accept-Encoding` 標頭值</span><span class="sxs-lookup"><span data-stu-id="78c03-470">`Accept-Encoding` header values</span></span> | <span data-ttu-id="78c03-471">支援的中介軟體</span><span class="sxs-lookup"><span data-stu-id="78c03-471">Middleware Supported</span></span> | <span data-ttu-id="78c03-472">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-472">Description</span></span> |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | <span data-ttu-id="78c03-473">否</span><span class="sxs-lookup"><span data-stu-id="78c03-473">No</span></span>                   | [<span data-ttu-id="78c03-474">Brotli 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="78c03-474">Brotli compressed data format</span></span>](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | <span data-ttu-id="78c03-475">否</span><span class="sxs-lookup"><span data-stu-id="78c03-475">No</span></span>                   | [<span data-ttu-id="78c03-476">DEFLATE 壓縮資料格式</span><span class="sxs-lookup"><span data-stu-id="78c03-476">DEFLATE compressed data format</span></span>](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | <span data-ttu-id="78c03-477">否</span><span class="sxs-lookup"><span data-stu-id="78c03-477">No</span></span>                   | [<span data-ttu-id="78c03-478">W3C 有效率的 XML 交換</span><span class="sxs-lookup"><span data-stu-id="78c03-478">W3C Efficient XML Interchange</span></span>](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | <span data-ttu-id="78c03-479">是 (預設)</span><span class="sxs-lookup"><span data-stu-id="78c03-479">Yes (default)</span></span>        | [<span data-ttu-id="78c03-480">GZIP 檔案格式</span><span class="sxs-lookup"><span data-stu-id="78c03-480">Gzip file format</span></span>](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | <span data-ttu-id="78c03-481">是</span><span class="sxs-lookup"><span data-stu-id="78c03-481">Yes</span></span>                  | <span data-ttu-id="78c03-482">「無編碼」識別碼：回應不得編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-482">"No encoding" identifier: The response must not be encoded.</span></span> |
| `pack200-gzip`                  | <span data-ttu-id="78c03-483">否</span><span class="sxs-lookup"><span data-stu-id="78c03-483">No</span></span>                   | [<span data-ttu-id="78c03-484">JAVA 封存的網路傳輸格式</span><span class="sxs-lookup"><span data-stu-id="78c03-484">Network Transfer Format for Java Archives</span></span>](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | <span data-ttu-id="78c03-485">是</span><span class="sxs-lookup"><span data-stu-id="78c03-485">Yes</span></span>                  | <span data-ttu-id="78c03-486">未明確要求任何可用的內容編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-486">Any available content encoding not explicitly requested</span></span> |

<span data-ttu-id="78c03-487">如需詳細資訊，請參閱[IANA 官方內容編碼清單](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。</span><span class="sxs-lookup"><span data-stu-id="78c03-487">For more information, see the [IANA Official Content Coding List](https://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry).</span></span>

<span data-ttu-id="78c03-488">中介軟體可讓您為自訂 `Accept-Encoding` 標頭值新增額外的壓縮提供者。</span><span class="sxs-lookup"><span data-stu-id="78c03-488">The middleware allows you to add additional compression providers for custom `Accept-Encoding` header values.</span></span> <span data-ttu-id="78c03-489">如需詳細資訊，請參閱下面的[自訂提供者](#custom-providers)。</span><span class="sxs-lookup"><span data-stu-id="78c03-489">For more information, see [Custom Providers](#custom-providers) below.</span></span>

<span data-ttu-id="78c03-490">中介軟體能夠在用戶端傳送時，回應品質值（qvalue、`q`）加權，以設定壓縮配置的優先順序。</span><span class="sxs-lookup"><span data-stu-id="78c03-490">The middleware is capable of reacting to quality value (qvalue, `q`) weighting when sent by the client to prioritize compression schemes.</span></span> <span data-ttu-id="78c03-491">如需詳細資訊，請參閱[RFC 7231：接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。</span><span class="sxs-lookup"><span data-stu-id="78c03-491">For more information, see [RFC 7231: Accept-Encoding](https://tools.ietf.org/html/rfc7231#section-5.3.4).</span></span>

<span data-ttu-id="78c03-492">壓縮演算法會受到壓縮速度與壓縮效率之間的取捨。</span><span class="sxs-lookup"><span data-stu-id="78c03-492">Compression algorithms are subject to a tradeoff between compression speed and the effectiveness of the compression.</span></span> <span data-ttu-id="78c03-493">此內容中的*有效性*是指壓縮之後的輸出大小。</span><span class="sxs-lookup"><span data-stu-id="78c03-493">*Effectiveness* in this context refers to the size of the output after compression.</span></span> <span data-ttu-id="78c03-494">最大的大小是透過*最佳*壓縮來達成。</span><span class="sxs-lookup"><span data-stu-id="78c03-494">The smallest size is achieved by the most *optimal* compression.</span></span>

<span data-ttu-id="78c03-495">下表說明有關要求、傳送、快取和接收壓縮內容的標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-495">The headers involved in requesting, sending, caching, and receiving compressed content are described in the table below.</span></span>

| <span data-ttu-id="78c03-496">頁首</span><span class="sxs-lookup"><span data-stu-id="78c03-496">Header</span></span>             | <span data-ttu-id="78c03-497">角色</span><span class="sxs-lookup"><span data-stu-id="78c03-497">Role</span></span> |
| ------------------ | ---- |
| `Accept-Encoding`  | <span data-ttu-id="78c03-498">從用戶端傳送到伺服器，以指示用戶端可接受的內容編碼配置。</span><span class="sxs-lookup"><span data-stu-id="78c03-498">Sent from the client to the server to indicate the content encoding schemes acceptable to the client.</span></span> |
| `Content-Encoding` | <span data-ttu-id="78c03-499">從伺服器傳送到用戶端，以指出內容在承載中的編碼方式。</span><span class="sxs-lookup"><span data-stu-id="78c03-499">Sent from the server to the client to indicate the encoding of the content in the payload.</span></span> |
| `Content-Length`   | <span data-ttu-id="78c03-500">進行壓縮時，會移除 `Content-Length` 標頭，因為當回應壓縮時，本文內容會變更。</span><span class="sxs-lookup"><span data-stu-id="78c03-500">When compression occurs, the `Content-Length` header is removed, since the body content changes when the response is compressed.</span></span> |
| `Content-MD5`      | <span data-ttu-id="78c03-501">進行壓縮時，會移除 `Content-MD5` 標頭，因為本文內容已變更，而且雜湊已不再有效。</span><span class="sxs-lookup"><span data-stu-id="78c03-501">When compression occurs, the `Content-MD5` header is removed, since the body content has changed and the hash is no longer valid.</span></span> |
| `Content-Type`     | <span data-ttu-id="78c03-502">指定內容的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-502">Specifies the MIME type of the content.</span></span> <span data-ttu-id="78c03-503">每個回應都應該指定其 `Content-Type`。</span><span class="sxs-lookup"><span data-stu-id="78c03-503">Every response should specify its `Content-Type`.</span></span> <span data-ttu-id="78c03-504">中介軟體會檢查此值，以判斷是否應該壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-504">The middleware checks this value to determine if the response should be compressed.</span></span> <span data-ttu-id="78c03-505">中介軟體會指定一組可編碼的[預設 MIME 類型](#mime-types)，但您可以取代或加入 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-505">The middleware specifies a set of [default MIME types](#mime-types) that it can encode, but you can replace or add MIME types.</span></span> |
| `Vary`             | <span data-ttu-id="78c03-506">當伺服器以 [`Accept-Encoding`] 的值傳送至 [用戶端] 和 [proxy] 時，`Vary` 標頭會向用戶端或 proxy 指出它應該根據要求的 `Accept-Encoding` 標頭值來快取（改變）回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-506">When sent by the server with a value of `Accept-Encoding` to clients and proxies, the `Vary` header indicates to the client or proxy that it should cache (vary) responses based on the value of the `Accept-Encoding` header of the request.</span></span> <span data-ttu-id="78c03-507">傳回具有 `Vary: Accept-Encoding` 標頭之內容的結果，是會分別快取壓縮和未壓縮的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-507">The result of returning content with the `Vary: Accept-Encoding` header is that both compressed and uncompressed responses are cached separately.</span></span> |

<span data-ttu-id="78c03-508">使用[範例應用程式](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples)探索回應壓縮中介軟體的功能。</span><span class="sxs-lookup"><span data-stu-id="78c03-508">Explore the features of the Response Compression Middleware with the [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples).</span></span> <span data-ttu-id="78c03-509">此範例說明：</span><span class="sxs-lookup"><span data-stu-id="78c03-509">The sample illustrates:</span></span>

* <span data-ttu-id="78c03-510">使用 Gzip 和自訂壓縮提供者來壓縮應用程式回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-510">The compression of app responses using Gzip and custom compression providers.</span></span>
* <span data-ttu-id="78c03-511">如何將 MIME 類型新增至 MIME 類型的預設清單以進行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-511">How to add a MIME type to the default list of MIME types for compression.</span></span>

## <a name="package"></a><span data-ttu-id="78c03-512">Package</span><span class="sxs-lookup"><span data-stu-id="78c03-512">Package</span></span>

<span data-ttu-id="78c03-513">若要在專案中包含中介軟體，請新增[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)的參考，其中包含[AspNetCore. ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。</span><span class="sxs-lookup"><span data-stu-id="78c03-513">To include the middleware in a project, add a reference to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), which includes the [Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/) package.</span></span>

## <a name="configuration"></a><span data-ttu-id="78c03-514">組態</span><span class="sxs-lookup"><span data-stu-id="78c03-514">Configuration</span></span>

<span data-ttu-id="78c03-515">下列程式碼示範如何為預設 MIME 類型和[Gzip 壓縮提供者](#gzip-compression-provider)啟用回應壓縮中介軟體：</span><span class="sxs-lookup"><span data-stu-id="78c03-515">The following code shows how to enable the Response Compression Middleware for default MIME types and the [Gzip Compression Provider](#gzip-compression-provider):</span></span>

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

<span data-ttu-id="78c03-516">注意：</span><span class="sxs-lookup"><span data-stu-id="78c03-516">Notes:</span></span>

* <span data-ttu-id="78c03-517">`app.UseResponseCompression` 必須在壓縮回應的任何中介軟體之前呼叫。</span><span class="sxs-lookup"><span data-stu-id="78c03-517">`app.UseResponseCompression` must be called before any middleware that compresses responses.</span></span> <span data-ttu-id="78c03-518">如需詳細資訊，請參閱 <xref:fundamentals/middleware/index#middleware-order>。</span><span class="sxs-lookup"><span data-stu-id="78c03-518">For more information, see <xref:fundamentals/middleware/index#middleware-order>.</span></span>
* <span data-ttu-id="78c03-519">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具來設定 `Accept-Encoding` 要求標頭，並研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="78c03-519">Use a tool such as [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/) to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span>

<span data-ttu-id="78c03-520">將要求提交至範例應用程式但不 `Accept-Encoding` 標頭，並觀察回應是否已解壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-520">Submit a request to the sample app without the `Accept-Encoding` header and observe that the response is uncompressed.</span></span> <span data-ttu-id="78c03-521">`Content-Encoding` 和 `Vary` 標頭不存在於回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-521">The `Content-Encoding` and `Vary` headers aren't present on the response.</span></span>

![顯示要求不含接受編碼標頭之結果的 Fiddler 視窗。](response-compression/_static/request-uncompressed.png)

<span data-ttu-id="78c03-524">使用 `Accept-Encoding: gzip` 標頭將要求提交至範例應用程式，並觀察回應是否已壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-524">Submit a request to the sample app with the `Accept-Encoding: gzip` header and observe that the response is compressed.</span></span> <span data-ttu-id="78c03-525">`Content-Encoding` 和 `Vary` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-525">The `Content-Encoding` and `Vary` headers are present on the response.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和 gzip 值之要求的結果。](response-compression/_static/request-compressed.png)

## <a name="providers"></a><span data-ttu-id="78c03-529">提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-529">Providers</span></span>

### <a name="gzip-compression-provider"></a><span data-ttu-id="78c03-530">Gzip 壓縮提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-530">Gzip Compression Provider</span></span>

<span data-ttu-id="78c03-531">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> 以[GZIP 檔案格式](https://tools.ietf.org/html/rfc1952)壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-531">Use the <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider> to compress responses with the [Gzip file format](https://tools.ietf.org/html/rfc1952).</span></span>

<span data-ttu-id="78c03-532">如果未將壓縮提供者明確新增至 <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>：</span><span class="sxs-lookup"><span data-stu-id="78c03-532">If no compression providers are explicitly added to the <xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:</span></span>

* <span data-ttu-id="78c03-533">Gzip 壓縮提供者預設會加入至壓縮提供者陣列。</span><span class="sxs-lookup"><span data-stu-id="78c03-533">The Gzip Compression Provider is added by default to the array of compression providers.</span></span>
* <span data-ttu-id="78c03-534">當用戶端支援 Gzip 壓縮時，壓縮會預設為 Gzip。</span><span class="sxs-lookup"><span data-stu-id="78c03-534">Compression defaults to Gzip when the client supports Gzip compression.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

<span data-ttu-id="78c03-535">當明確新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：</span><span class="sxs-lookup"><span data-stu-id="78c03-535">The Gzip Compression Provider must be added when any compression providers are explicitly added:</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=6)]

<span data-ttu-id="78c03-536">使用 <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>設定壓縮等級。</span><span class="sxs-lookup"><span data-stu-id="78c03-536">Set the compression level with <xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>.</span></span> <span data-ttu-id="78c03-537">Gzip 壓縮提供者預設為最快速的壓縮層級（[CompressionLevel](xref:System.IO.Compression.CompressionLevel)），這可能不會產生最有效率的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-537">The Gzip Compression Provider defaults to the fastest compression level ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel)), which might not produce the most efficient compression.</span></span> <span data-ttu-id="78c03-538">如果需要最有效率的壓縮，請設定中介軟體以獲得最佳壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-538">If the most efficient compression is desired, configure the middleware for optimal compression.</span></span>

| <span data-ttu-id="78c03-539">Compression Level</span><span class="sxs-lookup"><span data-stu-id="78c03-539">Compression Level</span></span> | <span data-ttu-id="78c03-540">描述</span><span class="sxs-lookup"><span data-stu-id="78c03-540">Description</span></span> |
| ----------------- | ----------- |
| [<span data-ttu-id="78c03-541">CompressionLevel。最快</span><span class="sxs-lookup"><span data-stu-id="78c03-541">CompressionLevel.Fastest</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-542">即使產生的輸出未以最佳方式壓縮，壓縮也應該儘快完成。</span><span class="sxs-lookup"><span data-stu-id="78c03-542">Compression should complete as quickly as possible, even if the resulting output isn't optimally compressed.</span></span> |
| [<span data-ttu-id="78c03-543">CompressionLevel. NoCompression</span><span class="sxs-lookup"><span data-stu-id="78c03-543">CompressionLevel.NoCompression</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-544">不應該執行壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-544">No compression should be performed.</span></span> |
| [<span data-ttu-id="78c03-545">CompressionLevel。最佳</span><span class="sxs-lookup"><span data-stu-id="78c03-545">CompressionLevel.Optimal</span></span>](xref:System.IO.Compression.CompressionLevel) | <span data-ttu-id="78c03-546">回應應以最佳方式壓縮，即使壓縮需要較長的時間才能完成也一樣。</span><span class="sxs-lookup"><span data-stu-id="78c03-546">Responses should be optimally compressed, even if the compression takes more time to complete.</span></span> |

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

### <a name="custom-providers"></a><span data-ttu-id="78c03-547">自訂提供者</span><span class="sxs-lookup"><span data-stu-id="78c03-547">Custom providers</span></span>

<span data-ttu-id="78c03-548">使用 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>建立自訂的壓縮。</span><span class="sxs-lookup"><span data-stu-id="78c03-548">Create custom compression implementations with <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>.</span></span> <span data-ttu-id="78c03-549"><xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> 代表此 `ICompressionProvider` 產生的內容編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-549">The <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*> represents the content encoding that this `ICompressionProvider` produces.</span></span> <span data-ttu-id="78c03-550">中介軟體會使用這項資訊，根據要求的 `Accept-Encoding` 標頭中所指定的清單來選擇提供者。</span><span class="sxs-lookup"><span data-stu-id="78c03-550">The middleware uses this information to choose the provider based on the list specified in the `Accept-Encoding` header of the request.</span></span>

<span data-ttu-id="78c03-551">使用範例應用程式時，用戶端會提交含有 `Accept-Encoding: mycustomcompression` 標頭的要求。</span><span class="sxs-lookup"><span data-stu-id="78c03-551">Using the sample app, the client submits a request with the `Accept-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="78c03-552">中介軟體會使用自訂壓縮實作為，並傳回具有 `Content-Encoding: mycustomcompression` 標頭的回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-552">The middleware uses the custom compression implementation and returns the response with a `Content-Encoding: mycustomcompression` header.</span></span> <span data-ttu-id="78c03-553">用戶端必須能夠解壓縮自訂編碼，才能讓自訂壓縮實行正常執行。</span><span class="sxs-lookup"><span data-stu-id="78c03-553">The client must be able to decompress the custom encoding in order for a custom compression implementation to work.</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=7)]

[!code-csharp[](response-compression/samples/2.x/SampleApp/CustomCompressionProvider.cs?name=snippet1)]

<span data-ttu-id="78c03-554">使用 `Accept-Encoding: mycustomcompression` 標頭將要求提交至範例應用程式，並觀察回應標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-554">Submit a request to the sample app with the `Accept-Encoding: mycustomcompression` header and observe the response headers.</span></span> <span data-ttu-id="78c03-555">`Vary` 和 `Content-Encoding` 標頭會出現在回應中。</span><span class="sxs-lookup"><span data-stu-id="78c03-555">The `Vary` and `Content-Encoding` headers are present on the response.</span></span> <span data-ttu-id="78c03-556">範例不會壓縮回應主體（未顯示）。</span><span class="sxs-lookup"><span data-stu-id="78c03-556">The response body (not shown) isn't compressed by the sample.</span></span> <span data-ttu-id="78c03-557">範例的 `CustomCompressionProvider` 類別中沒有壓縮的執行。</span><span class="sxs-lookup"><span data-stu-id="78c03-557">There isn't a compression implementation in the `CustomCompressionProvider` class of the sample.</span></span> <span data-ttu-id="78c03-558">不過，此範例會顯示您將在哪裡執行這種壓縮演算法。</span><span class="sxs-lookup"><span data-stu-id="78c03-558">However, the sample shows where you would implement such a compression algorithm.</span></span>

![Fiddler 視窗，顯示具有接受編碼標頭和值為 mycustomcompression 之要求的結果。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a><span data-ttu-id="78c03-561">MIME 類型</span><span class="sxs-lookup"><span data-stu-id="78c03-561">MIME types</span></span>

<span data-ttu-id="78c03-562">中介軟體會針對壓縮指定一組預設的 MIME 類型：</span><span class="sxs-lookup"><span data-stu-id="78c03-562">The middleware specifies a default set of MIME types for compression:</span></span>

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

<span data-ttu-id="78c03-563">以回應壓縮中介軟體選項取代或附加 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-563">Replace or append MIME types with the Response Compression Middleware options.</span></span> <span data-ttu-id="78c03-564">請注意，不支援萬用字元 MIME 類型，例如 `text/*`。</span><span class="sxs-lookup"><span data-stu-id="78c03-564">Note that wildcard MIME types, such as `text/*` aren't supported.</span></span> <span data-ttu-id="78c03-565">範例應用程式會為 `image/svg+xml` 新增 MIME 類型，並將 ASP.NET Core 的橫幅影像（*橫幅. svg*）壓縮並提供服務。</span><span class="sxs-lookup"><span data-stu-id="78c03-565">The sample app adds a MIME type for `image/svg+xml` and compresses and serves the ASP.NET Core banner image (*banner.svg*).</span></span>

[!code-csharp[](response-compression/samples/2.x/SampleApp/Startup.cs?name=snippet1&highlight=8-10)]

## <a name="compression-with-secure-protocol"></a><span data-ttu-id="78c03-566">使用安全通訊協定進行壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-566">Compression with secure protocol</span></span>

<span data-ttu-id="78c03-567">透過安全連線的壓縮回應可以使用 `EnableForHttps` 選項來控制，預設為停用。</span><span class="sxs-lookup"><span data-stu-id="78c03-567">Compressed responses over secure connections can be controlled with the `EnableForHttps` option, which is disabled by default.</span></span> <span data-ttu-id="78c03-568">以動態產生的頁面使用壓縮，可能會導致安全性問題，例如[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))和[入侵](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。</span><span class="sxs-lookup"><span data-stu-id="78c03-568">Using compression with dynamically generated pages can lead to security problems such as the [CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit)) and [BREACH](https://wikipedia.org/wiki/BREACH_(security_exploit)) attacks.</span></span>

## <a name="adding-the-vary-header"></a><span data-ttu-id="78c03-569">新增 Vary 標頭</span><span class="sxs-lookup"><span data-stu-id="78c03-569">Adding the Vary header</span></span>

<span data-ttu-id="78c03-570">根據 `Accept-Encoding` 標頭壓縮回應時，可能會有多個壓縮版本的回應和未壓縮的版本。</span><span class="sxs-lookup"><span data-stu-id="78c03-570">When compressing responses based on the `Accept-Encoding` header, there are potentially multiple compressed versions of the response and an uncompressed version.</span></span> <span data-ttu-id="78c03-571">為了指示用戶端和 proxy 快取有多個版本存在且應該儲存，`Vary` 標頭會加上 `Accept-Encoding` 值。</span><span class="sxs-lookup"><span data-stu-id="78c03-571">In order to instruct client and proxy caches that multiple versions exist and should be stored, the `Vary` header is added with an `Accept-Encoding` value.</span></span> <span data-ttu-id="78c03-572">在 ASP.NET Core 2.0 或更新版本中，中介軟體會在回應壓縮時自動新增 `Vary` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-572">In ASP.NET Core 2.0 or later, the middleware adds the `Vary` header automatically when the response is compressed.</span></span>

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a><span data-ttu-id="78c03-573">Nginx 反向 proxy 後方發生中介軟體問題</span><span class="sxs-lookup"><span data-stu-id="78c03-573">Middleware issue when behind an Nginx reverse proxy</span></span>

<span data-ttu-id="78c03-574">當要求由 Nginx proxy 時，就會移除 `Accept-Encoding` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-574">When a request is proxied by Nginx, the `Accept-Encoding` header is removed.</span></span> <span data-ttu-id="78c03-575">移除 `Accept-Encoding` 標頭可防止中介軟體壓縮回應。</span><span class="sxs-lookup"><span data-stu-id="78c03-575">Removal of the `Accept-Encoding` header prevents the middleware from compressing the response.</span></span> <span data-ttu-id="78c03-576">如需詳細資訊，請參閱[NGINX：壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。</span><span class="sxs-lookup"><span data-stu-id="78c03-576">For more information, see [NGINX: Compression and Decompression](https://www.nginx.com/resources/admin-guide/compression-and-decompression/).</span></span> <span data-ttu-id="78c03-577">此問題是藉由[瞭解 Nginx （aspnet/BasicMiddleware #123）的傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)來追蹤。</span><span class="sxs-lookup"><span data-stu-id="78c03-577">This issue is tracked by [Figure out pass-through compression for Nginx (aspnet/BasicMiddleware #123)](https://github.com/aspnet/BasicMiddleware/issues/123).</span></span>

## <a name="working-with-iis-dynamic-compression"></a><span data-ttu-id="78c03-578">使用 IIS 動態壓縮</span><span class="sxs-lookup"><span data-stu-id="78c03-578">Working with IIS dynamic compression</span></span>

<span data-ttu-id="78c03-579">如果您在想要針對應用程式停用的伺服器層級上設定作用中的 IIS 動態壓縮模組，請停用*包含 web.config 檔案*新增的模組。</span><span class="sxs-lookup"><span data-stu-id="78c03-579">If you have an active IIS Dynamic Compression Module configured at the server level that you would like to disable for an app, disable the module with an addition to the *web.config* file.</span></span> <span data-ttu-id="78c03-580">如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。</span><span class="sxs-lookup"><span data-stu-id="78c03-580">For more information, see [Disabling IIS modules](xref:host-and-deploy/iis/modules#disabling-iis-modules).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="78c03-581">疑難排解</span><span class="sxs-lookup"><span data-stu-id="78c03-581">Troubleshooting</span></span>

<span data-ttu-id="78c03-582">使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)之類的工具，這可讓您設定 `Accept-Encoding` 要求標頭，以及研究回應標頭、大小和主體。</span><span class="sxs-lookup"><span data-stu-id="78c03-582">Use a tool like [Fiddler](https://www.telerik.com/fiddler), [Firebug](https://getfirebug.com/), or [Postman](https://www.getpostman.com/), which allow you to set the `Accept-Encoding` request header and study the response headers, size, and body.</span></span> <span data-ttu-id="78c03-583">根據預設，回應壓縮中介軟體會壓縮符合下列條件的回應：</span><span class="sxs-lookup"><span data-stu-id="78c03-583">By default, Response Compression Middleware compresses responses that meet the following conditions:</span></span>

* <span data-ttu-id="78c03-584">`Accept-Encoding` 標頭存在，其值為 `gzip`、`*`，或符合您所建立之自訂壓縮提供者的自訂編碼。</span><span class="sxs-lookup"><span data-stu-id="78c03-584">The `Accept-Encoding` header is present with a value of `gzip`, `*`, or custom encoding that matches a custom compression provider that you've established.</span></span> <span data-ttu-id="78c03-585">此值不得為 `identity` 或具有品質值（qvalue，`q`）設定為0（零）。</span><span class="sxs-lookup"><span data-stu-id="78c03-585">The value must not be `identity` or have a quality value (qvalue, `q`) setting of 0 (zero).</span></span>
* <span data-ttu-id="78c03-586">必須設定 MIME 類型（`Content-Type`），而且必須符合在 <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>上設定的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="78c03-586">The MIME type (`Content-Type`) must be set and must match a MIME type configured on the <xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>.</span></span>
* <span data-ttu-id="78c03-587">要求不能包含 `Content-Range` 標頭。</span><span class="sxs-lookup"><span data-stu-id="78c03-587">The request must not include the `Content-Range` header.</span></span>
* <span data-ttu-id="78c03-588">除非在回應壓縮中介軟體選項中設定了安全通訊協定（HTTPs），否則要求必須使用不安全的通訊協定（HTTP）。</span><span class="sxs-lookup"><span data-stu-id="78c03-588">The request must use insecure protocol (http), unless secure protocol (https) is configured in the Response Compression Middleware options.</span></span> <span data-ttu-id="78c03-589">*請注意，在啟用安全內容壓縮時，[上述](#compression-with-secure-protocol)的危險。*</span><span class="sxs-lookup"><span data-stu-id="78c03-589">*Note the danger [described above](#compression-with-secure-protocol) when enabling secure content compression.*</span></span>

## <a name="additional-resources"></a><span data-ttu-id="78c03-590">其他資源</span><span class="sxs-lookup"><span data-stu-id="78c03-590">Additional resources</span></span>

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [<span data-ttu-id="78c03-591">Mozilla 開發人員網路：接受編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-591">Mozilla Developer Network: Accept-Encoding</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [<span data-ttu-id="78c03-592">RFC 7231 區段3.1.2.1： Content Codings</span><span class="sxs-lookup"><span data-stu-id="78c03-592">RFC 7231 Section 3.1.2.1: Content Codings</span></span>](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [<span data-ttu-id="78c03-593">RFC 7230 區段4.2.3： Gzip 編碼</span><span class="sxs-lookup"><span data-stu-id="78c03-593">RFC 7230 Section 4.2.3: Gzip Coding</span></span>](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [<span data-ttu-id="78c03-594">GZIP 檔案格式規格版本4。3</span><span class="sxs-lookup"><span data-stu-id="78c03-594">GZIP file format specification version 4.3</span></span>](https://www.ietf.org/rfc/rfc1952.txt)

::: moniker-end
