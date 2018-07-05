---
uid: mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
title: 實作有效率的資料分頁 |Microsoft Docs
author: microsoft
description: 步驟 8 示範如何將分頁支援新增至我們 /Dinners URL，讓不會顯示 1000 個 dinners 一次，我們只會顯示在 10 即將推出的 dinners...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: adea836d-dbc2-4005-94ea-53aef09e9e34
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/implement-efficient-data-paging
msc.type: authoredcontent
ms.openlocfilehash: e6347c817c7518ef96ffbbf83cf98dd4dc6c011e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372445"
---
<a name="implement-efficient-data-paging"></a>實作有效率的資料分頁
====================
by [Microsoft](https://github.com/microsoft)

[下載 PDF](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> 這是一套免費的步驟 8 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。
> 
> 步驟 8 示範如何將分頁支援加入至我們 /Dinners URL，以便不會顯示 1000 個 dinners 一次，我們將只顯示 10 即將推出的 dinners 一次-並讓使用者回到頁面和向前逐步執行整個清單在 SEO 易懂的方式。
> 
> 如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。


## <a name="nerddinner-step-8-paging-support"></a>NerdDinner 步驟 8： 分頁支援

如果我們的網站成功時，會有數千個即將推出的 dinners。 我們要確定我們的 UI 進行調整以處理所有這些 dinners，而且可讓使用者瀏覽它們。 若要這麼做，我們將新增的分頁支援我們 */Dinners* URL，因此在我們一次，顯示 1000 個 dinners 將只顯示 10 即將推出的 dinners 一次-並允許使用者頁面後和向前逐步執行中的整個清單SEO 易懂的方式。

### <a name="index-action-method-recap"></a>Index （） 動作方法的回顧

Index （） 動作方法，我們 DinnersController 類別內目前看起來如下所示：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample1.cs)]

若要提出要求時 */Dinners* URL，它會擷取一份所有即將推出的 dinners 和接下來會呈現出的所有清單：

![](implement-efficient-data-paging/_static/image1.png)

### <a name="understanding-iquerablelttgt"></a>了解 IQuerable&lt;T&gt;

*IQueryable&lt;T&gt;* 是引進了與 LINQ 為一部分的.NET 3.5 的介面。 它可讓我們可以利用實作分頁支援的強大 「 延後執行 」 案例。

在我們 DinnerRepository 中，我們會傳回 IQueryable&lt;Dinner&gt;從我們 FindUpcomingDinners() 方法的順序：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample2.cs)]

IQueryable&lt;Dinner&gt;我們 FindUpcomingDinners() 方法所傳回的物件會封裝 Dinner 物件擷取資料庫使用 LINQ to SQL 查詢。 重要的是，它將不會執行對資料庫的查詢，直到我們嘗試存取/逐一查看資料在查詢中，或在其上呼叫 Tolist 方法。 呼叫我們 FindUpcomingDinners() 方法的程式碼可以選擇性地選擇將額外的 「 鏈結 」 作業/篩選新增至 IQueryable&lt;Dinner&gt;之前執行的查詢物件。 LINQ to SQL 就夠聰明，無法合併對資料庫執行查詢時要求的資料。

若要實作分頁邏輯我們可以更新我們 DinnersController index （） 動作方法，使它套用額外的"Skip"和"Take"運算子來傳回 IQueryable&lt;Dinner&gt;之前在其上呼叫 Tolist 的序列：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample3.cs)]

上述程式碼會略過資料庫中的前 10 個即將推出 dinners，並傳回上一步 20 dinners。 LINQ to SQL 是聰明，足以建構執行此略過邏輯中的 SQL database-而不是在 web 伺服器的最佳化的 SQL 查詢。 這表示，即使我們即將推出的 Dinners 的數百萬個資料庫中，只有 10 的個我們想要將會擷取 （讓它成為有效率且可調整） 此要求的一部分。

### <a name="adding-a-page-value-to-the-url"></a>URL 中加入"page"值

