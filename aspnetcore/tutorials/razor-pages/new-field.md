---
title: 將欄位新增至 ASP.NET Core 中的 Razor 頁面
author: rick-anderson
description: 示範如何使用 Entity Framework Core 將新欄位新增至 Razor Pages
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 1b08e1515afe656b95be9fb436caa00cd53ab9ad
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334097"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="5c914-103">將欄位新增至 ASP.NET Core 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="5c914-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="5c914-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="5c914-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="5c914-105">在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="5c914-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="5c914-106">在模型中新增一個欄位。</span><span class="sxs-lookup"><span data-stu-id="5c914-106">Add a new field to the model.</span></span>
* <span data-ttu-id="5c914-107">將新的欄位結構描述變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="5c914-108">使用 EF Code First 自動建立資料庫時，Code First 會：</span><span class="sxs-lookup"><span data-stu-id="5c914-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="5c914-109">將 `__EFMigrationsHistory` 資料表加入至資料庫，以追蹤資料庫的架構是否與其產生的模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="5c914-109">Adds an `__EFMigrationsHistory` table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="5c914-110">如果模型類別與資料庫不同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5c914-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="5c914-111">自動驗證結構描述/模型是否同步，讓您更容易發現不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="5c914-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="5c914-112">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="5c914-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="5c914-113">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="5c914-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="5c914-114">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="5c914-114">Build the app.</span></span>

<span data-ttu-id="5c914-115">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="5c914-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,62-64)]

<span data-ttu-id="5c914-116">更新下列頁面：</span><span class="sxs-lookup"><span data-stu-id="5c914-116">Update the following pages:</span></span>

* <span data-ttu-id="5c914-117">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="5c914-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="5c914-118">使用 `Rating` 欄位更新 [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="5c914-118">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="5c914-119">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="5c914-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="5c914-120">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="5c914-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="5c914-121">在不更新資料庫的情況下執行應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="5c914-121">Running the app without updating the database throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="5c914-122">@No__t_0 例外狀況是因為更新的電影模型類別與資料庫之電影資料表的架構不同。</span><span class="sxs-lookup"><span data-stu-id="5c914-122">The `SqlException` exception is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="5c914-123">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="5c914-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="5c914-124">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="5c914-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="5c914-125">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="5c914-126">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="5c914-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="5c914-127">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="5c914-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="5c914-128">請勿在生產環境資料庫上使用此方法！</span><span class="sxs-lookup"><span data-stu-id="5c914-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="5c914-129">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="5c914-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="5c914-130">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="5c914-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="5c914-131">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="5c914-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="5c914-132">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="5c914-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="5c914-133">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="5c914-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="5c914-134">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="5c914-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="5c914-135">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="5c914-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="5c914-136">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="5c914-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="5c914-137">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="5c914-137">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="5c914-138">建置方案。</span><span class="sxs-lookup"><span data-stu-id="5c914-138">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c914-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c914-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="5c914-140">新增評等欄位的移轉</span><span class="sxs-lookup"><span data-stu-id="5c914-140">Add a migration for the rating field</span></span>

<span data-ttu-id="5c914-141">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="5c914-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="5c914-142">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5c914-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="5c914-143">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="5c914-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="5c914-144">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="5c914-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="5c914-145">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="5c914-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="5c914-146">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="5c914-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="5c914-147">建議您針對移轉檔案使用有意義的名稱，更加實用。</span><span class="sxs-lookup"><span data-stu-id="5c914-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="5c914-148">@No__t_0 命令會告訴架構將架構變更套用至資料庫，並保留現有的資料。</span><span class="sxs-lookup"><span data-stu-id="5c914-148">The `Update-Database` command tells the framework to apply the schema changes to the database and to preserve existing data.</span></span>

<a name="ssox"></a>

<span data-ttu-id="5c914-149">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="5c914-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="5c914-150">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="5c914-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="5c914-151">另一個選擇是刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="5c914-152">若要在 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="5c914-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="5c914-153">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="5c914-154">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="5c914-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="5c914-155">核取 [關閉現有的連接]。</span><span class="sxs-lookup"><span data-stu-id="5c914-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="5c914-156">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="5c914-156">Select **OK**.</span></span>
* <span data-ttu-id="5c914-157">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="5c914-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c914-158">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c914-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="5c914-159">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="5c914-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="5c914-160">刪除移轉資料夾。</span><span class="sxs-lookup"><span data-stu-id="5c914-160">Delete the migration folder.</span></span>  <span data-ttu-id="5c914-161">使用下列命令重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-161">Use the following commands to recreate the database.</span></span>

