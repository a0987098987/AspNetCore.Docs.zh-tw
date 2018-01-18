---
title: "使用 EF 核心 CRUD-2 的 8 razor 頁面"
author: rick-anderson
description: "示範如何建立、 讀取、 更新、 刪除與 EF 核心"
keywords: "ASP.NET Core、 Entity Framework Core、 CRUD，建立、 讀取、 更新、 刪除"
ms.author: riande
manager: wpickett
ms.date: 10/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/crud
ms.openlocfilehash: 246e6307989f2660d84288ceac6793c422875f93
ms.sourcegitcommit: 87168cdc409e7a7257f92a0f48f9c5ab320b5b28
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/17/2018
---
# <a name="create-read-update-and-delete---ef-core-with-razor-pages-2-of-8"></a><span data-ttu-id="d7779-104">建立、 讀取、 更新和刪除-Razor 頁面 (8 個 2) 使用的 EF 核心</span><span class="sxs-lookup"><span data-stu-id="d7779-104">Create, Read, Update, and Delete - EF Core with Razor Pages (2 of 8)</span></span>

<span data-ttu-id="d7779-105">由[Tom Dykstra](https://github.com/tdykstra)， [Jon P Smith](https://twitter.com/thereformedprog)，和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d7779-105">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d7779-106">在本教學課程，scaffold CRUD （建立、 讀取、 更新、 刪除） 的程式碼檢閱並自訂。</span><span class="sxs-lookup"><span data-stu-id="d7779-106">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="d7779-107">注意： 若要降低複雜性並將焦點放在 EF 核心這些教學課程，EF 核心程式碼會使用 Razor 頁面程式碼後置檔案中。</span><span class="sxs-lookup"><span data-stu-id="d7779-107">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages code-behind files.</span></span> <span data-ttu-id="d7779-108">有些開發人員會使用服務層或儲存機制模式中的建立 UI （Razor 頁面） 和資料存取層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="d7779-108">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="d7779-109">在本教學課程、 建立、 編輯、 刪除和詳細資料中的 Razor 頁面*學生*資料夾會被修改。</span><span class="sxs-lookup"><span data-stu-id="d7779-109">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="d7779-110">Scaffold 的程式碼會使用下列模式建立、 編輯和刪除的頁面：</span><span class="sxs-lookup"><span data-stu-id="d7779-110">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="d7779-111">取得並顯示要求的資料與 HTTP GET 方法`OnGetAsync`。</span><span class="sxs-lookup"><span data-stu-id="d7779-111">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="d7779-112">使用 HTTP POST 方法，將變更儲存至資料`OnPostAsync`。</span><span class="sxs-lookup"><span data-stu-id="d7779-112">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="d7779-113">索引 和 詳細資料頁面中取得並顯示要求的資料與 HTTP GET 方法`OnGetAsync`</span><span class="sxs-lookup"><span data-stu-id="d7779-113">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="d7779-114">取代 FirstOrDefaultAsync SingleOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="d7779-114">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="d7779-115">產生的程式碼會使用[SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)來提取要求的實體。</span><span class="sxs-lookup"><span data-stu-id="d7779-115">The generated code uses [SingleOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="d7779-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_)是更能有效地擷取一個實體：</span><span class="sxs-lookup"><span data-stu-id="d7779-116">[FirstOrDefaultAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="d7779-117">除非程式碼必須以確認沒有任何多個查詢所傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="d7779-117">Unless the code needs to verify that there is not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="d7779-118">`SingleOrDefaultAsync`提取更多資料，而且也不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="d7779-118">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="d7779-119">`SingleOrDefaultAsync`如果沒有符合篩選組件的多個實體，會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d7779-119">`SingleOrDefaultAsync` throws an exception if there is more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="d7779-120">`FirstOrDefaultAsync`如果沒有符合篩選組件的多個實體，不會擲回。</span><span class="sxs-lookup"><span data-stu-id="d7779-120">`FirstOrDefaultAsync` doesn't throw if there is more than one entity that fits the filter part.</span></span>

<span data-ttu-id="d7779-121">全域取代`SingleOrDefaultAsync`與`FirstOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="d7779-121">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="d7779-122">`SingleOrDefaultAsync`5 個地方使用：</span><span class="sxs-lookup"><span data-stu-id="d7779-122">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="d7779-123">`OnGetAsync`在 [詳細資料] 頁面中。</span><span class="sxs-lookup"><span data-stu-id="d7779-123">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="d7779-124">`OnGetAsync`和`OnPostAsync`編輯和刪除的頁面中。</span><span class="sxs-lookup"><span data-stu-id="d7779-124">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="d7779-125">FindAsync</span><span class="sxs-lookup"><span data-stu-id="d7779-125">FindAsync</span></span>

<span data-ttu-id="d7779-126">在大部分的 scaffold 的程式碼， [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___)可用取代`FirstOrDefaultAsync`或`SingleOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="d7779-126">In much of the scaffolded code, [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="d7779-127">`FindAsync`：</span><span class="sxs-lookup"><span data-stu-id="d7779-127">`FindAsync`:</span></span>

* <span data-ttu-id="d7779-128">尋找實體主索引鍵 (PK)。</span><span class="sxs-lookup"><span data-stu-id="d7779-128">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="d7779-129">如果內容正在追蹤在 PK 的實體，它會傳回沒有要求至 DB。</span><span class="sxs-lookup"><span data-stu-id="d7779-129">If an entity with the PK is being tracked by the context, it is returned without a request to the DB.</span></span>
* <span data-ttu-id="d7779-130">是簡單且精確。</span><span class="sxs-lookup"><span data-stu-id="d7779-130">Is simple and concise.</span></span>
* <span data-ttu-id="d7779-131">已最佳化來查閱單一實體。</span><span class="sxs-lookup"><span data-stu-id="d7779-131">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="d7779-132">可以在某些情況下，具有效能優點，但是它們很少是必須列入的一般 web 案例。</span><span class="sxs-lookup"><span data-stu-id="d7779-132">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="d7779-133">隱含使用[FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)而不是[SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。</span><span class="sxs-lookup"><span data-stu-id="d7779-133">Implicitly uses [FirstAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="d7779-134">但如果您想要包含其他項目，則尋找已不再適用。</span><span class="sxs-lookup"><span data-stu-id="d7779-134">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="d7779-135">這表示您可能要放棄尋找並將您的應用程式進行時將移至查詢。</span><span class="sxs-lookup"><span data-stu-id="d7779-135">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="d7779-136">自訂的詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="d7779-136">Customize the Details page</span></span>

<span data-ttu-id="d7779-137">瀏覽至`Pages/Students`頁面。</span><span class="sxs-lookup"><span data-stu-id="d7779-137">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="d7779-138">**編輯**，**詳細資料**，和**刪除**所產生連結[錨定標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)中*頁面/學生 /Index.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d7779-138">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="d7779-139">選取 [詳細資料] 連結。</span><span class="sxs-lookup"><span data-stu-id="d7779-139">Select a Details link.</span></span> <span data-ttu-id="d7779-140">URL 的格式是`http://localhost:5000/Students/Details?id=2`。</span><span class="sxs-lookup"><span data-stu-id="d7779-140">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="d7779-141">使用查詢字串來傳遞學生識別碼 (`?id=2`)。</span><span class="sxs-lookup"><span data-stu-id="d7779-141">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="d7779-142">更新編輯、 詳細資料，以及刪除 Razor 頁面使用`"{id:int}"`路由範本。</span><span class="sxs-lookup"><span data-stu-id="d7779-142">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="d7779-143">將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="d7779-143">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="d7779-144">"{Id: int}"的路由範本並與網頁要求**不**包含整數的路由值傳回 HTTP 404 （找不到） 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d7779-144">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="d7779-145">例如，`http://localhost:5000/Students/Details`傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d7779-145">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="d7779-146">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="d7779-146">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="d7779-147">執行應用程式，然後按一下 [詳細資料] 連結，再確認 URL 傳遞做為路由資料的識別碼 (`http://localhost:5000/Students/Details/2`)。</span><span class="sxs-lookup"><span data-stu-id="d7779-147">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="d7779-148">請不要全域變更`@page`至`@page "{id:int}"`、 執行中斷首頁連結，以及建立頁面。</span><span class="sxs-lookup"><span data-stu-id="d7779-148">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="d7779-149">新增相關的資料</span><span class="sxs-lookup"><span data-stu-id="d7779-149">Add related data</span></span>

<span data-ttu-id="d7779-150">學生索引頁面的 scaffold 程式碼不包含`Enrollments`屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-150">The scaffolded code for the Students Index page does not include the `Enrollments` property.</span></span> <span data-ttu-id="d7779-151">本節的內容中`Enrollments`集合會顯示在詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="d7779-151">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="d7779-152">`OnGetAsync`方法*Pages/Students/Details.cshtml.cs*使用`FirstOrDefaultAsync`方法來擷取單一`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="d7779-152">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="d7779-153">加入下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d7779-153">Add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="d7779-154">`Include`和`ThenInclude`方法會造成載入內容`Student.Enrollments`導覽屬性，以及在每個註冊內`Enrollment.Course`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-154">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="d7779-155">這些方法是 examinied 詳細閱讀相關的資料 」 教學課程中。</span><span class="sxs-lookup"><span data-stu-id="d7779-155">These methods are examinied in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="d7779-156">`AsNoTracking`方法可改善案例的效能時不會更新目前的內容中傳回的實體。</span><span class="sxs-lookup"><span data-stu-id="d7779-156">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="d7779-157">`AsNoTracking`在本教學課程稍後討論。</span><span class="sxs-lookup"><span data-stu-id="d7779-157">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="d7779-158">在詳細資料 頁面上顯示相關的註冊項目</span><span class="sxs-lookup"><span data-stu-id="d7779-158">Display related enrollments on the Details page</span></span>

<span data-ttu-id="d7779-159">開啟*Pages/Students/Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="d7779-159">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="d7779-160">加入下列反白顯示的程式碼，以顯示一份註冊項目：</span><span class="sxs-lookup"><span data-stu-id="d7779-160">Add the following highlighted code to display a list of enrollments:</span></span>

 <!--2do ricka. if doesn't change, remove dup -->
[!code-cshtml[Main](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]

<span data-ttu-id="d7779-161">如果程式碼縮排是錯的程式碼就會貼上之後，請按 CTRL-K-D 更正它。</span><span class="sxs-lookup"><span data-stu-id="d7779-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="d7779-162">上述程式碼中執行迴圈中的實體`Enrollments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="d7779-163">針對每個註冊，它會顯示課程標題以及等級。</span><span class="sxs-lookup"><span data-stu-id="d7779-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="d7779-164">課程標題會從儲存在課程實體`Course`註冊實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="d7779-165">執行應用程式中，選取**學生**索引標籤，然後按一下**詳細資料**學生的連結。</span><span class="sxs-lookup"><span data-stu-id="d7779-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="d7779-166">Courses 以及為選取的學生成績的清單隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="d7779-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="d7779-167">更新 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="d7779-167">Update the Create page</span></span>

<span data-ttu-id="d7779-168">更新`OnPostAsync`方法中的*Pages/Students/Create.cshtml.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d7779-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="d7779-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="d7779-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="d7779-170">檢查[TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_)程式碼：</span><span class="sxs-lookup"><span data-stu-id="d7779-170">Examine the [TryUpdateModelAsync](https://docs.microsoft.com/ dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="d7779-171">在上述程式碼，`TryUpdateModelAsync<Student>`嘗試更新`emptyStudent`物件使用的已張貼的表單值[PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext)屬性[PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="d7779-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d7779-172">`TryUpdateModelAsync`只會更新所列的屬性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="d7779-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="d7779-173">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="d7779-173">In the preceding sample:</span></span>

* <span data-ttu-id="d7779-174">第二個引數 (` "student", // Prefix`) 是前置詞會使用來查閱值。</span><span class="sxs-lookup"><span data-stu-id="d7779-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="d7779-175">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d7779-175">It's not case sensitive.</span></span>
* <span data-ttu-id="d7779-176">已張貼的表單值會轉換成中的型別`Student`模型使用[模型繫結](xref:mvc/models/model-binding#how-model-binding-works)。</span><span class="sxs-lookup"><span data-stu-id="d7779-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="d7779-177">Overposting</span><span class="sxs-lookup"><span data-stu-id="d7779-177">Overposting</span></span>

<span data-ttu-id="d7779-178">使用`TryUpdateModel`更新具有已張貼值欄位是安全性最佳作法，因為它會阻止 overposting。</span><span class="sxs-lookup"><span data-stu-id="d7779-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="d7779-179">例如，假設 學生實體包含`Secret`這個 web 網頁不應該更新或新增的屬性：</span><span class="sxs-lookup"><span data-stu-id="d7779-179">For example, suppose the Student entity includes a `Secret` property that this web page should not update or add:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="d7779-180">即使應用程式沒有`Secret`欄位上建立/更新 Razor 頁面上，駭客無法設定`Secret`overposting 值。</span><span class="sxs-lookup"><span data-stu-id="d7779-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="d7779-181">駭客無法使用 Fiddler 之類的工具或撰寫一些 JavaScript，張貼`Secret`形成值。</span><span class="sxs-lookup"><span data-stu-id="d7779-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="d7779-182">原始的程式碼並不會限制建立的學生執行個體時，會使用模型繫結器的欄位。</span><span class="sxs-lookup"><span data-stu-id="d7779-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="d7779-183">任何值指定給駭客`Secret`表單欄位會在資料庫中更新。</span><span class="sxs-lookup"><span data-stu-id="d7779-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="d7779-184">下圖顯示 Fiddler 工具加入`Secret`（具有值"OverPost"） 的已張貼的表單值的欄位。</span><span class="sxs-lookup"><span data-stu-id="d7779-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 加入密碼欄位](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="d7779-186">值"OverPost 」 已成功新增到`Secret`插入資料列的屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="d7779-187">絕不想要的應用程式設計師`Secret`要使用 [建立] 頁面來設定屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="d7779-188">檢視模型</span><span class="sxs-lookup"><span data-stu-id="d7779-188">View model</span></span>

<span data-ttu-id="d7779-189">檢視模型通常包含應用程式所使用的模型中包含的屬性子集。</span><span class="sxs-lookup"><span data-stu-id="d7779-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="d7779-190">應用程式模型通常稱為網域模型。</span><span class="sxs-lookup"><span data-stu-id="d7779-190">The application model is often called the domain model.</span></span> <span data-ttu-id="d7779-191">網域模型通常會包含所有需要對應的實體 DB 中的屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="d7779-192">檢視模型包含只針對所需的 UI 層 （例如，[建立] 頁面） 的屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="d7779-193">除了檢視模型，某些應用程式會使用繫結模型或輸入的模型，Razor 頁面程式碼後置類別和瀏覽器之間傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="d7779-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages code-behind class and the browser.</span></span> <span data-ttu-id="d7779-194">請考慮下列`Student`檢視模型：</span><span class="sxs-lookup"><span data-stu-id="d7779-194">Consider the following `Student` view model:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="d7779-195">檢視模型提供的替代方法，以防止 overposting。</span><span class="sxs-lookup"><span data-stu-id="d7779-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="d7779-196">檢視模型包含檢視 （顯示） 或更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="d7779-197">下列程式碼會使用`StudentVM`檢視模型來建立新的學生：</span><span class="sxs-lookup"><span data-stu-id="d7779-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d7779-198">[SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_)方法所讀取的值設定這個物件值[PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues)物件。</span><span class="sxs-lookup"><span data-stu-id="d7779-198">The [SetValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="d7779-199">`SetValues`使用屬性名稱比對。</span><span class="sxs-lookup"><span data-stu-id="d7779-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="d7779-200">檢視模型類型不需要有關聯的模型型別，您只需要取得相符的內容。</span><span class="sxs-lookup"><span data-stu-id="d7779-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="d7779-201">使用`StudentVM`需要[CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml)更新為使用`StudentVM`而不是`Student`。</span><span class="sxs-lookup"><span data-stu-id="d7779-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="d7779-202">在 Razor 頁面`PageModel`衍生的類別是檢視模型。</span><span class="sxs-lookup"><span data-stu-id="d7779-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="d7779-203">更新編輯頁面</span><span class="sxs-lookup"><span data-stu-id="d7779-203">Update the Edit page</span></span>

<span data-ttu-id="d7779-204">更新編輯頁面程式碼後置檔案：</span><span class="sxs-lookup"><span data-stu-id="d7779-204">Update the Edit page code-behind file:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="d7779-205">建立頁面，有一些例外狀況的程式碼變更如下：</span><span class="sxs-lookup"><span data-stu-id="d7779-205">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="d7779-206">`OnPostAsync`具有選擇性`id`參數。</span><span class="sxs-lookup"><span data-stu-id="d7779-206">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="d7779-207">目前的學生會擷取從資料庫中，而不是建立空白的學生。</span><span class="sxs-lookup"><span data-stu-id="d7779-207">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="d7779-208">`FirstOrDefaultAsync`已取代為[FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="d7779-208">`FirstOrDefaultAsync` has been replaced with [FindAsync](https://docs.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="d7779-209">`FindAsync`從主索引鍵選取實體時，是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="d7779-209">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="d7779-210">請參閱[FindAsync](#FindAsync)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="d7779-210">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="d7779-211">測試編輯，以及建立頁面</span><span class="sxs-lookup"><span data-stu-id="d7779-211">Test the Edit and Create pages</span></span>

<span data-ttu-id="d7779-212">建立和編輯幾個學生實體。</span><span class="sxs-lookup"><span data-stu-id="d7779-212">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="d7779-213">實體狀態</span><span class="sxs-lookup"><span data-stu-id="d7779-213">Entity States</span></span>

<span data-ttu-id="d7779-214">在記憶體中的實體 DB 中其對應的資料列同步是否記錄的 DB 內容。</span><span class="sxs-lookup"><span data-stu-id="d7779-214">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="d7779-215">DB 內容的同步資訊會決定會發生什麼事時`SaveChanges`呼叫。</span><span class="sxs-lookup"><span data-stu-id="d7779-215">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="d7779-216">例如，當新的實體傳遞至`Add`方法中，實體的狀態設為`Added`。</span><span class="sxs-lookup"><span data-stu-id="d7779-216">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="d7779-217">當`SaveChanges`呼叫時，資料庫內容發出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="d7779-217">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="d7779-218">實體可能處於下列狀態其中之一：</span><span class="sxs-lookup"><span data-stu-id="d7779-218">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="d7779-219">`Added`: 實體不在資料庫中尚未存在。</span><span class="sxs-lookup"><span data-stu-id="d7779-219">`Added`: The entity does not yet exist in the DB.</span></span> <span data-ttu-id="d7779-220">`SaveChanges`方法發出的 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d7779-220">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="d7779-221">`Unchanged`： 需要此實體與此一併儲存沒有變更。</span><span class="sxs-lookup"><span data-stu-id="d7779-221">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="d7779-222">從資料庫讀取時，實體具有此狀態。</span><span class="sxs-lookup"><span data-stu-id="d7779-222">An entity has this status when it is read from the DB.</span></span>

* <span data-ttu-id="d7779-223">`Modified`： 已修改部分或所有實體的屬性值。</span><span class="sxs-lookup"><span data-stu-id="d7779-223">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="d7779-224">`SaveChanges`方法發出 UPDATE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d7779-224">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="d7779-225">`Deleted`： 實體已被標示為刪除。</span><span class="sxs-lookup"><span data-stu-id="d7779-225">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="d7779-226">`SaveChanges`方法發出 DELETE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d7779-226">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="d7779-227">`Detached`： 實體沒有正在追蹤的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="d7779-227">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="d7779-228">在傳統型應用程式，會通常是自動設定的狀態變更。</span><span class="sxs-lookup"><span data-stu-id="d7779-228">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="d7779-229">實體讀取、 進行變更，並自動變更為實體狀態`Modified`。</span><span class="sxs-lookup"><span data-stu-id="d7779-229">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="d7779-230">呼叫`SaveChanges`會產生 SQL UPDATE 陳述式，以更新變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="d7779-230">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="d7779-231">在 web 應用程式中，`DbContext`實體，並顯示呈現頁面之後，會處置資料讀取。</span><span class="sxs-lookup"><span data-stu-id="d7779-231">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="d7779-232">當頁面`OnPostAsync`呼叫方法時，都會建立新的 web 要求的新執行個體的正與`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="d7779-232">When a pages `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="d7779-233">重新讀取該新的內容中的實體會模擬桌面的處理。</span><span class="sxs-lookup"><span data-stu-id="d7779-233">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d7779-234">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="d7779-234">Update the Delete page</span></span>

<span data-ttu-id="d7779-235">在本節中，加入程式碼實作自訂的錯誤訊息時呼叫`SaveChanges`失敗。</span><span class="sxs-lookup"><span data-stu-id="d7779-235">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="d7779-236">加入字串，包含可能的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="d7779-236">Add a string to contain possible error messages:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="d7779-237">以下列程式碼取代 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d7779-237">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="d7779-238">上述程式碼中包含選擇性參數`saveChangesError`。</span><span class="sxs-lookup"><span data-stu-id="d7779-238">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="d7779-239">`saveChangesError`指出是否要刪除的學生物件失敗後呼叫方法。</span><span class="sxs-lookup"><span data-stu-id="d7779-239">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="d7779-240">刪除作業可能會因為暫時性的網路問題而失敗。</span><span class="sxs-lookup"><span data-stu-id="d7779-240">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="d7779-241">暫時性的網路錯誤都可能在雲端。</span><span class="sxs-lookup"><span data-stu-id="d7779-241">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="d7779-242">`saveChangesError`為 false 時刪除頁面`OnGetAsync`呼叫從 UI。</span><span class="sxs-lookup"><span data-stu-id="d7779-242">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="d7779-243">當`OnGetAsync`稱為`OnPostAsync`（因為刪除作業失敗），則`saveChangesError`參數為 true。</span><span class="sxs-lookup"><span data-stu-id="d7779-243">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="d7779-244">刪除頁面 OnPostAsync 方法</span><span class="sxs-lookup"><span data-stu-id="d7779-244">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="d7779-245">取代`OnPostAsync`為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d7779-245">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d7779-246">上述程式碼擷取選取的實體，然後呼叫`Remove`實體的狀態設為方法`Deleted`。</span><span class="sxs-lookup"><span data-stu-id="d7779-246">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="d7779-247">當`SaveChanges`呼叫時，SQL DELETE 命令產生。</span><span class="sxs-lookup"><span data-stu-id="d7779-247">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="d7779-248">如果`Remove`失敗：</span><span class="sxs-lookup"><span data-stu-id="d7779-248">If `Remove` fails:</span></span>

* <span data-ttu-id="d7779-249">資料庫是例外。</span><span class="sxs-lookup"><span data-stu-id="d7779-249">The DB exception is caught.</span></span>
* <span data-ttu-id="d7779-250">刪除頁面`OnGetAsync`方法呼叫`saveChangesError=true`。</span><span class="sxs-lookup"><span data-stu-id="d7779-250">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="d7779-251">更新刪除 Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="d7779-251">Update the Delete Razor Page</span></span>

<span data-ttu-id="d7779-252">將下列反白顯示的錯誤訊息加入刪除 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="d7779-252">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[Main](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="d7779-253">測試刪除。</span><span class="sxs-lookup"><span data-stu-id="d7779-253">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="d7779-254">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="d7779-254">Common errors</span></span>

<span data-ttu-id="d7779-255">不適用學生/住家或其他連結：</span><span class="sxs-lookup"><span data-stu-id="d7779-255">Student/Home or other links don't work:</span></span>

<span data-ttu-id="d7779-256">確認 Razor 頁面包含正確`@page`指示詞。</span><span class="sxs-lookup"><span data-stu-id="d7779-256">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="d7779-257">比方說，學生/Razor 首頁應該**不**包含路由範本：</span><span class="sxs-lookup"><span data-stu-id="d7779-257">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="d7779-258">必須包含每個 Razor 頁面`@page`指示詞。</span><span class="sxs-lookup"><span data-stu-id="d7779-258">Each Razor Page must include the `@page` directive.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d7779-259">[上一頁](xref:data/ef-rp/intro)
[下一頁](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="d7779-259">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
