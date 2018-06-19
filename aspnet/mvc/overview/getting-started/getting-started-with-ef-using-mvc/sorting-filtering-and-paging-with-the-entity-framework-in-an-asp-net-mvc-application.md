---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/01/2015
ms.topic: article
ms.assetid: d5723e46-41fe-4d09-850a-e03b9e285bfa
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 02b7d988202966dc0011eeed32cd632c6e0565b4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874677"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application"></a>排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。


在上一個教學課程中您會實作一組基本 CRUD 作業的 web 網頁`Student`實體。 在本教學課程，您要加入排序、 篩選和分頁功能**學生**索引頁面。 此外，還要建立將執行簡易群組的頁面。

下圖顯示當您完成時的頁面外觀。 資料行標題是使用者可以按一下以依據該資料行排序的連結。 重覆按一下資料行標題，可切換遞增和遞減排序次序。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>將資料行排序連結新增至 Students 的 [索引] 頁面

若要加入排序學生索引頁，您要變更`Index`方法`Student`控制站，將程式碼加入`Student`索引檢視表。

### <a name="add-sorting-functionality-to-the-index-method"></a>加入排序功能 Index 方法

在*Controllers\StudentController.cs*，取代`Index`方法取代下列程式碼：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。 查詢字串值是由 ASP.NET MVC 提供，做為參數至動作方法。 該參數是 "Name" 或 "Date" 的字串，後面可選擇性地接著底線和字串 "desc" 來指定遞減順序。 預設的排序次序為遞增。

第一次要求 [索引] 頁面時，沒有任何查詢字串。 以遞增順序顯示學生`LastName`，在中斷的情況中所建立，這是預設`switch`陳述式。 使用者按一下資料行標題超連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。

這兩個`ViewBag`使用變數時，讓檢視可以設定適當的查詢字串值的資料行標題超連結：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.cs)]

這些是三元陳述式。 第一個指定，如果`sortOrder`參數為 null 或空的`ViewBag.NameSortParm`應該設定為 「 名稱\_desc"，否則應設為空字串。 這兩個陳述式讓檢視能夠設定資料行標題超連結，如下所示：

| 目前排序次序 | 姓氏超連結 | 日期超連結 |
| --- | --- | --- |
| 姓氏遞增 | descending | ascending |
| 姓氏遞減 | ascending | ascending |
| 日期遞增 | ascending | descending |
| 日期遞減 | ascending | ascending |

