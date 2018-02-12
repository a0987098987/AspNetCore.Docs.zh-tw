---
title: "使用 ASP.NET Core IHostingStartup 外部組件加入應用程式功能"
author: guardrex
description: "了解如何將功能加入至 ASP.NET Core 應用程式中，使用 IHostingStartup 實作的外部組件。"
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: e4b6293aff9fa39b70af40507a2cf5b7efcb295b
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/11/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="ffb65-103">使用 ASP.NET Core IHostingStartup 外部組件加入應用程式功能</span><span class="sxs-lookup"><span data-stu-id="ffb65-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="ffb65-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="ffb65-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ffb65-105">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup)實作可允許將功能加入應用程式在從外部應用程式的啟動`Startup`類別。</span><span class="sxs-lookup"><span data-stu-id="ffb65-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="ffb65-106">例如，可以使用外部工具程式庫`IHostingStartup`提供額外的組態提供者或服務應用程式的實作。</span><span class="sxs-lookup"><span data-stu-id="ffb65-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="ffb65-107">`IHostingStartup`*可在 ASP.NET Core 2.0 及更新版本。*</span><span class="sxs-lookup"><span data-stu-id="ffb65-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="ffb65-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ffb65-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="ffb65-109">探索載入裝載的啟動組件</span><span class="sxs-lookup"><span data-stu-id="ffb65-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="ffb65-110">若要探索裝載啟動的組件載入應用程式或程式庫，請啟用記錄，並檢查應用程式記錄檔。</span><span class="sxs-lookup"><span data-stu-id="ffb65-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="ffb65-111">載入組件時，會發生的錯誤會記錄。</span><span class="sxs-lookup"><span data-stu-id="ffb65-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="ffb65-112">在偵錯層級，記錄載入裝載的啟動組件，並會記錄所有錯誤。</span><span class="sxs-lookup"><span data-stu-id="ffb65-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="ffb65-113">範例應用程式讀取[HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey)到`string`陣列，並在應用程式的索引頁面中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="ffb65-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="ffb65-114">停用自動載入裝載啟動的組件</span><span class="sxs-lookup"><span data-stu-id="ffb65-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="ffb65-115">有兩種方式可以停用自動載入裝載啟動的組件：</span><span class="sxs-lookup"><span data-stu-id="ffb65-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="ffb65-116">設定[防止裝載啟動](xref:fundamentals/hosting#prevent-hosting-startup)裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="ffb65-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="ffb65-117">設定`ASPNETCORE_preventHostingStartup`環境變數。</span><span class="sxs-lookup"><span data-stu-id="ffb65-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="ffb65-118">當主控件設定或環境變數設為`true`或`1`、 裝載啟動的組件不會自動載入。</span><span class="sxs-lookup"><span data-stu-id="ffb65-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="ffb65-119">如果同時設定，主機設定控制的行為。</span><span class="sxs-lookup"><span data-stu-id="ffb65-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="ffb65-120">停用裝載使用主機的設定或環境變數的啟動組件全域停用它們，並可能會停用應用程式的數個功能。</span><span class="sxs-lookup"><span data-stu-id="ffb65-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="ffb65-121">它不是目前可以選擇性地停用裝載的啟動組件，加入程式庫，除非程式庫提供它自己的組態選項。</span><span class="sxs-lookup"><span data-stu-id="ffb65-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="ffb65-122">未來的版本會提供選擇性地停用裝載的啟動組件的能力 (請參閱[GitHub 發出 aspnet/主控 # 1243年](https://github.com/aspnet/Hosting/pull/1243))。</span><span class="sxs-lookup"><span data-stu-id="ffb65-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="ffb65-123">實作 IHostingStartup 功能</span><span class="sxs-lookup"><span data-stu-id="ffb65-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="ffb65-124">建立組件</span><span class="sxs-lookup"><span data-stu-id="ffb65-124">Create the assembly</span></span>

