---
title: ASP.NET Core MVC 與 EF Core - 移轉 - 4/10
author: rick-anderson
description: 在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。
ms.author: tdykstra
ms.date: 03/15/2018
uid: data/ef-mvc/migrations
ms.openlocfilehash: f710b33ac1a6017b0e3d7e8c3e528675a41424bb
ms.sourcegitcommit: c6ed2f00c7a08223d79090396b85793718b0dd69
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/29/2018
ms.locfileid: "37092938"
---
# <a name="aspnet-core-mvc-with-ef-core---migrations---4-of-10"></a><span data-ttu-id="47d47-103">ASP.NET Core MVC 與 EF Core - 移轉 - 4/10</span><span class="sxs-lookup"><span data-stu-id="47d47-103">ASP.NET Core MVC with EF Core - Migrations - 4 of 10</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc-21.md)]

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="47d47-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47d47-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="47d47-105">Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="47d47-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="47d47-106">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="47d47-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="47d47-107">在本教學課程中，您會先使用 EF Core 移轉功能來管理資料模型變更。</span><span class="sxs-lookup"><span data-stu-id="47d47-107">In this tutorial, you start using the EF Core migrations feature for managing data model changes.</span></span> <span data-ttu-id="47d47-108">在稍後的教學課程中，您將在變更資料模型時新增更多移轉作業。</span><span class="sxs-lookup"><span data-stu-id="47d47-108">In later tutorials, you'll add more migrations as you change the data model.</span></span>

## <a name="introduction-to-migrations"></a><span data-ttu-id="47d47-109">移轉簡介</span><span class="sxs-lookup"><span data-stu-id="47d47-109">Introduction to migrations</span></span>

<span data-ttu-id="47d47-110">在開發新的應用程式時，您的資料模型會頻繁變更，而每次模型變更時，模型會與資料庫不同步。</span><span class="sxs-lookup"><span data-stu-id="47d47-110">When you develop a new application, your data model changes frequently, and each time the model changes, it gets out of sync with the database.</span></span> <span data-ttu-id="47d47-111">首先，您會設定 Entity Framework 以建立資料庫 (如果尚未存在的話) 以進行本教學課程。</span><span class="sxs-lookup"><span data-stu-id="47d47-111">You started these tutorials by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="47d47-112">然後在每次變更資料模型 (新增、移除或變更實體類別或變更您的 DbContext 類別) 時，您可以刪除資料庫，EF 即會建立一個相符的新模型，並植入測試資料。</span><span class="sxs-lookup"><span data-stu-id="47d47-112">Then each time you change the data model -- add, remove, or change entity classes or change your DbContext class -- you can delete the database and EF creates a new one that matches the model, and seeds it with test data.</span></span>

<span data-ttu-id="47d47-113">在您將應用程式部署到生產環境之前，都可以使用上述方法讓資料庫與資料模型保持同步。</span><span class="sxs-lookup"><span data-stu-id="47d47-113">This method of keeping the database in sync with the data model works well until you deploy the application to production.</span></span> <span data-ttu-id="47d47-114">但當應用程式在生產環境中執行時，通常會儲存您想要保留的資料，而您也不想在每次資料變更 (例如新增資料行) 時遺失任何項目。</span><span class="sxs-lookup"><span data-stu-id="47d47-114">When the application is running in production it's usually storing data that you want to keep, and you don't want to lose everything each time you make a change such as adding a new column.</span></span> <span data-ttu-id="47d47-115">為了解決上述問題，EF Core 移轉功能可讓 EF 更新資料庫結構描述，而不是建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-115">The EF Core Migrations feature solves this problem by enabling EF to update the database schema instead of creating  a new database.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="47d47-116">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="47d47-116">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="47d47-117">您可以使用**套件管理員主控台** (PMC) 或命令列介面 (CLI)，來進行移轉作業。</span><span class="sxs-lookup"><span data-stu-id="47d47-117">To work with migrations, you can use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span>  <span data-ttu-id="47d47-118">這些教學課程會示範如何使用 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="47d47-118">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="47d47-119">PMC 的資訊位於[本教學課程結尾](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="47d47-119">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="47d47-120">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="47d47-120">The EF tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="47d47-121">若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合，如下所示。</span><span class="sxs-lookup"><span data-stu-id="47d47-121">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="47d47-122">**注意：** 您必須藉由編輯 *.csproj* 檔案來安裝這個套件；而不能使用 `install-package` 命令或套件管理員 GUI。</span><span class="sxs-lookup"><span data-stu-id="47d47-122">**Note:** You have to install this package by editing the *.csproj* file; you can't use the `install-package` command or the package manager GUI.</span></span> <span data-ttu-id="47d47-123">在方案總管中，以滑鼠右鍵按一下專案名稱，然後選取 [Edit ContosoUniversity.csproj] (編輯 ContosoUniversity.csproj) 以編輯 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="47d47-123">You can edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?range=12-15&highlight=2)]
  
