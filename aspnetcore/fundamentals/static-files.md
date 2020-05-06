---
title: ASP.NET Core 中的靜態檔案
author: rick-anderson
description: 了解如何在 ASP.NET Core Web 應用程式中提供靜態檔案、保護其安全，並設定靜態檔案裝載中介軟體的行為。
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/static-files
ms.openlocfilehash: f7697260e6ab29d0e9ba955dfdf0c8c81e4e1130
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775488"
---
# <a name="static-files-in-aspnet-core"></a><span data-ttu-id="ee728-103">ASP.NET Core 中的靜態檔案</span><span class="sxs-lookup"><span data-stu-id="ee728-103">Static files in ASP.NET Core</span></span>

<span data-ttu-id="ee728-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Scott Addie](https://twitter.com/Scott_Addie) 撰寫</span><span class="sxs-lookup"><span data-stu-id="ee728-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Scott Addie](https://twitter.com/Scott_Addie)</span></span>

<span data-ttu-id="ee728-105">HTML、CSS、影像和 JavaScript 這類靜態檔案都是 ASP.NET Core 應用程式直接提供給用戶端的資產。</span><span class="sxs-lookup"><span data-stu-id="ee728-105">Static files, such as HTML, CSS, images, and JavaScript, are assets an ASP.NET Core app serves directly to clients.</span></span> <span data-ttu-id="ee728-106">您需要進行一些設定，才能提供這些檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-106">Some configuration is required to enable serving of these files.</span></span>

<span data-ttu-id="ee728-107">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="ee728-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/static-files/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="serve-static-files"></a><span data-ttu-id="ee728-108">提供靜態檔案</span><span class="sxs-lookup"><span data-stu-id="ee728-108">Serve static files</span></span>

<span data-ttu-id="ee728-109">靜態檔案會儲存在專案的[web 根目錄](xref:fundamentals/index#web-root)中。</span><span class="sxs-lookup"><span data-stu-id="ee728-109">Static files are stored within the project's [web root](xref:fundamentals/index#web-root) directory.</span></span> <span data-ttu-id="ee728-110">預設目錄是 *{content root}/wwwroot*，但可以透過[UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)方法加以變更。</span><span class="sxs-lookup"><span data-stu-id="ee728-110">The default directory is *{content root}/wwwroot*, but it can be changed via the [UseWebRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usewebroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseWebRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) method.</span></span> <span data-ttu-id="ee728-111">如需詳細資訊，請參閱[內容根目錄](xref:fundamentals/index#content-root)和 [Web 根目錄](xref:fundamentals/index#web-root)。</span><span class="sxs-lookup"><span data-stu-id="ee728-111">See [Content root](xref:fundamentals/index#content-root) and [Web root](xref:fundamentals/index#web-root) for more information.</span></span>

<span data-ttu-id="ee728-112">您必須讓應用程式的 Web 主機記住內容根目錄。</span><span class="sxs-lookup"><span data-stu-id="ee728-112">The app's web host must be made aware of the content root directory.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="ee728-113">`WebHost.CreateDefaultBuilder` 方法可將內容根目錄設定為目前的目錄：</span><span class="sxs-lookup"><span data-stu-id="ee728-113">The `WebHost.CreateDefaultBuilder` method sets the content root to the current directory:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main&highlight=9)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ee728-114">您可以叫用 `Program.Main` 內的 [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_)，以將內容根目錄設定為目前的目錄：</span><span class="sxs-lookup"><span data-stu-id="ee728-114">Set the content root to the current directory by invoking [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot#Microsoft_AspNetCore_Hosting_HostingAbstractionsWebHostBuilderExtensions_UseContentRoot_Microsoft_AspNetCore_Hosting_IWebHostBuilder_System_String_) inside of `Program.Main`:</span></span>

[!code-csharp[](static-files/samples/1x/Program.cs?name=snippet_ProgramClass&highlight=7)]

::: moniker-end

<span data-ttu-id="ee728-115">靜態檔案可透過相對於[web 根目錄](xref:fundamentals/index#web-root)的路徑來存取。</span><span class="sxs-lookup"><span data-stu-id="ee728-115">Static files are accessible via a path relative to the [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="ee728-116">例如，**Web 應用程式**專案範本的 *wwwroot* 資料夾內包含數個資料夾：</span><span class="sxs-lookup"><span data-stu-id="ee728-116">For example, the **Web Application** project template contains several folders within the *wwwroot* folder:</span></span>

* <span data-ttu-id="ee728-117">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="ee728-117">**wwwroot**</span></span>
  * <span data-ttu-id="ee728-118">**css**</span><span class="sxs-lookup"><span data-stu-id="ee728-118">**css**</span></span>
  * <span data-ttu-id="ee728-119">**images**</span><span class="sxs-lookup"><span data-stu-id="ee728-119">**images**</span></span>
  * <span data-ttu-id="ee728-120">**node.js**</span><span class="sxs-lookup"><span data-stu-id="ee728-120">**js**</span></span>

<span data-ttu-id="ee728-121">若要存取 *images* 子資料夾的檔案，其 URI 格式為 http://\<伺服器位址>/images/\<影像檔名稱>\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee728-121">The URI format to access a file in the *images* subfolder is *http://\<server_address>/images/\<image_file_name>*.</span></span> <span data-ttu-id="ee728-122">例如： *http://localhost:9189/images/banner3.svg* 。</span><span class="sxs-lookup"><span data-stu-id="ee728-122">For example, *http://localhost:9189/images/banner3.svg*.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="ee728-123">如以 .NET Framework 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="ee728-123">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="ee728-124">如以 .NET Core 為目標，[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage-app)會包含此套件。</span><span class="sxs-lookup"><span data-stu-id="ee728-124">If targeting .NET Core, the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) includes this package.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="ee728-125">如以 .NET Framework 為目標，請將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="ee728-125">If targeting .NET Framework, add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span> <span data-ttu-id="ee728-126">如以 .NET Core 為目標，[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)會包含此套件。</span><span class="sxs-lookup"><span data-stu-id="ee728-126">If targeting .NET Core, the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage) includes this package.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="ee728-127">將 [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) 套件新增至專案。</span><span class="sxs-lookup"><span data-stu-id="ee728-127">Add the [Microsoft.AspNetCore.StaticFiles](https://www.nuget.org/packages/Microsoft.AspNetCore.StaticFiles/) package to the project.</span></span>

::: moniker-end

<span data-ttu-id="ee728-128">設定[中介軟體](xref:fundamentals/middleware/index)以提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-128">Configure the [middleware](xref:fundamentals/middleware/index) which enables the serving of static files.</span></span>

### <a name="serve-files-inside-of-web-root"></a><span data-ttu-id="ee728-129">提供 Web 根目錄內的檔案</span><span class="sxs-lookup"><span data-stu-id="ee728-129">Serve files inside of web root</span></span>

<span data-ttu-id="ee728-130">叫用 `Startup.Configure` 內的 [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：</span><span class="sxs-lookup"><span data-stu-id="ee728-130">Invoke the [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method within `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupStaticFiles.cs?name=snippet_ConfigureMethod&highlight=3)]

<span data-ttu-id="ee728-131">無參數`UseStaticFiles`方法多載會將[web 根目錄](xref:fundamentals/index#web-root)中的檔案標記為 servable。</span><span class="sxs-lookup"><span data-stu-id="ee728-131">The parameterless `UseStaticFiles` method overload marks the files in [web root](xref:fundamentals/index#web-root) as servable.</span></span> <span data-ttu-id="ee728-132">下列標記參考 *wwwroot/images/banner1.svg*：</span><span class="sxs-lookup"><span data-stu-id="ee728-132">The following markup references *wwwroot/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_wwwroot)]

<span data-ttu-id="ee728-133">在上述程式碼中，波狀`~/`符號字元會指向[web 根目錄](xref:fundamentals/index#web-root)。</span><span class="sxs-lookup"><span data-stu-id="ee728-133">In the preceding code, the tilde character `~/` points to the [web root](xref:fundamentals/index#web-root).</span></span>

### <a name="serve-files-outside-of-web-root"></a><span data-ttu-id="ee728-134">提供 Web 根目錄外的檔案</span><span class="sxs-lookup"><span data-stu-id="ee728-134">Serve files outside of web root</span></span>

<span data-ttu-id="ee728-135">假設有一個目錄階層，其中要提供的靜態檔案位於[web 根目錄](xref:fundamentals/index#web-root)外部：</span><span class="sxs-lookup"><span data-stu-id="ee728-135">Consider a directory hierarchy in which the static files to be served reside outside of the [web root](xref:fundamentals/index#web-root):</span></span>

* <span data-ttu-id="ee728-136">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="ee728-136">**wwwroot**</span></span>
  * <span data-ttu-id="ee728-137">**css**</span><span class="sxs-lookup"><span data-stu-id="ee728-137">**css**</span></span>
  * <span data-ttu-id="ee728-138">**images**</span><span class="sxs-lookup"><span data-stu-id="ee728-138">**images**</span></span>
  * <span data-ttu-id="ee728-139">**node.js**</span><span class="sxs-lookup"><span data-stu-id="ee728-139">**js**</span></span>
* <span data-ttu-id="ee728-140">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="ee728-140">**MyStaticFiles**</span></span>
  * <span data-ttu-id="ee728-141">**images**</span><span class="sxs-lookup"><span data-stu-id="ee728-141">**images**</span></span>
    * <span data-ttu-id="ee728-142">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="ee728-142">*banner1.svg*</span></span>

<span data-ttu-id="ee728-143">透過設定靜態檔案中介軟體，可讓要求存取 *banner1.svg* 檔案，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ee728-143">A request can access the *banner1.svg* file by configuring the Static File Middleware as follows:</span></span>

[!code-csharp[](static-files/samples/1x/StartupTwoStaticFiles.cs?name=snippet_ConfigureMethod&highlight=5-10)]

<span data-ttu-id="ee728-144">上述程式碼會透過 *StaticFiles* URI 區段公開 *MyStaticFiles* 目錄階層。</span><span class="sxs-lookup"><span data-stu-id="ee728-144">In the preceding code, the *MyStaticFiles* directory hierarchy is exposed publicly via the *StaticFiles* URI segment.</span></span> <span data-ttu-id="ee728-145">要求 http://\<伺服器位址>/StaticFiles/images/banner1.svg\*\* 時，會提供 *banner1.svg* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-145">A request to *http://\<server_address>/StaticFiles/images/banner1.svg* serves the *banner1.svg* file.</span></span>

<span data-ttu-id="ee728-146">下列標記參考 *MyStaticFiles/images/banner1.svg*：</span><span class="sxs-lookup"><span data-stu-id="ee728-146">The following markup references *MyStaticFiles/images/banner1.svg*:</span></span>

[!code-cshtml[](static-files/samples/1x/Views/Home/Index.cshtml?name=snippet_static_file_outside)]

### <a name="set-http-response-headers"></a><span data-ttu-id="ee728-147">設定 HTTP 回應標頭</span><span class="sxs-lookup"><span data-stu-id="ee728-147">Set HTTP response headers</span></span>

<span data-ttu-id="ee728-148">[StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) 物件可以用來設定 HTTP 回應標頭。</span><span class="sxs-lookup"><span data-stu-id="ee728-148">A [StaticFileOptions](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions) object can be used to set HTTP response headers.</span></span> <span data-ttu-id="ee728-149">除了設定[web 根目錄](xref:fundamentals/index#web-root)提供的靜態檔案外，下列程式碼也會設定`Cache-Control`標頭：</span><span class="sxs-lookup"><span data-stu-id="ee728-149">In addition to configuring static file serving from the [web root](xref:fundamentals/index#web-root), the following code sets the `Cache-Control` header:</span></span>

[!code-csharp[](static-files/samples/1x/StartupAddHeader.cs?name=snippet_ConfigureMethod)]
[!INCLUDE[about the series](~/includes/code-comments-loc.md)]

<span data-ttu-id="ee728-150">[HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) 方法存在於 [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) 套件中。</span><span class="sxs-lookup"><span data-stu-id="ee728-150">The [HeaderDictionaryExtensions.Append](/dotnet/api/microsoft.aspnetcore.http.headerdictionaryextensions.append) method exists in the [Microsoft.AspNetCore.Http](https://www.nuget.org/packages/Microsoft.AspNetCore.Http/) package.</span></span>

<span data-ttu-id="ee728-151">檔案已在開發環境中設為可公開快取 10 分鐘 (600 秒)：</span><span class="sxs-lookup"><span data-stu-id="ee728-151">The files have been made publicly cacheable for 10 minutes (600 seconds) in the Development environment:</span></span>

![已新增顯示「快取控制」標頭的回應標頭](static-files/_static/add-header.png)

## <a name="static-file-authorization"></a><span data-ttu-id="ee728-153">靜態檔案授權</span><span class="sxs-lookup"><span data-stu-id="ee728-153">Static file authorization</span></span>

<span data-ttu-id="ee728-154">靜態檔案中介軟體不提供授權檢查。</span><span class="sxs-lookup"><span data-stu-id="ee728-154">The Static File Middleware doesn't provide authorization checks.</span></span> <span data-ttu-id="ee728-155">其提供的所有檔案，包括在 *wwwroot* 下的檔案，皆可公開存取。</span><span class="sxs-lookup"><span data-stu-id="ee728-155">Any files served by it, including those under *wwwroot*, are publicly accessible.</span></span> <span data-ttu-id="ee728-156">若要依據授權來提供檔案：</span><span class="sxs-lookup"><span data-stu-id="ee728-156">To serve files based on authorization:</span></span>

* <span data-ttu-id="ee728-157">請將它們儲存在 *wwwroot* 外部和可存取靜態檔案中介軟體的任何目錄。</span><span class="sxs-lookup"><span data-stu-id="ee728-157">Store them outside of *wwwroot* and any directory accessible to the Static File Middleware.</span></span>
* <span data-ttu-id="ee728-158">透過動作方法，將它們提供給已套用授權的目標。</span><span class="sxs-lookup"><span data-stu-id="ee728-158">Serve them via an action method to which authorization is applied.</span></span> <span data-ttu-id="ee728-159">傳回 [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) 物件：</span><span class="sxs-lookup"><span data-stu-id="ee728-159">Return a [FileResult](/dotnet/api/microsoft.aspnetcore.mvc.fileresult) object:</span></span>

  [!code-csharp[](static-files/samples/1x/Controllers/HomeController.cs?name=snippet_BannerImageAction)]

## <a name="enable-directory-browsing"></a><span data-ttu-id="ee728-160">啟用目錄瀏覽</span><span class="sxs-lookup"><span data-stu-id="ee728-160">Enable directory browsing</span></span>

<span data-ttu-id="ee728-161">您的 Web 應用程式使用者可藉由目錄瀏覽功能，查看目錄清單及指定目錄內的檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-161">Directory browsing allows users of your web app to see a directory listing and files within a specified directory.</span></span> <span data-ttu-id="ee728-162">基於安全性考量，預設為停用目錄瀏覽功能 (請參閱[考量](#considerations))。</span><span class="sxs-lookup"><span data-stu-id="ee728-162">Directory browsing is disabled by default for security reasons (see [Considerations](#considerations)).</span></span> <span data-ttu-id="ee728-163">您可以叫用 `Startup.Configure` 內的 [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) 方法，以啟用目錄瀏覽功能：</span><span class="sxs-lookup"><span data-stu-id="ee728-163">Enable directory browsing by invoking the [UseDirectoryBrowser](/dotnet/api/microsoft.aspnetcore.builder.directorybrowserextensions.usedirectorybrowser#Microsoft_AspNetCore_Builder_DirectoryBrowserExtensions_UseDirectoryBrowser_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_DirectoryBrowserOptions_) method in `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=12-17)]

<span data-ttu-id="ee728-164">您可以從 `Startup.ConfigureServices` 叫用 [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 方法，以新增所需的服務：</span><span class="sxs-lookup"><span data-stu-id="ee728-164">Add required services by invoking the [AddDirectoryBrowser](/dotnet/api/microsoft.extensions.dependencyinjection.directorybrowserserviceextensions.adddirectorybrowser#Microsoft_Extensions_DependencyInjection_DirectoryBrowserServiceExtensions_AddDirectoryBrowser_Microsoft_Extensions_DependencyInjection_IServiceCollection_) method from `Startup.ConfigureServices`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureServicesMethod&highlight=3)]

<span data-ttu-id="ee728-165">上述程式碼可讓您使用 URL http://\<伺服器位址>/MyImages\*\* 與每個檔案和資料夾的連結，來進行 *wwwroot/images* 資料夾的目錄瀏覽：</span><span class="sxs-lookup"><span data-stu-id="ee728-165">The preceding code allows directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*, with links to each file and folder:</span></span>

![目錄瀏覽](static-files/_static/dir-browse.png)

<span data-ttu-id="ee728-167">如需了解啟用瀏覽功能時的安全性風險，請參閱[考量](#considerations)。</span><span class="sxs-lookup"><span data-stu-id="ee728-167">See [Considerations](#considerations) on the security risks when enabling browsing.</span></span>

<span data-ttu-id="ee728-168">請注意下列範例中的兩個 `UseStaticFiles` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="ee728-168">Note the two `UseStaticFiles` calls in the following example.</span></span> <span data-ttu-id="ee728-169">第一個呼叫可提供 *wwwroot* 資料夾中的靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-169">The first call enables the serving of static files in the *wwwroot* folder.</span></span> <span data-ttu-id="ee728-170">第二個呼叫可啟用使用 URL http://\<伺服器位址>/MyImages\*\* 的 *wwwroot/images* 資料夾目錄瀏覽功能：</span><span class="sxs-lookup"><span data-stu-id="ee728-170">The second call enables directory browsing of the *wwwroot/images* folder using the URL *http://\<server_address>/MyImages*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupBrowse.cs?name=snippet_ConfigureMethod&highlight=3,5)]

## <a name="serve-a-default-document"></a><span data-ttu-id="ee728-171">提供預設文件</span><span class="sxs-lookup"><span data-stu-id="ee728-171">Serve a default document</span></span>

<span data-ttu-id="ee728-172">設定預設的歡迎頁面，可讓訪客在瀏覽您的網站時有個合乎邏輯的起點。</span><span class="sxs-lookup"><span data-stu-id="ee728-172">Setting a default home page provides visitors a logical starting point when visiting your site.</span></span> <span data-ttu-id="ee728-173">若要提供預設頁面 (使用者不需完整限定 URI)，請從 `Startup.Configure` 呼叫 [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) 方法：</span><span class="sxs-lookup"><span data-stu-id="ee728-173">To serve a default page without the user fully qualifying the URI, call the [UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) method from `Startup.Configure`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupEmpty.cs?name=snippet_ConfigureMethod&highlight=3)]

> [!IMPORTANT]
> <span data-ttu-id="ee728-174">您必須在 `UseStaticFiles` 之前先呼叫 `UseDefaultFiles`，才能提供預設的檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-174">`UseDefaultFiles` must be called before `UseStaticFiles` to serve the default file.</span></span> <span data-ttu-id="ee728-175">`UseDefaultFiles` 是 URL 重寫器，並非實際提供檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-175">`UseDefaultFiles` is a URL rewriter that doesn't actually serve the file.</span></span> <span data-ttu-id="ee728-176">透過 `UseStaticFiles` 啟用靜態檔案中介軟體以提供檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-176">Enable Static File Middleware via `UseStaticFiles` to serve the file.</span></span>

<span data-ttu-id="ee728-177">使用 `UseDefaultFiles` 要求以搜尋資料夾：</span><span class="sxs-lookup"><span data-stu-id="ee728-177">With `UseDefaultFiles`, requests to a folder search for:</span></span>

* <span data-ttu-id="ee728-178">*default.htm*</span><span class="sxs-lookup"><span data-stu-id="ee728-178">*default.htm*</span></span>
* <span data-ttu-id="ee728-179">*default.html*</span><span class="sxs-lookup"><span data-stu-id="ee728-179">*default.html*</span></span>
* <span data-ttu-id="ee728-180">*index.htm*</span><span class="sxs-lookup"><span data-stu-id="ee728-180">*index.htm*</span></span>
* <span data-ttu-id="ee728-181">*索引 .html*</span><span class="sxs-lookup"><span data-stu-id="ee728-181">*index.html*</span></span>

<span data-ttu-id="ee728-182">系統會提供從清單中找到的第一個檔案，就像提出的要求是完整 URI 一樣。</span><span class="sxs-lookup"><span data-stu-id="ee728-182">The first file found from the list is served as though the request were the fully qualified URI.</span></span> <span data-ttu-id="ee728-183">瀏覽器 URL 仍會繼續反應要求的 URI。</span><span class="sxs-lookup"><span data-stu-id="ee728-183">The browser URL continues to reflect the URI requested.</span></span>

<span data-ttu-id="ee728-184">下列程式碼會將預設檔案名稱變更為 *mydefault.html*：</span><span class="sxs-lookup"><span data-stu-id="ee728-184">The following code changes the default file name to *mydefault.html*:</span></span>

[!code-csharp[](static-files/samples/1x/StartupDefault.cs?name=snippet_ConfigureMethod)]

## <a name="usefileserver"></a><span data-ttu-id="ee728-185">UseFileServer</span><span class="sxs-lookup"><span data-stu-id="ee728-185">UseFileServer</span></span>

<span data-ttu-id="ee728-186"><xref:Microsoft.AspNetCore.Builder.FileServerExtensions.UseFileServer*>結合`UseStaticFiles`、 `UseDefaultFiles`和的功能（選擇性） `UseDirectoryBrowser`。</span><span class="sxs-lookup"><span data-stu-id="ee728-186"><xref:Microsoft.AspNetCore.Builder.FileServerExtensions.UseFileServer*> combines the functionality of `UseStaticFiles`, `UseDefaultFiles`, and optionally `UseDirectoryBrowser`.</span></span>

<span data-ttu-id="ee728-187">下列程式碼可提供靜態檔案和預設檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-187">The following code enables the serving of static files and the default file.</span></span> <span data-ttu-id="ee728-188">未啟用目錄瀏覽功能。</span><span class="sxs-lookup"><span data-stu-id="ee728-188">Directory browsing isn't enabled.</span></span>

```csharp
app.UseFileServer();
```

<span data-ttu-id="ee728-189">下列程式碼會啟用目錄瀏覽功能，以在無參數多載上進行建置：</span><span class="sxs-lookup"><span data-stu-id="ee728-189">The following code builds upon the parameterless overload by enabling directory browsing:</span></span>

```csharp
app.UseFileServer(enableDirectoryBrowsing: true);
```

<span data-ttu-id="ee728-190">請考慮下列目錄階層：</span><span class="sxs-lookup"><span data-stu-id="ee728-190">Consider the following directory hierarchy:</span></span>

* <span data-ttu-id="ee728-191">**wwwroot**</span><span class="sxs-lookup"><span data-stu-id="ee728-191">**wwwroot**</span></span>
  * <span data-ttu-id="ee728-192">**css**</span><span class="sxs-lookup"><span data-stu-id="ee728-192">**css**</span></span>
  * <span data-ttu-id="ee728-193">**images**</span><span class="sxs-lookup"><span data-stu-id="ee728-193">**images**</span></span>
  * <span data-ttu-id="ee728-194">**node.js**</span><span class="sxs-lookup"><span data-stu-id="ee728-194">**js**</span></span>
* <span data-ttu-id="ee728-195">**MyStaticFiles**</span><span class="sxs-lookup"><span data-stu-id="ee728-195">**MyStaticFiles**</span></span>
  * <span data-ttu-id="ee728-196">**images**</span><span class="sxs-lookup"><span data-stu-id="ee728-196">**images**</span></span>
    * <span data-ttu-id="ee728-197">*banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="ee728-197">*banner1.svg*</span></span>
  * <span data-ttu-id="ee728-198">*default.html*</span><span class="sxs-lookup"><span data-stu-id="ee728-198">*default.html*</span></span>

<span data-ttu-id="ee728-199">下列程式碼可啟用靜態檔案、預設檔案和 `MyStaticFiles` 的目錄瀏覽功能：</span><span class="sxs-lookup"><span data-stu-id="ee728-199">The following code enables static files, default files, and directory browsing of `MyStaticFiles`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureMethod&highlight=5-11)]

<span data-ttu-id="ee728-200">當 `EnableDirectoryBrowsing` 屬性值是 `true` 時，必須呼叫 `AddDirectoryBrowser`：</span><span class="sxs-lookup"><span data-stu-id="ee728-200">`AddDirectoryBrowser` must be called when the `EnableDirectoryBrowsing` property value is `true`:</span></span>

[!code-csharp[](static-files/samples/1x/StartupUseFileServer.cs?name=snippet_ConfigureServicesMethod)]

<span data-ttu-id="ee728-201">使用檔案階層和上述程式碼時，URL 解析方式如下：</span><span class="sxs-lookup"><span data-stu-id="ee728-201">Using the file hierarchy and preceding code, URLs resolve as follows:</span></span>

| <span data-ttu-id="ee728-202">URI</span><span class="sxs-lookup"><span data-stu-id="ee728-202">URI</span></span>            |                             <span data-ttu-id="ee728-203">回應</span><span class="sxs-lookup"><span data-stu-id="ee728-203">Response</span></span>  |
| ------- | ------|
| <span data-ttu-id="ee728-204">*HTTP://\<server_address>/staticfiles/images/banner1.svg*</span><span class="sxs-lookup"><span data-stu-id="ee728-204">*http://\<server_address>/StaticFiles/images/banner1.svg*</span></span>    |      <span data-ttu-id="ee728-205">MyStaticFiles/images/banner1.svg</span><span class="sxs-lookup"><span data-stu-id="ee728-205">MyStaticFiles/images/banner1.svg</span></span> |
| <span data-ttu-id="ee728-206">*HTTP://\<server_address>/staticfiles*</span><span class="sxs-lookup"><span data-stu-id="ee728-206">*http://\<server_address>/StaticFiles*</span></span>             |     <span data-ttu-id="ee728-207">MyStaticFiles/default.html</span><span class="sxs-lookup"><span data-stu-id="ee728-207">MyStaticFiles/default.html</span></span> |

<span data-ttu-id="ee728-208">如果 *MyStaticFiles* 目錄中不存在預設名稱的檔案，http://\<伺服器位址>/StaticFiles\*\* 會傳回含有可點按連結的目錄清單：</span><span class="sxs-lookup"><span data-stu-id="ee728-208">If no default-named file exists in the *MyStaticFiles* directory, *http://\<server_address>/StaticFiles* returns the directory listing with clickable links:</span></span>

![靜態檔案清單](static-files/_static/db2.png)

> [!NOTE]
> <span data-ttu-id="ee728-210"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> 和 <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> 會從 `http://{SERVER ADDRESS}/StaticFiles` (不含尾端斜線) 執行用戶端重新導向至 `http://{SERVER ADDRESS}/StaticFiles/` (含尾端斜線)。</span><span class="sxs-lookup"><span data-stu-id="ee728-210"><xref:Microsoft.AspNetCore.Builder.DefaultFilesExtensions.UseDefaultFiles*> and <xref:Microsoft.AspNetCore.Builder.DirectoryBrowserExtensions.UseDirectoryBrowser*> perform a client-side redirect from `http://{SERVER ADDRESS}/StaticFiles` (without a trailing slash) to `http://{SERVER ADDRESS}/StaticFiles/` (with a trailing slash).</span></span> <span data-ttu-id="ee728-211">*StaticFiles* 目錄內的相對 URL 若未含尾端斜線則會被視為無效。</span><span class="sxs-lookup"><span data-stu-id="ee728-211">Relative URLs within the *StaticFiles* directory are invalid without a trailing slash.</span></span>

## <a name="fileextensioncontenttypeprovider"></a><span data-ttu-id="ee728-212">FileExtensionContentTypeProvider</span><span class="sxs-lookup"><span data-stu-id="ee728-212">FileExtensionContentTypeProvider</span></span>

<span data-ttu-id="ee728-213">[FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) 類別包含 `Mappings` 屬性以作為 MIME 內容類型的副檔名對應。</span><span class="sxs-lookup"><span data-stu-id="ee728-213">The [FileExtensionContentTypeProvider](/dotnet/api/microsoft.aspnetcore.staticfiles.fileextensioncontenttypeprovider) class contains a `Mappings` property serving as a mapping of file extensions to MIME content types.</span></span> <span data-ttu-id="ee728-214">在下列範例中，已有數個副檔名註冊為已知的 MIME 類型。</span><span class="sxs-lookup"><span data-stu-id="ee728-214">In the following sample, several file extensions are registered to known MIME types.</span></span> <span data-ttu-id="ee728-215">已取代 *.rtf* 副檔名，並已移除 *.mp4*。</span><span class="sxs-lookup"><span data-stu-id="ee728-215">The *.rtf* extension is replaced, and *.mp4* is removed.</span></span>

[!code-csharp[](static-files/samples/1x/StartupFileExtensionContentTypeProvider.cs?name=snippet_ConfigureMethod&highlight=3-12,19)]

<span data-ttu-id="ee728-216">請參閱 [MIME 內容類型](https://www.iana.org/assignments/media-types/media-types.xhtml)。</span><span class="sxs-lookup"><span data-stu-id="ee728-216">See [MIME content types](https://www.iana.org/assignments/media-types/media-types.xhtml).</span></span>

## <a name="non-standard-content-types"></a><span data-ttu-id="ee728-217">非標準的內容類型</span><span class="sxs-lookup"><span data-stu-id="ee728-217">Non-standard content types</span></span>

<span data-ttu-id="ee728-218">靜態檔案中介軟體可理解幾乎 400 種已知的檔案內容類型。</span><span class="sxs-lookup"><span data-stu-id="ee728-218">Static File Middleware understands almost 400 known file content types.</span></span> <span data-ttu-id="ee728-219">如果使用者要求具有未知檔案類型的檔案，則靜態檔案中介軟體會將該要求傳遞至管線中的下一個中介軟體。</span><span class="sxs-lookup"><span data-stu-id="ee728-219">If the user requests a file with an unknown file type, Static File Middleware passes the request to the next middleware in the pipeline.</span></span> <span data-ttu-id="ee728-220">如果沒有中介軟體處理要求，則會傳回「404 找不到」\*\* 回應。</span><span class="sxs-lookup"><span data-stu-id="ee728-220">If no middleware handles the request, a *404 Not Found* response is returned.</span></span> <span data-ttu-id="ee728-221">如果啟用目錄瀏覽功能，則會在目錄清單中顯示檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="ee728-221">If directory browsing is enabled, a link to the file is displayed in a directory listing.</span></span>

<span data-ttu-id="ee728-222">下列程式碼可讓您提供未知的類型，並會將未知的檔案轉譯為影像：</span><span class="sxs-lookup"><span data-stu-id="ee728-222">The following code enables serving unknown types and renders the unknown file as an image:</span></span>

[!code-csharp[](static-files/samples/1x/StartupServeUnknownFileTypes.cs?name=snippet_ConfigureMethod)]

<span data-ttu-id="ee728-223">使用上述程式碼時，系統會以影像來回應具有未知內容類型的檔案要求。</span><span class="sxs-lookup"><span data-stu-id="ee728-223">With the preceding code, a request for a file with an unknown content type is returned as an image.</span></span>

> [!WARNING]
> <span data-ttu-id="ee728-224">啟用 [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) 時會造成安全性風險。</span><span class="sxs-lookup"><span data-stu-id="ee728-224">Enabling [ServeUnknownFileTypes](/dotnet/api/microsoft.aspnetcore.builder.staticfileoptions.serveunknownfiletypes#Microsoft_AspNetCore_Builder_StaticFileOptions_ServeUnknownFileTypes) is a security risk.</span></span> <span data-ttu-id="ee728-225">預設為停用，亦不建議您使用。</span><span class="sxs-lookup"><span data-stu-id="ee728-225">It's disabled by default, and its use is discouraged.</span></span> <span data-ttu-id="ee728-226">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) 可提供更安全的替代方法，來提供非標準副檔名的檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-226">[FileExtensionContentTypeProvider](#fileextensioncontenttypeprovider) provides a safer alternative to serving files with non-standard extensions.</span></span>

## <a name="serve-files-from-multiple-locations"></a><span data-ttu-id="ee728-227">提供多個位置的檔案</span><span class="sxs-lookup"><span data-stu-id="ee728-227">Serve files from multiple locations</span></span>

<span data-ttu-id="ee728-228">`UseStaticFiles`和`UseFileServer`預設為指向*wwwroot*的檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="ee728-228">`UseStaticFiles` and `UseFileServer` defaults to the file provider pointing at *wwwroot*.</span></span> <span data-ttu-id="ee728-229">您可以提供其他的`UseStaticFiles`實例和`UseFileServer`其他檔案提供者，以從其他位置處理檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-229">You can provide additional instances of `UseStaticFiles` and `UseFileServer` with other file providers to serve files from other locations.</span></span> <span data-ttu-id="ee728-230">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/15578)。</span><span class="sxs-lookup"><span data-stu-id="ee728-230">For more information, see [this GitHub issue](https://github.com/dotnet/AspNetCore.Docs/issues/15578).</span></span>

### <a name="considerations"></a><span data-ttu-id="ee728-231">考量</span><span class="sxs-lookup"><span data-stu-id="ee728-231">Considerations</span></span>

> [!WARNING]
> <span data-ttu-id="ee728-232">`UseDirectoryBrowser` 和 `UseStaticFiles` 可能會導致洩漏祕密。</span><span class="sxs-lookup"><span data-stu-id="ee728-232">`UseDirectoryBrowser` and `UseStaticFiles` can leak secrets.</span></span> <span data-ttu-id="ee728-233">強烈建議您在生產環境中停用目錄瀏覽功能。</span><span class="sxs-lookup"><span data-stu-id="ee728-233">Disabling directory browsing in production is highly recommended.</span></span> <span data-ttu-id="ee728-234">透過 `UseStaticFiles` 或 `UseDirectoryBrowser`，仔細檢閱要啟用哪些目錄。</span><span class="sxs-lookup"><span data-stu-id="ee728-234">Carefully review which directories are enabled via `UseStaticFiles` or `UseDirectoryBrowser`.</span></span> <span data-ttu-id="ee728-235">因為整個目錄和其子目錄都可供公開存取。</span><span class="sxs-lookup"><span data-stu-id="ee728-235">The entire directory and its sub-directories become publicly accessible.</span></span> <span data-ttu-id="ee728-236">儲存適用于在專用目錄中提供給公用的檔案，例如\* \<content_root>/wwwroot\*。</span><span class="sxs-lookup"><span data-stu-id="ee728-236">Store files suitable for serving to the public in a dedicated directory, such as *\<content_root>/wwwroot*.</span></span> <span data-ttu-id="ee728-237">將這些檔案與 MVC views、 Razor Pages （僅限2.x）、設定檔等隔開。</span><span class="sxs-lookup"><span data-stu-id="ee728-237">Separate these files from MVC views, Razor Pages (2.x only), configuration files, etc.</span></span>

* <span data-ttu-id="ee728-238">使用 `UseDirectoryBrowser` 和 `UseStaticFiles` 公開內容的 URL 可能有區分大小寫，並受限於基礎檔案系統的字元限制。</span><span class="sxs-lookup"><span data-stu-id="ee728-238">The URLs for content exposed with `UseDirectoryBrowser` and `UseStaticFiles` are subject to the case sensitivity and character restrictions of the underlying file system.</span></span> <span data-ttu-id="ee728-239">例如，Windows 不區分大小寫&mdash;macOS 和 Linux 則區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="ee728-239">For example, Windows is case insensitive&mdash;macOS and Linux aren't.</span></span>

* <span data-ttu-id="ee728-240">裝載於 IIS 中的 ASP.NET Core 應用程式會使用 [ASP.NET Core 模組](xref:host-and-deploy/aspnet-core-module)，將所有要求轉送給應用程式 (包括靜態檔案要求)，</span><span class="sxs-lookup"><span data-stu-id="ee728-240">ASP.NET Core apps hosted in IIS use the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) to forward all requests to the app, including static file requests.</span></span> <span data-ttu-id="ee728-241">而不會使用 IIS 靜態檔案處理常式。</span><span class="sxs-lookup"><span data-stu-id="ee728-241">The IIS static file handler isn't used.</span></span> <span data-ttu-id="ee728-242">因此，上述處理常式不可能在模組處理要求之前處理要求。</span><span class="sxs-lookup"><span data-stu-id="ee728-242">It has no chance to handle requests before they're handled by the module.</span></span>

* <span data-ttu-id="ee728-243">請完成 IIS 管理員中的下列步驟，以移除伺服器或網站層級的 IIS 靜態檔案處理常式：</span><span class="sxs-lookup"><span data-stu-id="ee728-243">Complete the following steps in IIS Manager to remove the IIS static file handler at the server or website level:</span></span>
    1. <span data-ttu-id="ee728-244">巡覽至 [模組]\*\*\*\* 功能。</span><span class="sxs-lookup"><span data-stu-id="ee728-244">Navigate to the **Modules** feature.</span></span>
    1. <span data-ttu-id="ee728-245">選取清單中的 **StaticFileModule**。</span><span class="sxs-lookup"><span data-stu-id="ee728-245">Select **StaticFileModule** in the list.</span></span>
    1. <span data-ttu-id="ee728-246">按一下 [動作]\*\*\*\* 側邊欄中的 [移除]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="ee728-246">Click **Remove** in the **Actions** sidebar.</span></span>

> [!WARNING]
> <span data-ttu-id="ee728-247">如果已啟用 IIS 靜態檔案處理常式，**但是**未正確設定 ASP.NET Core 模組，仍可提供靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="ee728-247">If the IIS static file handler is enabled **and** the ASP.NET Core Module is configured incorrectly, static files are served.</span></span> <span data-ttu-id="ee728-248">舉例來說，未部署 *web.config* 檔案時可能會發生上述情況。</span><span class="sxs-lookup"><span data-stu-id="ee728-248">This happens, for example, if the *web.config* file isn't deployed.</span></span>

* <span data-ttu-id="ee728-249">將程式碼檔案（包括 *.cs*和*cshtml*）放在應用程式專案的[web 根目錄](xref:fundamentals/index#web-root)外部。</span><span class="sxs-lookup"><span data-stu-id="ee728-249">Place code files (including *.cs* and *.cshtml*) outside of the app project's [web root](xref:fundamentals/index#web-root).</span></span> <span data-ttu-id="ee728-250">如此一來，即會建立應用程式的用戶端內容與伺服器端程式碼之間的邏輯分隔。</span><span class="sxs-lookup"><span data-stu-id="ee728-250">A logical separation is therefore created between the app's client-side content and server-based code.</span></span> <span data-ttu-id="ee728-251">這樣可以防止伺服器端程式碼外洩。</span><span class="sxs-lookup"><span data-stu-id="ee728-251">This prevents server-side code from being leaked.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ee728-252">其他資源</span><span class="sxs-lookup"><span data-stu-id="ee728-252">Additional resources</span></span>

* [<span data-ttu-id="ee728-253">中介軟體</span><span class="sxs-lookup"><span data-stu-id="ee728-253">Middleware</span></span>](xref:fundamentals/middleware/index)
* [<span data-ttu-id="ee728-254">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="ee728-254">Introduction to ASP.NET Core</span></span>](xref:index)
