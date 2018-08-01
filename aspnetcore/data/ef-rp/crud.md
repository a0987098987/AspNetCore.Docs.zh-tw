---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - CRUD - 2/8
author: rick-anderson
description: 示範如何以 EF Core 來建立、讀取、更新、刪除
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/crud
ms.openlocfilehash: e3a0ec2e21ae9e9eeaae1eb7c17f1604897fb6f9
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342454"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="86c1c-103">ASP.NET Core 中的 Razor 頁面與 EF Core - CRUD - 2/8</span><span class="sxs-lookup"><span data-stu-id="86c1c-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="86c1c-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="86c1c-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](~/includes/RP-EF/intro.md)]

<span data-ttu-id="86c1c-105">在本教學課程中，將會檢閱並自訂 Scaffold CRUD (建立、讀取、更新、刪除)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="86c1c-106">為降低複雜性並將教學課程聚焦於 EF Core，EF Core 程式碼會使用於頁面模型中。</span><span class="sxs-lookup"><span data-stu-id="86c1c-106">To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the page models.</span></span> <span data-ttu-id="86c1c-107">有些開發人員會使用服務層或[存放庫模式](xref:fundamentals/repository-pattern)來建立介於 UI (Razor 頁面) 和資料存取層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="86c1c-107">Some developers use a service layer or [repository pattern](xref:fundamentals/repository-pattern) in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="86c1c-108">在本教學課程中，會檢查位於 *Student* 資料夾中的 [建立]、[編輯]、[刪除] 和 [詳細資料] Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="86c1c-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are examined.</span></span>

<span data-ttu-id="86c1c-109">Scaffold 程式碼會為 [建立]、[編輯]、[刪除] 頁面使用下列模式：</span><span class="sxs-lookup"><span data-stu-id="86c1c-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="86c1c-110">使用 HTTP GET 方法 `OnGetAsync` 來取得並顯示所要求的資料。</span><span class="sxs-lookup"><span data-stu-id="86c1c-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="86c1c-111">使用 HTTP POST `OnPostAsync` 方法將變更儲存至資料。</span><span class="sxs-lookup"><span data-stu-id="86c1c-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="86c1c-112">[索引] 頁面和 [詳細資料] 頁面以 HTTP GET 方法 `OnGetAsync` 取得並顯示所要求的資料</span><span class="sxs-lookup"><span data-stu-id="86c1c-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="singleordefaultasync-vs-firstordefaultasync"></a><span data-ttu-id="86c1c-113">FirstOrDefaultAsync 與 SingleOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="86c1c-113">SingleOrDefaultAsync vs FirstOrDefaultAsync</span></span>

