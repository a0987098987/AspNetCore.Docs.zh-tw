---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: 建立 ASP.NET MVC 應用程式 (1 / 10) 的 Entity Framework 資料模型 |Microsoft Docs
author: tdykstra
description: 本教學課程系列的較新版本可用，Visual Studio 2013、 Entity Framework 6，和 MVC 5。 Contoso 大學範例 web 應用程式 de...
ms.author: riande
ms.date: 07/30/2013
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b691f718258f98e03513a089ca26b286f284765e
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/10/2018
ms.locfileid: "48913228"
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>建立 ASP.NET MVC 應用程式 (1 / 10) 的 Entity Framework 資料模型
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A[本教學課程系列的較新版本](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)可用時，Visual Studio 2013、 Entity Framework 6，和 MVC 5。
> 
> 
> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 這個範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。 本教學課程系列會說明如何建立 Contoso 大學範例應用程式。 您可以[下載完成的應用程式](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)。
> 
> ## <a name="code-first"></a>Code First
> 
> 有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，並*Code First*。 本教學課程中，適合於您的程式碼第一次。 如需如何選擇最適合您案例的差異，這些工作流程和指引的相關資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="mvc"></a>MVC
> 
> 範例應用程式的基礎[ASP.NET MVC](../../../index.md)。 如果您想要使用 ASP.NET Web Form 模型，請參閱[模型繫結和 Web Form](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md)教學課程系列並[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **在本教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express for Web。 如果您還沒有 VS 2012 或 VS 2012 Express for Web，這會自動被安裝 Windows Azure sdk。 Visual Studio 2013 應該可行，但本教學課程尚未經過測試，且某些功能表選取項目和對話方塊都不同。 [VS 2013 版本的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510)需要 Windows Azure 部署。 |
> | .NET 4.5 | 顯示的功能大多可在.NET 4 中，但某些不會。 例如，在 EF 中的列舉支援需要.NET 4.5。 |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | 如果您略過 Windows Azure 的部署步驟，您不需要 SDK。 新的 sdk 版本發行時，連結會安裝較新版本。 在此情況下，您可能必須調整一些新的 UI 和功能的指示。 |
> 
> ## <a name="questions"></a>問題
> 
> 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)，則[Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。
> 
> ## <a name="acknowledgments"></a>感謝
> 
> 請參閱中的序列的最後一個教學課程[通知和 VB 注意事項](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments)。
> 
> ## <a name="original-version-of-the-tutorial"></a>本教學課程的原始版本
> 
> 本教學課程的原始版本位於[EF 4.1 / MVC 3 電子書](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)。


## <a name="the-contoso-university-web-application"></a>Contoso 大學 Web 應用程式

您在這些教學課程中會建置的應用程式為一個簡單的大學網站。

使用者可以檢視和更新學生、課程和教師資訊。 以下是您會建立的幾個畫面。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

此網站的 UI 樣式與內建範本產生的相當類似，以使此教學課程能聚焦於如何使用 Entity Framework。

## <a name="prerequisites"></a>必要條件

方向和螢幕擷取畫面，在本教學課程假設您使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads)或[Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131)、 最新的 [更新] 和 Azure SDK for.NET 安裝為準，年 7 月，2013。 您可以取得所有這些使用下列連結：

