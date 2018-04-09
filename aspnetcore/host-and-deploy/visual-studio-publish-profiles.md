---
title: Visual Studio 發行 ASP.NET Core 應用程式部署的設定檔
author: rick-anderson
description: 了解如何建立發行 Visual Studio 中的 ASP.NET Core 應用程式設定檔。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 64c96f572c42c56480cfe2bd58f926d54eddf35e
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="702f4-103">Visual Studio 發行 ASP.NET Core 應用程式部署的設定檔</span><span class="sxs-lookup"><span data-stu-id="702f4-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="702f4-104">作者：[Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) 及 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="702f4-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="702f4-105">本文著重在使用 Visual Studio 2017 建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="702f4-106">使用 Visual Studio 建立的發行設定檔，可從 MSBuild 和 Visual Studio 2017 執行。</span><span class="sxs-lookup"><span data-stu-id="702f4-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="702f4-107">本文提供發行程序的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="702f4-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="702f4-108">如需發行到 Azure 的指示，請參閱[使用 Visual Studio 將 ASP.NET Core Web 應用程式發行到 Azure App Service](xref:tutorials/publish-to-azure-webapp-using-vs)。</span><span class="sxs-lookup"><span data-stu-id="702f4-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="702f4-109">下列 *.csproj* 檔案是使用命令 `dotnet new mvc` 建立：</span><span class="sxs-lookup"><span data-stu-id="702f4-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="702f4-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="702f4-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="702f4-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="702f4-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="702f4-112">上述標記的 `<Project>` 項目 (在第一行) 的 `Sdk` 屬性會進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="702f4-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="702f4-113">從該內容檔案匯入*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props*開頭。</span><span class="sxs-lookup"><span data-stu-id="702f4-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="702f4-114">從結尾的 *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* 匯入目標檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="702f4-115">`MSBuildSDKsPath` (與 Visual Studio 2017 Enterprise) 的預設位置在 *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="702f4-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="702f4-116">`Microsoft.NET.Sdk.Web` 相依於：</span><span class="sxs-lookup"><span data-stu-id="702f4-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="702f4-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="702f4-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="702f4-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="702f4-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="702f4-119">這會導致下列的屬性和匯入的目標：</span><span class="sxs-lookup"><span data-stu-id="702f4-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="702f4-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="702f4-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="702f4-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="702f4-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="702f4-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="702f4-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="702f4-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="702f4-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="702f4-124">發行目標匯入設定的目標使用的發行方法為基礎的右邊。</span><span class="sxs-lookup"><span data-stu-id="702f4-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="702f4-125">當 MSBuild 或 Visual Studio 載入專案時，會執行下列高階動作：</span><span class="sxs-lookup"><span data-stu-id="702f4-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="702f4-126">建置專案</span><span class="sxs-lookup"><span data-stu-id="702f4-126">Build project</span></span>
* <span data-ttu-id="702f4-127">計算要發佈的檔案</span><span class="sxs-lookup"><span data-stu-id="702f4-127">Compute files to publish</span></span>
* <span data-ttu-id="702f4-128">將檔案發佈至目的地</span><span class="sxs-lookup"><span data-stu-id="702f4-128">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="702f4-129">計算專案項目</span><span class="sxs-lookup"><span data-stu-id="702f4-129">Compute project items</span></span>

