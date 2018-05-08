---
title: ASP.NET Core MVC 與 EF Core - 排序、篩選、分頁 - 3/10
author: tdykstra
description: 在本教學課程中，您將會使用 ASP.NET Core 和 Entity Framework Core 將排序、篩選、分頁功能新增至頁面。
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/sort-filter-page
ms.openlocfilehash: d4fe6386318210a751d1248c87299d414ab563a3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="aspnet-core-mvc-with-ef-core---sort-filter-paging---3-of-10"></a>ASP.NET Core MVC 與 EF Core - 排序、篩選、分頁 - 3/10

作者：[Tom Dykstra](https://github.com/tdykstra) 和 [Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大學範例 Web 應用程式將示範如何以 Entity Framework Core 和 Visual Studio 來建立 ASP.NET Core MVC Web 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](intro.md)。

在上一個教學課程中，您已針對學生實體的基本 CRUD 作業實作一組網頁。 在本教學課程中，您要將排序、篩選和分頁功能新增至 Students 的 [索引] 頁面。 此外，還要建立將執行簡易群組的頁面。

下圖顯示當您完成時的頁面外觀。 資料行標題是使用者可以按一下以依據該資料行排序的連結。 重覆按一下資料行標題，可切換遞增和遞減排序次序。

![Students [索引] 頁面](sort-filter-page/_static/paging.png)

## <a name="add-column-sort-links-to-the-students-index-page"></a>將資料行排序連結新增至 Students 的 [索引] 頁面

若要將排序新增至學生的 [索引] 頁面，您要變更 Students 控制器的 `Index` 方法，並將程式碼新增至學生的 [索引] 檢視。

### <a name="add-sorting-functionality-to-the-index-method"></a>將排序功能新增至 Index 方法

在 *StudentsController.cs* 中，以下列程式碼取代 `Index` 方法：

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly)]

此程式碼會從 URL 中的查詢字串接收 `sortOrder` 參數。 查詢字串值是由 ASP.NET Core MVC 提供，作為動作方法的參數。 該參數是 "Name" 或 "Date" 的字串，後面可選擇性地接著底線和字串 "desc" 來指定遞減順序。 預設的排序次序為遞增。

第一次要求 [索引] 頁面時，沒有任何查詢字串。 學生會依姓氏的遞增順序顯示，這是 `switch` 陳述式中失敗案例所建立的預設值。 使用者按一下資料行標題超連結時，適當的 `sortOrder` 值將會在查詢字串值中提供。

檢視會使用兩個 `ViewData` 項目 (NameSortParm 和 DateSortParm)，以適當的查詢字串值設定資料行標題超連結。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortOnly&highlight=3-4)]

這些是三元陳述式。 第一個陳述式指定當 `sortOrder` 參數為 null 或是空的時，NameSortParm 應該設定為 "name_desc"；否則它應該設定為空字串。 這兩個陳述式讓檢視能夠設定資料行標題超連結，如下所示：

|  目前排序次序  | 姓氏超連結 | 日期超連結 |
|:--------------------:|:-------------------:|:--------------:|
| 姓氏遞增  | descending          | ascending      |
| 姓氏遞減 | ascending           | ascending      |
| 日期遞增       | ascending           | descending     |
| 日期遞減      | ascending           | ascending      |

此方法使用 LINQ to Entities 來指定排序所依據的資料行。 此程式碼會在 switch 陳述式之前建立 `IQueryable` 變數、在 switch 陳述式中修改它，並在 `switch` 陳述式之後呼叫 `ToListAsync` 方法。 當您建立和修改 `IQueryable` 變數時，沒有查詢會傳送至資料庫。 在您呼叫 `ToListAsync` 等方法以將 `IQueryable` 物件轉換成集合之前，不會執行查詢。 因此，此程式碼會產生一個直到 `return View` 陳述式才會執行的單一查詢。

