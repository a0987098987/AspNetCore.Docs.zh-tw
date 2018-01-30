---
title: "使用 EF Core-razor 頁面讀取相關的資料-6，8 個"
author: rick-anderson
description: "在此教學課程中您可以讀取並顯示相關的資料--也就是，可將 Entity Framework 載入導覽屬性的資料。"
manager: wpickett
ms.author: riande
ms.date: 11/05/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/read-related-data
ms.openlocfilehash: ccb1e95ae2b43fd0a4c4b1ac9ed58a4d474ab3b6
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="reading-related-data---ef-core-with-razor-pages-6-of-8"></a><span data-ttu-id="f929d-103">讀取的相關資料-Razor 頁面 (8 個 6) 使用的 EF 核心</span><span class="sxs-lookup"><span data-stu-id="f929d-103">Reading related data - EF Core with Razor Pages (6 of 8)</span></span>

<span data-ttu-id="f929d-104">由[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f929d-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="f929d-105">在本教學課程中，會讀取及顯示相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-105">In this tutorial, related data is read and displayed.</span></span> <span data-ttu-id="f929d-106">EF 核心載入導覽屬性的資料相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-106">Related data is data that EF Core loads into navigation properties.</span></span>

<span data-ttu-id="f929d-107">如果您遇到無法解決的問題時，請下載[已完成的應用程式，為此階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related)。</span><span class="sxs-lookup"><span data-stu-id="f929d-107">If you run into problems you can't solve, download the [completed app for this stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots/cu-part6-related).</span></span>

<span data-ttu-id="f929d-108">下圖顯示已完成的頁面在此教學課程：</span><span class="sxs-lookup"><span data-stu-id="f929d-108">The following illustrations show the completed pages for this tutorial:</span></span>

![課程索引頁](read-related-data/_static/courses-index.png)

![講師索引頁](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="f929d-111">急切、 明確，和消極式載入之相關資料</span><span class="sxs-lookup"><span data-stu-id="f929d-111">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="f929d-112">有幾種 EF 核心可以將實體的導覽屬性載入相關的資料：</span><span class="sxs-lookup"><span data-stu-id="f929d-112">There are several ways that EF Core can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="f929d-113">[積極式載入](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading)。</span><span class="sxs-lookup"><span data-stu-id="f929d-113">[Eager loading](https://docs.microsoft.com/ef/core/querying/related-data#eager-loading).</span></span> <span data-ttu-id="f929d-114">積極式載入時，一種實體類型的查詢也會載入相關的實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-114">Eager loading is when a query for one type of entity also loads related entities.</span></span> <span data-ttu-id="f929d-115">實體讀取時，會擷取其相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-115">When the entity is read, its related data is retrieved.</span></span> <span data-ttu-id="f929d-116">這通常會導致單一聯結查詢以擷取所有所需的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="f929d-117">EF 核心會發出多個查詢，針對某些類型的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-117">EF Core will issue multiple queries for some types of eager loading.</span></span> <span data-ttu-id="f929d-118">發行多個查詢的內容可能會更有效率，比起 EF6 中的某些查詢的情況在先前存在的單一查詢。</span><span class="sxs-lookup"><span data-stu-id="f929d-118">Issuing multiple queries can be more efficient than was the case for some queries in EF6 where there was a single query.</span></span> <span data-ttu-id="f929d-119">積極式載入指定`Include`和`ThenInclude`方法。</span><span class="sxs-lookup"><span data-stu-id="f929d-119">Eager loading is specified with the `Include` and `ThenInclude` methods.</span></span>

 ![積極式載入範例](read-related-data/_static/eager-loading.png)
 
 <span data-ttu-id="f929d-121">積極式載入集合 nvavigation 包含在內時，會傳送多個查詢：</span><span class="sxs-lookup"><span data-stu-id="f929d-121">Eager loading sends multiple queries when a collection nvavigation is included:</span></span>

 * <span data-ttu-id="f929d-122">針對主查詢的一個查詢</span><span class="sxs-lookup"><span data-stu-id="f929d-122">One query for the main query</span></span> 
 * <span data-ttu-id="f929d-123">每個集合負載樹狀目錄中的 「 邊緣 」 的一個查詢。</span><span class="sxs-lookup"><span data-stu-id="f929d-123">One query for each collection "edge" in the load tree.</span></span>

* <span data-ttu-id="f929d-124">個別查詢`Load`： 可以在個別的查詢，擷取的資料和 EF 核心 「 修復 」 的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-124">Separate queries with `Load`: The data can be retrieved in separate queries, and EF Core "fixes up" the navigation properties.</span></span> <span data-ttu-id="f929d-125">「 向上修正 」 表示 EF 核心會自動填入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-125">"fixes up" means that EF Core automatically populates the navigation properties.</span></span> <span data-ttu-id="f929d-126">個別查詢`Load`則更像 explict 載入比積極式載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-126">Separate queries with `Load` is more like explict loading than eager loading.</span></span>

 ![個別查詢範例](read-related-data/_static/separate-queries.png)

 <span data-ttu-id="f929d-128">注意： EF 核心自動修復與先前已載入至內容執行個體的任何其他實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-128">Note: EF Core automatically fixes up navigation properties to any other entities that were previously loaded into the context instance.</span></span> <span data-ttu-id="f929d-129">即使瀏覽屬性資料是*不*明確包含，屬性可能仍然會填入某些或所有相關的項目先前已載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-129">Even if the data for a navigation property is *not* explicitly included, the property may still be populated if some or all of the related entities were previously loaded.</span></span>

* <span data-ttu-id="f929d-130">[明確式載入](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading)。</span><span class="sxs-lookup"><span data-stu-id="f929d-130">[Explicit loading](https://docs.microsoft.com/ef/core/querying/related-data#explicit-loading).</span></span> <span data-ttu-id="f929d-131">第一次讀取實體時，不被擷取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-131">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="f929d-132">必須撰寫程式碼會在需要時擷取的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-132">Code must be written to retrieve the related data when it's needed.</span></span> <span data-ttu-id="f929d-133">明確式載入個別的查詢結果傳送至資料庫的多個查詢。</span><span class="sxs-lookup"><span data-stu-id="f929d-133">Explicit loading with separate queries results in multiple queries sent to the DB.</span></span> <span data-ttu-id="f929d-134">明確式載入，與程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-134">With explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="f929d-135">使用`Load`方法以明確式載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-135">Use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="f929d-136">例如: </span><span class="sxs-lookup"><span data-stu-id="f929d-136">For example:</span></span>

 ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="f929d-138">[消極式載入](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading)。</span><span class="sxs-lookup"><span data-stu-id="f929d-138">[Lazy loading](https://docs.microsoft.com/ef/core/querying/related-data#lazy-loading).</span></span> <span data-ttu-id="f929d-139">[EF 核心目前不支援延遲載入](https://github.com/aspnet/EntityFrameworkCore/issues/3797)。</span><span class="sxs-lookup"><span data-stu-id="f929d-139">[EF Core doesn't currently support lazy loading](https://github.com/aspnet/EntityFrameworkCore/issues/3797).</span></span> <span data-ttu-id="f929d-140">第一次讀取實體時，不被擷取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-140">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="f929d-141">第一次存取導覽屬性時，自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-141">The first time a navigation property is accessed, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="f929d-142">查詢會傳送至資料庫，每次存取導覽屬性第一次。</span><span class="sxs-lookup"><span data-stu-id="f929d-142">A query is sent to the DB each time a navigation property is accessed for the first time.</span></span>

* <span data-ttu-id="f929d-143">`Select`運算子載入只有所需的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-143">The `Select` operator loads only the related data needed.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="f929d-144">建立可顯示部門名稱的課程頁面</span><span class="sxs-lookup"><span data-stu-id="f929d-144">Create a Courses page that displays department name</span></span>

<span data-ttu-id="f929d-145">課程實體包含導覽屬性，其中包含`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-145">The Course entity includes a navigation property that contains the `Department` entity.</span></span> <span data-ttu-id="f929d-146">`Department`實體包含課程指派給的部門。</span><span class="sxs-lookup"><span data-stu-id="f929d-146">The `Department` entity contains the department that the course is assigned to.</span></span>

<span data-ttu-id="f929d-147">若要指派的部門名稱在清單中顯示的課程：</span><span class="sxs-lookup"><span data-stu-id="f929d-147">To display the name of the assigned department in a list of courses:</span></span>

* <span data-ttu-id="f929d-148">取得`Name`屬性從`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-148">Get the `Name` property from the `Department` entity.</span></span>
* <span data-ttu-id="f929d-149">`Department`實體來自`Course.Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-149">The `Department` entity comes from the `Course.Department` navigation property.</span></span>

![ourse.Department](read-related-data/_static/dep-crs.png)

<a name="scaffold"></a>
### <a name="scaffold-the-course-model"></a><span data-ttu-id="f929d-151">Scaffold 課程模型</span><span class="sxs-lookup"><span data-stu-id="f929d-151">Scaffold the Course model</span></span>

* <span data-ttu-id="f929d-152">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f929d-152">Exit Visual Studio.</span></span>
* <span data-ttu-id="f929d-153">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f929d-153">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f929d-154">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f929d-154">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Course -dc SchoolContext -udl -outDir Pages\Courses --referenceScriptLibraries
 ```

<span data-ttu-id="f929d-155">上述命令 scaffold`Course`模型。</span><span class="sxs-lookup"><span data-stu-id="f929d-155">The preceding command scaffolds the `Course` model.</span></span> <span data-ttu-id="f929d-156">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="f929d-156">Open the project in Visual Studio.</span></span>

<span data-ttu-id="f929d-157">建置專案。</span><span class="sxs-lookup"><span data-stu-id="f929d-157">Build the project.</span></span> <span data-ttu-id="f929d-158">建置會產生類似如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="f929d-158">The build generates errors like the following:</span></span>

`1>Pages/Courses/Index.cshtml.cs(26,37,26,43): error CS1061: 'SchoolContext' does not
 contain a definition for 'Course' and no extension method 'Course' accepting a first
 argument of type 'SchoolContext' could be found (are you missing a using directive or
 an assembly reference?)`

 <span data-ttu-id="f929d-159">全域變更`_context.Course`至`_context.Courses`(亦即，加入"s" `Course`)。</span><span class="sxs-lookup"><span data-stu-id="f929d-159">Globally change `_context.Course` to `_context.Courses` (that is, add an "s" to `Course`).</span></span> <span data-ttu-id="f929d-160">找到項目 7 及更新。</span><span class="sxs-lookup"><span data-stu-id="f929d-160">7 occurrences are found and updated.</span></span>

<span data-ttu-id="f929d-161">開啟*Pages/Courses/Index.cshtml.cs*並檢查`OnGetAsync`方法。</span><span class="sxs-lookup"><span data-stu-id="f929d-161">Open *Pages/Courses/Index.cshtml.cs* and examine the `OnGetAsync` method.</span></span> <span data-ttu-id="f929d-162">Scaffolding 引擎所指定的積極式載入`Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-162">The scaffolding engine specified eager loading for the `Department` navigation property.</span></span> <span data-ttu-id="f929d-163">`Include`方法指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-163">The `Include` method specifies eager loading.</span></span>

<span data-ttu-id="f929d-164">執行應用程式並選取**課程**連結。</span><span class="sxs-lookup"><span data-stu-id="f929d-164">Run the app and select the **Courses** link.</span></span> <span data-ttu-id="f929d-165">Department 資料行顯示`DepartmentID`，不會很有用。</span><span class="sxs-lookup"><span data-stu-id="f929d-165">The department column displays the `DepartmentID`, which isn't useful.</span></span>

<span data-ttu-id="f929d-166">以下列程式碼更新 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="f929d-166">Update the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="f929d-167">上述程式碼會加入`AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="f929d-167">The preceding code adds `AsNoTracking`.</span></span> <span data-ttu-id="f929d-168">`AsNoTracking`改善效能，因為傳回的實體不會受到追蹤。</span><span class="sxs-lookup"><span data-stu-id="f929d-168">`AsNoTracking` improves performance because the entities returned are not tracked.</span></span> <span data-ttu-id="f929d-169">因為它們不在目前的內容更新時，不會追蹤實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-169">The entities are not tracked because they're not updated in the current context.</span></span>

<span data-ttu-id="f929d-170">更新*Views/Courses/Index.cshtml*以反白顯示下列標記：</span><span class="sxs-lookup"><span data-stu-id="f929d-170">Update *Views/Courses/Index.cshtml* with the following highlighted markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="f929d-171">已 scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="f929d-171">The following changes have been made to the scaffolded code:</span></span>

* <span data-ttu-id="f929d-172">從索引的標題變更為 Courses 中。</span><span class="sxs-lookup"><span data-stu-id="f929d-172">Changed the heading from Index to Courses.</span></span>
* <span data-ttu-id="f929d-173">加入**數目**顯示的資料行`CourseID`屬性值。</span><span class="sxs-lookup"><span data-stu-id="f929d-173">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="f929d-174">根據預設，主索引鍵不是建立結構，因為它們通常是無意義的使用者。</span><span class="sxs-lookup"><span data-stu-id="f929d-174">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="f929d-175">不過，在此情況下的主索引鍵是有意義。</span><span class="sxs-lookup"><span data-stu-id="f929d-175">However, in this case the primary key is meaningful.</span></span>
* <span data-ttu-id="f929d-176">變更**部門**資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="f929d-176">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="f929d-177">程式碼會顯示`Name`屬性`Department`實體載入到`Department`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="f929d-177">The code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="f929d-178">執行應用程式並選取**課程**索引標籤，查看與部門名稱清單。</span><span class="sxs-lookup"><span data-stu-id="f929d-178">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![課程索引頁](read-related-data/_static/courses-index.png)

<a name="select"></a>
### <a name="loading-related-data-with-select"></a><span data-ttu-id="f929d-180">正在載入相關的資料與 Select</span><span class="sxs-lookup"><span data-stu-id="f929d-180">Loading related data with Select</span></span>

<span data-ttu-id="f929d-181">`OnGetAsync`方法會載入相關的資料與`Include`方法：</span><span class="sxs-lookup"><span data-stu-id="f929d-181">The `OnGetAsync` method loads related data with the `Include` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/Index.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="f929d-182">`Select`運算子載入只有所需的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-182">The `Select` operator loads only the related data needed.</span></span> <span data-ttu-id="f929d-183">針對單一項目，例如`Department.Name`它會使用 SQL INNER JOIN。</span><span class="sxs-lookup"><span data-stu-id="f929d-183">For single items, like the `Department.Name` it uses a SQL INNER JOIN.</span></span> <span data-ttu-id="f929d-184">集合的方法，它可以使用另一個資料庫存取權，但會跟著移動。`Include`</span><span class="sxs-lookup"><span data-stu-id="f929d-184">For collections it uses another database access, but so does the .`Include`</span></span> <span data-ttu-id="f929d-185">在集合上的運算子。</span><span class="sxs-lookup"><span data-stu-id="f929d-185">operator on collections.</span></span>

<span data-ttu-id="f929d-186">下列程式碼會載入相關的資料與`Select`方法：</span><span class="sxs-lookup"><span data-stu-id="f929d-186">The following code loads related data with the `Select` method:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs?name=snippet_RevisedIndexMethod&highlight=4)]

<span data-ttu-id="f929d-187">`CourseViewModel`:</span><span class="sxs-lookup"><span data-stu-id="f929d-187">The `CourseViewModel`:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/CourseViewModel.cs?name=snippet)]

<span data-ttu-id="f929d-188">請參閱[IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml)和[IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs)完成完整的範例。</span><span class="sxs-lookup"><span data-stu-id="f929d-188">See [IndexSelect.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml) and [IndexSelect.cshtml.cs](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Courses/IndexSelect.cshtml.cs) for a complete example.</span></span>

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="f929d-189">建立講師頁面，其中顯示 Courses 以及註冊項目</span><span class="sxs-lookup"><span data-stu-id="f929d-189">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="f929d-190">在本節中，會建立講師頁面。</span><span class="sxs-lookup"><span data-stu-id="f929d-190">In this section, the Instructors page is created.</span></span>

<a name="IP"></a>
<span data-ttu-id="f929d-191">![講師索引頁](read-related-data/_static/instructors-index.png)</span><span class="sxs-lookup"><span data-stu-id="f929d-191">![Instructors Index page](read-related-data/_static/instructors-index.png)</span></span>

<span data-ttu-id="f929d-192">此頁面讀取，並會顯示相關的資料以下列方式：</span><span class="sxs-lookup"><span data-stu-id="f929d-192">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="f929d-193">講師清單會顯示相關的資料`OfficeAssignment`實體 (Office 的上述影像中)。</span><span class="sxs-lookup"><span data-stu-id="f929d-193">The list of instructors displays related data from the `OfficeAssignment` entity (Office in the preceding image).</span></span> <span data-ttu-id="f929d-194">`Instructor`和`OfficeAssignment`實體是以 0 或-1 一個關聯性中。</span><span class="sxs-lookup"><span data-stu-id="f929d-194">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="f929d-195">積極式載入用於`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-195">Eager loading is used for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="f929d-196">需要顯示相關的資料時，通常更有效率積極式載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-196">Eager loading is typically more efficient when the related data needs to be displayed.</span></span> <span data-ttu-id="f929d-197">在此情況下，會顯示對講師 office 指派。</span><span class="sxs-lookup"><span data-stu-id="f929d-197">In this case, office assignments for the instructors are displayed.</span></span>
* <span data-ttu-id="f929d-198">當使用者選取講師 (Harui 的上述影像中) 與相關`Course`實體便會顯示。</span><span class="sxs-lookup"><span data-stu-id="f929d-198">When the user selects an instructor (Harui in the preceding image), related `Course` entities are displayed.</span></span> <span data-ttu-id="f929d-199">`Instructor`和`Course`實體是位於多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f929d-199">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="f929d-200">Eager 載入`Course`實體和其相關`Department`使用實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-200">Eager loading for the `Course` entities and their related `Department` entities is used.</span></span> <span data-ttu-id="f929d-201">在此情況下，個別查詢可能會更有效率因為只選取講師課程所需。</span><span class="sxs-lookup"><span data-stu-id="f929d-201">In this case, separate queries might be more efficient because only courses for the selected instructor are needed.</span></span> <span data-ttu-id="f929d-202">這個範例示範如何使用中實體的導覽屬性中的導覽屬性的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="f929d-202">This example shows how to use eager loading for navigation properties in entities that are in navigation properties.</span></span>
* <span data-ttu-id="f929d-203">當使用者選取課程 （化學的上述影像中） 的相關資料從`Enrollments`顯示實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-203">When the user selects a course (Chemistry in the preceding image), related data from the `Enrollments` entity is displayed.</span></span> <span data-ttu-id="f929d-204">在上述映像中，會顯示學生名稱以及等級。</span><span class="sxs-lookup"><span data-stu-id="f929d-204">In the preceding image, student name and grade are displayed.</span></span> <span data-ttu-id="f929d-205">`Course`和`Enrollment`實體是位於一個對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="f929d-205">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="f929d-206">建立講師索引檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="f929d-206">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="f929d-207">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-207">The instructors page shows data from three different tables.</span></span> <span data-ttu-id="f929d-208">檢視模型就會建立包含三個實體代表三個資料表。</span><span class="sxs-lookup"><span data-stu-id="f929d-208">A view model is created that includes the three entities representing the three tables.</span></span>

<span data-ttu-id="f929d-209">在*SchoolViewModels*資料夾中，建立*InstructorIndexData.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f929d-209">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="scaffold-the-instructor-model"></a><span data-ttu-id="f929d-210">Scaffold 講師模型</span><span class="sxs-lookup"><span data-stu-id="f929d-210">Scaffold the Instructor model</span></span>

* <span data-ttu-id="f929d-211">結束 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="f929d-211">Exit Visual Studio.</span></span>
* <span data-ttu-id="f929d-212">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="f929d-212">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="f929d-213">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f929d-213">Run the following command:</span></span>

 ```console
dotnet aspnet-codegenerator razorpage -m Instructor -dc SchoolContext -udl -outDir Pages\Instructors --referenceScriptLibraries
 ```

<span data-ttu-id="f929d-214">上述命令 scaffold`Instructor`模型。</span><span class="sxs-lookup"><span data-stu-id="f929d-214">The preceding command scaffolds the `Instructor` model.</span></span> <span data-ttu-id="f929d-215">在 Visual Studio 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="f929d-215">Open the project in Visual Studio.</span></span>

<span data-ttu-id="f929d-216">建置專案。</span><span class="sxs-lookup"><span data-stu-id="f929d-216">Build the project.</span></span> <span data-ttu-id="f929d-217">建置會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="f929d-217">The build generates errors.</span></span>

<span data-ttu-id="f929d-218">全域變更`_context.Instructor`至`_context.Instructors`(亦即，加入"s" `Instructor`)。</span><span class="sxs-lookup"><span data-stu-id="f929d-218">Globally change `_context.Instructor` to `_context.Instructors` (that is, add an "s" to `Instructor`).</span></span> <span data-ttu-id="f929d-219">找到項目 7 及更新。</span><span class="sxs-lookup"><span data-stu-id="f929d-219">7 occurrences are found and updated.</span></span>

<span data-ttu-id="f929d-220">執行應用程式，並瀏覽至講師頁面。</span><span class="sxs-lookup"><span data-stu-id="f929d-220">Run the app and navigate to the instructors page.</span></span>

<span data-ttu-id="f929d-221">取代*Pages/Instructors/Index.cshtml.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f929d-221">Replace *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_all&highlight=2,20-)]

<span data-ttu-id="f929d-222">`OnGetAsync`方法會接受選擇性的路由資料選取講師的識別碼。</span><span class="sxs-lookup"><span data-stu-id="f929d-222">The `OnGetAsync` method accepts optional route data for the ID of the selected instructor.</span></span>

<span data-ttu-id="f929d-223">檢查查詢上*Pages/Instructors/Index.cshtml*頁面：</span><span class="sxs-lookup"><span data-stu-id="f929d-223">Examine the query on the *Pages/Instructors/Index.cshtml* page:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index1.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="f929d-224">查詢具有兩個包含：</span><span class="sxs-lookup"><span data-stu-id="f929d-224">The query has two includes:</span></span>

* <span data-ttu-id="f929d-225">`OfficeAssignment`： 顯示在[講師檢視](#IP)。</span><span class="sxs-lookup"><span data-stu-id="f929d-225">`OfficeAssignment`: Displayed in the [instructors view](#IP).</span></span>
* <span data-ttu-id="f929d-226">`CourseAssignments`： 它會教課程。</span><span class="sxs-lookup"><span data-stu-id="f929d-226">`CourseAssignments`: Which brings in the courses taught.</span></span>


### <a name="update-the-instructors-index-page"></a><span data-ttu-id="f929d-227">更新講師索引頁</span><span class="sxs-lookup"><span data-stu-id="f929d-227">Update the instructors Index page</span></span>

<span data-ttu-id="f929d-228">更新*Pages/Instructors/Index.cshtml*以下列標記：</span><span class="sxs-lookup"><span data-stu-id="f929d-228">Update *Pages/Instructors/Index.cshtml* with the following markup:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=1-65&highlight=1,5,8,16-21,25-32,43-57)]

<span data-ttu-id="f929d-229">上述標記進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="f929d-229">The preceding markup makes the following changes:</span></span>

* <span data-ttu-id="f929d-230">更新`page`指示詞從`@page`至`@page "{id:int?}"`。</span><span class="sxs-lookup"><span data-stu-id="f929d-230">Updates the `page` directive from `@page` to `@page "{id:int?}"`.</span></span> <span data-ttu-id="f929d-231">`"{id:int?}"`為路由範本。</span><span class="sxs-lookup"><span data-stu-id="f929d-231">`"{id:int?}"` is a route template.</span></span> <span data-ttu-id="f929d-232">路由範本變更路由資料的整數在 URL 中的查詢字串。</span><span class="sxs-lookup"><span data-stu-id="f929d-232">The route template changes integer query strings in the URL to route data.</span></span> <span data-ttu-id="f929d-233">例如，按一下**選取**講師頁面指示詞會產生類似下列的 URL 的連結：</span><span class="sxs-lookup"><span data-stu-id="f929d-233">For example, clicking on the **Select** link for an instructor when the page directive produces a URL like the following:</span></span>

    `http://localhost:1234/Instructors?id=2`

    <span data-ttu-id="f929d-234">頁面指示詞時`@page "{id:int?}"`，先前的 URL 是：</span><span class="sxs-lookup"><span data-stu-id="f929d-234">When the page directive is `@page "{id:int?}"`, the previous URL is:</span></span>

    `http://localhost:1234/Instructors/2`

* <span data-ttu-id="f929d-235">頁面標題**講師**。</span><span class="sxs-lookup"><span data-stu-id="f929d-235">Page title is **Instructors**.</span></span>
* <span data-ttu-id="f929d-236">加入**Office**顯示之資料行`item.OfficeAssignment.Location`才`item.OfficeAssignment`不是 null。</span><span class="sxs-lookup"><span data-stu-id="f929d-236">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="f929d-237">因為這是以 0 或-1 一個關聯性時，有可能不相關的 OfficeAssignment 實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-237">Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="f929d-238">加入**課程**資料行，顯示課程教導每個講師所。</span><span class="sxs-lookup"><span data-stu-id="f929d-238">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="f929d-239">請參閱[使用明確列轉換`@:`](xref:mvc/views/razor#explicit-line-transition-with-)如需有關此 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="f929d-239">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="f929d-240">加入的程式碼動態新增`class="success"`至`tr`選取講師項目。</span><span class="sxs-lookup"><span data-stu-id="f929d-240">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="f929d-241">這會設定使用啟動程序的類別所選取的資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="f929d-241">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.CourseID == Model.CourseID)
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="f929d-242">加入新的超連結標示為**選取**。</span><span class="sxs-lookup"><span data-stu-id="f929d-242">Added a new hyperlink labeled **Select**.</span></span> <span data-ttu-id="f929d-243">此連結傳送所選的講師識別碼`Index`方法，並設定背景色彩。</span><span class="sxs-lookup"><span data-stu-id="f929d-243">This link sends the selected instructor's ID to the `Index` method and sets a background color.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="f929d-244">執行應用程式並選取**講師** 索引標籤。此頁面會顯示`Location`(office) 從相關`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-244">Run the app and select the **Instructors** tab. The page displays the `Location` (office) from the related `OfficeAssignment` entity.</span></span> <span data-ttu-id="f929d-245">如果 OfficeAssignment' 是空的資料表資料格會顯示 null。</span><span class="sxs-lookup"><span data-stu-id="f929d-245">If OfficeAssignment\` is null, an empty table cell is displayed.</span></span>

![選取任何項目講師索引頁](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="f929d-247">按一下**選取**連結。</span><span class="sxs-lookup"><span data-stu-id="f929d-247">Click on the **Select** link.</span></span> <span data-ttu-id="f929d-248">資料列的樣式變更。</span><span class="sxs-lookup"><span data-stu-id="f929d-248">The row style changes.</span></span>

### <a name="add-courses-taught-by-selected-instructor"></a><span data-ttu-id="f929d-249">加入所選講師教導課程</span><span class="sxs-lookup"><span data-stu-id="f929d-249">Add courses taught by selected instructor</span></span>

<span data-ttu-id="f929d-250">更新`OnGetAsync`方法中的*Pages/Instructors/Index.cshtml.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f929d-250">Update the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_OnGetAsync&highlight=1,8,16-)]

<span data-ttu-id="f929d-251">檢查有更新的查詢：</span><span class="sxs-lookup"><span data-stu-id="f929d-251">Examine the updated query:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ThenInclude)]

<span data-ttu-id="f929d-252">上述查詢會將加入`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-252">The preceding query adds the `Department` entities.</span></span>

<span data-ttu-id="f929d-253">下列程式碼執行時已選取 instructor (`id != null`)。</span><span class="sxs-lookup"><span data-stu-id="f929d-253">The following code executes when an instructor is selected (`id != null`).</span></span> <span data-ttu-id="f929d-254">選取的講師會擷取從講師中檢視模型的清單。</span><span class="sxs-lookup"><span data-stu-id="f929d-254">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="f929d-255">檢視模型`Courses`屬性載入`Course`實體從該講師`CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-255">The view model's `Courses` property is loaded with the `Course` entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_ID)]

<span data-ttu-id="f929d-256">`Where`方法傳回的集合。</span><span class="sxs-lookup"><span data-stu-id="f929d-256">The `Where` method returns a collection.</span></span> <span data-ttu-id="f929d-257">在上述`Where`方法，只有一個`Instructor`傳回實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-257">In the preceding `Where` method, only a single `Instructor` entity is returned.</span></span> <span data-ttu-id="f929d-258">`Single`方法會將集合轉換成單一`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-258">The `Single` method converts the collection into a single `Instructor` entity.</span></span> <span data-ttu-id="f929d-259">`Instructor`實體提供存取`CourseAssignments`屬性。</span><span class="sxs-lookup"><span data-stu-id="f929d-259">The `Instructor` entity provides access to the `CourseAssignments` property.</span></span> <span data-ttu-id="f929d-260">`CourseAssignments`提供存取相關的`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-260">`CourseAssignments` provides access to the related `Course` entities.</span></span>

![講師-課程 m:M](complex-data-model/_static/courseassignment.png)

<span data-ttu-id="f929d-262">`Single`集合具有一個項目時，將會使用在集合上的方法。</span><span class="sxs-lookup"><span data-stu-id="f929d-262">The `Single` method is used on a collection when the collection has only one item.</span></span> <span data-ttu-id="f929d-263">`Single`方法擲回例外狀況，如果集合是空的或若有多個項目。</span><span class="sxs-lookup"><span data-stu-id="f929d-263">The `Single` method throws an exception if the collection is empty or if there's more than one item.</span></span> <span data-ttu-id="f929d-264">替代方式是`SingleOrDefault`，傳回的預設值 (null 在此情況下) 如果集合是空的。</span><span class="sxs-lookup"><span data-stu-id="f929d-264">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="f929d-265">使用`SingleOrDefault`空集合上：</span><span class="sxs-lookup"><span data-stu-id="f929d-265">Using `SingleOrDefault` on an empty collection:</span></span>

* <span data-ttu-id="f929d-266">造成例外狀況 (從嘗試尋找`Courses`對 null 參考的屬性)。</span><span class="sxs-lookup"><span data-stu-id="f929d-266">Results in an exception (from trying to find a `Courses` property on a null reference).</span></span>
* <span data-ttu-id="f929d-267">例外狀況訊息會較不清楚指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="f929d-267">The exception message would less clearly indicate the cause of the problem.</span></span>

<span data-ttu-id="f929d-268">下列程式碼會填入檢視模型`Enrollments`屬性選取課程時：</span><span class="sxs-lookup"><span data-stu-id="f929d-268">The following code populates the view model's `Enrollments` property when a course is selected:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index2.cshtml.cs?name=snippet_courseID)]

<span data-ttu-id="f929d-269">加入下列標記的結尾*Pages/Courses/Index.cshtml* Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="f929d-269">Add the following markup to the end of the *Pages/Courses/Index.cshtml* Razor Page:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=60-102&highlight=7-)]

