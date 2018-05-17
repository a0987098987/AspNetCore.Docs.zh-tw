---
title: ASP.NET Core MVC 與 EF Core - 讀取相關資料 - 6/10
author: rick-anderson
description: 在此教學課程中，您將讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/read-related-data
ms.openlocfilehash: db47340aa3dbca486cc30667baf03e49491f1d1a
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---read-related-data---6-of-10"></a><span data-ttu-id="cac58-103">ASP.NET Core MVC 與 EF Core - 讀取相關資料 - 6/10</span><span class="sxs-lookup"><span data-stu-id="cac58-103">ASP.NET Core MVC with EF Core - Read Related Data - 6 of 10</span></span>

<span data-ttu-id="cac58-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="cac58-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="cac58-105">Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="cac58-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="cac58-106">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="cac58-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="cac58-107">在上一個教學課程中，您已完成 School 資料模型。</span><span class="sxs-lookup"><span data-stu-id="cac58-107">In the previous tutorial, you completed the School data model.</span></span> <span data-ttu-id="cac58-108">在此教學課程中，您將讀取並顯示相關資料-- 也就是 Entity Framework 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-108">In this tutorial, you'll read and display related data -- that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="cac58-109">下列圖例顯示了您將操作的頁面。</span><span class="sxs-lookup"><span data-stu-id="cac58-109">The following illustrations show the pages that you'll work with.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

## <a name="eager-explicit-and-lazy-loading-of-related-data"></a><span data-ttu-id="cac58-112">相關資料的積極式、明確式和消極式載入</span><span class="sxs-lookup"><span data-stu-id="cac58-112">Eager, explicit, and lazy Loading of related data</span></span>

<span data-ttu-id="cac58-113">Entity Framework 等物件關聯式對應 (ORM) 有幾種方式可以將相關資料載入到實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="cac58-113">There are several ways that Object-Relational Mapping (ORM) software such as Entity Framework can load related data into the navigation properties of an entity:</span></span>

* <span data-ttu-id="cac58-114">積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-114">Eager loading.</span></span> <span data-ttu-id="cac58-115">讀取實體時，將會同時擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-115">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="cac58-116">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-116">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="cac58-117">您可以使用 `Include` 和 `ThenInclude` 方法，在 Entity Framework Core 中指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-117">You specify eager loading in Entity Framework Core by using the `Include` and `ThenInclude` methods.</span></span>

  ![積極式載入範例](read-related-data/_static/eager-loading.png)

  <span data-ttu-id="cac58-119">您可以在個別查詢中擷取一些資料，而 EF 會「修正」導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-119">You can retrieve some of the data in separate queries, and EF "fixes up" the navigation properties.</span></span>  <span data-ttu-id="cac58-120">也就是說，EF 會自動新增個別擷取的實體，其中它們屬於先前擷取之實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-120">That is, EF automatically adds the separately retrieved entities where they belong in navigation properties of previously retrieved entities.</span></span> <span data-ttu-id="cac58-121">對於擷取相關資料的查詢，您可以使用 `Load` 方法，而不是傳回清單或物件的方法，例如 `ToList` 或 `Single`。</span><span class="sxs-lookup"><span data-stu-id="cac58-121">For the query that retrieves related data, you can use the `Load` method instead of a method that returns a list or object, such as `ToList` or `Single`.</span></span>

  ![個別查詢範例](read-related-data/_static/separate-queries.png)

* <span data-ttu-id="cac58-123">明確式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-123">Explicit loading.</span></span> <span data-ttu-id="cac58-124">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-124">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="cac58-125">您可以撰寫程式碼，以擷取需要的相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-125">You write code that retrieves the related data if it's needed.</span></span> <span data-ttu-id="cac58-126">如同使用個別查詢的積極式載入，明確式載入會導致多個查詢傳送至資料庫。</span><span class="sxs-lookup"><span data-stu-id="cac58-126">As in the case of eager loading with separate queries, explicit loading results in multiple queries sent to the database.</span></span> <span data-ttu-id="cac58-127">不同之處在於，使用明確式載入時，程式碼會指定要載入的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-127">The difference is that with explicit loading, the code specifies the navigation properties to be loaded.</span></span> <span data-ttu-id="cac58-128">在 Entity Framework Core 1.1 中，您可以使用 `Load` 方法以執行明確式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-128">In Entity Framework Core 1.1 you can use the `Load` method to do explicit loading.</span></span> <span data-ttu-id="cac58-129">例如: </span><span class="sxs-lookup"><span data-stu-id="cac58-129">For example:</span></span>

  ![明確式載入範例](read-related-data/_static/explicit-loading.png)

