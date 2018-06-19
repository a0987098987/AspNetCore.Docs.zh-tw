---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
title: 處理與 Entity Framework 4.0 ASP.NET 4 Web 應用程式中的並行 |Microsoft 文件
author: tdykstra
description: 此教學課程系列為基礎所建立的開始使用 Entity Framework 4.0 教學課程系列的 Contoso 大學 web 應用程式。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: a5aa22a6-fb7f-4f41-9c7f-addda151940b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application
msc.type: authoredcontent
ms.openlocfilehash: f40695270006e4f8b0c9ad8e94049e5239f06e63
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889858"
---
<a name="handling-concurrency-with-the-entity-framework-40-in-an-aspnet-4-web-application"></a>處理並行與 Entity Framework 4.0 ASP.NET 4 Web 應用程式
====================
由[Tom Dykstra](https://github.com/tdykstra)

> 此教學課程系列為基礎所建立的 Contoso 大學 web 應用程式[開始使用 Entity Framework 4.0](https://asp.net/entity-framework/tutorials#Getting%20Started)教學課程系列。 如果您未完成先前的教學課程，您可以身分本教學課程的起始點來[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)您會建立。 您也可以[下載應用程式](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完整的教學課程所建立。 如果您有疑問的教學課程，您可以張貼他們[ASP.NET Entity Framework 論壇](https://forums.asp.net/1227.aspx)。


您在上一個教學課程中學會如何排序和篩選資料使用`ObjectDataSource`控制項和 Entity Framework。 本教學課程示範處理中使用 Entity Framework 的 ASP.NET web 應用程式的並行存取的選項。 您將建立新的網頁，專門用來更新講師 office 指派。 您將會處理在該頁面，然後在您稍早建立的 [部門] 頁面的並行問題。

[![Image06](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image2.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image1.png)

[![Image01](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image4.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image3.png)

## <a name="concurrency-conflicts"></a>並行衝突

當一位使用者編輯資料錄和另一位使用者編輯同一筆記錄，第一個使用者的變更寫入資料庫之前，就會發生並行衝突。 如果您不要設定 Entity Framework 來偵測這類衝突，只要更新資料庫最後會覆寫其他使用者的變更。 在許多應用程式，這項風險是可接受的而您不需要設定應用程式處理可能的並行存取衝突。 (如果有少數的使用者或一些更新，或如果不是最關鍵的某些變更會覆寫時，如果並行程式設計的成本可能會超過的好處。)如果您不需要擔心並行衝突，您可以略過本教學課程。數列中其餘的兩個教學課程不取決於您在此一中建立的任何項目。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 （鎖定）

若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。 這稱為*封閉式並行存取*。 例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。 若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。 若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。

管理鎖定也有一些缺點。 其程式可能相當複雜。 這需要大量的資料庫管理資源，而且它會造成效能問題的應用程式的使用者數目增加時 （亦即，將無法妥善調整）。 基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。 Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取另一種是*開放式並行存取*。 開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。 John 會執行，例如*Department.aspx*頁面上，按一下**編輯**連結的歷程記錄 」 部門，並減少**預算**1,000,000.00 $ $ 的數量125,000.00。 （John 管理競爭部門的而且想要釋出其部門的）。

[![Image07](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image6.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image5.png)

John 按之前**更新**，Jane 執行相同的頁面上，按一下**編輯**記錄部門，然後變更連結**開始日期**從 2011 年 1 月 10 日欄位設為 1/1 /1999。 （Jane 管理記錄部門的而且想要為它提供更多的 seniority）。

[![Image08](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image8.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image7.png)

John 按**更新**第一次，然後 Jane 按一下**更新**。 Jane 的瀏覽器現在清單**預算**做 $1,000,000.00，但是這樣的數量不正確，因為量已由 John 變更 $125,000.00。

在此案例中，您可以採取的動作包括下列：

- 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。 在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。 在下一次有人瀏覽歷程記錄部門，他們會看到 1/1/1999年和 $125,000.00。 

    這是 Entity Framework 中的預設行為，而且它可以大幅減少可能會導致資料遺失的衝突數目。 不過，此行為不會避免資料遺失，如果相同的實體屬性進行競爭的變更。 此外，這種行為並不一定都可能;當您為實體型別對應預存程序，所有實體的屬性會更新任何實體變更時在資料庫中。
- 您可以讓 Jane 的變更覆寫 John 的變更。 Jane 按一下之後**更新**、**預算**量會回到 $1,000,000.00。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 （用戶端的值優先於什麼是資料存放區中。）
- 您可以防止資料庫中的更新 Jane 的變更。 一般而言，您會顯示錯誤訊息、 顯示她目前狀態的資料，並允許她如果她仍然想要讓它們重新輸入其變更。 您無法進一步自動化程序，透過儲存其輸入，並讓她有機會重新套用而不必重新輸入。 這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

在 Entity Framework 中，您可以藉由處理解決衝突`OptimisticConcurrencyException`Entity Framework 就會擲回的例外狀況。 若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須適當的設定資料庫及資料模型。 一部分啟用衝突偵測的選項包括下列選項：

- 在資料庫中，包含可用來判斷已變更的資料列的資料表資料行。 接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。

    目的，是`Timestamp`中的資料行`OfficeAssignment`資料表。

    [![Image09](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image10.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image9.png)

    資料型別`Timestamp`資料行也稱為`Timestamp`。 不過，資料行實際上並不包含日期或時間值。 相反地，值會是已遞增每次更新資料列時的序號。 在`Update`或`Delete`命令，`Where`子句會包含原始`Timestamp`值。 如果正在更新的資料列已由另一位使用者中的值變更`Timestamp`不同於原始值，所以`Where`子句會傳回沒有要更新的資料列。 Entity Framework 找到任何資料列，已由目前已更新`Update`或`Delete`命令 （亦即，受影響的資料列數目為零） 時，它將會解譯為並行衝突。
- 設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。

    如同第一個選項之後第一次讀取的資料列，, 資料列中的任何項目已變更`Where`子句不會 Entity Framework 會解譯為並行衝突的傳回資料列來更新。 這個方法是使用效率`Timestamp`欄位，但可能會沒有效率。 對於具有許多資料行的資料庫資料表，可能會產生非常大`Where`子句，並在 web 應用程式可以要求您維護大量的狀態。 維護大量的狀態會影響應用程式效能，因為它需要的伺服器資源 （例如，工作階段狀態），或必須包含在網頁本身 （例如，檢視狀態）。

在本教學課程中，您會加入錯誤處理開放式並行存取衝突的實體沒有追蹤屬性 (`Department`實體) 和實體沒有追蹤屬性 (`OfficeAssignment`實體)。

## <a name="handling-optimistic-concurrency-without-a-tracking-property"></a>處理開放式並行存取沒有追蹤內容

若要實作的開放式並行存取`Department`沒有追蹤的實體 (`Timestamp`) 屬性，您將完成下列工作：

- 變更資料模型，以啟用追蹤的並行`Department`實體。
- 在`SchoolRepository`類別，控制代碼中的並行存取例外狀況`SaveChanges`方法。
- 在*Departments.aspx*頁面上，顯示訊息警告不成功的嘗試的變更使用者的控制代碼並行存取例外狀況。 使用者可以查看目前的值，然後重試所做的變更，如果仍然需要它們時。

### <a name="enabling-concurrency-tracking-in-the-data-model"></a>啟用追蹤資料模型中的並行存取

在 Visual Studio 中，開啟您已在此系列的上一個教學課程中使用的 Contoso 大學 web 應用程式。

開啟*SchoolModel.edmx*，在資料模型設計師中，以滑鼠右鍵按一下`Name`屬性`Department`實體，然後按一下**屬性**。 在**屬性**視窗中，變更`ConcurrencyMode`屬性`Fixed`。

[![Image16](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image12.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image11.png)

執行其他非主要金鑰純量屬性相同的動作 (`Budget`， `StartDate`，和`Administrator`。)（您無法這樣做為導覽屬性。）這會指定，每當 Entity Framework 產生`Update`或`Delete`SQL 命令，以更新`Department`資料庫中的實體，這些值的資料行 （原始） 必須包含在`Where`子句。 如果不找到任何資料列時`Update`或`Delete`命令執行時，Entity Framework 將會擲回開放式並行存取例外狀況。

儲存並關閉 資料模型。

### <a name="handling-concurrency-exceptions-in-the-dal"></a>在 DAL 的並行存取例外狀況處理

開啟*SchoolRepository.cs*並加入下列`using`陳述式`System.Data`命名空間：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample1.cs)]

新增下列新`SaveChanges`方法，這個方法會處理開放式並行存取例外狀況：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample2.cs)]

如果並行存取錯誤發生時呼叫這個方法，在記憶體中實體的屬性值會取代目前資料庫中的值。 並行存取例外狀況重新擲回，好讓網頁可以處理它。

在`DeleteDepartment`和`UpdateDepartment`方法，會取代現有呼叫`context.SaveChanges()`呼叫`SaveChanges()`才能叫用新的方法。

### <a name="handling-concurrency-exceptions-in-the-presentation-layer"></a>展示層中的並行存取例外狀況處理

開啟*Departments.aspx*並加入`OnDeleted="DepartmentsObjectDataSource_Deleted"`屬性`DepartmentsObjectDataSource`控制項。 控制項的開頭標記現在會類似下列的範例。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample3.aspx)]

在`DepartmentsGridView`控制項，指定所有的資料表資料行`DataKeyNames`屬性，如下列範例所示。 請注意，這會建立非常大的檢視狀態欄位，這是其中一個原因為何使用追蹤欄位通常是慣用的方法來追蹤並行衝突。

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample4.aspx)]

