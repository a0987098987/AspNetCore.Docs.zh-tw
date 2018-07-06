---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
title: 排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 (第 3 部 10) |Microsoft Docs
author: tdykstra
description: Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 的 ASP.NET MVC 4 應用程式...
ms.author: aspnetcontent
ms.date: 07/30/2013
ms.assetid: 8af630e0-fffa-4110-9eca-c96e201b2724
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: f5ce5f926021a53a5dd96e578b26b8c186c9a83c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37841030"
---
<a name="sorting-filtering-and-paging-with-the-entity-framework-in-an-aspnet-mvc-application-3-of-10"></a>排序、 篩選和分頁與 Entity Framework 中的 ASP.NET MVC 應用程式 (第 3 部 10)
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載已完成的專案](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> Contoso 大學範例 web 應用程式會示範如何建立使用 Entity Framework 5 Code First 和 Visual Studio 2012 的 ASP.NET MVC 4 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。 您可以從頭開始教學課程系列或[下載入門專案，如本章](building-the-ef5-mvc4-chapter-downloads.md)並從這裡開始。
> 
> > [!NOTE] 
> > 
> > 如果您遇到的問題，您無法解決，請[下載已完成的一章](building-the-ef5-mvc4-chapter-downloads.md)並嘗試重現您的問題。 您通常可以找到問題的解決方案，藉由比較您的程式碼的完整程式碼。 一些常見錯誤及如何解決這些問題，請參閱[錯誤和因應措施。](advanced-entity-framework-scenarios-for-an-mvc-web-application.md#errors)


在上一個教學課程中，您實作的基本 CRUD 作業的 web 網頁的一組`Student`實體。 在本教學課程中，您將新增排序、 篩選和分頁功能**學生**索引頁面。 此外，還要建立將執行簡易群組的頁面。

下圖顯示當您完成時的頁面外觀。 資料行標題是使用者可以按一下以依據該資料行排序的連結。 重覆按一下資料行標題，可切換遞增和遞減排序次序。

![Students_Index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>將資料行排序連結新增至 Students 的 [索引] 頁面

若要將排序新增至學生的 [索引] 頁面，您將會變更`Index`方法`Student`控制器並將程式碼加入`Student`索引檢視。

### <a name="add-sorting-functionality-to-the-index-method"></a>加入排序功能至 Index 方法

在  *Controllers\StudentController.cs*，取代`Index`為下列程式碼的方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.cs)]

此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。 查詢字串值是由 ASP.NET MVC 提供，做為動作方法的參數。 該參數是 "Name" 或 "Date" 的字串，後面可選擇性地接著底線和字串 "desc" 來指定遞減順序。 預設的排序次序為遞增。

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

此方法會使用[LINQ to Entities](https://msdn.microsoft.com/library/bb386964.aspx)指定排序所依據的資料行。 程式碼會建立[IQueryable](https://msdn.microsoft.com/library/bb351562.aspx)變數之前`switch`陳述式，會修改它`switch`陳述式以及呼叫`ToList`方法之後`switch`陳述式。 當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。 查詢不會執行直到您將轉換`IQueryable`物件加入集合中所呼叫的方法，例如`ToList`。 因此，此程式碼會產生之前不會執行單一查詢`return View`陳述式。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>新增資料行標題超連結至學生 [索引] 檢視

在  *Views\Student\Index.cshtml*，取代`<tr>`和`<th>`醒目提示的程式碼的標題資料列的項目：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cshtml?highlight=5-15)]

此程式碼使用中的資訊`ViewBag`屬性來設定超連結，以適當的查詢字串值。

執行頁面，然後按一下**姓氏**並**註冊日期**資料行標題，以確認該排序運作。

![Students_Index_page_with_sort_hyperlinks](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

按一下 之後**姓氏**標題之下，學生以遞減顯示姓氏來排序。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>在搜尋方塊新增至 Students [索引] 頁面

若要將篩選新增至 Students 的 [索引] 頁面，您要將文字方塊和提交按鈕新增至檢視，並在 `Index` 方法中進行對應的變更。 文字方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。

### <a name="add-filtering-functionality-to-the-index-method"></a>將篩選功能新增至 Index 方法

在  *Controllers\StudentController.cs*，取代`Index`（變更已醒目提示） 為下列程式碼的方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs?highlight=1,7-11)]

