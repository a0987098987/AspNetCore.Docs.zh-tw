---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8
author: rick-anderson
description: 在本教學課程中，您將開始使用 EF Core 移轉功能來管理 ASP.NET Core MVC 應用程式中的資料模型變更。
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/migrations
ms.openlocfilehash: d39e1aa40ff97d5b335f2bde6170242e89f6189a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272344"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---migrations---4-of-8"></a><span data-ttu-id="7201c-103">ASP.NET Core 中的 Razor 頁面與 EF Core - 移轉 - 4/8</span><span class="sxs-lookup"><span data-stu-id="7201c-103">Razor Pages with EF Core in ASP.NET Core - Migrations - 4 of 8</span></span>

<span data-ttu-id="7201c-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7201c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="7201c-105">在本教學課程中，會使用 EF Core 移轉功能來管理資料模型變更。</span><span class="sxs-lookup"><span data-stu-id="7201c-105">In this tutorial, the EF Core migrations feature for managing data model changes is used.</span></span>

<span data-ttu-id="7201c-106">如果您遇到無法解決的問題，請下載[此階段已完成的應用程式](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。</span><span class="sxs-lookup"><span data-stu-id="7201c-106">If you run into problems you can't solve, download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="7201c-107">開發新的應用程式時，資料模型經常變更。</span><span class="sxs-lookup"><span data-stu-id="7201c-107">When a new app is developed, the data model changes frequently.</span></span> <span data-ttu-id="7201c-108">每次模型變更時，模型就與資料庫不同步。</span><span class="sxs-lookup"><span data-stu-id="7201c-108">Each time the model changes, the model gets out of sync with the database.</span></span> <span data-ttu-id="7201c-109">本教學課程從設定 Entity Framework 來建立不存在的資料庫開始。</span><span class="sxs-lookup"><span data-stu-id="7201c-109">This tutorial started by configuring the Entity Framework to create the database if it doesn't exist.</span></span> <span data-ttu-id="7201c-110">每次資料模型變更時：</span><span class="sxs-lookup"><span data-stu-id="7201c-110">Each time the data model changes:</span></span>

* <span data-ttu-id="7201c-111">會卸除資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-111">The DB is dropped.</span></span>
* <span data-ttu-id="7201c-112">EF 會建立一個符合模型的新資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-112">EF creates a new one that matches the model.</span></span>
* <span data-ttu-id="7201c-113">應用程式會將測試資料植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-113">The app seeds the DB with test data.</span></span>

<span data-ttu-id="7201c-114">在您將應用程式部署到生產環境之前，這種讓資料庫與資料模型保持同步的方法效果不錯。</span><span class="sxs-lookup"><span data-stu-id="7201c-114">This approach to keeping the DB in sync with the data model works well until you deploy the app to production.</span></span> <span data-ttu-id="7201c-115">當應用程式在生產環境中執行時，它通常會儲存需要維護的資料。</span><span class="sxs-lookup"><span data-stu-id="7201c-115">When the app is running in production, it's usually storing data that needs to be maintained.</span></span> <span data-ttu-id="7201c-116">應用程式不能每次進行變更 (例如新增資料行) 時，都從測試資料庫開始。</span><span class="sxs-lookup"><span data-stu-id="7201c-116">The app can't start with a test DB each time a change is made (such as adding a new column).</span></span> <span data-ttu-id="7201c-117">EF Core 移轉功能可解決這個問題，方法是啟用 EF Core 以更新資料庫結構描述，來代替建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-117">The EF Core Migrations feature solves this problem by enabling EF Core to update the DB schema instead of creating a new DB.</span></span>

<span data-ttu-id="7201c-118">移轉會更新結構描述，並保留現有的資料，而不是在資料模型變更時，卸除並重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-118">Rather than dropping and recreating the DB when the data model changes, migrations updates the schema and retains existing data.</span></span>

## <a name="entity-framework-core-nuget-packages-for-migrations"></a><span data-ttu-id="7201c-119">用於移轉的 Entity Framework Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="7201c-119">Entity Framework Core NuGet packages for migrations</span></span>

<span data-ttu-id="7201c-120">若要使用移轉，請使用 [套件管理員主控台] (PMC) 或命令列介面 (CLI)。</span><span class="sxs-lookup"><span data-stu-id="7201c-120">To work with migrations, use the **Package Manager Console** (PMC) or the command-line interface (CLI).</span></span> <span data-ttu-id="7201c-121">這些教學課程會示範如何使用 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-121">These tutorials show how to use CLI commands.</span></span> <span data-ttu-id="7201c-122">PMC 的資訊位於[本教學課程結尾](#pmc)。</span><span class="sxs-lookup"><span data-stu-id="7201c-122">Information about the PMC is at [the end of this tutorial](#pmc).</span></span>

<span data-ttu-id="7201c-123">[Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet) 中提供了命令列介面 (CLI) 的 EF Core 工具。</span><span class="sxs-lookup"><span data-stu-id="7201c-123">The EF Core tools for the command-line interface (CLI) are provided in [Microsoft.EntityFrameworkCore.Tools.DotNet](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools.DotNet).</span></span> <span data-ttu-id="7201c-124">若要安裝這個套件，請將它新增至 *.csproj* 檔案中的 `DotNetCliToolReference` 集合，如下所示。</span><span class="sxs-lookup"><span data-stu-id="7201c-124">To install this package, add it to the `DotNetCliToolReference` collection in the *.csproj* file, as shown.</span></span> <span data-ttu-id="7201c-125">**注意：** 此套件必須透過編輯 *.csproj* 檔案進行安裝。</span><span class="sxs-lookup"><span data-stu-id="7201c-125">**Note:** This package must be installed by editing the *.csproj* file.</span></span> <span data-ttu-id="7201c-126">`install-package` 命令或套件管理員 GUI 無法用來安裝此套件。</span><span class="sxs-lookup"><span data-stu-id="7201c-126">The`install-package` command or the package manager GUI cannot be used to install this package.</span></span> <span data-ttu-id="7201c-127">以滑鼠右鍵按一下方案總管 中的專案名稱，然後選取 [Edit ContosoUniversity.csproj] (編輯 ContosoUniversity.csproj)，以編輯 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7201c-127">Edit the *.csproj* file by right-clicking the project name in **Solution Explorer** and selecting **Edit ContosoUniversity.csproj**.</span></span>

<span data-ttu-id="7201c-128">下列標記會顯示更新的 *.csproj* 檔案，並醒目提示 EF Core CLI 工具：</span><span class="sxs-lookup"><span data-stu-id="7201c-128">The following markup shows the updated *.csproj* file with the EF Core CLI tools highlighted:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity.csproj?highlight=12)]
  
