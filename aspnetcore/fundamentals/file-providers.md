---
title: ASP.NET Core 中的檔案提供者
author: rick-anderson
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2020
uid: fundamentals/file-providers
ms.openlocfilehash: 25607bd534cae05a6c6b11fa6d8902faa3c0684c
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80751097"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="e0a4a-103">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="e0a4a-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="e0a4a-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="e0a4a-104">By [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e0a4a-105">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="e0a4a-106">檔提供程式在整個ASP.NET核心框架中使用。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-106">File Providers are used throughout the ASP.NET Core framework.</span></span> <span data-ttu-id="e0a4a-107">例如：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-107">For example:</span></span>

* <span data-ttu-id="e0a4a-108"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>將應用[的內容根](xref:fundamentals/index#content-root)和 Web`IFileProvider`[根](xref:fundamentals/index#web-root)公開為 類型。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-108"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="e0a4a-109">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-109">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="e0a4a-110">[Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-110">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="e0a4a-111">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-111">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="e0a4a-112">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e0a4a-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="e0a4a-113">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="e0a4a-113">File Provider interfaces</span></span>

<span data-ttu-id="e0a4a-114">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-114">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="e0a4a-115">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-115">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="e0a4a-116">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-116">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="e0a4a-117">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-117">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="e0a4a-118">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-118">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="e0a4a-119">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-119">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="e0a4a-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="e0a4a-120"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="e0a4a-121"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="e0a4a-121"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="e0a4a-122">可以使用<xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*?displayProperty=nameWithType>方法從檔中讀取。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-122">You can read from the file using the <xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="e0a4a-123">*FileProviderSample*範例應用程式示範如何設定 檔提供`Startup.ConfigureServices`者, 以便透過[依賴項注入](xref:fundamentals/dependency-injection)在整個應用中使用。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-123">The *FileProviderSample* sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="e0a4a-124">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="e0a4a-124">File Provider implementations</span></span>

<span data-ttu-id="e0a4a-125">下表列出了`IFileProvider`的 實現。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-125">The following table lists implementations of `IFileProvider`.</span></span>

| <span data-ttu-id="e0a4a-126">實作</span><span class="sxs-lookup"><span data-stu-id="e0a4a-126">Implementation</span></span> | <span data-ttu-id="e0a4a-127">描述</span><span class="sxs-lookup"><span data-stu-id="e0a4a-127">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="e0a4a-128">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-128">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="e0a4a-129">用於提供對來自一個或多個其他提供程式的文件和目錄的聯合訪問。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-129">Used to provide combined access to files and directories from one or more other providers.</span></span> |
| [<span data-ttu-id="e0a4a-130">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-130">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="e0a4a-131">用於存取嵌入在程式集中的檔。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-131">Used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="e0a4a-132">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-132">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="e0a4a-133">用於訪問系統的物理檔。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-133">Used to access the system's physical files.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="e0a4a-134">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-134">PhysicalFileProvider</span></span>

<span data-ttu-id="e0a4a-135"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-135">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="e0a4a-136">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-136">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="e0a4a-137">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-137">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="e0a4a-138">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-138">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="e0a4a-139">直接實例化此提供程式時,需要絕對目錄路徑,並用作使用提供程式發出的所有請求的基本路徑。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-139">When instantiating this provider directly, an absolute directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="e0a4a-140">目錄路徑中不支援 Glob 模式。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-140">Glob patterns aren't supported in the directory path.</span></span>

<span data-ttu-id="e0a4a-141">以下程式碼示範如何`PhysicalFileProvider`用於 取得目錄內容與檔案資訊:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-141">The following code shows how to use `PhysicalFileProvider` to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var filePath = Path.Combine("wwwroot", "js", "site.js");
var fileInfo = provider.GetFileInfo(filePath);
```

<span data-ttu-id="e0a4a-142">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-142">Types in the preceding example:</span></span>

* <span data-ttu-id="e0a4a-143">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-143">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="e0a4a-144">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-144">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="e0a4a-145">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-145">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="e0a4a-146">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-146">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="e0a4a-147">Glob 模式無法傳遞`GetFileInfo`給 方法。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-147">Glob patterns can't be passed to the `GetFileInfo` method.</span></span> <span data-ttu-id="e0a4a-148">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-148">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="e0a4a-149">*FileProviderSample*範例使用`Startup.ConfigureServices`<xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider?displayProperty=nameWithType>: 在 方法中建立提供者:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-149">The *FileProviderSample* sample app creates the provider in the `Startup.ConfigureServices` method using <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider?displayProperty=nameWithType>:</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="e0a4a-150">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-150">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="e0a4a-151"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-151">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="e0a4a-152">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-152">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="e0a4a-153">要產生嵌入檔案的清單,可以:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-153">To generate a manifest of the embedded files:</span></span>

1. <span data-ttu-id="e0a4a-154">將[Microsoft.擴展.檔案提供者.嵌入式](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded)NuGet 套件添加到您的專案中。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-154">Add the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) NuGet package to your project.</span></span>
1. <span data-ttu-id="e0a4a-155">將 `<GenerateEmbeddedFilesManifest>` 屬性設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-155">Set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="e0a4a-156">指定要嵌入與[\<嵌入式資源>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)的檔案:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-156">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

    [!code-xml[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="e0a4a-157">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-157">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="e0a4a-158">*FileProviderSample*範例應用`ManifestEmbeddedFileProvider`創建 並將目前正在執行的程式集傳遞給其建構函數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-158">The *FileProviderSample* sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="e0a4a-159">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-159">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="e0a4a-160">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-160">Additional overloads allow you to:</span></span>

* <span data-ttu-id="e0a4a-161">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-161">Specify a relative file path.</span></span>
* <span data-ttu-id="e0a4a-162">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-162">Scope files to a last modified date.</span></span>
* <span data-ttu-id="e0a4a-163">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-163">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="e0a4a-164">多載</span><span class="sxs-lookup"><span data-stu-id="e0a4a-164">Overload</span></span> | <span data-ttu-id="e0a4a-165">描述</span><span class="sxs-lookup"><span data-stu-id="e0a4a-165">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="e0a4a-166">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-166">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="e0a4a-167">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-167">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="e0a4a-168">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-168">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="e0a4a-169">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-169">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="e0a4a-170">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-170">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="e0a4a-171">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-171">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="e0a4a-172">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-172">CompositeFileProvider</span></span>

<span data-ttu-id="e0a4a-173"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-173">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="e0a4a-174">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-174">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="e0a4a-175">在*FileProviderSample*範例應用中`PhysicalFileProvider``ManifestEmbeddedFileProvider`,a`CompositeFileProvider`和 a 向應用的服務容器中註冊提供檔。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-175">In the *FileProviderSample* sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container.</span></span> <span data-ttu-id="e0a4a-176">以下代碼位於專案`Startup.ConfigureServices`的方法中:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-176">The following code is found in the project's `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="e0a4a-177">監視變更</span><span class="sxs-lookup"><span data-stu-id="e0a4a-177">Watch for changes</span></span>

<span data-ttu-id="e0a4a-178">該方法<xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*?displayProperty=nameWithType>提供了一個方案,用於監視一個或多個檔或目錄以進行更改。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-178">The <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*?displayProperty=nameWithType> method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="e0a4a-179">`Watch` 方法：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-179">The `Watch` method:</span></span>

* <span data-ttu-id="e0a4a-180">接受檔案路徑字串,該字串可以使用[glob 模式](#glob-patterns)指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-180">Accepts a file path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span>
* <span data-ttu-id="e0a4a-181">傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-181">Returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span>

<span data-ttu-id="e0a4a-182">產生的變更權杖公開:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-182">The resulting change token exposes:</span></span>

* <span data-ttu-id="e0a4a-183"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash;可以檢查以確定是否發生了更改的屬性。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-183"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash;A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="e0a4a-184"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash;檢測到對指定路徑字串的更改時調用。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-184"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash;Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="e0a4a-185">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-185">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="e0a4a-186">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-186">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="e0a4a-187">每當*修改 TextFiles*目錄中的 *.txt*檔時 *,WatchConsole*範例應用程式都會寫入一條訊息:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-187">The *WatchConsole* sample app writes a message whenever a *.txt* file in the *TextFiles* directory is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1)]

<span data-ttu-id="e0a4a-188">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-188">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="e0a4a-189">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-189">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

### <a name="glob-patterns"></a><span data-ttu-id="e0a4a-190">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="e0a4a-190">Glob patterns</span></span>

<span data-ttu-id="e0a4a-191">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-191">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="e0a4a-192">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-192">Specify groups of files with these patterns.</span></span> <span data-ttu-id="e0a4a-193">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-193">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="e0a4a-194">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-194">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="e0a4a-195">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-195">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="e0a4a-196">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-196">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="e0a4a-197">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-197">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="e0a4a-198">下表提供了球形模式的常見示例。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-198">The following table provides common examples of glob patterns.</span></span>

|<span data-ttu-id="e0a4a-199">模式</span><span class="sxs-lookup"><span data-stu-id="e0a4a-199">Pattern</span></span>  |<span data-ttu-id="e0a4a-200">描述</span><span class="sxs-lookup"><span data-stu-id="e0a4a-200">Description</span></span>  |
|---------|---------|
|`directory/file.txt`|<span data-ttu-id="e0a4a-201">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-201">Matches a specific file in a specific directory.</span></span>|
|`directory/*.txt`|<span data-ttu-id="e0a4a-202">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-202">Matches all files with *.txt* extension in a specific directory.</span></span>|
|`directory/*/appsettings.json`|<span data-ttu-id="e0a4a-203">匹配目錄中的所有*應用設置.json*檔,正好在*目錄*資料夾的一級以下。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-203">Matches all *appsettings.json* files in directories exactly one level below the *directory* folder.</span></span>|
|`directory/**/*.txt`|<span data-ttu-id="e0a4a-204">將所有文件與*目錄*資料夾下任何位置找到的 *.txt*擴展名進行匹配。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-204">Matches all files with a *.txt* extension found anywhere under the *directory* folder.</span></span>|

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e0a4a-205">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-205">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="e0a4a-206">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-206">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="e0a4a-207"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>將應用[的內容根](xref:fundamentals/index#content-root)和 Web`IFileProvider`[根](xref:fundamentals/index#web-root)公開為 類型。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-207"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="e0a4a-208">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-208">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="e0a4a-209">[Razor](xref:mvc/views/razor) 使用「檔案提供者」來尋找頁面與檢視。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-209">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="e0a4a-210">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-210">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="e0a4a-211">[檢視或下載範例代碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples)([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e0a4a-211">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="e0a4a-212">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="e0a4a-212">File Provider interfaces</span></span>

<span data-ttu-id="e0a4a-213">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-213">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="e0a4a-214">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-214">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="e0a4a-215">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-215">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="e0a4a-216">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-216">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="e0a4a-217">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-217">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="e0a4a-218">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-218">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="e0a4a-219"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="e0a4a-219"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="e0a4a-220"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="e0a4a-220"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="e0a4a-221">您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-221">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="e0a4a-222">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-222">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="e0a4a-223">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="e0a4a-223">File Provider implementations</span></span>

<span data-ttu-id="e0a4a-224">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-224">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="e0a4a-225">實作</span><span class="sxs-lookup"><span data-stu-id="e0a4a-225">Implementation</span></span> | <span data-ttu-id="e0a4a-226">描述</span><span class="sxs-lookup"><span data-stu-id="e0a4a-226">Description</span></span> |
| -------------- | ----------- |
| [<span data-ttu-id="e0a4a-227">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-227">PhysicalFileProvider</span></span>](#physicalfileprovider) | <span data-ttu-id="e0a4a-228">實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-228">The physical provider is used to access the system's physical files.</span></span> |
| [<span data-ttu-id="e0a4a-229">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-229">ManifestEmbeddedFileProvider</span></span>](#manifestembeddedfileprovider) | <span data-ttu-id="e0a4a-230">資訊清單內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-230">The manifest embedded provider is used to access files embedded in assemblies.</span></span> |
| [<span data-ttu-id="e0a4a-231">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-231">CompositeFileProvider</span></span>](#compositefileprovider) | <span data-ttu-id="e0a4a-232">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-232">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="e0a4a-233">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-233">PhysicalFileProvider</span></span>

<span data-ttu-id="e0a4a-234"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-234">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="e0a4a-235">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-235">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="e0a4a-236">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-236">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="e0a4a-237">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-237">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="e0a4a-238">當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-238">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="e0a4a-239">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-239">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="e0a4a-240">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-240">Types in the preceding example:</span></span>

* <span data-ttu-id="e0a4a-241">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-241">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="e0a4a-242">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-242">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="e0a4a-243">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-243">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="e0a4a-244">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-244">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="e0a4a-245">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-245">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="e0a4a-246">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-246">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="e0a4a-247">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-247">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="e0a4a-248"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-248">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="e0a4a-249">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-249">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="e0a4a-250">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-250">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="e0a4a-251">指定要嵌入與[&lt;嵌入式資源的&gt;檔案 :](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)</span><span class="sxs-lookup"><span data-stu-id="e0a4a-251">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="e0a4a-252">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-252">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="e0a4a-253">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-253">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="e0a4a-254">*Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="e0a4a-254">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="e0a4a-255">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-255">Additional overloads allow you to:</span></span>

* <span data-ttu-id="e0a4a-256">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-256">Specify a relative file path.</span></span>
* <span data-ttu-id="e0a4a-257">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-257">Scope files to a last modified date.</span></span>
* <span data-ttu-id="e0a4a-258">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-258">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="e0a4a-259">多載</span><span class="sxs-lookup"><span data-stu-id="e0a4a-259">Overload</span></span> | <span data-ttu-id="e0a4a-260">描述</span><span class="sxs-lookup"><span data-stu-id="e0a4a-260">Description</span></span> |
| -------- | ----------- |
| `ManifestEmbeddedFileProvider(Assembly, String)` | <span data-ttu-id="e0a4a-261">接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-261">Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="e0a4a-262">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-262">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | <span data-ttu-id="e0a4a-263">接受選擇性的 `root` 相對路徑參數與 `lastModified` 日期 (<xref:System.DateTimeOffset>) 參數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-263">Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="e0a4a-264">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-264">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> |
| `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | <span data-ttu-id="e0a4a-265">接受選擇性的 `root` 相對路徑、`lastModified` 日期與 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-265">Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="e0a4a-266">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-266">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="e0a4a-267">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="e0a4a-267">CompositeFileProvider</span></span>

<span data-ttu-id="e0a4a-268"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-268">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="e0a4a-269">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-269">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="e0a4a-270">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-270">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="e0a4a-271">監視變更</span><span class="sxs-lookup"><span data-stu-id="e0a4a-271">Watch for changes</span></span>

<span data-ttu-id="e0a4a-272">[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-272">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="e0a4a-273">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-273">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="e0a4a-274">`Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-274">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="e0a4a-275">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-275">The change token exposes:</span></span>

* <span data-ttu-id="e0a4a-276"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>&ndash;可以檢查以確定是否發生了更改的屬性。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-276"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged> &ndash; A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="e0a4a-277"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>&ndash;檢測到對指定路徑字串的更改時調用。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-277"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*> &ndash; Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="e0a4a-278">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-278">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="e0a4a-279">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-279">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="e0a4a-280">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-280">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="e0a4a-281">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-281">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="e0a4a-282">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-282">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="e0a4a-283">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="e0a4a-283">Glob patterns</span></span>

<span data-ttu-id="e0a4a-284">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-284">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="e0a4a-285">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-285">Specify groups of files with these patterns.</span></span> <span data-ttu-id="e0a4a-286">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="e0a4a-286">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="e0a4a-287">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-287">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="e0a4a-288">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-288">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="e0a4a-289">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-289">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="e0a4a-290">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-290">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="e0a4a-291">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="e0a4a-291">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="e0a4a-292">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-292">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="e0a4a-293">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-293">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="e0a4a-294">符合「目錄」\*\* 資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-294">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="e0a4a-295">符合在「目錄」\*\* 資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="e0a4a-295">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
