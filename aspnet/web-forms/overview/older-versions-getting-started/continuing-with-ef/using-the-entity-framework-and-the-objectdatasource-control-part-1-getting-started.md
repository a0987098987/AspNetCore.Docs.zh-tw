---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
title: "使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 1 部分： 開始使用 |Microsoft 文件"
author: tdykstra
description: "此教學課程系列為基礎所建立的開始使用 Entity Framework 的教學課程系列的 Contoso 大學 web 應用程式。 如果 yo..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: 244278c1-fec8-4255-8a8a-13bde491c4f5
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started
msc.type: authoredcontent
ms.openlocfilehash: 83fe815af9030aee10a5204718b00c79925e9126
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-1-getting-started"></a>使用 Entity Framework 4.0 和 ObjectDataSource 控制項，第 1 部分： 使用者入門
====================
由[Tom Dykstra](https://github.com/tdykstra)

> 此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列。 如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。
> 
> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET Web Form 應用程式使用 Entity Framework 4.0 和 Visual Studio 2010。 範例應用程式是針對虛構的 Contoso 大學的網站。 其中包括功能，例如許可學生、 課程建立和講師指派。
> 
> 教學課程會示範 C# 中的範例。 [可下載範例](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)包含 C# 和 Visual Basic 中的程式碼。
> 
> ## <a name="database-first"></a>第一次資料庫
> 
> 有三種方式，您可以使用 Entity Framework 中的資料： *Database First*， *Model First*，和*Code First*。 本教學課程適用於第一個資料庫。 如需如何選擇適合您案例的這些工作流程和指引之間差異的詳細資訊，請參閱[Entity Framework 的開發工作流程](https://msdn.microsoft.com/library/ms178359.aspx#dbfmfcf)。
> 
> ## <a name="web-forms"></a>Web Form
> 
> 像是入門系列，本教學課程系列使用 ASP.NET Web Form 模型，並假設您知道如何使用 Visual Studio 中的 ASP.NET Web Form。 如果您沒有這麼做，請參閱[開始使用 ASP.NET 4.5 Web Form](../../getting-started/getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)。 如果您想要使用 ASP.NET MVC framework，請參閱[Entity Framework 使用 ASP.NET MVC 使用者入門](../../../../mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。
> 
> ## <a name="software-versions"></a>軟體版本
> 
> | **教學課程中所示** | **也可以搭配** |
> | --- | --- |
> | Windows 7 | Windows 8 |
> | Visual Studio 2010 | Visual Studio 2010 Express for Web. 本教學課程尚未經過測試的 Visual Studio 版本。 有許多差異功能表選取項目、 對話方塊和範本。 |
> | .NET 4 | .NET 4.5 回溯相容於.NET 4 中，但本教學課程不經過.NET 4.5。 |
> | Entity Framework 4 | 本教學課程尚未經過測試新的 Entity Framework 的版本。 從 Entity Framework 5 開始，依預設使用 EF`DbContext API`導入的已 EF 4.1。 EntityDataSource 控制項的設計使用`ObjectContext`應用程式開發介面。 如需如何使用 EntityDataSource 控制項`DbContext`API，請參閱[此部落格文章](https://blogs.msdn.com/b/webdev/archive/2012/09/13/how-to-use-the-entitydatasource-control-with-entity-framework-code-first.aspx)。 |
> 
> ## <a name="questions"></a>問題
> 
> 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)、 [Entity Framework 和 LINQ to Entities 論壇](https://social.msdn.microsoft.com/forums/adodotnetentityframework/threads/)，或[StackOverflow.com](http://stackoverflow.com/)。


`EntityDataSource`控制項可讓您建立的應用程式非常快速，但它通常會要求您保存相當數量的商務邏輯和資料存取邏輯中的您*.aspx*頁面。 如果您預期應用程式變得更複雜，並在需要持續性維護，您可投入更多的開發時間最前面位置才能建立*多層式架構*或*分層*應用程式結構這是更容易維護。 若要實作這種架構，您會從商務邏輯層 (BLL) 和資料存取層 (DAL) 分隔展示層。 若要實作此結構的一種方式為使用`ObjectDataSource`而不是控制`EntityDataSource`控制項。 當您使用`ObjectDataSource`控制項，您實作您自己的資料存取程式碼，然後叫用它在*.aspx*頁面使用的控制項有許多相同功能的其他資料來源控制項。 這可讓您與使用 Web Form 控制項進行資料存取的優點結合多層式架構方法的優點。

`ObjectDataSource`控制項可讓您更大的彈性以及其他的方式。 因為您撰寫自己的資料存取程式碼，所以您更輕鬆地執行不只是讀取、 插入、 更新或刪除特定實體類型，這是工作的`EntityDataSource`控制項用來執行。 比方說，您可以執行記錄每次更新實體，每當刪除實體時，或自動核取和更新的相關資料視需要插入具有外部索引鍵值的資料列時已封存的資料。

## <a name="business-logic-and-repository-classes"></a>商務邏輯和儲存機制類別

`ObjectDataSource`叫用您所建立的類別來控制的運作。 類別包含的方法，擷取和更新資料，而且您提供的這些方法的名稱`ObjectDataSource`標記中的控制項。 轉譯或回傳的處理期間`ObjectDataSource`呼叫您所指定的方法。

除了基本 CRUD 作業，您建立要使用的類別`ObjectDataSource`控制項可能需要執行商務邏輯時`ObjectDataSource`讀取或更新資料。 例如，當您更新部門時，您可能需要驗證沒有其他部門具有相同的系統管理員，因為一個人不能有多個部門的系統管理員。

在某些`ObjectDataSource`文件，例如[ObjectDataSource 類別概觀](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.aspx)，控制項會呼叫類別，稱為*商務物件*同時包含商務邏輯和資料存取邏輯. 在本教學課程中，您將建立個別的類別，為商務邏輯和資料存取邏輯。 封裝資料存取邏輯的類別稱為*儲存機制*。 商務邏輯類別包含商務邏輯方法和資料存取方法，但資料存取方法呼叫來執行資料存取工作的儲存機制。

您也會建立抽象層 BLL 之間 DAL，可協助自動化的單元測試的 BLL。 此抽象層是由建立介面，並且使用介面，當您具現化的儲存機制中的商務邏輯類別實作。 這可讓您提供的商務邏輯類別會實作儲存機制介面的任何物件的參考。 對於一般操作而言，您會提供適用於 Entity Framework 的儲存機制物件。 進行測試，您必須提供資料儲存在您可以輕鬆地操作，例如做為集合定義的類別變數的方式運作的儲存機制物件。

下圖顯示包含資料存取的邏輯，而不儲存機制的商務邏輯類別，另一個則會使用儲存機制之間的差異。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image1.png)

您會開始在其中建立網頁`ObjectDataSource`控制項繫結至儲存機制直接因為它只會執行基本的資料存取工作。 下一個教學課程中您會建立商務邏輯類別的驗證邏輯，並繫結`ObjectDataSource`儲存機制類別而不是該類別的控制項。 您也會建立單元測試驗證邏輯。 在這一系列的第三個教學課程中，您將加入排序和篩選應用程式的功能。

您在本教學課程中建立的頁面使用`Departments`資料模型中所建立的實體集[快速入門教學課程系列](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image3.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image5.png)

## <a name="updating-the-database-and-the-data-model"></a>更新資料庫和資料模型

藉由在資料庫中，這需要將資料模型中所建立的對應變更兩項變更，您會開始本教學課程[開始使用 Entity Framework 和 Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程。 在其中一個這些教學課程，您要設計工具中手動資料庫變更後，同步資料模型與資料庫進行變更。 在此教學課程中，您將使用設計工具的**由資料庫更新模型**工具，以自動更新資料模型。

### <a name="adding-a-relationship-to-the-database"></a>加入至資料庫的關聯性

在 Visual Studio 中，開啟您在中建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 和 Web Form](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-1.md)教學課程系列，然後再開啟`SchoolDiagram`資料庫圖表。

如果您看一下`Department`資料表在資料庫圖表中，您會看到它有`Administrator`資料行。 這個資料行是外部索引鍵`Person`資料庫中定義的資料表，但沒有外部索引鍵關聯性。 您要建立關聯性，並更新資料模型，好讓 Entity Framework 可以自動處理此關聯性。

在資料庫圖表中，以滑鼠右鍵按一下`Department`資料表，並選取**關聯性**。

[![Image80](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image7.png)

在**Foreign Key Relationships**中，按一下**新增**，然後按一下省略符號**資料表和資料行規格**。

[![Image81](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image9.png)

在**資料表和資料行**對話方塊方塊中，設定主索引鍵的資料表以及欄位至`Person`和`PersonID`，設定外部索引鍵的資料表和欄位到`Department`和`Administrator`。 (當您這樣做時，將會變更此關聯性名稱`FK_Department_Department`至`FK_Department_Person`。)

[![Image82](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image11.png)

按一下**確定**中**資料表和資料行**方塊中，按一下**關閉**中**Foreign Key Relationships**方塊中，並儲存變更。 如果您詢問您是否要儲存`Person`和`Department`表格中，按一下 **是**。

> [!NOTE]
> 如果您已刪除`Person`對應至已在資料的資料列`Administrator`資料行中，您將無法儲存此變更。 在此情況下，使用中的資料表編輯器**伺服器總管**，確定`Administrator`值在每個`Department`資料列都包含實際存在於記錄的識別碼`Person`資料表。
> 
> 儲存變更之後，您將無法刪除資料列從`Person`資料表如果該人員是部門的系統管理員。 在實際執行應用程式中，您會在資料庫條件約束可防止刪除，或您可以指定串聯刪除時提供特定的錯誤訊息。 如需如何指定串聯刪除的範例，請參閱[Entity Framework 和 ASP.NET – 取得啟動第 2 部分](../getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-2.md)。


### <a name="adding-a-view-to-the-database"></a>將檢視加入至資料庫

在新*Departments.aspx*您將建立的頁面上，您想要提供的講師，下拉式清單中 「 上次，第一次 」 的格式名稱，讓使用者可以選取部門系統管理員。 若要讓您更輕鬆地執行此作業，您會在資料庫中建立檢視。 檢視將會包含所需的下拉式清單資料: （正確格式） 的完整名稱和記錄的索引鍵。

在**伺服器總管**，依序展開*School.mdf*，以滑鼠右鍵按一下**檢視**資料夾，然後選取**加入新的檢視**。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image13.png)

按一下**關閉**時**加入資料表** 對話方塊隨即出現，並將下列 SQL 陳述式貼到 SQL 窗格：

[!code-sql[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample1.sql)]

儲存為檢視`vInstructorName`。

### <a name="updating-the-data-model"></a>更新資料模型

在*DAL*資料夾中，開啟*SchoolModel.edmx*檔案，以滑鼠右鍵按一下設計介面，並選取**從資料庫更新模型**。

[![Image07](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image15.png)

在**選擇您的資料庫物件**對話方塊中，選取**新增**索引標籤，然後選取您剛才建立的檢視。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image17.png)

按一下 [ **完成**]。

在設計工具中，您會看到此工具建立`vInstructorName`實體和之間的新關聯`Department`和`Person`實體。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image19.png)

> [!NOTE]
> 在**輸出**和**錯誤清單**windows，您可能會看到警告訊息，通知您此工具會自動建立主要索引鍵新`vInstructorName`檢視。 這是正常的現象。


當您參考新`vInstructorName`程式碼中的實體，您不想使用前面加上小寫"v"給它的資料庫慣例。 因此，您將會重新命名的實體和模型中實體集。

開啟**模型瀏覽器**。 您會看到`vInstructorName`列示為實體類型和檢視。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image21.png)

在下**SchoolModel** (不**SchoolModel.Store**)，以滑鼠右鍵按一下**vInstructorName**選取**屬性**。 在**屬性**視窗中，變更**名稱**屬性設為"InstructorName 」 並變更**實體集名稱**"InstructorNames"的屬性。

[![Image15](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image24.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image23.png)

儲存並關閉的資料模型，然後再重建專案。

## <a name="using-a-repository-class-and-an-objectdatasource-control"></a>使用儲存機制類別和 ObjectDataSource 控制項

建立新的類別檔案中*DAL*資料夾中，其命名*SchoolRepository.cs*，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample2.cs)]

這個程式碼提供單一`GetDepartments`方法會傳回所有的實體`Departments`實體集。 因為您知道您將會存取`Person`每個資料列的導覽屬性傳回，您指定積極式載入該屬性使用`Include`方法。 類別也實作`IDisposable`介面，以確保處置物件時釋放資料庫連接。

> [!NOTE]
> 常見的作法是建立每個實體類型的儲存機制類別。 在此教學課程中，會使用多個實體類型的一個儲存機制類別。 如需儲存機制模式的詳細資訊，請參閱中的文章[Entity Framework 小組部落格](https://blogs.msdn.com/b/adonet/archive/2009/06/16/using-repository-and-unit-of-work-patterns-with-entity-framework-4-0.aspx)和[Julie Lerman 部落格](http://thedatafarm.com/blog/data-access/agile-ef4-repository-part-3-fine-tuning-the-repository/)。


`GetDepartments`方法會傳回`IEnumerable`物件而非`IQueryable`為了確保在儲存機制物件本身已處置後，仍可使用所傳回之集合的物件。 `IQueryable`物件可能會導致資料庫存取權，每當它存取，但是儲存機制物件可能會處置資料繫結控制項嘗試將資料呈現的時間。 您也可以傳回另一個集合型別，例如`IList`物件而非`IEnumerable`物件。 不過，傳回`IEnumerable`物件可確保您可以執行一般的唯讀清單處理工作例如`foreach`迴圈與 LINQ 查詢，但是您無法加入或移除項目在集合中，會是這類變更可能暗示保存至資料庫。

建立*Departments.aspx*使用網頁*Site.Master*主版頁面，並加入下列標記中的`Content`控制項，名為`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample3.aspx)]

這個標記會建立`ObjectDataSource`會使用您剛才建立的儲存機制類別的控制項和`GridView`控制項來顯示資料。 `GridView`控制項指定**編輯**和**刪除**命令，但是您沒有加入尚未支援它們的程式碼。

數個資料行使用`DynamicField`控制項，好讓您可以利用自動的資料格式和驗證功能。 這些工作，您必須呼叫`EnableDynamicData`方法中的`Page_Init`事件處理常式。 (`DynamicControl`控制項不會用於`Administrator`欄位，因為不會使用瀏覽屬性。)

`Vertical-Align="Top"`屬性將會變得重要稍後當您將加入具有巢狀的資料行`GridView`控制項至格線。

開啟*Departments.aspx.cs*檔案，然後加入下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample4.cs)]

然後加入頁面的下列處理常式`Init`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample5.cs)]

在*DAL*資料夾中，建立新的類別檔案命名為*Department.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample6.cs)]

這個程式碼加入至資料模型的中繼資料。 它會指定`Budget`屬性`Department`實體實際上代表貨幣，雖然其資料類型是`Decimal`，其指定的值必須是介於 0 到 $1,000,000.00。 它也會指定`StartDate`屬性的格式應為 mm/dd/yyyy 格式的日期。

執行*Departments.aspx*頁面。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image26.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image25.png)

請注意，雖然您未指定格式字串中的*Departments.aspx*頁面標記**預算**或**開始日期**貨幣與日期的資料行，預設值給它們的套用格式`DynamicField`控制項，使用您在中提供的中繼資料*Department.cs*檔案。

## <a name="adding-insert-and-delete-functionality"></a>將插入和刪除功能

開啟*SchoolRepository.cs*，加入下列程式碼以便建立`Insert`方法和`Delete`方法。 程式碼也包含名為的方法`GenerateDepartmentID`計算下一個可用的記錄金鑰值，以供`Insert`方法。 這是必要的因為資料庫並未設定為會計算此自動為`Department`資料表。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample7.cs)]

### <a name="the-attach-method"></a>Attach 方法

`DeleteDepartment`方法呼叫`Attach`方法，以重新建立所維護的連結，在記憶體中的實體與資料庫之間的物件內容的物件狀態管理員中資料列它代表。 這必須在方法呼叫之前發生`SaveChanges`方法。

詞彙*物件內容*指的是 Entity Framework 類別衍生自`ObjectContext`您用來存取您的實體集及實體類別。 在此專案的程式碼的類別為`SchoolEntities`，和它的執行個體的名稱永遠`context`。 物件內容*物件狀態管理員*是衍生自類別`ObjectStateManager`類別。 物件連絡人使用的物件狀態管理員來儲存實體物件，並與其對應資料表資料列或資料列，在資料庫中的同步處理每個是否追蹤。

當讀取實體時，內容物件將它儲存在物件狀態管理員並追蹤是否與資料庫的同步處理該物件表示。 比方說，如果您變更屬性值，指出您所變更的屬性已不再與資料庫的同步處理會設定旗標。 然後當您呼叫`SaveChanges`物件內容的方法，知道如何執行在資料庫中，因為物件狀態管理員知道到底是實體的目前狀態和資料庫的狀態之間不同。

不過，此程序通常無法運作的 web 應用程式，因為呈現頁面之後，會處置讀取實體，以及其物件狀態管理員中的所有物件內容執行個體。 必須套用變更的物件內容執行個體是一個新的具現化回傳的處理。 如果是`DeleteDepartment`方法，`ObjectDataSource`控制重新將原始版本的實體會為您建立從值在檢視狀態，但這重新建立`Department`實體不存在的物件狀態管理員中。 如果您呼叫`DeleteObject`這個重新建立的實體上的方法，呼叫會失敗，因為物件內容不知道實體是否與資料庫的同步處理。 不過，呼叫`Attach`方法重新建立已原本時自動完成的實體讀取舊版執行個體的物件內容中的資料庫中重新建立的實體之間的值相同的追蹤。

但有些的時候，當您不想要追蹤中的物件狀態管理員中，實體的物件內容，因此您可以設定旗標，以避免這麼做可。 這個範例所示在這一系列之後的教學課程。

### <a name="the-savechanges-method"></a>SaveChanges 方法

這個簡單的儲存機制類別說明如何執行 CRUD 作業的基本原則。 在此範例中，`SaveChanges`每次更新之後立即呼叫方法。 在實際執行的應用程式中，您可能想要呼叫`SaveChanges`從不同的方法，讓您更充分掌控資料庫更新時的方法。 （在下一個教學課程結束時您會看到討論的工作模式，也就是一種方法來協調更新相關的單元技術白皮書的連結）。另請注意，在範例中，`DeleteDepartment`方法不包含處理並行衝突的程式碼; 在這一系列之後的教學課程中將新增程式碼，以執行此作業。

### <a name="retrieving-instructor-names-to-select-when-inserting"></a>擷取講師名稱來選取插入時

使用者必須能夠建立新的部門時，從講師下拉式清單中的清單中選取系統管理員。 因此，加入下列程式碼加入*SchoolRepository.cs*建立一個方法來擷取講師使用您稍早建立的檢視清單：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample8.cs)]

### <a name="creating-a-page-for-inserting-departments"></a>插入部門建立頁面

建立*DepartmentsAdd.aspx*使用網頁*Site.Master*頁面，然後加入下列標記中的`Content`控制項，名為`Content2`:

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample9.aspx)]

這個標記會建立兩個`ObjectDataSource`控制項，一個用於新插入`Department`實體，另一個用於擷取講師名稱`DropDownList`控制項用於選取部門系統管理員。 標記會建立`DetailsView`控制輸入新的部門，並指定控制項的處理常式`ItemInserting`事件，讓您可以設定`Administrator`外部索引鍵值。 在結束時`ValidationSummary`控制項來顯示錯誤訊息。

開啟*DepartmentsAdd.aspx.cs*並加入下列`using`陳述式：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample10.cs)]

新增下列類別變數和方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample11.cs)]

`Page_Init`方法可啟用動態資料功能。 處理常式`DropDownList`控制項的`Init`事件儲存至控制項，以及處理常式的參考`DetailsView`控制項的`Inserting`事件會使用該參考取得`PersonID`選取的講師和更新的值`Administrator`的外部索引鍵屬性`Department`實體。

執行網頁，加入新的部門的資訊，然後按一下**插入**連結。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image28.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image27.png)

輸入新的另一個部門的值。 輸入的數字大於中 1,000,000.00**預算**欄位和索引標籤的下一個欄位。 在欄位中，會顯示星號，如果您在按住滑鼠指標進入此，您可以看到中繼資料中輸入該欄位的錯誤訊息。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image30.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image29.png)

按一下**插入**，並查看所顯示的錯誤訊息`ValidationSummary`在頁面底部的控制項。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image32.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image31.png)

接下來，關閉瀏覽器，然後開啟*Departments.aspx*頁面。 將刪除功能加入*Departments.aspx*頁面加入`DeleteMethod`屬性`ObjectDataSource`控制項，和`DataKeyNames`屬性`GridView`控制項。 這些控制項的開頭標記現在會類似下列的範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample12.aspx)]

執行網頁。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image34.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image33.png)

刪除當您執行您加入的部門*DepartmentsAdd.aspx*頁面。

## <a name="adding-update-functionality"></a>加入更新的功能

開啟*SchoolRepository.cs*並加入下列`Update`方法：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample13.cs)]

當您按一下**更新**中*Departments.aspx*  頁面上，`ObjectDataSource`控制項建立兩個`Department`要傳遞至實體`UpdateDepartment`方法。 其中一個包含已儲存檢視狀態中的原始值和另一個包含輸入的新值`GridView`控制項。 中的程式碼`UpdateDepartment`方法傳遞`Department`實體，其具有原始值，以`Attach`方法，以建立實體和功能的資料庫之間的追蹤。 然後在程式碼通過`Department`實體具有新值來`ApplyCurrentValues`方法。 物件內容比較舊的和新值。 如果新的值是舊的值不同，屬性值變更物件內容。 `SaveChanges`方法，然後更新只變更資料行在資料庫中的。 （不過，如果此實體的更新函式已對應到預存程序中，整個資料列會更新資料行變更無論哪個。）

開啟*Departments.aspx*檔案，然後加入下列屬性來`DepartmentsObjectDataSource`控制項：

- `UpdateMethod="UpdateDepartment"`
- `ConflictDetection="CompareAllValues"`   
 是這個原因舊值儲存在檢視狀態，以便與中的新值進行比較`Update`方法。
- `OldValuesParameterFormatString="orig{0}"`   
 這會通知控制項，原始值參數的名稱是`origDepartment`。

開頭標記的標記`ObjectDataSource`控制項現在會類似下列的範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample14.aspx)]

新增`OnRowUpdating="DepartmentsGridView_RowUpdating"`屬性`GridView`控制項。 您將使用此設定`Administrator`屬性值會根據使用者選取下拉式清單中的資料列。 `GridView`現在開頭標記類似下列範例：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample15.aspx)]

新增`EditItemTemplate`控制`Administrator`欄`GridView`之後立即控制`ItemTemplate`控制該資料行：

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample16.aspx)]

這`EditItemTemplate`控制項是類似於`InsertItemTemplate`控制*DepartmentsAdd.aspx*頁面。 兩者的差異在於使用設定控制項的初始值`SelectedValue`屬性。

之前`GridView`控制項，加入`ValidationSummary`控制如同*DepartmentsAdd.aspx*頁面。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample17.aspx)]

開啟*Departments.aspx.cs*並立即在部分類別宣告之後，加入下列程式碼建立私用欄位來參考`DropDownList`控制項：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample18.cs)]

然後加入處理常式`DropDownList`控制項的`Init`事件和`GridView`控制項的`RowUpdating`事件：

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/samples/sample19.cs)]

處理常式`Init`事件會將儲存的參考`DropDownList`[類別] 欄位中的控制項。 處理常式`RowUpdating`事件來取得使用者輸入的值，並套用至使用參考`Administrator`屬性`Department`實體。

使用*DepartmentsAdd.aspx*頁面，即可加入新的部門，然後再執行*Departments.aspx*頁面上，按一下 **編輯**上您所加入的資料列。

> [!NOTE]
> 不能編輯未加入的資料列 (亦即，均已存在於資料庫)，因為資料庫中的資料無效使用資料庫所建立的資料列的系統管理員是學生。 如果您嘗試要編輯其中一個，就會報告類似錯誤的錯誤頁面`'InstructorsDropDownList' has a SelectedValue which is invalid because it does not exist in the list of items.`


[![Image10](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image36.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image35.png)

如果您輸入無效**預算**金額，然後按一下 **更新**，您會看到相同的星號和錯誤訊息中的*Departments.aspx*頁面。

變更欄位值或選取不同的系統管理員，然後按一下 **更新**。 變更已顯示。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image38.png)](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started/_static/image37.png)

如此即完成簡介使用`ObjectDataSource`控制基本 crud （建立、 讀取、 更新、 刪除） 作業與 Entity Framework。 您建立簡單的多層式架構應用程式，但是商務邏輯層仍緊密繫結到資料存取層，這讓事情更加自動化的單元測試。 下列教學課程中，您會看到如何實作以便利單元測試儲存機制模式。

>[!div class="step-by-step"]
[下一步](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests.md)
