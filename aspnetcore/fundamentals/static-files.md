---
title: "使用 ASP.NET Core 中的靜態檔案"
author: rick-anderson
description: "使用靜態檔案"
keywords: "ASP.NET Core 靜態檔案、 HTML、 CSS、 JavaScript 靜態資產"
ms.author: riande
manager: wpickett
ms.date: 4/07/2017
ms.topic: article
ms.assetid: e32245c7-4eee-4831-bd2e-915dbf9f5f70
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/static-files
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ea6c180332dd5ab3a7238dcd73a4a1c8534c6243
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-working-with-static-files-in-aspnet-core"></a><span data-ttu-id="d310d-104">簡介使用 ASP.NET Core 中的靜態檔案</span><span class="sxs-lookup"><span data-stu-id="d310d-104">Introduction to working with static files in ASP.NET Core</span></span>

<a name=fundamentals-static-files></a>

<span data-ttu-id="d310d-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d310d-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d310d-106">靜態檔案，例如 HTML、 CSS、 影像和 JavaScript 中，是 ASP.NET Core 應用程式可以直接用戶端提供服務的資產。</span><span class="sxs-lookup"><span data-stu-id="d310d-106">Static files, such as HTML, CSS, image, and JavaScript, are assets that an ASP.NET Core app can serve directly to clients.</span></span>

[<span data-ttu-id="d310d-107">檢視或下載範例程式碼</span><span class="sxs-lookup"><span data-stu-id="d310d-107">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/sample)

## <a name="serving-static-files"></a><span data-ttu-id="d310d-108">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="d310d-108">Serving static files</span></span>

<span data-ttu-id="d310d-109">靜態檔案通常位於`web root`(*\<內容根目錄 > / wwwroot*) 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d310d-109">Static files are typically located in the `web root` (*\<content-root>/wwwroot*) folder.</span></span> <span data-ttu-id="d310d-110">請參閱[內容的根](xref:fundamentals/index#content-root)和[Web 根目錄](xref:fundamentals/index#web-root)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="d310d-110">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span> <span data-ttu-id="d310d-111">您通常會設定為目前的目錄內容的根，讓您的專案`web root`會在開發中找到。</span><span class="sxs-lookup"><span data-stu-id="d310d-111">You generally set the content root to be the current directory so that your project's `web root` will be found while in development.</span></span>

<span data-ttu-id="d310d-112">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]</span><span class="sxs-lookup"><span data-stu-id="d310d-112">[!code-csharp[Main](../common/samples/WebApplication1/Program.cs?highlight=5&start=12&end=22)]</span></span>

<span data-ttu-id="d310d-113">靜態檔案可以儲存在底下的任何資料夾`web root`，存取與該根的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="d310d-113">Static files can be stored in any folder under the `web root` and accessed with a relative path to that root.</span></span> <span data-ttu-id="d310d-114">例如，當您建立使用 Visual Studio 的預設 Web 應用程式專案時，有數個資料夾內建立*wwwroot*資料夾- *css*，*映像*，和*js*。</span><span class="sxs-lookup"><span data-stu-id="d310d-114">For example, when you create a default Web application project using Visual Studio, there are several folders created within the *wwwroot*  folder - *css*, *images*, and *js*.</span></span> <span data-ttu-id="d310d-115">可存取中的映像的 URI*映像*子資料夾：</span><span class="sxs-lookup"><span data-stu-id="d310d-115">The URI to access an image in the *images* subfolder:</span></span>

* `http://<app>/images/<imageFileName>`
* `http://localhost:9189/images/banner3.svg`

