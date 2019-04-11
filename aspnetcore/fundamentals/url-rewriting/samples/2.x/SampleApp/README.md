# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>ASP.NET Core URL 重寫範例 (ASP.NET Core 2.x)

此範例說明 ASP.NET Core 2.x URL 重寫中介軟體的用法。 應用程式會示範 URL 重新導向和 URL 重寫選項。

執行範例時，非檔案回應會傳回當其中一個規則套用至要求 URL 時的重寫或重新導向 URL。 在 XML 和文字檔範例中，靜態檔案中介軟體會在中介軟體重寫要求 URL 之後提供檔案。

## <a name="examples-in-this-sample"></a>這個範例中的範例

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - 成功的狀態碼：302 (已找到)
  - 範例 (重新導向)：**/redirect-rule/{capture_group}** 至 **/redirected/{capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - 成功的狀態碼：200 (沒問題)
  - 範例 (重寫)：**/rewrite-rule/{capture_group_1}/{capture_group_2}** 至 **/rewritten?var1={capture_group_1}&var2={capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - 成功的狀態碼：302 (已找到)
  - 範例 (重新導向)：**/apache-mod-rules-redirect/{capture_group}** 至 **/redirected?id={capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - 成功的狀態碼：200 (沒問題)
  - 範例 (重寫)：**/iis-rules-rewrite/{capture_group}** 至 **/rewritten?id={capture_group}**
* `Add(RedirectXmlFileRequests)`
  - 成功的狀態碼：301 (已永久移動)
  - 範例 (重新導向)：**/file.xml** 至 **/xmlfiles/file.xml**
* `Add(RewriteTextFileRequests)`
  - 成功的狀態碼：200 (沒問題)
  - 範例 (重寫)：**/some_file.txt** 至 **/file.txt**
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - 成功的狀態碼：301 (已永久移動)
  - 範例 (重新導向)：**/image.png** 至 **/png-images/image.png**
  - 範例 (重新導向)：**/image.jpg** 至 **/jpg-images/image.jpg**

## <a name="use-a-physicalfileprovider"></a>使用 PhysicalFileProvider

藉由建立 `PhysicalFileProvider` 以傳入 `AddApacheModRewrite()` 和 `AddIISUrlRewrite()` 方法，您也可以取得 `IFileProvider`：

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a>安全的重新導向延伸模組

此範例包含應用程式的 `WebHostBuilder` 組態，以便使用 URL (`https://localhost:5001`、`https://localhost`) 和測試憑證 (*testCert.pfx*) 來協助瀏覽這些安全的重新導向方法。 如果伺服器已指派或正在使用連接埠 443，則 `https://localhost` 範例不適用&mdash;請從 *Program.cs* 檔案的 `CreateWebHostBuilder` 方法中移除連接埠 443 的 `ListenOptions`，或解除繫結伺服器上的連接埠 443，讓 Kestrel 可以使用該連接埠。

| 方法                           | 狀態碼 |    連接埠    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     301     | null (465) |
| `.AddRedirectToHttps()`          |     302     | null (465) |
| `.AddRedirectToHttps(301)`       |     301     | null (465) |
| `.AddRedirectToHttps(301, 5001)` |     301     |    5001    |
