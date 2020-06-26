---
title: dotnet aspnet-codegenerator 命令
author: rick-anderson
description: dotnet aspnet-codegenerator 命令會架起 ASP.NET Core 專案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: a106654c8a37e84e9186a2f06d90605df753e8a7
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405597"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="60d11-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="60d11-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="60d11-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="60d11-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="60d11-105">`dotnet aspnet-codegenerator` - 執行 ASP.NET Core Scaffolding 引擎。</span><span class="sxs-lookup"><span data-stu-id="60d11-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="60d11-106">從命令列進行 Scaffolding 時才需要 `dotnet aspnet-codegenerator`，在 Visual Studio 中不需要進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="60d11-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not needed to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="60d11-107">本文適用於 [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) 與更新版本。</span><span class="sxs-lookup"><span data-stu-id="60d11-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="60d11-108">Installing aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="60d11-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="60d11-109">`dotnet-aspnet-codegenerator` 是必須安裝的[全域工具](/dotnet/core/tools/global-tools)。</span><span class="sxs-lookup"><span data-stu-id="60d11-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="60d11-110">下列命令會安裝 `dotnet-aspnet-codegenerator` 工具的最新穩定版本：</span><span class="sxs-lookup"><span data-stu-id="60d11-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="60d11-111">下列命令會將 `dotnet-aspnet-codegenerator` 更新到可從已安裝之 .NET Core SDK 中取得的最新穩定版本：</span><span class="sxs-lookup"><span data-stu-id="60d11-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```dotnetcli
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="60d11-112">概要</span><span class="sxs-lookup"><span data-stu-id="60d11-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="60d11-113">說明</span><span class="sxs-lookup"><span data-stu-id="60d11-113">Description</span></span>

<span data-ttu-id="60d11-114">`dotnet aspnet-codegenerator` 全域命令會執行 ASP.NET Core 程式碼產生器與 Scaffolding 引擎。</span><span class="sxs-lookup"><span data-stu-id="60d11-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="60d11-115">引數</span><span class="sxs-lookup"><span data-stu-id="60d11-115">Arguments</span></span>

`generator`

<span data-ttu-id="60d11-116">要執行的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="60d11-116">The code generator to run.</span></span> <span data-ttu-id="60d11-117">可用產生器如下︰</span><span class="sxs-lookup"><span data-stu-id="60d11-117">The following generators are available:</span></span>

| <span data-ttu-id="60d11-118">產生器</span><span class="sxs-lookup"><span data-stu-id="60d11-118">Generator</span></span> | <span data-ttu-id="60d11-119">作業</span><span class="sxs-lookup"><span data-stu-id="60d11-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="60d11-120">區域</span><span class="sxs-lookup"><span data-stu-id="60d11-120">area</span></span>      | [<span data-ttu-id="60d11-121">架起區域</span><span class="sxs-lookup"><span data-stu-id="60d11-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="60d11-122">controller</span><span class="sxs-lookup"><span data-stu-id="60d11-122">controller</span></span>| [<span data-ttu-id="60d11-123">架起控制器</span><span class="sxs-lookup"><span data-stu-id="60d11-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="60d11-124">身分識別</span><span class="sxs-lookup"><span data-stu-id="60d11-124">identity</span></span>  | <span data-ttu-id="60d11-125">[ScaffoldIdentity](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="60d11-125">[Scaffolds Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span> |
  <span data-ttu-id="60d11-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="60d11-126">razorpage</span></span> | <span data-ttu-id="60d11-127">[Scaffold Razor 頁面](/aspnet/core/tutorials/razor-pages/model)</span><span class="sxs-lookup"><span data-stu-id="60d11-127">[Scaffolds Razor Pages](/aspnet/core/tutorials/razor-pages/model)</span></span> |
  <span data-ttu-id="60d11-128">檢視</span><span class="sxs-lookup"><span data-stu-id="60d11-128">view</span></span>      | [<span data-ttu-id="60d11-129">架起檢視</span><span class="sxs-lookup"><span data-stu-id="60d11-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="60d11-130">選項</span><span class="sxs-lookup"><span data-stu-id="60d11-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="60d11-131">指定 NuGet 套件目錄。</span><span class="sxs-lookup"><span data-stu-id="60d11-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="60d11-132">定義組建組態。</span><span class="sxs-lookup"><span data-stu-id="60d11-132">Defines the build configuration.</span></span> <span data-ttu-id="60d11-133">預設值是 `Debug`。</span><span class="sxs-lookup"><span data-stu-id="60d11-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="60d11-134">要使用的目標 [Framework](/dotnet/standard/frameworks)。</span><span class="sxs-lookup"><span data-stu-id="60d11-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="60d11-135">例如： `net46` 。</span><span class="sxs-lookup"><span data-stu-id="60d11-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="60d11-136">建置基底路徑。</span><span class="sxs-lookup"><span data-stu-id="60d11-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="60d11-137">印出命令的簡短說明。</span><span class="sxs-lookup"><span data-stu-id="60d11-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="60d11-138">不會在執行前建置專案。</span><span class="sxs-lookup"><span data-stu-id="60d11-138">Doesn't build the project before running.</span></span> <span data-ttu-id="60d11-139">選項也會隱含設定 `--no-restore` 旗標。</span><span class="sxs-lookup"><span data-stu-id="60d11-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="60d11-140">指定要執行的專案檔路徑 (資料夾名稱或完整路徑)。</span><span class="sxs-lookup"><span data-stu-id="60d11-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="60d11-141">如果未指定，則會預設為目前目錄。</span><span class="sxs-lookup"><span data-stu-id="60d11-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="60d11-142">產生器選項</span><span class="sxs-lookup"><span data-stu-id="60d11-142">Generator options</span></span>

<span data-ttu-id="60d11-143">以下各節詳細說明支援之產生器的可用選項：</span><span class="sxs-lookup"><span data-stu-id="60d11-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="60d11-144">區域</span><span class="sxs-lookup"><span data-stu-id="60d11-144">Area</span></span>
* <span data-ttu-id="60d11-145">控制器</span><span class="sxs-lookup"><span data-stu-id="60d11-145">Controller</span></span>
* Identity  
* <span data-ttu-id="60d11-146">Razorpage</span><span class="sxs-lookup"><span data-stu-id="60d11-146">Razorpage</span></span>
* <span data-ttu-id="60d11-147">檢視</span><span class="sxs-lookup"><span data-stu-id="60d11-147">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="60d11-148">區域選項</span><span class="sxs-lookup"><span data-stu-id="60d11-148">Area options</span></span>

<span data-ttu-id="60d11-149">此工具旨在用於搭配控制器與檢視使用 ASP.NET Core Web 專案。</span><span class="sxs-lookup"><span data-stu-id="60d11-149">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="60d11-150">並非適用于 Razor 頁面應用程式。</span><span class="sxs-lookup"><span data-stu-id="60d11-150">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="60d11-151">使用方式：`dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="60d11-151">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="60d11-152">上述命令會產生下列資料夾：</span><span class="sxs-lookup"><span data-stu-id="60d11-152">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="60d11-153">*區域*</span><span class="sxs-lookup"><span data-stu-id="60d11-153">*Areas*</span></span>
  * <span data-ttu-id="60d11-154">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="60d11-154">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="60d11-155">*控制器*</span><span class="sxs-lookup"><span data-stu-id="60d11-155">*Controllers*</span></span>
    * <span data-ttu-id="60d11-156">*Data*</span><span class="sxs-lookup"><span data-stu-id="60d11-156">*Data*</span></span>
    * <span data-ttu-id="60d11-157">*模型*</span><span class="sxs-lookup"><span data-stu-id="60d11-157">*Models*</span></span>
    * <span data-ttu-id="60d11-158">*檢視*</span><span class="sxs-lookup"><span data-stu-id="60d11-158">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="60d11-159">控制器選項</span><span class="sxs-lookup"><span data-stu-id="60d11-159">Controller options</span></span>

