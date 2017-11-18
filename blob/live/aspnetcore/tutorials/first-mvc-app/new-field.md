---
title: "新增欄位"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: get-started-article
ms.assetid: 16efbacf-fe7b-4b41-84b0-06a1574b95c2
ms.technology: aspnet
ms.prod: asp.net-core
uid: tutorials/first-mvc-app/new-field
ms.openlocfilehash: 9c872a48aba4974ddac2e49ca40c944da356f0e0
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="adding-a-new-field"></a><span data-ttu-id="534e8-103">新增欄位</span><span class="sxs-lookup"><span data-stu-id="534e8-103">Adding a New Field</span></span>

<span data-ttu-id="534e8-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="534e8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="534e8-105">在本節中，您會使用 [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First 移轉，將欄位新增至模型，然後將該變更移轉至資料庫。</span><span class="sxs-lookup"><span data-stu-id="534e8-105">In this section you'll use [Entity Framework](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) Code First Migrations to add a new field to the model and migrate that change to the database.</span></span>

<span data-ttu-id="534e8-106">當您使用 EF Code First 自動建立資料庫時，Code First 會將資料表新增至資料庫，以協助追蹤資料庫的結構描述是否與其產生的來源模型類別同步。</span><span class="sxs-lookup"><span data-stu-id="534e8-106">When you use EF Code First to automatically create a database, Code First adds a table to the database to help track whether the schema of the database is in sync with the model classes it was generated from.</span></span> <span data-ttu-id="534e8-107">如果未同步，EF 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="534e8-107">If they aren't in sync, EF throws an exception.</span></span> <span data-ttu-id="534e8-108">這可讓您更輕鬆地找出不一致的資料庫/程式碼問題。</span><span class="sxs-lookup"><span data-stu-id="534e8-108">This makes it easier to find inconsistent database/code issues.</span></span>

## <a name="adding-a-rating-property-to-the-movie-model"></a><span data-ttu-id="534e8-109">將 Rating 屬性新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="534e8-109">Adding a Rating Property to the Movie Model</span></span>

<span data-ttu-id="534e8-110">開啟 *Models/Movie.cs* 檔案，然後新增 `Rating` 屬性：</span><span class="sxs-lookup"><span data-stu-id="534e8-110">Open the *Models/Movie.cs* file and add a `Rating` property:</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/MovieDateRating.cs?highlight=11&range=7-18)]

<span data-ttu-id="534e8-111">建置應用程式 (Ctrl+Shift+B)。</span><span class="sxs-lookup"><span data-stu-id="534e8-111">Build the app (Ctrl+Shift+B).</span></span>

<span data-ttu-id="534e8-112">因為您已將欄位新增至 `Movie` 類別，所以也需要更新繫結白名單，以便包含這個新屬性。</span><span class="sxs-lookup"><span data-stu-id="534e8-112">Because you've added a new field to the `Movie` class, you also need to update the binding white list so this new property will be included.</span></span> <span data-ttu-id="534e8-113">在 *MoviesController.cs* 中，更新 `Create` 和 `Edit` 這兩個動作方法的 `[Bind]` 屬性 (attribute)，以包括 `Rating` 屬性 (property)：</span><span class="sxs-lookup"><span data-stu-id="534e8-113">In *MoviesController.cs*, update the `[Bind]` attribute for both the `Create` and `Edit` action methods to include the `Rating` property:</span></span>

```csharp
[Bind("ID,Title,ReleaseDate,Genre,Price,Rating")]
   ```

<span data-ttu-id="534e8-114">您也需要更新檢視範本，以便在瀏覽器檢視中顯示、建立和編輯新的 `Rating` 屬性。</span><span class="sxs-lookup"><span data-stu-id="534e8-114">You also need to update the view templates in order to display, create and edit the new `Rating` property in the browser view.</span></span>

<span data-ttu-id="534e8-115">編輯 */Views/Movies/Index.cshtml* 檔案，然後新增 `Rating` 欄位：</span><span class="sxs-lookup"><span data-stu-id="534e8-115">Edit the */Views/Movies/Index.cshtml* file and add a `Rating` field:</span></span>

