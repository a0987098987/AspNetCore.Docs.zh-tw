---
title: "ASP.NET Core MVC EF 核心 CRUD-2 的 10"
author: tdykstra
description: 
keywords: "ASP.NET Core、 Entity Framework Core、 CRUD，建立、 讀取、 更新、 刪除"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: 6e1cd570-40f1-4b24-8b6e-7d2d27758f18
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/crud
ms.openlocfilehash: b99a58d77d4f1751753ae576ade4bd6dd981fbbf
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/25/2017
---
# <a name="create-read-update-and-delete---ef-core-with-aspnet-core-mvc-tutorial-2-of-10"></a><span data-ttu-id="a46b6-103">建立、 讀取、 更新和刪除-EF Core 與 ASP.NET Core MVC 教學課程 (2 / 10)</span><span class="sxs-lookup"><span data-stu-id="a46b6-103">Create, Read, Update, and Delete - EF Core with ASP.NET Core MVC tutorial (2 of 10)</span></span>

<span data-ttu-id="a46b6-104">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a46b6-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a46b6-105">Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Core MVC web 應用程式使用 Entity Framework Core 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a46b6-105">The Contoso University sample web application demonstrates how to create ASP.NET Core MVC web applications using Entity Framework Core and Visual Studio.</span></span> <span data-ttu-id="a46b6-106">教學課程系列的相關資訊，請參閱[系列的第一個教學課程](intro.md)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-106">For information about the tutorial series, see [the first tutorial in the series](intro.md).</span></span>

<span data-ttu-id="a46b6-107">在上一個教學課程中，您可以建立 MVC 應用程式儲存和使用 Entity Framework 和 SQL Server LocalDB 顯示資料。</span><span class="sxs-lookup"><span data-stu-id="a46b6-107">In the previous tutorial, you created an MVC application that stores and displays data using the Entity Framework and SQL Server LocalDB.</span></span> <span data-ttu-id="a46b6-108">在本教學課程中，您將檢閱，並自訂 CRUD （建立、 讀取、 更新、 刪除） MVC scaffolding 自動為您建立控制器和檢視中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a46b6-108">In this tutorial, you'll review and customize the CRUD (create, read, update, delete) code that the MVC scaffolding automatically creates for you in controllers and views.</span></span>

> [!NOTE] 
> <span data-ttu-id="a46b6-109">它是常見的作法，若要建立您的控制器和資料存取層之間的抽象層實作儲存機制模式。</span><span class="sxs-lookup"><span data-stu-id="a46b6-109">It's a common practice to implement the repository pattern in order to create an abstraction layer between your controller and the data access layer.</span></span> <span data-ttu-id="a46b6-110">若要簡單，並著重於教導如何使用 Entity Framework 本身，請保留這些教學課程，它們不使用儲存機制。</span><span class="sxs-lookup"><span data-stu-id="a46b6-110">To keep these tutorials simple and focused on teaching how to use the Entity Framework itself, they don't use repositories.</span></span> <span data-ttu-id="a46b6-111">使用 EF 儲存機制的相關資訊，請參閱[這一系列中的最後一個教學課程](advanced.md)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-111">For information about repositories with EF, see [the last tutorial in this series](advanced.md).</span></span>

<span data-ttu-id="a46b6-112">在本教學課程中，您將使用下列網頁：</span><span class="sxs-lookup"><span data-stu-id="a46b6-112">In this tutorial, you'll work with the following web pages:</span></span>

![學生詳細資料頁面](crud/_static/student-details.png)

![學生建立頁面](crud/_static/student-create.png)

![學生編輯頁面](crud/_static/student-edit.png)

![學生刪除頁面](crud/_static/student-delete.png)

## <a name="customize-the-details-page"></a><span data-ttu-id="a46b6-117">自訂的詳細資料頁面</span><span class="sxs-lookup"><span data-stu-id="a46b6-117">Customize the Details page</span></span>

<span data-ttu-id="a46b6-118">學生索引頁面的 scaffold 程式碼中省略了`Enrollments`屬性，因為該屬性會保存集合。</span><span class="sxs-lookup"><span data-stu-id="a46b6-118">The scaffolded code for the Students Index page left out the `Enrollments` property, because that property holds a collection.</span></span> <span data-ttu-id="a46b6-119">在**詳細資料**您要顯示集合的內容以 HTML 表格的網頁。</span><span class="sxs-lookup"><span data-stu-id="a46b6-119">In the **Details** page you'll display the contents of the collection in an HTML table.</span></span>

<span data-ttu-id="a46b6-120">在*Controllers/StudentsController.cs*，動作方法的詳細資料檢視會使用`SingleOrDefaultAsync`方法來擷取單一`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-120">In *Controllers/StudentsController.cs*, the action method for the Details view uses the `SingleOrDefaultAsync` method to retrieve a single `Student` entity.</span></span> <span data-ttu-id="a46b6-121">加入程式碼，呼叫`Include`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-121">Add code that calls `Include`.</span></span> <span data-ttu-id="a46b6-122">`ThenInclude`與`AsNoTracking`方法，如下列反白顯示的程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="a46b6-122">`ThenInclude`,  and `AsNoTracking` methods, as shown in the following highlighted code.</span></span>

<span data-ttu-id="a46b6-123">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-123">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Details&highlight=8-12)]</span></span>

<span data-ttu-id="a46b6-124">`Include`和`ThenInclude`方法會造成載入內容`Student.Enrollments`導覽屬性，以及在每個註冊內`Enrollment.Course`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-124">The `Include` and `ThenInclude` methods cause the context to load the `Student.Enrollments` navigation property, and within each enrollment the `Enrollment.Course` navigation property.</span></span>  <span data-ttu-id="a46b6-125">您將進一步了解這些方法進行[讀取相關的資料](read-related-data.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="a46b6-125">You'll learn more about these methods in the [reading related data](read-related-data.md) tutorial.</span></span>

<span data-ttu-id="a46b6-126">`AsNoTracking`方法可改善的案例，其中傳回的實體將不會更新在目前內容的存留期間的效能。</span><span class="sxs-lookup"><span data-stu-id="a46b6-126">The `AsNoTracking` method improves performance in scenarios where the entities returned will not be updated in the current context's lifetime.</span></span> <span data-ttu-id="a46b6-127">您將深入了解`AsNoTracking`在本教學課程結尾處。</span><span class="sxs-lookup"><span data-stu-id="a46b6-127">You'll learn more about `AsNoTracking` at the end of this tutorial.</span></span>

### <a name="route-data"></a><span data-ttu-id="a46b6-128">路由資料</span><span class="sxs-lookup"><span data-stu-id="a46b6-128">Route data</span></span>

<span data-ttu-id="a46b6-129">索引鍵的值，傳遞至`Details`方法來自*路由資料*。</span><span class="sxs-lookup"><span data-stu-id="a46b6-129">The key value that is passed to the `Details` method comes from *route data*.</span></span> <span data-ttu-id="a46b6-130">路由資料是 URL 的區段中找到的模型繫結器的資料。</span><span class="sxs-lookup"><span data-stu-id="a46b6-130">Route data is data that the model binder found in a segment of the URL.</span></span> <span data-ttu-id="a46b6-131">例如，預設路由會指定控制器、 動作和 id 的區段：</span><span class="sxs-lookup"><span data-stu-id="a46b6-131">For example, the default route specifies controller, action, and id segments:</span></span>

<span data-ttu-id="a46b6-132">[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-132">[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Route&highlight=5)]</span></span>

<span data-ttu-id="a46b6-133">在下列 URL 中，預設路由對應講師為控制站，做為動作的索引和 1 做為識別碼;這種路由資料值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-133">In the following URL, the default route maps Instructor as the controller, Index as the action, and 1 as the id; these are route data values.</span></span>

```
http://localhost:1230/Instructor/Index/1?courseID=2021
```

<span data-ttu-id="a46b6-134">URL 的最後一個部分 (「？ courseID = 2021年 」) 是查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-134">The last part of the URL ("?courseID=2021") is a query string value.</span></span> <span data-ttu-id="a46b6-135">模型繫結器也會傳遞至的識別碼值`Details`方法`id`參數，如果您將它當做查詢字串值：</span><span class="sxs-lookup"><span data-stu-id="a46b6-135">The model binder will also pass the ID value to the `Details` method `id` parameter if you pass it as a query string value:</span></span>

```
http://localhost:1230/Instructor/Index?id=1&CourseID=2021
```

<span data-ttu-id="a46b6-136">在索引頁面中，Razor 檢視中的標記協助程式陳述式會建立超連結 Url。</span><span class="sxs-lookup"><span data-stu-id="a46b6-136">In the Index page, hyperlink URLs are created by tag helper statements in the Razor view.</span></span> <span data-ttu-id="a46b6-137">下列的 Razor 程式碼，`id`參數必須符合預設路由，因此`id`會加入至路由資料。</span><span class="sxs-lookup"><span data-stu-id="a46b6-137">In the following Razor code, the `id` parameter matches the default route, so `id` is added to the route data.</span></span>

```html
<a asp-action="Edit" asp-route-id="@item.ID">Edit</a>
```

<span data-ttu-id="a46b6-138">這會產生下列 HTML 時`item.ID`為 6:</span><span class="sxs-lookup"><span data-stu-id="a46b6-138">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit/6">Edit</a>
```