<span data-ttu-id="f929d-270">上述的標記會顯示一份時選取講師講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="f929d-270">The preceding markup displays a list of courses related to an instructor when an instructor is selected.</span></span>

<span data-ttu-id="f929d-271">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f929d-271">Test the app.</span></span> <span data-ttu-id="f929d-272">按一下**選取**講師頁面上的連結。</span><span class="sxs-lookup"><span data-stu-id="f929d-272">Click on a **Select** link on the instructors page.</span></span>

![選取講師索引頁面講師](read-related-data/_static/instructors-index-instructor-selected.png)

### <a name="show-student-data"></a><span data-ttu-id="f929d-274">顯示學生資料</span><span class="sxs-lookup"><span data-stu-id="f929d-274">Show student data</span></span>

<span data-ttu-id="f929d-275">在本節中，應用程式會更新以顯示所選課程的學生資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-275">In this section, the app is updated to show the student data for a selected course.</span></span>

<span data-ttu-id="f929d-276">更新中的查詢`OnGetAsync`方法中的*Pages/Instructors/Index.cshtml.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f929d-276">Update the query in the `OnGetAsync` method in *Pages/Instructors/Index.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="f929d-277">更新*Pages/Instructors/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="f929d-277">Update *Pages/Instructors/Index.cshtml*.</span></span> <span data-ttu-id="f929d-278">將下列標記加入至檔案結尾：</span><span class="sxs-lookup"><span data-stu-id="f929d-278">Add the following markup to the end of the file:</span></span>