<span data-ttu-id="ffb65-125">`IHostingStartup`做為主控台應用程式沒有進入點為基礎的組件部署功能。</span><span class="sxs-lookup"><span data-stu-id="ffb65-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="ffb65-126">組件參考[Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/)封裝：</span><span class="sxs-lookup"><span data-stu-id="ffb65-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="ffb65-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute)屬性會識別類別的實作為`IHostingStartup`載入和執行時建置[IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost)。</span><span class="sxs-lookup"><span data-stu-id="ffb65-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="ffb65-128">在下列範例中，命名空間是`StartupFeature`，而類別是`StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="ffb65-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="ffb65-129">類別會實作`IHostingStartup`。</span><span class="sxs-lookup"><span data-stu-id="ffb65-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="ffb65-130">類別的[設定](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure)方法會使用[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)將功能加入至應用程式：</span><span class="sxs-lookup"><span data-stu-id="ffb65-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="ffb65-131">建置時`IHostingStartup`專案、 相依性檔案 (*\*。 deps.json*) 設定`runtime`之組件的位置*bin*資料夾：</span><span class="sxs-lookup"><span data-stu-id="ffb65-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ffb65-132">部分檔案會顯示。</span><span class="sxs-lookup"><span data-stu-id="ffb65-132">Only part of the file is shown.</span></span> <span data-ttu-id="ffb65-133">在範例中的組件名稱是`StartupFeature`。</span><span class="sxs-lookup"><span data-stu-id="ffb65-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="ffb65-134">更新相依檔案</span><span class="sxs-lookup"><span data-stu-id="ffb65-134">Update the dependencies file</span></span>

<span data-ttu-id="ffb65-135">已指定的執行階段位置 *\*。 deps.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="ffb65-136">為 作用中的功能，`runtime`元素必須指定功能的執行階段組件的位置。</span><span class="sxs-lookup"><span data-stu-id="ffb65-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="ffb65-137">前置詞`runtime`位置`lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="ffb65-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ffb65-138">範例應用程式修改 *\*。 deps.json*檔案由執行[PowerShell](/powershell/scripting/powershell-scripting)指令碼。</span><span class="sxs-lookup"><span data-stu-id="ffb65-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="ffb65-139">PowerShell 指令碼會自動觸發建置 」 目標之專案檔中。</span><span class="sxs-lookup"><span data-stu-id="ffb65-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="ffb65-140">功能啟用</span><span class="sxs-lookup"><span data-stu-id="ffb65-140">Feature activation</span></span>

<span data-ttu-id="ffb65-141">**將組件檔放在**</span><span class="sxs-lookup"><span data-stu-id="ffb65-141">**Place the assembly file**</span></span>