<span data-ttu-id="a46b6-139">下列的 Razor 程式碼，`studentID`不符合預設路由中的參數，所以它會加入做為查詢字串。</span><span class="sxs-lookup"><span data-stu-id="a46b6-139">In the following Razor code, `studentID` doesn't match a parameter in the default route, so it's added as a query string.</span></span>

```html
<a asp-action="Edit" asp-route-studentID="@item.ID">Edit</a>
```

<span data-ttu-id="a46b6-140">這會產生下列 HTML 時`item.ID`為 6:</span><span class="sxs-lookup"><span data-stu-id="a46b6-140">This generates the following HTML when `item.ID` is 6:</span></span>

```html
<a href="/Students/Edit?studentID=6">Edit</a>
```

<span data-ttu-id="a46b6-141">如需標記協助程式的詳細資訊，請參閱[標記協助程式中 ASP.NET Core](xref:mvc/views/tag-helpers/intro)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-141">For more information about tag helpers, see [Tag helpers in ASP.NET Core](xref:mvc/views/tag-helpers/intro).</span></span>

### <a name="add-enrollments-to-the-details-view"></a><span data-ttu-id="a46b6-142">加入詳細資料檢視中的註冊項目</span><span class="sxs-lookup"><span data-stu-id="a46b6-142">Add enrollments to the Details view</span></span>

<span data-ttu-id="a46b6-143">開啟*Views/Students/Details.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="a46b6-143">Open *Views/Students/Details.cshtml*.</span></span> <span data-ttu-id="a46b6-144">每個欄位會顯示使用`DisplayNameFor`和`DisplayFor`helper，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a46b6-144">Each field is displayed using `DisplayNameFor` and `DisplayFor` helper, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=13-18&highlight=2,5)]

<span data-ttu-id="a46b6-145">之後的最後一個欄位，並立即結束之前`</dl>`標記中，加入下列程式碼，顯示一份註冊項目：</span><span class="sxs-lookup"><span data-stu-id="a46b6-145">After the last field and immediately before the closing `</dl>` tag, add the following code to display a list of enrollments:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Details.cshtml?range=31-52)]

<span data-ttu-id="a46b6-146">如果程式碼縮排錯誤貼上程式碼後，請按 CTRL-K-D 更正它。</span><span class="sxs-lookup"><span data-stu-id="a46b6-146">If code indentation is wrong after you paste the code, press CTRL-K-D to correct it.</span></span>

<span data-ttu-id="a46b6-147">此程式碼執行迴圈中的實體`Enrollments`導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-147">This code loops through the entities in the `Enrollments` navigation property.</span></span> <span data-ttu-id="a46b6-148">針對每個註冊，它會顯示課程標題以及等級。</span><span class="sxs-lookup"><span data-stu-id="a46b6-148">For each enrollment, it displays the course title and the grade.</span></span> <span data-ttu-id="a46b6-149">課程標題會從儲存在課程實體`Course`註冊實體的導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-149">The course title is retrieved from the Course entity that's stored in the `Course` navigation property of the Enrollments entity.</span></span>

<span data-ttu-id="a46b6-150">執行應用程式中，選取**學生**索引標籤，然後按一下**詳細資料**學生的連結。</span><span class="sxs-lookup"><span data-stu-id="a46b6-150">Run the application, select the **Students** tab, and click the **Details** link for a student.</span></span> <span data-ttu-id="a46b6-151">您的選取學生看到 courses 以及等級清單：</span><span class="sxs-lookup"><span data-stu-id="a46b6-151">You see the list of courses and grades for the selected student:</span></span>

![學生詳細資料頁面](crud/_static/student-details.png)

## <a name="update-the-create-page"></a><span data-ttu-id="a46b6-153">更新 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="a46b6-153">Update the Create page</span></span>

<span data-ttu-id="a46b6-154">在*StudentsController.cs*，修改 HttpPost`Create`方法加入 try-catch 區塊並移除從 ID`Bind`屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-154">In *StudentsController.cs*, modify the HttpPost `Create` method by adding a try-catch block and removing ID from the `Bind` attribute.</span></span>

<span data-ttu-id="a46b6-155">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-155">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=4,6-7,14-21)]</span></span>

<span data-ttu-id="a46b6-156">這個程式碼加入 ASP.NET MVC 模型繫結器的學生實體所建立的學生實體設定，然後將變更儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="a46b6-156">This code adds the Student entity created by the ASP.NET MVC model binder to the Students entity set and then saves the changes to the database.</span></span> <span data-ttu-id="a46b6-157">（模型繫結器指的是 ASP.NET MVC 功能，可讓您更輕鬆地使用 送出表單的資料，將已張貼的表單值轉換成 CLR 類型的模型繫結器，並將其傳遞至動作方法參數中。</span><span class="sxs-lookup"><span data-stu-id="a46b6-157">(Model binder refers to the ASP.NET MVC functionality that makes it easier for you to work with data submitted by a form; a model binder converts posted form values to CLR types and passes them to the action method in parameters.</span></span> <span data-ttu-id="a46b6-158">在此情況下，在模型繫結具現化學生實體為您使用表單集合中的屬性值。）</span><span class="sxs-lookup"><span data-stu-id="a46b6-158">In this case, the model binder instantiates a Student entity for you using property values from the Form collection.)</span></span>

