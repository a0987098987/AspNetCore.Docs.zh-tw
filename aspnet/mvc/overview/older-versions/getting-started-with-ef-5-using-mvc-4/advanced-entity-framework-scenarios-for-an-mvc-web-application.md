---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
title: 進階 MVC Web 應用程式 (10 小時，共 10) 的 Entity Framework 案例 |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 64906a1d-f734-41cf-9615-ee95f8740996
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/advanced-entity-framework-scenarios-for-an-mvc-web-application
msc.type: authoredcontent
ms.openlocfilehash: 5a75d85140a40660314ab267fdd74a8058d791fc
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832671"
---
<a name="advanced-entity-framework-scenarios-for-an-mvc-web-application-10-of-10"></a>進階的 Entity Framework 案例，MVC Web 應用程式 (10 小時，共 10)
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中，您實作的儲存機制和工作單元模式。 本教學課程涵蓋下列主題：

- 執行原始的 SQL 查詢。
- 執行無追蹤查詢。
- 檢查查詢傳送至資料庫。
- 使用 proxy 類別。
- 停用自動的偵測變更。
- 停用驗證時儲存變更。
- [錯誤和因應措施](#errors)

對於大部分的這些主題，您將使用您已建立的頁面。 若要執行大量更新使用原始 SQL 中，您將建立新的頁面，以更新資料庫中的所有課程的學分數：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image1.png)

並使用無追蹤查詢會將新的驗證邏輯新增至 Department 編輯 頁面：

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image2.png)

## <a name="performing-raw-sql-queries"></a>原始 SQL 查詢

Entity Framework 程式碼的第一個 API 包含可讓您的 SQL 命令直接傳遞至資料庫的方法。 下列選項可供您選擇：

- 針對傳回實體類型的查詢使用 `DbSet.SqlQuery` 方法。 傳回的物件必須是所預期的類型`DbSet`物件，而且它們會自動追蹤的資料庫內容除非您關閉追蹤。 (請參閱下一節，關於`AsNoTracking`方法。)
- 使用`Database.SqlQuery`方法的傳回類型不是實體的查詢。 即使您使用這個方法來擷取實體類型，資料庫內容也不會追蹤傳回的資料。
- 使用[Database.ExecuteSqlCommand](https://msdn.microsoft.com/library/gg679456(v=vs.103).aspx)非查詢命令。

使用 Entity Framework 的優點之一，是它可避免將程式碼繫結至太接近儲存資料之特定方法的位置。 它可透過產生 SQL 查詢和命令來達成此目的，同時這也可讓您不必自行撰寫。 但有例外情況時您需要執行特定 SQL 查詢您以手動方式建立，而且這些方法可讓您處理這類例外狀況。

如同在 Web 應用程式中執行 SQL 命令一樣，您必須採取一些預防措施，以保護您的網站免於遭受 SQL 插入式攻擊。 執行這項操作的方法之一是使用參數化查詢，以確定網頁所提交的字串無法解譯為 SQL 命令。 在本教學課程中，您會在將使用者輸入整合到查詢時，使用參數化查詢。

### <a name="calling-a-query-that-returns-entities"></a>呼叫查詢，傳回的實體

假設您想`GenericRepository`類別，以提供額外的篩選和排序的彈性，而不需要您以其他方法，建立衍生的類別。 為了達到此目標的一個方式是新增方法以接受 SQL 查詢。 您可以指定任何種類的篩選或排序您想要在控制器中，例如`Where`取決於聯結或子查詢的子句。 在本節中，您會看到如何實作這種方法。

建立`GetWithRawSql`加上下列程式碼的方法*GenericRepository.cs*:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample1.cs)]

在  *CourseController.cs*，呼叫新方法，從`Details`方法，如下列範例所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample2.cs)]

在此情況下您可以使用`GetByID`方法，但您使用`GetWithRawSql`方法，以確認`GetWithRawSQL`方法運作。

執行詳細資料頁面，即可確認 select 查詢的運作 (選取**課程**索引標籤，然後**詳細資料**針對一個課程)。

![Course_Details_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image3.png)

### <a name="calling-a-query-that-returns-other-types-of-objects"></a>呼叫查詢，傳回其他類型的物件

先前您已針對顯示每個註冊日期之學生數目的 About 頁面，建立學生統計資料方格。 在程式碼*HomeController.cs*使用 LINQ:

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample3.cs)]

假設您想要撰寫的程式碼會擷取直接在 SQL，而不是使用 LINQ 中的這項資料。 若要執行，您需要執行的查詢會傳回實體物件以外的項目，這表示您需要使用`Database.SqlQuery`方法。

