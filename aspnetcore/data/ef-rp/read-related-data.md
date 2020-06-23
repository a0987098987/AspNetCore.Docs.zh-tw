---
title: 第6部分， Razor ASP.NET Core 讀取相關資料中有 EF Core 的頁面
author: rick-anderson
description: 頁面的第6部分 Razor 和 Entity Framework 教學課程系列。
ms.author: riande
ms.custom: mvc
ms.date: 09/28/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: data/ef-rp/read-related-data
ms.openlocfilehash: 5ffb57f21c89e21fcbb14b933cb0d0cb29d9c71b
ms.sourcegitcommit: dd2a1542a4a377123490034153368c135fdbd09e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/23/2020
ms.locfileid: "85240947"
---
# <a name="part-6-razor-pages-with-ef-core-in-aspnet-core---read-related-data"></a><span data-ttu-id="6687d-103">第6部分， Razor ASP.NET Core 讀取相關資料中有 EF Core 的頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-103">Part 6, Razor Pages with EF Core in ASP.NET Core - Read Related Data</span></span>

<span data-ttu-id="6687d-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog)、[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6687d-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6687d-105">本教學課程說明如何讀取及顯示相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-105">This tutorial shows how to read and display related data.</span></span> <span data-ttu-id="6687d-106">相關資料是 EF Core 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="6687d-107">下圖顯示本教學課程的已完成頁面：</span><span class="sxs-lookup"><span data-stu-id="6687d-107">The following illustrations show the completed pages for this tutorial:</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index30.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index30.png)

## <a name="eager-explicit-and-lazy-loading"></a><span data-ttu-id="6687d-110">積極式、明確式和消極式載入</span><span class="sxs-lookup"><span data-stu-id="6687d-110">Eager, explicit, and lazy loading</span></span>

