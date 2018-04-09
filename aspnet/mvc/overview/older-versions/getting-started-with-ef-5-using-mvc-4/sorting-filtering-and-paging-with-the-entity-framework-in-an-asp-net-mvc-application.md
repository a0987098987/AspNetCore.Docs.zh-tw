---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-3) |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 09327b760d9be38d7e004cbcef08cad4eab3a26c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 (10-3)
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 4 應用程式使用 Entity Framework 5 Code First 和 Visual Studio 2012。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載本章節的入門專案](building-the-ef5-mvc4-chapter-downloads.md)和從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，[下載已完成的章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現問題。 您通常可以藉由比較您的程式碼完成的程式碼會發現問題的解決方案。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


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

您已將 `searchString` 參數新增至 `Index` 方法。 您也已經加入至 LINQ 陳述式`where`clausethat 選取其名字或姓氏包含搜尋字串的學生。 從文字方塊中，您會加入至索引檢視收到的搜尋字串值。加入陳述式[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句在沒有要搜尋的值時，才會執行。

> [!NOTE]
> 在許多情況下，Entity Framework 實體集上，或是當做擴充方法上的記憶體中集合，可以呼叫相同的方法。 結果通常都相同，但在某些情況下可能會不同。 例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回零個資料列空白字串。 因此在範例程式碼 (放`Where`陳述式內`if`陳述式) 可確保您取得所有版本的 SQL Server 相同的結果。 此外，.NET Framework 實作的`Contains`方法會根據預設，執行區分大小寫的比較，但預設實體架構的 SQL Server 提供者執行不區分大小寫的比較。 因此，呼叫`ToUpper`來進行測試更明確地不區分大小寫的方法可確保當您變更程式碼更新版本使用的儲存機制將會傳回結果不會變更`IEnumerable`集合，而不是`IQueryable`物件。 (當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)


### <a name="add-a-search-box-to-the-student-index-view"></a>將搜尋方塊新增至學生的 [索引] 檢視

在*Views\Student\Index.cshtml*，將之前開啟反白顯示的程式碼加入`table`才能建立 標題 文字方塊中，標記和**搜尋** 按鈕。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

執行頁面，輸入搜尋字串，然後按一下**搜尋**驗證篩選是否正在運作。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

請注意 URL 未包含"an"搜尋字串，這表示，如果您加入書籤此頁面，您不會取得篩選的清單，當您使用書籤。 您要變更**搜尋** 按鈕，稍後在本教學課程中使用的篩選準則的查詢字串。

## <a name="add-paging-to-the-students-index-page"></a>加入分頁學生索引頁

若要加入分頁學生索引頁，您會先安裝**PagedList.Mvc** NuGet 封裝。 然後您要進行其他變更的`Index`方法並新增到分頁連結`Index`檢視。 **PagedList.Mvc**許多良好分頁和排序封裝 ASP.NET mvc 的其中一個，而且它的使用只為例，不做為它的建議，透過其他選項。 下圖顯示分頁連結。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>安裝 PagedList.MVC NuGet 套件

NuGet **PagedList.Mvc**套件會自動安裝**PagedList**封裝做為相依性。 **PagedList**封裝安裝`PagedList`集合型別和延伸模組方法`IQueryable`和`IEnumerable`集合。 擴充方法建立單一頁面中的資料`PagedList`集合超出您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法，以便分頁。 **PagedList.Mvc**套件會安裝分頁的 helper 顯示分頁按鈕。

從**工具**功能表上，選取**程式庫套件管理員**然後**管理方案的 NuGet 套件**。

在**管理 NuGet 封裝**對話方塊中，按一下 **線上**左側索引標籤，然後在 搜尋 方塊中輸入 "分頁 」。 當您看到**PagedList.Mvc**封裝中，按一下 **安裝**。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

在**選取專案**方塊中，按一下**確定**。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能加入至索引方法

在*Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

以下列程式碼取代 `Index` 方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

這個程式碼加入`page`參數、 目前的排序順序參數，以及目前的篩選參數的方法簽章，如下所示：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

第一次顯示頁面，或是使用者尚未按一下分頁或排序連結時，所有參數都會是 null。 按一下分頁連結時，如果`page`變數將包含要顯示的頁面數。

`A ViewBag` 屬性會提供與目前的排序次序中，檢視，因為這必須以保持在分頁時相同的排序順序包含在分頁連結：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

另一個屬性`ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。 此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。 如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。 在文字方塊中輸入的值，並按下 [提交] 按鈕時，會變更搜尋字串。 在此情況下，`searchString`參數不是 null。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

方法的結尾`ToPagedList`學生上的擴充方法`IQueryable`物件會將學生查詢轉換成單一頁面的學生支援分頁的集合型別。 學生的單一頁面再傳遞到檢視中：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` 方法會採用頁面數。 兩個問號代表[null 聯合運算子](https://msdn.microsoft.com/library/ms173224.aspx)。 Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。

### <a name="add-paging-links-to-the-student-index-view"></a>將 Student 索引檢視中的分頁連結

在*Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PagedList` 物件，而不是 `List` 物件。

`using`陳述式`PagedList.Mvc`可讓 MVC 協助程式的存取 [分頁] 按鈕。

程式碼使用的多載[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx) ，可讓它指定[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

預設值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)提交表單與文章時，這表示，參數傳遞的 HTTP 訊息本文，而不是在 URL 查詢字串的形式的資料。 當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。 [W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)指定動作並不會導致更新時應使用 GET。

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

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

以不同排序次序按一下分頁連結，以確定分頁運作正常。 然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>建立相關頁面，其中顯示學生統計資料

Contoso 大學網站的相關頁面，您要顯示多少學生已註冊的每個註冊日期。 這需要對群組進行分組和簡單計算。 若要完成此工作，您需要執行下列作業：

- 針對您需要傳遞至檢視的資料，建立檢視模型類別。
- 修改`About`方法中的`Home`控制站。
- 修改`About`檢視。

### <a name="create-the-view-model"></a>建立檢視模型

建立*ViewModels*資料夾。 在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs* ，並以下列程式碼取代現有的程式碼：

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

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>選擇性： 部署應用程式至 Windows Azure

到目前為止您的應用程式具有已在本機執行 IIS Express 中開發電腦上。 若要使其可供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。 選擇性的本節的教學課程中您會部署至 Windows Azure 網站。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>使用 Code First 移轉，將資料庫部署

若要將資料庫部署中，您將使用 Code First 移轉。 當您建立發行設定檔讓您從 Visual Studio 部署的設定時，您會選取核取方塊標示為**執行 Code First 移轉 （在應用程式啟動時執行）**。 此設定會自動設定應用程式部署程序*Web.config*檔案在目的地伺服器上，所以第一個程式碼會使用`MigrateDatabaseToLatestVersion`初始設定式類別。

Visual Studio 不會與資料庫的任何項目在部署程序。 當部署的應用程式來存取資料庫第一次部署之後時，Code First 自動建立資料庫或更新資料庫結構描述的最新版本。 如果應用程式實作移轉`Seed`方法，在方法執行之後建立資料庫，或在更新結構描述。

您的移轉`Seed`方法會將測試資料。 如果您已部署至生產環境，您必須變更`Seed`方法，使它只會插入您想要插入至您的生產資料庫的資料。 例如，您目前的資料模型中您可以開發資料庫中有實際的課程，但虛構學生。 您可以撰寫`Seed`方法來載入兩者在開發中，並再標記為註解虛構的學生在部署到生產環境之前。 您可以撰寫或`Seed`載入只課程中，並使用應用程式的 UI 手動輸入虛構的學生在測試資料庫中的方法。

### <a name="get-a-windows-azure-account"></a>取得 Windows Azure 帳戶

您必須在 Windows Azure 帳戶。 如果您還沒有其中一個，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱[Windows Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>在 Windows Azure 中建立的網站和 SQL 資料庫

Windows Azure 網站會執行在共用裝載環境中，這表示它與其他 Windows Azure 用戶端共用的虛擬機器 (Vm) 上執行。 共用裝載環境是開始在雲端中的低成本方法。 更新版本中，如果您的 web 流量的增加，應用程式可以調整以符合需要專用的 Vm 上執行。 如果您需要更複雜的架構，您可以移轉至 Windows Azure 雲端服務。 專用根據您的需求，您可以設定的 Vm 上執行雲端服務。

Windows Azure SQL Database 是建立在 SQL Server 技術以雲端為基礎的關聯式資料庫服務。 工具和 SQL Server 所使用的應用程式也使用 SQL 資料庫。

1. 在[Windows Azure 管理入口網站](https://manage.windowsazure.com/)，按一下 **網站**在左側索引標籤上，然後按一下**新增**。

    ![在管理入口網站中的新按鈕](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. 按一下**自訂建立**。

    ![建立具有管理入口網站中的資料庫連結](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **新網站-自訂建立**精靈 隨即開啟。
3. 在**新網站**步驟在精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式。 完整的 URL 將會包含哪些您在此處輸入，加上尾碼，您所看到的文字方塊中。 下圖顯示 「 ConU"，但該 URL 可能會讓您必須選擇不同的密碼。

    ![建立具有管理入口網站中的資料庫連結](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. 在**區域**下拉式清單中，選擇接近您的區域。 此設定指定您的網站會在執行哪一個資料中心。
5. 在**資料庫**下拉式清單中，選擇**建立免費的 20MB SQL 資料庫**。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. 在**資料庫連接字串名稱**，輸入*SchoolContext*。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. 按一下底部的方塊右邊的箭號。 精靈會進入**資料庫設定**步驟。
8. 在**名稱**方塊中，輸入*ContosoUniversityDB*。
9. 在**伺服器**方塊中，選取**新 SQL Database 伺服器**。 或者，如果您先前建立的伺服器，您可以從下拉式清單選取該伺服器。
10. 輸入系統管理員**登入名稱**和**密碼**。 如果您選取**新 SQL Database 伺服器**不輸入現有的名稱和密碼，您輸入新名稱和密碼，您現在定義存取資料庫時使用。 如果您選取您先前建立的伺服器，您會輸入該伺服器的認證。 此教學課程中，您將不會選取***進階***核取方塊。 ***進階***選項可讓您將資料庫設為[定序](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)。
11. 選擇相同**區域**您所選擇的網站。
12. 按一下右下方的方塊以表示您完成在核取記號。   
  
    ![資料庫的設定步驟的新網站-建立資料庫精靈](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    下圖顯示使用現有的 SQL Server 和登入。   
  
    ![資料庫的設定步驟的新網站-建立資料庫精靈](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    在管理入口網站返回網站 頁面上，而**狀態**資料行會顯示建立網站。 （通常小於一分鐘），一段時間之後**狀態**資料行會顯示已成功建立站台。 在左側導覽列中的數量的網站，您會在您的帳戶旁會顯示**網站**圖示和資料庫數目旁會顯示**SQL 資料庫**圖示。

## <a name="deploy-the-application-to-windows-azure"></a>部署至 Windows Azure 應用程式

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**從內容功能表。  
  
    ![在專案內容功能表中發行](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. 在**設定檔** 索引標籤**發行 Web**精靈 中，按一下 **匯入**。  
  
    ![匯入發行設定](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. 如果您未在 Visual Studio 中先前加入 Windows Azure 訂用帳戶，執行下列步驟。 這些步驟中您加入您的訂用帳戶，讓下拉式清單底下**從 Windows Azure 網站匯入**將包含您的網站。

    a. 在**匯入發行設定檔**對話方塊中，按一下 **從 Windows Azure 網站匯入**，然後按一下 **新增 Windows Azure 訂用帳戶**。

    ![新增 Windows Azure 訂用帳戶](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. 在**匯入 Windows Azure 訂用帳戶**對話方塊中，按一下 **下載訂閱檔案**。

    ![下載訂用帳戶檔案](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c.  在瀏覽器視窗中，儲存*.publishsettings*檔案。

    ![下載.publishsettings 檔案](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > 安全性- *publishsettings*檔案包含您的認證 （未編碼），可用來管理 Windows Azure 訂用帳戶和服務。 這個檔案的安全性最佳作法是暫時儲存在來源目錄之外 (例如在*Libraries\Documents*資料夾)，然後刪除它，一旦完成匯入。 取得存取權的惡意使用者`.publishsettings`檔案可以編輯、 建立和刪除您的 Windows Azure 服務。

    d. 在**匯入 Windows Azure 訂用帳戶**對話方塊中，按一下 **瀏覽**並瀏覽至*.publishsettings*檔案。

    ![下載 sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. 按一下 [匯入] 。

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. 在**匯入發行設定檔**對話方塊中，選取**從 Windows Azure 網站匯入**，從下拉式清單中，選取您的網站，然後按一下**確定**。  
  
    ![匯入發行設定檔](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. 在**連接**索引標籤上，按一下 **驗證連線**確定設定正確無誤。  
  
    ![驗證連接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. 當已驗證的連接時，旁邊顯示綠色的核取記號**驗證連線** 按鈕。 按 [ **下一步**]。  
  
    ![已成功驗證的連接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. 開啟**遠端的連接字串**底下的下拉式清單**SchoolContext**並選取您所建立之資料庫的連接字串。
8. 選取**執行 Code First 移轉 （在應用程式啟動時執行）**。
9. 取消核取**在執行階段使用此連接字串**如**UserContext (DefaultConnection)**，因為此應用程式未使用成員資格資料庫。   
  
    ![設定 索引標籤](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. 按 [ **下一步**]。
11. 在**預覽**索引標籤上，按一下 **啟動預覽**。  
  
    ![在 [預覽] 索引標籤中的 StartPreview 按鈕](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    索引標籤會顯示將會複製到伺服器的檔案清單。 顯示預覽並不需要發行應用程式但要注意的有用的函式。 在此情況下，您不需要進行任何動作顯示的檔案清單。 下次當您部署此應用程式，這份清單中會變更的檔案。  
  
    ![StartPreview 檔案輸出](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. 按一下 [發行] 。  
    Visual Studio 會開始將檔案複製到 Windows Azure 伺服器的程序。
13. **輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。  
  
    ![報告成功部署的 [輸出] 視窗](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. 部署成功之後，預設瀏覽器會自動開啟至已部署的網站的 URL。  
    您所建立的應用程式現在在雲端中執行。 按一下 [學生] 索引標籤。  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

此時您*SchoolContext*資料庫已建立 Windows Azure SQL Database 中因為您選取**執行 Code First 移轉 （在應用程式啟動時執行）**。 *Web.config*已部署的網站中檔案已經變更，讓[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始設定式會執行第一次您的程式碼中讀取或寫入資料庫中的資料 （其中發生什麼事時您所選取**學生** 索引標籤):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

部署程序也會建立新的連接字串*(SchoolContext\_DatabasePublish*) 的 Code First 移轉，以用於更新資料庫結構描述和植入資料庫。

![Database_Publish 連接字串](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection*連接字串是成員資格資料庫 （這並未使用會在本教學課程）。 *SchoolContext*連接字串是 ContosoUniversity 資料庫。

您可以找到您自己的電腦上的 Web.config 檔案的部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。您可以存取已部署*Web.config*檔案本身可以使用 FTP。 如需指示，請參閱[使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。 遵循開頭 「 若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。 」

> [!NOTE]
> Web 應用程式不會實作安全性，以便尋找此 URL 的任何人都可以變更資料。 如需有關如何保護網站的指示，請參閱[將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 您可以防止其他人使用 Windows Azure 管理入口網站使用站台或**伺服器總管**停止站台的 Visual Studio 中。


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>程式碼的第一個初始設定式

在 [部署] 區段中看到[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)所使用的初始設定式。 程式碼第一次也會提供您可以使用，包括其他初始設定式[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （預設）、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。 `DropCreateAlways`初始設定式可用於設定單元測試的條件。 您也可以撰寫您自己的初始設定式，而且您可以呼叫初始設定式明確若不想等到應用程式讀取或寫入資料庫。 初始設定式的完整說明，請參閱第 6 章活頁簿[程式設計 Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何建立資料模型及實作基本 CRUD 排序、 篩選、 分頁和分組功能。 在下一個教學課程中開始，您將查看更進階的主題，方法是展開的資料模型。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [下一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