<span data-ttu-id="7201c-129">上述範例中的版本號碼在撰寫教學課程時是最新的。</span><span class="sxs-lookup"><span data-stu-id="7201c-129">The version numbers in the preceding example were current when the tutorial was written.</span></span> <span data-ttu-id="7201c-130">請使用其他套件中相同版本的 EF Core CLI 工具。</span><span class="sxs-lookup"><span data-stu-id="7201c-130">Use the same version for the EF Core CLI tools found in the other packages.</span></span>

## <a name="change-the-connection-string"></a><span data-ttu-id="7201c-131">變更連接字串</span><span class="sxs-lookup"><span data-stu-id="7201c-131">Change the connection string</span></span>

<span data-ttu-id="7201c-132">在 *appsettings.json* 檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2。</span><span class="sxs-lookup"><span data-stu-id="7201c-132">In the *appsettings.json* file, change the name of the DB in the connection string to ContosoUniversity2.</span></span>

[!code-json[](intro/samples/cu/appsettings2.json?range=1-4)]

<span data-ttu-id="7201c-133">變更連接字串中的資料庫名稱，會導致第一個移轉建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-133">Changing the DB name in the connection string causes the first migration to create a new DB.</span></span> <span data-ttu-id="7201c-134">因為不存在具有該名稱的資料庫，所以會建立新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-134">A new DB is created because one with that name doesn't exist.</span></span> <span data-ttu-id="7201c-135">開始使用移轉並不需要變更連接字串。</span><span class="sxs-lookup"><span data-stu-id="7201c-135">Changing the connection string isn't required for getting started with migrations.</span></span>

