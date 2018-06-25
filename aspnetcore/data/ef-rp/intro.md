---
title: ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8
author: rick-anderson
description: 示範如何建立使用 Entity Framework Core 的 Razor 頁面應用程式
ms.author: riande
ms.date: 11/15/2017
uid: data/ef-rp/intro
ms.openlocfilehash: cadf36f4e1ff3776ad4139e1d7c4e9b73687bc5c
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279226"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="74569-103">ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8</span><span class="sxs-lookup"><span data-stu-id="74569-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="74569-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="74569-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="74569-105">Contoso 大學的範例 Web 應用程式將示範如何以 Entity Framework (EF) Core 2.0 和 Visual Studio 2017 來建立 ASP.NET Core 2.0 MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="74569-105">The Contoso University sample web app demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="74569-106">這個範例應用程式是虛構的 Contoso 大學網站。</span><span class="sxs-lookup"><span data-stu-id="74569-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="74569-107">其中包括的功能有學生入學許可、課程建立、教師指派。</span><span class="sxs-lookup"><span data-stu-id="74569-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="74569-108">此頁面是說明如何建立 Contoso 大學範例應用程式教學課程系列中的第一頁。</span><span class="sxs-lookup"><span data-stu-id="74569-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="74569-109">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="74569-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="74569-110">[下載指示](xref:tutorials/index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="74569-110">[Download instructions](xref:tutorials/index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="74569-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="74569-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<span data-ttu-id="74569-112">熟悉 [Razor 頁面](xref:razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="74569-112">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="74569-113">新進程式設計人員應先完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，再開始此系列。</span><span class="sxs-lookup"><span data-stu-id="74569-113">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="74569-114">疑難排解</span><span class="sxs-lookup"><span data-stu-id="74569-114">Troubleshooting</span></span>

<span data-ttu-id="74569-115">如果您遭遇無法解決的問題，將您的程式碼與[已完成的階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)作比較，通常可以找到解答。</span><span class="sxs-lookup"><span data-stu-id="74569-115">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots).</span></span> <span data-ttu-id="74569-116">如需常見的錯誤以及如何解決這些問題的清單，請參閱[ 數列中的最後一個教學課程疑難排解 > 一節](xref:data/ef-mvc/advanced#common-errors)。</span><span class="sxs-lookup"><span data-stu-id="74569-116">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](xref:data/ef-mvc/advanced#common-errors).</span></span> <span data-ttu-id="74569-117">如果您在那裡找不到所需的資訊，可以將問題張貼至 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 中的 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="74569-117">If you don't find what you need there, you can post a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="74569-118">本系列教學課程建立於先前已完成的教學課程上。</span><span class="sxs-lookup"><span data-stu-id="74569-118">This series of tutorials builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="74569-119">成功完成每一個教學課程後，請儲存專案的複本。</span><span class="sxs-lookup"><span data-stu-id="74569-119">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="74569-120">如果您遇到問題，您可以從上一個教學課程來重新開始，而不需從頭開始。</span><span class="sxs-lookup"><span data-stu-id="74569-120">If you run into problems, you can start over from the previous tutorial instead of going back to the beginning.</span></span> <span data-ttu-id="74569-121">或者，您也可以下載[已完成的階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) 並以此階段重新開始。</span><span class="sxs-lookup"><span data-stu-id="74569-121">Alternatively, you can download a [completed stage](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) and start over using the completed stage.</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="74569-122">Contoso 大學 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="74569-122">The Contoso University web app</span></span>

<span data-ttu-id="74569-123">在教學課程中建立的應用程式，是一個基本的大學網站。</span><span class="sxs-lookup"><span data-stu-id="74569-123">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="74569-124">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="74569-124">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="74569-125">以下是幾個在教學課程中建立的畫面。</span><span class="sxs-lookup"><span data-stu-id="74569-125">Here are a few of the screens created in the tutorial.</span></span>

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

<span data-ttu-id="74569-128">此網站的 UI 樣式接近內建範本所產生的內容。</span><span class="sxs-lookup"><span data-stu-id="74569-128">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="74569-129">教學課程聚焦於 EF Core 與 Razor 頁面，而不是 UI。</span><span class="sxs-lookup"><span data-stu-id="74569-129">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-a-razor-pages-web-app"></a><span data-ttu-id="74569-130">建立 Razor 頁面 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="74569-130">Create a Razor Pages web app</span></span>

* <span data-ttu-id="74569-131">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。</span><span class="sxs-lookup"><span data-stu-id="74569-131">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="74569-132">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="74569-132">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="74569-133">將專案命名為 **ContosoUniversity**。</span><span class="sxs-lookup"><span data-stu-id="74569-133">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="74569-134">請務必將專案命名為 *ContosoUniversity*，當您複製/貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="74569-134">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
 <span data-ttu-id="74569-135">![新增 ASP.NET Core Web 應用程式](intro/_static/np.png)</span><span class="sxs-lookup"><span data-stu-id="74569-135">![new ASP.NET Core Web Application](intro/_static/np.png)</span></span>
* <span data-ttu-id="74569-136">在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="74569-136">Select **ASP.NET Core 2.0** in the dropdown, and then select **Web Application**.</span></span>
 <span data-ttu-id="74569-137">![Web 應用程式 (Razor 頁面)](../../razor-pages/index/_static/np2.png)</span><span class="sxs-lookup"><span data-stu-id="74569-137">![Web Application (Razor Pages)](../../razor-pages/index/_static/np2.png)</span></span>

<span data-ttu-id="74569-138">按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。</span><span class="sxs-lookup"><span data-stu-id="74569-138">Press **F5** to run the app in debug mode or **Ctrl-F5** to run without attaching the debugger</span></span>

## <a name="set-up-the-site-style"></a><span data-ttu-id="74569-139">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="74569-139">Set up the site style</span></span>

<span data-ttu-id="74569-140">一些變更設定了網站的功能表、配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="74569-140">A few changes set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="74569-141">開啟 *Pages/_Layout.cshtml* 並進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="74569-141">Open *Pages/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="74569-142">將每一個 "ContosoUniversity" 的發生次數變更為 "Contoso University"。</span><span class="sxs-lookup"><span data-stu-id="74569-142">Change each occurrence of "ContosoUniversity" to "Contoso University."</span></span> <span data-ttu-id="74569-143">共有三個發生次數。</span><span class="sxs-lookup"><span data-stu-id="74569-143">There are three occurrences.</span></span>

* <span data-ttu-id="74569-144">為 **Students**、**Courses**、**Instructors**、**Departments** 新增功能表項目，並刪除 **Contact** 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="74569-144">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="74569-145">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="74569-145">The changes are highlighted.</span></span> <span data-ttu-id="74569-146">(所有標記都「不會」顯示。)</span><span class="sxs-lookup"><span data-stu-id="74569-146">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

<span data-ttu-id="74569-147">在 *Pages/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：</span><span class="sxs-lookup"><span data-stu-id="74569-147">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

<span data-ttu-id="74569-148">按 CTRL+F5 執行專案。</span><span class="sxs-lookup"><span data-stu-id="74569-148">Press CTRL+F5 to run the project.</span></span> <span data-ttu-id="74569-149">首頁會以從下列教學課程中建立的索引標籤來顯示：</span><span class="sxs-lookup"><span data-stu-id="74569-149">The home page is displayed with tabs created in the following tutorials:</span></span>

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="create-the-data-model"></a><span data-ttu-id="74569-151">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="74569-151">Create the data model</span></span>

<span data-ttu-id="74569-152">為 Contoso 大學應用程式建立實體類別。</span><span class="sxs-lookup"><span data-stu-id="74569-152">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="74569-153">從下列三個實體開始：</span><span class="sxs-lookup"><span data-stu-id="74569-153">Start with the following three entities:</span></span>

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="74569-155">在 `Student` 和 `Enrollment` 實體之間會有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="74569-155">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="74569-156">在 `Course` 和 `Enrollment` 實體之間會有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="74569-156">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="74569-157">一位學生可註冊任何數目的課程。</span><span class="sxs-lookup"><span data-stu-id="74569-157">A student can enroll in any number of courses.</span></span> <span data-ttu-id="74569-158">一堂課程可以讓任意數目的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="74569-158">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="74569-159">在下列一節中，將為這些實體建立各自的類別。</span><span class="sxs-lookup"><span data-stu-id="74569-159">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="74569-160">Student 實體</span><span class="sxs-lookup"><span data-stu-id="74569-160">The Student entity</span></span>

![Student 實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="74569-162">建立 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="74569-162">Create a *Models* folder.</span></span> <span data-ttu-id="74569-163">在 *Models* 資料夾中，用下列程式碼建立一個名為 *Student.cs* 的類別檔案：</span><span class="sxs-lookup"><span data-stu-id="74569-163">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="74569-164">`ID` 屬性會成為資料庫 (DB) 資料表中的主索引鍵資料行，並對應至這個類別。</span><span class="sxs-lookup"><span data-stu-id="74569-164">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="74569-165">EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。</span><span class="sxs-lookup"><span data-stu-id="74569-165">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="74569-166">在 `classnameID` 中，`classname` 是類別的名稱，例如上述範例中的 `Student`。</span><span class="sxs-lookup"><span data-stu-id="74569-166">In `classnameID`, `classname` is the name of the class, such as `Student` in the preceding example.</span></span>

<span data-ttu-id="74569-167">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="74569-167">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="74569-168">導覽屬性會連結至與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="74569-168">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="74569-169">在這個案例中，`Student entity` 中的 `Enrollments` 屬性會保有與 `Student` 相關的所有 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="74569-169">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="74569-170">例如，如果資料庫中 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性會包含這兩個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="74569-170">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="74569-171">相關的 `Enrollment` 資料列，包含該學生在 `StudentID` 資料行中的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="74569-171">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="74569-172">例如，假設 student with ID=1 在 `Enrollment` 資料表中有兩個資料列。</span><span class="sxs-lookup"><span data-stu-id="74569-172">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="74569-173">`Enrollment` 資料表有兩個包含 `StudentID`=1 的資料列。</span><span class="sxs-lookup"><span data-stu-id="74569-173">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="74569-174">`StudentID` 為 `Enrollment` 資料表中的外部索引鍵，會指定在 `Student` 資料表中的學生。</span><span class="sxs-lookup"><span data-stu-id="74569-174">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="74569-175">如果導覽屬性可以保存多個實體，該導覽屬性必然是清單類型，例如 `ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="74569-175">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="74569-176">您可以指定 `ICollection<T>`，或是如 `List<T>`、`HashSet<T>` 的類型。</span><span class="sxs-lookup"><span data-stu-id="74569-176">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="74569-177">如使用 `ICollection<T>`，EF Core 預設將建立 `HashSet<T>` 集合。</span><span class="sxs-lookup"><span data-stu-id="74569-177">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="74569-178">保存多個實體的導覽屬性，來自多對多和一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="74569-178">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="74569-179">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="74569-179">The Enrollment entity</span></span>

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="74569-181">在 *Models* 資料夾中，用下列程式碼建立 *Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="74569-181">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="74569-182">`EnrollmentID` 屬性是主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74569-182">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="74569-183">這個實體會使用 `classnameID` 模式，而不是像 `Student` 實體一樣的 `ID`。</span><span class="sxs-lookup"><span data-stu-id="74569-183">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="74569-184">開發人員通常會選擇其中一個模式，並使用於整個資料模型。</span><span class="sxs-lookup"><span data-stu-id="74569-184">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="74569-185">在稍後的教學課程中，將示範使用不具類別名稱的 ID，使得在數據模型中實作繼承更加簡單。</span><span class="sxs-lookup"><span data-stu-id="74569-185">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="74569-186">`Grade` 屬性為 `enum`。</span><span class="sxs-lookup"><span data-stu-id="74569-186">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="74569-187">問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="74569-187">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="74569-188">為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。</span><span class="sxs-lookup"><span data-stu-id="74569-188">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="74569-189">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="74569-189">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="74569-190">一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="74569-190">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="74569-191">`Student` 實體與 `Student.Enrollments` 導覽屬性不同，導覽屬性包含多個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="74569-191">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="74569-192">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="74569-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="74569-193">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="74569-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="74569-194">如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="74569-194">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="74569-195">例如，`StudentID` 為 `Student` 導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`。</span><span class="sxs-lookup"><span data-stu-id="74569-195">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="74569-196">外部索引鍵屬性也可命名為 `<primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="74569-196">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="74569-197">例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。</span><span class="sxs-lookup"><span data-stu-id="74569-197">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="74569-198">Course 實體</span><span class="sxs-lookup"><span data-stu-id="74569-198">The Course entity</span></span>

![Course 實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="74569-200">在 *Models* 資料夾中，用下列程式碼建立 *Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="74569-200">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="74569-201">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="74569-201">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="74569-202">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="74569-202">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="74569-203">`DatabaseGenerated` 屬性 (attribute) 可讓應用程式指定主索引鍵，而不是讓 DB 來產生。</span><span class="sxs-lookup"><span data-stu-id="74569-203">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="create-the-schoolcontext-db-context"></a><span data-ttu-id="74569-204">建立 SchoolContext DB 內容</span><span class="sxs-lookup"><span data-stu-id="74569-204">Create the SchoolContext DB context</span></span>

<span data-ttu-id="74569-205">為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="74569-205">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="74569-206">資料內容衍生自 `Microsoft.EntityFrameworkCore.DbContext`。</span><span class="sxs-lookup"><span data-stu-id="74569-206">The data context is derived from `Microsoft.EntityFrameworkCore.DbContext`.</span></span> <span data-ttu-id="74569-207">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="74569-207">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="74569-208">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="74569-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="74569-209">在專案資料夾中，建立名為 *Data* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="74569-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="74569-210">在 *Data* 資料夾中以下列程式碼建立 *SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="74569-210">In the *Data* folder create *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="74569-211">程式碼會為每一個實體集建立 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="74569-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="74569-212">在 EF Core 用語中：</span><span class="sxs-lookup"><span data-stu-id="74569-212">In EF Core terminology:</span></span>

* <span data-ttu-id="74569-213">實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="74569-213">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="74569-214">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="74569-214">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="74569-215">`DbSet<Enrollment>` 和 `DbSet<Course>` 可以省略。</span><span class="sxs-lookup"><span data-stu-id="74569-215">`DbSet<Enrollment>` and `DbSet<Course>` can be omitted.</span></span> <span data-ttu-id="74569-216">EF Core 會隱含它們，因為 `Student` 實體引用 `Enrollment` 實體，而 `Enrollment` 實體引用 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="74569-216">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="74569-217">本教學課程中，請在 `SchoolContext` 保留 `DbSet<Enrollment>` 和 `DbSet<Course>`。</span><span class="sxs-lookup"><span data-stu-id="74569-217">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

<span data-ttu-id="74569-218">資料庫建立時，EF Core 會建立和 `DbSet` 屬性名稱相同的資料表。</span><span class="sxs-lookup"><span data-stu-id="74569-218">When the DB is created, EF Core creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="74569-219">集合的屬性名稱通常是複數 (Students 而不是 Student）。</span><span class="sxs-lookup"><span data-stu-id="74569-219">Property names for collections are typically plural (Students rather than Student).</span></span> <span data-ttu-id="74569-220">針對是否要讓資料表名稱都使用複數，開發人員並沒有共識。</span><span class="sxs-lookup"><span data-stu-id="74569-220">Developers disagree about whether table names should be plural.</span></span> <span data-ttu-id="74569-221">在此系列教學課程中，預設行為可藉由指定 DbContext 中的單數資料表名稱來覆寫。</span><span class="sxs-lookup"><span data-stu-id="74569-221">For these tutorials, the default behavior is overridden by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="74569-222">若要指定單數資料表名稱，請新增下列醒目標示的程式碼：</span><span class="sxs-lookup"><span data-stu-id="74569-222">To specify singular table names, add the following highlighted code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="74569-223">使用相依性插入來註冊內容</span><span class="sxs-lookup"><span data-stu-id="74569-223">Register the context with dependency injection</span></span>

<span data-ttu-id="74569-224">ASP.NET Core 包含了[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="74569-224">ASP.NET Core includes [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="74569-225">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="74569-225">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="74569-226">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="74569-226">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="74569-227">取得資料庫內容執行個體的建構函式程式碼，在本教學課程稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="74569-227">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="74569-228">若要將 `SchoolContext` 註冊為服務，請開啟 *Startup.cs*，並將醒目標示的程式碼新增至 `ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="74569-228">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="74569-229">連接字串的名稱，會透過呼叫 `DbContextOptionsBuilder` 物件上的方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="74569-229">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="74569-230">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="74569-230">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="74569-231">為 `ContosoUniversity.Data` 和 `Microsoft.EntityFrameworkCore` 命名空間新增 `using` 陳述式。</span><span class="sxs-lookup"><span data-stu-id="74569-231">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces.</span></span> <span data-ttu-id="74569-232">建置專案。</span><span class="sxs-lookup"><span data-stu-id="74569-232">Build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="74569-233">請開啟 *appsettings.json* 檔案並新增連接字串，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="74569-233">Open the *appsettings.json* file and add a connection string as shown in the following code:</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

<span data-ttu-id="74569-234">上述的連接字串使用 `ConnectRetryCount=0` 以防止 [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) 停滯。</span><span class="sxs-lookup"><span data-stu-id="74569-234">The preceding connection string uses `ConnectRetryCount=0` to prevent [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) from hanging.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="74569-235">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="74569-235">SQL Server Express LocalDB</span></span>

<span data-ttu-id="74569-236">連接字串會指定 SQL Server LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-236">The connection string specifies a SQL Server LocalDB DB.</span></span> <span data-ttu-id="74569-237">LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。</span><span class="sxs-lookup"><span data-stu-id="74569-237">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="74569-238">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="74569-238">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="74569-239">LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="74569-239">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="74569-240">新增程式碼來初始化含有測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="74569-240">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="74569-241">EF Core 會建立空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-241">EF Core creates an empty DB.</span></span> <span data-ttu-id="74569-242">在本節中，會寫入「種子」方法並以測試資料來填入該資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-242">In this section, a *Seed* method is written to populate it with test data.</span></span>

<span data-ttu-id="74569-243">在 *Data* 資料夾中，建立名為 *DbInitializer.cs* 的新類別檔案並新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="74569-243">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="74569-244">程式碼會檢查資料庫中是否有任何學生。</span><span class="sxs-lookup"><span data-stu-id="74569-244">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="74569-245">如果資料庫中沒有學生，測試資料將會植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-245">If there are no students in the DB, the DB is seeded with test data.</span></span> <span data-ttu-id="74569-246">會將測試資料載入至陣列而非 `List<T>` 集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="74569-246">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="74569-247">`EnsureCreated` 方法會自動為資料庫內容建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-247">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="74569-248">如果資料庫已存在，則 `EnsureCreated` 將會傳回而不修改該資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-248">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="74569-249">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="74569-249">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="74569-250">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="74569-250">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="74569-251">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="74569-251">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="74569-252">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="74569-252">Dispose the context when the seed method completes.</span></span>

<span data-ttu-id="74569-253">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="74569-253">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

<span data-ttu-id="74569-254">第一次執行應用程式時，會建立資料庫並植入測試資料。</span><span class="sxs-lookup"><span data-stu-id="74569-254">The first time the app is run, the DB is created and seeded with test data.</span></span> <span data-ttu-id="74569-255">更新資料模型時：</span><span class="sxs-lookup"><span data-stu-id="74569-255">When the data model is updated:</span></span>
* <span data-ttu-id="74569-256">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-256">Delete the DB.</span></span>
* <span data-ttu-id="74569-257">更新種子方法。</span><span class="sxs-lookup"><span data-stu-id="74569-257">Update the seed method.</span></span>
* <span data-ttu-id="74569-258">執行應用程式，並建立新的已植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-258">Run the app and a new seeded DB is created.</span></span> 

<span data-ttu-id="74569-259">在之後的教學課程中，資料模型變更但不需要刪除和重新建立資料庫時，將會更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-259">In later tutorials, the DB is updated when the data model changes, without deleting and re-creating the DB.</span></span>

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a><span data-ttu-id="74569-260">新增 Scaffold 工具</span><span class="sxs-lookup"><span data-stu-id="74569-260">Add scaffold tooling</span></span>

<span data-ttu-id="74569-261">在本節中，套件管理員主控台 (PMC) 將用來新增 Visual Studio web 程式碼產生套件。</span><span class="sxs-lookup"><span data-stu-id="74569-261">In this section, the Package Manager Console (PMC) is used to add the Visual Studio web code generation package.</span></span> <span data-ttu-id="74569-262">執行 Scaffolding 引擎需要此套件。</span><span class="sxs-lookup"><span data-stu-id="74569-262">This package is required to run the scaffolding engine.</span></span>

<span data-ttu-id="74569-263">從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。</span><span class="sxs-lookup"><span data-stu-id="74569-263">From the **Tools** menu, select **NuGet Package Manager** > **Package Manager Console**.</span></span>

<span data-ttu-id="74569-264">請在套件管理員主控台 (PMC) 中輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="74569-264">In the Package Manager Console (PMC), enter the following commands:</span></span>

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

<span data-ttu-id="74569-265">上述命令會將 NuGet 套件新增至 \*.csproj 檔案：</span><span class="sxs-lookup"><span data-stu-id="74569-265">The previous command adds the NuGet packages to the \*.csproj file:</span></span>

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a><span data-ttu-id="74569-266">Scaffold 模型</span><span class="sxs-lookup"><span data-stu-id="74569-266">Scaffold the model</span></span>

* <span data-ttu-id="74569-267">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="74569-267">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>
* <span data-ttu-id="74569-268">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="74569-268">Run the following commands:</span></span>


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

<span data-ttu-id="74569-269">如果您收到錯誤：</span><span class="sxs-lookup"><span data-stu-id="74569-269">If you get the error:</span></span>
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

<span data-ttu-id="74569-270">在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。</span><span class="sxs-lookup"><span data-stu-id="74569-270">Open a command window in the project directory (The directory that contains the *Program.cs*, *Startup.cs*, and *.csproj* files).</span></span>


<span data-ttu-id="74569-271">建置專案。</span><span class="sxs-lookup"><span data-stu-id="74569-271">Build the project.</span></span> <span data-ttu-id="74569-272">建置時會產生如下錯誤：</span><span class="sxs-lookup"><span data-stu-id="74569-272">The build generates errors like the following:</span></span>

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 <span data-ttu-id="74569-273">將 `_context.Student` 全域變更為 `_context.Students` (亦即，在 `Student` 的名稱上新增一個 "s")。</span><span class="sxs-lookup"><span data-stu-id="74569-273">Globally change `_context.Student` to `_context.Students` (that is, add an "s" to `Student`).</span></span> <span data-ttu-id="74569-274">找到並更新 7 個發生次數。</span><span class="sxs-lookup"><span data-stu-id="74569-274">7 occurrences are found and updated.</span></span> <span data-ttu-id="74569-275">我們希望下一個版本能修正[這個 Bug](https://github.com/aspnet/Scaffolding/issues/633)。</span><span class="sxs-lookup"><span data-stu-id="74569-275">We hope to fix [this bug](https://github.com/aspnet/Scaffolding/issues/633) in the next release.</span></span>

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a><span data-ttu-id="74569-276">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="74569-276">Test the app</span></span>

<span data-ttu-id="74569-277">執行應用程式並選取 **Students** 連結。</span><span class="sxs-lookup"><span data-stu-id="74569-277">Run the app and select the **Students** link.</span></span> <span data-ttu-id="74569-278">根據瀏覽器的寬度，**Students** 連結將會出現在頁面頂端。</span><span class="sxs-lookup"><span data-stu-id="74569-278">Depending on the browser width, the **Students** link appears at the top of the page.</span></span> <span data-ttu-id="74569-279">如果看不到 **Students** 連結，按一下右上角的導覽圖示。</span><span class="sxs-lookup"><span data-stu-id="74569-279">If the **Students** link isn't visible, click the navigation icon in the upper right corner.</span></span>

![Contoso 大學首頁 (窄)](intro/_static/home-page-narrow.png)

<span data-ttu-id="74569-281">測試**建立**、**編輯**和**詳細資料**連結。</span><span class="sxs-lookup"><span data-stu-id="74569-281">Test the **Create**, **Edit**, and **Details** links.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="74569-282">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="74569-282">View the DB</span></span>

<span data-ttu-id="74569-283">應用程式啟動時，`DbInitializer.Initialize` 呼叫 `EnsureCreated`。</span><span class="sxs-lookup"><span data-stu-id="74569-283">When the app is started, `DbInitializer.Initialize` calls `EnsureCreated`.</span></span> <span data-ttu-id="74569-284">`EnsureCreated` 會偵測資料庫是否已存在，並會在需要時建立一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-284">`EnsureCreated` detects if the DB exists, and creates one if necessary.</span></span> <span data-ttu-id="74569-285">如果資料庫中沒有學生，`Initialize` 方法將會新增學生。</span><span class="sxs-lookup"><span data-stu-id="74569-285">If there are no Students in the DB, the `Initialize` method adds students.</span></span>

<span data-ttu-id="74569-286">從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="74569-286">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="74569-287">在 SSOX 中，按一下 **(localdb) \MSSQLLocalDB > Databases > ContosoUniversity1**。</span><span class="sxs-lookup"><span data-stu-id="74569-287">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="74569-288">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="74569-288">Expand the **Tables** node.</span></span>

<span data-ttu-id="74569-289">以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="74569-289">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

<span data-ttu-id="74569-290"><em>.Mdf</em> 和 <em>.ldf</em> 資料庫檔案位於 <em>C:\Users\\<yourusername></em> 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="74569-290">The <em>.mdf</em> and <em>.ldf</em> DB files are in the <em>C:\Users\\<yourusername></em> folder.</span></span>

<span data-ttu-id="74569-291">應用程式啟動時會呼叫 `EnsureCreated` 以允許下列工作流程：</span><span class="sxs-lookup"><span data-stu-id="74569-291">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="74569-292">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-292">Delete the DB.</span></span>
* <span data-ttu-id="74569-293">變更資料庫結構描述 (例如新增 `EmailAddress` 欄位)。</span><span class="sxs-lookup"><span data-stu-id="74569-293">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="74569-294">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="74569-294">Run the app.</span></span>

<span data-ttu-id="74569-295">`EnsureCreated` 會建立包含 `EmailAddress` 資料行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="74569-295">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

## <a name="conventions"></a><span data-ttu-id="74569-296">慣例</span><span class="sxs-lookup"><span data-stu-id="74569-296">Conventions</span></span>

<span data-ttu-id="74569-297">若要以 EF Core 來建立一個完整的資料庫，所需的程式碼數量非常小，是因為使用慣例或 EF Core 所做之假設的緣故。</span><span class="sxs-lookup"><span data-stu-id="74569-297">The amount of code written in order for EF Core to create a complete DB is minimal because of the use of conventions, or assumptions that EF Core makes.</span></span>

* <span data-ttu-id="74569-298">`DbSet` 屬性的名稱會用作資料表的名稱。</span><span class="sxs-lookup"><span data-stu-id="74569-298">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="74569-299">針對 `DbSet` 屬性並未參考的實體，實體類別名稱會用於資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="74569-299">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="74569-300">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="74569-300">Entity property names are used for column names.</span></span>

* <span data-ttu-id="74569-301">命名為 ID 或 classnameID 的實體屬性，會辨識為主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="74569-301">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="74569-302">如果屬性已具名 *<navigation property name><primary key property name>* (例如，`StudentID` 為 `Student` 的導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`)，會將屬性解譯為外部索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="74569-302">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="74569-303">外部索引鍵屬性都可以具名 *<primary key property name>*(例如 `EnrollmentID`，因為 `Enrollment` 實體的主索引鍵是 `EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="74569-303">Foreign key properties can be named *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="74569-304">慣例行為可以被覆寫。</span><span class="sxs-lookup"><span data-stu-id="74569-304">Conventional behavior can be overridden.</span></span> <span data-ttu-id="74569-305">例如，資料表名稱可以明確指定，如稍早在本教學課程中所示。</span><span class="sxs-lookup"><span data-stu-id="74569-305">For example, the table names can be explicitly specified, as shown earlier in this tutorial.</span></span> <span data-ttu-id="74569-306">可以明確設定資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="74569-306">The column names can be explicitly set.</span></span> <span data-ttu-id="74569-307">主索引鍵和外部索引鍵可以明確設定。</span><span class="sxs-lookup"><span data-stu-id="74569-307">Primary keys and foreign keys can be explicitly set.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="74569-308">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="74569-308">Asynchronous code</span></span>

<span data-ttu-id="74569-309">非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。</span><span class="sxs-lookup"><span data-stu-id="74569-309">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="74569-310">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="74569-310">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="74569-311">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="74569-311">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="74569-312">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="74569-312">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="74569-313">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="74569-313">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="74569-314">因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="74569-314">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="74569-315">非同步程式碼會在執行階段導致少量的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="74569-315">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="74569-316">在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="74569-316">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="74569-317">在下列程式碼中，`async` 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和`ToListAsync` 方法使程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="74569-317">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="74569-318">`async` 關鍵字會指示編譯器：</span><span class="sxs-lookup"><span data-stu-id="74569-318">The `async` keyword tells the compiler to:</span></span>

  * <span data-ttu-id="74569-319">為方法主體的組件產生回呼。</span><span class="sxs-lookup"><span data-stu-id="74569-319">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="74569-320">自動建立傳回的 [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) 物件。</span><span class="sxs-lookup"><span data-stu-id="74569-320">Automatically create the [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) object that's returned.</span></span> <span data-ttu-id="74569-321">如需詳細資訊，請參閱 [Task 傳回型別](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。</span><span class="sxs-lookup"><span data-stu-id="74569-321">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="74569-322">隱含傳回型別 `Task` 代表進行中的工作。</span><span class="sxs-lookup"><span data-stu-id="74569-322">The implicit return type `Task` represents ongoing work.</span></span>

* <span data-ttu-id="74569-323">`await` 關鍵字會導致編譯器將方法分成兩個組件。</span><span class="sxs-lookup"><span data-stu-id="74569-323">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="74569-324">第一個部分會與非同步啟動的作業一同結束。</span><span class="sxs-lookup"><span data-stu-id="74569-324">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="74569-325">第二個部分則會放入作業完成時呼叫的回呼方法中。</span><span class="sxs-lookup"><span data-stu-id="74569-325">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="74569-326">`ToListAsync` 是 `ToList` 擴充方法的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="74569-326">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="74569-327">若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="74569-327">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="74569-328">只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="74569-328">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="74569-329">包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="74569-329">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="74569-330">不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="74569-330">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="74569-331">EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="74569-331">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span> 

* <span data-ttu-id="74569-332">若要利用非同步程式碼的效能優勢，請確認程式庫套件 (例如分頁) 在呼叫將查詢傳送至資料庫的 EF Core 方法時是使用非同步。</span><span class="sxs-lookup"><span data-stu-id="74569-332">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="74569-333">如需在 .NET 中非同步程式設計的詳細資訊，請參閱[非同步總覽](/dotnet/articles/standard/async)。</span><span class="sxs-lookup"><span data-stu-id="74569-333">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

<span data-ttu-id="74569-334">在下一個教學課程中，將會檢視基本的 CRUD (建立、讀取、更新、刪除) 作業。</span><span class="sxs-lookup"><span data-stu-id="74569-334">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="74569-335">下一步</span><span class="sxs-lookup"><span data-stu-id="74569-335">Next</span></span>](xref:data/ef-rp/crud)
