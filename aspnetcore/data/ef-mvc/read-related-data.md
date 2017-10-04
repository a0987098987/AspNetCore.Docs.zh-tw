---
title: "ASP.NET MVC EF 核心-讀取核心相關的資料-10-6"
author: tdykstra
description: "在本教學課程中，您將讀取並顯示相關的資料--也就是，可將 Entity Framework 載入導覽屬性的資料。"
keywords: "加入 ASP.NET Core，Entity Framework Core，相關資料，"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 71fec30f-8ea7-4ca8-96e3-d2e26c5be44e
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: 778ef976fdbef70684ca26b0c7c548ffcc83ee00
ms.sourcegitcommit: e45f8912ce32b4071bf7e83b8f8315cd8bba3520
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2017
---
# <a name="reading-related-data---ef-core-with-aspnet-core-mvc-tutorial-6-of-10"></a><span data-ttu-id="b9656-104">讀取的相關資料的 EF Core 與 ASP.NET Core MVC 教學課程 (10-6)</span><span class="sxs-lookup"><span data-stu-id="b9656-104">Reading related data - EF Core with ASP.NET Core MVC tutorial (6 of 10)</span></span>

<span data-ttu-id="b9656-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b9656-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b9656-106">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="b9656-106">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="b9656-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="b9656-107">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="b9656-108">在上一個教學課程中，您可以完成學校資料模型。</span><span class="sxs-lookup"><span data-stu-id="b9656-108">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="b9656-109">在本教學課程中，將讀取和顯示相關的資料--也就是，可將 Entity Framework 載入導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-109">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="b9656-110">下圖顯示的頁面，您將使用。</span><span class="sxs-lookup"><span data-stu-id="b9656-110">The following illustrations show the pages that you'll work with.</span></span>

![課程索引頁](read-related-data/_static/courses-index.png)