```dotnetcli
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

<span data-ttu-id="5c914-162">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="5c914-162">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="5c914-163">若未植入資料庫，請在 `SeedData.Initialize` 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="5c914-163">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c914-164">其他資源</span><span class="sxs-lookup"><span data-stu-id="5c914-164">Additional resources</span></span>

* [<span data-ttu-id="5c914-165">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="5c914-165">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="5c914-166">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="5c914-166">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="5c914-167">在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="5c914-167">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="5c914-168">在模型中新增一個欄位。</span><span class="sxs-lookup"><span data-stu-id="5c914-168">Add a new field to the model.</span></span>
* <span data-ttu-id="5c914-169">將新的欄位結構描述變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-169">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="5c914-170">使用 EF Code First 自動建立資料庫時，Code First 會：</span><span class="sxs-lookup"><span data-stu-id="5c914-170">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="5c914-171">將資料表新增至資料庫，以追蹤資料庫的結構描述是否與其產生的來源模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="5c914-171">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="5c914-172">如果模型類別與資料庫不同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="5c914-172">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="5c914-173">自動驗證結構描述/模型是否同步，讓您更容易發現不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="5c914-173">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="5c914-174">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="5c914-174">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="5c914-175">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="5c914-175">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="5c914-176">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="5c914-176">Build the app.</span></span>

<span data-ttu-id="5c914-177">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="5c914-177">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="5c914-178">更新下列頁面：</span><span class="sxs-lookup"><span data-stu-id="5c914-178">Update the following pages:</span></span>

* <span data-ttu-id="5c914-179">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="5c914-179">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="5c914-180">使用 `Rating` 欄位更新 [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="5c914-180">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="5c914-181">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="5c914-181">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="5c914-182">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="5c914-182">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="5c914-183">如果立即執行，應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="5c914-183">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="5c914-184">此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述</span><span class="sxs-lookup"><span data-stu-id="5c914-184">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="5c914-185">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="5c914-185">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="5c914-186">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="5c914-186">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="5c914-187">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-187">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="5c914-188">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="5c914-188">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="5c914-189">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="5c914-189">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="5c914-190">請勿在生產環境資料庫上使用此方法！</span><span class="sxs-lookup"><span data-stu-id="5c914-190">Don't use this approach on a production database!</span></span> <span data-ttu-id="5c914-191">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="5c914-191">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="5c914-192">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="5c914-192">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="5c914-193">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="5c914-193">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="5c914-194">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="5c914-194">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="5c914-195">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="5c914-195">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="5c914-196">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="5c914-196">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="5c914-197">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="5c914-197">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="5c914-198">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="5c914-198">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="5c914-199">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="5c914-199">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="5c914-200">建置方案。</span><span class="sxs-lookup"><span data-stu-id="5c914-200">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="5c914-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="5c914-201">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="5c914-202">新增評等欄位的移轉</span><span class="sxs-lookup"><span data-stu-id="5c914-202">Add a migration for the rating field</span></span>

<span data-ttu-id="5c914-203">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="5c914-203">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="5c914-204">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="5c914-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="5c914-205">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="5c914-205">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="5c914-206">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="5c914-206">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="5c914-207">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="5c914-207">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="5c914-208">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="5c914-208">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="5c914-209">建議您針對移轉檔案使用有意義的名稱，更加實用。</span><span class="sxs-lookup"><span data-stu-id="5c914-209">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="5c914-210">`Update-Database` 命令會指示架構將結構描述變更套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-210">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="5c914-211">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="5c914-211">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="5c914-212">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="5c914-212">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="5c914-213">另一個選擇是刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-213">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="5c914-214">若要在 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="5c914-214">To delete the database in SSOX:</span></span>

* <span data-ttu-id="5c914-215">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-215">Select the database in SSOX.</span></span>
* <span data-ttu-id="5c914-216">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="5c914-216">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="5c914-217">核取 [關閉現有的連接]。</span><span class="sxs-lookup"><span data-stu-id="5c914-217">Check **Close existing connections**.</span></span>
* <span data-ttu-id="5c914-218">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="5c914-218">Select **OK**.</span></span>
* <span data-ttu-id="5c914-219">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="5c914-219">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="5c914-220">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5c914-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="5c914-221">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="5c914-221">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="5c914-222">刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="5c914-222">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="5c914-223">若要刪除資料庫，請刪除資料庫檔案 (*MvcMovie.db*)。</span><span class="sxs-lookup"><span data-stu-id="5c914-223">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="5c914-224">然後執行 `ef database update` 命令：</span><span class="sxs-lookup"><span data-stu-id="5c914-224">Then run the `ef database update` command:</span></span>

```dotnetcli
dotnet ef database update
```

---

<span data-ttu-id="5c914-225">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="5c914-225">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="5c914-226">若未植入資料庫，請在 `SeedData.Initialize` 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="5c914-226">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="5c914-227">其他資源</span><span class="sxs-lookup"><span data-stu-id="5c914-227">Additional resources</span></span>

* [<span data-ttu-id="5c914-228">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="5c914-228">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="5c914-229">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="5c914-229">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end
