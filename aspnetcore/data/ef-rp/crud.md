---
title: ASP.NET Core 中的 Razor 頁面與 EF Core - CRUD - 2/8
author: rick-anderson
description: 示範如何以 EF Core 來建立、讀取、更新、刪除
ms.author: riande
ms.date: 10/15/2017
uid: data/ef-rp/crud
ms.openlocfilehash: 17d48cae50745508a64a9fb8a153b7b891e64a23
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278683"
---
# <a name="razor-pages-with-ef-core-in-aspnet-core---crud---2-of-8"></a><span data-ttu-id="d3540-103">ASP.NET Core 中的 Razor 頁面與 EF Core - CRUD - 2/8</span><span class="sxs-lookup"><span data-stu-id="d3540-103">Razor Pages with EF Core in ASP.NET Core - CRUD - 2 of 8</span></span>

<span data-ttu-id="d3540-104">作者：[Tom Dykstra](https://github.com/tdykstra)、[Jon P Smith](https://twitter.com/thereformedprog) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d3540-104">By [Tom Dykstra](https://github.com/tdykstra), [Jon P Smith](https://twitter.com/thereformedprog), and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE [about the series](../../includes/RP-EF/intro.md)]

<span data-ttu-id="d3540-105">在本教學課程中，將會檢閱並自訂 Scaffold CRUD (建立、讀取、更新、刪除)。</span><span class="sxs-lookup"><span data-stu-id="d3540-105">In this tutorial, the scaffolded CRUD (create, read, update, delete) code is reviewed and customized.</span></span>

<span data-ttu-id="d3540-106">注意：為降低複雜性並將教學課程聚焦於 EF Core，EF Core 程式碼會使用於 Razor 頁面的頁面模型中。</span><span class="sxs-lookup"><span data-stu-id="d3540-106">Note: To minimize complexity and keep these tutorials focused on EF Core, EF Core code is used in the Razor Pages page models.</span></span> <span data-ttu-id="d3540-107">有些開發人員會使用服務層或儲存機制模式來建立介於 UI (Razor 頁面) 和資料存取層之間的抽象層。</span><span class="sxs-lookup"><span data-stu-id="d3540-107">Some developers use a service layer or repository pattern in to create an abstraction layer between the UI (Razor Pages) and the data access layer.</span></span>

<span data-ttu-id="d3540-108">在本教學課程中、會修改位於 *Student* 資料夾中的 [建立]、[編輯]、[刪除] 和 [詳細資料] Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="d3540-108">In this tutorial, the Create, Edit, Delete, and Details Razor Pages in the *Student* folder are modified.</span></span>

<span data-ttu-id="d3540-109">Scaffold 程式碼會為 [建立]、[編輯]、[刪除] 頁面使用下列模式：</span><span class="sxs-lookup"><span data-stu-id="d3540-109">The scaffolded code uses the following pattern for Create, Edit, and Delete pages:</span></span>

* <span data-ttu-id="d3540-110">使用 HTTP GET 方法 `OnGetAsync` 來取得並顯示所要求的資料。</span><span class="sxs-lookup"><span data-stu-id="d3540-110">Get and display the requested data with the HTTP GET method `OnGetAsync`.</span></span>
* <span data-ttu-id="d3540-111">使用 HTTP POST `OnPostAsync` 方法將變更儲存至資料。</span><span class="sxs-lookup"><span data-stu-id="d3540-111">Save changes to the data with the HTTP POST method `OnPostAsync`.</span></span>

<span data-ttu-id="d3540-112">[索引] 頁面和 [詳細資料] 頁面以 HTTP GET 方法 `OnGetAsync` 取得並顯示所要求的資料</span><span class="sxs-lookup"><span data-stu-id="d3540-112">The Index and Details pages get and display the requested data with the HTTP GET method `OnGetAsync`</span></span>

## <a name="replace-singleordefaultasync-with-firstordefaultasync"></a><span data-ttu-id="d3540-113">用 FirstOrDefaultAsync 取代 SingleOrDefaultAsync</span><span class="sxs-lookup"><span data-stu-id="d3540-113">Replace SingleOrDefaultAsync with FirstOrDefaultAsync</span></span>

<span data-ttu-id="d3540-114">產生的程式碼會使用 [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) 來擷取所要求的實體。</span><span class="sxs-lookup"><span data-stu-id="d3540-114">The generated code uses [SingleOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleOrDefaultAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)  to fetch the requested entity.</span></span> <span data-ttu-id="d3540-115">[FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) 在擷取單一實體上更有效率：</span><span class="sxs-lookup"><span data-stu-id="d3540-115">[FirstOrDefaultAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstordefaultasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstOrDefaultAsync__1_System_Linq_IQueryable___0__System_Threading_CancellationToken_) is more efficient at fetching one entity:</span></span>

* <span data-ttu-id="d3540-116">除非程式碼需要確認從查詢傳回的實體不多於一個。</span><span class="sxs-lookup"><span data-stu-id="d3540-116">Unless the code needs to verify that there's not more than one entity returned from the query.</span></span> 
* <span data-ttu-id="d3540-117">`SingleOrDefaultAsync` 擷取更多資料並進行不必要的工作。</span><span class="sxs-lookup"><span data-stu-id="d3540-117">`SingleOrDefaultAsync` fetches more data and does unnecessary work.</span></span>
* <span data-ttu-id="d3540-118">如果有多於一個符合篩選組件的實體，`SingleOrDefaultAsync` 將會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d3540-118">`SingleOrDefaultAsync` throws an exception if there's more than one entity that fits the filter part.</span></span>
*  <span data-ttu-id="d3540-119">如果有多於一個符合篩選組件的實體，`FirstOrDefaultAsync` 不會擲回。</span><span class="sxs-lookup"><span data-stu-id="d3540-119">`FirstOrDefaultAsync` doesn't throw if there's more than one entity that fits the filter part.</span></span>

<span data-ttu-id="d3540-120">將 `SingleOrDefaultAsync` 全域取代為 `FirstOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="d3540-120">Globally replace `SingleOrDefaultAsync` with `FirstOrDefaultAsync`.</span></span> <span data-ttu-id="d3540-121">`SingleOrDefaultAsync` 使用於 5 個地方：</span><span class="sxs-lookup"><span data-stu-id="d3540-121">`SingleOrDefaultAsync` is used in 5 places:</span></span>

* <span data-ttu-id="d3540-122">`OnGetAsync` 在 [詳細資料] 頁面中。</span><span class="sxs-lookup"><span data-stu-id="d3540-122">`OnGetAsync` in the Details page.</span></span>
* <span data-ttu-id="d3540-123">`OnGetAsync` 和 `OnPostAsync` 在 [編輯] 和 [刪除] 頁面中。</span><span class="sxs-lookup"><span data-stu-id="d3540-123">`OnGetAsync` and `OnPostAsync` in the Edit and Delete pages.</span></span>

<a name="FindAsync"></a>
### <a name="findasync"></a><span data-ttu-id="d3540-124">FindAsync</span><span class="sxs-lookup"><span data-stu-id="d3540-124">FindAsync</span></span>

<span data-ttu-id="d3540-125">在大部分的 Scaffold 程式碼中，[FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) 可用來取代 `FirstOrDefaultAsync` 或 `SingleOrDefaultAsync`。</span><span class="sxs-lookup"><span data-stu-id="d3540-125">In much of the scaffolded code, [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbcontext.findasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_DbContext_FindAsync_System_Type_System_Object___) can be used in place of `FirstOrDefaultAsync` or `SingleOrDefaultAsync`.</span></span> 

<span data-ttu-id="d3540-126">`FindAsync`：</span><span class="sxs-lookup"><span data-stu-id="d3540-126">`FindAsync`:</span></span>

* <span data-ttu-id="d3540-127">尋找含有主索引鍵 (PK) 的實體。</span><span class="sxs-lookup"><span data-stu-id="d3540-127">Finds an entity with the primary key (PK).</span></span> <span data-ttu-id="d3540-128">如果內容正在追蹤含有主索引鍵的實體，會在沒有傳送要求至資料庫的情況下傳回。</span><span class="sxs-lookup"><span data-stu-id="d3540-128">If an entity with the PK is being tracked by the context, it's returned without a request to the DB.</span></span>
* <span data-ttu-id="d3540-129">簡單並精確。</span><span class="sxs-lookup"><span data-stu-id="d3540-129">Is simple and concise.</span></span>
* <span data-ttu-id="d3540-130">已最佳化來查閱單一實體。</span><span class="sxs-lookup"><span data-stu-id="d3540-130">Is optimized to look up a single entity.</span></span>
* <span data-ttu-id="d3540-131">在某些情況下可具有效能優勢，但是在一般的 Web 案例下很少見。</span><span class="sxs-lookup"><span data-stu-id="d3540-131">Can have perf benefits in some situations, but they rarely come into play for normal web scenarios.</span></span>
* <span data-ttu-id="d3540-132">隱含使用 [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) 而不是 [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_)。</span><span class="sxs-lookup"><span data-stu-id="d3540-132">Implicitly uses [FirstAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.firstasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_FirstAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_) instead of [SingleAsync](/dotnet/api/microsoft.entityframeworkcore.entityframeworkqueryableextensions.singleasync?view=efcore-2.0#Microsoft_EntityFrameworkCore_EntityFrameworkQueryableExtensions_SingleAsync__1_System_Linq_IQueryable___0__System_Linq_Expressions_Expression_System_Func___0_System_Boolean___System_Threading_CancellationToken_).</span></span>
<span data-ttu-id="d3540-133">但是如果您想要包含其他項目，則 [尋找] 已不再適用。</span><span class="sxs-lookup"><span data-stu-id="d3540-133">But if you want to Include other entities, then Find is no longer appropriate.</span></span> <span data-ttu-id="d3540-134">這表示您可能要放棄 [尋找] 並隨著您的應用程式進行而移至查詢。</span><span class="sxs-lookup"><span data-stu-id="d3540-134">This means that you may need to abandon Find and move to a query as your app progresses.</span></span>

## <a name="customize-the-details-page"></a><span data-ttu-id="d3540-135">自訂 [詳細資料] 頁面</span><span class="sxs-lookup"><span data-stu-id="d3540-135">Customize the Details page</span></span>

<span data-ttu-id="d3540-136">瀏覽至 `Pages/Students` 頁面。</span><span class="sxs-lookup"><span data-stu-id="d3540-136">Browse to `Pages/Students` page.</span></span> <span data-ttu-id="d3540-137">在 *Pages/Students/Index.cshtml* 檔案中，**編輯**、**詳細資料**、**刪除** 連結是由[錨點標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)所產生。</span><span class="sxs-lookup"><span data-stu-id="d3540-137">The **Edit**, **Details**, and **Delete** links are generated by the [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) in the *Pages/Students/Index.cshtml* file.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Index1.cshtml?range=40-44)]

<span data-ttu-id="d3540-138">選取詳細資料連結。</span><span class="sxs-lookup"><span data-stu-id="d3540-138">Select a Details link.</span></span> <span data-ttu-id="d3540-139">URL 的格式為 `http://localhost:5000/Students/Details?id=2`。</span><span class="sxs-lookup"><span data-stu-id="d3540-139">The URL is of the form `http://localhost:5000/Students/Details?id=2`.</span></span> <span data-ttu-id="d3540-140">使用 (`?id=2`) 查詢字串來傳遞 Student ID。</span><span class="sxs-lookup"><span data-stu-id="d3540-140">The Student ID is passed using a query string (`?id=2`).</span></span>

<span data-ttu-id="d3540-141">更新 [編輯]、[詳細資料] 和 [刪除] Razor 頁面，以使用 `"{id:int}"` 路由範本。</span><span class="sxs-lookup"><span data-stu-id="d3540-141">Update the Edit, Details, and Delete Razor Pages to use the `"{id:int}"` route template.</span></span> <span data-ttu-id="d3540-142">將這些頁面每一頁的頁面指示詞從 `@page` 變更為 `@page "{id:int}"`。</span><span class="sxs-lookup"><span data-stu-id="d3540-142">Change the page directive for each of these pages from `@page` to `@page "{id:int}"`.</span></span>

<span data-ttu-id="d3540-143">對使用 "{id:int}" 路由範本的頁面提出的要求若**未**包含整數路由值，將傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d3540-143">A request to the page with the "{id:int}" route template that does **not** include a integer route value returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="d3540-144">例如，`http://localhost:5000/Students/Details` 傳回 404 錯誤。</span><span class="sxs-lookup"><span data-stu-id="d3540-144">For example, `http://localhost:5000/Students/Details` returns a 404 error.</span></span> <span data-ttu-id="d3540-145">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="d3540-145">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="d3540-146">執行應用程式，按一下詳細資料連結，並確認 URL 作為路由資料 (`http://localhost:5000/Students/Details/2`) 來傳遞識別碼。</span><span class="sxs-lookup"><span data-stu-id="d3540-146">Run the app, click on a Details link, and verify the URL is passing the ID as route data (`http://localhost:5000/Students/Details/2`).</span></span>

<span data-ttu-id="d3540-147">請勿全域變更 `@page` 至 `@page "{id:int}"`，這麼做會中斷 [首頁] 頁面與 [建立] 頁面之間的連結。</span><span class="sxs-lookup"><span data-stu-id="d3540-147">Don't globally change `@page` to `@page "{id:int}"`, doing so breaks the links to the Home and Create pages.</span></span>

<!-- See https://github.com/aspnet/Scaffolding/issues/590 -->

### <a name="add-related-data"></a><span data-ttu-id="d3540-148">新增相關資料</span><span class="sxs-lookup"><span data-stu-id="d3540-148">Add related data</span></span>

<span data-ttu-id="d3540-149">Students [索引] 頁面的 Scaffold 程式碼不包含 `Enrollments` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-149">The scaffolded code for the Students Index page doesn't include the `Enrollments` property.</span></span> <span data-ttu-id="d3540-150">在本節中，`Enrollments` 集合的內容會顯示於 [詳細資料] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d3540-150">In this section, the contents of the `Enrollments` collection is displayed in the Details page.</span></span>

<span data-ttu-id="d3540-151">*Pages/Students/Details.cshtml.cs* 的 `OnGetAsync` 方法使用 `FirstOrDefaultAsync` 方法來擷取單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="d3540-151">The `OnGetAsync` method of *Pages/Students/Details.cshtml.cs* uses the `FirstOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="d3540-152">請新增下列醒目提示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d3540-152">Add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Details.cshtml.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="d3540-153">`Include` 及 `ThenInclude` 方法會使內容載入 `Student.Enrollments` 導覽屬性，以及位於每一個註冊中的 `Enrollment.Course` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-153">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span> <span data-ttu-id="d3540-154">將會在讀取相關資料教學課程中詳細檢視這些方法。</span><span class="sxs-lookup"><span data-stu-id="d3540-154">These methods are examined in detail in the reading-related data tutorial.</span></span>

<span data-ttu-id="d3540-155">`AsNoTracking` 方法在傳回實體未在目前內容中更新的案例下可改善效能。</span><span class="sxs-lookup"><span data-stu-id="d3540-155">The `AsNoTracking` method improves performance in scenarios when the entities returned are not updated in the current context.</span></span> <span data-ttu-id="d3540-156">`AsNoTracking` 稍後在本教學課程中將會討論。</span><span class="sxs-lookup"><span data-stu-id="d3540-156">`AsNoTracking` is discussed later in this tutorial.</span></span>

### <a name="display-related-enrollments-on-the-details-page"></a><span data-ttu-id="d3540-157">在 [詳細資料] 頁面上顯示相關的註冊</span><span class="sxs-lookup"><span data-stu-id="d3540-157">Display related enrollments on the Details page</span></span>

<span data-ttu-id="d3540-158">開啟 *Pages/Students/Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="d3540-158">Open *Pages/Students/Details.cshtml*.</span></span> <span data-ttu-id="d3540-159">請新增下列醒目顯示的程式碼，以顯示一份註冊清單：</span><span class="sxs-lookup"><span data-stu-id="d3540-159">Add the following highlighted code to display a list of enrollments:</span></span>

 <span data-ttu-id="d3540-160"><!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]</span><span class="sxs-lookup"><span data-stu-id="d3540-160"><!--2do ricka. if doesn't change, remove dup --> [!code-cshtml[](intro/samples/cu/Pages/Students/Details1.cshtml?highlight=32-53)]</span></span>

<span data-ttu-id="d3540-161">若在貼上程式碼之後，程式碼的縮排發生錯誤，請按 CTRL-K-D 修正它。</span><span class="sxs-lookup"><span data-stu-id="d3540-161">If code indentation is wrong after the code is pasted, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="d3540-162">上述程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。</span><span class="sxs-lookup"><span data-stu-id="d3540-162">The preceding code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="d3540-163">針對每個註冊，會顯示課程標題及成績。</span><span class="sxs-lookup"><span data-stu-id="d3540-163">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="d3540-164">課程標題會從儲存於 Enrollments 實體之 `Course` 導覽屬性中的課程 (Course) 實體擷取。</span><span class="sxs-lookup"><span data-stu-id="d3540-164">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="d3540-165">執行應用程式，選取 [Students] 索引標籤，然後按一下學生的 [詳細資料] 連結。</span><span class="sxs-lookup"><span data-stu-id="d3540-165">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="d3540-166">將會顯示所選取學生的課程及成績清單。</span><span class="sxs-lookup"><span data-stu-id="d3540-166">The list of courses and grades for the selected student is displayed.</span></span>

## <a name="update-the-create-page"></a><span data-ttu-id="d3540-167">更新 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="d3540-167">Update the Create page</span></span>

<span data-ttu-id="d3540-168">以下列程式碼更新 *Pages/Students/Create.cshtml.cs* 中的 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d3540-168">Update the `OnPostAsync` method in *Pages/Students/Create.cshtml.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_OnPostAsync)]

<a name="TryUpdateModelAsync"></a>
### <a name="tryupdatemodelasync"></a><span data-ttu-id="d3540-169">TryUpdateModelAsync</span><span class="sxs-lookup"><span data-stu-id="d3540-169">TryUpdateModelAsync</span></span>

<span data-ttu-id="d3540-170">檢查 [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) 程式碼：</span><span class="sxs-lookup"><span data-stu-id="d3540-170">Examine the [TryUpdateModelAsync](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.tryupdatemodelasync#Microsoft_AspNetCore_Mvc_ControllerBase_TryUpdateModelAsync_System_Object_System_Type_System_String_) code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Create.cshtml.cs?name=snippet_TryUpdateModelAsync)]

<span data-ttu-id="d3540-171">在上述程式碼，`TryUpdateModelAsync<Student>` 從 [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0) 中的 [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) 屬性中，使用已張貼的表單值來嘗試更新 `emptyStudent` 物件。</span><span class="sxs-lookup"><span data-stu-id="d3540-171">In the preceding code, `TryUpdateModelAsync<Student>` tries to update the `emptyStudent` object using the posted form values from the [PageContext](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel.pagecontext?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_RazorPages_PageModel_PageContext) property in the [PageModel](/dotnet/api/microsoft.aspnetcore.mvc.razorpages.pagemodel?view=aspnetcore-2.0).</span></span> <span data-ttu-id="d3540-172">`TryUpdateModelAsync` 只會更新列出的屬性 (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`)。</span><span class="sxs-lookup"><span data-stu-id="d3540-172">`TryUpdateModelAsync` only updates the properties listed (`s => s.FirstMidName, s => s.LastName, s => s.EnrollmentDate`).</span></span>