[Azure SDK for.NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

如果您已安裝的 Visual Studio 時，上面的連結會安裝任何遺漏的元件。 如果您沒有 Visual Studio，連結會安裝 Visual Studio 2012 Express for Web。 您可以使用 Visual Studio 2013，但某些畫面與必要程序會有所不同。

## <a name="create-an-mvc-web-application"></a>建立 MVC Web 應用程式

開啟 Visual Studio 並建立新 C# 專案名為"ContosoUniversity"使用**ASP.NET MVC 4 Web 應用程式**範本。 請確定您的目標 **.NET Framework 4.5** (您將會使用[`enum`屬性](https://msdn.microsoft.com/data/hh859576.aspx)，而這需要.NET 4.5)。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

在 [**新的 ASP.NET MVC 4 專案**] 對話方塊中選取**網際網路應用程式**範本。

離開**Razor**檢視引擎選取，並留下**建立單元測試專案**清除核取方塊。

按一下 [確定 **Deploying Office Solutions**]。

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>設定網站樣式

一些簡單的變更會設定網站的功能表、配置和首頁。

開啟*Views\Shared\\_Layout.cshtml*，並以下列程式碼取代檔案的內容。 所做的變更已醒目提示。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

此程式碼會進行下列變更：

- 「 My ASP.NET MVC 應用程式 」 和 「 您標誌的位置 」 的範本執行個體取代為"Contoso University"。
- 新增將用於本教學課程稍後的幾個動作連結。

在  *Views\Home\Index.cshtml*，檔案的內容取代為下列的程式碼，以消除關於 ASP.NET 和 MVC 範本段落：

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

在  *Controllers\HomeController.cs*，變更的值`ViewBag.Message`在`Index`要 「 歡迎使用 Contoso 大學 ！ 」，如下列範例所示的動作方法：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

按下 CTRL + F5，執行站台。 您會看到與主功能表的 [首頁] 頁面。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>建立資料模型

接下來您會為 Contoso 大學應用程式建立實體類別。 您首先要使用下列三個實體：

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

在 `Student` 和 `Enrollment` 實體之間存在一對多關聯性，`Course` 與 `Enrollment` 實體之間也存在一對多關聯性。 換句話說，一位學生可以註冊並參加任何數目的課程，而一個課程也可以有任何數目的學生註冊。

在下節中，您會為這些實體建立各自的類別。

> [!NOTE]
> 如果您嘗試編譯專案，才能完成建立所有這些實體類別時，您會收到編譯器錯誤。


### <a name="the-student-entity"></a>Student 實體

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

在 *模型*資料夾中，建立*Student.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID` 屬性會成為資料庫資料表中的主索引鍵資料行，並對應至這個類別。 根據預設，Entity Framework 的屬性解譯，稱為`ID`或是*classname* `ID`為主索引鍵。

`Enrollments` 屬性為*導覽屬性*。 導覽屬性會保留與此實體相關的其他實體。 在此情況下，`Enrollments`的屬性`Student`實體將保存的所有`Enrollment`相關的實體`Student`實體。 換句話說，如果給定`Student`資料庫中的資料列有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將包含這兩個`Enrollment`實體。

導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能這類*消極式載入*。 (將會說明消極式載入稍後，在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)稍後在本系列教學課程。

若導覽屬性可保有多個實體 (例如在多對多或一對多關聯性中的情況)，其類型必須為一個清單，使得實體可以在該清單中新增、刪除或更新，例如 `ICollection`。

### <a name="the-enrollment-entity"></a>Enrollment 實體

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

在 *Models* 資料夾中，建立 *Enrollment.cs*，然後使用下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

Grade 屬性是[enum](https://msdn.microsoft.com/data/hh859576.aspx)。 問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](https://msdn.microsoft.com/library/2cf62fcy.aspx)。 為 null 的成績等級是不同於成績為零： null 表示成績未知或尚未指派。

`StudentID` 屬性是外部索引鍵，對應的導覽屬性是 `Student`。 `Enrollment` 實體與一個 `Student` 實體關聯，因此屬性僅能保有單一 `Student` 實體 (不像您先前看到的 `Student.Enrollments` 導覽屬性可保有多個 `Enrollment` 實體)。

`CourseID` 屬性是外部索引鍵，對應的導覽屬性是 `Course`。 一個 `Enrollment` 實體與一個 `Course` 實體建立關聯。

### <a name="the-course-entity"></a>Course 實體

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

在 *模型*資料夾中，建立*Course.cs*，以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments` 屬性為導覽屬性。 `Course` 實體可以與任何數量的 `Enrollment` 實體相關。

我們將進一步討論 [[DatabaseGenerated](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx)。無）] 下一個教學課程中的屬性。 基本上，此屬性可讓您為課程輸入主索引鍵，而非讓資料庫產生它。

## <a name="create-the-database-context"></a>建立資料庫內容

協調 Entity Framework 功能，為指定的資料模型的主要類別是*資料庫內容*類別。 您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/library/system.data.entity.dbcontext(v=VS.103).aspx)類別。 在您的程式碼中，您會指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 行為。 在此專案中，類別命名為 `SchoolContext`。