[!code-html[](intro/samples/cu/Pages/Instructors/IndexRRD.cshtml?range=103-)]

<span data-ttu-id="f929d-279">上述的標記會顯示一份註冊的學生在選取的期間。</span><span class="sxs-lookup"><span data-stu-id="f929d-279">The preceding markup displays a list of the students who are enrolled in the selected course.</span></span>

<span data-ttu-id="f929d-280">重新整理頁面，然後選取 instructor。</span><span class="sxs-lookup"><span data-stu-id="f929d-280">Refresh the page and select an instructor.</span></span> <span data-ttu-id="f929d-281">選取的課程，若要查看已註冊的學生版和其等級清單。</span><span class="sxs-lookup"><span data-stu-id="f929d-281">Select a course to see the list of enrolled students and their grades.</span></span>

![講師索引頁面講師和選取的課程](read-related-data/_static/instructors-index.png)

## <a name="using-single"></a><span data-ttu-id="f929d-283">使用單一</span><span class="sxs-lookup"><span data-stu-id="f929d-283">Using Single</span></span>

<span data-ttu-id="f929d-284">`Single`方法可以傳入`Where`條件，而不是呼叫`Where`方法分別：</span><span class="sxs-lookup"><span data-stu-id="f929d-284">The `Single` method can pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexSingle.cshtml.cs?name=snippet_single&highlight=21,28-29)]

