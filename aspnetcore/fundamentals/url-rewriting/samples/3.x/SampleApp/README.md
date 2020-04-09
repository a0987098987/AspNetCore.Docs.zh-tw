# <a name="aspnet-core-url-rewriting-sample"></a><span data-ttu-id="ea7d6-101">ASP.NET Core URL 重寫範例</span><span class="sxs-lookup"><span data-stu-id="ea7d6-101">ASP.NET Core URL Rewriting Sample</span></span>

<span data-ttu-id="ea7d6-102">此範例說明 ASP.NET Core URL 重寫中介軟體的用法。</span><span class="sxs-lookup"><span data-stu-id="ea7d6-102">This sample illustrates usage of ASP.NET Core URL Rewriting Middleware.</span></span> <span data-ttu-id="ea7d6-103">應用程式會示範 URL 重新導向和 URL 重寫選項。</span><span class="sxs-lookup"><span data-stu-id="ea7d6-103">The app demonstrates URL redirect and URL rewriting options.</span></span>

<span data-ttu-id="ea7d6-104">執行範例時，非檔案回應會傳回當其中一個規則套用至要求 URL 時的重寫或重新導向 URL。</span><span class="sxs-lookup"><span data-stu-id="ea7d6-104">When running the sample, non-file responses return the rewritten or redirected URL when one of the rules is applied to a request URL.</span></span> <span data-ttu-id="ea7d6-105">在 XML 和文字檔範例中，靜態檔案中介軟體會在中介軟體重寫要求 URL 之後提供檔案。</span><span class="sxs-lookup"><span data-stu-id="ea7d6-105">For the XML and text file examples, Static File Middleware serves the file after the request URL is rewritten by the middleware.</span></span>

## <a name="examples-in-this-sample"></a><span data-ttu-id="ea7d6-106">這個範例中的範例</span><span class="sxs-lookup"><span data-stu-id="ea7d6-106">Examples in this sample</span></span>

* `AddRedirect("redirect-rule/(.*)", "redirected/$1")`
  - <span data-ttu-id="ea7d6-107">成功的狀態碼：302 (已找到)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-107">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="ea7d6-108">範例 (重新導向)：**/redirect-rule/{capture_group}** 至 **/redirected/{capture_group}**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-108">Example (redirect): **/redirect-rule/{capture_group}** to **/redirected/{capture_group}**</span></span>
* `AddRewrite(@"^rewrite-rule/(\d+)/(\d+)", "rewritten?var1=$1&var2=$2", skipRemainingRules: true)`
  - <span data-ttu-id="ea7d6-109">成功的狀態碼：200 (沒問題)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-109">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="ea7d6-110">範例 (重寫)：**/rewrite-rule/{capture_group_1}/{capture_group_2}** 至 **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-110">Example (rewrite): **/rewrite-rule/{capture_group_1}/{capture_group_2}** to **/rewritten?var1={capture_group_1}&var2={capture_group_2}**</span></span>
* `AddApacheModRewrite(env.ContentRootFileProvider, "ApacheModRewrite.txt")`
  - <span data-ttu-id="ea7d6-111">成功的狀態碼：302 (已找到)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-111">Success status code: 302 (Found)</span></span>
  - <span data-ttu-id="ea7d6-112">範例 (重新導向)：**/apache-mod-rules-redirect/{capture_group}** 至 **/redirected?id={capture_group}**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-112">Example (redirect): **/apache-mod-rules-redirect/{capture_group}** to **/redirected?id={capture_group}**</span></span>
* `AddIISUrlRewrite(env.ContentRootFileProvider, "IISUrlRewrite.xml")`
  - <span data-ttu-id="ea7d6-113">成功的狀態碼：200 (沒問題)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-113">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="ea7d6-114">範例 (重寫)：**/iis-rules-rewrite/{capture_group}** 至 **/rewritten?id={capture_group}**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-114">Example (rewrite): **/iis-rules-rewrite/{capture_group}** to **/rewritten?id={capture_group}**</span></span>
* `Add(RedirectXmlFileRequests)`
  - <span data-ttu-id="ea7d6-115">成功的狀態碼：301 (已永久移動)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-115">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="ea7d6-116">範例 (重新導向)：**/file.xml** 至 **/xmlfiles/file.xml**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-116">Example (redirect): **/file.xml** to **/xmlfiles/file.xml**</span></span>
* `Add(RewriteTextFileRequests)`
  - <span data-ttu-id="ea7d6-117">成功的狀態碼：200 (沒問題)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-117">Success status code: 200 (OK)</span></span>
  - <span data-ttu-id="ea7d6-118">範例 (重寫)：**/some_file.txt** 至 **/file.txt**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-118">Example (rewrite): **/some_file.txt** to **/file.txt**</span></span>