<span data-ttu-id="86c1c-114">產生的程式碼會使用 [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_)，一般會偏好它而非 [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-114">The generated code uses [FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_), which is generally preferred over [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

 <span data-ttu-id="86c1c-115">`FirstOrDefaultAsync` 在擷取單一實體上比 `SingleOrDefaultAsync` 更有效率：</span><span class="sxs-lookup"><span data-stu-id="86c1c-115">`FirstOrDefaultAsync` is more efficient than `SingleOrDefaultAsync` at fetching one entity:</span></span>

* <span data-ttu-id="86c1c-116">除非程式碼需要確認從查詢傳回的實體不多於一個。</span><span class="sxs-lookup"><span data-stu-id="86c1c-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span>
* <span data-ttu-id="86c1c-117">`SingleOrDefaultAsync` 擷取更多資料並進行不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="86c1c-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="86c1c-118">如果有多於一個符合篩選組件的實體，`SingleOrDefaultAsync` 將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="86c1c-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
* <span data-ttu-id="86c1c-119">如果有多於一個符合篩選組件的實體，`FirstOrDefaultAsync` 不會擲回。</span><span class="sxs-lookup"><span data-stu-id="86c1c-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<a name="FindAsync"></a>

### <a name="findasync"></a><span data-ttu-id="86c1c-120">FindAsync</span><span class="sxs-lookup"><span data-stu-id="86c1c-120">FindAsync</span></span>

<span data-ttu-id="86c1c-121">在大部分的 Scaffold 程式碼中，[FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) 可用來取代 `FirstOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="86c1c-121">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync`.</span></span>

<span data-ttu-id="86c1c-122">`FindAsync`：</span><span class="sxs-lookup"><span data-stu-id="86c1c-122">`FindAsync`:</span></span>

* <span data-ttu-id="86c1c-123">尋找含有主索引鍵 (PK) 的實體。</span><span class="sxs-lookup"><span data-stu-id="86c1c-123">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="86c1c-124">如果內容正在追蹤含有主索引鍵的實體，會在沒有傳送要求至資料庫的情況下傳回。</span><span class="sxs-lookup"><span data-stu-id="86c1c-124">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="86c1c-125">簡單並精確。</span><span class="sxs-lookup"><span data-stu-id="86c1c-125">Is simple and concise.</span></span>
* <span data-ttu-id="86c1c-126">已最佳化來查閱單一實體。</span><span class="sxs-lookup"><span data-stu-id="86c1c-126">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="86c1c-127">在某些情況下可具有效能優勢，但是在一般的 Web 應用程式很少見。</span><span class="sxs-lookup"><span data-stu-id="86c1c-127">Can have perf benefits in some situations, but they rarely happens for typical web apps.</span></span>
* <span data-ttu-id="86c1c-128">隱含使用 [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) 而不是 [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-128">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>

<span data-ttu-id="86c1c-129">但是如果您想要 `Include` 其他項目，則 `FindAsync` 已不再適用。</span><span class="sxs-lookup"><span data-stu-id="86c1c-129">But if you want to `Include` other entities, then `FindAsync` is no longer appropriate.</span></span> <span data-ttu-id="86c1c-130">這表示您可能要放棄 `FindAsync` 並隨著您的應用程式進行而移至查詢。</span><span class="sxs-lookup"><span data-stu-id="86c1c-130">This means that you may need to abandon `FindAsync` and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="86c1c-131">自訂 [詳細資料] 頁面</span><span class="sxs-lookup"><span data-stu-id="86c1c-131">Customize the Details page</span></span>

<span data-ttu-id="86c1c-132">瀏覽至 `Pages/Students` 頁面。</span><span class="sxs-lookup"><span data-stu-id="86c1c-132">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="86c1c-133">在 *Pages/Students/Index.cshtml* 檔案中，**編輯**、**詳細資料**、**刪除** 連結是由[錨點標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)所產生。</span><span class="sxs-lookup"><span data-stu-id="86c1c-133">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Index1.cshtml?name=snippet)]

<span data-ttu-id="86c1c-134">執行應用程式並選取 [詳細資料] 連結。</span><span class="sxs-lookup"><span data-stu-id="86c1c-134">Run the app and select a **Details** link.</span></span> <span data-ttu-id="86c1c-135">URL 的格式為 `http://localhost:5000/Students/Details?id=2` 。</span><span class="sxs-lookup"><span data-stu-id="86c1c-135">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="86c1c-136">使用 (`?id=2`) 查詢字串來傳遞 Student ID。</span><span class="sxs-lookup"><span data-stu-id="86c1c-136">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="86c1c-137">更新 [編輯]、[詳細資料] 和 [刪除] Razor 頁面，以使用 `"{id:int}"` 路由範本。</span><span class="sxs-lookup"><span data-stu-id="86c1c-137">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="86c1c-138">將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="86c1c-138">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="86c1c-139">對使用 "{id:int}" 路由範本的頁面提出的要求若**未**包含整數路由值，將傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="86c1c-139">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="86c1c-140">例如，`http://localhost:5000/Students/Details` 傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="86c1c-140">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="86c1c-141">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="86c1c-141">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="86c1c-142">執行應用程式，按一下詳細資料連結，並確認 URL 作為路由資料 (`http://localhost:5000/Students/Details/2`) 來傳遞識別碼。</span><span class="sxs-lookup"><span data-stu-id="86c1c-142">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="86c1c-143">請勿全域變更 `@page` 至 `@page "{id:int}"`，這麼做會中斷 [首頁] 頁面與 [建立] 頁面之間的連結。</span><span class="sxs-lookup"><span data-stu-id="86c1c-143">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="86c1c-144">新增相關資料</span><span class="sxs-lookup"><span data-stu-id="86c1c-144">Add related data</span></span>

