---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
title: "建立 ASP.NET MVC 應用程式 (1 / 10) 的 Entity Framework 資料模型 |Microsoft 文件"
author: tdykstra
description: "此教學課程系列的較新版本可用，Visual Studio 2013、 Entity Framework 6 和 MVC 5。 Contoso 大學範例 web 應用程式 de..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 4ba029b6-ee7c-4e45-a0e7-b703c37e5d9a
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: c25ebf472df5dcbc664257cdf8678bfac535d846
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-an-entity-framework-data-model-for-an-aspnet-mvc-application-1-of-10"></a>建立 ASP.NET MVC 應用程式 (1 / 10) 的 Entity Framework 資料模型
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> > [!NOTE] 
> > 
> > A[此教學課程系列的新版](../../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)可用，Visual Studio 2013、 Entity Framework 6，和 MVC 5。
> 
> 
> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 和 Visual Studio 2012。 範例應用程式是針對虛構的 Contoso 大學的網站。 其中包括功能，例如許可學生、 課程建立和講師指派。 此教學課程說明如何建置 Contoso 大學範例應用程式。 您可以[下載完成的應用程式](https://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)。
> 
> ## <a name="code-first"></a>Code First
> 
> 有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，和*Code First*。 本教學課程適用於第一個程式碼。 如需如何選擇適合您案例的這些工作流程和指引之間差異的詳細資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/en-us/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="mvc"></a>MVC
> 
> 範例應用程式根據[ASP.NET MVC](../../../index.md)。 如果您想要使用 ASP.NET Web Form 模型，請參閱[模型繫結和 Web Form](../../../../web-forms/overview/presenting-and-managing-data/model-binding/retrieving-data.md)教學課程系列和[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 8 | Windows 7 |
> | Visual Studio 2012 | Visual Studio 2012 Express for Web。 這會自動安裝 Windows Azure SDK 如果您還沒有 VS 2012 或 VS 2012 Express for Web。 Visual Studio 2013 應該會運作，但本教學課程尚未經過測試，和某些功能表選取項目和對話方塊不同。 [VS 2013 版本的 Windows Azure SDK](https://go.microsoft.com/fwlink/p/?linkid=323510)需要 Windows Azure 部署。 |
> | .NET 4.5 | 在.NET 4 中運作的大多數功能顯示，但有些不會。 例如，在 EF 列舉支援需要.NET 4.5。 |
> | Entity Framework 5 |  |
> | [Windows Azure SDK 2.1](https://go.microsoft.com/fwlink/p/?linkid=323511) | 如果您略過 Windows Azure 的部署步驟，您不需要 SDK。 新的 sdk 版本發行時，連結將會安裝較新版本。 在此情況下，您可能必須調整一些新的 UI 和功能的指示。 |
> 
> ## <a name="questions"></a>問題
> 
> 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)、 [Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/en-US/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。
> 
> ## <a name="acknowledgments"></a>通知
> 
> 請參閱中的序列的最後一個教學課程[通知和 VB 的附註](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#acknowledgments)。
> 
> ## <a name="original-version-of-the-tutorial"></a>原始版的教學課程
> 
> 本教學課程的原始版本位於[EF 4.1 / MVC 3 電子書](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#GettingStartedwiththeEntityFramework4.1usingASP.NETMVC)。


## <a name="the-contoso-university-web-application"></a>Contoso 大學 Web 應用程式

您會在這些教學課程建置的應用程式是簡單的大學的網站。

使用者可以檢視和更新學生、 課程、 和講師資訊。 以下是幾個您要建立的畫面。

![Students_Index_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image1.png)

![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image2.png)

此站台的 UI 樣式保留接近內建的範本，所產生的內容，讓本教學課程可以主要還是著重於如何使用 Entity Framework。

## <a name="prerequisites"></a>必要條件

方向和螢幕擷取畫面，在本教學課程假設您使用[Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads)或[Visual Studio 2012 Express for Web](https://go.microsoft.com/fwlink/?LinkID=275131)、 最新的 [更新] 和 Azure SDK for.NET 安裝為準，年 7 月，2013。 您可以取得這些全部都使用下列連結：

[Azure SDK for.NET (Visual Studio 2012)](https://go.microsoft.com/fwlink/?LinkId=254364)

如果您有安裝 Visual Studio 時，上面的連結將會安裝任何遺漏的元件。 如果您沒有 Visual Studio，連結將會安裝 Visual Studio 2012 Express for Web。 您可以使用 Visual Studio 2013 中，但某些必要的程序和畫面會不同。

## <a name="create-an-mvc-web-application"></a>建立 MVC Web 應用程式

開啟 Visual Studio 並建立新 C# 專案名為"ContosoUniversity 」 使用**ASP.NET MVC 4 Web 應用程式**範本。 請確定您的目標**.NET Framework 4.5** (您將使用[`enum`屬性](https://msdn.microsoft.com/en-us/data/hh859576.aspx)，而且這需要.NET 4.5)。

![New_project_dialog_box](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image3.png)

在**新增 ASP.NET MVC 4 專案**對話方塊中，選取**網際網路應用程式**範本。

保留**Razor**檢視引擎選取，並留下**建立單元測試專案**清除核取方塊。

按一下 [確定]。

![Project_template_options](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image4.png)

## <a name="set-up-the-site-style"></a>設定站台樣式

有一些簡單的變更將會設定網站 功能表、 配置和首頁。

開啟*_layout.cshtml\\_Layout.cshtml*，並以下列程式碼取代檔案的內容。 所做的變更會反白顯示。

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample1.cshtml?highlight=5,15,25-28,43)]

此程式碼進行下列變更：

- 取代"Contoso 大學"「 My ASP.NET MVC 應用程式 」 和 「 您的標誌在此 「 的範本執行個體。
- 新增將稍後在本教學課程中使用的數個動作連結。

在*Views\Home\Index.cshtml*，檔案的內容取代為下列的程式碼以消除有關 ASP.NET 和 MVC 範本段落：

[!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample2.cshtml)]

在*Controllers\HomeController.cs*，變更值`ViewBag.Message`中`Index`要 「 歡迎使用 Contoso 大學 ！"，如下列範例所示的動作方法：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample3.cs?highlight=3)]

按 CTRL + F5 以執行站台。 您會看到與主功能表的 [首頁] 頁面。

![Contoso_University_home_page](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image5.png)

## <a name="create-the-data-model"></a>建立資料模型

接下來您將建立 Contoso 大學應用程式的實體類別。 會先處理下列三個實體：

![Class_diagram](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image6.png)

一對多關聯性之間`Student`和`Enrollment`實體，而且沒有之間的一對多關聯性`Course`和`Enrollment`實體。 換句話說，一位學生可以註冊任何數目的課程中，而且一個課程可以有任意數目的學生在它註冊。

下列各節中，您將建立這些實體的每個類別。

> [!NOTE]
> 如果您嘗試編譯專案，才能完成建立所有的這些實體類別時，就會發生編譯器錯誤。


### <a name="the-student-entity"></a>學生實體

![Student_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image7.png)

在*模型*資料夾中，建立*Student.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample4.cs)]

`StudentID`屬性就會成為主要的索引鍵資料行對應至這個類別的資料庫資料表。 根據預設，Entity Framework 會解譯此屬性，名為`ID`或*classname* `ID`為主索引鍵。

`Enrollments`屬性是*導覽屬性*。 導覽屬性會保留此實體與相關的其他實體。 在此情況下，`Enrollments`屬性`Student`實體會保存所有`Enrollment`的實體有關的`Student`實體。 換句話說，如果給定`Student`資料庫中的資料列都有兩個相關`Enrollment`資料列 (包含該學生的主索引鍵的資料列中的值及其`StudentID`外部索引鍵資料行)，該`Student`實體的`Enrollments`導覽屬性將會包含這兩者`Enrollment`實體。

導覽屬性通常會定義為`virtual`，讓他們可以利用某些 Entity Framework 功能例如*消極式載入*。 (將說明消極式載入稍後在[讀取相關資料](reading-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)教學課程稍後在本系列。

如果導覽屬性都可以保存多個實體 （如同多對多或一對多的關聯性），其類型必須是的清單中的項目可以新增、 刪除和更新，例如`ICollection`。

### <a name="the-enrollment-entity"></a>註冊實體

![Enrollment_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image8.png)

在*模型*資料夾中，建立*Enrollment.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample5.cs)]

等級屬性是[列舉](https://msdn.microsoft.com/en-us/data/hh859576.aspx)。 問號之後`Grade`型別宣告表示`Grade`屬性是[可為 null](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)。 為 null 的等級是不同於零的等級，null 表示等級不未知或尚未被指派。

`StudentID`屬性是外部索引鍵，而對應的導覽屬性是`Student`。 `Enrollment`實體都與一個`Student`實體，所以此屬性只可以保存單一`Student`實體 (不同於`Student.Enrollments`導覽屬性之前看到，而可以包含多個`Enrollment`實體)。

`CourseID`屬性是外部索引鍵，而對應的導覽屬性是`Course`。 `Enrollment`實體都與一個`Course`實體。

### <a name="the-course-entity"></a>課程實體

![Course_entity](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image9.png)

在*模型*資料夾中，建立*Course.cs*，以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample6.cs)]

`Enrollments`屬性為導覽屬性。 A`Course`可以與任意數目的相關實體`Enrollment`實體。

我們將更多有關 [[DatabaseGenerated](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedattribute(v=vs.110).aspx)([DatabaseGeneratedOption](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.schema.databasegeneratedoption(v=vs.95).aspx)。無）] 下一個教學課程中的屬性。 基本上，此屬性可讓您輸入的主索引鍵的課程，而不是需要產生它的資料庫。

## <a name="create-the-database-context"></a>建立的資料庫內容

協調對給定的資料模型的 Entity Framework 功能的主要類別是*資料庫內容*類別。 您建立這個類別衍生自[System.Data.Entity.DbContext](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext(v=VS.103).aspx)類別。 在程式碼中指定資料模型中包含哪些實體。 您也可以自訂某些 Entity Framework 的行為。 在此專案中，類別會命名為`SchoolContext`。

建立名為的資料夾*DAL* （適用於資料存取層）。 該資料夾中建立新的類別檔案命名為*SchoolContext.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample7.cs)]

