---
title: "ASP.NET Core MVC EF 核心--4 的移轉 10"
author: tdykstra
description: "在本教學課程中，您啟動管理資料模型變更 ASP.NET Core MVC 應用程式中的使用 EF 核心移轉功能。"
keywords: "ASP.NET Core,Entity Framework Core,移轉"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 81f6c9c2-a819-4f3a-97a4-4b0503b56c26
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/migrations
ms.openlocfilehash: 20b05801ac666feef29fd05dd3e4738b1bd50b86
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="migrations---ef-core-with-aspnet-core-mvc-tutorial-4-of-10"></a><span data-ttu-id="2d5ec-104">移轉的 EF Core 與 ASP.NET Core MVC 教學課程 (10-4)</span><span class="sxs-lookup"><span data-stu-id="2d5ec-104">Migrations - EF Core with ASP.NET Core MVC tutorial (4 of 10)</span></span>

<span data-ttu-id="2d5ec-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2d5ec-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="2d5ec-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="2d5ec-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="2d5ec-108">在本教學課程中，您啟動管理資料模型變更使用的 EF 核心移轉功能。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-108">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="2d5ec-109">在之後的教學課程，您可以加入更多的移轉作業，當您變更資料模型。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-109">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="2d5ec-110">移轉的簡介</span><span class="sxs-lookup"><span data-stu-id="2d5ec-110">Introduction to migrations</span></span>

