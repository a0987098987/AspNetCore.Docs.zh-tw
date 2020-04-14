---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 讀取相關資料 - 6/8
author: rick-anderson
description: 在此教學課程中，您可以讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 0835dbf6b5434e4224d9f56ea3335df4b5d9e119
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277323"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="73fcd-103">ASP.NET Core 中的 Razor 頁面與 EF Core - 讀取相關資料 - 6/8</span><span class="sxs-lookup"><span data-stu-id="73fcd-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="73fcd-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="73fcd-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="73fcd-105">本教學課程說明如何讀取及顯示相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="73fcd-106">相關資料是 EF Core 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="73fcd-107">下圖顯示本教學課程的已完成頁面：</span><span class="sxs-lookup"><span data-stu-id="73fcd-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index30.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="73fcd-110">積極式、明確式和消極式載入</span><span class="sxs-lookup"><span data-stu-id="73fcd-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="73fcd-111">EF Core 有幾種方式可以將相關資料載入到實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="73fcd-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="73fcd-112">[渴望載入](/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="73fcd-113">積極式載入是指某一類型實體的查詢同時也會載入相關實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="73fcd-114">讀取實體時，將會擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="73fcd-115">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="73fcd-116">EF Core 將針對某些類型的積極式載入發出多個查詢。</span><span class="sxs-lookup"><span data-stu-id="73fcd-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="73fcd-117">發出多個查詢可能比大型單一查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="73fcd-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="73fcd-118">積極式載入是使用 `Include` 和 `ThenInclude` 方法加以指定。</span><span class="sxs-lookup"><span data-stu-id="73fcd-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="73fcd-120">當集合導覽包含在內時，積極式載入會傳送多個查詢：</span><span class="sxs-lookup"><span data-stu-id="73fcd-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="73fcd-121">針對主查詢傳送一個查詢</span><span class="sxs-lookup"><span data-stu-id="73fcd-121">One query for the main query</span></span> 
  * <span data-ttu-id="73fcd-122">針對負載樹狀結構中的每個集合「邊緣」傳送一個查詢。</span><span class="sxs-lookup"><span data-stu-id="73fcd-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="73fcd-123">使用 `Load` 的個別查詢：資料可以在個別查詢中擷取，而 EF Core 會「修正」導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="73fcd-124">「修正」表示 EF Core 會自動填入導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="73fcd-125">使用 `Load` 的個別查詢更像是明確式載入，而不是積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="73fcd-127">注意：EF Core 會將導覽屬性自動修正為先前已載入至內容執行個體的任何其他實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-127">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="73fcd-128">即使「未」\*\* 明確包含導覽屬性的資料，如果先前已載入某些或所有相關實體，仍然可能會填入該屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="73fcd-129">[明確載入](/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="73fcd-130">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="73fcd-131">必須撰寫程式碼，才能在需要時擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="73fcd-132">使用個別查詢的明確式載入會導致多個查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="73fcd-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="73fcd-133">透過明確式載入，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="73fcd-134">請使用 `Load` 方法來執行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="73fcd-135">例如：</span><span class="sxs-lookup"><span data-stu-id="73fcd-135">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="73fcd-137">[延遲載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="73fcd-138">[EF Core 已在 2.1 版中新增消極式載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="73fcd-139">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="73fcd-140">第一次存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="73fcd-141">每當第一次存取導覽屬性時，查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="73fcd-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="73fcd-142">建立 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-142">Create Course pages</span></span>

<span data-ttu-id="73fcd-143">`Course` 實體包含導覽屬性，其中包含相關的 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<span data-ttu-id="73fcd-145">若要顯示針對課程指派的部門名稱：</span><span class="sxs-lookup"><span data-stu-id="73fcd-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="73fcd-146">將相關的 `Department` 實體載入 `Course.Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="73fcd-147">從 `Department` 實體的 `Name` 屬性取得名稱。</span><span class="sxs-lookup"><span data-stu-id="73fcd-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="73fcd-148">Scaffold Course 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-148">Scaffold Course pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="73fcd-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73fcd-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="73fcd-150">遵循 [Scaffold Student 頁面](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，下列部分除外：</span><span class="sxs-lookup"><span data-stu-id="73fcd-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="73fcd-151">建立 *Pages/Courses* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="73fcd-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="73fcd-152">使用 `Course` 作為模型類別。</span><span class="sxs-lookup"><span data-stu-id="73fcd-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="73fcd-153">使用現有內容類別，而非建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="73fcd-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="73fcd-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73fcd-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="73fcd-155">建立 *Pages/Courses* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="73fcd-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="73fcd-156">執行下列命令來 scaffold Course 頁面。</span><span class="sxs-lookup"><span data-stu-id="73fcd-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="73fcd-157">**在 Windows 上:**</span><span class="sxs-lookup"><span data-stu-id="73fcd-157">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="73fcd-158">**Linux 或 macOS 上:**</span><span class="sxs-lookup"><span data-stu-id="73fcd-158">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="73fcd-159">開啟 *Pages/Courses/Index.cshtml.cs* 並檢查 `OnGetAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="73fcd-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="73fcd-160">Scaffolding 引擎已針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="73fcd-161">`Include` 方法可指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="73fcd-162">執行應用程式並選取**課程**連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="73fcd-163">部門資料行便會顯示沒有用的 `DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="73fcd-164">顯示部門名稱</span><span class="sxs-lookup"><span data-stu-id="73fcd-164">Display the department name</span></span>

<span data-ttu-id="73fcd-165">使用下列程式碼更新 Pages/Courses/Index.cshtml.cs：</span><span class="sxs-lookup"><span data-stu-id="73fcd-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="73fcd-166">上述程式碼會將 `Course` 屬性變更為 `Courses`，並新增 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="73fcd-167">`AsNoTracking` 可改善效能，因為不會追蹤傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="73fcd-168">無需追蹤實體的原因是它們不會在目前內容中更新。</span><span class="sxs-lookup"><span data-stu-id="73fcd-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="73fcd-169">使用下列程式碼更新 *Pages/Courses/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73fcd-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="73fcd-170">已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="73fcd-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="73fcd-171">已將 `Course` 屬性名稱變更為 `Courses`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="73fcd-172">新增顯示 `CourseID` 屬性值的 [編號]\*\*\*\* 資料行。</span><span class="sxs-lookup"><span data-stu-id="73fcd-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="73fcd-173">主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="73fcd-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="73fcd-174">不過，在此情況下，主索引鍵有意義。</span><span class="sxs-lookup"><span data-stu-id="73fcd-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="73fcd-175">變更 [部門]\*\*\*\* 資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="73fcd-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="73fcd-176">此程式碼會顯示已載入到 `Department` 導覽屬性之 `Department` 實體的 `Name` 屬性：</span><span class="sxs-lookup"><span data-stu-id="73fcd-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="73fcd-177">執行應用程式，並選取 [Courses]\*\*\*\* 索引標籤來查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="73fcd-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="73fcd-179">使用 Select 載入相關資料</span><span class="sxs-lookup"><span data-stu-id="73fcd-179">Loading related data with Select</span></span>

<span data-ttu-id="73fcd-180">`OnGetAsync` 方法使用 `Include` 方法載入相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="73fcd-181">`Select` 方法是一種替代方案，只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="73fcd-182">如果是單一項目 (例如 `Department.Name`)，它會使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="73fcd-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="73fcd-183">如果是集合，它會使用另一種資料庫存取，但集合上的 `Include` 運算子也是如此。</span><span class="sxs-lookup"><span data-stu-id="73fcd-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="73fcd-184">下列程式碼使用 `Select` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="73fcd-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="73fcd-185">前面的代碼不返回任何實體類型,因此不會執行跟蹤。</span><span class="sxs-lookup"><span data-stu-id="73fcd-185">The preceding code doesn't return any entity types, therefore no tracking is done.</span></span> <span data-ttu-id="73fcd-186">有關 EF 追蹤的詳細資訊,請參閱[追蹤與無追蹤查詢](/ef/core/querying/tracking)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-186">For more information about the EF tracking, see [Tracking vs. No-Tracking Queries](/ef/core/querying/tracking).</span></span>

<span data-ttu-id="73fcd-187">`CourseViewModel`：</span><span class="sxs-lookup"><span data-stu-id="73fcd-187">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="73fcd-188">如需完整範例，請參閱 [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-188">See [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="73fcd-189">建立 Instructor 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-189">Create Instructor pages</span></span>

<span data-ttu-id="73fcd-190">本節會 scaffold Instructor 頁面，並將相關的課程和註冊新增至 Instructors 索引頁面。</span><span class="sxs-lookup"><span data-stu-id="73fcd-190">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="73fcd-191">![教師索引頁面](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="73fcd-191">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="73fcd-192">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="73fcd-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="73fcd-193">講師清單會顯示來自 `OfficeAssignment` 實體 (上述映像中的 Office) 的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="73fcd-194">`Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="73fcd-195">積極式載入用於 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="73fcd-196">若需要顯示相關資料，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="73fcd-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="73fcd-197">在此情況下，將會顯示講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="73fcd-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="73fcd-198">當使用者選取講師時，將會顯示相關的 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-198">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="73fcd-199">`Instructor` 與 `Course` 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="73fcd-200">將會針對 `Course` 實體和其相關 `Department` 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-200">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="73fcd-201">在此情況下，個別查詢可能更有效率，因為只需要所選取講師的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="73fcd-202">這個範例示範如何使用導覽屬性中實體之導覽屬性的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="73fcd-203">當使用者選取課程時，將會顯示來自 `Enrollments` 實體的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-203">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="73fcd-204">在上述映像中，將會顯示學生姓名和年級。</span><span class="sxs-lookup"><span data-stu-id="73fcd-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="73fcd-205">`Course` 與 `Enrollment` 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="73fcd-206">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="73fcd-206">Create a view model</span></span>

<span data-ttu-id="73fcd-207">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="73fcd-208">需要檢視模型，其包含代表三個資料表的三個屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-208">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="73fcd-209">使用下列程式碼建立 *SchoolViewModels/InstructorIndexData.cs*：</span><span class="sxs-lookup"><span data-stu-id="73fcd-209">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="73fcd-210">Scaffold Instructor 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-210">Scaffold Instructor pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="73fcd-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73fcd-211">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="73fcd-212">遵循 [Scaffold Students 頁面](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，下列部分除外：</span><span class="sxs-lookup"><span data-stu-id="73fcd-212">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="73fcd-213">建立 *Pages/Instructors* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="73fcd-213">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="73fcd-214">使用 `Instructor` 作為模型類別。</span><span class="sxs-lookup"><span data-stu-id="73fcd-214">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="73fcd-215">使用現有內容類別，而非建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="73fcd-215">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="73fcd-216">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73fcd-216">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="73fcd-217">建立 *Pages/Instructors* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="73fcd-217">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="73fcd-218">執行下列命令來 Scaffold Instructor 頁面。</span><span class="sxs-lookup"><span data-stu-id="73fcd-218">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="73fcd-219">**在 Windows 上:**</span><span class="sxs-lookup"><span data-stu-id="73fcd-219">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="73fcd-220">**Linux 或 macOS 上:**</span><span class="sxs-lookup"><span data-stu-id="73fcd-220">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="73fcd-221">若要在更新之前查看 Scaffold 頁面的外觀，請執行應用程式並巡覽至 Instructors 頁面。</span><span class="sxs-lookup"><span data-stu-id="73fcd-221">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="73fcd-222">使用以下代碼更新*頁面/教師/Index.cshtml.cs:*</span><span class="sxs-lookup"><span data-stu-id="73fcd-222">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="73fcd-223">`OnGetAsync` 方法會針對所選取講師的識別碼接受選擇性的路由資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="73fcd-224">檢查 *Pages/Instructors/Index.cshtml.cs* 檔案中的查詢：</span><span class="sxs-lookup"><span data-stu-id="73fcd-224">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="73fcd-225">這個程式碼會針對下列導覽屬性指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="73fcd-225">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="73fcd-226">請注意 `CourseAssignments` 和 `Course` 之 `Include` 和 `ThenInclude` 方法的重複。</span><span class="sxs-lookup"><span data-stu-id="73fcd-226">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="73fcd-227">若要針對 `Course` 實體的兩個導覽屬性指定積極式載入，則重複是必要的。</span><span class="sxs-lookup"><span data-stu-id="73fcd-227">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="73fcd-228">下列程式碼會在已選取講師 (`id != null`) 時執行。</span><span class="sxs-lookup"><span data-stu-id="73fcd-228">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="73fcd-229">選取的講師會從檢視模型的講師清單中擷取。</span><span class="sxs-lookup"><span data-stu-id="73fcd-229">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="73fcd-230">檢視模型的 `Courses` 屬性則使用 `Course` 實體從該講師的 `CourseAssignments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-230">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="73fcd-231">`Where` 方法會傳回集合。</span><span class="sxs-lookup"><span data-stu-id="73fcd-231">The `Where` method returns a collection.</span></span> <span data-ttu-id="73fcd-232">但是，在此案例中，篩選會選取單一實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-232">But in this case, the filter will select a single entity.</span></span> <span data-ttu-id="73fcd-233">因此會呼叫 `Single` 方法將集合轉換成單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-233">so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="73fcd-234">`Instructor` 實體提供對 `CourseAssignments` 屬性的存取。</span><span class="sxs-lookup"><span data-stu-id="73fcd-234">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="73fcd-235">`CourseAssignments` 提供對相關 `Course` 實體的存取。</span><span class="sxs-lookup"><span data-stu-id="73fcd-235">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="73fcd-237">當集合只有一個項目時，將會在集合上使用 `Single` 方法。</span><span class="sxs-lookup"><span data-stu-id="73fcd-237">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="73fcd-238">如果集合是空的或是有多個項目，`Single` 方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="73fcd-238">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="73fcd-239">替代方式是 `SingleOrDefault`，它會在集合是空的時傳回預設值 (在此情況下為 Null)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-239">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="73fcd-240">選取課程時，下列程式碼會填入檢視模型的 `Enrollments` 屬性：</span><span class="sxs-lookup"><span data-stu-id="73fcd-240">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="73fcd-241">更新 Instructors [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-241">Update the instructors Index page</span></span>

<span data-ttu-id="73fcd-242">使用下列程式碼更新 *Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73fcd-242">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="73fcd-243">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="73fcd-243">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="73fcd-244">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-244">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="73fcd-245">`"{id:int?}"` 是路由範本。</span><span class="sxs-lookup"><span data-stu-id="73fcd-245">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="73fcd-246">路由範本將 URL 中的整數查詢字串變更為路由資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-246">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="73fcd-247">例如，只有在 `@page` 指示詞產生如下的 URL 時，按一下講師的 [選取]\*\*\*\* 連結：</span><span class="sxs-lookup"><span data-stu-id="73fcd-247">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="73fcd-248">頁面指示詞是 `@page "{id:int?}"` 時，URL 為：</span><span class="sxs-lookup"><span data-stu-id="73fcd-248">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="73fcd-249">新增 [辦公室]\*\*\*\* 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-249">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="73fcd-250">因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-250">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="73fcd-251">新增 [課程]\*\*\*\* 資料行，以顯示每位講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-251">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="73fcd-252">有關此剃刀語法的更多內容,請參閱[顯式行轉換](xref:mvc/views/razor#explicit-line-transition)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-252">See [Explicit line transition](xref:mvc/views/razor#explicit-line-transition) for more about this razor syntax.</span></span>

* <span data-ttu-id="73fcd-253">新增程式碼，將 `class="success"` 動態新增至所選講師和課程的 `tr` 項目。</span><span class="sxs-lookup"><span data-stu-id="73fcd-253">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="73fcd-254">這會使用啟動程序類別設定所選取資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="73fcd-254">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="73fcd-255">新增標示為**選取**的超連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-255">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="73fcd-256">此連結會將所選取的講師識別碼傳送至 `Index` 方法，並設定背景色彩。</span><span class="sxs-lookup"><span data-stu-id="73fcd-256">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="73fcd-257">新增所選講師的課程資料表。</span><span class="sxs-lookup"><span data-stu-id="73fcd-257">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="73fcd-258">新增所選課程的學生註冊資料表。</span><span class="sxs-lookup"><span data-stu-id="73fcd-258">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="73fcd-259">執行應用並選擇「**講師」** 選項卡。該頁顯示相關`Location``OfficeAssignment`實體的(辦公室)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-259">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="73fcd-260">如果 `OfficeAssignment` 為 Null，則會顯示空的資料表資料格。</span><span class="sxs-lookup"><span data-stu-id="73fcd-260">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="73fcd-261">按一下講師的 [選取]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-261">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="73fcd-262">資料列樣式會變更，並會顯示指派給該講師的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-262">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="73fcd-263">選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="73fcd-263">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="73fcd-265">使用 Single</span><span class="sxs-lookup"><span data-stu-id="73fcd-265">Using Single</span></span>

<span data-ttu-id="73fcd-266">`Single` 方法可以傳入 `Where` 條件，而不是個別呼叫 `Where` 方法：</span><span class="sxs-lookup"><span data-stu-id="73fcd-266">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="73fcd-267">使用 `Single` 搭配 Where 條件是個人喜好設定。</span><span class="sxs-lookup"><span data-stu-id="73fcd-267">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="73fcd-268">其不會對使用 `Where` 方法提供任何益處。</span><span class="sxs-lookup"><span data-stu-id="73fcd-268">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="73fcd-269">明確式載入</span><span class="sxs-lookup"><span data-stu-id="73fcd-269">Explicit loading</span></span>

<span data-ttu-id="73fcd-270">目前程式碼針對 `Enrollments` 和 `Students` 指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="73fcd-270">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="73fcd-271">假設使用者很少會想要查看課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="73fcd-271">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="73fcd-272">在此情況下，最佳方法就是在要求時，只載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-272">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="73fcd-273">在本節中，`OnGetAsync` 更新為針對 `Enrollments` 和 `Students` 使用明確式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-273">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="73fcd-274">使用下列程式碼更新 *Pages/Instructors/Index.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="73fcd-274">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="73fcd-275">上述程式碼會捨棄註冊和學生資料的 *ThenInclude* 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="73fcd-275">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="73fcd-276">如果選取了課程，則明確載入的程式碼會擷取：</span><span class="sxs-lookup"><span data-stu-id="73fcd-276">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="73fcd-277">所選取課程的 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-277">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="73fcd-278">每個 `Enrollment` 的 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-278">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="73fcd-279">請注意，上述程式碼會將 `.AsNoTracking()` 標記為註解。</span><span class="sxs-lookup"><span data-stu-id="73fcd-279">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="73fcd-280">針對所追蹤的實體，導覽屬性只能進行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-280">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="73fcd-281">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="73fcd-281">Test the app.</span></span> <span data-ttu-id="73fcd-282">就使用者的觀點而言，應用程式行為與先前版本相同。</span><span class="sxs-lookup"><span data-stu-id="73fcd-282">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="73fcd-283">後續步驟</span><span class="sxs-lookup"><span data-stu-id="73fcd-283">Next steps</span></span>

<span data-ttu-id="73fcd-284">下一個教學課程會示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-284">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="73fcd-285">[前面的教學](xref:data/ef-rp/complex-data-model)
>[下一個教學](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="73fcd-285">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="73fcd-286">在本教學課程中，將會讀取和顯示相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-286">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="73fcd-287">相關資料是 EF Core 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-287">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="73fcd-288">若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="73fcd-288">If you run into problems you can't solve, [download or view the completed app.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="73fcd-289">[下載標題](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-289">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="73fcd-290">下圖顯示本教學課程的已完成頁面：</span><span class="sxs-lookup"><span data-stu-id="73fcd-290">The following illustrations show the completed pages for this tutorial:</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="73fcd-293">相關資料的積極式、明確式和消極式載入</span><span class="sxs-lookup"><span data-stu-id="73fcd-293">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="73fcd-294">EF Core 有幾種方式可以將相關資料載入到實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="73fcd-294">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="73fcd-295">[渴望載入](/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-295">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="73fcd-296">積極式載入是指某一類型實體的查詢同時也會載入相關實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-296">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="73fcd-297">讀取實體時，將會擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-297">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="73fcd-298">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-298">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="73fcd-299">EF Core 將針對某些類型的積極式載入發出多個查詢。</span><span class="sxs-lookup"><span data-stu-id="73fcd-299">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="73fcd-300">相較於 EF6 中的某些查詢只有單一查詢的情況，發出多個查詢可能更有效率。</span><span class="sxs-lookup"><span data-stu-id="73fcd-300">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="73fcd-301">積極式載入是使用 `Include` 和 `ThenInclude` 方法加以指定。</span><span class="sxs-lookup"><span data-stu-id="73fcd-301">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="73fcd-303">當集合導覽包含在內時，積極式載入會傳送多個查詢：</span><span class="sxs-lookup"><span data-stu-id="73fcd-303">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="73fcd-304">針對主查詢傳送一個查詢</span><span class="sxs-lookup"><span data-stu-id="73fcd-304">One query for the main query</span></span> 
  * <span data-ttu-id="73fcd-305">針對負載樹狀結構中的每個集合「邊緣」傳送一個查詢。</span><span class="sxs-lookup"><span data-stu-id="73fcd-305">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="73fcd-306">使用 `Load` 的個別查詢：資料可以在個別查詢中擷取，而 EF Core 會「修正」導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-306">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="73fcd-307">「修正」表示 EF Core 會自動填入導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-307">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="73fcd-308">使用 `Load` 的個別查詢更像是明確式載入，而不是積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-308">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="73fcd-310">注意：EF Core 會將導覽屬性自動修正為先前已載入至內容執行個體的任何其他實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-310">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="73fcd-311">即使「未」\*\* 明確包含導覽屬性的資料，如果先前已載入某些或所有相關實體，仍然可能會填入該屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-311">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="73fcd-312">[明確載入](/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-312">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="73fcd-313">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-313">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="73fcd-314">必須撰寫程式碼，才能在需要時擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-314">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="73fcd-315">使用個別查詢的明確式載入會導致多個查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="73fcd-315">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="73fcd-316">透過明確式載入，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-316">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="73fcd-317">請使用 `Load` 方法來執行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-317">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="73fcd-318">例如：</span><span class="sxs-lookup"><span data-stu-id="73fcd-318">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="73fcd-320">[延遲載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-320">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="73fcd-321">[EF Core 已在 2.1 版中新增消極式載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-321">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="73fcd-322">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-322">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="73fcd-323">第一次存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-323">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="73fcd-324">每當第一次存取導覽屬性時，查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="73fcd-324">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="73fcd-325">`Select` 運算子只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-325">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="73fcd-326">建立顯示部門名稱的 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-326">Create a Course page that displays department name</span></span>

<span data-ttu-id="73fcd-327">Course 實體包含導覽屬性，其中包含 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-327">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="73fcd-328">`Department` 實體包含已指派課程的部門。</span><span class="sxs-lookup"><span data-stu-id="73fcd-328">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="73fcd-329">若要在課程清單中顯示所指派部門的名稱：</span><span class="sxs-lookup"><span data-stu-id="73fcd-329">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="73fcd-330">從 `Department` 實體取得 `Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-330">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="73fcd-331">`Department` 實體來自 `Course.Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-331">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="73fcd-333">Scaffold Course 模型</span><span class="sxs-lookup"><span data-stu-id="73fcd-333">Scaffold the Course model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="73fcd-334">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73fcd-334">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="73fcd-335">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Course`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-335">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="73fcd-336">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73fcd-336">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="73fcd-337">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="73fcd-337">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="73fcd-338">上述命令會 Scaffold `Course` 模型。</span><span class="sxs-lookup"><span data-stu-id="73fcd-338">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="73fcd-339">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="73fcd-339">Open the project in Visual Studio.</span></span>

<span data-ttu-id="73fcd-340">開啟 *Pages/Courses/Index.cshtml.cs* 並檢查 `OnGetAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="73fcd-340">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="73fcd-341">Scaffolding 引擎已針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-341">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="73fcd-342">`Include` 方法可指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-342">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="73fcd-343">執行應用程式並選取**課程**連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-343">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="73fcd-344">部門資料行便會顯示沒有用的 `DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-344">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="73fcd-345">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="73fcd-345">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="73fcd-346">上述程式碼會新增 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-346">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="73fcd-347">`AsNoTracking` 可改善效能，因為不會追蹤傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-347">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="73fcd-348">不會追蹤實體的原因是它們不會在目前的內容中更新。</span><span class="sxs-lookup"><span data-stu-id="73fcd-348">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="73fcd-349">以下列醒目提示的標記更新 *Pages/Courses/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="73fcd-349">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="73fcd-350">已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="73fcd-350">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="73fcd-351">已將標題從「索引」) 變更為「課程」。</span><span class="sxs-lookup"><span data-stu-id="73fcd-351">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="73fcd-352">新增顯示 `CourseID` 屬性值的 [編號]\*\*\*\* 資料行。</span><span class="sxs-lookup"><span data-stu-id="73fcd-352">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="73fcd-353">主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="73fcd-353">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="73fcd-354">不過，在此情況下，主索引鍵有意義。</span><span class="sxs-lookup"><span data-stu-id="73fcd-354">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="73fcd-355">變更 [部門]\*\*\*\* 資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="73fcd-355">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="73fcd-356">此程式碼會顯示已載入到 `Department` 導覽屬性之 `Department` 實體的 `Name` 屬性：</span><span class="sxs-lookup"><span data-stu-id="73fcd-356">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="73fcd-357">執行應用程式，並選取 [Courses]\*\*\*\* 索引標籤來查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="73fcd-357">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="73fcd-359">使用 Select 載入相關資料</span><span class="sxs-lookup"><span data-stu-id="73fcd-359">Loading related data with Select</span></span>

<span data-ttu-id="73fcd-360">`OnGetAsync` 方法使用 `Include` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="73fcd-360">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="73fcd-361">`Select` 運算子只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-361">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="73fcd-362">如果是單一項目 (例如 `Department.Name`)，它會使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="73fcd-362">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="73fcd-363">如果是集合，它會使用另一種資料庫存取，但集合上的 `Include` 運算子也是如此。</span><span class="sxs-lookup"><span data-stu-id="73fcd-363">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="73fcd-364">下列程式碼使用 `Select` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="73fcd-364">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="73fcd-365">`CourseViewModel`：</span><span class="sxs-lookup"><span data-stu-id="73fcd-365">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="73fcd-366">如需完整範例，請參閱 [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-366">See [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="73fcd-367">建立顯示課程和註冊的 Instructors 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-367">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="73fcd-368">在本節中，將會建立 Instructors 頁面。</span><span class="sxs-lookup"><span data-stu-id="73fcd-368">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="73fcd-369">![教師索引頁面](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="73fcd-369">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="73fcd-370">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="73fcd-370">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="73fcd-371">講師清單會顯示來自 `OfficeAssignment` 實體 (上述映像中的 Office) 的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-371">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="73fcd-372">`Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-372">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="73fcd-373">積極式載入用於 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-373">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="73fcd-374">若需要顯示相關資料，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="73fcd-374">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="73fcd-375">在此情況下，將會顯示講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="73fcd-375">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="73fcd-376">當使用者選取講師 (上述映像中的 Harui) 時，便會顯示相關的 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-376">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="73fcd-377">`Instructor` 與 `Course` 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-377">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="73fcd-378">將會針對 `Course` 實體和其相關 `Department` 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-378">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="73fcd-379">在此情況下，個別查詢可能更有效率，因為只需要所選取講師的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-379">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="73fcd-380">這個範例示範如何使用導覽屬性中實體之導覽屬性的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-380">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="73fcd-381">當使用者選取課程 (上述映像中的 Chemistry)，隨即顯示 `Enrollments` 中的相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-381">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="73fcd-382">在上述映像中，將會顯示學生姓名和年級。</span><span class="sxs-lookup"><span data-stu-id="73fcd-382">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="73fcd-383">`Course` 與 `Enrollment` 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="73fcd-383">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="73fcd-384">建立 Instructor [索引] 檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="73fcd-384">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="73fcd-385">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-385">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="73fcd-386">建立的檢視模型會包含三個實體代表三個資料表。</span><span class="sxs-lookup"><span data-stu-id="73fcd-386">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="73fcd-387">在 *SchoolViewModels* 資料夾中，以下列程式碼建立 *InstructorIndexData.cs*：</span><span class="sxs-lookup"><span data-stu-id="73fcd-387">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="73fcd-388">Scaffold Instructor 模型</span><span class="sxs-lookup"><span data-stu-id="73fcd-388">Scaffold the Instructor model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="73fcd-389">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="73fcd-389">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="73fcd-390">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Instructor`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-390">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="73fcd-391">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="73fcd-391">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="73fcd-392">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="73fcd-392">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="73fcd-393">上述命令會 Scaffold `Instructor` 模型。</span><span class="sxs-lookup"><span data-stu-id="73fcd-393">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="73fcd-394">執行應用程式並巡覽至講師頁面。</span><span class="sxs-lookup"><span data-stu-id="73fcd-394">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="73fcd-395">以下列程式碼取代 *Pages/Instructors/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="73fcd-395">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="73fcd-396">`OnGetAsync` 方法會針對所選取講師的識別碼接受選擇性的路由資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-396">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="73fcd-397">檢查 *Pages/Instructors/Index.cshtml.cs* 檔案中的查詢：</span><span class="sxs-lookup"><span data-stu-id="73fcd-397">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="73fcd-398">查詢具有兩個 Include：</span><span class="sxs-lookup"><span data-stu-id="73fcd-398">The query has two includes:</span></span>

* <span data-ttu-id="73fcd-399">`OfficeAssignment`：顯示在 [Instructors 檢視](#IP)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-399">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="73fcd-400">`CourseAssignments`：它顯示所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-400">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="73fcd-401">更新 Instructors [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="73fcd-401">Update the instructors Index page</span></span>

<span data-ttu-id="73fcd-402">以下列標記更新 *Pages/Instructors/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="73fcd-402">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="73fcd-403">上述標記會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="73fcd-403">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="73fcd-404">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-404">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="73fcd-405">`"{id:int?}"` 是路由範本。</span><span class="sxs-lookup"><span data-stu-id="73fcd-405">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="73fcd-406">路由範本將 URL 中的整數查詢字串變更為路由資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-406">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="73fcd-407">例如，只有在 `@page` 指示詞產生如下的 URL 時，按一下講師的 [選取]\*\*\*\* 連結：</span><span class="sxs-lookup"><span data-stu-id="73fcd-407">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="73fcd-408">頁面指示詞是 `@page "{id:int?}"` 時，先前的 URL 為：</span><span class="sxs-lookup"><span data-stu-id="73fcd-408">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="73fcd-409">頁面標題是 **Instructors**。</span><span class="sxs-lookup"><span data-stu-id="73fcd-409">Page title is **Instructors**.</span></span>
* <span data-ttu-id="73fcd-410">新增 [辦公室]\*\*\*\* 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。</span><span class="sxs-lookup"><span data-stu-id="73fcd-410">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="73fcd-411">因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-411">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="73fcd-412">新增 [課程]\*\*\*\* 資料行，以顯示每位講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-412">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="73fcd-413">有關此剃刀語法的更多內容,請參閱[顯式行轉換](xref:mvc/views/razor#explicit-line-transition)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-413">See [Explicit line transition](xref:mvc/views/razor#explicit-line-transition) for more about this razor syntax.</span></span>

* <span data-ttu-id="73fcd-414">新增程式碼，將 `class="success"` 動態新增至所選取講師的 `tr` 項目。</span><span class="sxs-lookup"><span data-stu-id="73fcd-414">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="73fcd-415">這會使用啟動程序類別設定所選取資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="73fcd-415">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="73fcd-416">新增標示為**選取**的超連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-416">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="73fcd-417">此連結會將所選取的講師識別碼傳送至 `Index` 方法，並設定背景色彩。</span><span class="sxs-lookup"><span data-stu-id="73fcd-417">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="73fcd-418">執行應用並選擇「**講師」** 選項卡。該頁顯示相關`Location``OfficeAssignment`實體的(辦公室)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-418">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="73fcd-419">如果 OfficeAssignment 是 Null，就會顯示空的資料表資料格。</span><span class="sxs-lookup"><span data-stu-id="73fcd-419">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="73fcd-420">按一下**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-420">Click on the **Select** link.</span></span> <span data-ttu-id="73fcd-421">資料列樣式變更。</span><span class="sxs-lookup"><span data-stu-id="73fcd-421">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="73fcd-422">新增選取的講師所教授的課程</span><span class="sxs-lookup"><span data-stu-id="73fcd-422">Add courses taught by selected instructor</span></span>

<span data-ttu-id="73fcd-423">以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="73fcd-423">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="73fcd-424">新增 `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="73fcd-424">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="73fcd-425">檢查已更新的查詢：</span><span class="sxs-lookup"><span data-stu-id="73fcd-425">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="73fcd-426">上述查詢會新增 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-426">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="73fcd-427">下列程式碼會在已選取講師 (`id != null`) 時執行。</span><span class="sxs-lookup"><span data-stu-id="73fcd-427">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="73fcd-428">選取的講師會從檢視模型的講師清單中擷取。</span><span class="sxs-lookup"><span data-stu-id="73fcd-428">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="73fcd-429">檢視模型的 `Courses` 屬性則使用 `Course` 實體從該講師的 `CourseAssignments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-429">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="73fcd-430">`Where` 方法會傳回集合。</span><span class="sxs-lookup"><span data-stu-id="73fcd-430">The `Where` method returns a collection.</span></span> <span data-ttu-id="73fcd-431">在上述 `Where` 方法中，只會傳回一個單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-431">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="73fcd-432">`Single` 方法會將集合轉換成單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-432">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="73fcd-433">`Instructor` 實體提供對 `CourseAssignments` 屬性的存取。</span><span class="sxs-lookup"><span data-stu-id="73fcd-433">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="73fcd-434">`CourseAssignments` 提供對相關 `Course` 實體的存取。</span><span class="sxs-lookup"><span data-stu-id="73fcd-434">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="73fcd-436">當集合只有一個項目時，將會在集合上使用 `Single` 方法。</span><span class="sxs-lookup"><span data-stu-id="73fcd-436">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="73fcd-437">如果集合是空的或是有多個項目，`Single` 方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="73fcd-437">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="73fcd-438">替代方式是 `SingleOrDefault`，它會在集合是空的時傳回預設值 (在此情況下為 Null)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-438">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="73fcd-439">在空集合上使用 `SingleOrDefault`：</span><span class="sxs-lookup"><span data-stu-id="73fcd-439">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="73fcd-440">造成例外狀況 (由於嘗試在 Null 參考上尋找 `Courses` 屬性)。</span><span class="sxs-lookup"><span data-stu-id="73fcd-440">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="73fcd-441">例外狀況訊息會不太清楚地指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="73fcd-441">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="73fcd-442">選取課程時，下列程式碼會填入檢視模型的 `Enrollments` 屬性：</span><span class="sxs-lookup"><span data-stu-id="73fcd-442">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="73fcd-443">將下列標記新增至 *Pages/Instructors/Index.cshtml* Razor 頁面的結尾：</span><span class="sxs-lookup"><span data-stu-id="73fcd-443">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="73fcd-444">當選取講師時，上述標記會顯示與講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="73fcd-444">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="73fcd-445">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="73fcd-445">Test the app.</span></span> <span data-ttu-id="73fcd-446">按一下講師頁面上的**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="73fcd-446">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="73fcd-447">顯示學生資料</span><span class="sxs-lookup"><span data-stu-id="73fcd-447">Show student data</span></span>

<span data-ttu-id="73fcd-448">在本節中，應用程式會更新以顯示所選取課程的學生資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-448">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="73fcd-449">以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中 `OnGetAsync` 方法的查詢：</span><span class="sxs-lookup"><span data-stu-id="73fcd-449">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="73fcd-450">更新 *Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="73fcd-450">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="73fcd-451">將下列標記新增至檔案結尾：</span><span class="sxs-lookup"><span data-stu-id="73fcd-451">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="73fcd-452">上述標記會顯示註冊所選取課程的學生清單。</span><span class="sxs-lookup"><span data-stu-id="73fcd-452">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="73fcd-453">重新整理頁面，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="73fcd-453">Refresh the page and select an instructor.</span></span> <span data-ttu-id="73fcd-454">選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="73fcd-454">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="73fcd-456">使用 Single</span><span class="sxs-lookup"><span data-stu-id="73fcd-456">Using Single</span></span>

<span data-ttu-id="73fcd-457">`Single` 方法可以傳入 `Where` 條件，而不是個別呼叫 `Where` 方法：</span><span class="sxs-lookup"><span data-stu-id="73fcd-457">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="73fcd-458">比起使用 `Where`，上述 `Single` 方法並沒有任何優勢。</span><span class="sxs-lookup"><span data-stu-id="73fcd-458">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="73fcd-459">某些開發人員偏好使用 `Single` 方法樣式。</span><span class="sxs-lookup"><span data-stu-id="73fcd-459">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="73fcd-460">明確式載入</span><span class="sxs-lookup"><span data-stu-id="73fcd-460">Explicit loading</span></span>

<span data-ttu-id="73fcd-461">目前程式碼針對 `Enrollments` 和 `Students` 指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="73fcd-461">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="73fcd-462">假設使用者很少會想要查看課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="73fcd-462">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="73fcd-463">在此情況下，最佳方法就是在要求時，只載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-463">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="73fcd-464">在本節中，`OnGetAsync` 更新為針對 `Enrollments` 和 `Students` 使用明確式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-464">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="73fcd-465">使用下列程式碼更新 `OnGetAsync`：</span><span class="sxs-lookup"><span data-stu-id="73fcd-465">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="73fcd-466">上述程式碼會捨棄註冊和學生資料的 *ThenInclude* 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="73fcd-466">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="73fcd-467">如果選取了課程，醒目提示的程式碼就會擷取：</span><span class="sxs-lookup"><span data-stu-id="73fcd-467">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="73fcd-468">所選取課程的 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-468">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="73fcd-469">每個 `Enrollment` 的 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="73fcd-469">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="73fcd-470">請注意，上述程式碼會將 `.AsNoTracking()` 註解化。</span><span class="sxs-lookup"><span data-stu-id="73fcd-470">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="73fcd-471">針對所追蹤的實體，導覽屬性只能進行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="73fcd-471">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="73fcd-472">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="73fcd-472">Test the app.</span></span> <span data-ttu-id="73fcd-473">就使用者的觀點而言，應用程式行為與之前的版本相同。</span><span class="sxs-lookup"><span data-stu-id="73fcd-473">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="73fcd-474">下一個教學課程會示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="73fcd-474">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="73fcd-475">其他資源</span><span class="sxs-lookup"><span data-stu-id="73fcd-475">Additional resources</span></span>

* [<span data-ttu-id="73fcd-476">這個教學課程的 YouTube 版本 (第 1 部分)</span><span class="sxs-lookup"><span data-stu-id="73fcd-476">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="73fcd-477">這個教學課程的 YouTube 版本 (第 2 部分)</span><span class="sxs-lookup"><span data-stu-id="73fcd-477">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="73fcd-478">[前一個](xref:data/ef-rp/complex-data-model)
>[下一個](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="73fcd-478">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