* <span data-ttu-id="cac58-131">消極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-131">Lazy loading.</span></span> <span data-ttu-id="cac58-132">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-132">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="cac58-133">不過，第一次嘗試存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-133">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="cac58-134">每當您第一次嘗試從導覽屬性取得資料時，就會傳送查詢到資料庫。</span><span class="sxs-lookup"><span data-stu-id="cac58-134">A query is sent to the database each time you try to get data from a navigation property for the first time.</span></span> <span data-ttu-id="cac58-135">Entity Framework Core 1.0 不支援消極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-135">Entity Framework Core 1.0 doesn't support lazy loading.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="cac58-136">效能考量</span><span class="sxs-lookup"><span data-stu-id="cac58-136">Performance considerations</span></span>

<span data-ttu-id="cac58-137">如果您知道擷取的每個實體需要相關資料，積極式載入通常可以提供最佳效能，因為傳送至資料庫的單一查詢通常比所擷取每個實體的個別查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="cac58-137">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="cac58-138">例如，假設每個部門各有十個相關課程。</span><span class="sxs-lookup"><span data-stu-id="cac58-138">For example, suppose that each department has ten related courses.</span></span> <span data-ttu-id="cac58-139">所有相關資料的積極式載入會導致只有單一 (聯結) 查詢，以及資料庫的單一來回行程。</span><span class="sxs-lookup"><span data-stu-id="cac58-139">Eager loading of all related data would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="cac58-140">每個部門課程的個別查詢則會導致資料庫的十一個來回行程。</span><span class="sxs-lookup"><span data-stu-id="cac58-140">A separate query for courses for each department would result in eleven round trips to the database.</span></span> <span data-ttu-id="cac58-141">當延遲很高時，資料庫的額外來回行程對效能特別不利。</span><span class="sxs-lookup"><span data-stu-id="cac58-141">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="cac58-142">相反地，在某些案例中，個別查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="cac58-142">On the other hand, in some scenarios separate queries is more efficient.</span></span> <span data-ttu-id="cac58-143">在單一查詢中進行所有相關資料的積極式載入時，可能會導致產生非常複雜的聯結，SQL Server 無法有效率地進行處理。</span><span class="sxs-lookup"><span data-stu-id="cac58-143">Eager loading of all related data in one query might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="cac58-144">或者，如果您只需要針對所處理之實體集的子集存取實體的導覽屬性，則執行個別查詢可能會更好；因為預先進行所有項目的積極式載入可能會擷取比您所需更多的資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-144">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, separate queries might perform better because eager loading of everything up front would retrieve more data than you need.</span></span> <span data-ttu-id="cac58-145">如果效能嚴重不足，最好先測試這兩種方式的效能，才能做出最好的選擇。</span><span class="sxs-lookup"><span data-stu-id="cac58-145">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="cac58-146">建立顯示部門名稱的 Courses 頁面</span><span class="sxs-lookup"><span data-stu-id="cac58-146">Create a Courses page that displays Department name</span></span>

<span data-ttu-id="cac58-147">Course 實體包括一個導覽屬性，其中包含已指派課程之部門的 Department 實體。</span><span class="sxs-lookup"><span data-stu-id="cac58-147">The Course entity includes a navigation property that contains the Department entity of the department that the course is assigned to.</span></span> <span data-ttu-id="cac58-148">若要在課程清單中顯示所指派部門的名稱，您需要從位於 `Course.Department` 導覽屬性的 Department 實體中取得 Name 屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-148">To display the name of the assigned department in a list of courses, you need to get the Name property from the Department entity that's in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="cac58-149">針對 Course 實體類型建立名為 CoursesController 的控制器，並對**使用 Entity Framework 執行檢視的 MVC 控制器**框架使用先前針對學生控制器使用的相同選項，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="cac58-149">Create a controller named CoursesController for the Course entity type, using the same options for the **MVC Controller with views, using Entity Framework** scaffolder that you did earlier for the Students controller, as shown in the following illustration:</span></span>