<span data-ttu-id="86c1c-145">Students [索引] 頁面的 Scaffold 程式碼不包含 `Enrollments` 屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-145">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="86c1c-146">在本節中，`Enrollments` 集合的內容會顯示於 [詳細資料] 頁面。</span><span class="sxs-lookup"><span data-stu-id="86c1c-146">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="86c1c-147">*Pages/Students/Details.cshtml.cs* 的 `OnGetAsync` 方法使用 `FirstOrDefaultAsync` 方法來擷取單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="86c1c-147">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="86c1c-148">請新增下列醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="86c1c-148">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="86c1c-149">[Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) 及 [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) 方法會使內容載入 `Student.Enrollments` 導覽屬性，以及位於每一個註冊中的 `Enrollment.Course` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-149">The [Include](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.include) and [ThenInclude](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.theninclude#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_ThenInclude__3_Microsoft_EntityFrameworkCore_Query_IIncludableQueryable___0_System_Collections_Generic_IEnumerable___1___System_Linq_Expressions_Expression_System_Func___1___2___) methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="86c1c-150">將會在讀取相關資料教學課程中詳細檢視這些方法。</span><span class="sxs-lookup"><span data-stu-id="86c1c-150">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="86c1c-151">[AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) 方法在傳回實體未在目前內容中更新的案例下可改善效能。</span><span class="sxs-lookup"><span data-stu-id="86c1c-151">The [AsNoTracking](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.asnotracking#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_AsNoTracking__1_System_Linq_IQueryable___0__) method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="86c1c-152">`AsNoTracking` 稍後在本教學課程中將會討論。</span><span class="sxs-lookup"><span data-stu-id="86c1c-152">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="86c1c-153">在 [詳細資料] 頁面上顯示相關的註冊</span><span class="sxs-lookup"><span data-stu-id="86c1c-153">Display related enrollments on the Details page</span></span>

<span data-ttu-id="86c1c-154">開啟 *Pages/Students/Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="86c1c-154">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="86c1c-155">請新增下列醒目顯示的程式碼，以顯示一份註冊清單：</span><span class="sxs-lookup"><span data-stu-id="86c1c-155">Add the following highlighted code to display a list of enrollments:</span></span>

[!code-cshtml[](intro/samples/cu21/Pages/Students/Details.cshtml?highlight=32-53)]

<span data-ttu-id="86c1c-156">若在貼上程式碼之後，程式碼的縮排發生錯誤，請按 CTRL-K-D 修正它。</span><span class="sxs-lookup"><span data-stu-id="86c1c-156">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="86c1c-157">上述程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。</span><span class="sxs-lookup"><span data-stu-id="86c1c-157">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="86c1c-158">針對每個註冊，會顯示課程標題及成績。</span><span class="sxs-lookup"><span data-stu-id="86c1c-158">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="86c1c-159">課程標題會從儲存於 Enrollments 實體之 `Course` 導覽屬性中的課程 (Course) 實體擷取。</span><span class="sxs-lookup"><span data-stu-id="86c1c-159">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="86c1c-160">執行應用程式，選取 [Students] 索引標籤，然後按一下學生的 [詳細資料] 連結。</span><span class="sxs-lookup"><span data-stu-id="86c1c-160">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="86c1c-161">將會顯示所選取學生的課程及成績清單。</span><span class="sxs-lookup"><span data-stu-id="86c1c-161">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="86c1c-162">更新 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="86c1c-162">Update the Create page</span></span>

<span data-ttu-id="86c1c-163">以下列程式碼更新 *Pages/Students/Create.cshtml.cs* 中的 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="86c1c-163">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>

### <a name="tryupdatemodelasync"></a><span data-ttu-id="86c1c-164">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="86c1c-164">TryUpdateModelAsync</span></span>

<span data-ttu-id="86c1c-165">檢查 [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) 程式碼：</span><span class="sxs-lookup"><span data-stu-id="86c1c-165">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="86c1c-166">在上述程式碼，`TryUpdateModelAsync<Student>` 從 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel) 中的 [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) 屬性中，使用已張貼的表單值來嘗試更新 `emptyStudent` 物件。</span><span class="sxs-lookup"><span data-stu-id="86c1c-166">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel).</span></span> <span data-ttu-id="86c1c-167">`TryUpdateModelAsync` 只會更新列出的屬性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-167">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="86c1c-168">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="86c1c-168">In the preceding sample:</span></span>

