---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
title: 處理使用 Entity Framework 的 ASP.NET MVC 應用程式 (7 的 10) 中的並行 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: b83f47c4-8521-4d0a-8644-e8f77e39733e
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 15dfce05d808a8af5ddfa6bbeb0b0baf449c7691
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378355"
---
<a name="handling-concurrency-with-the-entity-framework-in-an-aspnet-mvc-application-7-of-10"></a>處理並行使用 Entity Framework 中的 ASP.NET MVC 應用程式 (7 的 10)
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


先前的兩個教學課程中您用過的相關資料。 本教學課程會示範如何處理並行存取。 您將建立使用的網頁`Department`實體，以及編輯和刪除的頁面`Department`實體會處理並行錯誤。 下列圖例顯示索引 [刪除] 頁面，其中包括一些發生並行衝突會顯示的訊息。

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

## <a name="concurrency-conflicts"></a>並行衝突

當一名使用者為了編輯而顯示了實體的資料，然後另一名使用者在第一名使用者所作出的變更寫入到資料庫前便更新了相同實體的資料時，便會發生並行衝突。 若您沒有啟用針對這類衝突的偵測，最後更新資料庫的使用者所作出的變更便會覆寫前一名使用者所作出的變更。 在許多應用程式中，這類風險是可接受的：若僅有幾名使用者或僅有幾項更新，或覆寫變更的風險並不是那麼的重大，則為了處理並行而耗費的程式設計成本可能會大於其所能帶來的利益。 在此情況下，您便不需要設定應用程式來處理並行衝突。

### <a name="pessimistic-concurrency-locking"></a>封閉式並行存取 （鎖定）

若您的應用程式確實需要防止在並行案例下發生的意外資料遺失，其中一個方法便是使用資料庫鎖定。 這就叫做*封閉式並行存取*。 例如，在您從資料庫讀取一個資料列之前，您會要求唯讀鎖定或更新存取鎖定。 若您鎖定了一個資料列以進行更新存取，其他使用者便無法為了唯讀或更新存取而鎖定該資料列，因為他們會取得一個正在進行變更之資料的複本。 若您鎖定資料列以進行唯讀存取，其他使用者也可以為了唯讀存取將其鎖定，但無法進行更新。

管理鎖定有幾個缺點。 其程式可能相當複雜。 它需要大量的資料庫管理資源，以及它會導致效能問題的應用程式的使用者數目會增加 （亦即，將無法妥善調整）。 基於這些理由，不是所有的資料庫管理系統都支援封閉式並行存取。 Entity Framework 提供的內建支援，而本教學課程不會顯示您如何實作它。

### <a name="optimistic-concurrency"></a>開放式並行存取

封閉式並行存取的替代方案是*開放式並行存取*。 開放式並行存取表示允許並行衝突發生，然後在衝突發生時適當的做出反應。 例如，John 在部門編輯頁面上，將英文部門的**預算**從$350,000.00 變更為$0.00。