您已將 `searchString` 參數新增至 `Index` 方法。 您也已加入 LINQ 陳述式`where`clausethat 選取其名字或姓氏包含搜尋字串的學生。 從文字方塊中，您會加入至 [索引] 檢視接收搜尋字串值。加入陳述式[其中](https://msdn.microsoft.com/library/bb535040.aspx)子句在沒有要搜尋的值時，才會執行。

> [!NOTE]
> 在許多情況下，Entity Framework 實體集上，或是當做擴充方法在記憶體中的集合，可以呼叫相同的方法。 結果通常都相同，但在某些情況下可能會不同。 例如，.NET Framework 實作的`Contains`方法會傳回所有資料列，當您將空字串傳遞給它，但 SQL Server Compact 4.0 的 Entity Framework 提供者會傳回空字串的零個資料列。 因此在範例程式碼 (將放`Where`內的陳述式`if`陳述式) 可確保您取得所有的 SQL Server 版本相同的結果。 此外，.NET Framework 實作的`Contains`方法依預設，執行區分大小寫比較，但 Entity Framework 的 SQL Server 提供者預設情況下執行不區分大小寫的比較。 因此，呼叫`ToUpper`若要使測試明確不區分大小寫的方法可確保當您變更程式碼稍後使用的儲存機制，這會傳回結果不會變更`IEnumerable`而不是集合`IQueryable`物件。 (當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)


### <a name="add-a-search-box-to-the-student-index-view"></a>將搜尋方塊新增至學生的 [索引] 檢視

在*Views\Student\Index.cshtml*，新增醒目提示的程式碼開頭之前，立即`table`標記，以建立標題，文字方塊中，和**搜尋** 按鈕。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cshtml?highlight=5-10)]

執行頁面，輸入搜尋字串，然後按一下**搜尋**以確認篩選可以運作。

![Students_Index_page_with_search_box](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image4.png)

請注意 URL 未包含"an"搜尋字串，這表示，如果您將本頁加入標籤，您無法取得篩選的清單，當您使用書籤。 您將會變更**搜尋**来用於篩選準則中的查詢字串，稍後在本教學課程中的按鈕。

## <a name="add-paging-to-the-students-index-page"></a>將分頁新增至 Students [索引] 頁面

若要將分頁新增至 Students [索引] 頁面中，您會先安裝**PagedList.Mvc** NuGet 套件。 然後您將進行中的其他變更`Index`方法，並新增到分頁連結`Index`檢視。 **PagedList.Mvc**許多很好的分頁和排序封裝的 ASP.NET MVC 的其中一個，而且它的使用僅為例，不做為它的建議，透過其他選項。 下圖顯示分頁連結。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image5.png)

### <a name="install-the-pagedlistmvc-nuget-package"></a>安裝 PagedList.MVC NuGet 套件

NuGet **PagedList.Mvc**會自動安裝套件**PagedList**套件作為相依性。 **PagedList**封裝會安裝`PagedList`集合型別和擴充功能方法`IQueryable`和`IEnumerable`集合。 擴充方法建立單一頁面中的資料`PagedList`共收集您`IQueryable`或`IEnumerable`，和`PagedList`集合提供數個屬性和方法可簡化分頁。 **PagedList.Mvc**套件會安裝的分頁的協助程式會顯示分頁按鈕。

從**工具**功能表上，選取**程式庫套件管理員**，然後**管理方案的 NuGet 套件**。

在 **管理 NuGet 套件** 對話方塊中，按一下**線上**左側索引標籤，然後在 搜尋 方塊中輸入 「 分頁 」。 當您看到**PagedList.Mvc**套件，按一下 **安裝**。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image6.png)

在 **選取專案**方塊中，按一下**確定**。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image7.png)

