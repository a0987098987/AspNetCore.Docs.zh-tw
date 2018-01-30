---
title: "使用 Entity Framework Core 8 的教學課程 1 的 razor 頁面"
author: rick-anderson
description: "示範如何建立使用 Entity Framework Core 的 Razor 網頁應用程式"
manager: wpickett
ms.author: riande
ms.date: 11/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-rp/intro
ms.openlocfilehash: 091f34da347d52ba8e3e87779ddc4aeb790c2800
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a><span data-ttu-id="7e903-103">開始使用 Razor 頁面與使用 Visual Studio (以 8 為 1) 的 Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="7e903-103">Getting started with Razor Pages and Entity Framework Core using Visual Studio (1 of 8)</span></span>

<span data-ttu-id="7e903-104">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7e903-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7e903-105">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework (EF) 核心 2.0 和 Visual Studio 2017 ASP.NET Core 2.0 MVC web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e903-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="7e903-106">範例應用程式是針對虛構的 Contoso 大學的網站。</span><span class="sxs-lookup"><span data-stu-id="7e903-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="7e903-107">其中包括功能，例如許可學生、 課程建立和講師指派。</span><span class="sxs-lookup"><span data-stu-id="7e903-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="7e903-108">此頁面是一系列的說明如何建立 Contoso 大學範例應用程式的教學課程中的第一個。</span><span class="sxs-lookup"><span data-stu-id="7e903-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="7e903-109">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e903-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="7e903-110">[下載指示](xref:tutorials/index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="7e903-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e903-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="7e903-111">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

<span data-ttu-id="7e903-112">熟悉[Razor 頁面](xref:mvc/razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="7e903-112">Familiarity with [Razor Pages](xref:mvc/razor-pages/index).</span></span> <span data-ttu-id="7e903-113">新的程式設計人員應該完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)再開始此數列。</span><span class="sxs-lookup"><span data-stu-id="7e903-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7e903-114">疑難排解</span><span class="sxs-lookup"><span data-stu-id="7e903-114">Troubleshooting</span></span>

<span data-ttu-id="7e903-115">如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[完成階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)或[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final)。</span><span class="sxs-lookup"><span data-stu-id="7e903-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) or [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final).</span></span> <span data-ttu-id="7e903-116">如需常見的錯誤以及如何解決這些問題的清單，請參閱[數列中的最後一個教學課程疑難排解 > 一節](xref:data/ef-mvc/advanced#common-errors)。</span><span class="sxs-lookup"><span data-stu-id="7e903-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="7e903-117">如果您找不到您需要那里，您可以將問題張貼至[StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core)如[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)或[EF 核心](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="7e903-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="7e903-118">這一系列的教學課程是根據所完成的作業在先前的教學課程。</span><span class="sxs-lookup"><span data-stu-id="7e903-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="7e903-119">請考慮每個成功的教學課程完成後儲存專案的複本。</span><span class="sxs-lookup"><span data-stu-id="7e903-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="7e903-120">如果您遇到問題時，您可以從上一個教學課程，而不是要開始啟動一段。</span><span class="sxs-lookup"><span data-stu-id="7e903-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="7e903-121">或者，您可以下載[完成階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)然後再使用已完成的階段重新啟動。</span><span class="sxs-lookup"><span data-stu-id="7e903-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="7e903-122">Contoso 大學 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7e903-122">The Contoso University web app</span></span>

<span data-ttu-id="7e903-123">這些教學課程中建立的應用程式是基本大學的網站。</span><span class="sxs-lookup"><span data-stu-id="7e903-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="7e903-124">使用者可以檢視和更新學生、 課程、 和講師資訊。</span><span class="sxs-lookup"><span data-stu-id="7e903-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="7e903-125">以下是幾個教學課程中所建立的螢幕。</span><span class="sxs-lookup"><span data-stu-id="7e903-125">Here are a few of the screens created in the tutorial.</span></span>

![學生索引頁](intro/_static/students-index.png)

![學生編輯頁面](intro/_static/student-edit.png)

<span data-ttu-id="7e903-128">此站台的 UI 樣式會接近內建的範本所產生的內容。</span><span class="sxs-lookup"><span data-stu-id="7e903-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="7e903-129">教學課程的重點是在 EF 核心 Razor 網頁，不包含 UI。</span><span class="sxs-lookup"><span data-stu-id="7e903-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="7e903-130">建立 Razor 頁面的 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7e903-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="7e903-131">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="7e903-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="7e903-132">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e903-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="7e903-133">將專案命名**ContosoUniversity**。</span><span class="sxs-lookup"><span data-stu-id="7e903-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="7e903-134">請務必將專案命名*ContosoUniversity*使命名空間相符，當程式碼複製/貼上時。</span><span class="sxs-lookup"><span data-stu-id="7e903-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="7e903-135">![新增 ASP.NET Core Web 應用程式](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="7e903-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="7e903-136">在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="7e903-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="7e903-137">![Web 應用程式 (Razor 頁面)](../../mvc/razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="7e903-137">![Web Application (Razor Pages)](../../mvc/razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="7e903-138">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="7e903-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="7e903-139">設定站台樣式</span><span class="sxs-lookup"><span data-stu-id="7e903-139">Set up the site style</span></span>

<span data-ttu-id="7e903-140">有一些變更設定的網站] 功能表、 配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="7e903-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="7e903-141">開啟*Pages/_Layout.cshtml*並進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="7e903-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="7e903-142">將"ContosoUniversity"每次發生變更為"Contoso 大學。 」</span><span class="sxs-lookup"><span data-stu-id="7e903-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="7e903-143">有三個相符項目。</span><span class="sxs-lookup"><span data-stu-id="7e903-143">There are three occurrences.</span></span>

* <span data-ttu-id="7e903-144">加入功能表項目**學生**，**課程**，**講師**，和**部門**，並刪除**連絡人**功能表項目。</span><span class="sxs-lookup"><span data-stu-id="7e903-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="7e903-145">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="7e903-145">The changes are highlighted.</span></span> <span data-ttu-id="7e903-146">(所有標記都為*不*顯示。)</span><span class="sxs-lookup"><span data-stu-id="7e903-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="7e903-147">在*Pages/Index.cshtml*，ASP.NET MVC 的相關文字使用文字來取代此應用程式相關的下列程式碼取代檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="7e903-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="7e903-148">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="7e903-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="7e903-149">在首頁上會顯示包含下列教學課程中建立的索引標籤：</span><span class="sxs-lookup"><span data-stu-id="7e903-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="7e903-151">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="7e903-151">Create the data model</span></span>

<span data-ttu-id="7e903-152">建立 Contoso 大學應用程式的實體類別。</span><span class="sxs-lookup"><span data-stu-id="7e903-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="7e903-153">啟動具有下列三個實體：</span><span class="sxs-lookup"><span data-stu-id="7e903-153">Start with the following three entities:</span></span>

![課程註冊學生資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="7e903-155">一對多關聯性之間`Student`和`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="7e903-156">一對多關聯性之間`Course`和`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="7e903-157">一位學生可以註冊任何數目的課程。</span><span class="sxs-lookup"><span data-stu-id="7e903-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="7e903-158">課程可以有任意數目的學生在它註冊。</span><span class="sxs-lookup"><span data-stu-id="7e903-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="7e903-159">在下列章節中，建立這些實體的每個類別。</span><span class="sxs-lookup"><span data-stu-id="7e903-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="7e903-160">學生實體</span><span class="sxs-lookup"><span data-stu-id="7e903-160">The Student entity</span></span>

![學生實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="7e903-162">建立*模型*資料夾。</span><span class="sxs-lookup"><span data-stu-id="7e903-162">Create a *Models* folder.</span></span> <span data-ttu-id="7e903-163">在*模型*資料夾中，建立名為的類別檔案*Student.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7e903-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="7e903-164">`ID`屬性會對應至這個類別的資料庫 (DB) 資料表的主索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="7e903-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="7e903-165">根據預設，EF 核心會解譯此屬性，名為`ID`或`classnameID`為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="7e903-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="7e903-166">`Enrollments`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="7e903-166">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7e903-167">導覽屬性連結到此實體與相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-167">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="7e903-168">在此情況下，`Enrollments`屬性`Student entity`保存所有的`Enrollment`的實體有關的`Student`。</span><span class="sxs-lookup"><span data-stu-id="7e903-168">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="7e903-169">例如，如果 DB 中的學生資料列有兩個相關註冊資料列，`Enrollments`導覽屬性包含這兩者`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-169">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="7e903-170">相關`Enrollment`資料列是包含該學生的主索引鍵的值中的資料列`StudentID`資料行。</span><span class="sxs-lookup"><span data-stu-id="7e903-170">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="7e903-171">例如，假設學生識別碼 = 1 具有兩個資料列`Enrollment`資料表。</span><span class="sxs-lookup"><span data-stu-id="7e903-171">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="7e903-172">`Enrollment`資料表包含兩個資料列`StudentID`= 1。</span><span class="sxs-lookup"><span data-stu-id="7e903-172">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="7e903-173">`StudentID`為外部索引鍵中`Enrollment`資料表中，指定在學生`Student`資料表。</span><span class="sxs-lookup"><span data-stu-id="7e903-173">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="7e903-174">如果導覽屬性都可以保存多個實體，瀏覽屬性必須是清單型別，例如`ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="7e903-174">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="7e903-175">`ICollection<T>`您可以指定或型別，例如`List<T>`或`HashSet<T>`。</span><span class="sxs-lookup"><span data-stu-id="7e903-175">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="7e903-176">當`ICollection<T>`是使用 EF 核心建立`HashSet<T>`預設集合。</span><span class="sxs-lookup"><span data-stu-id="7e903-176">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="7e903-177">保存多個實體的導覽屬性是來自多對多 」 和 「 一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="7e903-177">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="7e903-178">註冊實體</span><span class="sxs-lookup"><span data-stu-id="7e903-178">The Enrollment entity</span></span>

![註冊的實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="7e903-180">在*模型*資料夾中，建立*Enrollment.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7e903-180">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="7e903-181">`EnrollmentID`屬性是主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="7e903-181">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="7e903-182">此實體會使用`classnameID`模式而不是`ID`像`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-182">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="7e903-183">通常開發人員會選擇其中一個模式，並將它整個資料模型。</span><span class="sxs-lookup"><span data-stu-id="7e903-183">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="7e903-184">在稍後的教學課程中，使用識別碼，而類別名稱不會顯示若要簡化資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="7e903-184">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="7e903-185">`Grade`屬性是`enum`。</span><span class="sxs-lookup"><span data-stu-id="7e903-185">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="7e903-186">問號之後`Grade`型別宣告表示`Grade`屬性可為 null。</span><span class="sxs-lookup"><span data-stu-id="7e903-186">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="7e903-187">為 null 的等級是不同於零等級--null 表示等級不未知或尚未被指派。</span><span class="sxs-lookup"><span data-stu-id="7e903-187">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="7e903-188">`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。</span><span class="sxs-lookup"><span data-stu-id="7e903-188">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="7e903-189">`Enrollment`實體都與一個`Student`實體，因此該屬性包含單一`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-189">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="7e903-190">`Student`實體與`Student.Enrollments`導覽屬性，其中包含多個`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-190">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="7e903-191">`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。</span><span class="sxs-lookup"><span data-stu-id="7e903-191">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="7e903-192">`Enrollment`實體都與一個`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-192">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="7e903-193">EF 核心會解譯為外部索引鍵屬性如果名稱為`<navigation property name><primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="7e903-193">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="7e903-194">例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`。</span><span class="sxs-lookup"><span data-stu-id="7e903-194">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="7e903-195">外部索引鍵屬性也名為`<primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="7e903-195">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="7e903-196">例如，`CourseID`因為`Course`實體的主索引鍵是`CourseID`。</span><span class="sxs-lookup"><span data-stu-id="7e903-196">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="7e903-197">課程實體</span><span class="sxs-lookup"><span data-stu-id="7e903-197">The Course entity</span></span>

![課程實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="7e903-199">在*模型*資料夾中，建立*Course.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7e903-199">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="7e903-200">`Enrollments`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="7e903-200">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="7e903-201">A`Course`可以與任意數目的相關實體`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-201">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="7e903-202">`DatabaseGenerated`屬性可讓應用程式，以指定的主索引鍵，而不是具有 DB 產生它。</span><span class="sxs-lookup"><span data-stu-id="7e903-202">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="7e903-203">建立 SchoolContext DB 內容</span><span class="sxs-lookup"><span data-stu-id="7e903-203">Create the SchoolContext DB context</span></span>

<span data-ttu-id="7e903-204">協調 EF 核心功能，為給定的資料模型的主要類別是 DB 內容類別。</span><span class="sxs-lookup"><span data-stu-id="7e903-204">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="7e903-205">資料內容衍生自`Microsoft.EntityFrameworkCore.DbContext`。</span><span class="sxs-lookup"><span data-stu-id="7e903-205">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="7e903-206">資料內容會指定資料模型中包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-206">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="7e903-207">在此專案中，類別會命名為`SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="7e903-207">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="7e903-208">在專案資料夾中，建立名為資料夾*資料*。</span><span class="sxs-lookup"><span data-stu-id="7e903-208">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="7e903-209">在*資料*資料夾建立*SchoolContext.cs*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7e903-209">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="7e903-210">此程式碼建立`DbSet`每一個實體集的屬性。</span><span class="sxs-lookup"><span data-stu-id="7e903-210">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="7e903-211">在 EF 核心術語：</span><span class="sxs-lookup"><span data-stu-id="7e903-211">In EF Core terminology:</span></span>

* <span data-ttu-id="7e903-212">實體集合通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="7e903-212">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="7e903-213">實體對應到資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="7e903-213">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="7e903-214">`DbSet<Enrollment>`和`DbSet<Course>`可以省略。</span><span class="sxs-lookup"><span data-stu-id="7e903-214">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="7e903-215">EF 核心包含這些隱含因為`Student`實體參考`Enrollment`實體，而`Enrollment`實體參考`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="7e903-215">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="7e903-216">本教學課程中，保留`DbSet<Enrollment>`和`DbSet<Course>`中`SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="7e903-216">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="7e903-217">EF 核心建立資料庫時，會建立具有相同名稱的資料表`DbSet`屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="7e903-217">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="7e903-218">集合的屬性名稱都是通常複數 （學生而不是學生）。</span><span class="sxs-lookup"><span data-stu-id="7e903-218">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="7e903-219">開發人員不同意有關資料表名稱是否應該使用複數。</span><span class="sxs-lookup"><span data-stu-id="7e903-219">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="7e903-220">如需這些教學課程中，預設行為會覆寫指定的 DbContext 單數資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="7e903-220">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="7e903-221">若要指定單一的資料表名稱，加入下列反白顯示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="7e903-221">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="7e903-222">暫存器具有相依性插入的內容</span><span class="sxs-lookup"><span data-stu-id="7e903-222">Register the context with dependency injection</span></span>

<span data-ttu-id="7e903-223">包含 ASP.NET Core[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="7e903-223">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="7e903-224">在應用程式啟動時的相依性插入會註冊服務 （例如 EF 核心 DB 內容）。</span><span class="sxs-lookup"><span data-stu-id="7e903-224">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="7e903-225">需要這些服務 （例如 Razor 頁面） 的元件會提供這些服務，透過建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="7e903-225">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="7e903-226">取得 db 內容執行個體的建構函式程式碼是稍後在本教學課程中所示。</span><span class="sxs-lookup"><span data-stu-id="7e903-226">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="7e903-227">若要註冊`SchoolContext`為服務時，開啟*Startup.cs*，並加入要反白顯示的行`ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-227">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="7e903-228">連接字串的名稱會傳遞至內容所呼叫的方法上`DbContextOptionsBuilder`物件。</span><span class="sxs-lookup"><span data-stu-id="7e903-228">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="7e903-229">本機開發， [ASP.NET Core 組態系統](xref:fundamentals/configuration/index)讀取連接字串從*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="7e903-229">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="7e903-230">新增`using`陳述式`ContosoUniversity.Data`和`Microsoft.EntityFrameworkCore`命名空間。</span><span class="sxs-lookup"><span data-stu-id="7e903-230">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="7e903-231">建置專案。</span><span class="sxs-lookup"><span data-stu-id="7e903-231">Build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="7e903-232">開啟*appsettings.json*檔案，然後加入連接字串，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="7e903-232">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="7e903-233">上述的連接字串使用`ConnectRetryCount=0`防止[SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework)從停滯。</span><span class="sxs-lookup"><span data-stu-id="7e903-233">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="7e903-234">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="7e903-234">SQL Server Express LocalDB</span></span>

<span data-ttu-id="7e903-235">連接字串會指定 SQL Server LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-235">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="7e903-236">LocalDB 是輕量版 SQL Server Express Database Engine 和適用於應用程式開發，而不是生產環境使用。</span><span class="sxs-lookup"><span data-stu-id="7e903-236">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="7e903-237">視需要啟動 LocalDB，並以使用者模式執行，因此沒有複雜的設定。</span><span class="sxs-lookup"><span data-stu-id="7e903-237">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="7e903-238">根據預設，建立 LocalDB *.mdf* DB 中的檔案`C:/Users/<user>`目錄。</span><span class="sxs-lookup"><span data-stu-id="7e903-238">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="7e903-239">加入程式碼以初始化測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="7e903-239">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="7e903-240">EF 核心會建立空的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-240">EF Core creates an empty DB.</span></span> <span data-ttu-id="7e903-241">在本節中，*種子*會寫入測試的資料填入的方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-241">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="7e903-242">在*資料*資料夾中，建立新的類別檔案命名為*DbInitializer.cs*並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="7e903-242">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="7e903-243">程式碼會檢查是否有任何學生 DB 中。</span><span class="sxs-lookup"><span data-stu-id="7e903-243">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="7e903-244">如果在資料庫中有沒有學生，測試資料植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-244">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="7e903-245">它將測試資料載入至陣列而非`List<T>`集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="7e903-245">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="7e903-246">`EnsureCreated`方法會自動建立的 DB DB 內容。</span><span class="sxs-lookup"><span data-stu-id="7e903-246">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="7e903-247">若 DB 存在，則`EnsureCreated`傳回而不需修改資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-247">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="7e903-248">在*Program.cs*，修改`Main`方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="7e903-248">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="7e903-249">從相依性插入容器中取得 DB 內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="7e903-249">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="7e903-250">呼叫種子方法，將內容傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="7e903-250">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="7e903-251">Seed 方法完成時處置內容。</span><span class="sxs-lookup"><span data-stu-id="7e903-251">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="7e903-252">下列程式碼將示範更新*Program.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="7e903-252">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="7e903-253">第一次執行應用程式時，資料庫會建立並植入的測試資料。</span><span class="sxs-lookup"><span data-stu-id="7e903-253">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="7e903-254">更新資料模型時：</span><span class="sxs-lookup"><span data-stu-id="7e903-254">When the data model is updated:</span></span>
* <span data-ttu-id="7e903-255">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-255">Delete the DB.</span></span>
* <span data-ttu-id="7e903-256">更新 seed 方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-256">Update the seed method.</span></span>
* <span data-ttu-id="7e903-257">執行應用程式，並建立新的植入的資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-257">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="7e903-258">在之後的教學課程中的資料模型變更，而不需要刪除並重新建立資料庫時，會更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-258">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="7e903-259">加入 scaffold 工具</span><span class="sxs-lookup"><span data-stu-id="7e903-259">Add scaffold tooling</span></span>

<span data-ttu-id="7e903-260">在本節中，封裝管理員主控台 (PMC) 用來新增 Visual Studio web 程式碼產生封裝。</span><span class="sxs-lookup"><span data-stu-id="7e903-260">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="7e903-261">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="7e903-261">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="7e903-262">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="7e903-262">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="7e903-263">在 [封裝管理員主控台 (PMC)，請輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="7e903-263">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="7e903-264">前一個命令會將 \*.csproj 檔案中的 NuGet 封裝：</span><span class="sxs-lookup"><span data-stu-id="7e903-264">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="7e903-265">Scaffold 模型</span><span class="sxs-lookup"><span data-stu-id="7e903-265">Scaffold the model</span></span>

* <span data-ttu-id="7e903-266">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="7e903-266">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="7e903-267">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="7e903-267">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
<span data-ttu-id="7e903-268">如果會產生下列錯誤：</span><span class="sxs-lookup"><span data-stu-id="7e903-268">If the following error is generated:</span></span>

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

<span data-ttu-id="7e903-269">重新執行命令，並將在頁面底部的註解。</span><span class="sxs-lookup"><span data-stu-id="7e903-269">Run the command again and leave a comment at the bottom of the page.</span></span>

<span data-ttu-id="7e903-270">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="7e903-270">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="7e903-271">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="7e903-271">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="7e903-272">建置專案。</span><span class="sxs-lookup"><span data-stu-id="7e903-272">Build the project.</span></span> <span data-ttu-id="7e903-273">建置會產生類似如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="7e903-273">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="7e903-274">全域變更`_context.Student`至`_context.Students`(亦即，加入"s" `Student`)。</span><span class="sxs-lookup"><span data-stu-id="7e903-274">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="7e903-275">找到項目 7 及更新。</span><span class="sxs-lookup"><span data-stu-id="7e903-275">7 occurrences are found and updated.</span></span> <span data-ttu-id="7e903-276">我們希望修正[這個 bug](https://github.com/aspnet/Scaffolding/issues/633)的下一個版本。</span><span class="sxs-lookup"><span data-stu-id="7e903-276">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="7e903-277">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="7e903-277">Test the app</span></span>

<span data-ttu-id="7e903-278">執行應用程式並選取**學生**連結。</span><span class="sxs-lookup"><span data-stu-id="7e903-278">Run the app and select the **Students** link.</span></span> <span data-ttu-id="7e903-279">根據瀏覽器寬度，**學生**連結出現在頁面頂端。</span><span class="sxs-lookup"><span data-stu-id="7e903-279">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="7e903-280">如果**學生**看不到連結中，按一下右上角的 [導覽] 圖示。</span><span class="sxs-lookup"><span data-stu-id="7e903-280">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Contoso 大學首頁窄](intro/_static/home-page-narrow.png)

<span data-ttu-id="7e903-282">測試**建立**，**編輯**，和**詳細資料**連結。</span><span class="sxs-lookup"><span data-stu-id="7e903-282">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="7e903-283">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="7e903-283">View the DB</span></span>

<span data-ttu-id="7e903-284">應用程式啟動時，`DbInitializer.Initialize`呼叫`EnsureCreated`。</span><span class="sxs-lookup"><span data-stu-id="7e903-284">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="7e903-285">`EnsureCreated`如果資料庫存在，而且若有需要，請建立一個偵測。</span><span class="sxs-lookup"><span data-stu-id="7e903-285">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="7e903-286">如果在 DB 中，沒有任何學生`Initialize`方法會將學生。</span><span class="sxs-lookup"><span data-stu-id="7e903-286">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="7e903-287">開啟**SQL Server 物件總管**(SSOX) 從**檢視**Visual Studio 中的功能表。</span><span class="sxs-lookup"><span data-stu-id="7e903-287">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="7e903-288">在 [SSOX，按一下 [ **(localdb) \MSSQLLocalDB > 資料庫 > ContosoUniversity1**。</span><span class="sxs-lookup"><span data-stu-id="7e903-288">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="7e903-289">展開**資料表**節點。</span><span class="sxs-lookup"><span data-stu-id="7e903-289">Expand the **Tables** node.</span></span>

<span data-ttu-id="7e903-290">以滑鼠右鍵按一下**學生**資料表，並按一下**檢視資料**若要查看建立的資料行和資料列插入資料表。</span><span class="sxs-lookup"><span data-stu-id="7e903-290">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="7e903-291">*.Mdf*和*.ldf* DB 檔案位於*C:\Users\\ <yourusername> *資料夾。</span><span class="sxs-lookup"><span data-stu-id="7e903-291">The *.mdf* and *.ldf* DB files are in the *C:\Users\\<yourusername>* folder.</span></span>

<span data-ttu-id="7e903-292">`EnsureCreated`啟動應用程式，可讓下列工作流程上呼叫：</span><span class="sxs-lookup"><span data-stu-id="7e903-292">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="7e903-293">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="7e903-293">Delete the DB.</span></span>
* <span data-ttu-id="7e903-294">變更資料庫結構描述 (例如，加入`EmailAddress`欄位)。</span><span class="sxs-lookup"><span data-stu-id="7e903-294">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="7e903-295">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="7e903-295">Run the app.</span></span>

<span data-ttu-id="7e903-296">`EnsureCreated`建立與 DB`EmailAddress`資料行。</span><span class="sxs-lookup"><span data-stu-id="7e903-296">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="7e903-297">慣例</span><span class="sxs-lookup"><span data-stu-id="7e903-297">Conventions</span></span>

<span data-ttu-id="7e903-298">若要建立完整資料庫的 EF 核心順序撰寫的程式碼數量是最小因為使用的慣例或讓 EF 核心的假設。</span><span class="sxs-lookup"><span data-stu-id="7e903-298">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="7e903-299">名稱`DbSet`屬性會用做資料表的名稱。</span><span class="sxs-lookup"><span data-stu-id="7e903-299">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="7e903-300">實體沒有參考`DbSet`屬性，實體類別名稱會當做資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="7e903-300">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="7e903-301">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="7e903-301">Entity property names are used for column names.</span></span>

* <span data-ttu-id="7e903-302">實體屬性會在名為 ID 或 classnameID 辨識為主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="7e903-302">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="7e903-303">屬性會解譯為外部索引鍵屬性上，如果名稱為* <navigation property name> <primary key property name> * (例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`).</span><span class="sxs-lookup"><span data-stu-id="7e903-303">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="7e903-304">外部索引鍵屬性都可以具名* <primary key property name> * (例如，`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="7e903-304">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="7e903-305">傳統行為可以被覆寫。</span><span class="sxs-lookup"><span data-stu-id="7e903-305">Conventional behavior can be overridden.</span></span> <span data-ttu-id="7e903-306">例如，資料表名稱可以明確指定，如稍早在本教學課程中所示。</span><span class="sxs-lookup"><span data-stu-id="7e903-306">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="7e903-307">可以明確設定資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="7e903-307">The column names can be explicitly set.</span></span> <span data-ttu-id="7e903-308">主索引鍵和外部索引鍵可以明確設定。</span><span class="sxs-lookup"><span data-stu-id="7e903-308">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="7e903-309">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="7e903-309">Asynchronous code</span></span>

<span data-ttu-id="7e903-310">非同步程式設計是預設的 ASP.NET Core 和 EF 核心模式。</span><span class="sxs-lookup"><span data-stu-id="7e903-310">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="7e903-311">Web 伺服器的有限的數目的執行緒可用，而且在高負載情況下的所有可用的執行緒可能正在使用中。</span><span class="sxs-lookup"><span data-stu-id="7e903-311">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="7e903-312">當發生這種情況時，伺服器無法處理新的要求，直到執行緒釋放。</span><span class="sxs-lookup"><span data-stu-id="7e903-312">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="7e903-313">使用同步程式碼，許多執行緒可能會將繫結起來雖然它們實際上並不執行任何工作，因為他們正在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="7e903-313">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="7e903-314">使用非同步程式碼，當處理程序在等候 I/O 完成，它的執行緒釋放針對伺服器將用於處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="7e903-314">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="7e903-315">如此一來，非同步程式碼可讓更有效率地使用伺服器資源，而且伺服器已啟用以處理更多流量不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="7e903-315">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="7e903-316">非同步程式碼會在執行階段導致少量的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="7e903-316">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="7e903-317">低流量的情況下，效能的衝擊，微不足道時的高流量的情況下，可能很大的潛在的效能改善。</span><span class="sxs-lookup"><span data-stu-id="7e903-317">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="7e903-318">下列程式碼，`async`關鍵字，`Task<T>`傳回值，`await`關鍵字和`ToListAsync`方法提供程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="7e903-318">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="7e903-319">`async`關鍵字會指示編譯器將：</span><span class="sxs-lookup"><span data-stu-id="7e903-319">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="7e903-320">產生的組件的方法主體的回呼。</span><span class="sxs-lookup"><span data-stu-id="7e903-320">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="7e903-321">自動建立[工作](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7)傳回物件。</span><span class="sxs-lookup"><span data-stu-id="7e903-321">Automatically create the [Task](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="7e903-322">如需詳細資訊，請參閱[工作傳回型別](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。</span><span class="sxs-lookup"><span data-stu-id="7e903-322">For more information, see [Task Return Type](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="7e903-323">隱含傳回型別`Task`代表進行中的工作。</span><span class="sxs-lookup"><span data-stu-id="7e903-323">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="7e903-324">`await`關鍵字會導致編譯器分成兩個部分的方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-324">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="7e903-325">以非同步方式啟動的作業結束的第一個部分。</span><span class="sxs-lookup"><span data-stu-id="7e903-325">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="7e903-326">第二個部分會放入作業完成時呼叫的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-326">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="7e903-327">`ToListAsync`非同步版本`ToList`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-327">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="7e903-328">若要撰寫使用 EF 核心的非同步程式碼時應該注意的事項：</span><span class="sxs-lookup"><span data-stu-id="7e903-328">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="7e903-329">只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="7e903-329">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="7e903-330">包含`ToListAsync`， `SingleOrDefaultAsync`， `FirstOrDefaultAsync`，和`SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="7e903-330">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="7e903-331">它不會包含陳述式，只要變更`IQueryable`，例如`var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="7e903-331">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="7e903-332">EF 核心內容不是安全執行緒： 不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="7e903-332">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="7e903-333">若要利用非同步程式碼的效能優勢，確認程式庫封裝 （例如分頁） 使用非同步呼叫查詢傳送至資料庫的 EF 核心方法。</span><span class="sxs-lookup"><span data-stu-id="7e903-333">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="7e903-334">如需在.NET 非同步程式設計的詳細資訊，請參閱[非同步概觀](https://docs.microsoft.com/dotnet/articles/standard/async)。</span><span class="sxs-lookup"><span data-stu-id="7e903-334">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="7e903-335">在下一個教學課程中，基本 CRUD （建立、 讀取、 更新、 刪除） 作業會檢查。</span><span class="sxs-lookup"><span data-stu-id="7e903-335">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7e903-336">下一步</span><span class="sxs-lookup"><span data-stu-id="7e903-336">Next</span></span>](xref:data/ef-rp/crud)
