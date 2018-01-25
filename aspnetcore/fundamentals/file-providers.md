---
title: "在 ASP.NET Core 檔案提供者"
author: ardalis
description: "了解 ASP.NET Core 抽象化透過檔案提供者使用的檔案系統存取的方式。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/file-providers
ms.openlocfilehash: 10f3276d3e71e8a29b452d4c62865cbb82298513
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="b39c8-103">在 ASP.NET Core 檔案提供者</span><span class="sxs-lookup"><span data-stu-id="b39c8-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="b39c8-104">由[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="b39c8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="b39c8-105">ASP.NET Core 抽象化透過檔案提供者使用的檔案系統存取權。</span><span class="sxs-lookup"><span data-stu-id="b39c8-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="b39c8-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b39c8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="b39c8-107">檔案提供者的抽象概念</span><span class="sxs-lookup"><span data-stu-id="b39c8-107">File Provider abstractions</span></span>

<span data-ttu-id="b39c8-108">檔案提供者是透過檔案系統的抽象概念。</span><span class="sxs-lookup"><span data-stu-id="b39c8-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="b39c8-109">主要介面為`IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="b39c8-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="b39c8-110">`IFileProvider`公開方法，以取得檔案資訊 (`IFileInfo`)，目錄資訊 (`IDirectoryContents`)，並設定變更通知 (使用`IChangeToken`)。</span><span class="sxs-lookup"><span data-stu-id="b39c8-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="b39c8-111">`IFileInfo`提供方法和相關的個別檔案或目錄的屬性。</span><span class="sxs-lookup"><span data-stu-id="b39c8-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="b39c8-112">它有兩個布林值屬性，`Exists`和`IsDirectory`，描述之檔案的內容以及`Name`， `Length` （以位元組為單位），和`LastModified`日期。</span><span class="sxs-lookup"><span data-stu-id="b39c8-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="b39c8-113">您可以從檔案使用讀取其`CreateReadStream`方法。</span><span class="sxs-lookup"><span data-stu-id="b39c8-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="b39c8-114">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="b39c8-114">File Provider implementations</span></span>

<span data-ttu-id="b39c8-115">三種實作方式`IFileProvider`可用： 實體、 內嵌，以及複合。</span><span class="sxs-lookup"><span data-stu-id="b39c8-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="b39c8-116">實體的提供者用來存取實際系統的檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="b39c8-117">內嵌的提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="b39c8-118">複合的提供者用來提供檔案和目錄從一或多個其他提供者的合併的存取。</span><span class="sxs-lookup"><span data-stu-id="b39c8-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="b39c8-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="b39c8-119">PhysicalFileProvider</span></span>

