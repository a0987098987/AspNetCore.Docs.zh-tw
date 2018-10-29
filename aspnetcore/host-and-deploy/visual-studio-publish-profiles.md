---
title: 適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔
author: rick-anderson
description: 了解如何在 Visual Studio 中建立發行設定檔，並使用這些設定檔來管理對各種目標的 ASP.NET Core 應用程式部署。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3e626f99b06b0343360d6c46447e357890433dda
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148924"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="c3b5a-103">適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔</span><span class="sxs-lookup"><span data-stu-id="c3b5a-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="c3b5a-104">作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c3b5a-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c3b5a-105">本文將焦點放在使用 Visual Studio 2017 來建立及使用發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="c3b5a-106">使用 Visual Studio 建立的發行設定檔，可從 MSBuild 和 Visual Studio 2017 執行。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="c3b5a-107">如需發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="c3b5a-108">以下是使用 `dotnet new mvc`命令來建立的專案檔：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-108">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.7" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.8" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="c3b5a-109">`<Project>` 元素的 `Sdk` 屬性會完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-109">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="c3b5a-110">一開始會從 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* 匯入屬性檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-110">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="c3b5a-111">從結尾的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* 匯入目標檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-111">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="c3b5a-112">`MSBuildSDKsPath` (與 Visual Studio 2017 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-112">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="c3b5a-113">`Microsoft.NET.Sdk.Web` SDK 依存於：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-113">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="c3b5a-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="c3b5a-115">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-115">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="c3b5a-116">這會導致匯入下列屬性和目標：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-116">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="c3b5a-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="c3b5a-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="c3b5a-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="c3b5a-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="c3b5a-121">發佈目標會根據所使用的發佈方法，匯入一組正確的目標。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-121">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="c3b5a-122">當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-122">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="c3b5a-123">建置專案</span><span class="sxs-lookup"><span data-stu-id="c3b5a-123">Build project</span></span>
* <span data-ttu-id="c3b5a-124">計算要發佈的檔案</span><span class="sxs-lookup"><span data-stu-id="c3b5a-124">Compute files to publish</span></span>
* <span data-ttu-id="c3b5a-125">將檔案發佈至目的地</span><span class="sxs-lookup"><span data-stu-id="c3b5a-125">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="c3b5a-126">計算專案項目</span><span class="sxs-lookup"><span data-stu-id="c3b5a-126">Compute project items</span></span>

<span data-ttu-id="c3b5a-127">載入專案時，會計算專案項目 (檔案)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-127">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="c3b5a-128">`item type` 屬性會決定如何處理檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-128">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="c3b5a-129">根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-129">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="c3b5a-130">`Compile` 項目清單中的檔案會予以編譯。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-130">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="c3b5a-131">`Content` 項目清單包含除了組建輸出之外還會另行發佈的檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-131">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="c3b5a-132">符合 `wwwroot/**` 模式的檔案預設會包含在 `Content` 項目中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-132">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="c3b5a-133">`wwwroot/\*\*` [萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)會比對 [wwwroot] 資料夾**和**子資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-133">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="c3b5a-134">若要明確地將檔案新增至發佈清單，請直接在 *.csproj* 檔案中新增檔案，如[包含檔案](#include-files)中所示。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-134">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="c3b5a-135">在 Visual Studio 中選取 [發佈] 按鈕，或從命令列發佈時：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-135">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="c3b5a-136">會計算屬性/項目 (需要建置的檔案)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-136">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="c3b5a-137">**僅限 Visual Studio**：會還原 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-137">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="c3b5a-138">(CLI 的使用者要明確還原。)</span><span class="sxs-lookup"><span data-stu-id="c3b5a-138">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="c3b5a-139">專案隨即建置。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-139">The project builds.</span></span>
* <span data-ttu-id="c3b5a-140">會計算的發佈項目 (需要發佈的檔案)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-140">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="c3b5a-141">會發佈專案 (計算的檔案會複製到發佈目的地)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-141">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="c3b5a-142">當 ASP.NET Core 專案參考專案檔中的 `Microsoft.NET.Sdk.Web` 時，*app_offline.htm* 檔案會放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-142">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="c3b5a-143">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-143">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="c3b5a-144">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-144">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="c3b5a-145">基本命令列發佈</span><span class="sxs-lookup"><span data-stu-id="c3b5a-145">Basic command-line publishing</span></span>

