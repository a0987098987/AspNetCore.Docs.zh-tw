---
title: 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Core (EF Core)，新增用來管理資料庫中電影的類別。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 07/27/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/razor-pages/model
ms.openlocfilehash: a288b454ac1b418ef0deacb3643be22d440cb938
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="add-a-model-to-a-razor-pages-app-in-aspnet-core"></a><span data-ttu-id="e2ae7-103">將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="e2ae7-103">Add a model to a Razor Pages app in ASP.NET Core</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="e2ae7-104">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="e2ae7-104">Add a data model</span></span>

<span data-ttu-id="e2ae7-105">在方案總管中，以滑鼠右鍵按一下 **RazorPagesMovie** 專案 > [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-105">In Solution Explorer, right-click the **RazorPagesMovie** project > **Add** > **New Folder**.</span></span> <span data-ttu-id="e2ae7-106">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-106">Name the folder *Models*.</span></span>

<span data-ttu-id="e2ae7-107">以滑鼠右鍵按一下 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-107">Right click the *Models* folder.</span></span> <span data-ttu-id="e2ae7-108">選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-108">Select **Add** > **Class**.</span></span> <span data-ttu-id="e2ae7-109">將類別命名為 **Movie** 並新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="e2ae7-109">Name the class **Movie** and add the following properties:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

<a name="cs"></a>
### <a name="add-a-database-connection-string"></a><span data-ttu-id="e2ae7-110">新增資料庫連線字串</span><span class="sxs-lookup"><span data-stu-id="e2ae7-110">Add a database connection string</span></span>

<span data-ttu-id="e2ae7-111">將連線字串新增到 *appsettings.json* 檔案中。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-111">Add a connection string to the *appsettings.json* file.</span></span>

[!code-json[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/appsettings.json?highlight=8-10)]

<a name="reg"></a>
###  <a name="register-the-database-context"></a><span data-ttu-id="e2ae7-112">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e2ae7-112">Register the database context</span></span>

<span data-ttu-id="e2ae7-113">以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-113">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices&highlight=3-5,7-9)]

<span data-ttu-id="e2ae7-114">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-114">Build the project to verify you don't have any errors.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling-and-perform-initial-migration"></a><span data-ttu-id="e2ae7-115">新增 Scaffold 工具並執行初始移轉</span><span class="sxs-lookup"><span data-stu-id="e2ae7-115">Add scaffold tooling and perform initial migration</span></span>

<span data-ttu-id="e2ae7-116">在本節中，您可以使用套件管理員主控台 (PMC) 進行下列作業：</span><span class="sxs-lookup"><span data-stu-id="e2ae7-116">In this section, you use the Package Manager Console (PMC) to:</span></span>

* <span data-ttu-id="e2ae7-117">新增 Visual Studio Web 程式碼產生套件。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-117">Add the Visual Studio web code generation package.</span></span> <span data-ttu-id="e2ae7-118">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-118">This package is required to run the scaffolding engine.</span></span>
* <span data-ttu-id="e2ae7-119">新增初始移轉。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-119">Add an initial migration.</span></span>
* <span data-ttu-id="e2ae7-120">以初始移轉更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-120">Update the database with the initial migration.</span></span>

<span data-ttu-id="e2ae7-121">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-121">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

  ![PMC 功能表](../first-mvc-app/adding-model/_static/pmc.png)

<span data-ttu-id="e2ae7-123">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="e2ae7-123">In the PMC, enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Add-Migration Initial
Update-Database
```

<span data-ttu-id="e2ae7-124">或者，您可以使用下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="e2ae7-124">Alternatively, the following .NET Core CLI commands can be used:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet ef migrations add Initial
dotnet ef database update
```

<span data-ttu-id="e2ae7-125">`Install-Package` 命令會安裝執行 Scaffolding 引擎所需的工具。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-125">The `Install-Package` command installs the tooling required to run the scaffolding engine.</span></span>

<span data-ttu-id="e2ae7-126">`Add-Migration` 命令會產生程式碼來建立初始資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-126">The `Add-Migration` command generates code to create the initial database schema.</span></span> <span data-ttu-id="e2ae7-127">結構描述是以 `DbContext` (位在 *Models/MovieContext.cs* 檔案中) 中指定的模型為基礎。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-127">The schema is based on the model specified in the `DbContext` (In the *Models/MovieContext.cs* file).</span></span> <span data-ttu-id="e2ae7-128">`Initial` 引數用來命名移轉。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-128">The `Initial` argument is used to name the migrations.</span></span> <span data-ttu-id="e2ae7-129">您可以使用任何名稱，但依照慣例，會選擇描述移轉的名稱。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-129">You can use any name, but by convention you choose a name that describes the migration.</span></span> <span data-ttu-id="e2ae7-130">如需詳細資訊，請參閱[移轉簡介](xref:data/ef-mvc/migrations#introduction-to-migrations)。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-130">See [Introduction to migrations](xref:data/ef-mvc/migrations#introduction-to-migrations) for more information.</span></span>

<span data-ttu-id="e2ae7-131">`Update-Database` 命令會執行 *Migrations/*\<時間戳記>_InitialCreate.cs 檔案中的 `Up` 方法，以建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-131">The `Update-Database` command runs the `Up` method in the *Migrations/\<time-stamp>_InitialCreate.cs* file, which creates the database.</span></span>

[!INCLUDE [model 4windows](../../includes/RP/model4Win.md)]

[!INCLUDE [model 4](../../includes/RP/model4tbl.md)]

<a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="e2ae7-132">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="e2ae7-132">Test the app</span></span>

* <span data-ttu-id="e2ae7-133">執行應用程式，並將 `/Movies` 附加至瀏覽器中的 URL (`http://localhost:port/movies`)。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-133">Run the app and append `/Movies` to the URL in the browser (`http://localhost:port/movies`).</span></span>
* <span data-ttu-id="e2ae7-134">測試 **Create** 連結。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-134">Test the **Create** link.</span></span>

  ![建立頁面](../../tutorials/razor-pages/model/_static/conan.png)

<a name="scaffold"></a>

* <span data-ttu-id="e2ae7-136">測試 **Edit**、**Details** 和 **Delete** 連結。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-136">Test the **Edit**, **Details**, and **Delete** links.</span></span>

<span data-ttu-id="e2ae7-137">如果您收到 SQL 例外狀況，請確認您已執行移轉並更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="e2ae7-137">If you get a SQL exception, verify you have run migrations and updated the database:</span></span>

<span data-ttu-id="e2ae7-138">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="e2ae7-138">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e2ae7-139">[上一步：開始使用](xref:tutorials/razor-pages/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages/page)</span><span class="sxs-lookup"><span data-stu-id="e2ae7-139">[Previous: Get Started](xref:tutorials/razor-pages/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages/page)</span></span>    