<span data-ttu-id="f929d-285">上述`Single`方法提供任何好處，透過使用`Where`。</span><span class="sxs-lookup"><span data-stu-id="f929d-285">The preceding `Single` approach provides no benefits over using `Where`.</span></span> <span data-ttu-id="f929d-286">某些開發人員偏好使用`Single`接近樣式。</span><span class="sxs-lookup"><span data-stu-id="f929d-286">Some developers prefer the `Single` approach style.</span></span>

## <a name="explicit-loading"></a><span data-ttu-id="f929d-287">明確式載入</span><span class="sxs-lookup"><span data-stu-id="f929d-287">Explicit loading</span></span>

<span data-ttu-id="f929d-288">目前的程式碼指定的積極式載入`Enrollments`和`Students`:</span><span class="sxs-lookup"><span data-stu-id="f929d-288">The current code specifies eager loading for `Enrollments` and `Students`:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/Index.cshtml.cs?name=snippet_ThenInclude&highlight=6-9)]

<span data-ttu-id="f929d-289">假設很少，使用者會想要看的課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="f929d-289">Suppose users rarely want to see enrollments in a course.</span></span> <span data-ttu-id="f929d-290">在此情況下，最佳方法，就是要求時，只載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-290">In that case, an optimization would be to only load the enrollment data if it's requested.</span></span> <span data-ttu-id="f929d-291">在本節中，`OnGetAsync`更新為使用明確載入`Enrollments`和`Students`。</span><span class="sxs-lookup"><span data-stu-id="f929d-291">In this section, the `OnGetAsync` is updated to use explicit loading of `Enrollments` and `Students`.</span></span>

