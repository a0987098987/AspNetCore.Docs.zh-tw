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
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

這是一系列教學課程中的第一篇，示範如何在[ASP.NET Core Razor Pages](xref:razor-pages/index)應用程式中使用 ENTITY FRAMEWORK （EF） Core。 教學課程會為虛構的 Contoso 大學建置網站。 網站包含學生入學許可、課程建立和講師指派等功能。

[下載或檢視已完成的應用程式。](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:index#how-to-download-a-sample)。

## <a name="prerequisites"></a>必要條件

* 若您是第一次使用 Razor Pages，請先前往[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start) 教學課程系列，再開始進行本教學課程。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[VS prereqs](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[VS Code prereqs](~/includes/net-core-prereqs-vsc-3.0.md)]

---

## <a name="database-engines"></a>資料庫引擎

Visual Studio 說明會使用 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)，它是一種只在 Windows 上執行的 SQL Server Express 版本。

Visual Studio Code 說明則會使用 [SQLite](https://www.sqlite.org/)，它是一種跨平台的資料庫引擎。

若您選擇使用 SQLite，請下載及安裝協力廠商工具來管理和檢視 SQLite 資料庫，例如 [DB Browser for SQLite](https://sqlitebrowser.org/)。

## <a name="troubleshooting"></a>疑難排解

若您遇到無法解決的問題，請將您的程式碼與[已完成的專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)進行比較。 取得協助的其中一種好方法是將問題張貼到 StackOverflow.com，並使用 [ASP.NET Core 標籤](https://stackoverflow.com/questions/tagged/asp.net-core)或 [EF Core 標籤](https://stackoverflow.com/questions/tagged/entity-framework-core)。

## <a name="the-sample-app"></a>範例應用程式

在教學課程中建立的應用程式，是一個基本的大學網站。 使用者可以檢視和更新學生、課程和教師資訊。 以下是幾個在教學課程中建立的畫面。

![Students [索引] 頁面](intro/_static/students-index30.png)

![Students [編輯] 頁面](intro/_static/student-edit30.png)

本網站的 UI 風格是以內建的專案範本為基礎。 教學課程的重點在於如何使用 EF Core，而非如何自訂 UI。

請遵循頁面頂端的連結來取得已完成專案的原始程式碼。 *cu30* 資料夾包含本教學課程 ASP.NET Core 3.0 版本的程式碼。 您可以在 *cu30snapshots* 資料夾中找到反映教學課程 1 到 7 程式碼狀態的檔案。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

在下載已完成的專案後執行應用程式：

* 刪除名稱中包含 *SQLite* 的三個檔案和一個資料夾。
* 建置專案。
* 在套件管理器主控台 (PMC) 中，執行下列命令：

  ```powershell
  Update-Database
  ```

* 執行專案來植入資料庫。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在下載已完成的專案後執行應用程式：

* 刪除 *ContosoUniversity.csproj*，並將 *ContosoUniversitySQLite.csproj* 重新命名為 *ContosoUniversity.csproj*。
* 刪除 *Startup.cs*，並將 *StartupSQLite.cs* 重新命名為 *Startup.cs*。
* 刪除 *appSettings.json*，並將 *appSettingsSQLite.json* 重新命名為 *appSettings.json*。
* 刪除 *Migrations* 資料夾，並將 *MigrationsSQL* 重新命名為 *Migrations*。
* 建置專案。
* 在專案資料夾中的命令提示字元內，執行下列命令：

  ```dotnetcli
  dotnet tool install --global dotnet-ef
  dotnet ef database update
  ```

* 在您的 SQLite 工具中，執行下列 SQL 陳述式：

  ```sql
  UPDATE Department SET RowVersion = randomblob(8)
  ```

* 執行專案來植入資料庫。

---

## <a name="create-the-web-app-project"></a>建立 Web 應用程式專案

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案]。
* 選取 [ASP.NET Core Web 應用程式]。
* 將專案命名為 *ContosoUniversity*。 使用與此名稱完全相符的名稱非常重要 (包括大寫)，這樣做可以讓命名空間在您複製和貼上程式碼時相符。
* 在下拉式清單中選取 [.NET Core] 及 [ASP.NET Core 3.0]，然後選取 [Web 應用程式]。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 在終端機中，巡覽至應建立專案資料夾的資料夾。

* 執行下列命令來建立 Razor Pages 專案並 `cd` 至新的專案資料夾：

  ```dotnetcli
  dotnet new webapp -o ContosoUniversity
  cd ContosoUniversity
  ```

---

## <a name="set-up-the-site-style"></a>設定網站樣式

更新 *Pages/Shared/_Layout.cshtml* 來設定網站頁首、頁尾和功能表：

* 將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。 共出現三次。

* 刪除 [Home] 和 [Privacy] 功能表項目，然後新增 [About]、[Students]、[Courses]、[Instructors] 和 [Departments] 的項目。

所做的變更已醒目標示。

[!code-cshtml[Main](intro/samples/cu30/Pages/Shared/_Layout.cshtml?highlight=6,14,21-35,49)]

在 *Pages/Index.cshtml* 中，將檔案內容替換成下列程式碼，將 ASP.NET Core 相關文字取代成此應用程式的相關文字：

[!code-cshtml[Main](intro/samples/cu30/Pages/Index.cshtml)]

執行應用程式來驗證首頁是否正常顯示。

## <a name="the-data-model"></a>資料模型

下列各節會建立資料模型：

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

學生可以註冊任何數量的課程，課程也能讓任意數量的學生註冊。

## <a name="the-student-entity"></a>Student 實體

![Student 實體圖表](intro/_static/student-entity.png)

* 在專案資料夾中建立 *Models* 資料夾。 

* 使用下列程式碼建立 *Models/Student.cs*：

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Student.cs)]