<span data-ttu-id="6687d-111">EF Core 有幾種方式可以將相關資料載入到實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="6687d-111">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="6687d-112">[積極式載入](/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-112">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="6687d-113">積極式載入是指某一類型實體的查詢同時也會載入相關實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-113">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="6687d-114">讀取實體時，將會擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-114">When an entity is read, its related data is retrieved.</span></span> <span data-ttu-id="6687d-115">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-115">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="6687d-116">EF Core 將針對某些類型的積極式載入發出多個查詢。</span><span class="sxs-lookup"><span data-stu-id="6687d-116">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="6687d-117">發出多個查詢可能比大型單一查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="6687d-117">Issuing multiple queries can be more efficient than a giant single query.</span></span> <span data-ttu-id="6687d-118">積極式載入是使用 `Include` 和 `ThenInclude` 方法加以指定。</span><span class="sxs-lookup"><span data-stu-id="6687d-118">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="6687d-120">當集合導覽包含在內時，積極式載入會傳送多個查詢：</span><span class="sxs-lookup"><span data-stu-id="6687d-120">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="6687d-121">針對主查詢傳送一個查詢</span><span class="sxs-lookup"><span data-stu-id="6687d-121">One query for the main query</span></span> 
  * <span data-ttu-id="6687d-122">針對負載樹狀結構中的每個集合「邊緣」傳送一個查詢。</span><span class="sxs-lookup"><span data-stu-id="6687d-122">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="6687d-123">使用 `Load` 的個別查詢：資料可以在個別查詢中擷取，而 EF Core 會「修正」導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-123">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="6687d-124">「修正」表示 EF Core 會自動填入導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-124">"Fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="6687d-125">使用 `Load` 的個別查詢更像是明確式載入，而不是積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-125">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="6687d-127">**注意：** EF Core 會將導覽屬性自動修正為先前已載入至內容實例的任何其他實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-127">**Note:** EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="6687d-128">即使「未」\*\* 明確包含導覽屬性的資料，如果先前已載入某些或所有相關實體，仍然可能會填入該屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-128">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="6687d-129">[明確載入](/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-129">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="6687d-130">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-130">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="6687d-131">必須撰寫程式碼，才能在需要時擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-131">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="6687d-132">使用個別查詢的明確式載入會導致多個查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="6687d-132">Explicit loading with separate queries results in multiple queries sent to the database.</span></span> <span data-ttu-id="6687d-133">透過明確式載入，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-133">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="6687d-134">請使用 `Load` 方法來執行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-134">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="6687d-135">例如：</span><span class="sxs-lookup"><span data-stu-id="6687d-135">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="6687d-137">消極式[載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-137">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="6687d-138">[EF Core 已在 2.1 版中新增消極式載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-138">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="6687d-139">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-139">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="6687d-140">第一次存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-140">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="6687d-141">每當第一次存取導覽屬性時，查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="6687d-141">A query is sent to the database each time a navigation property is accessed for the first time.</span></span>

## <a name="create-course-pages"></a><span data-ttu-id="6687d-142">建立 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-142">Create Course pages</span></span>

<span data-ttu-id="6687d-143">`Course` 實體包含導覽屬性，其中包含相關的 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-143">The `Course` entity includes a navigation property that contains the related `Department` entity.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<span data-ttu-id="6687d-145">若要顯示針對課程指派的部門名稱：</span><span class="sxs-lookup"><span data-stu-id="6687d-145">To display the name of the assigned department for a course:</span></span>

* <span data-ttu-id="6687d-146">將相關的 `Department` 實體載入 `Course.Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-146">Load the related `Department` entity into the `Course.Department` navigation property.</span></span>
* <span data-ttu-id="6687d-147">從 `Department` 實體的 `Name` 屬性取得名稱。</span><span class="sxs-lookup"><span data-stu-id="6687d-147">Get the name from the `Department` entity's `Name` property.</span></span>

<a name="scaffold"></a>

### <a name="scaffold-course-pages"></a><span data-ttu-id="6687d-148">Scaffold Course 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-148">Scaffold Course pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6687d-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6687d-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6687d-150">遵循 [Scaffold Student 頁面](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，下列部分除外：</span><span class="sxs-lookup"><span data-stu-id="6687d-150">Follow the instructions in [Scaffold Student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="6687d-151">建立 *Pages/Courses* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6687d-151">Create a *Pages/Courses* folder.</span></span>
  * <span data-ttu-id="6687d-152">使用 `Course` 作為模型類別。</span><span class="sxs-lookup"><span data-stu-id="6687d-152">Use `Course` for the model class.</span></span>
  * <span data-ttu-id="6687d-153">使用現有內容類別，而非建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="6687d-153">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6687d-154">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6687d-154">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6687d-155">建立 *Pages/Courses* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6687d-155">Create a *Pages/Courses* folder.</span></span>

* <span data-ttu-id="6687d-156">執行下列命令來 scaffold Course 頁面。</span><span class="sxs-lookup"><span data-stu-id="6687d-156">Run the following command to scaffold the Course pages.</span></span>

  <span data-ttu-id="6687d-157">**在 Windows 上：**</span><span class="sxs-lookup"><span data-stu-id="6687d-157">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

  <span data-ttu-id="6687d-158">**在 Linux 或 macOS 上：**</span><span class="sxs-lookup"><span data-stu-id="6687d-158">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages/Courses --referenceScriptLibraries
  ```

---

* <span data-ttu-id="6687d-159">開啟 *Pages/Courses/Index.cshtml.cs* 並檢查 `OnGetAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="6687d-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="6687d-160">Scaffolding 引擎已針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="6687d-161">`Include` 方法可指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-161">The `Include` method specifies eager loading.</span></span>

* <span data-ttu-id="6687d-162">執行應用程式並選取**課程**連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="6687d-163">部門資料行便會顯示沒有用的 `DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="6687d-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

### <a name="display-the-department-name"></a><span data-ttu-id="6687d-164">顯示部門名稱</span><span class="sxs-lookup"><span data-stu-id="6687d-164">Display the department name</span></span>

<span data-ttu-id="6687d-165">使用下列程式碼更新 Pages/Courses/Index.cshtml.cs：</span><span class="sxs-lookup"><span data-stu-id="6687d-165">Update Pages/Courses/Index.cshtml.cs with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Courses/Index.cshtml.cs?highlight=18,22,24)]

<span data-ttu-id="6687d-166">上述程式碼會將 `Course` 屬性變更為 `Courses`，並新增 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="6687d-166">The preceding code changes the `Course` property to `Courses` and adds `AsNoTracking`.</span></span> <span data-ttu-id="6687d-167">`AsNoTracking` 可改善效能，因為不會追蹤傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-167">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="6687d-168">無需追蹤實體的原因是它們不會在目前內容中更新。</span><span class="sxs-lookup"><span data-stu-id="6687d-168">The entities don't need to be tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="6687d-169">使用下列程式碼更新 *Pages/Courses/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="6687d-169">Update *Pages/Courses/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Courses/Index.cshtml?highlight=5,8,16-18,20,23,26,32,35-37,45)]

<span data-ttu-id="6687d-170">已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="6687d-170">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="6687d-171">已將 `Course` 屬性名稱變更為 `Courses`。</span><span class="sxs-lookup"><span data-stu-id="6687d-171">Changed the `Course` property name to `Courses`.</span></span>
* <span data-ttu-id="6687d-172">新增顯示 `CourseID` 屬性值的 [編號]\*\*\*\* 資料行。</span><span class="sxs-lookup"><span data-stu-id="6687d-172">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="6687d-173">主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="6687d-173">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="6687d-174">不過，在此情況下，主索引鍵有意義。</span><span class="sxs-lookup"><span data-stu-id="6687d-174">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="6687d-175">變更 [部門]\*\*\*\* 資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="6687d-175">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="6687d-176">此程式碼會顯示已載入到 `Department` 導覽屬性之 `Department` 實體的 `Name` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6687d-176">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="6687d-177">執行應用程式，並選取 [Courses]\*\*\*\* 索引標籤來查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="6687d-177">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index30.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="6687d-179">使用 Select 載入相關資料</span><span class="sxs-lookup"><span data-stu-id="6687d-179">Loading related data with Select</span></span>

<span data-ttu-id="6687d-180">`OnGetAsync` 方法使用 `Include` 方法載入相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-180">The `OnGetAsync` method loads related data with the `Include` method.</span></span> <span data-ttu-id="6687d-181">`Select` 方法是一種替代方案，只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-181">The `Select` method is an alternative that loads only the related data needed.</span></span> <span data-ttu-id="6687d-182">如果是單一項目 (例如 `Department.Name`)，它會使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="6687d-182">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="6687d-183">如果是集合，它會使用另一種資料庫存取，但集合上的 `Include` 運算子也是如此。</span><span class="sxs-lookup"><span data-stu-id="6687d-183">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="6687d-184">下列程式碼使用 `Select` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="6687d-184">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=6)]

<span data-ttu-id="6687d-185">上述程式碼不會傳回任何實體類型，因此不會進行任何追蹤。</span><span class="sxs-lookup"><span data-stu-id="6687d-185">The preceding code doesn't return any entity types, therefore no tracking is done.</span></span> <span data-ttu-id="6687d-186">如需 EF 追蹤的詳細資訊，請參閱[追蹤與不追蹤查詢](/ef/core/querying/tracking)。</span><span class="sxs-lookup"><span data-stu-id="6687d-186">For more information about the EF tracking, see [Tracking vs. No-Tracking Queries](/ef/core/querying/tracking).</span></span>