<span data-ttu-id="a46b6-159">您移除`ID`從`Bind`屬性，因為識別碼是 SQL Server 會插入資料列時自動設定的主要金鑰值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-159">You removed `ID` from the `Bind` attribute because ID is the primary key value which SQL Server will set automatically when the row is inserted.</span></span> <span data-ttu-id="a46b6-160">使用者輸入未設定的識別碼值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-160">Input from the user does not set the ID value.</span></span>

<span data-ttu-id="a46b6-161">除了`Bind`屬性，try catch 區塊是唯一的 scaffold 的程式碼所做的變更。</span><span class="sxs-lookup"><span data-stu-id="a46b6-161">Other than the `Bind` attribute, the try-catch block is the only change you've made to the scaffolded code.</span></span> <span data-ttu-id="a46b6-162">如果例外狀況衍生自`DbUpdateException`會攔截到儲存變更時，會顯示一般錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a46b6-162">If an exception that derives from `DbUpdateException` is caught while the changes are being saved, a generic error message is displayed.</span></span> <span data-ttu-id="a46b6-163">`DbUpdateException`例外狀況有時是由外部應用程式，而不是程式設計錯誤，造成，因此再試一次會建議使用者。</span><span class="sxs-lookup"><span data-stu-id="a46b6-163">`DbUpdateException` exceptions are sometimes caused by something external to the application rather than a programming error, so the user is advised to try again.</span></span> <span data-ttu-id="a46b6-164">未實作此範例中，雖然生產品質應用程式會記錄該例外狀況。</span><span class="sxs-lookup"><span data-stu-id="a46b6-164">Although not implemented in this sample, a production quality application would log the exception.</span></span> <span data-ttu-id="a46b6-165">如需詳細資訊，請參閱**記錄檔，以深入了解**一節中[監控與遙測 （建置真實世界雲端應用程式與 Azure）](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-165">For more information, see the **Log for insight** section in [Monitoring and Telemetry (Building Real-World Cloud Apps with Azure)](http://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/monitoring-and-telemetry).</span></span>

<span data-ttu-id="a46b6-166">`ValidateAntiForgeryToken`屬性有助於防止跨網站要求偽造 (CSRF) 攻擊。</span><span class="sxs-lookup"><span data-stu-id="a46b6-166">The `ValidateAntiForgeryToken` attribute helps prevent cross-site request forgery (CSRF) attacks.</span></span> <span data-ttu-id="a46b6-167">語彙基元自動插入檢視[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)和由使用者提交表單時，會包含。</span><span class="sxs-lookup"><span data-stu-id="a46b6-167">The token is automatically injected into the view by the [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) and is included when the form is submitted by the user.</span></span> <span data-ttu-id="a46b6-168">權杖由驗證`ValidateAntiForgeryToken`屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-168">The token is validated by the `ValidateAntiForgeryToken` attribute.</span></span> <span data-ttu-id="a46b6-169">如需 CSRF 的詳細資訊，請參閱[反要求偽造](../../security/anti-request-forgery.md)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-169">For more information about CSRF, see [Anti-Request Forgery](../../security/anti-request-forgery.md).</span></span>

<a id="overpost"></a>
### <a name="security-note-about-overposting"></a><span data-ttu-id="a46b6-170">關於 overposting 安全性注意事項</span><span class="sxs-lookup"><span data-stu-id="a46b6-170">Security note about overposting</span></span>

<span data-ttu-id="a46b6-171">`Bind` Scaffold 的程式碼包含的屬性`Create`方法是一種方式防止 overposting 中建立的案例。</span><span class="sxs-lookup"><span data-stu-id="a46b6-171">The `Bind` attribute that the scaffolded code includes on the `Create` method is one way to protect against overposting in create scenarios.</span></span> <span data-ttu-id="a46b6-172">例如，假設 學生實體包含`Secret`屬性，您不想要設定這個 web 網頁。</span><span class="sxs-lookup"><span data-stu-id="a46b6-172">For example, suppose the Student entity includes a `Secret` property that you don't want this web page to set.</span></span>

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

<span data-ttu-id="a46b6-173">即使您沒有`Secret`在網頁上，駭客的欄位無法使用 Fiddler 之類的工具或撰寫一些 JavaScript，張貼`Secret`形成值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-173">Even if you don't have a `Secret` field on the web page, a hacker could use a tool such as Fiddler, or write some JavaScript, to post a `Secret` form value.</span></span> <span data-ttu-id="a46b6-174">不含`Bind`限制建立的 「 學生 」 執行個體模型繫結器時，會使用模型繫結器的欄位的屬性將會收取，`Secret`形成值，並使用它來建立學生實體執行個體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-174">Without the `Bind` attribute limiting the fields that the model binder uses when it creates a Student instance, the model binder would pick up that `Secret` form value and use it to create the Student entity instance.</span></span> <span data-ttu-id="a46b6-175">然後任何值指定給駭客`Secret`表單欄位會更新您的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="a46b6-175">Then whatever value the hacker specified for the `Secret` form field would be updated in your database.</span></span> <span data-ttu-id="a46b6-176">下圖顯示 Fiddler 工具加入`Secret`（具有值"OverPost"） 的已張貼的表單值的欄位。</span><span class="sxs-lookup"><span data-stu-id="a46b6-176">The following image shows the Fiddler tool adding the `Secret` field (with the value "OverPost") to the posted form values.</span></span>

![Fiddler 加入密碼欄位](crud/_static/fiddler.png)

<span data-ttu-id="a46b6-178">值"OverPost 」 會接著已成功加入至`Secret`屬性插入的資料列，雖然您不想網頁能夠將該屬性設定。</span><span class="sxs-lookup"><span data-stu-id="a46b6-178">The value "OverPost" would then be successfully added to the `Secret` property of the inserted row, although you never intended that the web page be able to set that property.</span></span>

<span data-ttu-id="a46b6-179">您可以避免在編輯案例 overposting 先從資料庫讀取的實體，然後再呼叫`TryUpdateModel`，並傳遞明確允許的內容清單中。</span><span class="sxs-lookup"><span data-stu-id="a46b6-179">You can prevent overposting in edit scenarios by reading the entity from the database first and then calling `TryUpdateModel`, passing in an explicit allowed properties list.</span></span> <span data-ttu-id="a46b6-180">這是這些教學課程所使用的方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-180">That is the method used in these tutorials.</span></span>

<span data-ttu-id="a46b6-181">若要避免許多開發人員慣用的 overposting 替代方式是使用模型繫結的檢視模型，而不是實體類別。</span><span class="sxs-lookup"><span data-stu-id="a46b6-181">An alternative way to prevent overposting that is preferred by many developers is to use view models rather than entity classes with model binding.</span></span> <span data-ttu-id="a46b6-182">包含只想要更新的檢視模型中的屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-182">Include only the properties you want to update in the view model.</span></span> <span data-ttu-id="a46b6-183">一旦完成 MVC 模型繫結器，請將檢視模型內容複製到實體執行個體，選擇性地使用 AutoMapper 之類的工具。</span><span class="sxs-lookup"><span data-stu-id="a46b6-183">Once the MVC model binder has finished, copy the view model properties to the entity instance, optionally using a tool such as AutoMapper.</span></span> <span data-ttu-id="a46b6-184">使用`_context.Entry`實體執行個體，其狀態設定為上`Unchanged`，然後設定`Property("PropertyName").IsModified`為 true 的包含檢視模型中每一個實體屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-184">Use `_context.Entry` on the entity instance to set its state to `Unchanged`, and then set `Property("PropertyName").IsModified` to true on each entity property that is included in the view model.</span></span> <span data-ttu-id="a46b6-185">這個方法的運作方式同時編輯，並建立案例。</span><span class="sxs-lookup"><span data-stu-id="a46b6-185">This method works in both edit and create scenarios.</span></span>