* <span data-ttu-id="86c1c-169">第二個引數 (`"student", // Prefix`) 是使用來查閱值的前置詞。</span><span class="sxs-lookup"><span data-stu-id="86c1c-169">The second argument (`"student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="86c1c-170">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="86c1c-170">It's not case sensitive.</span></span>
* <span data-ttu-id="86c1c-171">已張貼的表單值會使用[模型繫結](xref:mvc/models/model-binding#how-model-binding-works)轉換至 `Student` 模型中的類型。</span><span class="sxs-lookup"><span data-stu-id="86c1c-171">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>

### <a name="overposting"></a><span data-ttu-id="86c1c-172">大量指派 (overposting)</span><span class="sxs-lookup"><span data-stu-id="86c1c-172">Overposting</span></span>

<span data-ttu-id="86c1c-173">使用 `TryUpdateModel` 來更新具有已張貼值欄位是最安全的做法，因為它會防止大量指派 (overposting)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-173">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="86c1c-174">例如，假設 Student 實體包含了一個此網頁不應更新或新增的 `Secret` 屬性：</span><span class="sxs-lookup"><span data-stu-id="86c1c-174">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="86c1c-175">即使應用程式在建立/更新 Razor 頁面上沒有 `Secret` 欄位，駭客仍然可以藉由大量指派 (overposting) 來設定 `Secret` 值。</span><span class="sxs-lookup"><span data-stu-id="86c1c-175">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="86c1c-176">駭客仍可能使用 Fiddler 等工具，或是撰寫 JavaScript，來張貼 `Secret` 表單值。</span><span class="sxs-lookup"><span data-stu-id="86c1c-176">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="86c1c-177">原始程式碼並不會限制模型繫結器在建立 Student 執行個體時所使用的欄位。</span><span class="sxs-lookup"><span data-stu-id="86c1c-177">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="86c1c-178">無論駭客在 `Secret` 表單欄位中指定了什麼值，該值都會更新到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="86c1c-178">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="86c1c-179">下列影響顯示了 Fiddler 工具將 `Secret` 欄位 (其值為 "OverPost") 新增到表單的值中。</span><span class="sxs-lookup"><span data-stu-id="86c1c-179">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 新增 [Secret] 欄位](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="86c1c-181">"OverPost" 值已成功新增至插入資料列的 `Secret` 屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-181">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="86c1c-182">應用程式設計師從未打算在 [建立] 頁面設定 `Secret` 屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-182">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>

### <a name="view-model"></a><span data-ttu-id="86c1c-183">檢視模型</span><span class="sxs-lookup"><span data-stu-id="86c1c-183">View model</span></span>

<span data-ttu-id="86c1c-184">檢視模型通常包含屬性中的子集，這些屬性包含在應用程式使用的模型中。</span><span class="sxs-lookup"><span data-stu-id="86c1c-184">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="86c1c-185">應用程式模型通常稱為網域模型。</span><span class="sxs-lookup"><span data-stu-id="86c1c-185">The application model is often called the domain model.</span></span> <span data-ttu-id="86c1c-186">網域模型通常會包含資料庫中對應實體所需要的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-186">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="86c1c-187">檢視模型只包含 UI 層所需要的屬性 (例如 [建立] 頁面)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-187">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="86c1c-188">除了檢視模型之外，某些應用程式會使用繫結模型或輸入模型，在 Razor 頁面的頁面模型類別和瀏覽器之間傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="86c1c-188">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="86c1c-189">請看看下列 `Student` 檢視模型：</span><span class="sxs-lookup"><span data-stu-id="86c1c-189">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu21/Models/StudentVM.cs)]

<span data-ttu-id="86c1c-190">檢視模型提供防止大量指派 (overposting) 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="86c1c-190">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="86c1c-191">檢視模型只包含用來檢視 (顯示) 或更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-191">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="86c1c-192">下列程式碼會使用 `StudentVM` 檢視模型來建立一名新學生：</span><span class="sxs-lookup"><span data-stu-id="86c1c-192">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="86c1c-193">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) 會設定這個物件的值，方法是藉由從其他 [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) 物件中讀取值。</span><span class="sxs-lookup"><span data-stu-id="86c1c-193">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="86c1c-194">`SetValues` 使用屬性名稱比對。</span><span class="sxs-lookup"><span data-stu-id="86c1c-194">`SetValues` uses property name matching.</span></span> <span data-ttu-id="86c1c-195">檢視模型類型不需要與模型類型相關，只需要有符合的屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-195">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="86c1c-196">使用 `StudentVM` 需要 [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) 更新為使用 `StudentVM`，而不是使用 `Student`。</span><span class="sxs-lookup"><span data-stu-id="86c1c-196">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu21/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="86c1c-197">在 Razor 頁面中，`PageModel` 的衍生類別是檢視模型。</span><span class="sxs-lookup"><span data-stu-id="86c1c-197">In Razor Pages, the `PageModel` derived class is the view model.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="86c1c-198">更新 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="86c1c-198">Update the Edit page</span></span>

<span data-ttu-id="86c1c-199">為 [編輯] 頁面更新頁面模型。</span><span class="sxs-lookup"><span data-stu-id="86c1c-199">Update the page model for the Edit page.</span></span> <span data-ttu-id="86c1c-200">主要變更已反白顯示：</span><span class="sxs-lookup"><span data-stu-id="86c1c-200">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="86c1c-201">程式碼的變更與 [建立] 頁面相似，但有一些例外狀況：</span><span class="sxs-lookup"><span data-stu-id="86c1c-201">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="86c1c-202">`OnPostAsync` 具有選擇性 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="86c1c-202">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="86c1c-203">目前的學生資料會從資料庫中擷取出來，而不會建立一個空白的學生資料。</span><span class="sxs-lookup"><span data-stu-id="86c1c-203">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="86c1c-204">`FirstOrDefaultAsync` 已取代為 [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-204">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync).</span></span> <span data-ttu-id="86c1c-205">從主索引鍵選取實體時，`FindAsync` 是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="86c1c-205">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="86c1c-206">請參閱 [FindAsync](#FindAsync) 以取得更詳細的資訊。</span><span class="sxs-lookup"><span data-stu-id="86c1c-206">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="86c1c-207">測試 [編輯] 頁面和 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="86c1c-207">Test the Edit and Create pages</span></span>

<span data-ttu-id="86c1c-208">建立並編輯一些學生實體。</span><span class="sxs-lookup"><span data-stu-id="86c1c-208">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="86c1c-209">實體狀態</span><span class="sxs-lookup"><span data-stu-id="86c1c-209">Entity States</span></span>

<span data-ttu-id="86c1c-210">無論記憶體中的實體是否與資料庫中相對應的資料列同步，資料庫內容都會持續追蹤。</span><span class="sxs-lookup"><span data-stu-id="86c1c-210">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="86c1c-211">呼叫 [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) 時，資料庫內容的同步資訊會決定該怎麼做。</span><span class="sxs-lookup"><span data-stu-id="86c1c-211">The DB context sync information determines what happens when [SaveChangesAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync#Microsoft_EntityFrameworkCore_DbContext_SaveChangesAsync_System_Threading_CancellationToken_) is called.</span></span> <span data-ttu-id="86c1c-212">例如，當傳遞一個新的實體到 [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) 方法時，該實體的狀態便會設定為 [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added)。</span><span class="sxs-lookup"><span data-stu-id="86c1c-212">For example, when a new entity is passed to the [AddAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.addasync) method, that entity's state is set to [Added](/dotnet/api/microsoft.entityframeworkcore.entitystate#Microsoft_EntityFrameworkCore_EntityState_Added).</span></span> <span data-ttu-id="86c1c-213">呼叫 `SaveChangesAsync` 時，資料庫內容會發出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="86c1c-213">When `SaveChangesAsync` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="86c1c-214">實體可為[下列狀態](/dotnet/api/microsoft.entityframeworkcore.entitystate)中的其中一個：</span><span class="sxs-lookup"><span data-stu-id="86c1c-214">An entity may be in one of the [following states](/dotnet/api/microsoft.entityframeworkcore.entitystate):</span></span>

* <span data-ttu-id="86c1c-215">`Added`：實體還不存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="86c1c-215">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="86c1c-216">`SaveChanges` 方法會發出 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="86c1c-216">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="86c1c-217">`Unchanged`：此實體沒有需要儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="86c1c-217">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="86c1c-218">從資料庫讀取時，此實體將會有這個狀態。</span><span class="sxs-lookup"><span data-stu-id="86c1c-218">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="86c1c-219">`Modified`：實體中一部分或全部的屬性值已經過修改。</span><span class="sxs-lookup"><span data-stu-id="86c1c-219">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="86c1c-220">`SaveChanges` 方法會發出 UPDATE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="86c1c-220">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="86c1c-221">`Deleted`：實體已遭標示刪除。</span><span class="sxs-lookup"><span data-stu-id="86c1c-221">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="86c1c-222">`SaveChanges` 方法會發出 DELETE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="86c1c-222">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="86c1c-223">`Detached`：實體未獲得資料庫內容追蹤。</span><span class="sxs-lookup"><span data-stu-id="86c1c-223">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="86c1c-224">在桌面應用程式中，狀態變更通常會自動進行設定。</span><span class="sxs-lookup"><span data-stu-id="86c1c-224">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="86c1c-225">實體已讀取、已進行變更，且實體狀態會自動變更為 `Modified`。</span><span class="sxs-lookup"><span data-stu-id="86c1c-225">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="86c1c-226">呼叫 `SaveChanges` 會產生 SQL UPDATE 陳述式，此陳述式只會更新變更過的屬性。</span><span class="sxs-lookup"><span data-stu-id="86c1c-226">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="86c1c-227">在 Web 應用程式中，讀取實體並顯示其資料以供編輯之用的 `DbContext` 會在頁面轉譯之後遭到處置。</span><span class="sxs-lookup"><span data-stu-id="86c1c-227">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="86c1c-228">當呼叫頁面的 `OnPostAsync` 方法時，會發出新的 Web 要求，並且會擁有一個新的 `DbContext` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="86c1c-228">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="86c1c-229">在新的內容中重新讀取實體時，會模擬桌面處理流程。</span><span class="sxs-lookup"><span data-stu-id="86c1c-229">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="86c1c-230">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="86c1c-230">Update the Delete page</span></span>

<span data-ttu-id="86c1c-231">在本節中，將新增程式碼來實作一個呼叫至 `SaveChanges` 失敗時的自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="86c1c-231">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="86c1c-232">請新增字串來包含可能的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="86c1c-232">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="86c1c-233">以下列程式碼取代 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="86c1c-233">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="86c1c-234">上述程式碼中包含選擇性參數 `saveChangesError`。</span><span class="sxs-lookup"><span data-stu-id="86c1c-234">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="86c1c-235">`saveChangesError` 會顯示出此方法是否已在刪除學生物件失敗後呼叫。</span><span class="sxs-lookup"><span data-stu-id="86c1c-235">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="86c1c-236">刪除作業可能會因為暫時性的網路問題而失敗。</span><span class="sxs-lookup"><span data-stu-id="86c1c-236">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="86c1c-237">暫時性的網路錯誤較可能是在雲端。</span><span class="sxs-lookup"><span data-stu-id="86c1c-237">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="86c1c-238">[刪除] 頁面 `OnGetAsync` 從 UI 中呼叫時，`saveChangesError` 為 false。</span><span class="sxs-lookup"><span data-stu-id="86c1c-238">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="86c1c-239">當 `OnPostAsync` 呼叫 `OnGetAsync` 時 (因刪除作業失敗)，則 `saveChangesError` 參數為 true。</span><span class="sxs-lookup"><span data-stu-id="86c1c-239">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="86c1c-240">[刪除] 頁面的 OnPostAsync 方法</span><span class="sxs-lookup"><span data-stu-id="86c1c-240">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="86c1c-241">使用下列程式碼取代 `OnPostAsync`：</span><span class="sxs-lookup"><span data-stu-id="86c1c-241">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="86c1c-242">上述程式碼會擷取選取的實體，然後呼叫 [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) 方法來將實體的狀態設定為 `Deleted`。</span><span class="sxs-lookup"><span data-stu-id="86c1c-242">The preceding code retrieves the selected entity, then calls the [Remove](/dotnet/api/microsoft.entityframeworkcore.dbcontext.remove#Microsoft_EntityFrameworkCore_DbContext_Remove_System_Object_) method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="86c1c-243">當呼叫 `SaveChanges` 時，便會產生 SQL DELETE 命令。</span><span class="sxs-lookup"><span data-stu-id="86c1c-243">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="86c1c-244">如果 `Remove` 失敗：</span><span class="sxs-lookup"><span data-stu-id="86c1c-244">If `Remove` fails:</span></span>

* <span data-ttu-id="86c1c-245">攔截到資料庫例外狀況。</span><span class="sxs-lookup"><span data-stu-id="86c1c-245">The DB exception is caught.</span></span>
* <span data-ttu-id="86c1c-246">[刪除] 頁面的 `OnGetAsync` 方法會以 `saveChangesError=true` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="86c1c-246">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="86c1c-247">更新 [刪除 Razor] 頁面</span><span class="sxs-lookup"><span data-stu-id="86c1c-247">Update the Delete Razor Page</span></span>

<span data-ttu-id="86c1c-248">將下列醒目標示的錯誤訊息新增至 [刪除 Razor] 頁面。</span><span class="sxs-lookup"><span data-stu-id="86c1c-248">Add the following highlighted error message to the Delete Razor Page.</span></span>
<!--
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?name=snippet&highlight=11)]
-->
[!code-cshtml[](intro/samples/cu21/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="86c1c-249">測試刪除。</span><span class="sxs-lookup"><span data-stu-id="86c1c-249">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="86c1c-250">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="86c1c-250">Common errors</span></span>

<span data-ttu-id="86c1c-251">Student/Index 或其他連結運作失常：</span><span class="sxs-lookup"><span data-stu-id="86c1c-251">Student/Index or other links don't work:</span></span>

<span data-ttu-id="86c1c-252">確認 Razor 頁面包含正確的 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="86c1c-252">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="86c1c-253">例如，Student/Index Razor 頁面**不應**包含路由範本：</span><span class="sxs-lookup"><span data-stu-id="86c1c-253">For example, The Student/Index Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="86c1c-254">每個 Razor 頁面都必須包含 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="86c1c-254">Each Razor Page must include the `@page` directive.</span></span>

::: moniker-end

> [!div class="step-by-step"]
> <span data-ttu-id="86c1c-255">[上一頁](xref:data/ef-rp/intro)
> [下一頁](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="86c1c-255">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
