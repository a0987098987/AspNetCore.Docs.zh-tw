---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: 實作有效率的資料分頁 |Microsoft 文件
author: microsoft
description: 步驟 8 示範如何將分頁支援新增至我們 /Dinners URL，以便而不是顯示 1000 個 dinners 一次，我們只會顯示在 10 即將 dinners...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: 0188e21438820adf2adbe05b047fdb772540e1a0
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30873244"
---
<a name="implement-efficient-data-paging"></a>實作有效率的資料分頁
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 8 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。
> 
> 步驟 8 示範如何將分頁支援新增至我們 /Dinners URL，以便而不是顯示 1000 個 dinners 一次，我們將只顯示 10 個即將 dinners 一次-並讓使用者可以回到頁面和向前逐步執行整個清單中 SEO 易懂的方式。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner 步驟 8： 分頁支援

如果網站順利完成時，會有數千個即將 dinners。 我們需要確定我們的 UI 調整為處理所有的這些 dinners，而且可讓使用者瀏覽它們。 若要啟用此功能，我們會將新增的分頁支援我們 */Dinners* URL 一次，我們在顯示 1000 個 dinners 的因此，而是會只顯示 10 個即將 dinners 一次-並讓使用者可以頁面後和向前逐步執行中的整個清單SEO 易懂的方式。

### <a name="index-action-method-recap"></a>Index 動作方法的重點

我們 DinnersController 類別內的 index 動作方法目前看起來類似下面的：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

發出要求來 */Dinners* URL，它會擷取一份所有即將 dinners 並接下來會呈現所有它們：

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>了解 IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;* 是使用 LINQ 做為.NET 3.5 的一部分導入的介面。 它可讓我們可以利用實作的分頁支援的功能強大 「 延後執行 」 案例。

我們會在我們 DinnerRepository 傳回 IQueryable&lt;Dinner&gt;順序從我們 FindUpcomingDinners() 方法：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt;我們 FindUpcomingDinners() 方法所傳回的物件會封裝 Dinner 物件擷取資料庫使用 LINQ to SQL 查詢。 重要的是，它將不會對資料庫執行查詢直到我們嘗試存取/反覆查看的資料，在查詢中，或我們上呼叫 tolist （） 方法。 呼叫我們 FindUpcomingDinners() 方法的程式碼可選擇性決定是否要加入其他的 「 鏈結 」 作業/篩選至 IQueryable&lt;Dinner&gt;之前執行的查詢物件。 LINQ to SQL 是再聰明，可以要求資料時，執行對資料庫合併的查詢。

若要實作分頁邏輯我們可以更新我們 DinnersController index 動作方法使其套用至傳回 IQueryable 的其他 「 略過 」 和 「 使用 」 運算子&lt;Dinner&gt;呼叫 tolist （） 之前的順序：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

上述程式碼略過前 10 個即將 dinners，在資料庫中，，然後傳回回 20 dinners。 LINQ to SQL 是聰明，可以建構最佳化的 SQL 查詢，以執行此略過邏輯中的 SQL database-而不是在 web 伺服器。 這表示即使有數百萬個即將 Dinners 資料庫中，只有的 10 我們想要將擷取的這項要求 （讓它成為有效率且可調整） 的一部分。

### <a name="adding-a-page-value-to-the-url"></a>URL 中加入"page"值

而不是硬式編碼的特定頁面範圍，我們希望我們 Url 以包括表示使用者正在要求哪一個 Dinner 範圍的 「 頁面 」 參數。

#### <a name="using-a-querystring-value"></a>使用查詢字串值

下列程式碼示範如何我們可以更新我們 index （） 的動作方法來支援查詢字串參數，並啟用 Url，例如 */Dinners？ 頁面 = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

上述的 index 動作方法具有名為 「 頁面 」 的參數。 參數宣告為可為 null 的整數 (這是什麼 int？ 表示)。 這表示 */Dinners？ 頁面 = 2* URL 會導致值為"2"傳遞為參數值。 */Dinners* URL （不含查詢字串值） 會導致要傳遞 null 值。