<span data-ttu-id="2d5ec-111">當您開發新的應用程式時，您的資料模型變更常見問題，以及每次將模型變更時，它會取得與資料庫同步處理。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-111">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="2d5ec-112">您可以設定 Entity Framework 來建立資料庫，如果不存在，以啟動這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-112">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="2d5ec-113">然後每次您變更資料模型-加入、 移除或變更實體類別或變更您的 DbContext 類別-您可以刪除資料庫，而且 EF 會建立一個新符合模型，並設測試資料。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-113">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="2d5ec-114">讓資料庫保持同步資料模型的這個方法適用於也直到您將部署到生產環境應用程式。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-114">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="2d5ec-115">它通常會儲存您想要保留，且您不想遺失的所有項目每次資料的生產環境中執行應用程式時您變更例如加入新的資料行。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-115">When the application is running in production it is usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="2d5ec-116">EF 核心移轉功能來解決這個問題，進而 EF 更新資料庫結構描述，而不是建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-116">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="2d5ec-117">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="2d5ec-117">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="2d5ec-118">若要使用移轉，您可以使用**Package Manager Console** (PMC) 或命令列介面 (CLI)。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-118">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="2d5ec-119">這些教學課程會示範如何使用 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-119">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="2d5ec-120">Pmc 依存的相關資訊位於[本教學課程結尾](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-120">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="2d5ec-121">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-121">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="2d5ec-122">若要安裝此套件，將它加入`DotNetCliToolReference`集合*.csproj*檔案，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-122">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="2d5ec-123">**注意：**您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-123">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="2d5ec-124">您可以編輯*.csproj*檔案中的專案名稱上按一下滑鼠右鍵**方案總管 中**，然後選取**編輯 ContosoUniversity.csproj**。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-124">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="2d5ec-125">（在此範例中的版本號碼是目前寫入教學課程時）。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-125">(The version numbers in this example were current when the tutorial was written.)</span></span> 

## <a name="change-the-connection-string"></a><span data-ttu-id="2d5ec-126">變更連接字串</span><span class="sxs-lookup"><span data-stu-id="2d5ec-126">Change the connection string</span></span>

<span data-ttu-id="2d5ec-127">在*appsettings.json*檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2 或您並未使用您所使用的電腦上的其他部份名稱。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-127">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[Main](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="2d5ec-128">這項變更設定，讓第一次移轉將會建立新的資料庫專案。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-128">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="2d5ec-129">這不是為了開始使用移轉，不過您看到更新版本的原因是個不錯的主意。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-129">This isn't required for getting started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="2d5ec-130">變更資料庫名稱的替代方案，您可以刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-130">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="2d5ec-131">使用**SQL Server 物件總管**(SSOX) 或`database drop`CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="2d5ec-131">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="2d5ec-132">下節說明如何執行 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-132">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="2d5ec-133">建立初始的移轉</span><span class="sxs-lookup"><span data-stu-id="2d5ec-133">Create an initial migration</span></span>

<span data-ttu-id="2d5ec-134">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-134">Save your changes and build the project.</span></span> <span data-ttu-id="2d5ec-135">開啟命令視窗，然後瀏覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-135">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="2d5ec-136">以下是快速的方法：</span><span class="sxs-lookup"><span data-stu-id="2d5ec-136">Here's a quick way to do that:</span></span>

* <span data-ttu-id="2d5ec-137">在**方案總管] 中**，以滑鼠右鍵按一下專案，然後選擇 [**在檔案總管中開啟**從內容功能表。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-137">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![在 [檔案總管] 功能表項目中開啟](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="2d5ec-139">在網址列中輸入"cmd 」，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-139">Enter "cmd" in the address bar and press Enter.</span></span>

  ![開啟命令視窗](migrations/_static/open-command-window.png)

<span data-ttu-id="2d5ec-141">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="2d5ec-141">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="2d5ec-142">您會看到類似 [命令] 視窗中的下列輸出：</span><span class="sxs-lookup"><span data-stu-id="2d5ec-142">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="2d5ec-143">如果您看到錯誤訊息*任何可執行檔中找到相符命令"dotnet-ef"*，請參閱[此部落格文章](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)如需疑難排解的協助。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-143">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="2d5ec-144">如果您看到錯誤訊息 「*無法存取檔案...ContosoUniversity.dll 因為另一個處理序正在使用它。*」，在 Windows 系統匣中，尋找 [IIS Express] 圖示，然後按一下滑鼠右鍵，然後按一下**ContosoUniversity > 停止站台**。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-144">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="2d5ec-145">檢查向上和向下方法</span><span class="sxs-lookup"><span data-stu-id="2d5ec-145">Examine the Up and Down methods</span></span>

<span data-ttu-id="2d5ec-146">當您執行`migrations add`命令，EF 產生的程式碼，將會從頭開始建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-146">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="2d5ec-147">此程式碼位於*移轉*資料夾中名為的檔案*\<時間戳記 > _InitialCreate.cs*。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-147">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="2d5ec-148">`Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表和`Down`方法會刪除它們，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-148">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="2d5ec-149">移轉呼叫`Up`方法，以實作資料模型變更，以進行移轉。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-149">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="2d5ec-150">當您輸入命令，以回復更新、 移轉呼叫`Down`方法。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-150">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="2d5ec-151">此程式碼是您輸入時所建立的初始移轉`migrations add InitialCreate`命令。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-151">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="2d5ec-152">移轉 name 參數 (在範例中為"InitialCreate 」) 使用的檔案名稱，而且可以是您所要的任何。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-152">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="2d5ec-153">您最好選擇的單字或片語來摘要列出所要完成移轉中的作業。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-153">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="2d5ec-154">例如，您可能會命名稍後移轉"AddDepartmentTable"。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-154">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="2d5ec-155">如果資料庫已經存在時，您會建立初始的移轉，資料庫建立程式碼會產生，但它不一定要執行，因為資料庫已經符合資料模型。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-155">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="2d5ec-156">當您部署應用程式到另一個環境，其中資料庫不存在，但這段程式碼會執行以建立您的資料庫時，因此，建議您先測試它。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-156">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="2d5ec-157">這就是為什麼您使移轉可以建立一個新從頭變更先前-在連接字串中的資料庫名稱。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-157">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="examine-the-data-model-snapshot"></a><span data-ttu-id="2d5ec-158">檢查資料模型快照集</span><span class="sxs-lookup"><span data-stu-id="2d5ec-158">Examine the data model snapshot</span></span>

<span data-ttu-id="2d5ec-159">移轉也會建立*快照*中目前的資料庫結構描述*Migrations/SchoolContextModelSnapshot.cs*。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-159">Migrations also creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="2d5ec-160">以下是該程式碼看起來像：</span><span class="sxs-lookup"><span data-stu-id="2d5ec-160">Here's what that code looks like:</span></span>

[!code-csharp[Main](intro/samples/cu/Migrations/SchoolContextModelSnapshot1.cs?name=snippet_Truncate)]

<span data-ttu-id="2d5ec-161">表示目前的資料庫結構描述時，程式碼中，因為 EF 核心沒有與要建立移轉作業的資料庫互動。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-161">Because the current database schema is represented in code, EF Core doesn't have to interact with the database to create migrations.</span></span> <span data-ttu-id="2d5ec-162">當您將在移轉時，EF 決定變更藉由比較資料模型，以快照集檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-162">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span> <span data-ttu-id="2d5ec-163">與資料庫的 EF 互動，才必須更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-163">EF interacts with the database only when it has to update the database.</span></span> 

<span data-ttu-id="2d5ec-164">快照集檔案必須加以建立，所以您不只要刪除名為的檔案移除移轉的移轉作業同步*\<時間戳記 > _\<migrationname >.cs*。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-164">The snapshot file has to be kept in sync with the migrations that create it, so you can't remove a migration just by deleting the file named  *\<timestamp>_\<migrationname>.cs*.</span></span> <span data-ttu-id="2d5ec-165">如果您刪除該檔案，將剩餘的移轉將會與資料庫快照集檔案同步處理。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-165">If you delete that file, the remaining migrations will be out of sync with the database snapshot file.</span></span> <span data-ttu-id="2d5ec-166">若要刪除最後一個您所加入的移轉，請使用[dotnet ef 移轉移除](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove)命令。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-166">To delete the last migration that you added, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="2d5ec-167">套用至資料庫的移轉</span><span class="sxs-lookup"><span data-stu-id="2d5ec-167">Apply the migration to the database</span></span>

<span data-ttu-id="2d5ec-168">在 [命令] 視窗中，輸入下列命令以在其中建立的資料庫和資料表。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-168">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="2d5ec-169">此命令的輸出是類似於`migrations add`命令時，不同之處在於如的 SQL 命令，將資料庫設定，請參閱記錄檔。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-169">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="2d5ec-170">下列範例輸出中將略過大部分的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-170">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="2d5ec-171">如果您不想看到此層級記錄檔訊息詳細資料，您可以變更記錄層級中的*appsettings。Development.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-171">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="2d5ec-172">如需詳細資訊，請參閱[簡介記錄](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-172">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

```text
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (467ms) [Parameters=[], CommandType='Text', CommandTimeout='60']
      CREATE DATABASE [ContosoUniversity2];
info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (20ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      CREATE TABLE [__EFMigrationsHistory] (
          [MigrationId] nvarchar(150) NOT NULL,
          [ProductVersion] nvarchar(32) NOT NULL,
          CONSTRAINT [PK___EFMigrationsHistory] PRIMARY KEY ([MigrationId])
      );

<logs omitted for brevity>

info: Microsoft.EntityFrameworkCore.Database.Command[200101]
      Executed DbCommand (3ms) [Parameters=[], CommandType='Text', CommandTimeout='30']
      INSERT INTO [__EFMigrationsHistory] ([MigrationId], [ProductVersion])
      VALUES (N'20170816151242_InitialCreate', N'2.0.0-rtm-26452');
Done.
```

<span data-ttu-id="2d5ec-173">使用**SQL Server 物件總管**檢查資料庫，您可以如同在第一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-173">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="2d5ec-174">您會發現 __EFMigrationsHistory 資料表，可追蹤的哪些移轉已經套用至資料庫的加法。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-174">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="2d5ec-175">檢視資料，該資料表中，您會看到第一次移轉一個資料列。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-175">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="2d5ec-176">（最後一個記錄檔，在上述的 CLI 輸出範例顯示建立此資料列 INSERT 陳述式）。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-176">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="2d5ec-177">執行應用程式驗證運作一切仍然正常與之前相同。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-177">Run the application to verify that everything still works the same as before.</span></span>

![學生索引頁](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="2d5ec-179">命令列介面 (CLI) 與Package Manager Console (PMC)</span><span class="sxs-lookup"><span data-stu-id="2d5ec-179">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="2d5ec-180">EF，工具管理移轉為.NET Core CLI 命令或從 Visual Studio 中的 PowerShell 指令程式使用**Package Manager Console** (PMC) 視窗。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-180">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="2d5ec-181">本教學課程示範如何使用 CLI，但是如果您想要的話，您可以使用 pmc 依存。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-181">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="2d5ec-182">PMC 命令的 EF 命令[Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools)封裝。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-182">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="2d5ec-183">此套件已隨附於[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，所以您不需要安裝它。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-183">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="2d5ec-184">**重要事項：**這不是相同的封裝，做為您藉由編輯安裝 CLI *.csproj*檔案。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-184">**Important:** This is not the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="2d5ec-185">這個名稱結尾`Tools`，不同於 CLI 封裝名稱中結尾`Tools.DotNet`。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-185">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="2d5ec-186">如需 CLI 命令的詳細資訊，請參閱[.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-186">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span> 

<span data-ttu-id="2d5ec-187">如需 PMC 命令的詳細資訊，請參閱[Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-187">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="2d5ec-188">總結</span><span class="sxs-lookup"><span data-stu-id="2d5ec-188">Summary</span></span>

<span data-ttu-id="2d5ec-189">在本教學課程中，您已經看到如何建立和套用您第一次移轉。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-189">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="2d5ec-190">在下一個教學課程中，開始，您將查看更進階的主題，方法是展開的資料模型。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-190">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="2d5ec-191">在過程中，您會建立並套用其他移轉。</span><span class="sxs-lookup"><span data-stu-id="2d5ec-191">Along the way you'll create and apply additional migrations.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2d5ec-192">[上一頁](sort-filter-page.md)
[下一頁](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="2d5ec-192">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>  