<span data-ttu-id="7201c-136">變更資料庫名稱的替代方式是刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-136">An alternative to changing the DB name is deleting the DB.</span></span> <span data-ttu-id="7201c-137">使用 [SQL Server 物件總管] (SSOX) 或 `database drop` CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="7201c-137">Use **SQL Server Object Explorer** (SSOX) or the `database drop` CLI command:</span></span>

 ```console
 dotnet ef database drop
 ```

<span data-ttu-id="7201c-138">下節說明如何執行 CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-138">The following section explains how to run CLI commands.</span></span>

## <a name="create-an-initial-migration"></a><span data-ttu-id="7201c-139">建立初始移轉</span><span class="sxs-lookup"><span data-stu-id="7201c-139">Create an initial migration</span></span>

<span data-ttu-id="7201c-140">建置專案。</span><span class="sxs-lookup"><span data-stu-id="7201c-140">Build the project.</span></span>

<span data-ttu-id="7201c-141">開啟命令視窗並巡覽至專案資料夾。</span><span class="sxs-lookup"><span data-stu-id="7201c-141">Open a command window and navigate to the project folder.</span></span> <span data-ttu-id="7201c-142">專案資料夾中包含 *Startup.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="7201c-142">The project folder contains the *Startup.cs* file.</span></span>

<span data-ttu-id="7201c-143">在命令視窗中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="7201c-143">Enter the following in the command window:</span></span>

```console
dotnet ef migrations add InitialCreate
```

<span data-ttu-id="7201c-144">命令視窗會顯示與下列類似的資訊：</span><span class="sxs-lookup"><span data-stu-id="7201c-144">The command window displays information similar to the following:</span></span>

```console
info: Microsoft.AspNetCore.DataProtection.KeyManagement.XmlKeyManager[0]
      User profile is available. Using 'C:\Users\username\AppData\Local\ASP.NET\DataProtection-Keys' as key repository and Windows DPAPI to encrypt keys at rest.
info: Microsoft.EntityFrameworkCore.Infrastructure[100403]
      Entity Framework Core 2.0.0-rtm-26452 initialized 'SchoolContext' using provider 'Microsoft.EntityFrameworkCore.SqlServer' with options: None
Done. To undo this action, use 'ef migrations remove'
```

<span data-ttu-id="7201c-145">如果移轉失敗，訊息「無法存取檔案...ContosoUniversity.dll，因為其他處理序正在使用此檔。」</span><span class="sxs-lookup"><span data-stu-id="7201c-145">If the migration fails with the message "*cannot access the file ... ContosoUniversity.dll because it is being used by another process.*"</span></span> <span data-ttu-id="7201c-146">隨即顯示：</span><span class="sxs-lookup"><span data-stu-id="7201c-146">is displayed:</span></span>

* <span data-ttu-id="7201c-147">停止 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="7201c-147">Stop IIS Express.</span></span>

   * <span data-ttu-id="7201c-148">結束並重新啟動 Visual Studio，或</span><span class="sxs-lookup"><span data-stu-id="7201c-148">Exit and restart Visual Studio, or</span></span>
   * <span data-ttu-id="7201c-149">在 Windows 系統匣中尋找 IIS Express 圖示。</span><span class="sxs-lookup"><span data-stu-id="7201c-149">Find the IIS Express icon in the Windows System Tray.</span></span>
   * <span data-ttu-id="7201c-150">以滑鼠右鍵按一下 IIS Express 圖示，然後按一下[ContosoUniversity] > [停止網站]</span><span class="sxs-lookup"><span data-stu-id="7201c-150">Right-click the IIS Express icon, and then click **ContosoUniversity > Stop Site**.</span></span>

