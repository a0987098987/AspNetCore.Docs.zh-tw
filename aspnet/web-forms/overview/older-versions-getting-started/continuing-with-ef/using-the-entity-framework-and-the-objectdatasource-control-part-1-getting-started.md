---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: 使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 1 部分： 開始使用 |Microsoft Docs
author: tdykstra
description: 本教學課程系列由開始使用 Entity Framework 的教學課程系列的 Contoso 大學 web 應用程式為基礎。 如果 yo...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 9eb39d0cf57e114537c76f33e2f4647196b0ff30
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837674"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 1 部分： 開始使用
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本教學課程系列是根據所建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列。 如果您未完成先前的教學課程，為本教學課程的起始點即可[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整的教學課程系列。
> 
> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 4.0 和 Visual Studio 2010 的 ASP.NET Web Forms 應用程式。 範例應用程式是虛構的 Contoso 大學網站。 其中包括的功能有學生入學許可、課程建立、教師指派。
> 
> 本教學課程會顯示在 C# 範例。 [可下載範例](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)包含 C# 和 Visual Basic 中的程式碼。
> 
> ## <a name="database-first"></a>第一次資料庫
> 
> 有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，並*Code First*。 本教學課程是針對第一個資料庫。 如需如何選擇最適合您案例的差異，這些工作流程和指引的相關資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="web-forms"></a>Web Form
> 
> 如同入門系列中，本教學課程系列會使用 ASP.NET Web Form 模型，並假設您知道如何使用 Visual Studio 中的 ASP.NET Web Form。 如果您沒有這麼做，請參閱 < [Getting Started with ASP.NET 4.5 Web Form](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 如果您想要使用 ASP.NET MVC 架構，請參閱[開始使用 ASP.NET MVC 的 Entity Framework 使用](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **在本教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web。 本教學課程尚未經過測試的 Visual Studio 更新版本。 有許多差異功能表選取項目、 對話方塊和範本。 |
> | .NET 4 | .NET 4.5 回溯相容於.NET 4 中，但本教學課程不經過.NET 4.5。 |
> | Entity Framework 4 | 本教學課程尚未經過測試新的 Entity Framework 的版本。 從 Entity Framework 5 開始，使用預設的 EF `DbContext API` ，引進了 EF 4.1。 EntityDataSource 控制項的設計旨在使用`ObjectContext`API。 如需如何使用 EntityDataSource 控制項`DbContext`API，請參閱 <<c2> [ 此部落格文章](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)。 |
> 
> ## <a name="questions"></a>問題
> 
> 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)，則[Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。


`EntityDataSource`控制項可讓您非常快速地建立應用程式，但它通常會要求您保留一段很長的商務邏輯和資料存取邏輯，在您 *.aspx*頁面。 如果您希望應用程式變得更複雜，並在需要持續維護，您可以投入更多的開發時間事先創造*多層式架構*或是*分層*應用程式結構這是更容易維護。 若要實作此架構，您分開的展示層商務邏輯層 (BLL) 和資料存取層 (DAL)。 若要實作這種結構的一個方式是使用`ObjectDataSource`控制項，而非`EntityDataSource`控制項。 當您使用`ObjectDataSource`控制項，您實作自己的資料存取程式碼，然後將它在叫用 *.aspx*頁面使用具有許多相同的控制項功能的其他資料來源控制項。 這可讓您使用 Web Form 控制項進行資料存取的優點結合多層式架構方法的優點。

`ObjectDataSource`控制項可讓您更大的彈性以及透過其他方式。 由於您撰寫自己的資料存取程式碼時，它很容易執行不只是讀取、 插入、 更新或刪除特定實體型別，這是工作的`EntityDataSource`控制項用來執行。 例如，您可以執行記錄每次更新實體時，每當刪除實體時，或自動檢查和更新相關資料，視需要插入具有外部索引鍵值的資料列時，將資料封存。

## <a name="business-logic-and-repository-classes"></a>商務邏輯和儲存機制類別

`ObjectDataSource`叫用您所建立的類別來控制運作方式。 類別包含方法，擷取和更新資料，而您提供的這些方法的名稱`ObjectDataSource`標記中的控制項。 轉譯或回傳的處理期間`ObjectDataSource`呼叫您所指定的方法。

除了基本的 CRUD 作業，您建立來搭配使用的類別`ObjectDataSource`控制可能需要執行商務邏輯時`ObjectDataSource`讀取或更新資料。 例如，當您更新部門時，您可能需要驗證沒有其他部門有相同的系統管理員，因為有人不能是多個部門的系統管理員。

在某些`ObjectDataSource`文件，例如[ObjectDataSource 類別概觀](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx)，控制項會呼叫類別，稱為*商務物件*包含商務邏輯和資料存取邏輯. 在本教學課程中，您將建立個別的類別，針對商務邏輯和資料存取邏輯。 封裝資料存取邏輯的類別稱為*存放庫*。 商務邏輯類別同時包含商務邏輯的方法和資料存取方法，但資料存取方法呼叫來執行資料存取工作存放庫。

您也會建立抽象層之間您 BLL 和 DAL，可促進自動化的單元測試的 BLL。 建立介面，並且使用介面，當您具現化的存放庫中的商務邏輯的類別會實作這個抽象層。 這項功能可讓您提供的商務邏輯的類別會實作存放庫介面的任何物件的參考。 對於一般操作而言，您會提供與 Entity Framework 搭配運作的存放庫物件。 進行測試，您可以提供適用於您可以輕鬆地操作，例如做為集合定義的類別變數的方式儲存的資料儲存機制物件。

下圖顯示商務邏輯的類別，其中包含資料存取邏輯，而不需要儲存機制，另一個使用儲存機制之間的差異。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

您將開始在其中建立網頁`ObjectDataSource`控制項直接繫結的儲存機制因為它只會執行基本的資料存取工作。 在下一個教學課程中建立的商務邏輯的類別與驗證邏輯，並繫結`ObjectDataSource`儲存機制類別而不是該類別的控制項。 您也將建立單元測試驗證邏輯。 在這一系列的第三個教學課程中，您將加入排序和篩選功能給應用程式。

您在本教學課程中建立頁面使用`Departments`的資料模型中所建立的實體集[快速入門教學課程系列](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>更新資料庫和資料模型

您將藉由變更兩個資料庫，都需要對應至您在建立資料模型的變更來開始本教學課程[Getting Started with Entity Framework 和 Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程。 在其中一個這些教學課程中，您要設計工具中手動資料庫變更之後，同步資料模型與資料庫進行變更。 本教學課程中，您將使用的設計師**從資料庫更新模型**工具來自動更新資料模型。

### <a name="adding-a-relationship-to-the-database"></a>加入至資料庫的關聯性

在 Visual Studio 中，開啟您建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework 和 Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列，然後再開啟`SchoolDiagram`資料庫圖表。

如果您看一下`Department`資料表在資料庫圖表中，您會看到它有`Administrator`資料行。 此資料行是外部索引鍵`Person`資料庫中定義的資料表，但沒有外部索引鍵關聯性。 您要建立關聯性，並更新資料模型，以便讓 Entity Framework 能夠自動處理此關聯性。

在資料庫圖表中，以滑鼠右鍵按一下`Department`資料表，然後選取**關聯性**。

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

在  **Foreign Key Relationships**中，按一下**新增**，然後按一下省略符號**資料表和資料行規格**。

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

在**資料表和資料行**對話方塊方塊中，設定主索引鍵的資料表和欄位設為`Person`並`PersonID`，設定外部索引鍵的資料表和欄位設為`Department`和`Administrator`。 (當您這樣做時，關聯性名稱會變更來自`FK_Department_Department`至`FK_Department_Person`。)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

按一下  **確定**中**資料表和資料行**方塊中，按一下**關閉**中**Foreign Key Relationships**方塊，然後儲存變更。 如果系統詢問您是否要儲存`Person`並`Department`資料表，按一下**是**。

> [!NOTE]
> 如果您已刪除`Person`對應到已在的資料的資料列`Administrator` 欄中，您將無法儲存此變更。 在此情況下，使用中的資料表編輯器**伺服器總管**以確保能夠`Administrator`中的值每個`Department`資料列都包含實際存在於記錄的識別碼`Person`資料表。
> 
> 儲存變更之後，您將無法刪除資料列從`Person`資料表如果該人員是部門管理員。 在生產環境應用程式中，您會提供特定的錯誤訊息，資料庫條件約束可防止刪除，或您可以指定串聯刪除時。 如需如何指定串聯刪除的範例，請參閱 < [Entity Framework 和 ASP.NET – 取得啟動第 2 部分](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md)。


### <a name="adding-a-view-to-the-database"></a>將檢視新增至資料庫

在新*Departments.aspx*您將建立的頁面上，您想要提供的講師，下拉式清單中 「 姓氏，名字"格式的名稱，讓使用者可以選取部門系統管理員。 若要讓您更輕鬆地這麼做，您會在資料庫中建立檢視。 檢視將會包含所需的下拉式清單資料： 全名 （格式正確）] 和 [記錄索引鍵。

在 **伺服器總管**，展開*School.mdf*，以滑鼠右鍵按一下**檢視**資料夾，然後選取**加入新的檢視**。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

按一下 [**關閉**當**加入資料表**] 對話方塊隨即出現，並將下列 SQL 陳述式貼到 [SQL] 窗格：

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

儲存為檢視`vInstructorName`。

### <a name="updating-the-data-model"></a>更新資料模型

在  *DAL*資料夾中，開啟*SchoolModel.edmx*檔案，以滑鼠右鍵按一下設計介面，然後選取**從資料庫更新模型**。

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

在 **選擇您的資料庫物件**對話方塊中，選取**新增**索引標籤，然後選取您剛才建立的檢視。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

按一下 [ **完成**]。

在設計工具中，您會看到此工具，建立`vInstructorName`實體與新的關聯之間`Department`和`Person`實體。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> 在 **輸出**並**錯誤清單**新的 windows，您可能會看到警告訊息，通知您此工具會自動建立主要金鑰`vInstructorName`檢視。 這是正常的現象。


當您參考新`vInstructorName`程式碼中的實體，您不想使用小寫"v"，前面加上的資料庫慣例。 因此，您將會重新命名的實體和模型中實體集。

開啟**模型瀏覽器**。 您會看到`vInstructorName`列為 實體類型和檢視。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

底下**SchoolModel** (不**SchoolModel.Store**)，以滑鼠右鍵按一下**vInstructorName** ，然後選取**屬性**。 在 **屬性**視窗中，變更**名稱**屬性設為"InstructorName"並變更**實體集名稱**"InstructorNames"的屬性。

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

儲存並關閉資料模型，然後再重新建置專案。

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>使用儲存機制類別和 ObjectDataSource 控制項

建立新的類別檔案中*DAL*資料夾，其命名*SchoolRepository.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

此程式碼提供單一`GetDepartments`方法會傳回的所有實體中`Departments`實體集。 因為您知道您將會存取`Person`每個資料列的導覽屬性傳回，您指定積極式載入該屬性使用`Include`方法。 類別也會實作`IDisposable`介面，以確保處置物件時釋放資料庫連線。

> [!NOTE]
> 常見的作法是建立每個實體類型的儲存機制類別。 在本教學課程中，會使用多個實體類型的一個儲存機制類別。 如需儲存機制模式的詳細資訊，請參閱中的文章[Entity Framework 小組的部落格](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)並[Julie Lerman 的部落格](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/)。


`GetDepartments`方法會傳回`IEnumerable`物件，而非`IQueryable`以確保傳回的集合是可使用，即使在存放庫物件本身已處置的物件。 `IQueryable`物件可能會導致資料庫存取權，只要存取，但儲存機制物件可能會處置資料繫結控制項嘗試轉譯資料的時間。 您也可以傳回另一個集合的型別，例如`IList`物件，而不是`IEnumerable`物件。 不過，傳回`IEnumerable`物件可確保您可以執行一般的唯讀清單處理工作是這類`foreach`迴圈與 LINQ 查詢，但是您無法加入或移除項目在集合中，可能暗示會是這類變更保存到資料庫。

建立*Departments.aspx*使用的頁面*Site.Master*主版頁面，並新增下列標記中的`Content`控制項，名為`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

此標記會建立`ObjectDataSource`會使用您剛才建立的儲存機制類別的控制項和`GridView`控制項來顯示資料。 `GridView`控制項會指定**編輯**並**刪除**命令，但是您還沒有加入尚未支援它們的程式碼。

數個資料行使用`DynamicField`控制項，好讓您可以利用自動的資料格式化與驗證功能。 這些工作，您必須呼叫`EnableDynamicData`方法中的`Page_Init`事件處理常式。 (`DynamicControl`控制項不會用於`Administrator`欄位，因為它們不適用於導覽屬性。)

`Vertical-Align="Top"`屬性時，會變重要稍後加入具有巢狀資料行`GridView`方格控制項。

開啟*Departments.aspx.cs*檔案，並新增下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

然後新增下列處理常式的頁面`Init`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

在  *DAL*資料夾中，建立新的類別檔案，名為*Department.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

此程式碼會將資料模型中的中繼資料。 它會指定`Budget`的屬性`Department`實體實際上代表貨幣，雖然其資料類型是`Decimal`，而且它會指定的值必須介於 0 和 $1,000,000.00。 它也會指定`StartDate`屬性的格式應為 mm/dd/yyyy 格式的日期。

執行*Departments.aspx*頁面。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

請注意，雖然您未指定的格式字串*Departments.aspx*頁面標記**預算**或是**Start Date**貨幣與日期的資料行，預設值格式化已套用至兩者`DynamicField`控制項，使用您在中提供的中繼資料*Department.cs*檔案。

## <a name="adding-insert-and-delete-functionality"></a>新增插入和刪除功能

開啟*SchoolRepository.cs*，新增下列程式碼，才能建立`Insert`方法和`Delete`方法。 程式碼也包含一個名為方法`GenerateDepartmentID`計算下一個可用的記錄金鑰值，以供`Insert`方法。 這是必要的因為資料庫未設定為自動為計算`Department`資料表。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach 方法

`DeleteDepartment`方法呼叫`Attach`方法，以重新建立記憶體中的實體與資料庫之間的物件內容的物件狀態管理員中的 連結是維護資料列它代表。 這必須發生在方法呼叫之前`SaveChanges`方法。

詞彙*物件內容*指的是衍生自 Entity Framework 類別`ObjectContext`您用來存取您的實體集和實體的類別。 在此專案的程式碼，則類別命名為`SchoolEntities`，和它的執行個體的名稱永遠`context`。 物件內容*物件的狀態管理員*是衍生自類別`ObjectStateManager`類別。 物件的連絡人會使用物件狀態管理員，來儲存實體物件和來追蹤是否每個會與其對應的資料表資料列或在資料庫中的資料列的同步處理。

當您讀取實體時，物件內容將它儲存在物件狀態管理員，並會持續追蹤的物件，表示是否與資料庫的同步處理。 比方說，如果您變更屬性值，指出您所變更的屬性已不再與資料庫的同步處理會設定旗標。 當您呼叫`SaveChanges`方法中，物件內容會知道如何處理在資料庫中，因為物件狀態管理員可讓您知道到底什麼是不同實體的目前狀態與資料庫的狀態。

不過，此程序通常無法 web 應用程式中，因為頁面轉譯之後處置物件內容執行個體讀取的實體，以及它的物件狀態管理員中的所有項目。 必須套用變更的物件內容執行個體是一個新具現化回傳的處理。 若是`DeleteDepartment`方法中，`ObjectDataSource`控制重新實體的原始版本會為您建立從值在檢視狀態，但重新建立此`Department`實體不存在於物件狀態管理員。 如果您呼叫`DeleteObject`這個重新建立的實體上的方法，呼叫會失敗，因為物件內容不知道實體是否與資料庫的同步處理。 不過，呼叫`Attach`方法會在重新建立相同的追蹤重新建立的實體和值之間項原本作業自動完成中舊版執行個體的物件內容中讀取實體時的資料庫中。

有些的時候，當您不想要追蹤實體在物件狀態管理員中，物件內容，而且您可以設定旗標，以防止它這麼做。 這個範例會顯示在本系列稍後的教學課程中。

### <a name="the-savechanges-method"></a>SaveChanges 方法

這個簡單的儲存機制類別會說明如何執行 CRUD 作業的基本原則。 在此範例中，`SaveChanges`每次更新之後，立即呼叫方法。 在實際執行的應用程式中，您可能想要呼叫`SaveChanges`從不同的方法，讓您更充分掌控資料庫更新時的方法。 （在下一個教學課程結尾處您會找到討論工作單位模式也就是其中一個方法，以及相關的更新協調一份白皮書的連結）。另外請注意，在範例中，`DeleteDepartment`方法不包含處理並行衝突的程式碼，在本系列稍後的教學課程中將新增程式碼，若要這樣做。

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>正在擷取講師名稱來選取插入時

使用者必須能夠建立新的部門時，從下拉式清單中的講師清單中選取系統管理員。 因此，新增下列程式碼*SchoolRepository.cs*建立一個方法來擷取使用您稍早建立之檢視的講師清單：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>插入部門建立頁面

建立*DepartmentsAdd.aspx*使用的頁面*Site.Master*頁面，然後加入下列標記中的`Content`控制項，名為`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

此標記會建立兩個`ObjectDataSource`控制項，一個用於新插入`Department`實體，另一個用於擷取講師名稱`DropDownList`可用來選取部門系統管理員的控制。 標記會建立`DetailsView`控制輸入新的部門，並指定控制項的處理常式`ItemInserting`事件，讓您可以設定`Administrator`外部索引鍵值。 在結束時`ValidationSummary`控制項來顯示錯誤訊息。

開啟*DepartmentsAdd.aspx.cs*並新增下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

新增下列類別變數和方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init`方法會啟用動態資料功能。 處理常式`DropDownList`控制項的`Init`事件儲存至控制項，與處理常式的參考`DetailsView`控制項的`Inserting`事件會使用該參考來取得`PersonID`所選取的講師和更新的值`Administrator`外部索引鍵屬性`Department`實體。

執行此頁面，加入新部門的資訊，然後按一下**插入**連結。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

輸入另一個新部門的值。 輸入數字大於在 1,000,000.00**預算**欄位和下一個欄位的索引標籤。 在欄位中，就會出現星號，如果您將滑鼠指標停留它，您可以看到您的中繼資料中輸入該欄位的錯誤訊息。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

按一下 **插入**，您會看到所顯示的錯誤訊息和`ValidationSummary`在頁面底部的控制項。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

接下來，關閉瀏覽器，並開啟*Departments.aspx*頁面。 新增刪除功能*Departments.aspx*加上頁面`DeleteMethod`屬性設定為`ObjectDataSource`控制項，和`DataKeyNames`屬性設定為`GridView`控制項。 這些控制項的開頭標記現在將會類似下列的範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

執行網頁。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

刪除您在執行時新增的部門*DepartmentsAdd.aspx*頁面。

## <a name="adding-update-functionality"></a>新增的更新功能

開啟*SchoolRepository.cs*並新增下列`Update`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

當您按一下 **更新**中*Departments.aspx*頁面上，`ObjectDataSource`控制項，會建立兩個`Department`要傳遞至實體`UpdateDepartment`方法。 其中包含已儲存檢視狀態中的原始值，另一個輸入中的新值`GridView`控制項。 中的程式碼`UpdateDepartment`方法傳遞`Department`具有原始的值，以實體`Attach`方法，以建立追蹤的實體和功能的資料庫。 然後程式碼會傳遞`Department`具有新值的實體`ApplyCurrentValues`方法。 物件內容會比較新舊值。 如果新的值從舊的值不同，物件內容會變更屬性值。 `SaveChanges`方法接著會更新只變更資料行在資料庫中的。 （不過，如果此實體的更新函式已對應至預存程序中，整個資料列會更新不論所變更之資料行）。

開啟*Departments.aspx*檔案，並新增下列屬性以`DepartmentsObjectDataSource`控制項：

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 此原因的舊值必須是儲存在檢視狀態，以便在新的值相較`Update`方法。
- `OldValuesParameterFormatString="orig{0}"`   
 這會通知控制項所用的原始值的參數名稱`origDepartment`。

開頭標記的標記`ObjectDataSource`控制項現在會類似下列範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

新增`OnRowUpdating="DepartmentsGridView_RowUpdating"`屬性設定為`GridView`控制項。 您將使用此設定`Administrator`屬性值會根據使用者選取下拉式清單中的資料列。 `GridView`開頭標記，現在類似下列範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

新增`EditItemTemplate`控制`Administrator`資料行`GridView`控制項，緊接在後`ItemTemplate`控制該資料行：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

這`EditItemTemplate`控制項是類似`InsertItemTemplate`控制中*DepartmentsAdd.aspx*頁面。 差別在於使用設定控制項的初始值`SelectedValue`屬性。

再`GridView`控制項，新增`ValidationSummary`控制中*DepartmentsAdd.aspx*頁面。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

開啟*Departments.aspx.cs*後面的部分類別宣告中，新增下列程式碼，建立參考的私用欄位和`DropDownList`控制項：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

然後加入處理常式`DropDownList`控制項的`Init`事件並`GridView`控制項的`RowUpdating`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

處理常式`Init`事件會將儲存的參考`DropDownList`中類別欄位的控制項。 處理常式`RowUpdating`事件會使用參考來取得使用者所輸入的值，並將它套用至`Administrator`屬性`Department`實體。

使用*DepartmentsAdd.aspx*頁面，即可加入新科系，然後執行*Departments.aspx*頁面，然後按一下**編輯**上您所加入的資料列。

> [!NOTE]
> 您無法編輯不是由您加入的資料列 (也就是，均已存在於資料庫)，因為在資料庫中有無效的資料已由資料庫所建立的資料列的系統管理員是學生。 如果您嘗試編輯其中一個，就會報告類似的錯誤的錯誤頁面 `'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

如果您輸入無效**預算**金額，然後按一下**更新**，您會看到相同的星號和您在看到的錯誤訊息*Departments.aspx*頁面。

變更欄位值或選取不同的系統管理員，然後按一下**更新**。 變更已顯示。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

如此即完成簡介使用`ObjectDataSource`控制基本的 CRUD （建立、 讀取、 更新、 刪除） 作業使用 Entity Framework。 您已經建置簡單的多層式架構應用程式，但是商務邏輯層仍緊密結合到資料存取層變得非常複雜自動化的單元測試。 在下列教學課程中，您會看到如何實作存放庫模式，以利於進行單元測試。

> [!div class="step-by-step"]
> [下一步](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
