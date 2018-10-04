---
uid: mvc/overview/getting-started/introduction/adding-search
title: 搜尋 |Microsoft Docs
author: Rick-Anderson
description: ''
ms.author: riande
ms.date: 05/22/2015
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 2cf2274a5592e1f073e62c9b8a789fbb61e23a51
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/04/2018
ms.locfileid: "48576372"
---
<a name="search"></a>搜尋
====================
藉由[Rick Anderson]((https://twitter.com/RickAndMSFT))

[!INCLUDE [Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>新增搜尋方法和搜尋檢視

在本節中，您會將搜尋功能加入`Index`動作方法，可讓您搜尋的內容類型或名稱的電影。

## <a name="updating-the-index-form"></a>更新索引表單

藉由更新開始`Index`至現有的動作方法`MoviesController`類別。 程式碼如下：

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

第一行`Index`方法會建立下列[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查詢，以選取電影：

[!code-csharp[Main](adding-search/samples/sample2.cs)]

到目前為止，定義查詢，但還未對資料庫執行。

如果`searchString`參數包含字串，會修改電影查詢來篩選搜尋字串，使用下列程式碼的值：

[!code-csharp[Main](adding-search/samples/sample3.cs)]

上述 `s => s.Title` 程式碼是 [Lambda 運算式](https://msdn.microsoft.com/library/bb397687.aspx)。 Lambda 會用來在以方法為基礎[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)這類查詢作為標準查詢運算子方法的引數[其中](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上述的程式碼中使用的方法。 當定義或修改這些呼叫的方法，例如，不會執行 LINQ 查詢`Where`或`OrderBy`。 相反地，延後查詢執行，這表示延遲評估的運算式，直到實際反覆運算其實現的值或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)呼叫方法。 在 `Search`範例中，在執行查詢*Index.cshtml*檢視。 如需延後查詢執行的詳細資訊，請參閱[查詢執行](https://msdn.microsoft.com/library/bb738633.aspx)。

> [!NOTE]
> [Contains](https://msdn.microsoft.com/library/bb155125.aspx)方法是在資料庫上執行，不是 c# 程式碼上方。 在資料庫上， [Contains](https://msdn.microsoft.com/library/bb155125.aspx)對應至[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)，這是不區分大小寫。

現在您可以更新`Index`會向使用者顯示表單的檢視。

執行應用程式，並瀏覽至 */Movies/Index*。 將查詢字串 (例如 `?searchString=ghost`) 附加至 URL。 隨即顯示篩選過的電影。

![SearchQryStr](adding-search/_static/image1.png)

如果您變更的簽章`Index`方法具有一個名為參數`id`，則`id`參數會比對`{id}`預留位置的預設路由中的設定*應用程式\_才能執行RouteConfig.cs*檔案。

[!code-json[Main](adding-search/samples/sample4.json)]

原始`Index`方法看起來像這樣：

[!code-csharp[Main](adding-search/samples/sample5.cs)]

已修改`Index`方法看起來如下：

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。

![](adding-search/_static/image2.png)

但是，您不能期望使用者在每次想要搜尋電影時修改 URL。 所以現在您將 UI，可協助他們篩選電影。 如果您變更的簽章`Index`方法，以測試如何傳遞路由繫結的 ID 參數，將它變更以便您`Index`方法會採用字串參數，名為`searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

開啟*Views\Movies\Index.cshtml*檔案，然後之後`@Html.ActionLink("Create New", "Create")`，新增下列醒目提示的表單標記：

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm`協助程式會建立開頭`<form>`標記。 `Html.BeginForm`協助程式會導致表單張貼至本身，當使用者提交表單時，依序按一下**篩選** 按鈕。

Visual Studio 2013 都有一項不錯的改進時顯示和編輯檢視檔案。 當您執行應用程式檢視檔案開啟時，Visual Studio 2013 會叫用正確的控制器動作方法，以顯示檢視。

![](adding-search/_static/image3.png)

（如圖所示），在 Visual Studio 中開啟 [索引] 檢視中，點選 Ctr f5 鍵或 F5 以執行應用程式然後再次嘗試搜尋電影。

![](adding-search/_static/image4.png)

沒有任何`HttpPost`多載`Index`方法。 您不需要它，因為方法不會變更狀態的應用程式，只會篩選資料。

您可以新增下列 `HttpPost Index` 方法。 在此情況下，會比對動作啟動程式`HttpPost Index`方法，而`HttpPost Index`方法會執行下面的影像所示。

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

不過，即使您新增這個 `HttpPost` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。 假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。 請注意，HTTP POST 要求的 URL 是 GET 要求 URL （localhost: xxxxx/Movies/Index） 相同--在 URL 本身沒有搜尋資訊。 權限現在，搜尋字串資訊會傳送到伺服器做為表單欄位值。 這表示您無法擷取該搜尋資訊以加上書籤，或傳送給朋友在 URL 中。

解決方法是使用的多載`BeginForm`可指定 POST 要求，應該將搜尋資訊新增至 URL，以及它應路由至`HttpGet`新版`Index`方法。 取代現有無參數`BeginForm`方法，以下列標記：

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

現在當您提交搜尋時，URL 會包含搜尋查詢字串。 即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>新增依內容類型搜尋

如果您已新增`HttpPost`新版`Index`方法，立即將其刪除。

接下來，您將新增特定功能，讓使用者進行搜尋的電影內容類型。 以下列程式碼取代 `Index` 方法：

[!code-csharp[Main](adding-search/samples/sample11.cs)]

本版`Index`方法會採用額外的參數，也就是`movieGenre`。 建立程式碼的前幾行`List`物件來保存從資料庫的電影內容類型。

下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。

[!code-csharp[Main](adding-search/samples/sample12.cs)]

程式碼會使用`AddRange`方法的泛型`List`將清單中的所有不同的內容類型的集合。 (不含`Distinct`修飾詞，會加入重複的內容類型 — 比方說，喜劇會新增兩次在我們的範例)。 程式碼再將儲存在內容類型清單`ViewBag.MovieGenre`物件。 儲存類別目錄資料 （這類電影內容類型的） 當做[SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)物件中`ViewBag`，則存取下拉式清單方塊中的類別目錄資料的 MVC 應用程式的典型方法。

下列程式碼示範如何檢查`movieGenre`參數。 如果不是空的程式碼進一步限制電影查詢來限制所選的影片來指定內容類型。

[!code-csharp[Main](adding-search/samples/sample13.cs)]

如先前所述，不執行查詢上的資料基底直到電影清單逐一 (這會在檢視中，在後`Index`動作方法傳回)。

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>將標記新增至 [索引] 檢視，以支援依內容類型搜尋

新增`Html.DropDownList`協助專家*Views\Movies\Index.cshtml*檔案，之前`TextBox`協助程式。 完整的標記如下所示：

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

下列程式碼：

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

參數"MovieGenre 」 提供的索引鍵`DropDownList`若要尋找的 helper`IEnumerable<SelectListItem>`在`ViewBag`。 `ViewBag`填入動作方法中：

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

「 全部 」 提供了選項的標籤參數。 如果您在瀏覽器中檢查該選擇，您會看到其 「 值 」 屬性為空白。 因為我們的控制器只會篩選`if`字串不是`null`或空的提交的值是空`movieGenre`顯示所有的內容類型。

您也可以設定預設會選取的選項。 如果您想"喜劇"做為預設選項，想要變更在控制器中的程式碼就像這樣：

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

執行應用程式，並瀏覽至 */Movies/Index*。 依內容類型、 電影名稱，以及這兩個準則，請嘗試搜尋。

![](adding-search/_static/image8.png)

在本節中，您建立的搜尋動作方法和檢視，可讓使用者進行搜尋電影標題和內容類型。 在下一步 區段中，您將探討如何將屬性新增至`Movie`模型，以及如何將會自動建立測試資料庫的初始設定式。

> [!div class="step-by-step"]
> [上一頁](examining-the-edit-methods-and-edit-view.md)
> [下一頁](adding-a-new-field.md)