此方法會使用[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)指定排序所依據的資料行。 程式碼會建立[IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)變數之前`switch`陳述式，修改在`switch`陳述式，並呼叫`ToList`方法之後`switch`陳述式。 當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。 查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToList`。 因此，此程式碼會產生單一查詢，將會等到執行`return View`陳述式。

替代方法來撰寫每個排序順序的不同 LINQ 陳述式中，您可以動態建立 LINQ 陳述式。 如需動態 LINQ 資訊，請參閱[動態 LINQ](https://go.microsoft.com/fwlink/?LinkID=323957)。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>加入資料行標題學生索引檢視的超連結

在*Views\Student\Index.cshtml*，取代`<tr>`和`<th>`標題資料列，以反白顯示的程式碼項目：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

此程式碼使用中的資訊`ViewBag`屬性，以設定適當的查詢與超連結的字串值。

執行網頁，然後按一下**姓氏**和**註冊日期**以確認該排序的資料行標題的運作方式。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

按一下 之後**姓氏**標題之下，學生顯示遞減最後一個的名稱。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>學生索引頁面中新增搜尋方塊

若要將篩選新增至 Students 的 [索引] 頁面，您要將文字方塊和提交按鈕新增至檢視，並在 `Index` 方法中進行對應的變更。 文字方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。

### <a name="add-filtering-functionality-to-the-index-method"></a>索引方法中加入篩選功能

在*Controllers\StudentController.cs*，取代`Index`（所做的變更會反白顯示） 為下列程式碼的方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

您已將 `searchString` 參數新增至 `Index` 方法。 從將新增至 [索引] 檢視的文字方塊中接收搜尋字串值。 您也已經加入至 LINQ 陳述式`where`選取其名字或姓氏包含搜尋字串的學生的子句。 加入陳述式[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句在沒有要搜尋的值時，才會執行。

> [!NOTE]
> 在許多情況下，Entity Framework 實體集上，或是當做擴充方法上的記憶體中集合，可以呼叫相同的方法。 結果通常都相同，但在某些情況下可能會不同。
> 
> 例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回零個資料列空白字串。 因此在範例程式碼 (放`Where`陳述式內`if`陳述式) 可確保您取得所有版本的 SQL Server 相同的結果。 此外，.NET Framework 實作的`Contains`方法會根據預設，執行區分大小寫的比較，但預設實體架構的 SQL Server 提供者執行不區分大小寫的比較。 因此，呼叫`ToUpper`來進行測試更明確地不區分大小寫的方法可確保當您變更程式碼更新版本使用的儲存機制將會傳回結果不會變更`IEnumerable`集合，而不是`IQueryable`物件。 (當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)
> 
> Null 處理也可能不同的資料庫提供者，或當您使用的`IQueryable`物件相較於，當您使用`IEnumerable`集合。 例如，在某些情況下`Where`這類條件`table.Column != 0`可能不會傳回具有資料行`null`做為值。 如需詳細資訊，請參閱['where' 子句中的 null 變數不正確地處理](https://data.uservoice.com/forums/72025-entity-framework-feature-suggestions/suggestions/1015361-incorrect-handling-of-null-variables-in-where-cl)。


### <a name="add-a-search-box-to-the-student-index-view"></a>將搜尋方塊新增至學生的 [索引] 檢視

在*Views\Student\Index.cshtml*，將之前開啟反白顯示的程式碼加入`table`才能建立 標題 文字方塊中，標記和**搜尋** 按鈕。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

執行頁面，輸入搜尋字串，然後按一下**搜尋**驗證篩選是否正在運作。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

請注意 URL 未包含"an"搜尋字串，這表示，如果您加入書籤此頁面，您不會取得篩選的清單，當您使用書籤。 這也適用於資料行排序連結，因為它們會排序整個清單。 您要變更**搜尋** 按鈕，稍後在本教學課程中使用的篩選準則的查詢字串。

## <a name="add-paging-to-the-students-index-page"></a>加入分頁學生索引頁

若要加入分頁學生索引頁，您會先安裝**PagedList.Mvc** NuGet 封裝。 然後您要進行其他變更的`Index`方法並新增到分頁連結`Index`檢視。 **PagedList.Mvc**許多良好分頁和排序封裝 ASP.NET mvc 的其中一個，而且它的使用只為例，不做為它的建議，透過其他選項。 下圖顯示分頁連結。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>安裝 PagedList.MVC NuGet 套件

NuGet **PagedList.Mvc**套件會自動安裝**PagedList**封裝做為相依性。 **PagedList**封裝安裝`PagedList`集合型別和延伸模組方法`IQueryable`和`IEnumerable`集合。 擴充方法建立單一頁面中的資料`PagedList`集合超出您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法，以便分頁。 **PagedList.Mvc**套件會安裝分頁的 helper 顯示分頁按鈕。

從**工具**功能表上，選取**程式庫套件管理員**然後**Package Manager Console**。

在**Package Manager Console**視窗中，請確定**套件來源**是**nuget.org**和**預設專案**是**ContosoUniversity**，然後輸入下列命令：

`Install-Package PagedList.Mvc`

![Install PagedList.Mvc](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

建置專案。 

### <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能加入至索引方法

在*Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

以下列程式碼取代 `Index` 方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs?highlight=1,3,7-16,41-43)]

這個程式碼加入`page`參數、 目前的排序順序參數，以及目前的篩選參數的方法簽章：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

第一次顯示頁面，或是使用者尚未按一下分頁或排序連結時，所有參數都會是 null。 按一下分頁連結時，如果`page`變數將包含要顯示的頁面數。

A`ViewBag`屬性提供的檢視，且目前的排序次序中，因為這必須以保持在分頁時相同的排序順序包含在分頁連結：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

另一個屬性`ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。 此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。 如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。 在文字方塊中輸入的值，並按下 [提交] 按鈕時，會變更搜尋字串。 在此情況下，`searchString`參數不是 null。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