<span data-ttu-id="7201c-151">如果錯誤訊息「建置失敗。」</span><span class="sxs-lookup"><span data-stu-id="7201c-151">If the error message "Build failed."</span></span> <span data-ttu-id="7201c-152">隨即顯示，請再次執行命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-152">is displayed, run the command again.</span></span> <span data-ttu-id="7201c-153">如果您收到這個錯誤，請在本教學課程的底部留下附註。</span><span class="sxs-lookup"><span data-stu-id="7201c-153">If you get this error, leave a note at the bottom of this tutorial.</span></span>

### <a name="examine-the-up-and-down-methods"></a><span data-ttu-id="7201c-154">檢查 Up 和 Down 方法</span><span class="sxs-lookup"><span data-stu-id="7201c-154">Examine the Up and Down methods</span></span>

<span data-ttu-id="7201c-155">EF Core 命令 `migrations add` 已產生用來建立資料庫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="7201c-155">The EF Core command `migrations add` generated code to create the DB from.</span></span> <span data-ttu-id="7201c-156">此移轉程式碼位於 Migrations\<時間戳記>_InitialCreate.cs 檔案中。</span><span class="sxs-lookup"><span data-stu-id="7201c-156">This migrations code is in the *Migrations\<timestamp>_InitialCreate.cs* file.</span></span> <span data-ttu-id="7201c-157">`InitialCreate` 類別的 `Up` 方法會建立對應至資料模型實體集的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="7201c-157">The `Up` method of the `InitialCreate` class creates the DB tables that correspond to the data model entity sets.</span></span> <span data-ttu-id="7201c-158">`Down` 方法則會刪除它們，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="7201c-158">The `Down` method deletes them, as shown in the following example:</span></span>

[!code-csharp[](intro/samples/cu/Migrations/20171026010210_InitialCreate.cs?range=8-24,77-)]

<span data-ttu-id="7201c-159">移轉會呼叫 `Up` 方法，以實作資料模型變更來進行移轉。</span><span class="sxs-lookup"><span data-stu-id="7201c-159">Migrations calls the `Up` method to implement the data model changes for a migration.</span></span> <span data-ttu-id="7201c-160">當您輸入命令以復原更新時，移轉會呼叫 `Down` 方法。</span><span class="sxs-lookup"><span data-stu-id="7201c-160">When you enter a command to roll back the update, migrations calls the `Down` method.</span></span>

<span data-ttu-id="7201c-161">上述程式碼適用於初始移轉。</span><span class="sxs-lookup"><span data-stu-id="7201c-161">The preceding code is for the initial migration.</span></span> <span data-ttu-id="7201c-162">該程式碼是在執行 `migrations add InitialCreate` 命令時建立。</span><span class="sxs-lookup"><span data-stu-id="7201c-162">That code was created when the `migrations add InitialCreate` command was run.</span></span> <span data-ttu-id="7201c-163">移轉名稱參數 (在範例中為 "InitialCreate") 用於檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="7201c-163">The migration name parameter ("InitialCreate" in the example) is used for the file name.</span></span> <span data-ttu-id="7201c-164">移轉名稱可以是任何有效的檔案名稱。</span><span class="sxs-lookup"><span data-stu-id="7201c-164">The migration name can be any valid file name.</span></span> <span data-ttu-id="7201c-165">您最好選擇單字或片語來摘要列出移轉中所要完成的作業。</span><span class="sxs-lookup"><span data-stu-id="7201c-165">It's best to choose a word or phrase that summarizes what is being done in the migration.</span></span> <span data-ttu-id="7201c-166">例如，新增了部門資料表的移轉可能稱為 "AddDepartmentTable"。</span><span class="sxs-lookup"><span data-stu-id="7201c-166">For example, a migration that added a department table might be called "AddDepartmentTable."</span></span>

<span data-ttu-id="7201c-167">如果已建立初始移轉並結束資料庫，則：</span><span class="sxs-lookup"><span data-stu-id="7201c-167">If the initial migration is created and the DB exists:</span></span>

