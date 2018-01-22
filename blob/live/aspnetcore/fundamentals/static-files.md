---
title: "使用 ASP.NET Core 中的靜態檔案"
author: rick-anderson
description: "了解如何做及安全靜態檔案，並設定裝載的 ASP.NET Core web 應用程式中的中介軟體行為的靜態檔案。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 60b205bf0a45e2965f9dab46f46956947ae513fd
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="da631-103">使用 ASP.NET Core 中的靜態檔案</span><span class="sxs-lookup"><span data-stu-id="da631-103">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="da631-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="da631-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="da631-105">靜態檔案，例如 HTML、 CSS、 影像和 JavaScript 中，是 ASP.NET Core 應用程式提供直接給用戶端的資產。</span><span class="sxs-lookup"><span data-stu-id="da631-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="da631-106">若要讓這些檔案的處理將需要一些設定。</span><span class="sxs-lookup"><span data-stu-id="da631-106">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="da631-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="da631-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="da631-108">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="da631-108">Serve static files</span></span>

<span data-ttu-id="da631-109">靜態檔案會儲存在專案的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="da631-109">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="da631-110">預設目錄是 *\<content_root > / wwwroot*，但是可以透過變更[UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)方法。</span><span class="sxs-lookup"><span data-stu-id="da631-110">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="da631-111">請參閱[內容的根](xref:fundamentals/index#content-root)和[Web 根目錄](xref:fundamentals/index#web-root)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="da631-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="da631-112">應用程式的 web 主機必須察覺之內容的根目錄。</span><span class="sxs-lookup"><span data-stu-id="da631-112">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da631-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da631-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da631-114">`WebHost.CreateDefaultBuilder`方法設定為目前的目錄內容的根：</span><span class="sxs-lookup"><span data-stu-id="da631-114">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da631-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da631-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da631-116">設定為目前的目錄內容的根，藉由叫用[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)內`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="da631-116">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="da631-117">靜態檔案會存取透過 web 根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="da631-117">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="da631-118">例如， **Web 應用程式**專案範本包含數個資料夾內*wwwroot*資料夾：</span><span class="sxs-lookup"><span data-stu-id="da631-118">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="da631-119">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="da631-119">**wwwroot**</span></span>
  * <span data-ttu-id="da631-120">**css**</span><span class="sxs-lookup"><span data-stu-id="da631-120">**css**</span></span>
  * <span data-ttu-id="da631-121">**images**</span><span class="sxs-lookup"><span data-stu-id="da631-121">**images**</span></span>
  * <span data-ttu-id="da631-122">**js**</span><span class="sxs-lookup"><span data-stu-id="da631-122">**js**</span></span>

<span data-ttu-id="da631-123">若要存取的檔案中的 URI 格式*映像*子資料夾位於*http://\<server_address > /images/\<image_file_name >*。</span><span class="sxs-lookup"><span data-stu-id="da631-123">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="da631-124">例如， *http://localhost:9189/images/banner3.svg*。</span><span class="sxs-lookup"><span data-stu-id="da631-124">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="da631-125">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="da631-125">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="da631-126">如果目標.NET Framework，加入[Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="da631-126">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="da631-127">如果目標.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)包含此套件。</span><span class="sxs-lookup"><span data-stu-id="da631-127">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="da631-128">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="da631-128">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="da631-129">新增[Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="da631-129">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="da631-130">設定[中介軟體](xref:fundamentals/middleware)讓靜態檔案處理。</span><span class="sxs-lookup"><span data-stu-id="da631-130">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="da631-131">支援在 web 根目錄內的檔案</span><span class="sxs-lookup"><span data-stu-id="da631-131">Serve files inside of web root</span></span>

<span data-ttu-id="da631-132">叫用[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)方法內`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="da631-132">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="da631-133">無參數`UseStaticFiles`方法多載會將標示為 servable web 根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="da631-133">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="da631-134">下列標記參考*wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="da631-134">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="da631-135">支援 web 根目錄之外的檔案</span><span class="sxs-lookup"><span data-stu-id="da631-135">Serve files outside of web root</span></span>

<span data-ttu-id="da631-136">請考慮靜態檔案提供服務所在的 web 根目錄之外的目錄階層：</span><span class="sxs-lookup"><span data-stu-id="da631-136">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="da631-137">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="da631-137">**wwwroot**</span></span>
  * <span data-ttu-id="da631-138">**css**</span><span class="sxs-lookup"><span data-stu-id="da631-138">**css**</span></span>
  * <span data-ttu-id="da631-139">**images**</span><span class="sxs-lookup"><span data-stu-id="da631-139">**images**</span></span>
  * <span data-ttu-id="da631-140">**js**</span><span class="sxs-lookup"><span data-stu-id="da631-140">**js**</span></span>
* <span data-ttu-id="da631-141">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="da631-141">**MyStaticFiles**</span></span>
  * <span data-ttu-id="da631-142">**images**</span><span class="sxs-lookup"><span data-stu-id="da631-142">**images**</span></span>
      * <span data-ttu-id="da631-143">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="da631-143">*banner1.svg*</span></span>

<span data-ttu-id="da631-144">要求可以存取*banner1.svg*檔案所設定的靜態檔案中介軟體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="da631-144">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="da631-145">在上述程式碼， *MyStaticFiles*目錄階層會公開透過*StaticFiles* URI 區段。</span><span class="sxs-lookup"><span data-stu-id="da631-145">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="da631-146">若要要求*http://\<server_address > /StaticFiles/images/banner1.svg*做*banner1.svg*檔案。</span><span class="sxs-lookup"><span data-stu-id="da631-146">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="da631-147">下列標記參考*MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="da631-147">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="da631-148">設定 HTTP 回應標頭</span><span class="sxs-lookup"><span data-stu-id="da631-148">Set HTTP response headers</span></span>

<span data-ttu-id="da631-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions)物件可以用來設定 HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="da631-149">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="da631-150">除了設定靜態檔案處理，從 web 根目錄下，下列程式碼設定`Cache-Control`標頭：</span><span class="sxs-lookup"><span data-stu-id="da631-150">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="da631-151">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append)方法存在於[Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/)封裝。</span><span class="sxs-lookup"><span data-stu-id="da631-151">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="da631-152">檔案已公開可快取為 10 分鐘 （600 秒）：</span><span class="sxs-lookup"><span data-stu-id="da631-152">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![已加入顯示的快取控制標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="da631-154">靜態檔案授權</span><span class="sxs-lookup"><span data-stu-id="da631-154">Static file authorization</span></span>

<span data-ttu-id="da631-155">靜態檔案中介軟體並不提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="da631-155">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="da631-156">任何檔案由提供服務，包括下*wwwroot*，是可公開存取。</span><span class="sxs-lookup"><span data-stu-id="da631-156">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="da631-157">若要提供檔案為基礎的授權：</span><span class="sxs-lookup"><span data-stu-id="da631-157">To serve files based on authorization:</span></span>

* <span data-ttu-id="da631-158">儲存它們的外部*wwwroot*和可存取靜態檔案中介軟體的任何目錄**和**</span><span class="sxs-lookup"><span data-stu-id="da631-158">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="da631-159">提供透過授權套用到動作方法。</span><span class="sxs-lookup"><span data-stu-id="da631-159">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="da631-160">傳回[FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult)物件：</span><span class="sxs-lookup"><span data-stu-id="da631-160">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="da631-161">啟用目錄瀏覽</span><span class="sxs-lookup"><span data-stu-id="da631-161">Enable directory browsing</span></span>

<span data-ttu-id="da631-162">瀏覽目錄可讓您 web 應用程式的使用者看見的目錄清單，而且檔案內指定的目錄。</span><span class="sxs-lookup"><span data-stu-id="da631-162">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="da631-163">瀏覽目錄已停用預設基於安全性考量 (請參閱[考量](#considerations))。</span><span class="sxs-lookup"><span data-stu-id="da631-163">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="da631-164">啟用目錄瀏覽叫用[UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_)方法中的`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="da631-164">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="da631-165">加入所需的服務所叫用[AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_)方法從`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="da631-165">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="da631-166">上述程式碼讓您瀏覽目錄*wwwroot/影像*資料夾使用的 URL *http://\<server_address > / MyImages*，每個檔案和資料夾的連結：</span><span class="sxs-lookup"><span data-stu-id="da631-166">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![瀏覽目錄](static-files/_static/dir-browse.png)

<span data-ttu-id="da631-168">請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。</span><span class="sxs-lookup"><span data-stu-id="da631-168">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="da631-169">請注意兩個`UseStaticFiles`在下列範例會呼叫。</span><span class="sxs-lookup"><span data-stu-id="da631-169">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="da631-170">第一次呼叫可讓靜態檔案中的服務*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="da631-170">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="da631-171">第二次呼叫可讓您啟用目錄瀏覽的*wwwroot/影像*資料夾使用的 URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="da631-171">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="da631-172">預設文件</span><span class="sxs-lookup"><span data-stu-id="da631-172">Serve a default document</span></span>

<span data-ttu-id="da631-173">設定預設的首頁提供訪客的邏輯起點造訪您的網站時。</span><span class="sxs-lookup"><span data-stu-id="da631-173">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="da631-174">若要完整限定的 URI，使用者不提供預設的網頁，呼叫[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)方法從`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="da631-174">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="da631-175">`UseDefaultFiles`之前必須先呼叫`UseStaticFiles`提供預設的檔案。</span><span class="sxs-lookup"><span data-stu-id="da631-175">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="da631-176">`UseDefaultFiles`是實際上並不提供此檔案的 URL 重寫器。</span><span class="sxs-lookup"><span data-stu-id="da631-176">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="da631-177">啟用透過靜態檔案中的介軟體`UseStaticFiles`來提供此檔案。</span><span class="sxs-lookup"><span data-stu-id="da631-177">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="da631-178">與`UseDefaultFiles`，資料夾中搜尋的要求：</span><span class="sxs-lookup"><span data-stu-id="da631-178">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="da631-179">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="da631-179">*default.htm*</span></span>
* <span data-ttu-id="da631-180">*default.html*</span><span class="sxs-lookup"><span data-stu-id="da631-180">*default.html*</span></span>
* <span data-ttu-id="da631-181">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="da631-181">*index.htm*</span></span>
* <span data-ttu-id="da631-182">*index.html*</span><span class="sxs-lookup"><span data-stu-id="da631-182">*index.html*</span></span>

<span data-ttu-id="da631-183">從清單中找到的第一個檔案是提供服務，即使該要求的完整格式的 URI。</span><span class="sxs-lookup"><span data-stu-id="da631-183">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="da631-184">在瀏覽器 URL 會繼續反映出要求的 URI。</span><span class="sxs-lookup"><span data-stu-id="da631-184">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="da631-185">下列程式碼變更的預設檔案名稱*mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="da631-185">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="da631-186">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="da631-186">UseFileServer</span></span>

<span data-ttu-id="da631-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_)結合的功能`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`。</span><span class="sxs-lookup"><span data-stu-id="da631-187">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="da631-188">下列程式碼可讓靜態檔案和預設的檔案服務。</span><span class="sxs-lookup"><span data-stu-id="da631-188">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="da631-189">瀏覽目錄未啟用。</span><span class="sxs-lookup"><span data-stu-id="da631-189">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="da631-190">下列程式碼會根據透過啟用目錄瀏覽的無參數多載：</span><span class="sxs-lookup"><span data-stu-id="da631-190">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="da631-191">請考慮下列的目錄階層：</span><span class="sxs-lookup"><span data-stu-id="da631-191">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="da631-192">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="da631-192">**wwwroot**</span></span>
  * <span data-ttu-id="da631-193">**css**</span><span class="sxs-lookup"><span data-stu-id="da631-193">**css**</span></span>
  * <span data-ttu-id="da631-194">**images**</span><span class="sxs-lookup"><span data-stu-id="da631-194">**images**</span></span>
  * <span data-ttu-id="da631-195">**js**</span><span class="sxs-lookup"><span data-stu-id="da631-195">**js**</span></span>
* <span data-ttu-id="da631-196">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="da631-196">**MyStaticFiles**</span></span>
  * <span data-ttu-id="da631-197">**images**</span><span class="sxs-lookup"><span data-stu-id="da631-197">**images**</span></span>
      * <span data-ttu-id="da631-198">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="da631-198">*banner1.svg*</span></span>
  * <span data-ttu-id="da631-199">*default.html*</span><span class="sxs-lookup"><span data-stu-id="da631-199">*default.html*</span></span>

<span data-ttu-id="da631-200">下列程式碼可啟用靜態檔案、 預設檔案和目錄瀏覽的`MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="da631-200">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="da631-201">`AddDirectoryBrowser`時，必須呼叫`EnableDirectoryBrowsing`屬性值是`true`:</span><span class="sxs-lookup"><span data-stu-id="da631-201">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="da631-202">Url 使用的檔案階層和之前程式碼，解決，如下所示：</span><span class="sxs-lookup"><span data-stu-id="da631-202">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="da631-203">URI</span><span class="sxs-lookup"><span data-stu-id="da631-203">URI</span></span>            |                             <span data-ttu-id="da631-204">回應</span><span class="sxs-lookup"><span data-stu-id="da631-204">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="da631-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="da631-205">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="da631-206">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="da631-206">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="da631-207">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="da631-207">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="da631-208">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="da631-208">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="da631-209">如果沒有預設值命名的檔案存在於*MyStaticFiles*目錄， *http://\<server_address > / StaticFiles*傳回目錄的可點按的連結清單：</span><span class="sxs-lookup"><span data-stu-id="da631-209">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![靜態檔案清單](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="da631-211">`UseDefaultFiles`和`UseDirectoryBrowser`使用 URL *http://\<server_address > / StaticFiles*沒有尾端斜線，以觸發用戶端重新導向至*http://\<server_address > /StaticFiles /*。</span><span class="sxs-lookup"><span data-stu-id="da631-211">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="da631-212">請注意結尾的斜線的加法。</span><span class="sxs-lookup"><span data-stu-id="da631-212">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="da631-213">文件中的相對 Url 會被視為無效，沒有尾端斜線。</span><span class="sxs-lookup"><span data-stu-id="da631-213">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="da631-214">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="da631-214">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="da631-215">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider)類別包含`Mappings`屬性做為副檔名的 MIME 內容類型的對應。</span><span class="sxs-lookup"><span data-stu-id="da631-215">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="da631-216">在下列範例中，數個檔案延伸模組已登錄到已知的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="da631-216">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="da631-217">*.Rtf*取代擴充功能，和*.mp4*已移除。</span><span class="sxs-lookup"><span data-stu-id="da631-217">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="da631-218">請參閱[MIME 內容類型](http://www.iana.org/assignments/media-types/media-types.xhtml)。</span><span class="sxs-lookup"><span data-stu-id="da631-218">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="da631-219">非標準的內容類型</span><span class="sxs-lookup"><span data-stu-id="da631-219">Non-standard content types</span></span>

<span data-ttu-id="da631-220">靜態檔案中介軟體可理解幾乎 400 已知的檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="da631-220">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="da631-221">如果使用者要求未知的檔案類型的檔案時，靜態檔案中介軟體會傳回 HTTP 404 （找不到） 回應。</span><span class="sxs-lookup"><span data-stu-id="da631-221">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="da631-222">如果啟用目錄瀏覽時，會顯示檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="da631-222">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="da631-223">URI 會傳回 HTTP 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="da631-223">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="da631-224">下列程式碼可讓處理未知的類型，並轉譯為影像未知的檔案：</span><span class="sxs-lookup"><span data-stu-id="da631-224">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="da631-225">使用上述的程式碼，具有未知的內容類型的檔案的要求會傳回做為映像。</span><span class="sxs-lookup"><span data-stu-id="da631-225">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="da631-226">啟用[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes)會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="da631-226">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="da631-227">根據預設，已停用，不建議使用它。</span><span class="sxs-lookup"><span data-stu-id="da631-227">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="da631-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider)提供更安全的替代方法，來提供檔案與非標準擴充功能。</span><span class="sxs-lookup"><span data-stu-id="da631-228">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="da631-229">考量</span><span class="sxs-lookup"><span data-stu-id="da631-229">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="da631-230">`UseDirectoryBrowser`和`UseStaticFiles`可能會導致洩漏機密資料。</span><span class="sxs-lookup"><span data-stu-id="da631-230">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="da631-231">強烈建議在生產環境中瀏覽的停用目錄。</span><span class="sxs-lookup"><span data-stu-id="da631-231">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="da631-232">請仔細檢閱 哪個目錄中已啟用透過`UseStaticFiles`或`UseDirectoryBrowser`。</span><span class="sxs-lookup"><span data-stu-id="da631-232">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="da631-233">整個目錄以及目錄及其子目錄會變成可公開存取。</span><span class="sxs-lookup"><span data-stu-id="da631-233">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="da631-234">存放區中的檔案適用於公開服務專用的目錄，例如 *\<content_root > / wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="da631-234">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="da631-235">這些檔案分開 MVC 檢視，Razor 頁面 (2.x 僅)，設定檔、 等等。</span><span class="sxs-lookup"><span data-stu-id="da631-235">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="da631-236">使用公開內容的 Url`UseDirectoryBrowser`和`UseStaticFiles`可能會有區分大小寫，基礎檔案系統的字元限制。</span><span class="sxs-lookup"><span data-stu-id="da631-236">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="da631-237">例如，Windows 不區分&mdash;Mac 和 Linux 不是。</span><span class="sxs-lookup"><span data-stu-id="da631-237">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="da631-238">ASP.NET Core 應用程式裝載於 IIS 使用[ASP.NET 核心模組 (ANCM)](xref:fundamentals/servers/aspnet-core-module)轉送至應用程式，包括靜態檔案要求的所有要求。</span><span class="sxs-lookup"><span data-stu-id="da631-238">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="da631-239">不會使用 IIS 靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="da631-239">The IIS static file handler isn't used.</span></span> <span data-ttu-id="da631-240">它有沒有機會之前由 ANCM 處理要求。</span><span class="sxs-lookup"><span data-stu-id="da631-240">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="da631-241">完成下列步驟在 IIS 管理員移除 IIS 靜態檔案處理常式，在伺服器或網站層級：</span><span class="sxs-lookup"><span data-stu-id="da631-241">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="da631-242">瀏覽至**模組**功能。</span><span class="sxs-lookup"><span data-stu-id="da631-242">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="da631-243">選取**StaticFileModule**清單中。</span><span class="sxs-lookup"><span data-stu-id="da631-243">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="da631-244">按一下**移除**中**動作**[資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="da631-244">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="da631-245">如果已啟用 IIS 靜態檔案處理常式**和**ANCM 設定不正確、 靜態檔案都有受到處理。</span><span class="sxs-lookup"><span data-stu-id="da631-245">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="da631-246">發生這種情況，例如，如果*web.config*檔案未部署。</span><span class="sxs-lookup"><span data-stu-id="da631-246">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="da631-247">程式碼檔案 (包括*.cs*和*.cshtml*) 應用程式專案的 web 根目錄之外。</span><span class="sxs-lookup"><span data-stu-id="da631-247">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="da631-248">邏輯分離，因此會建立應用程式的用戶端的內容與伺服器端程式碼之間。</span><span class="sxs-lookup"><span data-stu-id="da631-248">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="da631-249">這可防止伺服器端程式碼外洩。</span><span class="sxs-lookup"><span data-stu-id="da631-249">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="da631-250">其他資源</span><span class="sxs-lookup"><span data-stu-id="da631-250">Additional resources</span></span>

* [<span data-ttu-id="da631-251">中介軟體</span><span class="sxs-lookup"><span data-stu-id="da631-251">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="da631-252">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="da631-252">Introduction to ASP.NET Core</span></span>](xref:index)
