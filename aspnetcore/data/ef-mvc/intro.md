---
title: 教學課程：開始在 ASP.NET MVC web 應用程式中使用 EF Core
description: 這是說明如何從零開始建立 Contoso 大學範例應用程式教學課程系列中的第一頁。
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: 04694f20c7142cc2917df25458e8e335ee933900
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2020
ms.locfileid: "76268775"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a><span data-ttu-id="31957-103">教學課程：開始在 ASP.NET MVC web 應用程式中使用 EF Core</span><span class="sxs-lookup"><span data-stu-id="31957-103">Tutorial: Get started with EF Core in an ASP.NET MVC web app</span></span>

<span data-ttu-id="31957-104">此教學課程**尚未**升級至 ASP.NET Core 3.0。</span><span class="sxs-lookup"><span data-stu-id="31957-104">This tutorial has **not** been updated to ASP.NET Core 3.0.</span></span> <span data-ttu-id="31957-105">[Razor Pages 版本](xref:data/ef-rp/intro)已更新。</span><span class="sxs-lookup"><span data-stu-id="31957-105">The [Razor Pages version](xref:data/ef-rp/intro) has been updated.</span></span> <span data-ttu-id="31957-106">本教學課程 ASP.NET Core 3.0 和更新版本的大部分程式碼變更：</span><span class="sxs-lookup"><span data-stu-id="31957-106">Most of the code changes for the ASP.NET Core 3.0 and later version of this tutorial:</span></span>

* <span data-ttu-id="31957-107">位於*Startup.cs*和*Program.cs*檔案中。</span><span class="sxs-lookup"><span data-stu-id="31957-107">Are in the *Startup.cs* and *Program.cs* files.</span></span>
* <span data-ttu-id="31957-108">可以在[Razor Pages 版本](xref:data/ef-rp/intro)中找到。</span><span class="sxs-lookup"><span data-stu-id="31957-108">Can be found in the [Razor Pages version](xref:data/ef-rp/intro).</span></span> 

<span data-ttu-id="31957-109">如需何時可能更新此資訊的詳細資訊，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/13920) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="31957-109">For information on when this might be updated, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/13920).</span></span>

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

<span data-ttu-id="31957-110">Contoso 大學範例 Web 應用程式示範如何使用 Entity Framework (EF) Core 2.2 和 Visual Studio 2017 或 2019 建立 ASP.NET Core 2.2 MVC Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="31957-110">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.2 MVC web applications using Entity Framework (EF) Core 2.2 and Visual Studio 2017 or 2019.</span></span>

<span data-ttu-id="31957-111">這個範例應用程式是虛構的 Contoso 大學網站。</span><span class="sxs-lookup"><span data-stu-id="31957-111">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="31957-112">其中包括的功能有學生入學許可、課程建立、教師指派。</span><span class="sxs-lookup"><span data-stu-id="31957-112">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="31957-113">這是說明如何從零開始建立 Contoso 大學範例應用程式教學課程系列中的第一頁。</span><span class="sxs-lookup"><span data-stu-id="31957-113">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

<span data-ttu-id="31957-114">在本教學課程中，您將：</span><span class="sxs-lookup"><span data-stu-id="31957-114">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="31957-115">建立 ASP.NET Core MVC Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="31957-115">Create an ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="31957-116">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="31957-116">Set up the site style</span></span>
> * <span data-ttu-id="31957-117">了解 EF Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="31957-117">Learn about EF Core NuGet packages</span></span>
> * <span data-ttu-id="31957-118">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="31957-118">Create the data model</span></span>
> * <span data-ttu-id="31957-119">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="31957-119">Create the database context</span></span>
> * <span data-ttu-id="31957-120">為內容註冊相依性插入</span><span class="sxs-lookup"><span data-stu-id="31957-120">Register the context for dependency injection</span></span>
> * <span data-ttu-id="31957-121">使用測試資料將資料庫初始化</span><span class="sxs-lookup"><span data-stu-id="31957-121">Initialize the database with test data</span></span>
> * <span data-ttu-id="31957-122">建立控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="31957-122">Create a controller and views</span></span>
> * <span data-ttu-id="31957-123">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="31957-123">View the database</span></span>

## <a name="prerequisites"></a><span data-ttu-id="31957-124">必要條件：</span><span class="sxs-lookup"><span data-stu-id="31957-124">Prerequisites</span></span>