開啟*Departments.aspx.cs*並加入下列`using`陳述式`System.Data`命名空間：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample5.cs)]

將下列新方法，您會從資料來源控制項的呼叫`Updated`和`Deleted`處理並行例外狀況的事件處理常式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample6.cs)]

此程式碼會檢查例外狀況型別和並行存取例外狀況，所以如果程式碼以動態方式建立`CustomValidator`接著會顯示在訊息的控制項`ValidationSummary`控制項。

呼叫的新方法，從`Updated`您先前加入的事件處理常式。 此外，建立新`Deleted`呼叫相同方法 （但不執行任何其他動作） 的事件處理常式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample7.cs)]

### <a name="testing-optimistic-concurrency-in-the-departments-page"></a>在 [部門] 頁面中測試開放式並行存取

執行*Departments.aspx*頁面。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image14.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image13.png)

按一下**編輯**中一個資料列中的值變更和**預算**資料行。 (請記住，您可以只進行編輯記錄，您已建立本教學課程中，因為現有`School`資料庫的記錄包含某些無效的資料。 經濟部門的記錄是安全的一個試驗。）

[![Image18](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image16.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image15.png)

開啟新的瀏覽器視窗並執行資料頁 （複製的 URL 從第一個瀏覽器視窗的 [網址] 方塊的第二個瀏覽器視窗）。

[![Image17](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image18.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image17.png)

按一下**編輯**在同一個資料列您稍早編輯並變更**預算**為其他值。

[![Image19](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image20.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image19.png)

在第二個瀏覽器視窗中，按一下 **更新**。 **預算**數量已成功變更為這個新值。

[![Image20](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image22.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image21.png)

在第一個瀏覽器視窗中，按一下 **更新**。 更新失敗。 **預算**量會重新顯示，使用您在第二個瀏覽器視窗中，設定的值，您看到錯誤訊息。

[![Image21](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image24.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image23.png)

## <a name="handling-optimistic-concurrency-using-a-tracking-property"></a>使用 追蹤屬性處理開放式並行存取

若要處理的追蹤屬性的實體的開放式並行存取，您將完成下列工作：

- 將預存程序加入至資料模型，以管理`OfficeAssignment`實體。 （追蹤屬性和預存程序，不需要同時使用; 它們會只分組在一起這裡舉例來說）。
- CRUD 方法加入 DAL 和如 BLL`OfficeAssignment`實體，包括處理中 DAL 的開放式並行存取例外狀況的程式碼。
- 建立 office 指派網頁。
- 測試新的網頁中開放式並行存取。

### <a name="adding-officeassignment-stored-procedures-to-the-data-model"></a>加入 OfficeAssignment 預存程序的資料模型

開啟*SchoolModel.edmx*檔案在模型設計師中，以滑鼠右鍵按一下設計介面，然後按一下**從資料庫更新模型**。 在**新增** 索引標籤**選擇您的資料庫物件**對話方塊方塊中，展開 **預存程序**並選取三個`OfficeAssignment`預存程序 (請參閱下列螢幕擷取畫面），然後按一下 **完成**。 （這些預存程序均已在資料庫中下載，或使用指令碼在建立時。）

[![Image02](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image26.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image25.png)

以滑鼠右鍵按一下`OfficeAssignment`實體，然後選取**預存程序對應**。

[![Image03](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image28.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image27.png)

設定**插入**，**更新**，和**刪除**使用其對應之函數的預存程序。 如`OrigTimestamp`參數`Update`函式中，設定**屬性**至`Timestamp`選取**使用原始值**選項。

[![Image04](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image30.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image29.png)

當 Entity Framework 呼叫`UpdateOfficeAssignment`預存程序，它將傳遞的原始值`Timestamp`中的資料行`OrigTimestamp`參數。 預存程序會使用此參數以其`Where`子句：

[!code-sql[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample8.sql)]

預存程序也會選取的新值`Timestamp`更新後的資料行，以便可以讓 Entity Framework`OfficeAssignment`為記憶體中與對應的資料庫資料列的同步處理的實體。

(請注意，刪除 office 作業的預存程序沒有`OrigTimestamp`參數。 因此，Entity Framework 無法驗證實體不變，然後再刪除它。）

儲存並關閉 資料模型。

### <a name="adding-officeassignment-methods-to-the-dal"></a>Dal 加入 OfficeAssignment 方法

開啟*ISchoolRepository.cs*並加入下列的 CRUD 方法`OfficeAssignment`實體集：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample9.cs)]

將下列新方法加入*SchoolRepository.cs*。 在`UpdateOfficeAssignment`方法，呼叫本機`SaveChanges`方法，而非`context.SaveChanges`。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample10.cs)]

在測試專案中，開啟*MockSchoolRepository.cs*並加入下列`OfficeAssignment`集合和 CRUD 方法。 （模擬儲存機制必須實作儲存機制的介面，或將不會編譯方案）。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample11.cs)]

### <a name="adding-officeassignment-methods-to-the-bll"></a>加入 BLL OfficeAssignment 方法

在主專案中，開啟*SchoolBL.cs*並加入下列的 CRUD 方法`OfficeAssignment`實體設：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample12.cs)]

## <a name="creating-an-officeassignments-web-page"></a>建立 OfficeAssignments Web 網頁

建立新的 web 網頁會使用*Site.Master*主版頁面並將其命名*OfficeAssignments.aspx*。 加入下列標記以`Content`控制項，名為`Content2`:

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample13.aspx)]

請注意，在`DataKeyNames`屬性，指定標記`Timestamp`屬性，以及記錄索引鍵 (`InstructorID`)。 指定屬性中的`DataKeyNames`屬性使控制項將它們儲存在控制項的狀態 （這是類似於檢視狀態），讓回傳的處理期間有可用的原始值。

如果沒有儲存`Timestamp`值，Entity Framework 不會有它的`Where`子句的 SQL`Update`命令。 因此不會找到更新。 如此一來，Entity Framework 會擲回例外狀況的開放式並行存取每次`OfficeAssignment`在更新的實體。

開啟*OfficeAssignments.aspx.cs*並加入下列`using`資料存取層的陳述式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample14.cs)]

