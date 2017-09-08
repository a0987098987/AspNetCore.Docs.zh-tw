# <a name="response-compression-sample-application-aspnet-core-1x"></a>回應壓縮範例應用程式 (ASP.NET Core 1.x)

此範例說明如何使用 ASP.NET Core 1.x 回應壓縮來壓縮的 HTTP 回應的中介軟體。 此範例示範 Gzip 和自訂壓縮提供者的文字和影像的回應，並示範如何加入壓縮的 MIME 類型。 如需 ASP.NET Core 2.x 範例中，請參閱[回應壓縮範例應用程式 (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/performance/response-compression/samples/2.x)。

## <a name="examples-in-this-sample"></a>在此範例中的範例
* `GzipCompressionProvider`
  * `text/plain`
    * **/**-Lorem Ipsum 檔案回應的文字在 2,044 將經過壓縮以 927 位元組的位元組
    * **/testfile1kb.txt** -回應在 1,033 將經過壓縮以 47 位元組的位元組的文字檔案
    * **緩慢/** -1 秒的間隔為單一字元發出的回應 
  * `image/svg+xml`
    * **/banner.svg** -A 向量圖形 (SVG) 映像回應在 9,707 將經過壓縮以 4,459 位元組的位元組
* `CustomCompressionProvider`<br>示範如何實作與中介軟體搭配使用的自訂壓縮提供者

當要求包含`Accept-Encoding`標頭，此範例加入`Vary: Accept-Encoding`標頭至回應。 `Vary`標頭指示維護多份依據替代值的回應的快取`Accept-Encoding`，因此同時壓縮 (gzip) 和未壓縮的版本會儲存在快取的系統，可以接受壓縮或未壓縮的回應。

## <a name="using-the-sample"></a>使用範例
1. 請要求使用[Fiddler](http://www.telerik.com/fiddler)， [firebug 這類](http://getfirebug.com/)，或[郵差](https://www.getpostman.com/)應用程式不含`Accept-Encoding`標頭和附註的回應裝載，回應大小和回應標頭。
2. 新增`Accept-Encoding: gzip`標頭，並記下壓縮的回應的大小和回應標頭。 您會看到卸除，回應大小和`Content-Encoding: gzip`回應標頭包含範例應用程式。 當您查看回應內文的 Lorem Ipsum 或**testfile1kb.txt**回應，您會看到文字為壓縮，無法讀取。
3. 新增`Accept-Encoding: mycustomcompression`標頭，並記下回應標頭。 `CustomCompressionProvider`是實際上不會壓縮回應的空白實作，但是您可以建立自訂的壓縮資料流包裝函式`CreateStream()`方法。
