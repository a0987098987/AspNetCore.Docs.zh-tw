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
# <a name="razor-pages-with-entity-framework-core-in-aspnet-core---tutorial-1-of-8"></a>ASP.NET Core 中的 Razor 頁面與 Entity Framework Core 教學課程 - 1/8

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學的範例 Web 應用程式將示範如何以 Entity Framework (EF) Core 2.0 和 Visual Studio 2017 來建立 ASP.NET Core 2.0 MVC Web 應用程式。

這個範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。 此頁面是說明如何建立 Contoso 大學範例應用程式教學課程系列中的第一頁。

[下載或檢視已完成的應用程式。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:tutorials/index#how-to-download-a-sample)。

## <a name="prerequisites"></a>必要條件

[!INCLUDE [](~/includes/net-core-prereqs.md)]

熟悉 [Razor 頁面](xref:razor-pages/index)。 新進程式設計人員應先完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，再開始此系列。

## <a name="troubleshooting"></a>疑難排解

如果您遭遇無法解決的問題，將您的程式碼與[已完成的階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)作比較，通常可以找到解答。 如需常見的錯誤以及如何解決這些問題的清單，請參閱[ 數列中的最後一個教學課程疑難排解 > 一節](xref:data/ef-mvc/advanced#common-errors)。 如果您在那裡找不到所需的資訊，可以將問題張貼至 [StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core) 中的 [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。

> [!TIP]
> 本系列教學課程建立於先前已完成的教學課程上。 成功完成每一個教學課程後，請儲存專案的複本。 如果您遇到問題，您可以從上一個教學課程來重新開始，而不需從頭開始。 或者，您也可以下載[已完成的階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots) 並以此階段重新開始。

## <a name="the-contoso-university-web-app"></a>Contoso 大學 Web 應用程式

在教學課程中建立的應用程式，是一個基本的大學網站。

使用者可以檢視和更新學生、課程和教師資訊。 以下是幾個在教學課程中建立的畫面。

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

此網站的 UI 樣式接近內建範本所產生的內容。 教學課程聚焦於 EF Core 與 Razor 頁面，而不是 UI。

## <a name="create-a-razor-pages-web-app"></a>建立 Razor 頁面 Web 應用程式

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名為 **ContosoUniversity**。 請務必將專案命名為 *ContosoUniversity*，當您複製/貼上程式碼時，命名空間才會相符。
 ![新增 ASP.NET Core Web 應用程式](intro/_static/np.png)
* 在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。
 ![Web 應用程式 (Razor 頁面)](../../razor-pages/index/_static/np2.png)

按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。

## <a name="set-up-the-site-style"></a>設定網站樣式

一些變更設定了網站的功能表、配置和首頁。

開啟 *Pages/_Layout.cshtml* 並進行下列變更：

* 將每一個 "ContosoUniversity" 的發生次數變更為 "Contoso University"。 共有三個發生次數。

* 為 **Students**、**Courses**、**Instructors**、**Departments** 新增功能表項目，並刪除 **Contact** 功能表項目。

所做的變更已醒目標示。 (所有標記都「不會」顯示。)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

在 *Pages/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

按 CTRL+F5 執行專案。 首頁會以從下列教學課程中建立的索引標籤來顯示：

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>建立資料模型

為 Contoso 大學應用程式建立實體類別。 從下列三個實體開始：

![Course、Enrollment、Student 資料模型圖表](intro/_static/data-model-diagram.png)

在 `Student` 和 `Enrollment` 實體之間會有一對多關聯性。 在 `Course` 和 `Enrollment` 實體之間會有一對多關聯性。 一位學生可註冊任何數目的課程。 一堂課程可以讓任意數目的學生註冊。

在下列一節中，將為這些實體建立各自的類別。

### <a name="the-student-entity"></a>Student 實體

![Student 實體圖表](intro/_static/student-entity.png)

建立 *Models* 資料夾。 在 *Models* 資料夾中，用下列程式碼建立一個名為 *Student.cs* 的類別檔案：

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` 屬性會成為資料庫 (DB) 資料表中的主索引鍵資料行，並對應至這個類別。 EF Core 預設會解譯名為 `ID` 或 `classnameID` 作為主索引鍵的屬性。 在 `classnameID` 中，`classname` 是類別的名稱，例如上述範例中的 `Student`。

`Enrollments` 屬性為導覽屬性。 導覽屬性會連結至與此實體相關的其他實體。 在這個案例中，`Student entity` 中的 `Enrollments` 屬性會保有與 `Student` 相關的所有 `Enrollment` 實體。 例如，如果資料庫中 Student 資料列有兩個相關的 Enrollment 資料列，則 `Enrollments` 導覽屬性會包含這兩個 `Enrollment` 實體。 相關的 `Enrollment` 資料列，包含該學生在 `StudentID` 資料行中的主索引鍵值。 例如，假設 student with ID=1 在 `Enrollment` 資料表中有兩個資料列。 `Enrollment` 資料表有兩個包含 `StudentID`=1 的資料列。 `StudentID` 為 `Enrollment` 資料表中的外部索引鍵，會指定在 `Student` 資料表中的學生。

如果導覽屬性可以保存多個實體，該導覽屬性必然是清單類型，例如 `ICollection<T>`。 您可以指定 `ICollection<T>`，或是如 `List<T>`、`HashSet<T>` 的類型。 如使用 `ICollection<T>`，EF Core 預設將建立 `HashSet<T>` 集合。 保存多個實體的導覽屬性，來自多對多和一對多關聯性。

### <a name="the-enrollment-entity"></a>Enrollment 實體

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

在 *Models* 資料夾中，用下列程式碼建立 *Enrollment.cs*：

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` 屬性是主索引鍵。 這個實體會使用 `classnameID` 模式，而不是像 `Student` 實體一樣的 `ID`。 開發人員通常會選擇其中一個模式，並使用於整個資料模型。 在稍後的教學課程中，將示範使用不具類別名稱的 ID，使得在數據模型中實作繼承更加簡單。

`Grade` 屬性為 `enum`。 問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。 為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 一個 `Enrollment` 實體與一個 `Student` 實體建立關聯，因此該屬性包含單一 `Student` 實體。 `Student` 實體與 `Student.Enrollments` 導覽屬性不同，導覽屬性包含多個 `Enrollment` 實體。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

如果實體名為 `<navigation property name><primary key property name>`，則 EF Core 會將之解釋為外部索引鍵。 例如，`StudentID` 為 `Student` 導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`。 外部索引鍵屬性也可命名為 `<primary key property name>`。 例如 `CourseID`，因為 `Course` 實體的主索引鍵是 `CourseID`。

### <a name="the-course-entity"></a>Course 實體

![Course 實體圖表](intro/_static/course-entity.png)

在 *Models* 資料夾中，用下列程式碼建立 *Course.cs*：

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

`DatabaseGenerated` 屬性 (attribute) 可讓應用程式指定主索引鍵，而不是讓 DB 來產生。

## <a name="create-the-schoolcontext-db-context"></a>建立 SchoolContext DB 內容

為給定的資料模型協調 EF Core 功能的主要類別是資料庫內容類別。 資料內容衍生自 `Microsoft.EntityFrameworkCore.DbContext`。 資料內容會指定資料模型包含哪些實體。 在此專案中，類別命名為 `SchoolContext`。

在專案資料夾中，建立名為 *Data* 的資料夾。

在 *Data* 資料夾中以下列程式碼建立 *SchoolContext.cs*：

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

程式碼會為每一個實體集建立 `DbSet` 屬性。 在 EF Core 用語中：

* 實體集通常會對應至資料庫資料表。
* 實體會對應至資料表中的資料列。

`DbSet<Enrollment>` 和 `DbSet<Course>` 可以省略。 EF Core 會隱含它們，因為 `Student` 實體引用 `Enrollment` 實體，而 `Enrollment` 實體引用 `Course` 實體。 本教學課程中，請在 `SchoolContext` 保留 `DbSet<Enrollment>` 和 `DbSet<Course>`。

資料庫建立時，EF Core 會建立和 `DbSet` 屬性名稱相同的資料表。 集合的屬性名稱通常是複數 (Students 而不是 Student）。 針對是否要讓資料表名稱都使用複數，開發人員並沒有共識。 在此系列教學課程中，預設行為可藉由指定 DbContext 中的單數資料表名稱來覆寫。 若要指定單數資料表名稱，請新增下列醒目標示的程式碼：

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>使用相依性插入來註冊內容

ASP.NET Core 包含了[相依性插入](xref:fundamentals/dependency-injection)。 服務 (例如 EF Core DB 內容) 是在應用程式啟動期間使用相依性插入來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 Razor 頁面) 來提供服務。 取得資料庫內容執行個體的建構函式程式碼，在本教學課程稍後會示範。

若要將 `SchoolContext` 註冊為服務，請開啟 *Startup.cs*，並將醒目標示的程式碼新增至 `ConfigureServices` 方法。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

連接字串的名稱，會透過呼叫 `DbContextOptionsBuilder` 物件上的方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。

為 `ContosoUniversity.Data` 和 `Microsoft.EntityFrameworkCore` 命名空間新增 `using` 陳述式。 建置專案。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

請開啟 *appsettings.json* 檔案並新增連接字串，如下列程式碼所示：

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

上述的連接字串使用 `ConnectRetryCount=0` 以防止 [SQLClient](/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework) 停滯。

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

連接字串會指定 SQL Server LocalDB 資料庫。 LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程序開發，而不是生產用途。 LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。 LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。

## <a name="add-code-to-initialize-the-db-with-test-data"></a>新增程式碼來初始化含有測試資料的資料庫

EF Core 會建立空白資料庫。 在本節中，會寫入「種子」方法並以測試資料來填入該資料庫。

在 *Data* 資料夾中，建立名為 *DbInitializer.cs* 的新類別檔案並新增下列程式碼：

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

程式碼會檢查資料庫中是否有任何學生。 如果資料庫中沒有學生，測試資料將會植入資料庫。 會將測試資料載入至陣列而非 `List<T>` 集合，以最佳化效能。

`EnsureCreated` 方法會自動為資料庫內容建立資料庫。 如果資料庫已存在，則 `EnsureCreated` 將會傳回而不修改該資料庫。

在 *Program.cs* 中，修改 `Main` 方法來執行下列動作：

* 從相依性插入容器中取得資料庫內容執行個體。
* 呼叫種子方法，並將其傳遞給內容。
* 種子方法完成時處理內容。

下列程式碼顯示已更新的 *Program.cs* 檔案。

[!code-csharp[](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

第一次執行應用程式時，會建立資料庫並植入測試資料。 更新資料模型時：
* 刪除資料庫。
* 更新種子方法。
* 執行應用程式，並建立新的已植入資料庫。 

在之後的教學課程中，資料模型變更但不需要刪除和重新建立資料庫時，將會更新資料庫。

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>新增 Scaffold 工具

在本節中，套件管理員主控台 (PMC) 將用來新增 Visual Studio web 程式碼產生套件。 執行 Scaffolding 引擎需要此套件。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

請在套件管理員主控台 (PMC) 中輸入下列命令：

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

上述命令會將 NuGet 套件新增至 *.csproj 檔案：

[!code-xml[](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Scaffold 模型

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 執行下列命令：


 ```console
dotnet restore
dotnet tool install --global dotnet-aspnet-codegenerator --version 2.1.0
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```

如果您收到錯誤：
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。


建置專案。 建置時會產生如下錯誤：

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 將 `_context.Student` 全域變更為 `_context.Students` (亦即，在 `Student` 的名稱上新增一個 "s")。 找到並更新 7 個發生次數。 我們希望下一個版本能修正[這個 Bug](https://github.com/aspnet/Scaffolding/issues/633)。

[!INCLUDE [model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>測試應用程式

執行應用程式並選取 **Students** 連結。 根據瀏覽器的寬度，**Students** 連結將會出現在頁面頂端。 如果看不到 **Students** 連結，按一下右上角的導覽圖示。

![Contoso 大學首頁 (窄)](intro/_static/home-page-narrow.png)

測試**建立**、**編輯**和**詳細資料**連結。

## <a name="view-the-db"></a>檢視資料庫

應用程式啟動時，`DbInitializer.Initialize` 呼叫 `EnsureCreated`。 `EnsureCreated` 會偵測資料庫是否已存在，並會在需要時建立一個資料庫。 如果資料庫中沒有學生，`Initialize` 方法將會新增學生。

從 Visual Studio 中的 **View** 功能表開啟 **SQL Server 物件總管** (SSOX)。
在 SSOX 中，按一下 **(localdb) \MSSQLLocalDB > Databases > ContosoUniversity1**。

展開 **Tables** 節點。

以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料] 查看建立的資料行、插入資料表中的資料列。

<em>.Mdf</em> 和 <em>.ldf</em> 資料庫檔案位於 <em>C:\Users\\<yourusername></em> 資料夾中。

應用程式啟動時會呼叫 `EnsureCreated` 以允許下列工作流程：

* 刪除資料庫。
* 變更資料庫結構描述 (例如新增 `EmailAddress` 欄位)。
* 執行應用程式。

`EnsureCreated` 會建立包含 `EmailAddress` 資料行的資料庫。

## <a name="conventions"></a>慣例

若要以 EF Core 來建立一個完整的資料庫，所需的程式碼數量非常小，是因為使用慣例或 EF Core 所做之假設的緣故。

* `DbSet` 屬性的名稱會用作資料表的名稱。 針對 `DbSet` 屬性並未參考的實體，實體類別名稱會用於資料表名稱。

* 實體屬性名稱會用於資料行名稱。

* 命名為 ID 或 classnameID 的實體屬性，會辨識為主索引鍵屬性。

* 如果屬性已具名 *<navigation property name><primary key property name>* (例如，`StudentID` 為 `Student` 的導覽屬性，因為 `Student` 實體的主索引鍵是 `ID`)，會將屬性解譯為外部索引鍵屬性。 外部索引鍵屬性都可以具名 *<primary key property name>*(例如 `EnrollmentID`，因為 `Enrollment` 實體的主索引鍵是 `EnrollmentID`)。

慣例行為可以被覆寫。 例如，資料表名稱可以明確指定，如稍早在本教學課程中所示。 可以明確設定資料行名稱。 主索引鍵和外部索引鍵可以明確設定。

## <a name="asynchronous-code"></a>非同步程式碼

非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。

網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。 發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。 使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。 使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。 因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。

非同步程式碼會在執行階段導致少量的額外負荷。 在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。

在下列程式碼中，`async` 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和`ToListAsync` 方法使程式碼以非同步方式執行。

[!code-csharp[](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async` 關鍵字會指示編譯器：

  * 為方法主體的組件產生回呼。
  * 自動建立傳回的 [Task](/dotnet/api/system.threading.tasks.task?view=netframework-4.7) 物件。 如需詳細資訊，請參閱 [Task 傳回型別](/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。

* 隱含傳回型別 `Task` 代表進行中的工作。

* `await` 關鍵字會導致編譯器將方法分成兩個組件。 第一個部分會與非同步啟動的作業一同結束。 第二個部分則會放入作業完成時呼叫的回呼方法中。

* `ToListAsync` 是 `ToList` 擴充方法的非同步版本。

若要撰寫使用 EF Core 的非同步程式碼，請注意下列事項：

* 只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。 包含 `ToListAsync`、`SingleOrDefaultAsync`、`FirstOrDefaultAsync` 和 `SaveChangesAsync`。 不會包含只變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。

* EF Core 內容在執行緒中並不安全：不要嘗試執行多個平行作業。 

* 若要利用非同步程式碼的效能優勢，請確認程式庫套件 (例如分頁) 在呼叫將查詢傳送至資料庫的 EF Core 方法時是使用非同步。

如需在 .NET 中非同步程式設計的詳細資訊，請參閱[非同步總覽](/dotnet/articles/standard/async)。

在下一個教學課程中，將會檢視基本的 CRUD (建立、讀取、更新、刪除) 作業。

> [!div class="step-by-step"]
> [下一步](xref:data/ef-rp/crud)