* <span data-ttu-id="7201c-168">會產生資料庫建立程式碼。</span><span class="sxs-lookup"><span data-stu-id="7201c-168">The DB creation code is generated.</span></span>
* <span data-ttu-id="7201c-169">不需要執行資料庫建立程式碼，因為資料庫已符合資料模型。</span><span class="sxs-lookup"><span data-stu-id="7201c-169">The DB creation code doesn't need to run because the DB already matches the data model.</span></span> <span data-ttu-id="7201c-170">如果執行了資料庫建立程式碼，它不會進行任何變更，因為資料庫已符合資料模型。</span><span class="sxs-lookup"><span data-stu-id="7201c-170">If the DB creation code is run, it doesn't make any changes because the DB already matches the data model.</span></span>

<span data-ttu-id="7201c-171">當應用程式部署到新的環境時，必須執行資料庫建立程式碼來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-171">When the app is deployed to a new environment, the DB creation code must be run to create the DB.</span></span>

<span data-ttu-id="7201c-172">之前，連接字串已變更為使用資料庫的新名稱。</span><span class="sxs-lookup"><span data-stu-id="7201c-172">Previously the connection string was changed to use a new name for the DB.</span></span> <span data-ttu-id="7201c-173">指定的資料庫不存在；因此，移轉會建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-173">The specified DB doesn't exist, so migrations creates the DB.</span></span>

### <a name="the-data-model-snapshot"></a><span data-ttu-id="7201c-174">資料模型快照集</span><span class="sxs-lookup"><span data-stu-id="7201c-174">The data model snapshot</span></span>

<span data-ttu-id="7201c-175">移轉會在 *Migrations/SchoolContextModelSnapshot.cs* 中建立目前資料庫結構描述的「快照集」。</span><span class="sxs-lookup"><span data-stu-id="7201c-175">Migrations creates a *snapshot* of the current database schema in *Migrations/SchoolContextModelSnapshot.cs*.</span></span> <span data-ttu-id="7201c-176">當您新增移轉時，EF 會比較資料模型與快照集檔案，以判斷變更的內容。</span><span class="sxs-lookup"><span data-stu-id="7201c-176">When you add a migration, EF determines what changed by comparing the data model to the snapshot file.</span></span>

<span data-ttu-id="7201c-177">刪除移轉時，請使用 [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) 命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-177">When deleting a migration, use the [dotnet ef migrations remove](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet#dotnet-ef-migrations-remove) command.</span></span> <span data-ttu-id="7201c-178">`dotnet ef migrations remove` 會刪除移轉，並確保正確地重設快照集。</span><span class="sxs-lookup"><span data-stu-id="7201c-178">`dotnet ef migrations remove` deletes the migration and ensures the snapshot is correctly reset.</span></span>

<span data-ttu-id="7201c-179">如需如何使用快照集檔案的詳細資訊，請參閱[小組環境中的 EF Core 移轉](/ef/core/managing-schemas/migrations/teams)。</span><span class="sxs-lookup"><span data-stu-id="7201c-179">See [EF Core Migrations in Team Environments](/ef/core/managing-schemas/migrations/teams) for more information about how the snapshot file is used.</span></span>

## <a name="remove-ensurecreated"></a><span data-ttu-id="7201c-180">移除 EnsureCreated</span><span class="sxs-lookup"><span data-stu-id="7201c-180">Remove EnsureCreated</span></span>

<span data-ttu-id="7201c-181">早期開發會使用 `EnsureCreated` 命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-181">For early development, the `EnsureCreated` command was used.</span></span> <span data-ttu-id="7201c-182">在本教學課程中，則使用移轉。</span><span class="sxs-lookup"><span data-stu-id="7201c-182">In this tutorial, migrations is used.</span></span> <span data-ttu-id="7201c-183">`EnsureCreated` 具有下列限制：</span><span class="sxs-lookup"><span data-stu-id="7201c-183">`EnsureCreated` has the following limitations:</span></span>