在  *HomeController.cs*，取代中的 LINQ 陳述式`About`為下列程式碼的方法：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample4.cs)]

執行 [關於] 頁面。 它會顯示與之前相同的資料。

![About_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image4.png)

### <a name="calling-an-update-query"></a>呼叫更新查詢

假設 Contoso 大學的系統管理員想要能夠在資料庫中，例如變更每個課程的學分數執行大量變更。 如果該大學有大量的課程，擷取全部課程作為實體並個別進行變更的效率不佳。 在本節中，您將實作網頁，可讓使用者指定要變更所有課程的學分數的因數，您會變更執行 SQL`UPDATE`陳述式。 網頁看起來將如下圖所示：

![Update_Course_Credits_initial_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image5.png)

在上一個教學課程中您用來讀取和更新一般儲存機制`Course`中的實體`Course`控制站。 此大量更新作業中，您需要建立新的儲存機制方法不在一般的儲存機制中。 若要這樣做，您將建立的專用`CourseRepository`類別衍生自`GenericRepository`類別。

在  *DAL*資料夾中，建立*CourseRepository.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample5.cs)]

在  *UnitOfWork.cs*，變更`Course`存放庫類型從`GenericRepository<Course>`至 `CourseRepository:`

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample6.cs)]

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample7.cs)]

在  *CourseContoller.cs*，新增`UpdateCourseCredits`方法：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample8.cs)]

這個方法會使用兩個`HttpGet`和`HttpPost`。 當`HttpGet``UpdateCourseCredits`方法執行`multiplier`變數將會是 null，而且此檢視會顯示空白的文字方塊和提交按鈕，，如上圖所示。

當**更新**按一下按鈕時，`HttpPost`方法執行`multiplier`必須在文字方塊中輸入的值。 程式碼接著會呼叫儲存機制`UpdateCourseCredits`方法，以傳回的數目會受到影響的資料列，且該值會儲存在`ViewBag`物件。 當檢視收到的受影響資料列數目`ViewBag`物件，它會顯示該數字，而不是文字方塊中，並提交 按鈕，如下圖所示：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image6.png)

建立中的檢視*Views\Course*更新課程學分數頁面的資料夾：

![Add_View_dialog_box_for_Update_Course_Credits](https://asp.net/media/2578203/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Add_View_dialog_box_for_Update_Course_Credits.png)

在  *Views\Course\UpdateCourseCredits.cshtml*，以下列程式碼取代範本程式碼：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample9.cshtml)]

藉由選取 [課程]  索引標籤，然後將 "/UpdateCourseCredits" 新增至瀏覽器位址列中的 URL 結尾 (例如：`http://localhost:50205/Course/UpdateCourseCredits`)，以執行 `UpdateCourseCredits` 方法。 在文字方塊中輸入數目：

![Update_Course_Credits_initial_page_with_2_entered](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image7.png)

按一下 [更新] 。 您會看到受影響的資料列數目：

![Update_Course_Credits_rows_affected_page](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image8.png)

按一下 [回到清單]，以查看課程與已修訂學分數的清單。

![Courses_Index_page_showing_revised_credits](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image9.png)

如需原始 SQL 查詢的詳細資訊，請參閱[原始 SQL 查詢](https://blogs.msdn.com/b/adonet/archive/2011/02/04/using-dbcontext-in-ef-feature-ctp5-part-10-raw-sql-queries.aspx)Entity Framework 小組部落格上。

## <a name="no-tracking-queries"></a>不追蹤的查詢

當資料庫內容擷取的資料庫資料列，並建立代表他們的實體物件時，依預設它會追蹤的是否在記憶體中的實體與資料庫中都保持同步。 記憶體中的資料所扮演的角色是一個快取，並會在您更新實體時使用。 這個快取通常在 Web 應用程式當中是不需要的，因為內容執行個體通常壽命都很短 (每次要求都會建立一個新的並進行處置)，並且通常讀取實體的內容都會在實體重新獲得利用前遭到處置。

您可以指定是否將內容追蹤查詢的實體物件的使用`AsNoTracking`方法。 您會想要進行這項操作的常見案例包括下列情況：

- 此查詢會擷取這類大量的資料，關閉追蹤可能會大幅提升效能。
- 您想要將實體附加才能加以更新，但您稍早針對不同用途擷取相同的實體。 由於實體已由資料庫內容進行追蹤，您無法連結到您想要變更的實體。 若要避免這種情況的一個方式是使用`AsNoTracking`選項與先前的查詢。

在本節中，您將實作商務邏輯，說明這些案例的第二個。 具體來說，您將會強制執行商務規則，表示一名講師，不能超過一個部門的系統管理員。

在  *DepartmentController.cs*，新增新方法，您可以從呼叫`Edit`和`Create`方法，以確定沒有兩個部門都有相同的系統管理員：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample10.cs)]