<span data-ttu-id="60d11-160">下表列出和的選項 `aspnet-codegenerator` `controller` `razorpage` ：</span><span class="sxs-lookup"><span data-stu-id="60d11-160">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="60d11-161">下表列出 `aspnet-codegenerator controller` 的專用選項：</span><span class="sxs-lookup"><span data-stu-id="60d11-161">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="60d11-162">選項</span><span class="sxs-lookup"><span data-stu-id="60d11-162">Option</span></span>               | <span data-ttu-id="60d11-163">說明</span><span class="sxs-lookup"><span data-stu-id="60d11-163">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="60d11-164">--controllerName 或 -name</span><span class="sxs-lookup"><span data-stu-id="60d11-164">--controllerName or -name</span></span> | <span data-ttu-id="60d11-165">控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="60d11-165">Name of the controller.</span></span> |
| <span data-ttu-id="60d11-166">--useAsyncActions 或 -async</span><span class="sxs-lookup"><span data-stu-id="60d11-166">--useAsyncActions or -async</span></span> | <span data-ttu-id="60d11-167">產生非同步控制器動作。</span><span class="sxs-lookup"><span data-stu-id="60d11-167">Generate async controller actions.</span></span> |
| <span data-ttu-id="60d11-168">--noViews 或 -nv</span><span class="sxs-lookup"><span data-stu-id="60d11-168">--noViews or -nv</span></span> | <span data-ttu-id="60d11-169">**不**產生任何檢視。</span><span class="sxs-lookup"><span data-stu-id="60d11-169">Generate **no** views.</span></span> |
| <span data-ttu-id="60d11-170">--restWithNoViews 或 -api</span><span class="sxs-lookup"><span data-stu-id="60d11-170">--restWithNoViews or -api</span></span>  | <span data-ttu-id="60d11-171">使用 REST 樣式 API 產生控制器。</span><span class="sxs-lookup"><span data-stu-id="60d11-171">Generate a Controller with REST style API.</span></span> <span data-ttu-id="60d11-172">假設使用 `noViews` 且會忽略所有檢視相關選項。</span><span class="sxs-lookup"><span data-stu-id="60d11-172">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="60d11-173">--readWriteActions 或 -actions</span><span class="sxs-lookup"><span data-stu-id="60d11-173">--readWriteActions or -actions</span></span> | <span data-ttu-id="60d11-174">在不使用模型的情況下使用讀取/寫入動作產生控制器。</span><span class="sxs-lookup"><span data-stu-id="60d11-174">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="60d11-175">使用 `-h` 參數取得 `aspnet-codegenerator controller` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="60d11-175">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="60d11-176">請參閱[架起電影模型](/aspnet/core/tutorials/razor-pages/model)以取得 `dotnet aspnet-codegenerator controller` 的範例。</span><span class="sxs-lookup"><span data-stu-id="60d11-176">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="60d11-177">Razorpage</span><span class="sxs-lookup"><span data-stu-id="60d11-177">Razorpage</span></span>