`ID` 屬性會成為對應到此類別資料庫資料表的主索引鍵資料行。 EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。 因此 `Student` 類別主索引鍵的替代自動識別名稱為 `StudentID`。

`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。 導覽屬性會保留與此實體相關的其他實體。 在這種情況下，`Student` 實體的 `Enrollments` 屬性會保留所有與該 Student 相關的 `Enrollment` 實體。 例如，若資料庫中的 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性便會包含這兩個 Enrollment 項目。 

在資料庫中，若 Enrollment 資料列的 StudentID 資料行包含學生的識別碼值，則該資料列便會與 Student 資料列相關。 例如，假設某 Student 資料列的識別碼為 1。 相關的 Enrollment 資料列將會擁有 StudentID = 1。 StudentID 是 Enrollment 資料表中的「外部索引鍵」。 

`Enrollments` 屬性會定義為 `ICollection<Enrollment>`，因為可能會有多個相關的 Enrollment 實體。 您可以使用其他集合型別，例如 `List<Enrollment>` 或 `HashSet<Enrollment>`。 如使用 `ICollection<Enrollment>`，EF Core 預設將建立 `HashSet<Enrollment>` 集合。

## <a name="the-enrollment-entity"></a>Enrollment 實體

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

使用下列程式碼建立 *Models/Enrollment.cs*：

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Enrollment.cs)]

`EnrollmentID` 屬性是主索引鍵；這個實體會使用 `classnameID` 模式，而非 `ID` 本身。 針對生產資料模型，請選擇一個模式並一致地使用它。 本教學課程同時使用兩者的方式只是為了示範兩者都可運作。 在不使用 `classname` 的情況下使用 `ID` 可讓實作某些類型的資料模型變更更容易。

`Grade` 屬性為 `enum`。 `Grade` 型別宣告後方的問號表示 `Grade` 屬性[可為 Null](https://docs.microsoft.com/dotnet/csharp/programming-guide/nullable-types/)。 為 Null 的成績不同於成績為零&mdash;Null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。 例如，`StudentID` 是 `Student` 導覽屬性的外部索引鍵，因為 `Student` 實體的主索引鍵是 `ID`。 外部索引鍵屬性也可命名為 `<primary key property name>`。 例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。

## <a name="the-course-entity"></a>Course 實體

![Course 實體圖表](intro/_static/course-entity.png)

使用下列程式碼建立 *Models/Course.cs*：

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Models/Course.cs)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

`DatabaseGenerated` 屬性可讓應用程式指定主索引鍵，而非讓資料庫產生它。

建置專案以驗證沒有任何編譯器錯誤。

## <a name="scaffold-student-pages"></a>Scaffold Student 頁面

在本節中，您會使用 ASP.NET scaffolding 工具來產生：

* EF Core「內容」類別。 內容是協調指定資料模型 Entity Framework 功能的主類別。 它衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別。
* 處理 `Student` 實體建立、讀取、更新和刪除 (CRUD) 作業的 Razor 頁面。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 *Pages* 資料夾中建立 *Students* 資料夾。
* 在 [方案總管] 中，以滑鼠右鍵按一下 *Page/Students* 資料夾，然後選取 [新增] > [新增 Scaffold 項目]。
* 在 [新增 Scaffold] 對話方塊中，選取 [Razor Pages using Entity Framework (CRUD)] \(使用 Entity Framework 的 Razor Pages (CRUD)\) > [新增]。
* 在 [使用 Entity Framework (CRUD) 新增 Razor Pages] 對話方塊中：
  * 在 [模型類別] 下拉式清單中，選取 [學生 (ContosoUniversity.Models)]。
  * 在 [資料內容類別] 資料列中，選取 **+** (加號)。
  * 將資料內容的名稱從 *ContosoUniversity.Models.ContosoUniversityContext* 變更為 *ContosoUniversity.Data.SchoolContext*。
  * 選取 [新增]。

會自動安裝下列套件：

* `Microsoft.VisualStudio.Web.CodeGeneration.Design`
* `Microsoft.EntityFrameworkCore.SqlServer`
* `Microsoft.Extensions.Logging.Debug`
* `Microsoft.EntityFrameworkCore.Tools`

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 執行下列 .NET Core CLI 命令來安裝必要的 NuGet 套件：
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

  Microsoft.VisualStudio.Web.CodeGeneration.Design 套件是進行 scaffolding 時的必要項目。 雖然應用程式不會使用 SQL Server，scaffolding 工具仍需要 SQL Server 套件。

* 建立 *Pages/Students* 資料夾。

* 執行下列命令來安裝 [aspnet-codegenerator scaffolding 工具](xref:fundamentals/tools/dotnet-aspnet-codegenerator)。

  ```dotnetcli
  dotnet tool install --global dotnet-aspnet-codegenerator
  ```

* 執行下列命令來 scaffold Student 頁面。

  **在 Windows 上**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
  ```

  **在 macOS 或 Linux 上**

  ```dotnetcli
  dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Data.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
  ```

