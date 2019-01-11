---
title: 將欄位新增至 ASP.NET Core 中的 Razor 頁面
author: rick-anderson
description: 示範如何使用 Entity Framework Core 將新欄位新增至 Razor Pages
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/5/2018
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: 9b3ad5f6c4b1c9b5f016f5591127c8d1b213948d
ms.sourcegitcommit: 1ea1b4fc58055c62728143388562689f1ef96cb2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/13/2018
ms.locfileid: "53329129"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="52e29-103">將欄位新增至 ASP.NET Core 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="52e29-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="52e29-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="52e29-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="52e29-105">在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="52e29-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="52e29-106">在模型中新增一個欄位。</span><span class="sxs-lookup"><span data-stu-id="52e29-106">Add a new field to the model.</span></span>
* <span data-ttu-id="52e29-107">將新的欄位結構描述變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="52e29-108">使用 EF Code First 自動建立資料庫時，Code First 會：</span><span class="sxs-lookup"><span data-stu-id="52e29-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="52e29-109">將資料表新增至資料庫，以追蹤資料庫的結構描述是否與其產生的來源模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="52e29-109">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="52e29-110">如果模型類別與資料庫不同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="52e29-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="52e29-111">自動驗證結構描述/模型是否同步，讓您更容易發現不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="52e29-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="52e29-112">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="52e29-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="52e29-113">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="52e29-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="52e29-114">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="52e29-114">Build the app.</span></span>

<span data-ttu-id="52e29-115">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="52e29-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml.?highlight=40-42,61-63)]

<span data-ttu-id="52e29-116">更新下列頁面：</span><span class="sxs-lookup"><span data-stu-id="52e29-116">Update the following pages:</span></span>

* <span data-ttu-id="52e29-117">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="52e29-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="52e29-118">使用 `Rating` 欄位更新 [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="52e29-118">Update [Create.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="52e29-119">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="52e29-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="52e29-120">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="52e29-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="52e29-121">如果立即執行，應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="52e29-121">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="52e29-122">此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述</span><span class="sxs-lookup"><span data-stu-id="52e29-122">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="52e29-123">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="52e29-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="52e29-124">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="52e29-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="52e29-125">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="52e29-126">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="52e29-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="52e29-127">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="52e29-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="52e29-128">請勿在生產環境資料庫上使用此方法！</span><span class="sxs-lookup"><span data-stu-id="52e29-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="52e29-129">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="52e29-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="52e29-130">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="52e29-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="52e29-131">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="52e29-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="52e29-132">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="52e29-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="52e29-133">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="52e29-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="52e29-134">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="52e29-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="52e29-135">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="52e29-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="52e29-136">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="52e29-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="52e29-137">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="52e29-137">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="52e29-138">建置方案。</span><span class="sxs-lookup"><span data-stu-id="52e29-138">Build the solution.</span></span>

<!-- VS -------------------------->
# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="52e29-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="52e29-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="52e29-140">新增評等欄位的移轉</span><span class="sxs-lookup"><span data-stu-id="52e29-140">Add a migration for the rating field</span></span>

<span data-ttu-id="52e29-141">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="52e29-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="52e29-142">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="52e29-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="52e29-143">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="52e29-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="52e29-144">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="52e29-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="52e29-145">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="52e29-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="52e29-146">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="52e29-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="52e29-147">建議您針對移轉檔案使用有意義的名稱，這更加實用。</span><span class="sxs-lookup"><span data-stu-id="52e29-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="52e29-148">`Update-Database` 命令會指示架構將結構描述變更套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-148">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="52e29-149">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="52e29-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="52e29-150">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="52e29-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="52e29-151">另一個選擇是刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="52e29-152">若要在 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="52e29-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="52e29-153">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="52e29-154">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="52e29-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="52e29-155">核取 [關閉現有的連接]。</span><span class="sxs-lookup"><span data-stu-id="52e29-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="52e29-156">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="52e29-156">Select **OK**.</span></span>
* <span data-ttu-id="52e29-157">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="52e29-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

<!-- Code -------------------------->
# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="52e29-158">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="52e29-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<!-- copy/paste this tab to the next. Not worth an include  -->

