---
title: 適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔
author: rick-anderson
description: 了解如何在 Visual Studio 中建立發行設定檔，並使用這些設定檔來管理對各種目標的 ASP.NET Core 應用程式部署。
ms.author: riande
ms.custom: mvc
ms.date: 01/22/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: e1e8f99be18d6f395a146bda805f71c46cd0346d
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248351"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="bd079-103">適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔</span><span class="sxs-lookup"><span data-stu-id="bd079-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="bd079-104">作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="bd079-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="bd079-105">如需本主題的 1.1 版，請下載 [Visual Studio publish profiles for ASP.NET Core app deployment (PDF 1.1 版)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf) (適用於 ASP.NET Core 應用程式部署的 Visual Studio 發行設定檔)。</span><span class="sxs-lookup"><span data-stu-id="bd079-105">For the 1.1 version of this topic, download [Visual Studio publish profiles for ASP.NET Core app deployment (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/VS_Publish_Profiles_1.1.pdf).</span></span>

::: moniker-end

<span data-ttu-id="bd079-106">本文將焦點放在使用 Visual Studio 2017 或更新版本來建立及使用發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bd079-106">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="bd079-107">使用 Visual Studio 建立的發行設定檔，可從 MSBuild 和 Visual Studio 執行。</span><span class="sxs-lookup"><span data-stu-id="bd079-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="bd079-108">如需發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="bd079-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="bd079-109">以下是使用 `dotnet new mvc`命令來建立的專案檔：</span><span class="sxs-lookup"><span data-stu-id="bd079-109">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.2"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

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