<span data-ttu-id="f929d-292">更新`OnGetAsync`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f929d-292">Update the `OnGetAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Instructors/IndexXp.cshtml.cs?name=snippet_OnGetAsync&highlight=9-13,29-35)]

<span data-ttu-id="f929d-293">上述程式碼中會卸除*ThenInclude*方法會呼叫來註冊和學生的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-293">The preceding code drops the *ThenInclude* method calls for enrollment and student data.</span></span> <span data-ttu-id="f929d-294">如果選取課程時，反白顯示的程式碼就會擷取：</span><span class="sxs-lookup"><span data-stu-id="f929d-294">If a course is selected, the highlighted code retrieves:</span></span>

* <span data-ttu-id="f929d-295">`Enrollment`實體所選的課程。</span><span class="sxs-lookup"><span data-stu-id="f929d-295">The `Enrollment` entities for the selected course.</span></span>
* <span data-ttu-id="f929d-296">`Student`每個實體`Enrollment`。</span><span class="sxs-lookup"><span data-stu-id="f929d-296">The `Student` entities for each `Enrollment`.</span></span>

<span data-ttu-id="f929d-297">請注意上述程式碼註解`.AsNoTracking()`。</span><span class="sxs-lookup"><span data-stu-id="f929d-297">Notice the preceding code comments out `.AsNoTracking()`.</span></span> <span data-ttu-id="f929d-298">導覽屬性只能為明確載入的追蹤實體。</span><span class="sxs-lookup"><span data-stu-id="f929d-298">Navigation properties can only be explicitly loaded for tracked entities.</span></span>

<span data-ttu-id="f929d-299">測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f929d-299">Test the app.</span></span> <span data-ttu-id="f929d-300">使用者的觀點而言，應用程式的行為即會相同到之前的版本。</span><span class="sxs-lookup"><span data-stu-id="f929d-300">From a users perspective, the app behaves identically to the previous version.</span></span>

<span data-ttu-id="f929d-301">下一個教學課程會示範如何更新相關的資料。</span><span class="sxs-lookup"><span data-stu-id="f929d-301">The next tutorial shows how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="f929d-302">[上一頁](xref:data/ef-rp/complex-data-model)
>[下一頁](xref:data/ef-rp/update-related-data)</span><span class="sxs-lookup"><span data-stu-id="f929d-302">[Previous](xref:data/ef-rp/complex-data-model)
[Next](xref:data/ef-rp/update-related-data)</span></span>
