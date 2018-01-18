---
title: "使用 ASP.NET Core 中的靜態檔案"
author: rick-anderson
description: "了解如何做及安全靜態檔案，並設定裝載的 ASP.NET Core web 應用程式中的中介軟體行為的靜態檔案。"
keywords: "ASP.NET Core 靜態檔案、 HTML、 CSS、 JavaScript 靜態資產"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 01/18/2018
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/static-files
ms.openlocfilehash: 912923860939a1d1dd91ccc79862e23f9095d161
ms.sourcegitcommit: a3e88639a6bcf8fb4d634036dac93130c464a097
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2018
---
# <a name="work-with-static-files-in-aspnet-core"></a><span data-ttu-id="1379b-104">使用 ASP.NET Core 中的靜態檔案</span><span class="sxs-lookup"><span data-stu-id="1379b-104">Work with static files in ASP.NET Core</span></span>

<span data-ttu-id="1379b-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)和[Scott Addie](https://twitter.com/Scott_Addie)</span><span class="sxs-lookup"><span data-stu-id="1379b-105">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="1379b-106">靜態檔案，例如 HTML、 CSS、 影像和 JavaScript 中，是 ASP.NET Core 應用程式提供直接給用戶端的資產。</span><span class="sxs-lookup"><span data-stu-id="1379b-106">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="1379b-107">若要讓這些檔案的處理將需要一些設定。</span><span class="sxs-lookup"><span data-stu-id="1379b-107">Some configuration is required to enable to serving of these files.</span></span>

<span data-ttu-id="1379b-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1379b-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="1379b-109">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="1379b-109">Serve static files</span></span>

<span data-ttu-id="1379b-110">靜態檔案會儲存在專案的 web 根目錄。</span><span class="sxs-lookup"><span data-stu-id="1379b-110">Static files are stored within your project's web root directory.</span></span> <span data-ttu-id="1379b-111">預設目錄是 *\<content_root > / wwwroot*，但是可以透過變更[UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)方法。</span><span class="sxs-lookup"><span data-stu-id="1379b-111">The default directory is *\<content_root>/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="1379b-112">請參閱[內容的根](xref:fundamentals/index#content-root)和[Web 根目錄](xref:fundamentals/index#web-root)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="1379b-112">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="1379b-113">應用程式的 web 主機必須察覺之內容的根目錄。</span><span class="sxs-lookup"><span data-stu-id="1379b-113">The app's web host must be made aware of the content root directory.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1379b-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1379b-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1379b-115">`WebHost.CreateDefaultBuilder`方法設定為目前的目錄內容的根：</span><span class="sxs-lookup"><span data-stu-id="1379b-115">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1379b-116">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1379b-116">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1379b-117">設定為目前的目錄內容的根，藉由叫用[UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)內`Program.Main`:</span><span class="sxs-lookup"><span data-stu-id="1379b-117">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

---

<span data-ttu-id="1379b-118">靜態檔案會存取透過 web 根目錄的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="1379b-118">Static files are accessible via a path relative to the web root.</span></span> <span data-ttu-id="1379b-119">例如， **Web 應用程式**專案範本包含數個資料夾內*wwwroot*資料夾：</span><span class="sxs-lookup"><span data-stu-id="1379b-119">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="1379b-120">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="1379b-120">**wwwroot**</span></span>
  * <span data-ttu-id="1379b-121">**css**</span><span class="sxs-lookup"><span data-stu-id="1379b-121">**css**</span></span>
  * <span data-ttu-id="1379b-122">**images**</span><span class="sxs-lookup"><span data-stu-id="1379b-122">**images**</span></span>
  * <span data-ttu-id="1379b-123">**js**</span><span class="sxs-lookup"><span data-stu-id="1379b-123">**js**</span></span>

<span data-ttu-id="1379b-124">若要存取的檔案中的 URI 格式*映像*子資料夾位於*http://\<server_address > /images/\<image_file_name >*。</span><span class="sxs-lookup"><span data-stu-id="1379b-124">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="1379b-125">例如， *http://localhost:9189/images/banner3.svg*。</span><span class="sxs-lookup"><span data-stu-id="1379b-125">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="1379b-126">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="1379b-126">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="1379b-127">如果目標.NET Framework，加入[Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="1379b-127">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span> <span data-ttu-id="1379b-128">如果目標.NET Core [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage)包含此套件。</span><span class="sxs-lookup"><span data-stu-id="1379b-128">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="1379b-129">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="1379b-129">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="1379b-130">新增[Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/)封裝至您的專案。</span><span class="sxs-lookup"><span data-stu-id="1379b-130">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to your project.</span></span>

