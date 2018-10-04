# <a name="response-compression-sample-application-aspnet-core-2x"></a>回應壓縮範例應用程式 (ASP.NET Core 2.x)

> [!IMPORTANT]
> 此範例會使用 ASP.NET Core 2.2 Preview 2 SDK 和執行階段。 取得從預覽版[下載.NET Core 2.2](https://www.microsoft.com/net/download/dotnet-core/2.2)。 確認中的 SDK 版本*global.json*檔案並[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)版本是否正確，您的 SDK 和共用的 framework 版本的專案檔中。

此範例說明如何使用 ASP.NET Core 2.x 壓縮的 HTTP 回應的回應壓縮中介軟體。 此範例示範 Gzip、 Brotli 和自訂壓縮提供者的文字和影像的回應，並示範如何加入壓縮的 MIME 類型。 如需 ASP.NET Core 1.x 範例，請參閱[回應壓縮範例應用程式 (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/1.x)。

## <a name="examples-in-this-sample"></a>這個範例中的範例

* `BrotliCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum 文字檔案回應為 2,044 個位元組壓縮為 ~ 979 個位元組。
    * **/testfile1kb.txt** -1,033 位元組在文字檔案回應，會壓縮為 ~ 36 個位元組。
    * **/ 緩慢**-在 1 秒的間隔發出為單一字元的回應。
* `GzipCompressionProvider`
  * `text/plain`
    * **/** -Lorem Ipsum 文字檔案回應為 2,044 個位元組壓縮為 ~ 927 個位元組。
    * **/testfile1kb.txt** -1,033 位元組在文字檔案回應，會壓縮為 ~ 47 個位元組。
    * **/ 緩慢**-在 1 秒的間隔發出為單一字元的回應。
  * `image/svg+xml`
    * **/banner.svg** -可縮放向量圖形 (SVG) 映像回應 9,707 位元組可壓縮為 ~ 4,459 個位元組。
* `CustomCompressionProvider`<br>示範如何實作與中介軟體搭配使用自訂壓縮提供者。

當要求包含`Accept-Encoding`標頭和回應的壓縮是成功中, 介軟體會自動將新增`Vary: Accept-Encoding`標頭至回應。 `Vary`標頭會指示來維護多份的替代值為基礎的回應的快取`Accept-Encoding`，因此這兩個壓縮 （Gzip 或 Brotli） 和未壓縮的版本儲存在快取中的系統，可以接受壓縮或未壓縮的回應。

## <a name="using-the-sample"></a>使用範例

1. 請要求，使用[Fiddler](http://www.telerik.com/fiddler)， [Firebug](http://getfirebug.com/)，或[Postman](https://www.getpostman.com/)應用程式，而不需要`Accept-Encoding`標頭，並記下回應承載，回應大小和回應標頭。
1. 新增`Accept-Encoding: br`或`Accept-Encoding: gzip`標頭，並記下 壓縮的回應大小和回應標頭。 回應大小會卸除，而`Content-Encoding`指出該以任一 Gzip 壓縮中介軟體會包含回應標頭或 Brotli 發生。 當您查看回應內文 Lorem Ipsum 或**testfile1kb.txt**回應，您會看到文字是壓縮的而且無法讀取。
1. 新增`Accept-Encoding: mycustomcompression`標頭，並記下回應標頭。 `CustomCompressionProvider`是空的實作，並不實際壓縮回應，但您可以建立自訂的壓縮資料流包裝函式`CreateStream()`方法。
