---
title: ASP.NET Core MVC 和 EF Core - CRUD - 2/10
author: rick-anderson
description: ''
ms.author: tdykstra
ms.date: 03/15/2017
uid: data/ef-mvc/crud
ms.openlocfilehash: e0d454ce4f2319b48b649d46c0878d6969acbc9f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278553"
---
# <a name="aspnet-core-mvc-with-ef-core---crud---2-of-10"></a><span data-ttu-id="a1c70-102">ASP.NET Core MVC 和 EF Core - CRUD - 2/10</span><span class="sxs-lookup"><span data-stu-id="a1c70-102">ASP.NET Core MVC with EF Core - CRUD - 2 of 10</span></span>

<span data-ttu-id="a1c70-103">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a1c70-103">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a1c70-104">Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a1c70-104">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a1c70-105">如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-105">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a1c70-106">在前一個教學課程中，您建立了一個使用 Entity Framework 及 SQL Server LocalDB 來儲存及顯示資料的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a1c70-106">In the previous tutorial, you created an MVC application that stores and displays data using the Entity Framework and SQL Server LocalDB.</span></span> <span data-ttu-id="a1c70-107">在本教學課程中，您將檢閱並自訂 MVC Scaffolding 自動為您在控制器及檢視中建立的 CRUD (建立、讀取、更新、刪除) 程式碼。</span><span class="sxs-lookup"><span data-stu-id="a1c70-107">In this tutorial, you'll review and customize the CRUD (create, read, update, delete) code that the MVC scaffolding automatically creates for you in controllers and views.</span></span>

> [!NOTE] 
> <span data-ttu-id="a1c70-108">實作儲存機制模式，以在您的控制器及資料存取層之間建立抽象層是一種非常常見的做法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-108">It's a common practice to implement the repository pattern in order to create an abstraction layer between your controller and the data access layer.</span></span> <span data-ttu-id="a1c70-109">為了使這些教學課程維持簡單，並聚焦於教導如何使用 Entity Framework，課程中將不會使用儲存機制。</span><span class="sxs-lookup"><span data-stu-id="a1c70-109">To keep these tutorials simple and focused on teaching how to use the Entity Framework itself, they don't use repositories.</span></span> <span data-ttu-id="a1c70-110">如需儲存機制的詳細資訊，請參閱[本系列的最後一個教學課程](advanced.md)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-110">For information about repositories with EF, see [the last tutorial in this series](advanced.md).</span></span>

<span data-ttu-id="a1c70-111">在本教學課程中，您將使用下列網頁：</span><span class="sxs-lookup"><span data-stu-id="a1c70-111">In this tutorial, you'll work with the following web pages:</span></span>

![Student [詳細資料] 頁面](crud/_static/student-details.png)

![Student [建立] 頁面](crud/_static/student-create.png)

![Student [編輯] 頁面](crud/_static/student-edit.png)

