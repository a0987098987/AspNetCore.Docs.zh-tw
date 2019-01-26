---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 教學課程：開始使用 Entity Framework 6 Code First 使用 MVC 5 |Microsoft Docs
description: 在此系列教學課程中，您將了解如何建置使用 Entity Framework 6 存取資料的 ASP.NET MVC 5 應用程式。
author: tdykstra
ms.author: riande
ms.date: 01/22/2019
ms.topic: tutorial
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: a00a5a3aa295c2584b90abbf931c258c9e1b58ca
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889947"
---
# <a name="tutorial-get-started-with-entity-framework-6-code-first-using-mvc-5"></a>教學課程：開始使用 Entity Framework 6 Code First 使用 MVC 5

> [!NOTE]
> 對於新的開發，我們建議[ASP.NET Core Razor Pages](/aspnet/core/razor-pages)透過 ASP.NET MVC 控制器和檢視。 類似如下的教學課程系列的使用 Razor 頁面，請參閱[教學課程：開始使用 ASP.NET Core 中的 Razor Pages](/aspnet/core/tutorials/razor-pages/razor-pages-start)。 新的教學課程：
> * 比較容易學習。
> * 提供更多 EF Core 最佳做法。
> * 使用更有效率的查詢。
> * 具有最新的 API。
> * 涵蓋更多功能。
> * 是新應用程式開發的建議方法。

在此系列教學課程中，您將了解如何建置使用 Entity Framework 6 存取資料的 ASP.NET MVC 5 應用程式。 本教學課程使用 Code First 的工作流程。 如需有關如何選擇 Code First、 Database First 或 Model First 的資訊，請參閱[建立模型](/ef/ef6/modeling/)。

本教學課程系列會說明如何建立 Contoso 大學範例應用程式。 範例應用程式是簡單的大學網站。 有了它，您可以檢視和更新學生、 課程和講師資訊。 以下是兩個您所建立的畫面：

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![編輯學生](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 MVC web 應用程式
> * 設定網站樣式
> * 安裝 Entity Framework 6
> * 建立資料模型
> * 建立資料庫內容
> * 初始化含有測試資料的 DB
> * 將 EF 6 設定為使用 LocalDB
> * 建立控制器和檢視
> * 檢視資料庫

## <a name="prerequisites"></a>必要條件

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017)

## <a name="create-an-mvc-web-app"></a>建立 MVC web 應用程式

1. 開啟 Visual Studio 並建立C#web 專案使用**ASP.NET Web 應用程式 (.NET Framework)** 範本。 將專案命名為*ContosoUniversity* ，然後選取**確定**。

   ![在 Visual Studio 中新增專案對話方塊](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-project-dialog.png)

1. 在 **新增 ASP.NET Web 應用程式集 ContosoUniversity**，選取**MVC**。

   ![新 web 應用程式 對話方塊在 Visual Studio 中](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/new-web-app-dialog.png)

    > [!NOTE]
    > 根據預設，**驗證**選項設定為**不需要驗證**。 本教學課程中，web 應用程式不需要使用者登入。 此外，它不會限制存取權依據誰登入。

1. 選取 [確定] 建立專案。

## <a name="set-up-the-site-style"></a>設定網站樣式

一些簡單的變更會設定網站的功能表、配置和首頁。

1. 開啟*Views\Shared\\_Layout.cshtml*，並進行下列變更：

   - 將每個出現的"My ASP.NET Application"和"Application name"變更為"Contoso University"。
   - 新增功能表項目為學生、 課程、 講師和部門，並刪除連絡人項目。

   下列程式碼片段中，會反白顯示所做的變更：

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