<span data-ttu-id="b39c8-120">`PhysicalFileProvider`提供實體檔案系統的存取權。</span><span class="sxs-lookup"><span data-stu-id="b39c8-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="b39c8-121">它所包裝`System.IO.File`型別 （適用於實體提供者），範圍設定的目錄和其子系的所有路徑。</span><span class="sxs-lookup"><span data-stu-id="b39c8-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="b39c8-122">此範圍會限制存取特定目錄和子系，導致無法存取此界限以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="b39c8-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="b39c8-123">當具現化此提供者，您必須將它提供一個目錄路徑，會作為此提供者進行的所有要求的基底路徑 （和以限制存取此路徑之外）。</span><span class="sxs-lookup"><span data-stu-id="b39c8-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="b39c8-124">在 ASP.NET Core 應用程式，您可以具現化`PhysicalFileProvider`直接管理，提供者，或者您可以要求`IFileProvider`中的控制站或服務的建構函式透過[相依性插入](dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="b39c8-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="b39c8-125">第二種方法通常會導致更有彈性且可測試的方案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="b39c8-126">下列範例示範如何建立`PhysicalFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="b39c8-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="b39c8-127">您可以逐一查看目錄內容，或提供子路徑，以取得特定的檔案資訊。</span><span class="sxs-lookup"><span data-stu-id="b39c8-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="b39c8-128">若要要求提供者從控制器，指定控制器的建構函式中，並將它指派給本機欄位。</span><span class="sxs-lookup"><span data-stu-id="b39c8-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="b39c8-129">使用本機的執行個體，在動作方法：</span><span class="sxs-lookup"><span data-stu-id="b39c8-129">Use the local instance from your action methods:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="b39c8-130">接著，建立的應用程式中的 提供者`Startup`類別：</span><span class="sxs-lookup"><span data-stu-id="b39c8-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="b39c8-131">在*Index.cshtml*檢視中，逐一查看`IDirectoryContents`提供：</span><span class="sxs-lookup"><span data-stu-id="b39c8-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[Main](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="b39c8-132">結果：</span><span class="sxs-lookup"><span data-stu-id="b39c8-132">The result:</span></span>

![提供者範例應用程式清單實體檔案和資料夾的檔案](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="b39c8-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="b39c8-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="b39c8-135">`EmbeddedFileProvider`用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="b39c8-136">在.NET Core 您內嵌的組件的檔案`<EmbeddedResource>`中的項目*.csproj*檔案：</span><span class="sxs-lookup"><span data-stu-id="b39c8-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[Main](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="b39c8-137">您可以使用[通用慣例模式](#globbing-patterns)時指定要內嵌在組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="b39c8-138">這些模式可用來比對一個或多個檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="b39c8-139">不太可能想要實際內嵌在組件中; 在專案中的每一個.js 檔案上述範例只會用於示範用途。</span><span class="sxs-lookup"><span data-stu-id="b39c8-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="b39c8-140">建立時`EmbeddedFileProvider`，傳遞至其建構函式，它會讀取組件。</span><span class="sxs-lookup"><span data-stu-id="b39c8-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="b39c8-141">上述程式碼片段示範如何建立`EmbeddedFileProvider`存取目前執行的組件。</span><span class="sxs-lookup"><span data-stu-id="b39c8-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="b39c8-142">更新範例應用程式使用`EmbeddedFileProvider`會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="b39c8-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![列出內嵌的檔案的檔案提供者範例應用程式](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="b39c8-144">內嵌的資源不會公開目錄。</span><span class="sxs-lookup"><span data-stu-id="b39c8-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="b39c8-145">相反地，在它的檔名使用內嵌資源 （透過其命名空間） 的路徑`.`分隔符號。</span><span class="sxs-lookup"><span data-stu-id="b39c8-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="b39c8-146">`EmbeddedFileProvider`建構函式可接受選擇`baseNamespace`參數。</span><span class="sxs-lookup"><span data-stu-id="b39c8-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="b39c8-147">指定這個會範圍呼叫`GetDirectoryContents`提供的命名空間底下的這些資源。</span><span class="sxs-lookup"><span data-stu-id="b39c8-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="b39c8-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="b39c8-148">CompositeFileProvider</span></span>

<span data-ttu-id="b39c8-149">`CompositeFileProvider`結合`IFileProvider`執行個體，公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="b39c8-150">建立時`CompositeFileProvider`，您將一或多個傳遞`IFileProvider`其建構函式的執行個體：</span><span class="sxs-lookup"><span data-stu-id="b39c8-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[Main](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="b39c8-151">更新範例應用程式使用`CompositeFileProvider`包含這兩個實體和內嵌的提供者之前設定，會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="b39c8-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![列出實體檔案和資料夾以及內嵌的檔案的檔案提供者範例應用程式](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="b39c8-153">監看的變更</span><span class="sxs-lookup"><span data-stu-id="b39c8-153">Watching for changes</span></span>

<span data-ttu-id="b39c8-154">`IFileProvider` `Watch`方法可用來監看一或多個檔案或目錄的變更。</span><span class="sxs-lookup"><span data-stu-id="b39c8-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="b39c8-155">這個方法會接受路徑字串，可以使用[通用慣例模式](#globbing-patterns)來指定多個檔案，並傳回`IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="b39c8-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="b39c8-156">這個語彙基元會公開`HasChanged`屬性可檢查，和`RegisterChangeCallback`偵測到指定的路徑字串的變更時呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="b39c8-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="b39c8-157">請注意，每個變更權杖只會呼叫其關聯的回呼回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="b39c8-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="b39c8-158">若要啟用持續監控，您可以使用`TaskCompletionSource`如下所示，或重新建立`IChangeToken`執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="b39c8-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="b39c8-159">在本文的範例主控台應用程式會設定為每當修改文字檔案，顯示一則訊息：</span><span class="sxs-lookup"><span data-stu-id="b39c8-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[Main](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="b39c8-160">儲存檔案數次之後結果：</span><span class="sxs-lookup"><span data-stu-id="b39c8-160">The result, after saving the file several times:</span></span>

![之後執行 dotnet 執行顯示監視 quotes.txt 檔案變更的應用程式和檔案已變更五次的命令視窗。](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="b39c8-162">有些檔案系統，例如 Docker 容器和網路共用，可能不可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="b39c8-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="b39c8-163">設定`DOTNET_USE_POLLINGFILEWATCHER`環境變數，以`1`或`true`輪詢變更的檔案系統每 4 秒。</span><span class="sxs-lookup"><span data-stu-id="b39c8-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="b39c8-164">通用慣例模式</span><span class="sxs-lookup"><span data-stu-id="b39c8-164">Globbing patterns</span></span>

<span data-ttu-id="b39c8-165">檔案系統路徑中使用萬用字元模式比對呼叫*通用慣例模式*。</span><span class="sxs-lookup"><span data-stu-id="b39c8-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="b39c8-166">這些簡單的模式可以用來指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="b39c8-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="b39c8-167">兩個萬用字元只有`*`和`**`。</span><span class="sxs-lookup"><span data-stu-id="b39c8-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="b39c8-168">符合任何項目在目前的資料夾層級，或任何檔名或任何副檔名。</span><span class="sxs-lookup"><span data-stu-id="b39c8-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="b39c8-169">相符項目會終止`/`和`.`檔案路徑中的字元。</span><span class="sxs-lookup"><span data-stu-id="b39c8-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="b39c8-170">符合任何項目的跨多個目錄層級。</span><span class="sxs-lookup"><span data-stu-id="b39c8-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="b39c8-171">可用以遞迴方式來比對目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="b39c8-172">通用慣例模式範例</span><span class="sxs-lookup"><span data-stu-id="b39c8-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="b39c8-173">比對特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="b39c8-174">比對所有檔案與`.txt`特定目錄中的擴充功能。</span><span class="sxs-lookup"><span data-stu-id="b39c8-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="b39c8-175">比對所有`bower.json`目錄剛好只有一個層級中的檔案`directory`目錄。</span><span class="sxs-lookup"><span data-stu-id="b39c8-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="b39c8-176">比對所有檔案與`.txt`延伸模組會在下任何地方找到`directory`目錄。</span><span class="sxs-lookup"><span data-stu-id="b39c8-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="b39c8-177">在 ASP.NET Core 檔案提供者使用量</span><span class="sxs-lookup"><span data-stu-id="b39c8-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="b39c8-178">ASP.NET Core 的幾個部分會利用檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="b39c8-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="b39c8-179">`IHostingEnvironment`公開應用程式的根內容與 web 根目錄為`IFileProvider`型別。</span><span class="sxs-lookup"><span data-stu-id="b39c8-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="b39c8-180">靜態檔案中介軟體會使用檔案提供者尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="b39c8-181">Razor 讓大量使用`IFileProvider`中尋找檢視。</span><span class="sxs-lookup"><span data-stu-id="b39c8-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="b39c8-182">Dotnet 的發佈功能會使用檔案提供者和通用慣例模式以指定要發行哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="b39c8-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="b39c8-183">建議以供在應用程式中使用</span><span class="sxs-lookup"><span data-stu-id="b39c8-183">Recommendations for use in apps</span></span>

<span data-ttu-id="b39c8-184">如果您的 ASP.NET Core 應用程式需要檔案系統存取權，您可以要求的執行個體`IFileProvider`透過相依性插入，然後使用其方法來執行存取權，此範例中所示。</span><span class="sxs-lookup"><span data-stu-id="b39c8-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="b39c8-185">這可讓您一次，當應用程式啟動，而且可以減少您的應用程式具現化的實作類型的設定提供者。</span><span class="sxs-lookup"><span data-stu-id="b39c8-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
