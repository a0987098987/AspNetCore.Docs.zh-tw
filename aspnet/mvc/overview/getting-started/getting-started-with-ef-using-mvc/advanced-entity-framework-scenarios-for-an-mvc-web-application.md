---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 進階 MVC 5 Web 應用程式 (12 / 12) 的 Entity Framework 6 案例 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/08/2014
ms.topic: article
ms.assetid: f35a9b0c-49ef-4cde-b06d-19d1543feb0b
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 50987b30a49173605e7aeb8eb65ff1fe5d5e5753
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="advanced-entity-framework-6-scenarios-for-an-mvc-5-web-application-12-of-12"></a>MVC 5 Web 應用程式 (12 / 12) 的進階的 Entity Framework 6 案例
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在上一個教學課程中，您會實作資料表每個階層繼承。 本教學課程包含介紹幾個有用，可注意當您超越開發使用 Entity Framework Code First 的 ASP.NET web 應用程式的基本概念的主題。 逐步解說會引導您完成程式碼和使用 Visual Studio 的下列主題：

- [執行原始的 SQL 查詢](#rawsql)
- [執行無追蹤查詢](#notracking)
- [檢查 SQL 傳送至資料庫](#sql)

本教學課程介紹幾個主題的簡介，其後加上行連結資源，如需詳細資訊：

- [儲存機制和單位的工作模式](#repo)
- [Proxy 類別](#proxies)
- [自動變更偵測](#changedetection)
- [自動驗證](#validation)
- [Visual Studio 的 EF 工具](#tools)
- [Entity Framework 原始碼](#source)

本教學課程也包含下列各節：

- [摘要](#summary)
- [通知](#acknowledgments)
- [關於 VB 的附註](#vb)
- [常見的錯誤，和它們的因應措施或解決方案](#errors)

這些主題的大多數時間，您將使用您已經建立的網頁。 若要執行大量更新使用原始的 SQL 中，您將建立新的頁面，以更新資料庫中的所有課程的信用額度的數目：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

<a id="rawsql"></a>
## <a name="performing-raw-sql-queries"></a>執行原始的 SQL 查詢

Entity Framework 程式碼的第一個 API 包含可讓您將直接對資料庫的 SQL 命令的方法。 下列選項可供您選擇：

- 使用[DbSet.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.dbset.sqlquery.aspx)查詢來傳回實體類型的方法。 傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤對資料庫內容所除非您關閉追蹤。 (請參閱下一節有關[AsNoTracking](https://msdn.microsoft.com/library/system.data.entity.dbextensions.asnotracking.aspx)方法。)
- 使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery.aspx)方法的傳回類型不是實體的查詢。 即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。
- 使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456.aspx)非查詢命令。

使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。 它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。 但有例外狀況時，您需要執行特定 SQL 查詢，以手動方式建立，而且這些方法可讓您處理這類例外狀況。

如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。 執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。 在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。

### <a name="calling-a-query-that-returns-entities"></a>呼叫查詢會傳回實體

[DbSet&lt;TEntity&gt; ](https://msdn.microsoft.com/library/gg696460.aspx)類別會提供方法可讓您執行查詢，以傳回型別的實體`TEntity`。 若要查看這對您所做的運作將會在變更程式碼`Details`方法`Department`控制站。

在*DepartmentController.cs*，請在`Details`方法，取代`db.Departments.FindAsync`方法呼叫`db.Departments.SqlQuery`方法呼叫，如下列反白顯示的程式碼所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs?highlight=8-14)]

若要確認新的程式碼運作正常，請選取 [部門]  索引標籤，然後針對其中一個部門選取 [詳細資料] 。

![部門詳細資料](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>呼叫查詢傳回其他類型的物件

先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。 在程式碼*HomeController.cs*使用 LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

假設您想要撰寫的程式碼會擷取直接在 SQL，而不是使用 LINQ 中的此資料。 若要執行您要執行查詢，以傳回實體物件以外的項目，這表示您需要使用[Database.SqlQuery](https://msdn.microsoft.com/library/system.data.entity.database.sqlquery(v=VS.103).aspx)方法。

在*HomeController.cs*，取代中的 LINQ 陳述式`About`方法使用 SQL 陳述式，如下列反白顯示的程式碼所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs?highlight=3-18)]

執行 「 關於 」 頁面。 它會顯示與之前相同的資料。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-an-update-query"></a>呼叫更新查詢

假設 Contoso 大學系統管理員想要能夠執行大量變更，在資料庫中，例如變更的每個課程信用額度的數目。 如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。 在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您會變更執行 SQL`UPDATE`陳述式。 網頁看起來將如下圖所示：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

在*CourseContoller.cs*，新增`UpdateCourseCredits`方法`HttpGet`和`HttpPost`:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

當控制器處理`HttpGet`要求，不會傳回在`ViewBag.RowsAffected`變數，並檢視會顯示空白的文字方塊和送出按鈕，在上圖所示。

當**更新**按一下按鈕時，`HttpPost`呼叫方法時，和`multiplier`已在文字方塊中輸入的值。 然後執行的程式碼會更新課程並傳回受影響的資料列數目至檢視中的 SQL`ViewBag.RowsAffected`變數。 當檢視中，取得值，變數時，它會顯示更新而不是在文字方塊中的資料列數目並提交 按鈕，如下圖所示：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

在*CourseController.cs*，以滑鼠右鍵按一下其中一個`UpdateCourseCredits`方法，然後再按一下**加入檢視**。

![Add_View_dialog_box_for_Update_Course_Credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

在*Views\Course\UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cshtml)]

藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:50205/Course/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。 在文字方塊中輸入數目：

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

按一下 [更新] 。 您會看到受影響的資料列數目：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

按一下 [回到清單]，以查看課程與已修訂學分數的清單。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://msdn.microsoft.com/data/jj592907)MSDN 上。

<a id="notracking"></a>
## <a name="no-tracking-queries"></a>不追蹤查詢

當資料庫內容擷取資料表資料列並建立代表他們的實體物件時，根據預設，它會追蹤在記憶體中的實體是否與資料庫中的內容保持同步。 記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。 這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。

您可以使用停用記憶體中的實體物件的追蹤[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法。 您會想要進行這項操作的常見案例包括下列情況：

- 查詢會擷取這類大量的資料，關閉追蹤可能會大幅提升效能。
- 您想要將實體附加以更新，但您稍早擷取同一個實體用於不同用途。 由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。 若要處理這種情況的一種方式為使用`AsNoTracking`與前面的查詢選項。

如需範例，示範如何使用[AsNoTracking](https://msdn.microsoft.com/library/gg679352(v=vs.103).aspx)方法，請參閱[本教學課程的舊版](../../older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application.md)。 此版的教學課程不會修改旗標上設定繫結器建立模型中實體的編輯方法，讓它不需要`AsNoTracking`。

<a id="sql"></a>
## <a name="examining-sql-sent-to-the-database"></a>檢查 SQL 傳送至資料庫

有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。 在先前的教學課程中您可了解如何在攔截器的程式碼; 執行現在您會看到一些不寫入攔截器程式碼。 再試一次時，您將查看簡單查詢，並查看您新增這類 eager 載入、 篩選和排序選項，它會發生什麼事。

在*控制器/CourseController*，取代`Index`方法取代下列程式碼中，若要暫時停止積極式載入：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

現在上設定中斷點`return`陳述式 (F9 與該行上指標)。 按 F5 以偵錯模式執行專案並選取課程索引頁面。 當程式碼到達中斷點時，檢查`sql`變數。 您會看到傳送至 SQL Server 的查詢。 它是簡單`Select`陳述式。

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.json)]

按一下放大的類別，以查看中的查詢**文字視覺化檢視**。

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

現在您將新增下拉式選單課程索引頁，讓使用者可以篩選特定部門。 您將項目，所排序的課程，您將指定的積極式載入`Department`導覽屬性。

在*CourseController.cs*，取代`Index`方法取代下列程式碼：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

在還原中斷點`return`陳述式。

此方法接收的下拉式清單中選取的值`SelectedDepartment`參數。 如果選取任何項目，這個參數會是 null。

A`SelectList`集合，其中包含所有部門傳遞至檢視的下拉式清單。 參數傳遞至`SelectList`建構函式指定的值欄位名稱、 文字欄位名稱，以及選取的項目。

如`Get`方法`Course`儲存機制、 程式碼會指定篩選條件運算式、 排序次序，以及載入 eager`Department`導覽屬性。 篩選運算式永遠都會傳回`true`如果不會選取下拉式清單中 (也就是`SelectedDepartment`為 null)。

在*Views\Course\Index.cshtml*之前開啟,`table`標記中，加入下列程式碼，建立下拉式清單及提交按鈕：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

中斷點仍組，請執行課程索引頁面。 繼續透過程式碼叫用中斷點的行，第一次，讓瀏覽器中會顯示頁面。 從下拉式清單中選取部門並按一下**篩選**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

這次下拉式清單的部門查詢就是第一個中斷點。 略過，並檢視`query`變數在下一次程式碼中斷點時若要查看哪些`Course`查詢現在看起來像。 您會看到類似如下：

[!code-sql[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.sql)]

您可以看到查詢，現在是`JOIN`載入的查詢`Department`資料連同`Course`資料，以及它包含`WHERE`子句。

移除`var sql = courses.ToString()`列。

<a id="repo"></a>

## <a name="repository-and-unit-of-work-patterns"></a>存放庫和工作單元模式

許多開發人員撰寫程式碼以實作存放庫和工作單元模式，作為使用 Entity Framework 之程式碼周圍的包裝函式。 這些模式主要用來建立資料存取層和應用程式的商務邏輯層之間的抽象層。 實作這些模式可協助隔離您的應用程式與資料存放區中的變更，並可促進自動化單元測試或測試驅動開發 (TDD)。 不過，撰寫額外的程式碼來實作這些模式做不一定有數種原因使用 EF，應用程式的最佳選擇：

- EF 內容類別本身會隔離您的程式碼與資料存放區特有的程式碼。
- EF 內容類別可作為您使用 EF 進行之資料庫更新的工作單元類別。
- Entity Framework 6 中導入的功能可讓您更輕鬆地實作 TDD，而不需要撰寫的程式碼儲存機制。

如需如何實作儲存機制和單位的工作模式的詳細資訊，請參閱[此教學課程系列的 Entity Framework 5 新版](../../older-versions/getting-started-with-ef-5-using-mvc-4/implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)。 在 Entity Framework 6 中實作 TDD 的方式的相關資訊，請參閱下列資源：

- [如何 EF6 讓 Mocking DbSets 更輕鬆地](http://thedatafarm.com/data-access/how-ef6-enables-mocking-dbsets-more-easily/)
- [測試模擬架構](https://msdn.microsoft.com/data/dn314429)
- [使用您自己的測試複本測試](https://msdn.microsoft.com/data/dn314431)

<a id="proxies"></a>
## <a name="proxy-classes"></a>Proxy 類別

當 Entity Framework 建立的實體執行個體 （例如，當您執行查詢時） 時，它通常會建立這些動態產生的衍生型別，可做為實體的 proxy 的執行個體。 例如，請參閱下列兩個偵錯工具映像。 在第一個映像，您會看到`student`變數是預期`Student`您具現化實體之後，立即輸入。 在第二個影像中，已從資料庫讀取學生實體使用 EF 之後您會看到 proxy 類別。

![Proxy 類別之前](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

![Proxy 類別之後](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

這個 proxy 類別會覆寫要插入在屬性經過存取時，自動執行動作的攔截程序之實體的某些虛擬屬性。 一個用於這項機制的函式是消極式載入。

大部分的情況下您不需要注意這項使用的 proxy，但有例外狀況：

- 在某些情況下您可能想要防止 Entity Framework 建立 proxy 執行個體。 例如，當您序列化的實體您通常會想 POCO 類別，而不是 proxy 類別。 一種方式以避免發生序列化問題為序列化而不是實體物件的資料傳輸物件 (Dto) 中所示[使用 Web API 和 Entity Framework](../../../../web-api/overview/data/using-web-api-with-entity-framework/part-1.md)教學課程。 另一個方法是[停用 proxy 建立](https://msdn.microsoft.com/data/jj592886.aspx)。
- 當您具現化的實體類別使用`new`運算子，就無法取得 proxy 執行個體。 這表示您不先取得功能，例如消極式載入和自動變更追蹤。 這通常是好;您通常不需要延遲載入，因為您要建立新的實體不在資料庫中，而且通常不需要變更追蹤，如果您要明確地標示為實體`Added`。 不過，如果您需要消極式載入，而您需要變更追蹤，您可以建立新的實體執行個體使用的 proxy[建立](https://msdn.microsoft.com/library/gg679504.aspx)方法`DbSet`類別。
- 您可能想要從的 proxy 型別取得實際的實體類型。 您可以使用[GetObjectType](https://msdn.microsoft.com/library/system.data.objects.objectcontext.getobjecttype.aspx)方法`ObjectContext`類別取得的 proxy 型別執行個體的實際實體類型。

如需詳細資訊，請參閱[使用 Proxy](https://msdn.microsoft.com/data/JJ592886.aspx) MSDN 上。

<a id="changedetection"></a>
## <a name="automatic-change-detection"></a>自動變更偵測

Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。 查詢或附加實體時，會儲存原始值。 會導致自動變更偵測的一些方法如下：

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled.aspx)屬性。 如需詳細資訊，請參閱[自動偵測變更](https://msdn.microsoft.com/data/jj556205)MSDN 上。

<a id="validation"></a>
## <a name="automatic-validation"></a>自動驗證

當您呼叫`SaveChanges`方法，依預設 Entity Framework 中所有已變更實體的所有屬性的資料會先驗證更新資料庫。 如果您已更新大量實體且您已驗證資料，這項工作不需要確定儲存的程序所做的變更會藉由暫時關閉驗證需要較少的時間。 您可以使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled.aspx)屬性。 如需詳細資訊，請參閱[驗證](https://msdn.microsoft.com/data/gg193959)MSDN 上。

<a id="tools"></a>
## <a name="entity-framework-power-tools"></a>Entity Framework Power Tools

[Entity Framework Power Tools](https://visualstudiogallery.msdn.microsoft.com/72a60b14-1581-4b9b-89f2-846072eff19d)用來建立資料模型圖表的 Visual Studio 增益集顯示在這些教學課程。 工具也可以執行其他函式，例如產生實體類別根據現有的資料庫中的資料表，讓您可以使用 Code first 的資料庫。 安裝工具之後，其他選項會出現在內容功能表。 例如，當您以滑鼠右鍵按一下您的內容類別中**方案總管 中**，會產生圖表選項。 當您使用 Code First 時您無法變更圖表中的資料模型，但是您可以移動項目，讓您更輕鬆地了解。

![在內容功能表中的 EF](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image14.png)

![EF 圖表](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image15.png)

<a id="source"></a>
## <a name="entity-framework-source-code"></a>Entity Framework 原始碼

Entity Framework 6 的原始程式碼位於[GitHub](https://github.com/aspnet/EntityFramework6)。 您可以檔 bug，而且您可能會造成您自己的 EF 原始碼的增強功能。

雖然開啟原始碼時，Entity Framework 則完全支援以 Microsoft 產品。 Microsoft Entity Framework 小組將控制接受哪些貢獻，並測試所有的程式碼變更以確保每次發行的品質。

<a id="summary"></a>
## <a name="summary"></a>總結

如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework。 如需如何搭配使用 Entity Framework 資料的詳細資訊，請參閱[EF MSDN 上的文件頁面](https://msdn.microsoft.com/data/ee712907)和[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

如需如何建置它之後部署 web 應用程式的詳細資訊，請參閱[ASP.NET Web 部署的建議資源](../../../../whitepapers/aspnet-web-deployment-content-map.md)MSDN Library 中。

如需有關 MVC、 驗證和授權，例如其他主題，請參閱[ASP.NET MVC-建議資源](../recommended-resources-for-mvc.md)。

<a id="acknowledgments"></a>
## <a name="acknowledgments"></a>感謝

- Tom Dykstra 寫入此教學課程中的原始版本、 共同撰寫 EF 5 更新，並撰寫 EF 6 更新。 Tom 是資深的開發寫入器上的 Microsoft Web 平台和工具的內容團隊。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 未的大部分工作更新的教學課程 EF 5 和 MVC 4 和共同撰寫 EF 6 更新。 Rick 是將焦點放在 Azure 和 MVC Microsoft 資深程式寫入器。
- [Rowan Miller](http://www.romiller.com)和 Entity Framework 小組的其他成員協助程式碼檢閱，並協助偵錯移轉，我們已更新本教學課程 EF 5 和 EF 6 時，會發生的許多問題。

<a id="vb"></a>
## <a name="vb"></a>VB

本教學課程原本所產生的 EF 4.1，當我們提供 C# 和 VB 專案版本的下載已完成。 由於時間限制和其他優先順序我們您尚未這樣做，此版本。 如果您建立使用這些教學課程將 VB 專案，願意，與其他人共用，請讓我們知道。

<a id="errors"></a>
## <a name="common-errors-and-solutions-or-workarounds-for-them"></a>常見的錯誤，和它們的因應措施或解決方案

### <a name="cannot-createshadow-copy"></a>無法建立或陰影複製

錯誤訊息：

> 無法建立或陰影複製 '&lt;filename&gt;' 已存在該檔案。


方案

等候數秒鐘，並重新整理頁面。

### <a name="update-database-not-recognized"></a>更新資料庫無法辨識

錯誤訊息 (從`Update-Database`PMC 命令):

> 'Update-database' 詞彙無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。 請檢查名稱拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。


方案

結束 Visual Studio。 重新開啟專案，然後再試一次。

### <a name="validation-failed"></a>驗證失敗

錯誤訊息 (從`Update-Database`PMC 命令):

> 一個或多個實體的驗證失敗。 請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。


方案

發生此問題的其中一個原因是驗證錯誤時`Seed`方法執行。 請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)如需有關偵錯秘訣`Seed`方法。

### <a name="http-50019-error"></a>HTTP 500.19 錯誤

錯誤訊息：

> HTTP 錯誤 500.19-內部伺服器錯誤  
> 無法存取要求的網頁，因為該頁面的相關的組態資料無效。


方案

您可以取得此錯誤的其中一種方式是方案的從多個複本時，每個使用相同的連接埠號碼。 您通常可以結束 Visual Studio 中的所有執行個體，然後重新啟動的專案正努力解決這個問題。 如果無法解決問題，請變更通訊埠編號。 以滑鼠右鍵按一下專案檔，然後按一下 屬性。 選取**Web**索引標籤，然後變更 連接埠號碼**專案 Url**文字方塊。

### <a name="error-locating-sql-server-instance"></a>搜尋 SQL Server 執行個體時發生錯誤

錯誤訊息：

> 建立與 SQL Server　的連線時，發生與網路相關的錯誤或是執行個體特有的錯誤。 找不到或無法存取伺服器。 確認執行個名稱是否正確，以及 SQL Server 是否設定為允許遠端連線 (提供者：SQL 網路介面，錯誤：26 - 搜尋指定的伺服器/執行個體時發生錯誤)


方案

檢查連接字串。 如果您已經手動刪除資料庫，變更建構字串的資料庫名稱。

> [!div class="step-by-step"]
> [上一步](implementing-inheritance-with-the-entity-framework-in-an-asp-net-mvc-application.md)
