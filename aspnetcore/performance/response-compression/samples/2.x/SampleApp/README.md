# <a name="response-compression-sample-application-aspnet-core-2x"></a>回應壓縮範例應用程式（ASP.NET Core 2.x）

這個範例說明如何使用 ASP.NET Core 2.x 回應壓縮中介軟體來壓縮 HTTP 回應。 此範例示範適用于文字和影像回應的 Gzip、Brotli 和自訂壓縮提供者，並說明如何新增 MIME 類型以進行壓縮。 如 ASP.NET Core 1.x 範例，請參閱[回應壓縮範例應用程式（ASP.NET Core 1.x）](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x)。

## <a name="examples-in-this-sample"></a>這個範例中的範例

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** Lorem Ipsum 文字檔回應，以2044位元組為單位，會壓縮為 ~ 979 個位元組。
    * 以1033位元組為單位的 **/Testfile1kb.txt**文字檔回應，會壓縮為 ~ 36 個位元組。
    * **/trickle** -在1秒間隔內以單一字元發出的回應。
* `GzipCompressionProvider`
  * `text/plain`
    * **/** Lorem Ipsum 文字檔回應，以2044位元組為單位，會壓縮為 ~ 927 個位元組。
    * 以1033位元組為單位的 **/Testfile1kb.txt**文字檔回應，會壓縮為 ~ 47 個位元組。
    * **/trickle** -在1秒間隔內以單一字元發出的回應。
  * `image/svg+xml`
    * **/banner.svg** -可調整的向量圖形（svg）影像回應，9707個位元組，會壓縮為 ~ 4459 個位元組。
* `CustomCompressionProvider`<br>示範如何執行自訂壓縮提供者以搭配中介軟體使用。

當要求包含 `Accept-Encoding` 標頭，且回應壓縮成功時，中介軟體會自動將 `Vary: Accept-Encoding` 標頭新增至回應。 `Vary` 標頭會指示快取根據 `Accept-Encoding`的替代值來維護多個回應複本，因此壓縮的（Gzip 或 Brotli）和未壓縮的版本都會儲存在快取中，以供可以接受壓縮或未壓縮回應的系統使用。

## <a name="use-the-sample"></a>使用範例

1. 使用[Fiddler](https://www.telerik.com/fiddler)、 [Firebug](https://getfirebug.com/)或[Postman](https://www.getpostman.com/)對應用程式提出要求，而不 `Accept-Encoding` 標頭，並記下回應承載、回應大小和回應標頭。
1. 新增 `Accept-Encoding: br` 或 `Accept-Encoding: gzip` 標頭，並記下壓縮的回應大小和回應標頭。 回應大小會下降，而中介軟體會包含 `Content-Encoding` 回應標頭，表示發生 Gzip 或 Brotli 的壓縮。 當您查看 Lorem Ipsum 或**testfile1kb**的回應主體時，您會看到文字已壓縮且無法讀取。
1. 新增 `Accept-Encoding: mycustomcompression` 標頭，並記下回應標頭。 `CustomCompressionProvider` 是空的執行，實際上不會壓縮回應，但是您可以為 `CreateStream()` 方法建立自訂的壓縮資料流程包裝函式。
