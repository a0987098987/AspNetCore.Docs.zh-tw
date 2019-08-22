---
title: ASP.NET Core 中的檔案提供者
author: guardrex
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/30/2019
uid: fundamentals/file-providers
ms.openlocfilehash: b93b2df7fad7c173f43ad69aec865f09de6c9c34
ms.sourcegitcommit: 7a46973998623aead757ad386fe33602b1658793
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/15/2019
ms.locfileid: "69487583"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="f5cf3-103">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="f5cf3-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="f5cf3-104">作者：[Steve Smith](https://ardalis.com/) 和 [Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="f5cf3-104">By [Steve Smith](https://ardalis.com/) and [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="f5cf3-105">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="f5cf3-106">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-106">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="f5cf3-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) 將應用程式的內容根目錄與 Web 根目錄公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-107">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) exposes the app's content root and web root as `IFileProvider` types.</span></span>
* <span data-ttu-id="f5cf3-108">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-108">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="f5cf3-109">[Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-109">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="f5cf3-110">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-110">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="f5cf3-111">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f5cf3-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="f5cf3-112">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="f5cf3-112">File Provider interfaces</span></span>

<span data-ttu-id="f5cf3-113">主要介面是 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider)。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-113">The primary interface is [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> <span data-ttu-id="f5cf3-114">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-114">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="f5cf3-115">取得檔案資訊 ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo))。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-115">Obtain file information ([IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo)).</span></span>
* <span data-ttu-id="f5cf3-116">取得目錄資訊 ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents))。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-116">Obtain directory information ([IDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.idirectorycontents)).</span></span>
* <span data-ttu-id="f5cf3-117">設定變更通知 (使用 [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken))。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-117">Set up change notifications (using an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)).</span></span>

<span data-ttu-id="f5cf3-118">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-118">`IFileInfo` provides methods and properties for working with files:</span></span>

* [<span data-ttu-id="f5cf3-119">Exists</span><span class="sxs-lookup"><span data-stu-id="f5cf3-119">Exists</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.exists)
* [<span data-ttu-id="f5cf3-120">IsDirectory</span><span class="sxs-lookup"><span data-stu-id="f5cf3-120">IsDirectory</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.isdirectory)
* [<span data-ttu-id="f5cf3-121">名稱</span><span class="sxs-lookup"><span data-stu-id="f5cf3-121">Name</span></span>](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.name)
* <span data-ttu-id="f5cf3-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="f5cf3-122">[Length](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.length) (in bytes)</span></span>
* <span data-ttu-id="f5cf3-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) 日期</span><span class="sxs-lookup"><span data-stu-id="f5cf3-123">[LastModified](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.lastmodified) date</span></span>