2. 在  *Views\Home\Index.cshtml*，檔案的內容取代為下列的程式碼，以使用關於此應用程式的文字取代關於 ASP.NET 和 MVC 的文字：

   [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

3. 按下 Ctrl + F5 來執行網站。 您會看到與主功能表的 [首頁] 頁面。

## <a name="install-entity-framework-6"></a>安裝 Entity Framework 6

1. 從**工具**功能表上，選擇**NuGet 套件管理員**，然後選擇**Package Manager Console**。

2. 在 [ **Package Manager Console** ] 視窗中，輸入下列命令：

   ```text
   Install-Package EntityFramework
   ```

這個步驟是，本教學課程有您手動執行，但是，無法在完成自動由 ASP.NET MVC 樣板功能的幾個步驟。 您的所作所為它們以手動方式，讓您可以看到使用 Entity Framework (EF) 所需的步驟。 您將更新版本使用 scaffolding 建立 MVC 控制器和檢視。 替代方法是讓 scaffolding 自動安裝 EF NuGet 套件、 建立資料庫內容類別，以及建立連接字串。 當您準備好這麼這樣一來時，您只需要為略過這些步驟，並在您建立實體類別之後，建立 MVC 控制器的結構。

## <a name="create-the-data-model"></a>建立資料模型

接下來您會為 Contoso 大學應用程式建立實體類別。 您首先要使用下列三個實體：

**Course** <-> **註冊** <-> **學生**

| 實體 | Relationship |
| -------- | ------------ |
| 課程來註冊 | 一對多 |
| 若要註冊的學生 | 一對多 |

在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。 換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。

在下列章節中，您將建立這些實體的每個類別。

> [!NOTE]
> 如果您嘗試編譯專案，才能完成建立所有這些實體類別時，您會收到編譯器錯誤。

### <a name="the-student-entity"></a>Student 實體

- 在*模型*資料夾中，建立名為的類別檔案*Student.cs*中的資料夾上按一下滑鼠右鍵**方案總管 中**，然後選擇**新增**  > **類別**。 使用下列程式碼取代範本程式碼：

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。 根據預設，Entity Framework 的屬性解譯，稱為`ID`或是*classname* `ID`為主索引鍵。

`Enrollments` 屬性為*導覽屬性*。 導覽屬性會保留與此實體相關的其他實體。 在此情況下，`Enrollments`的屬性`Student`實體將保存的所有`Enrollment`相關的實體`Student`實體。 換句話說，如果給定`Student`資料庫中的資料列有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將包含這兩個`Enrollment`實體。

導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能這類*消極式載入*。 (將會說明消極式載入稍後，在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程。)

若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection`。

### <a name="the-enrollment-entity"></a>Enrollment 實體

- 在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID`屬性會是主索引鍵，此實體會使用*classname* `ID`模式，而非`ID`本身為您在看到`Student`實體。 通常您會選擇一個模式，然後在您整個資料模型中使用此模式。 在這裡，此變化僅作為向您展示使用不同模式之用。 在稍後的教學課程中，您會看到如何使用`ID`而不需要`classname`更輕鬆地在資料模型中實作繼承。

`Grade`屬性是[列舉](/ef/ef6/modeling/code-first/data-types/enums)。 問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](/dotnet/csharp/programming-guide/nullable-types/using-nullable-types)。 為 null 的成績等級是不同於成績為零： null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 `Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

Entity Framework 的屬性解譯為外部索引鍵屬性是否將它命名 *&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (例如`StudentID`for`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性也可以命名相同只要 *&lt;主索引鍵屬性名稱&gt;* (例如`CourseID`因為`Course`實體的主索引鍵是`CourseID`)。

### <a name="the-course-entity"></a>Course 實體

- 在 *模型*資料夾中，建立*Course.cs*，以下列程式碼取代範本程式碼：

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

我們將進一步討論<xref:System.ComponentModel.DataAnnotations.Schema.DatabaseGeneratedAttribute>在稍後的教學課程中，在這一系列的屬性。 基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。

## <a name="create-the-database-context"></a>建立資料庫內容

協調 Entity Framework 功能，為指定的資料模型的主要類別是*資料庫內容*類別。 您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=vs.113).aspx)類別。 在您的程式碼中，您可以指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 行為。 在此專案中，類別命名為 `SchoolContext`。