<span data-ttu-id="52e29-159">執行下列 .NET Core CLI 命令：</span><span class="sxs-lookup"><span data-stu-id="52e29-159">Run the following .NET Core CLI commands:</span></span>

```console
dotnet ef migrations add Rating
dotnet ef database update
```

<span data-ttu-id="52e29-160">`ef migrations add` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="52e29-160">The `ef migrations add` command tells the framework to:</span></span>

* <span data-ttu-id="52e29-161">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="52e29-161">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="52e29-162">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="52e29-162">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="52e29-163">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="52e29-163">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="52e29-164">建議您針對移轉檔案使用有意義的名稱，這更加實用。</span><span class="sxs-lookup"><span data-stu-id="52e29-164">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="52e29-165">`ef database update` 命令會指示架構將結構描述變更套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-165">The `ef database update` command tells the framework to apply the schema changes to the database.</span></span>

<span data-ttu-id="52e29-166">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="52e29-166">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="52e29-167">您可以使用瀏覽器中的刪除連結或使用 SQLite 工具來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="52e29-167">You can do this with the delete links in the browser or by using a SQLite tool.</span></span>

<span data-ttu-id="52e29-168">另一個選擇是刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="52e29-168">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="52e29-169">若要刪除資料庫，請刪除資料庫檔案 (*MvcMovie.db*)。</span><span class="sxs-lookup"><span data-stu-id="52e29-169">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="52e29-170">然後執行 `ef database update` 命令：</span><span class="sxs-lookup"><span data-stu-id="52e29-170">Then run the `ef database update` command:</span></span> 

```console
dotnet ef database update
```

> [!NOTE]
> <span data-ttu-id="52e29-171">EF Core SQLite 提供者不支援許多結構描述變更作業。</span><span class="sxs-lookup"><span data-stu-id="52e29-171">Many schema change operations are not supported by the EF Core SQLite provider.</span></span> <span data-ttu-id="52e29-172">例如，其支援新增資料行，但不支援移除資料行。</span><span class="sxs-lookup"><span data-stu-id="52e29-172">For example, adding a column is supported, but removing a column is not supported.</span></span> <span data-ttu-id="52e29-173">如果您新增移轉來移除資料行，`ef migrations add` 命令會成功，但 `ef database update` 命令會失敗。</span><span class="sxs-lookup"><span data-stu-id="52e29-173">If you add a migration to remove a column, the `ef migrations add` command succeeds but the `ef database update` command fails.</span></span> <span data-ttu-id="52e29-174">您可以手動撰寫移轉程式碼來重建資料表，藉此解決一些限制。</span><span class="sxs-lookup"><span data-stu-id="52e29-174">You can work around some of the limitations by manually writing migrations code to perform a table rebuild.</span></span> <span data-ttu-id="52e29-175">重建資料表需要重新命名現有的資料表、建立新的資料表、將資料複製到新的資料表，以及卸除舊的資料表。</span><span class="sxs-lookup"><span data-stu-id="52e29-175">A table rebuild involves renaming the existing table, creating a new table, copying data to the new table, and dropping the old table.</span></span> <span data-ttu-id="52e29-176">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="52e29-176">For more information, see the following resources:</span></span>
> * [<span data-ttu-id="52e29-177">SQLite EF Core 資料庫提供者限制</span><span class="sxs-lookup"><span data-stu-id="52e29-177">SQLite EF Core Database Provider Limitations</span></span>](/ef/core/providers/sqlite/limitations)
> * [<span data-ttu-id="52e29-178">自訂移轉程式碼</span><span class="sxs-lookup"><span data-stu-id="52e29-178">Customize migration code</span></span>](/ef/core/managing-schemas/migrations/#customize-migration-code)
> * [<span data-ttu-id="52e29-179">資料植入</span><span class="sxs-lookup"><span data-stu-id="52e29-179">Data seeding</span></span>](/ef/core/modeling/data-seeding)

---  
<!-- End of VS tabs -->

<span data-ttu-id="52e29-180">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="52e29-180">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="52e29-181">若未植入資料庫，請在 `SeedData.Initialize` 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="52e29-181">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="52e29-182">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="52e29-182">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>