![新增課程控制器](read-related-data/_static/add-courses-controller.png)

<span data-ttu-id="cac58-151">開啟 *CoursesController.cs*，並檢查 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="cac58-151">Open *CoursesController.cs* and examine the `Index` method.</span></span> <span data-ttu-id="cac58-152">自動 Scaffolding 已使用 `Include` 方法，針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-152">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="cac58-153">以下列程式碼取代 `Index` 方法，以針對傳回 Course 實體的 `IQueryable` 使用更合適的名稱 (`courses` 而不是 `schoolContext`)：</span><span class="sxs-lookup"><span data-stu-id="cac58-153">Replace the `Index` method with the following code that uses a more appropriate name for the `IQueryable` that returns Course entities (`courses` instead of `schoolContext`):</span></span>

[!code-csharp[](intro/samples/cu/Controllers/CoursesController.cs?name=snippet_RevisedIndexMethod)]

<span data-ttu-id="cac58-154">開啟 *Views/Courses/Index.cshtml*，並以下列程式碼取代範本程式碼。</span><span class="sxs-lookup"><span data-stu-id="cac58-154">Open *Views/Courses/Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="cac58-155">所做的變更已醒目提示：</span><span class="sxs-lookup"><span data-stu-id="cac58-155">The changes are highlighted:</span></span>

[!code-html[](intro/samples/cu/Views/Courses/Index.cshtml?highlight=4,7,15-17,34-36,44)]

<span data-ttu-id="cac58-156">您已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="cac58-156">You've made the following changes to the scaffolded code:</span></span>

* <span data-ttu-id="cac58-157">已將標題從「索引」) 變更為「課程」。</span><span class="sxs-lookup"><span data-stu-id="cac58-157">Changed the heading from Index to Courses.</span></span>

* <span data-ttu-id="cac58-158">新增顯示 `CourseID` 屬性值的 [編號] 資料行。</span><span class="sxs-lookup"><span data-stu-id="cac58-158">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="cac58-159">主索引鍵預設不會進行 Scaffold，因為它們對終端使用者通常沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="cac58-159">By default, primary keys aren't scaffolded because normally they're meaningless to end users.</span></span> <span data-ttu-id="cac58-160">不過，在此情況下主索引鍵有意義，因此您想要顯示它。</span><span class="sxs-lookup"><span data-stu-id="cac58-160">However, in this case the primary key is meaningful and you want to show it.</span></span>

* <span data-ttu-id="cac58-161">變更 [部門] 資料行來顯示部門名稱。</span><span class="sxs-lookup"><span data-stu-id="cac58-161">Changed the **Department** column to display the department name.</span></span> <span data-ttu-id="cac58-162">此程式碼會顯示已載入到 `Department` 導覽屬性之 Department 實體的 `Name` 屬性：</span><span class="sxs-lookup"><span data-stu-id="cac58-162">The code displays the `Name` property of the Department entity that's loaded into the `Department` navigation property:</span></span>

  ```html
  @Html.DisplayFor(modelItem => item.Department.Name)
  ```

<span data-ttu-id="cac58-163">執行應用程式，並選取 [Courses] 索引標籤來查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="cac58-163">Run the app and select the **Courses** tab to see the list with department names.</span></span>