<span data-ttu-id="47d47-124">(範例中的版本號碼為教學課程撰寫當下的最新版本。)</span><span class="sxs-lookup"><span data-stu-id="47d47-124">(The version numbers in this example were current when the tutorial was written.)</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="47d47-125">變更連接字串</span><span class="sxs-lookup"><span data-stu-id="47d47-125">Change the connection string</span></span>

<span data-ttu-id="47d47-126">在 *appsettings.json* 檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2 (或您所使用之電腦上未用過的其他名稱)。</span><span class="sxs-lookup"><span data-stu-id="47d47-126">In the *appsettings.json* file, change the name of the database in the connection string to ContosoUniversity2 or some other name that you haven't used on the computer you're using.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="47d47-127">這項變更會設定專案，以讓第一次移轉建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-127">This change sets up the project so that the first migration will create a new database.</span></span> <span data-ttu-id="47d47-128">即使不採取上述動作，仍可開始進行移轉作業，但稍後您會了解這麼做的好處何在。</span><span class="sxs-lookup"><span data-stu-id="47d47-128">This isn't required to get started with migrations, but you'll see later why it's a good idea.</span></span>

> [!NOTE]
> <span data-ttu-id="47d47-129">如果您不想變更資料庫名稱，替代方法是刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-129">As an alternative to changing the database name, you can delete the database.</span></span> <span data-ttu-id="47d47-130">使用 [SQL Server 物件總管] (SSOX) 或 `database drop` CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="47d47-130">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>
> ```console
> dotnet ef database drop
> ```
> <span data-ttu-id="47d47-131">下節說明如何執行 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="47d47-131">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="47d47-132">建立初始移轉</span><span class="sxs-lookup"><span data-stu-id="47d47-132">Create an initial migration</span></span>

<span data-ttu-id="47d47-133">儲存您的變更，並建置專案。</span><span class="sxs-lookup"><span data-stu-id="47d47-133">Save your changes and build the project.</span></span> <span data-ttu-id="47d47-134">接著，開啟命令視窗並巡覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="47d47-134">Then open a command window and navigate to the project folder.</span></span> <span data-ttu-id="47d47-135">以下是執行這個動作的快速方法：</span><span class="sxs-lookup"><span data-stu-id="47d47-135">Here's a quick way to do that:</span></span>

* <span data-ttu-id="47d47-136">在方案總管中，以滑鼠右鍵按一下專案，然後選擇操作功能表的 [在檔案總管中開啟]。</span><span class="sxs-lookup"><span data-stu-id="47d47-136">In **Solution Explorer**, right-click the project and choose **Open in File Explorer** from the context menu.</span></span>

  ![[在檔案總管中開啟] 功能表項目](migrations/_static/open-in-file-explorer.png)

* <span data-ttu-id="47d47-138">在位址列中輸入 "cmd"，然後按 Enter。</span><span class="sxs-lookup"><span data-stu-id="47d47-138">Enter "cmd" in the address bar and press Enter.</span></span>

  ![開啟命令視窗](migrations/_static/open-command-window.png)

