---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：新增排序、 篩選和分頁與 ASP.NET MVC 應用程式中的 Entity Framework |Microsoft Docs
author: tdykstra
description: 在本教學課程中新增排序、 篩選和分頁功能**學生**索引頁面。 您也會建立簡單的群組頁面。
ms.author: riande
ms.date: 01/14/2019
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 1f18a15d39d58ffb4ac48cfccee6519d33294e85
ms.sourcegitcommit: 728f4e47be91e1c87bb7c0041734191b5f5c6da3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/22/2019
ms.locfileid: "54444191"
---
# <a name="tutorial-add-sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>教學課程：新增排序、 篩選和分頁與 Entity Framework 的 ASP.NET MVC 應用程式中

在 [先前的教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)，您實作的基本 CRUD 作業的 web 網頁的一組`Student`實體。 在本教學課程中新增排序、 篩選和分頁功能**學生**索引頁面。 您也會建立簡單的群組頁面。

下圖顯示當您完成時的頁面的外觀。 資料行標題是使用者可以按一下以依據該資料行排序的連結。 重覆按一下資料行標題，可切換遞增和遞減排序次序。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

在本教學課程中，您已：

> [!div class="checklist"]
> * 新增資料行排序連結
> * 新增搜尋方塊
> * 加入分頁
> * 建立 [關於] 頁面

## <a name="prerequisites"></a>必要條件

* [實作基本的 CRUD 功能](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)

## <a name="add-column-sort-links"></a>新增資料行排序連結

若要將排序新增至學生的 [索引] 頁面，您將會變更`Index`方法`Student`控制器並將程式碼加入`Student`索引檢視。

### <a name="add-sorting-functionality-to-the-index-method"></a>將排序功能新增至 Index 方法

- 在  *Controllers\StudentController.cs*，取代`Index`為下列程式碼的方法：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。 查詢字串值是由 ASP.NET MVC 提供，做為動作方法的參數。 （選擇性） 後面接著底線和字串"desc"來指定遞減順序為"Name"Date"的字串參數。 預設的排序次序為遞增。

第一次要求 [索引] 頁面時，沒有任何查詢字串。 以遞增順序顯示學生`LastName`，這是預設值中的 fallthrough 案例所建立`switch`陳述式。 使用者按一下資料行標題超連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。

這兩個`ViewBag`使用變數時，讓檢視可以使用適當的查詢字串值來設定資料行標題超連結：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

這些是三元陳述式。 第一個指定，如果`sortOrder`參數為 null 或空的`ViewBag.NameSortParm`應設為 「 名稱\_desc"，否則它應該設定為空字串。 這兩個陳述式讓檢視能夠設定資料行標題超連結，如下所示：

| 目前排序次序 | 姓氏超連結 | 日期超連結 |
| --- | --- | --- |
| 姓氏遞增 | descending | ascending |
| 姓氏遞減 | ascending | ascending |
| 日期遞增 | ascending | descending |
| 日期遞減 | ascending | ascending |