---

<span data-ttu-id="1379b-131">設定[中介軟體](xref:fundamentals/middleware)讓靜態檔案處理。</span><span class="sxs-lookup"><span data-stu-id="1379b-131">Configure the [middleware](xref:fundamentals/middleware) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="1379b-132">支援在 web 根目錄內的檔案</span><span class="sxs-lookup"><span data-stu-id="1379b-132">Serve files inside of web root</span></span>

<span data-ttu-id="1379b-133">叫用[UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)方法內`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1379b-133">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="1379b-134">無參數`UseStaticFiles`方法多載會將標示為 servable web 根目錄中的檔案。</span><span class="sxs-lookup"><span data-stu-id="1379b-134">The parameterless `UseStaticFiles` method overload marks the files in web root as servable.</span></span> <span data-ttu-id="1379b-135">下列標記參考*wwwroot/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="1379b-135">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="1379b-136">支援 web 根目錄之外的檔案</span><span class="sxs-lookup"><span data-stu-id="1379b-136">Serve files outside of web root</span></span>

<span data-ttu-id="1379b-137">請考慮靜態檔案提供服務所在的 web 根目錄之外的目錄階層：</span><span class="sxs-lookup"><span data-stu-id="1379b-137">Consider a directory hierarchy in which the static files to be served reside outside of the web root:</span></span>

* <span data-ttu-id="1379b-138">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="1379b-138">**wwwroot**</span></span>
  * <span data-ttu-id="1379b-139">**css**</span><span class="sxs-lookup"><span data-stu-id="1379b-139">**css**</span></span>
  * <span data-ttu-id="1379b-140">**images**</span><span class="sxs-lookup"><span data-stu-id="1379b-140">**images**</span></span>
  * <span data-ttu-id="1379b-141">**js**</span><span class="sxs-lookup"><span data-stu-id="1379b-141">**js**</span></span>
* <span data-ttu-id="1379b-142">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="1379b-142">**MyStaticFiles**</span></span>
  * <span data-ttu-id="1379b-143">**images**</span><span class="sxs-lookup"><span data-stu-id="1379b-143">**images**</span></span>
      * <span data-ttu-id="1379b-144">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="1379b-144">*banner1.svg*</span></span>

<span data-ttu-id="1379b-145">要求可以存取*banner1.svg*檔案所設定的靜態檔案中介軟體，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1379b-145">A request can access the *banner1.svg* file by configuring the static file middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="1379b-146">在上述程式碼， *MyStaticFiles*目錄階層會公開透過*StaticFiles* URI 區段。</span><span class="sxs-lookup"><span data-stu-id="1379b-146">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="1379b-147">若要要求*http://\<server_address > /StaticFiles/images/banner1.svg*做*banner1.svg*檔案。</span><span class="sxs-lookup"><span data-stu-id="1379b-147">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="1379b-148">下列標記參考*MyStaticFiles/images/banner1.svg*:</span><span class="sxs-lookup"><span data-stu-id="1379b-148">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="1379b-149">設定 HTTP 回應標頭</span><span class="sxs-lookup"><span data-stu-id="1379b-149">Set HTTP response headers</span></span>

<span data-ttu-id="1379b-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions)物件可以用來設定 HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="1379b-150">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="1379b-151">除了設定靜態檔案處理，從 web 根目錄下，下列程式碼設定`Cache-Control`標頭：</span><span class="sxs-lookup"><span data-stu-id="1379b-151">In addition to configuring static file serving from the web root, the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="1379b-152">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append)方法存在於[Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/)封裝。</span><span class="sxs-lookup"><span data-stu-id="1379b-152">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="1379b-153">檔案已公開可快取為 10 分鐘 （600 秒）：</span><span class="sxs-lookup"><span data-stu-id="1379b-153">The files have been made publicly cacheable for 10 minutes (600 seconds):</span></span>

![已加入顯示的快取控制標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="1379b-155">靜態檔案授權</span><span class="sxs-lookup"><span data-stu-id="1379b-155">Static file authorization</span></span>

<span data-ttu-id="1379b-156">靜態檔案中介軟體並不提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="1379b-156">The static file middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="1379b-157">任何檔案由提供服務，包括下*wwwroot*，是可公開存取。</span><span class="sxs-lookup"><span data-stu-id="1379b-157">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="1379b-158">若要提供檔案為基礎的授權：</span><span class="sxs-lookup"><span data-stu-id="1379b-158">To serve files based on authorization:</span></span>

* <span data-ttu-id="1379b-159">儲存它們的外部*wwwroot*和可存取靜態檔案中介軟體的任何目錄**和**</span><span class="sxs-lookup"><span data-stu-id="1379b-159">Store them outside of *wwwroot* and any directory accessible to the static file middleware **and**</span></span>
* <span data-ttu-id="1379b-160">提供透過授權套用到動作方法。</span><span class="sxs-lookup"><span data-stu-id="1379b-160">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="1379b-161">傳回[FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult)物件：</span><span class="sxs-lookup"><span data-stu-id="1379b-161">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

[!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="1379b-162">啟用目錄瀏覽</span><span class="sxs-lookup"><span data-stu-id="1379b-162">Enable directory browsing</span></span>

<span data-ttu-id="1379b-163">瀏覽目錄可讓您 web 應用程式的使用者看見的目錄清單，而且檔案內指定的目錄。</span><span class="sxs-lookup"><span data-stu-id="1379b-163">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="1379b-164">瀏覽目錄已停用預設基於安全性考量 (請參閱[考量](#considerations))。</span><span class="sxs-lookup"><span data-stu-id="1379b-164">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="1379b-165">啟用目錄瀏覽叫用[UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_)方法中的`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1379b-165">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="1379b-166">加入所需的服務所叫用[AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_)方法從`Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="1379b-166">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="1379b-167">上述程式碼讓您瀏覽目錄*wwwroot/影像*資料夾使用的 URL *http://\<server_address > / MyImages*，每個檔案和資料夾的連結：</span><span class="sxs-lookup"><span data-stu-id="1379b-167">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![瀏覽目錄](static-files/_static/dir-browse.png)

