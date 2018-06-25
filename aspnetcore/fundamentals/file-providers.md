---
title: ASP.NET Core 中的檔案提供者
author: ardalis
description: 了解 ASP.NET Core 如何透過使用檔案提供者，將檔案系統存取抽象化。
ms.author: riande
ms.date: 02/14/2017
uid: fundamentals/file-providers
ms.openlocfilehash: 0d356322ea9f4cc2caead81746bf9ede4a87923f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276234"
---
# <a name="file-providers-in-aspnet-core"></a><span data-ttu-id="138e8-103">ASP.NET Core 中的檔案提供者</span><span class="sxs-lookup"><span data-stu-id="138e8-103">File Providers in ASP.NET Core</span></span>

<span data-ttu-id="138e8-104">作者：[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="138e8-104">By [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="138e8-105">ASP.NET Core 透過使用檔案提供者，將檔案系統存取抽象化。</span><span class="sxs-lookup"><span data-stu-id="138e8-105">ASP.NET Core abstracts file system access through the use of File Providers.</span></span>

<span data-ttu-id="138e8-106">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="138e8-106">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/file-providers/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="file-provider-abstractions"></a><span data-ttu-id="138e8-107">檔案提供者抽象概念</span><span class="sxs-lookup"><span data-stu-id="138e8-107">File Provider abstractions</span></span>

<span data-ttu-id="138e8-108">檔案提供者是對檔案系統的抽象。</span><span class="sxs-lookup"><span data-stu-id="138e8-108">File Providers are an abstraction over file systems.</span></span> <span data-ttu-id="138e8-109">主要介面是 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="138e8-109">The main interface is `IFileProvider`.</span></span> <span data-ttu-id="138e8-110">`IFileProvider` 公開了一些方法，用來取得檔案資訊 (`IFileInfo`)、目錄資訊 (`IDirectoryContents`)，以及設定變更通知 (使用 `IChangeToken`)。</span><span class="sxs-lookup"><span data-stu-id="138e8-110">`IFileProvider` exposes methods to get file information (`IFileInfo`), directory information (`IDirectoryContents`), and to set up change notifications (using an `IChangeToken`).</span></span>

<span data-ttu-id="138e8-111">`IFileInfo` 提供與個別檔案或目錄有關的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="138e8-111">`IFileInfo` provides methods and properties about individual files or directories.</span></span> <span data-ttu-id="138e8-112">它有兩個布林值屬性 (`Exists` 和 `IsDirectory`)，以及描述檔案的`Name`、`Length` (以位元組為單位) 和 `LastModified` 日期的屬性。</span><span class="sxs-lookup"><span data-stu-id="138e8-112">It has two boolean properties, `Exists` and `IsDirectory`, as well as properties describing the file's `Name`, `Length` (in bytes), and `LastModified` date.</span></span> <span data-ttu-id="138e8-113">您可以使用其 `CreateReadStream` 方法從檔案讀取。</span><span class="sxs-lookup"><span data-stu-id="138e8-113">You can read from the file using its `CreateReadStream` method.</span></span>

## <a name="file-provider-implementations"></a><span data-ttu-id="138e8-114">檔案提供者實作</span><span class="sxs-lookup"><span data-stu-id="138e8-114">File Provider implementations</span></span>

<span data-ttu-id="138e8-115">提供三種 `IFileProvider` 實作：實體、內嵌和複合。</span><span class="sxs-lookup"><span data-stu-id="138e8-115">Three implementations of `IFileProvider` are available: Physical, Embedded, and Composite.</span></span> <span data-ttu-id="138e8-116">實體提供者用來存取實際系統的檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-116">The physical provider is used to access the actual system's files.</span></span> <span data-ttu-id="138e8-117">內嵌提供者用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-117">The embedded provider is used to access files embedded in assemblies.</span></span> <span data-ttu-id="138e8-118">複合提供者則用來提供對一或多個其他提供者之檔案和目錄的合併存取。</span><span class="sxs-lookup"><span data-stu-id="138e8-118">The composite provider is used to provide combined access to files and directories from one or more other providers.</span></span>

### <a name="physicalfileprovider"></a><span data-ttu-id="138e8-119">PhysicalFileProvider</span><span class="sxs-lookup"><span data-stu-id="138e8-119">PhysicalFileProvider</span></span>

<span data-ttu-id="138e8-120">`PhysicalFileProvider` 提供對實體檔案系統的存取。</span><span class="sxs-lookup"><span data-stu-id="138e8-120">The `PhysicalFileProvider` provides access to the physical file system.</span></span> <span data-ttu-id="138e8-121">它會包裝 `System.IO.File` 類型 (用於實體提供者)，並將所有路徑的範圍設為某個目錄及其子系。</span><span class="sxs-lookup"><span data-stu-id="138e8-121">It wraps the `System.IO.File` type (for the physical provider), scoping all paths to a directory and its children.</span></span> <span data-ttu-id="138e8-122">此範圍會限制存取特定目錄及其子系，以防止存取這個界限以外的檔案系統。</span><span class="sxs-lookup"><span data-stu-id="138e8-122">This scoping limits access to a certain directory and its children, preventing access to the file system outside of this boundary.</span></span> <span data-ttu-id="138e8-123">具現化此提供者時，您必須為其提供一個目錄路徑，該路徑會作為對此提供者發出之所有要求的基礎路徑 (並限制此路徑之外的存取)。</span><span class="sxs-lookup"><span data-stu-id="138e8-123">When instantiating this provider, you must provide it with a directory path, which serves as the base path for all requests made to this provider (and which restricts access outside of this path).</span></span> <span data-ttu-id="138e8-124">在 ASP.NET Core 應用程式中，您可以直接具現化 `PhysicalFileProvider` 提供者，也可以透過[相依性插入](dependency-injection.md)在控制器或服務的建構函式中要求 `IFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="138e8-124">In an ASP.NET Core app, you can instantiate a `PhysicalFileProvider` provider directly, or you can request an `IFileProvider` in a Controller or service's constructor through [dependency injection](dependency-injection.md).</span></span> <span data-ttu-id="138e8-125">第二種方法通常會產生更有彈性且可測試的解決方案。</span><span class="sxs-lookup"><span data-stu-id="138e8-125">The latter approach will typically yield a more flexible and testable solution.</span></span>

<span data-ttu-id="138e8-126">下列範例示範如何建立 `PhysicalFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="138e8-126">The sample below shows how to create a `PhysicalFileProvider`.</span></span>


```csharp
IFileProvider provider = new PhysicalFileProvider(applicationRoot);
IDirectoryContents contents = provider.GetDirectoryContents(""); // the applicationRoot contents
IFileInfo fileInfo = provider.GetFileInfo("wwwroot/js/site.js"); // a file under applicationRoot
```

<span data-ttu-id="138e8-127">您可以逐一查看其目錄內容，或提供子路徑來取得特定檔案的資訊。</span><span class="sxs-lookup"><span data-stu-id="138e8-127">You can iterate through its directory contents or get a specific file's information by providing a subpath.</span></span>

<span data-ttu-id="138e8-128">若要從控制器要求提供者，請在控制器的建構函式中指定它，並將其指派給本機欄位。</span><span class="sxs-lookup"><span data-stu-id="138e8-128">To request a provider from a controller, specify it in the controller's constructor and assign it to a local field.</span></span> <span data-ttu-id="138e8-129">請使用動作方法中的本機執行個體：</span><span class="sxs-lookup"><span data-stu-id="138e8-129">Use the local instance from your action methods:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Controllers/HomeController.cs?highlight=5,7,12&range=6-19)]

<span data-ttu-id="138e8-130">接著，在應用程式的 `Startup` 類別中建立提供者：</span><span class="sxs-lookup"><span data-stu-id="138e8-130">Then, create the provider in the app's `Startup` class:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=35,40&range=1-43)]

<span data-ttu-id="138e8-131">在 *Index.cshtml* 檢視中，逐一查看提供的 `IDirectoryContents`：</span><span class="sxs-lookup"><span data-stu-id="138e8-131">In the *Index.cshtml* view, iterate through the `IDirectoryContents` provided:</span></span>

[!code-html[](file-providers/sample/src/FileProviderSample/Views/Home/Index.cshtml?highlight=2,7,9,11,15)]

<span data-ttu-id="138e8-132">結果：</span><span class="sxs-lookup"><span data-stu-id="138e8-132">The result:</span></span>

![列出實體檔案和資料夾的檔案提供者範例應用程式](file-providers/_static/physical-directory-listing.png)

### <a name="embeddedfileprovider"></a><span data-ttu-id="138e8-134">EmbeddedFileProvider</span><span class="sxs-lookup"><span data-stu-id="138e8-134">EmbeddedFileProvider</span></span>

<span data-ttu-id="138e8-135">`EmbeddedFileProvider` 用來存取內嵌於組件的檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-135">The `EmbeddedFileProvider` is used to access files embedded in assemblies.</span></span> <span data-ttu-id="138e8-136">在 .NET Core 中，您已在 *.csproj* 檔案內使用 `<EmbeddedResource>` 項目將檔案內嵌於組件：</span><span class="sxs-lookup"><span data-stu-id="138e8-136">In .NET Core, you embed files in an assembly with the `<EmbeddedResource>` element in the *.csproj* file:</span></span>

[!code-json[](file-providers/sample/src/FileProviderSample/FileProviderSample.csproj?range=13-18)]

<span data-ttu-id="138e8-137">指定要內嵌於組件的檔案時，您可以使用[萬用字元模式](#globbing-patterns)。</span><span class="sxs-lookup"><span data-stu-id="138e8-137">You can use [globbing patterns](#globbing-patterns) when specifying files to embed in the assembly.</span></span> <span data-ttu-id="138e8-138">這些模式可用來比對一或多個檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-138">These patterns can be used to match one or more files.</span></span>

> [!NOTE]
> <span data-ttu-id="138e8-139">您不太可能想要將專案中的每個 .js 檔案實際內嵌在其組件中；上述範例僅供示範之用。</span><span class="sxs-lookup"><span data-stu-id="138e8-139">It's unlikely you would ever want to actually embed every .js file in your project in its assembly; the above sample is for demo purposes only.</span></span>

<span data-ttu-id="138e8-140">建立 `EmbeddedFileProvider` 時，請將它會讀取的組件傳遞至其建構函式。</span><span class="sxs-lookup"><span data-stu-id="138e8-140">When creating an `EmbeddedFileProvider`, pass the assembly it will read to its constructor.</span></span>

```csharp
var embeddedProvider = new EmbeddedFileProvider(Assembly.GetEntryAssembly());
```

<span data-ttu-id="138e8-141">上述程式碼片段示範如何建立可存取目前執行組件的 `EmbeddedFileProvider`。</span><span class="sxs-lookup"><span data-stu-id="138e8-141">The snippet above demonstrates how to create an `EmbeddedFileProvider` with access to the currently executing assembly.</span></span>

<span data-ttu-id="138e8-142">更新範例應用程式以使用 `EmbeddedFileProvider` 時，會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="138e8-142">Updating the sample app to use an `EmbeddedFileProvider` results in the following output:</span></span>

![列出內嵌檔案的檔案提供者範例應用程式](file-providers/_static/embedded-directory-listing.png)

> [!NOTE]
> <span data-ttu-id="138e8-144">內嵌的資源不會公開目錄。</span><span class="sxs-lookup"><span data-stu-id="138e8-144">Embedded resources don't expose directories.</span></span> <span data-ttu-id="138e8-145">而是使用 `.` 分隔符號，將資源的路徑 (透過其命名空間) 內嵌在其檔案名稱中。</span><span class="sxs-lookup"><span data-stu-id="138e8-145">Rather, the path to the resource (via its namespace) is embedded in its filename using `.` separators.</span></span>

> [!TIP]
> <span data-ttu-id="138e8-146">`EmbeddedFileProvider` 建構函式可接受選擇性的 `baseNamespace` 參數。</span><span class="sxs-lookup"><span data-stu-id="138e8-146">The `EmbeddedFileProvider` constructor accepts an optional `baseNamespace` parameter.</span></span> <span data-ttu-id="138e8-147">指定此參數會將 `GetDirectoryContents` 的範圍設為所提供命名空間底下的那些資源。</span><span class="sxs-lookup"><span data-stu-id="138e8-147">Specifying this will scope calls to `GetDirectoryContents` to those resources under the provided namespace.</span></span>

### <a name="compositefileprovider"></a><span data-ttu-id="138e8-148">CompositeFileProvider</span><span class="sxs-lookup"><span data-stu-id="138e8-148">CompositeFileProvider</span></span>

<span data-ttu-id="138e8-149">`CompositeFileProvider` 結合了 `IFileProvider` 執行個體，並公開單一介面來處理來自多個提供者的檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-149">The `CompositeFileProvider` combines `IFileProvider` instances, exposing a single interface for working with files from multiple providers.</span></span> <span data-ttu-id="138e8-150">建立 `CompositeFileProvider` 時，您可以將一或多個 `IFileProvider` 執行個體傳遞至其建構函式：</span><span class="sxs-lookup"><span data-stu-id="138e8-150">When creating the `CompositeFileProvider`, you pass one or more `IFileProvider` instances to its constructor:</span></span>

[!code-csharp[](file-providers/sample/src/FileProviderSample/Startup.cs?highlight=3&range=35-37)]

<span data-ttu-id="138e8-151">更新範例應用程式使用 `CompositeFileProvider` (其同時包含先前設定的實體提供者和內嵌提供者) 時，會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="138e8-151">Updating the sample app to use a `CompositeFileProvider` that includes both the physical and embedded providers configured previously, results in the following output:</span></span>

![同時列出實體檔案和資料夾與內嵌檔案的檔案提供者範例應用程式](file-providers/_static/composite-directory-listing.png)

## <a name="watching-for-changes"></a><span data-ttu-id="138e8-153">監看變更</span><span class="sxs-lookup"><span data-stu-id="138e8-153">Watching for changes</span></span>

<span data-ttu-id="138e8-154">`IFileProvider` `Watch` 方法提供一種方式，用來監看一或多個檔案或目錄是否有變更。</span><span class="sxs-lookup"><span data-stu-id="138e8-154">The `IFileProvider` `Watch` method provides a way to watch one or more files or directories for changes.</span></span> <span data-ttu-id="138e8-155">這個方法接受路徑字串 (它可以使用[萬用字元模式](#globbing-patterns)來指定多個檔案)，並傳回 `IChangeToken`。</span><span class="sxs-lookup"><span data-stu-id="138e8-155">This method accepts a path string, which can use [globbing patterns](#globbing-patterns) to specify multiple files, and returns an `IChangeToken`.</span></span> <span data-ttu-id="138e8-156">這個權杖會將可以檢查的 `HasChanged` 屬性，以及偵測到變更時所呼叫的 `RegisterChangeCallback` 方法公開至指定的路徑字串。</span><span class="sxs-lookup"><span data-stu-id="138e8-156">This token exposes a `HasChanged` property that can be inspected, and a `RegisterChangeCallback` method that's called when changes are detected to the specified path string.</span></span> <span data-ttu-id="138e8-157">請注意，每個變更權杖只會呼叫其相關聯的回呼，以回應單一變更。</span><span class="sxs-lookup"><span data-stu-id="138e8-157">Note that each change token only calls its associated callback in response to a single change.</span></span> <span data-ttu-id="138e8-158">若要啟用持續監視，您可以如下所示使用 `TaskCompletionSource`，或重新建立 `IChangeToken` 執行個體以回應變更。</span><span class="sxs-lookup"><span data-stu-id="138e8-158">To enable constant monitoring, you can use a `TaskCompletionSource` as shown below, or re-create `IChangeToken` instances in response to changes.</span></span>

<span data-ttu-id="138e8-159">在本文的範例中，主控台應用程式會設定為每次修改文字檔時，顯示一則訊息：</span><span class="sxs-lookup"><span data-stu-id="138e8-159">In this article's sample, a console application is configured to display a message whenever a text file is modified:</span></span>

[!code-csharp[](file-providers/sample/src/WatchConsole/Program.cs?name=snippet1&highlight=1-2,16,19-20)]

<span data-ttu-id="138e8-160">儲存檔案數次之後的結果：</span><span class="sxs-lookup"><span data-stu-id="138e8-160">The result, after saving the file several times:</span></span>

![執行 dotnet run 之後的命令視窗顯示應用程式監視 quotes.txt 檔案是否有變更，並顯示檔案已變更五次。](file-providers/_static/watch-console.png)

> [!NOTE]
> <span data-ttu-id="138e8-162">某些檔案系統 (例如 Docker 容器和網路共用) 可能無法可靠地傳送變更通知。</span><span class="sxs-lookup"><span data-stu-id="138e8-162">Some file systems, such as Docker containers and network shares, may not reliably send change notifications.</span></span> <span data-ttu-id="138e8-163">請將 `DOTNET_USE_POLLINGFILEWATCHER` 環境變數設定為 `1` 或 `true`，以便每 4 秒輪詢檔案系統是否有變更。</span><span class="sxs-lookup"><span data-stu-id="138e8-163">Set the `DOTNET_USE_POLLINGFILEWATCHER` environment variable to `1` or `true` to poll the file system for changes every 4 seconds.</span></span>

## <a name="globbing-patterns"></a><span data-ttu-id="138e8-164">萬用字元模式</span><span class="sxs-lookup"><span data-stu-id="138e8-164">Globbing patterns</span></span>

<span data-ttu-id="138e8-165">檔案系統路徑中使用稱為「萬用字元模式」(*globbing patterns*) 的模式。</span><span class="sxs-lookup"><span data-stu-id="138e8-165">File system paths use wildcard patterns called *globbing patterns*.</span></span> <span data-ttu-id="138e8-166">這些簡單的模式可以用來指定檔案群組。</span><span class="sxs-lookup"><span data-stu-id="138e8-166">These simple patterns can be used to specify groups of files.</span></span> <span data-ttu-id="138e8-167">兩種萬用字元為 `*` 和 `**`。</span><span class="sxs-lookup"><span data-stu-id="138e8-167">The two wildcard characters are `*` and `**`.</span></span>

**`*`**

   <span data-ttu-id="138e8-168">符合目前資料夾層級的任何項目，或者任何檔案名稱或任何副檔名。</span><span class="sxs-lookup"><span data-stu-id="138e8-168">Matches anything at the current folder level, or any filename, or any file extension.</span></span> <span data-ttu-id="138e8-169">相符項是以檔案路徑中的 `/` 和 `.` 字元終止。</span><span class="sxs-lookup"><span data-stu-id="138e8-169">Matches are terminated by `/` and `.` characters in the file path.</span></span>

<strong><code>**</code></strong>

   <span data-ttu-id="138e8-170">符合多個目錄層級之間的任何項目。</span><span class="sxs-lookup"><span data-stu-id="138e8-170">Matches anything across multiple directory levels.</span></span> <span data-ttu-id="138e8-171">可用來以遞迴方式符合目錄階層內的許多檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-171">Can be used to recursively match many files within a directory hierarchy.</span></span>

### <a name="globbing-pattern-examples"></a><span data-ttu-id="138e8-172">萬用字元模式範例</span><span class="sxs-lookup"><span data-stu-id="138e8-172">Globbing pattern examples</span></span>

**`directory/file.txt`**

   <span data-ttu-id="138e8-173">符合特定目錄中的特定檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-173">Matches a specific file in a specific directory.</span></span>

**<code>directory/*.txt</code>**

   <span data-ttu-id="138e8-174">符合特定目錄中具有 `.txt` 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-174">Matches all files with `.txt` extension in a specific directory.</span></span>

**`directory/*/bower.json`**

   <span data-ttu-id="138e8-175">符合 `directory` 目錄下一層級的目錄中的所有 `bower.json` 檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-175">Matches all `bower.json` files in directories exactly one level below the `directory` directory.</span></span>

**<code>directory/&#42;&#42;/&#42;.txt</code>**

   <span data-ttu-id="138e8-176">符合在 `directory` 目錄下的任何地方所找到之具有 `.txt` 副檔名的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-176">Matches all files with `.txt` extension found anywhere under the `directory` directory.</span></span>

## <a name="file-provider-usage-in-aspnet-core"></a><span data-ttu-id="138e8-177">ASP.NET Core 中的檔案提供者使用方式</span><span class="sxs-lookup"><span data-stu-id="138e8-177">File Provider usage in ASP.NET Core</span></span>

<span data-ttu-id="138e8-178">ASP.NET Core 的數個部分會利用檔案提供者。</span><span class="sxs-lookup"><span data-stu-id="138e8-178">Several parts of ASP.NET Core utilize file providers.</span></span> <span data-ttu-id="138e8-179">`IHostingEnvironment` 將應用程式的內容根目錄與 Web 根目錄公開為 `IFileProvider` 類型。</span><span class="sxs-lookup"><span data-stu-id="138e8-179">`IHostingEnvironment` exposes the app's content root and web root as `IFileProvider` types.</span></span> <span data-ttu-id="138e8-180">靜態檔案中介軟體使用檔案提供者來尋找靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-180">The static files middleware uses file providers to locate static files.</span></span> <span data-ttu-id="138e8-181">Razor 大量使用 `IFileProvider` 來尋找檢視。</span><span class="sxs-lookup"><span data-stu-id="138e8-181">Razor makes heavy use of `IFileProvider` in locating views.</span></span> <span data-ttu-id="138e8-182">Dotnet 的發行功能使用檔案提供者和萬用字元模式來指定應發行哪些檔案。</span><span class="sxs-lookup"><span data-stu-id="138e8-182">Dotnet's publish functionality uses file providers and globbing patterns to specify which files should be published.</span></span>

## <a name="recommendations-for-use-in-apps"></a><span data-ttu-id="138e8-183">在應用程式中使用的建議</span><span class="sxs-lookup"><span data-stu-id="138e8-183">Recommendations for use in apps</span></span>

<span data-ttu-id="138e8-184">如果 ASP.NET Core 應用程式需要檔案系統存取，您可以透過相依性插入要求 `IFileProvider` 的執行個體，然後使用其方法來執行存取，如此範例所示。</span><span class="sxs-lookup"><span data-stu-id="138e8-184">If your ASP.NET Core app requires file system access, you can request an instance of `IFileProvider` through dependency injection, and then use its methods to perform the access, as shown in this sample.</span></span> <span data-ttu-id="138e8-185">這可讓您在應用程式啟動時設定提供者一次，並減少應用程式具現化的實作類型的數目。</span><span class="sxs-lookup"><span data-stu-id="138e8-185">This allows you to configure the provider once, when the app starts up, and reduces the number of implementation types your app instantiates.</span></span>
