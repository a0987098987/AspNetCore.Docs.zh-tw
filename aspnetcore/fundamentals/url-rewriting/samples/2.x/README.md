# <a name="aspnet-core-url-rewriting-sample-aspnet-core-2x"></a><span data-ttu-id="a44f8-101">ASP.NET Core URL 重寫範例 (ASP.NET Core 2.x)</span><span class="sxs-lookup"><span data-stu-id="a44f8-101">ASP.NET Core URL Rewriting Sample (ASP.NET Core 2.x)</span></span>

<span data-ttu-id="a44f8-102">此範例說明 ASP.NET Core 2.x URL 重寫中介軟體的用法。</span><span class="sxs-lookup"><span data-stu-id="a44f8-102">This sample illustrates usage of ASP.NET Core 2.x URL Rewriting Middleware.</span></span> <span data-ttu-id="a44f8-103">應用程式會示範 URL 重新導向和 URL 重寫選項。</span><span class="sxs-lookup"><span data-stu-id="a44f8-103">The application demonstrates URL redirect and URL rewriting options.</span></span> <span data-ttu-id="a44f8-104">如需 ASP.NET Core 1.x 範例，請參閱 [ASP.NET Core URL 重寫範例 (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x)。</span><span class="sxs-lookup"><span data-stu-id="a44f8-104">For the ASP.NET Core 1.x sample, see [ASP.NET Core URL Rewriting Sample (ASP.NET Core 1.x)](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/url-rewriting/samples/1.x).</span></span>

<span data-ttu-id="a44f8-105">執行範例時，將會提供回應，以顯示當其中一個規則套用至要求 URL 時的重寫或重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="a44f8-105">When running the sample, a response will be served that shows the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="a44f8-106">這個範例中的範例</span><span class="sxs-lookup"><span data-stu-id="a44f8-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - <span data-ttu-id="a44f8-107">成功的狀態碼：302 (已找到)</span><span class="sxs-lookup"><span data-stu-id="a44f8-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="a44f8-108">範例 (重新導向)：**/redirect-rule/{capture_group}** 至 **/redirected/{capture_group}**</span><span class="sxs-lookup"><span data-stu-id="a44f8-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="a44f8-109">成功的狀態碼：200 (沒問題)</span><span class="sxs-lookup"><span data-stu-id="a44f8-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="a44f8-110">範例 (重寫)：**/rewrite-rule/{capture_group_1}/{capture_group_2}** 至 **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span><span class="sxs-lookup"><span data-stu-id="a44f8-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="a44f8-111">成功的狀態碼：302 (已找到)</span><span class="sxs-lookup"><span data-stu-id="a44f8-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="a44f8-112">範例 (重新導向)：**/apache-mod-rules-redirect/{capture_group}** 至 **/redirected?id={capture_group}**</span><span class="sxs-lookup"><span data-stu-id="a44f8-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="a44f8-113">成功的狀態碼：200 (沒問題)</span><span class="sxs-lookup"><span data-stu-id="a44f8-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="a44f8-114">範例 (重寫)：**/iis-rules-rewrite/{capture_group}** 至 **/rewritten?id={capture_group}**</span><span class="sxs-lookup"><span data-stu-id="a44f8-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXMLRequests)`
  - <span data-ttu-id="a44f8-115">成功的狀態碼：301 (已永久移動)</span><span class="sxs-lookup"><span data-stu-id="a44f8-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="a44f8-116">範例 (重新導向)：**/file.xml** 至 **/xmlfiles/file.xml**</span><span class="sxs-lookup"><span data-stu-id="a44f8-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(new RedirectPNGRequests(".png", "/png-images")))`<br>`Add(new RedirectPNGRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="a44f8-117">成功的狀態碼：301 (已永久移動)</span><span class="sxs-lookup"><span data-stu-id="a44f8-117">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="a44f8-118">範例 (重新導向)：**/image.png** 至 **/png-images/image.png**</span><span class="sxs-lookup"><span data-stu-id="a44f8-118">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="a44f8-119">範例 (重新導向)：**/image.jpg** 至 **/jpg-images/image.jpg**</span><span class="sxs-lookup"><span data-stu-id="a44f8-119">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="using-a-physicalfileprovider"></a><span data-ttu-id="a44f8-120">使用 `PhysicalFileProvider`</span><span class="sxs-lookup"><span data-stu-id="a44f8-120">Using a `PhysicalFileProvider`</span></span>
<span data-ttu-id="a44f8-121">藉由建立 `PhysicalFileProvider` 以傳入 `AddApacheModRewrite()` 和 `AddIISUrlRewrite()` 方法，您也可以取得 `IFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="a44f8-121">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>
```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```
## <a name="secure-redirection-extensions"></a><span data-ttu-id="a44f8-122">安全的重新導向延伸模組</span><span class="sxs-lookup"><span data-stu-id="a44f8-122">Secure redirection extensions</span></span>
<span data-ttu-id="a44f8-123">此範例包含應用程式的 `WebHostBuilder` 組態，以便使用 URL (**https://localhost:5001**、**https://localhost**) 和測試憑證 (**testCert.pfx**) 來協助您瀏覽這些重新導向方法。</span><span class="sxs-lookup"><span data-stu-id="a44f8-123">This sample includes `WebHostBuilder` configuration for the app to use URLs (**https://localhost:5001**, **https://localhost**) and a test certificate (**testCert.pfx**) to assist you in exploring these redirect methods.</span></span> <span data-ttu-id="a44f8-124">請將其中任一項新增至 **Startup.cs** 中的 `RewriteOptions()` 以研究其行為。</span><span class="sxs-lookup"><span data-stu-id="a44f8-124">Add any of them to the `RewriteOptions()` in **Startup.cs** to study their behavior.</span></span>

<span data-ttu-id="a44f8-125">方法</span><span class="sxs-lookup"><span data-stu-id="a44f8-125">Method</span></span> | <span data-ttu-id="a44f8-126">狀態碼</span><span class="sxs-lookup"><span data-stu-id="a44f8-126">Status Code</span></span> | <span data-ttu-id="a44f8-127">連接埠</span><span class="sxs-lookup"><span data-stu-id="a44f8-127">Port</span></span>
--- | :---: | :---:
`.AddRedirectToHttpsPermanent()` | <span data-ttu-id="a44f8-128">301</span><span class="sxs-lookup"><span data-stu-id="a44f8-128">301</span></span> | <span data-ttu-id="a44f8-129">null (465)</span><span class="sxs-lookup"><span data-stu-id="a44f8-129">null (465)</span></span>
`.AddRedirectToHttps()` | <span data-ttu-id="a44f8-130">302</span><span class="sxs-lookup"><span data-stu-id="a44f8-130">302</span></span> | <span data-ttu-id="a44f8-131">null (465)</span><span class="sxs-lookup"><span data-stu-id="a44f8-131">null (465)</span></span>
`.AddRedirectToHttps(301)` | <span data-ttu-id="a44f8-132">301</span><span class="sxs-lookup"><span data-stu-id="a44f8-132">301</span></span> | <span data-ttu-id="a44f8-133">null (465)</span><span class="sxs-lookup"><span data-stu-id="a44f8-133">null (465)</span></span>
`.AddRedirectToHttps(301, 5001)` | <span data-ttu-id="a44f8-134">301</span><span class="sxs-lookup"><span data-stu-id="a44f8-134">301</span></span> | <span data-ttu-id="a44f8-135">5001</span><span class="sxs-lookup"><span data-stu-id="a44f8-135">5001</span></span>