加入下列`Page_Init`方法，可啟用動態資料功能。 也加入下列的處理常式，如`ObjectDataSource`控制項的`Updated`事件，若要檢查並行存取錯誤：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample15.cs)]

### <a name="testing-optimistic-concurrency-in-the-officeassignments-page"></a>在 OfficeAssignments 頁面中測試開放式並行存取

執行*OfficeAssignments.aspx*頁面。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image32.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image31.png)

按一下**編輯**中一個資料列中的值變更和**位置**資料行。

[![Image11](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image34.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image33.png)

開啟新的瀏覽器視窗並執行資料頁 （複製的 URL 從第一個瀏覽器視窗的第二個瀏覽器視窗）。

[![Image10](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image36.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image35.png)

按一下**編輯**在同一個資料列您稍早編輯並變更**位置**為其他值。

[![Image12](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image38.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image37.png)

在第二個瀏覽器視窗中，按一下 **更新**。

[![Image13](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image40.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image39.png)

切換至第一個瀏覽器視窗，然後按一下**更新**。

[![Image15](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image42.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image41.png)

您看到錯誤訊息和**位置**值已更新為顯示的值，第二個瀏覽器視窗中的變更。

## <a name="handling-concurrency-with-the-entitydatasource-control"></a>EntityDataSource 控制項處理並行存取

`EntityDataSource`控制項包含可辨識資料模型中的並行設定的內建邏輯，並控點會更新，並據以刪除作業。 不過，如同所有的例外狀況，您必須處理`OptimisticConcurrencyException`例外狀況自行以提供使用者易記的錯誤訊息。

接下來，您將設定*Courses.aspx*頁面 (它會使用`EntityDataSource`控制項) 允許更新和刪除作業，並顯示錯誤訊息，如果發生並行衝突。 `Course`實體沒有追蹤的資料行，所以您將使用相同的方法與您所執行的並行`Department`實體： 追蹤所有的非索引鍵屬性的值。

開啟*SchoolModel.edmx*檔案。 非索引鍵內容的`Course`實體 (`Title`， `Credits`，和`DepartmentID`)，將**並行模式**屬性`Fixed`。 然後儲存並關閉 資料模型。

開啟*Courses.aspx*頁面並進行下列變更：

- 在`CoursesEntityDataSource`控制項，加入`EnableUpdate="true"`和`EnableDelete="true"`屬性。 該控制項的開頭標記就會類似下列範例如下：

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample16.aspx)]
- 在`CoursesGridView`控制項，變更`DataKeyNames`屬性值，以`"CourseID,Title,Credits,DepartmentID"`。 然後加入`CommandField`元素`Columns`顯示的項目**編輯**和**刪除**按鈕 (`<asp:CommandField ShowEditButton="True" ShowDeleteButton="True" />`)。 `GridView`控制項現在會類似下列的範例：

    [!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample17.aspx)]

執行網頁，並建立衝突情況，如同您先前在 [部門] 頁面。 在兩個瀏覽器視窗中執行網頁中，按一下**編輯**在同一行中每個視窗中，並在每個不同變更。 按一下**更新**在一個視窗，然後按一下**更新**另一個視窗中。 當您按一下**更新**第二個階段中，您會看到錯誤頁面所產生的未處理的並行存取例外狀況。

[![Image22](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image44.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image43.png)

處理此錯誤的方式非常類似於如何處理它的`ObjectDataSource`控制項。 開啟*Courses.aspx*  頁面上，然後在`CoursesEntityDataSource`控制項，指定的處理常式`Deleted`和`Updated`事件。 控制項的開頭標記現在會類似下列的範例：

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample18.aspx)]

之前`CoursesGridView`控制項，加入下列`ValidationSummary`控制項：

[!code-aspx[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample19.aspx)]

在*Courses.aspx.cs*，新增`using`陳述式`System.Data`命名空間中，加入的方法，檢查對於並行存取例外狀況，並加入處理常式`EntityDataSource`控制項的`Updated`和`Deleted`處理常式。 程式碼看起來如下所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample20.cs)]

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/samples/sample21.cs)]

這個程式碼，您對唯一的差別`ObjectDataSource`控制項是在此情況下的並行存取例外狀況是在`Exception`屬性的事件引數物件，而不是在該例外狀況`InnerException`屬性。

執行網頁，然後重新建立並行衝突。 此時您會看到一則錯誤訊息：

[![Image23](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image46.png)](handling-concurrency-with-the-entity-framework-in-an-asp-net-web-application/_static/image45.png)

如此即完成了處理並行衝突的簡介。 下一個教學課程將提供如何改善使用 Entity Framework 的 web 應用程式效能的指引。

> [!div class="step-by-step"]
> [上一頁](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
> [下一頁](maximizing-performance-with-the-entity-framework-in-an-asp-net-web-application.md)
