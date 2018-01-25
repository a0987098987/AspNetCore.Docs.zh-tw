---
uid: mvc/overview/getting-started/introduction/adding-search
title: "搜尋 |Microsoft 文件"
author: Rick-Anderson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/22/2015
ms.topic: article
ms.assetid: df001954-18bf-4550-b03d-43911a0ea186
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/introduction/adding-search
msc.type: authoredcontent
ms.openlocfilehash: 116f681e14af0a09a4eb1502ef9f057c5db2f97d
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="search"></a>搜尋
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

[!INCLUDE[Tutorial Note](sample/code-location.md)]

## <a name="adding-a-search-method-and-search-view"></a>加入搜尋方法和搜尋檢視

在本節中，您可以加入搜尋功能加入`Index`動作方法，可讓您搜尋電影依類型或名稱。

## <a name="updating-the-index-form"></a>更新索引表單

藉由更新開始`Index`至現有的動作方法`MoviesController`類別。 下列程式碼：

[!code-csharp[Main](adding-search/samples/sample1.cs?highlight=1,6-9)]

第一行`Index`方法會建立下列[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)查詢，以選取影片：

[!code-csharp[Main](adding-search/samples/sample2.cs)]

此時，定義查詢，但還未對資料庫執行。

如果`searchString`參數包含字串，若要篩選搜尋字串，使用下列程式碼的值修改電影查詢：

[!code-csharp[Main](adding-search/samples/sample3.cs)]

