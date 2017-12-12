# <a name="aspnet-core-url-rewriting-sample-aspnet-core-1x"></a><span data-ttu-id="9b4b3-101">ASP.NET Core URL 重寫範例 (ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="9b4b3-101">ASP.NET Core URL Rewriting Sample (ASP.NET Core 1.x)</span></span>

<span data-ttu-id="9b4b3-102">此範例說明使用 ASP.NET Core 1.x URL 重寫中介軟體。</span><span class="sxs-lookup"><span data-stu-id="9b4b3-102">This sample illustrates usage of ASP.NET Core 1.x URL Rewriting Middleware.</span></span> <span data-ttu-id="9b4b3-103">應用程式示範重新導向 URL 和 URL 重寫選項。</span><span class="sxs-lookup"><span data-stu-id="9b4b3-103">The application demonstrates URL redirect and URL rewriting options.</span></span> <span data-ttu-id="9b4b3-104">如需 ASP.NET Core 2.x 範例中，請參閱[ASP.NET Core URL 重寫範例 (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x)。</span><span class="sxs-lookup"><span data-stu-id="9b4b3-104">For the ASP.NET Core 2.x sample, see [ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/2.x).</span></span>

<span data-ttu-id="9b4b3-105">執行範例時，會處理回應會顯示其中一個規則套用至要求的 URL 時的重寫或重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="9b4b3-105">When running the sample, a response will be served that shows the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="9b4b3-106">在此範例中的範例</span><span class="sxs-lookup"><span data-stu-id="9b4b3-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "$1")`
  - <span data-ttu-id="9b4b3-107">成功狀態碼： 302 （找到）</span><span class="sxs-lookup"><span data-stu-id="9b4b3-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="9b4b3-108">範例 （重新導向）： **/redirect-rule / {capture_group}**至**/redirected/ {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="9b4b3-109">成功狀態碼： 200 （確定）</span><span class="sxs-lookup"><span data-stu-id="9b4b3-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="9b4b3-110">範例 （重寫）： **/rewrite-rule / {capture_group_1} / {capture_group_2}**至**重寫 /？ var1 = {capture_group_1} （& s) var2 = {capture_group_2}**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="9b4b3-111">成功狀態碼： 302 （找到）</span><span class="sxs-lookup"><span data-stu-id="9b4b3-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="9b4b3-112">範例 （重新導向）： **/apache-mod-rules-redirect / {capture_group}**至**/ 重新導向？ id = {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="9b4b3-113">成功狀態碼： 200 （確定）</span><span class="sxs-lookup"><span data-stu-id="9b4b3-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="9b4b3-114">範例 （重寫）： **/iis-rules-rewrite / {capture_group}**至**重寫 /？ id = {capture_group}**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXMLRequests)`
  - <span data-ttu-id="9b4b3-115">成功狀態碼： 301 （已永久移動）</span><span class="sxs-lookup"><span data-stu-id="9b4b3-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="9b4b3-116">範例 （重新導向）： **/file.xml**至**/xmlfiles/file.xml**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="9b4b3-117">成功狀態碼： 301 （已永久移動）</span><span class="sxs-lookup"><span data-stu-id="9b4b3-117">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="9b4b3-118">範例 （重新導向）： **/image.png**至**/png-images/image.png**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-118">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="9b4b3-119">範例 （重新導向）： **/image.jpg**至**/jpg-images/image.jpg**</span><span class="sxs-lookup"><span data-stu-id="9b4b3-119">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="using-a-physicalfileprovider"></a><span data-ttu-id="9b4b3-120">使用`PhysicalFileProvider`</span><span class="sxs-lookup"><span data-stu-id="9b4b3-120">Using a `PhysicalFileProvider`</span></span>
<span data-ttu-id="9b4b3-121">您也可以取得`IFileProvider`藉由建立`PhysicalFileProvider`傳入`AddApacheModRewrite()`和`AddIISUrlRewrite()`方法：</span><span class="sxs-lookup"><span data-stu-id="9b4b3-121">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a><span data-ttu-id="9b4b3-122">安全的重新導向延伸模組</span><span class="sxs-lookup"><span data-stu-id="9b4b3-122">Secure redirection extensions</span></span>
<span data-ttu-id="9b4b3-123">此範例包含`WebHostBuilder`使用 Url 的應用程式的組態 (**https://localhost:5001**， **https://localhost**) 和測試憑證 (**testCert.pfx**) 以協助您在瀏覽這些重新導向方法。</span><span class="sxs-lookup"><span data-stu-id="9b4b3-123">This sample includes `WebHostBuilder` configuration for the app to use URLs (**https://localhost:5001**, **https://localhost**) and a test certificate (**testCert.pfx**) to assist you in exploring these redirect methods.</span></span> <span data-ttu-id="9b4b3-124">新增任何以`RewriteOptions()`中**Startup.cs**來研究它們的行為。</span><span class="sxs-lookup"><span data-stu-id="9b4b3-124">Add any of them to the `RewriteOptions()` in **Startup.cs** to study their behavior.</span></span>

<span data-ttu-id="9b4b3-125">方法</span><span class="sxs-lookup"><span data-stu-id="9b4b3-125">Method</span></span> | <span data-ttu-id="9b4b3-126">狀態碼</span><span class="sxs-lookup"><span data-stu-id="9b4b3-126">Status Code</span></span> | <span data-ttu-id="9b4b3-127">連接埠</span><span class="sxs-lookup"><span data-stu-id="9b4b3-127">Port</span></span>
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | <span data-ttu-id="9b4b3-128">301</span><span class="sxs-lookup"><span data-stu-id="9b4b3-128">301</span></span> | <span data-ttu-id="9b4b3-129">null (465)</span><span class="sxs-lookup"><span data-stu-id="9b4b3-129">null (465)</span></span>
`.AddRedirectToHttps()` | <span data-ttu-id="9b4b3-130">302</span><span class="sxs-lookup"><span data-stu-id="9b4b3-130">302</span></span> | <span data-ttu-id="9b4b3-131">null (465)</span><span class="sxs-lookup"><span data-stu-id="9b4b3-131">null (465)</span></span>
`.AddRedirectToHttps(301)` | <span data-ttu-id="9b4b3-132">301</span><span class="sxs-lookup"><span data-stu-id="9b4b3-132">301</span></span> | <span data-ttu-id="9b4b3-133">null (465)</span><span class="sxs-lookup"><span data-stu-id="9b4b3-133">null (465)</span></span>
`.AddRedirectToHttps(301, 5001)` | <span data-ttu-id="9b4b3-134">301</span><span class="sxs-lookup"><span data-stu-id="9b4b3-134">301</span></span> | <span data-ttu-id="9b4b3-135">5001</span><span class="sxs-lookup"><span data-stu-id="9b4b3-135">5001</span></span>
