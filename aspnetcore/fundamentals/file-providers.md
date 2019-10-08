---
title: ASP.NET Core 中的檔案提供者
author: guardrex
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/file-providers
ms.openlocfilehash: 3a92b44efc70d156596ee9fe80b4f6a65266e73d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007176"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="7f50c-103">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="7f50c-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="7f50c-104">作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="7f50c-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7f50c-105">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="7f50c-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="7f50c-106">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="7f50c-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="7f50c-107">`IWebHostEnvironment` 會將應用程式的[內容根目錄](xref:fundamentals/index#content-root)和[web 根目錄](xref:fundamentals/index#web-root)公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="7f50c-107">`IWebHostEnvironment` exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="7f50c-108">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="7f50c-109">[Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。</span><span class="sxs-lookup"><span data-stu-id="7f50c-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="7f50c-110">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="7f50c-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7f50c-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="7f50c-112">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="7f50c-112">File Provider interfaces</span></span>

<span data-ttu-id="7f50c-113">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="7f50c-113">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="7f50c-114">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="7f50c-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="7f50c-115">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="7f50c-115">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="7f50c-116">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="7f50c-116">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="7f50c-117">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="7f50c-117">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="7f50c-118">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="7f50c-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="7f50c-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="7f50c-119"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="7f50c-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="7f50c-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="7f50c-121">您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-121">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="7f50c-122">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="7f50c-122">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7f50c-123">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="7f50c-123">File Provider implementations</span></span>

<span data-ttu-id="7f50c-124">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="7f50c-124">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="7f50c-125">實作</span><span class="sxs-lookup"><span data-stu-id="7f50c-125">Implementation</span></span> | <span data-ttu-id="7f50c-126">描述</span><span class="sxs-lookup"><span data-stu-id="7f50c-126">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="7f50c-127">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-127">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="7f50c-128">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-128">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="7f50c-129">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-129">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="7f50c-130">資訊清單內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-130">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="7f50c-131">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-131">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="7f50c-132">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="7f50c-132">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="7f50c-133">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-133">PhysicalFileProvider</span></span>

<span data-ttu-id="7f50c-134"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="7f50c-134">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="7f50c-135">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="7f50c-135">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="7f50c-136">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="7f50c-136">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="7f50c-137">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-137">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="7f50c-138">當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="7f50c-138">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="7f50c-139">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="7f50c-139">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="7f50c-140">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="7f50c-140">Types in the preceding example:</span></span>

* <span data-ttu-id="7f50c-141">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-141">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="7f50c-142">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-142">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="7f50c-143">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-143">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="7f50c-144">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="7f50c-144">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="7f50c-145">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="7f50c-145">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="7f50c-146">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="7f50c-146">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="7f50c-147">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-147">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="7f50c-148"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-148">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="7f50c-149">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="7f50c-149">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="7f50c-150">將套件參考新增至 [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) 套件的專案中。</span><span class="sxs-lookup"><span data-stu-id="7f50c-150">Add a package reference to the project for the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) package.</span></span>

<span data-ttu-id="7f50c-151">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-151">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="7f50c-152">使用 [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 來指定要內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="7f50c-152">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="7f50c-153">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-153">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="7f50c-154">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="7f50c-154">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="7f50c-155">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="7f50c-155">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7f50c-156">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="7f50c-156">Additional overloads allow you to:</span></span>

* <span data-ttu-id="7f50c-157">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="7f50c-157">Specify a relative file path.</span></span>
* <span data-ttu-id="7f50c-158">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="7f50c-158">Scope files to a last modified date.</span></span>
* <span data-ttu-id="7f50c-159">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="7f50c-159">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="7f50c-160">多載</span><span class="sxs-lookup"><span data-stu-id="7f50c-160">Overload</span></span> | <span data-ttu-id="7f50c-161">描述</span><span class="sxs-lookup"><span data-stu-id="7f50c-161">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="7f50c-162">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="7f50c-162">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="7f50c-163">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="7f50c-163">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="7f50c-164">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。</span><span class="sxs-lookup"><span data-stu-id="7f50c-164">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="7f50c-165">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="7f50c-165">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="7f50c-166">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="7f50c-166">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="7f50c-167">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="7f50c-167">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="7f50c-168">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-168">CompositeFileProvider</span></span>

<span data-ttu-id="7f50c-169"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-169">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7f50c-170">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="7f50c-170">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="7f50c-171">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="7f50c-171">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="7f50c-172">監視變更</span><span class="sxs-lookup"><span data-stu-id="7f50c-172">Watch for changes</span></span>

<span data-ttu-id="7f50c-173">[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-173">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7f50c-174">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-174">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="7f50c-175">`Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="7f50c-175">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="7f50c-176">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="7f50c-176">The change token exposes:</span></span>

* <span data-ttu-id="7f50c-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; 可檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="7f50c-177"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="7f50c-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; 當偵測到所指定路徑字串發生變更時要呼叫的項目。</span><span class="sxs-lookup"><span data-stu-id="7f50c-178"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7f50c-179">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-179">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7f50c-180">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-180">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7f50c-181">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="7f50c-181">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="7f50c-182">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="7f50c-182">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7f50c-183">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-183">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="7f50c-184">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="7f50c-184">Glob patterns</span></span>

<span data-ttu-id="7f50c-185">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="7f50c-185">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="7f50c-186">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="7f50c-186">Specify groups of files with these patterns.</span></span> <span data-ttu-id="7f50c-187">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="7f50c-187">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="7f50c-188">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="7f50c-188">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="7f50c-189">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="7f50c-189">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="7f50c-190">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="7f50c-190">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7f50c-191">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-191">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="7f50c-192">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="7f50c-192">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="7f50c-193">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-193">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="7f50c-194">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-194">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="7f50c-195">符合「目錄」資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-195">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="7f50c-196">符合在「目錄」資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-196">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7f50c-197">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="7f50c-197">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="7f50c-198">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="7f50c-198">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="7f50c-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> 會將應用程式的[內容根目錄](xref:fundamentals/index#content-root)和[web 根目錄](xref:fundamentals/index#web-root)公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="7f50c-199"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="7f50c-200">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-200">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="7f50c-201">[Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。</span><span class="sxs-lookup"><span data-stu-id="7f50c-201">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="7f50c-202">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-202">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="7f50c-203">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7f50c-203">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="7f50c-204">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="7f50c-204">File Provider interfaces</span></span>

<span data-ttu-id="7f50c-205">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="7f50c-205">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="7f50c-206">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="7f50c-206">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="7f50c-207">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="7f50c-207">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="7f50c-208">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="7f50c-208">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="7f50c-209">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="7f50c-209">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="7f50c-210">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="7f50c-210">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="7f50c-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="7f50c-211"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="7f50c-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="7f50c-212"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="7f50c-213">您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-213">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="7f50c-214">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="7f50c-214">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="7f50c-215">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="7f50c-215">File Provider implementations</span></span>

<span data-ttu-id="7f50c-216">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="7f50c-216">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="7f50c-217">實作</span><span class="sxs-lookup"><span data-stu-id="7f50c-217">Implementation</span></span> | <span data-ttu-id="7f50c-218">描述</span><span class="sxs-lookup"><span data-stu-id="7f50c-218">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="7f50c-219">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-219">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="7f50c-220">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-220">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="7f50c-221">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-221">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="7f50c-222">資訊清單內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-222">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="7f50c-223">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-223">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="7f50c-224">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="7f50c-224">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="7f50c-225">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-225">PhysicalFileProvider</span></span>

<span data-ttu-id="7f50c-226"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="7f50c-226">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="7f50c-227">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="7f50c-227">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="7f50c-228">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="7f50c-228">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="7f50c-229">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-229">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="7f50c-230">當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="7f50c-230">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="7f50c-231">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="7f50c-231">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="7f50c-232">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="7f50c-232">Types in the preceding example:</span></span>

* <span data-ttu-id="7f50c-233">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-233">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="7f50c-234">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-234">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="7f50c-235">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-235">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="7f50c-236">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="7f50c-236">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="7f50c-237">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="7f50c-237">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="7f50c-238">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="7f50c-238">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="7f50c-239">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-239">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="7f50c-240"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-240">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="7f50c-241">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="7f50c-241">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="7f50c-242">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="7f50c-242">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="7f50c-243">使用 [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 來指定要內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="7f50c-243">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="7f50c-244">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-244">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="7f50c-245">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="7f50c-245">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="7f50c-246">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="7f50c-246">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="7f50c-247">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="7f50c-247">Additional overloads allow you to:</span></span>

* <span data-ttu-id="7f50c-248">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="7f50c-248">Specify a relative file path.</span></span>
* <span data-ttu-id="7f50c-249">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="7f50c-249">Scope files to a last modified date.</span></span>
* <span data-ttu-id="7f50c-250">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="7f50c-250">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="7f50c-251">多載</span><span class="sxs-lookup"><span data-stu-id="7f50c-251">Overload</span></span> | <span data-ttu-id="7f50c-252">描述</span><span class="sxs-lookup"><span data-stu-id="7f50c-252">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="7f50c-253">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="7f50c-253">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="7f50c-254">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="7f50c-254">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="7f50c-255">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。</span><span class="sxs-lookup"><span data-stu-id="7f50c-255">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="7f50c-256">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="7f50c-256">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="7f50c-257">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="7f50c-257">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="7f50c-258">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="7f50c-258">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="7f50c-259">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="7f50c-259">CompositeFileProvider</span></span>

<span data-ttu-id="7f50c-260"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-260">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="7f50c-261">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="7f50c-261">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="7f50c-262">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="7f50c-262">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="7f50c-263">監視變更</span><span class="sxs-lookup"><span data-stu-id="7f50c-263">Watch for changes</span></span>

<span data-ttu-id="7f50c-264">[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-264">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="7f50c-265">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-265">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="7f50c-266">`Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="7f50c-266">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="7f50c-267">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="7f50c-267">The change token exposes:</span></span>

* <span data-ttu-id="7f50c-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; 可檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="7f50c-268"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="7f50c-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; 當偵測到所指定路徑字串發生變更時要呼叫的項目。</span><span class="sxs-lookup"><span data-stu-id="7f50c-269"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="7f50c-270">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-270">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="7f50c-271">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-271">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="7f50c-272">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="7f50c-272">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="7f50c-273">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="7f50c-273">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="7f50c-274">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="7f50c-274">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="7f50c-275">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="7f50c-275">Glob patterns</span></span>

<span data-ttu-id="7f50c-276">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="7f50c-276">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="7f50c-277">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="7f50c-277">Specify groups of files with these patterns.</span></span> <span data-ttu-id="7f50c-278">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="7f50c-278">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="7f50c-279">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="7f50c-279">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="7f50c-280">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="7f50c-280">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="7f50c-281">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="7f50c-281">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="7f50c-282">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-282">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="7f50c-283">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="7f50c-283">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="7f50c-284">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-284">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="7f50c-285">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-285">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="7f50c-286">符合「目錄」資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-286">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="7f50c-287">符合在「目錄」資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="7f50c-287">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