此程式碼建立[DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=VS.103).aspx)每一個實體集的屬性。 在 Entity Framework 詞彙*實體集*通常會對應到資料庫資料表，和*實體*對應至資料表中的資料列。

`modelBuilder.Conventions.Remove`陳述式中的[OnModelCreating](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.onmodelcreating(v=vs.103).aspx)方法會從正在 pluralized 防止資料表名稱。 如果您沒有這麼做，所產生之資料表就會命名為`Students`， `Courses`，和`Enrollments`。 相反地，資料表名稱會`Student`， `Course`，和`Enrollment`。 針對是否要複數化資料表名稱，開發人員並沒有共識。 本教學課程使用的單數形式，但是很重要的一點是，您可以選取您想要包含或省略這行程式碼的任何表單。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

[LocalDB](https://blogs.msdn.com/b/sqlexpress/archive/2011/07/12/introducing-localdb-a-better-sql-express.aspx)是輕量版 SQL Server Express Database Engine，視需要啟動並以使用者模式執行。 LocalDB 以特殊的執行模式執行的 SQL Server Express，可讓您能夠使用資料庫，做為*.mdf*檔案。 一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用*.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。

一般 SQL Server Express 不用於生產環境 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是使用 IIS。

在 Visual Studio 2012 和更新版本中，使用 Visual Studio 的預設會安裝 LocalDB。 在 Visual Studio 2010 和舊版中，SQL Server Express （而不要使用 LocalDB) 預設會安裝 Visual studio;您必須手動安裝它，如果您使用 Visual Studio 2010。