將程式碼中的加入`try`區塊`HttpPost``Edit`方法來呼叫此新方法，如果不有任何驗證錯誤。 `try`區塊現在看起來如下列範例所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample11.cs?highlight=9-12)]

執行 Department 編輯 頁面，然後再次嘗試變更講師人員已經是不同的部門的系統管理員的部門的系統管理員。 您得到預期的錯誤訊息：

![Department_Edit_page_with_duplicate_administrator_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image10.png)

現在執行 Department [編輯] 頁面一次，此時間變更**預算**數量。 當您按一下 **儲存**，您會看到錯誤頁面：

![Department_Edit_page_with_object_state_manager_error_message](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image11.png)

例外狀況錯誤訊息是 「`An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key.`"發生此問題。 因為以下一連串事件：

- `Edit`方法呼叫`ValidateOneAdministratorAssignmentPerInstructor`方法，它會擷取所有具有其系統管理員身分的 Kim Abercrombie 的部門。 會導致英文部門時讀取。 因為這是正在編輯的部門時，不會報告錯誤。 此讀取作業，不過，已從資料庫讀取的英文部門實體現在正在追蹤的資料庫內容。
- `Edit`方法會嘗試設定`Modified`英文版上的旗標建立部門實體，這是 MVC 模型繫結器，但失敗因為 kontext již sleduje 英文部門實體。

此問題的解決方案之一是將內容從追蹤擷取驗證查詢的記憶體中的部門實體。 執行此操作，因為您不會更新此實體，或可從中獲益於記憶體中快取的方式一次讀取沒有缺點。

在  *DepartmentController.cs*，請在`ValidateOneAdministratorAssignmentPerInstructor`方法中，不指定任何追蹤，如下列所示：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample12.cs?highlight=4)]

重複您嘗試編輯**預算**部門的數量。 這次此作業成功，並在網站傳回如預期般至 Departments [索引] 頁面，顯示已修訂的預算值。

## <a name="examining-queries-sent-to-the-database"></a>檢查傳送至資料庫的查詢

有時能夠看到傳送至資料庫的實際 SQL 查詢很有幫助。 若要這樣做，您可以檢查偵錯工具中的查詢變數，或呼叫的查詢`ToString`方法。 若要這麼做時，您將看看一個簡單的查詢，並查看當您加入這類的積極式載入、 篩選和排序選項，它會發生什麼事。

在 *控制器/CourseController*，取代`Index`為下列程式碼的方法：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample13.cs?highlight=3)]

現在中設定中斷點*GenericRepository.cs*上`return query.ToList();`並`return orderBy(query).ToList();`陳述式的`Get`方法。 在偵錯模式中執行專案，然後選取 [課程索引] 頁面。 當程式碼到達中斷點時，檢查`query`變數。 您會看到傳送到 SQL Server 的查詢。 它是一項簡單`Select`陳述式：

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample14.json)]

![](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image12.png)

查詢也可以在 Visual Studio 中偵錯視窗中顯示太長。 若要查看整個查詢，您可以複製變數的值，並將它貼到文字編輯器：

