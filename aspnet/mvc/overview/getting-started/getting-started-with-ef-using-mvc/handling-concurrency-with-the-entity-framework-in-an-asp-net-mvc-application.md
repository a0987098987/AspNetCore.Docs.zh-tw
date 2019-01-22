---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：ASP.NET MVC 5 應用程式中處理 ef 的並行存取
description: 本教學課程會示範如何使用以多位使用者同時更新相同的實體時處理衝突的開放式並行存取。
author: tdykstra
ms.author: riande
ms.date: 01/21/2019
ms.topic: tutorial
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b77b8d6f952472f4d3030f54665f970b8ace2caf
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444177"
---
# <a name="tutorial-handle-concurrency-with-ef-in-an-aspnet-mvc-5-app"></a>教學課程：ASP.NET MVC 5 應用程式中處理 ef 的並行存取

在先前的教學課程中，您學會如何更新資料。 本教學課程會示範如何使用以多位使用者同時更新相同的實體時處理衝突的開放式並行存取。 變更網頁使用`Department`實體，使它們處理並行錯誤。 下列圖例顯示了 [編輯] 和 [刪除] 頁面，包括一些發生並行衝突時會顯示的訊息。

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

在本教學課程中，您已：

> [!div class="checklist"]
> * 深入了解並行衝突
> * 新增開放式並行存取
> * 修改 Department 控制器
> * 測試並行處理
> * 更新 [刪除] 頁面

## <a name="prerequisites"></a>必要條件