<span data-ttu-id="f5cf3-124">您可以使用 [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-124">You can read from the file using the [IFileInfo.CreateReadStream](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo.createreadstream) method.</span></span>

<span data-ttu-id="f5cf3-125">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-125">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="f5cf3-126">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="f5cf3-126">File Provider implementations</span></span>

<span data-ttu-id="f5cf3-127">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-127">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="f5cf3-128">實作</span><span class="sxs-lookup"><span data-stu-id="f5cf3-128">Implementation</span></span> | <span data-ttu-id="f5cf3-129">說明</span><span class="sxs-lookup"><span data-stu-id="f5cf3-129">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="f5cf3-130">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="f5cf3-130">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="f5cf3-131">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-131">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="f5cf3-132">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="f5cf3-132">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="f5cf3-133">資訊清單內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-133">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="f5cf3-134">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="f5cf3-134">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="f5cf3-135">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-135">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="f5cf3-136">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="f5cf3-136">PhysicalFileProvider</span></span>

<span data-ttu-id="f5cf3-137">[PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-137">The [PhysicalFileProvider](/dotnet/api/microsoft.extensions.fileproviders.physicalfileprovider) provides access to the physical file system.</span></span> <span data-ttu-id="f5cf3-138">`PhysicalFileProvider` 會使用 [System.IO.File](/dotnet/api/system.io.file) 類型 (針對實體提供者) 並並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-138">`PhysicalFileProvider` uses the [System.IO.File](/dotnet/api/system.io.file) type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="f5cf3-139">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-139">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="f5cf3-140">當具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-140">When instantiating this provider, a directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="f5cf3-141">您可以直接具現化 `PhysicalFileProvider` 提供者，也可以透過[相依性插入](xref:fundamentals/dependency-injection)在建構函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-141">You can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="f5cf3-142">**靜態型別**</span><span class="sxs-lookup"><span data-stu-id="f5cf3-142">**Static types**</span></span>

<span data-ttu-id="f5cf3-143">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-143">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="f5cf3-144">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-144">Types in the preceding example:</span></span>

* <span data-ttu-id="f5cf3-145">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-145">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="f5cf3-146">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-146">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="f5cf3-147">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-147">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="f5cf3-148">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-148">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="f5cf3-149">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-149">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="f5cf3-150">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-150">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.contentrootfileprovider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

<span data-ttu-id="f5cf3-151">**使用相依性插入取得檔案提供者型別**</span><span class="sxs-lookup"><span data-stu-id="f5cf3-151">**Obtain File Provider types with dependency injection**</span></span>

<span data-ttu-id="f5cf3-152">將提供者插入到任何類別建構函式，並將它指派給區域欄位。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-152">Inject the provider into any class constructor and assign it to a local field.</span></span> <span data-ttu-id="f5cf3-153">在類別的方法中使用該欄位來存取檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-153">Use the field throughout the class's methods to access files.</span></span>

<span data-ttu-id="f5cf3-154">在範例應用程式中，`IndexModel` 類別會接收 `IFileProvider` 執行個體以取得應用程式基底路徑的目錄內容。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-154">In the sample app, the `IndexModel` class receives an `IFileProvider` instance to obtain directory contents for the app's base path.</span></span>

<span data-ttu-id="f5cf3-155">*Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-155">*Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="f5cf3-156">會在頁面中逐一查看 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-156">The `IDirectoryContents` are iterated in the page.</span></span>

<span data-ttu-id="f5cf3-157">*Pages/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-157">*Pages/Index.cshtml*:</span></span>

[!code-cshtml[](file-providers/samples/2.x/FileProviderSample/Pages/Index.cshtml?name=snippet1)]

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="f5cf3-158">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="f5cf3-158">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="f5cf3-159">[ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) 是用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-159">The [ManifestEmbeddedFileProvider](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider) is used to access files embedded within assemblies.</span></span> <span data-ttu-id="f5cf3-160">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-160">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="f5cf3-161">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-161">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="f5cf3-162">使用 [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) 來指定要內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-162">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="f5cf3-163">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-163">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="f5cf3-164">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-164">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="f5cf3-165">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-165">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="f5cf3-166">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-166">Additional overloads allow you to:</span></span>

* <span data-ttu-id="f5cf3-167">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-167">Specify a relative file path.</span></span>
* <span data-ttu-id="f5cf3-168">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-168">Scope files to a last modified date.</span></span>
* <span data-ttu-id="f5cf3-169">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-169">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="f5cf3-170">多載</span><span class="sxs-lookup"><span data-stu-id="f5cf3-170">Overload</span></span> | <span data-ttu-id="f5cf3-171">說明</span><span class="sxs-lookup"><span data-stu-id="f5cf3-171">Description</span></span> |
| -------- | ----------- |
| [<span data-ttu-id="f5cf3-172">ManifestEmbeddedFileProvider(Assembly, String)</span><span class="sxs-lookup"><span data-stu-id="f5cf3-172">ManifestEmbeddedFileProvider(Assembly, String)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_) | <span data-ttu-id="f5cf3-173">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-173">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="f5cf3-174">指定 `root` 以將對 [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-174">Specify the `root` to scope calls to [GetDirectoryContents](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.getdirectorycontents) to those resources under the provided path.</span></span> |
| [<span data-ttu-id="f5cf3-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="f5cf3-175">ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_DateTimeOffset_) | <span data-ttu-id="f5cf3-176">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) 參數。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-176">Accepts an optional `root` relative path parameter and a `lastModified` date ([DateTimeOffset](/dotnet/api/system.datetimeoffset)) parameter.</span></span> <span data-ttu-id="f5cf3-177">`lastModified` 日期會限定為 [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) 執行個體 (由 [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider) 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-177">The `lastModified` date scopes the last modification date for the [IFileInfo](/dotnet/api/microsoft.extensions.fileproviders.ifileinfo) instances returned by the [IFileProvider](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider).</span></span> |
| [<span data-ttu-id="f5cf3-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span><span class="sxs-lookup"><span data-stu-id="f5cf3-178">ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)</span></span>](/dotnet/api/microsoft.extensions.fileproviders.manifestembeddedfileprovider.-ctor#Microsoft_Extensions_FileProviders_ManifestEmbeddedFileProvider__ctor_System_Reflection_Assembly_System_String_System_String_System_DateTimeOffset_) | <span data-ttu-id="f5cf3-179">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-179">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="f5cf3-180">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-180">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="f5cf3-181">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="f5cf3-181">CompositeFileProvider</span></span>

<span data-ttu-id="f5cf3-182">[CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-182">The [CompositeFileProvider](/dotnet/api/microsoft.extensions.fileproviders.compositefileprovider) combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="f5cf3-183">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-183">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="f5cf3-184">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-184">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="f5cf3-185">監視變更</span><span class="sxs-lookup"><span data-stu-id="f5cf3-185">Watch for changes</span></span>

<span data-ttu-id="f5cf3-186">[IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-186">The [IFileProvider.Watch](/dotnet/api/microsoft.extensions.fileproviders.ifileprovider.watch) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="f5cf3-187">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-187">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="f5cf3-188">`Watch` 會傳回 [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken)。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-188">`Watch` returns an [IChangeToken](/dotnet/api/microsoft.extensions.primitives.ichangetoken).</span></span> <span data-ttu-id="f5cf3-189">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-189">The change token exposes:</span></span>

* <span data-ttu-id="f5cf3-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged)：可經檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-190">[HasChanged](/dotnet/api/microsoft.extensions.primitives.ichangetoken.haschanged): A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="f5cf3-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback)：當偵測到所指定路徑字串發生變更時要呼叫的項目。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-191">[RegisterChangeCallback](/dotnet/api/microsoft.extensions.primitives.ichangetoken.registerchangecallback): Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="f5cf3-192">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-192">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="f5cf3-193">若要啟用持續監視，請使用 [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-193">To enable constant monitoring, use a [TaskCompletionSource](/dotnet/api/system.threading.tasks.taskcompletionsource-1) (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="f5cf3-194">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-194">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="f5cf3-195">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-195">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="f5cf3-196">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-196">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="f5cf3-197">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="f5cf3-197">Glob patterns</span></span>

<span data-ttu-id="f5cf3-198">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-198">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="f5cf3-199">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-199">Specify groups of files with these patterns.</span></span> <span data-ttu-id="f5cf3-200">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="f5cf3-200">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="f5cf3-201">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-201">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="f5cf3-202">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-202">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="f5cf3-203">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-203">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="f5cf3-204">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-204">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="f5cf3-205">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="f5cf3-205">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="f5cf3-206">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-206">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="f5cf3-207">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-207">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="f5cf3-208">符合「目錄」  資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-208">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="f5cf3-209">符合在「目錄」  資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="f5cf3-209">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>
