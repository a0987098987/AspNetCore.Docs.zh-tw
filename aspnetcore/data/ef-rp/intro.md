---
title: ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8
author: rick-anderson
description: 示範如何建立使用 Entity Framework Core 的 Razor 頁面應用程式
ms.author: riande
ms.date: 6/31/2017
uid: data/ef-rp/intro
ms.openlocfilehash: a234d5fefd671d4503f6c63b79074d47c893f69c
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207702"
---
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8

[!INCLUDE[2.0 version](~/includes/RP-EF/20-pdf.md)]

::: moniker range=">= aspnetcore-2.1"

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學的 Web 應用程式範例將示範如何以 Entity Framework (EF) Core 來建立 ASP.NET Core Razor Pages 應用程式。

這個範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。 此頁面是說明如何建立 Contoso 大學範例應用程式教學課程系列中的第一頁。

[下載或檢視已完成的應用程式。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:index#how-to-download-a-sample)。

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

[!INCLUDE [](~/includes/2.1-SDK.md)]

------

熟悉 [Razor 頁面](xref:razor-pages/index)。 新進程式設計人員應先完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，再開始此系列。

## <a name="troubleshooting"></a>疑難排解

如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。 取得協助的好方法是將問題公佈到 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 以詢問 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。

## <a name="the-contoso-university-web-app"></a>Contoso 大學 Web 應用程式

在教學課程中建立的應用程式，是一個基本的大學網站。

使用者可以檢視和更新學生、課程和教師資訊。 以下是幾個在教學課程中建立的畫面。

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

此網站的 UI 樣式接近內建範本所產生的內容。 教學課程聚焦於 EF Core 與 Razor 頁面，而不是 UI。

## <a name="create-the-contosouniversity-razor-pages-web-app"></a>建立 ContosoUniversity Razor Pages Web 應用程式

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **ContosoUniversity**。 請務必將專案命名為 *ContosoUniversity*，當您複製/貼上程式碼時，命名空間才會相符。
* 在下拉式清單中選取 [ASP.NET Core 2.1]，然後選取 [Web 應用程式]。

如需上述步驟的影像，請參閱[ 建立 Razor Web 應用程式](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-web-app)。
執行應用程式。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

```CLI
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

------

## <a name="set-up-the-site-style"></a>設定網站樣式

一些變更設定了網站的功能表、配置和首頁。 使用下列變更更新 *Pages/Shared/_Layout.cshtml*：

* 將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。 共出現三次。

* 為 **Students**、**Courses**、**Instructors**、**Departments** 新增功能表項目，並刪除 **Contact** 功能表項目。

所做的變更已醒目標示。 (所有標記都「不會」顯示。)

[!code-html[](intro/samples/cu21/Pages/Shared/_Layout.cshtml?highlight=6,29,35-38,50&name=snippet)]

在 *Pages/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：

[!code-html[](intro/samples/cu21/Pages/Index.cshtml)]

## <a name="create-the-data-model"></a>建立資料模型

為 Contoso 大學應用程式建立實體類別。 從下列三個實體開始：

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

在 `Student` 和 `Enrollment` 實體之間會有一對多關聯性。 在 `Course` 和 `Enrollment` 實體之間會有一對多關聯性。 一位學生可註冊任何數目的課程。 一堂課程可以讓任意數目的學生註冊。

在下列一節中，將為這些實體建立各自的類別。

### <a name="the-student-entity"></a>Student 實體

![Student 實體圖表](intro/_static/student-entity.png)

建立 *Models* 資料夾。 在 *Models* 資料夾中，用下列程式碼建立一個名為 *Student.cs* 的類別檔案：

[!code-csharp[](intro/samples/cu21/Models/Student.cs?name=snippet_Intro)]

`ID` 屬性會成為資料庫 (DB) 資料表中的主索引鍵資料行，並對應至這個類別。 EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。 在 `classnameID` 中，`classname` 是類別的名稱。 另一個自動識別的主索引鍵是上述範例中的 `StudentID`。

`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationship)。 導覽屬性會連結至與此實體相關的其他實體。 在這個案例中，`Student entity` 中的 `Enrollments` 屬性會保有與 `Student` 相關的所有 `Enrollment` 實體。 例如，如果資料庫中 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性會包含這兩個 `Enrollment` 實體。 相關的 `Enrollment` 資料列，包含該學生在 `StudentID` 資料行中的主索引鍵值。 例如，假設 student with ID=1 在 `Enrollment` 資料表中有兩個資料列。 `Enrollment` 資料表有兩個包含 `StudentID`=1 的資料列。 `StudentID` 為 `Enrollment` 資料表中的外部索引鍵，會指定在 `Student` 資料表中的學生。