此程式碼可取得使用大量資料行數目的詳細資訊。 [本系列中的最後一個教學課程](advanced.md#dynamic-linq)示範如何撰寫程式碼，讓您以字串變數傳遞 `OrderBy` 資料行的名稱。

### <a name="add-column-heading-hyperlinks-to-the-student-index-view"></a>將資料行標題超連結新增至學生的 [索引] 檢視

以下列程式碼取代 *Views/Students/Index.cshtml* 中的程式碼，以新增資料行標題超連結。 變更的行已醒目提示。

[!code-html[](intro/samples/cu/Views/Students/Index2.cshtml?highlight=16,22)]

此程式碼使用 `ViewData` 屬性中的資訊，以適當的查詢字串值設定超連結。

執行應用程式，選取 [Students] 索引標籤，然後按一下 [姓氏] 和 [註冊日期] 資料行標題，以確認排序的運作正常。

![以姓名順序排列的 Students [索引] 頁面](sort-filter-page/_static/name-order.png)

## <a name="add-a-search-box-to-the-students-index-page"></a>將搜尋方塊新增至 Students 的 [索引] 頁面

若要將篩選新增至 Students 的 [索引] 頁面，您要將文字方塊和提交按鈕新增至檢視，並在 `Index` 方法中進行對應的變更。 文字方塊可讓您輸入要在名字和姓氏欄位中搜尋的字串。

### <a name="add-filtering-functionality-to-the-index-method"></a>將篩選功能新增至 Index 方法

在 *StudentsController.cs* 中，以下列程式碼取代 `Index` 方法 (變更已醒目提示)。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilter&highlight=1,5,9-13)]

您已將 `searchString` 參數新增至 `Index` 方法。 從將新增至 [索引] 檢視的文字方塊中接收搜尋字串值。 您也已在 LINQ 陳述式中新增 where 子句，該子句只會選取其名字或姓氏包含搜尋字串的學生。 唯有當具有要搜尋的值時，才會執行新增 where 子句的陳述式。

