<!--
[!code-html[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Shared/_Layout.cshtml?highlight=7,31)]


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_1stSearch)]

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?name=snippet_SearchNull)]

![Index view](../../tutorials/first-mvc-app/search/_static/ghost.png)


[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Startup.cs?highlight=5&name=snippet_1)]

--> 

<span data-ttu-id="269f4-101">前一個 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="269f4-101">The previous `Index` method:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_1stSearch)]

<span data-ttu-id="269f4-102">含有 `id` 參數的已更新 `Index` 方法：</span><span class="sxs-lookup"><span data-stu-id="269f4-102">The updated `Index` method with `id` parameter:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1,8&name=snippet_SearchID)]

<span data-ttu-id="269f4-103">您現在可以將搜尋標題作為路由資料 (URL 區段) 傳遞，而不是作為查詢字串值。</span><span class="sxs-lookup"><span data-stu-id="269f4-103">You can now pass the search title as route data (a URL segment) instead of as a query string value.</span></span>

![已將 ghost 一詞新增至 URL，而傳回的電影清單包含 Ghostbusters 和 Ghostbusters 2 兩個電影的 Index 檢視](../../tutorials/first-mvc-app/search/_static/g2.png)

<span data-ttu-id="269f4-105">但是，您不能期望使用者在每次想要搜尋電影時修改 URL。</span><span class="sxs-lookup"><span data-stu-id="269f4-105">However, you can't expect users to modify the URL every time they want to search for a movie.</span></span> <span data-ttu-id="269f4-106">因此，現在您將新增可協助他們篩選電影的 UI。</span><span class="sxs-lookup"><span data-stu-id="269f4-106">So now you'll add UI to help them filter movies.</span></span> <span data-ttu-id="269f4-107">如果您已變更 `Index` 方法的簽章來測試如何傳遞路由繫結的 `ID` 參數，請將其變更回採用一個名為 `searchString` 參數：</span><span class="sxs-lookup"><span data-stu-id="269f4-107">If you changed the signature of the `Index` method to test how to pass the route-bound `ID` parameter, change it back so that it takes a parameter named `searchString`:</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_1stSearch)]

<span data-ttu-id="269f4-108">開啟 *Views/Movies/Index.cshtml* 檔案，並新增下面強調顯示的 `<form>` 標記：</span><span class="sxs-lookup"><span data-stu-id="269f4-108">Open the *Views/Movies/Index.cshtml* file, and add the `<form>` markup highlighted below:</span></span>

[!code-HTML[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Views/Movies/IndexForm1.cshtml?highlight=10-16&range=4-21)]

<span data-ttu-id="269f4-109">HTML `<form>` 標記使用[表單標記協助程式](../../mvc/views/working-with-forms.md)，因此當您提交表單時，篩選條件字串會張貼至電影控制器的 `Index` 動作。</span><span class="sxs-lookup"><span data-stu-id="269f4-109">The HTML `<form>` tag uses the [Form Tag Helper](../../mvc/views/working-with-forms.md), so when you submit the form, the filter string is posted to the `Index` action of the movies controller.</span></span> <span data-ttu-id="269f4-110">儲存變更，然後測試篩選條件。</span><span class="sxs-lookup"><span data-stu-id="269f4-110">Save your changes and then test the filter.</span></span>

![已將 ghost 一詞輸入 [標題] 篩選條件文字方塊的 Index 檢視](../../tutorials/first-mvc-app/search/_static/filter.png)

<span data-ttu-id="269f4-112">沒有您可能期望的 `Index` 方法的 `[HttpPost]` 多載。</span><span class="sxs-lookup"><span data-stu-id="269f4-112">There's no `[HttpPost]` overload of the `Index` method as you might expect.</span></span> <span data-ttu-id="269f4-113">您不需要它，因為方法不會變更應用程式的狀態，而只會篩選資料。</span><span class="sxs-lookup"><span data-stu-id="269f4-113">You don't need it, because the method isn't changing the state of the app, just filtering data.</span></span>

<span data-ttu-id="269f4-114">您可以新增下列 `[HttpPost] Index` 方法。</span><span class="sxs-lookup"><span data-stu-id="269f4-114">You could add the following `[HttpPost] Index` method.</span></span>

[!code-csharp[Main](../../tutorials/first-mvc-app/start-mvc/sample/MvcMovie/Controllers/MoviesController.cs?highlight=1&name=snippet_SearchPost)]

<span data-ttu-id="269f4-115">`notUsed` 參數用來建立 `Index` 方法的多載。</span><span class="sxs-lookup"><span data-stu-id="269f4-115">The `notUsed` parameter is used to create an overload for the `Index` method.</span></span> <span data-ttu-id="269f4-116">我們稍後將在本教學課程中加以討論。</span><span class="sxs-lookup"><span data-stu-id="269f4-116">We'll talk about that later in the tutorial.</span></span>