### <a name="test-the-create-page"></a><span data-ttu-id="a46b6-186">測試 [建立] 頁面</span><span class="sxs-lookup"><span data-stu-id="a46b6-186">Test the Create page</span></span>

<span data-ttu-id="a46b6-187">中的程式碼*Views/Students/Create.cshtml*使用`label`， `input`，和`span`（適用於驗證的訊息） 標記協助程式的每個欄位。</span><span class="sxs-lookup"><span data-stu-id="a46b6-187">The code in *Views/Students/Create.cshtml* uses `label`, `input`, and `span` (for validation messages) tag helpers for each field.</span></span>

<span data-ttu-id="a46b6-188">執行選取的頁面**學生** 索引標籤，然後按一下**新建**。</span><span class="sxs-lookup"><span data-stu-id="a46b6-188">Run the page by selecting the **Students** tab and clicking **Create New**.</span></span>

<span data-ttu-id="a46b6-189">輸入名稱和日期。</span><span class="sxs-lookup"><span data-stu-id="a46b6-189">Enter names and a date.</span></span> <span data-ttu-id="a46b6-190">請嘗試輸入無效的日期，如果您的瀏覽器可讓您執行此作業。</span><span class="sxs-lookup"><span data-stu-id="a46b6-190">Try entering an invalid date if your browser lets you do that.</span></span> <span data-ttu-id="a46b6-191">（某些瀏覽器會強制要求您使用 日期選擇器。）然後按一下 **建立**以查看錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a46b6-191">(Some browsers force you to use a date picker.) Then click **Create** to see the error message.</span></span>

![日期的驗證錯誤](crud/_static/date-error.png)

<span data-ttu-id="a46b6-193">這是預設值; 您取得的伺服器端驗證在稍後的教學課程中，您會看到如何加入屬性也會產生用戶端驗證的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a46b6-193">This is server-side validation that you get by default; in a later tutorial you'll see how to add attributes that will generate code for client-side validation also.</span></span> <span data-ttu-id="a46b6-194">將下列反白顯示的程式碼顯示中的模型驗證檢查`Create`方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-194">The following highlighted code shows the model validation check in the `Create` method.</span></span>

<span data-ttu-id="a46b6-195">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-195">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Create&highlight=8)]</span></span>

<span data-ttu-id="a46b6-196">將日期變更為有效的值，然後按一下**建立**以查看出現在新的學生**索引**頁面。</span><span class="sxs-lookup"><span data-stu-id="a46b6-196">Change the date to a valid value and click **Create** to see the new student appear in the **Index** page.</span></span>

## <a name="update-the-edit-page"></a><span data-ttu-id="a46b6-197">更新編輯頁面</span><span class="sxs-lookup"><span data-stu-id="a46b6-197">Update the Edit page</span></span>

<span data-ttu-id="a46b6-198">在*StudentController.cs*，HttpGet`Edit`方法 (不含一個`HttpPost`屬性) 會使用`SingleOrDefaultAsync`方法來擷取所選的學生實體，如同`Details`方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-198">In *StudentController.cs*, the HttpGet `Edit` method (the one without the `HttpPost` attribute) uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the `Details` method.</span></span> <span data-ttu-id="a46b6-199">您不需要變更這個方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-199">You don't need to change this method.</span></span>

### <a name="recommended-httppost-edit-code-read-and-update"></a><span data-ttu-id="a46b6-200">建議 HttpPost 編輯程式碼： 讀取和更新</span><span class="sxs-lookup"><span data-stu-id="a46b6-200">Recommended HttpPost Edit code: Read and update</span></span>

<span data-ttu-id="a46b6-201">下列程式碼中取代 HttpPost 編輯動作方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-201">Replace the HttpPost Edit action method with the following code.</span></span>

<span data-ttu-id="a46b6-202">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-202">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ReadFirst)]</span></span>

<span data-ttu-id="a46b6-203">這些變更實作安全性最佳作法，以防止 overposting。</span><span class="sxs-lookup"><span data-stu-id="a46b6-203">These changes implement a security best practice to prevent overposting.</span></span> <span data-ttu-id="a46b6-204">產生 scaffolder`Bind`屬性和加入的實體集與模型繫結器所建立的實體`Modified`旗標。</span><span class="sxs-lookup"><span data-stu-id="a46b6-204">The scaffolder generated a `Bind` attribute and added the entity created by the model binder to the entity set with a `Modified` flag.</span></span> <span data-ttu-id="a46b6-205">程式碼不建議用於許多案例因為`Bind`屬性清除任何預先存在的資料中未列出的欄位中`Include`參數。</span><span class="sxs-lookup"><span data-stu-id="a46b6-205">That code is not recommended for many scenarios because the `Bind` attribute clears out any pre-existing data in fields not listed in the `Include` parameter.</span></span>