![Student [刪除] 頁面](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a><span data-ttu-id="a1c70-116">自訂 [詳細資料] 頁面</span><span class="sxs-lookup"><span data-stu-id="a1c70-116">Customize the Details page</span></span>

<span data-ttu-id="a1c70-117">為 Students [索引] 頁面建立的 Scaffold 程式碼省略了 `Enrollments` 屬性，因為該屬性的值為一個集合。</span><span class="sxs-lookup"><span data-stu-id="a1c70-117">The scaffolded code for the Students Index page left out the `Enrollments` property, because that property holds a collection.</span></span> <span data-ttu-id="a1c70-118">在 [詳細資料] 頁面中，您會在一個 HTML 表格中顯示集合的內容。</span><span class="sxs-lookup"><span data-stu-id="a1c70-118">In the **Details** page, you'll display the contents of the collection in an HTML table.</span></span>

<span data-ttu-id="a1c70-119">在 *Controllers/StudentsController.cs* 中，[詳細資料] 檢視的動作方法會使用 `SingleOrDefaultAsync` 方法來擷取單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="a1c70-119">In *Controllers/StudentsController.cs*, the action method for the Details view uses the `SingleOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="a1c70-120">新增呼叫 `Include` 的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a1c70-120">Add code that calls `Include`.</span></span> <span data-ttu-id="a1c70-121">`ThenInclude`，以及 `AsNoTracking` 方法，如下列醒目提示的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="a1c70-121">`ThenInclude`,  and `AsNoTracking` methods, as shown in the following highlighted code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]

<span data-ttu-id="a1c70-122">`Include` 及 `ThenInclude` 方法會使內容載入 `Student.Enrollments` 導覽屬性，以及位於每一個註冊中的 `Enrollment.Course` 導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="a1c70-122">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span>  <span data-ttu-id="a1c70-123">您會在[讀取相關資料](read-related-data.md)教學課程中進一步了解這些方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-123">You'll learn more about these methods in the [read related data](read-related-data.md) tutorial.</span></span>

<span data-ttu-id="a1c70-124">`AsNoTracking` 方法在傳回實體不會在目前內容的存留期中更新的案例下可改善效能。</span><span class="sxs-lookup"><span data-stu-id="a1c70-124">The `AsNoTracking` method improves performance in scenarios where the entities returned won't be updated in the current context's lifetime.</span></span> <span data-ttu-id="a1c70-125">您會在本教學課程的最後進一步了解 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-125">You'll learn more about `AsNoTracking` at the end of this tutorial.</span></span>

### <a name="route-data"></a><span data-ttu-id="a1c70-126">路由資料</span><span class="sxs-lookup"><span data-stu-id="a1c70-126">Route data</span></span>

<span data-ttu-id="a1c70-127">傳遞至 `Details` 方法的索引鍵值是來自「路由資料」。</span><span class="sxs-lookup"><span data-stu-id="a1c70-127">The key value that's passed to the `Details` method comes from *route data*.</span></span> <span data-ttu-id="a1c70-128">路由資料是模型繫結器在 URL 區段中找到的資料。</span><span class="sxs-lookup"><span data-stu-id="a1c70-128">Route data is data that the model binder found in a segment of the URL.</span></span> <span data-ttu-id="a1c70-129">例如，預設路由指定了控制器、動作，以及識別碼區段：</span><span class="sxs-lookup"><span data-stu-id="a1c70-129">For example, the default route specifies controller, action, and id segments:</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]

<span data-ttu-id="a1c70-130">在下列 URL 中，預設路由會將講師 (Instructor) 對應為控制器，索引 (Index) 對應為動作，以及 1 對應為識別碼。這些是路由資料的值。</span><span class="sxs-lookup"><span data-stu-id="a1c70-130">In the following URL, the default route maps Instructor as the controller, Index as the action, and 1 as the id; these are route data values.</span></span>

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

<span data-ttu-id="a1c70-131">URL 最後的部分 ("?courseID=2021") 為查詢字串的值。</span><span class="sxs-lookup"><span data-stu-id="a1c70-131">The last part of the URL ("?courseID=2021") is a query string value.</span></span> <span data-ttu-id="a1c70-132">模型繫結器也會將識別碼的值作為 `id` 參數傳遞給 `Details` 方法 (若您將其作為查詢字串的值傳遞)：</span><span class="sxs-lookup"><span data-stu-id="a1c70-132">The model binder will also pass the ID value to the `Details` method `id` parameter if you pass it as a query string value:</span></span>

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

<span data-ttu-id="a1c70-133">在 [索引] 頁面中，Razor 檢視裡的標籤協助程式陳述式會建立超連結 URL。</span><span class="sxs-lookup"><span data-stu-id="a1c70-133">In the Index page, hyperlink URLs are created by tag helper statements in the Razor view.</span></span> <span data-ttu-id="a1c70-134">在下列 Razor 程式碼中，由於 `id` 參數符合預設路由，因此 `id` 便會新增至路由資料中。</span><span class="sxs-lookup"><span data-stu-id="a1c70-134">In the following Razor code, the `id` parameter matches the default route, so `id` is added to the route data.</span></span>

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

<span data-ttu-id="a1c70-135">這會在 `item.ID` 為 6 時產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="a1c70-135">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit/6">Edit</a>
```

<span data-ttu-id="a1c70-136">在下列 Razor 程式碼中，由於 `studentID` 不符合預設路由中的參數，因此將會作為查詢字串新增。</span><span class="sxs-lookup"><span data-stu-id="a1c70-136">In the following Razor code, `studentID` doesn't match a parameter in the default route, so it's added as a query string.</span></span>

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

<span data-ttu-id="a1c70-137">這會在 `item.ID` 為 6 時產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="a1c70-137">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

<span data-ttu-id="a1c70-138">如需標籤協助程式的詳細資訊，請參閱 [ASP.NET Core 中的標籤協助程式](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-138">For more information about tag helpers, see [Tag helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="add-enrollments-to-the-details-view"></a><span data-ttu-id="a1c70-139">將註冊新增至 [詳細資料] 檢視中</span><span class="sxs-lookup"><span data-stu-id="a1c70-139">Add enrollments to the Details view</span></span>

<span data-ttu-id="a1c70-140">開啟 *Views/Students/Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="a1c70-140">Open *Views/Students/Details.cshtml*.</span></span> <span data-ttu-id="a1c70-141">每個欄位都會使用 `DisplayNameFor` 及 `DisplayFor` 協助程式顯示，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a1c70-141">Each field is displayed using `DisplayNameFor` and `DisplayFor` helpers, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

<span data-ttu-id="a1c70-142">在最後一個欄位之後，並在結尾的 `</dl>` 標籤之前，新增下列程式碼以顯示註冊清單：</span><span class="sxs-lookup"><span data-stu-id="a1c70-142">After the last field and immediately before the closing `</dl>` tag, add the following code to display a list of enrollments:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

<span data-ttu-id="a1c70-143">若在您貼上程式碼之後，程式碼的縮排發生錯誤，請按 CTRL-K-D 修正它。</span><span class="sxs-lookup"><span data-stu-id="a1c70-143">If code indentation is wrong after you paste the code, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="a1c70-144">此程式碼會以迴圈逐一巡覽 `Enrollments` 導覽屬性中的實體。</span><span class="sxs-lookup"><span data-stu-id="a1c70-144">This code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="a1c70-145">針對每個註冊，它會顯示課程標題及成績。</span><span class="sxs-lookup"><span data-stu-id="a1c70-145">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="a1c70-146">課程標題會從儲存於 Enrollments 實體之 `Course` 導覽屬性中的課程 (Course) 實體擷取。</span><span class="sxs-lookup"><span data-stu-id="a1c70-146">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="a1c70-147">執行應用程式，選取 [Students] 索引標籤，然後按一下學生的 [詳細資料] 連結。</span><span class="sxs-lookup"><span data-stu-id="a1c70-147">Run the app, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="a1c70-148">您會看到選取學生的課程及成績清單：</span><span class="sxs-lookup"><span data-stu-id="a1c70-148">You see the list of courses and grades for the selected student:</span></span>

![Student [詳細資料] 頁面](crud/_static/student-details.png)

## <a name="update-the-create-page"></a><span data-ttu-id="a1c70-150">更新 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="a1c70-150">Update the Create page</span></span>

<span data-ttu-id="a1c70-151">在 *StudentsController.cs* 中，藉由新增 try-catch 區塊並從 `Bind` 屬性移除識別碼來修改 HttpPost `Create` 方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-151">In *StudentsController.cs*, modify the HttpPost `Create` method by adding a try-catch block and removing ID from the `Bind` attribute.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]

<span data-ttu-id="a1c70-152">這段程式碼會將 ASP.NET MVC 模型繫結器建立的 Student 實體新增至 Student 實體組，然後將變更儲存至資料庫。</span><span class="sxs-lookup"><span data-stu-id="a1c70-152">This code adds the Student entity created by the ASP.NET MVC model binder to the Students entity set and then saves the changes to the database.</span></span> <span data-ttu-id="a1c70-153">(模型繫結器指的是可讓您在操作由表單送出之資料上變得更為簡單的 ASP.NET MVC 功能。模型繫結器會將以 POST 方式送出之表單的值轉換為 CLR 類型，然後傳遞給參數中的動作方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-153">(Model binder refers to the ASP.NET MVC functionality that makes it easier for you to work with data submitted by a form; a model binder converts posted form values to CLR types and passes them to the action method in parameters.</span></span> <span data-ttu-id="a1c70-154">在此案例中，模型繫結器會使用來自表單 (Form) 集合的屬性值，為您執行個體化 Student 實體。)</span><span class="sxs-lookup"><span data-stu-id="a1c70-154">In this case, the model binder instantiates a Student entity for you using property values from the Form collection.)</span></span>

<span data-ttu-id="a1c70-155">您從 `Bind` 屬性移除了 `ID`，因為該識別碼是 SQL Server 在插入該資料列時自動為其建立的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="a1c70-155">You removed `ID` from the `Bind` attribute because ID is the primary key value which SQL Server will set automatically when the row is inserted.</span></span> <span data-ttu-id="a1c70-156">使用者輸入的內容不會設定識別碼值。</span><span class="sxs-lookup"><span data-stu-id="a1c70-156">Input from the user doesn't set the ID value.</span></span>

<span data-ttu-id="a1c70-157">除了 `Bind` 屬性外，try-catch 區塊是您對 Scaffold 程式碼進行的唯一變更。</span><span class="sxs-lookup"><span data-stu-id="a1c70-157">Other than the `Bind` attribute, the try-catch block is the only change you've made to the scaffolded code.</span></span> <span data-ttu-id="a1c70-158">若在儲存變更時捕捉到衍生自 `DbUpdateException` 的例外狀況，則會顯示一般錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a1c70-158">If an exception that derives from `DbUpdateException` is caught while the changes are being saved, a generic error message is displayed.</span></span> <span data-ttu-id="a1c70-159">`DbUpdateException` 例外狀況有時候是因為某些外部因素造成的，而非程式設計上的錯誤，因此系統會建議使用者再試一次。</span><span class="sxs-lookup"><span data-stu-id="a1c70-159">`DbUpdateException` exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again.</span></span> <span data-ttu-id="a1c70-160">雖然在此範例中並未實作，但生產環境品質的應用程式應記錄例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a1c70-160">Although not implemented in this sample, a production quality application would log the exception.</span></span> <span data-ttu-id="a1c70-161">如需詳細資訊，請參閱[監視及遙測 (使用 Azure 建置現實世界的雲端應用程式)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)中的**深入解析記錄檔**一節。</span><span class="sxs-lookup"><span data-stu-id="a1c70-161">For more information, see the **Log for insight** section in [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](https://docs.microsoft.com/aspnet/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).</span></span>

<span data-ttu-id="a1c70-162">`ValidateAntiForgeryToken` 屬性可協助防止跨網站偽造要求 (CSRF) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="a1c70-162">The `ValidateAntiForgeryToken` attribute helps prevent cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="a1c70-163">[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) 會自動將權杖插入檢視中，並在使用者提交表單時包含在內。</span><span class="sxs-lookup"><span data-stu-id="a1c70-163">The token is automatically injected into the view by the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) and is included when the form is submitted by the user.</span></span> <span data-ttu-id="a1c70-164">權杖會由 `ValidateAntiForgeryToken` 屬性進行驗證。</span><span class="sxs-lookup"><span data-stu-id="a1c70-164">The token is validated by the `ValidateAntiForgeryToken` attribute.</span></span> <span data-ttu-id="a1c70-165">如需 CSRF 的詳細資訊，請參閱[防偽要求](../../security/anti-request-forgery.md)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-165">For more information about CSRF, see [Anti-Request Forgery](../../security/anti-request-forgery.md).</span></span>

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a><span data-ttu-id="a1c70-166">關於大量指派 (overposting) 的安全性注意事項</span><span class="sxs-lookup"><span data-stu-id="a1c70-166">Security note about overposting</span></span>

<span data-ttu-id="a1c70-167">Scaffold 程式碼在 `Create` 方法上包含的 `Bind` 屬性是在建立案例中防止大量指派 (overposting) 的一種方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-167">The `Bind` attribute that the scaffolded code includes on the `Create` method is one way to protect against overposting in create scenarios.</span></span> <span data-ttu-id="a1c70-168">例如，假設 Student 實體包含了一個 `Secret` 屬性，而您不希望這個網頁設定這個屬性。</span><span class="sxs-lookup"><span data-stu-id="a1c70-168">For example, suppose the Student entity includes a `Secret` property that you don't want this web page to set.</span></span>

```csharp
public class Student
{
    public int ID { get; set; }
    public string LastName { get; set; }
    public string FirstMidName { get; set; }
    public DateTime EnrollmentDate { get; set; }
    public string Secret { get; set; }
}
```

<span data-ttu-id="a1c70-169">即使您在網頁上並未包含 `Secret` 欄位，駭客仍有可能會使用像是 Fiddler 等工具，或撰寫一些 JavaScript，來在表單中提交 `Secret` 值。</span><span class="sxs-lookup"><span data-stu-id="a1c70-169">Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="a1c70-170">若沒有 `Bind` 屬性限制模型繫結器在建立 Student 執行個體時能夠使用的欄位，則模型繫結器會揀選 `Secret` 表單值並使用它建立 Student 實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="a1c70-170">Without the `Bind` attribute limiting the fields that the model binder uses when it creates a Student instance, the model binder would pick up that `Secret` form value and use it to create the Student entity instance.</span></span> <span data-ttu-id="a1c70-171">則無論駭客在 `Secret` 表單欄位中指定了什麼值，該值都會更新到您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="a1c70-171">Then whatever value the hacker specified for the `Secret` form field would be updated in your database.</span></span> <span data-ttu-id="a1c70-172">下列影響顯示了 Fiddler 工具將 `Secret` 欄位 (其值為 "OverPost") 新增到表單的值中。</span><span class="sxs-lookup"><span data-stu-id="a1c70-172">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 新增 Secret 欄位](crud/_static/fiddler.png)

<span data-ttu-id="a1c70-174">"OverPost" 的值會成功新增到插入資料列的 `Secret` 屬性中，即使您沒有要讓網頁設定該屬性。</span><span class="sxs-lookup"><span data-stu-id="a1c70-174">The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to set that property.</span></span>

<span data-ttu-id="a1c70-175">您可以藉由先從資料庫讀取實體，然後呼叫 `TryUpdateModel`，明確傳遞允許的屬性清單，來在編輯案例中防止大量指派。</span><span class="sxs-lookup"><span data-stu-id="a1c70-175">You can prevent overposting in edit scenarios by reading the entity from the database first and then calling `TryUpdateModel`, passing in an explicit allowed properties list.</span></span> <span data-ttu-id="a1c70-176">這便是這些教學課程所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-176">That's the method used in these tutorials.</span></span>

<span data-ttu-id="a1c70-177">另一個許多開發人員偏好的防止大量指派的方法，便是使用檢視模型搭配模型繫結，而非實體類別。</span><span class="sxs-lookup"><span data-stu-id="a1c70-177">An alternative way to prevent overposting that's preferred by many developers is to use view models rather than entity classes with model binding.</span></span> <span data-ttu-id="a1c70-178">意即僅在檢視模型中包含您想要更新的屬性。</span><span class="sxs-lookup"><span data-stu-id="a1c70-178">Include only the properties you want to update in the view model.</span></span> <span data-ttu-id="a1c70-179">待 MVC 模型繫結器完成後，將檢視模型屬性複製到實體執行個體，並選擇性的使用像是 AutoMapper 等工具。</span><span class="sxs-lookup"><span data-stu-id="a1c70-179">Once the MVC model binder has finished, copy the view model properties to the entity instance, optionally using a tool such as AutoMapper.</span></span> <span data-ttu-id="a1c70-180">在實體執行個體上使用 `_context.Entry` 來將其狀態設定為 `Unchanged`，然後在每個包含在檢視模型中的實體屬性上將 `Property("PropertyName").IsModified` 設定為 true。</span><span class="sxs-lookup"><span data-stu-id="a1c70-180">Use `_context.Entry` on the entity instance to set its state to `Unchanged`, and then set `Property("PropertyName").IsModified` to true on each entity property that's included in the view model.</span></span> <span data-ttu-id="a1c70-181">這個方法可同時運用在編輯及建立案例中。</span><span class="sxs-lookup"><span data-stu-id="a1c70-181">This method works in both edit and create scenarios.</span></span>

### <a name="test-the-create-page"></a><span data-ttu-id="a1c70-182">測試 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="a1c70-182">Test the Create page</span></span>

<span data-ttu-id="a1c70-183">在 *Views/Students/Create.cshtml* 中的程式碼會針對各個欄位使用 `label`、`input`，以及 `span` (以驗證訊息) 標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="a1c70-183">The code in *Views/Students/Create.cshtml* uses `label`, `input`, and `span` (for validation messages) tag helpers for each field.</span></span>

<span data-ttu-id="a1c70-184">執行應用程式，選取 [Students] 索引標籤，然後按一下 [新建]。</span><span class="sxs-lookup"><span data-stu-id="a1c70-184">Run the app, select the **Students** tab, and click **Create New**.</span></span>

<span data-ttu-id="a1c70-185">輸入名稱和日期。</span><span class="sxs-lookup"><span data-stu-id="a1c70-185">Enter names and a date.</span></span> <span data-ttu-id="a1c70-186">嘗試輸入無效的日期 (若您的瀏覽器允許的話)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-186">Try entering an invalid date if your browser lets you do that.</span></span> <span data-ttu-id="a1c70-187">(某些瀏覽器會強制要求您使用日期選擇器。)然後按一下 [建立] 以查看錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a1c70-187">(Some browsers force you to use a date picker.) Then click **Create** to see the error message.</span></span>

![日期驗證錯誤](crud/_static/date-error.png)

<span data-ttu-id="a1c70-189">這是根據預設您會取得的伺服器端驗證。在稍後的教學課程中，您也會了解如何新增為用戶端驗證產生程式碼的屬性。</span><span class="sxs-lookup"><span data-stu-id="a1c70-189">This is server-side validation that you get by default; in a later tutorial you'll see how to add attributes that will generate code for client-side validation also.</span></span> <span data-ttu-id="a1c70-190">下列醒目提示的程式碼顯示了在 `Create` 方法中進行的模型驗證檢查。</span><span class="sxs-lookup"><span data-stu-id="a1c70-190">The following highlighted code shows the model validation check in the `Create` method.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]

<span data-ttu-id="a1c70-191">將日期變更為有效的值，然後按一下 [建立] 來在 [索引] 頁面上查看新增的學生。</span><span class="sxs-lookup"><span data-stu-id="a1c70-191">Change the date to a valid value and click **Create** to see the new student appear in the **Index** page.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="a1c70-192">更新 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="a1c70-192">Update the Edit page</span></span>

<span data-ttu-id="a1c70-193">在 *StudentController.cs* 中，HttpGet `Edit` 方法 (沒有 `HttpPost` 屬性的方法) 會使用 `SingleOrDefaultAsync` 方法來擷取選取的 Student 實體，如同您在 `Details` 方法中所看到的。</span><span class="sxs-lookup"><span data-stu-id="a1c70-193">In *StudentController.cs*, the HttpGet `Edit` method (the one without the `HttpPost` attribute) uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the `Details` method.</span></span> <span data-ttu-id="a1c70-194">您不需要變更這個方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-194">You don't need to change this method.</span></span>

### <a name="recommended-httppost-edit-code-read-and-update"></a><span data-ttu-id="a1c70-195">建議的 HttpPost Edit 程式碼：讀取及更新</span><span class="sxs-lookup"><span data-stu-id="a1c70-195">Recommended HttpPost Edit code: Read and update</span></span>

<span data-ttu-id="a1c70-196">將 HttpPost Edit 動作方法以下列程式碼取代。</span><span class="sxs-lookup"><span data-stu-id="a1c70-196">Replace the HttpPost Edit action method with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]