---

若您在上述步驟中遇到問題，請建置專案並重試 scaffold 步驟。

Scaffolding 流程：

* 在 *Pages/Students* 資料夾中建立 Razor 頁面：
  * *Create.cshtml* 和 *Create.cshtml.cs*
  * *Delete.cshtml* 和 *Delete.cshtml.cs*
  * *Details.cshtml* 和 *Details.cshtml.cs*
  * *Edit.cshtml* 和 *Edit.cshtml.cs*
  * *Index.cshtml* 和 *Index.cshtml.cs*
* 建立 *Data/SchoolContext.cs*。
* 將內容新增到 *Startup.cs* 中的相依性插入。
* 將資料庫連接字串新增到 *appsettings.json*。

## <a name="database-connection-string"></a>資料庫連接字串

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

連接字串會指定 [SQL Server LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb)。 

[!code-json[Main](intro/samples/cu30/appsettings.json?highlight=11)]

LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。 根據預設，LocalDB 會在 `C:/Users/<user>` 目錄中建立 *.mdf* 檔案。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

將連接字串變更為指向名為 *CU.db* 的 SQLite 資料庫檔案：

[!code-json[Main](intro/samples/cu30/appsettingsSQLite.json?highlight=11)]

---

## <a name="update-the-database-context-class"></a>更新資料庫內容類別