<span data-ttu-id="bd079-110">`<Project>` 元素的 `Sdk` 屬性會完成下列工作：</span><span class="sxs-lookup"><span data-stu-id="bd079-110">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="bd079-111">一開始會從 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* 匯入屬性檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-111">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="bd079-112">從結尾的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* 匯入目標檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-112">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="bd079-113">`MSBuildSDKsPath` (與 Visual Studio 2017 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="bd079-113">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="bd079-114">`Microsoft.NET.Sdk.Web` SDK 依存於：</span><span class="sxs-lookup"><span data-stu-id="bd079-114">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="bd079-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="bd079-115">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="bd079-116">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="bd079-116">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="bd079-117">這會導致匯入下列屬性和目標：</span><span class="sxs-lookup"><span data-stu-id="bd079-117">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="bd079-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="bd079-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="bd079-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="bd079-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="bd079-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="bd079-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="bd079-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="bd079-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="bd079-122">發佈目標會根據所使用的發佈方法，匯入一組正確的目標。</span><span class="sxs-lookup"><span data-stu-id="bd079-122">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="bd079-123">當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：</span><span class="sxs-lookup"><span data-stu-id="bd079-123">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="bd079-124">建置專案</span><span class="sxs-lookup"><span data-stu-id="bd079-124">Build project</span></span>
* <span data-ttu-id="bd079-125">計算要發佈的檔案</span><span class="sxs-lookup"><span data-stu-id="bd079-125">Compute files to publish</span></span>
* <span data-ttu-id="bd079-126">將檔案發佈至目的地</span><span class="sxs-lookup"><span data-stu-id="bd079-126">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="bd079-127">計算專案項目</span><span class="sxs-lookup"><span data-stu-id="bd079-127">Compute project items</span></span>

<span data-ttu-id="bd079-128">載入專案時，會計算專案項目 (檔案)。</span><span class="sxs-lookup"><span data-stu-id="bd079-128">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="bd079-129">`item type` 屬性會決定如何處理檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-129">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="bd079-130">根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="bd079-130">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="bd079-131">`Compile` 項目清單中的檔案會予以編譯。</span><span class="sxs-lookup"><span data-stu-id="bd079-131">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="bd079-132">`Content` 項目清單包含除了組建輸出之外還會另行發佈的檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-132">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="bd079-133">符合 `wwwroot/**` 模式的檔案預設會包含在 `Content` 項目中。</span><span class="sxs-lookup"><span data-stu-id="bd079-133">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="bd079-134">`wwwroot/\*\*` [萬用字元模式](https://gruntjs.com/configuring-tasks#globbing-patterns)會比對 [wwwroot] 資料夾**和**子資料夾中的所有檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-134">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="bd079-135">若要明確地將檔案新增至發佈清單，請直接在 *.csproj* 檔案中新增檔案，如[包含檔案](#include-files)中所示。</span><span class="sxs-lookup"><span data-stu-id="bd079-135">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="bd079-136">在 Visual Studio 中選取 [發佈] 按鈕，或從命令列發佈時：</span><span class="sxs-lookup"><span data-stu-id="bd079-136">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="bd079-137">會計算屬性/項目 (需要建置的檔案)。</span><span class="sxs-lookup"><span data-stu-id="bd079-137">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="bd079-138">**僅限 Visual Studio**：會還原 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="bd079-138">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="bd079-139">(CLI 的使用者要明確還原。)</span><span class="sxs-lookup"><span data-stu-id="bd079-139">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="bd079-140">專案隨即建置。</span><span class="sxs-lookup"><span data-stu-id="bd079-140">The project builds.</span></span>
* <span data-ttu-id="bd079-141">會計算的發佈項目 (需要發佈的檔案)。</span><span class="sxs-lookup"><span data-stu-id="bd079-141">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="bd079-142">會發佈專案 (計算的檔案會複製到發佈目的地)。</span><span class="sxs-lookup"><span data-stu-id="bd079-142">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="bd079-143">當 ASP.NET Core 專案參考專案檔中的 `Microsoft.NET.Sdk.Web` 時，*app_offline.htm* 檔案會放在 Web 應用程式目錄的根目錄中。</span><span class="sxs-lookup"><span data-stu-id="bd079-143">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="bd079-144">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-144">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="bd079-145">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#app_offlinehtm)。</span><span class="sxs-lookup"><span data-stu-id="bd079-145">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="bd079-146">基本命令列發佈</span><span class="sxs-lookup"><span data-stu-id="bd079-146">Basic command-line publishing</span></span>

<span data-ttu-id="bd079-147">命令列發佈適用於所有 .NET Core 支援的平台，而不需要 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="bd079-147">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="bd079-148">在下列範例中，是從專案目錄 (其中包含 *.csproj* 檔案) 執行 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令。</span><span class="sxs-lookup"><span data-stu-id="bd079-148">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="bd079-149">如果不在專案資料夾中，請以明確方式傳入專案檔路徑。</span><span class="sxs-lookup"><span data-stu-id="bd079-149">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="bd079-150">例如：</span><span class="sxs-lookup"><span data-stu-id="bd079-150">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="bd079-151">執行下列命令來建立及發佈 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="bd079-151">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="bd079-152">[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會產生類似以下的輸出：</span><span class="sxs-lookup"><span data-stu-id="bd079-152">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="bd079-153">預設發佈資料夾是 `bin\$(Configuration)\netcoreapp<version>\publish`。</span><span class="sxs-lookup"><span data-stu-id="bd079-153">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="bd079-154">`$(Configuration)` 的預設值是 *Debug*。</span><span class="sxs-lookup"><span data-stu-id="bd079-154">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="bd079-155">在上述範例中，`<TargetFramework>` 是 `netcoreapp2.0`。</span><span class="sxs-lookup"><span data-stu-id="bd079-155">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="bd079-156">`dotnet publish -h` 顯示發佈的說明資訊。</span><span class="sxs-lookup"><span data-stu-id="bd079-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="bd079-157">下列命令會指定 `Release` 組建和發佈目錄：</span><span class="sxs-lookup"><span data-stu-id="bd079-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="bd079-158">[dotnet publish](/dotnet/core/tools/dotnet-publish) 命令會呼叫 MSBuild 來叫用 `Publish` 目標。</span><span class="sxs-lookup"><span data-stu-id="bd079-158">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="bd079-159">所有傳遞給 `dotnet publish` 的參數都會傳遞給 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="bd079-159">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="bd079-160">`-c` 參數對應至 `Configuration` MSBuild 屬性。</span><span class="sxs-lookup"><span data-stu-id="bd079-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="bd079-161">`-o` 參數對應至 `OutputPath`。</span><span class="sxs-lookup"><span data-stu-id="bd079-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="bd079-162">您可以使用下列兩種格式其中之一來傳遞 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="bd079-162">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="bd079-163">下列命令會將 `Release` 組建發佈到網路共用：</span><span class="sxs-lookup"><span data-stu-id="bd079-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="bd079-164">網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。</span><span class="sxs-lookup"><span data-stu-id="bd079-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="bd079-165">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="bd079-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="bd079-166">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="bd079-167">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="bd079-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="bd079-168">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="bd079-168">Publish profiles</span></span>

<span data-ttu-id="bd079-169">本節會使用 Visual Studio 2017 或更新版本來建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bd079-169">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="bd079-170">建立設定檔之後，即可從 Visual Studio 或命令列進行發佈。</span><span class="sxs-lookup"><span data-stu-id="bd079-170">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="bd079-171">發行設定檔可以簡化發佈程序，且設定檔數目並無限制。</span><span class="sxs-lookup"><span data-stu-id="bd079-171">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="bd079-172">請在 Visual Studio 中選擇下列其中一個路徑來建立設定檔：</span><span class="sxs-lookup"><span data-stu-id="bd079-172">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="bd079-173">在 [方案總管] 中的專案上按一下滑鼠右鍵，然後選取 [發佈]。</span><span class="sxs-lookup"><span data-stu-id="bd079-173">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="bd079-174">從 [建置] 功能表選取 [發佈 {專案名稱}]。</span><span class="sxs-lookup"><span data-stu-id="bd079-174">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="bd079-175">應用程式容量頁面的 [發佈] 索引標籤隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="bd079-175">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="bd079-176">如果專案缺少發行設定檔，則會顯示下列頁面：</span><span class="sxs-lookup"><span data-stu-id="bd079-176">If the project lacks a publish profile, the following page is displayed:</span></span>

![應用程式容量頁面的 [發佈] 索引標籤](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="bd079-178">選取 [資料夾] 時，請指定要用來儲存所發佈資產的資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="bd079-178">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="bd079-179">預設的資料夾是 *bin\Release\PublishOutput*。</span><span class="sxs-lookup"><span data-stu-id="bd079-179">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="bd079-180">請按一下 [建立設定檔] 按鈕來完成操作。</span><span class="sxs-lookup"><span data-stu-id="bd079-180">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="bd079-181">建立發行設定檔之後，[發佈] 索引標籤就會變更。</span><span class="sxs-lookup"><span data-stu-id="bd079-181">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="bd079-182">新建立的設定檔會出現在下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="bd079-182">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="bd079-183">按一下 [建立新設定檔] 即可建立另一個新的設定檔。</span><span class="sxs-lookup"><span data-stu-id="bd079-183">Click **Create new profile** to create another new profile.</span></span>

![顯示 FolderProfile 的應用程式容量頁面 [發佈] 索引標籤](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="bd079-185">發佈精靈支援下列發佈目標：</span><span class="sxs-lookup"><span data-stu-id="bd079-185">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="bd079-186">Azure App Service</span><span class="sxs-lookup"><span data-stu-id="bd079-186">Azure App Service</span></span>
* <span data-ttu-id="bd079-187">Azure 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="bd079-187">Azure Virtual Machines</span></span>
* <span data-ttu-id="bd079-188">IIS、FTP 等等 (適用於任何網頁伺服器)</span><span class="sxs-lookup"><span data-stu-id="bd079-188">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="bd079-189">資料夾</span><span class="sxs-lookup"><span data-stu-id="bd079-189">Folder</span></span>
* <span data-ttu-id="bd079-190">匯入設定檔</span><span class="sxs-lookup"><span data-stu-id="bd079-190">Import Profile</span></span>

<span data-ttu-id="bd079-191">如需詳細資訊，請參閱[哪些發佈選項適合我？](/visualstudio/ide/not-in-toc/web-publish-options)。</span><span class="sxs-lookup"><span data-stu-id="bd079-191">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="bd079-192">使用 Visual Studio 來建立發行設定檔時，會建立 *Properties/PublishProfiles/{設定檔名稱}.pubxml* MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-192">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="bd079-193">此 *.pubxml* 檔案是一個 MSBuild 檔案，而且包含發佈組態設定。</span><span class="sxs-lookup"><span data-stu-id="bd079-193">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="bd079-194">您可以變更這個檔案以自訂建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="bd079-194">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="bd079-195">發佈程序會讀取這個檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-195">This file is read by the publishing process.</span></span> <span data-ttu-id="bd079-196">`<LastUsedBuildConfiguration>` 很特殊，因為它是全域屬性，不應該在組建中所匯入的任何檔案中。</span><span class="sxs-lookup"><span data-stu-id="bd079-196">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="bd079-197">如需詳細資訊，請參閱 [MSBuild：如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="bd079-197">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="bd079-198">發佈到 Azure 目標時，*.pubxml* 檔案會包含您的 Azure 訂用帳戶識別碼。</span><span class="sxs-lookup"><span data-stu-id="bd079-198">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="bd079-199">使用該目標類型時，不建議將此檔案新增至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="bd079-199">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="bd079-200">發佈到非 Azure 目標時，可放心簽入 *.pubxml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-200">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="bd079-201">機密資訊 (例如發佈密碼) 會依每個使用者/電腦加密。</span><span class="sxs-lookup"><span data-stu-id="bd079-201">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="bd079-202">其儲存位置是在 *Properties/PublishProfiles/{設定檔名稱}.pubxml.user* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="bd079-202">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="bd079-203">由於此檔案可以儲存機密資訊，因此不應該將它簽入至原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="bd079-203">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="bd079-204">如需如何在 ASP.NET Core 上發佈 Web 應用程式的概觀，請參閱[裝載及部署](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="bd079-204">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="bd079-205">發佈 ASP.NET Core 應用程式所需的 MSBuild 工作和目標是可從 https://github.com/aspnet/websdk 取得的開放原始碼。</span><span class="sxs-lookup"><span data-stu-id="bd079-205">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="bd079-206">`dotnet publish` 可以使用資料夾、MSDeploy 及 [Kudu](https://github.com/projectkudu/kudu/wiki) 發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="bd079-206">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="bd079-207">資料夾 (可跨平台運作)：</span><span class="sxs-lookup"><span data-stu-id="bd079-207">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="bd079-208">MSDeploy (目前僅適用於 Windows，因為 MSDeploy 無法跨平台運作)：</span><span class="sxs-lookup"><span data-stu-id="bd079-208">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="bd079-209">MSDeploy 套件 (目前僅適用於 Windows，因為 MSDeploy 無法跨平台運作)：</span><span class="sxs-lookup"><span data-stu-id="bd079-209">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="bd079-210">在先前的範例中，請「不要」將 `deployonbuild` 傳遞給 `dotnet publish`。</span><span class="sxs-lookup"><span data-stu-id="bd079-210">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="bd079-211">如需詳細資訊，請參閱 [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)\(英文\)。</span><span class="sxs-lookup"><span data-stu-id="bd079-211">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="bd079-212">`dotnet publish` 支援使用 Kudu API 從任何平台發佈到 Azure。</span><span class="sxs-lookup"><span data-stu-id="bd079-212">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="bd079-213">Visual Studio 發佈支援 Kudu API，但 WebSDK 也支援使用它來跨平台發佈到 Azure。</span><span class="sxs-lookup"><span data-stu-id="bd079-213">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="bd079-214">將含有下列內容的發行設定檔新增至 *Properties/PublishProfiles* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="bd079-214">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="bd079-215">執行下列命令以壓縮發佈內容，然後使用 Kudu API 將它發佈到 Azure：</span><span class="sxs-lookup"><span data-stu-id="bd079-215">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="bd079-216">使用發行設定檔時，請設定下列 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="bd079-216">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="bd079-217">使用名為 *FolderProfile* 的設定檔來進行發佈時，可以執行下列兩個命令其中之一：</span><span class="sxs-lookup"><span data-stu-id="bd079-217">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="bd079-218">叫用 [dotnet build](/dotnet/core/tools/dotnet-build) 時，它會呼叫 `msbuild` 來執行建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="bd079-218">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="bd079-219">傳入資料夾設定檔時，呼叫 `dotnet build` 或 `msbuild` 是一樣的。</span><span class="sxs-lookup"><span data-stu-id="bd079-219">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="bd079-220">直接在 Windows 上呼叫 MSBuild 時，會使用 .NET Framework 版本的 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="bd079-220">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="bd079-221">MSDeploy 目前僅限於 Windows 電腦進行發佈。</span><span class="sxs-lookup"><span data-stu-id="bd079-221">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="bd079-222">在非資料夾設定檔上呼叫 `dotnet build` 會叫用 MSBuild，而 MSBuild 會在非資料夾設定檔上使用 MSDeploy。</span><span class="sxs-lookup"><span data-stu-id="bd079-222">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="bd079-223">在非資料夾設定檔上呼叫 `dotnet build`，會叫用 MSBuild (使用 MSDeploy) 並造成失敗 (即使是在 Windows 平台上執行)。</span><span class="sxs-lookup"><span data-stu-id="bd079-223">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="bd079-224">若要發佈非資料夾的設定檔，請直接呼叫 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="bd079-224">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="bd079-225">下列資料夾發行設定檔是以 Visual Studio 建立並發佈至網路共用：</span><span class="sxs-lookup"><span data-stu-id="bd079-225">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="bd079-226">請注意，`<LastUsedBuildConfiguration>` 設定為 `Release`。</span><span class="sxs-lookup"><span data-stu-id="bd079-226">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="bd079-227">從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 組態屬性值。</span><span class="sxs-lookup"><span data-stu-id="bd079-227">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="bd079-228">`<LastUsedBuildConfiguration>` 組態屬性很特殊，不應該在匯入的 MSBuild 檔案中被覆寫。</span><span class="sxs-lookup"><span data-stu-id="bd079-228">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="bd079-229">您可以從命令列覆寫此屬性。</span><span class="sxs-lookup"><span data-stu-id="bd079-229">This property can be overridden from the command line.</span></span>

<span data-ttu-id="bd079-230">使用 .NET Core CLI：</span><span class="sxs-lookup"><span data-stu-id="bd079-230">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="bd079-231">使用 MSBuild：</span><span class="sxs-lookup"><span data-stu-id="bd079-231">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="bd079-232">從命令列發佈至 MSDeploy 端點</span><span class="sxs-lookup"><span data-stu-id="bd079-232">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="bd079-233">下列範例使用由 Visual Studio 建立的 ASP.NET Core Web 應用程式，名為 *AzureWebApp*。</span><span class="sxs-lookup"><span data-stu-id="bd079-233">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="bd079-234">並使用 Visual Studio 新增了一個 Azure 應用程式發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bd079-234">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="bd079-235">如需如何建立設定檔的詳細資訊，請參閱[發行設定檔](#publish-profiles)一節。</span><span class="sxs-lookup"><span data-stu-id="bd079-235">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="bd079-236">若要使用發行設定檔部署應用程式，請從 **Visual Studio 開發人員命令提示字元**執行 `msbuild` 命令。</span><span class="sxs-lookup"><span data-stu-id="bd079-236">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="bd079-237">您可以從 Windows 工作列 [開始] 功能表的 [Visual Studio] 資料夾中存取此命令提示字元。</span><span class="sxs-lookup"><span data-stu-id="bd079-237">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="bd079-238">為方便存取，您可以將命令提示字元新增至 Visual Studio 的 [工具] 功能表。</span><span class="sxs-lookup"><span data-stu-id="bd079-238">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="bd079-239">如需詳細資訊，請參閱[適用於 Visual Studio 的開發人員命令提示字元](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio)。</span><span class="sxs-lookup"><span data-stu-id="bd079-239">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="bd079-240">MSBuild 使用下列命令語法：</span><span class="sxs-lookup"><span data-stu-id="bd079-240">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="bd079-241">{PATH} &ndash; 應用程式的專案檔路徑。</span><span class="sxs-lookup"><span data-stu-id="bd079-241">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="bd079-242">{PROFILE} &ndash; 發行設定檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="bd079-242">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="bd079-243">{USERNAME} &ndash; MSDeploy 使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="bd079-243">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="bd079-244">{USERNAME} 可以在發行設定檔中找到。</span><span class="sxs-lookup"><span data-stu-id="bd079-244">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="bd079-245">{PASSWORD} &ndash; MSDeploy 密碼。</span><span class="sxs-lookup"><span data-stu-id="bd079-245">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="bd079-246">您可以從 *{PROFILE}.PublishSettings* 檔案取得 {PASSWORD}。</span><span class="sxs-lookup"><span data-stu-id="bd079-246">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="bd079-247">從下列其中一個位置下載 *.PublishSettings* 檔案：</span><span class="sxs-lookup"><span data-stu-id="bd079-247">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="bd079-248">方案總管：選取 [檢視] > [Cloud Explorer]。</span><span class="sxs-lookup"><span data-stu-id="bd079-248">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="bd079-249">連線到您的 Azure 訂用帳戶。</span><span class="sxs-lookup"><span data-stu-id="bd079-249">Connect with your Azure subscription.</span></span> <span data-ttu-id="bd079-250">開啟 [應用程式服務]。</span><span class="sxs-lookup"><span data-stu-id="bd079-250">Open **App Services**.</span></span> <span data-ttu-id="bd079-251">以滑鼠右鍵按一下應用程式。</span><span class="sxs-lookup"><span data-stu-id="bd079-251">Right-click the app.</span></span> <span data-ttu-id="bd079-252">選取 [下載發行設定檔]。</span><span class="sxs-lookup"><span data-stu-id="bd079-252">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="bd079-253">Azure 入口網站：在 Web 應用程式的 [概觀] 面板中，選取 [取得發行設定檔]。</span><span class="sxs-lookup"><span data-stu-id="bd079-253">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="bd079-254">下列範例使用名為 *AzureWebApp - Web Deploy* 的發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="bd079-254">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="bd079-255">您也可以從 Windows 命令提示字元透過 .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) 命令來使用發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="bd079-255">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="bd079-256">[dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) 命令可跨平台使用，並可在 macOS 和 Linux 上編譯 ASP.NET Core 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bd079-256">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="bd079-257">不過，macOS 和 Linux 上的 MSBuild 無法將應用程式部署至 Azure 或其他 MSDeploy 端點。</span><span class="sxs-lookup"><span data-stu-id="bd079-257">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="bd079-258">只有在 Windows 上才提供 MSDeploy。</span><span class="sxs-lookup"><span data-stu-id="bd079-258">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="bd079-259">設定環境</span><span class="sxs-lookup"><span data-stu-id="bd079-259">Set the environment</span></span>

<span data-ttu-id="bd079-260">在發行設定檔 (*.pubxml*) 或專案檔中包括 `<EnvironmentName>` 屬性，以設定應用程式的[環境](xref:fundamentals/environments)：</span><span class="sxs-lookup"><span data-stu-id="bd079-260">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="bd079-261">如果您需要進行 *web.config* 轉換 (例如根據設定、設定檔或環境設定環境變數)，請參閱<xref:host-and-deploy/iis/transform-webconfig>。</span><span class="sxs-lookup"><span data-stu-id="bd079-261">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="bd079-262">排除檔案</span><span class="sxs-lookup"><span data-stu-id="bd079-262">Exclude files</span></span>

<span data-ttu-id="bd079-263">發佈 ASP.NET Core Web 應用程式時，會包含 *wwwroot* 資料夾的組建成品和內容。</span><span class="sxs-lookup"><span data-stu-id="bd079-263">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="bd079-264">`msbuild` 支援[通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。</span><span class="sxs-lookup"><span data-stu-id="bd079-264">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="bd079-265">例如，下列 `<Content>` 元素會排除 *wwwroot/content* 資料夾及其所有子資料夾中的所有文字檔 (*.txt*)。</span><span class="sxs-lookup"><span data-stu-id="bd079-265">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="bd079-266">您可以將上述標記新增至發行設定檔或 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-266">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="bd079-267">當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bd079-267">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="bd079-268">下列 `<MsDeploySkipRules>` 元素會排除 *wwwroot/content* 資料夾中的所有檔案：</span><span class="sxs-lookup"><span data-stu-id="bd079-268">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="bd079-269">`<MsDeploySkipRules>` 不會從部署網站刪除「略過」目標。</span><span class="sxs-lookup"><span data-stu-id="bd079-269">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="bd079-270">這會從部署網站刪除 `<Content>` 鎖定目標的檔案和資料夾。</span><span class="sxs-lookup"><span data-stu-id="bd079-270">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="bd079-271">例如，假設所部署的 Web 應用程式包含下列檔案：</span><span class="sxs-lookup"><span data-stu-id="bd079-271">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="bd079-272">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bd079-272">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="bd079-273">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bd079-273">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="bd079-274">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="bd079-274">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="bd079-275">如果新增下列 `<MsDeploySkipRules>` 元素，就不會刪除部署網站上的這些檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-275">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="bd079-276">上述 `<MsDeploySkipRules>` 元素會防止部署「已略過」的檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-276">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="bd079-277">如果已部署這些檔案，它並不會將它們刪除。</span><span class="sxs-lookup"><span data-stu-id="bd079-277">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="bd079-278">下列 `<Content>` 元素會刪除部署網站上已鎖定目標的檔案：</span><span class="sxs-lookup"><span data-stu-id="bd079-278">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="bd079-279">使用命令列部署搭配上述 `<Content>` 元素會產生下列輸出：</span><span class="sxs-lookup"><span data-stu-id="bd079-279">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="bd079-280">Include 檔案</span><span class="sxs-lookup"><span data-stu-id="bd079-280">Include files</span></span>

<span data-ttu-id="bd079-281">下列標記會將專案目錄外的 [images] 資料夾包 含在發佈網站的 *wwwroot/images* 資料夾中：</span><span class="sxs-lookup"><span data-stu-id="bd079-281">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="bd079-282">標記可以新增至 *.csproj* 檔案或發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bd079-282">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="bd079-283">如果將它新增至 *.csproj* 檔案，它就會包含在專案的每個發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="bd079-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="bd079-284">下列反白顯示的標記會示範如何：</span><span class="sxs-lookup"><span data-stu-id="bd079-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="bd079-285">將專案外的檔案複製到 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="bd079-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="bd079-286">排除 *wwwroot\Content* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="bd079-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="bd079-287">排除 *Views\Home\About2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="bd079-287">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="bd079-288">如需更多的部署範例，請參閱 [WebSDK 讀我檔案](https://github.com/aspnet/websdk)。</span><span class="sxs-lookup"><span data-stu-id="bd079-288">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="bd079-289">在發佈之前或之後執行目標</span><span class="sxs-lookup"><span data-stu-id="bd079-289">Run a target before or after publishing</span></span>

<span data-ttu-id="bd079-290">內建的 `BeforePublish` 和 `AfterPublish` 目標可在發佈目標之前或之後執行目標。</span><span class="sxs-lookup"><span data-stu-id="bd079-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="bd079-291">將下列元素新增至發行設定檔，即可在發佈之前和之後都記錄主控台訊息：</span><span class="sxs-lookup"><span data-stu-id="bd079-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="bd079-292">使用未受信任的憑證來發佈至伺服器</span><span class="sxs-lookup"><span data-stu-id="bd079-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="bd079-293">將 `<AllowUntrustedCertificate>` 屬性搭配 `True` 值新增至發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="bd079-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="bd079-294">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="bd079-294">The Kudu service</span></span>

<span data-ttu-id="bd079-295">若要檢視 Azure App Service Web 應用程式部署中的檔案，請使用 [Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。</span><span class="sxs-lookup"><span data-stu-id="bd079-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="bd079-296">將 `scm` 權杖附加至 Web 應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="bd079-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="bd079-297">例如：</span><span class="sxs-lookup"><span data-stu-id="bd079-297">For example:</span></span>

| <span data-ttu-id="bd079-298">URL</span><span class="sxs-lookup"><span data-stu-id="bd079-298">URL</span></span>                                    | <span data-ttu-id="bd079-299">結果</span><span class="sxs-lookup"><span data-stu-id="bd079-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="bd079-300">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="bd079-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="bd079-301">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="bd079-301">Kudu service</span></span> |

<span data-ttu-id="bd079-302">選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目，以檢視、編輯、刪除或新增檔案。</span><span class="sxs-lookup"><span data-stu-id="bd079-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd079-303">其他資源</span><span class="sxs-lookup"><span data-stu-id="bd079-303">Additional resources</span></span>

* <span data-ttu-id="bd079-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) 可簡化對 IIS 伺服器的 Web 應用程式和網站部署。</span><span class="sxs-lookup"><span data-stu-id="bd079-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="bd079-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)：針對部署提出問題及要求功能。</span><span class="sxs-lookup"><span data-stu-id="bd079-305">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="bd079-306">從 Visual Studio 將 ASP.NET Web 應用程式發佈至 Azure VM</span><span class="sxs-lookup"><span data-stu-id="bd079-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
