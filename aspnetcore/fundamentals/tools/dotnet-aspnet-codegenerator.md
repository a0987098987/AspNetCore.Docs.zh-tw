---
title: dotnet aspnet-codegenerator 命令
author: rick-anderson
description: dotnet aspnet-codegenerator 命令會架起 ASP.NET Core 專案。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 07/04/2019
uid: fundamentals/tools/dotnet-aspnet-codegenerator
ms.openlocfilehash: 1043a578f66d5bb57f4a81e9fe21afa5e3c37cb8
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081501"
---
# <a name="dotnet-aspnet-codegenerator"></a><span data-ttu-id="2b1fe-103">dotnet aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="2b1fe-103">dotnet aspnet-codegenerator</span></span>

<span data-ttu-id="2b1fe-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2b1fe-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2b1fe-105">`dotnet aspnet-codegenerator` - 執行 ASP.NET Core Scaffolding 引擎。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-105">`dotnet aspnet-codegenerator` - Runs the ASP.NET Core scaffolding engine.</span></span> <span data-ttu-id="2b1fe-106">從命令列進行 Scaffolding 時才需要 `dotnet aspnet-codegenerator`，在 Visual Studio 中不需要進行 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-106">`dotnet aspnet-codegenerator` is only required to scaffold from the command line, it's not needed to use scaffolding with Visual Studio.</span></span>

<span data-ttu-id="2b1fe-107">本文適用於 [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) 與更新版本。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-107">This article applies to [.NET Core 2.1 SDK](https://dotnet.microsoft.com/download/dotnet-core/2.1) and later.</span></span>

## <a name="installing-aspnet-codegenerator"></a><span data-ttu-id="2b1fe-108">Installing aspnet-codegenerator</span><span class="sxs-lookup"><span data-stu-id="2b1fe-108">Installing aspnet-codegenerator</span></span>

<span data-ttu-id="2b1fe-109">`dotnet-aspnet-codegenerator` 是必須安裝的[全域工具](/dotnet/core/tools/global-tools)。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-109">`dotnet-aspnet-codegenerator` is a [global tool](/dotnet/core/tools/global-tools) that must be installed.</span></span> <span data-ttu-id="2b1fe-110">下列命令會安裝 `dotnet-aspnet-codegenerator` 工具的最新穩定版本：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-110">The following command installs the latest stable version of the `dotnet-aspnet-codegenerator` tool:</span></span>

```dotnetcli
dotnet tool install -g dotnet-aspnet-codegenerator
```

<span data-ttu-id="2b1fe-111">下列命令會將 `dotnet-aspnet-codegenerator` 更新到可從已安裝之 .NET Core SDK 中取得的最新穩定版本：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-111">The following command updates `dotnet-aspnet-codegenerator` to the latest stable version available from the installed .NET Core SDKs:</span></span>

```dotnetcli
dotnet tool update -g dotnet-aspnet-codegenerator
```

## <a name="synopsis"></a><span data-ttu-id="2b1fe-112">概要</span><span class="sxs-lookup"><span data-stu-id="2b1fe-112">Synopsis</span></span>

```
dotnet aspnet-codegenerator [arguments] [-p|--project] [-n|--nuget-package-dir] [-c|--configuration] [-tfm|--target-framework] [-b|--build-base-path] [--no-build] 
dotnet aspnet-codegenerator [-h|--help]
```

## <a name="description"></a><span data-ttu-id="2b1fe-113">說明</span><span class="sxs-lookup"><span data-stu-id="2b1fe-113">Description</span></span>

<span data-ttu-id="2b1fe-114">`dotnet aspnet-codegenerator` 全域命令會執行 ASP.NET Core 程式碼產生器與 Scaffolding 引擎。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-114">The `dotnet aspnet-codegenerator` global command runs the ASP.NET Core code generator and scaffolding engine.</span></span>

## <a name="arguments"></a><span data-ttu-id="2b1fe-115">引數</span><span class="sxs-lookup"><span data-stu-id="2b1fe-115">Arguments</span></span>