<span data-ttu-id="6687d-187">`CourseViewModel`：</span><span class="sxs-lookup"><span data-stu-id="6687d-187">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="6687d-188">如需完整範例，請參閱 [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs)。</span><span class="sxs-lookup"><span data-stu-id="6687d-188">See [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu30snapshots/6-related/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-instructor-pages"></a><span data-ttu-id="6687d-189">建立 Instructor 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-189">Create Instructor pages</span></span>

<span data-ttu-id="6687d-190">本節會 scaffold Instructor 頁面，並將相關的課程和註冊新增至 Instructors 索引頁面。</span><span class="sxs-lookup"><span data-stu-id="6687d-190">This section scaffolds Instructor pages and adds related Courses and Enrollments to the Instructors Index page.</span></span>

<a name="IP"></a>
<span data-ttu-id="6687d-191">![講師索引頁面](read-related-data/_static/instructors-index30.png)</span><span class="sxs-lookup"><span data-stu-id="6687d-191">![Instructors Index page](read-related-data/_static/instructors-index30.png)</span></span>

<span data-ttu-id="6687d-192">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="6687d-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="6687d-193">講師清單會顯示來自 `OfficeAssignment` 實體 (上述映像中的 Office) 的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="6687d-194">`Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="6687d-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="6687d-195">積極式載入用於 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="6687d-196">若需要顯示相關資料，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="6687d-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="6687d-197">在此情況下，將會顯示講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="6687d-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="6687d-198">當使用者選取講師時，將會顯示相關的 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-198">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="6687d-199">`Instructor` 與 `Course` 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="6687d-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="6687d-200">將會針對 `Course` 實體和其相關 `Department` 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-200">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="6687d-201">在此情況下，個別查詢可能更有效率，因為只需要所選取講師的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="6687d-202">這個範例示範如何使用導覽屬性中實體之導覽屬性的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="6687d-203">當使用者選取課程時，將會顯示來自 `Enrollments` 實體的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-203">When the user selects a course, related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="6687d-204">在上述映像中，將會顯示學生姓名和年級。</span><span class="sxs-lookup"><span data-stu-id="6687d-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="6687d-205">`Course` 與 `Enrollment` 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="6687d-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model"></a><span data-ttu-id="6687d-206">建立檢視模型</span><span class="sxs-lookup"><span data-stu-id="6687d-206">Create a view model</span></span>

<span data-ttu-id="6687d-207">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="6687d-208">需要檢視模型，其包含代表三個資料表的三個屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-208">A view model is needed that includes three properties representing the three tables.</span></span>

<span data-ttu-id="6687d-209">使用下列程式碼建立 *SchoolViewModels/InstructorIndexData.cs*：</span><span class="sxs-lookup"><span data-stu-id="6687d-209">Create *SchoolViewModels/InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-instructor-pages"></a><span data-ttu-id="6687d-210">Scaffold Instructor 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-210">Scaffold Instructor pages</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6687d-211">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6687d-211">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="6687d-212">遵循 [Scaffold Students 頁面](xref:data/ef-rp/intro#scaffold-student-pages)中的指示，下列部分除外：</span><span class="sxs-lookup"><span data-stu-id="6687d-212">Follow the instructions in [Scaffold the student pages](xref:data/ef-rp/intro#scaffold-student-pages) with the following exceptions:</span></span>

  * <span data-ttu-id="6687d-213">建立 *Pages/Instructors* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6687d-213">Create a *Pages/Instructors* folder.</span></span>
  * <span data-ttu-id="6687d-214">使用 `Instructor` 作為模型類別。</span><span class="sxs-lookup"><span data-stu-id="6687d-214">Use `Instructor` for the model class.</span></span>
  * <span data-ttu-id="6687d-215">使用現有內容類別，而非建立新的類別。</span><span class="sxs-lookup"><span data-stu-id="6687d-215">Use the existing context class instead of creating a new one.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6687d-216">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6687d-216">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="6687d-217">建立 *Pages/Instructors* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="6687d-217">Create a *Pages/Instructors* folder.</span></span>

* <span data-ttu-id="6687d-218">執行下列命令來 Scaffold Instructor 頁面。</span><span class="sxs-lookup"><span data-stu-id="6687d-218">Run the following command to scaffold the Instructor pages.</span></span>

  <span data-ttu-id="6687d-219">**在 Windows 上：**</span><span class="sxs-lookup"><span data-stu-id="6687d-219">**On Windows:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

  <span data-ttu-id="6687d-220">**在 Linux 或 macOS 上：**</span><span class="sxs-lookup"><span data-stu-id="6687d-220">**On Linux or macOS:**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages/Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="6687d-221">若要在更新之前查看 Scaffold 頁面的外觀，請執行應用程式並巡覽至 Instructors 頁面。</span><span class="sxs-lookup"><span data-stu-id="6687d-221">To see what the scaffolded page looks like before you update it, run the app and navigate to the Instructors page.</span></span>

<span data-ttu-id="6687d-222">使用下列程式碼更新*Pages/講師/Index. cshtml. .cs* ：</span><span class="sxs-lookup"><span data-stu-id="6687d-222">Update *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,19-53)]

<span data-ttu-id="6687d-223">`OnGetAsync` 方法會針對所選取講師的識別碼接受選擇性的路由資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-223">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="6687d-224">檢查 *Pages/Instructors/Index.cshtml.cs* 檔案中的查詢：</span><span class="sxs-lookup"><span data-stu-id="6687d-224">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading)]

<span data-ttu-id="6687d-225">這個程式碼會針對下列導覽屬性指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="6687d-225">The code specifies eager loading for the following navigation properties:</span></span>

* `Instructor.OfficeAssignment`
* `Instructor.CourseAssignments`
  * `CourseAssignments.Course`
    * `Course.Department`
    * `Course.Enrollments`
      * `Enrollment.Student`

<span data-ttu-id="6687d-226">請注意 `CourseAssignments` 和 `Course` 之 `Include` 和 `ThenInclude` 方法的重複。</span><span class="sxs-lookup"><span data-stu-id="6687d-226">Notice the repetition of `Include` and `ThenInclude` methods for `CourseAssignments` and `Course`.</span></span> <span data-ttu-id="6687d-227">若要針對 `Course` 實體的兩個導覽屬性指定積極式載入，則重複是必要的。</span><span class="sxs-lookup"><span data-stu-id="6687d-227">This repetition is necessary to specify eager loading for two navigation properties of the `Course` entity.</span></span>

<span data-ttu-id="6687d-228">下列程式碼會在已選取講師 (`id != null`) 時執行。</span><span class="sxs-lookup"><span data-stu-id="6687d-228">The following code executes when an instructor is selected (`id != null`).</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectInstructor)]