<span data-ttu-id="a1c70-197">這些變更會實作安全性最佳做法來防止大量指派。</span><span class="sxs-lookup"><span data-stu-id="a1c70-197">These changes implement a security best practice to prevent overposting.</span></span> <span data-ttu-id="a1c70-198">框架會產生一個 `Bind` 屬性，並為模型繫結器建立的實體加上 `Modified` 旗標，新增到實體組。</span><span class="sxs-lookup"><span data-stu-id="a1c70-198">The scaffolder generated a `Bind` attribute and added the entity created by the model binder to the entity set with a `Modified` flag.</span></span> <span data-ttu-id="a1c70-199">該程式碼不建議用於許多案例，因為 `Bind` 屬性會清空 `Include` 參數未包含的欄位中任何預先存在的資料。</span><span class="sxs-lookup"><span data-stu-id="a1c70-199">That code isn't recommended for many scenarios because the `Bind` attribute clears out any pre-existing data in fields not listed in the `Include` parameter.</span></span>

<span data-ttu-id="a1c70-200">新的程式碼會讀取現有的實體，呼叫 `TryUpdateModel` 來[根據使用者在 Post 表單資料中輸入的內容](xref:mvc/models/model-binding#how-model-binding-works)來更新已擷取實體中的欄位。</span><span class="sxs-lookup"><span data-stu-id="a1c70-200">The new code reads the existing entity and calls `TryUpdateModel` to update fields in the retrieved entity [based on user input in the posted form data](xref:mvc/models/model-binding#how-model-binding-works).</span></span> <span data-ttu-id="a1c70-201">Entity Framework 的自動變更追蹤會在表單輸入變更的欄位上設定 `Modified` 旗標。</span><span class="sxs-lookup"><span data-stu-id="a1c70-201">The Entity Framework's automatic change tracking sets the `Modified` flag on the fields that are changed by form input.</span></span> <span data-ttu-id="a1c70-202">當呼叫 `SaveChanges` 方法時，Entity Framework 會建立 SQL 陳述式來更新資料庫的資料列。</span><span class="sxs-lookup"><span data-stu-id="a1c70-202">When the `SaveChanges` method is called, the Entity Framework creates SQL statements to update the database row.</span></span> <span data-ttu-id="a1c70-203">系統會忽略並行衝突，並且只有使用者更新的資料表資料行會在資料庫中獲得更新。</span><span class="sxs-lookup"><span data-stu-id="a1c70-203">Concurrency conflicts are ignored, and only the table columns that were updated by the user are updated in the database.</span></span> <span data-ttu-id="a1c70-204">(之後的教學課程會顯示如何處理並行衝突。)</span><span class="sxs-lookup"><span data-stu-id="a1c70-204">(A later tutorial shows how to handle concurrency conflicts.)</span></span>

<span data-ttu-id="a1c70-205">作為防止大量指派的最佳做法，您要允許 [編輯] 頁面更新的欄位會在 `TryUpdateModel` 參數的允許清單中。</span><span class="sxs-lookup"><span data-stu-id="a1c70-205">As a best practice to prevent overposting, the fields that you want to be updateable by the **Edit** page are whitelisted in the `TryUpdateModel` parameters.</span></span> <span data-ttu-id="a1c70-206">(參數清單中欄位清單前的空字串是針對與表單欄位名稱一同使用的前置詞。)雖然目前沒有額外保護的欄位，但列出您希望模型繫結器繫結的欄位可確保您於未來將欄位新增到資料模型中時，新增的欄位會自動獲得保護，直到您明確的在這裡新增它們為止。</span><span class="sxs-lookup"><span data-stu-id="a1c70-206">(The empty string preceding the list of fields in the parameter list is for a prefix to use with the form fields names.) Currently there are no extra fields that you're protecting, but listing the fields that you want the model binder to bind ensures that if you add fields to the data model in the future, they're automatically protected until you explicitly add them here.</span></span>

<span data-ttu-id="a1c70-207">作為這些變更的結果，HttpPost `Edit` 方法的方法簽章會與 HttpGet `Edit` 方法的簽章一樣；因此，您已將方法重新命名為 `EditPost`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-207">As a result of these changes, the method signature of the HttpPost `Edit` method is the same as the HttpGet `Edit` method; therefore you've renamed the method `EditPost`.</span></span>

### <a name="alternative-httppost-edit-code-create-and-attach"></a><span data-ttu-id="a1c70-208">HttpPost Edit 程式碼的替代方案：建立及連結</span><span class="sxs-lookup"><span data-stu-id="a1c70-208">Alternative HttpPost Edit code: Create and attach</span></span>

<span data-ttu-id="a1c70-209">建議的 HttpPost Edit 程式碼可確保只有受到變更的資料行會獲得更新，並且會保留您不想要在資料繫結中包含之屬性內的資料。</span><span class="sxs-lookup"><span data-stu-id="a1c70-209">The recommended HttpPost edit code ensures that only changed columns get updated and preserves data in properties that you don't want included for model binding.</span></span> <span data-ttu-id="a1c70-210">然而，讀取優先的方法會需要額外的資料庫讀取，因此可能導致需要更多複雜的程式碼來處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="a1c70-210">However, the read-first approach requires an extra database read, and can result in more complex code for handling concurrency conflicts.</span></span> <span data-ttu-id="a1c70-211">其替代方案便是將模型繫結器建立的實體連結到 EF 內容，並將其標示為已修改。</span><span class="sxs-lookup"><span data-stu-id="a1c70-211">An alternative is to attach an entity created by the model binder to the EF context and mark it as modified.</span></span> <span data-ttu-id="a1c70-212">(請不要使用此程式碼更新您的專案。此程式碼僅作為展示選擇性的方法之用。)</span><span class="sxs-lookup"><span data-stu-id="a1c70-212">(Don't update your project with this code, it's only shown to illustrate an optional approach.)</span></span> 

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]