* <span data-ttu-id="7201c-184">略過移轉，並建立資料庫和結構描述。</span><span class="sxs-lookup"><span data-stu-id="7201c-184">Bypasses migrations and creates the DB and schema.</span></span>
* <span data-ttu-id="7201c-185">不會建立移轉資料表。</span><span class="sxs-lookup"><span data-stu-id="7201c-185">Doesn't create a migrations table.</span></span>
* <span data-ttu-id="7201c-186">「無法」與移轉搭配使用。</span><span class="sxs-lookup"><span data-stu-id="7201c-186">Can *not* be used with migrations.</span></span>
* <span data-ttu-id="7201c-187">設計用來測試或快速原型化經常卸除並重新建立資料庫的位置。</span><span class="sxs-lookup"><span data-stu-id="7201c-187">Is designed for testing or rapid prototyping where the DB is dropped and re-created frequently.</span></span>

<span data-ttu-id="7201c-188">從 `DbInitializer` 移除下列各行：</span><span class="sxs-lookup"><span data-stu-id="7201c-188">Remove the following line from `DbInitializer`:</span></span>

```csharp
context.Database.EnsureCreated();
```

## <a name="apply-the-migration-to-the-db-in-development"></a><span data-ttu-id="7201c-189">在開發環境中將移轉套用至資料庫</span><span class="sxs-lookup"><span data-stu-id="7201c-189">Apply the migration to the DB in development</span></span>

<span data-ttu-id="7201c-190">在命令視窗中輸入下列命令，以建立資料庫和資料表。</span><span class="sxs-lookup"><span data-stu-id="7201c-190">In the command window, enter the following to create the DB and tables.</span></span>

```console
dotnet ef database update
```

<span data-ttu-id="7201c-191">注意：如果 `update` 命令傳回「建置失敗。」錯誤：</span><span class="sxs-lookup"><span data-stu-id="7201c-191">Note: If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="7201c-192">再次執行命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-192">Run the command again.</span></span>
* <span data-ttu-id="7201c-193">如果再次失敗，請結束 Visual Studio，然後執行 `update` 命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-193">If it fails again, exit Visual Studio and then run the `update` command.</span></span>
* <span data-ttu-id="7201c-194">在頁面底部留下訊息。</span><span class="sxs-lookup"><span data-stu-id="7201c-194">Leave a message at the bottom of the page.</span></span>

<span data-ttu-id="7201c-195">此命令的輸出類似於 `migrations add` 命令輸出。</span><span class="sxs-lookup"><span data-stu-id="7201c-195">The output from the command is similar to the `migrations add` command output.</span></span> <span data-ttu-id="7201c-196">在上述命令中，會顯示設定資料庫之 SQL 命令的記錄。</span><span class="sxs-lookup"><span data-stu-id="7201c-196">In the preceding command, logs for the SQL commands that set up the DB are displayed.</span></span> <span data-ttu-id="7201c-197">下列範例輸出中省略了大部分的記錄：</span><span class="sxs-lookup"><span data-stu-id="7201c-197">Most of the logs are omitted in the following sample output:</span></span>

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

<span data-ttu-id="7201c-198">若要降低記錄訊息的詳細資料層級，請變更 *appsettings.Development.json* 檔案中的記錄層級。</span><span class="sxs-lookup"><span data-stu-id="7201c-198">To reduce the level of detail in log messages, change the log levels in the *appsettings.Development.json* file.</span></span> <span data-ttu-id="7201c-199">如需詳細資訊，請參閱[記錄簡介](xref:fundamentals/logging/index)。</span><span class="sxs-lookup"><span data-stu-id="7201c-199">For more information, see [Introduction to logging](xref:fundamentals/logging/index).</span></span>

<span data-ttu-id="7201c-200">使用 **SQL Server 物件總管**來檢查資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-200">Use **SQL Server Object Explorer** to inspect the DB.</span></span> <span data-ttu-id="7201c-201">請注意已新增 `__EFMigrationsHistory` 資料表。</span><span class="sxs-lookup"><span data-stu-id="7201c-201">Notice the addition of an `__EFMigrationsHistory` table.</span></span> <span data-ttu-id="7201c-202">`__EFMigrationsHistory` 資料表會追蹤哪些移轉經套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="7201c-202">The `__EFMigrationsHistory` table keeps track of which migrations have been applied to the DB.</span></span> <span data-ttu-id="7201c-203">檢視 `__EFMigrationsHistory` 資料表中的資料，它會顯示第一個移轉的某個資料列。</span><span class="sxs-lookup"><span data-stu-id="7201c-203">View the data in the `__EFMigrationsHistory` table, it shows one row for the first migration.</span></span> <span data-ttu-id="7201c-204">上述 CLI 輸出範例中的最後一則記錄會顯示建立此資料列的 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="7201c-204">The last log in the preceding CLI output example shows the INSERT statement that creates this row.</span></span>