![講師索引頁](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="b9656-113">急切、 明確，和消極式載入之相關資料</span><span class="sxs-lookup"><span data-stu-id="b9656-113">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="b9656-114">有幾種物件關聯式對應 (ORM) 軟體例如 Entity Framework 可以載入實體的導覽屬性的相關的資料：</span><span class="sxs-lookup"><span data-stu-id="b9656-114">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="b9656-115">積極式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-115">Eager loading.</span></span> <span data-ttu-id="b9656-116">實體讀取時，同時擷取的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-116">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="b9656-117">這通常會導致單一聯結查詢以擷取所有所需的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-117">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="b9656-118">在 Entity Framework Core 指定使用積極式載入`Include`和`ThenInclude`方法。</span><span class="sxs-lookup"><span data-stu-id="b9656-118">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="b9656-120">您可以擷取一些個別的查詢中的資料和 EF 「 修復 」 的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-120">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="b9656-121">也就是說，EF 中，自動加入個別擷取的實體所屬的先前擷取的實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-121">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="b9656-122">查詢中擷取相關的資料，您可以使用`Load`方法，而不是傳回的清單或物件，例如方法`ToList`或`Single`。</span><span class="sxs-lookup"><span data-stu-id="b9656-122">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="b9656-124">明確式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-124">Explicit loading.</span></span> <span data-ttu-id="b9656-125">第一次讀取實體時，不被擷取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-125">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b9656-126">您撰寫程式碼，以便擷取相關的資料需要。</span><span class="sxs-lookup"><span data-stu-id="b9656-126">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="b9656-127">如同 eager 載入個別查詢的情況，明確載入多個查詢的結果傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="b9656-127">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="b9656-128">差別在於，具有明確式載入，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-128">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="b9656-129">您可以使用 Entity Framework Core 1.1`Load`方法以明確式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-129">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="b9656-130">例如: </span><span class="sxs-lookup"><span data-stu-id="b9656-130">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="b9656-132">消極式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-132">Lazy loading.</span></span> <span data-ttu-id="b9656-133">第一次讀取實體時，不被擷取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-133">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="b9656-134">不過，第一次您嘗試存取的導覽屬性，該導覽屬性所需的資料自動擷取。</span><span class="sxs-lookup"><span data-stu-id="b9656-134">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="b9656-135">每次您嘗試從導覽屬性取得資料，第一次傳送查詢到資料庫。</span><span class="sxs-lookup"><span data-stu-id="b9656-135">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="b9656-136">Entity Framework Core 1.0 不支援消極式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-136">Entity Framework Core 1.0 does not support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="b9656-137">效能考量</span><span class="sxs-lookup"><span data-stu-id="b9656-137">Performance considerations</span></span>

<span data-ttu-id="b9656-138">如果您知道您需要針對擷取每個實體的相關的資料，積極式載入通常提供最佳效能，因為傳送至資料庫的單一查詢通常比針對擷取每個實體個別查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="b9656-138">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="b9656-139">例如，假設每個部門各有十個相關的課程。</span><span class="sxs-lookup"><span data-stu-id="b9656-139">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="b9656-140">積極式載入的所有相關資料會導致只的單一 （加入） 查詢，並在單一來回行程到資料庫。</span><span class="sxs-lookup"><span data-stu-id="b9656-140">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="b9656-141">個別的查詢，針對每個部門的課程會導致資料庫 11 個往返。</span><span class="sxs-lookup"><span data-stu-id="b9656-141">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="b9656-142">延遲性很高時，特別是危害到效能額外反覆存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="b9656-142">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="b9656-143">相反地，在某些案例中個別查詢會更有效率。</span><span class="sxs-lookup"><span data-stu-id="b9656-143">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="b9656-144">在單一查詢中的所有相關資料的積極式載入可能會導致非常複雜聯結來產生，SQL Server 無法有效率地進行處理。</span><span class="sxs-lookup"><span data-stu-id="b9656-144">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="b9656-145">或如果您需要存取實體的導覽屬性，只會針對您正在處理的實體集的子集，個別查詢可能會更好因為積極式載入的所有項目最前面位置會擷取比您所需的更多資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-145">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="b9656-146">如果效能嚴重不足，所以最好先測試效能才能做出最好的選擇這兩種方式。</span><span class="sxs-lookup"><span data-stu-id="b9656-146">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="b9656-147">建立可顯示部門名稱的課程頁面</span><span class="sxs-lookup"><span data-stu-id="b9656-147">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="b9656-148">課程實體包括包含課程指派給部門的 Department 實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-148">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="b9656-149">若要指派的部門名稱在清單中顯示的課程，您需要從 Department 實體中取得的 Name 屬性`Course.Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-149">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="b9656-150">建立名為 CoursesController 課程實體類型，使用的相同選項的控制站**使用 Entity Framework 的檢視，MVC 控制器**scaffolder 您對先前的學生控制站，如中所示下圖：</span><span class="sxs-lookup"><span data-stu-id="b9656-150">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![加入課程控制器](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="b9656-152">開啟*CoursesController.cs*並檢查`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="b9656-152">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="b9656-153">自動 scaffolding 已指定的積極式載入`Department`導覽屬性使用`Include`方法。</span><span class="sxs-lookup"><span data-stu-id="b9656-153">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="b9656-154">取代`Index`方法與使用更適合的名稱，如下列程式碼`IQueryable`傳回課程實體 (`courses`而不是`schoolContext`):</span><span class="sxs-lookup"><span data-stu-id="b9656-154">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="b9656-155">開啟*Views/Courses/Index.cshtml*和範本程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9656-155">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="b9656-156">所做的變更會反白顯示：</span><span class="sxs-lookup"><span data-stu-id="b9656-156">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="b9656-157">您已對 scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="b9656-157">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="b9656-158">從索引的標題變更為 Courses 中。</span><span class="sxs-lookup"><span data-stu-id="b9656-158">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="b9656-159">加入**數目**顯示的資料行`CourseID`屬性值。</span><span class="sxs-lookup"><span data-stu-id="b9656-159">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="b9656-160">根據預設，主索引鍵不是建立結構，因為它們通常是無意義的使用者。</span><span class="sxs-lookup"><span data-stu-id="b9656-160">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="b9656-161">不過，在此情況下的主索引鍵是有意義，而且您想要將其顯示。</span><span class="sxs-lookup"><span data-stu-id="b9656-161">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="b9656-162">變更**部門**資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="b9656-162">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="b9656-163">程式碼會顯示`Name`Department 實體載入到屬性`Department`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="b9656-163">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="b9656-164">執行應用程式並選取**課程**索引標籤，查看與部門名稱清單。</span><span class="sxs-lookup"><span data-stu-id="b9656-164">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![課程索引頁](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="b9656-166">建立講師頁面，其中顯示 Courses 以及註冊項目</span><span class="sxs-lookup"><span data-stu-id="b9656-166">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="b9656-167">在本節中，您將建立控制器和檢視 [Instructor] 實體，以顯示講師頁面：</span><span class="sxs-lookup"><span data-stu-id="b9656-167">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![講師索引頁](read-related-data/_static/instructors-index.png)

<span data-ttu-id="b9656-169">此頁面讀取，並會顯示相關的資料以下列方式：</span><span class="sxs-lookup"><span data-stu-id="b9656-169">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="b9656-170">講師清單會顯示從 OfficeAssignment 實體的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-170">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="b9656-171">「 講師 」 和 「 OfficeAssignment 實體是以 0 或-1 一個關聯性中。</span><span class="sxs-lookup"><span data-stu-id="b9656-171">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="b9656-172">您將用於 OfficeAssignment 實體的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-172">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="b9656-173">如前所述，積極式載入時，通常更有效率主資料表的所有擷取的資料列所需的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-173">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="b9656-174">在此情況下，您會想要顯示為顯示的所有講師 office 指派。</span><span class="sxs-lookup"><span data-stu-id="b9656-174">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="b9656-175">當使用者選取講師時，會顯示相關的課程實體。</span><span class="sxs-lookup"><span data-stu-id="b9656-175">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="b9656-176">「 講師 」 和 「 課程實體是多對多關聯性中。</span><span class="sxs-lookup"><span data-stu-id="b9656-176">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="b9656-177">您會使用課程實體和其相關的 Department 實體的積極式載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-177">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="b9656-178">在此情況下，個別查詢可能會更有效率因為您只會針對所選講師需要課程。</span><span class="sxs-lookup"><span data-stu-id="b9656-178">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="b9656-179">不過，此範例會示範如何使用積極式載入本身是導覽屬性中的實體內的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-179">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="b9656-180">當使用者選取課程之後時，會顯示註冊實體集的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-180">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="b9656-181">「 課程 」 和 「 註冊 」 實體是一對多關聯性中。</span><span class="sxs-lookup"><span data-stu-id="b9656-181">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="b9656-182">您將註冊的實體和其相關的學生實體使用不同的查詢。</span><span class="sxs-lookup"><span data-stu-id="b9656-182">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="b9656-183">建立講師索引檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="b9656-183">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="b9656-184">講師頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-184">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="b9656-185">因此，您將建立包含三個屬性，每個保留其中一個資料表的資料檢視模型。</span><span class="sxs-lookup"><span data-stu-id="b9656-185">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="b9656-186">在*SchoolViewModels*資料夾中，建立*InstructorIndexData.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="b9656-186">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="b9656-187">建立講師控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="b9656-187">Create the Instructor controller and views</span></span>

<span data-ttu-id="b9656-188">建立講師控制器 EF 讀取/寫入動作，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="b9656-188">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![新增講師控制站](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="b9656-190">開啟*InstructorsController.cs*和加入 using ViewModels 命名空間陳述式：</span><span class="sxs-lookup"><span data-stu-id="b9656-190">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="b9656-191">Index 方法取代為下列程式碼相關資料的積極式載入作業，並將其放在檢視模型。</span><span class="sxs-lookup"><span data-stu-id="b9656-191">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="b9656-192">該方法會接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`) 所提供的識別碼值選取的講師和選取的課程。</span><span class="sxs-lookup"><span data-stu-id="b9656-192">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="b9656-193">參數所提供的**選取**頁面上的超連結。</span><span class="sxs-lookup"><span data-stu-id="b9656-193">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="b9656-194">程式碼首先會建立檢視模型的執行個體，並放在它的講師清單。</span><span class="sxs-lookup"><span data-stu-id="b9656-194">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="b9656-195">程式碼會指定的積極式載入`Instructor.OfficeAssignment`和`Instructor.CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-195">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="b9656-196">內`CourseAssignments`屬性，`Course`屬性是載入，並在其中，`Enrollments`和`Department`屬性已載入，並在每個`Enrollment`實體`Student`屬性載入。</span><span class="sxs-lookup"><span data-stu-id="b9656-196">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="b9656-197">由於檢視一律需要 OfficeAssignment 實體，會在相同查詢中擷取的更有效率。</span><span class="sxs-lookup"><span data-stu-id="b9656-197">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="b9656-198">單一查詢是優於多個查詢，只有當頁面會顯示頻率課程，而不選取比在網頁上，選取講師時需要課程實體。</span><span class="sxs-lookup"><span data-stu-id="b9656-198">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="b9656-199">程式碼重複`CourseAssignments`和`Course`因為您需要從兩個屬性`Course`。</span><span class="sxs-lookup"><span data-stu-id="b9656-199">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="b9656-200">第一個字串的`ThenInclude`呼叫取得`CourseAssignment.Course`， `Course.Enrollments`，和`Enrollment.Student`。</span><span class="sxs-lookup"><span data-stu-id="b9656-200">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="b9656-201">此時，在程式碼的另一個`ThenInclude`導覽屬性是`Student`，您不需要。</span><span class="sxs-lookup"><span data-stu-id="b9656-201">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="b9656-202">但呼叫`Include`透過開頭`Instructor`屬性，所以您不必瀏覽鏈結同樣地，此時間指定`Course.Department`而不是`Course.Enrollments`。</span><span class="sxs-lookup"><span data-stu-id="b9656-202">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="b9656-203">當講師已選取時，就會執行下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9656-203">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="b9656-204">選取的講師會擷取從講師中檢視模型的清單。</span><span class="sxs-lookup"><span data-stu-id="b9656-204">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="b9656-205">檢視模型`Courses`屬性然後載入該講師課程實體與`CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-205">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="b9656-206">`Where`方法會傳回集合，但準則在此情況下傳遞至該方法會導致只有單一 Instructor 實體傳回。</span><span class="sxs-lookup"><span data-stu-id="b9656-206">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="b9656-207">`Single`方法會將集合轉換成可讓您存取與該實體單一講師實體`CourseAssignments`屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-207">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="b9656-208">`CourseAssignments`屬性包含`CourseAssignment`實體，在您想只相關`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="b9656-208">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="b9656-209">您使用`Single`集合，當您知道集合的方法將會有只有一個項目。</span><span class="sxs-lookup"><span data-stu-id="b9656-209">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="b9656-210">如果是空的集合傳遞給它，或若有多個項目，則單一方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="b9656-210">The Single method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="b9656-211">替代方式是`SingleOrDefault`，傳回的預設值 (null 在此情況下) 如果集合是空的。</span><span class="sxs-lookup"><span data-stu-id="b9656-211">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="b9656-212">不過，在此情況下，就仍然造成例外狀況 (從嘗試尋找`Courses`對 null 參考的屬性)，以及例外狀況訊息會較不清楚指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="b9656-212">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="b9656-213">當您呼叫`Single`方法，您也可以傳遞在 where 條件，而不是呼叫`Where`方法分別：</span><span class="sxs-lookup"><span data-stu-id="b9656-213">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="b9656-214">而非：</span><span class="sxs-lookup"><span data-stu-id="b9656-214">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="b9656-215">接下來，如果已選取課程，選定的課程從擷取中檢視模型的課程的清單。</span><span class="sxs-lookup"><span data-stu-id="b9656-215">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="b9656-216">然後檢視模型的`Enrollments`屬性載入與註冊的實體，從該課程`Enrollments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-216">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="b9656-217">修改講師索引檢視表</span><span class="sxs-lookup"><span data-stu-id="b9656-217">Modify the Instructor Index view</span></span>

<span data-ttu-id="b9656-218">在*Views/Instructors/Index.cshtml*，範本程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9656-218">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="b9656-219">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="b9656-219">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="b9656-220">您已對現有的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="b9656-220">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="b9656-221">變更模型類別`InstructorIndexData`。</span><span class="sxs-lookup"><span data-stu-id="b9656-221">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="b9656-222">變更頁面標題從**索引**至**講師**。</span><span class="sxs-lookup"><span data-stu-id="b9656-222">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="b9656-223">加入**Office**顯示之資料行`item.OfficeAssignment.Location`才`item.OfficeAssignment`不是 null。</span><span class="sxs-lookup"><span data-stu-id="b9656-223">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="b9656-224">（因為這是以 0 或-1 一個關聯性，可能不會有相關的 OfficeAssignment 實體。）</span><span class="sxs-lookup"><span data-stu-id="b9656-224">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="b9656-225">加入**課程**資料行，顯示課程教導每個講師所。</span><span class="sxs-lookup"><span data-stu-id="b9656-225">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="b9656-226">請參閱[使用明確列轉換`@:`](xref:mvc/views/razor#explicit-line-transition-with-)如需有關此 razor 語法。</span><span class="sxs-lookup"><span data-stu-id="b9656-226">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="b9656-227">加入的程式碼動態新增`class="success"`至`tr`選取講師項目。</span><span class="sxs-lookup"><span data-stu-id="b9656-227">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="b9656-228">這會設定使用啟動程序的類別所選取的資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="b9656-228">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="b9656-229">加入新的超連結標示為**選取**之前中每個資料列的其他連結，而使得所選的講師識別碼傳送至`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="b9656-229">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="b9656-230">執行應用程式並選取**講師** 索引標籤。沒有相關的 OfficeAssignment 實體時，頁面就會顯示相關的 OfficeAssignment 實體和空的資料表資料格的 Location 屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-230">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![選取任何項目講師索引頁](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="b9656-232">在*Views/Instructors/Index.cshtml*檔案之後關閉資料表項目 （在檔案的結尾），, 加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9656-232">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="b9656-233">此程式碼會顯示一份時選取講師講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="b9656-233">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="b9656-234">此程式碼讀取`Courses`要顯示的課程清單的檢視模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-234">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="b9656-235">它也提供**選取**超連結，將傳送至所選的課程識別碼`Index`動作方法。</span><span class="sxs-lookup"><span data-stu-id="b9656-235">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="b9656-236">重新整理頁面，然後選取 instructor。</span><span class="sxs-lookup"><span data-stu-id="b9656-236">Refresh the page and select an instructor.</span></span> <span data-ttu-id="b9656-237">現在您會看到一個方格，其中會顯示指派給所選講師，課程，每個課程中，您看到指派的部門名稱。</span><span class="sxs-lookup"><span data-stu-id="b9656-237">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![選取講師索引頁面講師](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="b9656-239">之後您剛才加入的程式碼區塊，加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9656-239">After the code block you just added, add the following code.</span></span> <span data-ttu-id="b9656-240">在課程中，選取該課程時，這會顯示一份學生註冊。</span><span class="sxs-lookup"><span data-stu-id="b9656-240">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="b9656-241">此程式碼會讀取檢視模型的註冊項目屬性，以顯示一份學生課程中註冊。</span><span class="sxs-lookup"><span data-stu-id="b9656-241">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="b9656-242">再次重新整理頁面，然後選取 instructor。</span><span class="sxs-lookup"><span data-stu-id="b9656-242">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="b9656-243">然後，選取課程來查看已註冊的學生版和其等級清單。</span><span class="sxs-lookup"><span data-stu-id="b9656-243">Then select a course to see the list of enrolled students and their grades.</span></span>

![講師索引頁面講師和選取的課程](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="b9656-245">明確式載入</span><span class="sxs-lookup"><span data-stu-id="b9656-245">Explicit loading</span></span>

<span data-ttu-id="b9656-246">當您擷取在講師清單*InstructorsController.cs*，您所指定的積極式載入`CourseAssignments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-246">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="b9656-247">假設您預期使用者很少想要查看所選的講師和課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="b9656-247">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="b9656-248">在此情況下，可能會想要載入註冊資料才會在收到要求。</span><span class="sxs-lookup"><span data-stu-id="b9656-248">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="b9656-249">若要查看如何明確載入的範例，取代`Index`為下列程式碼，以移除註冊的積極式載入，並明確地載入該屬性的方法。</span><span class="sxs-lookup"><span data-stu-id="b9656-249">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="b9656-250">程式碼變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="b9656-250">The code changes are highlighted.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="b9656-251">新的程式碼會卸除*ThenInclude*方法會呼叫註冊資料來擷取講師實體的程式碼。</span><span class="sxs-lookup"><span data-stu-id="b9656-251">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="b9656-252">如果選取講師和課程，反白顯示的程式碼會擷取所選的課程中，註冊實體和每個註冊的學生實體。</span><span class="sxs-lookup"><span data-stu-id="b9656-252">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="b9656-253">執行應用程式，請移至講師索引頁面現在，您會看到任何差異顯示在頁面上，雖然您已變更資料擷取的方式。</span><span class="sxs-lookup"><span data-stu-id="b9656-253">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="b9656-254">總結</span><span class="sxs-lookup"><span data-stu-id="b9656-254">Summary</span></span>

<span data-ttu-id="b9656-255">您現在所積極式載入一個查詢與多個查詢相關的資料讀取到導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="b9656-255">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="b9656-256">在下一個教學課程中，您將學習如何更新相關的資料。</span><span class="sxs-lookup"><span data-stu-id="b9656-256">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="b9656-257">[上一頁](complex-data-model.md)
>[下一頁](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="b9656-257">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
