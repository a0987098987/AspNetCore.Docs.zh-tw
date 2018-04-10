---
title: 從 ASP.NET Core 1.x 遷移至 2.0
author: scottaddie
description: 本文概述將 ASP.NET Core 1.x 專案移轉至 ASP.NET Core 2.0 的必要條件和最常見步驟。
manager: wpickett
ms.author: scaddie
ms.date: 10/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/1x-to-2x/index
ms.openlocfilehash: 4b7a13b31340f01c4f1527f602b925d3ac4e8241
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
---
# <a name="migrate-from-aspnet-core-1x-to-20"></a><span data-ttu-id="f7c03-103">從 ASP.NET Core 1.x 遷移至 2.0</span><span class="sxs-lookup"><span data-stu-id="f7c03-103">Migrate from ASP.NET Core 1.x to 2.0</span></span>

<span data-ttu-id="f7c03-104">作者：[Scott Addie](https://github.com/scottaddie)</span><span class="sxs-lookup"><span data-stu-id="f7c03-104">By [Scott Addie](https://github.com/scottaddie)</span></span>

<span data-ttu-id="f7c03-105">在本文中，我們將引導您將現有的 ASP.NET Core 1.x 專案更新至 ASP.NET Core 2.0。</span><span class="sxs-lookup"><span data-stu-id="f7c03-105">In this article, we'll walk you through updating an existing ASP.NET Core 1.x project to ASP.NET Core 2.0.</span></span> <span data-ttu-id="f7c03-106">將應用程式移轉至 ASP.NET Core 2.0 可讓您充分利用[許多新功能和效能改進](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="f7c03-106">Migrating your application to ASP.NET Core 2.0 enables you to take advantage of [many new features and performance improvements](https://docs.microsoft.com/aspnet/core/aspnetcore-2.0).</span></span> 

<span data-ttu-id="f7c03-107">現有的 ASP.NET Core 1.x 應用程式是根據特定版本的專案範本。</span><span class="sxs-lookup"><span data-stu-id="f7c03-107">Existing ASP.NET Core 1.x applications are based off of version-specific project templates.</span></span> <span data-ttu-id="f7c03-108">隨著 ASP.NET Core 架構發展，其中內含的專案範本和起始程式碼也跟著演進。</span><span class="sxs-lookup"><span data-stu-id="f7c03-108">As the ASP.NET Core framework evolves, so do the project templates and the starter code contained within them.</span></span> <span data-ttu-id="f7c03-109">除了更新 ASP.NET Core 架構之外，您還需要更新應用程式的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7c03-109">In addition to updating the ASP.NET Core framework, you need to update the code for your application.</span></span>

<a name="prerequisites"></a>

## <a name="prerequisites"></a><span data-ttu-id="f7c03-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="f7c03-110">Prerequisites</span></span>
<span data-ttu-id="f7c03-111">請參閱[開始使用 ASP.NET Core](xref:getting-started)。</span><span class="sxs-lookup"><span data-stu-id="f7c03-111">Please see [Get Started with ASP.NET Core](xref:getting-started).</span></span>

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a><span data-ttu-id="f7c03-112">更新 Target Framework Moniker (TFM)</span><span class="sxs-lookup"><span data-stu-id="f7c03-112">Update Target Framework Moniker (TFM)</span></span>
<span data-ttu-id="f7c03-113">以 .NET Core 為目標的專案應該使用版本大於或等於 .NET Core 2.0 的 [TFM](/dotnet/standard/frameworks#referring-to-frameworks)。</span><span class="sxs-lookup"><span data-stu-id="f7c03-113">Projects targeting .NET Core should use the [TFM](/dotnet/standard/frameworks#referring-to-frameworks) of a version greater than or equal to .NET Core 2.0.</span></span> <span data-ttu-id="f7c03-114">在 *.csproj* 檔案中搜尋 `<TargetFramework>` 節點，並以 `netcoreapp2.0` 取代其內部文字：</span><span class="sxs-lookup"><span data-stu-id="f7c03-114">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `netcoreapp2.0`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=3)]

<span data-ttu-id="f7c03-115">以 .NET Framework 為目標的專案應該使用版本大於或等於 .NET Framework 4.6.1 的 TFM。</span><span class="sxs-lookup"><span data-stu-id="f7c03-115">Projects targeting .NET Framework should use the TFM of a version greater than or equal to .NET Framework 4.6.1.</span></span> <span data-ttu-id="f7c03-116">在 *.csproj* 檔案中搜尋 `<TargetFramework>` 節點，並以 `net461` 取代其內部文字：</span><span class="sxs-lookup"><span data-stu-id="f7c03-116">Search for the `<TargetFramework>` node in the *.csproj* file, and replace its inner text with `net461`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> <span data-ttu-id="f7c03-117">.NET Core 2.0 提供了比 .NET Core 1.x 更大的介面區。</span><span class="sxs-lookup"><span data-stu-id="f7c03-117">.NET Core 2.0 offers a much larger surface area than .NET Core 1.x.</span></span> <span data-ttu-id="f7c03-118">如果您只是因為 .NET Core 1.x 中遺漏了 API 而以 .NET Framework 為目標，以 .NET Core 2.0 為目標可能有效。</span><span class="sxs-lookup"><span data-stu-id="f7c03-118">If you're targeting .NET Framework solely because of missing APIs in .NET Core 1.x, targeting .NET Core 2.0 is likely to work.</span></span>

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a><span data-ttu-id="f7c03-119">更新 global.json 中的 .NET Core SDK 版本</span><span class="sxs-lookup"><span data-stu-id="f7c03-119">Update .NET Core SDK version in global.json</span></span>
<span data-ttu-id="f7c03-120">如果您的方案依賴 [*global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) 檔案而以特定的 .NET Core SDK 版本為目標，請更新其 `version` 屬性，以使用您電腦上安裝的 2.0 版：</span><span class="sxs-lookup"><span data-stu-id="f7c03-120">If your solution relies upon a [*global.json*](https://docs.microsoft.com/dotnet/core/tools/global-json) file to target a specific .NET Core SDK version, update its `version` property to use the 2.0 version installed on your machine:</span></span>

[!code-json[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a><span data-ttu-id="f7c03-121">更新套件參考</span><span class="sxs-lookup"><span data-stu-id="f7c03-121">Update package references</span></span>
<span data-ttu-id="f7c03-122">1.x 專案中的 *.csproj* 檔案列出專案所使用的每個 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f7c03-122">The *.csproj* file in a 1.x project lists each NuGet package used by the project.</span></span>

<span data-ttu-id="f7c03-123">在以 .NET Core 2.0 為目標的 ASP.NET Core 2.0 專案中，*.csproj* 檔案中的單一[中繼套件](xref:fundamentals/metapackage) metapackage 參考會取代套件的集合：</span><span class="sxs-lookup"><span data-stu-id="f7c03-123">In an ASP.NET Core 2.0 project targeting .NET Core 2.0, a single [metapackage](xref:fundamentals/metapackage) reference in the *.csproj* file replaces the collection of packages:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=8-10)]

<span data-ttu-id="f7c03-124">中繼套件包含 ASP.NET Core 2.0 和 Entity Framework Core 2.0 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="f7c03-124">All the features of ASP.NET Core 2.0 and Entity Framework Core 2.0 are included in the metapackage.</span></span>

<span data-ttu-id="f7c03-125">以 .NET Framework 為目標時，ASP.NET Core 2.0 專案應該繼續參考個別的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f7c03-125">ASP.NET Core 2.0 projects targeting .NET Framework should continue to reference individual NuGet packages.</span></span> <span data-ttu-id="f7c03-126">將每個 `<PackageReference />` 節點的 `Version` 屬性更新為 2.0.0。</span><span class="sxs-lookup"><span data-stu-id="f7c03-126">Update the `Version` attribute of each `<PackageReference />` node to 2.0.0.</span></span>

<span data-ttu-id="f7c03-127">例如，以下是以 .NET Framework 為目標的典型 ASP.NET Core 2.0 專案所使用的 `<PackageReference />` 節點清單：</span><span class="sxs-lookup"><span data-stu-id="f7c03-127">For example, here's the list of `<PackageReference />` nodes used in a typical ASP.NET Core 2.0 project targeting .NET Framework:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a><span data-ttu-id="f7c03-128">更新 .NET Core CLI 工具</span><span class="sxs-lookup"><span data-stu-id="f7c03-128">Update .NET Core CLI tools</span></span>
<span data-ttu-id="f7c03-129">在 *.csproj* 檔案中，將每個 `<DotNetCliToolReference />` 節點的 `Version` 屬性更新為 2.0.0。</span><span class="sxs-lookup"><span data-stu-id="f7c03-129">In the *.csproj* file, update the `Version` attribute of each `<DotNetCliToolReference />` node to 2.0.0.</span></span>

<span data-ttu-id="f7c03-130">例如，以下是以 .NET Core 2.0 為目標的典型 ASP.NET Core 2.0 專案所使用的 CLI 工具清單：</span><span class="sxs-lookup"><span data-stu-id="f7c03-130">For example, here's the list of CLI tools used in a typical ASP.NET Core 2.0 project targeting .NET Core 2.0:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=12-16)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a><span data-ttu-id="f7c03-131">重新命名套件目標後援屬性</span><span class="sxs-lookup"><span data-stu-id="f7c03-131">Rename Package Target Fallback property</span></span>
<span data-ttu-id="f7c03-132">1.x 專案的 *.csproj* 檔案使用 `PackageTargetFallback` 節點和變數：</span><span class="sxs-lookup"><span data-stu-id="f7c03-132">The *.csproj* file of a 1.x project used a `PackageTargetFallback` node and variable:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=5)]

<span data-ttu-id="f7c03-133">將節點與變數都重新命名為 `AssetTargetFallback`：</span><span class="sxs-lookup"><span data-stu-id="f7c03-133">Rename both the node and variable to `AssetTargetFallback`:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App.csproj?range=4)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a><span data-ttu-id="f7c03-134">更新 Program.cs 中的 Main 方法</span><span class="sxs-lookup"><span data-stu-id="f7c03-134">Update Main method in Program.cs</span></span>
<span data-ttu-id="f7c03-135">在 1.x 專案中，*Program.cs*的 `Main` 方法看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="f7c03-135">In 1.x projects, the `Main` method of *Program.cs* looked like this:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