![Courses [索引] 頁面](read-related-data/_static/courses-index.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="cac58-165">建立顯示課程和註冊的 Instructors 頁面</span><span class="sxs-lookup"><span data-stu-id="cac58-165">Create an Instructors page that shows Courses and Enrollments</span></span>

<span data-ttu-id="cac58-166">在本節中，您將建立 Instructor 實體的控制器和檢視，以顯示 Instructors 頁面：</span><span class="sxs-lookup"><span data-stu-id="cac58-166">In this section, you'll create a controller and view for the Instructor entity in order to display the Instructors page:</span></span>

![Instructors [索引] 頁面](read-related-data/_static/instructors-index.png)

<span data-ttu-id="cac58-168">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="cac58-168">This page reads and displays related data in the following ways:</span></span>

* <span data-ttu-id="cac58-169">講師清單會顯示來自 OfficeAssignment 實體的相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-169">The list of instructors displays related data from the OfficeAssignment entity.</span></span> <span data-ttu-id="cac58-170">Instructor 與 OfficeAssignment 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="cac58-170">The Instructor and OfficeAssignment entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="cac58-171">您將針對 OfficeAssignment 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-171">You'll use eager loading for the OfficeAssignment entities.</span></span> <span data-ttu-id="cac58-172">如上所述，當您需要主要資料表中所有擷取資料列的相關資料時，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="cac58-172">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="cac58-173">在此情況下，您可能想要顯示所有已呈現講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="cac58-173">In this case, you want to display office assignments for all displayed instructors.</span></span>

* <span data-ttu-id="cac58-174">當使用者選取講師時，將會顯示相關的 Course 實體。</span><span class="sxs-lookup"><span data-stu-id="cac58-174">When the user selects an instructor, related Course entities are displayed.</span></span> <span data-ttu-id="cac58-175">Instructor 與 Course 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="cac58-175">The Instructor and Course entities are in a many-to-many relationship.</span></span> <span data-ttu-id="cac58-176">您將針對 Course 實體和其相關的 Department 實體使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-176">You'll use eager loading for the Course entities and their related Department entities.</span></span> <span data-ttu-id="cac58-177">在此情況下，個別查詢可能更有效率，因為您只需要所選取講師的課程。</span><span class="sxs-lookup"><span data-stu-id="cac58-177">In this case, separate queries might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="cac58-178">不過，這個範例會示範如何在本身處於導覽屬性內的實體中，針對導覽屬性使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-178">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>

* <span data-ttu-id="cac58-179">當使用者選取課程時，將會顯示來自 Enrollments 實體集的相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-179">When the user selects a course, related data from the Enrollments entity set is displayed.</span></span> <span data-ttu-id="cac58-180">Course 與 Enrollment 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="cac58-180">The Course and Enrollment entities are in a one-to-many relationship.</span></span> <span data-ttu-id="cac58-181">您將針對 Enrollment 實體和其相關的 Student 實體使用個別查詢。</span><span class="sxs-lookup"><span data-stu-id="cac58-181">You'll use separate queries for Enrollment entities and their related Student entities.</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="cac58-182">建立 Instructor [索引] 檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="cac58-182">Create a view model for the Instructor Index view</span></span>

<span data-ttu-id="cac58-183">Instructors 頁面會顯示下列三個不同資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-183">The Instructors page shows data from three different tables.</span></span> <span data-ttu-id="cac58-184">因此，您將建立包含三個屬性的檢視模型，每個保留其中一個資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-184">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="cac58-185">在 *SchoolViewModels* 資料夾中建立 *InstructorIndexData.cs*，然後以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="cac58-185">In the *SchoolViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/InstructorIndexData.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="cac58-186">建立 Instructor 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="cac58-186">Create the Instructor controller and views</span></span>

<span data-ttu-id="cac58-187">使用 EF 讀取/寫入動作建立 Instructors 控制器，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="cac58-187">Create an Instructors controller with EF read/write actions as shown in the following illustration:</span></span>

![新增 Instructors 控制器](read-related-data/_static/add-instructors-controller.png)

<span data-ttu-id="cac58-189">開啟 *InstructorsController.cs*，並針對 ViewModels 命名空間新增 using 陳述式：</span><span class="sxs-lookup"><span data-stu-id="cac58-189">Open *InstructorsController.cs* and add a using statement for the ViewModels namespace:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_Using)]

