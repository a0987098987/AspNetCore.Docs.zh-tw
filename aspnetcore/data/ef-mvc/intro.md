---
title: "Entity Framework Core 10 的教學課程 1 的 ASP.NET Core MVC"
author: tdykstra
description: 
keywords: "ASP.NET Core，Entity Framework Core，教學課程"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.assetid: b67c3d4a-f2bf-4132-a48b-4b0d599d7981
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/intro
ms.openlocfilehash: 379802f644b977563b0b50354feb1fb9a4c8fabb
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a><span data-ttu-id="f128e-103">開始使用 ASP.NET Core MVC 和 Entity Framework Core 使用 Visual Studio (1 / 10)</span><span class="sxs-lookup"><span data-stu-id="f128e-103">Getting started with ASP.NET Core MVC and Entity Framework Core using Visual Studio (1 of 10)</span></span>

<span data-ttu-id="f128e-104">由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f128e-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f128e-105">Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework (EF) 核心 2.0 和 Visual Studio 2017 ASP.NET Core 2.0 MVC web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f128e-105">The Contoso University sample web application demonstrates how to create ASP.NET Core 2.0 MVC web applications using Entity Framework (EF) Core 2.0 and Visual Studio 2017.</span></span>

<span data-ttu-id="f128e-106">範例應用程式是針對虛構的 Contoso 大學的網站。</span><span class="sxs-lookup"><span data-stu-id="f128e-106">The sample application is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="f128e-107">其中包括功能，例如許可學生、 課程建立和講師指派。</span><span class="sxs-lookup"><span data-stu-id="f128e-107">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="f128e-108">這是一系列的教學課程說明如何建置從頭 Contoso 大學範例應用程式中的第一個。</span><span class="sxs-lookup"><span data-stu-id="f128e-108">This is the first in a series of tutorials that explain how to build the Contoso University sample application from scratch.</span></span>

