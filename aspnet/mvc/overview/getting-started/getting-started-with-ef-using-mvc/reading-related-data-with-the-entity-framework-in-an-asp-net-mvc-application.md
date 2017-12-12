---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: "讀取與 Entity Framework 中的 ASP.NET MVC 應用程式的相關的資料 |Microsoft 文件"
author: tdykstra
description: /ajax/tutorials/using-ajax-control-toolkit-controls-and-control-extenders-vb
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: 18cdd896-8ed9-4547-b143-114711e3eafb
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 1f4912bb3113a8f9cdae4211e055a7e317ab2aff
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application"></a><span data-ttu-id="219bc-103">閱讀相關的 Entity Framework 的 ASP.NET MVC 應用程式中的資料</span><span class="sxs-lookup"><span data-stu-id="219bc-103">Reading Related Data with the Entity Framework in an ASP.NET MVC Application</span></span>
====================
<span data-ttu-id="219bc-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="219bc-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

<span data-ttu-id="219bc-105">[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span><span class="sxs-lookup"><span data-stu-id="219bc-105">[Download Completed Project](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8) or [Download PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)</span></span>

> <span data-ttu-id="219bc-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。</span><span class="sxs-lookup"><span data-stu-id="219bc-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 5 applications using the Entity Framework 6 Code First and Visual Studio 2013.</span></span> <span data-ttu-id="219bc-107">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="219bc-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


<span data-ttu-id="219bc-108">在上一個教學課程中，您會完成學校資料模型。</span><span class="sxs-lookup"><span data-stu-id="219bc-108">In the previous tutorial you completed the School data model.</span></span> <span data-ttu-id="219bc-109">在本教學課程，您將會讀取並顯示相關的資料，也就是，可將 Entity Framework 載入導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-109">In this tutorial you'll read and display related data — that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="219bc-110">下圖顯示的頁面，您將使用。</span><span class="sxs-lookup"><span data-stu-id="219bc-110">The following illustrations show the pages that you'll work with.</span></span>

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a><span data-ttu-id="219bc-112">延遲、 急切，和明確載入相關的資料</span><span class="sxs-lookup"><span data-stu-id="219bc-112">Lazy, Eager, and Explicit Loading of Related Data</span></span>

<span data-ttu-id="219bc-113">有數種方式，Entity Framework 可以將實體的導覽屬性載入相關的資料：</span><span class="sxs-lookup"><span data-stu-id="219bc-113">There are several ways that the Entity Framework can load related data into the navigation properties of an entity:</span></span>

- <span data-ttu-id="219bc-114">*消極式載入*。</span><span class="sxs-lookup"><span data-stu-id="219bc-114">*Lazy loading*.</span></span> <span data-ttu-id="219bc-115">第一次讀取實體時，不被擷取相關的資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-115">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="219bc-116">不過，第一次您嘗試存取的導覽屬性，該導覽屬性所需的資料自動擷取。</span><span class="sxs-lookup"><span data-stu-id="219bc-116">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="219bc-117">這會導致多個查詢傳送至資料庫，一個用於實體本身，一個必須擷取每個相關實體資料的時間。</span><span class="sxs-lookup"><span data-stu-id="219bc-117">This results in multiple queries sent to the database — one for the entity itself and one each time that related data for the entity must be retrieved.</span></span> <span data-ttu-id="219bc-118">`DbContext`類別啟用預設消極式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-118">The `DbContext` class enables lazy loading by default.</span></span> 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- <span data-ttu-id="219bc-120">*積極式載入*。</span><span class="sxs-lookup"><span data-stu-id="219bc-120">*Eager loading*.</span></span> <span data-ttu-id="219bc-121">實體讀取時，同時擷取的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-121">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="219bc-122">這通常會導致單一聯結查詢以擷取所有所需的資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-122">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="219bc-123">使用指定積極式載入`Include`方法。</span><span class="sxs-lookup"><span data-stu-id="219bc-123">You specify eager loading by using the `Include` method.</span></span>

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- <span data-ttu-id="219bc-125">*明確式載入*。</span><span class="sxs-lookup"><span data-stu-id="219bc-125">*Explicit loading*.</span></span> <span data-ttu-id="219bc-126">這是類似於延遲載入，不同之處在於您明確地擷取相關的資料中的程式碼;當您存取導覽屬性時，它不會發生自動。</span><span class="sxs-lookup"><span data-stu-id="219bc-126">This is similar to lazy loading, except that you explicitly retrieve the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="219bc-127">您載入相關的資料手動透過取得物件狀態管理員項目實體和呼叫[Collection.Load](https://msdn.microsoft.com/en-us/library/gg696220(v=vs.103).aspx)集合的方法或[Reference.Load](https://msdn.microsoft.com/en-us/library/gg679166(v=vs.103).aspx)保存的屬性方法單一實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-127">You load related data manually by getting the object state manager entry for an entity and calling the [Collection.Load](https://msdn.microsoft.com/en-us/library/gg696220(v=vs.103).aspx) method for collections or the [Reference.Load](https://msdn.microsoft.com/en-us/library/gg679166(v=vs.103).aspx) method for properties that hold a single entity.</span></span> <span data-ttu-id="219bc-128">(在下列範例中，如果您想要載入的系統管理員導覽屬性，您必須取代`Collection(x => x.Courses)`與`Reference(x => x.Administrator)`。)通常您會使用您已開啟消極式載入關閉時，才明確載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-128">(In the following example, if you wanted to load the Administrator navigation property, you'd replace `Collection(x => x.Courses)` with `Reference(x => x.Administrator)`.) Typically you'd use explicit loading only when you've turned lazy loading off.</span></span>

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

<span data-ttu-id="219bc-130">因為它們不會立即擷取的屬性值，消極式載入和明確載入也都稱為*延後載入*。</span><span class="sxs-lookup"><span data-stu-id="219bc-130">Because they don't immediately retrieve the property values, lazy loading and explicit loading are also both known as *deferred loading*.</span></span>

### <a name="performance-considerations"></a><span data-ttu-id="219bc-131">效能考量</span><span class="sxs-lookup"><span data-stu-id="219bc-131">Performance considerations</span></span>

<span data-ttu-id="219bc-132">如果您知道您需要針對擷取每個實體的相關的資料，積極式載入通常提供最佳效能，因為傳送至資料庫的單一查詢通常比針對擷取每個實體個別查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="219bc-132">If you know you need related data for every entity retrieved, eager loading often offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="219bc-133">例如，在上述範例中，假設每個部門各有十個相關的課程。</span><span class="sxs-lookup"><span data-stu-id="219bc-133">For example, in the above examples, suppose that each department has ten related courses.</span></span> <span data-ttu-id="219bc-134">積極式載入範例會產生單一 （加入） 查詢和單一來回行程到資料庫。</span><span class="sxs-lookup"><span data-stu-id="219bc-134">The eager loading example would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="219bc-135">消極式載入和明確載入範例會同時產生十一個查詢和 11 個往返資料庫。</span><span class="sxs-lookup"><span data-stu-id="219bc-135">The lazy loading and explicit loading examples would both result in eleven queries and eleven round trips to the database.</span></span> <span data-ttu-id="219bc-136">延遲性很高時，特別是危害到效能額外反覆存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="219bc-136">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="219bc-137">相反地，在某些情況下會更有效率消極式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-137">On the other hand, in some scenarios lazy loading is more efficient.</span></span> <span data-ttu-id="219bc-138">積極式載入可能會導致非常複雜聯結來產生，SQL Server 無法有效率地進行處理。</span><span class="sxs-lookup"><span data-stu-id="219bc-138">Eager loading might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="219bc-139">或者，如果您需要存取實體的導覽屬性，只會針對一組實體的子集正在處理，消極式載入可能會更好因為積極式載入會擷取比您所需的更多資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-139">Or if you need to access an entity's navigation properties only for a subset of a set of the entities you're processing, lazy loading might perform better because eager loading would retrieve more data than you need.</span></span> <span data-ttu-id="219bc-140">如果效能嚴重不足，所以最好先測試效能才能做出最好的選擇這兩種方式。</span><span class="sxs-lookup"><span data-stu-id="219bc-140">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

<span data-ttu-id="219bc-141">消極式載入，就能遮罩會造成效能問題的程式碼。</span><span class="sxs-lookup"><span data-stu-id="219bc-141">Lazy loading can mask code that causes performance problems.</span></span> <span data-ttu-id="219bc-142">例如，程式碼不會指定立即或明確載入但處理高容量的實體，而且每個反覆項目中使用數個導覽屬性可能非常沒有效率 （因為許多反覆存取的資料庫）。</span><span class="sxs-lookup"><span data-stu-id="219bc-142">For example, code that doesn't specify eager or explicit loading but processes a high volume of entities and uses several navigation properties in each iteration might be very inefficient (because of many round trips to the database).</span></span> <span data-ttu-id="219bc-143">在開發使用在內部部署 SQL server 中執行的應用程式可能會發生效能問題時移至 Azure SQL Database，因為增加的延遲及消極式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-143">An application that performs well in development using an on premise SQL server might have performance problems when moved to Azure SQL Database due to the increased latency and lazy loading.</span></span> <span data-ttu-id="219bc-144">程式碼剖析實際的測試負載的資料庫查詢，可協助您判斷是否適用消極式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-144">Profiling the database queries with a realistic test load will help you determine if lazy loading is appropriate.</span></span> <span data-ttu-id="219bc-145">如需詳細資訊，請參閱[Demystifying 實體架構策略： 載入相關資料](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx)和[使用 Entity Framework 來減少網路延遲到 SQL Azure](https://msdn.microsoft.com/en-us/magazine/gg309181.aspx)。</span><span class="sxs-lookup"><span data-stu-id="219bc-145">For more information see [Demystifying Entity Framework Strategies: Loading Related Data](https://msdn.microsoft.com/en-us/magazine/hh205756.aspx) and [Using the Entity Framework to Reduce Network Latency to SQL Azure](https://msdn.microsoft.com/en-us/magazine/gg309181.aspx).</span></span>

### <a name="disable-lazy-loading-before-serialization"></a><span data-ttu-id="219bc-146">停用序列化之前的延遲載入</span><span class="sxs-lookup"><span data-stu-id="219bc-146">Disable lazy loading before serialization</span></span>

<span data-ttu-id="219bc-147">如果您離開消極式載入啟用序列化期間，您可以得到查詢比您預期多很多資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-147">If you leave lazy loading enabled during serialization, you can end up querying significantly more data than you intended.</span></span> <span data-ttu-id="219bc-148">序列化通常運作所存取之型別的執行個體上的每個屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-148">Serialization generally works by accessing each property on an instance of a type.</span></span> <span data-ttu-id="219bc-149">屬性存取觸發記憶體消極式載入，且會序列化這些延遲載入的實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-149">Property access triggers lazy loading, and those lazy loaded entities are serialized.</span></span> <span data-ttu-id="219bc-150">序列化程序會存取每一個屬性的延遲載入的實體，而可能造成更多的消極式載入和序列化。</span><span class="sxs-lookup"><span data-stu-id="219bc-150">The serialization process then accesses each property of the lazy-loaded entities, potentially causing even more lazy loading and serialization.</span></span> <span data-ttu-id="219bc-151">若要避免 run-away 鏈結反應，請開啟消極式載入關閉之前序列化實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-151">To prevent this run-away chain reaction, turn lazy loading off before you serialize an entity.</span></span>

<span data-ttu-id="219bc-152">序列化也變得複雜由 Entity Framework 使用，proxy 類別中所述[進階案例的教學課程](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies)。</span><span class="sxs-lookup"><span data-stu-id="219bc-152">Serialization can also be complicated by the proxy classes that the Entity Framework uses, as explained in the [Advanced Scenarios tutorial](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#proxies).</span></span>

<span data-ttu-id="219bc-153">一種方式以避免發生序列化問題為序列化而不是實體物件的資料傳輸物件 (Dto) 中所示[使用 Web API 和 Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="219bc-153">One way to avoid serialization problems is to serialize data transfer objects (DTOs) instead of entity objects, as shown in the [Using Web API with Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-5.md) tutorial.</span></span>

<span data-ttu-id="219bc-154">如果您不使用 DTOs，您可以停用消極式載入，並避免 proxy 問題，依[停用 proxy 建立](https://msdn.microsoft.com/en-US/data/jj592886.aspx)。</span><span class="sxs-lookup"><span data-stu-id="219bc-154">If you don't use DTOs, you can disable lazy loading and avoid proxy issues by [disabling proxy creation](https://msdn.microsoft.com/en-US/data/jj592886.aspx).</span></span>

<span data-ttu-id="219bc-155">以下是一些其他[方式停用延遲載入](https://msdn.microsoft.com/en-US/data/jj574232):</span><span class="sxs-lookup"><span data-stu-id="219bc-155">Here are some other [ways to disable lazy loading](https://msdn.microsoft.com/en-US/data/jj574232):</span></span>

- <span data-ttu-id="219bc-156">對於特定的導覽屬性，請省略`virtual`關鍵字，當您宣告屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-156">For specific navigation properties, omit the `virtual` keyword when you declare the property.</span></span>
- <span data-ttu-id="219bc-157">對於所有導覽屬性，設定`LazyLoadingEnabled`至`false`，下列程式碼置於內容類別的建構函式：</span><span class="sxs-lookup"><span data-stu-id="219bc-157">For all navigation properties, set `LazyLoadingEnabled` to `false`, put the following code in the constructor of your context class:</span></span> 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

## <a name="create-a-courses-page-that-displays-department-name"></a><span data-ttu-id="219bc-158">建立課程頁面顯示部門名稱</span><span class="sxs-lookup"><span data-stu-id="219bc-158">Create a Courses Page That Displays Department Name</span></span>

<span data-ttu-id="219bc-159">`Course`實體包含導覽屬性，其中包含`Department`課程指派給部門的實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-159">The `Course` entity includes a navigation property that contains the `Department` entity of the department that the course is assigned to.</span></span> <span data-ttu-id="219bc-160">若要指派的部門名稱在清單中顯示的課程，您必須取得`Name`屬性從`Department`中的實體`Course.Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-160">To display the name of the assigned department in a list of courses, you need to get the `Name` property from the `Department` entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="219bc-161">建立名為控制器`CourseController`(不 CoursesController) 的`Course`實體類型，使用的相同選項**的 MVC 5 控制器與檢視，使用 Entity Framework** scaffolder 您稍早對`Student`控制站，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="219bc-161">Create a controller named `CourseController` (not CoursesController) for the `Course` entity type, using the same options for the **MVC 5 Controller with views, using Entity Framework** scaffolder that you did earlier for the `Student` controller, as shown in the following illustration:</span></span>

![Add_Controller_dialog_box_for_Course_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

<span data-ttu-id="219bc-163">開啟*Controllers\CourseController.cs*並查看`Index`方法：</span><span class="sxs-lookup"><span data-stu-id="219bc-163">Open *Controllers\CourseController.cs* and look at the `Index` method:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="219bc-164">自動 scaffolding 已指定的積極式載入`Department`導覽屬性使用`Include`方法。</span><span class="sxs-lookup"><span data-stu-id="219bc-164">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="219bc-165">開啟*Views\Course\Index.cshtml*和範本程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="219bc-165">Open *Views\Course\Index.cshtml* and replace the template code with the following code.</span></span> <span data-ttu-id="219bc-166">所做的變更會反白顯示：</span><span class="sxs-lookup"><span data-stu-id="219bc-166">The changes are highlighted:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,7,14-16,23-25,31-33,40-42)]

<span data-ttu-id="219bc-167">您已對 scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="219bc-167">You've made the following changes to the scaffolded code:</span></span>

- <span data-ttu-id="219bc-168">變更從標題**索引**至**課程**。</span><span class="sxs-lookup"><span data-stu-id="219bc-168">Changed the heading from **Index** to **Courses**.</span></span>
- <span data-ttu-id="219bc-169">加入**數目**顯示的資料行`CourseID`屬性值。</span><span class="sxs-lookup"><span data-stu-id="219bc-169">Added a **Number** column that shows the `CourseID` property value.</span></span> <span data-ttu-id="219bc-170">根據預設，主索引鍵不是建立結構，因為它們通常是無意義的使用者。</span><span class="sxs-lookup"><span data-stu-id="219bc-170">By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="219bc-171">不過，在此情況下的主索引鍵是有意義，而且您想要將其顯示。</span><span class="sxs-lookup"><span data-stu-id="219bc-171">However, in this case the primary key is meaningful and you want to show it.</span></span>
- <span data-ttu-id="219bc-172">移動**部門**至右側的資料行並變更其標題。</span><span class="sxs-lookup"><span data-stu-id="219bc-172">Moved the **Department** column to the right side and changed its heading.</span></span> <span data-ttu-id="219bc-173">Scaffolder 正確選擇顯示`Name`屬性從`Department`實體，但應該是資料行標題的課程頁面在這裡**部門**而**名稱**。</span><span class="sxs-lookup"><span data-stu-id="219bc-173">The scaffolder correctly chose to display the `Name` property from the `Department` entity, but here in the Course page the column heading should be **Department** rather than **Name**.</span></span>

<span data-ttu-id="219bc-174">請注意，[部門] 欄中，針對 scaffold 的程式碼會顯示`Name`屬性`Department`實體載入到`Department`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="219bc-174">Notice that for the Department column, the scaffolded code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml?highlight=2)]

<span data-ttu-id="219bc-175">執行網頁 (選取**課程**Contoso 大學首頁上的索引標籤) 若要查看與部門名稱清單。</span><span class="sxs-lookup"><span data-stu-id="219bc-175">Run the page (select the **Courses** tab on the Contoso University home page) to see the list with department names.</span></span>

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

## <a name="create-an-instructors-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="219bc-177">建立講師頁面，其中顯示 Courses 以及註冊項目</span><span class="sxs-lookup"><span data-stu-id="219bc-177">Create an Instructors Page That Shows Courses and Enrollments</span></span>

<span data-ttu-id="219bc-178">本節中您會建立控制器和檢視`Instructor`實體，才能顯示講師頁面：</span><span class="sxs-lookup"><span data-stu-id="219bc-178">In this section you'll create a controller and view for the `Instructor` entity in order to display the Instructors page:</span></span>

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="219bc-180">此頁面讀取，並會顯示相關的資料以下列方式：</span><span class="sxs-lookup"><span data-stu-id="219bc-180">This page reads and displays related data in the following ways:</span></span>

- <span data-ttu-id="219bc-181">講師清單會顯示相關的資料`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-181">The list of instructors displays related data from the `OfficeAssignment` entity.</span></span> <span data-ttu-id="219bc-182">`Instructor`和`OfficeAssignment`實體是以 0 或-1 一個關聯性中。</span><span class="sxs-lookup"><span data-stu-id="219bc-182">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="219bc-183">您將使用的積極式載入`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-183">You'll use eager loading for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="219bc-184">如前所述，積極式載入時，通常更有效率主資料表的所有擷取的資料列所需的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-184">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="219bc-185">在此情況下，您會想要顯示為顯示的所有講師 office 指派。</span><span class="sxs-lookup"><span data-stu-id="219bc-185">In this case, you want to display office assignments for all displayed instructors.</span></span>
- <span data-ttu-id="219bc-186">當使用者選取相關的講師`Course`實體便會顯示。</span><span class="sxs-lookup"><span data-stu-id="219bc-186">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="219bc-187">`Instructor`和`Course`實體是位於多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="219bc-187">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="219bc-188">您將使用的積極式載入`Course`實體和其相關`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-188">You'll use eager loading for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="219bc-189">在此情況下，消極式載入可能會更有效率因為您只會針對所選講師需要課程。</span><span class="sxs-lookup"><span data-stu-id="219bc-189">In this case, lazy loading might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="219bc-190">不過，此範例會示範如何使用積極式載入本身是導覽屬性中的實體內的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-190">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>
- <span data-ttu-id="219bc-191">當使用者選取的課程時，相關的資料`Enrollments`顯示實體集。</span><span class="sxs-lookup"><span data-stu-id="219bc-191">When the user selects a course, related data from the `Enrollments` entity set is displayed.</span></span> <span data-ttu-id="219bc-192">`Course`和`Enrollment`實體是位於一個對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="219bc-192">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span> <span data-ttu-id="219bc-193">您要加入的明確載入`Enrollment`實體和其相關`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-193">You'll add explicit loading for `Enrollment` entities and their related `Student` entities.</span></span> <span data-ttu-id="219bc-194">（明確載入不必要的因為消極式載入已啟用，但這會顯示如何執行明確載入函式）。</span><span class="sxs-lookup"><span data-stu-id="219bc-194">(Explicit loading isn't necessary because lazy loading is enabled, but this shows how to do explicit loading.)</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="219bc-195">建立講師索引檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="219bc-195">Create a View Model for the Instructor Index View</span></span>

<span data-ttu-id="219bc-196">講師頁面會顯示三個不同的資料表。</span><span class="sxs-lookup"><span data-stu-id="219bc-196">The Instructors page shows three different tables.</span></span> <span data-ttu-id="219bc-197">因此，您將建立包含三個屬性，每個保留其中一個資料表的資料檢視模型。</span><span class="sxs-lookup"><span data-stu-id="219bc-197">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="219bc-198">在*ViewModels*資料夾中，建立*InstructorIndexData.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="219bc-198">In the *ViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="create-the-instructor-controller-and-views"></a><span data-ttu-id="219bc-199">建立講師控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="219bc-199">Create the Instructor Controller and Views</span></span>

<span data-ttu-id="219bc-200">建立`InstructorController`(不 InstructorsController) 控制器 EF 讀取/寫入動作，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="219bc-200">Create an `InstructorController` (not InstructorsController) controller with EF read/write actions as shown in the following illustration:</span></span>

![Add_Controller_dialog_box_for_Instructor_controller](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="219bc-202">開啟*Controllers\InstructorController.cs*並加入`using`陳述式`ViewModels`命名空間：</span><span class="sxs-lookup"><span data-stu-id="219bc-202">Open *Controllers\InstructorController.cs* and add a `using` statement for the `ViewModels` namespace:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

<span data-ttu-id="219bc-203">Scaffold 中的程式碼`Index`方法會指定僅適用於的積極式載入`OfficeAssignment`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="219bc-203">The scaffolded code in the `Index` method specifies eager loading only for the `OfficeAssignment` navigation property:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="219bc-204">取代`Index`下列的程式碼，以載入其他方法的相關資料，然後放入檢視模型：</span><span class="sxs-lookup"><span data-stu-id="219bc-204">Replace the `Index` method with the following code to load additional related data and put it in the view model:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="219bc-205">該方法會接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`)，提供的識別碼值選取的講師和所選的課程，並將所有必要的資料傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="219bc-205">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course, and passes all of the required data to the view.</span></span> <span data-ttu-id="219bc-206">參數所提供的**選取**頁面上的超連結。</span><span class="sxs-lookup"><span data-stu-id="219bc-206">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

<span data-ttu-id="219bc-207">程式碼首先會建立檢視模型的執行個體，並放在它的講師清單。</span><span class="sxs-lookup"><span data-stu-id="219bc-207">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="219bc-208">程式碼會指定的積極式載入`Instructor.OfficeAssignment`和`Instructor.Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-208">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.Courses` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs?highlight=3-4)]

<span data-ttu-id="219bc-209">第二個`Include`方法載入的課程，並載入每個課程會積極式載入`Course.Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-209">The second `Include` method loads Courses, and for each Course that is loaded it does eager loading for the `Course.Department` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

<span data-ttu-id="219bc-210">如先前所述，不需要積極式載入，但為了改善效能。</span><span class="sxs-lookup"><span data-stu-id="219bc-210">As mentioned previously, eager loading is not required but is done to improve performance.</span></span> <span data-ttu-id="219bc-211">因為檢視一律需要`OfficeAssignment`實體，會更有效率，擷取在相同的查詢。</span><span class="sxs-lookup"><span data-stu-id="219bc-211">Since the view always requires the `OfficeAssignment` entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="219bc-212">`Course`積極式載入優於延遲載入的頁面會顯示頻率課程，而不選取，才是在網頁上，選取講師時需要的實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-212">`Course` entities are required when an instructor is selected in the web page, so eager loading is better than lazy loading only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="219bc-213">如果選取講師識別碼，則選取的講師會擷取從講師中檢視模型的清單。</span><span class="sxs-lookup"><span data-stu-id="219bc-213">If an instructor ID was selected, the selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="219bc-214">檢視模型`Courses`屬性接著會載入與`Course`實體從該講師`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-214">The view model's `Courses` property is then loaded with the `Course` entities from that instructor's `Courses` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

<span data-ttu-id="219bc-215">`Where`方法會傳回集合，但在此情況下準則傳遞至該方法的結果中只有一個`Instructor`所傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-215">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single `Instructor` entity being returned.</span></span> <span data-ttu-id="219bc-216">`Single`方法會將集合轉換成單一`Instructor`實體，可讓您存取與該實體`Courses`屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-216">The `Single` method converts the collection into a single `Instructor` entity, which gives you access to that entity's `Courses` property.</span></span>

<span data-ttu-id="219bc-217">您使用[單一](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.single.aspx)集合，當您知道集合的方法將會有只有一個項目。</span><span class="sxs-lookup"><span data-stu-id="219bc-217">You use the [Single](https://msdn.microsoft.com/en-us/library/system.linq.enumerable.single.aspx) method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="219bc-218">`Single`方法擲回例外狀況，如果是空的集合傳遞給它，或若有多個項目。</span><span class="sxs-lookup"><span data-stu-id="219bc-218">The `Single` method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="219bc-219">替代方式是[SingleOrDefault](https://msdn.microsoft.com/en-us/library/bb342451.aspx)，它會傳回預設值 (`null`在此情況下) 如果集合是空的。</span><span class="sxs-lookup"><span data-stu-id="219bc-219">An alternative is [SingleOrDefault](https://msdn.microsoft.com/en-us/library/bb342451.aspx), which returns a default value (`null` in this case) if the collection is empty.</span></span> <span data-ttu-id="219bc-220">不過，在此情況下，就仍然造成例外狀況 (從嘗試尋找`Courses`屬性`null`參考)，以及例外狀況訊息會較不清楚指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="219bc-220">However, in this case that would still result in an exception (from trying to find a `Courses` property on a `null` reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="219bc-221">當您呼叫`Single`方法，您也可以傳遞中`Where`條件，而不是呼叫`Where`方法分別：</span><span class="sxs-lookup"><span data-stu-id="219bc-221">When you call the `Single` method, you can also pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

<span data-ttu-id="219bc-222">而非：</span><span class="sxs-lookup"><span data-stu-id="219bc-222">Instead of:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="219bc-223">接下來，如果已選取課程，選定的課程從擷取中檢視模型的課程的清單。</span><span class="sxs-lookup"><span data-stu-id="219bc-223">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="219bc-224">然後檢視模型的`Enrollments`屬性載入`Enrollment`實體從該課程`Enrollments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-224">Then the view model's `Enrollments` property is loaded with the `Enrollment` entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

### <a name="modify-the-instructor-index-view"></a><span data-ttu-id="219bc-225">修改講師索引檢視表</span><span class="sxs-lookup"><span data-stu-id="219bc-225">Modify the Instructor Index View</span></span>

<span data-ttu-id="219bc-226">在*Views\Instructor\Index.cshtml*，範本程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="219bc-226">In *Views\Instructor\Index.cshtml*, replace the template code with the following code.</span></span> <span data-ttu-id="219bc-227">所做的變更會反白顯示：</span><span class="sxs-lookup"><span data-stu-id="219bc-227">The changes are highlighted:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1,4,14-18,21,23-28,38-43,45)]

<span data-ttu-id="219bc-228">您已對現有的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="219bc-228">You've made the following changes to the existing code:</span></span>

- <span data-ttu-id="219bc-229">變更模型類別`InstructorIndexData`。</span><span class="sxs-lookup"><span data-stu-id="219bc-229">Changed the model class to `InstructorIndexData`.</span></span>
- <span data-ttu-id="219bc-230">變更頁面標題從**索引**至**講師**。</span><span class="sxs-lookup"><span data-stu-id="219bc-230">Changed the page title from **Index** to **Instructors**.</span></span>
- <span data-ttu-id="219bc-231">加入**Office**顯示之資料行`item.OfficeAssignment.Location`才`item.OfficeAssignment`不是 null。</span><span class="sxs-lookup"><span data-stu-id="219bc-231">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="219bc-232">(因為這是以 0 或-1 一個關聯性，可能不會有相關`OfficeAssignment`實體。)</span><span class="sxs-lookup"><span data-stu-id="219bc-232">(Because this is a one-to-zero-or-one relationship, there might not be a related `OfficeAssignment` entity.)</span></span> 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]
- <span data-ttu-id="219bc-233">加入的程式碼將以動態方式新增`class="success"`至`tr`選取講師項目。</span><span class="sxs-lookup"><span data-stu-id="219bc-233">Added code that will dynamically add `class="success"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="219bc-234">這會將選取的資料列使用的背景色彩[Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap)類別。</span><span class="sxs-lookup"><span data-stu-id="219bc-234">This sets a background color for the selected row using a [Bootstrap](../../../../visual-studio/overview/2013/creating-web-projects-in-visual-studio.md#bootstrap) class.</span></span> 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]
- <span data-ttu-id="219bc-235">加入新`ActionLink`標示為**選取**之前中每個資料列的其他連結，讓所選的講師識別碼傳送至`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="219bc-235">Added a new `ActionLink` labeled **Select** immediately before the other links in each row, which causes the selected instructor ID to be sent to the `Index` method.</span></span>

<span data-ttu-id="219bc-236">執行應用程式並選取**講師** 索引標籤。此頁面會顯示`Location`屬性的相關`OfficeAssignment`實體和空的資料表資料格時沒有相關`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="219bc-236">Run the application and select the **Instructors** tab. The page displays the `Location` property of related `OfficeAssignment` entities and an empty table cell when there's no related `OfficeAssignment` entity.</span></span>

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

<span data-ttu-id="219bc-238">在*Views\Instructor\Index.cshtml*檔案，關閉後的`table`元素 （在檔案的結尾），加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="219bc-238">In the *Views\Instructor\Index.cshtml* file, after the closing `table` element (at the end of the file), add the following code.</span></span> <span data-ttu-id="219bc-239">此程式碼會顯示一份時選取講師講師相關的課程。</span><span class="sxs-lookup"><span data-stu-id="219bc-239">This code displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

<span data-ttu-id="219bc-240">此程式碼讀取`Courses`要顯示的課程清單的檢視模型的屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-240">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="219bc-241">它也提供`Select`超連結，將傳送至所選的課程識別碼`Index`動作方法。</span><span class="sxs-lookup"><span data-stu-id="219bc-241">It also provides a `Select` hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

<span data-ttu-id="219bc-242">執行網頁，然後選取 instructor。</span><span class="sxs-lookup"><span data-stu-id="219bc-242">Run the page and select an instructor.</span></span> <span data-ttu-id="219bc-243">現在您會看到一個方格，其中會顯示指派給所選講師，課程，每個課程中，您看到指派的部門名稱。</span><span class="sxs-lookup"><span data-stu-id="219bc-243">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

<span data-ttu-id="219bc-245">之後您剛才加入的程式碼區塊，加入下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="219bc-245">After the code block you just added, add the following code.</span></span> <span data-ttu-id="219bc-246">在課程中，選取該課程時，這會顯示一份學生註冊。</span><span class="sxs-lookup"><span data-stu-id="219bc-246">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

<span data-ttu-id="219bc-247">此程式碼讀取`Enrollments`屬性的檢視模型，以顯示一份學生註冊課程。</span><span class="sxs-lookup"><span data-stu-id="219bc-247">This code reads the `Enrollments` property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="219bc-248">執行網頁，然後選取 instructor。</span><span class="sxs-lookup"><span data-stu-id="219bc-248">Run the page and select an instructor.</span></span> <span data-ttu-id="219bc-249">然後，選取課程來查看已註冊的學生版和其等級清單。</span><span class="sxs-lookup"><span data-stu-id="219bc-249">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

### <a name="adding-explicit-loading"></a><span data-ttu-id="219bc-251">新增明確式載入</span><span class="sxs-lookup"><span data-stu-id="219bc-251">Adding Explicit Loading</span></span>

<span data-ttu-id="219bc-252">開啟*InstructorController.cs*並查看如何`Index`方法取得註冊項目所選課程的清單：</span><span class="sxs-lookup"><span data-stu-id="219bc-252">Open *InstructorController.cs* and look at how the `Index` method gets the list of enrollments for a selected course:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs)]

<span data-ttu-id="219bc-253">當您擷取講師的清單時，您指定的積極式載入`Courses`導覽屬性以及`Department`屬性的每個課程。</span><span class="sxs-lookup"><span data-stu-id="219bc-253">When you retrieved the list of instructors, you specified eager loading for the `Courses` navigation property and for the `Department` property of each course.</span></span> <span data-ttu-id="219bc-254">然後您將`Courses`集合中檢視模型，並且現在正在存取`Enrollments`該集合中的某個實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-254">Then you put the `Courses` collection in the view model, and now you're accessing the `Enrollments` navigation property from one entity in that collection.</span></span> <span data-ttu-id="219bc-255">因為您沒有指定的積極式載入`Course.Enrollments`導覽屬性，該屬性的資料出現在頁面中，由於消極式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-255">Because you didn't specify eager loading for the `Course.Enrollments` navigation property, the data from that property is appearing in the page as a result of lazy loading.</span></span>

<span data-ttu-id="219bc-256">如果您不需要變更任何其他方式中的程式碼停用延遲載入`Enrollments`屬性將會是 null，不論課程實際上有多少個註冊項目。</span><span class="sxs-lookup"><span data-stu-id="219bc-256">If you disabled lazy loading without changing the code in any other way, the `Enrollments` property would be null regardless of how many enrollments the course actually had.</span></span> <span data-ttu-id="219bc-257">在此情況下，載入`Enrollments`屬性，您必須指定積極式載入或明確式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-257">In that case, to load the `Enrollments` property, you'd have to specify either eager loading or explicit loading.</span></span> <span data-ttu-id="219bc-258">您已經了解如何進行積極式載入。</span><span class="sxs-lookup"><span data-stu-id="219bc-258">You've already seen how to do eager loading.</span></span> <span data-ttu-id="219bc-259">若要查看明確載入的範例，取代`Index`方法，以下列程式碼，明確載入`Enrollments`屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-259">In order to see an example of explicit loading, replace the `Index` method with the following code, which explicitly loads the `Enrollments` property.</span></span> <span data-ttu-id="219bc-260">變更的程式碼會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="219bc-260">The code changed are highlighted.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs?highlight=20-31)]

<span data-ttu-id="219bc-261">取得所選之後`Course`新的程式碼的實體明確載入該課程`Enrollments`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="219bc-261">After getting the selected `Course` entity, the new code explicitly loads that course's `Enrollments` navigation property:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

<span data-ttu-id="219bc-262">然後它會明確地載入每個`Enrollment`實體的相關`Student`實體：</span><span class="sxs-lookup"><span data-stu-id="219bc-262">Then it explicitly loads each `Enrollment` entity's related `Student` entity:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="219bc-263">請注意，您使用`Collection`方法以載入集合屬性，但為了保留一個實體的屬性，您使用`Reference`方法。</span><span class="sxs-lookup"><span data-stu-id="219bc-263">Notice that you use the `Collection` method to load a collection property, but for a property that holds just one entity, you use the `Reference` method.</span></span>

<span data-ttu-id="219bc-264">現在就執行講師索引頁，雖然您已變更資料擷取的方式，您會看到任何差異顯示在頁面上。</span><span class="sxs-lookup"><span data-stu-id="219bc-264">Run the Instructor Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="219bc-265">總結</span><span class="sxs-lookup"><span data-stu-id="219bc-265">Summary</span></span>

<span data-ttu-id="219bc-266">您現在使用所有三種方式 （延遲、 急切評估，以及明確） 相關的資料載入至導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="219bc-266">You've now used all three ways (lazy, eager, and explicit) to load related data into navigation properties.</span></span> <span data-ttu-id="219bc-267">在下一個教學課程中，您將學習如何更新相關的資料。</span><span class="sxs-lookup"><span data-stu-id="219bc-267">In the next tutorial you'll learn how to update related data.</span></span>

<span data-ttu-id="219bc-268">請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="219bc-268">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="219bc-269">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="219bc-269">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span>

<span data-ttu-id="219bc-270">Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="219bc-270">Links to other Entity Framework resources can be found in the [ASP.NET Data Access - Recommended Resources](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="219bc-271">[上一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[下一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="219bc-271">[Previous](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Next](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
