---
title: 在 ASP.NET Core 中使用 IHostingStartup 從外部組件增強應用程式
author: guardrex
description: 了解如何使用 IHostingStartup 實作，從外部組件增強 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 08/13/2018
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: a06c2da04c1631f5811a535c891ca5190b0d8864
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207533"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="2bdf6-103">在 ASP.NET Core 中使用 IHostingStartup 從外部組件增強應用程式</span><span class="sxs-lookup"><span data-stu-id="2bdf6-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="2bdf6-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="2bdf6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2bdf6-105">實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (裝載啟動) 會在啟動時，從外部組件將增強功能新增至應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="2bdf6-106">例如，外部程式庫可以使用裝載啟動實作，向應用程式提供額外的組態提供者或服務。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="2bdf6-107">ASP.NET Core 2.0 或更新版本提供 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-107">`IHostingStartup` *is available in ASP.NET Core 2.0 or later.*</span></span>

<span data-ttu-id="2bdf6-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2bdf6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="2bdf6-109">HostingStartup 屬性</span><span class="sxs-lookup"><span data-stu-id="2bdf6-109">HostingStartup attribute</span></span>

<span data-ttu-id="2bdf6-110">[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性指出裝載啟動組件的出現，於執行階段啟動。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-110">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="2bdf6-111">系統會自動掃描包含 `Startup` 類別的進入組件或組件是否有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-111">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="2bdf6-112">搜尋 `HostingStartup` 屬性的組件清單會在執行階段從 [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) 中的組態載入。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-112">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="2bdf6-113">要從探索中排除的組件清單會從 [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey) 載入。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-113">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="2bdf6-114">如需詳細資訊，請參閱 [Web 主機：裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)和 [Web 主機：裝載啟動排除組件](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-114">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="2bdf6-115">在下列範例中，裝載啟動組件的命名空間為 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-115">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="2bdf6-116">包含裝載啟動程式碼的類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-116">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="2bdf6-117">`HostingStartup` 屬性通常位於裝載啟動組件的 `IHostingStartup` 實作類別檔案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-117">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="2bdf6-118">探索載入的裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-118">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="2bdf6-119">若要探索載入的裝載啟動組件，請啟用記錄並檢查應用程式的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-119">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="2bdf6-120">系統會記錄載入組件時發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-120">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="2bdf6-121">將會在偵錯層級記錄載入的裝載啟動組件，並記錄所有錯誤。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-121">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="2bdf6-122">停用自動載入裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-122">Disable automatic loading of hosting startup assemblies</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="2bdf6-123">若要停用自動載入裝載啟動組件，請使用下列任一方法：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-123">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="2bdf6-124">若要防止載入所有裝載啟動組件，請將下列任一項目設為 `true` 或 `1`：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-124">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="2bdf6-125">[防止裝載啟動](xref:fundamentals/host/web-host#prevent-hosting-startup)主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-125">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="2bdf6-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-126">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="2bdf6-127">若要防止載入特定的裝載啟動組件，請將下列任一項目設為以分號分隔的裝載啟動組件字串，藉此於啟動時排除：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-127">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="2bdf6-128">[裝載啟動排除組件](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies)主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-128">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="2bdf6-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-129">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="2bdf6-130">若要停用自動載入裝載啟動組件，請將下列任一項目設為 `true` 或 `1`：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-130">To disable automatic loading of hosting startup assemblies, set one of the following to `true` or `1`:</span></span>

* <span data-ttu-id="2bdf6-131">[防止裝載啟動](xref:fundamentals/host/web-host#prevent-hosting-startup)主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-131">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="2bdf6-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-132">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

::: moniker-end

<span data-ttu-id="2bdf6-133">如已設定主機組態設定和環境變數，主機設定即會控制行為。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-133">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="2bdf6-134">使用主機設定或環境變數停用裝載啟動組件會停用全域的組件，並且可能會停用數項應用程式特性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-134">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="2bdf6-135">專案</span><span class="sxs-lookup"><span data-stu-id="2bdf6-135">Project</span></span>

<span data-ttu-id="2bdf6-136">以下列兩種專案類型的任一類型建立裝載啟動：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-136">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="2bdf6-137">類別庫</span><span class="sxs-lookup"><span data-stu-id="2bdf6-137">Class library</span></span>](#class-library)
* [<span data-ttu-id="2bdf6-138">沒有進入點的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="2bdf6-138">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="2bdf6-139">類別庫</span><span class="sxs-lookup"><span data-stu-id="2bdf6-139">Class library</span></span>

<span data-ttu-id="2bdf6-140">類別庫可以提供裝載啟動的增強功能。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-140">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="2bdf6-141">此程式庫包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-141">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="2bdf6-142">[程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)包含 Razor Pages 應用程式 *HostingStartupApp* 和類別庫 *HostingStartupLibrary*。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-142">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="2bdf6-143">類別庫：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-143">The class library:</span></span>

* <span data-ttu-id="2bdf6-144">包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-144">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="2bdf6-145">`ServiceKeyInjection` 使用記憶體內部組態提供者 ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection))，將成對的服務字串新增至應用程式的組態。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-145">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="2bdf6-146">包含 `HostingStartup` 屬性，可識別裝載啟動的命名空間和類別。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-146">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="2bdf6-147">`ServiceKeyInjection` 類別的 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)，將增強功能新增至應用程式中。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-147">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="2bdf6-148">執行階段會先呼叫裝載啟動組件中的 `IHostingStartup.Configure`，再呼叫使用者程式碼中的 `Startup.Configure`，這可讓使用者程式碼覆寫裝載啟動組件提供的所有組態。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-148">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

<span data-ttu-id="2bdf6-149">*HostingStartupLibrary/ServiceKeyInjection.cs*：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-149">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="2bdf6-150">應用程式的 [索引] 頁面會讀取並呈現類別庫裝載啟動組件所設定之兩個索引鍵的組態值：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-150">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="2bdf6-151">*HostingStartupApp/Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-151">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="2bdf6-152">[程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/)也包含提供另一個主機啟動 (*HostingStartupPackage*) 的 NuGet 套件專案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-152">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="2bdf6-153">此套件特性與前文所述之類別庫相同。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-153">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="2bdf6-154">套件：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-154">The package:</span></span>

* <span data-ttu-id="2bdf6-155">包含主機啟動類別 `ServiceKeyInjection`，其會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-155">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="2bdf6-156">`ServiceKeyInjection` 將成對的服務字串新增到應用程式組態。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-156">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="2bdf6-157">包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-157">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="2bdf6-158">*HostingStartupPackage/ServiceKeyInjection.cs*：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-158">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="2bdf6-159">應用程式的 [索引] 頁面會讀取並呈現套件裝載啟動組件所設定之兩個索引鍵的組態值：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-159">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="2bdf6-160">*HostingStartupApp/Pages/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-160">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="2bdf6-161">沒有進入點的主控台應用程式</span><span class="sxs-lookup"><span data-stu-id="2bdf6-161">Console app without an entry point</span></span>

<span data-ttu-id="2bdf6-162">這個方法僅適用於 .NET Core 應用程式，不適用於 .NET Framework。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-162">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="2bdf6-163">在沒有進入點的主控台應用程式中，可以提供不需要啟用編譯時間參考的動態裝載啟動增強功能。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-163">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point.</span></span> <span data-ttu-id="2bdf6-164">此應用程式包含 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-164">The app contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="2bdf6-165">建立動態裝載啟動：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-165">To create a dynamic hosting startup:</span></span>

1. <span data-ttu-id="2bdf6-166">實作程式庫是從包含 `IHostingStartup` 實作的類別建立。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-166">An implementation library is created from the class that contains the `IHostingStartup` implementation.</span></span> <span data-ttu-id="2bdf6-167">實作程式庫可視為一般套件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-167">The implementation library is treated as a normal package.</span></span>
1. <span data-ttu-id="2bdf6-168">沒有進入點的主控台應用程式會參考實作程式庫套件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-168">A console app without an entry point references the implementation library package.</span></span> <span data-ttu-id="2bdf6-169">使用主控台應用程式的原因：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-169">A console app is used because:</span></span>
   * <span data-ttu-id="2bdf6-170">相依性檔案是可執行的應用程式資產，所以程式庫無法提供相依性檔案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-170">A dependencies file is a runnable app asset, so a library can't furnish a dependencies file.</span></span>
   * <span data-ttu-id="2bdf6-171">程式庫無法直接新增至[執行階段套件存放區](/dotnet/core/deploying/runtime-store)，這需要以共用執行階段為目標的可執行專案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-171">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>
1. <span data-ttu-id="2bdf6-172">發佈主控台應用程式以取得裝載啟動的相依性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-172">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="2bdf6-173">發佈主控台應用程式的結果是從相依性檔案中刪減未使用的相依性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-173">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
1. <span data-ttu-id="2bdf6-174">應用程式及其相依性檔案會放入執行階段套件存放區。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-174">The app and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="2bdf6-175">若要探索裝載啟動組件及其相依性檔案，兩者會在成對的環境變數中參考。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-175">To discover the hosting startup assembly and its dependencies file, they're referenced in a pair of environment variables.</span></span>

<span data-ttu-id="2bdf6-176">主控台應用程式會參考 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) 套件：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-176">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="2bdf6-177">建置 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 時，[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性會將類別識別為用於載入和執行的 `IHostingStartup` 實作。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-177">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="2bdf6-178">在下列範例中，命名空間為 `StartupEnhancement`，而類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-178">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="2bdf6-179">類別會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-179">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="2bdf6-180">此類別的 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 將增強功能新增至應用程式中。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-180">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="2bdf6-181">執行階段會先呼叫裝載啟動組件中的 `IHostingStartup.Configure`，再呼叫使用者程式碼中的 `Startup.Configure`，這可讓使用者程式碼覆寫裝載啟動組件提供的所有組態。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-181">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="2bdf6-182">建置 `IHostingStartup` 專案時，相依性檔案 (*\*.deps.json*) 會將組件的 `runtime` 位置設定為 *bin* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-182">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="2bdf6-183">僅顯示部分檔案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-183">Only part of the file is shown.</span></span> <span data-ttu-id="2bdf6-184">範例中的組件名稱是 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-184">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="2bdf6-185">指定裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-185">Specify the hosting startup assembly</span></span>

<span data-ttu-id="2bdf6-186">為類別庫 (或主控台應用程式) 提供的裝載啟動，在 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數中指定裝載啟動組件的名稱。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-186">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="2bdf6-187">環境變數是以分號分隔的組件清單。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-187">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="2bdf6-188">只掃描裝載啟動組件是否有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-188">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="2bdf6-189">範例應用程式 *HostingStartupApp* 若要探索前文所述的裝載啟動，環境變數要設定為下列值：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-189">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="2bdf6-190">您也可以使用[裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)的主機組態設定來設定裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-190">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="2bdf6-191">當有多個裝載啟動組合存在時，其 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法會依組件的列示順序執行。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-191">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="2bdf6-192">啟用</span><span class="sxs-lookup"><span data-stu-id="2bdf6-192">Activation</span></span>

<span data-ttu-id="2bdf6-193">裝載啟動啟用的選項如下：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-193">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="2bdf6-194">[執行階段存放區](#runtime-store) &ndash; 啟用不需要啟用的編譯時間參考。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-194">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="2bdf6-195">範例應用程式會將裝載啟動組件和相依性檔案放入 *deployment* 資料夾，以利在多機器環境中部署裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-195">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="2bdf6-196">*deployment* 資料夾也包含 PowerShell 指令碼，可在部署系統上建立或修改環境變數，以啟用裝載啟動。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-196">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="2bdf6-197">啟用所需之編譯時間參考</span><span class="sxs-lookup"><span data-stu-id="2bdf6-197">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="2bdf6-198">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-198">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="2bdf6-199">專案 bin 資料夾</span><span class="sxs-lookup"><span data-stu-id="2bdf6-199">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="2bdf6-200">執行階段存放區</span><span class="sxs-lookup"><span data-stu-id="2bdf6-200">Runtime store</span></span>

<span data-ttu-id="2bdf6-201">裝載啟動實作置於[執行階段存放區](/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-201">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="2bdf6-202">增強的應用程式不需要組件的編譯時間參考。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-202">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="2bdf6-203">建置裝載啟動之後，裝載啟動的專案檔可作為 [dotnet store](/dotnet/core/tools/dotnet-store) 命令的資訊清單檔。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-203">After the hosting startup is built, the hosting startup's project file serves as the manifest file for the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest <PROJECT_FILE> --runtime <RUNTIME_IDENTIFIER>
```

<span data-ttu-id="2bdf6-204">此命令會將裝載啟動組件和不屬於共用架構的其他相依性放在使用者設定檔的執行階段存放區中，分別為：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-204">This command places the hosting startup assembly and other dependencies that aren't part of the shared framework in the user profile's runtime store at:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-205">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-205">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-206">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-206">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-207">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-207">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="2bdf6-208">如果您想要放置組件和相依性供全域使用，請將 `-o|--output` 選項新增至 `dotnet store` 命令，加上下列路徑：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-208">If you desire to place the assembly and dependencies for global use, add the `-o|--output` option to the `dotnet store` command with the following path:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-209">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-209">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-210">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-210">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-211">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-211">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/store/x64/<TARGET_FRAMEWORK_MONIKER>/<ENHANCEMENT_ASSEMBLY_NAME>/<ENHANCEMENT_VERSION>/lib/<TARGET_FRAMEWORK_MONIKER>/
```

---

<span data-ttu-id="2bdf6-212">**修改並放置裝載啟動的相依性檔案**</span><span class="sxs-lookup"><span data-stu-id="2bdf6-212">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="2bdf6-213">執行階段位置是在 *\*.deps.json* 檔案中指定。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-213">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="2bdf6-214">若要啟用增強功能，`runtime` 元素必須指定增強功能之執行階段組件的位置。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-214">To activate the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="2bdf6-215">請在 `runtime` 位置前面加上 `lib/<TARGET_FRAMEWORK_MONIKER>/`：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-215">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="2bdf6-216">在程式碼範例中 (*StartupDiagnostics* 專案)，*\*.deps.json* 檔案的修改是由 [PowerShell](/powershell/scripting/powershell-scripting) 指令碼執行。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-216">In the sample code (*StartupDiagnostics* project), modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="2bdf6-217">PowerShell 指令碼則是由專案檔中的建置目標自動觸發。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-217">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

<span data-ttu-id="2bdf6-218">此實作的 *\*.deps.json* 檔案必須位於可存取的位置。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-218">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="2bdf6-219">如果是供個別使用者使用，請將檔案置於使用者設定檔之 `.dotnet` 設定的 *additonalDeps* 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-219">For per-user use, place the file in the *additonalDeps* folder of the user profile's `.dotnet` settings:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-220">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-220">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-221">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-221">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-222">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-222">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="2bdf6-223">如果是供全域使用，請將檔案置於 .NET Core 安裝的 *additonalDeps* 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-223">For global use, place the file in the *additonalDeps* folder of the .NET Core installation:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-224">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-224">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-225">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-225">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-226">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-226">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/
```

---

<span data-ttu-id="2bdf6-227">共用的 Framework 版本反映了目標應用程式所使用的共用執行階段版本。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-227">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="2bdf6-228">*\*.runtimeconfig.json* 檔案會顯示共用的執行階段。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-228">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="2bdf6-229">在範例應用程式中 (*HostingStartupApp*)，是在 *HostingStartupApp.runtimeconfig.json* 檔案中指定共用的執行階段。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-229">In the sample app (*HostingStartupApp*), the shared runtime is specified in the *HostingStartupApp.runtimeconfig.json* file.</span></span>

<span data-ttu-id="2bdf6-230">**列出裝載啟動的相依性檔案**</span><span class="sxs-lookup"><span data-stu-id="2bdf6-230">**List the hosting startup's dependencies file**</span></span>

<span data-ttu-id="2bdf6-231">*\*.deps.json* 檔案的實作位置會列在 `DOTNET_ADDITIONAL_DEPS` 環境變數中。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-231">The location of the implementation's *\*.deps.json* file is listed in the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="2bdf6-232">如果檔案放在使用者設定檔的 *.dotnet* 資料夾中，請將環境變數值設定為：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-232">If the file is placed in the user profile's *.dotnet* folder, set the environment variable's value to:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-233">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-233">Windows</span></span>](#tab/windows)

```
%USERPROFILE%\.dotnet\x64\additionalDeps\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-234">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-234">macOS</span></span>](#tab/macos)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-235">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-235">Linux</span></span>](#tab/linux)

```
/Users/<USER>/.dotnet/x64/additionalDeps/
```

---

<span data-ttu-id="2bdf6-236">如果針對全域使用將檔案置於 .NET Core 安裝，請提供檔案的完整路徑：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-236">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-237">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-237">Windows</span></span>](#tab/windows)

```
%PROGRAMFILES%\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-238">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-238">macOS</span></span>](#tab/macos)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-239">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-239">Linux</span></span>](#tab/linux)

```
/usr/local/share/dotnet/additionalDeps/<ENHANCEMENT_ASSEMBLY_NAME>/shared/Microsoft.NETCore.App/<SHARED_FRAMEWORK_VERSION>/<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

---

<span data-ttu-id="2bdf6-240">為使範例應用程式 (*HostingStartupApp*) 尋找相依性檔案 (*HostingStartupApp.runtimeconfig.json*)，相依性檔案要放在使用者的設定檔中。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-240">For the sample app (*HostingStartupApp*) to find the dependencies file (*HostingStartupApp.runtimeconfig.json*), the dependencies file is placed in the user's profile.</span></span>

# <a name="windowstabwindows"></a>[<span data-ttu-id="2bdf6-241">Windows</span><span class="sxs-lookup"><span data-stu-id="2bdf6-241">Windows</span></span>](#tab/windows)

<span data-ttu-id="2bdf6-242">請將 `DOTNET_ADDITIONAL_DEPS` 環境變數設為下列值：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-242">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

# <a name="macostabmacos"></a>[<span data-ttu-id="2bdf6-243">macOS</span><span class="sxs-lookup"><span data-stu-id="2bdf6-243">macOS</span></span>](#tab/macos)

<span data-ttu-id="2bdf6-244">請將 `DOTNET_ADDITIONAL_DEPS` 環境變數設為下列值：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-244">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

# <a name="linuxtablinux"></a>[<span data-ttu-id="2bdf6-245">Linux</span><span class="sxs-lookup"><span data-stu-id="2bdf6-245">Linux</span></span>](#tab/linux)

<span data-ttu-id="2bdf6-246">請將 `DOTNET_ADDITIONAL_DEPS` 環境變數設為下列值：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-246">Set the `DOTNET_ADDITIONAL_DEPS` environment variable to the following value:</span></span>

```
/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/
```

---

<span data-ttu-id="2bdf6-247">如需如何為各種作業系統設定環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-247">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="2bdf6-248">**部署**</span><span class="sxs-lookup"><span data-stu-id="2bdf6-248">**Deployment**</span></span>

<span data-ttu-id="2bdf6-249">為利於在多機器環境中部署裝載啟動，範例應用程式會在包含下列項目的發佈輸出中建立 *deployment* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-249">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="2bdf6-250">裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-250">The hosting startup assembly.</span></span>
* <span data-ttu-id="2bdf6-251">裝載啟動相依性檔案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-251">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="2bdf6-252">建立或修改 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 和 `DOTNET_ADDITIONAL_DEPS` 以支援啟用裝載啟動的 PowerShell 指令碼。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-252">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="2bdf6-253">在部署系統上，從系統管理 PowerShell 命令提示字元執行指令碼。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-253">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="2bdf6-254">NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-254">NuGet package</span></span>

<span data-ttu-id="2bdf6-255">NuGet 套件可以提供裝載啟動的增強功能。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-255">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="2bdf6-256">此套件具有 `HostingStartup` 屬性。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-256">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="2bdf6-257">使用下列兩種方法的任一方法，套件提供的裝載啟動類型即可提供應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-257">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="2bdf6-258">增強應用程式的專案檔會在應用程式專案檔 (編譯時間參考) 中建立裝載啟動的套件參考。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-258">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="2bdf6-259">當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*\*.deps.json*)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-259">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="2bdf6-260">這種方法適用於發佈至 [nuget.org](https://www.nuget.org/) 的裝載啟動組件套件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-260">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="2bdf6-261">裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-261">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="2bdf6-262">如需有關 NuGet 套件和執行階段存放區的詳細資訊，請參閱下列主題：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-262">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="2bdf6-263">如何使用跨平台工具建立 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-263">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="2bdf6-264">發佈套件</span><span class="sxs-lookup"><span data-stu-id="2bdf6-264">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="2bdf6-265">執行階段套件存放區</span><span class="sxs-lookup"><span data-stu-id="2bdf6-265">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="2bdf6-266">專案 bin 資料夾</span><span class="sxs-lookup"><span data-stu-id="2bdf6-266">Project bin folder</span></span>

<span data-ttu-id="2bdf6-267">部署在增強應用程式 *bin* 下的組件可提供裝載啟動增強功能。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-267">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="2bdf6-268">使用下列兩種方法的任一方法，組件提供的裝載啟動類型即可提供應用程式使用：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-268">The hosting startup types provided by the assembly are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="2bdf6-269">增強應用程式的專案檔會建立裝載啟動的組件參考 (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-269">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="2bdf6-270">當編譯時間參考就定位時，裝載啟動組件及其所有相依性都會併入應用程式的相依性檔案 (*\*.deps.json*)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-270">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*\*.deps.json*).</span></span> <span data-ttu-id="2bdf6-271">當部署案例要求將已編譯裝載啟動程式庫的組件 (DLL 檔案) 移至取用專案或取用專案可存取的位置，且已為裝載啟動組件建立編譯時間參考時，即適用這種方法。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-271">This approach applies when the deployment scenario calls for moving the compiled hosting startup library's assembly (DLL file) to the consuming project or to a location accessible by the consuming project and a compile-time reference is made to the hosting startup's assembly.</span></span>
* <span data-ttu-id="2bdf6-272">裝載啟動的相依性檔案可提供增強應用程式使用，如[執行階段存放區](#runtime-store) 區段 (沒有編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-272">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

## <a name="sample-code"></a><span data-ttu-id="2bdf6-273">範例程式碼</span><span class="sxs-lookup"><span data-stu-id="2bdf6-273">Sample code</span></span>

<span data-ttu-id="2bdf6-274">[程式碼範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([如何下載](xref:index#how-to-download-a-sample)) 示範裝載啟動實作案例：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-274">The [sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="2bdf6-275">兩個裝載啟動組件 (類別程式庫) 各設定一對記憶體內部組態索引鍵/值組：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-275">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="2bdf6-276">NuGet 套件 (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="2bdf6-276">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="2bdf6-277">類別庫 (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="2bdf6-277">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="2bdf6-278">從部署在執行階段存放區的組件啟動裝載啟動 (*StartupDiagnostics*)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-278">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="2bdf6-279">此組件會在啟動時將兩個中介軟體新增至應用程式，以提供診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-279">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="2bdf6-280">已註冊服務</span><span class="sxs-lookup"><span data-stu-id="2bdf6-280">Registered services</span></span>
  * <span data-ttu-id="2bdf6-281">位址 (配置、主機、基底路徑、路徑、查詢字串)</span><span class="sxs-lookup"><span data-stu-id="2bdf6-281">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="2bdf6-282">連線 (遠端 IP、遠端連接埠、本機 IP、本機連接埠、用戶端憑證)</span><span class="sxs-lookup"><span data-stu-id="2bdf6-282">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="2bdf6-283">要求標頭</span><span class="sxs-lookup"><span data-stu-id="2bdf6-283">Request headers</span></span>
  * <span data-ttu-id="2bdf6-284">環境變數</span><span class="sxs-lookup"><span data-stu-id="2bdf6-284">Environment variables</span></span>

<span data-ttu-id="2bdf6-285">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-285">To run the sample:</span></span>

<span data-ttu-id="2bdf6-286">**從 NuGet 套件啟用**</span><span class="sxs-lookup"><span data-stu-id="2bdf6-286">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="2bdf6-287">使用 [dotnet pack](/dotnet/core/tools/dotnet-pack) 命令編譯 *HostingStartupPackage* 套件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-287">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="2bdf6-288">將 *HostingStartupPackage* 的套件組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-288">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="2bdf6-289">編譯並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-289">Compile and run the app.</span></span> <span data-ttu-id="2bdf6-290">增強的應用程式中有套件參考 (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-290">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="2bdf6-291">應用程式專案檔中的 `<PropertyGroup>` 會指定套件專案的輸出 (*../HostingStartupPackage/bin/Debug*) 為套件來源。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-291">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="2bdf6-292">這可讓應用程式使用套件，卻不用將套件上傳至 [nuget.org](https://www.nuget.org/)。如需詳細資訊，請參閱 HostingStartupApp 專案檔中的資訊。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-292">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```
1. <span data-ttu-id="2bdf6-293">觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合套件之 `ServiceKeyInjection.Configure` 方法設定的值。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-293">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="2bdf6-294">如果您變更並重新編譯 *HostingStartupPackage* 專案，請清除本機的 NuGet 套件快取，以確保 *HostingStartupApp* 從本機快取接收更新的套件，而非過時的套件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-294">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="2bdf6-295">若要清除本機 NuGet 快取，請執行下列 [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) 命令：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-295">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="2bdf6-296">**從類別庫啟用**</span><span class="sxs-lookup"><span data-stu-id="2bdf6-296">**Activation from a class library**</span></span>

1. <span data-ttu-id="2bdf6-297">使用 [dotnet build](/dotnet/core/tools/dotnet-build) 命令編譯 *HostingStartupLibrary* 類別庫。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-297">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="2bdf6-298">將 *HostingStartupLibrary* 的類別庫組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-298">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="2bdf6-299">將 *HostingStartupLibrary.dll* 檔案從類別庫的編譯輸出複製到應用程式的 *bin/Debug* 資料夾，在應用程式的 *bin* 下部署類別庫組件。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-299">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="2bdf6-300">編譯並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-300">Compile and run the app.</span></span> <span data-ttu-id="2bdf6-301">應用程式專案檔中的 `<ItemGroup>` 參考類別庫的組件 (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (編譯時間參考)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-301">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="2bdf6-302">如需詳細資訊，請參閱 HostingStartupApp 專案檔中的資訊。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-302">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```
1. <span data-ttu-id="2bdf6-303">觀察 [索引] 頁面所呈現的服務組態索引鍵值是否符合類別庫之 `ServiceKeyInjection.Configure` 方法所設定的值。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-303">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="2bdf6-304">**從部署在執行階段存放區的組件啟用**</span><span class="sxs-lookup"><span data-stu-id="2bdf6-304">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="2bdf6-305">*StartupDiagnostics* 專案使用 [PowerShell](/powershell/scripting/powershell-scripting) 修改其 *StartupDiagnostics.deps.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-305">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="2bdf6-306">從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 開始，會在 Windows 上預設安裝 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-306">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="2bdf6-307">若要在其他平台上取得 PowerShell，請參閱[安裝 Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-307">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="2bdf6-308">建置 *StartupDiagnostics* 專案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-308">Build the *StartupDiagnostics* project.</span></span> <span data-ttu-id="2bdf6-309">專案建置後，即會自動在專案檔中建置目標：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-309">After the project is built, a build target in the project file automatically:</span></span>
   * <span data-ttu-id="2bdf6-310">觸發 PowerShell 指令碼來修改 *StartupDiagnostics.deps.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-310">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="2bdf6-311">將 *StartupDiagnostics.deps.json* 檔案移至使用者設定檔的 *additionalDeps* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-311">Moves the *StartupDiagnostics.deps.json* file to the user profile's *additionalDeps* folder.</span></span>
1. <span data-ttu-id="2bdf6-312">在裝載啟動目錄中的命令提示字元執行 `dotnet store` 命令，將組件及其相依性儲存在使用者設定檔的執行階段存放區：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-312">Execute the `dotnet store` command at a command prompt in the hosting startup's directory to store the assembly and its dependencies in the user profile's runtime store:</span></span>

   ```console
   dotnet store --manifest StartupDiagnostics.csproj --runtime <RID>
   ```
   <span data-ttu-id="2bdf6-313">在 Windows，此命令會使用 `win7-x64` [執行階段識別碼 (RID)](/dotnet/core/rid-catalog)。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-313">For Windows, the command uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog).</span></span> <span data-ttu-id="2bdf6-314">為不同的執行階段提供裝載啟動時，請替換成正確的 RID。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-314">When providing the hosting startup for a different runtime, substitute the correct RID.</span></span>
1. <span data-ttu-id="2bdf6-315">設定環境變數：</span><span class="sxs-lookup"><span data-stu-id="2bdf6-315">Set the environment variables:</span></span>
   * <span data-ttu-id="2bdf6-316">將 *StartupDiagnostics* 的組件名稱新增至 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-316">Add the assembly name of *StartupDiagnostics* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="2bdf6-317">在 Windows 中，將 `DOTNET_ADDITIONAL_DEPS` 環境變數設為 `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-317">On Windows, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`.</span></span> <span data-ttu-id="2bdf6-318">在 macOS/Linux 中，將 `DOTNET_ADDITIONAL_DEPS` 環境變數設為 `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`，其中 `<USER>` 是包含裝載啟動的使用者設定檔。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-318">On macOS/Linux, set the `DOTNET_ADDITIONAL_DEPS` environment variable to `/Users/<USER>/.dotnet/x64/additionalDeps/StartupDiagnostics/`, where `<USER>` is the user profile that contains the hosting startup.</span></span>
1. <span data-ttu-id="2bdf6-319">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-319">Run the sample app.</span></span>
1. <span data-ttu-id="2bdf6-320">要求 `/services` 端點來查看應用程式的註冊服務。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-320">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="2bdf6-321">要求 `/diag` 端點來查看診斷資訊。</span><span class="sxs-lookup"><span data-stu-id="2bdf6-321">Request the `/diag` endpoint to see the diagnostic information.</span></span>