而不是硬式編碼的特定頁面範圍，我們會想我們的 Url 以包括表示使用者正在要求哪一個 Dinner 範圍的 「 頁面 」 參數。

#### <a name="using-a-querystring-value"></a>使用查詢字串值

下列程式碼示範我們如何更新我們的 index （） 動作方法，以支援的查詢字串參數，並啟用 Url，例如 */Dinners？ 頁面 = 2*:

[!code-csharp[Main](implement-efficient-data-paging/samples/sample4.cs)]

上述的 index （） 動作方法具有名為 「 頁面 」 的參數。 參數宣告為可為 null 的整數 (這是什麼 int？ 表示)。 這表示 */Dinners？ 頁面 = 2* URL 將會導致不可傳遞為參數值的"2"的值。 */Dinners* URL （不含查詢字串值） 會造成要傳遞 null 值。

我們會乘以分頁值的頁面大小 （在此情況下 10 個資料列） 來判斷多少 dinners 略過。 我們會使用[C# null"聯合"運算子 （？）](https://weblogs.asp.net/scottgu/archive/2007/09/20/the-new-c-null-coalescing-operator-and-using-it-with-linq.aspx)處理可為 null 的類型時，這很有用。 如果頁面參數為 null，上述程式碼將指派頁面 0 的值。

#### <a name="using-embedded-url-values"></a>使用內嵌 URL 值

使用查詢字串值的替代方案是內嵌實際的 URL 本身內的頁面參數。 例如： */Dinners/Page/2*或是*Dinners/2*。 ASP.NET MVC 包含功能強大的 URL 路由引擎，可讓您輕鬆地支援這種情況。

我們可以的對應至我們想要任何控制器類別或動作方法的任何連入的 URL 或 URL 格式來註冊自訂的路由規則。 我們只需要待辦事項是開啟我們的專案中的 Global.asax 檔案：

![](implement-efficient-data-paging/_static/image2.png)

然後註冊新的對應規則，使用 MapRoute() helper 方法，例如路由的第一個呼叫。MapRoute() 下方：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample5.cs)]

以上所述，我們要註冊新的路由規則，名為"UpcomingDinners 」。 我們會指出其 URL 格式"Dinners/網頁 / {}"– 其中 {頁面} 是內嵌在 URL 內的參數值。 MapRoute() 方法的第三個參數指出我們應該將對應符合這種格式來 DinnersController 類別上的 index （） 動作方法的 Url。

我們可以使用完全相同我們先前使用我們的 Querystring 案例 – 除了現在我們 「 頁面 」 的參數會從 URL，而且不是查詢字串的 index （） 程式碼：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample6.cs)]

現在當我們執行應用程式，並輸入並 */Dinners*我們會看到前 10 個即將推出 dinners:

![](implement-efficient-data-paging/_static/image3.png)

當我們輸入 */Dinners/Page/1*我們會看到 dinners 的下一個頁面：

![](implement-efficient-data-paging/_static/image4.png)

### <a name="adding-page-navigation-ui"></a>新增頁面導覽 UI

若要完成我們的分頁案例的最後一個步驟是實作 下一步 」 和 「 之前 」 導覽 UI 中檢視範本，讓使用者可以輕鬆地略過 Dinner 資料。

若要正確實作此，我們必須在資料庫中，知道 Dinners 總數以及如何多頁資料這會轉譯為。 我們接著要計算目前所要求的 「 頁面 」 值的開頭或結尾的資料，以及顯示或隱藏 「 之前 」 和 「 下一步 UI 據此。 我們可以在我們的 index （） 動作方法內實作此邏輯。 或者我們可以將協助程式類別，加入我們以多重複使用的方式會封裝這個邏輯的專案。

以下是一個簡單的"PaginatedList"helper 類別，衍生自清單&lt;T&gt;集合類別，內建於.NET Framework。 它會實作可用來重新編頁的 IQueryable 資料的任何序列的重複使用的集合類別。 NerdDinner 應用程式中我們要讓它運作 IQueryable&lt;Dinner&gt;結果，但它可以輕易地使用針對 IQueryable&lt;產品&gt;或 IQueryable&lt;客戶&gt;導致其他應用程式案例：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample7.cs)]