方法的結尾`ToPagedList`學生上的擴充方法`IQueryable`物件會將學生查詢轉換成單一頁面的學生支援分頁的集合型別。 學生的單一頁面再傳遞到檢視中：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` 方法會採用頁面數。 兩個問號代表[null 聯合運算子](https://msdn.microsoft.com/library/ms173224.aspx)。 Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。

### <a name="add-paging-links-to-the-student-index-view"></a>將 Student 索引檢視中的分頁連結

在*Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼。 所做的變更已醒目提示。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=1-3,6,9,14,17,24,30,55-56,58-59)]

頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PagedList` 物件，而不是 `List` 物件。

`using`陳述式`PagedList.Mvc`可讓 MVC 協助程式的存取 [分頁] 按鈕。

程式碼使用的多載[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ，可讓它指定[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

預設值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)提交表單與文章時，這表示，參數傳遞的 HTTP 訊息本文，而不是在 URL 查詢字串的形式的資料。 當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。 [W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)建議動作並不會導致更新時，您應該使用 GET。

在文字方塊中會使用目前的搜尋字串初始化，因此當您按一下新的頁面時，您就可以看到目前的搜尋字串。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample14.cshtml?highlight=1)]

資料行標頭連結會使用查詢字串，將目前的搜尋字串傳遞至控制器，讓使用者可以在篩選結果內排序：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample15.cshtml?highlight=1)]

會顯示目前頁面和總頁數。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample16.cshtml)]

如果沒有顯示網頁，則會顯示頁面 0 的 0 」。 (在此情況下頁面數目大於頁面計數因為`Model.PageNumber`為 1，和`Model.PageCount`為 0。)

分頁按鈕會顯示`PagedListPager`helper:

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample17.cshtml)]

`PagedListPager` Helper 提供數個選項，您可以自訂，包括 Url 和樣式設定。 如需詳細資訊，請參閱[TroyGoode / PagedList](https://github.com/TroyGoode/PagedList) GitHub 網站上。

執行網頁。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

以不同排序次序按一下分頁連結，以確定分頁運作正常。 然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>建立相關頁面，其中顯示學生統計資料

Contoso 大學網站的相關頁面，您要顯示多少學生已註冊的每個註冊日期。 這需要對群組進行分組和簡單計算。 若要完成此工作，您需要執行下列作業：

- 針對您需要傳遞至檢視的資料，建立檢視模型類別。
- 修改`About`方法中的`Home`控制站。
- 修改`About`檢視。

### <a name="create-the-view-model"></a>建立檢視模型

建立*ViewModels*專案資料夾中的資料夾。 在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*和範本程式碼取代為下列程式碼：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>修改 Home 控制器

在*HomeController.cs*，加入下列`using`在檔案最上方的陳述式：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

將資料庫內容的類別變數加入之後左大括號類別：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

以下列程式碼取代 `About` 方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。

新增`Dispose`方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>修改 About 檢視

中的程式碼取代*Views\Home\About.cshtml*以下列程式碼檔案：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

執行應用程式，然後按一下**有關**連結。 每個註冊日期的學生人數將會顯示在資料表中。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何建立資料模型及實作基本 CRUD 排序、 篩選、 分頁和分組功能。 在下一個教學課程中開始，您將查看更進階的主題，方法是展開的資料模型。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [下一頁](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)