> [!NOTE]
> 在這裡，您可以在 `IQueryable` 物件上呼叫 `Where` 方法，而篩選將會在伺服器上處理。 在某些情況下，您可能會呼叫 `Where` 方法在記憶體內部集合上作為擴充方法。 (例如，假設您變更了 `_context.Students` 的參考，以便它參考傳回 `IEnumerable` 集合的存放庫方法，而不是參考 EF `DbSet`)。結果通常都是一樣的，但在某些情況下可能會不同。
>
>例如，.NET Framework 實作的 `Contains` 方法預設會執行區分大小寫的比較，但在 SQL Server 中，這取決於 SQL Server 執行個體的定序設定。 該設定預設為不區分大小寫。 您可以呼叫 `ToUpper` 方法，使測試明確地不區分大小寫：*Where(s => s.LastName.ToUpper().Contains(searchString.ToUpper())*。 如果您稍後變更程式碼，以使用傳回 `IEnumerable` 集合 (而不是 `IQueryable` 物件) 的存放庫，這會確保結果保持不變。 (當您在 `IEnumerable` 集合上呼叫 `Contains` 方法時，將取得 .NET Framework 實作；當您在 `IQueryable` 物件上呼叫它時，則會取得資料庫提供者實作。)不過，此解決方案會對效能帶來負面影響。 `ToUpper` 程式碼會將一個函式置於 TSQL SELECT 陳述式的 WHERE 子句中。 這會防止最佳化工具使用索引。 假設 SQL 大部分安裝為不區分大小寫，最好避免使用 `ToUpper` 程式碼，直到您移轉至區分大小寫的資料存放區為止。

### <a name="add-a-search-box-to-the-student-index-view"></a>將搜尋方塊新增至學生的 [索引] 檢視

在 *Views/Student/Index.cshtml* 中，於開始表格標記之前立即新增醒目提示的程式碼，以建立標題、文字方塊及 [搜尋]  按鈕。

[!code-html[](intro/samples/cu/Views/Students/Index3.cshtml?range=9-23&highlight=5-13)]

此程式碼會使用 `<form>` [標籤協助程式](xref:mvc/views/tag-helpers/intro) 來新增搜尋文字方塊和按鈕。 `<form>` 標籤協助程式預設會使用 POST 提交表單資料，這表示參數會以 HTTP 訊息本文傳遞，而不是以 URL 作為查詢字串傳遞。 當您指定 HTTP GET 時，表單資料會以 URL 中作為查詢字串傳遞，這可讓使用者為該 URL 加上書籤。 W3C 指導方針建議，只有在動作不會產生更新時才應使用 GET。

執行應用程式，選取 [Students] 索引標籤，輸入搜尋字串，然後按一下 [搜尋] 以確認篩選可以運作。

![含篩選的 Students [索引] 頁面](sort-filter-page/_static/filtering.png)

請注意，URL 中包含了搜尋字串。

```html
http://localhost:5813/Students?SearchString=an
```

如果您為此頁面加上書籤，則會在使用書籤時取得篩選的清單。 將 `method="get"` 新增至 `form` 標籤會導致查詢字串的產生。

在這個階段，如果您按一下資料行標題排序連結，將會遺失您在 [搜尋] 方塊中輸入的篩選值。 您將在下節修正該問題。

## <a name="add-paging-functionality-to-the-students-index-page"></a>將分頁功能新增至 Students 的 [索引] 頁面

若要將分頁新增至 Students 的 [索引] 頁面，您要建立使用 `Skip` 和 `Take` 陳述式的 `PaginatedList` 類別來篩選伺服器上的資料，而不是一直擷取資料表的所有資料列。 然後，您要在 `Index` 方法中進行其他變更，並將分頁按鈕新增至 `Index` 檢視。 下圖顯示分頁按鈕。

![含分頁連結的 Students [索引] 頁面](sort-filter-page/_static/paging.png)

在專案資料夾中，建立 `PaginatedList.cs`，然後以下列程式碼取代範本程式碼。

[!code-csharp[](intro/samples/cu/PaginatedList.cs)]

此程式碼中的 `CreateAsync` 方法會採用頁面大小和頁面數，並會將適當的 `Skip` 和 `Take` 陳述式套用至 `IQueryable`。 在 `IQueryable` 上呼叫 `ToListAsync` 時，會傳回僅包含所要求頁面的清單。 `HasPreviousPage` 和 `HasNextPage` 屬性可用來啟用或停用 [上一頁]  和 [下一頁] 分頁按鈕。

`CreateAsync` 方法用來建立 `PaginatedList<T>` 物件而不是建構函式，因為建構函式無法執行非同步程式碼。

## <a name="add-paging-functionality-to-the-index-method"></a>將分頁功能新增至 Index 方法

在 *StudentsController.cs* 中，以下列程式碼取代 `Index` 方法。

[!code-csharp[](intro/samples/cu/Controllers/StudentsController.cs?name=snippet_SortFilterPage&highlight=1-5,7,11-18,45-46)]

此程式碼會將頁碼參數、目前排序次序參數和目前篩選參數新增至方法簽章。

```csharp
public async Task<IActionResult> Index(
    string sortOrder,
    string currentFilter,
    string searchString,
    int? page)
```

第一次顯示頁面，或是使用者尚未按一下分頁或排序連結時，所有參數都會是 null。  如果按一下分頁連結，頁面變數將包含要顯示的頁碼。

名為 CurrentSort 的 `ViewData` 項目會提供使用目前排序次序的檢視，因為這必須包含在分頁連結中，才能保持與分頁時相同的排序順序。

名為 CurrentFilter 的 `ViewData` 項目則提供使用目前篩選字串的檢視。 此值必須包含在分頁連結中，才能維護分頁期間的篩選設定，而且它必須在頁面重新顯示時還原為文字方塊。

如果搜尋字串在分頁期間變更，頁面必須重設為 1，因為新的篩選可能會導致顯示不同的資料。 在文字方塊中輸入值並按下 [提交] 按鈕時，即會變更搜尋字串。 在此情況下，`searchString` 參數不是 null。

```csharp
if (searchString != null)
{
    page = 1;
}
else
{
    searchString = currentFilter;
}
```

在 `Index` 方法的結尾處，`PaginatedList.CreateAsync` 方法會以支援分頁的集合類型，將學生查詢轉換成學生單一頁面。 該學生單一頁面接著會傳遞至檢視。

```csharp
return View(await PaginatedList<Student>.CreateAsync(students.AsNoTracking(), page ?? 1, pageSize));
```

`PaginatedList.CreateAsync` 方法會採用頁面數。 兩個問號代表 null 聯合運算子。 Null 聯合運算子將針對可為 Null 的型別定義預設值；`(page ?? 1)` 運算式表示在它含有值時會傳回值 `page`，或在 `page` 為 null 時傳回 1。

## <a name="add-paging-links-to-the-student-index-view"></a>將分頁連結新增至學生 [索引] 檢視

在 *Views/Students/Index.cshtml* 中，以下列程式碼取代現有程式碼。 所做的變更已醒目提示。

[!code-html[](intro/samples/cu/Views/Students/Index.cshtml?highlight=1,27,30,33,61-79)]

頁面頂端的 `@model` 陳述式指定檢視現在會取得 `PaginatedList<T>` 物件，而不是 `List<T>` 物件。

資料行標頭連結會使用查詢字串，將目前的搜尋字串傳遞至控制器，讓使用者可以在篩選結果內排序：

```html
<a asp-action="Index" asp-route-sortOrder="@ViewData["DateSortParm"]" asp-route-currentFilter ="@ViewData["CurrentFilter"]">Enrollment Date</a>
```

分頁按鈕會透過標籤協助程式來顯示：

```html
<a asp-action="Index"
   asp-route-sortOrder="@ViewData["CurrentSort"]"
   asp-route-page="@(Model.PageIndex - 1)"
   asp-route-currentFilter="@ViewData["CurrentFilter"]"
   class="btn btn-default @prevDisabled">
   Previous
</a>
```

執行應用程式並移至 Students 頁面。

![含分頁連結的 Students [索引] 頁面](sort-filter-page/_static/paging.png)

以不同排序次序按一下分頁連結，以確定分頁運作正常。 然後輸入搜尋字串並再次嘗試分頁，以確認分頁的排序和篩選能正確運作。

## <a name="create-an-about-page-that-shows-student-statistics"></a>建立顯示學生統計資料的 About 頁面

對於 Contoso 大學網站的 **About** 頁面，您將顯示每個註冊日期已有多少學生註冊。 這需要對群組進行分組和簡單計算。 若要完成此工作，您需要執行下列作業：

* 針對您需要傳遞至檢視的資料，建立檢視模型類別。

* 修改 Home 控制器中的 About 方法。

* 修改 About 檢視。

### <a name="create-the-view-model"></a>建立檢視模型

在 *Models* 資料夾中建立 *SchoolViewModels* 資料夾。

在新的資料夾中，新增類別檔案 *EnrollmentDateGroup.cs*，並以下列程式碼取代範本程式碼：

[!code-csharp[](intro/samples/cu/Models/SchoolViewModels/EnrollmentDateGroup.cs)]

### <a name="modify-the-home-controller"></a>修改 Home 控制器

在 *HomeController.cs* 中，將下列 using 陳述式新增在檔案頂最上方：

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_Usings1)]