協調指定資料模型 EF Core 功能的主類別是資料庫內容類別。 內容衍生自 [Microsoft.EntityFrameworkCore.DbContext](/dotnet/api/microsoft.entityframeworkcore.dbcontext)。 內容會指定哪些實體會包含在資料模型中。 在此專案中，類別命名為 `SchoolContext`。

使用下列程式碼來更新 *SchoolContext.cs*：

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/SchoolContext.cs?highlight=13-22)]

醒目標示的程式碼會為每一個實體集建立 [DbSet\<TEntity>](/dotnet/api/microsoft.entityframeworkcore.dbset-1) 屬性。 在 EF Core 用語中：

* 實體集通常會對應到資料庫資料表。
* 實體會對應至資料表中的資料列。

因為實體集會包含多個實體，所以 DBSet 屬性應為複數名稱。 因為 scaffolding 工具建立了 `Student` DBSet，所以此步驟會將它變更為複數的 `Students`。 

為了讓 Razor Pages 程式碼與新的 DBSet 名稱相符，請在整個專案中將 `_context.Student` 變更全域變更為 `_context.Students`。  會有 8 次變更。

建置專案以確認沒有任何編譯器錯誤。

## <a name="startupcs"></a>Startup.cs

ASP.NET Core 內建[相依性插入](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core 資料庫內容) 會在應用程式啟動期間對相依性插入進行註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼會顯示在本教學課程稍後部分。

Scaffolding 工具會自動對相依性插入容器註冊內容類別。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 在 `ConfigureServices` 中，Scaffolder 會新增下列醒目提示行：

  [!code-csharp[Main](intro/samples/cu30/Startup.cs?name=snippet_ConfigureServices&highlight=5-6)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 在 `ConfigureServices` 中，確認 Scaffolder 新增的程式碼會呼叫 `UseSqlite`。

  [!code-csharp[Main](intro/samples/cu30/StartupSQLite.cs?name=snippet_ConfigureServices&highlight=5-6)]

---

連接字串的名稱，會透過對 [DbContextOptions](/dotnet/api/microsoft.entityframeworkcore.dbcontextoptions) 物件呼叫方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core 設定系統](xref:fundamentals/configuration/index)會從 *appsettings.json* 檔案讀取連接字串。

## <a name="create-the-database"></a>建立資料庫

若未存在資料庫，則請更新 *Program.cs* 來加以建立：

[!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Program.cs?highlight=1-2,14-18,21-38)]

