---
title: "使用 Entity Framework Core 8 的教學課程 1 的 razor 頁面"
author: rick-anderson
description: "示範如何建立使用 Entity Framework Core 的 Razor 網頁應用程式"
keywords: "ASP.NET Core，Entity Framework Core，教學課程"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: 98fe1b0c2dcf2e133d921b2cc8695bd2056c5ec0
ms.sourcegitcommit: a33737ea24e1ea9642e461d1bc90d6701f889436
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2017
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="d0712-104">開始使用 Razor 頁面與使用 Visual Studio (以 8 為 1) 的 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="d0712-104">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="d0712-105">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d0712-105">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="d0712-106">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework (EF) 核心 2.0 和 Visual Studio 2017 ASP.NET Core 2.0 MVC web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0712-106">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="d0712-107">範例應用程式是針對虛構的 Contoso 大學的網站。</span><span class="sxs-lookup"><span data-stu-id="d0712-107">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="d0712-108">其中包括功能，例如許可學生、 課程建立和講師指派。</span><span class="sxs-lookup"><span data-stu-id="d0712-108">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="d0712-109">此頁面是一系列的說明如何建立 Contoso 大學範例應用程式的教學課程中的第一個。</span><span class="sxs-lookup"><span data-stu-id="d0712-109">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="d0712-110">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0712-110">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="d0712-111">[下載指示](xref:tutorials/index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="d0712-111">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0712-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="d0712-112">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="d0712-113">疑難排解</span><span class="sxs-lookup"><span data-stu-id="d0712-113">Troubleshooting</span></span>