<span data-ttu-id="f7c03-136">在 2.0 專案中，*Program.cs*的 `Main` 方法已經過簡化：</span><span class="sxs-lookup"><span data-stu-id="f7c03-136">In 2.0 projects, the `Main` method of *Program.cs* has been simplified:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program.cs?highlight=8-11)]

<span data-ttu-id="f7c03-137">強烈建議您採用這個新的 2.0 模式，像[Entity Framework (EF) Core 移轉](xref:data/ef-mvc/migrations)這類的產品功能必須有這個模式才能運作。</span><span class="sxs-lookup"><span data-stu-id="f7c03-137">The adoption of this new 2.0 pattern is highly recommended and is required for product features like [Entity Framework (EF) Core Migrations](xref:data/ef-mvc/migrations) to work.</span></span> <span data-ttu-id="f7c03-138">例如，從 [套件管理員主控台] 視窗執行 `Update-Database` 或從命令列 (在轉換成 ASP.NET Core 2.0 的專案中) 執行 `dotnet ef database update` 會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="f7c03-138">For example, running `Update-Database` from the Package Manager Console window or `dotnet ef database update` from the command line (on projects converted to ASP.NET Core 2.0) generates the following error:</span></span>

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="add-modify-configuration"></a>

## <a name="add-configuration-providers"></a><span data-ttu-id="f7c03-139">新增組態提供者</span><span class="sxs-lookup"><span data-stu-id="f7c03-139">Add configuration providers</span></span>
<span data-ttu-id="f7c03-140">在 1.x 專案中，若要將組態提供者新增至應用程式，必須透過 `Startup` 建構函式。</span><span class="sxs-lookup"><span data-stu-id="f7c03-140">In 1.x projects, adding configuration providers to an app was accomplished via the `Startup` constructor.</span></span> <span data-ttu-id="f7c03-141">其中涉及的步驟包括建立 `ConfigurationBuilder` 的執行個體、載入適用的提供者 (環境變數、應用程式設定等等)，以及初始化 `IConfigurationRoot` 的成員。</span><span class="sxs-lookup"><span data-stu-id="f7c03-141">The steps involved creating an instance of `ConfigurationBuilder`, loading applicable providers (environment variables, app settings, etc.), and initializing a member of `IConfigurationRoot`.</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_1xStartup)]

