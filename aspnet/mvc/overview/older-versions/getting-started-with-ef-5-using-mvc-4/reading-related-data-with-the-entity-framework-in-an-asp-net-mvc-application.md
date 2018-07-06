---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
title: 讀取相關的資料，使用 Entity Framework 中的 ASP.NET MVC 應用程式 (5 為 10) |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 0d6fb83b-71f7-425d-8dec-981197d7ec42
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 750c49572e99c6ab8c84d65e4c18f8da9b5d304c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835242"
---
<a name="reading-related-data-with-the-entity-framework-in-an-aspnet-mvc-application-5-of-10"></a><span data-ttu-id="14a4c-103">讀取相關資料，使用 Entity Framework 中的 ASP.NET MVC 應用程式 (5 為 10)</span><span class="sxs-lookup"><span data-stu-id="14a4c-103">Reading Related Data with the Entity Framework in an ASP.NET MVC Application (5 of 10)</span></span>
====================
<span data-ttu-id="14a4c-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="14a4c-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="14a4c-105">下載已完成的專案</span><span class="sxs-lookup"><span data-stu-id="14a4c-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="14a4c-106">Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。</span><span class="sxs-lookup"><span data-stu-id="14a4c-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="14a4c-107">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。</span><span class="sxs-lookup"><span data-stu-id="14a4c-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span> <span data-ttu-id="14a4c-108">您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。</span><span class="sxs-lookup"><span data-stu-id="14a4c-108">You can start the tutorial series from the beginning or [download a starter project for this chapter](building-the-ef5-mvc4-chapter-downloads.md) and start here.</span></span>
> 
> > [!NOTE] 
> > 
> > <span data-ttu-id="14a4c-109">如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。</span><span class="sxs-lookup"><span data-stu-id="14a4c-109">If you run into a problem you can't resolve, [download the completed chapter](building-the-ef5-mvc4-chapter-downloads.md) and try to reproduce your problem.</span></span> <span data-ttu-id="14a4c-110">您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。</span><span class="sxs-lookup"><span data-stu-id="14a4c-110">You can generally find the solution to the problem by comparing your code to the completed code.</span></span> <span data-ttu-id="14a4c-111">一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span><span class="sxs-lookup"><span data-stu-id="14a4c-111">For some common errors and how to solve them, see [Errors and Workarounds.](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)</span></span>


<span data-ttu-id="14a4c-112">在上一個教學課程，您已完成 School 資料模型的內容。</span><span class="sxs-lookup"><span data-stu-id="14a4c-112">In the previous tutorial you completed the School data model.</span></span> <span data-ttu-id="14a4c-113">在本教學課程中，您將讀取並顯示相關的資料-也就是 Entity Framework 載入到導覽屬性的資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-113">In this tutorial you'll read and display related data — that is, data that the Entity Framework loads into navigation properties.</span></span>

<span data-ttu-id="14a4c-114">下列圖例顯示了您將操作的頁面。</span><span class="sxs-lookup"><span data-stu-id="14a4c-114">The following illustrations show the pages that you'll work with.</span></span>

