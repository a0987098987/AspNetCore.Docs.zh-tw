---
title: 在 ASP.NET Core 中使用 IHostingStartup 從外部組件增強應用程式
author: guardrex
description: 了解如何使用 IHostingStartup 實作，從外部組件增強 ASP.NET Core 應用程式。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: bd9605dd8efee2c3ba8bc82a81554cace40630be
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278079"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="18344-103">在 ASP.NET Core 中使用 IHostingStartup 從外部組件增強應用程式</span><span class="sxs-lookup"><span data-stu-id="18344-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="18344-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="18344-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="18344-105">實作 [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) 即可在啟動時，從應用程式非 `Startup` 類別的外部組件，對應用程式新增強功能。</span><span class="sxs-lookup"><span data-stu-id="18344-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="18344-106">例如，外部工具程式庫可以使用 `IHostingStartup` 實作，提供額外的組態提供者或服務給應用程式。</span><span class="sxs-lookup"><span data-stu-id="18344-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="18344-107">ASP.NET Core 2.0 和更新版本提供了 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="18344-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="18344-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="18344-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="18344-109">探索載入的裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="18344-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="18344-110">若要探索應用程式或程式庫所載入的裝載啟動組件，請啟用記錄並檢查應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="18344-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="18344-111">系統會記錄載入組件時發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="18344-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="18344-112">將會在偵錯層級記錄載入的裝載啟動組件，並記錄所有錯誤。</span><span class="sxs-lookup"><span data-stu-id="18344-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="18344-113">範例應用程式會將 [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) 讀取到 `string` 陣列，並在應用程式的 [索引] 頁面中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="18344-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="18344-114">停用自動載入裝載啟動組件</span><span class="sxs-lookup"><span data-stu-id="18344-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="18344-115">有兩種方式可以停用自動載入裝載啟動組件：</span><span class="sxs-lookup"><span data-stu-id="18344-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="18344-116">設定[防止裝載啟動](xref:fundamentals/host/web-host#prevent-hosting-startup)主機組態設定。</span><span class="sxs-lookup"><span data-stu-id="18344-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="18344-117">設定 `ASPNETCORE_PREVENTHOSTINGSTARTUP` 環境變數。</span><span class="sxs-lookup"><span data-stu-id="18344-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="18344-118">當主機設定或環境變數設定為 `true` 或 `1` 時，不會自動載入裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="18344-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="18344-119">如果這兩者皆已設定，則主機設定會控制該行為。</span><span class="sxs-lookup"><span data-stu-id="18344-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="18344-120">使用主機設定或環境變數停用裝載啟動組件會在全域停用它們，並且可能會停用數項應用程式特性。</span><span class="sxs-lookup"><span data-stu-id="18344-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="18344-121">除非程式庫提供自己的組態選項，否則目前不能選擇性地停用程式庫所新增的裝載啟動組件。</span><span class="sxs-lookup"><span data-stu-id="18344-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="18344-122">未來版本會提供選擇性地停用裝載啟動組件的功能 (請參閱 [GitHub 問題 aspnet/裝載 # 1243](https://github.com/aspnet/Hosting/pull/1243) (英文))。</span><span class="sxs-lookup"><span data-stu-id="18344-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="18344-123">實作 IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="18344-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="18344-124">建立組件</span><span class="sxs-lookup"><span data-stu-id="18344-124">Create the assembly</span></span>

<span data-ttu-id="18344-125">`IHostingStartup` 增強功能會根據主控台應用程式部署為組件，而無需進入點。</span><span class="sxs-lookup"><span data-stu-id="18344-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="18344-126">此組件會參考 [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) 套件：</span><span class="sxs-lookup"><span data-stu-id="18344-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="18344-127">建置 [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost) 時，[HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) 屬性會將類別識別為用於載入和執行的 `IHostingStartup` 實作。</span><span class="sxs-lookup"><span data-stu-id="18344-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="18344-128">在下列範例中，命名空間為 `StartupEnhancement`，而類別為 `StartupEnhancementHostingStartup`：</span><span class="sxs-lookup"><span data-stu-id="18344-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="18344-129">類別會實作 `IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="18344-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="18344-130">此類別的 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法使用 [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) 將增強功能新增至應用程式中。</span><span class="sxs-lookup"><span data-stu-id="18344-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="18344-131">執行階段會先呼叫裝載啟動組件中的 `IHostingStartup.Configure`，再呼叫使用者程式碼中的 `Startup.Configure`，這可讓使用者程式碼覆寫裝載啟動組件提供的所有組態。</span><span class="sxs-lookup"><span data-stu-id="18344-131">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configruation provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="18344-132">建置 `IHostingStartup` 專案時，相依性檔案 (*\*.deps.json*) 會將組件的 `runtime` 位置設定為 *bin* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="18344-132">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="18344-133">僅顯示部分檔案。</span><span class="sxs-lookup"><span data-stu-id="18344-133">Only part of the file is shown.</span></span> <span data-ttu-id="18344-134">範例中的組件名稱是 `StartupEnhancement`。</span><span class="sxs-lookup"><span data-stu-id="18344-134">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="18344-135">更新相依性檔案</span><span class="sxs-lookup"><span data-stu-id="18344-135">Update the dependencies file</span></span>

<span data-ttu-id="18344-136">執行階段位置是在 *\*.deps.json* 檔案中指定。</span><span class="sxs-lookup"><span data-stu-id="18344-136">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="18344-137">若要啟用增強功能，`runtime` 元素必須指定增強功能之執行階段組件的位置。</span><span class="sxs-lookup"><span data-stu-id="18344-137">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="18344-138">請在 `runtime` 位置前面加上 `lib/<TARGET_FRAMEWORK_MONIKER>/`：</span><span class="sxs-lookup"><span data-stu-id="18344-138">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="18344-139">在範例應用程式中，*\*.deps.json* 檔案的修改是由 [PowerShell](/powershell/scripting/powershell-scripting) 指令碼所執行。</span><span class="sxs-lookup"><span data-stu-id="18344-139">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="18344-140">PowerShell 指令碼則是由專案檔中的建置目標自動觸發。</span><span class="sxs-lookup"><span data-stu-id="18344-140">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="18344-141">啟動增強功能</span><span class="sxs-lookup"><span data-stu-id="18344-141">Enhancement activation</span></span>

<span data-ttu-id="18344-142">**放置組件檔**</span><span class="sxs-lookup"><span data-stu-id="18344-142">**Place the assembly file**</span></span>

<span data-ttu-id="18344-143">`IHostingStartup` 實作的組件檔案必須部署在應用程式的 *bin* 中，或置於[執行階段存放區](/dotnet/core/deploying/runtime-store)：</span><span class="sxs-lookup"><span data-stu-id="18344-143">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="18344-144">如果是個別使用者使用，請將組件置於使用者設定檔的執行階段存放區中：</span><span class="sxs-lookup"><span data-stu-id="18344-144">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="18344-145">如果是全域使用，請將組件置於 .NET Core 安裝的執行階段存放區中：</span><span class="sxs-lookup"><span data-stu-id="18344-145">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="18344-146">將組件部署至執行階段存放區時，可能也會部署符號檔，但運作增強功能並不需要該檔案。</span><span class="sxs-lookup"><span data-stu-id="18344-146">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="18344-147">**放置相依性檔案**</span><span class="sxs-lookup"><span data-stu-id="18344-147">**Place the dependencies file**</span></span>

<span data-ttu-id="18344-148">此實作的 *\*.deps.json* 檔案必須位於可存取的位置。</span><span class="sxs-lookup"><span data-stu-id="18344-148">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="18344-149">如果是個別使用者使用，請將檔案置於使用者設定檔之 `.dotnet` 設定的 `additonalDeps` 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="18344-149">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="18344-150">如果是全域使用，請將檔案置於 .NET Core 安裝的 `additonalDeps` 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="18344-150">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="18344-151">共用的 Framework 版本反映了目標應用程式所使用的共用執行階段版本。</span><span class="sxs-lookup"><span data-stu-id="18344-151">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="18344-152">*\*.runtimeconfig.json* 檔案會顯示共用的執行階段。</span><span class="sxs-lookup"><span data-stu-id="18344-152">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="18344-153">在範例應用程式中，共用的執行階段則是在 *HostingStartupSample.runtimeconfig.json* 檔案中指定。</span><span class="sxs-lookup"><span data-stu-id="18344-153">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="18344-154">**設定環境變數**</span><span class="sxs-lookup"><span data-stu-id="18344-154">**Set environment variables**</span></span>

<span data-ttu-id="18344-155">在使用增強功能之應用程式的內容中設定下列環境變數。</span><span class="sxs-lookup"><span data-stu-id="18344-155">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="18344-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="18344-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="18344-157">只會掃描裝載啟動組件是否有 `HostingStartupAttribute`。</span><span class="sxs-lookup"><span data-stu-id="18344-157">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="18344-158">這個環境變數中提供了實作的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="18344-158">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="18344-159">範例應用程式會將此值設定為 `StartupDiagnostics`。</span><span class="sxs-lookup"><span data-stu-id="18344-159">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="18344-160">您也可以使用[裝載啟動組件](xref:fundamentals/host/web-host#hosting-startup-assemblies)的主機組態設定來設定此值。</span><span class="sxs-lookup"><span data-stu-id="18344-160">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="18344-161">當有多個裝載啟動組合存在時，其 [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) 方法會依組件的列示順序執行。</span><span class="sxs-lookup"><span data-stu-id="18344-161">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

<span data-ttu-id="18344-162">DOTNET_ADDITIONAL_DEPS</span><span class="sxs-lookup"><span data-stu-id="18344-162">DOTNET_ADDITIONAL_DEPS</span></span>

<span data-ttu-id="18344-163">實作的 *\*.deps.json* 檔案的位置。</span><span class="sxs-lookup"><span data-stu-id="18344-163">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="18344-164">如果針對個別使用者使用，將檔案置於使用者設定檔的 *.dotnet* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="18344-164">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="18344-165">如果針對全域使用將檔案置於 .NET Core 安裝，請提供檔案的完整路徑：</span><span class="sxs-lookup"><span data-stu-id="18344-165">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="18344-166">範例應用程式會將此值設定為：</span><span class="sxs-lookup"><span data-stu-id="18344-166">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="18344-167">如需如何為各種作業系統設定環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="18344-167">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="18344-168">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="18344-168">Sample app</span></span>

<span data-ttu-id="18344-169">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([如何下載](xref:tutorials/index#how-to-download-a-sample)) 會使用 `IHostingStartup` 來建立診斷工具。</span><span class="sxs-lookup"><span data-stu-id="18344-169">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="18344-170">此工具會在啟動時將兩個中介軟體新增至應用程式，以提供診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="18344-170">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="18344-171">已註冊服務</span><span class="sxs-lookup"><span data-stu-id="18344-171">Registered services</span></span>
* <span data-ttu-id="18344-172">位址：配置、主機、基底路徑、路徑、查詢字串</span><span class="sxs-lookup"><span data-stu-id="18344-172">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="18344-173">連線：遠端 IP、遠端連接埠、本機 IP、本機連接埠、用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="18344-173">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="18344-174">要求標頭</span><span class="sxs-lookup"><span data-stu-id="18344-174">Request headers</span></span>
* <span data-ttu-id="18344-175">環境變數</span><span class="sxs-lookup"><span data-stu-id="18344-175">Environment variables</span></span>

<span data-ttu-id="18344-176">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="18344-176">To run the sample:</span></span>

1. <span data-ttu-id="18344-177">啟動診斷專案使用 [PowerShell](/powershell/scripting/powershell-scripting) 來修改其 *StartupDiagnostics.deps.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18344-177">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="18344-178">從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 開始，Windows 作業系統上預設會安裝 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="18344-178">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="18344-179">若要在其他平台上取得 PowerShell，請參閱[安裝 Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell)。</span><span class="sxs-lookup"><span data-stu-id="18344-179">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="18344-180">建置啟動診斷專案。</span><span class="sxs-lookup"><span data-stu-id="18344-180">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="18344-181">專案檔中的建置目標會：</span><span class="sxs-lookup"><span data-stu-id="18344-181">A build target in the project file:</span></span>
   * <span data-ttu-id="18344-182">將組件檔和符號檔移至使用者設定檔的執行階段存放區。</span><span class="sxs-lookup"><span data-stu-id="18344-182">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="18344-183">觸發 PowerShell 指令碼來修改 *StartupDiagnostics.deps.json* 檔案。</span><span class="sxs-lookup"><span data-stu-id="18344-183">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="18344-184">將 *StartupDiagnostics.deps.json* 檔案移至使用者設定檔的 `additionalDeps` 資料夾。</span><span class="sxs-lookup"><span data-stu-id="18344-184">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="18344-185">設定環境變數：</span><span class="sxs-lookup"><span data-stu-id="18344-185">Set the environment variables:</span></span>
    * <span data-ttu-id="18344-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="18344-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="18344-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="18344-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="18344-188">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="18344-188">Run the sample app.</span></span>
5. <span data-ttu-id="18344-189">要求 `/services` 端點來查看應用程式的註冊服務。</span><span class="sxs-lookup"><span data-stu-id="18344-189">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="18344-190">要求 `/diag` 端點來查看診斷資訊。</span><span class="sxs-lookup"><span data-stu-id="18344-190">Request the `/diag` endpoint to see the diagnostic information.</span></span>