[<span data-ttu-id="f128e-109">下載或檢視完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="f128e-109">Download or view the completed application.</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

<span data-ttu-id="f128e-110">EF 核心 2.0 EF 的最新版本，但還沒有的 EF 的所有功能 6.x。</span><span class="sxs-lookup"><span data-stu-id="f128e-110">EF Core 2.0 is the latest version of EF but does not yet have all the features of EF 6.x.</span></span> <span data-ttu-id="f128e-111">如需有關如何選擇 EF 資訊 6.x 和 EF 核心，請參閱[EF 核心 vs。EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/)。</span><span class="sxs-lookup"><span data-stu-id="f128e-111">For information about how to choose between EF 6.x and EF Core, see [EF Core vs. EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/).</span></span> <span data-ttu-id="f128e-112">如果您選擇 EF 6.x，請參閱[此教學課程系列的舊版](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)。</span><span class="sxs-lookup"><span data-stu-id="f128e-112">If you choose EF 6.x, see [the previous version of this tutorial series](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application).</span></span>

> [!NOTE]
> * <span data-ttu-id="f128e-113">如需本教學課程的 ASP.NET Core 1.1 版本，請參閱[本教學課程以 PDF 格式的 VS 2017 Update 2 版本](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf)。</span><span class="sxs-lookup"><span data-stu-id="f128e-113">For the ASP.NET Core 1.1 version of this tutorial, see the [VS 2017 Update 2 version of this tutorial in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/ef-mvc/intro/_static/efmvc1.1.pdf).</span></span>
> * <span data-ttu-id="f128e-114">如需本教學課程的 Visual Studio 2015 版本，請參閱 [PDF 格式的 VS 2015 版本 ASP.NET 核心文件集](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf)。</span><span class="sxs-lookup"><span data-stu-id="f128e-114">For the Visual Studio 2015 version of this tutorial, see the [VS 2015 version of ASP.NET Core documentation in PDF format](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f128e-115">必要條件</span><span class="sxs-lookup"><span data-stu-id="f128e-115">Prerequisites</span></span>

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a><span data-ttu-id="f128e-116">疑難排解</span><span class="sxs-lookup"><span data-stu-id="f128e-116">Troubleshooting</span></span>

<span data-ttu-id="f128e-117">如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)。</span><span class="sxs-lookup"><span data-stu-id="f128e-117">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final).</span></span> <span data-ttu-id="f128e-118">如需常見的錯誤以及如何解決這些問題的清單，請參閱[數列中的最後一個教學課程疑難排解 > 一節](advanced.md#common-errors)。</span><span class="sxs-lookup"><span data-stu-id="f128e-118">For a list of common errors and how to solve them, see [the Troubleshooting section of the last tutorial in the series](advanced.md#common-errors).</span></span> <span data-ttu-id="f128e-119">如果您找不到您需要那里，您可以張貼問題的 StackOverflow.com [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)或[EF 核心](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="f128e-119">If you don't find what you need there, you can post a question to StackOverflow.com for  [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

> [!TIP] 
> <span data-ttu-id="f128e-120">這是一系列 10 教學課程，其中每一個都是根據所完成的作業在先前的教學課程。</span><span class="sxs-lookup"><span data-stu-id="f128e-120">This is a series of 10 tutorials, each of which builds on what is done in earlier tutorials.</span></span>  <span data-ttu-id="f128e-121">請考慮每個成功的教學課程完成後儲存專案的複本。</span><span class="sxs-lookup"><span data-stu-id="f128e-121">Consider saving a copy of the project after each successful tutorial completion.</span></span>  <span data-ttu-id="f128e-122">然後如果您遇到問題時，您可以透過從啟動上一個教學課程，而不是回到整個序列的開頭。</span><span class="sxs-lookup"><span data-stu-id="f128e-122">Then if you run into problems, you can start over from the previous tutorial instead of going back to the beginning of the whole series.</span></span>

## <a name="the-contoso-university-web-application"></a><span data-ttu-id="f128e-123">Contoso 大學 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f128e-123">The Contoso University web application</span></span>

<span data-ttu-id="f128e-124">您會在這些教學課程建置的應用程式是簡單的大學的網站。</span><span class="sxs-lookup"><span data-stu-id="f128e-124">The application you'll be building in these tutorials is a simple university web site.</span></span>

<span data-ttu-id="f128e-125">使用者可以檢視和更新學生、 課程、 和講師資訊。</span><span class="sxs-lookup"><span data-stu-id="f128e-125">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="f128e-126">以下是幾個您要建立的畫面。</span><span class="sxs-lookup"><span data-stu-id="f128e-126">Here are a few of the screens you'll create.</span></span>

![學生索引頁](intro/_static/students-index.png)

![學生編輯頁面](intro/_static/student-edit.png)

<span data-ttu-id="f128e-129">此站台的 UI 樣式保留接近內建的範本，所產生的內容，讓本教學課程可以主要還是著重於如何使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="f128e-129">The UI style of this site has been kept close to what's generated by the built-in templates, so that the tutorial can focus mainly on how to use the Entity Framework.</span></span>

## <a name="create-an-aspnet-core-mvc-web-application"></a><span data-ttu-id="f128e-130">建立 ASP.NET Core MVC web 應用程式</span><span class="sxs-lookup"><span data-stu-id="f128e-130">Create an ASP.NET Core MVC web application</span></span>

<span data-ttu-id="f128e-131">開啟 Visual Studio 並建立新 ASP.NET Core C# 的 web 專案名為"ContosoUniversity"。</span><span class="sxs-lookup"><span data-stu-id="f128e-131">Open Visual Studio and create a new ASP.NET Core C# web project named "ContosoUniversity".</span></span>

* <span data-ttu-id="f128e-132">從**檔案**功能表上，選取**新增 > 專案**。</span><span class="sxs-lookup"><span data-stu-id="f128e-132">From the **File** menu, select **New > Project**.</span></span>

* <span data-ttu-id="f128e-133">從左窗格中，選取**已安裝 > Visual C# > Web**。</span><span class="sxs-lookup"><span data-stu-id="f128e-133">From the left pane, select **Installed > Visual C# > Web**.</span></span>

* <span data-ttu-id="f128e-134">選取**ASP.NET Core Web 應用程式**專案範本。</span><span class="sxs-lookup"><span data-stu-id="f128e-134">Select the **ASP.NET Core Web Application** project template.</span></span>

* <span data-ttu-id="f128e-135">輸入**ContosoUniversity**做為名稱，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="f128e-135">Enter **ContosoUniversity** as the name and click **OK**.</span></span>

  ![[新增專案] 對話](intro/_static/new-project.png)

* <span data-ttu-id="f128e-137">等候**新 ASP.NET Core Web 應用程式 (.NET Core)**出現對話方塊</span><span class="sxs-lookup"><span data-stu-id="f128e-137">Wait for the **New ASP.NET Core Web Application (.NET Core)** dialog to appear</span></span>

* <span data-ttu-id="f128e-138">選取**ASP.NET Core 2.0**和**Web 應用程式 （模型-檢視-控制器）**範本。</span><span class="sxs-lookup"><span data-stu-id="f128e-138">Select **ASP.NET Core 2.0** and the **Web Application (Model-View-Controller)** template.</span></span>

  <span data-ttu-id="f128e-139">**注意：**本教學課程需要 ASP.NET Core 2.0 和 EF 核心 2.0 或更新版本-請確認**ASP.NET Core 1.1**未選取。</span><span class="sxs-lookup"><span data-stu-id="f128e-139">**Note:** This tutorial requires ASP.NET Core 2.0 and EF Core 2.0 or later -- make sure that **ASP.NET Core 1.1** is not selected.</span></span>

* <span data-ttu-id="f128e-140">請確定**驗證**設**非驗證**。</span><span class="sxs-lookup"><span data-stu-id="f128e-140">Make sure **Authentication** is set to **No Authentication**.</span></span>

* <span data-ttu-id="f128e-141">按一下 [確定] </span><span class="sxs-lookup"><span data-stu-id="f128e-141">Click **OK**</span></span>

  ![[新增 ASP.NET 專案] 對話方塊](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a><span data-ttu-id="f128e-143">設定站台樣式</span><span class="sxs-lookup"><span data-stu-id="f128e-143">Set up the site style</span></span>

<span data-ttu-id="f128e-144">有一些簡單的變更將會設定網站 功能表、 配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="f128e-144">A few simple changes will set up the site menu, layout, and home page.</span></span>

<span data-ttu-id="f128e-145">開啟*Views/Shared/_Layout.cshtml*並進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="f128e-145">Open *Views/Shared/_Layout.cshtml* and make the following changes:</span></span>

* <span data-ttu-id="f128e-146">將"ContosoUniversity"每次發生變更為"Contoso 大學"。</span><span class="sxs-lookup"><span data-stu-id="f128e-146">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="f128e-147">有三個相符項目。</span><span class="sxs-lookup"><span data-stu-id="f128e-147">There are three occurrences.</span></span>

* <span data-ttu-id="f128e-148">加入功能表項目**學生**，**課程**，**講師**，和**部門**，並刪除**連絡人**功能表項目。</span><span class="sxs-lookup"><span data-stu-id="f128e-148">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="f128e-149">所做的變更會反白顯示。</span><span class="sxs-lookup"><span data-stu-id="f128e-149">The changes are highlighted.</span></span>

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

<span data-ttu-id="f128e-150">在*Views/Home/Index.cshtml*，ASP.NET MVC 的相關文字使用文字來取代此應用程式有關的下列程式碼取代檔案的內容：</span><span class="sxs-lookup"><span data-stu-id="f128e-150">In *Views/Home/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this application:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

<span data-ttu-id="f128e-151">按 CTRL + F5 執行專案，或選擇**偵錯 > 啟動但不偵錯**從功能表。</span><span class="sxs-lookup"><span data-stu-id="f128e-151">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span> <span data-ttu-id="f128e-152">您會看到這些教學課程中，您將建立之頁面的索引標籤的 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="f128e-152">You see the home page with tabs for the pages you'll create in these tutorials.</span></span>

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a><span data-ttu-id="f128e-154">Entity Framework Core NuGet 封裝</span><span class="sxs-lookup"><span data-stu-id="f128e-154">Entity Framework Core NuGet packages</span></span>

<span data-ttu-id="f128e-155">若要將 EF Core 支援加入至專案，安裝您要當做目標的資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="f128e-155">To add EF Core support to a project, install the database provider that you want to target.</span></span> <span data-ttu-id="f128e-156">本教學課程使用 SQL Server，而且提供者套件可[Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。</span><span class="sxs-lookup"><span data-stu-id="f128e-156">This tutorial uses SQL Server, and the provider package is [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/).</span></span> <span data-ttu-id="f128e-157">此套件包含在[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，所以您不需要安裝它。</span><span class="sxs-lookup"><span data-stu-id="f128e-157">This package is included in the [Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage, so you don't have to install it.</span></span>

<span data-ttu-id="f128e-158">這個封裝和其相依性 (`Microsoft.EntityFrameworkCore`和`Microsoft.EntityFrameworkCore.Relational`) 提供 EF 的執行階段支援。</span><span class="sxs-lookup"><span data-stu-id="f128e-158">This package and its dependencies (`Microsoft.EntityFrameworkCore` and `Microsoft.EntityFrameworkCore.Relational`) provide runtime support for EF.</span></span> <span data-ttu-id="f128e-159">您會加入工具封裝稍後在[移轉](migrations.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="f128e-159">You'll add a tooling package later, in the [Migrations](migrations.md) tutorial.</span></span> 

<span data-ttu-id="f128e-160">其他資料庫提供者可用於 Entity Framework Core 的相關資訊，請參閱[資料庫提供者](https://docs.microsoft.com/ef/core/providers/)。</span><span class="sxs-lookup"><span data-stu-id="f128e-160">For information about other database providers that are available for Entity Framework Core, see [Database providers](https://docs.microsoft.com/ef/core/providers/).</span></span>

## <a name="create-the-data-model"></a><span data-ttu-id="f128e-161">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="f128e-161">Create the data model</span></span>

<span data-ttu-id="f128e-162">接下來您將建立 Contoso 大學應用程式的實體類別。</span><span class="sxs-lookup"><span data-stu-id="f128e-162">Next you'll create entity classes for the Contoso University application.</span></span> <span data-ttu-id="f128e-163">您會先處理下列三個實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-163">You'll start with the following three entities.</span></span>

![課程註冊學生資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="f128e-165">一對多關聯性之間`Student`和`Enrollment`實體，而且沒有之間的一對多關聯性`Course`和`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-165">There's a one-to-many relationship between `Student` and `Enrollment` entities, and there's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="f128e-166">換句話說，一位學生可以註冊任何數目的課程中，而且一個課程可以有任意數目的學生在它註冊。</span><span class="sxs-lookup"><span data-stu-id="f128e-166">In other words, a student can be enrolled in any number of courses, and a course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="f128e-167">下列各節中，您將建立這些實體的每個類別。</span><span class="sxs-lookup"><span data-stu-id="f128e-167">In the following sections you'll create a class for each one of these entities.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="f128e-168">學生實體</span><span class="sxs-lookup"><span data-stu-id="f128e-168">The Student entity</span></span>

![學生實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="f128e-170">在*模型*資料夾中，建立名為的類別檔案*Student.cs*和範本程式碼取代為下列程式碼。</span><span class="sxs-lookup"><span data-stu-id="f128e-170">In the *Models* folder, create a class file named *Student.cs* and replace the template code with the following code.</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="f128e-171">`ID`屬性就會成為主要的索引鍵資料行對應至這個類別的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="f128e-171">The `ID` property will become the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="f128e-172">根據預設，Entity Framework 會解譯此屬性，名為`ID`或`classnameID`為主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f128e-172">By default, the Entity Framework interprets a property that's named `ID` or `classnameID` as the primary key.</span></span>

<span data-ttu-id="f128e-173">`Enrollments`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f128e-173">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f128e-174">導覽屬性會保留此實體與相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-174">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="f128e-175">在此情況下，`Enrollments`屬性`Student entity`保存所有`Enrollment`的實體有關的`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-175">In this case, the `Enrollments` property of a `Student entity` will hold all of the `Enrollment` entities that are related to that `Student` entity.</span></span> <span data-ttu-id="f128e-176">換句話說，如果給定的學生資料列，在資料庫中有兩個相關註冊資料列 （資料列包含該學生的主索引鍵值其 StudentID 外部索引鍵資料行中），可`Student`實體的`Enrollments`導覽屬性會包含那些兩個`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-176">In other words, if a given Student row in the database has two related Enrollment rows (rows that contain that student's primary key value in their StudentID foreign key column), that `Student` entity's `Enrollments` navigation property will contain those two `Enrollment` entities.</span></span>

<span data-ttu-id="f128e-177">如果導覽屬性都可以保存多個實體 （如同多對多或一對多的關聯性），其類型必須是的清單中的項目可以新增、 刪除和更新，例如`ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="f128e-177">If a navigation property can hold multiple entities (as in many-to-many or one-to-many relationships), its type must be a list in which entries can be added, deleted, and updated, such as `ICollection<T>`.</span></span>  <span data-ttu-id="f128e-178">您可以指定`ICollection<T>`或型別，例如`List<T>`或`HashSet<T>`。</span><span class="sxs-lookup"><span data-stu-id="f128e-178">You can specify `ICollection<T>` or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="f128e-179">如果您指定`ICollection<T>`，EF 建立`HashSet<T>`預設集合。</span><span class="sxs-lookup"><span data-stu-id="f128e-179">If you specify `ICollection<T>`, EF creates a `HashSet<T>` collection by default.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="f128e-180">註冊實體</span><span class="sxs-lookup"><span data-stu-id="f128e-180">The Enrollment entity</span></span>

![註冊的實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="f128e-182">在*模型*資料夾中，建立*Enrollment.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f128e-182">In the *Models* folder, create *Enrollment.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="f128e-183">`EnrollmentID`屬性會是主索引鍵，此實體會使用`classnameID`模式而不是`ID`本身做為您在中看到`Student`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-183">The `EnrollmentID` property will be the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself as you saw in the `Student` entity.</span></span> <span data-ttu-id="f128e-184">通常您會選擇其中一個模式，並在您的資料模型中使用它。</span><span class="sxs-lookup"><span data-stu-id="f128e-184">Ordinarily you would choose one pattern and use it throughout your data model.</span></span> <span data-ttu-id="f128e-185">在這裡，變化說明您可以使用其中一個模式。</span><span class="sxs-lookup"><span data-stu-id="f128e-185">Here, the variation illustrates that you can use either pattern.</span></span> <span data-ttu-id="f128e-186">在[之後的教學課程](inheritance.md)，您會看到如何使用識別碼，而類別名稱不能簡化資料模型中實作繼承。</span><span class="sxs-lookup"><span data-stu-id="f128e-186">In a [later tutorial](inheritance.md), you'll see how using ID without classname makes it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="f128e-187">`Grade`屬性是`enum`。</span><span class="sxs-lookup"><span data-stu-id="f128e-187">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="f128e-188">問號之後`Grade`型別宣告表示`Grade`屬性可為 null。</span><span class="sxs-lookup"><span data-stu-id="f128e-188">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="f128e-189">為 null 的等級是不同於零等級--null 表示等級不未知或尚未被指派。</span><span class="sxs-lookup"><span data-stu-id="f128e-189">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="f128e-190">`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。</span><span class="sxs-lookup"><span data-stu-id="f128e-190">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="f128e-191">`Enrollment`實體都與一個`Student`實體，所以此屬性只可以保存單一`Student`實體 (不同於`Student.Enrollments`導覽屬性之前看到，而可以包含多個`Enrollment`實體)。</span><span class="sxs-lookup"><span data-stu-id="f128e-191">An `Enrollment` entity is associated with one `Student` entity, so the property can only hold a single `Student` entity (unlike the `Student.Enrollments` navigation property you saw earlier, which can hold multiple `Enrollment` entities).</span></span>

<span data-ttu-id="f128e-192">`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。</span><span class="sxs-lookup"><span data-stu-id="f128e-192">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="f128e-193">`Enrollment`實體都與一個`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-193">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="f128e-194">Entity Framework 會解譯為外部索引鍵屬性屬性如果名稱為`<navigation property name><primary key property name>`(例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。</span><span class="sxs-lookup"><span data-stu-id="f128e-194">Entity Framework interprets a property as a foreign key property if it's named `<navigation property name><primary key property name>` (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="f128e-195">外部索引鍵屬性也只稱為`<primary key property name>`(例如，`CourseID`因為`Course`實體的主索引鍵是`CourseID`)。</span><span class="sxs-lookup"><span data-stu-id="f128e-195">Foreign key properties can also be named simply `<primary key property name>` (for example, `CourseID` since the `Course` entity's primary key is `CourseID`).</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="f128e-196">課程實體</span><span class="sxs-lookup"><span data-stu-id="f128e-196">The Course entity</span></span>

![課程實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="f128e-198">在*模型*資料夾中，建立*Course.cs* ，並以下列程式碼取代現有的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f128e-198">In the *Models* folder, create *Course.cs* and replace the existing code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="f128e-199">`Enrollments`屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="f128e-199">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="f128e-200">A`Course`可以與任意數目的相關實體`Enrollment`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-200">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="f128e-201">我們將更多有關`DatabaseGenerated`屬性[之後的教學課程](complex-data-model.md)本系列。</span><span class="sxs-lookup"><span data-stu-id="f128e-201">We'll say more about the `DatabaseGenerated` attribute in a [later tutorial](complex-data-model.md) in this series.</span></span> <span data-ttu-id="f128e-202">基本上，此屬性可讓您輸入的主索引鍵的課程，而不是需要產生它的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-202">Basically, this attribute lets you enter the primary key for the course rather than having the database generate it.</span></span>

## <a name="create-the-database-context"></a><span data-ttu-id="f128e-203">建立的資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f128e-203">Create the Database Context</span></span>

<span data-ttu-id="f128e-204">協調對給定的資料模型的 Entity Framework 功能的主要類別是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="f128e-204">The main class that coordinates Entity Framework functionality for a given data model is the database context class.</span></span> <span data-ttu-id="f128e-205">若要建立此類別，您可以從 `Microsoft.EntityFrameworkCore.DbContext` 類別來衍生。</span><span class="sxs-lookup"><span data-stu-id="f128e-205">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span> <span data-ttu-id="f128e-206">在程式碼中指定資料模型中包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-206">In your code you specify which entities are included in the data model.</span></span> <span data-ttu-id="f128e-207">您也可以自訂某些 Entity Framework 的行為。</span><span class="sxs-lookup"><span data-stu-id="f128e-207">You can also customize certain Entity Framework behavior.</span></span> <span data-ttu-id="f128e-208">在此專案中，類別會命名為`SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="f128e-208">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="f128e-209">在專案資料夾中，建立名為資料夾*資料*。</span><span class="sxs-lookup"><span data-stu-id="f128e-209">In the project folder, create a folder named *Data*.</span></span>

<span data-ttu-id="f128e-210">在*資料*資料夾建立新的類別檔案命名為*SchoolContext.cs*，並將範本程式碼取代下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f128e-210">In the *Data* folder create a new class file named *SchoolContext.cs*, and replace the template code with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

<span data-ttu-id="f128e-211">此程式碼建立`DbSet`每一個實體集的屬性。</span><span class="sxs-lookup"><span data-stu-id="f128e-211">This code creates a `DbSet` property for each entity set.</span></span> <span data-ttu-id="f128e-212">在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="f128e-212">In Entity Framework terminology, an entity set typically corresponds to a database table, and an entity corresponds to a row in the table.</span></span>

<span data-ttu-id="f128e-213">您可以省略`DbSet<Enrollment>`和`DbSet<Course>`陳述式，它會運作方式相同。</span><span class="sxs-lookup"><span data-stu-id="f128e-213">You could have omitted the `DbSet<Enrollment>` and `DbSet<Course>` statements and it would work the same.</span></span> <span data-ttu-id="f128e-214">Entity Framework 會將其包含隱含因為`Student`實體參考`Enrollment`實體和`Enrollment`實體參考`Course`實體。</span><span class="sxs-lookup"><span data-stu-id="f128e-214">The Entity Framework would include them implicitly because the `Student` entity references the `Enrollment` entity and the `Enrollment` entity references the `Course` entity.</span></span>

<span data-ttu-id="f128e-215">EF 建立資料庫時，會建立具有相同名稱的資料表`DbSet`屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="f128e-215">When the database is created, EF creates tables that have names the same as the `DbSet` property names.</span></span> <span data-ttu-id="f128e-216">集合的屬性名稱通常是複數形式 （學生而不是學生），但開發人員不同意有關不論是否 pluralized 資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="f128e-216">Property names for collections are typically plural (Students rather than Student), but developers disagree about whether table names should be pluralized or not.</span></span> <span data-ttu-id="f128e-217">如需這些教學課程您都將指定的 DbContext 單數資料表名稱來覆寫預設行為。</span><span class="sxs-lookup"><span data-stu-id="f128e-217">For these tutorials you'll override the default behavior by specifying singular table names in the DbContext.</span></span> <span data-ttu-id="f128e-218">若要這樣做，請加入下列反白顯示的程式碼的最後一個 DbSet 屬性之後。</span><span class="sxs-lookup"><span data-stu-id="f128e-218">To do that, add the following highlighted code after the last DbSet property.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a><span data-ttu-id="f128e-219">暫存器具有相依性插入的內容</span><span class="sxs-lookup"><span data-stu-id="f128e-219">Register the context with dependency injection</span></span>

<span data-ttu-id="f128e-220">實作 ASP.NET Core[相依性插入](../../fundamentals/dependency-injection.md)預設。</span><span class="sxs-lookup"><span data-stu-id="f128e-220">ASP.NET Core implements [dependency injection](../../fundamentals/dependency-injection.md) by default.</span></span> <span data-ttu-id="f128e-221">在應用程式啟動時的相依性插入會註冊服務 （例如 EF 資料庫內容中）。</span><span class="sxs-lookup"><span data-stu-id="f128e-221">Services (such as the EF database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="f128e-222">需要這些服務 （例如 MVC 控制器） 的元件會提供這些服務，透過建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="f128e-222">Components that require these services (such as MVC controllers) are provided these services via constructor parameters.</span></span> <span data-ttu-id="f128e-223">您會看到的內容執行個體取得稍後在本教學課程的控制器建構函式程式碼。</span><span class="sxs-lookup"><span data-stu-id="f128e-223">You'll see the controller constructor code that gets a context instance later in this tutorial.</span></span>

<span data-ttu-id="f128e-224">若要註冊`SchoolContext`為服務時，開啟*Startup.cs*，並加入要反白顯示的行`ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-224">To register `SchoolContext` as a service, open *Startup.cs*, and add the highlighted lines to the `ConfigureServices` method.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

<span data-ttu-id="f128e-225">連接字串的名稱會傳遞至內容所呼叫的方法上`DbContextOptionsBuilder`物件。</span><span class="sxs-lookup"><span data-stu-id="f128e-225">The name of the connection string is passed in to the context by calling a method on a `DbContextOptionsBuilder` object.</span></span> <span data-ttu-id="f128e-226">本機開發， [ASP.NET Core 組態系統](../../fundamentals/configuration.md)讀取連接字串從*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="f128e-226">For local development, the [ASP.NET Core configuration system](../../fundamentals/configuration.md) reads the connection string from the *appsettings.json* file.</span></span>

<span data-ttu-id="f128e-227">新增`using`陳述式`ContosoUniversity.Data`和`Microsoft.EntityFrameworkCore`命名空間，然後建置專案。</span><span class="sxs-lookup"><span data-stu-id="f128e-227">Add `using` statements for `ContosoUniversity.Data`  and `Microsoft.EntityFrameworkCore` namespaces, and then build the project.</span></span>

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

<span data-ttu-id="f128e-228">開啟*appsettings.json*檔案，然後加入連接字串，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="f128e-228">Open the *appsettings.json* file and add a connection string as shown in the following example.</span></span>

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a><span data-ttu-id="f128e-229">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="f128e-229">SQL Server Express LocalDB</span></span>

<span data-ttu-id="f128e-230">連接字串會指定 SQL Server LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-230">The connection string specifies a SQL Server LocalDB database.</span></span> <span data-ttu-id="f128e-231">LocalDB 是輕量版 SQL Server Express Database Engine 和適用於應用程式開發，而不是生產環境使用。</span><span class="sxs-lookup"><span data-stu-id="f128e-231">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for application development, not production use.</span></span> <span data-ttu-id="f128e-232">LocalDB 會視需要啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="f128e-232">LocalDB starts on demand and runs in user mode, so there is no complex configuration.</span></span> <span data-ttu-id="f128e-233">根據預設，建立 LocalDB *.mdf*資料庫中的檔案`C:/Users/<user>`目錄。</span><span class="sxs-lookup"><span data-stu-id="f128e-233">By default, LocalDB creates *.mdf* database files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-database-with-test-data"></a><span data-ttu-id="f128e-234">加入程式碼以初始化測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="f128e-234">Add code to initialize the database with test data</span></span>

<span data-ttu-id="f128e-235">Entity Framework 會為您建立空的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-235">The Entity Framework will create an empty database for you.</span></span>  <span data-ttu-id="f128e-236">本節中，您可以撰寫以填入測試資料建立資料庫之後呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-236">In this section, you write a method that is called after the database is created in order to populate it with test data.</span></span>

<span data-ttu-id="f128e-237">在此您將使用`EnsureCreated`方法，以自動建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-237">Here you'll use the `EnsureCreated` method to automatically create the database.</span></span> <span data-ttu-id="f128e-238">在[之後的教學課程](migrations.md)您會看到如何使用 Code First 移轉，若要變更資料庫結構描述，而不是卸除並重新建立資料庫處理模型的變更。</span><span class="sxs-lookup"><span data-stu-id="f128e-238">In a [later tutorial](migrations.md) you'll see how to handle model changes by using Code First Migrations to change the database schema instead of dropping and re-creating the database.</span></span>

<span data-ttu-id="f128e-239">在*資料*資料夾中，建立新的類別檔案命名為*DbInitializer.cs*和範本程式碼取代為下列程式碼會使資料庫可在需要時建立負載測試到新的資料資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-239">In the *Data* folder, create a new class file named *DbInitializer.cs* and replace the template code with the following code, which causes a database to be created when needed and loads test data into the new database.</span></span>

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="f128e-240">如果在資料庫中，沒有任何的學生，而且如果沒有，則會假設資料庫新，而且必須是測試資料植入，則會檢查程式碼。</span><span class="sxs-lookup"><span data-stu-id="f128e-240">The code checks if there are any students in the database, and if not, it assumes the database is new and needs to be seeded with test data.</span></span>  <span data-ttu-id="f128e-241">它將測試資料載入至陣列而非`List<T>`集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="f128e-241">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="f128e-242">在*Program.cs*，修改`Main`方法來啟動應用程式上執行以下作業：</span><span class="sxs-lookup"><span data-stu-id="f128e-242">In *Program.cs*, modify the `Main` method to do the following on application startup:</span></span>

* <span data-ttu-id="f128e-243">從相依性插入容器中取得的資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="f128e-243">Get a database context instance from the dependency injection container.</span></span>
* <span data-ttu-id="f128e-244">呼叫種子方法，將內容傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="f128e-244">Call the seed method, passing to it the context.</span></span>
* <span data-ttu-id="f128e-245">Seed 方法完成時處置內容。</span><span class="sxs-lookup"><span data-stu-id="f128e-245">Dispose the context when the seed method is done.</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

<span data-ttu-id="f128e-246">新增`using`陳述式：</span><span class="sxs-lookup"><span data-stu-id="f128e-246">Add `using` statements:</span></span>

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

<span data-ttu-id="f128e-247">在較舊的教學課程中，您可能會看到類似的程式碼中`Configure`方法中的*Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="f128e-247">In older tutorials, you may see similar code in the `Configure` method in *Startup.cs*.</span></span> <span data-ttu-id="f128e-248">我們建議您改用`Configure`方法只是用來設定 要求管線。</span><span class="sxs-lookup"><span data-stu-id="f128e-248">We recommend that you use the `Configure` method only to set up the request pipeline.</span></span> <span data-ttu-id="f128e-249">應用程式啟動程式碼屬於`Main`方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-249">Application startup code belongs in the `Main` method.</span></span>

<span data-ttu-id="f128e-250">現在首次執行應用程式，資料庫將會建立並植入的測試資料。</span><span class="sxs-lookup"><span data-stu-id="f128e-250">Now the first time you run the application, the database will be created and seeded with test data.</span></span> <span data-ttu-id="f128e-251">每當您變更您的資料模型，您可以刪除資料庫，更新您種子的方法，並重新開始新的資料庫具有相同的方式。</span><span class="sxs-lookup"><span data-stu-id="f128e-251">Whenever you change your data model, you can delete the database, update your seed method, and start afresh with a new database the same way.</span></span> <span data-ttu-id="f128e-252">在稍後的教學課程中，您會看到如何修改資料庫時的資料模型變更，而不需要刪除並重新建立它。</span><span class="sxs-lookup"><span data-stu-id="f128e-252">In later tutorials, you'll see how to modify the database when the data model changes, without deleting and re-creating it.</span></span>

## <a name="create-a-controller-and-views"></a><span data-ttu-id="f128e-253">建立控制器和檢視</span><span class="sxs-lookup"><span data-stu-id="f128e-253">Create a controller and views</span></span>

<span data-ttu-id="f128e-254">接下來，您將加入此 MVC 控制器和檢視，將會用來查詢及儲存資料的 EF，Visual Studio 中使用 scaffolding 引擎。</span><span class="sxs-lookup"><span data-stu-id="f128e-254">Next, you'll use the scaffolding engine in Visual Studio to add an MVC controller and views that will use EF to query and save data.</span></span>

<span data-ttu-id="f128e-255">CRUD 動作方法和檢視表的自動建立稱為 scaffolding。</span><span class="sxs-lookup"><span data-stu-id="f128e-255">The automatic creation of CRUD action methods and views is known as scaffolding.</span></span> <span data-ttu-id="f128e-256">Scaffolding 與程式碼產生的 scaffold 的程式碼是您可以進行修改以符合您自己的需求，而您通常不修改產生的程式碼的起點。</span><span class="sxs-lookup"><span data-stu-id="f128e-256">Scaffolding differs from code generation in that the scaffolded code is a starting point that you can modify to suit your own requirements, whereas you typically don't modify generated code.</span></span> <span data-ttu-id="f128e-257">當您需要自訂產生的程式碼時，您使用部分類別，或重新產生程式碼項目變更時。</span><span class="sxs-lookup"><span data-stu-id="f128e-257">When you need to customize generated code, you use partial classes or you regenerate the code when things change.</span></span>

* <span data-ttu-id="f128e-258">以滑鼠右鍵按一下**控制器**資料夾中的**方案總管 中**選取**新增 > 新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="f128e-258">Right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="f128e-259">在 [新增 MVC 相依性] 對話方塊中，選取 [基本相依性]，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f128e-259">In the **Add MVC Dependencies** dialog, select **Minimal Dependencies**, and select **Add**.</span></span>

  ![新增相依性](intro/_static/add-depend.png)

  <span data-ttu-id="f128e-261">Visual Studio 加入 scaffold 控制器所需的相依性。</span><span class="sxs-lookup"><span data-stu-id="f128e-261">Visual Studio adds the dependencies needed to scaffold a controller.</span></span> <span data-ttu-id="f128e-262">專案檔中的唯一變更是新增`Microsoft.VisualStudio.Web.CodeGeneration.Design`封裝。</span><span class="sxs-lookup"><span data-stu-id="f128e-262">The only change in the project file is the addition of the `Microsoft.VisualStudio.Web.CodeGeneration.Design` package.</span></span>

  <span data-ttu-id="f128e-263">A *ScaffoldingReadMe.txt*建立您可以刪除的檔案。</span><span class="sxs-lookup"><span data-stu-id="f128e-263">A *ScaffoldingReadMe.txt* file is created which you can delete.</span></span>

* <span data-ttu-id="f128e-264">同樣地，以滑鼠右鍵按一下**控制器**資料夾中的**方案總管 中**選取**新增 > 新的 Scaffold 項目**。</span><span class="sxs-lookup"><span data-stu-id="f128e-264">Once again, right-click the **Controllers** folder in **Solution Explorer** and select **Add > New Scaffolded Item**.</span></span>

* <span data-ttu-id="f128e-265">在**新增 Scaffold**對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f128e-265">In the **Add Scaffold** dialog box:</span></span>

  * <span data-ttu-id="f128e-266">選取**檢視，使用 Entity Framework 的 MVC 控制器**。</span><span class="sxs-lookup"><span data-stu-id="f128e-266">Select **MVC controller with views, using Entity Framework**.</span></span>

  * <span data-ttu-id="f128e-267">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="f128e-267">Click **Add**.</span></span>

* <span data-ttu-id="f128e-268">在**加入控制器**對話方塊：</span><span class="sxs-lookup"><span data-stu-id="f128e-268">In the **Add Controller** dialog box:</span></span>

  * <span data-ttu-id="f128e-269">在**模型類別**選取**學生**。</span><span class="sxs-lookup"><span data-stu-id="f128e-269">In **Model class** select **Student**.</span></span>

  * <span data-ttu-id="f128e-270">在**資料內容類別**選取**SchoolContext**。</span><span class="sxs-lookup"><span data-stu-id="f128e-270">In **Data context class** select **SchoolContext**.</span></span>

  * <span data-ttu-id="f128e-271">接受預設值**StudentsController**做為名稱。</span><span class="sxs-lookup"><span data-stu-id="f128e-271">Accept the default **StudentsController** as the name.</span></span>

  * <span data-ttu-id="f128e-272">按一下 [加入] 。</span><span class="sxs-lookup"><span data-stu-id="f128e-272">Click **Add**.</span></span>

  ![Scaffold Student](intro/_static/scaffold-student.png)

  <span data-ttu-id="f128e-274">當您按一下**新增**，Visual Studio scaffolding 引擎會建立*StudentsController.cs*檔案和一組檢視 (*.cshtml*檔案)，所以搭配控制器。</span><span class="sxs-lookup"><span data-stu-id="f128e-274">When you click **Add**, the Visual Studio scaffolding engine creates a *StudentsController.cs* file and a set of views (*.cshtml* files) that work with the controller.</span></span>

<span data-ttu-id="f128e-275">(Scaffolding 引擎也可以建立的資料庫內容，如果您不要手動建立第一次可以如同您稍早在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="f128e-275">(The scaffolding engine can also create the database context for you if you don't create it manually first as you did earlier for this tutorial.</span></span> <span data-ttu-id="f128e-276">您可以指定新的內容類別中**加入控制器**方塊右邊的加號**資料內容類別**。</span><span class="sxs-lookup"><span data-stu-id="f128e-276">You can specify a new context class in the **Add Controller** box by clicking the plus sign to the right of **Data context class**.</span></span>  <span data-ttu-id="f128e-277">Visual Studio 接著會建立您`DbContext`以及控制器和檢視類別。)</span><span class="sxs-lookup"><span data-stu-id="f128e-277">Visual Studio will then create your `DbContext` class as well as the controller and views.)</span></span>

<span data-ttu-id="f128e-278">您會發現控制器會採用`SchoolContext`做為建構函式參數。</span><span class="sxs-lookup"><span data-stu-id="f128e-278">You'll notice that the controller takes a `SchoolContext` as a constructor parameter.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

<span data-ttu-id="f128e-279">ASP.NET 相依性插入會負責傳遞的執行個體的`SchoolContext`到控制器。</span><span class="sxs-lookup"><span data-stu-id="f128e-279">ASP.NET dependency injection will take care of passing an instance of `SchoolContext` into the controller.</span></span> <span data-ttu-id="f128e-280">您可以設定於*Startup.cs*稍早檔案。</span><span class="sxs-lookup"><span data-stu-id="f128e-280">You configured that in the *Startup.cs* file earlier.</span></span>

<span data-ttu-id="f128e-281">控制器含有`Index`動作方法，會顯示所有學生的資料庫中。</span><span class="sxs-lookup"><span data-stu-id="f128e-281">The controller contains an `Index` action method, which displays all students in the database.</span></span> <span data-ttu-id="f128e-282">方法會取得一份學生來自學生實體集藉由讀取`Students`的資料庫內容執行個體的屬性：</span><span class="sxs-lookup"><span data-stu-id="f128e-282">The method gets a list of students from the Students entity set by reading the `Students` property of the database context instance:</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

<span data-ttu-id="f128e-283">稍後在本教學課程中，您將了解這段程式碼中的非同步程式設計項目。</span><span class="sxs-lookup"><span data-stu-id="f128e-283">You'll learn about the asynchronous programming elements in this code later in the tutorial.</span></span>

<span data-ttu-id="f128e-284">*Views/Students/Index.cshtml*檢視會顯示在資料表中的這份清單：</span><span class="sxs-lookup"><span data-stu-id="f128e-284">The *Views/Students/Index.cshtml* view displays this list in a table:</span></span>

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

<span data-ttu-id="f128e-285">按 CTRL + F5 執行專案，或選擇**偵錯 > 啟動但不偵錯**從功能表。</span><span class="sxs-lookup"><span data-stu-id="f128e-285">Press CTRL+F5 to run the project or choose **Debug > Start Without Debugging** from the menu.</span></span>

<span data-ttu-id="f128e-286">按一下以查看測試資料的學生 索引標籤，`DbInitializer.Initialize`插入的方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-286">Click the Students tab to see the test data that the `DbInitializer.Initialize` method inserted.</span></span> <span data-ttu-id="f128e-287">根據如何窄瀏覽器視窗，您會看到`Student`] 索引標籤頂端的頁面中，或您的連結，則必須按一下 [瀏覽圖示以查看連結右上角。</span><span class="sxs-lookup"><span data-stu-id="f128e-287">Depending on how narrow your browser window is, you'll see the `Student` tab link at the top of the page or you'll have to click the navigation icon in the upper right corner to see the link.</span></span>

![Contoso 大學首頁窄](intro/_static/home-page-narrow.png)

![學生索引頁](intro/_static/students-index.png)

## <a name="view-the-database"></a><span data-ttu-id="f128e-290">檢視的資料庫</span><span class="sxs-lookup"><span data-stu-id="f128e-290">View the Database</span></span>

<span data-ttu-id="f128e-291">當您啟動應用程式，`DbInitializer.Initialize`方法呼叫`EnsureCreated`。</span><span class="sxs-lookup"><span data-stu-id="f128e-291">When you started the application, the `DbInitializer.Initialize` method calls `EnsureCreated`.</span></span> <span data-ttu-id="f128e-292">EF 看到不時發生的任何資料庫，並因此建立一個，則的其餘部分`Initialize`方法的程式碼填入資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-292">EF saw that there was no database and so it created one, then the remainder of the `Initialize` method code populated the database with data.</span></span> <span data-ttu-id="f128e-293">您可以使用**SQL Server 物件總管**(SSOX) 在 Visual Studio 中檢視的資料庫。</span><span class="sxs-lookup"><span data-stu-id="f128e-293">You can use **SQL Server Object Explorer** (SSOX) to view the database in Visual Studio.</span></span>

<span data-ttu-id="f128e-294">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f128e-294">Close the browser.</span></span>

<span data-ttu-id="f128e-295">如果 SSOX 視窗尚未開啟，請選取 從**檢視**Visual Studio 中的功能表。</span><span class="sxs-lookup"><span data-stu-id="f128e-295">If the SSOX window isn't already open, select it from the **View** menu in Visual Studio.</span></span>

<span data-ttu-id="f128e-296">在 SSOX，按一下  **(localdb) \MSSQLLocalDB > 資料庫**，然後按一下資料庫名稱中的連接字串中的項目您*appsettings.json*檔案。</span><span class="sxs-lookup"><span data-stu-id="f128e-296">In SSOX, click **(localdb)\MSSQLLocalDB > Databases**, and then click the entry for the database name that is in the connection string in your *appsettings.json* file.</span></span>

<span data-ttu-id="f128e-297">展開**資料表**節點以查看您的資料庫中的資料表。</span><span class="sxs-lookup"><span data-stu-id="f128e-297">Expand the **Tables** node to see the tables in your database.</span></span>

![SSOX 中的資料表](intro/_static/ssox-tables.png)

<span data-ttu-id="f128e-299">以滑鼠右鍵按一下**學生**資料表，並按一下**檢視資料**以查看所建立的資料行和資料列插入資料表。</span><span class="sxs-lookup"><span data-stu-id="f128e-299">Right-click the **Student** table and click **View Data** to see the columns that were created and the rows that were inserted into the table.</span></span>

![學生 SSOX 中的資料表](intro/_static/ssox-student-table.png)

<span data-ttu-id="f128e-301">*.Mdf*和*.ldf*資料庫檔案位於*C:\Users\<您的使用者名稱 >*資料夾。</span><span class="sxs-lookup"><span data-stu-id="f128e-301">The *.mdf* and *.ldf* database files are in the *C:\Users\<yourusername>* folder.</span></span>

<span data-ttu-id="f128e-302">因為您正在撥打`EnsureCreated`在啟動應用程式執行初始設定式方法中，您現在無法進行變更以`Student`類別、 刪除資料庫、 執行一次，應用程式和資料庫會自動重新建立它們以符合您的變更。</span><span class="sxs-lookup"><span data-stu-id="f128e-302">Because you're calling `EnsureCreated` in the initializer method that runs on app start, you could now make a change to the `Student` class, delete the database, run the application again, and the database would automatically be re-created to match your change.</span></span> <span data-ttu-id="f128e-303">例如，如果您加入`EmailAddress`屬性`Student`類別，您看到新`EmailAddress`重新建立資料表中的資料行。</span><span class="sxs-lookup"><span data-stu-id="f128e-303">For example, if you add an `EmailAddress` property to the `Student` class, you'll see a new `EmailAddress` column in the re-created table.</span></span>

## <a name="conventions"></a><span data-ttu-id="f128e-304">慣例</span><span class="sxs-lookup"><span data-stu-id="f128e-304">Conventions</span></span>

<span data-ttu-id="f128e-305">您必須撰寫 Entity framework 可以為您建立完整的資料庫中的程式碼數量是最小的因為使用的慣例或讓 Entity Framework 的假設。</span><span class="sxs-lookup"><span data-stu-id="f128e-305">The amount of code you had to write in order for the Entity Framework to be able to create a complete database for you is minimal because of the use of conventions, or assumptions that the Entity Framework makes.</span></span>

* <span data-ttu-id="f128e-306">名稱`DbSet`屬性會用做資料表的名稱。</span><span class="sxs-lookup"><span data-stu-id="f128e-306">The names of `DbSet` properties are used as table names.</span></span> <span data-ttu-id="f128e-307">實體沒有參考`DbSet`屬性，實體類別名稱會當做資料表名稱。</span><span class="sxs-lookup"><span data-stu-id="f128e-307">For entities not referenced by a `DbSet` property, entity class names are used as table names.</span></span>

* <span data-ttu-id="f128e-308">實體屬性名稱會用於資料行名稱。</span><span class="sxs-lookup"><span data-stu-id="f128e-308">Entity property names are used for column names.</span></span>

* <span data-ttu-id="f128e-309">實體屬性會在名為 ID 或 classnameID 辨識為主索引鍵屬性。</span><span class="sxs-lookup"><span data-stu-id="f128e-309">Entity properties that are named ID or classnameID are recognized as primary key properties.</span></span>

* <span data-ttu-id="f128e-310">屬性會解譯為外部索引鍵屬性上，如果名稱為 *<navigation property name> <primary key property name>*  (例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`).</span><span class="sxs-lookup"><span data-stu-id="f128e-310">A property is interpreted as a foreign key property if it's named *<navigation property name><primary key property name>* (for example, `StudentID` for the `Student` navigation property since the `Student` entity's primary key is `ID`).</span></span> <span data-ttu-id="f128e-311">外部索引鍵屬性也只稱為 *<primary key property name>*  (例如，`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。</span><span class="sxs-lookup"><span data-stu-id="f128e-311">Foreign key properties can also be named simply *<primary key property name>* (for example, `EnrollmentID` since the `Enrollment` entity's primary key is `EnrollmentID`).</span></span>

<span data-ttu-id="f128e-312">傳統行為可以被覆寫。</span><span class="sxs-lookup"><span data-stu-id="f128e-312">Conventional behavior can be overridden.</span></span> <span data-ttu-id="f128e-313">例如，您可以明確指定資料表名稱，如稍早在本教學課程中您所見。</span><span class="sxs-lookup"><span data-stu-id="f128e-313">For example, you can explicitly specify table names, as you saw earlier in this tutorial.</span></span> <span data-ttu-id="f128e-314">您可以設定資料行名稱和主索引鍵或外部索引鍵，以設定任何屬性，您會發現在[之後的教學課程](complex-data-model.md)本系列。</span><span class="sxs-lookup"><span data-stu-id="f128e-314">And you can set column names and set any property as primary key or foreign key, as you'll see in a [later tutorial](complex-data-model.md) in this series.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="f128e-315">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="f128e-315">Asynchronous code</span></span>

<span data-ttu-id="f128e-316">非同步程式設計是預設的 ASP.NET Core 和 EF 核心模式。</span><span class="sxs-lookup"><span data-stu-id="f128e-316">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="f128e-317">Web 伺服器的有限的數目的執行緒可用，而且在高負載情況下的所有可用的執行緒可能正在使用中。</span><span class="sxs-lookup"><span data-stu-id="f128e-317">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="f128e-318">當發生這種情況時，伺服器無法處理新的要求，直到執行緒釋放。</span><span class="sxs-lookup"><span data-stu-id="f128e-318">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="f128e-319">使用同步程式碼，許多執行緒可能會將繫結起來雖然它們實際上並不執行任何工作，因為他們正在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="f128e-319">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="f128e-320">使用非同步程式碼，當處理程序在等候 I/O 完成，它的執行緒釋放針對伺服器將用於處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="f128e-320">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="f128e-321">如此一來，非同步程式碼可讓更有效率地使用伺服器資源，而且伺服器已啟用以處理更多流量不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="f128e-321">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="f128e-322">非同步程式碼會導致少量的額外負荷，在執行階段，但效能的衝擊並不顯著，同時針對高流量的情況低流量的情況下，可能的效能提升的很大。</span><span class="sxs-lookup"><span data-stu-id="f128e-322">Asynchronous code does introduce a small amount of overhead at run time, but for low traffic situations the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="f128e-323">下列程式碼，`async`關鍵字，`Task<T>`傳回值，`await`關鍵字和`ToListAsync`方法提供程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="f128e-323">In the following code, the `async` keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="f128e-324">`async`關鍵字會指示編譯器以產生組件的方法主體的回呼，並自動建立`Task<IActionResult>`傳回物件。</span><span class="sxs-lookup"><span data-stu-id="f128e-324">The `async` keyword tells the compiler to generate callbacks for parts of the method body and to automatically create the `Task<IActionResult>` object that is returned.</span></span>

* <span data-ttu-id="f128e-325">傳回型別`Task<IActionResult>`代表進行中的工作，且結果型別`IActionResult`。</span><span class="sxs-lookup"><span data-stu-id="f128e-325">The return type `Task<IActionResult>` represents ongoing work with a result of type `IActionResult`.</span></span>

* <span data-ttu-id="f128e-326">`await`關鍵字會導致編譯器分成兩個部分的方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-326">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="f128e-327">以非同步方式啟動的作業結束的第一個部分。</span><span class="sxs-lookup"><span data-stu-id="f128e-327">The first part ends with the operation that is started asynchronously.</span></span> <span data-ttu-id="f128e-328">第二個部分會放入作業完成時呼叫的回呼方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-328">The second part is put into a callback method that is called when the operation completes.</span></span>

* <span data-ttu-id="f128e-329">`ToListAsync`非同步版本`ToList`擴充方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-329">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="f128e-330">要注意當您撰寫非同步程式碼，使用 Entity Framework 的一些事項：</span><span class="sxs-lookup"><span data-stu-id="f128e-330">Some things to be aware of when you are writing asynchronous code that uses the Entity Framework:</span></span>

* <span data-ttu-id="f128e-331">只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="f128e-331">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="f128e-332">其中包含，例如`ToListAsync`， `SingleOrDefaultAsync`，和`SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="f128e-332">That includes, for example, `ToListAsync`, `SingleOrDefaultAsync`, and `SaveChangesAsync`.</span></span>  <span data-ttu-id="f128e-333">它不包含，例如，陳述式，只要變更`IQueryable`，例如`var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="f128e-333">It does not include, for example, statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>

* <span data-ttu-id="f128e-334">EF 內容不是安全執行緒： 不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="f128e-334">An EF context is not thread safe: don't try to do multiple operations in parallel.</span></span> <span data-ttu-id="f128e-335">當您呼叫任何非同步 EF 方法時，一律使用`await`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="f128e-335">When you call any async EF method, always use the `await` keyword.</span></span>

* <span data-ttu-id="f128e-336">如果您想要充分利用非同步程式碼的效能優點，請確定任何程式庫封裝，您只使用 （例如分頁），也使用非同步呼叫會造成查詢傳送至資料庫的任何 Entity Framework 方法。</span><span class="sxs-lookup"><span data-stu-id="f128e-336">If you want to take advantage of the performance benefits of async code, make sure that any library packages that you're using (such as for paging), also use async if they call any Entity Framework methods that cause queries to be sent to the database.</span></span>

<span data-ttu-id="f128e-337">如需在.NET 非同步程式設計的詳細資訊，請參閱[非同步概觀](https://docs.microsoft.com/dotnet/articles/standard/async)。</span><span class="sxs-lookup"><span data-stu-id="f128e-337">For more information about asynchronous programming in .NET, see [Async Overview](https://docs.microsoft.com/dotnet/articles/standard/async).</span></span>

## <a name="summary"></a><span data-ttu-id="f128e-338">總結</span><span class="sxs-lookup"><span data-stu-id="f128e-338">Summary</span></span>

<span data-ttu-id="f128e-339">現在您已建立簡單的應用程式使用的 Entity Framework Core 和 SQL Server Express LocalDB 來儲存和顯示資料。</span><span class="sxs-lookup"><span data-stu-id="f128e-339">You've now created a simple application that uses the Entity Framework Core and SQL Server Express LocalDB to store and display data.</span></span> <span data-ttu-id="f128e-340">在下列的教學課程中，您將學習如何執行基本 CRUD （建立、 讀取、 更新、 刪除） 作業。</span><span class="sxs-lookup"><span data-stu-id="f128e-340">In the following tutorial, you'll learn how to perform basic CRUD (create, read, update, delete) operations.</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="f128e-341">下一步</span><span class="sxs-lookup"><span data-stu-id="f128e-341">Next</span></span>](crud.md)  
