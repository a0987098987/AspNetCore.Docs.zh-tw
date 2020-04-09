---
title: 教學:在 ASP.NET MVC Web 應用中開始使用 EF Core
description: 這是說明如何從零開始建立 Contoso 大學範例應用程式教學課程系列中的第一頁。
author: rick-anderson
ms.author: riande
ms.custom: mvc
ms.date: 02/06/2019
ms.topic: tutorial
uid: data/ef-mvc/intro
ms.openlocfilehash: fca9fdc425506ec8b4eec5c609237208f4c0d7b5
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79511297"
---
# <a name="tutorial-get-started-with-ef-core-in-an-aspnet-mvc-web-app"></a>教學:在 ASP.NET MVC Web 應用中開始使用 EF Core

此教學課程**尚未**升級至 ASP.NET Core 3.0。 [Razor Pages 版本](xref:data/ef-rp/intro)已更新。 ASP.NET酷3.0和本教程的更高版本的大部分代碼更改:

* 位於*Startup.cs*和*Program.cs*檔中。
* 可在[剃刀頁版本](xref:data/ef-rp/intro)中找到。 

如需何時可能更新此資訊的詳細資訊，請參閱[此 GitHub 問題](https://github.com/dotnet/AspNetCore.Docs/issues/13920) \(英文\)。

[!INCLUDE [RP better than MVC](~/includes/RP-EF/rp-over-mvc.md)]

Contoso 大學範例 Web 應用程式示範如何使用 Entity Framework (EF) Core 2.2 和 Visual Studio 2017 或 2019 建立 ASP.NET Core 2.2 MVC Web 應用程式。

這個範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。 這是說明如何從零開始建立 Contoso 大學範例應用程式教學課程系列中的第一頁。

在本教學課程中，您：

> [!div class="checklist"]
> * 建立 ASP.NET Core MVC Web 應用程式
> * 設定網站樣式
> * 了解 EF Core NuGet 套件
> * 建立資料模型
> * 建立資料庫內容
> * 為內容註冊相依性插入
> * 使用測試資料將資料庫初始化
> * 建立控制器和檢視
> * 檢視資料庫

## <a name="prerequisites"></a>Prerequisites

* [.NET Core SDK 2.2](https://dotnet.microsoft.com/download)
* 包含下列工作負載的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)：
  * **ASP.NET 與網頁程式開發**工作負載
  * **.NET Core 跨平台開發**工作負載

## <a name="troubleshooting"></a>疑難排解

如果您執行您不能解決問題，您可以藉由比較您的程式碼通常找到方案[已完成的專案](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)。 如需常見的錯誤以及如何解決這些問題的清單，請參閱[ 數列中的最後一個教學課程疑難排解 > 一節](advanced.md#common-errors)。 如果您找不到您需要那里，您可以張貼問題的 StackOverflow.com [ASP.NET Core](https://stackoverflow.com/questions/tagged/asp.net-core) 或 [EF Core](https://stackoverflow.com/questions/tagged/entity-framework-core)。

> [!TIP]
> 這是 10 個教學的系列課程，當中的每一個課程都是建置於先前教學課程的成果上。 成功完成每一個教學課程後，請儲存專案的複本。 如果您遇到問題，您可以從上一個教學課程來重新開始，而不需從系列的一開始從頭來過。

## <a name="contoso-university-web-app"></a>Contoso 大學 Web 應用程式

您在這些教學課程中會建置的應用程式為一個簡單的大學網站。

使用者可以檢視和更新學生、課程和教師資訊。 以下是您會建立的幾個畫面。

![Students [索引] 頁面](intro/_static/students-index.png)

![Students [編輯] 頁面](intro/_static/student-edit.png)

## <a name="create-web-app"></a>建立 Web 應用程式

* 開啟 Visual Studio。

* 從 [檔案]**** 功能表選取[新增] > [專案] ****。

* 從左側窗格中，選取 [已安裝] > [Visual C#] > [Web]****。

* 選取 [ASP.NET Core Web 應用程式]**** 專案範本。

* 輸入 **ContosoUniversity** 作為名稱，然後按一下 [確定]****。

  ![[新增專案] 對話方塊](intro/_static/new-project2.png)

* 等候 [新增 ASP.NET Core Web 應用程式]**** 對話方塊出現。

* 選取 [.NET Core]****、[ASP.NET Core 2.2]**** 和 [Web 應用程式 (Model-View-Controller)]**** 範本。

* 確保**身份驗證**設置為 **「無身份驗證**」。

* 選取 [確定]****

  ![[新增 ASP.NET Core 專案] 對話方塊](intro/_static/new-aspnet2.png)

## <a name="set-up-the-site-style"></a>設定網站樣式

一些簡單的變更會設定網站的功能表、配置和首頁。

開啟 *Views/Shared/_Layout.cshtml* 並進行下列變更：

* 將每個出現的 "ContosoUniversity" 都變更為 "Contoso University"。 共有三個發生次數。

* 為 **About**、**Students**、**Courses**、**Instructors** 及 **Departments** 新增功能表項目，並刪除 **Privacy** 功能表項目。

所做的變更已醒目提示。

[!code-cshtml[](intro/samples/cu/Views/Shared/_Layout.cshtml?highlight=6,34-48,63)]

在 *Views/Home/Index.cshtml* 中用下列程式碼取代檔案內容，以使用關於此應用程式的文字來取代關於 ASP.NET 和 MVC 的文字：

[!code-cshtml[](intro/samples/cu/Views/Home/Index.cshtml)]

按 CTRL+F5 來執行專案，或從功能表選擇 [偵錯] > [啟動但不偵錯]****。 您會看到在這些教學課程中，您將建立之頁面的索引標籤和首頁。

![Contoso 大學首頁](intro/_static/home-page.png)

## <a name="about-ef-core-nuget-packages"></a>關於 EF Core NuGet 套件

若要將 EF Core 支援新增至專案，請安裝您欲使用之資料庫的提供者。 本教學課程使用 SQL Server，其提供者套件為 [Microsoft.EntityFrameworkCore.SqlServer](https://www.nuget.org/packages/Microsoft.EntityFrameworkCore.SqlServer/)。 此套件包含在 [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) 中，因此您不需要參考該套件。

EF SQL Server 套件及其相依性 (`Microsoft.EntityFrameworkCore` 及 `Microsoft.EntityFrameworkCore.Relational`) 提供了 EF 的執行階段支援。 您會在稍後的[移轉](migrations.md)教學課程中新增工具套件。

如需其他 Entity Framework Core 可用之資料庫提供者的資訊，請參閱[資料庫提供者](/ef/core/providers/)。

## <a name="create-the-data-model"></a>建立資料模型

接下來您會為 Contoso 大學應用程式建立實體類別。 您會從下列三個實體開始。

![Course-Enrollment-Student 資料模型圖表](intro/_static/data-model-diagram.png)

在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。 換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。

在下節中，您會為這些實體建立各自的類別。

### <a name="the-student-entity"></a>Student 實體

![Student 實體圖表](intro/_static/student-entity.png)

在 *Models* 資料夾中，建立一個名為 *Student.cs* 的類別檔案，然後使用下列程式碼取代範本程式碼。

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_Intro)]

`ID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。 Entity Framework 預設會將名為 `ID` 或 `classnameID` 的屬性解譯為主索引鍵。

`Enrollments` 屬性為[導覽屬性](/ef/core/modeling/relationships)。 導覽屬性會保留與此實體相關的其他實體。 在這個案例中，`Student entity` 的 `Enrollments` 屬性會保有與該 `Student` 實體相關的所有 `Enrollment` 實體。 換句話說，若資料庫中給定的學生資料列有兩個相關的 Enrollment 資料列 (包含該學生於其 StudentID 外部索引鍵資料行中主索引鍵值的資料列)，該 `Student` 實體的 `Enrollments` 導覽屬性便會包含這兩個 `Enrollment` 實體。

若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection<T>`。 您可以指定 `ICollection<T>` 或如 `List<T>` 或 `HashSet<T>` 等類型。 若您指定了 `ICollection<T>`，EF 會根據預設建立一個 `HashSet<T>` 集合。

### <a name="the-enrollment-entity"></a>Enrollment 實體

![Enrollment 實體圖表](intro/_static/enrollment-entity.png)

在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：

[!code-csharp[](intro/samples/cu/Models/Enrollment.cs?name=snippet_Intro)]

`EnrollmentID` 屬性將為主索引鍵。此實體會使用 `classnameID` 模式，而非您在 `Student` 實體中所見到的自身 `ID`。 通常您會選擇一個模式，然後在您整個資料模型中使用此模式。 在這裡，此變化僅作為向您展示使用不同模式之用。 在[稍後的教學課程](inheritance.md)中，您會了解使用沒有 classname 的識別碼可讓在資料模型中實作繼承變得更為簡單。

`Grade` 屬性為一個 `enum`。 問號之後的 `Grade` 類型宣告表示 `Grade` 屬性可為 Null。 為 Null 的成績不同於成績為零：Null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 `Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

Entity Framework 會將名為 `<navigation property name><primary key property name>` 的屬性解譯為外部索引鍵屬性 (例如 `Student` 導覽屬性的 `StudentID`，因為 `Student` 實體的主索引鍵為 `ID`)。 外部索引鍵屬性也可以簡單的命名為 `<primary key property name>` (例如 `CourseID`，因為 `Course` 實體的主索引鍵為 `CourseID`)。

### <a name="the-course-entity"></a>Course 實體

![Course 實體圖表](intro/_static/course-entity.png)

在 *Models* 資料夾中，建立 *Course.cs*，然後使用下列程式碼取代現有的程式碼：

[!code-csharp[](intro/samples/cu/Models/Course.cs?name=snippet_Intro)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

我們會在此系列[稍後的教學課程](complex-data-model.md)中進一步討論 `DatabaseGenerated` 屬性。 基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。

## <a name="create-the-database-context"></a>建立資料庫內容

為指定資料模型協調 Entity Framework 功能的主要類別便是資料庫內容類別。 若要建立此類別，您可以從 `Microsoft.EntityFrameworkCore.DbContext` 類別來衍生。 在您的程式碼中，您會指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 行為。 在此專案中，類別命名為 `SchoolContext`。

在專案資料夾中，建立名為 *Data* 的資料夾。

在 *Data* 資料夾中，建立名為 *SchoolContext.cs* 的新類別檔案，然後使用下列程式碼取代範本程式碼：

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_Intro)]

程式碼會為每一個實體集建立 `DbSet` 屬性。 在 Entity Framework 詞彙中，實體集通常會對應至資料庫資料表，而實體則對應至資料表中的資料列。

您可以省略 `DbSet<Enrollment>` 及 `DbSet<Course>` 陳述式，其結果也會是相同的。 Entity Framework 會隱含它們，因為 `Student` 實體參考了 `Enrollment` 實體；而 `Enrollment` 實體參考了 `Course` 實體。

資料庫建立時，EF 會建立和 `DbSet` 屬性名稱相同的資料表。 集合的屬性名稱通常都是複數 (Students 而非 Student)，但許多開發人員會為了資料表名稱究竟是否該是複數型態而爭論。 在此系列教學課程中，您會藉由指定 DbContext 中的單數資料表名稱來覆寫預設行為。 若要完成這項操作，請在最後一個 DbSet 屬性後方新增下列醒目提示程式碼。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_TableNames&highlight=16-21)]

## <a name="register-the-schoolcontext"></a>註冊 SchoolContext

根據預設，ASP.NET Core 會實作[相依性插入](../../fundamentals/dependency-injection.md)。 服務 (例如 EF 資料庫內容) 是在應用程式啟動期間使用相依性插入來註冊。 接著，會透過建構函式參數，針對需要這些服務的元件 (例如 MVC 控制器) 來提供服務。 您會在此教學課程的稍後看到取得內容執行個體的控制器建構函式。

若要將 `SchoolContext` 註冊為服務，請開啟 *Startup.cs*，並將醒目標示的程式碼新增至 `ConfigureServices` 方法。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_SchoolContext&highlight=9-10)]

連接字串的名稱，會透過呼叫 `DbContextOptionsBuilder` 物件上的方法來傳遞至內容。 作為本機開發之用，[ASP.NET Core configuration system](xref:fundamentals/configuration/index) 會從 *appsettings.json* 檔案讀取連接字串。

為 `ContosoUniversity.Data` 和 `Microsoft.EntityFrameworkCore` 命名空間新增 `using` 陳述式，然後建置專案。

[!code-csharp[](intro/samples/cu/Startup.cs?name=snippet_Usings)]

開啟 *appsettings.json* 檔案，然後如以下範例所示新增連接字串。

[!code-json[](./intro/samples/cu/appsettings1.json?highlight=2-4)]

### <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

連接字串會指定 SQL Server LocalDB 資料庫。 LocalDB 是輕量版的 SQL Server Express Database Engine，旨在用於應用程式開發，而不是生產用途。 LocalDB 會依需求啟動，並以使用者模式執行，因此沒有複雜的組態。 LocalDB 預設會在 `C:/Users/<user>` 目錄中建立 *.mdf* 資料庫檔案。

## <a name="initialize-db-with-test-data"></a>使用測試資料將 DB 初始化

Entity Framework 會為您建立空白資料庫。 在本節中，您會撰寫一個方法，該方法會在資料庫建立之後呼叫，以將測試資料填入資料庫。

在此您將使用 `EnsureCreated` 方法來自動建立資料庫。 在[稍後的教學課程](migrations.md)中，您將會了解到如何使用 Code First 移轉變更資料庫結構描述，而非卸除並重新建立資料庫，來處理模型的變更。

在 *Data* 資料夾中，建立一個名為 *DbInitializer.cs* 的新類別檔案，使用下列程式碼取代範本程式碼。這些程式碼會在需要的時候建立資料庫，並將測試資料載入至新的資料庫。

[!code-csharp[](intro/samples/cu/Data/DbInitializer.cs?name=snippet_Intro)]

程式碼會檢查資料庫中是否有任何學生。若沒有的話，它便會假設資料庫是新的資料庫，因此需要植入測試資料。 會將測試資料載入至陣列而非 `List<T>` 集合，以最佳化效能。

在 *Program.cs* 中，修改 `Main` 方法來在應用程式啟動期間執行下列動作：

* 從相依性插入容器中取得資料庫內容執行個體。
* 呼叫種子方法，並將其傳遞給內容。
* 種子方法完成時處理內容。

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Seed&highlight=3-20)]

新增 `using` 陳述式：

[!code-csharp[](intro/samples/cu/Program.cs?name=snippet_Usings)]

在較舊的教學課程中，您會在 *Startup.cs* 的 `Configure` 方法中看到類似的程式碼。 我們建議您只在設定要求管道時使用 `Configure` 方法。 應用程式啟動程式碼屬於 `Main` 方法。

現在在您首次執行應用程式時，資料庫便會建立並植入測試資料。 每當您變更您的資料模型時，您可以刪除資料庫、更新您的種子方法，然後依照相同的方法取得新的資料庫以重新開始。 在稍後的教學課程中，您會了解如何在資料模型變更時修改資料庫，而不需要刪除和重新建立它。

## <a name="create-controller-and-views"></a>建立控制器和檢視

接下來，您會使用 Visual Studio 中的 scaffolding 引擎來新增使用 EF 查詢和儲存資料的 MVC 控制器及檢視。

自動建立 CRUD 動作方法和檢視稱為 Scaffolding。 Scaffolding 與產生程式碼不同。Scaffold 程式碼是一個開始點，使得您可以修改它以符合您的需求，然而您通常不會去修改產生的程式碼。 當您需要自訂產生的程式碼時，您會使用部分類別，或者您會在事務變更時重新產生程式碼。

* 在方案總管**** 中的 **Controllers** 資料夾上以滑鼠右鍵按一下，然後選取 [新增] > [新增 Scaffold 項目]****。

* 在 [新增 Scaffold]**** 對話方塊中：

  * 選取 [使用 Entity Framework 執行檢視的 MVC 控制器]****。

  * 按一下 **[新增]**。 [新增使用 Entity Framework 執行檢視的 MVC 控制器]**** 對話方塊隨即出現。

    ![Scaffold Student](intro/_static/scaffold-student2.png)

  * 在 [模型類別]**** 中，選取 [Student]****。

  * 在 [資料內容類別]**** 中，選取 [SchoolContext]****。

  * 接受預設的 **StudentsController** 作為名稱。

  * 按一下 **[新增]**。

  當您按一下 [新增]**** 時，Visual Studio Scaffolding 引擎便會建立 *StudentsController.cs* 檔案及一組可以使用該控制器的檢視 (*.cshtml* 檔案)。

(Scaffolding 引擎也可以在您沒有如本教學課程先前的操作一樣先手動建立時，為您建立資料庫內容。 您可以在 [新增控制器]**** 方塊中藉由按一下 [資料內容類別]**** 右側的加號來指定新的內容類別。  Visual Studio 接著便會建立您的 `DbContext` 類別及控制器和檢視。)

您會發現控制器會接受 `SchoolContext` 作為建構函式的參數。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_Context&highlight=5,7,9)]

ASP.NET Core 相依性插入會負責傳遞 `SchoolContext` 的執行個體給控制器。 您可以在先前的 *Startup.cs* 檔案中設定它。

控制器含有一個 `Index` 動作方法，該方法會顯示資料庫中的所有學生。 方法會藉由讀取資料庫內容執行個體的 `Students` 屬性，來從 Students 實體集中取得學生的清單：

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex&highlight=3)]

您會在教學課程的稍後學習到此程式碼中的非同步程式設計項目。

*Views/Students/Index.cshtml* 檢視會在一個資料表中顯示這份清單：

[!code-cshtml[](intro/samples/cu/Views/Students/Index1.cshtml)]

按 CTRL+F5 來執行專案，或從功能表選擇 [偵錯] > [啟動但不偵錯]****。

按一下 [Students] 索引標籤來查看 `DbInitializer.Initialize` 方法插入的測試資料。 取決於您瀏覽器視窗的寬度，您可能會在頁面的頂端看到 `Students` 索引標籤連結，或是按一下位於右上角的導覽圖示來查看連結。

![Contoso 大學首頁 (窄)](intro/_static/home-page-narrow.png)

![Students [索引] 頁面](intro/_static/students-index.png)

## <a name="view-the-database"></a>檢視資料庫

當您啟動應用程式時，`DbInitializer.Initialize` 方法會呼叫 `EnsureCreated`。 EF 看到不存在任何資料庫，於是便建立了一個資料庫，接著 `Initialize` 方法程式碼的剩餘部分便會將資料填入資料庫。 您可以使用 [SQL Server 物件總管**** (SSOX) 來在 Visual Studio 中檢視資料庫。

關閉瀏覽器。

若 SSOX 視窗尚未開啟，請從 Visual Studio 中的 [檢視]**** 功能表選取它。

在 SSOX 中，按一下 **(localdb)\MSSQLLocalDB > Databases**，然後按一下位於您 *appsettings.json* 檔案中連接字串內資料庫名稱的項目。

展開 [資料表]**** 節點以查看您資料庫中的資料表。

![SSOX 中的資料表](intro/_static/ssox-tables.png)

以滑鼠右鍵按一下 **Students** 資料表，並按一下 [檢視資料]**** 查看建立的資料行及插入資料表中的資料列。

![SSOX 中的 Student 資料表](intro/_static/ssox-student-table.png)

*.mdf*和 *.ldf*資料庫檔案位於*C:\\\\<使用者>* 資料夾中。

因為您在應用程式啟動時執行的初始設定式方法中呼叫了 `EnsureCreated`，您現在可以對 `Student` 類別進行變更、刪除資料庫、重新執行應用程式，資料庫會自動重新建立以符合您所作出的變更。 例如，若您將一個 `EmailAddress` 屬性新增到 `Student` 類別，您便會在重新建立的資料表中看到新的 `EmailAddress` 資料行。

## <a name="conventions"></a>慣例

為了讓 Entity Framework 能夠建立一個完整資料庫，您所需要撰寫的程式碼非常少，多虧了慣例的使用及 Entity Framework 所做出的假設。

* `DbSet` 屬性的名稱會用於資料表名稱。 針對 `DbSet` 屬性並未參考的實體，實體類別名稱會用於資料表名稱。

* 實體屬性名稱會用於資料行名稱。

* 命名為 ID 或 classnameID 的實體屬性，會辨識為主索引鍵屬性。

* 如果屬性的名稱為*\<導航屬性\<名稱>主键属性名称>(*`Student`例如,`StudentID`對於`Student`導航屬性,因為實體的主鍵為`ID`)則屬性將解釋為外鍵屬性。 外鍵屬性也可以只`EnrollmentID``Enrollment``EnrollmentID`*\<命名主鍵屬性名稱>(* 例如,因為實體的主鍵是 )。

慣例行為可以被覆寫。 例如，您可以明確指定資料表名稱，如稍早在本教學課程中您所見到的。 您可以設定資料行名稱以及將任何屬性設為主索引鍵或外部索引鍵，如同您在本系列[稍後的教學課程](complex-data-model.md)中所見。

## <a name="asynchronous-code"></a>非同步程式碼

非同步程式設計是預設的 ASP.NET Core 和 EF Core 模式。

網頁伺服器的可用執行緒數量有限，而且在高負載情況下，可能會使用所有可用的執行緒。 發生此情況時，伺服器將無法處理新的要求，直到執行緒空出來。 使用同步程式碼，許多執行緒可能在實際上並未執行任何工作時受到占用，原因是在等候 I/O 完成。 使用非同步程式碼，處理程序在等候 I/O 完成時，其執行緒將會空出來以讓伺服器處理其他要求。 因此，非同步程式碼可讓伺服器資源更有效率地使用，而且伺服器可處理更多流量而不會造成延遲。

非同步程式碼雖然的確會在執行階段造成少量的負荷，但在低流量情況下，對效能的衝擊非常微小；在高流量情況下，潛在的效能改善則相當大。

在下列程式碼中，`async` 關鍵字、`Task<T>` 傳回值、`await` 關鍵字和 `ToListAsync` 方法使程式碼以非同步方式執行。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_ScaffoldedIndex)]

* `async` 關鍵字會告訴編譯器為方法本體的一部分產生回呼，並自動建立傳回的 `Task<IActionResult>` 物件。

* 傳回類型 `Task<IActionResult>` 代表了正在進行的工作，其結果為 `IActionResult` 類型。

* `await` 關鍵字會使編譯器將方法分割為兩部分。 第一個部分會與非同步啟動的作業一同結束。 第二個部分則會放入作業完成時呼叫的回呼方法中。

* `ToListAsync` 是 `ToList` 擴充方法的非同步版本。

當您在撰寫使用 Entity Framework 的非同步程式碼時，請注意下列事項：

* 只有讓查詢或命令傳送至資料庫的陳述式，會以非同步方式執行。 其中包含，例如 `ToListAsync`、`SingleOrDefaultAsync`，以及 `SaveChangesAsync`。 其中不包含，例如：僅變更 `IQueryable` 的陳述式，例如 `var students = context.Students.Where(s => s.LastName == "Davolio")`。

* EF 內容在執行緒中並不安全：不要嘗試執行多個平行作業。 當您呼叫任何 async EF 方法時，請一律使用 `await` 關鍵字。

* 若您想要充分利用非同步程式碼帶來的效能優點，請確保任何您正在使用的程式庫 (例如分頁) 也使用了非同步 (若它們有呼叫任何可能會傳送查詢到資料庫的 Entity Framework 方法的話)。

如需在 .NET 中非同步程式設計的詳細資訊，請參閱[非同步總覽](/dotnet/articles/standard/async)。

## <a name="get-the-code"></a>取得程式碼

[下載或檢視已完成的應用程式。](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/data/ef-mvc/intro/samples/cu-final)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您：

> [!div class="checklist"]
> * 建立 ASP.NET Core MVC Web 應用程式
> * 設定網站樣式
> * 了解 EF Core NuGet 套件
> * 建立資料模型
> * 建立資料庫內容
> * 註冊 SchoolContext
> * 使用測試資料將 DB 初始化
> * 建立控制器和檢視
> * 檢視資料庫

在接下來的教學課程中，您將學習到如何執行基本的 CRUD (建立、讀取、更新、刪除) 作業。

若要了解如何執行基本的 CRUD (建立、讀取、更新、刪除) 作業，請前往下一個教學課程。

> [!div class="nextstepaction"]
> [實作基本的 CRUD 功能](crud.md)