<span data-ttu-id="d3540-173">在上述範例中：</span><span class="sxs-lookup"><span data-stu-id="d3540-173">In the preceding sample:</span></span>

* <span data-ttu-id="d3540-174">第二個引數 (` "student", // Prefix`) 是使用來查閱值的前置詞。</span><span class="sxs-lookup"><span data-stu-id="d3540-174">The second argument (` "student", // Prefix`) is the prefix uses to look up values.</span></span> <span data-ttu-id="d3540-175">不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="d3540-175">It's not case sensitive.</span></span>
* <span data-ttu-id="d3540-176">已張貼的表單值會使用[模型繫結](xref:mvc/models/model-binding#how-model-binding-works)轉換至 `Student` 模型中的類型。</span><span class="sxs-lookup"><span data-stu-id="d3540-176">The posted form values are converted to the types in the `Student` model using [model binding](xref:mvc/models/model-binding#how-model-binding-works).</span></span>

<a id="overpost"></a>
### <a name="overposting"></a><span data-ttu-id="d3540-177">大量指派 (overposting)</span><span class="sxs-lookup"><span data-stu-id="d3540-177">Overposting</span></span>

<span data-ttu-id="d3540-178">使用 `TryUpdateModel` 來更新具有已張貼值欄位是最安全的做法，因為它會防止大量指派 (overposting)。</span><span class="sxs-lookup"><span data-stu-id="d3540-178">Using `TryUpdateModel` to update fields with posted values is a security best practice because it prevents overposting.</span></span> <span data-ttu-id="d3540-179">例如，假設 Student 實體包含了一個此網頁不應更新或新增的 `Secret` 屬性：</span><span class="sxs-lookup"><span data-stu-id="d3540-179">For example, suppose the Student entity includes a `Secret` property that this web page shouldn't update or add:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentZsecret.cs?name=snippet_Intro&highlight=7)]

<span data-ttu-id="d3540-180">即使應用程式在建立/更新 Razor 頁面上沒有 `Secret` 欄位，駭客仍然可以藉由大量指派 (overposting) 來設定 `Secret` 值。</span><span class="sxs-lookup"><span data-stu-id="d3540-180">Even if the app doesn't have a `Secret` field on the create/update Razor Page, a hacker could set the `Secret` value by overposting.</span></span> <span data-ttu-id="d3540-181">駭客仍可能使用 Fiddler 等工具，或是撰寫 JavaScript，來張貼 `Secret` 表單值。</span><span class="sxs-lookup"><span data-stu-id="d3540-181">A hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="d3540-182">原始程式碼並不會限制模型繫結器在建立 Student 執行個體時所使用的欄位。</span><span class="sxs-lookup"><span data-stu-id="d3540-182">The original code doesn't limit the fields that the model binder uses when it creates a Student instance.</span></span>

<span data-ttu-id="d3540-183">無論駭客在 `Secret` 表單欄位中指定了什麼值，該值都會更新到資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d3540-183">Whatever value the hacker specified for the `Secret` form field is updated in the DB.</span></span> <span data-ttu-id="d3540-184">下列影響顯示了 Fiddler 工具將 `Secret` 欄位 (其值為 "OverPost") 新增到表單的值中。</span><span class="sxs-lookup"><span data-stu-id="d3540-184">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 新增 [Secret] 欄位](../ef-mvc/crud/_static/fiddler.png)

<span data-ttu-id="d3540-186">"OverPost" 值已成功新增至插入資料列的 `Secret` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-186">The value "OverPost" is successfully added to the `Secret` property of the inserted row.</span></span> <span data-ttu-id="d3540-187">應用程式設計師從未打算在 [建立] 頁面設定 `Secret` 屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-187">The app designer never intended the `Secret` property to be set with the Create page.</span></span>

<a name="vm"></a>
### <a name="view-model"></a><span data-ttu-id="d3540-188">檢視模型</span><span class="sxs-lookup"><span data-stu-id="d3540-188">View model</span></span>

<span data-ttu-id="d3540-189">檢視模型通常包含屬性中的子集，這些屬性包含在應用程式使用的模型中。</span><span class="sxs-lookup"><span data-stu-id="d3540-189">A view model typically contains a subset of the properties included in the model used by the application.</span></span> <span data-ttu-id="d3540-190">應用程式模型通常稱為網域模型。</span><span class="sxs-lookup"><span data-stu-id="d3540-190">The application model is often called the domain model.</span></span> <span data-ttu-id="d3540-191">網域模型通常會包含資料庫中對應實體所需要的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-191">The domain model typically contains all the properties required by the corresponding entity in the DB.</span></span> <span data-ttu-id="d3540-192">檢視模型只包含 UI 層所需要的屬性 (例如 [建立] 頁面)。</span><span class="sxs-lookup"><span data-stu-id="d3540-192">The view model contains only the properties needed for the UI layer (for example, the Create page).</span></span> <span data-ttu-id="d3540-193">除了檢視模型之外，某些應用程式會使用繫結模型或輸入模型，在 Razor 頁面的頁面模型類別和瀏覽器之間傳遞資料。</span><span class="sxs-lookup"><span data-stu-id="d3540-193">In addition to the view model, some apps use a binding model or input model to pass data between the Razor Pages page model class and the browser.</span></span> <span data-ttu-id="d3540-194">請看看下列 `Student` 檢視模型：</span><span class="sxs-lookup"><span data-stu-id="d3540-194">Consider the following `Student` view model:</span></span>

[!code-csharp[](intro/samples/cu/Models/StudentVM.cs)]

<span data-ttu-id="d3540-195">檢視模型提供防止大量指派 (overposting) 的替代方法。</span><span class="sxs-lookup"><span data-stu-id="d3540-195">View models provide an alternative way to prevent overposting.</span></span> <span data-ttu-id="d3540-196">檢視模型只包含用來檢視 (顯示) 或更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-196">The view model contains only the properties to view (display) or update.</span></span>

<span data-ttu-id="d3540-197">下列程式碼會使用 `StudentVM` 檢視模型來建立一名新學生：</span><span class="sxs-lookup"><span data-stu-id="d3540-197">The following code uses the `StudentVM` view model to create a new student:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/CreateVM.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d3540-198">[SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) 會設定這個物件的值，方法是藉由從其他 [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) 物件中讀取值。</span><span class="sxs-lookup"><span data-stu-id="d3540-198">The [SetValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues.setvalues?view=efcore-2.0#Microsoft_EntityFrameworkCore_ChangeTracking_PropertyValues_SetValues_System_Object_) method sets the values of this object by reading values from another [PropertyValues](/dotnet/api/microsoft.entityframeworkcore.changetracking.propertyvalues) object.</span></span> <span data-ttu-id="d3540-199">`SetValues` 使用屬性名稱比對。</span><span class="sxs-lookup"><span data-stu-id="d3540-199">`SetValues` uses property name matching.</span></span> <span data-ttu-id="d3540-200">檢視模型類型不需要與模型類型相關，只需要有符合的屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-200">The view model type doesn't need to be related to the model type, it just needs to have properties that match.</span></span>

<span data-ttu-id="d3540-201">使用 `StudentVM` 需要 [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) 更新為使用 `StudentVM`，而不是使用 `Student`。</span><span class="sxs-lookup"><span data-stu-id="d3540-201">Using `StudentVM` requires [CreateVM.cshtml](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu/Pages/Students/CreateVM.cshtml) be updated to use `StudentVM` rather than `Student`.</span></span>

<span data-ttu-id="d3540-202">在 Razor 頁面中，`PageModel` 的衍生類別是檢視模型。</span><span class="sxs-lookup"><span data-stu-id="d3540-202">In Razor Pages, the `PageModel` derived class is the view model.</span></span> 

## <a name="update-the-edit-page"></a><span data-ttu-id="d3540-203">更新 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="d3540-203">Update the Edit page</span></span>

<span data-ttu-id="d3540-204">為 [編輯] 頁面更新頁面模型。</span><span class="sxs-lookup"><span data-stu-id="d3540-204">Update the page model for the Edit page.</span></span> <span data-ttu-id="d3540-205">主要變更已反白顯示：</span><span class="sxs-lookup"><span data-stu-id="d3540-205">The major changes are highlighted:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Edit.cshtml.cs?name=snippet_OnPostAsync&highlight=20,36)]