<span data-ttu-id="269f4-117">如果您新增此方法，動作啟動程式會比對 `[HttpPost] Index` 方法，而 `[HttpPost] Index` 方法會如下列影像所示執行。</span><span class="sxs-lookup"><span data-stu-id="269f4-117">If you add this method, the action invoker would match the `[HttpPost] Index` method, and the `[HttpPost] Index` method would run as shown in the image below.</span></span>

![應用程式回應為 "From HttpPost Index: filter on ghost" 的瀏覽器視窗](../../tutorials/first-mvc-app/search/_static/fo.png)

<span data-ttu-id="269f4-119">不過，即使您新增這個 `[HttpPost]` 版本的 `Index` 方法，在如何全部實作此方法方面仍然有其限制。</span><span class="sxs-lookup"><span data-stu-id="269f4-119">However, even if you add this `[HttpPost]` version of the `Index` method, there's a limitation in how this has all been implemented.</span></span> <span data-ttu-id="269f4-120">假設您想要將特定的搜尋加為書籤，或者想要傳送連結給朋友，讓他們可以點選來查看相同的電影篩選清單。</span><span class="sxs-lookup"><span data-stu-id="269f4-120">Imagine that you want to bookmark a particular search or you want to send a link to friends that they can click in order to see the same filtered list of movies.</span></span> <span data-ttu-id="269f4-121">請注意，HTTP POST 要求的 URL 與 GET 要求的 URL (localhost:xxxxx/Movies/Index) 相同 -- 在 URL 中沒有搜尋資訊。</span><span class="sxs-lookup"><span data-stu-id="269f4-121">Notice that the URL for the HTTP POST request is the same as the URL for the GET request (localhost:xxxxx/Movies/Index) -- there's no search information in the URL.</span></span> <span data-ttu-id="269f4-122">搜尋字串資訊會以[表單欄位值](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data)的形式傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="269f4-122">The search string information is sent to the server as a [form field value](https://developer.mozilla.org/docs/Learn/HTML/Forms/Sending_and_retrieving_form_data).</span></span> <span data-ttu-id="269f4-123">您可以使用瀏覽器開發人員工具或絕佳的 [Fiddler 工具](http://www.telerik.com/fiddler)來進行確認。</span><span class="sxs-lookup"><span data-stu-id="269f4-123">You can verify that with the browser Developer tools or the excellent [Fiddler tool](http://www.telerik.com/fiddler).</span></span> <span data-ttu-id="269f4-124">下圖顯示 Chrome 瀏覽器開發人員工具：</span><span class="sxs-lookup"><span data-stu-id="269f4-124">The image below shows the Chrome browser Developer tools:</span></span>

![顯示 searchString 值為 ghost 之要求本文的 Microsoft Edge 開發人員工具的 [網路] 索引標籤](../../tutorials/first-mvc-app/search/_static/f12_rb.png)

<span data-ttu-id="269f4-126">您可以在要求本文中看到搜尋參數和 [XSRF](../../security/anti-request-forgery.md) 語彙基元。</span><span class="sxs-lookup"><span data-stu-id="269f4-126">You can see the search parameter and [XSRF](../../security/anti-request-forgery.md) token in the request body.</span></span> <span data-ttu-id="269f4-127">請注意，如先前的教學課程中所述，[表單標記協助程式](../../mvc/views/working-with-forms.md)會產生 [XSRF](../../security/anti-request-forgery.md) 防偽語彙基元。</span><span class="sxs-lookup"><span data-stu-id="269f4-127">Note, as mentioned in the previous tutorial, the [Form Tag Helper](../../mvc/views/working-with-forms.md) generates an [XSRF](../../security/anti-request-forgery.md) anti-forgery token.</span></span> <span data-ttu-id="269f4-128">我們不會修改資料，因此不需要驗證控制器方法中的語彙基元。</span><span class="sxs-lookup"><span data-stu-id="269f4-128">We're not modifying data, so we don't need to validate the token in the controller method.</span></span>

<span data-ttu-id="269f4-129">由於搜尋參數是在要求本文而不是 URL 中，因此您無法擷取該搜尋資訊以加為書籤或與其他人共用。</span><span class="sxs-lookup"><span data-stu-id="269f4-129">Because the search parameter is in the request body and not the URL, you can't capture that search information to bookmark or share with others.</span></span> <span data-ttu-id="269f4-130">藉由指定要求應該是 `HTTP GET`，即可修正此問題。</span><span class="sxs-lookup"><span data-stu-id="269f4-130">We'll fix this by specifying the request should be `HTTP GET`.</span></span>