<span data-ttu-id="7201c-205">執行應用程式，並確認一切運作正常。</span><span class="sxs-lookup"><span data-stu-id="7201c-205">Run the app and verify that everything works.</span></span>

## <a name="applying-migrations-in-production"></a><span data-ttu-id="7201c-206">在生產環境中套用移轉</span><span class="sxs-lookup"><span data-stu-id="7201c-206">Applying migrations in production</span></span>

<span data-ttu-id="7201c-207">建議在應用程式啟動時，生產環境應用程式**不**應該呼叫 [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_)。</span><span class="sxs-lookup"><span data-stu-id="7201c-207">We recommend production apps should **not** call [Database.Migrate](/dotnet/api/microsoft.entityframeworkcore.relationaldatabasefacadeextensions.migrate?view=efcore-2.0#Microsoft_EntityFrameworkCore_RelationalDatabaseFacadeExtensions_Migrate_Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_) at application startup.</span></span> <span data-ttu-id="7201c-208">`Migrate` 不應該從伺服器陣列中的應用程式進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="7201c-208">`Migrate` shouldn't be called from an app in server farm.</span></span> <span data-ttu-id="7201c-209">例如，如果應用程式已使用向外延展 (執行應用程式的多個執行個體) 進行雲端部署。</span><span class="sxs-lookup"><span data-stu-id="7201c-209">For example, if the app has been cloud deployed with scale-out (multiple instances of the app are running).</span></span>

<span data-ttu-id="7201c-210">資料庫移轉應該在部署中以受控制的方式完成。</span><span class="sxs-lookup"><span data-stu-id="7201c-210">Database migration should be done as part of deployment, and in a controlled way.</span></span> <span data-ttu-id="7201c-211">生產環境資料庫移轉方法包括：</span><span class="sxs-lookup"><span data-stu-id="7201c-211">Production database migration approaches include:</span></span>

* <span data-ttu-id="7201c-212">使用移轉來建立 SQL 指令碼，並在部署中使用 SQL 指令碼。</span><span class="sxs-lookup"><span data-stu-id="7201c-212">Using migrations to create SQL scripts and using the SQL scripts in deployment.</span></span>
* <span data-ttu-id="7201c-213">從受控制的環境中執行 `dotnet ef database update`。</span><span class="sxs-lookup"><span data-stu-id="7201c-213">Running `dotnet ef database update` from a controlled environment.</span></span>

<span data-ttu-id="7201c-214">EF Core 使用 `__MigrationsHistory` 資料表來查看是否有任何需要執行的移轉。</span><span class="sxs-lookup"><span data-stu-id="7201c-214">EF Core uses the `__MigrationsHistory` table to see if any migrations need to run.</span></span> <span data-ttu-id="7201c-215">如果資料庫是最新狀態，則不會執行移轉。</span><span class="sxs-lookup"><span data-stu-id="7201c-215">If the DB is up to date, no migration is run.</span></span>

<a id="pmc"></a>
## <a name="command-line-interface-cli-vs-package-manager-console-pmc"></a><span data-ttu-id="7201c-216">命令列介面 (CLI) 與套件管理員主控台 (PMC)</span><span class="sxs-lookup"><span data-stu-id="7201c-216">Command-line interface (CLI) vs. Package Manager Console (PMC)</span></span>

<span data-ttu-id="7201c-217">下列各項可使用管理移轉的 EF Core 工具：</span><span class="sxs-lookup"><span data-stu-id="7201c-217">The EF Core tooling for managing migrations is available from:</span></span>

* <span data-ttu-id="7201c-218">.NET Core CLI 命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-218">.NET Core CLI commands.</span></span>
* <span data-ttu-id="7201c-219">Visual Studio [套件管理員主控台] (PMC) 視窗中的 PowerShell Cmdlet。</span><span class="sxs-lookup"><span data-stu-id="7201c-219">The PowerShell cmdlets in the Visual Studio **Package Manager Console** (PMC) window.</span></span>

<span data-ttu-id="7201c-220">本教學課程示範如何使用 CLI，有些開發人員則偏好使用 PMC。</span><span class="sxs-lookup"><span data-stu-id="7201c-220">This tutorial shows how to use the CLI, some developers prefer using the PMC.</span></span>

<span data-ttu-id="7201c-221">PMC 的 EF Core 命令位於 [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) 套件中。</span><span class="sxs-lookup"><span data-stu-id="7201c-221">The EF Core commands for the PMC are in the [Microsoft.EntityFrameworkCore.Tools](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.Tools) package.</span></span> <span data-ttu-id="7201c-222">此套件包含在 [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) 中繼套件內，因此您不需要安裝它。</span><span class="sxs-lookup"><span data-stu-id="7201c-222">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="7201c-223">**重要事項：** 此套件與透過編輯 *.csproj* 檔案為 CLI 安裝的套件不同。</span><span class="sxs-lookup"><span data-stu-id="7201c-223">**Important:** This isn't the same package as the one you install for the CLI by editing the *.csproj* file.</span></span> <span data-ttu-id="7201c-224">這個套件的名稱以 `Tools` 結尾，不同於以 `Tools.DotNet` 結尾的 CLI 套件名稱。</span><span class="sxs-lookup"><span data-stu-id="7201c-224">The name of this one ends in `Tools`, unlike the CLI package name which ends in `Tools.DotNet`.</span></span>

<span data-ttu-id="7201c-225">如需 CLI 命令的詳細資訊，請參閱 [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="7201c-225">For more information about the CLI commands, see [.NET Core CLI](https://docs.microsoft.com/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="7201c-226">如需 PMC 命令的詳細資訊，請參閱[套件管理員主控台 (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell)。</span><span class="sxs-lookup"><span data-stu-id="7201c-226">For more information about the PMC commands, see [Package Manager Console (Visual Studio)](https://docs.microsoft.com/ef/core/miscellaneous/cli/powershell).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7201c-227">疑難排解</span><span class="sxs-lookup"><span data-stu-id="7201c-227">Troubleshooting</span></span>

<span data-ttu-id="7201c-228">下載[此階段已完成的應用程式](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations)。</span><span class="sxs-lookup"><span data-stu-id="7201c-228">Download the [completed app for this stage](
https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part4-migrations).</span></span>

<span data-ttu-id="7201c-229">應用程式會產生下列例外狀況：</span><span class="sxs-lookup"><span data-stu-id="7201c-229">The app generates the following exception:</span></span>

```text
SqlException: Cannot open database "ContosoUniversity" requested by the login.
The login failed.
Login failed for user 'user name'.
```

<span data-ttu-id="7201c-230">解決方案：執行 `dotnet ef database update`</span><span class="sxs-lookup"><span data-stu-id="7201c-230">Solution: Run `dotnet ef database update`</span></span>

<span data-ttu-id="7201c-231">如果 `update` 命令傳回「建置失敗。」錯誤：</span><span class="sxs-lookup"><span data-stu-id="7201c-231">If the `update` command returns the error "Build failed.":</span></span>

* <span data-ttu-id="7201c-232">再次執行命令。</span><span class="sxs-lookup"><span data-stu-id="7201c-232">Run the command again.</span></span>
* <span data-ttu-id="7201c-233">在頁面底部留下訊息。</span><span class="sxs-lookup"><span data-stu-id="7201c-233">Leave a message at the bottom of the page.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7201c-234">[上一頁](xref:data/ef-rp/sort-filter-page)
> [下一頁](xref:data/ef-rp/complex-data-model)</span><span class="sxs-lookup"><span data-stu-id="7201c-234">[Previous](xref:data/ef-rp/sort-filter-page)
[Next](xref:data/ef-rp/complex-data-model)</span></span>