建立名為的資料夾*DAL* （適用於資料存取層）。 在該資料夾中建立新的類別檔案，名為*SchoolContext.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

此程式碼會建立[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=VS.103).aspx)每一個實體集的屬性。 在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，以及*實體*對應至資料表中的資料列。

`modelBuilder.Conventions.Remove`中的陳述式[OnModelCreating](https://msdn.microsoft.com/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會防止資料表名稱的複數化。 如果您沒有這麼做，所產生之資料表就會命名為`Students`， `Courses`，和`Enrollments`。 相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。 針對是否要複數化資料表名稱，開發人員並沒有共識。 本教學課程使用的單數形式，但很重要的一點是，您可以選取任何您想要包含或省略這行程式碼的表單。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是輕量版的 SQL Server Express Database Engine 會視需要啟動，並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。 一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用 *.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。

SQL Server Express 不是常用的生產 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是在搭配 IIS 運作。

在 Visual Studio 2012 和更新版本中，使用 Visual Studio 的預設會安裝 LocalDB。 在 Visual Studio 2010 和舊版中，在預設情況下使用 Visual Studio，安裝 SQL Server Express （不含 LocalDB)您必須手動安裝它，如果您使用 Visual Studio 2010。

在本教學課程中您將使用 LocalDB，資料庫可以儲存在*應用程式\_資料*的資料夾 *.mdf*檔案。 開啟根目錄*Web.config*檔案，並將新的連接字串加入`connectionStrings`集合，如下列範例所示。 (請確定您更新*Web.config*根專案資料夾中的檔案。 另外還有*Web.config*檔案位於*檢視*您不需要更新的子資料夾。)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

根據預設，Entity Framework 會尋找名稱相同的連接字串`DbContext`類別 (`SchoolContext`這個專案)。 您已新增連接字串會指定名為 LocalDB 資料庫*ContosoUniversity.mdf*位於*應用程式\_資料*資料夾。 如需詳細資訊，請參閱 < [ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/library/jj653752.aspx)。

