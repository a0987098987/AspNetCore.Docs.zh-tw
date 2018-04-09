---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 進階 MVC Web 應用程式 (10-10) 的 Entity Framework 案例 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 277503b65d9b75a9d3cc05538d5327f9367f45e0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>MVC Web 應用程式 (10-10) 的進階的實體架構案例
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中您會實作儲存機制和工作模式的單位。 本教學課程涵蓋下列主題：

- 執行原始的 SQL 查詢。
- 執行無追蹤查詢。
- 檢查查詢傳送至資料庫。
- 使用 proxy 類別。
- 停用自動偵測的變更。
- 停用驗證時儲存變更。
- [錯誤和工作的方法](#errors)

這些主題的大多數時間，您將使用您已經建立的網頁。 若要執行大量更新使用原始的 SQL 中，您將建立新的頁面，以更新資料庫中的所有課程的信用額度的數目：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

並使用無追蹤查詢會將新的驗證邏輯加入至部門編輯頁面：

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>執行原始的 SQL 查詢

Entity Framework 程式碼的第一個 API 包含可讓您將直接對資料庫的 SQL 命令的方法。 下列選項可供您選擇：

- 針對傳回實體類型的查詢使用 `DbSet.SqlQuery` 方法。 傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤對資料庫內容所除非您關閉追蹤。 (請參閱下一節有關`AsNoTracking`方法。)
- 使用`Database.SqlQuery`方法的傳回類型不是實體的查詢。 即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。
- 使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非查詢命令。

使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。 它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。 但有例外狀況時，您需要執行特定 SQL 查詢，以手動方式建立，而且這些方法可讓您處理這類例外狀況。

如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。 執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。 在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。

### <a name="calling-a-query-that-returns-entities"></a>呼叫查詢會傳回實體

假設您想要`GenericRepository`類別，以提供額外的篩選和排序的彈性，而不需要您使用其他方法建立衍生的類別。 達成此目標的其中一種方式是加入接受 SQL 查詢的方法。 您可以指定任何一種篩選或排序您想在控制站，例如`Where`子句的聯結或子查詢而定。 本節中，您會看到如何實作這種方法。

建立`GetWithRawSql`方法加入下列程式碼加入*GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

在*CourseController.cs*，呼叫新的方法從`Details`方法，如下列範例所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

在此情況下您可能使用`GetByID`方法，但您正在使用`GetWithRawSql`方法可讓您確認`GetWithRawSQL`方法運作。

執行驗證的選取查詢運作的詳細資料頁面 (選取**課程** 索引標籤，然後**詳細資料**一個課程)。

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>呼叫查詢傳回其他類型的物件

先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。 在程式碼*HomeController.cs*使用 LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

假設您想要撰寫的程式碼會擷取直接在 SQL，而不是使用 LINQ 中的此資料。 若要執行您要執行查詢，以傳回實體物件以外的項目，這表示您需要使用`Database.SqlQuery`方法。

在*HomeController.cs*，取代中的 LINQ 陳述式`About`方法取代下列程式碼：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

執行 「 關於 」 頁面。 它會顯示與之前相同的資料。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>呼叫更新查詢

假設 Contoso 大學系統管理員想要能夠執行大量變更，在資料庫中，例如變更的每個課程信用額度的數目。 如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。 在本節中，您將實作網頁，可讓使用者能夠指定所要依據變更的所有課程，信用額度數目的因素，您會變更執行 SQL`UPDATE`陳述式。 網頁看起來將如下圖所示：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

在上一個教學課程中您必須使用泛型的儲存機制讀取及更新`Course`中的實體`Course`控制站。 這個大量更新作業中，您需要建立新的儲存機制方法不是泛型的儲存機制中。 若要這樣做，您將建立專用`CourseRepository`類別衍生自`GenericRepository`類別。

在*DAL*資料夾中，建立*CourseRepository.cs* ，並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

在*UnitOfWork.cs*，變更`Course`從儲存機制類型`GenericRepository<Course>`至 `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

在*CourseContoller.cs*，新增`UpdateCourseCredits`方法：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

這個方法會使用兩個`HttpGet`和`HttpPost`。 當`HttpGet``UpdateCourseCredits`方法執行`multiplier`變數將會是 null，而且檢視會顯示空白的文字方塊和 [提交] 按鈕，在上圖所示。

當**更新**按鈕和`HttpPost`方法執行`multiplier`必須在文字方塊中輸入的值。 然後程式碼呼叫的儲存機制`UpdateCourseCredits`方法，傳回的數目會受到影響的資料列，而該值會儲存在`ViewBag`物件。 當檢視接收中的受影響資料列數目`ViewBag`物件，它會顯示該數字，而不是在文字方塊中，並提交 按鈕，如下圖所示：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

中建立檢視*Views\Course*更新課程信用額度頁面的資料夾：

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

在*Views\Course\UpdateCourseCredits.cshtml*，範本程式碼取代為下列程式碼：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:50205/Course/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。 在文字方塊中輸入數目：

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

按一下 [更新] 。 您會看到受影響的資料列數目：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

按一下 [回到清單]，以查看課程與已修訂學分數的清單。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

如需原始的 SQL 查詢的詳細資訊，請參閱[原始的 SQL 查詢](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework 小組部落格上。

## <a name="no-tracking-queries"></a>不追蹤查詢

當資料庫內容擷取資料庫資料列，並建立代表的實體物件時，依預設它會追蹤的是否與資料庫中的實體記憶體中保持同步。 記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。 這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。

您可以指定內容是否會追蹤查詢的實體物件，使用`AsNoTracking`方法。 您會想要進行這項操作的常見案例包括下列情況：

- 此查詢會擷取這類大量的資料，關閉追蹤可能會大幅提升效能。
- 您想要將實體附加以更新，但您稍早擷取同一個實體用於不同用途。 由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。 若要避免這種情況的一種方式為使用`AsNoTracking`與前面的查詢選項。

在本節中，您將實作說明第二個案例的商務邏輯。 具體來說，您將會強制執行商務規則，表示講師不能多個部門的系統管理員。

在*DepartmentController.cs*，加入新的方法，您可以從呼叫`Edit`和`Create`方法，以確定沒有兩個部門有相同的系統管理員：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

將程式碼中的加入`try`區塊`HttpPost``Edit`方法來呼叫此新方法，如果不有任何驗證錯誤。 `try`區塊現在看起來像下列的範例：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

執行部門編輯網頁，並嘗試將部門的系統管理員變更為講師人員已經是不同的部門的系統管理員。 您會收到預期的錯誤訊息：

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

現在執行部門編輯頁面一次，此時間變更**預算**數量。 當您按一下**儲存**，您會看到錯誤頁面：

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

例外狀況錯誤訊息是 「`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`「 這是因為以下一連串事件：

- `Edit`方法呼叫`ValidateOneAdministratorAssignmentPerInstructor`方法，擷取所有具有其系統管理員身分 Kim Abercrombie 的部門。 會導致要讀取的英文部門。 因為這是正在編輯的部門，則會不報告任何錯誤。 由於這個讀取作業，不過，英文 department 實體已從資料庫讀取現在正在追蹤的資料庫內容。
- `Edit`方法嘗試設定`Modified`英文旗標 department 實體建立 MVC 模型繫結器，但是會失敗，因為內容已經在追蹤實體的英文版的部門。

這個問題的解決方案之一是讓內容追蹤記憶體中的部門由驗證查詢所擷取的實體。 不沒有這麼做，因為您將不會更新此實體或可從它在記憶體中快取獲益的方式一次讀取任何缺點。

在*DepartmentController.cs*，請在`ValidateOneAdministratorAssignmentPerInstructor`方法，指定沒有追蹤中，如下列所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

重複嘗試編輯**預算**部門的數量。 目前作業成功，並將站台傳回如預期般部門索引頁，顯示已修訂的預算值。

## <a name="examining-queries-sent-to-the-database"></a>檢查查詢傳送至資料庫

有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。 若要這樣做，您可以檢查偵錯工具中的查詢變數或將查詢稱為`ToString`方法。 再試一次時，您將查看簡單查詢，並查看您新增這類 eager 載入、 篩選和排序選項，它會發生什麼事。

在*控制器/CourseController*，取代`Index`方法取代下列程式碼：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

現在中設定中斷點*GenericRepository.cs*上`return query.ToList();`和`return orderBy(query).ToList();`陳述式的`Get`方法。 在偵錯模式中執行專案並選取課程索引頁面。 當程式碼到達中斷點時，檢查`query`變數。 您會看到傳送至 SQL Server 的查詢。 它是簡單`Select`陳述式：

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

查詢可能太長，無法在 Visual Studio 中偵錯視窗中顯示。 若要查看整個查詢，您可以複製變數的值，並將它貼到文字編輯器：

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

現在您將新增下拉式選單課程索引頁，讓使用者可以篩選特定部門。 您將項目，所排序的課程，您將指定的積極式載入`Department`導覽屬性。 在*CourseController.cs*，取代`Index`方法取代下列程式碼：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

此方法接收的下拉式清單中選取的值`SelectedDepartment`參數。 如果選取任何項目，這個參數會是 null。

A`SelectList`集合，其中包含所有部門傳遞至檢視的下拉式清單。 參數傳遞至`SelectList`建構函式指定的值欄位名稱、 文字欄位名稱，以及選取的項目。

如`Get`方法`Course`儲存機制、 程式碼會指定篩選條件運算式、 排序次序，以及載入 eager`Department`導覽屬性。 篩選運算式永遠都會傳回`true`如果不會選取下拉式清單中 (也就是`SelectedDepartment`為 null)。

在*Views\Course\Index.cshtml*之前開啟,`table`標記中，加入下列程式碼，建立下拉式清單及提交按鈕：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

與仍設定的中斷點`GenericRepository`類別，執行課程索引頁面。 繼續透過程式碼叫用中斷點時，前兩個時間如此頁面會顯示在瀏覽器。 從下拉式清單中選取部門並按一下**篩選**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

這次下拉式清單的部門查詢就是第一個中斷點。 略過，並檢視`query`變數在下一次程式碼中斷點時若要查看哪些`Course`查詢現在看起來像。 您會看到類似如下：

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

您可以看到查詢，現在是`JOIN`載入的查詢`Department`資料連同`Course`資料，以及它包含`WHERE`子句。

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>使用 Proxy 類別

當 Entity Framework 建立的實體執行個體 （例如，當您執行查詢時） 時，它通常會建立這些動態產生的衍生型別，可做為實體的 proxy 的執行個體。 此 proxy 會覆寫要插入在屬性經過存取時，自動執行動作的攔截程序之實體的某些虛擬屬性。 比方說，這項機制用來支援消極式載入的關聯性。

大部分的情況下您不需要注意這項使用的 proxy，但有例外狀況：

- 在某些情況下您可能想要防止 Entity Framework 建立 proxy 執行個體。 例如，序列化非 proxy 執行個體可能會更有效率序列化 proxy 執行個體。
- 當您具現化的實體類別使用`new`運算子，就無法取得 proxy 執行個體。 這表示您不先取得功能，例如消極式載入和自動變更追蹤。 這通常是好;您通常不需要延遲載入，因為您要建立新的實體不在資料庫中，而且通常不需要變更追蹤，如果您要明確地標示為實體`Added`。 不過，如果您需要消極式載入，而您需要變更追蹤，您可以建立新的實體執行個體使用的 proxy`Create`方法`DbSet`類別。
- 您可能想要從的 proxy 型別取得實際的實體類型。 您可以使用`GetObjectType`方法`ObjectContext`類別取得的 proxy 型別執行個體的實際實體類型。

如需詳細資訊，請參閱[使用 Proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) Entity Framework 小組部落格上。

## <a name="disabling-automatic-detection-of-changes"></a>停用自動偵測的變更

Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。 當查詢或附加的實體時，會儲存原始值。 會導致自動變更偵測的一些方法如下：

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

如果您追蹤的實體數量龐大，而且您在迴圈中呼叫其中一種方法多次，可能會暫時停用自動變更偵測使用收到的顯著效能改善[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)屬性。 如需詳細資訊，請參閱[自動偵測變更](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)。

## <a name="disabling-validation-when-saving-changes"></a>停用驗證時儲存變更

當您呼叫`SaveChanges`方法，依預設 Entity Framework 中所有已變更實體的所有屬性的資料會先驗證更新資料庫。 如果您已更新大量實體且您已驗證資料，這項工作不需要確定儲存的程序所做的變更會藉由暫時關閉驗證需要較少的時間。 您可以使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)屬性。 如需詳細資訊，請參閱[驗證](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)。

## <a name="summary"></a>總結

如此即完成這一系列的教學課程，在 ASP.NET MVC 應用程式中使用 Entity Framework。 Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

如需如何建置它之後部署 web 應用程式的詳細資訊，請參閱[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521.aspx)MSDN Library 中。

如需有關 MVC、 驗證和授權，例如其他主題，請參閱[MVC 建議資源](../../getting-started/recommended-resources-for-mvc.md)。

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>感謝

- Tom Dykstra 寫入此教學課程中的原始版本，並是資深的開發寫入器上的 Microsoft Web 平台和工具的內容團隊。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 共同撰寫本教學課程中，因此未更新 EF 5 和 MVC 4 工作的絕大部分。 Rick 是將焦點放在 Azure 和 MVC Microsoft 資深程式寫入器。
- [Rowan Miller](http://www.romiller.com)和 Entity Framework 小組的其他成員協助程式碼檢閱，並協助偵錯移轉，我們已更新本教學課程的 EF 5 時，會發生的許多問題。

## <a name="vb"></a>VB

時原先產生本教學課程，我們會提供 C# 和 VB 專案版本的下載已完成。 透過這項更新中，我們會提供 C# 可下載專案中每一章，讓您更輕鬆地在系列，但由於時間限制和其他優先順序，我們沒有做的 VB 的任何地方開始 如果您建立使用這些教學課程將 VB 專案，願意，與其他人共用，請讓我們知道。

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>錯誤和因應措施

### <a name="cannot-createshadow-copy"></a>無法建立或陰影複製

錯誤訊息：

*無法建立或陰影複製 'DotNetOpenAuth.OpenId' 時已存在的檔案。*

解決方案:

等候數秒鐘，並重新整理頁面。

### <a name="update-database-not-recognized"></a>更新資料庫無法辨識

錯誤訊息：

*'Update-database' 詞彙無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。請檢查名稱拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。*(從*`Update-Database`* PMC 命令。)

解決方案:

結束 Visual Studio。 重新開啟專案，然後再試一次。

### <a name="validation-failed"></a>驗證失敗

錯誤訊息：

*一個或多個實體的驗證失敗。請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。* (從*`Update-Database`* PMC 命令。)

解決方案:

發生此問題的其中一個原因是驗證錯誤時`Seed`方法執行。 請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)如需有關偵錯秘訣`Seed`方法。

### <a name="http-50019-error"></a>HTTP 500.19 錯誤

錯誤訊息：

*無法存取 HTTP 錯誤 500.19-內部伺服器錯誤所要求的頁面，因為該頁面的相關的組態資料無效。*

解決方案:

您可以取得此錯誤的其中一種方式是方案的從多個複本時，每個使用相同的連接埠號碼。 您通常可以結束 Visual Studio 中的所有執行個體，然後重新啟動專案上的工作，以解決這個問題。 如果無法解決問題，請變更通訊埠編號。 以滑鼠右鍵按一下專案檔，然後按一下 屬性。 選取**Web**索引標籤，然後變更 連接埠號碼**專案 Url**文字方塊。

### <a name="error-locating-sql-server-instance"></a>搜尋 SQL Server 執行個體時發生錯誤

錯誤訊息：

*建立 SQL Server 的連接時發生網路相關或執行個體特定錯誤。找不到或無法存取伺服器。確認執行個體名稱正確，且 SQL Server 設定為允許遠端連接。(提供者： SQL 網路介面，錯誤： 26-尋找指定時發生錯誤伺服器/執行個體)*

解決方案:

請檢查連接字串。 如果您已經手動刪除資料庫，變更建構字串的資料庫名稱。

> [!div class="step-by-step"]
> [上一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [下一頁](building-the-ef5-mvc4-chapter-downloads.md)