* `Add(new RedirectImageRequests(".png", "/png-images")))`<br>`Add(new RedirectImageRequests(".jpg", "/jpg-images")))`
  - <span data-ttu-id="ea7d6-119">成功的狀態碼：301 (已永久移動)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-119">Success status code: 301 (Moved Permanently)</span></span>
  - <span data-ttu-id="ea7d6-120">範例 (重新導向)：**/image.png** 至 **/png-images/image.png**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-120">Example (redirect): **/image.png** to **/png-images/image.png**</span></span>
  - <span data-ttu-id="ea7d6-121">範例 (重新導向)：**/image.jpg** 至 **/jpg-images/image.jpg**</span><span class="sxs-lookup"><span data-stu-id="ea7d6-121">Example (redirect): **/image.jpg** to **/jpg-images/image.jpg**</span></span>

## <a name="use-a-physicalfileprovider"></a><span data-ttu-id="ea7d6-122">使用 PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="ea7d6-122">Use a PhysicalFileProvider</span></span>

<span data-ttu-id="ea7d6-123">藉由建立 `PhysicalFileProvider` 以傳入 `AddApacheModRewrite()` 和 `AddIISUrlRewrite()` 方法，您也可以取得 `IFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="ea7d6-123">You can also obtain an `IFileProvider` by creating a `PhysicalFileProvider` to pass into the `AddApacheModRewrite()` and `AddIISUrlRewrite()` methods:</span></span>

```csharp
using Microsoft.Extensions.FileProviders;
PhysicalFileProvider fileProvider = new PhysicalFileProvider(Directory.GetCurrentDirectory());
```

## <a name="secure-redirection-extensions"></a><span data-ttu-id="ea7d6-124">安全的重新導向延伸模組</span><span class="sxs-lookup"><span data-stu-id="ea7d6-124">Secure redirection extensions</span></span>

<span data-ttu-id="ea7d6-125">此範例包含應用程式的 `WebHostBuilder` 組態，以便使用 URL (`https://localhost:5001`、`https://localhost`) 和測試憑證 (*testCert.pfx*) 來協助瀏覽這些安全的重新導向方法。</span><span class="sxs-lookup"><span data-stu-id="ea7d6-125">This sample includes `WebHostBuilder` configuration for the app to use URLs (`https://localhost:5001`, `https://localhost`) and a test certificate (*testCert.pfx*) to assist in exploring the secure redirect methods.</span></span> <span data-ttu-id="ea7d6-126">如果伺服器已指派或正在使用連接埠 443，則 `https://localhost` 範例不適用&mdash;請從 *Program.cs* 檔案的 `CreateWebHostBuilder` 方法中移除連接埠 443 的 `ListenOptions`，或解除繫結伺服器上的連接埠 443，讓 Kestrel 可以使用該連接埠。</span><span class="sxs-lookup"><span data-stu-id="ea7d6-126">If the server already has port 443 assigned or in use, the `https://localhost` example doesn't work&mdash;remove the `ListenOptions` for port 443 in the `CreateWebHostBuilder` method of the *Program.cs* file or unbind port 443 on the server so that Kestrel can use the port.</span></span>

| <span data-ttu-id="ea7d6-127">方法</span><span class="sxs-lookup"><span data-stu-id="ea7d6-127">Method</span></span>                           | <span data-ttu-id="ea7d6-128">狀態碼</span><span class="sxs-lookup"><span data-stu-id="ea7d6-128">Status Code</span></span> |    <span data-ttu-id="ea7d6-129">連接埠</span><span class="sxs-lookup"><span data-stu-id="ea7d6-129">Port</span></span>    |
| -------------------------------- | :---------: | :--------: |
| `.AddRedirectToHttpsPermanent()` |     <span data-ttu-id="ea7d6-130">301</span><span class="sxs-lookup"><span data-stu-id="ea7d6-130">301</span></span>     | <span data-ttu-id="ea7d6-131">null (465)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-131">null (465)</span></span> |
| `.AddRedirectToHttps()`          |     <span data-ttu-id="ea7d6-132">302</span><span class="sxs-lookup"><span data-stu-id="ea7d6-132">302</span></span>     | <span data-ttu-id="ea7d6-133">null (465)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-133">null (465)</span></span> |
| `.AddRedirectToHttps(301)`       |     <span data-ttu-id="ea7d6-134">301</span><span class="sxs-lookup"><span data-stu-id="ea7d6-134">301</span></span>     | <span data-ttu-id="ea7d6-135">null (465)</span><span class="sxs-lookup"><span data-stu-id="ea7d6-135">null (465)</span></span> |
| `.AddRedirectToHttps(301, 5001)` |     <span data-ttu-id="ea7d6-136">301</span><span class="sxs-lookup"><span data-stu-id="ea7d6-136">301</span></span>     |    <span data-ttu-id="ea7d6-137">5001</span><span class="sxs-lookup"><span data-stu-id="ea7d6-137">5001</span></span>    |