<span data-ttu-id="d0712-114">如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[完成階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)或[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final)。</span><span class="sxs-lookup"><span data-stu-id="d0712-114">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="d0712-115">如需常見的錯誤以及如何解決這些問題的清單，請參閱[數列中的最後一個教學課程疑難排解 > 一節](xref:data/ef-mvc/advanced#common-errors)。</span><span class="sxs-lookup"><span data-stu-id="d0712-115">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="d0712-116">如果您找不到您需要那里，您可以張貼問題的 StackOverflow.com [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)或[EF 核心](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="d0712-116">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="d0712-117">這一系列的教學課程是根據所完成的作業在先前的教學課程。</span><span class="sxs-lookup"><span data-stu-id="d0712-117">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="d0712-118">請考慮每個成功的教學課程完成後儲存專案的複本。</span><span class="sxs-lookup"><span data-stu-id="d0712-118">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="d0712-119">如果您遇到問題時，您可以從上一個教學課程，而不是要開始啟動一段。</span><span class="sxs-lookup"><span data-stu-id="d0712-119">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="d0712-120">或者，您可以下載[完成階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)然後再使用已完成的階段重新啟動。</span><span class="sxs-lookup"><span data-stu-id="d0712-120">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="d0712-121">Contoso 大學 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0712-121">The Contoso University web app</span></span>

<span data-ttu-id="d0712-122">這些教學課程中建立的應用程式是基本大學的網站。</span><span class="sxs-lookup"><span data-stu-id="d0712-122">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="d0712-123">使用者可以檢視和更新學生、 課程、 和講師資訊。</span><span class="sxs-lookup"><span data-stu-id="d0712-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="d0712-124">以下是幾個教學課程中所建立的螢幕。</span><span class="sxs-lookup"><span data-stu-id="d0712-124">Here are a few of the screens created in the tutorial.</span></span>

![學生索引頁](intro/_static/students-index.png)

![學生編輯頁面](intro/_static/student-edit.png)

<span data-ttu-id="d0712-127">此站台的 UI 樣式會接近內建的範本所產生的內容。</span><span class="sxs-lookup"><span data-stu-id="d0712-127">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="d0712-128">教學課程的重點是在 EF 核心 Razor 網頁，不包含 UI。</span><span class="sxs-lookup"><span data-stu-id="d0712-128">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="d0712-129">建立 Razor 頁面的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d0712-129">Create a Razor Pages web app</span></span>

* <span data-ttu-id="d0712-130">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="d0712-130">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="d0712-131">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0712-131">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="d0712-132">將專案命名**ContosoUniversity**。</span><span class="sxs-lookup"><span data-stu-id="d0712-132">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="d0712-133">請務必將專案命名*ContosoUniversity*使命名空間相符，當程式碼複製/貼上時。</span><span class="sxs-lookup"><span data-stu-id="d0712-133">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="d0712-134">![新增 ASP.NET Core Web 應用程式](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="d0712-134">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="d0712-135">在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="d0712-135">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="d0712-136">![Web 應用程式 (Razor 頁面)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="d0712-136">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="d0712-137">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="d0712-137">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="d0712-138">設定站台樣式</span><span class="sxs-lookup"><span data-stu-id="d0712-138">Set up the site style</span></span>

<span data-ttu-id="d0712-139">有一些變更設定的網站 功能表、 配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="d0712-139">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="d0712-140">開啟*Pages/_Layout.cshtml*並進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="d0712-140">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="d0712-141">將"ContosoUniversity"每次發生變更為"Contoso 大學。 」</span><span class="sxs-lookup"><span data-stu-id="d0712-141">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="d0712-142">有三個相符項目。</span><span class="sxs-lookup"><span data-stu-id="d0712-142">There are three occurrences.</span></span>

* <span data-ttu-id="d0712-143">加入功能表項目**學生**，**課程**，**講師**，和**部門**，並刪除**連絡人**功能表項目。</span><span class="sxs-lookup"><span data-stu-id="d0712-143">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="d0712-144">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="d0712-144">The changes are highlighted.</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="d0712-145">在*Pages/Index.cshtml*，ASP.NET MVC 的相關文字使用文字來取代此應用程式相關的下列程式碼取代檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="d0712-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="d0712-146">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="d0712-146">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="d0712-147">在首頁上會顯示包含下列教學課程中建立的索引標籤：</span><span class="sxs-lookup"><span data-stu-id="d0712-147">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="d0712-149">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="d0712-149">Create the data model</span></span>

<span data-ttu-id="d0712-150">建立 Contoso 大學應用程式的實體類別。</span><span class="sxs-lookup"><span data-stu-id="d0712-150">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="d0712-151">啟動具有下列三個實體：</span><span class="sxs-lookup"><span data-stu-id="d0712-151">Start with the following three entities:</span></span>

![課程註冊學生資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="d0712-153">一對多關聯性之間`Student`和`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-153">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="d0712-154">一對多關聯性之間`Course`和`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-154">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="d0712-155">一位學生可以註冊任何數目的課程。</span><span class="sxs-lookup"><span data-stu-id="d0712-155">A student can enroll in any number of courses.</span></span> <span data-ttu-id="d0712-156">課程可以有任意數目的學生在它註冊。</span><span class="sxs-lookup"><span data-stu-id="d0712-156">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="d0712-157">在下列章節中，建立這些實體的每個類別。</span><span class="sxs-lookup"><span data-stu-id="d0712-157">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="d0712-158">學生實體</span><span class="sxs-lookup"><span data-stu-id="d0712-158">The Student entity</span></span>

![學生實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="d0712-160">建立*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d0712-160">Create a *Models* folder.</span></span> <span data-ttu-id="d0712-161">在*模型*資料夾中，建立名為的類別檔案*Student.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d0712-161">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="d0712-162">`ID`屬性會對應至這個類別的資料庫 (DB) 資料表的主索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="d0712-162">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="d0712-163">根據預設，EF 核心會解譯此屬性，名為`ID`或`classnameID`為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d0712-163">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="d0712-164">`Enrollments`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="d0712-164">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d0712-165">導覽屬性連結到此實體與相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-165">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="d0712-166">在此情況下，`Enrollments`屬性`Student entity`保存所有的`Enrollment`的實體有關的`Student`。</span><span class="sxs-lookup"><span data-stu-id="d0712-166">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="d0712-167">例如，如果 DB 中的學生資料列有兩個相關註冊資料列，`Enrollments`導覽屬性包含這兩者`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-167">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="d0712-168">相關`Enrollment`資料列是包含該學生的主索引鍵的值中的資料列`StudentID`資料行。</span><span class="sxs-lookup"><span data-stu-id="d0712-168">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="d0712-169">例如，假設學生識別碼 = 1 具有兩個資料列`Enrollment`資料表。</span><span class="sxs-lookup"><span data-stu-id="d0712-169">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="d0712-170">`Enrollment`資料表包含兩個資料列`StudentID`= 1。</span><span class="sxs-lookup"><span data-stu-id="d0712-170">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="d0712-171">`StudentID`為外部索引鍵中`Enrollment`資料表中，指定在學生`Student`資料表。</span><span class="sxs-lookup"><span data-stu-id="d0712-171">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="d0712-172">如果導覽屬性都可以保存多個實體，瀏覽屬性必須是清單型別，例如`ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="d0712-172">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="d0712-173">`ICollection<T>`您可以指定或型別，例如`List<T>`或`HashSet<T>`。</span><span class="sxs-lookup"><span data-stu-id="d0712-173">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="d0712-174">當`ICollection<T>`是使用 EF 核心建立`HashSet<T>`預設集合。</span><span class="sxs-lookup"><span data-stu-id="d0712-174">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="d0712-175">保存多個實體的導覽屬性是來自多對多 」 和 「 一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="d0712-175">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="d0712-176">註冊實體</span><span class="sxs-lookup"><span data-stu-id="d0712-176">The Enrollment entity</span></span>

![註冊的實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="d0712-178">在*模型*資料夾中，建立*Enrollment.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d0712-178">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="d0712-179">`EnrollmentID`屬性是主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d0712-179">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="d0712-180">此實體會使用`classnameID`模式而不是`ID`像`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-180">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="d0712-181">通常開發人員會選擇其中一個模式，並將它整個資料模型。</span><span class="sxs-lookup"><span data-stu-id="d0712-181">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="d0712-182">在稍後的教學課程中，使用識別碼，而類別名稱不會顯示若要簡化資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="d0712-182">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="d0712-183">`Grade`屬性是`enum`。</span><span class="sxs-lookup"><span data-stu-id="d0712-183">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="d0712-184">問號之後`Grade`型別宣告表示`Grade`屬性可為 null。</span><span class="sxs-lookup"><span data-stu-id="d0712-184">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="d0712-185">為 null 的等級是不同於零等級--null 表示等級不未知或尚未被指派。</span><span class="sxs-lookup"><span data-stu-id="d0712-185">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="d0712-186">`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。</span><span class="sxs-lookup"><span data-stu-id="d0712-186">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="d0712-187">`Enrollment`實體都與一個`Student`實體，因此該屬性包含單一`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-187">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="d0712-188">`Student`實體與`Student.Enrollments`導覽屬性，其中包含多個`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-188">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="d0712-189">`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。</span><span class="sxs-lookup"><span data-stu-id="d0712-189">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="d0712-190">`Enrollment`實體都與一個`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-190">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="d0712-191">EF 核心會解譯為外部索引鍵屬性如果名稱為`<navigation property name><primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="d0712-191">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="d0712-192">例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`。</span><span class="sxs-lookup"><span data-stu-id="d0712-192">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="d0712-193">外部索引鍵屬性也名為`<primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="d0712-193">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="d0712-194">例如，`CourseID`因為`Course`實體的主索引鍵是`CourseID`。</span><span class="sxs-lookup"><span data-stu-id="d0712-194">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="d0712-195">課程實體</span><span class="sxs-lookup"><span data-stu-id="d0712-195">The Course entity</span></span>

![課程實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="d0712-197">在*模型*資料夾中，建立*Course.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d0712-197">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="d0712-198">`Enrollments`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="d0712-198">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="d0712-199">A`Course`可以與任意數目的相關實體`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-199">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="d0712-200">`DatabaseGenerated`屬性可讓應用程式，以指定的主索引鍵，而不是具有 DB 產生它。</span><span class="sxs-lookup"><span data-stu-id="d0712-200">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="d0712-201">建立 SchoolContext DB 內容</span><span class="sxs-lookup"><span data-stu-id="d0712-201">Create the SchoolContext DB context</span></span>

<span data-ttu-id="d0712-202">協調 EF 核心功能，為給定的資料模型的主要類別是 DB 內容類別。</span><span class="sxs-lookup"><span data-stu-id="d0712-202">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="d0712-203">資料內容衍生自`Microsoft.EntityFrameworkCore.DbContext`。</span><span class="sxs-lookup"><span data-stu-id="d0712-203">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="d0712-204">資料內容會指定資料模型中包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-204">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="d0712-205">在此專案中，類別會命名為`SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="d0712-205">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="d0712-206">在專案資料夾中，建立名為資料夾*資料*。</span><span class="sxs-lookup"><span data-stu-id="d0712-206">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="d0712-207">在*資料*資料夾建立*SchoolContext.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d0712-207">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="d0712-208">此程式碼建立`DbSet`每一個實體集的屬性。</span><span class="sxs-lookup"><span data-stu-id="d0712-208">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="d0712-209">在 EF 核心術語：</span><span class="sxs-lookup"><span data-stu-id="d0712-209">In EF Core terminology:</span></span>

* <span data-ttu-id="d0712-210">實體集合通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="d0712-210">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="d0712-211">實體對應到資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="d0712-211">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="d0712-212">`DbSet<Enrollment>`和`DbSet<Course>`可以省略。</span><span class="sxs-lookup"><span data-stu-id="d0712-212">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="d0712-213">EF 核心包含這些隱含因為`Student`實體參考`Enrollment`實體，而`Enrollment`實體參考`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="d0712-213">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="d0712-214">本教學課程中，保留`DbSet<Enrollment>`和`DbSet<Course>`中`SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="d0712-214">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="d0712-215">EF 核心建立資料庫時，會建立具有相同名稱的資料表`DbSet`屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="d0712-215">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="d0712-216">集合的屬性名稱都是通常複數 （學生而不是學生）。</span><span class="sxs-lookup"><span data-stu-id="d0712-216">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="d0712-217">開發人員不同意有關資料表名稱是否應該使用複數。</span><span class="sxs-lookup"><span data-stu-id="d0712-217">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="d0712-218">如需這些教學課程中，預設行為會覆寫指定的 DbContext 單數資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="d0712-218">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="d0712-219">若要指定單一的資料表名稱，加入下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="d0712-219">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="d0712-220">暫存器具有相依性插入的內容</span><span class="sxs-lookup"><span data-stu-id="d0712-220">Register the context with dependency injection</span></span>

<span data-ttu-id="d0712-221">包含 ASP.NET Core[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="d0712-221">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="d0712-222">在應用程式啟動時的相依性插入會註冊服務 （例如 EF 核心 DB 內容）。</span><span class="sxs-lookup"><span data-stu-id="d0712-222">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="d0712-223">需要這些服務 （例如 Razor 頁面） 的元件會提供這些服務，透過建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="d0712-223">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="d0712-224">取得 db 內容執行個體的建構函式程式碼是稍後在本教學課程中所示。</span><span class="sxs-lookup"><span data-stu-id="d0712-224">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="d0712-225">若要註冊`SchoolContext`為服務時，開啟*Startup.cs*，並加入要反白顯示的行`ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-225">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="d0712-226">連接字串的名稱會傳遞至內容所呼叫的方法上`DbContextOptionsBuilder`物件。</span><span class="sxs-lookup"><span data-stu-id="d0712-226">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="d0712-227">本機開發， [ASP.NET Core 組態系統](xref:fundamentals/configuration/index)讀取連接字串從*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="d0712-227">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="d0712-228">新增`using`陳述式`ContosoUniversity.Data`和`Microsoft.EntityFrameworkCore`命名空間。</span><span class="sxs-lookup"><span data-stu-id="d0712-228">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="d0712-229">建置專案。</span><span class="sxs-lookup"><span data-stu-id="d0712-229">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="d0712-230">開啟*appsettings.json*檔案，然後加入連接字串，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="d0712-230">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="d0712-231">上述的連接字串使用`ConnectRetryCount=0`防止[SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework)從停滯。</span><span class="sxs-lookup"><span data-stu-id="d0712-231">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="d0712-232">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="d0712-232">SQL Server Express LocalDB</span></span>

<span data-ttu-id="d0712-233">連接字串會指定 SQL Server LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-233">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="d0712-234">LocalDB 是輕量版 SQL Server Express Database Engine 和適用於應用程式開發，而不是生產環境使用。</span><span class="sxs-lookup"><span data-stu-id="d0712-234">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="d0712-235">LocalDB 會視需要啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="d0712-235">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="d0712-236">根據預設，建立 LocalDB *.mdf* DB 中的檔案`C:/Users/<user>`目錄。</span><span class="sxs-lookup"><span data-stu-id="d0712-236">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="d0712-237">加入程式碼以初始化測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="d0712-237">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="d0712-238">EF 核心會建立空的資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-238">EF Core creates an empty DB.</span></span> <span data-ttu-id="d0712-239">在本節中，*種子*會寫入測試的資料填入的方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-239">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="d0712-240">在*資料*資料夾中，建立新的類別檔案命名為*DbInitializer.cs*並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d0712-240">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="d0712-241">程式碼會檢查是否有任何學生 DB 中。</span><span class="sxs-lookup"><span data-stu-id="d0712-241">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="d0712-242">如果在資料庫中有沒有學生，測試資料植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-242">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="d0712-243">它將測試資料載入至陣列而非`List<T>`集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="d0712-243">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="d0712-244">`EnsureCreated`方法會自動建立的 DB DB 內容。</span><span class="sxs-lookup"><span data-stu-id="d0712-244">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="d0712-245">若 DB 存在，則`EnsureCreated`傳回而不需修改資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-245">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="d0712-246">在*Program.cs*，修改`Main`方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="d0712-246">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="d0712-247">從相依性插入容器中取得 DB 內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="d0712-247">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="d0712-248">呼叫種子方法，將內容傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="d0712-248">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="d0712-249">Seed 方法完成時處置內容。</span><span class="sxs-lookup"><span data-stu-id="d0712-249">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="d0712-250">下列程式碼將示範更新*Program.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="d0712-250">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="d0712-251">第一次執行應用程式時，資料庫會建立並植入的測試資料。</span><span class="sxs-lookup"><span data-stu-id="d0712-251">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="d0712-252">更新資料模型時：</span><span class="sxs-lookup"><span data-stu-id="d0712-252">When the data model is updated:</span></span>
* <span data-ttu-id="d0712-253">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-253">Delete the DB.</span></span>
* <span data-ttu-id="d0712-254">更新 seed 方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-254">Update the seed method.</span></span>
* <span data-ttu-id="d0712-255">執行應用程式，並建立新的植入的資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-255">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="d0712-256">在之後的教學課程中的資料模型變更，而不需要刪除並重新建立資料庫時，會更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-256">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="d0712-257">加入 scaffold 工具</span><span class="sxs-lookup"><span data-stu-id="d0712-257">Add scaffold tooling</span></span>

<span data-ttu-id="d0712-258">在本節中，封裝管理員主控台 (PMC) 用來新增 Visual Studio web 程式碼產生封裝。</span><span class="sxs-lookup"><span data-stu-id="d0712-258">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="d0712-259">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="d0712-259">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="d0712-260">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="d0712-260">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="d0712-261">在 封裝管理員主控台 (PMC)，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="d0712-261">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="d0712-262">前一個命令會將 *.csproj 檔案中的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="d0712-262">The previous command adds the NuGet packages to the *.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="d0712-263">Scaffold 模型</span><span class="sxs-lookup"><span data-stu-id="d0712-263">Scaffold the model</span></span>

* <span data-ttu-id="d0712-264">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="d0712-264">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="d0712-265">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="d0712-265">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="d0712-266">如果會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="d0712-266">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="d0712-267">重新執行命令，並將在頁面底部的註解。</span><span class="sxs-lookup"><span data-stu-id="d0712-267">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="d0712-268">建置專案。</span><span class="sxs-lookup"><span data-stu-id="d0712-268">Build the project.</span></span> <span data-ttu-id="d0712-269">建置會產生類似如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="d0712-269">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="d0712-270">全域變更`_context.Student`至`_context.Students`(亦即，加入"s" `Student`)。</span><span class="sxs-lookup"><span data-stu-id="d0712-270">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="d0712-271">找到項目 7 及更新。</span><span class="sxs-lookup"><span data-stu-id="d0712-271">7 occurrences are found and updated.</span></span> <span data-ttu-id="d0712-272">我們希望修正[這個 bug](https://github.com/aspnet/Scaffolding/issues/633)的下一個版本。</span><span class="sxs-lookup"><span data-stu-id="d0712-272">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="d0712-273">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="d0712-273">Test the app</span></span>

<span data-ttu-id="d0712-274">執行應用程式並選取**學生**連結。</span><span class="sxs-lookup"><span data-stu-id="d0712-274">Run the app and select the **Students** link.</span></span> <span data-ttu-id="d0712-275">根據瀏覽器寬度，**學生**連結出現在頁面頂端。</span><span class="sxs-lookup"><span data-stu-id="d0712-275">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="d0712-276">如果**學生**連結不可見，請按一下右上角的 [導覽] 圖示。</span><span class="sxs-lookup"><span data-stu-id="d0712-276">If the **Students** link is not visible, click the navigation icon in the upper right corner.</span></span>

![Contoso 大學首頁窄](intro/_static/home-page-narrow.png)

<span data-ttu-id="d0712-278">測試**建立**，**編輯**，和**詳細資料**連結。</span><span class="sxs-lookup"><span data-stu-id="d0712-278">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="d0712-279">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="d0712-279">View the DB</span></span>

<span data-ttu-id="d0712-280">應用程式啟動時，`DbInitializer.Initialize`呼叫`EnsureCreated`。</span><span class="sxs-lookup"><span data-stu-id="d0712-280">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="d0712-281">`EnsureCreated`如果資料庫存在，而且若有需要，請建立一個偵測。</span><span class="sxs-lookup"><span data-stu-id="d0712-281">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="d0712-282">如果在 DB 中，沒有任何學生`Initialize`方法會將學生。</span><span class="sxs-lookup"><span data-stu-id="d0712-282">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="d0712-283">開啟**SQL Server 物件總管**(SSOX) 從**檢視**Visual Studio 中的功能表。</span><span class="sxs-lookup"><span data-stu-id="d0712-283">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="d0712-284">在 SSOX，按一下  **(localdb) \MSSQLLocalDB > 資料庫 > ContosoUniversity1**。</span><span class="sxs-lookup"><span data-stu-id="d0712-284">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="d0712-285">展開**資料表**節點。</span><span class="sxs-lookup"><span data-stu-id="d0712-285">Expand the **Tables** node.</span></span>

<span data-ttu-id="d0712-286">以滑鼠右鍵按一下**學生**資料表，並按一下**檢視資料**若要查看建立的資料行和資料列插入資料表。</span><span class="sxs-lookup"><span data-stu-id="d0712-286">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="d0712-287">*.Mdf*和*.ldf* DB 檔案位於*C:\Users\\ <yourusername>* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d0712-287">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="d0712-288">`EnsureCreated`啟動應用程式，可讓下列工作流程上呼叫：</span><span class="sxs-lookup"><span data-stu-id="d0712-288">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="d0712-289">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="d0712-289">Delete the DB.</span></span>
* <span data-ttu-id="d0712-290">變更資料庫結構描述 (例如，加入`EmailAddress`欄位)。</span><span class="sxs-lookup"><span data-stu-id="d0712-290">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="d0712-291">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="d0712-291">Run the app.</span></span>

<span data-ttu-id="d0712-292">`EnsureCreated`建立與 DB`EmailAddress`資料行。</span><span class="sxs-lookup"><span data-stu-id="d0712-292">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="d0712-293">慣例</span><span class="sxs-lookup"><span data-stu-id="d0712-293">Conventions</span></span>

<span data-ttu-id="d0712-294">若要建立完整資料庫的 EF 核心順序撰寫的程式碼數量是最小因為使用的慣例或讓 EF 核心的假設。</span><span class="sxs-lookup"><span data-stu-id="d0712-294">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="d0712-295">名稱`DbSet`屬性會用做資料表的名稱。</span><span class="sxs-lookup"><span data-stu-id="d0712-295">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="d0712-296">實體沒有參考`DbSet`屬性，實體類別名稱會當做資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="d0712-296">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="d0712-297">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="d0712-297">Entity property names are used for column names.</span></span>

* <span data-ttu-id="d0712-298">實體屬性會在名為 ID 或 classnameID 辨識為主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="d0712-298">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="d0712-299">屬性會解譯為外部索引鍵屬性上，如果名稱為 *<navigation property name> <primary key property name>*  (例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`).</span><span class="sxs-lookup"><span data-stu-id="d0712-299">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="d0712-300">外部索引鍵屬性都可以具名 *<primary key property name>*  (例如，`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="d0712-300">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="d0712-301">傳統行為可以被覆寫。</span><span class="sxs-lookup"><span data-stu-id="d0712-301">Conventional behavior can be overridden.</span></span> <span data-ttu-id="d0712-302">例如，資料表名稱可以明確指定，如稍早在本教學課程中所示。</span><span class="sxs-lookup"><span data-stu-id="d0712-302">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="d0712-303">可以明確設定資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="d0712-303">The column names can be explicitly set.</span></span> <span data-ttu-id="d0712-304">主索引鍵和外部索引鍵可以明確設定。</span><span class="sxs-lookup"><span data-stu-id="d0712-304">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="d0712-305">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="d0712-305">Asynchronous code</span></span>

<span data-ttu-id="d0712-306">非同步程式設計是預設的 ASP.NET Core 和 EF 核心模式。</span><span class="sxs-lookup"><span data-stu-id="d0712-306">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="d0712-307">Web 伺服器的有限的數目的執行緒可用，而且在高負載情況下的所有可用的執行緒可能正在使用中。</span><span class="sxs-lookup"><span data-stu-id="d0712-307">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="d0712-308">當發生這種情況時，伺服器無法處理新的要求，直到執行緒釋放。</span><span class="sxs-lookup"><span data-stu-id="d0712-308">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="d0712-309">使用同步程式碼，許多執行緒可能會將繫結起來雖然它們實際上並不執行任何工作，因為他們正在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="d0712-309">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="d0712-310">使用非同步程式碼，當處理程序在等候 I/O 完成，它的執行緒釋放針對伺服器將用於處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="d0712-310">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="d0712-311">如此一來，非同步程式碼可讓更有效率地使用伺服器資源，而且伺服器已啟用以處理更多流量不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="d0712-311">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="d0712-312">非同步程式碼會在執行階段導致少量的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="d0712-312">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="d0712-313">低流量的情況下，效能的衝擊，微不足道時的高流量的情況下，可能很大的潛在的效能改善。</span><span class="sxs-lookup"><span data-stu-id="d0712-313">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="d0712-314">下列程式碼，`async`關鍵字，`Task<T>`傳回值，`await`關鍵字和`ToListAsync`方法提供程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="d0712-314">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="d0712-315">`async`關鍵字會指示編譯器將：</span><span class="sxs-lookup"><span data-stu-id="d0712-315">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="d0712-316">產生的組件的方法主體的回呼。</span><span class="sxs-lookup"><span data-stu-id="d0712-316">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="d0712-317">自動建立[工作](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7)傳回物件。</span><span class="sxs-lookup"><span data-stu-id="d0712-317">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that is returned.</span></span> <span data-ttu-id="d0712-318">如需詳細資訊，請參閱[工作傳回型別](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。</span><span class="sxs-lookup"><span data-stu-id="d0712-318">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="d0712-319">隱含傳回型別`Task`代表進行中的工作。</span><span class="sxs-lookup"><span data-stu-id="d0712-319">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="d0712-320">`await`關鍵字會導致編譯器分成兩個部分的方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-320">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="d0712-321">以非同步方式啟動的作業結束的第一個部分。</span><span class="sxs-lookup"><span data-stu-id="d0712-321">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="d0712-322">第二個部分會放入作業完成時呼叫的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-322">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="d0712-323">`ToListAsync`非同步版本`ToList`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-323">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="d0712-324">若要撰寫使用 EF 核心的非同步程式碼時應該注意的事項：</span><span class="sxs-lookup"><span data-stu-id="d0712-324">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="d0712-325">只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="d0712-325">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="d0712-326">包含`ToListAsync`， `SingleOrDefaultAsync`， `FirstOrDefaultAsync`，和`SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="d0712-326">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="d0712-327">它不包含只變更陳述式`IQueryable`，例如`var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="d0712-327">It does not include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="d0712-328">EF 核心內容不執行緒安全： 不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="d0712-328">An EF Core context is not threaded safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="d0712-329">若要利用非同步程式碼的效能優勢，確認程式庫封裝 （例如分頁） 使用非同步呼叫查詢傳送至資料庫的 EF 核心方法。</span><span class="sxs-lookup"><span data-stu-id="d0712-329">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="d0712-330">如需在.NET 非同步程式設計的詳細資訊，請參閱[非同步概觀](https://docs.microsoft.com/dotnet/articles/standard/async)。</span><span class="sxs-lookup"><span data-stu-id="d0712-330">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="d0712-331">在下一個教學課程中，基本 CRUD （建立、 讀取、 更新、 刪除） 作業會檢查。</span><span class="sxs-lookup"><span data-stu-id="d0712-331">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="d0712-332">下一步</span><span class="sxs-lookup"><span data-stu-id="d0712-332">Next</span></span>](xref:data/ef-rp/crud)