<span data-ttu-id="a1c70-213">當網頁 UI 包含了所有實體中的欄位，並且可以更新其中的任何一個欄位時，您可以使用此方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-213">You can use this approach when the web page UI includes all of the fields in the entity and can update any of them.</span></span>

<span data-ttu-id="a1c70-214">Scaffold 程式碼會使用「建立及連結 」方法，但僅會捕捉到 `DbUpdateConcurrencyException` 例外狀況並傳回 404 錯誤代碼。</span><span class="sxs-lookup"><span data-stu-id="a1c70-214">The scaffolded code uses the create-and-attach approach but only catches `DbUpdateConcurrencyException` exceptions and returns 404 error codes.</span></span>  <span data-ttu-id="a1c70-215">此處顯示的範例會捕捉到任何資料庫更新例外狀況，並顯示一個錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a1c70-215">The example shown catches any database update exception and displays an error message.</span></span>

### <a name="entity-states"></a><span data-ttu-id="a1c70-216">實體狀態</span><span class="sxs-lookup"><span data-stu-id="a1c70-216">Entity States</span></span>

<span data-ttu-id="a1c70-217">資料庫內容會追蹤實體在記憶體中是否與其在資料庫中相對應的資料列保持同步，並且此項資訊會決定當您呼叫 `SaveChanges` 方法時會發生什麼事情。</span><span class="sxs-lookup"><span data-stu-id="a1c70-217">The database context keeps track of whether entities in memory are in sync with their corresponding rows in the database, and this information determines what happens when you call the `SaveChanges` method.</span></span> <span data-ttu-id="a1c70-218">例如，當您傳遞一個新的實體到 `Add` 方法時，該實體的狀態便會設定為 `Added`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-218">For example, when you pass a new entity to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="a1c70-219">然後當您呼叫 `SaveChanges` 方法時，資料庫內容便會發出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="a1c70-219">Then when you call the `SaveChanges` method, the database context issues a SQL INSERT command.</span></span>

