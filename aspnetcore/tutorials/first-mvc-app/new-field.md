---
title: 將欄位新增至 ASP.NET Core MVC 應用程式
author: rick-anderson
description: 了解如何使用 Entity Framework Code First 移轉，將欄位新增至模型，然後將該變更移轉至資料庫。
ms.author: riande
ms.custom: mvc
ms.date: 12/13/2018
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 6a2a2ca45f793ab95d45281ebb23180ac64761ec
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71082319"
---
# <a name="add-a-new-field-to-an-aspnet-core-mvc-app"></a><span data-ttu-id="085eb-103">將欄位新增至 ASP.NET Core MVC 應用程式</span><span class="sxs-lookup"><span data-stu-id="085eb-103">Add a new field to an ASP.NET Core MVC app</span></span>

<span data-ttu-id="085eb-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="085eb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="085eb-105">在本節中，您會使用 [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="085eb-105">In this section [Entity Framework](/ef/core/get-started/aspnetcore/new-db) Code First Migrations is used to:</span></span>

* <span data-ttu-id="085eb-106">在模型中新增一個欄位。</span><span class="sxs-lookup"><span data-stu-id="085eb-106">Add a new field to the model.</span></span>
* <span data-ttu-id="085eb-107">將新的欄位移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="085eb-107">Migrate the new field to the database.</span></span>

<span data-ttu-id="085eb-108">使用 EF Code First 自動建立資料庫時，Code First 會：</span><span class="sxs-lookup"><span data-stu-id="085eb-108">When EF Code First is used to automatically create a database, Code First:</span></span>

* <span data-ttu-id="085eb-109">將資料表新增至資料庫，以追蹤資料庫的結構描述。</span><span class="sxs-lookup"><span data-stu-id="085eb-109">Adds a table to the database to  track the schema of the database.</span></span>
* <span data-ttu-id="085eb-110">驗證資料庫與其產生來源的模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="085eb-110">Verifies the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="085eb-111">如果未同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="085eb-111">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="085eb-112">這可讓您更輕鬆地找出不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="085eb-112">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="add-a-rating-property-to-the-movie-model"></a><span data-ttu-id="085eb-113">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="085eb-113">Add a Rating Property to the Movie Model</span></span>

<span data-ttu-id="085eb-114">將 `Rating` 屬性新增至 *Models/Movie.cs*：</span><span class="sxs-lookup"><span data-stu-id="085eb-114">Add a `Rating` property to *Models/Movie.cs*:</span></span>

[!code-csharp[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Models/MovieDateRating.cs?highlight=13&name=snippet)]

<span data-ttu-id="085eb-115">建置應用程式</span><span class="sxs-lookup"><span data-stu-id="085eb-115">Build the app</span></span>

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="085eb-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="085eb-116">Visual Studio</span></span>](#tab/visual-studio)

 <span data-ttu-id="085eb-117">CTRL+SHIFT+B</span><span class="sxs-lookup"><span data-stu-id="085eb-117">Ctrl+Shift+B</span></span>

### <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="085eb-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="085eb-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet build
```

### <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="085eb-119">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="085eb-119">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="085eb-120">Command ⌘ + B</span><span class="sxs-lookup"><span data-stu-id="085eb-120">Command ⌘ + B</span></span>

------

<span data-ttu-id="085eb-121">因為您已將欄位新增至 `Movie` 類別，所以需要更新繫結允許清單，以便包含這個新屬性。</span><span class="sxs-lookup"><span data-stu-id="085eb-121">Because you've added a new field to the `Movie` class, you need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="085eb-122">在 *MoviesController.cs* 中，更新 `Create` 和 `Edit` 這兩個動作方法的 `[Bind]` 屬性 (attribute)，以包括 `Rating` 屬性 (property)：</span><span class="sxs-lookup"><span data-stu-id="085eb-122">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("Id,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="085eb-123">更新檢視範本，以便在瀏覽器檢視中顯示、建立和編輯新的 `Rating` 屬性。</span><span class="sxs-lookup"><span data-stu-id="085eb-123">Update the view templates in order to display, create, and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="085eb-124">編輯 */Views/Movies/Index.cshtml* 檔案，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="085eb-124">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[](~/tutorials/first-mvc-app/start-mvc/sample/MvcMovie22/Views/Movies/IndexGenreRating.cshtml?highlight=16,38&range=24-64)]

<span data-ttu-id="085eb-125">使用 `Rating` 欄位更新 */Views/Movies/Create.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="085eb-125">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span>

# <a name="visual-studio--visual-studio-for-mactabvisual-studiovisual-studio-mac"></a>[<span data-ttu-id="085eb-126">Visual Studio / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="085eb-126">Visual Studio / Visual Studio for Mac</span></span>](#tab/visual-studio+visual-studio-mac)

<span data-ttu-id="085eb-127">您可以複製/貼上前一個「表單群組」，讓 IntelliSense 協助您更新這些欄位。</span><span class="sxs-lookup"><span data-stu-id="085eb-127">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="085eb-128">IntelliSense 會使用[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="085eb-128">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span>

![在檢視的第二個 Label 項目中，開發人員已針對 asp-for 屬性值鍵入字母 R。](new-field/_static/cr.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="085eb-132">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="085eb-132">Visual Studio Code</span></span>](#tab/visual-studio-code)

<!-- This tab intentionally left blank. -->

---

<span data-ttu-id="085eb-133">更新其餘的範本。</span><span class="sxs-lookup"><span data-stu-id="085eb-133">Update the remaining templates.</span></span>

<span data-ttu-id="085eb-134">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="085eb-134">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="085eb-135">範例變更如下所示，但您會想要為每個 `new Movie` 進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="085eb-135">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="085eb-136">在更新資料庫以包含新欄位之前，應用程式無法運作。</span><span class="sxs-lookup"><span data-stu-id="085eb-136">The app won't work until the DB is updated to include the new field.</span></span> <span data-ttu-id="085eb-137">如果立即執行，則會擲回下列 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="085eb-137">If it's run now, the following `SqlException` is thrown:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="085eb-138">此錯誤是因為更新的電影模型類別，不同於現有資料庫之電影資料表的結構描述。</span><span class="sxs-lookup"><span data-stu-id="085eb-138">This error occurs because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="085eb-139">(資料庫資料表中沒有任何 `Rating` 資料行)。</span><span class="sxs-lookup"><span data-stu-id="085eb-139">(There's no `Rating` column in the database table.)</span></span>

<span data-ttu-id="085eb-140">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="085eb-140">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="085eb-141">讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="085eb-141">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="085eb-142">在開發週期早期，當您在測試資料庫上進行開發時，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="085eb-142">This approach is very convenient early in the development cycle when you're doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="085eb-143">不過，它的缺點是您會遺失資料庫中現有的資料 — 因此您不會想在實際執行的資料庫上使用這種方法！</span><span class="sxs-lookup"><span data-stu-id="085eb-143">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="085eb-144">使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="085eb-144">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span> <span data-ttu-id="085eb-145">這是早期開發和使用 SQLite 時的好方法。</span><span class="sxs-lookup"><span data-stu-id="085eb-145">This is a good approach for early development and when using SQLite.</span></span>

2. <span data-ttu-id="085eb-146">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="085eb-146">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="085eb-147">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="085eb-147">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="085eb-148">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="085eb-148">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="085eb-149">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="085eb-149">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="085eb-150">在本教學課程中，請使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="085eb-150">For this tutorial, Code First Migrations is used.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="085eb-151">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="085eb-151">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="085eb-152">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="085eb-152">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC 功能表](adding-model/_static/pmc.png)

<span data-ttu-id="085eb-154">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="085eb-154">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="085eb-155">`Add-Migration` 命令會告知移轉架構，檢查目前的 `Movie` 模型與目前的 `Movie` 資料庫結構描述，並建立必要的程式碼以將資料庫移轉至新的模型。</span><span class="sxs-lookup"><span data-stu-id="085eb-155">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span>

<span data-ttu-id="085eb-156">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="085eb-156">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="085eb-157">建議您針對移轉檔案使用有意義的名稱，這更加實用。</span><span class="sxs-lookup"><span data-stu-id="085eb-157">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="085eb-158">如果刪除資料庫中的所有記錄，初始化方法會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="085eb-158">If all the records in the DB are deleted, the initialize method will seed the DB and include the `Rating` field.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="085eb-159">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="085eb-159">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

[!INCLUDE[](~/includes/RP-mvc-shared/sqlite-warn.md)]

<span data-ttu-id="085eb-160">刪除資料庫並使用移轉重新建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="085eb-160">Delete the database and use migrations to re-create the database.</span></span> <span data-ttu-id="085eb-161">若要刪除資料庫，請刪除資料庫檔案 (*MvcMovie.db*)。</span><span class="sxs-lookup"><span data-stu-id="085eb-161">To delete the database, delete the database file (*MvcMovie.db*).</span></span> <span data-ttu-id="085eb-162">然後執行 `ef database update` 命令：</span><span class="sxs-lookup"><span data-stu-id="085eb-162">Then run the `ef database update` command:</span></span>

```dotnetcli
dotnet ef database update
```

---
<!-- End of VS tabs -->

<span data-ttu-id="085eb-163">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="085eb-163">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="085eb-164">您應該將 `Rating` 欄位新增至 `Edit`、`Details` 和 `Delete` 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="085eb-164">You should add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="085eb-165">[上一頁](search.md)
> [下一頁](validation.md)</span><span class="sxs-lookup"><span data-stu-id="085eb-165">[Previous](search.md)
[Next](validation.md)</span></span>
