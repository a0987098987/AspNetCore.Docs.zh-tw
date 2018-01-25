---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "開始使用 Entity Framework 6 Code First 使用 MVC 5 |Microsoft 文件"
author: tdykstra
description: "此教學課程系列的更新的版本： 開始使用 ASP.NET Core 和使用 Visual Studio 2015 的 Entity Framework Core。 Contoso Universi..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/22/2015
ms.topic: article
ms.assetid: 00bc8b51-32ed-4fd3-9745-be4c2a9c1eaf
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 46f53279e2e6daa4266c06feb4ba544e14b68a03
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-6-code-first-using-mvc-5"></a>透過 MVC 5 開始使用 Entity Framework 6 Code First
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> > [!NOTE] 
> > 
> > 此教學課程系列的更新的版本：[開始使用 ASP.NET Core 和使用 Visual Studio 2015 的 Entity Framework Core](https://docs.asp.net/en/latest/data/ef-mvc/intro.html)。
> 
> 
> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 本教學課程會使用第一個程式碼的工作流程。 如需如何選擇第一個程式碼、 Database First 或 Model First 資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> 範例應用程式是針對虛構的 Contoso 大學的網站。 其中包括功能，例如許可學生、 課程建立和講師指派。 此教學課程說明如何建置 Contoso 大學範例應用程式。 您可以[下載完成的應用程式](https://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)。
> 
> 轉譯的 Mike Brind 有 Visual Basic 版本： [EF 6，在 Visual Basic 中的 MVC 5](http://www.mikesdotnetting.com/Article/241/MVC-5-with-EF-6-in-Visual-Basic-Creating-an-Entity-Framework-Data-Model) Mikesdotnetting 站台上。
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
> 本教學課程也應搭配[Visual Studio 2013 Express for Web](https://www.microsoft.com/visualstudio/eng/2013-downloads#d-2013-express)或 Visual Studio 2012。 [VS 2012 版本的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323511)無須使用 Visual Studio 2012 的 Windows Azure 部署。
> 
> 
> ## <a name="tutorial-versions"></a>教學課程版本
> 
> 針對本教學課程的先前版本，請參閱[EF 4.1 / MVC 3 電子書](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)和[入門使用 MVC 4 的 EF 5](../../older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)、 [Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。
> 
> 如果您執行您不能解決問題，您通常可以藉由比較您的程式碼已完成的專案，您可以下載會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[常見的錯誤，和它們的因應措施或解決方案。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


## <a name="the-contoso-university-web-application"></a>Contoso 大學 Web 應用程式

您會在這些教學課程建置的應用程式是簡單的大學的網站。

使用者可以檢視和更新學生、 課程、 和講師資訊。 以下是幾個您要建立的畫面。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![編輯學生](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

此站台的 UI 樣式保留接近內建的範本，所產生的內容，讓本教學課程可以主要還是著重於如何使用 Entity Framework。

## <a name="prerequisites"></a>必要條件

請參閱**軟體版本**頁面的頂端。 Entity Framework 6 不是必要條件，因為您安裝教學課程的 EF NuGet 套件。

## <a name="create-an-mvc-web-application"></a>建立 MVC Web 應用程式

開啟 Visual Studio 並建立新的 C# Web 專案，名為"ContosoUniversity"。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

在**新增 ASP.NET 專案**對話方塊中，選取**MVC**範本。

如果**雲端中的主機**中核取方塊**Microsoft Azure**選取區段，將其清除。

按一下**變更驗證**。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

在**變更驗證**對話方塊中，選取**非驗證**，然後按一下 **確定**。 本教學課程中您不會要求使用者登入或限制存取權限登入的使用者。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

在 新增 ASP.NET 專案 對話方塊中，按一下 **確定**建立專案。

## <a name="set-up-the-site-style"></a>設定站台樣式

有一些簡單的變更將會設定網站 功能表、 配置和首頁。

開啟*_layout.cshtml\\_Layout.cshtml*，並進行下列變更：

- 將每個出現的 「 我的 ASP.NET 應用程式 」 和 「 應用程式名稱 」 變更為"Contoso 大學"。
- 加入功能表項目如學生、 課程、 講師和部門，並刪除連絡人項目。

所做的變更會反白顯示。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=6,19,24-27,38)]

在*Views\Home\Index.cshtml*，ASP.NET MVC 的相關文字使用文字來取代此應用程式有關的下列程式碼取代檔案的內容：

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

按 CTRL + F5 以執行站台。 您會看到與主功能表的 [首頁] 頁面。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

## <a name="install-entity-framework-6"></a>安裝 Entity Framework 6

從**工具**功能表上，按一下**NuGet 套件管理員**，然後按一下  **Package Manager Console**。

在**Package Manager Console**視窗輸入下列命令：

`Install-Package EntityFramework`

![安裝的 EF](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

影像顯示 6.0.0 安裝，但 NuGet 會安裝最新版的 Entity Framework （不包含發行前版本），為準，教學課程最新的更新即 6.1.1。

這個步驟是其中幾個步驟，本教學課程中，您已手動的方式，但其無法在完成自動由 ASP.NET MVC scaffolding 功能。 您所做它們以手動方式，讓您可以查看使用 Entity Framework 所需的步驟。 您將建立的 MVC 控制器和檢視表的更新版本使用 scaffolding。 替代方法是讓 scaffolding 自動安裝 EF NuGet 封裝、 建立資料庫的內容類別，以及建立連接字串。 當您準備好要執行它，這樣一來時，您只需要為略過這些步驟，並在建立實體類別之後 scaffold MVC 控制器。

## <a name="create-the-data-model"></a>建立資料模型

接下來您將建立 Contoso 大學應用程式的實體類別。 會先處理下列三個實體：

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

一對多關聯性之間`Student`和`Enrollment`實體，而且沒有之間的一對多關聯性`Course`和`Enrollment`實體。 換句話說，一位學生可以註冊任何數目的課程中，而且一個課程可以有任意數目的學生在它註冊。

下列各節中，您將建立這些實體的每個類別。

> [!NOTE]
> 如果您嘗試編譯專案，才能完成建立所有的這些實體類別時，就會發生編譯器錯誤。


### <a name="the-student-entity"></a>學生實體

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

在*模型*資料夾中，建立名為的類別檔案*Student.cs*和範本程式碼取代為下列程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs)]

`ID`屬性就會成為主要的索引鍵資料行對應至這個類別的資料庫資料表。 根據預設，Entity Framework 會解譯此屬性，名為`ID`或*classname* `ID`為主索引鍵。

`Enrollments`屬性是*導覽屬性*。 導覽屬性會保留此實體與相關的其他實體。 在此情況下，`Enrollments`屬性`Student`實體會保存所有`Enrollment`的實體有關的`Student`實體。 換句話說，如果給定`Student`資料庫中的資料列都有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將會包含這兩者`Enrollment`實體。

導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能例如*消極式載入*。 (將說明消極式載入稍後在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程稍後在本系列。)

如果導覽屬性都可以保存多個實體 （如同多對多或一對多的關聯性），其類型必須是的清單中的項目可以新增、 刪除和更新，例如`ICollection`。

### <a name="the-enrollment-entity"></a>註冊實體

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)

在*模型*資料夾中，建立*Enrollment.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`EnrollmentID`屬性會是主索引鍵，此實體會使用*classname* `ID`模式而不是`ID`本身做為您在中看到`Student`實體。 通常您會選擇其中一個模式，並在您的資料模型中使用它。 在這裡，變化說明您可以使用其中一個模式。 在稍後的教學課程中，您會看到如何使用`ID`沒有`classname`輕鬆地在資料模型中實作繼承。

`Grade`屬性是[列舉](https://msdn.microsoft.com/data/hh859576.aspx)。 問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](https://msdn.microsoft.com/library/2cf62fcy.aspx)。 為 null 的等級是不同於零的等級，null 表示等級不未知或尚未被指派。

`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。 `Enrollment`實體都與一個`Student`實體，所以此屬性只可以保存單一`Student`實體 (不同於`Student.Enrollments`導覽屬性之前看到，而可以包含多個`Enrollment`實體)。

`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。 `Enrollment`實體都與一個`Course`實體。

Entity Framework 會解譯為外部索引鍵屬性屬性如果名稱為*&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (例如， `StudentID`如`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性可以也具有相同名稱只是*&lt;主索引鍵屬性名稱&gt;* (例如，`CourseID`因為`Course`實體的主索引鍵是`CourseID`)。

### <a name="the-course-entity"></a>課程實體

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

在*模型*資料夾中，建立*Course.cs*，範本程式碼取代為下列程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

`Enrollments`屬性為導覽屬性。 A`Course`可以與任意數目的相關實體`Enrollment`實體。

我們將更多有關[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)本系列之後的教學課程中的屬性。 基本上，此屬性可讓您輸入的主索引鍵的課程，而不是需要產生它的資料庫。

## <a name="create-the-database-context"></a>建立的資料庫內容

協調對給定的資料模型的 Entity Framework 功能的主要類別是*資料庫內容*類別。 您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)類別。 在程式碼中指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 的行為。 在此專案中，類別會命名為`SchoolContext`。

ContosoUniversity 專案中建立資料夾，請以滑鼠右鍵按一下中的專案**方案總管] 中**按一下**新增**，然後按一下 [**新資料夾**。 將新的資料夾命名*DAL* （適用於資料存取層）。 該資料夾中建立新的類別檔案命名為*SchoolContext.cs*，並將範本程式碼取代下列程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

### <a name="specifying-entity-sets"></a>指定的實體集

此程式碼建立[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)每一個實體集的屬性。 在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，和*實體*對應至資料表中的資料列。

> [!NOTE] 
> 
> 您可以省略`DbSet<Enrollment>`和`DbSet<Course>`陳述式，它會運作方式相同。 Entity Framework 會將其包含隱含因為`Student`實體參考`Enrollment`實體和`Enrollment`實體參考`Course`實體。


### <a name="specifying-the-connection-string"></a>指定連接字串

連接字串 （其中您稍後會加入至 Web.config 檔案） 的名稱傳遞給建構函式。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs?highlight=1)]

您也可以傳入連接字串而不是儲存在 Web.config 檔案中的其中一個名稱。 如需使用指定的資料庫選項的詳細資訊，請參閱[Entity Framework-連線和模型](https://msdn.microsoft.com/data/jj592674)。

如果您未指定連接字串或其中一個名稱明確，Entity Framework 會假設連接字串名稱是類別名稱相同。 在此範例中的預設連接字串名稱將`SchoolContext`，與您正在明確地指定相同。

### <a name="specifying-singular-table-names"></a>指定單一的資料表名稱

`modelBuilder.Conventions.Remove`陳述式中的[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會從正在 pluralized 防止資料表名稱。 如果沒有這樣做，請在資料庫中產生的資料表就會命名為`Students`， `Courses`，和`Enrollments`。 相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。 針對是否要複數化資料表名稱，開發人員並沒有共識。 本教學課程使用的單數形式，但是很重要的一點是，您可以選取您想要包含或省略這行程式碼的任何表單。

## <a name="set-up-ef-to-initialize-the-database-with-test-data"></a>初始化測試資料的資料庫設定 EF

Entity Framework 可以自動建立 （或卸除並重新建立） 為您的應用程式執行時的資料庫。 您可以指定每次執行應用程式，或只有當模型為與現有的資料庫同步處理時，應該此。 您也可以撰寫`Seed`Entity Framework 自動填入測試資料，以便在建立資料庫之後呼叫的方法。

預設行為是建立資料庫，只有當它不存在 （並擲回例外狀況，如果模型已變更，而且資料庫已經存在）。 本節中，您會指定資料庫應該卸除並重新建立每次模型變更。 卸除資料庫時，會造成您的資料遺失。 這通常是 [確定] 在開發期間，因為`Seed`方法會執行時重新建立資料庫，並會重新建立您的測試資料。 但在生產環境中通常不想每次您要變更資料庫結構描述會失去所有資料。 稍後您會看到如何使用 Code First 移轉，若要變更資料庫結構描述，而不是卸除並重新建立資料庫處理模型的變更。

在 DAL 資料夾中建立新的類別檔案命名為*SchoolInitializer.cs*和使用的範本程式碼取代  
下列程式碼，這會使資料庫建立時所需，並將測試資料載入至新的資料庫。

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.cs)]

`Seed`方法採用資料庫內容物件，做為輸入參數，並在方法中的程式碼使用  
若要將新的實體加入至資料庫物件。 每個實體類型，此程式碼建立的集合新增  
 實體，將它們加入至適當`DbSet`屬性，然後按一下 儲存至資料庫的變更。 它不是  
需要呼叫`SaveChanges`方法之後每個群組的實體，如同您在這裡，但這樣做，可協助  
如果程式碼寫入資料庫時發生例外狀況，您會找到問題的來源。

若要告知 Entity Framework 使用初始設定式類別，加入項目加入`entityFramework`應用程式中的項目*Web.config*檔案 （都有一個根專案資料夾中），如下列範例所示：

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.xml?highlight=2-6)]

`context type`指定完整的內容類別名稱和組件，而`databaseinitializer type`指定初始設定式類別和它是在組件的完整限定的名稱。 (當您不想要使用初始設定式的 EF 時，您可以設定屬性`context`項目： `disableDatabaseInitialization="true"`。)如需詳細資訊，請參閱[Entity Framework 的組態檔設定](https://msdn.microsoft.com/data/jj556606)。

除了設定中的初始設定式*Web.config*檔案是在程式碼中藉由新增`Database.SetInitializer`陳述式來`Application_Start`方法中的*Global.asax.cs*檔案。 如需詳細資訊，請參閱[Entity Framework Code First 中了解資料庫初始設定式](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)。

應用程式現在已設定讓該一次執行的第一次存取資料庫時  
應用程式中，Entity Framework 比較的模型資料庫 (您`SchoolContext`和實體類別)。 如果有差異，應用程式會卸除並重新建立資料庫。

> [!NOTE]
> 當您部署到生產環境 web 伺服器應用程式時，您必須移除或停用程式碼，會卸除並重新建立資料庫。 您將會執行，在這一系列之後的教學課程中。


## <a name="set-up-ef-to-use-a-sql-server-express-localdb-database"></a>設定為使用 SQL Server Express LocalDB 資料庫的 EF

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是輕量版 SQL Server Express Database Engine。 容易安裝及設定、 啟動視情況下，並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您能夠使用資料庫，做為*.mdf*檔案。 您可以將 LocalDB 資料庫檔案放在*應用程式\_資料*web 專案，如果您想要能夠加以複製的資料庫專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用*.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。 在 Visual Studio 2012 和更新版本中，使用 Visual Studio 的預設會安裝 LocalDB。

一般 SQL Server Express 不用於生產環境 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是使用 IIS。

在本教學課程中您將使用 LocalDB。 開啟應用程式*Web.config*檔案，然後加入`connectionStrings`前面的項目`appSettings`項目，如下列範例所示。 (請確定您更新*Web.config*根專案資料夾中的檔案。 另外還有*Web.config*檔案位於*檢視*不需要更新的子資料夾。)

如果您使用 Visual Studio 2015，取代為"v11.0 「 連接字串中的"MSSQLLocalDB"，因為預設的 SQL Server 執行個體名稱已變更。

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.xml?highlight=1-3)]

已加入的連接字串指定了 Entity Framework 會使用名為 LocalDB 資料庫*ContosoUniversity1.mdf*。 （資料庫還不存在。EF 會建立它。）如果您想要在建立資料庫您*應用程式\_資料*資料夾中，您可以加入`AttachDBFilename=|DataDirectory|\ContosoUniversity1.mdf`至連接字串。 如需連接字串的詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。

您不需要有連接字串*Web.config*檔案。 如果您並未提供的連接字串，Entity Framework 會使用預設值，其中根據您的內容類別。 如需詳細資訊，請參閱[Code First 到新的資料庫](https://msdn.microsoft.com/data/jj193542)。

## <a name="creating-a-student-controller-and-views"></a>建立學生控制器和檢視

現在您將建立網頁上顯示資料，並要求資料的程序將會自動觸發  
建立資料庫。 您一開始會藉由建立新的控制站。 但您這麼做之前，請建置專案，以提供 MVC 控制器的 scaffolding 模型和內容類別。

1. 以滑鼠右鍵按一下**控制器**資料夾中的**方案總管] 中**，選取**新增**，然後按一下 [**新的 Scaffold 項目**。
- 在**新增 Scaffold**對話方塊中，選取**的 MVC 5 控制器與檢視，使用 Entity Framework**。

    ![加入 Scaffold](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)
- 在 加入控制器 對話方塊中，進行下列選擇，然後按一下 **新增**:

    - 模型類別：**學生 (ContosoUniversity.Models)**。 （如果您沒有看到下拉式清單中的這個選項，建置專案並再試一次）。
    - 資料內容類別： **SchoolContext (ContosoUniversity.DAL)**。
    - 控制器名稱： **StudentController** (不 StudentsController)。
    - 保留其他欄位的預設值。

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    當您按一下**新增**，scaffolder 建立 StudentController.cs 檔案和一組檢視 （.cshtml 檔案） 可與控制器。 您建立使用 Entity Framework 的專案時，在未來您也可以利用的 scaffolder 的一些額外的功能： 只建立您的第一個模型類別，不會建立連接字串，然後在**加入控制器**方塊中指定新的內容類別。 將建立 scaffolder 您`DbContext`類別和您的連線字串以及控制器和檢視。
- Visual Studio 隨即開啟*Controllers\StudentController.cs*檔案。 您會看到已建立的資料庫內容物件具現化類別變數：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    `Index`動作方法會取得一份學生*學生*實體集藉由讀取`Students`的資料庫內容執行個體的屬性：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    *Student\Index.cshtml*檢視會顯示在資料表中的這份清單：

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cshtml)]
- 按 CTRL+F5 執行專案。 （如果您收到 「 無法建立陰影複製 」 的錯誤，請關閉瀏覽器並再試一次）。

    按一下**學生**索引標籤，查看測試資料的`Seed`插入的方法。 根據如何窄瀏覽器視窗，您會看到學生 索引標籤中的連結，最上層的網址列，或您必須按一下右上角才能看到該連結。

    ![功能表按鈕](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    ![學生索引頁](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)

## <a name="view-the-database"></a>檢視的資料庫

當您執行 [學生] 頁面上，而應用程式嘗試存取資料庫時，EF 已看到不時發生的任何資料庫，它會建立一個，然後執行 seed 方法，以填入資料的資料庫。

您可以使用**伺服器總管**或**SQL Server 物件總管**(SSOX) 在 Visual Studio 中檢視的資料庫。 此教學課程中，您將使用**伺服器總管**。 (在 Visual Studio Express 版本早於 2013年**伺服器總管**稱為**資料庫總管**。)

1. 關閉瀏覽器。
2. 在**伺服器總管**，展開**資料連接**，展開**學校內容 (ContosoUniversity)**，然後展開**資料表**查看新的資料庫中的資料表。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
3. 以滑鼠右鍵按一下**學生**資料表，並按一下**顯示資料表資料**以查看所建立的資料行和資料列插入資料表。

    ![學生資料表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
4. 關閉**伺服器總管**連線。

*ContosoUniversity1.mdf*和*.ldf*資料庫檔案位於`C:\Users\<yourusername>`資料夾。

因為您正在使用`DropCreateDatabaseIfModelChanges`初始設定式，您無法立即進行變更以`Student`類別，再次執行應用程式，而且資料庫會自動是重新建立它們以符合您的變更。 例如，如果您加入`EmailAddress`屬性`Student`類別，學生頁面再次執行，然後查看資料表一次，將會看到新`EmailAddress`資料行。

## <a name="conventions"></a>慣例

您必須撰寫 Entity framework 可以為您建立完整的資料庫中的程式碼數量是因為使用了最小*慣例*，或讓 Entity Framework 的假設。 其中部分已註明，或使用而不用知道它們的存在：

- Pluralized 的形式的實體類別名稱做為資料表名稱。
- 實體屬性名稱會用於資料行名稱。
- 實體屬性是名為`ID`或*classname* `ID`被當做主索引鍵屬性。
- 屬性會解譯為外部索引鍵屬性上，如果名稱為*&lt;導覽屬性名稱&gt;&lt;主索引鍵屬性名稱&gt;* (例如， `StudentID` 的`Student`導覽屬性，因為`Student`實體的主索引鍵是`ID`)。 外部索引鍵屬性可以也具有相同名稱只是&lt;主索引鍵屬性名稱&gt;(例如，`EnrollmentID`因為`Enrollment`實體的主索引鍵是`EnrollmentID`)。

您已看過可覆寫慣例。 例如，指定資料表名稱不應該 pluralized，，和更新版本，您會看到如何明確地將屬性標記為外部索引鍵屬性。 您將進一步了解慣例，以及如何覆寫它們在[建立多個複雜資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)教學課程稍後在本系列。 如需慣例的詳細資訊，請參閱[程式碼優先 」 慣例](https://msdn.microsoft.com/data/jj679962)。

## <a name="summary"></a>總結

現在您已建立簡單的應用程式使用 Entity Framework 和 SQL Server Express LocalDB 來儲存和顯示資料。 在下列的教學課程中，您將學習如何執行基本 CRUD （建立、 讀取、 更新、 刪除） 作業。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[下一步](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
