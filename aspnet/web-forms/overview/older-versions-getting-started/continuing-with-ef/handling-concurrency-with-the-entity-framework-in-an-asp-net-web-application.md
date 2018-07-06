---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: 處理使用 Entity Framework 4.0 ASP.NET 4 Web 應用程式中的並行 |Microsoft Docs
author: tdykstra
description: 本教學課程系列由開始使用 Entity Framework 4.0 的教學課程系列的 Contoso 大學 web 應用程式為基礎。 我...
ms.author: aspnetcontent
ms.date: 01/26/2011
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: 52c4dbdd5cf2919d0a04a883c1cd48cb56dd4e94
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804542"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>使用 Entity Framework 4.0 ASP.NET 4 Web 應用程式中處理並行
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

> 本教學課程系列是根據所建立的 Contoso 大學 web 應用程式[Getting Started with Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，為本教學課程的起始點即可[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)，它由完整的教學課程系列。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


您在上一個教學課程中學會如何排序和篩選資料使用`ObjectDataSource`控制項和 Entity Framework。 本教學課程會示範處理中使用 Entity Framework 的 ASP.NET web 應用程式的並行存取的選項。 您將建立新的網頁，專門用來更新講師的辦公室指派。 您將會處理在該頁面，並在您稍早建立的 [部門] 頁面的並行處理問題。

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>並行衝突

當一位使用者編輯一筆記錄，並另一位使用者第一位使用者的變更寫入到資料庫前，請編輯同一筆記錄時，就會發生並行衝突。 如果您沒有設定 Entity Framework 來偵測這類衝突，只要更新資料庫最後會覆寫其他使用者的變更。 在許多應用程式，此風險是可接受的而您不需要設定應用程式，以處理可能的並行存取衝突。 (如果有幾個使用者或幾項更新，或如果不是真的很重要的某些變更會覆寫時，如果並行的程式設計成本可能會超過好處。)如果您不需要擔心並行衝突，您可以略過本教學課程中;其餘的兩個教學課程系列中不需依賴您在此影片中所建立的任何項目。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 （鎖定）

若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。 這就叫做*封閉式並行存取*。 例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。 若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。 若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。

管理鎖定有一些缺點。 其程式可能相當複雜。 它需要大量的資料庫管理資源，以及它會導致效能問題的應用程式的使用者數目會增加 （亦即，將無法妥善調整）。 基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。 Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取的替代方案是*開放式並行存取*。 開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。 比方說，John 執行*Department.aspx*頁面上，按下**編輯**連結記錄部門，並減少**預算**$1,000,000.00 $ 的數量125,000.00。 （John 管理競爭的部門的而且想要釋出為自己的部門）。

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John 按之前**更新**，Jane 執行相同的頁面，按一下**編輯**連結記錄部門，然後變更**Start Date**從 2011 年 1 月 10 日欄位設為 1/1 /1999。 （Jane 管理歷程記錄部門的而且想要為它提供更多的 seniority）。

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John 按**更新**第一次，然後 Jane 按一下**更新**。 Jane 的瀏覽器目前清單**預算**量 $1,000,000.00，但這不正確，因為量已由 John 變更 $125,000.00。

在此案例中，您可以採取的動作包括下列：

- 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。 在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。 在下一次有人瀏覽歷程記錄部門，他們會看到 1/1/1999年和 $125,000.00。 

    這是 Entity Framework 中的預設行為，它可以大幅減少可能會導致資料遺失的衝突數目。 不過，此行為不會避免資料遺失，如果遭到變更的實體相同的屬性。 此外，此行為不一定可行的;當您將預存程序對應至的實體類型時，所有實體的屬性會更新任何實體變更時在資料庫中。
- 您可以讓 Jane 的變更覆寫 John 所作出的變更。 Jane 按一下後**更新**，則**預算**數量會回到 $1,000,000.00。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 （用戶端的值優先於功能的資料存放區。）
- 您可以防止資料庫中的更新 Jane 的變更。 一般而言，您會顯示錯誤訊息、 她的目前狀態的資料，並允許她在重新輸入她的變更，如果她仍然想要讓它們。 您可以進一步自動化程序，儲存其輸入，並提供她有機會重新套用它，而不必重新輸入。 這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

在 Entity Framework 中，您可以解決衝突，藉由處理`OptimisticConcurrencyException`Entity Framework 擲回的例外狀況。 若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須適當的設定資料庫及資料模型。 一部分啟用衝突偵測的選項包括下列選項：

- 在資料庫中，包含可用來判斷何時已變更的資料列的資料表資料行。 接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。

    這是目的`Timestamp`中的資料行`OfficeAssignment`資料表。

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    資料類型`Timestamp`資料行也稱為`Timestamp`。 不過，資料行實際上並不包含日期或時間值。 相反地，值會是一個循序號碼，都會遞增每個資料列更新的時間。 在 `Update`或是`Delete`命令，`Where`子句會包含原始`Timestamp`值。 如果要更新的資料列已由另一位使用者中的值變更`Timestamp`不同於原始值，因此`Where`子句會傳回任何資料列更新。 當 Entity Framework 會尋找目前尚未更新任何資料列`Update`或`Delete`命令 （也就是當受影響的資料列數目為零），它會解譯為並行衝突。
- 設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。

    第一個選項，如果資料列中的任何項目已變更因為這是第一個讀取的資料列，如同`Where`子句不會可解譯為並行衝突的 Entity Framework 傳回資料列來更新。 這個方法是，更有效率的方式使用`Timestamp`欄位，但可能會沒有效率。 針對資料庫有許多資料行的資料表，它可能會導致非常大`Where`子句，而且 web 應用程式中它可能需要您維持大量的狀態。 維持大量的狀態可能會影響應用程式效能，因為它需要伺服器資源 （例如，工作階段狀態），或必須包含在網頁中 （例如，檢視狀態）。

在本教學課程中，您會將錯誤處理開放式並行存取衝突的實體又沒有追蹤屬性 (`Department`實體) 並沒有追蹤屬性的實體 (`OfficeAssignment`實體)。

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>處理開放式並行存取，而且沒有追蹤屬性

若要實作的開放式並行存取`Department`沒有追蹤的實體 (`Timestamp`) 屬性，您將完成下列工作：

- 變更資料模型，以啟用追蹤的並行存取`Department`實體。
- 在 `SchoolRepository`類別，控制代碼中的並行存取例外狀況`SaveChanges`方法。
- 在  *Departments.aspx*頁面上，顯示訊息警告不成功的嘗試的變更使用者的處理並行例外狀況。 然後使用者可以看到目前的值，並仍然需要它們時的重試，一次所做的變更。

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>啟用追蹤的資料模型中的並行存取

在 Visual Studio 中，開啟您已在此系列的上一個教學課程中使用的 Contoso 大學 web 應用程式。

開啟*SchoolModel.edmx*，在資料模型設計工具中，以滑鼠右鍵按一下`Name`中的屬性`Department`實體，然後按一下**屬性**。 在 **屬性**視窗中，變更`ConcurrencyMode`屬性設`Fixed`。

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

這麼做的其他非主要金鑰純量屬性 (`Budget`， `StartDate`，和`Administrator`。)（您無法執行此導覽屬性。）這會指定，每當 Entity Framework 會產生`Update`或是`Delete`SQL 命令來更新`Department`資料庫中的實體，這些值的資料行 （原始） 必須包含在`Where`子句。 如果不找到任何資料列何時`Update`或`Delete`命令執行時，Entity Framework 將會擲回開放式並行存取例外狀況。

儲存並關閉資料模型。

### <a name="handling-concurrency-exceptions-in-the-dal"></a>DAL 中處理並行例外狀況

開啟*SchoolRepository.cs*並新增下列`using`陳述式`System.Data`命名空間：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

新增下列新`SaveChanges`方法，用來處理開放式並行存取例外狀況：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

如果呼叫這個方法時，就會發生並行錯誤，就會在記憶體中實體的屬性值取代目前資料庫中的值。 並行存取例外狀況重新擲回，好讓網頁可以處理它。

在 `DeleteDepartment`並`UpdateDepartment`方法，會取代現有的呼叫`context.SaveChanges()`藉由呼叫`SaveChanges()`才能叫用新的方法。

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>展示層中的並行存取例外狀況處理

開啟*Departments.aspx*並加入`OnDeleted="DepartmentsObjectDataSource_Deleted"`屬性設定為`DepartmentsObjectDataSource`控制項。 控制項的開頭標記現在會類似下列的範例。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

在 `DepartmentsGridView`控制項，指定所有的資料表資料行`DataKeyNames`屬性，如下列範例所示。 請注意，這會建立非常大的檢視狀態欄位，這是其中一個原因為何使用追蹤欄位通常是較好的方法追蹤並行存取衝突。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

開啟*Departments.aspx.cs*並新增下列`using`陳述式`System.Data`命名空間：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

新增下列新方法，從資料來源控制項，您會呼叫`Updated`和`Deleted`處理並行例外狀況的事件處理常式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

此程式碼會檢查例外狀況類型，以及如果是並行例外狀況，程式碼以動態方式建立`CustomValidator`接著會顯示在訊息的控制項`ValidationSummary`控制項。

呼叫新方法，從`Updated`您先前加入的事件處理常式。 此外，建立 新`Deleted`呼叫相同方法 （但不會執行任何其他項目） 的事件處理常式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>在 [部門] 頁面測試開放式並行存取

執行*Departments.aspx*頁面。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

按一下 **編輯**資料列中，並在值變更**預算**資料行。 (請記住，您可以只編輯記錄，您已建立本教學課程中，因為現有`School`資料庫記錄包含一些無效的資料。 經濟效益部門的記錄是安全地試驗）。

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

開啟新的瀏覽器視窗並執行頁面一次 （複製的 URL 從第一個瀏覽器視窗的 [網址] 方塊的第二個瀏覽器視窗）。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

按一下 **編輯**在同一個資料列您稍早編輯並變更**預算**為不同的值。

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

在第二個瀏覽器視窗中，按一下**更新**。 **預算**量已成功變更為這個新值。

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

在第一個瀏覽器視窗中，按一下**更新**。 更新會失敗。 **預算**數量會重新顯示，使用您在第二個瀏覽器視窗中，設定的值，您會看到一則錯誤訊息。

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>使用 追蹤屬性處理開放式並行存取

若要處理的追蹤屬性的實體的開放式並行存取，您將完成下列工作：

- 將預存程序新增至資料模型，來管理`OfficeAssignment`實體。 （沒有同時使用 追蹤屬性和預存程序，它們只群組在一起此處為了說明）。
- CRUD 方法加入 DAL 和的 BLL`OfficeAssignment`實體，包括程式碼來處理 DAL 中的開放式並行存取例外狀況。
- 建立 office 指派網頁。
- 測試新的 web 網頁中開放式並行存取。

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>新增 OfficeAssignment 預存程序的資料模型

開啟*SchoolModel.edmx*檔案在模型設計師中，以滑鼠右鍵按一下設計介面上，按一下 **從資料庫更新模型**。 在**新增**索引標籤**選擇您的資料庫物件**對話方塊方塊中，展開**預存程序**，然後選取三個`OfficeAssignment`預存程序 (請參閱下列螢幕擷取畫面），然後按一下**完成**。 （這些預存程序均已存在於資料庫當您下載或使用指令碼加以建立。）

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

以滑鼠右鍵按一下`OfficeAssignment`實體，然後選取**預存程序對應**。

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

設定**插入**，**更新**，並**刪除**函式來使用其對應預存程序。 針對`OrigTimestamp`的參數`Update`函式中，設定**屬性**來`Timestamp`，然後選取**使用原始值**選項。

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

當 Entity Framework 會呼叫`UpdateOfficeAssignment`預存程序，它會將傳遞的原始值`Timestamp`中的資料行`OrigTimestamp`參數。 預存程序會使用此參數以其`Where`子句：

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

預存程序也會選取新的值`Timestamp`更新後的資料行，以便可以讓 Entity Framework`OfficeAssignment`會在記憶體中與對應的資料庫資料列的同步處理的實體。

(請注意，刪除辦公室指派的預存程序沒有`OrigTimestamp`參數。 因此，Entity Framework 無法確認實體不變，然後再刪除它。）

儲存並關閉資料模型。

### <a name="adding-officeassignment-methods-to-the-dal"></a>將 OfficeAssignment 方法加入至 DAL

開啟*ISchoolRepository.cs*並加入下列的 CRUD 方法`OfficeAssignment`實體集：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

加入下列新方法，來*SchoolRepository.cs*。 在 `UpdateOfficeAssignment`方法，呼叫本機`SaveChanges`方法，而非`context.SaveChanges`。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

在測試專案中，開啟*MockSchoolRepository.cs*並新增下列`OfficeAssignment`集合和它的 CRUD 方法。 （模擬儲存機制必須實作儲存機制的介面，或解決方案不會編譯）。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>將 OfficeAssignment 方法加入至 BLL

在主專案中，開啟*SchoolBL.cs*並加入下列的 CRUD 方法`OfficeAssignment`給它的實體集：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>建立 OfficeAssignments Web 網頁

建立會使用新網頁*Site.Master*主版頁面，然後將它命名*OfficeAssignments.aspx*。 新增下列標記，即可`Content`控制項，名為`Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

請注意，在`DataKeyNames`屬性，指定標記`Timestamp`屬性，以及記錄的索引鍵 (`InstructorID`)。 指定屬性中的`DataKeyNames`屬性使控制項將它們儲存在控制項的狀態 （也就是類似於檢視狀態），以讓回傳的處理期間會使用原始值。

如果您未儲存`Timestamp`值，Entity Framework 不會有它`Where`子句的 sql`Update`命令。 因此不會找到更新。 如此一來，Entity Framework 會擲回例外狀況開放式並行存取每次`OfficeAssignment`更新實體。

開啟*OfficeAssignments.aspx.cs*並新增下列`using`資料存取層的陳述式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

新增下列`Page_Init`方法，可讓動態資料功能。 也將新增的下列處理常式`ObjectDataSource`控制項的`Updated`事件，檢查是否有並行存取錯誤：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>測試 [OfficeAssignments] 頁面中的開放式並行存取

執行*OfficeAssignments.aspx*頁面。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

按一下 **編輯**資料列中，並在值變更**位置**資料行。

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

開啟新的瀏覽器視窗並執行頁面一次 （複製的 URL 從第一個瀏覽器視窗的第二個瀏覽器視窗）。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

按一下 **編輯**在同一個資料列您稍早編輯並變更**位置**為不同的值。

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

在第二個瀏覽器視窗中，按一下**更新**。

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

切換的第一個瀏覽器視窗，然後按一下**更新**。

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

您會看到一則錯誤訊息和**位置**值已更新為您在第二個瀏覽器視窗中，變更才會顯示值。

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>處理並行與 EntityDataSource 控制項

`EntityDataSource`控制項包含可辨識資料模型中的並行設定的內建邏輯和處理更新，並據此刪除作業。 不過，如同所有的例外狀況，您必須處理`OptimisticConcurrencyException`例外狀況自行以提供使用者易記錯誤訊息。

接下來，您會設定*Courses.aspx*網頁 (使用`EntityDataSource`控制項) 允許更新和刪除作業，並顯示錯誤訊息，如果發生並行衝突。 `Course`實體沒有追蹤的資料行，所以您將使用相同的方法與並行`Department`實體： 追蹤所有的非索引鍵屬性的值。

開啟*SchoolModel.edmx*檔案。 非索引鍵屬性的`Course`實體 (`Title`， `Credits`，以及`DepartmentID`)，將**並行模式**屬性設`Fixed`。 然後儲存並關閉資料模型。

開啟*Courses.aspx*頁面並進行下列變更：

- 在 `CoursesEntityDataSource`控制項，新增`EnableUpdate="true"`和`EnableDelete="true"`屬性。 該控制項的開頭標記現在會類似下列範例：

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- 在 `CoursesGridView`控制項，變更`DataKeyNames`屬性值來`"CourseID,Title,Credits,DepartmentID"`。 然後新增`CommandField`項目`Columns`顯示的項目**編輯**並**刪除**按鈕 (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`)。 `GridView`控制項現在會類似下列範例：

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

執行網頁並建立衝突情況，如同您先前在 [部門] 頁面中。 在兩個瀏覽器視窗中執行的頁面中，按一下**編輯**在同一個列在每個視窗中，並在每個不同的變更。 按一下 **更新**在一個視窗，然後按一下**更新**在其他視窗中。 當您按一下 **更新**第二個階段中，您會看到錯誤頁面所產生的未處理的並行存取例外狀況。

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

處理此錯誤，非常類似於您對它所做的處理方式`ObjectDataSource`控制項。 開啟*Courses.aspx*頁面上，然後在`CoursesEntityDataSource`控制項，指定的處理常式`Deleted`和`Updated`事件。 控制項的開頭標記現在會類似下列範例：

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

再`CoursesGridView`控制項，新增下列`ValidationSummary`控制項：

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

在  *Courses.aspx.cs*，新增`using`陳述式`System.Data`命名空間新增一個方法，檢查並行存取例外狀況，以及加入處理常式`EntityDataSource`控制項的`Updated`和`Deleted`處理常式。 程式碼看起來如下所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

這個程式碼，您對唯一的差別`ObjectDataSource`控制項，不在此情況下的並行存取例外狀況都是`Exception`屬性的事件引數物件中，而不是在該例外狀況`InnerException`屬性。

執行頁面，然後重新建立並行衝突。 此時您會看到一則錯誤訊息：

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

如此即完成了處理並行衝突的簡介。 下一個教學課程將提供有關如何在使用 Entity Framework 的 web 應用程式中改善效能的指引。

> [!div class="step-by-step"]
> [上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [下一頁](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