<span data-ttu-id="d3540-206">程式碼的變更與 [建立] 頁面相似，但有一些例外狀況：</span><span class="sxs-lookup"><span data-stu-id="d3540-206">The code changes are similar to the Create page with a few exceptions:</span></span>

* <span data-ttu-id="d3540-207">`OnPostAsync` 具有選擇性 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="d3540-207">`OnPostAsync` has an optional `id` parameter.</span></span>
* <span data-ttu-id="d3540-208">目前的學生資料會從資料庫中擷取出來，而不會建立一個空白的學生資料。</span><span class="sxs-lookup"><span data-stu-id="d3540-208">The current student is fetched from the DB, rather than creating an empty student.</span></span>
* <span data-ttu-id="d3540-209">`FirstOrDefaultAsync` 已取代為 [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0)。</span><span class="sxs-lookup"><span data-stu-id="d3540-209">`FirstOrDefaultAsync` has been replaced with [FindAsync](/dotnet/api/microsoft.entityframeworkcore.dbset-1.findasync?view=efcore-2.0).</span></span> <span data-ttu-id="d3540-210">從主索引鍵選取實體時，`FindAsync` 是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="d3540-210">`FindAsync` is a good choice when selecting an entity from the primary key.</span></span> <span data-ttu-id="d3540-211">請參閱 [FindAsync](#FindAsync) 以取得更詳細的資訊。</span><span class="sxs-lookup"><span data-stu-id="d3540-211">See [FindAsync](#FindAsync) for more information.</span></span>

### <a name="test-the-edit-and-create-pages"></a><span data-ttu-id="d3540-212">測試 [編輯] 頁面和 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="d3540-212">Test the Edit and Create pages</span></span>

<span data-ttu-id="d3540-213">建立並編輯一些學生實體。</span><span class="sxs-lookup"><span data-stu-id="d3540-213">Create and edit a few student entities.</span></span>

## <a name="entity-states"></a><span data-ttu-id="d3540-214">實體狀態</span><span class="sxs-lookup"><span data-stu-id="d3540-214">Entity States</span></span>

<span data-ttu-id="d3540-215">無論記憶體中的實體是否與資料庫中相對應的資料列同步，資料庫內容都會持續追蹤。</span><span class="sxs-lookup"><span data-stu-id="d3540-215">The DB context keeps track of whether entities in memory are in sync with their corresponding rows in the DB.</span></span> <span data-ttu-id="d3540-216">呼叫 `SaveChanges` 時，資料庫內容的同步資訊會決定該怎麼做。</span><span class="sxs-lookup"><span data-stu-id="d3540-216">The DB context sync information determines what happens when `SaveChanges` is called.</span></span> <span data-ttu-id="d3540-217">例如，當傳遞一個新的實體到 `Add` 方法時，該實體的狀態便會設定為 `Added`。</span><span class="sxs-lookup"><span data-stu-id="d3540-217">For example, when a new entity is passed to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="d3540-218">呼叫 `SaveChanges` 時，資料庫內容會發出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="d3540-218">When `SaveChanges` is called, the DB context issues a SQL INSERT command.</span></span>

<span data-ttu-id="d3540-219">實體可為下列狀態中的其中一個：</span><span class="sxs-lookup"><span data-stu-id="d3540-219">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="d3540-220">`Added`：實體還不存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d3540-220">`Added`: The entity doesn't yet exist in the DB.</span></span> <span data-ttu-id="d3540-221">`SaveChanges` 方法會發出 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d3540-221">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="d3540-222">`Unchanged`：此實體沒有需要儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="d3540-222">`Unchanged`: No changes need to be saved with this entity.</span></span> <span data-ttu-id="d3540-223">從資料庫讀取時，此實體將會有這個狀態。</span><span class="sxs-lookup"><span data-stu-id="d3540-223">An entity has this status when it's read from the DB.</span></span>

* <span data-ttu-id="d3540-224">`Modified`：實體中一部分或全部的屬性值已經過修改。</span><span class="sxs-lookup"><span data-stu-id="d3540-224">`Modified`: Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="d3540-225">`SaveChanges` 方法會發出 UPDATE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d3540-225">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="d3540-226">`Deleted`：實體已遭標示刪除。</span><span class="sxs-lookup"><span data-stu-id="d3540-226">`Deleted`: The entity has been marked for deletion.</span></span> <span data-ttu-id="d3540-227">`SaveChanges` 方法會發出 DELETE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="d3540-227">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="d3540-228">`Detached`：實體未獲得資料庫內容追蹤。</span><span class="sxs-lookup"><span data-stu-id="d3540-228">`Detached`: The entity isn't being tracked by the DB context.</span></span>

<span data-ttu-id="d3540-229">在桌面應用程式中，狀態變更通常會自動進行設定。</span><span class="sxs-lookup"><span data-stu-id="d3540-229">In a desktop app, state changes are typically set automatically.</span></span> <span data-ttu-id="d3540-230">實體已讀取、已進行變更，且實體狀態會自動變更為 `Modified`。</span><span class="sxs-lookup"><span data-stu-id="d3540-230">An entity is read, changes are made, and the entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="d3540-231">呼叫 `SaveChanges` 會產生 SQL UPDATE 陳述式，此陳述式只會更新變更過的屬性。</span><span class="sxs-lookup"><span data-stu-id="d3540-231">Calling `SaveChanges` generates a SQL UPDATE statement that updates only the changed properties.</span></span>

<span data-ttu-id="d3540-232">在 Web 應用程式中，讀取實體並顯示其資料以供編輯之用的 `DbContext` 會在頁面轉譯之後遭到處置。</span><span class="sxs-lookup"><span data-stu-id="d3540-232">In a web app, the `DbContext` that reads an entity and displays the data is disposed after a page is rendered.</span></span> <span data-ttu-id="d3540-233">當呼叫頁面的 `OnPostAsync` 方法時，會發出新的 Web 要求，並且會擁有一個新的 `DbContext` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d3540-233">When a page's `OnPostAsync` method is called, a new web request is made and with a new instance of the `DbContext`.</span></span> <span data-ttu-id="d3540-234">在新的內容中重新讀取實體時，會模擬桌面處理流程。</span><span class="sxs-lookup"><span data-stu-id="d3540-234">Re-reading the entity in that new context simulates desktop processing.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="d3540-235">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="d3540-235">Update the Delete page</span></span>

<span data-ttu-id="d3540-236">在本節中，將新增程式碼來實作一個呼叫至 `SaveChanges` 失敗時的自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="d3540-236">In this section, code is added to implement a custom error message when the call to `SaveChanges` fails.</span></span> <span data-ttu-id="d3540-237">請新增字串來包含可能的錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="d3540-237">Add a string to contain possible error messages:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet1&highlight=12)]

