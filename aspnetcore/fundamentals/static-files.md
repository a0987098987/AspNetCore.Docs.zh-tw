---
title: ASP.NET Core 中的靜態檔案
author: rick-anderson
description: 了解如何在 ASP.NET Core Web 應用程式中提供靜態檔案、保護其安全，並設定靜態檔案裝載中介軟體的行為。
ms.author: riande
ms.custom: mvc
ms.date: 6/23/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/static-files
ms.openlocfilehash: 39c5c5d4875e1d59abaa6d998a09dbffd723214d
ms.sourcegitcommit: 895e952aec11c91d703fbdd3640a979307b8cc67
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/01/2020
ms.locfileid: "85793415"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="bfb36-103">ASP.NET Core 中的靜態檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-103">Static files in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bfb36-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Kirk Larkin](https://twitter.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="bfb36-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Kirk Larkin](https://twitter.com/serpent5)</span></span>

<span data-ttu-id="bfb36-105">靜態檔案（例如 HTML、CSS、影像和 JavaScript）是 ASP.NET Core 應用程式預設會直接提供給用戶端的資產。</span><span class="sxs-lookup"><span data-stu-id="bfb36-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients by default.</span></span>

<span data-ttu-id="bfb36-106">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples/3x)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="bfb36-106">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples/3x) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="bfb36-107">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-107">Serve static files</span></span>

<span data-ttu-id="bfb36-108">靜態檔案會儲存在專案的[web 根目錄](xref:fundamentals/index#web-root)中。</span><span class="sxs-lookup"><span data-stu-id="bfb36-108">Static files are stored within the project's [web root](xref:fundamentals/index#web-root) directory.</span></span> <span data-ttu-id="bfb36-109">預設目錄是 `{content root}/wwwroot` ，但可以使用[UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)方法來變更。</span><span class="sxs-lookup"><span data-stu-id="bfb36-109">The default directory is `{content root}/wwwroot`, but it can be changed with the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="bfb36-110">如需詳細資訊，請參閱[內容根目錄](xref:fundamentals/index#content-root)和[Web 根目錄](xref:fundamentals/index#web-root)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-110">For more information, see [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root).</span></span>

<span data-ttu-id="bfb36-111"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> 方法可將內容根目錄設定為目前的目錄：</span><span class="sxs-lookup"><span data-stu-id="bfb36-111">The <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder%2A> method sets the content root to the current directory:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/Program2.cs?name=snippet_Main)]

<span data-ttu-id="bfb36-112">上述程式碼是使用 web 應用程式範本所建立。</span><span class="sxs-lookup"><span data-stu-id="bfb36-112">The preceding code was created with the web app template.</span></span>

<span data-ttu-id="bfb36-113">靜態檔案可透過相對於[web 根目錄](xref:fundamentals/index#web-root)的路徑來存取。</span><span class="sxs-lookup"><span data-stu-id="bfb36-113">Static files are accessible via a path relative to the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="bfb36-114">例如， **Web 應用程式**專案範本包含資料夾內的數個資料夾 `wwwroot` ：</span><span class="sxs-lookup"><span data-stu-id="bfb36-114">For example, the **Web Application** project templates contain several folders within the `wwwroot` folder:</span></span>

* `wwwroot`
  * `css`
  * `js`
  * `lib`

<span data-ttu-id="bfb36-115">請考慮建立*wwwroot/images*資料夾，並新增*wwwroot/images/MyImage.jpg*檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-115">Consider creating the *wwwroot/images* folder and adding the *wwwroot/images/MyImage.jpg* file.</span></span> <span data-ttu-id="bfb36-116">用來存取資料夾中檔案的 URI 格式 `images` 為 `https://<hostname>/images/<image_file_name>` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-116">The URI format to access a file in the `images` folder is `https://<hostname>/images/<image_file_name>`.</span></span> <span data-ttu-id="bfb36-117">例如， `https://localhost:5001/images/MyImage.jpg`</span><span class="sxs-lookup"><span data-stu-id="bfb36-117">For example, `https://localhost:5001/images/MyImage.jpg`</span></span>

### <a name="serve-files-in-web-root"></a><span data-ttu-id="bfb36-118">在 web 根目錄中提供檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-118">Serve files in web root</span></span>

<span data-ttu-id="bfb36-119">預設 web 應用程式範本會呼叫 <xref:Owin.StaticFileExtensions.UseStaticFiles%2A> 中的方法 `Startup.Configure` ，讓您能夠提供靜態檔案：</span><span class="sxs-lookup"><span data-stu-id="bfb36-119">The default web app templates call the <xref:Owin.StaticFileExtensions.UseStaticFiles%2A> method in `Startup.Configure`, which enables static files to be served:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/Startup.cs?name=snippet&highlight=14)]