![Changing_English_dept_budget_to_100000](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

John 按之前**儲存**，Jane 執行相同的頁面和變更**Start Date**欄位從 時間 2007 年 9 月 1 日到 2013 年 8 月 8 日。

![Changing_English_dept_start_date_to_1999](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

John 按**儲存**第一個和所看到他的變更，回到 [索引] 頁面，然後 Jane 的瀏覽器時按下**儲存**。 接下來發生的情況便是由您處理並行衝突的方式決定。 一部分選項包括下列項目：

- 您可以追蹤使用者修改的屬性，然後僅在資料庫中更新相對應的資料行。 在範例案例中，將不會發生資料遺失，因為兩名使用者更新的屬性不同。 在下一次有人瀏覽英文部門時，就會看到 Jane 和 John 所作出的變更，一開始的時間 2013 年 8 月 8 日的日期及預算為美金 0 元。

    這個更新方法可減少可能導致資料遺失之衝突發生的次數，但卻無法在實體中的相同屬性遭到變更時避免資料遺失。 Entity Framework 是否會以這種方式處理並行衝突，取決於您實作更新程式碼的方式。 通常在 Web 應用程式中，這種方法並不實用，因為它需要您維持大量的狀態，以追蹤實體所有原始的屬性值和新的值。 維持大量的狀態可能會影響應用程式效能，因為它需要伺服器資源，或必須包含在 web 網頁 （例如，在隱藏欄位）。
- 您可以讓 Jane 的變更覆寫 John 所作出的變更。 在下一次有人瀏覽英文部門時，就會看到時間 2013 年 8 月 8 日和還原的美金 $350,000.00 元調整值。 這稱之為「用戶端獲勝 (Client Wins)」或「最後寫入為準 (Last in Wins)」案例。 （用戶端的值優先於功能的資料存放區。）如同本節一開始所描述，若您沒有為並行衝突撰寫任何程式碼，這種情況便會自動發生。
- 您可以防止資料庫中的更新 Jane 的變更。 一般而言，您會顯示錯誤訊息、 她的目前狀態的資料，並允許她在她仍然想要讓它們重新套用變更。 這稱之為「存放區獲勝 (Store Wins)」案例。 (資料存放區的值會優先於用戶端所提交的值。)您將在此教學課程中實作存放區獲勝案例。 這個方法可確保沒有任何變更會在使用者收到警示，告知其發生的事情前遭到覆寫。

### <a name="detecting-concurrency-conflicts"></a>偵測並行衝突

您可以藉由處理解決衝突[OptimisticConcurrencyException](https://msdn.microsoft.com/library/system.data.optimisticconcurrencyexception.aspx) Entity Framework 擲回的例外狀況。 若要得知何時應擲回這些例外狀況，Entity Framework 必須能夠偵測衝突。 因此，您必須適當的設定資料庫及資料模型。 一部分啟用衝突偵測的選項包括下列選項：

- 在資料庫資料表中，包含一個追蹤資料行，該資料行可用於決定資料列發生變更的時機。 接著，您可以設定要包含在該資料行的 Entity Framework`Where`子句的 SQL`Update`或`Delete`命令。

    追蹤資料行的資料類型是通常[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)。 [Rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)值是一個循序號碼，都會遞增每個資料列更新的時間。 在 `Update`或是`Delete`命令，`Where`子句會包含追蹤資料行 （原始的資料列版本） 的原始值。 如果正在更新的資料列已由另一位使用者中的值變更`rowversion`資料行是不同於原始值，因此`Update`或是`Delete`陳述式找不到的資料列，因為更新`Where`子句。 當 Entity Framework 會尋找已藉由更新任何資料列`Update`或`Delete`命令 （也就是當受影響的資料列數目為零），它會解譯為並行衝突。
- 設定要包含的資料表中的每個資料行的原始值的 Entity Framework`Where`子句`Update`和`Delete`命令。

    第一個選項，如果資料列中的任何項目已變更因為這是第一個讀取的資料列，如同`Where`子句不會可解譯為並行衝突的 Entity Framework 傳回資料列來更新。 針對資料庫有許多資料行的資料表，這種方法可能會導致非常大`Where`子句，而且可能需要您維持大量的狀態。 如先前所述，維持大量狀態可能會影響應用程式的效能，因為它需要伺服器資源，或必須包含在網頁中。 因此一般而言不建議使用這個方法，並不在本教學課程所使用的方法。

    如果您想要實作這種並行存取的方法，您必須將標記您想要追蹤的並行存取，藉由新增實體中的所有非-主索引鍵屬性[ConcurrencyCheck](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.concurrencycheckattribute.aspx)屬性。 變更可讓 Entity Framework，包含 SQL 中的所有資料行`WHERE`子句`UPDATE`陳述式。

在本教學課程的其餘部分您可以加入[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)追蹤屬性`Department`實體，建立控制器和檢視，並測試以確認一切都運作正常。

## <a name="add-an-optimistic-concurrency-property-to-the-department-entity"></a>新增到 Department 實體的開放式並行存取屬性

在  *Models\Department.cs*，新增一個名為的追蹤屬性`RowVersion`:

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs?highlight=18-19)]

[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)屬性會指定這個資料行都將包含在`Where`子句`Update`和`Delete`命令傳送至資料庫。 該屬性稱為[時間戳記](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.timestampattribute.aspx)因為舊版的 SQL Server 在以 SQL[時間戳記](https://msdn.microsoft.com/library/ms182776(v=SQL.90).aspx)之前使用了 SQL 資料型別[rowversion](https://msdn.microsoft.com/library/ms182776(v=sql.110).aspx)加以取代。 .Net 類型`rowversion`是一個位元組陣列。 如果您想要使用 fluent API，您可以使用[IsConcurrencyToken](https://msdn.microsoft.com/library/gg679501(v=VS.103).aspx)方法，以指定追蹤屬性，如下列範例所示：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

由於新增屬性之後，您也變更了資料庫模型，因此您必須再一次進行移轉。 請在套件管理員主控台 (PMC) 中輸入下列命令：

[!code-console[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cmd)]

## <a name="create-a-department-controller"></a>建立 Department 控制器

建立`Department`控制器和檢視相同的方式就像其他控制站，使用下列設定：

![Add_Controller_dialog_box_for_Department_controller](https://asp.net/media/2578041/Windows-Live-Writer_Handling-C.NET-MVC-Application-7-of-10h1_AFDC_Add_Controller_dialog_box_for_Department_controller_d1d9c788-f970-4d6a-9f5a-1eddc84330b7.png)

在  *Controllers\DepartmentController.cs*，新增`using`陳述式：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

變更"LastName"為"FullName"這個檔案 （四個相符項目） 中的所有位置使部門系統管理員下拉式清單會包含講師的完整名稱，而非只有姓氏。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs?highlight=1)]

取代現有的程式碼，如`HttpPost``Edit`為下列程式碼的方法：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

檢視會儲存原始`RowVersion`隱藏欄位中的值。 當建立模型繫結`department`執行個體，該物件會有原始`RowVersion`屬性值和其他屬性，如使用者所輸入的 [編輯] 頁面上的新值。 然後當 Entity Framework 建立 SQL 時，才`UPDATE`命令，命令便會包含`WHERE`尋找擁有原始的資料列的子句`RowVersion`值。

如果沒有任何資料列會受到`UPDATE`命令 (沒有任何資料列具有原始`RowVersion`值)，Entity Framework 擲回`DbUpdateConcurrencyException`例外狀況和中的程式碼`catch`區塊會取得受影響`Department`例外狀況中的實體物件。 此實體已從資料庫讀取的值和新使用者所輸入的值：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

接下來，程式碼會加入每個資料行具有資料庫的值不同於在 [編輯] 頁面上輸入的使用者自訂錯誤訊息：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

較長的錯誤訊息，說明發生了什麼事，以及如何處理它：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

最後，程式碼會設定`RowVersion`的值`Department`從資料庫擷取物件的新值。 這個新的 `RowVersion` 值會在編輯頁面重新顯示時儲存於隱藏欄位中，並且當下一次使用者按一下 [儲存] 時，只有在重新顯示 [編輯] 頁面之後發生的並行錯誤才會被捕捉到。

在  *Views\Department\Edit.cshtml*，加入隱藏的欄位，以儲存`RowVersion`屬性值的隱藏的欄位的正後方`DepartmentID`屬性：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cshtml?highlight=17)]