<span data-ttu-id="d3540-238">以下列程式碼取代 `OnGetAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="d3540-238">Replace the `OnGetAsync` method with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnGetAsync&highlight=1,9,17-20)]

<span data-ttu-id="d3540-239">上述程式碼中包含選擇性參數 `saveChangesError`。</span><span class="sxs-lookup"><span data-stu-id="d3540-239">The preceding code contains the optional parameter `saveChangesError`.</span></span> <span data-ttu-id="d3540-240">`saveChangesError` 會顯示出此方法是否已在刪除學生物件失敗後呼叫。</span><span class="sxs-lookup"><span data-stu-id="d3540-240">`saveChangesError` indicates whether the method was called after a failure to delete the student object.</span></span> <span data-ttu-id="d3540-241">刪除作業可能會因為暫時性的網路問題而失敗。</span><span class="sxs-lookup"><span data-stu-id="d3540-241">The delete operation might fail because of transient network problems.</span></span> <span data-ttu-id="d3540-242">暫時性的網路錯誤較可能是在雲端。</span><span class="sxs-lookup"><span data-stu-id="d3540-242">Transient network errors are more likely in the cloud.</span></span> <span data-ttu-id="d3540-243">[刪除] 頁面 `OnGetAsync` 從 UI 中呼叫時，`saveChangesError` 為 false。</span><span class="sxs-lookup"><span data-stu-id="d3540-243">`saveChangesError`is false when the Delete page `OnGetAsync` is called from the UI.</span></span> <span data-ttu-id="d3540-244">當 `OnPostAsync` 呼叫 `OnGetAsync` 時 (因刪除作業失敗)，則 `saveChangesError` 參數為 true。</span><span class="sxs-lookup"><span data-stu-id="d3540-244">When `OnGetAsync` is called by `OnPostAsync` (because the delete operation failed), the `saveChangesError` parameter is true.</span></span>

### <a name="the-delete-pages-onpostasync-method"></a><span data-ttu-id="d3540-245">[刪除] 頁面的 OnPostAsync 方法</span><span class="sxs-lookup"><span data-stu-id="d3540-245">The Delete pages OnPostAsync method</span></span>

<span data-ttu-id="d3540-246">使用下列程式碼取代 `OnPostAsync`：</span><span class="sxs-lookup"><span data-stu-id="d3540-246">Replace the `OnPostAsync` with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Delete.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="d3540-247">上述程式碼會擷取選取的實體，然後呼叫 `Remove` 方法來將實體的狀態設定為 `Deleted`。</span><span class="sxs-lookup"><span data-stu-id="d3540-247">The preceding code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="d3540-248">當呼叫 `SaveChanges` 時，便會產生 SQL DELETE 命令。</span><span class="sxs-lookup"><span data-stu-id="d3540-248">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span> <span data-ttu-id="d3540-249">如果 `Remove` 失敗：</span><span class="sxs-lookup"><span data-stu-id="d3540-249">If `Remove` fails:</span></span>

* <span data-ttu-id="d3540-250">攔截到資料庫例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d3540-250">The DB exception is caught.</span></span>
* <span data-ttu-id="d3540-251">[刪除] 頁面的 `OnGetAsync` 方法會以 `saveChangesError=true` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="d3540-251">The Delete pages `OnGetAsync` method is called with `saveChangesError=true`.</span></span>

### <a name="update-the-delete-razor-page"></a><span data-ttu-id="d3540-252">更新 [刪除 Razor] 頁面</span><span class="sxs-lookup"><span data-stu-id="d3540-252">Update the Delete Razor Page</span></span>

<span data-ttu-id="d3540-253">將下列醒目標示的錯誤訊息新增至 [刪除 Razor] 頁面。</span><span class="sxs-lookup"><span data-stu-id="d3540-253">Add the following highlighted error message to the Delete Razor Page.</span></span>

[!code-cshtml[](intro/samples/cu/Pages/Students/Delete.cshtml?range=1-13&highlight=10)]

<span data-ttu-id="d3540-254">測試刪除。</span><span class="sxs-lookup"><span data-stu-id="d3540-254">Test Delete.</span></span>

## <a name="common-errors"></a><span data-ttu-id="d3540-255">常見的錯誤</span><span class="sxs-lookup"><span data-stu-id="d3540-255">Common errors</span></span>

<span data-ttu-id="d3540-256">Student/首頁或其他連結運作失常：</span><span class="sxs-lookup"><span data-stu-id="d3540-256">Student/Home or other links don't work:</span></span>

<span data-ttu-id="d3540-257">確認 Razor 頁面包含正確的 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="d3540-257">Verify the Razor Page contains the correct `@page` directive.</span></span> <span data-ttu-id="d3540-258">比方說，Student/Razor 首頁**不應**包含路由範本：</span><span class="sxs-lookup"><span data-stu-id="d3540-258">For example, The Student/Home Razor Page should **not** contain a route template:</span></span>

```cshtml
@page "{id:int}"
```

<span data-ttu-id="d3540-259">每個 Razor 頁面都必須包含 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="d3540-259">Each Razor Page must include the `@page` directive.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d3540-260">[上一頁](xref:data/ef-rp/intro)
> [下一頁](xref:data/ef-rp/sort-filter-page)</span><span class="sxs-lookup"><span data-stu-id="d3540-260">[Previous](xref:data/ef-rp/intro)
[Next](xref:data/ef-rp/sort-filter-page)</span></span>
