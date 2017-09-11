---
title: "使用 Visual Studio for Mac 將模型新增至 Razor 頁面應用程式"
author: rick-anderson
description: "使用 Visual Studio for Mac 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式"
keywords: "ASP.NET Core, Razor 頁面, Razor,MVC, 模型"
ms.author: riande
manager: wpickett
ms.date: 8/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages-mac/model
ms.openlocfilehash: b234eb93fbca1f4c83712990712b86e9941968fd
ms.sourcegitcommit: d9ec19e5452af83648074db5d96c0a0f4f9e7f9a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/29/2017
---
# <a name="adding-a-model-to-a-razor-pages-app-in-aspnet-core-with-visual-studio-for-mac"></a><span data-ttu-id="f94d8-104">使用 Visual Studio for Mac 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="f94d8-104">Adding a model to a Razor Pages app in ASP.NET Core with Visual Studio for Mac</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="f94d8-105">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="f94d8-105">Add a data model</span></span>

* <span data-ttu-id="f94d8-106">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案，然後選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-106">In Solution Explorer, right-click the **RazorPagesMovie** project, and then select **Add** > **New Folder**.</span></span> <span data-ttu-id="f94d8-107">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f94d8-107">Name the folder *Models*.</span></span>
* <span data-ttu-id="f94d8-108">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-108">Right-click the *Models* folder, and then select **Add** > **New File**.</span></span>
* <span data-ttu-id="f94d8-109">在 [新增檔案] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f94d8-109">In the **New File** dialog:</span></span>

  * <span data-ttu-id="f94d8-110">在左窗格中選取 [一般]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-110">Select **General** in the left pane.</span></span>
  * <span data-ttu-id="f94d8-111">在中央窗格中選取 [空類別]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-111">Select **Empty Class** in the center pain.</span></span>
  * <span data-ttu-id="f94d8-112">將類別命名為 **Movie**，並選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-112">Name the class **Movie** and select **New**.</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]
[!INCLUDE[model 2a](../../includes/RP/model2a.md)]

<span data-ttu-id="f94d8-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span><span class="sxs-lookup"><span data-stu-id="f94d8-113">[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=3-6)]</span></span>

<span data-ttu-id="f94d8-114">以滑鼠右鍵按一下紅色書寫行，例如 `services.AddDbContext<MovieContext>(options =>` 這行中的 `MovieContext`。</span><span class="sxs-lookup"><span data-stu-id="f94d8-114">Right click on a red squiggly line, for example `MovieContext` in the line `services.AddDbContext<MovieContext>(options =>`.</span></span> <span data-ttu-id="f94d8-115">選取 [快速檢修] > [using RazorPagesMovie.Models;]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-115">Select **Quick Fix > using RazorPagesMovie.Models;**.</span></span> <span data-ttu-id="f94d8-116">Visual Studio 會新增至 using 陳述式。</span><span class="sxs-lookup"><span data-stu-id="f94d8-116">Visual studio adds the using statement.</span></span>

<span data-ttu-id="f94d8-117">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="f94d8-117">Build the project to verify you don't have any errors.</span></span>

![建立頁面](model/red.png)

### <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="f94d8-119">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="f94d8-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="f94d8-120">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="f94d8-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="f94d8-121">若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合。</span><span class="sxs-lookup"><span data-stu-id="f94d8-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file.</span></span> <span data-ttu-id="f94d8-122">**注意：**您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。</span><span class="sxs-lookup"><span data-stu-id="f94d8-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span>

<span data-ttu-id="f94d8-123">若要編輯 *.csproj* 檔案：</span><span class="sxs-lookup"><span data-stu-id="f94d8-123">To edit a *.csproj* file:</span></span>

* <span data-ttu-id="f94d8-124">選取 [檔案] > [開啟]，然後選取 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f94d8-124">Select **File > Open**, and then select the *.csproj* file.</span></span>
* <span data-ttu-id="f94d8-125">選取 [選項]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-125">Select **Options**.</span></span>
* <span data-ttu-id="f94d8-126">將 [開啟方式] 變更為 [原始程式碼編輯器]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-126">Change **Open with** to **Source Code Editor**.</span></span>

![編輯 csproj 檔案](model/csproj.png)

<span data-ttu-id="f94d8-128">下列程式碼顯示已更新的 *csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="f94d8-128">The following code shows the updated *csproj* file.</span></span>

[!code-xml[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/RazorPagesMovie.cli.csproj?highlight=10)]

[!INCLUDE[model3](../../includes/RP/model3.md)]
[!INCLUDE[model 4x](../../includes/RP/model4x.md)]

[!INCLUDE[model 4 exit](../../includes/RP/model4exit.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

### <a name="add-the-pagesmovies-files-to-the-project"></a><span data-ttu-id="f94d8-129">將 Pages/Movies 檔案新增至專案</span><span class="sxs-lookup"><span data-stu-id="f94d8-129">Add the Pages/Movies files to the project</span></span>

* <span data-ttu-id="f94d8-130">在 Visual Studio 中，以滑鼠右鍵按一下 *Pages* 資料夾，然後選取 [新增] > [新增現有資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-130">In Visual Studio, Right-click the *Pages* folder and select **Add > Add existing Folder**.</span></span>
* <span data-ttu-id="f94d8-131">選取 *Movies* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f94d8-131">Select the *Movies* folder.</span></span>
* <span data-ttu-id="f94d8-132">在 [選擇要內含在此專案中的檔案] 對話方塊中，選取 [全部包含]。</span><span class="sxs-lookup"><span data-stu-id="f94d8-132">In the *Chosse files to include in the project* dialog, select **Include All**.</span></span>

<span data-ttu-id="f94d8-133">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="f94d8-133">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="f94d8-134">[上一步：開始使用](xref:tutorials/razor-pages-mac/razor-pages-start)
[下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="f94d8-134">[Previous: Getting Started](xref:tutorials/razor-pages-mac/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>