<span data-ttu-id="702f4-130">載入專案時，會計算專案項目 (檔案)。</span><span class="sxs-lookup"><span data-stu-id="702f4-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="702f4-131">`item type` 屬性會決定如何處理檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="702f4-132">根據預設，*.cs* 檔案會包含在 `Compile` 項目清單中。</span><span class="sxs-lookup"><span data-stu-id="702f4-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="702f4-133">`Compile` 項目清單中的檔案會予以編譯。</span><span class="sxs-lookup"><span data-stu-id="702f4-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="702f4-134">`Content`項目清單會包含組建輸出除了已發行的檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="702f4-135">根據預設，檔案符合模式`wwwroot/**`隨附的`Content`項目。</span><span class="sxs-lookup"><span data-stu-id="702f4-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="702f4-136">[wwwroot /\* \*是通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)指定中的所有檔案*wwwroot*資料夾**和**子資料夾。</span><span class="sxs-lookup"><span data-stu-id="702f4-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="702f4-137">若要明確地將檔案新增至發佈清單，請直接在 *.csproj* 檔案中新增檔案，如[包括檔案](#including-files)中所示。</span><span class="sxs-lookup"><span data-stu-id="702f4-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="702f4-138">選取時**發行**按鈕在 Visual Studio 中，或從命令列發行：</span><span class="sxs-lookup"><span data-stu-id="702f4-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="702f4-139">會計算屬性/項目 (需要建置的檔案)。</span><span class="sxs-lookup"><span data-stu-id="702f4-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="702f4-140">僅限 Visual Studio：還原 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="702f4-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="702f4-141">(CLI 的使用者要明確還原。)</span><span class="sxs-lookup"><span data-stu-id="702f4-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="702f4-142">專案隨即建置。</span><span class="sxs-lookup"><span data-stu-id="702f4-142">The project builds.</span></span>
* <span data-ttu-id="702f4-143">會計算的發佈項目 (需要發佈的檔案)。</span><span class="sxs-lookup"><span data-stu-id="702f4-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="702f4-144">專案已發佈。</span><span class="sxs-lookup"><span data-stu-id="702f4-144">The project is published.</span></span> <span data-ttu-id="702f4-145">(計算的檔案會複製到發佈目的地。)</span><span class="sxs-lookup"><span data-stu-id="702f4-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="702f4-146">當 ASP.NET Core 專案參考`Microsoft.NET.Sdk.Web`在專案檔中， *app_offline.htm*檔案會放在 web 應用程式目錄的根目錄。</span><span class="sxs-lookup"><span data-stu-id="702f4-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="702f4-147">當檔案存在時，ASP.NET Core 模組會正常關閉應用程式，並在部署期間提供 *app_offline.htm* 檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="702f4-148">如需詳細資訊，請參閱 [ASP.NET Core 模組組態參考](xref:host-and-deploy/aspnet-core-module#appofflinehtm)。</span><span class="sxs-lookup"><span data-stu-id="702f4-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="702f4-149">基本命令列發行</span><span class="sxs-lookup"><span data-stu-id="702f4-149">Basic command-line publishing</span></span>