<span data-ttu-id="6687d-229">選取的講師會從檢視模型的講師清單中擷取。</span><span class="sxs-lookup"><span data-stu-id="6687d-229">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="6687d-230">檢視模型的 `Courses` 屬性則使用 `Course` 實體從該講師的 `CourseAssignments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-230">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="6687d-231">`Where` 方法會傳回集合。</span><span class="sxs-lookup"><span data-stu-id="6687d-231">The `Where` method returns a collection.</span></span> <span data-ttu-id="6687d-232">但是在此情況下，篩選會選取單一實體，因此 `Single` 會呼叫方法，將集合轉換成單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-232">But in this case, the filter will select a single entity, so the `Single` method is called to convert the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="6687d-233">`Instructor` 實體提供對 `CourseAssignments` 屬性的存取。</span><span class="sxs-lookup"><span data-stu-id="6687d-233">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="6687d-234">`CourseAssignments` 提供對相關 `Course` 實體的存取。</span><span class="sxs-lookup"><span data-stu-id="6687d-234">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="6687d-236">當集合只有一個項目時，將會在集合上使用 `Single` 方法。</span><span class="sxs-lookup"><span data-stu-id="6687d-236">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="6687d-237">如果集合是空的或是有多個項目，`Single` 方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6687d-237">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="6687d-238">替代方式是 `SingleOrDefault`，它會在集合是空的時傳回預設值 (在此情況下為 Null)。</span><span class="sxs-lookup"><span data-stu-id="6687d-238">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span>

<span data-ttu-id="6687d-239">選取課程時，下列程式碼會填入檢視模型的 `Enrollments` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6687d-239">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_SelectCourse)]

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="6687d-240">更新 Instructors [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-240">Update the instructors Index page</span></span>

<span data-ttu-id="6687d-241">使用下列程式碼更新 *Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="6687d-241">Update *Pages/Instructors/Index.cshtml* with the following code.</span></span>

[!code-cshtml[](intro/samples/cu30/Pages/Instructors/Index.cshtml?highlight=1,5,8,16-21,25-32,43-57,67-102,104-126)]

<span data-ttu-id="6687d-242">上述程式碼會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="6687d-242">The preceding code makes the following changes:</span></span>

* <span data-ttu-id="6687d-243">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="6687d-243">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="6687d-244">`"{id:int?}"` 是路由範本。</span><span class="sxs-lookup"><span data-stu-id="6687d-244">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="6687d-245">路由範本將 URL 中的整數查詢字串變更為路由資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-245">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="6687d-246">例如，只有在 `@page` 指示詞產生如下的 URL 時，按一下講師的 [選取]\*\*\*\* 連結：</span><span class="sxs-lookup"><span data-stu-id="6687d-246">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `https://localhost:5001/Instructors?id=2`

  <span data-ttu-id="6687d-247">頁面指示詞是 `@page "{id:int?}"` 時，URL 為：</span><span class="sxs-lookup"><span data-stu-id="6687d-247">When the page directive is `@page "{id:int?}"`, the URL is:</span></span>

  `https://localhost:5001/Instructors/2`

* <span data-ttu-id="6687d-248">新增 [辦公室]\*\*\*\* 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。</span><span class="sxs-lookup"><span data-stu-id="6687d-248">Adds an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="6687d-249">因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-249">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="6687d-250">新增 [課程]\*\*\*\* 資料行，以顯示每位講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-250">Adds a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="6687d-251">如需此 razor 語法的詳細資訊，請參閱[明確行轉換](xref:mvc/views/razor#explicit-line-transition)。</span><span class="sxs-lookup"><span data-stu-id="6687d-251">See [Explicit line transition](xref:mvc/views/razor#explicit-line-transition) for more about this razor syntax.</span></span>

* <span data-ttu-id="6687d-252">新增程式碼，將 `class="success"` 動態新增至所選講師和課程的 `tr` 項目。</span><span class="sxs-lookup"><span data-stu-id="6687d-252">Adds code that dynamically adds `class="success"` to the `tr` element of the selected instructor and course.</span></span> <span data-ttu-id="6687d-253">這會使用啟動程序類別設定所選取資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="6687d-253">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="6687d-254">新增標示為**選取**的超連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-254">Adds a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="6687d-255">此連結會將所選取的講師識別碼傳送至 `Index` 方法，並設定背景色彩。</span><span class="sxs-lookup"><span data-stu-id="6687d-255">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

* <span data-ttu-id="6687d-256">新增所選講師的課程資料表。</span><span class="sxs-lookup"><span data-stu-id="6687d-256">Adds a table of courses for the selected Instructor.</span></span>

* <span data-ttu-id="6687d-257">新增所選課程的學生註冊資料表。</span><span class="sxs-lookup"><span data-stu-id="6687d-257">Adds a table of student enrollments for the selected course.</span></span>

<span data-ttu-id="6687d-258">執行應用程式，然後選取 [**講師**] 索引標籤。此頁面會顯示 `Location` 來自相關實體的（office） `OfficeAssignment` 。</span><span class="sxs-lookup"><span data-stu-id="6687d-258">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="6687d-259">如果 `OfficeAssignment` 為 Null，則會顯示空的資料表資料格。</span><span class="sxs-lookup"><span data-stu-id="6687d-259">If `OfficeAssignment` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="6687d-260">按一下講師的 [選取]\*\*\*\* 連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-260">Click on the **Select** link for an instructor.</span></span> <span data-ttu-id="6687d-261">資料列樣式會變更，並會顯示指派給該講師的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-261">The row style changes and courses assigned to that instructor are displayed.</span></span>

<span data-ttu-id="6687d-262">選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="6687d-262">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index30.png)

## <a name="using-single"></a><span data-ttu-id="6687d-264">使用 Single</span><span class="sxs-lookup"><span data-stu-id="6687d-264">Using Single</span></span>

<span data-ttu-id="6687d-265">`Single` 方法可以傳入 `Where` 條件，而不是個別呼叫 `Where` 方法：</span><span class="sxs-lookup"><span data-stu-id="6687d-265">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="6687d-266">使用 `Single` 搭配 Where 條件是個人喜好設定。</span><span class="sxs-lookup"><span data-stu-id="6687d-266">Use of `Single` with a Where condition is a matter of personal preference.</span></span> <span data-ttu-id="6687d-267">其不會對使用 `Where` 方法提供任何益處。</span><span class="sxs-lookup"><span data-stu-id="6687d-267">It provides no benefits over using the `Where` method.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="6687d-268">明確式載入</span><span class="sxs-lookup"><span data-stu-id="6687d-268">Explicit loading</span></span>

