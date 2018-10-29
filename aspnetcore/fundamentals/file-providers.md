---
title: ASP.NET Core 中的檔案提供者
author: guardrex
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
ms.author: riande
ms.custom: mvc
ms.date: 08/01/2018
uid: fundamentals/file-providers
ms.openlocfilehash: 3274615a0d6b6f928301ce97d18f5d9768963a30
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207312"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="77ba3-103">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="77ba3-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="77ba3-104">作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="77ba3-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="77ba3-105">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="77ba3-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="77ba3-106">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="77ba3-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="77ba3-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) 將應用程式的內容根目錄與 Web 根目錄公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="77ba3-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="77ba3-108">[靜態檔案中介軟體](xref:fundamentals/static-files)使用「檔案提供者」來尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-108">[Static Files Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="77ba3-109">[Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。</span><span class="sxs-lookup"><span data-stu-id="77ba3-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="77ba3-110">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="77ba3-111">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="77ba3-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="77ba3-112">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="77ba3-112">File Provider interfaces</span></span>

<span data-ttu-id="77ba3-113">主要介面是 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)。</span><span class="sxs-lookup"><span data-stu-id="77ba3-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="77ba3-114">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="77ba3-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="77ba3-115">取得檔案資訊 ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo))。</span><span class="sxs-lookup"><span data-stu-id="77ba3-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="77ba3-116">取得目錄資訊 ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents))。</span><span class="sxs-lookup"><span data-stu-id="77ba3-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="77ba3-117">設定變更通知 (使用 [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken))。</span><span class="sxs-lookup"><span data-stu-id="77ba3-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="77ba3-118">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="77ba3-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="77ba3-119">Exists</span><span class="sxs-lookup"><span data-stu-id="77ba3-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="77ba3-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="77ba3-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="77ba3-121">名稱</span><span class="sxs-lookup"><span data-stu-id="77ba3-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="77ba3-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="77ba3-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="77ba3-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) 日期</span><span class="sxs-lookup"><span data-stu-id="77ba3-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="77ba3-124">您可以使用 [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="77ba3-125">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="77ba3-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="77ba3-126">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="77ba3-126">File Provider implementations</span></span>

<span data-ttu-id="77ba3-127">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="77ba3-127">Three implementations of `IFileProvider` are available.</span></span>

::: moniker range=">= aspnetcore-2.0"

| <span data-ttu-id="77ba3-128">實作</span><span class="sxs-lookup"><span data-stu-id="77ba3-128">Implementation</span></span> | <span data-ttu-id="77ba3-129">描述</span><span class="sxs-lookup"><span data-stu-id="77ba3-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="77ba3-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="77ba3-131">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="77ba3-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="77ba3-133">資訊清單內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="77ba3-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="77ba3-135">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="77ba3-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

| <span data-ttu-id="77ba3-136">實作</span><span class="sxs-lookup"><span data-stu-id="77ba3-136">Implementation</span></span> | <span data-ttu-id="77ba3-137">描述</span><span class="sxs-lookup"><span data-stu-id="77ba3-137">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="77ba3-138">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-138">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="77ba3-139">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-139">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="77ba3-140">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-140">EmbeddedFileProvider</span></span>](#embeddedfileprovider) | <span data-ttu-id="77ba3-141">內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-141">The embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="77ba3-142">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-142">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="77ba3-143">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="77ba3-143">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

::: moniker-end

### <a name="physicalfileprovider"></a><span data-ttu-id="77ba3-144">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-144">PhysicalFileProvider</span></span>

<span data-ttu-id="77ba3-145">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="77ba3-145">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="77ba3-146">`PhysicalFileProvider` 會使用 [System.IO.File](/dotnet/api/system.io.file) 類型 (針對實體提供者) 並並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="77ba3-146">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="77ba3-147">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="77ba3-147">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="77ba3-148">當具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="77ba3-148">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="77ba3-149">您可以直接具現化 `PhysicalFileProvider` 提供者，也可以透過[相依性插入](xref:fundamentals/dependency-injection)在建構函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-149">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="77ba3-150">**靜態型別**</span><span class="sxs-lookup"><span data-stu-id="77ba3-150">**Static types**</span></span>

<span data-ttu-id="77ba3-151">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="77ba3-151">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="77ba3-152">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="77ba3-152">Types in the preceding example:</span></span>

* <span data-ttu-id="77ba3-153">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-153">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="77ba3-154">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-154">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="77ba3-155">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-155">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="77ba3-156">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="77ba3-156">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="77ba3-157">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="77ba3-157">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="77ba3-158">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="77ba3-158">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="77ba3-159">**使用相依性插入取得檔案提供者型別**</span><span class="sxs-lookup"><span data-stu-id="77ba3-159">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="77ba3-160">將提供者插入到任何類別建構函式，並將它指派給區域欄位。</span><span class="sxs-lookup"><span data-stu-id="77ba3-160">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="77ba3-161">在類別的方法中使用該欄位來存取檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-161">Use the field throughout the class's methods to access files.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="77ba3-162">在範例應用程式中，`IndexModel` 類別會接收 `IFileProvider` 執行個體以取得應用程式基底路徑的目錄內容。</span><span class="sxs-lookup"><span data-stu-id="77ba3-162">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="77ba3-163">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="77ba3-163">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="77ba3-164">會在頁面中逐一查看 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-164">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="77ba3-165">*Pages/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="77ba3-165">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="77ba3-166">在範例應用程式中，`HomeController` 類別會接收 `IFileProvider` 執行個體以取得應用程式基底路徑的目錄內容。</span><span class="sxs-lookup"><span data-stu-id="77ba3-166">In the sample app, the `HomeController` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="77ba3-167">*Controllers/HomeController.cs*：</span><span class="sxs-lookup"><span data-stu-id="77ba3-167">*Controllers/HomeController.cs*:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Controllers/HomeController.cs?name=snippet1)]

<span data-ttu-id="77ba3-168">會在檢視中逐一查看 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-168">The `IDirectoryContents` are iterated in the view.</span></span>

<span data-ttu-id="77ba3-169">*Views/Home/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="77ba3-169">*Views/Home/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/1.x/FileProviderSample/Views/Home/Index.cshtml?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="77ba3-170">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-170">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="77ba3-171">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) 是用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-171">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="77ba3-172">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="77ba3-172">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

> [!NOTE]
> <span data-ttu-id="77ba3-173">ASP.NET Core 2.1 或更新版本中提供了 `ManifestEmbeddedFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-173">The `ManifestEmbeddedFileProvider` is available in ASP.NET Core 2.1 or later.</span></span> <span data-ttu-id="77ba3-174">若要在 ASP.NET Core 2.0 或更舊的版本中存取內嵌於組件中的檔案，請參閱[此主題的 ASP.NET Core 1.x 版本](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1)。</span><span class="sxs-lookup"><span data-stu-id="77ba3-174">To access files embedded in assemblies in ASP.NET Core 2.0 or earlier, see the [ASP.NET Core 1.x version of this topic](/aspnet/core/fundamentals/file-providers?view=aspnetcore-1.1).</span></span>

<span data-ttu-id="77ba3-175">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-175">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="77ba3-176">使用 [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 來指定要內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="77ba3-176">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="77ba3-177">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-177">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="77ba3-178">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="77ba3-178">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="77ba3-179">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="77ba3-179">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="77ba3-180">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="77ba3-180">Additional overloads allow you to:</span></span>

* <span data-ttu-id="77ba3-181">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="77ba3-181">Specify a relative file path.</span></span>
* <span data-ttu-id="77ba3-182">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="77ba3-182">Scope files to a last modified date.</span></span>
* <span data-ttu-id="77ba3-183">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="77ba3-183">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="77ba3-184">多載</span><span class="sxs-lookup"><span data-stu-id="77ba3-184">Overload</span></span> | <span data-ttu-id="77ba3-185">描述</span><span class="sxs-lookup"><span data-stu-id="77ba3-185">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="77ba3-186">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="77ba3-186">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="77ba3-187">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="77ba3-187">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="77ba3-188">指定 `root` 以將對 [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="77ba3-188">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="77ba3-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="77ba3-189">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="77ba3-190">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) 參數。</span><span class="sxs-lookup"><span data-stu-id="77ba3-190">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="77ba3-191">`lastModified` 日期會限定為 [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) 執行個體 (由 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="77ba3-191">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="77ba3-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="77ba3-192">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="77ba3-193">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="77ba3-193">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="77ba3-194">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="77ba3-194">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.0"

### <a name="embeddedfileprovider"></a><span data-ttu-id="77ba3-195">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-195">EmbeddedFileProvider</span></span>

<span data-ttu-id="77ba3-196">[EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) 是用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-196">The [EmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="77ba3-197">在專案檔中使用 [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 屬性來指定要內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="77ba3-197">Specify the files to embed with the [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) property in the project file:</span></span>

```xml
<ItemGroup>
  <EmbeddedResource Include="Resource.txt" />
</ItemGroup>
```

<span data-ttu-id="77ba3-198">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-198">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="77ba3-199">範例應用程式會建立 `EmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="77ba3-199">The sample app creates an `EmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="77ba3-200">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="77ba3-200">*Startup.cs*:</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="77ba3-201">內嵌的資源不會公開目錄。</span><span class="sxs-lookup"><span data-stu-id="77ba3-201">Embedded resources don't expose directories.</span></span> <span data-ttu-id="77ba3-202">而是使用 `.` 分隔符號，將資源的路徑 (透過其命名空間) 內嵌在其檔案名稱中。</span><span class="sxs-lookup"><span data-stu-id="77ba3-202">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span> <span data-ttu-id="77ba3-203">在範例應用程式中，`baseNamespace` 是 `FileProviderSample.`。</span><span class="sxs-lookup"><span data-stu-id="77ba3-203">In the sample app, the `baseNamespace` is `FileProviderSample.`.</span></span>

<span data-ttu-id="77ba3-204">[EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) 建構函式接受選擇性的 `baseNamespace` 參數。</span><span class="sxs-lookup"><span data-stu-id="77ba3-204">The [EmbeddedFileProvider(Assembly, String)](/dotnet/api/microsoft.extensions.fileproviders.embeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_EmbeddedFileProvider__ctor_System_Reflection_Assembly_) constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="77ba3-205">指定基底命名空間以將對 [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) 的呼叫限定為所提供命名空間下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="77ba3-205">Specify the base namespace to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided namespace.</span></span>

::: moniker-end

### <a name="compositefileprovider"></a><span data-ttu-id="77ba3-206">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="77ba3-206">CompositeFileProvider</span></span>

<span data-ttu-id="77ba3-207">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-207">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="77ba3-208">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="77ba3-208">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="77ba3-209">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="77ba3-209">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="77ba3-210">在範例應用程式中，`PhysicalFileProvider` 與 `EmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="77ba3-210">In the sample app, a `PhysicalFileProvider` and an `EmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/1.x/FileProviderSample/Startup.cs?name=snippet1)]

::: moniker-end

## <a name="watch-for-changes"></a><span data-ttu-id="77ba3-211">監視變更</span><span class="sxs-lookup"><span data-stu-id="77ba3-211">Watch for changes</span></span>

<span data-ttu-id="77ba3-212">[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="77ba3-212">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="77ba3-213">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-213">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="77ba3-214">`Watch` 會傳回 [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)。</span><span class="sxs-lookup"><span data-stu-id="77ba3-214">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="77ba3-215">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="77ba3-215">The change token exposes:</span></span>

* <span data-ttu-id="77ba3-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)：可以檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="77ba3-216">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="77ba3-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)：當偵測到所指定的路徑字串發生變更時要呼叫的項目。</span><span class="sxs-lookup"><span data-stu-id="77ba3-217">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="77ba3-218">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="77ba3-218">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="77ba3-219">若要啟用持續監視，請使用 [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="77ba3-219">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="77ba3-220">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="77ba3-220">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](file-providers/samples/1.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

::: moniker-end

<span data-ttu-id="77ba3-221">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="77ba3-221">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="77ba3-222">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="77ba3-222">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="77ba3-223">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="77ba3-223">Glob patterns</span></span>

<span data-ttu-id="77ba3-224">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="77ba3-224">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="77ba3-225">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="77ba3-225">Specify groups of files with these patterns.</span></span> <span data-ttu-id="77ba3-226">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="77ba3-226">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="77ba3-227">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="77ba3-227">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="77ba3-228">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="77ba3-228">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="77ba3-229">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="77ba3-229">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="77ba3-230">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-230">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="77ba3-231">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="77ba3-231">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="77ba3-232">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-232">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="77ba3-233">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-233">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="77ba3-234">符合「目錄」資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-234">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="77ba3-235">符合在「目錄」資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="77ba3-235">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