<span data-ttu-id="cac58-190">以下列程式碼取代 Index 方法，以便執行相關資料的積極式載入，並將其置於檢視模型中。</span><span class="sxs-lookup"><span data-stu-id="cac58-190">Replace the Index method with the following code to do eager loading of related data and put it in the view model.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_EagerLoading)]

<span data-ttu-id="cac58-191">此方法可接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`)，以提供所選取講師和所選取課程的識別碼值。</span><span class="sxs-lookup"><span data-stu-id="cac58-191">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course.</span></span> <span data-ttu-id="cac58-192">這些參數由頁面上的**選取**超連結提供。</span><span class="sxs-lookup"><span data-stu-id="cac58-192">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="cac58-193">此程式碼是從建立檢視模型的執行個體，並將其置於講師清單開始。</span><span class="sxs-lookup"><span data-stu-id="cac58-193">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="cac58-194">這個程式碼會針對 `Instructor.OfficeAssignment` 和 `Instructor.CourseAssignments` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-194">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.CourseAssignments` navigation properties.</span></span> <span data-ttu-id="cac58-195">在 `CourseAssignments` 屬性內載入 `Course` 屬性，並在其中載入 `Enrollments` 和 `Department` 屬性，而在每個 `Enrollment` 實體內載入 `Student` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-195">Within the `CourseAssignments` property, the `Course` property is loaded, and within that, the `Enrollments` and `Department` properties are loaded, and within each `Enrollment` entity the `Student` property is loaded.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude)]

<span data-ttu-id="cac58-196">由於檢視一律需要 OfficeAssignment 實體，因此在相同查詢中擷取該實體更有效率。</span><span class="sxs-lookup"><span data-stu-id="cac58-196">Since the view always requires the OfficeAssignment entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="cac58-197">在網頁上選取講師時需要 Course 實體，因此只有在選取課程時的頁面顯示頻率高於未選取時，單一查詢才優於多個查詢。</span><span class="sxs-lookup"><span data-stu-id="cac58-197">Course entities are required when an instructor is selected in the web page, so a single query is better than multiple queries only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="cac58-198">此程式碼會重複 `CourseAssignments` 和 `Course`，因為您需要來自 `Course` 的兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-198">The code repeats `CourseAssignments` and `Course` because you need two properties from `Course`.</span></span> <span data-ttu-id="cac58-199">`ThenInclude` 呼叫的第一個字串會取得 `CourseAssignment.Course`、`Course.Enrollments` 和 `Enrollment.Student`。</span><span class="sxs-lookup"><span data-stu-id="cac58-199">The first string of `ThenInclude` calls gets `CourseAssignment.Course`, `Course.Enrollments`, and `Enrollment.Student`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=3-6)]

<span data-ttu-id="cac58-200">此時，程式碼中的另一個 `ThenInclude` 將用於您不需要的 `Student` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-200">At that point in the code, another `ThenInclude` would be for navigation properties of `Student`, which you don't need.</span></span> <span data-ttu-id="cac58-201">但呼叫 `Include` 會使用 `Instructor`　屬性從頭開始，所以您必須再次瀏覽鏈結，這次請指定 `Course.Department` 而不是 `Course.Enrollments`。</span><span class="sxs-lookup"><span data-stu-id="cac58-201">But calling `Include` starts over with `Instructor` properties, so you have to go through the chain again, this time specifying `Course.Department` instead of `Course.Enrollments`.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ThenInclude&highlight=7-9)]

<span data-ttu-id="cac58-202">下列程式碼會在已選取講師時執行。</span><span class="sxs-lookup"><span data-stu-id="cac58-202">The following code executes when an instructor was selected.</span></span> <span data-ttu-id="cac58-203">選取的講師會從檢視模型的講師清單中擷取。</span><span class="sxs-lookup"><span data-stu-id="cac58-203">The selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="cac58-204">檢視模型的 `Courses` 屬性則使用 Course 實體從該講師的 `CourseAssignments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-204">The view model's `Courses` property is then loaded with the Course entities from that instructor's `CourseAssignments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=56-62)]

