---
title: "將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式"
author: rick-anderson
description: "將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式"
keywords: "ASP.NET Core, Razor 頁面, Razor, MVC"
ms.author: riande
manager: wpickett
ms.date: 07/27/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/model
ms.openlocfilehash: 195128f670223f15e1679c51fb6354c1aa527c01
ms.sourcegitcommit: 3ba32b2b6425ed94604cb0f681db0d5bb5f8ad58
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="adding-a-model-to-a-razor-pages-app"></a><span data-ttu-id="0e88c-104">將模型新增至 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="0e88c-104">Adding a model to a Razor Pages app</span></span>

[!INCLUDE[model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0e88c-105">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="0e88c-105">Add a data model</span></span>

<span data-ttu-id="0e88c-106">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="0e88c-106">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="0e88c-107">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="0e88c-107">Name the folder *Models*.</span></span>

<span data-ttu-id="0e88c-108">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0e88c-108">Right click the *Models* folder.</span></span> <span data-ttu-id="0e88c-109">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="0e88c-109">Select **Add** > **Class**.</span></span> <span data-ttu-id="0e88c-110">將類別命名為 **Movie** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="0e88c-110">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE[model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="0e88c-111">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="0e88c-111">Add a database connection string</span></span>

<span data-ttu-id="0e88c-112">將連線字串新增到 *appsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="0e88c-112">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="0e88c-113">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="0e88c-113">Register the database context</span></span>

<span data-ttu-id="0e88c-114">以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="0e88c-114">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[Main](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-6)]

<span data-ttu-id="0e88c-115">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="0e88c-115">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="0e88c-116">新增 Scaffold 工具並執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="0e88c-116">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="0e88c-117">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="0e88c-117">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="0e88c-118">新增 Visual Studio Web 程式碼產生套件。</span><span class="sxs-lookup"><span data-stu-id="0e88c-118">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="0e88c-119">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="0e88c-119">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="0e88c-120">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="0e88c-120">Add an initial migration.</span></span>
* <span data-ttu-id="0e88c-121">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="0e88c-121">Update the database with the initial migration.</span></span>

<span data-ttu-id="0e88c-122">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="0e88c-122">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="0e88c-124">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="0e88c-124">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design -Version 2.0.0
Add-Migration Initial
Update-Database
```

<span data-ttu-id="0e88c-125">`Install-Package` 命令會安裝執行 Scaffolding 引擎所需的工具。</span><span class="sxs-lookup"><span data-stu-id="0e88c-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="0e88c-126">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="0e88c-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="0e88c-127">結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="0e88c-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="0e88c-128">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="0e88c-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="0e88c-129">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="0e88c-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="0e88c-130">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="0e88c-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="0e88c-131">`Update-Database` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="0e88c-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE[model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE[model 4](../../includes/RP/model4.md)]

<span data-ttu-id="0e88c-132">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="0e88c-132">The next tutorial explains the files created by scaffolding.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0e88c-133">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
[下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="0e88c-133">[Previous: Getting Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