您實際上不需要指定連接字串。 如果您未提供的連接字串，Entity Framework 會為您建立一個;不過，資料庫可能不在*應用程式\_資料*應用程式的資料夾。 如需會建立此資料庫的資訊，請參閱[Code First 至新的資料庫](https://msdn.microsoft.com/data/jj193542)。

`connectionStrings`集合也具有名為連接字串`DefaultConnection`用於成員資格資料庫。 您不會在本教學課程使用成員資格資料庫。 資料庫名稱和名稱屬性值的兩個連接字串之間唯一的差別。

## <a name="set-up-and-execute-a-code-first-migration"></a>設定及執行程式碼的第一次移轉

當您第一次開始開發應用程式時，您的資料模型變更頻繁，且每次模型變更，它會取得與資料庫同步處理。 您可以設定 Entity Framework 自動卸除並重新建立每次變更資料模型資料庫。 因為測試資料會輕鬆重新建立，但您已部署至生產環境之後，您通常想要更新資料庫結構描述，但不卸除資料庫，這是不在開發初期的問題。 移轉功能可讓 Code First 來更新資料庫卸除並重新建立它。 您可能要使用在新的專案開發週期早期[DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=vs.103).aspx)卸除、 重新建立，並重新植入資料庫中，每次模型變更。 您做好準備以部署您的應用程式的其中一個，您可以將它轉換成的移轉方法。 本教學課程中，您只會使用移轉。 如需詳細資訊，請參閱 < [Code First Migrations](https://msdn.microsoft.com/data/jj591621)並[移轉螢幕錄製影片系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。

### <a name="enable-code-first-migrations"></a>啟用 Code First 移轉

1. 從**工具**功能表上，按一下**NuGet 套件管理員**，然後**Package Manager Console**。

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. 在`PM>`提示字元輸入下列命令：

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![啟用移轉命令](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    此命令會建立*移轉*資料夾中 [ContosoUniversity] 專案，並將該資料夾中放*Configuration.cs*設定移轉，您可以編輯的檔案。

    ![Migrations 資料夾](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration`類別包含`Seed`在建立資料庫時，以及每次更新後的資料模型變更時呼叫的方法。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    的目的`Seed`方法是讓您將測試資料插入資料庫，Code First 會加以建立或更新它之後。

### <a name="set-up-the-seed-method"></a>設定種子方法

[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法執行 Code First 移轉會建立資料庫以及每次它將資料庫更新為最新的移轉。 種子方法的目的是讓您將資料插入資料表之前的應用程式會在 第一次存取資料庫。

在舊版的 Code First，發行移轉之前，它是很常見的`Seed`方法來插入測試資料，因為資料庫已完全刪除並從頭開始重新建立在開發期間每次模型變更。 使用 Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法通常不是必要。 事實上，您不想`Seed`方法來插入測試資料，如果您將使用移轉將資料庫部署到生產環境，因為`Seed`方法會在生產環境中執行。 在此情況下您想`Seed`方法，以將您想要在生產環境中要插入的資料插入至資料庫。 例如，您可能想要包括在實際的部門名稱的資料庫`Department`資料表在生產環境中，您可以使用應用程式時。

本教學課程中，您將使用移轉來進行部署，但您`Seed`方法都會仍要插入的測試資料，以便讓您更輕鬆地查看應用程式的功能而不需要以手動方式插入大量資料的運作方式。

1. 內容取代*Configuration.cs*下列程式碼，會將測試資料載入新的資料庫檔案。 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法會採用資料庫內容物件做為輸入參數，並在方法中的程式碼會使用該物件來將新實體新增至資料庫。 每個實體類型，程式碼會建立新實體的集合，將它們新增至適當[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)屬性，然後按一下 儲存至資料庫的變更。 不需要呼叫[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)實體的每一個群組之後的方法，做為在這裡，完成，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。

    某些插入資料的陳述式使用[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)執行"upsert"作業的方法。 因為`Seed`方法會執行每個移轉，因為您嘗試新增的資料列會有第一個移轉建立資料庫之後插入資料。 "Upsert"作業可避免當您嘗試插入資料列已存在，但它會發生的錯誤***會覆寫***資料，您可能會在測試應用程式時所做的任何變更。 某些資料表中的測試資料與您可能不想發生這種情況： 在某些情況下當您將資料變更時測試您的要資料庫更新後要保持變更。 在此情況下您想要進行條件式的 insert 作業： 插入資料列，只有當不存在。 種子方法會使用這兩種方法。

    第一個參數傳遞給[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定要用來檢查資料列是否已經存在的屬性。 為您提供測試學生資料`LastName`屬性可以使用針對此目的，因為每個清單中的最後一個名稱是唯一的：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    此程式碼會假設這些姓氏是唯一。 如果您以手動方式新增最後一個名稱重複的學生，您會收到下列例外狀況下一次您執行移轉。

    序列包含一個以上的項目

    如需詳細資訊`AddOrUpdate`方法，請參閱 <<c2> [ 負責使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 的部落格上。

    新增的程式碼`Enrollment`不會使用實體`AddOrUpdate`方法。 它會檢查是否實體已經存在，而且如果不存在，插入實體。 這種方法將會保留您對註冊級移轉執行時變更。 此程式碼迴圈的每個成員`Enrollment`[清單](https://msdn.microsoft.com/library/6sh2ey19.aspx)和資料庫中找不到註冊，它會將註冊加入資料庫。 第一次更新資料庫，資料庫將會是空的因此它會將新增每個註冊。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    如需如何偵錯資訊`Seed`方法，以及如何處理重複的資料，例如兩個名為"Alexander Carson"的學生請參閱[植入及偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson 部落格上。
2. 建置專案。

### <a name="create-and-execute-the-first-migration"></a>建立並執行第一次移轉

1. 在 [套件管理員主控台] 視窗中，輸入下列命令： 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration`命令會新增至 Migrations 資料夾 *[時間戳記]\_InitialCreate.cs*包含程式碼以建立資料庫檔案。 第一個參數 (`InitialCreate)`用於檔案名稱，而且可以是任何您想要的結果，您通常會選擇的單字或片語，概括未移轉正在進行。 例如，您可能會在此名稱稍後進行移轉&quot;AddDepartmentTable&quot;。

    ![移轉資料夾中，使用初始移轉](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up`方法`InitialCreate`類別會建立對應至資料模型實體集，資料庫資料表和`Down`方法會刪除它們。 Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。 當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。 下列程式碼顯示的內容`InitialCreate`檔案：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database`命令會執行`Up`方法來建立資料庫然後執行`Seed`方法來填入資料庫。

SQL Server 資料庫現在已建立您的資料模型。 資料庫的名稱是*ContosoUniversity*，而 *.mdf*檔案位於您的專案*應用程式\_資料*資料夾，因為這是您在中指定您連接字串。

您可以使用**伺服器總管**或是**SQL Server 物件總管**(SSOX)，以在 Visual Studio 中檢視的資料庫。 您將使用本教學課程**伺服器總管**。 在 Visual Studio Express 2012 for Web**伺服器總管**稱為**資料庫總管**。

1. 從**檢視**功能表上，按一下**伺服器總管**。
2. 按一下 **加入連接**圖示。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. 如果您收到提示**選擇資料來源** 對話方塊中，按一下**Microsoft SQL Server**，然後按一下**繼續**。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. 在 **加入連接**對話方塊方塊中，輸入 **(localdb) \v11.0** for**伺服器名稱**。 底下**選取或輸入資料庫名稱**，選取**ContosoUniversity。**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. 按一下 [確定] 
6. 依序展開**SchoolContext** ，然後展開**資料表**。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. 以滑鼠右鍵按一下**學生**資料表，然後按一下**顯示資料表資料**以查看所建立的資料行和資料列插入至資料表。

    ![Student 資料表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>建立學生控制器和檢視

下一個步驟是在您的應用程式可以使用其中一個資料表中建立 ASP.NET MVC 控制器和檢視。

1. 若要建立`Student`控制站，以滑鼠右鍵按一下**控制器**資料夾中的**方案總管] 中**，選取**新增**，然後按一下 [**控制器**. 在 **新增控制器**對話方塊方塊中，進行下列選擇，然後按一下**新增**: 

   - 控制器名稱： **StudentController**。
   - 範本：**具有讀取/寫入動作和檢視，使用 Entity Framework 的 MVC 控制器**。
   - 模型類別：**學生 (ContosoUniversity.Models)**。 （如果您沒有看到此選項，在下拉式清單中的，建置專案並再試一次）。
   - 資料內容類別： **SchoolContext (ContosoUniversity.Models)**。
   - 檢視： **Razor (CSHTML)**。 （預設值。）

     ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
2. Visual Studio 會開啟*Controllers\StudentController.cs*檔案。 您會看到已建立的資料庫內容物件具現化類別變數：

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

     `Index`動作方法會取得一份的學生*學生*實體集，請閱讀`Students`的資料庫內容執行個體的屬性：

     [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

     *Student\Index.cshtml*檢視以表格顯示這份清單：

     [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
3. 按 CTRL+F5 執行專案。

     按一下 **學生**索引標籤來查看測試資料，`Seed`插入的方法。

     ![學生 [索引] 頁面](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>慣例

您必須讓 Entity Framework 能夠為您建立完整的資料庫撰寫的程式碼數量非常小，因為使用了*慣例*，可讓 Entity Framework 的假設。 其中一些已記下來：

- 複數化的形式的實體類別名稱會用於資料表名稱。
- 實體屬性名稱會用於資料行名稱。
- 名為實體屬性`ID`或是*classname* `ID`被視為主索引鍵屬性。

您已了解可以覆寫慣例 （例如，您指定資料表名稱不應該要複數化），您也將了解更多關於慣例及如何覆寫其[建立更多複雜的資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)教學課程稍後在本系列中。 如需詳細資訊，請參閱 <<c0> [ 程式碼的第一個慣例](https://msdn.microsoft.com/data/jj679962)。

## <a name="summary"></a>總結

您現在已建立簡單的應用程式會使用 Entity Framework 和 SQL Server Express 來儲存和顯示資料。 在下列教學課程中，您將學習如何執行基本的 CRUD （建立、 讀取、 更新、 刪除） 作業。 您可以保留在此頁面底部提供意見反應。 請讓我們知道您喜歡本教學課程的這個部分的方式，我們可以如何改善它。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [下一步](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