<span data-ttu-id="cac58-205">`Where` 方法會傳回集合，但在此情況下，傳遞至該方法的準則將導致只傳回單一 Instructor 實體。</span><span class="sxs-lookup"><span data-stu-id="cac58-205">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single Instructor entity being returned.</span></span> <span data-ttu-id="cac58-206">`Single` 方法會將集合轉換單一 Instructor 實體，這可讓您存取該實體的 `CourseAssignments` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-206">The `Single` method converts the collection into a single Instructor entity, which gives you access to that entity's `CourseAssignments` property.</span></span> <span data-ttu-id="cac58-207">`CourseAssignments` 屬性包含 `CourseAssignment` 實體，您只想要來自該實體的相關 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="cac58-207">The `CourseAssignments` property contains `CourseAssignment` entities, from which you want only the related `Course` entities.</span></span>

<span data-ttu-id="cac58-208">當您知道集合只會有一個項目時，就可以在集合上使用 `Single` 方法。</span><span class="sxs-lookup"><span data-stu-id="cac58-208">You use the `Single` method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="cac58-209">如果傳遞的集合是空的或有多個項目，Single 方法會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="cac58-209">The Single method throws an exception if the collection passed to it's empty or if there's more than one item.</span></span> <span data-ttu-id="cac58-210">替代方式是 `SingleOrDefault`，它會在集合是空的時傳回預設值 (在此情況下為 Null)。</span><span class="sxs-lookup"><span data-stu-id="cac58-210">An alternative is `SingleOrDefault`, which returns a default value (null in this case) if the collection is empty.</span></span> <span data-ttu-id="cac58-211">不過，在此情況下仍然會造成例外狀況 (由於嘗試在 Null 參考上尋找 `Courses` 屬性)，而例外狀況訊息會不太清楚地指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="cac58-211">However, in this case that would still result in an exception (from trying to find a `Courses` property on a null reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="cac58-212">當您呼叫 `Single` 方法時，也可以傳入 Where 條件，而不是分別呼叫 `Where` 方法：</span><span class="sxs-lookup"><span data-stu-id="cac58-212">When you call the `Single` method, you can also pass in the Where condition instead of calling the `Where` method separately:</span></span>

```csharp
.Single(i => i.ID == id.Value)
```

<span data-ttu-id="cac58-213">而非：</span><span class="sxs-lookup"><span data-stu-id="cac58-213">Instead of:</span></span>

```csharp
.Where(I => i.ID == id.Value).Single()
```

<span data-ttu-id="cac58-214">接下來，如果已選取課程，則會從檢視模型的課程清單中擷取選取的課程。</span><span class="sxs-lookup"><span data-stu-id="cac58-214">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="cac58-215">然後，檢視模型的 `Enrollments` 屬性會使用 Enrollment 實體從該課程的 `Enrollments` 導覽屬性載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-215">Then the view model's `Enrollments` property is loaded with the Enrollment entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?range=64-69)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="cac58-216">修改 Instructor [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="cac58-216">Modify the Instructor Index view</span></span>

<span data-ttu-id="cac58-217">在 *Views/Instructors/Index.cshtml* 中，以下列程式碼取代範本程式碼。</span><span class="sxs-lookup"><span data-stu-id="cac58-217">In *Views/Instructors/Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="cac58-218">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="cac58-218">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=1-64&highlight=1,3-7,15-19,24,26-31,41-54,56)]

<span data-ttu-id="cac58-219">您已對現有程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="cac58-219">You've made the following changes to the existing code:</span></span>

* <span data-ttu-id="cac58-220">已將模型類別變更為 `InstructorIndexData`。</span><span class="sxs-lookup"><span data-stu-id="cac58-220">Changed the model class to `InstructorIndexData`.</span></span>

* <span data-ttu-id="cac58-221">已將頁面標題從**索引**變更為**講師**。</span><span class="sxs-lookup"><span data-stu-id="cac58-221">Changed the page title from **Index** to **Instructors**.</span></span>

