---
<span data-ttu-id="1f2c3-101">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-101">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-102">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-103">'Identity'</span></span>
- <span data-ttu-id="1f2c3-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-105">'Razor'</span></span>
- <span data-ttu-id="1f2c3-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-106">'SignalR' uid:</span></span> 

---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="1f2c3-107">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="1f2c3-107">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="1f2c3-108">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="1f2c3-108">By [Steve Smith](https://ardalis.com/)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1f2c3-109">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-109">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="1f2c3-110">檔案提供者會在整個 ASP.NET Core 架構中使用。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-110">File Providers are used throughout the ASP.NET Core framework.</span></span> <span data-ttu-id="1f2c3-111">例如：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-111">For example:</span></span>

* <span data-ttu-id="1f2c3-112"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>將應用程式的[內容根目錄](xref:fundamentals/index#content-root)和[web 根目錄](xref:fundamentals/index#web-root)公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-112"><xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="1f2c3-113">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-113">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="1f2c3-114">[Razor](xref:mvc/views/razor)會使用檔案提供者來尋找頁面和瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-114">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="1f2c3-115">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-115">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="1f2c3-116">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1f2c3-116">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="1f2c3-117">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="1f2c3-117">File Provider interfaces</span></span>

<span data-ttu-id="1f2c3-118">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-118">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="1f2c3-119">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-119">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="1f2c3-120">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-120">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="1f2c3-121">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-121">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="1f2c3-122">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-122">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="1f2c3-123">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-123">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="1f2c3-124"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="1f2c3-124"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="1f2c3-125"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="1f2c3-125"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="1f2c3-126">您可以使用方法，從檔案讀取 <xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*?displayProperty=nameWithType> 。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-126">You can read from the file using the <xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="1f2c3-127">*FileProviderSample*範例應用程式示範如何在中設定檔案提供者，以透過相依性 `Startup.ConfigureServices` [插入](xref:fundamentals/dependency-injection)在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-127">The *FileProviderSample* sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="1f2c3-128">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="1f2c3-128">File Provider implementations</span></span>

<span data-ttu-id="1f2c3-129">下表列出的執行 `IFileProvider` 。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-129">The following table lists implementations of `IFileProvider`.</span></span>

| <span data-ttu-id="1f2c3-130">實作</span><span class="sxs-lookup"><span data-stu-id="1f2c3-130">Implementation</span></span> | <span data-ttu-id="1f2c3-131">描述</span><span class="sxs-lookup"><span data-stu-id="1f2c3-131">Description</span></span> |
| ---
<span data-ttu-id="1f2c3-132">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-132">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-133">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-133">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-134">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-134">'Identity'</span></span>
- <span data-ttu-id="1f2c3-135">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-135">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-136">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-136">'Razor'</span></span>
- <span data-ttu-id="1f2c3-137">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-137">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-138">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-138">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-139">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-139">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-140">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-140">'Identity'</span></span>
- <span data-ttu-id="1f2c3-141">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-141">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-142">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-142">'Razor'</span></span>
- <span data-ttu-id="1f2c3-143">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-143">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-144">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-144">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-145">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-145">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-146">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-146">'Identity'</span></span>
- <span data-ttu-id="1f2c3-147">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-147">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-148">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-148">'Razor'</span></span>
- <span data-ttu-id="1f2c3-149">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-149">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-150">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-150">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-151">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-151">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-152">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-152">'Identity'</span></span>
- <span data-ttu-id="1f2c3-153">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-153">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-154">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-154">'Razor'</span></span>
- <span data-ttu-id="1f2c3-155">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-155">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-156">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-156">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-157">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-157">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-158">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-158">'Identity'</span></span>
- <span data-ttu-id="1f2c3-159">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-159">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-160">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-160">'Razor'</span></span>
- <span data-ttu-id="1f2c3-161">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-161">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-162">------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-162">------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-163">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-163">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-164">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-164">'Identity'</span></span>
- <span data-ttu-id="1f2c3-165">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-165">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-166">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-166">'Razor'</span></span>
- <span data-ttu-id="1f2c3-167">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-167">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-168">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-168">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-169">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-169">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-170">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-170">'Identity'</span></span>
- <span data-ttu-id="1f2c3-171">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-171">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-172">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-172">'Razor'</span></span>
- <span data-ttu-id="1f2c3-173">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-173">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-174">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-174">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-175">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-175">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-176">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-176">'Identity'</span></span>
- <span data-ttu-id="1f2c3-177">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-177">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-178">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-178">'Razor'</span></span>
- <span data-ttu-id="1f2c3-179">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-179">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-180">------ | |[CompositeFileProvider](#compositefileprovider) |用來提供來自一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-180">------ | | [CompositeFileProvider](#compositefileprovider) | Used to provide combined access to files and directories from one or more other providers.</span></span> <span data-ttu-id="1f2c3-181">| |[ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) |用來存取內嵌于元件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-181">| | [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | Used to access files embedded in assemblies.</span></span> <span data-ttu-id="1f2c3-182">| |[PhysicalFileProvider](#physicalfileprovider) |用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-182">| | [PhysicalFileProvider](#physicalfileprovider) | Used to access the system's physical files.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="1f2c3-183">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f2c3-183">PhysicalFileProvider</span></span>

<span data-ttu-id="1f2c3-184"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-184">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="1f2c3-185">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-185">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="1f2c3-186">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-186">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="1f2c3-187">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-187">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="1f2c3-188">直接具現化此提供者時，需要絕對目錄路徑，並作為所有使用提供者所提出之要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-188">When instantiating this provider directly, an absolute directory path is required and serves as the base path for all requests made using the provider.</span></span> <span data-ttu-id="1f2c3-189">目錄路徑中不支援 Glob 模式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-189">Glob patterns aren't supported in the directory path.</span></span>

<span data-ttu-id="1f2c3-190">下列程式碼顯示如何使用 `PhysicalFileProvider` 來取得目錄內容和檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-190">The following code shows how to use `PhysicalFileProvider` to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var filePath = Path.Combine("wwwroot", "js", "site.js");
var fileInfo = provider.GetFileInfo(filePath);
```

<span data-ttu-id="1f2c3-191">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-191">Types in the preceding example:</span></span>

* <span data-ttu-id="1f2c3-192">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-192">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="1f2c3-193">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-193">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="1f2c3-194">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-194">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="1f2c3-195">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-195">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="1f2c3-196">Glob 模式無法傳遞至 `GetFileInfo` 方法。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-196">Glob patterns can't be passed to the `GetFileInfo` method.</span></span> <span data-ttu-id="1f2c3-197">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-197">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="1f2c3-198">*FileProviderSample*範例應用程式會使用，在方法中建立提供者 `Startup.ConfigureServices` <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider?displayProperty=nameWithType> ：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-198">The *FileProviderSample* sample app creates the provider in the `Startup.ConfigureServices` method using <xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider?displayProperty=nameWithType>:</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="1f2c3-199">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f2c3-199">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="1f2c3-200"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-200">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1f2c3-201">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-201">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="1f2c3-202">若要產生內嵌檔案的資訊清單：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-202">To generate a manifest of the embedded files:</span></span>

1. <span data-ttu-id="1f2c3-203">將[FileProviders](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) NuGet 套件新增至您的專案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-203">Add the [Microsoft.Extensions.FileProviders.Embedded](https://www.nuget.org/packages/Microsoft.Extensions.FileProviders.Embedded) NuGet package to your project.</span></span>
1. <span data-ttu-id="1f2c3-204">將 `<GenerateEmbeddedFilesManifest>` 屬性設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-204">Set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="1f2c3-205">指定要內嵌的檔案 [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects) ：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-205">Specify the files to embed with [\<EmbeddedResource>](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

    [!code-xml[](file-providers/samples/3.x/FileProviderSample/FileProviderSample.csproj?highlight=5,13)]

<span data-ttu-id="1f2c3-206">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-206">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1f2c3-207">*FileProviderSample*範例應用程式會建立 `ManifestEmbeddedFileProvider` ，並將目前執行的元件傳遞至其函式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-207">The *FileProviderSample* sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1f2c3-208">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-208">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="1f2c3-209">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-209">Additional overloads allow you to:</span></span>

* <span data-ttu-id="1f2c3-210">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-210">Specify a relative file path.</span></span>
* <span data-ttu-id="1f2c3-211">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-211">Scope files to a last modified date.</span></span>
* <span data-ttu-id="1f2c3-212">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-212">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="1f2c3-213">多載</span><span class="sxs-lookup"><span data-stu-id="1f2c3-213">Overload</span></span> | <span data-ttu-id="1f2c3-214">描述</span><span class="sxs-lookup"><span data-stu-id="1f2c3-214">Description</span></span> |
| ---
<span data-ttu-id="1f2c3-215">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-215">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-216">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-216">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-217">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-217">'Identity'</span></span>
- <span data-ttu-id="1f2c3-218">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-218">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-219">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-219">'Razor'</span></span>
- <span data-ttu-id="1f2c3-220">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-220">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-221">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-221">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-222">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-222">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-223">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-223">'Identity'</span></span>
- <span data-ttu-id="1f2c3-224">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-224">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-225">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-225">'Razor'</span></span>
- <span data-ttu-id="1f2c3-226">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-226">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-227">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-227">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-228">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-228">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-229">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-229">'Identity'</span></span>
- <span data-ttu-id="1f2c3-230">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-230">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-231">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-231">'Razor'</span></span>
- <span data-ttu-id="1f2c3-232">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-232">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-233">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-233">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-234">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-235">'Identity'</span></span>
- <span data-ttu-id="1f2c3-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-237">'Razor'</span></span>
- <span data-ttu-id="1f2c3-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-238">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-239">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-239">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-240">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-240">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-241">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-241">'Identity'</span></span>
- <span data-ttu-id="1f2c3-242">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-242">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-243">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-243">'Razor'</span></span>
- <span data-ttu-id="1f2c3-244">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-244">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-245">------ | |`ManifestEmbeddedFileProvider(Assembly, String)` |接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-245">------ | | `ManifestEmbeddedFileProvider(Assembly, String)` | Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="1f2c3-246">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-246">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> <span data-ttu-id="1f2c3-247">| |`ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` |接受選擇性的 `root` 相對路徑參數和 `lastModified` date （ <xref:System.DateTimeOffset> ）參數。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-247">| | `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="1f2c3-248">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-248">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="1f2c3-249">| |`ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` |接受選擇性的 `root` 相對路徑、 `lastModified` 日期和 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-249">| | `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="1f2c3-250">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-250">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="1f2c3-251">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f2c3-251">CompositeFileProvider</span></span>

<span data-ttu-id="1f2c3-252"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-252">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="1f2c3-253">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-253">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="1f2c3-254">在*FileProviderSample*範例應用程式中， `PhysicalFileProvider` 和會 `ManifestEmbeddedFileProvider` 提供檔案給在 `CompositeFileProvider` 應用程式的服務容器中註冊的。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-254">In the *FileProviderSample* sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container.</span></span> <span data-ttu-id="1f2c3-255">您可在專案的方法中找到下列程式碼 `Startup.ConfigureServices` ：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-255">The following code is found in the project's `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](file-providers/samples/3.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="1f2c3-256">監視變更</span><span class="sxs-lookup"><span data-stu-id="1f2c3-256">Watch for changes</span></span>

<span data-ttu-id="1f2c3-257"><xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*?displayProperty=nameWithType>方法會提供一個案例，讓您監看一或多個檔案或目錄的變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-257">The <xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*?displayProperty=nameWithType> method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="1f2c3-258">`Watch` 方法：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-258">The `Watch` method:</span></span>

* <span data-ttu-id="1f2c3-259">接受檔案路徑字串，其可使用[glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-259">Accepts a file path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span>
* <span data-ttu-id="1f2c3-260">傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-260">Returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span>

<span data-ttu-id="1f2c3-261">產生的變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-261">The resulting change token exposes:</span></span>

* <span data-ttu-id="1f2c3-262"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>：可以檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-262"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>: A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="1f2c3-263"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>：偵測到指定的路徑字串發生變更時呼叫。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-263"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>: Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="1f2c3-264">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-264">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="1f2c3-265">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-265">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="1f2c3-266">*WatchConsole*範例應用程式會在*TextFiles*目錄中的 *.txt*檔案修改時寫入訊息：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-266">The *WatchConsole* sample app writes a message whenever a *.txt* file in the *TextFiles* directory is modified:</span></span>

[!code-csharp[](file-providers/samples/3.x/WatchConsole/Program.cs?name=snippet1)]

<span data-ttu-id="1f2c3-267">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-267">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="1f2c3-268">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-268">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

### <a name="glob-patterns"></a><span data-ttu-id="1f2c3-269">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="1f2c3-269">Glob patterns</span></span>

<span data-ttu-id="1f2c3-270">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-270">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="1f2c3-271">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-271">Specify groups of files with these patterns.</span></span> <span data-ttu-id="1f2c3-272">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-272">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="1f2c3-273">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-273">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="1f2c3-274">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-274">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="1f2c3-275">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-275">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="1f2c3-276">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-276">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="1f2c3-277">下表提供 glob 模式的常見範例。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-277">The following table provides common examples of glob patterns.</span></span>

|<span data-ttu-id="1f2c3-278">模式</span><span class="sxs-lookup"><span data-stu-id="1f2c3-278">Pattern</span></span>  |<span data-ttu-id="1f2c3-279">描述</span><span class="sxs-lookup"><span data-stu-id="1f2c3-279">Description</span></span>  |
|---
<span data-ttu-id="1f2c3-280">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-280">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-281">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-281">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-282">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-282">'Identity'</span></span>
- <span data-ttu-id="1f2c3-283">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-283">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-284">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-284">'Razor'</span></span>
- <span data-ttu-id="1f2c3-285">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-285">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-286">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-286">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-287">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-287">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-288">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-288">'Identity'</span></span>
- <span data-ttu-id="1f2c3-289">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-289">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-290">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-290">'Razor'</span></span>
- <span data-ttu-id="1f2c3-291">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-291">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-292">-----|---
標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-292">-----|---
title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-293">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-293">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-294">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-294">'Identity'</span></span>
- <span data-ttu-id="1f2c3-295">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-295">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-296">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-296">'Razor'</span></span>
- <span data-ttu-id="1f2c3-297">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-297">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-298">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-298">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-299">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-299">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-300">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-300">'Identity'</span></span>
- <span data-ttu-id="1f2c3-301">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-301">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-302">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-302">'Razor'</span></span>
- <span data-ttu-id="1f2c3-303">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-303">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-304">-----|
|`directory/file.txt`|符合特定目錄中的特定檔案。 ||`directory/*.txt`|符合特定目錄中副檔名為 *.txt*的所有檔案。 ||`directory/*/appsettings.json`|比對目錄中的所有*appsettings*檔案與*目錄*資料夾底下的一個層級。 ||`directory/**/*.txt`|符合*目錄*資料夾下任何位置的副檔名為 *.txt*的檔案。 |</span><span class="sxs-lookup"><span data-stu-id="1f2c3-304">-----|
|`directory/file.txt`|Matches a specific file in a specific directory.| |`directory/*.txt`|Matches all files with *.txt* extension in a specific directory.| |`directory/*/appsettings.json`|Matches all *appsettings.json* files in directories exactly one level below the *directory* folder.| |`directory/**/*.txt`|Matches all files with a *.txt* extension found anywhere under the *directory* folder.|</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="1f2c3-305">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-305">ASP.NET Core abstracts file system access through the use of File Providers.</span></span> <span data-ttu-id="1f2c3-306">「檔案提供者」在整個 ASP.NET Core 架構中使用：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-306">File Providers are used throughout the ASP.NET Core framework:</span></span>

* <span data-ttu-id="1f2c3-307"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment>將應用程式的[內容根目錄](xref:fundamentals/index#content-root)和[web 根目錄](xref:fundamentals/index#web-root)公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-307"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> exposes the app's [content root](xref:fundamentals/index#content-root) and [web root](xref:fundamentals/index#web-root) as `IFileProvider` types.</span></span>
* <span data-ttu-id="1f2c3-308">[靜態檔案中介軟體](xref:fundamentals/static-files)使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-308">[Static File Middleware](xref:fundamentals/static-files) uses File Providers to locate static files.</span></span>
* <span data-ttu-id="1f2c3-309">[Razor](xref:mvc/views/razor)會使用檔案提供者來尋找頁面和瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-309">[Razor](xref:mvc/views/razor) uses File Providers to locate pages and views.</span></span>
* <span data-ttu-id="1f2c3-310">.NET Core 工具使用「檔案提供者」與 Glob 模式來指定應該要發佈哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-310">.NET Core tooling uses File Providers and glob patterns to specify which files should be published.</span></span>

<span data-ttu-id="1f2c3-311">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="1f2c3-311">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/file-providers/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="file-provider-interfaces"></a><span data-ttu-id="1f2c3-312">檔案提供者介面</span><span class="sxs-lookup"><span data-stu-id="1f2c3-312">File Provider interfaces</span></span>

<span data-ttu-id="1f2c3-313">主要介面是 <xref:Microsoft.Extensions.FileProviders.IFileProvider>。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-313">The primary interface is <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="1f2c3-314">`IFileProvider` 公開方法以：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-314">`IFileProvider` exposes methods to:</span></span>

* <span data-ttu-id="1f2c3-315">取得檔案資訊 (<xref:Microsoft.Extensions.FileProviders.IFileInfo>)。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-315">Obtain file information (<xref:Microsoft.Extensions.FileProviders.IFileInfo>).</span></span>
* <span data-ttu-id="1f2c3-316">取得目錄資訊 (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>)。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-316">Obtain directory information (<xref:Microsoft.Extensions.FileProviders.IDirectoryContents>).</span></span>
* <span data-ttu-id="1f2c3-317">設定變更通知 (使用 <xref:Microsoft.Extensions.Primitives.IChangeToken>)。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-317">Set up change notifications (using an <xref:Microsoft.Extensions.Primitives.IChangeToken>).</span></span>

<span data-ttu-id="1f2c3-318">`IFileInfo` 提供可用來使用檔案的方法與屬性：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-318">`IFileInfo` provides methods and properties for working with files:</span></span>

* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Exists>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.IsDirectory>
* <xref:Microsoft.Extensions.FileProviders.IFileInfo.Name>
* <span data-ttu-id="1f2c3-319"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (以位元組為單位)</span><span class="sxs-lookup"><span data-stu-id="1f2c3-319"><xref:Microsoft.Extensions.FileProviders.IFileInfo.Length> (in bytes)</span></span>
* <span data-ttu-id="1f2c3-320"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> 日期</span><span class="sxs-lookup"><span data-stu-id="1f2c3-320"><xref:Microsoft.Extensions.FileProviders.IFileInfo.LastModified> date</span></span>

<span data-ttu-id="1f2c3-321">您可以使用 [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) 方法來讀取該檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-321">You can read from the file using the [IFileInfo.CreateReadStream](xref:Microsoft.Extensions.FileProviders.IFileInfo.CreateReadStream*) method.</span></span>

<span data-ttu-id="1f2c3-322">範例應用程式示範如何在 `Startup.ConfigureServices` 中設定「檔案提供者」，以透過 [dependency injection](xref:fundamentals/dependency-injection) 在整個應用程式中使用。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-322">The sample app demonstrates how to configure a File Provider in `Startup.ConfigureServices` for use throughout the app via [dependency injection](xref:fundamentals/dependency-injection).</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="1f2c3-323">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="1f2c3-323">File Provider implementations</span></span>

<span data-ttu-id="1f2c3-324">我們提供三個 `IFileProvider` 的實作。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-324">Three implementations of `IFileProvider` are available.</span></span>

| <span data-ttu-id="1f2c3-325">實作</span><span class="sxs-lookup"><span data-stu-id="1f2c3-325">Implementation</span></span> | <span data-ttu-id="1f2c3-326">描述</span><span class="sxs-lookup"><span data-stu-id="1f2c3-326">Description</span></span> |
| ---
<span data-ttu-id="1f2c3-327">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-327">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-328">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-328">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-329">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-329">'Identity'</span></span>
- <span data-ttu-id="1f2c3-330">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-330">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-331">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-331">'Razor'</span></span>
- <span data-ttu-id="1f2c3-332">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-332">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-333">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-333">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-334">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-334">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-335">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-335">'Identity'</span></span>
- <span data-ttu-id="1f2c3-336">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-336">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-337">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-337">'Razor'</span></span>
- <span data-ttu-id="1f2c3-338">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-338">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-339">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-339">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-340">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-341">'Identity'</span></span>
- <span data-ttu-id="1f2c3-342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-342">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-343">'Razor'</span></span>
- <span data-ttu-id="1f2c3-344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-345">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-345">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-346">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-347">'Identity'</span></span>
- <span data-ttu-id="1f2c3-348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-348">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-349">'Razor'</span></span>
- <span data-ttu-id="1f2c3-350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-351">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-351">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-352">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-353">'Identity'</span></span>
- <span data-ttu-id="1f2c3-354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-354">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-355">'Razor'</span></span>
- <span data-ttu-id="1f2c3-356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-356">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-357">------- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-357">------- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-358">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-359">'Identity'</span></span>
- <span data-ttu-id="1f2c3-360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-360">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-361">'Razor'</span></span>
- <span data-ttu-id="1f2c3-362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-362">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-363">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-363">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-364">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-364">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-365">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-365">'Identity'</span></span>
- <span data-ttu-id="1f2c3-366">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-366">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-367">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-367">'Razor'</span></span>
- <span data-ttu-id="1f2c3-368">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-368">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-369">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-369">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-370">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-370">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-371">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-371">'Identity'</span></span>
- <span data-ttu-id="1f2c3-372">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-372">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-373">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-373">'Razor'</span></span>
- <span data-ttu-id="1f2c3-374">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-374">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-375">------ | |[PhysicalFileProvider](#physicalfileprovider) |實體提供者用來存取系統的實體檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-375">------ | | [PhysicalFileProvider](#physicalfileprovider) | The physical provider is used to access the system's physical files.</span></span> <span data-ttu-id="1f2c3-376">| |[ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) |資訊清單內嵌提供者用來存取內嵌于元件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-376">| | [ManifestEmbeddedFileProvider](#manifestembeddedfileprovider) | The manifest embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="1f2c3-377">| |[CompositeFileProvider](#compositefileprovider) |複合提供者是用來提供來自一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-377">| | [CompositeFileProvider](#compositefileprovider) | The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span> |

### <a name="physicalfileprovider"></a><span data-ttu-id="1f2c3-378">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f2c3-378">PhysicalFileProvider</span></span>

<span data-ttu-id="1f2c3-379"><xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-379">The <xref:Microsoft.Extensions.FileProviders.PhysicalFileProvider> provides access to the physical file system.</span></span> <span data-ttu-id="1f2c3-380">`PhysicalFileProvider` 會使用 <xref:System.IO.File?displayProperty=fullName> 類型 (針對實體提供者) 並將所有路徑的範圍限定為某個目錄與其子系。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-380">`PhysicalFileProvider` uses the <xref:System.IO.File?displayProperty=fullName> type (for the physical provider) and scopes all paths to a directory and its children.</span></span> <span data-ttu-id="1f2c3-381">此範圍限定動作可防止存取所指定目錄與其子系以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-381">This scoping prevents access to the file system outside of the specified directory and its children.</span></span> <span data-ttu-id="1f2c3-382">建立並使用 `PhysicalFileProvider` 的最常見情節是透過[相依性插入](xref:fundamentals/dependency-injection)在函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-382">The most common scenario for creating and using a `PhysicalFileProvider` is to request an `IFileProvider` in a constructor through [dependency injection](xref:fundamentals/dependency-injection).</span></span>

<span data-ttu-id="1f2c3-383">當直接具現化此提供者時，會需要一個目錄路徑，而且此目錄路徑會做為使用該提供者發出之所有要求的基底路徑。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-383">When instantiating this provider directly, a directory path is required and serves as the base path for all requests made using the provider.</span></span>

<span data-ttu-id="1f2c3-384">下列程式碼顯示如何建立 `PhysicalFileProvider` 並使用它來取得目錄內容與檔案資訊：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-384">The following code shows how to create a `PhysicalFileProvider` and use it to obtain directory contents and file information:</span></span>

```csharp
var provider = new PhysicalFileProvider(applicationRoot);
var contents = provider.GetDirectoryContents(string.Empty);
var fileInfo = provider.GetFileInfo("wwwroot/js/site.js");
```

<span data-ttu-id="1f2c3-385">上述範例中的型別：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-385">Types in the preceding example:</span></span>

* <span data-ttu-id="1f2c3-386">`provider` 是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-386">`provider` is an `IFileProvider`.</span></span>
* <span data-ttu-id="1f2c3-387">`contents` 是 `IDirectoryContents`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-387">`contents` is an `IDirectoryContents`.</span></span>
* <span data-ttu-id="1f2c3-388">`fileInfo` 是 `IFileInfo`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-388">`fileInfo` is an `IFileInfo`.</span></span>

<span data-ttu-id="1f2c3-389">「檔案提供者」可用來逐一查看由 `applicationRoot` o所指定的目錄或呼叫 `GetFileInfo` 以取得檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-389">The File Provider can be used to iterate through the directory specified by `applicationRoot` or call `GetFileInfo` to obtain a file's information.</span></span> <span data-ttu-id="1f2c3-390">「檔案提供者」沒有 `applicationRoot` 外部之項目的存取權。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-390">The File Provider has no access outside of the `applicationRoot` directory.</span></span>

<span data-ttu-id="1f2c3-391">範例應用程式會使用 [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider)在應用程式的 `Startup.ConfigureServices` 類別中建立該提供者：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-391">The sample app creates the provider in the app's `Startup.ConfigureServices` class using [IHostingEnvironment.ContentRootFileProvider](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ContentRootFileProvider):</span></span>

```csharp
var physicalProvider = _env.ContentRootFileProvider;
```

### <a name="manifestembeddedfileprovider"></a><span data-ttu-id="1f2c3-392">ManifestEmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f2c3-392">ManifestEmbeddedFileProvider</span></span>

<span data-ttu-id="1f2c3-393"><xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> 用來存取內嵌於組件內的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-393">The <xref:Microsoft.Extensions.FileProviders.ManifestEmbeddedFileProvider> is used to access files embedded within assemblies.</span></span> <span data-ttu-id="1f2c3-394">`ManifestEmbeddedFileProvider` 使用已編譯到組件中的資訊清單來重新建構內嵌檔案的原始路徑。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-394">The `ManifestEmbeddedFileProvider` uses a manifest compiled into the assembly to reconstruct the original paths of the embedded files.</span></span>

<span data-ttu-id="1f2c3-395">若要產生內嵌檔案的資訊清單，請將 `<GenerateEmbeddedFilesManifest>` 屬性設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-395">To generate a manifest of the embedded files, set the `<GenerateEmbeddedFilesManifest>` property to `true`.</span></span> <span data-ttu-id="1f2c3-396">指定要與[ &lt; EmbeddedResource &gt; ](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects)內嵌的檔案：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-396">Specify the files to embed with [&lt;EmbeddedResource&gt;](/dotnet/core/tools/csproj#default-compilation-includes-in-net-core-projects):</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/FileProviderSample.csproj?highlight=6,14)]

<span data-ttu-id="1f2c3-397">使用 [Glob 模式](#glob-patterns)來指定一或多個要內嵌到組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-397">Use [glob patterns](#glob-patterns) to specify one or more files to embed into the assembly.</span></span>

<span data-ttu-id="1f2c3-398">範例應用程式會建立 `ManifestEmbeddedFileProvider` 並將目前執行中組件傳遞到其建構函式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-398">The sample app creates an `ManifestEmbeddedFileProvider` and passes the currently executing assembly to its constructor.</span></span>

<span data-ttu-id="1f2c3-399">*Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-399">*Startup.cs*:</span></span>

```csharp
var manifestEmbeddedProvider = 
    new ManifestEmbeddedFileProvider(typeof(Program).Assembly);
```

<span data-ttu-id="1f2c3-400">額外的多載可讓您：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-400">Additional overloads allow you to:</span></span>

* <span data-ttu-id="1f2c3-401">指定相對檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-401">Specify a relative file path.</span></span>
* <span data-ttu-id="1f2c3-402">將檔案限定為上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-402">Scope files to a last modified date.</span></span>
* <span data-ttu-id="1f2c3-403">為包內嵌檔案資訊清單的內嵌資源命名。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-403">Name the embedded resource containing the embedded file manifest.</span></span>

| <span data-ttu-id="1f2c3-404">多載</span><span class="sxs-lookup"><span data-stu-id="1f2c3-404">Overload</span></span> | <span data-ttu-id="1f2c3-405">描述</span><span class="sxs-lookup"><span data-stu-id="1f2c3-405">Description</span></span> |
| ---
<span data-ttu-id="1f2c3-406">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-406">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-407">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-407">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-408">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-408">'Identity'</span></span>
- <span data-ttu-id="1f2c3-409">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-409">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-410">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-410">'Razor'</span></span>
- <span data-ttu-id="1f2c3-411">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-411">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-412">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-412">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-413">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-413">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-414">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-414">'Identity'</span></span>
- <span data-ttu-id="1f2c3-415">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-415">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-416">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-416">'Razor'</span></span>
- <span data-ttu-id="1f2c3-417">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-417">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-418">---- |---標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-418">---- | --- title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-419">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-419">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-420">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-420">'Identity'</span></span>
- <span data-ttu-id="1f2c3-421">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-421">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-422">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-422">'Razor'</span></span>
- <span data-ttu-id="1f2c3-423">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-423">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-424">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-424">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-425">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-425">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-426">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-426">'Identity'</span></span>
- <span data-ttu-id="1f2c3-427">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-427">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-428">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-428">'Razor'</span></span>
- <span data-ttu-id="1f2c3-429">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-429">'SignalR' uid:</span></span> 

-
<span data-ttu-id="1f2c3-430">標題： author： description： monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-430">title: author: description: monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="1f2c3-431">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-431">'Blazor'</span></span>
- <span data-ttu-id="1f2c3-432">'Identity'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-432">'Identity'</span></span>
- <span data-ttu-id="1f2c3-433">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-433">'Let's Encrypt'</span></span>
- <span data-ttu-id="1f2c3-434">'Razor'</span><span class="sxs-lookup"><span data-stu-id="1f2c3-434">'Razor'</span></span>
- <span data-ttu-id="1f2c3-435">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-435">'SignalR' uid:</span></span> 

<span data-ttu-id="1f2c3-436">------ | |`ManifestEmbeddedFileProvider(Assembly, String)` |接受選擇性的 `root` 相對路徑參數。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-436">------ | | `ManifestEmbeddedFileProvider(Assembly, String)` | Accepts an optional `root` relative path parameter.</span></span> <span data-ttu-id="1f2c3-437">指定 `root` 以將對 <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> 的呼叫限定為所提供路徑下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-437">Specify the `root` to scope calls to <xref:Microsoft.Extensions.FileProviders.IFileProvider.GetDirectoryContents*> to those resources under the provided path.</span></span> <span data-ttu-id="1f2c3-438">| |`ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` |接受選擇性的 `root` 相對路徑參數和 `lastModified` date （ <xref:System.DateTimeOffset> ）參數。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-438">| | `ManifestEmbeddedFileProvider(Assembly, String, DateTimeOffset)` | Accepts an optional `root` relative path parameter and a `lastModified` date (<xref:System.DateTimeOffset>) parameter.</span></span> <span data-ttu-id="1f2c3-439">`lastModified` 日期會限定為 <xref:Microsoft.Extensions.FileProviders.IFileInfo> 執行個體 (由 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 所傳回) 的上次修改日期。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-439">The `lastModified` date scopes the last modification date for the <xref:Microsoft.Extensions.FileProviders.IFileInfo> instances returned by the <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span></span> <span data-ttu-id="1f2c3-440">| |`ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` |接受選擇性的 `root` 相對路徑、 `lastModified` 日期和 `manifestName` 參數。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-440">| | `ManifestEmbeddedFileProvider(Assembly, String, String, DateTimeOffset)` | Accepts an optional `root` relative path, `lastModified` date, and `manifestName` parameters.</span></span> <span data-ttu-id="1f2c3-441">`manifestName` 代表包含資訊清單之內嵌資源的名稱。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-441">The `manifestName` represents the name of the embedded resource containing the manifest.</span></span> |

### <a name="compositefileprovider"></a><span data-ttu-id="1f2c3-442">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="1f2c3-442">CompositeFileProvider</span></span>

<span data-ttu-id="1f2c3-443"><xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-443">The <xref:Microsoft.Extensions.FileProviders.CompositeFileProvider> combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="1f2c3-444">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-444">When creating the `CompositeFileProvider`, pass one or more `IFileProvider` instances to its constructor.</span></span>

<span data-ttu-id="1f2c3-445">在範例應用程式中，`PhysicalFileProvider` 與 `ManifestEmbeddedFileProvider` 提供檔案給在應用程式的服務容器中註冊的 `CompositeFileProvider`：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-445">In the sample app, a `PhysicalFileProvider` and a `ManifestEmbeddedFileProvider` provide files to a `CompositeFileProvider` registered in the app's service container:</span></span>

[!code-csharp[](file-providers/samples/2.x/FileProviderSample/Startup.cs?name=snippet1)]

## <a name="watch-for-changes"></a><span data-ttu-id="1f2c3-446">監視變更</span><span class="sxs-lookup"><span data-stu-id="1f2c3-446">Watch for changes</span></span>

<span data-ttu-id="1f2c3-447">[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 方法提供一種情節，用來監視一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-447">The [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) method provides a scenario to watch one or more files or directories for changes.</span></span> <span data-ttu-id="1f2c3-448">`Watch` 接受路徑字串，該字串可以使用 [Glob 模式](#glob-patterns)來指定多個檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-448">`Watch` accepts a path string, which can use [glob patterns](#glob-patterns) to specify multiple files.</span></span> <span data-ttu-id="1f2c3-449">`Watch` 會傳回 <xref:Microsoft.Extensions.Primitives.IChangeToken>。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-449">`Watch` returns an <xref:Microsoft.Extensions.Primitives.IChangeToken>.</span></span> <span data-ttu-id="1f2c3-450">變更權杖會公開：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-450">The change token exposes:</span></span>

* <span data-ttu-id="1f2c3-451"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>：可以檢查以判斷是否發生變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-451"><xref:Microsoft.Extensions.Primitives.IChangeToken.HasChanged>: A property that can be inspected to determine if a change has occurred.</span></span>
* <span data-ttu-id="1f2c3-452"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>：偵測到指定的路徑字串發生變更時呼叫。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-452"><xref:Microsoft.Extensions.Primitives.IChangeToken.RegisterChangeCallback*>: Called when changes are detected to the specified path string.</span></span> <span data-ttu-id="1f2c3-453">每個變更權杖都只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-453">Each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="1f2c3-454">若要啟用持續監視，請使用 <xref:System.Threading.Tasks.TaskCompletionSource`1> (如下所示) 或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-454">To enable constant monitoring, use a <xref:System.Threading.Tasks.TaskCompletionSource`1> (shown below) or recreate `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="1f2c3-455">在範例應用程式中，*WatchConsole* 主控台應用程式是設定為在文字檔案被修改時顯示訊息：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-455">In the sample app, the *WatchConsole* console app is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/samples/2.x/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="1f2c3-456">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-456">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="1f2c3-457">將 `DOTNET_USE_POLLING_FILE_WATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒 (無法設定) 輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-457">Set the `DOTNET_USE_POLLING_FILE_WATCHER` environment variable to `1` or `true` to poll the file system for changes every four seconds (not configurable).</span></span>

## <a name="glob-patterns"></a><span data-ttu-id="1f2c3-458">Glob 模式</span><span class="sxs-lookup"><span data-stu-id="1f2c3-458">Glob patterns</span></span>

<span data-ttu-id="1f2c3-459">檔案系統路徑使用稱為 *Glob (或 Glob 處理) 模式*的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-459">File system paths use wildcard patterns called *glob (or globbing) patterns*.</span></span> <span data-ttu-id="1f2c3-460">使用這些模式指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-460">Specify groups of files with these patterns.</span></span> <span data-ttu-id="1f2c3-461">兩種萬用字元為 `*` 與 `**`：</span><span class="sxs-lookup"><span data-stu-id="1f2c3-461">The two wildcard characters are `*` and `**`:</span></span>

**`*`**  
<span data-ttu-id="1f2c3-462">符合目前資料夾層級、任何檔案名稱或任何副檔名的任何項目。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-462">Matches anything at the current folder level, any filename, or any file extension.</span></span> <span data-ttu-id="1f2c3-463">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-463">Matches are terminated by `/` and `.` characters in the file path.</span></span>

**`**`**  
<span data-ttu-id="1f2c3-464">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-464">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="1f2c3-465">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-465">Can be used to recursively match many files within a directory hierarchy.</span></span>

<span data-ttu-id="1f2c3-466">**Glob 模式範例**</span><span class="sxs-lookup"><span data-stu-id="1f2c3-466">**Glob pattern examples**</span></span>

**`directory/file.txt`**  
<span data-ttu-id="1f2c3-467">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-467">Matches a specific file in a specific directory.</span></span>

**`directory/*.txt`**  
<span data-ttu-id="1f2c3-468">符合特定目錄中具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-468">Matches all files with *.txt* extension in a specific directory.</span></span>

**`directory/*/appsettings.json`**  
<span data-ttu-id="1f2c3-469">符合「目錄」\*\* 資料夾下一層級之目錄中的所有 `appsettings.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-469">Matches all `appsettings.json` files in directories exactly one level below the *directory* folder.</span></span>

**`directory/**/*.txt`**  
<span data-ttu-id="1f2c3-470">符合在「目錄」\*\* 資料夾下之任何地方所找到的具有 *.txt* 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="1f2c3-470">Matches all files with *.txt* extension found anywhere under the *directory* folder.</span></span>

::: moniker-end