<span data-ttu-id="c3b5a-146">命令列發佈適用於所有 .NET Core 支援的平台，而不需要 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-146">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="c3b5a-147">在下列範例中，是從專案目錄 (其中包含 *.csproj* 檔案) 執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-147">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="c3b5a-148">如果不在專案資料夾中，請以明確方式傳入專案檔路徑。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-148">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="c3b5a-149">例如: </span><span class="sxs-lookup"><span data-stu-id="c3b5a-149">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="c3b5a-150">執行下列命令來建立及發佈 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-150">Run the following commands to create and publish a web app:</span></span>

::: moniker range=">= aspnetcore-2.0"

```console
dotnet new mvc
dotnet publish
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```console
dotnet new mvc
dotnet restore
dotnet publish
```

::: moniker-end

<span data-ttu-id="c3b5a-151">[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會產生類似以下的輸出：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-151">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="c3b5a-152">預設發佈資料夾是 `bin\$(Configuration)\netcoreapp<version>\publish`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="c3b5a-153">`$(Configuration)` 的預設值是 *Debug*。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-153">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="c3b5a-154">在上述範例中，`<TargetFramework>` 是 `netcoreapp2.0`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-154">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="c3b5a-155">`dotnet publish -h` 顯示發佈的說明資訊。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-155">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="c3b5a-156">下列命令會指定 `Release` 組建和發佈目錄：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-156">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="c3b5a-157">[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會呼叫 MSBuild 來叫用 `Publish` 目標。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="c3b5a-158">所有傳遞給 `dotnet publish` 的參數都會傳遞給 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-158">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="c3b5a-159">`-c` 參數對應至 `Configuration` MSBuild 屬性。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-159">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="c3b5a-160">`-o` 參數對應至 `OutputPath`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-160">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="c3b5a-161">您可以使用下列兩種格式其中之一來傳遞 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-161">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="c3b5a-162">下列命令會將 `Release` 組建發佈到網路共用：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-162">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="c3b5a-163">網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-163">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="c3b5a-164">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-164">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="c3b5a-165">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-165">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="c3b5a-166">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-166">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="c3b5a-167">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="c3b5a-167">Publish profiles</span></span>

<span data-ttu-id="c3b5a-168">本節會使用 Visual Studio 2017 來建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-168">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="c3b5a-169">建立之後，即可從 Visual Studio 或命令列進行發佈。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-169">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="c3b5a-170">發行設定檔可以簡化發佈程序，且設定檔數目並無限制。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-170">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="c3b5a-171">請在 Visual Studio 中選擇下列其中一個路徑來建立設定檔：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-171">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="c3b5a-172">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [發佈]。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-172">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="c3b5a-173">從 [建置] 功能表中選取 [發佈 &lt;project_name&gt;] 。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-173">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="c3b5a-174">應用程式容量頁面的 [發佈] 索引標籤隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-174">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="c3b5a-175">如果專案缺少發行設定檔，則會顯示下列頁面：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-175">If the project lacks a publish profile, the following page is displayed:</span></span>

![應用程式容量頁面的 [發佈] 索引標籤](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="c3b5a-177">選取 [資料夾] 時，請指定要用來儲存所發佈資產的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-177">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="c3b5a-178">預設的資料夾是 *bin\Release\PublishOutput*。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-178">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="c3b5a-179">請按一下 [建立設定檔] 按鈕來完成操作。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-179">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="c3b5a-180">建立發行設定檔之後，[發佈] 索引標籤就會變更。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-180">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="c3b5a-181">新建立的設定檔會出現在下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-181">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="c3b5a-182">按一下 [建立新設定檔] 即可建立另一個新的設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-182">Click **Create new profile** to create another new profile.</span></span>

![顯示 FolderProfile 的應用程式容量頁面 [發佈] 索引標籤](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="c3b5a-184">發佈精靈支援下列發佈目標：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-184">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="c3b5a-185">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="c3b5a-185">Azure App Service</span></span>
* <span data-ttu-id="c3b5a-186">Azure 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="c3b5a-186">Azure Virtual Machines</span></span>
* <span data-ttu-id="c3b5a-187">IIS、FTP 等等 (適用於任何網頁伺服器)</span><span class="sxs-lookup"><span data-stu-id="c3b5a-187">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="c3b5a-188">資料夾</span><span class="sxs-lookup"><span data-stu-id="c3b5a-188">Folder</span></span>
* <span data-ttu-id="c3b5a-189">匯入設定檔</span><span class="sxs-lookup"><span data-stu-id="c3b5a-189">Import Profile</span></span>

<span data-ttu-id="c3b5a-190">如需詳細資訊，請參閱[哪些發佈選項適合我？](/visualstudio/ide/not-in-toc/web-publish-options)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-190">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="c3b5a-191">使用 Visual Studio 來建立發行設定檔時，會建立 *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-191">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="c3b5a-192">此 *.pubxml* 檔案是一個 MSBuild 檔案，而且包含發佈組態設定。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-192">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="c3b5a-193">您可以變更這個檔案以自訂建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-193">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="c3b5a-194">發佈程序會讀取這個檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-194">This file is read by the publishing process.</span></span> <span data-ttu-id="c3b5a-195">`<LastUsedBuildConfiguration>` 很特殊，因為它是全域屬性，不應該在組建中所匯入的任何檔案中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-195">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="c3b5a-196">如需詳細資訊，請參閱 [MSBuild：如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-196">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="c3b5a-197">發佈到 Azure 目標時，*.pubxml* 檔案會包含您的 Azure 訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-197">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="c3b5a-198">使用該目標類型時，不建議將此檔案新增至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-198">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="c3b5a-199">發佈到非 Azure 目標時，可放心簽入 *.pubxml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-199">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="c3b5a-200">機密資訊 (例如發佈密碼) 會依每個使用者/電腦加密。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-200">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="c3b5a-201">其儲存位置是在 *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-201">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="c3b5a-202">由於此檔案可以儲存機密資訊，因此不應該將它簽入至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-202">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="c3b5a-203">如需如何在 ASP.NET Core 上發佈 Web 應用程式的概觀，請參閱[裝載及部署](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-203">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="c3b5a-204">發佈 ASP.NET Core 應用程式所需的 MSBuild 工作和目標是可從 https://github.com/aspnet/websdk 取得的開放原始碼。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-204">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="c3b5a-205">`dotnet publish` 可以使用資料夾、MSDeploy 及 [Kudu](https://github.com/projectkudu/kudu/wiki) 發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-205">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="c3b5a-206">資料夾 (可跨平台運作)：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-206">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="c3b5a-207">MSDeploy (目前僅適用於 Windows，因為 MSDeploy 無法跨平台運作)：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-207">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="c3b5a-208">MSDeploy 套件 (目前僅適用於 Windows，因為 MSDeploy 無法跨平台運作)：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-208">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="c3b5a-209">在先前的範例中，請「不要」將 `deployonbuild` 傳遞給 `dotnet publish`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-209">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="c3b5a-210">如需詳細資訊，請參閱 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)\(英文\)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="c3b5a-211">`dotnet publish` 支援使用 Kudu API 從任何平台發佈到 Azure。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-211">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="c3b5a-212">Visual Studio 發佈支援 Kudu API，但 WebSDK 也支援使用它來跨平台發佈到 Azure。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-212">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="c3b5a-213">將含有下列內容的發行設定檔新增至 *Properties/PublishProfiles* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-213">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="c3b5a-214">執行下列命令以壓縮發佈內容，然後使用 Kudu API 將它發佈到 Azure：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-214">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="c3b5a-215">使用發行設定檔時，請設定下列 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-215">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="c3b5a-216">使用名為 *FolderProfile* 的設定檔來進行發佈時，可以執行下列兩個命令其中之一：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-216">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="c3b5a-217">叫用 [dotnet build](/dotnet/core/tools/dotnet-build) 時，它會呼叫 `msbuild` 來執行建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-217">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="c3b5a-218">傳入資料夾設定檔時，呼叫 `dotnet build` 或 `msbuild` 是一樣的。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-218">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="c3b5a-219">直接在 Windows 上呼叫 MSBuild 時，會使用 .NET Framework 版本的 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-219">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="c3b5a-220">MSDeploy 目前僅限於 Windows 電腦進行發佈。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="c3b5a-221">在非資料夾設定檔上呼叫 `dotnet build` 會叫用 MSBuild，而 MSBuild 會在非資料夾設定檔上使用 MSDeploy。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="c3b5a-222">在非資料夾設定檔上呼叫 `dotnet build`，會叫用 MSBuild (使用 MSDeploy) 並造成失敗 (即使是在 Windows 平台上執行)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="c3b5a-223">若要發佈非資料夾的設定檔，請直接呼叫 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="c3b5a-224">下列資料夾發行設定檔是以 Visual Studio 建立並發佈至網路共用：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="c3b5a-225">請注意，`<LastUsedBuildConfiguration>` 設定為 `Release`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="c3b5a-226">從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 組態屬性值。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="c3b5a-227">`<LastUsedBuildConfiguration>` 組態屬性很特殊，不應該在匯入的 MSBuild 檔案中被覆寫。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="c3b5a-228">您可以從命令列覆寫此屬性。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-228">This property can be overridden from the command line.</span></span>

<span data-ttu-id="c3b5a-229">使用 .NET Core CLI：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-229">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="c3b5a-230">使用 MSBuild：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-230">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="c3b5a-231">從命令列發佈至 MSDeploy 端點</span><span class="sxs-lookup"><span data-stu-id="c3b5a-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="c3b5a-232">您可以使用 .NET Core CLI 或 MSBuild 來完成發佈。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-232">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="c3b5a-233">`dotnet publish` 在 .NET Core 的內容中執行。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="c3b5a-234">`msbuild` 命令需要 .NET Framework，因此會將它限制在 Windows 環境。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-234">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="c3b5a-235">以 MSDeploy 發佈最簡單的方式，是先在 Visual Studio 2017 中建立發行設定檔，再從命令列使用設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="c3b5a-236">下列範例會建立 ASP.NET Core Web 應用程式 (使用 `dotnet new mvc`)，並使用 Visual Studio 來新增 Azure 發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-236">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="c3b5a-237">請從「VS 2017 的開發人員命令提示字元」執行 `msbuild`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-237">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="c3b5a-238">「開發人員命令提示字元」的路徑中會有已設定一些 MSBuild 變數的正確 *msbuild.exe*。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-238">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="c3b5a-239">MSBuild 使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-239">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="c3b5a-240">請從 *\<發佈名稱>.PublishSettings* 檔案取得 `Password`。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-240">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="c3b5a-241">從下列其中一個位置下載 *.PublishSettings* 檔案：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-241">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="c3b5a-242">方案總管：在 Web 應用程式上按一下滑鼠右鍵，然後選取 [下載發行設定檔]。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-242">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="c3b5a-243">Azure 入口網站：按一下 Web 應用程式 [概觀] 面板上的 [取得發行設定檔]。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-243">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="c3b5a-244">`Username` 可以在發行設定檔中找到。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="c3b5a-245">下列範例使用 *Web11112 - Web Deploy*發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-245">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="c3b5a-246">排除檔案</span><span class="sxs-lookup"><span data-stu-id="c3b5a-246">Exclude files</span></span>

<span data-ttu-id="c3b5a-247">發佈 ASP.NET Core Web 應用程式時，會包含 *wwwroot* 資料夾的組建成品和內容。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="c3b5a-248">`msbuild` 支援[通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="c3b5a-249">例如，下列 `<Content>` 元素會排除 *wwwroot/content* 資料夾及其所有子資料夾中的所有文字檔 (*.txt*)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-249">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="c3b5a-250">您可以將上述標記新增至發行設定檔或 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-250">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="c3b5a-251">當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="c3b5a-252">下列 `<MsDeploySkipRules>` 元素會排除 *wwwroot/content* 資料夾中的所有檔案：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-252">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="c3b5a-253">`<MsDeploySkipRules>` 不會從部署網站刪除「略過」目標。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-253">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="c3b5a-254">這會從部署網站刪除 `<Content>` 鎖定目標的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-254">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="c3b5a-255">例如，假設所部署的 Web 應用程式包含下列檔案：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-255">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="c3b5a-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-256">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="c3b5a-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-257">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="c3b5a-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="c3b5a-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="c3b5a-259">如果新增下列 `<MsDeploySkipRules>` 元素，就不會刪除部署網站上的這些檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-259">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="c3b5a-260">上述 `<MsDeploySkipRules>` 元素會防止部署「已略過」的檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-260">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="c3b5a-261">如果已部署這些檔案，它並不會將它們刪除。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-261">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="c3b5a-262">下列 `<Content>` 元素會刪除部署網站上已鎖定目標的檔案：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-262">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="c3b5a-263">使用命令列部署搭配上述 `<Content>` 元素會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-263">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="c3b5a-264">Include 檔案</span><span class="sxs-lookup"><span data-stu-id="c3b5a-264">Include files</span></span>

<span data-ttu-id="c3b5a-265">下列標記會將專案目錄外的 [images] 資料夾包 含在發佈網站的 *wwwroot/images* 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-265">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="c3b5a-266">標記可以新增至 *.csproj* 檔案或發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-266">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="c3b5a-267">如果將它新增至 *.csproj* 檔案，它就會包含在專案的每個發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-267">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="c3b5a-268">下列反白顯示的標記會示範如何：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-268">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="c3b5a-269">將專案外的檔案複製到 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-269">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="c3b5a-270">排除 *wwwroot\Content* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-270">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="c3b5a-271">排除 *Views\Home\About2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-271">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="c3b5a-272">如需更多的部署範例，請參閱 [WebSDK 讀我檔案](https://github.com/aspnet/websdk)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-272">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="c3b5a-273">在發佈之前或之後執行目標</span><span class="sxs-lookup"><span data-stu-id="c3b5a-273">Run a target before or after publishing</span></span>

<span data-ttu-id="c3b5a-274">內建的 `BeforePublish` 和 `AfterPublish` 目標可在發佈目標之前或之後執行目標。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-274">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="c3b5a-275">將下列元素新增至發行設定檔，即可在發佈之前和之後都記錄主控台訊息：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-275">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="c3b5a-276">使用未受信任的憑證來發佈至伺服器</span><span class="sxs-lookup"><span data-stu-id="c3b5a-276">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="c3b5a-277">將 `<AllowUntrustedCertificate>` 屬性搭配 `True` 值新增至發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="c3b5a-277">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="c3b5a-278">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="c3b5a-278">The Kudu service</span></span>

<span data-ttu-id="c3b5a-279">若要檢視 Azure App Service Web 應用程式部署中的檔案，請使用 [Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-279">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="c3b5a-280">將 `scm` 權杖附加至 Web 應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-280">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="c3b5a-281">例如: </span><span class="sxs-lookup"><span data-stu-id="c3b5a-281">For example:</span></span>

| <span data-ttu-id="c3b5a-282">URL</span><span class="sxs-lookup"><span data-stu-id="c3b5a-282">URL</span></span>                                    | <span data-ttu-id="c3b5a-283">結果</span><span class="sxs-lookup"><span data-stu-id="c3b5a-283">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="c3b5a-284">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="c3b5a-284">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="c3b5a-285">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="c3b5a-285">Kudu service</span></span> |

<span data-ttu-id="c3b5a-286">選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目，以檢視、編輯、刪除或新增檔案。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-286">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c3b5a-287">其他資源</span><span class="sxs-lookup"><span data-stu-id="c3b5a-287">Additional resources</span></span>

* <span data-ttu-id="c3b5a-288">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) 可簡化對 IIS 伺服器的 Web 應用程式和網站部署。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-288">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="c3b5a-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)：針對部署提出問題及要求功能。</span><span class="sxs-lookup"><span data-stu-id="c3b5a-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="c3b5a-290">從 Visual Studio 將 ASP.NET Web 應用程式發佈至 Azure VM</span><span class="sxs-lookup"><span data-stu-id="c3b5a-290">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