<span data-ttu-id="bfb36-120">無參數方法多載會 `UseStaticFiles` 將[web 根目錄](xref:fundamentals/index#web-root)中的檔案標記為 servable。</span><span class="sxs-lookup"><span data-stu-id="bfb36-120">The parameterless `UseStaticFiles` method overload marks the files in [web root](xref:fundamentals/index#web-root) as servable.</span></span> <span data-ttu-id="bfb36-121">下列標記會參考*wwwroot/images/MyImage.jpg*：</span><span class="sxs-lookup"><span data-stu-id="bfb36-121">The following markup references *wwwroot/images/MyImage.jpg*:</span></span>

```html
<img src="~/images/MyImage.jpg"  class="img" alt="My image." />
```

<span data-ttu-id="bfb36-122">在上述程式碼中，波狀符號字元會 `~/` 指向[web 根目錄](xref:fundamentals/index#web-root)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-122">In the preceding code, the tilde character `~/` points to the [web root](xref:fundamentals/index#web-root).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="bfb36-123">提供 Web 根目錄外的檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-123">Serve files outside of web root</span></span>

<span data-ttu-id="bfb36-124">假設有一個目錄階層，其中要提供的靜態檔案位於[web 根目錄](xref:fundamentals/index#web-root)外部：</span><span class="sxs-lookup"><span data-stu-id="bfb36-124">Consider a directory hierarchy in which the static files to be served reside outside of the [web root](xref:fundamentals/index#web-root):</span></span>

* `wwwroot`
  * `css`
  * `images`
  * `js`
* `MyStaticFiles`
  * `images`
    * `red-rose.jpg`

<span data-ttu-id="bfb36-125">要求可以藉由設定 `red-rose.jpg` 靜態檔案中介軟體來存取檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bfb36-125">A request can access the `red-rose.jpg` file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupRose.cs?name=snippet&highlight=14-21)]

<span data-ttu-id="bfb36-126">上述程式碼會透過 *StaticFiles* URI 區段公開 *MyStaticFiles* 目錄階層。</span><span class="sxs-lookup"><span data-stu-id="bfb36-126">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="bfb36-127">為red-rose.jpg檔案提供 `https://<hostname>/StaticFiles/images/red-rose.jpg` 服務*red-rose.jpg*的要求。</span><span class="sxs-lookup"><span data-stu-id="bfb36-127">A request to `https://<hostname>/StaticFiles/images/red-rose.jpg` serves the *red-rose.jpg* file.</span></span>

<span data-ttu-id="bfb36-128">下列標記會參考*MyStaticFiles/images/red-rose.jpg*：</span><span class="sxs-lookup"><span data-stu-id="bfb36-128">The following markup references *MyStaticFiles/images/red-rose.jpg*:</span></span>

```html
<img src="~/StaticFiles/images/red-rose.jpg" class="img" alt="A red rose." />
```

### <a name="set-http-response-headers"></a><span data-ttu-id="bfb36-129">設定 HTTP 回應標頭</span><span class="sxs-lookup"><span data-stu-id="bfb36-129">Set HTTP response headers</span></span>

<span data-ttu-id="bfb36-130"><xref:Microsoft.AspNetCore.Builder.StaticFileOptions>物件可以用來設定 HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="bfb36-130">A <xref:Microsoft.AspNetCore.Builder.StaticFileOptions> object can be used to set HTTP response headers.</span></span> <span data-ttu-id="bfb36-131">除了設定[web 根目錄](xref:fundamentals/index#web-root)提供的靜態檔案外，下列程式碼也會設定 `Cache-Control` 標頭：</span><span class="sxs-lookup"><span data-stu-id="bfb36-131">In addition to configuring static file serving from the [web root](xref:fundamentals/index#web-root), the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupAddHeader.cs?name=snippet&highlight=14-23)]

<span data-ttu-id="bfb36-132">靜態檔案可公開快取達600秒：</span><span class="sxs-lookup"><span data-stu-id="bfb36-132">Static files are publicly cacheable for 600 seconds:</span></span>

![已新增顯示「快取控制」標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="bfb36-134">靜態檔案授權</span><span class="sxs-lookup"><span data-stu-id="bfb36-134">Static file authorization</span></span>

<span data-ttu-id="bfb36-135">靜態檔案中介軟體不提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="bfb36-135">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="bfb36-136">由 it 提供服務的任何檔案（包括下的檔案） `wwwroot` 都可公開存取。</span><span class="sxs-lookup"><span data-stu-id="bfb36-136">Any files served by it, including those under `wwwroot`, are publicly accessible.</span></span> <span data-ttu-id="bfb36-137">若要依據授權來提供檔案：</span><span class="sxs-lookup"><span data-stu-id="bfb36-137">To serve files based on authorization:</span></span>

* <span data-ttu-id="bfb36-138">將它們儲存 `wwwroot` 在之外，以及可存取靜態檔案中介軟體的任何目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-138">Store them outside of `wwwroot` and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="bfb36-139">透過套用授權的動作方法來提供服務，並傳回[FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult)物件：</span><span class="sxs-lookup"><span data-stu-id="bfb36-139">Serve them via an action method to which authorization is applied and return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="directory-browsing"></a><span data-ttu-id="bfb36-140">瀏覽目錄</span><span class="sxs-lookup"><span data-stu-id="bfb36-140">Directory browsing</span></span>

<span data-ttu-id="bfb36-141">流覽目錄允許在指定的目錄中列出目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-141">Directory browsing allows directory listing within specified directories.</span></span>

<span data-ttu-id="bfb36-142">基於安全性理由，預設會停用流覽目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-142">Directory browsing is disabled by default for security reasons.</span></span> <span data-ttu-id="bfb36-143">如需詳細資訊，請參閱[考慮](#sc)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-143">For more information, see [Considerations](#sc).</span></span>

<span data-ttu-id="bfb36-144">使用下列方式啟用瀏覽目錄：</span><span class="sxs-lookup"><span data-stu-id="bfb36-144">Enable directory browsing with:</span></span>

* <span data-ttu-id="bfb36-145"><xref:Microsoft.Extensions.DependencyInjection.DirectoryBrowserServiceExtensions.AddDirectoryBrowser%2A>在中 `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-145"><xref:Microsoft.Extensions.DependencyInjection.DirectoryBrowserServiceExtensions.AddDirectoryBrowser%2A> in `Startup.ConfigureServices`.</span></span>
* <span data-ttu-id="bfb36-146">中的[UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-146">[UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) in `Startup.Configure`.</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupBrowse.cs?name=snippet&highlight=4,20-34)]

<span data-ttu-id="bfb36-147">上述程式碼允許使用 URL 流覽*wwwroot/images*資料夾的目錄，並 `https://<hostname>/MyImages` 提供每個檔案和資料夾的連結：</span><span class="sxs-lookup"><span data-stu-id="bfb36-147">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL `https://<hostname>/MyImages`, with links to each file and folder:</span></span>

![目錄瀏覽](static-files/_static/dir-browse.png)

## <a name="serve-default-documents"></a><span data-ttu-id="bfb36-149">提供預設檔</span><span class="sxs-lookup"><span data-stu-id="bfb36-149">Serve default documents</span></span>

<span data-ttu-id="bfb36-150">設定預設頁面可讓訪客成為網站的起點。</span><span class="sxs-lookup"><span data-stu-id="bfb36-150">Setting a default page provides visitors a starting point on a site.</span></span> <span data-ttu-id="bfb36-151">若要從沒有完整 URI 的中提供預設頁面 `wwwroot` ，請呼叫 <xref:Owin.DefaultFilesExtensions.UseDefaultFiles%2A> 方法：</span><span class="sxs-lookup"><span data-stu-id="bfb36-151">To serve a default page from `wwwroot` without a fully qualified URI, call the <xref:Owin.DefaultFilesExtensions.UseDefaultFiles%2A> method:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupEmpty.cs?name=snippet&highlight=14)]

<span data-ttu-id="bfb36-152">您必須在 `UseStaticFiles` 之前先呼叫 `UseDefaultFiles`，才能提供預設的檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-152">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="bfb36-153">`UseDefaultFiles`是不會為檔案提供服務的 URL 重寫工具。</span><span class="sxs-lookup"><span data-stu-id="bfb36-153">`UseDefaultFiles` is a URL rewriter that doesn't serve the file.</span></span>

<span data-ttu-id="bfb36-154">使用 `UseDefaultFiles` ，對中的資料夾要求 `wwwroot` 搜尋：</span><span class="sxs-lookup"><span data-stu-id="bfb36-154">With `UseDefaultFiles`, requests to a folder in `wwwroot` search for:</span></span>

* <span data-ttu-id="bfb36-155">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="bfb36-155">*default.htm*</span></span>
* <span data-ttu-id="bfb36-156">*default.html*</span><span class="sxs-lookup"><span data-stu-id="bfb36-156">*default.html*</span></span>
* <span data-ttu-id="bfb36-157">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="bfb36-157">*index.htm*</span></span>
* <span data-ttu-id="bfb36-158">*index.html*</span><span class="sxs-lookup"><span data-stu-id="bfb36-158">*index.html*</span></span>

<span data-ttu-id="bfb36-159">系統會提供從清單中找到的第一個檔案，就像提出的要求是完整 URI 一樣。</span><span class="sxs-lookup"><span data-stu-id="bfb36-159">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="bfb36-160">瀏覽器 URL 仍會繼續反應要求的 URI。</span><span class="sxs-lookup"><span data-stu-id="bfb36-160">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="bfb36-161">下列程式碼會將預設檔案名稱變更為 *mydefault.html*：</span><span class="sxs-lookup"><span data-stu-id="bfb36-161">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupDefault.cs?name=snippet2)]

<span data-ttu-id="bfb36-162">下列程式碼會顯示 `Startup.Configure` 上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="bfb36-162">The following code shows `Startup.Configure` with the preceding code:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupDefault.cs?name=snippet)]

