---
title: 使用 Visual Studio for Mac 將模型新增至 ASP.NET Core Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Visual Studio for Mac 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 97bc9f14b8d6da958a7f587e54a37d2d0e0aabd4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
ms.locfileid: "31483659"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-for-mac"></a><span data-ttu-id="ad6f4-103">使用 Visual Studio for Mac 將模型新增至 ASP.NET Core Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="ad6f4-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio for Mac</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="ad6f4-104">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="ad6f4-104">Add a data model</span></span>

* <span data-ttu-id="ad6f4-105">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="ad6f4-106">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="ad6f4-107">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="ad6f4-108">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="ad6f4-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="ad6f4-109">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="ad6f4-110">在中央窗格中選取 [空類別]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="ad6f4-111">將類別命名為 **Movie**，並選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="ad6f4-112">以滑鼠右鍵按一下紅色書寫行，例如 `services.AddDbContext<MovieContext>(options =>` 這行中的 `MovieContext`。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="ad6f4-113">選取 [快速檢修] > [using RazorPagesMovie.Models;]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="ad6f4-114">Visual Studio 會新增至 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="ad6f4-115">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-115">Build the project to verify you don't have any errors.</span></span>

![建立頁面](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="ad6f4-117">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="ad6f4-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="ad6f4-118">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="ad6f4-119">按一下 [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 連結以取得要使用的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-119">Click on the [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) link to get the version number to use.</span></span> <span data-ttu-id="ad6f4-120">若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-120">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="ad6f4-121">**注意：** 您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-121">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="ad6f4-122">若要編輯 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="ad6f4-122">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="ad6f4-123">選取 [檔案] > [開啟]，然後選取 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-123">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="ad6f4-124">選取 [選項]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-124">Select **Options**.</span></span>
* <span data-ttu-id="ad6f4-125">將 [開啟方式] 變更為 [原始程式碼編輯器]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-125">Change **Open with** to **Source Code Editor**.</span></span>

![編輯 csproj 檔案](model/csproj.png)

<span data-ttu-id="ad6f4-127">將 `Microsoft.EntityFrameworkCore.Tools.DotNet` 工具參考新增到第二個 **\<ItemGroup >**：</span><span class="sxs-lookup"><span data-stu-id="ad6f4-127">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**.:</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

<span data-ttu-id="ad6f4-128">顯示於下方程式碼中的版本號碼，在寫入時為正確。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-128">The version numbers shown in the following code were correct at the time of writing.</span></span>

[!INCLUDE [model3](../../includes/RP/model3.md)]

[!INCLUDE [model 4x](../../includes/RP/model4x.md)]

[!INCLUDE [model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE [model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="ad6f4-129">將 Pages/Movies 檔案新增至專案</span><span class="sxs-lookup"><span data-stu-id="ad6f4-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="ad6f4-130">在 Visual Studio 中，以滑鼠右鍵按一下 *Pages* 資料夾，然後選取 [新增] > [新增現有資料夾]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="ad6f4-131">選取 *Movies* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="ad6f4-132">在 [選擇要內含在此專案中的檔案] 對話方塊中，選取 [全部包含]。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-132">In the *Choose files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="ad6f4-133">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="ad6f4-133">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="ad6f4-134">[上一步：開始使用](xref:tutorials/razor-pages-mac/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages-mac/page)</span><span class="sxs-lookup"><span data-stu-id="ad6f4-134">[Previous: Get Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-mac/page)</span></span>