此方法會使用[LINQ to Entities](/dotnet/framework/data/adonet/ef/language-reference/linq-to-entities)指定排序所依據的資料行。 程式碼會建立<xref:System.Linq.IQueryable%601>變數之前`switch`陳述式，會修改它`switch`陳述式，並呼叫`ToList`方法之後`switch`陳述式。 當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。 查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToList`。 因此，此程式碼會產生之前不會執行單一查詢`return View`陳述式。

撰寫不同的 LINQ 陳述式，針對每個排序順序的替代方案，您可以動態地建立 LINQ 陳述式。 如需動態 LINQ 的資訊，請參閱[動態 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>將資料行標題超連結新增至學生 [索引] 檢視

1. 在  *Views\Student\Index.cshtml*，取代`<tr>`和`<th>`醒目提示的程式碼的標題資料列的項目：

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

   此程式碼使用中的資訊`ViewBag`屬性來設定超連結，以適當的查詢字串值。

2. 執行頁面，然後按一下**姓氏**並**註冊日期**資料行標題，以確認該排序運作。

   按一下 之後**姓氏**標題之下，學生以遞減顯示姓氏來排序。

## <a name="add-a-search-box"></a>新增搜尋方塊

若要新增至 Students [索引] 頁面的篩選，您會將文字方塊和提交按鈕新增至檢視，並進行對應的變更，在`Index`方法。 [文字] 方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。

### <a name="add-filtering-functionality-to-the-index-method"></a>將篩選功能新增至 Index 方法

- 在  *Controllers\StudentController.cs*，取代`Index`（變更已醒目提示） 為下列程式碼的方法：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

程式碼新增`searchString`參數來`Index`方法。 從將新增至 [索引] 檢視的文字方塊中接收搜尋字串值。 它也會新增`where`子句來選取其名字或姓氏包含搜尋字串的學生將 LINQ 陳述式。 陳述式，將它新增<xref:System.Linq.Queryable.Where%2A>子句執行只有當沒有要搜尋的值。

> [!NOTE]
> 在許多情況下，Entity Framework 實體集上，或是當做擴充方法在記憶體中的集合，可以呼叫相同的方法。 結果通常都相同，但在某些情況下可能會不同。
>
> 例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回空字串的零個資料列。 因此在範例程式碼 (將放`Where`內的陳述式`if`陳述式) 可確保您取得所有的 SQL Server 版本相同的結果。 此外，.NET Framework 實作的`Contains`方法依預設，執行區分大小寫比較，但 Entity Framework 的 SQL Server 提供者預設情況下執行不區分大小寫的比較。 因此，呼叫`ToUpper`若要使測試明確不區分大小寫的方法可確保當您變更程式碼稍後使用的儲存機制，這會傳回結果不會變更`IEnumerable`而不是集合`IQueryable`物件。 (當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)
>
> Null 處理也可能是不同的資料庫提供者，或當您使用不同`IQueryable`當您使用物件相較於`IEnumerable`集合。 例如，在某些情況下`Where`這類條件`table.Column != 0`可能不會傳回具有資料行`null`做為值。 如需詳細資訊，請參閱 <<c0> [ 為 null 的變數 'where' 子句中不正確地處理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)。

### <a name="add-a-search-box-to-the-student-index-view"></a>在搜尋方塊新增至學生 [索引] 檢視

1. 在*Views\Student\Index.cshtml*，新增醒目提示的程式碼開頭之前，立即`table`標記，以建立標題，文字方塊中，和**搜尋** 按鈕。

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=4-11)]

2. 執行頁面，輸入搜尋字串，然後按一下**搜尋**以確認篩選可以運作。

   請注意 URL 未包含"an"搜尋字串，這表示，如果您將本頁加入標籤，您無法取得篩選的清單，當您使用書籤。 這也適用於資料行排序連結時，因為它們會排序整個清單。 您將會變更**搜尋**来用於篩選準則中的查詢字串，稍後在本教學課程中的按鈕。

## <a name="add-paging"></a>加入分頁

若要將分頁新增至 Students [索引] 頁面中，您將先安裝**PagedList.Mvc** NuGet 套件。 然後您將進行中的其他變更`Index`方法，並新增到分頁連結`Index`檢視。 **PagedList.Mvc**許多很好的分頁和排序封裝的 ASP.NET MVC 的其中一個，而且它的使用僅為例，不做為它的建議，透過其他選項。

### <a name="install-the-pagedlistmvc-nuget-package"></a>安裝 PagedList.MVC NuGet 套件

NuGet **PagedList.Mvc**會自動安裝套件**PagedList**套件作為相依性。 **PagedList**封裝會安裝`PagedList`集合型別和擴充功能方法`IQueryable`和`IEnumerable`集合。 擴充方法建立單一頁面中的資料`PagedList`共收集您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法可簡化分頁。 **PagedList.Mvc**套件會安裝的分頁的協助程式會顯示分頁按鈕。

1. 從**工具**功能表上，選取**NuGet 套件管理員**，然後**Package Manager Console**。

2. 在**Package Manager Console**  視窗中，請確定**套件來源**是**nuget.org**而**預設專案**是**ContosoUniversity**，然後輸入下列命令：

   ```text
   Install-Package PagedList.Mvc
   ```

3. 建置專案。

### <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能新增至 Index 方法

1. 在  *Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

2. 以下列程式碼取代 `Index` 方法：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

   這個程式碼加入`page`參數、 目前的排序次序參數和目前的篩選參數至方法簽章：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

   第一次顯示頁面，或如果使用者尚未按一下分頁或排序連結，所有參數都是 null。 如果按一下分頁連結，`page`變數包含要顯示的頁碼。

   A`ViewBag`屬性會提供與目前的排序次序中，檢視，因為這必須包含在分頁連結，才能保留分頁時相同的排序次序：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

   另一個屬性， `ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。 此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。 如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。 在文字方塊中輸入值，並按下 [提交] 按鈕會變更搜尋字串。 在此情況下，`searchString`參數不是 null。

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

   方法的結尾`ToPagedList`擴充方法給學生們`IQueryable`物件將學生查詢轉換成支援分頁的集合類型的單一頁面。 該學生單一頁面接著會傳遞至檢視：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

   `ToPagedList` 方法會採用頁面數。 兩個問號代表[null 聯合運算子](/dotnet/csharp/language-reference/operators/null-coalescing-operator)。 Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。