<span data-ttu-id="6687d-269">目前程式碼針對 `Enrollments` 和 `Students` 指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="6687d-269">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu30snapshots/6-related/Pages/Instructors/Index1.cshtml.cs?name=snippet_EagerLoading&highlight=6-9)]

<span data-ttu-id="6687d-270">假設使用者很少會想要查看課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="6687d-270">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="6687d-271">在此情況下，最佳方法就是在要求時，只載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-271">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="6687d-272">在本節中，`OnGetAsync` 更新為針對 `Enrollments` 和 `Students` 使用明確式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-272">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="6687d-273">使用下列程式碼更新 *Pages/Instructors/Index.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="6687d-273">Update *Pages/Instructors/Index.cshtml.cs* with the following code.</span></span>

[!code-csharp[](intro/samples/cu30/Pages/Instructors/Index.cshtml.cs?highlight=31-35,52-56)]

<span data-ttu-id="6687d-274">上述程式碼會捨棄註冊和學生資料的 *ThenInclude* 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="6687d-274">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="6687d-275">如果選取了課程，則明確載入的程式碼會擷取：</span><span class="sxs-lookup"><span data-stu-id="6687d-275">If a course is selected, the explicit loading code retrieves:</span></span>

* <span data-ttu-id="6687d-276">所選取課程的 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-276">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="6687d-277">每個 `Enrollment` 的 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-277">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="6687d-278">請注意，上述程式碼會將 `.AsNoTracking()` 標記為註解。</span><span class="sxs-lookup"><span data-stu-id="6687d-278">Notice that the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="6687d-279">針對所追蹤的實體，導覽屬性只能進行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-279">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="6687d-280">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="6687d-280">Test the app.</span></span> <span data-ttu-id="6687d-281">就使用者的觀點而言，應用程式行為與先前版本相同。</span><span class="sxs-lookup"><span data-stu-id="6687d-281">From a user's perspective, the app behaves identically to the previous version.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6687d-282">接下來的步驟</span><span class="sxs-lookup"><span data-stu-id="6687d-282">Next steps</span></span>