### <a name="usefileserver-for-default-documents"></a><span data-ttu-id="bfb36-163">預設檔的 UseFileServer</span><span class="sxs-lookup"><span data-stu-id="bfb36-163">UseFileServer for default documents</span></span>

<span data-ttu-id="bfb36-164"><xref:Microsoft.AspNetCore.Builder.FileServerExtensions.UseFileServer*>結合 `UseStaticFiles` 、和的功能 `UseDefaultFiles` （選擇性） `UseDirectoryBrowser` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-164"><xref:Microsoft.AspNetCore.Builder.FileServerExtensions.UseFileServer*> combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and optionally `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="bfb36-165">呼叫 `app.UseFileServer` 以啟用靜態檔案和預設檔案的服務。</span><span class="sxs-lookup"><span data-stu-id="bfb36-165">Call `app.UseFileServer` to enable the serving of static files and the default file.</span></span> <span data-ttu-id="bfb36-166">未啟用目錄瀏覽功能。</span><span class="sxs-lookup"><span data-stu-id="bfb36-166">Directory browsing isn't enabled.</span></span> <span data-ttu-id="bfb36-167">下列程式碼顯示 `Startup.Configure` `UseFileServer` ：</span><span class="sxs-lookup"><span data-stu-id="bfb36-167">The following code shows `Startup.Configure` with `UseFileServer`:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupEmpty2.cs?name=snippet&highlight=14)]

<span data-ttu-id="bfb36-168">下列程式碼會啟用靜態檔案、預設檔案和流覽目錄的服務：</span><span class="sxs-lookup"><span data-stu-id="bfb36-168">The following code enables the serving of static files, the default file, and directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="bfb36-169">下列程式碼會顯示 `Startup.Configure` 上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="bfb36-169">The following code shows `Startup.Configure` with the preceding code:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupEmpty3.cs?name=snippet&highlight=14-18)]

<span data-ttu-id="bfb36-170">請考慮下列目錄階層：</span><span class="sxs-lookup"><span data-stu-id="bfb36-170">Consider the following directory hierarchy:</span></span>

* `wwwroot`
  * `css`
  * `images`
  * `js`
* `MyStaticFiles`
  * `images`
    * `MyImage.jpg`
  * `default.html`

<span data-ttu-id="bfb36-171">下列程式碼會啟用靜態檔案、預設檔案和瀏覽目錄的服務 `MyStaticFiles` ：</span><span class="sxs-lookup"><span data-stu-id="bfb36-171">The following code enables the serving of static files, the default file, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupUseFileServer.cs?name=snippet&highlight=4,20-30)]

<span data-ttu-id="bfb36-172"><xref:Microsoft.Extensions.DependencyInjection.DirectoryBrowserServiceExtensions.AddDirectoryBrowser%2A>當 `EnableDirectoryBrowsing` 屬性值為時，必須呼叫 `true` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-172"><xref:Microsoft.Extensions.DependencyInjection.DirectoryBrowserServiceExtensions.AddDirectoryBrowser%2A> must be called when the `EnableDirectoryBrowsing` property value is `true`.</span></span>

<span data-ttu-id="bfb36-173">使用檔案階層和上述程式碼時，URL 解析方式如下：</span><span class="sxs-lookup"><span data-stu-id="bfb36-173">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="bfb36-174">URI</span><span class="sxs-lookup"><span data-stu-id="bfb36-174">URI</span></span>            |      <span data-ttu-id="bfb36-175">回應</span><span class="sxs-lookup"><span data-stu-id="bfb36-175">Response</span></span>  |
| ------- | ------|
| `https://<hostname>/StaticFiles/images/MyImage.jpg` | <span data-ttu-id="bfb36-176">*MyStaticFiles/images/MyImage.jpg*</span><span class="sxs-lookup"><span data-stu-id="bfb36-176">*MyStaticFiles/images/MyImage.jpg*</span></span> |
| `https://<hostname>/StaticFiles` | <span data-ttu-id="bfb36-177">*MyStaticFiles/default.html*</span><span class="sxs-lookup"><span data-stu-id="bfb36-177">*MyStaticFiles/default.html*</span></span> |

<span data-ttu-id="bfb36-178">如果*MyStaticFiles*目錄中沒有預設名稱的檔案存在，則會傳回具有可按式 `https://<hostname>/StaticFiles` 連結的目錄清單：</span><span class="sxs-lookup"><span data-stu-id="bfb36-178">If no default-named file exists in the *MyStaticFiles* directory, `https://<hostname>/StaticFiles` returns the directory listing with clickable links:</span></span>

![靜態檔案清單](static-files/_static/db2.png)