`generator`

<span data-ttu-id="2b1fe-116">要執行的程式碼產生器。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-116">The code generator to run.</span></span> <span data-ttu-id="2b1fe-117">可用產生器如下︰</span><span class="sxs-lookup"><span data-stu-id="2b1fe-117">The following generators are available:</span></span>

| <span data-ttu-id="2b1fe-118">Generator</span><span class="sxs-lookup"><span data-stu-id="2b1fe-118">Generator</span></span> | <span data-ttu-id="2b1fe-119">運算</span><span class="sxs-lookup"><span data-stu-id="2b1fe-119">Operation</span></span> |
| ----------------- | ------------ | 
| <span data-ttu-id="2b1fe-120">區域</span><span class="sxs-lookup"><span data-stu-id="2b1fe-120">area</span></span>      | [<span data-ttu-id="2b1fe-121">架起區域</span><span class="sxs-lookup"><span data-stu-id="2b1fe-121">Scaffolds an Area</span></span>](/aspnet/core/mvc/controllers/areas) |
  <span data-ttu-id="2b1fe-122">控制器</span><span class="sxs-lookup"><span data-stu-id="2b1fe-122">controller</span></span>| [<span data-ttu-id="2b1fe-123">架起控制器</span><span class="sxs-lookup"><span data-stu-id="2b1fe-123">Scaffolds a controller</span></span>](/aspnet/core/tutorials/first-mvc-app/adding-model) |
  <span data-ttu-id="2b1fe-124">識別</span><span class="sxs-lookup"><span data-stu-id="2b1fe-124">identity</span></span>  | [<span data-ttu-id="2b1fe-125">架起身分識別</span><span class="sxs-lookup"><span data-stu-id="2b1fe-125">Scaffolds Identity</span></span>](/aspnet/core/security/authentication/scaffold-identity) |
  <span data-ttu-id="2b1fe-126">razorpage</span><span class="sxs-lookup"><span data-stu-id="2b1fe-126">razorpage</span></span> | [<span data-ttu-id="2b1fe-127">架起 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="2b1fe-127">Scaffolds Razor Pages</span></span>](/aspnet/core/tutorials/razor-pages/model) |
  <span data-ttu-id="2b1fe-128">view</span><span class="sxs-lookup"><span data-stu-id="2b1fe-128">view</span></span>      | [<span data-ttu-id="2b1fe-129">架起檢視</span><span class="sxs-lookup"><span data-stu-id="2b1fe-129">Scaffolds a view</span></span>](/aspnet/core/mvc/views/overview) |

## <a name="options"></a><span data-ttu-id="2b1fe-130">選項</span><span class="sxs-lookup"><span data-stu-id="2b1fe-130">Options</span></span>

`-n|--nuget-package-dir`

<span data-ttu-id="2b1fe-131">指定 NuGet 套件目錄。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-131">Specifies the NuGet package directory.</span></span>

`-c|--configuration {Debug|Release}`

<span data-ttu-id="2b1fe-132">定義組建組態。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-132">Defines the build configuration.</span></span> <span data-ttu-id="2b1fe-133">預設值為 `Debug`。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-133">The default value is `Debug`.</span></span>

`-tfm|--target-framework`

<span data-ttu-id="2b1fe-134">要使用的目標 [Framework](/dotnet/standard/frameworks)。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-134">Target [Framework](/dotnet/standard/frameworks) to use.</span></span> <span data-ttu-id="2b1fe-135">例如： `net46` 。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-135">For example, `net46`.</span></span>

`-b|--build-base-path`

<span data-ttu-id="2b1fe-136">建置基底路徑。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-136">The build base path.</span></span>

`-h|--help`

<span data-ttu-id="2b1fe-137">印出命令的簡短說明。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-137">Prints out a short help for the command.</span></span>

`--no-build`