![](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="lazy-eager-and-explicit-loading-of-related-data"></a><span data-ttu-id="14a4c-116">延遲，積極式，明確載入相關資料</span><span class="sxs-lookup"><span data-stu-id="14a4c-116">Lazy, Eager, and Explicit Loading of Related Data</span></span>

<span data-ttu-id="14a4c-117">有數種方式，Entity Framework，可將相關的資料載入實體的導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="14a4c-117">There are several ways that the Entity Framework can load related data into the navigation properties of an entity:</span></span>

- <span data-ttu-id="14a4c-118">*消極式載入*。</span><span class="sxs-lookup"><span data-stu-id="14a4c-118">*Lazy loading*.</span></span> <span data-ttu-id="14a4c-119">第一次讀取實體時，不會擷取相關資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-119">When the entity is first read, related data isn't retrieved.</span></span> <span data-ttu-id="14a4c-120">不過，第一次嘗試存取導覽屬性時，將會自動擷取該導覽屬性所需的資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-120">However, the first time you attempt to access a navigation property, the data required for that navigation property is automatically retrieved.</span></span> <span data-ttu-id="14a4c-121">這會導致多個查詢傳送至資料庫，一個用於實體本身，一個必須擷取每個相關實體資料的時間。</span><span class="sxs-lookup"><span data-stu-id="14a4c-121">This results in multiple queries sent to the database — one for the entity itself and one each time that related data for the entity must be retrieved.</span></span> 

    ![Lazy_loading_example](https://asp.net/media/2577850/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Lazy_loading_example_2c44eabb-5fd3-485a-837d-8e3d053f2c0c.png)
- <span data-ttu-id="14a4c-123">*積極式載入*。</span><span class="sxs-lookup"><span data-stu-id="14a4c-123">*Eager loading*.</span></span> <span data-ttu-id="14a4c-124">讀取實體時，將會同時擷取其相關資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-124">When the entity is read, related data is retrieved along with it.</span></span> <span data-ttu-id="14a4c-125">這通常會導致單一聯結查詢，其可擷取所有需要的資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-125">This typically results in a single join query that retrieves all of the data that's needed.</span></span> <span data-ttu-id="14a4c-126">使用指定積極式載入`Include`方法。</span><span class="sxs-lookup"><span data-stu-id="14a4c-126">You specify eager loading by using the `Include` method.</span></span>

    ![Eager_loading_example](https://asp.net/media/2577856/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Eager_loading_example_33f907ff-f0b0-4057-8e75-05a8cacac807.png)
- <span data-ttu-id="14a4c-128">*明確式載入*。</span><span class="sxs-lookup"><span data-stu-id="14a4c-128">*Explicit loading*.</span></span> <span data-ttu-id="14a4c-129">這是類似於消極式載入，不同之處在於您明確地擷取相關的資料，在程式碼當您存取導覽屬性時，它不會發生自動。</span><span class="sxs-lookup"><span data-stu-id="14a4c-129">This is similar to lazy loading, except that you explicitly retrieve the related data in code; it doesn't happen automatically when you access a navigation property.</span></span> <span data-ttu-id="14a4c-130">您載入相關的資料手動藉由取得物件狀態管理員項目實體和呼叫`Collection.Load`集合的方法或`Reference.Load`保存單一實體屬性的方法。</span><span class="sxs-lookup"><span data-stu-id="14a4c-130">You load related data manually by getting the object state manager entry for an entity and calling the `Collection.Load` method for collections or the `Reference.Load` method for properties that hold a single entity.</span></span> <span data-ttu-id="14a4c-131">(在下列範例中，如果您想要載入 [管理員] 瀏覽屬性中，您會取代`Collection(x => x.Courses)`與`Reference(x => x.Administrator)`。)</span><span class="sxs-lookup"><span data-stu-id="14a4c-131">(In the following example, if you wanted to load the Administrator navigation property, you'd replace `Collection(x => x.Courses)` with `Reference(x => x.Administrator)`.)</span></span>

    ![Explicit_loading_example](https://asp.net/media/2577862/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Explicit_loading_example_79d8c368-6d82-426f-be9a-2b443644ab15.png)

<span data-ttu-id="14a4c-133">因為它們不立即擷取的屬性值，消極式載入和明確式載入同時所謂*延後載入*。</span><span class="sxs-lookup"><span data-stu-id="14a4c-133">Because they don't immediately retrieve the property values, lazy loading and explicit loading are also both known as *deferred loading*.</span></span>

<span data-ttu-id="14a4c-134">一般情況下，如果您知道您需要相關的資料的每個實體擷取，積極式載入可提供最佳效能，因為傳送至資料庫的單一查詢通常比擷取每個實體的個別查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="14a4c-134">In general, if you know you need related data for every entity retrieved, eager loading offers the best performance, because a single query sent to the database is typically more efficient than separate queries for each entity retrieved.</span></span> <span data-ttu-id="14a4c-135">例如，在上述範例中，假設每個部門有十個相關的課程。</span><span class="sxs-lookup"><span data-stu-id="14a4c-135">For example, in the above examples, suppose that each department has ten related courses.</span></span> <span data-ttu-id="14a4c-136">積極式載入範例都只是單一 （聯結） 查詢和單一來回行程到資料庫。</span><span class="sxs-lookup"><span data-stu-id="14a4c-136">The eager loading example would result in just a single (join) query and a single round trip to the database.</span></span> <span data-ttu-id="14a4c-137">消極式載入和明確式載入範例會同時產生十一個查詢和十一個來回行程到資料庫。</span><span class="sxs-lookup"><span data-stu-id="14a4c-137">The lazy loading and explicit loading examples would both result in eleven queries and eleven round trips to the database.</span></span> <span data-ttu-id="14a4c-138">當延遲很高時，資料庫的額外來回行程對效能特別不利。</span><span class="sxs-lookup"><span data-stu-id="14a4c-138">The extra round trips to the database are especially detrimental to performance when latency is high.</span></span>

<span data-ttu-id="14a4c-139">相反地，在某些情況下會更有效率消極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-139">On the other hand, in some scenarios lazy loading is more efficient.</span></span> <span data-ttu-id="14a4c-140">積極式載入可能會導致非常複雜的聯結產生的 SQL Server 無法有效率地處理。</span><span class="sxs-lookup"><span data-stu-id="14a4c-140">Eager loading might cause a very complex join to be generated, which SQL Server can't process efficiently.</span></span> <span data-ttu-id="14a4c-141">或者如果您需要存取實體的導覽屬性，只會針對一組實體的子集正在處理，消極式載入可能會比較好，因為積極式載入會擷取比您所需的更多資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-141">Or if you need to access an entity's navigation properties only for a subset of a set of entities you're processing, lazy loading might perform better because eager loading would retrieve more data than you need.</span></span> <span data-ttu-id="14a4c-142">如果效能嚴重不足，最好先測試這兩種方式的效能，才能做出最好的選擇。</span><span class="sxs-lookup"><span data-stu-id="14a4c-142">If performance is critical, it's best to test performance both ways in order to make the best choice.</span></span>

<span data-ttu-id="14a4c-143">通常您會使用您已開啟消極式載入關閉時，只明確載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-143">Typically you'd use explicit loading only when you've turned lazy loading off.</span></span> <span data-ttu-id="14a4c-144">您應該開啟消極式載入關閉時的其中一個案例是在序列化期間。</span><span class="sxs-lookup"><span data-stu-id="14a4c-144">One scenario when you should turn lazy loading off is during serialization.</span></span> <span data-ttu-id="14a4c-145">消極式載入及序列化不要混用，而且如果您不小心可能會結束查詢相當多的資料，比您想要時延遲載入已啟用。</span><span class="sxs-lookup"><span data-stu-id="14a4c-145">Lazy loading and serialization don't mix well, and if you aren't careful you can end up querying significantly more data than you intended when lazy loading is enabled.</span></span> <span data-ttu-id="14a4c-146">序列化通常適用於藉由存取類型的執行個體上的每個屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-146">Serialization generally works by accessing each property on an instance of a type.</span></span> <span data-ttu-id="14a4c-147">屬性存取就會觸發消極式載入，並會序列化這些消極式載入的實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-147">Property access triggers lazy loading, and those lazy loaded entities are serialized.</span></span> <span data-ttu-id="14a4c-148">序列化程序之後即可存取的消極式載入的實體，而可能造成更多的消極式載入及序列化的每個屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-148">The serialization process then accesses each property of the lazy-loaded entities, potentially causing even more lazy loading and serialization.</span></span> <span data-ttu-id="14a4c-149">若要避免 run-away 鏈結反應，請開啟消極式載入關閉之前序列化實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-149">To prevent this run-away chain reaction, turn lazy loading off before you serialize an entity.</span></span>

<span data-ttu-id="14a4c-150">資料庫內容類別預設會執行消極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-150">The database context class performs lazy loading by default.</span></span> <span data-ttu-id="14a4c-151">有兩種方式可停用消極式載入：</span><span class="sxs-lookup"><span data-stu-id="14a4c-151">There are two ways to disable lazy loading:</span></span>

- <span data-ttu-id="14a4c-152">針對特定的導覽屬性，請省略`virtual`關鍵字宣告的屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-152">For specific navigation properties, omit the `virtual` keyword when you declare the property.</span></span>
- <span data-ttu-id="14a4c-153">對於所有導覽屬性，來設定`LazyLoadingEnabled`至`false`。</span><span class="sxs-lookup"><span data-stu-id="14a4c-153">For all navigation properties, set `LazyLoadingEnabled` to `false`.</span></span> <span data-ttu-id="14a4c-154">例如，您可以將下列程式碼中您的內容類別的建構函式：</span><span class="sxs-lookup"><span data-stu-id="14a4c-154">For example, you can put the following code in the constructor of your context class:</span></span> 

    [!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

<span data-ttu-id="14a4c-155">消極式載入可加上遮罩會造成效能問題的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14a4c-155">Lazy loading can mask code that causes performance problems.</span></span> <span data-ttu-id="14a4c-156">例如，未指定積極式或明確載入，但處理大量的實體，而且每個反覆項目中使用數個導覽屬性的程式碼可能會非常沒有效率 （因為許多往返資料庫）。</span><span class="sxs-lookup"><span data-stu-id="14a4c-156">For example, code that doesn't specify eager or explicit loading but processes a high volume of entities and uses several navigation properties in each iteration might be very inefficient (because of many round trips to the database).</span></span> <span data-ttu-id="14a4c-157">在開發使用在內部部署 SQL server 中正常執行的應用程式可能會遇到效能問題移到 Azure SQL Database，因為增加的延遲和消極式載入時。</span><span class="sxs-lookup"><span data-stu-id="14a4c-157">An application that performs well in development using an on premise SQL server might have performance problems when moved to Azure SQL Database due to the increased latency and lazy loading.</span></span> <span data-ttu-id="14a4c-158">分析實際的測試負載的資料庫查詢，可協助您判斷是否適當消極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-158">Profiling the database queries with a realistic test load will help you determine if lazy loading is appropriate.</span></span> <span data-ttu-id="14a4c-159">如需詳細資訊，請參閱[揭密 Entity Framework 策略： 載入相關的資料](https://msdn.microsoft.com/magazine/hh205756.aspx)並[使用 Entity Framework 對 SQL Azure 以減少網路延遲](https://msdn.microsoft.com/magazine/gg309181.aspx)。</span><span class="sxs-lookup"><span data-stu-id="14a4c-159">For more information see [Demystifying Entity Framework Strategies: Loading Related Data](https://msdn.microsoft.com/magazine/hh205756.aspx) and [Using the Entity Framework to Reduce Network Latency to SQL Azure](https://msdn.microsoft.com/magazine/gg309181.aspx).</span></span>

## <a name="create-a-courses-index-page-that-displays-department-name"></a><span data-ttu-id="14a4c-160">建立顯示部門名稱的 Courses 索引頁面</span><span class="sxs-lookup"><span data-stu-id="14a4c-160">Create a Courses Index Page That Displays Department Name</span></span>

<span data-ttu-id="14a4c-161">`Course`實體包含導覽屬性，其中包含`Department`指派課程的部門實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-161">The `Course` entity includes a navigation property that contains the `Department` entity of the department that the course is assigned to.</span></span> <span data-ttu-id="14a4c-162">若要在課程清單中顯示所指派部門的名稱，您需要取得`Name`屬性從`Department`中的實體`Course.Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-162">To display the name of the assigned department in a list of courses, you need to get the `Name` property from the `Department` entity that is in the `Course.Department` navigation property.</span></span>

<span data-ttu-id="14a4c-163">建立名為控制器`CourseController`針對`Course`實體類型，使用您先前為相同的選項`Student`控制器，如下圖所示 （除了像映像，您的內容類別都是 DAL 命名空間中，不是模型命名空間）：</span><span class="sxs-lookup"><span data-stu-id="14a4c-163">Create a controller named `CourseController` for the `Course` entity type, using the same options that you did earlier for the `Student` controller, as shown in the following illustration (except unlike the image, your context class is in the DAL namespace, not the Models namespace):</span></span>

![Add_Controller_dialog_box_for_Course_controller](https://asp.net/media/2577868/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Course_controller_c167c11e-2d3e-4b64-a2b9-a0b368b8041d.png)

<span data-ttu-id="14a4c-165">開啟*Controllers\CourseController.cs*並查看`Index`方法：</span><span class="sxs-lookup"><span data-stu-id="14a4c-165">Open *Controllers\CourseController.cs* and look at the `Index` method:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

<span data-ttu-id="14a4c-166">自動 Scaffolding 已使用 `Include` 方法，針對 `Department` 導覽屬性指定積極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-166">The automatic scaffolding has specified eager loading for the `Department` navigation property by using the `Include` method.</span></span>

<span data-ttu-id="14a4c-167">開啟*Views\Course\Index.cshtml*並以下列程式碼取代現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14a4c-167">Open *Views\Course\Index.cshtml* and replace the existing code with the following code.</span></span> <span data-ttu-id="14a4c-168">所做的變更已醒目提示：</span><span class="sxs-lookup"><span data-stu-id="14a4c-168">The changes are highlighted:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=4,15,18,28-30)]

<span data-ttu-id="14a4c-169">您已對包含 Scaffold 的程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="14a4c-169">You've made the following changes to the scaffolded code:</span></span>

- <span data-ttu-id="14a4c-170">變更從標題**Index**要**課程**。</span><span class="sxs-lookup"><span data-stu-id="14a4c-170">Changed the heading from **Index** to **Courses**.</span></span>
- <span data-ttu-id="14a4c-171">移至左側的資料列連結。</span><span class="sxs-lookup"><span data-stu-id="14a4c-171">Moved the row links to the left.</span></span>
- <span data-ttu-id="14a4c-172">加入資料行標題底下**數字**，以顯示`CourseID`屬性值。</span><span class="sxs-lookup"><span data-stu-id="14a4c-172">Added a column under the heading **Number** that shows the `CourseID` property value.</span></span> <span data-ttu-id="14a4c-173">(根據預設，主索引鍵不會進行 scaffold 因為它們通常是使用者沒有任何意義。</span><span class="sxs-lookup"><span data-stu-id="14a4c-173">(By default, primary keys aren't scaffolded because normally they are meaningless to end users.</span></span> <span data-ttu-id="14a4c-174">不過，在此情況下主索引鍵有意義且您想要顯示它。）</span><span class="sxs-lookup"><span data-stu-id="14a4c-174">However, in this case the primary key is meaningful and you want to show it.)</span></span>
- <span data-ttu-id="14a4c-175">變更從最後一個資料行標題**DepartmentID** (外部索引鍵的名稱`Department`實體) 來**部門**。</span><span class="sxs-lookup"><span data-stu-id="14a4c-175">Changed the last column heading from **DepartmentID** (the name of the foreign key to the `Department` entity) to **Department**.</span></span>

<span data-ttu-id="14a4c-176">請注意，最後一個資料行，scaffold 程式碼會顯示`Name`的屬性`Department`載入的實體`Department`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="14a4c-176">Notice that for the last column, the scaffolded code displays the `Name` property of the `Department` entity that's loaded into the `Department` navigation property:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cshtml)]

<span data-ttu-id="14a4c-177">執行網頁 (選取**課程**的 Contoso 大學首頁上的索引標籤) 以查看含有部門名稱的清單。</span><span class="sxs-lookup"><span data-stu-id="14a4c-177">Run the page (select the **Courses** tab on the Contoso University home page) to see the list with department names.</span></span>

![Courses_index_page_with_department_names](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="create-an-instructors-index-page-that-shows-courses-and-enrollments"></a><span data-ttu-id="14a4c-179">建立顯示課程和註冊的講師索引頁面</span><span class="sxs-lookup"><span data-stu-id="14a4c-179">Create an Instructors Index Page That Shows Courses and Enrollments</span></span>

<span data-ttu-id="14a4c-180">在本節中，您會建立控制器並檢視`Instructor`實體以顯示 Instructors [索引] 頁面：</span><span class="sxs-lookup"><span data-stu-id="14a4c-180">In this section you'll create a controller and view for the `Instructor` entity in order to display the Instructors Index page:</span></span>

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

<span data-ttu-id="14a4c-182">此頁面將以下列方式讀取和顯示相關資料：</span><span class="sxs-lookup"><span data-stu-id="14a4c-182">This page reads and displays related data in the following ways:</span></span>

- <span data-ttu-id="14a4c-183">講師清單會顯示相關的資料，從`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-183">The list of instructors displays related data from the `OfficeAssignment` entity.</span></span> <span data-ttu-id="14a4c-184">`Instructor` 與 `OfficeAssignment` 實體具有一對零或一關聯性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-184">The `Instructor` and `OfficeAssignment` entities are in a one-to-zero-or-one relationship.</span></span> <span data-ttu-id="14a4c-185">您將使用積極式載入`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-185">You'll use eager loading for the `OfficeAssignment` entities.</span></span> <span data-ttu-id="14a4c-186">如上所述，當您需要主要資料表中所有擷取資料列的相關資料時，積極式載入通常更有效率。</span><span class="sxs-lookup"><span data-stu-id="14a4c-186">As explained earlier, eager loading is typically more efficient when you need the related data for all retrieved rows of the primary table.</span></span> <span data-ttu-id="14a4c-187">在此情況下，您可能想要顯示所有已呈現講師的辦公室指派。</span><span class="sxs-lookup"><span data-stu-id="14a4c-187">In this case, you want to display office assignments for all displayed instructors.</span></span>
- <span data-ttu-id="14a4c-188">當使用者選取講師，相關`Course`實體便會顯示。</span><span class="sxs-lookup"><span data-stu-id="14a4c-188">When the user selects an instructor, related `Course` entities are displayed.</span></span> <span data-ttu-id="14a4c-189">`Instructor` 與 `Course` 實體具有多對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-189">The `Instructor` and `Course` entities are in a many-to-many relationship.</span></span> <span data-ttu-id="14a4c-190">您將使用積極式載入`Course`實體和其相關`Department`實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-190">You'll use eager loading for the `Course` entities and their related `Department` entities.</span></span> <span data-ttu-id="14a4c-191">在此情況下，消極式載入可能會更有效率因為您需要只會針對所選講師的課程。</span><span class="sxs-lookup"><span data-stu-id="14a4c-191">In this case, lazy loading might be more efficient because you need courses only for the selected instructor.</span></span> <span data-ttu-id="14a4c-192">不過，這個範例會示範如何在本身處於導覽屬性內的實體中，針對導覽屬性使用積極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-192">However, this example shows how to use eager loading for navigation properties within entities that are themselves in navigation properties.</span></span>
- <span data-ttu-id="14a4c-193">當使用者選取課程時，相關資料`Enrollments`顯示實體集。</span><span class="sxs-lookup"><span data-stu-id="14a4c-193">When the user selects a course, related data from the `Enrollments` entity set is displayed.</span></span> <span data-ttu-id="14a4c-194">`Course` 與 `Enrollment` 實體具有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-194">The `Course` and `Enrollment` entities are in a one-to-many relationship.</span></span> <span data-ttu-id="14a4c-195">您將新增的明確式載入`Enrollment`實體和其相關`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-195">You'll add explicit loading for `Enrollment` entities and their related `Student` entities.</span></span> <span data-ttu-id="14a4c-196">（明確式載入不必要的因為已啟用消極式載入，但這會顯示如何執行明確式載入函式）。</span><span class="sxs-lookup"><span data-stu-id="14a4c-196">(Explicit loading isn't necessary because lazy loading is enabled, but this shows how to do explicit loading.)</span></span>

### <a name="create-a-view-model-for-the-instructor-index-view"></a><span data-ttu-id="14a4c-197">建立 Instructor [索引] 檢視的檢視模型</span><span class="sxs-lookup"><span data-stu-id="14a4c-197">Create a View Model for the Instructor Index View</span></span>

<span data-ttu-id="14a4c-198">Instructor 索引 頁面會顯示三個不同的資料表。</span><span class="sxs-lookup"><span data-stu-id="14a4c-198">The Instructor Index page shows three different tables.</span></span> <span data-ttu-id="14a4c-199">因此，您將建立包含三個屬性的檢視模型，每個保留其中一個資料表的資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-199">Therefore, you'll create a view model that includes three properties, each holding the data for one of the tables.</span></span>

<span data-ttu-id="14a4c-200">在  *Viewmodel*資料夾中，建立*InstructorIndexData.cs*並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="14a4c-200">In the *ViewModels* folder, create *InstructorIndexData.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

### <a name="adding-a-style-for-selected-rows"></a><span data-ttu-id="14a4c-201">加入選取的資料列的樣式</span><span class="sxs-lookup"><span data-stu-id="14a4c-201">Adding a Style for Selected Rows</span></span>

<span data-ttu-id="14a4c-202">若要將標示選取的資料列中，您需要不同的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="14a4c-202">To mark selected rows you need a different background color.</span></span> <span data-ttu-id="14a4c-203">若要提供此 UI 的樣式，將下列反白顯示的程式碼新增至區段`/* info and errors */`中*Content\Site.css*，如下所示：</span><span class="sxs-lookup"><span data-stu-id="14a4c-203">To provide a style for this UI, add the following highlighted code to the section `/* info and errors */` in *Content\Site.css*, as shown below:</span></span>

[!code-css[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.css?highlight=2-5)]

### <a name="creating-the-instructor-controller-and-views"></a><span data-ttu-id="14a4c-204">建立 Instructor 控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="14a4c-204">Creating the Instructor Controller and Views</span></span>

<span data-ttu-id="14a4c-205">建立`InstructorController`控制器，如下圖所示：</span><span class="sxs-lookup"><span data-stu-id="14a4c-205">Create an `InstructorController` controller as shown in the following illustration:</span></span>

![Add_Controller_dialog_box_for_Instructor_controller](https://asp.net/media/2577909/Windows-Live-Writer_Reading-Re.NET-MVC-Application-5-of-10h1_ADC3_Add_Controller_dialog_box_for_Instructor_controller_f99c10aa-1efd-49d6-af1d-b00461616107.png)

<span data-ttu-id="14a4c-207">開啟*Controllers\InstructorController.cs*並加入`using`陳述式`ViewModels`命名空間：</span><span class="sxs-lookup"><span data-stu-id="14a4c-207">Open *Controllers\InstructorController.cs* and add a `using` statement for the `ViewModels` namespace:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

<span data-ttu-id="14a4c-208">中的 scaffold 程式碼`Index`方法指定積極式載入，僅適用於`OfficeAssignment`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="14a4c-208">The scaffolded code in the `Index` method specifies eager loading only for the `OfficeAssignment` navigation property:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

<span data-ttu-id="14a4c-209">取代`Index`方法具有下列的程式碼，以載入其他相關資料，並將它放在檢視模型中：</span><span class="sxs-lookup"><span data-stu-id="14a4c-209">Replace the `Index` method with the following code to load additional related data and put it in the view model:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

<span data-ttu-id="14a4c-210">方法會接受選擇性的路由資料 (`id`) 和查詢字串參數 (`courseID`)，提供所選取的講師和所選的課程的識別碼值，並將所有必要的資料傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="14a4c-210">The method accepts optional route data (`id`) and a query string parameter (`courseID`) that provide the ID values of the selected instructor and selected course, and passes all of the required data to the view.</span></span> <span data-ttu-id="14a4c-211">這些參數由頁面上的**選取**超連結提供。</span><span class="sxs-lookup"><span data-stu-id="14a4c-211">The parameters are provided by the **Select** hyperlinks on the page.</span></span>

> [!TIP]
> 
> <span data-ttu-id="14a4c-212">**路由資料**</span><span class="sxs-lookup"><span data-stu-id="14a4c-212">**Route data**</span></span>
> 
> <span data-ttu-id="14a4c-213">路由資料是在路由表中指定的 URL 區段中找到的模型繫結的資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-213">Route data is data that the model binder found in a URL segment specified in the routing table.</span></span> <span data-ttu-id="14a4c-214">例如，指定預設路由`controller`， `action`，和`id`區段：</span><span class="sxs-lookup"><span data-stu-id="14a4c-214">For example, the default route specifies `controller`, `action`, and `id` segments:</span></span>
> 
> <span data-ttu-id="14a4c-215">routes.MapRoute(</span><span class="sxs-lookup"><span data-stu-id="14a4c-215">routes.MapRoute(</span></span>  
>  <span data-ttu-id="14a4c-216">名稱:"Default"，</span><span class="sxs-lookup"><span data-stu-id="14a4c-216">name: "Default",</span></span>  
>  <span data-ttu-id="14a4c-217">url:"{controller} / {action} / {id}"，</span><span class="sxs-lookup"><span data-stu-id="14a4c-217">url: "{controller}/{action}/{id}",</span></span>  
>  <span data-ttu-id="14a4c-218">預設值： new {控制器 ="Home"，動作 ="Index"，識別碼 = UrlParameter.Optional}</span><span class="sxs-lookup"><span data-stu-id="14a4c-218">defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional }</span></span>  
> <span data-ttu-id="14a4c-219">);</span><span class="sxs-lookup"><span data-stu-id="14a4c-219">);</span></span>
> 
> <span data-ttu-id="14a4c-220">在下列 URL 中，對應的預設路由`Instructor`作為`controller`，`Index`作為`action`並 1 做為`id`; 這些是路由資料值。</span><span class="sxs-lookup"><span data-stu-id="14a4c-220">In the following URL, the default route maps `Instructor` as the `controller`, `Index` as the `action` and 1 as the `id`; these are route data values.</span></span>
> 
> `http://localhost:1230/Instructor/Index/1?courseID=2021`
> 
> <span data-ttu-id="14a4c-221">「？ courseID courseid=2021"查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="14a4c-221">"?courseID=2021" is a query string value.</span></span> <span data-ttu-id="14a4c-222">如果您傳遞，也會運作模型繫結`id`作為查詢字串值：</span><span class="sxs-lookup"><span data-stu-id="14a4c-222">The model binder will also work if you pass the `id` as a query string value:</span></span>
> 
> `http://localhost:1230/Instructor/Index?id=1&CourseID=2021`
> 
> <span data-ttu-id="14a4c-223">Url 藉由`ActionLink`Razor 檢視中的陳述式。</span><span class="sxs-lookup"><span data-stu-id="14a4c-223">The URLs are created by `ActionLink` statements in the Razor view.</span></span> <span data-ttu-id="14a4c-224">下列程式碼中，`id`參數符合預設路由，因此`id`會新增至路由資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-224">In the following code, the `id` parameter matches the default route, so `id` is added to the route data.</span></span>
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml)]
> 
> <span data-ttu-id="14a4c-225">下列程式碼，`courseID`不符合預設路由中的參數，所以它會加入做為查詢字串。</span><span class="sxs-lookup"><span data-stu-id="14a4c-225">In the following code, `courseID` doesn't match a parameter in the default route, so it's added as a query string.</span></span>
> 
> [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]


<span data-ttu-id="14a4c-226">此程式碼是從建立檢視模型的執行個體，並將其置於講師清單開始。</span><span class="sxs-lookup"><span data-stu-id="14a4c-226">The code begins by creating an instance of the view model and putting in it the list of instructors.</span></span> <span data-ttu-id="14a4c-227">程式碼會指定積極式載入`Instructor.OfficeAssignment`而`Instructor.Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-227">The code specifies eager loading for the `Instructor.OfficeAssignment` and the `Instructor.Courses` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs?highlight=3-4)]

<span data-ttu-id="14a4c-228">第二個`Include`方法載入課程，並針對每個載入的課程會積極式載入`Course.Department`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-228">The second `Include` method loads Courses, and for each Course that is loaded it does eager loading for the `Course.Department` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

<span data-ttu-id="14a4c-229">如先前所述，不需要積極式載入，但是為了改善效能。</span><span class="sxs-lookup"><span data-stu-id="14a4c-229">As mentioned previously, eager loading is not required but is done to improve performance.</span></span> <span data-ttu-id="14a4c-230">由於檢視一律需要`OfficeAssignment`實體，它是擷取該相同的查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="14a4c-230">Since the view always requires the `OfficeAssignment` entity, it's more efficient to fetch that in the same query.</span></span> <span data-ttu-id="14a4c-231">`Course` 因此積極式載入是優於消極式載入，而不需要選取比更常顯示頁面時，才在網頁上，選取講師時，所需的實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-231">`Course` entities are required when an instructor is selected in the web page, so eager loading is better than lazy loading only if the page is displayed more often with a course selected than without.</span></span>

<span data-ttu-id="14a4c-232">如果選取的講師識別碼，所選取的講師會從檢視模型中的講師清單中。</span><span class="sxs-lookup"><span data-stu-id="14a4c-232">If an instructor ID was selected, the selected instructor is retrieved from the list of instructors in the view model.</span></span> <span data-ttu-id="14a4c-233">檢視模型`Courses`屬性然後充滿`Course`實體從該講師`Courses`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-233">The view model's `Courses` property is then loaded with the `Course` entities from that instructor's `Courses` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

<span data-ttu-id="14a4c-234">`Where`方法會傳回集合，但準則在此情況下傳遞至該方法的結果中只有一個`Instructor`所傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-234">The `Where` method returns a collection, but in this case the criteria passed to that method result in only a single `Instructor` entity being returned.</span></span> <span data-ttu-id="14a4c-235">`Single`方法會將集合轉換成單一`Instructor`可讓您存取與該實體的實體`Courses`屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-235">The `Single` method converts the collection into a single `Instructor` entity, which gives you access to that entity's `Courses` property.</span></span>

<span data-ttu-id="14a4c-236">您使用[單一](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx)集合，當您知道集合上的方法必須只有一個項目。</span><span class="sxs-lookup"><span data-stu-id="14a4c-236">You use the [Single](https://msdn.microsoft.com/library/system.linq.enumerable.single.aspx) method on a collection when you know the collection will have only one item.</span></span> <span data-ttu-id="14a4c-237">`Single`方法擲回例外狀況，如果是空的集合傳遞給它，或是否有多個項目。</span><span class="sxs-lookup"><span data-stu-id="14a4c-237">The `Single` method throws an exception if the collection passed to it is empty or if there's more than one item.</span></span> <span data-ttu-id="14a4c-238">替代方法是[SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx)，它會傳回預設值 (`null`在此情況下) 如果集合是空的。</span><span class="sxs-lookup"><span data-stu-id="14a4c-238">An alternative is [SingleOrDefault](https://msdn.microsoft.com/library/bb342451.aspx), which returns a default value (`null` in this case) if the collection is empty.</span></span> <span data-ttu-id="14a4c-239">不過，在此情況下，仍然會造成例外狀況 (由於嘗試尋找`Courses`屬性上的`null`參考)，和例外狀況訊息會不太清楚地指出問題的原因。</span><span class="sxs-lookup"><span data-stu-id="14a4c-239">However, in this case that would still result in an exception (from trying to find a `Courses` property on a `null` reference), and the exception message would less clearly indicate the cause of the problem.</span></span> <span data-ttu-id="14a4c-240">當您呼叫`Single`方法中，您也可以傳入`Where`條件，而不是呼叫`Where`方法分開：</span><span class="sxs-lookup"><span data-stu-id="14a4c-240">When you call the `Single` method, you can also pass in the `Where` condition instead of calling the `Where` method separately:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

<span data-ttu-id="14a4c-241">而非：</span><span class="sxs-lookup"><span data-stu-id="14a4c-241">Instead of:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

<span data-ttu-id="14a4c-242">接下來，如果已選取課程，則會從檢視模型的課程清單中擷取選取的課程。</span><span class="sxs-lookup"><span data-stu-id="14a4c-242">Next, if a course was selected, the selected course is retrieved from the list of courses in the view model.</span></span> <span data-ttu-id="14a4c-243">然後，檢視模型的`Enrollments`屬性已載入`Enrollment`實體從該課程`Enrollments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-243">Then the view model's `Enrollments` property is loaded with the `Enrollment` entities from that course's `Enrollments` navigation property.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cs)]

### <a name="modifying-the-instructor-index-view"></a><span data-ttu-id="14a4c-244">修改 Instructor [索引] 檢視</span><span class="sxs-lookup"><span data-stu-id="14a4c-244">Modifying the Instructor Index View</span></span>

<span data-ttu-id="14a4c-245">在  *Views\Instructor\Index.cshtml*，以下列程式碼取代現有的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14a4c-245">In *Views\Instructor\Index.cshtml*, replace the existing code with the following code.</span></span> <span data-ttu-id="14a4c-246">所做的變更已醒目提示：</span><span class="sxs-lookup"><span data-stu-id="14a4c-246">The changes are highlighted:</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml?highlight=1,4,18,22-27,29,43-48)]

<span data-ttu-id="14a4c-247">您已對現有程式碼進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="14a4c-247">You've made the following changes to the existing code:</span></span>

- <span data-ttu-id="14a4c-248">已將模型類別變更為 `InstructorIndexData`。</span><span class="sxs-lookup"><span data-stu-id="14a4c-248">Changed the model class to `InstructorIndexData`.</span></span>
- <span data-ttu-id="14a4c-249">已將頁面標題從**索引**變更為**講師**。</span><span class="sxs-lookup"><span data-stu-id="14a4c-249">Changed the page title from **Index** to **Instructors**.</span></span>
- <span data-ttu-id="14a4c-250">移至左側的資料列的連結資料行。</span><span class="sxs-lookup"><span data-stu-id="14a4c-250">Moved the row link columns to the left.</span></span>
- <span data-ttu-id="14a4c-251">移除**FullName**資料行。</span><span class="sxs-lookup"><span data-stu-id="14a4c-251">Removed the **FullName** column.</span></span>
- <span data-ttu-id="14a4c-252">新增**辦公室**會顯示的資料行`item.OfficeAssignment.Location`只有當`item.OfficeAssignment`不是 null。</span><span class="sxs-lookup"><span data-stu-id="14a4c-252">Added an **Office** column that displays `item.OfficeAssignment.Location` only if `item.OfficeAssignment` is not null.</span></span> <span data-ttu-id="14a4c-253">(因為這是一對零-或-一關聯性時，可能不會有相關`OfficeAssignment`實體。)</span><span class="sxs-lookup"><span data-stu-id="14a4c-253">(Because this is a one-to-zero-or-one relationship, there might not be a related `OfficeAssignment` entity.)</span></span> 

    [!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]
- <span data-ttu-id="14a4c-254">新增程式碼，將會以動態方式新增`class="selectedrow"`至`tr`所選取講師的項目。</span><span class="sxs-lookup"><span data-stu-id="14a4c-254">Added code that will dynamically add `class="selectedrow"` to the `tr` element of the selected instructor.</span></span> <span data-ttu-id="14a4c-255">這會設定使用您稍早建立的 CSS 類別選取的資料列的背景色彩。</span><span class="sxs-lookup"><span data-stu-id="14a4c-255">This sets a background color for the selected row using the CSS class that you created earlier.</span></span> <span data-ttu-id="14a4c-256">(`valign`屬性時，會在下列教學課程中有用的多重資料列資料行加入資料表。)</span><span class="sxs-lookup"><span data-stu-id="14a4c-256">(The `valign` attribute will be useful in the following tutorial when you add a multi-row column to the table.)</span></span> 

    [!code-html[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.html)]
- <span data-ttu-id="14a4c-257">已新增`ActionLink`標示**選取**正前方中每個資料列的其他連結，這會讓選取的講師識別碼傳送至`Index`方法。</span><span class="sxs-lookup"><span data-stu-id="14a4c-257">Added a new `ActionLink` labeled **Select** immediately before the other links in each row, which causes the selected instructor ID to be sent to the `Index` method.</span></span>

<span data-ttu-id="14a4c-258">執行應用程式，然後選取**講師** 索引標籤。此頁面會顯示`Location`屬性的相關`OfficeAssignment`實體和空的資料表資料格時有無相關`OfficeAssignment`實體。</span><span class="sxs-lookup"><span data-stu-id="14a4c-258">Run the application and select the **Instructors** tab. The page displays the `Location` property of related `OfficeAssignment` entities and an empty table cell when there's no related `OfficeAssignment` entity.</span></span>

![Instructors_index_page_with_nothing_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

<span data-ttu-id="14a4c-260">在  *Views\Instructor\Index.cshtml*檔案之後則會在結尾,`table`項目 （在檔案結尾），新增下列醒目提示的程式碼。</span><span class="sxs-lookup"><span data-stu-id="14a4c-260">In the *Views\Instructor\Index.cshtml* file, after the closing `table` element (at the end of the file), add the following highlighted code.</span></span> <span data-ttu-id="14a4c-261">這會顯示當選取講師時相關的講師的課程清單。</span><span class="sxs-lookup"><span data-stu-id="14a4c-261">This displays a list of courses related to an instructor when an instructor is selected.</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cshtml?highlight=11-46)]

<span data-ttu-id="14a4c-262">此程式碼會讀取檢視模型的 `Courses` 屬性以顯示課程清單。</span><span class="sxs-lookup"><span data-stu-id="14a4c-262">This code reads the `Courses` property of the view model to display a list of courses.</span></span> <span data-ttu-id="14a4c-263">它也會提供`Select`超連結，會傳送至所選取課程的識別碼`Index`動作方法。</span><span class="sxs-lookup"><span data-stu-id="14a4c-263">It also provides a `Select` hyperlink that sends the ID of the selected course to the `Index` action method.</span></span>

> [!NOTE]
> <span data-ttu-id="14a4c-264">*.Css*由瀏覽器快取檔案。</span><span class="sxs-lookup"><span data-stu-id="14a4c-264">The *.css* file is cached by browsers.</span></span> <span data-ttu-id="14a4c-265">如果您沒有看到所做的變更，當您執行應用程式時，執行硬碟重新整理 (按住 CTRL 鍵同時按一下**重新整理**按鈕，或按 CTRL + F5)。</span><span class="sxs-lookup"><span data-stu-id="14a4c-265">If you don't see the changes when you run the application, do a hard refresh (hold down the CTRL key while clicking the **Refresh** button, or press CTRL+F5).</span></span>


<span data-ttu-id="14a4c-266">執行網頁，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="14a4c-266">Run the page and select an instructor.</span></span> <span data-ttu-id="14a4c-267">現在您會看到一個方格，其中顯示指派給所選取講師的課程，而且在每個課程中，您可以看到指派的部門名稱。</span><span class="sxs-lookup"><span data-stu-id="14a4c-267">Now you see a grid that displays courses assigned to the selected instructor, and for each course you see the name of the assigned department.</span></span>

![Instructors_index_page_with_instructor_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

<span data-ttu-id="14a4c-269">在您剛才新增的程式碼區塊之後，新增下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="14a4c-269">After the code block you just added, add the following code.</span></span> <span data-ttu-id="14a4c-270">這會在選取課程時，顯示已註冊該課程的學生清單。</span><span class="sxs-lookup"><span data-stu-id="14a4c-270">This displays a list of the students who are enrolled in a course when that course is selected.</span></span>

[!code-cshtml[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cshtml)]

<span data-ttu-id="14a4c-271">此程式碼會讀取`Enrollments`屬性以顯示學生的清單檢視模型的註冊過程中。</span><span class="sxs-lookup"><span data-stu-id="14a4c-271">This code reads the `Enrollments` property of the view model in order to display a list of students enrolled in the course.</span></span>

<span data-ttu-id="14a4c-272">執行網頁，然後選取講師。</span><span class="sxs-lookup"><span data-stu-id="14a4c-272">Run the page and select an instructor.</span></span> <span data-ttu-id="14a4c-273">接著選取課程，以查看已註冊學生和其年級的清單。</span><span class="sxs-lookup"><span data-stu-id="14a4c-273">Then select a course to see the list of enrolled students and their grades.</span></span>

![Instructors_index_page_with_instructor_and_course_selected](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="adding-explicit-loading"></a><span data-ttu-id="14a4c-275">新增明確式載入</span><span class="sxs-lookup"><span data-stu-id="14a4c-275">Adding Explicit Loading</span></span>

<span data-ttu-id="14a4c-276">開啟*InstructorController.cs*並查看如何`Index`方法會取得所選取課程的註冊清單：</span><span class="sxs-lookup"><span data-stu-id="14a4c-276">Open *InstructorController.cs* and look at how the `Index` method gets the list of enrollments for a selected course:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cs)]

<span data-ttu-id="14a4c-277">當您擷取講師清單時，您會指定積極式載入`Courses`導覽屬性和`Department`屬性的每個課程。</span><span class="sxs-lookup"><span data-stu-id="14a4c-277">When you retrieved the list of instructors, you specified eager loading for the `Courses` navigation property and for the `Department` property of each course.</span></span> <span data-ttu-id="14a4c-278">然後您將放`Courses`在檢視模型中，而您正在存取的集合`Enrollments`從該集合中的某個實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-278">Then you put the `Courses` collection in the view model, and now you're accessing the `Enrollments` navigation property from one entity in that collection.</span></span> <span data-ttu-id="14a4c-279">因為您沒有指定積極式載入`Course.Enrollments`導覽屬性，該屬性的資料出現在頁面中，由於消極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-279">Because you didn't specify eager loading for the `Course.Enrollments` navigation property, the data from that property is appearing in the page as a result of lazy loading.</span></span>

<span data-ttu-id="14a4c-280">如果您不需要變更任何其他方式中的程式碼停用消極式載入`Enrollments`屬性將會是 null，不論課程實際上有多少的註冊項目。</span><span class="sxs-lookup"><span data-stu-id="14a4c-280">If you disabled lazy loading without changing the code in any other way, the `Enrollments` property would be null regardless of how many enrollments the course actually had.</span></span> <span data-ttu-id="14a4c-281">在此情況下，載入`Enrollments`屬性，您必須指定積極式載入或明確式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-281">In that case, to load the `Enrollments` property, you'd have to specify either eager loading or explicit loading.</span></span> <span data-ttu-id="14a4c-282">您已經看到如何進行積極式載入。</span><span class="sxs-lookup"><span data-stu-id="14a4c-282">You've already seen how to do eager loading.</span></span> <span data-ttu-id="14a4c-283">若要查看的明確式載入範例，取代`Index`方法，明確地載入為下列程式碼`Enrollments`屬性。</span><span class="sxs-lookup"><span data-stu-id="14a4c-283">In order to see an example of explicit loading, replace the `Index` method with the following code, which explicitly loads the `Enrollments` property.</span></span> <span data-ttu-id="14a4c-284">變更的程式碼會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="14a4c-284">The code changed are highlighted.</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample24.cs?highlight=20-27)]

<span data-ttu-id="14a4c-285">取得所選之後`Course`實體，新的程式碼，明確地載入該課程`Enrollments`導覽屬性：</span><span class="sxs-lookup"><span data-stu-id="14a4c-285">After getting the selected `Course` entity, the new code explicitly loads that course's `Enrollments` navigation property:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample25.cs)]

<span data-ttu-id="14a4c-286">然後它會明確地載入每個`Enrollment`實體的相關`Student`實體：</span><span class="sxs-lookup"><span data-stu-id="14a4c-286">Then it explicitly loads each `Enrollment` entity's related `Student` entity:</span></span>

[!code-csharp[Main](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample26.cs)]

<span data-ttu-id="14a4c-287">請注意，您使用`Collection`方法來載入集合屬性，但會保留一個實體的屬性，您使用`Reference`方法。</span><span class="sxs-lookup"><span data-stu-id="14a4c-287">Notice that you use the `Collection` method to load a collection property, but for a property that holds just one entity, you use the `Reference` method.</span></span> <span data-ttu-id="14a4c-288">您可以立即執行 Instructor 索引 頁面，您會看到任何差異，在頁面上，顯示的內容，雖然您已變更資料擷取的方式。</span><span class="sxs-lookup"><span data-stu-id="14a4c-288">You can run the Instructor Index page now and you'll see no difference in what's displayed on the page, although you've changed how the data is retrieved.</span></span>

## <a name="summary"></a><span data-ttu-id="14a4c-289">總結</span><span class="sxs-lookup"><span data-stu-id="14a4c-289">Summary</span></span>

<span data-ttu-id="14a4c-290">您現在已使用三種方式 （lazy，積極式，和明確） 載入到導覽屬性的相關的資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-290">You've now used all three ways (lazy, eager, and explicit) to load related data into navigation properties.</span></span> <span data-ttu-id="14a4c-291">在下一個教學課程中，您將了解如何更新相關資料。</span><span class="sxs-lookup"><span data-stu-id="14a4c-291">In the next tutorial you'll learn how to update related data.</span></span>

<span data-ttu-id="14a4c-292">其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。</span><span class="sxs-lookup"><span data-stu-id="14a4c-292">Links to other Entity Framework resources can be found in the [ASP.NET Data Access Content Map](../../../../whitepapers/aspnet-data-access-content-map.md).</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="14a4c-293">[上一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
> [下一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span><span class="sxs-lookup"><span data-stu-id="14a4c-293">[Previous](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
[Next](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)</span></span>