<span data-ttu-id="bfb36-180"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*>和 <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> 會從目標 uri 執行用戶端重新導向，而不使用尾端的 `/` 目標 uri `/` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-180"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> and <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> perform a client-side redirect from the target URI without a trailing `/`  to the target URI with a trailing `/`.</span></span> <span data-ttu-id="bfb36-181">例如，從 `https://<hostname>/StaticFiles` 到 `https://<hostname>/StaticFiles/` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-181">For example, from `https://<hostname>/StaticFiles` to `https://<hostname>/StaticFiles/`.</span></span> <span data-ttu-id="bfb36-182">*StaticFiles*目錄內的相對 url 無效，沒有尾端斜線（ `/` ）。</span><span class="sxs-lookup"><span data-stu-id="bfb36-182">Relative URLs within the *StaticFiles* directory are invalid without a trailing slash (`/`).</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="bfb36-183">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="bfb36-183">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="bfb36-184">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider)類別包含一個 `Mappings` 屬性，可做為 MIME 內容類型的副檔名對應。</span><span class="sxs-lookup"><span data-stu-id="bfb36-184">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property that serves as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="bfb36-185">在下列範例中，有數個副檔名會對應到已知的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="bfb36-185">In the following sample, several file extensions are mapped to known MIME types.</span></span> <span data-ttu-id="bfb36-186">已取代 *.rtf*副檔名，而已移除 *. （.* ）：</span><span class="sxs-lookup"><span data-stu-id="bfb36-186">The *.rtf* extension is replaced, and *.mp4* is removed:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupFileExtensionContentTypeProvider.cs?name=snippet2)]

<span data-ttu-id="bfb36-187">下列程式碼會顯示 `Startup.Configure` 上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="bfb36-187">The following code shows `Startup.Configure` with the preceding code:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupFileExtensionContentTypeProvider.cs?name=snippet&highlight=14-42)]

<span data-ttu-id="bfb36-188">請參閱 [MIME 內容類型](https://www.iana.org/assignments/media-types/media-types.xhtml)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-188">See [MIME content types](https://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="bfb36-189">非標準的內容類型</span><span class="sxs-lookup"><span data-stu-id="bfb36-189">Non-standard content types</span></span>

<span data-ttu-id="bfb36-190">靜態檔案中介軟體會瞭解將近400的已知檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="bfb36-190">The Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="bfb36-191">如果使用者要求的檔案具有不明的檔案類型，則靜態檔案中介軟體會將要求傳遞至管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="bfb36-191">If the user requests a file with an unknown file type, the Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="bfb36-192">如果沒有中介軟體處理要求，則會傳回「404 找不到」\*\* 回應。</span><span class="sxs-lookup"><span data-stu-id="bfb36-192">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="bfb36-193">如果啟用目錄瀏覽功能，則會在目錄清單中顯示檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="bfb36-193">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="bfb36-194">下列程式碼可讓您提供未知的類型，並會將未知的檔案轉譯為影像：</span><span class="sxs-lookup"><span data-stu-id="bfb36-194">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupServeUnknownFileTypes.cs?name=snippet2)]

<span data-ttu-id="bfb36-195">下列程式碼會顯示 `Startup.Configure` 上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="bfb36-195">The following code shows `Startup.Configure` with the preceding code:</span></span>

[!code-csharp[](~/fundamentals/static-files/samples/3x/sample/StartupServeUnknownFileTypes.cs?name=snippet&highlight=14-18)]

<span data-ttu-id="bfb36-196">使用上述程式碼時，系統會以影像來回應具有未知內容類型的檔案要求。</span><span class="sxs-lookup"><span data-stu-id="bfb36-196">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="bfb36-197">啟用 [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) 時會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="bfb36-197">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="bfb36-198">預設為停用，亦不建議您使用。</span><span class="sxs-lookup"><span data-stu-id="bfb36-198">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="bfb36-199">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) 可提供更安全的替代方法，來提供非標準副檔名的檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-199">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

## <a name="serve-files-from-multiple-locations"></a><span data-ttu-id="bfb36-200">提供多個位置的檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-200">Serve files from multiple locations</span></span>

<span data-ttu-id="bfb36-201">`UseStaticFiles`和 `UseFileServer` 預設為指向的檔案提供者 `wwwroot` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-201">`UseStaticFiles` and `UseFileServer` default to the file provider pointing at `wwwroot`.</span></span> <span data-ttu-id="bfb36-202">和的其他 `UseStaticFiles` 實例 `UseFileServer` 可與其他檔案提供者一起提供，以提供其他位置的檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-202">Additional instances of `UseStaticFiles` and `UseFileServer` can be provided with other file providers to serve files from other locations.</span></span> <span data-ttu-id="bfb36-203">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/15578)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-203">For more information, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/15578).</span></span>

<a name="sc"></a>

### <a name="security-considerations-for-static-files"></a><span data-ttu-id="bfb36-204">靜態檔案的安全性考慮</span><span class="sxs-lookup"><span data-stu-id="bfb36-204">Security considerations for static files</span></span>

> [!WARNING]
> <span data-ttu-id="bfb36-205">`UseDirectoryBrowser` 和 `UseStaticFiles` 可能會導致洩漏祕密。</span><span class="sxs-lookup"><span data-stu-id="bfb36-205">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="bfb36-206">強烈建議您在生產環境中停用目錄瀏覽功能。</span><span class="sxs-lookup"><span data-stu-id="bfb36-206">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="bfb36-207">透過 `UseStaticFiles` 或 `UseDirectoryBrowser`，仔細檢閱要啟用哪些目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-207">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="bfb36-208">因為整個目錄和其子目錄都可供公開存取。</span><span class="sxs-lookup"><span data-stu-id="bfb36-208">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="bfb36-209">儲存適合用於在專用目錄（例如）中公用的檔案 `<content_root>/wwwroot` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-209">Store files suitable for serving to the public in a dedicated directory, such as `<content_root>/wwwroot`.</span></span> <span data-ttu-id="bfb36-210">將這些檔案與 MVC 視圖、 Razor 頁面、設定檔等區隔開。</span><span class="sxs-lookup"><span data-stu-id="bfb36-210">Separate these files from MVC views, Razor Pages, configuration files, etc.</span></span>

* <span data-ttu-id="bfb36-211">使用 `UseDirectoryBrowser` 和 `UseStaticFiles` 公開內容的 URL 可能有區分大小寫，並受限於基礎檔案系統的字元限制。</span><span class="sxs-lookup"><span data-stu-id="bfb36-211">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="bfb36-212">例如，Windows 不區分大小寫，但 macOS 和 Linux 則不會。</span><span class="sxs-lookup"><span data-stu-id="bfb36-212">For example, Windows is case insensitive, but macOS and Linux aren't.</span></span>