在  *Views\Department\Index.cshtml*，現有的程式碼取代為下列程式碼，向左移動資料列的連結，並變更要顯示的標題和資料行標題`FullName`而不是`LastName`中**系統管理員**資料行：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cshtml)]

## <a name="testing-optimistic-concurrency-handling"></a>測試開放式並行存取處理

執行站台，然後按一下**部門**:

![Department_Index_page_before_edits](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

以滑鼠右鍵按一下**編輯**超連結，Kim Abercrombie 然後選取**中新的索引標籤上，開啟**然後按一下**編輯**Kim Abercrombie 的超連結。 兩個視窗會顯示相同的資訊。

![Department_Edit_page_before_changes](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

變更第一個瀏覽器視窗中的欄位，然後按一下**儲存**。

![Department_Edit_page_1_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

瀏覽器會顯示索引頁面，當中包含了變更之後的值。

![Departments_Index_page_after_first_budget_edit](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

變更第二個瀏覽器視窗中的任何欄位，然後按一下**儲存**。

![Department_Edit_page_2_after_change](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

按一下 **儲存**第二個瀏覽器視窗中。 您會看到一個錯誤訊息：

![Department_Edit_page_2_after_clicking_Save](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

再按一下 [儲存]。 您在第二個瀏覽器中輸入的值會儲存與您在第一個瀏覽器中變更資料的原始值。 您會在索引頁面出現時看到儲存的值。

![Department_Index_page_with_change_from_second_browser](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)

## <a name="updating-the-delete-page"></a>更新 [刪除] 頁面

針對 [刪除] 頁面，Entity Framework 會偵測由其他對部門進行類似編輯的人員所造成的並行衝突。 當`HttpGet``Delete`方法會顯示確認檢視，檢視會包含原始`RowVersion`隱藏欄位中的值。 值，便可`HttpPost``Delete`在使用者確認刪除時呼叫的方法。 當 Entity Framework 建立 SQL`DELETE`命令，其中包含`WHERE`子句，而其原始`RowVersion`值。 如果命令會導致零個資料列受影響 （亦即已顯示 [刪除] 確認頁面之後，已變更資料列），則會擲回並行例外狀況，而`HttpGet Delete`方法呼叫錯誤旗標設為`true`才能重新顯示確認頁面，並出現錯誤訊息。 此外，也可以零個資料列已受到影響，因為另一位使用者，已刪除資料列，因此在此情況下會顯示不同的錯誤訊息。

在  *DepartmentController.cs*，取代`HttpGet``Delete`為下列程式碼的方法：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cs)]

方法會接受一個選用的參數，該參數會指示頁面是否已在發生並行錯誤之後重新顯示。 如果這個旗標`true`，將錯誤訊息傳送以檢視使用`ViewBag`屬性。

中的程式碼取代`HttpPost``Delete`方法 (名為`DeleteConfirmed`) 為下列程式碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cs)]

在您剛剛取代的 Scaffold 程式碼中，此方法僅會接受一個記錄識別碼：

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cs)]

