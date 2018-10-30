---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - 讀取相關資料 - 6/8
author: rick-anderson
description: 在此教學課程中，您可以讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: data/ef-rp/read-related-data
ms.openlocfilehash: cf8733e1e806c4be0c4b217fc45c7a338a03a3ce
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207552"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---read-related-data---6-of-8"></a><span data-ttu-id="70675-103">ASP.NET Core 中的 Razor 頁面與 EF Core - 讀取相關資料 - 6/8</span><span class="sxs-lookup"><span data-stu-id="70675-103">Razor Pages with EF Core in ASP.NET Core - Read Related Data - 6 of 8</span></span>

<span data-ttu-id="70675-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="70675-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="70675-105">在本教學課程中，將會讀取和顯示相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="70675-106">相關資料是 EF Core 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="70675-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="70675-107">若您遇到無法解決的問題，請[下載或檢視完整應用程式。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span><span class="sxs-lookup"><span data-stu-id="70675-107">If you run into problems you can't solve, [download or view the completed app.](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)</span></span> <span data-ttu-id="70675-108">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="70675-108">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="70675-109">下圖顯示本教學課程的已完成頁面：</span><span class="sxs-lookup"><span data-stu-id="70675-109">The following illustrations show the completed pages for this tutorial:</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="70675-112">相關資料的積極式、明確式和消極式載入</span><span class="sxs-lookup"><span data-stu-id="70675-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="70675-113">EF Core 有幾種方式可以將相關資料載入到實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="70675-113">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="70675-114">[積極式載入](/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="70675-114">[Eager loading](/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="70675-115">積極式載入是指某一類型實體的查詢同時也會載入相關實體。</span><span class="sxs-lookup"><span data-stu-id="70675-115">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="70675-116">讀取實體時，將會擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-116">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="70675-117">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="70675-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="70675-118">EF Core 將針對某些類型的積極式載入發出多個查詢。</span><span class="sxs-lookup"><span data-stu-id="70675-118">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="70675-119">相較於 EF6 中的某些查詢只有單一查詢的情況，發出多個查詢可能更有效率。</span><span class="sxs-lookup"><span data-stu-id="70675-119">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="70675-120">積極式載入是使用 `Include` 和 `ThenInclude` 方法加以指定。</span><span class="sxs-lookup"><span data-stu-id="70675-120">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
  <span data-ttu-id="70675-122">當集合導覽包含在內時，積極式載入會傳送多個查詢：</span><span class="sxs-lookup"><span data-stu-id="70675-122">Eager loading sends multiple queries when a collection navigation is included:</span></span>

  * <span data-ttu-id="70675-123">針對主查詢傳送一個查詢</span><span class="sxs-lookup"><span data-stu-id="70675-123">One query for the main query</span></span> 
  * <span data-ttu-id="70675-124">針對負載樹狀結構中的每個集合「邊緣」傳送一個查詢。</span><span class="sxs-lookup"><span data-stu-id="70675-124">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="70675-125">使用 `Load` 的個別查詢：資料可以在個別查詢中擷取，而 EF Core 會「修正」導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="70675-125">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="70675-126">「修正」表示 EF Core 會自動填入導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="70675-126">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="70675-127">使用 `Load` 的個別查詢更像是明確式載入，而不是積極式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-127">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

  <span data-ttu-id="70675-129">注意：EF Core 會將導覽屬性自動修正為先前已載入至內容執行個體的任何其他實體。</span><span class="sxs-lookup"><span data-stu-id="70675-129">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="70675-130">即使「未」明確包含導覽屬性的資料，如果先前已載入某些或所有相關實體，仍然可能會填入該屬性。</span><span class="sxs-lookup"><span data-stu-id="70675-130">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="70675-131">[明確式載入](/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="70675-131">[Explicit loading](/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="70675-132">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="70675-133">必須撰寫程式碼，才能在需要時擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-133">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="70675-134">使用個別查詢的明確式載入會導致多個查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="70675-134">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="70675-135">透過明確式載入，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="70675-135">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="70675-136">請使用 `Load` 方法來執行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-136">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="70675-137">例如: </span><span class="sxs-lookup"><span data-stu-id="70675-137">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="70675-139">[消極式載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="70675-139">[Lazy loading](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="70675-140">[EF Core 已在 2.1 版中新增消極式載入](/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="70675-140">[Lazy loading was added to EF Core in version 2.1](/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="70675-141">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-141">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="70675-142">第一次存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="70675-142">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="70675-143">每當第一次存取導覽屬性時，查詢會傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="70675-143">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="70675-144">`Select` 運算子只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-144">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-course-page-that-displays-department-name"></a><span data-ttu-id="70675-145">建立顯示部門名稱的 Course 頁面</span><span class="sxs-lookup"><span data-stu-id="70675-145">Create a Course page that displays department name</span></span>

<span data-ttu-id="70675-146">Course 實體包含導覽屬性，其中包含 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-146">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="70675-147">`Department` 實體包含已指派課程的部門。</span><span class="sxs-lookup"><span data-stu-id="70675-147">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="70675-148">若要在課程清單中顯示所指派部門的名稱：</span><span class="sxs-lookup"><span data-stu-id="70675-148">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="70675-149">從 `Department` 實體取得 `Name` 屬性。</span><span class="sxs-lookup"><span data-stu-id="70675-149">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="70675-150">`Department` 實體來自 `Course.Department` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="70675-150">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="70675-152">Scaffold Course 模型</span><span class="sxs-lookup"><span data-stu-id="70675-152">Scaffold the Course model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="70675-153">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70675-153">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="70675-154">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Course`。</span><span class="sxs-lookup"><span data-stu-id="70675-154">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Course` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="70675-155">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="70675-155">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="70675-156">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="70675-156">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
  ```

------

<span data-ttu-id="70675-157">上述命令會 Scaffold `Course` 模型。</span><span class="sxs-lookup"><span data-stu-id="70675-157">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="70675-158">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="70675-158">Open the project in Visual Studio.</span></span>

<span data-ttu-id="70675-159">開啟 *Pages/Courses/Index.cshtml.cs* 並檢查 `OnGetAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="70675-159">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="70675-160">Scaffolding 引擎已針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-160">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="70675-161">`Include` 方法可指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-161">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="70675-162">執行應用程式並選取**課程**連結。</span><span class="sxs-lookup"><span data-stu-id="70675-162">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="70675-163">部門資料行便會顯示沒有用的 `DepartmentID`。</span><span class="sxs-lookup"><span data-stu-id="70675-163">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="70675-164">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="70675-164">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="70675-165">上述程式碼會新增 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="70675-165">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="70675-166">`AsNoTracking` 可改善效能，因為不會追蹤傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="70675-166">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="70675-167">不會追蹤實體的原因是它們不會在目前的內容中更新。</span><span class="sxs-lookup"><span data-stu-id="70675-167">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="70675-168">以下列醒目提示的標記更新 *Pages/Courses/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="70675-168">Update *Pages/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="70675-169">已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="70675-169">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="70675-170">已將標題從 Index 變更為 Courses。</span><span class="sxs-lookup"><span data-stu-id="70675-170">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="70675-171">新增顯示 `CourseID` 屬性值的 [編號] 資料行。</span><span class="sxs-lookup"><span data-stu-id="70675-171">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="70675-172">主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="70675-172">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="70675-173">不過，在此情況下，主索引鍵有意義。</span><span class="sxs-lookup"><span data-stu-id="70675-173">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="70675-174">變更 [部門]  資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="70675-174">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="70675-175">此程式碼會顯示已載入到 `Department` 導覽屬性之 `Department` 實體的 `Name` 屬性：</span><span class="sxs-lookup"><span data-stu-id="70675-175">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="70675-176">執行應用程式，並選取 [Courses]  索引標籤來查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="70675-176">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="70675-178">使用 Select 載入相關資料</span><span class="sxs-lookup"><span data-stu-id="70675-178">Loading related data with Select</span></span>

<span data-ttu-id="70675-179">`OnGetAsync` 方法使用 `Include` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="70675-179">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="70675-180">`Select` 運算子只會載入所需的相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-180">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="70675-181">如果是單一項目 (例如 `Department.Name`)，它會使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="70675-181">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="70675-182">如果是集合，它會使用另一種資料庫存取，但集合上的 `Include` 運算子也是如此。</span><span class="sxs-lookup"><span data-stu-id="70675-182">For collections, it uses another database access, but so does the `Include` operator on collections.</span></span>

<span data-ttu-id="70675-183">下列程式碼使用 `Select` 方法載入相關資料：</span><span class="sxs-lookup"><span data-stu-id="70675-183">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="70675-184">`CourseViewModel`：</span><span class="sxs-lookup"><span data-stu-id="70675-184">The `CourseViewModel`:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="70675-185">如需完整範例，請參閱 [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) 和 [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)。</span><span class="sxs-lookup"><span data-stu-id="70675-185">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="70675-186">建立顯示「課程」和「註冊」的 Instructors 頁面</span><span class="sxs-lookup"><span data-stu-id="70675-186">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="70675-187">在本節中，將會建立 Instructors 頁面。</span><span class="sxs-lookup"><span data-stu-id="70675-187">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="70675-188">![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="70675-188">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="70675-189">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="70675-189">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="70675-190">講師清單會顯示來自 `OfficeAssignment` 實體 (上述映像中的 Office) 的相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-190">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="70675-191">`Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="70675-191">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="70675-192">積極式載入用於 `OfficeAssignment` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-192">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="70675-193">若需要顯示相關資料，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="70675-193">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="70675-194">在此情況下，將會顯示講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="70675-194">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="70675-195">當使用者選取講師 (上述映像中的 Harui) 時，便會顯示相關的 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-195">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="70675-196">`Instructor` 與 `Course` 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="70675-196">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="70675-197">將會針對 `Course` 實體和其相關 `Department` 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-197">Eager loading is used for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="70675-198">在此情況下，個別查詢可能更有效率，因為只需要所選取講師的課程。</span><span class="sxs-lookup"><span data-stu-id="70675-198">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="70675-199">這個範例示範如何使用導覽屬性中實體之導覽屬性的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-199">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="70675-200">當使用者選取課程 (上述映像中的 Chemistry)，隨即顯示 `Enrollments` 中的相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-200">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="70675-201">在上述映像中，將會顯示學生姓名和年級。</span><span class="sxs-lookup"><span data-stu-id="70675-201">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="70675-202">`Course` 與 `Enrollment` 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="70675-202">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="70675-203">建立 Instructor 索引檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="70675-203">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="70675-204">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="70675-204">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="70675-205">建立的檢視模型會包含三個實體代表三個資料表。</span><span class="sxs-lookup"><span data-stu-id="70675-205">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="70675-206">在 *SchoolViewModels* 資料夾中，以下列程式碼建立 *InstructorIndexData.cs*：</span><span class="sxs-lookup"><span data-stu-id="70675-206">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="70675-207">Scaffold Instructor 模型</span><span class="sxs-lookup"><span data-stu-id="70675-207">Scaffold the Instructor model</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="70675-208">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="70675-208">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="70675-209">請遵循[建立學生結構模型](xref:data/ef-rp/intro#scaffold-the-student-model)中的指示，並為模型類別使用 `Instructor`。</span><span class="sxs-lookup"><span data-stu-id="70675-209">Follow the instructions in [Scaffold the student model](xref:data/ef-rp/intro#scaffold-the-student-model) and use `Instructor` for the model class.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="70675-210">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="70675-210">.NET Core CLI</span></span>](#tab/netcore-cli)

 <span data-ttu-id="70675-211">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="70675-211">Run the following command:</span></span>

  ```console
  dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
  ```

------

<span data-ttu-id="70675-212">上述命令會 Scaffold `Instructor` 模型。</span><span class="sxs-lookup"><span data-stu-id="70675-212">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="70675-213">執行應用程式並巡覽至講師頁面。</span><span class="sxs-lookup"><span data-stu-id="70675-213">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="70675-214">以下列程式碼取代 *Pages/Instructors/Index.cshtml.cs*：</span><span class="sxs-lookup"><span data-stu-id="70675-214">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,18-99)]

<span data-ttu-id="70675-215">`OnGetAsync` 方法會針對所選取講師的識別碼接受選擇性的路由資料。</span><span class="sxs-lookup"><span data-stu-id="70675-215">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="70675-216">檢查 *Pages/Instructors/Index.cshtml.cs* 檔案中的查詢：</span><span class="sxs-lookup"><span data-stu-id="70675-216">Examine the query in the *Pages/Instructors/Index.cshtml.cs* file:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="70675-217">查詢具有兩個 Include：</span><span class="sxs-lookup"><span data-stu-id="70675-217">The query has two includes:</span></span>

* <span data-ttu-id="70675-218">`OfficeAssignment`：顯示在 [Instructors 檢視](#IP)。</span><span class="sxs-lookup"><span data-stu-id="70675-218">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="70675-219">`CourseAssignments`：它顯示所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="70675-219">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="70675-220">更新 Instructors [索引] 頁面</span><span class="sxs-lookup"><span data-stu-id="70675-220">Update the instructors Index page</span></span>

<span data-ttu-id="70675-221">以下列標記更新 *Pages/Instructors/Index.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="70675-221">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="70675-222">上述標記會進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="70675-222">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="70675-223">將 `page` 指示詞從 `@page` 更新為 `@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="70675-223">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="70675-224">`"{id:int?}"` 是路由範本。</span><span class="sxs-lookup"><span data-stu-id="70675-224">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="70675-225">路由範本將 URL 中的整數查詢字串變更為路由資料。</span><span class="sxs-lookup"><span data-stu-id="70675-225">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="70675-226">例如，只有在 `@page` 指示詞產生如下的 URL 時，按一下講師的 [選取] 連結：</span><span class="sxs-lookup"><span data-stu-id="70675-226">For example, clicking on the **Select** link for an instructor with only the `@page` directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="70675-227">頁面指示詞是 `@page "{id:int?}"` 時，先前的 URL 為：</span><span class="sxs-lookup"><span data-stu-id="70675-227">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="70675-228">頁面標題是 **Instructors**。</span><span class="sxs-lookup"><span data-stu-id="70675-228">Page title is **Instructors**.</span></span>
* <span data-ttu-id="70675-229">新增 [辦公室] 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。</span><span class="sxs-lookup"><span data-stu-id="70675-229">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="70675-230">因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-230">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="70675-231">新增 課程 資料行，以顯示每位講師所教授課程。</span><span class="sxs-lookup"><span data-stu-id="70675-231">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="70675-232">如需此 Razor 語法的詳細資訊，請參閱[使用 `@:` 的明確行轉換](xref:mvc/views/razor#explicit-line-transition-with-)。</span><span class="sxs-lookup"><span data-stu-id="70675-232">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="70675-233">新增程式碼，將 `class="success"` 動態新增至所選取講師的 `tr` 項目。</span><span class="sxs-lookup"><span data-stu-id="70675-233">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="70675-234">這會使用啟動程序類別設定所選取資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="70675-234">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="70675-235">新增標示為**選取**的超連結。</span><span class="sxs-lookup"><span data-stu-id="70675-235">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="70675-236">此連結會將所選取的講師識別碼傳送至 `Index` 方法，並設定背景色彩。</span><span class="sxs-lookup"><span data-stu-id="70675-236">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="70675-237">執行應用程式並選取 [Instructors] 索引標籤。此頁面會顯示來自相關 `OfficeAssignment` 實體的 `Location` (辦公室)。</span><span class="sxs-lookup"><span data-stu-id="70675-237">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="70675-238">如果 OfficeAssignment 是 Null，就會顯示空的資料表資料格。</span><span class="sxs-lookup"><span data-stu-id="70675-238">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![Instructors [索引] 頁面未選取任何項目](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="70675-240">按一下**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="70675-240">Click on the **Select** link.</span></span> <span data-ttu-id="70675-241">資料列樣式變更。</span><span class="sxs-lookup"><span data-stu-id="70675-241">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="70675-242">新增選取的講師所教授的課程</span><span class="sxs-lookup"><span data-stu-id="70675-242">Add courses taught by selected instructor</span></span>

<span data-ttu-id="70675-243">以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中的 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="70675-243">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-999)]

<span data-ttu-id="70675-244">新增 `public int CourseID { get; set; }`</span><span class="sxs-lookup"><span data-stu-id="70675-244">Add `public int CourseID { get; set; }`</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_1&highlight=12)]

<span data-ttu-id="70675-245">檢查已更新的查詢：</span><span class="sxs-lookup"><span data-stu-id="70675-245">Examine the updated query:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="70675-246">上述查詢會新增 `Department` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-246">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="70675-247">下列程式碼會在已選取講師 (`id != null`) 時執行。</span><span class="sxs-lookup"><span data-stu-id="70675-247">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="70675-248">選取的講師會從檢視模型的講師清單中擷取。</span><span class="sxs-lookup"><span data-stu-id="70675-248">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="70675-249">檢視模型的 `Courses` 屬性則使用 `Course` 實體從該講師的 `CourseAssignments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="70675-249">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="70675-250">`Where` 方法會傳回集合。</span><span class="sxs-lookup"><span data-stu-id="70675-250">The `Where` method returns a collection.</span></span> <span data-ttu-id="70675-251">在上述 `Where` 方法中，只會傳回一個單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-251">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="70675-252">`Single` 方法會將集合轉換成單一 `Instructor` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-252">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="70675-253">`Instructor` 實體提供對 `CourseAssignments` 屬性的存取。</span><span class="sxs-lookup"><span data-stu-id="70675-253">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="70675-254">`CourseAssignments` 提供對相關 `Course` 實體的存取。</span><span class="sxs-lookup"><span data-stu-id="70675-254">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師對課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="70675-256">當集合只有一個項目時，將會在集合上使用 `Single` 方法。</span><span class="sxs-lookup"><span data-stu-id="70675-256">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="70675-257">如果集合是空的或是有多個項目，`Single` 方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="70675-257">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="70675-258">替代方式是 `SingleOrDefault`，它會在集合為空時傳回預設值 (在此情況下為 Null)。</span><span class="sxs-lookup"><span data-stu-id="70675-258">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="70675-259">在空集合上使用 `SingleOrDefault`：</span><span class="sxs-lookup"><span data-stu-id="70675-259">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="70675-260">造成例外狀況 (由於嘗試在 Null 參考上尋找 `Courses` 屬性)。</span><span class="sxs-lookup"><span data-stu-id="70675-260">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="70675-261">例外狀況訊息會不太清楚地指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="70675-261">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="70675-262">選取課程時，下列程式碼會填入檢視模型的 `Enrollments` 屬性：</span><span class="sxs-lookup"><span data-stu-id="70675-262">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="70675-263">將下列標記新增至 *Pages/Instructors/Index.cshtml* Razor 頁面的結尾：</span><span class="sxs-lookup"><span data-stu-id="70675-263">Add the following markup to the end of the *Pages/Instructors/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-999)]

<span data-ttu-id="70675-264">當選取講師時，上述標記會顯示與講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="70675-264">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="70675-265">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="70675-265">Test the app.</span></span> <span data-ttu-id="70675-266">按一下講師頁面上的**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="70675-266">Click on a **Select** link on the instructors page.</span></span>

![Instructors [索引] 頁面已選取講師](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="70675-268">顯示學生資料</span><span class="sxs-lookup"><span data-stu-id="70675-268">Show student data</span></span>

<span data-ttu-id="70675-269">在本節中，應用程式會更新以顯示所選取課程的學生資料。</span><span class="sxs-lookup"><span data-stu-id="70675-269">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="70675-270">以下列程式碼更新 *Pages/Instructors/Index.cshtml.cs* 中 `OnGetAsync` 方法的查詢：</span><span class="sxs-lookup"><span data-stu-id="70675-270">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="70675-271">更新 *Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="70675-271">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="70675-272">將下列標記新增至檔案結尾：</span><span class="sxs-lookup"><span data-stu-id="70675-272">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="70675-273">上述標記會顯示註冊所選取課程的學生清單。</span><span class="sxs-lookup"><span data-stu-id="70675-273">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="70675-274">重新整理頁面，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="70675-274">Refresh the page and select an instructor.</span></span> <span data-ttu-id="70675-275">選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="70675-275">Select a course to see the list of enrolled students and their grades.</span></span>

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="70675-277">使用 Single</span><span class="sxs-lookup"><span data-stu-id="70675-277">Using Single</span></span>

<span data-ttu-id="70675-278">`Single` 方法可以傳入 `Where` 條件，而不是個別呼叫 `Where` 方法：</span><span class="sxs-lookup"><span data-stu-id="70675-278">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21-22,30-31)]

<span data-ttu-id="70675-279">比起使用 `Where`，上述 `Single` 方法並沒有任何優勢。</span><span class="sxs-lookup"><span data-stu-id="70675-279">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="70675-280">某些開發人員偏好使用 `Single` 方法樣式。</span><span class="sxs-lookup"><span data-stu-id="70675-280">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="70675-281">明確式載入</span><span class="sxs-lookup"><span data-stu-id="70675-281">Explicit loading</span></span>

<span data-ttu-id="70675-282">目前程式碼針對 `Enrollments` 和 `Students` 指定積極式載入：</span><span class="sxs-lookup"><span data-stu-id="70675-282">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="70675-283">假設使用者很少會想要查看課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="70675-283">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="70675-284">在此情況下，最佳方法就是在要求時，只載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="70675-284">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="70675-285">在本節中，`OnGetAsync` 更新為針對 `Enrollments` 和 `Students` 使用明確式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-285">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="70675-286">使用下列程式碼更新 `OnGetAsync`：</span><span class="sxs-lookup"><span data-stu-id="70675-286">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="70675-287">上述程式碼會捨棄註冊和學生資料的 *ThenInclude* 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="70675-287">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="70675-288">如果選取了課程，醒目提示的程式碼就會擷取：</span><span class="sxs-lookup"><span data-stu-id="70675-288">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="70675-289">所選取課程的 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-289">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="70675-290">每個 `Enrollment` 的 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="70675-290">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="70675-291">請注意，上述程式碼會將 `.AsNoTracking()` 註解化。</span><span class="sxs-lookup"><span data-stu-id="70675-291">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="70675-292">針對所追蹤的實體，導覽屬性只能進行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="70675-292">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="70675-293">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="70675-293">Test the app.</span></span> <span data-ttu-id="70675-294">就使用者的觀點而言，應用程式行為與之前的版本相同。</span><span class="sxs-lookup"><span data-stu-id="70675-294">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="70675-295">下一個教學課程會示範如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="70675-295">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="70675-296">[上一頁](xref:data/ef-rp/complex-data-model)
>[下一頁](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="70675-296">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