* <span data-ttu-id="bfb36-213">裝載於 IIS 中的 ASP.NET Core 應用程式會使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，將所有要求轉送給應用程式 (包括靜態檔案要求)，</span><span class="sxs-lookup"><span data-stu-id="bfb36-213">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="bfb36-214">未使用 IIS 靜態檔案處理常式，而且沒有機會處理要求。</span><span class="sxs-lookup"><span data-stu-id="bfb36-214">The IIS static file handler isn't used and has no chance to handle requests.</span></span>

* <span data-ttu-id="bfb36-215">請完成 IIS 管理員中的下列步驟，以移除伺服器或網站層級的 IIS 靜態檔案處理常式：</span><span class="sxs-lookup"><span data-stu-id="bfb36-215">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="bfb36-216">巡覽至 [模組]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="bfb36-216">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="bfb36-217">選取清單中的 **StaticFileModule**。</span><span class="sxs-lookup"><span data-stu-id="bfb36-217">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="bfb36-218">按一下 [動作]\*\*\*\* 側邊欄中的 [移除]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="bfb36-218">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="bfb36-219">如果已啟用 IIS 靜態檔案處理常式，**但是**未正確設定 ASP.NET Core 模組，仍可提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-219">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="bfb36-220">舉例來說，未部署 *web.config* 檔案時可能會發生上述情況。</span><span class="sxs-lookup"><span data-stu-id="bfb36-220">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="bfb36-221">將程式碼檔案（包括 *.cs*和*cshtml*）放在應用程式專案的[web 根目錄](xref:fundamentals/index#web-root)外部。</span><span class="sxs-lookup"><span data-stu-id="bfb36-221">Place code files, including *.cs* and *.cshtml*, outside of the app project's [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="bfb36-222">如此一來，即會建立應用程式的用戶端內容與伺服器端程式碼之間的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="bfb36-222">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="bfb36-223">這樣可以防止伺服器端程式碼外洩。</span><span class="sxs-lookup"><span data-stu-id="bfb36-223">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bfb36-224">其他資源</span><span class="sxs-lookup"><span data-stu-id="bfb36-224">Additional resources</span></span>

* [<span data-ttu-id="bfb36-225">中介軟體</span><span class="sxs-lookup"><span data-stu-id="bfb36-225">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="bfb36-226">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="bfb36-226">Introduction to ASP.NET Core</span></span>](xref:index)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bfb36-227">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Scott Addie](https://twitter.com/Scott_Addie) 撰寫</span><span class="sxs-lookup"><span data-stu-id="bfb36-227">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="bfb36-228">HTML、CSS、影像和 JavaScript 這類靜態檔案都是 ASP.NET Core 應用程式直接提供給用戶端的資產。</span><span class="sxs-lookup"><span data-stu-id="bfb36-228">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="bfb36-229">您需要進行一些設定，才能提供這些檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-229">Some configuration is required to enable serving of these files.</span></span>

<span data-ttu-id="bfb36-230">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="bfb36-230">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="bfb36-231">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-231">Serve static files</span></span>