請注意，上例計算的方式，然後公開屬性，例如 「 PageIndex"、"PageSize"、"TotalCount"和 「 TotalPages"。 它也會公開兩個協助程式屬性"HasPreviousPage 」 和 「 HasNextPage"，表示集合中的資料頁的開頭或結尾的原始順序。 上述程式碼會導致兩個 SQL 查詢執行-第一個擷取 Dinner 物件總數的計數 （這不會傳回物件，而不是它會執行"SELECT COUNT 」 陳述式會傳回整數），若要擷取的列，第二個我們需要從目前的頁面資料的資料庫的資料。

然後，我們可以更新我們 DinnersController.Index() 協助程式方法，以建立 PaginatedList&lt;Dinner&gt;從我們 DinnerRepository.FindUpcomingDinners() 產生，並將它傳遞至我們的檢視範本：

[!code-csharp[Main](implement-efficient-data-paging/samples/sample8.cs)]

然後，我們可以更新 \Views\Dinners\Index.aspx 檢視範本繼承自 ViewPage&lt;NerdDinner.Helpers.PaginatedList&lt;Dinner&gt; &gt;而不是 ViewPage&lt;IEnumerable&lt;Dinner&gt;&gt;，然後將下列程式碼新增至我們檢視範本來顯示或隱藏 下一步 和 上一個巡覽 UI 底部：

[!code-aspx[Main](implement-efficient-data-paging/samples/sample9.aspx)]

請注意，上例我們如何使用 Html.RouteLink() helper 方法來產生我們的超連結。 這個方法很類似於我們先前使用過 Html.ActionLink() 協助程式方法。 差別在於，我們會產生使用 「 UpcomingDinners"路由規則設定我們的 Global.asax 檔案中的 URL。 這可確保我們將具有格式我們 index （） 動作方法產生 Url: */Dinners/網頁 / {page}* – 其中 {page} 值是的變數，我們會提供上述根據目前 PageIndex。

看現在當我們執行我們應用程式一次一次的 10 個 dinners 在我們的瀏覽器中：

![](implement-efficient-data-paging/_static/image5.png)

我們也有&lt; &lt; &lt;並&gt; &gt; &gt;巡覽 UI 底部的頁面，好讓我們略過向前和向後透過我們的資料會使用搜尋引擎存取的 Url:

![](implement-efficient-data-paging/_static/image6.png)

| **側邊的主題內容： 了解 IQueryable 的含意&lt;T&gt;** |
| --- |
| IQueryable&lt;T&gt;是一項非常強大的功能，可讓各種有趣的延後的執行案例 （例如分頁和轉譯緩衝處理架構的查詢）。 為所有的強大功能，您想要謹慎使用方式，並確定不被濫用。 請務必辨識該傳回 IQueryable&lt;T&gt;結果從儲存機制可讓呼叫的程式碼，以附加於鏈結的運算子方法，並因此參與 ultimate 的查詢執行。 如果不想提供呼叫端程式碼這項功能，則您應該會傳回傳回 IList&lt;T&gt;或 IEnumerable&lt;T&gt;結果-其中包含已執行查詢的結果。 針對分頁案例這會要求您將實際的資料分頁邏輯推送至儲存機制方法呼叫。 在此案例中，我們可能會更新我們 FindUpcomingDinners() 搜尋工具方法中，使其中一個傳回 PaginatedList 的簽章： PaginatedList&lt; Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize） {} 或傳回 IList&lt;Dinner&gt;，使用"totalCount"out 參數傳回 Dinners 總數： IList&lt;Dinner&gt; FindUpcomingDinners （int pageIndex，int pageSize，放大 int totalCount） {} |

### <a name="next-step"></a>下一個步驟

現在來看看我們如何新增至應用程式支援的驗證和授權。

> [!div class="step-by-step"]
> [上一頁](re-use-ui-using-master-pages-and-partials.md)
> [下一頁](secure-applications-using-authentication-and-authorization.md)