- ContosoUniversity 專案中建立資料夾，請以滑鼠右鍵按一下專案中的**方案總管**，按一下 **新增**，然後按一下**新資料夾**。 將新資料夾命名*DAL* （適用於資料存取層）。 在該資料夾中，建立新的類別檔案，名為*SchoolContext.cs*，並以下列程式碼取代範本程式碼：

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specify-entity-sets"></a>指定實體集

此程式碼會建立[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.113).aspx)每一個實體集的屬性。 在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，以及*實體*對應至資料表中的資料列。

> [!NOTE]
>
> 您可以省略`DbSet<Enrollment>`和`DbSet<Course>`陳述式和它的運作。 Entity Framework 會隱含它們，因為`Student`odkazy`Enrollment`實體並`Enrollment`實體參考`Course`實體。

### <a name="specify-the-connection-string"></a>指定的連接字串

連接字串 （其中您稍後會加入至 Web.config 檔案） 的名稱會傳遞至建構函式。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

您也可以傳入連接字串而不是儲存在 Web.config 檔案中的其中一個的名稱。 如需使用指定的資料庫選項的詳細資訊，請參閱[連接字串和模型](/ef/ef6/fundamentals/configuring/connection-strings)。

如果您未指定連接字串或其中一個名稱明確，Entity Framework 會假設此連接字串名稱是類別名稱相同。 在此範例中的預設連接字串名稱就得`SchoolContext`，等同於您要明確指定。

### <a name="specify-singular-table-names"></a>指定單數資料表名稱