<span data-ttu-id="a1c70-220">實體可為下列狀態中的其中一個：</span><span class="sxs-lookup"><span data-stu-id="a1c70-220">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="a1c70-221">`Added`.</span><span class="sxs-lookup"><span data-stu-id="a1c70-221">`Added`.</span></span> <span data-ttu-id="a1c70-222">實體尚未存在於資料庫中。</span><span class="sxs-lookup"><span data-stu-id="a1c70-222">The entity doesn't yet exist in the database.</span></span> <span data-ttu-id="a1c70-223">`SaveChanges` 方法會發出 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a1c70-223">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="a1c70-224">`Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="a1c70-224">`Unchanged`.</span></span> <span data-ttu-id="a1c70-225">`SaveChanges` 方法針對這個實體不需要進行任何動作。</span><span class="sxs-lookup"><span data-stu-id="a1c70-225">Nothing needs to be done with this entity by the `SaveChanges` method.</span></span> <span data-ttu-id="a1c70-226">當您從資料庫讀取一個實體時，實體便會以此狀態開始。</span><span class="sxs-lookup"><span data-stu-id="a1c70-226">When you read an entity from the database, the entity starts out with this status.</span></span>

* <span data-ttu-id="a1c70-227">`Modified`.</span><span class="sxs-lookup"><span data-stu-id="a1c70-227">`Modified`.</span></span> <span data-ttu-id="a1c70-228">實體中一部分或全部的屬性值已經過修改。</span><span class="sxs-lookup"><span data-stu-id="a1c70-228">Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="a1c70-229">`SaveChanges` 方法會發出 UPDATE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a1c70-229">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="a1c70-230">`Deleted`.</span><span class="sxs-lookup"><span data-stu-id="a1c70-230">`Deleted`.</span></span> <span data-ttu-id="a1c70-231">實體已遭標示刪除。</span><span class="sxs-lookup"><span data-stu-id="a1c70-231">The entity has been marked for deletion.</span></span> <span data-ttu-id="a1c70-232">`SaveChanges` 方法會發出 DELETE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a1c70-232">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="a1c70-233">`Detached`.</span><span class="sxs-lookup"><span data-stu-id="a1c70-233">`Detached`.</span></span> <span data-ttu-id="a1c70-234">實體未獲得資料庫內容追蹤。</span><span class="sxs-lookup"><span data-stu-id="a1c70-234">The entity isn't being tracked by the database context.</span></span>

<span data-ttu-id="a1c70-235">在桌面應用程式中，狀態變更通常會自動進行設定。</span><span class="sxs-lookup"><span data-stu-id="a1c70-235">In a desktop application, state changes are typically set automatically.</span></span> <span data-ttu-id="a1c70-236">您讀取了一個實體並對其中的一些屬性值進行變更。</span><span class="sxs-lookup"><span data-stu-id="a1c70-236">You read an entity and make changes to some of its property values.</span></span> <span data-ttu-id="a1c70-237">這會使得其實體狀態自動變更為 `Modified`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-237">This causes its entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="a1c70-238">當您呼叫 `SaveChanges` 時，Entity Framework 會產生 SQL UPDATE 陳述式，並且只會更新您實際變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="a1c70-238">Then when you call `SaveChanges`, the Entity Framework generates a SQL UPDATE statement that updates only the actual properties that you changed.</span></span>