在類別的左大括弧之後立即新增資料庫內容的類別變數，並從 ASP.NET Core DI 取得內容的執行個體：

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_AddContext&highlight=3,5,7)]

以下列程式碼取代 `About` 方法：

[!code-csharp[](intro/samples/cu/Controllers/HomeController.cs?name=snippet_UseDbSet)]

LINQ 陳述式會依註冊日期將學生實體組成群組、計算每個群組中的實體數目、將結果儲存在 `EnrollmentDateGroup` 檢視模型物件的集合中。
> [!NOTE] 
> 在 Entity Framework Core 1.0 版中，整個結果集會傳回到用戶端，並且在用戶端上完成群組作業。 在某些情況下，這可能會造成效能問題。 請務必使用生產環境數量的資料來測試效能，如有必要，請使用原始 SQL 在伺服器上執行群組作業。 如需如何使用原始 SQL 的資訊，請參閱[本系列的最後一個教學課程](advanced.md)。

### <a name="modify-the-about-view"></a>修改 About 檢視

以下列程式碼取代 *Views/Home/About.cshtml* 檔案中的程式碼：

[!code-html[](intro/samples/cu/Views/Home/About.cshtml)]

執行應用程式並移至 About 頁面。 每個註冊日期的學生人數將會顯示在資料表中。

![About 頁面](sort-filter-page/_static/about.png)

## <a name="summary"></a>總結

在本教學課程中，您已了解如何執行排序、篩選、分頁和群組。 在下一個教學課程中，您將學習如何使用移轉來處理資料模型變更。

> [!div class="step-by-step"]
> [上一頁](crud.md)
> [下一頁](migrations.md)  
