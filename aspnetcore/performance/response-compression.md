---
title: "ASP.NET Core 壓縮回應中介軟體"
author: guardrex
description: "了解回應壓縮以及如何在 ASP.NET Core 應用程式中使用回應壓縮中介軟體。"
manager: wpickett
ms.author: riande
ms.date: 08/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: performance/response-compression
ms.openlocfilehash: c10f94b40fec00e7533cc3a6e88daa3f3da614ed
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="response-compression-middleware-for-aspnet-core"></a>ASP.NET Core 壓縮回應中介軟體

作者：[Luke Latham](https://github.com/guardrex)

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

網路頻寬是有限的資源。 降低回應的大小通常應用程式的回應能力通常會大幅增加。 若要減少內容大小的方法之一是壓縮應用程式的回應。

## <a name="when-to-use-response-compression-middleware"></a>使用回應壓縮中介軟體的時機
使用 IIS、 Apache 或 Nginx 中的伺服器回應壓縮技術。 中介軟體的效能可能不符合伺服器模組。 [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)和[Kestrel](xref:fundamentals/servers/kestrel)目前並未提供內建壓縮支援。

當您準備使用回應壓縮中介軟體：

* 無法使用下列伺服器為基礎的壓縮技術：
  * [IIS 動態壓縮模組](https://www.iis.net/overview/reliability/dynamiccachingandcompression)
  * [Apache mod_deflate 模組](http://httpd.apache.org/docs/current/mod/mod_deflate.html)
  * [Nginx 壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)
* 主機上直接：
  * [HTTP.sys 伺服器](xref:fundamentals/servers/httpsys)(先前稱為[WebListener](xref:fundamentals/servers/weblistener))
  * [Kestrel](xref:fundamentals/servers/kestrel)

## <a name="response-compression"></a>回應的壓縮
通常，任何原本不壓縮的回應可以從回應壓縮中受益。 回應原本不壓縮通常包括： CSS、 JavaScript、 HTML、 XML 和 JSON。 您不應該壓縮原生壓縮的資產，例如 PNG 檔案。 如果您嘗試以進一步壓縮原生壓縮的回應，小型其他致使大小和傳輸的時間將很可能會被掩蓋處理壓縮花費的時間。 不壓縮檔案小於大約 150-1000年位元組 （取決於檔案的內容和壓縮效率）。 壓縮的小檔案的額外負荷可能會產生比未壓縮的檔案較大的壓縮的檔。

當用戶端可以處理壓縮的內容時，用戶端必須透過傳送通知的伺服器，其功能`Accept-Encoding`與要求標頭。 當伺服器傳送壓縮的內容時，它必須包括中的資訊`Content-Encoding`標頭壓縮的回應編碼的方式。 下表中，會顯示由中介軟體所支援的內容編碼方式指定。

| `Accept-Encoding`標頭值 | 支援的中介軟體 | 描述                                                 |
| :-----------------------------: | :------------------: | ----------------------------------------------------------- |
| `br`                            | 否                   | Brotli 壓縮的資料格式                               |
| `compress`                      | 否                   | UNIX 「 壓縮 」 的資料格式                                 |
| `deflate`                       | 否                   | 「 deflate 」 內的 「 zlib 」 資料格式的壓縮的資料     |
| `exi`                           | 否                   | W3C 有效率 XML 交換                               |
| `gzip`                          | [是] （預設值）        | gzip 檔案格式                                            |
| `identity`                      | [是]                  | 「 無編碼 」 的識別項： 必須編碼回應。 |
| `pack200-gzip`                  | 否                   | Java 封存的網路傳輸格式                   |
| `*`                             | [是]                  | 任何可用的內容編碼不明確要求     |

如需詳細資訊，請參閱[IANA 官方內容編碼清單](http://www.iana.org/assignments/http-parameters/http-parameters.xml#http-content-coding-registry)。

中介軟體可讓您新增額外的壓縮提供者的自訂`Accept-Encoding`標頭值。 如需詳細資訊，請參閱[自訂提供者](#custom-providers)下方。

中介軟體是否能夠對回應品質值 (qvalue， `q`) 加權時由用戶端傳送至設定優先權的壓縮配置。 如需詳細資訊，請參閱[RFC 7231： 接受編碼](https://tools.ietf.org/html/rfc7231#section-5.3.4)。

壓縮演算法受限於壓縮速度與壓縮的效能之間權衡取捨。 *有效性*在此內容中參照輸出大小的壓縮後。 最小大小達成最*最佳*壓縮。

參與要求的標頭，傳送、 快取，並接收壓縮的內容是下表中所述。

| 頁首             | 角色 |
| ------------------ | ---- |
| `Accept-Encoding`  | 表示配置給用戶端可接受的編碼方式的內容，從用戶端傳送到伺服器。 |
| `Content-Encoding` | 指出在裝載中的內容編碼方式，從伺服器傳送至用戶端。 |
| `Content-Length`   | 壓縮時，`Content-Length`移除標頭，因為主體的內容變更壓縮回應時。 |
| `Content-MD5`      | 壓縮時，`Content-MD5`移除標頭，因為本文內容已變更，雜湊已不再有效。 |
| `Content-Type`     | 指定內容的 MIME 類型。 應該指定每個回應其`Content-Type`。 中介軟體會檢查此值來判斷是否應該壓縮回應。 中介軟體中指定的一組[預設 MIME 類型](#mime-types)即可進行編碼，但您可以取代或新增 MIME 類型。 |
| `Vary`             | 值是伺服器所傳送`Accept-Encoding`到用戶端和 proxy，`Vary`標頭指出的用戶端或 proxy，它應該快取 （而有所不同） 的值為基礎的回應`Accept-Encoding`要求標頭。 傳回具有內容的結果`Vary: Accept-Encoding`標頭是同時壓縮，而且未壓縮的回應會分別快取。 |

您可以瀏覽與回應壓縮中介軟體功能[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples)。 此範例將說明：
* 使用 gzip 和自訂壓縮提供者的應用程式回應的壓縮。
* 如何將 MIME 類型加入至壓縮的 MIME 類型的預設清單。

## <a name="package"></a>Package
若要在專案中包含中介軟體，將參考加入[ `Microsoft.AspNetCore.ResponseCompression` ](https://www.nuget.org/packages/Microsoft.AspNetCore.ResponseCompression/)封裝，或使用[ `Microsoft.AspNetCore.All` ](https://www.nuget.org/packages/Microsoft.AspNetCore.All/)封裝。 這項功能是適用於 ASP.NET Core 1.1 版為目標的應用程式或更新版本。

## <a name="configuration"></a>組態
下列程式碼會示範如何啟用回應壓縮中介軟體搭配預設 gzip 壓縮和預設的 MIME 類型。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/StartupBasic.cs?name=snippet1&highlight=4,8)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/StartupBasic.cs?name=snippet1&highlight=3,8)]

---

> [!NOTE]
> 使用這類工具[Fiddler](http://www.telerik.com/fiddler)， [firebug 這類](http://getfirebug.com/)，或[郵差](https://www.getpostman.com/)設定`Accept-Encoding`要求標頭和研究回應標頭、 大小和主體。

將要求提交到範例應用程式，而不`Accept-Encoding`標頭，並觀察回應未壓縮。 `Content-Encoding`和`Vary`標頭中沒有顯示的回應。

![Fiddler 視窗顯示沒有 Accept-encoding 標頭之要求的結果。 未壓縮的回應。](response-compression/_static/request-uncompressed.png)

將要求提交到範例應用程式與`Accept-Encoding: gzip`標頭，並觀察壓縮回應。 `Content-Encoding`和`Vary`標頭會出現在回應上。

![顯示具有 Accept-encoding 標頭之要求的結果而值為 gzip fiddler 視窗。 Vary 和內容編碼標頭會新增至回應。 壓縮回應。](response-compression/_static/request-compressed.png)

## <a name="providers"></a>提供者
### <a name="gzipcompressionprovider"></a>GzipCompressionProvider
使用`GzipCompressionProvider`壓縮回應以 gzip。 如果未指定，這是預設壓縮提供者。 您可以設定壓縮層級與`GzipCompressionProviderOptions`。 

Gzip 壓縮提供者預設為最快的壓縮層級 (`CompressionLevel.Fastest`)，這可能不會產生最有效的壓縮。 如果想要使用最有效率的壓縮，您可以設定最佳的壓縮的中介軟體。

| 壓縮層級                | 描述                                                                                                   |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------- |
| `CompressionLevel.Fastest`       | 即使不最佳的方式壓縮所產生的輸出，應該儘快，完成壓縮。 |
| `CompressionLevel.NoCompression` | 您應該不執行任何壓縮。                                                                           |
| `CompressionLevel.Optimal`       | 回應應以最佳方式壓縮，即使壓縮會使用更多時間來完成。                |


# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=3,8-11)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=5,10-13)]

---

## <a name="mime-types"></a>MIME 類型
中介軟體會指定一組預設的 MIME 類型的壓縮：
* `text/plain`
* `text/css`
* `application/javascript`
* `text/html`
* `application/xml`
* `text/xml`
* `application/json`
* `text/json`

您可以取代或附加的回應壓縮中介軟體選項的 MIME 類型。 請注意，萬用字元 MIME 類型，例如`text/*`不支援。 範例應用程式新增 MIME 類型`image/svg+xml`和壓縮，並做 ASP.NET Core 橫幅影像 (*banner.svg*)。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=7)]

---

### <a name="custom-providers"></a>自訂提供者
您可以建立自訂壓縮實作與`ICompressionProvider`。 `EncodingName`代表內容的編碼這個`ICompressionProvider`產生。 中介軟體會使用此資訊來選擇根據清單中指定的提供者`Accept-Encoding`要求標頭。

使用範例應用程式，在用戶端提交的要求`Accept-Encoding: mycustomcompression`標頭。 中介軟體會使用自訂壓縮實作，並傳回與回應`Content-Encoding: mycustomcompression`標頭。 用戶端必須能夠解壓縮自訂壓縮實作，才能讓自訂編碼。

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

[!code-csharp[Main](response-compression/samples/2.x/Program.cs?name=snippet1&highlight=4)]

[!code-csharp[Main](response-compression/samples/2.x/CustomCompressionProvider.cs?name=snippet1)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet2&highlight=6)]

[!code-csharp[Main](response-compression/samples/1.x/CustomCompressionProvider.cs?name=snippet1)]

---

將要求提交到範例應用程式與`Accept-Encoding: mycustomcompression`標頭，並觀察回應標頭。 `Vary`和`Content-Encoding`標頭會出現在回應上。 此範例不被壓縮 （未顯示），回應主體。 沒有在壓縮實作`CustomCompressionProvider`範例的類別。 不過，此範例會示範您可以在其中實作這類的壓縮演算法。

![顯示具有 Accept-encoding 標頭之要求的結果而值為 mycustomcompression fiddler 視窗。 Vary 和內容編碼標頭會新增至回應。](response-compression/_static/request-custom-compression.png)

## <a name="compression-with-secure-protocol"></a>使用安全通訊協定的壓縮
透過安全的連線壓縮的回應可以控制與`EnableForHttps`選項，預設會停用。 使用動態產生的頁面壓縮可能會導致安全性問題例如[CRIME](https://wikipedia.org/wiki/CRIME_(security_exploit))和[破壞](https://wikipedia.org/wiki/BREACH_(security_exploit))攻擊。

## <a name="adding-the-vary-header"></a>加入 Vary 標頭
當壓縮回應基礎`Accept-Encoding`標頭，但是會有潛在的多個壓縮的版本回應和未壓縮的版本。 若要指示用戶端和 proxy 快取多個版本存在，而且應該儲存`Vary`標頭加入使用`Accept-Encoding`值。 在 ASP.NET Core 1.x 加入`Vary`標頭至回應以手動方式完成。 在 ASP.NET Core 2.x 中, 介軟體新增`Vary`標頭壓縮回應時，自動。

**ASP.NET Core 只 1.x**

[!code-csharp[Main](response-compression/samples/1.x/Startup.cs?name=snippet1)]

## <a name="middleware-issue-when-behind-an-nginx-reverse-proxy"></a>位於 Nginx 反向 proxy 後方的中介軟體問題
當要求 Nginx，由代理`Accept-Encoding`標頭會移除。 這可防止壓縮回應的中介軟體。 如需詳細資訊，請參閱[NGINX： 壓縮和解壓縮](https://www.nginx.com/resources/admin-guide/compression-and-decompression/)。 此問題會追蹤[找出 Nginx (BasicMiddleware #123) 傳遞壓縮](https://github.com/aspnet/BasicMiddleware/issues/123)。

## <a name="working-with-iis-dynamic-compression"></a>使用 IIS 動態壓縮
如果您有使用中 IIS 動態壓縮模組在您想要停用應用程式的伺服器層級設定，您可以新增至與您*web.config*檔案。 如需詳細資訊，請參閱[停用 IIS 模組](xref:host-and-deploy/iis/modules#disabling-iis-modules)。

## <a name="troubleshooting"></a>疑難排解
使用這類工具[Fiddler](http://www.telerik.com/fiddler)， [firebug 這類](http://getfirebug.com/)，或[郵差](https://www.getpostman.com/)，可讓您設定`Accept-Encoding`要求標頭和研究回應標頭、 大小和主體。 回應壓縮中介軟體壓縮符合下列條件的回應：
* `Accept-Encoding`標頭已存在的值與`gzip`， `*`，或自訂編碼符合您所建立的自訂壓縮提供者。 值不能`identity`或有品質值 (qvalue， `q`) 設定為 0 （零）。
* MIME 類型 (`Content-Type`) 必須設定，而且必須符合上設定 MIME 類型`ResponseCompressionOptions`。
* 要求必須包含`Content-Range`標頭。
* 除非回應壓縮中介軟體選項中設定安全通訊協定 (https)，要求必須使用不安全的通訊協定 (http)。 *請注意危險[上述](#compression-with-secure-protocol)時啟用安全內容的壓縮。*

## <a name="additional-resources"></a>其他資源
* [應用程式啟動](xref:fundamentals/startup)
* [中介軟體](xref:fundamentals/middleware/index)
* [Mozilla Developer Network： 接受編碼](https://developer.mozilla.org/docs/Web/HTTP/Headers/Accept-Encoding)
* [RFC 7231 區段 3.1.2.1： 內容 Codings](https://tools.ietf.org/html/rfc7231#section-3.1.2.1)
* [RFC 7230 第 4.2.3 節： Gzip 編碼](https://tools.ietf.org/html/rfc7230#section-4.2.3)
* [GZIP 檔案格式規格 4.3 版](http://www.ietf.org/rfc/rfc1952.txt)
