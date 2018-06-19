---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 處理並行與 Entity Framework 6 在 ASP.NET MVC 5 應用程式 (10 / 12) |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: be0c098a-1fb2-457e-b815-ddca601afc65
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: b92aded80ad6b435a2409a137bb96fe4d0a726f4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875650"
---
<a name="handling-concurrency-with-the-entity-framework-6-in-an-aspnet-mvc-5-application-10-of-12"></a>處理並行與 Entity Framework 6 在 ASP.NET MVC 5 應用程式 (10 / 12)
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在先前的教學課程中，您學會如何更新資料。 本教學課程會顯示如何在多位使用者同時更新相同實體時處理衝突。

您將會變更網頁使用`Department`實體，好讓它們處理的並行錯誤。 下圖顯示的索引和刪除的頁面，包括發生並行衝突時，會顯示一些訊息。

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>並行衝突

當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。 若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。 在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。 在此情況下，您便不需要設定應用程式來處理並行衝突。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 （鎖定）

若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。 這稱為*封閉式並行存取*。 例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。 若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。 若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。

管理鎖定有幾個缺點。 其程式可能相當複雜。 這需要大量的資料庫管理資源，並且可能會隨著應用程式使用者的數量提升而導致效能問題。 基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。 Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取另一種是*開放式並行存取*。 開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。 例如，John 在部門編輯頁面上，將英文部門的**預算**從$350,000.00 變更為$0.00。

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John 按之前**儲存**，Jane 執行相同的頁面和變更**開始日期**從 2007 年 9 月 1 日欄位設為 2013 年 8 月 8 日。

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John 按**儲存**第一個和所看到的索引頁，然後 Jane 的瀏覽器傳回時，他變更按一下**儲存**。 接下來發生的情況便是由您處理並行衝突的方式決定。 一部分選項包括下列項目：

- 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。 在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。 在下一次有人瀏覽英文部門，他們會看到的 John 和 Jane 的變更 — 2013 年 8 月 8 日的開始日期和零美元的預算。

    這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。 Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。 通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。 維持大量狀態可能會影響應用程式的效能，因為它不是需要伺服器資源，就是必須包含在網頁中 (例如隱藏欄位)，或是保存在 Cookie 中。
- 您可以讓 Jane 的變更覆寫 John 的變更。 在下一次有人瀏覽英文部門，他們會看到時間 2013 年 8 月 8 日和還原 $350,000.00 值。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 (所有來自用戶端的值都會優先於資料存放區中的資料)。如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。
- 您可以防止資料庫中的更新 Jane 的變更。 一般而言，您會顯示錯誤訊息、 顯示她目前狀態的資料，並允許她她仍然想要讓它們重新套用變更。 這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 就會擲回的例外狀況。 若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須適當的設定資料庫及資料模型。 一部分啟用衝突偵測的選項包括下列選項：