在此教學課程中您將使用 LocalDB 資料庫可以儲存在*應用程式\_資料*資料夾*.mdf*檔案。 開啟根*Web.config*檔案，然後加入新的連接字串至`connectionStrings`集合中，如下列範例所示。 (請確定您更新*Web.config*根專案資料夾中的檔案。 另外還有*Web.config*檔案位於*檢視*不需要更新的子資料夾。)

[!code-xml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample8.xml)]

根據預設，Entity Framework 會尋找名稱相同的連接字串`DbContext`類別 (`SchoolContext`這個專案)。 已新增的連接字串會指定名為 LocalDB 資料庫*ContosoUniversity.mdf*位於*應用程式\_資料*資料夾。 如需詳細資訊，請參閱[ASP.NET Web 應用程式的 SQL Server 連接字串](https://msdn.microsoft.com/en-us/library/jj653752.aspx)。

實際上，您不需要指定的連接字串。 如果您並未提供的連接字串，Entity Framework 會建立一個。不過，資料庫可能不會在*應用程式\_資料*應用程式資料夾。 會建立此資料庫的資訊，請參閱[Code First 到新的資料庫](https://msdn.microsoft.com/en-us/data/jj193542)。

`connectionStrings`集合也具有名為連接字串`DefaultConnection`用的成員資格資料庫。 您將不會在本教學課程使用成員資格資料庫。 兩個連接字串之間唯一的差別是資料庫名稱和 name 屬性值。

## <a name="set-up-and-execute-a-code-first-migration"></a>設定及執行程式碼的第一次移轉

當您第一次開始開發應用程式時，您的資料模型變更常見問題，以及每次將模型變更，它會取得與資料庫同步處理。 您可以設定 Entity Framework 自動卸除並重新建立每次變更資料模型資料庫。 因為測試資料是容易重新建立，但您已部署至實際執行環境之後，您通常想要更新資料庫結構描述，但不卸除資料庫，這是不及早在開發中的問題。 移轉功能可讓程式碼第一次更新資料庫卸除並重新建立它。 您可能要使用在新專案的開發週期的早期[DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/en-us/library/gg679604(v=vs.103).aspx)卸除、 重新建立，並重新植入資料庫中，每次將模型變更。 您做好準備以部署您的應用程式的其中一個，您可以將轉換的移轉方法。 此教學課程中只會使用移轉作業。 如需詳細資訊，請參閱[Code First 移轉](https://msdn.microsoft.com/en-us/data/jj591621)和[移轉錄影畫面數列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。

### <a name="enable-code-first-migrations"></a>啟用 Code First 移轉

1. 從**工具**功能表上，按一下 **程式庫套件管理員**然後**Package Manager Console**。

    ![Selecting_Package_Manager_Console](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image10.png)
2. 在`PM>`提示字元輸入下列命令：

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample9.ps1)]

    ![enable-migrations 命令](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image11.png)

    此命令會建立*移轉*資料夾 ContosoUniversity 專案中，它會將該資料夾中放*configuration.cs 中*設定移轉，您可以編輯的檔案。

    ![Migrations 資料夾](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image12.png)

    `Configuration`類別包含`Seed`建立資料庫時，每次更新之後的資料模型變更時呼叫的方法。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample10.cs)]

    目的`Seed`方法是為了讓您將測試資料插入資料庫之後第一個程式碼就會建立或更新它。