若已存在內容的資料庫，則 [EnsureCreated](/dotnet/api/microsoft.entityframeworkcore.infrastructure.databasefacade.ensurecreated#Microsoft_EntityFrameworkCore_Infrastructure_DatabaseFacade_EnsureCreated) 方法便不會採取任何動作。 若資料庫不存在，則它會建立資料庫和結構描述。 `EnsureCreated` 會啟用下列工作流程來處理資料模型變更：

* 刪除資料庫。 任何現有的資料都會遺失。
* 變更資料模型。 例如，新增 `EmailAddress` 欄位。
* 執行應用程式。
* `EnsureCreated` 會使用新的結構描述來建立資料庫。

只要您不需要保存資料，此工作流程在開發初期結構描述快速變化時的運作效果便會相當良好。 當資料輸入資料庫且需要進行保存時，狀況則會不同。 在這種情況下，請使用移轉。

在教學課程系列的稍後部分，您會刪除 `EnsureCreated` 建立的資料庫並改為使用移轉。 `EnsureCreated` 建立的資料庫無法使用移轉來更新。

### <a name="test-the-app"></a>測試應用程式

* 執行應用程式。
* 選取 [學生] 連結，然後選取 [新建]。
* 測試 [編輯]、[詳細資料] 和 [刪除] 連結。

## <a name="seed-the-database"></a>植入資料庫

`EnsureCreated` 方法會建立空白資料庫。 本節會新增程式碼以使用測試資料來填入資料庫。

使用下列程式碼建立 *Data/DbInitializer.cs*：

  [!code-csharp[Main](intro/samples/cu30snapshots/1-intro/Data/DbInitializer.cs)]

  程式碼會檢查資料庫中是否有任何學生。 若沒有任何學生，它便會將測試資料新增到資料庫。 它會以陣列的方式建立測試資料，而非 `List<T>` 集合，來最佳化效能。

* 在 *Program.cs* 中，將 `EnsureCreated` 呼叫替換成 `DbInitializer.Initialize` 呼叫：

  ```csharp
  // context.Database.EnsureCreated();
  DbInitializer.Initialize(context);
  ````

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

停止應用程式 (如果它正在執行)，並在**套件管理員主控台** (PMC) 中執行下列命令：

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 若應用程式正在執行中，請停止它，然後刪除 *CU.db* 檔案。

---

* 重新啟動應用程式。

* 選取 Students 頁面來查看植入的資料。

## <a name="view-the-database"></a>檢視資料庫

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* 從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。
* 在 SSOX 中，選取 [(localdb)\MSSQLLocalDB] > [資料庫] > [SchoolContext-{GUID}]。 資料庫名稱是以您稍早所提供的內容名稱加上虛線和 GUID 所產生。
* 展開 **Tables** 節點。
* 以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。
* 以滑鼠右鍵按一下 **Student** 資料表然後按一下 [檢視程式碼] 來查看 `Student` 模型對應到 `Student` 資料表結構描述的方式。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

使用 SQLite 工具來檢視資料庫結構描述和植入的資料。 資料庫檔案名為 *CU.db* 且位於專案資料夾中。

---

## <a name="asynchronous-code"></a>非同步程式碼

非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。

網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。 發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。 使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。 使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。 因此，非同步程式碼可以更有效率地使用伺服器資源，且伺服器可處理更多流量而不會造成延遲。

非同步程式碼會在執行階段導致少量的額外負荷。 在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。

在下列程式碼中，[async](/dotnet/csharp/language-reference/keywords/async) 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法會使程式碼以非同步方式執行。

```csharp
public async Task OnGetAsync()
{
    Students = await _context.Students.ToListAsync();
}
```

* `async` 關鍵字會指示編譯器：
  * 為方法主體的組件產生回呼。
  * 建立傳回的 [Task](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType) 物件。
* `Task<T>` 傳回型別代表正在進行的工作。
* `await` 關鍵字會導致編譯器將方法分成兩個組件。 第一個部分會與非同步啟動的作業一同結束。 第二個部分則會放入作業完成時呼叫的回呼方法中。
* `ToListAsync` 是 `ToList` 擴充方法的非同步版本。

若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：

* 只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。 其中包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。 不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。
* EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。
* 若要利用非同步程式碼所帶來的效能利益，請驗證該程式庫套件 (例如用於分頁) 在呼叫傳送查詢到資料庫的 EF Core 方法時使用 async。

如需非同步方法的詳細資訊，請參閱 [Async 概觀](/dotnet/standard/async)和[使用 Async 和 Await 設計非同步程式](/dotnet/csharp/programming-guide/concepts/async/)。

## <a name="next-steps"></a>後續步驟

> [!div class="step-by-step"]
> [下一個教學課程](xref:data/ef-rp/crud)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Contoso 大學的 Web 應用程式範例將示範如何以 Entity Framework (EF) Core 來建立 ASP.NET Core Razor Pages 應用程式。

這個範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。 此頁面是說明如何建立 Contoso 大學範例應用程式教學課程系列中的第一頁。

[下載或檢視已完成的應用程式。](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:index#how-to-download-a-sample)。

## <a name="prerequisites"></a>必要條件

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE [](~/includes/net-core-prereqs-windows.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE [](~/includes/2.1-SDK.md)]

---

熟悉 [Razor 頁面](xref:razor-pages/index)。 新進程式設計人員應先完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，再開始此系列。

## <a name="troubleshooting"></a>疑難排解

如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-rp/intro/samples)。 取得協助的好方法是將問題公佈到 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 以詢問 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。

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

如需上述步驟的影像，請參閱[ 建立 Razor Web 應用程式](xref:tutorials/razor-pages/razor-pages-start#create-a-razor-pages-web-app)。
執行應用程式。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

```dotnetcli
dotnet new webapp -o ContosoUniversity
cd ContosoUniversity
dotnet run
```

---

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

`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。 導覽屬性會連結至與此實體相關的其他實體。 在這個案例中，`Student entity` 中的 `Enrollments` 屬性會保有與 `Student` 相關的所有 `Enrollment` 實體。 例如，如果資料庫中 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性會包含這兩個 `Enrollment` 實體。 相關的 `Enrollment` 資料列，包含該學生在 `StudentID` 資料行中的主索引鍵值。 例如，假設 student with ID=1 在 `Enrollment` 資料表中有兩個資料列。 `Enrollment` 資料表有兩個包含 `StudentID`=1 的資料列。 `StudentID` 為 `Enrollment` 資料表中的外部索引鍵，會指定在 `Student` 資料表中的學生。

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

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

執行下列命令來 Scaffold 學生模型。

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 2.1.0
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator razorpage -m Student -dc ContosoUniversity.Models.SchoolContext -udl -outDir Pages/Students --referenceScriptLibraries
```

---

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

注意：上述程式碼針對命名空間 (`namespace ContosoUniversity.Models`) 使用 `Models`，而不是使用 `Data`。 `Models` 與產生框架的程式碼一致。 如需詳細資訊，請參閱[此 GitHub Scaffolding 問題](https://github.com/aspnet/Scaffolding/issues/822)。

程式碼會檢查資料庫中是否有任何學生。 如果資料庫中沒有學生，則會使用測試資料初始化資料庫。 它會將測試資料載入陣列之中，而非 `List<T>` 集合，以最佳化效能。

`EnsureCreated` 方法會自動為資料庫內容建立資料庫。 如果資料庫已存在，則 `EnsureCreated` 將會傳回而不修改該資料庫。

在 *Program.cs* 中，修改 `Main` 方法來呼叫 `Initialize`：

[!code-csharp[](intro/samples/cu21/Program.cs?name=snippet2&highlight=14-15)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

停止應用程式 (如果它正在執行)，並在**套件管理員主控台** (PMC) 中執行下列命令：

```powershell
Drop-Database
```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* 若應用程式正在執行中，請停止它，然後刪除 *CU.db* 檔案。

---

## <a name="view-the-db"></a>檢視資料庫

資料庫名稱是以您稍早所提供的內容名稱加上虛線和 GUID 所產生。 因此，資料庫名稱將會是 "SchoolContext-{GUID}"。 GUID 針對每個使用者都將不同。
從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。
在 SSOX 中，按一下 **(localdb)\MSSQLLocalDB > Databases > SchoolContext-{GUID}** 。

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

如需非同步方法的詳細資訊，請參閱 [Async 概觀](/dotnet/standard/async)和[使用 Async 和 Await 設計非同步程式](/dotnet/csharp/programming-guide/concepts/async/)。

在下一個教學課程中，將會檢視基本的 CRUD (建立、讀取、更新、刪除) 作業。



## <a name="additional-resources"></a>其他資源

* [這個教學課程的 YouTube 版本](https://www.youtube.com/watch?v=P7iTtQnkrNs)

> [!div class="step-by-step"]
> [下一個](xref:data/ef-rp/crud)

::: moniker-end