- 在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。 接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。

    追蹤資料行的資料類型通常是[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。 [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是連續的數字已遞增資料列更新一次。 在`Update`或`Delete`命令，`Where`子句會包含追蹤資料行 （原始資料列版本） 的原始值。 如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，所以`Update`或`Delete`陳述式找不到的資料列，因為更新`Where`子句。 Entity Framework 找到的任何資料列已更新`Update`或`Delete`命令 （亦即，受影響的資料列數目為零） 時，它將會解譯為並行衝突。
- 設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。

    如同第一個選項之後第一次讀取的資料列，, 資料列中的任何項目已變更`Where`子句不會 Entity Framework 會解譯為並行衝突的傳回資料列來更新。 對於具有許多資料行的資料庫資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維護大量的狀態。 如前文所述，維持大量的狀態可能會影響應用程式的效能。 因此通常不建議使用這種方法，並且這種方法也不是此教學課程中所使用的方法。

    如果您想要實作這個方法來同步存取，您必須將您想要加入追蹤的並行存取實體中的所有非-主索引鍵屬性標記[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)這些屬性。 變更可讓 Entity Framework 了包含在 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。

在本教學課程的其餘部分，您會加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體建立控制器和檢視，和測試以確認一切運作正常。

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>Department 實體中加入的開放式並行存取屬性

在*Models\Department.cs*，加入名為的追蹤屬性`RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=20-22)]

[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定此資料行，將會併入`Where`子句`Update`和`Delete`命令傳送至資料庫。 該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 使用 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)資料類型，再將 SQL [rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)取代它。 .Net 類型[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)是位元組陣列。

如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定的追蹤屬性，如下列範例所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。 請在套件管理員主控台 (PMC) 中輸入下列命令：

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="modify-the-department-controller"></a>修改部門控制站

在*Controllers\DepartmentController.cs*，新增`using`陳述式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

在*DepartmentController.cs*檔案中，將所有的四個出現的"LastName"變更為"FullName"，使部門系統管理員的下拉式清單包含講師的完整名稱，而不是最後一個的名稱。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

現有程式碼取代`HttpPost``Edit`方法取代下列程式碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

若 `FindAsync` 方法傳回 Null，則該部門便已遭其他使用者刪除。 顯示的程式碼會使用已張貼的表單值建立 department 實體，以便可以重新顯示 [編輯] 頁面，並出現錯誤訊息。 或者，若您選擇只顯示錯誤訊息，而不重新顯示部門欄位，則您也可以不需要重新建立部門實體。

檢視會儲存原始`RowVersion`值的隱藏的欄位和方法中會收到在`rowVersion`參數。 在您呼叫 `SaveChanges` 之前，您必須將該原始 `RowVersion` 屬性值放入實體的 `OriginalValues` 集合中。 然後當 Entity Framework 建立 SQL`UPDATE`命令時，命令將會包含`WHERE`會尋找具有原始的資料列的子句`RowVersion`值。

如果任何資料列不受到`UPDATE`命令 (沒有資料列的原始`RowVersion`值)，Entity Framework 就會擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得在受影響`Department`例外狀況的實體物件。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

此物件具有新的值，在使用者輸入其`Entity`屬性，而且可以取得的值從資料庫讀取藉由呼叫`GetDatabaseValues`方法。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

`GetDatabaseValues`方法會傳回 null，如果有人已從資料庫刪除資料列; 否則您必須將傳回的物件轉換成`Department`類別才能存取`Department`屬性。 (因為您已檢查過為刪除，`databaseEntry`會是 null，只有當部門之後已刪除`FindAsync`執行之前`SaveChanges`執行。)

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

接下來，程式碼會加入每個資料行，其中包含資料庫值不同於 [編輯] 頁面上輸入的使用者自訂錯誤訊息：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

較長的錯誤訊息，說明發生了什麼事和資訊，請參閱：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

最後，此程式碼設定`RowVersion`值`Department`從資料庫擷取物件新的值。 這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。

在*Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值，緊接的隱藏的欄位`DepartmentID`屬性：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=18)]

## <a name="testing-optimistic-concurrency-handling"></a>測試開放式並行存取處理

執行站台，然後按一下**部門**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

以滑鼠右鍵按一下**編輯**英文部門並選取超連結**中新的索引標籤上，開啟**然後按一下 **編輯**英文部門的超連結。 兩個索引標籤會顯示相同的資訊。

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

變更第一個瀏覽器索引標籤中的欄位，然後按一下 [儲存]。

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

瀏覽器會顯示索引頁面，當中包含了變更之後的值。

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

變更第二個瀏覽器索引標籤中的欄位，然後按一下**儲存**。

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

按一下**儲存**第二個瀏覽器索引標籤。您會看到一個錯誤訊息：

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

再按一下 [儲存]。 您在第二個瀏覽器索引標籤中輸入的值會與您在第一個瀏覽器中變更資料的原始值一起儲存。 您會在索引頁面出現時看到儲存的值。

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>更新刪除頁面

針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。 當`HttpGet``Delete`方法會顯示 [確認] 檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。 值，就可以使用`HttpPost``Delete`使用者確認刪除作業時所呼叫的方法。 當 Entity Framework 建立 SQL`DELETE`命令時，它包含`WHERE`子句使用原始`RowVersion`值。 如果命令結果中零個資料列受影響 （已顯示 [刪除確認] 頁面之後，資料列已變更的意義），則會擲回並行存取例外狀況，而`HttpGet Delete`錯誤旗標設定為呼叫方法`true`以重新顯示確認頁面，並出現錯誤訊息。 此外，也可以零個資料列已受到影響，因為如此不同的錯誤訊息就會顯示在此情況下，資料列由其他使用者時，所刪除。

在*DepartmentController.cs*，取代`HttpGet``Delete`方法取代下列程式碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。 如果這個旗標`true`，錯誤訊息傳送至檢視使用`ViewBag`屬性。

中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 取代下列程式碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

您已變更此參數，以`Department`模型繫結器所建立的實體執行個體。 這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cs)]

您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。 名為 scaffold 的程式碼`HttpPost``Delete`方法`DeleteConfirmed`以便`HttpPost`方法唯一的簽章。 （在 CLR 需要有不同的方法參數的多載的方法）。現在，簽章是唯一的您可以盡可能使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`刪除方法。

若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。

在*Views\Department\Delete.cshtml*，scaffold 的程式碼取代下列程式碼新增的錯誤訊息欄位和 DepartmentID 和 RowVersion 屬性的隱藏的欄位。 所做的變更已醒目提示。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml?highlight=9-10,21,52-54)]

這個程式碼加入錯誤訊息之間`h2`和`h3`標題：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

它會取代`LastName`與`FullName`中`Administrator`欄位：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

最後，它會加入隱藏的欄位`DepartmentID`和`RowVersion`屬性之後`Html.BeginForm`陳述式：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cshtml)]

執行部門索引頁面。 以滑鼠右鍵按一下**刪除**英文部門並選取超連結**中新的索引標籤上，開啟**然後在第一個索引標籤中按一下**編輯**英文部門的超連結。

在第一個視窗中，變更其中一個值，然後按一下**儲存**:

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

索引頁確認變更。

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

在第二個索引標籤中，按一下 [刪除]。

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。

## <a name="summary"></a>總結

如此即完成了處理並行衝突的簡介。 其他的方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://msdn.microsoft.com/data/jj592904)和[屬性值使用](https://msdn.microsoft.com/data/jj592677)MSDN 上。 下一個教學課程示範如何實作的每個階層的資料表繼承`Instructor`和`Student`實體。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](async-and-stored-procedures-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