* [非同步的預存程序](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="concurrency-conflicts"></a>並行衝突

當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。 若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。 在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。 在此情況下，您便不需要設定應用程式來處理並行衝突。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 （鎖定）

若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。 這就叫做*封閉式並行存取*。 例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。 若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。 若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。

管理鎖定有幾個缺點。 其程式可能相當複雜。 這需要大量的資料庫管理資源，並且可能會隨著應用程式使用者的數量提升而導致效能問題。 基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。 Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取的替代方案是*開放式並行存取*。 開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。 例如，John 在部門編輯頁面上，將英文部門的**預算**從$350,000.00 變更為$0.00。

John 按之前**儲存**，Jane 執行相同的頁面和變更**Start Date**欄位從 時間 2007 年 9 月 1 日到 2013 年 8 月 8 日。

John 按**儲存**第一個和所看到他的變更，回到 [索引] 頁面，然後 Jane 的瀏覽器時按下**儲存**。 接下來發生的情況便是由您處理並行衝突的方式決定。 一部分選項包括下列項目：

- 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。 在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。 在下一次有人瀏覽英文部門時，就會看到 Jane 和 John 所作出的變更，一開始的時間 2013 年 8 月 8 日的日期及預算為美金 0 元。

    這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。 Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。 通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。 維持大量狀態可能會影響應用程式的效能，因為它不是需要伺服器資源，就是必須包含在網頁中 (例如隱藏欄位)，或是保存在 Cookie 中。
- 您可以讓 Jane 的變更覆寫 John 所作出的變更。 在下一次有人瀏覽英文部門時，就會看到時間 2013 年 8 月 8 日和還原的美金 $350,000.00 元調整值。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 (所有來自用戶端的值都會優先於資料存放區中的資料)。如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。
- 您可以防止資料庫中的更新 Jane 的變更。 一般而言，您會顯示錯誤訊息、 她的目前狀態的資料，並允許她在她仍然想要讓它們重新套用變更。 這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 擲回的例外狀況。 若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須適當的設定資料庫及資料模型。 一部分啟用衝突偵測的選項包括下列選項：

- 在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。 接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。

    追蹤資料行的資料類型是通常[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。 [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是一個循序號碼，都會遞增每個資料列更新的時間。 在 `Update`或是`Delete`命令，`Where`子句會包含追蹤資料行 （原始的資料列版本） 的原始值。 如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，因此`Update`或是`Delete`陳述式找不到的資料列，因為更新`Where`子句。 當 Entity Framework 會尋找已藉由更新任何資料列`Update`或`Delete`命令 （也就是當受影響的資料列數目為零），它會解譯為並行衝突。
- 設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。

    第一個選項，如果資料列中的任何項目已變更因為這是第一個讀取的資料列，如同`Where`子句不會可解譯為並行衝突的 Entity Framework 傳回資料列來更新。 針對資料庫有許多資料行的資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維持大量的狀態。 如前文所述，維持大量的狀態可能會影響應用程式的效能。 因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。

    如果您想要實作這種並行存取的方法，您必須將標記您想要追蹤的並行存取，藉由新增實體中的所有非-主索引鍵屬性[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)屬性。 變更可讓 Entity Framework，包含 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。

在本教學課程的其餘部分您可以加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體，建立控制器和檢視，並測試以確認一切都運作正常。

## <a name="add-optimistic-concurrency"></a>新增開放式並行存取

在  *Models\Department.cs*，新增一個名為的追蹤屬性`RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定這個資料行都將包含在`Where`子句`Update`和`Delete`命令傳送至資料庫。 該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 在以 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)之前使用了 SQL 資料型別[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)加以取代。 .Net 類型[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)是一個位元組陣列。

如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定追蹤屬性，如下列範例所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。 請在套件管理員主控台 (PMC) 中輸入下列命令：

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-department-controller"></a>修改 Department 控制器

在  *Controllers\DepartmentController.cs*，新增`using`陳述式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

在  *DepartmentController.cs*檔案中，所有的四個出現的"LastName"變更為"FullName"，使部門系統管理員下拉式清單將包含講師的完整名稱，而非只有姓氏。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

取代現有的程式碼，如`HttpPost``Edit`為下列程式碼的方法：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

若 `FindAsync` 方法傳回 Null，則該部門便已遭其他使用者刪除。 顯示的程式碼會使用已張貼的表單值建立部門實體，以便可以重新顯示 [編輯] 頁面，並出現錯誤訊息。 或者，若您選擇只顯示錯誤訊息，而不重新顯示部門欄位，則您也可以不需要重新建立部門實體。

檢視儲存的原始`RowVersion`隱藏的欄位和方法中的值會接收在`rowVersion`參數。 在您呼叫 `SaveChanges` 之前，您必須將該原始 `RowVersion` 屬性值放入實體的 `OriginalValues` 集合中。 然後當 Entity Framework 建立 SQL 時，才`UPDATE`命令，命令便會包含`WHERE`尋找擁有原始的資料列的子句`RowVersion`值。

如果沒有任何資料列會受到`UPDATE`命令 (沒有任何資料列具有原始`RowVersion`值)，Entity Framework 擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得受影響`Department`例外狀況中的實體物件。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

此物件具有新的值，在使用者輸入其`Entity`屬性，而且可以取得的值從資料庫讀取藉由呼叫`GetDatabaseValues`方法。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues`方法會傳回 null，如果有人已從資料庫刪除資料列; 否則您必須將傳回的物件轉型`Department`類別，才能存取`Department`屬性。 (您已核取為刪除，因為`databaseEntry`會是 null 之後已刪除部門時，才`FindAsync`執行之前`SaveChanges`執行。)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

接下來，程式碼會加入每個資料行具有資料庫的值不同於在 [編輯] 頁面上輸入的使用者自訂錯誤訊息：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

較長的錯誤訊息，說明發生了什麼事，以及如何處理它：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

最後，程式碼會設定`RowVersion`的值`Department`從資料庫擷取物件的新值。 這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。

在  *Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值的隱藏的欄位的正後方`DepartmentID`屬性：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="test-concurrency-handling"></a>測試並行處理

執行站台，然後按一下**部門**。

以滑鼠右鍵按一下**編輯**超連結，英文部門時，然後選取**中新的索引標籤上，開啟**然後按一下**編輯**英文部門的超連結。 兩個索引標籤會顯示相同的資訊。

變更第一個瀏覽器索引標籤中的欄位，然後按一下 [儲存]。

瀏覽器會顯示索引頁面，當中包含了變更之後的值。

變更第二個瀏覽器索引標籤中的欄位，然後按一下**儲存**。 您會看到一個錯誤訊息：

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

再按一下 [儲存]。 您在第二個瀏覽器索引標籤中輸入的值會儲存與您在第一個瀏覽器中變更資料的原始值。 您會在索引頁面出現時看到儲存的值。

## <a name="update-the-delete-page"></a>更新 [刪除] 頁面

針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。 當`HttpGet``Delete`方法會顯示確認檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。 值，便可`HttpPost``Delete`在使用者確認刪除時呼叫的方法。 當 Entity Framework 建立 SQL`DELETE`命令，其中包含`WHERE`子句，而其原始`RowVersion`值。 如果命令會導致零個資料列受影響 （亦即已顯示 [刪除] 確認頁面之後，已變更資料列），則會擲回並行例外狀況，而`HttpGet Delete`方法呼叫錯誤旗標設為`true`才能重新顯示確認頁面，並出現錯誤訊息。 此外，也可以零個資料列已受到影響，因為另一位使用者，已刪除資料列，因此在此情況下會顯示不同的錯誤訊息。

在  *DepartmentController.cs*，取代`HttpGet``Delete`為下列程式碼的方法：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。 如果這個旗標`true`，將錯誤訊息傳送以檢視使用`ViewBag`屬性。

中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 為下列程式碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

您已變更此參數，以`Department`模型繫結所建立的實體執行個體。 這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。 名為的 scaffold 程式碼`HttpPost``Delete`方法`DeleteConfirmed`給`HttpPost`方法唯一的簽章。 （CLR 要求多載的方法，以不同的方法參數）。既然簽章是唯一的您可以繼續使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`delete 方法。

若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。

在  *Views\Department\Delete.cshtml*，將錯誤訊息欄位和 DepartmentID 及 RowVersion 屬性隱藏的欄位的下列程式碼取代 scaffold 程式碼。 所做的變更已醒目標示。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

此程式碼會新增一則錯誤訊息之間`h2`和`h3`標題：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

它會取代`LastName`具有`FullName`在`Administrator`欄位：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

最後，它會新增隱藏的欄位`DepartmentID`並`RowVersion`屬性之後`Html.BeginForm`陳述式：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

執行 Departments 索引 頁面。 以滑鼠右鍵按一下**刪除**超連結，英文部門時，然後選取**中新的索引標籤上，開啟**然後在第一個索引標籤中按一下**編輯**英文部門的超連結。

在第一個視窗中，請變更其中一個值，然後按一下**儲存**。

[索引] 頁面會確認變更。

在第二個索引標籤中，按一下 [刪除]。

您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。

## <a name="get-the-code"></a>取得程式碼

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>其他資源

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

其他方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://msdn.microsoft.com/data/jj592904)並[屬性值使用](https://msdn.microsoft.com/data/jj592677)MSDN 上。 下一個教學課程示範如何實作每個階層的資料表繼承`Instructor`和`Student`實體。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 了解並行衝突
> * 已新增的開放式並行存取
> * 修改後的 Department 控制器
> * 已測試的並行處理
> * 更新 [刪除] 頁面

請前往下一篇文章，以了解如何在資料模型中實作繼承。
> [!div class="nextstepaction"]
> [在資料模型中實作繼承](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)