<span data-ttu-id="f7c03-142">上述範例載入 `Configuration` 成員時，會使用 *appsettings.json* 與所有 *appsettings.\<EnvironmentName\>.json* 檔案中符合 `IHostingEnvironment.EnvironmentName` 屬性的組態設定。</span><span class="sxs-lookup"><span data-stu-id="f7c03-142">The preceding example loads the `Configuration` member with configuration settings from *appsettings.json* as well as any *appsettings.\<EnvironmentName\>.json* file matching the `IHostingEnvironment.EnvironmentName` property.</span></span> <span data-ttu-id="f7c03-143">這些檔案的位置與 *Startup.cs* 的路徑相同。</span><span class="sxs-lookup"><span data-stu-id="f7c03-143">The location of these files is at the same path as *Startup.cs*.</span></span>

<span data-ttu-id="f7c03-144">在 2.0 專案中，1.x 專案固有的模板組態程式碼會在幕後執行。</span><span class="sxs-lookup"><span data-stu-id="f7c03-144">In 2.0 projects, the boilerplate configuration code inherent to 1.x projects runs behind-the-scenes.</span></span> <span data-ttu-id="f7c03-145">例如，環境變數及應用程式設定會在啟動時載入。</span><span class="sxs-lookup"><span data-stu-id="f7c03-145">For example, environment variables and app settings are loaded at startup.</span></span> <span data-ttu-id="f7c03-146">對等的 *Startup.cs* 程式碼會縮減成為插入執行個體的 `IConfiguration` 初始化：</span><span class="sxs-lookup"><span data-stu-id="f7c03-146">The equivalent *Startup.cs* code is reduced to `IConfiguration` initialization with the injected instance:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Startup.cs?name=snippet_2xStartup)]