[!code-HTML[Main](start-mvc/sample/MvcMovie/Views/Movies/IndexGenreRating.cshtml?highlight=17,39&range=24-64)]

<span data-ttu-id="534e8-116">使用 `Rating` 欄位更新 */Views/Movies/Create.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="534e8-116">Update the */Views/Movies/Create.cshtml* with a `Rating` field.</span></span> <span data-ttu-id="534e8-117">您可以複製/貼上前一個「表單群組」，讓 IntelliSense 協助您更新這些欄位。</span><span class="sxs-lookup"><span data-stu-id="534e8-117">You can copy/paste the previous "form group" and let intelliSense help you update the fields.</span></span> <span data-ttu-id="534e8-118">IntelliSense 會使用[標記協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="534e8-118">IntelliSense works with [Tag Helpers](xref:mvc/views/tag-helpers/intro).</span></span> <span data-ttu-id="534e8-119">注意：使用 RTM 版本的 Visual Studio 2017 時，您需要安裝適用於 Razor IntelliSense 的 [Razor 語言服務](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices)。</span><span class="sxs-lookup"><span data-stu-id="534e8-119">Note: In the RTM verison of Visual Studio 2017 you need to install the [Razor Language Services](https://marketplace.visualstudio.com/items?itemName=ms-madsk.RazorLanguageServices) for Razor intelliSense.</span></span> <span data-ttu-id="534e8-120">下一版會修正此問題。</span><span class="sxs-lookup"><span data-stu-id="534e8-120">This will be fixed in the next release.</span></span>

![在檢視的第二個 Label 項目中，開發人員已針對 asp-for 屬性值鍵入字母 R。](new-field/_static/cr.png)

<span data-ttu-id="534e8-124">在我們更新資料庫使其包含新欄位之後，應用程式才能運作。</span><span class="sxs-lookup"><span data-stu-id="534e8-124">The app won't work until we update the DB to include the new field.</span></span> <span data-ttu-id="534e8-125">如果您立即執行應用程式，則會收到下列 `SqlException`：</span><span class="sxs-lookup"><span data-stu-id="534e8-125">If you run it now, you'll get the following `SqlException`:</span></span>

`SqlException: Invalid column name 'Rating'.`

<span data-ttu-id="534e8-126">您看到此錯誤是因為更新的電影模型類別，不同於現有資料庫的電影資料表的結構描述</span><span class="sxs-lookup"><span data-stu-id="534e8-126">You're seeing this error because the updated Movie model class is different than the schema of the Movie table of the existing database.</span></span> <span data-ttu-id="534e8-127">(資料庫資料表中沒有任何 Rating 資料行)。</span><span class="sxs-lookup"><span data-stu-id="534e8-127">(There's no Rating column in the database table.)</span></span>

<span data-ttu-id="534e8-128">有幾個方法可以解決這個錯誤：</span><span class="sxs-lookup"><span data-stu-id="534e8-128">There are a few approaches to resolving the error:</span></span>

1. <span data-ttu-id="534e8-129">讓 Entity Framework 自動卸除資料庫，並重新依據新的模型類別結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="534e8-129">Have the Entity Framework automatically drop and re-create the database based on the new model class schema.</span></span> <span data-ttu-id="534e8-130">在開發週期早期，當您在測試資料庫上進行開發時，這個方法會很方便；其可讓您一併調整模型和資料庫結構描述，更加快速。</span><span class="sxs-lookup"><span data-stu-id="534e8-130">This approach is very convenient early in the development cycle when you are doing active development on a test database; it allows you to quickly evolve the model and database schema together.</span></span> <span data-ttu-id="534e8-131">不過，它的缺點是您會遺失資料庫中現有的資料 — 因此您不會想在實際執行的資料庫上使用這種方法！</span><span class="sxs-lookup"><span data-stu-id="534e8-131">The downside, though, is that you lose existing data in the database — so you don't want to use this approach on a production database!</span></span> <span data-ttu-id="534e8-132">使用初始設定式將測試資料自動植入資料庫，通常是開發應用程式的有效方式。</span><span class="sxs-lookup"><span data-stu-id="534e8-132">Using an initializer to automatically seed a database with test data is often a productive way to develop an application.</span></span>

2. <span data-ttu-id="534e8-133">您可明確修改現有資料庫的結構描述，使其符合模型類別。</span><span class="sxs-lookup"><span data-stu-id="534e8-133">Explicitly modify the schema of the existing database so that it matches the model classes.</span></span> <span data-ttu-id="534e8-134">這種方法的優點是可以保留您的資料。</span><span class="sxs-lookup"><span data-stu-id="534e8-134">The advantage of this approach is that you keep your data.</span></span> <span data-ttu-id="534e8-135">您可以手動方式或藉由建立資料庫變更指令碼來進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="534e8-135">You can make this change either manually or by creating a database change script.</span></span>

3. <span data-ttu-id="534e8-136">使用 Code First 移轉來更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="534e8-136">Use Code First Migrations to update the database schema.</span></span>

<span data-ttu-id="534e8-137">在本教學課程中，我們將使用 Code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="534e8-137">For this tutorial, we'll use Code First Migrations.</span></span>

<span data-ttu-id="534e8-138">更新 `SeedData` 類別，使其提供新資料行的值。</span><span class="sxs-lookup"><span data-stu-id="534e8-138">Update the `SeedData` class so that it provides a value for the new column.</span></span> <span data-ttu-id="534e8-139">範例變更如下所示，但您會想要為每個 `new Movie` 進行這項變更。</span><span class="sxs-lookup"><span data-stu-id="534e8-139">A sample change is shown below, but you'll want to make this change for each `new Movie`.</span></span>

[!code-csharp[Main](start-mvc/sample/MvcMovie/Models/SeedDataRating.cs?name=snippet1&highlight=6)]

<span data-ttu-id="534e8-140">建置方案。</span><span class="sxs-lookup"><span data-stu-id="534e8-140">Build the solution.</span></span>

<span data-ttu-id="534e8-141">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="534e8-141">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>

  ![PMC 功能表](adding-model/_static/pmc.png)

<span data-ttu-id="534e8-143">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="534e8-143">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration Rating
Update-Database
```

<span data-ttu-id="534e8-144">`Add-Migration` 命令會告知移轉架構，檢查目前的 `Movie` 模型與目前的 `Movie` 資料庫結構描述，並建立必要的程式碼以將資料庫移轉至新的模型。</span><span class="sxs-lookup"><span data-stu-id="534e8-144">The `Add-Migration` command tells the migration framework to examine the current `Movie` model with the current `Movie` DB schema and create the necessary code to migrate the DB to the new model.</span></span> <span data-ttu-id="534e8-145">"Rating" 是用來命名移轉檔案的任意名稱。</span><span class="sxs-lookup"><span data-stu-id="534e8-145">The name "Rating" is arbitrary and is used to name the migration file.</span></span> <span data-ttu-id="534e8-146">建議您針對移轉檔案使用有意義的名稱，更加實用。</span><span class="sxs-lookup"><span data-stu-id="534e8-146">It's helpful to use a meaningful name for the migration file.</span></span>

<span data-ttu-id="534e8-147">如果您刪除資料庫中的所有記錄，初始化作業會將內容植入資料庫，並包含 `Rating` 欄位。</span><span class="sxs-lookup"><span data-stu-id="534e8-147">If you delete all the records in the DB, the initialize will seed the DB and include the `Rating` field.</span></span> <span data-ttu-id="534e8-148">您可以使用瀏覽器或 SSOX 的刪除連結來執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="534e8-148">You can do this with the delete links in the browser or from SSOX.</span></span>

<span data-ttu-id="534e8-149">執行應用程式，並驗證您可以使用 `Rating` 欄位建立/編輯/顯示電影。</span><span class="sxs-lookup"><span data-stu-id="534e8-149">Run the app and verify you can create/edit/display movies with a `Rating` field.</span></span> <span data-ttu-id="534e8-150">您也應該將 `Rating` 欄位新增至 `Edit`、`Details` 和 `Delete` 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="534e8-150">You should also add the `Rating` field to the `Edit`, `Details`, and `Delete` view templates.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="534e8-151">[上一頁](search.md)
[下一頁](validation.md)</span><span class="sxs-lookup"><span data-stu-id="534e8-151">[Previous](search.md)
[Next](validation.md)</span></span>  
