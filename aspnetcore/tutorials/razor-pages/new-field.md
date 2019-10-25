---
title: 將欄位新增至 ASP.NET Core 中的 Razor 頁面
author: rick-anderson
description: 示範如何使用 Entity Framework Core 將新欄位新增至 Razor Pages
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: b31711eb6f797de2de1559a3303e14b32a88f1ff
ms.sourcegitcommit: b3ebf96560b75b752d0e71161d788da800ad0999
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/24/2019
ms.locfileid: "72822381"
---
# <a name="add-a-new-field-to-a-razor-page-in-aspnet-core"></a><span data-ttu-id="c0d53-103">將欄位新增至 ASP.NET Core 中的 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="c0d53-103">Add a new field to a Razor Page in ASP.NET Core</span></span>

<span data-ttu-id="c0d53-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="c0d53-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="c0d53-105">在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="c0d53-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="c0d53-106">在模型中新增一個欄位。</span><span class="sxs-lookup"><span data-stu-id="c0d53-106">Add a new field to the model.</span></span>
* <span data-ttu-id="c0d53-107">將新的欄位結構描述變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-107">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="c0d53-108">使用 EF Code First 自動建立資料庫時，Code First 會：</span><span class="sxs-lookup"><span data-stu-id="c0d53-108">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="c0d53-109">將 `__EFMigrationsHistory` 資料表加入至資料庫，以追蹤資料庫的架構是否與其產生的模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="c0d53-109">Adds an `__EFMigrationsHistory` table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="c0d53-110">如果模型類別與資料庫不同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c0d53-110">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="c0d53-111">自動驗證結構描述/模型是否同步，讓您更容易發現不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="c0d53-111">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c0d53-112">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="c0d53-112">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c0d53-113">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="c0d53-113">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="c0d53-114">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="c0d53-114">Build the app.</span></span>

<span data-ttu-id="c0d53-115">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="c0d53-115">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

<a name="addrat"></a>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie30/SnapShots/IndexRating.cshtml?highlight=40-42,62-64)]

<span data-ttu-id="c0d53-116">更新下列頁面：</span><span class="sxs-lookup"><span data-stu-id="c0d53-116">Update the following pages:</span></span>

