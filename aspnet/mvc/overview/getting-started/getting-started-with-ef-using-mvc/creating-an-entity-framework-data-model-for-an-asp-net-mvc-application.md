---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 開始使用 Entity Framework 6 Code First 使用 MVC 5 |Microsoft Docs
author: tdykstra
description: 本教學課程系列的較新版本位於： 開始使用 ASP.NET Core 和 Entity Framework Core 使用 Visual Studio 2015。 Contoso Universi 中...
ms.author: aspnetcontent
ms.date: 10/22/2015
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f03ddcf7dcc8b5d20c5459a7fb0015ab20f340c5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837165"
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>透過 MVC 5 開始使用 Entity Framework 6 Code First
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > 本教學課程系列的較新版本位於：[開始使用 ASP.NET Core 和 Entity Framework Core 使用 Visual Studio 2015](https://docs.asp.net/en/latest/data/ef-mvc/intro.html)。
> 
> 
> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 6 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 本教學課程使用 Code First 的工作流程。 如需有關如何選擇 Code First、 Database First 或 Model First 的資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> 這個範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。 本教學課程系列會說明如何建立 Contoso 大學範例應用程式。 您可以[下載完成的應用程式](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)。
> 
> 由 Mike Brind 轉譯的 Visual Basic 版本位於： [EF 6，在 Visual Basic 中的 MVC 5](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting 站台上。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - Entity Framework 6 （EntityFramework 6.1.0 NuGet 套件）
> - [Windows Azure SDK 2.2](https://go.microsoft.com/fwlink/p/?linkid=323510) （選擇性）
>   
> 
> 本教學課程也應該使用[Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)或 Visual Studio 2012。 [VS 2012 版的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511) ，才能使用 Visual Studio 2012 的 Windows Azure 部署。
> 
> 
> ## <a name="tutorial-versions"></a>教學課程的版本
> 
> 本教學課程的先前版本，請參閱[EF 4.1 / MVC 3 電子書](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)並[開始使用 MVC 4 的 EF 5](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)，則[Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。
> 
> 如果您遇到的問題，無法解析時，您通常可以藉由比較您的程式碼已完成的專案，您可以下載中找到問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[常見的錯誤和解決方案或因應措施它們。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso 大學 Web 應用程式

您在這些教學課程中會建置的應用程式為一個簡單的大學網站。

使用者可以檢視和更新學生、課程和教師資訊。 以下是您會建立的幾個畫面。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![編輯學生](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

此網站的 UI 樣式與內建範本產生的相當類似，以使此教學課程能聚焦於如何使用 Entity Framework。

## <a name="prerequisites"></a>必要條件

請參閱**軟體版本**頁面的頂端。 Entity Framework 6 不是必要條件，因為您安裝 EF NuGet 套件做為本教學課程的一部分。

## <a name="create-an-mvc-web-application"></a>建立 MVC Web 應用程式

開啟 Visual Studio 並建立新的 C# Web 專案，名為"ContosoUniversity"。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

在 [**新的 ASP.NET 專案**] 對話方塊中選取**MVC**範本。

如果**在雲端託管**中的核取方塊**Microsoft Azure**區段會被選取，請清除它。

按一下 **變更驗證**。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

在 **變更驗證**對話方塊中，選取**不需要驗證**，然後按一下**確定**。 本教學課程中您不會要求使用者登入或根據登入的使用者限制存取。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

在 [新增 ASP.NET 專案] 對話方塊中，按一下**確定**建立專案。

## <a name="set-up-the-site-style"></a>設定網站樣式

一些簡單的變更會設定網站的功能表、配置和首頁。

開啟*Views\Shared\\_Layout.cshtml*，並進行下列變更：

- 將每個出現的"My ASP.NET Application"和"Application name"變更為"Contoso University"。
- 新增功能表項目為學生、 課程、 講師和部門，並刪除連絡人項目。

所做的變更已醒目提示。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

在  *Views\Home\Index.cshtml*，檔案的內容取代為下列的程式碼，以使用關於此應用程式的文字取代關於 ASP.NET 和 MVC 的文字：

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

按下 CTRL + F5，執行站台。 您會看到與主功能表的 [首頁] 頁面。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>安裝 Entity Framework 6

從**工具**功能表上，按一下**NuGet 套件管理員**，然後按一下  **Package Manager Console**。

在  **Package Manager Console**視窗中輸入下列命令：

`Install-Package EntityFramework`

![安裝 EF](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

下圖顯示 6.0.0 安裝，但 NuGet 會安裝最新版的 Entity Framework （不含發行前版本），這本教學課程以最新的更新是 6.1.1。

這個步驟是，本教學課程有您手動執行，但它無法在完成自動由 ASP.NET MVC 樣板功能的幾個步驟。 您的所作所為它們以手動方式，讓您可以看到使用 Entity Framework 所需的步驟。 您將更新版本使用 scaffolding 建立 MVC 控制器和檢視。 替代方法是讓 scaffolding 自動安裝 EF NuGet 套件、 建立資料庫內容類別，以及建立連接字串。 當您準備好這麼這樣一來時，您只需要為略過這些步驟，並在您建立實體類別之後，建立 MVC 控制器的結構。

## <a name="create-the-data-model"></a>建立資料模型

接下來您會為 Contoso 大學應用程式建立實體類別。 您首先要使用下列三個實體：

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。 換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。

在下節中，您會為這些實體建立各自的類別。

> [!NOTE]
> 如果您嘗試編譯專案，才能完成建立所有這些實體類別時，您會收到編譯器錯誤。


### <a name="the-student-entity"></a>Student 實體

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

在 *模型*資料夾中，建立名為的類別檔案*Student.cs*並以下列程式碼取代範本程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。 根據預設，Entity Framework 的屬性解譯，稱為`ID`或是*classname* `ID`為主索引鍵。

`Enrollments`屬性是*導覽屬性*。 導覽屬性會保留與此實體相關的其他實體。 在此情況下，`Enrollments`的屬性`Student`實體將保存的所有`Enrollment`相關的實體`Student`實體。 換句話說，如果給定`Student`資料庫中的資料列有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將包含這兩個`Enrollment`實體。

導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能這類*消極式載入*。 (將會說明消極式載入稍後，在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程。)

若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection`。

### <a name="the-enrollment-entity"></a>Enrollment 實體

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID`屬性會是主索引鍵，此實體會使用*classname* `ID`模式，而非`ID`本身為您在看到`Student`實體。 通常您會選擇一個模式，然後在您整個資料模型中使用此模式。 在這裡，此變化僅作為向您展示使用不同模式之用。 在稍後的教學課程中，您會看到如何使用`ID`而不需要`classname`更輕鬆地在資料模型中實作繼承。

`Grade`屬性是[列舉](https://msdn.microsoft.com/data/hh859576.aspx)。 問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](https://msdn.microsoft.com/library/2cf62fcy.aspx)。 為 null 的成績等級是不同於成績為零： null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 `Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

Entity Framework 的屬性解譯為外部索引鍵屬性是否將它命名*&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (例如`StudentID`for`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性也可以命名相同只要*&lt;主索引鍵屬性名稱&gt;* (例如`CourseID`因為`Course`實體的主索引鍵是`CourseID`)。

### <a name="the-course-entity"></a>Course 實體

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

在 *模型*資料夾中，建立*Course.cs*，以下列程式碼取代範本程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

我們將進一步討論[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)在稍後的教學課程中，在這一系列的屬性。 基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。

## <a name="create-the-database-context"></a>建立資料庫內容

協調 Entity Framework 功能，為指定的資料模型的主要類別是*資料庫內容*類別。 您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)類別。 在您的程式碼中，您會指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 行為。 在此專案中，類別命名為 `SchoolContext`。

ContosoUniversity 專案中建立資料夾，請以滑鼠右鍵按一下專案中的**方案總管**，按一下 **新增**，然後按一下**新資料夾**。 將新資料夾命名*DAL* （適用於資料存取層）。 在該資料夾中建立新的類別檔案，名為*SchoolContext.cs*，並以下列程式碼取代範本程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>指定實體集

此程式碼會建立[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)每一個實體集的屬性。 在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，以及*實體*對應至資料表中的資料列。

> [!NOTE] 
> 
> 您可以省略`DbSet<Enrollment>`和`DbSet<Course>`陳述式和它的運作。 Entity Framework 會隱含它們，因為 `Student` 實體參考了 `Enrollment` 實體；而 `Enrollment` 實體參考了 `Course` 實體。


### <a name="specifying-the-connection-string"></a>指定連接字串

連接字串 （其中您稍後會加入至 Web.config 檔案） 的名稱會傳遞至建構函式。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

您也可以傳入連接字串而不是儲存在 Web.config 檔案中的其中一個的名稱。 如需使用指定的資料庫選項的詳細資訊，請參閱[Entity Framework-連線和模型](https://msdn.microsoft.com/data/jj592674)。

如果您未指定連接字串或其中一個名稱明確，Entity Framework 會假設此連接字串名稱是類別名稱相同。 在此範例中的預設連接字串名稱就得`SchoolContext`，等同於您要明確指定。

### <a name="specifying-singular-table-names"></a>指定單數資料表名稱

`modelBuilder.Conventions.Remove`中的陳述式[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會防止資料表名稱的複數化。 如果您沒有這麼做，在資料庫中產生的資料表就會命名為`Students`， `Courses`，和`Enrollments`。 相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。 針對是否要複數化資料表名稱，開發人員並沒有共識。 本教學課程使用的單數形式，但很重要的一點是，您可以選取任何您想要包含或省略這行程式碼的表單。

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>初始化具有測試資料的資料庫設定 EF

Entity Framework 可以自動建立 （或卸除並重新建立） 為您的應用程式執行時的資料庫。 您可以指定每次執行應用程式，或此模型會與現有的資料庫同步處理時，才，應該完成這。 您也可以撰寫`Seed`Entity Framework 會自動以填入測試資料建立資料庫之後呼叫的方法。

預設行為是建立資料庫，如果它不存在 （且擲回例外狀況，如果模型已變更，而且資料庫已經存在）。 在本節中，您會指定資料庫應該卸除並重新建立每次模型變更時。 卸除資料庫時，會造成您的所有資料遺失。 這通常是 [確定] 在開發期間，因為`Seed`資料庫重新建立，而且將會重新建立您的測試資料時，會執行方法。 但在生產環境中通常不想遺失所有資料，每當您需要變更資料庫結構描述。 稍後您會看到如何使用 Code First 移轉來變更資料庫結構描述，而不是卸除並重新建立資料庫來處理模型變更。

在 DAL 資料夾中，建立新的類別檔案，名為*SchoolInitializer.cs*並取代範本程式碼  
下列程式碼，使資料庫建立時所需，並將測試資料載入新的資料庫。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed`方法會採用資料庫內容物件做為輸入參數，並在方法中的程式碼會使用該物件來將新實體新增至資料庫。 每個實體類型，程式碼會建立新實體的集合，將它們新增至適當`DbSet`屬性，然後按一下 儲存至資料庫的變更。 不需要呼叫`SaveChanges`實體的每一個群組之後的方法，做為在這裡，完成，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。

若要告訴 Entity Framework 使用初始設定式類別，將新增項目`entityFramework`應用程式中的項目*Web.config*檔的檔案 （在根專案資料夾中），如下列範例所示：

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type`指定完整的內容類別名稱和組件，而`databaseinitializer type`指定初始設定式類別和組件中的完整的名稱。 (當您不想要使用初始設定式的 EF 時，您可以設定屬性`context`項目： `disableDatabaseInitialization="true"`。)如需詳細資訊，請參閱 < [Entity Framework-設定檔設定](https://msdn.microsoft.com/data/jj556606)。

除了設定初始設定式*Web.config*檔案是要在程式碼中加上`Database.SetInitializer`陳述式來`Application_Start`中的方法*Global.asax.cs*檔案。 如需詳細資訊，請參閱 <<c0> [ 了解在 Entity Framework Code First 資料庫初始設定式](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)。

應用程式現在已設定讓該特定執行的第一次存取資料庫時  
應用程式中，Entity Framework 比較的模型資料庫 (您`SchoolContext`和實體類別)。 如果沒有差異，應用程式會卸除並重新建立資料庫。

> [!NOTE]
> 當您部署在生產網頁伺服器應用程式時，您必須移除或停用程式碼，會卸除並重新建立資料庫。 您將在稍後的教學課程中，在這一系列來這麼做。


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>設定為使用 SQL Server Express LocalDB 資料庫的 EF

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是輕量版的 SQL Server Express Database Engine。 輕鬆安裝和設定、 啟動隨選、 並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。 您可以將 LocalDB 資料庫檔案放在*應用程式\_資料*web 專案，如果您想要能夠將複製的資料庫專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用 *.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。 在 Visual Studio 2012 和更新版本中，使用 Visual Studio 的預設會安裝 LocalDB。

SQL Server Express 不是常用的生產 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是在搭配 IIS 運作。

在本教學課程中您將使用 LocalDB。 開啟應用程式*Web.config*檔案，並新增`connectionStrings`上述項目`appSettings`項目，如下列範例所示。 (請確定您更新*Web.config*根專案資料夾中的檔案。 另外還有*Web.config*檔案位於*檢視*您不需要更新的子資料夾。)

如果您使用 Visual Studio 2015，將"v11.0"連接字串中與"MSSQLLocalDB"，因為預設的 SQL Server 執行個體名稱已變更。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

您已新增連接字串會指定使用 Entity Framework 會使用名為 LocalDB 資料庫*ContosoUniversity1.mdf*。 （資料庫還不存在;EF 會建立它。）如果您想要在建立資料庫您*應用程式\_資料*資料夾中，您可以新增`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`的連接字串。 如需有關連接字串的詳細資訊，請參閱 < [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。

您實際上不需要在連接字串*Web.config*檔案。 如果您未提供的連接字串，Entity Framework 會使用一項是依據您的內容類別的預設值。 如需詳細資訊，請參閱 < [Code First 至新的資料庫](https://msdn.microsoft.com/data/jj193542)。

## <a name="creating-a-student-controller-and-views"></a>建立學生控制器和檢視

現在您將建立網頁上顯示資料，並要求資料的程序會自動觸發  
建立資料庫。 您一開始先建立新的控制器。 但您這麼做之前，建置專案，以供 MVC 控制器的 scaffolding 的模型和內容的類別。

1. 以滑鼠右鍵按一下**控制站**資料夾中的**方案總管**，選取**新增**，然後按一下**新增 Scaffold 項目**。
2. 在 **新增 Scaffold**對話方塊中，選取**使用 MVC 5 控制器與檢視，Entity Framework**。

     ![新增 Scaffold](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
3. 在 新增控制器 對話方塊中，進行下列選擇，然後按一下 **新增**:

   - 模型類別：**學生 (ContosoUniversity.Models)**。 （如果您沒有看到此選項，在下拉式清單中的，建置專案並再試一次）。
   - 資料內容類別： **SchoolContext (ContosoUniversity.DAL)**。
   - 控制器名稱： **StudentController** (不 StudentsController)。
   - 保留其他欄位的預設值。

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

     當您按一下 **新增**，框架建立 StudentController.cs 檔案和一組可以使用該控制器的檢視 （.cshtml 檔案）。 在未來，當您建立使用 Entity Framework 的專案中您也可以利用的一些額外的功能的框架： 只要建立您的第一個模型類別，請勿建立連接字串，然後在**新增控制器**  方塊中指定新的內容類別。 框架會建立您`DbContext`類別和您的連線字串以及控制器和檢視。
4. Visual Studio 會開啟*Controllers\StudentController.cs*檔案。 您會看到已建立的資料庫內容物件具現化類別變數：

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

     `Index`動作方法會取得一份的學生*學生*實體集，請閱讀`Students`的資料庫內容執行個體的屬性：

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

     *Student\Index.cshtml*檢視以表格顯示這份清單：

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
5. 按 CTRL+F5 執行專案。 （如果您收到 「 無法建立陰影複製 」 的錯誤時，關閉瀏覽器並再試一次）。

     按一下 **學生**索引標籤來查看測試資料，`Seed`插入的方法。 根據如何窄瀏覽器視窗是，您會看到的最上層的網址列中的學生 索引標籤連結，或您必須按一下右上角來查看連結。

     ![功能表按鈕](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

     ![學生 [索引] 頁面](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>檢視資料庫

當您執行 Students 頁面並應用程式嘗試存取資料庫時，EF 會看到沒有任何資料庫，並讓它建立一個，然後執行 seed 方法，來填入資料的資料庫。

您可以使用**伺服器總管**或是**SQL Server 物件總管**(SSOX)，以在 Visual Studio 中檢視的資料庫。 您將使用本教學課程**伺服器總管**。 (在 Visual Studio Express 版本早於 2013 年**伺服器總管**稱為**資料庫總管**。)

1. 關閉瀏覽器。
2. 中**伺服器總管**，展開**資料連接**，展開**學校內容 (ContosoUniversity)**，然後展開**資料表**查看您新的資料庫中的資料表。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. 以滑鼠右鍵按一下**學生**資料表，然後按一下**顯示資料表資料**以查看所建立的資料行和資料列插入至資料表。

    ![Student 資料表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. 關閉**伺服器總管**連接。

*ContosoUniversity1.mdf*並 *.ldf*資料庫檔案位於`C:\Users\<yourusername>`資料夾。

因為您使用`DropCreateDatabaseIfModelChanges`初始設定式，您可以現在進行的變更`Student`類別，請再次執行應用程式，以及資料庫會自動重新建立以符合您的變更。 例如，如果您加入`EmailAddress`屬性，以`Student`類別，請再次執行 Students 頁面以及再查看資料表一次，您將會看到新`EmailAddress`資料行。

## <a name="conventions"></a>慣例

您必須讓 Entity Framework 能夠為您建立完整的資料庫撰寫的程式碼數量非常小，因為使用了*慣例*，可讓 Entity Framework 的假設。 其中一些已註明，或用來而您知道它們的存在：

- 複數化的形式的實體類別名稱會用於資料表名稱。
- 實體屬性名稱會用於資料行名稱。
- 名為實體屬性`ID`或是*classname* `ID`被視為主索引鍵屬性。
- 是否將它命名，將會解譯為外部索引鍵屬性的屬性*&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (比方說，`StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性也可以命名相同只要&lt;主索引鍵屬性名稱&gt;(例如`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。

您已了解可以覆寫慣例。 例如，指定資料表名稱不應該要複數化，，和更新版本，您會看到如何明確地標示為外部索引鍵屬性的屬性。 您將進一步了解慣例和如何覆寫其[建立更多複雜的資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)稍後在本系列教學課程。 如需慣例的詳細資訊，請參閱 <<c0> [ 程式碼的第一個慣例](https://msdn.microsoft.com/data/jj679962)。

## <a name="summary"></a>總結

您現在已建立簡單的應用程式使用 Entity Framework 和 SQL Server Express LocalDB 來儲存和顯示資料。 在下列教學課程中，您將學習如何執行基本的 CRUD （建立、 讀取、 更新、 刪除） 作業。

您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [下一步](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