<span data-ttu-id="f7c03-147">若要移除 `WebHostBuilder.CreateDefaultBuilder` 所新增的預設提供者，請叫用 `ConfigureAppConfiguration` 內 `IConfigurationBuilder.Sources` 屬性上的 `Clear` 方法。</span><span class="sxs-lookup"><span data-stu-id="f7c03-147">To remove the default providers added by `WebHostBuilder.CreateDefaultBuilder`, invoke the `Clear` method on the `IConfigurationBuilder.Sources` property inside of `ConfigureAppConfiguration`.</span></span> <span data-ttu-id="f7c03-148">若要將提供者新增回來，請利用 *Program.cs* 中的 `ConfigureAppConfiguration` 方法：</span><span class="sxs-lookup"><span data-stu-id="f7c03-148">To add providers back, utilize the `ConfigureAppConfiguration` method in *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/Program.cs?name=snippet_ProgramMainConfigProviders&highlight=9-14)]

<span data-ttu-id="f7c03-149">[此處](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152) \(英文\) 有前述程式碼片段中 `CreateDefaultBuilder` 方法所使用的組態。</span><span class="sxs-lookup"><span data-stu-id="f7c03-149">The configuration used by the `CreateDefaultBuilder` method in the preceding code snippet can be seen [here](https://github.com/aspnet/MetaPackages/blob/rel/2.0.0/src/Microsoft.AspNetCore/WebHost.cs#L152).</span></span>

<span data-ttu-id="f7c03-150">如需詳細資訊，請參閱 [ASP.NET Core 中的組態](xref:fundamentals/configuration/index)。</span><span class="sxs-lookup"><span data-stu-id="f7c03-150">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

<a name="db-init-code"></a>

## <a name="move-database-initialization-code"></a><span data-ttu-id="f7c03-151">移動資料庫初始化程式碼</span><span class="sxs-lookup"><span data-stu-id="f7c03-151">Move database initialization code</span></span>
<span data-ttu-id="f7c03-152">在使用 EF Core 1.x 的 1.x 專案中，`dotnet ef migrations add` 這類命令會執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="f7c03-152">In 1.x projects using EF Core 1.x, a command such as `dotnet ef migrations add` does the following:</span></span>
1. <span data-ttu-id="f7c03-153">將 `Startup` 執行個體具現化</span><span class="sxs-lookup"><span data-stu-id="f7c03-153">Instantiates a `Startup` instance</span></span>
2. <span data-ttu-id="f7c03-154">叫用 `ConfigureServices` 方法以註冊所有具相依性導入的服務 (包括 `DbContext` 類型)</span><span class="sxs-lookup"><span data-stu-id="f7c03-154">Invokes the `ConfigureServices` method to register all services with dependency injection (including `DbContext` types)</span></span>
3. <span data-ttu-id="f7c03-155">執行其必要工作</span><span class="sxs-lookup"><span data-stu-id="f7c03-155">Performs its requisite tasks</span></span>

<span data-ttu-id="f7c03-156">在使用 EF Core 2.0 的 2.0 專案中，則會叫用 `Program.BuildWebHost` 來取得應用程式服務。</span><span class="sxs-lookup"><span data-stu-id="f7c03-156">In 2.0 projects using EF Core 2.0, `Program.BuildWebHost` is invoked to obtain the application services.</span></span> <span data-ttu-id="f7c03-157">與 1.x 不同的是，這會有叫用 `Startup.Configure` 的額外副作用。</span><span class="sxs-lookup"><span data-stu-id="f7c03-157">Unlike 1.x, this has the additional side effect of invoking `Startup.Configure`.</span></span> <span data-ttu-id="f7c03-158">如果您的 1.x 應用程式在其 `Configure` 方法中叫用了資料庫初始化程式碼，可能會發生未預期的問題。</span><span class="sxs-lookup"><span data-stu-id="f7c03-158">If your 1.x app invoked database initialization code in its `Configure` method, unexpected problems can occur.</span></span> <span data-ttu-id="f7c03-159">例如，如果資料庫尚不存在，植入程式碼會在 EF Core 移轉命令執行前就執行程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7c03-159">For example, if the database doesn't yet exist, the seeding code runs before the EF Core Migrations command execution.</span></span> <span data-ttu-id="f7c03-160">如果資料庫尚不存在，這個問題會造成 `dotnet ef migrations list` 命令失敗。</span><span class="sxs-lookup"><span data-stu-id="f7c03-160">This problem causes a `dotnet ef migrations list` command to fail if the database doesn't yet exist.</span></span>

<span data-ttu-id="f7c03-161">請考慮在 *Startup.cs* 的 `Configure` 方法中使用下列 1.x 植入初始化程式碼：</span><span class="sxs-lookup"><span data-stu-id="f7c03-161">Consider the following 1.x seed initialization code in the `Configure` method of *Startup.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Startup.cs?name=snippet_ConfigureSeedData&highlight=8)]