<span data-ttu-id="702f4-150">命令列發行適用於所有.NET Core 支援的平台上，而不需要 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="702f4-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="702f4-151">下列範例中[dotnet 發行](/dotnet/core/tools/dotnet-publish)從專案目錄執行命令 (其中包含*.csproj*檔案)。</span><span class="sxs-lookup"><span data-stu-id="702f4-151">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="702f4-152">如果不是在專案資料夾中，明確地傳入專案檔案路徑。</span><span class="sxs-lookup"><span data-stu-id="702f4-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="702f4-153">例如: </span><span class="sxs-lookup"><span data-stu-id="702f4-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="702f4-154">執行下列命令來建立及發佈 Web 應用程式：</span><span class="sxs-lookup"><span data-stu-id="702f4-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="702f4-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="702f4-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="702f4-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="702f4-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="702f4-157">[Dotnet 發行](/dotnet/core/tools/dotnet-publish)命令產生的輸出類似如下：</span><span class="sxs-lookup"><span data-stu-id="702f4-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="702f4-158">預設發佈資料夾是 `bin\$(Configuration)\netcoreapp<version>\publish`。</span><span class="sxs-lookup"><span data-stu-id="702f4-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="702f4-159">`$(Configuration)` 預設為偵錯。</span><span class="sxs-lookup"><span data-stu-id="702f4-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="702f4-160">在上例中，`<TargetFramework>` 是 `netcoreapp2.0`。</span><span class="sxs-lookup"><span data-stu-id="702f4-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="702f4-161">`dotnet publish -h` 顯示發佈的說明資訊。</span><span class="sxs-lookup"><span data-stu-id="702f4-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="702f4-162">下列命令會指定 `Release` 組建和發佈目錄：</span><span class="sxs-lookup"><span data-stu-id="702f4-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="702f4-163">[Dotnet 發行](/dotnet/core/tools/dotnet-publish)命令呼叫叫用 MSBuild`Publish`目標。</span><span class="sxs-lookup"><span data-stu-id="702f4-163">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="702f4-164">任何參數傳遞至`dotnet publish`傳遞至 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="702f4-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="702f4-165">`-c` 參數對應至 `Configuration` MSBuild 屬性。</span><span class="sxs-lookup"><span data-stu-id="702f4-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="702f4-166">`-o` 參數對應至 `OutputPath`。</span><span class="sxs-lookup"><span data-stu-id="702f4-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="702f4-167">MSBuild 屬性可以使用下列格式傳遞：</span><span class="sxs-lookup"><span data-stu-id="702f4-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="702f4-168">下列命令會將 `Release` 組建發佈到網路共用：</span><span class="sxs-lookup"><span data-stu-id="702f4-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="702f4-169">網路共用以正斜線 (*//r8/*) 指定，適用於所有 .NET Core 支援的平台。</span><span class="sxs-lookup"><span data-stu-id="702f4-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="702f4-170">確認部署的已發佈應用程式未在執行中。</span><span class="sxs-lookup"><span data-stu-id="702f4-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="702f4-171">當應用程式執行時，會鎖定 *publish* 資料夾中的檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="702f4-172">因為無法複製鎖定的檔案，所以無法執行部署。</span><span class="sxs-lookup"><span data-stu-id="702f4-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="702f4-173">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="702f4-173">Publish profiles</span></span>