<span data-ttu-id="d310d-116">為了讓服務靜態檔案，您必須設定[中介軟體](middleware.md)將靜態檔案新增至管線。</span><span class="sxs-lookup"><span data-stu-id="d310d-116">In order for static files to be served, you must configure the [Middleware](middleware.md) to add static files to the pipeline.</span></span> <span data-ttu-id="d310d-117">您可以設定的靜態檔案中介軟體上新增相依性*Microsoft.AspNetCore.StaticFiles*封裝加入您的專案，並再呼叫`UseStaticFiles`擴充方法，從`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="d310d-117">The static file middleware can be configured by adding a dependency on the *Microsoft.AspNetCore.StaticFiles* package to your project and then calling the `UseStaticFiles` extension method from `Startup.Configure`:</span></span>

<span data-ttu-id="d310d-118">[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-118">[!code-csharp[Main](../fundamentals/static-files/sample/StartupStaticFiles.cs?highlight=3&name=snippet1)]</span></span>

<span data-ttu-id="d310d-119">`app.UseStaticFiles();`中的檔案可讓`web root`(*wwwroot*依預設) servable。</span><span class="sxs-lookup"><span data-stu-id="d310d-119">`app.UseStaticFiles();` makes the files in `web root` (*wwwroot* by default) servable.</span></span> <span data-ttu-id="d310d-120">稍後，我將示範如何讓其他目錄的內容與 servable `UseStaticFiles`。</span><span class="sxs-lookup"><span data-stu-id="d310d-120">Later I'll show how to make other directory contents servable with `UseStaticFiles`.</span></span>

<span data-ttu-id="d310d-121">您必須包含 NuGet 封裝 「 Microsoft.AspNetCore.StaticFiles"。</span><span class="sxs-lookup"><span data-stu-id="d310d-121">You must include the NuGet package "Microsoft.AspNetCore.StaticFiles".</span></span>

> [!NOTE]
> <span data-ttu-id="d310d-122">`web root`預設為*wwwroot*目錄中，但是您可以設定`web root`目錄用於`UseWebRoot`。</span><span class="sxs-lookup"><span data-stu-id="d310d-122">`web root` defaults to the *wwwroot* directory, but you can set the `web root` directory with `UseWebRoot`.</span></span>

<span data-ttu-id="d310d-123">假設您有靜態檔案，您想要做的外部專案階層架構`web root`。</span><span class="sxs-lookup"><span data-stu-id="d310d-123">Suppose you have a project hierarchy where the static files you wish to serve are outside the `web root`.</span></span> <span data-ttu-id="d310d-124">例如: </span><span class="sxs-lookup"><span data-stu-id="d310d-124">For example:</span></span>

* <span data-ttu-id="d310d-125">wwwroot</span><span class="sxs-lookup"><span data-stu-id="d310d-125">wwwroot</span></span>
  * <span data-ttu-id="d310d-126">css</span><span class="sxs-lookup"><span data-stu-id="d310d-126">css</span></span>
  * <span data-ttu-id="d310d-127">影像</span><span class="sxs-lookup"><span data-stu-id="d310d-127">images</span></span>
  * <span data-ttu-id="d310d-128">...</span><span class="sxs-lookup"><span data-stu-id="d310d-128">...</span></span>
* <span data-ttu-id="d310d-129">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="d310d-129">MyStaticFiles</span></span>
  * <span data-ttu-id="d310d-130">test.png</span><span class="sxs-lookup"><span data-stu-id="d310d-130">test.png</span></span>

<span data-ttu-id="d310d-131">若要存取要求*test.png*，設定靜態檔案中介軟體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d310d-131">For a request to access *test.png*, configure the static files middleware as follows:</span></span>

<span data-ttu-id="d310d-132">[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-132">[!code-csharp[Main](../fundamentals/static-files/sample/StartupTwoStaticFiles.cs?highlight=5,6,7,8,9,10&name=snippet1)]</span></span>

<span data-ttu-id="d310d-133">若要要求`http://<app>/StaticFiles/test.png`做*test.png*檔案。</span><span class="sxs-lookup"><span data-stu-id="d310d-133">A request to `http://<app>/StaticFiles/test.png` will serve the *test.png* file.</span></span>

<span data-ttu-id="d310d-134">`StaticFileOptions()`可以設定回應標頭。</span><span class="sxs-lookup"><span data-stu-id="d310d-134">`StaticFileOptions()` can set response headers.</span></span> <span data-ttu-id="d310d-135">例如，下列程式碼會設定靜態檔案提供從*wwwroot*資料夾，然後設定`Cache-Control`標頭，讓它們可公開可快取的 10 分鐘 （600 秒）：</span><span class="sxs-lookup"><span data-stu-id="d310d-135">For example, the code below sets up static file serving from the *wwwroot* folder and sets the `Cache-Control` header to make them publicly cacheable for 10 minutes (600 seconds):</span></span>