<span data-ttu-id="2b1fe-138">不會在執行前建置專案。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-138">Doesn't build the project before running.</span></span> <span data-ttu-id="2b1fe-139">選項也會隱含設定 `--no-restore` 旗標。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-139">It also implicitly sets the `--no-restore` flag.</span></span>

`-p|--project <PATH>`

<span data-ttu-id="2b1fe-140">指定要執行的專案檔路徑 (資料夾名稱或完整路徑)。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-140">Specifies the path of the project file to run (folder name or full path).</span></span> <span data-ttu-id="2b1fe-141">如果未指定，則會預設為目前目錄。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-141">If not specified, it defaults to the current directory.</span></span>

## <a name="generator-options"></a><span data-ttu-id="2b1fe-142">產生器選項</span><span class="sxs-lookup"><span data-stu-id="2b1fe-142">Generator options</span></span>

<span data-ttu-id="2b1fe-143">以下各節詳細說明支援之產生器的可用選項：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-143">The following sections detail the options available for the supported generators:</span></span>

* <span data-ttu-id="2b1fe-144">區域</span><span class="sxs-lookup"><span data-stu-id="2b1fe-144">Area</span></span>
* <span data-ttu-id="2b1fe-145">控制器</span><span class="sxs-lookup"><span data-stu-id="2b1fe-145">Controller</span></span>
* <span data-ttu-id="2b1fe-146">身分識別</span><span class="sxs-lookup"><span data-stu-id="2b1fe-146">Identity</span></span>  
* <span data-ttu-id="2b1fe-147">Razorpage</span><span class="sxs-lookup"><span data-stu-id="2b1fe-147">Razorpage</span></span>
* <span data-ttu-id="2b1fe-148">檢視</span><span class="sxs-lookup"><span data-stu-id="2b1fe-148">View</span></span>

<a name="area"></a>

### <a name="area-options"></a><span data-ttu-id="2b1fe-149">區域選項</span><span class="sxs-lookup"><span data-stu-id="2b1fe-149">Area options</span></span>

<span data-ttu-id="2b1fe-150">此工具旨在用於搭配控制器與檢視使用 ASP.NET Core Web 專案。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-150">This tool is intended for ASP.NET Core web projects with controllers and views.</span></span> <span data-ttu-id="2b1fe-151">它不是要用於 Razor Pages 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-151">It's not intended for Razor Pages apps.</span></span>

<span data-ttu-id="2b1fe-152">使用方式：`dotnet aspnet-codegenerator area AreaNameToGenerate`</span><span class="sxs-lookup"><span data-stu-id="2b1fe-152">Usage: `dotnet aspnet-codegenerator area AreaNameToGenerate`</span></span>

<span data-ttu-id="2b1fe-153">上述命令會產生下列資料夾：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-153">The preceding command generates the following folders:</span></span>

* <span data-ttu-id="2b1fe-154">*區域*</span><span class="sxs-lookup"><span data-stu-id="2b1fe-154">*Areas*</span></span>
  * <span data-ttu-id="2b1fe-155">*AreaNameToGenerate*</span><span class="sxs-lookup"><span data-stu-id="2b1fe-155">*AreaNameToGenerate*</span></span>
    * <span data-ttu-id="2b1fe-156">*控制器*</span><span class="sxs-lookup"><span data-stu-id="2b1fe-156">*Controllers*</span></span>
    * <span data-ttu-id="2b1fe-157">*Data*</span><span class="sxs-lookup"><span data-stu-id="2b1fe-157">*Data*</span></span>
    * <span data-ttu-id="2b1fe-158">*模型*</span><span class="sxs-lookup"><span data-stu-id="2b1fe-158">*Models*</span></span>
    * <span data-ttu-id="2b1fe-159">*檢視*</span><span class="sxs-lookup"><span data-stu-id="2b1fe-159">*Views*</span></span>

<a name="ctl"></a>

### <a name="controller-options"></a><span data-ttu-id="2b1fe-160">控制器選項</span><span class="sxs-lookup"><span data-stu-id="2b1fe-160">Controller options</span></span>