### <a name="add-paging-links-to-the-student-index-view"></a>將分頁連結新增至學生 [索引] 檢視

1. 在  *Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼。 所做的變更已醒目標示。

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

   頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PagedList` 物件，而不是 `List` 物件。

   `using`陳述式`PagedList.Mvc`分頁按鈕讓 MVC 協助程式的存取。

   此程式碼使用的多載[BeginForm](/previous-versions/aspnet/dd492719(v=vs.108))可指定[FormMethod.Get](/previous-versions/aspnet/dd460179(v=vs.100))。

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

   預設值[BeginForm](/previous-versions/aspnet/dd492719(v=vs.108))文章時，這表示參數會在 HTTP 訊息本文中並不是以 URL 傳遞作為查詢字串使用提交表單資料。 當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。 [W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)建議的動作不會產生更新時，您應該使用 GET。

   文字方塊會初始化與目前的搜尋字串中，當您按一下新的頁面時，您就可以看到目前的搜尋字串。

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

   資料行標頭連結會使用查詢字串，將目前的搜尋字串傳遞至控制器，讓使用者可以在篩選結果內排序：

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

   會顯示目前頁面和總頁數。

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

   如果不沒有顯示任何頁面，會顯示 「 頁面 0 的 0 」。 (在此情況下頁編號大於頁面計數因為`Model.PageNumber`為 1，和`Model.PageCount`為 0。)

   分頁按鈕會顯示`PagedListPager`協助程式：

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

   `PagedListPager`協助程式提供數個選項，您可以自訂，包括 Url 和樣式設定。 如需詳細資訊，請參閱 < [TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 網站上。

2. 執行網頁。

   以不同排序次序按一下分頁連結，以確定分頁運作正常。 然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。

## <a name="create-an-about-page"></a>建立 [關於] 頁面

Contoso 大學網站的相關頁面，您將會顯示多少學生註冊每個註冊日期。 這需要對群組進行分組和簡單計算。 若要完成此工作，您需要執行下列作業：

- 針對您需要傳遞至檢視的資料，建立檢視模型類別。
- 修改`About`方法中的`Home`控制站。
- 修改`About`檢視。

### <a name="create-the-view-model"></a>建立檢視模型

建立*Viewmodel*專案資料夾中的資料夾。 在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*並以下列程式碼取代範本程式碼：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>修改 Home 控制器

1. 在  *HomeController.cs*，新增下列`using`陳述式在檔案頂端：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

2. 此類別的左大括弧之後立即新增資料庫內容的類別變數：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

3. 以下列程式碼取代 `About` 方法：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

   LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。

4. 新增`Dispose`方法：

   [!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>修改 About 檢視

1. 中的程式碼取代*Views\Home\About.cshtml*為下列程式碼的檔案：

   [!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

2. 執行應用程式，然後按一下**關於**連結。

   以表格顯示每個註冊日期的學生人數。

   ![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="get-the-code"></a>取得程式碼

[下載已完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)

## <a name="additional-resources"></a>其他資源

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 新增資料行排序連結
> * 新增搜尋方塊
> * 加入分頁
> * 建立 [關於] 頁面

請前往下一篇文章，以了解如何使用連線恢復功能和命令攔截。
> [!div class="nextstepaction"]
> [連接恢復功能和命令攔截](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)