我們會頁面值乘以頁面大小 （在此情況下 10 個資料列） 來判斷多少 dinners 略過。 我們使用[C# null"聯合 」 運算子 （？）](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx)處理可為 null 類型時，會很有用。 上述程式碼將指派頁面 0 的值如果頁面參數為 null。

#### <a name="using-embedded-url-values"></a>使用內嵌 URL 值

使用查詢字串值的替代方式是將內嵌頁面內之參數的實際 URL 本身。 例如： */Dinners/Page/2*或 */Dinners/2*。 ASP.NET MVC 包含功能強大的 URL 路由引擎，以簡化支援像這樣的情況。

我們可以註冊自訂的路由規則的對應要我們想要任何控制器類別或動作方法的任何連入的 URL 或 URL 格式。 我們只需要待辦事項是開啟內受測專案的 Global.asax 檔案：

![](implement-efficient-data-paging/_static/image2.png)

然後再註冊新的對應規則使用 MapRoute() helper 方法，例如路由的第一個呼叫。MapRoute() 下方：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

上方我們註冊新的路由規則，名為"UpcomingDinners"。 我們會指出其 URL 格式"Dinners/頁面 / {頁面}"– {頁面} 其中是 URL 中內嵌參數值。 MapRoute() 方法的第三個參數，表示我們應該對應符合這種格式來 DinnersController 類別上的 index 動作方法的 Url。

我們可以使用完全相同我們之前與我們的 Querystring 案例 – 除了現在我們的 「 頁面 」 參數會從 URL，而且不是查詢字串的 index （） 程式碼：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

現在當我們執行應用程式，並在輸入和 */Dinners*我們會看到前 10 個即將 dinners:

![](implement-efficient-data-paging/_static/image3.png)

和中，當我們輸入 */Dinners/Page/1*我們會看到 dinners 的下一個頁面：

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>加入頁面巡覽 UI

若要完成我們分頁案例的最後一個步驟是實作 下一步 」 和 「 之前 」 內的導覽 UI，讓使用者可以輕鬆地略過 Dinner 資料我們檢視範本。

若要正確實作此，我們需要知道 Dinners 的總數在資料庫中，以及如何多頁資料這會轉譯成。 我們再需要計算目前所要求的 「 頁面 」 值的開頭或結尾的資料，以及顯示或隱藏 「 之前 」 和 「 下一步 UI 據此。 我們無法在我們的 index 動作方法內實作此邏輯。 或者我們可以將協助程式類別，加入受測專案會封裝這個邏輯更重複使用的方式。

以下是一個簡單的"PaginatedList"helper 類別，衍生自清單&lt;T&gt;內建的集合類別的.NET Framework。 它會實作可用來重新編頁 IQueryable 資料的任何一串的可重複使用的集合類別。 在我們 NerdDinner 應用程式中我們將有它透過 IQueryable 運作&lt;Dinner&gt;結果，但它無法輕易地用於 IQueryable&lt;產品&gt;或 IQueryable&lt;客戶&gt;導致其他應用程式案例：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

請注意，上述方式計算，然後公開屬性例如"PageIndex"、"PageSize"、"TotalCount"和 「 TotalPages"。 它也會公開兩個 helper 屬性 」 HasPreviousPage"和"HasNextPage"，表示集合中的資料頁的開頭或原始序列的結尾。 上述程式碼會導致兩個 SQL 查詢執行的第一個擷取的 Dinner 物件總數的計數 （不會傳回物件 – 而不是它會執行傳回一個整數的 「 選取計數 」 陳述式），第二個擷取的列我們需要從我們的資料庫目前的頁面資料的資料。

然後，我們可以更新我們 DinnersController.Index() helper 方法，以建立 PaginatedList&lt;Dinner&gt;從我們 DinnerRepository.FindUpcomingDinners() 產生，並將它傳遞至我們檢視範本：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

然後，我們可以更新繼承自 ViewPage \Views\Dinners\Index.aspx 檢視範本&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt;而不是 ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;，然後將下列程式碼新增至我們檢視範本以顯示或隱藏下一頁和上一頁巡覽 UI 底部：

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

請注意，上述方式使用 Html.RouteLink() helper 方法來產生我們超連結。 這個方法是我們先前使用過的 Html.ActionLink() helper 方法類似。 差別在於，我們會產生使用 「 UpcomingDinners"路由規則，在我們設定我們 Global.asax 檔案中的 URL。 這可確保我們將以我們採用之格式的 index 動作方法產生 Url: */Dinners/頁面 / {頁面}* – 其中 {分頁} 值是我們提供上述根據目前 PageIndex 的變數。

現在當我們執行我們的應用程式再次我們會看到一次 10 dinners 我們瀏覽器中：

![](implement-efficient-data-paging/_static/image5.png)

我們也有&lt; &lt; &lt;和&gt; &gt; &gt;巡覽 UI 底部的頁面，讓我們來略過向前和回溯透過我們的資料使用搜尋搜尋引擎可存取的 Url:

![](implement-efficient-data-paging/_static/image6.png)

| **側邊主題內容： 了解 IQueryable 的含意&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt;的非常強大的功能，可讓各種有趣的延後的執行案例 （例如分頁和組合基礎查詢）。 做為其中所有功能強大的功能，您想要的是小心使用它，並確定不被濫用。 請務必識別傳回 IQueryable&lt;T&gt;結果從您的儲存機制可讓呼叫端程式碼附加鏈結的運算子方法，並因此參與最終查詢執行。 如果不想提供呼叫的程式碼這項功能，則您應該會傳回回 IList&lt;T&gt;或 IEnumerable&lt;T&gt;結果-包含已執行之查詢的結果。 分頁方案的這會需要您的實際資料分頁邏輯推送儲存機制所呼叫方法。 在此案例中，我們可能會更新 FindUpcomingDinners() finder 方法有可能傳回 PaginatedList 簽章： PaginatedList&lt; Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize） {} 傳回回 IList 或&lt;Dinner&gt;，使用"totalCount"out 參數傳回 Dinners 總數： IList&lt;Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize，out int totalCount） {} |

### <a name="next-step"></a>下一個步驟

我們現在看我們可以將驗證和授權支援新增至我們的應用程式的方式。

> [!div class="step-by-step"]
> [上一頁](re-use-ui-using-master-pages-and-partials.md)
> [下一頁](secure-applications-using-authentication-and-authorization.md)