<span data-ttu-id="ffb65-142">`IHostingStartup`實作的組件檔案必須是*bin*-部署的應用程式中，或置於[階段存放區](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="ffb65-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="ffb65-143">對於每個使用者使用，請在使用者設定檔的執行階段存放區中放置組件：</span><span class="sxs-lookup"><span data-stu-id="ffb65-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="ffb65-144">供全域使用，將.NET Core 安裝的執行階段存放區中的組件：</span><span class="sxs-lookup"><span data-stu-id="ffb65-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="ffb65-145">時將組件部署至執行階段存放區中，符號檔可能也會部署，但不是必要的功能運作。</span><span class="sxs-lookup"><span data-stu-id="ffb65-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="ffb65-146">**相依性檔案的位置**</span><span class="sxs-lookup"><span data-stu-id="ffb65-146">**Place the dependencies file**</span></span>

<span data-ttu-id="ffb65-147">這個實作的 *\*。 deps.json*檔案必須是可存取的位置。</span><span class="sxs-lookup"><span data-stu-id="ffb65-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="ffb65-148">對於每個使用者使用，將檔案置於`additonalDeps`使用者設定檔的資料夾`.dotnet`設定：</span><span class="sxs-lookup"><span data-stu-id="ffb65-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="ffb65-149">供全域使用，將檔案置於`additonalDeps`.NET Core 安裝的資料夾：</span><span class="sxs-lookup"><span data-stu-id="ffb65-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="ffb65-150">請注意該版本， `2.0.0`，也會反映目標應用程式所使用的共用執行階段版本。</span><span class="sxs-lookup"><span data-stu-id="ffb65-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="ffb65-151">共用的執行階段如下所示 *\*。 runtimeconfig.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="ffb65-152">範例應用程式中指定共用的執行階段*HostingStartupSample.runtimeconfig.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="ffb65-153">**設定環境變數**</span><span class="sxs-lookup"><span data-stu-id="ffb65-153">**Set environment variables**</span></span>

<span data-ttu-id="ffb65-154">使用此功能的應用程式的內容中設定下列環境變數。</span><span class="sxs-lookup"><span data-stu-id="ffb65-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="ffb65-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="ffb65-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="ffb65-156">只有裝載的啟動組件會受到掃描`HostingStartupAttribute`。</span><span class="sxs-lookup"><span data-stu-id="ffb65-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="ffb65-157">在這個環境變數中提供實作的組件名稱。</span><span class="sxs-lookup"><span data-stu-id="ffb65-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="ffb65-158">範例應用程式會將此值設`StartupDiagnostics`。</span><span class="sxs-lookup"><span data-stu-id="ffb65-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="ffb65-159">值也可以設定使用[裝載啟動組件](xref:fundamentals/hosting#hosting-startup-assemblies)裝載組態設定。</span><span class="sxs-lookup"><span data-stu-id="ffb65-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="ffb65-160">DOTNET\_其他\_DEPS</span><span class="sxs-lookup"><span data-stu-id="ffb65-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="ffb65-161">實作的位置 *\*。 deps.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="ffb65-162">如果要將檔案放置在使用者設定檔的*.dotnet*每位使用者使用的資料夾：</span><span class="sxs-lookup"><span data-stu-id="ffb65-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="ffb65-163">如果檔案放在全域使用的.NET Core 安裝，請提供檔案的完整路徑：</span><span class="sxs-lookup"><span data-stu-id="ffb65-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="ffb65-164">範例應用程式會將此值設定為：</span><span class="sxs-lookup"><span data-stu-id="ffb65-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="ffb65-165">如需如何設定各種作業系統的環境變數的範例，請參閱[使用多個環境](xref:fundamentals/environments)。</span><span class="sxs-lookup"><span data-stu-id="ffb65-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="ffb65-166">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="ffb65-166">Sample app</span></span>

<span data-ttu-id="ffb65-167">[範例應用程式](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/)([如何下載](xref:tutorials/index#how-to-download-a-sample)) 會使用`IHostingStartup`建立的診斷工具。</span><span class="sxs-lookup"><span data-stu-id="ffb65-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="ffb65-168">此工具會加入兩個 middlewares 應用程式啟動時提供診斷資訊：</span><span class="sxs-lookup"><span data-stu-id="ffb65-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="ffb65-169">已註冊的服務</span><span class="sxs-lookup"><span data-stu-id="ffb65-169">Registered services</span></span>
* <span data-ttu-id="ffb65-170">位址： 配置、 主機、 基底路徑、 路徑、 查詢字串</span><span class="sxs-lookup"><span data-stu-id="ffb65-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="ffb65-171">連接： 遠端 IP、 遠端連接埠、 本機 IP，本機連接埠，用戶端憑證</span><span class="sxs-lookup"><span data-stu-id="ffb65-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="ffb65-172">要求標頭</span><span class="sxs-lookup"><span data-stu-id="ffb65-172">Request headers</span></span>
* <span data-ttu-id="ffb65-173">環境變數</span><span class="sxs-lookup"><span data-stu-id="ffb65-173">Environment variables</span></span>

<span data-ttu-id="ffb65-174">若要執行範例：</span><span class="sxs-lookup"><span data-stu-id="ffb65-174">To run the sample:</span></span>

1. <span data-ttu-id="ffb65-175">診斷啟動專案使用[PowerShell](/powershell/scripting/powershell-scripting)修改其*StartupDiagnostics.deps.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="ffb65-176">從 Windows 7 SP1 和 Windows Server 2008 R2 SP1 的 Windows 作業系統上預設會安裝 PowerShell。</span><span class="sxs-lookup"><span data-stu-id="ffb65-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="ffb65-177">若要取得其他平台上的 PowerShell，請參閱[安裝 Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell)。</span><span class="sxs-lookup"><span data-stu-id="ffb65-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="ffb65-178">建置診斷啟動專案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="ffb65-179">在專案檔建置 」 目標：</span><span class="sxs-lookup"><span data-stu-id="ffb65-179">A build target in the project file:</span></span>
   * <span data-ttu-id="ffb65-180">將組件和符號存放區-執行階段的使用者設定檔的檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="ffb65-181">PowerShell 指令碼來修改觸發程序*StartupDiagnostics.deps.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="ffb65-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="ffb65-182">移動*StartupDiagnostics.deps.json*使用者設定檔的檔案`additionalDeps`資料夾。</span><span class="sxs-lookup"><span data-stu-id="ffb65-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="ffb65-183">設定環境變數：</span><span class="sxs-lookup"><span data-stu-id="ffb65-183">Set the environment variables:</span></span>
    * <span data-ttu-id="ffb65-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="ffb65-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="ffb65-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="ffb65-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="ffb65-186">執行範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="ffb65-186">Run the sample app.</span></span>
5. <span data-ttu-id="ffb65-187">要求`/services`端點，請參閱應用程式的註冊服務。</span><span class="sxs-lookup"><span data-stu-id="ffb65-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="ffb65-188">要求`/diag`端點，請參閱 < 診斷的資訊。</span><span class="sxs-lookup"><span data-stu-id="ffb65-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