<span data-ttu-id="a1c70-239">在 Web 應用程式中，初始讀取實體並顯示其資料以供編輯之用的 `DbContext` 會在頁面轉譯之後遭到處置。</span><span class="sxs-lookup"><span data-stu-id="a1c70-239">In a web app, the `DbContext` that initially reads an entity and displays its data to be edited is disposed after a page is rendered.</span></span> <span data-ttu-id="a1c70-240">當呼叫 HttpPost `Edit` 動作方法時，會發出新的 Web 要求，並且您便會擁有一個新的 `DbContext` 執行個體。</span><span class="sxs-lookup"><span data-stu-id="a1c70-240">When the HttpPost `Edit` action method is called,  a new web request is made and you have a new instance of the `DbContext`.</span></span> <span data-ttu-id="a1c70-241">當您在新的內容中重新讀取時，您便會模擬桌面處理流程。</span><span class="sxs-lookup"><span data-stu-id="a1c70-241">If you re-read the entity in that new context, you simulate desktop processing.</span></span>

<span data-ttu-id="a1c70-242">但若您不想要進行額外的讀取作業，則您必須使用由模型繫結器建立的實體物件。</span><span class="sxs-lookup"><span data-stu-id="a1c70-242">But if you don't want to do the extra read operation, you have to use the entity object created by the model binder.</span></span>  <span data-ttu-id="a1c70-243">完成這項作業最簡單的方式，便是將實體狀態設定為「已修改」(Modified)，作為先前顯示之 HttpPost Edit 程式碼的替代方案。</span><span class="sxs-lookup"><span data-stu-id="a1c70-243">The simplest way to do this is to set the entity state to Modified as is done in the alternative HttpPost Edit code shown earlier.</span></span> <span data-ttu-id="a1c70-244">則當您呼叫 `SaveChanges` 時，Entity Framework 會更新資料庫資料列中所有的資料行，因為內容無法得知您變更的屬性為何。</span><span class="sxs-lookup"><span data-stu-id="a1c70-244">Then when you call `SaveChanges`, the Entity Framework updates all columns of the database row, because the context has no way to know which properties you changed.</span></span>

<span data-ttu-id="a1c70-245">若您想要避免讀取優先的方法，但又想要 SQL UPDATE 陳述式僅更新使用者實際變更的欄位，則程式碼會變得更為複雜。</span><span class="sxs-lookup"><span data-stu-id="a1c70-245">If you want to avoid the read-first approach, but you also want the SQL UPDATE statement to update only the fields that the user actually changed, the code is more complex.</span></span> <span data-ttu-id="a1c70-246">您必須先以某種方式儲存原始的值 (例如使用隱藏欄位)，使得在呼叫 HttpPost `Edit` 方法時仍可以使用那些值。</span><span class="sxs-lookup"><span data-stu-id="a1c70-246">You have to save the original values in some way (such as by using hidden fields) so that they're available when the HttpPost `Edit` method is called.</span></span> <span data-ttu-id="a1c70-247">然後您便可以使用原始的值建立 Student 實體，使用原始版本的實體呼叫 `Attach` 方法，將實體的值更新為新的值，然後呼叫 `SaveChanges`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-247">Then you can create a Student entity using the original values, call the `Attach` method with that original version of the entity, update the entity's values to the new values, and then call `SaveChanges`.</span></span>

### <a name="test-the-edit-page"></a><span data-ttu-id="a1c70-248">測試 [編輯] 頁面</span><span class="sxs-lookup"><span data-stu-id="a1c70-248">Test the Edit page</span></span>

<span data-ttu-id="a1c70-249">執行應用程式，選取 [Students] 索引標籤，然後按一下 [編輯] 超連結。</span><span class="sxs-lookup"><span data-stu-id="a1c70-249">Run the app, select the **Students** tab, then click an **Edit** hyperlink.</span></span>

![Students [編輯] 頁面](crud/_static/student-edit.png)

<span data-ttu-id="a1c70-251">變更一部分的資料，然後按一下 [儲存]。</span><span class="sxs-lookup"><span data-stu-id="a1c70-251">Change some of the data and click **Save**.</span></span> <span data-ttu-id="a1c70-252">[索引] 頁面便會開啟，而您會看到更新的資料。</span><span class="sxs-lookup"><span data-stu-id="a1c70-252">The **Index** page opens and you see the changed data.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="a1c70-253">更新 [刪除] 頁面</span><span class="sxs-lookup"><span data-stu-id="a1c70-253">Update the Delete page</span></span>

<span data-ttu-id="a1c70-254">在 *StudentController.cs* 中，HttpGet `Delete` 方法的範本程式碼使用了 `SingleOrDefaultAsync` 方法來擷取選取的 Student 實體，如您在 Details 及 Edit 方法中所看到的。</span><span class="sxs-lookup"><span data-stu-id="a1c70-254">In *StudentController.cs*, the template code for the HttpGet `Delete` method uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the Details and Edit methods.</span></span> <span data-ttu-id="a1c70-255">然而，若要在呼叫 `SaveChanges` 失敗時實作自訂錯誤訊息，您需要將一些功能新增至此方法及其對應的檢視。</span><span class="sxs-lookup"><span data-stu-id="a1c70-255">However, to implement a custom error message when the call to `SaveChanges` fails, you'll add some functionality to this method and its corresponding view.</span></span>

<span data-ttu-id="a1c70-256">如同您在更新及建立作業中所看到的，刪除作業需要兩個動作方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-256">As you saw for update and create operations, delete operations require two action methods.</span></span> <span data-ttu-id="a1c70-257">為回應 GET 要求而呼叫的方法會顯示一個檢視，給予使用者機會核准或取消刪除作業。</span><span class="sxs-lookup"><span data-stu-id="a1c70-257">The method that's called in response to a GET request displays a view that gives the user a chance to approve or cancel the delete operation.</span></span> <span data-ttu-id="a1c70-258">若使用者核准，則便會建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="a1c70-258">If the user approves it, a POST request is created.</span></span> <span data-ttu-id="a1c70-259">當這種情況發生時，便會呼叫 HttpPost `Delete` 方法，然後該方法便會實際執行刪除作業。</span><span class="sxs-lookup"><span data-stu-id="a1c70-259">When that happens, the HttpPost `Delete` method is called and then that method actually performs the delete operation.</span></span>

