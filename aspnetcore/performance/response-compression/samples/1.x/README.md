# <a name="response-compression-sample-application-aspnet-core-1x"></a>回應壓縮範例應用程式 (ASP.NET Core 1.x)

此範例說明如何使用 ASP.NET Core 1.x 至壓縮的 HTTP 回應的回應壓縮中介軟體。 此範例示範 Gzip 和自訂壓縮提供者的文字和影像的回應，並示範如何加入壓縮的 MIME 類型。 如需 ASP.NET Core 2.x 範例，請參閱[回應壓縮範例應用程式 (ASP.NET Core 2.x)](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x)。

## <a name="examples-in-this-sample"></a>這個範例中的範例

* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum 文字檔案回應 2,044 將經過壓縮以 927 位元組的位元組
    * **/testfile1kb.txt** -文字檔案回應 1,033 將經過壓縮以 47 個位元組的位元組
    * **/ 緩慢**-在 1 秒的間隔發出為單一字元的回應
  * `image/svg+xml`
    * **/banner.svg** -可縮放向量圖形 (SVG) 映像回應 9,707 將經過壓縮以 4,459 位元組的位元組
* `CustomCompressionProvider`<br>示範如何實作與中介軟體搭配使用自訂壓縮提供者

當要求包含`Accept-Encoding`標頭，此範例會將`Vary: Accept-Encoding`至回應的標頭。 `Vary`標頭會指示來維護多份的替代值為基礎的回應的快取`Accept-Encoding`，因此壓縮 (gzip) 」 和 「 未壓縮的版本會儲存在快取的系統，可以接受壓縮或未壓縮的回應。

## <a name="using-the-sample"></a>使用範例

1. 請要求，使用[Fiddler](https://www.telerik.com/fiddler)， [Firebug](https://getfirebug.com/)，或[Postman](https://www.getpostman.com/)應用程式，而不需要`Accept-Encoding`標頭，並記下回應承載，回應大小和回應標頭。
1. 新增`Accept-Encoding: gzip`標頭，並記下 壓縮的回應大小和回應標頭。 您會看到回應大小卸除，而`Content-Encoding: gzip`回應標頭會包含範例應用程式。 當您查看回應內文 Lorem Ipsum 或**testfile1kb.txt**回應，您會看到文字是壓縮的而且無法讀取。
1. 新增`Accept-Encoding: mycustomcompression`標頭，並記下回應標頭。 `CustomCompressionProvider`是空的實作，並不實際壓縮回應，但您可以建立自訂的壓縮資料流包裝函式`CreateStream()`方法。