* <span data-ttu-id="c0d53-117">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="c0d53-117">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="c0d53-118">使用 `Rating` 欄位更新 [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-118">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="c0d53-119">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="c0d53-119">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="c0d53-120">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="c0d53-120">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="c0d53-121">在不更新資料庫的情況下執行應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="c0d53-121">Running the app without updating the database throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="c0d53-122">@No__t_0 例外狀況是因為更新的電影模型類別與資料庫之電影資料表的架構不同。</span><span class="sxs-lookup"><span data-stu-id="c0d53-122">The `SqlException` exception is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="c0d53-123">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-123">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c0d53-124">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="c0d53-124">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c0d53-125">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-125">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="c0d53-126">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="c0d53-126">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c0d53-127">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="c0d53-127">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="c0d53-128">請勿在生產環境資料庫上使用此方法！</span><span class="sxs-lookup"><span data-stu-id="c0d53-128">Don't use this approach on a production database!</span></span> <span data-ttu-id="c0d53-129">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="c0d53-129">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c0d53-130">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="c0d53-130">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c0d53-131">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="c0d53-131">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c0d53-132">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="c0d53-132">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c0d53-133">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="c0d53-133">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c0d53-134">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="c0d53-134">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="c0d53-135">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="c0d53-135">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c0d53-136">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="c0d53-136">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="c0d53-137">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-137">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="c0d53-138">建置方案。</span><span class="sxs-lookup"><span data-stu-id="c0d53-138">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0d53-139">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0d53-139">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="c0d53-140">新增評等欄位的移轉</span><span class="sxs-lookup"><span data-stu-id="c0d53-140">Add a migration for the rating field</span></span>

<span data-ttu-id="c0d53-141">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c0d53-142">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="c0d53-142">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="c0d53-143">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="c0d53-143">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="c0d53-144">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="c0d53-144">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="c0d53-145">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="c0d53-145">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="c0d53-146">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="c0d53-146">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c0d53-147">建議您針對移轉檔案使用有意義的名稱，更加實用。</span><span class="sxs-lookup"><span data-stu-id="c0d53-147">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="c0d53-148">@No__t_0 命令會告訴架構將架構變更套用至資料庫，並保留現有的資料。</span><span class="sxs-lookup"><span data-stu-id="c0d53-148">The `Update-Database` command tells the framework to apply the schema changes to the database and to preserve existing data.</span></span>

<a name="ssox"></a>

<span data-ttu-id="c0d53-149">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="c0d53-149">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="c0d53-150">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="c0d53-150">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="c0d53-151">另一個選擇是刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-151">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="c0d53-152">若要在 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="c0d53-152">To delete the database in SSOX:</span></span>

* <span data-ttu-id="c0d53-153">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-153">Select the database in SSOX.</span></span>
* <span data-ttu-id="c0d53-154">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-154">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="c0d53-155">核取 [關閉現有的連接]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-155">Check **Close existing connections**.</span></span>
* <span data-ttu-id="c0d53-156">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-156">Select **OK**.</span></span>
* <span data-ttu-id="c0d53-157">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c0d53-157">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c0d53-158">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c0d53-158">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="c0d53-159">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="c0d53-159">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="c0d53-160">刪除移轉資料夾。</span><span class="sxs-lookup"><span data-stu-id="c0d53-160">Delete the migration folder.</span></span>  <span data-ttu-id="c0d53-161">使用下列命令重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-161">Use the following commands to recreate the database.</span></span>

```dotnetcli
dotnet ef database drop
dotnet ef migrations add InitialCreate
dotnet ef database update
```

---

<span data-ttu-id="c0d53-162">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="c0d53-162">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c0d53-163">若未植入資料庫，請在 `SeedData.Initialize` 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="c0d53-163">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0d53-164">其他資源</span><span class="sxs-lookup"><span data-stu-id="c0d53-164">Additional resources</span></span>

* [<span data-ttu-id="c0d53-165">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="c0d53-165">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="c0d53-166">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="c0d53-166">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[!INCLUDE[](~/includes/rp/download.md)]

<span data-ttu-id="c0d53-167">在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="c0d53-167">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="c0d53-168">在模型中新增一個欄位。</span><span class="sxs-lookup"><span data-stu-id="c0d53-168">Add a new field to the model.</span></span>
* <span data-ttu-id="c0d53-169">將新的欄位結構描述變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-169">Migrate the new field schema change to the database.</span></span>

<span data-ttu-id="c0d53-170">使用 EF Code First 自動建立資料庫時，Code First 會：</span><span class="sxs-lookup"><span data-stu-id="c0d53-170">When using EF Code First to automatically create a database, Code First:</span></span>

* <span data-ttu-id="c0d53-171">將資料表新增至資料庫，以追蹤資料庫的結構描述是否與其產生的來源模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="c0d53-171">Adds a table to the database to track whether the schema of the database is in sync with the model classes it was generated from.</span></span>
* <span data-ttu-id="c0d53-172">如果模型類別與資料庫不同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c0d53-172">If the model classes aren't in sync with the DB, EF throws an exception.</span></span>

<span data-ttu-id="c0d53-173">自動驗證結構描述/模型是否同步，讓您更容易發現不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="c0d53-173">Automatic verification of schema/model in sync makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="c0d53-174">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="c0d53-174">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="c0d53-175">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="c0d53-175">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="c0d53-176">建置應用程式。</span><span class="sxs-lookup"><span data-stu-id="c0d53-176">Build the app.</span></span>

<span data-ttu-id="c0d53-177">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="c0d53-177">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/IndexRating.cshtml?highlight=40-42,61-63)]

<span data-ttu-id="c0d53-178">更新下列頁面：</span><span class="sxs-lookup"><span data-stu-id="c0d53-178">Update the following pages:</span></span>