<span data-ttu-id="a46b6-206">新的程式碼會讀取現有的實體和呼叫`TryUpdateModel`更新中擷取之實體的欄位[根據使用者輸入中已張貼的表單資料](xref:mvc/models/model-binding#how-model-binding-works)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-206">The new code reads the existing entity and calls `TryUpdateModel` to update fields in the retrieved entity [based on user input in the posted form data](xref:mvc/models/model-binding#how-model-binding-works).</span></span> <span data-ttu-id="a46b6-207">Entity Framework 自動變更追蹤設定`Modified`旗標變更表單輸入的欄位。</span><span class="sxs-lookup"><span data-stu-id="a46b6-207">The Entity Framework's automatic change tracking sets the `Modified` flag on the fields that are changed by form input.</span></span> <span data-ttu-id="a46b6-208">當`SaveChanges`呼叫方法時，Entity Framework 建立 SQL 陳述式來更新資料庫的資料列。</span><span class="sxs-lookup"><span data-stu-id="a46b6-208">When the `SaveChanges` method is called, the Entity Framework creates SQL statements to update the database row.</span></span> <span data-ttu-id="a46b6-209">並行衝突，系統會忽略，只有使用者已更新的資料表資料行在更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="a46b6-209">Concurrency conflicts are ignored, and only the table columns that were updated by the user are updated in the database.</span></span> <span data-ttu-id="a46b6-210">（之後的教學課程示範如何處理並行存取衝突）。</span><span class="sxs-lookup"><span data-stu-id="a46b6-210">(A later tutorial shows how to handle concurrency conflicts.)</span></span>

<span data-ttu-id="a46b6-211">最佳作法是避免 overposting，您想要透過可更新的欄位**編輯**頁面會在允許清單中的`TryUpdateModel`參數。</span><span class="sxs-lookup"><span data-stu-id="a46b6-211">As a best practice to prevent overposting, the fields that you want to be updateable by the **Edit** page are whitelisted in the `TryUpdateModel` parameters.</span></span> <span data-ttu-id="a46b6-212">（空的字串前面的參數清單中的欄位清單是針對使用表單欄位名稱的前置詞）。目前沒有額外的欄位，您要保護，但列出您想要繫結模型繫結器的欄位可確保您將欄位加入至資料模型在未來，如果它們自動保護直到您明確地在此處加入。</span><span class="sxs-lookup"><span data-stu-id="a46b6-212">(The empty string preceding the list of fields in the parameter list is for a prefix to use with the form fields names.) Currently there are no extra fields that you're protecting, but listing the fields that you want the model binder to bind ensures that if you add fields to the data model in the future, they're automatically protected until you explicitly add them here.</span></span>

<span data-ttu-id="a46b6-213">由於這些變更，HttpPost 的方法簽章`Edit`方法等同於 HttpGet`Edit`方法; 因此已重新命名方法`EditPost`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-213">As a result of these changes, the method signature of the HttpPost `Edit` method is the same as the HttpGet `Edit` method; therefore you've renamed the method `EditPost`.</span></span>

### <a name="alternative-httppost-edit-code-create-and-attach"></a><span data-ttu-id="a46b6-214">替代 HttpPost 編輯程式碼： 建立並附加</span><span class="sxs-lookup"><span data-stu-id="a46b6-214">Alternative HttpPost Edit code: Create and attach</span></span>

<span data-ttu-id="a46b6-215">建議的 HttpPost 編輯程式碼可確保只有已變更的資料行取得更新，並保留您不想包含模型繫結的屬性中的資料。</span><span class="sxs-lookup"><span data-stu-id="a46b6-215">The recommended HttpPost edit code ensures that only changed columns get updated and preserves data in properties that you don't want included for model binding.</span></span> <span data-ttu-id="a46b6-216">不過，讀取第一個方法都需要額外的資料庫讀取，並可能會導致更複雜的程式碼來處理並行衝突。</span><span class="sxs-lookup"><span data-stu-id="a46b6-216">However, the read-first approach requires an extra database read, and can result in more complex code for handling concurrency conflicts.</span></span> <span data-ttu-id="a46b6-217">替代方法是附加至 EF 內容的模型繫結器所建立的實體，並將其標示為已修改。</span><span class="sxs-lookup"><span data-stu-id="a46b6-217">An alternative is to attach an entity created by the model binder to the EF context and mark it as modified.</span></span> <span data-ttu-id="a46b6-218">(沒有更新您的專案與此程式碼，它只顯示說明的選擇性的方法。)</span><span class="sxs-lookup"><span data-stu-id="a46b6-218">(Don't update your project with this code, it's only shown to illustrate an optional approach.)</span></span> 

<span data-ttu-id="a46b6-219">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-219">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_CreateAndAttach)]</span></span>

<span data-ttu-id="a46b6-220">網頁 UI 實體中包含的所有欄位，可以更新任何語言時，您可以使用這個方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-220">You can use this approach when the web page UI includes all of the fields in the entity and can update any of them.</span></span>

<span data-ttu-id="a46b6-221">Scaffold 的程式碼使用的建立和附加方法，但只會攔截`DbUpdateConcurrencyException`例外狀況，會傳回 404 錯誤代碼。</span><span class="sxs-lookup"><span data-stu-id="a46b6-221">The scaffolded code uses the create-and-attach approach but only catches `DbUpdateConcurrencyException` exceptions and returns 404 error codes.</span></span>  <span data-ttu-id="a46b6-222">顯示的範例攔截任何資料庫更新例外狀況，並顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a46b6-222">The example shown catches any database update exception and displays an error message.</span></span>

### <a name="entity-states"></a><span data-ttu-id="a46b6-223">實體狀態</span><span class="sxs-lookup"><span data-stu-id="a46b6-223">Entity States</span></span>

<span data-ttu-id="a46b6-224">是否在記憶體中的實體會在資料庫中，其對應的資料列和同步，這項資訊會決定當您呼叫時，會發生什麼情況，會持續追蹤的資料庫內容`SaveChanges`方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-224">The database context keeps track of whether entities in memory are in sync with their corresponding rows in the database, and this information determines what happens when you call the `SaveChanges` method.</span></span> <span data-ttu-id="a46b6-225">例如，當您傳遞至新的實體`Add`方法中，實體的狀態設為`Added`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-225">For example, when you pass a new entity to the `Add` method, that entity's state is set to `Added`.</span></span> <span data-ttu-id="a46b6-226">然後當您呼叫`SaveChanges`方法，將資料庫內容發出 SQL INSERT 命令。</span><span class="sxs-lookup"><span data-stu-id="a46b6-226">Then when you call the `SaveChanges` method, the database context issues a SQL INSERT command.</span></span>

<span data-ttu-id="a46b6-227">實體可能處於下列狀態其中之一：</span><span class="sxs-lookup"><span data-stu-id="a46b6-227">An entity may be in one of the following states:</span></span>

* <span data-ttu-id="a46b6-228">`Added`.</span><span class="sxs-lookup"><span data-stu-id="a46b6-228">`Added`.</span></span> <span data-ttu-id="a46b6-229">在資料庫中尚未存在的實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-229">The entity does not yet exist in the database.</span></span> <span data-ttu-id="a46b6-230">`SaveChanges`方法發出的 INSERT 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a46b6-230">The `SaveChanges` method issues an INSERT statement.</span></span>

* <span data-ttu-id="a46b6-231">`Unchanged`.</span><span class="sxs-lookup"><span data-stu-id="a46b6-231">`Unchanged`.</span></span> <span data-ttu-id="a46b6-232">與這個實體所完成，不需要`SaveChanges`方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-232">Nothing needs to be done with this entity by the `SaveChanges` method.</span></span> <span data-ttu-id="a46b6-233">當您從資料庫讀取實體時，實體開始於此狀態。</span><span class="sxs-lookup"><span data-stu-id="a46b6-233">When you read an entity from the database, the entity starts out with this status.</span></span>

* <span data-ttu-id="a46b6-234">`Modified`.</span><span class="sxs-lookup"><span data-stu-id="a46b6-234">`Modified`.</span></span> <span data-ttu-id="a46b6-235">修改部分或所有實體的屬性值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-235">Some or all of the entity's property values have been modified.</span></span> <span data-ttu-id="a46b6-236">`SaveChanges`方法發出 UPDATE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a46b6-236">The `SaveChanges` method issues an UPDATE statement.</span></span>

* <span data-ttu-id="a46b6-237">`Deleted`.</span><span class="sxs-lookup"><span data-stu-id="a46b6-237">`Deleted`.</span></span> <span data-ttu-id="a46b6-238">實體已被標示為刪除。</span><span class="sxs-lookup"><span data-stu-id="a46b6-238">The entity has been marked for deletion.</span></span> <span data-ttu-id="a46b6-239">`SaveChanges`方法發出 DELETE 陳述式。</span><span class="sxs-lookup"><span data-stu-id="a46b6-239">The `SaveChanges` method issues a DELETE statement.</span></span>

* <span data-ttu-id="a46b6-240">`Detached`.</span><span class="sxs-lookup"><span data-stu-id="a46b6-240">`Detached`.</span></span> <span data-ttu-id="a46b6-241">不追蹤實體的資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="a46b6-241">The entity isn't being tracked by the database context.</span></span>

<span data-ttu-id="a46b6-242">在桌面應用程式，會通常是自動設定的狀態變更。</span><span class="sxs-lookup"><span data-stu-id="a46b6-242">In a desktop application, state changes are typically set automatically.</span></span> <span data-ttu-id="a46b6-243">您讀取實體，並變更它的某些屬性值。</span><span class="sxs-lookup"><span data-stu-id="a46b6-243">You read an entity and make changes to some of its property values.</span></span> <span data-ttu-id="a46b6-244">這會導致其實體狀態，以自動變更為`Modified`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-244">This causes its entity state to automatically be changed to `Modified`.</span></span> <span data-ttu-id="a46b6-245">然後當您呼叫`SaveChanges`，Entity Framework 會產生 SQL UPDATE 陳述式，以更新只針對您所變更的實際屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-245">Then when you call `SaveChanges`, the Entity Framework generates a SQL UPDATE statement that updates only the actual properties that you changed.</span></span>

<span data-ttu-id="a46b6-246">在 web 應用程式中，`DbContext`一開始讀取，而且會顯示来編輯其資料已處置之後網頁呈現的實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-246">In a web app, the `DbContext` that initially reads an entity and displays its data to be edited is disposed after a page is rendered.</span></span> <span data-ttu-id="a46b6-247">當 HttpPost`Edit`呼叫動作方法，建立新的 web 要求，而且您必須的新執行個體`DbContext`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-247">When the HttpPost `Edit` action method is called,  a new web request is made and you have a new instance of the `DbContext`.</span></span> <span data-ttu-id="a46b6-248">如果您重新讀取該新的內容中的實體，您會模擬桌面的處理。</span><span class="sxs-lookup"><span data-stu-id="a46b6-248">If you re-read the entity in that new context, you simulate desktop processing.</span></span>

<span data-ttu-id="a46b6-249">但如果您不想要執行額外的讀取作業，您必須使用模型繫結器所建立的實體物件。</span><span class="sxs-lookup"><span data-stu-id="a46b6-249">But if you don't want to do the extra read operation, you have to use the entity object created by the model binder.</span></span>  <span data-ttu-id="a46b6-250">若要這樣做最簡單的方式是將實體狀態設定修改，如同您在稍早所示的替代 HttpPost 編輯程式碼。</span><span class="sxs-lookup"><span data-stu-id="a46b6-250">The simplest way to do this is to set the entity state to Modified as is done in the alternative HttpPost Edit code shown earlier.</span></span> <span data-ttu-id="a46b6-251">然後當您呼叫`SaveChanges`，Entity Framework 更新的資料庫資料列的所有資料行，因為內容中有無從得知您已變更的屬性。</span><span class="sxs-lookup"><span data-stu-id="a46b6-251">Then when you call `SaveChanges`, the Entity Framework updates all columns of the database row, because the context has no way to know which properties you changed.</span></span>

<span data-ttu-id="a46b6-252">如果您想要避免讀取第一個方法，但您也想要更新的使用者實際上已變更的欄位更新 SQL 陳述式，程式碼是更複雜。</span><span class="sxs-lookup"><span data-stu-id="a46b6-252">If you want to avoid the read-first approach, but you also want the SQL UPDATE statement to update only the fields that the user actually changed, the code is more complex.</span></span> <span data-ttu-id="a46b6-253">您必須將原始值儲存在某些方面 (例如使用隱藏的欄位)，以便他們可以使用時 HttpPost`Edit`方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="a46b6-253">You have to save the original values in some way (such as by using hidden fields) so that they are available when the HttpPost `Edit` method is called.</span></span> <span data-ttu-id="a46b6-254">然後您可以建立使用原始值，也就是呼叫學生實體`Attach`方法的原始版本，在實體的實體的值更新為新的值，然後呼叫`SaveChanges`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-254">Then you can create a Student entity using the original values, call the `Attach` method with that original version of the entity, update the entity's values to the new values, and then call `SaveChanges`.</span></span>

### <a name="test-the-edit-page"></a><span data-ttu-id="a46b6-255">測試 編輯頁面</span><span class="sxs-lookup"><span data-stu-id="a46b6-255">Test the Edit page</span></span>

<span data-ttu-id="a46b6-256">執行應用程式並選取**學生**索引標籤，然後按一下 **編輯**超連結。</span><span class="sxs-lookup"><span data-stu-id="a46b6-256">Run the application and select the **Students** tab, then click an **Edit** hyperlink.</span></span>

![學生編輯頁面](crud/_static/student-edit.png)

<span data-ttu-id="a46b6-258">變更部分的資料和按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="a46b6-258">Change some of the data and click **Save**.</span></span> <span data-ttu-id="a46b6-259">**索引**頁面隨即開啟，並查看變更的資料。</span><span class="sxs-lookup"><span data-stu-id="a46b6-259">The **Index** page opens and you see the changed data.</span></span>

## <a name="update-the-delete-page"></a><span data-ttu-id="a46b6-260">更新刪除頁面</span><span class="sxs-lookup"><span data-stu-id="a46b6-260">Update the Delete page</span></span>

<span data-ttu-id="a46b6-261">在*StudentController.cs*，HttpGet 的範本程式碼`Delete`方法會使用`SingleOrDefaultAsync`方法來擷取所選的學生實體，如您所見的詳細資料和編輯方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-261">In *StudentController.cs*, the template code for the HttpGet `Delete` method uses the `SingleOrDefaultAsync` method to retrieve the selected Student entity, as you saw in the Details and Edit methods.</span></span> <span data-ttu-id="a46b6-262">不過，若要實作自訂的錯誤訊息時呼叫`SaveChanges`失敗，您會加入一些功能至這個方法，其對應的檢視。</span><span class="sxs-lookup"><span data-stu-id="a46b6-262">However, to implement a custom error message when the call to `SaveChanges` fails, you'll add some functionality to this method and its corresponding view.</span></span>

<span data-ttu-id="a46b6-263">當您看到更新，並建立作業，刪除作業都需要兩個動作方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-263">As you saw for update and create operations, delete operations require two action methods.</span></span> <span data-ttu-id="a46b6-264">呼叫以回應 GET 要求的方法會顯示可讓使用者有機會核准或取消刪除作業的檢視。</span><span class="sxs-lookup"><span data-stu-id="a46b6-264">The method that is called in response to a GET request displays a view that gives the user a chance to approve or cancel the delete operation.</span></span> <span data-ttu-id="a46b6-265">如果使用者核准時，會建立 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="a46b6-265">If the user approves it, a POST request is created.</span></span> <span data-ttu-id="a46b6-266">當發生這種情況，HttpPost`Delete`方法呼叫，然後該方法會實際執行刪除作業。</span><span class="sxs-lookup"><span data-stu-id="a46b6-266">When that happens, the HttpPost `Delete` method is called and then that method actually performs the delete operation.</span></span>

<span data-ttu-id="a46b6-267">您會加入 try-catch 區塊至 HttpPost`Delete`方法來處理資料庫的更新時可能會發生任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="a46b6-267">You'll add a try-catch block to the HttpPost `Delete` method to handle any errors that might occur when the database is updated.</span></span> <span data-ttu-id="a46b6-268">如果發生錯誤，HttpPost Delete 方法會呼叫 HttpGet Delete 方法，將其傳遞的參數，表示已發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="a46b6-268">If an error occurs, the HttpPost Delete method calls the HttpGet Delete method, passing it a parameter that indicates that an error has occurred.</span></span> <span data-ttu-id="a46b6-269">HttpGet Delete 方法然後重新顯示 [確認] 頁面，以及錯誤訊息，讓使用者有機會取消，或再試一次。</span><span class="sxs-lookup"><span data-stu-id="a46b6-269">The HttpGet Delete method then redisplays the confirmation page along with the error message, giving the user an opportunity to cancel or try again.</span></span>

<span data-ttu-id="a46b6-270">取代 HttpGet`Delete`動作方法，以下列程式碼，管理錯誤報告。</span><span class="sxs-lookup"><span data-stu-id="a46b6-270">Replace the HttpGet `Delete` action method with the following code, which manages error reporting.</span></span>

<span data-ttu-id="a46b6-271">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-271">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteGet&highlight=1,9,16-21)]</span></span>

<span data-ttu-id="a46b6-272">此程式碼會接受選擇性參數，指出是否將變更儲存在失敗之後呼叫此方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-272">This code accepts an optional parameter that indicates whether the method was called after a failure to save changes.</span></span> <span data-ttu-id="a46b6-273">此參數為 false 時 HttpGet`Delete`呼叫方法時沒有先前的失敗。</span><span class="sxs-lookup"><span data-stu-id="a46b6-273">This parameter is false when the HttpGet `Delete` method is called without a previous failure.</span></span> <span data-ttu-id="a46b6-274">當呼叫它的 HttpPost`Delete`方法以回應資料庫更新錯誤，參數為 true，且錯誤訊息會傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="a46b6-274">When it is called by the HttpPost `Delete` method in response to a database update error, the parameter is true and an error message is passed to the view.</span></span>

### <a name="the-read-first-approach-to-httppost-delete"></a><span data-ttu-id="a46b6-275">HttpPost Delete 讀取第一個方法</span><span class="sxs-lookup"><span data-stu-id="a46b6-275">The read-first approach to HttpPost Delete</span></span>

<span data-ttu-id="a46b6-276">取代 HttpPost`Delete`動作方法 (名為`DeleteConfirmed`) 取代下列程式碼，這會執行實際的刪除作業和攔截的任何資料庫更新錯誤。</span><span class="sxs-lookup"><span data-stu-id="a46b6-276">Replace the HttpPost `Delete` action method (named `DeleteConfirmed`) with the following code, which performs the actual delete operation and catches any database update errors.</span></span>

<span data-ttu-id="a46b6-277">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-277">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithReadFirst&highlight=6,8-11,13-14,18-23)]</span></span>

<span data-ttu-id="a46b6-278">此程式碼會擷取選取的實體，然後呼叫`Remove`實體的狀態設為方法`Deleted`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-278">This code retrieves the selected entity, then calls the `Remove` method to set the entity's status to `Deleted`.</span></span> <span data-ttu-id="a46b6-279">當`SaveChanges`呼叫時，SQL DELETE 命令產生。</span><span class="sxs-lookup"><span data-stu-id="a46b6-279">When `SaveChanges` is called, a SQL DELETE command is generated.</span></span>

### <a name="the-create-and-attach-approach-to-httppost-delete"></a><span data-ttu-id="a46b6-280">建立並附加 HttpPost Delete 方法</span><span class="sxs-lookup"><span data-stu-id="a46b6-280">The create-and-attach approach to HttpPost Delete</span></span>

<span data-ttu-id="a46b6-281">如果改善大量的應用程式中的效能是優先考量，就可以避免不必要的 SQL 查詢，藉由執行個體化使用只在主要的學生實體索引鍵的值，然後將實體狀態設定為`Deleted`。</span><span class="sxs-lookup"><span data-stu-id="a46b6-281">If improving performance in a high-volume application is a priority, you could avoid an unnecessary SQL query by instantiating a Student entity using only the primary key value and then setting the entity state to `Deleted`.</span></span> <span data-ttu-id="a46b6-282">這是所有 Entity Framework 必須以刪除實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-282">That's all that the Entity Framework needs in order to delete the entity.</span></span> <span data-ttu-id="a46b6-283">（不要將這段程式碼放置在專案中，就在這裡只是為了說明替代）。</span><span class="sxs-lookup"><span data-stu-id="a46b6-283">(Don't put this code in your project; it's here just to illustrate an alternative.)</span></span>

<span data-ttu-id="a46b6-284">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]</span><span class="sxs-lookup"><span data-stu-id="a46b6-284">[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_DeleteWithoutReadFirst&highlight=7-8)]</span></span>

