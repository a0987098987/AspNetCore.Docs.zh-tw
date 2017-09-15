---
title: "將新欄位新增至 Razor 頁面"
author: rick-anderson
description: "示範如何使用 Entity Framework Core 將新欄位新增至 Razor 頁面"
keywords: "ASP.NET Core,Entity Framework Core,移轉"
ms.author: riande
manager: wpickett
ms.date: 8/7/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: aspnet-core
uid: tutorials/razor-pages/new-field
ms.openlocfilehash: bda00f290043251ad308192c5b1a873ae7cd0d85
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="adding-a-new-field-to-a-razor-page"></a><span data-ttu-id="e0c55-104">將新欄位新增至 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="e0c55-104">Adding a new field to a Razor Page</span></span>

<span data-ttu-id="e0c55-105">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="e0c55-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e0c55-106">在本節中，您會使用 [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First 移轉，將新欄位新增至模型，然後將該變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="e0c55-106">In this section you'll use [Entity Framework](http://docs.efproject.net/en/latest/platforms/aspnetcore/new-db.html) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="e0c55-107">當您使用 EF Code First 自動建立資料庫時，Code First 會將資料表新增至資料庫，以協助追蹤資料庫的結構描述是否與其產生的來源模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="e0c55-107">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="e0c55-108">如果未同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e0c55-108">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="e0c55-109">這可讓您更輕鬆地找出不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="e0c55-109">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="e0c55-110">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="e0c55-110">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="e0c55-111">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e0c55-111">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

<span data-ttu-id="e0c55-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span><span class="sxs-lookup"><span data-stu-id="e0c55-112">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]</span></span>

<span data-ttu-id="e0c55-113">建置應用程式 (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="e0c55-113">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="e0c55-114">編輯 *Pages/Movies/Index.cshtml*，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="e0c55-114">Edit *Pages/Movies/Index.cshtml*, and add a `Rating` field:</span></span>

<span data-ttu-id="e0c55-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span><span class="sxs-lookup"><span data-stu-id="e0c55-115">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Index.cshtml?highlight=40-42,61-63)]</span></span>

<span data-ttu-id="e0c55-116">將 `Rating` 欄位新增至 Delete 和 Details 頁面。</span><span class="sxs-lookup"><span data-stu-id="e0c55-116">Add the `Rating` field to the Delete and Details pages.</span></span>

<span data-ttu-id="e0c55-117">使用 `Rating` 欄位更新 *Create.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e0c55-117">Update *Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="e0c55-118">您可以複製/貼上前一個 `<div>` 項目，讓 IntelliSense 協助您更新這些欄位。</span><span class="sxs-lookup"><span data-stu-id="e0c55-118">You can copy/paste the previous `<div>` element and let intelliSense help you update the fields.</span></span> <span data-ttu-id="e0c55-119">IntelliSense 會使用[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="e0c55-119">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![在檢視的第二個 Label 項目中，開發人員已針對 asp-for 屬性值鍵入字母 R。](new-field/_static/cr.png)

<span data-ttu-id="e0c55-123">下列程式碼會示範 *Create.cshtml* 與 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="e0c55-123">The following code shows *Create.cshtml* with a `Rating` field:</span></span>

<span data-ttu-id="e0c55-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span><span class="sxs-lookup"><span data-stu-id="e0c55-124">[!code-cshtml[Main](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?highlight=31-35)]</span></span>

<span data-ttu-id="e0c55-125">將 `Rating` 欄位新增至 Edit 頁面。</span><span class="sxs-lookup"><span data-stu-id="e0c55-125">Add the `Rating` field to the Edit Page.</span></span>

<span data-ttu-id="e0c55-126">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="e0c55-126">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="e0c55-127">如果立即執行，應用程式會擲回 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="e0c55-127">If run now, the app throws a `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="e0c55-128">此錯誤是因為更新的電影模型類別，不同於資料庫之電影資料表的結構描述</span><span class="sxs-lookup"><span data-stu-id="e0c55-128">This error is caused by the updated Movie model class being different than the schema of the Movie table of the database.</span></span> <span data-ttu-id="e0c55-129">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="e0c55-129">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="e0c55-130">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="e0c55-130">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="e0c55-131">讓 Entity Framework 自動卸除資料庫，並使用新的模型類別結構描述來重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="e0c55-131">Have the Entity Framework automatically drop and re-create the database using  the new model class schema.</span></span> <span data-ttu-id="e0c55-132">在開發週期早期，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="e0c55-132">This approach is convenient early in the development cycle; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="e0c55-133">缺點是會遺失在資料庫中的現有資料。</span><span class="sxs-lookup"><span data-stu-id="e0c55-133">The downside is that you lose existing data in the database.</span></span> <span data-ttu-id="e0c55-134">您不想要在生產環境資料庫上使用此方法 ！</span><span class="sxs-lookup"><span data-stu-id="e0c55-134">You don't want to use this approach on a production database!</span></span> <span data-ttu-id="e0c55-135">在結構描述變更時卸除資料庫以及使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="e0c55-135">Dropping the DB on schema changes and using an initializer to automatically seed the database with test data is often a productive way to develop an app.</span></span>

2. <span data-ttu-id="e0c55-136">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="e0c55-136">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="e0c55-137">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="e0c55-137">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="e0c55-138">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="e0c55-138">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="e0c55-139">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e0c55-139">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="e0c55-140">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="e0c55-140">For this tutorial, use Code First Migrations.</span></span>

<span data-ttu-id="e0c55-141">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="e0c55-141">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="e0c55-142">範例變更如下所示，但您會想要為每個 `new Movie` 區塊進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="e0c55-142">A sample change is shown below, but you'll want to make this change for each `new Movie` block.</span></span>

<span data-ttu-id="e0c55-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="e0c55-143">[!code-csharp[Main](razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]</span></span>

<span data-ttu-id="e0c55-144">請參閱[完整的 SeedData.cs 檔案](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs) (英文)。</span><span class="sxs-lookup"><span data-stu-id="e0c55-144">See the [completed SeedData.cs file](https://github.com/aspnet/Docs/blob/master/aspnetcore/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie/Models/SeedDataRating.cs).</span></span>

<span data-ttu-id="e0c55-145">建置方案。</span><span class="sxs-lookup"><span data-stu-id="e0c55-145">Build the solution.</span></span>

<a name="pmc"></a>

<span data-ttu-id="e0c55-146">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="e0c55-146">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="e0c55-147">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="e0c55-147">In the PMC, enter the following commands:</span></span>

```PMC
Add-Migration Rating
Update-Database
```

<span data-ttu-id="e0c55-148">`Add-Migration` 命令會告知架構，以便：</span><span class="sxs-lookup"><span data-stu-id="e0c55-148">The `Add-Migration` command tells the framework to:</span></span>

* <span data-ttu-id="e0c55-149">比較 `Movie` 模型與 `Movie` 資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="e0c55-149">Compare the `Movie` model with the `Movie` DB schema.</span></span>
* <span data-ttu-id="e0c55-150">建立程式碼，將資料庫結構描述移轉至新模型。</span><span class="sxs-lookup"><span data-stu-id="e0c55-150">Create code to migrate the DB schema to the new model.</span></span>

<span data-ttu-id="e0c55-151">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="e0c55-151">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="e0c55-152">建議您針對移轉檔案使用有意義的名稱，這更加實用。</span><span class="sxs-lookup"><span data-stu-id="e0c55-152">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="e0c55-153"><a name="ssox"></a> 如果您刪除資料庫中的所有記錄，初始設定式會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="e0c55-153"><a name="ssox"></a> If you delete all the records in the DB, the initializer will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="e0c55-154">您可以使用瀏覽器或 [Sql Server 物件總管](xref:tutorials/razor-pages/sql#ssox) (SSOX) 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="e0c55-154">You can do this with the delete links in the browser or from [Sql Server Object Explorer](xref:tutorials/razor-pages/sql#ssox) (SSOX).</span></span> <span data-ttu-id="e0c55-155">若要從 SSOX 中刪除資料庫：</span><span class="sxs-lookup"><span data-stu-id="e0c55-155">To delete the database from SSOX:</span></span>

* <span data-ttu-id="e0c55-156">在 SSOX 中選取資料庫。</span><span class="sxs-lookup"><span data-stu-id="e0c55-156">Select the database in SSOX.</span></span>
* <span data-ttu-id="e0c55-157">以滑鼠右鍵按一下資料庫，然後選取 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="e0c55-157">Right click on the database, and select *Delete*.</span></span>
* <span data-ttu-id="e0c55-158">核取 *[關閉現有的連接]</span><span class="sxs-lookup"><span data-stu-id="e0c55-158">Check **Close existing connections*</span></span>
* <span data-ttu-id="e0c55-159">選取 [確定]</span><span class="sxs-lookup"><span data-stu-id="e0c55-159">Select **OK**</span></span>
* <span data-ttu-id="e0c55-160">在 [PMC](xref:tutorials/razor-pages/new-field#pmc) 中，更新資料庫</span><span class="sxs-lookup"><span data-stu-id="e0c55-160">In the [PMC](xref:tutorials/razor-pages/new-field#pmc), update the database</span></span> 

    ```PMC
    Update-Database
    ```

<span data-ttu-id="e0c55-161">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="e0c55-161">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="e0c55-162">如果未植入資料庫，請停止 IIS Express，然後執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e0c55-162">If the database is not seeded, stop IIS Express, and then run the app.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="e0c55-163">[上一步：新增搜尋](xref:tutorials/razor-pages/search)
[下一步：新增欄位](xref:tutorials/razor-pages/new-field)</span><span class="sxs-lookup"><span data-stu-id="e0c55-163">[Previous: Adding Search](xref:tutorials/razor-pages/search)
[Next: Adding a new field](xref:tutorials/razor-pages/new-field)</span></span>
