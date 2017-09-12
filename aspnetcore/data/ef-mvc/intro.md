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
ms.openlocfilehash: 949733119b4e3a4b8716f2bcc1f631949d5049bc
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/12/2017
---
# <a name="getting-started-with-aspnet-core-mvc-and-entity-framework-core-using-visual-studio-1-of-10"></a>開始使用 ASP.NET Core MVC 和 Entity Framework Core 使用 Visual Studio (1 / 10)

由[Tom Dykstra](https://github.com/tdykstra)和[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework (EF) 核心 2.0 和 Visual Studio 2017 ASP.NET Core 2.0 MVC web 應用程式。

範例應用程式是針對虛構的 Contoso 大學的網站。 其中包括功能，例如許可學生、 課程建立和講師指派。 這是一系列的教學課程說明如何建置從頭 Contoso 大學範例應用程式中的第一個。

[下載或檢視完成的應用程式。](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

EF 核心 2.0 EF 的最新版本，但還沒有的 EF 的所有功能 6.x。 如需有關如何選擇 EF 資訊 6.x 和 EF 核心，請參閱[EF 核心 vs。EF6.x](https://docs.microsoft.com/ef/efcore-and-ef6/)。 如果您選擇 EF 6.x，請參閱[此教學課程系列的舊版](https://docs.microsoft.com/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)。

> [!NOTE]
> * 如需本教學課程的 ASP.NET Core 1.1 版本，請參閱[本教學課程以 PDF 格式的 VS 2017 Update 2 版本](https://github.com/aspnet/Docs/blob/master/aspnetcore/data/efmvc/intro/_static/efmvc1.1.pdf)。
> * 如需本教學課程的 Visual Studio 2015 版本，請參閱 [PDF 格式的 VS 2015 版本 ASP.NET 核心文件集](https://github.com/aspnet/Docs/blob/master/aspnetcore/common/_static/aspnet-core-project-json.pdf)。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[install 2.0](../../includes/install2.0.md)]

## <a name="troubleshooting"></a>疑難排解

如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/aspnet/Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)。 如需常見的錯誤以及如何解決這些問題的清單，請參閱[數列中的最後一個教學課程疑難排解 > 一節](advanced.md#common-errors)。 如果您找不到您需要那里，您可以張貼問題的 StackOverflow.com [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core)或[EF 核心](https://stackoverflow.com/questions/tagged/entity-framework-core)。

> [!TIP] 
> 這是一系列 10 教學課程，其中每一個都是根據所完成的作業在先前的教學課程。  請考慮每個成功的教學課程完成後儲存專案的複本。  然後如果您遇到問題時，您可以透過從啟動上一個教學課程，而不是回到整個序列的開頭。

## <a name="the-contoso-university-web-application"></a>Contoso 大學 web 應用程式

您會在這些教學課程建置的應用程式是簡單的大學的網站。

使用者可以檢視和更新學生、 課程、 和講師資訊。 以下是幾個您要建立的畫面。

![學生索引頁](intro/_static/students-index.png)

![學生編輯頁面](intro/_static/student-edit.png)

此站台的 UI 樣式保留接近內建的範本，所產生的內容，讓本教學課程可以主要還是著重於如何使用 Entity Framework。

## <a name="create-an-aspnet-core-mvc-web-application"></a>建立 ASP.NET Core MVC web 應用程式

開啟 Visual Studio 並建立新 ASP.NET Core C# 的 web 專案名為"ContosoUniversity"。

* 從**檔案**功能表上，選取**新增 > 專案**。

* 從左窗格中，選取**已安裝 > Visual C# > Web**。

* 選取**ASP.NET Core Web 應用程式**專案範本。

* 輸入**ContosoUniversity**做為名稱，然後按一下**確定**。

  ![[新增專案] 對話](intro/_static/new-project.png)

* 等候**新 ASP.NET Core Web 應用程式 (.NET Core)**出現對話方塊

* 選取**ASP.NET Core 2.0**和**Web 應用程式 （模型-檢視-控制器）**範本。

  **注意：**本教學課程需要 ASP.NET Core 2.0 和 EF 核心 2.0 或更新版本-請確認**ASP.NET Core 1.1**未選取。

* 請確定**驗證**設**非驗證**。

* 按一下 [確定] 

  ![新增 ASP.NET 專案 對話方塊](intro/_static/new-aspnet.png)

## <a name="set-up-the-site-style"></a>設定站台樣式

有一些簡單的變更將會設定網站 功能表、 配置和首頁。

開啟*Views/Shared/_Layout.cshtml*並進行下列變更：

* 將"ContosoUniversity"每次發生變更為"Contoso 大學"。 有三個相符項目。

* 加入功能表項目**學生**，**課程**，**講師**，和**部門**，並刪除**連絡人**功能表項目。

所做的變更會反白顯示。

[!code-html[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,30,36-39,48)]

在*Views/Home/Index.cshtml*，ASP.NET MVC 的相關文字使用文字來取代此應用程式有關的下列程式碼取代檔案的內容：

[!code-html[](intro/samples/cu/Views/Home/Index.cshtml)]

按 CTRL + F5 執行專案，或選擇**偵錯 > 啟動但不偵錯**從功能表。 您會看到這些教學課程中，您將建立之頁面的索引標籤的 [首頁] 頁面。

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="entity-framework-core-nuget-packages"></a>Entity Framework Core NuGet 封裝

若要將 EF Core 支援加入至專案，安裝您要當做目標的資料庫提供者。 本教學課程使用 SQL Server，而且提供者套件可[Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。 此套件包含在[Microsoft.AspNetCore.All](xref:fundamentals/metapackage) metapackage，所以您不需要安裝它。

這個封裝和其相依性 (`Microsoft.EntityFrameworkCore`和`Microsoft.EntityFrameworkCore.Relational`) 提供 EF 的執行階段支援。 您會加入工具封裝稍後在[移轉](migrations.md)教學課程。 

其他資料庫提供者可用於 Entity Framework Core 的相關資訊，請參閱[資料庫提供者](https://docs.microsoft.com/ef/core/providers/)。

## <a name="create-the-data-model"></a>建立資料模型

接下來您將建立 Contoso 大學應用程式的實體類別。 您會先處理下列三個實體。

![課程註冊學生資料模型圖表](intro/_static/data-model-diagram.png)

一對多關聯性之間`Student`和`Enrollment`實體，而且沒有之間的一對多關聯性`Course`和`Enrollment`實體。 換句話說，一位學生可以註冊任何數目的課程中，而且一個課程可以有任意數目的學生在它註冊。

下列各節中，您將建立這些實體的每個類別。

### <a name="the-student-entity"></a>學生實體

![學生實體圖表](intro/_static/student-entity.png)

在*模型*資料夾中，建立名為的類別檔案*Student.cs*和範本程式碼取代為下列程式碼。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID`屬性就會成為主要的索引鍵資料行對應至這個類別的資料庫資料表。 根據預設，Entity Framework 會解譯此屬性，名為`ID`或`classnameID`為主索引鍵。

`Enrollments`屬性為導覽屬性。 導覽屬性會保留此實體與相關的其他實體。 在此情況下，`Enrollments`屬性`Student entity`保存所有`Enrollment`的實體有關的`Student`實體。 換句話說，如果給定的學生資料列，在資料庫中有兩個相關註冊資料列 （資料列包含該學生的主索引鍵值其 StudentID 外部索引鍵資料行中），可`Student`實體的`Enrollments`導覽屬性會包含那些兩個`Enrollment`實體。

如果導覽屬性都可以保存多個實體 （如同多對多或一對多的關聯性），其類型必須是的清單中的項目可以新增、 刪除和更新，例如`ICollection<T>`。  您可以指定`ICollection<T>`或型別，例如`List<T>`或`HashSet<T>`。 如果您指定`ICollection<T>`，EF 建立`HashSet<T>`預設集合。

### <a name="the-enrollment-entity"></a>註冊實體

![註冊的實體圖表](intro/_static/enrollment-entity.png)

在*模型*資料夾中，建立*Enrollment.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID`屬性會是主索引鍵，此實體會使用`classnameID`模式而不是`ID`本身做為您在中看到`Student`實體。 通常您會選擇其中一個模式，並在您的資料模型中使用它。 在這裡，變化說明您可以使用其中一個模式。 在[之後的教學課程](inheritance.md)，您會看到如何使用識別碼，而類別名稱不能簡化資料模型中實作繼承。

`Grade`屬性是`enum`。 問號之後`Grade`型別宣告表示`Grade`屬性可為 null。 為 null 的等級是不同於零等級--null 表示等級不未知或尚未被指派。

`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。 `Enrollment`實體都與一個`Student`實體，所以此屬性只可以保存單一`Student`實體 (不同於`Student.Enrollments`導覽屬性之前看到，而可以包含多個`Enrollment`實體)。

`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。 `Enrollment`實體都與一個`Course`實體。

Entity Framework 會解譯為外部索引鍵屬性屬性如果名稱為`<navigation property name><primary key property name>`(例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性也只稱為`<primary key property name>`(例如，`CourseID`因為`Course`實體的主索引鍵是`CourseID`)。

### <a name="the-course-entity"></a>課程實體

![課程實體圖表](intro/_static/course-entity.png)

在*模型*資料夾中，建立*Course.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments`屬性為導覽屬性。 A`Course`可以與任意數目的相關實體`Enrollment`實體。

我們將更多有關`DatabaseGenerated`屬性[之後的教學課程](complex-data-model.md)本系列。 基本上，此屬性可讓您輸入的主索引鍵的課程，而不是需要產生它的資料庫。

## <a name="create-the-database-context"></a>建立的資料庫內容

協調對給定的資料模型的 Entity Framework 功能的主要類別是資料庫內容類別。 您可以透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立此類別。 在程式碼中指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 的行為。 在此專案中，類別會命名為`SchoolContext`。

在專案資料夾中，建立名為資料夾*資料*。

在*資料*資料夾建立新的類別檔案命名為*SchoolContext.cs*，並將範本程式碼取代下列程式碼：

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

此程式碼建立`DbSet`每一個實體集的屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。

您可以省略`DbSet<Enrollment>`和`DbSet<Course>`陳述式，它會運作方式相同。 Entity Framework 會將其包含隱含因為`Student`實體參考`Enrollment`實體和`Enrollment`實體參考`Course`實體。

EF 建立資料庫時，會建立具有相同名稱的資料表`DbSet`屬性名稱。 集合的屬性名稱通常是複數形式 （學生而不是學生），但開發人員不同意有關不論是否 pluralized 資料表名稱。 如需這些教學課程您都將指定的 DbContext 單數資料表名稱來覆寫預設行為。 若要這樣做，請加入下列反白顯示的程式碼的最後一個 DbSet 屬性之後。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-context-with-dependency-injection"></a>暫存器具有相依性插入的內容

實作 ASP.NET Core[相依性插入](../../fundamentals/dependency-injection.md)預設。 在應用程式啟動時的相依性插入會註冊服務 （例如 EF 資料庫內容中）。 需要這些服務 （例如 MVC 控制器） 的元件會提供這些服務，透過建構函式參數。 您會看到的內容執行個體取得稍後在本教學課程的控制器建構函式程式碼。

若要註冊`SchoolContext`為服務時，開啟*Startup.cs*，並加入要反白顯示的行`ConfigureServices`方法。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=3-4)]

連接字串的名稱會傳遞至內容所呼叫的方法上`DbContextOptionsBuilder`物件。 本機開發， [ASP.NET Core 組態系統](../../fundamentals/configuration.md)讀取連接字串從*appsettings.json*檔案。

新增`using`陳述式`ContosoUniversity.Data`和`Microsoft.EntityFrameworkCore`命名空間，然後建置專案。

[!code-csharp[Main](intro/samples/cu/Startup.cs?name=snippet_Usings)]

開啟*appsettings.json*檔案，然後加入連接字串，如下列範例所示。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

連接字串會指定 SQL Server LocalDB 資料庫。 LocalDB 是輕量版 SQL Server Express Database Engine 和適用於應用程式開發，而不是生產環境使用。 視需要啟動 LocalDB，並以使用者模式執行，因此沒有複雜的設定。 根據預設，建立 LocalDB *.mdf*資料庫中的檔案`C:/Users/<user>`目錄。

## <a name="add-code-to-initialize-the-database-with-test-data"></a>加入程式碼以初始化測試資料的資料庫

Entity Framework 會為您建立空的資料庫。  本節中，您可以撰寫以填入測試資料建立資料庫之後呼叫的方法。

在此您將使用`EnsureCreated`方法，以自動建立資料庫。 在[之後的教學課程](migrations.md)您會看到如何使用 Code First 移轉，若要變更資料庫結構描述，而不是卸除並重新建立資料庫處理模型的變更。

在*資料*資料夾中，建立新的類別檔案命名為*DbInitializer.cs*和範本程式碼取代為下列程式碼會使資料庫可在需要時建立負載測試到新的資料資料庫。

[!code-csharp[Main](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

如果在資料庫中，沒有任何的學生，而且如果沒有，則會假設資料庫新，而且必須是測試資料植入，則會檢查程式碼。  它將測試資料載入至陣列而非`List<T>`集合，以最佳化效能。

在*Program.cs*，修改`Main`方法來啟動應用程式上執行以下作業：

* 從相依性插入容器中取得的資料庫內容執行個體。
* 呼叫種子方法，將內容傳遞給它。
* Seed 方法完成時處置內容。

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

新增`using`陳述式：

[!code-csharp[Main](intro/samples/cu/Program.cs?name=snippet_Usings)]

在較舊的教學課程中，您可能會看到類似的程式碼中`Configure`方法中的*Startup.cs*。 我們建議您改用`Configure`方法只是用來設定 要求管線。 應用程式啟動程式碼屬於`Main`方法。

現在首次執行應用程式，資料庫將會建立並植入的測試資料。 每當您變更您的資料模型，您可以刪除資料庫，更新您種子的方法，並重新開始新的資料庫具有相同的方式。 在稍後的教學課程中，您會看到如何修改資料庫時的資料模型變更，而不需要刪除並重新建立它。

## <a name="create-a-controller-and-views"></a>建立控制器和檢視

接下來，您將加入此 MVC 控制器和檢視，將會用來查詢及儲存資料的 EF，Visual Studio 中使用 scaffolding 引擎。

CRUD 動作方法和檢視表的自動建立稱為 scaffolding。 Scaffolding 與程式碼產生的 scaffold 的程式碼是您可以進行修改以符合您自己的需求，而您通常不修改產生的程式碼的起點。 當您需要自訂產生的程式碼時，您使用部分類別，或重新產生程式碼項目變更時。

* 以滑鼠右鍵按一下**控制器**資料夾中的**方案總管 中**選取**新增 > 新的 Scaffold 項目**。

* 在**將 MVC 相依性**對話方塊中，選取**最少的相依性**，然後選取**新增**。

  ![新增相依性](intro/_static/add-depend.png)

  Visual Studio 加入 scaffold 控制器所需的相依性。 專案檔中的唯一變更是新增`Microsoft.VisualStudio.Web.CodeGeneration.Design`封裝。

  A *ScaffoldingReadMe.txt*建立您可以刪除的檔案。

* 同樣地，以滑鼠右鍵按一下**控制器**資料夾中的**方案總管 中**選取**新增 > 新的 Scaffold 項目**。

* 在**新增 Scaffold**對話方塊：

  * 選取**檢視，使用 Entity Framework 的 MVC 控制器**。

  * 按一下 [加入] 。

* 在**加入控制器**對話方塊：

  * 在**模型類別**選取**學生**。

  * 在**資料內容類別**選取**SchoolContext**。

  * 接受預設值**StudentsController**做為名稱。

  * 按一下 [加入] 。

  ![Scaffold Student](intro/_static/scaffold-student.png)

  當您按一下**新增**，Visual Studio scaffolding 引擎會建立*StudentsController.cs*檔案和一組檢視 (*.cshtml*檔案)，所以搭配控制器。

(Scaffolding 引擎也可以建立的資料庫內容，如果您不要手動建立第一次可以如同您稍早在本教學課程。 您可以指定新的內容類別中**加入控制器**方塊右邊的加號**資料內容類別**。  Visual Studio 接著會建立您`DbContext`以及控制器和檢視類別。)

您會發現控制器會採用`SchoolContext`做為建構函式參數。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET 相依性插入會負責傳遞的執行個體的`SchoolContext`到控制器。 您可以設定於*Startup.cs*稍早檔案。

控制器含有`Index`動作方法，會顯示所有學生的資料庫中。 方法會取得一份學生來自學生實體集藉由讀取`Students`的資料庫內容執行個體的屬性：

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

稍後在本教學課程中，您將了解這段程式碼中的非同步程式設計項目。

*Views/Students/Index.cshtml*檢視會顯示在資料表中的這份清單：

[!code-html[](intro/samples/cu/Views/Students/Index1.cshtml)]

按 CTRL + F5 執行專案，或選擇**偵錯 > 啟動但不偵錯**從功能表。

按一下以查看測試資料的學生 索引標籤，`DbInitializer.Initialize`插入的方法。 根據如何窄瀏覽器視窗，您會看到`Student`] 索引標籤頂端的頁面中，或您的連結，則必須按一下 [瀏覽圖示以查看連結右上角。

![Contoso 大學首頁窄](intro/_static/home-page-narrow.png)

![學生索引頁](intro/_static/students-index.png)

## <a name="view-the-database"></a>檢視的資料庫

當您啟動應用程式，`DbInitializer.Initialize`方法呼叫`EnsureCreated`。 EF 看到不時發生的任何資料庫，並因此建立一個，則的其餘部分`Initialize`方法的程式碼填入資料的資料庫。 您可以使用**SQL Server 物件總管**(SSOX) 在 Visual Studio 中檢視的資料庫。

關閉瀏覽器。

如果 SSOX 視窗尚未開啟，請選取 從**檢視**Visual Studio 中的功能表。

在 SSOX，按一下  **(localdb) \MSSQLLocalDB > 資料庫**，然後按一下 資料庫名稱中的連接字串中的項目您*appsettings.json*檔案。

展開**資料表**節點以查看您的資料庫中的資料表。

![SSOX 中的資料表](intro/_static/ssox-tables.png)

以滑鼠右鍵按一下**學生**資料表，並按一下**檢視資料**以查看所建立的資料行和資料列插入資料表。

![學生 SSOX 中的資料表](intro/_static/ssox-student-table.png)

*.Mdf*和*.ldf*資料庫檔案位於*C:\Users\<您的使用者名稱 >*資料夾。

因為您正在撥打`EnsureCreated`在啟動應用程式執行初始設定式方法中，您現在無法進行變更以`Student`類別、 刪除資料庫、 執行一次，應用程式和資料庫會自動重新建立它們以符合您的變更。 例如，如果您加入`EmailAddress`屬性`Student`類別，您看到新`EmailAddress`重新建立資料表中的資料行。

## <a name="conventions"></a>慣例

您必須撰寫 Entity framework 可以為您建立完整的資料庫中的程式碼數量是最小的因為使用的慣例或讓 Entity Framework 的假設。

* 名稱`DbSet`屬性會用做資料表的名稱。 實體沒有參考`DbSet`屬性，實體類別名稱會當做資料表名稱。

* 實體屬性名稱會用於資料行名稱。

* 實體屬性會在名為 ID 或 classnameID 辨識為主索引鍵屬性。

* 屬性會解譯為外部索引鍵屬性上，如果名稱為* <navigation property name> <primary key property name> * (例如，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`). 外部索引鍵屬性也只稱為* <primary key property name> * (例如，`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。

傳統行為可以被覆寫。 例如，您可以明確指定資料表名稱，如稍早在本教學課程中您所見。 您可以設定資料行名稱和主索引鍵或外部索引鍵，以設定任何屬性，您會發現在[之後的教學課程](complex-data-model.md)本系列。

## <a name="asynchronous-code"></a>非同步程式碼

非同步程式設計是預設的 ASP.NET Core 和 EF 核心模式。

Web 伺服器的有限的數目的執行緒可用，而且在高負載情況下的所有可用的執行緒可能正在使用中。 當發生這種情況時，伺服器無法處理新的要求，直到執行緒釋放。 使用同步程式碼，許多執行緒可能會將繫結起來雖然它們實際上並不執行任何工作，因為他們正在等候 I/O 完成。 使用非同步程式碼，當處理程序在等候 I/O 完成，它的執行緒釋放針對伺服器將用於處理其他要求。 如此一來，非同步程式碼可讓更有效率地使用伺服器資源，而且伺服器已啟用以處理更多流量不會造成延遲。

非同步程式碼會導致少量的額外負荷，在執行階段，但效能的衝擊並不顯著，同時針對高流量的情況低流量的情況下，可能的效能提升的很大。

下列程式碼，`async`關鍵字，`Task<T>`傳回值，`await`關鍵字和`ToListAsync`方法提供程式碼以非同步方式執行。

[!code-csharp[Main](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async`關鍵字會指示編譯器以產生組件的方法主體的回呼，並自動建立`Task<IActionResult>`傳回物件。

* 傳回型別`Task<IActionResult>`代表進行中的工作，且結果型別`IActionResult`。

* `await`關鍵字會導致編譯器分成兩個部分的方法。 以非同步方式啟動的作業結束的第一個部分。 第二個部分會放入作業完成時呼叫的回呼方法。

* `ToListAsync`非同步版本`ToList`擴充方法。

要注意當您撰寫非同步程式碼，使用 Entity Framework 的一些事項：

* 只會造成查詢或命令傳送至資料庫的陳述式會以非同步方式執行。 其中包含，例如`ToListAsync`， `SingleOrDefaultAsync`，和`SaveChangesAsync`。  它不包含，例如，陳述式，只要變更`IQueryable`，例如`var students = context.Students.Where(s => s.LastName == "Davolio")`。

* EF 內容不是安全執行緒： 不要嘗試執行多個平行作業。 當您呼叫任何非同步 EF 方法時，一律使用`await`關鍵字。

* 如果您想要充分利用非同步程式碼的效能優點，請確定任何程式庫封裝，您只使用 （例如分頁），也使用非同步呼叫會造成查詢傳送至資料庫的任何 Entity Framework 方法。

如需在.NET 非同步程式設計的詳細資訊，請參閱[非同步概觀](https://docs.microsoft.com/dotnet/articles/standard/async)。

## <a name="summary"></a>總結

現在您已建立簡單的應用程式使用的 Entity Framework Core 和 SQL Server Express LocalDB 來儲存和顯示資料。 在下列的教學課程中，您將學習如何執行基本 CRUD （建立、 讀取、 更新、 刪除） 作業。

>[!div class="step-by-step"]
[下一步](crud.md)  