上述 `s => s.Title` 程式碼是 [Lambda 運算式](https://msdn.microsoft.com/library/bb397687.aspx)。 Lambda 會用來在以方法為基礎[LINQ](https://msdn.microsoft.com/library/bb397926.aspx)這類查詢做為標準查詢運算子方法引數[其中](https://msdn.microsoft.com/library/system.linq.enumerable.where.aspx)上述程式碼中使用的方法。 LINQ 查詢不會執行，當它們被定義或修改這些呼叫的方法，例如`Where`或`OrderBy`。 相反地，延後查詢執行，這表示運算式的評估是否延遲到實際反覆查看其實現的值或[ `ToList` ](https://msdn.microsoft.com/library/bb342261.aspx)方法呼叫。 在`Search`範例中，在執行查詢*Index.cshtml*檢視。 如需延後查詢執行的詳細資訊，請參閱[查詢執行](https://msdn.microsoft.com/library/bb738633.aspx)。

> [!NOTE]
> [Contains](https://msdn.microsoft.com/library/bb155125.aspx)方法是在資料庫上執行，不是 c# 程式碼上方。 在資料庫上， [Contains](https://msdn.microsoft.com/library/bb155125.aspx)對應至[SQL LIKE](https://msdn.microsoft.com/library/ms179859.aspx)，這是不區分大小寫。

現在您可以更新`Index`會向使用者顯示表單的檢視。

執行應用程式，並瀏覽至*/電影/索引*。 將查詢字串 (例如 `?searchString=ghost`) 附加至 URL。 隨即顯示篩選過的電影。

![SearchQryStr](adding-search/_static/image1.png)

如果您變更的簽章`Index`方法有名稱為的參數`id`、`id`參數將會符合`{id}`預留位置預設路由的設定*應用程式\_才能執行RouteConfig.cs*檔案。

[!code-json[Main](adding-search/samples/sample4.json)]

原始`Index`方法看起來像這樣：

[!code-csharp[Main](adding-search/samples/sample5.cs)]

修改`Index`方法可能看起來如下：

[!code-csharp[Main](adding-search/samples/sample6.cs?highlight=1,3)]

您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。

![](adding-search/_static/image2.png)

但是，您不能期望使用者在每次想要搜尋電影時修改 URL。 因此，現在您將 UI 來協助它們篩選電影。 如果您已變更的簽章`Index`方法來測試如何傳遞路由繫結的 ID 參數，將它變更回，讓您`Index`方法接受字串參數名稱為`searchString`:

[!code-csharp[Main](adding-search/samples/sample7.cs)]

開啟*Views\Movies\Index.cshtml*檔案，然後之後`@Html.ActionLink("Create New", "Create")`，加入下面反白顯示的表單標記：

[!code-cshtml[Main](adding-search/samples/sample8.cshtml?highlight=12-15)]

`Html.BeginForm` Helper 會建立開頭`<form>`標記。 `Html.BeginForm` Helper 會導致表單張貼至本身，當使用者提交表單，即可**篩選** 按鈕。

Visual Studio 2013 有很棒的改進時顯示與編輯檢視檔案。 當您執行應用程式的檢視檔案開啟時，Visual Studio 2013 會叫用正確的控制器動作方法，以顯示檢視。

![](adding-search/_static/image3.png)

具備索引檢視 （如上面的影像所示），開啟 Visual Studio 中，點選 Ctr F5 執行應用程式，然後再次嘗試搜尋 電影。

![](adding-search/_static/image4.png)

沒有任何`HttpPost`多載`Index`方法。 您不需要它，因為方法不會變更狀態的應用程式，只要篩選資料。

您可以新增下列 `HttpPost Index` 方法。 在此情況下，會比對動作啟動程式`HttpPost Index`方法，而`HttpPost Index`方法會執行下面的影像中所示。

[!code-csharp[Main](adding-search/samples/sample9.cs)]

![SearchPostGhost](adding-search/_static/image5.png)

不過，即使您新增這個 `HttpPost` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。 假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。 請注意，HTTP POST 要求的 URL 是 GET 要求 （localhost:xxxxx/電影/索引） 的 URL 相同--在 URL 本身沒有搜尋資訊。 右現在，搜尋字串資訊會傳送到伺服器做為表單欄位值。 這表示您無法擷取書籤，或傳送給朋友在 URL 中搜尋資訊。

解決方案是使用的多載`BeginForm`，會指定 POST 要求應新增搜尋資訊的 url，應路由至`HttpGet`版本`Index`方法。 取代現有無參數`BeginForm`以下列標記的方法：

[!code-cshtml[Main](adding-search/samples/sample10.cshtml)]

![BeginFormPost_SM](adding-search/_static/image6.png)

現在當您提交搜尋時，URL 中包含的搜尋查詢字串。 即使您有 `HttpPost Index` 方法，搜尋也會移至 `HttpGet Index` 動作方法。

![IndexWithGetURL](adding-search/_static/image7.png)

## <a name="adding-search-by-genre"></a>加入搜尋的影片

如果您加入`HttpPost`版本`Index`方法，立即將它刪除。

接下來，您要加入的功能，讓使用者搜尋的電影內容類型。 以下列程式碼取代 `Index` 方法：

[!code-csharp[Main](adding-search/samples/sample11.cs)]

這個版本的`Index`方法會採用額外的參數，也就是`movieGenre`。 第一個幾行程式碼建立`List`物件來保存從資料庫的電影內容類型。

下列程式碼是一種 LINQ 查詢，其會從資料庫中擷取所有的內容類型。

[!code-csharp[Main](adding-search/samples/sample12.cs)]

程式碼會使用`AddRange`方法屬於泛型`List`將不同的內容類型加入至清單的集合。 (不含`Distinct`修飾詞，會加入重複的內容類型 — 例如，喜劇會加入兩次在我們的範例)。 程式碼再儲存清單中的內容類型`ViewBag.MovieGenre`物件。 儲存類別目錄資料 （這類電影內容類型的） 做為[SelectList](https://msdn.microsoft.cus/library/system.web.mvc.selectlist(v=vs.108).aspx)物件存放至`ViewBag`，則存取下拉式清單方塊中的類別目錄資料是一般 MVC 應用程式的方法。

下列程式碼示範如何檢查`movieGenre`參數。 如果它不是空的再進一步限制電影查詢，以限制選取的影片，以指定的內容類型。

[!code-csharp[Main](adding-search/samples/sample13.cs)]

如先前所述，查詢就不會執行以資料為基礎直到電影清單重複處理 (也就是在檢視中之後,`Index`動作方法傳回)。

## <a name="adding-markup-to-the-index-view-to-support-search-by-genre"></a>將標記加入至索引檢視，以支援搜尋的影片

新增`Html.DropDownList`協助專家*Views\Movies\Index.cshtml*檔案之前`TextBox`協助程式。 已完成的標記如下所示：

[!code-cshtml[Main](adding-search/samples/sample14.cshtml?highlight=11)]

下列程式碼：

[!code-cshtml[Main](adding-search/samples/sample15.cshtml)]

參數"MovieGenre 」 提供的索引鍵`DropDownList`helper 來尋找`IEnumerable<SelectListItem>`中`ViewBag`。 `ViewBag`擴展動作方法中：

[!code-csharp[Main](adding-search/samples/sample16.cs?highlight=10)]

"All"提供選項標籤參數。 如果您在瀏覽器中檢查該選項，您會看到其 「 值 」 屬性為空白。 因為我們控制器只會篩選`if`字串不是`null`或 empty，送出的值是空`movieGenre`顯示所有的內容類型。

您也可以設定預設會選取的選項。 如果您想"喜劇"做為預設選項，您需要變更控制器中的程式碼如下所示：

[!code-cshtml[Main](adding-search/samples/sample17.cshtml)]

執行應用程式，並瀏覽至*/電影/索引*。 內容類型、 電影名稱，以及這兩個準則，請嘗試搜尋。

![](adding-search/_static/image8.png)

這一節中建立和檢視，讓使用者可以搜尋電影標題和內容類型的搜尋動作方法。 在下一步 區段中，您會看到如何將屬性加入`Movie`模型以及如何將會自動建立測試資料庫的初始設定式。

>[!div class="step-by-step"]
[上一頁](examining-the-edit-methods-and-edit-view.md)
[下一頁](adding-a-new-field.md)
