# <a name="response-compression-sample-application-aspnet-core-3x"></a>回應壓縮範例應用程式 (ASP.NET Core 3.x)

這個範例說明如何使用 ASP.NET Core 3.x 回應壓縮中介軟體來壓縮 HTTP 回應。 此範例示範適用于文字和影像回應的 Gzip、Brotli 和自訂壓縮提供者, 並說明如何新增 MIME 類型以進行壓縮。 如需 ASP.NET Core 2.x 範例, 請參閱[回應壓縮範例應用程式 (ASP.NET Core 2.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x)。

## <a name="examples-in-this-sample"></a>這個範例中的範例

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum 文字檔回應 (2044 個位元組), 會壓縮到 ~ 979 個位元組。
    * 以1033位元組為單位的 **/Testfile1kb.txt**文字檔回應, 會壓縮為 ~ 36 個位元組。
    * **/trickle** -在1秒間隔內以單一字元發出的回應。
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum 文字檔回應 (2044 個位元組), 會壓縮到 ~ 927 個位元組。
    * 以1033位元組為單位的 **/Testfile1kb.txt**文字檔回應, 會壓縮為 ~ 47 個位元組。
    * **/trickle** -在1秒間隔內以單一字元發出的回應。
  * `image/svg+xml`
    * **/banner.svg** -可調整的向量圖形 (svg) 影像回應, 9707 個位元組, 會壓縮為 ~ 4459 個位元組。
* `CustomCompressionProvider`<br>示範如何執行自訂壓縮提供者以搭配中介軟體使用。

當要求包含`Accept-Encoding`標頭和回應壓縮成功時, 中介軟體會自動`Vary: Accept-Encoding`將標頭新增至回應。 標頭會指示快取根據的`Accept-Encoding`替代值來維護回應的多個複本, 因此壓縮的 (Gzip 或 Brotli) 和未壓縮的版本都會儲存在快取中, 以供可能接受`Vary`已壓縮或未壓縮的回應。

## <a name="use-the-sample"></a>使用範例

1. `Accept-Encoding`在沒有標頭的情況下, 對應用程式使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)提出要求, 並記下回應承載、回應大小和回應標頭。
1. `Accept-Encoding: br`新增或`Accept-Encoding: gzip`標頭, 並記下壓縮的回應大小和回應標頭。 回應大小會下降, 而`Content-Encoding`中介軟體會包含回應標頭, 表示發生 Gzip 或 Brotli 的壓縮。 當您查看 Lorem Ipsum 或**testfile1kb**的回應主體時, 您會看到文字已壓縮且無法讀取。
1. `Accept-Encoding: mycustomcompression`新增標頭並記下回應標頭。 是空的實作為, 並不會實際壓縮回應, 但是您可以`CreateStream()`為方法建立自訂的壓縮資料流程包裝函式。 `CustomCompressionProvider`