<span data-ttu-id="702f4-174">本節會使用 Visual Studio 2017 及更高版本建立發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="702f4-175">一旦建立之後，從 Visual Studio 或命令列發行使用。</span><span class="sxs-lookup"><span data-stu-id="702f4-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="702f4-176">發行設定檔可簡化發佈程序。</span><span class="sxs-lookup"><span data-stu-id="702f4-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="702f4-177">多個發行設定檔可以存在。</span><span class="sxs-lookup"><span data-stu-id="702f4-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="702f4-178">若要在 Visual Studio 中建立的發行設定檔，以滑鼠右鍵按一下方案總管 中的專案，然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="702f4-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="702f4-179">或者，選取**發行\<專案名稱 >** [建置] 功能表。</span><span class="sxs-lookup"><span data-stu-id="702f4-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="702f4-180">隨即顯示應用程式容量頁面的 [發佈] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="702f4-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="702f4-181">如果專案不包含發行設定檔，則會顯示下列頁面：</span><span class="sxs-lookup"><span data-stu-id="702f4-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![顯示 Azure、 IIS、 FTB、 資料夾與 Azure 應用程式的容量為頁面的 [發行] 索引標籤選取。](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="702f4-184">選取 [資料夾] 時，[發佈] 按鈕會建立並發佈發行設定檔資料夾。</span><span class="sxs-lookup"><span data-stu-id="702f4-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![應用程式容量頁面的 [發佈]**** 索引標籤，顯示 Azure、IIS、FTB、資料夾。](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="702f4-186">一旦建立發行設定檔，**發行**索引標籤的變更，然後選取**建立新的設定檔**來建立新的設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![應用程式容量頁面 [發佈]**** 索引標籤：顯示在最後一個步驟中建立的 FolderProfile，以及建立新的設定檔連結。](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="702f4-188">發佈精靈支援下列發佈目標：</span><span class="sxs-lookup"><span data-stu-id="702f4-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="702f4-189">Microsoft Azure App Service</span><span class="sxs-lookup"><span data-stu-id="702f4-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="702f4-190">IIS、FTP 等等 (適用於任何網頁伺服器)</span><span class="sxs-lookup"><span data-stu-id="702f4-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="702f4-191">資料夾</span><span class="sxs-lookup"><span data-stu-id="702f4-191">Folder</span></span>
* <span data-ttu-id="702f4-192">匯入設定檔 （允許設定檔匯入）。</span><span class="sxs-lookup"><span data-stu-id="702f4-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="702f4-193">Microsoft Azure 虛擬機器</span><span class="sxs-lookup"><span data-stu-id="702f4-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="702f4-194">如需詳細資訊，請參閱[哪些發佈選項適合我？](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options)</span><span class="sxs-lookup"><span data-stu-id="702f4-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="702f4-195">使用 Visual Studio 中，建立的發行設定檔時*屬性/PublishProfiles/\<發行名稱 >.pubxml*建立 MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="702f4-196">這個 *.pubxml* 檔案是 MSBuild 檔案，而且包含發佈組態設定。</span><span class="sxs-lookup"><span data-stu-id="702f4-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="702f4-197">自訂建置和發佈程序，可以變更這個檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="702f4-198">發佈程序會讀取這個檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-198">This file is read by the publishing process.</span></span> <span data-ttu-id="702f4-199">`<LastUsedBuildConfiguration>` 是特殊，因為它是全域屬性，且不應該在組建中匯入任何檔案中。</span><span class="sxs-lookup"><span data-stu-id="702f4-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="702f4-200">如需詳細資訊，請參閱 [MSBuild：如何設定組態屬性](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx)。</span><span class="sxs-lookup"><span data-stu-id="702f4-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>

<span data-ttu-id="702f4-201">*.Pubxml*檔案不應該簽入原始檔控制，因為它相依於*.user*檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="702f4-202">*.user* 檔案絕對不能簽入原始檔控制，因為它可以包含機密資訊，而且只對一位使用者和一部電腦有效。</span><span class="sxs-lookup"><span data-stu-id="702f4-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="702f4-203">機密資訊 (例如發佈密碼) 是依每個使用者/電腦加密，且儲存在 *Properties/PublishProfiles/*\<發佈名稱>.pubxml.user 檔案中。</span><span class="sxs-lookup"><span data-stu-id="702f4-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="702f4-204">因為這個檔案可以包含機密資訊，所以它「不」應該簽入原始檔控制。</span><span class="sxs-lookup"><span data-stu-id="702f4-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="702f4-205">如需如何發行 ASP.NET core web 應用程式的概觀，請參閱[主機和部署](index.md)。</span><span class="sxs-lookup"><span data-stu-id="702f4-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="702f4-206">[主機和部署](index.md)是開放原始碼專案在https://github.com/aspnet/websdk。</span><span class="sxs-lookup"><span data-stu-id="702f4-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="702f4-207">`dotnet publish` 可以使用 MSDeploy，資料夾和[KUDU](https://github.com/projectkudu/kudu/wiki)發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="702f4-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="702f4-208">（適用於跨平台） 的資料夾： `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="702f4-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="702f4-209">MSDeploy （目前這僅適用於 windows 因為 MSDeploy 並不跨平台）： `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="702f4-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="702f4-210">MSDeploy 封裝 （目前這僅適用於 windows 因為 MSDeploy 並不跨平台）： `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="702f4-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="702f4-211">在先前範例中，**不**傳遞`deployonbuild`至`dotnet publish`。</span><span class="sxs-lookup"><span data-stu-id="702f4-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="702f4-212">如需詳細資訊，請參閱[Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)。</span><span class="sxs-lookup"><span data-stu-id="702f4-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="702f4-213">`dotnet publish` 支援使用 KUDU API 從任何平台發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="702f4-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="702f4-214">Visual Studio 發行的支援 KUDU Api，但它會受到 websdk 交叉 p 發行至 Azure。</span><span class="sxs-lookup"><span data-stu-id="702f4-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="702f4-215">搭配下列內容將發行設定檔新增至 *Properties/PublishProfiles* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="702f4-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="702f4-216">執行下列命令送達發行內容，並將它發行到 Azure 中使用 KUDU Api:</span><span class="sxs-lookup"><span data-stu-id="702f4-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="702f4-217">使用發行設定檔時，請設定下列 MSBuild 屬性：</span><span class="sxs-lookup"><span data-stu-id="702f4-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="702f4-218">發佈與名為設定檔時*FolderProfile*，可以執行下列命令之一：</span><span class="sxs-lookup"><span data-stu-id="702f4-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="702f4-219">叫用時[dotnet 組建](/dotnet/core/tools/dotnet-build)，它會呼叫`msbuild`來執行建置和發佈程序。</span><span class="sxs-lookup"><span data-stu-id="702f4-219">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="702f4-220">呼叫`dotnet build`或`msbuild`時，基本上等同資料夾設定檔中傳遞。</span><span class="sxs-lookup"><span data-stu-id="702f4-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="702f4-221">在 Windows 上直接呼叫 MSBuild 時，會使用 MSBuild 的.NET Framework 版本。</span><span class="sxs-lookup"><span data-stu-id="702f4-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="702f4-222">MSDeploy 目前僅限於 Windows 電腦進行發佈。</span><span class="sxs-lookup"><span data-stu-id="702f4-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="702f4-223">在非資料夾設定檔上呼叫 `dotnet build` 會叫用 MSBuild，而 MSBuild 會在非資料夾設定檔上使用 MSDeploy。</span><span class="sxs-lookup"><span data-stu-id="702f4-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="702f4-224">在非資料夾設定檔上呼叫 `dotnet build`，會叫用 MSBuild (使用 MSDeploy) 並造成失敗 (即使是在 Windows 平台上執行)。</span><span class="sxs-lookup"><span data-stu-id="702f4-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="702f4-225">若要發佈非資料夾的設定檔，請直接呼叫 MSBuild。</span><span class="sxs-lookup"><span data-stu-id="702f4-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="702f4-226">下列資料夾發行設定檔是以 Visual Studio 建立並發佈至網路共用：</span><span class="sxs-lookup"><span data-stu-id="702f4-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="702f4-227">請注意，`<LastUsedBuildConfiguration>` 設定為 `Release`。</span><span class="sxs-lookup"><span data-stu-id="702f4-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="702f4-228">從 Visual Studio 發佈時，會使用發佈程序開始時的值來設定 `<LastUsedBuildConfiguration>` 組態屬性值。</span><span class="sxs-lookup"><span data-stu-id="702f4-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="702f4-229">`<LastUsedBuildConfiguration>`組態屬性是特殊的且不應覆寫中的匯入的 MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="702f4-230">這個屬性可以覆寫從命令列。</span><span class="sxs-lookup"><span data-stu-id="702f4-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="702f4-231">例如: </span><span class="sxs-lookup"><span data-stu-id="702f4-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="702f4-232">使用 MSBuild：</span><span class="sxs-lookup"><span data-stu-id="702f4-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="702f4-233">從命令列發佈至 MSDeploy 端點</span><span class="sxs-lookup"><span data-stu-id="702f4-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="702f4-234">如先前所述，可以使用完成發行`dotnet publish`或`msbuild`命令。</span><span class="sxs-lookup"><span data-stu-id="702f4-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="702f4-235">`dotnet publish` 在 .NET Core 的內容中執行。</span><span class="sxs-lookup"><span data-stu-id="702f4-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="702f4-236">`msbuild`命令需要.NET framework，因此限於 Windows 環境。</span><span class="sxs-lookup"><span data-stu-id="702f4-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="702f4-237">以 MSDeploy 發佈最簡單的方式，是先在 Visual Studio 2017 中建立發行設定檔，再從命令列使用設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="702f4-238">在下列範例中，建立 ASP.NET Core web 應用程式 (使用`dotnet new mvc`)，並使用 Visual Studio 加入 Azure 發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="702f4-239">執行`msbuild`從**VS 2017 的開發人員命令提示字元**。</span><span class="sxs-lookup"><span data-stu-id="702f4-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="702f4-240">開發人員命令提示字元中具有正確*msbuild.exe*在其路徑與一些 MSBuild 在設定變數中。</span><span class="sxs-lookup"><span data-stu-id="702f4-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="702f4-241">MSBuild 使用下列語法：</span><span class="sxs-lookup"><span data-stu-id="702f4-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="702f4-242">取得`Password`從*\<發行名稱 >。PublishSettings*檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="702f4-243">下載*。PublishSettings*從檔案：</span><span class="sxs-lookup"><span data-stu-id="702f4-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="702f4-244">方案總管: 以滑鼠右鍵按一下 Web 應用程式，然後選取**下載發行設定檔**。</span><span class="sxs-lookup"><span data-stu-id="702f4-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="702f4-245">Azure 管理入口網站中： 選取 [**取得發行設定檔**的 Web 應用程式] 刀鋒視窗。</span><span class="sxs-lookup"><span data-stu-id="702f4-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="702f4-246">`Username` 可以在發行設定檔中找到。</span><span class="sxs-lookup"><span data-stu-id="702f4-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="702f4-247">下列範例使用「Web11112 - Web 部署」發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="702f4-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="702f4-248">排除檔案</span><span class="sxs-lookup"><span data-stu-id="702f4-248">Excluding files</span></span>

<span data-ttu-id="702f4-249">發佈 ASP.NET Core Web 應用程式時，會包含 *wwwroot* 資料夾的組建成品和內容。</span><span class="sxs-lookup"><span data-stu-id="702f4-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="702f4-250">`msbuild` 支援[通用慣例模式](https://gruntjs.com/configuring-tasks#globbing-patterns)。</span><span class="sxs-lookup"><span data-stu-id="702f4-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="702f4-251">例如，下列`<Content>`元素標記會排除所有的文字 (*.txt*) 從檔案*wwwroot/content*資料夾及其所有子資料夾。</span><span class="sxs-lookup"><span data-stu-id="702f4-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="702f4-252">上述標記可以新增至發行設定檔或 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="702f4-253">當新增至 *.csproj* 檔案時，此規則會新增至專案中的所有發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="702f4-254">下列 `<MsDeploySkipRules>` 項目標記會排除 *wwwroot/content* 資料夾中的所有檔案：</span><span class="sxs-lookup"><span data-stu-id="702f4-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="702f4-255">`<MsDeploySkipRules>` 不會刪除*略過*從部署站台的目標。</span><span class="sxs-lookup"><span data-stu-id="702f4-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="702f4-256">`<Content>` 目標的檔案與資料夾會從部署網站刪除。</span><span class="sxs-lookup"><span data-stu-id="702f4-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="702f4-257">例如，假設已部署的 web 應用程式有下列檔案：</span><span class="sxs-lookup"><span data-stu-id="702f4-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="702f4-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="702f4-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="702f4-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="702f4-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="702f4-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="702f4-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="702f4-261">如果下列`<MsDeploySkipRules>`新增標記，這些檔案不會刪除部署站台上。</span><span class="sxs-lookup"><span data-stu-id="702f4-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="702f4-262">`<MsDeploySkipRules>`如上所示的標記會防止*略過*防止 depoyed 檔案但不會刪除這些檔案，在部署之後。</span><span class="sxs-lookup"><span data-stu-id="702f4-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="702f4-263">下列`<Content>`標記刪除部署位置的目標的檔案：</span><span class="sxs-lookup"><span data-stu-id="702f4-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="702f4-264">使用命令列部署`<Content>`上述結果類似下列的輸出中的標記：</span><span class="sxs-lookup"><span data-stu-id="702f4-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="702f4-265">包括檔案</span><span class="sxs-lookup"><span data-stu-id="702f4-265">Including files</span></span>

<span data-ttu-id="702f4-266">下列標記包含*映像*資料夾要在專案目錄外的*wwwroot/影像*發佈站台資料夾的：</span><span class="sxs-lookup"><span data-stu-id="702f4-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="702f4-267">標記可以新增至 *.csproj* 檔案或發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="702f4-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="702f4-268">如果要加入至*.csproj*檔案，它是否包含在專案中的每個發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="702f4-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="702f4-269">下列反白顯示的標記會示範如何：</span><span class="sxs-lookup"><span data-stu-id="702f4-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="702f4-270">將專案外的檔案複製到 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="702f4-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="702f4-271">排除 *wwwroot\Content* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="702f4-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="702f4-272">排除 *Views\Home\About2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="702f4-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="702f4-273">如需更多的部署範例，請參閱 [WebSDK 讀我檔案](https://github.com/aspnet/websdk)。</span><span class="sxs-lookup"><span data-stu-id="702f4-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="702f4-274">在發佈之前或之後執行目標</span><span class="sxs-lookup"><span data-stu-id="702f4-274">Run a target before or after publishing</span></span>

<span data-ttu-id="702f4-275">內建`BeforePublish`和`AfterPublish`目標可以用來執行目標之前或之後發行的目標。</span><span class="sxs-lookup"><span data-stu-id="702f4-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="702f4-276">下列標記可以新增至發行設定檔，先將訊息記錄至主控台輸出再發佈：</span><span class="sxs-lookup"><span data-stu-id="702f4-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="702f4-277">發行至伺服器，使用不受信任的憑證</span><span class="sxs-lookup"><span data-stu-id="702f4-277">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="702f4-278">新增`<AllowUntrustedCertificate>`屬性值為`True`至發行設定檔：</span><span class="sxs-lookup"><span data-stu-id="702f4-278">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="702f4-279">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="702f4-279">The Kudu service</span></span>

<span data-ttu-id="702f4-280">若要檢視中的檔案的 Azure 應用程式服務 web 應用程式部署，使用[Kudu 服務](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service)。</span><span class="sxs-lookup"><span data-stu-id="702f4-280">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="702f4-281">附加`scm`權杖的 web 應用程式名稱。</span><span class="sxs-lookup"><span data-stu-id="702f4-281">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="702f4-282">例如: </span><span class="sxs-lookup"><span data-stu-id="702f4-282">For example:</span></span>

| <span data-ttu-id="702f4-283">URL</span><span class="sxs-lookup"><span data-stu-id="702f4-283">URL</span></span>                                    | <span data-ttu-id="702f4-284">結果</span><span class="sxs-lookup"><span data-stu-id="702f4-284">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="702f4-285">Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="702f4-285">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="702f4-286">Kudu 服務</span><span class="sxs-lookup"><span data-stu-id="702f4-286">Kudu sevice</span></span> |

<span data-ttu-id="702f4-287">選取[偵錯主控台](https://github.com/projectkudu/kudu/wiki/Kudu-console)功能表項目以檢視/編輯/刪除/新增檔案。</span><span class="sxs-lookup"><span data-stu-id="702f4-287">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="702f4-288">其他資源</span><span class="sxs-lookup"><span data-stu-id="702f4-288">Additional resources</span></span>

* <span data-ttu-id="702f4-289">[Web 部署](https://www.iis.net/downloads/microsoft/web-deploy)(MSDeploy) 可簡化部署的 web 應用程式和網站的 IIS 伺服器。</span><span class="sxs-lookup"><span data-stu-id="702f4-289">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="702f4-290">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues)： 檔案問題，並要求部署的功能。</span><span class="sxs-lookup"><span data-stu-id="702f4-290">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