<span data-ttu-id="d310d-136">[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-136">[!code-csharp[Main](../fundamentals/static-files/sample/StartupAddHeader.cs?name=snippet1)]</span></span>

![已加入顯示的快取控制標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="d310d-138">靜態檔案授權</span><span class="sxs-lookup"><span data-stu-id="d310d-138">Static file authorization</span></span>

<span data-ttu-id="d310d-139">提供靜態檔案模組**沒有**授權檢查。</span><span class="sxs-lookup"><span data-stu-id="d310d-139">The static file module provides **no** authorization checks.</span></span> <span data-ttu-id="d310d-140">任何檔案由提供服務，包括下*wwwroot*公開使用。</span><span class="sxs-lookup"><span data-stu-id="d310d-140">Any files served by it, including those under *wwwroot* are publicly available.</span></span> <span data-ttu-id="d310d-141">若要提供檔案為基礎的授權：</span><span class="sxs-lookup"><span data-stu-id="d310d-141">To serve files based on authorization:</span></span>

* <span data-ttu-id="d310d-142">儲存它們的外部*wwwroot*和可存取靜態檔案中介軟體的任何目錄**和**</span><span class="sxs-lookup"><span data-stu-id="d310d-142">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>

* <span data-ttu-id="d310d-143">提供控制器動作，傳回透過`FileResult`套用授權的位置</span><span class="sxs-lookup"><span data-stu-id="d310d-143">Serve them through a controller action, returning a `FileResult` where authorization is applied</span></span>

## <a name="enabling-directory-browsing"></a><span data-ttu-id="d310d-144">啟用目錄瀏覽</span><span class="sxs-lookup"><span data-stu-id="d310d-144">Enabling directory browsing</span></span>

<span data-ttu-id="d310d-145">瀏覽目錄可讓您 web 應用程式的使用者，若要查看目錄和檔案中指定的目錄清單。</span><span class="sxs-lookup"><span data-stu-id="d310d-145">Directory browsing allows the user of your web app to see a list of directories and files within a specified directory.</span></span> <span data-ttu-id="d310d-146">瀏覽目錄已停用預設基於安全性考量 (請參閱[考量](#considerations))。</span><span class="sxs-lookup"><span data-stu-id="d310d-146">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="d310d-147">若要啟用瀏覽目錄，請呼叫`UseDirectoryBrowser`擴充方法，從`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="d310d-147">To enable directory browsing, call the `UseDirectoryBrowser` extension method from  `Startup.Configure`:</span></span>

<span data-ttu-id="d310d-148">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-148">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet1)]</span></span>

<span data-ttu-id="d310d-149">並加入所需的服務，藉由呼叫`AddDirectoryBrowser`擴充方法，從`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d310d-149">And add required services by calling `AddDirectoryBrowser` extension method from `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="d310d-150">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="d310d-150">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?name=snippet2)]</span></span>

<span data-ttu-id="d310d-151">上述程式碼讓您瀏覽目錄*wwwroot/影像*資料夾使用 URL http://\<應用程式 > / MyImages，每個檔案和資料夾的連結：</span><span class="sxs-lookup"><span data-stu-id="d310d-151">The code above allows directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages, with links to each file and folder:</span></span>

![瀏覽目錄](static-files/_static/dir-browse.png)

<span data-ttu-id="d310d-153">請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。</span><span class="sxs-lookup"><span data-stu-id="d310d-153">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="d310d-154">請注意兩個`app.UseStaticFiles`呼叫。</span><span class="sxs-lookup"><span data-stu-id="d310d-154">Note the two `app.UseStaticFiles` calls.</span></span> <span data-ttu-id="d310d-155">第一個，才能提供 CSS、 影像及中的 JavaScript *wwwroot*資料夾，然後瀏覽目錄的第二個呼叫*wwwroot/影像*資料夾使用 URL http://\<應用程式> / MyImages:</span><span class="sxs-lookup"><span data-stu-id="d310d-155">The first one is required to serve the CSS, images and JavaScript in the *wwwroot* folder, and the second call for directory browsing of the *wwwroot/images* folder using the URL http://\<app>/MyImages:</span></span>

<span data-ttu-id="d310d-156">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-156">[!code-csharp[Main](static-files/sample/StartupBrowse.cs?highlight=3,5&name=snippet1)]</span></span>

## <a name="serving-a-default-document"></a><span data-ttu-id="d310d-157">服務的預設文件</span><span class="sxs-lookup"><span data-stu-id="d310d-157">Serving a default document</span></span>

<span data-ttu-id="d310d-158">設定預設的首頁提供站台訪客開始瀏覽您的網站時的起點。</span><span class="sxs-lookup"><span data-stu-id="d310d-158">Setting a default home page gives site visitors a place to start when visiting your site.</span></span> <span data-ttu-id="d310d-159">為了讓您不必完整限定的 URI，使用者不提供預設頁面的 Web 應用程式，請呼叫`UseDefaultFiles`擴充方法，從`Startup.Configure`，如下所示。</span><span class="sxs-lookup"><span data-stu-id="d310d-159">In order for your Web app to serve a default page without the user having to fully qualify the URI, call the `UseDefaultFiles` extension method from `Startup.Configure` as follows.</span></span>

<span data-ttu-id="d310d-160">[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-160">[!code-csharp[Main](../fundamentals/static-files/sample/StartupEmpty.cs?highlight=3&name=snippet1)]</span></span>

> [!NOTE]
> <span data-ttu-id="d310d-161">`UseDefaultFiles`之前必須先呼叫`UseStaticFiles`提供預設的檔案。</span><span class="sxs-lookup"><span data-stu-id="d310d-161">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="d310d-162">`UseDefaultFiles`是實際上並不提供此檔案的 URL 重新寫入器。</span><span class="sxs-lookup"><span data-stu-id="d310d-162">`UseDefaultFiles` is a URL re-writer that doesn't actually serve the file.</span></span> <span data-ttu-id="d310d-163">您必須啟用靜態檔案中介軟體 (`UseStaticFiles`) 來提供此檔案。</span><span class="sxs-lookup"><span data-stu-id="d310d-163">You must enable the static file middleware (`UseStaticFiles`) to serve the file.</span></span>

<span data-ttu-id="d310d-164">與`UseDefaultFiles`，就會搜尋要求的資料夾：</span><span class="sxs-lookup"><span data-stu-id="d310d-164">With `UseDefaultFiles`, requests to a folder will search for:</span></span>

* <span data-ttu-id="d310d-165">default.htm</span><span class="sxs-lookup"><span data-stu-id="d310d-165">default.htm</span></span>
* <span data-ttu-id="d310d-166">default.html</span><span class="sxs-lookup"><span data-stu-id="d310d-166">default.html</span></span>
* <span data-ttu-id="d310d-167">index.htm</span><span class="sxs-lookup"><span data-stu-id="d310d-167">index.htm</span></span>
* <span data-ttu-id="d310d-168">index.html</span><span class="sxs-lookup"><span data-stu-id="d310d-168">index.html</span></span>

<span data-ttu-id="d310d-169">將服務從清單中找到的第一個檔案，因為如果要求的完整格式的 URI （雖然在瀏覽器 URL 將會繼續顯示要求的 URI）。</span><span class="sxs-lookup"><span data-stu-id="d310d-169">The first file found from the list will be served as if the request was the fully qualified URI (although the browser URL will continue to show the URI requested).</span></span>

<span data-ttu-id="d310d-170">下列程式碼示範如何變更預設檔案名稱以*mydefault.html*。</span><span class="sxs-lookup"><span data-stu-id="d310d-170">The following code shows how to change the default file name to *mydefault.html*.</span></span>

<span data-ttu-id="d310d-171">[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-171">[!code-csharp[Main](static-files/sample/StartupDefault.cs?name=snippet1)]</span></span>

## <a name="usefileserver"></a><span data-ttu-id="d310d-172">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="d310d-172">UseFileServer</span></span>

<span data-ttu-id="d310d-173">`UseFileServer`結合的功能`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`。</span><span class="sxs-lookup"><span data-stu-id="d310d-173">`UseFileServer` combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="d310d-174">下列程式碼可讓靜態檔案和預設檔案，以提供，但不允許瀏覽目錄：</span><span class="sxs-lookup"><span data-stu-id="d310d-174">The following code enables static files and the default file to be served, but does not allow directory browsing:</span></span>

```csharp
app.UseFileServer();
   ```

<span data-ttu-id="d310d-175">下列程式碼可讓靜態檔案中，預設檔案和目錄瀏覽：</span><span class="sxs-lookup"><span data-stu-id="d310d-175">The following code enables static files, default files and  directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
   ```

<span data-ttu-id="d310d-176">請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。</span><span class="sxs-lookup"><span data-stu-id="d310d-176">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span> <span data-ttu-id="d310d-177">如同`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`，如果您想要提供檔存在於之外的`web root`，具現化，並設定`FileServerOptions`物件當做參數傳遞`UseFileServer`。</span><span class="sxs-lookup"><span data-stu-id="d310d-177">As with `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`, if you wish to serve files that exist outside the `web root`, you instantiate and configure an `FileServerOptions` object that you pass as a parameter to `UseFileServer`.</span></span> <span data-ttu-id="d310d-178">例如，假設下列目錄階層中您 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="d310d-178">For example, given the following directory hierarchy in your Web app:</span></span>

* <span data-ttu-id="d310d-179">wwwroot</span><span class="sxs-lookup"><span data-stu-id="d310d-179">wwwroot</span></span>

  * <span data-ttu-id="d310d-180">css</span><span class="sxs-lookup"><span data-stu-id="d310d-180">css</span></span>

  * <span data-ttu-id="d310d-181">影像</span><span class="sxs-lookup"><span data-stu-id="d310d-181">images</span></span>

  * <span data-ttu-id="d310d-182">...</span><span class="sxs-lookup"><span data-stu-id="d310d-182">...</span></span>

* <span data-ttu-id="d310d-183">MyStaticFiles</span><span class="sxs-lookup"><span data-stu-id="d310d-183">MyStaticFiles</span></span>

  * <span data-ttu-id="d310d-184">test.png</span><span class="sxs-lookup"><span data-stu-id="d310d-184">test.png</span></span>

  * <span data-ttu-id="d310d-185">default.html</span><span class="sxs-lookup"><span data-stu-id="d310d-185">default.html</span></span>

<span data-ttu-id="d310d-186">使用階層上述範例中，您可能想要啟用靜態檔案、 預設檔案，以及瀏覽`MyStaticFiles`目錄。</span><span class="sxs-lookup"><span data-stu-id="d310d-186">Using the hierarchy example above, you might want to enable static files, default files, and browsing for the `MyStaticFiles` directory.</span></span> <span data-ttu-id="d310d-187">在下列程式碼片段中，達成透過單一呼叫`FileServerOptions`。</span><span class="sxs-lookup"><span data-stu-id="d310d-187">In the following code snippet, that is accomplished with a single call to `FileServerOptions`.</span></span>

<span data-ttu-id="d310d-188">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-188">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?highlight=5,6,7,8,9,10,11&name=snippet1)]</span></span>

<span data-ttu-id="d310d-189">如果`enableDirectoryBrowsing`設`true`您才能呼叫`AddDirectoryBrowser`擴充方法，從`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d310d-189">If `enableDirectoryBrowsing` is set to `true` you are required to call `AddDirectoryBrowser` extension method from  `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="d310d-190">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="d310d-190">[!code-csharp[Main](static-files/sample/StartupUseFileServer.cs?name=snippet2)]</span></span>

<span data-ttu-id="d310d-191">使用的檔案階層和上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="d310d-191">Using the file hierarchy and code above:</span></span>

| <span data-ttu-id="d310d-192">URI</span><span class="sxs-lookup"><span data-stu-id="d310d-192">URI</span></span>            |                             <span data-ttu-id="d310d-193">回應</span><span class="sxs-lookup"><span data-stu-id="d310d-193">Response</span></span>  |
| ------- | ------|
| `http://<app>/StaticFiles/test.png`    |      <span data-ttu-id="d310d-194">MyStaticFiles/test.png</span><span class="sxs-lookup"><span data-stu-id="d310d-194">MyStaticFiles/test.png</span></span> |
| `http://<app>/StaticFiles`              |     <span data-ttu-id="d310d-195">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="d310d-195">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="d310d-196">如果沒有名為檔案的預設值是在*MyStaticFiles*目錄、 http://\<應用程式 > / StaticFiles 的目錄清單的可點按的連結：</span><span class="sxs-lookup"><span data-stu-id="d310d-196">If no default named files are in the *MyStaticFiles* directory, http://\<app>/StaticFiles returns the directory listing with clickable links:</span></span>

![靜態檔案清單](static-files/_static/db2.PNG)

> [!NOTE]
> <span data-ttu-id="d310d-198">`UseDefaultFiles`和`UseDirectoryBrowser`需要 url http://\<應用程式 >，沒有尾端斜線及原因 StaticFiles 用戶端重新導向至 http:// /\<應用程式 > /StaticFiles/ （新增結尾的斜線）。</span><span class="sxs-lookup"><span data-stu-id="d310d-198">`UseDefaultFiles` and `UseDirectoryBrowser` will take the url http://\<app>/StaticFiles without the trailing slash and cause a client side redirect to http://\<app>/StaticFiles/ (adding the trailing slash).</span></span> <span data-ttu-id="d310d-199">沒有內文件結尾的斜線相對 Url 會不正確。</span><span class="sxs-lookup"><span data-stu-id="d310d-199">Without the trailing slash relative URLs within the documents would be incorrect.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="d310d-200">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="d310d-200">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="d310d-201">`FileExtensionContentTypeProvider`類別包含 MIME 內容類型對應檔案副檔名集合。</span><span class="sxs-lookup"><span data-stu-id="d310d-201">The `FileExtensionContentTypeProvider` class contains a  collection that maps file extensions to MIME content types.</span></span> <span data-ttu-id="d310d-202">在下列範例中，數個檔案延伸模組已登錄到已知的 MIME 型別，取代".rtf 」，並 「.mp4"移除。</span><span class="sxs-lookup"><span data-stu-id="d310d-202">In the following sample, several file extensions are registered to known MIME types, the ".rtf" is replaced, and ".mp4" is removed.</span></span>

<span data-ttu-id="d310d-203">[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-203">[!code-csharp[Main](../fundamentals/static-files/sample/StartupFileExtensionContentTypeProvider.cs?highlight=3,4,5,6,7,8,9,10,11,12,19&name=snippet1)]</span></span>

<span data-ttu-id="d310d-204">請參閱[MIME 內容類型](http://www.iana.org/assignments/media-types/media-types.xhtml)。</span><span class="sxs-lookup"><span data-stu-id="d310d-204">See   [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="d310d-205">非標準的內容類型</span><span class="sxs-lookup"><span data-stu-id="d310d-205">Non-standard content types</span></span>

<span data-ttu-id="d310d-206">ASP.NET 靜態檔案中介軟體可理解幾乎 400 已知的檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="d310d-206">The ASP.NET static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="d310d-207">如果使用者要求未知的檔案類型的檔案時，靜態檔案中介軟體會傳回 HTTP 404 （找不到） 回應。</span><span class="sxs-lookup"><span data-stu-id="d310d-207">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not found) response.</span></span> <span data-ttu-id="d310d-208">如果已啟用目錄瀏覽，將會顯示檔案的連結，但 URI 會傳回 HTTP 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d310d-208">If directory browsing is enabled, a link to the file will be displayed, but the URI will return an HTTP 404 error.</span></span>

<span data-ttu-id="d310d-209">下列程式碼可讓處理未知的類型，並會轉譯為影像未知的檔案。</span><span class="sxs-lookup"><span data-stu-id="d310d-209">The following code enables serving unknown types and will render the unknown file as an image.</span></span>

<span data-ttu-id="d310d-210">[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="d310d-210">[!code-csharp[Main](static-files/sample/StartupServeUnknownFileTypes.cs?name=snippet1)]</span></span>

<span data-ttu-id="d310d-211">上述程式碼，具有未知的內容類型的檔案要求將會傳回做為映像。</span><span class="sxs-lookup"><span data-stu-id="d310d-211">With the code above, a request for a file with an unknown content type will be returned as an image.</span></span>

>[!WARNING]
> <span data-ttu-id="d310d-212">啟用`ServeUnknownFileTypes`會造成安全性風險，建議使用它。</span><span class="sxs-lookup"><span data-stu-id="d310d-212">Enabling `ServeUnknownFileTypes` is a security risk and using it is discouraged.</span></span>  <span data-ttu-id="d310d-213">`FileExtensionContentTypeProvider`（上述說明） 提供了更安全的替代方法，來提供檔案與非標準擴充功能。</span><span class="sxs-lookup"><span data-stu-id="d310d-213">`FileExtensionContentTypeProvider`  (explained above) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="d310d-214">考量</span><span class="sxs-lookup"><span data-stu-id="d310d-214">Considerations</span></span>

>[!WARNING]
> <span data-ttu-id="d310d-215">`UseDirectoryBrowser`和`UseStaticFiles`可能會導致洩漏機密資料。</span><span class="sxs-lookup"><span data-stu-id="d310d-215">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="d310d-216">我們建議您**不**啟用目錄瀏覽在生產環境中。</span><span class="sxs-lookup"><span data-stu-id="d310d-216">We recommend that you **not** enable directory browsing in production.</span></span> <span data-ttu-id="d310d-217">請小心有關哪些目錄啟用與`UseStaticFiles`或`UseDirectoryBrowser`如整個目錄和所有子目錄可存取。</span><span class="sxs-lookup"><span data-stu-id="d310d-217">Be careful about which directories you enable with `UseStaticFiles` or `UseDirectoryBrowser` as the entire directory and all sub-directories will be accessible.</span></span> <span data-ttu-id="d310d-218">我們建議您在自己的目錄中保持公用內容，例如*\<內容根目錄 > / wwwroot*、 離開應用程式檢視、 組態檔等等。</span><span class="sxs-lookup"><span data-stu-id="d310d-218">We recommend keeping public content in its own directory such as *\<content root>/wwwroot*, away from application views, configuration files, etc.</span></span>

* <span data-ttu-id="d310d-219">使用公開內容的 Url`UseDirectoryBrowser`和`UseStaticFiles`可能會有區分大小寫，其基礎檔案系統的字元限制。</span><span class="sxs-lookup"><span data-stu-id="d310d-219">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of their underlying file system.</span></span> <span data-ttu-id="d310d-220">例如，Windows 不區分大小寫，但 Mac 和 Linux 不是。</span><span class="sxs-lookup"><span data-stu-id="d310d-220">For example, Windows is case insensitive, but Mac and Linux are not.</span></span>

* <span data-ttu-id="d310d-221">在 IIS 中裝載的 ASP.NET Core 應用程式使用 ASP.NET 核心模組，將所有要求都轉送到應用程式包含靜態檔案的要求。</span><span class="sxs-lookup"><span data-stu-id="d310d-221">ASP.NET Core applications hosted in IIS use the ASP.NET Core Module to forward all requests to the application including requests for static files.</span></span> <span data-ttu-id="d310d-222">不使用 IIS 靜態檔案處理常式，因為它不會有機會處理要求之前會由 ASP.NET 核心模組。</span><span class="sxs-lookup"><span data-stu-id="d310d-222">The IIS static file handler is not used because it doesn't get a chance to handle requests before they are handled by the ASP.NET Core Module.</span></span>

* <span data-ttu-id="d310d-223">若要移除 IIS 靜態檔案處理常式 （在伺服器或網站層級）：</span><span class="sxs-lookup"><span data-stu-id="d310d-223">To remove the IIS static file handler (at the server or website level):</span></span>

     * <span data-ttu-id="d310d-224">瀏覽至**模組**功能</span><span class="sxs-lookup"><span data-stu-id="d310d-224">Navigate to the **Modules** feature</span></span>

     * <span data-ttu-id="d310d-225">選取**StaticFileModule**清單中</span><span class="sxs-lookup"><span data-stu-id="d310d-225">Select **StaticFileModule** in the list</span></span>

     * <span data-ttu-id="d310d-226">點選**移除**中**動作**[資訊看板]</span><span class="sxs-lookup"><span data-stu-id="d310d-226">Tap **Remove** in the **Actions** sidebar</span></span>

>[!WARNING]
> <span data-ttu-id="d310d-227">如果已啟用 IIS 靜態檔案處理常式**和**ASP.NET 核心模組 (ANCM) 的設定不正確 (例如如果*web.config*未部署)，系統將會處理靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="d310d-227">If the IIS static file handler is enabled **and** the ASP.NET Core Module (ANCM) is not correctly configured (for example if *web.config* was not deployed), static files will be served.</span></span>

* <span data-ttu-id="d310d-228">程式碼檔案 （包括 c# 和 Razor） 應該放在應用程式專案外部`web root`(*wwwroot*依預設)。</span><span class="sxs-lookup"><span data-stu-id="d310d-228">Code files (including c# and Razor) should be placed outside of the app project's `web root` (*wwwroot* by default).</span></span> <span data-ttu-id="d310d-229">這會建立您的應用程式的用戶端端內容和伺服器端原始程式碼，以防止伺服器端程式碼外洩清楚分隔。</span><span class="sxs-lookup"><span data-stu-id="d310d-229">This creates a clean separation between your app's client side content and server side source code, which prevents server side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d310d-230">其他資源</span><span class="sxs-lookup"><span data-stu-id="d310d-230">Additional Resources</span></span>

* [<span data-ttu-id="d310d-231">中介軟體</span><span class="sxs-lookup"><span data-stu-id="d310d-231">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="d310d-232">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="d310d-232">Introduction to ASP.NET Core</span></span>](../index.md)
