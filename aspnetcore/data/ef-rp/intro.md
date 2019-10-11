---
title: ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8
author: rick-anderson
description: 示範如何建立使用 Entity Framework Core 的 Razor 頁面應用程式
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 09/26/2019
uid: data/ef-rp/intro
ms.openlocfilehash: 01e507326ddd57057aa178ad3909fd4027a013fd
ms.sourcegitcommit: 7d3c6565dda6241eb13f9a8e1e1fd89b1cfe4d18
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/11/2019
ms.locfileid: "72259370"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a><span data-ttu-id="47b75-103">ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8</span><span class="sxs-lookup"><span data-stu-id="47b75-103">Razor Pages with Entity Framework Core in ASP.NET Core - Tutorial 1 of 8</span></span>

<span data-ttu-id="47b75-104">作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="47b75-104">By [Tom Dykstra](https://github.com/tdykstra) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="47b75-105">這是一系列教學課程中的第一篇，示範如何在[ASP.NET Core Razor Pages](xref:razor-pages/index)應用程式中使用 ENTITY FRAMEWORK （EF） Core。</span><span class="sxs-lookup"><span data-stu-id="47b75-105">This is the first in a series of tutorials that show how to use Entity Framework (EF) Core in an [ASP.NET Core Razor Pages](xref:razor-pages/index) app.</span></span> <span data-ttu-id="47b75-106">教學課程會為虛構的 Contoso 大學建置網站。</span><span class="sxs-lookup"><span data-stu-id="47b75-106">The tutorials build a web site for a fictional Contoso University.</span></span> <span data-ttu-id="47b75-107">網站包含學生入學許可、課程建立和講師指派等功能。</span><span class="sxs-lookup"><span data-stu-id="47b75-107">The site includes functionality such as student admission, course creation, and instructor assignments.</span></span>

[<span data-ttu-id="47b75-108">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-108">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="47b75-109">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="47b75-109">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47b75-110">必要條件</span><span class="sxs-lookup"><span data-stu-id="47b75-110">Prerequisites</span></span>

* <span data-ttu-id="47b75-111">若您是第一次使用 Razor Pages，請先前往[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start) 教學課程系列，再開始進行本教學課程。</span><span class="sxs-lookup"><span data-stu-id="47b75-111">If you're new to Razor Pages, go through the [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) tutorial series before starting this one.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a><span data-ttu-id="47b75-114">資料庫引擎</span><span class="sxs-lookup"><span data-stu-id="47b75-114">Database engines</span></span>

<span data-ttu-id="47b75-115">Visual Studio 說明會使用 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)，它是一種只在 Windows 上執行的 SQL Server Express 版本。</span><span class="sxs-lookup"><span data-stu-id="47b75-115">The Visual Studio instructions use [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb), a version of SQL Server Express that runs only on Windows.</span></span>

<span data-ttu-id="47b75-116">Visual Studio Code 說明則會使用 [SQLite](https://www.sqlite.org/)，它是一種跨平台的資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="47b75-116">The Visual Studio Code instructions use [SQLite](https://www.sqlite.org/), a cross-platform database engine.</span></span>

<span data-ttu-id="47b75-117">若您選擇使用 SQLite，請下載及安裝協力廠商工具來管理和檢視 SQLite 資料庫，例如 [DB Browser for SQLite](https://sqlitebrowser.org/)。</span><span class="sxs-lookup"><span data-stu-id="47b75-117">If you choose to use SQLite, download and install a third-party tool for managing and viewing a SQLite database, such as [DB Browser for SQLite](https://sqlitebrowser.org/).</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="47b75-118">疑難排解</span><span class="sxs-lookup"><span data-stu-id="47b75-118">Troubleshooting</span></span>

<span data-ttu-id="47b75-119">若您遇到無法解決的問題，請將您的程式碼與[已完成的專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)進行比較。</span><span class="sxs-lookup"><span data-stu-id="47b75-119">If you run into a problem you can't resolve, compare your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="47b75-120">取得協助的其中一種好方法是將問題張貼到 StackOverflow.com，並使用 [ASP.NET Core 標籤](https://stackoverflow.com/questions/tagged/asp.net-core)或 [EF Core 標籤](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="47b75-120">A good way to get help is by posting a question to StackOverflow.com, using the [ASP.NET Core tag](https://stackoverflow.com/questions/tagged/asp.net-core) or the [EF Core tag](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-sample-app"></a><span data-ttu-id="47b75-121">範例應用程式</span><span class="sxs-lookup"><span data-stu-id="47b75-121">The sample app</span></span>

<span data-ttu-id="47b75-122">在教學課程中建立的應用程式，是一個基本的大學網站。</span><span class="sxs-lookup"><span data-stu-id="47b75-122">The app built in these tutorials is a basic university web site.</span></span> <span data-ttu-id="47b75-123">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="47b75-123">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="47b75-124">以下是幾個在教學課程中建立的畫面。</span><span class="sxs-lookup"><span data-stu-id="47b75-124">Here are a few of the screens created in the tutorial.</span></span>

![Students [索引] 頁面](intro/_static/students-index30.png)

![Students [編輯] 頁面](intro/_static/student-edit30.png)

<span data-ttu-id="47b75-127">本網站的 UI 風格是以內建的專案範本為基礎。</span><span class="sxs-lookup"><span data-stu-id="47b75-127">The UI style of this site is based on the built-in project templates.</span></span> <span data-ttu-id="47b75-128">教學課程的重點在於如何使用 EF Core，而非如何自訂 UI。</span><span class="sxs-lookup"><span data-stu-id="47b75-128">The tutorial's focus is on how to use EF Core, not how to customize the UI.</span></span>

<span data-ttu-id="47b75-129">請遵循頁面頂端的連結來取得已完成專案的原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="47b75-129">Follow the link at the top of the page to get the source code for the completed project.</span></span> <span data-ttu-id="47b75-130">*cu30* 資料夾包含本教學課程 ASP.NET Core 3.0 版本的程式碼。</span><span class="sxs-lookup"><span data-stu-id="47b75-130">The *cu30* folder has the code for the ASP.NET Core 3.0 version of the tutorial.</span></span> <span data-ttu-id="47b75-131">您可以在 *cu30snapshots* 資料夾中找到反映教學課程 1 到 7 程式碼狀態的檔案。</span><span class="sxs-lookup"><span data-stu-id="47b75-131">Files that reflect the state of the code for tutorials 1-7 can be found in the *cu30snapshots* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-132">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-132">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="47b75-133">在下載已完成的專案後執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="47b75-133">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="47b75-134">刪除名稱中包含 *SQLite* 的三個檔案和一個資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-134">Delete three files and one folder that have *SQLite* in the name.</span></span>
* <span data-ttu-id="47b75-135">建置專案。</span><span class="sxs-lookup"><span data-stu-id="47b75-135">Build the project.</span></span>
* <span data-ttu-id="47b75-136">在套件管理器主控台 (PMC) 中，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="47b75-136">In Package Manager Console (PMC) run the following command:</span></span>

  ```powershell
  Update-Database
  ```

* <span data-ttu-id="47b75-137">執行專案來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-137">Run the project to seed the database.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-138">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-138">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="47b75-139">在下載已完成的專案後執行應用程式：</span><span class="sxs-lookup"><span data-stu-id="47b75-139">To run the app after downloading the completed project:</span></span>

* <span data-ttu-id="47b75-140">刪除 *ContosoUniversity.csproj*，並將 *ContosoUniversitySQLite.csproj* 重新命名為 *ContosoUniversity.csproj*。</span><span class="sxs-lookup"><span data-stu-id="47b75-140">Delete *ContosoUniversity.csproj*, and rename *ContosoUniversitySQLite.csproj* to *ContosoUniversity.csproj*.</span></span>
* <span data-ttu-id="47b75-141">刪除 *Startup.cs*，並將 *StartupSQLite.cs* 重新命名為 *Startup.cs*。</span><span class="sxs-lookup"><span data-stu-id="47b75-141">Delete *Startup.cs*, and rename *StartupSQLite.cs* to *Startup.cs*.</span></span>
* <span data-ttu-id="47b75-142">刪除 *appSettings.json*，並將 *appSettingsSQLite.json* 重新命名為 *appSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="47b75-142">Delete *appSettings.json*, and rename *appSettingsSQLite.json* to *appSettings.json*.</span></span>
* <span data-ttu-id="47b75-143">刪除 *Migrations* 資料夾，並將 *MigrationsSQL* 重新命名為 *Migrations*。</span><span class="sxs-lookup"><span data-stu-id="47b75-143">Delete the *Migrations* folder, and rename *MigrationsSQL* to *Migrations*.</span></span>
* <span data-ttu-id="47b75-144">建置專案。</span><span class="sxs-lookup"><span data-stu-id="47b75-144">Build the project.</span></span>
* <span data-ttu-id="47b75-145">在專案資料夾中的命令提示字元內，執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="47b75-145">At a command prompt in the project folder, run the following commands:</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* <span data-ttu-id="47b75-146">在您的 SQLite 工具中，執行下列 SQL 陳述式：</span><span class="sxs-lookup"><span data-stu-id="47b75-146">In your SQLite tool, run the following SQL statement:</span></span>

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* <span data-ttu-id="47b75-147">執行專案來植入資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-147">Run the project to seed the database.</span></span>

---

## <a name="create-the-web-app-project"></a><span data-ttu-id="47b75-148">建立 Web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="47b75-148">Create the web app project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-149">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-149">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47b75-150">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="47b75-150">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="47b75-151">選取 [ASP.NET Core Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="47b75-151">Select **ASP.NET Core Web Application**.</span></span>
* <span data-ttu-id="47b75-152">將專案命名為 *ContosoUniversity*。</span><span class="sxs-lookup"><span data-stu-id="47b75-152">Name the project *ContosoUniversity*.</span></span> <span data-ttu-id="47b75-153">使用與此名稱完全相符的名稱非常重要 (包括大寫)，這樣做可以讓命名空間在您複製和貼上程式碼時相符。</span><span class="sxs-lookup"><span data-stu-id="47b75-153">It's important to use this exact name including capitalization, so the namespaces match when code is copied and pasted.</span></span>
* <span data-ttu-id="47b75-154">在下拉式清單中選取 [.NET Core] 及 [ASP.NET Core 3.0]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="47b75-154">Select **.NET Core** and **ASP.NET Core 3.0** in the dropdowns, and then select **Web Application**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-155">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-155">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="47b75-156">在終端機中，巡覽至應建立專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-156">In a terminal, navigate to the folder in which the project folder should be created.</span></span>

* <span data-ttu-id="47b75-157">執行下列命令來建立 Razor Pages 專案並 `cd` 至新的專案資料夾：</span><span class="sxs-lookup"><span data-stu-id="47b75-157">Run the following commands to create a Razor Pages project and `cd` into the new project folder:</span></span>

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="47b75-158">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="47b75-158">Set up the site style</span></span>

<span data-ttu-id="47b75-159">更新 *Pages/Shared/_Layout.cshtml* 來設定網站頁首、頁尾和功能表：</span><span class="sxs-lookup"><span data-stu-id="47b75-159">Set up the site header, footer, and menu by updating *Pages/Shared/_Layout.cshtml*:</span></span>

* <span data-ttu-id="47b75-160">將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。</span><span class="sxs-lookup"><span data-stu-id="47b75-160">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="47b75-161">共出現三次。</span><span class="sxs-lookup"><span data-stu-id="47b75-161">There are three occurrences.</span></span>

* <span data-ttu-id="47b75-162">刪除 [Home] 和 [Privacy] 功能表項目，然後新增 [About]、[Students]、[Courses]、[Instructors] 和 [Departments] 的項目。</span><span class="sxs-lookup"><span data-stu-id="47b75-162">Delete the **Home** and **Privacy** menu entries, and add entries for **About**, **Students**, **Courses**, **Instructors**, and **Departments**.</span></span>

<span data-ttu-id="47b75-163">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="47b75-163">The changes are highlighted.</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

<span data-ttu-id="47b75-164">在 *Pages/Index.cshtml* 中，將檔案內容替換成下列程式碼，將 ASP.NET Core 相關文字取代成此應用程式的相關文字：</span><span class="sxs-lookup"><span data-stu-id="47b75-164">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET Core with text about this app:</span></span>

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

<span data-ttu-id="47b75-165">執行應用程式來驗證首頁是否正常顯示。</span><span class="sxs-lookup"><span data-stu-id="47b75-165">Run the app to verify that the home page appears.</span></span>

## <a name="the-data-model"></a><span data-ttu-id="47b75-166">資料模型</span><span class="sxs-lookup"><span data-stu-id="47b75-166">The data model</span></span>

<span data-ttu-id="47b75-167">下列各節會建立資料模型：</span><span class="sxs-lookup"><span data-stu-id="47b75-167">The following sections create a data model:</span></span>

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="47b75-169">學生可以註冊任何數量的課程，課程也能讓任意數量的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="47b75-169">A student can enroll in any number of courses, and a course can have any number of students enrolled in it.</span></span>

## <a name="the-student-entity"></a><span data-ttu-id="47b75-170">Student 實體</span><span class="sxs-lookup"><span data-stu-id="47b75-170">The Student entity</span></span>

![Student 實體圖表](intro/_static/student-entity.png)

* <span data-ttu-id="47b75-172">在專案資料夾中建立 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-172">Create a *Models* folder in the project folder.</span></span> 

* <span data-ttu-id="47b75-173">使用下列程式碼建立 *Models/Student.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-173">Create *Models/Student.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

<span data-ttu-id="47b75-174">`ID` 屬性會成為對應到此類別資料庫資料表的主索引鍵資料行。</span><span class="sxs-lookup"><span data-stu-id="47b75-174">The `ID` property becomes the primary key column of the database table that corresponds to this class.</span></span> <span data-ttu-id="47b75-175">EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。</span><span class="sxs-lookup"><span data-stu-id="47b75-175">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="47b75-176">因此 `Student` 類別主索引鍵的替代自動識別名稱為 `StudentID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-176">So the alternative automatically recognized name for the `Student` class primary key is `StudentID`.</span></span>

<span data-ttu-id="47b75-177">`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。</span><span class="sxs-lookup"><span data-stu-id="47b75-177">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="47b75-178">導覽屬性會保留與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-178">Navigation properties hold other entities that are related to this entity.</span></span> <span data-ttu-id="47b75-179">在這種情況下，`Student` 實體的 `Enrollments` 屬性會保留所有與該 Student 相關的 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-179">In this case, the `Enrollments` property of a `Student` entity holds all of the `Enrollment` entities that are related to that Student.</span></span> <span data-ttu-id="47b75-180">例如，若資料庫中的 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性便會包含這兩個 Enrollment 項目。</span><span class="sxs-lookup"><span data-stu-id="47b75-180">For example, if a Student row in the database has two related Enrollment rows, the `Enrollments` navigation property contains those two Enrollment entities.</span></span> 

<span data-ttu-id="47b75-181">在資料庫中，若 Enrollment 資料列的 StudentID 資料行包含學生的識別碼值，則該資料列便會與 Student 資料列相關。</span><span class="sxs-lookup"><span data-stu-id="47b75-181">In the database, an Enrollment row is related to a Student row if its StudentID column contains the student's ID value.</span></span> <span data-ttu-id="47b75-182">例如，假設某 Student 資料列的識別碼為 1。</span><span class="sxs-lookup"><span data-stu-id="47b75-182">For example, suppose a Student row has ID=1.</span></span> <span data-ttu-id="47b75-183">相關的 Enrollment 資料列將會擁有 StudentID = 1。</span><span class="sxs-lookup"><span data-stu-id="47b75-183">Related Enrollment rows will have StudentID = 1.</span></span> <span data-ttu-id="47b75-184">StudentID 是 Enrollment 資料表中的「外部索引鍵」。</span><span class="sxs-lookup"><span data-stu-id="47b75-184">StudentID is a *foreign key* in the Enrollment table.</span></span> 

<span data-ttu-id="47b75-185">`Enrollments` 屬性會定義為 `ICollection<Enrollment>`，因為可能會有多個相關的 Enrollment 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-185">The `Enrollments` property is defined as `ICollection<Enrollment>` because there may be multiple related Enrollment entities.</span></span> <span data-ttu-id="47b75-186">您可以使用其他集合型別，例如 `List<Enrollment>` 或 `HashSet<Enrollment>`。</span><span class="sxs-lookup"><span data-stu-id="47b75-186">You can use other collection types, such as `List<Enrollment>` or `HashSet<Enrollment>`.</span></span> <span data-ttu-id="47b75-187">如使用 `ICollection<Enrollment>`，EF Core 預設將建立 `HashSet<Enrollment>` 集合。</span><span class="sxs-lookup"><span data-stu-id="47b75-187">When `ICollection<Enrollment>` is used, EF Core creates a `HashSet<Enrollment>` collection by default.</span></span>

## <a name="the-enrollment-entity"></a><span data-ttu-id="47b75-188">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="47b75-188">The Enrollment entity</span></span>

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="47b75-190">使用下列程式碼建立 *Models/Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-190">Create *Models/Enrollment.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

<span data-ttu-id="47b75-191">`EnrollmentID` 屬性是主索引鍵；這個實體會使用 `classnameID` 模式，而非 `ID` 本身。</span><span class="sxs-lookup"><span data-stu-id="47b75-191">The `EnrollmentID` property is the primary key; this entity uses the `classnameID` pattern instead of `ID` by itself.</span></span> <span data-ttu-id="47b75-192">針對生產資料模型，請選擇一個模式並一致地使用它。</span><span class="sxs-lookup"><span data-stu-id="47b75-192">For a production data model, choose one pattern and use it consistently.</span></span> <span data-ttu-id="47b75-193">本教學課程同時使用兩者的方式只是為了示範兩者都可運作。</span><span class="sxs-lookup"><span data-stu-id="47b75-193">This tutorial uses both just to illustrate that both work.</span></span> <span data-ttu-id="47b75-194">在不使用 `classname` 的情況下使用 `ID` 可讓實作某些類型的資料模型變更更容易。</span><span class="sxs-lookup"><span data-stu-id="47b75-194">Using `ID` without `classname` makes it easier to implement some kinds of data model changes.</span></span>

<span data-ttu-id="47b75-195">`Grade` 屬性為 `enum`。</span><span class="sxs-lookup"><span data-stu-id="47b75-195">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="47b75-196">`Grade` 型別宣告後方的問號表示 `Grade` 屬性[可為 Null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)。</span><span class="sxs-lookup"><span data-stu-id="47b75-196">The question mark after the `Grade` type declaration indicates that the `Grade` property is [nullable](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/).</span></span> <span data-ttu-id="47b75-197">為 Null 的成績不同於成績為零&mdash;Null 表示成績未知或尚未指派。</span><span class="sxs-lookup"><span data-stu-id="47b75-197">A grade that's null is different from a zero grade&mdash;null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="47b75-198">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="47b75-198">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="47b75-199">一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-199">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span>

<span data-ttu-id="47b75-200">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="47b75-200">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="47b75-201">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="47b75-201">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="47b75-202">如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="47b75-202">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="47b75-203">例如，`StudentID` 是 `Student` 導覽屬性的外部索引鍵，因為 `Student` 實體的主索引鍵是 `ID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-203">For example,`StudentID` is the foreign key for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="47b75-204">外部索引鍵屬性也可命名為 `<primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="47b75-204">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="47b75-205">例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-205">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

## <a name="the-course-entity"></a><span data-ttu-id="47b75-206">Course 實體</span><span class="sxs-lookup"><span data-stu-id="47b75-206">The Course entity</span></span>

![Course 實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="47b75-208">使用下列程式碼建立 *Models/Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-208">Create *Models/Course.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

<span data-ttu-id="47b75-209">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="47b75-209">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="47b75-210">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="47b75-210">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="47b75-211">`DatabaseGenerated` 屬性可讓應用程式指定主索引鍵，而非讓資料庫產生它。</span><span class="sxs-lookup"><span data-stu-id="47b75-211">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the database generate it.</span></span>

<span data-ttu-id="47b75-212">建置專案以驗證沒有任何編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="47b75-212">Build the project to validate that there are no compiler errors.</span></span>

## <a name="scaffold-student-pages"></a><span data-ttu-id="47b75-213">Scaffold Student 頁面</span><span class="sxs-lookup"><span data-stu-id="47b75-213">Scaffold Student pages</span></span>

<span data-ttu-id="47b75-214">在本節中，您會使用 ASP.NET scaffolding 工具來產生：</span><span class="sxs-lookup"><span data-stu-id="47b75-214">In this section, you use the ASP.NET Core scaffolding tool to generate:</span></span>

* <span data-ttu-id="47b75-215">EF Core「內容」類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-215">An EF Core *context* class.</span></span> <span data-ttu-id="47b75-216">內容是協調指定資料模型 Entity Framework 功能的主類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-216">The context is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="47b75-217">它衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-217">It derives from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>
* <span data-ttu-id="47b75-218">處理 `Student` 實體建立、讀取、更新和刪除 (CRUD) 作業的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="47b75-218">Razor pages that handle Create, Read, Update, and Delete (CRUD) operations for the `Student` entity.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-219">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-219">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47b75-220">在 *Pages* 資料夾中建立 *Students* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-220">Create a *Students* folder in the *Pages* folder.</span></span>
* <span data-ttu-id="47b75-221">在 [方案總管] 中，以滑鼠右鍵按一下 *Page/Students* 資料夾，然後選取 [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="47b75-221">In **Solution Explorer**, right-click the *Pages/Students* folder and select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="47b75-222">在 [新增 Scaffold] 對話方塊中，選取 [Razor Pages using Entity Framework (CRUD)] \(使用 Entity Framework 的 Razor Pages (CRUD)\) > [新增]。</span><span class="sxs-lookup"><span data-stu-id="47b75-222">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>
* <span data-ttu-id="47b75-223">在 [使用 Entity Framework (CRUD) 新增 Razor Pages] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="47b75-223">In the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>
  * <span data-ttu-id="47b75-224">在 [模型類別] 下拉式清單中，選取 [學生 (ContosoUniversity.Models)]。</span><span class="sxs-lookup"><span data-stu-id="47b75-224">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
  * <span data-ttu-id="47b75-225">在 [資料內容類別] 資料列中，選取 **+** (加號)。</span><span class="sxs-lookup"><span data-stu-id="47b75-225">In the **Data context class** row, select the **+** (plus) sign.</span></span>
  * <span data-ttu-id="47b75-226">將資料內容的名稱從 *ContosoUniversity.Models.ContosoUniversityContext* 變更為 *ContosoUniversity.Data.SchoolContext*。</span><span class="sxs-lookup"><span data-stu-id="47b75-226">Change the data context name from *ContosoUniversity.Models.ContosoUniversityContext* to *ContosoUniversity.Data.SchoolContext*.</span></span>
  * <span data-ttu-id="47b75-227">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="47b75-227">Select **Add**.</span></span>

<span data-ttu-id="47b75-228">會自動安裝下列套件：</span><span class="sxs-lookup"><span data-stu-id="47b75-228">The following packages are automatically installed:</span></span>

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-229">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-229">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="47b75-230">執行下列 .NET Core CLI 命令來安裝必要的 NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="47b75-230">Run the following .NET Core CLI commands to install required NuGet packages:</span></span>
<!-- TO DO  After testing, Replace with
[!INCLUDE[](~/includes/includes/add-EF-NuGet-SQLite-CLI.md)]
remove dotnet tool install --global  below
 -->
  ```dotnetcli
  dotnet add package Microsoft.EntityFrameworkCore.SQLite
  dotnet add package Microsoft.EntityFrameworkCore.SqlServer
  dotnet add package Microsoft.EntityFrameworkCore.Design
  dotnet add package Microsoft.EntityFrameworkCore.Tools
  dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
  dotnet add package Microsoft.Extensions.Logging.Debug
  ```

  <span data-ttu-id="47b75-231">Microsoft.VisualStudio.Web.CodeGeneration.Design 套件是進行 scaffolding 時的必要項目。</span><span class="sxs-lookup"><span data-stu-id="47b75-231">The Microsoft.VisualStudio.Web.CodeGeneration.Design package is required for scaffolding.</span></span> <span data-ttu-id="47b75-232">雖然應用程式不會使用 SQL Server，scaffolding 工具仍需要 SQL Server 套件。</span><span class="sxs-lookup"><span data-stu-id="47b75-232">Although the app won't use SQL Server, the scaffolding tool needs the SQL Server package.</span></span>

* <span data-ttu-id="47b75-233">建立 *Pages/Students* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-233">Create a *Pages/Students* folder.</span></span>

* <span data-ttu-id="47b75-234">執行下列命令來安裝 [aspnet-codegenerator scaffolding 工具](xref:fundamentals/tools/dotnet-aspnet-codegenerator)。</span><span class="sxs-lookup"><span data-stu-id="47b75-234">Run the following command to install the [aspnet-codegenerator scaffolding tool](xref:fundamentals/tools/dotnet-aspnet-codegenerator).</span></span>

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* <span data-ttu-id="47b75-235">執行下列命令來 scaffold Student 頁面。</span><span class="sxs-lookup"><span data-stu-id="47b75-235">Run the following command to scaffold Student pages.</span></span>

  <span data-ttu-id="47b75-236">**在 Windows 上**</span><span class="sxs-lookup"><span data-stu-id="47b75-236">**On Windows**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  <span data-ttu-id="47b75-237">**在 macOS 或 Linux 上**</span><span class="sxs-lookup"><span data-stu-id="47b75-237">**On macOS or Linux**</span></span>

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

<span data-ttu-id="47b75-238">若您在上述步驟中遇到問題，請建置專案並重試 scaffold 步驟。</span><span class="sxs-lookup"><span data-stu-id="47b75-238">If you have a problem with the preceding step, build the project and retry the scaffold step.</span></span>

<span data-ttu-id="47b75-239">Scaffolding 流程：</span><span class="sxs-lookup"><span data-stu-id="47b75-239">The scaffolding process:</span></span>

* <span data-ttu-id="47b75-240">在 *Pages/Students* 資料夾中建立 Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="47b75-240">Creates Razor pages in the *Pages/Students* folder:</span></span>
  * <span data-ttu-id="47b75-241">*Create.cshtml* 和 *Create.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="47b75-241">*Create.cshtml* and *Create.cshtml.cs*</span></span>
  * <span data-ttu-id="47b75-242">*Delete.cshtml* 和 *Delete.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="47b75-242">*Delete.cshtml* and *Delete.cshtml.cs*</span></span>
  * <span data-ttu-id="47b75-243">*Details.cshtml* 和 *Details.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="47b75-243">*Details.cshtml* and *Details.cshtml.cs*</span></span>
  * <span data-ttu-id="47b75-244">*Edit.cshtml* 和 *Edit.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="47b75-244">*Edit.cshtml* and *Edit.cshtml.cs*</span></span>
  * <span data-ttu-id="47b75-245">*Index.cshtml* 和 *Index.cshtml.cs*</span><span class="sxs-lookup"><span data-stu-id="47b75-245">*Index.cshtml* and *Index.cshtml.cs*</span></span>
* <span data-ttu-id="47b75-246">建立 *Data/SchoolContext.cs*。</span><span class="sxs-lookup"><span data-stu-id="47b75-246">Creates *Data/SchoolContext.cs*.</span></span>
* <span data-ttu-id="47b75-247">將內容新增到 *Startup.cs* 中的相依性插入。</span><span class="sxs-lookup"><span data-stu-id="47b75-247">Adds the context to dependency injection in *Startup.cs*.</span></span>
* <span data-ttu-id="47b75-248">將資料庫連接字串新增到 *appsettings.json*。</span><span class="sxs-lookup"><span data-stu-id="47b75-248">Adds a database connection string to *appsettings.json*.</span></span>

## <a name="database-connection-string"></a><span data-ttu-id="47b75-249">資料庫連接字串</span><span class="sxs-lookup"><span data-stu-id="47b75-249">Database connection string</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-250">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-250">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="47b75-251">連接字串會指定 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。</span><span class="sxs-lookup"><span data-stu-id="47b75-251">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

<span data-ttu-id="47b75-252">LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。</span><span class="sxs-lookup"><span data-stu-id="47b75-252">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="47b75-253">根據預設，LocalDB 會在 `C:/Users/<user>` 目錄中建立 *.mdf* 檔案。</span><span class="sxs-lookup"><span data-stu-id="47b75-253">By default, LocalDB creates *.mdf* files in the `C:/Users/<user>` directory.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-254">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-254">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="47b75-255">將連接字串變更為指向名為 *CU.db* 的 SQLite 資料庫檔案：</span><span class="sxs-lookup"><span data-stu-id="47b75-255">Change the connection string to point to a SQLite database file named *CU.db*:</span></span>

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a><span data-ttu-id="47b75-256">更新資料庫內容類別</span><span class="sxs-lookup"><span data-stu-id="47b75-256">Update the database context class</span></span>

<span data-ttu-id="47b75-257">協調指定資料模型 EF Core 功能的主類別是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-257">The main class that coordinates EF Core functionality for a given data model is the database context class.</span></span> <span data-ttu-id="47b75-258">內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="47b75-258">The context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="47b75-259">內容會指定哪些實體會包含在資料模型中。</span><span class="sxs-lookup"><span data-stu-id="47b75-259">The context specifies which entities are included in the data model.</span></span> <span data-ttu-id="47b75-260">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="47b75-260">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="47b75-261">使用下列程式碼來更新 *SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-261">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

<span data-ttu-id="47b75-262">醒目標示的程式碼會為每一個實體集建立 [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="47b75-262">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="47b75-263">在 EF Core 用語中：</span><span class="sxs-lookup"><span data-stu-id="47b75-263">In EF Core terminology:</span></span>

* <span data-ttu-id="47b75-264">實體集通常會對應到資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="47b75-264">An entity set typically corresponds to a database table.</span></span>
* <span data-ttu-id="47b75-265">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="47b75-265">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="47b75-266">因為實體集會包含多個實體，所以 DBSet 屬性應為複數名稱。</span><span class="sxs-lookup"><span data-stu-id="47b75-266">Since an entity set contains multiple entities, the DBSet properties should be plural names.</span></span> <span data-ttu-id="47b75-267">因為 scaffolding 工具建立了 `Student` DBSet，所以此步驟會將它變更為複數的 `Students`。</span><span class="sxs-lookup"><span data-stu-id="47b75-267">Since the scaffolding tool created a`Student` DBSet, this step changes it to plural `Students`.</span></span> 

<span data-ttu-id="47b75-268">為了讓 Razor Pages 程式碼與新的 DBSet 名稱相符，請在整個專案中將 `_context.Student` 變更全域變更為 `_context.Students`。</span><span class="sxs-lookup"><span data-stu-id="47b75-268">To make the Razor Pages code match the new DBSet name, make a global change across the whole project of `_context.Student` to `_context.Students`.</span></span>  <span data-ttu-id="47b75-269">會有 8 次變更。</span><span class="sxs-lookup"><span data-stu-id="47b75-269">There are 8 occurrences.</span></span>

<span data-ttu-id="47b75-270">建置專案以確認沒有任何編譯器錯誤。</span><span class="sxs-lookup"><span data-stu-id="47b75-270">Build the project to verify there are no compiler errors.</span></span>

## <a name="startupcs"></a><span data-ttu-id="47b75-271">Startup.cs</span><span class="sxs-lookup"><span data-stu-id="47b75-271">Startup.cs</span></span>

<span data-ttu-id="47b75-272">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="47b75-272">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="47b75-273">服務 (例如 EF Core 資料庫內容) 會在應用程式啟動期間對相依性插入進行註冊。</span><span class="sxs-lookup"><span data-stu-id="47b75-273">Services (such as the EF Core database context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="47b75-274">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="47b75-274">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="47b75-275">取得資料庫內容執行個體的建構函式程式碼會顯示在本教學課程稍後部分。</span><span class="sxs-lookup"><span data-stu-id="47b75-275">The constructor code that gets a database context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="47b75-276">Scaffolding 工具會自動對相依性插入容器註冊內容類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-276">The scaffolding tool automatically registered the context class with the dependency injection container.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-277">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-277">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47b75-278">在 `ConfigureServices` 中，Scaffolder 會新增下列醒目提示行：</span><span class="sxs-lookup"><span data-stu-id="47b75-278">In `ConfigureServices`, the highlighted lines were added by the scaffolder:</span></span>

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-279">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-279">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="47b75-280">在 `ConfigureServices` 中，確認 Scaffolder 新增的程式碼會呼叫 `UseSqlite`。</span><span class="sxs-lookup"><span data-stu-id="47b75-280">In `ConfigureServices`, make sure the code added by the scaffolder calls `UseSqlite`.</span></span>

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

<span data-ttu-id="47b75-281">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="47b75-281">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="47b75-282">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="47b75-282">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="create-the-database"></a><span data-ttu-id="47b75-283">建立資料庫</span><span class="sxs-lookup"><span data-stu-id="47b75-283">Create the database</span></span>

<span data-ttu-id="47b75-284">若未存在資料庫，則請更新 *Program.cs* 來加以建立：</span><span class="sxs-lookup"><span data-stu-id="47b75-284">Update *Program.cs* to create the database if it doesn't exist:</span></span>

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

<span data-ttu-id="47b75-285">若已存在內容的資料庫，則 [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) 方法便不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="47b75-285">The [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) method takes no action if a database for the context exists.</span></span> <span data-ttu-id="47b75-286">若資料庫不存在，則它會建立資料庫和結構描述。</span><span class="sxs-lookup"><span data-stu-id="47b75-286">If no database exists, it creates the database and schema.</span></span> <span data-ttu-id="47b75-287">`EnsureCreated` 會啟用下列工作流程來處理資料模型變更：</span><span class="sxs-lookup"><span data-stu-id="47b75-287">`EnsureCreated` enables the following workflow for handling data model changes:</span></span>

* <span data-ttu-id="47b75-288">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-288">Delete the database.</span></span> <span data-ttu-id="47b75-289">任何現有的資料都會遺失。</span><span class="sxs-lookup"><span data-stu-id="47b75-289">Any existing data is lost.</span></span>
* <span data-ttu-id="47b75-290">變更資料模型。</span><span class="sxs-lookup"><span data-stu-id="47b75-290">Change the data model.</span></span> <span data-ttu-id="47b75-291">例如，新增 `EmailAddress` 欄位。</span><span class="sxs-lookup"><span data-stu-id="47b75-291">For example, add an `EmailAddress` field.</span></span>
* <span data-ttu-id="47b75-292">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-292">Run the app.</span></span>
* <span data-ttu-id="47b75-293">`EnsureCreated` 會使用新的結構描述來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-293">`EnsureCreated` creates a database with the new schema.</span></span>

<span data-ttu-id="47b75-294">只要您不需要保存資料，此工作流程在開發初期結構描述快速變化時的運作效果便會相當良好。</span><span class="sxs-lookup"><span data-stu-id="47b75-294">This workflow works well early in development when the schema is rapidly evolving, as long as you don't need to preserve data.</span></span> <span data-ttu-id="47b75-295">當資料輸入資料庫且需要進行保存時，狀況則會不同。</span><span class="sxs-lookup"><span data-stu-id="47b75-295">The situation is different when data that has been entered into the database needs to be preserved.</span></span> <span data-ttu-id="47b75-296">在這種情況下，請使用移轉。</span><span class="sxs-lookup"><span data-stu-id="47b75-296">When that is the case, use migrations.</span></span>

<span data-ttu-id="47b75-297">在教學課程系列的稍後部分，您會刪除 `EnsureCreated` 建立的資料庫並改為使用移轉。</span><span class="sxs-lookup"><span data-stu-id="47b75-297">Later in the tutorial series, you delete the database that was created by `EnsureCreated` and use migrations instead.</span></span> <span data-ttu-id="47b75-298">`EnsureCreated` 建立的資料庫無法使用移轉來更新。</span><span class="sxs-lookup"><span data-stu-id="47b75-298">A database that is created by `EnsureCreated` can't be updated by using migrations.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="47b75-299">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="47b75-299">Test the app</span></span>

* <span data-ttu-id="47b75-300">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-300">Run the app.</span></span>
* <span data-ttu-id="47b75-301">選取 [學生] 連結，然後選取 [新建]。</span><span class="sxs-lookup"><span data-stu-id="47b75-301">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="47b75-302">測試 [編輯]、[詳細資料] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="47b75-302">Test the Edit, Details, and Delete links.</span></span>

## <a name="seed-the-database"></a><span data-ttu-id="47b75-303">植入資料庫</span><span class="sxs-lookup"><span data-stu-id="47b75-303">Seed the database</span></span>

<span data-ttu-id="47b75-304">`EnsureCreated` 方法會建立空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-304">The `EnsureCreated` method creates an empty database.</span></span> <span data-ttu-id="47b75-305">本節會新增程式碼以使用測試資料來填入資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-305">This section adds code that populates the database with test data.</span></span>

<span data-ttu-id="47b75-306">使用下列程式碼建立 *Data/DbInitializer.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-306">Create *Data/DbInitializer.cs* with the following code:</span></span>

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  <span data-ttu-id="47b75-307">程式碼會檢查資料庫中是否有任何學生。</span><span class="sxs-lookup"><span data-stu-id="47b75-307">The code checks if there are any students in the database.</span></span> <span data-ttu-id="47b75-308">若沒有任何學生，它便會將測試資料新增到資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-308">If there are no students, it adds test data to the database.</span></span> <span data-ttu-id="47b75-309">它會以陣列的方式建立測試資料，而非 `List<T>` 集合，來最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="47b75-309">It creates the test data in arrays rather than `List<T>` collections to optimize performance.</span></span>

* <span data-ttu-id="47b75-310">在 *Program.cs* 中，將 `EnsureCreated` 呼叫替換成 `DbInitializer.Initialize` 呼叫：</span><span class="sxs-lookup"><span data-stu-id="47b75-310">In *Program.cs*, replace the `EnsureCreated` call with a `DbInitializer.Initialize` call:</span></span>

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-311">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-311">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="47b75-312">停止應用程式 (如果它正在執行)，並在**套件管理員主控台** (PMC) 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="47b75-312">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-313">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-313">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="47b75-314">若應用程式正在執行中，請停止它，然後刪除 *CU.db* 檔案。</span><span class="sxs-lookup"><span data-stu-id="47b75-314">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

* <span data-ttu-id="47b75-315">重新啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-315">Restart the app.</span></span>

* <span data-ttu-id="47b75-316">選取 Students 頁面來查看植入的資料。</span><span class="sxs-lookup"><span data-stu-id="47b75-316">Select the Students page to see the seeded data.</span></span>

## <a name="view-the-database"></a><span data-ttu-id="47b75-317">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="47b75-317">View the database</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-318">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-318">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47b75-319">從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="47b75-319">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
* <span data-ttu-id="47b75-320">在 SSOX 中，選取 [(localdb)\MSSQLLocalDB] > [資料庫] > [SchoolContext-{GUID}]。</span><span class="sxs-lookup"><span data-stu-id="47b75-320">In SSOX, select **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span> <span data-ttu-id="47b75-321">資料庫名稱是以您稍早所提供的內容名稱加上虛線和 GUID 所產生。</span><span class="sxs-lookup"><span data-stu-id="47b75-321">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span>
* <span data-ttu-id="47b75-322">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="47b75-322">Expand the **Tables** node.</span></span>
* <span data-ttu-id="47b75-323">以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="47b75-323">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>
* <span data-ttu-id="47b75-324">以滑鼠右鍵按一下 **Student** 資料表然後按一下 [檢視程式碼] 來查看 `Student` 模型對應到 `Student` 資料表結構描述的方式。</span><span class="sxs-lookup"><span data-stu-id="47b75-324">Right-click the **Student** table and click **View Code** to see how the `Student` model maps to the `Student` table schema.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-325">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-325">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="47b75-326">使用 SQLite 工具來檢視資料庫結構描述和植入的資料。</span><span class="sxs-lookup"><span data-stu-id="47b75-326">Use your SQLite tool to view the database schema and seeded data.</span></span> <span data-ttu-id="47b75-327">資料庫檔案名為 *CU.db* 且位於專案資料夾中。</span><span class="sxs-lookup"><span data-stu-id="47b75-327">The database file is named *CU.db* and is located in the project folder.</span></span>

---

## <a name="asynchronous-code"></a><span data-ttu-id="47b75-328">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="47b75-328">Asynchronous code</span></span>

<span data-ttu-id="47b75-329">非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。</span><span class="sxs-lookup"><span data-stu-id="47b75-329">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="47b75-330">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="47b75-330">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="47b75-331">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="47b75-331">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="47b75-332">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="47b75-332">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="47b75-333">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="47b75-333">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="47b75-334">因此，非同步程式碼可以更有效率地使用伺服器資源，且伺服器可處理更多流量而不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="47b75-334">As a result, asynchronous code enables server resources to be used more efficiently, and the server can handle more traffic without delays.</span></span>

<span data-ttu-id="47b75-335">非同步程式碼會在執行階段導致少量的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="47b75-335">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="47b75-336">在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="47b75-336">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="47b75-337">在下列程式碼中，[async](/dotnet/csharp/language-reference/keywords/async) 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法會使程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="47b75-337">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* <span data-ttu-id="47b75-338">`async` 關鍵字會指示編譯器：</span><span class="sxs-lookup"><span data-stu-id="47b75-338">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="47b75-339">為方法主體的組件產生回呼。</span><span class="sxs-lookup"><span data-stu-id="47b75-339">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="47b75-340">建立傳回的 [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) 物件。</span><span class="sxs-lookup"><span data-stu-id="47b75-340">Create the [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) object that's returned.</span></span>
* <span data-ttu-id="47b75-341">`Task<T>` 傳回型別代表正在進行的工作。</span><span class="sxs-lookup"><span data-stu-id="47b75-341">The `Task<T>` return type represents ongoing work.</span></span>
* <span data-ttu-id="47b75-342">`await` 關鍵字會導致編譯器將方法分成兩個組件。</span><span class="sxs-lookup"><span data-stu-id="47b75-342">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="47b75-343">第一個部分會與非同步啟動的作業一同結束。</span><span class="sxs-lookup"><span data-stu-id="47b75-343">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="47b75-344">第二個部分則會放入作業完成時呼叫的回呼方法中。</span><span class="sxs-lookup"><span data-stu-id="47b75-344">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="47b75-345">`ToListAsync` 是 `ToList` 擴充方法的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="47b75-345">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="47b75-346">若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="47b75-346">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="47b75-347">只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="47b75-347">Only statements that cause queries or commands to be sent to the database are executed asynchronously.</span></span> <span data-ttu-id="47b75-348">其中包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="47b75-348">That includes `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="47b75-349">不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="47b75-349">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="47b75-350">EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="47b75-350">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="47b75-351">若要利用非同步程式碼所帶來的效能利益，請驗證該程式庫套件 (例如用於分頁) 在呼叫傳送查詢到資料庫的 EF Core 方法時使用 async。</span><span class="sxs-lookup"><span data-stu-id="47b75-351">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the database.</span></span>

<span data-ttu-id="47b75-352">如需非同步方法的詳細資訊，請參閱 [Async 概觀](/dotnet/standard/async)和[使用 Async 和 Await 設計非同步程式](/dotnet/csharp/programming-guide/concepts/async/)。</span><span class="sxs-lookup"><span data-stu-id="47b75-352">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

## <a name="next-steps"></a><span data-ttu-id="47b75-353">後續步驟</span><span class="sxs-lookup"><span data-stu-id="47b75-353">Next steps</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="47b75-354">下一個教學課程</span><span class="sxs-lookup"><span data-stu-id="47b75-354">Next tutorial</span></span>](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="47b75-355">Contoso 大學的 Web 應用程式範例將示範如何以 Entity Framework (EF) Core 來建立 ASP.NET Core Razor Pages 應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-355">The Contoso University sample web app demonstrates how to create an ASP.NET Core Razor Pages app using Entity Framework (EF) Core.</span></span>

<span data-ttu-id="47b75-356">這個範例應用程式是虛構的 Contoso 大學網站。</span><span class="sxs-lookup"><span data-stu-id="47b75-356">The sample app is a web site for a fictional Contoso University.</span></span> <span data-ttu-id="47b75-357">其中包括的功能有學生入學許可、課程建立、教師指派。</span><span class="sxs-lookup"><span data-stu-id="47b75-357">It includes functionality such as student admission, course creation, and instructor assignments.</span></span> <span data-ttu-id="47b75-358">此頁面是說明如何建立 Contoso 大學範例應用程式教學課程系列中的第一頁。</span><span class="sxs-lookup"><span data-stu-id="47b75-358">This page is the first in a series of tutorials that explain how to build the Contoso University sample app.</span></span>

[<span data-ttu-id="47b75-359">下載或檢視已完成的應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-359">Download or view the completed app.</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) <span data-ttu-id="47b75-360">[下載指示](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="47b75-360">[Download instructions](xref:index#how-to-download-a-sample).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="47b75-361">必要條件</span><span class="sxs-lookup"><span data-stu-id="47b75-361">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-362">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-362">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-363">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-363">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

<span data-ttu-id="47b75-364">熟悉 [Razor 頁面](xref:razor-pages/index)。</span><span class="sxs-lookup"><span data-stu-id="47b75-364">Familiarity with [Razor Pages](xref:razor-pages/index).</span></span> <span data-ttu-id="47b75-365">新進程式設計人員應先完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，再開始此系列。</span><span class="sxs-lookup"><span data-stu-id="47b75-365">New programmers should complete [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) before starting this series.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="47b75-366">疑難排解</span><span class="sxs-lookup"><span data-stu-id="47b75-366">Troubleshooting</span></span>

<span data-ttu-id="47b75-367">如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。</span><span class="sxs-lookup"><span data-stu-id="47b75-367">If you run into a problem you can't resolve, you can generally find the solution by comparing your code to the [completed project](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples).</span></span> <span data-ttu-id="47b75-368">取得協助的好方法是將問題公佈到 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 以詢問 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。</span><span class="sxs-lookup"><span data-stu-id="47b75-368">A good way to get help is by posting a question to [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) for [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) or [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core).</span></span>

## <a name="the-contoso-university-web-app"></a><span data-ttu-id="47b75-369">Contoso 大學 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="47b75-369">The Contoso University web app</span></span>

<span data-ttu-id="47b75-370">在教學課程中建立的應用程式，是一個基本的大學網站。</span><span class="sxs-lookup"><span data-stu-id="47b75-370">The app built in these tutorials is a basic university web site.</span></span>

<span data-ttu-id="47b75-371">使用者可以檢視和更新學生、課程和教師資訊。</span><span class="sxs-lookup"><span data-stu-id="47b75-371">Users can view and update student, course, and instructor information.</span></span> <span data-ttu-id="47b75-372">以下是幾個在教學課程中建立的畫面。</span><span class="sxs-lookup"><span data-stu-id="47b75-372">Here are a few of the screens created in the tutorial.</span></span>

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

<span data-ttu-id="47b75-375">此網站的 UI 樣式接近內建範本所產生的內容。</span><span class="sxs-lookup"><span data-stu-id="47b75-375">The UI style of this site is close to what's generated by the built-in templates.</span></span> <span data-ttu-id="47b75-376">教學課程聚焦於 EF Core 與 Razor 頁面，而不是 UI。</span><span class="sxs-lookup"><span data-stu-id="47b75-376">The tutorial focus is on EF Core with Razor Pages, not the UI.</span></span>

## <a name="create-the-contosouniversity-razor-pages-web-app"></a><span data-ttu-id="47b75-377">建立 ContosoUniversity Razor Pages Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="47b75-377">Create the ContosoUniversity Razor Pages web app</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-378">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-378">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47b75-379">從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="47b75-379">From the Visual Studio **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="47b75-380">建立新的 ASP.NET Core Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-380">Create a new ASP.NET Core Web Application.</span></span> <span data-ttu-id="47b75-381">將專案命名為 **ContosoUniversity**。</span><span class="sxs-lookup"><span data-stu-id="47b75-381">Name the project **ContosoUniversity**.</span></span> <span data-ttu-id="47b75-382">請務必將專案命名為 *ContosoUniversity*，當您複製/貼上程式碼時，命名空間才會相符。</span><span class="sxs-lookup"><span data-stu-id="47b75-382">It's important to name the project *ContosoUniversity* so the namespaces match when code is copy/pasted.</span></span>
* <span data-ttu-id="47b75-383">在下拉式清單中選取 [ASP.NET Core 2.1]，然後選取 [Web 應用程式]。</span><span class="sxs-lookup"><span data-stu-id="47b75-383">Select **ASP.NET Core 2.1** in the dropdown, and then select **Web Application**.</span></span>

<span data-ttu-id="47b75-384">如需上述步驟的影像，請參閱[ 建立 Razor Web 應用程式](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app)。</span><span class="sxs-lookup"><span data-stu-id="47b75-384">For images of the preceding steps, see [Create a Razor web app](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app).</span></span>
<span data-ttu-id="47b75-385">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-385">Run the app.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-386">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-386">Visual Studio Code</span></span>](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

## <a name="set-up-the-site-style"></a><span data-ttu-id="47b75-387">設定網站樣式</span><span class="sxs-lookup"><span data-stu-id="47b75-387">Set up the site style</span></span>

<span data-ttu-id="47b75-388">一些變更設定了網站的功能表、配置和首頁。</span><span class="sxs-lookup"><span data-stu-id="47b75-388">A few changes set up the site menu, layout, and home page.</span></span> <span data-ttu-id="47b75-389">使用下列變更更新 *Pages/Shared/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="47b75-389">Update *Pages/Shared/_Layout.cshtml* with the following changes:</span></span>

* <span data-ttu-id="47b75-390">將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。</span><span class="sxs-lookup"><span data-stu-id="47b75-390">Change each occurrence of "ContosoUniversity" to "Contoso University".</span></span> <span data-ttu-id="47b75-391">共出現三次。</span><span class="sxs-lookup"><span data-stu-id="47b75-391">There are three occurrences.</span></span>

* <span data-ttu-id="47b75-392">為 **Students**、**Courses**、**Instructors**、**Departments** 新增功能表項目，並刪除 **Contact** 功能表項目。</span><span class="sxs-lookup"><span data-stu-id="47b75-392">Add menu entries for **Students**, **Courses**, **Instructors**, and **Departments**, and delete the **Contact** menu entry.</span></span>

<span data-ttu-id="47b75-393">所做的變更已醒目標示。</span><span class="sxs-lookup"><span data-stu-id="47b75-393">The changes are highlighted.</span></span> <span data-ttu-id="47b75-394">(所有標記都「不會」顯示。)</span><span class="sxs-lookup"><span data-stu-id="47b75-394">(All the markup is *not* displayed.)</span></span>

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

<span data-ttu-id="47b75-395">在 *Pages/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：</span><span class="sxs-lookup"><span data-stu-id="47b75-395">In *Pages/Index.cshtml*, replace the contents of the file with the following code to replace the text about ASP.NET and MVC with text about this app:</span></span>

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a><span data-ttu-id="47b75-396">建立資料模型</span><span class="sxs-lookup"><span data-stu-id="47b75-396">Create the data model</span></span>

<span data-ttu-id="47b75-397">為 Contoso 大學應用程式建立實體類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-397">Create entity classes for the Contoso University app.</span></span> <span data-ttu-id="47b75-398">從下列三個實體開始：</span><span class="sxs-lookup"><span data-stu-id="47b75-398">Start with the following three entities:</span></span>

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

<span data-ttu-id="47b75-400">在 `Student` 和 `Enrollment` 實體之間會有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="47b75-400">There's a one-to-many relationship between `Student` and `Enrollment` entities.</span></span> <span data-ttu-id="47b75-401">在 `Course` 和 `Enrollment` 實體之間會有一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="47b75-401">There's a one-to-many relationship between `Course` and `Enrollment` entities.</span></span> <span data-ttu-id="47b75-402">一位學生可註冊任何數目的課程。</span><span class="sxs-lookup"><span data-stu-id="47b75-402">A student can enroll in any number of courses.</span></span> <span data-ttu-id="47b75-403">一堂課程可以讓任意數目的學生註冊。</span><span class="sxs-lookup"><span data-stu-id="47b75-403">A course can have any number of students enrolled in it.</span></span>

<span data-ttu-id="47b75-404">在下列一節中，將為這些實體建立各自的類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-404">In the following sections, a class for each one of these entities is created.</span></span>

### <a name="the-student-entity"></a><span data-ttu-id="47b75-405">Student 實體</span><span class="sxs-lookup"><span data-stu-id="47b75-405">The Student entity</span></span>

![Student 實體圖表](intro/_static/student-entity.png)

<span data-ttu-id="47b75-407">建立 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-407">Create a *Models* folder.</span></span> <span data-ttu-id="47b75-408">在 *Models* 資料夾中，用下列程式碼建立一個名為 *Student.cs* 的類別檔案：</span><span class="sxs-lookup"><span data-stu-id="47b75-408">In the *Models* folder, create a class file named *Student.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

<span data-ttu-id="47b75-409">`ID` 屬性會成為資料庫 (DB) 資料表中的主索引鍵資料行，並對應至這個類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-409">The `ID` property becomes the primary key column of the database (DB) table that corresponds to this class.</span></span> <span data-ttu-id="47b75-410">EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。</span><span class="sxs-lookup"><span data-stu-id="47b75-410">By default, EF Core interprets a property that's named `ID` or `classnameID` as the primary key.</span></span> <span data-ttu-id="47b75-411">在 `classnameID` 中，`classname` 是類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="47b75-411">In `classnameID`, `classname` is the name of the class.</span></span> <span data-ttu-id="47b75-412">另一個自動識別的主索引鍵是上述範例中的 `StudentID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-412">The alternative automatically recognized primary key is `StudentID` in the preceding example.</span></span>

<span data-ttu-id="47b75-413">`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。</span><span class="sxs-lookup"><span data-stu-id="47b75-413">The `Enrollments` property is a [navigation property](/ef/core/modeling/relationships).</span></span> <span data-ttu-id="47b75-414">導覽屬性會連結至與此實體相關的其他實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-414">Navigation properties link to other entities that are related to this entity.</span></span> <span data-ttu-id="47b75-415">在這個案例中，`Student entity` 中的 `Enrollments` 屬性會保有與 `Student` 相關的所有 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-415">In this case, the `Enrollments` property of a `Student entity` holds all of the `Enrollment` entities that are related to that `Student`.</span></span> <span data-ttu-id="47b75-416">例如，如果資料庫中 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性會包含這兩個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-416">For example, if a Student row in the DB has two related Enrollment rows, the `Enrollments` navigation property contains those two `Enrollment` entities.</span></span> <span data-ttu-id="47b75-417">相關的 `Enrollment` 資料列，包含該學生在 `StudentID` 資料行中的主索引鍵值。</span><span class="sxs-lookup"><span data-stu-id="47b75-417">A related `Enrollment` row is a row that contains that student's primary key value in the `StudentID` column.</span></span> <span data-ttu-id="47b75-418">例如，假設 student with ID=1 在 `Enrollment` 資料表中有兩個資料列。</span><span class="sxs-lookup"><span data-stu-id="47b75-418">For example, suppose the student with ID=1 has two rows in the `Enrollment` table.</span></span> <span data-ttu-id="47b75-419">`Enrollment` 資料表有兩個包含 `StudentID`=1 的資料列。</span><span class="sxs-lookup"><span data-stu-id="47b75-419">The `Enrollment` table has two rows with `StudentID` = 1.</span></span> <span data-ttu-id="47b75-420">`StudentID` 為 `Enrollment` 資料表中的外部索引鍵，會指定在 `Student` 資料表中的學生。</span><span class="sxs-lookup"><span data-stu-id="47b75-420">`StudentID` is a foreign key in the `Enrollment` table that specifies the student in the `Student` table.</span></span>

<span data-ttu-id="47b75-421">如果導覽屬性可以保存多個實體，該導覽屬性必然是清單類型，例如 `ICollection<T>`。</span><span class="sxs-lookup"><span data-stu-id="47b75-421">If a navigation property can hold multiple entities, the navigation property must be a list type, such as `ICollection<T>`.</span></span> <span data-ttu-id="47b75-422">您可以指定 `ICollection<T>`，或是如 `List<T>`、`HashSet<T>` 的類型。</span><span class="sxs-lookup"><span data-stu-id="47b75-422">`ICollection<T>` can be specified, or a type such as `List<T>` or `HashSet<T>`.</span></span> <span data-ttu-id="47b75-423">如使用 `ICollection<T>`，EF Core 預設將建立 `HashSet<T>` 集合。</span><span class="sxs-lookup"><span data-stu-id="47b75-423">When `ICollection<T>` is used, EF Core creates a `HashSet<T>` collection by default.</span></span> <span data-ttu-id="47b75-424">保存多個實體的導覽屬性，來自多對多和一對多關聯性。</span><span class="sxs-lookup"><span data-stu-id="47b75-424">Navigation properties that hold multiple entities come from many-to-many and one-to-many relationships.</span></span>

### <a name="the-enrollment-entity"></a><span data-ttu-id="47b75-425">Enrollment 實體</span><span class="sxs-lookup"><span data-stu-id="47b75-425">The Enrollment entity</span></span>

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

<span data-ttu-id="47b75-427">在 *Models* 資料夾中，用下列程式碼建立 *Enrollment.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-427">In the *Models* folder, create *Enrollment.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

<span data-ttu-id="47b75-428">`EnrollmentID` 屬性是主索引鍵。</span><span class="sxs-lookup"><span data-stu-id="47b75-428">The `EnrollmentID` property is the primary key.</span></span> <span data-ttu-id="47b75-429">這個實體會使用 `classnameID` 模式，而不是像 `Student` 實體一樣的 `ID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-429">This entity uses the `classnameID` pattern instead of `ID` like the `Student` entity.</span></span> <span data-ttu-id="47b75-430">開發人員通常會選擇其中一個模式，並使用於整個資料模型。</span><span class="sxs-lookup"><span data-stu-id="47b75-430">Typically developers choose one pattern and use it throughout the data model.</span></span> <span data-ttu-id="47b75-431">在稍後的教學課程中，將示範使用不具類別名稱的 ID，使得在數據模型中實作繼承更加簡單。</span><span class="sxs-lookup"><span data-stu-id="47b75-431">In a later tutorial, using ID without classname is shown to make it easier to implement inheritance in the data model.</span></span>

<span data-ttu-id="47b75-432">`Grade` 屬性為 `enum`。</span><span class="sxs-lookup"><span data-stu-id="47b75-432">The `Grade` property is an `enum`.</span></span> <span data-ttu-id="47b75-433">問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。</span><span class="sxs-lookup"><span data-stu-id="47b75-433">The question mark after the `Grade` type declaration indicates that the `Grade` property is nullable.</span></span> <span data-ttu-id="47b75-434">為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。</span><span class="sxs-lookup"><span data-stu-id="47b75-434">A grade that's null is different from a zero grade -- null means a grade isn't known or hasn't been assigned yet.</span></span>

<span data-ttu-id="47b75-435">`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。</span><span class="sxs-lookup"><span data-stu-id="47b75-435">The `StudentID` property is a foreign key, and the corresponding navigation property is `Student`.</span></span> <span data-ttu-id="47b75-436">一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-436">An `Enrollment` entity is associated with one `Student` entity, so the property contains a single `Student` entity.</span></span> <span data-ttu-id="47b75-437">`Student` 實體與 `Student.Enrollments` 導覽屬性不同，導覽屬性包含多個 `Enrollment` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-437">The `Student` entity differs from the `Student.Enrollments` navigation property, which contains multiple `Enrollment` entities.</span></span>

<span data-ttu-id="47b75-438">`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。</span><span class="sxs-lookup"><span data-stu-id="47b75-438">The `CourseID` property is a foreign key, and the corresponding navigation property is `Course`.</span></span> <span data-ttu-id="47b75-439">一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。</span><span class="sxs-lookup"><span data-stu-id="47b75-439">An `Enrollment` entity is associated with one `Course` entity.</span></span>

<span data-ttu-id="47b75-440">如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。</span><span class="sxs-lookup"><span data-stu-id="47b75-440">EF Core interprets a property as a foreign key if it's named `<navigation property name><primary key property name>`.</span></span> <span data-ttu-id="47b75-441">例如，`StudentID` 為 `Student` 導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-441">For example,`StudentID` for the `Student` navigation property, since the `Student` entity's primary key is `ID`.</span></span> <span data-ttu-id="47b75-442">外部索引鍵屬性也可命名為 `<primary key property name>`。</span><span class="sxs-lookup"><span data-stu-id="47b75-442">Foreign key properties can also be named `<primary key property name>`.</span></span> <span data-ttu-id="47b75-443">例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。</span><span class="sxs-lookup"><span data-stu-id="47b75-443">For example, `CourseID` since the `Course` entity's primary key is `CourseID`.</span></span>

### <a name="the-course-entity"></a><span data-ttu-id="47b75-444">Course 實體</span><span class="sxs-lookup"><span data-stu-id="47b75-444">The Course entity</span></span>

![Course 實體圖表](intro/_static/course-entity.png)

<span data-ttu-id="47b75-446">在 *Models* 資料夾中，用下列程式碼建立 *Course.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-446">In the *Models* folder, create *Course.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

<span data-ttu-id="47b75-447">`Enrollments` 屬性為導覽屬性。</span><span class="sxs-lookup"><span data-stu-id="47b75-447">The `Enrollments` property is a navigation property.</span></span> <span data-ttu-id="47b75-448">`Course` 實體可以與任何數量的 `Enrollment` 實體相關。</span><span class="sxs-lookup"><span data-stu-id="47b75-448">A `Course` entity can be related to any number of `Enrollment` entities.</span></span>

<span data-ttu-id="47b75-449">`DatabaseGenerated` 屬性 (attribute) 可讓應用程式指定主索引鍵，而不是讓 DB 來產生。</span><span class="sxs-lookup"><span data-stu-id="47b75-449">The `DatabaseGenerated` attribute allows the app to specify the primary key rather than having the DB generate it.</span></span>

## <a name="scaffold-the-student-model"></a><span data-ttu-id="47b75-450">Scaffold 學生模型</span><span class="sxs-lookup"><span data-stu-id="47b75-450">Scaffold the student model</span></span>

<span data-ttu-id="47b75-451">在本節中會 scaffold 學生模型。</span><span class="sxs-lookup"><span data-stu-id="47b75-451">In this section, the student model is scaffolded.</span></span> <span data-ttu-id="47b75-452">亦即 Scaffolding 工具會產生學生模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。</span><span class="sxs-lookup"><span data-stu-id="47b75-452">That is, the scaffolding tool produces pages for Create, Read, Update, and Delete (CRUD) operations for the student model.</span></span>

* <span data-ttu-id="47b75-453">建置專案。</span><span class="sxs-lookup"><span data-stu-id="47b75-453">Build the project.</span></span>
* <span data-ttu-id="47b75-454">建立 *Pages/Students* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b75-454">Create the *Pages/Students* folder.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-455">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-455">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="47b75-456">在 [方案總管] 中，以滑鼠右鍵按一下 *Pages/Students* 資料夾 > [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="47b75-456">In **Solution Explorer**, right click on the *Pages/Students* folder > **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="47b75-457">在 [新增 Scaffold] 對話方塊中，選取 [Razor Pages using Entity Framework (CRUD)] \(使用 Entity Framework 的 Razor Pages (CRUD)\) > [新增]。</span><span class="sxs-lookup"><span data-stu-id="47b75-457">In the **Add Scaffold** dialog, select **Razor Pages using Entity Framework (CRUD)** > **ADD**.</span></span>

<span data-ttu-id="47b75-458">完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：</span><span class="sxs-lookup"><span data-stu-id="47b75-458">Complete the **Add Razor Pages using Entity Framework (CRUD)** dialog:</span></span>

* <span data-ttu-id="47b75-459">在 [模型類別] 下拉式清單中，選取 [學生 (ContosoUniversity.Models)]。</span><span class="sxs-lookup"><span data-stu-id="47b75-459">In the **Model class** drop-down, select **Student (ContosoUniversity.Models)**.</span></span>
* <span data-ttu-id="47b75-460">在 [資料內容類別] 列中，選取 **+** (加號) 符號，並將產生的名稱變更為 **ContosoUniversity.Models.SchoolContext**。</span><span class="sxs-lookup"><span data-stu-id="47b75-460">In the **Data context class** row, select the **+** (plus) sign and change the generated name to **ContosoUniversity.Models.SchoolContext**.</span></span>
* <span data-ttu-id="47b75-461">在 [資料內容類別] 下拉式清單中，選取 **ContosoUniversity.Models.SchoolContext**</span><span class="sxs-lookup"><span data-stu-id="47b75-461">In the **Data context class** drop-down,  select **ContosoUniversity.Models.SchoolContext**</span></span>
* <span data-ttu-id="47b75-462">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="47b75-462">Select **Add**.</span></span>

![CRUD 對話方塊](intro/_static/s1.png)

<span data-ttu-id="47b75-464">如果您有上一個步驟的問題，請參閱 [Scaffold 影片模型](xref:tutorials/razor-pages/model#scaffold-the-movie-model)。</span><span class="sxs-lookup"><span data-stu-id="47b75-464">See [Scaffold the movie model](xref:tutorials/razor-pages/model#scaffold-the-movie-model) if you have a problem with the preceding step.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-465">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-465">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="47b75-466">執行下列命令來 Scaffold 學生模型。</span><span class="sxs-lookup"><span data-stu-id="47b75-466">Run the following commands to scaffold the student model.</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

<span data-ttu-id="47b75-467">隨即建立 Scaffold 處理序並變更下列檔案：</span><span class="sxs-lookup"><span data-stu-id="47b75-467">The scaffold process created and changed the following files:</span></span>

### <a name="files-created"></a><span data-ttu-id="47b75-468">建立的檔案</span><span class="sxs-lookup"><span data-stu-id="47b75-468">Files created</span></span>

* <span data-ttu-id="47b75-469">*Pages/Students* 建立、刪除、詳細資料、編輯、索引。</span><span class="sxs-lookup"><span data-stu-id="47b75-469">*Pages/Students* Create, Delete, Details, Edit, Index.</span></span>
* <span data-ttu-id="47b75-470">*Data/SchoolContext.cs*</span><span class="sxs-lookup"><span data-stu-id="47b75-470">*Data/SchoolContext.cs*</span></span>

### <a name="file-updates"></a><span data-ttu-id="47b75-471">檔案更新</span><span class="sxs-lookup"><span data-stu-id="47b75-471">File updates</span></span>

* <span data-ttu-id="47b75-472">*Startup.cs*：這個檔案的變更會於下一節詳述。</span><span class="sxs-lookup"><span data-stu-id="47b75-472">*Startup.cs* : Changes to this file are detailed in the next section.</span></span>
* <span data-ttu-id="47b75-473">*appsettings.json*：已新增用來連線到本機資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="47b75-473">*appsettings.json* : The connection string used to connect to a local database is added.</span></span>

## <a name="examine-the-context-registered-with-dependency-injection"></a><span data-ttu-id="47b75-474">檢查使用相依性插入所註冊的內容</span><span class="sxs-lookup"><span data-stu-id="47b75-474">Examine the context registered with dependency injection</span></span>

<span data-ttu-id="47b75-475">ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。</span><span class="sxs-lookup"><span data-stu-id="47b75-475">ASP.NET Core is built with [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="47b75-476">服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。</span><span class="sxs-lookup"><span data-stu-id="47b75-476">Services (such as the EF Core DB context) are registered with dependency injection during application startup.</span></span> <span data-ttu-id="47b75-477">接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。</span><span class="sxs-lookup"><span data-stu-id="47b75-477">Components that require these services (such as Razor Pages) are provided these services via constructor parameters.</span></span> <span data-ttu-id="47b75-478">取得資料庫內容執行個體的建構函式程式碼，在本教學課程稍後會示範。</span><span class="sxs-lookup"><span data-stu-id="47b75-478">The constructor code that gets a db context instance is shown later in the tutorial.</span></span>

<span data-ttu-id="47b75-479">Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。</span><span class="sxs-lookup"><span data-stu-id="47b75-479">The scaffolding tool automatically created a DB Context and registered it with the dependency injection container.</span></span>

<span data-ttu-id="47b75-480">檢查 *Startup.cs* 中的 `ConfigureServices` 方法。</span><span class="sxs-lookup"><span data-stu-id="47b75-480">Examine the `ConfigureServices` method in *Startup.cs*.</span></span> <span data-ttu-id="47b75-481">Scaffolder 已新增醒目標示行：</span><span class="sxs-lookup"><span data-stu-id="47b75-481">The highlighted line was added by the scaffolder:</span></span>

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

<span data-ttu-id="47b75-482">連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。</span><span class="sxs-lookup"><span data-stu-id="47b75-482">The name of the connection string is passed in to the context by calling a method on a [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) object.</span></span> <span data-ttu-id="47b75-483">作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。</span><span class="sxs-lookup"><span data-stu-id="47b75-483">For local development, the [ASP.NET Core configuration system](xref:fundamentals/configuration/index) reads the connection string from the *appsettings.json* file.</span></span>

## <a name="update-main"></a><span data-ttu-id="47b75-484">更新 main</span><span class="sxs-lookup"><span data-stu-id="47b75-484">Update main</span></span>

<span data-ttu-id="47b75-485">在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：</span><span class="sxs-lookup"><span data-stu-id="47b75-485">In *Program.cs*, modify the `Main` method to do the following:</span></span>

* <span data-ttu-id="47b75-486">從相依性插入容器中取得資料庫內容執行個體。</span><span class="sxs-lookup"><span data-stu-id="47b75-486">Get a DB context instance from the dependency injection container.</span></span>
* <span data-ttu-id="47b75-487">呼叫 [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)。</span><span class="sxs-lookup"><span data-stu-id="47b75-487">Call the  [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated).</span></span>
* <span data-ttu-id="47b75-488">`EnsureCreated` 方法完成時處理內容。</span><span class="sxs-lookup"><span data-stu-id="47b75-488">Dispose the context when the `EnsureCreated` method completes.</span></span>

<span data-ttu-id="47b75-489">下列程式碼顯示已更新的 *Program.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="47b75-489">The following code shows the updated *Program.cs* file.</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

<span data-ttu-id="47b75-490">`EnsureCreated` 可確保內容的資料庫存在。</span><span class="sxs-lookup"><span data-stu-id="47b75-490">`EnsureCreated` ensures that the database for the context exists.</span></span> <span data-ttu-id="47b75-491">如果存在，不會採取任何動作。</span><span class="sxs-lookup"><span data-stu-id="47b75-491">If it exists, no action is taken.</span></span> <span data-ttu-id="47b75-492">如果不存在，則會建立資料庫及其所有結構描述。</span><span class="sxs-lookup"><span data-stu-id="47b75-492">If it does not exist, then the database and all its schema are created.</span></span> <span data-ttu-id="47b75-493">`EnsureCreated` 不會使用移轉來建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-493">`EnsureCreated` does not use migrations to create the database.</span></span> <span data-ttu-id="47b75-494">使用 `EnsureCreated` 建立的資料庫稍後無法使用移轉來加以更新。</span><span class="sxs-lookup"><span data-stu-id="47b75-494">A database that is created with `EnsureCreated` cannot be later updated using migrations.</span></span>

<span data-ttu-id="47b75-495">應用程式啟動時會呼叫 `EnsureCreated` 以允許下列工作流程：</span><span class="sxs-lookup"><span data-stu-id="47b75-495">`EnsureCreated` is called on app start, which allows the following work flow:</span></span>

* <span data-ttu-id="47b75-496">刪除資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-496">Delete the DB.</span></span>
* <span data-ttu-id="47b75-497">變更資料庫結構描述 (例如新增 `EmailAddress` 欄位)。</span><span class="sxs-lookup"><span data-stu-id="47b75-497">Change the DB schema (for example, add an `EmailAddress` field).</span></span>
* <span data-ttu-id="47b75-498">執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b75-498">Run the app.</span></span>
* <span data-ttu-id="47b75-499">`EnsureCreated` 會建立包含 `EmailAddress` 資料行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-499">`EnsureCreated` creates a DB with the`EmailAddress` column.</span></span>

<span data-ttu-id="47b75-500">在開發初期，結構描述快速發展之時，`EnsureCreated` 很方便。</span><span class="sxs-lookup"><span data-stu-id="47b75-500">`EnsureCreated` is convenient early in development when the schema is rapidly evolving.</span></span> <span data-ttu-id="47b75-501">稍後在本教學課程中，會刪除資料庫並使用移轉。</span><span class="sxs-lookup"><span data-stu-id="47b75-501">Later in the tutorial the DB is deleted and migrations are used.</span></span>

### <a name="test-the-app"></a><span data-ttu-id="47b75-502">測試應用程式</span><span class="sxs-lookup"><span data-stu-id="47b75-502">Test the app</span></span>

<span data-ttu-id="47b75-503">執行應用程式，並接受 Cookie 原則。</span><span class="sxs-lookup"><span data-stu-id="47b75-503">Run the app and accept the cookie policy.</span></span> <span data-ttu-id="47b75-504">此應用程式不會保留個人資訊。</span><span class="sxs-lookup"><span data-stu-id="47b75-504">This app doesn't keep personal information.</span></span> <span data-ttu-id="47b75-505">您可以在[歐盟一般資料保護規定 (GDPR) 支援](xref:security/gdpr)閱讀 Cookie 原則相關資訊。</span><span class="sxs-lookup"><span data-stu-id="47b75-505">You can read about the cookie policy at [EU General Data Protection Regulation (GDPR) support](xref:security/gdpr).</span></span>

* <span data-ttu-id="47b75-506">選取 [學生] 連結，然後選取 [新建]。</span><span class="sxs-lookup"><span data-stu-id="47b75-506">Select the **Students** link and then **Create New**.</span></span>
* <span data-ttu-id="47b75-507">測試 [編輯]、[詳細資料] 和 [刪除] 連結。</span><span class="sxs-lookup"><span data-stu-id="47b75-507">Test the Edit, Details, and Delete links.</span></span>

## <a name="examine-the-schoolcontext-db-context"></a><span data-ttu-id="47b75-508">檢查 SchoolContext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="47b75-508">Examine the SchoolContext DB context</span></span>

<span data-ttu-id="47b75-509">為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。</span><span class="sxs-lookup"><span data-stu-id="47b75-509">The main class that coordinates EF Core functionality for a given data model is the DB context class.</span></span> <span data-ttu-id="47b75-510">資料內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。</span><span class="sxs-lookup"><span data-stu-id="47b75-510">The data context is derived from [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext).</span></span> <span data-ttu-id="47b75-511">資料內容會指定資料模型包含哪些實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-511">The data context specifies which entities are included in the data model.</span></span> <span data-ttu-id="47b75-512">在此專案中，類別命名為 `SchoolContext`。</span><span class="sxs-lookup"><span data-stu-id="47b75-512">In this project, the class is named `SchoolContext`.</span></span>

<span data-ttu-id="47b75-513">使用下列程式碼來更新 *SchoolContext.cs*：</span><span class="sxs-lookup"><span data-stu-id="47b75-513">Update *SchoolContext.cs* with the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

<span data-ttu-id="47b75-514">醒目標示的程式碼會為每一個實體集建立 [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。</span><span class="sxs-lookup"><span data-stu-id="47b75-514">The highlighted code creates a [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) property for each entity set.</span></span> <span data-ttu-id="47b75-515">在 EF Core 用語中：</span><span class="sxs-lookup"><span data-stu-id="47b75-515">In EF Core terminology:</span></span>

* <span data-ttu-id="47b75-516">實體集通常會對應至資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="47b75-516">An entity set typically corresponds to a DB table.</span></span>
* <span data-ttu-id="47b75-517">實體會對應至資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="47b75-517">An entity corresponds to a row in the table.</span></span>

<span data-ttu-id="47b75-518">`DbSet<Enrollment>` 和 `DbSet<Course>` 可加以忽略。</span><span class="sxs-lookup"><span data-stu-id="47b75-518">`DbSet<Enrollment>` and `DbSet<Course>` could be omitted.</span></span> <span data-ttu-id="47b75-519">EF Core 會隱含它們，因為 `Student` 實體引用 `Enrollment` 實體，而 `Enrollment` 實體引用 `Course` 實體。</span><span class="sxs-lookup"><span data-stu-id="47b75-519">EF Core includes them implicitly because the `Student` entity references the `Enrollment` entity, and the `Enrollment` entity references the `Course` entity.</span></span> <span data-ttu-id="47b75-520">本教學課程中，請在 `SchoolContext` 保留 `DbSet<Enrollment>` 和 `DbSet<Course>`。</span><span class="sxs-lookup"><span data-stu-id="47b75-520">For this tutorial, keep `DbSet<Enrollment>` and `DbSet<Course>` in the `SchoolContext`.</span></span>

### <a name="sql-server-express-localdb"></a><span data-ttu-id="47b75-521">SQL Server Express LocalDB</span><span class="sxs-lookup"><span data-stu-id="47b75-521">SQL Server Express LocalDB</span></span>

<span data-ttu-id="47b75-522">連接字串會指定 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。</span><span class="sxs-lookup"><span data-stu-id="47b75-522">The connection string specifies [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb).</span></span> <span data-ttu-id="47b75-523">LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。</span><span class="sxs-lookup"><span data-stu-id="47b75-523">LocalDB is a lightweight version of the SQL Server Express Database Engine and is intended for app development, not production use.</span></span> <span data-ttu-id="47b75-524">LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。</span><span class="sxs-lookup"><span data-stu-id="47b75-524">LocalDB starts on demand and runs in user mode, so there's no complex configuration.</span></span> <span data-ttu-id="47b75-525">LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。</span><span class="sxs-lookup"><span data-stu-id="47b75-525">By default, LocalDB creates *.mdf* DB files in the `C:/Users/<user>` directory.</span></span>

## <a name="add-code-to-initialize-the-db-with-test-data"></a><span data-ttu-id="47b75-526">新增程式碼來初始化含有測試資料的資料庫</span><span class="sxs-lookup"><span data-stu-id="47b75-526">Add code to initialize the DB with test data</span></span>

<span data-ttu-id="47b75-527">EF Core 會建立空白資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-527">EF Core creates an empty DB.</span></span> <span data-ttu-id="47b75-528">在本節中，會寫入 `Initialize` 方法並以測試資料來填入該資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-528">In this section, an `Initialize` method is written to populate it with test data.</span></span>

<span data-ttu-id="47b75-529">在 *Data* 資料夾中，建立名為 *DbInitializer.cs* 的新類別檔案並新增下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="47b75-529">In the *Data* folder, create a new class file named *DbInitializer.cs* and add the following code:</span></span>

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

<span data-ttu-id="47b75-530">注意：上述程式碼針對命名空間 (`namespace ContosoUniversity.Models`) 使用 `Models`，而不是使用 `Data`。</span><span class="sxs-lookup"><span data-stu-id="47b75-530">Note: The preceding code uses `Models` for the namespace (`namespace ContosoUniversity.Models`) rather than `Data`.</span></span> <span data-ttu-id="47b75-531">`Models` 與產生框架的程式碼一致。</span><span class="sxs-lookup"><span data-stu-id="47b75-531">`Models` is consistent with the scaffolder-generated code.</span></span> <span data-ttu-id="47b75-532">如需詳細資訊，請參閱[此 GitHub Scaffolding 問題](https://github.com/aspnet/Scaffolding/issues/822)。</span><span class="sxs-lookup"><span data-stu-id="47b75-532">For more information, see [this GitHub scaffolding issue](https://github.com/aspnet/Scaffolding/issues/822).</span></span>

<span data-ttu-id="47b75-533">程式碼會檢查資料庫中是否有任何學生。</span><span class="sxs-lookup"><span data-stu-id="47b75-533">The code checks if there are any students in the DB.</span></span> <span data-ttu-id="47b75-534">如果資料庫中沒有學生，則會使用測試資料初始化資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-534">If there are no students in the DB, the DB is initialized with test data.</span></span> <span data-ttu-id="47b75-535">它會將測試資料載入陣列之中，而非 `List<T>` 集合，以最佳化效能。</span><span class="sxs-lookup"><span data-stu-id="47b75-535">It loads test data into arrays rather than `List<T>` collections to optimize performance.</span></span>

<span data-ttu-id="47b75-536">`EnsureCreated` 方法會自動為資料庫內容建立資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-536">The `EnsureCreated` method automatically creates the DB for the DB context.</span></span> <span data-ttu-id="47b75-537">如果資料庫已存在，則 `EnsureCreated` 將會傳回而不修改該資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b75-537">If the DB exists, `EnsureCreated` returns without modifying the DB.</span></span>

<span data-ttu-id="47b75-538">在 *Program.cs* 中，修改 `Main` 方法來呼叫 `Initialize`：</span><span class="sxs-lookup"><span data-stu-id="47b75-538">In *Program.cs*, modify the `Main` method to call `Initialize`:</span></span>

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="47b75-539">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="47b75-539">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="47b75-540">停止應用程式 (如果它正在執行)，並在**套件管理員主控台** (PMC) 中執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="47b75-540">Stop the app if it's running, and run the following command in the **Package Manager Console** (PMC):</span></span>

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="47b75-541">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="47b75-541">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="47b75-542">若應用程式正在執行中，請停止它，然後刪除 *CU.db* 檔案。</span><span class="sxs-lookup"><span data-stu-id="47b75-542">Stop the app if it's running, and delete the *CU.db* file.</span></span>

---

## <a name="view-the-db"></a><span data-ttu-id="47b75-543">檢視資料庫</span><span class="sxs-lookup"><span data-stu-id="47b75-543">View the DB</span></span>

<span data-ttu-id="47b75-544">資料庫名稱是以您稍早所提供的內容名稱加上虛線和 GUID 所產生。</span><span class="sxs-lookup"><span data-stu-id="47b75-544">The database name is generated from the context name you provided earlier plus a dash and a GUID.</span></span> <span data-ttu-id="47b75-545">因此，資料庫名稱將會是 "SchoolContext-{GUID}"。</span><span class="sxs-lookup"><span data-stu-id="47b75-545">Thus, the database name will be "SchoolContext-{GUID}".</span></span> <span data-ttu-id="47b75-546">GUID 針對每個使用者都將不同。</span><span class="sxs-lookup"><span data-stu-id="47b75-546">The GUID will be different for each user.</span></span>
<span data-ttu-id="47b75-547">從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。</span><span class="sxs-lookup"><span data-stu-id="47b75-547">Open **SQL Server Object Explorer** (SSOX) from the **View** menu in Visual Studio.</span></span>
<span data-ttu-id="47b75-548">在 SSOX 中，按一下 **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}** 。</span><span class="sxs-lookup"><span data-stu-id="47b75-548">In SSOX, click **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}**.</span></span>

<span data-ttu-id="47b75-549">展開 **Tables** 節點。</span><span class="sxs-lookup"><span data-stu-id="47b75-549">Expand the **Tables** node.</span></span>

<span data-ttu-id="47b75-550">以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。</span><span class="sxs-lookup"><span data-stu-id="47b75-550">Right-click the **Student** table and click **View Data** to see the columns created and the rows inserted into the table.</span></span>

## <a name="asynchronous-code"></a><span data-ttu-id="47b75-551">非同步程式碼</span><span class="sxs-lookup"><span data-stu-id="47b75-551">Asynchronous code</span></span>

<span data-ttu-id="47b75-552">非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。</span><span class="sxs-lookup"><span data-stu-id="47b75-552">Asynchronous programming is the default mode for ASP.NET Core and EF Core.</span></span>

<span data-ttu-id="47b75-553">網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。</span><span class="sxs-lookup"><span data-stu-id="47b75-553">A web server has a limited number of threads available, and in high load situations all of the available threads might be in use.</span></span> <span data-ttu-id="47b75-554">發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。</span><span class="sxs-lookup"><span data-stu-id="47b75-554">When that happens, the server can't process new requests until the threads are freed up.</span></span> <span data-ttu-id="47b75-555">使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。</span><span class="sxs-lookup"><span data-stu-id="47b75-555">With synchronous code, many threads may be tied up while they aren't actually doing any work because they're waiting for I/O to complete.</span></span> <span data-ttu-id="47b75-556">使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。</span><span class="sxs-lookup"><span data-stu-id="47b75-556">With asynchronous code, when a process is waiting for I/O to complete, its thread is freed up for the server to use for processing other requests.</span></span> <span data-ttu-id="47b75-557">因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。</span><span class="sxs-lookup"><span data-stu-id="47b75-557">As a result, asynchronous code enables server resources to be used more efficiently, and the server is enabled to handle more traffic without delays.</span></span>

<span data-ttu-id="47b75-558">非同步程式碼會在執行階段導致少量的額外負荷。</span><span class="sxs-lookup"><span data-stu-id="47b75-558">Asynchronous code does introduce a small amount of overhead at run time.</span></span> <span data-ttu-id="47b75-559">在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。</span><span class="sxs-lookup"><span data-stu-id="47b75-559">For low traffic situations, the performance hit is negligible, while for high traffic situations, the potential performance improvement is substantial.</span></span>

<span data-ttu-id="47b75-560">在下列程式碼中，[async](/dotnet/csharp/language-reference/keywords/async) 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法會使程式碼以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="47b75-560">In the following code, the [async](/dotnet/csharp/language-reference/keywords/async) keyword, `Task<T>` return value, `await` keyword, and `ToListAsync` method make the code execute asynchronously.</span></span>

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* <span data-ttu-id="47b75-561">`async` 關鍵字會指示編譯器：</span><span class="sxs-lookup"><span data-stu-id="47b75-561">The `async` keyword tells the compiler to:</span></span>
  * <span data-ttu-id="47b75-562">為方法主體的組件產生回呼。</span><span class="sxs-lookup"><span data-stu-id="47b75-562">Generate callbacks for parts of the method body.</span></span>
  * <span data-ttu-id="47b75-563">自動建立傳回的 [Task](/dotnet/api/system.threading.tasks.task) 物件。</span><span class="sxs-lookup"><span data-stu-id="47b75-563">Automatically create the [Task](/dotnet/api/system.threading.tasks.task) object that's returned.</span></span> <span data-ttu-id="47b75-564">如需詳細資訊，請參閱 [Task 傳回型別](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。</span><span class="sxs-lookup"><span data-stu-id="47b75-564">For more information, see [Task Return Type](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType).</span></span>

* <span data-ttu-id="47b75-565">隱含傳回型別 `Task` 代表進行中的工作。</span><span class="sxs-lookup"><span data-stu-id="47b75-565">The implicit return type `Task` represents ongoing work.</span></span>
* <span data-ttu-id="47b75-566">`await` 關鍵字會導致編譯器將方法分成兩個組件。</span><span class="sxs-lookup"><span data-stu-id="47b75-566">The `await` keyword causes the compiler to split the method into two parts.</span></span> <span data-ttu-id="47b75-567">第一個部分會與非同步啟動的作業一同結束。</span><span class="sxs-lookup"><span data-stu-id="47b75-567">The first part ends with the operation that's started asynchronously.</span></span> <span data-ttu-id="47b75-568">第二個部分則會放入作業完成時呼叫的回呼方法中。</span><span class="sxs-lookup"><span data-stu-id="47b75-568">The second part is put into a callback method that's called when the operation completes.</span></span>
* <span data-ttu-id="47b75-569">`ToListAsync` 是 `ToList` 擴充方法的非同步版本。</span><span class="sxs-lookup"><span data-stu-id="47b75-569">`ToListAsync` is the asynchronous version of the `ToList` extension method.</span></span>

<span data-ttu-id="47b75-570">若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：</span><span class="sxs-lookup"><span data-stu-id="47b75-570">Some things to be aware of when writing asynchronous code that uses EF Core:</span></span>

* <span data-ttu-id="47b75-571">只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="47b75-571">Only statements that cause queries or commands to be sent to the DB are executed asynchronously.</span></span> <span data-ttu-id="47b75-572">包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。</span><span class="sxs-lookup"><span data-stu-id="47b75-572">That includes, `ToListAsync`, `SingleOrDefaultAsync`, `FirstOrDefaultAsync`, and `SaveChangesAsync`.</span></span> <span data-ttu-id="47b75-573">不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。</span><span class="sxs-lookup"><span data-stu-id="47b75-573">It doesn't include statements that just change an `IQueryable`, such as `var students = context.Students.Where(s => s.LastName == "Davolio")`.</span></span>
* <span data-ttu-id="47b75-574">EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。</span><span class="sxs-lookup"><span data-stu-id="47b75-574">An EF Core context isn't thread safe: don't try to do multiple operations in parallel.</span></span>
* <span data-ttu-id="47b75-575">若要利用非同步程式碼的效能優勢，請確認程式庫套件 (例如分頁) 在呼叫將查詢傳送至資料庫的 EF Core 方法時是使用非同步。</span><span class="sxs-lookup"><span data-stu-id="47b75-575">To take advantage of the performance benefits of async code, verify that library packages (such as for paging) use async if they call EF Core methods that send queries to the DB.</span></span>

<span data-ttu-id="47b75-576">如需非同步方法的詳細資訊，請參閱 [Async 概觀](/dotnet/standard/async)和[使用 Async 和 Await 設計非同步程式](/dotnet/csharp/programming-guide/concepts/async/)。</span><span class="sxs-lookup"><span data-stu-id="47b75-576">For more information about asynchronous programming in .NET, see [Async Overview](/dotnet/standard/async) and [Asynchronous programming with async and await](/dotnet/csharp/programming-guide/concepts/async/).</span></span>

<span data-ttu-id="47b75-577">在下一個教學課程中，將會檢視基本的 CRUD (建立、讀取、更新、刪除) 作業。</span><span class="sxs-lookup"><span data-stu-id="47b75-577">In the next tutorial, basic CRUD (create, read, update, delete) operations are examined.</span></span>



## <a name="additional-resources"></a><span data-ttu-id="47b75-578">其他資源</span><span class="sxs-lookup"><span data-stu-id="47b75-578">Additional resources</span></span>

* [<span data-ttu-id="47b75-579">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="47b75-579">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [<span data-ttu-id="47b75-580">下一個</span><span class="sxs-lookup"><span data-stu-id="47b75-580">Next</span></span>](xref:data/ef-rp/crud)

::: moniker-end
