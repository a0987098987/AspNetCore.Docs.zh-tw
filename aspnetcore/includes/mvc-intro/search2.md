<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

前一個 `Index` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

含有 `id` 參數的已更新 `Index` 方法：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。

![已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩個電影的 Index 檢視](../../tutorials/first-mvc-app/search/_static/g2.png)

但是，您不能期望使用者在每次想要搜尋電影時修改 URL。 因此，現在您將新增可協助他們篩選電影的 UI。 如果您已變更 `Index` 方法的簽章來測試如何傳遞路由繫結的 `ID` 參數，請將其變更回採用一個名為 `searchString` 參數：

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

開啟 *Views/Movies/Index.cshtml* 檔案，並新增下面強調顯示的 `<form>` 標記：

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

HTML `<form>` 標記使用[表單標記協助程式](../../mvc/views/working-with-forms.md)，因此當您提交表單時，篩選條件字串會張貼至電影控制器的 `Index` 動作。 儲存變更，然後測試篩選條件。

![已將 ghost 一詞輸入 [標題] 篩選條件文字方塊的 Index 檢視](../../tutorials/first-mvc-app/search/_static/filter.png)

沒有您可能期望的 `Index` 方法的 `[HttpPost]` 多載。 您不需要它，因為方法不會變更應用程式的狀態，而只會篩選資料。

您可以新增下列 `[HttpPost] Index` 方法。

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

`notUsed` 參數用來建立 `Index` 方法的多載。 我們稍後將在本教學課程中加以討論。

如果您新增此方法，動作啟動程式會比對 `[HttpPost] Index` 方法，而 `[HttpPost] Index` 方法會如下列影像所示執行。

![應用程式回應為 "From HttpPost Index: filter on ghost" 的瀏覽器視窗](../../tutorials/first-mvc-app/search/_static/fo.png)

不過，即使您新增這個 `[HttpPost]` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。 假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。 請注意，HTTP POST 要求的 URL 與 GET 要求的 URL (localhost:xxxxx/Movies/Index) 相同 -- 在 URL 中沒有搜尋資訊。 搜尋字串資訊會以[表單欄位值](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)的形式傳送至伺服器。 您可以使用瀏覽器開發人員工具或絕佳的 [Fiddler 工具](http://www.telerik.com/fiddler)來進行確認。 下圖顯示 Chrome 瀏覽器開發人員工具：

![顯示 searchString 值為 ghost 之要求本文的 Microsoft Edge 開發人員工具的 [網路] 索引標籤](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

您可以在要求本文中看到搜尋參數和 [XSRF](../../security/anti-request-forgery.md) 語彙基元。 請注意，如先前的教學課程中所述，[表單標記協助程式](../../mvc/views/working-with-forms.md)會產生 [XSRF](../../security/anti-request-forgery.md) 防偽語彙基元。 我們不會修改資料，因此不需要驗證控制器方法中的語彙基元。

由於搜尋參數是在要求本文而不是 URL 中，因此您無法擷取該搜尋資訊以加為書籤或與其他人共用。 藉由指定要求應該是 `HTTP GET`，即可修正此問題。