<span data-ttu-id="f7c03-162">在 2.0 專案中，將 `SeedData.Initialize` 呼叫移到 *Program.cs* 的 `Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="f7c03-162">In 2.0 projects, move the `SeedData.Initialize` call to the `Main` method of *Program.cs*:</span></span>

[!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore2App/AspNetCoreDotNetCore2App/Program2.cs?name=snippet_Main2Code&highlight=10)]

<span data-ttu-id="f7c03-163">從 2.0 開始，除了建置及設定網頁主機外，並不適合在 `BuildWebHost` 中執行任何作業。</span><span class="sxs-lookup"><span data-stu-id="f7c03-163">As of 2.0, it's bad practice to do anything in `BuildWebHost` except build and configure the web host.</span></span> <span data-ttu-id="f7c03-164">有關執行應用程式的任何作業都應該在 `BuildWebHost` 外處理 &mdash; 通常會在 *Program.cs* 的 `Main` 方法中處理。</span><span class="sxs-lookup"><span data-stu-id="f7c03-164">Anything that's about running the application should be handled outside of `BuildWebHost` &mdash; typically in the `Main` method of *Program.cs*.</span></span>

<a name="view-compilation"></a>

## <a name="review-razor-view-compilation-setting"></a><span data-ttu-id="f7c03-165">檢閱 Razor 檢視編譯設定</span><span class="sxs-lookup"><span data-stu-id="f7c03-165">Review Razor view compilation setting</span></span>
<span data-ttu-id="f7c03-166">更快速的應用程式啟動時間和較小的發行組合對您而言極為重要。</span><span class="sxs-lookup"><span data-stu-id="f7c03-166">Faster application startup time and smaller published bundles are of utmost importance to you.</span></span> <span data-ttu-id="f7c03-167">基於這些原因，預設會在 ASP.NET Core 2.0 中啟用 [Razor 檢視編譯](xref:mvc/views/view-compilation)。</span><span class="sxs-lookup"><span data-stu-id="f7c03-167">For these reasons, [Razor view compilation](xref:mvc/views/view-compilation) is enabled by default in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="f7c03-168">已不再需要將 `MvcRazorCompileOnPublish` 屬性設定為 true。</span><span class="sxs-lookup"><span data-stu-id="f7c03-168">Setting the `MvcRazorCompileOnPublish` property to true is no longer required.</span></span> <span data-ttu-id="f7c03-169">除非您停用檢視編譯，否則屬性可能會從 *.csproj* 檔案中移除。</span><span class="sxs-lookup"><span data-stu-id="f7c03-169">Unless you're disabling view compilation, the property may be removed from the *.csproj* file.</span></span>

<span data-ttu-id="f7c03-170">以 .NET Framework 為目標時，仍然需要在 *.csproj* 檔案中明確參考 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="f7c03-170">When targeting .NET Framework, you still need to explicitly reference the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet package in your *.csproj* file:</span></span>

[!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a><span data-ttu-id="f7c03-171">依賴 Application Insights「啟動」功能</span><span class="sxs-lookup"><span data-stu-id="f7c03-171">Rely on Application Insights "light-up" features</span></span>
<span data-ttu-id="f7c03-172">輕鬆安裝應用程式效能檢測很重要。</span><span class="sxs-lookup"><span data-stu-id="f7c03-172">Effortless setup of application performance instrumentation is important.</span></span> <span data-ttu-id="f7c03-173">您現在可以依賴 Visual Studio 2017 工具中提供的 [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview)「啟動」功能。</span><span class="sxs-lookup"><span data-stu-id="f7c03-173">You can now rely on the new [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview) "light-up" features available in the Visual Studio 2017 tooling.</span></span>

<span data-ttu-id="f7c03-174">根據預設，在 Visual Studio 2017 中建立的 ASP.NET Core 1.1 專案已新增 Application Insights。</span><span class="sxs-lookup"><span data-stu-id="f7c03-174">ASP.NET Core 1.1 projects created in Visual Studio 2017 added Application Insights by default.</span></span> <span data-ttu-id="f7c03-175">如果您不要直接使用 Application Insights SDK，請在 *Program.cs* 和 *Startup.cs* 之外，遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="f7c03-175">If you're not using the Application Insights SDK directly, outside of *Program.cs* and *Startup.cs*, follow these steps:</span></span>

1. <span data-ttu-id="f7c03-176">如果以 .NET Core 為目標，請從 *.csproj* 檔案中移除下列 `<PackageReference />` 節點：</span><span class="sxs-lookup"><span data-stu-id="f7c03-176">If targeting .NET Core, remove the following `<PackageReference />` node from the *.csproj* file:</span></span>
    
    [!code-xml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App.csproj?range=10)]

2. <span data-ttu-id="f7c03-177">如果以 .NET Core 為目標，請從 *Program.cs* 中移除 `UseApplicationInsights` 擴充方法引動過程：</span><span class="sxs-lookup"><span data-stu-id="f7c03-177">If targeting .NET Core, remove the `UseApplicationInsights` extension method invocation from *Program.cs*:</span></span>

    [!code-csharp[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. <span data-ttu-id="f7c03-178">從 *_Layout.cshtml* 中移除 Application Insights 用戶端 API 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f7c03-178">Remove the Application Insights client-side API call from *_Layout.cshtml*.</span></span> <span data-ttu-id="f7c03-179">它包含下列兩行程式碼：</span><span class="sxs-lookup"><span data-stu-id="f7c03-179">It comprises the following two lines of code:</span></span>

    [!code-cshtml[](../1x-to-2x/samples/AspNetCoreDotNetCore1App/AspNetCoreDotNetCore1App/Views/Shared/_Layout.cshtml?range=1,19&dedent=4)]

<span data-ttu-id="f7c03-180">如果您要直接使用 Application Insights SDK，請繼續執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="f7c03-180">If you are using the Application Insights SDK directly, continue to do so.</span></span> <span data-ttu-id="f7c03-181">2.0 [中繼套件](xref:fundamentals/metapackage)含有最新版本的 Application Insights，因此如果您參考的是較舊的版本，就會出現套件降級錯誤。</span><span class="sxs-lookup"><span data-stu-id="f7c03-181">The 2.0 [metapackage](xref:fundamentals/metapackage) includes the latest version of Application Insights, so a package downgrade error appears if you're referencing an older version.</span></span>

<a name="auth-and-identity"></a>

## <a name="adopt-authenticationidentity-improvements"></a><span data-ttu-id="f7c03-182">採用驗證/身分識別的改進功能</span><span class="sxs-lookup"><span data-stu-id="f7c03-182">Adopt authentication/Identity improvements</span></span>
<span data-ttu-id="f7c03-183">ASP.NET Core 2.0 具有新的驗證模型和對 ASP.NET Core 身分識別的一些重大變更。</span><span class="sxs-lookup"><span data-stu-id="f7c03-183">ASP.NET Core 2.0 has a new authentication model and a number of significant changes to ASP.NET Core Identity.</span></span> <span data-ttu-id="f7c03-184">如果您在啟用個別使用者帳戶的情況下建立專案，或如果已手動新增驗證或身分識別，請參閱[將驗證和身分識別遷移至 ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)。</span><span class="sxs-lookup"><span data-stu-id="f7c03-184">If you created your project with Individual User Accounts enabled, or if you have manually added authentication or Identity, see [Migrate Authentication and Identity to ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f7c03-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="f7c03-185">Additional resources</span></span>

* [<span data-ttu-id="f7c03-186">ASP.NET Core 2.0 的重大變更</span><span class="sxs-lookup"><span data-stu-id="f7c03-186">Breaking Changes in ASP.NET Core 2.0</span></span>](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