您已變更此參數，以`Department`模型繫結所建立的實體執行個體。 這可讓您存取`RowVersion`除了記錄索引鍵的屬性值。

[!code-csharp[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cs)]

您也將動作方法的名稱從 `DeleteConfirmed` 變更為 `Delete`。 名為的 scaffold 程式碼`HttpPost``Delete`方法`DeleteConfirmed`給`HttpPost`方法唯一的簽章。 (CLR 要求多載方法必須要有不同的方法參數。)既然簽章是唯一的您可以繼續使用 MVC 慣例，並使用相同的名稱`HttpPost`和`HttpGet`delete 方法。

若捕捉到並行錯誤，程式碼會重新顯示刪除確認頁面，並提供一個旗標指示其應顯示並行錯誤訊息。

在  *Views\Department\Delete.cshtml*，取代為下列程式碼，可讓某些格式的 scaffold 程式碼變更，並將錯誤訊息欄位。 所做的變更已醒目提示。

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml?highlight=9,37,40,45-46)]

此程式碼會新增一則錯誤訊息之間`h2`和`h3`標題：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

它會取代`LastName`具有`FullName`在`Administrator`欄位：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cshtml)]

最後，它會新增隱藏的欄位`DepartmentID`並`RowVersion`屬性之後`Html.BeginForm`陳述式：

[!code-cshtml[Main](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cshtml)]

執行 Departments 索引 頁面。 以滑鼠右鍵按一下**刪除**超連結，英文部門時，然後選取**在新視窗中，開啟**然後在第一個視窗中按一下**編輯**適用於英文的超連結部門。

在第一個視窗中，請變更其中一個值，然後按一下**儲存**:

![Department_Edit_page_after_change_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

[索引] 頁面會確認變更。

![Departments_Index_page_after_budget_edit_before_delete](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)

在第二個視窗中，按一下**刪除**。

![Department_Delete_confirmation_page_before_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)

您會看到並行錯誤訊息，並且 Department 值已根據資料庫中的內容重新整理。

![Department_Delete_confirmation_page_with_concurrency_error](handling-concurrency-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)

若您再按一下 [刪除]，則您將會重新導向至 [索引] 頁面，並且系統將顯示該部門已遭刪除。

## <a name="summary"></a>總結

如此即完成了處理並行衝突的簡介。 其他方式來處理各種並行案例的相關資訊，請參閱[開放式並行存取模式](https://blogs.msdn.com/b/adonet/archive/2011/02/03/using-dbcontext-in-ef-feature-ctp5-part-9-optimistic-concurrency-patterns.aspx)並[屬性值使用](https://blogs.msdn.com/b/adonet/archive/2011/01/30/using-dbcontext-in-ef-feature-ctp5-part-5-working-with-property-values.aspx)Entity Framework 小組部落格上。 下一個教學課程示範如何實作每個階層的資料表繼承`Instructor`和`Student`實體。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](updating-related-data-with-the-entity-framework-in-an-asp-net-mvc-application.md)
> [下一頁](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