### <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能新增至 Index 方法

在  *Controllers\StudentController.cs*，新增`using`陳述式`PagedList`命名空間：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs)]

以下列程式碼取代 `Index` 方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

這個程式碼加入`page`參數、 目前的排序次序參數和目前的篩選參數至方法簽章，如下所示：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

第一次顯示頁面，或是使用者尚未按一下分頁或排序連結時，所有參數都會是 null。 如果按一下分頁連結，`page`變數將包含要顯示的頁碼。

`A ViewBag` 屬性會提供目前的排序次序中，使用的檢視，因為這必須包含在分頁連結，才能保留分頁時相同的排序次序：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample9.cs)]

另一個屬性， `ViewBag.CurrentFilter`，與目前的篩選條件字串中提供的檢視。 此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。 如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。 在文字方塊中輸入值，並按下 [提交] 按鈕會變更搜尋字串。 在此情況下，`searchString`參數不是 null。

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample10.cs)]

方法的結尾`ToPagedList`擴充方法給學生們`IQueryable`物件將學生查詢轉換成支援分頁的集合類型的單一頁面。 該學生單一頁面接著會傳遞至檢視：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample11.cs)]

`ToPagedList` 方法會採用頁面數。 兩個問號代表[null 聯合運算子](https://msdn.microsoft.com/library/ms173224.aspx)。 Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。

### <a name="add-paging-links-to-the-student-index-view"></a>將分頁連結新增至學生 [索引] 檢視

在  *Views\Student\Index.cshtml*，以下列程式碼取代現有的程式碼：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample12.cshtml?highlight=6,9,14-20,56-58)]

頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PagedList` 物件，而不是 `List` 物件。

`using`陳述式`PagedList.Mvc`分頁按鈕讓 MVC 協助程式的存取。

此程式碼使用的多載[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)可指定[FormMethod.Get](https://msdn.microsoft.com/library/system.web.mvc.formmethod(v=vs.100).aspx/css)。

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample13.cshtml?highlight=1)]

預設值[BeginForm](https://msdn.microsoft.com/library/system.web.mvc.html.formextensions.beginform(v=vs.108).aspx)文章時，這表示參數會在 HTTP 訊息本文中並不是以 URL 傳遞作為查詢字串使用提交表單資料。 當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。 [W3C 指導方針，使用 HTTP GET](http://www.w3.org/2001/tag/doc/whenToUseGet.html)指定動作不會產生更新時應使用 GET。

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

執行網頁。

![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image8.png)

以不同排序次序按一下分頁連結，以確定分頁運作正常。 然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image9.png)

## <a name="create-an-about-page-that-shows-student-statistics"></a>建立顯示學生統計資料的頁面

Contoso 大學網站的相關頁面，您將會顯示多少學生註冊每個註冊日期。 這需要對群組進行分組和簡單計算。 若要完成此工作，您需要執行下列作業：

- 針對您需要傳遞至檢視的資料，建立檢視模型類別。
- 修改`About`方法中的`Home`控制站。
- 修改`About`檢視。

### <a name="create-the-view-model"></a>建立檢視模型

建立*Viewmodel*資料夾。 在該資料夾中，將類別檔案加入*EnrollmentDateGroup.cs*並以下列程式碼取代現有的程式碼：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample18.cs)]

### <a name="modify-the-home-controller"></a>修改 Home 控制器

在  *HomeController.cs*，新增下列`using`陳述式在檔案頂端：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample19.cs)]

此類別的左大括弧之後立即新增資料庫內容的類別變數：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample20.cs?highlight=3)]

以下列程式碼取代 `About` 方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample21.cs)]

LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。

新增`Dispose`方法：

[!code-csharp[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample22.cs)]

### <a name="modify-the-about-view"></a>修改 About 檢視

中的程式碼取代*Views\Home\About.cshtml*為下列程式碼的檔案：

[!code-cshtml[Main](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample23.cshtml)]

執行應用程式，然後按一下**關於**連結。 每個註冊日期的學生人數將會顯示在資料表中。

![About_page](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)

## <a name="optional-deploy-the-app-to-windows-azure"></a>選擇性： 將應用程式部署至 Windows Azure

到目前為止您的應用程式已經執行本機 IIS Express 中的開發電腦上。 若要使其可供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。 選擇性的本節的教學課程中您會將它部署至 Windows Azure 網站。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>使用 Code First 移轉，將資料庫部署

若要將資料庫部署中，您將使用 Code First 移轉。 當您建立發行設定檔，您用來設定從 Visual Studio 部署的設定時，您會選取標示的核取方塊**Execute Code First Migrations （在應用程式啟動時執行）**。 此設定會使部署程序會自動設定應用程式*Web.config*檔案在目的地伺服器上，讓 Code First 會使用`MigrateDatabaseToLatestVersion`初始設定式類別。

Visual Studio 不會在部署程序執行與資料庫的任何項目。 當部署的應用程式來存取資料庫第一次部署之後時，Code First 自動建立資料庫或更新至最新版本的資料庫結構描述。 如果應用程式實作移轉`Seed`方法，在方法執行之後建立資料庫，或在更新結構描述。

移轉`Seed`方法會將測試資料。 如果您已部署至生產環境中，您必須變更`Seed`方法，讓它只會插入您想要插入到您的生產資料庫的資料。 比方說，您目前的資料模型中可能會想要在開發資料庫的實際課程但虛構的學生。 您可以撰寫`Seed`負載皆在開發中，並再標記為註解虛構的學生在部署至生產環境之前的方法。 或者您可以撰寫`Seed`方法來載入只有課程，並使用應用程式的 UI 手動輸入虛構的學生在測試資料庫。

### <a name="get-a-windows-azure-account"></a>取得 Windows Azure 帳戶

您必須在 Windows Azure 帳戶。 如果您還沒有做，您可以建立免費的試用帳戶，只需要幾分鐘的時間。 如需詳細資訊，請參閱 < [Windows Azure 免費試用](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。

### <a name="create-a-web-site-and-a-sql-database-in-windows-azure"></a>在 Windows Azure 中建立網站和 SQL database

Windows Azure 網站會在共用主控環境中，這表示它會在與其他 Windows Azure 用戶端所共用的虛擬機器 (Vm) 上執行。 共用主控環境是開始在雲端中的低成本方法。 稍後，如果您的 web 流量增加，應用程式可擴充至專用的 Vm 上執行依需求。 如果您需要更複雜的架構，您可以移轉至 Windows Azure 雲端服務。 您可以根據您的需求設定的專用 Vm 上，執行雲端服務。

Windows Azure SQL Database 是建置在 SQL Server 技術的雲端型關聯式資料庫服務。 工具和使用 SQL Server 的應用程式也可以使用 SQL Database。

1. 在  [Windows Azure 管理入口網站](https://manage.windowsazure.com/)，按一下**網站**中的索引標籤，然後按一下**新增**。

    ![在管理入口網站中的新按鈕](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image11.png)
2. 按一下 **自訂建立**。

    ![建立具有在管理入口網站中的資料庫連結](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image12.png)

   **新的網站-自訂建立**精靈 隨即開啟。
3. 在  **New Web Site**步驟在精靈中，輸入字串**URL**方塊，以使用唯一的 URL 做為您的應用程式。 完整的 URL 將會包含您在此處輸入，加上尾碼，您會看到的文字方塊中。 下圖顯示 「 ConU"，但該 URL 可能會讓您將必須選擇其他名稱。

    ![建立具有在管理入口網站中的資料庫連結](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image13.png)
4. 在 **地區**下拉式清單中，選擇接近您的區域。 此設定會指定您的網站會以哪個資料中心。
5. 在 **資料庫**下拉式清單中，選擇**建立免費的 20MB SQL 資料庫**。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image14.png)
6. 在  **DB 連接字串名稱**，輸入*SchoolContext*。

    ![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image15.png)
7. 按一下底部的方塊右邊的箭號。 精靈會進入**資料庫設定**步驟。
8. 在 **名稱**方塊中，輸入*ContosoUniversityDB*。
9. 在 **伺服器**方塊中，選取**新的 SQL Database 伺服器**。 或者，如果您先前建立的伺服器，您可以從下拉式清單中選取該伺服器。
10. 輸入系統管理員**登入名稱**並**密碼**。 如果您選取**新的 SQL Database 伺服器**您不要在此輸入現有的名稱和密碼，您輸入新名稱和密碼，您現在定義以供未來存取資料庫時使用。 如果您選取您先前建立的伺服器，您要輸入認證，該伺服器。 本教學課程中，您將不會選取***進階***核取方塊。 ***進階***選項可讓您將資料庫設為[定序](https://msdn.microsoft.com/library/aa174903(v=SQL.80).aspx)。
11. 選擇相同**地區**您選擇的網站。
12. 按一下核取記號在右下方的方塊以表示您已完成。   
  
    ![資料庫的設定步驟的新網站-建立資料庫精靈](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image16.png)  

    下圖顯示使用現有的 SQL Server 和登入。   
  
    ![資料庫的設定步驟的新網站-建立資料庫精靈](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image17.png)  
  
    在管理入口網站會回到 網站 頁面中，而**狀態**欄顯示 正在建立網站。 一段時間之後 （通常少於一分鐘），**狀態**資料行會顯示已成功建立網站。 在左側導覽列中，您會在您的帳戶的網站數目旁會出現**Web Sites**圖示，然後資料庫數目旁會出現**SQL 資料庫**圖示。

## <a name="deploy-the-application-to-windows-azure"></a>部署至 Windows Azure 應用程式

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。  
  
    ![在專案操作功能表中發行](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image18.png)
2. 在 **設定檔** 索引標籤**發佈 Web**精靈 中，按一下**匯入**。  
  
    ![匯入發行設定](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image19.png)
3. 如果您未在 Visual Studio 中先前新增 Windows Azure 訂用帳戶，請執行下列步驟。 在這些步驟中您新增您的訂用帳戶以便下拉式清單底下**從 Windows Azure 網站上的匯入**會包含您的網站。

    a. 在 **匯入發行設定檔** 對話方塊中，按一下**從 Windows Azure 網站上的匯入**，然後按一下 **新增 Windows Azure 訂用帳戶**。

    ![新增 Windows Azure 訂用帳戶](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image20.png)

    b. 在 [**匯入 Windows Azure 訂用帳戶**] 對話方塊中，按一下**下載訂用帳戶檔案**。

    ![下載訂閱檔案](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image21.png)

    c.  在瀏覽器視窗中，儲存 *.publishsettings*檔案。

    ![下載.publishsettings 檔案](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image22.png)

    > [!WARNING]
    > 安全性- *publishsettings*檔案包含您的認證 （未編碼），用來管理您的 Windows Azure 訂用帳戶和服務。 此檔案的安全性最佳作法是暫時儲存在來源目錄之外 (秷濆 菾*Libraries\Documents*資料夾)，然後再匯入完成後刪除它。 取得存取權的惡意使用者`.publishsettings`檔案可以編輯、 建立和刪除您的 Windows Azure 服務。

    d. 在 [**匯入 Windows Azure 訂用帳戶**] 對話方塊中，按一下**瀏覽**並瀏覽至 *.publishsettings*檔案。

    ![下載 sub](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image23.png)

    e. 按一下 [匯入] 。

    ![import](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image24.png)
4. 在 **匯入發行設定檔**對話方塊中，選取**從 Windows Azure 網站上的匯入**，從下拉式清單中，選取您的網站，然後按一下**確定**。  
  
    ![匯入發行設定檔](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image25.png)
5. 在 **連接**索引標籤上，按一下**驗證連線**藉此確定設定正確無誤。  
  
    ![驗證連線](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)
6. 當已驗證的連接時旁, 顯示綠色的核取記號**驗證連線** 按鈕。 按 [ **下一步**]。  
  
    ![已成功驗證的連接](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image27.png)
7. 開啟**遠端的連接字串**下方的下拉式清單**SchoolContext** ，然後選取您建立的資料庫的連接字串。
8. 選取  **Execute Code First Migrations （在應用程式啟動時執行）**。
9. 取消核取**在執行階段使用此連接字串**for **UserContext (DefaultConnection)**，因為此應用程式未使用成員資格資料庫。   
  
    ![設定 索引標籤](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image28.png)
10. 按 [ **下一步**]。
11. 在  **Preview**索引標籤上，按一下**開始預覽**。  
  
    ![[預覽] 索引標籤中的 [StartPreview] 按鈕](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image29.png)  
  
    [] 索引標籤會顯示將會複製到伺服器的檔案清單。 顯示預覽不需要發佈應用程式，但是是有用的函式，要注意。 在此情況下，您不需要採取任何動作顯示的檔案清單。 下次您部署此應用程式中，只有已變更的檔案會出現在此清單。  
  
    ![StartPreview 檔案輸出](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image30.png)
12. 按一下 [發行] 。  
    Visual Studio 會開始將檔案複製到 Windows Azure 伺服器的程序。
13. **輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。  
  
    ![報告部署成功的 [輸出] 視窗](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image31.png)
14. 部署成功時，預設瀏覽器會自動開啟至已部署的網站的 URL。  
    您所建立的應用程式現在正在雲端中執行的。 按一下 [Students] 索引標籤。  
  
    ![Students_index_page_with_paging](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image32.png)

此時您*SchoolContext*資料庫已在 Windows Azure SQL Database 中已建立，因為您選取**Execute Code First Migrations （在應用程式啟動時執行）**。 *Web.config*在已部署的網站上的檔案已變更，讓[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始設定式會執行第一次您的程式碼中讀取或寫入資料庫中的資料 （其中您選取時發生**學生** 索引標籤上):

![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image33.png)

部署程序也會建立新的連接字串 *(SchoolContext\_DatabasePublish*) 對要用於更新資料庫結構描述和植入資料庫的 Code First 移轉。

![Database_Publish 連接字串](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image34.png)

*DefaultConnection*連接字串做為成員資格資料庫 （我們不會使用在本教學課程）。 *SchoolContext*是 ContosoUniversity 資料庫的連接字串。

您可以找到 Web.config 檔案在電腦上部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。您可以存取已部署*Web.config*檔案本身使用 FTP。 如需相關指示，請參閱 <<c0> [ 使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新](../../../../web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update.md)。 遵循指示以開始 「 若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。 」

> [!NOTE]
> Web 應用程式不會實作安全性，以便找到 URL 的任何人可以變更的資料。 如需有關如何保護網站上的指示，請參閱[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 應用程式部署至 Windows Azure 網站](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 您可以防止其他人使用 Windows Azure 管理入口網站中使用網站或**伺服器總管**停止站台的 Visual Studio 中。


![](sorting-filtering-and-paging-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image35.png)

## <a name="code-first-initializers"></a>程式碼的第一個初始設定式

在 [部署] 區段中您所見[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)所使用的初始設定式。 程式碼第一次也會提供您可以使用，包括其他初始設定式[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （預設值）， [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx)和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。 `DropCreateAlways`初始設定式可用於設定 單元測試的條件。 您也可以撰寫您自己的初始設定式，而您可以呼叫初始設定式明確地如果您不想等候，直到應用程式讀取或寫入資料庫。 如需初始設定式的完整說明，請參閱本書第 6 章[Programming Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。

## <a name="summary"></a>總結

在本教學課程中，您已了解如何建立資料模型，並實作基本的 CRUD、 排序、 篩選、 分頁和分組功能。 在下一個教學課程中，您將開始查看更進階的主題，方法是展開的資料模型。

其他 Entity Framework 資源連結可在[ASP.NET 資料存取內容對應](../../../../whitepapers/aspnet-data-access-content-map.md)。

> [!div class="step-by-step"]
> [上一頁](implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application.md)
> [下一頁](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