* [<span data-ttu-id="31957-125">.NET Core SDK 2.2</span><span class="sxs-lookup"><span data-stu-id="31957-125">.NET Core SDK 2.2</span></span>](https://www.microsoft.com/net/download)
* <span data-ttu-id="31957-126">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 和下列工作負載：</span><span class="sxs-lookup"><span data-stu-id="31957-126">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the following workloads:</span></span>
  * <span data-ttu-id="31957-127">**ASP.NET 與網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="31957-127">**ASP.NET and web development** workload</span></span>
  * <span data-ttu-id="31957-128">**.NET Core 跨平台開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="31957-128">**.NET Core cross-platform development** workload</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="31957-129">疑難排解</span><span class="sxs-lookup"><span data-stu-id="31957-129">Troubleshooting</span></span>

<span data-ttu-id="31957-130">如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)。</span><span class="sxs-lookup"><span data-stu-id="31957-130">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="31957-131">如需常見的錯誤以及如何解決這些問題的清單，請參閱[ 數列中的最後一個教學課程疑難排解 > 一節](advanced.md#common-errors)。</span><span class="sxs-lookup"><span data-stu-id="31957-131">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="31957-132">如果您找不到您需要那里，您可以張貼問題的 StackOverflow.com [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="31957-132">If you don't find what you need there, you can post a question to StackOverflow.com for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP]
> <span data-ttu-id="31957-133">這是 10 個教學的系列課程，當中的每一個課程都是建置於先前教學課程的成果上。</span><span class="sxs-lookup"><span data-stu-id="31957-133">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span> <span data-ttu-id="31957-134">成功完成每一個教學課程後，請儲存專案的複本。</span><span class="sxs-lookup"><span data-stu-id="31957-134">Consider saving a copy of the project after each successful tutorial completion.</span></span> <span data-ttu-id="31957-135">如果您遇到問題，您可以從上一個教學課程來重新開始，而不需從系列的一開始從頭來過。</span><span class="sxs-lookup"><span data-stu-id="31957-135">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="contoso-university-web-app"></a><span data-ttu-id="31957-136">Contoso 大學 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="31957-136">Contoso University web app</span></span>

<span data-ttu-id="31957-137">您在這些教學課程中會建置的應用程式為一個簡單的大學網站。</span><span class="sxs-lookup"><span data-stu-id="31957-137">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="31957-138">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="31957-138">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="31957-139">以下是您會建立的幾個畫面。</span><span class="sxs-lookup"><span data-stu-id="31957-139">Here are a few of the screens you'll create.</span></span>

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

## <a name="create-web-app"></a><span data-ttu-id="31957-142">建立 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="31957-142">Create web app</span></span>

* <span data-ttu-id="31957-143">開啟 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="31957-143">Open Visual Studio.</span></span>

* <span data-ttu-id="31957-144">從 [檔案] 功能表選取[新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="31957-144">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="31957-145">從左側窗格中，選取 [已安裝] > [Visual C#] > [Web]。</span><span class="sxs-lookup"><span data-stu-id="31957-145">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="31957-146">選取 [ASP.NET Core Web 應用程式] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="31957-146">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="31957-147">輸入 **ContosoUniversity** 作為名稱，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="31957-147">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![新增專案對話方塊](intro/_static/new-project2.png)

* <span data-ttu-id="31957-149">等候 [新增 ASP.NET Core Web 應用程式] 對話方塊出現。</span><span class="sxs-lookup"><span data-stu-id="31957-149">Wait for the **New ASP.NET Core Web Application** dialog to appear.</span></span>

* <span data-ttu-id="31957-150">選取 [.NET Core]、[ASP.NET Core 2.2] 和 [Web 應用程式 (Model-View-Controller)] 範本。</span><span class="sxs-lookup"><span data-stu-id="31957-150">Select **.NET Core**, **ASP.NET Core 2.2** and the **Web Application (Model-View-Controller)** template.</span></span>

* <span data-ttu-id="31957-151">確認 [驗證] 已設為 [No Authentication] (無驗證)。</span><span class="sxs-lookup"><span data-stu-id="31957-151">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="31957-152">選取 [確定]</span><span class="sxs-lookup"><span data-stu-id="31957-152">Select **OK**</span></span>

  ![[新增 ASP.NET Core 專案] 對話方塊](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="31957-154">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="31957-154">Set up the site style</span></span>

<span data-ttu-id="31957-155">一些簡單的變更會設定網站的功能表、配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="31957-155">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="31957-156">開啟 *Views/Shared/_Layout.cshtml* 並進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="31957-156">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="31957-157">將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。</span><span class="sxs-lookup"><span data-stu-id="31957-157">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="31957-158">共有三個發生次數。</span><span class="sxs-lookup"><span data-stu-id="31957-158">There are three occurrences.</span></span>

* <span data-ttu-id="31957-159">為 **About**、**Students**、**Courses**、**Instructors** 及 **Departments** 新增功能表項目，並刪除 **Privacy** 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="31957-159">Add menu entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Privacy** menu entry.</span></span>

<span data-ttu-id="31957-160">所做的變更已醒目提示。</span><span class="sxs-lookup"><span data-stu-id="31957-160">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

<span data-ttu-id="31957-161">在 *Views/Home/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：</span><span class="sxs-lookup"><span data-stu-id="31957-161">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="31957-162">按 CTRL+F5 來執行專案，或從功能表選擇 [偵錯] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="31957-162">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="31957-163">您會看到在這些教學課程中，您將建立之頁面的索引標籤和首頁。</span><span class="sxs-lookup"><span data-stu-id="31957-163">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a><span data-ttu-id="31957-165">關於 EF Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="31957-165">About EF Core NuGet packages</span></span>

<span data-ttu-id="31957-166">若要將 EF Core 支援新增至專案，請安裝您欲使用之資料庫的提供者。</span><span class="sxs-lookup"><span data-stu-id="31957-166">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="31957-167">本教學課程使用 SQL Server，其提供者套件為 [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。</span><span class="sxs-lookup"><span data-stu-id="31957-167">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="31957-168">此套件包含在 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 中，因此您不需要參考該套件。</span><span class="sxs-lookup"><span data-stu-id="31957-168">This package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), so you don't need to reference the package.</span></span>

<span data-ttu-id="31957-169">EF SQL Server 套件及其相依性 (`Microsoft.EntityFrameworkCore` 及 `Microsoft.EntityFrameworkCore.Relational`) 提供了 EF 的執行階段支援。</span><span class="sxs-lookup"><span data-stu-id="31957-169">The EF SQL Server package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="31957-170">您會在稍後的[移轉](migrations.md)教學課程中新增工具套件。</span><span class="sxs-lookup"><span data-stu-id="31957-170">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span>

<span data-ttu-id="31957-171">如需其他 Entity Framework Core 可用之資料庫提供者的資訊，請參閱[資料庫提供者](/ef/core/providers/)。</span><span class="sxs-lookup"><span data-stu-id="31957-171">For information about other database providers that are available for Entity Framework Core, see [Database providers](/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="31957-172">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="31957-172">Create the data model</span></span>

<span data-ttu-id="31957-173">接下來您會為 Contoso 大學應用程式建立實體類別。</span><span class="sxs-lookup"><span data-stu-id="31957-173">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="31957-174">您會從下列三個實體開始。</span><span class="sxs-lookup"><span data-stu-id="31957-174">You'll start with the following three entities.</span></span>

![Course-Enrollment-Student 資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="31957-176">在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="31957-176">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="31957-177">換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="31957-177">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="31957-178">在下節中，您會為這些實體建立各自的類別。</span><span class="sxs-lookup"><span data-stu-id="31957-178">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="31957-179">Student 實體</span><span class="sxs-lookup"><span data-stu-id="31957-179">The Student entity</span></span>

![Student 實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="31957-181">在 *Models* 資料夾中，建立一個名為 *Student.cs* 的類別檔案，然後使用下列程式碼取代範本程式碼。</span><span class="sxs-lookup"><span data-stu-id="31957-181">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="31957-182">`ID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。</span><span class="sxs-lookup"><span data-stu-id="31957-182">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="31957-183">Entity Framework 預設會將名為 `ID` 或 `classnameID` 的屬性解譯為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="31957-183">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="31957-184">`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。</span><span class="sxs-lookup"><span data-stu-id="31957-184">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="31957-185">導覽屬性會保留與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="31957-185">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="31957-186">在這個案例中，`Student entity` 的 `Enrollments` 屬性會保有與該 `Student` 實體相關的所有 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="31957-186">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="31957-187">換句話說，若資料庫中給定的學生資料列有兩個相關的 Enrollment 資料列 (包含該學生於其 StudentID 外部索引鍵資料行中主索引鍵值的資料列)，該 `Student` 實體的 `Enrollments` 導覽屬性便會包含這兩個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="31957-187">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="31957-188">若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="31957-188">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span> <span data-ttu-id="31957-189">您可以指定 `ICollection<T>` 或如 `List<T>` 或 `HashSet<T>` 等類型。</span><span class="sxs-lookup"><span data-stu-id="31957-189">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="31957-190">若您指定了 `ICollection<T>`，EF 會根據預設建立一個 `HashSet<T>` 集合。</span><span class="sxs-lookup"><span data-stu-id="31957-190">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="31957-191">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="31957-191">The Enrollment entity</span></span>

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="31957-193">在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="31957-193">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="31957-194">`EnrollmentID` 屬性將為主索引鍵。此實體會使用 `classnameID` 模式，而非您在 `Student` 實體中所見到的自身 `ID`。</span><span class="sxs-lookup"><span data-stu-id="31957-194">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="31957-195">通常您會選擇一個模式，然後在您整個資料模型中使用此模式。</span><span class="sxs-lookup"><span data-stu-id="31957-195">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="31957-196">在這裡，此變化僅作為向您展示使用不同模式之用。</span><span class="sxs-lookup"><span data-stu-id="31957-196">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="31957-197">在[稍後的教學課程](inheritance.md)中，您會了解使用沒有 classname 的識別碼可讓在資料模型中實作繼承變得更為簡單。</span><span class="sxs-lookup"><span data-stu-id="31957-197">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="31957-198">`Grade` 屬性為一個 `enum`。</span><span class="sxs-lookup"><span data-stu-id="31957-198">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="31957-199">問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="31957-199">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="31957-200">為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。</span><span class="sxs-lookup"><span data-stu-id="31957-200">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="31957-201">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="31957-201">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="31957-202">`Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。</span><span class="sxs-lookup"><span data-stu-id="31957-202">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="31957-203">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="31957-203">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="31957-204">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="31957-204">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="31957-205">Entity Framework 會將名為 `<navigation property name><primary key property name>` 的屬性解譯為外部索引鍵屬性 (例如 `Student` 導覽屬性的 `StudentID`，因為 `Student` 實體的主索引鍵為 `ID`)。</span><span class="sxs-lookup"><span data-stu-id="31957-205">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="31957-206">外部索引鍵屬性也可以簡單的命名為 `<primary key property name>` (例如 `CourseID`，因為 `Course` 實體的主索引鍵為 `CourseID`)。</span><span class="sxs-lookup"><span data-stu-id="31957-206">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="31957-207">Course 實體</span><span class="sxs-lookup"><span data-stu-id="31957-207">The Course entity</span></span>

![Course 實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="31957-209">在 *Models* 資料夾中，建立 *Course.cs*，然後使用下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="31957-209">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="31957-210">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="31957-210">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="31957-211">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="31957-211">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="31957-212">我們會在此系列[稍後的教學課程](complex-data-model.md)中進一步討論 `DatabaseGenerated` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31957-212">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="31957-213">基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。</span><span class="sxs-lookup"><span data-stu-id="31957-213">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="31957-214">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="31957-214">Create the database context</span></span>

<span data-ttu-id="31957-215">為指定資料模型協調 Entity Framework 功能的主要類別便是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="31957-215">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="31957-216">您可以透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立此類別。</span><span class="sxs-lookup"><span data-stu-id="31957-216">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="31957-217">在您的程式碼中，您會指定資料模型中包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="31957-217">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="31957-218">您也可以自訂某些 Entity Framework 行為。</span><span class="sxs-lookup"><span data-stu-id="31957-218">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="31957-219">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="31957-219">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="31957-220">在專案資料夾中，建立名為 *Data* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="31957-220">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="31957-221">在 *Data* 資料夾中，建立名為 *SchoolContext.cs* 的新類別檔案，然後使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="31957-221">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="31957-222">程式碼會為每一個實體集建立 `DbSet` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31957-222">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="31957-223">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="31957-223">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="31957-224">您可以省略 `DbSet<Enrollment>` 及 `DbSet<Course>` 陳述式，其結果也會是相同的。</span><span class="sxs-lookup"><span data-stu-id="31957-224">You could've omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="31957-225">Entity Framework 會隱含它們，因為 `Student` 實體參考了 `Enrollment` 實體；而 `Enrollment` 實體參考了 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="31957-225">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="31957-226">資料庫建立時，EF 會建立和 `DbSet` 屬性名稱相同的資料表。</span><span class="sxs-lookup"><span data-stu-id="31957-226">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="31957-227">集合的屬性名稱通常都是複數 (Students 而非 Student)，但許多開發人員會為了資料表名稱究竟是否該是複數型態而爭論。</span><span class="sxs-lookup"><span data-stu-id="31957-227">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="31957-228">在此系列教學課程中，您會藉由指定 DbContext 中的單數資料表名稱來覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="31957-228">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="31957-229">若要完成這項操作，請在最後一個 DbSet 屬性後方新增下列醒目提示程式碼。</span><span class="sxs-lookup"><span data-stu-id="31957-229">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a><span data-ttu-id="31957-230">註冊 SchoolContext</span><span class="sxs-lookup"><span data-stu-id="31957-230">Register the SchoolContext</span></span>

<span data-ttu-id="31957-231">根據預設，ASP.NET Core 會實作[相依性插入](../../fundamentals/dependency-injection.md)。</span><span class="sxs-lookup"><span data-stu-id="31957-231">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="31957-232">服務 (例如 EF 資料庫內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="31957-232">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="31957-233">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 MVC 控制器) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="31957-233">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="31957-234">您會在此教學課程的稍後看到取得內容執行個體的控制器建構函式。</span><span class="sxs-lookup"><span data-stu-id="31957-234">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="31957-235">若要將 `SchoolContext` 註冊為服務，請開啟 *Startup.cs*，並將醒目標示的程式碼新增至 `ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="31957-235">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

<span data-ttu-id="31957-236">連接字串的名稱，會透過呼叫 `DbContextOptionsBuilder` 物件上的方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="31957-236">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="31957-237">作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="31957-237">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="31957-238">為 `ContosoUniversity.Data` 和 `Microsoft.EntityFrameworkCore` 命名空間新增 `using` 陳述式，然後建置專案。</span><span class="sxs-lookup"><span data-stu-id="31957-238">Add `using` statements for `ContosoUniversity.Data` and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="31957-239">開啟 *appsettings.json* 檔案，然後如以下範例所示新增連接字串。</span><span class="sxs-lookup"><span data-stu-id="31957-239">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="31957-240">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="31957-240">SQL Server Express LocalDB</span></span>

<span data-ttu-id="31957-241">連接字串會指定 SQL Server LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-241">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="31957-242">LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程式開發，而不是生產用途。</span><span class="sxs-lookup"><span data-stu-id="31957-242">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="31957-243">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="31957-243">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="31957-244">LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="31957-244">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="initialize-db-with-test-data"></a><span data-ttu-id="31957-245">使用測試資料將 DB 初始化</span><span class="sxs-lookup"><span data-stu-id="31957-245">Initialize DB with test data</span></span>

<span data-ttu-id="31957-246">Entity Framework 會為您建立空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-246">The Entity Framework will create an empty database for you.</span></span> <span data-ttu-id="31957-247">在本節中，您會撰寫一個方法，該方法會在資料庫建立之後呼叫，以將測試資料填入資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-247">In this section, you write a method that's called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="31957-248">在此您將使用 `EnsureCreated` 方法來自動建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-248">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="31957-249">在[稍後的教學課程](migrations.md)中，您將會了解到如何使用 Code First 移轉變更資料庫結構描述，而非卸除並重新建立資料庫，來處理模型的變更。</span><span class="sxs-lookup"><span data-stu-id="31957-249">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="31957-250">在 *Data* 資料夾中，建立一個名為 *DbInitializer.cs* 的新類別檔案，使用下列程式碼取代範本程式碼。這些程式碼會在需要的時候建立資料庫，並將測試資料載入至新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-250">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="31957-251">程式碼會檢查資料庫中是否有任何學生。若沒有的話，它便會假設資料庫是新的資料庫，因此需要植入測試資料。</span><span class="sxs-lookup"><span data-stu-id="31957-251">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span> <span data-ttu-id="31957-252">會將測試資料載入至陣列而非 `List<T>` 集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="31957-252">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="31957-253">在 *Program.cs* 中，修改 `Main` 方法來在應用程式啟動期間執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="31957-253">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="31957-254">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="31957-254">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="31957-255">呼叫種子方法，並將其傳遞給內容。</span><span class="sxs-lookup"><span data-stu-id="31957-255">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="31957-256">種子方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="31957-256">Dispose the context when the seed method is done.</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="31957-257">新增 `using` 陳述式：</span><span class="sxs-lookup"><span data-stu-id="31957-257">Add `using` statements:</span></span>

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="31957-258">在較舊的教學課程中，您會在 *Startup.cs* 的 `Configure` 方法中看到類似的程式碼。</span><span class="sxs-lookup"><span data-stu-id="31957-258">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="31957-259">我們建議您只在設定要求管道時使用 `Configure` 方法。</span><span class="sxs-lookup"><span data-stu-id="31957-259">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="31957-260">應用程式啟動程式碼屬於 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="31957-260">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="31957-261">現在在您首次執行應用程式時，資料庫便會建立並植入測試資料。</span><span class="sxs-lookup"><span data-stu-id="31957-261">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="31957-262">每當您變更您的資料模型時，您可以刪除資料庫、更新您的種子方法，然後依照相同的方法取得新的資料庫以重新開始。</span><span class="sxs-lookup"><span data-stu-id="31957-262">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="31957-263">在稍後的教學課程中，您會了解如何在資料模型變更時修改資料庫，而不需要刪除和重新建立它。</span><span class="sxs-lookup"><span data-stu-id="31957-263">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-controller-and-views"></a><span data-ttu-id="31957-264">建立控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="31957-264">Create controller and views</span></span>

<span data-ttu-id="31957-265">接下來，您會使用 Visual Studio 中的 scaffolding 引擎來新增使用 EF 查詢和儲存資料的 MVC 控制器及檢視。</span><span class="sxs-lookup"><span data-stu-id="31957-265">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="31957-266">自動建立 CRUD 動作方法和檢視稱為 Scaffolding。</span><span class="sxs-lookup"><span data-stu-id="31957-266">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="31957-267">Scaffolding 與產生程式碼不同。Scaffold 程式碼是一個開始點，使得您可以修改它以符合您的需求，然而您通常不會去修改產生的程式碼。</span><span class="sxs-lookup"><span data-stu-id="31957-267">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="31957-268">當您需要自訂產生的程式碼時，您會使用部分類別，或者您會在事務變更時重新產生程式碼。</span><span class="sxs-lookup"><span data-stu-id="31957-268">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="31957-269">在方案總管中的 **Controllers** 資料夾上以滑鼠右鍵按一下，然後選取 [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="31957-269">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="31957-270">在 [新增 Scaffold] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="31957-270">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="31957-271">選取 [使用 Entity Framework 執行檢視的 MVC 控制器]。</span><span class="sxs-lookup"><span data-stu-id="31957-271">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="31957-272">按一下 [加入]。</span><span class="sxs-lookup"><span data-stu-id="31957-272">Click **Add**.</span></span> <span data-ttu-id="31957-273">[新增使用 Entity Framework 執行檢視的 MVC 控制器] 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="31957-273">The **Add MVC Controller with views, using Entity Framework** dialog box appears.</span></span>

    ![Scaffold Student](intro/_static/scaffold-student2.png)

  * <span data-ttu-id="31957-275">在 [模型類別] 中，選取 [Student]。</span><span class="sxs-lookup"><span data-stu-id="31957-275">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="31957-276">在 [資料內容類別] 中，選取 [SchoolContext]。</span><span class="sxs-lookup"><span data-stu-id="31957-276">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="31957-277">接受預設的 **StudentsController** 作為名稱。</span><span class="sxs-lookup"><span data-stu-id="31957-277">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="31957-278">按一下 [加入]。</span><span class="sxs-lookup"><span data-stu-id="31957-278">Click **Add**.</span></span>

  <span data-ttu-id="31957-279">當您按一下 [新增] 時，Visual Studio Scaffolding 引擎便會建立 *StudentsController.cs* 檔案及一組可以使用該控制器的檢視 ( *.cshtml* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="31957-279">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="31957-280">(Scaffolding 引擎也可以在您沒有如本教學課程先前的操作一樣先手動建立時，為您建立資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="31957-280">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="31957-281">您可以在 [新增控制器] 方塊中藉由按一下 [資料內容類別] 右側的加號來指定新的內容類別。</span><span class="sxs-lookup"><span data-stu-id="31957-281">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="31957-282">Visual Studio 接著便會建立您的 `DbContext` 類別及控制器和檢視。)</span><span class="sxs-lookup"><span data-stu-id="31957-282">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="31957-283">您會發現控制器會接受 `SchoolContext` 作為建構函式的參數。</span><span class="sxs-lookup"><span data-stu-id="31957-283">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="31957-284">ASP.NET Core 相依性插入會負責傳遞 `SchoolContext` 的執行個體給控制器。</span><span class="sxs-lookup"><span data-stu-id="31957-284">ASP.NET Core dependency injection takes care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="31957-285">您可以在先前的 *Startup.cs* 檔案中設定它。</span><span class="sxs-lookup"><span data-stu-id="31957-285">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="31957-286">控制器含有一個 `Index` 動作方法，該方法會顯示資料庫中的所有學生。</span><span class="sxs-lookup"><span data-stu-id="31957-286">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="31957-287">方法會藉由讀取資料庫內容執行個體的 `Students` 屬性，來從 Students 實體集中取得學生的清單：</span><span class="sxs-lookup"><span data-stu-id="31957-287">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="31957-288">您會在教學課程的稍後學習到此程式碼中的非同步程式設計項目。</span><span class="sxs-lookup"><span data-stu-id="31957-288">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="31957-289">*Views/Students/Index.cshtml* 檢視會在一個資料表中顯示這份清單：</span><span class="sxs-lookup"><span data-stu-id="31957-289">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="31957-290">按 CTRL+F5 來執行專案，或從功能表選擇 [偵錯] > [啟動但不偵錯]。</span><span class="sxs-lookup"><span data-stu-id="31957-290">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="31957-291">按一下 [Students] 索引標籤來查看 `DbInitializer.Initialize` 方法插入的測試資料。</span><span class="sxs-lookup"><span data-stu-id="31957-291">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="31957-292">取決於您瀏覽器視窗的寬度，您可能會在頁面的頂端看到 `Students` 索引標籤連結，或是按一下位於右上角的導覽圖示來查看連結。</span><span class="sxs-lookup"><span data-stu-id="31957-292">Depending on how narrow your browser window is, you'll see the `Students` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Contoso 大學首頁 (窄)](intro/_static/home-page-narrow.png)

![Students [索引] 頁面](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="31957-295">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="31957-295">View the database</span></span>

<span data-ttu-id="31957-296">當您啟動應用程式時，`DbInitializer.Initialize` 方法會呼叫 `EnsureCreated`。</span><span class="sxs-lookup"><span data-stu-id="31957-296">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="31957-297">EF 看到不存在任何資料庫，於是便建立了一個資料庫，接著 `Initialize` 方法程式碼的剩餘部分便會將資料填入資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-297">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="31957-298">您可以使用 SQL Server 物件總管 (SSOX) 來在 Visual Studio 中檢視資料庫。</span><span class="sxs-lookup"><span data-stu-id="31957-298">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="31957-299">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="31957-299">Close the browser.</span></span>

<span data-ttu-id="31957-300">若 SSOX 視窗尚未開啟，請從 Visual Studio 中的 [檢視] 功能表選取它。</span><span class="sxs-lookup"><span data-stu-id="31957-300">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="31957-301">在 SSOX 中，按一下 **(localdb)\MSSQLLocalDB > Databases**，然後按一下位於您 *appsettings.json* 檔案中連接字串內資料庫名稱的項目。</span><span class="sxs-lookup"><span data-stu-id="31957-301">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that's in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="31957-302">展開 [資料表] 節點以查看您資料庫中的資料表。</span><span class="sxs-lookup"><span data-stu-id="31957-302">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX 中的資料表](intro/_static/ssox-tables.png)

<span data-ttu-id="31957-304">以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行及插入資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="31957-304">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![SSOX 中的 Student 資料表](intro/_static/ssox-student-table.png)

<span data-ttu-id="31957-306">*.mdf* 和 *.ldf* 資料庫檔案位於 *C:\Users\\\<您的使用者名稱>* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="31957-306">The *.mdf* and *.ldf* database files are in the *C:\Users\\\<yourusername>* folder.</span></span>

<span data-ttu-id="31957-307">因為您在應用程式啟動時執行的初始設定式方法中呼叫了 `EnsureCreated`，您現在可以對 `Student` 類別進行變更、刪除資料庫、重新執行應用程式，資料庫會自動重新建立以符合您所作出的變更。</span><span class="sxs-lookup"><span data-stu-id="31957-307">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="31957-308">例如，若您將一個 `EmailAddress` 屬性新增到 `Student` 類別，您便會在重新建立的資料表中看到新的 `EmailAddress` 資料行。</span><span class="sxs-lookup"><span data-stu-id="31957-308">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="31957-309">慣例</span><span class="sxs-lookup"><span data-stu-id="31957-309">Conventions</span></span>

<span data-ttu-id="31957-310">為了讓 Entity Framework 能夠建立一個完整資料庫，您所需要撰寫的程式碼非常少，多虧了慣例的使用及 Entity Framework 所做出的假設。</span><span class="sxs-lookup"><span data-stu-id="31957-310">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="31957-311">`DbSet` 屬性的名稱會用於資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="31957-311">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="31957-312">針對 `DbSet` 屬性並未參考的實體，實體類別名稱會用於資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="31957-312">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="31957-313">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="31957-313">Entity property names are used for column names.</span></span>

* <span data-ttu-id="31957-314">命名為 ID 或 classnameID 的實體屬性，會辨識為主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="31957-314">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="31957-315">如果屬性命名為 *\<導覽屬性名稱>\<主索引鍵屬性名稱>* ，系統就會將該屬性解譯為外部索引鍵屬性 (例如，若為 `Student` 導覽屬性，則為 `StudentID`，因為 `Student` 實體的主索引鍵是 `ID`)。</span><span class="sxs-lookup"><span data-stu-id="31957-315">A property is interpreted as a foreign key property if it's named *\<navigation property name>\<primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="31957-316">外部索引鍵屬性也可以直接命名為 *\<主索引鍵屬性名稱>* (例如 `EnrollmentID`，因為 `Enrollment` 實體的主索引鍵為 `EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="31957-316">Foreign key properties can also be named simply *\<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="31957-317">慣例行為可以被覆寫。</span><span class="sxs-lookup"><span data-stu-id="31957-317">Conventional behavior can be overridden.</span></span> <span data-ttu-id="31957-318">例如，您可以明確指定資料表名稱，如稍早在本教學課程中您所見到的。</span><span class="sxs-lookup"><span data-stu-id="31957-318">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="31957-319">您可以設定資料行名稱以及將任何屬性設為主索引鍵或外部索引鍵，如同您在本系列[稍後的教學課程](complex-data-model.md)中所見。</span><span class="sxs-lookup"><span data-stu-id="31957-319">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="31957-320">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="31957-320">Asynchronous code</span></span>

<span data-ttu-id="31957-321">非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。</span><span class="sxs-lookup"><span data-stu-id="31957-321">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="31957-322">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="31957-322">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="31957-323">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="31957-323">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="31957-324">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="31957-324">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="31957-325">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="31957-325">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="31957-326">因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="31957-326">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="31957-327">非同步程式碼雖然的確會在執行階段造成少量的負荷，但在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="31957-327">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="31957-328">在下列程式碼中，`async` 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法使程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="31957-328">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="31957-329">`async` 關鍵字會告訴編譯器為方法本體的一部分產生回呼，並自動建立傳回的 `Task<IActionResult>` 物件。</span><span class="sxs-lookup"><span data-stu-id="31957-329">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that's returned.</span></span>

* <span data-ttu-id="31957-330">傳回類型 `Task<IActionResult>` 代表了正在進行的工作，其結果為 `IActionResult` 類型。</span><span class="sxs-lookup"><span data-stu-id="31957-330">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="31957-331">`await` 關鍵字會使編譯器將方法分割為兩部分。</span><span class="sxs-lookup"><span data-stu-id="31957-331">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="31957-332">第一個部分會與非同步啟動的作業一同結束。</span><span class="sxs-lookup"><span data-stu-id="31957-332">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="31957-333">第二個部分則會放入作業完成時呼叫的回呼方法中。</span><span class="sxs-lookup"><span data-stu-id="31957-333">The second part is put into a callback method that's called when the operation completes.</span></span>

* <span data-ttu-id="31957-334">`ToListAsync` 是 `ToList` 擴充方法的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="31957-334">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="31957-335">當您在撰寫使用 Entity Framework 的非同步程式碼時，請注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="31957-335">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="31957-336">只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="31957-336">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="31957-337">其中包含，例如 `ToListAsync`、`SingleOrDefaultAsync`，以及 `SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="31957-337">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="31957-338">其中不包含，例如：僅變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="31957-338">It doesn't include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="31957-339">EF 內容在執行緒中並不安全：不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="31957-339">An EF context isn't thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="31957-340">當您呼叫任何 async EF 方法時，請一律使用 `await` 關鍵字。</span><span class="sxs-lookup"><span data-stu-id="31957-340">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="31957-341">若您想要充分利用非同步程式碼帶來的效能優點，請確保任何您正在使用的程式庫 (例如分頁) 也使用了非同步 (若它們有呼叫任何可能會傳送查詢到資料庫的 Entity Framework 方法的話)。</span><span class="sxs-lookup"><span data-stu-id="31957-341">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="31957-342">如需在 .NET 中非同步程式設計的詳細資訊，請參閱[非同步總覽](/dotnet/articles/standard/async)。</span><span class="sxs-lookup"><span data-stu-id="31957-342">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/articles/standard/async).</span></span>

## <a name="get-the-code"></a><span data-ttu-id="31957-343">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="31957-343">Get the code</span></span>

[<span data-ttu-id="31957-344">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="31957-344">Download or view the completed application.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a><span data-ttu-id="31957-345">後續步驟</span><span class="sxs-lookup"><span data-stu-id="31957-345">Next steps</span></span>

<span data-ttu-id="31957-346">在本教學課程中，您將：</span><span class="sxs-lookup"><span data-stu-id="31957-346">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="31957-347">建立 ASP.NET Core MVC Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="31957-347">Created ASP.NET Core MVC web app</span></span>
> * <span data-ttu-id="31957-348">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="31957-348">Set up the site style</span></span>
> * <span data-ttu-id="31957-349">了解 EF Core NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="31957-349">Learned about EF Core NuGet packages</span></span>
> * <span data-ttu-id="31957-350">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="31957-350">Created the data model</span></span>
> * <span data-ttu-id="31957-351">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="31957-351">Created the database context</span></span>
> * <span data-ttu-id="31957-352">註冊 SchoolContext</span><span class="sxs-lookup"><span data-stu-id="31957-352">Registered the SchoolContext</span></span>
> * <span data-ttu-id="31957-353">使用測試資料將 DB 初始化</span><span class="sxs-lookup"><span data-stu-id="31957-353">Initialized DB with test data</span></span>
> * <span data-ttu-id="31957-354">建立控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="31957-354">Created controller and views</span></span>
> * <span data-ttu-id="31957-355">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="31957-355">Viewed the database</span></span>

<span data-ttu-id="31957-356">在接下來的教學課程中，您將學習到如何執行基本的 CRUD (建立、讀取、更新、刪除) 作業。</span><span class="sxs-lookup"><span data-stu-id="31957-356">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

<span data-ttu-id="31957-357">若要了解如何執行基本的 CRUD (建立、讀取、更新、刪除) 作業，請前往下一個教學課程。</span><span class="sxs-lookup"><span data-stu-id="31957-357">Advance to the next tutorial to learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="31957-358">實作基本的 CRUD 功能</span><span class="sxs-lookup"><span data-stu-id="31957-358">Implement basic CRUD functionality</span></span>](crud.md)