![Copy_value_of_variable_in_debug_mode](https://asp.net/media/2578239/Windows-Live-Writer_Advanced-Entity-Framework-Scenarios-for-_CEF8_Copy_value_of_variable_in_debug_mode_0902a2b1-b799-47a6-9b4b-f266c79d83c1.png)

現在您將新增下拉式清單至課程索引 頁面，讓使用者可以篩選特定的部門。 您將項目，所排序的課程，並指定積極式載入`Department`導覽屬性。 在  *CourseController.cs*，取代`Index`為下列程式碼的方法：

[!code-csharp[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample15.cs)]

此方法接收的下拉式清單中選取的值`SelectedDepartment`參數。 如果未選取，則會有此參數是 null。

A`SelectList`集合，其中包含所有部門的下拉式清單，會傳遞至檢視。 參數傳遞給`SelectList`建構函式指定的值欄位名稱、 文字欄位名稱，以及選取的項目。

針對`Get`方法`Course`存放庫中，程式碼會指定篩選條件運算式、 排序次序，以及積極式載入`Department`導覽屬性。 篩選運算式永遠都會傳回`true`如果不會選取下拉式清單中 (也就是`SelectedDepartment`為 null)。

在  *Views\Course\Index.cshtml*，開啟之前，立即`table`標記中加入下列程式碼，建立下拉式清單和提交按鈕：

[!code-cshtml[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample16.cshtml)]

使用中的中斷點仍`GenericRepository`類別，執行課程索引頁面。 繼續透過程式碼叫用中斷點的行，第一次兩次，以便在瀏覽器中顯示頁面。 從下拉式清單中選取一個部門，然後按一下**篩選**:

![Course_Index_page_with_department_selected](advanced-entity-framework-scenarios-for-an-mvc-web-application/_static/image13.png)

這次的第一個中斷點會部門查詢的下拉式清單。 略過，並檢視`query`變數在下一次程式碼觸達中斷點時若要查看`Course`現在看起來就像查詢。 您會看到類似如下：

[!code-json[Main](advanced-entity-framework-scenarios-for-an-mvc-web-application/samples/sample17.json)]

您可以看到查詢現在`JOIN`載入的查詢`Department`資料連同`Course`資料，且其中包含`WHERE`子句。

<a id="proxyclasses"></a>

## <a name="working-with-proxy-classes"></a>使用 Proxy 類別

當 Entity Framework 建立的實體執行個體 （例如，當您執行查詢時） 時，它通常會建立它們的動態產生的衍生型別，做為實體的 proxy 執行個體。 此 proxy 會覆寫要插入存取屬性時，就會自動執行動作的攔截程序之實體的某些虛擬屬性。 比方說，這項機制用來支援關聯性的消極式載入。

大多時候您不需要注意這項使用的 proxy，但有例外狀況：

- 在某些情況下，您可能想要防止 Entity Framework 建立 proxy 執行個體。 比方說，序列化非 proxy 執行個體可能會比序列化 proxy 執行個體更有效率。
- 當您具現化實體類別使用`new`運算子，您不會取得 proxy 執行個體。 這表示您未獲得的功能，例如消極式載入和自動變更追蹤。 這通常是好;您通常不需要消極式載入，因為您要建立新的實體不在資料庫中，而且您通常不需要變更追蹤，如果您要明確地標示為實體`Added`。 不過，如果您需要消極式載入，而且您需要變更追蹤，您可以建立新的實體執行個體使用的 proxy`Create`方法的`DbSet`類別。
- 您可以從 proxy 型別取得實際的實體類型。 您可以使用`GetObjectType`方法的`ObjectContext`類別，以取得 proxy 型別執行個體的實際實體類型。

如需詳細資訊，請參閱 <<c0> [ 使用 Proxy](https://blogs.msdn.com/b/adonet/archive/2011/02/02/using-dbcontext-in-ef-feature-ctp5-part-8-working-with-proxies.aspx) Entity Framework 小組部落格上。

## <a name="disabling-automatic-detection-of-changes"></a>停用自動的偵測變更

Entity Framework 藉由比較實體的目前值與原始值，判斷實體如何變更 (以及因此需要將哪些更新傳送至資料庫)。 查詢或附加實體時，會儲存原始的值。 會導致自動變更偵測的一些方法如下：

- `DbSet.Find`
- `DbSet.Local`
- `DbSet.Remove`
- `DbSet.Add`
- `DbSet.Attach`
- `DbContext.SaveChanges`
- `DbContext.GetValidationErrors`
- `DbContext.Entry`
- `DbChangeTracker.Entries`

如果您正在追蹤大量的實體，而且您在迴圈中呼叫其中一個方法許多次，您可能會暫時關閉自動變更偵測使用收到的顯著效能改善[AutoDetectChangesEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.autodetectchangesenabled(VS.103).aspx)屬性。 如需詳細資訊，請參閱 <<c0> [ 自動偵測變更](https://blogs.msdn.com/b/adonet/archive/2011/02/06/using-dbcontext-in-ef-feature-ctp5-part-12-automatically-detecting-changes.aspx)。

## <a name="disabling-validation-when-saving-changes"></a>停用驗證時儲存變更

當您呼叫`SaveChanges`方法，預設的 Entity Framework 會驗證所有屬性的所有變更的實體中的資料更新資料庫之前。 如果您已更新大量的實體和您已驗證資料，這項工作不需要，而且您可以讓儲存的程序所做的變更會藉由暫時關閉驗證需要較少的時間。 您可以執行的方法使用[ValidateOnSaveEnabled](https://msdn.microsoft.com/library/system.data.entity.infrastructure.dbcontextconfiguration.validateonsaveenabled(VS.103).aspx)屬性。 如需詳細資訊，請參閱 <<c0> [ 驗證](https://blogs.msdn.com/b/adonet/archive/2010/12/15/ef-feature-ctp5-validation.aspx)。

## <a name="summary"></a>總結

如此即完成本系列的教學課程使用 Entity Framework 的 ASP.NET MVC 應用程式中。 其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

如需如何在您已經建置它後，部署您的 web 應用程式的詳細資訊，請參閱[ASP.NET 部署內容對應](https://msdn.microsoft.com/library/bb386521.aspx)MSDN Library 中。

如需其他相關 MVC，例如驗證和授權，主題的詳細資訊[MVC 建議資源](../../getting-started/recommended-resources-for-mvc.md)。

<a id="acknowledgments"></a>

## <a name="acknowledgments"></a>感謝

- Tom Dykstra 撰寫本教學課程中的原始版本，而是擔任 Microsoft Web Platform and Tools 內容小組的資深程式設計作家。
- [Rick Anderson](https://blogs.msdn.com/b/rickandy/) (twitter [ @RickAndMSFT ](http://twitter.com/RickAndMSFT)) 共同撰寫本教學課程和大部分的工作更新的 EF 5 和 MVC 4 的做法一樣。 Rick 是將焦點放在 Azure 和 MVC 的 microsoft 的資深程式設計作家。
- [Rowan Miller](http://www.romiller.com)和 Entity Framework 小組的其他成員協助進行程式碼檢閱，並協助偵錯移轉，我們已更新本教學課程的 EF 5 時發生的許多問題。

## <a name="vb"></a>VB

時原先產生的教學課程中，我們會提供 C# 和 VB 專案版本的已完成的下載。 透過這項更新中，我們會提供 C# 可下載專案中的每一章，讓您更輕鬆地開始任何地方在系列中，但由於時間限制和其他優先事項，我們沒有可用於 VB 如果您建置使用這些教學課程的 VB 專案，並願意與其他人分享，請讓我們知道。

<a id="errors"></a>

## <a name="errors-and-workarounds"></a>錯誤和因應措施

### <a name="cannot-createshadow-copy"></a>無法建立/陰影複製

錯誤訊息：

*無法建立/陰影複製 'DotNetOpenAuth.OpenId' 已經存在的檔案。*

解決方案:

等候幾秒鐘的時間，然後重新整理頁面。

### <a name="update-database-not-recognized"></a>更新資料庫無法辨識

錯誤訊息：

*詞彙 更新資料庫 ' 無法辨識為 cmdlet、 函式、 指令碼檔案或可執行程式的名稱。請檢查名稱的拼字，或如果包含路徑的話，確認路徑正確，然後再試一次。*(從*`Update-Database`* PMC 命令。)

解決方案:

結束 Visual Studio。 重新開啟專案，並再試一次。

### <a name="validation-failed"></a>驗證失敗

錯誤訊息：

*一個或多個實體的驗證失敗。請參閱 'EntityValidationErrors' 屬性，如需詳細資訊。* (從*`Update-Database`* PMC 命令。)

解決方案:

此問題的其中一個原因是驗證錯誤時`Seed`方法執行。 請參閱[植入及偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx)如需有關偵錯秘訣`Seed`方法。

### <a name="http-50019-error"></a>HTTP 500.19 錯誤

錯誤訊息：

*無法存取 HTTP 錯誤 500.19-內部伺服器錯誤 」 要求的頁面，因為頁面的相關的組態資料無效。*

解決方案:

您可以取得此錯誤的一個方式是解決方案的從多個複本，每個使用相同的連接埠號碼。 您通常可以結束 Visual Studio 中的所有執行個體，然後重新啟動專案上的工作，藉此解決這個問題。 如果這個方式沒有用，請嘗試變更連接埠號碼。 以滑鼠右鍵按一下專案檔，然後按一下 屬性。 選取  **Web**索引標籤，然後將變更的連接埠號碼**專案 Url**文字方塊。

### <a name="error-locating-sql-server-instance"></a>搜尋 SQL Server 執行個體時發生錯誤

錯誤訊息：

*建立 SQL Server 的連接時發生網路相關或執行個體特有的錯誤。找不到或無法存取伺服器。請確認執行個體名稱正確，且 SQL Server 設定為允許遠端連接。(提供者： SQL 網路介面，錯誤： 26-尋找指定時發生錯誤伺服器/執行個體)*

解決方案:

請檢查連接字串。 如果您已經手動刪除資料庫，請變更建構字串的資料庫名稱。

> [!div class="step-by-step"]
> [上一頁](implementing-the-repository-and-unit-of-work-patterns-in-an-asp-net-mvc-application.md)
> [下一頁](building-the-ef5-mvc4-chapter-downloads.md)