<span data-ttu-id="a46b6-285">如果實體有相關應該一併刪除的資料，請確定該串聯刪除已在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="a46b6-285">If the entity has related data that should also be deleted, make sure that cascade delete is configured in the database.</span></span> <span data-ttu-id="a46b6-286">使用這個方法以刪除實體，EF 可能不了解有相關的實體被刪除。</span><span class="sxs-lookup"><span data-stu-id="a46b6-286">With this approach to entity deletion, EF might not realize there are related entities to be deleted.</span></span>

### <a name="update-the-delete-view"></a><span data-ttu-id="a46b6-287">更新刪除檢視</span><span class="sxs-lookup"><span data-stu-id="a46b6-287">Update the Delete view</span></span>

<span data-ttu-id="a46b6-288">在*Views/Student/Delete.cshtml*，新增 h2 標題之間 h3 標題、 錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="a46b6-288">In *Views/Student/Delete.cshtml*, add an error message between the h2 heading and the h3 heading, as shown in the following example:</span></span>

[!code-html[](intro/samples/cu/Views/Students/Delete.cshtml?range=7-9&highlight=2)]

<span data-ttu-id="a46b6-289">執行選取的頁面**學生** 索引標籤，然後按一下**刪除**超連結：</span><span class="sxs-lookup"><span data-stu-id="a46b6-289">Run the page by selecting the **Students** tab and clicking a **Delete** hyperlink:</span></span>