<span data-ttu-id="1379b-169">請參閱[考量](#considerations)上啟用瀏覽時的安全性風險。</span><span class="sxs-lookup"><span data-stu-id="1379b-169">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="1379b-170">請注意兩個`UseStaticFiles`在下列範例會呼叫。</span><span class="sxs-lookup"><span data-stu-id="1379b-170">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="1379b-171">第一次呼叫可讓靜態檔案中的服務*wwwroot*資料夾。</span><span class="sxs-lookup"><span data-stu-id="1379b-171">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="1379b-172">第二次呼叫可讓您啟用目錄瀏覽的*wwwroot/影像*資料夾使用的 URL *http://\<server_address > / MyImages*:</span><span class="sxs-lookup"><span data-stu-id="1379b-172">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="1379b-173">預設文件</span><span class="sxs-lookup"><span data-stu-id="1379b-173">Serve a default document</span></span>

<span data-ttu-id="1379b-174">設定預設的首頁提供訪客的邏輯起點造訪您的網站時。</span><span class="sxs-lookup"><span data-stu-id="1379b-174">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="1379b-175">若要完整限定的 URI，使用者不提供預設的網頁，呼叫[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)方法從`Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="1379b-175">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="1379b-176">`UseDefaultFiles`之前必須先呼叫`UseStaticFiles`提供預設的檔案。</span><span class="sxs-lookup"><span data-stu-id="1379b-176">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="1379b-177">`UseDefaultFiles`是實際上並不提供此檔案的 URL 重寫器。</span><span class="sxs-lookup"><span data-stu-id="1379b-177">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="1379b-178">啟用透過靜態檔案中的介軟體`UseStaticFiles`來提供此檔案。</span><span class="sxs-lookup"><span data-stu-id="1379b-178">Enable the static file middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="1379b-179">與`UseDefaultFiles`，資料夾中搜尋的要求：</span><span class="sxs-lookup"><span data-stu-id="1379b-179">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="1379b-180">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="1379b-180">*default.htm*</span></span>
* <span data-ttu-id="1379b-181">*default.html*</span><span class="sxs-lookup"><span data-stu-id="1379b-181">*default.html*</span></span>
* <span data-ttu-id="1379b-182">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="1379b-182">*index.htm*</span></span>
* <span data-ttu-id="1379b-183">*index.html*</span><span class="sxs-lookup"><span data-stu-id="1379b-183">*index.html*</span></span>

<span data-ttu-id="1379b-184">從清單中找到的第一個檔案是提供服務，即使該要求的完整格式的 URI。</span><span class="sxs-lookup"><span data-stu-id="1379b-184">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="1379b-185">在瀏覽器 URL 會繼續反映出要求的 URI。</span><span class="sxs-lookup"><span data-stu-id="1379b-185">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="1379b-186">下列程式碼變更的預設檔案名稱*mydefault.html*:</span><span class="sxs-lookup"><span data-stu-id="1379b-186">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="1379b-187">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="1379b-187">UseFileServer</span></span>

<span data-ttu-id="1379b-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_)結合的功能`UseStaticFiles`， `UseDefaultFiles`，和`UseDirectoryBrowser`。</span><span class="sxs-lookup"><span data-stu-id="1379b-188">[UseFileServer](/dotnet/api/microsoft.aspnetcore.builder.fileserverextensions.usefileserver#Microsoft_AspNetCore_Builder_FileServerExtensions_UseFileServer_Microsoft_AspNetCore_Builder_IApplicationBuilder_) combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="1379b-189">下列程式碼可讓靜態檔案和預設的檔案服務。</span><span class="sxs-lookup"><span data-stu-id="1379b-189">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="1379b-190">瀏覽目錄未啟用。</span><span class="sxs-lookup"><span data-stu-id="1379b-190">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="1379b-191">下列程式碼會根據透過啟用目錄瀏覽的無參數多載：</span><span class="sxs-lookup"><span data-stu-id="1379b-191">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="1379b-192">請考慮下列的目錄階層：</span><span class="sxs-lookup"><span data-stu-id="1379b-192">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="1379b-193">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="1379b-193">**wwwroot**</span></span>
  * <span data-ttu-id="1379b-194">**css**</span><span class="sxs-lookup"><span data-stu-id="1379b-194">**css**</span></span>
  * <span data-ttu-id="1379b-195">**images**</span><span class="sxs-lookup"><span data-stu-id="1379b-195">**images**</span></span>
  * <span data-ttu-id="1379b-196">**js**</span><span class="sxs-lookup"><span data-stu-id="1379b-196">**js**</span></span>
* <span data-ttu-id="1379b-197">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="1379b-197">**MyStaticFiles**</span></span>
  * <span data-ttu-id="1379b-198">**images**</span><span class="sxs-lookup"><span data-stu-id="1379b-198">**images**</span></span>
      * <span data-ttu-id="1379b-199">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="1379b-199">*banner1.svg*</span></span>
  * <span data-ttu-id="1379b-200">*default.html*</span><span class="sxs-lookup"><span data-stu-id="1379b-200">*default.html*</span></span>

<span data-ttu-id="1379b-201">下列程式碼可啟用靜態檔案、 預設檔案和目錄瀏覽的`MyStaticFiles`:</span><span class="sxs-lookup"><span data-stu-id="1379b-201">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="1379b-202">`AddDirectoryBrowser`時，必須呼叫`EnableDirectoryBrowsing`屬性值是`true`:</span><span class="sxs-lookup"><span data-stu-id="1379b-202">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="1379b-203">Url 使用的檔案階層和之前程式碼，解決，如下所示：</span><span class="sxs-lookup"><span data-stu-id="1379b-203">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="1379b-204">URI</span><span class="sxs-lookup"><span data-stu-id="1379b-204">URI</span></span>            |                             <span data-ttu-id="1379b-205">回應</span><span class="sxs-lookup"><span data-stu-id="1379b-205">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="1379b-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="1379b-206">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="1379b-207">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="1379b-207">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="1379b-208">*http://\<server_address>/StaticFiles*</span><span class="sxs-lookup"><span data-stu-id="1379b-208">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="1379b-209">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="1379b-209">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="1379b-210">如果沒有預設值命名的檔案存在於*MyStaticFiles*目錄， *http://\<server_address > / StaticFiles*傳回目錄的可點按的連結清單：</span><span class="sxs-lookup"><span data-stu-id="1379b-210">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![靜態檔案清單](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="1379b-212">`UseDefaultFiles`和`UseDirectoryBrowser`使用 URL *http://\<server_address > / StaticFiles*沒有尾端斜線，以觸發用戶端重新導向至*http://\<server_address > /StaticFiles /*。</span><span class="sxs-lookup"><span data-stu-id="1379b-212">`UseDefaultFiles` and `UseDirectoryBrowser` use the URL *http://\<server_address>/StaticFiles* without the trailing slash to trigger a client-side redirect to *http://\<server_address>/StaticFiles/*.</span></span> <span data-ttu-id="1379b-213">請注意結尾的斜線的加法。</span><span class="sxs-lookup"><span data-stu-id="1379b-213">Notice the addition of the trailing slash.</span></span> <span data-ttu-id="1379b-214">文件中的相對 Url 會被視為無效，沒有尾端斜線。</span><span class="sxs-lookup"><span data-stu-id="1379b-214">Relative URLs within the documents are deemed invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="1379b-215">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="1379b-215">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="1379b-216">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider)類別包含`Mappings`屬性做為副檔名的 MIME 內容類型的對應。</span><span class="sxs-lookup"><span data-stu-id="1379b-216">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="1379b-217">在下列範例中，數個檔案延伸模組已登錄到已知的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="1379b-217">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="1379b-218">*.Rtf*取代擴充功能，和*.mp4*已移除。</span><span class="sxs-lookup"><span data-stu-id="1379b-218">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="1379b-219">請參閱[MIME 內容類型](http://www.iana.org/assignments/media-types/media-types.xhtml)。</span><span class="sxs-lookup"><span data-stu-id="1379b-219">See [MIME content types](http://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="1379b-220">非標準的內容類型</span><span class="sxs-lookup"><span data-stu-id="1379b-220">Non-standard content types</span></span>

<span data-ttu-id="1379b-221">靜態檔案中介軟體可理解幾乎 400 已知的檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="1379b-221">The static file middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="1379b-222">如果使用者要求未知的檔案類型的檔案時，靜態檔案中介軟體會傳回 HTTP 404 （找不到） 回應。</span><span class="sxs-lookup"><span data-stu-id="1379b-222">If the user requests a file of an unknown file type, the static file middleware returns a HTTP 404 (Not Found) response.</span></span> <span data-ttu-id="1379b-223">如果啟用目錄瀏覽時，會顯示檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="1379b-223">If directory browsing is enabled, a link to the file is displayed.</span></span> <span data-ttu-id="1379b-224">URI 會傳回 HTTP 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="1379b-224">The URI returns an HTTP 404 error.</span></span>

<span data-ttu-id="1379b-225">下列程式碼可讓處理未知的類型，並轉譯為影像未知的檔案：</span><span class="sxs-lookup"><span data-stu-id="1379b-225">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="1379b-226">使用上述的程式碼，具有未知的內容類型的檔案的要求會傳回做為映像。</span><span class="sxs-lookup"><span data-stu-id="1379b-226">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="1379b-227">啟用[ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes)會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="1379b-227">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="1379b-228">根據預設，已停用，不建議使用它。</span><span class="sxs-lookup"><span data-stu-id="1379b-228">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="1379b-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider)提供更安全的替代方法，來提供檔案與非標準擴充功能。</span><span class="sxs-lookup"><span data-stu-id="1379b-229">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

### <a name="considerations"></a><span data-ttu-id="1379b-230">考量</span><span class="sxs-lookup"><span data-stu-id="1379b-230">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="1379b-231">`UseDirectoryBrowser`和`UseStaticFiles`可能會導致洩漏機密資料。</span><span class="sxs-lookup"><span data-stu-id="1379b-231">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="1379b-232">強烈建議在生產環境中瀏覽的停用目錄。</span><span class="sxs-lookup"><span data-stu-id="1379b-232">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="1379b-233">請仔細檢閱 哪個目錄中已啟用透過`UseStaticFiles`或`UseDirectoryBrowser`。</span><span class="sxs-lookup"><span data-stu-id="1379b-233">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="1379b-234">整個目錄以及目錄及其子目錄會變成可公開存取。</span><span class="sxs-lookup"><span data-stu-id="1379b-234">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="1379b-235">存放區中的檔案適用於公開服務專用的目錄，例如 *\<content_root > / wwwroot*。</span><span class="sxs-lookup"><span data-stu-id="1379b-235">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="1379b-236">這些檔案分開 MVC 檢視，Razor 頁面 (2.x 僅)，設定檔、 等等。</span><span class="sxs-lookup"><span data-stu-id="1379b-236">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="1379b-237">使用公開內容的 Url`UseDirectoryBrowser`和`UseStaticFiles`可能會有區分大小寫，基礎檔案系統的字元限制。</span><span class="sxs-lookup"><span data-stu-id="1379b-237">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="1379b-238">例如，Windows 不區分&mdash;Mac 和 Linux 不是。</span><span class="sxs-lookup"><span data-stu-id="1379b-238">For example, Windows is case insensitive&mdash;Mac and Linux aren't.</span></span>

* <span data-ttu-id="1379b-239">ASP.NET Core 應用程式裝載於 IIS 使用[ASP.NET 核心模組 (ANCM)](xref:fundamentals/servers/aspnet-core-module)轉送至應用程式，包括靜態檔案要求的所有要求。</span><span class="sxs-lookup"><span data-stu-id="1379b-239">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module (ANCM)](xref:fundamentals/servers/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="1379b-240">不會使用 IIS 靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="1379b-240">The IIS static file handler isn't used.</span></span> <span data-ttu-id="1379b-241">它有沒有機會之前由 ANCM 處理要求。</span><span class="sxs-lookup"><span data-stu-id="1379b-241">It has no chance to handle requests before they're handled by the ANCM.</span></span>

* <span data-ttu-id="1379b-242">完成下列步驟在 IIS 管理員移除 IIS 靜態檔案處理常式，在伺服器或網站層級：</span><span class="sxs-lookup"><span data-stu-id="1379b-242">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="1379b-243">瀏覽至**模組**功能。</span><span class="sxs-lookup"><span data-stu-id="1379b-243">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="1379b-244">選取**StaticFileModule**清單中。</span><span class="sxs-lookup"><span data-stu-id="1379b-244">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="1379b-245">按一下**移除**中**動作**[資訊看板]。</span><span class="sxs-lookup"><span data-stu-id="1379b-245">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="1379b-246">如果已啟用 IIS 靜態檔案處理常式**和**ANCM 設定不正確、 靜態檔案都有受到處理。</span><span class="sxs-lookup"><span data-stu-id="1379b-246">If the IIS static file handler is enabled **and** the ANCM is configured incorrectly, static files are served.</span></span> <span data-ttu-id="1379b-247">發生這種情況，例如，如果*web.config*檔案未部署。</span><span class="sxs-lookup"><span data-stu-id="1379b-247">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="1379b-248">程式碼檔案 (包括*.cs*和*.cshtml*) 應用程式專案的 web 根目錄之外。</span><span class="sxs-lookup"><span data-stu-id="1379b-248">Place code files (including *.cs* and *.cshtml*) outside of the app project's web root.</span></span> <span data-ttu-id="1379b-249">邏輯分離，因此會建立應用程式的用戶端的內容與伺服器端程式碼之間。</span><span class="sxs-lookup"><span data-stu-id="1379b-249">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="1379b-250">這可防止伺服器端程式碼外洩。</span><span class="sxs-lookup"><span data-stu-id="1379b-250">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1379b-251">其他資源</span><span class="sxs-lookup"><span data-stu-id="1379b-251">Additional resources</span></span>

* [<span data-ttu-id="1379b-252">中介軟體</span><span class="sxs-lookup"><span data-stu-id="1379b-252">Middleware</span></span>](xref:fundamentals/middleware)

* [<span data-ttu-id="1379b-253">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="1379b-253">Introduction to ASP.NET Core</span></span>](xref:index)