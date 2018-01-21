---
title: "使用 Entity Framework Core 8 的教學課程 1 的 razor 頁面"
author: rick-anderson
description: "示範如何建立使用 Entity Framework Core 的 Razor 網頁應用程式"
ms.author: riande
manager: wpickett
ms.date: 11/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-rp/intro
ms.openlocfilehash: bea3b12ebe476c4b59abe117393b0ec8bb7f0306
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="getting-started-with-razor-pages-and-entity-framework-core-using-visual-studio-1-of-8"></a>開始使用 Razor 頁面與使用 Visual Studio (以 8 為 1) 的 Entity Framework Core

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework (EF) 核心 2.0 和 Visual Studio 2017 ASP.NET Core 2.0 MVC web 應用程式。

範例應用程式是針對虛構的 Contoso 大學的網站。 其中包括功能，例如許可學生、 課程建立和講師指派。 此頁面是一系列的說明如何建立 Contoso 大學範例應用程式的教學課程中的第一個。

[下載或檢視已完成的應用程式。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples) [下載指示](xref:tutorials/index#how-to-download-a-sample)。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

熟悉[Razor 頁面](xref:mvc/razor-pages/index)。 新的程式設計人員應該完成[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)再開始此數列。

## <a name="troubleshooting"></a>疑難排解

如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[完成階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)或[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/cu-final)。 如需常見的錯誤以及如何解決這些問題的清單，請參閱[數列中的最後一個教學課程疑難排解 > 一節](xref:data/ef-mvc/advanced#common-errors)。 如果您找不到您需要那里，您可以將問題張貼至[StackOverflow.com](https://stackoverflow.com/questions/tagged/asp.net-core)如[ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)或[EF 核心](https://stackoverflow.com/questions/tagged/entity-framework-core)。

> [!TIP]
> 這一系列的教學課程是根據所完成的作業在先前的教學課程。 請考慮每個成功的教學課程完成後儲存專案的複本。 如果您遇到問題時，您可以從上一個教學課程，而不是要開始啟動一段。 或者，您可以下載[完成階段](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-rp/intro/samples/StageSnapShots)然後再使用已完成的階段重新啟動。

## <a name="the-contoso-university-web-app"></a>Contoso 大學 web 應用程式

這些教學課程中建立的應用程式是基本大學的網站。

使用者可以檢視和更新學生、 課程、 和講師資訊。 以下是幾個教學課程中所建立的螢幕。

![學生索引頁](intro/_static/students-index.png)

![學生編輯頁面](intro/_static/student-edit.png)

此站台的 UI 樣式會接近內建的範本所產生的內容。 教學課程的重點是在 EF 核心 Razor 網頁，不包含 UI。

## <a name="create-a-razor-pages-web-app"></a>建立 Razor 頁面的 web 應用程式

* 從 Visual Studio 的 [檔案] 功能表中，選取 [新增] > [專案] 。
* 建立新的 ASP.NET Core Web 應用程式。 將專案命名**ContosoUniversity**。 請務必將專案命名*ContosoUniversity*使命名空間相符，當程式碼複製/貼上時。
 ![新增 ASP.NET Core Web 應用程式](intro/_static/np.png)
* 在下拉式清單中選取 [ASP.NET Core 2.0]，然後選取 [Web 應用程式]。
 ![Web 應用程式 (Razor 頁面)](../../mvc/razor-pages/index/_static/np2.png)

按 **F5** 在偵錯模式中執行應用程式，或按 **Ctrl-F5** 執行而不附加偵錯工具。

## <a name="set-up-the-site-style"></a>設定站台樣式

有一些變更設定的網站 功能表、 配置和首頁。

開啟*Pages/_Layout.cshtml*並進行下列變更：

* 將"ContosoUniversity"每次發生變更為"Contoso 大學。 」 有三個相符項目。

* 加入功能表項目**學生**，**課程**，**講師**，和**部門**，並刪除**連絡人**功能表項目。

所做的變更會反白顯示。 (所有標記都為*不*顯示。)

[!code-html[](intro/samples/cu/Pages/_Layout.cshtml?highlight=6,29,35-38,47&range=1-50)]

在*Pages/Index.cshtml*，ASP.NET MVC 的相關文字使用文字來取代此應用程式相關的下列程式碼取代檔案的內容：

[!code-html[](intro/samples/cu/Pages/Index.cshtml)]

按 CTRL+F5 執行專案。 在首頁上會顯示包含下列教學課程中建立的索引標籤：

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="create-the-data-model"></a>建立資料模型

建立 Contoso 大學應用程式的實體類別。 啟動具有下列三個實體：

![課程註冊學生資料模型圖表](intro/_static/data-model-diagram.png)

一對多關聯性之間`Student`和`Enrollment`實體。 一對多關聯性之間`Course`和`Enrollment`實體。 一位學生可以註冊任何數目的課程。 課程可以有任意數目的學生在它註冊。

在下列章節中，建立這些實體的每個類別。

### <a name="the-student-entity"></a>學生實體

![學生實體圖表](intro/_static/student-entity.png)

建立*模型*資料夾。 在*模型*資料夾中，建立名為的類別檔案*Student.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID`屬性會對應至這個類別的資料庫 (DB) 資料表的主索引鍵資料行。 根據預設，EF 核心會解譯此屬性，名為`ID`或`classnameID`為主索引鍵。

`Enrollments`屬性為導覽屬性。 導覽屬性連結到此實體與相關的其他實體。 在此情況下，`Enrollments`屬性`Student entity`保存所有的`Enrollment`的實體有關的`Student`。 例如，如果 DB 中的學生資料列有兩個相關註冊資料列，`Enrollments`導覽屬性包含這兩者`Enrollment`實體。 相關`Enrollment`資料列是包含該學生的主索引鍵的值中的資料列`StudentID`資料行。 例如，假設學生識別碼 = 1 具有兩個資料列`Enrollment`資料表。 `Enrollment`資料表包含兩個資料列`StudentID`= 1。 `StudentID`為外部索引鍵中`Enrollment`資料表中，指定在學生`Student`資料表。

如果導覽屬性都可以保存多個實體，瀏覽屬性必須是清單型別，例如`ICollection<T>`。 `ICollection<T>`您可以指定或型別，例如`List<T>`或`HashSet<T>`。 當`ICollection<T>`是使用 EF 核心建立`HashSet<T>`預設集合。 保存多個實體的導覽屬性是來自多對多 」 和 「 一對多關聯性。

### <a name="the-enrollment-entity"></a>註冊實體

![註冊的實體圖表](intro/_static/enrollment-entity.png)

在*模型*資料夾中，建立*Enrollment.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID`屬性是主索引鍵。 此實體會使用`classnameID`模式而不是`ID`像`Student`實體。 通常開發人員會選擇其中一個模式，並將它整個資料模型。 在稍後的教學課程中，使用識別碼，而類別名稱不會顯示若要簡化資料模型中實作繼承。

`Grade`屬性是`enum`。 問號之後`Grade`型別宣告表示`Grade`屬性可為 null。 為 null 的等級是不同於零等級--null 表示等級不未知或尚未被指派。

`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。 `Enrollment`實體都與一個`Student`實體，因此該屬性包含單一`Student`實體。 `Student`實體與`Student.Enrollments`導覽屬性，其中包含多個`Enrollment`實體。

`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。 `Enrollment`實體都與一個`Course`實體。

EF 核心會解譯為外部索引鍵屬性如果名稱為`<navigation property name><primary key property name>`。 例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`。 外部索引鍵屬性也名為`<primary key property name>`。 例如，`CourseID`因為`Course`實體的主索引鍵是`CourseID`。

### <a name="the-course-entity"></a>課程實體

![課程實體圖表](intro/_static/course-entity.png)

在*模型*資料夾中，建立*Course.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments`屬性為導覽屬性。 A`Course`可以與任意數目的相關實體`Enrollment`實體。

`DatabaseGenerated`屬性可讓應用程式，以指定的主索引鍵，而不是具有 DB 產生它。

## <a name="create-the-schoolcontext-db-context"></a>建立 SchoolContext DB 內容

協調 EF 核心功能，為給定的資料模型的主要類別是 DB 內容類別。 資料內容衍生自`Microsoft.EntityFrameworkCore.DbContext`。 資料內容會指定資料模型中包含哪些實體。 在此專案中，類別會命名為`SchoolContext`。

在專案資料夾中，建立名為資料夾*資料*。

在*資料*資料夾建立*SchoolContext.cs*為下列程式碼：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

此程式碼建立`DbSet`每一個實體集的屬性。 在 EF 核心術語：

* 實體集合通常會對應至資料庫資料表。
* 實體對應到資料表中的資料列。

`DbSet<Enrollment>`和`DbSet<Course>`可以省略。 EF 核心包含這些隱含因為`Student`實體參考`Enrollment`實體，而`Enrollment`實體參考`Course`實體。 本教學課程中，保留`DbSet<Enrollment>`和`DbSet<Course>`中`SchoolContext`。

EF 核心建立資料庫時，會建立具有相同名稱的資料表`DbSet`屬性名稱。 集合的屬性名稱都是通常複數 （學生而不是學生）。 開發人員不同意有關資料表名稱是否應該使用複數。 如需這些教學課程中，預設行為會覆寫指定的 DbContext 單數資料表名稱。 若要指定單一的資料表名稱，加入下列反白顯示的程式碼：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>暫存器具有相依性插入的內容

包含 ASP.NET Core[相依性插入](xref:fundamentals/dependency-injection)。 在應用程式啟動時的相依性插入會註冊服務 （例如 EF 核心 DB 內容）。 需要這些服務 （例如 Razor 頁面） 的元件會提供這些服務，透過建構函式參數。 取得 db 內容執行個體的建構函式程式碼是稍後在本教學課程中所示。

若要註冊`SchoolContext`為服務時，開啟*Startup.cs*，並加入要反白顯示的行`ConfigureServices`方法。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

連接字串的名稱會傳遞至內容所呼叫的方法上`DbContextOptionsBuilder`物件。 本機開發， [ASP.NET Core 組態系統](xref:fundamentals/configuration/index)讀取連接字串從*appsettings.json*檔案。

新增`using`陳述式`ContosoUniversity.Data`和`Microsoft.EntityFrameworkCore`命名空間。 建置專案。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

開啟*appsettings.json*檔案，然後加入連接字串，如下列程式碼所示：

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

上述的連接字串使用`ConnectRetryCount=0`防止[SQLClient](https://docs.microsoft.com/dotnet/framework/data/adonet/ef/sqlclient-for-the-entity-framework)從停滯。

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

連接字串會指定 SQL Server LocalDB 資料庫。 LocalDB 是輕量版 SQL Server Express Database Engine 和適用於應用程式開發，而不是生產環境使用。 LocalDB 會視需要啟動，並以使用者模式執行，因此沒有複雜的組態。 根據預設，建立 LocalDB *.mdf* DB 中的檔案`C:/Users/<user>`目錄。

## <a name="add-code-to-initialize-the-db-with-test-data"></a>加入程式碼以初始化測試資料的資料庫

EF 核心會建立空的資料庫。 在本節中，*種子*會寫入測試的資料填入的方法。

在*資料*資料夾中，建立新的類別檔案命名為*DbInitializer.cs*並加入下列程式碼：

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

程式碼會檢查是否有任何學生 DB 中。 如果在資料庫中有沒有學生，測試資料植入資料庫。 它將測試資料載入至陣列而非`List<T>`集合，以最佳化效能。

`EnsureCreated`方法會自動建立的 DB DB 內容。 若 DB 存在，則`EnsureCreated`傳回而不需修改資料庫。

在*Program.cs*，修改`Main`方法來執行下列動作：

* 從相依性插入容器中取得 DB 內容執行個體。
* 呼叫種子方法，將內容傳遞給它。
* Seed 方法完成時處置內容。

下列程式碼將示範更新*Program.cs*檔案。

[!code-csharp[Main](intro/samples/cu/ProgramOriginal.cs?name=snippet)]

第一次執行應用程式時，資料庫會建立並植入的測試資料。 更新資料模型時：
* 刪除資料庫。
* 更新 seed 方法。
* 執行應用程式，並建立新的植入的資料庫。 

在之後的教學課程中的資料模型變更，而不需要刪除並重新建立資料庫時，會更新資料庫。

<a name="pmc"></a>
## <a name="add-scaffold-tooling"></a>加入 scaffold 工具

在本節中，封裝管理員主控台 (PMC) 用來新增 Visual Studio web 程式碼產生封裝。 執行 Scaffolding 引擎需要此套件。

從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

在 封裝管理員主控台 (PMC)，請輸入下列命令：

```powershell
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Design
Install-Package Microsoft.VisualStudio.Web.CodeGeneration.Utils
```

前一個命令會將 *.csproj 檔案中的 NuGet 封裝：

[!code-csharp[Main](intro/samples/cu/ContosoUniversity1_csproj.txt?highlight=7-8)]

<a name="scaffold"></a>
## <a name="scaffold-the-model"></a>Scaffold 模型

* 在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。
* 執行下列命令：


 ```console
dotnet restore
dotnet aspnet-codegenerator razorpage -m Student -dc SchoolContext -udl -outDir Pages\Students --referenceScriptLibraries
 ```
 
如果會產生下列錯誤：

```text
Unhandled Exception: System.IO.FileNotFoundException: 
Could not load file or assembly 
'Microsoft.VisualStudio.Web.CodeGeneration.Utils, 
Version=2.0.0.0, Culture=neutral, PublicKeyToken=adb9793829ddae60'.
The system cannot find the file specified.
```

重新執行命令，並將在頁面底部的註解。

如果您收到錯誤：
  ```
No executable found matching command "dotnet-aspnet-codegenerator"
  ```

在專案目錄 (包含 *Program.cs*、*Startup.cs* 和 *.csproj* 檔案的目錄) 中開啟一個命令視窗。


建置專案。 建置會產生類似如下的錯誤：

 `1>Pages\Students\Index.cshtml.cs(26,38,26,45): error CS1061: 'SchoolContext' does not contain a definition for 'Student'`

 全域變更`_context.Student`至`_context.Students`(亦即，加入"s" `Student`)。 找到項目 7 及更新。 我們希望修正[這個 bug](https://github.com/aspnet/Scaffolding/issues/633)的下一個版本。

[!INCLUDE[model4tbl](../../includes/RP/model4tbl.md)]

 <a name="test"></a>
### <a name="test-the-app"></a>測試應用程式

執行應用程式並選取**學生**連結。 根據瀏覽器寬度，**學生**連結出現在頁面頂端。 如果**學生**連結不可見，請按一下右上角的 [導覽] 圖示。

![Contoso 大學首頁窄](intro/_static/home-page-narrow.png)

測試**建立**，**編輯**，和**詳細資料**連結。

## <a name="view-the-db"></a>檢視資料庫

應用程式啟動時，`DbInitializer.Initialize`呼叫`EnsureCreated`。 `EnsureCreated`如果資料庫存在，而且若有需要，請建立一個偵測。 如果在 DB 中，沒有任何學生`Initialize`方法會將學生。

開啟**SQL Server 物件總管**(SSOX) 從**檢視**Visual Studio 中的功能表。
在 SSOX，按一下  **(localdb) \MSSQLLocalDB > 資料庫 > ContosoUniversity1**。

展開**資料表**節點。

以滑鼠右鍵按一下**學生**資料表，並按一下**檢視資料**若要查看建立的資料行和資料列插入資料表。

*.Mdf*和*.ldf* DB 檔案位於*C:\Users\\ <yourusername>* 資料夾。

`EnsureCreated`啟動應用程式，可讓下列工作流程上呼叫：

* 刪除資料庫。
* 變更資料庫結構描述 (例如，加入`EmailAddress`欄位)。
* 執行應用程式。

`EnsureCreated`建立與 DB`EmailAddress`資料行。

## <a name="conventions"></a>慣例

若要建立完整資料庫的 EF 核心順序撰寫的程式碼數量是最小因為使用的慣例或讓 EF 核心的假設。

* 名稱`DbSet`屬性會用做資料表的名稱。 實體沒有參考`DbSet`屬性，實體類別名稱會當做資料表名稱。

* 實體屬性名稱會用於資料行名稱。

* 實體屬性會在名為 ID 或 classnameID 辨識為主索引鍵屬性。

* 屬性會解譯為外部索引鍵屬性上，如果名稱為 *<navigation property name> <primary key property name>*  (例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`). 外部索引鍵屬性都可以具名 *<primary key property name>*  (例如，`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。

傳統行為可以被覆寫。 例如，資料表名稱可以明確指定，如稍早在本教學課程中所示。 可以明確設定資料行名稱。 主索引鍵和外部索引鍵可以明確設定。

## <a name="asynchronous-code"></a>非同步程式碼

非同步程式設計是預設的 ASP.NET Core 和 EF 核心模式。

Web 伺服器的有限的數目的執行緒可用，而且在高負載情況下的所有可用的執行緒可能正在使用中。 當發生這種情況時，伺服器無法處理新的要求，直到執行緒釋放。 使用同步程式碼，許多執行緒可能會將繫結起來雖然它們實際上並不執行任何工作，因為他們正在等候 I/O 完成。 使用非同步程式碼，當處理程序在等候 I/O 完成，它的執行緒釋放針對伺服器將用於處理其他要求。 如此一來，非同步程式碼可讓更有效率地使用伺服器資源，而且伺服器已啟用以處理更多流量不會造成延遲。

非同步程式碼會在執行階段導致少量的額外負荷。 低流量的情況下，效能的衝擊，微不足道時的高流量的情況下，可能很大的潛在的效能改善。

下列程式碼，`async`關鍵字，`Task<T>`傳回值，`await`關鍵字和`ToListAsync`方法提供程式碼以非同步方式執行。

[!code-csharp[Main](intro/samples/cu/Pages/Students/Index.cshtml.cs?name=snippet_ScaffoldedIndex)]

* `async`關鍵字會指示編譯器將：

  * 產生的組件的方法主體的回呼。
  * 自動建立[工作](https://docs.microsoft.com/dotnet/api/system.threading.tasks.task?view=netframework-4.7)傳回物件。 如需詳細資訊，請參閱[工作傳回型別](https://docs.microsoft.com/dotnet/csharp/programming-guide/concepts/async/async-return-types#BKMK_TaskReturnType)。

* 隱含傳回型別`Task`代表進行中的工作。

* `await`關鍵字會導致編譯器分成兩個部分的方法。 以非同步方式啟動的作業結束的第一個部分。 第二個部分會放入作業完成時呼叫的回呼方法。

* `ToListAsync`非同步版本`ToList`擴充方法。

若要撰寫使用 EF 核心的非同步程式碼時應該注意的事項：

* 只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。 包含`ToListAsync`， `SingleOrDefaultAsync`， `FirstOrDefaultAsync`，和`SaveChangesAsync`。 它不包含只變更陳述式`IQueryable`，例如`var students = context.Students.Where(s => s.LastName == "Davolio")`。

* EF 核心內容不執行緒安全： 不要嘗試執行多個平行作業。 

* 若要利用非同步程式碼的效能優勢，確認程式庫封裝 （例如分頁） 使用非同步呼叫查詢傳送至資料庫的 EF 核心方法。

如需在.NET 非同步程式設計的詳細資訊，請參閱[非同步概觀](https://docs.microsoft.com/dotnet/articles/standard/async)。

在下一個教學課程中，基本 CRUD （建立、 讀取、 更新、 刪除） 作業會檢查。

>[!div class="step-by-step"]
[下一步](xref:data/ef-rp/crud)