![刪除確認 頁面](crud/_static/student-delete.png)

<span data-ttu-id="a46b6-291">按一下**刪除**。</span><span class="sxs-lookup"><span data-stu-id="a46b6-291">Click **Delete**.</span></span> <span data-ttu-id="a46b6-292">索引頁會顯示不含已刪除的學生。</span><span class="sxs-lookup"><span data-stu-id="a46b6-292">The Index page is displayed without the deleted student.</span></span> <span data-ttu-id="a46b6-293">（您會看到錯誤處理並行教學課程中的動作中的程式碼的範例）。</span><span class="sxs-lookup"><span data-stu-id="a46b6-293">(You'll see an example of the error handling code in action in the concurrency tutorial.)</span></span>

## <a name="closing-database-connections"></a><span data-ttu-id="a46b6-294">關閉資料庫連接</span><span class="sxs-lookup"><span data-stu-id="a46b6-294">Closing database connections</span></span>

<span data-ttu-id="a46b6-295">若要釋放出保留的資料庫連接的資源，當您在使用它，則必須儘速處置內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-295">To free up the resources that a database connection holds, the context instance must be disposed as soon as possible when you are done with it.</span></span> <span data-ttu-id="a46b6-296">ASP.NET Core 內建[相依性插入](../../fundamentals/dependency-injection.md)會負責您該工作。</span><span class="sxs-lookup"><span data-stu-id="a46b6-296">The ASP.NET Core built-in [dependency injection](../../fundamentals/dependency-injection.md) takes care of that task for you.</span></span>

<span data-ttu-id="a46b6-297">在*Startup.cs*您呼叫[AddDbContext 擴充方法](https://github.com/aspnet/EntityFramework/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs)佈建`DbContext`ASP.NET DI 容器中的類別。</span><span class="sxs-lookup"><span data-stu-id="a46b6-297">In *Startup.cs* you call the [AddDbContext extension method](https://github.com/aspnet/EntityFramework/blob/03bcb5122e3f577a84498545fcf130ba79a3d987/src/Microsoft.EntityFrameworkCore/EntityFrameworkServiceCollectionExtensions.cs) to provision the `DbContext` class in the ASP.NET DI container.</span></span> <span data-ttu-id="a46b6-298">方法會設定為服務的存留期`Scoped`預設。</span><span class="sxs-lookup"><span data-stu-id="a46b6-298">That method sets the service lifetime to `Scoped` by default.</span></span> <span data-ttu-id="a46b6-299">`Scoped`表示內容物件存留期伴隨 web 要求存留時間，而`Dispose`方法將會自動呼叫 web 要求的結尾。</span><span class="sxs-lookup"><span data-stu-id="a46b6-299">`Scoped` means the context object lifetime coincides with the web request life time, and the `Dispose` method will be called automatically at the end of the web request.</span></span>

## <a name="handling-transactions"></a><span data-ttu-id="a46b6-300">處理交易</span><span class="sxs-lookup"><span data-stu-id="a46b6-300">Handling Transactions</span></span>

<span data-ttu-id="a46b6-301">根據預設 Entity Framework 會以隱含方式實作交易。</span><span class="sxs-lookup"><span data-stu-id="a46b6-301">By default the Entity Framework implicitly implements transactions.</span></span> <span data-ttu-id="a46b6-302">在案例中，您對多個資料列或資料表的變更，然後呼叫`SaveChanges`，Entity Framework 自動確保可能進行的所有變更成功，或完全失敗。</span><span class="sxs-lookup"><span data-stu-id="a46b6-302">In scenarios where you make changes to multiple rows or tables and then call `SaveChanges`, the Entity Framework automatically makes sure that either all of your changes succeed or they all fail.</span></span> <span data-ttu-id="a46b6-303">如果先完成某些變更，然後就會發生錯誤，這些變更會自動回復運作。</span><span class="sxs-lookup"><span data-stu-id="a46b6-303">If some changes are done first and then an error happens, those changes are automatically rolled back.</span></span> <span data-ttu-id="a46b6-304">如案例，您需要更大的控制權-例如，如果您想要包含在交易-外面 Entity Framework 執行的作業，請參閱[交易](https://docs.microsoft.com/ef/core/saving/transactions)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-304">For scenarios where you need more control -- for example, if you want to include operations done outside of Entity Framework in a transaction -- see [Transactions](https://docs.microsoft.com/ef/core/saving/transactions).</span></span>

## <a name="no-tracking-queries"></a><span data-ttu-id="a46b6-305">不追蹤查詢</span><span class="sxs-lookup"><span data-stu-id="a46b6-305">No-tracking queries</span></span>

<span data-ttu-id="a46b6-306">當資料庫內容中擷取資料表資料列，並建立代表的實體物件時，依預設它會追蹤的是否與資料庫中的實體記憶體中保持同步。</span><span class="sxs-lookup"><span data-stu-id="a46b6-306">When a database context retrieves table rows and creates entity objects that represent them, by default it keeps track of whether the entities in memory are in sync with what's in the database.</span></span> <span data-ttu-id="a46b6-307">記憶體中的資料做為快取，並更新實體時，會使用。</span><span class="sxs-lookup"><span data-stu-id="a46b6-307">The data in memory acts as a cache and is used when you update an entity.</span></span> <span data-ttu-id="a46b6-308">這種快取，所以通常不必要的 web 應用程式通常存留較短 （新的其中一個是建立及處置每個要求） 以及內容的內容執行個體讀取實體通常處置之前會再次使用該實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-308">This caching is often unnecessary in a web application because context instances are typically short-lived (a new one is created and disposed for each request) and the context that reads an entity is typically disposed before that entity is used again.</span></span>

<span data-ttu-id="a46b6-309">您可以藉由呼叫停用記憶體中的實體物件的追蹤`AsNoTracking`方法。</span><span class="sxs-lookup"><span data-stu-id="a46b6-309">You can disable tracking of entity objects in memory by calling the `AsNoTracking` method.</span></span> <span data-ttu-id="a46b6-310">您可以執行此作業的一般案例包括下列：</span><span class="sxs-lookup"><span data-stu-id="a46b6-310">Typical scenarios in which you might want to do that include the following:</span></span>

* <span data-ttu-id="a46b6-311">內容存留期間，您不需要更新任何實體，所以您無須以 EF[自動載入具有不同的查詢所擷取的實體的導覽屬性](read-related-data.md)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-311">During the context lifetime you don't need to update any entities, and you don't need EF to [automatically load navigation properties with  entities retrieved by separate queries](read-related-data.md).</span></span> <span data-ttu-id="a46b6-312">經常在控制器的 HttpGet 動作方法中符合這些條件。</span><span class="sxs-lookup"><span data-stu-id="a46b6-312">Frequently these conditions are met in a controller's HttpGet action methods.</span></span>

* <span data-ttu-id="a46b6-313">您正在執行的查詢會擷取大量資料，並將更新傳回的資料一小部分。</span><span class="sxs-lookup"><span data-stu-id="a46b6-313">You are running a query that retrieves a large volume of data, and only a small portion of the returned data will be updated.</span></span> <span data-ttu-id="a46b6-314">它可能會關閉追蹤大型查詢，並執行更新版本需要更新的幾個實體的查詢更有效率。</span><span class="sxs-lookup"><span data-stu-id="a46b6-314">It may be more efficient to turn off tracking for the large query, and run a query later for the few entities that need to be updated.</span></span>

* <span data-ttu-id="a46b6-315">您想要將實體附加以更新，但您稍早針對不同用途擷取相同的實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-315">You want to attach an entity in order to update it, but earlier you retrieved the same entity for a different purpose.</span></span> <span data-ttu-id="a46b6-316">因為實體已經受到追蹤的資料庫內容，您無法附加您想要變更的實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-316">Because the entity is already being tracked by the database context, you can't attach the entity that you want to change.</span></span> <span data-ttu-id="a46b6-317">若要處理這種情況的一種方式為呼叫`AsNoTracking`上前面的查詢。</span><span class="sxs-lookup"><span data-stu-id="a46b6-317">One way to handle this situation is to call `AsNoTracking` on the earlier query.</span></span>

<span data-ttu-id="a46b6-318">如需詳細資訊，請參閱[追蹤 vs。不追蹤](https://docs.microsoft.com/ef/core/querying/tracking)。</span><span class="sxs-lookup"><span data-stu-id="a46b6-318">For more information, see [Tracking vs. No-Tracking](https://docs.microsoft.com/ef/core/querying/tracking).</span></span>

## <a name="summary"></a><span data-ttu-id="a46b6-319">總結</span><span class="sxs-lookup"><span data-stu-id="a46b6-319">Summary</span></span>

<span data-ttu-id="a46b6-320">現在，您會有一組完整的頁面，執行簡單的 CRUD 作業的學生實體。</span><span class="sxs-lookup"><span data-stu-id="a46b6-320">You now have a complete set of pages that perform simple CRUD operations for Student entities.</span></span> <span data-ttu-id="a46b6-321">在下一個教學課程中，您將擴充的功能**索引**加入排序、 篩選和分頁的頁面。</span><span class="sxs-lookup"><span data-stu-id="a46b6-321">In the next tutorial you'll expand the functionality of the **Index** page by adding sorting, filtering, and paging.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="a46b6-322">[上一頁](intro.md)
[下一頁](sort-filter-page.md)</span><span class="sxs-lookup"><span data-stu-id="a46b6-322">[Previous](intro.md)
[Next](sort-filter-page.md)</span></span>  