<span data-ttu-id="bfb36-232">靜態檔案會儲存在專案的[web 根目錄](xref:fundamentals/index#web-root)中。</span><span class="sxs-lookup"><span data-stu-id="bfb36-232">Static files are stored within the project's [web root](xref:fundamentals/index#web-root) directory.</span></span> <span data-ttu-id="bfb36-233">預設目錄是 *{content root}/wwwroot*，但可以透過[UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)方法加以變更。</span><span class="sxs-lookup"><span data-stu-id="bfb36-233">The default directory is *{content root}/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="bfb36-234">如需詳細資訊，請參閱[內容根目錄](xref:fundamentals/index#content-root)和 [Web 根目錄](xref:fundamentals/index#web-root)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-234">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="bfb36-235">您必須讓應用程式的 Web 主機記住內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-235">The app's web host must be made aware of the content root directory.</span></span>

<span data-ttu-id="bfb36-236">`WebHost.CreateDefaultBuilder` 方法可將內容根目錄設定為目前的目錄：</span><span class="sxs-lookup"><span data-stu-id="bfb36-236">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

<span data-ttu-id="bfb36-237">靜態檔案可透過相對於[web 根目錄](xref:fundamentals/index#web-root)的路徑來存取。</span><span class="sxs-lookup"><span data-stu-id="bfb36-237">Static files are accessible via a path relative to the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="bfb36-238">例如， **Web 應用程式**專案範本包含資料夾內的數個資料夾 `wwwroot` ：</span><span class="sxs-lookup"><span data-stu-id="bfb36-238">For example, the **Web Application** project template contains several folders within the `wwwroot` folder:</span></span>

* `wwwroot`
  * `css`
  * `images`
  * `js`

<span data-ttu-id="bfb36-239">在*images*子資料夾中存取檔案的 URI 格式為\*HTTP:// \<server_address> /images/ \<image_file_name> \*。</span><span class="sxs-lookup"><span data-stu-id="bfb36-239">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="bfb36-240">例如： *http://localhost:9189/images/banner3.svg* 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-240">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

<span data-ttu-id="bfb36-241">如以 .NET Framework 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-241">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="bfb36-242">如以 .NET Core 為目標，[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage-app)會包含此套件。</span><span class="sxs-lookup"><span data-stu-id="bfb36-242">If targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) includes this package.</span></span>

<span data-ttu-id="bfb36-243">設定[中介軟體](xref:fundamentals/middleware/index)，這會啟用靜態檔案的服務。</span><span class="sxs-lookup"><span data-stu-id="bfb36-243">Configure the [middleware](xref:fundamentals/middleware/index), which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="bfb36-244">提供 Web 根目錄內的檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-244">Serve files inside of web root</span></span>

<span data-ttu-id="bfb36-245">叫用 `Startup.Configure` 內的 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：</span><span class="sxs-lookup"><span data-stu-id="bfb36-245">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="bfb36-246">無參數方法多載會 `UseStaticFiles` 將[web 根目錄](xref:fundamentals/index#web-root)中的檔案標記為 servable。</span><span class="sxs-lookup"><span data-stu-id="bfb36-246">The parameterless `UseStaticFiles` method overload marks the files in [web root](xref:fundamentals/index#web-root) as servable.</span></span> <span data-ttu-id="bfb36-247">下列標記參考 *wwwroot/images/banner1.svg*：</span><span class="sxs-lookup"><span data-stu-id="bfb36-247">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

<span data-ttu-id="bfb36-248">在上述程式碼中，波狀符號字元會 `~/` 指向[web 根目錄](xref:fundamentals/index#web-root)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-248">In the preceding code, the tilde character `~/` points to the [web root](xref:fundamentals/index#web-root).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="bfb36-249">提供 Web 根目錄外的檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-249">Serve files outside of web root</span></span>

<span data-ttu-id="bfb36-250">假設有一個目錄階層，其中要提供的靜態檔案位於[web 根目錄](xref:fundamentals/index#web-root)外部：</span><span class="sxs-lookup"><span data-stu-id="bfb36-250">Consider a directory hierarchy in which the static files to be served reside outside of the [web root](xref:fundamentals/index#web-root):</span></span>

* `wwwroot`
  * `css`
  * `images`
  * `js`
* `MyStaticFiles`
  * `images`
    * `banner1.svg`

<span data-ttu-id="bfb36-251">透過設定靜態檔案中介軟體，可讓要求存取 *banner1.svg* 檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bfb36-251">A request can access the *banner1.svg* file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="bfb36-252">上述程式碼會透過 *StaticFiles* URI 區段公開 *MyStaticFiles* 目錄階層。</span><span class="sxs-lookup"><span data-stu-id="bfb36-252">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="bfb36-253">對*Http:// \<server_address> /StaticFiles/images/banner1.svg*的要求會提供*banner1.svg svg*檔案的服務。</span><span class="sxs-lookup"><span data-stu-id="bfb36-253">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="bfb36-254">下列標記參考 *MyStaticFiles/images/banner1.svg*：</span><span class="sxs-lookup"><span data-stu-id="bfb36-254">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="bfb36-255">設定 HTTP 回應標頭</span><span class="sxs-lookup"><span data-stu-id="bfb36-255">Set HTTP response headers</span></span>

<span data-ttu-id="bfb36-256">[StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) 物件可以用來設定 HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="bfb36-256">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="bfb36-257">除了設定[web 根目錄](xref:fundamentals/index#web-root)提供的靜態檔案外，下列程式碼也會設定 `Cache-Control` 標頭：</span><span class="sxs-lookup"><span data-stu-id="bfb36-257">In addition to configuring static file serving from the [web root](xref:fundamentals/index#web-root), the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="bfb36-258">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) 方法存在於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="bfb36-258">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="bfb36-259">檔案已在開發環境中設為可公開快取 10 分鐘 (600 秒)：</span><span class="sxs-lookup"><span data-stu-id="bfb36-259">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![已新增顯示「快取控制」標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="bfb36-261">靜態檔案授權</span><span class="sxs-lookup"><span data-stu-id="bfb36-261">Static file authorization</span></span>

<span data-ttu-id="bfb36-262">靜態檔案中介軟體不提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="bfb36-262">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="bfb36-263">其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開存取。</span><span class="sxs-lookup"><span data-stu-id="bfb36-263">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="bfb36-264">若要依據授權來提供檔案：</span><span class="sxs-lookup"><span data-stu-id="bfb36-264">To serve files based on authorization:</span></span>

* <span data-ttu-id="bfb36-265">請將它們儲存在 *wwwroot* 外部和可存取靜態檔案中介軟體的任何目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-265">Store them outside of *wwwroot* and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="bfb36-266">透過動作方法，將它們提供給已套用授權的目標。</span><span class="sxs-lookup"><span data-stu-id="bfb36-266">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="bfb36-267">傳回 [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) 物件：</span><span class="sxs-lookup"><span data-stu-id="bfb36-267">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="bfb36-268">啟用目錄瀏覽</span><span class="sxs-lookup"><span data-stu-id="bfb36-268">Enable directory browsing</span></span>

<span data-ttu-id="bfb36-269">您的 Web 應用程式使用者可藉由目錄瀏覽功能，查看目錄清單及指定目錄內的檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-269">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="bfb36-270">基於安全性考量，預設為停用目錄瀏覽功能 (請參閱[考量](#sc))。</span><span class="sxs-lookup"><span data-stu-id="bfb36-270">Directory browsing is disabled by default for security reasons (see [Considerations](#sc)).</span></span> <span data-ttu-id="bfb36-271">您可以叫用 `Startup.Configure` 內的 [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) 方法，以啟用目錄瀏覽功能：</span><span class="sxs-lookup"><span data-stu-id="bfb36-271">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="bfb36-272">從叫用方法來新增所需的服務 <xref:Microsoft.Extensions.DependencyInjection.DirectoryBrowserServiceExtensions.AddDirectoryBrowser%2A> `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="bfb36-272">Add required services by invoking the <xref:Microsoft.Extensions.DependencyInjection.DirectoryBrowserServiceExtensions.AddDirectoryBrowser%2A> method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="bfb36-273">上述程式碼允許使用 URL *Http:// \<server_address> /MyImages*流覽*wwwroot/images*資料夾的目錄，並提供每個檔案和資料夾的連結：</span><span class="sxs-lookup"><span data-stu-id="bfb36-273">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![目錄瀏覽](static-files/_static/dir-browse.png)

<span data-ttu-id="bfb36-275">如需了解啟用瀏覽功能時的安全性風險，請參閱[考量](#considerations)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-275">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="bfb36-276">請注意下列範例中的兩個 `UseStaticFiles` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="bfb36-276">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="bfb36-277">第一個呼叫可提供 *wwwroot* 資料夾中的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-277">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="bfb36-278">第二個呼叫會使用 URL *Http:// \<server_address> /MyImages*，啟用*wwwroot/images*資料夾的瀏覽目錄：</span><span class="sxs-lookup"><span data-stu-id="bfb36-278">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="bfb36-279">提供預設文件</span><span class="sxs-lookup"><span data-stu-id="bfb36-279">Serve a default document</span></span>

<span data-ttu-id="bfb36-280">設定預設的歡迎頁面，可讓訪客在瀏覽您的網站時有個合乎邏輯的起點。</span><span class="sxs-lookup"><span data-stu-id="bfb36-280">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="bfb36-281">若要提供預設頁面 (使用者不需完整限定 URI)，請從 `Startup.Configure` 呼叫 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：</span><span class="sxs-lookup"><span data-stu-id="bfb36-281">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="bfb36-282">您必須在 `UseStaticFiles` 之前先呼叫 `UseDefaultFiles`，才能提供預設的檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-282">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="bfb36-283">`UseDefaultFiles` 是 URL 重寫器，並非實際提供檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-283">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="bfb36-284">透過 `UseStaticFiles` 啟用靜態檔案中介軟體以提供檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-284">Enable Static File Middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="bfb36-285">使用 `UseDefaultFiles` 要求以搜尋資料夾：</span><span class="sxs-lookup"><span data-stu-id="bfb36-285">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="bfb36-286">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="bfb36-286">*default.htm*</span></span>
* <span data-ttu-id="bfb36-287">*default.html*</span><span class="sxs-lookup"><span data-stu-id="bfb36-287">*default.html*</span></span>
* <span data-ttu-id="bfb36-288">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="bfb36-288">*index.htm*</span></span>
* <span data-ttu-id="bfb36-289">*index.html*</span><span class="sxs-lookup"><span data-stu-id="bfb36-289">*index.html*</span></span>

<span data-ttu-id="bfb36-290">系統會提供從清單中找到的第一個檔案，就像提出的要求是完整 URI 一樣。</span><span class="sxs-lookup"><span data-stu-id="bfb36-290">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="bfb36-291">瀏覽器 URL 仍會繼續反應要求的 URI。</span><span class="sxs-lookup"><span data-stu-id="bfb36-291">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="bfb36-292">下列程式碼會將預設檔案名稱變更為 *mydefault.html*：</span><span class="sxs-lookup"><span data-stu-id="bfb36-292">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="bfb36-293">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="bfb36-293">UseFileServer</span></span>

<span data-ttu-id="bfb36-294"><xref:Microsoft.AspNetCore.Builder.FileServerExtensions.UseFileServer*>結合 `UseStaticFiles` 、和的功能 `UseDefaultFiles` （選擇性） `UseDirectoryBrowser` 。</span><span class="sxs-lookup"><span data-stu-id="bfb36-294"><xref:Microsoft.AspNetCore.Builder.FileServerExtensions.UseFileServer*> combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and optionally `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="bfb36-295">下列程式碼可提供靜態檔案和預設檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-295">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="bfb36-296">未啟用目錄瀏覽功能。</span><span class="sxs-lookup"><span data-stu-id="bfb36-296">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="bfb36-297">下列程式碼會啟用目錄瀏覽功能，以在無參數多載上進行建置：</span><span class="sxs-lookup"><span data-stu-id="bfb36-297">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="bfb36-298">請考慮下列目錄階層：</span><span class="sxs-lookup"><span data-stu-id="bfb36-298">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="bfb36-299">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="bfb36-299">**wwwroot**</span></span>
  * <span data-ttu-id="bfb36-300">**css**</span><span class="sxs-lookup"><span data-stu-id="bfb36-300">**css**</span></span>
  * <span data-ttu-id="bfb36-301">**images**</span><span class="sxs-lookup"><span data-stu-id="bfb36-301">**images**</span></span>
  * <span data-ttu-id="bfb36-302">**node.js**</span><span class="sxs-lookup"><span data-stu-id="bfb36-302">**js**</span></span>
* <span data-ttu-id="bfb36-303">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="bfb36-303">**MyStaticFiles**</span></span>
  * <span data-ttu-id="bfb36-304">**images**</span><span class="sxs-lookup"><span data-stu-id="bfb36-304">**images**</span></span>
    * <span data-ttu-id="bfb36-305">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="bfb36-305">*banner1.svg*</span></span>
  * <span data-ttu-id="bfb36-306">*default.html*</span><span class="sxs-lookup"><span data-stu-id="bfb36-306">*default.html*</span></span>

<span data-ttu-id="bfb36-307">下列程式碼可啟用靜態檔案、預設檔案和 `MyStaticFiles` 的目錄瀏覽功能：</span><span class="sxs-lookup"><span data-stu-id="bfb36-307">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="bfb36-308">當 `EnableDirectoryBrowsing` 屬性值是 `true` 時，必須呼叫 `AddDirectoryBrowser`：</span><span class="sxs-lookup"><span data-stu-id="bfb36-308">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="bfb36-309">使用檔案階層和上述程式碼時，URL 解析方式如下：</span><span class="sxs-lookup"><span data-stu-id="bfb36-309">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="bfb36-310">URI</span><span class="sxs-lookup"><span data-stu-id="bfb36-310">URI</span></span>            |                             <span data-ttu-id="bfb36-311">回應</span><span class="sxs-lookup"><span data-stu-id="bfb36-311">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="bfb36-312">*HTTP:// \<server_address> /StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="bfb36-312">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="bfb36-313">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="bfb36-313">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="bfb36-314">*HTTP:// \<server_address> /StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="bfb36-314">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="bfb36-315">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="bfb36-315">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="bfb36-316">如果*MyStaticFiles*目錄中沒有任何預設名稱的檔案存在， *HTTP:// \<server_address> /StaticFiles*會傳回具有可按式連結的目錄清單：</span><span class="sxs-lookup"><span data-stu-id="bfb36-316">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![靜態檔案清單](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="bfb36-318"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> 和 <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> 會從 `http://{SERVER ADDRESS}/StaticFiles` (不含尾端斜線) 執行用戶端重新導向至 `http://{SERVER ADDRESS}/StaticFiles/` (含尾端斜線)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-318"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> and <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> perform a client-side redirect from `http://{SERVER ADDRESS}/StaticFiles` (without a trailing slash) to `http://{SERVER ADDRESS}/StaticFiles/` (with a trailing slash).</span></span> <span data-ttu-id="bfb36-319">*StaticFiles* 目錄內的相對 URL 若未含尾端斜線則會被視為無效。</span><span class="sxs-lookup"><span data-stu-id="bfb36-319">Relative URLs within the *StaticFiles* directory are invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="bfb36-320">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="bfb36-320">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="bfb36-321">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) 類別包含 `Mappings` 屬性以作為 MIME 內容類型的副檔名對應。</span><span class="sxs-lookup"><span data-stu-id="bfb36-321">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="bfb36-322">在下列範例中，已有數個副檔名註冊為已知的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="bfb36-322">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="bfb36-323">已取代 *.rtf* 副檔名，並已移除 *.mp4*。</span><span class="sxs-lookup"><span data-stu-id="bfb36-323">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="bfb36-324">請參閱 [MIME 內容類型](https://www.iana.org/assignments/media-types/media-types.xhtml)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-324">See [MIME content types](https://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="bfb36-325">非標準的內容類型</span><span class="sxs-lookup"><span data-stu-id="bfb36-325">Non-standard content types</span></span>

<span data-ttu-id="bfb36-326">靜態檔案中介軟體可理解幾乎 400 種已知的檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="bfb36-326">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="bfb36-327">如果使用者要求具有未知檔案類型的檔案，則靜態檔案中介軟體會將該要求傳遞至管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="bfb36-327">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="bfb36-328">如果沒有中介軟體處理要求，則會傳回「404 找不到」\*\* 回應。</span><span class="sxs-lookup"><span data-stu-id="bfb36-328">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="bfb36-329">如果啟用目錄瀏覽功能，則會在目錄清單中顯示檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="bfb36-329">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="bfb36-330">下列程式碼可讓您提供未知的類型，並會將未知的檔案轉譯為影像：</span><span class="sxs-lookup"><span data-stu-id="bfb36-330">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="bfb36-331">使用上述程式碼時，系統會以影像來回應具有未知內容類型的檔案要求。</span><span class="sxs-lookup"><span data-stu-id="bfb36-331">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="bfb36-332">啟用 [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) 時會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="bfb36-332">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="bfb36-333">預設為停用，亦不建議您使用。</span><span class="sxs-lookup"><span data-stu-id="bfb36-333">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="bfb36-334">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) 可提供更安全的替代方法，來提供非標準副檔名的檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-334">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

## <a name="serve-files-from-multiple-locations"></a><span data-ttu-id="bfb36-335">提供多個位置的檔案</span><span class="sxs-lookup"><span data-stu-id="bfb36-335">Serve files from multiple locations</span></span>

<span data-ttu-id="bfb36-336">`UseStaticFiles`和 `UseFileServer` 預設為指向*wwwroot*的檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="bfb36-336">`UseStaticFiles` and `UseFileServer` defaults to the file provider pointing at *wwwroot*.</span></span> <span data-ttu-id="bfb36-337">您可以提供其他的實例 `UseStaticFiles` 和 `UseFileServer` 其他檔案提供者，以從其他位置處理檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-337">You can provide additional instances of `UseStaticFiles` and `UseFileServer` with other file providers to serve files from other locations.</span></span> <span data-ttu-id="bfb36-338">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/15578)。</span><span class="sxs-lookup"><span data-stu-id="bfb36-338">For more information, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/15578).</span></span>

### <a name="considerations"></a><span data-ttu-id="bfb36-339">考量</span><span class="sxs-lookup"><span data-stu-id="bfb36-339">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="bfb36-340">`UseDirectoryBrowser` 和 `UseStaticFiles` 可能會導致洩漏祕密。</span><span class="sxs-lookup"><span data-stu-id="bfb36-340">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="bfb36-341">強烈建議您在生產環境中停用目錄瀏覽功能。</span><span class="sxs-lookup"><span data-stu-id="bfb36-341">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="bfb36-342">透過 `UseStaticFiles` 或 `UseDirectoryBrowser`，仔細檢閱要啟用哪些目錄。</span><span class="sxs-lookup"><span data-stu-id="bfb36-342">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="bfb36-343">因為整個目錄和其子目錄都可供公開存取。</span><span class="sxs-lookup"><span data-stu-id="bfb36-343">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="bfb36-344">儲存適用于在專用目錄中提供給公用的檔案，例如\* \<content_root> /wwwroot\*。</span><span class="sxs-lookup"><span data-stu-id="bfb36-344">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="bfb36-345">將這些檔案與 MVC views、 Razor Pages （僅限2.x）、設定檔等隔開。</span><span class="sxs-lookup"><span data-stu-id="bfb36-345">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="bfb36-346">使用 `UseDirectoryBrowser` 和 `UseStaticFiles` 公開內容的 URL 可能有區分大小寫，並受限於基礎檔案系統的字元限制。</span><span class="sxs-lookup"><span data-stu-id="bfb36-346">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="bfb36-347">例如，Windows 不區分大小寫&mdash;macOS 和 Linux 則區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="bfb36-347">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="bfb36-348">裝載於 IIS 中的 ASP.NET Core 應用程式會使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，將所有要求轉送給應用程式 (包括靜態檔案要求)，</span><span class="sxs-lookup"><span data-stu-id="bfb36-348">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="bfb36-349">而不會使用 IIS 靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="bfb36-349">The IIS static file handler isn't used.</span></span> <span data-ttu-id="bfb36-350">因此，上述處理常式不可能在模組處理要求之前處理要求。</span><span class="sxs-lookup"><span data-stu-id="bfb36-350">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="bfb36-351">請完成 IIS 管理員中的下列步驟，以移除伺服器或網站層級的 IIS 靜態檔案處理常式：</span><span class="sxs-lookup"><span data-stu-id="bfb36-351">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="bfb36-352">巡覽至 [模組]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="bfb36-352">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="bfb36-353">選取清單中的 **StaticFileModule**。</span><span class="sxs-lookup"><span data-stu-id="bfb36-353">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="bfb36-354">按一下 [動作]\*\*\*\* 側邊欄中的 [移除]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="bfb36-354">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="bfb36-355">如果已啟用 IIS 靜態檔案處理常式，**但是**未正確設定 ASP.NET Core 模組，仍可提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="bfb36-355">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="bfb36-356">舉例來說，未部署 *web.config* 檔案時可能會發生上述情況。</span><span class="sxs-lookup"><span data-stu-id="bfb36-356">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="bfb36-357">將程式碼檔案（包括 *.cs*和*cshtml*）放在應用程式專案的[web 根目錄](xref:fundamentals/index#web-root)外部。</span><span class="sxs-lookup"><span data-stu-id="bfb36-357">Place code files (including *.cs* and *.cshtml*) outside of the app project's [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="bfb36-358">如此一來，即會建立應用程式的用戶端內容與伺服器端程式碼之間的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="bfb36-358">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="bfb36-359">這樣可以防止伺服器端程式碼外洩。</span><span class="sxs-lookup"><span data-stu-id="bfb36-359">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bfb36-360">其他資源</span><span class="sxs-lookup"><span data-stu-id="bfb36-360">Additional resources</span></span>

* [<span data-ttu-id="bfb36-361">中介軟體</span><span class="sxs-lookup"><span data-stu-id="bfb36-361">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="bfb36-362">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="bfb36-362">Introduction to ASP.NET Core</span></span>](xref:index)

::: moniker-end