<span data-ttu-id="a1c70-260">您會將一個 try-catch 區塊新增到 HttpPost `Delete` 方法，來處理任何可能在資料庫更新時可能發生的錯誤。</span><span class="sxs-lookup"><span data-stu-id="a1c70-260">You'll add a try-catch block to the HttpPost `Delete` method to handle any errors that might occur when the database is updated.</span></span> <span data-ttu-id="a1c70-261">若發生錯誤，則 HttpPost Delete 方法會呼叫 HttpGet Delete 方法，將一個指示發生錯誤的參數傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="a1c70-261">If an error occurs, the HttpPost Delete method calls the HttpGet Delete method, passing it a parameter that indicates that an error has occurred.</span></span> <span data-ttu-id="a1c70-262">HttpGet Delete 方法接著便會重新顯示確認頁面及錯誤訊息，給予使用者機會取消或再試一次。</span><span class="sxs-lookup"><span data-stu-id="a1c70-262">The HttpGet Delete method then redisplays the confirmation page along with the error message, giving the user an opportunity to cancel or try again.</span></span>

<span data-ttu-id="a1c70-263">請以下列程式碼取代 HttpGet `Delete` 動作方法。這段程式碼會管理錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="a1c70-263">Replace the HttpGet `Delete` action method with the following code, which manages error reporting.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]

<span data-ttu-id="a1c70-264">此程式碼會接受一個選用的參數，該參數會在儲存變更發生錯誤之後指示方法是否有進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="a1c70-264">This code accepts an optional parameter that indicates whether the method was called after a failure to save changes.</span></span> <span data-ttu-id="a1c70-265">當呼叫 HttpGet `Delete` 方法時，若先前沒有發生錯誤，則此參數將為 false。</span><span class="sxs-lookup"><span data-stu-id="a1c70-265">This parameter is false when the HttpGet `Delete` method is called without a previous failure.</span></span> <span data-ttu-id="a1c70-266">當此方法是由 HttpPost `Delete` 方法為回應資料庫更新錯誤而呼叫時，則參數將為 true，且錯誤訊息將會傳遞給檢視。</span><span class="sxs-lookup"><span data-stu-id="a1c70-266">When it's called by the HttpPost `Delete` method in response to a database update error, the parameter is true and an error message is passed to the view.</span></span>

### <a name="the-read-first-approach-to-httppost-delete"></a><span data-ttu-id="a1c70-267">HttpPost Delete 讀取優先方法</span><span class="sxs-lookup"><span data-stu-id="a1c70-267">The read-first approach to HttpPost Delete</span></span>

<span data-ttu-id="a1c70-268">請以下列程式碼取代 HttpPost `Delete` 動作方法 (名為 `DeleteConfirmed`)。此程式碼會執行實際的刪除作業並捕捉任何資料庫更新錯誤。</span><span class="sxs-lookup"><span data-stu-id="a1c70-268">Replace the HttpPost `Delete` action method (named `DeleteConfirmed`) with the following code, which performs the actual delete operation and catches any database update errors.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]

<span data-ttu-id="a1c70-269">此程式碼會擷取選取的實體，然後呼叫 `Remove` 方法來將實體的狀態設定為 `Deleted`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-269">This code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="a1c70-270">當呼叫 `SaveChanges` 時，便會產生 SQL DELETE 命令。</span><span class="sxs-lookup"><span data-stu-id="a1c70-270">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span>

### <a name="the-create-and-attach-approach-to-httppost-delete"></a><span data-ttu-id="a1c70-271">HttpPost Delete 的「建立及連結 」方法</span><span class="sxs-lookup"><span data-stu-id="a1c70-271">The create-and-attach approach to HttpPost Delete</span></span>

<span data-ttu-id="a1c70-272">若在一個高容量的應用程式中改善效能是您的優先事項，您可以透過僅使用主索引鍵值執行個體化 Student 實體，然後將實體狀態設定為 `Deleted`，來避免不必要的 SQL 查詢。</span><span class="sxs-lookup"><span data-stu-id="a1c70-272">If improving performance in a high-volume application is a priority, you could avoid an unnecessary SQL query by instantiating a Student entity using only the primary key value and then setting the entity state to `Deleted`.</span></span> <span data-ttu-id="a1c70-273">這便是 Entity Framework 要刪除實體所需要的一切資訊。</span><span class="sxs-lookup"><span data-stu-id="a1c70-273">That's all that the Entity Framework needs in order to delete the entity.</span></span> <span data-ttu-id="a1c70-274">(請不要將此程式碼放在您的專案中。此程式碼僅作為展示替代方案之用。)</span><span class="sxs-lookup"><span data-stu-id="a1c70-274">(Don't put this code in your project; it's here just to illustrate an alternative.)</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]

<span data-ttu-id="a1c70-275">若實體有也需要刪除的相關資料，請確認您已在資料庫中設定串聯刪除。</span><span class="sxs-lookup"><span data-stu-id="a1c70-275">If the entity has related data that should also be deleted, make sure that cascade delete is configured in the database.</span></span> <span data-ttu-id="a1c70-276">若使用此方法進行實體刪除，EF 可能無法得知有相關實體需要進行刪除。</span><span class="sxs-lookup"><span data-stu-id="a1c70-276">With this approach to entity deletion, EF might not realize there are related entities to be deleted.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="a1c70-277">更新 [刪除] 檢視</span><span class="sxs-lookup"><span data-stu-id="a1c70-277">Update the Delete view</span></span>

<span data-ttu-id="a1c70-278">在 *Views/Student/Delete.cshtml* 中，在 h2 標題和 h3 標題之間新增一個錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a1c70-278">In *Views/Student/Delete.cshtml*, add an error message between the h2 heading and the h3 heading, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

<span data-ttu-id="a1c70-279">執行應用程式，選取 [Students] 索引標籤，然後按一下**刪除**超連結：</span><span class="sxs-lookup"><span data-stu-id="a1c70-279">Run the app, select the **Students** tab, and click a **Delete** hyperlink:</span></span>

![刪除確認頁面](crud/_static/student-delete.png)

