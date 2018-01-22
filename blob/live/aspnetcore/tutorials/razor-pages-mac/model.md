---
title: "使用 Visual Studio for Mac 將模型新增至 Razor 頁面應用程式"
author: rick-anderson
description: "使用 Visual Studio for Mac 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式"
ms.author: riande
manager: wpickett
ms.date: 08/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: 7b1b2d54e9c68b0a6f2b1355726d0d1cb484f69e
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="56d86-103">使用 Visual Studio for Mac 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="56d86-103">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="56d86-104">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="56d86-104">Add a data model</span></span>

* <span data-ttu-id="56d86-105">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="56d86-105">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="56d86-106">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="56d86-106">Name the folder *Models*.</span></span>
* <span data-ttu-id="56d86-107">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="56d86-107">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="56d86-108">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="56d86-108">In the **New File** dialog:</span></span>

  * <span data-ttu-id="56d86-109">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="56d86-109">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="56d86-110">在中央窗格中選取 [空類別]。</span><span class="sxs-lookup"><span data-stu-id="56d86-110">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="56d86-111">將類別命名為 **Movie**，並選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="56d86-111">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]

<span data-ttu-id="56d86-112">以滑鼠右鍵按一下紅色書寫行，例如 `services.AddDbContext<MovieContext>(options =>` 這行中的 `MovieContext`。</span><span class="sxs-lookup"><span data-stu-id="56d86-112">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="56d86-113">選取 [快速檢修] > [using RazorPagesMovie.Models;]。</span><span class="sxs-lookup"><span data-stu-id="56d86-113">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="56d86-114">Visual Studio 會新增至 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="56d86-114">Visual studio adds the using statement.</span></span>

<span data-ttu-id="56d86-115">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="56d86-115">Build the project to verify you don't have any errors.</span></span>

![建立頁面](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="56d86-117">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="56d86-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="56d86-118">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="56d86-118">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="56d86-119">若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合。</span><span class="sxs-lookup"><span data-stu-id="56d86-119">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="56d86-120">**注意：**您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。</span><span class="sxs-lookup"><span data-stu-id="56d86-120">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="56d86-121">若要編輯 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="56d86-121">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="56d86-122">選取 [檔案] > [開啟]，然後選取 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="56d86-122">Select **File** > **Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="56d86-123">選取 [選項]。</span><span class="sxs-lookup"><span data-stu-id="56d86-123">Select **Options**.</span></span>
* <span data-ttu-id="56d86-124">將 [開啟方式] 變更為 [原始程式碼編輯器]。</span><span class="sxs-lookup"><span data-stu-id="56d86-124">Change **Open with** to **Source Code Editor**.</span></span>

![編輯 csproj 檔案](model/csproj.png)

<span data-ttu-id="56d86-126">將 `Microsoft.EntityFrameworkCore.Tools.DotNet` 工具參考新增到第二個 **\<ItemGroup >**：</span><span class="sxs-lookup"><span data-stu-id="56d86-126">Add the `Microsoft.EntityFrameworkCore.Tools.DotNet` tool reference to the second **\<ItemGroup>**:</span></span>

[!code-xml[Main](../../tutorials/razor-pages/razor-pages-start/snapshot_cli_sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?range=12-16&highlight=4)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="56d86-127">將 Pages/Movies 檔案新增至專案</span><span class="sxs-lookup"><span data-stu-id="56d86-127">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="56d86-128">在 Visual Studio 中，以滑鼠右鍵按一下 *Pages* 資料夾，然後選取 [新增] > [新增現有資料夾]。</span><span class="sxs-lookup"><span data-stu-id="56d86-128">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="56d86-129">選取 *Movies* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="56d86-129">Select the *Movies* folder.</span></span>
* <span data-ttu-id="56d86-130">在 [選擇要內含在此專案中的檔案] 對話方塊中，選取 [全部包含]。</span><span class="sxs-lookup"><span data-stu-id="56d86-130">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="56d86-131">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="56d86-131">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="56d86-132">[上一步：開始使用](xref:tutorials/razor-pages-mac/razor-pages-start)
[下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="56d86-132">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