<span data-ttu-id="47d47-140">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="47d47-140">Enter the following command in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="47d47-141">在命令視窗中，您會看到類似如下的輸出：</span><span class="sxs-lookup"><span data-stu-id="47d47-141">You see output like the following in the command window:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

> [!NOTE]
> <span data-ttu-id="47d47-142">如果您看到錯誤訊息：「找不到符合命令 "dotnet-ef" 的可執行檔」，請參閱[這篇部落格文章](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/)以取得疑難排解說明。</span><span class="sxs-lookup"><span data-stu-id="47d47-142">If you see an error message *No executable found matching command "dotnet-ef"*, see [this blog post](http://thedatafarm.com/data-access/no-executable-found-matching-command-dotnet-ef/) for help troubleshooting.</span></span>

<span data-ttu-id="47d47-143">如果您看到錯誤訊息：「無法存取檔案...ContosoUniversity.dll，因為其他處理序正在使用此檔案。」請在 Windows 系統匣中尋找 IIS Express 圖示，以滑鼠右鍵按一下它，然後按一下 [ContosoUniversity] > [停止網站]。</span><span class="sxs-lookup"><span data-stu-id="47d47-143">If you see an error message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*", find the IIS Express icon in the Windows System Tray, and right-click it, then click **ContosoUniversity > Stop Site**.</span></span>

## <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="47d47-144">檢查 Up 和 Down 方法</span><span class="sxs-lookup"><span data-stu-id="47d47-144">Examine the Up and Down methods</span></span>

<span data-ttu-id="47d47-145">當您執行 `migrations add` 命令時，EF 產生的程式碼會從頭開始建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-145">When you executed the `migrations add` command, EF generated the code that will create the database from scratch.</span></span> <span data-ttu-id="47d47-146">您可以在 *Migrations* 資料夾，名為 \<時間戳記>_InitialCreate.cs 的檔案中，找到這個程式碼。</span><span class="sxs-lookup"><span data-stu-id="47d47-146">This code is in the *Migrations* folder, in the file named *\<timestamp>_InitialCreate.cs*.</span></span> <span data-ttu-id="47d47-147">`InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表，而 `Down` 方法則會刪除它們，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="47d47-147">The `Up` method of the `InitialCreate` class creates the database tables that correspond to the data model entity sets, and the `Down` method deletes them, as shown in the following example.</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20170215220724_InitialCreate.cs?range=92-118)]

<span data-ttu-id="47d47-148">Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。</span><span class="sxs-lookup"><span data-stu-id="47d47-148">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="47d47-149">當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。</span><span class="sxs-lookup"><span data-stu-id="47d47-149">When you enter a command to roll back the update, Migrations calls the `Down` method.</span></span>

<span data-ttu-id="47d47-150">這個程式碼適用於您之前輸入 `migrations add InitialCreate` 命令時所建立的初始移轉。</span><span class="sxs-lookup"><span data-stu-id="47d47-150">This code is for the initial migration that was created when you entered the `migrations add InitialCreate` command.</span></span> <span data-ttu-id="47d47-151">移轉名稱參數 (在此範例中為 "InitialCreate") 可作為檔案名稱，您也可以任意命名。</span><span class="sxs-lookup"><span data-stu-id="47d47-151">The migration name parameter ("InitialCreate" in the example) is used for the file name and can be whatever you want.</span></span> <span data-ttu-id="47d47-152">建議您選擇某個單字或片語，以摘要說明移轉中所要完成的作業。</span><span class="sxs-lookup"><span data-stu-id="47d47-152">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="47d47-153">例如，您可以將稍後的移轉命名為 "AddDepartmentTable"。</span><span class="sxs-lookup"><span data-stu-id="47d47-153">For example, you might name a later migration "AddDepartmentTable".</span></span>

<span data-ttu-id="47d47-154">如果在您建立初始移轉時資料庫已經存在，系統會產生資料庫建立程式碼，但不需要執行，因為資料庫已經符合資料模型。</span><span class="sxs-lookup"><span data-stu-id="47d47-154">If you created the initial migration when the database already exists, the database creation code is generated but it doesn't have to run because the database already matches the data model.</span></span> <span data-ttu-id="47d47-155">當您將應用程式部署到資料庫尚未存在的其他環境中時，即會執行這個程式碼以建立您的資料庫；建議您先進行測試。</span><span class="sxs-lookup"><span data-stu-id="47d47-155">When you deploy the app to another environment where the database doesn't exist yet, this code will run to create your database, so it's a good idea to test it first.</span></span> <span data-ttu-id="47d47-156">這就是為什麼稍早要您變更連接字串中資料庫名稱的原因，這樣一來，移轉即可從頭建立一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-156">That's why you changed the name of the database in the connection string earlier -- so that migrations can create a new one from scratch.</span></span>

## <a name="the-data-model-snapshot"></a><span data-ttu-id="47d47-157">資料模型快照集</span><span class="sxs-lookup"><span data-stu-id="47d47-157">The data model snapshot</span></span>

<span data-ttu-id="47d47-158">移轉會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料庫結構描述的「快照集」。</span><span class="sxs-lookup"><span data-stu-id="47d47-158">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="47d47-159">當您新增移轉時，EF 會比較資料模型與快照集檔案，以判斷變更的內容。</span><span class="sxs-lookup"><span data-stu-id="47d47-159">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="47d47-160">刪除移轉時，請使用 [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。</span><span class="sxs-lookup"><span data-stu-id="47d47-160">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="47d47-161">`dotnet ef migrations remove` 會刪除移轉，並確保正確地重設快照集。</span><span class="sxs-lookup"><span data-stu-id="47d47-161">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="47d47-162">如需如何使用快照集檔案的詳細資訊，請參閱[小組環境中的 EF Core 移轉](/ef/core/managing-schemas/migrations/teams)。</span><span class="sxs-lookup"><span data-stu-id="47d47-162">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="apply-the-migration-to-the-database"></a><span data-ttu-id="47d47-163">將移轉套用至資料庫</span><span class="sxs-lookup"><span data-stu-id="47d47-163">Apply the migration to the database</span></span>

<span data-ttu-id="47d47-164">在命令視窗中輸入下列命令，以建立資料庫和其中的資料表。</span><span class="sxs-lookup"><span data-stu-id="47d47-164">In the command window, enter the following command to create the database and tables in it.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="47d47-165">此命令的輸出類似於 `migrations add` 命令，不同之處在於其會顯示設定資料庫之 SQL 命令的記錄。</span><span class="sxs-lookup"><span data-stu-id="47d47-165">The output from the command is similar to the `migrations add` command, except that you see logs for the SQL commands that set up the database.</span></span> <span data-ttu-id="47d47-166">下列範例輸出中省略了大部分的記錄。</span><span class="sxs-lookup"><span data-stu-id="47d47-166">Most of the logs are omitted in the following sample output.</span></span> <span data-ttu-id="47d47-167">如果您不想看到這麼詳細的記錄訊息，可以變更 *appsettings.Development.json* 檔案中的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="47d47-167">If you prefer not to see this level of detail in log messages, you can change the log level in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="47d47-168">如需詳細資訊，請參閱[記錄簡介](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="47d47-168">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

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

<span data-ttu-id="47d47-169">按照第一個教學課程的做法，使用 [SQL Server 物件總管] 檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-169">Use **SQL Server Object Explorer** to inspect the database as you did in the first tutorial.</span></span>  <span data-ttu-id="47d47-170">您會發現其中新增了 __EFMigrationsHistory 資料表，其會持續追蹤哪些移轉已套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="47d47-170">You'll notice the addition of an __EFMigrationsHistory table that keeps track of which migrations have been applied to the database.</span></span> <span data-ttu-id="47d47-171">檢視該資料表中的資料，您會看到其中一個資料列代表第一個移轉。</span><span class="sxs-lookup"><span data-stu-id="47d47-171">View the data in that table and you'll see one row for the first migration.</span></span> <span data-ttu-id="47d47-172">(上述 CLI 輸出範例中的最後一則記錄會顯示建立此資料列的 INSERT 陳述式。)</span><span class="sxs-lookup"><span data-stu-id="47d47-172">(The last log in the preceding CLI output example shows the INSERT statement that creates this row.)</span></span>

<span data-ttu-id="47d47-173">執行應用程式，以驗證一切如往常般運作。</span><span class="sxs-lookup"><span data-stu-id="47d47-173">Run the application to verify that everything still works the same as before.</span></span>

![Students [索引] 頁面](migrations/_static/students-index.png)

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="47d47-175">命令列介面 (CLI) 與套件管理員主控台 (PMC)</span><span class="sxs-lookup"><span data-stu-id="47d47-175">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="47d47-176">您可以透過 .NET Core CLI 命令或 Visual Studio [套件管理員主控台] (PMC) 視窗中的 PowerShell Cmdlet，取得適用於管理移轉的 EF 工具。</span><span class="sxs-lookup"><span data-stu-id="47d47-176">The EF tooling for managing migrations is available from .NET Core CLI commands or from PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span> <span data-ttu-id="47d47-177">本教學課程示範如何使用 CLI，但是您也可以視習慣使用 PMC。</span><span class="sxs-lookup"><span data-stu-id="47d47-177">This tutorial shows how to use the CLI, but you can use the PMC if you prefer.</span></span>

<span data-ttu-id="47d47-178">適用於 PMC 命令的 EF 命令位於 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 套件中。</span><span class="sxs-lookup"><span data-stu-id="47d47-178">The EF commands for the PMC commands are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="47d47-179">此套件已包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 中繼套件內，因此您不需要進行安裝。</span><span class="sxs-lookup"><span data-stu-id="47d47-179">This package is already included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="47d47-180">**重要事項：** 此套件與您為 CLI 安裝的套件不同 (透過編輯 *.csproj* 檔案來進行)。</span><span class="sxs-lookup"><span data-stu-id="47d47-180">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="47d47-181">這個套件的名稱以 `Tools` 結尾，不同於以 `Tools.DotNet` 結尾的 CLI 套件名稱。</span><span class="sxs-lookup"><span data-stu-id="47d47-181">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="47d47-182">如需 CLI 命令的詳細資訊，請參閱 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="47d47-182">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="47d47-183">如需 PMC 命令的詳細資訊，請參閱[套件管理員主控台 (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。</span><span class="sxs-lookup"><span data-stu-id="47d47-183">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="summary"></a><span data-ttu-id="47d47-184">總結</span><span class="sxs-lookup"><span data-stu-id="47d47-184">Summary</span></span>

<span data-ttu-id="47d47-185">在本教學課程中，您已了解如何建立並套用第一次移轉。</span><span class="sxs-lookup"><span data-stu-id="47d47-185">In this tutorial, you've seen how to create and apply your first migration.</span></span> <span data-ttu-id="47d47-186">在下一個教學課程中，您就可以展開資料模型以學習更進階的主題。</span><span class="sxs-lookup"><span data-stu-id="47d47-186">In the next tutorial, you'll begin looking at more advanced topics by expanding the data model.</span></span> <span data-ttu-id="47d47-187">在過程中，您會建立並套用其他移轉。</span><span class="sxs-lookup"><span data-stu-id="47d47-187">Along the way you'll create and apply additional migrations.</span></span>
::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="47d47-188">[上一頁](sort-filter-page.md)
> [下一頁](complex-data-model.md)</span><span class="sxs-lookup"><span data-stu-id="47d47-188">[Previous](sort-filter-page.md)
[Next](complex-data-model.md)</span></span>
