---
title: ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8
author: rick-anderson
description: 示範如何建立使用 Entity Framework Core 的 Razor 頁面應用程式
ms.author: riande
ms.custom: seodec18
ms.date: 11/22/2018
uid: data/ef-rp/intro
ms.openlocfilehash: fa18307485596ef9b3e9d5116df062d190cb6c72
ms.sourcegitcommit: 34bf9fc6ea814c039401fca174642f0acb14be3c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/14/2019
ms.locfileid: "57841484"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="319f9-103">ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8</span><span class="sxs-lookup"><span data-stu-id="319f9-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="319f9-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="319f9-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="319f9-105">Contoso 大學的 Web 應用程式範例將示範如何以 Entity Framework (EF) Core 來建立 ASP.NET Core Razor Pages 應用程式。</span><span class="sxs-lookup"><span data-stu-id="319f9-105">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="319f9-106">這個範例應用程式是虛構的 Contoso 大學網站。</span><span class="sxs-lookup"><span data-stu-id="319f9-106">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="319f9-107">其中包括的功能有學生入學許可、課程建立、教師指派。</span><span class="sxs-lookup"><span data-stu-id="319f9-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="319f9-108">此頁面是說明如何建立 Contoso 大學範例應用程式教學課程系列中的第一頁。</span><span class="sxs-lookup"><span data-stu-id="319f9-108">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="319f9-109">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="319f9-109">Download or view the completed app.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="319f9-110">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="319f9-110">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="319f9-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="319f9-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319f9-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319f9-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="319f9-113">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="319f9-113">.NET Core CLI</span></span>](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

<span data-ttu-id="319f9-114">熟悉 [Razor 頁面](xref:razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="319f9-114">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="319f9-115">新進程式設計人員應先完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，再開始此系列。</span><span class="sxs-lookup"><span data-stu-id="319f9-115">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="319f9-116">疑難排解</span><span class="sxs-lookup"><span data-stu-id="319f9-116">Troubleshooting</span></span>

<span data-ttu-id="319f9-117">如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。</span><span class="sxs-lookup"><span data-stu-id="319f9-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="319f9-118">取得協助的好方法是將問題公佈到 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 以詢問 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="319f9-118">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="319f9-119">Contoso 大學 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="319f9-119">The Contoso University web app</span></span>

<span data-ttu-id="319f9-120">在教學課程中建立的應用程式，是一個基本的大學網站。</span><span class="sxs-lookup"><span data-stu-id="319f9-120">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="319f9-121">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="319f9-121">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="319f9-122">以下是幾個在教學課程中建立的畫面。</span><span class="sxs-lookup"><span data-stu-id="319f9-122">Here are a few of the screens created in the tutorial.</span></span>

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

<span data-ttu-id="319f9-125">此網站的 UI 樣式接近內建範本所產生的內容。</span><span class="sxs-lookup"><span data-stu-id="319f9-125">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="319f9-126">教學課程聚焦於 EF Core 與 Razor 頁面，而不是 UI。</span><span class="sxs-lookup"><span data-stu-id="319f9-126">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="319f9-127">建立 ContosoUniversity Razor Pages Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="319f9-127">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319f9-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319f9-128">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319f9-129">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="319f9-129">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="319f9-130">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="319f9-130">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="319f9-131">將專案命名為 **ContosoUniversity**。</span><span class="sxs-lookup"><span data-stu-id="319f9-131">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="319f9-132">請務必將專案命名為 *ContosoUniversity*，當您複製/貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="319f9-132">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="319f9-133">在下拉式清單中選取 [ASP.NET Core 2.1]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="319f9-133">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="319f9-134">如需上述步驟的影像，請參閱[ 建立 Razor Web 應用程式](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app)。</span><span class="sxs-lookup"><span data-stu-id="319f9-134">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="319f9-135">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="319f9-135">Run the app.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="319f9-136">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="319f9-136">.NET Core CLI</span></span>](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a><span data-ttu-id="319f9-137">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="319f9-137">Set up the site style</span></span>

<span data-ttu-id="319f9-138">一些變更設定了網站的功能表、配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="319f9-138">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="319f9-139">使用下列變更更新 *Pages/Shared/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="319f9-139">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="319f9-140">將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。</span><span class="sxs-lookup"><span data-stu-id="319f9-140">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="319f9-141">共出現三次。</span><span class="sxs-lookup"><span data-stu-id="319f9-141">There are three occurrences.</span></span>

* <span data-ttu-id="319f9-142">為 **Students**、**Courses**、**Instructors**、**Departments** 新增功能表項目，並刪除 **Contact** 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="319f9-142">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="319f9-143">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="319f9-143">The changes are highlighted.</span></span> <span data-ttu-id="319f9-144">(所有標記都「不會」顯示。)</span><span class="sxs-lookup"><span data-stu-id="319f9-144">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="319f9-145">在 *Pages/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：</span><span class="sxs-lookup"><span data-stu-id="319f9-145">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="319f9-146">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="319f9-146">Create the data model</span></span>

<span data-ttu-id="319f9-147">為 Contoso 大學應用程式建立實體類別。</span><span class="sxs-lookup"><span data-stu-id="319f9-147">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="319f9-148">從下列三個實體開始：</span><span class="sxs-lookup"><span data-stu-id="319f9-148">Start with the following three entities:</span></span>

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="319f9-150">在 `Student` 和 `Enrollment` 實體之間會有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="319f9-150">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="319f9-151">在 `Course` 和 `Enrollment` 實體之間會有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="319f9-151">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="319f9-152">一位學生可註冊任何數目的課程。</span><span class="sxs-lookup"><span data-stu-id="319f9-152">A student can enroll in any number of courses.</span></span> <span data-ttu-id="319f9-153">一堂課程可以讓任意數目的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="319f9-153">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="319f9-154">在下列一節中，將為這些實體建立各自的類別。</span><span class="sxs-lookup"><span data-stu-id="319f9-154">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="319f9-155">Student 實體</span><span class="sxs-lookup"><span data-stu-id="319f9-155">The Student entity</span></span>

![Student 實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="319f9-157">建立 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="319f9-157">Create a *Models* folder.</span></span> <span data-ttu-id="319f9-158">在 *Models* 資料夾中，用下列程式碼建立一個名為 *Student.cs* 的類別檔案：</span><span class="sxs-lookup"><span data-stu-id="319f9-158">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="319f9-159">`ID` 屬性會成為資料庫 (DB) 資料表中的主索引鍵資料行，並對應至這個類別。</span><span class="sxs-lookup"><span data-stu-id="319f9-159">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="319f9-160">EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。</span><span class="sxs-lookup"><span data-stu-id="319f9-160">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="319f9-161">在 `classnameID` 中，`classname` 是類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="319f9-161">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="319f9-162">另一個自動識別的主索引鍵是上述範例中的 `StudentID`。</span><span class="sxs-lookup"><span data-stu-id="319f9-162">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="319f9-163">`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。</span><span class="sxs-lookup"><span data-stu-id="319f9-163">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="319f9-164">導覽屬性會連結至與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-164">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="319f9-165">在這個案例中，`Student entity` 中的 `Enrollments` 屬性會保有與 `Student` 相關的所有 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-165">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="319f9-166">例如，如果資料庫中 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性會包含這兩個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-166">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="319f9-167">相關的 `Enrollment` 資料列，包含該學生在 `StudentID` 資料行中的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="319f9-167">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="319f9-168">例如，假設 student with ID=1 在 `Enrollment` 資料表中有兩個資料列。</span><span class="sxs-lookup"><span data-stu-id="319f9-168">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="319f9-169">`Enrollment` 資料表有兩個包含 `StudentID`=1 的資料列。</span><span class="sxs-lookup"><span data-stu-id="319f9-169">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="319f9-170">`StudentID` 為 `Enrollment` 資料表中的外部索引鍵，會指定在 `Student` 資料表中的學生。</span><span class="sxs-lookup"><span data-stu-id="319f9-170">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="319f9-171">如果導覽屬性可以保存多個實體，該導覽屬性必然是清單類型，例如 `ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="319f9-171">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="319f9-172">您可以指定 `ICollection<T>`，或是如 `List<T>`、`HashSet<T>` 的類型。</span><span class="sxs-lookup"><span data-stu-id="319f9-172">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="319f9-173">如使用 `ICollection<T>`，EF Core 預設將建立 `HashSet<T>` 集合。</span><span class="sxs-lookup"><span data-stu-id="319f9-173">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="319f9-174">保存多個實體的導覽屬性，來自多對多和一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="319f9-174">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="319f9-175">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="319f9-175">The Enrollment entity</span></span>

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="319f9-177">在 *Models* 資料夾中，用下列程式碼建立 *Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="319f9-177">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="319f9-178">`EnrollmentID` 屬性是主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="319f9-178">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="319f9-179">這個實體會使用 `classnameID` 模式，而不是像 `Student` 實體一樣的 `ID`。</span><span class="sxs-lookup"><span data-stu-id="319f9-179">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="319f9-180">開發人員通常會選擇其中一個模式，並使用於整個資料模型。</span><span class="sxs-lookup"><span data-stu-id="319f9-180">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="319f9-181">在稍後的教學課程中，將示範使用不具類別名稱的 ID，使得在數據模型中實作繼承更加簡單。</span><span class="sxs-lookup"><span data-stu-id="319f9-181">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="319f9-182">`Grade` 屬性為 `enum`。</span><span class="sxs-lookup"><span data-stu-id="319f9-182">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="319f9-183">問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="319f9-183">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="319f9-184">為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。</span><span class="sxs-lookup"><span data-stu-id="319f9-184">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="319f9-185">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="319f9-185">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="319f9-186">一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-186">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="319f9-187">`Student` 實體與 `Student.Enrollments` 導覽屬性不同，導覽屬性包含多個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-187">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="319f9-188">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="319f9-188">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="319f9-189">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="319f9-189">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="319f9-190">如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="319f9-190">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="319f9-191">例如，`StudentID` 為 `Student` 導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`。</span><span class="sxs-lookup"><span data-stu-id="319f9-191">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="319f9-192">外部索引鍵屬性也可命名為 `<primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="319f9-192">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="319f9-193">例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。</span><span class="sxs-lookup"><span data-stu-id="319f9-193">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="319f9-194">Course 實體</span><span class="sxs-lookup"><span data-stu-id="319f9-194">The Course entity</span></span>

![Course 實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="319f9-196">在 *Models* 資料夾中，用下列程式碼建立 *Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="319f9-196">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="319f9-197">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="319f9-197">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="319f9-198">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="319f9-198">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="319f9-199">`DatabaseGenerated` 屬性 (attribute) 可讓應用程式指定主索引鍵，而不是讓 DB 來產生。</span><span class="sxs-lookup"><span data-stu-id="319f9-199">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="319f9-200">Scaffold 學生模型</span><span class="sxs-lookup"><span data-stu-id="319f9-200">Scaffold the student model</span></span>

<span data-ttu-id="319f9-201">在本節中會 scaffold 學生模型。</span><span class="sxs-lookup"><span data-stu-id="319f9-201">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="319f9-202">亦即 Scaffolding 工具會產生學生模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="319f9-202">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="319f9-203">建置專案。</span><span class="sxs-lookup"><span data-stu-id="319f9-203">Build the project.</span></span>
* <span data-ttu-id="319f9-204">建立 *Pages/Students* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="319f9-204">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="319f9-205">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="319f9-205">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="319f9-206">在 [方案總管] 中，以滑鼠右鍵按一下 *Pages/Students* 資料夾 > [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="319f9-206">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="319f9-207">在 [新增 Scaffold] 對話方塊中，選取 [Razor Pages using Entity Framework (CRUD)] \(使用 Entity Framework 的 Razor Pages (CRUD)\) > [新增]。</span><span class="sxs-lookup"><span data-stu-id="319f9-207">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="319f9-208">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="319f9-208">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="319f9-209">在 [模型類別] 下拉式清單中，選取 [學生 (ContosoUniversity.Models)]。</span><span class="sxs-lookup"><span data-stu-id="319f9-209">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="319f9-210">在 [資料內容類別] 列中，選取 **+** (加號) 符號，並將產生的名稱變更為 **ContosoUniversity.Models.SchoolContext**。</span><span class="sxs-lookup"><span data-stu-id="319f9-210">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="319f9-211">在 [資料內容類別] 下拉式清單中，選取 **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="319f9-211">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="319f9-212">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="319f9-212">Select **Add**.</span></span>

![CRUD 對話方塊](intro/_static/s1.png)

<span data-ttu-id="319f9-214">如果您有上一個步驟的問題，請參閱 [Scaffold 影片模型](xref:tutorials/razor-pages/model#scaffold-the-movie-model)。</span><span class="sxs-lookup"><span data-stu-id="319f9-214">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="319f9-215">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="319f9-215">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="319f9-216">執行下列命令來 Scaffold 學生模型。</span><span class="sxs-lookup"><span data-stu-id="319f9-216">Run the following commands to scaffold the student model.</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```
------

<span data-ttu-id="319f9-217">隨即建立 Scaffold 處理序並變更下列檔案：</span><span class="sxs-lookup"><span data-stu-id="319f9-217">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="319f9-218">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="319f9-218">Files created</span></span>

* <span data-ttu-id="319f9-219">*Pages/Students* 建立、刪除、詳細資料、編輯、索引。</span><span class="sxs-lookup"><span data-stu-id="319f9-219">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="319f9-220">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="319f9-220">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="319f9-221">檔案更新</span><span class="sxs-lookup"><span data-stu-id="319f9-221">File updates</span></span>

* <span data-ttu-id="319f9-222">*Startup.cs*：這個檔案的變更會於下一節詳述。</span><span class="sxs-lookup"><span data-stu-id="319f9-222">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="319f9-223">*appsettings.json*：已新增用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="319f9-223">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="319f9-224">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="319f9-224">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="319f9-225">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="319f9-225">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="319f9-226">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="319f9-226">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="319f9-227">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="319f9-227">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="319f9-228">取得資料庫內容執行個體的建構函式程式碼，在本教學課程稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="319f9-228">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="319f9-229">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="319f9-229">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="319f9-230">檢查 *Startup.cs* 中的 `ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="319f9-230">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="319f9-231">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="319f9-231">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="319f9-232">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="319f9-232">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="319f9-233">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="319f9-233">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="319f9-234">更新 main</span><span class="sxs-lookup"><span data-stu-id="319f9-234">Update main</span></span>

<span data-ttu-id="319f9-235">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="319f9-235">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="319f9-236">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="319f9-236">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="319f9-237">呼叫 [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)。</span><span class="sxs-lookup"><span data-stu-id="319f9-237">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="319f9-238">`EnsureCreated` 方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="319f9-238">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="319f9-239">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="319f9-239">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="319f9-240">`EnsureCreated` 可確保內容的資料庫存在。</span><span class="sxs-lookup"><span data-stu-id="319f9-240">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="319f9-241">如果存在，不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="319f9-241">If it exists, no action is taken.</span></span> <span data-ttu-id="319f9-242">如果不存在，則會建立資料庫及其所有結構描述。</span><span class="sxs-lookup"><span data-stu-id="319f9-242">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="319f9-243">`EnsureCreated` 不會使用移轉來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-243">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="319f9-244">使用 `EnsureCreated` 建立的資料庫稍後無法使用移轉來加以更新。</span><span class="sxs-lookup"><span data-stu-id="319f9-244">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="319f9-245">應用程式啟動時會呼叫 `EnsureCreated` 以允許下列工作流程：</span><span class="sxs-lookup"><span data-stu-id="319f9-245">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="319f9-246">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-246">Delete the DB.</span></span>
* <span data-ttu-id="319f9-247">變更資料庫結構描述 (例如新增 `EmailAddress` 欄位)。</span><span class="sxs-lookup"><span data-stu-id="319f9-247">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="319f9-248">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="319f9-248">Run the app.</span></span>
* <span data-ttu-id="319f9-249">`EnsureCreated` 會建立包含 `EmailAddress` 資料行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-249">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="319f9-250">在開發初期，結構描述快速發展之時，`EnsureCreated` 很方便。</span><span class="sxs-lookup"><span data-stu-id="319f9-250">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="319f9-251">稍後在本教學課程中，會刪除資料庫並使用移轉。</span><span class="sxs-lookup"><span data-stu-id="319f9-251">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="319f9-252">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="319f9-252">Test the app</span></span>

<span data-ttu-id="319f9-253">執行應用程式，並接受 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="319f9-253">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="319f9-254">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="319f9-254">This app doesn't keep personal information.</span></span> <span data-ttu-id="319f9-255">您可以在[歐盟一般資料保護規定 (GDPR) 支援](xref:security/gdpr)閱讀 Cookie 原則相關資訊。</span><span class="sxs-lookup"><span data-stu-id="319f9-255">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="319f9-256">選取 [學生] 連結，然後選取 [新建]。</span><span class="sxs-lookup"><span data-stu-id="319f9-256">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="319f9-257">測試 [編輯]、[詳細資料] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="319f9-257">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="319f9-258">檢查 SchoolContext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="319f9-258">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="319f9-259">為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="319f9-259">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="319f9-260">資料內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="319f9-260">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="319f9-261">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-261">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="319f9-262">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="319f9-262">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="319f9-263">使用下列程式碼來更新 *SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="319f9-263">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="319f9-264">醒目標示的程式碼會為每一個實體集建立 [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="319f9-264">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="319f9-265">在 EF Core 用語中：</span><span class="sxs-lookup"><span data-stu-id="319f9-265">In EF Core terminology:</span></span>

* <span data-ttu-id="319f9-266">實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="319f9-266">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="319f9-267">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="319f9-267">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="319f9-268">`DbSet<Enrollment>` 和 `DbSet<Course>` 可加以忽略。</span><span class="sxs-lookup"><span data-stu-id="319f9-268">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="319f9-269">EF Core 會隱含它們，因為 `Student` 實體引用 `Enrollment` 實體，而 `Enrollment` 實體引用 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="319f9-269">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="319f9-270">本教學課程中，請在 `SchoolContext` 保留 `DbSet<Enrollment>` 和 `DbSet<Course>`。</span><span class="sxs-lookup"><span data-stu-id="319f9-270">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="319f9-271">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="319f9-271">SQL Server Express LocalDB</span></span>

<span data-ttu-id="319f9-272">連接字串會指定 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。</span><span class="sxs-lookup"><span data-stu-id="319f9-272">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="319f9-273">LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。</span><span class="sxs-lookup"><span data-stu-id="319f9-273">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="319f9-274">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="319f9-274">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="319f9-275">LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="319f9-275">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="319f9-276">新增程式碼來初始化含有測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="319f9-276">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="319f9-277">EF Core 會建立空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-277">EF Core creates an empty DB.</span></span> <span data-ttu-id="319f9-278">在本節中，會寫入 `Initialize` 方法並以測試資料來填入該資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-278">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="319f9-279">在 *Data* 資料夾中，建立名為 *DbInitializer.cs* 的新類別檔案並新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="319f9-279">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="319f9-280">注意:上述程式碼針對命名空間 (`namespace ContosoUniversity.Models`) 使用 `Models`，而不是使用 `Data`。</span><span class="sxs-lookup"><span data-stu-id="319f9-280">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="319f9-281">`Models` 與產生框架的程式碼一致。</span><span class="sxs-lookup"><span data-stu-id="319f9-281">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="319f9-282">如需詳細資訊，請參閱[此 GitHub Scaffolding 問題](https://github.com/aspnet/Scaffolding/issues/822)。</span><span class="sxs-lookup"><span data-stu-id="319f9-282">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="319f9-283">程式碼會檢查資料庫中是否有任何學生。</span><span class="sxs-lookup"><span data-stu-id="319f9-283">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="319f9-284">如果資料庫中沒有學生，則會使用測試資料初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-284">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="319f9-285">它會將測試資料載入陣列之中，而非 `List<T>` 集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="319f9-285">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="319f9-286">`EnsureCreated` 方法會自動為資料庫內容建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-286">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="319f9-287">如果資料庫已存在，則 `EnsureCreated` 將會傳回而不修改該資料庫。</span><span class="sxs-lookup"><span data-stu-id="319f9-287">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="319f9-288">在 *Program.cs* 中，修改 `Main` 方法來呼叫 `Initialize`：</span><span class="sxs-lookup"><span data-stu-id="319f9-288">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

<span data-ttu-id="319f9-289">刪除任何學生記錄並重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="319f9-289">Delete any student records and restart the app.</span></span> <span data-ttu-id="319f9-290">如果資料庫未初始化，在 `Initialize` 中設定中斷點來診斷問題。</span><span class="sxs-lookup"><span data-stu-id="319f9-290">If the DB is not initialized, set a break point in `Initialize` to diagnose the problem.</span></span>

## <a name="view-the-db"></a><span data-ttu-id="319f9-291">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="319f9-291">View the DB</span></span>

<span data-ttu-id="319f9-292">從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="319f9-292">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="319f9-293">在 SSOX 中，按一下 **(localdb) \MSSQLLocalDB > Databases > ContosoUniversity1**。</span><span class="sxs-lookup"><span data-stu-id="319f9-293">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > ContosoUniversity1**.</span></span>

<span data-ttu-id="319f9-294">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="319f9-294">Expand the **Tables** node.</span></span>

<span data-ttu-id="319f9-295">以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="319f9-295">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="319f9-296">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="319f9-296">Asynchronous code</span></span>

<span data-ttu-id="319f9-297">非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。</span><span class="sxs-lookup"><span data-stu-id="319f9-297">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="319f9-298">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="319f9-298">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="319f9-299">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="319f9-299">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="319f9-300">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="319f9-300">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="319f9-301">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="319f9-301">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="319f9-302">因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="319f9-302">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="319f9-303">非同步程式碼會在執行階段導致少量的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="319f9-303">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="319f9-304">在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="319f9-304">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="319f9-305">在下列程式碼中，[async](/dotnet/csharp/language-reference/keywords/async) 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法會使程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="319f9-305">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="319f9-306">`async` 關鍵字會指示編譯器：</span><span class="sxs-lookup"><span data-stu-id="319f9-306">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="319f9-307">為方法主體的組件產生回呼。</span><span class="sxs-lookup"><span data-stu-id="319f9-307">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="319f9-308">自動建立傳回的 [Task](/dotnet/api/system.threading.tasks.task) 物件。</span><span class="sxs-lookup"><span data-stu-id="319f9-308">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="319f9-309">如需詳細資訊，請參閱 [Task 傳回型別](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。</span><span class="sxs-lookup"><span data-stu-id="319f9-309">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="319f9-310">隱含傳回型別 `Task` 代表進行中的工作。</span><span class="sxs-lookup"><span data-stu-id="319f9-310">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="319f9-311">`await` 關鍵字會導致編譯器將方法分成兩個組件。</span><span class="sxs-lookup"><span data-stu-id="319f9-311">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="319f9-312">第一個部分會與非同步啟動的作業一同結束。</span><span class="sxs-lookup"><span data-stu-id="319f9-312">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="319f9-313">第二個部分則會放入作業完成時呼叫的回呼方法中。</span><span class="sxs-lookup"><span data-stu-id="319f9-313">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="319f9-314">`ToListAsync` 是 `ToList` 擴充方法的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="319f9-314">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="319f9-315">若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="319f9-315">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="319f9-316">只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="319f9-316">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="319f9-317">包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="319f9-317">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="319f9-318">不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="319f9-318">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="319f9-319">EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="319f9-319">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="319f9-320">若要利用非同步程式碼的效能優勢，請確認程式庫套件 (例如分頁) 在呼叫將查詢傳送至資料庫的 EF Core 方法時是使用非同步。</span><span class="sxs-lookup"><span data-stu-id="319f9-320">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="319f9-321">如需非同步方法的詳細資訊，請參閱 [Async 概觀](/dotnet/standard/async)和[使用 Async 和 Await 設計非同步程式](/dotnet/csharp/programming-guide/concepts/async/)。</span><span class="sxs-lookup"><span data-stu-id="319f9-321">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="319f9-322">在下一個教學課程中，將會檢視基本的 CRUD (建立、讀取、更新、刪除) 作業。</span><span class="sxs-lookup"><span data-stu-id="319f9-322">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="319f9-323">其他資源</span><span class="sxs-lookup"><span data-stu-id="319f9-323">Additional resources</span></span>

* [<span data-ttu-id="319f9-324">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="319f9-324">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="319f9-325">下一步</span><span class="sxs-lookup"><span data-stu-id="319f9-325">Next</span></span>](xref:data/ef-rp/crud)