* <span data-ttu-id="cac58-222">新增 [辦公室] 資料行，該資料行只有在 `item.OfficeAssignment` 不是 Null 時才會顯示 `item.OfficeAssignment.Location`。</span><span class="sxs-lookup"><span data-stu-id="cac58-222">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` isn't null.</span></span> <span data-ttu-id="cac58-223">(因為這是一對零或一關聯性，所有可能沒有相關的 OfficeAssignment 實體。)</span><span class="sxs-lookup"><span data-stu-id="cac58-223">(Because this is a one-to-zero-or-one relationship, there might not be a related OfficeAssignment entity.)</span></span>

  ```html
  @if (item.OfficeAssignment != null)
  {
      @item.OfficeAssignment.Location
  }
  ```

* <span data-ttu-id="cac58-224">新增 [課程] 資料行，以顯示每位講師所教授的課程。</span><span class="sxs-lookup"><span data-stu-id="cac58-224">Added a **Courses** column that displays courses taught by each instructor.</span></span> <span data-ttu-id="cac58-225">如需此 Razor 語法的詳細資訊，請參閱[使用 `@:` 的明確行轉換](xref:mvc/views/razor#explicit-line-transition-with-)。</span><span class="sxs-lookup"><span data-stu-id="cac58-225">See [Explicit Line Transition with `@:`](xref:mvc/views/razor#explicit-line-transition-with-) for more about this razor syntax.</span></span>

* <span data-ttu-id="cac58-226">新增程式碼，將 `class="success"` 動態新增至所選取講師的 `tr` 項目。</span><span class="sxs-lookup"><span data-stu-id="cac58-226">Added code that dynamically adds `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="cac58-227">這會使用啟動程序類別設定所選取資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="cac58-227">This sets a background color for the selected row using a Bootstrap class.</span></span>

  ```html
  string selectedRow = "";
  if (item.ID == (int?)ViewData["InstructorID"])
  {
      selectedRow = "success";
  }
  <tr class="@selectedRow">
  ```

