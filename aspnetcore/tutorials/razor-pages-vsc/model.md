---
title: 使用 Visual Studio Code 將模型新增至 ASP.NET Core Razor 頁面應用程式
author: rick-anderson
description: 了解如何使用 Visual Studio Code 將模型新增至 ASP.NET Core 中的 Razor 頁面應用程式。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 08/27/2017
uid: tutorials/razor-pages-vsc/model
ms.openlocfilehash: c4aef369bb3965b70d1b461cf63e6f5a26a00628
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244719"
---
# <a name="add-a-model-to-an-aspnet-core-razor-pages-app-with-visual-studio-code"></a><span data-ttu-id="0742b-103">使用 Visual Studio Code 將模型新增至 ASP.NET Core Razor 頁面應用程式</span><span class="sxs-lookup"><span data-stu-id="0742b-103">Add a model to an ASP.NET Core Razor Pages app with Visual Studio Code</span></span>

[!INCLUDE [model1](../../includes/RP/model1.md)]

## <a name="add-a-data-model"></a><span data-ttu-id="0742b-104">新增資料模型</span><span class="sxs-lookup"><span data-stu-id="0742b-104">Add a data model</span></span>

* <span data-ttu-id="0742b-105">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="0742b-105">Add a folder named *Models*.</span></span>
* <span data-ttu-id="0742b-106">將類別新增至名為 *Movie.cs* 的 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="0742b-106">Add a class to the *Models* folder named *Movie.cs*.</span></span>
* <span data-ttu-id="0742b-107">將下列程式碼新增至 *Models/Movie.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="0742b-107">Add the following code to the *Models/Movie.cs* file:</span></span>

[!INCLUDE [model 2](../../includes/RP/model2.md)]

[!INCLUDE [model 2a](../../includes/RP/model2a.md)]

### <a name="entity-framework-core-nuget-package-for-sqlite"></a><span data-ttu-id="0742b-108">SQLite 的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="0742b-108">Entity Framework Core NuGet package for SQLite</span></span>

<span data-ttu-id="0742b-109">從命令列中，執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="0742b-109">From the command line, run the following .NET Core CLI command:</span></span>

```console
dotnet add package Microsoft.EntityFrameworkCore.SQLite
```

<a name="reg"></a>

### <a name="register-the-database-context"></a><span data-ttu-id="0742b-110">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="0742b-110">Register the database context</span></span>

<span data-ttu-id="0742b-111">以[相依性插入](xref:fundamentals/dependency-injection)容器在 *Startup.cs* 檔案中登錄資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="0742b-111">Register the database context with the [dependency injection](xref:fundamentals/dependency-injection) container in the *Startup.cs* file.</span></span>

[!code-csharp[](../../tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Startup.cs?name=snippet_ConfigureServices2&highlight=10-11)]

<span data-ttu-id="0742b-112">在 *Startup.cs* 最上方新增下列 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="0742b-112">Add the following `using` statements at the top of *Startup.cs*:</span></span>

```csharp
using RazorPagesMovie.Models;
using Microsoft.EntityFrameworkCore;
```

<span data-ttu-id="0742b-113">請建置專案，以確認您沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="0742b-113">Build the project to verify you don't have any errors.</span></span>

[!INCLUDE [model 3](../../includes/RP/model3.md)]

<a name="scaffold"></a>

### <a name="scaffold-the-movie-model"></a><span data-ttu-id="0742b-114">Scaffold 電影模型</span><span class="sxs-lookup"><span data-stu-id="0742b-114">Scaffold the Movie model</span></span>

* <span data-ttu-id="0742b-115">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="0742b-115">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="0742b-116">**針對 Windows**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0742b-116">**For Windows**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages\Movies --referenceScriptLibraries
  ```

* <span data-ttu-id="0742b-117">**針對 macOS 與 Linux**：執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="0742b-117">**For macOS and Linux**: Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Movie -dc MovieContext -udl -outDir Pages/Movies --referenceScriptLibraries
  ```

[!INCLUDE [model 4](../../includes/RP/model4.md)]

<span data-ttu-id="0742b-118">下一個教學課程說明 Scaffolding 所建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="0742b-118">The next tutorial explains the files created by scaffolding.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="0742b-119">[上一步：開始使用](xref:tutorials/razor-pages-vsc/razor-pages-start)
> [下一步：Scaffold Razor 頁面](xref:tutorials/razor-pages-vsc/page)</span><span class="sxs-lookup"><span data-stu-id="0742b-119">[Previous: Get Started](xref:tutorials/razor-pages-vsc/razor-pages-start)
[Next: Scaffolded Razor Pages](xref:tutorials/razor-pages-vsc/page)</span></span>