如果導覽屬性可以保存多個實體，該導覽屬性必然是清單類型，例如 `ICollection<T>`。 您可以指定 `ICollection<T>`，或是如 `List<T>`、`HashSet<T>` 的類型。 如使用 `ICollection<T>`，EF Core 預設將建立 `HashSet<T>` 集合。 保存多個實體的導覽屬性，來自多對多和一對多關聯性。

### <a name="the-enrollment-entity"></a>Enrollment 實體

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

在 *Models* 資料夾中，用下列程式碼建立 *Enrollment.cs*：

[!code-csharp[](intro/samples/cu21/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` 屬性是主索引鍵。 這個實體會使用 `classnameID` 模式，而不是像 `Student` 實體一樣的 `ID`。 開發人員通常會選擇其中一個模式，並使用於整個資料模型。 在稍後的教學課程中，將示範使用不具類別名稱的 ID，使得在數據模型中實作繼承更加簡單。

`Grade` 屬性為 `enum`。 問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。 為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。 `Student` 實體與 `Student.Enrollments` 導覽屬性不同，導覽屬性包含多個 `Enrollment` 實體。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。 例如，`StudentID` 為 `Student` 導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`。 外部索引鍵屬性也可命名為 `<primary key property name>`。 例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。

### <a name="the-course-entity"></a>Course 實體

![Course 實體圖表](intro/_static/course-entity.png)

在 *Models* 資料夾中，用下列程式碼建立 *Course.cs*：

[!code-csharp[](intro/samples/cu21/Models/Course.cs?name=snippet_Intro)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

`DatabaseGenerated` 屬性 (attribute) 可讓應用程式指定主索引鍵，而不是讓 DB 來產生。

## <a name="scaffold-the-student-model"></a>Scaffold 學生模型

在本節中會 scaffold 學生模型。 亦即 Scaffolding 工具會產生學生模型的建立、讀取、更新和刪除 (CRUD) 作業頁面。

* 建置專案。
* 建立 *Pages/Students* 資料夾。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 [方案總管] 中，以滑鼠右鍵按一下 *Pages/Students* 資料夾 > [新增] > [新增 Scaffold 項目]。
* 在 [新增 Scaffold] 對話方塊中，選取 [Razor Pages using Entity Framework (CRUD)] \(使用 Entity Framework 的 Razor Pages (CRUD)\) > [新增]。

完成 [Add Razor Pages using Entity Framework (CRUD)] \(新增使用 Entity Framework 的 Razor Pages (CRUD)\) 對話方塊：

* 在 [模型類別] 下拉式清單中，選取 [學生 (ContosoUniversity.Models)]。
* 在 [資料內容類別] 列中，選取 **+** (加號) 符號，並將產生的名稱變更為 **ContosoUniversity.Models.SchoolContext**。
* 在 [資料內容類別] 下拉式清單中，選取 **ContosoUniversity.Models.SchoolContext**
* 選取 [新增]。

![CRUD 對話方塊](intro/_static/s1.png)

如果您有上一個步驟的問題，請參閱 [Scaffold 影片模型](xref:tutorials/razor-pages/model#scaffold-the-movie-model)。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

執行下列命令來 Scaffold 學生模型。

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
```
------

隨即建立 Scaffold 處理序並變更下列檔案：

### <a name="files-created"></a>建立的檔案

* *Pages/Students* 建立、刪除、詳細資料、編輯、索引。
* *Data/SchoolContext.cs*

### <a name="file-updates"></a>檔案更新

* *Startup.cs*：這個檔案的變更會於下一節詳述。
* *appsettings.json*：已新增用來連線到本機資料庫的連接字串。

## <a name="examine-the-context-registered-with-dependency-injection"></a>檢查使用相依性插入所註冊的內容

ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，在本教學課程稍後會示範。

Scaffolding 工具會自動建立資料庫內容，並向相依性插入容器註冊。

檢查 *Startup.cs* 中的 `ConfigureServices` 方法。 Scaffolder 已新增醒目標示行：

[!code-csharp[](intro/samples/cu21/Startup.cs?name=snippet_SchoolContext&highlight=13-14)]

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。

## <a name="update-main"></a>更新 main

在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：

* 從相依性插入容器中取得資料庫內容執行個體。
* 呼叫 [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated)。
* `EnsureCreated` 方法完成時處理內容。

下列程式碼顯示已更新的 *Program.cs* 檔案。

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet)]

`EnsureCreated` 可確保內容的資料庫存在。 如果存在，不會採取任何動作。 如果不存在，則會建立資料庫及其所有結構描述。 `EnsureCreated` 不會使用移轉來建立資料庫。 使用 `EnsureCreated` 建立的資料庫稍後無法使用移轉來加以更新。

應用程式啟動時會呼叫 `EnsureCreated` 以允許下列工作流程：

* 刪除資料庫。
* 變更資料庫結構描述 (例如新增 `EmailAddress` 欄位)。
* 執行應用程式。
* `EnsureCreated` 會建立包含 `EmailAddress` 資料行的資料庫。

在開發初期，結構描述快速發展之時，`EnsureCreated` 很方便。 稍後在本教學課程中，會刪除資料庫並使用移轉。

### <a name="test-the-app"></a>測試應用程式

執行應用程式，並接受 Cookie 原則。 此應用程式不會保留個人資訊。 您可以在[歐盟一般資料保護規定 (GDPR) 支援](xref:security/gdpr)閱讀 Cookie 原則相關資訊。

* 選取 [學生] 連結，然後選取 [新建]。
* 測試 [編輯]、[詳細資料] 和 [刪除] 連結。

## <a name="examine-the-schoolcontext-db-context"></a>檢查 SchoolContext 資料庫內容

為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。 資料內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 資料內容會指定資料模型包含哪些實體。 在此專案中，類別命名為 `SchoolContext`。

使用下列程式碼來更新 *SchoolContext.cs*：

[!code-csharp[](intro/samples/cu21/Data/SchoolContext.cs?name=snippet_Intro&highlight=12-14)]

醒目標示的程式碼會為每一個實體集建立 [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 EF Core 用語中：

* 實體集通常會對應至資料庫資料表。
* 實體會對應至資料表中的資料列。

`DbSet<Enrollment>` 和 `DbSet<Course>` 可加以忽略。 EF Core 會隱含它們，因為 `Student` 實體引用 `Enrollment` 實體，而 `Enrollment` 實體引用 `Course` 實體。 本教學課程中，請在 `SchoolContext` 保留 `DbSet<Enrollment>` 和 `DbSet<Course>`。

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

連接字串會指定 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。 LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。 LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。 LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。

## <a name="add-code-to-initialize-the-db-with-test-data"></a>新增程式碼來初始化含有測試資料的資料庫

EF Core 會建立空白資料庫。 在本節中，會寫入 `Initialize` 方法並以測試資料來填入該資料庫。

在 *Data* 資料夾中，建立名為 *DbInitializer.cs* 的新類別檔案並新增下列程式碼：

[!code-csharp[](intro/samples/cu21/Data/DbInitializer.cs?name=snippet_Intro)]

程式碼會檢查資料庫中是否有任何學生。 如果資料庫中沒有學生，則會使用測試資料初始化資料庫。 它會將測試資料載入陣列之中，而非 `List<T>` 集合，以最佳化效能。

`EnsureCreated` 方法會自動為資料庫內容建立資料庫。 如果資料庫已存在，則 `EnsureCreated` 將會傳回而不修改該資料庫。

在 *Program.cs* 中，修改 `Main` 方法來呼叫 `Initialize`：

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

刪除任何學生記錄並重新啟動應用程式。 如果資料庫未初始化，在 `Initialize` 中設定中斷點來診斷問題。

## <a name="view-the-db"></a>檢視資料庫

從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。
在 SSOX 中，按一下 **(localdb) \MSSQLLocalDB > Databases > ContosoUniversity1**。

展開 **Tables** 節點。

以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。

## <a name="asynchronous-code"></a>非同步程式碼

非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。

網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。 發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。 使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。 使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。 因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。

非同步程式碼會在執行階段導致少量的額外負荷。 在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。

在下列程式碼中，[async](/dotnet/csharp/language-reference/keywords/async) 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法會使程式碼以非同步方式執行。

[!code-csharp[](intro/samples/cu21/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` 關鍵字會指示編譯器：
  * 為方法主體的組件產生回呼。
  * 自動建立傳回的 [Task](/dotnet/api/system.threading.tasks.task) 物件。 如需詳細資訊，請參閱 [Task 傳回型別](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。

* 隱含傳回型別 `Task` 代表進行中的工作。
* `await` 關鍵字會導致編譯器將方法分成兩個組件。 第一個部分會與非同步啟動的作業一同結束。 第二個部分則會放入作業完成時呼叫的回呼方法中。
* `ToListAsync` 是 `ToList` 擴充方法的非同步版本。

若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：

* 只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。 包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。 不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。
* EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。
* 若要利用非同步程式碼的效能優勢，請確認程式庫套件 (例如分頁) 在呼叫將查詢傳送至資料庫的 EF Core 方法時是使用非同步。

如需非同步方法的詳細資訊，請參閱 [Async 概觀](/dotnet/articles/standard/async)和[使用 Async 和 Await 設計非同步程式](/dotnet/csharp/programming-guide/concepts/async/)。

在下一個教學課程中，將會檢視基本的 CRUD (建立、讀取、更新、刪除) 作業。

::: moniker-end

> [!div class="step-by-step"]
> [下一步](xref:data/ef-rp/crud)