<span data-ttu-id="6687d-283">下一個教學課程會示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-283">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="6687d-284">[上一個教學](xref:data/ef-rp/complex-data-model) 
> 課程[下一個教學](xref:data/ef-rp/update-related-data)課程</span><span class="sxs-lookup"><span data-stu-id="6687d-284">[Previous tutorial](xref:data/ef-rp/complex-data-model)
[Next tutorial](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6687d-285">在本教學課程中，將會讀取和顯示相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-285">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="6687d-286">相關資料是 EF Core 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-286">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="6687d-287">若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="6687d-287">If you run into problems you can't solve, [download or view the completed app.](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="6687d-288">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="6687d-288">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="6687d-289">下圖顯示本教學課程的已完成頁面：</span><span class="sxs-lookup"><span data-stu-id="6687d-289">The following illustrations show the completed pages for this tutorial:</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="6687d-292">相關資料的積極式、明確式和消極式載入</span><span class="sxs-lookup"><span data-stu-id="6687d-292">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="6687d-293">EF Core 有幾種方式可以將相關資料載入到實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="6687d-293">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="6687d-294">[積極式載入](/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-294">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="6687d-295">積極式載入是指某一類型實體的查詢同時也會載入相關實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-295">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="6687d-296">讀取實體時，將會擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-296">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="6687d-297">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-297">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="6687d-298">EF Core 將針對某些類型的積極式載入發出多個查詢。</span><span class="sxs-lookup"><span data-stu-id="6687d-298">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="6687d-299">相較於 EF6 中的某些查詢只有單一查詢的情況，發出多個查詢可能更有效率。</span><span class="sxs-lookup"><span data-stu-id="6687d-299">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="6687d-300">積極式載入是使用 `Include` 和 `ThenInclude` 方法加以指定。</span><span class="sxs-lookup"><span data-stu-id="6687d-300">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="6687d-302">當集合導覽包含在內時，積極式載入會傳送多個查詢：</span><span class="sxs-lookup"><span data-stu-id="6687d-302">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="6687d-303">針對主查詢傳送一個查詢</span><span class="sxs-lookup"><span data-stu-id="6687d-303">One query for the main query</span></span> 
  * <span data-ttu-id="6687d-304">針對負載樹狀結構中的每個集合「邊緣」傳送一個查詢。</span><span class="sxs-lookup"><span data-stu-id="6687d-304">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="6687d-305">使用 `Load` 的個別查詢：資料可以在個別查詢中擷取，而 EF Core 會「修正」導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-305">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="6687d-306">「修正」表示 EF Core 會自動填入導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-306">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="6687d-307">使用 `Load` 的個別查詢更像是明確式載入，而不是積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-307">Separate queries with `Load` is more like explicit loading than eager loading.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="6687d-309">注意：EF Core 會將導覽屬性自動修正為先前已載入至內容執行個體的任何其他實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-309">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="6687d-310">即使「未」\*\* 明確包含導覽屬性的資料，如果先前已載入某些或所有相關實體，仍然可能會填入該屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-310">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="6687d-311">[明確載入](/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-311">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="6687d-312">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-312">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="6687d-313">必須撰寫程式碼，才能在需要時擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-313">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="6687d-314">使用個別查詢的明確式載入會導致多個查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="6687d-314">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="6687d-315">透過明確式載入，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-315">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="6687d-316">請使用 `Load` 方法來執行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-316">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="6687d-317">例如：</span><span class="sxs-lookup"><span data-stu-id="6687d-317">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="6687d-319">消極式[載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-319">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="6687d-320">[EF Core 已在 2.1 版中新增消極式載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="6687d-320">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="6687d-321">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-321">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="6687d-322">第一次存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-322">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="6687d-323">每當第一次存取導覽屬性時，查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="6687d-323">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="6687d-324">`Select` 運算子只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-324">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="6687d-325">建立顯示部門名稱的 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-325">Create a Course page that displays department name</span></span>

<span data-ttu-id="6687d-326">Course 實體包含導覽屬性，其中包含 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-326">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="6687d-327">`Department` 實體包含已指派課程的部門。</span><span class="sxs-lookup"><span data-stu-id="6687d-327">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="6687d-328">若要在課程清單中顯示所指派部門的名稱：</span><span class="sxs-lookup"><span data-stu-id="6687d-328">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="6687d-329">從 `Department` 實體取得 `Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-329">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="6687d-330">`Department` 實體來自 `Course.Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="6687d-330">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![Course.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>

### <a name="scaffold-the-course-model"></a><span data-ttu-id="6687d-332">Scaffold Course 模型</span><span class="sxs-lookup"><span data-stu-id="6687d-332">Scaffold the Course model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6687d-333">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6687d-333">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="6687d-334">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Course`。</span><span class="sxs-lookup"><span data-stu-id="6687d-334">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6687d-335">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6687d-335">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="6687d-336">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="6687d-336">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

---

<span data-ttu-id="6687d-337">上述命令會 Scaffold `Course` 模型。</span><span class="sxs-lookup"><span data-stu-id="6687d-337">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="6687d-338">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="6687d-338">Open the project in Visual Studio.</span></span>

<span data-ttu-id="6687d-339">開啟 *Pages/Courses/Index.cshtml.cs* 並檢查 `OnGetAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="6687d-339">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="6687d-340">Scaffolding 引擎已針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-340">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="6687d-341">`Include` 方法可指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-341">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="6687d-342">執行應用程式並選取**課程**連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-342">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="6687d-343">部門資料行便會顯示沒有用的 `DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="6687d-343">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="6687d-344">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="6687d-344">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="6687d-345">上述程式碼會新增 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="6687d-345">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="6687d-346">`AsNoTracking` 可改善效能，因為不會追蹤傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-346">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="6687d-347">不會追蹤實體的原因是它們不會在目前的內容中更新。</span><span class="sxs-lookup"><span data-stu-id="6687d-347">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="6687d-348">以下列醒目提示的標記更新 *Pages/Courses/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="6687d-348">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="6687d-349">已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="6687d-349">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="6687d-350">已將標題從「索引」) 變更為「課程」。</span><span class="sxs-lookup"><span data-stu-id="6687d-350">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="6687d-351">新增顯示 `CourseID` 屬性值的 [編號]\*\*\*\* 資料行。</span><span class="sxs-lookup"><span data-stu-id="6687d-351">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="6687d-352">主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="6687d-352">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="6687d-353">不過，在此情況下，主索引鍵有意義。</span><span class="sxs-lookup"><span data-stu-id="6687d-353">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="6687d-354">變更 [部門]\*\*\*\* 資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="6687d-354">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="6687d-355">此程式碼會顯示已載入到 `Department` 導覽屬性之 `Department` 實體的 `Name` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6687d-355">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="6687d-356">執行應用程式，並選取 [Courses]\*\*\*\* 索引標籤來查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="6687d-356">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

<a name="select"></a>

### <a name="loading-related-data-with-select"></a><span data-ttu-id="6687d-358">使用 Select 載入相關資料</span><span class="sxs-lookup"><span data-stu-id="6687d-358">Loading related data with Select</span></span>

<span data-ttu-id="6687d-359">`OnGetAsync` 方法使用 `Include` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="6687d-359">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="6687d-360">`Select` 運算子只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-360">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="6687d-361">如果是單一項目 (例如 `Department.Name`)，它會使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="6687d-361">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="6687d-362">如果是集合，它會使用另一種資料庫存取，但集合上的 `Include` 運算子也是如此。</span><span class="sxs-lookup"><span data-stu-id="6687d-362">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="6687d-363">下列程式碼使用 `Select` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="6687d-363">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="6687d-364">`CourseViewModel`：</span><span class="sxs-lookup"><span data-stu-id="6687d-364">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="6687d-365">如需完整範例，請參閱 [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)。</span><span class="sxs-lookup"><span data-stu-id="6687d-365">See [IndexSelect.cshtml](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="6687d-366">建立顯示課程和註冊的 Instructors 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-366">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="6687d-367">在本節中，將會建立 Instructors 頁面。</span><span class="sxs-lookup"><span data-stu-id="6687d-367">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="6687d-368">![講師索引頁面](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="6687d-368">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="6687d-369">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="6687d-369">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="6687d-370">講師清單會顯示來自 `OfficeAssignment` 實體 (上述映像中的 Office) 的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-370">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="6687d-371">`Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="6687d-371">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="6687d-372">積極式載入用於 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-372">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="6687d-373">若需要顯示相關資料，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="6687d-373">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="6687d-374">在此情況下，將會顯示講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="6687d-374">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="6687d-375">當使用者選取講師 (上述映像中的 Harui) 時，便會顯示相關的 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-375">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="6687d-376">`Instructor` 與 `Course` 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="6687d-376">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="6687d-377">將會針對 `Course` 實體和其相關 `Department` 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-377">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="6687d-378">在此情況下，個別查詢可能更有效率，因為只需要所選取講師的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-378">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="6687d-379">這個範例示範如何使用導覽屬性中實體之導覽屬性的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-379">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="6687d-380">當使用者選取課程 (上述映像中的 Chemistry)，隨即顯示 `Enrollments` 中的相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-380">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="6687d-381">在上述映像中，將會顯示學生姓名和年級。</span><span class="sxs-lookup"><span data-stu-id="6687d-381">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="6687d-382">`Course` 與 `Enrollment` 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="6687d-382">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="6687d-383">建立 Instructor [索引] 檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="6687d-383">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="6687d-384">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-384">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="6687d-385">建立的檢視模型會包含三個實體代表三個資料表。</span><span class="sxs-lookup"><span data-stu-id="6687d-385">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="6687d-386">在 *SchoolViewModels* 資料夾中，以下列程式碼建立 *InstructorIndexData.cs*：</span><span class="sxs-lookup"><span data-stu-id="6687d-386">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="6687d-387">Scaffold Instructor 模型</span><span class="sxs-lookup"><span data-stu-id="6687d-387">Scaffold the Instructor model</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="6687d-388">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="6687d-388">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="6687d-389">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Instructor`。</span><span class="sxs-lookup"><span data-stu-id="6687d-389">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="6687d-390">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="6687d-390">Visual Studio Code</span></span>](#tab/visual-studio-code)

 <span data-ttu-id="6687d-391">執行以下命令：</span><span class="sxs-lookup"><span data-stu-id="6687d-391">Run the following command:</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

---

<span data-ttu-id="6687d-392">上述命令會 Scaffold `Instructor` 模型。</span><span class="sxs-lookup"><span data-stu-id="6687d-392">The preceding command scaffolds the `Instructor` model.</span></span> 
<span data-ttu-id="6687d-393">執行應用程式並巡覽至講師頁面。</span><span class="sxs-lookup"><span data-stu-id="6687d-393">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="6687d-394">以下列程式碼取代 *Pages/Instructors/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="6687d-394">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="6687d-395">`OnGetAsync` 方法會針對所選取講師的識別碼接受選擇性的路由資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-395">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="6687d-396">檢查 *Pages/Instructors/Index.cshtml.cs* 檔案中的查詢：</span><span class="sxs-lookup"><span data-stu-id="6687d-396">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="6687d-397">查詢具有兩個 Include：</span><span class="sxs-lookup"><span data-stu-id="6687d-397">The query has two includes:</span></span>

* <span data-ttu-id="6687d-398">`OfficeAssignment`：顯示在 [Instructors 檢視](#IP)。</span><span class="sxs-lookup"><span data-stu-id="6687d-398">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="6687d-399">`CourseAssignments`：它顯示所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-399">`CourseAssignments`: Which brings in the courses taught.</span></span>

### <a name="update-the-instructors-index-page"></a><span data-ttu-id="6687d-400">更新 Instructors [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="6687d-400">Update the instructors Index page</span></span>

<span data-ttu-id="6687d-401">以下列標記更新 *Pages/Instructors/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="6687d-401">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="6687d-402">上述標記會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="6687d-402">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="6687d-403">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="6687d-403">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="6687d-404">`"{id:int?}"` 是路由範本。</span><span class="sxs-lookup"><span data-stu-id="6687d-404">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="6687d-405">路由範本將 URL 中的整數查詢字串變更為路由資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-405">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="6687d-406">例如，只有在 `@page` 指示詞產生如下的 URL 時，按一下講師的 [選取]\*\*\*\* 連結：</span><span class="sxs-lookup"><span data-stu-id="6687d-406">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

  `http://localhost:1234/Instructors?id=2`

  <span data-ttu-id="6687d-407">頁面指示詞是 `@page "{id:int?}"` 時，先前的 URL 為：</span><span class="sxs-lookup"><span data-stu-id="6687d-407">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

  `http://localhost:1234/Instructors/2`

* <span data-ttu-id="6687d-408">頁面標題是 **Instructors**。</span><span class="sxs-lookup"><span data-stu-id="6687d-408">Page title is **Instructors**.</span></span>
* <span data-ttu-id="6687d-409">新增 [辦公室]\*\*\*\* 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。</span><span class="sxs-lookup"><span data-stu-id="6687d-409">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="6687d-410">因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-410">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="6687d-411">新增 [課程]\*\*\*\* 資料行，以顯示每位講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-411">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="6687d-412">如需此 razor 語法的詳細資訊，請參閱[明確行轉換](xref:mvc/views/razor#explicit-line-transition)。</span><span class="sxs-lookup"><span data-stu-id="6687d-412">See [Explicit line transition](xref:mvc/views/razor#explicit-line-transition) for more about this razor syntax.</span></span>

* <span data-ttu-id="6687d-413">新增程式碼，將 `class="success"` 動態新增至所選取講師的 `tr` 項目。</span><span class="sxs-lookup"><span data-stu-id="6687d-413">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="6687d-414">這會使用啟動程序類別設定所選取資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="6687d-414">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="6687d-415">新增標示為**選取**的超連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-415">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="6687d-416">此連結會將所選取的講師識別碼傳送至 `Index` 方法，並設定背景色彩。</span><span class="sxs-lookup"><span data-stu-id="6687d-416">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="6687d-417">執行應用程式，然後選取 [**講師**] 索引標籤。此頁面會顯示 `Location` 來自相關實體的（office） `OfficeAssignment` 。</span><span class="sxs-lookup"><span data-stu-id="6687d-417">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="6687d-418">如果 OfficeAssignment 是 Null，就會顯示空的資料表資料格。</span><span class="sxs-lookup"><span data-stu-id="6687d-418">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

<span data-ttu-id="6687d-419">按一下**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-419">Click on the **Select** link.</span></span> <span data-ttu-id="6687d-420">資料列樣式變更。</span><span class="sxs-lookup"><span data-stu-id="6687d-420">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="6687d-421">新增選取的講師所教授的課程</span><span class="sxs-lookup"><span data-stu-id="6687d-421">Add courses taught by selected instructor</span></span>

<span data-ttu-id="6687d-422">以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="6687d-422">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="6687d-423">新增 `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="6687d-423">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="6687d-424">檢查已更新的查詢：</span><span class="sxs-lookup"><span data-stu-id="6687d-424">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="6687d-425">上述查詢會新增 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-425">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="6687d-426">下列程式碼會在已選取講師 (`id != null`) 時執行。</span><span class="sxs-lookup"><span data-stu-id="6687d-426">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="6687d-427">選取的講師會從檢視模型的講師清單中擷取。</span><span class="sxs-lookup"><span data-stu-id="6687d-427">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="6687d-428">檢視模型的 `Courses` 屬性則使用 `Course` 實體從該講師的 `CourseAssignments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-428">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="6687d-429">`Where` 方法會傳回集合。</span><span class="sxs-lookup"><span data-stu-id="6687d-429">The `Where` method returns a collection.</span></span> <span data-ttu-id="6687d-430">在上述 `Where` 方法中，只會傳回一個單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-430">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="6687d-431">`Single` 方法會將集合轉換成單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-431">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="6687d-432">`Instructor` 實體提供對 `CourseAssignments` 屬性的存取。</span><span class="sxs-lookup"><span data-stu-id="6687d-432">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="6687d-433">`CourseAssignments` 提供對相關 `Course` 實體的存取。</span><span class="sxs-lookup"><span data-stu-id="6687d-433">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="6687d-435">當集合只有一個項目時，將會在集合上使用 `Single` 方法。</span><span class="sxs-lookup"><span data-stu-id="6687d-435">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="6687d-436">如果集合是空的或是有多個項目，`Single` 方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="6687d-436">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="6687d-437">替代方式是 `SingleOrDefault`，它會在集合是空的時傳回預設值 (在此情況下為 Null)。</span><span class="sxs-lookup"><span data-stu-id="6687d-437">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="6687d-438">在空集合上使用 `SingleOrDefault`：</span><span class="sxs-lookup"><span data-stu-id="6687d-438">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="6687d-439">造成例外狀況 (由於嘗試在 Null 參考上尋找 `Courses` 屬性)。</span><span class="sxs-lookup"><span data-stu-id="6687d-439">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="6687d-440">例外狀況訊息會不太清楚地指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="6687d-440">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="6687d-441">選取課程時，下列程式碼會填入檢視模型的 `Enrollments` 屬性：</span><span class="sxs-lookup"><span data-stu-id="6687d-441">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="6687d-442">將下列標記新增至*Pages/講師/Index. cshtml* Razor 頁面的結尾：</span><span class="sxs-lookup"><span data-stu-id="6687d-442">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="6687d-443">當選取講師時，上述標記會顯示與講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="6687d-443">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="6687d-444">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="6687d-444">Test the app.</span></span> <span data-ttu-id="6687d-445">按一下講師頁面上的**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="6687d-445">Click on a **Select** link on the instructors page.</span></span>

### <a name="show-student-data"></a><span data-ttu-id="6687d-446">顯示學生資料</span><span class="sxs-lookup"><span data-stu-id="6687d-446">Show student data</span></span>

<span data-ttu-id="6687d-447">在本節中，應用程式會更新以顯示所選取課程的學生資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-447">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="6687d-448">以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中 `OnGetAsync` 方法的查詢：</span><span class="sxs-lookup"><span data-stu-id="6687d-448">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="6687d-449">更新 *Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="6687d-449">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="6687d-450">將下列標記新增至檔案結尾：</span><span class="sxs-lookup"><span data-stu-id="6687d-450">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="6687d-451">上述標記會顯示註冊所選取課程的學生清單。</span><span class="sxs-lookup"><span data-stu-id="6687d-451">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="6687d-452">重新整理頁面，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="6687d-452">Refresh the page and select an instructor.</span></span> <span data-ttu-id="6687d-453">選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="6687d-453">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="6687d-455">使用 Single</span><span class="sxs-lookup"><span data-stu-id="6687d-455">Using Single</span></span>

<span data-ttu-id="6687d-456">`Single` 方法可以傳入 `Where` 條件，而不是個別呼叫 `Where` 方法：</span><span class="sxs-lookup"><span data-stu-id="6687d-456">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="6687d-457">比起使用 `Where`，上述 `Single` 方法並沒有任何優勢。</span><span class="sxs-lookup"><span data-stu-id="6687d-457">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="6687d-458">某些開發人員偏好使用 `Single` 方法樣式。</span><span class="sxs-lookup"><span data-stu-id="6687d-458">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="6687d-459">明確式載入</span><span class="sxs-lookup"><span data-stu-id="6687d-459">Explicit loading</span></span>

<span data-ttu-id="6687d-460">目前程式碼針對 `Enrollments` 和 `Students` 指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="6687d-460">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="6687d-461">假設使用者很少會想要查看課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="6687d-461">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="6687d-462">在此情況下，最佳方法就是在要求時，只載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-462">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="6687d-463">在本節中，`OnGetAsync` 更新為針對 `Enrollments` 和 `Students` 使用明確式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-463">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="6687d-464">使用下列程式碼更新 `OnGetAsync`：</span><span class="sxs-lookup"><span data-stu-id="6687d-464">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="6687d-465">上述程式碼會捨棄註冊和學生資料的 *ThenInclude* 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="6687d-465">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="6687d-466">如果選取了課程，醒目提示的程式碼就會擷取：</span><span class="sxs-lookup"><span data-stu-id="6687d-466">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="6687d-467">所選取課程的 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-467">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="6687d-468">每個 `Enrollment` 的 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="6687d-468">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="6687d-469">請注意，上述程式碼會將 `.AsNoTracking()` 註解化。</span><span class="sxs-lookup"><span data-stu-id="6687d-469">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="6687d-470">針對所追蹤的實體，導覽屬性只能進行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="6687d-470">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="6687d-471">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="6687d-471">Test the app.</span></span> <span data-ttu-id="6687d-472">就使用者的觀點而言，應用程式行為與之前的版本相同。</span><span class="sxs-lookup"><span data-stu-id="6687d-472">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="6687d-473">下一個教學課程會示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="6687d-473">The next tutorial shows how to update related data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6687d-474">其他資源</span><span class="sxs-lookup"><span data-stu-id="6687d-474">Additional resources</span></span>

* [<span data-ttu-id="6687d-475">這個教學課程的 YouTube 版本 (第 1 部分)</span><span class="sxs-lookup"><span data-stu-id="6687d-475">YouTube version of this tutorial (part1)</span></span>](https://www.youtube.com/watch?v=PzKimUDmrvE)
* [<span data-ttu-id="6687d-476">這個教學課程的 YouTube 版本 (第 2 部分)</span><span class="sxs-lookup"><span data-stu-id="6687d-476">YouTube version of this tutorial (part2)</span></span>](https://www.youtube.com/watch?v=xvDDrIHv5ko)

>[!div class="step-by-step"]
><span data-ttu-id="6687d-477">[上一個](xref:data/ef-rp/complex-data-model) 
>[下一步](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="6687d-477">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>

::: moniker-end