* <span data-ttu-id="cac58-228">在每個資料列的其他連結之前，立即新增標示為**選取**的超連結，這會使得選取的講師識別碼傳送到 `Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="cac58-228">Added a new hyperlink labeled **Select** immediately before the other links in each row, which causes the selected instructor's ID to be sent to the `Index` method.</span></span>

  ```html
  <a asp-action="Index" asp-route-id="@item.ID">Select</a> |
  ```

<span data-ttu-id="cac58-229">執行應用程式並選取 [講師]  索引標籤。沒有相關的 OfficeAssignment 實體時，此頁面就會顯示相關 OfficeAssignment 實體的 Location 屬性和空的資料表儲存格。</span><span class="sxs-lookup"><span data-stu-id="cac58-229">Run the app and select the **Instructors** tab. The page displays the Location property of related OfficeAssignment entities and an empty table cell when there's no related OfficeAssignment entity.</span></span>

![Instructors [索引] 頁面未選取任何項目](read-related-data/_static/instructors-index-no-selection.png)

<span data-ttu-id="cac58-231">在 *Views/Instructors/Index.cshtml* 檔案的結束資料表項目 (在檔案的結尾) 之後，新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="cac58-231">In the *Views/Instructors/Index.cshtml* file, after the closing table element (at the end of the file), add the following code.</span></span> <span data-ttu-id="cac58-232">當選取講師時，此程式碼會顯示與講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="cac58-232">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=66-101)]

<span data-ttu-id="cac58-233">此程式碼會讀取檢視模型的 `Courses` 屬性以顯示課程清單。</span><span class="sxs-lookup"><span data-stu-id="cac58-233">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="cac58-234">它也會提供**選取**超連結，將所選取課程的識別碼傳送至 `Index` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="cac58-234">It also provides a **Select** hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="cac58-235">重新整理頁面，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="cac58-235">Refresh the page and select an instructor.</span></span> <span data-ttu-id="cac58-236">現在您會看到一個方格，其中顯示指派給所選取講師的課程，而且在每個課程中，您可以看到指派的部門名稱。</span><span class="sxs-lookup"><span data-stu-id="cac58-236">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instructors [索引] 頁面已選取講師](read-related-data/_static/instructors-index-instructor-selected.png)

<span data-ttu-id="cac58-238">在您剛才新增的程式碼區塊之後，新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="cac58-238">After the code block you just added, add the following code.</span></span> <span data-ttu-id="cac58-239">這會在選取課程時，顯示已註冊該課程的學生清單。</span><span class="sxs-lookup"><span data-stu-id="cac58-239">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-html[](intro/samples/cu/Views/Instructors/Index1.cshtml?range=103-125)]

<span data-ttu-id="cac58-240">此程式碼會讀取檢視模型的 Enrollments 屬性，以顯示已註冊課程的學生清單。</span><span class="sxs-lookup"><span data-stu-id="cac58-240">This code reads the Enrollments property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="cac58-241">再次重新整理頁面，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="cac58-241">Refresh the page again and select an instructor.</span></span> <span data-ttu-id="cac58-242">接著選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="cac58-242">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instructors [索引] 頁面已選取講師和課程](read-related-data/_static/instructors-index.png)

## <a name="explicit-loading"></a><span data-ttu-id="cac58-244">明確式載入</span><span class="sxs-lookup"><span data-stu-id="cac58-244">Explicit loading</span></span>

<span data-ttu-id="cac58-245">當您在 *InstructorsController.cs* 中擷取講師清單時，已針對 `CourseAssignments` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="cac58-245">When you retrieved the list of instructors in *InstructorsController.cs*, you specified eager loading for the `CourseAssignments` navigation property.</span></span>

<span data-ttu-id="cac58-246">假設您預期使用者很少想要查看所選取講師和課程中的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="cac58-246">Suppose you expected users to only rarely want to see enrollments in a selected instructor and course.</span></span> <span data-ttu-id="cac58-247">在此情況下，您可能想要只在要求時才載入註冊資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-247">In that case, you might want to load the enrollment data only if it's requested.</span></span> <span data-ttu-id="cac58-248">若要查看如何執行明確式載入的範例，請以下列程式碼取代 `Index` 方法，這會移除 Enrollments 的積極式載入，並明確地載入該屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-248">To see an example of how to do explicit loading, replace the `Index` method with the following code, which removes eager loading for Enrollments and loads that property explicitly.</span></span> <span data-ttu-id="cac58-249">程式碼變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="cac58-249">The code changes are highlighted.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/InstructorsController.cs?name=snippet_ExplicitLoading&highlight=23-29)]

<span data-ttu-id="cac58-250">新的程式碼會從擷取 Instructor 實體的程式碼中，捨棄用於註冊資料的 *ThenInclude* 方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="cac58-250">The new code drops the *ThenInclude* method calls for enrollment data from the code that retrieves instructor entities.</span></span> <span data-ttu-id="cac58-251">如果選取了講師和課程，醒示提示的程式碼就會針對所選取的課程擷取 Enrollment 實體，並針對每個 Enrollment 擷取 Student 實體。</span><span class="sxs-lookup"><span data-stu-id="cac58-251">If an instructor and course are selected, the highlighted code retrieves Enrollment entities for the selected course, and Student entities for each Enrollment.</span></span>

<span data-ttu-id="cac58-252">執行應用程式，並立即移至 Instructors [索引] 頁面；雖然您已變更資料的擷取方式，但您會發現頁面上顯示的內容沒有任何差異。</span><span class="sxs-lookup"><span data-stu-id="cac58-252">Run the app, go to the Instructors Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="cac58-253">總結</span><span class="sxs-lookup"><span data-stu-id="cac58-253">Summary</span></span>

<span data-ttu-id="cac58-254">您現在已使用積極式載入搭配一個查詢與多個查詢，將相關資料讀取到導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="cac58-254">You've now used eager loading with one query and with multiple queries to read related data into navigation properties.</span></span> <span data-ttu-id="cac58-255">在下一個教學課程中，您將了解如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="cac58-255">In the next tutorial you'll learn how to update related data.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="cac58-256">[上一頁](complex-data-model.md)
>[下一頁](update-related-data.md)</span><span class="sxs-lookup"><span data-stu-id="cac58-256">[Previous](complex-data-model.md)
[Next](update-related-data.md)</span></span>  