<a name="rp"></a>

Razor<span data-ttu-id="60d11-178">您可以藉由指定要使用的新頁面名稱和範本來個別 scaffold 頁面。</span><span class="sxs-lookup"><span data-stu-id="60d11-178"> Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="60d11-179">支援的範本為：</span><span class="sxs-lookup"><span data-stu-id="60d11-179">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="60d11-180">例如，下列命令會使用「編輯」範本來產生 *MyEdit.cshtml* 與 *MyEdit.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="60d11-180">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="60d11-181">一般而言，不會指定範本與產生的檔案名稱，而且會建立下列範本：</span><span class="sxs-lookup"><span data-stu-id="60d11-181">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="60d11-182">下表列出和的選項 `aspnet-codegenerator` `razorpage` `controller` ：</span><span class="sxs-lookup"><span data-stu-id="60d11-182">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="60d11-183">下表列出 `aspnet-codegenerator razorpage` 的專用選項：</span><span class="sxs-lookup"><span data-stu-id="60d11-183">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="60d11-184">選項</span><span class="sxs-lookup"><span data-stu-id="60d11-184">Option</span></span>               | <span data-ttu-id="60d11-185">說明</span><span class="sxs-lookup"><span data-stu-id="60d11-185">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="60d11-186">--namespaceName 或 -namespace</span><span class="sxs-lookup"><span data-stu-id="60d11-186">--namespaceName or -namespace</span></span> | <span data-ttu-id="60d11-187">要用於產生之 PageModel 的命名空間名稱</span><span class="sxs-lookup"><span data-stu-id="60d11-187">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="60d11-188">--partialView 或 -partial</span><span class="sxs-lookup"><span data-stu-id="60d11-188">--partialView or -partial</span></span> | <span data-ttu-id="60d11-189">產生部分檢視。</span><span class="sxs-lookup"><span data-stu-id="60d11-189">Generate a partial view.</span></span> <span data-ttu-id="60d11-190">若指定此選項，會忽略版面配置選項 -l 與 -udl。</span><span class="sxs-lookup"><span data-stu-id="60d11-190">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="60d11-191">--noPageModel 或 -npm</span><span class="sxs-lookup"><span data-stu-id="60d11-191">--noPageModel or -npm</span></span> | <span data-ttu-id="60d11-192">切換為不產生空白範本的 PageModel 類別</span><span class="sxs-lookup"><span data-stu-id="60d11-192">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="60d11-193">使用 `-h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="60d11-193">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="60d11-194">請參閱[架起電影模型](/aspnet/core/tutorials/razor-pages/model)以取得 `dotnet aspnet-codegenerator razorpage` 的範例。</span><span class="sxs-lookup"><span data-stu-id="60d11-194">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### Identity

<span data-ttu-id="60d11-195">請[參閱 Identity Scaffold](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="60d11-195">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