`modelBuilder.Conventions.Remove`中的陳述式[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會防止資料表名稱的複數化。 如果您沒有這麼做，在資料庫中產生的資料表就會命名為`Students`， `Courses`，和`Enrollments`。 相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。 針對是否要複數化資料表名稱，開發人員並沒有共識。 本教學課程使用的單數形式，但很重要的一點是，您可以選取任何您想要包含或省略這行程式碼的表單。

## <a name="initialize-db-with-test-data"></a>初始化含有測試資料的 DB

Entity Framework 可以自動建立 （或卸除並重新建立） 為您的應用程式執行時的資料庫。 您可以指定每次執行應用程式，或此模型會與現有的資料庫同步處理時，才，應該完成這。 您也可以撰寫`Seed`方法，Entity Framework 會自動呼叫之後才能填入測試資料建立資料庫。

預設行為是建立資料庫，如果它不存在 （且擲回例外狀況，如果模型已變更，而且資料庫已經存在）。 在本節中，您會指定資料庫應該卸除並重新建立每次模型變更時。 卸除資料庫時，會造成您的所有資料遺失。 這通常是好在開發期間，因為`Seed`資料庫重新建立，而且將會重新建立您的測試資料時，會執行方法。 但在生產環境中通常不想遺失所有資料，每當您需要變更資料庫結構描述。 稍後您會看到如何使用 Code First 移轉來變更資料庫結構描述，而不是卸除並重新建立資料庫來處理模型變更。

1. 在 DAL 資料夾中，建立新的類別檔案，名為*SchoolInitializer.cs*會使資料庫建立時所需的下列程式碼取代範本程式碼和測試資料載入至新的資料庫。

   [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

   `Seed`方法會採用資料庫內容物件做為輸入參數，並在方法中的程式碼會使用該物件來將新實體新增至資料庫。 每個實體類型，程式碼會建立新實體的集合，將它們新增至適當`DbSet`屬性，然後按一下 儲存至資料庫的變更。 不需要呼叫`SaveChanges`實體的每一個群組之後的方法，做為在這裡，完成，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。

2. 若要告訴 Entity Framework 使用初始設定式類別，將新增項目`entityFramework`應用程式中的項目*Web.config*檔的檔案 （在根專案資料夾中），如下列範例所示：

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

   `context type`指定完整的內容類別名稱和組件，而`databaseinitializer type`指定初始設定式類別和組件中的完整的名稱。 (當您不想要使用初始設定式的 EF 時，您可以設定屬性`context`項目： `disableDatabaseInitialization="true"`。)如需詳細資訊，請參閱 <<c0> [ 組態檔設定](/ef/ef6/fundamentals/configuring/config-file)。

   設定初始設定式的替代方法*Web.config*檔案是要在程式碼中加上`Database.SetInitializer`陳述式來`Application_Start`中的方法*Global.asax.cs*檔案。 如需詳細資訊，請參閱 <<c0> [ 了解在 Entity Framework Code First 資料庫初始設定式](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)。

應用程式現在已設定，讓您存取資料庫一次執行應用程式的第一次時，Entity Framework 會比較模型資料庫 (您`SchoolContext`和實體類別)。 如果沒有差異，應用程式會卸除並重新建立資料庫。

> [!NOTE]
> 當您部署在生產網頁伺服器應用程式時，您必須移除或停用程式碼，會卸除並重新建立資料庫。 您將在稍後的教學課程中，在這一系列來這麼做。

## <a name="set-up-ef-6-to-use-localdb"></a>將 EF 6 設定為使用 LocalDB

[LocalDB](/sql/database-engine/configure-windows/sql-server-2016-express-localdb?view=sql-server-2017)是輕量版的 SQL Server Express database engine。 輕鬆安裝和設定、 啟動隨選、 並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。 您可以將 LocalDB 資料庫檔案放在*應用程式\_資料*web 專案，如果您想要能夠將複製的資料庫專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用 *.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。 使用 Visual Studio 預設會安裝 LocalDB。

一般而言，SQL Server Express 不用於生產環境 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是設計來搭配 IIS 運作。

- 在本教學課程中，您將使用 LocalDB。 開啟應用程式*Web.config*檔案，並新增`connectionStrings`上述項目`appSettings`項目，如下列範例所示。 (請確定您更新*Web.config*根專案資料夾中的檔案。 另外還有*Web.config*中的檔案*檢視*您不需要更新的子資料夾。)

   [!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

您已新增連接字串會指定使用 Entity Framework 會使用名為 LocalDB 資料庫*ContosoUniversity1.mdf*。 （尚不存在的資料庫，但 EF 會建立它）。如果您想要在其中建立資料庫您*應用程式\_資料*資料夾中，您可以新增`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`的連接字串。 如需有關連接字串的詳細資訊，請參閱 < [ASP.NET Web 應用程式的 SQL Server 連接字串](/previous-versions/aspnet/jj653752(v=vs.110))。

您實際上不需要在連接字串*Web.config*檔案。 如果您未提供的連接字串，Entity Framework 會使用預設連接字串，根據您的內容類別。 如需詳細資訊，請參閱 < [Code First 至新的資料庫](/ef/ef6/modeling/code-first/workflows/new-database)。

## <a name="create-controller-and-views"></a>建立控制器和檢視

現在您將建立顯示資料的網頁。 會自動要求資料的程序就會觸發建立的資料庫。 您一開始先建立新的控制器。 但您這麼做之前，建置專案，以供 MVC 控制器的 scaffolding 的模型和內容的類別。

1. 以滑鼠右鍵按一下**控制站**資料夾中的**方案總管**，選取**新增**，然後按一下**新增 Scaffold 項目**。
2. 在**新增 Scaffold**對話方塊中，選取**使用 MVC 5 控制器與檢視，Entity Framework**，然後選擇**新增**。

     ![在 Visual Studio 中 [新增 Scaffold] 對話方塊](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/add-scaffold.png)

3. 在 [**新增控制器**] 對話方塊中，進行下列選擇，，然後選擇**新增**:

   - 模型類別：**學生 (ContosoUniversity.Models)**。 （如果您沒有看到此選項，在下拉式清單中的，建置專案並再試一次）。
   - 資料內容類別：**SchoolContext (ContosoUniversity.DAL)**。
   - 控制器名稱：**StudentController** (不 StudentsController)。
   - 保留其他欄位的預設值。

     當您按一下 **新增**，建立框架*StudentController.cs*檔案和一組檢視 (*.cshtml*檔案)，可以使用該控制器。 未來當您建立使用 Entity Framework 的專案，您也可以利用的框架的一些額外的功能： 建立第一個模型類別，請勿建立連接字串，然後在 **新增控制器**  方塊中指定 **新的資料內容** 藉由選取 **+** 旁邊 **資料內容類別**。 框架會建立您`DbContext`類別和您的連線字串以及控制器和檢視。
4. Visual Studio 會開啟*Controllers\StudentController.cs*檔案。 您會看到類別變數已經建立的資料庫內容物件具現化：

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index`動作方法會取得一份的學生*學生*實體集，請閱讀`Students`的資料庫內容執行個體的屬性：

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml*檢視以表格顯示這份清單：

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. 按 Ctrl + F5 執行專案。 （如果您收到 「 無法建立陰影複製 」 的錯誤時，關閉瀏覽器並再試一次）。

     按一下 **學生**索引標籤來查看測試資料，`Seed`插入的方法。 根據如何窄瀏覽器視窗是，您會看到的最上層的網址列中的學生 索引標籤連結，或您必須按一下右上角來查看連結。

     ![功能表按鈕](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

## <a name="view-the-database"></a>檢視資料庫

當您執行 Students 頁面並應用程式嘗試存取資料庫時，EF 會發現有任何資料庫，並建立一個。 EF 然後會執行 seed 方法，來填入資料的資料庫。

您可以使用**伺服器總管**或是**SQL Server 物件總管**(SSOX)，以在 Visual Studio 中檢視的資料庫。 本教學課程中，您將使用**伺服器總管**。

1. 關閉瀏覽器。
2. 在**伺服器總管**，展開**資料連接**（您可能需要先選取 [重新整理] 按鈕），展開**學校內容 (ContosoUniversity)**，然後展開**資料表**以查看新的資料庫中的資料表。

3. 以滑鼠右鍵按一下**學生**資料表，然後按一下**顯示資料表資料**以查看所建立的資料行和資料列插入至資料表。

4. 關閉**伺服器總管**連接。

*ContosoUniversity1.mdf*並 *.ldf*資料庫檔案位於 *%USERPROFILE%* 資料夾。

因為您使用`DropCreateDatabaseIfModelChanges`初始設定式，您可以現在進行的變更`Student`類別，請再次執行應用程式，以及資料庫會自動重新建立以符合您的變更。 例如，如果您加入`EmailAddress`屬性，以`Student`類別，請再次執行 Students 頁面以及再查看資料表一次，您將會看到新`EmailAddress`資料行。

## <a name="conventions"></a>慣例

您必須撰寫 Entity framework 能夠為您建立完整的資料庫中的程式碼數量非常小，因為*慣例*，可讓 Entity Framework 的假設。 其中一些已註明，或用來而您知道它們的存在：

- 複數化的形式的實體類別名稱會用於資料表名稱。
- 實體屬性名稱會用於資料行名稱。
- 名為實體屬性`ID`或是*classname* `ID`被視為主索引鍵屬性。
- 是否將它命名，將會解譯為外部索引鍵屬性的屬性 *&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (比方說，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性也可以命名相同只要&lt;主索引鍵屬性名稱&gt;(例如`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。

您已了解可以覆寫慣例。 例如，指定資料表名稱不應該要複數化，，和更新版本，您會看到如何明確地標示為外部索引鍵屬性的屬性。

## <a name="get-the-code"></a>取得程式碼

[下載已完成的專案](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>其他資源

如需有關 EF 6 的詳細資訊，請參閱下列文章：

* [ASP.NET 資料存取 - 建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)

* [程式碼的第一個慣例](/ef/ef6/modeling/code-first/conventions/built-in)

* [建立更複雜的資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 建立 MVC web 應用程式
> * 設定網站樣式
> * 安裝的 Entity Framework 6
> * 建立資料模型
> * 建立資料庫內容
> * 初始化含有測試資料的 DB
> * 將 EF 6 設定為使用 LocalDB
> * 建立的控制器和檢視
> * 檢視資料庫

請前往下一篇文章，以了解如何檢閱和自訂的建立、 讀取、 更新、 刪除 (CRUD) 程式碼，在您的控制器和檢視。
> [!div class="nextstepaction"]
> [實作基本的 CRUD 功能](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)