<span data-ttu-id="a1c70-281">按一下 [刪除]。</span><span class="sxs-lookup"><span data-stu-id="a1c70-281">Click **Delete**.</span></span> <span data-ttu-id="a1c70-282">顯示的 [索引] 頁面將不會包含遭刪除的學生。</span><span class="sxs-lookup"><span data-stu-id="a1c70-282">The Index page is displayed without the deleted student.</span></span> <span data-ttu-id="a1c70-283">(您會在並行教學課程中看到錯誤處理程式碼範例的實際情況。)</span><span class="sxs-lookup"><span data-stu-id="a1c70-283">(You'll see an example of the error handling code in action in the concurrency tutorial.)</span></span>

## <a name="closing-database-connections"></a><span data-ttu-id="a1c70-284">關閉資料庫連線</span><span class="sxs-lookup"><span data-stu-id="a1c70-284">Closing database connections</span></span>

<span data-ttu-id="a1c70-285">若要釋放資料庫連線保留的資源，您必須在完成作業並不再需要內容執行個體時盡快處置它。</span><span class="sxs-lookup"><span data-stu-id="a1c70-285">To free up the resources that a database connection holds, the context instance must be disposed as soon as possible when you are done with it.</span></span> <span data-ttu-id="a1c70-286">ASP.NET Core 內建的[相依性插入](../../fundamentals/dependency-injection.md)會為您完成這項工作。</span><span class="sxs-lookup"><span data-stu-id="a1c70-286">The ASP.NET Core built-in [dependency injection](../../fundamentals/dependency-injection.md) takes care of that task for you.</span></span>

<span data-ttu-id="a1c70-287">在 *Startup.cs* 中，您會呼叫 [AddDbContext 擴充方法](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) 來在 ASP.NET DI 容器中佈建 `DbContext` 類別。</span><span class="sxs-lookup"><span data-stu-id="a1c70-287">In *Startup.cs*, you call the [AddDbContext extension method](https://github.com/aspnet/EntityFrameworkCore/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) to provision the `DbContext` class in the ASP.NET DI container.</span></span> <span data-ttu-id="a1c70-288">根據預設，該方法會將服務存留期設定為 `Scoped`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-288">That method sets the service lifetime to `Scoped` by default.</span></span> <span data-ttu-id="a1c70-289">`Scoped` 表示內容物件的存留期會與 Web 要求的存留期保持一致，並且在 Web 要求結束時會自動呼叫 `Dispose` 方法。</span><span class="sxs-lookup"><span data-stu-id="a1c70-289">`Scoped` means the context object lifetime coincides with the web request life time, and the `Dispose` method will be called automatically at the end of the web request.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="a1c70-290">處理交易</span><span class="sxs-lookup"><span data-stu-id="a1c70-290">Handling Transactions</span></span>

<span data-ttu-id="a1c70-291">根據預設，Entity Framework 隱含性的實作了交易。</span><span class="sxs-lookup"><span data-stu-id="a1c70-291">By default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="a1c70-292">在您對多個資料列或資料表進行變更，然後呼叫 `SaveChanges` 的案例下，Entity Framework 會自動確認您所有的變更都已成功或是失敗。</span><span class="sxs-lookup"><span data-stu-id="a1c70-292">In scenarios where you make changes to multiple rows or tables and then call `SaveChanges`, the Entity Framework automatically makes sure that either all of your changes succeed or they all fail.</span></span> <span data-ttu-id="a1c70-293">若有些變更已先完成，之後卻發生錯誤，則這些變更都會自動進行復原。</span><span class="sxs-lookup"><span data-stu-id="a1c70-293">If some changes are done first and then an error happens, those changes are automatically rolled back.</span></span> <span data-ttu-id="a1c70-294">針對您需要更多控制的案例 -- 例如，若您想要在一個交易中包含在 Entity Framework 之外完成的作業 -- 請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-294">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="a1c70-295">無追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="a1c70-295">No-tracking queries</span></span>

<span data-ttu-id="a1c70-296">當資料庫內容擷取資料表資料列並建立代表他們的實體物件時，根據預設，它會追蹤在記憶體中的實體是否與資料庫中的內容保持同步。</span><span class="sxs-lookup"><span data-stu-id="a1c70-296">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="a1c70-297">記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。</span><span class="sxs-lookup"><span data-stu-id="a1c70-297">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="a1c70-298">這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。</span><span class="sxs-lookup"><span data-stu-id="a1c70-298">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="a1c70-299">您可以藉由呼叫 `AsNoTracking` 方法來停用追蹤記憶體中的實體物件。</span><span class="sxs-lookup"><span data-stu-id="a1c70-299">You can disable tracking of entity objects in memory by calling the `AsNoTracking` method.</span></span> <span data-ttu-id="a1c70-300">您會想要進行這項操作的常見案例包括下列情況：</span><span class="sxs-lookup"><span data-stu-id="a1c70-300">Typical scenarios in which you might want to do that include the following:</span></span>

* <span data-ttu-id="a1c70-301">在內容的存留期間，您不需要更新任何實體，並且您也不需要 EF [使用透過個別查詢擷取的實體自動載入導覽屬性](read-related-data.md)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-301">During the context lifetime you don't need to update any entities, and you don't need EF to [automatically load navigation properties with  entities retrieved by separate queries](read-related-data.md).</span></span> <span data-ttu-id="a1c70-302">控制器中的 HttpGet 動作方法常常會符合這些條件。</span><span class="sxs-lookup"><span data-stu-id="a1c70-302">Frequently these conditions are met in a controller's HttpGet action methods.</span></span>

* <span data-ttu-id="a1c70-303">您正在執行一個擷取大量資料的查詢，但是傳回資料之中只有一小部分需要更新。</span><span class="sxs-lookup"><span data-stu-id="a1c70-303">You are running a query that retrieves a large volume of data, and only a small portion of the returned data will be updated.</span></span> <span data-ttu-id="a1c70-304">在這種情況下，為大型查詢關閉追蹤，然後在稍後為需要更新的幾個實體執行查詢可能會比較有效率。</span><span class="sxs-lookup"><span data-stu-id="a1c70-304">It may be more efficient to turn off tracking for the large query, and run a query later for the few entities that need to be updated.</span></span>

* <span data-ttu-id="a1c70-305">您想要連結到一個實體以更新它，但稍早之前您才為了不同的目的擷取過相同的實體。</span><span class="sxs-lookup"><span data-stu-id="a1c70-305">You want to attach an entity in order to update it, but earlier you retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="a1c70-306">由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。</span><span class="sxs-lookup"><span data-stu-id="a1c70-306">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="a1c70-307">處理這種情況的一種方法，便是在稍早之前的查詢中呼叫 `AsNoTracking`。</span><span class="sxs-lookup"><span data-stu-id="a1c70-307">One way to handle this situation is to call `AsNoTracking` on the earlier query.</span></span>

<span data-ttu-id="a1c70-308">如需詳細資訊，請參閱[追蹤與不追蹤](https://docs.microsoft.com/ef/core/querying/tracking)。</span><span class="sxs-lookup"><span data-stu-id="a1c70-308">For more information, see [Tracking vs. No-Tracking](https://docs.microsoft.com/ef/core/querying/tracking).</span></span>

## <a name="summary"></a><span data-ttu-id="a1c70-309">總結</span><span class="sxs-lookup"><span data-stu-id="a1c70-309">Summary</span></span>

<span data-ttu-id="a1c70-310">您現在已有完整的頁面組，可為 Student 實體進行簡易的 CRUD 作業。</span><span class="sxs-lookup"><span data-stu-id="a1c70-310">You now have a complete set of pages that perform simple CRUD operations for Student entities.</span></span> <span data-ttu-id="a1c70-311">在下一個教學課程中，您會將 [索引] 頁面的功能進行拓展，新增排序、篩選和分頁功能。</span><span class="sxs-lookup"><span data-stu-id="a1c70-311">In the next tutorial you'll expand the functionality of the **Index** page by adding sorting, filtering, and paging.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a1c70-312">[上一頁](intro.md)
> [下一頁](sort-filter-page.md)</span><span class="sxs-lookup"><span data-stu-id="a1c70-312">[Previous](intro.md)
[Next](sort-filter-page.md)</span></span>  