* <span data-ttu-id="c0d53-179">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="c0d53-179">Add the `Rating` field to the Delete and Details pages.</span></span>
* <span data-ttu-id="c0d53-180">使用 `Rating` 欄位更新 [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-180">Update [Create.cshtml](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Pages/Movies/Create.cshtml) with a `Rating` field.</span></span>
* <span data-ttu-id="c0d53-181">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="c0d53-181">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="c0d53-182">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="c0d53-182">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="c0d53-183">如果立即執行，應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="c0d53-183">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="c0d53-184">此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述</span><span class="sxs-lookup"><span data-stu-id="c0d53-184">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="c0d53-185">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-185">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="c0d53-186">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="c0d53-186">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="c0d53-187">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-187">Have the Entity Framework automatically drop and re-create the database using the new model class schema.</span></span> <span data-ttu-id="c0d53-188">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="c0d53-188">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="c0d53-189">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="c0d53-189">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="c0d53-190">請勿在生產環境資料庫上使用此方法！</span><span class="sxs-lookup"><span data-stu-id="c0d53-190">Don't use this approach on a production database!</span></span> <span data-ttu-id="c0d53-191">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="c0d53-191">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="c0d53-192">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="c0d53-192">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="c0d53-193">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="c0d53-193">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="c0d53-194">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="c0d53-194">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="c0d53-195">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="c0d53-195">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="c0d53-196">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="c0d53-196">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="c0d53-197">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="c0d53-197">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="c0d53-198">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="c0d53-198">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs?name=snippet1&highlight=8)]

<span data-ttu-id="c0d53-199">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-199">See the [completed SeedData.cs file](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie22/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="c0d53-200">建置方案。</span><span class="sxs-lookup"><span data-stu-id="c0d53-200">Build the solution.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="c0d53-201">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c0d53-201">Visual Studio</span></span>](#tab/visual-studio)

<a name="pmc"></a>

### <a name="add-a-migration-for-the-rating-field"></a><span data-ttu-id="c0d53-202">新增評等欄位的移轉</span><span class="sxs-lookup"><span data-stu-id="c0d53-202">Add a migration for the rating field</span></span>

<span data-ttu-id="c0d53-203">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-203">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c0d53-204">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="c0d53-204">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="c0d53-205">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="c0d53-205">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="c0d53-206">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="c0d53-206">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="c0d53-207">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="c0d53-207">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="c0d53-208">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="c0d53-208">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="c0d53-209">建議您針對移轉檔案使用有意義的名稱，更加實用。</span><span class="sxs-lookup"><span data-stu-id="c0d53-209">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="c0d53-210">`Update-Database` 命令會指示架構將結構描述變更套用至資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-210">The `Update-Database` command tells the framework to apply the schema changes to the database.</span></span>

<a name="ssox"></a>

<span data-ttu-id="c0d53-211">如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="c0d53-211">If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="c0d53-212">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="c0d53-212">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span>

<span data-ttu-id="c0d53-213">另一個選擇是刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-213">Another option is to delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="c0d53-214">若要在 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="c0d53-214">To delete the database in SSOX:</span></span>

* <span data-ttu-id="c0d53-215">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-215">Select the database in SSOX.</span></span>
* <span data-ttu-id="c0d53-216">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-216">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="c0d53-217">核取 [關閉現有的連接]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-217">Check **Close existing connections**.</span></span>
* <span data-ttu-id="c0d53-218">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="c0d53-218">Select **OK**.</span></span>
* <span data-ttu-id="c0d53-219">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫：</span><span class="sxs-lookup"><span data-stu-id="c0d53-219">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database:</span></span>

  ```powershell
  Update-Database
  ```

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="c0d53-220">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c0d53-220">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

### <a name="drop-and-re-create-the-database"></a><span data-ttu-id="c0d53-221">卸除並重新建立資料庫</span><span class="sxs-lookup"><span data-stu-id="c0d53-221">Drop and re-create the database</span></span>

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="c0d53-222">刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="c0d53-222">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="c0d53-223">若要刪除資料庫，請刪除資料庫檔案 (*MvcMovie.db*)。</span><span class="sxs-lookup"><span data-stu-id="c0d53-223">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="c0d53-224">然後執行 `ef database update` 命令：</span><span class="sxs-lookup"><span data-stu-id="c0d53-224">Then run the `ef database update` command:</span></span>

```dotnetcli
dotnet ef database update
```

---

<span data-ttu-id="c0d53-225">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="c0d53-225">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="c0d53-226">若未植入資料庫，請在 `SeedData.Initialize` 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="c0d53-226">If the database isn't seeded, set a break point in the `SeedData.Initialize` method.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c0d53-227">其他資源</span><span class="sxs-lookup"><span data-stu-id="c0d53-227">Additional resources</span></span>

* [<span data-ttu-id="c0d53-228">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="c0d53-228">YouTube version of this tutorial</span></span>](https://youtu.be/3i7uMxiGGR8)

> [!div class="step-by-step"]
> <span data-ttu-id="c0d53-229">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
> [下一步：新增驗證](xref:tutorials/razor-pages/validation)</span><span class="sxs-lookup"><span data-stu-id="c0d53-229">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding Validation](xref:tutorials/razor-pages/validation)</span></span>

::: moniker-end