<span data-ttu-id="2b1fe-161">下表列出 `aspnet-codegenerator` `controller` 與 `razorpage` 的選項：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-161">The following table lists options for  `aspnet-codegenerator` `controller` and `razorpage`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="2b1fe-162">下表列出 `aspnet-codegenerator controller` 的專用選項：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-162">The following table lists options unique to  `aspnet-codegenerator controller`:</span></span>

| <span data-ttu-id="2b1fe-163">選項</span><span class="sxs-lookup"><span data-stu-id="2b1fe-163">Option</span></span>               | <span data-ttu-id="2b1fe-164">描述</span><span class="sxs-lookup"><span data-stu-id="2b1fe-164">Description</span></span>|
| ----------------- | ------------ |
| <span data-ttu-id="2b1fe-165">--controllerName 或 -name</span><span class="sxs-lookup"><span data-stu-id="2b1fe-165">--controllerName or -name</span></span> | <span data-ttu-id="2b1fe-166">控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-166">Name of the controller.</span></span> |
| <span data-ttu-id="2b1fe-167">--useAsyncActions 或 -async</span><span class="sxs-lookup"><span data-stu-id="2b1fe-167">--useAsyncActions or -async</span></span> | <span data-ttu-id="2b1fe-168">產生非同步控制器動作。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-168">Generate async controller actions.</span></span> |
| <span data-ttu-id="2b1fe-169">--noViews 或 -nv</span><span class="sxs-lookup"><span data-stu-id="2b1fe-169">--noViews or -nv</span></span> | <span data-ttu-id="2b1fe-170">**不**產生任何檢視。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-170">Generate **no** views.</span></span> |
| <span data-ttu-id="2b1fe-171">--restWithNoViews 或 -api</span><span class="sxs-lookup"><span data-stu-id="2b1fe-171">--restWithNoViews or -api</span></span>  | <span data-ttu-id="2b1fe-172">使用 REST 樣式 API 產生控制器。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-172">Generate a Controller with REST style API.</span></span> <span data-ttu-id="2b1fe-173">假設使用 `noViews` 且會忽略所有檢視相關選項。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-173">`noViews` is assumed and any view related options are ignored.</span></span> |
| <span data-ttu-id="2b1fe-174">--readWriteActions 或 -actions</span><span class="sxs-lookup"><span data-stu-id="2b1fe-174">--readWriteActions or -actions</span></span> | <span data-ttu-id="2b1fe-175">在不使用模型的情況下使用讀取/寫入動作產生控制器。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-175">Generate controller with read/write actions without a model.</span></span> |

<span data-ttu-id="2b1fe-176">使用 `-h` 參數取得 `aspnet-codegenerator controller` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-176">Use the `-h` switch for help on the `aspnet-codegenerator controller` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator controller -h
```

<span data-ttu-id="2b1fe-177">請參閱[架起電影模型](/aspnet/core/tutorials/razor-pages/model)以取得 `dotnet aspnet-codegenerator controller` 的範例。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-177">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator controller`.</span></span>

### <a name="razorpage"></a><span data-ttu-id="2b1fe-178">Razorpage</span><span class="sxs-lookup"><span data-stu-id="2b1fe-178">Razorpage</span></span>

<a name="rp"></a>

<span data-ttu-id="2b1fe-179">您可以透過指定新頁面名稱與要使用的範本來個別架起 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-179">Razor Pages can be individually scaffolded by specifying the name of the new page and the template to use.</span></span> <span data-ttu-id="2b1fe-180">支援的範本為：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-180">The supported templates are:</span></span>

