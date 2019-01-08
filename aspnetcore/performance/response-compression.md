---
title: ASP.NET Core 中的回應壓縮
author: guardrex
description: 了解回應壓縮及如何使用 ASP.NET Core 應用程式中的回應壓縮中介軟體。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2018
uid: performance/response-compression
ms.openlocfilehash: a9f72a6816298b11e7b7d30b2b4bd44083baab3a
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54099035"
---
# <a name="response-compression-in-aspnet-core"></a>ASP.NET Core 中的回應壓縮

作者：[Luke Latham](https://github.com/guardrex)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

網路頻寬是有限的資源。 減少回應的大小通常應用程式的回應速度通常會大幅增加。 減少承載大小的方法之一是壓縮應用程式的回應。

## <a name="when-to-use-response-compression-middleware"></a>回應壓縮中介軟體的使用時機

使用 IIS、 Apache 或 Nginx 中的伺服器為基礎的回應壓縮技術。 中介軟體的效能可能不會符合伺服器模組。 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)伺服器和[Kestrel](xref:fundamentals/servers/kestrel)伺服器目前並未提供內建壓縮支援。

當您準備時，請使用回應壓縮中介軟體：

* 無法使用下列伺服器為基礎的壓縮技術：
  * [IIS 動態壓縮模組](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate 模組](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx 壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* 直接在裝載：
  * [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)（先前稱為 WebListener）
  * [Kestrel 伺服器](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>回應壓縮

通常，任何原本不壓縮的回應可以獲益於回應壓縮。 回應原本不壓縮通常包括：CSS、 JavaScript、 HTML、 XML 和 JSON。 您不應該壓縮原生壓縮的資產，例如 PNG 檔案。 如果您嘗試以進一步將壓縮的原生壓縮的回應，降低小的大小和傳輸的時間將可能會執行處理壓縮花費的時間。 不壓縮檔案小於大約 150 1000年位元組 （取決於檔案的內容及壓縮的效率）。 壓縮的小檔案的額外負荷可能會產生比未壓縮的檔案較大的壓縮的檔。

當用戶端可以處理壓縮的內容時，用戶端必須傳送通知其功能的伺服器`Accept-Encoding`與要求的標頭。 當伺服器傳送壓縮的內容時，它必須包含資訊`Content-Encoding`標頭壓縮的回應編碼的方式。 中介軟體所支援的內容編碼表示法會顯示下表中。

::: moniker range=">= aspnetcore-2.2"

| `Accept-Encoding` 標頭值 | 支援的中介軟體 | 描述 |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | [是] （預設值）        | [Brotli 壓縮的資料格式](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | 否                   | [DEFLATE 壓縮的資料格式](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | 否                   | [W3C 有效的 XML 交換](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | 是                  | [Gzip 檔案格式](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | 是                  | 「 沒有編碼 」 的識別項：回應必須不會進行編碼。 |
| `pack200-gzip`                  | 否                   | [Java 封存的網路傳輸格式](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | 是                  | 編碼不明確要求任何可用的內容 |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

| `Accept-Encoding` 標頭值 | 支援的中介軟體 | 描述 |
| ------------------------------- | :------------------: | ----------- |
| `br`                            | 否                   | [Brotli 壓縮的資料格式](https://tools.ietf.org/html/rfc7932) |
| `deflate`                       | 否                   | [DEFLATE 壓縮的資料格式](https://tools.ietf.org/html/rfc1951) |
| `exi`                           | 否                   | [W3C 有效的 XML 交換](https://tools.ietf.org/id/draft-varga-netconf-exi-capability-00.html) |
| `gzip`                          | [是] （預設值）        | [Gzip 檔案格式](https://tools.ietf.org/html/rfc1952) |
| `identity`                      | 是                  | 「 沒有編碼 」 的識別項：回應必須不會進行編碼。 |
| `pack200-gzip`                  | 否                   | [Java 封存的網路傳輸格式](https://jcp.org/aboutJava/communityprocess/review/jsr200/index.html) |
| `*`                             | 是                  | 編碼不明確要求任何可用的內容 |

::: moniker-end

如需詳細資訊，請參閱 < [IANA 官方內容撰寫程式碼清單](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。

中介軟體可讓您新增額外的壓縮提供者的自訂`Accept-Encoding`標頭值。 如需詳細資訊，請參閱 <<c0> [ 自訂提供者](#custom-providers)如下。

中介軟體可以回應的品質值 (qvalue， `q`) 加權時由用戶端傳送到設定優先權的壓縮配置。 如需詳細資訊，請參閱[RFC 7231:接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。

壓縮演算法受限於壓縮速度與壓縮的效能之間取捨。 *有效性*在此內容中是指輸出的大小在壓縮後的。 最小的大小之後，即可最*最佳*壓縮。

參與要求的標頭，傳送、 快取，並接收壓縮的內容是下表中所述。

| 頁首             | 角色 |
| ------------------ | ---- |
| `Accept-Encoding`  | 表示編碼配置給用戶端可接受的內容，從用戶端傳送到伺服器。 |
| `Content-Encoding` | 表示承載中的內容編碼，從伺服器傳送至用戶端。 |
| `Content-Length`   | 壓縮時，`Content-Length`移除標頭，因為本文的內容變更時壓縮回應。 |
| `Content-MD5`      | 壓縮時，`Content-MD5`移除標頭，因為本文內容已變更，而且雜湊已不再有效。 |
| `Content-Type`     | 指定內容的 MIME 類型。 每個回應應該指定其`Content-Type`。 中介軟體會檢查此值，以判斷應該將壓縮的回應。 中介軟體會指定一組[預設 MIME 類型](#mime-types)即可進行編碼，但您可以取代或新增 MIME 類型。 |
| `Vary`             | 值是伺服器所傳送`Accept-Encoding`用戶端和 proxy`Vary`標頭會指出用戶端或 proxy，應快取 （而有所不同） 回應為基礎的值`Accept-Encoding`要求標頭。 傳回內容的結果`Vary: Accept-Encoding`標頭時，壓縮和未壓縮的回應會分別快取。 |

瀏覽的功能與回應壓縮中介軟體[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)。 此範例會說明：

* 使用 Gzip 和自訂壓縮提供者的應用程式回應的壓縮。
* 如何將 MIME 類型加入至壓縮的 MIME 類型的預設清單。

## <a name="package"></a>封裝

::: moniker range=">= aspnetcore-2.1"

若要在專案中包含中介軟體，將參考加入[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，其中包括[Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

若要在專案中包含中介軟體，將參考加入[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)，其中包括[Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

若要在專案中包含中介軟體，將參考加入[Microsoft.AspNetCore.ResponseCompression](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝。

::: moniker-end

## <a name="configuration"></a>組態

::: moniker range=">= aspnetcore-2.2"

下列程式碼示範如何啟用預設的 MIME 類型和壓縮提供者在回應壓縮中介軟體 ([Brotli](#brotli-compression-provider)並[Gzip](#gzip-compression-provider)):

::: moniker-end

::: moniker range="< aspnetcore-2.2"

下列程式碼示範如何啟用預設的 MIME 類型回應壓縮中介軟體和[Gzip 壓縮提供者](#gzip-compression-provider):

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

附註： 

* `app.UseResponseCompression` 之前必須先呼叫`app.UseMvc`。
* 使用一種工具，例如[Fiddler](http://www.telerik.com/fiddler)， [Firebug](http://getfirebug.com/)，或[Postman](https://www.getpostman.com/)設`Accept-Encoding`要求標頭，然後研究回應標頭、 大小和主體。

將要求提交到範例應用程式，而不需要`Accept-Encoding`標頭，並觀察回應是未壓縮。 `Content-Encoding`和`Vary`標頭不存在的回應。

![Fiddler 視窗會顯示沒有 Accept-encoding 標頭之要求的結果。 回應不會壓縮。](response-compression/_static/request-uncompressed.png)

::: moniker range=">= aspnetcore-2.2"

將要求提交到範例應用程式與`Accept-Encoding: br`標頭 （Brotli 壓縮），並觀察壓縮回應。 `Content-Encoding`和`Vary`標頭有回應。

![Fiddler 視窗中顯示結果的要求包含 Accept-encoding 標頭和 b 的值。 Vary 和 Content-encoding 標頭加入回應。 回應會自動壓縮。](response-compression/_static/request-compressed-br.png)

::: moniker-end

::: moniker range="< aspnetcore-2.2"

將要求提交到範例應用程式與`Accept-Encoding: gzip`標頭，並觀察壓縮回應。 `Content-Encoding`和`Vary`標頭有回應。

![顯示包含 Accept-encoding 標頭之要求的結果而值為 gzip 的 fiddler 視窗。 Vary 和 Content-encoding 標頭加入回應。 回應會自動壓縮。](response-compression/_static/request-compressed.png)

::: moniker-end

## <a name="providers"></a>提供者

::: moniker range=">= aspnetcore-2.2"

### <a name="brotli-compression-provider"></a>Brotli 壓縮提供者

使用`BrotliCompressionProvider`壓縮回應[Brotli 壓縮的資料格式](https://tools.ietf.org/html/rfc7932)。

如果任何壓縮提供者明確不新增至<xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

* 預設會新增 Brotli 壓縮提供者連同壓縮提供者的陣列[Gzip 壓縮提供者](#gzip-compression-provider)。
* 當用戶端支援 Brotli 壓縮的資料格式時，壓縮會預設為 Brotli 壓縮。 如果用戶端不支援 Brotli，壓縮會預設為 Gzip 當用戶端支援 Gzip 壓縮。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

明確地新增任何壓縮提供者時，必須加入 Brotoli 壓縮提供者：

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

設定壓縮層級使用`BrotliCompressionProviderOptions`。 Brotli 壓縮提供者預設為最快的壓縮層級 ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel))，這可能不會產生最有效的壓縮。 如果想要最有效的壓縮，請設定最佳的壓縮中介軟體。

| 壓縮層級 | 描述 |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | 即使未以最佳方式壓縮所產生的輸出，應儘速完成壓縮。 |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | 應該不執行任何壓縮。 |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | 回應應該以最佳方式壓縮，即使壓縮需要更多的時間才能完成。 |

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

### <a name="gzip-compression-provider"></a>Gzip 壓縮提供者

使用<xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProvider>壓縮回應[Gzip 檔案格式](https://tools.ietf.org/html/rfc1952)。

如果任何壓縮提供者明確不新增至<xref:Microsoft.AspNetCore.ResponseCompression.CompressionProviderCollection>:

::: moniker range=">= aspnetcore-2.2"

* Gzip 壓縮提供者會加入預設的壓縮提供者，以及陣列[Brotli 壓縮提供者](#brotli-compression-provider)。
* 當用戶端支援 Brotli 壓縮的資料格式時，壓縮會預設為 Brotli 壓縮。 如果用戶端不支援 Brotli，壓縮會預設為 Gzip 當用戶端支援 Gzip 壓縮。

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* Gzip 壓縮提供者會依預設加入壓縮提供者的陣列。
* 當用戶端支援 Gzip 壓縮，壓縮會預設為 Gzip。

::: moniker-end

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddResponseCompression();
}
```

明確地新增任何壓縮提供者時，必須加入 Gzip 壓縮提供者：

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

設定壓縮層級使用<xref:Microsoft.AspNetCore.ResponseCompression.GzipCompressionProviderOptions>。 Gzip 壓縮提供者預設為最快的壓縮層級 ([CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel))，這可能不會產生最有效的壓縮。 如果想要最有效的壓縮，請設定最佳的壓縮中介軟體。

| 壓縮層級 | 描述 |
| ----------------- | ----------- |
| [CompressionLevel.Fastest](xref:System.IO.Compression.CompressionLevel) | 即使未以最佳方式壓縮所產生的輸出，應儘速完成壓縮。 |
| [CompressionLevel.NoCompression](xref:System.IO.Compression.CompressionLevel) | 應該不執行任何壓縮。 |
| [CompressionLevel.Optimal](xref:System.IO.Compression.CompressionLevel) | 回應應該以最佳方式壓縮，即使壓縮需要更多的時間才能完成。 |

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

### <a name="custom-providers"></a>自訂提供者

建立使用自訂壓縮實作<xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider>。 <xref:Microsoft.AspNetCore.ResponseCompression.ICompressionProvider.EncodingName*>表示將內容編碼這個`ICompressionProvider`產生。 中介軟體會使用此資訊來選擇清單中指定為基礎的提供者`Accept-Encoding`要求標頭。

使用範例應用程式，在用戶端提交的要求`Accept-Encoding: mycustomcompression`標頭。 中介軟體會使用自訂壓縮實作，並傳回包含回應`Content-Encoding: mycustomcompression`標頭。 用戶端必須能夠解壓縮自訂壓縮實作，才能讓自訂編碼。

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

將要求提交到範例應用程式與`Accept-Encoding: mycustomcompression`標頭，並觀察回應標頭。 `Vary`和`Content-Encoding`標頭有回應。 此範例不壓縮 （未顯示），回應主體。 沒有在壓縮實作`CustomCompressionProvider`類別的範例。 不過，此範例會顯示您將在其中實作這類的壓縮演算法。

![顯示結果的要求包含 Accept-encoding 標頭的值，並針對 mycustomcompression fiddler 視窗。 Vary 和 Content-encoding 標頭加入回應。](response-compression/_static/request-custom-compression.png)

## <a name="mime-types"></a>MIME 類型

中介軟體會指定一組預設的壓縮的 MIME 類型：

* `application/javascript`
* `application/json`
* `application/xml`
* `text/css`
* `text/html`
* `text/json`
* `text/plain`
* `text/xml`

取代或附加的回應壓縮中介軟體選項的 MIME 類型。 請注意該萬用字元 MIME 類型，例如`text/*`不支援。 範例應用程式新增的 MIME 類型`image/svg+xml`和壓縮，並提供 ASP.NET Core 橫幅影像 (*banner.svg*)。

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

## <a name="compression-with-secure-protocol"></a>使用安全的通訊協定的壓縮

透過安全的連線壓縮的回應可以控制與`EnableForHttps`選項，預設會停用。 使用動態產生的頁面壓縮可能會導致安全性問題這類[犯罪](https://wikipedia.org/wiki/CRIME_(security_exploit))並[缺口](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。

## <a name="adding-the-vary-header"></a>新增 Vary 標頭

::: moniker range=">= aspnetcore-2.0"

當壓縮回應基礎`Accept-Encoding`標頭，有可能的多個壓縮的版本回應和未壓縮的版本。 若要指示用戶端和 proxy 的快取多個版本存在，並應該儲存`Vary`標頭加`Accept-Encoding`值。 在 ASP.NET Core 2.0 或更新版本中中, 介軟體新增`Vary`標頭壓縮回應時，自動。

::: moniker-end

::: moniker range="< aspnetcore-2.0"

當壓縮回應基礎`Accept-Encoding`標頭，有可能的多個壓縮的版本回應和未壓縮的版本。 若要指示用戶端和 proxy 的快取多個版本存在，並應該儲存`Vary`標頭加`Accept-Encoding`值。 在 ASP.NET Core 1.x 中，新增`Vary`至回應的標頭以手動方式完成：

[!code-csharp[](response-compression/samples/1.x/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>Nginx 背後時的中介軟體問題的反向 proxy

當要求 proxy 處理的 Nginx`Accept-Encoding`標頭移除。 移除`Accept-Encoding`標頭會防止從壓縮回應的中介軟體。 如需詳細資訊，請參閱[NGINX:壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。 此問題會追蹤[找出 nginx 傳遞壓縮 (aspnet/BasicMiddleware \#123)](https://github.com/aspnet/BasicMiddleware/issues/123)。

## <a name="working-with-iis-dynamic-compression"></a>使用 IIS 動態壓縮

如果您有作用中 IIS 動態壓縮模組在您想要停用的應用程式伺服器層級設定時，停用的模組使用的補充*web.config*檔案。 如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。

## <a name="troubleshooting"></a>疑難排解

使用之類的工具[Fiddler](https://www.telerik.com/fiddler)， [Firebug](https://getfirebug.com/)，或[Postman](https://www.getpostman.com/)，這可讓您設定`Accept-Encoding`要求標頭，然後研究回應標頭、 大小和主體。 根據預設，回應壓縮中介軟體會壓縮回應，符合下列條件：

::: moniker range=">= aspnetcore-2.2"

* `Accept-Encoding`標頭已存在的值`br`， `gzip`， `*`，或符合您所建立的自訂壓縮提供者的自訂編碼方式。 值不能`identity`或有某個品質值 (qvalue， `q`) 設定為 0 （零）。
* MIME 類型 (`Content-Type`) 必須設定，而且必須符合上設定的 MIME 類型<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>。
* 要求必須包含`Content-Range`標頭。
* 要求必須使用不安全的通訊協定 (http)，除非已在回應壓縮中介軟體選項中設定安全的通訊協定 (https)。 *請注意危險[上述](#compression-with-secure-protocol)時啟用安全的內容壓縮。*

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* `Accept-Encoding`標頭已存在的值`gzip`， `*`，或符合您所建立的自訂壓縮提供者的自訂編碼方式。 值不能`identity`或有某個品質值 (qvalue， `q`) 設定為 0 （零）。
* MIME 類型 (`Content-Type`) 必須設定，而且必須符合上設定的 MIME 類型<xref:Microsoft.AspNetCore.ResponseCompression.ResponseCompressionOptions>。
* 要求必須包含`Content-Range`標頭。
* 要求必須使用不安全的通訊協定 (http)，除非已在回應壓縮中介軟體選項中設定安全的通訊協定 (https)。 *請注意危險[上述](#compression-with-secure-protocol)時啟用安全的內容壓縮。*

::: moniker-end

## <a name="additional-resources"></a>其他資源

* <xref:fundamentals/startup>
* <xref:fundamentals/middleware/index>
* [Mozilla 開發人員網路：接受編碼](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 節 3.1.2.1:內容 Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [第 4.2.3 RFC 7230 節：Gzip 編碼](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [GZIP 檔案格式規格 4.3 版](http://www.ietf.org/rfc/rfc1952.txt)
