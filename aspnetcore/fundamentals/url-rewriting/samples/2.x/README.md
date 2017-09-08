# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a>ASP.NET Core URL 重寫範例 (ASP.NET Core 2.x)

此範例說明使用 ASP.NET Core 2.x URL 重寫中介軟體。 應用程式示範重新導向 URL 和 URL 重寫選項。 如需 ASP.NET Core 1.x 範例中，請參閱[ASP.NET Core URL 重寫範例 (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x)。

執行範例時，會處理回應會顯示其中一個規則套用至要求的 URL 時的重寫或重新導向 URL。

## <a name="examples-in-this-sample"></a>在此範例中的範例

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - 成功狀態碼： 302 （找到）
  - 範例 （重新導向）： **/redirect-rule / {capture_group}**至**/redirected/ {capture_group}**
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - 成功狀態碼： 200 （確定）
  - 範例 （重寫）： **/rewrite-rule / {capture_group_1} / {capture_group_2}**至**重寫 /？ var1 = {capture_group_1} （& s) var2 = {capture_group_2}**
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - 成功狀態碼： 302 （找到）
  - 範例 （重新導向）： **/apache-mod-rules-redirect / {capture_group}**至**/ 重新導向？ id = {capture_group}**
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - 成功狀態碼： 200 （確定）
  - 範例 （重寫）： **/iis-rules-rewrite / {capture_group}**至**重寫 /？ id = {capture_group}**
* `Add(RedirectXMLRequests)`
  - 成功狀態碼： 301 （已永久移動）
  - 範例 （重新導向）： **/file.xml**至**/xmlfiles/file.xml**
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - 成功狀態碼： 301 （已永久移動）
  - 範例 （重新導向）： **/image.png**至**/png-images/image.png**
  - 範例 （重新導向）： **/image.jpg**至**/jpg-images/image.jpg**

## <a name="using-a-physicalfileprovider"></a>使用`PhysicalFileProvider`
您也可以取得`IFileProvider`藉由建立`PhysicalFileProvider`傳入`AddApacheModRewrite()`和`AddIISUrlRewrite()`方法：
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a>安全的重新導向延伸模組
此範例包含`WebHostBuilder`使用 Url 的應用程式的組態 (**https://localhost:5001**， **https://localhost**) 和測試憑證 (**testCert.pfx**) 以協助您在瀏覽這些重新導向方法。 新增任何以`RewriteOptions()`中**Startup.cs**來研究它們的行為。

方法 | 狀態碼 | 連接埠
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | 301 | null (465)
`.AddRedirectToHttps()` | 302 | null (465)
`.AddRedirectToHttps(301)` | 301 | null (465)
`.AddRedirectToHttps(301, 5001)` | 301 | 5001