* `Empty`
* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="2b1fe-181">例如，下列命令會使用「編輯」範本來產生 *MyEdit.cshtml* 與 *MyEdit.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-181">For example, the following command uses the Edit template to generate *MyEdit.cshtml* and *MyEdit.cshtml.cs*:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage MyEdit Edit -m Movie -dc RazorPagesMovieContext -outDir Pages/Movies
```

<span data-ttu-id="2b1fe-182">一般而言，不會指定範本與產生的檔案名稱，而且會建立下列範本：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-182">Typically, the template and generated file name is not specified, and the following templates are created:</span></span>

* `Create`
* `Edit`
* `Delete`
* `Details`
* `List`

<span data-ttu-id="2b1fe-183">下列表出 `aspnet-codegenerator` `razorpage` 與 `controller` 的選項：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-183">The following table lists options for  `aspnet-codegenerator` `razorpage` and `controller`:</span></span>

[!INCLUDE [aspnet-codegenerator-args-md.md](~/includes/aspnet-codegenerator-args-md.md)]

<span data-ttu-id="2b1fe-184">下表列出 `aspnet-codegenerator razorpage` 的專用選項：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-184">The following table lists options unique to  `aspnet-codegenerator razorpage`:</span></span>

| <span data-ttu-id="2b1fe-185">選項</span><span class="sxs-lookup"><span data-stu-id="2b1fe-185">Option</span></span>               | <span data-ttu-id="2b1fe-186">說明</span><span class="sxs-lookup"><span data-stu-id="2b1fe-186">Description</span></span>|
| ----------------- | ------------ |
|   <span data-ttu-id="2b1fe-187">--namespaceName 或 -namespace</span><span class="sxs-lookup"><span data-stu-id="2b1fe-187">--namespaceName or -namespace</span></span> | <span data-ttu-id="2b1fe-188">要用於產生之 PageModel 的命名空間名稱</span><span class="sxs-lookup"><span data-stu-id="2b1fe-188">The name of the namespace to use for the generated PageModel</span></span> |
| <span data-ttu-id="2b1fe-189">--partialView 或 -partial</span><span class="sxs-lookup"><span data-stu-id="2b1fe-189">--partialView or -partial</span></span> | <span data-ttu-id="2b1fe-190">產生部分檢視。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-190">Generate a partial view.</span></span> <span data-ttu-id="2b1fe-191">若指定此選項，會忽略版面配置選項 -l 與 -udl。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-191">Layout options -l and -udl are ignored if this is specified.</span></span> |
| <span data-ttu-id="2b1fe-192">--noPageModel 或 -npm</span><span class="sxs-lookup"><span data-stu-id="2b1fe-192">--noPageModel or -npm</span></span> | <span data-ttu-id="2b1fe-193">切換為不產生空白範本的 PageModel 類別</span><span class="sxs-lookup"><span data-stu-id="2b1fe-193">Switch to not generate a PageModel class for Empty template</span></span> |

<span data-ttu-id="2b1fe-194">使用 `-h` 參數取得 `aspnet-codegenerator razorpage` 命令的說明：</span><span class="sxs-lookup"><span data-stu-id="2b1fe-194">Use the `-h` switch for help on the `aspnet-codegenerator razorpage` command:</span></span>

```dotnetcli
dotnet aspnet-codegenerator razorpage -h
```

<span data-ttu-id="2b1fe-195">請參閱[架起電影模型](/aspnet/core/tutorials/razor-pages/model)以取得 `dotnet aspnet-codegenerator razorpage` 的範例。</span><span class="sxs-lookup"><span data-stu-id="2b1fe-195">See [Scaffold the movie model](/aspnet/core/tutorials/razor-pages/model) for an example of `dotnet aspnet-codegenerator razorpage`.</span></span>

### <a name="identity"></a><span data-ttu-id="2b1fe-196">身分識別</span><span class="sxs-lookup"><span data-stu-id="2b1fe-196">Identity</span></span>

<span data-ttu-id="2b1fe-197">[架起身分識別](/aspnet/core/security/authentication/scaffold-identity)</span><span class="sxs-lookup"><span data-stu-id="2b1fe-197">See [Scaffold Identity](/aspnet/core/security/authentication/scaffold-identity)</span></span>