### <a name="set-up-the-seed-method"></a>設定種子方法

[種子](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx)方法執行 Code First 移轉建立資料庫時，以及每次它將資料庫更新為最新的移轉。 Seed 方法的目的是讓您能夠將資料插入資料表之前應用程式存取資料庫第一次。

在舊版的第一個程式碼中，發行移轉之前，它是很常見的`Seed`方法來插入測試資料，因為資料庫已完全刪除並從頭開始重新建立每一次在開發期間的模型變更。 Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料[種子](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx)方法通常不是必要。 事實上，您不想`Seed`方法插入的測試資料，如果您將使用移轉將資料庫部署到生產環境，因為`Seed`方法會在生產環境中執行。 在此情況下您想`Seed`方法，以您想要在生產環境中要插入的資料插入資料庫。 例如，您可能想要包括在實際的部門名稱的資料庫`Department`資料表在生產環境中可以使用應用程式時。

此教學課程中，您將會使用移轉部署，但您`Seed`方法將還是插入測試資料，以便更輕鬆地查看應用程式的功能而不需要以手動方式插入大量資料的運作方式。

1. 取代內容*configuration.cs 中*以下列程式碼，將測試資料載入至新的資料庫檔案。 

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample11.cs)]

    [種子](https://msdn.microsoft.com/en-us/library/hh829453(v=vs.103).aspx)方法會採用做為輸入參數，為資料庫物件和方法中的程式碼會使用該物件加入資料庫中的新實體。 每個實體類型，程式碼會建立新實體的集合，將它們加入至適當[DbSet](https://msdn.microsoft.com/en-us/library/system.data.entity.dbset(v=vs.103).aspx)屬性，然後按一下 儲存至資料庫的變更。 不需要呼叫[SaveChanges](https://msdn.microsoft.com/en-us/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法之後每個實體群組，做為執行這項，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。

    某些插入資料的陳述式使用[AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，以執行 「 更新插入 」 作業。 因為`Seed`方法執行每個移轉，因為您嘗試新增的資料列，就會有第一個移轉建立資料庫之後插入資料。 「 更新插入 」 作業會防止您嘗試要插入的資料列已經存在，但它會發生的錯誤***會覆寫***資料您測試應用程式時所做的任何變更。 測試資料表中的資料部分可能不會想才會發生： 在某些情況下測試時變更資料時要您的資料庫更新後要保持的變更。 在此情況下您要執行條件式的 insert 作業： 插入資料列，只有當其不存在。 Seed 方法會使用這兩種方法。

    第一個參數傳遞至[AddOrUpdate](https://msdn.microsoft.com/en-us/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定用來檢查資料列是否已經存在的屬性。 您提供的測試學生資料`LastName`因為每個清單中的最後一個名稱是唯一的可以針對此用途使用屬性：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample12.cs)]

    此程式碼會假設這些姓氏是唯一。 如果您手動新增一位學生重複最後一個名稱，您會得到下列例外狀況下一次執行移轉。

    序列包含一個以上的項目

    如需有關`AddOrUpdate`方法，請參閱[小心以 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 部落格上。

    加入的程式碼`Enrollment`實體不會使用`AddOrUpdate`方法。 它會檢查是否實體已經存在，並將實體，如果不存在。 這種方法將會保留您對註冊等級執行移轉時變更。 此程式碼迴圈的每個成員`Enrollment`[清單](https://msdn.microsoft.com/en-us/library/6sh2ey19.aspx)，如果資料庫中找不到註冊，它會將註冊加入資料庫。 第一次更新資料庫，資料庫將是空的因此它會將加入每個註冊。

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample13.cs)]

    如需有關如何偵錯資訊`Seed`方法以及如何處理重複的資料，例如兩個學生名為"Alexander Carson"，請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson 部落格上。
2. 建置專案。

### <a name="create-and-execute-the-first-migration"></a>建立和執行第一次移轉

1. 在 [封裝管理員主控台] 視窗中，輸入下列命令： 

    [!code-powershell[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample14.ps1)]

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image13.png)

    `add-migration`命令會新增至 Migrations 資料夾*[DateStamp]\_InitialCreate.cs*包含程式碼會建立資料庫檔案。 第一個參數 (`InitialCreate)`用於檔案名稱，而且可以是您所要的任何; 您通常會選擇的單字或片語來摘要列出所要完成移轉中的作業。 例如，您可能會命名稍後移轉&quot;AddDepartmentTable&quot;。

    ![初始移轉 migrations 資料夾](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image14.png)

    `Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表和`Down`方法會刪除它們。 移轉呼叫`Up`方法，以實作資料模型變更，以進行移轉。 當您輸入命令，以回復更新、 移轉呼叫`Down`方法。 下列程式碼顯示的內容`InitialCreate`檔案：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample15.cs)]

    `update-database`命令執行`Up`方法來建立資料庫然後執行`Seed`方法來擴展資料庫。

SQL Server 資料庫現在已建立資料模型。 資料庫的名稱是*ContosoUniversity*，而*.mdf*檔案位於您的專案*應用程式\_資料*資料夾因為這是您在中指定您連接字串。

您可以使用**伺服器總管**或**SQL Server 物件總管**(SSOX) 在 Visual Studio 中檢視的資料庫。 此教學課程中，您將使用**伺服器總管**。 在 Visual Studio Express 2012 for Web，**伺服器總管**稱為**資料庫總管**。

1. 從**檢視**功能表上，按一下 **伺服器總管**。
2. 按一下**加入連接**圖示。

    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image15.png)
3. 如果您收到提示**選擇資料來源** 對話方塊中，按一下  **Microsoft SQL Server**，然後按一下 **繼續**。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image16.png)
4. 在**加入連接**對話方塊方塊中，輸入**(localdb) \v11.0**如**伺服器名稱**。 在下**選取或輸入資料庫名稱**，選取**ContosoUniversity。**  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image17.png)
5. 按一下 [確定] 
6. 展開**SchoolContext** ，然後展開 **資料表**。  
  
    ![](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image18.png)
7. 以滑鼠右鍵按一下**學生**資料表，並按一下**顯示資料表資料**以查看所建立的資料行和資料列插入資料表。

    ![學生資料表](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image19.png)

## <a name="creating-a-student-controller-and-views"></a>建立學生控制器和檢視

下一個步驟是在您可以使用其中一個資料表的應用程式中建立 ASP.NET MVC 控制器和檢視。

1. 若要建立`Student`控制站，以滑鼠右鍵按一下**控制站**資料夾中的**方案總管] 中**，選取**新增**，然後按一下 [**控制器**. 在**加入控制器**對話方塊方塊中，請選取下列項目，然後按一下**新增**: 

    - 控制器名稱： **StudentController**。
    - 範本：**具有讀取/寫入動作和檢視表、 使用 Entity Framework 的 MVC 控制器**。
    - 模型類別：**學生 (ContosoUniversity.Models)**。 （如果您沒有看到下拉式清單中的這個選項，建置專案並再試一次）。
    - 資料內容類別： **SchoolContext (ContosoUniversity.Models)**。
    - 檢視： **Razor (CSHTML)**。 （預設值。）

    ![Add_Controller_dialog_box_for_Student_controller](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image20.png)
- Visual Studio 隨即開啟*Controllers\StudentController.cs*檔案。 您會看到已建立的資料庫內容物件具現化類別變數：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample16.cs)]

    `Index`動作方法會取得一份學生*學生*實體集藉由讀取`Students`的資料庫內容執行個體的屬性：

    [!code-csharp[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample17.cs)]

    *Student\Index.cshtml*檢視會顯示在資料表中的這份清單：

    [!code-cshtml[Main](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/samples/sample18.cshtml)]
- 按 CTRL+F5 執行專案。

    按一下**學生**索引標籤，查看測試資料的`Seed`插入的方法。

    ![學生索引頁](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application/_static/image21.png)

## <a name="conventions"></a>慣例

您必須撰寫 Entity framework 可以為您建立完整的資料庫中的程式碼數量是因為使用了最小*慣例*，或讓 Entity Framework 的假設。 其中部分已經被記：

- Pluralized 的形式的實體類別名稱做為資料表名稱。
- 實體屬性名稱會用於資料行名稱。
- 實體屬性是名為`ID`或*classname* `ID`被當做主索引鍵屬性。

您已看過可覆寫慣例 （例如，您指定資料表名稱不應該 pluralized），您將學習更多關於慣例及如何覆寫在[建立多個複雜資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)教學課程稍後在本系列。 如需詳細資訊，請參閱[程式碼優先 」 慣例](https://msdn.microsoft.com/en-us/data/jj679962)。

## <a name="summary"></a>總結

現在您已建立簡單的應用程式使用 Entity Framework 和 SQL Server Express 來儲存和顯示資料。 在下列的教學課程中，您將學習如何執行基本 CRUD （建立、 讀取、 更新、 刪除） 作業。 您可以離開此頁面底部的意見反應。 請讓我們知道您所喜歡的教學課程的這個部分的方式，以及我們如何改善它。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

>[!div class="step-by-step"]
[下一步](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
